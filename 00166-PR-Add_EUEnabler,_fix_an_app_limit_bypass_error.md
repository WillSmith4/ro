# Add EUEnabler, fix an app limit bypass error

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#166](https://github.com/rooootdev/lara/pull/166)  
> **State:** 🟣 merged  
> **Branch:** `khanhduytran0:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 22, 2026  
> **Changes:** +349 / -16 across 10 files  
> **Created:** Apr 22, 2026  
> **Updated:** Apr 22, 2026  
> **Closed:** Apr 22, 2026  

---

## Description

*Opened by [@khanhduytran0](https://github.com/khanhduytran0)* *on Apr 22, 2026*


With this you can install EU/Japan marketplace apps. There are 2 buttons:
- Overwrite eligibility: this RemoteCall `os_eligibility_force_domain_answer` on eligibilityd to force certain domains to eligible. This is persistent so only needed to apply once.
- Enable Spoof EU Region: this spoof the region to Poland when requesting `https://amp-api.apps-marketplace.apple.com/v1/catalog/<country code>/...`, otherwise it will error out. Poland was chosen because there is a method `-[PHPhotoLibrary vcp_description]` returning `PL` string that we could swizzle to it. This is not persistent.

I also included a fix for `Couldn't get com.apple.installd.validatedByFreeProfile EA from` error of app limit bypass


❤️ 4


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- 🔴 [build](https://github.com/rooootdev/lara/actions/runs/24761428176/job/72445869641) — **failure** — *GitHub Actions*
- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24761428176/job/72445940365) — **skipped** — *GitHub Actions*


---

## PR Commits (2)

