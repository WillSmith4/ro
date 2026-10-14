# DO NOT MERGE WITHOUT BACKUP. This is a experimental fork added resume and pause to the crasher.

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#276](https://github.com/rooootdev/lara/pull/276)  
> **State:** 🔴 closed  
> **Branch:** `notthemystery:main` → `rooootdev:main`  
> **Changes:** +132 / -6 across 8 files  
> **Created:** May 28, 2026  
> **Updated:** May 29, 2026  
> **Closed:** May 29, 2026  

---

## Description

*Opened by [@notthemystery](https://github.com/notthemystery)* *on May 28, 2026*


Not tested.the find UUID is WIP. I’m using that for another tweak


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (16)

- [`779d9de`](https://github.com/rooootdev/lara/commit/779d9deca6e9e99de24b899118300e30093886f2) Create FindUUID.m — *The Mystery* · May 26, 2026
- [`78e1a68`](https://github.com/rooootdev/lara/commit/78e1a68049e1f173c89cb04dbd59496c7ed88650) Change foundUUID assignment to use full path — *The Mystery* · May 26, 2026
- [`b6c3b3e`](https://github.com/rooootdev/lara/commit/b6c3b3e496b618859079f5bfe8ceed9dcce8fc77) Update utils.m — *The Mystery* · May 27, 2026
- [`6eb9a97`](https://github.com/rooootdev/lara/commit/6eb9a97fd58b2a62bd1f6298067ea4c2a0f05d3b) Update utils.h — *The Mystery* · May 27, 2026
- [`597e6c2`](https://github.com/rooootdev/lara/commit/597e6c2d9a8479708363d309af3b142c1f33e188) Update ToolsView.swift — *The Mystery* · May 27, 2026
- [`c124505`](https://github.com/rooootdev/lara/commit/c124505d51e7e20d14b4c89d952e577be987f2aa) Delete lara.xcodeproj directory — *The Mystery* · May 27, 2026
- [`f389fbd`](https://github.com/rooootdev/lara/commit/f389fbd21e7243e56ff278233070d8bf7683c9ae) Update build.yml — *The Mystery* · May 27, 2026
- [`d15d704`](https://github.com/rooootdev/lara/commit/d15d7046fb820fa28b51dc3541b54cdd00519d6a) Create project.yml — *The Mystery* · May 27, 2026
- [`9e75369`](https://github.com/rooootdev/lara/commit/9e75369ba6e8a1d47b380acfe9d9c27795a2fc59) Update project.yml — *The Mystery* · May 27, 2026
- [`d900a6d`](https://github.com/rooootdev/lara/commit/d900a6d069ea97b60cc5cc7ce2adf05390e06507) Update build_ipa.sh — *The Mystery* · May 27, 2026
- [`07198ab`](https://github.com/rooootdev/lara/commit/07198abe9c447c24e6e92c628b0ed4ae33e6ad73) Update build_ipa.sh — *The Mystery* · May 27, 2026
- [`e2d2094`](https://github.com/rooootdev/lara/commit/e2d2094888c4c32f0e7e05b9fdc17cf6b028c82e) Add files via upload — *The Mystery* · May 27, 2026
- [`56d2489`](https://github.com/rooootdev/lara/commit/56d24893fe79c30edcfbd0fcb77d57a48061daea) Rename project.pbxproj to project.pbxproj — *The Mystery* · May 27, 2026
- [`8d0652b`](https://github.com/rooootdev/lara/commit/8d0652b9781f74c057c13cdb93d3b64296d43e73) Rename contents.xcworkspacedata to contents.xcworkspacedata — *The Mystery* · May 27, 2026
- [`cd029af`](https://github.com/rooootdev/lara/commit/cd029aff61f97d03c65e4051c1e24741f2a39ea7) Update build.yml — *The Mystery* · May 27, 2026
- [`64ffb0e`](https://github.com/rooootdev/lara/commit/64ffb0e6b06b06c5855bb0c524765882d1343b44) Merge branch 'rooootdev:main' into main — *The Mystery* · May 28, 2026


---

## Changed Files (8)

- 🟡 `.github/workflows/build.yml` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -18,7 +18,7 @@ jobs:
         uses: actions/checkout@v4
         with:
           fetch-depth: 0
-
+      
       - name: setup xcode
         uses: maxim-lobanov/setup-xcode@v1
         with:
  ```

  </details>
- 🔴 `lara.xcodeproj/.gitignore` (removed) — +0 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -1 +0,0 @@
-xcuserdata/
\ No newline at end of file
  ```

  </details>
- 🟢 `lara/classes/FindUUID.m` (added) — +59 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,59 @@
+#import <Foundation/Foundation.h>
+#include <dlfcn.h>
+#include <objc/runtime.h>
+
+@interface LSApplicationWorkspace : NSObject
++ (id)defaultWorkspace;
+- (id)allInstalledApplications;
+@end
+
+@interface LSApplicationProxy : NSObject
+- (NSString *)applicationIdentifier;
+- (NSString *)localizedName;
+- (NSURL *)containerURL; 
+@end
+
+NSString* get_container_uuid_for_app(NSString *targetName) {
+    if (!targetName || [targetName length] == 0) {
+        return nil;
+    }
+    
+    void *handle = dlopen("/System/Library/PrivateFrameworks/MobileCoreServices.framework/MobileCoreServices", RTLD_NOW);
+    if (!handle) {
+        handle = dlopen("/System/Library/Frameworks/CoreServices.framework/CoreServices", RTLD_NOW);
+    }
+    
+    if (!handle) {
+        return nil;
+    }
+    
+    Class workspaceClass = objc_getClass("LSApplicationWorkspace");
+    if (!workspaceClass) {
+        dlclose(handle);
+        return nil;
+    }
+    
+    id workspace = [workspaceClass defaultWorkspace];
+    NSArray *apps = [workspace allInstalledApplications];
+    NSString *foundUUID = nil;
+    
+    NSString *searchQuery = [targetName lowercaseString];
+    
+    for (LSApplicationProxy *app in apps) {
+        NSString *appName = [[app localizedName] lowercaseString];
+        NSString *bundleId = [[app applicationIdentifier] lowercaseString];
+        
+        if ((appName && [appName rangeOfString:searchQuery].location != NSNotFound) || 
+            (bundleId && [bundleId rangeOfString:searchQuery].location != NSNotFound)) {
+            
+            NSURL *containerURL = [app containerURL];
+            if (containerURL) {
+                foundUUID = [containerURL path];
+                break;
+            }
+        }
+    }
+    
+    dlclose(handle);
+    return foundUUID;
+}
  ```

  </details>
- 🟡 `lara/kexploit/utils.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -68,6 +68,7 @@ uint64_t proc_self(void);
 uint64_t task_self(void);
 
 int crashproc(const char* pid);
+int proc_pause_resume(const char *name, bool resume)
 
 #ifdef __cplusplus
 }
  ```

  </details>
- 🟡 `lara/kexploit/utils.m` (modified) — +21 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -853,3 +853,24 @@ int crashproc(const char* name) {
     ds_kwrite64(state + offsetof(struct arm_saved_state64, sp), 0x1337133713371337);
     return 0;
 }
+
+int proc_pause_resume(const char *name, bool resume) {
+    uint64_t proc = procbyname(name);
+    pid_t pid = (pid_t)ds_kread32(proc + off_proc_p_pid);
+    mach_port_t task = MACH_PORT_NULL;
+    kern_return_t kr =
+        task_for_pid(mach_task_self(), pid, &task);
+    if (resume) {
+        kr = task_resume(task);
+    } else {
+        kr = task_suspend(task);
+    }
+    if (kr != KERN_SUCCESS) {
+        mach_port_deallocate(mach_task_self(), task);
+    }
+    printf("%s %s\n",
+           name,
+           resume ? "resumed" : "suspended");
+    mach_port_deallocate(mach_task_self(), task);
+    return 0;
+}
  ```

  </details>
- 🟡 `lara/views/tweaks/ToolsView.swift` (modified) — +19 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -24,6 +24,7 @@ struct ToolsView: View {
     @State private var pid: pid_t = getpid()
     @State private var status: String?
     @State private var crashname: String = "SpringBoard"
+    @State private var pausedProcesses: Set<String> = []
     
     private enum tokenclass: String, CaseIterable, Identifiable {
         case read = "com.apple.app-sandbox.read"
@@ -156,10 +157,26 @@ struct ToolsView: View {
                     }
                 }
                 .disabled(crashname.isEmpty)
+                
+                Button("Pause") {
+                    crashname.withCString { cstr in
+                         _ = proc_pause_resume(cstr, false)
+                    pausedProcesses.insert(crashname)
+                    }
+                }
+                .disabled(crashname.isEmpty || pausedProcess.contains(crashname))
+
+                Button("Resume") {
+                    crashname.withCString { cstr in
+                         _ = proc_pause_resume(cstr, true)
+                    }
+                }
+                .disabled(crashname.isEmpty || !pausedProcess.contains(crashname))
+                
             } header: {
-                Text("Crasher")
+                Text("Task Manager")
             } footer: {
-                Text("Crashes the selected process")
+                Text("Pause, Resume or Crash a Selected Process")
             }
 
             Section {
  ```

  </details>
- 🟢 `project.yml` (added) — +26 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,26 @@
+name: lara
+
+options:
+  bundleIdPrefix: com.roooot
+  deploymentTarget:
+    iOS: 17.0
+
+targets:
+  Lara:
+    type: application
+    platform: iOS
+
+    sources:
+      - path: lara
+
+    resources:
+      - path: lara/assets/lara.png
+
+    settings:
+      base:
+        PRODUCT_BUNDLE_IDENTIFIER: com.roooot.lara
+        INFOPLIST_FILE: lara/Info.plist
+        SWIFT_VERSION: 5.0
+        ENABLE_BITCODE: NO
+        CLANG_ENABLE_OBJC_ARC: YES
+        SWIFT_OBJC_BRIDGING_HEADER: lara/lara-Bridging-Header.h
  ```

  </details>
- 🟡 `scripts/build_ipa.sh` (modified) — +5 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -7,9 +7,12 @@ mkdir -p build
 echo "Build Started!"
 echo
 
+SCHEME=$(xcodebuild -list -project lara.xcodeproj | sed -n '/Schemes:/,$p' | tail -n +2 | head -n 1 | xargs)
+echo "Scheme: $SCHEME"
+
 xcodebuild \
   -project lara.xcodeproj \
-  -scheme lara \
+  -scheme "$SCHEME" \
   -configuration Debug \
   -sdk iphoneos \
   -arch arm64e \
@@ -18,7 +21,7 @@ xcodebuild \
   CODE_SIGN_IDENTITY="" \
   CODE_SIGN_ENTITLEMENTS="Config/lara.entitlements" \
   archive \
-  -archivePath "$PWD/build/lara.xcarchive" 2>&1 | xcpretty
+  -archivePath "$PWD/build/lara.xcarchive"
 
 APP_PATH="$PWD/build/lara.xcarchive/Products/Applications/lara.app"
 if [ ! -d "$APP_PATH" ]; then
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 779d9de: Create FindUUID.m
- : 📝 Commit 78e1a68: Change foundUUID assignment to use full path
- : 📝 Commit b6c3b3e: Update utils.m
- : 📝 Commit 6eb9a97: Update utils.h
- : 📝 Commit 597e6c2: Update ToolsView.swift
- : 📝 Commit c124505: Delete lara.xcodeproj directory
- : 📝 Commit f389fbd: Update build.yml
- : 📝 Commit d15d704: Create project.yml
- : 📝 Commit 9e75369: Update project.yml
- : 📝 Commit d900a6d: Update build_ipa.sh
- : 📝 Commit 07198ab: Update build_ipa.sh
- : 📝 Commit e2d2094: Add files via upload
- : 📝 Commit 56d2489: Rename project.pbxproj to project.pbxproj
- : 📝 Commit 8d0652b: Rename contents.xcworkspacedata to contents.xcworkspacedata
- : 📝 Commit cd029af: Update build.yml
- : 📝 Commit 64ffb0e: Merge branch 'rooootdev:main' into main
- May 28, 2026: 🟢 @notthemystery marked ready for review
- May 29, 2026: 🔴 Closed by @notthemystery


---

## Comments (1)

### Comment #1 — [@notthemystery](https://github.com/notthemystery) · May 28, 2026

`ID: 4563637237`


Also it gives error about vnode.h when compiling bridging header


---

*Exported on 10/14/2026, 3:09:27 PM from [https://github.com/rooootdev/lara/pull/276](https://github.com/rooootdev/lara/pull/276)*
