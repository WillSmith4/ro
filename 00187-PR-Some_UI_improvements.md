# Some UI improvements

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#187](https://github.com/rooootdev/lara/pull/187)  
> **State:** 🟣 merged  
> **Branch:** `jurre111:UI-improvements` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 26, 2026  
> **Changes:** +251 / -169 across 4 files  
> **Created:** Apr 24, 2026  
> **Updated:** Apr 26, 2026  
> **Closed:** Apr 26, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on Apr 24, 2026*


Tweaks: Text --> Labels and split into categories
Move Offsets in settings into its own NavigationLink
Share button in LogsView
An extra warning and reload button in EditorView


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- 🔴 [build](https://github.com/rooootdev/lara/actions/runs/24953287840/job/73067110825) — **failure** — *GitHub Actions*
- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24953287840/job/73067145369) — **skipped** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 25, 2026

## Pull request overview

This PR refines Lara’s SwiftUI navigation and labeling to make the UI more structured, and adds a couple of convenience actions (sharing logs, reloading editor state) plus additional warning messaging for a risky editor toggle.

**Changes:**
- Reorganized “Tweaks” navigation lists into categorized `Section`s and replaced plain text rows with `Label`-based rows.
- Moved the offsets display/edit UI in Settings into a dedicated `NavigationLink` destination.
- Added a log share action in `LogsView` and a “Reload from plist” action + warning text updates in `EditorView`.

### Reviewed changes

Copilot reviewed 4 out of 4 changed files in this pull request and generated 4 comments.

| File | Description |
| ---- | ----------- |
| lara/views/SettingsView.swift | Moves offsets UI into a separate navigation destination (“Modify Offsets”). |
| lara/views/EditorView.swift | Adds reload action and expands warning messaging when toggling “Become iPadOS”. |
| lara/views/ContentView.swift | Categorizes tweak destinations into sections and switches to `Label` rows with SF Symbols. |
| lara/classes/Logger.swift | Adds a ShareLink to export the `lara.log` file from the Logs screen toolbar. |





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (4)

*Inline comments on code changes*

