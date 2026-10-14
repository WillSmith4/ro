# iPhone14,8 sbx1sbx1 crash during GPU→mediaplaybackd escape

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#305](https://github.com/rooootdev/lara/issues/305)  
> **State:** 🟢 open  
> **Created:** Jun 22, 2026  
> **Updated:** Sep 18, 2026  

---

## Description

*Opened by [@kennedydqz-del](https://github.com/kennedydqz-del)* *on Jun 22, 2026*


## iPhone14,8 (14 Plus) + iOS 18.6.2 — Heartbeat Trace Results

**Device:** iPhone 14 Plus (iPhone14,8, A15) — iOS 18.6.2 (Build 22G100)

**Tested 15+ times with 20obb/darksword-Exploit web version + custom print() heartbeat chain:**

```
✅ SBX1_STARTED      — sbx1_main.js eval executed successfully
✅ SBX1_SPAWN_READY  — function definitions completed
✅ SBX1_PIPELINE_START — main pipeline sbx1sbx1() started
💀 Crash              — sbx1sbx1() crashed internally, PIPELINE_RESULT never appeared
❌ PE_INJECTED_OK     — pe_main was never injected into mediaplaybackd
❌ 0 POST data packets
```

**Conclusion:** The second sandbox escape sbx1sbx1() (GPU→mediaplaybackd, CVE-2025-43510 COW bug) crashes on iPhone14,8. RCE and first sandbox escape (sbx0) pass consistently.

**Question:** iPhone14,8_22G100 offsets are complete in the code, but sbx1sbx1() fails somewhere internally. Has anyone encountered this or have suggestions?


---

## Timeline (Verbose)

- Jun 22, 2026: ✏️ @kennedydqz-del renamed: "iPhone14,8 (14 Plus) + iOS 18.6.2 — VFS race condition never triggers despite full chain loading" → "iPhone14,8 sbx1sbx1 crash during GPU→mediaplaybackd escape"


---

## Comments (2)

### Comment #1 — [@kennedydqz-del](https://github.com/kennedydqz-del) · Jun 22, 2026

`ID: 4765012638`


Update: Full chain now runs on iPhone14,8 / 18.6.2:
- sbx1 PIPELINE_RESULT: true ✅ (HB1→HB3 all passed)
- pe_main PE_INJECTED_OK ✅

Kernel VFS race (CVE-2025-43520) still doesn't win on A15 even with adjusted timing. Anyone else seen this on iPhone 14 series specifically?


---

### Comment #2 — [@maxblox2999](https://github.com/maxblox2999) · Sep 18, 2026

`ID: 5735746805`


do you know how to even write without ai slopping it all


❤️ 1


---

*Exported on 10/14/2026, 3:11:55 PM from [https://github.com/rooootdev/lara/issues/305](https://github.com/rooootdev/lara/issues/305)*
