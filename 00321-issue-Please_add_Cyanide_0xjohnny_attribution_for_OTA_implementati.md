# Please add Cyanide / 0xjohnny attribution for OTA implementation

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#321](https://github.com/rooootdev/lara/issues/321)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Aug 27, 2026  
> **Updated:** Sep 25, 2026  
> **Closed:** Sep 25, 2026  

---

## Description

*Opened by [@0xjohnnydev](https://github.com/0xjohnnydev)* *on Aug 27, 2026*


Hi, I am 0xjohnny. My GitHub username is @0xjohnnydev, and I previously used zeroxjf.

Lara's `lara/kexploit/ota.m` contains substantial implementation overlap with Cyanide's OTA implementation. Cyanide had the related implementation by May 8, 2026, while Lara added its similar file on May 29, 2026.

Evidence:

- Cyanide implementation and history: https://github.com/0xjohnnydev/cyanide/commit/61d07ebaaa3e770ab297d24d4c1826ea9c8470b2
- Lara addition: https://github.com/rooootdev/lara/commit/1f0c85aff83d01702ec15bdcf9adc0ee3042165f
- Cyanide current source: https://github.com/0xjohnnydev/cyanide/blob/852ef9e87ec4d6db3f4dd6db1e19d40202885a23/Cyanide/tweaks/darksword_ota.m

After comments and blank lines are removed and whitespace is normalized, the initial Cyanide and Lara snapshots contain 122 matching lines. That covers about 40% of Cyanide's file and 44% of Lara's file.

The current Lara source and acknowledgements do not credit Cyanide or me. Please add a clear provenance notice near the top of `lara/kexploit/ota.m` and in the README or credits. Please retain it in future source and binary releases.

Suggested notice:

> Portions of the OTA implementation are adapted from Cyanide by 0xjohnny (GitHub @0xjohnnydev, formerly zeroxjf), first published in May 2026 under AGPL-3.0.

This is a request to correct attribution and AGPL license-notice provenance. It is not a takedown request.


---

## Timeline (Verbose)

- Sep 25, 2026: 🔴 Closed by @rooootdev


---

## Comments (1)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Sep 25, 2026

`ID: 5839784067`


Sorry about that, the feature was a pull request I simply merged. Credits added.


---

*Exported on 10/14/2026, 3:13:08 PM from [https://github.com/rooootdev/lara/issues/321](https://github.com/rooootdev/lara/issues/321)*
