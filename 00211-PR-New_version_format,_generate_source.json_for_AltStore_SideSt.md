# New version format, generate source.json for AltStore/SideStore updates

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#211](https://github.com/rooootdev/lara/pull/211)  
> **State:** 🔴 closed  
> **Branch:** `thongass000:main` → `rooootdev:main`  
> **Changes:** +215 / -6 across 3 files  
> **Created:** Apr 28, 2026  
> **Updated:** May 9, 2026  
> **Closed:** May 9, 2026  

---

## Description

*Opened by [@thongass000](https://github.com/thongass000)* *on Apr 28, 2026*


Changes in this request:
    - Add new versioning using date, run number, and commit hash (example: 1.2-20260429.10-a1b2c3)
    - Generate source.json with Python for update through AltStore/SideStore
    - Add release notes to source.json and GitHub releases
    - Update README to use new source.json


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25107983771/job/73574304352) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25107983771/job/73574053380) — **success** — *GitHub Actions*


---

## PR Commits (3)

- [`3fd3b4c`](https://github.com/rooootdev/lara/commit/3fd3b4c9b9250189581b76196cbfb96d73f49711) New version format, generate source.json for AltStore/SideStore updates — *James Nguyen* · Apr 28, 2026
- [`ead7eec`](https://github.com/rooootdev/lara/commit/ead7eecaabe5569e8bc2281cb809dc320fcb08a7) Add IPA size calculation to AltStore/SideStore source — *James Nguyen* · Apr 29, 2026
- [`2d7215b`](https://github.com/rooootdev/lara/commit/2d7215b27d19ffda45ff61a442c23e2fe343c803) format — *James Nguyen* · Apr 29, 2026


---

## Changed Files (3)

- 🟡 `.github/workflows/build.yml` (modified) — +73 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -9,10 +9,15 @@ permissions:
 jobs:
   build:
     runs-on: macos-26
-
+    outputs:
+      version: ${{ steps.meta.outputs.version }}
+      iso_date: ${{ steps.meta.outputs.iso_date }}
+      timestamp: ${{ steps.meta.outputs.timestamp }}
     steps:
       - name: checkout
         uses: actions/checkout@v4
+        with:
+          fetch-depth: 0
       - name: setup xcode
         uses: maxim-lobanov/setup-xcode@v1
         with:
@@ -21,10 +26,50 @@ jobs:
         run: xcodebuild -version
       - name: install ldid
         run: brew install ldid
+      - name: generate version + timestamp
+        id: meta
+        run: |
+          BASE_VERSION=$(xcodebuild -showBuildSettings | grep MARKETING_VERSION | head -n1 | awk '{print $3}')
+          TIMESTAMP=$(date -u +"%Y%m%d%H%M%S")
+          DATE=$(date -u +"%Y%m%d")
+          ISO_DATE=$(date -u +"%Y-%m-%dT%H:%M:%SZ")
+          SHA=$(git rev-parse --short HEAD)
+          RUN_NUMBER=${{ github.run_number }}
+          VERSION="${BASE_VERSION}-${DATE}.${RUN_NUMBER}-${SHA}"
+          echo "version=$VERSION" >> $GITHUB_OUTPUT
+          echo "iso_date=$ISO_DATE" >> $GITHUB_OUTPUT
+          echo "timestamp=$TIMESTAMP" >> $GITHUB_OUTPUT
       - name: build
         run: |
           chmod +x scripts/build_ipa.sh
           ./scripts/build_ipa.sh
+      - name: patch version inside IPA
+        run: |
+          VERSION="${{ steps.meta.outputs.version }}"
+          cd build
+          unzip -q lara.ipa -d extracted
+          APP_PLIST="extracted/Payload/lara.app/Info.plist"
+          echo "Patching version to $VERSION"
+          /usr/libexec/PlistBuddy -c "Set :CFBundleShortVersionString $VERSION" "$APP_PLIST"
+          /usr/libexec/PlistBuddy -c "Set :CFBundleVersion $VERSION" "$APP_PLIST"
+          echo "Repacking IPA..."
+          rm -f lara.ipa
+          cd extracted
+          zip -qry ../lara.ipa Payload
+          cd ..
+          rm -rf extracted
+      - name: get IPA size
+        id: ipa_size
+        run: |
+          SIZE=$(stat -f%z build/lara.ipa)
+          echo "size=$SIZE" >> $GITHUB_OUTPUT
+      - name: generate source.json + release notes
+        run: |
+          python3 scripts/generate_source.py \
+            "${{ steps.meta.outputs.version }}" \
+            "${{ steps.meta.outputs.iso_date }}" \
+            "${{ github.repository }}" \
+            "${{ steps.ipa_size.outputs.size }}"
       - name: upload build artifact
         if: github.event_name == 'pull_request'
         uses: actions/upload-artifact@v4
@@ -33,16 +78,22 @@ jobs:
           path: |
             build/lara.ipa
             build/xcodebuild.log
-      - name: upload ipa for release
+      - name: upload artifact for release
         if: github.event_name != 'pull_request'
         uses: actions/upload-artifact@v4
         with:
           name: release-ipa
-          path: build/lara.ipa
+          path: |
+            build/lara.ipa
+            build/source.json
   release:
     if: github.event_name != 'pull_request'
     needs: build
     runs-on: ubuntu-latest
+    env:
+      VERSION: ${{ needs.build.outputs.version }}
+      ISO_DATE: ${{ needs.build.outputs.iso_date }}
+      TIMESTAMP: ${{ needs.build.outputs.timestamp }}
     steps:
       - name: checkout
         uses: actions/checkout@v4
@@ -57,7 +108,7 @@ jobs:
       - name: create release tag
         id: tag
         run: |
-          TAG="build_$(date +'%Y%m%d%H%M%S')-$(git rev-parse --short HEAD)"
+          TAG="build_${TIMESTAMP}-$(git rev-parse --short HEAD)"
           echo "TAG=$TAG" >> $GITHUB_ENV
       - name: get last 12h commits
         id: commits
@@ -86,13 +137,30 @@ jobs:
                 release_id: latestRelease.id
               });
             }
