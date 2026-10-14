# Add OTA Disabler

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#248](https://github.com/rooootdev/lara/pull/248)  
> **State:** 🔴 closed  
> **Branch:** `hxhlb:codex/ota-disabler` → `rooootdev:main`  
> **Changes:** +604 / -0 across 4 files  
> **Created:** May 14, 2026  
> **Updated:** May 20, 2026  
> **Closed:** May 15, 2026  

---

## Description

*Opened by [@hxhlb](https://github.com/hxhlb)* *on May 14, 2026*


## Summary

- Adds an OTA Disabler page under Tweaks > System.
- Uses a temporary launchd RemoteCall to update /var/db/com.apple.xpc.launchd/disabled.plist.
- Supports disable, enable, local backup, restore, operation logs, and a launchd RemoteCall countdown.
- Warns when an existing SpringBoard RemoteCall session must be destroyed before switching to launchd.
- Includes reference links to kolbicz/DarkSword-Tweaks and zeroxjf/kfun-zeroxjf.

## Notes

- A full device reboot is required after changing OTA daemon state; a respring is not enough.
- This was manually tested on limited devices only, so I am sending this as a separate PR for easier review.

## Related

- Related to #221.

## Validation

- Built with xcodebuild using CODE_SIGNING_ALLOWED=NO.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (4)

- 🔴 [build](https://github.com/rooootdev/lara/actions/runs/25847308956/job/75945387164) — **failure** — *GitHub Actions*
- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25847477701/job/75946067395) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25847477701/job/75945919026) — **success** — *GitHub Actions*
- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25847308956/job/75945456690) — **skipped** — *GitHub Actions*


---

## PR Commits (1)

