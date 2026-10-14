# Some improvements to the file manager

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#194](https://github.com/rooootdev/lara/pull/194)  
> **State:** 🟣 merged  
> **Branch:** `jurre111:Improved-File-Manager` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 26, 2026  
> **Changes:** +121 / -16 across 2 files  
> **Created:** Apr 25, 2026  
> **Updated:** Apr 26, 2026  
> **Closed:** Apr 26, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on Apr 25, 2026*


- Make recursive search optional
- Make app folders display with bundle id or app name

This took me way longer than necessary and the code is not my best work, but it works.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24941527411/job/73036043293) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24941527411/job/73035937747) — **success** — *GitHub Actions*


---

## Reviews (3)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 25, 2026

## Pull request overview

This PR adds user-configurable options to the in-app file manager for (1) whether search should be recursive and (2) how iOS app container folders are displayed (UUID vs bundle ID vs app name).

**Changes:**
- Added Settings toggles for recursive search and a picker for app-folder display mode.
- Extended `SantanderPath` with a `displayName` and updated sorting/cell rendering to use it.
- Implemented non-recursive search path and app-container display-name resolution in SBX directory listing.

### Reviewed changes

Copilot reviewed 2 out of 2 changed files in this pull request and generated 4 comments.

| File | Description |
| ---- | ----------- |
| `lara/views/SettingsView.swift` | Adds settings UI and persists new file-manager preferences; introduces `fmAppsDisplayMode`. |
| `lara/views/SantanderView.swift` | Implements optional recursive search and app-folder display-name mapping via `displayName`. |





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.

---

### 💬 COMMENTED — [@jurre111](https://github.com/jurre111) · Apr 25, 2026

---

### 💬 COMMENTED — [@jurre111](https://github.com/jurre111) · Apr 25, 2026


---

## Review Comments (6)

*Inline comments on code changes*

### `lara/views/SantanderView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 25, 2026 (line 134) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -129,6 +131,8 @@ final class SantanderPathListViewController: UITableViewController, UISearchResu
     private var initialEmptyStateMessage: String?
     private var isSearching = false
     private var displayHiddenFiles = true
+    @AppStorage("selectedFmAppsDisplayMode") private var selectedFmAppsDisplayMode: fmAppsDisplayMode = .appName
```

</details>

`selectedFmAppsDisplayMode` is declared with `@AppStorage` on the view controller but isn’t used; the display mode is instead read directly from `UserDefaults` inside `loadDirectoryContentsSBX`. This duplication makes the source of truth unclear—either remove the unused property or refactor `loadDirectoryContents*` to accept the mode as an argument (and use the property).
```suggestion

```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 25, 2026 (lines 386-420) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -367,11 +384,79 @@ final class SantanderPathListViewController: UITableViewController, UISearchResu
         }
         do {
             let entries = try fm.contentsOfDirectory(atPath: path.path)
+            let modeRaw = UserDefaults.standard.string(forKey: "selectedFmAppsDisplayMode")
+            let mode = fmAppsDisplayMode(rawValue: modeRaw ?? "") ?? .appName
+
+            let dataAppPaths = [
+                "/private/var/mobile/Containers/Data/Application",
+                "/var/mobile/Containers/Data/Application"
+            ]
+            let bundleAppPaths = [
+                "/private/var/containers/Bundle/Application",
+                "/var/containers/Bundle/Application"
+            ]
+            var bundleIDNameCache: [String:String] = [:]
+            if mode == .appName {
+                let bundlePath = "/private/var/containers/Bundle/Application"
+                let apps = try fm.contentsOfDirectory(atPath: bundlePath)
+                var results: [String:String] = [:]
+                for app in apps {
+                    let appPath = bundlePath + "/" + app
+                    let contents = try fm.contentsOfDirectory(atPath: appPath)
+                    for item in contents {
+                        if item.hasSuffix(".app") {
+                            if let plist = NSDictionary(contentsOf: URL(fileURLWithPath: appPath + "/" + item + "/Info.plist")),
+                                let bundleID = plist["CFBundleIdentifier"] as? String,
+                                let appName = 
+                                    (plist["CFBundleDisplayName"] as? String) ??
+                                    (plist["CFBundleName"] as? String) ??
+                                    (plist["CFBundleExecutable"] as? String) {
+                                results[bundleID] = appName
+                            }
+                            break
+                        }
+                    }
+                }
+                bundleIDNameCache = results
+            }
```

