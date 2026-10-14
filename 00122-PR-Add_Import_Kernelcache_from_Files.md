# Add "Import Kernelcache from Files"

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#122](https://github.com/rooootdev/lara/pull/122)  
> **State:** 🟣 merged  
> **Branch:** `hxhlb:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 19, 2026  
> **Changes:** +76 / -0 across 1 files  
> **Created:** Apr 18, 2026  
> **Updated:** Apr 19, 2026  
> **Closed:** Apr 19, 2026  

---

## Description

*Opened by [@hxhlb](https://github.com/hxhlb)* *on Apr 18, 2026*


Select kernelcache from Files


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`e7f6fb6`](https://github.com/rooootdev/lara/commit/e7f6fb684a3d2d9fdd14ccbb877343ed79bce487) Add "Import Kernelcache from Files" — *Hades* · Apr 18, 2026


---

## Changed Files (1)

- 🟡 `lara/views/SettingsView.swift` (modified) — +76 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -7,12 +7,15 @@
 
 import SwiftUI
 import UIKit
+import UniformTypeIdentifiers
 
 struct SettingsView: View {
     @ObservedObject var mgr: laramgr
     @Binding var hasoffsets: Bool
     @State private var showresetalert: Bool = false
     @State private var downloadingkernelcache = false
+    @State private var showingKernelcacheImporter: Bool = false
+    @State private var importingkernelcache: Bool = false
     @AppStorage("loggernobullshit") private var loggernobullshit: Bool = true
     @AppStorage("keepalive") private var iskeepalive: Bool = true
     @AppStorage("showfmintabs") private var showfmintabs: Bool = true
@@ -133,6 +136,41 @@ struct SettingsView: View {
                         Button("Fetch Kernelcache") {
                             mgr.run()
                         }
+                        
+                        Button("Import Kernelcache from Files") {
+                            guard !importingkernelcache else { return }
+                            showingKernelcacheImporter = true
+                        }
+                        .disabled(importingkernelcache)
+
+                        VStack(alignment: .leading, spacing: 10) {
+                            Text("How to obtain a kernelcache (macOS)")
+                                .font(.footnote.weight(.semibold))
+                                .foregroundColor(.primary)
+
+                            Text("1. Download the IPSW tool for your device.")
+                            Link("https://github.com/blacktop/ipsw/releases",
+                                 destination: URL(string: "https://github.com/blacktop/ipsw/releases")!)
+
+                            Text("2. Extract the archive.")
+                            Text("3. Open Terminal.")
+                            Text("4. Navigate to the extracted folder:")
+                            Text("cd /path/to/ipsw_3.1.671_something_something/")
+                                .font(.system(.caption2, design: .monospaced))
+                                .textSelection(.enabled)
+
+                            Text("5. Extract the kernel:")
+                            Text("./ipsw extract --kernel [drag your ipsw here]")
+                                .font(.system(.caption2, design: .monospaced))
+                                .textSelection(.enabled)
+
+                            Text("6. Get the kernelcache file.")
+                            Text("7. Transfer the kernelcache to your iCloud or iPhone.")
+                            Text("8. Tap the button above and select the kernelcache, for example kernelcache.release.iPhone14,3.")
+                        }
+                        .font(.caption)
+                        .foregroundColor(.secondary)
+                        .padding(.vertical, 4)
                     }
                     
                     Button {
@@ -298,6 +336,44 @@ struct SettingsView: View {
             }
             .navigationTitle("Settings")
         }
+        .fileImporter(isPresented: $showingKernelcacheImporter,
+                      allowedContentTypes: [.data],
+                      allowsMultipleSelection: false) { result in
+            switch result {
+            case .success(let urls):
+                guard let url = urls.first else { return }
+                importingkernelcache = true
+                DispatchQueue.global(qos: .userInitiated).async {
+                    var ok = false
+                    let shouldStopAccess = url.startAccessingSecurityScopedResource()
+                    defer {
+                        if shouldStopAccess {
+                            url.stopAccessingSecurityScopedResource()
+                        }
+                    }
+                    let fm = FileManager.default
+                    if let docs = fm.urls(for: .documentDirectory, in: .userDomainMask).first {
+                        let dest = docs.appendingPathComponent("kernelcache")
+                        do {
+                            if fm.fileExists(atPath: dest.path) {
+                                try fm.removeItem(at: dest)
+                            }
+                            try fm.copyItem(at: url, to: dest)
+                            ok = dlkerncache()
+                        } catch {
+                            print("failed to import kernelcache: \(error)")
+                            ok = false
+                        }
+                    }
+                    DispatchQueue.main.async {
+                        hasoffsets = ok
+                        importingkernelcache = false
+                    }
+                }
+            case .failure:
+                break
+            }
+        }
         .alert("Clear Kernelcache Data?", isPresented: $showresetalert) {
             Button("Cancel", role: .cancel) {}
             Button("Delete", role: .destructive) {
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit e7f6fb6: Add "Import Kernelcache from Files"
- Apr 19, 2026: 🟣 @rooootdev merged
- Apr 19, 2026: 🔴 Closed by @rooootdev


---

## Comments (1)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Apr 19, 2026

`ID: 4275564825`


Great PR!


---

*Exported on 10/14/2026, 2:56:10 PM from [https://github.com/rooootdev/lara/pull/122](https://github.com/rooootdev/lara/pull/122)*