+      - name: format release body
+        run: |
+          BUILT_AT_UTC=$(date -u -d "${ISO_DATE}" +"%a %b %d %H:%M:%S %Y")
+          BUILT_AT_DATE=$(date -u -d "${ISO_DATE}" +"%Y-%m-%d")
+          SHA=$(git rev-parse HEAD)
+          BODY="## Build Info
+          Built at (UTC): \`$BUILT_AT_UTC\`
+          Built at (UTC date): \`$BUILT_AT_DATE\`
+          Commit SHA: $SHA
+          Version: \`${VERSION}\`
+          
+          **Changes in the last 12 hours**
+          $COMMITS"
+          echo "RELEASE_BODY<<EOF" >> $GITHUB_ENV
+          echo "$BODY" >> $GITHUB_ENV
+          echo "EOF" >> $GITHUB_ENV
       - name: create new release
         uses: softprops/action-gh-release@v1
         with:
           tag_name: latest
           name: Latest Build
-          body: ${{ env.COMMITS }}
+          body: ${{ env.RELEASE_BODY }}
           files: |
             build/lara.ipa
+            build/source.json
         env:
           GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  ```

  </details>
- 🟡 `README.md` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -138,7 +138,7 @@ if things get weird later, **Delete Kernelcache Data** in Settings wipes the cac
 ## installation
 
 <p align="center">
