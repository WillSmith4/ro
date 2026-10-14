# feat: Add VarClean 

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#113](https://github.com/rooootdev/lara/pull/113)  
> **State:** 🟣 merged  
> **Branch:** `m1337v:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 17, 2026  
> **Changes:** +695 / -36 across 6 files  
> **Created:** Apr 17, 2026  
> **Updated:** Apr 17, 2026  
> **Closed:** Apr 17, 2026  

---

## Description

*Opened by [@m1337v](https://github.com/m1337v)* *on Apr 17, 2026*


This PR adds VarClean, a targeted cleanup utility for finding leftover jailbreak/tool residue under /var that can trigger false jailbreak-detection positives. Useful for previous upgrades/backup restores from jailbroken setups or after playing around with FilzaJailedDS.

It scans a curated ruleset of known residue paths and Filza-related bundle identifiers, shows the matched files before deletion, and only enables cleanup once sandbox access is available. 

<img width="1179" height="2556" alt="filza_varclean" src="https://github.com/user-attachments/assets/3a95ce52-7a71-4f11-9734-5194b4d5e860" />


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24560271308/job/71806979122) — **skipped** — *GitHub Actions*
- ✅ [build (arm64e)](https://github.com/rooootdev/lara/actions/runs/24560271308/job/71806886619) — **success** — *GitHub Actions*


---

## PR Commits (4)

- [`0c69a07`](https://github.com/rooootdev/lara/commit/0c69a0746c468533f4e0b4d032f0b3719e90e014) feat: add varclean support — *Morpheus* · Apr 12, 2026
- [`491dd2d`](https://github.com/rooootdev/lara/commit/491dd2d8a802c6ad34ccae17926146aaf7472920) chore: remove whitelist as it's not needed — *Morpheus* · Apr 12, 2026
- [`b168fd5`](https://github.com/rooootdev/lara/commit/b168fd5a58fbf0bfc29077ef51d5889451f3b2c5) Merge remote-tracking branch 'upstream/main' — *Morpheus* · Apr 17, 2026
- [`23f5234`](https://github.com/rooootdev/lara/commit/23f52346d7896c0e844450d9e58f3fbe31710136) fix: adjust regex — *Morpheus* · Apr 17, 2026


---

## Changed Files (6)

- 🟢 `lara/VarCleanBridge.h` (added) — +17 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,17 @@
+#import <Foundation/Foundation.h>
+
+NS_ASSUME_NONNULL_BEGIN
+
+@interface VarCleanBridge : NSObject
+
++ (NSDictionary *)loadRulesNamed:(NSString *)resourceName
+                        inBundle:(NSBundle *)bundle
+                           error:(NSError * _Nullable * _Nullable)error;
+
++ (BOOL)probePathExists:(NSString *)path
+            isDirectory:(BOOL *)isDirectory
+              isSymlink:(BOOL *)isSymlink;
+
+@end
+
+NS_ASSUME_NONNULL_END
  ```

  </details>
