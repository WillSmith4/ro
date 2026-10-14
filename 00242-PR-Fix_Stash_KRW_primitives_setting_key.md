# Fix Stash KRW primitives setting key

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#242](https://github.com/rooootdev/lara/pull/242)  
> **State:** 🟣 merged  
> **Branch:** `hxhlb:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on May 13, 2026  
> **Changes:** +1 / -1 across 1 files  
> **Created:** May 13, 2026  
> **Updated:** May 13, 2026  
> **Closed:** May 13, 2026  

---

## Description

*Opened by [@hxhlb](https://github.com/hxhlb)* *on May 13, 2026*


## Summary

Fix the UserDefaults key used by Darksword when checking whether `Stash KRW primitives` is enabled.

`SettingsView` currently stores the toggle with `@AppStorage("stashKRW")`, but `darksword.m` was still reading `"stashkrw"`. Since UserDefaults keys are case-sensitive, enabling the toggle in Settings did not affect the Darksword code path.

## Changes

- Update `stashkrw()` to read `"stashKRW"`, matching the Settings toggle.

## Testing

- Not run. This is a one-line defaults-key alignment fix.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25774930278/job/75705581514) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25774930278/job/75705470176) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`937cc18`](https://github.com/rooootdev/lara/commit/937cc18334f12310fdf17242628201541567b244) Fix stash KRW defaults key — *Hades* · May 13, 2026


---

## Changed Files (1)

- 🟡 `lara/kexploit/darksword.m` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -134,7 +134,7 @@ static void cmp8_wait_for_change(volatile uint64_t *ptr, uint64_t old_val) {
 static char host_executable_name[PROCESS_MARKER_MAX_LEN];
 
 BOOL stashkrw(void) {
-    return [[NSUserDefaults standardUserDefaults] boolForKey:@"stashkrw"];
+    return [[NSUserDefaults standardUserDefaults] boolForKey:@"stashKRW"];
 }
 
 static void addprocmarker(const char *marker) {
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 937cc18: Fix stash KRW defaults key
- May 13, 2026: 🟣 @rooootdev merged
- May 13, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:06:32 PM from [https://github.com/rooootdev/lara/pull/242](https://github.com/rooootdev/lara/pull/242)*
