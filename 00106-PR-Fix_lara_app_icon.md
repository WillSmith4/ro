# Fix lara app icon

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#106](https://github.com/rooootdev/lara/pull/106)  
> **State:** 🟣 merged  
> **Branch:** `neonmodder123:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 16, 2026  
> **Changes:** +1 / -1 across 1 files  
> **Created:** Apr 16, 2026  
> **Updated:** Apr 16, 2026  
> **Closed:** Apr 16, 2026  

---

## Description

*Opened by [@neonmodder123](https://github.com/neonmodder123)* *on Apr 16, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (1)

- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24521805319/job/71681312571) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`a80b863`](https://github.com/rooootdev/lara/commit/a80b8634b96f8d9712838c30eb5d9bbd06c70b31) fix app icon — *neon* · Apr 16, 2026


---

## Changed Files (1)

- 🟡 `scripts/build_ipa.sh` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -22,7 +22,7 @@ done
 
 PROJECT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")/.." && pwd)"
 SCHEME="lara"
-CONFIGURATION="Release"
+CONFIGURATION="Debug"
 APP_NAME="lara"
 DIST_DIR="$PROJECT_DIR/dist"
 PAYLOAD_DIR="$DIST_DIR/Payload"
  ```

  </details>


---

## References

### Commits (1)

- [ab2cf8f](https://github.com/danosito/lara-m-chips-fix/commit/ab2cf8fb85b366fa531028c0f8f734ff2474adef) — Merge pull request #106 from neonmodder123/main


---

## Timeline (Verbose)

- : 📝 Commit a80b863: fix app icon
- Apr 16, 2026: 🟣 @rooootdev merged
- Apr 16, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit ab2cf8f — Merge pull request #106 from neonmodder123/main


---

## Comments (2)

### Comment #1 — [@Pro42good](https://github.com/Pro42good) · Apr 16, 2026

`ID: 4261715379`


why the debug configuration?


---

### Comment #2 — [@neonmodder123](https://github.com/neonmodder123) · Apr 16, 2026

`ID: 4261725865`


> why the debug configuration?

Because that fixes the app icon, I noticed that building with Debug shows the icon and Release doesnt


👍 1 · 👀 1


---

*Exported on 10/14/2026, 2:54:43 PM from [https://github.com/rooootdev/lara/pull/106](https://github.com/rooootdev/lara/pull/106)*
