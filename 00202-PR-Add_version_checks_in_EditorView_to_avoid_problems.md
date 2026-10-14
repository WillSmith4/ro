# Add version checks in EditorView to avoid problems

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#202](https://github.com/rooootdev/lara/pull/202)  
> **State:** 🟣 merged  
> **Branch:** `jurre111:mbg-versioncheck` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 27, 2026  
> **Changes:** +13 / -1 across 1 files  
> **Created:** Apr 27, 2026  
> **Updated:** Apr 27, 2026  
> **Closed:** Apr 27, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on Apr 27, 2026*


small change, but could be important


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (6)

- 🔴 [Cleanup artifacts](https://github.com/rooootdev/lara/actions/runs/25004329080/job/73223148644) — **failure** — *GitHub Actions*
- ✅ [Upload results](https://github.com/rooootdev/lara/actions/runs/25004329080/job/73223084800) — **success** — *GitHub Actions*
- ✅ [Agent](https://github.com/rooootdev/lara/actions/runs/25004329080/job/73222813972) — **success** — *GitHub Actions*
- ✅ [Prepare](https://github.com/rooootdev/lara/actions/runs/25004329080/job/73222786272) — **success** — *GitHub Actions*
- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24985597751/job/73158173991) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24985597751/job/73157965202) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 27, 2026

## Pull request overview

Adds OS version gating in `EditorView` so certain MobileGestalt toggles are disabled on unsupported iOS versions, reducing the likelihood of enabling features that won’t work on older systems.

**Changes:**
- Add an OS-version helper (`requiresVersion`) to compare against the current device OS.
- Disable several feature toggles (Action Button / AoD / Camera button / Charge limit) when running below the required major iOS version.
- Adjust the “Action Button” toggle label (removes “17+” from the title and enforces via `.disabled` instead).





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (2)

*Inline comments on code changes*

### `lara/views/EditorView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 27, 2026 (line 104) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -88,15 +89,19 @@ struct EditorView: View {
                         }
                         .pickerStyle(.menu)
                     }
-                    Toggle("Action Button (17+)", isOn: mgkeybinding(["cT44WE1EohiwRzhsZ8xEsw"]))
+                    Toggle("Action Button", isOn: mgkeybinding(["cT44WE1EohiwRzhsZ8xEsw"]))
+                        .disabled(requiresVersion(17))
                     Toggle("Allow installing iPadOS apps", isOn: mgkeybinding(["9MZ5AdH43csAUajl/dU+IQ"], type: [Int].self, default: [1], enable: [1, 2]))
                     Toggle("Always on Display (18.0+)", isOn: mgkeybinding(["j8/Omm6s1lsmTDFsXjsBfA", "2OOJf1VhaM7NxfRok3HbWQ"]))
+                        .disabled(requiresVersion(18))
                     // Toggle("Apple Intelligence", isOn: bindingForAppleIntelligence())
                     //    .disabled(requiresVersion(18))
                     Toggle("Apple Pencil", isOn: mgkeybinding(["yhHcB0iH0d1XzPO/CFd3ow"]))
                     Toggle("Boot chime", isOn: mgkeybinding(["QHxt+hGLaBPbQJbXiUJX3w"]))
                     Toggle("Camera button (18.0rc+)", isOn: mgkeybinding(["CwvKxM2cEogD3p+HYgaW0Q", "oOV1jhJbdV3AddkcCg0AEA"]))
+                        .disabled(requiresVersion(18))
                     Toggle("Charge limit (17+)", isOn: mgkeybinding(["37NVydb//GP/GrhuTN+exg"]))
+                    .   disabled(requiresVersion(17))
```

</details>

Line has a stray "." and extra spaces before `disabled`, which will not compile. It should be chained as `.disabled(requiresVersion(17))` with normal indentation.
```suggestion
                        .disabled(requiresVersion(17))
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 27, 2026 (line 327) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -318,4 +323,11 @@ struct EditorView: View {
             }
         )
     }
