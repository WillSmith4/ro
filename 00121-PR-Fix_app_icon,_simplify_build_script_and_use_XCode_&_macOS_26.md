# Fix app icon, simplify build script and use XCode & macOS 26 for building

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#121](https://github.com/rooootdev/lara/pull/121)  
> **State:** 🟣 merged  
> **Branch:** `neonmodder123:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 19, 2026  
> **Changes:** +163 / -334 across 21 files  
> **Created:** Apr 17, 2026  
> **Updated:** Apr 19, 2026  
> **Closed:** Apr 19, 2026  

---

## Description

*Opened by [@neonmodder123](https://github.com/neonmodder123)* *on Apr 17, 2026*


Pretty much the title, liquid glass doesn't show on devices below iOS 26 though.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24601482359/job/71940840998) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24601482359/job/71940808541) — **success** — *GitHub Actions*


---

## PR Commits (9)

- [`a717f4f`](https://github.com/rooootdev/lara/commit/a717f4f341c6bd79efe23b428942a5c804ef889f) Update build.yml — *neon* · Apr 17, 2026
- [`93eda6c`](https://github.com/rooootdev/lara/commit/93eda6cbac9cfc5147fb81fafbcbbeea3c186f0d) Merge branch 'rooootdev:main' into main — *neon* · Apr 17, 2026
- [`a0e5b36`](https://github.com/rooootdev/lara/commit/a0e5b3685c0a52c2ba0d84f1201b1d71e6120208) fix icon — *neon* · Apr 17, 2026
- [`9a7f0b2`](https://github.com/rooootdev/lara/commit/9a7f0b2b0886159e6220767a48b021d272405c18) Merge branch 'rooootdev:main' into main — *neon* · Apr 17, 2026
- [`9647c4a`](https://github.com/rooootdev/lara/commit/9647c4ab749293775597643c29866760aa831891) Update build_ipa.sh — *neon* · Apr 17, 2026
- [`fff30f6`](https://github.com/rooootdev/lara/commit/fff30f6f4078998921c4100c7e2c1a4ba11ffa61) Enhance build_ipa.sh with error handling and logging — *Pro42good* · Apr 17, 2026
- [`f0b26b2`](https://github.com/rooootdev/lara/commit/f0b26b2c1c8788dbe6a4437bd57a79ba5929cc0b) Update build workflow to install ldid and rename artifacts — *Pro42good* · Apr 17, 2026
- [`5aa4dd3`](https://github.com/rooootdev/lara/commit/5aa4dd3adcfa1becc6b581e0db4aa56937bc764e) Merge branch 'rooootdev:main' into main — *neon* · Apr 18, 2026
- [`f04af76`](https://github.com/rooootdev/lara/commit/f04af7629cda3af36104f759828c64249f106cc3) fix the build_ipa.sh — *neon* · Apr 18, 2026


---

## Changed Files (21)

- 🟡 `.github/workflows/build.yml` (modified) — +11 / -32
  <details><summary>Diff</summary>

  ```diff
  @@ -4,56 +4,41 @@ on:
     branches: [ main ]
   pull_request:
   workflow_dispatch:
-
 permissions:
   contents: write
-
 jobs:
   build:
-    runs-on: macos-14
-    strategy:
-      matrix:
-        include:
-          - arch: arm64e
-            flag: ""
-            suffix: ""
+    runs-on: macos-26
+
     steps:
       - name: checkout
         uses: actions/checkout@v4
-
       - name: setup xcode
         uses: maxim-lobanov/setup-xcode@v1
         with:
           xcode-version: latest-stable
-
       - name: show xcode version
         run: xcodebuild -version
-
+      - name: install ldid
+        run: brew install ldid
       - name: build
         run: |
           chmod +x scripts/build_ipa.sh
