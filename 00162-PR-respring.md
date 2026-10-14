# respring

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#162](https://github.com/rooootdev/lara/pull/162)  
> **State:** 🟣 merged  
> **Branch:** `neonmodder123:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 21, 2026  
> **Changes:** +112 / -7 across 6 files  
> **Created:** Apr 21, 2026  
> **Updated:** Apr 21, 2026  
> **Closed:** Apr 21, 2026  

---

## Description

*Opened by [@rooootdev](https://github.com/rooootdev)* *on Apr 21, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (6)

- 🔴 [build](https://github.com/rooootdev/lara/actions/runs/24740472264/job/72378512392) — **failure** — *GitHub Actions*
- ✅ [Cleanup artifacts](https://github.com/rooootdev/lara/actions/runs/24740472777/job/72378882723) — **success** — *GitHub Actions*
- ✅ [Upload results](https://github.com/rooootdev/lara/actions/runs/24740472777/job/72378848074) — **success** — *GitHub Actions*
- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24740472264/job/72378653191) — **skipped** — *GitHub Actions*
- ✅ [Agent](https://github.com/rooootdev/lara/actions/runs/24740472777/job/72378533369) — **success** — *GitHub Actions*
- ✅ [Prepare](https://github.com/rooootdev/lara/actions/runs/24740472777/job/72378516014) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 21, 2026

## Pull request overview

Updates the app’s “Respring” functionality to use an embedded WebKit-based respring script UI, and adjusts Credits to include the Swift implementation contributor.

**Changes:**
- Adds a `RespringView` (`WKWebView` via `UIViewRepresentable`) backed by an inlined HTML/JS respring document.
- Replaces several `mgr.respring()` button actions with conditional overlay-based presentation of `RespringView`.
- Updates Credits by adding “Skadz” and modifying the existing “neon” link target.

### Reviewed changes

Copilot reviewed 6 out of 6 changed files in this pull request and generated 9 comments.

<details>
<summary>Show a summary per file</summary>

| File | Description |
| ---- | ----------- |
| lara/views/ToolsView.swift | Replaces Respring button action with an overlay-driven respring UI presentation. |
| lara/views/SettingsView.swift | Adds a new Credits entry and changes tap target URL for an existing entry. |
| lara/views/FontPicker.swift | Replaces Respring trigger with an overlay-driven respring UI presentation. |
| lara/views/EditorView.swift | Attempts to add respring UI presentation from an alert action. |
| lara/views/ContentView.swift | Replaces Respring button action with an overlay-driven respring UI presentation. |
| lara/classes/laramgr.swift | Introduces `RespringView` + the embedded HTML/JS respring document. |
</details>






---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (9)

*Inline comments on code changes*

### `lara/views/ToolsView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (lines 78-87) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -75,7 +75,13 @@ struct ToolsView: View {
             
             Section {
                 Button {
-                    mgr.respring()
+                    .overlay {
+                        if mgr.showRespringView {
+                            RespringView()
+                                .brightness(-1.0)
+                                .ignoresSafeArea()
+                        }
+                    }
                 } label: {
                     Text("Respring")
                 }
```

</details>

`Button { ... }` action closure contains a `.overlay { ... }` view modifier call, which is invalid Swift (there’s no view being modified inside the action) and leaves the Respring button doing nothing. Move the overlay onto the `Button` view itself (or wrap with a `ZStack`) and keep the action as something like toggling a state / calling `mgr.respring()` to actually trigger the respring UI.
```suggestion
                    mgr.respring()
                } label: {
                    Text("Respring")
                }
                .overlay {
                    if mgr.showRespringView {
                        RespringView()
                            .brightness(-1.0)
                            .ignoresSafeArea()
                    }
                }
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (lines 79-83) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -75,7 +75,13 @@ struct ToolsView: View {
             
             Section {
                 Button {
-                    mgr.respring()
+                    .overlay {
+                        if mgr.showRespringView {
+                            RespringView()
+                                .brightness(-1.0)
+                                .ignoresSafeArea()
+                        }
```

</details>

This references `mgr.showRespringView`, but `laramgr` does not define that published/state property anywhere in the repo, so the project won’t compile. Add an `@Published var showRespringView` (or equivalent) to `laramgr` (and ensure it’s set to `true` from the Respring button) or remove this conditional.


### `lara/views/ContentView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (lines 449-454) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -446,7 +446,13 @@ struct ContentView: View {
                         }
 
                         Button("Respring") {
-                            mgr.respring()
+                            .overlay {
+                                if mgr.showRespringView {
+                                    RespringView()
+                                        .brightness(-1.0)
+                                        .ignoresSafeArea()
+                                }
```

</details>

The Respring button’s action closure contains only an `.overlay { ... }` modifier call, so the button currently has no effect and won’t compile. The overlay should be attached to the button/view hierarchy, while the action should toggle whatever state controls showing the respring UI (or call `mgr.respring()`).
```suggestion
                            mgr.showRespringView = true
                        }
                        .overlay {
                            if mgr.showRespringView {
                                RespringView()
                                    .brightness(-1.0)
                                    .ignoresSafeArea()
```


### `lara/classes/laramgr.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (lines 56-57) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -10,6 +10,58 @@ import Foundation
 import Darwin
 import notify
 import SafariServices
+import SwiftUI
+import UIKit
+import WebKit
+
+let respringDocument = """
+<!DOCTYPE html>
+<html>
+    <body>
+        <iframe id="frame" srcdoc="" sandbox="allow-forms allow-modals allow-orientation-lock allow-pointer-lock allow-popups allow-presentation allow-scripts"></iframe>
+        <script>
+            const frame = document.getElementById('frame');
+            const respringScript = `
+                <html>
+                <body>
+                    <script>
+                        const container = document.createElement('div');
+                        container.style.cssText = 'perspective: 1px; perspective-origin: 9999999% 9999999%;';
+                        document.body.appendChild(container);
+    
+                        for (let i = 0; i < 500; i++) {
+                            let d = document.createElement('div');
+                            d.style.cssText = 'position: absolute; width: 100vw; height: 100vh; backdrop-filter: blur(100px); -webkit-backdrop-filter: blur(100px); transform: translate3d(100000px, 100000px, ' + i + 'px) rotateY(90deg);';
+                            container.appendChild(d);
+                        }
+    
+                        setInterval(() => {
+                            navigator.share({ title: 'R', text: 'R'.repeat(100000) }).catch(() => {});
+                            let x = new Uint8Array(1024 * 1024 * 10);
+                            crypto.getRandomValues(x);
+                        }, 0);
+                    <\\/script>
+                </body>
+                </html>
+            `;
+    
+            frame.srcdoc = respringScript;
+        </script>
+    </body>
+</html>
+"""
+
+struct RespringView: UIViewRepresentable {
+    func makeUIView(context: Context) -> WKWebView {
+        let webView = WKWebView()
+        WKWebpagePreferences().allowsContentJavaScript = true
```

</details>

`WKWebpagePreferences().allowsContentJavaScript = true` creates and mutates a preferences instance that is never attached to the `WKWebView`, so it has no effect (and is misleading). Either remove it or construct the `WKWebView` with a `WKWebViewConfiguration` and set `configuration.defaultWebpagePreferences.allowsContentJavaScript` (and any other needed prefs) before creating the web view.
```suggestion
        let configuration = WKWebViewConfiguration()
        configuration.defaultWebpagePreferences.allowsContentJavaScript = true
        let webView = WKWebView(frame: .zero, configuration: configuration)
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (lines 58-62) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -10,6 +10,58 @@ import Foundation
 import Darwin
 import notify
 import SafariServices
+import SwiftUI
+import UIKit
+import WebKit
+
+let respringDocument = """
+<!DOCTYPE html>
+<html>
+    <body>
+        <iframe id="frame" srcdoc="" sandbox="allow-forms allow-modals allow-orientation-lock allow-pointer-lock allow-popups allow-presentation allow-scripts"></iframe>
+        <script>
+            const frame = document.getElementById('frame');
+            const respringScript = `
+                <html>
+                <body>
+                    <script>
+                        const container = document.createElement('div');
+                        container.style.cssText = 'perspective: 1px; perspective-origin: 9999999% 9999999%;';
+                        document.body.appendChild(container);
+    
+                        for (let i = 0; i < 500; i++) {
+                            let d = document.createElement('div');
+                            d.style.cssText = 'position: absolute; width: 100vw; height: 100vh; backdrop-filter: blur(100px); -webkit-backdrop-filter: blur(100px); transform: translate3d(100000px, 100000px, ' + i + 'px) rotateY(90deg);';
+                            container.appendChild(d);
+                        }
+    
+                        setInterval(() => {
+                            navigator.share({ title: 'R', text: 'R'.repeat(100000) }).catch(() => {});
+                            let x = new Uint8Array(1024 * 1024 * 10);
+                            crypto.getRandomValues(x);
+                        }, 0);
+                    <\\/script>
+                </body>
+                </html>
+            `;
+    
+            frame.srcdoc = respringScript;
+        </script>
+    </body>
+</html>
+"""
+
+struct RespringView: UIViewRepresentable {
+    func makeUIView(context: Context) -> WKWebView {
+        let webView = WKWebView()
+        WKWebpagePreferences().allowsContentJavaScript = true
+        return webView
+    }
+
+    func updateUIView(_ webView: WKWebView, context: Context) {
+        webView.loadHTMLString(respringDocument, baseURL: nil)
```

</details>

`updateUIView` unconditionally calls `loadHTMLString`, which will reload the page on every SwiftUI update (potentially many times). Load once (e.g., in `makeUIView`, or guard with a flag / `webView.url == nil`) to avoid repeated reloads and unpredictable behavior.
```suggestion
        webView.loadHTMLString(respringDocument, baseURL: nil)
        return webView
    }

    func updateUIView(_ webView: WKWebView, context: Context) {
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (line 39) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -10,6 +10,58 @@ import Foundation
 import Darwin
 import notify
 import SafariServices
+import SwiftUI
+import UIKit
+import WebKit
+
+let respringDocument = """
+<!DOCTYPE html>
+<html>
+    <body>
+        <iframe id="frame" srcdoc="" sandbox="allow-forms allow-modals allow-orientation-lock allow-pointer-lock allow-popups allow-presentation allow-scripts"></iframe>
+        <script>
+            const frame = document.getElementById('frame');
+            const respringScript = `
+                <html>
+                <body>
+                    <script>
+                        const container = document.createElement('div');
+                        container.style.cssText = 'perspective: 1px; perspective-origin: 9999999% 9999999%;';
+                        document.body.appendChild(container);
+    
+                        for (let i = 0; i < 500; i++) {
+                            let d = document.createElement('div');
+                            d.style.cssText = 'position: absolute; width: 100vw; height: 100vh; backdrop-filter: blur(100px); -webkit-backdrop-filter: blur(100px); transform: translate3d(100000px, 100000px, ' + i + 'px) rotateY(90deg);';
+                            container.appendChild(d);
+                        }
+    
+                        setInterval(() => {
+                            navigator.share({ title: 'R', text: 'R'.repeat(100000) }).catch(() => {});
```

</details>

In the injected JS, `navigator.share(...)` is called without checking if `navigator.share` exists. In environments where it’s undefined, this throws and can prevent the rest of the interval body (including the memory pressure code) from running. Wrap the call in a `try/catch` or guard `if (navigator.share) { ... }` so the rest of the loop still executes.
```suggestion
                            if (navigator.share) {
                                navigator.share({ title: 'R', text: 'R'.repeat(100000) }).catch(() => {});
                            }
```


### `lara/views/FontPicker.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (lines 132-137) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -128,9 +128,13 @@ struct FontPicker: View {
                     Text(globallogger.logs.last ?? "No logs yet")
                         .font(.system(size: 13, design: .monospaced))
                     
-                    if #unavailable(iOS 18.2) {
-                        Button("Respring") {
-                            mgr.respring()
+                    Button("Respring") {
+                        .overlay {
+                            if mgr.showRespringView {
+                                RespringView()
+                                    .brightness(-1.0)
+                                    .ignoresSafeArea()
+                            }
```

</details>

`Button("Respring") { .overlay { ... } }` is not valid SwiftUI: the `.overlay` modifier is being placed inside the button’s action closure, so nothing is executed and it won’t compile. Apply `.overlay(...)` to the `Button` view (or present `RespringView` via `.sheet`/`.fullScreenCover`) and keep the action for triggering the presentation.
```suggestion
                        mgr.showRespringView = true
                    }
                    .overlay {
                        if mgr.showRespringView {
                            RespringView()
                                .brightness(-1.0)
                                .ignoresSafeArea()
```


### `lara/views/EditorView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (lines 134-144) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -130,7 +130,15 @@ struct EditorView: View {
             }
             .alert("Done", isPresented: .constant(alert != nil)) {
                 Button("Cancel") { alert = nil }
-                Button("Respring") { mgr.respring() }
+                Button("Respring") {
+                    .overlay {
+                        if mgr.showRespringView {
+                            RespringView()
+                                .brightness(-1.0)
+                                .ignoresSafeArea()
+                        }
+                    }
+                }
             } message: {
                 Text(alert ?? "uhh...")
             }
```

</details>

SwiftUI `Alert` buttons can’t have view modifiers like `.overlay`, and this action closure currently contains only a modifier call (so it won’t compile / won’t trigger respring). Keep the alert button action to toggle state or call `mgr.respring()`, and present `RespringView` outside the alert using `.sheet`/`.fullScreenCover`/overlay on the parent view.
```suggestion
                    alert = nil
                    mgr.showRespringView = true
                }
            } message: {
                Text(alert ?? "uhh...")
            }
            .fullScreenCover(isPresented: $mgr.showRespringView) {
                RespringView()
                    .brightness(-1.0)
                    .ignoresSafeArea()
            }
```


### `lara/views/SettingsView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 21, 2026 (line 499) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -496,7 +496,36 @@ struct SettingsView: View {
                         Spacer()
                     }
                     .onTapGesture {
-                        if let url = URL(string: "https://github.com/neonmodder123"),
+                        if let url = URL(string: "https://github.com/skadz108"),
```

</details>

The `neon` credit row still displays neon’s avatar/text, but its `.onTapGesture` now opens `https://github.com/skadz108`. This makes the link incorrect and duplicates the Skadz row link below. Update the URL to match the displayed contributor (or adjust the displayed name/avatar accordingly).
```suggestion
                        if let url = URL(string: "https://github.com/neonmodder123"),
```



---

## PR Commits (1)

- [`e11bd76`](https://github.com/rooootdev/lara/commit/e11bd76863eb875752d5e2b7c0ef65ef60322e29) respring — *neon* · Apr 21, 2026


---

## Changed Files (6)

- 🟡 `lara/classes/laramgr.swift` (modified) — +52 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -10,6 +10,58 @@ import Foundation
 import Darwin
 import notify
 import SafariServices
+import SwiftUI
+import UIKit
+import WebKit
+
+let respringDocument = """
+<!DOCTYPE html>
+<html>
+    <body>
+        <iframe id="frame" srcdoc="" sandbox="allow-forms allow-modals allow-orientation-lock allow-pointer-lock allow-popups allow-presentation allow-scripts"></iframe>
+        <script>
+            const frame = document.getElementById('frame');
+            const respringScript = `
+                <html>
+                <body>
+                    <script>
+                        const container = document.createElement('div');
+                        container.style.cssText = 'perspective: 1px; perspective-origin: 9999999% 9999999%;';
+                        document.body.appendChild(container);
+    
+                        for (let i = 0; i < 500; i++) {
+                            let d = document.createElement('div');
+                            d.style.cssText = 'position: absolute; width: 100vw; height: 100vh; backdrop-filter: blur(100px); -webkit-backdrop-filter: blur(100px); transform: translate3d(100000px, 100000px, ' + i + 'px) rotateY(90deg);';
+                            container.appendChild(d);
+                        }
+    
+                        setInterval(() => {
+                            navigator.share({ title: 'R', text: 'R'.repeat(100000) }).catch(() => {});
+                            let x = new Uint8Array(1024 * 1024 * 10);
+                            crypto.getRandomValues(x);
+                        }, 0);
+                    <\\/script>
+                </body>
+                </html>
+            `;
+    
+            frame.srcdoc = respringScript;
+        </script>
+    </body>
+</html>
+"""
+
+struct RespringView: UIViewRepresentable {
+    func makeUIView(context: Context) -> WKWebView {
+        let webView = WKWebView()
+        WKWebpagePreferences().allowsContentJavaScript = true
+        return webView
+    }
+
+    func updateUIView(_ webView: WKWebView, context: Context) {
+        webView.loadHTMLString(respringDocument, baseURL: nil)
+    }
+}
 
 final class laramgr: ObservableObject {
     @Published var log: String = ""
  ```

  </details>
- 🟡 `lara/views/ContentView.swift` (modified) — +7 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -446,7 +446,13 @@ struct ContentView: View {
                         }
 
                         Button("Respring") {
-                            mgr.respring()
+                            .overlay {
+                                if mgr.showRespringView {
+                                    RespringView()
+                                        .brightness(-1.0)
+                                        .ignoresSafeArea()
+                                }
+                            }
                         }
 
                         Button("Panic!") {
  ```

  </details>
- 🟡 `lara/views/EditorView.swift` (modified) — +9 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -130,7 +130,15 @@ struct EditorView: View {
             }
             .alert("Done", isPresented: .constant(alert != nil)) {
                 Button("Cancel") { alert = nil }
-                Button("Respring") { mgr.respring() }
+                Button("Respring") {
+                    .overlay {
+                        if mgr.showRespringView {
+                            RespringView()
+                                .brightness(-1.0)
+                                .ignoresSafeArea()
+                        }
+                    }
+                }
             } message: {
                 Text(alert ?? "uhh...")
             }
  ```

  </details>
- 🟡 `lara/views/FontPicker.swift` (modified) — +7 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -128,9 +128,13 @@ struct FontPicker: View {
                     Text(globallogger.logs.last ?? "No logs yet")
                         .font(.system(size: 13, design: .monospaced))
                     
-                    if #unavailable(iOS 18.2) {
-                        Button("Respring") {
-                            mgr.respring()
+                    Button("Respring") {
+                        .overlay {
+                            if mgr.showRespringView {
+                                RespringView()
+                                    .brightness(-1.0)
+                                    .ignoresSafeArea()
+                            }
                         }
                     }
                 }
  ```

  </details>
- 🟡 `lara/views/SettingsView.swift` (modified) — +30 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -496,7 +496,36 @@ struct SettingsView: View {
                         Spacer()
                     }
                     .onTapGesture {
-                        if let url = URL(string: "https://github.com/neonmodder123"),
+                        if let url = URL(string: "https://github.com/skadz108"),
+                           UIApplication.shared.canOpenURL(url) {
+                            UIApplication.shared.open(url)
+                        }
+                    }
+
+                    HStack(alignment: .top) {
+                        AsyncImage(url: URL(string: "https://github.com/skadz108.png")) { image in
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
+                            Text("Skadz")
+                                .font(.headline)
+                            
+                            Text("Implemented the respring script in Swift")
+                                .font(.subheadline)
+                                .foregroundColor(Color.secondary)
+                        }
+                        
+                        Spacer()
+                    }
+                    .onTapGesture {
+                        if let url = URL(string: "https://github.com/skadz108"),
                            UIApplication.shared.canOpenURL(url) {
                             UIApplication.shared.open(url)
                         }
  ```

  </details>
- 🟡 `lara/views/ToolsView.swift` (modified) — +7 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -75,7 +75,13 @@ struct ToolsView: View {
             
             Section {
                 Button {
-                    mgr.respring()
+                    .overlay {
+                        if mgr.showRespringView {
+                            RespringView()
+                                .brightness(-1.0)
+                                .ignoresSafeArea()
+                        }
+                    }
                 } label: {
                     Text("Respring")
                 }
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit e11bd76: respring
- Apr 21, 2026: 👀 @rooootdev requested review from @Copilot
- Apr 21, 2026: 🟣 @rooootdev merged
- Apr 21, 2026: 🔴 Closed by @rooootdev
- : 💬 someone reviewed: commented


---

*Exported on 10/14/2026, 2:59:21 PM from [https://github.com/rooootdev/lara/pull/162](https://github.com/rooootdev/lara/pull/162)*
