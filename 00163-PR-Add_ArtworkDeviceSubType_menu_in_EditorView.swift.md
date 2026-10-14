# Add ArtworkDeviceSubType menu in EditorView.swift

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#163](https://github.com/rooootdev/lara/pull/163)  
> **State:** 🟣 merged  
> **Branch:** `jurre111:mbg` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 22, 2026  
> **Changes:** +60 / -4 across 1 files  
> **Created:** Apr 21, 2026  
> **Updated:** Apr 22, 2026  
> **Closed:** Apr 22, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on Apr 21, 2026*


Seems to be working fine after some testing


---

## Checks & Status

- Overall: 🟡 **pending**


---

## Reviews (2)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 21, 2026

## Pull request overview

This PR adds a UI control in `EditorView` to edit the `ArtworkDeviceSubType` (used for Dynamic Island-related behavior) by introducing a menu-based Picker and persisting the original subtype value.

**Changes:**
- Add `SubType` enum + Picker UI for selecting an `ArtworkDeviceSubType` value.
- Persist an “original subtype” using `@AppStorage` and initialize selection from the MobileGestalt plist.
- Write `ArtworkDeviceSubType` into the plist during `apply()`; minor label/warning text updates.





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.

---

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 21, 2026

## Pull request overview

Adds a UI control to edit `ArtworkDeviceSubType` (used for Dynamic Island behavior) from within `EditorView`, and wires that selection into the MobileGestalt apply flow.

**Changes:**
- Add `ArtworkDeviceSubType` picker (menu) backed by a small `SubType` enum.
- Read current `ArtworkDeviceSubType` on view init and persist an “original” value via `@AppStorage`.
- Write `ArtworkDeviceSubType` back into the MobileGestalt dictionary during `apply()`.





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (10)

*Inline comments on code changes*

### `lara/views/EditorView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (line 89) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -35,24 +55,44 @@ struct EditorView: View {
             _mg = State(initialValue: [:])
             _status = State(initialValue: "Failed to copy MobileGestalt: \(error)")
         }
