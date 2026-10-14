# fix: Lara not working in Livecontainer on ios 17.1

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#86](https://github.com/rooootdev/lara/pull/86)  
> **State:** 🟣 merged  
> **Branch:** `m1337v:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 12, 2026  
> **Changes:** +101 / -12 across 5 files  
> **Created:** Apr 12, 2026  
> **Updated:** Apr 12, 2026  
> **Closed:** Apr 12, 2026  

---

## Description

*Opened by [@m1337v](https://github.com/m1337v)* *on Apr 12, 2026*


Fixes LiveContainer follow-up failures caused by stale cached proc/task state and early fallback/default values, by retrying proc/task recovery only in the affected LiveContainer paths instead of trusting the initial Darksword cache. (#85)


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (1)

- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24313175011/job/70986231586) — **success** — *GitHub Actions*


---

## PR Commits (3)

- [`02691af`](https://github.com/rooootdev/lara/commit/02691afb9c17b12cb29abb5c2d0b5e963f4775d1) fix: avoid stale proc/task state during LiveContainer VFS init — *Morpheus* · Apr 12, 2026
- [`fa328cf`](https://github.com/rooootdev/lara/commit/fa328cfd6bddf44b67d864d5a3796417c3fc1e79) fix: avoid stale proc/task reads in LiveContainer follow-up paths — *Morpheus* · Apr 12, 2026
- [`8e0653a`](https://github.com/rooootdev/lara/commit/8e0653a02f0b682423ce56df29a4dae1bc21a418) fix: don't touch release for PR builds but upload artifact — *Morpheus* · Apr 12, 2026


---

## Changed Files (5)

- 🟡 `.github/workflows/build.yml` (modified) — +13 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -46,13 +46,24 @@ jobs:
           cp -R "$APP_PATH" "$PWD/build/Payload/"
           (cd "$PWD/build" && /usr/bin/zip -qry lara.ipa Payload)
 
+      - name: upload build artifact
+        if: github.event_name == 'pull_request'
+        uses: actions/upload-artifact@v4
+        with:
+          name: lara-ipa
+          path: |
+            build/lara.ipa
+            build.log
+
       - name: create release tag
+        if: github.event_name != 'pull_request'
         id: tag
         run: |
           TAG="build_$(date +'%Y%m%d%H%M%S')-$(git rev-parse --short HEAD)"
           echo "TAG=$TAG" >> $GITHUB_ENV
 
       - name: get last 12h commits
+        if: github.event_name != 'pull_request'
         id: commits
         run: |
           COMMITS=$(git log --since="12 hours ago" --pretty=format:"- %h %s (%an)")
@@ -65,6 +76,7 @@ jobs:
           echo "EOF" >> $GITHUB_ENV
 
       - name: delete old release
+        if: github.event_name != 'pull_request'
         uses: actions/github-script@v7
         with:
           github-token: ${{ secrets.GITHUB_TOKEN }}
@@ -84,6 +96,7 @@ jobs:
             }
 
       - name: create new release
+        if: github.event_name != 'pull_request'
         uses: softprops/action-gh-release@v1
         with:
           tag_name: latest
  ```

  </details>
- 🟡 `lara/kexploit/pe/apfs.m` (modified) — +28 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -7,6 +7,7 @@
 
 #import "darksword.h"
 #import "offsets.h"
+#import "utils.h"
 #import "xpaci.h"
 
 #include <stdint.h>