-          LARA_LDID_SIGN=0 ./scripts/build_ipa.sh ${{ matrix.flag }}
-
-      - name: rename ipa
-        if: matrix.suffix != ''
-        run: mv dist/lara.ipa dist/lara${{ matrix.suffix }}.ipa
-
+          ./scripts/build_ipa.sh
       - name: upload build artifact
         if: github.event_name == 'pull_request'
         uses: actions/upload-artifact@v4
         with:
-          name: lara-ipa${{ matrix.suffix }}
+          name: lara-ipa
           path: |
-            dist/lara${{ matrix.suffix }}.ipa
+            build/lara.ipa
             build/xcodebuild.log
-
       - name: upload ipa for release
         if: github.event_name != 'pull_request'
         uses: actions/upload-artifact@v4
         with:
-          name: release-ipa${{ matrix.suffix }}
-          path: dist/lara${{ matrix.suffix }}.ipa
-
+          name: release-ipa
+          path: build/lara.ipa
   release:
     if: github.event_name != 'pull_request'
     needs: build
@@ -63,20 +48,17 @@ jobs:
         uses: actions/checkout@v4
         with:
           fetch-depth: 0
-
       - name: download all ipas
         uses: actions/download-artifact@v4
         with:
           pattern: release-ipa*
           merge-multiple: true
-          path: dist/
-
+          path: build/
       - name: create release tag
         id: tag
         run: |
           TAG="build_$(date +'%Y%m%d%H%M%S')-$(git rev-parse --short HEAD)"
           echo "TAG=$TAG" >> $GITHUB_ENV
-
       - name: get last 12h commits
         id: commits
         run: |
@@ -87,7 +69,6 @@ jobs:
           echo "COMMITS<<EOF" >> $GITHUB_ENV
           echo "$COMMITS" >> $GITHUB_ENV
           echo "EOF" >> $GITHUB_ENV
-
       - name: delete old release
         uses: actions/github-script@v7
         with:
@@ -97,7 +78,6 @@ jobs:
               owner: context.repo.owner,
               repo: context.repo.repo
             });
-
             const latestRelease = releases.data.find(r => r.tag_name === 'latest');
             if (latestRelease) {
               await github.rest.repos.deleteRelease({
@@ -106,14 +86,13 @@ jobs:
                 release_id: latestRelease.id
               });
             }
-
       - name: create new release
         uses: softprops/action-gh-release@v1
         with:
           tag_name: latest
           name: Latest Build
           body: ${{ env.COMMITS }}
           files: |
-            dist/lara.ipa
+            build/lara.ipa
         env:
           GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  ```

  </details>
- 🟡 `lara.xcodeproj/project.pbxproj` (modified) — +4 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -345,7 +345,8 @@
 			isa = XCBuildConfiguration;
 			buildSettings = {
 				ARCHS = arm64e;
-				ASSETCATALOG_COMPILER_APPICON_NAME = lara;
+				ASSETCATALOG_COMPILER_APPICON_NAME = AppIcon;
+				ASSETCATALOG_COMPILER_INCLUDE_ALL_APPICON_ASSETS = YES;
 				CODE_SIGN_STYLE = Automatic;
 				CURRENT_PROJECT_VERSION = 1;
 				DEVELOPMENT_TEAM = DN4S6CXFWY;
@@ -392,7 +393,8 @@
 			isa = XCBuildConfiguration;
 			buildSettings = {
 				ARCHS = arm64e;
-				ASSETCATALOG_COMPILER_APPICON_NAME = lara;
+				ASSETCATALOG_COMPILER_APPICON_NAME = AppIcon;
+				ASSETCATALOG_COMPILER_INCLUDE_ALL_APPICON_ASSETS = YES;
 				CODE_SIGN_STYLE = Automatic;
 				CURRENT_PROJECT_VERSION = 1;
 				DEVELOPMENT_TEAM = DN4S6CXFWY;
  ```

  </details>
