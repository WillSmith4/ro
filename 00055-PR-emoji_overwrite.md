# emoji overwrite

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#55](https://github.com/rooootdev/lara/pull/55)  
> **State:** 🔴 closed  
> **Branch:** `blahblahh1:test` → `rooootdev:main`  
> **Changes:** +143 / -0 across 2 files  
> **Created:** Apr 8, 2026  
> **Updated:** Apr 17, 2026  
> **Closed:** Apr 17, 2026  

---

## Description

*Opened by [@blahblahh1](https://github.com/blahblahh1)* *on Apr 8, 2026*


tested on ios 17.5.1


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (2)

- [`3d37e10`](https://github.com/rooootdev/lara/commit/3d37e10fe23c62918be3db1f2e6645d74f67a0ce) emoji overwrite — *blahblahh1* · Apr 8, 2026
- [`e2c04ae`](https://github.com/rooootdev/lara/commit/e2c04aeddc5f31ca20bdf53c8da7e50b178daaa2) woops — *blahblahh1* · Apr 8, 2026


---

## Changed Files (2)

- 🟡 `lara/views/ContentView.swift` (modified) — +8 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -141,6 +141,10 @@ struct ContentView: View {
                             .disabled(!mgr.dsready || mgr.vfsready || mgr.vfsrunning)
                             
                             if mgr.vfsready {
+                                NavigationLink("Emoji Overwrite") {
+                                    EmojiPicker(mgr: mgr)
+                                }
+                                
                                 NavigationLink("Font Overwrite") {
                                     FontPicker(mgr: mgr)
                                 }
@@ -302,6 +306,10 @@ struct ContentView: View {
                                     }
                                 }
                                 
+                                NavigationLink("Emoji Overwrite") {
+                                    EmojiPicker(mgr: mgr)
+                                }
+                                
                                 NavigationLink("Font Overwrite") {
                                     FontPicker(mgr: mgr)
                                 }
  ```

  </details>
- 🟢 `lara/views/EmojiPicker.swift` (added) — +135 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,135 @@
+//
+//  EmojiPicker.swift
+//  lara
+//
+//  Created by user on 8/4/2026.
+//
+
+import SwiftUI
+
+struct EmojiPicker: View {
+    @ObservedObject var mgr: laramgr
+    private let emojipath = "/System/Library/Fonts/CoreAddition/AppleColorEmoji-160px.ttc"
+    
+    
+    var body: some View {
+        NavigationStack {
+            List {
+                Section {
+                    Button {
+                        applyemojis(url :"https://github.com/PoomSmart/EmojiFonts/releases/download/17.0.0/AppleColorEmoji.ttc",
+                        label: "AppleColorEmoji.ttc")
+                    } label: {
+                        Text("Apple Color Emoji (default)")
+                    }
+                    
+                    Button {
+                        applyemojis(url :"https://github.com/PoomSmart/EmojiFonts/releases/download/17.0.0/AppleColorEmoji-pixel.ttc",
+                        label: "AppleColorEmoji-pixel.ttc")
+                    } label: {
+                        Text("Apple Color Emoji (pixel)")
+                    }
+                    
+                    Button {
+                        applyemojis(url :"https://github.com/PoomSmart/EmojiFonts/releases/download/17.0.0/twemoji.ttc",
+                        label: "twemoji.ttc")
+                    } label: {
+                        Text("Twemoji")
+                    }
+                    
+                    Button {
+                        applyemojis(url :"https://github.com/PoomSmart/EmojiFonts/releases/download/17.0.0/blobmoji.ttc",
+                        label: "blobmoji.ttc")
+                    } label: {
+                        Text("Blobmoji")
+                    }
+                    
+                    Button {
+                        applyemojis(url :"https://github.com/PoomSmart/EmojiFonts/releases/download/17.0.0/oneui.ttc",
+                        label: "oneui.ttc")
+                    } label: {
+                        Text("OneUI")
+                    }
+                    
+                    Button {
+                        applyemojis(url :"https://github.com/PoomSmart/EmojiFonts/releases/download/17.0.0/noto-emoji.ttc",
+                        label: "noto-emoji.ttc")
+                    } label: {
+                        Text("Noto Emoji")
+                    }
+                }
+                
+                Section {
+                    Text(globallogger.logs.last ?? "No logs yet")
+                        .font(.system(size: 13, design: .monospaced))
+                    
+                    if #unavailable(iOS 18.2) {
+                        Button("Respring") {
+                            mgr.respring()
+                        }
+                    }
+                }
+            }
+            .navigationTitle("Emoji Overwrite")
+
+        }
+    }
+    
+    private func applyemojis(url: String, label: String) {
+        Task {
+            let fm = FileManager.default
+            var source: URL?
+            
+            if let docs = fm.urls(for: .documentDirectory, in: .userDomainMask).first {
+                let fileURL = docs.appendingPathComponent("Emojis").appendingPathComponent(label)
+                if !fm.fileExists(atPath: fileURL.path) {
+                    mgr.logmsg("downloading emojis from \(url)")
+                    source = await downloademoji(url: url)
+                    if source == nil {
+                        mgr.logmsg("download failed!")
+                        return
+                    }
+                } else {
+                    source = fileURL
+                }
+            }
+            
+            guard let src = source else {
+                mgr.logmsg("no source");
+                return
+            }
+            
+            let success = mgr.vfsoverwritefromlocalpath(target: emojipath, source: src.path)
+            
+            success ? mgr.logmsg("emoji changed to \(label)") : mgr.logmsg("failed to change emojis")
+        }
+    }
+}
+
+
+
+private func downloademoji(url: String) async -> URL? {
+    guard let remoteurl = URL(string: url) else { return nil }
+    let fm = FileManager.default
+    let docs = fm.urls(for: .documentDirectory, in: .userDomainMask)[0]
+    let dir = docs.appendingPathComponent("Emojis")
+    
+    do {
+        if !fm.fileExists(atPath: dir.path) {
+            try fm.createDirectory(at: dir, withIntermediateDirectories: true, attributes: nil)
+        }
+
+        let dest = dir.appendingPathComponent(remoteurl.lastPathComponent)
+        if fm.fileExists(atPath: dest.path) {
+            return dest
+        }
+        
+        let (tempurl, _) = try await URLSession.shared.download(from: remoteurl)
+
+        try fm.moveItem(at: tempurl, to: dest)
+        return dest
+    } catch {
+        return nil
+    }
+}
+
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 3d37e10: emoji overwrite
- : 📝 Commit e2c04ae: woops
- Apr 17, 2026: 🔴 Closed by @Pro42good


---

## Comments (2)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Apr 16, 2026

`ID: 4261657314`


ill add this but not merge your pr as I dont like how it works... you will still be credited of course


---

### Comment #2 — [@Pro42good](https://github.com/Pro42good) · Apr 17, 2026

`ID: 4271144685`


I'm just going to close this PR since rooootdev is adding it with credit and since this is not going to be merged directly.


---

*Exported on 10/14/2026, 2:50:41 PM from [https://github.com/rooootdev/lara/pull/55](https://github.com/rooootdev/lara/pull/55)*
