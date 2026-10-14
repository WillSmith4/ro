# fix: selectedmethod not persisted in ContentView and SantanderView (@State → @AppStorage)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#178](https://github.com/rooootdev/lara/pull/178)  
> **State:** 🟣 merged  
> **Branch:** `tyhallcsu:fix/appstorage-method-selection` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 23, 2026  
> **Changes:** +2 / -2 across 2 files  
> **Created:** Apr 23, 2026  
> **Updated:** Apr 23, 2026  
> **Closed:** Apr 23, 2026  

---

## Description

*Opened by [@tyhallcsu](https://github.com/tyhallcsu)* *on Apr 23, 2026*


## Problem

`ContentView` and `SantanderView` both declare `selectedmethod` as `@State` with a hard-coded default, instead of `@AppStorage`. This means the VFS/SBX/Hybrid method the user picks in Settings is **never actually applied** in either view — they always use their default values.

```swift
// ContentView.swift:16 — always .hybrid, ignores Settings
@State private var selectedmethod: method = .hybrid

// SantanderView.swift:17 — always .sbx, ignores Settings
@State private var selectedmethod: method = .sbx
```

All other uses of this setting (`lara.swift`, `SettingsView.swift`, `AppsView.swift`) correctly use `@AppStorage("selectedmethod")`.

`SantanderView` even has an explicit `UserDefaults.didChangeNotification` workaround (`refreshSelectedMethod()`, lines 50–52) that compensates for the missing `@AppStorage` — confirming this was a known issue. With `@AppStorage` in place that workaround is now redundant but harmless; left for the maintainer to clean up.

## Fix

Change both declarations to `@AppStorage("selectedmethod")` with a consistent default of `.hybrid` (matching `lara.swift` and `SettingsView`).

## Verification

1. Install and open the app
2. Go to Settings → change Method to "VFS"
3. Return to the main tab
4. The step-sequence should now show "Initialise VFS" (not "Escape Sandbox")
5. Repeat with "SBX" — should show "Escape Sandbox"


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`0e679d7`](https://github.com/rooootdev/lara/commit/0e679d7b86ff33dbc076d8cb22d868a8c98f7dd1) fix: use @AppStorage for selectedmethod in ContentView and SantanderView — *tyhallcsu* · Apr 23, 2026


---

## Changed Files (2)

- 🟡 `lara/views/ContentView.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -13,7 +13,7 @@ struct ContentView: View {
     @ObservedObject private var mgr = laramgr.shared
     @State private var hasoffsets = true
     @State private var showsettings = false
-    @State private var selectedmethod: method = .hybrid
+    @AppStorage("selectedmethod") private var selectedmethod: method = .hybrid
 
     var body: some View {
         NavigationStack {
  ```

  </details>
- 🟡 `lara/views/SantanderView.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -14,7 +14,7 @@ import QuickLook
 
 struct SantanderView: View {
     let startPath: String
-    @State private var selectedmethod: method = .sbx
+    @AppStorage("selectedmethod") private var selectedmethod: method = .hybrid
     @ObservedObject private var mgr = laramgr.shared
 
     init(startPath: String = "/") {
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 0e679d7: fix: use @AppStorage for selectedmethod in ContentView and SantanderView
- Apr 23, 2026: 🟣 @rooootdev merged
- Apr 23, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:00:57 PM from [https://github.com/rooootdev/lara/pull/178](https://github.com/rooootdev/lara/pull/178)*
