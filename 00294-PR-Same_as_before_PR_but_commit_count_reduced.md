# Same as before PR but commit count reduced

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#294](https://github.com/rooootdev/lara/pull/294)  
> **State:** 🟢 open  
> **Branch:** `notthemystery:main` → `rooootdev:main`  
> **Changes:** +917 / -3 across 9 files  
> **Created:** Jun 10, 2026  
> **Updated:** Jun 29, 2026  

---

## Description

*Opened by [@notthemystery](https://github.com/notthemystery)* *on Jun 10, 2026*


Same as before PR but commit count reduced


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (16)

- [`6f6a876`](https://github.com/rooootdev/lara/commit/6f6a876912c962243c4af7c948d073c94fd5786e) Update sbx.h — *The Mystery* · Jun 10, 2026
- [`5dedefd`](https://github.com/rooootdev/lara/commit/5dedefdfd076c21a6f386a6d652606227b82f7b0) Update utils.m — *The Mystery* · Jun 10, 2026
- [`9e0a60b`](https://github.com/rooootdev/lara/commit/9e0a60bd42ddd66f7229e0334b71598d8bf5c853) Update utils.h — *The Mystery* · Jun 10, 2026
- [`d8ecadb`](https://github.com/rooootdev/lara/commit/d8ecadb8748d76d522106628533ded9b8faa1737) Add files via upload — *The Mystery* · Jun 10, 2026
- [`78d9af1`](https://github.com/rooootdev/lara/commit/78d9af1633a33dbe2d579c0c77283111f55b70a7) Rename lara/views/tweaks/BundleResolver 2.swift to lara/views/tweaks/cacheclean/BundleResolver.swift — *The Mystery* · Jun 10, 2026
- [`9ffc7df`](https://github.com/rooootdev/lara/commit/9ffc7df21817326da3d766941a80c9c712b63f23) Rename lara/views/tweaks/CacheView.swift to lara/views/tweaks/cleancache/CacheView.swift — *The Mystery* · Jun 10, 2026
- [`3ad2014`](https://github.com/rooootdev/lara/commit/3ad20142579e35902fc5dd1caafa7bcad0d62a6b) Rename CacheView.swift to CacheView.swift — *The Mystery* · Jun 10, 2026
- [`17bf225`](https://github.com/rooootdev/lara/commit/17bf225fb454181e68b5666952e251d55f17a67b) Update ToolsView.swift — *The Mystery* · Jun 10, 2026
- [`f2ba6b8`](https://github.com/rooootdev/lara/commit/f2ba6b8f03ec04b01d1511b38ae4a5f987daa36b) Update TweaksView.swift — *The Mystery* · Jun 10, 2026
- [`964510f`](https://github.com/rooootdev/lara/commit/964510fc555534c1c76314ad79cfe301f307dc84) Update ToolsView.swift — *The Mystery* · Jun 10, 2026
- [`668bd71`](https://github.com/rooootdev/lara/commit/668bd7110a03581f0f86686dbbf210d18f624ab7) Wrong file corrected — *The Mystery* · Jun 10, 2026
- [`62bb76c`](https://github.com/rooootdev/lara/commit/62bb76c5ac0da3f6d796330e59d5488ef83cf91d) Update README.md — *The Mystery* · Jun 10, 2026
- [`04cd314`](https://github.com/rooootdev/lara/commit/04cd314fe62c8b1d8ca078d54d0a29b1deb29587) Create RecordView.swift — *The Mystery* · Jun 17, 2026
- [`4037c21`](https://github.com/rooootdev/lara/commit/4037c21912b21696258031ae503a2132cad553f2) Update TweaksView.swift — *The Mystery* · Jun 17, 2026
- [`5e152ae`](https://github.com/rooootdev/lara/commit/5e152aed0e6e99dbbdd972d77c6fcf8a54306c3e) Update RecordView.swift — *The Mystery* · Jun 17, 2026
- [`f8e1f9b`](https://github.com/rooootdev/lara/commit/f8e1f9b63592f5bff1493402150a3f4641dfa7fa) Remove 'This Call is being Recorded' Sound — *The Mystery* · Jun 17, 2026


---

## Changed Files (9)

- 🟡 `README.md` (modified) — +5 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -25,6 +25,9 @@
   <a href="https://github.com/rooootdev/lara/actions">
     <img src="https://img.shields.io/github/actions/workflow/status/rooootdev/lara/build.yml?branch=main&style=flat&logo=github" alt="GitHub Actions">
   </a>
+  <a href="https://laraapp.netlify.app">
+    <img src="https://img.shields.io/badge/Website-blue" alt="website">
+  </a>
 </p>
 
 <p align="center">
@@ -91,7 +94,8 @@ Important Notes:
 - OTA Update Disabler
 - Screen Time Disabler 
 - App Decrypt
-
+- Clean Cache
+- Remove "This Call is being Recorded" Sound
 
 ### Coming Soon
 - FTP Server
  ```

  </details>
- 🟡 `lara/kexploit/pe/sbx.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -10,6 +10,7 @@
 
 #include <stdint.h>
 
+uint64_t sbx_ucredbyproc(uint64_t proc);
 int sbx_escape(uint64_t self_proc);
 void sbx_setlogcallback(void (*callback)(const char *message));
 uint64_t sbx_gettoken(pid_t pid);
  ```

  </details>
- 🟡 `lara/kexploit/utils.h` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -68,6 +68,8 @@ uint64_t proc_self(void);
 uint64_t task_self(void);
 
 int crashproc(const char* pid);
+int proc_pause_resume(const char *name, bool resume);
+int count_pids(uint64_t allproc);
 
 #ifdef __cplusplus
 }
  ```

  </details>
- 🟡 `lara/kexploit/utils.m` (modified) — +47 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -853,3 +853,50 @@ int crashproc(const char* name) {
     ds_kwrite64(state + offsetof(struct arm_saved_state64, sp), 0x1337133713371337);
     return 0;
 }
+
+int proc_pause_resume(const char *name, bool resume) {
+    if (!name) {
+        return -1;
+    }
+
+    uint64_t proc = procbyname(name);
+    if (!proc) {
+        printf("(signal) process not found: %s\n", name);
+        return -1;
+    }
+
+    uint32_t pid = ds_kread32(proc + PROC_PID_OFFSET);
+    int result;
+    
+    if (resume) {
+        result = kill(pid, SIGCONT);
+    } else {
+        result = kill(pid, SIGSTOP);
+    }
+
+    if (result != 0) {
+        perror("(signal) kill failed");
+        return -1;
+    }
+
+    printf("(signal) %s %s\n",
+           name,
+           resume ? "resumed" : "paused");
+    return 0;
+}
+
+int count_pids(uint64_t allproc) {
+    int count = 0;
+    uint64_t proc = allproc;
+    
+    for (int i = 0; i < 12000 && proc; i++) {
+        int pid = ds_kread32(proc + off_proc_p_pid);
+        if (pid > 0 && pid < 99999)
+            count++;
+        uint64_t next = ds_kread64(proc + off_proc_p_list_le_next);
+        if (next == 0 || next == proc)
+            break;
+        proc = next;
+    }
+    return count;
+}
  ```

  </details>
- 🟢 `lara/views/tweaks/RecordView.swift` (added) — +251 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,251 @@
+import SwiftUI
+import UIKit
+import UniformTypeIdentifiers
+
+struct RecordView: View {
+    @ObservedObject var mgr: laramgr
+
+    @State private var disabled = false
+    @State private var isOverwriting = false
+
+    private let target1 = "/var/mobile/Library/CallServices/Greetings/default/StartDisclosureWithTone.m4a"
+    private let target2 = "/var/mobile/Library/CallServices/Greetings/default/StopDisclosure.caf"
+
+    // MARK: - Remote Download URLs
+    private let remote1 = URL(string:
+        "https://github.com/YangJiiii/Disable-Call-Recording-BookRestore-/raw/refs/heads/main/Sounds/StartDisclosureWithTone.m4a"
+    )!
+
+    private let remote2 = URL(string:
+        "https://github.com/YangJiiii/Disable-Call-Recording-BookRestore-/raw/refs/heads/main/Sounds/StopDisclosure.caf"
+    )!
+
+    var body: some View {
+        List {
+
+            Section(header: HeaderLabel(text: "Status", icon: "info.circle")) {
+                HStack {
+                    Text("Status")
+                    Spacer()
+
+                    Text(disabled ? "Disabled" : "Enabled")
+                        .foregroundColor(disabled ? .red : .green)
+                        .monospaced()
+                }
+            }
+
+            Section(header: HeaderLabel(text: "Actions", icon: "hammer")) {
+
+                Button("Disable") {
+                    disableRecordNotify()
+                }
+                .disabled(disabled || isOverwriting)
+
+                Button("Enable") {
+                    enableRecordNotify()
+                }
+                .disabled(isOverwriting)
+            }
+
+            // MARK: - NEW DOWNLOAD SECTION
+            Section(header: HeaderLabel(text: "Download", icon: "arrow.down.circle")) {
+
+                Button("Download Sounds") {
+                    downloadSounds()
+                }
+                .disabled(isOverwriting)
+            }
+        }
+        .navigationTitle("Call Record Notification")
+        .onAppear {
+            downloadIfNeeded()
+            check()
+        }
+    }
+
+    // MARK: - Documents Paths
+
+    private var documents: URL {
+        FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
+    }
+
+    private var local1: URL {
+        documents.appendingPathComponent("StartDisclosurewithTone.m4a")
+    }
+
+    private var local2: URL {
+        documents.appendingPathComponent("StopDisclosure.caf")
+    }
+
+    // MARK: - Status Check
+
+    private func check() {
+        guard
+            let attrs1 = try? FileManager.default.attributesOfItem(atPath: target1),
+            let attrs2 = try? FileManager.default.attributesOfItem(atPath: target2),
+            let size1 = attrs1[.size] as? NSNumber,
+            let size2 = attrs2[.size] as? NSNumber
+        else {
+            disabled = false
+            return
+        }
+
+        disabled = size1.intValue < 2048 || size2.intValue < 2048
+    }
+
+    // MARK: - DOWNLOAD SYSTEM
+
+    private func downloadIfNeeded() {
+        let fm = FileManager.default
+
+        if !fm.fileExists(atPath: local1.path) {
+            download(remote1, to: local1)
+        }
+
+        if !fm.fileExists(atPath: local2.path) {
+            download(remote2, to: local2)
+        }
+    }
+
+    private func downloadSounds() {
+        isOverwriting = true
+
+        let group = DispatchGroup()
+
+        group.enter()
+        download(remote1, to: local1) { group.leave() }
+
+        group.enter()
+        download(remote2, to: local2) { group.leave() }
+
+        group.notify(queue: .main) {
+            self.isOverwriting = false
+            self.mgr.logmsg("Sounds downloaded")
+            self.check()
+        }
+    }
+
+    private func download(_ url: URL, to dest: URL, completion: (() -> Void)? = nil) {
+        URLSession.shared.downloadTask(with: url) { tempURL, _, error in
+
+            defer { completion?() }
+
+            guard let tempURL = tempURL, error == nil else {
+                DispatchQueue.main.async {
+                    self.mgr.logmsg("Download failed: \(url.lastPathComponent)")
+                }
+                return
+            }
+
+            do {
+                let fm = FileManager.default
+
+                if fm.fileExists(atPath: dest.path) {
+                    try fm.removeItem(at: dest)
+                }
+
+                try fm.moveItem(at: tempURL, to: dest)
+
+                DispatchQueue.main.async {
+                    self.mgr.logmsg("Downloaded: \(dest.lastPathComponent)")
+                }
+
+            } catch {
+                DispatchQueue.main.async {
+                    self.mgr.logmsg("File error: \(error.localizedDescription)")
+                }
+            }
+        }.resume()
+    }
+
+    // MARK: - Backup Creation
+
+    private func createBackupsIfNeeded() {
+        let fm = FileManager.default
+
+        let backupFolder = documents.appendingPathComponent("Backup")
+
+        let backup1 = backupFolder.appendingPathComponent("StartDisclosurewithTone.m4a")
+        let backup2 = backupFolder.appendingPathComponent("StopDisclosure.caf")
+
+        if !fm.fileExists(atPath: backupFolder.path) {
+            try? fm.createDirectory(at: backupFolder, withIntermediateDirectories: true)
+        }
+
+        if !fm.fileExists(atPath: backup1.path) {
+            try? fm.copyItem(at: URL(fileURLWithPath: target1), to: backup1)
+        }
+
+        if !fm.fileExists(atPath: backup2.path) {
+            try? fm.copyItem(at: URL(fileURLWithPath: target2), to: backup2)
+        }
+    }
+
+    // MARK: - Overwrite
+
+    @discardableResult
+    private func overwrite(target: String, source: String) -> Bool {
+        let ok = mgr.vfsoverwritefromlocalpath(target: target, source: source)
+
+        mgr.logmsg(ok ? "overwrite ok: \(target)" : "overwrite failed: \(target)")
+        return ok
+    }
+
+    // MARK: - Disable
+
+    private func disableRecordNotify() {
+        isOverwriting = true
+
+        DispatchQueue.global(qos: .userInitiated).async {
+
+            self.createBackupsIfNeeded()
+
+            let ok1 = self.overwrite(target: self.target1, source: self.local1.path)
+            let ok2 = self.overwrite(target: self.target2, source: self.local2.path)
+
+            DispatchQueue.main.async {
+                self.isOverwriting = false
+                self.check()
+
+                if !(ok1 && ok2) {
+                    self.mgr.logmsg("Failed disabling notification")
+                }
+            }
+        }
+    }
+
+    // MARK: - Enable
+
+    private func enableRecordNotify() {
+        let fm = FileManager.default
+
+        let backupFolder = documents.appendingPathComponent("Backup")
+        let backup1 = backupFolder.appendingPathComponent("StartDisclosurewithTone.m4a")
+        let backup2 = backupFolder.appendingPathComponent("StopDisclosure.caf")
+
+        guard
+            fm.fileExists(atPath: backup1.path),
+            fm.fileExists(atPath: backup2.path)
+        else {
+            mgr.logmsg("Backups not found")
+            return
+        }
+
+        isOverwriting = true
+
+        DispatchQueue.global(qos: .userInitiated).async {
+
+            let ok1 = self.overwrite(target: self.target1, source: backup1.path)
+            let ok2 = self.overwrite(target: self.target2, source: backup2.path)
+
+            DispatchQueue.main.async {
+                self.isOverwriting = false
+                self.check()
+
+                if !(ok1 && ok2) {
+                    self.mgr.logmsg("Failed restoring notification")
+                }
+            }
+        }
+    }
+}
  ```

  </details>
- 🟡 `lara/views/tweaks/ToolsView.swift` (modified) — +31 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -24,6 +24,8 @@ struct ToolsView: View {
     @State private var pid: pid_t = getpid()
     @State private var status: String?
     @State private var crashname: String = "SpringBoard"
+    @State private var pausedProcesses: Set<String> = []
+    @State private var proc_sbx: UInt64 = 0
     
     private enum tokenclass: String, CaseIterable, Identifiable {
         case read = "com.apple.app-sandbox.read"
@@ -156,10 +158,37 @@ struct ToolsView: View {
                     }
                 }
                 .disabled(crashname.isEmpty)
+                Button("Pause") {
+                    crashname.withCString { _ = proc_pause_resume($0, false) }
+                    pausedProcesses.insert(crashname)
+                }
+                .disabled(crashname.isEmpty || pausedProcesses.contains(crashname))
+
+                Button("Resume") {
+                    crashname.withCString { _ = proc_pause_resume($0, true) }
+                    pausedProcesses.remove(crashname)
+                }
+                .disabled(crashname.isEmpty || !pausedProcesses.contains(crashname))
+                
+                Button("SBX Escape Helper") {
+                    crashname.withCString { cstr in
+                        proc_sbx = procbyname(cstr)
+                    }
+
+                    if proc_sbx == 0 {
+                            status = "Failed to get proc"
+                            return
+                        }
+                    
+                        let errorcheck = sbx_escape(proc_sbx)
+                        status = errorcheck == 0 ? nil : "Failure"
+                    }
+                    .disabled(crashname.isEmpty)
+
             } header: {
-                Text("Crasher")
+                Text("Task Manager")
             } footer: {
-                Text("Crashes the selected process")
+                Text("Manages The Selected Process")
             }
 
             Section {
  ```

  </details>
- 🟡 `lara/views/tweaks/TweaksView.swift` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -59,6 +59,8 @@ struct TweaksView: View {
                         .disabled(!mgr.vfsready)
                     NavigationLink("OTA Updates", destination: OTAView(mgr: mgr))
                     NavigationLink("Screen Time", destination: ScreenTimeView(mgr: mgr))
+                    NavigationLink("Clean Cache", destination: CacheView())
+                    NavigationLink("Call Record Notification", destination: RecordView(mgr: mgr))
                 }
                 
                 Section(header: HeaderLabel(text: "Broken", icon: "exclamationmark.triangle.fill")) {
  ```

  </details>
- 🟢 `lara/views/tweaks/cacheclean/BundleResolver.swift` (added) — +222 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,222 @@
+//
+//  BundleResolver.swift
+//  lara
+//
+
+import Foundation
+import UIKit
+
+// MARK: - Model
+
+struct ResolvedApp {
+    let dataUUID: String
+    let bundleID: String
+    let bundlePath: String
+    let name: String
+    let icon: UIImage?
+}
+
+// MARK: - Resolver
+
+final class BundleResolver {
+
+    private let fm = FileManager.default
+
+    private let bundleRoot = "/var/containers/Bundle/Application"
+    private let dataRoot = "/var/mobile/Containers/Data/Application"
+
+    // MARK: Public
+
+    func resolveAll() -> [ResolvedApp] {
+
+        let bundleMap = buildBundleMap()
+
+        guard let dataContainers = try? fm.contentsOfDirectory(atPath: dataRoot) else {
+            return []
+        }
+
+        var results: [ResolvedApp] = []
+
+        for dataUUID in dataContainers {
+
+            let dataPath = dataRoot + "/" + dataUUID
+            let metaPath = dataPath + "/.com.apple.mobile_container_manager.metadata.plist"
+
+            // MARK: STEP 1 - metadata
+            var bundleID =
+                NSDictionary(contentsOfFile: metaPath)?["MCMMetadataIdentifier"] as? String
+
+            // MARK: STEP 2 - fallback scan inside container
+            if bundleID == nil {
+                bundleID = findBundleIDInDataContainer(dataPath)
+            }
+
+            // ❗ DO NOT DROP APP
+            let finalBundleID = bundleID ?? "unknown.\(dataUUID)"
+
+            // MARK: STEP 3 - resolve bundle path
+            let resolvedBundlePath =
+                bundleID != nil
+                ? (bundleMap[finalBundleID] ?? findBundlePathFallback(bundleID: finalBundleID))
+                : nil
+
+            let finalBundlePath = resolvedBundlePath ?? ""
+
+            // MARK: STEP 4 - name resolution (NEVER FAIL)
+            let finalName = readSafeName(
+                bundlePath: finalBundlePath.isEmpty ? dataPath : finalBundlePath,
+                fallback: dataUUID
+            )
+
+            // MARK: STEP 5 - icon resolution (NEVER FAIL)
+            let finalIcon =
+                finalBundlePath.isEmpty
+                ? UIImage(systemName: "app")
+                : readIcon(finalBundlePath)
+
+            results.append(
+                ResolvedApp(
+                    dataUUID: dataUUID,
+                    bundleID: finalBundleID,
+                    bundlePath: finalBundlePath,
+                    name: finalName,
+                    icon: finalIcon
+                )
+            )
+        }
+
+        return results
+    }
+
+    // MARK: Bundle Map
+
+    private func buildBundleMap() -> [String: String] {
+
+        var map: [String: String] = [:]
+
+        guard let roots = try? fm.contentsOfDirectory(atPath: bundleRoot) else {
+            return map
+        }
+
+        for root in roots {
+
+            let rootPath = bundleRoot + "/" + root
+
+            guard let items = try? fm.contentsOfDirectory(atPath: rootPath) else {
+                continue
+            }
+
+            for item in items where item.hasSuffix(".app") {
+
+                let appPath = rootPath + "/" + item
+
+                // PRIMARY
+                if let meta = NSDictionary(contentsOfFile: appPath + "/.com.apple.mobile_container_manager.metadata.plist"),
+                   let bundleID = meta["MCMMetadataIdentifier"] as? String {
+                    map[bundleID] = appPath
+                    continue
+                }
+
+                // FALLBACK
+                if let info = NSDictionary(contentsOfFile: appPath + "/Info.plist"),
+                   let bundleID = info["CFBundleIdentifier"] as? String {
+                    map[bundleID] = appPath
+                }
+            }
+        }
+
+        return map
+    }
+
+    // MARK: Data container scan
+
+    private func findBundleIDInDataContainer(_ dataPath: String) -> String? {
+
+        guard let items = try? fm.contentsOfDirectory(atPath: dataPath) else {
+            return nil
+        }
+
+        for item in items where item.hasSuffix(".app") {
+
+            let infoPath = dataPath + "/" + item + "/Info.plist"
+
+            if let info = NSDictionary(contentsOfFile: infoPath),
+               let bundleID = info["CFBundleIdentifier"] as? String {
+                return bundleID
+            }
+        }
+
+        return nil
+    }
+
+    // MARK: Bundle fallback
+
+    private func findBundlePathFallback(bundleID: String) -> String? {
+
+        guard let roots = try? fm.contentsOfDirectory(atPath: bundleRoot) else {
+            return nil
+        }
+
+        for root in roots {
+
+            let rootPath = bundleRoot + "/" + root
+
+            guard let items = try? fm.contentsOfDirectory(atPath: rootPath) else {
+                continue
+            }
+
+            for item in items where item.hasSuffix(".app") {
+
+                let appPath = rootPath + "/" + item
+
+                if let info = NSDictionary(contentsOfFile: appPath + "/Info.plist"),
+                   let id = info["CFBundleIdentifier"] as? String,
+                   id == bundleID {
+                    return appPath
+                }
+            }
+        }
+
+        return nil
+    }
+
+    // MARK: SAFE NAME (never empty)
+
+    private func readSafeName(bundlePath: String, fallback: String) -> String {
+
+        let infoPath = bundlePath + "/Info.plist"
+
+        guard let info = NSDictionary(contentsOfFile: infoPath) else {
+            return fallback
+        }
+
+        return info["CFBundleDisplayName"] as? String ??
+               info["CFBundleName"] as? String ??
+               fallback
+    }
+
+    // MARK: Icon
+
+    private func readIcon(_ bundlePath: String) -> UIImage? {
+
+        let infoPath = bundlePath + "/Info.plist"
+
+        guard let info = NSDictionary(contentsOfFile: infoPath) else {
+            return UIImage(systemName: "app")
+        }
+
+        if let icons = info["CFBundleIcons"] as? [String: Any],
+           let primary = icons["CFBundlePrimaryIcon"] as? [String: Any],
+           let files = primary["CFBundleIconFiles"] as? [String],
+           let iconName = files.last {
+
+            let path = bundlePath + "/" + iconName
+
+            return UIImage(contentsOfFile: path)
+                ?? UIImage(contentsOfFile: path + "@2x.png")
+                ?? UIImage(contentsOfFile: path + ".png")
+        }
+
+        return UIImage(systemName: "app")
+    }
+}
  ```

  </details>
- 🟢 `lara/views/tweaks/cacheclean/CacheView.swift` (added) — +356 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,356 @@
+//
+//
+// CacheView.swift
+//
+// lara
+//
+
+import SwiftUI
+import UIKit
+import WebKit
+import Combine
+
+// MARK: - Models
+
+struct CacheApp: Identifiable {
+    let id: String
+    let name: String
+    let bundleID: String?
+
+    let appBundlePath: String?
+    let dataContainerPath: String
+
+    let icon: UIImage?
+
+    let cacheSize: Int64
+    let tmpSize: Int64
+    let documentsSize: Int64
+
+    let cachePath: String
+    let tmpPath: String
+    let documentsPath: String
+}
+
+struct StorageSnapshot: Identifiable {
+    let id = UUID()
+    let date = Date()
+    let totalBytes: Int64
+}
+
+struct AppRecord {
+    let dataUUID: String
+    let bundleID: String
+    let bundlePath: String
+    let name: String
+    let icon: UIImage?
+}
+
+// MARK: - Manager
+
+final class CleanerManager: ObservableObject {
+
+    @Published var apps: [CacheApp] = []
+    @Published var snapshots: [StorageSnapshot] = []
+
+    @Published var isScanning = false
+    @Published var scanProgress: Double = 0
+    @Published var statusText = ""
+
+    @Published var totalCacheBytes: Int64 = 0
+
+    private let fm = FileManager.default
+    private let dataRoot = "/var/mobile/Containers/Data/Application"
+
+    // Resolver (optional enrichment layer)
+    private let resolver = BundleResolver()
+    private var appDB: [String: AppRecord] = [:]
+
+    // MARK: Build DB (SAFE)
+    private func buildDatabase() {
+        let resolved = resolver.resolveAll()
+
+        var db: [String: AppRecord] = [:]
+        for app in resolved {
+            db[app.dataUUID] = AppRecord(
+                dataUUID: app.dataUUID,
+                bundleID: app.bundleID,
+                bundlePath: app.bundlePath,
+                name: app.name,
+                icon: app.icon
+            )
+        }
+
+        self.appDB = db
+    }
+
+    // MARK: Scan
+
+    func startScan(minSizeMB: Int64 = 1) {
+
+        guard !isScanning else { return }
+
+        isScanning = true
+        apps.removeAll()
+        scanProgress = 0
+        totalCacheBytes = 0
+        statusText = "Scanning apps..."
+
+        DispatchQueue.global(qos: .userInitiated).async {
+
+            self.buildDatabase()
+
+            let containers = (try? self.fm.contentsOfDirectory(atPath: self.dataRoot)) ?? []
+
+            let total = max(containers.count, 1)
+            var processed = 0
+
+            var results: [CacheApp] = []
+
+            for uuid in containers {
+
+                let dataPath = self.dataRoot + "/" + uuid
+
+                let cachePath = dataPath + "/Library/Caches"
+                let tmpPath = dataPath + "/tmp"
+                let docsPath = dataPath + "/Documents"
+
+                let cacheSize = self.folderSize(cachePath)
+                let tmpSize = self.folderSize(tmpPath)
+                let docsSize = self.folderSize(docsPath)
+
+                let totalSize = cacheSize + tmpSize + docsSize
+
+                // Only skip extremely small containers
+                if totalSize < minSizeMB * 1024 * 1024 {
+                    processed += 1
+                    continue
+                }
+
+                let appInfo = self.appDB[uuid]
+
+                let name = appInfo?.name ?? "App \(uuid.suffix(6))"
+                let bundleID = appInfo?.bundleID
+                let bundlePath = appInfo?.bundlePath
+                let icon = appInfo?.icon
+
+                results.append(CacheApp(
+                    id: uuid,
+                    name: name,
+                    bundleID: bundleID,
+                    appBundlePath: bundlePath,
+                    dataContainerPath: dataPath,
+                    icon: icon,
+                    cacheSize: cacheSize,
+                    tmpSize: tmpSize,
+                    documentsSize: docsSize,
+                    cachePath: cachePath,
+                    tmpPath: tmpPath,
+                    documentsPath: docsPath
+                ))
+
+                processed += 1
+
+                DispatchQueue.main.async {
+                    self.scanProgress = Double(processed) / Double(total)
+                    self.statusText = "Scanning \(processed)/\(total)"
+                }
+            }
+
+            let totalBytes = results.reduce(0) {
+                $0 + $1.cacheSize + $1.tmpSize + $1.documentsSize
+            }
+
+            DispatchQueue.main.async {
+                self.apps = results.sorted { $0.cacheSize > $1.cacheSize }
+                self.totalCacheBytes = totalBytes
+
+                self.snapshots.append(StorageSnapshot(totalBytes: totalBytes))
+
+                self.isScanning = false
+                self.scanProgress = 1.0
+                self.statusText = "Completed (\(results.count) apps)"
+            }
+        }
+    }
+
+    // MARK: Delete
+
+    func deleteCache(_ app: CacheApp) {
+        try? fm.removeItem(atPath: app.cachePath)
+        try? fm.removeItem(atPath: app.tmpPath)
+    }
+
+    func deleteAll() {
+        for app in apps {
+            deleteCache(app)
+        }
+    }
+
+    // MARK: Web cleanup
+
+    func cleanWKWebView() {
+
+        let types: Set<String> = [
+            WKWebsiteDataTypeDiskCache,
+            WKWebsiteDataTypeMemoryCache,
+            WKWebsiteDataTypeCookies,
+            WKWebsiteDataTypeLocalStorage,
+            WKWebsiteDataTypeSessionStorage,
+            WKWebsiteDataTypeIndexedDBDatabases,
+            WKWebsiteDataTypeWebSQLDatabases
+        ]
+
+        WKWebsiteDataStore.default().removeData(
+            ofTypes: types,
+            modifiedSince: Date(timeIntervalSince1970: 0)
+        ) {
+            DispatchQueue.main.async {
+                self.statusText = "WKWebView cleared"
+            }
+        }
+    }
+
+    func cleanURLCache() {
+        URLCache.shared.removeAllCachedResponses()
+    }
+
+    // MARK: Size
+
+    private func folderSize(_ path: String) -> Int64 {
+        guard let e = fm.enumerator(atPath: path) else { return 0 }
+
+        var size: Int64 = 0
+
+        for case let file as String in e {
+
+            let full = (path as NSString).appendingPathComponent(file)
+
+            if let attrs = try? fm.attributesOfItem(atPath: full),
+               let fileSize = attrs[.size] as? NSNumber {
+                size += fileSize.int64Value
+            }
+        }
+
+        return size
+    }
+}
+
+// MARK: - UI
+
+struct CacheView: View {
+
+    @StateObject var mgr = CleanerManager()
+
+    var body: some View {
+
+        NavigationStack {
+
+            VStack {
+
+                Text("Clean Cache")
+                    .font(.title2).bold()
+
+                Text("\(mgr.totalCacheBytes / 1024 / 1024) MB Total")
+                    .font(.title)
+
+                ProgressView(value: mgr.scanProgress)
+
+                Text(mgr.statusText)
+                    .font(.caption)
+
+                List {
+
+                    Section("Apps") {
+
+                        ForEach(mgr.apps) { app in
+
+                            HStack {
+
+                                if let icon = app.icon {
+                                    Image(uiImage: icon)
+                                        .resizable()
+                                        .frame(width: 40, height: 40)
+                                        .cornerRadius(8)
+                                } else {
+                                    Image(systemName: "app")
+                                }
+
+                                VStack(alignment: .leading) {
+                                    Text(app.name).bold()
+                                    Text("Cache \(app.cacheSize / 1024 / 1024) MB")
+                                    Text("Tmp \(app.tmpSize / 1024 / 1024) MB")
+                                    Text("Docs \(app.documentsSize / 1024 / 1024) MB")
+                                        .font(.caption)
+                                        .foregroundStyle(.secondary)
+                                }
+                            }
+                            .swipeActions {
+                                Button(role: .destructive) {
+                                    mgr.deleteCache(app)
+                                } label: {
+                                    Text("Delete")
+                                }
+                            }
+                            .swipeActions(edge: .trailing) {
+                                Button(role: .destructive) {
+                                    mgr.deleteCache(app)
+                                } label: {
+                                    Text("Cache")
+                                }
+                            }
+                            .swipeActions(edge: .leading) {
+                                Button(role: .destructive) {
+                                try? FileManager.default.removeItem(atPath: app.documentsPath)
+                                if let index = mgr.apps.firstIndex(where: { $0.id == app.id }) {
+                                    mgr.apps[index] = CacheApp(
+                                        id: app.id,
+                                        name: app.name,
+                                        bundleID: app.bundleID,
+                                        appBundlePath: app.appBundlePath,
+                                        dataContainerPath: app.dataContainerPath,
+                                        icon: app.icon,
+                                        cacheSize: app.cacheSize,
+                                        tmpSize: app.tmpSize,
+                                        documentsSize: 0,  
+                                        cachePath: app.cachePath,
+                                        tmpPath: app.tmpPath,
+                                        documentsPath: app.documentsPath
+
+                                    )
+                                }
+                                    
+                            } label: {
+                                Text("Docs")
+                            }
+                            .tint(.orange)
+                            }
+                        }
+                    }
+
+                    Section("Tools") {
+
+                        Button("Delete ALL Cache") {
+                            mgr.deleteAll()
+                        }
+                        .foregroundStyle(.red)
+
+                        Button("Clear WKWebView") {
+                            mgr.cleanWKWebView()
+                        }
+
+                        Button("Clear URLCache") {
+                            mgr.cleanURLCache()
+                        }
+
+                        Button("Rescan") {
+                            mgr.startScan()
+                        }
+                    }
+                }
+            }
+            .onAppear {
+                mgr.startScan()
+            }
+        }
+    }
+}
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 6f6a876: Update sbx.h
- : 📝 Commit 5dedefd: Update utils.m
- : 📝 Commit 9e0a60b: Update utils.h
- : 📝 Commit d8ecadb: Add files via upload
- : 📝 Commit 78d9af1: Rename lara/views/tweaks/BundleResolver 2.swift to lara/views/tweaks/cacheclean/BundleResolver.swift
- : 📝 Commit 9ffc7df: Rename lara/views/tweaks/CacheView.swift to lara/views/tweaks/cleancache/CacheView.swift
- : 📝 Commit 3ad2014: Rename CacheView.swift to CacheView.swift
- : 📝 Commit 17bf225: Update ToolsView.swift
- : 📝 Commit f2ba6b8: Update TweaksView.swift
- : 📝 Commit 964510f: Update ToolsView.swift
- : 📝 Commit 668bd71: Wrong file corrected
- : 📝 Commit 62bb76c: Update README.md
- : 📝 Commit 04cd314: Create RecordView.swift
- : 📝 Commit 4037c21: Update TweaksView.swift
- : 📝 Commit 5e152ae: Update RecordView.swift
- : 📝 Commit f8e1f9b: Remove 'This Call is being Recorded' Sound


---

## Comments (2)

### Comment #1 — [@notthemystery](https://github.com/notthemystery) · Jun 16, 2026

`ID: 4714789608`


Whyyyyyyyyy


---

### Comment #2 — [@aletschfirn](https://github.com/aletschfirn) · Jun 29, 2026

`ID: 4837168663`


Why not just squash it then? It does require force push but it's still on your repo in specific branch, so it shouldn't be a problem? You can squash it with git rebase safely (git rebase -i HEAD~N (with N - commits to squash)


---

*Exported on 10/14/2026, 3:11:11 PM from [https://github.com/rooootdev/lara/pull/294](https://github.com/rooootdev/lara/pull/294)*
