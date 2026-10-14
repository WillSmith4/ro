# Add on-device Pocket Poster Helper (done now)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#203](https://github.com/rooootdev/lara/pull/203)  
> **State:** 🟣 merged  
> **Branch:** `jurre111:pocketposter` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 27, 2026  
> **Changes:** +122 / -1 across 2 files  
> **Created:** Apr 27, 2026  
> **Updated:** Apr 27, 2026  
> **Closed:** Apr 27, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on Apr 27, 2026*


Works like it should. Test ipa: [lara.ipa](https://github.com/jurre111/lara/releases/download/latest/lara.ipa)


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25017707718/job/73270019084) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25017707718/job/73269807075) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 27, 2026

## Pull request overview

Adds an on-device “Pocket Poster Helper” tool to generate and drop required container hashes into the Pocket Poster app’s Documents directory, so users don’t need a PC.

**Changes:**
- Adds a new “Pocket Poster Helper” action in Tools UI with a status alert.
- Implements `PPHelper()` in `laramgr` to scan container metadata and write out hash files for Pocket Poster detection.

### Reviewed changes

Copilot reviewed 2 out of 2 changed files in this pull request and generated 5 comments.

| File | Description |
| ---- | ----------- |
| `lara/views/ToolsView.swift` | Adds a Pocket Poster section with a helper button and status alert. |
| `lara/classes/laramgr.swift` | Implements the helper logic to locate app container hashes and write them into Pocket Poster’s Documents folder. |





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (5)

*Inline comments on code changes*

### `lara/views/ToolsView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 27, 2026 (lines 146-151) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -140,6 +141,23 @@ struct ToolsView: View {
             } footer: {
                 Text("Currently broken.")
             }
+
+            Section {
+                Button {
+                    if mgr.PPHelper() {
+                        status = "Succeeded. Open the Pocket Poster app, open settings and tap Detect."
+                    } else {
+                        status = "Failed. Check logs."
+                    }
```

</details>

`PPHelper()` performs synchronous filesystem scanning and plist parsing, and it’s invoked directly from a Button action (main thread). This can noticeably stall the UI on devices with many containers. Consider running the work off the main thread (e.g., via `Task {}` / `DispatchQueue.global`) and only updating `status` back on the main actor, and optionally disable the button while the task is running.


### `lara/classes/laramgr.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 27, 2026 (line 458) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -445,6 +445,47 @@ final class laramgr: ObservableObject {
             }
         }
     }
+
+    // inspired by nugget from leminlimez
+    func PPHelper() -> Bool {
```

</details>

Method names in this file are consistently lowerCamelCase (e.g., `sbxescape`, `vfsinit`, `apfsown`), but the newly added `PPHelper()` uses UpperCamelCase. Rename it to lowerCamelCase (e.g., `ppHelper` / `pocketPosterHelper`) to match Swift/API conventions and local project style.
```suggestion
    func ppHelper() -> Bool {
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 27, 2026 (line 471) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -445,6 +445,47 @@ final class laramgr: ObservableObject {
             }
         }
     }