- 🔄 `lara/assets/AppIcon.icon/Assets/out.png` (renamed) ← `lara/assets/lara.icon/Assets/out.png` — +0 / -0
- 🟢 `lara/assets/AppIcon.icon/icon.json` (added) — +72 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,72 @@
+{
+  "fill-specializations" : [
+    {
+      "value" : {
+        "linear-gradient" : [
+          "display-p3:0.82353,0.00784,1.00000,1.00000",
+          "display-p3:0.00000,0.70196,0.63137,1.00000"
+        ],
+        "orientation" : {
+          "start" : {
+            "x" : 0.5,
+            "y" : 0
+          },
+          "stop" : {
+            "x" : 0.5,
+            "y" : 0.7
+          }
+        }
+      }
+    },
+    {
+      "appearance" : "dark",
+      "value" : {
+        "linear-gradient" : [
+          "display-p3:0.19216,0.19216,0.19216,1.00000",
+          "display-p3:0.00000,0.00000,0.00000,1.00000"
+        ],
+        "orientation" : {
+          "start" : {
+            "x" : 0.5,
+            "y" : 0
+          },
+          "stop" : {
+            "x" : 0.5,
+            "y" : 0.7
+          }
+        }
+      }
+    }
+  ],
+  "groups" : [
+    {
+      "layers" : [
+        {
+          "image-name" : "out.png",
+          "name" : "out",
+          "position" : {
+            "scale" : 0.5,
+            "translation-in-points" : [
+              0,
+              0
+            ]
+          }
+        }
+      ],
+      "shadow" : {
+        "kind" : "neutral",
+        "opacity" : 0.5
+      },
+      "translucency" : {
+        "enabled" : true,
+        "value" : 0.5
+      }
+    }
+  ],
+  "supported-platforms" : {
+    "circles" : [
+      "watchOS"
+    ],
+    "squares" : "shared"
+  }
+}
\ No newline at end of file
  ```

  </details>
- 🔴 `lara/assets/lara.icon/icon.json` (removed) — +0 / -51
  <details><summary>Diff</summary>

  ```diff
  @@ -1,51 +0,0 @@
-{
-  "fill" : {
-    "linear-gradient" : [
-      "display-p3:0.82531,0.00154,1.00000,1.00000",
-      "display-p3:0.00000,0.70583,0.63996,1.00000"
-    ],
-    "orientation" : {
-      "start" : {
-        "x" : 0.5,
-        "y" : 0
-      },
-      "stop" : {
-        "x" : 0.5,
-        "y" : 0.7
-      }
-    }
-  },
-  "groups" : [
-    {
-      "layers" : [
-        {
-          "fill" : "none",
-          "glass" : true,
-          "image-name" : "out.png",
-          "name" : "out",
-          "position" : {
-            "scale" : 0.5,
-            "translation-in-points" : [
-              0,
-              0
-            ]
-          }
-        }
-      ],
-      "shadow" : {
-        "kind" : "neutral",
-        "opacity" : 0.5
-      },
-      "translucency" : {
-        "enabled" : true,
-        "value" : 0.5
-      }
-    }
-  ],
-  "supported-platforms" : {
-    "circles" : [
-      "watchOS"
-    ],
-    "squares" : "shared"
-  }
-}
\ No newline at end of file
  ```

  </details>
- 🟢 `lara/other/media.xcassets/AppIcon.appiconset/Contents.json` (added) — +38 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,38 @@
+{
+  "images" : [
+    {
+      "filename" : "square.png",
+      "idiom" : "universal",
+      "platform" : "ios",
+      "size" : "1024x1024"
+    },
+    {
+      "appearances" : [
+        {
+          "appearance" : "luminosity",
+          "value" : "dark"
+        }
+      ],
+      "filename" : "square-dark 1.png",
+      "idiom" : "universal",
+      "platform" : "ios",
+      "size" : "1024x1024"
+    },
+    {
+      "appearances" : [
+        {
+          "appearance" : "luminosity",
+          "value" : "tinted"
+        }
+      ],
+      "filename" : "out.png",
+      "idiom" : "universal",
+      "platform" : "ios",
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
- 🟢 `lara/other/media.xcassets/AppIcon.appiconset/out.png` (added) — +0 / -0
- 🟢 `lara/other/media.xcassets/AppIcon.appiconset/square-dark 1.png` (added) — +0 / -0
- 🟢 `lara/other/media.xcassets/AppIcon.appiconset/square.png` (added) — +0 / -0
- 🔴 `lara/other/media.xcassets/lara.appiconset/Contents.json` (removed) — +0 / -68
  <details><summary>Diff</summary>

  ```diff
  @@ -1,68 +0,0 @@
