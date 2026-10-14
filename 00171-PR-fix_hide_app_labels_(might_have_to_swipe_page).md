# fix hide app labels (might have to swipe page)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#171](https://github.com/rooootdev/lara/pull/171)  
> **State:** 🟣 merged  
> **Branch:** `Scr-eam:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 23, 2026  
> **Changes:** +48 / -49 across 1 files  
> **Created:** Apr 22, 2026  
> **Updated:** Apr 23, 2026  
> **Closed:** Apr 23, 2026  

---

## Description

*Opened by [@Scr-eam](https://github.com/Scr-eam)* *on Apr 22, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (11)

- [`1f94312`](https://github.com/rooootdev/lara/commit/1f9431202af2c704c23d70ec332bcdc7ed4439fd) Update rc.m — *Scream* · Apr 22, 2026
- [`1b124ec`](https://github.com/rooootdev/lara/commit/1b124ecd378e2fe35aed4effd53607965cf27322) Update rc.m — *Scream* · Apr 22, 2026
- [`52ae22c`](https://github.com/rooootdev/lara/commit/52ae22cdaaf5b3d0d0fdcb2ccda029ef7c2bee9c) Update rc.m — *Scream* · Apr 22, 2026
- [`505f583`](https://github.com/rooootdev/lara/commit/505f5830432b0cae6dcdf6eb236d4b485183bbab) Update rc.m — *Scream* · Apr 22, 2026
- [`29ae7ac`](https://github.com/rooootdev/lara/commit/29ae7acc8aa42a3717e882567276c77a7c0f9506) Update rc.m — *Scream* · Apr 22, 2026
- [`3b1927b`](https://github.com/rooootdev/lara/commit/3b1927b7120712e0db70938bd1e154a5f300d0c0) Update rc.m — *Scream* · Apr 22, 2026
- [`627fc05`](https://github.com/rooootdev/lara/commit/627fc05be82f17751ed60c94a44fd60184074622) add sleeps in between — *Scream* · Apr 22, 2026
- [`f30a1fa`](https://github.com/rooootdev/lara/commit/f30a1fa1c254497031e55eda620d82f2584523b5) Update rc.m — *Scream* · Apr 22, 2026
- [`ae46d92`](https://github.com/rooootdev/lara/commit/ae46d9239d25339455ed5516f10cca8e2bb794b8) Update rc.m — *Scream* · Apr 22, 2026
- [`2b73b36`](https://github.com/rooootdev/lara/commit/2b73b36231e3a8609d54f013bc732b58d58f523f) Update rc.m — *Scream* · Apr 22, 2026
- [`5bfa90a`](https://github.com/rooootdev/lara/commit/5bfa90a2ba240c6e941aa6794d676330d16e840d) Update rc.m — *Scream* · Apr 22, 2026


---

## Changed Files (1)

- 🟡 `lara/kexploit/pe/rc.m` (modified) — +48 / -49
  <details><summary>Diff</summary>

  ```diff
  @@ -128,35 +128,33 @@ int enable_jit(RemoteCall *proc, const char *bid) {
 static int hide_labels_in_view(RemoteCall *proc, uint64_t parentView) {
     if (!parentView) return 0;
 
-    uint64_t selSubviews   = remote_sel(proc, "subviews");
-    uint64_t selCount      = remote_sel(proc, "count");
-    uint64_t selObjAtIdx   = remote_sel(proc, "objectAtIndex:");
-    uint64_t selSetAllows  = remote_sel(proc, "setAllowsLabelArea:");
-    uint64_t selResponds   = remote_sel(proc, "respondsToSelector:");
+    uint64_t selSubviews    = remote_sel(proc, "subviews");
+    uint64_t selCount       = remote_sel(proc, "count");
+    uint64_t selObjAtIdx    = remote_sel(proc, "objectAtIndex:");
+    uint64_t selSetAllows   = remote_sel(proc, "setAllowsLabelArea:");
+    uint64_t selResponds    = remote_sel(proc, "respondsToSelector:");
+    uint64_t selPerform     = remote_sel(proc, "performSelectorOnMainThread:withObject:waitUntilDone:");
+    uint64_t selNeedsLayout = remote_sel(proc, "setNeedsLayout");
 
     uint64_t subviews = remote_msg(proc, parentView, selSubviews, 0,0,0,0);
     if (!subviews) return 0;
+    
     uint64_t count = remote_msg(proc, subviews, selCount, 0,0,0,0);
     int hidden = 0;
 
-    for (uint64_t i = 0; i < count && i < 200; i++) {
+    for (uint64_t i = 0; i < count; i++) {
         uint64_t view = remote_msg(proc, subviews, selObjAtIdx, i, 0,0,0);
         if (!view) continue;
-        uint64_t selPerform = remote_sel(proc, "performSelectorOnMainThread:withObject:waitUntilDone:");
+        
         if (remote_msg(proc, view, selResponds, selSetAllows, 0,0,0)) {
-            remote_msg(proc, view, selPerform, selSetAllows, 0 /*nil=NO*/, 1 /*YES wait*/, 0);
+            remote_msg(proc, view, selPerform, selSetAllows, 0 /* NO */, 1 /* YES wait */, 0);
+            remote_msg(proc, view, selPerform, selNeedsLayout, 0, 1, 0);
             hidden++;
         }
-        uint64_t inner = remote_msg(proc, view, selSubviews, 0,0,0,0);
-        uint64_t innerCount = remote_msg(proc, inner, selCount, 0,0,0,0);
-        for (uint64_t j = 0; j < innerCount && j < 50; j++) {
-            uint64_t v2 = remote_msg(proc, inner, selObjAtIdx, j, 0,0,0);
-            if (v2 && remote_msg(proc, v2, selResponds, selSetAllows, 0,0,0)) {
-                remote_msg(proc, v2, selPerform, selSetAllows, 0, 1, 0);
-                hidden++;
-            }
-        }
+        
+        hidden += hide_labels_in_view(proc, view);
     }
+    
     return hidden;
 }
 
@@ -165,8 +163,36 @@ int hide_icon_labels(RemoteCall *proc) {
         printf("(rc) RemoteCall not initialized\n");
         return -1;
     }
+
     printf("(rc) Hiding icon labels natively...\n");
 
+    uint64_t clsNSObject = remote_getClass(proc, "NSObject");
+    uint64_t clsSBIconView = remote_getClass(proc, "SBIconView");
+    
+    uint64_t methodSetAllows = 0;
+    uint64_t impNoOp = 0;
+
+    if (clsNSObject && clsSBIconView) {
+        uint64_t selIsProxy = remote_sel(proc, "isProxy");
+        uint64_t methodReturn0 = RemoteArbCall(proc, class_getInstanceMethod, clsNSObject, selIsProxy);
+        uint64_t impReturn0 = RemoteArbCall(proc, method_getImplementation, methodReturn0);
+
+        uint64_t selAllowsGet = remote_sel(proc, "allowsLabelArea");
+        uint64_t methodAllowsGet = RemoteArbCall(proc, class_getInstanceMethod, clsSBIconView, selAllowsGet);
+        if (methodAllowsGet && impReturn0) {
+            RemoteArbCall(proc, method_setImplementation, methodAllowsGet, impReturn0);
+        }
+
+        uint64_t selSelf = remote_sel(proc, "self");
+        uint64_t methodNoOp = RemoteArbCall(proc, class_getInstanceMethod, clsNSObject, selSelf);
+        impNoOp = RemoteArbCall(proc, method_getImplementation, methodNoOp);
+        
+        uint64_t selSetAllows = remote_sel(proc, "setAllowsLabelArea:");
+        methodSetAllows = RemoteArbCall(proc, class_getInstanceMethod, clsSBIconView, selSetAllows);
+    }
+
+    usleep(50000);
+
     uint64_t selShared   = remote_sel(proc, "sharedInstance");
     uint64_t selIconMgr  = remote_sel(proc, "iconManager");
     uint64_t selDock     = remote_sel(proc, "dockListView");
@@ -175,17 +201,15 @@ int hide_icon_labels(RemoteCall *proc) {
 
     uint64_t ctrlCls = remote_getClass(proc, "SBIconController");
     uint64_t ctrl = remote_msg(proc, ctrlCls, selShared, 0,0,0,0);
-    if (!ctrl) { printf("(rc) SBIconController not found\n"); return -1; }
+    if (!ctrl) return -1;
     
     uint64_t mgr = remote_msg(proc, ctrl, selIconMgr, 0,0,0,0);
     int totalHidden = 0;
 
     uint64_t dock = 0;
     if (mgr) dock = remote_msg(proc, mgr, selDock, 0,0,0,0);
     if (!dock) dock = remote_msg(proc, ctrl, selDock, 0,0,0,0);
-    if (dock) {
-        totalHidden += hide_labels_in_view(proc, dock);
-    }
+    if (dock) totalHidden += hide_labels_in_view(proc, dock);
 
     uint64_t selRootFC    = remote_sel(proc, "rootFolderController");
     uint64_t selListViews = remote_sel(proc, "iconListViews");
@@ -200,35 +224,10 @@ int hide_icon_labels(RemoteCall *proc) {
             totalHidden += hide_labels_in_view(proc, listView);
         }
     }
-    printf("(rc) Initial hidden labels: %d\n", totalHidden);
 
-    uint64_t clsNSObject = remote_getClass(proc, "NSObject");
-    uint64_t clsSBIconView = remote_getClass(proc, "SBIconView");
-    
-    if (clsNSObject && clsSBIconView) {
-        uint64_t selIsProxy = remote_sel(proc, "isProxy");
-        uint64_t methodReturn0 = RemoteArbCall(proc, class_getInstanceMethod, clsNSObject, selIsProxy);
-        uint64_t impReturn0 = RemoteArbCall(proc, method_getImplementation, methodReturn0);
-        
-        uint64_t selSelf = remote_sel(proc, "self");
-        uint64_t methodNoOp = RemoteArbCall(proc, class_getInstanceMethod, clsNSObject, selSelf);
-        uint64_t impNoOp = RemoteArbCall(proc, method_getImplementation, methodNoOp);
-
-        uint64_t selAllowsGet = remote_sel(proc, "allowsLabelArea");
-        uint64_t methodAllowsGet = RemoteArbCall(proc, class_getInstanceMethod, clsSBIconView, selAllowsGet);
-        if (methodAllowsGet && impReturn0) {
-            RemoteArbCall(proc, method_setImplementation, methodAllowsGet, impReturn0);
-            printf("(rc) Swizzled allowsLabelArea to return NO\n");
-        }
-
-        uint64_t selSetAllows = remote_sel(proc, "setAllowsLabelArea:");
-        uint64_t methodSetAllows = RemoteArbCall(proc, class_getInstanceMethod, clsSBIconView, selSetAllows);
-        if (methodSetAllows && impNoOp) {
-            RemoteArbCall(proc, method_setImplementation, methodSetAllows, impNoOp);
-            printf("(rc) Swizzled setAllowsLabelArea: to no-op\n");
-        }
-    } else {
-        printf("(rc) Failed to find SBIconView or NSObject for swizzling\n");
+    if (methodSetAllows && impNoOp) {
+        RemoteArbCall(proc, method_setImplementation, methodSetAllows, impNoOp);
+        printf("(rc) App labels should now be hidden!\n");
     }
 
     return totalHidden;
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 1f94312: Update rc.m
- : 📝 Commit 1b124ec: Update rc.m
- : 📝 Commit 52ae22c: Update rc.m
- : 📝 Commit 505f583: Update rc.m
- : 📝 Commit 29ae7ac: Update rc.m
- : 📝 Commit 3b1927b: Update rc.m
- : 📝 Commit 627fc05: add sleeps in between
- : 📝 Commit f30a1fa: Update rc.m
- : 📝 Commit ae46d92: Update rc.m
- : 📝 Commit 2b73b36: Update rc.m
- : 📝 Commit 5bfa90a: Update rc.m
- Apr 23, 2026: 🟣 @rooootdev merged
- Apr 23, 2026: 🔴 Closed by @rooootdev


---

## Comments (1)

### Comment #1 — [@Scr-eam](https://github.com/Scr-eam) · Apr 23, 2026

`ID: 4301004583`


i tried to get it to work without having to change page for yall but couldn't lol, so just switch to a diff app page and they'll be hidden (if you only have 1 page of apps or are on the first page they'll automatically hide)


---

*Exported on 10/14/2026, 3:00:21 PM from [https://github.com/rooootdev/lara/pull/171](https://github.com/rooootdev/lara/pull/171)*
