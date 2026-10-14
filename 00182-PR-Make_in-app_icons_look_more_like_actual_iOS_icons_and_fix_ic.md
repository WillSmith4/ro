# Make in-app icons look more like actual iOS icons and fix icon fallback in DarkBoardView.swift

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#182](https://github.com/rooootdev/lara/pull/182)  
> **State:** 🟣 merged  
> **Branch:** `jurre111:iconsfix` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 23, 2026  
> **Changes:** +9 / -9 across 3 files  
> **Created:** Apr 23, 2026  
> **Updated:** Apr 26, 2026  
> **Closed:** Apr 23, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on Apr 23, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (6)

- 🔴 [Cleanup artifacts](https://github.com/rooootdev/lara/actions/runs/24842366337/job/72720578853) — **failure** — *GitHub Actions*
- ✅ [Upload results](https://github.com/rooootdev/lara/actions/runs/24842366337/job/72720534569) — **success** — *GitHub Actions*
- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24842420277/job/72720162121) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24842420277/job/72719925991) — **success** — *GitHub Actions*
- ✅ [Agent](https://github.com/rooootdev/lara/actions/runs/24842366337/job/72719767963) — **success** — *GitHub Actions*
- ✅ [Prepare](https://github.com/rooootdev/lara/actions/runs/24842366337/job/72719744125) — **success** — *GitHub Actions*


---

## Reviews (2)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 23, 2026

## Pull request overview

> [!NOTE]
> Copilot was unable to run its full agentic suite in this review.

Updates app icon masking to better resemble iOS-style rounded icons across multiple views.

**Changes:**
- Replaced `cornerRadius(6)` with `clipShape(RoundedRectangle(...))` in `JitView`.
- Increased corner radius values to 9 for 40×40 icons in `AppsView` and `DarkBoardView`.
- Applied the updated clipping to the “unknown” icon fallback in `AppsView`.

### Reviewed changes

Copilot reviewed 3 out of 3 changed files in this pull request and generated 5 comments.

| File | Description |
| ---- | ----------- |
| lara/views/JitView.swift | Switches icon rounding to `clipShape(RoundedRectangle(...))` for a more iOS-like mask. |
| lara/views/DarkBoardView.swift | Adjusts icon corner radius for cached icons (but not the placeholder). |
| lara/views/AppsView.swift | Updates icon rounding and aligns the “unknown” fallback with the new mask. |





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.

---

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 23, 2026

## Pull request overview

Updates app icon presentation in SwiftUI views to better match iOS icon proportions (larger size + more “squircle”-like rounding) and standardizes the missing-icon fallback in DarkBoard.

**Changes:**
- Increase JIT list icon size (32 → 40) and switch to `clipShape(RoundedRectangle(...))` for more iOS-like rounding.
- Use the `unknown` asset as the missing-icon fallback in `DarkBoardView` (instead of a gray placeholder tile).
- Align icon corner radii across views (e.g., 40px icons → radius 9; 60px preview → radius 13).

### Reviewed changes

Copilot reviewed 3 out of 3 changed files in this pull request and generated no comments.

| File | Description |
| ---- | ----------- |
| lara/views/JitView.swift | Makes process/app icons larger and adjusts rounding to better resemble iOS icons. |
| lara/views/DarkBoardView.swift | Updates icon rounding and changes missing-icon fallback to the `unknown` asset; adjusts theme preview rounding. |
| lara/views/AppsView.swift | Aligns icon rounding and applies the same rounding to the fallback `unknown` image. |





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (5)

*Inline comments on code changes*

### `lara/views/DarkBoardView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 23, 2026 (line 126) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -121,7 +121,7 @@ struct DarkBoardView: View {
     @ViewBuilder
     private func iconView(for app: AppInfo) -> some View {
         if let icon = iconCache[app.bundlePath] {
-            Image(uiImage: icon).resizable().frame(width: 40, height: 40).clipShape(RoundedRectangle(cornerRadius: 6))
+            Image(uiImage: icon).resizable().frame(width: 40, height: 40).clipShape(RoundedRectangle(cornerRadius: 9))
         } else {
             RoundedRectangle(cornerRadius: 6).fill(Color.gray.opacity(0.3)).frame(width: 40, height: 40)
```

</details>

The placeholder uses `cornerRadius: 6` while real icons use `cornerRadius: 9`, which will make the UI look inconsistent and undermines the PR’s goal. Update the placeholder `RoundedRectangle` to use the same corner radius (and shape style, if adopted) as the icon clipping.


### `lara/views/JitView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 23, 2026 (line 70) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -67,12 +67,12 @@ struct JitView: View {
                                     Image(uiImage: icon)
                                         .resizable()
                                         .frame(width: 32, height: 32)
-                                        .cornerRadius(6)
+                                        .clipShape(RoundedRectangle(cornerRadius: 7))
```

</details>

If the goal is to mimic iOS icon corners, consider using `RoundedRectangle(cornerRadius: ..., style: .continuous)` (or a shared icon mask shape) to better match the continuous-squircle feel iOS uses rather than the default corner style.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 23, 2026 (line 75) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -67,12 +67,12 @@ struct JitView: View {
                                     Image(uiImage: icon)
                                         .resizable()
                                         .frame(width: 32, height: 32)
-                                        .cornerRadius(6)
+                                        .clipShape(RoundedRectangle(cornerRadius: 7))
                                 } else {
                                     Image("unknown")
                                         .resizable()
                                         .frame(width: 32, height: 32)
-                                        .cornerRadius(6)
+                                        .clipShape(RoundedRectangle(cornerRadius: 7))
```

</details>

If the goal is to mimic iOS icon corners, consider using `RoundedRectangle(cornerRadius: ..., style: .continuous)` (or a shared icon mask shape) to better match the continuous-squircle feel iOS uses rather than the default corner style.


### `lara/views/AppsView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 23, 2026 (line 245) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -242,11 +242,12 @@ struct AppsView: View {
                                 Image(uiImage: icon)
                                     .resizable()
                                     .frame(width: 40, height: 40)
-                                    .clipShape(RoundedRectangle(cornerRadius: 6))
+                                    .clipShape(RoundedRectangle(cornerRadius: 9))
```

</details>

Corner radius values (e.g., 7 vs 9 across views) are hard-coded and repeated, which makes consistent tuning harder. Consider centralizing the icon mask parameters (e.g., a shared `AppIconMask` view/modifier or a single constant per icon size) so future adjustments only need to be made in one place.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 23, 2026 (line 250) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -242,11 +242,12 @@ struct AppsView: View {
                                 Image(uiImage: icon)
                                     .resizable()
                                     .frame(width: 40, height: 40)
-                                    .clipShape(RoundedRectangle(cornerRadius: 6))
+                                    .clipShape(RoundedRectangle(cornerRadius: 9))
                             } else {
                                 Image("unknown")
                                     .resizable()
                                     .frame(width: 40, height: 40)
+                                    .clipShape(RoundedRectangle(cornerRadius: 9))
```

</details>

Corner radius values (e.g., 7 vs 9 across views) are hard-coded and repeated, which makes consistent tuning harder. Consider centralizing the icon mask parameters (e.g., a shared `AppIconMask` view/modifier or a single constant per icon size) so future adjustments only need to be made in one place.



---

## PR Commits (3)

- [`e6ca1d2`](https://github.com/rooootdev/lara/commit/e6ca1d2b8e20dda174147652e0f9a51a2213d32d) Make icons look more ios-like — *jurre111* · Apr 23, 2026
- [`cf6903b`](https://github.com/rooootdev/lara/commit/cf6903b23057f621b1f6003fbe6d59ac8e0ca07c) Improve consistency — *jurre111* · Apr 23, 2026
- [`096e587`](https://github.com/rooootdev/lara/commit/096e587ccfee6f50e54cef77a3119c907d69a246) Some improvements and fixes — *jurre111* · Apr 23, 2026


---

## Changed Files (3)

- 🟡 `lara/views/AppsView.swift` (modified) — +2 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -242,11 +242,12 @@ struct AppsView: View {
                                 Image(uiImage: icon)
                                     .resizable()
                                     .frame(width: 40, height: 40)
-                                    .clipShape(RoundedRectangle(cornerRadius: 6))
+                                    .clipShape(RoundedRectangle(cornerRadius: 9))
                             } else {
                                 Image("unknown")
                                     .resizable()
                                     .frame(width: 40, height: 40)
+                                    .clipShape(RoundedRectangle(cornerRadius: 9))
                             }
 
                             VStack(alignment: .leading) {
  ```

  </details>
- 🟡 `lara/views/DarkBoardView.swift` (modified) — +3 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -121,10 +121,9 @@ struct DarkBoardView: View {
     @ViewBuilder
     private func iconView(for app: AppInfo) -> some View {
         if let icon = iconCache[app.bundlePath] {
-            Image(uiImage: icon).resizable().frame(width: 40, height: 40).clipShape(RoundedRectangle(cornerRadius: 6))
+            Image(uiImage: icon).resizable().frame(width: 40, height: 40).clipShape(RoundedRectangle(cornerRadius: 9))
         } else {
-            RoundedRectangle(cornerRadius: 6).fill(Color.gray.opacity(0.3)).frame(width: 40, height: 40)
-                .overlay(Image(systemName: "app.fill").foregroundColor(.secondary))
+            Image("unknown").resizable().frame(width: 40, height: 40).clipShape(RoundedRectangle(cornerRadius: 9))
         }
     }
     
@@ -344,7 +343,7 @@ struct ThemePickerSheet: View {
                         }
                         Spacer()
                         if let img = previewImage {
-                            Image(uiImage: img).resizable().frame(width: 60, height: 60).clipShape(RoundedRectangle(cornerRadius: 10))
+                            Image(uiImage: img).resizable().frame(width: 60, height: 60).clipShape(RoundedRectangle(cornerRadius: 13))
                         }
                     }
                 }
  ```

  </details>
- 🟡 `lara/views/JitView.swift` (modified) — +4 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -66,13 +66,13 @@ struct JitView: View {
                                 if let icon = proc.icon {
                                     Image(uiImage: icon)
                                         .resizable()
-                                        .frame(width: 32, height: 32)
-                                        .cornerRadius(6)
+                                        .frame(width: 40, height: 40)
+                                        .clipShape(RoundedRectangle(cornerRadius: 9))
                                 } else {
                                     Image("unknown")
                                         .resizable()
-                                        .frame(width: 32, height: 32)
-                                        .cornerRadius(6)
+                                        .frame(width: 40, height: 40)
+                                        .clipShape(RoundedRectangle(cornerRadius: 9))
                                 }
 
                                 VStack(alignment: .leading) {
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit e6ca1d2: Make icons look more ios-like
- Apr 23, 2026: 👀 @jurre111 requested review from @Copilot
- Apr 23, 2026: ✏️ @jurre111 renamed: "Make icons look more like actual iOS icons" → "Make in-app icons look more like actual iOS icons"
- : 💬 someone reviewed: commented
- : 📝 Commit cf6903b: Improve consistency
- : 📝 Commit 096e587: Some improvements and fixes
- Apr 23, 2026: ✏️ @jurre111 renamed: "Make in-app icons look more like actual iOS icons" → "Make in-app icons look more like actual iOS icons and fix icon fallback in DarkBoardView.swift"
- Apr 23, 2026: 👀 @jurre111 requested review from @Copilot
- : 💬 someone reviewed: commented
- Apr 23, 2026: 🟣 @rooootdev merged
- Apr 23, 2026: 🔴 Closed by @rooootdev
- Apr 26, 2026: 🗑️ @jurre111 deleted head branch


---

*Exported on 10/14/2026, 3:01:25 PM from [https://github.com/rooootdev/lara/pull/182](https://github.com/rooootdev/lara/pull/182)*
