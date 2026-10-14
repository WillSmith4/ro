# Add app decryption

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#267](https://github.com/rooootdev/lara/pull/267)  
> **State:** 🟣 merged  
> **Branch:** `neonmodder123:v2` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on May 27, 2026  
> **Changes:** +1101 / -1 across 8 files  
> **Created:** May 24, 2026  
> **Updated:** May 27, 2026  
> **Closed:** May 27, 2026  

---

## Description

*Opened by [@neonmodder123](https://github.com/neonmodder123)* *on May 24, 2026*


Add app decryption, add credits and add support to zipmgr to compress files/folders into .zip-s


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/26537658566/job/78171011480) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/26537658566/job/78170712336) — **success** — *GitHub Actions*


---

## PR Commits (12)

- [`b7121c6`](https://github.com/rooootdev/lara/commit/b7121c651ab8be98c624c04e154860c0e7c283dc) add app decryptor — *neon* · May 24, 2026
- [`5876a9f`](https://github.com/rooootdev/lara/commit/5876a9f0ec384cd9a70fcaea96c437dfe9a1bf08) Merge branch 'rooootdev:main' into v2 — *neon* · May 24, 2026
- [`d6d1a4d`](https://github.com/rooootdev/lara/commit/d6d1a4d610f1e3ca2b6626f04d593aa5003f99e6) fix copy fail — *neon* · May 24, 2026
- [`548a021`](https://github.com/rooootdev/lara/commit/548a02108cf36c958492c43668f72360bc52f709) Update decrypt.m — *neon* · May 25, 2026
- [`a3c63c4`](https://github.com/rooootdev/lara/commit/a3c63c473d77b4b7daabcb4af55a435cb016eac1) Revert "fix copy fail" — *neon* · May 25, 2026
- [`af0628e`](https://github.com/rooootdev/lara/commit/af0628e212b90b60eab08e12f28810f6906fcf04) Revert "Update decrypt.m" — *neon* · May 25, 2026
- [`bfb90a5`](https://github.com/rooootdev/lara/commit/bfb90a5f511633db985c00834f2821f5f16fc322) fix — *neon* · May 25, 2026
- [`6a7d646`](https://github.com/rooootdev/lara/commit/6a7d646894035db96d37f00e31f5818f0afbd7a2) add a fallback if not found — *neon* · May 25, 2026
- [`a866f45`](https://github.com/rooootdev/lara/commit/a866f45a4c83182a15864c4b387d9887500c8505) Merge branch 'rooootdev:main' into v2 — *neon* · May 25, 2026
- [`25a0c07`](https://github.com/rooootdev/lara/commit/25a0c07ef7a74b2c2c8ccce5b357aaf4a410de5d) Update DecryptView.swift — *neon* · May 25, 2026
- [`19bba3d`](https://github.com/rooootdev/lara/commit/19bba3d0bf73b9cc83f3e1a31bb22d3e1448d34a) Update DecryptView.swift — *neon* · May 25, 2026
- [`4f1d569`](https://github.com/rooootdev/lara/commit/4f1d569e8c6c6cc8a548e3339329d4a560f7abfa) Merge branch 'main' into v2 — *roooot* · May 27, 2026


---

## Changed Files (8)

- 🟡 `lara/PartyUI/App/Functions/Shareinator.swift` (modified) — +11 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -19,6 +19,17 @@ public func presentShareSheet(with url: URL) {
         }
         
         let activityViewController = UIActivityViewController(activityItems: [url], applicationActivities: nil)
+        
+        if UIDevice.current.userInterfaceIdiom == .pad {
+            activityViewController.popoverPresentationController?.sourceView = topController.view
+            activityViewController.popoverPresentationController?.sourceRect = CGRect(
+                x: topController.view.bounds.midX,
+                y: topController.view.bounds.midY,
+                width: 0, height: 0
+            )
+            activityViewController.popoverPresentationController?.permittedArrowDirections = []
+        }
+        
         topController.present(activityViewController, animated: true)
     }
 }
  ```

  </details>
- 🟡 `lara/classes/zipmgr.swift` (modified) — +162 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -386,3 +386,165 @@ public class ZipArchive {
 
     private static let lfhSize = 30
 }
+
+private func writeLE16(_ value: UInt16, to data: inout Data) {
+    withUnsafeBytes(of: value) { data.append(contentsOf: $0) }
+}
+
+private func writeLE32(_ value: UInt32, to data: inout Data) {
+    withUnsafeBytes(of: value) { data.append(contentsOf: $0) }
+}
+
+private func crc32OfFile(at url: URL) -> (crc: UInt32, data: Data) {
+    let data = (try? Data(contentsOf: url)) ?? Data()
+    return (data.zipCRC32, data)
+}
+
+public func createZipArchive(fromDirectory sourceURL: URL, to destinationURL: URL, compressionMethod: UInt16 = 0) throws {
+    let mgr = laramgr.shared
+    var error = ""
+    let fm = FileManager.default
+    var fileEntries: [(name: String, data: Data, crc32: UInt32)] = []
+    var dirEntries: [String] = []
+
+    let resolvedSource = sourceURL.resolvingSymlinksInPath()
+
+    guard let enumerator = fm.enumerator(at: sourceURL, includingPropertiesForKeys: [.isDirectoryKey], options: [.skipsHiddenFiles]) else {
+        error = "(zip) cannot enumerate source directory"
+        mgr.logmsg("\(error)")
+        throw ZipError.corruptArchive("\(error)")
+    }
+
+    for case let fileURL as URL in enumerator {
+        let resolvedFile = fileURL.resolvingSymlinksInPath()
+        guard let relRange = resolvedFile.path.range(of: resolvedSource.path + "/") else { continue }
+        let relPath = String(resolvedFile.path[relRange.upperBound...])
+        var isDir: ObjCBool = false
+        guard fm.fileExists(atPath: fileURL.path, isDirectory: &isDir) else { continue }
+
+        if isDir.boolValue {
+            dirEntries.append(relPath + "/")
+        } else {
+            let (crc, data) = crc32OfFile(at: fileURL)
+            fileEntries.append((relPath, data, crc))
+        }
+    }
+
+    guard fm.createFile(atPath: destinationURL.path, contents: nil, attributes: nil) else {
+        error = "(zip) cannot create output file"
+        mgr.logmsg("\(error)")
+        throw ZipError.corruptArchive("\(error)")
+    }
+    let fh = try FileHandle(forWritingTo: destinationURL)
+    defer { try? fh.close() }
+
+    var localOffsets: [UInt32] = []
+    var cdEntries: [Data] = []
+
+    for entry in fileEntries {
+        let nameData = entry.name.data(using: .utf8)!
+        let nameLen = UInt16(nameData.count)
+        let offset = UInt32(try fh.offset())
+
+        var lfh = Data()
+        writeLE32(lfhSignature, to: &lfh)
+        writeLE16(20, to: &lfh)
+        writeLE16(0, to: &lfh)
+        writeLE16(0, to: &lfh)
+        writeLE16(0, to: &lfh)
+        writeLE16(0, to: &lfh)
+        writeLE32(entry.crc32, to: &lfh)
+        writeLE32(UInt32(entry.data.count), to: &lfh)
+        writeLE32(UInt32(entry.data.count), to: &lfh)
+        writeLE16(nameLen, to: &lfh)
+        writeLE16(0, to: &lfh)
+        try fh.write(contentsOf: lfh)
+        try fh.write(contentsOf: nameData)
+        try fh.write(contentsOf: entry.data)
+
+        var cd = Data()
+        writeLE32(cdSignature, to: &cd)
+        writeLE16(20, to: &cd)
+        writeLE16(20, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE32(entry.crc32, to: &cd)
+        writeLE32(UInt32(entry.data.count), to: &cd)
+        writeLE32(UInt32(entry.data.count), to: &cd)
+        writeLE16(nameLen, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE32(0x81A40000, to: &cd)
+        writeLE32(offset, to: &cd)
+        cd.append(nameData)
+
+        localOffsets.append(offset)
+        cdEntries.append(cd)
+    }
+
+    for dirName in dirEntries {
+        let nameData = dirName.data(using: .utf8)!
+        let nameLen = UInt16(nameData.count)
+        let offset = UInt32(try fh.offset())
+
+        var lfh = Data()
+        writeLE32(lfhSignature, to: &lfh)
+        writeLE16(20, to: &lfh)
+        writeLE16(0, to: &lfh)
+        writeLE16(0, to: &lfh)
+        writeLE16(0, to: &lfh)
+        writeLE16(0, to: &lfh)
+        writeLE32(0, to: &lfh)
+        writeLE32(0, to: &lfh)
+        writeLE32(0, to: &lfh)
+        writeLE16(nameLen, to: &lfh)
+        writeLE16(0, to: &lfh)
+        try fh.write(contentsOf: lfh)
+        try fh.write(contentsOf: nameData)
+
+        var cd = Data()
+        writeLE32(cdSignature, to: &cd)
+        writeLE16(20, to: &cd)
+        writeLE16(20, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE32(0, to: &cd)
+        writeLE32(0, to: &cd)
+        writeLE32(0, to: &cd)
+        writeLE16(nameLen, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE16(0, to: &cd)
+        writeLE32(0x41ED0000, to: &cd)
+        writeLE32(offset, to: &cd)
+        cd.append(nameData)
+
+        cdEntries.append(cd)
+    }
+
+    let cdOffset = UInt32(try fh.offset())
+    var cdSize: UInt32 = 0
+    for cd in cdEntries {
+        try fh.write(contentsOf: cd)
+        cdSize += UInt32(cd.count)
+    }
+
+    let totalEntries = UInt16(fileEntries.count + dirEntries.count)
+    var eocd = Data()
+    writeLE32(eocdSignature, to: &eocd)
+    writeLE16(0, to: &eocd)
+    writeLE16(0, to: &eocd)
+    writeLE16(totalEntries, to: &eocd)
+    writeLE16(totalEntries, to: &eocd)
+    writeLE32(cdSize, to: &eocd)
+    writeLE32(cdOffset, to: &eocd)
+    writeLE16(0, to: &eocd)
+    try fh.write(contentsOf: eocd)
+}
  ```

  </details>
- 🟢 `lara/kexploit/decrypt.h` (added) — +32 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,32 @@
+//
+//  decrypt.h
+//  lara
+//
+//  Created by neonmodder123 on 23.05.26.
+//
+
+#ifndef decrypt_h
+#define decrypt_h
+
+#include <stdint.h>
+#include <stdbool.h>
+#include <unistd.h>
+
+typedef void (*log_callback_t)(const char *message);
+typedef void (*progress_callback_t)(double progress);
+
+void set_log_callback(log_callback_t callback);
+void set_progress_callback(progress_callback_t callback);
+
+extern volatile double decrypt_progress;
+
+int is_encrypted(const char *bundlePath, const char *executableName);
+int is_encrypted_path(const char *binaryPath);
+int decrypt_app(const char *bundlePath, const char *executableName, const char *outputPath);
+int decrypt_binary(const char *binaryPath, const char *processName, const char *outputPath);
+int decrypt_binary_pid(const char *binaryPath, pid_t pid, const char *outputPath);
+
+pid_t find_process_pid(const char *processName);
+int launch_app(const char *bundleID);
+
+#endif /* decrypt_h */
  ```

  </details>
- 🟢 `lara/kexploit/decrypt.m` (added) — +468 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,468 @@
+//
+//  decrypt.m
+//  lara
+//
+//  Created by neonmodder123 on 23.05.26.
+//
+
+#import "decrypt.h"
+#import "darksword.h"
+#import "offsets.h"
+#import "utils.h"
+#import "vm.h"
+
+#import <Foundation/Foundation.h>
+#import <mach/mach.h>
+#import <mach-o/loader.h>
+#import <mach-o/fat.h>
+#import <fcntl.h>
+#import <unistd.h>
+#import <objc/message.h>
+
+extern kern_return_t mach_vm_deallocate(task_t task, mach_vm_address_t addr, mach_vm_size_t size);
+
+static log_callback_t g_log_cb = NULL;
+static progress_callback_t g_prog_cb = NULL;
+
+void set_log_callback(log_callback_t callback) {
+    g_log_cb = callback;
+}
+
+void set_progress_callback(progress_callback_t callback) {
+    g_prog_cb = callback;
+}
+
+volatile double decrypt_progress = 0.0;
+
+struct macho_ctx {
+    uint32_t cryptid;
+    uint32_t cryptoff;
+    uint32_t cryptsize;
+    uint64_t text_vmaddr;
+    uint64_t text_fileoff;
+    uint64_t text_filesize;
+    uint64_t binary_size;
+    bool     is_64;
+    uint32_t ncmds;
+};
+
+static void logmsg(const char *msg) {
+    if (g_log_cb) { g_log_cb(msg); }
+    printf("(decrypt) %s\n", msg);
+}
+
+static void progupdate(double p) {
+    decrypt_progress = p;
+    if (g_prog_cb) { g_prog_cb(p); }
+}
+
+int launch_app(const char *bundleID) {
+    char msg[256];
+
+    Class cls = objc_getClass("LSApplicationWorkspace");
+    if (!cls) {
+        snprintf(msg, sizeof(msg), "LSApplicationWorkspace not found");
+        logmsg(msg);
+        return -1;
+    }
+
+    id ws = ((id (*)(id, SEL))objc_msgSend)((id)cls, sel_registerName("defaultWorkspace"));
+    if (!ws) {
+        snprintf(msg, sizeof(msg), "defaultWorkspace returned nil");
+        logmsg(msg);
+        return -1;
+    }
+
+    NSString *bid = [[NSString alloc] initWithUTF8String:bundleID];
+    BOOL ok = ((BOOL (*)(id, SEL, id))objc_msgSend)(ws, sel_registerName("openApplicationWithBundleID:"), bid);
+    if (!ok) {
+        snprintf(msg, sizeof(msg), "openApplicationWithBundleID failed for %s", bundleID);
+        logmsg(msg);
+        return -1;
+    }
+
+    return 0;
+}
+
+pid_t find_process_pid(const char *processName) {
+    uint64_t proc = procbyname(processName);
+    if (proc == 0) {
+        char name_buf[64] = {0};
+        strncpy(name_buf, processName, 63);
+        char *dot = strchr(name_buf, '.');
+        if (dot) *dot = '\0';
+        proc = procbyname(name_buf);
+    }
+    if (proc == 0) return -1;
+
+    pid_t pid = ds_kread32(proc + off_proc_p_pid);
+    char msg[256];
+    snprintf(msg, sizeof(msg), "found pid %d", pid);
+    logmsg(msg);
+    return pid;
+}
+
+static int refile(const char *path, uint8_t **out, uint64_t *out_size) {
+    int fd = open(path, O_RDONLY);
+    if (fd < 0) { return -1; }
+    off_t fsz = lseek(fd, 0, SEEK_END);
+    lseek(fd, 0, SEEK_SET);
+    if (fsz <= 0) { close(fd); logmsg("empty file"); return -1; }
+    uint8_t *buf = malloc((size_t)fsz);
+    if (!buf) { close(fd); logmsg("malloc failed"); return -1; }
+    ssize_t n = read(fd, buf, (size_t)fsz);
+    close(fd);
+    if (n != fsz) { free(buf); logmsg("read failed to read full binary"); return -1; }
+    *out = buf;
+    *out_size = (uint64_t)fsz;
+    return 0;
+}
+
+static int write_file(const char *path, uint8_t *data, uint64_t size) {
+    int fd = open(path, O_WRONLY | O_CREAT | O_TRUNC, 0644);
+    if (fd < 0) { logmsg("open for write failed"); return -1; }
+    ssize_t n = write(fd, data, (size_t)size);
+    close(fd);
+    if (n != (ssize_t)size) { logmsg("write failed"); return -1; }
+    return 0;
+}
+
+static int parse_macho(uint8_t *buf, uint64_t size, struct macho_ctx *ctx) {
+    memset(ctx, 0, sizeof(*ctx));
+    uint32_t magic = *(uint32_t *)buf;
+
+    if (magic == FAT_MAGIC || magic == FAT_CIGAM) {
+        struct fat_header *fh = (struct fat_header *)buf;
+        uint32_t narch = OSSwapBigToHostInt32(fh->nfat_arch);
+        struct fat_arch *archs = (struct fat_arch *)(buf + sizeof(struct fat_header));
+        for (uint32_t i = 0; i < narch; i++) {
+            if (OSSwapBigToHostInt32(archs[i].cputype) == CPU_TYPE_ARM64) {
+                uint32_t offset = OSSwapBigToHostInt32(archs[i].offset);
+                uint32_t arch_size = OSSwapBigToHostInt32(archs[i].size);
+                if (offset + arch_size <= size)
+                    return parse_macho(buf + offset, arch_size, ctx);
+            }
+        }
+        return -1;
+    }
+
+    if (magic != MH_MAGIC_64 && magic != MH_MAGIC) {
+        return -1;
+    }
+
+    ctx->is_64 = (magic == MH_MAGIC_64);
+    struct mach_header_64 *header64 = (struct mach_header_64 *)buf;
+    struct mach_header *header32 = (struct mach_header *)buf;
+    ctx->ncmds = ctx->is_64 ? header64->ncmds : header32->ncmds;
+
+    uint64_t off = ctx->is_64 ? sizeof(struct mach_header_64) : sizeof(struct mach_header);
+    bool found_encryption = false;
+    bool found_text = false;
+
+    for (uint32_t i = 0; i < ctx->ncmds; i++) {
+        if (off + sizeof(struct load_command) > size) break;
+        struct load_command *lc = (struct load_command *)(buf + off);
+
+        if (lc->cmd == LC_ENCRYPTION_INFO_64 && ctx->is_64) {
+            struct encryption_info_command_64 *eic = (struct encryption_info_command_64 *)lc;
+            ctx->cryptid = eic->cryptid;
+            ctx->cryptoff = eic->cryptoff;
+            ctx->cryptsize = eic->cryptsize;
+            found_encryption = true;
+        } else if (lc->cmd == LC_ENCRYPTION_INFO && !ctx->is_64) {
+            struct encryption_info_command *eic = (struct encryption_info_command *)lc;
+            ctx->cryptid = eic->cryptid;
+            ctx->cryptoff = eic->cryptoff;
+            ctx->cryptsize = eic->cryptsize;
+            found_encryption = true;
+        } else if (lc->cmd == LC_SEGMENT_64 && ctx->is_64) {
+            struct segment_command_64 *seg = (struct segment_command_64 *)lc;
+            if (strncmp(seg->segname, "__TEXT", 16) == 0) {
+                ctx->text_vmaddr = seg->vmaddr;
+                ctx->text_fileoff = seg->fileoff;
+                ctx->text_filesize = seg->filesize;
+                found_text = true;
+            }
+        } else if (lc->cmd == LC_SEGMENT && !ctx->is_64) {
+            struct segment_command *seg = (struct segment_command *)lc;
+            if (strncmp(seg->segname, "__TEXT", 16) == 0) {
+                ctx->text_vmaddr = seg->vmaddr;
+                ctx->text_fileoff = seg->fileoff;
+                ctx->text_filesize = seg->filesize;
+                found_text = true;
+            }
+        }
+        off += lc->cmdsize;
+    }
+
+    if (!found_encryption) return -1;
+    if (!found_text) return -1;
+    ctx->binary_size = size;
+    return 0;
+}
+
+int is_encrypted_path(const char *binaryPath) {
+    if (!ds_is_ready()) return -1;
+    uint8_t *buf = NULL;
+    uint64_t size = 0;
+    if (refile(binaryPath, &buf, &size) != 0) return -1;
+    struct macho_ctx ctx;
+    int ret = parse_macho(buf, size, &ctx);
+    free(buf);
+    if (ret != 0) return -1;
+    return ctx.cryptid != 0 ? 1 : 0;
+}
+
+int is_encrypted(const char *bundlePath, const char *executableName) {
+    char execPath[1024];
+    snprintf(execPath, sizeof(execPath), "%s/%s", bundlePath, executableName);
+    return is_encrypted_path(execPath);
+}
+
+static int find_text_segment_in_vm_map(uint64_t vmMap, uint64_t fileoff, uint64_t *out_addr) {
+    __block uint64_t found_addr = 0;
+    __block BOOL done = NO;
+
+    vmmapiterateentries(vmMap, ^(uint64_t start, uint64_t end, uint64_t entry, BOOL *stop) {
+        if (done) return;
+        if (start == 0 || start >= 0xffffff8000000000ULL) return;
+
+        uint64_t alias_offset = ds_kread64(entry + off_vm_map_entry_vme_alias);
+        if ((alias_offset >> 12) == (fileoff >> 14) && start > 0x100000000ULL) {
+            struct vmshmem shmem = vmmapremotepage(vmMap, start);
+            if (shmem.used) {
+                uint32_t magic = *(volatile uint32_t *)(uintptr_t)shmem.localAddress;
+                mach_vm_deallocate(mach_task_self_, shmem.localAddress, PAGE_SIZE);
+                if (magic == MH_MAGIC_64 || magic == MH_MAGIC ||
+                    magic == FAT_MAGIC || magic == FAT_CIGAM) {
+                    found_addr = start;
+                    done = YES;
+                    *stop = YES;
+                }
+            }
+        }
+    });
+
+    if (found_addr == 0) {
+        vmmapiterateentries(vmMap, ^(uint64_t start, uint64_t end, uint64_t entry, BOOL *stop) {
+            if (done) return;
+            if (start == 0 || start >= 0xffffff8000000000ULL) return;
+
+            struct vmshmem shmem = vmmapremotepage(vmMap, start);
+            if (shmem.used) {
+                uint32_t magic = *(volatile uint32_t *)(uintptr_t)shmem.localAddress;
+                mach_vm_deallocate(mach_task_self_, shmem.localAddress, PAGE_SIZE);
+                if (magic == MH_MAGIC_64 || magic == MH_MAGIC ||
+                    magic == FAT_MAGIC || magic == FAT_CIGAM) {
+                    found_addr = start;
+                    done = YES;
+                    *stop = YES;
+                }
+            }
+        });
+    }
+
+    *out_addr = found_addr;
+    return (found_addr != 0) ? 0 : -1;
+}
+
+static int find_macho_in_vm(uint64_t vmMap, uint64_t *out_base, uint64_t *out_end) {
+    __block uint64_t found = 0;
+    __block uint64_t found_end = 0;
+
+    vmmapiterateentries(vmMap, ^(uint64_t start, uint64_t end, uint64_t entry, BOOL *stop) {
+        if (found) return;
+        if (start == 0 || start >= 0xffffff8000000000ULL) return;
+
+        uint64_t raw = ds_kread64(entry + off_vm_map_entry_vme_alias);
+        if ((raw >> 12) != 0) return;
+
+        struct vmshmem shmem = vmmapremotepage(vmMap, start);
+        if (shmem.used) {
+            uint32_t magic = *(volatile uint32_t *)(uintptr_t)shmem.localAddress;
+            mach_vm_deallocate(mach_task_self_, shmem.localAddress, PAGE_SIZE);
+            if (magic == MH_MAGIC_64 || magic == MH_MAGIC) {
+                found = start;
+                found_end = end;
+                *stop = YES;
+            }
+        }
+    });
+
+    if (found == 0) {
+        vmmapiterateentries(vmMap, ^(uint64_t start, uint64_t end, uint64_t entry, BOOL *stop) {
+            if (found) return;
+            if (start == 0 || start >= 0xffffff8000000000ULL) return;
+
+            struct vmshmem shmem = vmmapremotepage(vmMap, start);
+            if (shmem.used) {
+                uint32_t magic = *(volatile uint32_t *)(uintptr_t)shmem.localAddress;
+                mach_vm_deallocate(mach_task_self_, shmem.localAddress, PAGE_SIZE);
+                if (magic == MH_MAGIC_64 || magic == MH_MAGIC) {
+                    found = start;
+                    found_end = end;
+                    *stop = YES;
+                }
+            }
+        });
+    }
+
+    *out_base = found;
+    if (out_end) *out_end = found_end;
+    return found != 0 ? 0 : -1;
+}
+
+static int read_file_from_vm(uint64_t vmMap, uint64_t base, uint64_t size, uint8_t **out) {
+    uint8_t *buf = malloc(size);
+    if (!buf) return -1;
+    uint64_t off = 0;
+    while (off < size) {
+        uint64_t addr = base + off;
+        uint64_t page_start = addr & ~(PAGE_SIZE - 1);
+        uint64_t page_off = addr - page_start;
+        struct vmshmem shmem = vmmapremotepage(vmMap, page_start);
+        if (!shmem.used) { off += PAGE_SIZE - page_off; continue; }
+        uint64_t cpy = size - off;
+        if (cpy > PAGE_SIZE - page_off) cpy = PAGE_SIZE - page_off;
+        memcpy(buf + off, (void *)(uintptr_t)shmem.localAddress + page_off, cpy);
+        mach_vm_deallocate(mach_task_self_, shmem.localAddress, PAGE_SIZE);
+        off += cpy;
+    }
+    *out = buf;
+    return 0;
+}
+
+static int decrypt_to_output(const char *binaryPath, pid_t pid, uint8_t *file_buf, uint64_t file_size, struct macho_ctx *ctx, const char *outputPath) {
+    char msg[256];
+
+    uint64_t proc = procbypid(pid);
+    if (proc == 0) {
+        snprintf(msg, sizeof(msg), "process pid %d not found", pid);
+        logmsg(msg);
+        return -1;
+    }
+
+    uint64_t task = taskbyproc(proc);
+    if (task == 0) { logmsg("failed to get task"); return -1; }
+
+    uint64_t vmMap = task_get_vm_map(task);
+    if (vmMap == 0) { logmsg("failed to get vm_map"); return -1; }
+
+    uint64_t text_addr = 0;
+    if (find_text_segment_in_vm_map(vmMap, ctx->text_fileoff, &text_addr) != 0) {
+        logmsg("failed to find text segment in process memory");
+        return -1;
+    }
+
+    uint8_t *decrypted = malloc(file_size);
+    if (!decrypted) { logmsg("malloc failed"); return -1; }
+    memcpy(decrypted, file_buf, file_size);
+
+    uint64_t segment_fileoff = ctx->text_fileoff;
+    uint32_t page_size = (uint32_t)PAGE_SIZE;
+    uint32_t start_page = ctx->cryptoff / page_size;
+    uint32_t end_page = (ctx->cryptoff + ctx->cryptsize + page_size - 1) / page_size;
+    uint32_t total_pages = end_page - start_page;
+    uint32_t pages_done = 0;
+
+    for (uint32_t pg = start_page; pg < end_page; pg++) {
+        uint64_t pg_file_start = (uint64_t)pg * page_size;
+        uint64_t pg_virt = text_addr + pg_file_start - segment_fileoff;
+
+        struct vmshmem shmem = vmmapremotepage(vmMap, pg_virt);
+        if (!shmem.used) {
+            shmem = vmmapremotepage(vmMap, pg_virt & ~(page_size - 1));
+            if (!shmem.used) {
+                pages_done++;
+                progupdate((double)pages_done / total_pages);
+                continue;
+            }
+        }
+
+        uint64_t copy_off = pg_file_start;
+        uint32_t copy_size = page_size;
+        if (copy_off + copy_size > file_size)
+            copy_size = (uint32_t)(file_size - copy_off);
+
+        memcpy(decrypted + copy_off, (void *)(uintptr_t)shmem.localAddress, copy_size);
+        mach_vm_deallocate(mach_task_self_, shmem.localAddress, PAGE_SIZE);
+
+        pages_done++;
+        progupdate((double)pages_done / total_pages);
+    }
+
+    uint64_t crypto_off = ctx->is_64 ? sizeof(struct mach_header_64) : sizeof(struct mach_header);
+    for (uint32_t i = 0; i < ctx->ncmds; i++) {
+        if (crypto_off + sizeof(struct load_command) > file_size) break;
+        struct load_command *lc = (struct load_command *)(decrypted + crypto_off);
+        if (lc->cmd == LC_ENCRYPTION_INFO_64 && ctx->is_64) {
+            ((struct encryption_info_command_64 *)lc)->cryptid = 0;
+            break;
+        } else if (lc->cmd == LC_ENCRYPTION_INFO && !ctx->is_64) {
+            ((struct encryption_info_command *)lc)->cryptid = 0;
+            break;
+        }
+        crypto_off += lc->cmdsize;
+    }
+
+    if (write_file(outputPath, decrypted, file_size) != 0) {
+        free(decrypted);
+        logmsg("failed to write decrypted binary");
+        return -1;
+    }
+
+    free(decrypted);
+    return 0;
+}
+
+int decrypt_binary_pid(const char *binaryPath, pid_t process_pid, const char *outputPath) {
+    progupdate(0.0);
+    if (!ds_is_ready()) { logmsg("darksword not ready"); return -1; }
+
+    uint8_t *file_buf = NULL;
+    uint64_t file_size = 0;
+
+    if (refile(binaryPath, &file_buf, &file_size) != 0) {
+        uint64_t proc = procbypid(process_pid);
+        if (proc == 0) { logmsg("process not found"); return -1; }
+        uint64_t task = taskbyproc(proc);
+        if (task == 0) { logmsg("failed to get task"); return -1; }
+        uint64_t vmMap = task_get_vm_map(task);
+        if (vmMap == 0) { logmsg("failed to get vm_map"); return -1; }
+
+        uint64_t base = 0, end = 0;
+        if (find_macho_in_vm(vmMap, &base, &end) != 0) { logmsg("binary not found in process memory"); return -1; }
+        file_size = end - base;
+        if (read_file_from_vm(vmMap, base, file_size, &file_buf) != 0) { logmsg("failed to read binary from process memory"); return -1; }
+    }
+
+    struct macho_ctx ctx;
+    if (parse_macho(file_buf, file_size, &ctx) != 0) { free(file_buf); logmsg("failed to parse mach-o"); return -1; }
+    if (ctx.cryptid == 0) { free(file_buf); logmsg("binary not encrypted"); return -1; }
+
+    int ret = decrypt_to_output(binaryPath, process_pid, file_buf, file_size, &ctx, outputPath);
+    free(file_buf);
+    progupdate(1.0);
+    return ret;
+}
+
+int decrypt_binary(const char *binaryPath, const char *processName, const char *outputPath) {
+    char msg[256];
+
+    pid_t pid = find_process_pid(processName);
+    if (pid <= 0) {
+        snprintf(msg, sizeof(msg), "process '%s' not found", processName);
+        logmsg(msg);
+        return -1;
+    }
+
+    return decrypt_binary_pid(binaryPath, pid, outputPath);
+}
+
+int decrypt_app(const char *bundlePath, const char *executableName, const char *outputPath) {
+    char execPath[1024];
+    snprintf(execPath, sizeof(execPath), "%s/%s", bundlePath, executableName);
+    return decrypt_binary(execPath, executableName, outputPath);
+}
+
+
  ```

  </details>
- 🟡 `lara/lara-Bridging-Header.h` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -16,6 +16,7 @@
 #import "IconServices.h"
 #import "rc.h"
 #import "RemoteCall.h"
+#import "decrypt.h"
 #import "persistence.h"
 
 #import <zlib.h>
  ```

  </details>
- 🟡 `lara/views/app/settings/CreditsView.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -32,7 +32,7 @@ struct CreditsView: View {
                 LinkCreditCell(name: "Jurre", description: "EditorView, PocketPoster Helper, various improvements", url: "https://github.com/jurre111") {
                     LinkCreditIcon(url: "https://github.com/jurre111.png")
                 }
-                LinkCreditCell(name: "neon", description: "Respring Script and zipmgr", url: "https://github.com/neonmodder123") {
+                LinkCreditCell(name: "neon", description: "Respring Script, zipmgr, fixing Passcode Themes, and adding App Decryption", url: "https://github.com/neonmodder123") {
                     LinkCreditIcon(url: "https://github.com/neonmodder123.png")
                 }
                 LinkCreditCell(name: "Skadz", description: "Respring Method", url: "https://github.com/skadz108") {
  ```

  </details>
- 🟢 `lara/views/tweaks/DecryptView.swift` (added) — +424 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,424 @@
+//
+//  DecryptView.swift
+//  lara
+//
+//  Created by neonmodder123 on 23.05.26.
+//
+
+import SwiftUI
+
+struct decryptapp: Identifiable {
+    let id = UUID()
+    let name: String
+    let bundleid: String
+    let bundlepath: String
+    let executable: String
+    let icon: UIImage?
+    var isencrypted: decryptstatus = .unknown
+}
+
+enum decryptstatus {
+    case encrypted, unencrypted, unknown
+}
+
+struct DecryptView: View {
+    @ObservedObject private var mgr = laramgr.shared
+    @State private var query = ""
+    @State private var apps: [decryptapp] = []
+    @State private var decryptingbid: String? = nil
+    @State private var errormsg: String? = nil
+    @State private var pendingdecrypt: decryptapp? = nil
+
+    private let documentspath = NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true).first ?? ""
+
+    private var filteredapps: [decryptapp] {
+        let trimmed = query.trimmingCharacters(in: .whitespacesAndNewlines)
+        if trimmed.isEmpty { return apps }
+        let q = trimmed.lowercased()
+        return apps.filter { $0.name.lowercased().contains(q) || $0.bundleid.lowercased().contains(q) }
+    }
+
+    var body: some View {
+        NavigationStack {
+            List {
+                if !mgr.sbxready {
+                    Section {
+                        Text("Sandbox escape not ready. Run the sandbox escape first.")
+                            .foregroundColor(.secondary)
+                    } header: { Text("Status") }
+                }
+
+                HStack {
+                    TextField("Search", text: $query)
+                        .textInputAutocapitalization(.never)
+                        .autocorrectionDisabled()
+                    Button(action: loadapps) {
+                        Image(systemName: "arrow.clockwise")
+                    }
+                    .disabled(!mgr.sbxready || decryptingbid != nil)
+                }
+
+                if let error = errormsg {
+                    Section {
+                        PlainAlert(title: "Error", icon: "exclamationmark.triangle", text: error, color: .red)
+                    }
+                }
+
+                Section {
+                    if filteredapps.isEmpty {
+                        Text(query.isEmpty ? "Loading..." : "No matches.")
+                            .foregroundColor(.secondary)
+                    } else {
+                        ForEach(filteredapps) { app in
+                            AppRow(app: app, isdecrypting: decryptingbid == app.bundleid) {
+                                startdecrypt(app)
+                            }
+                        }
+                    }
+                } header: {
+                    HeaderLabel(text: "Installed Apps", icon: "app.badge")
+                }
+            }
+            .navigationTitle("App Decrypt")
+        }
+        .onAppear {
+            set_log_callback { msg in
+                guard let msg = msg else { return }
+                let s = String(cString: msg)
+                DispatchQueue.main.async {
+                    laramgr.shared.logmsg("(decrypt) \(s)")
+                }
+            }
+            if mgr.sbxready { loadapps() }
+        }
+        .onReceive(NotificationCenter.default.publisher(for: UIApplication.didBecomeActiveNotification)) { _ in
+            if let app = pendingdecrypt {
+                pendingdecrypt = nil
+                let pid = find_process_pid(app.executable)
+                if pid > 0 {
+                    dodecrypt(app, pid: pid)
+                } else {
+                    errormsg = "App is not running, try again."
+                    decryptingbid = nil
+                }
+            }
+        }
+        .onChange(of: mgr.sbxready) { ready in
+            if ready { loadapps() } else { apps.removeAll() }
+        }
+    }
+
+    func loadapps() {
+        guard mgr.sbxready else { return }
+        DispatchQueue.global(qos: .userInitiated).async {
+            var results: [decryptapp] = []
+            let bundleFolder = "/private/var/containers/Bundle/Application"
+
+            guard let bundles = try? FileManager.default.contentsOfDirectory(atPath: bundleFolder) else {
+                DispatchQueue.main.async { apps.removeAll() }
+                return
+            }
+
+            for bundle in bundles {
+                let appPath = bundleFolder + "/" + bundle
+                guard let contents = try? FileManager.default.contentsOfDirectory(atPath: appPath) else { continue }
+                for item in contents {
+                    guard item.hasSuffix(".app") else { continue }
+                    let fullAppPath = appPath + "/" + item
+                    let infoPath = fullAppPath + "/Info.plist"
+                    guard let info = NSDictionary(contentsOfFile: infoPath) else { continue }
+
+                    let executable = info["CFBundleExecutable"] as? String ?? ""
+                    if executable.isEmpty { continue }
+                    let bundleid = info["CFBundleIdentifier"] as? String ?? ""
+                    let name = (info["CFBundleDisplayName"] as? String) ??
+                               (info["CFBundleName"] as? String) ??
+                               (item as NSString).deletingPathExtension
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
+                    let encResult = is_encrypted(fullAppPath, executable)
+                    let status: decryptstatus
+                    if encResult > 0 { status = .encrypted }
+                    else if encResult == 0 { status = .unencrypted }
+                    else { status = .unknown }
+
+                    results.append(decryptapp(
+                        name: name, bundleid: bundleid, bundlepath: fullAppPath,
+                        executable: executable,
+                        icon: icon ?? UIImage(named: "unknown"),
+                        isencrypted: status
+                    ))
+                    break
+                }
+            }
+
+            results.sort { $0.name.lowercased() < $1.name.lowercased() }
+            DispatchQueue.main.async { apps = results }
+        }
+    }
+
+    func startdecrypt(_ app: decryptapp) {
+        guard decryptingbid == nil && pendingdecrypt == nil else { return }
+        guard mgr.dsready else { errormsg = "Darksword not ready, run the exploit first."; return }
+        guard mgr.sbxready else { errormsg = "Sandbox not escaped."; return }
+
+        let appurl = URL(fileURLWithPath: app.bundlepath)
+        var total: Int64 = 0
+        if let enumerator = FileManager.default.enumerator(at: appurl, includingPropertiesForKeys: [.fileSizeKey]) {
+            for case let furi as URL in enumerator {
+                if let fs = (try? furi.resourceValues(forKeys: [.fileSizeKey]))?.fileSize {
+                    total += Int64(fs)
+                }
+            }
+        }
+        if total > 200 * 1024 * 1024 {
+            Alertinator.shared.alert(
+                title: "Large App",
+                body: "This app is over 200 MB, decryption may take a while.",
+                actionLabel: "Continue",
+                action: { self.rundecrypt(app) }
+            )
+            return
+        }
+        rundecrypt(app)
+    }
+
+    func rundecrypt(_ app: decryptapp) {
+        errormsg = nil
+        decryptingbid = app.bundleid
+
+        let pid = find_process_pid(app.executable)
+        if pid > 0 {
+            dodecrypt(app, pid: pid)
+        } else {
+            pendingdecrypt = app
+
+            var bgTask: UIBackgroundTaskIdentifier = .invalid
+            bgTask = UIApplication.shared.beginBackgroundTask(withName: "AutoResumeDecrypt") {
+                UIApplication.shared.endBackgroundTask(bgTask)
+            }
+
+            let ret = launch_app(app.bundleid)
+            guard ret == 0 else {
+                UIApplication.shared.endBackgroundTask(bgTask)
+                pendingdecrypt = nil
+                decryptingbid = nil
+                errormsg = "Could not launch app. Open it manually."
+                return
+            }
+
+            DispatchQueue.global(qos: .userInitiated).asyncAfter(deadline: .now() + 2.5) {
+                launch_app(Bundle.main.bundleIdentifier!)
+                usleep(500000)
+
+                if self.pendingdecrypt != nil {
+                    self.pendingdecrypt = nil
+                    let foundPid = find_process_pid(app.executable)
+                    if foundPid > 0 {
+                        DispatchQueue.main.async {
+                            self.dodecrypt(app, pid: foundPid)
+                        }
+                    } else {
+                        DispatchQueue.main.async {
+                            self.decryptingbid = nil
+                            self.errormsg = "Process not found after launch. Try manually."
+                        }
+                    }
+                }
+
+                DispatchQueue.main.async {
+                    UIApplication.shared.endBackgroundTask(bgTask)
+                }
+            }
+        }
+    }
+
+    func dodecrypt(_ app: decryptapp, pid: pid_t) {
+        let appName = (app.bundlepath as NSString).lastPathComponent
+        let ipaName = app.name
+            .replacingOccurrences(of: " ", with: "")
+            .replacingOccurrences(of: "\t", with: "")
+            .appending(".ipa")
+        let workDir = documentspath + "/Decrypted/.tmp_" + app.bundleid
+        let destAppPath = workDir + "/" + appName
+        let payloadDir = workDir + "/Payload"
+        let payloadAppPath = payloadDir + "/" + appName
+        let ipaPath = documentspath + "/Decrypted/" + ipaName
+
+        laramgr.shared.logmsg("(decrypt) decrypting \(app.bundleid)...")
+
+        DispatchQueue.global(qos: .userInitiated).async {
+            let fm = FileManager.default
+
+            try? fm.removeItem(atPath: workDir)
+            try? fm.createDirectory(atPath: payloadDir, withIntermediateDirectories: true)
+
+            cpdir(app.bundlepath, destAppPath)
+            var dstSt = stat()
+            guard stat(destAppPath, &dstSt) == 0 else {
+                DispatchQueue.main.async {
+                    decryptingbid = nil
+                    errormsg = "Failed to copy bundle, some files may be inaccessible."
+                    laramgr.shared.logmsg("(decrypt) copy failed: could not copy \(app.bundlepath)")
+                }
+                return
+            }
+
+            let mainBinary = destAppPath + "/" + app.executable
+            let mainRet = decrypt_binary_pid((app.bundlepath + "/" + app.executable), pid, mainBinary)
+            guard mainRet == 0 else {
+                DispatchQueue.main.async {
+                    decryptingbid = nil
+                    errormsg = "Failed to decrypt main binary"
+                    laramgr.shared.logmsg("(decrypt) main binary decrypt failed")
+                }
+                return
+            }
+
+            let srcFrameworks = app.bundlepath + "/Frameworks"
+            var srcFwSt = stat()
+            if stat(srcFrameworks, &srcFwSt) == 0 {
+                let frameworksPath = destAppPath + "/Frameworks"
+                let frameworks = (try? fm.contentsOfDirectory(atPath: frameworksPath)) ?? []
+                for fw in frameworks where fw.hasSuffix(".framework") {
+                    let fwName = (fw as NSString).deletingPathExtension
+                    let fwBinary = frameworksPath + "/" + fw + "/" + fwName
+                    if !fm.fileExists(atPath: fwBinary) { continue }
+                    if is_encrypted_path(fwBinary) > 0 {
+                        let fwRet = decrypt_binary_pid(fwBinary, pid, fwBinary)
+                        if fwRet != 0 {
+                            laramgr.shared.logmsg("(decrypt) framework \(fwName) decrypt failed")
+                        }
+                    }
+                }
+            }
+
+            do {
+                try fm.moveItem(atPath: destAppPath, toPath: payloadAppPath)
+            } catch {
+                DispatchQueue.main.async {
+                    decryptingbid = nil
+                    errormsg = "Failed to move app bundle: \(error.localizedDescription)"
+                    laramgr.shared.logmsg("(decrypt) move failed: \(error.localizedDescription)")
+                }
+                return
+            }
+
+            do {
+                try createZipArchive(
+                    fromDirectory: URL(fileURLWithPath: workDir, isDirectory: true),
+                    to: URL(fileURLWithPath: ipaPath)
+                )
+            } catch {
+                DispatchQueue.main.async {
+                    decryptingbid = nil
+                    errormsg = "Failed to create ipa: \(error.localizedDescription)"
+                    laramgr.shared.logmsg("(decrypt) zip failed: \(error.localizedDescription)")
+                }
+                return
+            }
+
+            try? fm.removeItem(atPath: workDir)
+
+            DispatchQueue.main.async {
+                decryptingbid = nil
+                laramgr.shared.logmsg("(decrypt) ipa saved to \(ipaPath)")
+                DispatchQueue.main.asyncAfter(deadline: .now() + 0.5) {
+                    presentShareSheet(with: URL(fileURLWithPath: ipaPath))
+                }
+            }
+        }
+    }
+}
+
+struct AppRow: View {
+    let app: decryptapp
+    let isdecrypting: Bool
+    let ondecrypt: () -> Void
+
+    var body: some View {
+        HStack {
+            if let icon = app.icon {
+                Image(uiImage: icon)
+                    .resizable().frame(width: 40, height: 40)
+                    .clipShape(RoundedRectangle(cornerRadius: 9))
+            } else {
+                Image("unknown")
+                    .resizable().frame(width: 40, height: 40)
+                    .clipShape(RoundedRectangle(cornerRadius: 9))
+            }
+
+            VStack(alignment: .leading, spacing: 2) {
+                Text(app.name).font(.headline)
+                Text(app.bundleid).font(.caption).foregroundColor(.gray)
+                HStack(spacing: 4) {
+                    switch app.isencrypted {
+                    case .encrypted:
+                        Text("Encrypted").font(.caption2).foregroundStyle(.orange)
+                    case .unencrypted:
+                        Text("Unencrypted").font(.caption2).foregroundStyle(.green)
+                    case .unknown:
+                        Text("Not eligible").font(.caption2).foregroundStyle(.gray)
+                    }
+                }
+            }
+
+            Spacer()
+
+            switch app.isencrypted {
+            case .encrypted:
+                Button(action: ondecrypt) {
+                    if isdecrypting { ProgressView() }
+                    else { Text("Decrypt") }
+                }
+                .disabled(isdecrypting)
+            case .unknown:
+                Button(action: ondecrypt) {
+                    if isdecrypting { ProgressView() }
+                    else { Text("Decrypt") }
+                }
+                .disabled(true)
+            case .unencrypted:
+                Image(systemName: "checkmark.circle").foregroundColor(.green)
+            }
+        }
+        .opacity(isdecrypting ? 0.6 : 1.0)
+    }
+}
+
+func cpdir(_ src: String, _ dst: String) {
+    var isDir = ObjCBool(false)
+    let exists = FileManager.default.fileExists(atPath: src, isDirectory: &isDir)
+
+    if exists, !isDir.boolValue {
+        guard let handle = try? FileHandle(forReadingFrom: URL(fileURLWithPath: src)) else { return }
+        defer { try? handle.close() }
+        let data: Data
+        if #available(iOS 13.4, *) {
+            guard let d = try? handle.readToEnd() else { return }
+            data = d
+        } else {
+            data = handle.readDataToEndOfFile()
+        }
+        try? data.write(to: URL(fileURLWithPath: dst), options: .atomic)
+        return
+    }
+
+    guard exists, isDir.boolValue else { return }
+    try? FileManager.default.createDirectory(atPath: dst, withIntermediateDirectories: true)
+
+    guard let items = try? FileManager.default.contentsOfDirectory(atPath: src) else { return }
+    for item in items {
+        cpdir(src + "/" + item, dst + "/" + item)
+    }
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
+                    NavigationLink("App Decrypt", destination: DecryptView())
+                        .disabled(!mgr.sbxready)
                     NavigationLink("3 App Bypass", destination: AppsView())
                         .disabled(!mgr.sbxready)
                     NavigationLink("Unblacklist", destination: WhitelistView())
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit b7121c6: add app decryptor
- : 📝 Commit 5876a9f: Merge branch 'rooootdev:main' into v2
- May 24, 2026: ⚪ @neonmodder123 converted to draft
- : 📝 Commit d6d1a4d: fix copy fail
- : 📝 Commit 548a021: Update decrypt.m
- : 📝 Commit a3c63c4: Revert "fix copy fail"
- : 📝 Commit af0628e: Revert "Update decrypt.m"
- : 📝 Commit bfb90a5: fix
- : 📝 Commit 6a7d646: add a fallback if not found
- : 📝 Commit a866f45: Merge branch 'rooootdev:main' into v2
- : 📝 Commit 25a0c07: Update DecryptView.swift
- : 📝 Commit 19bba3d: Update DecryptView.swift
- May 27, 2026: 🟢 @neonmodder123 marked ready for review
- : 📝 Commit 4f1d569: Merge branch 'main' into v2
- May 27, 2026: 🟣 @rooootdev merged
- May 27, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:08:41 PM from [https://github.com/rooootdev/lara/pull/267](https://github.com/rooootdev/lara/pull/267)*
