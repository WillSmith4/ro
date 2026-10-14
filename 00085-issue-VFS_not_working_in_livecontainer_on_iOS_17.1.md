# VFS not working in livecontainer on iOS 17.1

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#85](https://github.com/rooootdev/lara/issues/85)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 12, 2026  
> **Updated:** Apr 14, 2026  
> **Closed:** Apr 14, 2026  

---

## Description

*Opened by [@Tobim6](https://github.com/Tobim6)* *on Apr 12, 2026*


Hi.
lara started: 2026-04-12 19:11:53

(utils) getprocsize() returned 0x0. defaulting to 0x740.

(utils) TASK_TNEXT_OFFSET: 0x58

(utils) THREAD_MUPCB_OFFSET: 0xb0

(utils) PROC_PID_OFFSET: 0x28

(utils) PROC_STRUCT_SIZE: 0x740

initialized offsetsdownloading kernelcache → /var/mobile/Containers/Data/Application/3E405DE4-7B64-4466-9CF9-A17D2B0A4844/Documents/Data/Application/0D715F8C-CBB4-42E1-A77F-36DBD558FE04/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2023FallFCS/fullrestores/042-07806/68C80FC0-071A-46DD-ABFE-6A5BA2FCF162/iPhone14,5_17.1_21B74_Restore.ipsw (OTA: no)

libgrabkernel2: Downloading BuildManifest.plist...

libgrabkernel2: Downloading kernelcache.release.iphone14 to /var/mobile/Containers/Data/Application/3E405DE4-7B64-4466-9CF9-A17D2B0A4844/Documents/Data/Application/0D715F8C-CBB4-42E1-A77F-36DBD558FE04/Documents/kernelcache...

(ds) starting darksword
(ds) device: iPhone14,5
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0x14
(ds) write_fd: 0x15
(ds) executable_path: /private/var/mobile/Containers/Data/Application/3E405DE4-7B64-4466-9CF9-A17D2B0A4844/Documents/Applications/com.roooot.laraapp.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/42AF2D77-5D94-4411-BBD8-8D204C229480/LiveContainer.app/LiveContainer
(ds) guest_executable_name: lara
(ds) host_executable_name: LiveContainer
(ds) kernel_process_name: LiveContainer
(ds) livecontainer_bundle: yes
(ds) livecontainer_guest: yes
(ds) rehosted_process: yes
(ds) process_marker[0]: LiveProcess
(ds) process_marker[1]: LiveContainer
(ds) process_marker[2]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x103100000
(ds) physical_mapping_address: 0x1175ac000
(ds) pc_object: 0xb503
(ds) pc_address: 0x38ef70000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x42b92
(ds) end_pcb_id: 0x4db90
(ds) looking in search mapping: 0
(ds) Matched PCB via process marker: LiveContainer
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x43ff0
(ds) inp_list_next_pointer: 0xffffffdd37e7c400
(ds) icmp6filter: 0xffffffe0ce158d80
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffdd37e7c548
(ds) found control_socket at idx: 0xa2f
(ds) highest_success_idx: 479
(ds) success_read_count: 48
(ds) Walking kernel structures...

(utils) kernel proc: 0x9f867ff06d8b0c20

(utils) kernel proc pointer invalid

(utils) socket fallback: pcb=0xffffffdd37e7c400 pid=6210
(ds) control_socket_pcb: 0xffffffdd37e7c000
(ds) pcbinfo_pointer: 0xfffffff06f8613c0
(ds) ipi_zone: 0xfffffff06ce95060
(ds) zv_name: 0xfffffff06c5e783e
(ds) searching for kernel Mach-O header from 0xfffffff06c5e4000...

(utils) socket found via pcb+0x28 = 0xfffffff06f856ba0 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffdd37e7c400 sock=0xfffffff06f856ba0)

(utils) LiveContainer self-proc lookup exhausted pid-based methods
(ds) candidate Mach-O at 0xfffffff06c584000: filetype=2 cpuinfo=0x2c0000002 (iter=24)
(ds) candidate Mach-O at 0xfffffff06c57c000: filetype=12 cpuinfo=0xc00000002 (iter=26)
(ds) found MH_FILESET header at 0xfffffff06c57c000
(ds) kernel_base:  0xfffffff06c57c000
(ds) kernel_slide: 0x65578000
(ds) iOS 17: using so_count offset 0x24c
(ds) kernel r/w is ready!
exploit success!
kernel_base:  0xfffffff06c57c000
kernel_slide: 0x65578000

(ds) our_proc: 0x0
(ds) our_task: 0x0

exploit success!
kernel_base:  0xfffffff06c57c000
kernel_slide: 0x65578000

