# Add OTA Updates and Screen Time disable/enable tweaks

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#280](https://github.com/rooootdev/lara/pull/280)  
> **State:** 🟣 merged  
> **Branch:** `A11pwnX:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Jun 1, 2026  
> **Changes:** +939 / -0 across 10 files  
> **Created:** May 29, 2026  
> **Updated:** Jun 1, 2026  
> **Closed:** Jun 1, 2026  

---

## Description

*Opened by [@A11pwnX](https://github.com/A11pwnX)* *on May 29, 2026*


For the OTA update blocker, I based my implementation on the following project:
https://github.com/zeroxjf/cyanide-ios/blob/main/Cyanide/tweaks/darksword_ota.m

The implementation used in Cyanide is itself based on a fork of:
https://github.com/kolbicz/DarkSword-Tweaks/tree/main

For the Screen Time bypass, I used the following project as a reference:
https://github.com/c22dev/Geranium/tree/main/Geranium/ByeTime

I tested both features on my iPad 10th generation running iOS 18.3.2, and both are working correctly.

This is my first pull request, so I apologize for any potential mistakes or issues.


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (12)

- [`1f0c85a`](https://github.com/rooootdev/lara/commit/1f0c85aff83d01702ec15bdcf9adc0ee3042165f) Add files via upload — *A11pwnX* · May 29, 2026
- [`6aef6d1`](https://github.com/rooootdev/lara/commit/6aef6d1ce585ff80bc823b3371d987ec183a4309) Add files via upload — *A11pwnX* · May 29, 2026
- [`173a357`](https://github.com/rooootdev/lara/commit/173a357c23b99b240c9e4d8820469fc5433eaca2) Add ota.h import to Bridging Header — *A11pwnX* · May 29, 2026
- [`a3fee93`](https://github.com/rooootdev/lara/commit/a3fee93712aa315a6a0cb0f70c87e113383b18e9) Fix NavigationLink for OTA Updates — *A11pwnX* · May 29, 2026
- [`02938c3`](https://github.com/rooootdev/lara/commit/02938c3a0bd8cab7ab6844d199a0603c1cee5c2f) Add ota.m and ota.h to project references — *A11pwnX* · May 29, 2026
- [`90e4445`](https://github.com/rooootdev/lara/commit/90e44450b7d51898fc9476d691101f1344f8e796) Update README.md — *A11pwnX* · May 29, 2026
- [`dc073b9`](https://github.com/rooootdev/lara/commit/dc073b951ea7c45b290e398e694b7ea8abaff5da) Add files via upload — *A11pwnX* · May 29, 2026
- [`cfcbd9e`](https://github.com/rooootdev/lara/commit/cfcbd9e1d1bca182518f637b8a2186c03c15f634) Add files via upload — *A11pwnX* · May 29, 2026
- [`587e778`](https://github.com/rooootdev/lara/commit/587e778f364e3ff9d4f10653e22d21b154369780) Add import for screentime.h in Bridging Header — *A11pwnX* · May 29, 2026
- [`de80b05`](https://github.com/rooootdev/lara/commit/de80b05cdd76c2fc3dce6d1e873b642cf116c66b) Add NavigationLink for Screen Time view — *A11pwnX* · May 29, 2026
- [`12a156f`](https://github.com/rooootdev/lara/commit/12a156f1c5ac2ba0b291ed5a3fb03f79485271c4) Add screentime.m and screentime.h to project — *A11pwnX* · May 29, 2026
- [`809078e`](https://github.com/rooootdev/lara/commit/809078ee8845daf743eb68666650824da2143520) Add Screen Time Disabler to features list — *A11pwnX* · May 29, 2026


---

## Changed Files (10)

- 🟡 `README.md` (modified) — +3 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -86,6 +86,9 @@ Important Notes:
 - Grid App Switcher
 - Performance HUD
 - JIT Enabler (only for apps with `get-task-allow`)
+- OTA Update Disabler
+- Screen Time Disabler 
+  
 
 ### Coming Soon
 - App Decrypt
  ```

  </details>
- 🟡 `lara.xcodeproj/project.pbxproj` (modified) — +8 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -76,6 +76,10 @@
 		8F5C1F9B2D6A4E9D9F4B1A21 /* lara/kexploit/darksword.m */ = {isa = PBXFileReference; lastKnownFileType = sourcecode.c.objc; path = lara/kexploit/darksword.m; sourceTree = "<group>"; };
 		9F4A1D2E3C4E567890123456 /* lara/lib/libxpf.dylib */ = {isa = PBXFileReference; lastKnownFileType = "compiled.mach-o.dylib"; path = lara/lib/libxpf.dylib; sourceTree = "<group>"; };
 		9F4A1D413C4E567890123456 /* lara/lib/libgrabkernel2.dylib */ = {isa = PBXFileReference; lastKnownFileType = "compiled.mach-o.dylib"; path = lara/lib/libgrabkernel2.dylib; sourceTree = "<group>"; };