-{
-  "images" : [
-    {
-      "filename" : "icon-20.png",
-      "idiom" : "iphone",
-      "scale" : "2x",
-      "size" : "20x20"
-    },
-    {
-      "filename" : "icon-20@3x.png",
-      "idiom" : "iphone",
-      "scale" : "3x",
-      "size" : "20x20"
-    },
-    {
-      "filename" : "icon-29.png",
-      "idiom" : "iphone",
-      "scale" : "1x",
-      "size" : "29x29"
-    },
-    {
-      "filename" : "icon-29@2x.png",
-      "idiom" : "iphone",
-      "scale" : "2x",
-      "size" : "29x29"
-    },
-    {
-      "filename" : "icon-29@3x.png",
-      "idiom" : "iphone",
-      "scale" : "3x",
-      "size" : "29x29"
-    },
-    {
-      "filename" : "icon-40@2x.png",
-      "idiom" : "iphone",
-      "scale" : "2x",
-      "size" : "40x40"
-    },
-    {
-      "filename" : "icon-40@3x.png",
-      "idiom" : "iphone",
-      "scale" : "3x",
-      "size" : "40x40"
-    },
-    {
-      "filename" : "icon-60@2x.png",
-      "idiom" : "iphone",
-      "scale" : "2x",
-      "size" : "60x60"
-    },
-    {
-      "filename" : "icon-60@3x.png",
-      "idiom" : "iphone",
-      "scale" : "3x",
-      "size" : "60x60"
-    },
-    {
-      "filename" : "icon-1024.png",
-      "idiom" : "ios-marketing",
-      "scale" : "1x",
-      "size" : "1024x1024"
-    }
-  ],
-  "info" : {
-    "author" : "xcode",
-    "version" : 1
-  }
-}
  ```

  </details>
- 🔴 `lara/other/media.xcassets/lara.appiconset/icon-1024.png` (removed) — +0 / -0
- 🔴 `lara/other/media.xcassets/lara.appiconset/icon-20.png` (removed) — +0 / -0
- 🔴 `lara/other/media.xcassets/lara.appiconset/icon-20@3x.png` (removed) — +0 / -0
- 🔴 `lara/other/media.xcassets/lara.appiconset/icon-29.png` (removed) — +0 / -0
- 🔴 `lara/other/media.xcassets/lara.appiconset/icon-29@2x.png` (removed) — +0 / -0
- 🔴 `lara/other/media.xcassets/lara.appiconset/icon-29@3x.png` (removed) — +0 / -0
- 🔴 `lara/other/media.xcassets/lara.appiconset/icon-40@2x.png` (removed) — +0 / -0
- 🔴 `lara/other/media.xcassets/lara.appiconset/icon-40@3x.png` (removed) — +0 / -0
- 🔴 `lara/other/media.xcassets/lara.appiconset/icon-60@2x.png` (removed) — +0 / -0
- 🔴 `lara/other/media.xcassets/lara.appiconset/icon-60@3x.png` (removed) — +0 / -0
- 🟡 `scripts/build_ipa.sh` (modified) — +38 / -181
  <details><summary>Diff</summary>

  ```diff
  @@ -1,187 +1,44 @@
-#!/usr/bin/env bash
+#!/bin/bash
 set -euo pipefail
 