(ds) starting darksword
(ds) device: iPhone14,5
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0x14
(ds) write_fd: 0x15
(ds) executable_path: /private/var/mobile/Containers/Data/Application/3E405DE4-7B64-4466-9CF9-A17D2B0A4844/Documents/Applications/com.roooot.laraapp.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/42AF2D77-5D94-4411-BBD8-8D204C229480/LiveContainer.app/LiveContainer
(ds) guest_executable_name: lara
(ds) host_executable_name: LiveContainer
(ds) kernel_process_name: LiveContainer
(ds) livecontainer_bundle: yes
(ds) livecontainer_guest: yes
(ds) rehosted_process: yes
(ds) process_marker[0]: LiveProcess
(ds) process_marker[1]: LiveContainer
(ds) process_marker[2]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x1175c4000
(ds) physical_mapping_address: 0x1175c8000
(ds) pc_object: 0x56fc07
(ds) pc_address: 0x3667c8000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x58b8e
(ds) end_pcb_id: 0x63b8c
(ds) looking in search mapping: 0
(ds) Matched PCB via process marker: LiveContainer
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x5ba5a
(ds) inp_list_next_pointer: 0xffffffdd3901c400
(ds) icmp6filter: 0xffffffe0cddcc9c0
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffdd3901c548
(ds) found control_socket at idx: 0x1766

(utils) kernel proc: 0x9f867ff06d8b0c20

(utils) kernel proc pointer invalid

(utils) socket fallback: pcb=0xffffffdd3901c400 pid=6210
exploit success!
kernel_base:  0xfffffff06c57c000
kernel_slide: 0x65578000

(utils) socket found via pcb+0x28 = 0xfffffff06f856ba0 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffdd3901c400 sock=0xfffffff06f856ba0)

(utils) LiveContainer self-proc lookup exhausted pid-based methods

(ds) highest_success_idx: 479
(ds) success_read_count: 75
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffdd3901c000
(ds) pcbinfo_pointer: 0xfffffff06f8613c0
(ds) ipi_zone: 0xfffffff06ce95060
(ds) zv_name: 0xfffffff06c5e783e
(ds) searching for kernel Mach-O header from 0xfffffff06c5e4000...
(ds) candidate Mach-O at 0xfffffff06c584000: filetype=2 cpuinfo=0x2c0000002 (iter=24)
(ds) candidate Mach-O at 0xfffffff06c57c000: filetype=12 cpuinfo=0xc00000002 (iter=26)
(ds) found MH_FILESET header at 0xfffffff06c57c000
(ds) kernel_base:  0xfffffff06c57c000
(ds) kernel_slide: 0x65578000
(ds) iOS 17: using so_count offset 0x24c
(ds) kernel r/w is ready!
(ds) our_proc: 0x0
(ds) our_task: 0x0

exploit success!
kernel_base:  0xfffffff06c57c000
kernel_slide: 0x65578000

(ds) starting darksword
(ds) device: iPhone14,5
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0x14
(ds) write_fd: 0x15
(ds) executable_path: /private/var/mobile/Containers/Data/Application/3E405DE4-7B64-4466-9CF9-A17D2B0A4844/Documents/Applications/com.roooot.laraapp.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/42AF2D77-5D94-4411-BBD8-8D204C229480/LiveContainer.app/LiveContainer
(ds) guest_executable_name: lara
(ds) host_executable_name: LiveContainer
(ds) kernel_process_name: LiveContainer
(ds) livecontainer_bundle: yes
(ds) livecontainer_guest: yes
(ds) rehosted_process: yes
(ds) process_marker[0]: LiveProcess
(ds) process_marker[1]: LiveContainer
(ds) process_marker[2]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x1175d4000
(ds) physical_mapping_address: 0x1175dc000
(ds) pc_object: 0x9d0b
(ds) pc_address: 0x31feb0000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x6eb8a
(ds) end_pcb_id: 0x79b88
(ds) looking in search mapping: 0
(ds) Matched PCB via process marker: LiveContainer
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x73b9c
(ds) inp_list_next_pointer: 0xffffffdd38870400
(ds) icmp6filter: 0xffffffe0ce1343c0
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffdd38870548
(ds) found control_socket at idx: 0x2809

(utils) kernel proc: 0x9f867ff06d8b0c20

(utils) kernel proc pointer invalid

(utils) socket fallback: pcb=0xffffffdd38870400 pid=6210

(utils) socket found via pcb+0x28 = 0xfffffff06f856ba0 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffdd38870400 sock=0xfffffff06f856ba0)

(utils) LiveContainer self-proc lookup exhausted pid-based methods
exploit success!
kernel_base:  0xfffffff06c57c000
kernel_slide: 0x65578000