+		A1B2C3D4E5F60001A1B2C3D4 /* lara/kexploit/ota.m */ = {isa = PBXFileReference; lastKnownFileType = sourcecode.c.objc; path = lara/kexploit/ota.m; sourceTree = "<group>"; };
+		A1B2C3D4E5F60002A1B2C3D4 /* lara/kexploit/ota.h */ = {isa = PBXFileReference; lastKnownFileType = sourcecode.c.h; path = lara/kexploit/ota.h; sourceTree = "<group>"; };
+		A1B2C3D4E5F60003A1B2C3D4 /* lara/kexploit/screentime.m */ = {isa = PBXFileReference; lastKnownFileType = sourcecode.c.objc; path = lara/kexploit/screentime.m; sourceTree = "<group>"; };
+		A1B2C3D4E5F60004A1B2C3D4 /* lara/kexploit/screentime.h */ = {isa = PBXFileReference; lastKnownFileType = sourcecode.c.h; path = lara/kexploit/screentime.h; sourceTree = "<group>"; };
 		CC1C8B392F71DF9C00206982 /* lara.app */ = {isa = PBXFileReference; explicitFileType = wrapper.application; includeInIndex = 0; path = lara.app; sourceTree = BUILT_PRODUCTS_DIR; };
 		CC6842DF2F8311C300B08EB0 /* libSystem.B.tbd */ = {isa = PBXFileReference; lastKnownFileType = "sourcecode.text-based-dylib-definition"; name = libSystem.B.tbd; path = usr/lib/libSystem.B.tbd; sourceTree = SDKROOT; };
 /* End PBXFileReference section */
@@ -232,6 +236,10 @@
 			isa = PBXGroup;
 			children = (
 				8F5C1F9B2D6A4E9D9F4B1A21 /* lara/kexploit/darksword.m */,
+				A1B2C3D4E5F60001A1B2C3D4 /* lara/kexploit/ota.m */,
+				A1B2C3D4E5F60002A1B2C3D4 /* lara/kexploit/ota.h */,
+				A1B2C3D4E5F60003A1B2C3D4 /* lara/kexploit/screentime.m */,
+				A1B2C3D4E5F60004A1B2C3D4 /* lara/kexploit/screentime.h */,
 				9F4A1D2E3C4E567890123456 /* lara/lib/libxpf.dylib */,
 				9F4A1D413C4E567890123456 /* lara/lib/libgrabkernel2.dylib */,
 			);
  ```

  </details>
- 🟢 `lara/kexploit/ota.h` (added) — +8 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,8 @@
+#ifndef ota_h
+#define ota_h
+
+#include <stdbool.h>
+
+bool ota_set_disabled(bool disabled);
+
+#endif
  ```

  </details>
