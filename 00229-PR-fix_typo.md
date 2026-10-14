# fix typo

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#229](https://github.com/rooootdev/lara/pull/229)  
> **State:** 🟣 merged  
> **Branch:** `tryh4rd3rr:typo-patch` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on May 7, 2026  
> **Changes:** +1 / -1 across 1 files  
> **Created:** May 6, 2026  
> **Updated:** May 7, 2026  
> **Closed:** May 7, 2026  

---

## Description

*Opened by [@tryh4rd3rr](https://github.com/tryh4rd3rr)* *on May 6, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25426221019/job/74580361822) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25426221019/job/74580207345) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`770ca7d`](https://github.com/rooootdev/lara/commit/770ca7dcb4a097a69cf5b2f5b93ae345597a61da) fix typo — *Shin Morisawa* · May 6, 2026


---

## Changed Files (1)

- 🟡 `lara/views/app/ContentView.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -113,7 +113,7 @@ struct ContentView: View {
                             } else if mgr.vfsattempted && mgr.vfsfailed || mgr.sbxattempted && mgr.sbxfailed {
                                 ButtonLabel(text: "Failed!", icon: "xmark")
                             } else {
-                                ButtonLabel(text: "Initalize System", icon: "folder")
+                                ButtonLabel(text: "Initialize System", icon: "folder")
                             }
                         }
                         .buttonStyle(TranslucentButtonStyle(color: mgr.vfsattempted && mgr.vfsfailed || mgr.sbxattempted && mgr.sbxfailed ? .red : .purple))
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 770ca7d: fix typo
- May 7, 2026: 🟣 @rooootdev merged
- May 7, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:05:31 PM from [https://github.com/rooootdev/lara/pull/229](https://github.com/rooootdev/lara/pull/229)*
