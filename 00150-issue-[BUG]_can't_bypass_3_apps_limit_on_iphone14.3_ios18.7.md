# [BUG] can't bypass 3 apps limit on iphone14.3 ios18.7

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#150](https://github.com/rooootdev/lara/issues/150)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Labels:** `bug`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 21, 2026  
> **Updated:** Apr 21, 2026  
> **Closed:** Apr 21, 2026  

---

## Description

*Opened by [@Auox](https://github.com/Auox)* *on Apr 21, 2026*


lara started: 2026-04-21 14:06:36

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0xb8

(utils) PROC_PID_OFFSET: 0x60

(offs) loaded saved t1sz_boot: 0x19

(offs) t1sz_boot: 0x19

(offs) saved t1sz_boot: 0x19

initialized offsets

(offs) loaded saved t1sz_boot: 0x19

(offs) t1sz_boot: 0x19

(offs) saved t1sz_boot: 0x19

initialized offsets

(ds) starting darksword
(ds) device: iPhone14,3
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0xa
(ds) write_fd: 0xb
(ds) executable_path: /private/var/containers/Bundle/Application/EFDF4B9D-630E-4E30-AEB3-C14541E5E295/App.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/EFDF4B9D-630E-4E30-AEB3-C14541E5E295/App.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name: lara
(ds) kernel_process_name: lara
(ds) livecontainer_bundle: no
(ds) livecontainer_guest: no
(ds) rehosted_process: no
(ds) process_marker[0]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x112180000
(ds) physical_mapping_address: 0x1111e8000
(ds) pc_object: 0x660b
(ds) pc_address: 0x39d230000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0xc6658
(ds) end_pcb_id: 0xd1656
(ds) looking in search mapping: 0
(ds) Matched PCB via process marker: lara
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0xcece6
(ds) inp_list_next_pointer: 0xffffffe8bb234400
(ds) icmp6filter: 0xffffffebbb602b20
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffe8bb234548
(ds) found control_socket at idx: 0x4347

(utils) socket fallback: pcb=0xffffffe8bb234400 pid=4425

(utils) socket found via pcb+0x28 = 0xfffffff0460c9468 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffe8bb234400 sock=0xfffffff0460c9468)
(ds) exploit success!
(ds) kernel_base:  0xfffffff042a4c000
(ds) kernel_slide: 0x3ba48000

(ds) highest_success_idx: 497
(ds) success_read_count: 353
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffe8bb234000
(ds) pcbinfo_pointer: 0xfffffff0460d70e0
(ds) ipi_zone: 0xfffffff0434cc840
(ds) zv_name: 0xfffffff042abccb8
(ds) searching for kernel Mach-O header from 0xfffffff042abc000...
(ds) candidate Mach-O at 0xfffffff042a54000: filetype=2 cpuinfo=0x2c0000002 (iter=26)
(ds) candidate Mach-O at 0xfffffff042a4c000: filetype=12 cpuinfo=0xc00000002 (iter=28)
(ds) found MH_FILESET header at 0xfffffff042a4c000
(ds) kernel_base:  0xfffffff042a4c000
(ds) kernel_slide: 0x3ba48000
(ds) iOS 18: using so_count offset 0x254
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffe7da7e7220
(ds) our_task: 0xffffffe7da7e7960

(ds) exploit success!
(ds) kernel_base:  0xfffffff042a4c000
(ds) kernel_slide: 0x3ba48000

(sbx) proc=0xffffffe7da7e7220 proc_ro_raw=0xffffffe6a401e9b0 proc_ro=0xffffffe6a401e9b0
(sbx) scanning proc_ro for ucred...
(sbx)   proc_ro+0x10: raw=0x1149 smr=0x1149 pac=0x1149
(sbx)   proc_ro+0x18: raw=0x100002afd smr=0x100002afd pac=0x100002afd
(sbx)   proc_ro+0x20: raw=0x2200230500000001 smr=0x500000001 pac=0x500000001
(sbx)   proc_ro+0x28: raw=0xffffffe6f1fb3550 smr=0xffffffe6f1fb3550 pac=0xffffffe6f1fb3550
(sbx) found ucred at proc_ro+0x28 (SMR) = 0xffffffe6f1fb3550
(sbx) ucred=0xffffffe6f1fb3550 label=0xffffffe6f1fda8c0 sandbox=0xffffffe6a418c700 ext_set=0xffffffe6f3eb8640
(sbx) patched 5 extensions
(sbx) changed 2 extension classes
(sbx) filled 12 empty hash slots
(sbx) escaped!

sandbox escape ready!


(utils) socket fallback: pcb=0xffffffe8bb234400 pid=4425

(utils) socket found via pcb+0x28 = 0xfffffff0460c9468 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffe8bb234400 sock=0xfffffff0460c9468)
(vfs) vfs_init starting...
(vfs) heap PAC prefix: 0xffffff0000000000
(vfs) proc=0xffffffe7da7e7220 task=0xffffffe7da7e7960 (from exploit)
(vfs) file overwrite ready!
(vfs) g_rootvnode = 0xffffffe7d79bc000
(vfs) first_nc = 0xffffffe7d532d2e8
(vfs) nc_vp = 0xffffffe7d79bc000
(vfs) nc_name = 0xfdcc7ae4
(vfs) nc_name offset probe: 0x60
(vfs) ncache ok: first child='.tmp.Oy3bgE90Bp'
(vfs) vfs_init done

vfs ready!


changed owner of /private/var/containers/Bundle/Application/EFDF4B9D-630E-4E30-AEB3-C14541E5E295/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/C78B587C-11CA-411E-9C11-D223802AD50A/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B6AFF727-9091-4913-9110-793A5E1C7EDE/LiveContainer.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/EFDF4B9D-630E-4E30-AEB3-C14541E5E295/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/EFDF4B9D-630E-4E30-AEB3-C14541E5E295/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/EFDF4B9D-630E-4E30-AEB3-C14541E5E295/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/C78B587C-11CA-411E-9C11-D223802AD50A/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/C78B587C-11CA-411E-9C11-D223802AD50A/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/C78B587C-11CA-411E-9C11-D223802AD50A/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B6AFF727-9091-4913-9110-793A5E1C7EDE/LiveContainer.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B6AFF727-9091-4913-9110-793A5E1C7EDE/LiveContainer.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B6AFF727-9091-4913-9110-793A5E1C7EDE/LiveContainer.app size=3
(sbx) processed 3 app(s)


---

## Timeline (Verbose)

- Apr 21, 2026: 👤 @Auox assigned @Pro42good
- Apr 21, 2026: 🏷️ @Auox added label `bug`
- Apr 21, 2026: 🔴 Closed by @rooootdev


---

## Comments (1)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Apr 21, 2026

`ID: 4287094764`


looks like everthing works. an indicator has been added in latest


---

*Exported on 10/14/2026, 2:58:25 PM from [https://github.com/rooootdev/lara/issues/150](https://github.com/rooootdev/lara/issues/150)*
