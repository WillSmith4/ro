# kernelcache fixes 

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#250](https://github.com/rooootdev/lara/pull/250)  
> **State:** 🟣 merged  
> **Branch:** `forestg10:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on May 16, 2026  
> **Changes:** +75 / -16 across 5 files  
> **Created:** May 16, 2026  
> **Updated:** May 21, 2026  
> **Closed:** May 16, 2026  

---

## Description

*Opened by [@rooootdev](https://github.com/rooootdev)* *on May 16, 2026*


on behalf of @forestg10.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (6)

- ✅ [Cleanup artifacts](https://github.com/rooootdev/lara/actions/runs/25963234935/job/76322260179) — **success** — *GitHub Actions*
- ✅ [Upload results](https://github.com/rooootdev/lara/actions/runs/25963234935/job/76322250980) — **success** — *GitHub Actions*
- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25963262396/job/76322232389) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25963262396/job/76322177729) — **success** — *GitHub Actions*
- ✅ [Agent](https://github.com/rooootdev/lara/actions/runs/25963234935/job/76322109203) — **success** — *GitHub Actions*
- ✅ [Prepare](https://github.com/rooootdev/lara/actions/runs/25963234935/job/76322104313) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · May 16, 2026

## Pull request overview

This PR improves the kernelcache/offsets flow by ensuring offsets are only marked present when they’ve actually been resolved from a valid kernelcache, hardening the kernelcache fetch routine, and refining iPad model detection used during offsets initialization.

**Changes:**
- Update the UI “Fetch Kernelcache” flow to resolve offsets (`dlkcache()`) after a successful fetch, and set `hasOffsets` based on that result.
- Harden `fetchkcache()` with readiness guards, robust read/write loops, and a basic output validity check before accepting the fetched kernelcache.
- Avoid misclassifying certain A-series iPads as M-series in `offsets_init()` by excluding known A-series identifiers.

### Reviewed changes

Copilot reviewed 5 out of 5 changed files in this pull request and generated no comments.

<details>
<summary>Show a summary per file</summary>

| File | Description |
| ---- | ----------- |
| lara/views/app/settings/SettingsView.swift | Sets `hasOffsets` based on `dlkcache()` result after a successful kernelcache fetch. |
| lara/views/app/ContentView.swift | Mirrors the Settings flow: `hasOffsets` reflects whether offsets resolution succeeded. |
| lara/kexploit/offsets.m | Refines M-series iPad detection by excluding known A-series iPad identifiers. |
| lara/funcs/fetchkcache.swift | Adds readiness checks, robust IO, and validates kernelcache output before reporting success. |
| lara/classes/laramgr.swift | Requires `hasOffsets` before allowing VFS init / sandbox escape to start. |
</details>







---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## PR Commits (4)

- [`1b0602d`](https://github.com/rooootdev/lara/commit/1b0602dbe9ce5a89a2e4fbcf0ce3d2ec1808f392) Fix A14 offsets — *Forest* · May 15, 2026
- [`7ce219f`](https://github.com/rooootdev/lara/commit/7ce219f1c5b088c1c644296d6e2bcf30fdcfce0a) Fix A14 offsets — *Forest* · May 15, 2026
- [`a82f013`](https://github.com/rooootdev/lara/commit/a82f01382187acbcc1d530f1bce73d9c93151594) Fix A14 offsets — *Forest* · May 15, 2026
- [`d29cce2`](https://github.com/rooootdev/lara/commit/d29cce2106520d06086cf1ff908295e3a3cd18e9) Fix A14 offsets — *Forest* · May 15, 2026


---

## Changed Files (5)

- 🟡 `lara/classes/laramgr.swift` (modified) — +2 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -166,6 +166,7 @@ final class laramgr: ObservableObject {
     }
     
     func vfsinit(completion: ((Bool) -> Void)? = nil) {
+        guard dsready, hasOffsets, !vfsrunning else { return }
         vfs_setlogcallback(laramgr.vfslogcallback)
         vfs_setprogresscallback { progress in
             DispatchQueue.main.async {
@@ -197,7 +198,7 @@ final class laramgr: ObservableObject {
     }
     
     func sbxescape(completion: ((Bool) -> Void)? = nil) {
-        guard dsready, !sbxrunning else { return }
+        guard dsready, hasOffsets, !sbxrunning else { return }
         sbxattempted = true
         sbxfailed = false
         sbxrunning = true
  ```

  </details>
