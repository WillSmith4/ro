# [BUG] App crashes

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#304](https://github.com/rooootdev/lara/issues/304)  
> **State:** 🟢 open  
> **Labels:** `bug`  
> **Created:** Jun 21, 2026  

---

## Description

*Opened by [@DevaloperHH](https://github.com/DevaloperHH)* *on Jun 21, 2026*


### Describe the issue

Lara works correctly when installed using:

- Developer certificate
- Developer Mode enabled

However, when the same IPA is signed with an Enterprise Distribution certificate and installed successfully, the application immediately crashes on launch.

### Environment

- Device: iPhone X
- iOS: 18.6.2
- Lara version: last 

### Reproduction

1. Build/download official IPA
2. Sign with Enterprise Distribution certificate
3. Install successfully
4. Launch app
5. Immediate crash

### Additional information

The same IPA works when signed with a development certificate.


---

## Timeline (Verbose)

- Jun 21, 2026: 🏷️ @DevaloperHH added label `bug`


---

*Exported on 10/14/2026, 3:11:51 PM from [https://github.com/rooootdev/lara/issues/304](https://github.com/rooootdev/lara/issues/304)*
