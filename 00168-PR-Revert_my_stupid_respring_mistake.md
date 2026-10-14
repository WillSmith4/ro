# Revert my stupid respring mistake

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#168](https://github.com/rooootdev/lara/pull/168)  
> **State:** 🟣 merged  
> **Branch:** `neonmodder123:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 22, 2026  
> **Changes:** +82 / -127 across 9 files  
> **Created:** Apr 22, 2026  
> **Updated:** Apr 22, 2026  
> **Closed:** Apr 22, 2026  

---

## Description

*Opened by [@neonmodder123](https://github.com/neonmodder123)* *on Apr 22, 2026*


the title

also 2 syncs with your repo


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24782517467/job/72517475337) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24782517467/job/72517300261) — **success** — *GitHub Actions*


---

## PR Commits (12)

- [`07a132d`](https://github.com/rooootdev/lara/commit/07a132d299f36ca9cea7f8d4eed4bda2fe3bdc64) fix laramgr — *neon* · Apr 22, 2026
- [`de7f431`](https://github.com/rooootdev/lara/commit/de7f431bf46b40ac6e52e6674308b77bec8941ba) fix contentview — *neon* · Apr 22, 2026
- [`ee1af97`](https://github.com/rooootdev/lara/commit/ee1af97cc04b6b18fd91469ac5d1037964c7580b) fix editorview — *neon* · Apr 22, 2026
- [`e8e8ea1`](https://github.com/rooootdev/lara/commit/e8e8ea1d212b2a59759eba3c1a6dd905a3dd28fe) fix fontpicker — *neon* · Apr 22, 2026
- [`93ba28c`](https://github.com/rooootdev/lara/commit/93ba28c3e7f0e8686fc818dc0034c2425c084d14) remove skadz from credits — *neon* · Apr 22, 2026
- [`6d3e32b`](https://github.com/rooootdev/lara/commit/6d3e32bf726c21afd04ce3b7a53931373605e74c) fix toolsview — *neon* · Apr 22, 2026
- [`f86ed03`](https://github.com/rooootdev/lara/commit/f86ed03bbabd834b7f0773becb6f551219c06ea8) Create fileport.h — *neon* · Apr 22, 2026
- [`f2d09f5`](https://github.com/rooootdev/lara/commit/f2d09f5bad74ca829864b3b9be9fb04e89e6b883) Update persistence.m — *neon* · Apr 22, 2026
- [`4ee0d0b`](https://github.com/rooootdev/lara/commit/4ee0d0b541e7ce0f66cac74708c2f25c865270c4) fix jitview — *neon* · Apr 22, 2026
- [`0c18fec`](https://github.com/rooootdev/lara/commit/0c18fecdfce123a0f6832d2a2aa9721dcd1ff2f8) Merge branch 'rooootdev:main' into main — *neon* · Apr 22, 2026
- [`55cc6fb`](https://github.com/rooootdev/lara/commit/55cc6fb58dbdae395be94a32ab79c24633fb5adc) Merge branch 'rooootdev:main' into main — *neon* · Apr 22, 2026
- [`6e7b119`](https://github.com/rooootdev/lara/commit/6e7b1195b4809c21d6bd0b6b4af1efd11cc59907) final jitview fix — *neon* · Apr 22, 2026


---

## Changed Files (9)

- 🟡 `lara/classes/laramgr.swift` (modified) — +0 / -52
  <details><summary>Diff</summary>

  ```diff
  @@ -10,58 +10,6 @@ import Foundation
 import Darwin
 import notify
 import SafariServices
