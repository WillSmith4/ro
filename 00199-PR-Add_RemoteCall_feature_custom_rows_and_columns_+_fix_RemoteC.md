# Add RemoteCall feature: custom rows and columns + fix RemoteCall crashes

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#199](https://github.com/rooootdev/lara/pull/199)  
> **State:** 🟣 merged  
> **Branch:** `xelahot:SBRowsAndColumns` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 26, 2026  
> **Changes:** +696 / -78 across 13 files  
> **Created:** Apr 26, 2026  
> **Updated:** Apr 27, 2026  
> **Closed:** Apr 26, 2026  

---

## Description

*Opened by [@xelahot](https://github.com/xelahot)* *on Apr 26, 2026*


<p>This PR implments multiple things:</p>
<ul>
  <li>SpringBoard custom rows & columns. This is inspired by <a href="https://github.com/zeroxjf/lightsaber">Lightsaber</a>  by zeroxjf</li>
  <li>RemoteCall crash fix when enabling it. All credits to m1337v. I simply took his fix from <a href="https://github.com/m1337v/lara">his fork</a> since he didnt make a pull request</li>
  <li>RemoteCall persistance/crash fix when closing Lara. All credits to m1337v. I simply took his fix from <a href="https://github.com/m1337v/lara">his fork</a> since he didnt make a pull request</li>
</ul>
<table>
  <tr>
    <td><img width="220" height="477" alt="c80b8263-7243-40ed-9677-437be90363d8" src="https://github.com/user-attachments/assets/e86cb080-87b1-4038-8085-a2c30cb6f8ee"/></td>
    <td><img width="220" height="477" alt="068fe7ce-001d-4d52-a05f-3caa59c72f10" src="https://github.com/user-attachments/assets/d2322cb9-8d32-4b71-969b-5562f7ef9a28"/></td>
  </tr>
</table>

If you want to test it, here's the IPA: https://github.com/xelahot/lara/actions/runs/24965986084/artifacts/6650097126


👍 2


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (3)

- [`7c9ea81`](https://github.com/rooootdev/lara/commit/7c9ea81e75588e92937d14deaf6e2ec7087ebd4a) SBRowsAndColums: Fix RemoteCall crash & persistance (credits to m1337v) — *xelahot* · Apr 26, 2026
- [`8112b74`](https://github.com/rooootdev/lara/commit/8112b7428fbf7e52764c3b7160064a05b8475233) SBRowsAndColums: Add SpringBoard custom rows & columns (inspired from Lightsaber by zeroxjf) — *xelahot* · Apr 26, 2026
- [`9cab18c`](https://github.com/rooootdev/lara/commit/9cab18c91ffb272a92aaecd9f7d51627e1fb12ff) SBRowsAndColums: Change the order of the feature and remove header — *xelahot* · Apr 26, 2026


---

## Changed Files (13)

- 🟡 `lara/classes/laramgr.swift` (modified) — +16 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -32,6 +32,7 @@ final class laramgr: ObservableObject {
     @Published var eu1running: Bool = false
     @Published var eu2progress: Double = 0.0
     @Published var eu2running: Bool = false
+    @Published var rcLastError: String?
     #endif
     
     @Published var vfsready: Bool = false
@@ -472,6 +473,7 @@ final class laramgr: ObservableObject {
         }
         
         rcrunning = true
+        rcLastError = nil
         logmsg("initializing remote call on \(process)...")
         
         DispatchQueue.global(qos: .userInitiated).async { [weak self] in
@@ -482,10 +484,18 @@ final class laramgr: ObservableObject {
                 let success = self.sbProc != nil
                 if success {
                     self.logmsg("remote call initialized on \(process)")
+                    self.rcLastError = nil
                     self.rcrunning = false
                     self.rcready = true
                 } else {
                     self.logmsg("remote call init failed on \(process)")
+                    let error = RemoteCall.lastInitError()
+                    self.rcLastError = error
+                    if let error, !error.isEmpty {
+                        self.logmsg("remote call init failed on \(process): \(error)")
+                    } else {
+                        self.logmsg("remote call init failed on \(process)")
+                    }
                     self.rcrunning = false
                 }
                 completion?(success)
@@ -518,7 +528,12 @@ final class laramgr: ObservableObject {
                     self.logmsg("remote call initialized on \(process)")
                     self.rcrunning = false
                 } else {
-                    self.logmsg("remote call init failed on \(process)")
+                    let error = RemoteCall.lastInitError()
+                    if let error, !error.isEmpty {
+                        self.logmsg("remote call init failed on \(process): \(error)")
+                    } else {
+                        self.logmsg("remote call init failed on \(process)")
+                    }
                     self.rcrunning = false
                 }
             }
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/Exception.h` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -37,6 +37,6 @@ typedef struct {
 
 mach_port_t create_exception_port(void);
 bool wait_exception(mach_port_t exceptionPort, ExceptionMessage *excBuffer, int timeout, bool debug);
-void reply_with_state(ExceptionMessage *exc, arm_thread_state64_internal *state);
+bool reply_with_state(ExceptionMessage *exc, arm_thread_state64_internal *state);
 
 #endif /* Exception_h */
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/Exception.m` (modified) — +5 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -50,7 +50,7 @@ bool wait_exception(mach_port_t exceptionPort, ExceptionMessage *excBuffer, int
     return true;
 }
 
-void reply_with_state(ExceptionMessage *exc, arm_thread_state64_internal *state)
+bool reply_with_state(ExceptionMessage *exc, arm_thread_state64_internal *state)
 {
     uint8_t replyBuf[EXCEPTION_REPLY_SIZE];
     memset(replyBuf, 0, sizeof(replyBuf));
@@ -73,6 +73,9 @@ void reply_with_state(ExceptionMessage *exc, arm_thread_state64_internal *state)
                                 MACH_PORT_NULL,
                                 MACH_MSG_TIMEOUT_NONE,
                                 MACH_PORT_NULL);
-    if (kr != KERN_SUCCESS)
+    if (kr != KERN_SUCCESS) {
         printf("(exc) reply_with_state failed: %s\n", mach_error_string(kr));
+        return false;
+    }
+    return true;
 }
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/PAC.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -17,6 +17,7 @@ uint64_t native_strip(uint64_t address);
 uint64_t pacia(uint64_t ptr, uint64_t modifier);
 uint64_t ptrauth_blend_discriminator_wrapper(uint64_t diver, uint64_t discriminator);
 uint64_t ptrauth_string_discriminator_special(const char *name);
+bool local_pac_signing_works(void);
 uint64_t find_pacia_gadget(void);
 void pac_cleanup(mach_port_t pacThread, mach_port_t exceptionPort, void *stack);
 uint64_t remote_pac(uint64_t remoteThreadAddr, uint64_t address, uint64_t modifier);
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/PAC.m` (modified) — +17 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -67,6 +67,23 @@ uint64_t ptrauth_string_discriminator_special(const char *name) {
     return 0;
 }
 
+bool local_pac_signing_works(void) {
+    if (!is_pac_supported()) {
+        return true;
+    }
+
+    void *pcSymbol = dlsym(RTLD_DEFAULT, "getpid");
+    if (!pcSymbol) {
+        pcSymbol = (void *)&local_pac_signing_works;
+    }
+
+    uint64_t pcProbe = native_strip((uint64_t)pcSymbol);
+    uint64_t lrProbe = native_strip((uint64_t)&local_pac_signing_works);
+    uint64_t signedPC = pacia(pcProbe, ptrauth_string_discriminator("pc"));
+    uint64_t signedLR = pacia(lrProbe, ptrauth_string_discriminator("lr"));
+    return signedPC != pcProbe || signedLR != lrProbe;
+}
+
 uint64_t find_pacia_gadget(void) {
     const uint32_t paciaGadgetOpcodes[] = {
         0xDAC10230,   // pacia x16, x17
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.h` (modified) — +7 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -74,6 +74,10 @@ uint64_t remote_pac(uint64_t remoteThreadAddr, uint64_t address, uint64_t modifi
     uint64_t _selfThreadAddr;
     uint32_t _selfThreadCtid;
     arm_thread_state64_internal _originalState;
+    bool _originalThreadNeedsRestore;
+    uint64_t _firstThreadReturnTrap;
+    uint64_t _secondThreadReturnTrap;
+    bool _liveContainerRuntime;
     uint64_t _vmMap;
     bool _success; // = true;
     //NSMutableArray<NSNumber *> *_threadList = nil;
@@ -88,6 +92,9 @@ uint64_t remote_pac(uint64_t remoteThreadAddr, uint64_t address, uint64_t modifi
 @property(nonatomic) uint64_t callThreadAddr;
 @property(nonatomic) uint64_t trojanThreadAddr;
 
++ (NSString *)lastInitError;
++ (BOOL)isLiveContainerRuntime;
++ (BOOL)isLiveProcessRuntime;
 - (NSUInteger)doRemoteCallStableWithTimeout:(int)timeout functionName:(char *)name functionPointer:(void *)ptr
                             args:(uint64_t *)args argCount:(NSUInteger)argCount;
 - (int)destroyRemoteCall;
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.m` (modified) — +506 / -72
  <details><summary>Diff</summary>

  ```diff
  @@ -12,6 +12,8 @@
 #include <errno.h>
 #include <string.h>
 #include <unistd.h>
+#include <limits.h>
+#include <mach-o/dyld.h>
 #import <sys/mman.h>
 
 #import "RemoteCall.h"
@@ -24,9 +26,181 @@
 #import "darksword.h"
 #import "utils.h"
 
+extern int proc_name(int pid, void *buffer, uint32_t buffersize);
+extern int proc_pidpath(int pid, void *buffer, uint32_t buffersize);
+
+#ifndef PROC_PIDPATHINFO_MAXSIZE
+#define PROC_PIDPATHINFO_MAXSIZE (4 * PATH_MAX)
+#endif
+
 @import ObjectiveC;
 
+@interface NSUserDefaults (LaraLiveContainerRuntime)
++ (bool)isLiveProcess;
++ (NSString *)lcGuestAppId;
+@end
+
 static bool g_mig_bypass_enabled = false;
+static NSString *g_rc_last_init_error = nil;
+
+#define RC_TASK_EXC_GUARD_MP_DELIVER   0x10
+#define RC_TASK_EXC_GUARD_MP_CORPSE    0x40
+#define RC_TASK_EXC_GUARD_MP_FATAL     0x80
+
+static BOOL rc_is_kernel_ptr(uint64_t value) {
+    if (!value) {
+        return NO;
+    }
+    if (VM_MIN_KERNEL_ADDRESS && VM_MAX_KERNEL_ADDRESS) {
+        return value >= VM_MIN_KERNEL_ADDRESS && value <= VM_MAX_KERNEL_ADDRESS;
+    }
+    return (value & 0xffff000000000000ULL) == 0xffff000000000000ULL;
+}
+
+static BOOL rc_is_kernel_or_smr_ptr(uint64_t value) {
+    if (!value) {
+        return NO;
+    }
+    if (rc_is_kernel_ptr(value)) {
+        return YES;
+    }
+
+    // kread_smrptr() can return compact kalloc/SMR table pointers outside
+    // VM_MIN_KERNEL_ADDRESS on PAC devices; the original IPC lookup uses them.
+    return (value & 0xffff000000000000ULL) == 0xffff000000000000ULL;
+}
+
+static bool rc_livecontainer_api_is_liveprocess(void) {
+    return [NSUserDefaults respondsToSelector:@selector(isLiveProcess)] && [NSUserDefaults isLiveProcess];
+}
+
+static void rc_current_process_identity(char kernelName[64], char hostName[64], char guestName[64]) {
+    kernelName[0] = '\0';
+    hostName[0] = '\0';
+    guestName[0] = '\0';
+
+    if (proc_name(getpid(), kernelName, 64) <= 0) {
+        kernelName[0] = '\0';
+    }
+
+    char hostPath[PROC_PIDPATHINFO_MAXSIZE] = {0};
+    if (proc_pidpath(getpid(), hostPath, sizeof(hostPath)) > 0) {
+        const char *hostBase = strrchr(hostPath, '/');
+        hostBase = hostBase ? hostBase + 1 : hostPath;
+        if (hostBase && hostBase[0]) {
+            snprintf(hostName, 64, "%s", hostBase);
+        }
+    }
+
+    char executablePath[PROC_PIDPATHINFO_MAXSIZE] = {0};
+    uint32_t executablePathLength = sizeof(executablePath);
+    if (_NSGetExecutablePath(executablePath, &executablePathLength) == 0) {
+        const char *guestBase = strrchr(executablePath, '/');
+        guestBase = guestBase ? guestBase + 1 : executablePath;
+        if (guestBase && guestBase[0]) {
+            snprintf(guestName, 64, "%s", guestBase);
+        }
+    }
+}
+
+static bool rc_name_is_liveprocess(const char *name) {
+    return name && strncmp(name, "LiveProcess", 64) == 0;
+}
+
+static bool rc_livecontainer_is_actual_liveprocess(bool liveContainerRuntime) {
+    if (!liveContainerRuntime) {
+        return false;
+    }
+    if (rc_livecontainer_api_is_liveprocess()) {
+        return true;
+    }
+
+    char kernelName[64] = {0};
+    char hostName[64] = {0};
+    char guestName[64] = {0};
+    rc_current_process_identity(kernelName, hostName, guestName);
+    if (rc_name_is_liveprocess(kernelName) || rc_name_is_liveprocess(hostName) || rc_name_is_liveprocess(guestName)) {
+        return true;
+    }
+
+    return false;
+}
+
+static BOOL rc_disable_excguard_kill_checked(uint64_t task) {
+    if (!rc_is_kernel_ptr(task) || !off_task_task_exc_guard) {
+        return NO;
+    }
+
+    uint64_t addr = task + off_task_task_exc_guard;
+    uint32_t before = ds_kread32(addr);
+
+    if (before & 0xffff0000U) {
+        return NO;
+    }
+
+    uint32_t after = before;
+    after &= ~(RC_TASK_EXC_GUARD_MP_CORPSE | RC_TASK_EXC_GUARD_MP_FATAL);
+    after |= RC_TASK_EXC_GUARD_MP_DELIVER;
+    ds_kwrite32(addr, after);
+
+    uint32_t verify = ds_kread32(addr);
+    if ((verify & (RC_TASK_EXC_GUARD_MP_CORPSE | RC_TASK_EXC_GUARD_MP_FATAL)) ||
+        !(verify & RC_TASK_EXC_GUARD_MP_DELIVER)) {
+        return NO;
+    }
+
+    return YES;
+}
+
+static uint64_t rc_task_get_ipc_port_object(uint64_t task, mach_port_t port) {
+    if (!rc_is_kernel_ptr(task) || port == MACH_PORT_NULL) {
+        return 0;
+    }
+
+    uint64_t itk_space = kread_ptr(task + off_task_itk_space);
+    if (!rc_is_kernel_ptr(itk_space)) {
+        return 0;
+    }
+
+    if (!sizeof_ipc_entry || !off_ipc_space_is_table) {
+        return 0;
+    }
+
+    uint64_t table = kread_smrptr(itk_space + off_ipc_space_is_table);
+    if (!is_pac_supported()) {
+        table |= 0xFFFFFF8000000000ULL;
+        table = kalloc_array_decode(table);
+    }
+    if (!rc_is_kernel_or_smr_ptr(table)) {
+        return 0;
+    }
+
+    uint64_t entry = table + (sizeof_ipc_entry * (port >> 8));
+    if (!rc_is_kernel_or_smr_ptr(entry)) {
+        return 0;
+    }
+
+    uint64_t object = kread_ptr(entry + off_ipc_entry_ie_object);
+    if (!rc_is_kernel_ptr(object)) {
+        return 0;
+    }
+
+    return object;
+}
+
+static uint64_t rc_task_get_ipc_port_kobject(uint64_t task, mach_port_t port) {
+    uint64_t object = rc_task_get_ipc_port_object(task, port);
+    if (!object) {
+        return 0;
+    }
+
+    uint64_t kobject = kread_ptr(object + off_ipc_port_ip_kobject);
+    if (!rc_is_kernel_ptr(kobject)) {
+        return 0;
+    }
+
+    return kobject;
+}
 
 void mig_bypass_init(uint64_t kernelSlide, uint64_t migLockOff, uint64_t migSbxMsgOff, uint64_t migKernelStackLROff) {
     printf("(rc) mig bypass init stub - not implemented yet\n");
@@ -38,18 +212,46 @@ void mig_bypass_pause(void) { }
 void mig_bypass_monitor_threads(uint64_t thread1, uint64_t thread2) { }
 
 @implementation RemoteCall
+
++ (NSString *)lastInitError {
+    return g_rc_last_init_error;
+}
+
++ (BOOL)isLiveContainerRuntime {
+    return islcruntime();
+}
+
++ (BOOL)isLiveProcessRuntime {
+    return rc_livecontainer_is_actual_liveprocess(islcruntime());
+}
+
 // bool set_exception_port_on_thread(mach_port_t exceptionPort, uint64_t currThread, bool useMigFilterBypass) {
 - (BOOL)setExceptionPortOnThread:(mach_port_t)exceptionPort forThread:(uint64_t)currThread useMigFilterBypass:(BOOL)useMigFilterBypass {
     bool success = false;
     void* thread_set_exception_ports_addr = dlsym(RTLD_DEFAULT, "thread_set_exception_ports");
     void* pthread_exit_addr = dlsym(RTLD_DEFAULT, "pthread_exit");
+    if (!thread_set_exception_ports_addr || !pthread_exit_addr) {
+        return false;
+    }
     
     pthread_t pthread = NULL;
-    pthread_create_suspended_np(&pthread, NULL,
+    int pthreadErr = pthread_create_suspended_np(&pthread, NULL,
         (void *(*)(void *))thread_set_exception_ports_addr, NULL);
+    if (pthreadErr != 0 || !pthread) {
+        return false;
+    }
     
     mach_port_t machThread = pthread_mach_thread_np(pthread);
-    uint64_t machThreadAddr = task_get_ipc_port_kobject(task_self(), machThread);
+    if (machThread == MACH_PORT_NULL) {
+        pthread_cancel(pthread);
+        return false;
+    }
+
+    uint64_t machThreadAddr = rc_task_get_ipc_port_kobject(task_self(), machThread);
+    if (!machThreadAddr) {
+        pthread_cancel(pthread);
+        return false;
+    }
 
     if(useMigFilterBypass) {
         mig_bypass_monitor_threads(_selfThreadAddr, machThreadAddr);
@@ -58,16 +260,26 @@ - (BOOL)setExceptionPortOnThread:(mach_port_t)exceptionPort forThread:(uint64_t)
     arm_thread_state64_internal state;
     memset(&state, 0, sizeof(state));
     mach_msg_type_number_t count = ARM_THREAD_STATE64_COUNT;
-    thread_get_state(machThread, ARM_THREAD_STATE64, (thread_state_t)&state, &count);
+    kern_return_t kr = thread_get_state(machThread, ARM_THREAD_STATE64, (thread_state_t)&state, &count);
+    if (kr != KERN_SUCCESS) {
+        pthread_cancel(pthread);
+        return false;
+    }
     
     uint64_t diver = 0;
     diver = (uint64_t)state.__flags & __DARWIN_ARM_THREAD_STATE64_USER_DIVERSIFIER_MASK;
     
     arm_thread_state64_set_pc_fptr(state, thread_set_exception_ports_addr);
     arm_thread_state64_set_lr_fptr(state, pthread_exit_addr);
     
+    uint64_t exceptionMask = EXC_MASK_GUARD |
+                             EXC_MASK_BAD_ACCESS |
+                             EXC_MASK_BAD_INSTRUCTION |
+                             EXC_MASK_BREAKPOINT |
+                             EXC_MASK_ARITHMETIC;
+
     state.__x[0] = _dummyThreadMach;
-    state.__x[1] = EXC_MASK_GUARD | EXC_MASK_BAD_ACCESS;
+    state.__x[1] = exceptionMask;
     state.__x[2] = exceptionPort;
     state.__x[3] = EXCEPTION_STATE | MACH_EXCEPTION_CODES;
     state.__x[4] = ARM_THREAD_STATE64;
@@ -76,16 +288,20 @@ - (BOOL)setExceptionPortOnThread:(mach_port_t)exceptionPort forThread:(uint64_t)
         usleep(100000);
     
     if (!thread_set_state_wrapper(machThread, machThreadAddr,
-                                  (arm_thread_state64_internal *)&state))
+                                  (arm_thread_state64_internal *)&state)) {
+        pthread_cancel(pthread);
         return false;
+    }
     
     if(useMigFilterBypass)
         usleep(100000);
     
     thread_set_mutex(_dummyThreadAddr, _selfThreadCtid);
     
-    if (!thread_resume_wrapper(machThread))
+    if (!thread_resume_wrapper(machThread)) {
+        pthread_cancel(pthread);
         return false;
+    }
     
     for (int i = 0; i < 10; i++)
     {
@@ -134,13 +350,13 @@ - (BOOL)setExceptionPortOnThread:(mach_port_t)exceptionPort forThread:(uint64_t)
         uint64_t checkAddr2 = trunc_page(kernelSP) + found + 0x10ULL;
         uint64_t checkVal2  = ds_kread64(checkAddr2);
 
-        printf("(rc) [iter %d] checkVal=0x%llx checkVal2=0x%llx (expecting 0x1002)\n", i, checkVal, checkVal2);
+        printf("(rc) [iter %d] checkVal=0x%llx checkVal2=0x%llx (expecting 0x%llx)\n", i, checkVal, checkVal2, exceptionMask);
         fflush(stdout);
 
-        if (checkVal == 0x1002 || checkVal2 == 0x1002) {
+        if (checkVal == exceptionMask || checkVal2 == exceptionMask) {
             correctTro = true;
         } else {
-            printf("(rc) [iter %d] Wrong tro checkVals (0x%llx, 0x%llx) != 0x1002. Retry...\n", i, checkVal, checkVal2);
+            printf("(rc) [iter %d] Wrong tro checkVals (0x%llx, 0x%llx) != 0x%llx. Retry...\n", i, checkVal, checkVal2, exceptionMask);
             fflush(stdout);
             continue;
         }
@@ -186,20 +402,31 @@ - (void)signState:(uint64_t)signingThread withState:(arm_thread_state64_internal
         diver = (uint64_t)state->__flags & __DARWIN_ARM_THREAD_STATE64_USER_DIVERSIFIER_MASK;
         uint64_t discPC = ptrauth_blend_discriminator_wrapper(diver, ptrauth_string_discriminator_special("pc"));
         uint64_t discLR = ptrauth_blend_discriminator_wrapper(diver, ptrauth_string_discriminator_special("lr"));
+        uint64_t strippedPC = native_strip(pc);
+        uint64_t strippedLR = native_strip(lr);
+        uint64_t signedPC = 0;
+        uint64_t signedLR = 0;
         
         if (pc) {
-            uint32_t flags = state->__flags;
-            flags &= ~__DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_PC;
-            state->__flags = flags;
-            state->__pc = remote_pac(signingThread, pc, discPC);
+            signedPC = remote_pac(signingThread, pc, discPC);
+            if (!signedPC || signedPC == UINT64_MAX) {
+                signedPC = strippedPC;
+            }
         }
         if (lr) {
-            uint32_t flags = state->__flags;
-            flags &= ~(__DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_LR |
-                       __DARWIN_ARM_THREAD_STATE64_FLAGS_IB_SIGNED_LR);
-            state->__flags = flags;
-            state->__lr = remote_pac(signingThread, lr, discLR);
+            signedLR = remote_pac(signingThread, lr, discLR);
+            if (!signedLR || signedLR == UINT64_MAX) {
+                signedLR = strippedLR;
+            }
         }
+
+        uint32_t flags = state->__flags;
+        flags &= ~(__DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_PC |
+                   __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_LR |
+                   __DARWIN_ARM_THREAD_STATE64_FLAGS_IB_SIGNED_LR);
+        state->__flags = flags;
+        if (pc) state->__pc = signedPC;
+        if (lr) state->__lr = signedLR;
         return;
     }
     
@@ -216,7 +443,62 @@ - (NSUInteger)doRemoteCallTempWithTimeout:(int)timeout functionName:(char *)name
                                      args:(uint64_t *)args argCount:(NSUInteger)argCount
 {
     return [self doRemoteCallInternalTimeout:timeout exceptionPort:_firstExceptionPort
-                                                 lrMarker:FAKE_LR_TROJAN_CREATOR functionName:name functionPointer:ptr args:args argCount:argCount];
+                                                 lrMarker:(_firstThreadReturnTrap ?: FAKE_LR_TROJAN_CREATOR) functionName:name functionPointer:ptr args:args argCount:argCount];
+}
+
+- (NSUInteger)doRemoteCallWithPendingException:(ExceptionMessage *)exc
+                                       timeout:(int)timeout
+                                 exceptionPort:(mach_port_t)exceptionPort
+                                      lrMarker:(uint64_t)lrMarker
+                                  functionName:(char *)name
+                               functionPointer:(void *)ptr
+                                          args:(uint64_t *)args
+                                      argCount:(NSUInteger)argCount
+{
+    int newTimeout = (10000 > timeout) ? 10000 : timeout;
+    uint64_t pcAddr = native_strip((uint64_t)ptr);
+
+    if (argCount > 8) {
+        uint64_t sp = native_strip(exc->threadState.__sp);
+        for (NSUInteger i = 8; i < argCount; i++) {
+            self[sp + ((i - 8) * sizeof(uint64_t))].value64 = args[i];
+        }
+        argCount = 8;
+    }
+    memcpy(&exc->threadState.__x[0], args, argCount * sizeof(uint64_t));
+    bzero(&exc->threadState.__x[argCount], (8 - argCount) * sizeof(uint64_t));
+    [self signState:_trojanThreadAddr withState:&exc->threadState pc:pcAddr lr:lrMarker];
+    if (_liveContainerRuntime && is_pac_supported() &&
+        (exc->threadState.__pc == pcAddr || exc->threadState.__lr == native_strip(lrMarker))) {
+        self.lastError = @"LiveContainer RemoteCall PAC signing returned raw pointers.";
+        return 0;
+    }
+
+    if (!reply_with_state(exc, &exc->threadState)) {
+        return 0;
+    }
+
+    if (timeout < 0) {
+        return 0;
+    }
+
+    ExceptionMessage exc2;
+    if (!wait_exception(exceptionPort, &exc2, newTimeout, false)) {
+        return 0;
+    }
+    uint64_t returnPC = native_strip(exc2.threadState.__pc);
+    uint64_t returnLR = native_strip(exc2.threadState.__lr);
+    if (returnPC != native_strip(lrMarker) && returnLR != native_strip(lrMarker)) {
+        self.lastError = [NSString stringWithFormat:@"Unexpected trap after %s: pc=0x%llx lr=0x%llx", name, exc2.threadState.__pc, exc2.threadState.__lr];
+        return 0;
+    }
+
+    uint64_t retValue = exc2.threadState.__x[0];
+    if (!reply_with_state(&exc2, &exc2.threadState)) {
+        return 0;
+    }
+
+    return retValue;
 }
 
 //uint64_t do_remote_call_stable(int timeout, const char *name,
@@ -228,7 +510,7 @@ - (NSUInteger)doRemoteCallStableWithTimeout:(int)timeout functionName:(char *)na
     if (!_creatingExtraThread)
         //return do_remote_call_temp(timeout, name, x0, x1, x2, x3, x4, x5, x6, x7);
         return [self doRemoteCallTempWithTimeout:timeout functionName:name functionPointer:pcAddr args:args argCount:argCount];
-    return [self doRemoteCallInternalTimeout:timeout exceptionPort:_secondExceptionPort lrMarker:FAKE_LR_TROJAN
+    return [self doRemoteCallInternalTimeout:timeout exceptionPort:_secondExceptionPort lrMarker:(_secondThreadReturnTrap ?: FAKE_LR_TROJAN)
                                 functionName:name functionPointer:pcAddr args:args argCount:argCount];
 }
 
@@ -260,14 +542,16 @@ - (NSUInteger)doRemoteCallInternalTimeout:(int)timeout exceptionPort:(mach_port_
     if (argCount > 8) {
         uint64_t sp = native_strip(exc.threadState.__sp);
         for (int i = 8; i < argCount; i++) {
-            self[sp + sizeof(uint64_t[i-8])].value64 = args[i];
-            argCount = 8;
+            self[sp + ((i - 8) * sizeof(uint64_t))].value64 = args[i];
         }
+        argCount = 8;
     }
     memcpy(&exc.threadState.__x[0], args, argCount * sizeof(uint64_t));
     bzero(&exc.threadState.__x[argCount], (8 - argCount) * sizeof(uint64_t));
     [self signState:_trojanThreadAddr withState:&exc.threadState pc:pcAddr lr:lrMarker];
-    reply_with_state(&exc, &exc.threadState);
+    if (!reply_with_state(&exc, &exc.threadState)) {
+        return 0;
+    }
 
     if (timeout < 0) {
         printf("(rc) Trojan thread cleanup\n");
@@ -279,14 +563,19 @@ - (NSUInteger)doRemoteCallInternalTimeout:(int)timeout exceptionPort:(mach_port_
         printf("(rc) Don't receive second exception on %s thread\n", threadStr);
         return 0;
     }
-    if (native_strip(exc2.threadState.__lr) != lrMarker) {
-        printf("[%s:%d] Process might have crashed! Unexpected LR: 0x%llx (expected 0x%llx)\n", __FUNCTION__, __LINE__, exc2.threadState.__lr, lrMarker);
+    uint64_t returnPC = native_strip(exc2.threadState.__pc);
+    uint64_t returnLR = native_strip(exc2.threadState.__lr);
+    if (returnPC != native_strip(lrMarker) && returnLR != native_strip(lrMarker)) {
+        printf("(rc) Process might have crashed! Unexpected trap: pc=0x%llx lr=0x%llx expected=0x%llx\n",
+               exc2.threadState.__pc, exc2.threadState.__lr, lrMarker);
         NSLog(@"%@", NSThread.callStackSymbols);
         [self destroyRemoteCall];
-        self.lastError = [NSString stringWithFormat:@"Process might have crashed! Unexpected LR: 0x%llx (expected 0x%llx)", exc2.threadState.__lr, lrMarker];
+        self.lastError = [NSString stringWithFormat:@"Process might have crashed! Unexpected trap pc=0x%llx lr=0x%llx", exc2.threadState.__pc, exc2.threadState.__lr];
     }
     uint64_t retValue = exc2.threadState.__x[0];
-    reply_with_state(&exc2, &exc2.threadState);
+    if (!reply_with_state(&exc2, &exc2.threadState)) {
+        return 0;
+    }
     printf("(rc) %s func's retValue = 0x%llx(%llu)\n", name, retValue, retValue);
     if(isTempCall && strcmp(name, "getpid") == 0 && retValue == 0) {
         printf("(rc) getpid failed\n");
@@ -300,37 +589,54 @@ - (NSUInteger)doRemoteCallInternalTimeout:(int)timeout exceptionPort:(mach_port_
 - (BOOL)restoreTrojanThreadWithState:(arm_thread_state64_internal *)state
 {
     ExceptionMessage exc;
-    if (!wait_exception(_firstExceptionPort, &exc, 20000, false)) {
+    if (!wait_exception(_firstExceptionPort, &exc, 5000, false)) {
         printf("(rc) Failed to receive exception while restoring\n");
         return false;
     }
     
     state->__flags = exc.threadState.__flags;
     [self signState:_trojanThreadAddr withState:state pc:state->__pc lr:state->__lr];
-    reply_with_state(&exc, state);
+    if (!reply_with_state(&exc, state)) {
+        return false;
+    }
+    _originalThreadNeedsRestore = false;
     return true;
 }
 
 // int destroy_remote_call(void) {
 - (int)destroyRemoteCall {
-    if (_trojanMem) {
+    if (_success && _trojanMem) {
         RemoteArbCallWithTimeout(100, self, munmap, _trojanMem, PAGE_SIZE);
-        _trojanMem = 0;
-    } else {
-        return 0;
-    }
-    if (_creatingExtraThread) {
-        RemoteArbCallWithTimeout(-1, self, pthread_exit, 0);
-    }
-    else {
+
+        if (_creatingExtraThread) {
+            RemoteArbCallWithTimeout(-1, self, pthread_exit, 0);
+        }
+        else {
+            [self restoreTrojanThreadWithState:&_originalState];
+        }
+    } else if (_originalThreadNeedsRestore) {
         [self restoreTrojanThreadWithState:&_originalState];
     }
 
-    mach_port_destruct(mach_task_self_, _firstExceptionPort, 0, 0);
-    mach_port_destruct(mach_task_self_, _secondExceptionPort, 0, 0);
-    pthread_cancel(_dummyThread);
+    if (_firstExceptionPort != MACH_PORT_NULL) {
+        mach_port_destruct(mach_task_self_, _firstExceptionPort, 0, 0);
+        _firstExceptionPort = MACH_PORT_NULL;
+    }
+    if (_secondExceptionPort != MACH_PORT_NULL) {
+        mach_port_destruct(mach_task_self_, _secondExceptionPort, 0, 0);
+        _secondExceptionPort = MACH_PORT_NULL;
+    }
+    if (_dummyThread) {
+        pthread_cancel(_dummyThread);
+        _dummyThread = NULL;
+    }
     
     self.threadList = [NSMutableArray new];
+    _trojanMem = 0;
+    _success = false;
+    _creatingExtraThread = false;
+    _originalThreadNeedsRestore = false;
+    _liveContainerRuntime = false;
     
     return 0;
 }
@@ -508,13 +814,29 @@ - (uint64_t)retryFirstThreadWithMigFilterBypass:(BOOL)useMigFilterBypass {
 // NOTE: Do not run this function while "attaching xcode" on iOS 18+, it will make device unstable.
 //int init_remote_call(const char* process, bool useMigFilterBypass) {
 - (int)initRemoteCallForProcess:(const char *)process useMigFilterBypass:(BOOL)useMigFilterBypass {
+    if (!process || process[0] == '\0') {
+        return -1;
+    }
+
+    _liveContainerRuntime = islcruntime();
+    if (_liveContainerRuntime) {
+        bool localPACWorks = local_pac_signing_works();
+        if (!localPACWorks) {
+            self.lastError = @"LiveContainer can run the exploit, but this launch context cannot generate arm64e PAC for RemoteCall.";
+            return -1;
+        }
+    }
+
     uint64_t procAddr = proc_find_by_name(process);
     if (!procAddr) {
         printf("(rc) Unable to find process: %s\n", process);
         return -1;
     }
     printf("(rc) process: %s, pid: %u\n",  process, ds_kread32(procAddr + off_proc_p_pid));
     _taskAddr = proc_task(procAddr);
+    if (!_taskAddr) {
+        return -1;
+    }
     
     mach_port_t firstExceptionPort = create_exception_port();
     mach_port_t secondExceptionPort = create_exception_port();
@@ -530,27 +852,61 @@ - (int)initRemoteCallForProcess:(const char *)process useMigFilterBypass:(BOOL)u
     }
     
     // Make sure the task won't crash after we handle an exception
-    disable_excguard_kill(_taskAddr);
+    if (!rc_disable_excguard_kill_checked(_taskAddr)) {
+        mach_port_destruct(mach_task_self_, firstExceptionPort, 0, 0);
+        mach_port_destruct(mach_task_self_, secondExceptionPort, 0, 0);
+        return -1;
+    }
     
     mach_exception_code_t guardCode = 0;
     EXC_GUARD_ENCODE_TYPE(guardCode, GUARD_TYPE_MACH_PORT);
     EXC_GUARD_ENCODE_FLAVOR(guardCode, kGUARD_EXC_INVALID_RIGHT);
     EXC_GUARD_ENCODE_TARGET(guardCode, 0xf503ULL);  // ??? what is 0xf503 value meaning?
     
-    uint64_t firstPortAddr = task_get_ipc_port_kobject(task_self(), firstExceptionPort);
-    uint64_t secondPortAddr = task_get_ipc_port_kobject(task_self(), secondExceptionPort);
+    uint64_t selfTask = task_self();
+    uint64_t firstPortAddr = rc_task_get_ipc_port_object(selfTask, firstExceptionPort);
+    uint64_t secondPortAddr = rc_task_get_ipc_port_object(selfTask, secondExceptionPort);
+    if (!firstPortAddr || !secondPortAddr) {
+        mach_port_destruct(mach_task_self_, firstExceptionPort, 0, 0);
+        mach_port_destruct(mach_task_self_, secondExceptionPort, 0, 0);
+        return -1;
+    }
     
     pthread_t dummyThread = NULL;
     void *dummyFunc = dlsym(RTLD_DEFAULT, "getpid");
-    pthread_create_suspended_np(&dummyThread, NULL, (void *(*)(void *))dummyFunc, NULL);
+    if (!dummyFunc) {
+        mach_port_destruct(mach_task_self_, firstExceptionPort, 0, 0);
+        mach_port_destruct(mach_task_self_, secondExceptionPort, 0, 0);
+        return -1;
+    }
+    int dummyErr = pthread_create_suspended_np(&dummyThread, NULL, (void *(*)(void *))dummyFunc, NULL);
+    if (dummyErr != 0 || !dummyThread) {
+        mach_port_destruct(mach_task_self_, firstExceptionPort, 0, 0);
+        mach_port_destruct(mach_task_self_, secondExceptionPort, 0, 0);
+        return -1;
+    }
     mach_port_t dummyThreadMach = pthread_mach_thread_np(dummyThread);
-    uint64_t dummyThreadAddr = task_get_ipc_port_kobject(task_self(), dummyThreadMach);
+    if (dummyThreadMach == MACH_PORT_NULL) {
+        pthread_cancel(dummyThread);
+        mach_port_destruct(mach_task_self_, firstExceptionPort, 0, 0);
+        mach_port_destruct(mach_task_self_, secondExceptionPort, 0, 0);
+        return -1;
+    }
+    uint64_t dummyThreadAddr = rc_task_get_ipc_port_kobject(selfTask, dummyThreadMach);
     uint64_t dummyThreadTro = ds_kread64(dummyThreadAddr + off_thread_t_tro);
     mach_port_t threadSelf = mach_thread_self();
-    uint64_t selfThreadAddr = task_get_ipc_port_kobject(task_self(), threadSelf);
+    uint64_t selfThreadAddr = rc_task_get_ipc_port_kobject(selfTask, threadSelf);
     uint32_t selfThreadCtid = ds_kread32(selfThreadAddr + off_thread_ctid);
+    if (!dummyThreadAddr || !dummyThreadTro || !selfThreadAddr) {
+        pthread_cancel(dummyThread);
+        mach_port_deallocate(mach_task_self_, threadSelf);
+        mach_port_destruct(mach_task_self_, firstExceptionPort, 0, 0);
+        mach_port_destruct(mach_task_self_, secondExceptionPort, 0, 0);
+        return -1;
+    }
+    mach_port_deallocate(mach_task_self_, threadSelf);
     
-    _creatingExtraThread = true;
+    _creatingExtraThread = false;
     _firstExceptionPort = firstExceptionPort;
     _secondExceptionPort = secondExceptionPort;
     _firstExceptionPortAddr = firstPortAddr;
@@ -569,13 +925,17 @@ - (int)initRemoteCallForProcess:(const char *)process useMigFilterBypass:(BOOL)u
     int successThreadCount = 0;
     uint64_t firstThread = ds_kread64(_taskAddr + off_task_threads_next);
     uint64_t currThread = firstThread;
+    if (!firstThread) {
+        [self destroyRemoteCall];
+        return -1;
+    }
     
-    _trojanThreadAddr = firstThread;
+    _trojanThreadAddr = 0;
     
     if (useMigFilterBypass)
         mig_bypass_resume();
     
-    while (successThreadCount < 2 && validThreadCount < 5 && retryCount < 3) {
+    while (successThreadCount < 1 && validThreadCount < 5 && retryCount < 3) {
         uint64_t task = thread_get_task(currThread);
         if (!task) {
             if (!validThreadCount) {
@@ -611,6 +971,7 @@ - (int)initRemoteCallForProcess:(const char *)process useMigFilterBypass:(BOOL)u
                         continue;
                     }
                 } else {
+                    _trojanThreadAddr = currThread;
                     successThreadCount++;
                     [_threadList addObject:@(currThread)];
                     printf("(rc) Inject EXC_GUARD on thread:0x%llx OK\n", (unsigned long long)currThread);
@@ -673,49 +1034,106 @@ - (int)initRemoteCallForProcess:(const char *)process useMigFilterBypass:(BOOL)u
         reply_with_state(&exc2, &exc2.threadState);
     }
     
-    uint64_t trojanMemTemp = ((uint64_t)exc.threadState.__sp & 0x7fffffffffULL) - 0x100ULL;
+    uint64_t trojanMemTemp = ((uint64_t)exc.threadState.__sp & 0x7fffffffffULL) - 0x4000ULL;
     printf("(rc) trojanMemTemp: 0x%llx\n", trojanMemTemp);
     fflush(stdout);
     
-    arm_thread_state64_internal newState = exc.threadState;
-    printf("(rc) calling sign_state for FAKE_PC_TROJAN_CREATOR...\n");
-    fflush(stdout);
-    [self signState:_trojanThreadAddr withState:&newState pc:FAKE_PC_TROJAN_CREATOR lr:FAKE_LR_TROJAN_CREATOR];
-    printf("(rc) sign_state done, pc=0x%llx lr=0x%llx flags=0x%x\n",
-           newState.__pc, newState.__lr, newState.__flags);
-    fflush(stdout);
-    reply_with_state(&exc, &newState);
-    printf("(rc) reply_with_state done, waiting for crash...\n");
-    fflush(stdout);
-    
     _vmMap = task_get_vm_map(_taskAddr);
     printf("(rc) vmMap: 0x%llx\n", _vmMap);
     fflush(stdout);
+
+    // Match wh1te4ever / pre-OOP Lara: use a low fake PC gate so the first
+    // parked exception is EXC_BAD_ACCESS, not EXC_BREAKPOINT.
+    uint64_t firstThreadParkTrap = FAKE_PC_TROJAN_CREATOR;
+    _firstThreadReturnTrap = FAKE_LR_TROJAN_CREATOR;
+    _secondThreadReturnTrap = FAKE_LR_TROJAN;
+
+    _originalThreadNeedsRestore = true;
+
+    uint64_t probePid = 0;
+    if (_liveContainerRuntime) {
+        uint64_t noArgs[1] = {0};
+        probePid = [self doRemoteCallWithPendingException:&exc
+                                                  timeout:100
+                                            exceptionPort:firstExceptionPort
+                                                 lrMarker:_firstThreadReturnTrap
+                                             functionName:"getpid"
+                                          functionPointer:getpid
+                                                     args:noArgs
+                                                 argCount:0];
+    } else {
+        arm_thread_state64_internal parkState = exc.threadState;
+        [self signState:_trojanThreadAddr withState:&parkState pc:firstThreadParkTrap lr:_firstThreadReturnTrap];
+        if (!reply_with_state(&exc, &parkState)) {
+            [self destroyRemoteCall];
+            return -1;
+        }
+
+        probePid = RemoteArbCallTempWithTimeout(100, self, getpid);
+    }
+    if (!probePid) {
+        [self destroyRemoteCall];
+        return -1;
+    }
     
-    printf("(rc) signing FAKE_PC_TROJAN via remote_pac...\n");
-    fflush(stdout);
-    uint64_t remoteCrashSigned = remote_pac(_trojanThreadAddr, FAKE_PC_TROJAN, 0);
+    uint64_t threadStartTrap = FAKE_PC_TROJAN;
+    uint64_t remoteCrashSigned = remote_pac(_trojanThreadAddr, threadStartTrap, 0);
     printf("(rc) remoteCrashSigned: 0x%llx\n", remoteCrashSigned);
     fflush(stdout);
-    RemoteArbCallTempWithTimeout(100, self, getpid); // for testing
-    RemoteArbCallTempWithTimeout(100, self, pthread_create_suspended_np, trojanMemTemp, 0, remoteCrashSigned, 0);
+    if (!remoteCrashSigned) {
+        [self destroyRemoteCall];
+        return -1;
+    }
+    uint64_t createThreadRet = RemoteArbCallTempWithTimeout(100, self, pthread_create_suspended_np, trojanMemTemp, 0, remoteCrashSigned, 0);
     
     printf("(rc) trojanMemTemp: 0x%llx\n", trojanMemTemp);
     uint64_t pthreadAddr    = self[trojanMemTemp].value64;
     printf("(rc) pthreadAddr: 0x%llx\n", pthreadAddr);
+    if (createThreadRet != 0 || !pthreadAddr) {
+        [self destroyRemoteCall];
+        return -1;
+    }
     uint64_t callThreadPort = RemoteArbCallTempWithTimeout(100, self, pthread_mach_thread_np, pthreadAddr);
     printf("(rc) callThreadPort: 0x%llx\n", callThreadPort);
-    _callThreadAddr = task_get_ipc_port_kobject(_taskAddr, (mach_port_t)callThreadPort);
+    if (!callThreadPort) {
+        [self destroyRemoteCall];
+        return -1;
+    }
+    _callThreadAddr = rc_task_get_ipc_port_kobject(_taskAddr, (mach_port_t)callThreadPort);
+    if (!_callThreadAddr) {
+        [self destroyRemoteCall];
+        return -1;
+    }
     
     if(useMigFilterBypass)
         mig_bypass_resume();
     
     if (![self setExceptionPortOnThread:secondExceptionPort forThread:_callThreadAddr useMigFilterBypass:useMigFilterBypass]) {
         printf("(rc) Failed set exc port on new thread, retrying...\n");
-        pthread_create_suspended_np(&dummyThread, NULL, (void *(*)(void *))dummyFunc, NULL);
+        int retryDummyErr = pthread_create_suspended_np(&dummyThread, NULL, (void *(*)(void *))dummyFunc, NULL);
+        if (retryDummyErr != 0 || !dummyThread) {
+            if(useMigFilterBypass)
+                mig_bypass_pause();
+            [self destroyRemoteCall];
+            return -1;
+        }
         _dummyThreadMach = pthread_mach_thread_np(dummyThread);
-        _dummyThreadAddr = task_get_ipc_port_kobject(mach_task_self_, _dummyThreadMach);
+        if (_dummyThreadMach == MACH_PORT_NULL) {
+            pthread_cancel(dummyThread);
+            if(useMigFilterBypass)
+                mig_bypass_pause();
+            [self destroyRemoteCall];
+            return -1;
+        }
+        _dummyThreadAddr = rc_task_get_ipc_port_kobject(task_self(), _dummyThreadMach);
         _dummyThreadTro  = thread_get_t_tro(_dummyThreadAddr);
+        if (!_dummyThreadAddr || !_dummyThreadTro) {
+            pthread_cancel(dummyThread);
+            if(useMigFilterBypass)
+                mig_bypass_pause();
+            [self destroyRemoteCall];
+            return -1;
+        }
         sleep(1);
         if (![self setExceptionPortOnThread:secondExceptionPort forThread:_callThreadAddr useMigFilterBypass:useMigFilterBypass]) {
             if(useMigFilterBypass)
@@ -734,19 +1152,31 @@ - (int)initRemoteCallForProcess:(const char *)process useMigFilterBypass:(BOOL)u
     if (ret != 0) {
         printf("(rc) Couldn't resume new thread, falling back to original\n");
         _creatingExtraThread = false;
+    } else {
+        _creatingExtraThread = true;
     }
     
     if (_creatingExtraThread) {
         printf("(rc) New thread created, resuming original\n");
         //restore_trojan_thread(&_originalState);
         [self restoreTrojanThreadWithState:&_originalState];
+        _trojanThreadAddr = _callThreadAddr;
     }
     printf("(rc) Original thread restored\n");
     
     _pid = (int)RemoteArbCallWithTimeout(100, self, getpid);
     printf("(rc) Task pid: %d\n", _pid);
+    if (_pid <= 0) {
+        [self destroyRemoteCall];
+        return -1;
+    }
     
     _trojanMem = RemoteArbCallWithTimeout(100, self, mmap, 0, PAGE_SIZE, VM_PROT_READ | VM_PROT_WRITE, MAP_PRIVATE | MAP_ANON, (uint64_t)-1, 0);
+    if (!_trojanMem || _trojanMem == UINT64_MAX) {
+        _trojanMem = 0;
+        [self destroyRemoteCall];
+        return -1;
+    }
     
     RemoteArbCallWithTimeout(100, self, memset, _trojanMem, 0, PAGE_SIZE);
     
@@ -758,10 +1188,14 @@ - (int)initRemoteCallForProcess:(const char *)process useMigFilterBypass:(BOOL)u
 
 - (instancetype)initWithProcess:(NSString *)process useMigFilterBypass:(BOOL)useMigFilterBypass {
     self = [super init];
-    if ([self initRemoteCallForProcess:process.UTF8String useMigFilterBypass:useMigFilterBypass]) {
+    const char *processName = process.UTF8String;
+    int rc = [self initRemoteCallForProcess:processName useMigFilterBypass:useMigFilterBypass];
+    if (rc) {
         NSLog(@"(rc) initRemoteCallForProcess failed");
+        g_rc_last_init_error = self.lastError ?: @"RemoteCall init failed";
         return nil;
     }
+    g_rc_last_init_error = nil;
     return self;
 }
 
@@ -869,4 +1303,4 @@ uint64_t remote_NSString(RemoteCall *proc, const char *str) {
     uint64_t result = remote_msg(proc, cls_NSString, sel_stringWithCString, resultCStr, 0, 0, 0);
     RemoteArbCall(proc, free, resultCStr);
     return result;
-}
+}
\ No newline at end of file
  ```

  </details>
- 🟢 `lara/kexploit/homescreen.h` (added) — +6 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,6 @@
+#ifndef homescreen_h
+#define homescreen_h
+#import "RemoteCall.h"
+
+bool patch_homescreen_grid(RemoteCall *proc, int cols, int rows);
+#endif
\ No newline at end of file
  ```

  </details>
- 🟢 `lara/kexploit/homescreen.m` (added) — +93 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,93 @@
+#import "RemoteCall.h"
+
+bool patch_homescreen_grid(RemoteCall *proc, int cols, int rows) {
+    if (!proc) return false;
+
+    uint64_t selShared      = remote_sel(proc, "sharedInstance");
+    uint64_t selIconMgr     = remote_sel(proc, "iconManager");
+    uint64_t selProvider    = remote_sel(proc, "listLayoutProvider");
+    uint64_t selLayout      = remote_sel(proc, "layoutForIconLocation:");
+    uint64_t selLayoutCfg   = remote_sel(proc, "layoutConfiguration");
+    uint64_t selSetCols     = remote_sel(proc, "setNumberOfPortraitColumns:");
+    uint64_t selSetRows     = remote_sel(proc, "setNumberOfPortraitRows:");
+    uint64_t selSetLandCols = remote_sel(proc, "setNumberOfLandscapeColumns:");
+    uint64_t selSetLandRows = remote_sel(proc, "setNumberOfLandscapeRows:");
+    uint64_t selRootFolder  = remote_sel(proc, "rootFolderController");
+    uint64_t selViewCount   = remote_sel(proc, "iconListViewCount");
+    uint64_t selViewAtIndex = remote_sel(proc, "iconListViewAtIndex:");
+    uint64_t selDockLV      = remote_sel(proc, "dockListView");
+    uint64_t selSetAutoFit  = remote_sel(proc, "setAutomaticallyAdjustsLayoutMetricsToFit:");
+    uint64_t selGridGet     = remote_sel(proc, "gridSize");
+    uint64_t selGridSet     = remote_sel(proc, "setGridSize:");
+    uint64_t selModel       = remote_sel(proc, "model");
+    uint64_t selModel2      = remote_sel(proc, "iconListModel");
+
+    // Step 1: SBIconController -> iconManager
+    uint64_t cls = remote_getClass(proc, "SBIconController");
+    if (!cls) return false;
+    uint64_t ctrl = remote_msg(proc, cls, selShared, 0, 0, 0, 0);
+    if (!ctrl) return false;
+    uint64_t mgr = remote_msg(proc, ctrl, selIconMgr, 0, 0, 0, 0);
+    if (!mgr) return false;
+
+    // Step 2: patch layout config
+    uint64_t provider = remote_msg(proc, mgr, selProvider, 0, 0, 0, 0);
+    if (provider) {
+        uint64_t locStr = remote_NSString(proc, "SBIconLocationRoot");
+        if (locStr) {
+            uint64_t layout = remote_msg(proc, provider, selLayout, locStr, 0, 0, 0);
+            if (layout) {
+                uint64_t cfg = remote_msg(proc, layout, selLayoutCfg, 0, 0, 0, 0);
+                if (cfg) {
+                    remote_msg(proc, cfg, selSetCols,     (uint64_t)cols, 0, 0, 0);
+                    remote_msg(proc, cfg, selSetRows,     (uint64_t)rows, 0, 0, 0);
+                    remote_msg(proc, cfg, selSetLandCols, (uint64_t)rows, 0, 0, 0);
+                    remote_msg(proc, cfg, selSetLandRows, (uint64_t)cols, 0, 0, 0);
+                    printf("(rc) layout config patched\n");
+                }
+            }
+        }
+    }
+
+    // Step 3: rootFolderController -> list views
+    uint64_t rootFolder = remote_msg(proc, mgr, selRootFolder, 0, 0, 0, 0);
+    if (!rootFolder) return false;
+
+    uint64_t newGrid = (((uint64_t)rows & 0xFFFF) << 16) | ((uint64_t)cols & 0xFFFF);
+
+    uint64_t viewCount = remote_msg(proc, rootFolder, selViewCount, 0, 0, 0, 0);
+    if (viewCount > 64) viewCount = 64;
+
+    for (uint64_t i = 0; i < viewCount; i++) {
+        uint64_t lv = remote_msg(proc, rootFolder, selViewAtIndex, i, 0, 0, 0);
+        if (!lv) continue;
+
+        // disable auto fit
+        remote_msg(proc, lv, selSetAutoFit, 0, 0, 0, 0);
+
+        // patch gridSize on the page model
+        uint64_t pageModel = remote_msg(proc, lv, selModel, 0, 0, 0, 0);
+        if (!pageModel) pageModel = remote_msg(proc, lv, selModel2, 0, 0, 0, 0);
+        if (pageModel) {
+            uint64_t oldGrid = remote_msg(proc, pageModel, selGridGet, 0, 0, 0, 0) & 0xFFFFFFFF;
+            remote_msg(proc, pageModel, selGridSet, newGrid, 0, 0, 0);
+            printf("(rc) page %llu model gridSize 0x%llx -> 0x%llx\n", i, oldGrid, newGrid);
+        }
+    }
+
+    // Step 4: dock
+    uint64_t dockLV = remote_msg(proc, mgr, selDockLV, 0, 0, 0, 0);
+    if (dockLV) {
+        remote_msg(proc, dockLV, selSetAutoFit, 0, 0, 0, 0);
+        uint64_t dockModel = remote_msg(proc, dockLV, selModel, 0, 0, 0, 0);
+        if (!dockModel) dockModel = remote_msg(proc, dockLV, selModel2, 0, 0, 0, 0);
+        if (dockModel) {
+            uint64_t oldGrid = remote_msg(proc, dockModel, selGridGet, 0, 0, 0, 0) & 0xFFFFFFFF;
+            remote_msg(proc, dockModel, selGridSet, newGrid, 0, 0, 0);
+            printf("(rc) dock model gridSize 0x%llx -> 0x%llx\n", oldGrid, newGrid);
+        }
+    }
+
+    printf("(rc) patch_homescreen_grid done\n");
+    return true;
+}
\ No newline at end of file
  ```

  </details>
- 🟡 `lara/kexploit/offsets.m` (modified) — +5 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -300,7 +300,11 @@ void offsets_init(void) {
                             cpuFamily == CPUFAMILY_ARM_IBIZA ||  // M3
                             cpuFamily == CPUFAMILY_ARM_DONAN); // M4
     
-    
+    uint64_t expected_t1sz_boot = (isA16Above || isMSeriesIpad) ? 0x11 : 0x19;
+    if (t1sz_boot != expected_t1sz_boot) {
+        t1sz_boot = expected_t1sz_boot;
+    }
+
     gIsPACSupported = is_pac_supported();
     
     if (SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"17.0")) {
  ```

  </details>
- 🟡 `lara/lara-Bridging-Header.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -14,6 +14,7 @@
 #import "sbx.h"
 #import "rc.h"
 #import "RemoteCall.h"
+#import "homescreen.h"
 
 long FindCacheDataOffset(const char *mgKey);
 
  ```

  </details>
- 🟡 `lara/views/ContentView.swift` (modified) — +4 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -507,10 +507,13 @@ struct ContentView: View {
                     } header: {
                         Text("RemoteCall")
                     } footer: {
-                        if let error = mgr.sbProc?.lastError {
+                        if let error = mgr.rcLastError ?? mgr.sbProc?.lastError {
                             Text("RemoteCall error: \(error)")
                                 .foregroundColor(.red)
                         }
+                        if RemoteCall.isLiveContainerRuntime() && !RemoteCall.isLiveProcessRuntime() {
+                            Text("RemoteCall needs a PAC-enabled LiveContainer launch context. The main exploit may still work when RemoteCall is unavailable.")
+                        }
                         if isdebugged() {
                             Text("Not available when a debugger is attached.")
                         }
  ```

  </details>
- 🟡 `lara/views/RemoteView.swift` (modified) — +34 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -21,6 +21,8 @@ struct RemoteView: View {
     @State private var customTimeoutMs: Int = 100
     @State private var customMigBypass: Bool = false
     @State private var customLastResult: String = ""
+    @State private var hsRows: Int = 6
+    @State private var hsColumns: Int = 4
 
     private var dockMaxColumns: Int { rcdockunlimited ? 50 : 10 }
 
@@ -58,6 +60,38 @@ struct RemoteView: View {
                 Text("SpringBoard")
             }
 
+            Section {
+                Stepper(value: $hsColumns, in: 1...10) {
+                    HStack {
+                        Text("Home screen columns")
+                        Spacer()
+                        Text("\(hsColumns)")
+                            .foregroundColor(.secondary)
+                            .monospacedDigit()
+                    }
+                }
+                
+                Stepper(value: $hsRows, in: 1...10) {
+                    HStack {
+                        Text("Home screen rows")
+                        Spacer()
+                        Text("\(hsRows)")
+                            .foregroundColor(.secondary)
+                            .monospacedDigit()
+                    }
+                }
+
+                Button {
+                    run("Patch Home Screen Grid \(hsColumns)x\(hsRows)") {
+                        return patch_homescreen_grid(mgr.sbProc, Int32(hsColumns), Int32(hsRows))
+                            ? "patch_homescreen_grid(\(hsColumns), \(hsRows)) -> ok"
+                            : "patch_homescreen_grid(\(hsColumns), \(hsRows)) -> failed"
+                    }
+                } label: {
+                    Text("Apply Home Screen Grid")
+                }
+            }
+
             Section {
                 Stepper(value: $columns, in: 1...dockMaxColumns) {
                     HStack {
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 7c9ea81: SBRowsAndColums: Fix RemoteCall crash & persistance (credits to m1337v)
- : 📝 Commit 8112b74: SBRowsAndColums: Add SpringBoard custom rows & columns (inspired from Lightsaber by zeroxjf)
- Apr 26, 2026: ✏️ @xelahot renamed: "Add RemoteCall feature: custom rows and columns + fix RemoteCall crash" → "Add RemoteCall feature: custom rows and columns + fix RemoteCall crashes"
- : 📝 Commit 9cab18c: SBRowsAndColums: Change the order of the feature and remove header
- Apr 26, 2026: 🟣 @rooootdev merged
- Apr 26, 2026: 🔴 Closed by @rooootdev
- Apr 26, 2026: 🗑️ @xelahot deleted head branch


---

## Comments (2)

### Comment #1 — [@Rupeshkaadyan](https://github.com/Rupeshkaadyan) · Apr 27, 2026

`ID: 4323305338`


nice


👍 1


---

### Comment #2 — [@heinekenpsx](https://github.com/heinekenpsx) · Apr 27, 2026

`ID: 4323691785`


Nice, but the default app icon seems too big so they are a bit close to each other, if the app icon can be resized, it will be better


---

*Exported on 10/14/2026, 3:02:57 PM from [https://github.com/rooootdev/lara/pull/199](https://github.com/rooootdev/lara/pull/199)*
