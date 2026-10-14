# Add Keychain Reader Tweak

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#311](https://github.com/rooootdev/lara/pull/311)  
> **State:** 🟢 open  
> **Branch:** `DYY-Studio:keychain-reader` → `rooootdev:main`  
> **Changes:** +2005 / -42 across 8 files  
> **Created:** Jul 9, 2026  
> **Updated:** Jul 10, 2026  

---

## Description

*Opened by [@DYY-Studio](https://github.com/DYY-Studio)* *on Jul 9, 2026*


Issue #309

* New **Keychain Reader** Tweak: Use RemoteCall to read Keychain of user-selected app. Support JSON export.
    * Remote `SecItemCopyMatching` 
    * Remote `NSPropertyListSerialization`
    * Copy serialized data to Lara
* Add `NO_PTRAUTH` flag check to RemoteCall.
    * Avoid PAC signing when has `NO_PTRAUTH` flag 
    * I don't know how to modify `doRemoteCallSyncOnMainThread:`, as there have not any connection to thread state.

It seems that most of fails when RemoteCall to common apps are caused by having `NO_PTRAUTH` but still sign PAC.

I manually write basic ObjC logic and a simple UI modified from `DecryptView.swift`.
Codex helps clean code, fix bugs and complete the UI.

I've tested that this can work on `iPhone SE (3rd Gen) / iPhone14,6`, `iOS 17.7.2 (21H221)`. Basiclly stable.

More test is needed. I only has this device...


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (17)

- [`611eded`](https://github.com/rooootdev/lara/commit/611eded52b1e17da1a9ffdf63ac5e8da719aa898) in-dev — *QZ Yan* · Jul 8, 2026
- [`ab5fdfc`](https://github.com/rooootdev/lara/commit/ab5fdfce457af8bc31a1301bac35412902e4cae0) fix — *QZ Yan* · Jul 8, 2026
- [`11630f6`](https://github.com/rooootdev/lara/commit/11630f670af47aea60488be3e843b499621db5ab) test cache — *QZ Yan* · Jul 9, 2026
- [`889f3be`](https://github.com/rooootdev/lara/commit/889f3be8221d48ed860616587433f349f34b5442) fix pac strategy — *QZ Yan* · Jul 9, 2026
- [`cf0a55c`](https://github.com/rooootdev/lara/commit/cf0a55c2013eb78c38b33228b390939467d7d2b6) backend workable — *QZ Yan* · Jul 9, 2026
- [`411201c`](https://github.com/rooootdev/lara/commit/411201ced8b4531071d9ab2594b8c67820077f3d) full function UI — *QZ Yan* · Jul 9, 2026
- [`4c66b06`](https://github.com/rooootdev/lara/commit/4c66b06b1018efb1dc775361e8a003901afe873b) move objc lifetime control sel to keychain — *QZ Yan* · Jul 9, 2026
- [`76adae4`](https://github.com/rooootdev/lara/commit/76adae4559a11de79042efd2c4302deec094ecce) Add warning and fix crash — *QZ Yan* · Jul 9, 2026
- [`a870fde`](https://github.com/rooootdev/lara/commit/a870fdee1dafbe96f29fc9ed875b87c8679e585e) ObjC code clean and enhance exception handling — *QZ Yan* · Jul 9, 2026
- [`b359682`](https://github.com/rooootdev/lara/commit/b359682d8e7c59fde3cceb1669d623d8506bb778) sign — *QZ Yan* · Jul 9, 2026
- [`ecb1c33`](https://github.com/rooootdev/lara/commit/ecb1c3345c29f7a57c11a4c4d3b84abde4eb4c22) seems no need to use misaligned — *QZ Yan* · Jul 9, 2026
- [`ed69dc5`](https://github.com/rooootdev/lara/commit/ed69dc561b384d2376cca307d4983144c6260e4d) clean header — *QZ Yan* · Jul 9, 2026
- [`4d70c1f`](https://github.com/rooootdev/lara/commit/4d70c1f58bb2043789dd8f3bd9bc3bd3acfbf592) Update Readme.md — *QZ Yan* · Jul 9, 2026
- [`ad2136a`](https://github.com/rooootdev/lara/commit/ad2136a758796c77aeff6819e33c198ac2913c90) Only allow App Store and self-signed apps, block self RemoteCall. — *QZ Yan* · Jul 9, 2026
- [`62f1f23`](https://github.com/rooootdev/lara/commit/62f1f23e2ba0f3a3ee85278bd33cbfeb7530b434) (empty) instead of (empty file) — *QZ Yan* · Jul 9, 2026
- [`1854739`](https://github.com/rooootdev/lara/commit/1854739a3393980f8ad660411f0a5058fe2fa840) add more infomation — *QZ Yan* · Jul 9, 2026
- [`e6cb7bd`](https://github.com/rooootdev/lara/commit/e6cb7bd891e6c01e9e05c25fbe076e22a116d906) little fix — *QZ Yan* · Jul 10, 2026


---

## Changed Files (8)

- 🟡 `README.md` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -91,6 +91,7 @@ Important Notes:
 - OTA Update Disabler
 - Screen Time Disabler 
 - App Decrypt
+- Keychain Reader
 
 
 ### Coming Soon
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.h` (modified) — +3 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -22,6 +22,7 @@
 
 // xnu-10002.81.5/osfmk/mach/arm/_structs.h
 #define __DARWIN_ARM_THREAD_STATE64_USER_DIVERSIFIER_MASK 0xff000000
+#define __DARWIN_ARM_THREAD_STATE64_FLAGS_NO_PTRAUTH 0x1
 #define __DARWIN_ARM_THREAD_STATE64_FLAGS_IB_SIGNED_LR 0x2
 #define __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_PC 0x4
 #define __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_LR 0x8
@@ -137,7 +138,9 @@ int disable_excguard_kill(uint64_t task);
 #define RemoteArbCall(instance, _pc, ...) RemoteArbCallWithTimeout(5, instance, _pc, __VA_ARGS__)
 
 uint64_t remote_alloc_str(RemoteCall *proc, const char *str);
+uint64_t remote_alloc_ref(RemoteCall *proc, uint64_t pointer);
 uint64_t remote_sel(RemoteCall *proc, const char *name);
+uint64_t remote_dlsym(RemoteCall *proc, const char *name);
 uint64_t remote_getClass(RemoteCall *proc, const char *name);
 uint64_t remote_msg(RemoteCall *proc, uint64_t obj, uint64_t sel, uint64_t a0, uint64_t a1, uint64_t a2, uint64_t a3);
 int remote_errno(RemoteCall *proc);
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.m` (modified) — +84 / -42
  <details><summary>Diff</summary>

  ```diff
  @@ -543,52 +543,57 @@ - (BOOL)setExceptionPortOnThread:(mach_port_t)exceptionPort forThread:(uint64_t)
 - (void)signState:(uint64_t)signingThread withState:(arm_thread_state64_internal *)state pc:(uint64_t)pc lr:(uint64_t)lr
 {
     if(is_pac_supported()) {
-        uint64_t diver = 0;
-        diver = (uint64_t)state->__flags & __DARWIN_ARM_THREAD_STATE64_USER_DIVERSIFIER_MASK;
-        uint64_t discPC = ptrauthblend(diver, ptrauthstrdisc("pc"));
-        uint64_t discLR = ptrauthblend(diver, ptrauthstrdisc("lr"));
         uint64_t strippedPC = nativestrip(pc);
         uint64_t strippedLR = nativestrip(lr);
-        uint64_t signedPC = 0;
-        uint64_t signedLR = 0;
-        
-        if (pc) {
-            signedPC = remotepac(signingThread, pc, discPC);
-            if (!signedPC || signedPC == UINT64_MAX) {
-                signedPC = strippedPC;
+        if (!(state->__flags & __DARWIN_ARM_THREAD_STATE64_FLAGS_NO_PTRAUTH)) {
+            uint64_t diver = 0;
+            diver = (uint64_t)state->__flags & __DARWIN_ARM_THREAD_STATE64_USER_DIVERSIFIER_MASK;
+            uint64_t discPC = ptrauthblend(diver, ptrauthstrdisc("pc"));
+            uint64_t discLR = ptrauthblend(diver, ptrauthstrdisc("lr"));
+            uint64_t signedPC = 0;
+            uint64_t signedLR = 0;
+            
+            if (pc) {
+                signedPC = remotepac(signingThread, pc, discPC);
+                if (!signedPC || signedPC == UINT64_MAX) {
+                    signedPC = strippedPC;
+                }
             }
-        }
-        if (lr) {
-            signedLR = remotepac(signingThread, lr, discLR);
-            if (!signedLR || signedLR == UINT64_MAX) {
-                signedLR = strippedLR;
+            if (lr) {
+                signedLR = remotepac(signingThread, lr, discLR);
+                if (!signedLR || signedLR == UINT64_MAX) {
+                    signedLR = strippedLR;
+                }
             }
-        }
 
-        printf("(rc) signState: thread=0x%llx flags_before=0x%x diver=0x%llx pc_raw=0x%llx pc_signed=0x%llx lr_raw=0x%llx lr_signed=0x%llx\n",
-               signingThread,
-               state->__flags,
-               diver,
-               strippedPC,
-               signedPC,
-               strippedLR,
-               signedLR);
-        fflush(stdout);
-        rc_ios16_log_return_path_thread(signingThread, "signState");
-
-        uint32_t flags = state->__flags;
-        flags &= ~(__DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_PC |
-                   __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_LR |
-                   __DARWIN_ARM_THREAD_STATE64_FLAGS_IB_SIGNED_LR);
-        state->__flags = flags;
-        if (pc) state->__pc = signedPC;
-        if (lr) state->__lr = signedLR;
-        if (rc_is_ios16()) {
-            RC_IOS16_DEBUG_LOG("(rc.iOS16) signState flags after clear: flags_after=0x%x pc_set=%d lr_set=%d\n",
-                   state->__flags,
-                   pc != 0,
-                   lr != 0);
+            printf("(rc) signState: thread=0x%llx flags_before=0x%x diver=0x%llx pc_raw=0x%llx pc_signed=0x%llx lr_raw=0x%llx lr_signed=0x%llx\n",
+                signingThread,
+                state->__flags,
+                diver,
+                strippedPC,
+                signedPC,
+                strippedLR,
+                signedLR);
             fflush(stdout);
+            rc_ios16_log_return_path_thread(signingThread, "signState");
+
+            uint32_t flags = state->__flags;
+            flags &= ~(__DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_PC |
+                    __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_LR |
+                    __DARWIN_ARM_THREAD_STATE64_FLAGS_IB_SIGNED_LR);
+            state->__flags = flags;
+            if (pc) state->__pc = signedPC;
+            if (lr) state->__lr = signedLR;
+            if (rc_is_ios16()) {
+                RC_IOS16_DEBUG_LOG("(rc.iOS16) signState flags after clear: flags_after=0x%x pc_set=%d lr_set=%d\n",
+                    state->__flags,
+                    pc != 0,
+                    lr != 0);
+                fflush(stdout);
+            }
+        } else {
+            if (pc) state->__pc = strippedPC;
+            if (lr) state->__lr = strippedLR;
         }
         return;
     }
@@ -1427,7 +1432,7 @@ - (int)initRemoteCallForProcess:(const char *)process useMigFilterBypass:(BOOL)u
     }
     
     uint64_t threadStartTrap = FAKE_PC_TROJAN;
-    uint64_t remoteCrashSigned = remotepac(_trojanThreadAddr, threadStartTrap, 0);
+    uint64_t remoteCrashSigned = (exc2.threadState.__flags & __DARWIN_ARM_THREAD_STATE64_FLAGS_NO_PTRAUTH) ? threadStartTrap : remotepac(_trojanThreadAddr, threadStartTrap, 0);
     printf("(rc) remoteCrashSigned: 0x%llx\n", remoteCrashSigned);
     fflush(stdout);
     if (!remoteCrashSigned) {
@@ -1656,6 +1661,34 @@ uint64_t remote_alloc_str(RemoteCall *proc, const char *str) {
     return buf;
 }
 
+uint64_t remote_alloc_ref(RemoteCall *proc, uint64_t pointer) {
+    uint64_t len = sizeof(uint64_t);
+    if (proc.trojanMemIsStackFallback) {
+        uint64_t offset = (proc.trojanMemScratchOffset + 7) & ~7ULL;
+        if (offset + len > PAGE_SIZE) {
+            RC_IOS16_DEBUG_LOG("(rc.iOS16) stack fallback remote_alloc_ref overflow len=0x%llx offset=0x%llx\n", len, offset);
+            fflush(stdout);
+            return 0;
+        }
+        uint64_t buf = proc.trojanMem + offset;
+        if (![proc remote_write64:buf value:pointer]) {
+            RC_IOS16_DEBUG_LOG("(rc.iOS16) stack fallback remote_alloc_ref write failed buf=0x%llx len=0x%llx\n", buf, len);
+            fflush(stdout);
+            return 0;
+        }
+        proc.trojanMemScratchOffset = offset + len;
+        RC_IOS16_DEBUG_LOG("(rc.iOS16) stack fallback remote_alloc_ref buf=0x%llx len=0x%llx next=0x%llx pointer=0x%llx\n",
+               buf, len, proc.trojanMemScratchOffset, pointer);
+        fflush(stdout);
+        return buf;
+    }
+    uint64_t buf = RemoteArbCall(proc, malloc, len);
+    if (buf) {
+        proc[buf].value64 = pointer;
+    }
+    return buf;
+}
+
 uint64_t remote_sel(RemoteCall *proc, const char *name) {
     uint64_t str = remote_alloc_str(proc, name);
     uint64_t sel = RemoteArbCall(proc, sel_registerName, str);
@@ -1665,6 +1698,15 @@ uint64_t remote_sel(RemoteCall *proc, const char *name) {
     return sel;
 }
 
+uint64_t remote_dlsym(RemoteCall *proc, const char* name) {
+    uint64_t str = remote_alloc_str(proc, name);
+    uint64_t sym = RemoteArbCall(proc, dlsym, RTLD_DEFAULT, str);
+    if (!proc.trojanMemIsStackFallback) {
+        RemoteArbCall(proc, free, str);
+    }
+    return sym;
+}
+
 uint64_t remote_getClass(RemoteCall *proc, const char *name) {
     uint64_t str = remote_alloc_str(proc, name);
     uint64_t cls = RemoteArbCall(proc, objc_getClass, str);
@@ -3714,7 +3756,7 @@ - (int)initRemoteCallForProcessIOS16:(const char *)process useMigFilterBypass:(B
     }
 
     uint64_t threadStartTrap = FAKE_PC_TROJAN;
-    uint64_t remoteCrashSigned = remotepac(_trojanThreadAddr, threadStartTrap, 0);
+    uint64_t remoteCrashSigned = (parkState.__flags & __DARWIN_ARM_THREAD_STATE64_FLAGS_NO_PTRAUTH) ? threadStartTrap : remotepac(_trojanThreadAddr, threadStartTrap, 0);
     printf("(rc) remoteCrashSigned: 0x%llx\n", remoteCrashSigned);
     fflush(stdout);
     if (!remoteCrashSigned) {
  ```

  </details>
- 🟢 `lara/kexploit/keychain.h` (added) — +54 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,54 @@
+//
+//  keychain.h
+//  lara
+//
+//  Created by yyfll on 08.07.26.
+//
+
+#ifndef keychain_h
+#define keychain_h
+
+#include <stdint.h>
+#include <stdbool.h>
+#import <Foundation/Foundation.h>
+#import "RemoteCall.h"
+
+typedef void (*log_callback_t)(const char * _Nullable message);
+
+void set_log_callback_kc(log_callback_t _Nullable callback);
+
+typedef struct {
+    uint64_t SecItemCopyMatching;
+
+    uint64_t kSecClass;
+    uint64_t kSecClassGenericPassword;
+    uint64_t kSecClassInternetPassword;
+    uint64_t kSecClassCertificate;
+    uint64_t kSecClassKey;
+    uint64_t kSecClassIdentity;
+
+    uint64_t kCFBooleanTrue;
+    uint64_t kCFBooleanFalse;
+
+    uint64_t kSecReturnAttributes;
+    uint64_t kSecReturnData;
+
+    uint64_t kSecMatchLimit;
+    uint64_t kSecMatchLimitAll;
+    uint64_t kSecMatchLimitOne;
+
+    uint64_t kSecAttrAccessControl;
+} remote_sec_symbols;
+
+typedef NS_ENUM(NSUInteger, sec_class) {
+    kSCGenericPassword = 0,
+    kSCInternetPassword,
+    kSCCertificate,
+    kSCKey,
+    kSCIdentity
+};
+
+bool find_secitem_symbols(RemoteCall * _Nonnull proc, remote_sec_symbols * _Nonnull symbols);
+NSArray<NSDictionary *> * _Nullable get_secitems(RemoteCall * _Nonnull proc, remote_sec_symbols * _Nonnull symbols, sec_class target_sec_class, bool get_data);
+
+#endif /* keychain_h */
  ```

  </details>
- 🟢 `lara/kexploit/keychain.m` (added) — +615 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,615 @@
+//
+//  keychain.m
+//  lara
+//
+//  Created by yyfll on 08.07.26.
+//
+
+#import "keychain.h"
+
+#import <Foundation/Foundation.h>
+#import <dlfcn.h>
+#import <stdio.h>
+#import <stdlib.h>
+#import <string.h>
+
+typedef struct {
+    uint64_t sel_retain;
+    uint64_t sel_release;
+} remote_objc_lifetimes;
+
+static log_callback_t g_log_cb_kc = NULL;
+
+static const int32_t kSecItemNotFoundStatus = -25300;
+static const char *kSecurityFrameworkPath = "/System/Library/Frameworks/Security.framework/Security";
+
+void set_log_callback_kc(log_callback_t callback) {
+    g_log_cb_kc = callback;
+}
+
+static void logmsg(const char *msg) {
+    const char *safeMessage = msg ? msg : "(null)";
+    if (g_log_cb_kc) {
+        g_log_cb_kc(safeMessage);
+    }
+    printf("(keychain) %s\n", safeMessage);
+}
+
+static void log_status_message(const char *prefix, int32_t status) {
+    char msg[160];
+    snprintf(msg, sizeof(msg), "%s (%d)", prefix, status);
+    logmsg(msg);
+}
+
+static bool find_objc_lifetimes(RemoteCall *proc, remote_objc_lifetimes *lifetimes) {
+    if (!proc || !lifetimes) {
+        return false;
+    }
+
+    *lifetimes = (remote_objc_lifetimes){0};
+    lifetimes->sel_retain = remote_sel(proc, "retain");
+    lifetimes->sel_release = remote_sel(proc, "release");
+    return lifetimes->sel_retain != 0 && lifetimes->sel_release != 0;
+}
+
+static uint64_t remote_retain(RemoteCall *proc, const remote_objc_lifetimes *lifetimes, uint64_t remoteObj) {
+    if (!proc || !lifetimes || !lifetimes->sel_retain || !remoteObj) {
+        return 0;
+    }
+    return remote_msg(proc, remoteObj, lifetimes->sel_retain, 0, 0, 0, 0);
+}
+
+static void remote_release(RemoteCall *proc, const remote_objc_lifetimes *lifetimes, uint64_t remoteObj) {
+    if (!proc || !lifetimes || !lifetimes->sel_release || !remoteObj) {
+        return;
+    }
+    remote_msg(proc, remoteObj, lifetimes->sel_release, 0, 0, 0, 0);
+}
+
+static bool remote_load_library(RemoteCall *proc, const char *path) {
+    if (!proc || !path) {
+        return false;
+    }
+
+    uint64_t remotePath = remote_alloc_str(proc, path);
+    if (!remotePath) {
+        return false;
+    }
+
+    uint64_t handle = RemoteArbCall(proc, dlopen, remotePath, RTLD_LAZY | RTLD_GLOBAL);
+    if (!proc.trojanMemIsStackFallback) {
+        RemoteArbCall(proc, free, remotePath);
+    }
+    return handle != 0;
+}
+
+static uint64_t remote_resolve_object_symbol(RemoteCall *proc, const char *name) {
+    if (!proc || !name) {
+        return 0;
+    }
+
+    uint64_t symbolSlot = remote_dlsym(proc, name);
+    if (!symbolSlot) {
+        return 0;
+    }
+
+    uint64_t tmp = proc.trojanMem;
+    RemoteArbCall(proc, memcpy, tmp, symbolSlot, 8);
+    return [proc remoteRead64From:tmp];
+}
+
+static bool resolve_function_symbol(RemoteCall *proc, const char *name, uint64_t *symbol, bool logFailures) {
+    if (!proc || !name || !symbol) {
+        return false;
+    }
+
+    *symbol = remote_dlsym(proc, name);
+    if (*symbol) {
+        return true;
+    }
+
+    if (logFailures) {
+        char msg[160];
+        snprintf(msg, sizeof(msg), "failed to resolve %s", name);
+        logmsg(msg);
+    }
+    return false;
+}
+
+static bool resolve_object_symbol(RemoteCall *proc, const char *name, uint64_t *symbol, bool logFailures) {
+    if (!proc || !name || !symbol) {
+        return false;
+    }
+
+    *symbol = remote_resolve_object_symbol(proc, name);
+    if (*symbol) {
+        return true;
+    }
+
+    if (logFailures) {
+        char msg[160];
+        snprintf(msg, sizeof(msg), "failed to resolve %s", name);
+        logmsg(msg);
+    }
+    return false;
+}
+
+static bool resolve_secitem_symbols(RemoteCall *proc, remote_sec_symbols *symbols, bool logFailures) {
+    if (!proc || !symbols) {
+        return false;
+    }
+
+    *symbols = (remote_sec_symbols){0};
+
+#define RESOLVE_FUNCTION(field, symbolName) \
+    do { \
+        if (!resolve_function_symbol(proc, symbolName, &symbols->field, logFailures)) { \
+            return false; \
+        } \
+    } while (0)
+
+#define RESOLVE_OBJECT(field, symbolName) \
+    do { \
+        if (!resolve_object_symbol(proc, symbolName, &symbols->field, logFailures)) { \
+            return false; \
+        } \
+    } while (0)
+
+    RESOLVE_FUNCTION(SecItemCopyMatching, "SecItemCopyMatching");
+
+    RESOLVE_OBJECT(kSecClass, "kSecClass");
+    RESOLVE_OBJECT(kSecClassGenericPassword, "kSecClassGenericPassword");
+    RESOLVE_OBJECT(kSecClassInternetPassword, "kSecClassInternetPassword");
+    RESOLVE_OBJECT(kSecClassCertificate, "kSecClassCertificate");
+    RESOLVE_OBJECT(kSecClassKey, "kSecClassKey");
+    RESOLVE_OBJECT(kSecClassIdentity, "kSecClassIdentity");
+
+    RESOLVE_OBJECT(kCFBooleanTrue, "kCFBooleanTrue");
+    RESOLVE_OBJECT(kCFBooleanFalse, "kCFBooleanFalse");
+
+    RESOLVE_OBJECT(kSecReturnAttributes, "kSecReturnAttributes");
+    RESOLVE_OBJECT(kSecReturnData, "kSecReturnData");
+
+    RESOLVE_OBJECT(kSecMatchLimit, "kSecMatchLimit");
+    RESOLVE_OBJECT(kSecMatchLimitAll, "kSecMatchLimitAll");
+    RESOLVE_OBJECT(kSecMatchLimitOne, "kSecMatchLimitOne");
+
+    RESOLVE_OBJECT(kSecAttrAccessControl, "kSecAttrAccessControl");
+
+#undef RESOLVE_FUNCTION
+#undef RESOLVE_OBJECT
+
+    return true;
+}
+
+static void remote_description(RemoteCall *proc, const remote_objc_lifetimes *lifetimes, uint64_t remoteObj) {
+    if (!proc || !remoteObj) {
+        return;
+    }
+
+    uint64_t sel_description = remote_sel(proc, "description");
+    uint64_t sel_UTF8String = remote_sel(proc, "UTF8String");
+    if (!sel_description || !sel_UTF8String) {
+        return;
+    }
+
+    uint64_t remoteDesc = remote_msg(proc, remoteObj, sel_description, 0, 0, 0, 0);
+    if (!remoteDesc) {
+        return;
+    }
+
+    uint64_t retainedDesc = remote_retain(proc, lifetimes, remoteDesc);
+    if (!retainedDesc) {
+        return;
+    }
+
+    uint64_t remoteStr = remote_msg(proc, retainedDesc, sel_UTF8String, 0, 0, 0, 0);
+    if (!remoteStr) {
+        remote_release(proc, lifetimes, retainedDesc);
+        return;
+    }
+
+    size_t len = (size_t)RemoteArbCall(proc, strlen, remoteStr);
+    if (len == 0) {
+        remote_release(proc, lifetimes, retainedDesc);
+        return;
+    }
+
+    char *desc = calloc(len + 1, sizeof(char));
+    if (!desc) {
+        remote_release(proc, lifetimes, retainedDesc);
+        logmsg("failed to allocate remote description buffer");
+        return;
+    }
+
+    if ([proc remoteRead:remoteStr to:desc size:len]) {
+        desc[len] = '\0';
+        logmsg(desc);
+    } else {
+        logmsg("failed to read remote description");
+    }
+
+    free(desc);
+    remote_release(proc, lifetimes, retainedDesc);
+}
+
+static NSData *copy_remote_nsdata(RemoteCall *proc, uint64_t remoteData) {
+    if (!proc || !remoteData) {
+        return nil;
+    }
+
+    uint64_t sel_length = remote_sel(proc, "length");
+    uint64_t sel_bytes = remote_sel(proc, "bytes");
+    if (!sel_length || !sel_bytes) {
+        return nil;
+    }
+
+    uint64_t dataLength = remote_msg(proc, remoteData, sel_length, 0, 0, 0, 0);
+    if (dataLength == 0) {
+        return [NSData data];
+    }
+
+    uint64_t bytesPtr = remote_msg(proc, remoteData, sel_bytes, 0, 0, 0, 0);
+    if (!bytesPtr) {
+        logmsg("failed to get bytes from remote NSData");
+        return nil;
+    }
+
+    void *buffer = malloc((size_t)dataLength);
+    if (!buffer) {
+        logmsg("failed to allocate local buffer for remote NSData");
+        return nil;
+    }
+
+    NSData *data = nil;
+    if ([proc remoteRead:bytesPtr to:buffer size:dataLength]) {
+        data = [NSData dataWithBytes:buffer length:(NSUInteger)dataLength];
+    } else {
+        logmsg("failed to read remote NSData bytes");
+    }
+
+    free(buffer);
+    return data;
+}
+
+static NSData *copy_remote_property_list_data(RemoteCall *proc, const remote_objc_lifetimes *lifetimes, uint64_t remotePropertyList) {
+    if (!proc || !lifetimes || !remotePropertyList) {
+        return nil;
+    }
+
+    NSData *plistData = nil;
+    uint64_t errorRef = 0;
+    uint64_t remoteData = 0;
+    uint64_t retainedData = 0;
+
+    uint64_t cls_NSPropertyListSerialization = remote_getClass(proc, "NSPropertyListSerialization");
+    uint64_t sel_dataWithPropertyList = remote_sel(proc, "dataWithPropertyList:format:options:error:");
+    if (!cls_NSPropertyListSerialization || !sel_dataWithPropertyList) {
+        return nil;
+    }
+
+    errorRef = remote_alloc_ref(proc, 0);
+    if (!errorRef) {
+        logmsg("failed to allocate remote serialization error pointer");
+        goto cleanup;
+    }
+
+    remoteData = remote_msg(
+        proc,
+        cls_NSPropertyListSerialization,
+        sel_dataWithPropertyList,
+        remotePropertyList,
+        NSPropertyListBinaryFormat_v1_0,
+        0,
+        errorRef
+    );
+    if (!remoteData) {
+        logmsg("NSPropertyListSerialization returned no data");
+        uint64_t error = [proc remoteRead64From:errorRef];
+        if (error) {
+            remote_description(proc, lifetimes, error);
+        }
+        goto cleanup;
+    }
+
+    retainedData = remote_retain(proc, lifetimes, remoteData);
+    if (!retainedData) {
+        logmsg("failed to retain serialized property list data");
+        goto cleanup;
+    }
+
+    plistData = copy_remote_nsdata(proc, retainedData);
+
+cleanup:
+    if (retainedData) {
+        remote_release(proc, lifetimes, retainedData);
+    }
+    if (!proc.trojanMemIsStackFallback && errorRef) {
+        RemoteArbCall(proc, free, errorRef);
+    }
+    return plistData;
+}
+
+static NSArray<NSDictionary *> * _Nullable parse_secitems_property_list(NSData *plistData) {
+    if (!plistData) {
+        return nil;
+    }
+
+    NSError *error = nil;
+    id plist = [NSPropertyListSerialization propertyListWithData:plistData
+                                                         options:NSPropertyListImmutable
+                                                          format:nil
+                                                           error:&error];
+    if (!plist) {
+        const char *message = error.localizedDescription.UTF8String;
+        logmsg(message ? message : "failed to decode keychain plist");
+        return nil;
+    }
+
+    if ([plist isKindOfClass:[NSDictionary class]]) {
+        return @[ (NSDictionary *)plist ];
+    }
+
+    if ([plist isKindOfClass:[NSArray class]]) {
+        NSArray *items = (NSArray *)plist;
+        for (id item in items) {
+            if (![item isKindOfClass:[NSDictionary class]]) {
+                logmsg("keychain plist contained a non-dictionary item");
+                return nil;
+            }
+        }
+        return (NSArray<NSDictionary *> *)items;
+    }
+
+    logmsg("keychain plist root was not a dictionary or array");
+    return nil;
+}
+
+bool find_secitem_symbols(RemoteCall *proc, remote_sec_symbols *symbols) {
+    if (!proc || !symbols) {
+        return false;
+    }
+
+    if (resolve_secitem_symbols(proc, symbols, false)) {
+        return true;
+    }
+
+    if (!remote_load_library(proc, kSecurityFrameworkPath)) {
+        logmsg("failed to load Security.framework");
+    }
+
+    return resolve_secitem_symbols(proc, symbols, true);
+}
+
+static uint64_t remote_NSMutableDictionary_dictionary(RemoteCall *proc) {
+    if (!proc) {
+        return 0;
+    }
+
+    uint64_t cls_NSMutableDictionary = remote_getClass(proc, "NSMutableDictionary");
+    uint64_t sel_alloc = remote_sel(proc, "alloc");
+    uint64_t sel_init = remote_sel(proc, "init");
+    if (!cls_NSMutableDictionary || !sel_alloc || !sel_init) {
+        return 0;
+    }
+
+    uint64_t rawHandler = remote_msg(proc, cls_NSMutableDictionary, sel_alloc, 0, 0, 0, 0);
+    if (!rawHandler) {
+        return 0;
+    }
+
+    return remote_msg(proc, rawHandler, sel_init, 0, 0, 0, 0);
+}
+
+static bool remote_NSMutableDictionary_setObjectForKey(RemoteCall *proc, uint64_t remoteDict, uint64_t remoteObject, uint64_t remoteKey) {
+    if (!proc || !remoteDict || !remoteObject || !remoteKey) {
+        return false;
+    }
+
+    uint64_t sel_setObjectForKey = remote_sel(proc, "setObject:forKey:");
+    if (!sel_setObjectForKey) {
+        return false;
+    }
+
+    remote_msg(proc, remoteDict, sel_setObjectForKey, remoteObject, remoteKey, 0, 0);
+    return true;
+}
+
+static bool remote_isKindOfClass_name(RemoteCall *proc, uint64_t remoteObject, const char *className) {
+    if (!proc || !remoteObject || !className) {
+        return false;
+    }
+
+    uint64_t sel_isKindOfClass = remote_sel(proc, "isKindOfClass:");
+    uint64_t cls_target = remote_getClass(proc, className);
+    if (!sel_isKindOfClass || !cls_target) {
+        return false;
+    }
+
+    return remote_msg(proc, remoteObject, sel_isKindOfClass, cls_target, 0, 0, 0) == YES;
+}
+
+static bool remote_isKindOfClass_cls(RemoteCall *proc, uint64_t remoteObject, uint64_t cls) {
+    if (!proc || !remoteObject || !cls) {
+        return false;
+    }
+
+    uint64_t sel_isKindOfClass = remote_sel(proc, "isKindOfClass:");
+    if (!sel_isKindOfClass) {
+        return false;
+    }
+
+    return remote_msg(proc, remoteObject, sel_isKindOfClass, cls, 0, 0, 0) == YES;
+}
+
+static NSData *serialize_keychain_results(RemoteCall *proc, const remote_objc_lifetimes *lifetimes, remote_sec_symbols *symbols, uint64_t remoteResultObject) {
+    if (!proc || !lifetimes || !symbols || !remoteResultObject) {
+        return nil;
+    }
+
+    NSData *plistData = nil;
+
+    uint64_t cls_NSMutableArray = remote_getClass(proc, "NSMutableArray");
+    uint64_t cls_NSDictionary = remote_getClass(proc, "NSDictionary");
+    uint64_t sel_count = remote_sel(proc, "count");
+    uint64_t sel_arrayWithCapacity = remote_sel(proc, "arrayWithCapacity:");
+    uint64_t sel_objectAtIndex = remote_sel(proc, "objectAtIndex:");
+    uint64_t sel_mutableCopy = remote_sel(proc, "mutableCopy");
+    uint64_t sel_removeObjectForKey = remote_sel(proc, "removeObjectForKey:");
+    uint64_t sel_addObject = remote_sel(proc, "addObject:");
+
+    if (!cls_NSMutableArray ||
+        !cls_NSDictionary ||
+        !sel_count ||
+        !sel_arrayWithCapacity ||
+        !sel_objectAtIndex ||
+        !sel_mutableCopy ||
+        !sel_removeObjectForKey ||
+        !sel_addObject) {
+        return nil;
+    }
+
+    if (remote_isKindOfClass_name(proc, remoteResultObject, "NSArray")) {
+        uint64_t remoteArrayCount = remote_msg(proc, remoteResultObject, sel_count, 0, 0, 0, 0);
+        uint64_t mutableArray = remote_msg(proc, cls_NSMutableArray, sel_arrayWithCapacity, remoteArrayCount, 0, 0, 0);
+        uint64_t retainedArray = remote_retain(proc, lifetimes, mutableArray);
+        if (!retainedArray) {
+            logmsg("failed to retain mutable array for keychain serialization");
+            return nil;
+        }
+
+        for (uint64_t idx = 0; idx < remoteArrayCount; idx++) {
+            uint64_t item = remote_msg(proc, remoteResultObject, sel_objectAtIndex, idx, 0, 0, 0);
+            if (remote_isKindOfClass_cls(proc, item, cls_NSDictionary)) {
+                uint64_t mutableDict = remote_msg(proc, item, sel_mutableCopy, 0, 0, 0, 0);
+                if (!mutableDict) {
+                    logmsg("failed to create mutable keychain item copy");
+                    remote_release(proc, lifetimes, retainedArray);
+                    return nil;
+                }
+                remote_msg(proc, mutableDict, sel_removeObjectForKey, symbols->kSecAttrAccessControl, 0, 0, 0);
+                remote_msg(proc, retainedArray, sel_addObject, mutableDict, 0, 0, 0);
+                remote_release(proc, lifetimes, mutableDict);
+            } else {
+                remote_msg(proc, retainedArray, sel_addObject, item, 0, 0, 0);
+            }
+        }
+
+        plistData = copy_remote_property_list_data(proc, lifetimes, retainedArray);
+        remote_release(proc, lifetimes, retainedArray);
+        return plistData;
+    }
+
+    if (remote_isKindOfClass_cls(proc, remoteResultObject, cls_NSDictionary)) {
+        uint64_t mutableDict = remote_msg(proc, remoteResultObject, sel_mutableCopy, 0, 0, 0, 0);
+        if (!mutableDict) {
+            logmsg("failed to create mutable keychain result copy");
+            return nil;
+        }
+        remote_msg(proc, mutableDict, sel_removeObjectForKey, symbols->kSecAttrAccessControl, 0, 0, 0);
+        plistData = copy_remote_property_list_data(proc, lifetimes, mutableDict);
+        remote_release(proc, lifetimes, mutableDict);
+        return plistData;
+    }
+
+    return copy_remote_property_list_data(proc, lifetimes, remoteResultObject);
+}
+
+NSArray<NSDictionary *> * _Nullable get_secitems(RemoteCall *proc, remote_sec_symbols *symbols, sec_class target_sec_class, bool get_data) {
+    if (!proc || !symbols) {
+        return nil;
+    }
+
+    remote_objc_lifetimes lifetimes = {0};
+    if (!find_objc_lifetimes(proc, &lifetimes)) {
+        logmsg("failed to resolve Objective-C lifetime selectors");
+        return nil;
+    }
+
+    uint64_t remoteQueryDict = remote_NSMutableDictionary_dictionary(proc);
+    uint64_t remoteResultRef = 0;
+    uint64_t remoteResultObject = 0;
+    uint64_t remoteClassValue = 0;
+    int32_t status = 0;
+    NSArray<NSDictionary *> *results = nil;
+    NSData *plistData = nil;
+
+    if (!remoteQueryDict) {
+        return nil;
+    }
+
+    switch (target_sec_class) {
+        case kSCGenericPassword:
+            remoteClassValue = symbols->kSecClassGenericPassword;
+            break;
+        case kSCInternetPassword:
+            remoteClassValue = symbols->kSecClassInternetPassword;
+            break;
+        case kSCCertificate:
+            remoteClassValue = symbols->kSecClassCertificate;
+            break;
+        case kSCKey:
+            remoteClassValue = symbols->kSecClassKey;
+            break;
+        case kSCIdentity:
+            remoteClassValue = symbols->kSecClassIdentity;
+            break;
+    }
+
+    if (!remoteClassValue) {
+        logmsg("unsupported keychain class");
+        goto cleanup;
+    }
+
+    if (!remote_NSMutableDictionary_setObjectForKey(proc, remoteQueryDict, remoteClassValue, symbols->kSecClass) ||
+        !remote_NSMutableDictionary_setObjectForKey(proc, remoteQueryDict, symbols->kCFBooleanTrue, symbols->kSecReturnAttributes) ||
+        !remote_NSMutableDictionary_setObjectForKey(proc, remoteQueryDict, symbols->kSecMatchLimitAll, symbols->kSecMatchLimit)) {
+        logmsg("failed to build remote keychain query");
+        goto cleanup;
+    }
+
+    if (get_data) {
+        if (!remote_NSMutableDictionary_setObjectForKey(proc, remoteQueryDict, symbols->kCFBooleanTrue, symbols->kSecReturnData)) {
+            logmsg("failed to request keychain item data");
+            goto cleanup;
+        }
+    }
+
+    remoteResultRef = remote_alloc_ref(proc, 0);
+    if (!remoteResultRef) {
+        logmsg("failed to allocate remote result pointer");
+        goto cleanup;
+    }
+
+    status = (int32_t)RemoteArbCall(proc, symbols->SecItemCopyMatching, remoteQueryDict, remoteResultRef);
+    if (status == kSecItemNotFoundStatus) {
+        results = @[];
+        goto cleanup;
+    }
+    if (status != 0) {
+        log_status_message("SecItemCopyMatching failed", status);
+        goto cleanup;
+    }
+
+    remoteResultObject = [proc remoteRead64From:remoteResultRef];
+    if (!remoteResultObject) {
+        logmsg("SecItemCopyMatching succeeded but returned no result");
+        goto cleanup;
+    }
+
+    plistData = serialize_keychain_results(proc, &lifetimes, symbols, remoteResultObject);
+    if (!plistData) {
+        logmsg("failed to serialize remote keychain result");
+        goto cleanup;
+    }
+
+    results = parse_secitems_property_list(plistData);
+
+cleanup:
+    if (!proc.trojanMemIsStackFallback && remoteResultRef) {
+        RemoteArbCall(proc, free, remoteResultRef);
+    }
+    remote_release(proc, &lifetimes, remoteQueryDict);
+    remote_release(proc, &lifetimes, remoteResultObject);
+    return results;
+}
  ```

  </details>
- 🟡 `lara/lara-Bridging-Header.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -17,6 +17,7 @@
 #import "rc.h"
 #import "RemoteCall.h"
 #import "decrypt.h"
+#import "keychain.h"
 #import "persistence.h"
 #import "ota.h"
 #import "screentime.h"
  ```

  </details>
- 🟢 `lara/views/tweaks/KeychainView.swift` (added) — +1245 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,1245 @@
+//
+//  KeychainView.swift
+//  lara
+//
+//  Created by yyfll on 08.07.26.
+//  Modified from DecryptView.swift
+//
+
+import SwiftUI
+import UIKit
+
+private struct KeychainApp: Identifiable {
+    let name: String
+    let bundleID: String
+    let bundlePath: String
+    let executable: String
+    let icon: UIImage?
+
+    var id: String { bundleID }
+}
+
+private enum KeychainClassKind: String, CaseIterable, Identifiable {
+    case genericPassword
+    case internetPassword
+    case certificate
+    case key
+    case identity
+
+    var id: String { rawValue }
+
+    var title: String {
+        switch self {
+        case .genericPassword: return "Generic Password"
+        case .internetPassword: return "Internet Password"
+        case .certificate: return "Certificate"
+        case .key: return "Key"
+        case .identity: return "Identity"
+        }
+    }
+
+    var icon: String {
+        switch self {
+        case .genericPassword: return "key.horizontal"
+        case .internetPassword: return "globe"
+        case .certificate: return "checkmark.seal"
+        case .key: return "key"
+        case .identity: return "person.text.rectangle"
+        }
+    }
+
+    var exportKey: String {
+        switch self {
+        case .genericPassword: return "genericPassword"
+        case .internetPassword: return "internetPassword"
+        case .certificate: return "certificate"
+        case .key: return "key"
+        case .identity: return "identity"
+        }
+    }
+
+    var secClass: sec_class {
+        switch self {
+        case .genericPassword: return .scGenericPassword
+        case .internetPassword: return .scInternetPassword
+        case .certificate: return .scCertificate
+        case .key: return .scKey
+        case .identity: return .scIdentity
+        }
+    }
+}
+
+private enum KeychainClassLoadState {
+    case success([KeychainItemResult])
+    case empty
+    case failure(String)
+}
+
+private struct KeychainClassResult: Identifiable {
+    let kind: KeychainClassKind
+    let state: KeychainClassLoadState
+
+    var id: String { kind.id }
+
+    var itemCount: Int {
+        switch state {
+        case .success(let items): return items.count
+        case .empty, .failure: return 0
+        }
+    }
+
+    var items: [KeychainItemResult] {
+        switch state {
+        case .success(let items): return items
+        case .empty, .failure: return []
+        }
+    }
+
+    var statusText: String {
+        switch state {
+        case .success(let items): return "\(items.count) items"
+        case .empty: return "0 items"
+        case .failure: return "Failed"
+        }
+    }
+
+    var statusColor: Color {
+        switch state {
+        case .success(let items):
+            return items.isEmpty ? .secondary : .green
+        case .empty:
+            return .secondary
+        case .failure:
+            return .red
+        }
+    }
+
+    var failureMessage: String? {
+        if case .failure(let message) = state {
+            return message
+        }
+        return nil
+    }
+
+    var jsonObject: [String: Any] {
+        var object: [String: Any] = [
+            "id": kind.exportKey,
+            "title": kind.title,
+            "status": jsonStatus,
+            "itemCount": itemCount,
+            "items": items.map(\.jsonObject),
+        ]
+        if let failureMessage {
+            object["error"] = failureMessage
+        }
+        return object
+    }
+
+    private var jsonStatus: String {
+        switch state {
+        case .success: return "success"
+        case .empty: return "empty"
+        case .failure: return "failure"
+        }
+    }
+}
+
+private struct KeychainFieldResult: Identifiable {
+    let key: String
+    let displayValue: String
+    let detailText: String
+    let searchText: String
+    let isMonospaced: Bool
+    let exportValue: Any
+
+    var id: String { key }
+
+    var jsonObject: [String: Any] {
+        [
+            "key": key,
+            "value": exportValue,
+        ]
+    }
+}
+
+private struct KeychainItemResult: Identifiable {
+    let id: String
+    let title: String
+    let subtitle: String?
+    let fields: [KeychainFieldResult]
+
+    var summaryFields: [KeychainFieldResult] {
+        let priorityKeys = [
+            "labl", "acct", "svce", "srvr", "path", "agrp", "alis",
+            "subj", "issuer", "cdat", "mdat"
+        ]
+
+        var matches: [KeychainFieldResult] = []
+        for key in priorityKeys {
+            if let field = fields.first(where: { $0.key == key }) {
+                matches.append(field)
+            }
+        }
+
+        if matches.isEmpty {
+            return Array(fields.prefix(3))
+        }
+        return matches
+    }
+
+    var searchableText: String {
+        var pieces = [title]
+        if let subtitle {
+            pieces.append(subtitle)
+        }
+        pieces.append(contentsOf: fields.map { "\($0.key) \($0.searchText)" })
+        return pieces.joined(separator: "\n")
+    }
+
+    var secondarySummary: String? {
+        let values = summaryFields
+            .map(\.displayValue)
+            .filter { !$0.isEmpty }
+            .filter { value in
+                value != title && value != (subtitle ?? "")
+            }
+
+        return values.first
+    }
+
+    var jsonObject: [String: Any] {
+        var object: [String: Any] = [
+            "id": id,
+            "title": title,
+            "fields": fields.map(\.jsonObject),
+        ]
+        if let subtitle {
+            object["subtitle"] = subtitle
+        }
+        return object
+    }
+
+    var prettyJSON: String {
+        prettyPrintedJSONString(jsonObject) ?? "{}"
+    }
+}
+
+private struct KeychainReadResult {
+    let app: KeychainApp
+    let date: Date
+    let classResults: [KeychainClassResult]
+
+    var totalItems: Int {
+        classResults.reduce(0) { $0 + $1.itemCount }
+    }
+
+    var exportObject: [String: Any] {
+        [
+            "app": [
+                "name": app.name,
+                "bundleID": app.bundleID,
+                "bundlePath": app.bundlePath,
+                "executable": app.executable,
+            ],
+            "readAt": iso8601Formatter.string(from: date),
+            "totalItems": totalItems,
+            "classes": classResults.map(\.jsonObject),
+        ]
+    }
+}
+
+struct KeychainView: View {
+    @ObservedObject private var mgr = laramgr.shared
+    @State private var query = ""
+    @State private var apps: [KeychainApp] = []
+    @State private var datareadingbid: String? = nil
+    @State private var errormsg: String? = nil
+    @State private var pendingread: KeychainApp? = nil
+    @State private var currentResult: KeychainReadResult? = nil
+    @State private var isLoadingApps = false
+    @State private var launchBackgroundTask: UIBackgroundTaskIdentifier = .invalid
+
+    private var filteredapps: [KeychainApp] {
+        let trimmed = query.trimmingCharacters(in: .whitespacesAndNewlines)
+        if trimmed.isEmpty { return apps }
+        let q = trimmed.lowercased()
+        return apps.filter { $0.name.lowercased().contains(q) || $0.bundleID.lowercased().contains(q) }
+    }
+
+    private var appEmptyMessage: String {
+        if !mgr.sbxready {
+            return "Run the sandbox escape to list installed apps."
+        }
+        if isLoadingApps {
+            return "Loading..."
+        }
+        if query.isEmpty {
+            return "No apps found."
+        }
+        return "No matches."
+    }
+
+    var body: some View {
+        NavigationStack {
+            List {
+                if let error = errormsg {
+                    Section {
+                        PlainAlert(title: "Error", icon: "exclamationmark.triangle", text: error, color: .red)
+                    }
+                }
+
+                currentResultSection
+                resultsSection
+                installedAppsSection
+            }
+            .navigationTitle("Keychain Reader")
+        }
+        .onAppear {
+            set_log_callback_kc { msg in
+                guard let msg = msg else { return }
+                let s = String(cString: msg)
+                DispatchQueue.main.async {
+                    laramgr.shared.logmsg("(keychain) \(s)")
+                }
+            }
+            if mgr.sbxready && apps.isEmpty {
+                loadApps()
+            }
+        }
+        .onReceive(NotificationCenter.default.publisher(for: UIApplication.didBecomeActiveNotification)) { _ in
+            attemptPendingRead(reportFailure: false)
+        }
+        .onChange(of: mgr.sbxready) { ready in
+            if ready {
+                loadApps()
+            } else {
+                apps.removeAll()
+                isLoadingApps = false
+            }
+        }
+    }
+
+    private var currentResultSection: some View {
+        Section(
+            header: HeaderLabel(text: "Current Result", icon: "tray.full"),
+            footer: Text("Keychain reading relies on RemoteCall and exploit state. It may be unstable and can fail, return incomplete results or make app crash.")
+        ) {
+            if let currentResult {
+                LabeledContent("App") {
+                    Text(currentResult.app.name)
+                }
+                LabeledContent("Bundle ID") {
+                    Text(currentResult.app.bundleID)
+                        .font(.system(.body, design: .monospaced))
+                        .foregroundStyle(.secondary)
+                }
+                LabeledContent("Read At") {
+                    Text(currentResult.date.formatted(date: .abbreviated, time: .standard))
+                        .foregroundStyle(.secondary)
+                }
+                LabeledContent("Total Items") {
+                    Text("\(currentResult.totalItems)")
+                        .monospacedDigit()
+                        .foregroundStyle(.secondary)
+                }
+
+                Button("Export JSON") {
+                    exportCurrentResult()
+                }
+                .disabled(datareadingbid != nil)
+            } else {
+                Text("Read an app to browse its keychain results.")
+                    .foregroundColor(.secondary)
+            }
+        }
+    }
+
+    private var resultsSection: some View {
+        Section(
+            header: HeaderLabel(text: "Results", icon: "key.horizontal"),
+            footer: Text("Keychain Reader will switch to target app and then switch back 2 times. Generally, you do not need to take any action during this process.\n\nIf keep in progress, try manually switch to target app and then switch back to lara.")
+        ) {
+            if let currentResult {
+                ForEach(currentResult.classResults) { classResult in
+                    switch classResult.state {
+                    case .success(let items) where !items.isEmpty:
+                        NavigationLink {
+                            KeychainClassDetailView(
+                                app: currentResult.app,
+                                classResult: classResult
+                            )
+                        } label: {
+                            KeychainClassRow(classResult: classResult)
+                        }
+                    default:
+                        KeychainClassRow(classResult: classResult)
+                    }
+                }
+            } else {
+                Text("No result yet.")
+                    .foregroundColor(.secondary)
+            }
+        }
+    }
+
+    private var installedAppsSection: some View {
+        Section(header: HeaderLabel(text: "Installed Apps", icon: "app.badge")) {
+            HStack {
+                TextField("Search", text: $query)
+                    .textInputAutocapitalization(.never)
+                    .autocorrectionDisabled()
+
+                Button(action: loadApps) {
+                    Image(systemName: "arrow.clockwise")
+                }
+                .disabled(!mgr.sbxready || datareadingbid != nil || isLoadingApps)
+            }
+
+            if filteredapps.isEmpty {
+                Text(appEmptyMessage)
+                    .foregroundColor(.secondary)
+            } else {
+                ForEach(filteredapps) { app in
+                    KCAppRow(
+                        app: app,
+                        isreading: datareadingbid == app.bundleID,
+                        isdisabled: datareadingbid != nil && datareadingbid != app.bundleID
+                    ) {
+                        startRead(app)
+                    }
+                }
+            }
+        }
+    }
+
+    private func loadApps() {
+        guard mgr.sbxready else {
+            DispatchQueue.main.async {
+                self.apps.removeAll()
+                self.isLoadingApps = false
+            }
+            return
+        }
+
+        DispatchQueue.main.async {
+            self.isLoadingApps = true
+        }
+
+        DispatchQueue.global(qos: .userInitiated).async {
+            var results: [KeychainApp] = []
+            let bundleFolder = "/private/var/containers/Bundle/Application"
+
+            guard let bundles = try? FileManager.default.contentsOfDirectory(atPath: bundleFolder) else {
+                DispatchQueue.main.async {
+                    self.apps.removeAll()
+                    self.isLoadingApps = false
+                }
+                return
+            }
+
+            for bundle in bundles {
+                let appPath = bundleFolder + "/" + bundle
+                guard let contents = try? FileManager.default.contentsOfDirectory(atPath: appPath) else { continue }
+                for item in contents {
+                    guard item.hasSuffix(".app") else { continue }
+                    guard isEligibleKeychainApp(at: appPath, bname: item) else { continue }
+                    let fullAppPath = appPath + "/" + item
+                    let infoPath = fullAppPath + "/Info.plist"
+                    guard let info = NSDictionary(contentsOfFile: infoPath) else { continue }
+0
+                    let executable = info["CFBundleExecutable"] as? String ?? ""
+                    if executable.isEmpty { continue }
+                    let bundleid = info["CFBundleIdentifier"] as? String ?? ""
+                    if bundleid != nil && bundleid == Bundle.main.bundleIdentifier { continue }
+                    let name = (info["CFBundleDisplayName"] as? String) ??
+                        (info["CFBundleName"] as? String) ??
+                        (item as NSString).deletingPathExtension
+
+                    var icon: UIImage? = nil
+                    if let icons = info["CFBundleIcons"] as? [String: Any],
+                       let primary = icons["CFBundlePrimaryIcon"] as? [String: Any],
+                       let iconfiles = primary["CFBundleIconFiles"] as? [String],
+                       let iconname = iconfiles.last {
+                        let iconpath = fullAppPath + "/" + iconname
+                        if let img = UIImage(contentsOfFile: iconpath) { icon = img }
+                        else if let img = UIImage(contentsOfFile: iconpath + "@2x.png") { icon = img }
+                        else if let img = UIImage(contentsOfFile: iconpath + ".png") { icon = img }
+                    }
+
+                    results.append(KeychainApp(
+                        name: name,
+                        bundleID: bundleid,
+                        bundlePath: fullAppPath,
+                        executable: executable,
+                        icon: icon ?? UIImage(named: "unknown")
+                    ))
+                    break
+                }
+            }
+
+            results.sort { $0.name.lowercased() < $1.name.lowercased() }
+            DispatchQueue.main.async {
+                self.apps = results
+                self.isLoadingApps = false
+            }
+        }
+    }
+
+    private func isEligibleKeychainApp(at containerPath: String, bname bundleName: String) -> Bool {
+        FileManager.default.fileExists(atPath: containerPath + "/" + bundleName + "/embedded.mobileprovision") || FileManager.default.fileExists(atPath: containerPath + "/iTunesMetadata.plist")
+    }
+
+    private func startRead(_ app: KeychainApp) {
+        guard datareadingbid == nil && pendingread == nil else { return }
+        guard mgr.dsready else {
+            errormsg = "Darksword not ready. Run the exploit first."
+            return
+        }
+        guard mgr.hasOffsets else {
+            errormsg = "Offsets not ready. Fetch the kernelcache offsets first."
+            return
+        }
+        guard mgr.sbxready else {
+            errormsg = "Sandbox escape not ready."
+            return
+        }
+        guard !app.executable.trimmingCharacters(in: .whitespacesAndNewlines).isEmpty else {
+            errormsg = "App executable is missing."
+            return
+        }
+
+        runRead(app)
+    }
+
+    private func runRead(_ app: KeychainApp) {
+        errormsg = nil
+        datareadingbid = app.bundleID
+        pendingread = app
+        beginLaunchBackgroundTask()
+
+        let ret = launch_app(app.bundleID)
+        guard ret == 0 else {
+            pendingread = nil
+            datareadingbid = nil
+            errormsg = "Could not launch app. Open it manually."
+            endLaunchBackgroundTask()
+            return
+        }
+
+        DispatchQueue.global(qos: .userInitiated).asyncAfter(deadline: .now() + 5.0) {
+            returnToLaraApp()
+            usleep(1_000_000)
+            DispatchQueue.main.async {
+                self.attemptPendingRead(reportFailure: true)
+            }
+        }
+    }
+
+    private func attemptPendingRead(reportFailure _: Bool) {
+        guard let app = pendingread else { return }
+
+        guard mgr.dsready, mgr.hasOffsets, mgr.sbxready else {
+            pendingread = nil
+            datareadingbid = nil
+            errormsg = "Exploit state changed. Reinitialize and try again."
+            endLaunchBackgroundTask()
+            return
+        }
+
+        guard !app.executable.trimmingCharacters(in: .whitespacesAndNewlines).isEmpty else {
+            pendingread = nil
+            datareadingbid = nil
+            errormsg = "App executable is missing."
+            endLaunchBackgroundTask()
+            return
+        }
+
+        pendingread = nil
+        endLaunchBackgroundTask()
+        doRead(app)
+    }
+
+    private func doRead(_ app: KeychainApp) {
+        guard mgr.dsready, mgr.hasOffsets, mgr.sbxready else {
+            datareadingbid = nil
+            errormsg = "Exploit state changed. Reinitialize and try again."
+            return
+        }
+
+        laramgr.shared.logmsg("(keychain) reading \(app.bundleID)...")
+
+        DispatchQueue.global(qos: .userInitiated).async {
+            guard let proc = RemoteCall(process: app.executable, useMigFilterBypass: false) else {
+                DispatchQueue.main.async {
+                    self.datareadingbid = nil
+                    self.errormsg = "Cannot init RemoteCall."
+                }
+                return
+            }
+
+            defer {
+                proc.destroy()
+                returnToLaraApp()
+            }
+
+            let relaunchSucceeded = DispatchQueue.main.sync {
+                launch_app(app.bundleID) == 0
+            }
+            guard relaunchSucceeded else {
+                DispatchQueue.main.async {
+                    self.datareadingbid = nil
+                    self.errormsg = "Could not relaunch app for keychain scan."
+                }
+                return
+            }
+
+            usleep(500_000)
+
+            let result = self.performRead(app: app, proc: proc)
+            DispatchQueue.main.async {
+                self.currentResult = result
+                self.datareadingbid = nil
+                self.errormsg = nil
+            }
+        }
+    }
+
+    private func performRead(app: KeychainApp, proc: RemoteCall) -> KeychainReadResult {
+        var secSymbols = remote_sec_symbols()
+        guard find_secitem_symbols(proc, &secSymbols) else {
+            let message = "Failed to resolve Security symbols."
+            laramgr.shared.logmsg("(keychain) \(message)")
+            return KeychainReadResult(
+                app: app,
+                date: Date(),
+                classResults: KeychainClassKind.allCases.map {
+                    KeychainClassResult(kind: $0, state: .failure(message))
+                }
+            )
+        }
+
+        var classResults: [KeychainClassResult] = []
+
+        for kind in KeychainClassKind.allCases {
+            guard let rawItems = get_secitems(proc, &secSymbols, kind.secClass, true) as? [NSDictionary] else {
+                laramgr.shared.logmsg("(keychain) \(kind.title.lowercased()) read failed")
+                classResults.append(
+                    KeychainClassResult(
+                        kind: kind,
+                        state: .failure("Failed to read \(kind.title.lowercased()) items.")
+                    )
+                )
+                continue
+            }
+
+            let items = normalizeItems(rawItems, kind: kind)
+            if items.isEmpty {
+                laramgr.shared.logmsg("(keychain) \(kind.title.lowercased()): 0 items")
+                classResults.append(KeychainClassResult(kind: kind, state: .empty))
+            } else {
+                laramgr.shared.logmsg("(keychain) \(kind.title.lowercased()): \(items.count) items")
+                classResults.append(KeychainClassResult(kind: kind, state: .success(items)))
+            }
+        }
+
+        let result = KeychainReadResult(
+            app: app,
+            date: Date(),
+            classResults: classResults
+        )
+        laramgr.shared.logmsg("(keychain) finished \(app.bundleID) with \(result.totalItems) total item(s)")
+        return result
+    }
+
+    private func normalizeItems(_ rawItems: [NSDictionary], kind: KeychainClassKind) -> [KeychainItemResult] {
+        rawItems.enumerated().map { index, rawItem in
+            let rawPairs = rawItem.allKeys.compactMap { key -> (String, Any)? in
+                guard let value = rawItem.object(forKey: key) else { return nil }
+                return (String(describing: key), value)
+            }
+            let pairs = rawPairs.sorted { $0.0.localizedCaseInsensitiveCompare($1.0) == .orderedAscending }
+            let fields = pairs.map { normalizeField(key: $0.0, value: $0.1) }
+
+            let title = itemLabel(
+                fallback: "\(kind.title) Item \(index + 1)",
+                fields: fields,
+                preferredKeys: ["labl", "svce", "srvr", "acct", "alis", "subj", "type"]
+            )
+            let subtitle = itemSubtitle(fields: fields, excluding: title)
+
+            return KeychainItemResult(
+                id: "\(kind.exportKey)-\(index)",
+                title: title,
+                subtitle: subtitle,
+                fields: fields
+            )
+        }
+    }
+
+    private func normalizeField(key: String, value: Any) -> KeychainFieldResult {
+        if let data = value as? Data {
+            if data.isEmpty {
+                let exportValue: [String: Any] = [
+                    "type": "data",
+                    "byteCount": 0,
+                    "base64": "",
+                    "preview": "",
+                ]
+                return KeychainFieldResult(
+                    key: key,
+                    displayValue: "",
+                    detailText: "",
+                    searchText: "",
+                    isMonospaced: true,
+                    exportValue: exportValue
+                )
+            }
+
+            if key.caseInsensitiveCompare("sha1") == .orderedSame {
+                let hex = hexString(data)
+                let exportValue: [String: Any] = [
+                    "type": "data",
+                    "byteCount": data.count,
+                    "hex": hex,
+                    "base64": data.base64EncodedString(),
+                ]
+                return KeychainFieldResult(
+                    key: key,
+                    displayValue: hex,
+                    detailText: hex,
+                    searchText: hex,
+                    isMonospaced: true,
+                    exportValue: exportValue
+                )
+            }
+
+            let preview = santanderfs.render(data: data).text
+            let exportValue: [String: Any] = [
+                "type": "data",
+                "byteCount": data.count,
+                "base64": data.base64EncodedString(),
+                "preview": preview,
+            ]
+            return KeychainFieldResult(
+                key: key,
+                displayValue: compactText(preview),
+                detailText: preview,
+                searchText: preview,
+                isMonospaced: true,
+                exportValue: exportValue
+            )
+        }
+
+        if let string = value as? String {
+            return KeychainFieldResult(
+                key: key,
+                displayValue: compactText(string),
+                detailText: string,
+                searchText: string,
+                isMonospaced: false,
+                exportValue: string
+            )
+        }
+
+        if let date = value as? Date {
+            let display = date.formatted(date: .abbreviated, time: .standard)
+            return KeychainFieldResult(
+                key: key,
+                displayValue: display,
+                detailText: display,
+                searchText: display,
+                isMonospaced: false,
+                exportValue: iso8601Formatter.string(from: date)
+            )
+        }
+
+        if let bool = value as? Bool {
+            let display = bool ? "True" : "False"
+            return KeychainFieldResult(
+                key: key,
+                displayValue: display,
+                detailText: display,
+                searchText: display,
+                isMonospaced: false,
+                exportValue: bool
+            )
+        }
+
+        if let int = value as? Int {
+            let display = String(int)
+            return KeychainFieldResult(
+                key: key,
+                displayValue: display,
+                detailText: display,
+                searchText: display,
+                isMonospaced: false,
+                exportValue: int
+            )
+        }
+
+        if let double = value as? Double {
+            let display = String(format: "%.3f", double)
+            return KeychainFieldResult(
+                key: key,
+                displayValue: display,
+                detailText: display,
+                searchText: display,
+                isMonospaced: false,
+                exportValue: double
+            )
+        }
+
+        if let number = value as? NSNumber {
+            if CFGetTypeID(number) == CFBooleanGetTypeID() {
+                let bool = number.boolValue
+                let display = bool ? "True" : "False"
+                return KeychainFieldResult(
+                    key: key,
+                    displayValue: display,
+                    detailText: display,
+                    searchText: display,
+                    isMonospaced: false,
+                    exportValue: bool
+                )
+            }
+
+            let display = number.stringValue
+            let exportValue: Any = Int(display).map { $0 } ?? Double(display).map { $0 } ?? display
+            return KeychainFieldResult(
+                key: key,
+                displayValue: display,
+                detailText: display,
+                searchText: display,
+                isMonospaced: false,
+                exportValue: exportValue
+            )
+        }
+
+        if let dict = value as? NSDictionary {
+            let exportValue = normalizedDictionary(dict)
+            let detail = prettyPrintedJSONString(exportValue) ?? String(describing: exportValue)
+            return KeychainFieldResult(
+                key: key,
+                displayValue: compactText(detail),
+                detailText: detail,
+                searchText: detail,
+                isMonospaced: true,
+                exportValue: exportValue
+            )
+        }
+
+        if let dict = value as? [AnyHashable: Any] {
+            let exportValue = normalizedDictionary(dict)
+            let detail = prettyPrintedJSONString(exportValue) ?? String(describing: exportValue)
+            return KeychainFieldResult(
+                key: key,
+                displayValue: compactText(detail),
+                detailText: detail,
+                searchText: detail,
+                isMonospaced: true,
+                exportValue: exportValue
+            )
+        }
+
+        if let array = value as? [Any] {
+            let exportValue = array.map(normalizedJSONValue)
+            let detail = prettyPrintedJSONString(exportValue) ?? String(describing: exportValue)
+            return KeychainFieldResult(
+                key: key,
+                displayValue: compactText(detail),
+                detailText: detail,
+                searchText: detail,
+                isMonospaced: true,
+                exportValue: exportValue
+            )
+        }
+
+        let fallback = String(describing: value)
+        return KeychainFieldResult(
+            key: key,
+            displayValue: compactText(fallback),
+            detailText: fallback,
+            searchText: fallback,
+            isMonospaced: true,
+            exportValue: fallback
+        )
+    }
+
+    private func itemLabel(fallback: String, fields: [KeychainFieldResult], preferredKeys: [String]) -> String {
+        for key in preferredKeys {
+            if let value = fieldDisplayValue(for: key, in: fields), !value.isEmpty {
+                return value
+            }
+        }
+        return fallback
+    }
+
+    private func itemSubtitle(fields: [KeychainFieldResult], excluding title: String) -> String? {
+        let preferredKeys = ["acct", "svce", "srvr", "path", "agrp", "alis", "issuer", "subj"]
+        for key in preferredKeys {
+            guard let value = fieldDisplayValue(for: key, in: fields), !value.isEmpty, value != title else { continue }
+            return value
+        }
+        return nil
+    }
+
+    private func fieldDisplayValue(for key: String, in fields: [KeychainFieldResult]) -> String? {
+        fields.first(where: { $0.key == key })?.displayValue
+    }
+
+    private func exportCurrentResult() {
+        guard let currentResult else { return }
+
+        do {
+            let data = try JSONSerialization.data(
+                withJSONObject: currentResult.exportObject,
+                options: [.prettyPrinted, .sortedKeys]
+            )
+            let timestamp = iso8601Formatter.string(from: currentResult.date)
+                .replacingOccurrences(of: ":", with: "-")
+            let filename = "keychain-\(currentResult.app.bundleID)-\(timestamp).json"
+            let url = FileManager.default.temporaryDirectory.appendingPathComponent(filename)
+            try data.write(to: url, options: .atomic)
+            presentShareSheet(with: url)
+        } catch {
+            errormsg = "Failed to export result: \(error.localizedDescription)"
+        }
+    }
+
+    private func beginLaunchBackgroundTask() {
+        guard launchBackgroundTask == .invalid else { return }
+        launchBackgroundTask = UIApplication.shared.beginBackgroundTask(withName: "KeychainLaunch") {
+            DispatchQueue.main.async {
+                self.endLaunchBackgroundTask()
+            }
+        }
+    }
+
+    private func endLaunchBackgroundTask() {
+        guard launchBackgroundTask != .invalid else { return }
+        UIApplication.shared.endBackgroundTask(launchBackgroundTask)
+        launchBackgroundTask = .invalid
+    }
+}
+
+private struct KeychainClassDetailView: View {
+    let app: KeychainApp
+    let classResult: KeychainClassResult
+
+    @State private var searchQuery = ""
+
+    private var filteredItems: [KeychainItemResult] {
+        let trimmed = searchQuery.trimmingCharacters(in: .whitespacesAndNewlines)
+        if trimmed.isEmpty { return classResult.items }
+        let query = trimmed.lowercased()
+        return classResult.items.filter { item in
+            item.searchableText.lowercased().contains(query)
+        }
+    }
+
+    var body: some View {
+        List {
+            Section {
+                LabeledContent("App") {
+                    Text(app.name)
+                }
+                LabeledContent("Bundle ID") {
+                    Text(app.bundleID)
+                        .font(.system(.body, design: .monospaced))
+                        .foregroundStyle(.secondary)
+                }
+                LabeledContent("Items") {
+                    Text("\(classResult.itemCount)")
+                        .monospacedDigit()
+                        .foregroundStyle(.secondary)
+                }
+            } header: {
+                HeaderLabel(text: classResult.kind.title, icon: classResult.kind.icon)
+            }
+
+            Section {
+                if filteredItems.isEmpty {
+                    Text(searchQuery.isEmpty ? "No items." : "No matches.")
+                        .foregroundColor(.secondary)
+                } else {
+                    ForEach(filteredItems) { item in
+                        NavigationLink {
+                            KeychainItemDetailView(item: item)
+                        } label: {
+                            VStack(alignment: .leading, spacing: 4) {
+                                Text(item.title)
+                                    .font(.headline)
+                                    .foregroundColor(.primary)
+
+                                if let subtitle = item.subtitle {
+                                    Text(subtitle)
+                                        .font(.caption)
+                                        .foregroundColor(.secondary)
+                                        .lineLimit(2)
+                                }
+
+                                if let summary = item.secondarySummary {
+                                    Text(summary)
+                                        .font(.caption)
+                                        .foregroundColor(.secondary)
+                                        .lineLimit(2)
+                                }
+                            }
+                        }
+                    }
+                }
+            } header: {
+                HeaderLabel(text: "Items", icon: "list.bullet.rectangle")
+            }
+        }
+        .navigationTitle(classResult.kind.title)
+        .searchable(text: $searchQuery, prompt: "Search items")
+    }
+}
+
+private struct KeychainItemDetailView: View {
+    let item: KeychainItemResult
+
+    var body: some View {
+        List {
+            if !item.summaryFields.isEmpty {
+                Section(header: HeaderLabel(text: "Summary", icon: "text.alignleft")) {
+                    ForEach(item.summaryFields) { field in
+                        KeychainFieldRow(field: field)
+                    }
+                }
+            }
+
+            Section(header: HeaderLabel(text: "All Fields", icon: "doc.text")) {
+                ForEach(item.fields) { field in
+                    KeychainFieldRow(field: field)
+                }
+            }
+        }
+        .navigationTitle(item.title)
+        .navigationBarTitleDisplayMode(.inline)
+        .toolbar {
+            ToolbarItem(placement: .topBarTrailing) {
+                Button {
+                    UIPasteboard.general.string = item.prettyJSON
+                    UIImpactFeedbackGenerator(style: .light).impactOccurred()
+                } label: {
+                    Image(systemName: "doc.on.doc")
+                }
+            }
+        }
+    }
+}
+
+private struct KeychainClassRow: View {
+    let classResult: KeychainClassResult
+
+    var body: some View {
+        HStack(spacing: 12) {
+            Image(systemName: classResult.kind.icon)
+                .frame(width: 20, alignment: .center)
+                .foregroundColor(.accentColor)
+
+            VStack(alignment: .leading, spacing: 4) {
+                Text(classResult.kind.title)
+                    .foregroundColor(.primary)
+
+                if let failureMessage = classResult.failureMessage {
+                    Text(failureMessage)
+                        .font(.caption)
+                        .foregroundColor(.secondary)
+                        .lineLimit(2)
+                }
+            }
+
+            Spacer()
+
+            Text(classResult.statusText)
+                .font(.caption)
+                .foregroundColor(classResult.statusColor)
+                .monospaced()
+        }
+    }
+}
+
+private struct KeychainFieldRow: View {
+    let field: KeychainFieldResult
+
+    private var usesExpandedLayout: Bool {
+        field.isMonospaced || field.detailText.contains("\n") || field.detailText.count > 90
+    }
+
+    private var renderedText: String {
+        field.detailText.isEmpty ? "(empty)" : field.detailText
+    }
+
+    var body: some View {
+        if usesExpandedLayout {
+            VStack(alignment: .leading, spacing: 6) {
+                Text(field.key)
+                    .fontWeight(.medium)
+
+                Text(renderedText)
+                    .font(field.isMonospaced ? .system(size: 13, design: .monospaced) : .body)
+                    .foregroundColor(.secondary)
+                    .textSelection(.enabled)
+            }
+            .frame(maxWidth: .infinity, alignment: .leading)
+        } else {
+            LabeledContent(field.key) {
+                Text(renderedText)
+                    .foregroundStyle(.secondary)
+                    .multilineTextAlignment(.trailing)
+                    .textSelection(.enabled)
+            }
+        }
+    }
+}
+
+private struct KCAppRow: View {
+    let app: KeychainApp
+    let isreading: Bool
+    let isdisabled: Bool
+    let onread: () -> Void
+
+    var body: some View {
+        HStack {
+            if let icon = app.icon {
+                Image(uiImage: icon)
+                    .resizable()
+                    .frame(width: 40, height: 40)
+                    .clipShape(RoundedRectangle(cornerRadius: 9))
+            } else {
+                Image("unknown")
+                    .resizable()
+                    .frame(width: 40, height: 40)
+                    .clipShape(RoundedRectangle(cornerRadius: 9))
+            }
+
+            VStack(alignment: .leading, spacing: 2) {
+                Text(app.name)
+                    .font(.headline)
+                Text(app.bundleID)
+                    .font(.caption)
+                    .foregroundColor(.gray)
+            }
+
+            Spacer()
+
+            Button(action: onread) {
+                if isreading {
+                    ProgressView()
+                } else {
+                    Text("Read")
+                }
+            }
+            .disabled(isdisabled || isreading)
+        }
+        .opacity(isreading || isdisabled ? 0.6 : 1.0)
+    }
+}
+
+private let iso8601Formatter: ISO8601DateFormatter = {
+    let formatter = ISO8601DateFormatter()
+    formatter.formatOptions = [.withInternetDateTime, .withFractionalSeconds]
+    return formatter
+}()
+
+private func returnToLaraApp() {
+    guard let bundleID = Bundle.main.bundleIdentifier else { return }
+    _ = launch_app(bundleID)
+}
+
+private func compactText(_ text: String, maxLength: Int = 96) -> String {
+    let collapsed = text
+        .replacingOccurrences(of: "\r\n", with: "\n")
+        .replacingOccurrences(of: "\n", with: " ")
+        .trimmingCharacters(in: .whitespacesAndNewlines)
+
+    guard collapsed.count > maxLength else { return collapsed }
+    return String(collapsed.prefix(maxLength - 3)) + "..."
+}
+
+private func normalizedDictionary(_ dictionary: NSDictionary) -> [String: Any] {
+    let pairs = dictionary.allKeys.compactMap { key -> (String, Any)? in
+        guard let value = dictionary.object(forKey: key) else { return nil }
+        return (String(describing: key), value)
+    }
+    return normalizedDictionary(pairs)
+}
+
+private func normalizedDictionary(_ dictionary: [AnyHashable: Any]) -> [String: Any] {
+    let pairs = dictionary.map { (String(describing: $0.key), $0.value) }
+    return normalizedDictionary(pairs)
+}
+
+private func normalizedDictionary(_ pairs: [(String, Any)]) -> [String: Any] {
+    var result: [String: Any] = [:]
+    for (key, value) in pairs.sorted(by: { $0.0.localizedCaseInsensitiveCompare($1.0) == .orderedAscending }) {
+        result[key] = normalizedJSONValue(value)
+    }
+    return result
+}
+
+private func normalizedJSONValue(_ value: Any) -> Any {
+    if let data = value as? Data {
+        let preview = santanderfs.render(data: data).text
+        return [
+            "type": "data",
+            "byteCount": data.count,
+            "base64": data.base64EncodedString(),
+            "preview": preview,
+        ]
+    }
+    if let string = value as? String {
+        return string
+    }
+    if let date = value as? Date {
+        return iso8601Formatter.string(from: date)
+    }
+    if let bool = value as? Bool {
+        return bool
+    }
+    if let int = value as? Int {
+        return int
+    }
+    if let double = value as? Double {
+        return double
+    }
+    if let number = value as? NSNumber {
+        if CFGetTypeID(number) == CFBooleanGetTypeID() {
+            return number.boolValue
+        }
+        if let intValue = Int(number.stringValue) {
+            return intValue
+        }
+        if let doubleValue = Double(number.stringValue) {
+            return doubleValue
+        }
+        return number.stringValue
+    }
+    if let dict = value as? NSDictionary {
+        return normalizedDictionary(dict)
+    }
+    if let dict = value as? [AnyHashable: Any] {
+        return normalizedDictionary(dict)
+    }
+    if let array = value as? [Any] {
+        return array.map(normalizedJSONValue)
+    }
+    return String(describing: value)
+}
+
+private func hexString(_ data: Data) -> String {
+    data.map { String(format: "%02X", $0) }.joined()
+}
+
+private func prettyPrintedJSONString(_ value: Any) -> String? {
+    guard JSONSerialization.isValidJSONObject(value),
+          let data = try? JSONSerialization.data(withJSONObject: value, options: [.prettyPrinted, .sortedKeys]),
+          let string = String(data: data, encoding: .utf8)
+    else {
+        return nil
+    }
+    return string
+}
  ```

  </details>
- 🟡 `lara/views/tweaks/TweaksView.swift` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -31,6 +31,8 @@ struct TweaksView: View {
                 Section(header: HeaderLabel(text: "Apps", icon: "app")) {
                     NavigationLink("Card Overwrite", destination: CardView())
                         .disabled(!mgr.vfsready)
+                    NavigationLink("Keychain Reader", destination: KeychainView())
+                        .disabled(!mgr.sbxready)
                     NavigationLink("App Decrypt", destination: DecryptView())
                         .disabled(!mgr.sbxready)
                     NavigationLink("3 App Bypass", destination: AppsView())
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 611eded: in-dev
- : 📝 Commit ab5fdfc: fix
- : 📝 Commit 11630f6: test cache
- : 📝 Commit 889f3be: fix pac strategy
- : 📝 Commit cf0a55c: backend workable
- : 📝 Commit 411201c: full function UI
- : 📝 Commit 4c66b06: move objc lifetime control sel to keychain
- : 📝 Commit 76adae4: Add warning and fix crash
- : 📝 Commit a870fde: ObjC code clean and enhance exception handling
- : 📝 Commit b359682: sign
- : 📝 Commit ecb1c33: seems no need to use misaligned
- : 📝 Commit ed69dc5: clean header
- : 📝 Commit 4d70c1f: Update Readme.md
- : 📝 Commit ad2136a: Only allow App Store and self-signed apps, block self RemoteCall.
- : 📝 Commit 62f1f23: (empty) instead of (empty file)
- : 📝 Commit 1854739: add more infomation
- : 📝 Commit e6cb7bd: little fix


---

*Exported on 10/14/2026, 3:12:25 PM from [https://github.com/rooootdev/lara/pull/311](https://github.com/rooootdev/lara/pull/311)*
