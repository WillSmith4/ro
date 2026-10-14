# [BUG] Does jit work

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#279](https://github.com/rooootdev/lara/issues/279)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@Jaggerisbest](https://github.com/Jaggerisbest)  
> **Labels:** `bug`  
> **Created:** May 29, 2026  
> **Updated:** May 29, 2026  
> **Closed:** May 29, 2026  

---

## Description

*Opened by [@Jaggerisbest](https://github.com/Jaggerisbest)* *on May 29, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
When i use the larajit feature it respings after i try to leave the app and dolphin(which i signed with an jit cert) says jit not aquirred

### To Reproduce:
Steps to reproduce the behavior:
1. Go to larajit section
2. Tap enable on dolphin 
3. Exit the app
4. See error

### Expected behavior:
<!-- What you expected to happen. -->
When i click enable it should let me leave the app and when i go to dolphin it should say jit aquired

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

### Device Info:
- **Device:** ``iphone 11`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
- **Chip:** `A13`` <!-- e.g. A15, A14, M1 -->
- **iOS Version:** ``18.6.2`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** ``[NOPE(sadly)]`` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``.2`` <!-- e.g. v0.3 or commit hash -->

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
```

lara started: 2026-05-29 12:27:32

(rc) route process=youtube os=18.6.2 ios16=0

(utils) darksword not ready

(rc) Unable to find process: youtube

(rc) RemoteCall dealloc

(rc) destroyRemoteCall enter success=0 trojanMem=0x0 creatingExtraThread=0 originalNeedsRestore=0 firstPort=0x0 secondPort=0x0

(utils) T1SZ_BOOT: 0x19

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0xb8

(utils) PROC_PID_OFFSET: 0x60

(offs) (user-set) t1sz_boot = 0x19 (default: 0x0)

(offs) loaded user-saved offsets above

(offs) initialized offsets

(offs) kernel: Darwin Kernel Version 24.6.0: Tue Jul 15 21:53:57 PDT 2025; root:xnu-11417.140.69~16/RELEASE_ARM64_T8030

(offs) kernbase: 0xfffffff007004000

(offs) kernentry: 0xfffffff007ef04f0

(offs) allproc: 0x364fcb8

(offs) kernproc: 0xada3a8

(offs) rootvnode: 0x3650300

(offs) procsize: 0x740

(offs) t1szboot: 0x19

(offs) initialized offsets

(ds) starting darksword
(ds) offsets: inpcb.icmp6filt=0x148 socket.usecount=0x254 socket.proto=0x20 protosw.input=0x28
(ds) device: iPhone12,1
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0xb
(ds) write_fd: 0xc
(ds) executable_path:       /private/var/containers/Bundle/Application/859B973A-97E1-49B9-86A8-42EC977AA296/lara.app/lara
(ds) host_executable_path:  /private/var/containers/Bundle/Application/859B973A-97E1-49B9-86A8-42EC977AA296/lara.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name:  lara
(ds) kernel_process_name:   lara
(ds) livecontainer_bundle:  no
(ds) livecontainer_guest:   no
(ds) rehosted_process:      no
(ds) process_marker 0: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x134e6c000
(ds) physical_mapping_address: 0x112c80000
(ds) pc_object: 0x8403
(ds) pc_address: 0x806870000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x6dff4
(ds) end_pcb_id: 0x78ff2
(ds) looking in search mapping: 0 / 8
(ds) matched PCB via process marker: lara
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x765be
(ds) inp_list_next_pointer: 0xffffffe0ec574400
(ds) icmp6filter: 0xffffffe3ef33a3e0
(ds) corrupting icmp6filter pointer...
(ds) corrupt attempt 0: write target=0xe8248 value=0xffffffe0ec574548
(ds) corrupt attempt 0: write returned
(ds) corrupt attempt 0: read new_icmp6filter=0xffffffe0ec574548
(ds) target corrupted: 0xffffffe0ec574548
(ds) found control_socket at idx: 0x42e5

(utils) found self proc via thread_ro+0x18 -> 0xffffffe1d5c07248 pid=13437 (pidoff=0x60)

(utils) found self proc via socket hardcoded -> 0xffffffe1d5c07248
(ds) exploit success!
(ds) kernel_base:  0xfffffff01ca8c000
(ds) kernel_slide: 0x15a88000

(ds) highest_success_idx: 268
(ds) success_read_count: 19
(ds) walking kernel structures...
(ds) control_socket_pcb: 0xffffffe0ec574000
(ds) pcbinfo_pointer: 0xfffffff0200dbd90
(ds) ipi_zone: 0xfffffff01d56f6a0
(ds) zv_name: 0xfffffff01cb02303
(ds) searching for kernel mach-o header from 0xfffffff01cb00000...
(ds) candidate mach-o at 0xfffffff01ca94000: filetype=2 cpuinfo=0x2c0000002 (iter=27)
(ds) candidate mach-o at 0xfffffff01ca8c000: filetype=12 cpuinfo=0xc00000002 (iter=29)
(ds) found mh_fileset header at 0xfffffff01ca8c000
(ds) t1sz_boot: 0xffffffffffffffff
(ds) kernel_base:  0xfffffff01ca8c000
(ds) kernel_slide: 0x15a88000
(ds) krw leak: control_socket_pcb=0xffffffe0ec574000 rw_socket_pcb=0xffffffe0ec574400
(ds) krw leak: control_socket_addr=0xffffffe0ec524500 rw_socket_addr=0xffffffe0ec5248d8
(ds) krw leak: control_socket_so_count=0x1 rw_socket_so_count=0x2
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffe1d5c07248
(ds) our_task: 0xffffffe1d5c07988

(ds) exploit success!
(ds) kernel_base:  0xfffffff01ca8c000
(ds) kernel_slide: 0x15a88000

(vfs) vfs_init starting...
(vfs) heap PAC prefix: 0xffffff0000000000
(vfs) proc=0xffffffe1d5c07248 task=0xffffffe1d5c07988 (from exploit)
(vfs) file overwrite ready!
(sbx) proc=0xffffffe1d5c07248 proc_ro_raw=0xffffffded6d19470 proc_ro=0xffffffded6d19470
(sbx) scanning proc_ro for ucred...
(sbx)   proc_ro+0x10: raw=0x347d smr=0x347d pac=0x347d
(sbx)   proc_ro+0x18: raw=0x1000097b9 smr=0x1000097b9 pac=0x1000097b9
(sbx)   proc_ro+0x20: raw=0x2200230100000001 smr=0x100000001 pac=0x100000001
(sbx)   proc_ro+0x28: raw=0xffffffdf239842b0 smr=0xffffffdf239842b0 pac=0xffffffdf239842b0
(sbx) found ucred at proc_ro+0x28 (SMR) = 0xffffffdf239842b0
(sbx) ucred=0xffffffdf239842b0 label=0xffffffded6cef080 sandbox=0xffffffded6dea080 ext_set=0xffffffe1d78310c0
(sbx) patched 5 extensions
(sbx) changed 3 extension classes
(sbx) filled 11 empty hash slots
(vfs) g_rootvnode = 0xffffffe2bce1c400
(vfs) first_nc = 0xffffffe1d7606ef0
(vfs) nc_vp = 0xffffffe2bce1c400
(vfs) nc_name = 0x718b60ff
(vfs) nc_name offset probe: 0x60
(vfs) ncache ok: first child='.tmp.x6AQlaAURI'
(vfs) vfs_init done

vfs ready!

(sbx) escaped!

sandbox escape ready!

(jit) enabling for me.oatmealdome.DolphiniOS-njb-patreon-beta...

(rc) route process=SpringBoard os=18.6.2 ios16=0

(rc) process: SpringBoard, pid: 13300

(rc) firstExceptionPort: 0x56c00f, secondExceptionPort: 0x592513
initializing remote call on SpringBoard...

(rc) [iter 0] kstack=0xffffffe3f1caff30 kernelSP=0xffffffe3f1caf880

(rc) [iter 0] Found TRO at offset=0x39f0

(rc) [iter 0] checkVal=0x104e checkVal2=0xffffffded6cef740 (expecting 0x104e)

(rc) [iter 0] TRO swap: writing target tro=0xffffffded6eb9be8 to addr=0xffffffe3f1caf9f0

(rc) TRO swap SUCCESS!

(rc) set_exception_port_on_thread returning success=1

(rc) Inject EXC_GUARD on thread:0xffffffe0efed5050 OK

(rc) Valid threads: 1

(rc) Injected threads: 1

(rc) Finish clearing EXC_GUARD from all other threads...

(rc) trojanMemTemp: 0x16d219d40

(rc) vmMap: 0xffffffded6c2b3d0

(pac) found pacia gadget, gadget addr = 0x19362ab30

(pac) remotepac: addr=0x101 mod=0x7481000006000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xf70f798000000101

(pac) remotepac: addr=0x201 mod=0x77d3000006000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xbc48ca0000000201

(rc) signState: thread=0xffffffe0efed5050 flags_before=0x600000c diver=0x6000000 pc_raw=0x101 pc_signed=0xf70f798000000101 lr_raw=0x201 lr_signed=0xbc48ca0000000201

(rc) pcAddr for getpid: 0x1e5af6f9c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=getpid temp=1 exc=0x1 code=[0x101 0x101] pc=0x6b45c48000000101 lr=0x2847920000000201 sp=0x4e40816d21dd40 flags=0xdb00000c trojan=0xffffffe0efed5050

(pac) remotepac: addr=0x1e5af6f9c mod=0x74810000db000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xf9528601e5af6f9c

(pac) remotepac: addr=0x201 mod=0x77d30000db000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x275d9b0000000201

(rc) signState: thread=0xffffffe0efed5050 flags_before=0xdb00000c diver=0xdb000000 pc_raw=0x1e5af6f9c pc_signed=0xf9528601e5af6f9c lr_raw=0x201 lr_signed=0x275d9b0000000201

(rc) Replying remote call: name=getpid pc=0xf9528601e5af6f9c lr=0x275d9b0000000201 sp=0x4e40816d21dd40 flags=0xdb000000 x0=0x0 x1=0x0

(rc) Return exception: name=getpid temp=1 exc=0x1 code=[0x101 0x201] pc=0x58526c0000000201 lr=0x2847920000000201 returnPC=0x201 returnLR=0x201 expected=0x201 x0=0x33f4 flags=0x7300000c

(rc) getpid func's retValue = 0x33f4(13300)

(pac) remotepac: addr=0x301 mod=0x0 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x4e3ea98000000301

(rc) remoteCrashSigned: 0x4e3ea98000000301

(rc) pcAddr for pthread_create_suspended_np: 0x21f128bd0

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=pthread_create_suspended_np temp=1 exc=0x1 code=[0x101 0x201] pc=0x58526c0000000201 lr=0x2847920000000201 sp=0x4e40816d21dd40 flags=0xa00000c trojan=0xffffffe0efed5050

(pac) remotepac: addr=0x21f128bd0 mod=0x748100000a000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xb352da021f128bd0

(pac) remotepac: addr=0x201 mod=0x77d300000a000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x943d020000000201

(rc) signState: thread=0xffffffe0efed5050 flags_before=0xa00000c diver=0xa000000 pc_raw=0x21f128bd0 pc_signed=0xb352da021f128bd0 lr_raw=0x201 lr_signed=0x943d020000000201

(rc) Replying remote call: name=pthread_create_suspended_np pc=0xb352da021f128bd0 lr=0x943d020000000201 sp=0x4e40816d21dd40 flags=0xa000000 x0=0x16d219d40 x1=0x0

(rc) Return exception: name=pthread_create_suspended_np temp=1 exc=0x1 code=[0x101 0x201] pc=0x58526c0000000201 lr=0x8841798000000201 returnPC=0x201 returnLR=0x201 expected=0x201 x0=0x0 flags=0xf800000e

(rc) pthread_create_suspended_np func's retValue = 0x0(0)

(rc) trojanMemTemp: 0x16d219d40

(rc) pthreadAddr: 0x16d81f000

(rc) pcAddr for pthread_mach_thread_np: 0x21f1242d4

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=pthread_mach_thread_np temp=1 exc=0x1 code=[0x101 0x201] pc=0x58526c0000000201 lr=0x8841798000000201 sp=0x4e40816d21dd40 flags=0xb200000e trojan=0xffffffe0efed5050

(pac) remotepac: addr=0x21f1242d4 mod=0x74810000b2000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xa91db8821f1242d4

(pac) remotepac: addr=0x201 mod=0x77d30000b2000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x3e23e30000000201

(rc) signState: thread=0xffffffe0efed5050 flags_before=0xb200000e diver=0xb2000000 pc_raw=0x21f1242d4 pc_signed=0xa91db8821f1242d4 lr_raw=0x201 lr_signed=0x3e23e30000000201

(rc) Replying remote call: name=pthread_mach_thread_np pc=0xa91db8821f1242d4 lr=0x3e23e30000000201 sp=0x4e40816d21dd40 flags=0xb2000000 x0=0x16d81f000 x1=0x0

(rc) Return exception: name=pthread_mach_thread_np temp=1 exc=0x1 code=[0x101 0x201] pc=0x58526c0000000201 lr=0x8841798000000201 returnPC=0x201 returnLR=0x201 expected=0x201 x0=0x5a7f flags=0x1b00000e

(rc) pthread_mach_thread_np func's retValue = 0x5a7f(23167)

(rc) callThreadPort: 0x5a7f

(rc) [iter 0] kstack=0xffffffe3f1e0bf30 kernelSP=0xffffffe3f1e0b880

(rc) [iter 0] Found TRO at offset=0x39f0

(rc) [iter 0] checkVal=0x104e checkVal2=0xffffffded6cef740 (expecting 0x104e)

(rc) [iter 0] TRO swap: writing target tro=0xffffffded6dbf190 to addr=0xffffffe3f1e0b9f0

(rc) TRO swap SUCCESS!

(rc) set_exception_port_on_thread returning success=1

(rc) All good! Resuming trojan thread...

(rc) pcAddr for thread_resume: 0x1e5af30b0

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=thread_resume temp=1 exc=0x1 code=[0x101 0x201] pc=0x58526c0000000201 lr=0x8841798000000201 sp=0x4e40816d21dd40 flags=0xa900000e trojan=0xffffffe0efed5050

(pac) remotepac: addr=0x1e5af30b0 mod=0x74810000a9000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x4e747f01e5af30b0

(pac) remotepac: addr=0x201 mod=0x77d30000a9000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x300ff00000000201

(rc) signState: thread=0xffffffe0efed5050 flags_before=0xa900000e diver=0xa9000000 pc_raw=0x1e5af30b0 pc_signed=0x4e747f01e5af30b0 lr_raw=0x201 lr_signed=0x300ff00000000201

(rc) Replying remote call: name=thread_resume pc=0x4e747f01e5af30b0 lr=0x300ff00000000201 sp=0x4e40816d21dd40 flags=0xa9000000 x0=0x5a7f x1=0x0

(rc) Return exception: name=thread_resume temp=1 exc=0x1 code=[0x101 0x201] pc=0x58526c0000000201 lr=0x8841798000000201 returnPC=0x201 returnLR=0x201 expected=0x201 x0=0x0 flags=0x9d00000e

(rc) thread_resume func's retValue = 0x0(0)

(rc) New thread created, resuming original

(pac) remotepac: addr=0x390a6301e5af1ce4 mod=0x74810000db000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x5e47dd81e5af1ce4

(pac) remotepac: addr=0xc2c6d01e5af539c mod=0x77d30000db000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x24367601e5af539c

(rc) signState: thread=0xffffffe0efed5050 flags_before=0xdb00000e diver=0xdb000000 pc_raw=0x1e5af1ce4 pc_signed=0x5e47dd81e5af1ce4 lr_raw=0x1e5af539c lr_signed=0x24367601e5af539c

(rc) Original thread restored

(rc) pcAddr for getpid: 0x1e5af6f9c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=getpid temp=0 exc=0x1 code=[0x101 0x301] pc=0xfc21908000000301 lr=0x3941b5821f123344 sp=0xf67816d81efd0 flags=0xda00000c trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1e5af6f9c mod=0x74810000da000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x47674501e5af6f9c

(pac) remotepac: addr=0x401 mod=0x77d30000da000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x3b25d88000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xda00000c diver=0xda000000 pc_raw=0x1e5af6f9c pc_signed=0x47674501e5af6f9c lr_raw=0x401 lr_signed=0x3b25d88000000401

(rc) Replying remote call: name=getpid pc=0x47674501e5af6f9c lr=0x3b25d88000000401 sp=0xf67816d81efd0 flags=0xda000000 x0=0x0 x1=0x0

(rc) Return exception: name=getpid temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0x33f4 flags=0xc100000c

(rc) getpid func's retValue = 0x33f4(13300)

(rc) Task pid: 13300

(rc) pcAddr for mmap: 0x1e5af7bcc

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=mmap temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 sp=0xf67816d81efd0 flags=0x800000c trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1e5af7bcc mod=0x7481000008000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xa87d1a01e5af7bcc

(pac) remotepac: addr=0x401 mod=0x77d3000008000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xdb01860000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x800000c diver=0x8000000 pc_raw=0x1e5af7bcc pc_signed=0xa87d1a01e5af7bcc lr_raw=0x401 lr_signed=0xdb01860000000401

(rc) Replying remote call: name=mmap pc=0xa87d1a01e5af7bcc lr=0xdb01860000000401 sp=0xf67816d81efd0 flags=0x8000000 x0=0x0 x1=0x4000

(rc) Return exception: name=mmap temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0x7e1364000 flags=0xb300000e

(rc) mmap func's retValue = 0x7e1364000(33843200000)

(rc) pcAddr for memset: 0x21f076120

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=memset temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0xb400000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x21f076120 mod=0x74810000b4000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x5a1d07821f076120

(pac) remotepac: addr=0x401 mod=0x77d30000b4000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xa7a4c8000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xb400000e diver=0xb4000000 pc_raw=0x21f076120 pc_signed=0x5a1d07821f076120 lr_raw=0x401 lr_signed=0xa7a4c8000000401

(rc) Replying remote call: name=memset pc=0x5a1d07821f076120 lr=0xa7a4c8000000401 sp=0xf67816d81efd0 flags=0xb4000000 x0=0x7e1364000 x1=0x0

(rc) Return exception: name=memset temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0x7e1364000 flags=0xe200000c

(rc) memset func's retValue = 0x7e1364000(33843200000)

(rc) Finished successfully

(rc) pcAddr for malloc: 0x1a532c03c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 sp=0xf67816d81efd0 flags=0xc600000c trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a532c03c mod=0x74810000c6000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xfd554081a532c03c

(pac) remotepac: addr=0x401 mod=0x77d30000c6000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x7802228000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xc600000c diver=0xc6000000 pc_raw=0x1a532c03c pc_signed=0xfd554081a532c03c lr_raw=0x401 lr_signed=0x7802228000000401

(rc) Replying remote call: name=malloc pc=0xfd554081a532c03c lr=0x7802228000000401 sp=0xf67816d81efd0 flags=0xc6000000 x0=0x14 x1=0x0

(rc) Return exception: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xd49b5fa20 flags=0xc700000c

(rc) malloc func's retValue = 0xd49b5fa20(57071237664)

(rc) pcAddr for objc_getClass: 0x192236d88

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=objc_getClass temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 sp=0xf67816d81efd0 flags=0x9b00000c trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x192236d88 mod=0x748100009b000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xcf3f650192236d88

(pac) remotepac: addr=0x401 mod=0x77d300009b000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x903f448000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x9b00000c diver=0x9b000000 pc_raw=0x192236d88 pc_signed=0xcf3f650192236d88 lr_raw=0x401 lr_signed=0x903f448000000401

(rc) Replying remote call: name=objc_getClass pc=0xcf3f650192236d88 lr=0x903f448000000401 sp=0xf67816d81efd0 flags=0x9b000000 x0=0xd49b5fa20 x1=0x0

(rc) Return exception: name=objc_getClass temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0x1ff834f20 flags=0xaa00000e

(rc) objc_getClass func's retValue = 0x1ff834f20(8581762848)

(rc) pcAddr for free: 0x1a5327f00

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0x1600000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a5327f00 mod=0x7481000016000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x4857a481a5327f00

(pac) remotepac: addr=0x401 mod=0x77d3000016000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xa67d110000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x1600000e diver=0x16000000 pc_raw=0x1a5327f00 pc_signed=0x4857a481a5327f00 lr_raw=0x401 lr_signed=0xa67d110000000401

(rc) Replying remote call: name=free pc=0x4857a481a5327f00 lr=0xa67d110000000401 sp=0xf67816d81efd0 flags=0x16000000 x0=0xd49b5fa20 x1=0x0

(rc) Return exception: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xbb7a878000000401 flags=0x500000e

(rc) free func's retValue = 0xbb7a878000000401(13509259016076788737)

(rc) pcAddr for malloc: 0x1a532c03c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0x9600000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a532c03c mod=0x7481000096000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x485b6e81a532c03c

(pac) remotepac: addr=0x401 mod=0x77d3000096000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x5667158000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x9600000e diver=0x96000000 pc_raw=0x1a532c03c pc_signed=0x485b6e81a532c03c lr_raw=0x401 lr_signed=0x5667158000000401

(rc) Replying remote call: name=malloc pc=0x485b6e81a532c03c lr=0x5667158000000401 sp=0xf67816d81efd0 flags=0x96000000 x0=0x11 x1=0x0

(rc) Return exception: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xd49b5fa20 flags=0x3300000c

(rc) malloc func's retValue = 0xd49b5fa20(57071237664)

(rc) pcAddr for objc_getClass: 0x192236d88

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=objc_getClass temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 sp=0xf67816d81efd0 flags=0xe800000c trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x192236d88 mod=0x74810000e8000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x16505a8192236d88

(pac) remotepac: addr=0x401 mod=0x77d30000e8000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xb360510000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xe800000c diver=0xe8000000 pc_raw=0x192236d88 pc_signed=0x16505a8192236d88 lr_raw=0x401 lr_signed=0xb360510000000401

(rc) Replying remote call: name=objc_getClass pc=0x16505a8192236d88 lr=0xb360510000000401 sp=0xf67816d81efd0 flags=0xe8000000 x0=0xd49b5fa20 x1=0x0

(rc) Return exception: name=objc_getClass temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0x1ff8356c8 flags=0xbe00000e

(rc) objc_getClass func's retValue = 0x1ff8356c8(8581764808)

(rc) pcAddr for free: 0x1a5327f00

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0x6700000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a5327f00 mod=0x7481000067000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xe30e9501a5327f00

(pac) remotepac: addr=0x401 mod=0x77d3000067000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x8a611c8000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x6700000e diver=0x67000000 pc_raw=0x1a5327f00 pc_signed=0xe30e9501a5327f00 lr_raw=0x401 lr_signed=0x8a611c8000000401

(rc) Replying remote call: name=free pc=0xe30e9501a5327f00 lr=0x8a611c8000000401 sp=0xf67816d81efd0 flags=0x67000000 x0=0xd49b5fa20 x1=0x0

(rc) Return exception: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xbb7a878000000401 flags=0xb000000e

(rc) free func's retValue = 0xbb7a878000000401(13509259016076788737)

(rc) pcAddr for malloc: 0x1a532c03c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0xb00000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a532c03c mod=0x748100000b000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x9054c381a532c03c

(pac) remotepac: addr=0x401 mod=0x77d300000b000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x4740f60000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xb00000e diver=0xb000000 pc_raw=0x1a532c03c pc_signed=0x9054c381a532c03c lr_raw=0x401 lr_signed=0x4740f60000000401

(rc) Replying remote call: name=malloc pc=0x9054c381a532c03c lr=0x4740f60000000401 sp=0xf67816d81efd0 flags=0xb000000 x0=0x13 x1=0x0

(rc) Return exception: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xd49b5fa20 flags=0xb600000c

(rc) malloc func's retValue = 0xd49b5fa20(57071237664)

(rc) pcAddr for sel_registerName: 0x192236b98

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=sel_registerName temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 sp=0xf67816d81efd0 flags=0x7f00000c trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x192236b98 mod=0x748100007f000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x860f6f0192236b98

(pac) remotepac: addr=0x401 mod=0x77d300007f000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xae3f500000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x7f00000c diver=0x7f000000 pc_raw=0x192236b98 pc_signed=0x860f6f0192236b98 lr_raw=0x401 lr_signed=0xae3f500000000401

(rc) Replying remote call: name=sel_registerName pc=0x860f6f0192236b98 lr=0xae3f500000000401 sp=0xf67816d81efd0 flags=0x7f000000 x0=0xd49b5fa20 x1=0x0

(rc) Return exception: name=sel_registerName temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0x20df2b669 flags=0x5700000e

(rc) sel_registerName func's retValue = 0x20df2b669(8823944809)

(rc) pcAddr for free: 0x1a5327f00

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0x5300000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a5327f00 mod=0x7481000053000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xdc605581a5327f00

(pac) remotepac: addr=0x401 mod=0x77d3000053000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x9a745e8000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x5300000e diver=0x53000000 pc_raw=0x1a5327f00 pc_signed=0xdc605581a5327f00 lr_raw=0x401 lr_signed=0x9a745e8000000401

(rc) Replying remote call: name=free pc=0xdc605581a5327f00 lr=0x9a745e8000000401 sp=0xf67816d81efd0 flags=0x53000000 x0=0xd49b5fa20 x1=0x0

(rc) Return exception: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xbb7a878000000401 flags=0xc400000e

(rc) free func's retValue = 0xbb7a878000000401(13509259016076788737)

(rc) pcAddr for malloc: 0x1a532c03c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0x1b00000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a532c03c mod=0x748100001b000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x611fba01a532c03c

(pac) remotepac: addr=0x401 mod=0x77d300001b000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x8a3b118000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x1b00000e diver=0x1b000000 pc_raw=0x1a532c03c pc_signed=0x611fba01a532c03c lr_raw=0x401 lr_signed=0x8a3b118000000401

(rc) Replying remote call: name=malloc pc=0x611fba01a532c03c lr=0x8a3b118000000401 sp=0xf67816d81efd0 flags=0x1b000000 x0=0x9 x1=0x0

(rc) Return exception: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xd47430fd0 flags=0x300000c

(rc) malloc func's retValue = 0xd47430fd0(57030152144)
remote call initialized on SpringBoard
(jit) error enabling for me.oatmealdome.DolphiniOS-njb-patreon-beta!

(rc) pcAddr for objc_getClass: 0x192236d88

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=objc_getClass temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 sp=0xf67816d81efd0 flags=0x9200000c trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x192236d88 mod=0x7481000092000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x8f7b590192236d88

(pac) remotepac: addr=0x401 mod=0x77d3000092000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x35135a0000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x9200000c diver=0x92000000 pc_raw=0x192236d88 pc_signed=0x8f7b590192236d88 lr_raw=0x401 lr_signed=0x35135a0000000401

(rc) Replying remote call: name=objc_getClass pc=0x8f7b590192236d88 lr=0x35135a0000000401 sp=0xf67816d81efd0 flags=0x92000000 x0=0xd47430fd0 x1=0x0

(rc) Return exception: name=objc_getClass temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0x1ff7feb18 flags=0x2800000e

(rc) objc_getClass func's retValue = 0x1ff7feb18(8581540632)

(rc) pcAddr for free: 0x1a5327f00

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0xfa00000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a5327f00 mod=0x74810000fa000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x475fe701a5327f00

(pac) remotepac: addr=0x401 mod=0x77d30000fa000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xab74cc8000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xfa00000e diver=0xfa000000 pc_raw=0x1a5327f00 pc_signed=0x475fe701a5327f00 lr_raw=0x401 lr_signed=0xab74cc8000000401

(rc) Replying remote call: name=free pc=0x475fe701a5327f00 lr=0xab74cc8000000401 sp=0xf67816d81efd0 flags=0xfa000000 x0=0xd47430fd0 x1=0x0

(rc) Return exception: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xbb7a878000000401 flags=0xfe00000e

(rc) free func's retValue = 0xbb7a878000000401(13509259016076788737)

(rc) pcAddr for malloc: 0x1a532c03c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0xe500000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a532c03c mod=0x74810000e5000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x366b981a532c03c

(pac) remotepac: addr=0x401 mod=0x77d30000e5000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xb008560000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xe500000e diver=0xe5000000 pc_raw=0x1a532c03c pc_signed=0x366b981a532c03c lr_raw=0x401 lr_signed=0xb008560000000401

(rc) Replying remote call: name=malloc pc=0x366b981a532c03c lr=0xb008560000000401 sp=0xf67816d81efd0 flags=0xe5000000 x0=0x2b x1=0x0

(rc) Return exception: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xd49bc4db0 flags=0xea00000c

(rc) malloc func's retValue = 0xd49bc4db0(57071652272)

(rc) pcAddr for objc_msgSend: 0x192209c00

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=objc_msgSend temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 sp=0xf67816d81efd0 flags=0xea00000c trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x192209c00 mod=0x74810000ea000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xfe21240192209c00

(pac) remotepac: addr=0x401 mod=0x77d30000ea000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x2f67a48000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xea00000c diver=0xea000000 pc_raw=0x192209c00 pc_signed=0xfe21240192209c00 lr_raw=0x401 lr_signed=0x2f67a48000000401

(rc) Replying remote call: name=objc_msgSend pc=0xfe21240192209c00 lr=0x2f67a48000000401 sp=0xf67816d81efd0 flags=0xea000000 x0=0x1ff7feb18 x1=0x20df2b669

(rc) Return exception: name=objc_msgSend temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xd47e012c0 flags=0xfe00000e

(rc) objc_msgSend func's retValue = 0xd47e012c0(57040442048)

(rc) pcAddr for free: 0x1a5327f00

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0xb300000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a5327f00 mod=0x74810000b3000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xc834e681a5327f00

(pac) remotepac: addr=0x401 mod=0x77d30000b3000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xa271390000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xb300000e diver=0xb3000000 pc_raw=0x1a5327f00 pc_signed=0xc834e681a5327f00 lr_raw=0x401 lr_signed=0xa271390000000401

(rc) Replying remote call: name=free pc=0xc834e681a5327f00 lr=0xa271390000000401 sp=0xf67816d81efd0 flags=0xb3000000 x0=0xd49bc4db0 x1=0x0

(rc) Return exception: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xbb7a878000000401 flags=0x4a00000e

(rc) free func's retValue = 0xbb7a878000000401(13509259016076788737)

(rc) pcAddr for malloc: 0x1a532c03c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0xc600000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a532c03c mod=0x74810000c6000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xfd554081a532c03c

(pac) remotepac: addr=0x401 mod=0x77d30000c6000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x7802228000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xc600000e diver=0xc6000000 pc_raw=0x1a532c03c pc_signed=0xfd554081a532c03c lr_raw=0x401 lr_signed=0x7802228000000401

(rc) Replying remote call: name=malloc pc=0xfd554081a532c03c lr=0x7802228000000401 sp=0xf67816d81efd0 flags=0xc6000000 x0=0x23 x1=0x0

(rc) Return exception: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xd49bc4db0 flags=0xeb00000c

(rc) malloc func's retValue = 0xd49bc4db0(57071652272)

(rc) pcAddr for sel_registerName: 0x192236b98

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=sel_registerName temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 sp=0xf67816d81efd0 flags=0x8f00000c trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x192236b98 mod=0x748100008f000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xe31a8b8192236b98

(pac) remotepac: addr=0x401 mod=0x77d300008f000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xdc3c790000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x8f00000c diver=0x8f000000 pc_raw=0x192236b98 pc_signed=0xe31a8b8192236b98 lr_raw=0x401 lr_signed=0xdc3c790000000401

(rc) Replying remote call: name=sel_registerName pc=0xe31a8b8192236b98 lr=0xdc3c790000000401 sp=0xf67816d81efd0 flags=0x8f000000 x0=0xd49bc4db0 x1=0x0

(rc) Return exception: name=sel_registerName temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0x20d788d35 flags=0xb300000e

(rc) sel_registerName func's retValue = 0x20d788d35(8815938869)

(rc) pcAddr for free: 0x1a5327f00

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0x5700000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a5327f00 mod=0x7481000057000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xe3453401a5327f00

(pac) remotepac: addr=0x401 mod=0x77d3000057000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xd707130000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x5700000e diver=0x57000000 pc_raw=0x1a5327f00 pc_signed=0xe3453401a5327f00 lr_raw=0x401 lr_signed=0xd707130000000401

(rc) Replying remote call: name=free pc=0xe3453401a5327f00 lr=0xd707130000000401 sp=0xf67816d81efd0 flags=0x57000000 x0=0xd49bc4db0 x1=0x0

(rc) Return exception: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xbb7a878000000401 flags=0x6700000e

(rc) free func's retValue = 0xbb7a878000000401(13509259016076788737)

(rc) pcAddr for malloc: 0x1a532c03c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0xe100000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a532c03c mod=0x74810000e1000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x319a381a532c03c

(pac) remotepac: addr=0x401 mod=0x77d30000e1000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xf276480000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xe100000e diver=0xe1000000 pc_raw=0x1a532c03c pc_signed=0x319a381a532c03c lr_raw=0x401 lr_signed=0xf276480000000401

(rc) Replying remote call: name=malloc pc=0x319a381a532c03c lr=0xf276480000000401 sp=0xf67816d81efd0 flags=0xe1000000 x0=0x1a x1=0x0

(rc) Return exception: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xd49b5fa20 flags=0xce00000c

(rc) malloc func's retValue = 0xd49b5fa20(57071237664)

(rc) pcAddr for sel_registerName: 0x192236b98

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=sel_registerName temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 sp=0xf67816d81efd0 flags=0xef00000c trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x192236b98 mod=0x74810000ef000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x3302188192236b98

(pac) remotepac: addr=0x401 mod=0x77d30000ef000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xd21c568000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xef00000c diver=0xef000000 pc_raw=0x192236b98 pc_signed=0x3302188192236b98 lr_raw=0x401 lr_signed=0xd21c568000000401

(rc) Replying remote call: name=sel_registerName pc=0x3302188192236b98 lr=0xd21c568000000401 sp=0xf67816d81efd0 flags=0xef000000 x0=0xd49b5fa20 x1=0x0

(rc) Return exception: name=sel_registerName temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0x20d71c810 flags=0x8500000e

(rc) sel_registerName func's retValue = 0x20d71c810(8815495184)

(rc) pcAddr for free: 0x1a5327f00

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0x4400000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a5327f00 mod=0x7481000044000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xfb6c0501a5327f00

(pac) remotepac: addr=0x401 mod=0x77d3000044000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xee6bf70000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x4400000e diver=0x44000000 pc_raw=0x1a5327f00 pc_signed=0xfb6c0501a5327f00 lr_raw=0x401 lr_signed=0xee6bf70000000401

(rc) Replying remote call: name=free pc=0xfb6c0501a5327f00 lr=0xee6bf70000000401 sp=0xf67816d81efd0 flags=0x44000000 x0=0xd49b5fa20 x1=0x0

(rc) Return exception: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xbb7a878000000401 flags=0xe600000e

(rc) free func's retValue = 0xbb7a878000000401(13509259016076788737)

(rc) pcAddr for malloc: 0x1a532c03c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0xcc00000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a532c03c mod=0x74810000cc000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x395ef181a532c03c

(pac) remotepac: addr=0x401 mod=0x77d30000cc000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x4c04488000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xcc00000e diver=0xcc000000 pc_raw=0x1a532c03c pc_signed=0x395ef181a532c03c lr_raw=0x401 lr_signed=0x4c04488000000401

(rc) Replying remote call: name=malloc pc=0x395ef181a532c03c lr=0x4c04488000000401 sp=0xf67816d81efd0 flags=0xcc000000 x0=0x8 x1=0x0

(rc) Return exception: name=malloc temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xd47430fd0 flags=0xff00000c

(rc) malloc func's retValue = 0xd47430fd0(57030152144)

(rc) pcAddr for sel_registerName: 0x192236b98

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=sel_registerName temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 sp=0xf67816d81efd0 flags=0x6900000c trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x192236b98 mod=0x7481000069000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x7330660192236b98

(pac) remotepac: addr=0x401 mod=0x77d3000069000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x1812dd8000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x6900000c diver=0x69000000 pc_raw=0x192236b98 pc_signed=0x7330660192236b98 lr_raw=0x401 lr_signed=0x1812dd8000000401

(rc) Replying remote call: name=sel_registerName pc=0x7330660192236b98 lr=0x1812dd8000000401 sp=0xf67816d81efd0 flags=0x69000000 x0=0xd47430fd0 x1=0x0

(rc) Return exception: name=sel_registerName temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0x20cf739b1 flags=0xca00000e

(rc) sel_registerName func's retValue = 0x20cf739b1(8807463345)

(rc) pcAddr for free: 0x1a5327f00

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0xda00000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1a5327f00 mod=0x74810000da000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xe2023181a5327f00

(pac) remotepac: addr=0x401 mod=0x77d30000da000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x3b25d88000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xda00000e diver=0xda000000 pc_raw=0x1a5327f00 pc_signed=0xe2023181a5327f00 lr_raw=0x401 lr_signed=0x3b25d88000000401

(rc) Replying remote call: name=free pc=0xe2023181a5327f00 lr=0x3b25d88000000401 sp=0xf67816d81efd0 flags=0xda000000 x0=0xd47430fd0 x1=0x0

(rc) Return exception: name=free temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xbb7a878000000401 flags=0x1800000e

(rc) free func's retValue = 0xbb7a878000000401(13509259016076788737)

(rc) pcAddr for objc_msgSend: 0x192209c00

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=objc_msgSend temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0x8300000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x192209c00 mod=0x7481000083000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x750a690192209c00

(pac) remotepac: addr=0x401 mod=0x77d3000083000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xdb610e8000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x8300000e diver=0x83000000 pc_raw=0x192209c00 pc_signed=0x750a690192209c00 lr_raw=0x401 lr_signed=0xdb610e8000000401

(rc) Replying remote call: name=objc_msgSend pc=0x750a690192209c00 lr=0xdb610e8000000401 sp=0xf67816d81efd0 flags=0x83000000 x0=0x1ff834f20 x1=0x20d788d35

(rc) Return exception: name=objc_msgSend temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xd47c9a5c0 flags=0x1600000e

(rc) objc_msgSend func's retValue = 0xd47c9a5c0(57038972352)

(rc) pcAddr for objc_msgSend: 0x192209c00

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=objc_msgSend temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0x4200000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x192209c00 mod=0x7481000042000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x5545f78192209c00

(pac) remotepac: addr=0x401 mod=0x77d3000042000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x2b6e0b0000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x4200000e diver=0x42000000 pc_raw=0x192209c00 pc_signed=0x5545f78192209c00 lr_raw=0x401 lr_signed=0x2b6e0b0000000401

(rc) Replying remote call: name=objc_msgSend pc=0x5545f78192209c00 lr=0x2b6e0b0000000401 sp=0xf67816d81efd0 flags=0x42000000 x0=0x1ff8356c8 x1=0x20d71c810

(rc) Return exception: name=objc_msgSend temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xd48720600 flags=0x3b00000e

(rc) objc_msgSend func's retValue = 0xd48720600(57050007040)

(rc) pcAddr for objc_msgSend: 0x192209c00

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=objc_msgSend temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0x1d00000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x192209c00 mod=0x748100001d000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x60396b8192209c00

(pac) remotepac: addr=0x401 mod=0x77d300001d000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xa742fc8000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x1d00000e diver=0x1d000000 pc_raw=0x192209c00 pc_signed=0x60396b8192209c00 lr_raw=0x401 lr_signed=0xa742fc8000000401

(rc) Replying remote call: name=objc_msgSend pc=0x60396b8192209c00 lr=0xa742fc8000000401 sp=0xf67816d81efd0 flags=0x1d000000 x0=0xd48720600 x1=0x20cf739b1

(rc) Return exception: name=objc_msgSend temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0x3402 flags=0x2b00000c

(rc) objc_msgSend func's retValue = 0x3402(13314)

(rc) pcAddr for CFRelease: 0x194c890e8

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=CFRelease temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 sp=0xf67816d81efd0 flags=0x5000000c trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x194c890e8 mod=0x7481000050000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x825ceb0194c890e8

(pac) remotepac: addr=0x401 mod=0x77d3000050000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x8f1e618000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x5000000c diver=0x50000000 pc_raw=0x194c890e8 pc_signed=0x825ceb0194c890e8 lr_raw=0x401 lr_signed=0x8f1e618000000401

(rc) Replying remote call: name=CFRelease pc=0x825ceb0194c890e8 lr=0x8f1e618000000401 sp=0xf67816d81efd0 flags=0x50000000 x0=0xd47e012c0 x1=0x0

(rc) Return exception: name=CFRelease temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xd47e012c0 flags=0xd600000e

(rc) CFRelease func's retValue = 0xd47e012c0(57040442048)

(rc) pcAddr for ptrace: 0x1e5b04afc

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=ptrace temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0x3d00000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1e5b04afc mod=0x748100003d000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xe54bed01e5b04afc

(pac) remotepac: addr=0x401 mod=0x77d300003d000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xeb119f0000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0x3d00000e diver=0x3d000000 pc_raw=0x1e5b04afc pc_signed=0xe54bed01e5b04afc lr_raw=0x401 lr_signed=0xeb119f0000000401

(rc) Replying remote call: name=ptrace pc=0xe54bed01e5b04afc lr=0xeb119f0000000401 sp=0xf67816d81efd0 flags=0x3d000000 x0=0xe x1=0x3402

(rc) Return exception: name=ptrace temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0xffffffffffffffff flags=0x2f00000e

(rc) ptrace func's retValue = 0xffffffffffffffff(18446744073709551615)

(rc) pcAddr for __error: 0x1e5af178c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying: name=__error temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xbb7a878000000401 sp=0xf67816d81efd0 flags=0xb300000e trojan=0xffffffe0efd2c2b0

(pac) remotepac: addr=0x1e5af178c mod=0x74810000b3000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x8062c781e5af178c

(pac) remotepac: addr=0x401 mod=0x77d30000b3000000 gadget=0x19362ab30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xa271390000000401

(rc) signState: thread=0xffffffe0efd2c2b0 flags_before=0xb300000e diver=0xb3000000 pc_raw=0x1e5af178c pc_signed=0x8062c781e5af178c lr_raw=0x401 lr_signed=0xa271390000000401

(rc) Replying remote call: name=__error pc=0x8062c781e5af178c lr=0xa271390000000401 sp=0xf67816d81efd0 flags=0xb3000000 x0=0x0 x1=0x0

(rc) Return exception: name=__error temp=0 exc=0x1 code=[0x101 0x401] pc=0x6a29f18000000401 lr=0xdb4f688000000401 returnPC=0x401 returnLR=0x401 expected=0x401 x0=0x16d81f0ac flags=0x4400000c

(rc) __error func's retValue = 0x16d81f0ac(6132199596)

```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``[Insert here]``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [ x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [ x] I can reproduce this on the latest version / commit of Lara
- [ x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [ x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- May 29, 2026: 🏷️ @Jaggerisbest added label `bug`
- May 29, 2026: 🔴 Closed by @Jaggerisbest


---

## Comments (1)

### Comment #1 — [@Jaggerisbest](https://github.com/Jaggerisbest) · May 29, 2026

`ID: 4578013491`


I did not see readme i am crazy


---

*Exported on 10/14/2026, 3:09:48 PM from [https://github.com/rooootdev/lara/issues/279](https://github.com/rooootdev/lara/issues/279)*
