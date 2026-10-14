# Improvements to EditorView (revert will be a later PR)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#217](https://github.com/rooootdev/lara/pull/217)  
> **State:** 🟣 merged  
> **Branch:** `jurre111:mbg` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on May 1, 2026  
> **Changes:** +14 / -27 across 1 files  
> **Created:** May 1, 2026  
> **Updated:** May 1, 2026  
> **Closed:** May 1, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on May 1, 2026*


Sorry for the load of commits, but:

- Added 15 Pro Max & X Gestures SubTypes (more are welcome)
- Picker -> Menu
- Disabling options in the menu based on device specs and version


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25212292192/job/73925334429) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25212292192/job/73925260579) — **success** — *GitHub Actions*


---

## Reviews (3)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · May 1, 2026

## Pull request overview

Updates the MobileGestalt editor UI to expand Dynamic Island subtype options, switch the selection UI from a `Picker` to a `Menu`, and gate options based on OS/device capabilities; also adjusts the CI workflow trigger behavior.

**Changes:**
- Replace the subtype `Picker` with a `Menu`-driven selection UI.
- Add additional subtype IDs/names and disable menu options based on OS version / device characteristics.
- Update the build workflow to run on `push` events without restricting to `main`.

### Reviewed changes

Copilot reviewed 1 out of 2 changed files in this pull request and generated 2 comments.

| File | Description |
| ---- | ----------- |
| lara/views/EditorView.swift | Adds more Dynamic Island subtype choices, moves selection UI to a `Menu`, and introduces per-option disabling logic. |
| .github/workflows/build.yml | Removes the `push` branch filter, changing when the build/release workflow runs. |





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.

---

### 💬 COMMENTED — [@jurre111](https://github.com/jurre111) · May 1, 2026

---

### 💬 COMMENTED — [@jurre111](https://github.com/jurre111) · May 1, 2026


---

## Review Comments (4)

*Inline comments on code changes*

### `lara/views/EditorView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 1, 2026 (lines 25-27) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -18,28 +18,13 @@ struct EditorView: View {
     @State private var valid: Bool = true
     @AppStorage("ogSubType") private var ogSubType: Int = -1
     @State private var selectedSubType: Int = -1
-
-    enum SubType: Int, CaseIterable, Identifiable {
-        case iPhone14Pro = 2556
-        case iPhone14ProMax = 2796
-        case iPhone16Pro = 2622
-        case iPhone16ProMax = 2868
-        // X gestures for SE?
-
-        var id: Int { self.rawValue }
-        var displayName: String {
-            switch self {
-            case .iPhone14Pro: return "14 Pro (2556)"
-            case .iPhone14ProMax: return "14 Pro Max (2796)"
-            case .iPhone16Pro: return "iOS 18+:\n16 Pro (2622)"
-            case .iPhone16ProMax: return "iOS 18+:\n16 Pro Max (2868)"
-            }
-        }
-    }
     
     private let path = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist"
     private let ogmgurl: URL
     let os = ProcessInfo().operatingSystemVersion
+    var subtypes = [2556, 2796, 2976, 2622, 2868, 2436]
+    var subtypeNames = [2556: "14 Pro (2556)", 2796: "14 Pro Max (2796)", 2976: "15 Pro Max (2976)", 2622: "16 Pro (2622)", 2868: "16 Pro Max (2868)", 2436: "X Gestures (2436)"]
+    var subtypeDisabled: [Int: Bool] = [:]
```

</details>

`subtypes`, `subtypeNames`, and `subtypeDisabled` are separate sources of truth. This makes it easy to add a new subtype but forget to add its name/disabled rule, which currently results in a "??" label and (because of `?? true`) an option that is always disabled. Consider consolidating into a single collection (e.g., an array of structs/tuples containing id, displayName, and disabled predicate) so options can’t get out of sync.

  **[@jurre111](https://github.com/jurre111)** · May 1, 2026 (lines 25-27) *(reply)* · review: COMMENTED

  A dict will not be ordered and with an array you'll have to iterate over it to remove the ogSubtype.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 1, 2026 (line 59) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -69,7 +54,9 @@ struct EditorView: View {
         if ogSubType == -1 {
             ogSubType = subType
         }
-
+        let originalSubType = ogSubType
+        subtypes.removeAll { $0 == originalSubType }
+        subtypeDisabled = [2556: requiresVersion(16), 2796: requiresVersion(16), 2976: requiresVersion(17), 2622: requiresVersion(18), 2868: requiresVersion(18), 2436: !UIDevice._hasHomeButton()]
```

</details>