- [`0bd4feb`](https://github.com/rooootdev/lara/commit/0bd4feb153ac04b7b5bb0cdeedc403e1dc241246) Add OTA Disabler — *Hades* · May 14, 2026


---

## Changed Files (4)

- 🟡 `lara/kexploit/pe/rc.h` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -31,5 +31,7 @@ int euenabler_overwrite_eligibility(RemoteCall *proc);
 void euenabler_override_country_code(RemoteCall *proc, rc_euenabler_callback_t callback);
 bool patch_homescreen_grid(RemoteCall *proc, int cols, int rows);
 void youtube_tweak(RemoteCall *proc);
+int ota_set_updates_disabled(RemoteCall *proc, bool disabled);
+int ota_write_disabled_plist(RemoteCall *proc, NSData *data);
 
 #endif /* rc_h */
  ```

  </details>
- 🟡 `lara/kexploit/pe/rc.m` (modified) — +136 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -12,6 +12,7 @@
 #import <pthread.h>
 #import <stdint.h>
 #include <errno.h>
+#include <fcntl.h>
 #include <string.h>
 #include <unistd.h>
 #import <sys/mman.h>
@@ -960,3 +961,138 @@ void youtube_tweak(RemoteCall *proc) {
     
     printf("Done\n");
 }
+
+int ota_set_updates_disabled(RemoteCall *proc, bool disabled) {
+    if (!proc) return EINVAL;
+
+    const char *plistPath = "/var/db/com.apple.xpc.launchd/disabled.plist";
+    uint64_t fileBuf = RemoteArbCallWithTimeout(1000, proc, mmap, 0, 65536, VM_PROT_READ | VM_PROT_WRITE, MAP_PRIVATE | MAP_ANON, (uint64_t)-1, 0);
+    if (!fileBuf || fileBuf == UINT64_MAX) {
+        return ENOMEM;
+    }
+
+    [proc remote_write:proc.trojanMem string:plistPath];
+
+    NSMutableDictionary *plist = [NSMutableDictionary dictionary];
+    uint64_t fd = RemoteArbCallWithTimeout(1000, proc, open, proc.trojanMem, O_RDONLY, 0);
+    if ((int64_t)fd >= 0) {
+        uint64_t bytesRead = RemoteArbCallWithTimeout(1000, proc, read, fd, fileBuf, 65536);
+        RemoteArbCallWithTimeout(1000, proc, close, fd);
+
+        if ((int64_t)bytesRead > 0) {
+            void *buf = malloc((size_t)bytesRead);
+            if (!buf) {
+                RemoteArbCallWithTimeout(1000, proc, munmap, fileBuf, 65536);
+                return ENOMEM;
+            }
+
+            [proc remoteRead:fileBuf to:buf size:bytesRead];
+            NSData *data = [NSData dataWithBytes:buf length:(NSUInteger)bytesRead];
+            free(buf);
+
+            NSMutableDictionary *existing = [[NSPropertyListSerialization propertyListWithData:data options:NSPropertyListMutableContainersAndLeaves format:nil error:nil] mutableCopy];
+            if (existing) {
+                plist = existing;
+            }
+        }
+    }
+
+    NSArray *services = @[
+        @"com.apple.mobile.softwareupdated",
+        @"com.apple.OTATaskingAgent",
+        @"com.apple.softwareupdateservicesd",
+        @"com.apple.mobile.NRDUpdated",
+    ];
+
+    bool changed = false;
+    for (NSString *service in services) {
+        if (disabled) {
+            if (![plist[service] isEqual:@YES]) {
+                plist[service] = @YES;
+                changed = true;
+            }
+        } else if (plist[service]) {
+            [plist removeObjectForKey:service];
+            changed = true;
+        }
+    }
+
+    if (!changed) {
+        RemoteArbCallWithTimeout(1000, proc, munmap, fileBuf, 65536);
+        return 0;
+    }
+
+    NSError *error = nil;
+    NSData *outData = [NSPropertyListSerialization dataWithPropertyList:plist format:NSPropertyListXMLFormat_v1_0 options:0 error:&error];
+    if (!outData || outData.length > 65536) {
+        RemoteArbCallWithTimeout(1000, proc, munmap, fileBuf, 65536);
+        return EOVERFLOW;
+    }
+
+    [proc remote_write:fileBuf from:outData.bytes size:outData.length];
+    [proc remote_write:proc.trojanMem string:plistPath];
+
+    uint64_t wfd = RemoteArbCallWithTimeout(1000, proc, open, proc.trojanMem, O_WRONLY | O_CREAT | O_TRUNC, 0644);
+    if ((int64_t)wfd < 0) {
+        int err = remote_errno(proc);
+        RemoteArbCallWithTimeout(1000, proc, munmap, fileBuf, 65536);
+        return err > 0 ? err : EPERM;
+    }
+
+    uint64_t totalWritten = 0;
+    uint64_t remaining = outData.length;
+    while (remaining > 0) {
+        uint64_t written = RemoteArbCallWithTimeout(1000, proc, write, wfd, fileBuf + totalWritten, remaining);
+        if ((int64_t)written <= 0) {
+            int err = remote_errno(proc);
+            RemoteArbCallWithTimeout(1000, proc, close, wfd);
+            RemoteArbCallWithTimeout(1000, proc, munmap, fileBuf, 65536);
+            return err > 0 ? err : EIO;
+        }
+        totalWritten += written;
+        remaining -= written;
+    }
+
+    RemoteArbCallWithTimeout(1000, proc, close, wfd);
+    RemoteArbCallWithTimeout(1000, proc, munmap, fileBuf, 65536);
+    return 0;
+}
+
+int ota_write_disabled_plist(RemoteCall *proc, NSData *data) {
+    if (!proc || !data) return EINVAL;
+    if (data.length > 65536) return EOVERFLOW;
+
+    const char *plistPath = "/var/db/com.apple.xpc.launchd/disabled.plist";
+    uint64_t fileBuf = RemoteArbCallWithTimeout(1000, proc, mmap, 0, 65536, VM_PROT_READ | VM_PROT_WRITE, MAP_PRIVATE | MAP_ANON, (uint64_t)-1, 0);
+    if (!fileBuf || fileBuf == UINT64_MAX) {
+        return ENOMEM;
+    }
+
+    [proc remote_write:fileBuf from:data.bytes size:data.length];
+    [proc remote_write:proc.trojanMem string:plistPath];
+
+    uint64_t wfd = RemoteArbCallWithTimeout(1000, proc, open, proc.trojanMem, O_WRONLY | O_CREAT | O_TRUNC, 0644);
+    if ((int64_t)wfd < 0) {
+        int err = remote_errno(proc);
+        RemoteArbCallWithTimeout(1000, proc, munmap, fileBuf, 65536);
+        return err > 0 ? err : EPERM;
+    }
+
+    uint64_t totalWritten = 0;
+    uint64_t remaining = data.length;
+    while (remaining > 0) {
+        uint64_t written = RemoteArbCallWithTimeout(1000, proc, write, wfd, fileBuf + totalWritten, remaining);
+        if ((int64_t)written <= 0) {
+            int err = remote_errno(proc);
+            RemoteArbCallWithTimeout(1000, proc, close, wfd);
+            RemoteArbCallWithTimeout(1000, proc, munmap, fileBuf, 65536);
+            return err > 0 ? err : EIO;
+        }
+        totalWritten += written;
+        remaining -= written;
+    }
+
+    RemoteArbCallWithTimeout(1000, proc, close, wfd);
+    RemoteArbCallWithTimeout(1000, proc, munmap, fileBuf, 65536);
+    return 0;
+}
  ```

  </details>
- 🟡 `lara/views/tweaks/TweaksView.swift` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -53,6 +53,8 @@ struct TweaksView: View {
                 Section(header: HeaderLabel(text: "System", icon: "gear")) {
                     NavigationLink("VarClean", destination: VarCleanView())
                         .disabled(!mgr.sbxready)
+                    NavigationLink("OTA Disabler", destination: OTAView())
+                        .disabled(!mgr.dsready)
                     NavigationLink("Custom Overwrite", destination: CustomView(mgr: mgr))
                         .disabled(!mgr.vfsready)
                 }
  ```

  </details>
- 🟢 `lara/views/tweaks/system/OTAView.swift` (added) — +464 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,464 @@
+//
+//  OTAView.swift
+//  lara
+//
+//  Created by hxhlb on 5/14/26.
+//
+
+import SwiftUI
+import Darwin
+import Combine
+
+struct OTAView: View {
+    @ObservedObject private var mgr = laramgr.shared
+
+    private enum OTAAction: String, Identifiable {
+        case disable
+        case enable
+        case restoreBackup
+
+        var id: String { rawValue }
+
+        var title: String {
+            switch self {
+            case .disable: return "Disable OTA Updates"
+            case .enable: return "Enable OTA Updates"
+            case .restoreBackup: return "Restore OTA Backup"
+            }
+        }
+
+        var message: String {
+            switch self {
+            case .disable:
+                return "This will disable OTA update daemons by writing them to launchd's disabled.plist. A full device reboot is required."
+            case .enable:
+                return "This will remove OTA update daemons from launchd's disabled.plist. A full device reboot is required."
+            case .restoreBackup:
+                return "This will overwrite the system disabled.plist with Lara's local backup. A full device reboot is required."
+            }
+        }
+    }
+
+    private enum OTAState {
+        case unknown
+        case enabled
+        case disabled
+        case mixed
+
+        var title: String {
+            switch self {
+            case .unknown: return "Unknown"
+            case .enabled: return "Enabled"
+            case .disabled: return "Disabled"
+            case .mixed: return "Partially Disabled"
+            }
+        }
+    }
+
+    private let plistPath = "/var/db/com.apple.xpc.launchd/disabled.plist"
+    private let darkSwordReferenceURL = URL(string: "https://github.com/kolbicz/DarkSword-Tweaks")!
+    private let kfunReferenceURL = URL(string: "https://github.com/zeroxjf/kfun-zeroxjf")!
+    private let backupURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
+        .appendingPathComponent("disabled.plist.backup")
+    private let otaServices = [
+        "com.apple.mobile.softwareupdated",
+        "com.apple.OTATaskingAgent",
+        "com.apple.softwareupdateservicesd",
+        "com.apple.mobile.NRDUpdated",
+    ]
+
+    @State private var state: OTAState = .unknown
+    @State private var serviceStates: [String: Bool] = [:]
+    @State private var status: String?
+    @State private var working = false
+    @State private var pendingAction: OTAAction?
+    @State private var hasBackup = false
+    @State private var showOperationSheet = false
+    @State private var operationTitle = "OTA Operation"
+    @State private var operationLogs: [String] = []
+    @State private var launchdRCDeadline: Date?
+    @State private var launchdRCRemaining = 0
+    @State private var launchdRCDidTimeout = false
+
+    private let launchdRCTimeoutSeconds = 120
+
+    var body: some View {
+        NavigationStack {
+            List {
+                Section {
+                    HStack {
+                        Text("Current State")
+                        Spacer()
+                        Text(state.title)
+                            .foregroundStyle(state == .disabled ? .orange : .secondary)
+                    }
+
+                    Button("Refresh Status") {
+                        refresh()
+                    }
+                    .disabled(!mgr.sbxready || working)
+                } footer: {
+                    Text("Reads \(plistPath). Missing entries mean OTA updates are not disabled by this toggle.")
+                }
+
+                Section {
+                    Button("Backup disabled.plist") {
+                        backup()
+                    }
+                    .disabled(!mgr.sbxready || working)
+
+                    Button("Restore Backup") {
+                        pendingAction = .restoreBackup
+                    }
+                    .disabled(!mgr.dsready || working || !hasBackup)
+
+                    HStack {
+                        Text("Backup")
+                        Spacer()
+                        Text(hasBackup ? "Available" : "Missing")
+                            .foregroundStyle(hasBackup ? Color.secondary : Color.orange)
+                    }
+                } header: {
+                    HeaderLabel(text: "Backup", icon: "externaldrive")
+                } footer: {
+                    VStack(alignment: .leading, spacing: 6) {
+                        if !mgr.sbxready {
+                            Text("Sandbox is not ready. Backup is unavailable until sandbox escape is initialized.")
+                        }
+                        Text("Backup path: \(backupURL.path)")
+                    }
+                }
+
+                Section {
+                    Button(role: .destructive) {
+                        pendingAction = .disable
+                    } label: {
+                        Text("Disable OTA Updates")
+                    }
+                    .disabled(!mgr.dsready || working)
+
+                    Button {
+                        pendingAction = .enable
+                    } label: {
+                        Text("Enable OTA Updates")
+                    }
+                    .disabled(!mgr.dsready || working)
+                } header: {
+                    HeaderLabel(text: "Actions", icon: "antenna.radiowaves.left.and.right")
+                } footer: {
+                    VStack(alignment: .leading, spacing: 6) {
+                        Text("A full device reboot is required after changing this setting. A respring is not enough.")
+                        Link("Reference: kolbicz/DarkSword-Tweaks", destination: darkSwordReferenceURL)
+                        Link("Reference: zeroxjf/kfun-zeroxjf", destination: kfunReferenceURL)
+                    }
+                }
+
+                Section {
+                    ForEach(otaServices, id: \.self) { service in
+                        HStack {
+                            Text(service)
+                                .font(.system(size: 13, design: .monospaced))
+                            Spacer()
+                            Text(serviceStates[service] == true ? "Disabled" : "Not Disabled")
+                                .foregroundStyle(serviceStates[service] == true ? .orange : .secondary)
+                        }
+                    }
+                } header: {
+                    Text("Launchd Services")
+                }
+
+                if working {
+                    Section {
+                        HStack {
+                            ProgressView()
+                            Text("Working...")
+                        }
+                    }
+                }
+            }
+            .navigationTitle("OTA Disabler")
+            .onAppear {
+                refresh()
+                refreshBackupState()
+            }
+            .alert("Status", isPresented: .constant(status != nil)) {
+                Button("OK") { status = nil }
+            } message: {
+                Text(status ?? "")
+            }
+            .alert(item: $pendingAction) { action in
+                Alert(
+                    title: Text(action.title),
+                    message: Text(confirmMessage(for: action)),
+                    primaryButton: .destructive(Text(action.title)) {
+                        apply(action)
+                    },
+                    secondaryButton: .cancel()
+                )
+            }
+            .sheet(isPresented: $showOperationSheet) {
+                NavigationStack {
+                    List {
+                        Section {
+                            HStack {
+                                if working {
+                                    ProgressView()
+                                } else {
+                                    Image(systemName: "checkmark.circle")
+                                        .foregroundStyle(.green)
+                                }
+                                Text(working ? "Working..." : "Finished")
+                            }
+
+                            if launchdRCDeadline != nil && working {
+                                HStack {
+                                    Text("launchd RC")
+                                    Spacer()
+                                    Text(launchdRCDidTimeout ? "Still waiting" : "\(launchdRCRemaining)s remaining")
+                                        .foregroundStyle(launchdRCDidTimeout ? Color.orange : Color.secondary)
+                                }
+                                Text(launchdRCDidTimeout ? "RemoteCall creation exceeded the expected 120s window and may be stuck." : "RemoteCall creation can take up to 120 seconds.")
+                                    .font(.footnote)
+                                    .foregroundStyle(.secondary)
+                            }
+                        }
+
+                        Section {
+                            ScrollView {
+                                Text(operationLogs.joined(separator: "\n"))
+                                    .font(.system(size: 13, design: .monospaced))
+                                    .textSelection(.enabled)
+                                    .frame(maxWidth: .infinity, alignment: .leading)
+                            }
+                            .frame(minHeight: 260)
+                        } header: {
+                            Text("Logs")
+                        }
+                    }
+                    .navigationTitle(operationTitle)
+                    .toolbar {
+                        ToolbarItem(placement: .confirmationAction) {
+                            Button("Done") {
+                                showOperationSheet = false
+                            }
+                            .disabled(working)
+                        }
+                    }
+                }
+                .presentationDetents([.medium, .large])
+                .onReceive(Timer.publish(every: 1, on: .main, in: .common).autoconnect()) { now in
+                    updateLaunchdRCCountdown(now: now)
+                }
+            }
+        }
+    }
+
+    private func confirmMessage(for action: OTAAction) -> String {
+        var message = action.message
+        if mgr.sbProc != nil || mgr.rcready {
+            message += "\n\nA SpringBoard RemoteCall session is currently active. Continuing will temporarily destroy the SpringBoard RemoteCall and switch to launchd. You can re-initialize SpringBoard RemoteCall after this operation."
+        }
+        return message
+    }
+
+    private func refresh() {
+        guard mgr.sbxready else {
+            state = .unknown
+            serviceStates = [:]
+            refreshBackupState()
+            return
+        }
+
+        let plist = readDisabledPlist()
+        serviceStates = Dictionary(uniqueKeysWithValues: otaServices.map { service in
+            (service, plist[service] as? Bool == true)
+        })
+
+        let disabledCount = serviceStates.values.filter { $0 }.count
+        if disabledCount == otaServices.count {
+            state = .disabled
+        } else if disabledCount == 0 {
+            state = .enabled
+        } else {
+            state = .mixed
+        }
+        refreshBackupState()
+    }
+
+    private func apply(_ action: OTAAction) {
+        guard mgr.dsready else {
+            status = "Darksword is not ready."
+            return
+        }
+
+        operationTitle = action.title
+        operationLogs = []
+        showOperationSheet = true
+        otaLog("Started \(action.title)")
+        clearLaunchdRCCountdown()
+        working = true
+
+        DispatchQueue.global(qos: .userInitiated).async {
+            let result: Int32
+            #if DISABLE_REMOTECALL
+            otaLog("RemoteCall is disabled in this build")
+            result = ENOTSUP
+            #else
+            DispatchQueue.main.async {
+                otaLog("Switching RemoteCall target to launchd")
+            }
+
+            if let sbProc = mgr.sbProc {
+                DispatchQueue.main.async {
+                    otaLog("Destroying existing SpringBoard RemoteCall")
+                }
+                sbProc.destroy()
+                DispatchQueue.main.async {
+                    mgr.sbProc = nil
+                    mgr.rcready = false
+                }
+            }
+
+            DispatchQueue.main.async {
+                otaLog("Creating RemoteCall(process: launchd)")
+                startLaunchdRCCountdown()
+            }
+
+            guard let proc = RemoteCall(process: "launchd", useMigFilterBypass: false) else {
+                let error = RemoteCall.lastInitError() ?? "unknown error"
+                DispatchQueue.main.async {
+                    clearLaunchdRCCountdown()
+                    working = false
+                    status = "launchd RemoteCall failed: \(error)"
+                    otaLog("launchd RemoteCall failed: \(error)")
+                }
+                return
+            }
+
+            DispatchQueue.main.async {
+                clearLaunchdRCCountdown()
+                otaLog("launchd RemoteCall initialized")
+            }
+
+            if action == .restoreBackup {
+                do {
+                    let data = try Data(contentsOf: backupURL)
+                    DispatchQueue.main.async {
+                        otaLog("Loaded backup: \(data.count) bytes")
+                        otaLog("Restoring disabled.plist from backup")
+                    }
+                    result = ota_write_disabled_plist(proc, data)
+                } catch {
+                    DispatchQueue.main.async {
+                        working = false
+                        status = "Restore failed: \(error.localizedDescription)"
+                        otaLog("Restore failed: \(error.localizedDescription)")
+                    }
+                    return
+                }
+            } else {
+                DispatchQueue.main.async {
+                    otaLog("Writing launchd disabled.plist")
+                }
+                result = ota_set_updates_disabled(proc, action == .disable)
+            }
+
+            DispatchQueue.main.async {
+                otaLog("Destroying launchd RemoteCall")
+            }
+            proc.destroy()
+            #endif
+
+            DispatchQueue.main.async {
+                working = false
+                if result == 0 {
+                    refresh()
+                    status = "\(action.title) completed through launchd. Fully reboot the device to apply."
+                    otaLog("\(action.title) completed")
+                    otaLog("Fully reboot the device to apply. A respring is not enough.")
+                } else {
+                    status = "\(action.title) failed: \(String(cString: strerror(result)))"
+                    otaLog("\(action.title) failed: \(String(cString: strerror(result)))")
+                }
+            }
+        }
+    }
+
+    private func backup() {
+        guard mgr.sbxready else {
+            status = "sandbox escape not ready"
+            return
+        }
+
+        working = true
+        defer { working = false }
+
+        do {
+            let data = try Data(contentsOf: URL(fileURLWithPath: plistPath), options: .mappedIfSafe)
+            try data.write(to: backupURL, options: .atomic)
+            refreshBackupState()
+            status = "Backup saved to \(backupURL.lastPathComponent)."
+        } catch {
+            status = "Backup failed: \(error.localizedDescription)"
+        }
+    }
+
+    private func readDisabledPlist() -> [String: Any] {
+        if !fileExists(plistPath) {
+            return [:]
+        }
+
+        do {
+            let data = try Data(contentsOf: URL(fileURLWithPath: plistPath), options: .mappedIfSafe)
+            return (try PropertyListSerialization.propertyList(
+                from: data,
+                options: [.mutableContainersAndLeaves],
+                format: nil
+            ) as? [String: Any]) ?? [:]
+        } catch {
+            mgr.logmsg("[ota] failed to read disabled.plist: \(error.localizedDescription)")
+            return [:]
+        }
+    }
+
+    private func fileExists(_ path: String) -> Bool {
+        var statbuf = stat()
+        return stat(path, &statbuf) == 0
+    }
+
+    private func refreshBackupState() {
+        hasBackup = FileManager.default.fileExists(atPath: backupURL.path)
+    }
+
+    private func otaLog(_ message: String) {
+        operationLogs.append(message)
+        mgr.logmsg("[ota] \(message)")
+    }
+
+    private func startLaunchdRCCountdown() {
+        launchdRCDidTimeout = false
+        launchdRCRemaining = launchdRCTimeoutSeconds
+        launchdRCDeadline = Date().addingTimeInterval(TimeInterval(launchdRCTimeoutSeconds))
+        otaLog("launchd RemoteCall may take up to \(launchdRCTimeoutSeconds)s")
+    }
+
+    private func clearLaunchdRCCountdown() {
+        launchdRCDeadline = nil
+        launchdRCRemaining = 0
+        launchdRCDidTimeout = false
+    }
+
+    private func updateLaunchdRCCountdown(now: Date) {
+        guard let deadline = launchdRCDeadline else { return }
+        let remaining = Int(ceil(deadline.timeIntervalSince(now)))
+        if remaining > 0 {
+            launchdRCRemaining = remaining
+        } else {
+            launchdRCRemaining = 0
+            if !launchdRCDidTimeout {
+                launchdRCDidTimeout = true
+                otaLog("launchd RemoteCall exceeded \(launchdRCTimeoutSeconds)s and may be stuck")
+            }
+        }
+    }
+}
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 0bd4feb: Add OTA Disabler
- May 14, 2026: ⚪ @hxhlb converted to draft
- May 14, 2026: 🟢 @hxhlb marked ready for review
- May 15, 2026: 🔴 Closed by @lunginspector


---

## Comments (6)

### Comment #1 — [@hxhlb](https://github.com/hxhlb) · May 14, 2026

`ID: 4448705361`


More testing is needed!


---

### Comment #2 — [@hxhlb](https://github.com/hxhlb) · May 14, 2026

`ID: 4451660256`


OK， test two device。


---

### Comment #3 — [@lunginspector](https://github.com/lunginspector) · May 15, 2026

`ID: 4455085492`


sorry, but we're not accepting prs as we're rewriting the app currently. but with this pr in particular, i would like to say that it's really confusing to see this use remotecall to write to what is a basic plist. doing all of what i see you doing makes no sense to me and seems like a really over-engineered solution.


---

### Comment #4 — [@hxhlb](https://github.com/hxhlb) · May 15, 2026

`ID: 4456164139`


> sorry, but we're not accepting prs as we're rewriting the app currently. but with this pr in particular, i would like to say that it's really confusing to see this use remotecall to write to what is a basic plist. doing all of what i see you doing makes no sense to me and seems like a really over-engineered solution.

Thanks for the feedback.

The RemoteCall part is not because the plist format itself is complex. The reason is the write target: /var/db/com.apple.xpc.launchd/disabled.plist is not reliably writable from Lara's normal sandbox/vfs context.

In my testing, reading the file worked, but writing through the existing vfs/sandbox path failed with EPERM:

sbx open failed errno=13 Permission denied
vfs overwrite failed

That is why the implementation switches to a temporary launchd RemoteCall: the plist update itself is still just a normal property-list edit, but the final open/write/truncate needs to happen from launchd's permission context.

I agree the UI/implementation can be simplified, and given the rewrite I understand not merging it right now. I just wanted to clarify that RemoteCall was used for the write permission boundary, not because the plist data needed over-engineering.


---

### Comment #5 — [@henryyuyhy](https://github.com/henryyuyhy) · May 16, 2026

`ID: 4466575749`


Where is the page? i can't find it
or does it requires remotecall?


---

### Comment #6 — [@hxhlb](https://github.com/hxhlb) · May 20, 2026

`ID: 4493656183`


> Where is the page? i can't find it or does it requires remotecall?

The PR was not accepted, so the feature is not available right now.  
I’ll submit it again once the rewrite is finished.


---

*Exported on 10/14/2026, 3:07:09 PM from [https://github.com/rooootdev/lara/pull/248](https://github.com/rooootdev/lara/pull/248)*
