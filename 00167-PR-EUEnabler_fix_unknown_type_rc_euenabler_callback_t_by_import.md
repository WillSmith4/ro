# EUEnabler: fix unknown type rc_euenabler_callback_t by importing headers

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#167](https://github.com/rooootdev/lara/pull/167)  
> **State:** 🟣 merged  
> **Branch:** `thongass000:dev` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 22, 2026  
> **Changes:** +1 / -0 across 1 files  
> **Created:** Apr 22, 2026  
> **Updated:** Apr 22, 2026  
> **Closed:** Apr 22, 2026  

---

## Description

*Opened by [@thongass000](https://github.com/thongass000)* *on Apr 22, 2026*


```text
/Users/runner/work/lara/lara/lara/kexploit/pe/rc.m:551:56: error: unknown type name 'rc_euenabler_callback_t'
  551 | void euenabler_override_country_code(RemoteCall *proc, rc_euenabler_callback_t callback) {
      |                                                        ^
1 error generated.
```


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`01e9230`](https://github.com/rooootdev/lara/commit/01e9230cd3794244c5fc1729686b8e22bb219e85) EUEnabler: fix unknown type rc_euenabler_callback_t by importing headers — *James Nguyen* · Apr 22, 2026


---

## Changed Files (1)

- 🟡 `lara/kexploit/pe/rc.m` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -19,6 +19,7 @@
 
 #import "RemoteCall.h"
 #import "PrivateAPI.h"
+#import "rc.h"
 
 int ptrace(int _request, pid_t _pid, caddr_t _addr, int _data);
 #define PT_DETACH       11
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 01e9230: EUEnabler: fix unknown type rc_euenabler_callback_t by importing headers
- Apr 22, 2026: 🟣 @rooootdev merged
- Apr 22, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 2:59:53 PM from [https://github.com/rooootdev/lara/pull/167](https://github.com/rooootdev/lara/pull/167)*
