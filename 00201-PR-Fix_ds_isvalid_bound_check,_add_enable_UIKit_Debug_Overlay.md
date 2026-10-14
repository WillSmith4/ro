# Fix ds_isvalid bound check, add enable UIKit Debug Overlay

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#201](https://github.com/rooootdev/lara/pull/201)  
> **State:** 🟣 merged  
> **Branch:** `khanhduytran0:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 27, 2026  
> **Changes:** +105 / -26 across 8 files  
> **Created:** Apr 27, 2026  
> **Updated:** Apr 27, 2026  
> **Closed:** Apr 27, 2026  

---

## Description

*Opened by [@khanhduytran0](https://github.com/khanhduytran0)* *on Apr 27, 2026*


- Fix ds_isvalid bound check to mitigate panic.

- After enabling UIKit Debug Overlay (aka [UIDebuggingInformationOverlay](https://www.ryanipete.com/blog/ios/swift/objective-c/uidebugginginformationoverlay)), it can be activated with double tap gesture on the status bar on Home Screen. It includes similar features to FLEX, but read-only.
<img width="1290" height="1415" alt="image" src="https://github.com/user-attachments/assets/25cf8b0f-2f73-44e7-8184-0357a80bafc2" />


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24983952811/job/73152634243) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24983952811/job/73152503725) — **success** — *GitHub Actions*


---

## PR Commits (4)

- [`ac0a871`](https://github.com/rooootdev/lara/commit/ac0a8712ca4db169db35fd4a88594155193cc8e2) Add Enable UIKit Debug Overlay — *khanhduytran0* · Apr 23, 2026
- [`dce04dc`](https://github.com/rooootdev/lara/commit/dce04dcb1b1b62e85f26f807aa889126750accac) Fix ds_isvalid bound check and reporting — *khanhduytran0* · Apr 26, 2026
- [`296746e`](https://github.com/rooootdev/lara/commit/296746efb1a2f1017ca580146ed6811ac0990860) Fix crash if recovering stashed primitives fails — *khanhduytran0* · Apr 27, 2026
- [`601e9dd`](https://github.com/rooootdev/lara/commit/601e9ddda9d2672fb13ca24f496f30340539c051) Fix deadlock when throwing exception from set_target_kaddr — *khanhduytran0* · Apr 27, 2026


---

## Changed Files (8)

- 🟡 `lara/kexploit/TaskRop/PrivateAPI.h` (modified) — +6 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -8,12 +8,18 @@
 @import Foundation;
 @import QuartzCore;
 
+id _Nullable objc_alloc(Class _Nullable cls);
+void objc_release(id _Nullable obj);
+
 #define CA_DEBUG_FLAG_PERFORMANCE_HUD 0x10000000
 extern void CARenderServerGetDebugFlags(mach_port_t port, int key);
 extern void CARenderServerGetDebugValue(mach_port_t port, int key);
 extern void CARenderServerSetDebugFlags(mach_port_t port, int key, int value);
 extern void CARenderServerSetDebugValue(mach_port_t port, int key, int value);
 
+// return 3 gadget
+extern int CGGetHardwareVectorCapabilities(void);
+
 // os_eligibility.h
 typedef CF_ENUM(uint64_t, EligibilityDomainType) {
     /// Install marketplace-distributed apps
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.m` (modified) — +9 / -8
  <details><summary>Diff</summary>

  ```diff
  @@ -48,13 +48,7 @@ + (NSString *)lcGuestAppId;
 #define RC_TASK_EXC_GUARD_MP_FATAL     0x80
 
 static BOOL rc_is_kernel_ptr(uint64_t value) {
-    if (!value) {
-        return NO;
-    }
-    if (VM_MIN_KERNEL_ADDRESS && VM_MAX_KERNEL_ADDRESS) {
-        return value >= VM_MIN_KERNEL_ADDRESS && value <= VM_MAX_KERNEL_ADDRESS;
-    }
-    return (value & 0xffff000000000000ULL) == 0xffff000000000000ULL;
+    return ds_isvalid(value);
 }
 
 static BOOL rc_is_kernel_or_smr_ptr(uint64_t value) {
@@ -1189,7 +1183,14 @@ - (int)initRemoteCallForProcess:(const char *)process useMigFilterBypass:(BOOL)u
 - (instancetype)initWithProcess:(NSString *)process useMigFilterBypass:(BOOL)useMigFilterBypass {
     self = [super init];
     const char *processName = process.UTF8String;
-    int rc = [self initRemoteCallForProcess:processName useMigFilterBypass:useMigFilterBypass];
+    int rc;
+    @try {
+        rc = [self initRemoteCallForProcess:processName useMigFilterBypass:useMigFilterBypass];
+    } @catch (NSException *exception) {
+        NSLog(@"(rc) initRemoteCallForProcess failed: %@", exception);
+        g_rc_last_init_error = exception.description;
+        return nil;
+    }
     if (rc) {
         NSLog(@"(rc) initRemoteCallForProcess failed");
         g_rc_last_init_error = self.lastError ?: @"RemoteCall init failed";
  ```

  </details>
