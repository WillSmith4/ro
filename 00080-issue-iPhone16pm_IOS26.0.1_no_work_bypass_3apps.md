# iPhone16pm IOS26.0.1 no work bypass 3apps

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#80](https://github.com/rooootdev/lara/issues/80)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Apr 11, 2026  
> **Updated:** Apr 16, 2026  
> **Closed:** Apr 16, 2026  

---

## Description

*Opened by [@Servant9527](https://github.com/Servant9527)* *on Apr 11, 2026*


"Bypass 3 app limit"Button click has no response; verified that it's impossible to install over 3 apps simultaneously:
logs:

lara started: 2026-04-11 21:52:55


livecontainer detected: nah

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0xb8

(utils) PROC_PID_OFFSET: 0x60

(utils) PROC_STRUCT_SIZE: 0x748
(ds) starting darksword
(ds) device: iPhone17,2
(ds) ispac: yes
(ds) running on a18 device
(ds) read_fd: 0xa
(ds) write_fd: 0xb
(ds) executable_name: lara
(ds) free_thread_arg: 0x10fcf8000
(ds) physical_mapping_address: 0x1067b4000
(ds) pc_object: 0x5023
(ds) pc_address: 0x37374c000
(ds) allocating wired memory (131072 entries)...
(ds) allocating wired memory done
(ds) seeking_offset: 0xf8000: Found wired_page: 0x12c7ac000
(ds) seeking_offset: 0x7c000: Found wired_page: 0x148738000
(ds) seeking_offset: 0x68000: Found wired_page: 0x12d9f0000
(ds) seeking_offset: 0x80000: Found wired_page: 0x302930000
(ds) seeking_offset: 0x54000: Found wired_page: 0x148c10000
(ds) seeking_offset: 0x4000: Found wired_page: 0x15b334000
(ds) seeking_offset: 0x8000: Found wired_page: 0x1599d0000
(ds) seeking_offset: 0x8c000: Found wired_page: 0x300e30000
(ds) waiting for zone trimming...
(ds) seeking_offset: 0x30000: Found wired_page: 0x301730000
(ds) seeking_offset: 0x110000: Found wired_page: 0x1308b0000
(ds) seeking_offset: 0x0: Found wired_page: 0x309944000
(ds) seeking_offset: 0x64000: Found wired_page: 0x128e30000
(ds) seeking_offset: 0x58000: Found wired_page: 0x160b48000
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x270dc
(ds) inp_list_next_pointer: 0xffffffe234850400
(ds) icmp6filter: 0xffffffe4e0883ee0
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffe234850548
(ds) found control_socket at idx: 0x306b
(ds) seeking_offset: 0x58000: Reallocated PCB page

initialized offsetsinitialized offsets(utils) kernel proc: 0xffffffe22dd73220

(utils) looking for pid: 572

(utils) found proc: lara (pid=572 uid=501 gid=501) @ 0xffffffe22b7bd100
(ds) highest_success_idx: 494
exploit success!
kernel_base:  0xfffffff052c4c000
kernel_slide: 0x4bc48000

(ds) success_read_count: 81
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffe234854000
(ds) pcbinfo_pointer: 0xfffffff056f42af0
(ds) ipi_zone: 0xfffffff0538fe950
(ds) zv_name: 0xfffffff052cc865c
(ds) searching for kernel Mach-O header from 0xfffffff052cc8000...
(ds) candidate Mach-O at 0xfffffff052c54000: filetype=2 cpuinfo=0x2c0000002 (iter=29)
(ds) candidate Mach-O at 0xfffffff052c4c000: filetype=12 cpuinfo=0xcc0000002 (iter=31)
(ds) found MH_FILESET header at 0xfffffff052c4c000
(ds) kernel_base:  0xfffffff052c4c000
(ds) kernel_slide: 0x4bc48000
(ds) iOS 26: using so_count offset 0x23c
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffe22b7bd100
(ds) our_task: 0xffffffe22b7bd848

exploit success!
kernel_base:  0xfffffff052c4c000
kernel_slide: 0x4bc48000

(sbx) proc=0xffffffe22b7bd100 proc_ro_raw=0xffffffe147fbc150 proc_ro=0xffffffe147fbc150
(sbx) scanning proc_ro for ucred...
(sbx)   proc_ro+0x10: raw=0x23c smr=0x23c pac=0x23c
(sbx)   proc_ro+0x18: raw=0x10000062c smr=0x10000062c pac=0x10000062c
(sbx)   proc_ro+0x20: raw=0x2200230500000001 smr=0x500000001 pac=0x500000001
(sbx)   proc_ro+0x28: raw=0xffffffe147fdcb20 smr=0xffffffe147fdcb20 pac=0xffffffe147fdcb20
(sbx) found ucred at proc_ro+0x28 (SMR) = 0xffffffe147fdcb20
(sbx) ucred=0xffffffe147fdcb20 label=0xffffffe0fa10e0a0 sandbox=0xffffffe0fa0ebb20 ext_set=0xffffffe4dee959c0
(sbx) patched 5 extensions
(sbx) changed 2 extension classes
(sbx) filled 12 empty hash slots
(sbx) escaped!

sandbox escape ready!


changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/0ACA6F7A-7D5D-4B66-B484-ECCDCF7CB3EB/App.app size=3
(sbx) processed 3 app(s)

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!

changed owner of /private/var/containers/Bundle/Application/5BDAF604-9593-400E-BE79-0A8F42981B8B/App.app to 501:501!

changed owner of /private/var/containers/Bundle/Application/B7735453-DEA9-4394-A10E-4A128A84D79C/App.app to 501:501!

(ds) setsockopt failed (set_target_kaddr)! (2x)
(ds) setsockopt failed (early_kwrite32bytes)!


---

## Timeline (Verbose)

- Apr 16, 2026: 🔴 Closed by @rooootdev


---

## Comments (4)

### Comment #1 — [@Pro42good](https://github.com/Pro42good) · Apr 11, 2026

`ID: 4230116474`


It's a known issue that's being worked on.


---

### Comment #2 — [@ctrssss](https://github.com/ctrssss) · Apr 12, 2026

`ID: 4231002715`


on ios 17 fails to bypass too


---

### Comment #3 — [@proxer05](https://github.com/proxer05) · Apr 12, 2026

`ID: 4231315774`


After restarting Lara and starting exploit again it starts to work here(iphone14,2 ios18.7.1)


---

### Comment #4 — [@willieb2006](https://github.com/willieb2006) · Apr 15, 2026

`ID: 4248549753`


Working fine for me 15 pro max 26.0.1

<img width="645" height="1398" alt="Image" src="https://github.com/user-attachments/assets/aec7b594-f600-438f-bbef-0e70f9d36d3f" />


---

*Exported on 10/14/2026, 2:52:39 PM from [https://github.com/rooootdev/lara/issues/80](https://github.com/rooootdev/lara/issues/80)*