- [`cdb0810`](https://github.com/rooootdev/lara/commit/cdb0810a817fc59039eea1a2cde9507ff09b97a2) Fix "Couldn't get com.apple.installd.validatedByFreeProfile EA from" of app limit bypass — *khanhduytran0* · Apr 21, 2026
- [`f0d7c09`](https://github.com/rooootdev/lara/commit/f0d7c090dfdaadc4fa3e6a419ad3cb5d28b73e7b) Add EUEnabler — *khanhduytran0* · Apr 22, 2026


---

## Changed Files (10)

- 🟡 `lara/classes/laramgr.swift` (modified) — +37 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -79,6 +79,11 @@ final class laramgr: ObservableObject {
     @Published var testresult: String?
     #if !DISABLE_REMOTECALL
     @Published var rcrunning: Bool = false
+    @Published var eligibilitystate: Bool?
+    @Published var eu1progress: Double = 0.0
+    @Published var eu1running: Bool = false
+    @Published var eu2progress: Double = 0.0
+    @Published var eu2running: Bool = false
     #endif
     
     @Published var vfsready: Bool = false
@@ -539,6 +544,38 @@ final class laramgr: ObservableObject {
         }
     }
     
+    func rcinitDaemon(serviceName: String, framework: String? = nil, process: String, migbypass: Bool = false, completion: ((RemoteCall?) -> Void)? = nil) {
+        guard dsready, let sbProc else {
+            completion?(nil)
+            return
+        }
+        
+        rcrunning = true
+        logmsg("initializing remote call on \(process)...")
+        
+        DispatchQueue.global(qos: .userInitiated).async { [weak self] in
+            if process.withCString({ proc_find_by_name($0) == 0 }) {
+                wake_up_daemon(sbProc, serviceName, framework)
+                sleep(1) // give the daemon some time to start up
+            }
+            
+            let proc = RemoteCall(process: process, useMigFilterBypass: migbypass)
+            completion?(proc)
+            
+            DispatchQueue.main.async {
+                guard let self = self else { return }
+                let success = proc != nil
+                if success {
+                    self.logmsg("remote call initialized on \(process)")
+                    self.rcrunning = false
+                } else {
+                    self.logmsg("remote call init failed on \(process)")
+                    self.rcrunning = false
+                }
+            }
+        }
+    }
+    
     func rcdestroy(completion: (() -> Void)? = nil) {
         guard rcready else { return }
         
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/PrivateAPI.h` (modified) — +31 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -5,10 +5,41 @@
 //  Created by Duy Tran on 20/4/26.
 //
 
+@import Foundation;
 @import QuartzCore;
 
 #define CA_DEBUG_FLAG_PERFORMANCE_HUD 0x10000000
 extern void CARenderServerGetDebugFlags(mach_port_t port, int key);
 extern void CARenderServerGetDebugValue(mach_port_t port, int key);
 extern void CARenderServerSetDebugFlags(mach_port_t port, int key, int value);
 extern void CARenderServerSetDebugValue(mach_port_t port, int key, int value);
+
+// os_eligibility.h
+typedef CF_ENUM(uint64_t, EligibilityDomainType) {
+    /// Install marketplace-distributed apps
+    EligibilityDomainTypeHydrogen = 2,
+    /// Update/restore marketplace-distributed apps
+    EligibilityDomainTypeHelium = 3,
+    /// Update/restore web browser engine host apps
+    EligibilityDomainTypeLithium = 4,
+    /// Update/restore embedded web engine apps
+    EligibilityDomainTypeBoron = 6,
+    /// Install web browser engine host apps
+    EligibilityDomainTypeCarbon = 7,
+    /// Install apps distributed via the web
+    EligibilityDomainTypeArgon = 19,
+    /// Update/restore apps distributed via the web
+    EligibilityDomainTypePotassium = 20,
+    /// Install embedded web engine apps
+    EligibilityDomainTypeTitanium = 23,
+    /// iPhone Mirroring
+    EligibilityDomainTypeIron = 27,
+};
+typedef CF_ENUM(uint64_t, EligibilityAnswer) {
+    EligibilityAnswerEligible = 4,
+};
+typedef CF_ENUM(uint64_t, EligibilityAnswerSource) {
+    EligibilityAnswerSourceInvalid = 0,
+    EligibilityAnswerSourceComputed = 1,
+    EligibilityAnswerSourceForced = 2,
+};
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.h` (modified) — +4 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -75,17 +75,18 @@ uint64_t remote_pac(uint64_t remoteThreadAddr, uint64_t address, uint64_t modifi
     uint32_t _selfThreadCtid;
     arm_thread_state64_internal _originalState;
     uint64_t _vmMap;
-    uint64_t _callThreadAddr;
-    uint64_t _trojanThreadAddr;
-    int _pid;
     bool _success; // = true;
     //NSMutableArray<NSNumber *> *_threadList = nil;
     //uint64_t _trojanMem;
     struct VMShmem _shmemCache[SHMEM_CACHE_SIZE];
 }
 
+@property(nonatomic) NSString *lastError;
 @property(nonatomic) NSMutableArray<NSNumber *> *threadList;
 @property(nonatomic) uint64_t trojanMem;
+@property(nonatomic) pid_t pid;
+@property(nonatomic) uint64_t callThreadAddr;
+@property(nonatomic) uint64_t trojanThreadAddr;
 
 - (NSUInteger)doRemoteCallStableWithTimeout:(int)timeout functionName:(char *)name functionPointer:(void *)ptr
                             args:(uint64_t *)args argCount:(NSUInteger)argCount;
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.m` (modified) — +7 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -279,6 +279,12 @@ - (NSUInteger)doRemoteCallInternalTimeout:(int)timeout exceptionPort:(mach_port_
         printf("(rc) Don't receive second exception on %s thread\n", threadStr);
         return 0;
     }
+    if (native_strip(exc2.threadState.__lr) != lrMarker) {
+        printf("[%s:%d] Process might have crashed! Unexpected LR: 0x%llx (expected 0x%llx)\n", __FUNCTION__, __LINE__, exc2.threadState.__lr, lrMarker);
+        NSLog(@"%@", NSThread.callStackSymbols);
+        [self destroyRemoteCall];
+        self.lastError = [NSString stringWithFormat:@"Process might have crashed! Unexpected LR: 0x%llx (expected 0x%llx)", exc2.threadState.__lr, lrMarker];
+    }
     uint64_t retValue = exc2.threadState.__x[0];
     reply_with_state(&exc2, &exc2.threadState);
     printf("(rc) %s func's retValue = 0x%llx(%llu)\n", name, retValue, retValue);
@@ -503,7 +509,7 @@ - (uint64_t)retryFirstThreadWithMigFilterBypass:(BOOL)useMigFilterBypass {
 //int init_remote_call(const char* process, bool useMigFilterBypass) {
 - (int)initRemoteCallForProcess:(const char *)process useMigFilterBypass:(BOOL)useMigFilterBypass {
     uint64_t procAddr = proc_find_by_name(process);
-    if (procAddr == -1) {
+    if (!procAddr) {
         printf("(rc) Unable to find process: %s\n", process);
         return -1;
     }
  ```

  </details>
- 🟡 `lara/kexploit/darksword.m` (modified) — +5 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -543,12 +543,15 @@ static void set_target_kaddr(uint64_t where) {
     if (res != 0) { pe_log("setsockopt failed (set_target_kaddr)!"); }
 }
 
+static pthread_mutex_t krwLock = PTHREAD_MUTEX_INITIALIZER;
 static void early_kread(uint64_t where, void *read_buf, uint64_t size) {
+    pthread_mutex_lock(&krwLock);
     if (size > EARLY_KRW_LENGTH) { pe_log("[!] error: size > EARLY_KRW_LENGTH"); return; }
     set_target_kaddr(where);
     socklen_t read_data_length = (socklen_t)size;
     int res = getsockopt(rw_socket, IPPROTO_ICMPV6, ICMP6_FILTER, read_buf, &read_data_length);
     if (res != 0) { pe_log("getsockopt failed (early_kread)!"); }
+    pthread_mutex_unlock(&krwLock);
 }
 
 static uint64_t early_kread64(uint64_t where) {
@@ -558,10 +561,12 @@ static uint64_t early_kread64(uint64_t where) {
 }
 
 static void early_kwrite32bytes(uint64_t where, void *write_buf) {
+    pthread_mutex_lock(&krwLock);
     set_target_kaddr(where);
     int res = setsockopt(rw_socket, IPPROTO_ICMPV6, ICMP6_FILTER,
                           write_buf, (socklen_t)EARLY_KRW_LENGTH);
     if (res != 0) { pe_log("setsockopt failed (early_kwrite32bytes)!"); }
+    pthread_mutex_unlock(&krwLock);
 }
 
 static void early_kwrite64(uint64_t where, uint64_t what) {
  ```

  </details>
- 🟡 `lara/kexploit/pe/rc.h` (modified) — +5 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -10,6 +10,8 @@
 
 #import "RemoteCall.h"
 
+typedef void (^rc_euenabler_callback_t)(double progress);
+
 void status_bar_tweak(RemoteCall *proc);
 int hide_icon_labels(RemoteCall *proc);
 int enable_jit(RemoteCall *proc, const char *bundleID);
@@ -20,5 +22,8 @@ int enable_floating_dock(RemoteCall *proc);
 int enable_grid_app_switcher(RemoteCall *proc);
 int get_performance_hud(RemoteCall *proc);
 int set_performance_hud(RemoteCall *proc, int selection);
+void wake_up_daemon(RemoteCall *sb, NSString *daemon, NSString *frameworkContainingXPCSerice);
+int euenabler_overwrite_eligibility(RemoteCall *proc);
+void euenabler_override_country_code(RemoteCall *proc, rc_euenabler_callback_t callback);
 
 #endif /* rc_h */
  ```

  </details>
- 🟡 `lara/kexploit/pe/rc.m` (modified) — +142 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -457,3 +457,145 @@ int set_performance_hud(RemoteCall *proc, int selection) {
     RemoteArbCall(proc, CARenderServerSetDebugFlags, 0, CA_DEBUG_FLAG_PERFORMANCE_HUD, flags);
     return 0;
 }
+
+void wake_up_daemon(RemoteCall *sb, NSString *daemon, NSString *frameworkContainingXPCSerice) {
+    uint64_t memRemote = sb.trojanMem;
+    
+    // special case for certain daemons
+    if ([daemon isEqualToString:@"appstorecomponentsd"]) {
+        // [[ASCServicesConnection sharedConnection] testConnection]
+        uint64_t cls = remote_getClass(sb, "ASCServicesConnection");
+        uint64_t selShared = remote_sel(sb, "sharedConnection");
+        uint64_t selTest = remote_sel(sb, "testConnection");
+        uint64_t connection = remote_msg(sb, cls, selShared, 0,0,0,0);
+        if (connection) {
+            remote_msg(sb, connection, selTest, 0,0,0,0);
+        }
+        return;
+    }
+    
+    if (frameworkContainingXPCSerice) {
+        sb[memRemote].string = frameworkContainingXPCSerice;
+        //@"/System/Library/PrivateFrameworks/DVTInstrumentsFoundation.framework/DVTInstrumentsFoundation";
+        RemoteArbCall(sb, dlopen, memRemote, 0);
+    }
+    
+    sb[memRemote].string = daemon;
+    uint64_t conn;
+    if (frameworkContainingXPCSerice) {
+        // for XPC services embedded in a certain framework, eg. com.apple.dt.instruments.dtsecurity.xpc
+        conn = RemoteArbCall(sb, xpc_connection_create, memRemote, 0);
+    } else {
+        // for Mach/XPC services registered in launchd.plist
+        void *xpc_connection_create_mach_service = dlsym(RTLD_DEFAULT, "xpc_connection_create_mach_service");
+        conn = RemoteArbCall(sb, xpc_connection_create_mach_service, memRemote, 0);
+    }
+    // ^(xpc_object_t event) {}
+    // Construct a block
+    RemoteArbCall(sb, bzero, memRemote, PAGE_SIZE);
+    uint64_t block = memRemote;
+    uint64_t descriptor = block + 0x20; // right next to block
+    sb[block].value64 = remote_getClass(sb, "__NSGlobalBlock__"); // isa
+    sb[block+0x8].value32 = (1<<30) | (1<<28); //flags: BLOCK_HAS_SIGNATURE | BLOCK_IS_GLOBAL;
+    sb[block+0x10].value64 = remote_pac(sb.callThreadAddr, (uint64_t)getpid, ptrauth_blend_discriminator((void *)(block+0x10), 0));
+    sb[block+0x18].value64 = descriptor;
+    sb[descriptor].value64 = 0; // reserved
+    sb[descriptor+0x8].value64 = 0x20; // size
+    RemoteArbCall(sb, xpc_connection_set_event_handler, conn, block);
+    
+    RemoteArbCall(sb, xpc_connection_resume, conn, 0);
+    uint64_t msg = RemoteArbCall(sb, xpc_dictionary_create, 0, 0, 0);
+    RemoteArbCall(sb, xpc_connection_send_message, conn, msg, 0);
+    RemoteArbCall(sb, xpc_release, msg, 0);
+    RemoteArbCall(sb, xpc_release, conn, 0);
+//    xpc_connection_t conn = xpc_connection_create("com.apple.dt.instruments.dtsecurity.xpc", 0);
+//    xpc_connection_set_event_handler(conn, ^(id event) {});
+//    xpc_connection_resume(conn);
+//    xpc_connection_send_message(conn, xpc_dictionary_create(NULL, NULL, 0));
+}
+
+int euenabler_overwrite_eligibility(RemoteCall *proc) {
+    void *os_eligibility_force_domain_answer = dlsym(RTLD_DEFAULT, "os_eligibility_force_domain_answer");
+    // probably old iOS, skip this
+    if (!os_eligibility_force_domain_answer) return -1;
+    int result = (int)RemoteArbCall(proc, os_eligibility_force_domain_answer, EligibilityDomainTypeHydrogen, EligibilityAnswerEligible, 0);
+    RemoteArbCall(proc, os_eligibility_force_domain_answer, EligibilityDomainTypeHelium, EligibilityAnswerEligible, 0);
+    RemoteArbCall(proc, os_eligibility_force_domain_answer, EligibilityDomainTypeLithium, EligibilityAnswerEligible, 0);
+    RemoteArbCall(proc, os_eligibility_force_domain_answer, EligibilityDomainTypeBoron, EligibilityAnswerEligible, 0);
+    RemoteArbCall(proc, os_eligibility_force_domain_answer, EligibilityDomainTypeCarbon, EligibilityAnswerEligible, 0);
+    RemoteArbCall(proc, os_eligibility_force_domain_answer, EligibilityDomainTypeArgon, EligibilityAnswerEligible, 0);
+    RemoteArbCall(proc, os_eligibility_force_domain_answer, EligibilityDomainTypePotassium, EligibilityAnswerEligible, 0);
+    RemoteArbCall(proc, os_eligibility_force_domain_answer, EligibilityDomainTypeTitanium, EligibilityAnswerEligible, 0);
+    RemoteArbCall(proc, os_eligibility_force_domain_answer, EligibilityDomainTypeIron, EligibilityAnswerEligible, 0);
+    // return first one
+    return result;
+}
+
+// Extremely hack here: this need to swizzle countryCodeOverride methods to return one of EU countries, since we can't just hook or inject code
+// It does this by finding a certain `return @"<country code>";` gadget, and swizzling in both managedappdistributiond and appstorecomponentsd
+// We find that /System/Library/PrivateFrameworks/MediaAnalysis.framework/MediaAnalysis has -[PHPhotoLibrary vcp_description] which could return PL aka Poland country code.
+// However, said method calls 2 other methods, so we add these fake methods to make them return 0, its implementation is as follows:
+#if 0
+@implementation PHPhotoLibrary(MediaAnalysis)
+- (NSString *)vcp_description {
+    if ([self isSystemPhotoLibrary]) {
+        return @"SPL";
+    }
+    if ([self wellKnownPhotoLibraryIdentifier] == 3) {
+        return @"SyndPL";
+    }
+    return @"PL";
+}
+@end
+#endif
+void euenabler_override_country_code(RemoteCall *proc, rc_euenabler_callback_t callback) {
+    callback(0.2);
+    uint64_t mem = proc.trojanMem;
+    proc[mem].string = @"/System/Library/PrivateFrameworks/MediaAnalysis.framework/MediaAnalysis";
+    RemoteArbCall(proc, dlopen, mem, RTLD_GLOBAL);
+    
+    uint64_t cls1 = remote_getClass(proc, "ASCLockupBatchRequest");
+    uint64_t cls2 = remote_getClass(proc, "ASCLockupRequest");
+    uint64_t clsReturnPL = remote_getClass(proc, "PHPhotoLibrary");
+    
+    uint64_t selCountryCodeOverride = remote_sel(proc, "countryCodeOverride");
+    uint64_t selFakeMethod1 = remote_sel(proc, "isSystemPhotoLibrary");
+    uint64_t selFakeMethod2 = remote_sel(proc, "wellKnownPhotoLibraryIdentifier");
+    uint64_t selReturnNil = remote_sel(proc, "platformOverride");
+    uint64_t selReturnPL = remote_sel(proc, "vcp_description");
+    callback(0.4);
+    
+    // Add fake isSystemPhotoLibrary and wellKnownPhotoLibraryIdentifier methods that return 0, so that vcp_description will return "PL"
+    uint64_t methodReturnNil = RemoteArbCall(proc, class_getInstanceMethod, cls1, selReturnNil);
+    uint64_t returnNilImp = RemoteArbCall(proc, method_getImplementation, methodReturnNil);
+    uint64_t fakeMethodEncoding = RemoteArbCall(proc, method_getTypeEncoding, methodReturnNil);
+    RemoteArbCall(proc, class_addMethod, cls1, selFakeMethod1, (uint64_t)returnNilImp, fakeMethodEncoding);
+    RemoteArbCall(proc, class_addMethod, cls1, selFakeMethod2, (uint64_t)returnNilImp, fakeMethodEncoding);
+    RemoteArbCall(proc, class_addMethod, cls2, selFakeMethod1, (uint64_t)returnNilImp, fakeMethodEncoding);
+    RemoteArbCall(proc, class_addMethod, cls2, selFakeMethod2, (uint64_t)returnNilImp, fakeMethodEncoding);
+    callback(0.6);
+    
+    // Swizzle countryCodeOverride -> vcp_description
+    uint64_t methodReturnPL = RemoteArbCall(proc, class_getInstanceMethod, clsReturnPL, selReturnPL);
+    uint64_t method1 = RemoteArbCall(proc, class_getInstanceMethod, cls1, selCountryCodeOverride);
+    uint64_t method2 = RemoteArbCall(proc, class_getInstanceMethod, cls2, selCountryCodeOverride);
+    uint64_t returnPLImp = RemoteArbCall(proc, method_getImplementation, methodReturnPL);
+    RemoteArbCall(proc, method_setImplementation, method1, (uint64_t)returnPLImp);
+    RemoteArbCall(proc, method_setImplementation, method2, (uint64_t)returnPLImp);
+    callback(0.8);
+    
+    // Swizzle some methods in LSBundleRecord to bypass "Not bypassing eligibility for com.apple.mobilesafari:personal (isProfileValidated: false isUPPValidated:false isBeta:false"
+    uint64_t clsLSBundleRecord = remote_getClass(proc, "LSBundleRecord");
+    if (clsLSBundleRecord) {
+        uint64_t selIsProfileValidated = remote_sel(proc, "isProfileValidated");
+        uint64_t selReturn1 = remote_sel(proc, "supportsSecureCoding");
+        // uint64_t selIsUPPValidated = remote_sel(proc, "isUPPValidated");
+        // uint64_t selIsBeta = remote_sel(proc, "isBeta");
+        uint64_t methodProfile = RemoteArbCall(proc, class_getInstanceMethod, clsLSBundleRecord, selIsProfileValidated);
+        uint64_t methodReturn1 = RemoteArbCall(proc, class_getClassMethod, clsLSBundleRecord, selReturn1);
+        //uint64_t return0Imp = (uint64_t)RemoteArbCall(proc, method_getImplementation, methodProfile);
+        uint64_t return1Imp = (uint64_t)RemoteArbCall(proc, method_getImplementation, methodReturn1);
+        RemoteArbCall(proc, method_setImplementation, methodProfile, return1Imp);
+    }
+    callback(1.0);
+}
  ```

  </details>
- 🟡 `lara/kexploit/utils.m` (modified) — +14 / -10
  <details><summary>Diff</summary>

  ```diff
  @@ -379,14 +379,18 @@ uint64_t taskbyproc(uint64_t procaddr) {
     return pr_task;
 }
 
-char procNameTmp[40];
-char* proc_get_p_name(uint64_t proc) {
-    if(!proc)   return NULL;
-    memset(procNameTmp, 0, 40);
+typedef struct { char data[40]; } StringWrapper;
+StringWrapper proc_get_p_name(uint64_t proc) {
+    StringWrapper procNameTmp = {0};
+    if(!proc)   return procNameTmp;
+    memset(procNameTmp.data, 0, 40);
     // Fix unaligned read panic. TODO: move to early_kread
     uint64_t off = off_proc_p_name & 0x7;
-    ds_kreadbuf(proc + off_proc_p_name - off, &procNameTmp, 40);
-    return procNameTmp + off;
+    ds_kreadbuf(proc + off_proc_p_name - off, &procNameTmp.data, 40);
+    memmove(procNameTmp.data, procNameTmp.data + off, 40 - off);
+    procNameTmp.data[32] = '\0';
+    NSLog(@"Proc: %s", procNameTmp.data);
+    return procNameTmp;
 }
 
 uint64_t procbyname(const char *name) {
@@ -402,17 +406,17 @@ uint64_t procbyname(const char *name) {
 
     uint64_t proc = proc_self();
     while (1) {
-        char *p_name = proc_get_p_name(proc);
-        if(p_name != NULL && strcmp(p_name, name) == 0)
+        char *p_name = proc_get_p_name(proc).data;
+        if(strcmp(p_name, name) == 0)
             return proc;
         proc = ds_kread64(proc + off_proc_p_list_le_next);
         if(!proc) break;
     }
     
     proc = proc_self();
     while (1) {
-        char *p_name = proc_get_p_name(proc);
-        if(p_name != NULL && strcmp(p_name, name) == 0)
+        char *p_name = proc_get_p_name(proc).data;
+        if(strcmp(p_name, name) == 0)
             return proc;
         proc = ds_kread64(proc + off_proc_p_list_le_prev);
         if(!proc) return 0;    // not found
  ```

  </details>
- 🟡 `lara/views/AppsView.swift` (modified) — +1 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -101,7 +101,6 @@ struct AppsView: View {
                     guard access(mp, F_OK) == 0 else { continue }
 
                     let testkey = "com.apple.installd.validatedByFreeProfile"
-                    var value: [UInt8] = [1, 2, 3]
                     
                     let success = mgr.apfsown(path: bundlepath, uid: 501, gid: 501)
                     if !success {
@@ -111,7 +110,7 @@ struct AppsView: View {
                     }
 
                     errno = 0
-                    let rc = setxattr(bundlepath, testkey, &value, value.count, 0, 0)
+                    let rc = removexattr(bundlepath, testkey, 0)
                     if rc == 0 {
                         mgr.logmsg("(sbx) set xattr on: \(bundlepath)")
                         processed += 1
  ```

  </details>
- 🟡 `lara/views/ContentView.swift` (modified) — +103 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -430,12 +430,115 @@ struct ContentView: View {
                     } header: {
                         Text("RemoteCall")
                     } footer: {
+                        if let error = mgr.sbProc?.lastError {
+                            Text("RemoteCall error: \(error)")
+                                .foregroundColor(.red)
+                        }
                         if isdebugged() {
                             Text("Not available when a debugger is attached.")
                         }
                         Text("RemoteCall is still in development and may not work properly 100% of the time.")
                     }
                     .disabled(mgr.rcrunning)
+
+                    if #available(iOS 17.4, *) {
+                        Section {
+                            Button {
+                                mgr.rcinitDaemon(serviceName: "com.apple.xpc.amsaccountsd", process: "amsaccountsd", migbypass: false) { proc in
+                                    guard let proc else {
+                                        mgr.logmsg("rc init failed")
+                                        return
+                                    }
+                                    mgr.logmsg("rc init succeeded!")
+                                    mgr.eligibilitystate = euenabler_overwrite_eligibility(proc) == 0
+                                    mgr.logmsg("overwrite_eligibility() returned: \(mgr.eligibilitystate! ? "success" : "failure")")
+                                    proc.destroy()
+                                }
+                            } label: {
+                                HStack {
+                                    Text("Overwrite eligibility (one time setup)")
+                                    if let state = mgr.eligibilitystate {
+                                        Spacer()
+                                        if state {
+                                            Image(systemName: "checkmark.circle")
+                                                .foregroundColor(.green)
+                                        } else {
+                                            Image(systemName: "xmark.circle")
+                                                .foregroundColor(.red)
+                                        }
+                                    }
+                                }
+                            }
+                            .disabled(mgr.eligibilitystate ?? false)
+                            Button {
+                                mgr.eu1progress = 0.0
+                                mgr.eu2progress = 0.0
+                                mgr.eu1running = true
+                                mgr.eu2running = true
+                                // fix App is unavailable
+                                mgr.rcinitDaemon(serviceName: "com.apple.managedappdistributiond.xpc", process: "managedappdistributiond", migbypass: false) { proc in
+                                    guard let proc else {
+                                        mgr.logmsg("rc init failed")
+                                        mgr.eu1running = false
+                                        return
+                                    }
+                                    mgr.logmsg("rc init succeeded!")
+                                    euenabler_override_country_code(proc) { progress in
+                                        DispatchQueue.main.async {
+                                            self.mgr.eu1progress = progress
+                                        }
+                                    }
+                                    proc.destroy()
+                                    DispatchQueue.main.async {
+                                        mgr.eu1running = false
+                                    }
+                                }
+                                // fix unable to load app info
+                                mgr.rcinitDaemon(serviceName: "com.apple.appstorecomponentsd.xpc", process: "appstorecomponentsd", migbypass: false) { proc in
+                                    guard let proc else {
+                                        mgr.logmsg("rc init failed")
+                                        mgr.eu2running = false
+                                        return
+                                    }
+                                    mgr.logmsg("rc init succeeded!")
+                                    euenabler_override_country_code(proc) { progress in
+                                        DispatchQueue.main.async {
+                                            self.mgr.eu2progress = progress
+                                        }
+                                    }
+                                    proc.destroy()
+                                    DispatchQueue.main.async {
+                                        mgr.eu2running = false
+                                    }
+                                }
+                            } label: {
+                                HStack {
+                                    if mgr.eu1running || mgr.eu2running {
+                                        ProgressView(value: (mgr.eu1progress + mgr.eu2progress)/2)
+                                            .progressViewStyle(.circular)
+                                            .frame(width: 18, height: 18)
+                                        Text("Running...")
+                                        Spacer()
+                                        Text("\(Int((mgr.eu1progress + mgr.eu2progress)/2 * 100))%")
+                                    } else {
+                                        Text("Enable Spoof EU Region")
+                                        Spacer()
+                                        if mgr.eu1progress + mgr.eu2progress == 2 {
+                                            Image(systemName: "checkmark.circle")
+                                                .foregroundColor(.green)
+                                        } else if mgr.dsattempted && mgr.dsfailed {
+                                            Image(systemName: "xmark.circle")
+                                                .foregroundColor(.red)
+                                        }
+                                    }
+                                }
+                            }
+                            .disabled(mgr.eu1running || mgr.eu2running || mgr.eu1progress+mgr.eu2progress == 2)
+                        } footer: {
+                            Text("Enables installing of EU/Japan Marketplace apps.")
+                        }
+                        .disabled(isdebugged() || mgr.rcrunning || !mgr.rcready)
+                    }
                     #endif
 
                     Section {
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit cdb0810: Fix "Couldn't get com.apple.installd.validatedByFreeProfile EA from" of app limit bypass
- : 📝 Commit f0d7c09: Add EUEnabler
- Apr 22, 2026: ✏️ @khanhduytran0 renamed: "Add EUEnabler" → "Add EUEnabler, fix an app limit bypass error"
- Apr 22, 2026: 🟣 @rooootdev merged
- Apr 22, 2026: 🔴 Closed by @rooootdev


---

## Comments (1)

### Comment #1 — [@kesierzg](https://github.com/kesierzg) · Apr 22, 2026

`ID: 4296462597`


POLAND STRONG


---

*Exported on 10/14/2026, 2:59:46 PM from [https://github.com/rooootdev/lara/pull/166](https://github.com/rooootdev/lara/pull/166)*