- 🟡 `lara/kexploit/darksword.m` (modified) — +22 / -15
  <details><summary>Diff</summary>

  ```diff
  @@ -526,25 +526,29 @@ static void physical_oob_write_mo(mach_port_t mo, uint64_t mo_offset,
     uwrite64((uint64_t)target_object_sync_ptr, 0);
 }
 
+static pthread_mutex_t krwLock = PTHREAD_MUTEX_INITIALIZER;
 static void set_target_kaddr(uint64_t where) {
     if(!ds_isvalid(where)) {
-        printf("(ds) kaddr isn't valid, spinning here. kaddr: 0x%llx\n", where);
-        
-        void *bt[64]; int n = backtrace(bt, 64);
-        char **s = backtrace_symbols(bt, n);
-        for (int i = 0; i < n; i++) printf("  #%d %s\n", i, s[i]);
-        free(s);
+        printf("(ds) kaddr isn't valid: 0x%llx\n", where);
         
-        __builtin_trap();  // make crash intentionally
+        pthread_mutex_unlock(&krwLock);
+        @throw [NSException exceptionWithName:@"DarkswordException"
+                                       reason:[NSString stringWithFormat:@"set_target_kaddr got invalid kaddr 0x%llx", where]
+                                        userInfo:nil];
     }
     memset(control_data, 0, EARLY_KRW_LENGTH);
     *(uint64_t *)control_data = where;
     int res = setsockopt(control_socket, IPPROTO_ICMPV6, ICMP6_FILTER,
                           control_data, (socklen_t)EARLY_KRW_LENGTH);
-    if (res != 0) { pe_log("setsockopt failed (set_target_kaddr)!"); }
+    if (res != 0) {
+        pe_log("setsockopt failed (set_target_kaddr)!");
+        pthread_mutex_unlock(&krwLock);
+        @throw [NSException exceptionWithName:@"DarkswordException"
+                                        reason:@"setsockopt failed (set_target_kaddr)"
+                                        userInfo:nil];
+    }
 }
 
-static pthread_mutex_t krwLock = PTHREAD_MUTEX_INITIALIZER;
 static void early_kread(uint64_t where, void *read_buf, uint64_t size) {
     pthread_mutex_lock(&krwLock);
     if (size > EARLY_KRW_LENGTH) { pe_log("[!] error: size > EARLY_KRW_LENGTH"); return; }
@@ -1152,8 +1156,8 @@ static int pe(void) {
     pe_log("kernel r/w is ready!");
     ds_progress(0.99);
     
-    our_proc = ourproc();
-    our_task = taskbyproc(our_proc);
+    our_proc = proc_self();
+    our_task = task_self();
     
     pe_log("our_proc: 0x%llx", our_proc);
     pe_log("our_task: 0x%llx", our_task);
@@ -1387,9 +1391,12 @@ uint64_t ds_get_our_task(void) {
     return our_task;
 }
 
-bool ds_isvalid(uint64_t addr) {
-    if ((addr & 0xfffff00000000000) != 0xfffff00000000000) {
-        return false;
+bool ds_isvalid(uint64_t value) {
+    if (!value) {
+        return NO;
+    }
+    if (VM_MIN_KERNEL_ADDRESS && VM_MAX_KERNEL_ADDRESS) {
+        return value >= VM_MIN_KERNEL_ADDRESS && value <= VM_MAX_KERNEL_ADDRESS;
     }
-    return true;
+    return (value & 0xffff000000000000ULL) == 0xffff000000000000ULL;
 }
  ```

  </details>
