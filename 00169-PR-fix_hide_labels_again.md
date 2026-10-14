# fix hide labels again

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#169](https://github.com/rooootdev/lara/pull/169)  
> **State:** 🔴 closed  
> **Branch:** `Scr-eam:main` → `rooootdev:main`  
> **Changes:** +63 / -46 across 1 files  
> **Created:** Apr 22, 2026  
> **Updated:** Apr 22, 2026  
> **Closed:** Apr 22, 2026  

---

## Description

*Opened by [@Scr-eam](https://github.com/Scr-eam)* *on Apr 22, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (18)

- [`df804d1`](https://github.com/rooootdev/lara/commit/df804d1a10db970b2b1211680b577d7ff1d39231) try to fix labels — *Scream* · Apr 22, 2026
- [`76dc4f9`](https://github.com/rooootdev/lara/commit/76dc4f90a08ff33428266bdc69ad88e7944ce594) test #2 — *Scream* · Apr 22, 2026
- [`663aaad`](https://github.com/rooootdev/lara/commit/663aaadf269e784e8c04cd220a88ab6a6c6bbefc) a — *Scream* · Apr 22, 2026
- [`99f2544`](https://github.com/rooootdev/lara/commit/99f254456cd10d4f523464fd319f6a7761022daa) fully fix hide labels again — *Scream* · Apr 22, 2026
- [`75f1c49`](https://github.com/rooootdev/lara/commit/75f1c49bf84324685a8d5d4cdc502530920de71b) now do apps instantly hide? — *Scream* · Apr 22, 2026
- [`1272257`](https://github.com/rooootdev/lara/commit/127225738ef9810685ecd6412ae721b768aa8561) Merge branch 'rooootdev:main' into main — *Scream* · Apr 22, 2026
- [`7a73703`](https://github.com/rooootdev/lara/commit/7a73703b219967eb3f6a259e1dd7d5cd28a49e53) app labels now instantly and permanently hidden? — *Scream* · Apr 22, 2026
- [`89b364e`](https://github.com/rooootdev/lara/commit/89b364e4974308767edc91e82c07be74fe8dd85b) fix respring? — *Scream* · Apr 22, 2026
- [`790fbf4`](https://github.com/rooootdev/lara/commit/790fbf41a7b2715b9954ce67a992a24cd4bb32cc) use original hide_labels_in_view — *Scream* · Apr 22, 2026
- [`fac6e40`](https://github.com/rooootdev/lara/commit/fac6e40e917aaf04e6e33e07f5cc6e0477e8095a) fix view — *Scream* · Apr 22, 2026
- [`1068dd4`](https://github.com/rooootdev/lara/commit/1068dd481ff3605cbb726ff39ba598c00a252ade) Merge branch 'rooootdev:main' into main — *Scream* · Apr 22, 2026
- [`7d2e9d5`](https://github.com/rooootdev/lara/commit/7d2e9d5abd35075332aeb7ec0ff31adc79da7106) commit didn’t go through properly — *Scream* · Apr 22, 2026
- [`d237cb6`](https://github.com/rooootdev/lara/commit/d237cb629642d92fff9bb31df10a482388b26ae3) fix hide_icon_labels — *Scream* · Apr 22, 2026
- [`d752557`](https://github.com/rooootdev/lara/commit/d752557dd350ee037fee16a527d784045eba6498) last attempt at making this work without swipe — *Scream* · Apr 22, 2026
- [`789cdc9`](https://github.com/rooootdev/lara/commit/789cdc93d5750720b4554a7d2b537d7efe2541bf) fix only instantly hiding on main home page — *Scream* · Apr 22, 2026
- [`b01ae07`](https://github.com/rooootdev/lara/commit/b01ae07d2fa2f8190b55b09cf77a5e9167214d6e) Update rc.m — *Scream* · Apr 22, 2026
- [`301f19b`](https://github.com/rooootdev/lara/commit/301f19bb7aa85b7a0f4964885478dd46536184f9) Update rc.m — *Scream* · Apr 22, 2026
- [`8d43a0e`](https://github.com/rooootdev/lara/commit/8d43a0e62efce9064ce08c183556859d1d93d411) Update rc.m — *Scream* · Apr 22, 2026


---

## Changed Files (1)

- 🟡 `lara/kexploit/pe/rc.m` (modified) — +63 / -46
  <details><summary>Diff</summary>

  ```diff
  @@ -128,11 +128,15 @@ int enable_jit(RemoteCall *proc, const char *bid) {
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
+    
+    uint64_t selNeedsLayout = remote_sel(proc, "setNeedsLayout");
+    uint64_t selLayoutIf    = remote_sel(proc, "layoutIfNeeded");
 
     uint64_t subviews = remote_msg(proc, parentView, selSubviews, 0,0,0,0);
     if (!subviews) return 0;
@@ -142,53 +146,81 @@ static int hide_labels_in_view(RemoteCall *proc, uint64_t parentView) {
     for (uint64_t i = 0; i < count && i < 200; i++) {
         uint64_t view = remote_msg(proc, subviews, selObjAtIdx, i, 0,0,0);
         if (!view) continue;
-        uint64_t selPerform = remote_sel(proc, "performSelectorOnMainThread:withObject:waitUntilDone:");
+        
         if (remote_msg(proc, view, selResponds, selSetAllows, 0,0,0)) {
-            remote_msg(proc, view, selPerform, selSetAllows, 0 /*nil=NO*/, 1 /*YES wait*/, 0);
+            remote_msg(proc, view, selPerform, selSetAllows, 0, 1, 0);
+            remote_msg(proc, view, selPerform, selNeedsLayout, 0, 1, 0);
             hidden++;
         }
+        
         uint64_t inner = remote_msg(proc, view, selSubviews, 0,0,0,0);
         uint64_t innerCount = remote_msg(proc, inner, selCount, 0,0,0,0);
         for (uint64_t j = 0; j < innerCount && j < 50; j++) {
             uint64_t v2 = remote_msg(proc, inner, selObjAtIdx, j, 0,0,0);
             if (v2 && remote_msg(proc, v2, selResponds, selSetAllows, 0,0,0)) {
                 remote_msg(proc, v2, selPerform, selSetAllows, 0, 1, 0);
+                remote_msg(proc, v2, selPerform, selNeedsLayout, 0, 1, 0);
                 hidden++;
             }
         }
     }
+
+    remote_msg(proc, parentView, selPerform, selNeedsLayout, 0, 1, 0);
+    remote_msg(proc, parentView, selPerform, selLayoutIf, 0, 1, 0);
+
     return hidden;
 }
 
+here, i “totally” coded this for you. ai def didn’t do it <:Trolleystore:1490889639970472010> 
+
+
 int hide_icon_labels(RemoteCall *proc) {
     if (!proc) {
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
-    uint64_t selDock     = remote_sel(proc, "dockListView");
-    uint64_t selCount    = remote_sel(proc, "count");
-    uint64_t selObjAtIdx = remote_sel(proc, "objectAtIndex:");
-
+    uint64_t selRootFC   = remote_sel(proc, "rootFolderController");
+    
     uint64_t ctrlCls = remote_getClass(proc, "SBIconController");
     uint64_t ctrl = remote_msg(proc, ctrlCls, selShared, 0,0,0,0);
-    if (!ctrl) { printf("(rc) SBIconController not found\n"); return -1; }
+    if (!ctrl) return -1;
     
     uint64_t mgr = remote_msg(proc, ctrl, selIconMgr, 0,0,0,0);
-    int totalHidden = 0;
 
-    uint64_t dock = 0;
-    if (mgr) dock = remote_msg(proc, mgr, selDock, 0,0,0,0);
-    if (!dock) dock = remote_msg(proc, ctrl, selDock, 0,0,0,0);
-    if (dock) {
-        totalHidden += hide_labels_in_view(proc, dock);
-    }
+    int totalHidden = 0;
 
-    uint64_t selRootFC    = remote_sel(proc, "rootFolderController");
-    uint64_t selListViews = remote_sel(proc, "iconListViews");
     uint64_t rootFC = 0;
     if (mgr) rootFC = remote_msg(proc, mgr, selRootFC, 0,0,0,0);
 
@@ -200,35 +232,20 @@ int hide_icon_labels(RemoteCall *proc) {
             totalHidden += hide_labels_in_view(proc, listView);
         }
     }
+
     printf("(rc) Initial hidden labels: %d\n", totalHidden);
 
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
+    uint64_t selSetNeeds = remote_sel(proc, "setNeedsLayout");
+    uint64_t selLayoutIf = remote_sel(proc, "layoutIfNeeded");
+    uint64_t selPerform  = remote_sel(proc, "performSelectorOnMainThread:withObject:waitUntilDone:");
 
-        uint64_t selAllowsGet = remote_sel(proc, "allowsLabelArea");
-        uint64_t methodAllowsGet = RemoteArbCall(proc, class_getInstanceMethod, clsSBIconView, selAllowsGet);
-        if (methodAllowsGet && impReturn0) {
-            RemoteArbCall(proc, method_setImplementation, methodAllowsGet, impReturn0);
-            printf("(rc) Swizzled allowsLabelArea to return NO\n");
-        }
+    if (contentView) {
+       remote_msg(proc, contentView, selPerform, selSetNeeds, 0, 1, 0);
+       remote_msg(proc, contentView, selPerform, selLayoutIf, 0, 1, 0);
+    }
 
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
     }
 
     return totalHidden;
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit df804d1: try to fix labels
- : 📝 Commit 76dc4f9: test #2
- : 📝 Commit 663aaad: a
- : 📝 Commit 99f2544: fully fix hide labels again
- : 📝 Commit 75f1c49: now do apps instantly hide?
- : 📝 Commit 1272257: Merge branch 'rooootdev:main' into main
- Apr 22, 2026: 🔴 Closed by @Scr-eam
- : 📝 Commit 7a73703: app labels now instantly and permanently hidden?
- : 📝 Commit 89b364e: fix respring?
- : 📝 Commit 790fbf4: use original hide_labels_in_view
- : 📝 Commit fac6e40: fix view
- : 📝 Commit 1068dd4: Merge branch 'rooootdev:main' into main
- : 📝 Commit 7d2e9d5: commit didn’t go through properly
- : 📝 Commit d237cb6: fix hide_icon_labels
- : 📝 Commit d752557: last attempt at making this work without swipe
- : 📝 Commit 789cdc9: fix only instantly hiding on main home page
- : 📝 Commit b01ae07: Update rc.m
- : 📝 Commit 301f19b: Update rc.m
- : 📝 Commit 8d43a0e: Update rc.m
- Apr 22, 2026: 🟢 Reopened by @Scr-eam
- Apr 22, 2026: 🔴 Closed by @Scr-eam


---

## Comments (1)

### Comment #1 — [@Scr-eam](https://github.com/Scr-eam) · Apr 22, 2026

`ID: 4297550779`


doing some quick fix


---

*Exported on 10/14/2026, 3:00:07 PM from [https://github.com/rooootdev/lara/pull/169](https://github.com/rooootdev/lara/pull/169)*