+
+    private func requiresVersion(_ major : Int, _ minor: Int = 0, _ patch: Int = 0) -> Bool {
```

</details>

Spacing in the new helper signature is inconsistent with Swift style (`major : Int`). Consider changing to `major: Int` to match typical formatting in the rest of the file.
```suggestion
    private func requiresVersion(_ major: Int, _ minor: Int = 0, _ patch: Int = 0) -> Bool {
```



---

## PR Commits (2)

- [`24d3107`](https://github.com/rooootdev/lara/commit/24d3107e492ea073cf61da7e712d693610dc1d56) Add version checks in EditorView — *jurre111* · Apr 27, 2026
- [`c1f98e8`](https://github.com/rooootdev/lara/commit/c1f98e821eba0e7907c22cbd5080daa2ecfbc1f2) static -> private — *Jurre* · Apr 27, 2026


---

## Changed Files (1)

- 🟡 `lara/views/EditorView.swift` (modified) — +13 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -39,6 +39,7 @@ struct EditorView: View {
     
     private let path = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist"
     private let ogmgurl: URL
+    let os = ProcessInfo().operatingSystemVersion
 
     init() {
         let docs = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
@@ -88,15 +89,19 @@ struct EditorView: View {
                         }
                         .pickerStyle(.menu)
                     }
-                    Toggle("Action Button (17+)", isOn: mgkeybinding(["cT44WE1EohiwRzhsZ8xEsw"]))
+                    Toggle("Action Button", isOn: mgkeybinding(["cT44WE1EohiwRzhsZ8xEsw"]))
+                        .disabled(requiresVersion(17))
                     Toggle("Allow installing iPadOS apps", isOn: mgkeybinding(["9MZ5AdH43csAUajl/dU+IQ"], type: [Int].self, default: [1], enable: [1, 2]))
                     Toggle("Always on Display (18.0+)", isOn: mgkeybinding(["j8/Omm6s1lsmTDFsXjsBfA", "2OOJf1VhaM7NxfRok3HbWQ"]))
+                        .disabled(requiresVersion(18))
                     // Toggle("Apple Intelligence", isOn: bindingForAppleIntelligence())
                     //    .disabled(requiresVersion(18))
                     Toggle("Apple Pencil", isOn: mgkeybinding(["yhHcB0iH0d1XzPO/CFd3ow"]))
                     Toggle("Boot chime", isOn: mgkeybinding(["QHxt+hGLaBPbQJbXiUJX3w"]))
                     Toggle("Camera button (18.0rc+)", isOn: mgkeybinding(["CwvKxM2cEogD3p+HYgaW0Q", "oOV1jhJbdV3AddkcCg0AEA"]))
+                        .disabled(requiresVersion(18))
                     Toggle("Charge limit (17+)", isOn: mgkeybinding(["37NVydb//GP/GrhuTN+exg"]))
+                    .   disabled(requiresVersion(17))
                     Toggle("Crash Detection (might not work)", isOn: mgkeybinding(["HCzWusHQwZDea6nNhaKndw"]))
                     // Toggle("Dynamic Island (17.4+, might not work)", isOn: mgkeybinding(["YlEtTtHlNesRBMal1CqRaA"]))
                     // Toggle("Disable region restrictions", isOn: bindingForRegionRestriction())
@@ -318,4 +323,11 @@ struct EditorView: View {
             }
         )
     }
+
+    private func requiresVersion(_ major : Int, _ minor: Int = 0, _ patch: Int = 0) -> Bool {
+        // XXYYZZ: major XX, minor YY, patch ZZ
+        let requiredVersion = major*10000 + minor*100 + patch
+        let currentVersion = os.majorVersion*10000 + os.minorVersion*100 + os.patchVersion
+        return currentVersion < requiredVersion
+    }
 }
  ```

  </details>


---

## References

### Commits (1)

- [9fc6ffb](https://github.com/letienndat/lara/commit/9fc6ffb8e54049fe785acc3b6361633ea2578ce1) — Squashed commit of the following:


---

## Timeline (Verbose)

- : 📝 Commit 24d3107: Add version checks in EditorView
- : 📝 Commit c1f98e8: static -> private
- Apr 27, 2026: 🟢 @jurre111 marked ready for review
- Apr 27, 2026: 👀 @jurre111 requested review from @Copilot
- : 💬 someone reviewed: commented
- Apr 27, 2026: 🟣 @rooootdev merged
- Apr 27, 2026: 🔴 Closed by @rooootdev
- Apr 27, 2026: 🗑️ @jurre111 deleted head branch
- Apr 29, 2026: 🔗 Commit 9fc6ffb — Squashed commit of the following:


---

*Exported on 10/14/2026, 3:03:14 PM from [https://github.com/rooootdev/lara/pull/202](https://github.com/rooootdev/lara/pull/202)*
