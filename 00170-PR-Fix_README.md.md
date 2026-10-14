# Fix README.md

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#170](https://github.com/rooootdev/lara/pull/170)  
> **State:** 🟣 merged  
> **Branch:** `neonmodder123:patch-2` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 22, 2026  
> **Changes:** +5 / -4 across 1 files  
> **Created:** Apr 22, 2026  
> **Updated:** Apr 22, 2026  
> **Closed:** Apr 22, 2026  

---

## Description

*Opened by [@neonmodder123](https://github.com/neonmodder123)* *on Apr 22, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24789481665/job/72542915110) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24789481665/job/72542676692) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`abb1914`](https://github.com/rooootdev/lara/commit/abb19143776dde7c882ba0c55b97b17ac604cf51) Fix README.md — *neon* · Apr 22, 2026


---

## Changed Files (1)

- 🟡 `README.md` (modified) — +5 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -41,10 +41,10 @@ currently tested on iOS 17.1 - 26.0.1, up to iOS 18.7.1 only on the 18.7 series.
 | **iOS 18** | 18.0 — 18.7.1 | supported |
 | **iOS 26.0/26.0.1** | 26.0 — 26.0.1 **only** | supported |
 | **iOS 26.1+** | 26.1+ | **patched** |
-| **M-series chips** | M1, M2, M3, etc...  | **unsupported** |
+| **M-series chips** | M1, M2, M3, etc...  | supported |
 
 > [!CAUTION]
-> if you are on an M-series device or any iOS version higher than 26.0.1, the app will crash on launch. this isn't a bug, lara just doesnt support those devices yet.
+> if you are on an M-series device, go to lara settings, scroll down set t1sz_boot to `0x11`. if you are on any iOS version higher than 26.0.1 the app will crash on launch. this isn't a bug, lara just doesnt support those devices.
 > 
 > **ISSUES THAT INVOLVE LARA NOT WORKING ON UNSUPPORTED VERSIONS WILL BE CLOSED IMMEDIATELY.**<br>
 > **Issues related to lara not working on versions that the exploit DOES technically support will be closed and added to the known issues section**
@@ -71,10 +71,10 @@ If lara doesnt work on your device, and you want to help the project, please als
 - Upside Down
 - Floating Dock (Broken)
 - Grid App Switcher
-- Performance 
+- Performance
+- JIT
 
 ### coming soon:
-- JIT
 - App Decrypt
 
 ## known issues:
@@ -85,6 +85,7 @@ If lara doesnt work on your device, and you want to help the project, please als
 - ui is buggy on 17.x
 - .aea ota updates do not work.
 - A16+ and M-series devices dont support RemoteCall (yet)
+- apps don't detect JIT enabled however they are enabled.
 
 ### fixes:
 kernelcache download fix:
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit abb1914: Fix README.md
- Apr 22, 2026: 🟣 @rooootdev merged
- Apr 22, 2026: 🔴 Closed by @rooootdev
- Apr 22, 2026: 🗑️ @neonmodder123 deleted head branch


---

*Exported on 10/14/2026, 3:00:13 PM from [https://github.com/rooootdev/lara/pull/170](https://github.com/rooootdev/lara/pull/170)*
