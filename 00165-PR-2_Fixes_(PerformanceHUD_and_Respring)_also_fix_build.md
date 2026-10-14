# 2 Fixes (PerformanceHUD and Respring) also fix build

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#165](https://github.com/rooootdev/lara/pull/165)  
> **State:** 🟣 merged  
> **Branch:** `mrtacoguy01:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 22, 2026  
> **Changes:** +31 / -28 across 6 files  
> **Created:** Apr 22, 2026  
> **Updated:** Apr 22, 2026  
> **Closed:** Apr 22, 2026  

---

## Description

*Opened by [@mrtacoguy01](https://github.com/mrtacoguy01)* *on Apr 22, 2026*


Fix PerformanceHUD starting at "Basic" instead of "Off"
Made Respring Fullscreen instead of Overlay so its a little more reliable

Tested on iPhone 14 (iOS 18.1 22B5045g)

I tested this and I dont see any issues
<img width="256" height="auto" alt="image" src="https://github.com/user-attachments/assets/ca595444-66ef-4f92-9bb2-e100b0f3f53c" />
<img width="256" height="auto" alt="image" src="https://github.com/user-attachments/assets/a601fc3e-4fd9-4f18-b1a7-bb0bd0bccdcf" />


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24755878451/job/72428961136) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24755878451/job/72428867373) — **success** — *GitHub Actions*


---

## PR Commits (2)

- [`76a6db2`](https://github.com/rooootdev/lara/commit/76a6db2557bdf6a08f525bee040f3a870604bc0b) Fix PerformanceHUD starting at "Basic" instead of "Off" — *t* · Apr 22, 2026
- [`5fe606f`](https://github.com/rooootdev/lara/commit/5fe606f018e13f9bb9faf55475ac361b7a763579) Make Respring Fullscreen instead of Overlay — *t* · Apr 22, 2026


---

## Changed Files (6)

- 🟡 `lara/classes/laramgr.swift` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -92,6 +92,7 @@ final class laramgr: ObservableObject {
     @Published var sbxfailed: Bool = false
     @Published var sbxrunning: Bool = false
     @Published var rcready: Bool = false
+    @Published var showRespringView: Bool = false
     
     var sbProc: RemoteCall?
     
  ```

  </details>
- 🟡 `lara/views/ContentView.swift` (modified) — +7 / -6
  <details><summary>Diff</summary>

  ```diff
  @@ -446,12 +446,13 @@ struct ContentView: View {
                         }
 
                         Button("Respring") {
-                            .overlay {
-                                if mgr.showRespringView {
-                                    RespringView()
-                                        .brightness(-1.0)
-                                        .ignoresSafeArea()
-                                }
+                            mgr.showRespringView = true
+                        }
+                        .fullScreenCover(isPresented: $mgr.showRespringView) {
+                            if mgr.showRespringView {
+                                RespringView()
+                                    .brightness(-1.0)
+                                    .ignoresSafeArea()
                             }
                         }
 
  ```

  </details>
- 🟡 `lara/views/EditorView.swift` (modified) — +7 / -6
  <details><summary>Diff</summary>

  ```diff
  @@ -131,12 +131,13 @@ struct EditorView: View {
             .alert("Done", isPresented: .constant(alert != nil)) {
                 Button("Cancel") { alert = nil }
                 Button("Respring") {
-                    .overlay {
-                        if mgr.showRespringView {
-                            RespringView()
-                                .brightness(-1.0)
-                                .ignoresSafeArea()
-                        }
+                    mgr.showRespringView = true
+                }
+                .fullScreenCover(isPresented: $mgr.showRespringView) {
+                    if mgr.showRespringView {
+                        RespringView()
+                            .brightness(-1.0)
+                            .ignoresSafeArea()
                     }
                 }
             } message: {
  ```

  </details>
- 🟡 `lara/views/FontPicker.swift` (modified) — +7 / -6
  <details><summary>Diff</summary>

  ```diff
  @@ -129,12 +129,13 @@ struct FontPicker: View {
                         .font(.system(size: 13, design: .monospaced))
                     
                     Button("Respring") {
-                        .overlay {
-                            if mgr.showRespringView {
-                                RespringView()
-                                    .brightness(-1.0)
-                                    .ignoresSafeArea()
-                            }
+                        mgr.showRespringView = true
+                    }
+                    .fullScreenCover(isPresented: $mgr.showRespringView) {
+                        if mgr.showRespringView {
+                            RespringView()
+                                .brightness(-1.0)
+                                .ignoresSafeArea()
                         }
                     }
                 }
  ```

  </details>
- 🟡 `lara/views/RemoteView.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -11,7 +11,7 @@ struct RemoteView: View {
     @ObservedObject var mgr: laramgr
     @State private var running: Bool = false
     @State private var columns: Int = 5
-    @State private var performanceHUD: Int = 0
+    @State private var performanceHUD: Int = -1
     @AppStorage("rcdockunlimited") private var rcdockunlimited: Bool = false
 
     private var dockMaxColumns: Int { rcdockunlimited ? 50 : 10 }
  ```

  </details>
- 🟡 `lara/views/ToolsView.swift` (modified) — +8 / -9
  <details><summary>Diff</summary>

  ```diff
  @@ -74,16 +74,15 @@ struct ToolsView: View {
             }
             
             Section {
-                Button {
-                    .overlay {
-                        if mgr.showRespringView {
-                            RespringView()
-                                .brightness(-1.0)
-                                .ignoresSafeArea()
-                        }
+                Button("Respring") {
+                    mgr.showRespringView = true
+                }
+                .fullScreenCover(isPresented: $mgr.showRespringView) {
+                    if mgr.showRespringView {
+                        RespringView()
+                            .brightness(-1.0)
+                            .ignoresSafeArea()
                     }
-                } label: {
-                    Text("Respring")
                 }
                 
                 HStack {
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 76a6db2: Fix PerformanceHUD starting at "Basic" instead of "Off"
- : 📝 Commit 5fe606f: Make Respring Fullscreen instead of Overlay
- Apr 22, 2026: ✏️ @mrtacoguy01 renamed: "2 Fixes (PerformanceHUD and Respring)" → "2 Fixes (PerformanceHUD and Respring) also fix build"
- Apr 22, 2026: 🟣 @rooootdev merged
- Apr 22, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 2:59:39 PM from [https://github.com/rooootdev/lara/pull/165](https://github.com/rooootdev/lara/pull/165)*
