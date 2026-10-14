# [BUG] 5 Dock Issue

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#138](https://github.com/rooootdev/lara/issues/138)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@xsxs18-dev](https://github.com/xsxs18-dev)  
> **Labels:** `bug`, `sufficient info`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 20, 2026  
> **Updated:** Apr 20, 2026  
> **Closed:** Apr 20, 2026  

---

## Description

*Opened by [@xsxs18-dev](https://github.com/xsxs18-dev)* *on Apr 20, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
``[When I click on apply 5 Dock and go to the homescreen and take the 5th app into my dock my phone resprings]``

### To Reproduce:
Steps to reproduce the behavior:
1. Go to 'INIT REMOTECALL'
2. Tap 'Apply 5 Dock'
3. Go to home screen 'Respring'
4. See error

### Expected behavior:
<!-- What you expected to happen. -->
``[The SpringBoard restarted]``

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

### Device Info:
- **Device:** ``[iPhone XR]`` <!-- -->
- **Chip:** ``[A12]`` <!-- e.g. -->
- **iOS Version:** ``[iOS 18.4]`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** ``[No]`` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``[1.2 for 8h]`` <!-- e.g. v0.3 or commit hash -->

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. --> lara started: 2026-04-20 07:40:56

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0xb8

(utils) PROC_PID_OFFSET: 0x60

(offs) t1sz_boot: 0x19

initialized offsets

(offs) t1sz_boot: 0x19

initialized offsets

(persistance) recovered control_socket: 9

(persistance) recovered rw_socket: 10

(persistance) kread failed

(ds) starting darksword
(ds) setsockopt failed (set_target_kaddr)!
(ds) device: iPhone11,8
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0xb
(ds) write_fd: 0xc
(ds) executable_path: /private/var/containers/Bundle/Application/00FD66D6-B655-433C-82C9-D00B9059DCF6/App.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/00FD66D6-B655-433C-82C9-D00B9059DCF6/App.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name: lara
(ds) kernel_process_name: lara
(ds) livecontainer_bundle: no
(ds) livecontainer_guest: no
(ds) rehosted_process: no
(ds) process_marker[0]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x15f414000
(ds) physical_mapping_address: 0x10e580000
(ds) pc_object: 0x8c03
(ds) pc_address: 0x354544000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x84010
(ds) end_pcb_id: 0x8f00e
(ds) looking in search mapping: 0
(ds) Matched PCB via process marker: lara
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x8dd5e
(ds) inp_list_next_pointer: 0xffffffdf01448400
(ds) icmp6filter: 0xffffffe1ff0aca20
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffdf01448548
(ds) found control_socket at idx: 0x4ea7

(utils) socket fallback: pcb=0xffffffdf01448400 pid=1431

(utils) socket found via pcb+0x28 = 0xfffffff02a7d21d8 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffdf01448400 sock=0xfffffff02a7d21d8)

(utils) socket fallback: pcb=0xffffffdf01448400 pid=1431

(utils) socket found via pcb+0x28 = 0xfffffff02a7d21d8 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffdf01448400 sock=0xfffffff02a7d21d8)

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:516] process: launchd, pid: 1

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:522] firstExceptionPort: 0x890b, secondExceptionPort: 0x591307
(ds) highest_success_idx: 483
(ds) success_read_count: 44
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffdf01448000
(ds) pcbinfo_pointer: 0xfffffff02a7dfd50
(ds) ipi_zone: 0xfffffff027f9f570
(ds) zv_name: 0xfffffff0275c54c0
(ds) searching for kernel Mach-O header from 0xfffffff0275c4000...
(ds) candidate Mach-O at 0xfffffff027558000: filetype=2 cpuinfo=0x2c0000002 (iter=27)
(ds) candidate Mach-O at 0xfffffff027550000: filetype=12 cpuinfo=0xc00000002 (iter=29)
(ds) found MH_FILESET header at 0xfffffff027550000
(ds) kernel_base:  0xfffffff027550000
(ds) kernel_slide: 0x2054c000
(ds) iOS 18: using so_count offset 0x254
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffdfe9cb56f0
(ds) our_task: 0xffffffdfe9cb5e30

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:116] [iter 0] kstack=0xffffffe544b07f30 kernelSP=0xffffffe544b07890

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:133] [iter 0] Found TRO at offset=0x39f0

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:144] [iter 0] checkVal=0x1002 checkVal2=0xffffffdd3385b760 (expecting 0x1002)

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:159] [iter 0] TRO swap: writing target tro=0xffffffdce6bd8220 to addr=0xffffffe544b079f0

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:163] TRO swap SUCCESS!

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:175] set_exception_port_on_thread returning success=1

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:616] Inject EXC_GUARD on thread:0xffffffdf001a28a8 OK

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:116] [iter 0] kstack=0xffffffe544f1bf30 kernelSP=0xffffffe544f1b890

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:133] [iter 0] Found TRO at offset=0x39f0

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:144] [iter 0] checkVal=0x1002 checkVal2=0xffffffdd3385b800 (expecting 0x1002)

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:159] [iter 0] TRO swap: writing target tro=0xffffffdce6cac278 to addr=0xffffffe544f1b9f0

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:163] TRO swap SUCCESS!

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:175] set_exception_port_on_thread returning success=1

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:616] Inject EXC_GUARD on thread:0xffffffdefef94dc8 OK

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:647] Valid threads: 2

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:648] Injected threads: 2

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:668] Finish clearing EXC_GUARD from all other threads...

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:677] trojanMemTemp: 0x16b8be950

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:681] calling sign_state for FAKE_PC_TROJAN_CREATOR...

[find_pacia_gadget:101] found pacia gadget, gadget addr = 0x19fae5b30

[remote_pac:142] remote_pac: addr=0x101 mod=0x7481000015000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0xc04a520000000101

[remote_pac:142] remote_pac: addr=0x201 mod=0x77d3000015000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x7222818000000201

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:685] sign_state done, pc=0xc04a520000000101 lr=0x7222818000000201 flags=0x15000000

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:688] reply_with_state done, waiting for crash...

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:692] vmMap: 0xffffffdc00541820

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:695] signing FAKE_PC_TROJAN via remote_pac...

[remote_pac:142] remote_pac: addr=0x301 mod=0x0 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x7d4ea38000000301

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:698] remoteCrashSigned: 0x7d4ea38000000301

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for getpid: 0x1f1d58f9c

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x1f1d58f9c mod=0x74810000aa000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0xf9223701f1d58f9c

[remote_pac:142] remote_pac: addr=0x201 mod=0x77d30000aa000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x111fd78000000201

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] getpid func's retValue = 0x1(1)

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for pthread_create_suspended_np: 0x22b6c1bcc

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x22b6c1bcc mod=0x74810000cd000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x432c5e022b6c1bcc

[remote_pac:142] remote_pac: addr=0x201 mod=0x77d30000cd000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x6e596e0000000201

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] pthread_create_suspended_np func's retValue = 0x0(0)

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:703] trojanMemTemp: 0x16b8be950

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:705] pthreadAddr: 0x16bc93000

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for pthread_mach_thread_np: 0x22b6be200

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x22b6be200 mod=0x7481000058000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x5d400f022b6be200

[remote_pac:142] remote_pac: addr=0x201 mod=0x77d3000058000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x820b3a0000000201

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] pthread_mach_thread_np func's retValue = 0xaf733(718643)

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:707] callThreadPort: 0xaf733

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:116] [iter 0] kstack=0xffffffe544ea3f30 kernelSP=0xffffffe544ea3890
exploit success!
kernel_base:  0xfffffff027550000
kernel_slide: 0x2054c000


exploit success!
kernel_base:  0xfffffff027550000
kernel_slide: 0x2054c000


[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:133] [iter 0] Found TRO at offset=0x39f0

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:144] [iter 0] checkVal=0x1002 checkVal2=0xffffffdd3385b800 (expecting 0x1002)

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:159] [iter 0] TRO swap: writing target tro=0xffffffdce6bce168 to addr=0xffffffe544ea39f0

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:163] TRO swap SUCCESS!

[-[RemoteCall setExceptionPortOnThread:forThread:useMigFilterBypass:]:175] set_exception_port_on_thread returning success=1

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:731] All good! Resuming trojan thread...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for thread_resume: 0x1f1d550b0

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x1f1d550b0 mod=0x748100006e000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x4b780681f1d550b0

[remote_pac:142] remote_pac: addr=0x201 mod=0x77d300006e000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0xf60e1e0000000201

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] thread_resume func's retValue = 0x0(0)

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:740] New thread created, resuming original

[remote_pac:142] remote_pac: addr=0xed6941022b6ba9f0 mod=0x74810000d5000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x664661022b6ba9f0

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:744] Original thread restored

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for getpid: 0x1f1d58f9c

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x1f1d58f9c mod=0x748100000d000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0xd923cf81f1d58f9c

[remote_pac:142] remote_pac: addr=0x401 mod=0x77d300000d000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x57b00000000401

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] getpid func's retValue = 0x1(1)

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:747] Task pid: 1

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for mmap: 0x1f1d59bcc

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x1f1d59bcc mod=0x74810000a1000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0xfa3e2f81f1d59bcc

[remote_pac:142] remote_pac: addr=0x401 mod=0x77d30000a1000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x54196c0000000401

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] mmap func's retValue = 0xc6c010000(53351612416)

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for memset: 0x22b60ffc0

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x22b60ffc0 mod=0x748100008b000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0xfc1951022b60ffc0

[remote_pac:142] remote_pac: addr=0x401 mod=0x77d300008b000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x5f5ade8000000401

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] memset func's retValue = 0xc6c010000(53351612416)

[-[RemoteCall initRemoteCallForProcess:useMigFilterBypass:]:754] Finished successfully

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for sandbox_extension_issue_mach: 0x22b602374

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x22b602374 mod=0x7481000057000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x4f1739822b602374

[remote_pac:142] remote_pac: addr=0x401 mod=0x77d3000057000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x8f3dd90000000401

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] sandbox_extension_issue_mach func's retValue = 0x619c68000(26202243072)

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for strcpy: 0x22b6103f8

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x22b6103f8 mod=0x74810000ad000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0xd357f5822b6103f8

[remote_pac:142] remote_pac: addr=0x401 mod=0x77d30000ad000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x9279c30000000401

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] strcpy func's retValue = 0xc6c010200(53351612928)

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for strlen: 0x22b610160

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x22b610160 mod=0x74810000f4000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0xe16c82022b610160

[remote_pac:142] remote_pac: addr=0x401 mod=0x77d30000f4000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x6532698000000401

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] strlen func's retValue = 0xc1(193)

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for sandbox_extension_issue_mach: 0x22b602374

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x22b602374 mod=0x7481000057000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x4f1739822b602374

[remote_pac:142] remote_pac: addr=0x401 mod=0x77d3000057000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x8f3dd90000000401

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] sandbox_extension_issue_mach func's retValue = 0x619c68800(26202245120)

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for strcpy: 0x22b6103f8

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x22b6103f8 mod=0x748100006f000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0xee48ab022b6103f8

[remote_pac:142] remote_pac: addr=0x401 mod=0x77d300006f000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0xe50a740000000401

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] strcpy func's retValue = 0xc6c010200(53351612928)

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for strlen: 0x22b610160

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x22b610160 mod=0x748100005c000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x63631f022b610160

[remote_pac:142] remote_pac: addr=0x401 mod=0x77d300005c000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x8807908000000401

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] strlen func's retValue = 0xbc(188)

(persistance) bootstrap_register failed for krw.darksword.control_port: unknown error code

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for munmap: 0x1f1d5a180

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x1f1d5a180 mod=0x7481000027000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x886e7d01f1d5a180

[remote_pac:142] remote_pac: addr=0x401 mod=0x77d3000027000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0xed750c0000000401

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:291] munmap func's retValue = 0x0(0)

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:252] pcAddr for pthread_exit: 0x22b6ba960

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:257] Waiting for exception...

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:264] Exception received, setting up args and replying

[remote_pac:142] remote_pac: addr=0x22b6ba960 mod=0x74810000d7000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0xd31e4822b6ba960

[remote_pac:142] remote_pac: addr=0x401 mod=0x77d30000d7000000 gadget=0x19fae5b30

[remote_pac:205] PAC thread state set, swapping keys and resuming

[remote_pac:228] remote_pac result: 0x4b40fe0000000401

[-[RemoteCall doRemoteCallInternalTimeout:exceptionPort:lrMarker:functionName:functionPointer:args:argCount:]:280] Trojan thread cleanup 
```
(I searched in the analysis data and in the lara app but there are no logs...)
```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``[Cowabunga lite, SparseBoxPlus and EnsWilde]``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- Apr 20, 2026: 👤 @xsxs18-dev assigned @Pro42good
- Apr 20, 2026: 🏷️ @xsxs18-dev added label `bug`
- Apr 20, 2026: 🔴 Closed by @xsxs18-dev
- Apr 20, 2026: 🏷️ @Pro42good added label `sufficient info`


---

## Comments (1)

### Comment #1 — [@KilIswitch](https://github.com/KilIswitch) · Apr 20, 2026

`ID: 4278571814`


For me it works just fine but only if I leave Lara open....if I close lara from app switcher it resprings. (16 pro max, 18.3)


---

*Exported on 10/14/2026, 2:57:30 PM from [https://github.com/rooootdev/lara/issues/138](https://github.com/rooootdev/lara/issues/138)*