-  <a href="https://celloserenity.github.io/altdirect/?url=https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/source.json" target="_blank">
+  <a href="https://celloserenity.github.io/altdirect/?url=https://github.com/rooootdev/lara/releases/download/latest/source.json" target="_blank">
     <img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/AltSource_Blue.png?raw=true" alt="Add AltSource" width="200">
   </a>
   <a href="https://github.com/rooootdev/lara/releases/download/latest/lara.ipa" target="_blank">
  ```

  </details>
- 🟢 `scripts/generate_source.py` (added) — +141 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,141 @@
+#!/usr/bin/env python3
+
+import json
+import re
+import sys
+import subprocess
+from pathlib import Path
+from urllib.request import urlopen
+from datetime import datetime, timezone
+
+README_URL = "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/README.md"
+AVATAR_URL = "https://avatars.githubusercontent.com/u/103732419?v=4"
+HEADER_URL = "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/icon.png"
+APP_ICON_URL = "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/lara.png"
+
+def run(cmd):
+	return subprocess.check_output(cmd, text=True).strip()
+
+def fetch_description():
+	with urlopen(README_URL) as response:
+		text = response.read().decode("utf-8")
+	marker = text.find("---")
+	if marker == -1:
+		raise ValueError('"---" marker not found in README.md')
+	text = text[marker + 3:]
+	text = re.sub(r"<[^>]+>", "", text)
+	text = "\n".join(line.rstrip() for line in text.splitlines())
+	text = text.strip()
+	text = re.sub(r"\n+", "\n", text)
+	return text
+
+def get_recent_commits():
+	try:
+		commits = run([
+			"git",
+			"log",
+			"--since=12 hours ago",
+			"--pretty=format:- %h %s (%an)"
+		])
+		return commits or "No commits in the last 12 hours"
+	except Exception:
+		return "No commits in the last 12 hours"
+
+def generate_release_notes(version, iso_date):
+	sha = run(["git", "rev-parse", "HEAD"])
+	dt = datetime.fromisoformat(
+		iso_date.replace("Z", "+00:00")
+	).astimezone(timezone.utc)
+	built_at_utc = dt.strftime("%a %b %d %H:%M:%S %Y")
+	built_at_date = dt.strftime("%Y-%m-%d")
+	commits = get_recent_commits()
+	text = f"""## Build Info
+
+Built at (UTC): {built_at_utc}
+Built at (UTC date): {built_at_date}
+Commit SHA: {sha}
+Version: {version}
+
+**Changes in the last 12 hours**
+
+{commits}
+"""
+	text = text.strip()
+	text = re.sub(r"\n+", "\n", text)
+	return text
+
+def build_source(version, iso_date, repo, size):
+	download_url = f"https://github.com/{repo}/releases/download/latest/lara.ipa"
+	release_url = f"https://github.com/{repo}/releases/tag/latest"
+	description = fetch_description()
+	release_notes = generate_release_notes(version, iso_date)
+	return {
+		"name": "lara",
+		"subtitle": "WIP darksword kexploit implement",
+		"description": "",
+		"iconURL": AVATAR_URL,
+		"headerURL": HEADER_URL,
+		"website": f"https://github.com/{repo}",
+		"tintColor": "#4185A9",
+		"featuredApps": [
+			"com.roooot.lara"
+		],
+		"news": [
+			{
+				"appID": "com.roooot.lara",
+				"caption": "Update of lara just got released!",
+				"date": iso_date,
+				"identifier": f"release-{version}",
+				"imageURL": APP_ICON_URL,
+				"notify": True,
+				"tintColor": "#4185A9",
+				"title": f"{version} - lara",
+				"url": release_url
+			}
+		],
+		"apps": [
+			{
+				"name": "lara",
+				"bundleIdentifier": "com.roooot.lara",
+				"developerName": "rooootdev",
+				"subtitle": "iOS 18.7.1- & 26.0.1-",
+				"localizedDescription": description,
+				"iconURL": APP_ICON_URL,
+				"tintColor": "#5CA399",
+				"versions": [
+					{
+						"version": version,
+						"date": iso_date,
+						"size": size,
+						"downloadURL": download_url,
+						"localizedDescription": release_notes,
+						"minOSVersion": "15.0"
+					}
+				],
+				"appPermissions": {}
+			}
+		]
+	}
+
+
+def main():
+	if len(sys.argv) != 5:
+		print(
+			"usage: generate_source.py <version> <iso_date> <repo> <size>",
+			file=sys.stderr
+		)
+		sys.exit(1)
+
+	version = sys.argv[1]
+	iso_date = sys.argv[2]
+	repo = sys.argv[3]
+	size = int(sys.argv[4])
+
+	data = build_source(version, iso_date, repo, size)
+	Path("build").mkdir(exist_ok=True)
+	with open("build/source.json", "w", encoding="utf-8") as f:
+		json.dump(data, f, indent=2, ensure_ascii=False)
+	print("generated build/source.json")
+
+if __name__ == "__main__":
+	main()
\ No newline at end of file
  ```

  </details>


---

## Timeline (Verbose)

- Apr 28, 2026: ⬆️ @thongass000 force-pushed
- : 📝 Commit 3fd3b4c: New version format, generate source.json for AltStore/SideStore updates
- : 📝 Commit ead7eec: Add IPA size calculation to AltStore/SideStore source
- Apr 29, 2026: ⬆️ @thongass000 force-pushed
- : 📝 Commit 2d7215b: format
- May 9, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:04:04 PM from [https://github.com/rooootdev/lara/pull/211](https://github.com/rooootdev/lara/pull/211)*
