# epic remotecall success

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#108](https://github.com/rooootdev/lara/pull/108)  
> **State:** 🔴 closed  
> **Branch:** `Baconium:main` → `rooootdev:main`  
> **Changes:** +110 / -31 across 8 files  
> **Created:** Apr 16, 2026  
> **Updated:** Apr 16, 2026  
> **Closed:** Apr 16, 2026  

---

## Description

*Opened by [@Baconium](https://github.com/Baconium)* *on Apr 16, 2026*


ok uhhh in theory it should be fully working now (unless you're on an iPad 6th gen, iPad Pro 2nd gen 12.9 inch, or an iPad 7th gen). I also added a remotecall-less version for arm64 but idk if it works cuz I didn't test it


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (3)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24532522624/job/71719387698) — **skipped** — *GitHub Actions*
- ✅ [build (arm64, --arm64, -arm64)](https://github.com/rooootdev/lara/actions/runs/24532522624/job/71719163445) — **success** — *GitHub Actions*
- ✅ [build (arm64e)](https://github.com/rooootdev/lara/actions/runs/24532522624/job/71719163417) — **success** — *GitHub Actions*


---

## PR Commits (5)

- [`8a84da1`](https://github.com/rooootdev/lara/commit/8a84da1f9ce73f0961b1268c046b6d6a13dbde16) fix remotecall — *BaconMania* · Apr 16, 2026
- [`6b6b1ab`](https://github.com/rooootdev/lara/commit/6b6b1abc3e337422bc57e6abb3ba890918c21362) remove dist and build — *BaconMania* · Apr 16, 2026
- [`70a91c8`](https://github.com/rooootdev/lara/commit/70a91c8ec3f0c99275da34e1a93ef8acd484f527) hopefully add remotecallless version for arm64 — *BaconMania* · Apr 16, 2026
- [`86081b0`](https://github.com/rooootdev/lara/commit/86081b0728112fa6b9f99b23b6985e7ea5b0e9df) ok remotecall should be fully good now I think — *BaconMania* · Apr 16, 2026
- [`8610fea`](https://github.com/rooootdev/lara/commit/8610fea6cd0d2456120912f31f6c20b27a75372b) Merge branch 'rooootdev:main' into main — *BaconMania* · Apr 16, 2026


---

## Changed Files (8)

- 🟡 `.github/workflows/build.yml` (modified) — +43 / -9
  <details><summary>Diff</summary>

  ```diff
  @@ -11,6 +11,15 @@ permissions:
 jobs:
   build:
     runs-on: macos-14
+    strategy:
+      matrix:
+        include:
+          - arch: arm64e
+            flag: ""
+            suffix: ""
+          - arch: arm64
+            flag: "--arm64"
+            suffix: "-arm64"
     steps:
       - name: checkout
         uses: actions/checkout@v4
@@ -26,30 +35,55 @@ jobs:
       - name: build
         run: |
           chmod +x scripts/build_ipa.sh
-          LARA_LDID_SIGN=0 ./scripts/build_ipa.sh
+          LARA_LDID_SIGN=0 ./scripts/build_ipa.sh ${{ matrix.flag }}
+
+      - name: rename ipa
+        if: matrix.suffix != ''
+        run: mv dist/lara.ipa dist/lara${{ matrix.suffix }}.ipa
 
       - name: upload build artifact
         if: github.event_name == 'pull_request'
         uses: actions/upload-artifact@v4
         with:
-          name: lara-ipa
+          name: lara-ipa${{ matrix.suffix }}
           path: |
-            dist/lara.ipa
+            dist/lara${{ matrix.suffix }}.ipa
             build/xcodebuild.log
 
-      - name: create release tag
+      - name: upload ipa for release
         if: github.event_name != 'pull_request'
+        uses: actions/upload-artifact@v4
+        with:
+          name: release-ipa${{ matrix.suffix }}
+          path: dist/lara${{ matrix.suffix }}.ipa
+
+  release:
+    if: github.event_name != 'pull_request'
+    needs: build
+    runs-on: ubuntu-latest
+    steps:
+      - name: checkout
+        uses: actions/checkout@v4
+        with:
+          fetch-depth: 0
+
+      - name: download all ipas
+        uses: actions/download-artifact@v4
+        with:
+          pattern: release-ipa*
+          merge-multiple: true
+          path: dist/
+
+      - name: create release tag
         id: tag
         run: |
           TAG="build_$(date +'%Y%m%d%H%M%S')-$(git rev-parse --short HEAD)"
           echo "TAG=$TAG" >> $GITHUB_ENV
 
       - name: get last 12h commits
-        if: github.event_name != 'pull_request'
         id: commits
         run: |
           COMMITS=$(git log --since="12 hours ago" --pretty=format:"- %h %s (%an)")
-          # Use fallback text if no commits
           if [ -z "$COMMITS" ]; then
             COMMITS="No commits in the last 12 hours"
           fi
@@ -58,7 +92,6 @@ jobs:
           echo "EOF" >> $GITHUB_ENV
 
       - name: delete old release
-        if: github.event_name != 'pull_request'
         uses: actions/github-script@v7
         with:
           github-token: ${{ secrets.GITHUB_TOKEN }}
@@ -78,12 +111,13 @@ jobs:
             }
 
       - name: create new release
-        if: github.event_name != 'pull_request'
         uses: softprops/action-gh-release@v1
         with:
           tag_name: latest
           name: Latest Build
           body: ${{ env.COMMITS }}
-          files: dist/lara.ipa
+          files: |
+            dist/lara.ipa
+            dist/lara-arm64.ipa
         env:
           GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  ```

  </details>
- 🟡 `.gitignore` (modified) — +3 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -1,4 +1,6 @@
 Resources/*
 xcuserdata/
 *.xcuserstate
-.DS_Store
\ No newline at end of file
+.DS_Store
+/dist
+/build
\ No newline at end of file
  ```

  </details>
- 🟡 `lara.xcodeproj/project.pbxproj` (modified) — +4 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -226,7 +226,7 @@
 			isa = XCBuildConfiguration;
 			buildSettings = {
 				ALWAYS_SEARCH_USER_PATHS = NO;
-				ARCHS = arm64;
+				ARCHS = arm64e;
 				ASSETCATALOG_COMPILER_GENERATE_SWIFT_ASSET_SYMBOL_EXTENSIONS = YES;
 				CLANG_ANALYZER_NONNULL = YES;
 				CLANG_ANALYZER_NUMBER_OBJECT_CONVERSION = YES_AGGRESSIVE;
@@ -291,7 +291,7 @@
 			isa = XCBuildConfiguration;
 			buildSettings = {
 				ALWAYS_SEARCH_USER_PATHS = NO;
-				ARCHS = arm64;
+				ARCHS = arm64e;
 				ASSETCATALOG_COMPILER_GENERATE_SWIFT_ASSET_SYMBOL_EXTENSIONS = YES;
 				CLANG_ANALYZER_NONNULL = YES;
 				CLANG_ANALYZER_NUMBER_OBJECT_CONVERSION = YES_AGGRESSIVE;
@@ -348,7 +348,7 @@
 		CC1C8B452F71DF9C00206982 /* Debug */ = {
 			isa = XCBuildConfiguration;
 			buildSettings = {
-				ARCHS = arm64;
+				ARCHS = arm64e;
 				ASSETCATALOG_COMPILER_APPICON_NAME = lara;
 				CODE_SIGN_STYLE = Automatic;
 				CURRENT_PROJECT_VERSION = 1;
@@ -395,7 +395,7 @@
 		CC1C8B462F71DF9C00206982 /* Release */ = {
 			isa = XCBuildConfiguration;
 			buildSettings = {
-				ARCHS = arm64;
+				ARCHS = arm64e;
 				ASSETCATALOG_COMPILER_APPICON_NAME = lara;
 				CODE_SIGN_STYLE = Automatic;
 				CURRENT_PROJECT_VERSION = 1;
  ```

  </details>
- 🟡 `lara/classes/laramgr.swift` (modified) — +4 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -25,7 +25,9 @@ final class laramgr: ObservableObject {
     @Published var kaccesserror: String?
     @Published var fileopinprogress: Bool = false
     @Published var testresult: String?
+    #if !DISABLE_REMOTECALL
     @Published var remotecallrunning: Bool = false
+    #endif
     
     @Published var vfsready: Bool = false
     @Published var vfsinitlog: String = ""
@@ -453,6 +455,7 @@ final class laramgr: ObservableObject {
         return true
     }
     
+    #if !DISABLE_REMOTECALL
     func rcinit(process: String, migbypass: Bool = false, completion: ((Bool) -> Void)? = nil) {
         guard dsready, !remotecallrunning else {
             completion?(false)
@@ -513,4 +516,5 @@ final class laramgr: ObservableObject {
             padded[4], padded[5], padded[6], padded[7]
         )
     }
+    #endif
 }
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.m` (modified) — +18 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -551,10 +551,25 @@ uint64_t retry_first_thread(bool useMigFilterBypass) {
 
 // NOTE: Do not run this function while "attaching xcode" on iOS 18+, it will make device unstable.
 int init_remote_call(const char* process, bool useMigFilterBypass) {
-    
+    if (!process || process[0] == '\0') {
+        printf("[%s:%d] Invalid target process name\n", __FUNCTION__, __LINE__);
+        return -1;
+    }
+
     uint64_t procAddr = proc_find_by_name(process);
-    printf("[%s:%d] process: %s, pid: %u\n",  __FUNCTION__, __LINE__, process, kread32(procAddr + off_proc_p_pid));
+    if (!procAddr) {
+        printf("[%s:%d] Failed to resolve process '%s'\n", __FUNCTION__, __LINE__, process);
+        return -1;
+    }
+
+    uint32_t procPid = kread32(procAddr + off_proc_p_pid);
+    printf("[%s:%d] process: %s, pid: %u\n",  __FUNCTION__, __LINE__, process, procPid);
+
     g_RC_taskAddr = proc_task(procAddr);
+    if (!g_RC_taskAddr) {
+        printf("[%s:%d] Failed to resolve task for process '%s'\n", __FUNCTION__, __LINE__, process);
+        return -1;
+    }
     
     mach_port_t firstExceptionPort = create_exception_port();
     mach_port_t secondExceptionPort = create_exception_port();
@@ -726,6 +741,7 @@ int init_remote_call(const char* process, bool useMigFilterBypass) {
     printf("[%s:%d] reply_with_state done, waiting for crash...\n", __FUNCTION__, __LINE__);
     fflush(stdout);
     
+    g_RC_vmMap = task_get_vm_map(g_RC_taskAddr);
     printf("[%s:%d] vmMap: 0x%llx\n", __FUNCTION__, __LINE__, g_RC_vmMap);
     fflush(stdout);
     
  ```

  </details>
- 🟡 `lara/kexploit/utils.m` (modified) — +16 / -8
  <details><summary>Diff</summary>

  ```diff
  @@ -36,10 +36,10 @@
 #define P_DISABLE_ASLR 0x00001000
 
 uint32_t PROC_PID_OFFSET;                           // p_pid
-static const uint32_t PROC_NAME_OFFSET = 0x56c;     // p_comm
+static const uint32_t PROC_NAME_OFFSET_FALLBACK = 0x56c;     // p_comm
 static const uint32_t PROC_UID_OFFSET  = 0x30;      // p_uid
 static const uint32_t PROC_GID_OFFSET  = 0x34;      // p_gid
-static const uint32_t PROC_NEXT_OFFSET = 0x08;      // p_list le_next
+static const uint32_t PROC_NEXT_OFFSET_FALLBACK = 0x08;      // p_list le_next
 static const uint32_t PROC_PREV_OFFSET = 0x00;      // p_list le_prev
 static const uint32_t PROC_PFLAG_OFFSET = 0x454;
 static const uint32_t ARM_SS_OFFSET = 0x8;
@@ -127,6 +127,14 @@ void init_offsets(void) {
 
 static NSString *const kkernprocoffset = @"lara.kernprocoff";
 
+static inline uint32_t proc_name_offset(void) {
+    return off_proc_p_name ? off_proc_p_name : PROC_NAME_OFFSET_FALLBACK;
+}
+
+static inline uint32_t proc_next_offset(void) {
+    return off_proc_p_list_le_next ? off_proc_p_list_le_next : PROC_NEXT_OFFSET_FALLBACK;
+}
+
 static bool is_kptr(uint64_t p) {
     return (p & 0xffff000000000000ULL) == 0xffff000000000000ULL;
 }
@@ -336,7 +344,7 @@ uint64_t procbypid(pid_t targetpid) {
         if (pid == targetpid) {
 
             char name[64] = {0};
-            ds_kread(currentproc + PROC_NAME_OFFSET, name, 32);
+            ds_kread(currentproc + proc_name_offset(), name, 32);
 
             uint32_t uid = ds_kread32(currentproc + PROC_UID_OFFSET);
             uint32_t gid = ds_kread32(currentproc + PROC_GID_OFFSET);
@@ -347,7 +355,7 @@ uint64_t procbypid(pid_t targetpid) {
             return currentproc;
         }
 
-        uint64_t next = ds_kread64(currentproc + PROC_NEXT_OFFSET);
+        uint64_t next = ds_kread64(currentproc + proc_next_offset());
 
         if (!is_kptr(next) || next == currentproc) {
             printf("(utils) proc list ended at step %d\n", count);
@@ -430,7 +438,7 @@ uint64_t procbyname(const char *procname) {
         }
 
         char name[64] = {0};
-        ds_kread(currentproc + PROC_NAME_OFFSET, name, 32);
+        ds_kread(currentproc + proc_name_offset(), name, 32);
 
         if (strcmp(name, procname) == 0) {
 
@@ -440,7 +448,7 @@ uint64_t procbyname(const char *procname) {
             return procbypid(pid);
         }
 
-        uint64_t next = ds_kread64(currentproc + PROC_NEXT_OFFSET);
+        uint64_t next = ds_kread64(currentproc + proc_next_offset());
 
         if (!is_kptr(next) || next == currentproc) {
             break;
@@ -485,7 +493,7 @@ uint64_t procbyname(const char *procname) {
         if (!is_kptr(currentproc)) break;
 
         char name[33] = {0};
-        ds_kread(currentproc + PROC_NAME_OFFSET, name, 32);
+        ds_kread(currentproc + proc_name_offset(), name, 32);
         name[32] = '\0';
 
         bool match = list_all || (strcasestr(name, search) != NULL);
@@ -503,7 +511,7 @@ uint64_t procbyname(const char *procname) {
             matches++;
         }
 
-        uint64_t next = ds_kread64(currentproc + PROC_NEXT_OFFSET);
+        uint64_t next = ds_kread64(currentproc + proc_next_offset());
         if (!is_kptr(next) || next == currentproc) break;
 
         currentproc = next;
  ```

  </details>
- 🟡 `lara/views/ContentView.swift` (modified) — +4 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -365,10 +365,11 @@ struct ContentView: View {
                         }
                     }
                     
+                    #if !DISABLE_REMOTECALL
                     Section {
                         Button("Init RemoteCall") {
                             mgr.logmsg("T")
-                            mgr.rcinit(process: "springboard", migbypass: false) { success in
+                            mgr.rcinit(process: "SpringBoard", migbypass: false) { success in
                                 if success {
                                     mgr.logmsg("rc init succeeded!")
                                     let pid = mgr.rccall(name: "getpid")
@@ -388,8 +389,9 @@ struct ContentView: View {
                     } header: {
                         Text("RemoteCall")
                     } footer: {
-                        Text("Broken. Do not create issues or ask for support regarding this feature.")
+                        Text("hopefully not broken anymore, but still useless. -baconmania")
                     }
+                    #endif
                     
                     Section {
                         if mgr.dsready {
  ```

  </details>
- 🟡 `scripts/build_ipa.sh` (modified) — +18 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -2,19 +2,24 @@
 set -euo pipefail
 
 BUILD_MACOS=0
+BUILD_ARM64=0
 for arg in "$@"; do
   case "$arg" in
     --macos)
       BUILD_MACOS=1
       ;;
+    --arm64)
+      BUILD_ARM64=1
+      ;;
     -h|--help)
-      echo "Usage: $0 [--macos]"
+      echo "Usage: $0 [--macos] [--arm64]"
       echo "  --macos   Build the Mac Catalyst .app into dist/"
+      echo "  --arm64   Build for arm64 instead of arm64e (no RemoteCall support)"
       exit 0
       ;;
     *)
       echo "Unknown argument: $arg" >&2
-      echo "Usage: $0 [--macos]" >&2
+      echo "Usage: $0 [--macos] [--arm64]" >&2
       exit 2
       ;;
   esac
@@ -41,6 +46,13 @@ fi
 LARA_LDID_SIGN="${LARA_LDID_SIGN:-1}"
 LARA_LDID_ENTITLEMENTS="${LARA_LDID_ENTITLEMENTS:-$PROJECT_DIR/Config/lara.entitlements}"
 
+BUILD_ARCHS="arm64e"
+SWIFT_FLAGS=""
+if [[ "$BUILD_ARM64" == "1" ]]; then
+  BUILD_ARCHS="arm64"
+  SWIFT_FLAGS="SWIFT_ACTIVE_COMPILATION_CONDITIONS=DISABLE_REMOTECALL"
+fi
+
 rm -rf "$DIST_DIR" "$PROJECT_DIR/build"
 
 XCODEBUILD_LOG="$PROJECT_DIR/build/xcodebuild.log"
@@ -64,8 +76,9 @@ run_xcodebuild() {
     ASSETCATALOG_COMPILER_LAUNCHIMAGE_NAME="" \
     ASSETCATALOG_COMPILER_GENERATE_SWIFT_ASSET_SYMBOL_EXTENSIONS=NO \
     ENABLE_ON_DEMAND_RESOURCES=NO \
-    ARCHS=arm64 \
-    ONLY_ACTIVE_ARCH=NO
+    ARCHS="$BUILD_ARCHS" \
+    ONLY_ACTIVE_ARCH=NO \
+    $SWIFT_FLAGS
 }
 
 prepare_maccatalyst_linker_path_workaround() {
@@ -147,7 +160,7 @@ fi
 
 rm -rf "$DEST_APP/_CodeSignature" "$DEST_APP/embedded.mobileprovision" || true
 
-# Ensure UIFileSharingEnabled is present (Xcode 26 build system drops this INFOPLIST_KEY_ setting)
+# nsure UIFileSharingEnabled is present (xcode 26 build system drops this INFOPLIST_KEY_ setting)
 plutil -replace UIFileSharingEnabled -bool YES "$DEST_APP/Info.plist"
 
 if [[ "$LARA_LDID_SIGN" == "1" ]]; then
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 8a84da1: fix remotecall
- : 📝 Commit 6b6b1ab: remove dist and build
- : 📝 Commit 70a91c8: hopefully add remotecallless version for arm64
- : 📝 Commit 86081b0: ok remotecall should be fully good now I think
- : 📝 Commit 8610fea: Merge branch 'rooootdev:main' into main
- Apr 16, 2026: 🔴 Closed by @Baconium


---

## Comments (1)

### Comment #1 — [@neonmodder123](https://github.com/neonmodder123) · Apr 16, 2026

`ID: 4263225264`


very nice


---

*Exported on 10/14/2026, 2:54:58 PM from [https://github.com/rooootdev/lara/pull/108](https://github.com/rooootdev/lara/pull/108)*
