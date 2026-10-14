# Fix passcode themer

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#256](https://github.com/rooootdev/lara/pull/256)  
> **State:** 🟣 merged  
> **Branch:** `neonmodder123:v2` → `rooootdev:main`  
> **Merged by:** [@lunginspector](https://github.com/lunginspector) on May 22, 2026  
> **Changes:** +1815 / -623 across 19 files  
> **Created:** May 20, 2026  
> **Updated:** May 22, 2026  
> **Closed:** May 22, 2026  

---

## Description

*Opened by [@neonmodder123](https://github.com/neonmodder123)* *on May 20, 2026*


add zip support (zipmgr.swift), fix passcode themer completely, fix darkboard partially, add a broken section in tweaksview, make a function for chown-ing in santanderview and add an explore view for passcode themer


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/26303007008/job/77432852074) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/26303007008/job/77432665735) — **success** — *GitHub Actions*


---

## PR Commits (7)

- [`f30d5c8`](https://github.com/rooootdev/lara/commit/f30d5c80513e35bb94f0fe8602ad74703bb3fe89) big changes — *neon* · May 20, 2026
- [`cda8088`](https://github.com/rooootdev/lara/commit/cda8088b0353318f88d3d12e864fff9cb3f27dcf) Update source.json — *neon* · May 22, 2026
- [`ef435d5`](https://github.com/rooootdev/lara/commit/ef435d55895286c18f97af41bd13512841a30400) Create source_nightly.json — *neon* · May 22, 2026
- [`c9cf9ba`](https://github.com/rooootdev/lara/commit/c9cf9bae4e52a26e0bffe0ad38d6c44d8a646549) Update source_nightly.json — *neon* · May 22, 2026
- [`e7e53c0`](https://github.com/rooootdev/lara/commit/e7e53c0ff06e5b9d1c4742f39821ec1fd5cff476) Add nightly repo in README.md — *neon* · May 22, 2026
- [`552a584`](https://github.com/rooootdev/lara/commit/552a584f4df41e03da77c5966c81e0f3fc57c629) Update IconThemeManager.swift — *neon* · May 22, 2026
- [`9dcd551`](https://github.com/rooootdev/lara/commit/9dcd551154e4702ff366950ddfc96dad7da3d3e4) add credits — *neon* · May 22, 2026


---

## Changed Files (19)

- 🟡 `README.md` (modified) — +12 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -53,12 +53,22 @@ Important Notes:
 - YMMV on M-series CPUs. If you are on an M-series device, try going to lara settings, selecting `Modify Offsets`, and setting `t1sz_boot` to `0x11`.
 - Issues involving lara not working on either unsupported or *technically* supported versions will be closed immediately.
 
+## Releases
 <p align="center">
+  <h3>Latest Stable</h3>
   <a href="https://celloserenity.github.io/altdirect/?url=https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/source.json" target="_blank">
     <img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/AltSource_Blue.png?raw=true" alt="Add AltSource" width="200">
   </a>
-  <a href="https://github.com/rooootdev/lara/releases/download/0.1/lara_v0.1.ipa" target="_blank">
-    <img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/Download_Blue.png?raw=true" alt="Download .ipa" width="200">
+  <a href="https://github.com/rooootdev/lara/releases/download/v0.1/lara_v0.1.ipa" target="_blank">
+    <img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/Download_White.png?raw=true" alt="Download .ipa" width="200">
+  </a>
+
+  <h3>Latest Nightly</h3>
+  <a href="https://celloserenity.github.io/altdirect/?url=https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/source_nightly.json" target="_blank">
+    <img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/AltSource_Blue.png?raw=true" alt="Add AltSource" width="200">
+  </a>
+  <a href="https://github.com/rooootdev/lara/releases/download/nightly/lara.ipa" target="_blank">
+    <img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/Download_White.png?raw=true" alt="Download .ipa" width="200">
   </a>
 </p>
 
  ```

  </details>
- 🟡 `lara.xcodeproj/project.pbxproj` (modified) — +6 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -27,6 +27,7 @@
 		15F024352FBB9510004BD090 /* podBackgroundViewDark.visualstyleset in Resources */ = {isa = PBXBuildFile; fileRef = 15F023FC2FBB9510004BD090 /* podBackgroundViewDark.visualstyleset */; };
 		9F4A1D2F3C4E567890123456 /* lara/lib/libxpf.dylib in Embed Frameworks */ = {isa = PBXBuildFile; fileRef = 9F4A1D2E3C4E567890123456 /* lara/lib/libxpf.dylib */; settings = {ATTRIBUTES = (CodeSignOnCopy, RemoveHeadersOnCopy, ); }; };
 		9F4A1D423C4E567890123456 /* lara/lib/libgrabkernel2.dylib in Embed Frameworks */ = {isa = PBXBuildFile; fileRef = 9F4A1D413C4E567890123456 /* lara/lib/libgrabkernel2.dylib */; settings = {ATTRIBUTES = (CodeSignOnCopy, RemoveHeadersOnCopy, ); }; };
+		CD22E7942FBB416D00824D68 /* libz.tbd in Frameworks */ = {isa = PBXBuildFile; fileRef = CD22E7932FBB416600824D68 /* libz.tbd */; };
 /* End PBXBuildFile section */
 
 /* Begin PBXCopyFilesBuildPhase section */
@@ -78,6 +79,8 @@
 		9F4A1D413C4E567890123456 /* lara/lib/libgrabkernel2.dylib */ = {isa = PBXFileReference; lastKnownFileType = "compiled.mach-o.dylib"; path = lara/lib/libgrabkernel2.dylib; sourceTree = "<group>"; };
 		CC1C8B392F71DF9C00206982 /* lara.app */ = {isa = PBXFileReference; explicitFileType = wrapper.application; includeInIndex = 0; path = lara.app; sourceTree = BUILT_PRODUCTS_DIR; };
 		CC6842DF2F8311C300B08EB0 /* libSystem.B.tbd */ = {isa = PBXFileReference; lastKnownFileType = "sourcecode.text-based-dylib-definition"; name = libSystem.B.tbd; path = usr/lib/libSystem.B.tbd; sourceTree = SDKROOT; };
+		CD22E7862FBA18DF00824D68 /* libarchive.tbd */ = {isa = PBXFileReference; lastKnownFileType = "sourcecode.text-based-dylib-definition"; name = libarchive.tbd; path = usr/lib/libarchive.tbd; sourceTree = SDKROOT; };
+		CD22E7932FBB416600824D68 /* libz.tbd */ = {isa = PBXFileReference; lastKnownFileType = "sourcecode.text-based-dylib-definition"; name = libz.tbd; path = usr/lib/libz.tbd; sourceTree = SDKROOT; };
 /* End PBXFileReference section */
 
 /* Begin PBXFileSystemSynchronizedBuildFileExceptionSet section */
@@ -106,6 +109,7 @@
 			isa = PBXFrameworksBuildPhase;
 			buildActionMask = 2147483647;
 			files = (
+				CD22E7942FBB416D00824D68 /* libz.tbd in Frameworks */,
 			);
 			runOnlyForDeploymentPostprocessing = 0;
 		};
@@ -241,6 +245,8 @@
 		CC6842DE2F8311C300B08EB0 /* Frameworks */ = {
 			isa = PBXGroup;
 			children = (
+				CD22E7932FBB416600824D68 /* libz.tbd */,
+				CD22E7862FBA18DF00824D68 /* libarchive.tbd */,
 				CC6842DF2F8311C300B08EB0 /* libSystem.B.tbd */,
 			);
 			name = Frameworks;
  ```

  </details>
- 🔴 `lara.xcodeproj/project.xcworkspace/xcshareddata/swiftpm/Package.resolved` (removed) — +0 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -1,5 +0,0 @@
-{
-  "pins" : [
-  ],
-  "version" : 3
-}
  ```

  </details>
- 🔄 `lara/classes/tweaks/darkboard/IconCacheClearer.m` (renamed) ← `lara/classes/IconCacheClearer.m` — +0 / -0
- 🔄 `lara/classes/tweaks/darkboard/IconThemeGalleryManager.swift` (renamed) ← `lara/classes/IconThemeGalleryManager.swift` — +0 / -0
- 🔄 `lara/classes/tweaks/darkboard/IconThemeManager.swift` (renamed) ← `lara/classes/IconThemeManager.swift` — +182 / -79
  <details><summary>Diff</summary>

  ```diff
  @@ -1,5 +1,4 @@
 import Combine
-import Compression
 import Foundation
 import SwiftUI
 import UIKit
@@ -8,6 +7,7 @@ private let iconThemeStorageRoot = URL(fileURLWithPath: "/var/mobile/.DO-NOT-DEL
 private let rawThemesDir = iconThemeStorageRoot.appendingPathComponent("RawThemes", isDirectory: true)
 private let processedThemesDir = iconThemeStorageRoot.appendingPathComponent("ProcessedThemes", isDirectory: true)
 private let originalIconsDir = iconThemeStorageRoot.appendingPathComponent("OriginalIconsBackup", isDirectory: true)
+private func clearIconCache() { LaraClearIconCache() }
 
 struct LaraIconTheme: Identifiable, Equatable, Hashable {
     let name: String
@@ -158,9 +158,20 @@ struct LaraThemedApp: Identifiable, Hashable {
             }
 
             guard let cachedIcon else { continue }
-            let result = laramgr.shared.lara_overwritefile(target: iconURL.path, data: cachedIcon)
-            if !result.ok {
-                throw NSError(domain: "IconThemer", code: 6, userInfo: [NSLocalizedDescriptionKey: "\(bundleIdentifier): \(result.message)"])
+            
+            let chown1 = SantanderChown.chown( path: iconURL.path, uid: 501, gid: 501)
+            if(!chown1) {
+                throw NSError(domain: "IconThemer", code: 6, userInfo: [NSLocalizedDescriptionKey: "\(bundleIdentifier): 1st chown failed"])
+            }
+            
+            let overwrite = laramgr.shared.lara_overwritefile(target: iconURL.path, data: cachedIcon)
+            if !overwrite.ok {
+                throw NSError(domain: "IconThemer", code: 6, userInfo: [NSLocalizedDescriptionKey: "\(bundleIdentifier): \(overwrite.message)"])
+            }
+            
+            let chown2 = SantanderChown.chown( path: iconURL.path, uid: 33, gid: 33)
+            if(!chown2) {
+                throw NSError(domain: "IconThemer", code: 6, userInfo: [NSLocalizedDescriptionKey: "\(bundleIdentifier): 2nd chown failed"])
             }
         }
     }
@@ -184,6 +195,7 @@ final class IconThemeManager: ObservableObject {
     @Published var fixupProgress: Double = 0
     @Published var fixupMessage = ""
     @Published var showFixupSheet = false
+    private let mgr = laramgr.shared
 
     private let fm = FileManager.default
     private let selectedThemesKey = "lara.iconThemes.selectedThemes"
@@ -224,6 +236,20 @@ final class IconThemeManager: ObservableObject {
         }
         return result
     }
+    
+    func icon_logmsg(_ message: String) {
+        DispatchQueue.main.async {
+            self.mgr.log += "(icon) " + message + "\n"
+            globallogger.log("(icon) " + message)
+        }
+    }
+
+    func unzip_logmsg(_ message: String) {
+        DispatchQueue.main.async {
+            self.mgr.log += "(zip) " + message + "\n"
+            globallogger.log("(zip) " + message)
+        }
+    }
 
     func theme(named name: String) -> LaraIconTheme? {
         themes.first(where: { $0.name == name })
@@ -374,10 +400,38 @@ final class IconThemeManager: ObservableObject {
                 try fm.createDirectory(at: tempDir, withIntermediateDirectories: true, attributes: nil)
                 defer { try? fm.removeItem(at: tempDir) }
 
+                let extractDir = tempDir.appendingPathComponent("Extracted", isDirectory: true)
+                try fm.createDirectory(at: extractDir, withIntermediateDirectories: true, attributes: nil)
+                
                 let archivePath = tempDir.appendingPathComponent("import.\(ext == "zip" ? "zip" : "theme")")
                 try Data(contentsOf: workingURL).write(to: archivePath)
-                try unzipFile(at: archivePath, to: tempDir)
-                sourceDirectory = try resolveThemeSourceDirectory(from: tempDir)
+                try unzipFile(at: archivePath, to: extractDir)
+                let resolvedSource = try resolveThemeSourceDirectory(from: extractDir)
+                let themeURL = rawThemesDir.appendingPathComponent(finalThemeName, isDirectory: true)
+                try? fm.removeItem(at: themeURL)
+                try fm.createDirectory(at: themeURL, withIntermediateDirectories: true, attributes: nil)
+
+                for icon in (try? fm.contentsOfDirectory(at: resolvedSource, includingPropertiesForKeys: nil)) ?? [] {
+                    guard icon.pathExtension.lowercased() == "png" else { continue }
+                    let appID = appIDFromIcon(url: icon)
+                    let destination = themeURL.appendingPathComponent(appID + ".png")
+                    try? fm.removeItem(at: destination)
+                    do {
+                        try fm.copyItem(at: icon, to: destination)
+                        icon_logmsg("copied icon: \(appID)")
+                    } catch {
+                        icon_logmsg("copy fail: \(appID) -> \(error.localizedDescription)")
+                    }
+                }
+
+                let importedIcons = ((try? fm.contentsOfDirectory(at: themeURL, includingPropertiesForKeys: nil)) ?? []).filter { $0.pathExtension.lowercased() == "png" }
+                if importedIcons.isEmpty {
+                    try? fm.removeItem(at: themeURL)
+                    throw NSError(domain: "IconThemer", code: 9, userInfo: [NSLocalizedDescriptionKey: "No icons were found in the imported theme. Expected `<bundle-id>.png` in a folder."])
+                }
+
+                refreshThemes()
+                return
             } else {
                 throw NSError(domain: "IconThemer", code: 8, userInfo: [NSLocalizedDescriptionKey: "Unsupported theme import type: \(workingURL.lastPathComponent)"])
             }
@@ -392,13 +446,18 @@ final class IconThemeManager: ObservableObject {
             let appID = appIDFromIcon(url: icon)
             let destination = themeURL.appendingPathComponent(appID + ".png")
             try? fm.removeItem(at: destination)
-            try fm.copyItem(at: icon, to: destination)
+            do {
+                try fm.copyItem(at: icon, to: destination)
+                icon_logmsg("copied icon: \(appID)")
+            } catch {
+                icon_logmsg("copy fail: \(appID) -> \(error.localizedDescription)")
+            }
         }
 
         let importedIcons = ((try? fm.contentsOfDirectory(at: themeURL, includingPropertiesForKeys: nil)) ?? []).filter { $0.pathExtension.lowercased() == "png" }
         if importedIcons.isEmpty {
             try? fm.removeItem(at: themeURL)
-            throw NSError(domain: "IconThemer", code: 9, userInfo: [NSLocalizedDescriptionKey: "No icons were found in the imported theme. Expected `IconBundles/<bundle-id>.png`."])
+            throw NSError(domain: "IconThemer", code: 9, userInfo: [NSLocalizedDescriptionKey: "No icons were found in the imported theme. Expected `<bundle-id>.png` in a folder."])
         }
 
         refreshThemes()
@@ -565,25 +624,72 @@ final class IconThemeManager: ObservableObject {
     }
 
     private func resolveThemeSourceDirectory(from url: URL) throws -> URL {
+        icon_logmsg("resolve theme directory: \(url.path)")
+
         if url.lastPathComponent == "IconBundles" {
+            icon_logmsg("using root iconbundles")
             return url
         }
 
         let iconBundlesURL = url.appendingPathComponent("IconBundles", isDirectory: true)
         if fm.fileExists(atPath: iconBundlesURL.path) {
+            icon_logmsg("found direct iconbundles: \(iconBundlesURL.path)")
             return iconBundlesURL
         }
 
-        if let nested = try findIconBundlesDirectory(in: url) {
-            return nested
+        if let subdir = try findIconBundlesDirectory(in: url) {
+            icon_logmsg("found iconbundles in a subdirectory: \(subdir.path)")
+            return subdir
         }
 
-        let pngs = ((try? fm.contentsOfDirectory(at: url, includingPropertiesForKeys: nil)) ?? []).filter { $0.pathExtension.lowercased() == "png" }
-        if !pngs.isEmpty {
-            return url
+        if let pngDir = findFirstDirectoryContainingPNGs(in: url) {
+            icon_logmsg("found png directory: \(pngDir.path)")
+            return pngDir
         }
 
-        throw NSError(domain: "IconThemer", code: 10, userInfo: [NSLocalizedDescriptionKey: "Could not find `IconBundles` in \(url.lastPathComponent)"])
+        icon_logmsg("failed to find theme directory")
+
+        throw NSError(domain: "IconThemer", code: 10, userInfo: [
+                NSLocalizedDescriptionKey:
+                    "Could not find icons in \(url.lastPathComponent). Expected PNGs named <bundle-id>.png."
+            ]
+        )
+    }
+
+    private func findFirstDirectoryContainingPNGs(in root: URL) -> URL? {
+        icon_logmsg("scanning root: \(root.path)")
+
+        let rootPNGs = (try? fm.contentsOfDirectory(at: root, includingPropertiesForKeys: nil))?.filter { $0.pathExtension.lowercased() == "png" } ?? []
+
+        if !rootPNGs.isEmpty {
+            icon_logmsg("found png-s in root: \(root.path)")
+            for png in rootPNGs {
+                icon_logmsg("png: \(png.lastPathComponent)")
+            }
+            return root
+        }
+
+        guard let enumerator = fm.enumerator(at: root, includingPropertiesForKeys: [.isDirectoryKey], options: [.skipsHiddenFiles]
+        ) else {
+            icon_logmsg("failed to create enumerator")
+            return nil
+        }
+
+        for case let url as URL in enumerator {
+            icon_logmsg("scanning directory \(url.path) ...")
+
+            guard (try? url.resourceValues(forKeys: [.isDirectoryKey]))?.isDirectory == true else { continue }
+
+            let pngs = (try? fm.contentsOfDirectory(at: url, includingPropertiesForKeys: nil))?.filter { $0.pathExtension.lowercased() == "png" } ?? []
+
+            if !pngs.isEmpty {
+                icon_logmsg("found png-s: \(url.path)")
+                for png in pngs { icon_logmsg("png: \(png.lastPathComponent)") }
+                return url
+            } else { icon_logmsg("no png-s: \(url.lastPathComponent)") }
+        }
+        icon_logmsg("no directories found containing .png-s")
+        return nil
     }
 
     private func sanitizedThemeName(_ name: String) -> String {
@@ -608,84 +714,81 @@ final class IconThemeManager: ObservableObject {
         return nil
     }
 
-    private func unzipFile(at source: URL, to destination: URL) throws {
-        let fileData = try Data(contentsOf: source)
-        var offset = 0
-
-        while offset < fileData.count - 4 {
-            let signature = fileData.subdata(in: offset..<offset + 4).withUnsafeBytes { $0.load(as: UInt32.self) }
-            guard signature == 0x04034b50 else { break }
-            guard let entry = readLocalFileEntry(data: fileData, offset: offset) else { break }
-
-            let nameRange = offset + 30..<(offset + 30 + entry.nameLength)
-            let entryName = String(data: fileData.subdata(in: nameRange), encoding: .utf8) ?? ""
-            let contentOffset = offset + 30 + entry.nameLength + entry.extraLength
-
-            if !entryName.hasSuffix("/") {
-                let entryDestination = destination.appendingPathComponent(entryName)
-                let parentDir = entryDestination.deletingLastPathComponent()
-                try fm.createDirectory(at: parentDir, withIntermediateDirectories: true, attributes: nil)
-
-                let fileRange = contentOffset..<(contentOffset + entry.compressedSize)
-                if entry.compressionMethod == 0 {
-                    let fileEntryData = fileData.subdata(in: fileRange)
-                    try fileEntryData.write(to: entryDestination)
-                } else if entry.compressionMethod == 8 {
-                    let compressedData = fileData.subdata(in: fileRange)
-                    guard let decompressed = decompress(deflate: compressedData, originalSize: entry.uncompressedSize) else {
-                        throw NSError(domain: "IconThemer", code: 11, userInfo: [NSLocalizedDescriptionKey: "Failed to decompress \(entryName)"])
-                    }
-                    try decompressed.write(to: entryDestination)
-                }
+    func unzipFile(at source: URL, to destination: URL) throws {
+        try FileManager.default.createDirectory(
+            at: destination,
+            withIntermediateDirectories: true
+        )
+
+        let archive = try ZipArchive(data: try Data(contentsOf: source))
+        unzip_logmsg("zip entries: \(archive.entries.count)")
+
+        for entry in archive.entries {
+            unzip_logmsg("entry: \(entry.path)")
+
+            let normalizedPath = entry.path.replacingOccurrences(of: "\\", with: "/")
+            let outputURL = destination.appendingPathComponent(normalizedPath)
+
+            unzip_logmsg("output: \(outputURL.path)")
+
+            guard !normalizedPath.contains("..") else {
+                unzip_logmsg("skip path traversal: \(normalizedPath)")
+                continue
             }
 
-            offset += 30 + entry.nameLength + entry.extraLength + entry.compressedSize
+            if entry.isDirectory {
+                do {
+                    try FileManager.default.createDirectory(at: outputURL, withIntermediateDirectories: true)
+                } catch {
+                    unzip_logmsg("mkdir fail: \(error.localizedDescription)")
+                }
+
+            } else {
+
+                do {
+                    try FileManager.default.createDirectory(at: outputURL.deletingLastPathComponent(), withIntermediateDirectories: true)
+                    let extracted = try archive.extract(entry)
+                    unzip_logmsg("extracted size: \(extracted.count)")
+                    
+                    try extracted.write(to: outputURL)
+                    unzip_logmsg("wrote to file")
+                } catch { unzip_logmsg("extract fail: \(entry.path) to \(error.localizedDescription)") }
+            }
         }
-    }
 
-    private struct ZipEntry {
-        let compressionMethod: UInt16
-        let compressedSize: Int
-        let uncompressedSize: Int
-        let nameLength: Int
-        let extraLength: Int
+        unzip_logmsg("contents:")
+
+        if let e = FileManager.default.enumerator(at: destination, includingPropertiesForKeys: nil) { for case let fileURL as URL in e { unzip_logmsg(fileURL.path) } }
     }
+    
+    private func findNextZipHeader(
+        in data: Data,
+        start: Int
+    ) -> Int? {
 
-    private func readLocalFileEntry(data: Data, offset: Int) -> ZipEntry? {
-        guard offset + 30 <= data.count else { return nil }
+        guard start < data.count - 4 else { return nil }
 
-        let compressionMethod = data.subdata(in: offset + 8..<offset + 10).withUnsafeBytes { $0.load(as: UInt16.self) }
-        let compressedSize = Int(data.subdata(in: offset + 18..<offset + 22).withUnsafeBytes { $0.load(as: UInt32.self) })
-        let uncompressedSize = Int(data.subdata(in: offset + 22..<offset + 26).withUnsafeBytes { $0.load(as: UInt32.self) })
-        let nameLength = Int(data.subdata(in: offset + 26..<offset + 28).withUnsafeBytes { $0.load(as: UInt16.self) })
-        let extraLength = Int(data.subdata(in: offset + 28..<offset + 30).withUnsafeBytes { $0.load(as: UInt16.self) })
+        for i in start..<(data.count - 4) {
 
-        return ZipEntry(compressionMethod: compressionMethod, compressedSize: compressedSize, uncompressedSize: uncompressedSize, nameLength: nameLength, extraLength: extraLength)
-    }
+            let sig = data.subdata(in: i..<i + 4)
+                .withUnsafeBytes {
+                    UInt32(littleEndian: $0.load(as: UInt32.self))
+                }
 
-    private func decompress(deflate data: Data, originalSize: Int) -> Data? {
-        guard originalSize > 0 else { return Data() }
+            if sig == 0x04034b50 {
+                return i
+            }
 
-        let destinationBuffer = UnsafeMutablePointer<UInt8>.allocate(capacity: originalSize)
-        defer { destinationBuffer.deallocate() }
+            if sig == 0x02014b50 {
+                return i
+            }
 
-        let result = data.withUnsafeBytes { sourceBuffer -> Int in
-            guard let baseAddress = sourceBuffer.baseAddress else { return 0 }
-            return compression_decode_buffer(
-                destinationBuffer,
-                originalSize,
-                baseAddress.assumingMemoryBound(to: UInt8.self),
-                data.count,
-                nil,
-                COMPRESSION_ZLIB
-            )
+            if sig == 0x06054b50 {
+                return i
+            }
         }
 
-        return result == originalSize ? Data(bytes: destinationBuffer, count: originalSize) : nil
-    }
-
-    private func clearIconCache() {
-        LaraClearIconCache()
+        return nil
     }
 }
 
  ```

  </details>
- 🟢 `lara/classes/tweaks/passcode/PasscodeGalleryManager.swift` (added) — +172 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,172 @@
+//
+//  PasscodeGalleryManager.swift
+//  lara
+//
+//  Created by neonmodder123 on 20/05/2026.
+//
+
+import Foundation
+import SwiftUI
+import Combine
+
+// just forked the cowabunga theme repo to update JSON structure
+let defaultPasscodeRepoURL = "https://raw.githubusercontent.com/neonmodder123/theme-repo/refs/heads/main/passcode-themes.json"
+private let passcodeRepoKey = "passcodeThemeRepos"
+
+struct PasscodeGalleryTheme: Identifiable, Decodable, Equatable {
+    let name: String
+    let description: String
+    let url: String
+    let preview: String
+    let contact: PasscodeThemeContact
+    let version: String
+    var id: String { name }
+}
+
+struct PasscodeThemeContact: Decodable, Equatable {
+    private let raw: [String: String]
+    init(from decoder: Decoder) throws {
+        let container = try decoder.singleValueContainer()
+        raw = try container.decode([String: String].self)
+    }
+    var displayName: String {
+        raw.map { "\($0.key): \($0.value)" }.first ?? ""
+    }
+}
+
+struct PasscodeRepoData: Identifiable {
+    var id: String { name }
+    let name: String
+    let author: String?
+    let icon: String?
+    let themes: [PasscodeGalleryTheme]
+    let baseURL: URL
+}
+
+struct PasscodeRepoState: Identifiable {
+    var id: String { url }
+    let url: String
+    var isLoading: Bool
+    var error: String?
+    var data: PasscodeRepoData?
+}
+
+@MainActor
+final class PasscodeGalleryManager: ObservableObject {
+    static let shared = PasscodeGalleryManager()
+
+    @Published var repos: [PasscodeRepoState] = []
+    @Published var downloading: Set<String> = []
+
+    private var repoURLs: [String] = loadPasscodeRepoURLs()
+
+    var allThemes: [PasscodeGalleryTheme] { repos.flatMap { $0.data?.themes ?? [] } }
+    var themes: [PasscodeGalleryTheme] { allThemes }
+    var isLoading: Bool { repos.contains { $0.isLoading } }
+    var loadError: String? { repos.first { $0.error != nil }?.error }
+
+    func previewURL(for theme: PasscodeGalleryTheme) -> URL? {
+        guard let repoData = repos.first(where: { $0.data?.themes.contains(where: { $0.id == theme.id }) == true })?.data else { return nil }
+        if theme.preview.hasPrefix("http") { return URL(string: theme.preview) }
+        return repoData.baseURL.appendingPathComponent(theme.preview)
+    }
+
+    func downloadURL(for theme: PasscodeGalleryTheme) -> URL? {
+        guard let repoData = repos.first(where: { $0.data?.themes.contains(where: { $0.id == theme.id }) == true })?.data else { return nil }
+        if theme.url.hasPrefix("http") { return URL(string: theme.url) }
+        return repoData.baseURL.appendingPathComponent(theme.url)
+    }
+
+    func isDownloading(_ theme: PasscodeGalleryTheme) -> Bool { downloading.contains(theme.id) }
+
+    func loadThemes(forceRefresh: Bool = false) async {
+        await refreshRepos(forceRefresh: forceRefresh)
+    }
+
+    func refreshRepos(forceRefresh: Bool = false) async {
+        repos = repoURLs.map { PasscodeRepoState(url: $0, isLoading: true, error: nil, data: nil) }
+
+        await withTaskGroup(of: (String, Result<PasscodeRepoData, Error>).self) { group in
+            for url in repoURLs {
+                group.addTask {
+                    do {
+                        return (url, .success(try await self.fetchRepo(url, forceRefresh: forceRefresh)))
+                    } catch {
+                        return (url, .failure(error))
+                    }
+                }
+            }
+            for await (url, result) in group {
+                guard let idx = repos.firstIndex(where: { $0.url == url }) else { continue }
+                repos[idx].isLoading = false
+                switch result {
+                case .success(let data): repos[idx].data = data
+                case .failure(let error): repos[idx].error = error.localizedDescription
+                }
+            }
+        }
+    }
+
+    func addRepo(_ urlString: String) async {
+        let trimmed = urlString.trimmingCharacters(in: .whitespacesAndNewlines)
+        guard !trimmed.isEmpty, URL(string: trimmed) != nil, !repoURLs.contains(trimmed) else { return }
+        repoURLs.append(trimmed)
+        savePasscodeRepoURLs(repoURLs)
+        await refreshRepos()
+    }
+
+    func removeRepo(_ url: String) {
+        guard url != defaultPasscodeRepoURL else { return }
+        repoURLs.removeAll { $0 == url }
+        savePasscodeRepoURLs(repoURLs)
+        Task { await refreshRepos() }
+    }
+
+    func downloadAndImport(_ theme: PasscodeGalleryTheme) async throws {
+        guard let fileURL = downloadURL(for: theme) else { throw URLError(.badURL) }
+        downloading.insert(theme.id)
+        defer { downloading.remove(theme.id) }
+        let (data, _) = try await URLSession.shared.data(from: fileURL)
+        let dest = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
+            .appendingPathComponent(theme.name + ".passthm")
+        try data.write(to: dest, options: .atomic)
+    }
+
+    private func fetchRepo(_ urlString: String, forceRefresh: Bool = false) async throws -> PasscodeRepoData {
+        guard let url = URL(string: urlString) else { throw URLError(.badURL) }
+        var req = URLRequest(url: url)
+        if forceRefresh { req.cachePolicy = .reloadIgnoringLocalCacheData }
+        let (data, _) = try await URLSession.shared.data(for: req)
+        let baseURL = url.deletingLastPathComponent()
+
+        if let themes = try? JSONDecoder().decode([PasscodeGalleryTheme].self, from: data) {
+            let name = urlString == defaultPasscodeRepoURL ? "Cowabunga" : (url.deletingPathExtension().lastPathComponent)
+            return PasscodeRepoData(name: name, author: nil, icon: nil, themes: themes, baseURL: baseURL)
+        }
+
+        struct RepoJSON: Decodable {
+            let repo_name: String
+            let repo_author: String?
+            let repo_icon: String?
+            let themes: [PasscodeGalleryTheme]
+        }
+        let parsed = try JSONDecoder().decode(RepoJSON.self, from: data)
+        return PasscodeRepoData(name: parsed.repo_name, author: parsed.repo_author, icon: parsed.repo_icon, themes: parsed.themes, baseURL: baseURL)
+    }
+}
+
+private func loadPasscodeRepoURLs() -> [String] {
+    if let data = UserDefaults.standard.data(forKey: passcodeRepoKey),
+       let urls = try? JSONDecoder().decode([String].self, from: data), !urls.isEmpty {
+        var result = urls
+        if !result.contains(defaultPasscodeRepoURL) { result.insert(defaultPasscodeRepoURL, at: 0) }
+        return result
+    }
+    return [defaultPasscodeRepoURL]
+}
+
+private func savePasscodeRepoURLs(_ urls: [String]) {
+    if let encoded = try? JSONEncoder().encode(urls) {
+        UserDefaults.standard.set(encoded, forKey: passcodeRepoKey)
+    }
+}
  ```

  </details>
- 🟢 `lara/classes/zipmgr.swift` (added) — +388 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,388 @@
+//
+//  zipmgr.swift
+//  lara
+//
+//  Created by neonmodder123 on 17/05/2026.
+//
+
+import Foundation
+import zlib
+
+private let crcTable: [UInt32] = [
+    0x00000000, 0x77073096, 0xee0e612c, 0x990951ba, 0x076dc419, 0x706af48f, 0xe963a535, 0x9e6495a3, 0x0edb8832,
+    0x79dcb8a4, 0xe0d5e91e, 0x97d2d988, 0x09b64c2b, 0x7eb17cbd, 0xe7b82d07, 0x90bf1d91, 0x1db71064, 0x6ab020f2,
+    0xf3b97148, 0x84be41de, 0x1adad47d, 0x6ddde4eb, 0xf4d4b551, 0x83d385c7, 0x136c9856, 0x646ba8c0, 0xfd62f97a,
+    0x8a65c9ec, 0x14015c4f, 0x63066cd9, 0xfa0f3d63, 0x8d080df5, 0x3b6e20c8, 0x4c69105e, 0xd56041e4, 0xa2677172,
+    0x3c03e4d1, 0x4b04d447, 0xd20d85fd, 0xa50ab56b, 0x35b5a8fa, 0x42b2986c, 0xdbbbc9d6, 0xacbcf940, 0x32d86ce3,
+    0x45df5c75, 0xdcd60dcf, 0xabd13d59, 0x26d930ac, 0x51de003a, 0xc8d75180, 0xbfd06116, 0x21b4f4b5, 0x56b3c423,
+    0xcfba9599, 0xb8bda50f, 0x2802b89e, 0x5f058808, 0xc60cd9b2, 0xb10be924, 0x2f6f7c87, 0x58684c11, 0xc1611dab,
+    0xb6662d3d, 0x76dc4190, 0x01db7106, 0x98d220bc, 0xefd5102a, 0x71b18589, 0x06b6b51f, 0x9fbfe4a5, 0xe8b8d433,
+    0x7807c9a2, 0x0f00f934, 0x9609a88e, 0xe10e9818, 0x7f6a0dbb, 0x086d3d2d, 0x91646c97, 0xe6635c01, 0x6b6b51f4,
+    0x1c6c6162, 0x856530d8, 0xf262004e, 0x6c0695ed, 0x1b01a57b, 0x8208f4c1, 0xf50fc457, 0x65b0d9c6, 0x12b7e950,
+    0x8bbeb8ea, 0xfcb9887c, 0x62dd1ddf, 0x15da2d49, 0x8cd37cf3, 0xfbd44c65, 0x4db26158, 0x3ab551ce, 0xa3bc0074,
+    0xd4bb30e2, 0x4adfa541, 0x3dd895d7, 0xa4d1c46d, 0xd3d6f4fb, 0x4369e96a, 0x346ed9fc, 0xad678846, 0xda60b8d0,
+    0x44042d73, 0x33031de5, 0xaa0a4c5f, 0xdd0d7cc9, 0x5005713c, 0x270241aa, 0xbe0b1010, 0xc90c2086, 0x5768b525,
+    0x206f85b3, 0xb966d409, 0xce61e49f, 0x5edef90e, 0x29d9c998, 0xb0d09822, 0xc7d7a8b4, 0x59b33d17, 0x2eb40d81,
+    0xb7bd5c3b, 0xc0ba6cad, 0xedb88320, 0x9abfb3b6, 0x03b6e20c, 0x74b1d29a, 0xead54739, 0x9dd277af, 0x04db2615,
+    0x73dc1683, 0xe3630b12, 0x94643b84, 0x0d6d6a3e, 0x7a6a5aa8, 0xe40ecf0b, 0x9309ff9d, 0x0a00ae27, 0x7d079eb1,
+    0xf00f9344, 0x8708a3d2, 0x1e01f268, 0x6906c2fe, 0xf762575d, 0x806567cb, 0x196c3671, 0x6e6b06e7, 0xfed41b76,
+    0x89d32be0, 0x10da7a5a, 0x67dd4acc, 0xf9b9df6f, 0x8ebeeff9, 0x17b7be43, 0x60b08ed5, 0xd6d6a3e8, 0xa1d1937e,
+    0x38d8c2c4, 0x4fdff252, 0xd1bb67f1, 0xa6bc5767, 0x3fb506dd, 0x48b2364b, 0xd80d2bda, 0xaf0a1b4c, 0x36034af6,
+    0x41047a60, 0xdf60efc3, 0xa867df55, 0x316e8eef, 0x4669be79, 0xcb61b38c, 0xbc66831a, 0x256fd2a0, 0x5268e236,
+    0xcc0c7795, 0xbb0b4703, 0x220216b9, 0x5505262f, 0xc5ba3bbe, 0xb2bd0b28, 0x2bb45a92, 0x5cb36a04, 0xc2d7ffa7,
+    0xb5d0cf31, 0x2cd99e8b, 0x5bdeae1d, 0x9b64c2b0, 0xec63f226, 0x756aa39c, 0x026d930a, 0x9c0906a9, 0xeb0e363f,
+    0x72076785, 0x05005713, 0x95bf4a82, 0xe2b87a14, 0x7bb12bae, 0x0cb61b38, 0x92d28e9b, 0xe5d5be0d, 0x7cdcefb7,
+    0x0bdbdf21, 0x86d3d2d4, 0xf1d4e242, 0x68ddb3f8, 0x1fda836e, 0x81be16cd, 0xf6b9265b, 0x6fb077e1, 0x18b74777,
+    0x88085ae6, 0xff0f6a70, 0x66063bca, 0x11010b5c, 0x8f659eff, 0xf862ae69, 0x616bffd3, 0x166ccf45, 0xa00ae278,
+    0xd70dd2ee, 0x4e048354, 0x3903b3c2, 0xa7672661, 0xd06016f7, 0x4969474d, 0x3e6e77db, 0xaed16a4a, 0xd9d65adc,
+    0x40df0b66, 0x37d83bf0, 0xa9bcae53, 0xdebb9ec5, 0x47b2cf7f, 0x30b5ffe9, 0xbdbdf21c, 0xcabac28a, 0x53b39330,
+    0x24b4a3a6, 0xbad03605, 0xcdd70693, 0x54de5729, 0x23d967bf, 0xb3667a2e, 0xc4614ab8, 0x5d681b02, 0x2a6f2b94,
+    0xb40bbe37, 0xc30c8ea1, 0x5a05df1b, 0x2d02ef8d]
+
+extension Data {
+    var zipCRC32: UInt32 {
+        let mask: UInt32 = 0xffffffff
+        var result = mask
+        crcTable.withUnsafeBufferPointer { table in
+            self.withUnsafeBytes { buf in
+                for i in 0..<self.count {
+                    let byte = buf[i]
+                    let idx = Int((result ^ UInt32(byte)) & 0xff)
+                    result = (result >> 8) ^ table[idx]
+                }
+            }
+        }
+        return result ^ mask
+    }
+}
+
+extension Data {
+    func scan<T>(at offset: Int) -> T {
+        self.withUnsafeBytes { $0.loadUnaligned(fromByteOffset: offset, as: T.self) }
+    }
+}
+
+private let cp437Table: [UInt8: String] = [
+    0x80: "Ç", 0x81: "ü", 0x82: "é", 0x83: "â", 0x84: "ä", 0x85: "à", 0x86: "å", 0x87: "ç",
+    0x88: "ê", 0x89: "ë", 0x8a: "è", 0x8b: "ï", 0x8c: "î", 0x8d: "ì", 0x8e: "Ä", 0x8f: "Å",
+    0x90: "É", 0x91: "æ", 0x92: "Æ", 0x93: "ô", 0x94: "ö", 0x95: "ò", 0x96: "û", 0x97: "ù",
+    0x98: "ÿ", 0x99: "Ö", 0x9a: "Ü", 0x9b: "ø", 0x9c: "£", 0x9d: "Ø", 0x9e: "₧", 0x9f: "ƒ",
+    0xa0: "á", 0xa1: "í", 0xa2: "ó", 0xa3: "ú", 0xa4: "ñ", 0xa5: "Ñ", 0xa6: "ª", 0xa7: "º",
+    0xa8: "¿", 0xa9: "⌐", 0xaa: "¬", 0xab: "½", 0xac: "¼", 0xad: "¡", 0xae: "«", 0xaf: "»",
+    0xb0: "░", 0xb1: "▒", 0xb2: "▓", 0xb3: "│", 0xb4: "┤", 0xb5: "╡", 0xb6: "╢", 0xb7: "╖",
+    0xb8: "╕", 0xb9: "╣", 0xba: "║", 0xbb: "╗", 0xbc: "╝", 0xbd: "╜", 0xbe: "╛", 0xbf: "┐",
+    0xc0: "└", 0xc1: "┴", 0xc2: "┬", 0xc3: "├", 0xc4: "─", 0xc5: "┼", 0xc6: "╞", 0xc7: "╟",
+    0xc8: "╚", 0xc9: "╔", 0xca: "╩", 0xcb: "╦", 0xcc: "╠", 0xcd: "═", 0xce: "╬", 0xcf: "╧",
+    0xd0: "╨", 0xd1: "╤", 0xd2: "╥", 0xd3: "╙", 0xd4: "╘", 0xd5: "╒", 0xd6: "╓", 0xd7: "╫",
+    0xd8: "╪", 0xd9: "┘", 0xda: "┌", 0xdb: "█", 0xdc: "▄", 0xdd: "▌", 0xde: "▐", 0xdf: "▀",
+    0xe0: "α", 0xe1: "ß", 0xe2: "Γ", 0xe3: "π", 0xe4: "Σ", 0xe5: "σ", 0xe6: "µ", 0xe7: "τ",
+    0xe8: "Φ", 0xe9: "Θ", 0xea: "Ω", 0xeb: "δ", 0xec: "∞", 0xed: "φ", 0xee: "ε", 0xef: "∩",
+    0xf0: "≡", 0xf1: "±", 0xf2: "≥", 0xf3: "≤", 0xf4: "⌠", 0xf5: "⌡", 0xf6: "÷", 0xf7: "≈",
+    0xf8: "°", 0xf9: "∙", 0xfa: "·", 0xfb: "√", 0xfc: "ⁿ", 0xfd: "²", 0xfe: "■", 0xff: " "
+]
+
+extension String {
+    init(cp437 data: Data) {
+        var result = ""
+        result.reserveCapacity(data.count)
+        for byte in data {
+            if byte < 0x80 {
+                result.append(Character(UnicodeScalar(byte)))
+            } else {
+                result.append(cp437Table[byte] ?? "?")
+            }
+        }
+        self = result
+    }
+}
+
+private let eocdSignature: UInt32 = 0x06054b50
+private let cdSignature: UInt32 = 0x02014b50
+private let lfhSignature: UInt32 = 0x04034b50
+private let zip64EOCDLocatorSignature: UInt32 = 0x07064b50
+private let zip64EOCDRecordSignature: UInt32 = 0x06064b50
+
+public struct ZipEntry {
+    public let path: String
+    public let compressionMethod: UInt16
+    public let compressedSize: UInt64
+    public let uncompressedSize: UInt64
+    public let crc32: UInt32
+    public let dataOffset: UInt64
+    public let isDirectory: Bool
+}
+
+public enum ZipError: Error {
+    case notFound(String)
+    case corruptArchive(String)
+    case unsupportedCompression
+    case crcMismatch
+}
+
+public class ZipArchive {
+    private let data: Data
+    public private(set) var entries: [ZipEntry] = []
+    private var entryMap: [String: ZipEntry]
+    private let mgr = laramgr.shared
+    private var error = ""
+
+    public init(data: Data) throws {
+        self.data = data
+        self.entryMap = [:]
+        try scanEntries()
+        var map: [String: ZipEntry] = [:]
+        for entry in entries {
+            map[entry.path] = entry
+        }
+        self.entryMap = map
+    }
+
+    public subscript(path: String) -> ZipEntry? { entryMap[path] }
+
+    public func extract(_ entry: ZipEntry) throws -> Data {
+        let end = entry.dataOffset + entry.compressedSize
+        guard entry.dataOffset < UInt64(data.count),
+              end <= data.count else {
+            error = "(zip) entry data out of bounds"
+            mgr.logmsg("\(error)")
+            throw ZipError.corruptArchive("\(error)")
+        }
+
+        switch entry.compressionMethod {
+        case 0:
+            let raw = data.subdata(in: Int(entry.dataOffset)..<Int(end))
+            guard raw.zipCRC32 == entry.crc32 else {
+                error = "(zip) crc mismatch"
+                mgr.logmsg("\(error)")
+                throw ZipError.crcMismatch
+            }
+            return raw
+        case 8:
+            let raw = data.subdata(in: Int(entry.dataOffset)..<Int(end))
+            let decompressed = try decompressDeflate(raw, decompressedSize: Int(entry.uncompressedSize))
+            guard decompressed.zipCRC32 == entry.crc32 else {
+                error = "(zip) crc mismatch"
+                mgr.logmsg("\(error)")
+                throw ZipError.crcMismatch
+            }
+            return decompressed
+        default:
+            error = "(zip) unsupported compression, not a valid .zip"
+            mgr.logmsg("\(error)")
+            throw ZipError.unsupportedCompression
+        }
+    }
+
+    private func scanEntries() throws {
+        guard data.count >= 22 else {
+            error = "(zip) too small"
+            mgr.logmsg("\(error)")
+            throw ZipError.corruptArchive("\(error)")
+        }
+
+        let (eocdOffset, _) = try locateEOCD()
+        let cdOffset: UInt64
+        let cdSize: UInt64
+        let totalEntries: UInt64
+
+        let cdOffset32: UInt32 = data.scan(at: eocdOffset + 16)
+        let cdSize32: UInt32 = data.scan(at: eocdOffset + 12)
+        let totalEntries16: UInt16 = data.scan(at: eocdOffset + 10)
+
+        if cdOffset32 == UInt32.max || cdSize32 == UInt32.max || totalEntries16 == UInt16.max {
+            let (z64off, z64rec) = try locateZIP64EOCD(eocdOffset: eocdOffset)
+            cdOffset = z64rec.isEmpty ? UInt64(cdOffset32) : z64rec.scan(at: 48) as UInt64
+            cdSize = z64rec.isEmpty ? UInt64(cdSize32) : z64rec.scan(at: 40) as UInt64
+            totalEntries = z64rec.isEmpty ? UInt64(totalEntries16) : z64rec.scan(at: 32) as UInt64
+        } else {
+            cdOffset = UInt64(cdOffset32)
+            cdSize = UInt64(cdSize32)
+            totalEntries = UInt64(totalEntries16)
+        }
+
+        guard cdOffset + cdSize <= UInt64(data.count) else {
+            error = "(zip) cd out of bounds"
+            mgr.logmsg("\(error)")
+            throw ZipError.corruptArchive("\(error)")
+        }
+
+        var pos = Int(cdOffset)
+        for _ in 0..<totalEntries {
+            guard pos + 46 <= data.count else {
+                error = "(zip) cd entry truncated"
+                mgr.logmsg("\(error)")
+                throw ZipError.corruptArchive("\(error)")
+            }
+            let sig: UInt32 = data.scan(at: pos)
+            guard sig == cdSignature else {
+                error = "(zip) bad cd sig"
+                mgr.logmsg("\(error)")
+                throw ZipError.corruptArchive("\(error)")
+            }
+
+            let compMethod: UInt16 = data.scan(at: pos + 10)
+            let crc: UInt32 = data.scan(at: pos + 16)
+            let csize32: UInt32 = data.scan(at: pos + 20)
+            let usize32: UInt32 = data.scan(at: pos + 24)
+            let nameLen: UInt16 = data.scan(at: pos + 28)
+            let extraLen: UInt16 = data.scan(at: pos + 30)
+            let commentLen: UInt16 = data.scan(at: pos + 32)
+            let lfhOffset32: UInt32 = data.scan(at: pos + 42)
+
+            let nameData = data.subdata(in: pos + 46..<pos + 46 + Int(nameLen))
+            let extraData = data.subdata(in: pos + 46 + Int(nameLen)..<pos + 46 + Int(nameLen) + Int(extraLen))
+            let flags: UInt16 = data.scan(at: pos + 8)
+
+            let usesUTF8 = (flags & (1 << 11)) != 0
+            let path: String = usesUTF8 ? (String(data: nameData, encoding: .utf8) ?? String(cp437: nameData)) : String(cp437: nameData)
+
+            let csize: UInt64
+            let usize: UInt64
+            let lfhOff: UInt64
+
+            if csize32 == UInt32.max || usize32 == UInt32.max || lfhOffset32 == UInt32.max {
+                let fields = parseZIP64Extra(extraData)
+                csize = fields.compressedSize ?? UInt64(csize32)
+                usize = fields.uncompressedSize ?? UInt64(usize32)
+                lfhOff = fields.relativeOffset ?? UInt64(lfhOffset32)
+            } else {
+                csize = UInt64(csize32)
+                usize = UInt64(usize32)
+                lfhOff = UInt64(lfhOffset32)
+            }
+
+            let isDir = path.hasSuffix("/") || ((data.scan(at: pos + 38) as UInt32 >> 4) & 1) != 0
+            let dataOff = try computeDataOffset(lfhOffset: lfhOff)
+
+            entries.append(ZipEntry(
+                path: path,
+                compressionMethod: compMethod,
+                compressedSize: csize,
+                uncompressedSize: usize,
+                crc32: crc,
+                dataOffset: dataOff,
+                isDirectory: isDir
+            ))
+
+            pos += 46 + Int(nameLen) + Int(extraLen) + Int(commentLen)
+        }
+    }
+
+    private func computeDataOffset(lfhOffset: UInt64) throws -> UInt64 {
+        let off = Int(lfhOffset)
+        guard off + 30 <= data.count else {
+            error = "(zip) lfh truncated"
+            mgr.logmsg("\(error)")
+            throw ZipError.corruptArchive("\(error)")
+        }
+        let sig: UInt32 = data.scan(at: off)
+        guard sig == lfhSignature else {
+            error = "(zip) bad lfh sig"
+            mgr.logmsg("\(error)")
+            throw ZipError.corruptArchive("\(error)")
+        }
+        let nameLen: UInt16 = data.scan(at: off + 26)
+        let extraLen: UInt16 = data.scan(at: off + 28)
+        return lfhOffset + 30 + UInt64(nameLen) + UInt64(extraLen)
+    }
+
+    private func decompressDeflate(_ compressed: Data, decompressedSize: Int) throws -> Data {
+        var result = Data(count: decompressedSize)
+        var actualSize: Int = 0
+
+        let status: Int32 = result.withUnsafeMutableBytes { destBuf in
+            compressed.withUnsafeBytes { srcBuf in
+                guard let dest = destBuf.baseAddress,
+                      let src  = srcBuf.baseAddress else { return Z_BUF_ERROR }
+
+                var stream = z_stream()
+                stream.next_in   = UnsafeMutablePointer<Bytef>(mutating: src.assumingMemoryBound(to: Bytef.self))
+                stream.avail_in  = uInt(compressed.count)
+                stream.next_out  = dest.assumingMemoryBound(to: Bytef.self)
+                stream.avail_out = uInt(decompressedSize)
+
+                var ret = inflateInit2_(&stream, -15, ZLIB_VERSION, Int32(MemoryLayout<z_stream>.size))
+                guard ret == Z_OK else { return ret }
+                ret = inflate(&stream, Z_FINISH)
+                actualSize = decompressedSize - Int(stream.avail_out)
+                inflateEnd(&stream)
+                return ret
+            }
+        }
+
+        guard status == Z_STREAM_END else {
+            error = "(zip) raw deflate failed with zlib status \(status)"
+            mgr.logmsg("\(error)")
+            throw ZipError.corruptArchive("\(error)")
+        }
+        return result.prefix(actualSize)
+    }
+
+    private func locateEOCD() throws -> (offset: Int, commentLen: Int) {
+        let searchStart = max(0, data.count - 65557)
+        for i in (searchStart..<data.count - 3).reversed() {
+            let sig: UInt32 = data.scan(at: i)
+            if sig == eocdSignature {
+                let commentLen: UInt16 = data.scan(at: i + 20)
+                return (i, Int(commentLen))
+            }
+        }
+        error = "(zip) no eocd"
+        mgr.logmsg("\(error)")
+        throw ZipError.corruptArchive("\(error)")
+    }
+
+    private func locateZIP64EOCD(eocdOffset: Int) throws -> (offset: Int, recordData: Data) {
+        let locatorOff = eocdOffset - 20
+        guard locatorOff >= 0 else { return (0, Data()) }
+        let sig: UInt32 = data.scan(at: locatorOff)
+        guard sig == zip64EOCDLocatorSignature else { return (0, Data()) }
+        let z64Off: UInt64 = data.scan(at: locatorOff + 8)
+
+        guard z64Off < UInt64(data.count) - 56 else { return (0, Data()) }
+        let recSig: UInt32 = data.scan(at: Int(z64Off))
+        guard recSig == zip64EOCDRecordSignature else { return (0, Data()) }
+        let recSize: UInt64 = data.scan(at: Int(z64Off) + 4)
+        let totalSize = Int(recSize) + 12
+        guard Int(z64Off) + totalSize <= data.count else { return (0, Data()) }
+        let recData = data.subdata(in: Int(z64Off)..<Int(z64Off) + totalSize)
+        return (Int(z64Off), recData)
+    }
+
+    private struct ZIP64Fields {
+        var uncompressedSize: UInt64?
+        var compressedSize: UInt64?
+        var relativeOffset: UInt64?
+    }
+
+    private func parseZIP64Extra(_ extra: Data) -> ZIP64Fields {
+        var offset = 0
+        while offset + 4 <= extra.count {
+            let id: UInt16 = extra.scan(at: offset)
+            let size: UInt16 = extra.scan(at: offset + 2)
+            let fieldEnd = offset + 4 + Int(size)
+            guard fieldEnd <= extra.count else { break }
+            if id == 0x0001 {
+                var fields = ZIP64Fields()
+                var readOff = offset + 4
+                if readOff + 8 <= fieldEnd {
+                    fields.uncompressedSize = extra.scan(at: readOff)
+                    readOff += 8
+                }
+                if readOff + 8 <= fieldEnd {
+                    fields.compressedSize = extra.scan(at: readOff)
+                    readOff += 8
+                }
+                if readOff + 8 <= fieldEnd {
+                    fields.relativeOffset = extra.scan(at: readOff)
+                    readOff += 8
+                }
+                return fields
+            }
+            offset = fieldEnd
+        }
+        return ZIP64Fields()
+    }
+
+    private static let lfhSize = 30
+}
  ```

  </details>
- 🟡 `lara/lara-Bridging-Header.h` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -17,6 +17,8 @@
 #import "rc.h"
 #import "RemoteCall.h"
 
+#import <zlib.h>
+
 long findcachedataoff(const char *mgkey);
 void LaraClearIconCache(void);
 
  ```

  </details>
- 🟡 `lara/views/app/settings/CreditsView.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -32,7 +32,7 @@ struct CreditsView: View {
                 LinkCreditCell(name: "Jurre", description: "EditorView, PocketPoster Helper, various improvements", url: "https://github.com/jurre111") {
                     LinkCreditIcon(url: "https://github.com/jurre111.png")
                 }
-                LinkCreditCell(name: "neon", description: "Respring Script", url: "https://github.com/neonmodder123") {
+                LinkCreditCell(name: "neon", description: "Respring Script and zipmgr", url: "https://github.com/neonmodder123") {
                     LinkCreditIcon(url: "https://github.com/neonmodder123.png")
                 }
                 LinkCreditCell(name: "Skadz", description: "Respring Method", url: "https://github.com/skadz108") {
  ```

  </details>
- 🟡 `lara/views/fm/SantanderView.swift` (modified) — +9 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -10,6 +10,15 @@ import UniformTypeIdentifiers
 import AVKit
 import UIKit
 import Combine
+import Foundation
+
+enum SantanderChown {
+    static func chown(path: String, uid: UInt32, gid: UInt32) -> Bool {
+        path.withCString {
+            apfs_own($0, uid, gid) == 0
+        }
+    }
+}
 
 struct SantanderView: View {
     let startpath: String
  ```

  </details>
- 🟡 `lara/views/tweaks/TweaksView.swift` (modified) — +5 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -17,8 +17,6 @@ struct TweaksView: View {
                 Section(header: HeaderLabel(text: "SpringBoard", icon: "house")) {
                     NavigationLink("RemoteCall Customizer", destination: RemoteView(mgr: mgr))
                         .disabled(!mgr.rcready)
-                    NavigationLink("DarkBoard", destination: DarkBoardView())
-                        .disabled(true)
                     NavigationLink("Liquid Glass", destination: LiquidGlassView())
                         .disabled(!mgr.vfsready)
                     if doubleSystemVersion() < 26.0 {
@@ -61,6 +59,11 @@ struct TweaksView: View {
                         .disabled(!mgr.vfsready)
                 }
                 
+                Section(header: HeaderLabel(text: "Broken", icon: "exclamationmark.triangle.fill")) {
+                    NavigationLink("DarkBoard", destination: DarkBoardView())
+                        .disabled(true)
+                }
+                
                 NavigationLink("Extra Tools", destination: ToolsView())
             }
             .disabled(!mgr.dsready)
  ```

  </details>
- 🔴 `lara/views/tweaks/broken/PasscodeView.swift` (removed) — +0 / -487
  <details><summary>Diff</summary>

  ```diff
  @@ -1,487 +0,0 @@
-//
-//  PasscodeView.swift
-//  lara
-//
-//  Created by ruter on 29.03.26.
-//
-
-import SwiftUI
-import UIKit
-import PhotosUI
-import UniformTypeIdentifiers
-import Compression
-
-struct PasscodeKey: Identifiable {
-    let id: String
-    let digit: String
-    let displayName: String
-    
-    var sourceFilename: String { "\(id).png" }
-}
-
-struct PasscodeView: View {
-    @ObservedObject var mgr: laramgr
-    
-    @State private var selectedKeys: [String: Data] = [:]
-    @State private var showImagePicker: String?
-    @State private var showFilePicker = false
-    @State private var processing = false
-    @State private var statusMessage: String = ""
-    
-    let telephonyOptions = [
-        "TelephonyUI-15",
-        "TelephonyUI-14",
-        "TelephonyUI-13",
-        "TelephonyUI-12",
-        "TelephonyUI-11",
-        "TelephonyUI-10",
-        "TelephonyUI-9",
-        "TelephonyUI-8"
-    ]
-    
-    let passcodeKeys: [PasscodeKey] = [
-        PasscodeKey(id: "0", digit: "0", displayName: "0"),
-        PasscodeKey(id: "1", digit: "1", displayName: "1"),
-        PasscodeKey(id: "2", digit: "2", displayName: "2"),
-        PasscodeKey(id: "3", digit: "3", displayName: "3"),
-        PasscodeKey(id: "4", digit: "4", displayName: "4"),
-        PasscodeKey(id: "5", digit: "5", displayName: "5"),
-        PasscodeKey(id: "6", digit: "6", displayName: "6"),
-        PasscodeKey(id: "7", digit: "7", displayName: "7"),
-        PasscodeKey(id: "8", digit: "8", displayName: "8"),
-        PasscodeKey(id: "9", digit: "9", displayName: "9"),
-    ]
-
-    private var passcodeKeyMap: [String: PasscodeKey] {
-        Dictionary(uniqueKeysWithValues: passcodeKeys.map { ($0.id, $0) })
-    }
-
-    private let passcodeKeyLayout: [String?] = [
-        "1", "2", "3",
-        "4", "5", "6",
-        "7", "8", "9",
-        nil, "0", nil
-    ]
-    
-    var body: some View {
-        NavigationStack {
-            Form {
-                Section(header: Text("Import Theme")) {
-                    Button {
-                        showFilePicker = true
-                    } label: {
-                        Label("Import .passthm / .zip File", systemImage: "square.and.arrow.down")
-                    }
-                }
-                
-                Section {
-                    LazyVGrid(columns: [
-                        GridItem(.flexible(), spacing: 0),
-                        GridItem(.flexible(), spacing: 0),
-                        GridItem(.flexible(), spacing: 0)
-                    ], spacing: 0) {
-                        ForEach(Array(passcodeKeyLayout.enumerated()), id: \.offset) { _, keyId in
-                            if let keyId, let key = passcodeKeyMap[keyId] {
-                                PasscodeKeyButton(
-                                    key: key,
-                                    imageData: selectedKeys[key.id],
-                                    onSelect: {
-                                        showImagePicker = key.id
-                                    }
-                                )
-                            } else {
-                                Color.clear
-                                    .aspectRatio(1, contentMode: .fit)
-                                    .frame(maxWidth: .infinity)
-                            }
-                        }
-                    }
-                }
-                
-                Section(header: Text("Apply")) {
-                    Button("Apply Passcode Theme") {
-                        applyTheme()
-                    }
-                    .disabled(selectedKeys.isEmpty || processing)
-                    
-                    if !statusMessage.isEmpty {
-                        Text(statusMessage)
-                            .foregroundColor(statusMessage.contains("Error") ? .red : .green)
-                            .font(.footnote)
-                    }
-                }
-                
-                Section(header: Text("Danger Zone")) {
-                    Button("Clear All Keys", role: .destructive) {
-                        selectedKeys.removeAll()
-                    }
-                }
-            }
-            .headerProminence(.increased)
-            .navigationTitle("Passcode Theme")
-            .navigationBarTitleDisplayMode(.inline)
-            .sheet(item: $showImagePicker) { keyId in
-                ImagePicker(imageData: $selectedKeys[keyId])
-            }
-            .fileImporter(
-                isPresented: $showFilePicker,
-                allowedContentTypes: [UTType(filenameExtension: "passthm") ?? .zip, .zip],
-                allowsMultipleSelection: false
-            ) { result in
-                handleFileImport(result)
-            }
-        }
-    }
-    
-    func handleFileImport(_ result: Result<[URL], Error>) {
-        switch result {
-        case .success(let urls):
-            guard let url = urls.first else { return }
-            importPassthmFile(url: url)
-        case .failure(let error):
-            statusMessage = "Error: \(error.localizedDescription)"
-        }
-    }
-    
-    func importPassthmFile(url: URL) {
-        processing = true
-        statusMessage = "Importing theme..."
-        
-        DispatchQueue.global(qos: .userInitiated).async {
-            let accessing = url.startAccessingSecurityScopedResource()
-            defer {
-                if accessing {
-                    url.stopAccessingSecurityScopedResource()
-                }
-            }
-            
-            do {
-                let data = try Data(contentsOf: url)
-                let tempDir = FileManager.default.temporaryDirectory.appendingPathComponent(UUID().uuidString)
-                try FileManager.default.createDirectory(at: tempDir, withIntermediateDirectories: true)
-                defer {
-                    try? FileManager.default.removeItem(at: tempDir)
-                }
-                
-                let zipPath = tempDir.appendingPathComponent("theme.zip")
-                try data.write(to: zipPath)
-                
-                try unzipFile(at: zipPath, to: tempDir)
-                
-                let extractedKeys = try findAndExtractImages(from: tempDir)
-                
-                DispatchQueue.main.async {
-                    for (keyId, imageData) in extractedKeys {
-                        selectedKeys[keyId] = imageData
-                    }
-                    processing = false
-                    statusMessage = "Imported \(extractedKeys.count) key(s)"
-                }
-            } catch {
-                DispatchQueue.main.async {
-                    processing = false
-                    statusMessage = "Error: \(error.localizedDescription)"
-                }
-            }
-        }
-    }
-    
-    func unzipFile(at source: URL, to destination: URL) throws {
-        let fileData = try Data(contentsOf: source)
-        var offset = 0
-        
-        while offset < fileData.count - 4 {
-            let signature = fileData.subdata(in: offset..<offset+4).withUnsafeBytes { $0.load(as: UInt32.self) }
-            
-            if signature == 0x04034b50 {
-                guard let entry = readLocalFileEntry(data: fileData, offset: offset) else { break }
-                
-                let entryName = String(data: fileData.subdata(in: offset+30..<offset+30+entry.nameLength), encoding: .utf8) ?? ""
-                
-                if !entryName.hasSuffix("/") {
-                    let entryDestination = destination.appendingPathComponent(entryName)
-                    let parentDir = entryDestination.deletingLastPathComponent()
-                    try FileManager.default.createDirectory(at: parentDir, withIntermediateDirectories: true)
-                    
-                    if entry.compressionMethod == 0 {
-                        let fileDataEntry = fileData.subdata(in: offset+30+entry.nameLength..<offset+30+entry.nameLength+entry.compressedSize)
-                        try fileDataEntry.write(to: entryDestination)
-                    } else if entry.compressionMethod == 8 {
-                        let compressedData = fileData.subdata(in: offset+30+entry.nameLength..<offset+30+entry.nameLength+entry.compressedSize)
-                        let decompressed = decompress(deflate: compressedData, originalSize: entry.uncompressedSize)
-                        if let decompressed {
-                            try decompressed.write(to: entryDestination)
-                        }
-                    }
-                }
-                
-                offset += 30 + entry.nameLength + entry.extraLength + entry.compressedSize
-            } else {
-                break
-            }
-        }
-    }
-    
-    struct ZipEntry {
-        let compressionMethod: UInt16
-        let compressedSize: Int
-        let uncompressedSize: Int
-        let nameLength: Int
-        let extraLength: Int
-    }
-    
-    func readLocalFileEntry(data: Data, offset: Int) -> ZipEntry? {
-        guard offset + 30 <= data.count else { return nil }
-        
-        let compressionMethod = data.subdata(in: offset+8..<offset+10).withUnsafeBytes { $0.load(as: UInt16.self) }
-        let compressedSize = Int(data.subdata(in: offset+18..<offset+22).withUnsafeBytes { $0.load(as: UInt32.self) })
-        let uncompressedSize = Int(data.subdata(in: offset+22..<offset+26).withUnsafeBytes { $0.load(as: UInt32.self) })
-        let nameLength = Int(data.subdata(in: offset+26..<offset+28).withUnsafeBytes { $0.load(as: UInt16.self) })
-        let extraLength = Int(data.subdata(in: offset+28..<offset+30).withUnsafeBytes { $0.load(as: UInt16.self) })
-        
-        return ZipEntry(compressionMethod: compressionMethod, compressedSize: compressedSize, uncompressedSize: uncompressedSize, nameLength: nameLength, extraLength: extraLength)
-    }
-    
-    func decompress(deflate data: Data, originalSize: Int) -> Data? {
-        guard originalSize > 0 else { return Data() }
-        
-        let pageSize = 16384
-        let destinationBuffer = UnsafeMutablePointer<UInt8>.allocate(capacity: originalSize)
-        defer { destinationBuffer.deallocate() }
-        
-        let result = data.withUnsafeBytes { (sourceBuffer: UnsafeRawBufferPointer) -> Int in
-            guard let baseAddress = sourceBuffer.baseAddress else { return 0 }
-            return compression_decode_buffer(
-                destinationBuffer,
-                originalSize,
-                baseAddress.assumingMemoryBound(to: UInt8.self),
-                data.count,
-                nil,
-                COMPRESSION_ZLIB
-            )
-        }
-        
-        return result == originalSize ? Data(bytes: destinationBuffer, count: originalSize) : nil
-    }
-    
-    func findAndExtractImages(from directory: URL) throws -> [String: Data] {
-        var result: [String: Data] = [:]
-        let fileManager = FileManager.default
-        
-        guard let enumerator = fileManager.enumerator(at: directory, includingPropertiesForKeys: [.isRegularFileKey], options: [.skipsHiddenFiles]) else {
-            return result
-        }
-        
-        for case let fileURL as URL in enumerator {
-            let ext = fileURL.pathExtension.lowercased()
-            guard ext == "png" || ext == "jpg" || ext == "jpeg" else { continue }
-            
-            let filename = fileURL.lastPathComponent.lowercased()
-            let fullPath = fileURL.path.lowercased()
-            
-            if let keyId = matchFilenameToKey(filename) ?? matchFilenameToKey(fullPath) {
-                if let imageData = try? Data(contentsOf: fileURL) {
-                    result[keyId] = imageData
-                }
-            }
-        }
-        
-        return result
-    }
-    
-    func matchFilenameToKey(_ filename: String) -> String? {
-        let lowercased = filename.lowercased()
-        
-        for i in 0...9 {
-            if lowercased.contains("other-2-\(i)--dark") ||
-                lowercased.contains("-\(i)-") ||
-                lowercased.contains("-\(i)@") ||
-                lowercased.contains("_\(i)_") ||
-                lowercased.contains("_\(i)@") ||
-                lowercased.contains("/\(i).png") ||
-                lowercased.contains("/\(i).jpg") ||
-                lowercased.contains("/\(i).jpeg") {
-                return String(i)
-            }
-        }
-        
-        return nil
-    }
-    
-    func applyTheme() {
-        guard mgr.sbxready else {
-            statusMessage = "Error: SBX not ready"
-            return
-        }
-        
-        processing = true
-        statusMessage = "Applying theme..."
-        
-        DispatchQueue.global(qos: .userInitiated).async {
-            guard let basePath = resolveTelephonyBasePath() else {
-                DispatchQueue.main.async {
-                    processing = false
-                    statusMessage = "Error: TelephonyUI cache not found"
-                }
-                return
-            }
-            var successCount = 0
-            var failCount = 0
-            
-            for (keyId, imageData) in selectedKeys {
-                let filename = getFilenameForKey(keyId)
-                let targetPath = "\(basePath)/\(filename)"
-                
-                if sbxwrite(path: targetPath, data: imageData) {
-                    successCount += 1
-                    mgr.logmsg("Applied \(filename) to \(targetPath)")
-                } else {
-                    failCount += 1
-                    mgr.logmsg("Failed to apply \(filename)")
-                }
-            }
-            
-            DispatchQueue.main.async {
-                processing = false
-                if failCount == 0 {
-                    statusMessage = "applied \(successCount) key(s)"
-                } else {
-                    statusMessage = "applied \(successCount), failed \(failCount)"
-                }
-            }
-        }
-    }
-
-    func resolveTelephonyBasePath() -> String? {
-        var candidates: [String] = []
-        for version in telephonyOptions {
-            candidates.append("/var/mobile/Library/Caches/\(version)")
-            candidates.append("/var/mobile/Library/Caches/com.apple.\(version)")
-            candidates.append("/var/mobile/Library/Caches/com.apple.\(version.lowercased())")
-            candidates.append("/var/mobile/Library/Caches/com.apple.TelephonyUI/\(version)")
-            candidates.append("/var/mobile/Library/Caches/com.apple.telephonyui/\(version)")
-        }
-
-        for path in candidates {
-            if sbxdirExists(path: path) {
-                mgr.logmsg("TelephonyUI cache: \(path)")
-                return path
-            }
-        }
-
-        mgr.logmsg("TelephonyUI cache not found. Tried: \(candidates.joined(separator: ", "))")
-        return nil
-    }
-
-    func sbxdirExists(path: String) -> Bool {
-        var isDir: ObjCBool = false
-        return FileManager.default.fileExists(atPath: path, isDirectory: &isDir) && isDir.boolValue
-    }
-
-    func sbxwrite(path: String, data: Data) -> Bool {
-        do {
-            try data.write(to: URL(fileURLWithPath: path), options: .atomic)
-            return true
-        } catch {
-            return false
-        }
-    }
-    
-    func getFilenameForKey(_ keyId: String) -> String {
-        switch keyId {
-        case "delete": return "other-2-DELETE--dark.png"
-        case "cancel": return "other-2-CANCEL--dark.png"
-        default: return "other-2-\(keyId)--dark.png"
-        }
-    }
-}
-
-struct PasscodeKeyButton: View {
-    let key: PasscodeKey
-    let imageData: Data?
-    let onSelect: () -> Void
-    
-    var body: some View {
-        Button(action: onSelect) {
-            GeometryReader { geo in
-                ZStack {
-                    if let data = imageData, let uiImage = UIImage(data: data) {
-                        Image(uiImage: uiImage)
-                            .resizable()
-                            .scaledToFill()
-                            .frame(width: geo.size.width, height: geo.size.width)
-                            .clipped()
-                    } else {
-                        Rectangle()
-                            .fill(Color.gray.opacity(0.2))
-                    }
-                }
-                .frame(width: geo.size.width, height: geo.size.width)
-            }
-            .aspectRatio(1, contentMode: .fit)
-        }
-        .buttonStyle(.plain)
-    }
-}
-
-extension String: @retroactive Identifiable {
-    public var id: String { self }
-}
-
-struct ImagePicker: UIViewControllerRepresentable {
-    @Binding var imageData: Data?
-    @Environment(\.dismiss) var dismiss
-    
-    func makeUIViewController(context: Context) -> PHPickerViewController {
-        var config = PHPickerConfiguration()
-        config.filter = .images
-        config.selectionLimit = 1
-        
-        let picker = PHPickerViewController(configuration: config)
-        picker.delegate = context.coordinator
-        return picker
-    }
-    
-    func updateUIViewController(_ uiViewController: PHPickerViewController, context: Context) {}
-    
-    func makeCoordinator() -> Coordinator {
-        Coordinator(self)
-    }
-    
-    class Coordinator: NSObject, PHPickerViewControllerDelegate {
-        let parent: ImagePicker
-        
-        init(_ parent: ImagePicker) {
-            self.parent = parent
-        }
-        
-        func picker(_ picker: PHPickerViewController, didFinishPicking results: [PHPickerResult]) {
-            parent.dismiss()
-            
-            guard let result = results.first else { return }
-            
-            result.itemProvider.loadObject(ofClass: UIImage.self) { [weak self] object, error in
-                guard let image = object as? UIImage else { return }
-                guard let self else { return }
-                
-                let resized = self.resizeImage(image, targetHeight: 202)
-                if let pngData = resized.pngData() {
-                    DispatchQueue.main.async {
-                        self.parent.imageData = pngData
-                    }
-                }
-            }
-        }
-        
-        func resizeImage(_ image: UIImage, targetHeight: CGFloat) -> UIImage {
-            let scale = targetHeight / image.size.height
-            let newWidth = image.size.width * scale
-            let newSize = CGSize(width: newWidth, height: targetHeight)
-            
-            let renderer = UIGraphicsImageRenderer(size: newSize)
-            return renderer.image { _ in
-                image.draw(in: CGRect(origin: .zero, size: newSize))
-            }
-        }
-    }
-}
  ```

  </details>
- 🟡 `lara/views/tweaks/broken/darkboard/DarkBoardExploreView.swift` (modified) — +25 / -27
  <details><summary>Diff</summary>

  ```diff
  @@ -12,6 +12,7 @@ struct DarkBoardExploreView: View {
     @State private var filter: IconThemeGalleryFilter = .random
     @State private var searchTerm = ""
     @State private var alert: DarkBoardExploreAlert?
+    @State private var displayedThemes: [GalleryTheme] = []
 
     var body: some View {
         ScrollView {
@@ -25,17 +26,26 @@ struct DarkBoardExploreView: View {
         .searchable(text: $searchTerm, prompt: "Search themes or authors")
         .refreshable {
             await gallery.loadThemes(forceRefresh: true)
+            updateDisplayedThemes()
         }
         .task {
             if gallery.themes.isEmpty {
                 await gallery.loadThemes()
             }
+            updateDisplayedThemes()
         }
-        .alert(item: $alert) { alert in
-            Alert(title: Text("Theme Gallery"), message: Text(alert.message), dismissButton: .default(Text("OK")))
+        .onChange(of: searchTerm)     { _ in updateDisplayedThemes() }
+        .onChange(of: filter)         { _ in updateDisplayedThemes() }
+        .onChange(of: gallery.themes) { _ in updateDisplayedThemes() }
+        .alert(item: $alert) { a in
+            Alert(title: Text("Theme Gallery"), message: Text(a.message), dismissButton: .default(Text("OK")))
         }
     }
 
+    private func updateDisplayedThemes() {
+        displayedThemes = gallery.filteredThemes(searchTerm: searchTerm, filter: filter)
+    }
+
     private var filterBar: some View {
         HStack {
             Menu {
@@ -57,9 +67,7 @@ struct DarkBoardExploreView: View {
                     .background(.thinMaterial)
                     .clipShape(Capsule())
             }
-
             Spacer()
-
             if gallery.isLoading {
                 ProgressView()
                     .controlSize(.small)
@@ -79,6 +87,7 @@ struct DarkBoardExploreView: View {
                 Button("Retry") {
                     Task {
                         await gallery.loadThemes(forceRefresh: true)
+                        updateDisplayedThemes()
                     }
                 }
                 .buttonStyle(.borderedProminent)
@@ -106,7 +115,7 @@ struct DarkBoardExploreView: View {
         } else {
             LazyVStack(spacing: 14) {
                 ForEach(displayedThemes) { theme in
-                    GalleryThemeCard(theme: theme, isImported: themes.theme(named: theme.name) != nil, isDownloading: gallery.isDownloading(theme)) {
+                    GalleryThemeCard(theme: theme, previewURL: gallery.previewURL(for: theme), isImported: themes.theme(named: theme.name) != nil, isDownloading: gallery.isDownloading(theme)) {
                         Task {
                             do {
                                 try await gallery.downloadAndImport(theme)
@@ -120,23 +129,18 @@ struct DarkBoardExploreView: View {
             }
         }
     }
-
-    private var displayedThemes: [GalleryTheme] {
-        gallery.filteredThemes(searchTerm: searchTerm, filter: filter)
-    }
 }
 
 private struct GalleryThemeCard: View {
-    @ObservedObject private var gallery = IconThemeGalleryManager.shared
-
     let theme: GalleryTheme
+    let previewURL: URL?
     let isImported: Bool
     let isDownloading: Bool
     let onDownload: () -> Void
 
     var body: some View {
         VStack(alignment: .leading, spacing: 0) {
-            if let previewURL = gallery.previewURL(for: theme) {
+            if let previewURL {
                 AsyncImage(url: previewURL) { phase in
                     switch phase {
                     case let .success(image):
@@ -146,11 +150,7 @@ private struct GalleryThemeCard: View {
                             .frame(height: 180)
                             .frame(maxWidth: .infinity)
                             .clipped()
-                    case .failure(_):
-                        previewPlaceholder
-                    case .empty:
-                        previewPlaceholder
-                    @unknown default:
+                    default:
                         previewPlaceholder
                     }
                 }
@@ -176,30 +176,30 @@ private struct GalleryThemeCard: View {
                             .foregroundStyle(.green)
                     }
                 }
-
                 Text(theme.description)
-                    .font(.footnote)
-                    .foregroundStyle(.secondary)
-                    .lineLimit(3)
 
                 Button {
-                    onDownload()
+                    Task { await onDownload() }
                 } label: {
                     HStack {
                         if isDownloading {
                             ProgressView()
                                 .controlSize(.small)
                                 .tint(.white)
                         } else {
-                            Image(systemName: isImported ? "arrow.triangle.2.circlepath" : "arrow.down.circle")
+                            Image(systemName: isImported
+                                  ? "arrow.triangle.2.circlepath"
+                                  : "arrow.down.circle")
                         }
+
                         Text(isImported ? "Reimport Theme" : "Import Theme")
                     }
                     .frame(maxWidth: .infinity)
                     .padding(.vertical, 10)
                 }
-                .disabled(isDownloading)
                 .buttonStyle(.borderedProminent)
+                .disabled(isDownloading)
+                .contentShape(Rectangle())
             }
             .padding()
         }
@@ -214,8 +214,6 @@ private struct GalleryThemeCard: View {
                 .font(.system(size: 34, weight: .medium))
                 .foregroundStyle(.secondary)
         }
-        .frame(height: 180)
-        .frame(maxWidth: .infinity)
-        .clipShape(RoundedRectangle(cornerRadius: 18))
+        .frame(height: 180).frame(maxWidth: .infinity)
     }
 }
  ```

  </details>
- 🟢 `lara/views/tweaks/passcode/PasscodeExploreView.swift` (added) — +179 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,179 @@
+//
+//  PasscodeExploreView.swift
+//  lara
+//
+//  Created by neonmodder123 on 20/05/2026.
+//
+
+import SwiftUI
+
+struct PasscodeExploreView: View {
+    @ObservedObject var mgr: laramgr
+    @ObservedObject private var gallery = PasscodeGalleryManager.shared
+
+    @State private var searchTerm = ""
+    @State private var alertMessage: String?
+    var onImport: ((URL) -> Void)? = nil
+    @Environment(\.dismiss) private var dismiss
+    
+    @State private var showRepoMgr = false
+    
+    private func handleDownload(_ theme: PasscodeGalleryTheme) async {
+        do {
+            try await gallery.downloadAndImport(theme)
+            let dest = FileManager.default
+                .urls(for: .documentDirectory, in: .userDomainMask)[0]
+                .appendingPathComponent(theme.name + ".passthm")
+            onImport?(dest)
+            dismiss()
+        } catch { alertMessage = error.localizedDescription }
+    }
+
+    private var displayed: [PasscodeGalleryTheme] {
+        guard !searchTerm.isEmpty else { return gallery.themes }
+        let q = searchTerm.lowercased()
+        return gallery.themes.filter {
+            $0.name.lowercased().contains(q) ||
+            $0.description.lowercased().contains(q) ||
+            $0.contact.displayName.lowercased().contains(q)
+        }
+    }
+
+    var body: some View {
+        ScrollView {
+            LazyVGrid(columns: [GridItem(.flexible(), spacing: 14)], spacing: 14) {
+                ForEach(displayed) { theme in
+                    PasscodeGalleryCard(
+                        theme: theme,
+                        previewURL: gallery.previewURL(for: theme),
+                        isDownloading: gallery.isDownloading(theme)
+                    ) { Task { await handleDownload(theme) } }
+                }
+            }
+            .padding()
+        }
+        .toolbar {
+            ToolbarItem(placement: .navigationBarTrailing) {
+                Button {
+                    showRepoMgr = true
+                } label: {
+                    Image(systemName: "shippingbox")
+                }
+            }
+        }
+        .sheet(isPresented: $showRepoMgr) {
+            PasscodeRepoView()
+        }
+        .navigationTitle("Explore Passcode Themes")
+        .navigationBarTitleDisplayMode(.inline)
+        .searchable(text: $searchTerm, prompt: "Search themes or authors")
+        .refreshable { await gallery.loadThemes(forceRefresh: true) }
+        .task { if gallery.themes.isEmpty { await gallery.loadThemes() } }
+        .alert("Passcode Themes", isPresented: Binding(get: { alertMessage != nil }, set: { if !$0 { alertMessage = nil } })) { Button("OK", role: .cancel) {} } message: { Text(alertMessage ?? "") }
+    }
+
+    private var loadingView: some View {
+        VStack(spacing: 12) {
+            ProgressView().controlSize(.large)
+            Text("Loading themes…")
+                .font(.footnote)
+                .foregroundStyle(.secondary)
+        }
+        .frame(maxWidth: .infinity)
+        .padding(.top, 80)
+    }
+
+    private func errorView(_ message: String) -> some View {
+        VStack(alignment: .leading, spacing: 12) {
+            Text("Could not load themes.")
+                .font(.headline)
+            Text(message)
+                .font(.footnote)
+                .foregroundStyle(.secondary)
+            Button("Retry") { Task { await gallery.loadThemes(forceRefresh: true) } }
+            .buttonStyle(.borderedProminent)
+        }
+        .padding()
+        .frame(maxWidth: .infinity, alignment: .leading)
+        .background(.thinMaterial)
+        .clipShape(RoundedRectangle(cornerRadius: 18))
+    }
+}
+
+private struct PasscodeGalleryCard: View {
+    let theme: PasscodeGalleryTheme
+    let previewURL: URL?
+    let isDownloading: Bool
+    let onDownload: () -> Void
+
+    var body: some View {
+        VStack(alignment: .leading, spacing: 0) {
+            if let previewURL {
+                AsyncImage(url: previewURL) { phase in
+                    switch phase {
+                    case .success(let img):
+                        img
+                            .resizable()
+                            .interpolation(.low)
+                            .scaledToFit()
+                            .frame(maxWidth: .infinity)
+                            .frame(height: 220)
+                            .background(Color.black.opacity(0.001))
+                    default:
+                        placeholder
+                    }
+                }
+            } else {
+                placeholder
+            }
+
+            VStack(alignment: .leading, spacing: 8) {
+                HStack(alignment: .top) {
+                    VStack(alignment: .leading, spacing: 3) {
+                        Text(theme.name).font(.headline).lineLimit(1)
+                        Text(theme.contact.displayName)
+                            .font(.caption)
+                            .foregroundStyle(.secondary)
+                            .lineLimit(1)
+                    }
+                    Spacer()
+                }
+
+                if !theme.description.isEmpty {
+                    Text(theme.description)
+                        .font(.subheadline)
+                        .foregroundStyle(.secondary)
+                }
+
+                Button(action: onDownload) {
+                    HStack {
+                        if isDownloading { ProgressView().controlSize(.small).tint(.white) } else { Image(systemName: "arrow.down.circle") }
+                        Text("Import Theme")
+                    }
+                    .frame(maxWidth: .infinity)
+                    .padding(.vertical, 10)
+                }
+                .buttonStyle(.borderedProminent)
+                .disabled(isDownloading)
+            }
+            .padding()
+        }
+        .background(.thinMaterial)
+        .clipShape(RoundedRectangle(cornerRadius: 18))
+    }
+
+    private var placeholder: some View {
+        ZStack {
+            LinearGradient(
+                colors: [.gray.opacity(0.35), .gray.opacity(0.15)],
+                startPoint: .topLeading,
+                endPoint: .bottomTrailing
+            )
+            Image(systemName: "lock.rectangle.stack")
+                .font(.system(size: 34, weight: .medium))
+                .foregroundStyle(.secondary)
+        }
+        .frame(height: 220)
+        .frame(maxWidth: .infinity)
+    }
+}
  ```

  </details>
- 🟢 `lara/views/tweaks/passcode/PasscodeRepoView.swift` (added) — +117 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,117 @@
+//
+//  PasscodeRepoView.swift
+//  lara
+//
+//  Created by neonmodder123 on 20/05/2026.
+//
+
+import SwiftUI
+import Combine
+
+struct PasscodeRepoView: View {
+    @ObservedObject private var gallery = PasscodeGalleryManager.shared
+    @State private var showAddRepo = false
+    @State private var newRepoURL = ""
+
+    var body: some View {
+        NavigationStack {
+            List {
+                Section {
+                    ForEach(gallery.repos) { repo in
+                        HStack(spacing: 12) {
+                            if let icon = repo.data?.icon,
+                               let iconURL = URL(string: icon) {
+                                AsyncImage(url: iconURL) { phase in
+                                    switch phase {
+                                    case .success(let img):
+                                        img
+                                            .resizable()
+                                            .interpolation(.low)
+                                            .scaledToFit()
+                                            .frame(maxWidth: .infinity)
+                                            .frame(height: 220)
+                                    default:
+                                        Image(systemName: "shippingbox")
+                                            .resizable()
+                                            .scaledToFit()
+                                            .padding(6)
+                                            .foregroundStyle(.secondary)
+                                    }
+                                }
+                                .frame(width: 42, height: 42)
+                                .clipShape(RoundedRectangle(cornerRadius: 10))
+                            }
+                            VStack(alignment: .leading, spacing: 2) {
+                                Text(repo.data?.name ?? repo.url)
+                                    .font(.headline)
+
+                                if let author = repo.data?.author,
+                                   !author.isEmpty {
+                                    Text(author)
+                                        .font(.caption)
+                                        .foregroundColor(.secondary)
+                                }
+                            }
+                            Spacer()
+                            if repo.isLoading {
+                                ProgressView()
+                            } else if let error = repo.error {
+                                Text(error)
+                                    .font(.caption)
+                                    .foregroundColor(.orange)
+                                    .lineLimit(1)
+                            } else if repo.url != defaultPasscodeRepoURL {
+                                Button(role: .destructive) {
+                                    gallery.removeRepo(repo.url)
+                                } label: {
+                                    Image(systemName: "trash")
+                                }
+                            }
+                        }
+                        .swipeActions {
+                            if repo.url != defaultPasscodeRepoURL {
+                                Button(role: .destructive) {
+                                    gallery.removeRepo(repo.url)
+                                } label: {
+                                    Text("Remove")
+                                }
+                            }
+                        }
+                    }
+                } header: {
+                    Text("Repos")
+                }
+            }
+            .navigationTitle("Passcode Repos")
+            .toolbar {
+                ToolbarItem(placement: .navigationBarTrailing) {
+                    Button {
+                        newRepoURL = ""
+                        showAddRepo = true
+                    } label: {
+                        Image(systemName: "plus")
+                    }
+                }
+                ToolbarItem(placement: .navigationBarLeading) {
+                    Button {
+                        Task { await gallery.refreshRepos(forceRefresh: true) }
+                    } label: {
+                        Image(systemName: "arrow.clockwise")
+                    }
+                }
+            }
+            .alert("Add Passcode Repo", isPresented: $showAddRepo) {
+                TextField("URL", text: $newRepoURL)
+                    .textInputAutocapitalization(.never)
+                    .keyboardType(.URL)
+                    .autocorrectionDisabled()
+                Button("Add") {
+                    Task { await gallery.addRepo(newRepoURL) }
+                }
+                Button("Cancel", role: .cancel) {}
+            } message: {
+                Text("Enter the URL to a passcode theme repo JSON.")
+            }
+        }
+    }
+}
  ```

  </details>
- 🟢 `lara/views/tweaks/passcode/PasscodeView.swift` (added) — +683 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,683 @@
+//
+//  PasscodeView.swift
+//  lara
+//
+//  Created by ruter on 29.03.26.
+//
+
+import SwiftUI
+import UIKit
+import PhotosUI
+import UniformTypeIdentifiers
+import Compression
+import Combine
+
+private let passcodeThemeStorageRoot = URL(
+    fileURLWithPath: "/var/mobile/.DO-NOT-DELETE-lara/PasscodeThemes",
+    isDirectory: true
+)
+
+private let passcodeBackupDir = passcodeThemeStorageRoot
+    .appendingPathComponent("Originals", isDirectory: true)
+
+struct PasscodeKey: Identifiable {
+    let id: String
+    let digit: String
+    let displayName: String
+    
+    var sourceFilename: String { "\(id).png" }
+}
+
+final class PasscodeThemeManager: ObservableObject {
+    static let shared = PasscodeThemeManager()
+
+    @Published var isApplying = false
+    @Published var progress: Double = 0
+    @Published var message = ""
+
+    private let fm = FileManager.default
+
+    func createDirectoriesIfNeeded() {
+        try? fm.createDirectory(
+            at: passcodeBackupDir,
+            withIntermediateDirectories: true,
+            attributes: nil
+        )
+    }
+
+    func backupIfNeeded(targetPath: String) {
+        createDirectoriesIfNeeded()
+        let targetURL = URL(fileURLWithPath: targetPath)
+        let backupURL = backupURLFor(targetPath: targetPath)
+
+        guard fm.fileExists(atPath: targetPath) else { return }
+        
+        if !fm.fileExists(atPath: backupURL.path) {
+            try? fm.copyItem(at: targetURL, to: backupURL)
+        }
+    }
+
+    func restoreBackup(targetPath: String) throws {
+        let backupURL = backupURLFor(targetPath: targetPath)
+        guard fm.fileExists(atPath: backupURL.path) else { return }
+        let data = try Data(contentsOf: backupURL)
+        
+        let overwrite = laramgr.shared.lara_overwritefile(
+            target: targetPath,
+            data: data
+        )
+        
+        if !overwrite.ok {
+            throw NSError(domain: "PasscodeTheme", code: 1, userInfo: [ NSLocalizedDescriptionKey: overwrite.message ]
+            )
+        }
+    }
+    
+    func restoreAll(basePath: String, logmsg: ((String) -> Void)? = nil) throws {
+        let fm = FileManager.default
+        guard let enumerator = fm.enumerator(atPath: basePath) else {
+            throw NSError(domain: "PasscodeTheme", code: 3,
+                          userInfo: [NSLocalizedDescriptionKey: "Failed to enumerate cache"])
+        }
+
+        var allTargets: [String] = []
+        for case let file as String in enumerator {
+            guard file.lowercased().hasSuffix(".png") else { continue }
+            let fullPath = "\(basePath)/\(file)"
+            let lower = file.lowercased()
+            for i in 0...9 {
+                if lower.contains("other-2-\(i)--dark") ||
+                   lower.contains("-\(i)-") ||
+                   lower.contains("_\(i)_") ||
+                   lower.contains("_\(i)@") {
+                    allTargets.append(fullPath)
+                    break
+                }
+            }
+        }
+
+        for path in allTargets {
+            do {
+                try restoreBackup(targetPath: path)
+                logmsg?("restored \(path)")
+            } catch {
+                logmsg?("failed to restore \(path): \(error.localizedDescription)")
+            }
+        }
+    }
+
+    func applyImage(data: Data, to targetPath: String) throws {
+        backupIfNeeded(targetPath: targetPath)
+        let overwrite = laramgr.shared.lara_overwritefile(target: targetPath, data: data)
+
+        if !overwrite.ok { throw NSError(domain: "PasscodeTheme", code: 2, userInfo: [NSLocalizedDescriptionKey: overwrite.message]) }
+    }
+
+    private func backupURLFor(targetPath: String) -> URL {
+        let sanitized = targetPath
+            .replacingOccurrences(of: "/", with: "_")
+        return passcodeBackupDir
+            .appendingPathComponent(sanitized)
+    }
+}
+
+struct PasscodeView: View {
+    @ObservedObject var mgr: laramgr
+    
+    @State private var selectedKeys: [String: Data] = [:]
+    @State private var showImagePicker: String?
+    @State private var showFilePicker = false
+    @State private var processing = false
+    @State private var statusMessage: String = ""
+    
+    @ObservedObject private var themeManager = IconThemeManager.shared
+    @ObservedObject private var passcodeThemeManager = PasscodeThemeManager.shared
+    
+    let initialImportURL: URL?
+    
+    init(mgr: laramgr, initialImportURL: URL? = nil) {
+        self.mgr = mgr
+        self.initialImportURL = initialImportURL
+    }
+    
+    let telephonyOptions = [
+        "TelephonyUI-15",
+        "TelephonyUI-14",
+        "TelephonyUI-13",
+        "TelephonyUI-12",
+        "TelephonyUI-11",
+        "TelephonyUI-10",
+        "TelephonyUI-9",
+        "TelephonyUI-8"
+    ]
+    
+    let passcodeKeys: [PasscodeKey] = [
+        PasscodeKey(id: "0", digit: "0", displayName: "0"),
+        PasscodeKey(id: "1", digit: "1", displayName: "1"),
+        PasscodeKey(id: "2", digit: "2", displayName: "2"),
+        PasscodeKey(id: "3", digit: "3", displayName: "3"),
+        PasscodeKey(id: "4", digit: "4", displayName: "4"),
+        PasscodeKey(id: "5", digit: "5", displayName: "5"),
+        PasscodeKey(id: "6", digit: "6", displayName: "6"),
+        PasscodeKey(id: "7", digit: "7", displayName: "7"),
+        PasscodeKey(id: "8", digit: "8", displayName: "8"),
+        PasscodeKey(id: "9", digit: "9", displayName: "9"),
+    ]
+
+    private var passcodeKeyMap: [String: PasscodeKey] { Dictionary(uniqueKeysWithValues: passcodeKeys.map { ($0.id, $0) }) }
+
+    private let passcodeKeyLayout: [String?] = [
+        "1", "2", "3",
+        "4", "5", "6",
+        "7", "8", "9",
+        nil, "0", nil
+    ]
+    
+    var body: some View {
+        NavigationStack {
+            Form {
+                Section(header: Text("Import Theme")) {
+                    NavigationLink("Explore") {
+                        PasscodeExploreView(mgr: mgr) { url in
+                            importPassthmFile(url: url)
+                        }
+                    }
+                    Button { showFilePicker = true } label: {
+                        Label("Import .passthm / .zip File", systemImage: "square.and.arrow.down")
+                    }
+                }
+                Section {
+                    if passcodeThemeManager.isApplying {
+                        VStack(spacing: 10) {
+                            ProgressView(value: passcodeThemeManager.progress, total: 1.0)
+                            Text(passcodeThemeManager.message)
+                                .font(.footnote)
+                                .foregroundStyle(.secondary)
+                        }
+                        .padding(.vertical, 4)
+                    }
+                    LazyVGrid(columns: [GridItem(.flexible(), spacing: 0), GridItem(.flexible(), spacing: 0), GridItem(.flexible(), spacing: 0)], spacing: 0) {
+                        ForEach(Array(passcodeKeyLayout.enumerated()), id: \.offset) { _, keyId in
+                            if let keyId,
+                               let key = passcodeKeyMap[keyId] {
+
+                                PasscodeKeyButton(key: key, imageData: selectedKeys[key.id], onSelect: { showImagePicker = key.id })
+                            } else {
+                                Color.clear
+                                    .aspectRatio(1, contentMode: .fit)
+                                    .frame(maxWidth: .infinity)
+                            }
+                        }
+                    }
+                }
+                
+                Section(header: Text("Apply")) {
+                    Button("Apply Passcode Theme") {
+                        applyTheme()
+                    }
+                    .disabled(selectedKeys.isEmpty || processing || passcodeThemeManager.isApplying)
+                    if !statusMessage.isEmpty {
+                        Text(statusMessage)
+                            .foregroundColor(statusMessage.contains("Error") ? .red : .green)
+                            .font(.footnote)
+                    }
+                }
+                
+                Section(header: Text("Danger Zone")) {
+                    Button("Clear All Keys", role: .destructive) {
+                        selectedKeys.removeAll()
+                    }
+                    Button("Restore Original Icons", role: .destructive) { restoreTheme() }
+                    .disabled(processing || passcodeThemeManager.isApplying)
+                }
+            }
+            .headerProminence(.increased)
+            .navigationTitle("Passcode Theme")
+            .navigationBarTitleDisplayMode(.inline)
+            .sheet(item: $showImagePicker) { keyId in
+                ImagePicker(imageData: $selectedKeys[keyId])
+            }
+            .fileImporter(
+                isPresented: $showFilePicker,
+                allowedContentTypes: [UTType(filenameExtension: "passthm") ?? .zip, .zip],
+                allowsMultipleSelection: false
+            ) { result in
+                handleFileImport(result)
+            }
+        }
+        .task {
+            if let url = initialImportURL {
+                importPassthmFile(url: url)
+            }
+        }
+    }
+    
+    func handleFileImport(_ result: Result<[URL], Error>) {
+        switch result {
+        case .success(let urls):
+            guard let url = urls.first else { return }
+            importPassthmFile(url: url)
+        case .failure(let error):
+            statusMessage = "Error: \(error.localizedDescription)"
+        }
+    }
+    
+    func importPassthmFile(url: URL) {
+        processing = true
+        statusMessage = "Importing theme..."
+        
+        DispatchQueue.global(qos: .userInitiated).async {
+            let accessing = url.startAccessingSecurityScopedResource()
+            defer {
+                if accessing {
+                    url.stopAccessingSecurityScopedResource()
+                }
+            }
+            
+            do {
+                let data = try Data(contentsOf: url)
+                let tempDir =
+                    FileManager.default.temporaryDirectory.appendingPathComponent(UUID().uuidString)
+                try FileManager.default.createDirectory(at: tempDir, withIntermediateDirectories: true)
+                defer {
+                    try? FileManager.default.removeItem(at: tempDir)
+                }
+                
+                let zipPath = tempDir.appendingPathComponent("theme.zip")
+                try data.write(to: zipPath)
+                try themeManager.unzipFile(at: zipPath, to: tempDir)
+                
+                let extractedKeys = try findAndExtractImages(from: tempDir)
+                
+                DispatchQueue.main.async {
+                    for (keyId, imageData) in extractedKeys {
+                        selectedKeys[keyId] = imageData
+                    }
+                    processing = false
+                    statusMessage = "Imported \(extractedKeys.count) key(s)"
+                }
+            } catch {
+                DispatchQueue.main.async {
+                    processing = false
+                    statusMessage = "Error: \(error.localizedDescription)"
+                }
+            }
+        }
+    }
+    
+    struct ZipEntry {
+        let compressionMethod: UInt16
+        let compressedSize: Int
+        let uncompressedSize: Int
+        let nameLength: Int
+        let extraLength: Int
+    }
+
+    func readLocalFileEntry(
+        data: Data,
+        offset: Int
+    ) -> ZipEntry? {
+        guard offset + 30 <= data.count else { return nil }
+        let compressionMethod = data.subdata(in: offset + 8..<offset + 10).withUnsafeBytes { $0.load(as: UInt16.self) }
+        let compressedSize = Int(data.subdata(in: offset + 18..<offset + 22).withUnsafeBytes { $0.load(as: UInt32.self) })
+        let uncompressedSize = Int(data.subdata(in: offset + 22..<offset + 26).withUnsafeBytes { $0.load(as: UInt32.self) })
+        let nameLength = Int(data.subdata(in: offset + 26..<offset + 28).withUnsafeBytes { $0.load(as: UInt16.self) })
+        let extraLength = Int(data.subdata(in: offset + 28..<offset + 30).withUnsafeBytes { $0.load(as: UInt16.self) })
+
+        return ZipEntry(
+            compressionMethod: compressionMethod,
+            compressedSize: compressedSize,
+            uncompressedSize: uncompressedSize,
+            nameLength: nameLength,
+            extraLength: extraLength
+        )
+    }
+    
+    func decompress(
+        deflate data: Data,
+        originalSize: Int
+    ) -> Data? {
+        guard originalSize > 0 else { return Data() }
+        let destinationBuffer = UnsafeMutablePointer<UInt8>
+            .allocate(capacity: originalSize)
+        defer { destinationBuffer.deallocate() }
+        
+        let result = data.withUnsafeBytes { (sourceBuffer: UnsafeRawBufferPointer) -> Int in
+            guard let baseAddress = sourceBuffer.baseAddress else { return 0 }
+            return compression_decode_buffer(
+                destinationBuffer,
+                originalSize,
+                baseAddress.assumingMemoryBound(to: UInt8.self),
+                data.count,
+                nil,
+                COMPRESSION_ZLIB
+            )
+        }
+        
+        return result == originalSize ? Data(bytes: destinationBuffer, count: originalSize) : nil
+    }
+    
+    func findAndExtractImages(from directory: URL) throws -> [String: Data] {
+        var result: [String: Data] = [:]
+        let fileManager = FileManager.default
+        
+        guard let enumerator = fileManager.enumerator(at: directory, includingPropertiesForKeys: [.isRegularFileKey], options: [.skipsHiddenFiles]) else { return result }
+        
+        for case let fileURL as URL in enumerator {
+            let ext = fileURL.pathExtension.lowercased()
+            guard ext == "png" || ext == "jpg" || ext == "jpeg" else { continue }
+            
+            let filename = fileURL.lastPathComponent.lowercased()
+            let fullPath = fileURL.path.lowercased()
+            
+            if let keyId =
+                matchFilenameToKey(filename) ??
+                matchFilenameToKey(fullPath) {
+                if let imageData = try? Data(contentsOf: fileURL) {
+                    result[keyId] = imageData
+                }
+            }
+        }
+        
+        return result
+    }
+    
+    func matchFilenameToKey(_ filename: String) -> String? {
+        let lowercased = filename.lowercased()
+        
+        for i in 0...9 {
+            if lowercased.contains("other-2-\(i)--dark") ||
+                lowercased.contains("-\(i)-") ||
+                lowercased.contains("-\(i)@") ||
+                lowercased.contains("_\(i)_") ||
+                lowercased.contains("_\(i)@") ||
+                lowercased.contains("/\(i).png") ||
+                lowercased.contains("/\(i).jpg") ||
+                lowercased.contains("/\(i).jpeg") {
+                return String(i)
+            }
+        }
+        
+        return nil
+    }
+    
+    func applyTheme() {
+        guard mgr.sbxready else {
+            statusMessage = "Error: SBX not ready"
+            return
+        }
+
+        processing = true
+        statusMessage = ""
+
+        DispatchQueue.global(qos: .userInitiated).async {
+            guard let basePath = resolveTelephonyBasePath() else {
+                DispatchQueue.main.async {
+                    processing = false
+                    statusMessage = "Error: TelephonyUI cache not found"
+                }
+                return
+            }
+
+            let fm = FileManager.default
+            guard let enumerator = fm.enumerator(atPath: basePath) else {
+                DispatchQueue.main.async {
+                    processing = false
+                    statusMessage = "Error: failed to enumerate cache"
+                }
+                return
+            }
+
+            var targets: [String: [String]] = [:]
+
+            for case let file as String in enumerator {
+                let lower = file.lowercased()
+                guard lower.hasSuffix(".png") else { continue }
+
+                for i in 0...9 {
+                    if lower.contains("other-2-\(i)--dark") ||
+                        lower.contains("-\(i)-") ||
+                        lower.contains("_\(i)_") ||
+                        lower.contains("_\(i)@") {
+                        targets[String(i), default: []].append("\(basePath)/\(file)")
+                    }
+                }
+            }
+
+            let total = max(Double(selectedKeys.count), 1.0)
+            var successCount = 0
+            var failCount = 0
+            var errors: [String] = []
+
+            DispatchQueue.main.async {
+                passcodeThemeManager.isApplying = true
+                passcodeThemeManager.progress = 0
+                passcodeThemeManager.message = "preparing passcode theme..."
+            }
+
+            defer {
+                DispatchQueue.main.async {
+                    processing = false
+                    passcodeThemeManager.isApplying = false
+                }
+            }
+
+            for (index, item) in selectedKeys.enumerated() {
+                autoreleasepool {
+                    let keyId = item.key
+                    let imageData = item.value
+                    let matched = targets[keyId] ?? []
+
+                    DispatchQueue.main.async {
+                        passcodeThemeManager.progress = Double(index) / total
+                        passcodeThemeManager.message = "applying \(keyId)"
+                    }
+
+                    if matched.isEmpty {
+                        failCount += 1
+                        errors.append("no target found for \(keyId)")
+                        return
+                    }
+
+                    for path in matched {
+                        do {
+                            try passcodeThemeManager.applyImage(data: imageData, to: path)
+                            successCount += 1
+                            mgr.logmsg("applied \(keyId) -> \(path)")
+                        } catch {
+                            failCount += 1
+                            errors.append("\(path): \(error.localizedDescription)")
+                            mgr.logmsg("failed \(path): \(error.localizedDescription)")
+                        }
+                    }
+                }
+            }
+
+            DispatchQueue.main.async {
+                passcodeThemeManager.progress = 1.0
+
+                if failCount == 0 {
+                    passcodeThemeManager.message = "Done"
+                    statusMessage = "applied \(successCount) file(s)"
+                } else {
+                    passcodeThemeManager.message = "Completed with errors"
+                    statusMessage = "applied \(successCount), failed \(failCount)\n\n\(errors.joined(separator: "\n"))"
+                }
+            }
+        }
+    }
+
+    func resolveTelephonyBasePath() -> String? {
+        for version in telephonyOptions {
+            let path = "/var/mobile/Library/Caches/\(version)"
+
+            if sbxdirExists(path: path) {
+                mgr.logmsg("TelephonyUI cache: \(path)")
+                return path
+            }
+        }
+        
+        return nil
+    }
+
+    func sbxdirExists(path: String) -> Bool {
+        var isDir: ObjCBool = false
+        return FileManager.default.fileExists(
+            atPath: path,
+            isDirectory: &isDir
+        ) && isDir.boolValue
+    }
+    
+    func restoreTheme() {
+        guard mgr.sbxready else {
+            statusMessage = "Error: SBX not ready"
+            return
+        }
+        processing = true
+        statusMessage = ""
+
+        DispatchQueue.global(qos: .userInitiated).async {
+            guard let basePath = resolveTelephonyBasePath() else {
+                DispatchQueue.main.async {
+                    processing = false
+                    statusMessage = "Error: TelephonyUI cache not found"
+                }
+                return
+            }
+
+            do {
+                try passcodeThemeManager.restoreAll(basePath: basePath) { msg in
+                    mgr.logmsg(msg)
+                }
+                DispatchQueue.main.async {
+                    processing = false
+                    statusMessage = "Originals restored"
+                }
+            } catch {
+                DispatchQueue.main.async {
+                    processing = false
+                    statusMessage = "Error: \(error.localizedDescription)"
+                }
+            }
+        }
+    }
+}
+
+struct PasscodeKeyButton: View {
+    let key: PasscodeKey
+    let imageData: Data?
+    let onSelect: () -> Void
+    
+    var body: some View {
+        Button(action: onSelect) {
+            GeometryReader { geo in
+                ZStack {
+                    if let data = imageData,
+                       let uiImage = UIImage(data: data) {
+                        Image(uiImage: uiImage)
+                            .resizable()
+                            .scaledToFill()
+                            .frame(
+                                width: geo.size.width,
+                                height: geo.size.width
+                            )
+                            .clipped()
+                    } else {
+                        Rectangle()
+                            .fill(Color.gray.opacity(0.2))
+                    }
+                }
+                .frame(
+                    width: geo.size.width,
+                    height: geo.size.width
+                )
+            }
+            .aspectRatio(1, contentMode: .fit)
+        }
+        .buttonStyle(.plain)
+    }
+}
+
+extension String: @retroactive Identifiable {
+    public var id: String { self }
+}
+
+struct ImagePicker: UIViewControllerRepresentable {
+    @Binding var imageData: Data?
+    @Environment(\.dismiss) var dismiss
+
+    func makeUIViewController(
+        context: Context
+    ) -> PHPickerViewController {
+        var config = PHPickerConfiguration()
+        config.filter = .images
+        config.selectionLimit = 1
+        
+        let picker = PHPickerViewController(
+            configuration: config
+        )
+        
+        picker.delegate = context.coordinator
+        return picker
+    }
+    
+    func updateUIViewController(
+        _ uiViewController: PHPickerViewController,
+        context: Context
+    ) {}
+    
+    func makeCoordinator() -> Coordinator {
+        Coordinator(self)
+    }
+    
+    class Coordinator: NSObject, PHPickerViewControllerDelegate {
+        let parent: ImagePicker
+        init(_ parent: ImagePicker) {
+            self.parent = parent
+        }
+        
+        func picker(
+            _ picker: PHPickerViewController,
+            didFinishPicking results: [PHPickerResult]
+        ) {
+            parent.dismiss()
+            guard let result = results.first else { return }
+            result.itemProvider.loadObject( ofClass: UIImage.self ) {
+                [weak self] object, error in
+                guard let image = object as? UIImage else { return }
+                guard let self else { return }
+                let resized = self.resizeImage(
+                    image,
+                    targetHeight: 202
+                )
+                if let pngData = resized.pngData() {
+                    DispatchQueue.main.async {
+                        self.parent.imageData = pngData
+                    }
+                }
+            }
+        }
+
+        func resizeImage(
+            _ image: UIImage,
+            targetHeight: CGFloat
+        ) -> UIImage {
+            let scale = targetHeight / image.size.height
+            let newWidth = image.size.width * scale
+            let newSize = CGSize(
+                width: newWidth,
+                height: targetHeight
+            )
+            
+            let renderer = UIGraphicsImageRenderer( size: newSize )
+            return renderer.image { _ in
+                image.draw(
+                    in: CGRect(
+                        origin: .zero,
+                        size: newSize
+                    )
+                )
+            }
+        }
+    }
+}
  ```

  </details>
- 🟡 `source.json` (modified) — +0 / -20
  <details><summary>Diff</summary>

  ```diff
  @@ -29,26 +29,6 @@
         }
       ],
       "appPermissions": {}
-    },
-    {
-      "name": "lara (Nightly)",
-      "bundleIdentifier": "com.roooot.lara",
-      "developerName": "rooootdev & lara team",
-      "subtitle": "This is the nightly testing build for lara.",
-      "localizedDescription": "This is the nightly testing build for lara. Only use this if you're interested in new features and do not care about stability. lara is a customization toolbox that utilizes DarkSword. Supports iOS 16.0 - iOS 18.7.1 & iOS 26.0.x, excluding M5 and A19.\n\nFor more information, check out our GitHub! https://github.com/rooootdev/lara. Join the Discord if you'd like to report any bugs! https://discord.gg/gw8PcRF3Jr.",
-      "iconURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/lara.png",
-      "tintColor": "#5CA399",
-      "versions": [
-        {
-          "version": "0.2",
-          "date": "2026-05-5",
-          "size": 3450675,
-          "downloadURL": "https://github.com/rooootdev/lara/releases/download/nightly/lara.ipa",
-          "localizedDescription": "",
-          "minOSVersion": "16.0"
-        }
-      ],
-      "appPermissions": {}
     }
   ]
 }
  ```

  </details>
- 🟢 `source_nightly.json` (added) — +34 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,34 @@
+{
+  "name": "lara nightly",
+  "subtitle": "A customization toolbox that utilizes DarkSword.",
+  "description": "",
+  "iconURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/lara.png",
+  "headerURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/icon.png",
+  "website": "https://github.com/rooootdev/lara",
+  "tintColor": "#4185A9",
+  "featuredApps": [
+    "com.roooot.lara"
+  ],
+  "apps": [
+    {
+      "name": "lara (Nightly)",
+      "bundleIdentifier": "com.roooot.lara",
+      "developerName": "rooootdev & lara team",
+      "subtitle": "This is the nightly testing build for lara.",
+      "localizedDescription": "This is the nightly testing build for lara. Only use this if you're interested in new features and do not care about stability. lara is a customization toolbox that utilizes DarkSword. Supports iOS 16.0 - iOS 18.7.1 & iOS 26.0.x, excluding M5 and A19.\n\nFor more information, check out our GitHub! https://github.com/rooootdev/lara. Join the Discord if you'd like to report any bugs! https://discord.gg/gw8PcRF3Jr.",
+      "iconURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/lara.png",
+      "tintColor": "#5CA399",
+      "versions": [
+        {
+          "version": "0.2",
+          "date": "2026-05-5",
+          "size": 3450675,
+          "downloadURL": "https://github.com/rooootdev/lara/releases/download/nightly/lara.ipa",
+          "localizedDescription": "",
+          "minOSVersion": "16.0"
+        }
+      ],
+      "appPermissions": {}
+    }
+  ]
+}
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit f30d5c8: big changes
- May 22, 2026: ✏️ @neonmodder123 renamed: "big changes" → "Fix passcode themer"
- : 📝 Commit cda8088: Update source.json
- : 📝 Commit ef435d5: Create source_nightly.json
- : 📝 Commit c9cf9ba: Update source_nightly.json
- : 📝 Commit e7e53c0: Add nightly repo in README.md
- : 📝 Commit 552a584: Update IconThemeManager.swift
- : 📝 Commit 9dcd551: add credits
- May 22, 2026: 🟣 @lunginspector merged
- May 22, 2026: 🔴 Closed by @lunginspector


---

## Comments (1)

### Comment #1 — [@neonmodder123](https://github.com/neonmodder123) · May 22, 2026

`ID: 4514756749`


I have also added a separate nightly source for sideloaders as there was a conflict with both v0.1 and nightly having the same bundle id.


---

*Exported on 10/14/2026, 3:07:47 PM from [https://github.com/rooootdev/lara/pull/256](https://github.com/rooootdev/lara/pull/256)*