- 🟡 `lara/kexploit/pe/rc.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -20,6 +20,7 @@ int five_icon_dock(RemoteCall *proc);
 int enable_upside_down(RemoteCall *proc);
 int enable_floating_dock(RemoteCall *proc);
 int enable_grid_app_switcher(RemoteCall *proc);
+int enable_debug_overlay(RemoteCall *proc);
 int get_performance_hud(RemoteCall *proc);
 int set_performance_hud(RemoteCall *proc, int selection);
 void wake_up_daemon(RemoteCall *sb, NSString *daemon, NSString *frameworkContainingXPCSerice);
  ```

  </details>
- 🟡 `lara/kexploit/pe/rc.m` (modified) — +49 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -17,6 +17,7 @@
 #import <sys/mman.h>
 #import <objc/runtime.h>
 
+#import "rc.h"
 #import "RemoteCall.h"
 #import "PrivateAPI.h"
 #import "rc.h"
@@ -429,6 +430,54 @@ int enable_grid_app_switcher(RemoteCall *proc) {
     return 0;
 }
 
+int enable_debug_overlay(RemoteCall *proc) {
+    uint64_t clsSBMainWorkspace = remote_getClass(proc, "SBMainWorkspace");
+    uint64_t clsDebugOverlayWindow = remote_getClass(proc, "UIDebuggingInformationOverlay");
+    uint64_t clsDebugOverlayHandler = remote_getClass(proc, "UIDebuggingInformationOverlayInvokeGestureHandler");
+    uint64_t clsUIApplication = remote_getClass(proc, "UIApplication");
+    uint64_t clsUITapGestureRecognizer = remote_getClass(proc, "UITapGestureRecognizer");
+    uint64_t clsUIWindow = remote_getClass(proc, "UIWindow");
+    
+    uint64_t selAddGestureRecognizer = remote_sel(proc, "addGestureRecognizer:");
+    uint64_t selHandleActivation = remote_sel(proc, "_handleActivationGesture:");
+    uint64_t selInit = remote_sel(proc, "init");
+    uint64_t selInitWithTargetAction = remote_sel(proc, "initWithTarget:action:");
+    uint64_t selMainHandler = remote_sel(proc, "mainHandler");
+    uint64_t selMainWindowScene = remote_sel(proc, "mainWindowScene");
+    uint64_t selOverlay = remote_sel(proc, "overlay");
+    uint64_t selPerform = remote_sel(proc, "performSelectorOnMainThread:withObject:waitUntilDone:");
+    uint64_t selSetNumberOfTapsRequired = remote_sel(proc, "setNumberOfTapsRequired:");
+    uint64_t selSetWindowScene = remote_sel(proc, "setWindowScene:");
+    uint64_t selSharedApplication = remote_sel(proc, "sharedApplication");
+    uint64_t selSharedInstance = remote_sel(proc, "sharedInstance");
+    uint64_t selStatusBar = remote_sel(proc, "statusBarForEmbeddedDisplay");
+    
+    // Swizzle -[UIDebuggingInformationOverlay init] to -[UIWindow init] to bypass internal check
+    uint64_t methodWindowInit = RemoteArbCall(proc, class_getInstanceMethod, clsUIWindow, selInit);
+    uint64_t methodOverlayInit = RemoteArbCall(proc, class_getInstanceMethod, clsDebugOverlayWindow, selInit);
+    uint64_t impWindowInit = RemoteArbCall(proc, method_getImplementation, methodWindowInit);
+    RemoteArbCall(proc, method_setImplementation, methodOverlayInit, impWindowInit);
+    
+    // Set windowScene
+    uint64_t workspace = remote_msg(proc, clsSBMainWorkspace, selSharedInstance, 0,0,0,0);
+    uint64_t mainWindowScene = remote_msg(proc, workspace, selMainWindowScene, 0,0,0,0);
+    remote_msg(proc, clsDebugOverlayWindow, selPerform, selOverlay, 0, 1,0); // init window
+    uint64_t debugOverlay = remote_msg(proc, clsDebugOverlayWindow, selOverlay, 0,0,0,0);
+    remote_msg(proc, debugOverlay, selPerform, selSetWindowScene, mainWindowScene, 1,0);
+    
+    // register gesture recognizer to the status bar
+    uint64_t target = remote_msg(proc, clsDebugOverlayHandler, selMainHandler, 0,0,0,0);
+    uint64_t doubleTapGesture = RemoteArbCall(proc, objc_alloc, clsUITapGestureRecognizer);
+    doubleTapGesture = remote_msg(proc, doubleTapGesture, selInitWithTargetAction, target, selHandleActivation, 0,0);
+    remote_msg(proc, doubleTapGesture, selSetNumberOfTapsRequired, 2, 0,0,0);
+    uint64_t app = remote_msg(proc, clsUIApplication, selSharedApplication, 0,0,0,0);
+    uint64_t statusBar = remote_msg(proc, app, selStatusBar, 0,0,0,0);
+    remote_msg(proc, statusBar, selPerform, selAddGestureRecognizer, doubleTapGesture, 1,0);
+    RemoteArbCall(proc, objc_release, doubleTapGesture);
+    
+    return 0;
+}
+
 int get_performance_hud(RemoteCall *proc) {
     uint64_t flags = RemoteArbCall(proc, CARenderServerGetDebugFlags, 0);
     if (flags & CA_DEBUG_FLAG_PERFORMANCE_HUD) {
  ```

  </details>
- 🟡 `lara/kexploit/persistence.m` (modified) — +7 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -123,8 +123,13 @@ bool recover_krw_primitives(void) {
     kernel_base = [primitive[@"KernelBase"] unsignedLongLongValue];
     kernel_slide = [primitive[@"KernelSlide"] unsignedLongLongValue];
     // see if it works properly
-    if (ds_kread32(kernel_base) != 0xFEEDFACF || ds_kread32(kernel_base + 4) != CPU_TYPE_ARM64) {
-        printf("(persist) kread failed\n");
+    @try {
+        if (ds_kread32(kernel_base) != 0xFEEDFACF || ds_kread32(kernel_base + 4) != CPU_TYPE_ARM64) {
+            printf("(persist) kread failed\n");
+            return false;
+        }
+    } @catch (NSException *exception) {
+        printf("(persist) kread threw an exception: %s\n", exception.reason.UTF8String);
         return false;
     }
     
  ```

  </details>
- 🟡 `lara/kexploit/utils.m` (modified) — +0 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -389,7 +389,6 @@ StringWrapper proc_get_p_name(uint64_t proc) {
     ds_kreadbuf(proc + off_proc_p_name - off, &procNameTmp.data, 40);
     memmove(procNameTmp.data, procNameTmp.data + off, 40 - off);
     procNameTmp.data[32] = '\0';
-    NSLog(@"Proc: %s", procNameTmp.data);
     return procNameTmp;
 }
 
  ```

  </details>
- 🟡 `lara/views/RemoteView.swift` (modified) — +11 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -147,6 +147,17 @@ struct RemoteView: View {
                 } label: {
                     Text("Enable Grid App Switcher (Broken animation)")
                 }
+                
+                Button {
+                    run("Enable UIKit Debug Overlay") {
+                        let result = enable_debug_overlay(mgr.sbProc)
+                        return "enable_debug_overlay() -> \(result)"
+                    }
+                } label: {
+                    Text("Enable UIKit Debug Overlay")
+                }
+            } footer: {
+                Text("To use UIKit Debug Overlay, double tap the status bar.")
             }
             
             Section {
  ```

  </details>


---

## References

### Commits (1)

- [9fc6ffb](https://github.com/letienndat/lara/commit/9fc6ffb8e54049fe785acc3b6361633ea2578ce1) — Squashed commit of the following:


---

## Timeline (Verbose)

- : 📝 Commit ac0a871: Add Enable UIKit Debug Overlay
- : 📝 Commit dce04dc: Fix ds_isvalid bound check and reporting
- : 📝 Commit 296746e: Fix crash if recovering stashed primitives fails
- : 📝 Commit 601e9dd: Fix deadlock when throwing exception from set_target_kaddr
- Apr 27, 2026: 🟣 @rooootdev merged
- Apr 27, 2026: 🔴 Closed by @rooootdev
- Apr 29, 2026: 🔗 Commit 9fc6ffb — Squashed commit of the following:


---

## Comments (2)

### Comment #1 — [@hxhlb](https://github.com/hxhlb) · Apr 27, 2026

`ID: 4324879795`


It fails to run "Ran Exploit" —crashes immediately on execution.
iOS 18.6, iPhone SE (2nd generation).

log is here:

lara started: 2026-04-27 15:06:09
(utils) TASK_TNEXT_OFFSET: 0x50
(utils) THREAD_MUPCB_OFFSET: 0xb8
(utils) PROC_PID_OFFSET: 0x60
(offs) loaded saved t1sz_boot: 0x19
(offs) t1sz_boot: 0x19
(offs) saved t1sz_boot: 0x19
initialized offsets
(offs) loaded saved t1sz_boot: 0x19
(offs) t1sz_boot: 0x19
(offs) saved t1sz_boot: 0x19
initialized offsets
(persist) recovered control_socket: 10
(persist) recovered rw_socket: 11
exploit failed.
(ds) starting darksword
(ds) recovering primitives from launchd...
(ds) setsockopt failed (set_target_kaddr)!
(ds) ds_run exception: DarkswordException: setsockopt failed (set_target_kaddr)

exploit failed.


---

### Comment #2 — [@hxhlb](https://github.com/hxhlb) · Apr 27, 2026

`ID: 4324982540`


I made these changes to get the program running properly.
Not sure if they’ll be helpful to you.

```
diff --git a/lara/kexploit/darksword.m b/lara/kexploit/darksword.m
index 42aef9b..bf2fbb0 100644
--- a/lara/kexploit/darksword.m
+++ b/lara/kexploit/darksword.m
@@ -540,9 +540,6 @@ static void set_target_kaddr(uint64_t where) {
     int res = setsockopt(control_socket, IPPROTO_ICMPV6, ICMP6_FILTER,
                           control_data, (socklen_t)EARLY_KRW_LENGTH);
     if (res != 0) {
         pe_log("setsockopt failed (set_target_kaddr)!");
-        @throw [NSException exceptionWithName:@"DarkswordException"
-                                        reason:@"setsockopt failed (set_target_kaddr)"
-                                        userInfo:nil];
     }
 }
 
@@ -1166,7 +1163,7 @@ static int pe(void) {
     return 0;
 }
 
-int ds_run(void) {
+static int ds_run_impl(void) {
     g_ds_ready = false;
     ds_progress(0.0);
     random_marker = ((uint64_t)arc4random() << 32) | arc4random();
@@ -1183,8 +1180,24 @@ int ds_run(void) {
         ds_progress(0.99);
     }
 
-    free(default_file_content);
-    default_file_content = NULL;
     return result;
 }
 
+int ds_run(void) {
+    int result = -1;
+    @try {
+        result = ds_run_impl();
+    } @catch (NSException *exception) {
+        const char *name = exception.name.UTF8String ?: "unknown";
+        const char *reason = exception.reason.UTF8String ?: "no reason";
+        pe_log("ds_run exception: %s: %s", name, reason);
+        g_ds_ready = false;
+        ds_progress(0.99);
+        result = -1;
+    } @finally {
+        free(default_file_content);
+        default_file_content = NULL;
+    }
+    return result;
+}
+
 bool ds_is_ready(void) {
     return g_ds_ready;
 }

```


---

*Exported on 10/14/2026, 3:03:07 PM from [https://github.com/rooootdev/lara/pull/201](https://github.com/rooootdev/lara/pull/201)*