- 🟢 `lara/VarCleanBridge.m` (added) — +66 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,66 @@
+#import "VarCleanBridge.h"
+
+#include <sys/stat.h>
+#include <unistd.h>
+
+@implementation VarCleanBridge
+
++ (NSDictionary *)loadRulesNamed:(NSString *)resourceName
+                        inBundle:(NSBundle *)bundle
+                           error:(NSError * _Nullable * _Nullable)error {
+    NSString *jsonPath = [bundle pathForResource:resourceName ofType:@"json"];
+    if (jsonPath.length == 0) {
+        if (error) {
+            *error = [NSError errorWithDomain:@"VarCleanBridge"
+                                         code:1
+                                     userInfo:@{NSLocalizedDescriptionKey: @"Missing VarClean rules resource"}];
+        }
+        return @{};
+    }
+
+    NSData *jsonData = [NSData dataWithContentsOfFile:jsonPath options:0 error:error];
+    if (!jsonData) {
+        return @{};
+    }
+
+    id rules = [NSJSONSerialization JSONObjectWithData:jsonData
+                                               options:NSJSONReadingMutableContainers
+                                                 error:error];
+    if (![rules isKindOfClass:NSDictionary.class]) {
+        if (error && !*error) {
+            *error = [NSError errorWithDomain:@"VarCleanBridge"
+                                         code:2
+                                     userInfo:@{NSLocalizedDescriptionKey: @"Invalid VarClean rules format"}];
+        }
+        return @{};
+    }
+
+    return rules;
+}
+
++ (BOOL)probePathExists:(NSString *)path
+            isDirectory:(BOOL *)isDirectory
+              isSymlink:(BOOL *)isSymlink {
+    if (isDirectory) *isDirectory = NO;
+    if (isSymlink) *isSymlink = NO;
+    if (path.length == 0) {
+        return NO;
+    }
+
+    struct stat st = {0};
+    if (lstat(path.fileSystemRepresentation, &st) == 0) {
+        if (isSymlink) *isSymlink = S_ISLNK(st.st_mode);
+        if (isDirectory) *isDirectory = S_ISDIR(st.st_mode);
+        return YES;
+    }
+
+    BOOL directory = NO;
+    if ([[NSFileManager defaultManager] fileExistsAtPath:path isDirectory:&directory]) {
+        if (isDirectory) *isDirectory = directory;
+        return YES;
+    }
+
+    return access(path.fileSystemRepresentation, F_OK) == 0;
+}
+
+@end
  ```

  </details>
- 🟢 `lara/VarCleanRules.json` (added) — +275 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,275 @@
+{
+  "/var/tmp": {
+    "blacklist": []
+  },
+  "/var/mobile/Library/UserConfigurationProfiles/PublicInfo/": {
+    "blacklist": [
+      "Flex3Patches.plist"
+    ]
+  },
+  "/var/mobile/Library/SplashBoard/Snapshots": {
+    "blacklist": [
+      "com.roothide.manager",
+      "com.opa334.Dopamine.roothide",
+      "com.opa334.Dopamine",
+      {
+        "match": "regexp",
+        "name": "^com\\.tigisoftware\\."
+      },
+      "com.xina.jailbreak",
+      "org.coolstar.SileoStore",
+      "ru.domo.cocoatop64",
+      "ws.hbang.Terminal",
+      "xyz.willy.Zebra",
+      "com.apple.Terminal"
+    ]
+  },
+  "/var/mobile/Library/Application Support/Containers": {
+    "blacklist": [
+      "xyz.willy.Zebra",
+      {
+        "match": "regexp",
+        "name": "^com\\.tigisoftware\\."
+      },
+      "org.coolstar.SileoStore",
+      "com.apple.Terminal"
+    ]
+  },
+  "/var/containers/Bundle": {
+    "blacklist": [
+      "dylib",
+      "xina"
+    ]
+  },
+  "/var/mobile/Library/Cookies": {
+    "blacklist": [
+      "com.johncoates.Flex.binarycookies"
+    ]
+  },
+  "/var/root/Library/Caches": {
+    "blacklist": [
+      "shshd"
+    ]
+  },
+  "/var/mobile/Library/Logs": {
+    "blacklist": [
+      "Cydia"
+    ]
+  },
+  "/var/mobile/Library/Preferences": {
+    "blacklist": [
+      "com.roothide.manager.plist",
+      "com.opa334.Dopamine.roothide.plist",
+      "com.opa334.Dopamine.plist",
+      {
+        "match": "regexp",
+        "name": "^com\\.tigisoftware\\."
+      },
+      "com.xina.jailbreak.plist",
+      "org.coolstar.SileoStore.plist",
+      "ru.domo.cocoatop64.plist",
+      "ws.hbang.Terminal.plist",
+      "xyz.willy.Zebra.plist",
+      "com.apple.lockscreencache-new.plist",
+      "com.apple.homescreencache-new.plist",
+      "com.apple.terminal.plist"
+    ]
+  },
+  "/var/mobile/Library/Caches": {
+    "blacklist": [
+      "ImageTables",
+      "SentryCrash",
+      "io.sentry",
+      "com.hackemist.SDImageCache",
+      "com.opa334.Dopamine",
+      {
+        "match": "regexp",
+        "name": "^com\\.tigisoftware\\."
+      },
+      "org.coolstar.SileoStore",
+      "ws.hbang.Terminal",
+      "xyz.willy.Zebra",
+      "Cephei",
+      "com.apple.Terminal"
+    ]
+  },
+  "/var": {
+    "blacklist": [
+      "stash",
+      "alternatives",
+      "ap",
+      "apt",
+      "bin",
+      "bzip2",
+      "cache",
+      "dpkg",
+      "etc",
+      "gzip",
+      "local",
+      "lib",
+      "Lib",
+      "libexec",
+      "Library",
+      "LIY",
+      "Liy",
+      "newuser",
+      "profile",
+      "sbin",
+      "sh",
+      "share",
+      "ssh",
+      "sudo_logsrvd.conf",
+      "suid_profile",
+      "sy",
+      "usr",
+      "zlogin",
+      "zlogout",
+      "zprofile",
+      "zshenv",
+      "zshrc",
+      "master.passwd",
+      ".keep_symlinks"
+    ]
+  },
+  "/var/mobile/Library": {
+    "blacklist": [
+      "Sileo",
+      "Filza",
+      "Flex3",
+      "SBSettings",
+      "RootHide",
+      "iCleaner",
+      "Terminal",
+      "varClean",
+      "ADManager"
+    ]
+  },
+  "/var/mobile/Library/Saved Application State": {
+    "blacklist": [
+      "com.opa334.Dopamine.savedState",
+      {
+        "match": "regexp",
+        "name": "^com\\.tigisoftware\\."
+      },
+      "org.coolstar.SileoStore.savedState",
+      "ws.hbang.Terminal.savedState",
+      "xyz.willy.Zebra.savedState",
+      "ru.domo.cocoatop64.savedState",
+      "com.apple.Terminal.savedState"
+    ]
+  },
+  "/var/mobile/Library/WebKit": {
+    "blacklist": [
+      "xyz.willy.Zebra",
+      "com.ivanobilenchi.icleaner"
+    ]
+  },
+  "/var/mobile": {
+    "blacklist": [
+      "log.txt",
+      ".ekenablelogging",
+      ".eksafemode",
+      ".serotonin_hidetext",
+      ".TrollApps",
+      "Serotonin.jp2"
+    ]
+  },
+  "/var/mobile/Library/Application Support": {
+    "blacklist": [
+      "xyz.willy.Zebra",
+      "com.apple.Terminal"
+    ]
+  },
+  "/var/mobile/Media": {
+    "blacklist": [
+      {
+        "match": "include",
+        "name": "Auto"
+      },
+      {
+        "match": "include",
+        "name": "Touch"
+      },
+      {
+        "match": "include",
+        "name": "Theme"
+      },
+      {
+        "match": "include",
+        "name": "Script"
+      },
+      "Cydia",
+      "CatScript",
+      "TrollRecorder",
+      ".evasi0n7_installed",
+      ".loli",
+      "blackb0x.log",
+      "Divise",
+      ".bootstrapped_electraRemover",
+      "install",
+      "log.txt",
+      "p0laris",
+      ".Trash",
+      "spirit",
+      "TouchSprite",
+      ".bootstrapped_Th0r_remover",
+      "AppRank",
+      "KDTScript",
+      "NeonStaticClockIcon.png",
+      "Erika"
+    ]
+  },
+  "/var/mobile/Library/HTTPStorages": {
+    "blacklist": [
+      "com.opa334.Dopamine",
+      {
+        "match": "regexp",
+        "name": "^com\\.tigisoftware\\."
+      },
+      "org.coolstar.SileoStore",
+      "ws.hbang.Terminal",
+      "xyz.willy.Zebra"
+    ]
+  },
+  "/var/MobileSoftwareUpdate/mnt1": {
+    "blacklist": []
+  },
+  "/var/root/Library/Preferences": {
+    "blacklist": [
+      "com.xina.jailbreak.plist",
+      "com.xina.blacklist.plist",
+      "ws.hbang.Terminal.plist"
+    ]
+  },
+  "/var/root/Library/Application Support": {
+    "blacklist": []
+  },
+  "/var/lib": {
+    "blacklist": [
+      "filza"
+    ]
+  },
+  "/var/mobile/Documents": {
+    "blacklist": [
+      "Flex",
+      "DumpDecrypter",
+      ".DynamicCowBackups"
+    ]
+  },
+  "/var/log": {
+    "blacklist": []
+  },
+  "/var/root": {
+    "blacklist": [
+      ".bash_history",
+      ".config",
+      ".local"
+    ]
+  },
+  "/var/root/Library/HTTPStorages": {
+    "blacklist": [
+      "shshd",
+      "FilzaHelper"
+    ]
+  }
+}
  ```

  </details>