`UIDevice._hasHomeButton()` is referenced here but there is no definition for it in the repository, so this will not compile as-is. Also, the disable condition for the “X Gestures (2436)” subtype looks inverted: if this subtype is intended for gesture-navigation devices, it should be disabled when the device *has* a Home button rather than when it doesn’t.

  **[@jurre111](https://github.com/jurre111)** · May 1, 2026 (line 59) *(reply)* · review: COMMENTED

  It's working fine as-is. No change needed.



---

## PR Commits (17)

- [`dd1adee`](https://github.com/rooootdev/lara/commit/dd1adeeb95f8f7a9735db022a46026bba7bec54e) Update build.yml to allow pushes to any branch — *Jurre* · Apr 30, 2026
- [`d13c208`](https://github.com/rooootdev/lara/commit/d13c208ca29e9c362217d9e3d3b9d081677ed7ce) Merge branch 'rooootdev:main' into mbg — *Jurre* · May 1, 2026
- [`dc3008b`](https://github.com/rooootdev/lara/commit/dc3008ba87a97150db2565b270753d503feaaae2) new picker — *Jurre* · May 1, 2026
- [`882d603`](https://github.com/rooootdev/lara/commit/882d6035a9ecbad6afdc89d870d9050ed14ee428) fix — *Jurre* · May 1, 2026
- [`9ef9771`](https://github.com/rooootdev/lara/commit/9ef9771f4a93aa6c8a667d1342dc0c5c6fbafa0a) fix? — *jurre111* · May 1, 2026
- [`3365c8e`](https://github.com/rooootdev/lara/commit/3365c8e3d6efd0aa962ba85df0f14046a1ec88f5) oops — *jurre111* · May 1, 2026
- [`b1a5457`](https://github.com/rooootdev/lara/commit/b1a5457d5c501064e8ae162e5fd5f737f64a39a7) fuck picker; menu? — *jurre111* · May 1, 2026
- [`5f8a226`](https://github.com/rooootdev/lara/commit/5f8a2263065ad17192c9444c5a01a925881aa5c9) oops — *jurre111* · May 1, 2026
- [`0429249`](https://github.com/rooootdev/lara/commit/04292499ac176c25a14f75979cf3706a156b4da5) Fix — *jurre111* · May 1, 2026
- [`618c790`](https://github.com/rooootdev/lara/commit/618c7908a6ed7e0f7f059a951be4c09ca1b4d5d6) ok — *jurre111* · May 1, 2026
- [`cb7032e`](https://github.com/rooootdev/lara/commit/cb7032e794928a14bc47170e4848835d14a090ec) ohh — *jurre111* · May 1, 2026
- [`fb27165`](https://github.com/rooootdev/lara/commit/fb2716564fc0170bce113aa09d3dcf4bd9ddb4d6) ? — *jurre111* · May 1, 2026
- [`78b8dd3`](https://github.com/rooootdev/lara/commit/78b8dd368c1f0de977278c7ccc6e3e89768e1d4b) yes — *jurre111* · May 1, 2026
- [`84b9d06`](https://github.com/rooootdev/lara/commit/84b9d067b468e7daac22ed72cd74481fa4771174) more yes — *jurre111* · May 1, 2026
- [`758b2f7`](https://github.com/rooootdev/lara/commit/758b2f7ecd2936e1fee2999d83abe370f4808d18) change build.yml back — *jurre111* · May 1, 2026
- [`f28e0e7`](https://github.com/rooootdev/lara/commit/f28e0e7bb7fa4b333d34a5cf522d5e981b8a9dbd) now it should be back to normal — *jurre111* · May 1, 2026
- [`a2bd04c`](https://github.com/rooootdev/lara/commit/a2bd04cac54fc43a5a7a099b1c92d930bfb717a0) Fix footer — *jurre111* · May 1, 2026


---

## Changed Files (1)

- 🟡 `lara/views/EditorView.swift` (modified) — +14 / -27
  <details><summary>Diff</summary>

  ```diff
  @@ -18,28 +18,13 @@ struct EditorView: View {
     @State private var valid: Bool = true
     @AppStorage("ogSubType") private var ogSubType: Int = -1
     @State private var selectedSubType: Int = -1
-
-    enum SubType: Int, CaseIterable, Identifiable {
-        case iPhone14Pro = 2556
-        case iPhone14ProMax = 2796
-        case iPhone16Pro = 2622
-        case iPhone16ProMax = 2868
-        // X gestures for SE?
-
-        var id: Int { self.rawValue }
-        var displayName: String {
-            switch self {
-            case .iPhone14Pro: return "14 Pro (2556)"
-            case .iPhone14ProMax: return "14 Pro Max (2796)"
-            case .iPhone16Pro: return "iOS 18+:\n16 Pro (2622)"
-            case .iPhone16ProMax: return "iOS 18+:\n16 Pro Max (2868)"
-            }
-        }
-    }
     
     private let path = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist"
     private let ogmgurl: URL
     let os = ProcessInfo().operatingSystemVersion
+    var subtypes = [2556, 2796, 2976, 2622, 2868, 2436]
+    var subtypeNames = [2556: "14 Pro (2556)", 2796: "14 Pro Max (2796)", 2976: "15 Pro Max (2976)", 2622: "16 Pro (2622)", 2868: "16 Pro Max (2868)", 2436: "X Gestures (2436)"]
+    var subtypeDisabled: [Int: Bool] = [:]
 
     init() {
         let docs = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
@@ -69,7 +54,9 @@ struct EditorView: View {
         if ogSubType == -1 {
             ogSubType = subType
         }
-
+        let originalSubType = ogSubType
+        subtypes.removeAll { $0 == originalSubType }
+        subtypeDisabled = [2556: requiresVersion(16), 2796: requiresVersion(16), 2976: requiresVersion(17), 2622: requiresVersion(18), 2868: requiresVersion(18), 2436: !UIDevice._hasHomeButton()]
     }
 
     var body: some View {
@@ -78,16 +65,16 @@ struct EditorView: View {
                 Section {
                     HStack {
                         Text("Dynamic Island")
-                        
                         Spacer()
-                        
-                        Picker("", selection: $selectedSubType) {
-                            Text("Original (\(String(ogSubType)))").tag(ogSubType)
-                            ForEach(SubType.allCases.filter { $0.rawValue != ogSubType }) { subtype in
-                                Text(subtype.displayName).tag(subtype.rawValue)
+                        Menu {
+                            Button("Original (\(String(ogSubType)))") { selectedSubType = ogSubType }
+                            ForEach(subtypes, id: \.self) { subtype in
+                                Button(subtypeNames[subtype] ?? "??") { selectedSubType = subtype }
+                                    .disabled(subtypeDisabled[subtype] ?? true)
                             }
+                        } label: {
+                            Text(selectedSubType == ogSubType ? "Original (\(String(selectedSubType)))" :  (subtypeNames[selectedSubType] ?? "Unknown (\(String(selectedSubType)))"))
                         }
-                        .pickerStyle(.menu)
                     }
                     Toggle("Action Button", isOn: mgkeybinding(["cT44WE1EohiwRzhsZ8xEsw"]))
                         .disabled(requiresVersion(17))
@@ -117,7 +104,7 @@ struct EditorView: View {
                 } header: {
                     Text("MobileGestalt")
                 } footer: {
-                    Text("Note: some tweaks may not work or cause instability.\nWARNING: Never enable features your device doesn't support.")
+                    Text("Note: some tweaks may not work or cause instability.")
                 }
                 Section {
                     let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary
  ```

  </details>


---

## References

### Commits (1)

- [ace3da5](https://github.com/letienndat/lara/commit/ace3da5e57504fd67743a615d1745349fef8ac44) — Squashed commit of the following:


---

## Timeline (Verbose)

- : 📝 Commit dd1adee: Update build.yml to allow pushes to any branch
- : 📝 Commit d13c208: Merge branch 'rooootdev:main' into mbg
- : 📝 Commit dc3008b: new picker
- : 📝 Commit 882d603: fix
- : 📝 Commit 9ef9771: fix?
- : 📝 Commit 3365c8e: oops
- : 📝 Commit b1a5457: fuck picker; menu?
- : 📝 Commit 5f8a226: oops
- : 📝 Commit 0429249: Fix
- : 📝 Commit 618c790: ok
- : 📝 Commit cb7032e: ohh
- : 📝 Commit fb27165: ?
- : 📝 Commit 78b8dd3: yes
- : 📝 Commit 84b9d06: more yes
- May 1, 2026: 👀 @jurre111 requested review from @Copilot
- : 📝 Commit 758b2f7: change build.yml back
- : 💬 someone reviewed: commented
- : 📝 Commit f28e0e7: now it should be back to normal
- : 📝 Commit a2bd04c: Fix footer
- May 1, 2026: 🟣 @rooootdev merged
- May 1, 2026: 🔴 Closed by @rooootdev
- May 4, 2026: 🔗 Commit ace3da5 — Squashed commit of the following:


---

## Comments (1)

### Comment #1 — [@jurre111](https://github.com/jurre111) · May 1, 2026

`ID: 4358971305`


wait lemme change build.yml back


---

*Exported on 10/14/2026, 3:04:37 PM from [https://github.com/rooootdev/lara/pull/217](https://github.com/rooootdev/lara/pull/217)*