(ds) highest_success_idx: 487
(ds) success_read_count: 152
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffdd38870000
(ds) pcbinfo_pointer: 0xfffffff06f8613c0
(ds) ipi_zone: 0xfffffff06ce95060
(ds) zv_name: 0xfffffff06c5e783e
(ds) searching for kernel Mach-O header from 0xfffffff06c5e4000...
(ds) candidate Mach-O at 0xfffffff06c584000: filetype=2 cpuinfo=0x2c0000002 (iter=24)
(ds) candidate Mach-O at 0xfffffff06c57c000: filetype=12 cpuinfo=0xc00000002 (iter=26)
(ds) found MH_FILESET header at 0xfffffff06c57c000
(ds) kernel_base:  0xfffffff06c57c000
(ds) kernel_slide: 0x65578000
(ds) iOS 17: using so_count offset 0x24c
(ds) kernel r/w is ready!
(ds) our_proc: 0x0
(ds) our_task: 0x0

exploit success!
kernel_base:  0xfffffff06c57c000
kernel_slide: 0x65578000

(ds) starting darksword
(ds) device: iPhone14,5
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0x14
(ds) write_fd: 0x15
(ds) executable_path: /private/var/mobile/Containers/Data/Application/3E405DE4-7B64-4466-9CF9-A17D2B0A4844/Documents/Applications/com.roooot.laraapp.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/42AF2D77-5D94-4411-BBD8-8D204C229480/LiveContainer.app/LiveContainer
(ds) guest_executable_name: lara
(ds) host_executable_name: LiveContainer
(ds) kernel_process_name: LiveContainer
(ds) livecontainer_bundle: yes
(ds) livecontainer_guest: yes
(ds) rehosted_process: yes
(ds) process_marker[0]: LiveProcess
(ds) process_marker[1]: LiveContainer
(ds) process_marker[2]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x117afc000
(ds) physical_mapping_address: 0x117b0c000
(ds) pc_object: 0x58e307
(ds) pc_address: 0x125250000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x84b86
(ds) end_pcb_id: 0x8fb84
(ds) looking in search mapping: 0
(ds) Matched PCB via process marker: LiveContainer
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x861ea
(ds) inp_list_next_pointer: 0xffffffdd39438400
(ds) icmp6filter: 0xffffffe0cdf97a60
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffdd39438548
(ds) found control_socket at idx: 0xb32

(utils) kernel proc: 0x9f867ff06d8b0c20

(utils) kernel proc pointer invalid

(utils) socket fallback: pcb=0xffffffdd39438400 pid=6210

(utils) socket found via pcb+0x28 = 0xfffffff06f856ba0 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffdd39438400 sock=0xfffffff06f856ba0)

(utils) LiveContainer self-proc lookup exhausted pid-based methods
exploit success!
kernel_base:  0xfffffff06c57c000
kernel_slide: 0x65578000

(ds) highest_success_idx: 487
(ds) success_read_count: 160
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffdd39438000
(ds) pcbinfo_pointer: 0xfffffff06f8613c0
(ds) ipi_zone: 0xfffffff06ce95060
(ds) zv_name: 0xfffffff06c5e783e
(ds) searching for kernel Mach-O header from 0xfffffff06c5e4000...
(ds) candidate Mach-O at 0xfffffff06c584000: filetype=2 cpuinfo=0x2c0000002 (iter=24)
(ds) candidate Mach-O at 0xfffffff06c57c000: filetype=12 cpuinfo=0xc00000002 (iter=26)
(ds) found MH_FILESET header at 0xfffffff06c57c000
(ds) kernel_base:  0xfffffff06c57c000
(ds) kernel_slide: 0x65578000
(ds) iOS 17: using so_count offset 0x24c
(ds) kernel r/w is ready!
(ds) our_proc: 0x0
(ds) our_task: 0x0

exploit success!
kernel_base:  0xfffffff06c57c000
kernel_slide: 0x65578000


downloading kernelcache → /var/mobile/Containers/Data/Application/3E405DE4-7B64-4466-9CF9-A17D2B0A4844/Documents/Data/Application/0D715F8C-CBB4-42E1-A77F-36DBD558FE04/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2023FallFCS/fullrestores/042-07806/68C80FC0-071A-46DD-ABFE-6A5BA2FCF162/iPhone14,5_17.1_21B74_Restore.ipsw (OTA: no)

libgrabkernel2: Downloading BuildManifest.plist...

libgrabkernel2: Downloading kernelcache.release.iphone14 to /var/mobile/Containers/Data/Application/3E405DE4-7B64-4466-9CF9-A17D2B0A4844/Documents/Data/Application/0D715F8C-CBB4-42E1-A77F-36DBD558FE04/Documents/kernelcache...

libgrabkernel2: Downloaded kernelcache!

xpf dict failed, continuing without offsets: Set "translation" failed on "kernelSymbol.cpu_ttep"

kernel: Darwin Kernel Version 23.1.0: Tue Oct 10 02:21:15 PDT 2023; root:xnu-10002.42.9~2/RELEASE_ARM64_T8110

