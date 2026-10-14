# Fix Floating Dock not auto hiding (partial)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#214](https://github.com/rooootdev/lara/pull/214)  
> **State:** 🟣 merged  
> **Branch:** `khanhduytran0:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 30, 2026  
> **Changes:** +131 / -21 across 5 files  
> **Created:** Apr 30, 2026  
> **Updated:** Apr 30, 2026  
> **Closed:** Apr 30, 2026  

---

## Description

*Opened by [@khanhduytran0](https://github.com/khanhduytran0)* *on Apr 30, 2026*


This makes floating dock mostly functional now. Some issue with this is the dock isn’t hidden when going to App Library and opening App Library from floating dock doesn’t work.

For developers, I’ve added some stuff to RemoteCall:
- `-[RemoteCall doRemoteCallSyncOnMainThread:]`: lets you run a full block of remote calls on process’ main thread. Useful if you wanna get the return value of a method requiring main thread.
- `-[RemotePointer valueDouble/setValueDouble:]`: get/set a double value
- `remote_getCGRect`/`remote_setCGRect`: proper way to call `setFrame:` etc. Might also be copied to stuff like setAlpha, etc


👍 1 · ❤️ 1


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25163321318/job/73763195528) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25163321318/job/73763047478) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`f8d4a95`](https://github.com/rooootdev/lara/commit/f8d4a95535b5d750b450e497b44c76eeb2c787f8) Fix Floating Dock not auto hiding — *khanhduytran0* · Apr 30, 2026


---

## Changed Files (5)

- 🟡 `lara/kexploit/TaskRop/PrivateAPI.h` (modified) — +8 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -8,8 +8,9 @@
 @import Foundation;
 @import QuartzCore;
 
-id _Nullable objc_alloc(Class _Nullable cls);
-void objc_release(id _Nullable obj);
+id objc_alloc(Class cls);
+void objc_release(id obj);
+pthread_t pthread_main_thread_np(void);
 
 #define CA_DEBUG_FLAG_PERFORMANCE_HUD 0x10000000
 extern void CARenderServerGetDebugFlags(mach_port_t port, int key);
@@ -49,3 +50,8 @@ typedef CF_ENUM(uint64_t, EligibilityAnswerSource) {
     EligibilityAnswerSourceComputed = 1,
     EligibilityAnswerSourceForced = 2,
 };
+
+@interface CAMetalDrawable : NSObject
+- (CGRect)dirtyRect;
+- (void)setDirtyRect:(CGRect)rect;
+@end
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.h` (modified) — +5 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -97,6 +97,7 @@ uint64_t remote_pac(uint64_t remoteThreadAddr, uint64_t address, uint64_t modifi
 + (BOOL)isLiveProcessRuntime;
 - (NSUInteger)doRemoteCallStableWithTimeout:(int)timeout functionName:(char *)name functionPointer:(void *)ptr
                             args:(uint64_t *)args argCount:(NSUInteger)argCount;
+- (BOOL)doRemoteCallSyncOnMainThread:(BOOL (^)(void))block;
 - (int)destroyRemoteCall;
 - (BOOL)remoteRead:(uint64_t)src to:(void *)dst size:(uint64_t)size;
 - (uint64_t)remoteRead64From:(uint64_t)src;
@@ -120,11 +121,13 @@ uint64_t remote_pac(uint64_t remoteThreadAddr, uint64_t address, uint64_t modifi
 - (void)setValue16:(uint16_t)val;
 - (void)setValue32:(uint32_t)val;
 - (void)setValue64:(uint64_t)val;
+- (void)setValueDouble:(CGFloat)val;
 - (NSString *)string;
 - (uint8_t)value8;
 - (uint16_t)value16;
 - (uint32_t)value32;
 - (uint64_t)value64;
+- (CGFloat)valueDouble;
 @end
 
 #define RemoteArbCallTempWithTimeout(timeout, instance, _pc, ...) [instance doRemoteCallTempWithTimeout:timeout functionName:(char *)#_pc functionPointer:(void *)(_pc) args:(uint64_t[]){__VA_ARGS__} argCount:sizeof((uint64_t[]){__VA_ARGS__})/sizeof(uint64_t)]
@@ -137,5 +140,7 @@ uint64_t remote_getClass(RemoteCall *proc, const char *name);
 uint64_t remote_msg(RemoteCall *proc, uint64_t obj, uint64_t sel, uint64_t a0, uint64_t a1, uint64_t a2, uint64_t a3);
 int remote_errno(RemoteCall *proc);
 uint64_t remote_NSString(RemoteCall *proc, const char *str);
+CGRect remote_getCGRect(RemoteCall *proc, uint64_t obj, uint64_t sel);
+void remote_setCGRect(RemoteCall *proc, uint64_t obj, uint64_t sel, CGRect newRect);
 
 #endif /* RemoteCall_h */
  ```

  </details>
- 🟡 `lara/kexploit/TaskRop/RemoteCall.m` (modified) — +87 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -548,7 +548,9 @@ - (NSUInteger)doRemoteCallInternalTimeout:(int)timeout exceptionPort:(mach_port_
     }
 
     if (timeout < 0) {
-        printf("(rc) Trojan thread cleanup\n");
+        if (ptr == pthread_exit) {
+            printf("(rc) Trojan thread cleanup\n");
+        }
         return 0;
     }
 
@@ -579,6 +581,45 @@ - (NSUInteger)doRemoteCallInternalTimeout:(int)timeout exceptionPort:(mach_port_
     return retValue;
 }
 
+// performSelectorOnMainThread doesn't return result, so we run the entire code block in main thread
+- (BOOL)doRemoteCallSyncOnMainThread:(BOOL (^)(void))block {
+    if (!_creatingExtraThread) {
+        return block();
+    }
+    
+    // Set exception port on main thread
+    uint64_t oldTrojanThreadAddr = _trojanThreadAddr;
+    _trojanThreadAddr = ds_kread64(_taskAddr + off_task_threads_next);
+    [self setExceptionPortOnThread:_secondExceptionPort forThread:_trojanThreadAddr useMigFilterBypass:NO];
+    
+    // Make main thread call FAKE_LR_TROJAN. Yes, dispatch_get_main_queue returns the same pointer across processes.
+    uint64_t signedFakePC = remote_pac(_trojanThreadAddr, FAKE_PC_TROJAN, 0);
+    RemoteArbCallWithTimeout(-1, self, dispatch_async_and_wait_f, (uint64_t)dispatch_get_main_queue(), 0, signedFakePC);
+    
+    // Now the main thread takes over RemoteCall exception handler, do stuff
+    ExceptionMessage exc;
+    if (!wait_exception(_secondExceptionPort, &exc, 5000, false)) {
+        printf("(rc) Failed to receive exception on main thread\n");
+        return false;
+    }
+    memcpy(&_originalState, &exc.threadState, sizeof(arm_thread_state64_internal));
+    reply_with_state(&exc, &exc.threadState);
+    BOOL result = block();
+    
+    // Restore it
+    wait_exception(_secondExceptionPort, &exc, 1, false);
+    _originalState.__flags = exc.threadState.__flags;
+    [self signState:_trojanThreadAddr withState:&_originalState pc:native_strip((uint64_t)getpid) lr:_originalState.__lr];
+    if (!reply_with_state(&exc, &_originalState)) {
+        return false;
+    }
+    
+    // Now we're back to call thread, remove exception handler
+    [self setExceptionPortOnThread:0 forThread:_trojanThreadAddr useMigFilterBypass:NO];
+    _trojanThreadAddr = oldTrojanThreadAddr;
+    return result;
+}
+
 // bool restore_trojan_thread(arm_thread_state64_internal *state)
 - (BOOL)restoreTrojanThreadWithState:(arm_thread_state64_internal *)state
 {
@@ -1230,6 +1271,9 @@ - (void)setValue32:(uint32_t)val {
 - (void)setValue64:(uint64_t)val {
     [self.remoteCall remote_write:_address from:&val size:sizeof(val)];
 }
+- (void)setValueDouble:(CGFloat)val {
+    [self.remoteCall remote_write:_address from:&val size:sizeof(val)];
+}
 
 - (NSString *)string {
     // I'm lazy to deal with mem leak so I'm just wrapping it to NSString for ARC to do its job
@@ -1262,6 +1306,11 @@ - (uint64_t)value64 {
     [self.remoteCall remoteRead:_address to:&val size:sizeof(val)];
     return val;
 }
+- (CGFloat)valueDouble {
+    CGFloat val = 0;
+    [self.remoteCall remoteRead:_address to:&val size:sizeof(val)];
+    return val;
+}
 @end
 
 uint64_t remote_alloc_str(RemoteCall *proc, const char *str) {
@@ -1304,4 +1353,40 @@ uint64_t remote_NSString(RemoteCall *proc, const char *str) {
     uint64_t result = remote_msg(proc, cls_NSString, sel_stringWithCString, resultCStr, 0, 0, 0);
     RemoteArbCall(proc, free, resultCStr);
     return result;
-}
\ No newline at end of file
+}
+
+// Helper to get/set CGRect from double registers that we cannot modify via normal thread state
+CGRect remote_getCGRect(RemoteCall *proc, uint64_t obj, uint64_t sel) {
+    // Spill CGRect to double registers first
+    remote_msg(proc, obj, sel, 0,0,0,0);
+    
+    // -[CAMetalDrawable setDirtyRect:]: save double registers to address
+    // stp    d0, d1, [x0, #0x20]
+    // stp    d2, d3, [x0, #0x30]
+    // ret
+    uint64_t where = proc.trojanMem;
+    Class class = NSClassFromString(@"CAMetalDrawable");
+    Method method = class_getInstanceMethod(class, @selector(setDirtyRect:));
+    void *setDoubleRegistersImp = method_getImplementation(method);
+    RemoteArbCall(proc, setDoubleRegistersImp, where-0x20);
+    
+    CGRect result;
+    [proc remoteRead:where to:&result size:sizeof(result)];
+    return result;
+}
+
+void remote_setCGRect(RemoteCall *proc, uint64_t obj, uint64_t sel, CGRect newRect) {
+    uint64_t where = proc.trojanMem;
+    [proc remote_write:where from:&newRect size:sizeof(newRect)];
+    // -[CAMetalDrawable dirtyRect]: spill to double registers
+    // ldp    d0, d1, [x0, #0x20]
+    // ldp    d2, d3, [x0, #0x30]
+    // ret
+    Class class = NSClassFromString(@"CAMetalDrawable");
+    Method method = class_getInstanceMethod(class, @selector(dirtyRect));
+    void *setDoubleRegistersImp = method_getImplementation(method);
+    RemoteArbCall(proc, setDoubleRegistersImp, where-0x20);
+    
+    // Now do the actual thing
+    remote_msg(proc, obj, sel, 0,0,0,0);
+}
  ```

  </details>
- 🟡 `lara/kexploit/pe/rc.m` (modified) — +30 / -16
  <details><summary>Diff</summary>

  ```diff
  @@ -373,8 +373,9 @@ int enable_floating_dock(RemoteCall *proc) {
     uint64_t clsSBMainWorkspace = remote_getClass(proc, "SBMainWorkspace");
     
     uint64_t createFloatingDock = remote_sel(proc, "createFloatingDockControllerForWindowScene:");
+    uint64_t selDockListView = remote_sel(proc, "dockListView");
+    uint64_t selIconMgr   = remote_sel(proc, "iconManager");
     uint64_t selMainWindowScene = remote_sel(proc, "mainWindowScene");
-    uint64_t selPerform = remote_sel(proc, "performSelectorOnMainThread:withObject:waitUntilDone:");
     uint64_t selSharedInstance = remote_sel(proc, "sharedInstance");
     uint64_t selRespondsTo = remote_sel(proc, "respondsToSelector:");
     uint64_t selSupported = remote_sel(proc, "isFloatingDockSupported");
@@ -392,21 +393,34 @@ int enable_floating_dock(RemoteCall *proc) {
     uint64_t workspace = remote_msg(proc, clsSBMainWorkspace, selSharedInstance, 0,0,0,0);
     uint64_t mainWindowScene = remote_msg(proc, workspace, selMainWindowScene, 0,0,0,0);
     uint64_t iconController = remote_msg(proc, clsSBIconController, selSharedInstance, 0,0,0,0);
-    if (remote_msg(proc, iconController, selRespondsTo, createFloatingDock, 0,0,0)) {
-        // iOS < 26 has -[SBIconController createFloatingDockControllerForWindowScene:]
-        remote_msg(proc, iconController, selPerform, createFloatingDock, mainWindowScene, 0 /*nil*/, 1 /*YES wait*/);
-    } else {
-        // iOS 26 has -[SBHomeScreenController createFloatingDockControllerForWindowScene:]
-        uint64_t selHomeScreen = remote_sel(proc, "homeScreenController");
-        uint64_t homeScreenController = remote_msg(proc, mainWindowScene, selHomeScreen, 0,0,0,0);
-        remote_msg(proc, homeScreenController, selPerform, createFloatingDock, mainWindowScene, 0 /*nil*/, 1 /*YES wait*/);
-    }
-    
-    // SBMainWorkSpace.sharedInstance.mainWindowScene
-    // +[SBFloatingDockController isFloatingDockUtilitiesSupported]
-    // * frame #0: 0x000000022164406c SpringBoard`-[SBFloatingDockController initWithWindowScene:homeScreenContextProvider:]
-    //   frame #1: 0x0000000221980214 SpringBoard`-[SBHomeScreenController createFloatingDockControllerForWindowScene:] + 68
-    return 0;
+    return ![proc doRemoteCallSyncOnMainThread:^{
+        uint64_t dockController;
+        if (remote_msg(proc, iconController, selRespondsTo, createFloatingDock, 0,0,0)) {
+            // iOS < 26 has -[SBIconController createFloatingDockControllerForWindowScene:]
+            dockController = remote_msg(proc, iconController, createFloatingDock, mainWindowScene, 0,0,0);
+        } else {
+            // iOS 26 has -[SBHomeScreenController createFloatingDockControllerForWindowScene:]
+            uint64_t selHomeScreen = remote_sel(proc, "homeScreenController");
+            uint64_t homeScreenController = remote_msg(proc, mainWindowScene, selHomeScreen, 0,0,0,0);
+            dockController = remote_msg(proc, homeScreenController, createFloatingDock, mainWindowScene, 0,0,0);
+        }
+        
+        uint64_t sceneDelegate = remote_msg(proc, mainWindowScene, remote_sel(proc, "delegate"), 0,0,0,0);
+        uint64_t sceneContext = remote_msg(proc, sceneDelegate, remote_sel(proc, "_sbWindowSceneContext"), 0,0,0,0);
+        remote_msg(proc, sceneContext, remote_sel(proc, "setFloatingDockController:"), dockController, 0,0,0);
+        
+        uint64_t iconManager = remote_msg(proc, iconController, selIconMgr, 0,0,0,0);
+        uint64_t rfcVC = remote_msg(proc, iconManager, remote_sel(proc, "rootFolderController"), 0,0,0,0);
+        if (remote_msg(proc, rfcVC, selRespondsTo, selDockListView, 0,0,0)) {
+            uint64_t dlv = remote_msg(proc, rfcVC, selDockListView, 0,0,0,0);
+            if (dlv) {
+                // Also hide the stock dock background
+                uint64_t dockBG = remote_msg(proc, dlv, remote_sel(proc, "superview"), 0,0,0,0);
+                remote_msg(proc, dockBG, remote_sel(proc, "setHidden:"), 1, 0,0,0);
+            }
+        }
+        return YES;
+    }];
 }
 
 int enable_grid_app_switcher(RemoteCall *proc) {
  ```

  </details>
- 🟡 `lara/views/RemoteView.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -136,7 +136,7 @@ struct RemoteView: View {
                         return "enable_floating_dock() -> \(result)"
                     }
                 } label: {
-                    Text("Enable Floating Dock (Broken)")
+                    Text("Enable Floating Dock")
                 }
                 
                 Button {
  ```

  </details>


---

## References

### Commits (1)

- [ace3da5](https://github.com/letienndat/lara/commit/ace3da5e57504fd67743a615d1745349fef8ac44) — Squashed commit of the following:


---

## Timeline (Verbose)

- : 📝 Commit f8d4a95: Fix Floating Dock not auto hiding
- Apr 30, 2026: 🟣 @rooootdev merged
- Apr 30, 2026: 🔴 Closed by @rooootdev
- May 4, 2026: 🔗 Commit ace3da5 — Squashed commit of the following:


---

*Exported on 10/14/2026, 3:04:18 PM from [https://github.com/rooootdev/lara/pull/214](https://github.com/rooootdev/lara/pull/214)*