-BUILD_MACOS=0
-BUILD_ARM64=0
-for arg in "$@"; do
-  case "$arg" in
-    --macos)
-      BUILD_MACOS=1
-      ;;
-    --arm64)
-      BUILD_ARM64=1
-      ;;
-    -h|--help)
-      echo "Usage: $0 [--macos] [--arm64]"
-      echo "  --macos   Build the Mac Catalyst .app into dist/"
-      echo "  --arm64   Build for arm64 instead of arm64e (no RemoteCall support)"
-      exit 0
-      ;;
-    *)
-      echo "Unknown argument: $arg" >&2
-      echo "Usage: $0 [--macos] [--arm64]" >&2
-      exit 2
-      ;;
-  esac
-done
-
-PROJECT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")/.." && pwd)"
-SCHEME="lara"
-CONFIGURATION="Debug"
-APP_NAME="lara"
-DIST_DIR="$PROJECT_DIR/dist"
-PAYLOAD_DIR="$DIST_DIR/Payload"
-IPA_PATH="$DIST_DIR/${APP_NAME}.ipa"
-DERIVED_DATA_DIR="$PROJECT_DIR/build/DerivedData"
-
-SDK="iphoneos"
-DESTINATION="generic/platform=iOS"
-PRODUCT_SUBDIR="${CONFIGURATION}-iphoneos"
-if [[ "$BUILD_MACOS" == "1" ]]; then
-  SDK="macosx"
-  DESTINATION="platform=macOS,variant=Mac Catalyst"
-  PRODUCT_SUBDIR="${CONFIGURATION}-maccatalyst"
-fi
-
-LARA_LDID_SIGN="${LARA_LDID_SIGN:-1}"
-LARA_LDID_ENTITLEMENTS="${LARA_LDID_ENTITLEMENTS:-$PROJECT_DIR/Config/lara.entitlements}"
-
-BUILD_ARCHS="arm64e"
-SWIFT_FLAGS=""
-if [[ "$BUILD_ARM64" == "1" ]]; then
-  BUILD_ARCHS="arm64"
-  SWIFT_FLAGS="SWIFT_ACTIVE_COMPILATION_CONDITIONS=DISABLE_REMOTECALL"
-fi
-
-rm -rf "$DIST_DIR" "$PROJECT_DIR/build"
-
-XCODEBUILD_LOG="$PROJECT_DIR/build/xcodebuild.log"
-mkdir -p "$(dirname "$XCODEBUILD_LOG")"
-rm -f "$XCODEBUILD_LOG"
-
-run_xcodebuild() {
-  local destination="$1"
-  xcodebuild \
-    -project "$PROJECT_DIR/lara.xcodeproj" \
-    -scheme "$SCHEME" \
-    -configuration "$CONFIGURATION" \
-    -sdk "$SDK" \
-    -destination "$destination" \
-    -derivedDataPath "$DERIVED_DATA_DIR" \
-    build \
-    CODE_SIGNING_ALLOWED=NO \
-    CODE_SIGNING_REQUIRED=NO \
-    CODE_SIGN_STYLE=Manual \
-    ASSETCATALOG_COMPILER_APPICON_NAME="" \
-    ASSETCATALOG_COMPILER_LAUNCHIMAGE_NAME="" \
-    ASSETCATALOG_COMPILER_GENERATE_SWIFT_ASSET_SYMBOL_EXTENSIONS=NO \
-    ENABLE_ON_DEMAND_RESOURCES=NO \
-    ARCHS="$BUILD_ARCHS" \
-    ONLY_ACTIVE_ARCH=NO \
-    $SWIFT_FLAGS
-}
-
-prepare_maccatalyst_linker_path_workaround() {
-  local products_dir="$DERIVED_DATA_DIR/Build/Products"
-  local intermediates_dir="$DERIVED_DATA_DIR/Build/Intermediates.noindex"
-
-  mkdir -p "$products_dir"
-  if [[ ! -e "$products_dir/Release" ]]; then
-    ln -s "Release-maccatalyst" "$products_dir/Release"
-  fi
-
-  local pkg
-  for pkg in SWCompression BitByteData; do
-    local release_dir="$intermediates_dir/${pkg}.build/Release"
-    local macabi_dir="$intermediates_dir/${pkg}.build/Release-maccatalyst"
-    mkdir -p "$release_dir"
-    if [[ -d "$macabi_dir" && ! -e "$release_dir/${pkg}.build" ]]; then
-      ln -s "../Release-maccatalyst/${pkg}.build" "$release_dir/${pkg}.build"
-    fi
-  done
-}
-
-set +e
-if [[ "$BUILD_MACOS" == "1" ]]; then
-  echo "Building with destination: $DESTINATION"
-  run_xcodebuild "$DESTINATION" 2>&1 | tee "$XCODEBUILD_LOG"
-  XCODEBUILD_STATUS=${PIPESTATUS[0]}
-  if [[ $XCODEBUILD_STATUS -ne 0 ]] && grep -q "Build input files cannot be found" "$XCODEBUILD_LOG"; then
-    echo "Retrying macOS build with linker path workaround for Swift package object products"
-    prepare_maccatalyst_linker_path_workaround
-    run_xcodebuild "$DESTINATION" 2>&1 | tee "$XCODEBUILD_LOG"
-    XCODEBUILD_STATUS=${PIPESTATUS[0]}
-  fi
-else
-  run_xcodebuild "$DESTINATION" 2>&1 | tee "$XCODEBUILD_LOG"
-  XCODEBUILD_STATUS=${PIPESTATUS[0]}
-fi
-set -e
-
-if [[ $XCODEBUILD_STATUS -ne 0 ]]; then
-  echo "ERROR: xcodebuild failed (exit $XCODEBUILD_STATUS). Log: $XCODEBUILD_LOG" >&2
-  echo "--- Last 200 lines ---" >&2
-  cat "$XCODEBUILD_LOG" >&2 || true
-  exit "$XCODEBUILD_STATUS"
-fi
-
-APP_DIR="$DERIVED_DATA_DIR/Build/Products/${PRODUCT_SUBDIR}/${APP_NAME}.app"
-if [[ ! -d "$APP_DIR" ]]; then
-  echo "ERROR: Could not find built ${APP_NAME}.app at: $APP_DIR" >&2
+rm -rf build/
+mkdir -p build
+
+echo "Build Started!"
+echo
+
+xcodebuild \
+  -project lara.xcodeproj \
+  -scheme lara \
+  -configuration Debug \
+  -sdk iphoneos \
+  -arch arm64e \
+  CODE_SIGNING_ALLOWED=NO \
+  CODE_SIGNING_REQUIRED=NO \
+  CODE_SIGN_IDENTITY="" \
+  CODE_SIGN_ENTITLEMENTS="Config/lara.entitlements" \
+  archive \
+  -archivePath "$PWD/build/lara.xcarchive" 2>&1 | xcpretty
+
+APP_PATH="$PWD/build/lara.xcarchive/Products/Applications/lara.app"
+if [ ! -d "$APP_PATH" ]; then
+  echo "Missing app at $APP_PATH"
   exit 1
 fi