+
+    // inspired by nugget from leminlimez
+    func PPHelper() -> Bool {
+        do {
+            let fm = FileManager.default
+            let dataFolder = "/private/var/mobile/Containers/Data/Application"
+            var bundleIDs = ["com.lemin.Pocket-Poster", "com.apple.PosterBoard"]
+            if UIDevice.current.userInterfaceIdiom == .phone {
+                bundleIDs.append("com.apple.CarPlayWallpaper")
+            }
+
+            let apps = try fm.contentsOfDirectory(atPath: dataFolder)
+            var hashes: [String:String] = [:]
+            for app in apps {
+                if let plist = NSDictionary(contentsOf: URL(fileURLWithPath: dataFolder + "/" + app + "/.com.apple.mobile_container_manager.metadata.plist")),
+                    let bundleID = plist["MCMMetadataIdentifier"] as? String {
+                    if bundleIDs.contains(bundleID) {
+                        hashes[bundleID] = app
+                    }
+                }
+            }
+
+            if let PPHash = hashes["com.lemin.Pocket-Poster"] {
+                hashes["com.lemin.Pocket-Poster"] = nil
```

</details>

`hashes["com.lemin.Pocket-Poster"] = nil` works, but using `removeValue(forKey:)` is clearer about intent (removing the entry) and avoids the “set to nil” idiom when reading quickly.
```suggestion
                hashes.removeValue(forKey: "com.lemin.Pocket-Poster")
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 27, 2026 (lines 488-490) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -445,6 +445,47 @@ final class laramgr: ObservableObject {
             }
         }
     }
+
+    // inspired by nugget from leminlimez
+    func PPHelper() -> Bool {
+        do {
+            let fm = FileManager.default
+            let dataFolder = "/private/var/mobile/Containers/Data/Application"
+            var bundleIDs = ["com.lemin.Pocket-Poster", "com.apple.PosterBoard"]
+            if UIDevice.current.userInterfaceIdiom == .phone {
+                bundleIDs.append("com.apple.CarPlayWallpaper")
+            }
+
+            let apps = try fm.contentsOfDirectory(atPath: dataFolder)
+            var hashes: [String:String] = [:]
+            for app in apps {
+                if let plist = NSDictionary(contentsOf: URL(fileURLWithPath: dataFolder + "/" + app + "/.com.apple.mobile_container_manager.metadata.plist")),
+                    let bundleID = plist["MCMMetadataIdentifier"] as? String {
+                    if bundleIDs.contains(bundleID) {
+                        hashes[bundleID] = app
+                    }
+                }
+            }
+
+            if let PPHash = hashes["com.lemin.Pocket-Poster"] {
+                hashes["com.lemin.Pocket-Poster"] = nil
+                for bundleID in hashes.keys {
+                    let fileName = "Nugget" + bundleID.replacingOccurrences(of: "com.apple.", with: "") + "Hash"
+                    let content = hashes[bundleID]!
```

</details>

Inside the loop, `for bundleID in hashes.keys` followed by `let content = hashes[bundleID]!` does an extra dictionary lookup per iteration and forces an unwrap. Iterating the dictionary as `(bundleID, content)` (or `for (bundleID, content) in hashes`) is simpler and avoids the `!`.
```suggestion
                for (bundleID, content) in hashes {
                    let fileName = "Nugget" + bundleID.replacingOccurrences(of: "com.apple.", with: "") + "Hash"
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 27, 2026 (lines 489-492) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -445,6 +445,47 @@ final class laramgr: ObservableObject {
             }
         }
     }
+
+    // inspired by nugget from leminlimez
+    func PPHelper() -> Bool {
+        do {
+            let fm = FileManager.default
+            let dataFolder = "/private/var/mobile/Containers/Data/Application"
+            var bundleIDs = ["com.lemin.Pocket-Poster", "com.apple.PosterBoard"]
+            if UIDevice.current.userInterfaceIdiom == .phone {
+                bundleIDs.append("com.apple.CarPlayWallpaper")
+            }
+
+            let apps = try fm.contentsOfDirectory(atPath: dataFolder)
+            var hashes: [String:String] = [:]
+            for app in apps {
+                if let plist = NSDictionary(contentsOf: URL(fileURLWithPath: dataFolder + "/" + app + "/.com.apple.mobile_container_manager.metadata.plist")),
+                    let bundleID = plist["MCMMetadataIdentifier"] as? String {
+                    if bundleIDs.contains(bundleID) {
+                        hashes[bundleID] = app
+                    }
+                }
+            }
+
+            if let PPHash = hashes["com.lemin.Pocket-Poster"] {
+                hashes["com.lemin.Pocket-Poster"] = nil
+                for bundleID in hashes.keys {
+                    let fileName = "Nugget" + bundleID.replacingOccurrences(of: "com.apple.", with: "") + "Hash"
+                    let content = hashes[bundleID]!
+                    let filePath = dataFolder + "/" + PPHash + "/Documents/" + fileName
+                    try content.write(to: URL(fileURLWithPath: filePath), atomically: true, encoding: .utf8)
```

</details>

File paths are built via string concatenation (`dataFolder + "/" + PPHash + "/Documents/" + fileName`), which is easy to get wrong and harder to maintain. Prefer building paths with `URL`/`appendingPathComponent` (and consider verifying/creating the Documents directory before writing) for safer path handling.



---

## PR Commits (16)

- [`4e9bd55`](https://github.com/rooootdev/lara/commit/4e9bd55f3847f3d1319f04650b5cd7f81d170f60) Pocket Poster Helper test run — *jurre111* · Apr 27, 2026
- [`b936764`](https://github.com/rooootdev/lara/commit/b9367644ad5b1cc3f4c3297d5d8a83c9a8a0d72e) Fixes — *jurre111* · Apr 27, 2026
- [`603d530`](https://github.com/rooootdev/lara/commit/603d530ac0e804a5f300df81d8a1fab893400756) Fixes again — *jurre111* · Apr 27, 2026
- [`ced049c`](https://github.com/rooootdev/lara/commit/ced049c9dd8fbaade0a3bf0012944316d9c9f9b9) done? — *jurre111* · Apr 27, 2026
- [`ce7529c`](https://github.com/rooootdev/lara/commit/ce7529cce15e846b9254fe315c354ff9ee51ae91) oops not yet — *jurre111* · Apr 27, 2026
- [`bda7a0f`](https://github.com/rooootdev/lara/commit/bda7a0ffec137791f8561951a09f7a2d7d293e12) typo :) — *jurre111* · Apr 27, 2026
- [`6566118`](https://github.com/rooootdev/lara/commit/656611859a0bf9f0500f42d3b6b4850f90c9c3de) im stupid — *jurre111* · Apr 27, 2026
- [`740fa4a`](https://github.com/rooootdev/lara/commit/740fa4aad8e2ff876dca0c6e68259b4f580d08b8) Fix status — *jurre111* · Apr 27, 2026
- [`9c0725b`](https://github.com/rooootdev/lara/commit/9c0725b2f650f2f1932914d5e2a23e9e1b1ed2c1) Try: Improvements and Live Container support — *jurre111* · Apr 27, 2026
- [`c1c0b10`](https://github.com/rooootdev/lara/commit/c1c0b10eeab2963bdada434755379ab1a282e89f) Fixes — *jurre111* · Apr 27, 2026
- [`5bebb4e`](https://github.com/rooootdev/lara/commit/5bebb4e6268c5a03d873beab5b110684773c4ecb) no need for progress and fixes — *jurre111* · Apr 27, 2026
- [`f8e5495`](https://github.com/rooootdev/lara/commit/f8e5495db17d4fa5474bf27759f47939beebdc5b) oops — *jurre111* · Apr 27, 2026
- [`806374b`](https://github.com/rooootdev/lara/commit/806374bb77de0e89ccfce6ef2c5923160bd49895) button fixed now — *jurre111* · Apr 27, 2026
- [`548bb42`](https://github.com/rooootdev/lara/commit/548bb4245abb4370bedfd2cc734ea860fca22256) remove async stuff cause idk — *jurre111* · Apr 27, 2026
- [`ed926e6`](https://github.com/rooootdev/lara/commit/ed926e6805bc3b477ccc249b8d1677c9299d882c) i forgor — *jurre111* · Apr 27, 2026
- [`53839a5`](https://github.com/rooootdev/lara/commit/53839a54e8b99dcc598bb0383eed2713b410ab62) hm — *jurre111* · Apr 27, 2026


---

## Changed Files (2)

- 🟡 `lara/classes/laramgr.swift` (modified) — +99 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -55,6 +55,14 @@ final class laramgr: ObservableObject {
     static let italicfontpath = "/System/Library/Fonts/Core/SFUIItalic.ttf"
     static let monofontpath = "/System/Library/Fonts/Core/SFUIMono.ttf"
     private init() {}
+
+    struct AppInfo {
+        let executable: String
+        let displayName: String
+        let bundleName: String
+        let dataFolder: String
+        let bundleFolder: String
+    }
     
     func run(completion: ((Bool) -> Void)? = nil) {
         guard !dsrunning else { return }
@@ -445,7 +453,97 @@ final class laramgr: ObservableObject {
             }
         }
     }
-    
+
+    // inspired by nugget from leminlimez
+    func PPHelper() -> Bool {
+        do {
+            let fm = FileManager.default
+            let dataFolder = "/private/var/mobile/Containers/Data/Application"
+            let bundleFolder = "/private/var/containers/Bundle/Application"
+            var bundleIDs = ["com.apple.PosterBoard"]
+            if UIDevice.current.userInterfaceIdiom == .phone {
+                bundleIDs.append("com.apple.CarPlayWallpaper")
+            }
+            guard let appList = getAppList() else { return false}
+            var hashes: [String:String] = [:]
+            for bundleID in bundleIDs {
+                if let appInfo = appList[bundleID] {
+                    hashes[bundleID] = appInfo.dataFolder
+                } else {
+                    // this shouldn't happen
+                    logmsg("Could not find app with bundle ID \(bundleID).")
+                    return false
+                }
+            }
+            var PPbundleID = "com.leemin.Pocket-Poster"
+            for (bundleID, info) in appList {
+                if info.executable == "Pocket Poster" {
+                    PPbundleID = bundleID
+                    break
+                } else if info.executable == "LiveContainer" {
+                    PPbundleID = bundleID
+                }
+            }
+            if let PPHash = appList[PPbundleID]?.dataFolder {
+                for bundleID in hashes.keys {
+                    let fileName = "Nugget" + bundleID.replacingOccurrences(of: "com.apple.", with: "") + "Hash"
+                    let content = hashes[bundleID]!
+                    let filePath = dataFolder + "/" + PPHash + "/Documents/" + fileName
+                    try content.write(to: URL(fileURLWithPath: filePath), atomically: true, encoding: .utf8)
+                    logmsg("Wrote hash \(content) to \(filePath)")
+                }
+                return true
+            } else {
+                logmsg("Please install Pocket Poster before using Pocket Poster Helper. If you do have Pocket Poster installed, make sure you did not modify the bundle ID. If you installed Pocket Poster inside of LiveContainer, make sure you also did not modify the bundle ID of LiveContainer.")
+                return false
+            }
+        } catch {
+            logmsg("Error with Pocket Poster Helper: \(error.localizedDescription)")
+            return false
+        }
+    }
+
+    func getAppList() -> [String:AppInfo]? {
+        let fm = FileManager.default
+        let dataFolder = "/private/var/mobile/Containers/Data/Application"
+        let bundleFolder = "/private/var/containers/Bundle/Application"
+        var appList: [String:AppInfo] = [:]
+        do {
+            let appData = try fm.contentsOfDirectory(atPath: dataFolder)
+            for app in appData {
+                if let plist = NSDictionary(contentsOf: URL(fileURLWithPath: dataFolder + "/" + app + "/.com.apple.mobile_container_manager.metadata.plist")),
+                    let bundleID = plist["MCMMetadataIdentifier"] as? String {
+                    appList[bundleID] = AppInfo(executable: "", displayName: "", bundleName: "", dataFolder: app, bundleFolder: "")
+                }
+            }
+
+            let appBundles = try fm.contentsOfDirectory(atPath: bundleFolder)
+            for app in appBundles {
+                let appPath = bundleFolder + "/" + app
+                let contents = try fm.contentsOfDirectory(atPath: appPath)
+                for item in contents {
+                    if item.hasSuffix(".app") {
+                        if let plist = NSDictionary(contentsOf: URL(fileURLWithPath: appPath + "/" + item + "/Info.plist")),
+                            let bundleID = plist["CFBundleIdentifier"] as? String {
+                            let executable = plist["CFBundleExecutable"] as? String ?? ""
+                            let displayName = plist["CFBundleDisplayName"] as? String ?? ""
+                            let bundleName = plist["CFBundleName"] as? String ?? ""
+                            let dataFolderID = appList[bundleID]?.dataFolder ?? ""
+                            let appInfo = AppInfo(executable: executable, displayName: displayName, bundleName: bundleName, dataFolder: dataFolderID, bundleFolder: app)
+                            appList[bundleID] = appInfo
+                        }
+                        break
+                    }
+                }
+
+            }
+        } catch {
+            logmsg("Error getting app list: \(error.localizedDescription)")
+            return nil
+        }
+        return appList
+    }
+
     @discardableResult
     func apfsown(path: String, uid: UInt32, gid: UInt32) -> Bool {
         if !isapfs(path) {
  ```

  </details>
- 🟡 `lara/views/ToolsView.swift` (modified) — +23 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -32,6 +32,7 @@ struct ToolsView: View {
     @State private var issuepath: String = "/"
     @State private var uid: uid_t = getuid()
     @State private var pid: pid_t = getpid()
+    @State private var status: String?
     
     var body: some View {
         List {
@@ -140,6 +141,23 @@ struct ToolsView: View {
             } footer: {
                 Text("Currently broken.")
             }
+
+            Section {
+                Button {
+                    if mgr.PPHelper() {
+                        status = "Succeeded. Open the Pocket Poster app, open settings and tap Detect."
+                    } else {
+                        status = "Failed. Check logs."
+                    }
+                } label: {
+                    Text("Pocket Poster Helper")
+                }
+                .disabled(!mgr.sbxready)
+            } header: {
+                Text("Pocket Poster")
+            } footer: {
+                Text("Get the needed hashes for Pocket Poster without the need of a PC.")
+            }
             
             Section {
                 HStack {
@@ -216,6 +234,11 @@ struct ToolsView: View {
             }
         }
         .navigationTitle("Tools")
+        .alert("Status", isPresented: .constant(status != nil)) {
+                Button("OK") { status = nil }
+            } message: {
+                Text(status ?? "")
+            }
         .onAppear {
             if mgr.dsready {
                 getaslrstate()
  ```

  </details>


---

## References

### Commits (1)

- [9fc6ffb](https://github.com/letienndat/lara/commit/9fc6ffb8e54049fe785acc3b6361633ea2578ce1) — Squashed commit of the following:


---

## Timeline (Verbose)

- : 📝 Commit 4e9bd55: Pocket Poster Helper test run
- : 📝 Commit b936764: Fixes
- : 📝 Commit 603d530: Fixes again
- : 📝 Commit ced049c: done?
- : 📝 Commit ce7529c: oops not yet
- : 📝 Commit bda7a0f: typo :)
- : 📝 Commit 6566118: im stupid
- : 📝 Commit 740fa4a: Fix status
- Apr 27, 2026: 👀 @jurre111 requested review from @Copilot
- : 💬 someone reviewed: commented
- Apr 27, 2026: ⚪ @jurre111 converted to draft
- : 📝 Commit 9c0725b: Try: Improvements and Live Container support
- : 📝 Commit c1c0b10: Fixes
- : 📝 Commit 5bebb4e: no need for progress and fixes
- Apr 27, 2026: 🟢 @rooootdev marked ready for review
- : 📝 Commit f8e5495: oops
- : 📝 Commit 806374b: button fixed now
- : 📝 Commit 548bb42: remove async stuff cause idk
- : 📝 Commit ed926e6: i forgor
- : 📝 Commit 53839a5: hm
- Apr 27, 2026: ✏️ @jurre111 renamed: "Add on-device Pocket Poster Helper" → "Add on-device Pocket Poster Helper (done now)"
- Apr 27, 2026: 🟣 @rooootdev merged
- Apr 27, 2026: 🔴 Closed by @rooootdev
- Apr 29, 2026: 🔗 Commit 9fc6ffb — Squashed commit of the following:


---

## Comments (2)

### Comment #1 — [@jurre111](https://github.com/jurre111) · Apr 27, 2026

`ID: 4326387265`


LiveContainer support later today


---

### Comment #2 — [@jurre111](https://github.com/jurre111) · Apr 27, 2026

`ID: 4330236930`


Finally done


---

*Exported on 10/14/2026, 3:03:22 PM from [https://github.com/rooootdev/lara/pull/203](https://github.com/rooootdev/lara/pull/203)*
