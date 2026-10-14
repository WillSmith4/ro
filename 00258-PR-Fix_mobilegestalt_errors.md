# Fix mobilegestalt errors

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#258](https://github.com/rooootdev/lara/pull/258)  
> **State:** 🟣 merged  
> **Branch:** `neonmodder123:tmp` → `rooootdev:main`  
> **Merged by:** [@lunginspector](https://github.com/lunginspector) on May 22, 2026  
> **Changes:** +1 / -1 across 1 files  
> **Created:** May 22, 2026  
> **Updated:** May 22, 2026  
> **Closed:** May 22, 2026  

---

## Description

*Opened by [@neonmodder123](https://github.com/neonmodder123)* *on May 22, 2026*


Changes line 49 mgr: laramgr() to mgr: mgr, by @jurre111


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/26291042579/job/77391247101) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/26291042579/job/77391062434) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`8cbef24`](https://github.com/rooootdev/lara/commit/8cbef243a06c9eac707cc77ef92e0f70e701c519) Fix mobilegestalt — *neon* · May 22, 2026


---

## Changed Files (1)

- 🟡 `lara/views/tweaks/TweaksView.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -46,7 +46,7 @@ struct TweaksView: View {
                 Section(header: HeaderLabel(text: "User Interface", icon: "eye")) {
                     NavigationLink("dirtyZero", destination: dirtyZeroView())
                         .disabled(!mgr.vfsready)
-                    NavigationLink("MobileGestalt", destination: GestaltView(mgr: laramgr()))
+                    NavigationLink("MobileGestalt", destination: GestaltView(mgr: mgr))
                         .disabled(!mgr.sbxready)
                     NavigationLink("Font Overwrite", destination: FontPicker(mgr: mgr))
                         .disabled(!mgr.vfsready)
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 8cbef24: Fix mobilegestalt
- May 22, 2026: 🟣 @lunginspector merged
- May 22, 2026: 🔴 Closed by @lunginspector


---

*Exported on 10/14/2026, 3:08:00 PM from [https://github.com/rooootdev/lara/pull/258](https://github.com/rooootdev/lara/pull/258)*
