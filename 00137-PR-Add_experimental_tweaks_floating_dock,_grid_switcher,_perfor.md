# Add experimental tweaks: floating dock, grid switcher, performance HUD

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#137](https://github.com/rooootdev/lara/pull/137)  
> **State:** 🟣 merged  
> **Branch:** `khanhduytran0:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 20, 2026  
> **Changes:** +153 / -2 across 4 files  
> **Created:** Apr 20, 2026  
> **Updated:** Apr 20, 2026  
> **Closed:** Apr 20, 2026  

---

## Description

*Opened by [@khanhduytran0](https://github.com/khanhduytran0)* *on Apr 20, 2026*


I'm here again and I'm just adding more random tweaks:
- Performance HUD works similar to my [CAPerfHUD](https://github.com/khanhduytran0/CAPerfHUD) app, which displays an FPS overlay on top of SpringBoard

Experimental broken tweaks:
- Floating dock: currently it sticks as it, doesn't auto hide or anything
- Grid app switcher: broken animation when opening the app switcher

Also I've slightly tweaked RemoteView to split into several Sections.


🎉 1


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24649085507/job/72067785707) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24649085507/job/72067709486) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`fc27e86`](https://github.com/rooootdev/lara/commit/fc27e86d5b53ddb18e7d66950e682e7fe0fa72c4) Add experimental tweaks: floating dock, grid switcher, performance HUD — *khanhduytran0* · Apr 20, 2026


---

## Changed Files (4)

- 🟢 `lara/kexploit/TaskRop/PrivateAPI.h` (added) — +14 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,14 @@
+//
+//  PrivateAPI.h
+//  lara
+//
+//  Created by Duy Tran on 20/4/26.
+//
+
+@import QuartzCore;
+
+#define CA_DEBUG_FLAG_PERFORMANCE_HUD 0x10000000
+extern void CARenderServerGetDebugFlags(mach_port_t port, int key);
+extern void CARenderServerGetDebugValue(mach_port_t port, int key);
+extern void CARenderServerSetDebugFlags(mach_port_t port, int key, int value);
+extern void CARenderServerSetDebugValue(mach_port_t port, int key, int value);
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.h` (modified) — +4 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -139,5 +139,9 @@ int enable_jit(RemoteCall *proc, const char *bundleID);
 int set_dock_icon_count(RemoteCall *proc, int count);
 int five_icon_dock(RemoteCall *proc);
 int enable_upside_down(RemoteCall *proc);