+        guard let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary, let oPeik = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary else {
+            return
+        }
+        _selectedSubType = State(initialValue: oPeik["ArtworkDeviceSubType"] as? Int ?? -1)
+        if ogSubType == -1 {
+            ogSubType = selectedSubType
+        }
 
     }
 
     var body: some View {
         NavigationStack {
             List {
                 Section {
-                    Toggle("Action Button (iOS 17+)", isOn: mgkeybinding(["cT44WE1EohiwRzhsZ8xEsw"]))
+                    HStack {
+                        Text("Dynamic Island")
+                        
+                        Spacer()
+                        
+                        Picker("", selection: $selectedSubType) {
+                            Text("Original (\(String(ogSubType)))").tag(ogSubType)
+                            ForEach(SubType.allCases) { subtype in
+                                Text(subtype.displayName).tag(subtype.rawValue)
+                            }
+                        }
+                        .pickerStyle(.menu)
```

</details>

The Picker uses an empty label (""), which leaves it without a meaningful accessibility label; VoiceOver users may not understand what the menu controls. Give the Picker a real label (e.g., "Dynamic Island") and hide the visible label if needed, or add an explicit `.accessibilityLabel(...)` tied to this control.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (lines 84-86) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -35,24 +55,44 @@ struct EditorView: View {
             _mg = State(initialValue: [:])
             _status = State(initialValue: "Failed to copy MobileGestalt: \(error)")
         }
+        guard let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary, let oPeik = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary else {
+            return
+        }
+        _selectedSubType = State(initialValue: oPeik["ArtworkDeviceSubType"] as? Int ?? -1)
+        if ogSubType == -1 {
+            ogSubType = selectedSubType
+        }
 
     }
 
     var body: some View {
         NavigationStack {
             List {
                 Section {
-                    Toggle("Action Button (iOS 17+)", isOn: mgkeybinding(["cT44WE1EohiwRzhsZ8xEsw"]))
+                    HStack {
+                        Text("Dynamic Island")
+                        
+                        Spacer()
+                        
+                        Picker("", selection: $selectedSubType) {
+                            Text("Original (\(String(ogSubType)))").tag(ogSubType)
+                            ForEach(SubType.allCases) { subtype in
+                                Text(subtype.displayName).tag(subtype.rawValue)
```

</details>

The "Original" option is always inserted with `.tag(ogSubType)`, but if `ogSubType` matches one of `SubType.allCases` raw values, the Picker will end up with duplicate tags/values. This can cause SwiftUI selection ambiguity and a confusing menu (two items that select the same value). Consider only showing the separate "Original" row when it’s not already in `SubType.allCases`, or annotate the matching subtype’s label as "(Original)" instead.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (line 133) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -90,7 +130,7 @@ struct EditorView: View {
                 } header: {
                     Text("Apply")
                 } footer: {
-                    Text("Use at your own risk.")
+                    Text("Use at your own risk. Always keep a backup of you MobileGestalt somewhere safe.")
```

</details>

The footer text has a grammatical typo: "backup of you MobileGestalt" should be "backup of your MobileGestalt" (and likely refers to the plist).


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (line 206) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -161,6 +201,10 @@ struct EditorView: View {
 
     private func apply() {
         do {
+            guard let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary, let oPeik = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary else {
+                return
+            }
```

</details>

If `CacheExtra` / `oPeik/...` is missing, `apply()` returns silently. From the user’s perspective, tapping "Apply" will do nothing with no feedback. Set `status` (or another visible error) explaining why applying can’t proceed, so failures aren’t silent.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (line 207) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -161,6 +201,10 @@ struct EditorView: View {
 
     private func apply() {
         do {
+            guard let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary, let oPeik = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary else {
+                return
+            }
+            oPeik["ArtworkDeviceSubType"] = selectedSubType
```

</details>

`selectedSubType` is initialized to `-1` when `ArtworkDeviceSubType` is missing/non-Int, and `apply()` will then write `-1` into the plist if the user taps Apply without changing the Picker. Unless `-1` is a known valid value here, this can unintentionally corrupt the setting. Consider treating `-1` as “unset” (don’t write / remove the key) and/or disabling Apply until a valid subtype is selected.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (line 84) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -35,24 +55,50 @@ struct EditorView: View {
             _mg = State(initialValue: [:])
             _status = State(initialValue: "Failed to copy MobileGestalt: \(error)")
         }
+        guard let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary, let oPeik = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary else {
+            _status = State(initialValue: "Failed to get dictionaries from MobileGestalt. Reopen the page.")
+            return
+        }
+        guard let subType = oPeik["ArtworkDeviceSubType"] as? Int else {
+            _status = State(initialValue: "Failed to get SubType from MobileGestalt. Reopen the page.")
+            return
+        }
+        _selectedSubType = State(initialValue: subType)
+        // This only happens on the first load
+        if ogSubType == -1 {
+            ogSubType = selectedSubType
+        }
 
     }
 
     var body: some View {
         NavigationStack {
             List {
                 Section {
-                    Toggle("Action Button (iOS 17+)", isOn: mgkeybinding(["cT44WE1EohiwRzhsZ8xEsw"]))
+                    HStack {
+                        Text("Dynamic Island")
+                        
+                        Spacer()
+                        
+                        Picker("", selection: $selectedSubType) {
+                            Text("Original (\(String(ogSubType)))").tag(ogSubType)
```

</details>

`Picker` includes an "Original" option tagged with `ogSubType`, but the `ForEach(SubType.allCases)` may contain the same raw value. Duplicate `.tag(...)` values can make the menu selection ambiguous and show/choose the wrong row. Consider only showing the "Original" entry when its value isn’t already present in `SubType.allCases`, or make the selection type distinct (e.g., an enum with an `.original(Int)` case) so tags are unique.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (lines 83-88) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -35,24 +55,50 @@ struct EditorView: View {
             _mg = State(initialValue: [:])
             _status = State(initialValue: "Failed to copy MobileGestalt: \(error)")
         }
+        guard let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary, let oPeik = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary else {
+            _status = State(initialValue: "Failed to get dictionaries from MobileGestalt. Reopen the page.")
+            return
+        }
+        guard let subType = oPeik["ArtworkDeviceSubType"] as? Int else {
+            _status = State(initialValue: "Failed to get SubType from MobileGestalt. Reopen the page.")
+            return
+        }
+        _selectedSubType = State(initialValue: subType)
+        // This only happens on the first load
+        if ogSubType == -1 {
+            ogSubType = selectedSubType
+        }
 
     }
 
     var body: some View {
         NavigationStack {
             List {
                 Section {
-                    Toggle("Action Button (iOS 17+)", isOn: mgkeybinding(["cT44WE1EohiwRzhsZ8xEsw"]))
+                    HStack {
+                        Text("Dynamic Island")
+                        
+                        Spacer()
+                        
+                        Picker("", selection: $selectedSubType) {
+                            Text("Original (\(String(ogSubType)))").tag(ogSubType)
+                            ForEach(SubType.allCases) { subtype in
+                                Text(subtype.displayName).tag(subtype.rawValue)
+                            }
+                        }
```

</details>

The `Picker` is created with an empty label (`Picker("", ...)`). This makes it harder for VoiceOver to announce what the control is for. Use a meaningful label (e.g., "Dynamic Island") and hide it visually with `.labelsHidden()` (or add an explicit `.accessibilityLabel(...)`) so the row stays accessible.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (line 69) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -35,24 +55,50 @@ struct EditorView: View {
             _mg = State(initialValue: [:])
             _status = State(initialValue: "Failed to copy MobileGestalt: \(error)")
         }
+        guard let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary, let oPeik = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary else {
+            _status = State(initialValue: "Failed to get dictionaries from MobileGestalt. Reopen the page.")
+            return
+        }
+        guard let subType = oPeik["ArtworkDeviceSubType"] as? Int else {
+            _status = State(initialValue: "Failed to get SubType from MobileGestalt. Reopen the page.")
+            return
+        }
+        _selectedSubType = State(initialValue: subType)
+        // This only happens on the first load
+        if ogSubType == -1 {
+            ogSubType = selectedSubType
```

</details>

In `init()`, `ogSubType` is set from `selectedSubType` right after `_selectedSubType` is initialized. Reading the `@State`-backed `selectedSubType` here is unnecessary and can be fragile during initialization. Assign `ogSubType` directly from the local `subType` value you just read from the plist to avoid ever persisting `-1` or an out-of-sync value.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (lines 211-216) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -161,6 +207,16 @@ struct EditorView: View {
 
     private func apply() {
         do {
+            if selectedSubType != -1 {
+                guard let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary, let oPeik = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary else {
+                    status = "Failed to get dictionaries from MobileGestalt."
+                    return
+                }
+                oPeik["ArtworkDeviceSubType"] = selectedSubType
+            } else {
```

</details>

The MobileGestalt key path strings (`"CacheExtra"`, `"oPeik/9e8lQWMszEjbPzng"`, `"ArtworkDeviceSubType"`) are duplicated in multiple places (init + apply). It’s easy for these to drift if one changes. Consider hoisting them into `private let` constants (or a small helper) and reusing them in both locations.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (lines 210-219) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -161,6 +207,16 @@ struct EditorView: View {
 
     private func apply() {
         do {
+            if selectedSubType != -1 {
+                guard let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary, let oPeik = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary else {
+                    status = "Failed to get dictionaries from MobileGestalt."
+                    return
+                }
+                oPeik["ArtworkDeviceSubType"] = selectedSubType
+            } else {
+                status = "Selected SubType is -1? Reload the page."
+                return
+            }
```

</details>

The status message "Selected SubType is -1? Reload the page." reads like an internal assertion and may not help users recover. Consider a clearer user-facing message (e.g., indicate that the MobileGestalt SubType couldn’t be loaded and the view should be reopened) and/or disable the Apply button until `selectedSubType` is initialized.



---

## PR Commits (9)

- [`19f251b`](https://github.com/rooootdev/lara/commit/19f251b9ccd21136984e152c933dc78a8616bade) Add ArtworkDeviceSubtype selector — *Jurre* · Apr 21, 2026
- [`cea3e75`](https://github.com/rooootdev/lara/commit/cea3e7585035b6f36fb9b5b18ed7cbd949e9ada3) Change build branch from 'main' to 'mbg' — *Jurre* · Apr 21, 2026
- [`0ae69a3`](https://github.com/rooootdev/lara/commit/0ae69a3acbbb7d3ba82640ce53933fde92e36f8e) Merge branch 'rooootdev:main' into mbg — *Jurre* · Apr 21, 2026
- [`6b0d9e9`](https://github.com/rooootdev/lara/commit/6b0d9e934d3efe00860631443abedb0064ebb30e) Update build.yml — *Jurre* · Apr 21, 2026
- [`d9dd08f`](https://github.com/rooootdev/lara/commit/d9dd08f384a7c56c7a9f611ab61061bbb381512f) fix typo — *Jurre* · Apr 21, 2026
- [`b1eb56e`](https://github.com/rooootdev/lara/commit/b1eb56e72e8e295173cac0faa4af0d2fdb975751) Enhance error handling — *Jurre* · Apr 21, 2026
- [`d9b6e47`](https://github.com/rooootdev/lara/commit/d9b6e47da6aafa7fe0d8cdc8b092ea28ff4e9338) Fix error — *Jurre* · Apr 21, 2026
- [`8690b61`](https://github.com/rooootdev/lara/commit/8690b6151f0d2713f0ab77372a66e66ad4afb56e) Fix init() — *Jurre* · Apr 21, 2026
- [`1f08a7a`](https://github.com/rooootdev/lara/commit/1f08a7a68b6d4253fa0314dda54bf2b74943a257) Avoid double options in SubType menu — *Jurre* · Apr 21, 2026


---

## Changed Files (1)

- 🟡 `lara/views/EditorView.swift` (modified) — +60 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -16,6 +16,26 @@ struct EditorView: View {
     @State private var status: String?
     @State private var alert: String?
     @State private var valid: Bool = true
+    @AppStorage("ogSubType") private var ogSubType: Int = -1
+    @State private var selectedSubType: Int = -1
+
+    enum SubType: Int, CaseIterable, Identifiable {
+        case iPhone14Pro = 2556
+        case iPhone14ProMax = 2796
+        case iPhone16Pro = 2622
+        case iPhone16ProMax = 2868
+        // X gestures for SE?
+
+        var id: Int { self.rawValue }
+        var displayName: String {
+            switch self {
+            case .iPhone14Pro: return "14 Pro (2556)"
+            case .iPhone14ProMax: return "14 Pro Max (2796)"
+            case .iPhone16Pro: return "iOS 18+:\n16 Pro (2622)"
+            case .iPhone16ProMax: return "iOS 18+:\n16 Pro Max (2868)"
+            }
+        }
+    }
     
     private let path = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist"
     private let ogmgurl: URL
@@ -35,24 +55,50 @@ struct EditorView: View {
             _mg = State(initialValue: [:])
             _status = State(initialValue: "Failed to copy MobileGestalt: \(error)")
         }
+        guard let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary, let oPeik = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary else {
+            _status = State(initialValue: "Failed to get dictionaries from MobileGestalt. Reopen the page.")
+            return
+        }
+        guard let subType = oPeik["ArtworkDeviceSubType"] as? Int else {
+            _status = State(initialValue: "Failed to get SubType from MobileGestalt. Reopen the page.")
+            return
+        }
+        _selectedSubType = State(initialValue: subType)
+        // This only happens on the first load
+        if ogSubType == -1 {
+            ogSubType = subType
+        }
 
     }
 
     var body: some View {
         NavigationStack {
             List {
                 Section {
-                    Toggle("Action Button (iOS 17+)", isOn: mgkeybinding(["cT44WE1EohiwRzhsZ8xEsw"]))
+                    HStack {
+                        Text("Dynamic Island")
+                        
+                        Spacer()
+                        
+                        Picker("", selection: $selectedSubType) {
+                            Text("Original (\(String(ogSubType)))").tag(ogSubType)
+                            ForEach(SubType.allCases.filter { $0.rawValue != ogSubType }) { subtype in
+                                Text(subtype.displayName).tag(subtype.rawValue)
+                            }
+                        }
+                        .pickerStyle(.menu)
+                    }
+                    Toggle("Action Button (17+)", isOn: mgkeybinding(["cT44WE1EohiwRzhsZ8xEsw"]))
                     Toggle("Allow installing iPadOS apps", isOn: mgkeybinding(["9MZ5AdH43csAUajl/dU+IQ"], type: [Int].self, default: [1], enable: [1, 2]))
                     Toggle("Always on Display (18.0+)", isOn: mgkeybinding(["j8/Omm6s1lsmTDFsXjsBfA", "2OOJf1VhaM7NxfRok3HbWQ"]))
                     // Toggle("Apple Intelligence", isOn: bindingForAppleIntelligence())
                     //    .disabled(requiresVersion(18))
                     Toggle("Apple Pencil", isOn: mgkeybinding(["yhHcB0iH0d1XzPO/CFd3ow"]))
                     Toggle("Boot chime", isOn: mgkeybinding(["QHxt+hGLaBPbQJbXiUJX3w"]))
                     Toggle("Camera button (18.0rc+)", isOn: mgkeybinding(["CwvKxM2cEogD3p+HYgaW0Q", "oOV1jhJbdV3AddkcCg0AEA"]))
-                    Toggle("Charge limit (iOS 17+)", isOn: mgkeybinding(["37NVydb//GP/GrhuTN+exg"]))
+                    Toggle("Charge limit (17+)", isOn: mgkeybinding(["37NVydb//GP/GrhuTN+exg"]))
                     Toggle("Crash Detection (might not work)", isOn: mgkeybinding(["HCzWusHQwZDea6nNhaKndw"]))
-                    Toggle("Dynamic Island (17.4+, might not work)", isOn: mgkeybinding(["YlEtTtHlNesRBMal1CqRaA"]))
+                    // Toggle("Dynamic Island (17.4+, might not work)", isOn: mgkeybinding(["YlEtTtHlNesRBMal1CqRaA"]))
                     // Toggle("Disable region restrictions", isOn: bindingForRegionRestriction())
                     Toggle("Internal Storage info", isOn: mgkeybinding(["LBJfwOEzExRxzlAnSuI7eg"]))
                     // Toggle("Internal stuff", isOn: bindingForInternalStuff())
@@ -90,7 +136,7 @@ struct EditorView: View {
                 } header: {
                     Text("Apply")
                 } footer: {
-                    Text("Use at your own risk.")
+                    Text("Use at your own risk. Always keep a backup of your MobileGestalt somewhere safe.")
                 }
                 
                 HStack(alignment: .top) {
@@ -161,6 +207,16 @@ struct EditorView: View {
 
     private func apply() {
         do {
+            if selectedSubType != -1 {
+                guard let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary, let oPeik = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary else {
+                    status = "Failed to get dictionaries from MobileGestalt."
+                    return
+                }
+                oPeik["ArtworkDeviceSubType"] = selectedSubType
+            } else {
+                status = "Selected SubType is -1? Reload the page."
+                return
+            }
             let data = try PropertyListSerialization.data(
                 fromPropertyList: mg,
                 format: .binary,
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 19f251b: Add ArtworkDeviceSubtype selector
- : 📝 Commit cea3e75: Change build branch from 'main' to 'mbg'
- : 📝 Commit 0ae69a3: Merge branch 'rooootdev:main' into mbg
- : 📝 Commit 6b0d9e9: Update build.yml
- Apr 21, 2026: 👀 @jurre111 requested review from @Copilot
- : 💬 someone reviewed: commented
- : 📝 Commit d9dd08f: fix typo
- : 📝 Commit b1eb56e: Enhance error handling
- : 📝 Commit d9b6e47: Fix error
- Apr 21, 2026: 👀 @jurre111 requested review from @Copilot
- : 💬 someone reviewed: commented
- : 📝 Commit 8690b61: Fix init()
- : 📝 Commit 1f08a7a: Avoid double options in SubType menu
- Apr 22, 2026: 🟣 @rooootdev merged
- Apr 22, 2026: 🔴 Closed by @rooootdev


---

## Comments (1)

### Comment #1 — [@jurre111](https://github.com/jurre111) · Apr 21, 2026

`ID: 4291486708`


Should be done now


---

*Exported on 10/14/2026, 2:59:29 PM from [https://github.com/rooootdev/lara/pull/163](https://github.com/rooootdev/lara/pull/163)*