### `lara/views/ContentView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 25, 2026 (line 382) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -311,46 +354,76 @@ struct ContentView: View {
                             if mgr.vfsready && mgr.sbxready {
                                 NavigationLink("Tweaks") {
                                     List {
-                                        if !showfmintabs {
-                                            NavigationLink("File Manager") {
-                                                SantanderView(startPath: "/")
+                                        Section {
+                                            NavigationLink {
+                                                FontPicker(mgr: mgr)
+                                            } label: {
+                                                Label("Font Overwrite", systemImage: "textformat.alt")
                                             }
-                                        }
-
-                                        NavigationLink("Font Overwrite") {
-                                            FontPicker(mgr: mgr)
-                                        }
 
-                                        NavigationLink("Card Overwrite") {
-                                            CardView()
-                                        }
-
-                                        NavigationLink("Custom Overwrite") {
-                                            CustomView(mgr: mgr)
-                                        }
+                                            NavigationLink {
+                                                CardView()
+                                            } label: {
+                                                Label("Card Overwrite", systemImage: "creditcard")
+                                            }
 
-                                        NavigationLink("MobileGestalt") {
-                                            EditorView()
+                                            NavigationLink {
+                                                ZeroView(mgr: mgr)
+                                            } label: {
+                                                Label("DirtyZero", systemImage: "doc")
+                                            }
+                                            
+                                            NavigationLink {
+                                                DarkBoardView()
+                                            } label: {
+                                                Label("DarkBoard", systemImage: "app.badge")
+                                            }
+                                        } header: {
+                                            Text("UI tweaks")
```

</details>

Section header casing is inconsistent ("UI Tweaks" elsewhere vs "UI tweaks" here). Align the capitalization to keep the UI consistent.
```suggestion
                                            Text("UI Tweaks")
```


### `lara/views/EditorView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 25, 2026 (line 278) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -271,6 +275,7 @@ struct EditorView: View {
                 return false
             },
             set: { enabled in
+                status = "Become iPadOS is a risky feature so please take note of the following:\n\n1. iOS Only apps, like WhatsApp, can lose data. It's reccomended to offload these apps.\n2. The homescreen layout can get fucked if you have empty space.\n3. If you have an alphabetical password, it's very hard to get into your phone after locking.\n4. Any option with Dock under Stage Manager should NOT be modified.\n\n Only continue if you're okay with this. Else click reload from plist"
```

</details>

`status` is set to the warning message on every toggle change (both enabling and disabling). This will show the warning even when turning the feature off; consider only showing the warning when `enabled` is true (or use a confirmation dialog before applying changes).
```suggestion
                if enabled {
                    status = "Become iPadOS is a risky feature so please take note of the following:\n\n1. iOS Only apps, like WhatsApp, can lose data. It's reccomended to offload these apps.\n2. The homescreen layout can get fucked if you have empty space.\n3. If you have an alphabetical password, it's very hard to get into your phone after locking.\n4. Any option with Dock under Stage Manager should NOT be modified.\n\n Only continue if you're okay with this. Else click reload from plist"
                } else {
                    status = nil
                }
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 25, 2026 (line 278) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -271,6 +275,7 @@ struct EditorView: View {
                 return false
             },
             set: { enabled in
+                status = "Become iPadOS is a risky feature so please take note of the following:\n\n1. iOS Only apps, like WhatsApp, can lose data. It's reccomended to offload these apps.\n2. The homescreen layout can get fucked if you have empty space.\n3. If you have an alphabetical password, it's very hard to get into your phone after locking.\n4. Any option with Dock under Stage Manager should NOT be modified.\n\n Only continue if you're okay with this. Else click reload from plist"
```

</details>

Spelling: "reccomended" should be "recommended".
```suggestion
                status = "Become iPadOS is a risky feature so please take note of the following:\n\n1. iOS Only apps, like WhatsApp, can lose data. It's recommended to offload these apps.\n2. The homescreen layout can get fucked if you have empty space.\n3. If you have an alphabetical password, it's very hard to get into your phone after locking.\n4. Any option with Dock under Stage Manager should NOT be modified.\n\n Only continue if you're okay with this. Else click reload from plist"
```


### `lara/classes/Logger.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 25, 2026 (line 384) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -375,6 +379,10 @@ struct LogsView: View {
             .navigationTitle("Logs")
             .toolbar {
                 ToolbarItemGroup(placement: .navigationBarTrailing) {
+                    ShareLink(item: logsURL) {
+                        Image(systemName: "square.and.arrow.up")
+                    }
```

</details>

The ShareLink in the toolbar is icon-only and doesn't provide an explicit accessibility label, which can make it unclear to VoiceOver users. Consider using a `Label("Share logs", systemImage: ...)` or adding `.accessibilityLabel("Share logs")`.
```suggestion
                    }
                    .accessibilityLabel("Share logs")
```



---

## PR Commits (8)

- [`d9277bb`](https://github.com/rooootdev/lara/commit/d9277bbb3e565dba07f21796b3b75e8e2c56eba6) Tweaks: Text --> Labels and split into categories — *jurre111* · Apr 24, 2026
- [`869eb91`](https://github.com/rooootdev/lara/commit/869eb919323c1ef29fe3d646c3b96c6f2c057c45) Move Offsets in settings into its own NavigationLink — *jurre111* · Apr 24, 2026
- [`806bef4`](https://github.com/rooootdev/lara/commit/806bef43ba5350b070ef125821fcf9a3efc726f4) Add share button in LogsView — *jurre111* · Apr 25, 2026
- [`cdb6f85`](https://github.com/rooootdev/lara/commit/cdb6f85865482024cf6d41f3066b796f870b6bdc) Add warning in EditorView.swift and fix error — *jurre111* · Apr 25, 2026
- [`57a606b`](https://github.com/rooootdev/lara/commit/57a606b6e56d56d123220160bef4596db7fc40bc) EditorView: Add seperate warning for TrollPad and add reload button — *jurre111* · Apr 25, 2026
- [`99cea6a`](https://github.com/rooootdev/lara/commit/99cea6aa1eee78abc3db83633de75a8d6c109f1f) Improve warning message — *jurre111* · Apr 25, 2026
- [`9d608fc`](https://github.com/rooootdev/lara/commit/9d608fcc491c6c60bd1bb6e13dd5b8aaaae61ca6) Implement some of Copilot's suggestions — *jurre111* · Apr 25, 2026
- [`5518a97`](https://github.com/rooootdev/lara/commit/5518a97ca059476316c327517494eb895c6ade7c) Merge branch 'main' into UI-improvements — *roooot* · Apr 26, 2026


---

## Changed Files (4)

- 🟡 `lara/classes/Logger.swift` (modified) — +8 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -347,6 +347,10 @@ struct LogsView: View {
     @ObservedObject var logger: Logger
     
     private let nobullshitkey = "loggernobullshit"
+    let logsURL: URL = {
+        let docs = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
+        return docs.appendingPathComponent("lara.log")
+    }()
 
     var body: some View {
         NavigationStack {
@@ -375,6 +379,10 @@ struct LogsView: View {
             .navigationTitle("Logs")
             .toolbar {
                 ToolbarItemGroup(placement: .navigationBarTrailing) {
+                    ShareLink(item: logsURL) {
+                        Image(systemName: "square.and.arrow.up")
+                    }
+
                     Button {
                         let allLogs = logger.logs.joined(separator: "\n\n")
                         UIPasteboard.general.string = allLogs
  ```

  </details>
- 🟡 `lara/views/ContentView.swift` (modified) — +128 / -64
  <details><summary>Diff</summary>

  ```diff
  @@ -144,26 +144,44 @@ struct ContentView: View {
                             if mgr.vfsready {
                                 NavigationLink("Tweaks") {
                                     List {
-                                        NavigationLink("Font Overwrite") {
-                                            FontPicker(mgr: mgr)
-                                        }
-
-                                        NavigationLink("Card Overwrite") {
-                                            CardView()
-                                        }
+                                        Section {
+                                            NavigationLink {
+                                                FontPicker(mgr: mgr)
+                                            } label: {
+                                                Label("Font Overwrite", systemImage: "textformat.alt")
+                                            }
 
-                                        NavigationLink("Custom Overwrite") {
-                                            CustomView(mgr: mgr)
-                                        }
+                                            NavigationLink {
+                                                CardView()
+                                            } label: {
+                                                Label("Card Overwrite", systemImage: "creditcard")
+                                            }
 
-                                        NavigationLink("DirtyZero (Broken)") {
-                                            ZeroView(mgr: mgr)
+                                            NavigationLink {
+                                                ZeroView(mgr: mgr)
+                                            } label: {
+                                                Label("DirtyZero", systemImage: "doc")
+                                            }
+                                        } header: {
+                                            Text("UI Tweaks")
                                         }
 
-                                        if !showfmintabs {
-                                            NavigationLink("File Manager") {
-                                                SantanderView(startPath: "/")
+                                        Section {
+                                            if !showfmintabs {
+                                                NavigationLink {
+                                                    SantanderView(startPath: "/")
+                                                } label: {
+                                                    Label("File Manager", systemImage: "folder")
+                                                }
+                                            }
+                                            
+                                            NavigationLink {
+                                                CustomView(mgr: mgr)
+                                            } label: {
+                                                Label("Custom Overwrite", systemImage: "pencil")
                                             }
+                                        } header: {
+                                            Text("Other")
                                         }
                                     }
                                     .navigationTitle(Text("Tweaks"))
@@ -206,35 +224,60 @@ struct ContentView: View {
                             if mgr.sbxready {
                                 NavigationLink("Tweaks") {
                                     List {
-                                        if !showfmintabs {
-                                            NavigationLink("File Manager") {
-                                                SantanderView(startPath: "/")
+                                        Section {
+                                            NavigationLink {
+                                                CardView()
+                                            } label: {
+                                                Label("Card Overwrite", systemImage: "creditcard")
                                             }
+                                        } header: {
+                                            Text("UI Tweaks")
                                         }
 
-                                        NavigationLink("Card Overwrite") {
-                                            CardView()
-                                        }
+                                        Section {
+                                            NavigationLink {
+                                                AppsView(mgr: mgr)
+                                            } label: {
+                                                Label("3 App Bypass", systemImage: "lock.open.fill")
+                                            }
 
-                                        NavigationLink("3 App Bypass") {
-                                            AppsView(mgr: mgr)
+                                            NavigationLink {
+                                                WhitelistView()
+                                            } label: {
+                                                Label("Unblacklist (Broken?)", systemImage: "checkmark.seal")
+                                            }
+                                        } header: {
+                                            Text("App Management")
                                         }
 
-                                        NavigationLink("VarClean") {
-                                            VarCleanView()
-                                        }
+                                        Section {
+                                            if !showfmintabs {
+                                                NavigationLink {
+                                                    SantanderView(startPath: "/")
+                                                } label: {
+                                                    Label("File Manager", systemImage: "folder")
+                                                }
+                                            }
 
-                                        NavigationLink("Unblacklist (Broken?)") {
-                                            WhitelistView()
+                                            NavigationLink {
+                                                VarCleanView()
+                                            } label: {
+                                                Label("VarClean", systemImage: "sparkles")
+                                            }
+                                        } header: {
+                                            Text("Other")
                                         }
 
                                         if 1 == 2 {
-                                            NavigationLink("MobileGestalt") {
+                                            NavigationLink {
                                                 EditorView()
+                                            } label: {
+                                                Label("MobileGestalt", systemImage: "gear")
                                             }
-
-                                            NavigationLink("Passcode Theme") {
+                                            NavigationLink {
                                                 PasscodeView(mgr: mgr)
+                                            } label: {
+                                                Label("Passcode Theme", systemImage: "1.circle")
                                             }
                                         }
                                     }
@@ -313,27 +356,39 @@ struct ContentView: View {
                             if mgr.vfsready && mgr.sbxready {
                                 NavigationLink("Tweaks") {
                                     List {
-                                        if !showfmintabs {
-                                            NavigationLink("File Manager") {
-                                                SantanderView(startPath: "/")
+                                        Section {
+                                            NavigationLink {
+                                                FontPicker(mgr: mgr)
+                                            } label: {
+                                                Label("Font Overwrite", systemImage: "textformat.alt")
                                             }
-                                        }
 
-                                        NavigationLink("Font Overwrite") {
-                                            FontPicker(mgr: mgr)
-                                        }
-
-                                        NavigationLink("Card Overwrite") {
-                                            CardView()
-                                        }
-
-                                        NavigationLink("Custom Overwrite") {
-                                            CustomView(mgr: mgr)
-                                        }
+                                            NavigationLink {
+                                                CardView()
+                                            } label: {
+                                                Label("Card Overwrite", systemImage: "creditcard")
+                                            }
 
-                                        NavigationLink("MobileGestalt") {
-                                            EditorView()
+                                            NavigationLink {
+                                                ZeroView(mgr: mgr)
+                                            } label: {
+                                                Label("DirtyZero", systemImage: "doc")
+                                            }
+                                            
+                                            NavigationLink {
+                                                DarkBoardView()
+                                            } label: {
+                                                Label("DarkBoard", systemImage: "app.badge")
+                                            }
+                                        } header: {
+                                            Text("UI Tweaks")
                                         }
+                                        Section {
+                                            NavigationLink {
+                                                AppsView(mgr: mgr)
+                                            } label: {
+                                                Label("3 App Bypass", systemImage: "lock.open.fill")
+                                            }
 
                                         if os.majorVersion >= 26 {
                                             NavigationLink("Liquid Glass") {
@@ -344,21 +399,34 @@ struct ContentView: View {
                                         NavigationLink("3 App Bypass") {
                                             AppsView(mgr: mgr)
                                         }
+                                        Section {
+                                            if !showfmintabs {
+                                                NavigationLink {
+                                                    SantanderView(startPath: "/")
+                                                } label: {
+                                                    Label("File Manager", systemImage: "folder")
+                                                }
+                                            }
 
-                                        NavigationLink("VarClean") {
-                                            VarCleanView()
-                                        }
+                                            NavigationLink {
+                                                CustomView(mgr: mgr)
+                                            } label: {
+                                                Label("Custom Overwrite", systemImage: "pencil")
+                                            }
 
-                                        NavigationLink("Whitelist") {
-                                            WhitelistView()
-                                        }
+                                            NavigationLink {
+                                                EditorView()
+                                            } label: {
+                                                Label("MobileGestalt", systemImage: "gear")
+                                            }
 
-                                        NavigationLink("DirtyZero") {
-                                            ZeroView(mgr: mgr)
-                                        }
-                                        
-                                        NavigationLink("DarkBoard") {
-                                            DarkBoardView()
+                                            NavigationLink {
+                                                VarCleanView()
+                                            } label: {
+                                                Label("VarClean", systemImage: "sparkles")
+                                            }
+                                        } header: {
+                                            Text("Other")
                                         }
 
                                         if 1 == 2 {
@@ -369,10 +437,6 @@ struct ContentView: View {
                                             NavigationLink("Passcode Theme") {
                                                 PasscodeView(mgr: mgr)
                                             }
-
-                                            NavigationLink("3 App Bypass") {
-                                                AppsView(mgr: mgr)
-                                            }
                                         }
                                     }
                                     .navigationTitle(Text("Tweaks"))
  ```

  </details>
- 🟡 `lara/views/EditorView.swift` (modified) — +8 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -138,7 +138,11 @@ struct EditorView: View {
                                 .foregroundColor(.red)
                         }
                     }
-                    
+                    Button() {
+                        load()
+                    } label: {
+                        Text("Reload from plist")
+                    }
                     Button() {
                         apply()
                     } label: {
@@ -271,6 +275,9 @@ struct EditorView: View {
                 return false
             },
             set: { enabled in
+                if enabled {
+                    status = "Become iPadOS is a risky feature so please take note of the following:\n\n1. iOS Only apps, like WhatsApp, can lose data. It's recommended to offload these apps.\n2. The homescreen layout can get fucked if you have empty space.\n3. If you have an alphabetical password, it's very hard to get into your phone after locking.\n4. Any option with Dock under Stage Manager should NOT be modified.\n\n Only continue if you're okay with this. Else click reload from plist"
+                }
                 cacheData.mutableBytes.storeBytes(of: enabled ? 3 : 1, toByteOffset: valueOffset, as: Int.self)
                 for key in keys {
                     if enabled {
  ```

  </details>
- 🟡 `lara/views/SettingsView.swift` (modified) — +107 / -104
  <details><summary>Diff</summary>

  ```diff
  @@ -240,110 +240,113 @@ struct SettingsView: View {
                 }
                 
                 Section {
-                    HStack { Text("off_inpcb_inp_list_le_next"); Spacer(); Text(hex(UInt64(off_inpcb_inp_list_le_next))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_inpcb_inp_pcbinfo"); Spacer(); Text(hex(UInt64(off_inpcb_inp_pcbinfo))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_inpcb_inp_socket"); Spacer(); Text(hex(UInt64(off_inpcb_inp_socket))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_inpcbinfo_ipi_zone"); Spacer(); Text(hex(UInt64(off_inpcbinfo_ipi_zone))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_inpcb_inp_depend6_inp6_icmp6filt"); Spacer(); Text(hex(UInt64(off_inpcb_inp_depend6_inp6_icmp6filt))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_inpcb_inp_depend6_inp6_chksum"); Spacer(); Text(hex(UInt64(off_inpcb_inp_depend6_inp6_chksum))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_socket_so_usecount"); Spacer(); Text(hex(UInt64(off_socket_so_usecount))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_socket_so_proto"); Spacer(); Text(hex(UInt64(off_socket_so_proto))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_socket_so_background_thread"); Spacer(); Text(hex(UInt64(off_socket_so_background_thread))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_kalloc_type_view_kt_zv_zv_name"); Spacer(); Text(hex(UInt64(off_kalloc_type_view_kt_zv_zv_name))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_thread_t_tro"); Spacer(); Text(hex(UInt64(off_thread_t_tro))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_ro_tro_proc"); Spacer(); Text(hex(UInt64(off_thread_ro_tro_proc))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_ro_tro_task"); Spacer(); Text(hex(UInt64(off_thread_ro_tro_task))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_machine_upcb"); Spacer(); Text(hex(UInt64(off_thread_machine_upcb))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_machine_contextdata"); Spacer(); Text(hex(UInt64(off_thread_machine_contextdata))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_ctid"); Spacer(); Text(hex(UInt64(off_thread_ctid))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_options"); Spacer(); Text(hex(UInt64(off_thread_options))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_mutex_lck_mtx_data"); Spacer(); Text(hex(UInt64(off_thread_mutex_lck_mtx_data))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_machine_kstackptr"); Spacer(); Text(hex(UInt64(off_thread_machine_kstackptr))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_guard_exc_info_code"); Spacer(); Text(hex(UInt64(off_thread_guard_exc_info_code))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_mach_exc_info_code"); Spacer(); Text(hex(UInt64(off_thread_mach_exc_info_code))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_mach_exc_info_os_reason"); Spacer(); Text(hex(UInt64(off_thread_mach_exc_info_os_reason))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_mach_exc_info_exception_type"); Spacer(); Text(hex(UInt64(off_thread_mach_exc_info_exception_type))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_ast"); Spacer(); Text(hex(UInt64(off_thread_ast))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_task_threads_next"); Spacer(); Text(hex(UInt64(off_thread_task_threads_next))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_machine_jop_pid"); Spacer(); Text(hex(UInt64(off_thread_machine_jop_pid))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_thread_machine_rop_pid"); Spacer(); Text(hex(UInt64(off_thread_machine_rop_pid))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_proc_p_list_le_next"); Spacer(); Text(hex(UInt64(off_proc_p_list_le_next))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_proc_p_list_le_prev"); Spacer(); Text(hex(UInt64(off_proc_p_list_le_prev))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_proc_p_proc_ro"); Spacer(); Text(hex(UInt64(off_proc_p_proc_ro))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_proc_p_pid"); Spacer(); Text(hex(UInt64(off_proc_p_pid))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_proc_p_fd"); Spacer(); Text(hex(UInt64(off_proc_p_fd))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_proc_p_flag"); Spacer(); Text(hex(UInt64(off_proc_p_flag))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_proc_p_textvp"); Spacer(); Text(hex(UInt64(off_proc_p_textvp))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_proc_p_name"); Spacer(); Text(hex(UInt64(off_proc_p_name))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_proc_ro_pr_task"); Spacer(); Text(hex(UInt64(off_proc_ro_pr_task))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_proc_ro_p_ucred"); Spacer(); Text(hex(UInt64(off_proc_ro_p_ucred))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_ucred_cr_label"); Spacer(); Text(hex(UInt64(off_ucred_cr_label))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_task_itk_space"); Spacer(); Text(hex(UInt64(off_task_itk_space))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_task_threads_next"); Spacer(); Text(hex(UInt64(off_task_threads_next))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_task_task_exc_guard"); Spacer(); Text(hex(UInt64(off_task_task_exc_guard))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_task_map"); Spacer(); Text(hex(UInt64(off_task_map))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_filedesc_fd_ofiles"); Spacer(); Text(hex(UInt64(off_filedesc_fd_ofiles))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_filedesc_fd_cdir"); Spacer(); Text(hex(UInt64(off_filedesc_fd_cdir))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_fileproc_fp_glob"); Spacer(); Text(hex(UInt64(off_fileproc_fp_glob))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_fileglob_fg_data"); Spacer(); Text(hex(UInt64(off_fileglob_fg_data))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_fileglob_fg_flag"); Spacer(); Text(hex(UInt64(off_fileglob_fg_flag))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_vnode_v_ncchildren_tqh_first"); Spacer(); Text(hex(UInt64(off_vnode_v_ncchildren_tqh_first))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vnode_v_nclinks_lh_first"); Spacer(); Text(hex(UInt64(off_vnode_v_nclinks_lh_first))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vnode_v_parent"); Spacer(); Text(hex(UInt64(off_vnode_v_parent))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vnode_v_data"); Spacer(); Text(hex(UInt64(off_vnode_v_data))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vnode_v_name"); Spacer(); Text(hex(UInt64(off_vnode_v_name))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vnode_v_usecount"); Spacer(); Text(hex(UInt64(off_vnode_v_usecount))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vnode_v_iocount"); Spacer(); Text(hex(UInt64(off_vnode_v_iocount))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vnode_v_writecount"); Spacer(); Text(hex(UInt64(off_vnode_v_writecount))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vnode_v_flag"); Spacer(); Text(hex(UInt64(off_vnode_v_flag))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vnode_v_mount"); Spacer(); Text(hex(UInt64(off_vnode_v_mount))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_mount_mnt_flag"); Spacer(); Text(hex(UInt64(off_mount_mnt_flag))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_namecache_nc_vp"); Spacer(); Text(hex(UInt64(off_namecache_nc_vp))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_namecache_nc_child_tqe_next"); Spacer(); Text(hex(UInt64(off_namecache_nc_child_tqe_next))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_arm_saved_state64_lr"); Spacer(); Text(hex(UInt64(off_arm_saved_state64_lr))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_arm_saved_state64_pc"); Spacer(); Text(hex(UInt64(off_arm_saved_state64_pc))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_arm_saved_state_uss_ss_64"); Spacer(); Text(hex(UInt64(off_arm_saved_state_uss_ss_64))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_ipc_space_is_table"); Spacer(); Text(hex(UInt64(off_ipc_space_is_table))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_ipc_entry_ie_object"); Spacer(); Text(hex(UInt64(off_ipc_entry_ie_object))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_ipc_port_ip_kobject"); Spacer(); Text(hex(UInt64(off_ipc_port_ip_kobject))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_arm_kernel_saved_state_sp"); Spacer(); Text(hex(UInt64(off_arm_kernel_saved_state_sp))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_vm_map_hdr"); Spacer(); Text(hex(UInt64(off_vm_map_hdr))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vm_map_header_nentries"); Spacer(); Text(hex(UInt64(off_vm_map_header_nentries))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vm_map_entry_links_next"); Spacer(); Text(hex(UInt64(off_vm_map_entry_links_next))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vm_map_entry_vme_object_or_delta"); Spacer(); Text(hex(UInt64(off_vm_map_entry_vme_object_or_delta))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vm_map_entry_vme_alias"); Spacer(); Text(hex(UInt64(off_vm_map_entry_vme_alias))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vm_map_header_links_next"); Spacer(); Text(hex(UInt64(off_vm_map_header_links_next))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_vm_object_vo_un1_vou_size"); Spacer(); Text(hex(UInt64(off_vm_object_vo_un1_vou_size))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vm_object_ref_count"); Spacer(); Text(hex(UInt64(off_vm_object_ref_count))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_vm_named_entry_backing_copy"); Spacer(); Text(hex(UInt64(off_vm_named_entry_backing_copy))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_vm_named_entry_size"); Spacer(); Text(hex(UInt64(off_vm_named_entry_size))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("off_label_l_perpolicy_amfi"); Spacer(); Text(hex(UInt64(off_label_l_perpolicy_amfi))).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("off_label_l_perpolicy_sandbox"); Spacer(); Text(hex(UInt64(off_label_l_perpolicy_sandbox))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("sizeof_ipc_entry"); Spacer(); Text(hex(UInt64(sizeof_ipc_entry))).foregroundColor(.secondary).monospaced() }
-                    
-                    HStack { Text("smr_base"); Spacer(); Text(hex(smr_base)).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("T1SZ_BOOT"); Spacer(); TextField("0x19", text: t1szbootbind).foregroundColor(.secondary).multilineTextAlignment(.trailing).monospaced() }
-                    HStack { Text("VM_MIN_KERNEL_ADDRESS"); Spacer(); Text(hex(VM_MIN_KERNEL_ADDRESS)).foregroundColor(.secondary).monospaced() }
-                    HStack { Text("VM_MAX_KERNEL_ADDRESS"); Spacer(); Text(hex(VM_MAX_KERNEL_ADDRESS)).foregroundColor(.secondary).monospaced() }
-                    
+                    NavigationLink("Modify Offsets") {
+                        List {
+                            HStack { Text("off_inpcb_inp_list_le_next"); Spacer(); Text(hex(UInt64(off_inpcb_inp_list_le_next))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_inpcb_inp_pcbinfo"); Spacer(); Text(hex(UInt64(off_inpcb_inp_pcbinfo))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_inpcb_inp_socket"); Spacer(); Text(hex(UInt64(off_inpcb_inp_socket))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_inpcbinfo_ipi_zone"); Spacer(); Text(hex(UInt64(off_inpcbinfo_ipi_zone))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_inpcb_inp_depend6_inp6_icmp6filt"); Spacer(); Text(hex(UInt64(off_inpcb_inp_depend6_inp6_icmp6filt))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_inpcb_inp_depend6_inp6_chksum"); Spacer(); Text(hex(UInt64(off_inpcb_inp_depend6_inp6_chksum))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_socket_so_usecount"); Spacer(); Text(hex(UInt64(off_socket_so_usecount))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_socket_so_proto"); Spacer(); Text(hex(UInt64(off_socket_so_proto))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_socket_so_background_thread"); Spacer(); Text(hex(UInt64(off_socket_so_background_thread))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_kalloc_type_view_kt_zv_zv_name"); Spacer(); Text(hex(UInt64(off_kalloc_type_view_kt_zv_zv_name))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_thread_t_tro"); Spacer(); Text(hex(UInt64(off_thread_t_tro))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_ro_tro_proc"); Spacer(); Text(hex(UInt64(off_thread_ro_tro_proc))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_ro_tro_task"); Spacer(); Text(hex(UInt64(off_thread_ro_tro_task))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_machine_upcb"); Spacer(); Text(hex(UInt64(off_thread_machine_upcb))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_machine_contextdata"); Spacer(); Text(hex(UInt64(off_thread_machine_contextdata))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_ctid"); Spacer(); Text(hex(UInt64(off_thread_ctid))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_options"); Spacer(); Text(hex(UInt64(off_thread_options))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_mutex_lck_mtx_data"); Spacer(); Text(hex(UInt64(off_thread_mutex_lck_mtx_data))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_machine_kstackptr"); Spacer(); Text(hex(UInt64(off_thread_machine_kstackptr))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_guard_exc_info_code"); Spacer(); Text(hex(UInt64(off_thread_guard_exc_info_code))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_mach_exc_info_code"); Spacer(); Text(hex(UInt64(off_thread_mach_exc_info_code))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_mach_exc_info_os_reason"); Spacer(); Text(hex(UInt64(off_thread_mach_exc_info_os_reason))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_mach_exc_info_exception_type"); Spacer(); Text(hex(UInt64(off_thread_mach_exc_info_exception_type))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_ast"); Spacer(); Text(hex(UInt64(off_thread_ast))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_task_threads_next"); Spacer(); Text(hex(UInt64(off_thread_task_threads_next))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_machine_jop_pid"); Spacer(); Text(hex(UInt64(off_thread_machine_jop_pid))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_thread_machine_rop_pid"); Spacer(); Text(hex(UInt64(off_thread_machine_rop_pid))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_proc_p_list_le_next"); Spacer(); Text(hex(UInt64(off_proc_p_list_le_next))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_proc_p_list_le_prev"); Spacer(); Text(hex(UInt64(off_proc_p_list_le_prev))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_proc_p_proc_ro"); Spacer(); Text(hex(UInt64(off_proc_p_proc_ro))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_proc_p_pid"); Spacer(); Text(hex(UInt64(off_proc_p_pid))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_proc_p_fd"); Spacer(); Text(hex(UInt64(off_proc_p_fd))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_proc_p_flag"); Spacer(); Text(hex(UInt64(off_proc_p_flag))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_proc_p_textvp"); Spacer(); Text(hex(UInt64(off_proc_p_textvp))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_proc_p_name"); Spacer(); Text(hex(UInt64(off_proc_p_name))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_proc_ro_pr_task"); Spacer(); Text(hex(UInt64(off_proc_ro_pr_task))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_proc_ro_p_ucred"); Spacer(); Text(hex(UInt64(off_proc_ro_p_ucred))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_ucred_cr_label"); Spacer(); Text(hex(UInt64(off_ucred_cr_label))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_task_itk_space"); Spacer(); Text(hex(UInt64(off_task_itk_space))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_task_threads_next"); Spacer(); Text(hex(UInt64(off_task_threads_next))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_task_task_exc_guard"); Spacer(); Text(hex(UInt64(off_task_task_exc_guard))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_task_map"); Spacer(); Text(hex(UInt64(off_task_map))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_filedesc_fd_ofiles"); Spacer(); Text(hex(UInt64(off_filedesc_fd_ofiles))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_filedesc_fd_cdir"); Spacer(); Text(hex(UInt64(off_filedesc_fd_cdir))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_fileproc_fp_glob"); Spacer(); Text(hex(UInt64(off_fileproc_fp_glob))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_fileglob_fg_data"); Spacer(); Text(hex(UInt64(off_fileglob_fg_data))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_fileglob_fg_flag"); Spacer(); Text(hex(UInt64(off_fileglob_fg_flag))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_vnode_v_ncchildren_tqh_first"); Spacer(); Text(hex(UInt64(off_vnode_v_ncchildren_tqh_first))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vnode_v_nclinks_lh_first"); Spacer(); Text(hex(UInt64(off_vnode_v_nclinks_lh_first))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vnode_v_parent"); Spacer(); Text(hex(UInt64(off_vnode_v_parent))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vnode_v_data"); Spacer(); Text(hex(UInt64(off_vnode_v_data))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vnode_v_name"); Spacer(); Text(hex(UInt64(off_vnode_v_name))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vnode_v_usecount"); Spacer(); Text(hex(UInt64(off_vnode_v_usecount))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vnode_v_iocount"); Spacer(); Text(hex(UInt64(off_vnode_v_iocount))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vnode_v_writecount"); Spacer(); Text(hex(UInt64(off_vnode_v_writecount))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vnode_v_flag"); Spacer(); Text(hex(UInt64(off_vnode_v_flag))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vnode_v_mount"); Spacer(); Text(hex(UInt64(off_vnode_v_mount))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_mount_mnt_flag"); Spacer(); Text(hex(UInt64(off_mount_mnt_flag))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_namecache_nc_vp"); Spacer(); Text(hex(UInt64(off_namecache_nc_vp))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_namecache_nc_child_tqe_next"); Spacer(); Text(hex(UInt64(off_namecache_nc_child_tqe_next))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_arm_saved_state64_lr"); Spacer(); Text(hex(UInt64(off_arm_saved_state64_lr))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_arm_saved_state64_pc"); Spacer(); Text(hex(UInt64(off_arm_saved_state64_pc))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_arm_saved_state_uss_ss_64"); Spacer(); Text(hex(UInt64(off_arm_saved_state_uss_ss_64))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_ipc_space_is_table"); Spacer(); Text(hex(UInt64(off_ipc_space_is_table))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_ipc_entry_ie_object"); Spacer(); Text(hex(UInt64(off_ipc_entry_ie_object))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_ipc_port_ip_kobject"); Spacer(); Text(hex(UInt64(off_ipc_port_ip_kobject))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_arm_kernel_saved_state_sp"); Spacer(); Text(hex(UInt64(off_arm_kernel_saved_state_sp))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_vm_map_hdr"); Spacer(); Text(hex(UInt64(off_vm_map_hdr))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vm_map_header_nentries"); Spacer(); Text(hex(UInt64(off_vm_map_header_nentries))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vm_map_entry_links_next"); Spacer(); Text(hex(UInt64(off_vm_map_entry_links_next))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vm_map_entry_vme_object_or_delta"); Spacer(); Text(hex(UInt64(off_vm_map_entry_vme_object_or_delta))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vm_map_entry_vme_alias"); Spacer(); Text(hex(UInt64(off_vm_map_entry_vme_alias))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vm_map_header_links_next"); Spacer(); Text(hex(UInt64(off_vm_map_header_links_next))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_vm_object_vo_un1_vou_size"); Spacer(); Text(hex(UInt64(off_vm_object_vo_un1_vou_size))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vm_object_ref_count"); Spacer(); Text(hex(UInt64(off_vm_object_ref_count))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_vm_named_entry_backing_copy"); Spacer(); Text(hex(UInt64(off_vm_named_entry_backing_copy))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_vm_named_entry_size"); Spacer(); Text(hex(UInt64(off_vm_named_entry_size))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("off_label_l_perpolicy_amfi"); Spacer(); Text(hex(UInt64(off_label_l_perpolicy_amfi))).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("off_label_l_perpolicy_sandbox"); Spacer(); Text(hex(UInt64(off_label_l_perpolicy_sandbox))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("sizeof_ipc_entry"); Spacer(); Text(hex(UInt64(sizeof_ipc_entry))).foregroundColor(.secondary).monospaced() }
+                            
+                            HStack { Text("smr_base"); Spacer(); Text(hex(smr_base)).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("T1SZ_BOOT"); Spacer(); TextField("0x19", text: t1szbootbind).foregroundColor(.secondary).multilineTextAlignment(.trailing).monospaced() }
+                            HStack { Text("VM_MIN_KERNEL_ADDRESS"); Spacer(); Text(hex(VM_MIN_KERNEL_ADDRESS)).foregroundColor(.secondary).monospaced() }
+                            HStack { Text("VM_MAX_KERNEL_ADDRESS"); Spacer(); Text(hex(VM_MAX_KERNEL_ADDRESS)).foregroundColor(.secondary).monospaced() }
+                        }
+                    }
                     Button {
                         save()
                         statusmsg = "Offsets saved!"
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

- : 📝 Commit d9277bb: Tweaks: Text --> Labels and split into categories
- : 📝 Commit 869eb91: Move Offsets in settings into its own NavigationLink
- : 📝 Commit 806bef4: Add share button in LogsView
- : 📝 Commit cdb6f85: Add warning in EditorView.swift and fix error
- : 📝 Commit 57a606b: EditorView: Add seperate warning for TrollPad and add reload button
- : 📝 Commit 99cea6a: Improve warning message
- Apr 25, 2026: 🟢 @jurre111 marked ready for review
- Apr 25, 2026: 👀 @jurre111 requested review from @Copilot
- : 💬 someone reviewed: commented
- : 📝 Commit 9d608fc: Implement some of Copilot's suggestions
- Apr 25, 2026: ⬆️ @jurre111 force-pushed
- Apr 25, 2026: ⚪ @jurre111 converted to draft
- Apr 25, 2026: ⬆️ @jurre111 force-pushed
- Apr 25, 2026: 🟢 @jurre111 marked ready for review
- : 📝 Commit 5518a97: Merge branch 'main' into UI-improvements
- Apr 26, 2026: 🟣 @rooootdev merged
- Apr 26, 2026: 🔴 Closed by @rooootdev
- Apr 26, 2026: 🗑️ @jurre111 deleted head branch
- Apr 26, 2026: 🔗 Commit 930a0c7 — Squashed commit of the following:
- Apr 27, 2026: 🔗 Commit 07c3a6c — Squashed commit of the following:
- Apr 29, 2026: 🔗 Commit e0da526 — Squashed commit of the following:
- Apr 29, 2026: 🔗 Commit 9fc6ffb — Squashed commit of the following:


---

*Exported on 10/14/2026, 3:01:56 PM from [https://github.com/rooootdev/lara/pull/187](https://github.com/rooootdev/lara/pull/187)*
