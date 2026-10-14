# [BUG] Lara app crash on Initialise VFS

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#206](https://github.com/rooootdev/lara/issues/206)  
> **State:** 🟢 open  
> **Labels:** `bug`, `sufficient info`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 27, 2026  
> **Updated:** May 23, 2026  

---

## Description

*Opened by [@theXappy](https://github.com/theXappy)* *on Apr 27, 2026*


### Describe the bug:
``Current version of Lara crashes when "Initialise VFS" is clicked on my device.``

### To Reproduce:
Steps to reproduce the behavior:
1. Switch to `Hybrid` in settings
2. Click Run Exploit (KRW)
3. Click Sandbox Escape
4. Click Initialise VFS --> App immediately crashes. Device is back at springboard.

### Expected behavior:
```App shouldn't crash and FS should be berowsable```

### Screenshots:
- none -

### Device Info:
- **Device:** ``iPhone 16e``
- **Chip:** ``A18``
- **iOS Version:** ``26.0.1 (23A355) ``
- **Jailbroken before?** ``Yes, with Lara IPA over the last 4 days``
- **Lara version / commit:** ``5d442e9b467766327bf25f7613fa4d34f729a4ea (Latest commit on main as of writing)``

### Logs:
This issue happened to me for both with or without LiveContainer.
But the call stack below is from the LiveContainer run, because I can't get the call stack otherwise.
```
set_target_kaddr got invalid kaddr 0xffffec5e1c3fadc0
Call stack: (
	0   CoreFoundation                      0x000000019bb968d4 973E47E5-88F0-3367-B9D4-46D5CB999B75 + 825556
	1   libobjc.A.dylib                     0x0000000198b0d7c4 objc_exception_throw + 88
	2   lara                                0x0000000103f88f00 set_target_kaddr + 200
	3   lara                                0x0000000103f8509c early_kread + 80
	4   lara                                0x0000000103f85004 early_kread64 + 40
	5   lara                                0x0000000103f84fd0 ds_kread64 + 28
	6   lara                                0x0000000103f7a328 getrootvnode + 124
	7   lara                                0x0000000103f79de4 vfs_init + 336
	8   lara                                0x0000000103fab4c4 $s4lara7laramgrC7vfsinit10completionyySbcSg_tFyyYbcfU0_ + 304
	9   lara                                0x0000000103fa9e50 $sIegh_IeyBh_TR + 56
	10  libdispatch.dylib                   0x00000001d34a2adc D34AC6A9-10DC-3535-9944-E3EFC5994788 + 6876
	11  libdispatch.dylib                   0x00000001d34bc7ec D34AC6A9-10DC-3535-9944-E3EFC5994788 + 112620
	12  libdispatch.dylib                   0x00000001d34d942c D34AC6A9-10DC-3535-9944-E3EFC5994788 + 230444
	13  libdispatch.dylib                   0x00000001d34b50bc D34AC6A9-10DC-3535-9944-E3EFC5994788 + 82108
	14  libdispatch.dylib                   0x00000001d34b56fc D34AC6A9-10DC-3535-9944-E3EFC5994788 + 83708
	15  libsystem_pthread.dylib             0x00000001f691b37c _pthread_wqthread + 232
	16  libsystem_pthread.dylib             0x00000001f691a8c0 start_wqthread + 8
)
```

### Additional context:
I think this was broken TODAY.
I used Lara over the last weekend, with an IPA built form this commit:
`` 138a9fcdd9a07240ba2684fe6ab7fc61cf6d4e84 ``
And it did not crash on initalise VFS & I could browser the filesystem.
Seems like something broke very recently.

I also made sure to clear the kernelcache info and re-downloaded/process it. Still crashes.

### Pre-submission checklist:
- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support


---

## Timeline (Verbose)

- Apr 27, 2026: 👤 @theXappy assigned @Pro42good
- Apr 27, 2026: 🏷️ @theXappy added label `bug`
- May 4, 2026: 🏷️ @Pro42good added label `sufficient info`


---

## Comments (4)

### Comment #1 — [@theXappy](https://github.com/theXappy) · Apr 27, 2026

`ID: 4330354648`


Forgot to mention, I tried this solution 
https://github.com/rooootdev/lara/issues/157#issuecomment-4288315534
To change `T1SZ_BOOT` to `0x19`
And it made things worse - with it "Esacpe Sandbox" click reboots the device itself.


---

### Comment #2 — [@Pro42good](https://github.com/Pro42good) · May 4, 2026

`ID: 4373263149`


Hello, This is a copy paste response, Im just trying to close issues.

But could you try on the latest official version and or on the latest nightly version of lara?

Please let me know if its fixed it's self or you can close the issue.


---

### Comment #3 — [@theXappy](https://github.com/theXappy) · May 5, 2026

`ID: 4375049127`


@Pro42good i usted tested v0.1
Yes, it still crashed
"Run exploit" finished ok, but "Initialise System" crashes immediately after press


---

### Comment #4 — [@tryh4rd3rr](https://github.com/tryh4rd3rr) · May 23, 2026

`ID: 4524001682`


still happens in v0.2, same exact device & version


---

*Exported on 10/14/2026, 3:03:37 PM from [https://github.com/rooootdev/lara/issues/206](https://github.com/rooootdev/lara/issues/206)*