kernbase: 0xfffffff027004000

kernentry: 0xfffffff02a058000

kernproc: 0x910520

rootvnode: 0x32e5900

procsize: 0x730

(utils) kernel proc: 0xffffffdfe7aec710

(utils) looking for pid: 6210

(utils) found proc: Container (pid=6210 uid=501 gid=501) @ 0xffffffdfe84897b0
(sbx) ds_get_our_proc() returned 0x0 — trying ourproc() fallback...
(sbx) proc=0xffffffdfe84897b0 proc_ro_raw=0xffffffdce8156200 proc_ro=0xffffffdce8156200
(sbx) scanning proc_ro for ucred...
(sbx)   proc_ro+0x10: raw=0x1842 smr=0x1842 pac=0x1842
(sbx)   proc_ro+0x18: raw=0x22002305000046ba smr=0x5000046ba pac=0x5000046ba
(sbx)   proc_ro+0x20: raw=0xffffffdce8020070 smr=0xffffffdce8020070 pac=0xffffffdce8020070
(sbx) found ucred at proc_ro+0x20 (SMR) = 0xffffffdce8020070
(sbx) ucred=0xffffffdce8020070 label=0xffffffdd35ea73a0 sandbox=0xffffffdd35f1ee78 ext_set=0xffffffdfe763c580
(sbx) patched 4 extensions
(sbx) changed 3 extension classes
(sbx) filled 12 empty hash slots
(sbx) escaped!

sandbox escape ready!


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


libgrabkernel2: Downloaded kernelcache!

xpf dict failed, continuing without offsets: Set "translation" failed on "kernelSymbol.cpu_ttep" (Set "translation" failed on "kernelSymbol.cpu_ttep")

kernel: Darwin Kernel Version 23.1.0: Tue Oct 10 02:21:15 PDT 2023; root:xnu-10002.42.9~2/RELEASE_ARM64_T8110

kernbase: 0xfffffff027004000

kernentry: 0xfffffff02a058000

kernproc: 0x910520

rootvnode: 0x32e5900

procsize: 0x730

kernelcache removed

downloading kernelcache → /var/mobile/Containers/Data/Application/3E405DE4-7B64-4466-9CF9-A17D2B0A4844/Documents/Data/Application/0D715F8C-CBB4-42E1-A77F-36DBD558FE04/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2023FallFCS/fullrestores/042-07806/68C80FC0-071A-46DD-ABFE-6A5BA2FCF162/iPhone14,5_17.1_21B74_Restore.ipsw (OTA: no)

libgrabkernel2: Downloading BuildManifest.plist...

libgrabkernel2: Downloading kernelcache.release.iphone14 to /var/mobile/Containers/Data/Application/3E405DE4-7B64-4466-9CF9-A17D2B0A4844/Documents/Data/Application/0D715F8C-CBB4-42E1-A77F-36DBD558FE04/Documents/kernelcache...

libgrabkernel2: Downloaded kernelcache!

xpf dict failed, continuing without offsets: Set "translation" failed on "kernelSymbol.cpu_ttep" (Set "translation" failed on "kernelSymbol.cpu_ttep" (Set "translation" failed on "kernelSymbol.cpu_ttep"))

kernel: Darwin Kernel Version 23.1.0: Tue Oct 10 02:21:15 PDT 2023; root:xnu-10002.42.9~2/RELEASE_ARM64_T8110

kernbase: 0xfffffff027004000

kernentry: 0xfffffff02a058000

kernproc: 0x910520

rootvnode: 0x32e5900

procsize: 0x730

(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


(utils) looking for process: launchd

(utils) process 'launchd' not found
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


---

## Timeline (Verbose)

- Apr 14, 2026: 🔴 Closed by @Pro42good


---

## Comments (4)

### Comment #1 — [@Tobim6](https://github.com/Tobim6) · Apr 12, 2026

`ID: 4232032075`


@m1337v


---

### Comment #2 — [@m1337v](https://github.com/m1337v) · Apr 12, 2026

`ID: 4232059677`


Please try this version @Tobim6 and let me know if it works or attach a new log
https://github.com/m1337v/lara/releases/download/release/laralc.ipa


---

### Comment #3 — [@Tobim6](https://github.com/Tobim6) · Apr 12, 2026

`ID: 4232213357`


> Please try this version 

Works.


👍 1


---

### Comment #4 — [@Pro42good](https://github.com/Pro42good) · Apr 14, 2026

`ID: 4246885542`


This should be fixed, so I'm closing this issue. any new comments can re-open this issue thread.


---

*Exported on 10/14/2026, 2:53:01 PM from [https://github.com/rooootdev/lara/issues/85](https://github.com/rooootdev/lara/issues/85)*