- 🟡 `lara/funcs/fetchkcache.swift` (modified) — +54 / -6
  <details><summary>Diff</summary>

  ```diff
  @@ -18,6 +18,18 @@ func larakcpath() -> String? {
 }
 
 func fetchkcache() -> Bool {
+    guard ds_is_ready(),
+          off_proc_p_fd != 0,
+          off_filedesc_fd_ofiles != 0,
+          off_fileproc_fp_glob != 0,
+          off_fileglob_fg_data != 0,
+          off_vnode_v_data != 0,
+          off_namecache_nc_vp != 0,
+          off_namecache_nc_child_tqe_next != 0 else {
+        globallogger.log("(fetchkcache) exploit or offsets not ready")
+        return false
+    }
+
     guard let kcpath = syskcpath() else {
         globallogger.log("(fetchkcache) failed to get kernelcache path")
         return false
@@ -63,23 +75,59 @@ func fetchkcache() -> Bool {
     }
 
     var buffer = [UInt8](repeating: 0, count: 0x4000)
+    let bufferSize = buffer.count
+    var totalBytes = 0
 
     while true {
-        let n = read(src, &buffer, buffer.count)
+        let n = buffer.withUnsafeMutableBytes { rawBuffer in
+            read(src, rawBuffer.baseAddress!, bufferSize)
+        }
 
-        if n <= 0 {
+        if n < 0 {
+            globallogger.log("(fetchkcache) failed to read kernelcache")
+            return false
+        }
+
+        if n == 0 {
             break
         }
 
-        _ = write(dst, buffer, n)
+        var written = 0
+        while written < n {
+            let w = buffer.withUnsafeBytes { rawBuffer in
+                write(dst, rawBuffer.baseAddress!.advanced(by: written), n - written)
+            }
+
+            if w <= 0 {
+                globallogger.log("(fetchkcache) failed to write kernelcache")
+                return false
+            }
+
+            written += w
+        }
+
+        totalBytes += n
+    }
+
+    if !FileManager.default.fileExists(atPath: outpath) || totalBytes == 0 {
+        globallogger.log("(fetchkcache) kernelcache output missing")
+        return false
     }
 
-    if !FileManager.default.fileExists(atPath: outpath) {
+    guard let handle = FileHandle(forReadingAtPath: outpath) else {
         globallogger.log("(fetchkcache) kernelcache output missing")
         return false
-    } else {
-        globallogger.log("(fetchkcache) kernelcache fetch success!")
     }
 
+    let magic = handle.readData(ofLength: 2)
+    handle.closeFile()
+
+    guard magic.count == 2, magic[magic.startIndex] == 0x30, magic[magic.index(after: magic.startIndex)] == 0x84 else {
+        unlink(outpath)
+        globallogger.log("(fetchkcache) invalid kernelcache output")
+        return false
+    }
+
+    globallogger.log("(fetchkcache) kernelcache fetch success!")
     return true
 }
  ```

  </details>
- 🟡 `lara/kexploit/offsets.m` (modified) — +13 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -530,6 +530,18 @@ static bool is_ipad_device(void) {
     return strncmp(machine, "iPad", 4) == 0;
 }
 
+static bool is_known_a_series_ipad_identifier(void) {
+    char machine[64] = {0};
+    size_t sz = sizeof(machine);
+    if (sysctlbyname("hw.machine", machine, &sz, NULL, 0) != 0) return false;
+    return strcmp(machine, "iPad13,1") == 0 ||
+           strcmp(machine, "iPad13,2") == 0 ||
+           strcmp(machine, "iPad13,18") == 0 ||
+           strcmp(machine, "iPad13,19") == 0 ||
+           strcmp(machine, "iPad14,1") == 0 ||
+           strcmp(machine, "iPad14,2") == 0;
+}
+
 void offsets_init(void) {
     if (!(SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"17.0") && SYSTEM_VERSION_LESS_THAN(@"26.1"))) {
         printf("(offs) only supported offset for iOS 17.0 - 26.0.x\n");
@@ -575,7 +587,7 @@ void offsets_init(void) {
                      cpuFamily == CPUFAMILY_ARM_TAHITI ||
                      cpuFamily == CPUFAMILY_ARM_DONAN);
     
-    bool isMSeriesIpad = is_ipad_device() && (
+    bool isMSeriesIpad = is_ipad_device() && !is_known_a_series_ipad_identifier() && (
                                               cpuFamily == CPUFAMILY_ARM_FIRESTORM_ICESTORM ||
                                               cpuFamily == CPUFAMILY_ARM_BLIZZARD_AVALANCHE ||
                                               cpuFamily == CPUFAMILY_ARM_IBIZA ||
  ```

  </details>
- 🟡 `lara/views/app/ContentView.swift` (modified) — +3 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -91,17 +91,16 @@ struct ContentView: View {
                         let fetched = fetchkcache()
 
                         if fetched {
+                            let dlkc = dlkcache()
                             DispatchQueue.main.async {
-                                mgr.hasOffsets = true
+                                mgr.hasOffsets = dlkc
                                 dlingkcache = false
                             }
                             return
                         }
 
-                        let dlkc = dlkcache()
-
                         DispatchQueue.main.async {
-                            mgr.hasOffsets = dlkc
+                            mgr.hasOffsets = false
                             dlingkcache = false
                         }
                     }
  ```

  </details>
- 🟡 `lara/views/app/settings/SettingsView.swift` (modified) — +3 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -79,17 +79,16 @@ struct SettingsView: View {
                                 let fetched = fetchkcache()
 
                                 if fetched {
+                                    let dlkc = dlkcache()
                                     DispatchQueue.main.async {
-                                        mgr.hasOffsets = true
+                                        mgr.hasOffsets = dlkc
                                         dlingkcache = false
                                     }
                                     return
                                 }
 
-                                let dlkc = dlkcache()
-
                                 DispatchQueue.main.async {
-                                    mgr.hasOffsets = dlkc
+                                    mgr.hasOffsets = false
                                     dlingkcache = false
                                 }
                             }
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 1b0602d: Fix A14 offsets
- : 📝 Commit 7ce219f: Fix A14 offsets
- : 📝 Commit a82f013: Fix A14 offsets
- : 📝 Commit d29cce2: Fix A14 offsets
- May 16, 2026: 👀 @rooootdev requested review from @Copilot
- May 16, 2026: 🟣 @rooootdev merged
- May 16, 2026: 🔴 Closed by @rooootdev
- : 💬 someone reviewed: commented


---

*Exported on 10/14/2026, 3:07:19 PM from [https://github.com/rooootdev/lara/pull/250](https://github.com/rooootdev/lara/pull/250)*
