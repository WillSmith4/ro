# [Feature] Read Keychain of specific App (Any App RemoteCall)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#309](https://github.com/rooootdev/lara/issues/309)  
> **State:** 🟢 open  
> **Labels:** `enhancement`  
> **Created:** Jul 4, 2026  
> **Updated:** Jul 9, 2026  

---

## Description

*Opened by [@DYY-Studio](https://github.com/DYY-Studio)* *on Jul 4, 2026*


### Is your feature request related to a problem?
I thought that the mechanism of DarkSword is fully capable of achieving RemoteCall for any App, but the pipeline is not yet mature and there are many detailed issues.

Through vibe coding, I performed extensive testing and explored a basically stable pipeline for executing RemoteCall on any App under Arm64e, using a physical iPhone SE (3rd Gen) running iOS 17.7.2. During this process, I also discovered some issues with the current pipeline.

I am opening this issue because I do not have a deep understanding of the hardware mechanisms, cannot properly explain my findings and make sure they are right; additionally, the highly experimental pipeline and messy code produced from vibe coding are not suitable for submitting a PR.

Keychain Read is what I consider a relatively interesting application of this approach, and it is universally applicable to most Apps. I've tested that this is fully possible.

> <img width="50%" alt="Image" src="https://github.com/user-attachments/assets/5c295d7d-3d5d-4003-9bb2-2ef38f9bda9b" />
>
> Here shows the Keychain Read Experiment Results. Target: BOOK WALKER (Japan Store)
>
> This experiment only fetch the `description` of the returned dictionary

You can see the whole experiment route in this branch, although the branch name is wrong and the code is really messy, also included some unneeded functions such as dynamic offset resolve and multi-thread inject:
https://github.com/DYY-Studio/lara/tree/leaf-vm-scan

Key points are listed in Addtional context.

Hope this helpful.

### Describe the solution you'd like:
New Tweak **Keychain Read**
* Provide an App List for users to choose from
* Execute RemoteCall on the target App to read Keychain
  * Remotely check if `Security.framework` is available (via `dlsym`, `dlopen`, or `dyld_image` scanning)
  * (remote dlsym `SecItemCopyMatching` and then use it to query)
  * For available Apps, execute `SecItemCopyMatching` to list all entries under the five classes
  * Allow selecting specific entries to read, or directly fetch the values together with `kSecReturnData` when executing `SecItemCopyMatching`
### Describe alternatives you've considered:
NONE
### Scope:
<!-- Check all that apply. -->
- [x] UI / UX change
- [x] New exploit capability or device support
- [x] Tweak / package management
- [ ] Persistence or boot behavior
- [ ] Logging, diagnostics, or error handling
- [ ] Build system / CI
- [ ] Documentation
- [ ] Other
### Additional context:
In my experimental vibe-coded pipeline, I found that for any arbitrary App, the following points generally need attention:
* PAC Signing Strategy for PC / LR
   - The current pipeline signs PC and LR on Arm64e.
   - The current pipeline do not check / clear `__DARWIN_ARM_THREAD_STATE64_FLAGS_NO_PTRAUTH` flag.
   - In my testing, I found that both PC and LR are bare pointers without PAC during takeover.
     - The apps I have tested all has `__DARWIN_ARM_THREAD_STATE64_FLAGS_NO_PTRAUTH` flag.
     - If the contents placed into PC and LR are signed with PAC at this moment, it instead triggers a `CODESIGNING 2` error, resulting in a `SIGKILL`.
     - Signed PC also make `pthread_create_suspended_np` failed with non-zero return code.
  
<del>

* Trap Exception Triggerin
   - The FAKE_LR `0x401` / `0x201` in the current strategy triggers `CODESIGNING 2` (`SIGKILL`) on typical Apps.
   - Through experimentation, I found that the following two approaches are feasible:
      1. Misalign
         * Offset a pointer of a function from the `dyld_shared_cache` (such as `getpid`) by `0x2` or `0x6`.
         * This operation triggers `EXC_BAD_ACCESS (SIGBUS)`, which can be reliably triggered regardless of whether PAC is valid.
         * This aligns with the current `EXC_BAD_ACCESS` handling strategy and does not kill the process.
      2. ObjCTrapBRK
         * For apps with an ObjC runtime, use `objc_exception_throw` to raise a `BRK`.
         * However, this direction does not align with the current strategy and is only mentioned in passing.
         
</del>
       
Maybe more but I cannot remember.
### Are you willing to help implement this?
- [x] Yes, I can submit a PR
- [x] I can help test
- [ ] I'm just suggesting the idea

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Requests missing these may be closed. -->
- [x] I have searched existing issues (open and closed) and confirmed this feature has not already been requested
- [x] This request is relevant to the latest version / commit of Lara
- [x] I have provided sufficient detail (problem, proposed solution, scope) for a maintainer to evaluate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- Jul 4, 2026: 🏷️ @DYY-Studio added label `enhancement`


---

*Exported on 10/14/2026, 3:12:13 PM from [https://github.com/rooootdev/lara/issues/309](https://github.com/rooootdev/lara/issues/309)*