@@ -151,12 +152,38 @@
 uint64_t getvnodefor(const char *path) {
     int fd = open(path, O_RDONLY);
     if (fd == -1) return -1;
+
+    uint64_t self_proc = ds_get_our_proc();
+    if (!self_proc && is_likely_livecontainer_runtime()) {
+        self_proc = ourproc();
+        if (!self_proc) {
+            printf("(apfs) failed to recover our proc in LiveContainer\n");
+            close(fd);
+            return -1;
+        }
+    }
+    if (!self_proc) {
+        close(fd);
+        return -1;
+    }
     
-    uint64_t fileprocPtrArr = ds_kread64(ds_get_our_proc() + off_proc_p_fd + off_filedesc_fd_ofiles);
+    uint64_t fileprocPtrArr = ds_kread64(self_proc + off_proc_p_fd + off_filedesc_fd_ofiles);
     fileprocPtrArr = xpaci(fileprocPtrArr);
+    if (!fileprocPtrArr) {
+        close(fd);
+        return -1;
+    }
     uint64_t fileproc = ds_kread64(fileprocPtrArr + (8 * fd));
+    if (!fileproc) {
+        close(fd);
+        return -1;
+    }
     uint64_t fp_glob = ds_kread64(fileproc + off_fileproc_fp_glob);
     fp_glob = xpaci(fp_glob);
+    if (!fp_glob) {
+        close(fd);
+        return -1;
+    }
     uint64_t vnode = ds_kread64(fp_glob + off_fileglob_fg_data);
     vnode = xpaci(vnode);
     
  ```

  </details>
- 🟡 `lara/kexploit/pe/sbx.m` (modified) — +15 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -308,6 +308,12 @@ int sbx_escape(uint64_t self_proc) {
 
 int sbx_elevate(void) {
     uint64_t launchd = procbyname("launchd");
+    if (!launchd && is_likely_livecontainer_runtime()) {
+        launchd = procbypid(1);
+        if (launchd) {
+            sbx_log("resolved launchd via pid 1 fallback: 0x%llx", launchd);
+        }
+    }
     if (!launchd) {
         sbx_log("could not find launchd");
         return -1;
@@ -320,18 +326,22 @@ int sbx_elevate(void) {
     }
     sbx_log("launchd ucred: 0x%llx", launchducred);
     
-    uint64_t ourproc = ds_get_our_proc();
-    if (!ourproc) {
+    uint64_t self_proc = ds_get_our_proc();
+    if (!self_proc && is_likely_livecontainer_runtime()) {
+        sbx_log("ds_get_our_proc() returned 0x0 during elevate; trying ourproc() fallback...");
+        self_proc = ourproc();
+    }
+    if (!self_proc) {
         sbx_log("failed to get our proc");
         return -1;
     }
-    sbx_log("ourproc: 0x%llx", ourproc);
+    sbx_log("ourproc: 0x%llx", self_proc);
     
-    uint64_t ourucredraw = ds_kread64(ourproc + 0x10);
+    uint64_t ourucredraw = ds_kread64(self_proc + 0x10);
     uint64_t ourucred = S(ourucredraw);
     sbx_log("ourucred: 0x%llx", ourucred);
     
-    ds_kwrite64(ourproc + 0x10, launchducred);
+    ds_kwrite64(self_proc + 0x10, launchducred);
     
     if (getuid() == 0) {
         sbx_log("elevate success!");
  ```

  </details>
- 🟡 `lara/kexploit/pe/vfs.m` (modified) — +44 / -6
  <details><summary>Diff</summary>

  ```diff
  @@ -195,6 +195,40 @@ static void vfs_progress(double progress) {
 
 static NSString *const krootvnodekey = @"lara.rootvnode_offset";
 
+static uint64_t vfs_effective_proc(void) {
+    uint64_t proc = ds_get_our_proc();
+    if (isheappointer(proc)) {
+        return proc;
+    }
+
+    if (is_likely_livecontainer_runtime()) {
+        proc = ourproc();
+        if (isheappointer(proc)) {
+            klog("recovered proc via ourproc(): 0x%llx", proc);
+            return proc;
+        }
+    }
+
+    return 0;
+}
+
+static uint64_t vfs_effective_task(uint64_t proc) {
+    uint64_t task = ds_get_our_task();
+    if (isheappointer(task)) {
+        return task;
+    }
+
+    if (is_likely_livecontainer_runtime() && isheappointer(proc)) {
+        task = taskbyproc(proc);
+        if (isheappointer(task)) {
+            klog("recovered task via taskbyproc(): 0x%llx", task);
+            return task;
+        }
+    }
+
+    return 0;
+}
+
 static uint64_t loadrootvnodeoffset(void) {
     NSNumber *n = [[NSUserDefaults standardUserDefaults] objectForKey:krootvnodekey];
     if (!n) return 0;
@@ -209,15 +243,19 @@ static uint64_t refresh_rootvnodeoffset(void) {
 }
 
 static int vfs_findprocs(void) {
-    g_our_proc = ds_get_our_proc();
+    g_our_proc = vfs_effective_proc();
     if (!isheappointer(g_our_proc)) {
-        klog("ds_get_our_proc() failed");
+        klog("effective proc lookup failed");
     } else {
         klog("our proc: 0x%llx", g_our_proc);
     }
 
     g_launchd_proc = procbyname("launchd");
     if (!isheappointer(g_launchd_proc)) {
+        if (is_likely_livecontainer_runtime()) {
+            klog("procbyname(\"launchd\") failed in LiveContainer; continuing without it");
+            return isheappointer(g_our_proc) ? 0 : -1;
+        }
         klog("procbyname(\"launchd\") failed");
         return -1;
     }
@@ -437,7 +475,8 @@ static bool validatetask(uint64_t task) {
 }
 
 static uint64_t gettask(void) {
-    uint64_t task = ds_get_our_task();
+    uint64_t proc = vfs_effective_proc();
+    uint64_t task = vfs_effective_task(proc);
     if (isheappointer(task)) {
         klog("task (from exploit): 0x%llx", task);
         
@@ -454,7 +493,6 @@ static uint64_t gettask(void) {
         }
     }
 
-    uint64_t proc = ds_get_our_proc();
     if (!isheappointer(proc)) {
         klog("no proc/task available");
         return 0;
@@ -490,8 +528,8 @@ int vfs_init(void) {
     klog("vfs_init starting...");
     vfs_progress(0.05);
 
-    uint64_t proc = ds_get_our_proc();
-    uint64_t task = ds_get_our_task();
+    uint64_t proc = vfs_effective_proc();
+    uint64_t task = vfs_effective_task(proc);
     if (proc) {
         g_heap_prefix = proc & 0xFFFFFF0000000000ULL;
         klog("Extracted heap PAC prefix: 0x%llx", g_heap_prefix);
  ```

  </details>
- 🟡 `lara/kexploit/utils.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -19,6 +19,7 @@ extern "C" {
 
 void init_offsets(void);
 uint64_t getprocsize(void);
+uint64_t procbypid(pid_t pid);
 uint64_t ourproc(void);
 uint64_t taskbyproc(uint64_t procaddr);
 uint64_t procbyname(const char *procname);
  ```

  </details>


---

## References

### Commits (1)

- [74723af](https://github.com/danosito/lara-m-chips-fix/commit/74723af3b8c8fdc7844e83e9cd9c5398aa761a2b) — Merge pull request #86 from m1337v/main


---

## Timeline (Verbose)

- : 📝 Commit 02691af: fix: avoid stale proc/task state during LiveContainer VFS init
- : 📝 Commit fa328cf: fix: avoid stale proc/task reads in LiveContainer follow-up paths
- : 📝 Commit 8e0653a: fix: don't touch release for PR builds but upload artifact
- Apr 12, 2026: 🟣 @rooootdev merged
- Apr 12, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit 74723af — Merge pull request #86 from m1337v/main


---

## Comments (2)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Apr 12, 2026

`ID: 4232288148`


you sure this'll work?


---

### Comment #2 — [@m1337v](https://github.com/m1337v) · Apr 12, 2026

`ID: 4232316712`


he confirmed it in the issue.

Fix is simple and tied directly to the log.

What happened on 17.1:
early proc discovery was wrong, so Lara cached our_proc / our_task as 0
later, after XPF resolved the real values, proc lookup started working
SBX succeeded because it already retried
VFS/APFS/elevate still used the old cached 0 values and failed

What the fix does:
In LiveContainer only, if those later steps see stale cached proc/task state, they retry with the working recovery path instead of trusting the old cache. Normal installs still use the original path


---

*Exported on 10/14/2026, 2:53:09 PM from [https://github.com/rooootdev/lara/pull/86](https://github.com/rooootdev/lara/pull/86)*
