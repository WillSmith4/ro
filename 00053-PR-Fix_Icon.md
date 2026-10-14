# Fix Icon

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#53](https://github.com/rooootdev/lara/pull/53)  
> **State:** 🟣 merged  
> **Branch:** `XtraCube:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 8, 2026  
> **Changes:** +70 / -7 across 13 files  
> **Created:** Apr 7, 2026  
> **Updated:** Apr 8, 2026  
> **Closed:** Apr 8, 2026  

---

## Description

*Opened by [@XtraCube](https://github.com/XtraCube)* *on Apr 7, 2026*


this is better than manually updating if ur releasing with actions anyways


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (4)

- [`a2e6648`](https://github.com/rooootdev/lara/commit/a2e66481d50b07bd9c4ff0ccd832b070f10948e6) use CI number for versioning — *XtraCube* · Apr 8, 2026
- [`532a8f5`](https://github.com/rooootdev/lara/commit/532a8f57547b89493d5a36e180da73632ef88774) fix icon — *xtracube* · Apr 8, 2026
- [`01880f8`](https://github.com/rooootdev/lara/commit/01880f85adbe2c9dfe294af1edd196adcfead65e) actually fix icon? — *xtracube* · Apr 8, 2026
- [`62b540d`](https://github.com/rooootdev/lara/commit/62b540d123a229da527a04ee0b287d98361e94ef) Revert "use CI number for versioning" — *XtraCube* · Apr 8, 2026


---

## Changed Files (13)

- 🟡 `lara.xcodeproj/project.pbxproj` (modified) — +2 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -37,18 +37,15 @@
 		CC6842DF2F8311C300B08EB0 /* libSystem.B.tbd */ = {isa = PBXFileReference; lastKnownFileType = "sourcecode.text-based-dylib-definition"; name = libSystem.B.tbd; path = usr/lib/libSystem.B.tbd; sourceTree = SDKROOT; };
 /* End PBXFileReference section */
 
-/* Begin PBXFileSystemSynchronizedBuildFileExceptionSet section */
+	/* Begin PBXFileSystemSynchronizedBuildFileExceptionSet section */
 		CC70C4A12F78D4C10052726B /* Exceptions for "lara" folder in "lara" target */ = {
 			isa = PBXFileSystemSynchronizedBuildFileExceptionSet;
 			membershipExceptions = (
 				Info.plist,
-				other/media.xcassets,
 			);
 			target = CC1C8B382F71DF9C00206982 /* lara */;
 		};
-/* End PBXFileSystemSynchronizedBuildFileExceptionSet section */
-
-/* Begin PBXFileSystemSynchronizedRootGroup section */
+	/* End PBXFileSystemSynchronizedBuildFileExceptionSet section *//* Begin PBXFileSystemSynchronizedRootGroup section */
 		CC1C8B3B2F71DF9C00206982 /* lara */ = {
 			isa = PBXFileSystemSynchronizedRootGroup;
 			exceptions = (
  ```

  </details>
- 🟡 `lara/Info.plist` (modified) — +0 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -2,8 +2,6 @@
 <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
 <plist version="1.0">
 <dict>
-	<key>CFBundleIconFile</key>
-	<string>assets/lara.icon</string>
 	<key>UIBackgroundModes</key>
 	<array>
 		<string>audio</string>
  ```

  </details>
- 🟢 `lara/other/media.xcassets/lara.appiconset/Contents.json` (added) — +68 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,68 @@
+{
+  "images" : [
+    {
+      "filename" : "icon-20.png",
+      "idiom" : "iphone",
+      "scale" : "2x",
+      "size" : "20x20"
+    },
+    {
+      "filename" : "icon-20@3x.png",
+      "idiom" : "iphone",
+      "scale" : "3x",
+      "size" : "20x20"
+    },
+    {
+      "filename" : "icon-29.png",
+      "idiom" : "iphone",
+      "scale" : "1x",
+      "size" : "29x29"
+    },
+    {
+      "filename" : "icon-29@2x.png",
+      "idiom" : "iphone",
+      "scale" : "2x",
+      "size" : "29x29"
+    },
+    {
+      "filename" : "icon-29@3x.png",
+      "idiom" : "iphone",
+      "scale" : "3x",
+      "size" : "29x29"
+    },
+    {
+      "filename" : "icon-40@2x.png",
+      "idiom" : "iphone",
+      "scale" : "2x",
+      "size" : "40x40"
+    },
+    {
+      "filename" : "icon-40@3x.png",
+      "idiom" : "iphone",
+      "scale" : "3x",
+      "size" : "40x40"
+    },
+    {
+      "filename" : "icon-60@2x.png",
+      "idiom" : "iphone",
+      "scale" : "2x",
+      "size" : "60x60"
+    },
+    {
+      "filename" : "icon-60@3x.png",
+      "idiom" : "iphone",
+      "scale" : "3x",
+      "size" : "60x60"
+    },
+    {
+      "filename" : "icon-1024.png",
+      "idiom" : "ios-marketing",
+      "scale" : "1x",
+      "size" : "1024x1024"
+    }
+  ],
+  "info" : {
+    "author" : "xcode",
+    "version" : 1
+  }
+}
  ```

  </details>