- 🟢 `lara/kexploit/ota.m` (added) — +376 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,376 @@
+#import "ota.h"
+#import "darksword.h"
+#import "pe/apfs.h"
+#import "TaskRop/RemoteCall.h"
+
+#import <Foundation/Foundation.h>
+#import <dlfcn.h>
+#import <errno.h>
+#import <fcntl.h>
+#import <mach-o/dyld.h>
+#import <mach-o/getsect.h>
+#import <notify.h>
+#import <stdint.h>
+#import <stdio.h>
+#import <stdlib.h>
+#import <string.h>
+#import <sys/mman.h>
+#import <sys/stat.h>
+#import <unistd.h>
+
+static const char *kOTAPlistAbsPath = "/private/var/db/com.apple.xpc.launchd/disabled.plist";
+static NSString * const kOTAMobileGestaltPath =
+    @"/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist";
+static const uint64_t kOTABufSize = 65536;
+
+static NSArray<NSString *> *ota_daemon_labels(void) {
+    return @[
+        @"com.apple.mobile.softwareupdated",
+        @"com.apple.OTATaskingAgent",
+        @"com.apple.softwareupdateservicesd",
+        @"com.apple.mobile.NRDUpdated",
+    ];
+}
+
+static NSArray<NSString *> *ota_catalog_pref_paths(void) {
+    return @[
+        @"/var/mobile/Library/Preferences/com.apple.softwareupdateservicesd.plist",
+        @"/var/mobile/Library/Preferences/com.apple.MobileSoftwareUpdate.plist",
+    ];
+}
+
+static bool ota_write_catalog_pref(NSString *path, NSDictionary *plist) {
+    NSData *outData = [NSPropertyListSerialization dataWithPropertyList:plist
+                                                                 format:NSPropertyListXMLFormat_v1_0
+                                                                options:0
+                                                                  error:nil];
+    if (outData.length == 0) return false;
+    BOOL ok = [outData writeToFile:path atomically:YES];
+    if (ok) chmod(path.UTF8String, 0644);
+    return ok;
+}
+
+static bool ota_ensure_catalog_preferences(void) {
+    bool ok = true;
+    bool changed = false;
+    for (NSString *path in ota_catalog_pref_paths()) {
+        NSData *data = [NSData dataWithContentsOfFile:path];
+        NSMutableDictionary *plist = nil;
+        if (data.length > 0) {
+            plist = [[NSPropertyListSerialization
+                propertyListWithData:data
+                             options:NSPropertyListMutableContainersAndLeaves
+                              format:nil
+                               error:nil] mutableCopy];
+        }
+        if (![plist isKindOfClass:NSMutableDictionary.class]) {
+            plist = [NSMutableDictionary dictionary];
+        }
+        if ([plist[@"SUQueryCustomerBuilds"] isEqual:@YES]) continue;
+        plist[@"SUQueryCustomerBuilds"] = @YES;
+        if (!ota_write_catalog_pref(path, plist)) {
+            ok = false;
+        } else {
+            changed = true;
+        }
+    }
+    if (changed) notify_post("SUPreferencesChangedNotification");
+    return ok;
+}
+
+static long ota_find_mobilegestalt_cachedata_offset(const char *mgKey) {
+    const char *mgName = "/usr/lib/libMobileGestalt.dylib";
+    const struct mach_header_64 *header = NULL;
+
+    dlopen(mgName, RTLD_LAZY | RTLD_GLOBAL);
+    for (uint32_t i = 0; i < _dyld_image_count(); i++) {
+        const char *imageName = _dyld_get_image_name(i);
+        if (imageName && strncmp(mgName, imageName, strlen(mgName)) == 0) {
+            header = (const struct mach_header_64 *)_dyld_get_image_header(i);
+            break;
+        }
+    }
+    if (!header) return -1;
+
+    unsigned long cstringSize = 0;
+    const char *cstringSection = (const char *)getsectiondata(header, "__TEXT", "__cstring", &cstringSize);
+    if (!cstringSection) return -1;
+
+    const char *keyPtr = NULL;
+    for (unsigned long off = 0; off < cstringSize; ) {
+        const char *s = cstringSection + off;
+        size_t len = strnlen(s, cstringSize - off);
+        if (strcmp(s, mgKey) == 0) { keyPtr = s; break; }
+        off += len + 1;
+        if (len == 0 && off >= cstringSize) break;
+    }
+    if (!keyPtr) return -1;
+
+    unsigned long constSize = 0;
+    const uintptr_t *constSection = (const uintptr_t *)getsectiondata(header, "__AUTH_CONST", "__const", &constSize);
+    if (!constSection) {
+        constSection = (const uintptr_t *)getsectiondata(header, "__DATA_CONST", "__const", &constSize);
+    }
+    if (!constSection) return -1;
+
+    for (unsigned long i = 0; i < constSize / sizeof(uintptr_t); i++) {
+        if (constSection[i] == (uintptr_t)keyPtr) {
+            const uint16_t *entry = (const uint16_t *)&constSection[i];
+            return ((long)entry[0x9a / 2]) << 3;
+        }
+    }
+    return -1;
+}
+
+static bool ota_zero_mobilegestalt_cachedata_key(NSMutableData *cacheData, const char *key) {
+    long offset = ota_find_mobilegestalt_cachedata_offset(key);
+    if (offset < 0) return false;
+    if ((NSUInteger)offset + sizeof(uint64_t) > cacheData.length) return false;
+
+    uint64_t oldValue = 0;
+    memcpy(&oldValue, (const uint8_t *)cacheData.bytes + offset, sizeof(oldValue));
+    if (oldValue == 0) return false;
+
+    uint64_t zero = 0;
+    memcpy((uint8_t *)cacheData.mutableBytes + offset, &zero, sizeof(zero));
+    printf("[OTA][MG] zeroed CacheData key=%s offset=%ld old=0x%llx\n",
+           key, offset, (unsigned long long)oldValue);
+    return true;
+}
+
+static bool ota_clear_mobilegestalt_internal_flags(void) {
+    NSData *data = [NSData dataWithContentsOfFile:kOTAMobileGestaltPath options:0 error:nil];
+    if (data.length == 0) {
+        printf("[OTA][MG] unable to read MobileGestalt plist\n");
+        return false;
+    }
+
+    NSMutableDictionary *mg = [[NSPropertyListSerialization
+        propertyListWithData:data
+                     options:NSPropertyListMutableContainersAndLeaves
+                      format:nil
+                       error:nil] mutableCopy];
+    if (![mg isKindOfClass:NSMutableDictionary.class]) {
+        printf("[OTA][MG] MobileGestalt plist parse failed\n");
+        return false;
+    }
+
+    NSMutableDictionary *cacheExtra = mg[@"CacheExtra"];
+    NSMutableData *cacheData = mg[@"CacheData"];
+    if (![cacheExtra isKindOfClass:NSMutableDictionary.class] ||
+        ![cacheData isKindOfClass:NSMutableData.class]) {
+        printf("[OTA][MG] missing CacheExtra/CacheData\n");
+        return false;
+    }
+
+    bool changed = false;
+    NSArray<NSString *> *internalKeys = @[
+        @"LBJfwOEzExRxzlAnSuI7eg",
+        @"EqrsVvjcYDdxHBiQmGhAWw",
+        @"Oji6HRoPi7rH7HPdWVakuw",
+    ];
+
+    for (NSString *key in internalKeys) {
+        if (cacheExtra[key]) {
+            [cacheExtra removeObjectForKey:key];
+            changed = true;
+        }
+        if (ota_zero_mobilegestalt_cachedata_key(cacheData, key.UTF8String)) {
+            changed = true;
+        }
+    }
+
+    if (!changed) {
+        printf("[OTA][MG] internal flags already clear\n");
+        return true;
+    }
+
+    NSData *outData = [NSPropertyListSerialization dataWithPropertyList:mg
+                                                                 format:NSPropertyListBinaryFormat_v1_0
+                                                                options:0
+                                                                  error:nil];
+    if (outData.length == 0) {
+        printf("[OTA][MG] serialization failed\n");
+        return false;
+    }
+
+    BOOL ok = [outData writeToFile:kOTAMobileGestaltPath atomically:YES];
+    if (ok) chmod(kOTAMobileGestaltPath.UTF8String, 0644);
+    printf("[OTA][MG] internal flag cleanup %s bytes=%lu\n",
+           ok ? "ok" : "failed", (unsigned long)outData.length);
+    return ok;
+}
+
+static bool ota_run_enable_cleanup(void) {
+    bool ok = ota_ensure_catalog_preferences();
+    ok = ota_clear_mobilegestalt_internal_flags() && ok;
+    return ok;
+}
+
+static RemoteCall *ota_create_launchd_rc(void) {
+    RemoteCall *proc = [[RemoteCall alloc] initWithProcess:@"launchd" useMigFilterBypass:NO];
+    if (!proc) {
+        printf("[OTA] failed to create RemoteCall for launchd\n");
+    }
+    return proc;
+}
+
+static NSMutableDictionary *ota_rc_read_plist(RemoteCall *proc, uint64_t fileBuf) {
+    uint64_t scratch = proc.trojanMem;
+    if (!scratch) return [NSMutableDictionary dictionary];
+
+    [proc remote_write:scratch string:kOTAPlistAbsPath];
+    uint64_t fd = RemoteArbCall(proc, open, scratch, (uint64_t)O_RDONLY, 0);
+    if ((int64_t)fd < 0) {
+        printf("[OTA] launchd open disabled.plist failed — will create fresh\n");
+        return [NSMutableDictionary dictionary];
+    }
+
+    uint64_t bytesRead = RemoteArbCall(proc, read, fd, fileBuf, kOTABufSize);
+    RemoteArbCall(proc, close, fd);
+
+    if ((int64_t)bytesRead <= 0 || bytesRead > kOTABufSize) {
+        printf("[OTA] launchd read returned %lld bytes\n", (int64_t)bytesRead);
+        return [NSMutableDictionary dictionary];
+    }
+
+    uint8_t *buf = malloc((size_t)bytesRead);
+    if (!buf) return [NSMutableDictionary dictionary];
+
+    NSMutableDictionary *result = [NSMutableDictionary dictionary];
+    if ([proc remoteRead:fileBuf to:buf size:bytesRead]) {
+        NSData *data = [NSData dataWithBytes:buf length:(NSUInteger)bytesRead];
+        NSMutableDictionary *parsed = [[NSPropertyListSerialization
+            propertyListWithData:data
+                         options:NSPropertyListMutableContainersAndLeaves
+                          format:nil
+                           error:nil] mutableCopy];
+        if ([parsed isKindOfClass:NSMutableDictionary.class]) {
+            result = parsed;
+        }
+    }
+    free(buf);
+    return result;
+}
+
+static bool ota_rc_write_plist(RemoteCall *proc,
+                                uint64_t fileBuf,
+                                NSDictionary *plist) {
+    NSData *outData = [NSPropertyListSerialization dataWithPropertyList:plist
+                                                                 format:NSPropertyListXMLFormat_v1_0
+                                                                options:0
+                                                                  error:nil];
+    if (!outData || outData.length == 0 || outData.length > kOTABufSize) {
+        printf("[OTA] plist serialization failed\n");
+        return false;
+    }
+
+    if (![proc remote_write:fileBuf from:outData.bytes size:outData.length]) {
+        printf("[OTA] failed to copy plist into launchd buffer\n");
+        return false;
+    }
+
+    uint64_t scratch = proc.trojanMem;
+    [proc remote_write:scratch string:kOTAPlistAbsPath];
+
+    uint64_t wfd = RemoteArbCall(proc, open,
+                                 scratch,
+                                 (uint64_t)(O_WRONLY | O_CREAT | O_TRUNC),
+                                 (uint64_t)0644);
+    if ((int64_t)wfd < 0) {
+        printf("[OTA] launchd open for write failed\n");
+        return false;
+    }
+
+    uint64_t totalWritten = 0;
+    uint64_t remaining = outData.length;
+    while (remaining > 0) {
+        uint64_t n = RemoteArbCall(proc, write, wfd, fileBuf + totalWritten, remaining);
+        if ((int64_t)n <= 0) break;
+        totalWritten += n;
+        remaining -= n;
+    }
+    RemoteArbCall(proc, close, wfd);
+
+    if (remaining != 0) {
+        printf("[OTA] launchd write incomplete: %llu/%lu bytes\n",
+               totalWritten, (unsigned long)outData.length);
+        return false;
+    }
+
+    if (ds_is_ready()) {
+        apfs_own(kOTAPlistAbsPath, 0, 0);
+        apfs_mod(kOTAPlistAbsPath, S_IFREG | 0644);
+    }
+
+    printf("[OTA] disabled.plist written %llu bytes via launchd\n", totalWritten);
+    return true;
+}
+
+static bool ota_apply_via_launchd_rc(bool disabled) {
+    printf("[OTA] === %s OTA via launchd RemoteCall ===\n",
+           disabled ? "DISABLING" : "ENABLING");
+
+    RemoteCall *proc = ota_create_launchd_rc();
+    if (!proc) return false;
+
+    bool ok = false;
+
+    uint64_t fileBuf = RemoteArbCall(proc, mmap,
+                                     0,
+                                     kOTABufSize,
+                                     VM_PROT_READ | VM_PROT_WRITE,
+                                     MAP_PRIVATE | MAP_ANON,
+                                     (uint64_t)-1,
+                                     0);
+    if (!fileBuf) {
+        printf("[OTA] mmap in launchd failed\n");
+        [proc destroyRemoteCall];
+        return false;
+    }
+
+    NSMutableDictionary *plist = ota_rc_read_plist(proc, fileBuf);
+
+    int changed = 0;
+    for (NSString *label in ota_daemon_labels()) {
+        if (disabled) {
+            if (![plist[label] boolValue]) {
+                plist[label] = @YES;
+                changed++;
+                printf("[OTA] disabling %s\n", label.UTF8String);
+            } else {
+                printf("[OTA] already disabled %s\n", label.UTF8String);
+            }
+        } else {
+            if (plist[label]) {
+                [plist removeObjectForKey:label];
+                changed++;
+                printf("[OTA] enabling %s\n", label.UTF8String);
+            } else {
+                printf("[OTA] already enabled %s\n", label.UTF8String);
+            }
+        }
+    }
+
+    if (changed == 0) {
+        printf("[OTA] no changes needed\n");
+        ok = true;
+    } else {
+        ok = ota_rc_write_plist(proc, fileBuf, plist);
+    }
+
+    RemoteArbCall(proc, munmap, fileBuf, kOTABufSize);
+    [proc destroyRemoteCall];
+
+    if (!disabled && ok) {
+        ok = ota_run_enable_cleanup() && ok;
+    }
+
+    printf("[OTA] === %s OTA result=%d reboot required ===\n",
+           disabled ? "DISABLE" : "ENABLE", ok);
+    return ok;
+}
+
+bool ota_set_disabled(bool disabled) {
+    return ota_apply_via_launchd_rc(disabled);
+}
  ```

  </details>
- 🟢 `lara/kexploit/screentime.h` (added) — +13 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,13 @@
+#ifndef screentime_h
+#define screentime_h
+
+#include <stdbool.h>
+
+bool screentime_disable(bool killScreenTimeAgent,
+                        bool killUsageTrackingAgent,
+                        bool killHomed,
+                        bool killFamilycircled);
+
+bool screentime_enable(void);
+
+#endif
  ```

  </details>
