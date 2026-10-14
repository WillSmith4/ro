# refactor: unify offsets for iPad M-Series

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#133](https://github.com/rooootdev/lara/pull/133)  
> **State:** 🟣 merged  
> **Branch:** `Kinue72:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 19, 2026  
> **Changes:** +27 / -56 across 5 files  
> **Created:** Apr 19, 2026  
> **Updated:** Apr 19, 2026  
> **Closed:** Apr 19, 2026  

---

## Description

*Opened by [@Kinue72](https://github.com/Kinue72)* *on Apr 19, 2026*


Tested on:
- iPad Pro M2 / 18.1
- iPhone 12 / 18.5
- iPhone SE 3 / 18.6.2


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`2ca441c`](https://github.com/rooootdev/lara/commit/2ca441c4a252f923760d60d3f4815996121c17b8) refactor: unify offsets for iPad M-Series — *Kinue72* · Apr 19, 2026


---

## Changed Files (5)

- 🟡 `lara/kexploit/offsets.h` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -105,6 +105,8 @@ extern uint64_t t1sz_boot;
 extern uint64_t VM_MIN_KERNEL_ADDRESS;
 extern uint64_t VM_MAX_KERNEL_ADDRESS;
 
+extern uint64_t pac_mask;
+
 void offsets_init(void);
 
 bool dlkerncache(void);
  ```

  </details>
- 🟡 `lara/kexploit/offsets.m` (modified) — +17 / -26
  <details><summary>Diff</summary>

  ```diff
  @@ -160,6 +160,8 @@
 uint64_t VM_MIN_KERNEL_ADDRESS = 0;
 uint64_t VM_MAX_KERNEL_ADDRESS = 0;
 
+uint64_t pac_mask = 0;
+
 bool gIsPACSupported = false;
 bool gIsA18Above = false;
 
@@ -239,8 +241,12 @@ void offsets_init(void) {
                          cpuFamily == CPUFAMILY_ARM_TAHITI ||   // A18 Pro
                          cpuFamily == CPUFAMILY_ARM_DONAN);  // M4
     
-    bool isM3       =   (cpuFamily == CPUFAMILY_ARM_IBIZA);  // M3
-    bool isM4       =   (cpuFamily == CPUFAMILY_ARM_DONAN);  // M4
+    bool isMSeriesIpad = is_ipad_device() && (
+                            cpuFamily == CPUFAMILY_ARM_FIRESTORM_ICESTORM || // M1
+                            cpuFamily == CPUFAMILY_ARM_BLIZZARD_AVALANCHE || // M2
+                            cpuFamily == CPUFAMILY_ARM_IBIZA ||  // M3
+                            cpuFamily == CPUFAMILY_ARM_DONAN); // M4
+    
     
     gIsPACSupported = is_pac_supported();
     
@@ -354,7 +360,7 @@ void offsets_init(void) {
             off_task_task_exc_guard = 0x5d4;
         }
         
-        if(isA16Above) {
+        if(isA16Above || isMSeriesIpad) {
             t1sz_boot = 0x11;
         }
         
@@ -388,6 +394,11 @@ void offsets_init(void) {
             off_thread_task_threads_next = 0x378;
             off_task_task_exc_guard = 0x59c;
         }
+
+        if(isMSeriesIpad) {
+            VM_MIN_KERNEL_ADDRESS = 0xFFFFFE0000000000;
+            VM_MAX_KERNEL_ADDRESS = 0xFFFFFE8FFFFFFFFF;
+        }
     }
     // iOS 17.1-17.3
     if (SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"17.1")) {
@@ -497,12 +508,7 @@ void offsets_init(void) {
             off_thread_guard_exc_info_code = 0x350;
             off_thread_ast = 0x3c4;
             off_thread_task_threads_next = 0x390;
-        }
-        
-        if(isM4) {
-            VM_MIN_KERNEL_ADDRESS = 0xFFFFFE0000000000;
-            VM_MAX_KERNEL_ADDRESS = 0xFFFFFE8FFFFFFFFF;
-        }
+        } 
     }
     
     // iOS 18.0.x
@@ -967,23 +973,8 @@ void offsets_init(void) {
         }
     }
     
-    // M1/M2 iPad: same cpuFamily as A14/A15 but use 47-bit kernel VA (T1SZ=0x11)
-    // A16+ already gets t1sz_boot=0x11 via isA16Above; M1/M2 are missed
-    bool isIPad = is_ipad_device();
-    if ((cpuFamily == CPUFAMILY_ARM_FIRESTORM_ICESTORM ||
-         cpuFamily == CPUFAMILY_ARM_BLIZZARD_AVALANCHE) && isIPad) {
-        t1sz_boot = 0x11;
-        VM_MIN_KERNEL_ADDRESS = 0xFFFFFE0000000000;
-        VM_MAX_KERNEL_ADDRESS = 0xFFFFFE8FFFFFFFFF;
-        printf("[offsets] M1/M2 iPad detected — overriding to t1sz=0x11, VM_MIN=0xFFFFFE0000000000\n");
-    }
-    // M3 has correct t1sz from isA16Above but VM range was not set
-    if (isM3 && VM_MIN_KERNEL_ADDRESS != 0xFFFFFE0000000000) {
-        VM_MIN_KERNEL_ADDRESS = 0xFFFFFE0000000000;
-        VM_MAX_KERNEL_ADDRESS = 0xFFFFFE8FFFFFFFFF;
-        printf("[offsets] M3 VM range corrected\n");
-    }
-
+    pac_mask = ~((1ULL << (64 - t1sz_boot)) - 1ULL);
+    
     printf("initialized offsets\n");
 }
 
  ```

  </details>