+int enable_floating_dock(RemoteCall *proc);
+int enable_grid_app_switcher(RemoteCall *proc);
+int get_performance_hud(RemoteCall *proc);
+int set_performance_hud(RemoteCall *proc, int selection);
 
 #endif /* RemoteCall_h */
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.m` (modified) — +85 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -15,6 +15,7 @@
 #import <sys/mman.h>
 
 #import "RemoteCall.h"
+#import "PrivateAPI.h"
 #import "VM.h"
 #import "Exception.h"
 #import "PAC.h"
@@ -1207,3 +1208,87 @@ int enable_upside_down(RemoteCall *proc) {
      */
     return 0;
 }
+
+// FIXME: floating dock does not hide at all
+int enable_floating_dock(RemoteCall *proc) {
+    uint64_t clsReturn1 = remote_getClass(proc, "SBCoverSheetPrimarySlidingViewController");
+    uint64_t clsSBFloatingDockController = remote_getClass(proc, "SBFloatingDockController");
+    uint64_t clsSBIconController = remote_getClass(proc, "SBIconController");
+    uint64_t clsSBMainWorkspace = remote_getClass(proc, "SBMainWorkspace");
+    
+    uint64_t createFloatingDock = remote_sel(proc, "createFloatingDockControllerForWindowScene:");
+    uint64_t selMainWindowScene = remote_sel(proc, "mainWindowScene");
+    uint64_t selPerform = remote_sel(proc, "performSelectorOnMainThread:withObject:waitUntilDone:");
+    uint64_t selSharedInstance = remote_sel(proc, "sharedInstance");
+    uint64_t selRespondsTo = remote_sel(proc, "respondsToSelector:");
+    uint64_t selSupported = remote_sel(proc, "isFloatingDockSupported");
+    uint64_t selSupported2 = remote_sel(proc, "isFloatingDockUtilitiesSupported");
+    uint64_t selReturn1 = remote_sel(proc, "_canShowWhileLocked");
+    
+    uint64_t methodSupported = RemoteArbCall(proc, class_getClassMethod, clsSBFloatingDockController, selSupported);
+    uint64_t methodSupported2 = RemoteArbCall(proc, class_getClassMethod, clsSBFloatingDockController, selSupported2);
+    uint64_t methodReturn1 = RemoteArbCall(proc, class_getInstanceMethod, clsReturn1, selReturn1);
+    
+    uint64_t return1Imp = RemoteArbCall(proc, method_getImplementation, methodReturn1);
+    RemoteArbCall(proc, method_setImplementation, methodSupported, (uint64_t)return1Imp);
+    RemoteArbCall(proc, method_setImplementation, methodSupported2, (uint64_t)return1Imp);
+    
+    uint64_t workspace = remote_msg(proc, clsSBMainWorkspace, selSharedInstance, 0,0,0,0);
+    uint64_t mainWindowScene = remote_msg(proc, workspace, selMainWindowScene, 0,0,0,0);
+    uint64_t iconController = remote_msg(proc, clsSBIconController, selSharedInstance, 0,0,0,0);
+    if (remote_msg(proc, iconController, selRespondsTo, createFloatingDock, 0,0,0)) {
+        // iOS < 26 has -[SBIconController createFloatingDockControllerForWindowScene:]
+        remote_msg(proc, iconController, selPerform, createFloatingDock, mainWindowScene, 0 /*nil*/, 1 /*YES wait*/);
+    } else {
+        // iOS 26 has -[SBHomeScreenController createFloatingDockControllerForWindowScene:]
+        uint64_t selHomeScreen = remote_sel(proc, "homeScreenController");
+        uint64_t homeScreenController = remote_msg(proc, mainWindowScene, selHomeScreen, 0,0,0,0);
+        remote_msg(proc, homeScreenController, selPerform, createFloatingDock, mainWindowScene, 0 /*nil*/, 1 /*YES wait*/);
+    }
+    
+    // SBMainWorkSpace.sharedInstance.mainWindowScene
+    // +[SBFloatingDockController isFloatingDockUtilitiesSupported]
+    // * frame #0: 0x000000022164406c SpringBoard`-[SBFloatingDockController initWithWindowScene:homeScreenContextProvider:]
+    //   frame #1: 0x0000000221980214 SpringBoard`-[SBHomeScreenController createFloatingDockControllerForWindowScene:] + 68
+    return 0;
+}
+
+int enable_grid_app_switcher(RemoteCall *proc) {
+    uint64_t clsReturn2 = remote_getClass(proc, "SBDeckSwitcherModifier");
+    uint64_t clsSBAppSwitcherSettings = remote_getClass(proc, "SBAppSwitcherSettings");
+    
+    uint64_t selStyle = remote_sel(proc, "switcherStyle");
+    uint64_t selReturn2 = remote_sel(proc, "dockUpdateMode");
+    
+    uint64_t methodSupported = RemoteArbCall(proc, class_getInstanceMethod, clsSBAppSwitcherSettings, selStyle);
+    uint64_t methodReturn2 = RemoteArbCall(proc, class_getInstanceMethod, clsReturn2, selReturn2);
+    
+    uint64_t return2Imp = RemoteArbCall(proc, method_getImplementation, methodReturn2);
+    RemoteArbCall(proc, method_setImplementation, methodSupported, (uint64_t)return2Imp);
+    
+    // <SBSwitcherController: 0x670b5b300>
+    // [[0x670b5b300 contentViewController] personality]
+    // TODO: fix animation?
+    return 0;
+}
+
+int get_performance_hud(RemoteCall *proc) {
+    uint64_t flags = RemoteArbCall(proc, CARenderServerGetDebugFlags, 0);
+    if (flags & CA_DEBUG_FLAG_PERFORMANCE_HUD) {
+        return (int)RemoteArbCall(proc, CARenderServerGetDebugValue, 0, 1);
+    }
+    return -1;
+}
+
+int set_performance_hud(RemoteCall *proc, int selection) {
+    BOOL enabled = selection >= 0;
+    uint64_t flags = RemoteArbCall(proc, CARenderServerGetDebugFlags, 0);
+    if (enabled) {
+        flags |= CA_DEBUG_FLAG_PERFORMANCE_HUD;
+        RemoteArbCall(proc, CARenderServerSetDebugValue, 0, 1, selection);
+    } else {
+        flags &= ~CA_DEBUG_FLAG_PERFORMANCE_HUD;
+    }
+    RemoteArbCall(proc, CARenderServerSetDebugFlags, 0, CA_DEBUG_FLAG_PERFORMANCE_HUD, flags);
+    return 0;
+}
  ```

  </details>
- 🟡 `lara/views/RemoteView.swift` (modified) — +50 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -11,6 +11,7 @@ struct RemoteView: View {
     @ObservedObject var mgr: laramgr
     @State private var running: Bool = false
     @State private var columns: Int = 5
+    @State private var performanceHUD: Int = 0
     @AppStorage("rcdockunlimited") private var rcdockunlimited: Bool = false
 
     private var dockMaxColumns: Int { rcdockunlimited ? 50 : 10 }
@@ -35,7 +36,11 @@ struct RemoteView: View {
                 } label: {
                     Text("Hide Icon Labels")
                 }
+            } header: {
+                Text("SpringBoard")
+            }
 
+            Section {
                 Stepper(value: $columns, in: 1...dockMaxColumns) {
                     HStack {
                         Text("Dock columns")
@@ -59,7 +64,9 @@ struct RemoteView: View {
                 } label: {
                     Text("Apply Dock Columns")
                 }
+            }
 
+            Section {
                 Button {
                     run("Enable Upside Down") {
                         let result = enable_upside_down(mgr.sbProc)
@@ -68,8 +75,49 @@ struct RemoteView: View {
                 } label: {
                     Text("Enable Upside Down")
                 }
-            } header: {
-                Text("SpringBoard")
+            }
+
+            Section {
+                Button {
+                    run("Enable Floating Dock") {
+                        let result = enable_floating_dock(mgr.sbProc)
+                        return "enable_floating_dock() -> \(result)"
+                    }
+                } label: {
+                    Text("Enable Floating Dock (Broken)")
+                }
+                
+                Button {
+                    run("Enable Grid App Switcher") {
+                        let result = enable_grid_app_switcher(mgr.sbProc)
+                        return "enable_grid_app_switcher() -> \(result)"
+                    }
+                } label: {
+                    Text("Enable Grid App Switcher (Broken animation)")
+                }
+            }
+            
+            Section {
+                Picker("Performance HUD", selection: $performanceHUD) {
+                    Text("Off").tag(-1)
+                    Text("Basic").tag(0)
+                    Text("Backdrops").tag(1)
+                    Text("Particles").tag(2)
+                    Text("Full").tag(3)
+                    Text("Power").tag(5)
+                    Text("EDR").tag(7)
+                    Text("Glitches").tag(8)
+                    Text("GPU Time").tag(9)
+                    Text("Memory Bandwidth").tag(10)
+                }
+                .onChange(of: performanceHUD) { newValue in
+                    set_performance_hud(mgr.sbProc, Int32(newValue))
+                }
+                .onAppear {
+                    if mgr.rcrunning {
+                        performanceHUD = Int(get_performance_hud(mgr.sbProc))
+                    }
+                }
             } footer: {
                 Text("These call into SpringBoard via RemoteCall. Keep RemoteCall initialized while running them.")
                 
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit fc27e86: Add experimental tweaks: floating dock, grid switcher, performance HUD
- Apr 20, 2026: 🟣 @rooootdev merged
- Apr 20, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 2:57:26 PM from [https://github.com/rooootdev/lara/pull/137](https://github.com/rooootdev/lara/pull/137)*