- 🟢 `lara/kexploit/screentime.m` (added) — +308 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,308 @@
+#import "screentime.h"
+#import "darksword.h"
+#import "pe/apfs.h"
+#import "TaskRop/RemoteCall.h"
+
+#import <Foundation/Foundation.h>
+#import <errno.h>
+#import <fcntl.h>
+#import <signal.h>
+#import <stdint.h>
+#import <stdio.h>
+#import <stdlib.h>
+#import <string.h>
+#import <sys/mman.h>
+#import <sys/stat.h>
+#import <sys/sysctl.h>
+#import <unistd.h>
+
+extern int proc_name(int pid, void *buffer, uint32_t buffersize);
+
+static void st_killproc(const char *name) {
+    int mib[3] = { CTL_KERN, KERN_PROC, KERN_PROC_ALL };
+    size_t size = 0;
+    if (sysctl(mib, 3, NULL, &size, NULL, 0) != 0) return;
+
+    struct kinfo_proc *procs = malloc(size);
+    if (!procs) return;
+
+    if (sysctl(mib, 3, procs, &size, NULL, 0) != 0) {
+        free(procs);
+        return;
+    }
+
+    int count = (int)(size / sizeof(struct kinfo_proc));
+    for (int i = 0; i < count; i++) {
+        char pname[64] = {0};
+        proc_name(procs[i].kp_proc.p_pid, pname, sizeof(pname));
+        if (strcasecmp(pname, name) == 0) {
+            printf("[ST] killing %s pid=%d\n", name, procs[i].kp_proc.p_pid);
+            kill(procs[i].kp_proc.p_pid, SIGKILL);
+        }
+    }
+    free(procs);
+}
+
+static const char *kSTPlistAbsPath   = "/private/var/db/com.apple.xpc.launchd/disabled.plist";
+static const char *kSTPrefsPath      = "/var/mobile/Library/Preferences/com.apple.ScreenTimeAgent.plist";
+static const char *kSTBackupPath     = "/var/mobile/Library/Preferences/com.apple.ScreenTimeAgent.plist.bak";
+static const uint64_t kSTBufSize     = 65536;
+
+static const char *kST_ScreenTimeAgent     = "com.apple.ScreenTimeAgent";
+static const char *kST_UsageTrackingAgent  = "com.apple.UsageTrackingAgent";
+static const char *kST_Homed               = "com.apple.homed";
+static const char *kST_Familycircled       = "com.apple.familycircled";
+
+static RemoteCall *st_create_launchd_rc(void) {
+    RemoteCall *proc = [[RemoteCall alloc] initWithProcess:@"launchd" useMigFilterBypass:NO];
+    if (!proc) {
+        printf("[ST] failed to create RemoteCall for launchd\n");
+    }
+    return proc;
+}
+
+static NSMutableDictionary *st_rc_read_disabled_plist(RemoteCall *proc, uint64_t fileBuf) {
+    uint64_t scratch = proc.trojanMem;
+    if (!scratch) return [NSMutableDictionary dictionary];
+
+    [proc remote_write:scratch string:kSTPlistAbsPath];
+    uint64_t fd = RemoteArbCall(proc, open, scratch, (uint64_t)O_RDONLY, 0);
+    if ((int64_t)fd < 0) {
+        printf("[ST] launchd open disabled.plist failed — will create fresh\n");
+        return [NSMutableDictionary dictionary];
+    }
+
+    uint64_t bytesRead = RemoteArbCall(proc, read, fd, fileBuf, kSTBufSize);
+    RemoteArbCall(proc, close, fd);
+
+    if ((int64_t)bytesRead <= 0 || bytesRead > kSTBufSize) {
+        printf("[ST] launchd read returned %lld bytes\n", (int64_t)bytesRead);
+        return [NSMutableDictionary dictionary];
+    }
+
+    uint8_t *buf = malloc((size_t)bytesRead);
+    if (!buf) return [NSMutableDictionary dictionary];
+
+    NSMutableDictionary *result = [NSMutableDictionary dictionary];
+    if ([proc remoteRead:fileBuf to:buf size:bytesRead]) {
+        NSData *data = [NSData dataWithBytes:buf length:(NSUInteger)bytesRead];
+        NSMutableDictionary *parsed = [[NSPropertyListSerialization
+            propertyListWithData:data
+                         options:NSPropertyListMutableContainersAndLeaves
+                          format:nil
+                           error:nil] mutableCopy];
+        if ([parsed isKindOfClass:NSMutableDictionary.class]) {
+            result = parsed;
+        }
+    }
+    free(buf);
+    return result;
+}
+
+static bool st_rc_write_disabled_plist(RemoteCall *proc,
+                                        uint64_t fileBuf,
+                                        NSDictionary *plist) {
+    NSData *outData = [NSPropertyListSerialization dataWithPropertyList:plist
+                                                                 format:NSPropertyListXMLFormat_v1_0
+                                                                options:0
+                                                                  error:nil];
+    if (!outData || outData.length == 0 || outData.length > kSTBufSize) {
+        printf("[ST] disabled.plist serialization failed\n");
+        return false;
+    }
+
+    if (![proc remote_write:fileBuf from:outData.bytes size:outData.length]) {
+        printf("[ST] failed to copy plist into launchd buffer\n");
+        return false;
+    }
+
+    uint64_t scratch = proc.trojanMem;
+    [proc remote_write:scratch string:kSTPlistAbsPath];
+
+    uint64_t wfd = RemoteArbCall(proc, open,
+                                 scratch,
+                                 (uint64_t)(O_WRONLY | O_CREAT | O_TRUNC),
+                                 (uint64_t)0644);
+    if ((int64_t)wfd < 0) {
+        printf("[ST] launchd open for write failed\n");
+        return false;
+    }
+
+    uint64_t totalWritten = 0;
+    uint64_t remaining = outData.length;
+    while (remaining > 0) {
+        uint64_t n = RemoteArbCall(proc, write, wfd, fileBuf + totalWritten, remaining);
+        if ((int64_t)n <= 0) break;
+        totalWritten += n;
+        remaining -= n;
+    }
+    RemoteArbCall(proc, close, wfd);
+
+    if (remaining != 0) {
+        printf("[ST] launchd write incomplete: %llu/%lu bytes\n",
+               totalWritten, (unsigned long)outData.length);
+        return false;
+    }
+
+    if (ds_is_ready()) {
+        apfs_own(kSTPlistAbsPath, 0, 0);
+        apfs_mod(kSTPlistAbsPath, S_IFREG | 0644);
+    }
+
+    printf("[ST] disabled.plist written %llu bytes via launchd\n", totalWritten);
+    return true;
+}
+
+static bool st_patch_disabled_plist(bool killScreenTimeAgent,
+                                     bool killUsageTrackingAgent,
+                                     bool killHomed,
+                                     bool killFamilycircled,
+                                     bool disable) {
+    RemoteCall *proc = st_create_launchd_rc();
+    if (!proc) return false;
+
+    bool ok = false;
+
+    uint64_t fileBuf = RemoteArbCall(proc, mmap,
+                                     0,
+                                     kSTBufSize,
+                                     VM_PROT_READ | VM_PROT_WRITE,
+                                     MAP_PRIVATE | MAP_ANON,
+                                     (uint64_t)-1,
+                                     0);
+    if (!fileBuf) {
+        printf("[ST] mmap in launchd failed\n");
+        [proc destroyRemoteCall];
+        return false;
+    }
+
+    NSMutableDictionary *plist = st_rc_read_disabled_plist(proc, fileBuf);
+
+    struct { const char *key; bool active; } entries[] = {
+        { kST_ScreenTimeAgent,    killScreenTimeAgent    },
+        { kST_UsageTrackingAgent, killUsageTrackingAgent },
+        { kST_Homed,              killHomed              },
+        { kST_Familycircled,      killFamilycircled      },
+    };
+
+    int changed = 0;
+    for (size_t i = 0; i < sizeof(entries) / sizeof(entries[0]); i++) {
+        if (!entries[i].active) continue;
+        NSString *key = @(entries[i].key);
+        if (disable) {
+            if (![plist[key] boolValue]) {
+                plist[key] = @YES;
+                changed++;
+                printf("[ST] disabling daemon %s\n", entries[i].key);
+            } else {
+                printf("[ST] daemon already disabled %s\n", entries[i].key);
+            }
+        } else {
+            if (plist[key]) {
+                [plist removeObjectForKey:key];
+                changed++;
+                printf("[ST] enabling daemon %s\n", entries[i].key);
+            } else {
+                printf("[ST] daemon already enabled %s\n", entries[i].key);
+            }
+        }
+    }
+
+    if (changed == 0) {
+        printf("[ST] no plist changes needed\n");
+        ok = true;
+    } else {
+        ok = st_rc_write_disabled_plist(proc, fileBuf, plist);
+    }
+
+    RemoteArbCall(proc, munmap, fileBuf, kSTBufSize);
+    [proc destroyRemoteCall];
+    return ok;
+}
+
+static void st_backup_prefs(void) {
+    if (access(kSTBackupPath, F_OK) == 0) {
+        printf("[ST] backup already exists at %s\n", kSTBackupPath);
+        return;
+    }
+    if (access(kSTPrefsPath, F_OK) != 0) {
+        printf("[ST] no ScreenTimeAgent prefs to back up\n");
+        return;
+    }
+    NSError *err = nil;
+    [[NSFileManager defaultManager]
+        copyItemAtPath:@(kSTPrefsPath)
+                toPath:@(kSTBackupPath)
+                 error:&err];
+    if (err) {
+        printf("[ST] backup failed: %s\n", err.localizedDescription.UTF8String);
+    } else {
+        printf("[ST] prefs backed up to %s\n", kSTBackupPath);
+    }
+}
+
+static void st_remove_prefs(void) {
+    if (access(kSTPrefsPath, F_OK) != 0) return;
+    NSError *err = nil;
+    [[NSFileManager defaultManager] removeItemAtPath:@(kSTPrefsPath) error:&err];
+    if (err) {
+        printf("[ST] failed to remove prefs: %s\n", err.localizedDescription.UTF8String);
+    } else {
+        printf("[ST] removed ScreenTimeAgent prefs\n");
+    }
+}
+
+bool screentime_disable(bool killScreenTimeAgent,
+                        bool killUsageTrackingAgent,
+                        bool killHomed,
+                        bool killFamilycircled) {
+    printf("[ST] === DISABLING SCREEN TIME ===\n");
+
+    st_backup_prefs();
+    st_remove_prefs();
+
+    if (killScreenTimeAgent)    st_killproc("ScreenTimeAgent");
+    if (killUsageTrackingAgent) st_killproc("UsageTrackingAgent");
+    if (killHomed)              st_killproc("homed");
+    if (killFamilycircled)      st_killproc("familycircled");
+
+    st_remove_prefs();
+
+    bool ok = st_patch_disabled_plist(killScreenTimeAgent,
+                                      killUsageTrackingAgent,
+                                      killHomed,
+                                      killFamilycircled,
+                                      true);
+
+    printf("[ST] === DISABLE SCREEN TIME result=%d reboot required ===\n", ok);
+    return ok;
+}
+
+bool screentime_enable(void) {
+    printf("[ST] === ENABLING SCREEN TIME ===\n");
+
+    if (access(kSTBackupPath, F_OK) == 0) {
+        st_remove_prefs();
+        NSError *err = nil;
+        [[NSFileManager defaultManager]
+            copyItemAtPath:@(kSTBackupPath)
+                    toPath:@(kSTPrefsPath)
+                     error:&err];
+        if (err) {
+            printf("[ST] failed to restore backup: %s\n", err.localizedDescription.UTF8String);
+        } else {
+            printf("[ST] prefs restored from backup\n");
+            if (ds_is_ready()) {
+                apfs_own(kSTPrefsPath, 501, 501);
+                apfs_mod(kSTPrefsPath, S_IFREG | 0644);
+            }
+        }
+    } else {
+        printf("[ST] no backup found — skipping prefs restore\n");
+    }
+
+    bool ok = st_patch_disabled_plist(true, true, true, true, false);
+
+    printf("[ST] === ENABLE SCREEN TIME result=%d reboot required ===\n", ok);
+    return ok;
+}
  ```

  </details>
- 🟡 `lara/lara-Bridging-Header.h` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -18,6 +18,8 @@
 #import "RemoteCall.h"
 #import "decrypt.h"
 #import "persistence.h"
+#import "ota.h"
+#import "screentime.h"
 
 #import <zlib.h>
 
  ```

  </details>
