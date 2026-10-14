# Use MobileContainer metadata for file manager display names

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#244](https://github.com/rooootdev/lara/pull/244)  
> **State:** 🟣 merged  
> **Branch:** `hxhlb:fm-container-display-names` → `rooootdev:main`  
> **Merged by:** [@lunginspector](https://github.com/lunginspector) on May 15, 2026  
> **Changes:** +13 / -12 across 1 files  
> **Created:** May 14, 2026  
> **Updated:** May 23, 2026  
> **Closed:** May 15, 2026  

---

## Description

*Opened by [@hxhlb](https://github.com/hxhlb)* *on May 14, 2026*


## Summary

Improve file manager display names for MobileContainer-managed directories.

Previously, Santander only converted UUID directory names for a few hardcoded container paths such as app data and app bundle directories. Other MobileContainer-managed folders, such as App Groups under `/var/mobile/Containers/Shared/AppGroup`, were still shown as raw UUIDs.

This PR makes the file manager detect `.com.apple.mobile_container_manager.metadata.plist` for directory entries and use `MCMMetadataIdentifier` as the display name when available.

## Changes

- Use MobileContainer metadata to display identifiers for UUID container directories.
- Keep UUID display mode unchanged.
- Keep app bundle display name handling for `/var/containers/Bundle/Application`.
- In App Name mode, prefer the app name when it can be resolved, otherwise fall back to the metadata identifier.
- Use middle truncation for long file names and paths so both the beginning and end remain visible on small screens.

## Testing

Tested on an iPhone SE 3.

- App Group containers under `/var/mobile/Containers/Shared/AppGroup` now show identifiers instead of raw UUIDs.
- No noticeable listing slowdown was observed.
- Long names are truncated in the middle and remain easier to identify on a small screen.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25838431608/job/75918476816) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25838431608/job/75918384538) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`36bd59b`](https://github.com/rooootdev/lara/commit/36bd59be8409a01606739e1c8a3f03b4f3cab1ed) Use MobileContainer metadata for file manager display names — *Hades* · May 14, 2026


---

## Changed Files (1)

- 🟡 `lara/views/fm/SantanderView.swift` (modified) — +13 / -12
  <details><summary>Diff</summary>

  ```diff
  @@ -523,12 +523,14 @@ private struct santanderdirview: View {
                 Text(entry.display)
                     .foregroundColor(entry.name.hasPrefix(".") ? .gray : .primary)
                     .lineLimit(1)
+                    .truncationMode(.middle)
 
                 if !query.trimmingCharacters(in: .whitespacesAndNewlines).isEmpty {
                     Text(entry.path)
                         .font(.caption)
                         .foregroundColor(.secondary)
                         .lineLimit(1)
+                        .truncationMode(.middle)
                 }
             }
 
@@ -1005,17 +1007,13 @@ private enum santanderfs {
         do {
             let names = try fm.contentsOfDirectory(atPath: item.path)
             let mode = fmAppsDisplayMode(rawValue: UserDefaults.standard.string(forKey: "selectedFmAppsDisplayMode") ?? "") ?? .appName
-            let datadirs = [
-                "/private/var/mobile/Containers/Data/Application",
-                "/var/mobile/Containers/Data/Application"
-            ]
             let bundledirs = [
                 "/private/var/containers/Bundle/Application",
                 "/var/containers/Bundle/Application"
             ]
             var appnames: [String: String] = [:]
 
-            if mode == .appName && datadirs.contains(item.path) {
+            if mode == .appName {
                 appnames = appnamecache()
             }
 
@@ -1025,15 +1023,18 @@ private enum santanderfs {
                 fm.fileExists(atPath: full, isDirectory: &isdir)
                 var display = name
 
-                if (bundledirs + datadirs).contains(item.path) {
-                    if mode == .appName {
-                        if bundledirs.contains(item.path) {
-                            display = bundleappname(at: full) ?? name
-                        } else if let bundleid = bundleidforcontainer(at: full) {
+                if mode != .UUID, isdir.boolValue {
+                    if bundledirs.contains(item.path), mode == .appName {
+                        display = bundleappname(at: full) ?? name
+                    } else if let bundleid = bundleidforcontainer(at: full) {
+                        switch mode {
+                        case .appName:
                             display = appnames[bundleid] ?? bundleid
+                        case .bundleID:
+                            display = bundleid
+                        case .UUID:
+                            break
                         }
-                    } else if mode == .bundleID, let bundleid = bundleidforcontainer(at: full) {
-                        display = bundleid
                     }
                 }
 
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 36bd59b: Use MobileContainer metadata for file manager display names
- May 14, 2026: ⬆️ @hxhlb force-pushed
- May 15, 2026: 🟣 @lunginspector merged
- May 15, 2026: 🔴 Closed by @lunginspector
- May 23, 2026: 🗑️ @hxhlb deleted head branch


---

*Exported on 10/14/2026, 3:06:44 PM from [https://github.com/rooootdev/lara/pull/244](https://github.com/rooootdev/lara/pull/244)*
