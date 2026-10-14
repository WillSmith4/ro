# small fixes

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#273](https://github.com/rooootdev/lara/pull/273)  
> **State:** 🟣 merged  
> **Branch:** `neonmodder123:v2` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Jun 1, 2026  
> **Changes:** +62 / -80 across 3 files  
> **Created:** May 28, 2026  
> **Updated:** Jun 1, 2026  
> **Closed:** Jun 1, 2026  

---

## Description

*Opened by [@neonmodder123](https://github.com/neonmodder123)* *on May 28, 2026*


fix bugs and naming in decryption and zip code from #267


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/26554515660/job/78223512762) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/26554515660/job/78223433133) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`42c5915`](https://github.com/rooootdev/lara/commit/42c59154c44bcf8c941386387de20a5be7a5ff57) small fixes — *neon* · May 28, 2026


---

## Changed Files (3)

- 🟡 `lara/classes/zipmgr.swift` (modified) — +3 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -409,7 +409,7 @@ public func createZipArchive(fromDirectory sourceURL: URL, to destinationURL: UR
 
     let resolvedSource = sourceURL.resolvingSymlinksInPath()
 
-    guard let enumerator = fm.enumerator(at: sourceURL, includingPropertiesForKeys: [.isDirectoryKey], options: [.skipsHiddenFiles]) else {
+    guard let enumerator = fm.enumerator(at: resolvedSource, includingPropertiesForKeys: [.isDirectoryKey], options: [.skipsHiddenFiles]) else {
         error = "(zip) cannot enumerate source directory"
         mgr.logmsg("\(error)")
         throw ZipError.corruptArchive("\(error)")
@@ -450,7 +450,7 @@ public func createZipArchive(fromDirectory sourceURL: URL, to destinationURL: UR
         writeLE32(lfhSignature, to: &lfh)
         writeLE16(20, to: &lfh)
         writeLE16(0, to: &lfh)
-        writeLE16(0, to: &lfh)
+        writeLE16(compressionMethod, to: &lfh)
         writeLE16(0, to: &lfh)
         writeLE16(0, to: &lfh)
         writeLE32(entry.crc32, to: &lfh)
@@ -467,7 +467,7 @@ public func createZipArchive(fromDirectory sourceURL: URL, to destinationURL: UR
         writeLE16(20, to: &cd)
         writeLE16(20, to: &cd)
         writeLE16(0, to: &cd)
-        writeLE16(0, to: &cd)
+        writeLE16(compressionMethod, to: &cd)
         writeLE16(0, to: &cd)
         writeLE16(0, to: &cd)
         writeLE32(entry.crc32, to: &cd)
  ```

  </details>
- 🟡 `lara/kexploit/decrypt.m` (modified) — +4 / -7
  <details><summary>Diff</summary>

  ```diff
  @@ -221,10 +221,9 @@ int is_encrypted(const char *bundlePath, const char *executableName) {
 
 static int find_text_segment_in_vm_map(uint64_t vmMap, uint64_t fileoff, uint64_t *out_addr) {
     __block uint64_t found_addr = 0;
-    __block BOOL done = NO;
 
     vmmapiterateentries(vmMap, ^(uint64_t start, uint64_t end, uint64_t entry, BOOL *stop) {
-        if (done) return;
+        if (found_addr) return;
         if (start == 0 || start >= 0xffffff8000000000ULL) return;
 
         uint64_t alias_offset = ds_kread64(entry + off_vm_map_entry_vme_alias);
@@ -236,7 +235,6 @@ static int find_text_segment_in_vm_map(uint64_t vmMap, uint64_t fileoff, uint64_
                 if (magic == MH_MAGIC_64 || magic == MH_MAGIC ||
                     magic == FAT_MAGIC || magic == FAT_CIGAM) {
                     found_addr = start;
-                    done = YES;
                     *stop = YES;
                 }
             }
@@ -245,8 +243,8 @@ static int find_text_segment_in_vm_map(uint64_t vmMap, uint64_t fileoff, uint64_
 
     if (found_addr == 0) {
         vmmapiterateentries(vmMap, ^(uint64_t start, uint64_t end, uint64_t entry, BOOL *stop) {
-            if (done) return;
-            if (start == 0 || start >= 0xffffff8000000000ULL) return;
+            if (found_addr) return;
+            if (start >= 0xffffff8000000000ULL) return;
 
             struct vmshmem shmem = vmmapremotepage(vmMap, start);
             if (shmem.used) {
@@ -255,7 +253,6 @@ static int find_text_segment_in_vm_map(uint64_t vmMap, uint64_t fileoff, uint64_
                 if (magic == MH_MAGIC_64 || magic == MH_MAGIC ||
                     magic == FAT_MAGIC || magic == FAT_CIGAM) {
                     found_addr = start;
-                    done = YES;
                     *stop = YES;
                 }
             }
@@ -292,7 +289,7 @@ static int find_macho_in_vm(uint64_t vmMap, uint64_t *out_base, uint64_t *out_en
     if (found == 0) {
         vmmapiterateentries(vmMap, ^(uint64_t start, uint64_t end, uint64_t entry, BOOL *stop) {
             if (found) return;
-            if (start == 0 || start >= 0xffffff8000000000ULL) return;
+            if (start >= 0xffffff8000000000ULL) return;
 
             struct vmshmem shmem = vmmapremotepage(vmMap, start);
             if (shmem.used) {
  ```

  </details>
- 🟡 `lara/views/tweaks/DecryptView.swift` (modified) — +55 / -70
  <details><summary>Diff</summary>

  ```diff
  @@ -7,35 +7,35 @@
 
 import SwiftUI
 
-struct decryptapp: Identifiable {
+struct DecryptApp: Identifiable {
     let id = UUID()
     let name: String
-    let bundleid: String
-    let bundlepath: String
+    let bundleID: String
+    let bundlePath: String
     let executable: String
     let icon: UIImage?
-    var isencrypted: decryptstatus = .unknown
+    var isEncrypted: DecryptStatus = .unknown
 }
 
-enum decryptstatus {
+enum DecryptStatus {
     case encrypted, unencrypted, unknown
 }
 
 struct DecryptView: View {
     @ObservedObject private var mgr = laramgr.shared
     @State private var query = ""
-    @State private var apps: [decryptapp] = []
+    @State private var apps: [DecryptApp] = []
     @State private var decryptingbid: String? = nil
     @State private var errormsg: String? = nil
-    @State private var pendingdecrypt: decryptapp? = nil
+    @State private var pendingdecrypt: DecryptApp? = nil
 
     private let documentspath = NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true).first ?? ""
 
-    private var filteredapps: [decryptapp] {
+    private var filteredapps: [DecryptApp] {
         let trimmed = query.trimmingCharacters(in: .whitespacesAndNewlines)
         if trimmed.isEmpty { return apps }
         let q = trimmed.lowercased()
-        return apps.filter { $0.name.lowercased().contains(q) || $0.bundleid.lowercased().contains(q) }
+        return apps.filter { $0.name.lowercased().contains(q) || $0.bundleID.lowercased().contains(q) }
     }
 
     var body: some View {
@@ -52,7 +52,7 @@ struct DecryptView: View {
                     TextField("Search", text: $query)
                         .textInputAutocapitalization(.never)
                         .autocorrectionDisabled()
-                    Button(action: loadapps) {
+                    Button(action: loadApps) {
                         Image(systemName: "arrow.clockwise")
                     }
                     .disabled(!mgr.sbxready || decryptingbid != nil)
@@ -70,8 +70,8 @@ struct DecryptView: View {
                             .foregroundColor(.secondary)
                     } else {
                         ForEach(filteredapps) { app in
-                            AppRow(app: app, isdecrypting: decryptingbid == app.bundleid) {
-                                startdecrypt(app)
+                            AppRow(app: app, isdecrypting: decryptingbid == app.bundleID) {
+                                startDecrypt(app)
                             }
                         }
                     }
@@ -89,29 +89,29 @@ struct DecryptView: View {
                     laramgr.shared.logmsg("(decrypt) \(s)")
                 }
             }
-            if mgr.sbxready { loadapps() }
+            if mgr.sbxready { loadApps() }
         }
         .onReceive(NotificationCenter.default.publisher(for: UIApplication.didBecomeActiveNotification)) { _ in
             if let app = pendingdecrypt {
                 pendingdecrypt = nil
                 let pid = find_process_pid(app.executable)
                 if pid > 0 {
-                    dodecrypt(app, pid: pid)
+                    doDecrypt(app, pid: pid)
                 } else {
                     errormsg = "App is not running, try again."
                     decryptingbid = nil
                 }
             }
         }
         .onChange(of: mgr.sbxready) { ready in
-            if ready { loadapps() } else { apps.removeAll() }
+            if ready { loadApps() } else { apps.removeAll() }
         }
     }
 
-    func loadapps() {
+    func loadApps() {
         guard mgr.sbxready else { return }
         DispatchQueue.global(qos: .userInitiated).async {
-            var results: [decryptapp] = []
+            var results: [DecryptApp] = []
             let bundleFolder = "/private/var/containers/Bundle/Application"
 
             guard let bundles = try? FileManager.default.contentsOfDirectory(atPath: bundleFolder) else {
@@ -147,16 +147,16 @@ struct DecryptView: View {
                     }
 
                     let encResult = is_encrypted(fullAppPath, executable)
-                    let status: decryptstatus
+                    let status: DecryptStatus
                     if encResult > 0 { status = .encrypted }
                     else if encResult == 0 { status = .unencrypted }
                     else { status = .unknown }
 
-                    results.append(decryptapp(
-                        name: name, bundleid: bundleid, bundlepath: fullAppPath,
+                    results.append(DecryptApp(
+                        name: name, bundleID: bundleid, bundlePath: fullAppPath,
                         executable: executable,
                         icon: icon ?? UIImage(named: "unknown"),
-                        isencrypted: status
+                        isEncrypted: status
                     ))
                     break
                 }
@@ -167,12 +167,12 @@ struct DecryptView: View {
         }
     }
 
-    func startdecrypt(_ app: decryptapp) {
+    func startDecrypt(_ app: DecryptApp) {
         guard decryptingbid == nil && pendingdecrypt == nil else { return }
         guard mgr.dsready else { errormsg = "Darksword not ready, run the exploit first."; return }
         guard mgr.sbxready else { errormsg = "Sandbox not escaped."; return }
 
-        let appurl = URL(fileURLWithPath: app.bundlepath)
+        let appurl = URL(fileURLWithPath: app.bundlePath)
         var total: Int64 = 0
         if let enumerator = FileManager.default.enumerator(at: appurl, includingPropertiesForKeys: [.fileSizeKey]) {
             for case let furi as URL in enumerator {
@@ -186,20 +186,20 @@ struct DecryptView: View {
                 title: "Large App",
                 body: "This app is over 200 MB, decryption may take a while.",
                 actionLabel: "Continue",
-                action: { self.rundecrypt(app) }
+                action: { self.runDecrypt(app) }
             )
             return
         }
-        rundecrypt(app)
+        runDecrypt(app)
     }
 
-    func rundecrypt(_ app: decryptapp) {
+    func runDecrypt(_ app: DecryptApp) {
         errormsg = nil
-        decryptingbid = app.bundleid
+        decryptingbid = app.bundleID
 
         let pid = find_process_pid(app.executable)
         if pid > 0 {
-            dodecrypt(app, pid: pid)
+            doDecrypt(app, pid: pid)
         } else {
             pendingdecrypt = app
 
@@ -208,7 +208,7 @@ struct DecryptView: View {
                 UIApplication.shared.endBackgroundTask(bgTask)
             }
 
-            let ret = launch_app(app.bundleid)
+            let ret = launch_app(app.bundleID)
             guard ret == 0 else {
                 UIApplication.shared.endBackgroundTask(bgTask)
                 pendingdecrypt = nil
@@ -226,7 +226,7 @@ struct DecryptView: View {
                     let foundPid = find_process_pid(app.executable)
                     if foundPid > 0 {
                         DispatchQueue.main.async {
-                            self.dodecrypt(app, pid: foundPid)
+                            self.doDecrypt(app, pid: foundPid)
                         }
                     } else {
                         DispatchQueue.main.async {
@@ -243,39 +243,49 @@ struct DecryptView: View {
         }
     }
 
-    func dodecrypt(_ app: decryptapp, pid: pid_t) {
-        let appName = (app.bundlepath as NSString).lastPathComponent
+    func doDecrypt(_ app: DecryptApp, pid: pid_t) {
+        let appName = (app.bundlePath as NSString).lastPathComponent
         let ipaName = app.name
             .replacingOccurrences(of: " ", with: "")
             .replacingOccurrences(of: "\t", with: "")
             .appending(".ipa")
-        let workDir = documentspath + "/Decrypted/.tmp_" + app.bundleid
+        let workDir = documentspath + "/Decrypted/.tmp_" + app.bundleID
         let destAppPath = workDir + "/" + appName
         let payloadDir = workDir + "/Payload"
         let payloadAppPath = payloadDir + "/" + appName
         let ipaPath = documentspath + "/Decrypted/" + ipaName
 
-        laramgr.shared.logmsg("(decrypt) decrypting \(app.bundleid)...")
+        laramgr.shared.logmsg("(decrypt) decrypting \(app.bundleID)...")
 
         DispatchQueue.global(qos: .userInitiated).async {
             let fm = FileManager.default
 
             try? fm.removeItem(atPath: workDir)
             try? fm.createDirectory(atPath: payloadDir, withIntermediateDirectories: true)
 
-            cpdir(app.bundlepath, destAppPath)
-            var dstSt = stat()
-            guard stat(destAppPath, &dstSt) == 0 else {
+            guard let enumerator = fm.enumerator(atPath: app.bundlePath) else {
                 DispatchQueue.main.async {
                     decryptingbid = nil
-                    errormsg = "Failed to copy bundle, some files may be inaccessible."
-                    laramgr.shared.logmsg("(decrypt) copy failed: could not copy \(app.bundlepath)")
+                    errormsg = "Failed to enumerate bundle for copy"
+                    laramgr.shared.logmsg("(decrypt) cannot enumerate \(app.bundlePath)")
                 }
                 return
             }
 
+            for case let subpath as String in enumerator {
+                let src = (app.bundlePath as NSString).appendingPathComponent(subpath)
+                let dst = (destAppPath as NSString).appendingPathComponent(subpath)
+                var isDir: ObjCBool = false
+                guard fm.fileExists(atPath: src, isDirectory: &isDir) else { continue }
+                if isDir.boolValue {
+                    try? fm.createDirectory(atPath: dst, withIntermediateDirectories: true)
+                } else {
+                    try? fm.copyItem(atPath: src, toPath: dst)
+                }
+            }
+
             let mainBinary = destAppPath + "/" + app.executable
-            let mainRet = decrypt_binary_pid((app.bundlepath + "/" + app.executable), pid, mainBinary)
+            let mainRet = decrypt_binary_pid((app.bundlePath + "/" + app.executable), pid, mainBinary)
             guard mainRet == 0 else {
                 DispatchQueue.main.async {
                     decryptingbid = nil
@@ -285,7 +295,7 @@ struct DecryptView: View {
                 return
             }
 
-            let srcFrameworks = app.bundlepath + "/Frameworks"
+            let srcFrameworks = app.bundlePath + "/Frameworks"
             var srcFwSt = stat()
             if stat(srcFrameworks, &srcFwSt) == 0 {
                 let frameworksPath = destAppPath + "/Frameworks"
@@ -342,7 +352,7 @@ struct DecryptView: View {
 }
 
 struct AppRow: View {
-    let app: decryptapp
+    let app: DecryptApp
     let isdecrypting: Bool
     let ondecrypt: () -> Void
 
@@ -360,9 +370,9 @@ struct AppRow: View {
 
             VStack(alignment: .leading, spacing: 2) {
                 Text(app.name).font(.headline)
-                Text(app.bundleid).font(.caption).foregroundColor(.gray)
+                Text(app.bundleID).font(.caption).foregroundColor(.gray)
                 HStack(spacing: 4) {
-                    switch app.isencrypted {
+                    switch app.isEncrypted {
                     case .encrypted:
                         Text("Encrypted").font(.caption2).foregroundStyle(.orange)
                     case .unencrypted:
@@ -375,7 +385,7 @@ struct AppRow: View {
 
             Spacer()
 
-            switch app.isencrypted {
+            switch app.isEncrypted {
             case .encrypted:
                 Button(action: ondecrypt) {
                     if isdecrypting { ProgressView() }
@@ -396,29 +406,4 @@ struct AppRow: View {
     }
 }
 
-func cpdir(_ src: String, _ dst: String) {
-    var isDir = ObjCBool(false)
-    let exists = FileManager.default.fileExists(atPath: src, isDirectory: &isDir)
-
-    if exists, !isDir.boolValue {
-        guard let handle = try? FileHandle(forReadingFrom: URL(fileURLWithPath: src)) else { return }
-        defer { try? handle.close() }
-        let data: Data
-        if #available(iOS 13.4, *) {
-            guard let d = try? handle.readToEnd() else { return }
-            data = d
-        } else {
-            data = handle.readDataToEndOfFile()
-        }
-        try? data.write(to: URL(fileURLWithPath: dst), options: .atomic)
-        return
-    }
-
-    guard exists, isDir.boolValue else { return }
-    try? FileManager.default.createDirectory(atPath: dst, withIntermediateDirectories: true)
 
-    guard let items = try? FileManager.default.contentsOfDirectory(atPath: src) else { return }
-    for item in items {
-        cpdir(src + "/" + item, dst + "/" + item)
-    }
-}
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 42c5915: small fixes
- Jun 1, 2026: 🟣 @rooootdev merged
- Jun 1, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:09:10 PM from [https://github.com/rooootdev/lara/pull/273](https://github.com/rooootdev/lara/pull/273)*