- 🟢 `lara/views/tweaks/OTAView.swift` (added) — +86 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,86 @@
+import SwiftUI
+
+struct OTAView: View {
+    @ObservedObject var mgr: laramgr
+    @AppStorage("lara.ota.disabled") private var otaDisabled: Bool = false
+    @State private var isWorking: Bool = false
+    @State private var lastResult: String? = nil
+
+    var body: some View {
+        List {
+            Section(header: HeaderLabel(text: "Status", icon: "antenna.radiowaves.left.and.right")) {
+                HStack {
+                    Text("OTA Updates")
+                    Spacer()
+                    Text(otaDisabled ? "Disabled" : "Enabled")
+                        .foregroundColor(otaDisabled ? .red : .green)
+                        .monospaced()
+                }
+            }
+
+            Section(
+                header: HeaderLabel(text: "Actions", icon: "wrench.and.screwdriver"),
+                footer: Text("Modifies launchd's disabled.plist via RemoteCall to prevent OTA update daemons from running. A reboot is required for changes to take effect.")
+            ) {
+                Button {
+                    apply(disabled: true)
+                } label: {
+                    if isWorking && !otaDisabled {
+                        HStack {
+                            Text("Disabling…")
+                            Spacer()
+                            ProgressView()
+                        }
+                    } else {
+                        Text("Disable OTA Updates")
+                    }
+                }
+                .disabled(isWorking || otaDisabled)
+
+                Button {
+                    apply(disabled: false)
+                } label: {
+                    if isWorking && otaDisabled {
+                        HStack {
+                            Text("Enabling…")
+                            Spacer()
+                            ProgressView()
+                        }
+                    } else {
+                        Text("Enable OTA Updates")
+                    }
+                }
+                .disabled(isWorking || !otaDisabled)
+
+                Button("Respring") {
+                    mgr.respring()
+                }
+                .disabled(isWorking)
+            }
+        }
+        .navigationTitle("OTA Updates")
+        .alert("Result", isPresented: .constant(lastResult != nil)) {
+            Button("OK") { lastResult = nil }
+        } message: {
+            Text(lastResult ?? "")
+        }
+    }
+
+    private func apply(disabled: Bool) {
+        isWorking = true
+        DispatchQueue.global(qos: .userInitiated).async {
+            let ok = ota_set_disabled(disabled)
+            DispatchQueue.main.async {
+                isWorking = false
+                if ok {
+                    otaDisabled = disabled
+                    lastResult = disabled
+                        ? "OTA updates disabled. Reboot to apply."
+                        : "OTA updates enabled. Reboot to apply."
+                } else {
+                    lastResult = "Operation failed. Check logs for details."
+                }
+            }
+        }
+    }
+}
  ```

  </details>
- 🟢 `lara/views/tweaks/ScreenTimeView.swift` (added) — +133 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,133 @@
+import SwiftUI
+
+struct ScreenTimeView: View {
+    @ObservedObject var mgr: laramgr
+    @AppStorage("lara.screentime.disabled") private var screenTimeDisabled: Bool = false
+    @State private var killScreenTimeAgent: Bool = true
+    @State private var killUsageTrackingAgent: Bool = true
+    @State private var killHomed: Bool = false
+    @State private var killFamilycircled: Bool = false
+    @State private var isWorking: Bool = false
+    @State private var lastResult: String? = nil
+
+    private var backupExists: Bool {
+        FileManager.default.fileExists(atPath: "/var/mobile/Library/Preferences/com.apple.ScreenTimeAgent.plist.bak")
+    }
+
+    var body: some View {
+        List {
+            Section(header: HeaderLabel(text: "Status", icon: "hourglass")) {
+                HStack {
+                    Text("Screen Time")
+                    Spacer()
+                    Text(screenTimeDisabled ? "Disabled" : "Enabled")
+                        .foregroundColor(screenTimeDisabled ? .red : .green)
+                        .monospaced()
+                }
+                HStack {
+                    Text("Preferences backup")
+                    Spacer()
+                    Text(backupExists ? "Found" : "Not found")
+                        .foregroundColor(backupExists ? .green : .secondary)
+                        .monospaced()
+                }
+            }
+
+            Section(
+                header: HeaderLabel(text: "Daemons", icon: "gearshape.2"),
+                footer: Text("Select which daemons to disable. ScreenTimeAgent and UsageTrackingAgent are the minimum required to fully disable Screen Time.")
+            ) {
+                Toggle("ScreenTimeAgent", isOn: $killScreenTimeAgent)
+                    .disabled(isWorking || screenTimeDisabled)
+                Toggle("UsageTrackingAgent", isOn: $killUsageTrackingAgent)
+                    .disabled(isWorking || screenTimeDisabled)
+                Toggle("Homed", isOn: $killHomed)
+                    .disabled(isWorking || screenTimeDisabled)
+                Toggle("Familycircled", isOn: $killFamilycircled)
+                    .disabled(isWorking || screenTimeDisabled)
+            }
+
+            Section(
+                header: HeaderLabel(text: "Actions", icon: "wrench.and.screwdriver"),
+                footer: Text("Kills selected daemons, removes Screen Time preferences, and marks them as disabled in launchd's disabled.plist. A reboot is required for changes to take effect.")
+            ) {
+                Button {
+                    applyDisable()
+                } label: {
+                    if isWorking && !screenTimeDisabled {
+                        HStack {
+                            Text("Disabling…")
+                            Spacer()
+                            ProgressView()
+                        }
+                    } else {
+                        Text("Disable Screen Time")
+                    }
+                }
+                .disabled(isWorking || screenTimeDisabled)
+
+                Button {
+                    applyEnable()
+                } label: {
+                    if isWorking && screenTimeDisabled {
+                        HStack {
+                            Text("Enabling…")
+                            Spacer()
+                            ProgressView()
+                        }
+                    } else {
+                        Text("Enable Screen Time")
+                    }
+                }
+                .disabled(isWorking || !screenTimeDisabled)
+
+                Button("Respring") {
+                    mgr.respring()
+                }
+                .disabled(isWorking)
+            }
+        }
+        .navigationTitle("Screen Time")
+        .alert("Result", isPresented: .constant(lastResult != nil)) {
+            Button("OK") { lastResult = nil }
+        } message: {
+            Text(lastResult ?? "")
+        }
+    }
+
+    private func applyDisable() {
+        isWorking = true
+        let agent = killScreenTimeAgent
+        let usage = killUsageTrackingAgent
+        let homed = killHomed
+        let family = killFamilycircled
+        DispatchQueue.global(qos: .userInitiated).async {
+            let ok = screentime_disable(agent, usage, homed, family)
+            DispatchQueue.main.async {
+                isWorking = false
+                if ok {
+                    screenTimeDisabled = true
+                    lastResult = "Screen Time disabled. Reboot to apply."
+                } else {
+                    lastResult = "Operation failed. Check logs for details."
+                }
+            }
+        }
+    }
+
+    private func applyEnable() {
+        isWorking = true
+        DispatchQueue.global(qos: .userInitiated).async {
+            let ok = screentime_enable()
+            DispatchQueue.main.async {
+                isWorking = false
+                if ok {
+                    screenTimeDisabled = false
+                    lastResult = "Screen Time enabled. Reboot to apply."
+                } else {
+                    lastResult = "Operation failed. Check logs for details."
+                }
+            }
+        }
+    }
+}
  ```

  </details>
