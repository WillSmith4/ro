# Fix overriding user-set offsets

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#207](https://github.com/rooootdev/lara/pull/207)  
> **State:** 🟣 merged  
> **Branch:** `jurre111:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 28, 2026  
> **Changes:** +5 / -4 across 1 files  
> **Created:** Apr 28, 2026  
> **Updated:** Apr 28, 2026  
> **Closed:** Apr 28, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on Apr 28, 2026*


Could be me, but everything around t1sz_boot seems unnecessarily complicated and fragile. Anyways, a PR added a check that overrode t1sz_boot if set to a different one by the user, so i just commented it out.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25047598969/job/73366849949) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25047598969/job/73366680261) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`96ceea5`](https://github.com/rooootdev/lara/commit/96ceea5a090dc74a0714719e647d6a4dd07c1ce3) Fix overriding user-set offsets — *jurre111* · Apr 28, 2026


---

## Changed Files (1)

- 🟡 `lara/kexploit/offsets.m` (modified) — +5 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -300,10 +300,11 @@ void offsets_init(void) {
                             cpuFamily == CPUFAMILY_ARM_IBIZA ||  // M3
                             cpuFamily == CPUFAMILY_ARM_DONAN); // M4
     
-    uint64_t expected_t1sz_boot = (isA16Above || isMSeriesIpad) ? 0x11 : 0x19;
-    if (t1sz_boot != expected_t1sz_boot) {
-        t1sz_boot = expected_t1sz_boot;
-    }
+    // these checks will overide the user set offsets
+    // uint64_t expected_t1sz_boot = (isA16Above || isMSeriesIpad) ? 0x11 : 0x19;
+    // if (t1sz_boot != expected_t1sz_boot) {
+    //     t1sz_boot = expected_t1sz_boot;
+    // }
 
     gIsPACSupported = is_pac_supported();
     
  ```

  </details>


---

## References

### Commits (1)

- [9fc6ffb](https://github.com/letienndat/lara/commit/9fc6ffb8e54049fe785acc3b6361633ea2578ce1) — Squashed commit of the following:


---

## Timeline (Verbose)

- : 📝 Commit 96ceea5: Fix overriding user-set offsets
- Apr 28, 2026: 👀 @jurre111 requested review from @Copilot
- Apr 28, 2026: 🟣 @rooootdev merged
- Apr 28, 2026: 🔴 Closed by @rooootdev
- Apr 29, 2026: 🔗 Commit 9fc6ffb — Squashed commit of the following:


---

*Exported on 10/14/2026, 3:03:44 PM from [https://github.com/rooootdev/lara/pull/207](https://github.com/rooootdev/lara/pull/207)*