- 🟡 `lara/lara-Bridging-Header.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -11,6 +11,7 @@
 #import "apfs.h"
 #import "vfs.h"
 #import "sbx.h"
+#import "VarCleanBridge.h"
 
 // TaskRop / RemoteCall
 #import "kexploit/TaskRop/RemoteCall.h"
  ```

  </details>
- 🟡 `lara/views/ContentView.swift` (modified) — +44 / -36
  <details><summary>Diff</summary>

  ```diff
  @@ -14,7 +14,7 @@ struct ContentView: View {
     @State private var hasoffsets = haskernproc()
     @State private var showsettings = false
     @State private var selectedmethod: method = .hybrid
-    
+
     var body: some View {
         NavigationStack {
             List {
@@ -63,23 +63,23 @@ struct ContentView: View {
                         }
                         .disabled(mgr.dsrunning)
                         .disabled(mgr.dsready)
-                        
+
                         HStack {
                             Text("kernproc:")
                             Spacer()
                             Text(String(format: "0x%llx", getrootvnode()))
                                 .font(.system(.body, design: .monospaced))
                                 .foregroundColor(.secondary)
                         }
-                        
+
                         HStack {
                             Text("rootvnode:")
                             Spacer()
                             Text(String(format: "0x%llx", getkernproc()))
                                 .font(.system(.body, design: .monospaced))
                                 .foregroundColor(.secondary)
                         }
-                        
+
                         if mgr.dsready {
                             HStack {
                                 Text("kernel_base:")
@@ -88,7 +88,7 @@ struct ContentView: View {
                                     .font(.system(.body, design: .monospaced))
                                     .foregroundColor(.secondary)
                             }
-                            
+
                             HStack {
                                 Text("kernel_slide:")
                                 Spacer()
@@ -140,26 +140,26 @@ struct ContentView: View {
                                 }
                             }
                             .disabled(!mgr.dsready || mgr.vfsready || mgr.vfsrunning)
-                            
+
                             if mgr.vfsready {
                                 NavigationLink("Tweaks") {
                                     List {
                                         NavigationLink("Font Overwrite") {
                                             FontPicker(mgr: mgr)
                                         }
-                                        
+
                                         NavigationLink("Card Overwrite") {
                                             CardView()
                                         }
-                                        
+
                                         NavigationLink("Custom Overwrite") {
                                             CustomView(mgr: mgr)
                                         }
-                                        
+
                                         NavigationLink("DirtyZero (Broken)") {
                                             ZeroView(mgr: mgr)
                                         }
-                                        
+
                                         if !showfmintabs {
                                             NavigationLink("File Manager") {
                                                 SantanderView(startPath: "/")
@@ -202,7 +202,7 @@ struct ContentView: View {
                                 }
                             }
                             .disabled(!mgr.dsready || mgr.sbxready || mgr.sbxrunning)
-                            
+
                             if mgr.sbxready {
                                 NavigationLink("Tweaks") {
                                     List {
@@ -211,24 +211,28 @@ struct ContentView: View {
                                                 SantanderView(startPath: "/")
                                             }
                                         }
-                                        
+
                                         NavigationLink("Card Overwrite") {
                                             CardView()
                                         }
-                                        
+
                                         NavigationLink("3 App Bypass") {
                                             AppsView(mgr: mgr)
                                         }
-                                        
+
+                                        NavigationLink("VarClean") {
+                                            VarCleanView()
+                                        }
+
                                         NavigationLink("Unblacklist (Broken?)") {
                                             WhitelistView()
                                         }
-                                        
+
                                         if 1 == 2 {
                                             NavigationLink("MobileGestalt") {
                                                 EditorView()
                                             }
-                                            
+
                                             NavigationLink("Passcode Theme") {
                                                 PasscodeView(mgr: mgr)
                                             }
@@ -305,7 +309,7 @@ struct ContentView: View {
                                 }
                                 .disabled(!mgr.dsready || mgr.vfsready || mgr.vfsrunning)
                             }
-                            
+
                             if mgr.vfsready && mgr.sbxready {
                                 NavigationLink("Tweaks") {
                                     List {
@@ -314,44 +318,48 @@ struct ContentView: View {
                                                 SantanderView(startPath: "/")
                                             }
                                         }
-                                        
+
                                         NavigationLink("Font Overwrite") {
                                             FontPicker(mgr: mgr)
                                         }
-                                        
+
                                         NavigationLink("Card Overwrite") {
                                             CardView()
                                         }
-                                        
+
                                         NavigationLink("Custom Overwrite") {
                                             CustomView(mgr: mgr)
                                         }
-                                        
+
                                         NavigationLink("MobileGestalt") {
                                             EditorView()
                                         }
-                                        
+
                                         NavigationLink("3 App Bypass") {
                                             AppsView(mgr: mgr)
-                                        } 
-                                        
+                                        }
+
+                                        NavigationLink("VarClean") {
+                                            VarCleanView()
+                                        }
+
                                         NavigationLink("Whitelist") {
                                             WhitelistView()
                                         }
-                                        
+
                                         NavigationLink("DirtyZero") {
                                             ZeroView(mgr: mgr)
                                         }
-                                        
+
                                         if 1 == 2 {
                                             NavigationLink("Control Center") {
                                                 CCView()
                                             }
-                                            
+
                                             NavigationLink("Passcode Theme") {
                                                 PasscodeView(mgr: mgr)
                                             }
-                                            
+
                                             NavigationLink("3 App Bypass") {
                                                 AppsView(mgr: mgr)
                                             }
@@ -368,7 +376,7 @@ struct ContentView: View {
                             Text("Font Overwrite is only available in VFS or Hybrid mode. (Settings -> Method -> VFS/Hybrid)")
                         }
                     }
-                    
+
                     #if !DISABLE_REMOTECALL
                     Section {
                         Button("Init RemoteCall") {
@@ -384,12 +392,12 @@ struct ContentView: View {
                             }
                         }
                         .disabled(!mgr.dsready || mgr.remotecallrunning)
-                        
+
                         if mgr.remotecallrunning {
                             NavigationLink("Tweaks") {
                                 RemoteView(mgr: mgr)
                             }
-                            
+
                             Button("Destroy RemoteCall") {
                                 mgr.rcdestroy()
                             }
@@ -404,18 +412,18 @@ struct ContentView: View {
                     }
                     .disabled(isdebugged())
                     #endif
-                    
+
                     Section {
                         if mgr.dsready {
                             NavigationLink("Tools") {
                                 ToolsView()
                             }
                         }
-            
+
                         Button("Respring") {
                             mgr.respring()
                         }
-                        
+
                         Button("Panic!") {
                             mgr.panic()
                         }
@@ -424,7 +432,7 @@ struct ContentView: View {
                         Text("Other")
                     }
                 }
-                
+
             }
             .navigationTitle("lara")
             .toolbar {
@@ -447,7 +455,7 @@ struct ContentView: View {
             refreshselectedmethod()
         }
     }
-    
+
     private func refreshselectedmethod() {
         if let raw = UserDefaults.standard.string(forKey: "selectedmethod"),
            let m = method(rawValue: raw) {
  ```

  </details>
