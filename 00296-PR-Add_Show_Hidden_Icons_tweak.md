# Add Show Hidden Icons tweak

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#296](https://github.com/rooootdev/lara/pull/296)  
> **State:** 🟣 merged  
> **Branch:** `hxhlb:show-hidden-icons` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Jun 21, 2026  
> **Changes:** +211 / -0 across 2 files  
> **Created:** Jun 17, 2026  
> **Updated:** Jun 21, 2026  
> **Closed:** Jun 21, 2026  

---

## Description

*Opened by [@hxhlb](https://github.com/hxhlb)* *on Jun 17, 2026*


## Summary

Adds a new **Show Hidden Icons** tweak inspired by Nugget's "Show Hidden Icons on Home Screen" option.

This tweak:
- writes `SBIconVisibility` to GlobalPreferences
- exposes a separate page under User Interface
- includes a "Remove Preference" action that deletes the key instead of writing `false`
- includes an optional "Rebuild Application State DB" action matching Nugget's rebuild option

## Safety Notes

Rebuilding SpringBoard's Application State DB can have side effects. The UI explicitly warns that:
- the device should be rebooted after rebuilding
- respring may leave SpringBoard on a black screen
- some app/widget state, including widget configuration, may be lost

## Testing

Tested locally on device:
- enabling `SBIconVisibility`
- rebuilding Application State DB
- rebooting to allow SpringBoard to rebuild its state


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/27662939366/job/81810959732) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/27662939366/job/81810834755) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`fb0fc1f`](https://github.com/rooootdev/lara/commit/fb0fc1ffb46453834174c6b27f90195ccf1b4533) Add Show Hidden Icons tweak — *Hades* · Jun 17, 2026


---

## Changed Files (2)

- 🟢 `lara/views/tweaks/ShowHiddenIconsView.swift` (added) — +209 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,209 @@
+//
+//  ShowHiddenIconsView.swift
+//  lara
+//
+
+import SwiftUI
+
+struct ShowHiddenIconsView: View {
+    @ObservedObject var mgr: laramgr
+
+    private let key = "SBIconVisibility"
+    private let path = fileloc.globalprefs.rawValue
+
+    @State private var isEnabled = false
+    @State private var isLoading = false
+    @State private var status: String?
+    @State private var confirmRebuildDB = false
+
+    var body: some View {
+        List {
+            Section(
+                header: HeaderLabel(text: "Home Screen", icon: "app.badge"),
+                footer: Text("Inspired by Nugget's \"Show Hidden Icons on Home Screen\" tweak. Writes SBIconVisibility to GlobalPreferences. This may not take effect until SpringBoard's Application State DB is rebuilt.")
+            ) {
+                Toggle("Show Hidden Icons", isOn: Binding(
+                    get: { isEnabled },
+                    set: { setEnabled($0) }
+                ))
+                .disabled(isLoading || !canWrite)
+
+                HStack {
+                    Text("Preference")
+                    Spacer()
+                    Text(isEnabled ? "Enabled" : "Disabled")
+                        .foregroundColor(isEnabled ? .green : .secondary)
+                        .monospaced()
+                }
+            }
+
+            Section(
+                header: HeaderLabel(text: "Actions", icon: "wrench.and.screwdriver"),
+                footer: Text("Refresh only reloads the current SBIconVisibility value from GlobalPreferences. It does not rebuild SpringBoard caches.")
+            ) {
+                Button {
+                    loadState()
+                } label: {
+                    if isLoading {
+                        HStack {
+                            Text("Refreshing...")
+                            Spacer()
+                            ProgressView()
+                        }
+                    } else {
+                        Text("Refresh Preference State")
+                    }
+                }
+                .disabled(isLoading || !canWrite)
+
+                HStack {
+                    Button(role: .destructive) {
+                        reset()
+                    } label: {
+                        Text("Remove Preference")
+                    }
+                    .disabled(isLoading || !canWrite)
+
+                    Spacer()
+
+                    Button {
+                        Alertinator.shared.alert(
+                            title: "Remove Preference",
+                            body: "Deletes the SBIconVisibility key from GlobalPreferences instead of writing false. This restores the default preference value. If SpringBoard has already cached the old state, you may still need to rebuild the Application State DB and reboot."
+                        )
+                    } label: {
+                        Image(systemName: "info.circle")
+                            .frame(width: 24, height: 22)
+                    }
+                    .buttonStyle(.plain)
+                }
+
+                HStack {
+                    Button(role: .destructive) {
+                        confirmRebuildDB = true
+                    } label: {
+                        Text("Rebuild Application State DB")
+                    }
+                    .disabled(isLoading || !canWrite)
+
+                    Spacer()
+
+                    Button {
+                        Alertinator.shared.alert(
+                            title: "Rebuild Application State DB",
+                            body: "Clears SpringBoard's applicationState.db, applicationState.db-wal, and applicationState.db-shm so SpringBoard rebuilds them. Reboot after applying. Respring may leave SpringBoard on a black screen, and some widget configuration may be lost."
+                        )
+                    } label: {
+                        Image(systemName: "info.circle")
+                            .frame(width: 24, height: 22)
+                    }
+                    .buttonStyle(.plain)
+                }
+            }
+        }
+        .navigationTitle("Show Hidden Icons")
+        .onAppear {
+            loadState()
+        }
+        .alert("Show Hidden Icons", isPresented: .constant(status != nil)) {
+            Button("OK") { status = nil }
+        } message: {
+            Text(status ?? "")
+        }
+        .alert("Rebuild Application State DB?", isPresented: $confirmRebuildDB) {
+            Button("Cancel", role: .cancel) {}
+            Button("Rebuild", role: .destructive) {
+                rebuildApplicationStateDB()
+            }
+        } message: {
+            Text("This matches Nugget's \"Rebuild SpringBoard Application State DB\" option. It replaces SpringBoard's applicationState.db with an empty file so SpringBoard can rebuild it. You should reboot the device after applying. Respring may leave SpringBoard on a black screen. Rebuilding this DB can also reset some app/widget state, including widget configuration. Continue only if you accept that risk.")
+        }
+    }
+
+    private var canWrite: Bool {
+        mgr.sbxready || mgr.vfsready
+    }
+
+    private func loadState() {
+        guard canWrite else {
+            status = "Sandbox escape or VFS is not ready."
+            return
+        }
+
+        isLoading = true
+        defer { isLoading = false }
+
+        let result = mgr.getplistvalue(path: path, key: key)
+        if result.ok, let value = result.value as? Bool {
+            isEnabled = value
+        } else {
+            isEnabled = false
+        }
+    }
+
+    private func setEnabled(_ enabled: Bool) {
+        guard canWrite else {
+            status = "Sandbox escape or VFS is not ready."
+            return
+        }
+
+        isLoading = true
+        defer { isLoading = false }
+
+        let result = mgr.setplistvalue(
+            path: path,
+            key: (key, enabled ? true : nil),
+            force: true
+        )
+
+        if result.ok {
+            isEnabled = enabled
+            status = enabled
+                ? "SBIconVisibility enabled. If there is no visible effect, you may need to rebuild SpringBoard's Application State DB, then reboot the device."
+                : "SBIconVisibility preference removed. If there is no visible effect, rebuild SpringBoard's Application State DB, then reboot the device."
+        } else {
+            status = result.message
+            loadState()
+        }
+    }
+
+    private func reset() {
+        setEnabled(false)
+    }
+
+    private func rebuildApplicationStateDB() {
+        guard canWrite else {
+            status = "Sandbox escape or VFS is not ready."
+            return
+        }
+
+        isLoading = true
+        defer { isLoading = false }
+
+        let dbPaths = [
+            "/var/mobile/Library/FrontBoard/applicationState.db",
+            "/var/mobile/Library/FrontBoard/applicationState.db-wal",
+            "/var/mobile/Library/FrontBoard/applicationState.db-shm",
+        ]
+
+        var failures: [String] = []
+        for dbPath in dbPaths {
+            let result = mgr.lara_overwritefile(target: dbPath, data: Data())
+            if !result.ok {
+                failures.append("\(dbPath): \(result.message)")
+            }
+        }
+
+        if failures.isEmpty {
+            status = "Application State DB cleared. Reboot the device now. Do not rely on respring; it may leave SpringBoard on a black screen. Some widget configuration may be lost."
+        } else {
+            status = failures.joined(separator: "\n")
+        }
+    }
+}
+
+#Preview {
+    NavigationStack {
+        ShowHiddenIconsView(mgr: laramgr.shared)
+    }
+}
  ```

  </details>
- 🟡 `lara/views/tweaks/TweaksView.swift` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -44,6 +44,8 @@ struct TweaksView: View {
                 Section(header: HeaderLabel(text: "User Interface", icon: "eye")) {
                     NavigationLink("dirtyZero", destination: dirtyZeroView())
                         .disabled(!mgr.vfsready)
+                    NavigationLink("Show Hidden Icons", destination: ShowHiddenIconsView(mgr: mgr))
+                        .disabled(!mgr.sbxready && !mgr.vfsready)
                     NavigationLink("MobileGestalt", destination: GestaltView(mgr: mgr))
                         .disabled(!mgr.sbxready)
                     NavigationLink("Font Overwrite", destination: FontPicker(mgr: mgr))
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit fb0fc1f: Add Show Hidden Icons tweak
- Jun 21, 2026: 🟣 @rooootdev merged
- Jun 21, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:11:21 PM from [https://github.com/rooootdev/lara/pull/296](https://github.com/rooootdev/lara/pull/296)*