- 🟢 `lara/other/media.xcassets/lara.appiconset/icon-1024.png` (added) — +0 / -0
- 🟢 `lara/other/media.xcassets/lara.appiconset/icon-20.png` (added) — +0 / -0
- 🟢 `lara/other/media.xcassets/lara.appiconset/icon-20@3x.png` (added) — +0 / -0
- 🟢 `lara/other/media.xcassets/lara.appiconset/icon-29.png` (added) — +0 / -0
- 🟢 `lara/other/media.xcassets/lara.appiconset/icon-29@2x.png` (added) — +0 / -0
- 🟢 `lara/other/media.xcassets/lara.appiconset/icon-29@3x.png` (added) — +0 / -0
- 🟢 `lara/other/media.xcassets/lara.appiconset/icon-40@2x.png` (added) — +0 / -0
- 🟢 `lara/other/media.xcassets/lara.appiconset/icon-40@3x.png` (added) — +0 / -0
- 🟢 `lara/other/media.xcassets/lara.appiconset/icon-60@2x.png` (added) — +0 / -0
- 🟢 `lara/other/media.xcassets/lara.appiconset/icon-60@3x.png` (added) — +0 / -0


---

## References

### Commits (1)

- [0e78b82](https://github.com/danosito/lara-m-chips-fix/commit/0e78b82bae33bf961c769a01c0d171a39c9ed124) — Merge pull request #53 from XtraCube/main


---

## Timeline (Verbose)

- Apr 7, 2026: ⚪ @XtraCube converted to draft
- Apr 8, 2026: ⬆️ @XtraCube force-pushed
- : 📝 Commit a2e6648: use CI number for versioning
- Apr 8, 2026: ⬆️ @XtraCube force-pushed
- : 📝 Commit 532a8f5: fix icon
- : 📝 Commit 01880f8: actually fix icon?
- Apr 8, 2026: ✏️ @XtraCube renamed: "Use CI number for versioning so we know what version we r using" → "Fix Icon + use CI number for versioning"
- Apr 8, 2026: 🟢 @XtraCube marked ready for review
- : 📝 Commit 62b540d: Revert "use CI number for versioning"
- Apr 8, 2026: ✏️ @XtraCube renamed: "Fix Icon + use CI number for versioning" → "Fix Icon"
- Apr 8, 2026: 🟣 @rooootdev merged
- Apr 8, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit 0e78b82 — Merge pull request #53 from XtraCube/main


---

## Comments (6)

### Comment #1 — [@XtraCube](https://github.com/XtraCube) · Apr 7, 2026

`ID: 4202321075`


wtff why are the commits not attributed to me


---

### Comment #2 — [@XtraCube](https://github.com/XtraCube) · Apr 7, 2026

`ID: 4202321496`


whos this random guy :sob:


---

### Comment #3 — [@Pro42good](https://github.com/Pro42good) · Apr 8, 2026

`ID: 4203066405`


Hey, Just going to say, Every time a new version comes out and the app version is a mismatch from the hardcoded source.json altstore install feature in the readme it breaks the install, I fixed this issue earlier, so could we kinda not mess with the version number by editing the PR?


---

### Comment #4 — [@XtraCube](https://github.com/XtraCube) · Apr 8, 2026

`ID: 4206424226`


ohh, so you need the source.json to match as well? I'll adjust the PR to generate a source.json and put it in the release so it can be queried via: `https://github.com/rooootdev/lara/releases/download/latest/source.json`.

@Pro42good would that be sufficient?


👍 1


---

### Comment #5 — [@Pro42good](https://github.com/Pro42good) · Apr 8, 2026

`ID: 4207692941`


Yeah, that would be more than sufficient to fix the issue.


---

### Comment #6 — [@XtraCube](https://github.com/XtraCube) · Apr 8, 2026

`ID: 4209222819`


I talked to roooot and decided to hold off on versioning changes for now


👍 1


---

*Exported on 10/14/2026, 2:50:27 PM from [https://github.com/rooootdev/lara/pull/53](https://github.com/rooootdev/lara/pull/53)*