-
-if [[ "$BUILD_MACOS" == "1" ]]; then
-  mkdir -p "$DIST_DIR"
-  DEST_APP="$DIST_DIR/${APP_NAME}.app"
-  rm -rf "$DEST_APP"
-  if command -v rsync >/dev/null 2>&1; then
-    rsync -aL --delete "$APP_DIR/" "$DEST_APP/"
-  else
-    mkdir -p "$DEST_APP"
-    cp -aL "$APP_DIR/." "$DEST_APP/"
-  fi
-
-  echo "Created: $DEST_APP"
-  echo "macOS app built successfully"
-  exit 0
-fi
-
-mkdir -p "$PAYLOAD_DIR"
-rm -rf "$PAYLOAD_DIR"/*
-
-DEST_APP="$PAYLOAD_DIR/${APP_NAME}.app"
-if command -v rsync >/dev/null 2>&1; then
-  rsync -aL --delete "$APP_DIR/" "$DEST_APP/"
-else
-  mkdir -p "$DEST_APP"
-  cp -aL "$APP_DIR/." "$DEST_APP/"
-fi
-
-rm -rf "$DEST_APP/_CodeSignature" "$DEST_APP/embedded.mobileprovision" || true
-
-# nsure UIFileSharingEnabled is present (xcode 26 build system drops this INFOPLIST_KEY_ setting)
-plutil -replace UIFileSharingEnabled -bool YES "$DEST_APP/Info.plist"
-
-if [[ "$LARA_LDID_SIGN" == "1" ]]; then
-  if ! command -v ldid >/dev/null 2>&1; then
-    echo "ERROR: LARA_LDID_SIGN=1 but 'ldid' is not installed. Try: brew install ldid" >&2
-    exit 1
-  fi
-  if [[ ! -f "$LARA_LDID_ENTITLEMENTS" ]]; then
-    echo "ERROR: Entitlements file not found: $LARA_LDID_ENTITLEMENTS" >&2
-    exit 1
-  fi
-  echo "Signing $APP_NAME executable with ldid entitlements: $LARA_LDID_ENTITLEMENTS"
-  ldid -S"$LARA_LDID_ENTITLEMENTS" "$DEST_APP/$APP_NAME"
+rm -rf "$PWD/build/Payload"
+mkdir -p "$PWD/build/Payload"
+cp -R "$APP_PATH" "$PWD/build/Payload/"
+# patch Info.plist
+plutil -replace UIFileSharingEnabled -bool YES "$PWD/build/Payload/lara.app/Info.plist"
+# sign with ldid + entitlements
+if ! command -v ldid >/dev/null 2>&1; then
+  echo "ERROR: ldid not installed. Install with: brew install ldid" >&2
+  exit 1
 fi
+ldid -SConfig/lara.entitlements "$PWD/build/Payload/lara.app/lara"
+(cd "$PWD/build" && /usr/bin/zip -qry lara.ipa Payload)
 
-mkdir -p "$DIST_DIR"
-rm -f "$IPA_PATH"
-(
-  cd "$DIST_DIR"
-  /usr/bin/zip -qr "$IPA_PATH" "Payload"
-)
-
-echo "Created: $IPA_PATH"
-echo "IPA built successfully"
+echo
+echo "Build Successful!"
+echo "IPA at: build/lara.ipa"
+exit 0
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit a717f4f: Update build.yml
- : 📝 Commit 93eda6c: Merge branch 'rooootdev:main' into main
- : 📝 Commit a0e5b36: fix icon
- : 📝 Commit 9a7f0b2: Merge branch 'rooootdev:main' into main
- : 📝 Commit 9647c4a: Update build_ipa.sh
- : 📝 Commit fff30f6: Enhance build_ipa.sh with error handling and logging
- : 📝 Commit f0b26b2: Update build workflow to install ldid and rename artifacts
- : 📝 Commit 5aa4dd3: Merge branch 'rooootdev:main' into main
- : 📝 Commit f04af76: fix the build_ipa.sh
- Apr 19, 2026: 🟣 @rooootdev merged
- Apr 19, 2026: 🔴 Closed by @rooootdev


---

## Comments (3)

### Comment #1 — [@Pro42good](https://github.com/Pro42good) · Apr 17, 2026

`ID: 4270461777`


What I did to the yml :
```
      - name: install ldid
        run: brew install ldid
```
macos-26 runners don't have ldid preinstalled and the new script requires it, so CI would fail without this step being added to the yml


👍 1


---

### Comment #2 — [@neonmodder123](https://github.com/neonmodder123) · Apr 17, 2026

`ID: 4270495882`


Ok thanks a lot!


👍 1 · 😄 1


---

### Comment #3 — [@TheDiamondSquidy](https://github.com/TheDiamondSquidy) · Apr 18, 2026

`ID: 4273343130`


Liquid ass


👎 3


---

*Exported on 10/14/2026, 2:56:02 PM from [https://github.com/rooootdev/lara/pull/121](https://github.com/rooootdev/lara/pull/121)*