- 🟡 `lara/views/tweaks/TweaksView.swift` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -57,6 +57,8 @@ struct TweaksView: View {
                         .disabled(!mgr.sbxready)
                     NavigationLink("Custom Overwrite", destination: CustomView(mgr: mgr))
                         .disabled(!mgr.vfsready)
+                    NavigationLink("OTA Updates", destination: OTAView(mgr: mgr))
+                    NavigationLink("Screen Time", destination: ScreenTimeView(mgr: mgr))
                 }
                 
                 Section(header: HeaderLabel(text: "Broken", icon: "exclamationmark.triangle.fill")) {
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 1f0c85a: Add files via upload
- : 📝 Commit 6aef6d1: Add files via upload
- : 📝 Commit 173a357: Add ota.h import to Bridging Header
- : 📝 Commit a3fee93: Fix NavigationLink for OTA Updates
- : 📝 Commit 02938c3: Add ota.m and ota.h to project references
- : 📝 Commit 90e4445: Update README.md
- : 📝 Commit dc073b9: Add files via upload
- : 📝 Commit cfcbd9e: Add files via upload
- : 📝 Commit 587e778: Add import for screentime.h in Bridging Header
- : 📝 Commit de80b05: Add NavigationLink for Screen Time view
- : 📝 Commit 12a156f: Add screentime.m and screentime.h to project
- : 📝 Commit 809078e: Add Screen Time Disabler to features list
- Jun 1, 2026: 🟣 @rooootdev merged
- Jun 1, 2026: 🔴 Closed by @rooootdev


---

## Comments (2)

### Comment #1 — [@notthemystery](https://github.com/notthemystery) · May 30, 2026

`ID: 4581780887`


No. It’s not the first project to use those techniques. The first project is https://github.com/kolbicz/DarkSword-Tweaks/tree/main cyanide has also been forked from this


---

### Comment #2 — [@A11pwnX](https://github.com/A11pwnX) · May 30, 2026

`ID: 4581978113`


> No. It’s not the first project to use those techniques. The first project is https://github.com/kolbicz/DarkSword-Tweaks/tree/main cyanide has also been forked from this

Thank you for the information, I have updated my comment.


---

*Exported on 10/14/2026, 3:09:57 PM from [https://github.com/rooootdev/lara/pull/280](https://github.com/rooootdev/lara/pull/280)*