-import SwiftUI
-import UIKit
-import WebKit
-
-let respringDocument = """
-<!DOCTYPE html>
-<html>
-    <body>
-        <iframe id="frame" srcdoc="" sandbox="allow-forms allow-modals allow-orientation-lock allow-pointer-lock allow-popups allow-presentation allow-scripts"></iframe>
-        <script>
-            const frame = document.getElementById('frame');
-            const respringScript = `
-                <html>
-                <body>
-                    <script>
-                        const container = document.createElement('div');
-                        container.style.cssText = 'perspective: 1px; perspective-origin: 9999999% 9999999%;';
-                        document.body.appendChild(container);
-    
-                        for (let i = 0; i < 500; i++) {
-                            let d = document.createElement('div');
-                            d.style.cssText = 'position: absolute; width: 100vw; height: 100vh; backdrop-filter: blur(100px); -webkit-backdrop-filter: blur(100px); transform: translate3d(100000px, 100000px, ' + i + 'px) rotateY(90deg);';
-                            container.appendChild(d);
-                        }
-    
-                        setInterval(() => {
-                            navigator.share({ title: 'R', text: 'R'.repeat(100000) }).catch(() => {});
-                            let x = new Uint8Array(1024 * 1024 * 10);
-                            crypto.getRandomValues(x);
-                        }, 0);
-                    <\\/script>
-                </body>
-                </html>
-            `;
-    
-            frame.srcdoc = respringScript;
-        </script>
-    </body>
-</html>
-"""
-
-struct RespringView: UIViewRepresentable {
-    func makeUIView(context: Context) -> WKWebView {
-        let webView = WKWebView()
-        WKWebpagePreferences().allowsContentJavaScript = true
-        return webView
-    }
-
-    func updateUIView(_ webView: WKWebView, context: Context) {
-        webView.loadHTMLString(respringDocument, baseURL: nil)
-    }
-}
 
 final class laramgr: ObservableObject {
     @Published var log: String = ""
  ```

  </details>
- 🟢 `lara/headers/fileport.h` (added) — +56 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,56 @@
+/*
+ * Copyright (c) 2010 Apple Inc. All rights reserved.
+ *
+ * @APPLE_OSREFERENCE_LICENSE_HEADER_START@
+ * 
+ * This file contains Original Code and/or Modifications of Original Code
+ * as defined in and that are subject to the Apple Public Source License
+ * Version 2.0 (the 'License'). You may not use this file except in
+ * compliance with the License. The rights granted to you under the License
+ * may not be used to create, or enable the creation or redistribution of,
+ * unlawful or unlicensed copies of an Apple operating system, or to
+ * circumvent, violate, or enable the circumvention or violation of, any
+ * terms of an Apple operating system software license agreement.
+ * 
+ * Please obtain a copy of the License at
+ * http://www.opensource.apple.com/apsl/ and read it before using this file.
+ * 
+ * The Original Code and all software distributed under the License are
+ * distributed on an 'AS IS' basis, WITHOUT WARRANTY OF ANY KIND, EITHER
+ * EXPRESS OR IMPLIED, AND APPLE HEREBY DISCLAIMS ALL SUCH WARRANTIES,
+ * INCLUDING WITHOUT LIMITATION, ANY WARRANTIES OF MERCHANTABILITY,
+ * FITNESS FOR A PARTICULAR PURPOSE, QUIET ENJOYMENT OR NON-INFRINGEMENT.
+ * Please see the License for the specific language governing rights and
+ * limitations under the License.
+ * 
+ * @APPLE_OSREFERENCE_LICENSE_HEADER_END@
+ */
+
+#ifndef _SYS_FILEPORT_H_
+#define _SYS_FILEPORT_H_
+
+#include <sys/_types.h>
+#include <sys/cdefs.h>
+
+#ifndef KERNEL
+
+__BEGIN_DECLS
+
+#if !defined(_POSIX_C_SOURCE) || defined(_DARWIN_C_SOURCE)
+
+#ifndef _FILEPORT_T
+#define _FILEPORT_T
+typedef __darwin_mach_port_t fileport_t;
+#define FILEPORT_NULL	((fileport_t)0)
+#endif /* _FILEPORT_T */
+
+int	fileport_makeport(int, fileport_t *);
+int	fileport_makefd(fileport_t);
+
+#endif /* (!_POSIX_C_SOURCE || _DARWIN_C_SOURCE) */
+
+__END_DECLS
+
+#endif /* !KERNEL */
+
+#endif	/* !_SYS_FILEPORT_H_ */
  ```

  </details>
- 🟡 `lara/kexploit/persistence.m` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -6,7 +6,7 @@
 //
 
 #import <Foundation/Foundation.h>
-#import <sys/fileport.h>
+#import "fileport.h"
 #import "darksword.h"
 #import "offsets.h"
 #import "persistence.h"
  ```

  </details>
- 🟡 `lara/views/ContentView.swift` (modified) — +1 / -8
  <details><summary>Diff</summary>

  ```diff
  @@ -450,14 +450,7 @@ struct ContentView: View {
                         }
 
                         Button("Respring") {
-                            mgr.showRespringView = true
-                        }
-                        .fullScreenCover(isPresented: $mgr.showRespringView) {
-                            if mgr.showRespringView {
-                                RespringView()
-                                    .brightness(-1.0)
-                                    .ignoresSafeArea()
-                            }
+                            mgr.respring()
                         }
 
                         Button("Panic!") {
  ```

  </details>
- 🟡 `lara/views/EditorView.swift` (modified) — +1 / -8
  <details><summary>Diff</summary>

  ```diff
  @@ -177,14 +177,7 @@ struct EditorView: View {
             .alert("Done", isPresented: .constant(alert != nil)) {
                 Button("Cancel") { alert = nil }
                 Button("Respring") {
-                    mgr.showRespringView = true
-                }
-                .fullScreenCover(isPresented: $mgr.showRespringView) {
-                    if mgr.showRespringView {
-                        RespringView()
-                            .brightness(-1.0)
-                            .ignoresSafeArea()
-                    }
+                    mgr.respring()
                 }
             } message: {
                 Text(alert ?? "uhh...")
  ```

  </details>
- 🟡 `lara/views/FontPicker.swift` (modified) — +1 / -8
  <details><summary>Diff</summary>

  ```diff
  @@ -129,14 +129,7 @@ struct FontPicker: View {
                         .font(.system(size: 13, design: .monospaced))
                     
                     Button("Respring") {
-                        mgr.showRespringView = true
-                    }
-                    .fullScreenCover(isPresented: $mgr.showRespringView) {
-                        if mgr.showRespringView {
-                            RespringView()
-                                .brightness(-1.0)
-                                .ignoresSafeArea()
-                        }
+                        mgr.respring()
                     }
                 }
             }
  ```

  </details>
- 🟡 `lara/views/JitView.swift` (modified) — +21 / -14
  <details><summary>Diff</summary>

  ```diff
  @@ -174,20 +174,27 @@ struct JitView: View {
 	        globallogger.log("(jit) enabling for \(bundleID)...")
 
 	        let runEnable: () -> Void = {
-	            DispatchQueue.global(qos: .userInitiated).async {
-	                let err: Int32 = bundleID.withCString { cStr in
-                        enable_jit(mgr.sbProc, cStr)
-	                }
-	                DispatchQueue.main.async {
-	                    if err == 0 {
-	                        globallogger.log("(jit) enabled for \(bundleID)")
-	                    } else {
-	                        globallogger.log("(jit) failed for \(bundleID): \(err)")
-	                    }
-	                    enablingBundleID = nil
-	                }
-	            }
-	        }
+				guard let sbProc = mgr.sbProc else {
+					globallogger.log("(jit) error: sbProc is nil")
+					DispatchQueue.main.async { enablingBundleID = nil }
+					return
+				}
+
+				DispatchQueue.global(qos: .userInitiated).async {
+					let err: Int32 = bundleID.withCString { (cStr: UnsafePointer<Int8>) -> Int32 in
+						return enable_jit(sbProc, cStr)
+					}
+
+					DispatchQueue.main.async {
+						if err == 0 {
+							globallogger.log("(jit) enabled for \(bundleID)")
+						} else {
+							globallogger.log("(jit) error enabling for \(bundleID)!")
+						}
+						enablingBundleID = nil
+					}
+				}
+			}
 
 	        if mgr.rcrunning {
 	            runEnable()
  ```

  </details>
- 🟡 `lara/views/SettingsView.swift` (modified) — +0 / -28
  <details><summary>Diff</summary>

  ```diff
  @@ -502,34 +502,6 @@ struct SettingsView: View {
                         }
                     }
 
-                    HStack(alignment: .top) {
-                        AsyncImage(url: URL(string: "https://github.com/skadz108.png")) { image in
-                            image
-                                .resizable()
-                                .scaledToFill()
-                        } placeholder: {
-                            ProgressView()
-                        }
-                        .frame(width: 40, height: 40)
-                        .clipShape(Circle())
-                        
-                        VStack(alignment: .leading) {
-                            Text("Skadz")
-                                .font(.headline)
-                            
-                            Text("Implemented the respring script in Swift")
-                                .font(.subheadline)
-                                .foregroundColor(Color.secondary)
-                        }
-                        
-                        Spacer()
-                    }
-                    .onTapGesture {
-                        if let url = URL(string: "https://github.com/skadz108"),
-                           UIApplication.shared.canOpenURL(url) {
-                            UIApplication.shared.open(url)
-                        }
-                    }
                 } header: {
                     Text("Credits")
                 }
  ```

  </details>
- 🟡 `lara/views/ToolsView.swift` (modified) — +1 / -8
  <details><summary>Diff</summary>

  ```diff
  @@ -75,14 +75,7 @@ struct ToolsView: View {
             
             Section {
                 Button("Respring") {
-                    mgr.showRespringView = true
-                }
-                .fullScreenCover(isPresented: $mgr.showRespringView) {
-                    if mgr.showRespringView {
-                        RespringView()
-                            .brightness(-1.0)
-                            .ignoresSafeArea()
-                    }
+                    mgr.respring()
                 }
                 
                 HStack {
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 07a132d: fix laramgr
- : 📝 Commit de7f431: fix contentview
- : 📝 Commit ee1af97: fix editorview
- : 📝 Commit e8e8ea1: fix fontpicker
- : 📝 Commit 93ba28c: remove skadz from credits
- : 📝 Commit 6d3e32b: fix toolsview
- : 📝 Commit f86ed03: Create fileport.h
- : 📝 Commit f2d09f5: Update persistence.m
- : 📝 Commit 4ee0d0b: fix jitview
- : 📝 Commit 0c18fec: Merge branch 'rooootdev:main' into main
- : 📝 Commit 55cc6fb: Merge branch 'rooootdev:main' into main
- : 📝 Commit 6e7b119: final jitview fix
- Apr 22, 2026: 🟣 @rooootdev merged
- Apr 22, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 2:59:58 PM from [https://github.com/rooootdev/lara/pull/168](https://github.com/rooootdev/lara/pull/168)*
