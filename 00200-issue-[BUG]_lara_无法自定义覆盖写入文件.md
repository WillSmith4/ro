# [BUG] lara 无法自定义覆盖写入文件

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#200](https://github.com/rooootdev/lara/issues/200)  
> **State:** 🟢 open  
> **Labels:** `bug`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 27, 2026  
> **Updated:** Apr 27, 2026  

---

## Description

*Opened by [@TequilaDash](https://github.com/TequilaDash)* *on Apr 27, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
``[自定义路径覆盖功能无法对/System/Library/PrivateFrameworks/MaterialKit.framework/Assets.car 该路径的文件进行写入，显示失败]``

### To Reproduce:
Steps to reproduce the behavior:
1. Go to '...'
2. Tap '...'
3. Scroll down to '...'
4. See error

### Expected behavior:
<!-- What you expected to happen. -->
``[不清楚什么原因，希望修复一下这个问题]``

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

### Device Info:
- **Device:** ``[iPhone XS]`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
- **Chip:** ``[A12]`` <!-- e.g. A15, A14, M1 -->
- **iOS Version:** ``[iOS 18.0 (22A3354)]`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** ``[否]`` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``[1.2]`` <!-- e.g. v0.3 or commit hash -->

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
```

<img width="1125" height="2436" alt="Image" src="https://github.com/user-attachments/assets/fac3cf90-c44a-46ae-a759-a74b85970a57" />

lara started: 2026-04-27 09:57:31

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0xb8

(utils) PROC_PID_OFFSET: 0x28

(offs) loaded saved t1sz_boot: 0x19

(offs) t1sz_boot: 0x19

(offs) saved t1sz_boot: 0x19

initialized offsets

(offs) loaded saved t1sz_boot: 0x19

(offs) t1sz_boot: 0x19

(offs) saved t1sz_boot: 0x19

initialized offsets

(persist) No stashed primitive found

(ds) starting darksword
(ds) recovering primitives from launchd...
(ds) device: iPhone11,2
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0xa
(ds) write_fd: 0xb
(ds) executable_path: /private/var/containers/Bundle/Application/A1FD3CFB-A036-46EC-9070-06FA2CEF2E40/lara.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/A1FD3CFB-A036-46EC-9070-06FA2CEF2E40/lara.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name: lara
(ds) kernel_process_name: lara
(ds) livecontainer_bundle: no
(ds) livecontainer_guest: no
(ds) rehosted_process: no
(ds) process_marker[0]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x10e934000
(ds) physical_mapping_address: 0x11b124000
(ds) pc_object: 0xa703
(ds) pc_address: 0x3f7efc000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x16004
(ds) end_pcb_id: 0x21002
(ds) looking in search mapping: 0
(ds) Matched PCB via process marker: lara
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x20762
(ds) inp_list_next_pointer: 0xffffffdf38670400
(ds) icmp6filter: 0xffffffe1f1981380
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffdf38670548
(ds) found control_socket at idx: 0x53af

(utils) socket fallback: pcb=0xffffffdf38670400 pid=523

(utils) socket found via pcb+0x28 = 0xfffffff01a65a220 (backptr at +0x0)
(ds) highest_success_idx: 282
(ds) success_read_count: 17
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffdf38670000
(ds) pcbinfo_pointer: 0xfffffff01a664940
(ds) ipi_zone: 0xfffffff017de1d10
(ds) zv_name: 0xfffffff0174574ee
(ds) searching for kernel Mach-O header from 0xfffffff017454000...
(ds) candidate Mach-O at 0xfffffff0173ec000: filetype=2 cpuinfo=0x2c0000002 (iter=26)
(ds) candidate Mach-O at 0xfffffff0173e4000: filetype=12 cpuinfo=0xc00000002 (iter=28)
(ds) found MH_FILESET header at 0xfffffff0173e4000
(ds) kernel_base:  0xfffffff0173e4000
(ds) kernel_slide: 0x103e0000
(ds) iOS 18: using so_count offset 0x254

(utils) socket scan found nothing (pcb=0xffffffdf38670400 sock=0xfffffff01a65a220)

(utils) socket fallback: pcb=0xffffffdf38670400 pid=523

(utils) socket found via pcb+0x28 = 0xfffffff01a65a220 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffdf38670400 sock=0xfffffff01a65a220)
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffdf38356ed0
(ds) our_task: 0xffffffdf38357610
(ds) transferring primitives to launchd...

(rc) process: launchd, pid: 1

(rc) firstExceptionPort: 0xa91b, secondExceptionPort: 0x56fb07

(rc) [iter 0] kstack=0xffffffef8cc4bf30 kernelSP=0xffffffef8cc4b860

(rc) [iter 0] Found TRO at offset=0x39c0

(rc) [iter 0] checkVal=0x1002 checkVal2=0xffffffde09a1cfe0 (expecting 0x1002)

(rc) [iter 0] TRO swap: writing target tro=0xffffffde09a87d98 to addr=0xffffffef8cc4b9c0

(rc) TRO swap SUCCESS!

(rc) set_exception_port_on_thread returning success=1

(rc) Inject EXC_GUARD on thread:0xffffffe109a390f8 OK

(rc) [iter 0] kstack=0xffffffef8ce43f30 kernelSP=0xffffffef8ce43860

(rc) [iter 0] Found TRO at offset=0x39c0

(rc) [iter 0] checkVal=0x1002 checkVal2=0xffffffde09a1d000 (expecting 0x1002)

(rc) [iter 0] TRO swap: writing target tro=0xffffffde09b50430 to addr=0xffffffef8ce439c0

(rc) TRO swap SUCCESS!

(rc) set_exception_port_on_thread returning success=1

(rc) Inject EXC_GUARD on thread:0xffffffe10a7d61f0 OK

(rc) Valid threads: 2

(rc) Injected threads: 2

(rc) Finish clearing EXC_GUARD from all other threads...

(rc) trojanMemTemp: 0x16db72f00

(rc) calling sign_state for FAKE_PC_TROJAN_CREATOR...

(pac) found pacia gadget, gadget addr = 0x19279075c

(pac) remote_pac: addr=0x101 mod=0x74810000d5000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x6544148000000101

(pac) remote_pac: addr=0x201 mod=0x77d30000d5000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x5b31028000000201

(rc) sign_state done, pc=0x6544148000000101 lr=0x5b31028000000201 flags=0xd5000000

(rc) reply_with_state done, waiting for crash...

(rc) vmMap: 0xffffffdd233b01e0

(rc) signing FAKE_PC_TROJAN via remote_pac...

(pac) remote_pac: addr=0x301 mod=0x0 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x785a7d0000000301

(rc) remoteCrashSigned: 0x785a7d0000000301

(rc) pcAddr for getpid: 0x1e3ca1b90

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x1e3ca1b90 mod=0x74810000d6000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x16021f81e3ca1b90

(pac) remote_pac: addr=0x201 mod=0x77d30000d6000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x8d14958000000201

(rc) getpid func's retValue = 0x1(1)

(rc) pcAddr for pthread_create_suspended_np: 0x21b1cc874

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b1cc874 mod=0x7481000001000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xf65215821b1cc874

(pac) remote_pac: addr=0x201 mod=0x77d3000001000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xcb43fc0000000201

(rc) pthread_create_suspended_np func's retValue = 0x0(0)

(rc) trojanMemTemp: 0x16db72f00

(rc) pthreadAddr: 0x16dbff000

(rc) pcAddr for pthread_mach_thread_np: 0x21b1c49c0

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b1c49c0 mod=0x7481000063000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xcc1aa6021b1c49c0

(pac) remote_pac: addr=0x201 mod=0x77d3000063000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x256f1c0000000201

(rc) pthread_mach_thread_np func's retValue = 0x863af(549807)

(rc) callThreadPort: 0x863af

(rc) [iter 0] kstack=0xffffffef8cd0bf30 kernelSP=0xffffffef8cd0b860

(rc) [iter 0] Found TRO at offset=0x39c0

(rc) [iter 0] checkVal=0x1002 checkVal2=0xffffffde09a1d000 (expecting 0x1002)

(rc) [iter 0] TRO swap: writing target tro=0xffffffde09a84010 to addr=0xffffffef8cd0b9c0

(rc) TRO swap SUCCESS!

(rc) set_exception_port_on_thread returning success=1

(rc) All good! Resuming trojan thread...

(rc) pcAddr for thread_resume: 0x1e3c9db20

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x1e3c9db20 mod=0x74810000db000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming
(ds) exploit success!
(ds) kernel_base:  0xfffffff0173e4000
(ds) kernel_slide: 0x103e0000

(pac) remote_pac result: 0x2310e381e3c9db20

(pac) remote_pac: addr=0x201 mod=0x77d30000db000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xcb08e20000000201

(rc) thread_resume func's retValue = 0x0(0)

(rc) New thread created, resuming original

(pac) remote_pac: addr=0x1435d0021b1c4480 mod=0x74810000a9000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x8f5d19021b1c4480

(rc) Original thread restored

(rc) pcAddr for getpid: 0x1e3ca1b90

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x1e3ca1b90 mod=0x74810000e7000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x5078ea01e3ca1b90

(pac) remote_pac: addr=0x401 mod=0x77d30000e7000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xbe78298000000401

(rc) getpid func's retValue = 0x1(1)

(rc) Task pid: 1

(rc) pcAddr for mmap: 0x1e3ca27c4

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x1e3ca27c4 mod=0x7481000079000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xc2567e01e3ca27c4

(pac) remote_pac: addr=0x401 mod=0x77d3000079000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xdd3ab98000000401

(rc) mmap func's retValue = 0x68e010000(28152233984)

(rc) pcAddr for memset: 0x21b116040

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b116040 mod=0x748100001c000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xe70bdc021b116040

(pac) remote_pac: addr=0x401 mod=0x77d300001c000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xad14de8000000401

(rc) memset func's retValue = 0x68e010000(28152233984)

(rc) Finished successfully

(rc) pcAddr for sandbox_extension_issue_mach: 0x21b1089cc

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b1089cc mod=0x748100000b000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x4a550f021b1089cc

(pac) remote_pac: addr=0x401 mod=0x77d300000b000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x6e3bc30000000401

(rc) sandbox_extension_issue_mach func's retValue = 0xda99de000(58680270848)

(rc) pcAddr for strcpy: 0x21b117760

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b117760 mod=0x7481000004000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xda32d1821b117760

(pac) remote_pac: addr=0x401 mod=0x77d3000004000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x6e404a8000000401

(rc) strcpy func's retValue = 0x68e010200(28152234496)

(rc) pcAddr for strlen: 0x21b117b40

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b117b40 mod=0x748100004a000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x92638e821b117b40

(pac) remote_pac: addr=0x401 mod=0x77d300004a000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x8325be0000000401

(rc) strlen func's retValue = 0xc1(193)

(rc) pcAddr for sandbox_extension_issue_mach: 0x21b1089cc

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b1089cc mod=0x74810000ea000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x4e2c67021b1089cc

(pac) remote_pac: addr=0x401 mod=0x77d30000ea000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xe652308000000401

(rc) sandbox_extension_issue_mach func's retValue = 0xda99de800(58680272896)

(rc) pcAddr for strcpy: 0x21b117760

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b117760 mod=0x74810000a0000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xb4113b821b117760

(pac) remote_pac: addr=0x401 mod=0x77d30000a0000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x5108cd0000000401

(rc) strcpy func's retValue = 0x68e010200(28152234496)

(rc) pcAddr for strlen: 0x21b117b40

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b117b40 mod=0x748100000b000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x613dac021b117b40

(pac) remote_pac: addr=0x401 mod=0x77d300000b000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x6e3bc30000000401

(rc) strlen func's retValue = 0xbc(188)

(rc) pcAddr for sandbox_extension_issue_mach: 0x21b1089cc

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b1089cc mod=0x74810000ff000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x3916aa821b1089cc

(pac) remote_pac: addr=0x401 mod=0x77d30000ff000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x5f7ead8000000401

(rc) sandbox_extension_issue_mach func's retValue = 0xda99df000(58680274944)

(rc) pcAddr for strcpy: 0x21b117760

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b117760 mod=0x74810000b1000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x435366021b117760

(pac) remote_pac: addr=0x401 mod=0x77d30000b1000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xaa70500000000401

(rc) strcpy func's retValue = 0x68e010200(28152234496)

(rc) pcAddr for strlen: 0x21b117b40

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b117b40 mod=0x748100005a000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xd7611f021b117b40

(pac) remote_pac: addr=0x401 mod=0x77d300005a000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xea511c0000000401

(rc) strlen func's retValue = 0xbf(191)

(rc) pcAddr for sandbox_extension_issue_mach: 0x21b1089cc

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b1089cc mod=0x74810000e1000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x62308e821b1089cc

(pac) remote_pac: addr=0x401 mod=0x77d30000e1000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xe53be30000000401

(rc) sandbox_extension_issue_mach func's retValue = 0xda99dd000(58680266752)

(rc) pcAddr for strcpy: 0x21b117760

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b117760 mod=0x7481000057000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xdf45e0021b117760

(pac) remote_pac: addr=0x401 mod=0x77d3000057000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x1c2a7b0000000401

(rc) strcpy func's retValue = 0x68e010200(28152234496)

(rc) pcAddr for strlen: 0x21b117b40

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b117b40 mod=0x74810000a4000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x776d49021b117b40

(pac) remote_pac: addr=0x401 mod=0x77d30000a4000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x597bf10000000401

(rc) strlen func's retValue = 0xba(186)

    KernelBase = 18446744005380030464;

    KernelSlide = 272498688;

    LaunchdProcAddr = 18446743932995589056;

(rc) pcAddr for munmap: 0x1e3ca2d80

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x1e3ca2d80 mod=0x7481000011000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xf9508b81e3ca2d80

(pac) remote_pac: addr=0x401 mod=0x77d3000011000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xc26f3e0000000401

(rc) munmap func's retValue = 0x0(0)

(rc) pcAddr for pthread_exit: 0x21b1cb1d8

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remote_pac: addr=0x21b1cb1d8 mod=0x748100000c000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0x7f1f2d021b1cb1d8

(pac) remote_pac: addr=0x401 mod=0x77d300000c000000 gadget=0x19279075c

(pac) PAC thread state set, swapping keys and resuming

(pac) remote_pac result: 0xa3126e8000000401

(rc) Trojan thread cleanup


(ds) exploit success!
(ds) kernel_base:  0xfffffff0173e4000
(ds) kernel_slide: 0x103e0000

(sbx) proc=0xffffffdf38356ed0 proc_ro_raw=0xffffffde566b6800 proc_ro=0xffffffde566b6800
(sbx) scanning proc_ro for ucred...
(sbx)   proc_ro+0x10: raw=0x20b smr=0x20b pac=0x20b
(sbx)   proc_ro+0x18: raw=0x22002301000005b0 smr=0x1000005b0 pac=0x1000005b0
(sbx)   proc_ro+0x20: raw=0xffffffde09a71390 smr=0xffffffde09a71390 pac=0xffffffde09a71390
(sbx) found ucred at proc_ro+0x20 (SMR) = 0xffffffde09a71390
(sbx) ucred=0xffffffde09a71390 label=0xffffffde09b5efc0 sandbox=0xffffffde565f81c0 ext_set=0xffffffdf3c37eb00
(sbx) patched 5 extensions
(sbx) changed 2 extension classes
(sbx) filled 13 empty hash slots
(sbx) escaped!

sandbox escape ready!

(vfs) vfs_init starting...
(vfs) heap PAC prefix: 0xffffff0000000000
(vfs) proc=0xffffffdf38356ed0 task=0xffffffdf38357610 (from exploit)
(vfs) file overwrite ready!
(vfs) g_rootvnode = 0xffffffe1efa9a300
(vfs) first_nc = 0xffffffde584ac038
(vfs) nc_vp = 0xffffffe1efa9a300
(vfs) nc_name = 0x74f42981
(vfs) nc_name offset probe: 0x60
(vfs) ncache ok: first child='.tmp.HATYlCLkPZ'
(vfs) vfs_init done

vfs ready!

selected source: Assets.car

(vfs) target /private/var/mobile/Containers/Data/Application/BE1B6B43-94DF-461C-8B05-BEA142EB5C8C/tmp/vfs_custom_1419339C-1BD5-4FA9-8F08-A11C4C8904DC -> /System/Library/PrivateFrameworks/MaterialKit.framework/Assets.car

(vfs) vfs_overwritefile returned: -1

(vfs) failed to overwrite file
(vfs) overwrite: /System/Library/PrivateFrameworks/MaterialKit.framework/Assets.car <- /private/var/mobile/Containers/Data/Application/BE1B6B43-94DF-461C-8B05-BEA142EB5C8C/tmp/vfs_custom_1419339C-1BD5-4FA9-8F08-A11C4C8904DC
(vfs) target mmap'd at 0x11bb90000 (size 20536)
(vfs) task (from exploit): 0xffffffdf38357610
(vfs) vm_map: 0xffffffde0994c2c0 (task+0x28, nentries=216)
(vfs) exploit task looks valid (nentries=216)
(vfs) vm_map: 0xffffffde0994c2c0 (task+0x28, nentries=216)
(vfs) vm_map entries: 216, looking for 0x11bb90000
(vfs) found entry 0xffffffdd23c2ce50: 0x11bb90000-0x11bb98000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) mmap source failed: Invalid argument
overwrite failed: /System/Library/PrivateFrameworks/MaterialKit.framework/Assets.car
```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``[Insert here]``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- Apr 27, 2026: 👤 @TequilaDash assigned @Pro42good
- Apr 27, 2026: 🏷️ @TequilaDash added label `bug`


---

*Exported on 10/14/2026, 3:03:00 PM from [https://github.com/rooootdev/lara/issues/200](https://github.com/rooootdev/lara/issues/200)*
