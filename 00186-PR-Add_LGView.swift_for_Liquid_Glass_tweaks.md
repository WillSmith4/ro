# Add LGView.swift for Liquid Glass tweaks

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#186](https://github.com/rooootdev/lara/pull/186)  
> **State:** 🟣 merged  
> **Branch:** `jurre111:DisableLG` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 26, 2026  
> **Changes:** +173 / -0 across 2 files  
> **Created:** Apr 24, 2026  
> **Updated:** Apr 26, 2026  
> **Closed:** Apr 26, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on Apr 24, 2026*


Only useful for iOS 26.0 and 26.0.1 of course


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24910596170/job/72951137242) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24910596170/job/72950971954) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 24, 2026

## Pull request overview

Adds a new SwiftUI screen to tweak “Liquid Glass”/Solarium-related flags by editing the system `.GlobalPreferences.plist`, and exposes that screen from the existing Tweaks navigation.

**Changes:**
- Introduce `LGView` to load/edit/apply `/var/Managed Preferences/mobile/.GlobalPreferences.plist` via toggles and an “Apply” action.
- Add a conditional “Liquid Glass” `NavigationLink` in `ContentView` gated by OS version.

### Reviewed changes

Copilot reviewed 2 out of 2 changed files in this pull request and generated 5 comments.

| File | Description |
| ---- | ----------- |
| `lara/views/LGView.swift` | New view for loading/editing/applying GlobalPreferences Liquid Glass-related keys, with refresh/apply/respring UI. |
| `lara/views/ContentView.swift` | Adds OS-version-gated entry point to `LGView` from the Tweaks list. |





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (5)

*Inline comments on code changes*

