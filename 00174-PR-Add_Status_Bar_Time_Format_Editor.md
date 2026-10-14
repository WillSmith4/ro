# Add Status Bar Time Format Editor

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#174](https://github.com/rooootdev/lara/pull/174)  
> **State:** 🟣 merged  
> **Branch:** `mrtacoguy01:status-bar-time-editor` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 23, 2026  
> **Changes:** +17 / -7 across 3 files  
> **Created:** Apr 23, 2026  
> **Updated:** Apr 23, 2026  
> **Closed:** Apr 23, 2026  

---

## Description

*Opened by [@mrtacoguy01](https://github.com/mrtacoguy01)* *on Apr 23, 2026*


Tested on iPhone 14 (iOS 18.1 22B5045g)
<img width="256" height="auto" alt="Screenshot 2026-04-22 at 9 56 58 PM" src="https://github.com/user-attachments/assets/c31347f3-0664-434a-aab6-0c4b34663e3d" />


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24812573692/job/72620412010) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24812573692/job/72620321647) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`3dcba88`](https://github.com/rooootdev/lara/commit/3dcba88277924ad8d8032a028a7f983099d44031) Add Status Bar Time Format Editor — *t* · Apr 23, 2026


---

## Changed Files (3)

- 🟡 `lara/kexploit/pe/rc.h` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -12,7 +12,7 @@
 
 typedef void (^rc_euenabler_callback_t)(double progress);
 
-void status_bar_tweak(RemoteCall *proc);
+void status_bar_time_format(RemoteCall *proc, const char *dateFormat);
 int hide_icon_labels(RemoteCall *proc);
 int enable_jit(RemoteCall *proc, const char *bundleID);
 int set_dock_icon_count(RemoteCall *proc, int count);
  ```

  </details>
- 🟡 `lara/kexploit/pe/rc.m` (modified) — +2 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -25,16 +25,15 @@
 #define PT_DETACH       11
 #define PT_ATTACHEXC    14
 
-void status_bar_tweak(RemoteCall *proc) {
+void status_bar_time_format(RemoteCall *proc, const char *dateFormat) {
     if (!proc) {
         printf("(rc) RemoteCall not initialized\n");
         return;
     }
 
     printf("(rc) Adding heart beside time...\n");
 
-    const char *heartFmt = "HH:mm - E d/M/yyyy";
-    uint64_t fmtNSStr = remote_NSString(proc, heartFmt);
+    uint64_t fmtNSStr = remote_NSString(proc, dateFormat);
 
     uint64_t selSharedApp   = remote_sel(proc, "sharedApplication");
     uint64_t selAggregator  = remote_sel(proc, "statusBarStateAggregator");
  ```

  </details>
- 🟡 `lara/views/RemoteView.swift` (modified) — +14 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -10,6 +10,7 @@ import Darwin
 
 struct RemoteView: View {
     @ObservedObject var mgr: laramgr
+    @State private var statusBarTimeFormat: String = "HH:mm"
     @State private var running: Bool = false
     @State private var columns: Int = 5
     @State private var performanceHUD: Int = -1
@@ -26,15 +27,25 @@ struct RemoteView: View {
     var body: some View {
         List {
             Section {
+                TextField("Date format (e.g. HH:mm)", text: $statusBarTimeFormat)
+                    .autocorrectionDisabled()
+                    .textInputAutocapitalization(.never)
+
                 Button {
                     run("Status Bar Time Format") {
-                        status_bar_tweak(mgr.sbProc)
-                        return "status_bar_tweak() done"
+                        status_bar_time_format(mgr.sbProc, statusBarTimeFormat)
+                        return "status_bar_time_format() done"
                     }
                 } label: {
-                    Text("Status Bar Time Format")
+                    Text("Apply")
                 }
+            } header: {
+                Text("Status Bar Time Format")
+            } footer: {
+                Text("The text automatically updates every MINUTE")
+            }
 
+            Section {
                 Button {
                     run("Hide Icon Labels") {
                         let hidden = hide_icon_labels(mgr.sbProc)
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 3dcba88: Add Status Bar Time Format Editor
- Apr 23, 2026: 🟣 @rooootdev merged
- Apr 23, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:00:34 PM from [https://github.com/rooootdev/lara/pull/174](https://github.com/rooootdev/lara/pull/174)*