- 🟢 `lara/views/VarCleanView.swift` (added) — +292 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,292 @@
+//
+//  VarCleanView.swift
+//  lara
+//
+
+import SwiftUI
+
+private struct VarCleanMatch: Identifiable, Hashable {
+    let path: String
+    let name: String
+    let isDirectory: Bool
+    let isSymlink: Bool
+    var isSelected: Bool
+
+    var id: String { path }
+}
+
+private struct VarCleanGroup: Identifiable, Hashable {
+    let path: String
+    var items: [VarCleanMatch]
+
+    var id: String { path }
+}
+
+struct VarCleanView: View {
+    @ObservedObject private var mgr = laramgr.shared
+    @State private var groups: [VarCleanGroup] = []
+    @State private var isRefreshing = false
+    @State private var isDeleting = false
+    @State private var statusMessage: String?
+    @State private var showDeleteConfirmation = false
+
+    private var cleanupAvailable: Bool { mgr.sbxready }
+
+    private var selectedCount: Int {
+        groups.reduce(0) { $0 + $1.items.filter(\.isSelected).count }
+    }
+
+    var body: some View {
+        List {
+            Section("Status") {
+                Text(cleanupAvailable
+                     ? "Cleanup enabled via sandbox escape."
+                     : "Detection only. Escape the sandbox to delete matched paths.")
+                    .foregroundColor(.secondary)
+
+                if let statusMessage, !statusMessage.isEmpty {
+                    Text(statusMessage)
+                        .foregroundColor(.secondary)
+                        .font(.footnote)
+                }
+            }
+
+            if groups.isEmpty && !isRefreshing {
+                Section("Matches") {
+                    Text("No blacklisted residue from VarCleanRules.json was found.")
+                        .foregroundColor(.secondary)
+                }
+            } else {
+                ForEach(groups.indices, id: \.self) { groupIndex in
+                    Section(groups[groupIndex].path) {
+                        ForEach(groups[groupIndex].items.indices, id: \.self) { itemIndex in
+                            let item = groups[groupIndex].items[itemIndex]
+                            Button {
+                                guard cleanupAvailable else { return }
+                                groups[groupIndex].items[itemIndex].isSelected.toggle()
+                            } label: {
+                                HStack(spacing: 12) {
+                                    Text(item.isSymlink ? "🔗" : (item.isDirectory ? "🗂️" : "📄"))
+                                    VStack(alignment: .leading, spacing: 2) {
+                                        Text(item.name)
+                                            .foregroundColor(.primary)
+                                        Text(item.path)
+                                            .font(.caption)
+                                            .foregroundColor(.secondary)
+                                            .lineLimit(2)
+                                    }
+                                    Spacer()
+                                    if cleanupAvailable {
+                                        Image(systemName: item.isSelected ? "checkmark.circle.fill" : "circle")
+                                            .foregroundColor(item.isSelected ? .red : .secondary)
+                                    }
+                                }
+                            }
+                            .buttonStyle(.plain)
+                            .disabled(!cleanupAvailable)
+                        }
+                    }
+                }
+            }
+        }
+        .navigationTitle("VarClean")
+        .toolbar {
+            ToolbarItemGroup(placement: .topBarTrailing) {
+                Button {
+                    Task { await refresh() }
+                } label: {
+                    if isRefreshing {
+                        ProgressView()
+                    } else {
+                        Image(systemName: "arrow.clockwise")
+                    }
+                }
+                .disabled(isRefreshing || isDeleting)
+
+                Button(selectedCount == 0 ? "Select All" : "Clear") {
+                    toggleSelection()
+                }
+                .disabled(!cleanupAvailable || groups.isEmpty || isDeleting)
+
+                Button("Clean") {
+                    showDeleteConfirmation = true
+                }
+                .disabled(!cleanupAvailable || selectedCount == 0 || isDeleting)
+            }
+        }
+        .task {
+            await refresh()
+        }
+        .alert("Delete Selected Items?", isPresented: $showDeleteConfirmation) {
+            Button("Cancel", role: .cancel) {}
+            Button("Delete", role: .destructive) {
+                Task { await deleteSelected() }
+            }
+        } message: {
+            Text("Delete \(selectedCount) matched path\(selectedCount == 1 ? "" : "s")?")
+        }
+    }
+
+    private func toggleSelection() {
+        let shouldSelect = selectedCount == 0
+        for groupIndex in groups.indices {
+            for itemIndex in groups[groupIndex].items.indices {
+                groups[groupIndex].items[itemIndex].isSelected = shouldSelect
+            }
+        }
+    }
+
+    @MainActor
+    private func refresh() async {
+        guard !isRefreshing else { return }
+        isRefreshing = true
+        defer { isRefreshing = false }
+
+        let newGroups = await Task.detached(priority: .userInitiated) {
+            loadVarCleanGroups()
+        }.value
+
+        groups = newGroups
+        if groups.isEmpty {
+            statusMessage = nil
+        } else {
+            let matchCount = groups.reduce(0) { $0 + $1.items.count }
+            statusMessage = "Found \(matchCount) matched path\(matchCount == 1 ? "" : "s")."
+        }
+    }
+
+    @MainActor
+    private func deleteSelected() async {
+        guard cleanupAvailable else { return }
+        isDeleting = true
+        defer { isDeleting = false }
+
+        let selectedPaths = groups
+            .flatMap(\.items)
+            .filter(\.isSelected)
+            .map(\.path)
+            .sorted { $0.count > $1.count }
+
+        var deletedCount = 0
+        var failures: [String] = []
+        let fileManager = FileManager.default
+
+        for path in selectedPaths {
+            do {
+                if fileManager.fileExists(atPath: path) {
+                    try fileManager.removeItem(atPath: path)
+                }
+                deletedCount += 1
+            } catch {
+                failures.append("\(path): \(error.localizedDescription)")
+            }
+        }
+
+        if failures.isEmpty {
+            statusMessage = "Deleted \(deletedCount) path\(deletedCount == 1 ? "" : "s")."
+        } else {
+            statusMessage = "Deleted \(deletedCount) path\(deletedCount == 1 ? "" : "s"), failed \(failures.count)."
+        }
+
+        await refresh()
+    }
+}
+
+private func loadVarCleanGroups() -> [VarCleanGroup] {
+    var error: NSError?
+    guard let rules = VarCleanBridge.loadRulesNamed("VarCleanRules", in: .main, error: &error) as? [String: Any] else {
+        return []
+    }
+
+    var grouped: [String: [VarCleanMatch]] = [:]
+    var seenPaths = Set<String>()
+    var directoryEntriesCache: [String: [String]] = [:]
+    let sortedRulePaths = rules.keys.sorted { $0.localizedCaseInsensitiveCompare($1) == .orderedAscending }
+
+    for basePath in sortedRulePaths {
+        guard let rule = rules[basePath] as? [String: Any],
+              let blacklist = rule["blacklist"] as? [Any] else {
+            continue
+        }
+
+        for probePath in probePaths(
+            for: basePath,
+            blacklist: blacklist,
+            seenPaths: &seenPaths,
+            directoryEntriesCache: &directoryEntriesCache
+        ) {
+            var isDirectory = ObjCBool(false)
+            var isSymlink = ObjCBool(false)
+            guard VarCleanBridge.probePathExists(probePath, isDirectory: &isDirectory, isSymlink: &isSymlink) else {
+                continue
+            }
+
+            let groupPath = (probePath as NSString).deletingLastPathComponent.isEmpty
+                ? "/"
+                : (probePath as NSString).deletingLastPathComponent
+
+            let match = VarCleanMatch(
+                path: probePath,
+                name: (probePath as NSString).lastPathComponent,
+                isDirectory: isDirectory.boolValue,
+                isSymlink: isSymlink.boolValue,
+                isSelected: false
+            )
+            grouped[groupPath, default: []].append(match)
+        }
+    }
+
+    let sortedGroups = grouped.keys.sorted { $0.localizedCaseInsensitiveCompare($1) == .orderedAscending }
+    return sortedGroups.map { groupPath in
+        let items = (grouped[groupPath] ?? [])
+            .sorted { $0.name.localizedCaseInsensitiveCompare($1.name) == .orderedAscending }
+        return VarCleanGroup(path: groupPath, items: items)
+    }
+}
+
+private func probePaths(
+    for basePath: String,
+    blacklist: [Any],
+    seenPaths: inout Set<String>,
+    directoryEntriesCache: inout [String: [String]]
+) -> [String] {
+    var probePaths: [String] = []
+
+    for entry in blacklist {
+        if let name = entry as? String, !name.isEmpty {
+            let probePath = (basePath as NSString).appendingPathComponent(name)
+            if seenPaths.insert(probePath).inserted {
+                probePaths.append(probePath)
+            }
+            continue
+        }
+
+        guard let condition = entry as? [String: Any],
+              let match = condition["match"] as? String,
+              match == "regexp",
+              let pattern = condition["name"] as? String,
+              let regex = try? NSRegularExpression(pattern: pattern) else {
+            continue
+        }
+
+        let entries = directoryEntries(atPath: basePath, cache: &directoryEntriesCache)
+        for name in entries where regex.firstMatch(in: name, range: NSRange(name.startIndex..., in: name)) != nil {
+            let probePath = (basePath as NSString).appendingPathComponent(name)
+            if seenPaths.insert(probePath).inserted {
+                probePaths.append(probePath)
+            }
+        }
+    }
+
+    return probePaths
+}
+
+private func directoryEntries(atPath path: String, cache: inout [String: [String]]) -> [String] {
+    if let cached = cache[path] {
+        return cached
+    }
+
+    let entries = (try? FileManager.default.contentsOfDirectory(atPath: path)) ?? []
+    cache[path] = entries
+    return entries
+}
  ```

  </details>


---

## References

### Commits (1)

- [c41887d](https://github.com/danosito/lara-m-chips-fix/commit/c41887d72358ded7db461e118db1f35844f83fda) — Merge pull request #113 from m1337v/main


---

## Timeline (Verbose)

- : 📝 Commit 0c69a07: feat: add varclean support
- : 📝 Commit 491dd2d: chore: remove whitelist as it's not needed
- : 📝 Commit b168fd5: Merge remote-tracking branch 'upstream/main'
- : 📝 Commit 23f5234: fix: adjust regex
- Apr 17, 2026: 🟣 @rooootdev merged
- Apr 17, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit c41887d — Merge pull request #113 from m1337v/main


---

*Exported on 10/14/2026, 2:55:25 PM from [https://github.com/rooootdev/lara/pull/113](https://github.com/rooootdev/lara/pull/113)*