</details>

`loadDirectoryContentsSBX` builds `bundleIDNameCache` by enumerating `/private/var/containers/Bundle/Application` whenever `mode == .appName`, even when listing unrelated directories. This adds significant overhead and can cause *any* directory listing to fail if that path isn’t readable (the `try` will throw and the whole listing returns an error). Consider only building this cache when listing the app container directories (or make it best-effort with `try?` / guarded do-catch, and/or pick the first readable path from `bundleAppPaths`).

  **[@jurre111](https://github.com/jurre111)** · Apr 25, 2026 (lines 386-420) *(reply)* · review: COMMENTED

  Fixed


### `lara/views/SettingsView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 25, 2026 (line 141) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -121,13 +123,26 @@ struct SettingsView: View {
                         }
                     
                     Toggle("Show File Manager in Tabs", isOn: $showfmintabs)
-
+                    Toggle("Enable recursive search in File Manager", isOn: $fmRecursiveSearch)
                 } header: {
                     Text("Lara Settings")
                 } footer: {
                     Text("Keep Alive keeps the app running in the background when it is minimized (not closed from app switcher).")
                 }
 
+                Section {
+                    Picker("Display Mode", selection: $selectedFmAppsDisplayMode) {
+                        ForEach(fmAppsDisplayMode.allCases, id: \.self) { mode in
+                            Text(mode.rawValue).tag(mode)
+                        }
+                    }
+                    .pickerStyle(.menu)
+                } header: {
+                    Text("File Manger App Management")
```

</details>

Section header contains a typo: "File Manger App Management" should be "File Manager App Management".
```suggestion
                    Text("File Manager App Management")
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 25, 2026 (lines 629-632) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -610,3 +625,9 @@ enum method: String, CaseIterable {
     case sbx = "SBX"
     case hybrid = "Hybrid"
 }
+
+enum fmAppsDisplayMode: String, CaseIterable {
+    case UUID = "UUID"
+    case bundleID = "Bundle ID"
+    case appName = "App Name"
```

</details>

`fmAppsDisplayMode` uses user-facing strings as the enum `rawValue`, but `@AppStorage` persists the raw value to UserDefaults. Using UI labels as persisted values makes future renames/localization a breaking change for stored settings; prefer stable raw values (e.g., "uuid", "bundleID", "appName") and derive the display label separately.
```suggestion
enum fmAppsDisplayMode: CaseIterable, RawRepresentable {
    case UUID
    case bundleID
    case appName

    init?(rawValue: String) {
        switch rawValue {
        case "uuid", "UUID":
            self = .UUID
        case "bundleID", "Bundle ID":
            self = .bundleID
        case "appName", "App Name":
            self = .appName
        default:
            return nil
        }
    }

    var rawValue: String {
        switch self {
        case .UUID:
            return "uuid"
        case .bundleID:
            return "bundleID"
        case .appName:
            return "appName"
        }
    }

    var displayName: String {
        switch self {
        case .UUID:
            return "UUID"
        case .bundleID:
            return "Bundle ID"
        case .appName:
            return "App Name"
        }
    }
```

  **[@jurre111](https://github.com/jurre111)** · Apr 25, 2026 (lines 629-632) *(reply)* · review: COMMENTED

  unnecessary



---

## PR Commits (5)

- [`8bbd7ee`](https://github.com/rooootdev/lara/commit/8bbd7eeee28978586217b6b897869c2bd6bb2f8a) Filemanager: Make recursive search optional and make app folders display with Bundle ID or app name — *jurre111* · Apr 25, 2026
- [`de8a8d4`](https://github.com/rooootdev/lara/commit/de8a8d4388e1b3574ae060dcd9abadcdf2e73732) Fix unused variable — *Jurre* · Apr 25, 2026
- [`3d27ed2`](https://github.com/rooootdev/lara/commit/3d27ed2cb7e4bb2e254526dd42cb1213d8330483) Fix Typo — *Jurre* · Apr 25, 2026
- [`8caef1f`](https://github.com/rooootdev/lara/commit/8caef1f764f9d8ac894543d510678c01e5d5eae0) Fix condition in loadDirectoryContentsSBX() — *jurre111* · Apr 25, 2026
- [`4f5a9bc`](https://github.com/rooootdev/lara/commit/4f5a9bcdc44d46a352bbab248667e069eb0bb1e9) hmm — *jurre111* · Apr 25, 2026


---

## Changed Files (2)

- 🟡 `lara/views/SantanderView.swift` (modified) — +99 / -15
  <details><summary>Diff</summary>

  ```diff
  @@ -92,6 +92,7 @@ struct SantanderPath: Hashable {
     let lastPathComponent: String
     let isDirectory: Bool
     let contentType: UTType?
+    let displayName: String
 
     var displayImage: UIImage? {
         if isDirectory { return UIImage(systemName: "folder.fill") }
@@ -103,9 +104,10 @@ struct SantanderPath: Hashable {
         return UIImage(systemName: "doc")
     }
 
-    init(path: String, isDirectory: Bool) {
+    init(path: String, isDirectory: Bool, displayName: String? = nil) {
         self.path = path
         self.lastPathComponent = path == "/" ? "/" : (path as NSString).lastPathComponent
+        self.displayName = displayName ?? self.lastPathComponent
         self.isDirectory = isDirectory
         let ext = (path as NSString).pathExtension
         self.contentType = ext.isEmpty ? nil : UTType(filenameExtension: ext)
@@ -129,6 +131,7 @@ final class SantanderPathListViewController: UITableViewController, UISearchResu
     private var initialEmptyStateMessage: String?
     private var isSearching = false
     private var displayHiddenFiles = true
+    @AppStorage("fmRecursiveSearch") private var fmRecursiveSearch: Bool = false
 
     init(path: SantanderPath, readUsesSBX: Bool, useVFSOverwrite: Bool) {
         self.readUsesSBX = readUsesSBX
@@ -222,7 +225,6 @@ final class SantanderPathListViewController: UITableViewController, UISearchResu
             ) { [weak self] _ in
                 self?.confirmDelete(item)
             }
-            
             return UIMenu(children: [
                 copyAction,
                 infoAction,
@@ -255,17 +257,31 @@ final class SantanderPathListViewController: UITableViewController, UISearchResu
         if !query.isEmpty {
             isSearching = true
 
-            DispatchQueue.global(qos: .userInitiated).async {
-                let results = self.recursiveSearchSBX(
-                    at: self.currentPath.path,
-                    query: query
-                )
+            if fmRecursiveSearch {
+                DispatchQueue.global(qos: .userInitiated).async {
+                    let results = self.recursiveSearchSBX(
+                        at: self.currentPath.path,
+                        query: query
+                    )
 
-                DispatchQueue.main.async {
-                    self.renderedContents = results
-                    self.updateEmptyState(query: query)
-                    self.tableView.reloadData()
+                    DispatchQueue.main.async {
+                        self.renderedContents = results
+                        self.updateEmptyState(query: query)
+                        self.tableView.reloadData()
+                    }
+                }
+            } else {
+                var results: [SantanderPath] = []
+
+                for item in unfilteredContents {
+                    if item.displayName.localizedCaseInsensitiveContains(query) {
+                        results.append(item)
+                    }
                 }
+
+                renderedContents = results
+                updateEmptyState(query: query)
+                tableView.reloadData()
             }
 
             return
@@ -367,11 +383,79 @@ final class SantanderPathListViewController: UITableViewController, UISearchResu
         }
         do {
             let entries = try fm.contentsOfDirectory(atPath: path.path)
+            let modeRaw = UserDefaults.standard.string(forKey: "selectedFmAppsDisplayMode")
+            let mode = fmAppsDisplayMode(rawValue: modeRaw ?? "") ?? .appName
+
+            let dataAppPaths = [
+                "/private/var/mobile/Containers/Data/Application",
+                "/var/mobile/Containers/Data/Application"
+            ]
+            let bundleAppPaths = [
+                "/private/var/containers/Bundle/Application",
+                "/var/containers/Bundle/Application"
+            ]
+            var bundleIDNameCache: [String:String] = [:]
+            if mode == .appName && dataAppPaths.contains(path.path) {
+                let bundlePath = "/private/var/containers/Bundle/Application"
+                let apps = try fm.contentsOfDirectory(atPath: bundlePath)
+                var results: [String:String] = [:]
+                for app in apps {
+                    let appPath = bundlePath + "/" + app
+                    let contents = try fm.contentsOfDirectory(atPath: appPath)
+                    for item in contents {
+                        if item.hasSuffix(".app") {
+                            if let plist = NSDictionary(contentsOf: URL(fileURLWithPath: appPath + "/" + item + "/Info.plist")),
+                                let bundleID = plist["CFBundleIdentifier"] as? String,
+                                let appName = 
+                                    (plist["CFBundleDisplayName"] as? String) ??
+                                    (plist["CFBundleName"] as? String) ??
+                                    (plist["CFBundleExecutable"] as? String) {
+                                results[bundleID] = appName
+                            }
+                            break
+                        }
+                    }
+                }
+                bundleIDNameCache = results
+            }
             let items = entries.map { name in
                 let fullPath = path.path == "/" ? "/" + name : path.path + "/" + name
                 var isDir: ObjCBool = false
+                var displayName = name
                 fm.fileExists(atPath: fullPath, isDirectory: &isDir)
-                return SantanderPath(path: fullPath, isDirectory: isDir.boolValue)
+
+                if (bundleAppPaths + dataAppPaths).contains(path.path) {
+                    if mode == .appName {
+                        if bundleAppPaths.contains(path.path) {
+                            if let contents = try? fm.contentsOfDirectory(atPath: fullPath) {
+                                for item in contents {
+                                    if item.hasSuffix(".app") {
+                                        if let plist = NSDictionary(contentsOf: URL(fileURLWithPath: fullPath + "/" + item + "/Info.plist")),
+                                            let appName = 
+                                                (plist["CFBundleDisplayName"] as? String) ??
+                                                (plist["CFBundleName"] as? String) ??
+                                                (plist["CFBundleExecutable"] as? String) {
+                                            displayName = appName
+                                        }
+                                        break
+                                    }
+                                }
+                            }
+                        } else {
+                            if let plist = NSDictionary(contentsOf: URL(fileURLWithPath: fullPath + "/.com.apple.mobile_container_manager.metadata.plist")),
+                                let bundleID = plist["MCMMetadataIdentifier"] as? String {
+                                displayName = bundleIDNameCache[bundleID] ?? bundleID
+                            }
+                        }
+
+                    } else if mode == .bundleID {
+                        if let plist = NSDictionary(contentsOf: URL(fileURLWithPath: fullPath + "/.com.apple.mobile_container_manager.metadata.plist")), let bundleID = plist["MCMMetadataIdentifier"] as? String {
+                            displayName = bundleID
+                        }
+                    }
+                }
+
+                return SantanderPath(path: fullPath, isDirectory: isDir.boolValue, displayName: displayName)
             }
             if items.isEmpty {
                 return ([], "Directory is empty.")
@@ -433,12 +517,12 @@ final class SantanderPathListViewController: UITableViewController, UISearchResu
         }
         let sortAZ = UIAction(title: "Sort A-Z", image: UIImage(systemName: "textformat")) { [weak self] _ in
             guard let self else { return }
-            self.unfilteredContents.sort { $0.lastPathComponent.localizedCaseInsensitiveCompare($1.lastPathComponent) == .orderedAscending }
+            self.unfilteredContents.sort { $0.displayName.localizedCaseInsensitiveCompare($1.displayName) == .orderedAscending }
             self.applyFilters(query: self.navigationItem.searchController?.searchBar.text ?? "")
         }
         let sortZA = UIAction(title: "Sort Z-A", image: UIImage(systemName: "textformat")) { [weak self] _ in
             guard let self else { return }
-            self.unfilteredContents.sort { $0.lastPathComponent.localizedCaseInsensitiveCompare($1.lastPathComponent) == .orderedDescending }
+            self.unfilteredContents.sort { $0.displayName.localizedCaseInsensitiveCompare($1.displayName) == .orderedDescending }
             self.applyFilters(query: self.navigationItem.searchController?.searchBar.text ?? "")
         }
         let toggleHidden = UIAction(title: "Display hidden files", image: UIImage(systemName: "eye"), state: displayHiddenFiles ? .on : .off) { [weak self] _ in
@@ -474,7 +558,7 @@ final class SantanderPathListViewController: UITableViewController, UISearchResu
     }
 
     private func pathCellRow(forURL fsItem: SantanderPath, displayFullPathAsSubtitle useSubtitle: Bool = false) -> UITableViewCell {
-        let pathName = fsItem.lastPathComponent
+        let pathName = fsItem.displayName
         let cell = UITableViewCell(style: useSubtitle ? .subtitle : .default, reuseIdentifier: nil)
         var conf = cell.defaultContentConfiguration()
         conf.text = pathName
  ```

  </details>
- 🟡 `lara/views/SettingsView.swift` (modified) — +22 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -24,6 +24,8 @@ struct SettingsView: View {
     @AppStorage("selectedmethod") private var selectedmethod: method = .hybrid
     @AppStorage("rcdockunlimited") private var rcdockunlimited: Bool = false
     @AppStorage("stashkrw") private var stashkrw: Bool = false
+    @AppStorage("selectedFmAppsDisplayMode") private var selectedFmAppsDisplayMode: fmAppsDisplayMode = .appName
+    @AppStorage("fmRecursiveSearch") private var fmRecursiveSearch: Bool = false
     
     var appname: String {
         Bundle.main.object(forInfoDictionaryKey: "CFBundleDisplayName") as? String
@@ -121,13 +123,26 @@ struct SettingsView: View {
                         }
                     
                     Toggle("Show File Manager in Tabs", isOn: $showfmintabs)
-
+                    Toggle("Enable recursive search in File Manager", isOn: $fmRecursiveSearch)
                 } header: {
                     Text("Lara Settings")
                 } footer: {
                     Text("Keep Alive keeps the app running in the background when it is minimized (not closed from app switcher).")
                 }
 
+                Section {
+                    Picker("Display Mode", selection: $selectedFmAppsDisplayMode) {
+                        ForEach(fmAppsDisplayMode.allCases, id: \.self) { mode in
+                            Text(mode.rawValue).tag(mode)
+                        }
+                    }
+                    .pickerStyle(.menu)
+                } header: {
+                    Text("File Manager App Management")
+                } footer: {
+                    Text("Change the way app folders get displayed in the file manager.")
+                }
+
                 #if !DISABLE_REMOTECALL
                 Section {
                     Toggle("Stash KRW primitives", isOn: $stashkrw)
@@ -610,3 +625,9 @@ enum method: String, CaseIterable {
     case sbx = "SBX"
     case hybrid = "Hybrid"
 }
+
+enum fmAppsDisplayMode: String, CaseIterable {
+    case UUID = "UUID"
+    case bundleID = "Bundle ID"
+    case appName = "App Name"
+}
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

- : 📝 Commit 8bbd7ee: Filemanager: Make recursive search optional and make app folders display with Bundle ID or app name
- Apr 25, 2026: 👀 @jurre111 requested review from @Copilot
- : 💬 someone reviewed: commented
- : 📝 Commit de8a8d4: Fix unused variable
- : 📝 Commit 3d27ed2: Fix Typo
- : 📝 Commit 8caef1f: Fix condition in loadDirectoryContentsSBX()
- : 📝 Commit 4f5a9bc: hmm
- Apr 26, 2026: 🟣 @rooootdev merged
- Apr 26, 2026: 🔴 Closed by @rooootdev
- Apr 26, 2026: 🗑️ @jurre111 deleted head branch
- Apr 26, 2026: 🔗 Commit 930a0c7 — Squashed commit of the following:
- Apr 27, 2026: 🔗 Commit 07c3a6c — Squashed commit of the following:
- Apr 29, 2026: 🔗 Commit e0da526 — Squashed commit of the following:
- Apr 29, 2026: 🔗 Commit 9fc6ffb — Squashed commit of the following:


---

*Exported on 10/14/2026, 3:02:28 PM from [https://github.com/rooootdev/lara/pull/194](https://github.com/rooootdev/lara/pull/194)*