### `lara/views/LGView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 24, 2026 (lines 113-139) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,164 @@
+//
+//  LGView.swift
+//  lara
+//
+//  Created by jurre111 on 24.04.26.
+//
+
+// Most of the code is from Duy's SparseBox
+
+import SwiftUI
+
+struct LGView: View {
+    @ObservedObject private var mgr = laramgr.shared
+    @State private var gp: NSMutableDictionary
+    @State private var status: String?
+    @State private var alert: String?
+    @State private var valid: Bool = true
+    
+    private let path = "/var/Managed Preferences/mobile/.GlobalPreferences.plist"
+    private let oggpurl: URL
+
+    init() {
+        let docs = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
+        oggpurl = docs.appendingPathComponent("ogGlobalPreferences.plist")
+        let sysurl = URL(fileURLWithPath: path)
+        do {
+            if !FileManager.default.fileExists(atPath: oggpurl.path) {
+                try FileManager.default.copyItem(at: sysurl, to: oggpurl)
+            }
+            chmod(oggpurl.path, 0o644)
+            
+            _gp = State(initialValue: try NSMutableDictionary(contentsOf: URL(fileURLWithPath: path), error: ()))
+        } catch {
+            _gp = State(initialValue: [:])
+            _status = State(initialValue: "Failed to copy GlobalPreferences: \(error)")
+        }
+
+    }
+
+    var body: some View {
+        NavigationStack {
+            List {
+                Section {
+                    Toggle("Force Solarium Fallback", isOn: gpkeybinding("SolariumForceFallback"))
+                    Toggle("Disable Liquid Glass", isOn: gpkeybinding("com.apple.SwiftUI.DisableSolarium"))
+                    Toggle("Ignore Liquid Glass App Build Check", isOn: gpkeybinding("com.apple.SwiftUI.IgnoreSolariumLinkedOnCheck"))
+                    Toggle("Disable Liquid Glass on LS Clock", isOn: gpkeybinding("SBDisallowGlassTime"))
+                    Toggle("Disable Liquid Glass on Dock", isOn: gpkeybinding("SBDisableGlassDock"))
+                    Toggle("Disable Specular Motion", isOn: gpkeybinding("SBDisableSpecularEverywhereUsingLSSAssertion"))
+                    Toggle("Disable Outer Refraction", isOn: gpkeybinding("SolariumDisableOuterRefraction"))
+                    Toggle("Disable Solarium HDR", isOn: gpkeybinding("SolariumAllowHDR", default: true, enable: false))
+                } header: {
+                    Text("Liquid Glass")
+                } footer: {
+                    Text("Note: some tweaks may not work or cause instability.")
+                }
+                Section {
+                    HStack {
+                        Text("Status")
+                        
+                        Spacer()
+                        
+                        if valid {
+                            Text("valid!")
+                                .monospaced(true)
+                                .foregroundColor(.green)
+                        } else {
+                            Text("invalid.")
+                                .monospaced(true)
+                                .foregroundColor(.red)
+                        }
+                    }
+                    Button() {
+                        load()
+                    } label: {
+                        Text("Refresh plist")
+                    }
+                    Button() {
+                        apply()
+                    } label: {
+                        Text("Apply")
+                    }
+                    .disabled(!valid)
+                } header: {
+                    Text("Actions")
+                } footer: {
+                    Text("Use at your own risk. Always keep a backup of \"/var/Managed Preferences/mobile/.GlobalPreferences.plist\" somewhere safe.")
+                }
+            }
+            .navigationTitle("Liquid Glass")
+            .alert("Status", isPresented: .constant(status != nil)) {
+                Button("OK") { status = nil }
+            } message: {
+                Text(status ?? "")
+            }
+            .alert("Done", isPresented: .constant(alert != nil)) {
+                Button("Cancel") { alert = nil }
+                Button("Respring") {
+                    mgr.respring()
+                }
+            } message: {
+                Text(alert ?? "uhh...")
+            }
+            .onAppear(perform: load)
+        }
+    }
+    
+    private func validate(_ dict: NSMutableDictionary) -> Bool {
+        return !gp.allKeys.isEmpty
+    }
+
+    private func load() {
+        do {
+            gp = try NSMutableDictionary(contentsOf: URL(fileURLWithPath: path), error: ())
+        } catch {
+            status = "Failed to load GlobalPreferences"
+        }
+    }
+
+    private func apply() {
+        do {
+            let data = try PropertyListSerialization.data(
+                fromPropertyList: gp,
+                format: .binary,
+                options: 0
+            )
+            
+            let result = laramgr.shared.lara_overwritefile(
+                target: path,
+                data: data
+            )
+            load()
+            if result.ok && validate(gp) {
+                mgr.logmsg("overwrote GlobalPreferences.plist at \(path)")
+                alert = "Applied plist, respring to see changes."
+            } else {
+                status = "overwrite failed: \(result.message)"
+            }
```

</details>

`apply()` serializes and overwrites the target file before checking `validate(gp)`, and then immediately calls `load()` even when `result.ok` is false—this can discard the user's pending changes and (in the invalid case) can overwrite the system plist with bad data. Validate before serializing/writing, and only reload from disk after a successful overwrite.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 24, 2026 (line 98) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,164 @@
+//
+//  LGView.swift
+//  lara
+//
+//  Created by jurre111 on 24.04.26.
+//
+
+// Most of the code is from Duy's SparseBox
+
+import SwiftUI
+
+struct LGView: View {
+    @ObservedObject private var mgr = laramgr.shared
+    @State private var gp: NSMutableDictionary
+    @State private var status: String?
+    @State private var alert: String?
+    @State private var valid: Bool = true
+    
+    private let path = "/var/Managed Preferences/mobile/.GlobalPreferences.plist"
+    private let oggpurl: URL
+
+    init() {
+        let docs = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
+        oggpurl = docs.appendingPathComponent("ogGlobalPreferences.plist")
+        let sysurl = URL(fileURLWithPath: path)
+        do {
+            if !FileManager.default.fileExists(atPath: oggpurl.path) {
+                try FileManager.default.copyItem(at: sysurl, to: oggpurl)
+            }
+            chmod(oggpurl.path, 0o644)
+            
+            _gp = State(initialValue: try NSMutableDictionary(contentsOf: URL(fileURLWithPath: path), error: ()))
+        } catch {
+            _gp = State(initialValue: [:])
+            _status = State(initialValue: "Failed to copy GlobalPreferences: \(error)")
+        }
+
+    }
+
+    var body: some View {
+        NavigationStack {
+            List {
+                Section {
+                    Toggle("Force Solarium Fallback", isOn: gpkeybinding("SolariumForceFallback"))
+                    Toggle("Disable Liquid Glass", isOn: gpkeybinding("com.apple.SwiftUI.DisableSolarium"))
+                    Toggle("Ignore Liquid Glass App Build Check", isOn: gpkeybinding("com.apple.SwiftUI.IgnoreSolariumLinkedOnCheck"))
+                    Toggle("Disable Liquid Glass on LS Clock", isOn: gpkeybinding("SBDisallowGlassTime"))
+                    Toggle("Disable Liquid Glass on Dock", isOn: gpkeybinding("SBDisableGlassDock"))
+                    Toggle("Disable Specular Motion", isOn: gpkeybinding("SBDisableSpecularEverywhereUsingLSSAssertion"))
+                    Toggle("Disable Outer Refraction", isOn: gpkeybinding("SolariumDisableOuterRefraction"))
+                    Toggle("Disable Solarium HDR", isOn: gpkeybinding("SolariumAllowHDR", default: true, enable: false))
+                } header: {
+                    Text("Liquid Glass")
+                } footer: {
+                    Text("Note: some tweaks may not work or cause instability.")
+                }
+                Section {
+                    HStack {
+                        Text("Status")
+                        
+                        Spacer()
+                        
+                        if valid {
+                            Text("valid!")
+                                .monospaced(true)
+                                .foregroundColor(.green)
+                        } else {
+                            Text("invalid.")
+                                .monospaced(true)
+                                .foregroundColor(.red)
+                        }
+                    }
+                    Button() {
+                        load()
+                    } label: {
+                        Text("Refresh plist")
+                    }
+                    Button() {
+                        apply()
+                    } label: {
+                        Text("Apply")
+                    }
+                    .disabled(!valid)
+                } header: {
+                    Text("Actions")
+                } footer: {
+                    Text("Use at your own risk. Always keep a backup of \"/var/Managed Preferences/mobile/.GlobalPreferences.plist\" somewhere safe.")
+                }
+            }
+            .navigationTitle("Liquid Glass")
+            .alert("Status", isPresented: .constant(status != nil)) {
+                Button("OK") { status = nil }
+            } message: {
+                Text(status ?? "")
+            }
+            .alert("Done", isPresented: .constant(alert != nil)) {
+                Button("Cancel") { alert = nil }
+                Button("Respring") {
```

</details>

In the "Respring" action, `alert` isn't cleared before calling `mgr.respring()`. If respring fails or the view stays alive, the alert can remain stuck presented because `alert != nil` stays true. Clear `alert` (set it to nil) before triggering the respring, consistent with other views (e.g., EditorView).


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 24, 2026 (line 109) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,164 @@
+//
+//  LGView.swift
+//  lara
+//
+//  Created by jurre111 on 24.04.26.
+//
+
+// Most of the code is from Duy's SparseBox
+
+import SwiftUI
+
+struct LGView: View {
+    @ObservedObject private var mgr = laramgr.shared
+    @State private var gp: NSMutableDictionary
+    @State private var status: String?
+    @State private var alert: String?
+    @State private var valid: Bool = true
+    
+    private let path = "/var/Managed Preferences/mobile/.GlobalPreferences.plist"
+    private let oggpurl: URL
+
+    init() {
+        let docs = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
+        oggpurl = docs.appendingPathComponent("ogGlobalPreferences.plist")
+        let sysurl = URL(fileURLWithPath: path)
+        do {
+            if !FileManager.default.fileExists(atPath: oggpurl.path) {
+                try FileManager.default.copyItem(at: sysurl, to: oggpurl)
+            }
+            chmod(oggpurl.path, 0o644)
+            
+            _gp = State(initialValue: try NSMutableDictionary(contentsOf: URL(fileURLWithPath: path), error: ()))
+        } catch {
+            _gp = State(initialValue: [:])
+            _status = State(initialValue: "Failed to copy GlobalPreferences: \(error)")
+        }
+
+    }
+
+    var body: some View {
+        NavigationStack {
+            List {
+                Section {
+                    Toggle("Force Solarium Fallback", isOn: gpkeybinding("SolariumForceFallback"))
+                    Toggle("Disable Liquid Glass", isOn: gpkeybinding("com.apple.SwiftUI.DisableSolarium"))
+                    Toggle("Ignore Liquid Glass App Build Check", isOn: gpkeybinding("com.apple.SwiftUI.IgnoreSolariumLinkedOnCheck"))
+                    Toggle("Disable Liquid Glass on LS Clock", isOn: gpkeybinding("SBDisallowGlassTime"))
+                    Toggle("Disable Liquid Glass on Dock", isOn: gpkeybinding("SBDisableGlassDock"))
+                    Toggle("Disable Specular Motion", isOn: gpkeybinding("SBDisableSpecularEverywhereUsingLSSAssertion"))
+                    Toggle("Disable Outer Refraction", isOn: gpkeybinding("SolariumDisableOuterRefraction"))
+                    Toggle("Disable Solarium HDR", isOn: gpkeybinding("SolariumAllowHDR", default: true, enable: false))
+                } header: {
+                    Text("Liquid Glass")
+                } footer: {
+                    Text("Note: some tweaks may not work or cause instability.")
+                }
+                Section {
+                    HStack {
+                        Text("Status")
+                        
+                        Spacer()
+                        
+                        if valid {
+                            Text("valid!")
+                                .monospaced(true)
+                                .foregroundColor(.green)
+                        } else {
+                            Text("invalid.")
+                                .monospaced(true)
+                                .foregroundColor(.red)
+                        }
+                    }
+                    Button() {
+                        load()
+                    } label: {
+                        Text("Refresh plist")
+                    }
+                    Button() {
+                        apply()
+                    } label: {
+                        Text("Apply")
+                    }
+                    .disabled(!valid)
+                } header: {
+                    Text("Actions")
+                } footer: {
+                    Text("Use at your own risk. Always keep a backup of \"/var/Managed Preferences/mobile/.GlobalPreferences.plist\" somewhere safe.")
+                }
+            }
+            .navigationTitle("Liquid Glass")
+            .alert("Status", isPresented: .constant(status != nil)) {
+                Button("OK") { status = nil }
+            } message: {
+                Text(status ?? "")
+            }
+            .alert("Done", isPresented: .constant(alert != nil)) {
+                Button("Cancel") { alert = nil }
+                Button("Respring") {
+                    mgr.respring()
+                }
+            } message: {
+                Text(alert ?? "uhh...")
+            }
+            .onAppear(perform: load)
+        }
+    }
+    
+    private func validate(_ dict: NSMutableDictionary) -> Bool {
+        return !gp.allKeys.isEmpty
```

</details>

`validate(_:)` ignores its `dict` parameter and instead checks `gp`. This makes the parameter misleading and can mask logic errors (e.g., validating a freshly loaded dictionary). Use the passed-in dictionary for validation (and remove the unused parameter if you always intend to validate `gp`).


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 24, 2026 (lines 103-111) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,164 @@
+//
+//  LGView.swift
+//  lara
+//
+//  Created by jurre111 on 24.04.26.
+//
+
+// Most of the code is from Duy's SparseBox
+
+import SwiftUI
+
+struct LGView: View {
+    @ObservedObject private var mgr = laramgr.shared
+    @State private var gp: NSMutableDictionary
+    @State private var status: String?
+    @State private var alert: String?
+    @State private var valid: Bool = true
+    
+    private let path = "/var/Managed Preferences/mobile/.GlobalPreferences.plist"
+    private let oggpurl: URL
+
+    init() {
+        let docs = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
+        oggpurl = docs.appendingPathComponent("ogGlobalPreferences.plist")
+        let sysurl = URL(fileURLWithPath: path)
+        do {
+            if !FileManager.default.fileExists(atPath: oggpurl.path) {
+                try FileManager.default.copyItem(at: sysurl, to: oggpurl)
+            }
+            chmod(oggpurl.path, 0o644)
+            
+            _gp = State(initialValue: try NSMutableDictionary(contentsOf: URL(fileURLWithPath: path), error: ()))
+        } catch {
+            _gp = State(initialValue: [:])
+            _status = State(initialValue: "Failed to copy GlobalPreferences: \(error)")
+        }
+
+    }
+
+    var body: some View {
+        NavigationStack {
+            List {
+                Section {
+                    Toggle("Force Solarium Fallback", isOn: gpkeybinding("SolariumForceFallback"))
+                    Toggle("Disable Liquid Glass", isOn: gpkeybinding("com.apple.SwiftUI.DisableSolarium"))
+                    Toggle("Ignore Liquid Glass App Build Check", isOn: gpkeybinding("com.apple.SwiftUI.IgnoreSolariumLinkedOnCheck"))
+                    Toggle("Disable Liquid Glass on LS Clock", isOn: gpkeybinding("SBDisallowGlassTime"))
+                    Toggle("Disable Liquid Glass on Dock", isOn: gpkeybinding("SBDisableGlassDock"))
+                    Toggle("Disable Specular Motion", isOn: gpkeybinding("SBDisableSpecularEverywhereUsingLSSAssertion"))
+                    Toggle("Disable Outer Refraction", isOn: gpkeybinding("SolariumDisableOuterRefraction"))
+                    Toggle("Disable Solarium HDR", isOn: gpkeybinding("SolariumAllowHDR", default: true, enable: false))
+                } header: {
+                    Text("Liquid Glass")
+                } footer: {
+                    Text("Note: some tweaks may not work or cause instability.")
+                }
+                Section {
+                    HStack {
+                        Text("Status")
+                        
+                        Spacer()
+                        
+                        if valid {
+                            Text("valid!")
+                                .monospaced(true)
+                                .foregroundColor(.green)
+                        } else {
+                            Text("invalid.")
+                                .monospaced(true)
+                                .foregroundColor(.red)
+                        }
+                    }
+                    Button() {
+                        load()
+                    } label: {
+                        Text("Refresh plist")
+                    }
+                    Button() {
+                        apply()
+                    } label: {
+                        Text("Apply")
+                    }
+                    .disabled(!valid)
+                } header: {
+                    Text("Actions")
+                } footer: {
+                    Text("Use at your own risk. Always keep a backup of \"/var/Managed Preferences/mobile/.GlobalPreferences.plist\" somewhere safe.")
+                }
+            }
+            .navigationTitle("Liquid Glass")
+            .alert("Status", isPresented: .constant(status != nil)) {
+                Button("OK") { status = nil }
+            } message: {
+                Text(status ?? "")
+            }
+            .alert("Done", isPresented: .constant(alert != nil)) {
+                Button("Cancel") { alert = nil }
+                Button("Respring") {
+                    mgr.respring()
+                }
+            } message: {
+                Text(alert ?? "uhh...")
+            }
+            .onAppear(perform: load)
+        }
+    }
+    
+    private func validate(_ dict: NSMutableDictionary) -> Bool {
+        return !gp.allKeys.isEmpty
+    }
+
+    private func load() {
+        do {
+            gp = try NSMutableDictionary(contentsOf: URL(fileURLWithPath: path), error: ())
+        } catch {
+            status = "Failed to load GlobalPreferences"
+        }
+    }
```

</details>

`valid` is never recalculated after `load()` (or after the init error path), so the UI can show "valid" and enable Apply even when the plist failed to load (e.g., `gp` is empty after init catch). Update `valid` after successfully loading, and set it to false on load failure to prevent overwriting with an invalid/empty dictionary.


### `lara/views/ContentView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 24, 2026 (lines 338-342) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -333,6 +335,12 @@ struct ContentView: View {
                                             EditorView()
                                         }
 
+                                        if os.majorVersion >= 26 {
+                                            NavigationLink("Liquid Glass") {
+                                                LGView()
+                                            }
+                                        }
```

</details>

PR description says this is "only useful for iOS 26.0 and 26.0.1", but the UI gate is `os.majorVersion >= 26` which will also show it on later 26.x/27+. If the feature is known to be safe/valid only on 26.0/26.0.1, gate on the specific minor/patch versions (or use `#available(iOS 26, *)` plus an explicit minor/patch check).



---

## PR Commits (14)

- [`7e9c8d0`](https://github.com/rooootdev/lara/commit/7e9c8d0222ac83eda141093571246d03210dcdfc) Add LGView.swift — *jurre111* · Apr 24, 2026
- [`a9e048c`](https://github.com/rooootdev/lara/commit/a9e048c2b2deefb6b661048e440ad4877331d2f3) Fixes — *jurre111* · Apr 24, 2026
- [`2a50cbd`](https://github.com/rooootdev/lara/commit/2a50cbd5d1c62033326d570eb0646a19f6056d38) stupid — *jurre111* · Apr 24, 2026
- [`c70e736`](https://github.com/rooootdev/lara/commit/c70e736034b4acf8e20beb93bc81225b786c2fd0) Minor Improvements — *jurre111* · Apr 24, 2026
- [`e1bd4e0`](https://github.com/rooootdev/lara/commit/e1bd4e0d8c14a970b9dc804cf5ffb209f11431ab) Change path to real one — *jurre111* · Apr 24, 2026
- [`6f9f310`](https://github.com/rooootdev/lara/commit/6f9f3100186630eac8f517e7855ddb7293fc70eb) Add version check — *jurre111* · Apr 24, 2026
- [`b896188`](https://github.com/rooootdev/lara/commit/b896188eb8c9f08e40dea5d85616b415122a3963) Fix — *jurre111* · Apr 24, 2026
- [`d1b91c2`](https://github.com/rooootdev/lara/commit/d1b91c251b978b4323a7d311ce795bb03245f47c) again — *jurre111* · Apr 24, 2026
- [`d64ffe8`](https://github.com/rooootdev/lara/commit/d64ffe8a44dd01b18aa4a9305dc1f2a766010387) Correct path — *jurre111* · Apr 24, 2026
- [`7ac4815`](https://github.com/rooootdev/lara/commit/7ac4815872e94cb0032387d42cb80cafa874c6ad) Update footer — *jurre111* · Apr 24, 2026
- [`3202967`](https://github.com/rooootdev/lara/commit/320296718ed8a4d085d1759e9c9b13d70255cd89) Merge branch 'rooootdev:main' into DisableLG — *Jurre* · Apr 24, 2026
- [`aac5c46`](https://github.com/rooootdev/lara/commit/aac5c469b0412b7d677098762be0bae8ce4d5cd1) Some improvements again :) — *jurre111* · Apr 24, 2026
- [`124c060`](https://github.com/rooootdev/lara/commit/124c060993e2f565bd8d4af72ee44ecd9f91152e) Merge branch 'DisableLG' of https://github.com/jurre111/lara into DisableLG — *jurre111* · Apr 24, 2026
- [`5cea096`](https://github.com/rooootdev/lara/commit/5cea09600be091d32f126f590af51400f1663c9d) Better now — *jurre111* · Apr 24, 2026


---

## Changed Files (2)

- 🟡 `lara/views/ContentView.swift` (modified) — +8 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -15,6 +15,8 @@ struct ContentView: View {
     @State private var showsettings = false
     @State private var selectedmethod: method = .hybrid
 
+    let os = ProcessInfo().operatingSystemVersion
+
     var body: some View {
         NavigationStack {
             List {
@@ -333,6 +335,12 @@ struct ContentView: View {
                                             EditorView()
                                         }
 
+                                        if os.majorVersion >= 26 {
+                                            NavigationLink("Liquid Glass") {
+                                                LGView()
+                                            }
+                                        }
+
                                         NavigationLink("3 App Bypass") {
                                             AppsView(mgr: mgr)
                                         }
  ```

  </details>
- 🟢 `lara/views/LGView.swift` (added) — +165 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,165 @@
+//
+//  LGView.swift
+//  lara
+//
+//  Created by jurre111 on 24.04.26.
+//
+
+// Credits to leminlimez and Duy Tran for most of the code
+
+import SwiftUI
+
+struct LGView: View {
+    @ObservedObject private var mgr = laramgr.shared
+    @State private var gp: NSMutableDictionary
+    @State private var status: String?
+    @State private var valid: Bool = true
+    
+    private let path = "/var/Managed Preferences/mobile/.GlobalPreferences.plist"
+    private let oggpurl: URL
+
+    init() {
+        let docs = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
+        oggpurl = docs.appendingPathComponent("ogGlobalPreferences.plist")
+        let sysurl = URL(fileURLWithPath: path)
+        do {
+            if !FileManager.default.fileExists(atPath: oggpurl.path) {
+                try FileManager.default.copyItem(at: sysurl, to: oggpurl)
+            }
+            chmod(oggpurl.path, 0o644)
+            
+            _gp = State(initialValue: try NSMutableDictionary(contentsOf: URL(fileURLWithPath: path), error: ()))
+        } catch {
+            _gp = State(initialValue: [:])
+            _status = State(initialValue: "Failed to copy GlobalPreferences: \(error)")
+        }
+
+    }
+
+    var body: some View {
+        NavigationStack {
+            List {
+                Section {
+                    Toggle("Force Solarium Fallback", isOn: gpkeybinding("SolariumForceFallback"))
+                    Toggle("Disable Liquid Glass", isOn: gpkeybinding("com.apple.SwiftUI.DisableSolarium"))
+                    Toggle("Ignore Liquid Glass App Build Check", isOn: gpkeybinding("com.apple.SwiftUI.IgnoreSolariumLinkedOnCheck"))
+                    Toggle("Disable Liquid Glass on LS Clock", isOn: gpkeybinding("SBDisallowGlassTime"))
+                    Toggle("Disable Liquid Glass on Dock", isOn: gpkeybinding("SBDisableGlassDock"))
+                    Toggle("Disable Specular Motion", isOn: gpkeybinding("SBDisableSpecularEverywhereUsingLSSAssertion"))
+                    Toggle("Disable Outer Refraction", isOn: gpkeybinding("SolariumDisableOuterRefraction"))
+                    Toggle("Disable Solarium HDR", isOn: gpkeybinding("SolariumAllowHDR", default: true, enable: false))
+                } header: {
+                    Text("Liquid Glass")
+                } footer: {
+                    Text("Note: some tweaks may not work or cause instability.")
+                }
+                Section {
+                    HStack {
+                        Text("Status")
+                        
+                        Spacer()
+                        
+                        if valid {
+                            Text("valid!")
+                                .monospaced(true)
+                                .foregroundColor(.green)
+                        } else {
+                            Text("invalid.")
+                                .monospaced(true)
+                                .foregroundColor(.red)
+                        }
+                    }
+                    Button() {
+                        load()
+                    } label: {
+                        Text("Refresh plist")
+                    }
+                    Button() {
+                        apply()
+                    } label: {
+                        Text("Apply")
+                    }
+                    .disabled(!valid)
+                } header: {
+                    Text("Actions")
+                } footer: {
+                    Text("Use at your own risk. Always keep a backup of \"/var/Managed Preferences/mobile/.GlobalPreferences.plist\" somewhere safe.")
+                }
+            }
+            .navigationTitle("Liquid Glass")
+            .alert("Status", isPresented: .constant(status != nil)) {
+                Button("OK") { status = nil }
+            } message: {
+                Text(status ?? "")
+            }
+            .onAppear(perform: load)
+        }
+    }
+    
+    private func validate(_ dict: NSMutableDictionary) -> Bool {
+        return !dict.allKeys.isEmpty
+    }
+
+    private func load() {
+        do {
+            gp = try NSMutableDictionary(contentsOf: URL(fileURLWithPath: path), error: ())
+        } catch {
+            status = "Failed to load GlobalPreferences"
+        }
+
+        valid = validate(gp)
+    }
+
+    private func apply() {
+        if !validate(gp) {
+            status = "Plist is invalid."
+            return
+        }
+        do {
+            let data = try PropertyListSerialization.data(
+                fromPropertyList: gp,
+                format: .binary,
+                options: 0
+            )
+            
+            let result = laramgr.shared.lara_overwritefile(
+                target: path,
+                data: data
+            )
+            if result.ok {
+                load()
+                if valid {
+                    mgr.logmsg("overwrote GlobalPreferences.plist at \(path)")
+                    status = "Applied plist, reboot to see changes."
+                } else {
+                    status = "Applied plist but it's invalid. Don't respring and copy backup (lara/Documents/ogGlobalPreferences.plist) to orignal location."
+                }
+            } else {
+                status = "overwrite failed: \(result.message)"
+            }
+            
+        } catch {
+            status = "serialization failed: \(error.localizedDescription)"
+        }
+    }
+    
+    private func gpkeybinding<T: Equatable>(_ key: String, type: T.Type = Bool.self, default: T? = false, enable: T? = true) -> Binding<Bool> {
+        return Binding(
+            get: {
+                if let value = gp[key] as? T?, let enable {
+                    return value == enable
+                }
+                return false
+            },
+            set: { enabled in
+                if enabled {
+                    gp[key] = enable
+                } else {
+                    gp.removeObject(forKey: key)
+                }
+                
+                valid = validate(gp)
+            }
+        )
+    }
+}
  ```

  </details>


---

## References

### Commits (4)

- [930a0c7](https://github.com/letienndat/lara/commit/930a0c76a6cc42f0c74438a29d93ff7efdc56291) — Squashed commit of the following:
- [07c3a6c](https://github.com/letienndat/lara/commit/07c3a6ccd3a1ce37626a6ac44f59b3eb69bffed2) — Squashed commit of the following:
- [e0da526](https://github.com/letienndat/lara/commit/e0da526996585fa0cb7aaf3d6479a69cc2a8d2a4) — Squashed commit of the following:
- [9fc6ffb](https://github.com/letienndat/lara/commit/9fc6ffb8e54049fe785acc3b6361633ea2578ce1) — Squashed commit of the following:


---

## Timeline (Verbose)

- : 📝 Commit 7e9c8d0: Add LGView.swift
- : 📝 Commit a9e048c: Fixes
- : 📝 Commit 2a50cbd: stupid
- : 📝 Commit c70e736: Minor Improvements
- : 📝 Commit e1bd4e0: Change path to real one
- : 📝 Commit 6f9f310: Add version check
- : 📝 Commit b896188: Fix
- : 📝 Commit d1b91c2: again
- : 📝 Commit d64ffe8: Correct path
- : 📝 Commit 7ac4815: Update footer
- : 📝 Commit 3202967: Merge branch 'rooootdev:main' into DisableLG
- Apr 24, 2026: 🟢 @jurre111 marked ready for review
- Apr 24, 2026: 👀 @jurre111 requested review from @Copilot
- : 💬 someone reviewed: commented
- : 📝 Commit aac5c46: Some improvements again :)
- : 📝 Commit 124c060: Merge branch 'DisableLG' of https://github.com/jurre111/lara into DisableLG
- : 📝 Commit 5cea096: Better now
- Apr 26, 2026: 🟣 @rooootdev merged
- Apr 26, 2026: 🔴 Closed by @rooootdev
- Apr 26, 2026: 🗑️ @jurre111 deleted head branch
- Apr 26, 2026: 🔗 Commit 930a0c7 — Squashed commit of the following:
- Apr 27, 2026: 🔗 Commit 07c3a6c — Squashed commit of the following:
- Apr 29, 2026: 🔗 Commit e0da526 — Squashed commit of the following:
- Apr 29, 2026: 🔗 Commit 9fc6ffb — Squashed commit of the following:


---

*Exported on 10/14/2026, 3:01:49 PM from [https://github.com/rooootdev/lara/pull/186](https://github.com/rooootdev/lara/pull/186)*
