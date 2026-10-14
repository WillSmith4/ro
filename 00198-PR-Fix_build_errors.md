# Fix build errors

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#198](https://github.com/rooootdev/lara/pull/198)  
> **State:** 🟣 merged  
> **Branch:** `jurre111:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 26, 2026  
> **Changes:** +13 / -8 across 1 files  
> **Created:** Apr 26, 2026  
> **Updated:** Apr 26, 2026  
> **Closed:** Apr 26, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on Apr 26, 2026*


as the title states: fix some things up so the build succeeds again


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (6)

- 🔴 [Cleanup artifacts](https://github.com/rooootdev/lara/actions/runs/24960295748/job/73085785441) — **failure** — *GitHub Actions*
- ✅ [Upload results](https://github.com/rooootdev/lara/actions/runs/24960295748/job/73085775204) — **success** — *GitHub Actions*
- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24960295980/job/73085744916) — **skipped** — *GitHub Actions*
- ✅ [Agent](https://github.com/rooootdev/lara/actions/runs/24960295748/job/73085682821) — **success** — *GitHub Actions*
- ✅ [Prepare](https://github.com/rooootdev/lara/actions/runs/24960295748/job/73085678702) — **success** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24960295980/job/73085678698) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 26, 2026

## Pull request overview

Adjusts the “Tweaks” navigation list in `ContentView` to resolve build issues and present tweak destinations using consistent `NavigationLink { … } label: { … }` syntax.

**Changes:**
- Adds an iOS-major-version–gated “Liquid Glass” navigation entry under “UI Tweaks”.
- Reworks the “App Management” section links to use `NavigationLink` with explicit labels (and adds the “Unblacklist (Broken?)” entry there).
- Removes the older string-based `NavigationLink("…") { … }` entries in the modified region.





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## PR Commits (1)

- [`af7a2e0`](https://github.com/rooootdev/lara/commit/af7a2e0d34c3e93f5b657bd9b67c5ff0cd6854de) Fix build errors — *jurre111* · Apr 26, 2026


---

## Changed Files (1)

- 🟡 `lara/views/ContentView.swift` (modified) — +13 / -8
  <details><summary>Diff</summary>

  ```diff
  @@ -380,6 +380,13 @@ struct ContentView: View {
                                             } label: {
                                                 Label("DarkBoard", systemImage: "app.badge")
                                             }
+                                            if os.majorVersion >= 26 {
+                                                NavigationLink {
+                                                    LGView()
+                                                } label: {
+                                                    Label("Liquid Glass", systemImage: "capsule")
+                                                }
+                                            }
                                         } header: {
                                             Text("UI Tweaks")
                                         }
@@ -389,15 +396,13 @@ struct ContentView: View {
                                             } label: {
                                                 Label("3 App Bypass", systemImage: "lock.open.fill")
                                             }
-
-                                        if os.majorVersion >= 26 {
-                                            NavigationLink("Liquid Glass") {
-                                                LGView()
+                                            NavigationLink {
+                                                WhitelistView()
+                                            } label: {
+                                                Label("Unblacklist (Broken?)", systemImage: "checkmark.seal")
                                             }
-                                        }
-
-                                        NavigationLink("3 App Bypass") {
-                                            AppsView(mgr: mgr)
+                                        } header: {
+                                            Text("App Management")
                                         }
                                         Section {
                                             if !showfmintabs {
  ```

  </details>


---

## References

### Commits (4)

- [930a0c7](https://github.com/letienndat/lara/commit/930a0c76a6cc42f0c74438a29d93ff7efdc56291) — Squashed commit of the following:
- [07c3a6c](https://github.com/letienndat/lara/commit/07c3a6ccd3a1ce37626a6ac44f59b3eb69bffed2) — Squashed commit of the following:
- [e0da526](https://github.com/letienndat/lara/commit/e0da526996585fa0cb7aaf3d6479a69cc2a8d2a4) — Squashed commit of the following:
- [9fc6ffb](https://github.com/letienndat/lara/commit/9fc6ffb8e54049fe785acc3b6361633ea2578ce1) — Squashed commit of the following:


---

## Timeline (Verbose)

- : 📝 Commit af7a2e0: Fix build errors
- Apr 26, 2026: 👀 @jurre111 requested review from @Copilot
- Apr 26, 2026: 🟣 @rooootdev merged
- Apr 26, 2026: 🔴 Closed by @rooootdev
- : 💬 someone reviewed: commented
- Apr 26, 2026: 🔗 Commit 930a0c7 — Squashed commit of the following:
- Apr 27, 2026: 🔗 Commit 07c3a6c — Squashed commit of the following:
- Apr 29, 2026: 🔗 Commit e0da526 — Squashed commit of the following:
- Apr 29, 2026: 🔗 Commit 9fc6ffb — Squashed commit of the following:


---

*Exported on 10/14/2026, 3:02:50 PM from [https://github.com/rooootdev/lara/pull/198](https://github.com/rooootdev/lara/pull/198)*
