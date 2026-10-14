# Fix hide app labels

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#161](https://github.com/rooootdev/lara/pull/161)  
> **State:** 🔴 closed  
> **Branch:** `Scr-eam:main` → `rooootdev:main`  
> **Changes:** +42 / -8 across 1 files  
> **Created:** Apr 21, 2026  
> **Updated:** Apr 21, 2026  
> **Closed:** Apr 21, 2026  

---

## Description

*Opened by [@Scr-eam](https://github.com/Scr-eam)* *on Apr 21, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (3)

- [`834cc18`](https://github.com/rooootdev/lara/commit/834cc186472434490e5a84181f51d916b29f51e3) Update RemoteCall.m — *Scream* · Apr 21, 2026
- [`76257e7`](https://github.com/rooootdev/lara/commit/76257e73d35c1879f0cd7b6250971a301572e5e8) Update source.json — *Scream* · Apr 21, 2026
- [`cf1af9a`](https://github.com/rooootdev/lara/commit/cf1af9a47e8ca34e3933164424077d569fd8d232) Update source.json — *Scream* · Apr 21, 2026


---

## Changed Files (1)

- 🟡 `lara/kexploit/TaskRop/RemoteCall.m` (modified) — +42 / -8
  <details><summary>Diff</summary>

  ```diff
  @@ -1016,8 +1016,11 @@ int hide_icon_labels(RemoteCall *proc) {
         printf("(rc) RemoteCall not initialized\n");
         return -1;
     }
-    printf("(rc) Hiding icon labels...\n");
+    printf("(rc) Hiding icon labels natively...\n");
 
+    // ==========================================
+    // 1. Hide Currently Visible Labels First
+    // ==========================================
     uint64_t selShared   = remote_sel(proc, "sharedInstance");
     uint64_t selIconMgr  = remote_sel(proc, "iconManager");
     uint64_t selDock     = remote_sel(proc, "dockListView");
@@ -1027,37 +1030,68 @@ int hide_icon_labels(RemoteCall *proc) {
     uint64_t ctrlCls = remote_getClass(proc, "SBIconController");
     uint64_t ctrl = remote_msg(proc, ctrlCls, selShared, 0,0,0,0);
     if (!ctrl) { printf("(rc) SBIconController not found\n"); return -1; }
-    printf("(rc) SBIconController = 0x%llx\n", ctrl);
-
+    
     uint64_t mgr = remote_msg(proc, ctrl, selIconMgr, 0,0,0,0);
     int totalHidden = 0;
 
     uint64_t dock = 0;
     if (mgr) dock = remote_msg(proc, mgr, selDock, 0,0,0,0);
     if (!dock) dock = remote_msg(proc, ctrl, selDock, 0,0,0,0);
     if (dock) {
-        printf("(rc) dock = 0x%llx\n", dock);
         totalHidden += hide_labels_in_view(proc, dock);
     }
 
     uint64_t selRootFC    = remote_sel(proc, "rootFolderController");
     uint64_t selListViews = remote_sel(proc, "iconListViews");
     uint64_t rootFC = 0;
     if (mgr) rootFC = remote_msg(proc, mgr, selRootFC, 0,0,0,0);
-    printf("(rc) rootFolderController = 0x%llx\n", rootFC);
 
     if (rootFC) {
         uint64_t listViews = remote_msg(proc, rootFC, selListViews, 0,0,0,0);
         uint64_t listCount = remote_msg(proc, listViews, selCount, 0,0,0,0);
-        printf("(rc) %llu icon list views\n", listCount);
-
         for (uint64_t i = 0; i < listCount && i < 20; i++) {
             uint64_t listView = remote_msg(proc, listViews, selObjAtIdx, i, 0,0,0);
             totalHidden += hide_labels_in_view(proc, listView);
         }
     }
+    printf("(rc) Initial hidden labels: %d\n", totalHidden);
+
+    // ==========================================
+    // 2. Swizzle SBIconView to make it permanent
+    // ==========================================
+    uint64_t clsNSObject = remote_getClass(proc, "NSObject");
+    uint64_t clsSBIconView = remote_getClass(proc, "SBIconView");
+    
+    if (clsNSObject && clsSBIconView) {
+        // Prepare Gadget 1: -[NSObject isProxy] -> Returns NO (0)
+        uint64_t selIsProxy = remote_sel(proc, "isProxy");
+        uint64_t methodReturn0 = RemoteArbCall(proc, class_getInstanceMethod, clsNSObject, selIsProxy);
+        uint64_t impReturn0 = RemoteArbCall(proc, method_getImplementation, methodReturn0);
+
+        // Prepare Gadget 2: -[NSObject self] -> Returns self, ignores W2 arg (Perfect No-Op)
+        uint64_t selSelf = remote_sel(proc, "self");
+        uint64_t methodNoOp = RemoteArbCall(proc, class_getInstanceMethod, clsNSObject, selSelf);
+        uint64_t impNoOp = RemoteArbCall(proc, method_getImplementation, methodNoOp);
+
+        // Hook Getter: -[SBIconView allowsLabelArea] -> Force return NO
+        uint64_t selAllowsGet = remote_sel(proc, "allowsLabelArea");
+        uint64_t methodAllowsGet = RemoteArbCall(proc, class_getInstanceMethod, clsSBIconView, selAllowsGet);
+        if (methodAllowsGet && impReturn0) {
+            RemoteArbCall(proc, method_setImplementation, methodAllowsGet, impReturn0);
+            printf("(rc) Swizzled allowsLabelArea to return NO\n");
+        }
+
+        // Hook Setter: -[SBIconView setAllowsLabelArea:] -> Nuke the setter
+        uint64_t selSetAllows = remote_sel(proc, "setAllowsLabelArea:");
+        uint64_t methodSetAllows = RemoteArbCall(proc, class_getInstanceMethod, clsSBIconView, selSetAllows);
+        if (methodSetAllows && impNoOp) {
+            RemoteArbCall(proc, method_setImplementation, methodSetAllows, impNoOp);
+            printf("(rc) Swizzled setAllowsLabelArea: to no-op\n");
+        }
+    } else {
+        printf("(rc) Failed to find SBIconView or NSObject for swizzling\n");
+    }
 
-    printf("(rc) Hidden labels on %d icon views\n", totalHidden);
     return totalHidden;
 }
 
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 834cc18: Update RemoteCall.m
- : 📝 Commit 76257e7: Update source.json
- : 📝 Commit cf1af9a: Update source.json
- Apr 21, 2026: 🔴 Closed by @rooootdev


---

## Comments (2)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Apr 21, 2026

`ID: 4290952899`


fire


---

### Comment #2 — [@rooootdev](https://github.com/rooootdev) · Apr 21, 2026

`ID: 4290986807`


cant directly merge. changes implemented, you will be credited in the view.


---

*Exported on 10/14/2026, 2:59:14 PM from [https://github.com/rooootdev/lara/pull/161](https://github.com/rooootdev/lara/pull/161)*
