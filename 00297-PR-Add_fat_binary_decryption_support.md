# Add fat binary decryption support

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#297](https://github.com/rooootdev/lara/pull/297)  
> **State:** 🟣 merged  
> **Branch:** `neonmodder123:v2` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Jun 20, 2026  
> **Changes:** +12 / -5 across 1 files  
> **Created:** Jun 17, 2026  
> **Updated:** Jun 20, 2026  
> **Closed:** Jun 20, 2026  

---

## Description

*Opened by [@neonmodder123](https://github.com/neonmodder123)* *on Jun 17, 2026*


Literally the title


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/27710052926/job/81968675305) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/27710052926/job/81968469698) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`2f32b69`](https://github.com/rooootdev/lara/commit/2f32b6969b3a403a1d6dea04b9d84a0974e4169d) add fat binary support — *neon* · Jun 17, 2026


---

## Changed Files (1)

- 🟡 `lara/kexploit/decrypt.m` (modified) — +12 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -42,6 +42,7 @@ void set_progress_callback(progress_callback_t callback) {
     uint64_t text_fileoff;
     uint64_t text_filesize;
     uint64_t binary_size;
+    uint64_t fat_offset;
     bool     is_64;
     uint32_t ncmds;
 };
@@ -139,8 +140,16 @@ static int parse_macho(uint8_t *buf, uint64_t size, struct macho_ctx *ctx) {
             if (OSSwapBigToHostInt32(archs[i].cputype) == CPU_TYPE_ARM64) {
                 uint32_t offset = OSSwapBigToHostInt32(archs[i].offset);
                 uint32_t arch_size = OSSwapBigToHostInt32(archs[i].size);
-                if (offset + arch_size <= size)
-                    return parse_macho(buf + offset, arch_size, ctx);
+                if (offset + arch_size <= size) {
+                    int ret = parse_macho(buf + offset, arch_size, ctx);
+                    if (ret == 0) {
+                        ctx->cryptoff += offset;
+                        ctx->text_fileoff += offset;
+                        ctx->binary_size = size;
+                        ctx->fat_offset = offset;
+                    }
+                    return ret;
+                }
             }
         }
         return -1;
@@ -388,7 +397,7 @@ static int decrypt_to_output(const char *binaryPath, pid_t pid, uint8_t *file_bu
         progupdate((double)pages_done / total_pages);
     }
 
-    uint64_t crypto_off = ctx->is_64 ? sizeof(struct mach_header_64) : sizeof(struct mach_header);
+    uint64_t crypto_off = ctx->fat_offset + (ctx->is_64 ? sizeof(struct mach_header_64) : sizeof(struct mach_header));
     for (uint32_t i = 0; i < ctx->ncmds; i++) {
         if (crypto_off + sizeof(struct load_command) > file_size) break;
         struct load_command *lc = (struct load_command *)(decrypted + crypto_off);
@@ -461,5 +470,3 @@ int decrypt_app(const char *bundlePath, const char *executableName, const char *
     snprintf(execPath, sizeof(execPath), "%s/%s", bundlePath, executableName);
     return decrypt_binary(execPath, executableName, outputPath);
 }
-
-
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 2f32b69: add fat binary support
- Jun 20, 2026: 🟣 @rooootdev merged
- Jun 20, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:11:27 PM from [https://github.com/rooootdev/lara/pull/297](https://github.com/rooootdev/lara/pull/297)*
