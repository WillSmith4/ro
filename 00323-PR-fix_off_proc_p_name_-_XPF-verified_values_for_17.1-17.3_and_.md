# fix: off_proc_p_name - XPF-verified values for 17.1-17.3 and 26.0.x

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#323](https://github.com/rooootdev/lara/pull/323)  
> **State:** 🟢 open  
> **Branch:** `kaffeindecaf:fix/off-proc-p-name-xpf-verified` → `rooootdev:main`  
> **Changes:** +5 / -2 across 1 files  
> **Created:** Sep 2, 2026  

---

## Description

*Opened by [@kaffeindecaf](https://github.com/kaffeindecaf)* *on Sep 2, 2026*


proc.p_name is per-xnu-build, not per-SoC — the previous values were KDK approximations. These come from real kernelcache pulls (libgrabkernel2 + XPF) in the W0lfSword project:

- **17.1–17.3 → 0x520**: XPF-verified on iPhone14,6 (A15/T8110) 17.1 (21B74), xnu-10002.42.9~2. The old 0x579 was KDK-approx and wrong on that range.
- **26.0–26.0.1 → 0x6A0**: XPF-verified on iPhone14,7 (A15/T8110) 26.0.1 (23A355). Per-build: 26.6 resolves 0x488, 17.4+/18.x = 0x57d (0x57D was KDK-approx).

Unchanged: 17.0 keeps 0x579, 17.4+ keeps the 0x57d already set in the 17.4 block, 18.x keeps 0x57d.

Related: #299 (iOS 16.2 p_name should be 0x381 per IDA). This PR doesn't touch 16.x — the value needs confirmation against a 16.x kernelcache — but the same class of KDK-vs-real divergence is at play.

Follow-ups worth a separate PR (not included to keep this one reviewable):
- XPF kstackptr anchor: Apple renamed the panic string to "Invalid kernel stack pointer (probable overflow)." on 26.x — the prebuilt libxpf.dylib still anchors on the old "probable corruption." string, so the kstackptr finder goes dark on 26.x.
- apfs fsnode field offsets as globals (0x70 flags / 0x80 uid / 0x84 gid / 0x88 mode, XPF-verified) if the struct-based access in pe/apfs.m ever drifts.


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`7ddc365`](https://github.com/rooootdev/lara/commit/7ddc3650f1f4a95beac8e9012c9e9e2d9eba459d) fix: off_proc_p_name - XPF-verified values for 17.1-17.3 and 26.0.x — *kaffeindecaf* · Sep 2, 2026


---

## Changed Files (1)

- 🟡 `lara/kexploit/offsets.m` (modified) — +5 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -649,7 +649,10 @@ void offsets_init(void) {
             off_proc_p_flag = 0x25c;
             off_proc_p_textvp = 0x350;
         }
-        off_proc_p_name = 0x579;
+        off_proc_p_name = (SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"17.1") && SYSTEM_VERSION_LESS_THAN(@"17.4")) ? 0x520 : 0x579;
+        // 17.1-17.3 = 0x520: XPF-verified on iPhone14,6 (A15/T8110) 17.1 (21B74)
+        // kernelcache (xnu-10002.42.9~2) - the old 0x579 was KDK-approx and wrong
+        // on that range (17.4+ re-sets 0x57d below, 26.0.x re-sets 0x6A0).
         off_proc_ro_pr_task = 0x8;
         off_proc_ro_p_ucred = 0x20;
         off_ucred_cr_label = 0x78;
@@ -1231,7 +1234,7 @@ void offsets_init(void) {
         off_proc_p_fd = 0xd0;
         off_proc_p_flag = 0x454;
         off_proc_p_textvp = 0x548;
-        off_proc_p_name = 0x57D;
+        off_proc_p_name = 0x6A0;   // 26.0-26.0.1: XPF-verified on iPhone14,7 (A15/T8110) 26.0.1 (23A355) kernelcache — per-BUILD: 26.6 resolves 0x488, 18.x/17.4+ = 0x57d; the old 0x57D was KDK-approx and wrong
         off_proc_ro_pr_task = 0x8;
         off_proc_ro_p_ucred = 0x28;
         off_ucred_cr_label = 0x78;
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 7ddc365: fix: off_proc_p_name - XPF-verified values for 17.1-17.3 and 26.0.x


---

*Exported on 10/14/2026, 3:13:17 PM from [https://github.com/rooootdev/lara/pull/323](https://github.com/rooootdev/lara/pull/323)*
