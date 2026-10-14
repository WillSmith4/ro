# [Feature] disable recording message voice

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#293](https://github.com/rooootdev/lara/issues/293)  
> **State:** 🟢 open  
> **Labels:** `enhancement`  
> **Created:** Jun 8, 2026  
> **Updated:** Jun 17, 2026  

---

## Description

*Opened by [@baghdadhost-wq](https://github.com/baghdadhost-wq)* *on Jun 8, 2026*


### Is your feature request related to a problem?
<!-- A clear and concise description of what the problem is. e.g. "It's frustrating when..." -->
It's frustrating when the system plays an audible voice message/greeting to announce that a call is being recorded. Furthermore, manually replacing the audio files with empty ones is only a temporary fix, as the system restores the original audio files and the sound returns after restarting the device.

### Describe the solution you'd like:

### Is your feature request related to a problem?

I would like a feature (perhaps a toggle in the settings) that persistently disables the call recording announcement. The tweak should automatically inject or maintain empty/blank audio files in the following directory: `Var/mobile/library/callservices/greetings/default/***.m4a and ***.caf`
Crucially, the tweak needs to ensure this patch 

It's frustrating when the system plays an audible voice message/greeting to announce that a call is being recorded. Furthermore, manually replacing the audio files with empty ones is only a temporary fix, as the system restores the original audio files and the sound returns after restarting the device.

### Describe the solution:
<!-- What you want to happen. Be specific about the behavior, UI, or capability. -->
I would like a feature (perhaps a toggle in the Lara settings ) that inject the empty files in following directory: `Var/mobile/library/callservices/greetings/default/***.m4a and ***.caf`  to disables the call recording announcement without copy and paste every time the phone is rebooted .

### Describe alternatives you've considered:
<!-- Other approaches, workarounds, or existing tools that partially solve this. -->
The current workaround is manually navigating to the directory using the built-in file browser within the Lara application and replacing the original audio files with empty ones. However, this is tedious and fails after a device reboot, requiring the user to repeat the process every single time.

### Scope:
- [x] UI / UX change
- [ ] New exploit capability or device support
- [x] Tweak / package management
- [x] Persistence or boot behavior
- [ ] Logging, diagnostics, or error handling
- [ ] Build system / CI
- [ ] Documentation
- [x] Other (File system modification)


### Scope:
<!-- Check all that apply. -->
- [x] UI / UX change
- [ ] New exploit capability or device support
- [x] Tweak / package management
- [x] Persistence or boot behavior
- [ ] Logging, diagnostics, or error handling
- [ ] Build system / CI
- [ ] Documentation
- [x] Other (File system modification)

### Additional context:
<!-- Mockups, references to similar features in other jailbreaks, related issues, links to research. -->


### Are you willing to help implement this?
- [ ] Yes, I can submit a PR
- [x] I can help test
- [x] I'm just suggesting the idea

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Requests missing these may be closed. -->
- [x] I have searched existing issues (open and closed) and confirmed this feature has not already been requested
- [x] This request is relevant to the latest version / commit of Lara
- [x] I have provided sufficient detail (problem, proposed solution, scope) for a maintainer to evaluate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support

```


---

## Timeline (Verbose)

- Jun 8, 2026: 🏷️ @baghdadhost-wq added label `enhancement`


---

## Comments (2)

### Comment #1 — [@notthemystery](https://github.com/notthemystery) · Jun 17, 2026

`ID: 4731707237`


I added this. It’s in my fork of lara


---

### Comment #2 — [@baghdadhost-wq](https://github.com/baghdadhost-wq) · Jun 17, 2026

`ID: 4732391274`


> I added this. It’s in my fork of lara

Thanks I tested, and it’s working 
But status showing enabled all the time


---

*Exported on 10/14/2026, 3:11:03 PM from [https://github.com/rooootdev/lara/issues/293](https://github.com/rooootdev/lara/issues/293)*