- 🟡 `lara/kexploit/pe/sbx.m` (modified) — +4 / -14
  <details><summary>Diff</summary>

  ```diff
  @@ -41,14 +41,6 @@ static void sbx_log(const char *fmt, ...) {
     NSLog(@"%s", buf);
 }
 
-// vfs.m uses isheappointer() covering both A-series (0xFFFFFF8...) and M-series (0xFFFFFE...) ranges.
-// The old K() only covered A-series — on M1/M2/M3 iPads all pointer checks failed silently.
-static inline bool sbx_iskptr(uint64_t x) {
-    if (x == 0) return false;
-    if (x >= 0xfffffe0000000000ULL && x <= 0xfffffeFFFFFFFFFFULL) return true;  // M-series iPad
-    if (x > 0xFFFFFF8000000000ULL) return true;                                  // A-series iPhone
-    return false;
-}
 #define KRW_LEN 0x20
 
 #define OFF_PROC_PROC_RO       0x18
@@ -60,7 +52,9 @@ static inline bool sbx_iskptr(uint64_t x) {
 #define OFF_EXT_DATA           0x40
 #define OFF_EXT_DATALEN        0x48
 
+extern uint64_t VM_MIN_KERNEL_ADDRESS;
 extern uint64_t t1sz_boot;
+extern uint64_t pac_mask;
 
 static bool ispac(void) {
     cpu_subtype_t cpusubtype = 0;
@@ -80,17 +74,13 @@ static inline uint64_t xpaci(uint64_t a) {
     return x0;
 }
 
-static inline uint64_t sbx_pac_mask(void) {
-    return ~((1ULL << (64 - (uint8_t)t1sz_boot)) - 1ULL);
-}
-
 static inline uint64_t signptr(uint64_t v) {
-    if ((v >> 32) > 0xFFFF) return v | sbx_pac_mask();
+    if ((v >> 32) > 0xFFFF) return v | pac_mask;
     return v;
 }
 
 #define S(x) ({ uint64_t _v = xpaci(x); signptr(_v); })
-#define K(x) sbx_iskptr(x)
+#define K(x) ((x) > VM_MIN_KERNEL_ADDRESS)
 
 static uint64_t smrdecode(uint64_t value, uint64_t base) {
     uint64_t bits = (base << (62 - (uint8_t)t1sz_boot));
  ```

  </details>
- 🟡 `lara/kexploit/pe/vfs.m` (modified) — +3 / -11
  <details><summary>Diff</summary>

  ```diff
  @@ -67,18 +67,11 @@
 #define FLAGS_PROT_MASK    0x780
 #define FLAGS_MAXPROT_MASK 0x7800
 
-extern uint64_t t1sz_boot;
+extern uint64_t pac_mask;
 
 #define BIT(b)    (1ULL << (b))
-#define ONES(x)   (BIT(x)-1)
 
-static inline uint64_t vfs_pac_mask(void) {
-    return ~((1ULL << (64 - (uint8_t)t1sz_boot)) - 1ULL);
-}
-static inline bool vfs_is_sign_bit_set(uint64_t p) {
-    // bit 55 is set for any canonical kernel address in all supported VA layouts
-    return (p & BIT(55)) != 0;
-}
+#define SIGN(p)          ((p) & BIT(55))
 
 #define ENTRY_SAFE_BASE   0x30
 #define FLAGS_OFF_IN_BUF  (O_ENTRY_FLAGS - ENTRY_SAFE_BASE)
@@ -128,8 +121,7 @@ static uint64_t resolve_vnode_once(uint64_t vn) {
 }
 
 static inline uint64_t pacstrip(uint64_t p) {
-    uint64_t pm = vfs_pac_mask();
-    return vfs_is_sign_bit_set(p) ? (p | pm) : (p & ~pm);
+    return SIGN(p) ? (p | pac_mask) : (p & ~pac_mask);
 }
 
 uint64_t kreadpointer(uint64_t addr) {
  ```

  </details>
- 🟡 `lara/kexploit/utils.m` (modified) — +1 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -129,12 +129,8 @@ static bool is_kptr(uint64_t p) {
     return (p & 0xffff000000000000ULL) == 0xffff000000000000ULL;
 }
 
-static inline uint64_t utils_pac_mask(void) {
-    return ~((1ULL << (64 - (uint8_t)t1sz_boot)) - 1ULL);
-}
-
 static inline uint64_t signptr(uint64_t v) {
-    if ((v >> 32) > 0xFFFF) return v | utils_pac_mask();
+    if ((v >> 32) > 0xFFFF) return v | pac_mask;
     return v;
 }
 
  ```

  </details>


---

## Timeline (Verbose)

- Apr 19, 2026: ⬆️ @Kinue72 force-pushed
- Apr 19, 2026: ⬆️ @Kinue72 force-pushed
- : 📝 Commit 2ca441c: refactor: unify offsets for iPad M-Series
- Apr 19, 2026: ⬆️ @Kinue72 force-pushed
- Apr 19, 2026: 🟣 @rooootdev merged
- Apr 19, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 2:57:09 PM from [https://github.com/rooootdev/lara/pull/133](https://github.com/rooootdev/lara/pull/133)*
