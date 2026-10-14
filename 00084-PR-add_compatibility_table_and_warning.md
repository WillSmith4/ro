# add compatibility table and warning

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#84](https://github.com/rooootdev/lara/pull/84)  
> **State:** 🟣 merged  
> **Branch:** `neonmodder123:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 12, 2026  
> **Changes:** +16 / -2 across 1 files  
> **Created:** Apr 12, 2026  
> **Updated:** Apr 12, 2026  
> **Closed:** Apr 12, 2026  

---

## Description

*Opened by [@neonmodder123](https://github.com/neonmodder123)* *on Apr 12, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`d9bf7b7`](https://github.com/rooootdev/lara/commit/d9bf7b7a7f9dd338868aa9f6d38c3139652c0832) add compatibility table and warning — *neon* · Apr 12, 2026


---

## Changed Files (1)

- 🟡 `README.md` (modified) — +16 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -31,9 +31,23 @@
 ## support
 lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.
 
-ANY ISSUES THAT INVOLVE LARA NOT WORKING ON UNSUPPORTED VERSIONS WILL BE CLOSED IMMEDIATLEY. 
+currently tested on iOS 17.1.1 - 26.0.1, up to iOS 18.7.1 only on the 18.7 series.
+
+## compatibility
+
+| series | version / chip | status |
+| :--- | :--- | :--- |
+| **iOS 17** | all versions | supported |
+| **iOS 18** | 18.0 — 18.7.1 | supported |
+| **iOS 26.0/26.0.1** | 26.0 — 26.0.1 **only** | supported |
+| **iOS 26.1+** | 26.1+ | **patched** |
+| **M-series chips** | M1, M2, M3, etc...  | **unsupported** |
+
+> [!CAUTION]
+> if you are on an M-series device or any iOS version higher than 26.0.1, the app will crash on launch. this isn't a bug, this means that the exploit is patched by Apple.
+> 
+> **ANY ISSUES THAT INVOLVES LARA NOT WORKING ON UNSUPPORTED VERSIONS WILL BE CLOSED IMMEDIATLEY.**
 
-currently tested on iOS 17.1.1 - 26.0.1.
 If you run lara on your device, and it ends up working, please contact me on [discord](https://discord.gg/gw8PcRF3Jr) and tell me:
 1. your device
 2. your iOS version
  ```

  </details>


---

## References

### Commits (1)

- [040fa0a](https://github.com/danosito/lara-m-chips-fix/commit/040fa0ade819ce96df60b75a9dda2b0080fda2e5) — Merge pull request #84 from neonmodder123/main


---

## Timeline (Verbose)

- : 📝 Commit d9bf7b7: add compatibility table and warning
- Apr 12, 2026: 🟣 @rooootdev merged
- Apr 12, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit 040fa0a — Merge pull request #84 from neonmodder123/main


---

*Exported on 10/14/2026, 2:52:57 PM from [https://github.com/rooootdev/lara/pull/84](https://github.com/rooootdev/lara/pull/84)*
