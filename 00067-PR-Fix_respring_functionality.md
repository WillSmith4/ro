# Fix respring functionality

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#67](https://github.com/rooootdev/lara/pull/67)  
> **State:** 🔴 closed  
> **Branch:** `neonmodder123:main` → `rooootdev:main`  
> **Changes:** +58 / -154 across 10 files  
> **Created:** Apr 10, 2026  
> **Updated:** Apr 10, 2026  
> **Closed:** Apr 10, 2026  

---

## Description

*Opened by [@neonmodder123](https://github.com/neonmodder123)* *on Apr 10, 2026*


Fix respring functionality by opening my respring page and add credits to me :)


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (11)

- [`e43a370`](https://github.com/rooootdev/lara/commit/e43a37058427efe53b0a1725a068953f77ebef3c) Delete lara/kexploit/respring.m — *neon* · Apr 10, 2026
- [`d652996`](https://github.com/rooootdev/lara/commit/d652996e6fc00a06b87c72f725cd5fe4708289fb) Delete lara/kexploit/respring.h — *neon* · Apr 10, 2026
- [`0a02941`](https://github.com/rooootdev/lara/commit/0a0294154a52bad91df4877e31bc9e3dca21e862) Create respring.swift — *neon* · Apr 10, 2026
- [`e90bc9c`](https://github.com/rooootdev/lara/commit/e90bc9cba4ef6831dc0b8b309df1b6760981f5a3) Update laramgr.swift — *neon* · Apr 10, 2026
- [`16e7421`](https://github.com/rooootdev/lara/commit/16e7421b35b9a7cff0deb083b64f2ed870b4174c) Update ContentView.swift — *neon* · Apr 10, 2026
- [`9dc227e`](https://github.com/rooootdev/lara/commit/9dc227e80789668d71a011dc206cd5fbd48361a1) Update EditorView.swift — *neon* · Apr 10, 2026
- [`a7e35b7`](https://github.com/rooootdev/lara/commit/a7e35b74a08bd66fc28c64951a34cd4b8771ad33) Update FontPicker.swift — *neon* · Apr 10, 2026
- [`783e91f`](https://github.com/rooootdev/lara/commit/783e91f606f52539aae230b2c845e388e3a4f3c6) Update SettingsView.swift — *neon* · Apr 10, 2026
- [`3173f2d`](https://github.com/rooootdev/lara/commit/3173f2d42a84e8b246a2f3f5f95d9c1206fd2d0a) Update ToolsView.swift — *neon* · Apr 10, 2026
- [`e5097f0`](https://github.com/rooootdev/lara/commit/e5097f0a2b96e3e3f5d4f74579b646b2eb9a830e) Update lara-Bridging-Header.h — *neon* · Apr 10, 2026
- [`21596a4`](https://github.com/rooootdev/lara/commit/21596a4fbdbae6d799391c69ecac7f61a747ca06) Update SettingsView.swift — *neon* · Apr 10, 2026


---

## Changed Files (10)

- 🟡 `lara/classes/laramgr.swift` (modified) — +0 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -132,11 +132,6 @@ final class laramgr: ObservableObject {
         }
     }
     
-    func respring() {
-        notify_post("com.apple.springboard.toggleLockScreen")
-        // killproc("springboard")
-    }
-    
     func vfsinit(completion: ((Bool) -> Void)? = nil) {
         vfs_setlogcallback(laramgr.vfslogcallback)
         vfs_setprogresscallback { progress in
  ```

  </details>
- 🟢 `lara/classes/respring.swift` (added) — +21 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,21 @@
+//
+//  respring.swift
+//  lara
+//
+//  Created by neonmodder123 on 10.04.26.
+//
+
+import SwiftUI
+import SafariServices
+
+struct respring {
+    static func showRespringPage() {
+        guard let url = URL(string: "https://neonmodder123.github.io/respring") else { return }
+        let safariVC = SFSafariViewController(url: url)
+        safariVC.modalPresentationStyle = .overFullScreen
+        if let windowScene = UIApplication.shared.connectedScenes.first as? UIWindowScene,
+           let rootVC = windowScene.windows.first?.rootViewController {
+            rootVC.present(safariVC, animated: true)
+        }
+    }
+}
  ```

  </details>
- 🔴 `lara/kexploit/respring.h` (removed) — +0 / -14
  <details><summary>Diff</summary>

  ```diff
  @@ -1,14 +0,0 @@
-//
-//  respring.h
-//  lara
-//
-//  Created by ruter on 06.04.26.
-//
-
-#ifndef respring_h
-#define respring_h
-
-void respringBackboard(void);
-void respringFrontboard(void);
-
-#endif /* respring_h */
  ```

  </details>
- 🔴 `lara/kexploit/respring.m` (removed) — +0 / -122
  <details><summary>Diff</summary>

  ```diff
  @@ -1,122 +0,0 @@
-//
-//  respring.m
-//  lara
-//
-//  Created by ruter on 06.04.26.
-//
-
-#import <Foundation/Foundation.h>
-#include <string.h>
-#include <mach/mach.h>
-#include <dirent.h>
-#include "respring.h"
-
-// Credit to Ian Beer
-
-kern_return_t
-bootstrap_look_up(mach_port_t bp, const char* service_name, mach_port_t *sp);
-
-struct xpc_w00t {
-  mach_msg_header_t hdr;
-  mach_msg_body_t body;
-  mach_msg_port_descriptor_t client_port;
-  mach_msg_port_descriptor_t reply_port;
-};
-
-mach_port_t get_send_once(mach_port_t recv) {
-  mach_port_t so = MACH_PORT_NULL;
-  mach_msg_type_name_t type = 0;
-  kern_return_t err = mach_port_extract_right(mach_task_self(), recv, MACH_MSG_TYPE_MAKE_SEND_ONCE, &so, &type);
-  if (err != KERN_SUCCESS) {
-    printf("port right extraction failed: %s\n", mach_error_string(err));
-    return MACH_PORT_NULL;
-  }
-  printf("made so: 0x%x from recv: 0x%x\n", so, recv);
-  return so;
-}
-
-void xpc_crasher(char* service_name) {
-  mach_port_t client_port = MACH_PORT_NULL;
-  mach_port_t reply_port = MACH_PORT_NULL;
-
-  mach_port_t service_port = MACH_PORT_NULL;
-
-  kern_return_t err = bootstrap_look_up(bootstrap_port, service_name, &service_port);
-  if(err != KERN_SUCCESS){
-    printf("unable to look up %s\n", service_name);
-    return;
-  }
-
-  if (service_port == MACH_PORT_NULL) {
-    printf("bad service port\n");
-    return;
-  }
-
-  // allocate the client and reply port:
-  err = mach_port_allocate(mach_task_self(), MACH_PORT_RIGHT_RECEIVE, &client_port);
-  if (err != KERN_SUCCESS) {
-    printf("port allocation failed: %s\n", mach_error_string(err));
-    return;
-  }
-
-  mach_port_t so0 = get_send_once(client_port);
-  mach_port_t so1 = get_send_once(client_port);
-
-  // insert a send so we maintain the ability to send to this port
-  err = mach_port_insert_right(mach_task_self(), client_port, client_port, MACH_MSG_TYPE_MAKE_SEND);
-  if (err != KERN_SUCCESS) {
-    printf("port right insertion failed: %s\n", mach_error_string(err));
-    return;
-  }
-
-  err = mach_port_allocate(mach_task_self(), MACH_PORT_RIGHT_RECEIVE, &reply_port);
-  if (err != KERN_SUCCESS) {
-    printf("port allocation failed: %s\n", mach_error_string(err));
-    return;
-  }
-
-  struct xpc_w00t msg;
-  memset(&msg.hdr, 0, sizeof(msg));
-  msg.hdr.msgh_bits = MACH_MSGH_BITS_SET(MACH_MSG_TYPE_COPY_SEND, 0, 0, MACH_MSGH_BITS_COMPLEX);
-  msg.hdr.msgh_size = sizeof(msg);
-  msg.hdr.msgh_remote_port = service_port;
-  msg.hdr.msgh_id   = 'w00t';
-
-  msg.body.msgh_descriptor_count = 2;
-
-  msg.client_port.name        = client_port;
-  msg.client_port.disposition = MACH_MSG_TYPE_MOVE_RECEIVE; // we still keep the send
-  msg.client_port.type        = MACH_MSG_PORT_DESCRIPTOR;
-
-  msg.reply_port.name        = reply_port;
-  msg.reply_port.disposition = MACH_MSG_TYPE_MAKE_SEND;
-  msg.reply_port.type        = MACH_MSG_PORT_DESCRIPTOR;
-
-  err = mach_msg(&msg.hdr,
-                 MACH_SEND_MSG|MACH_MSG_OPTION_NONE,
-                 msg.hdr.msgh_size,
-                 0,
-                 MACH_PORT_NULL,
-                 MACH_MSG_TIMEOUT_NONE,
-                 MACH_PORT_NULL);
-
-  if (err != KERN_SUCCESS) {
-    printf("w00t message send failed: %s\n", mach_error_string(err));
-    return;
-  } else {
-    printf("sent xpc w00t message\n");
-  }
-
-  mach_port_deallocate(mach_task_self(), so0);
-  mach_port_deallocate(mach_task_self(), so1);
-
-  return;
-}
-
-void respringBackboard(void) {
-  xpc_crasher("com.apple.backboard.TouchDeliveryPolicyServer");
-}
-
-void respringFrontboard(void) {
-  xpc_crasher("com.apple.frontboard.systemappservices");
-}
  ```

  </details>
- 🟡 `lara/lara-Bridging-Header.h` (modified) — +0 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -6,7 +6,6 @@
 #import <Foundation/Foundation.h>
 
 #import "darksword.h"
-#import "respring.h"
 #import "offsets.h"
 #import "utils.h"
 #import "apfs.h"
  ```

  </details>
- 🟡 `lara/views/ContentView.swift` (modified) — +2 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -357,10 +357,8 @@ struct ContentView: View {
                             }
                         }
                         
-                        if #unavailable(iOS 18.2) {
-                            Button("Respring") {
-                                mgr.respring()
-                            }
+                        Button("Respring (needs internet)") {
+                            respring.showRespringPage()
                         }
                         
                         Button("Panic!") {
  ```

  </details>
- 🟡 `lara/views/EditorView.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -130,7 +130,7 @@ struct EditorView: View {
             }
             .alert("Done", isPresented: .constant(alert != nil)) {
                 Button("Cancel") { alert = nil }
-                Button("Respring") { mgr.respring() }
+                Button("Respring (needs internet)") { respring.showRespringPage() }
             } message: {
                 Text(alert ?? "uhh...")
             }
  ```

  </details>
- 🟡 `lara/views/FontPicker.swift` (modified) — +2 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -91,10 +91,8 @@ struct FontPicker: View {
                     Text(globallogger.logs.last ?? "No logs yet")
                         .font(.system(size: 13, design: .monospaced))
                     
-                    if #unavailable(iOS 18.2) {
-                        Button("Respring") {
-                            mgr.respring()
-                        }
+                    Button("Respring (needs internet)") {
+                        respring.showRespringPage()
                     }
                 }
             }
  ```

  </details>
- 🟡 `lara/views/SettingsView.swift` (modified) — +30 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -247,6 +247,36 @@ struct SettingsView: View {
                             UIApplication.shared.open(url)
                         }
                     }
+
+                    HStack(alignment: .top) {
+                        AsyncImage(url: URL(string: "https://github.com/neonmodder123.png")) { image in
+                            image
+                                .resizable()
+                                .scaledToFill()
+                        } placeholder: {
+                            ProgressView()
+                        }
+                        .frame(width: 40, height: 40)
+                        .clipShape(Circle())
+                        
+                        VStack(alignment: .leading) {
+                            Text("neonmodder123")
+                                .font(.headline)
+                            
+                            Text("Fixed respring functionality.")
+                                .font(.subheadline)
+                                .foregroundColor(Color.secondary)
+                        }
+                        
+                        Spacer()
+                    }
+                    .onTapGesture {
+                        if let url = URL(string: "https://github.com/neonmodder123"),
+                           UIApplication.shared.canOpenURL(url) {
+                            UIApplication.shared.open(url)
+                        }
+                    }
+                    
                 } header: {
                     Text("Credits")
                 }
  ```

  </details>
- 🟡 `lara/views/ToolsView.swift` (modified) — +2 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -69,10 +69,9 @@ struct ToolsView: View {
             
             Section {
                 Button {
-                    globallogger.log(String(format: "0x%llx",procbyname("springboard")))
-                    // killproc("springboard")
+                    respring.showRespringPage()
                 } label: {
-                    Text("Respring (probably broken)")
+                    Text("Respring (needs internet)")
                 }
                 
                 HStack {
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit e43a370: Delete lara/kexploit/respring.m
- : 📝 Commit d652996: Delete lara/kexploit/respring.h
- : 📝 Commit 0a02941: Create respring.swift
- : 📝 Commit e90bc9c: Update laramgr.swift
- : 📝 Commit 16e7421: Update ContentView.swift
- : 📝 Commit 9dc227e: Update EditorView.swift
- : 📝 Commit a7e35b7: Update FontPicker.swift
- : 📝 Commit 783e91f: Update SettingsView.swift
- : 📝 Commit 3173f2d: Update ToolsView.swift
- : 📝 Commit e5097f0: Update lara-Bridging-Header.h
- : 📝 Commit 21596a4: Update SettingsView.swift
- Apr 10, 2026: 🔴 Closed by @neonmodder123


---

## Comments (1)

### Comment #1 — [@neonmodder123](https://github.com/neonmodder123) · Apr 10, 2026

`ID: 4226003279`


Thank you for adding this feature!


---

*Exported on 10/14/2026, 2:51:45 PM from [https://github.com/rooootdev/lara/pull/67](https://github.com/rooootdev/lara/pull/67)*
