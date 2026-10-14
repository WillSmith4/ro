# iPhone 7 Plus + iOS 15.8.4 | Crash on open

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#10](https://github.com/rooootdev/lara/issues/10)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Mar 28, 2026  
> **Updated:** Mar 28, 2026  
> **Closed:** Mar 28, 2026  

---

## Description

*Opened by [@thearctiic](https://github.com/thearctiic)* *on Mar 28, 2026*


Aware the project starts at iOS 17, so went digging through the code to replace newer API with iOS 15 API:

EditorView.swift
```swift
import SwiftUI

struct EditorView: View {
    @ObservedObject private var mgr = laramgr.shared
    private let systemPath = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist"

    private let origURL: URL
    private let modURL: URL

    @State private var mg: [String: Any] = [:]
    @State private var productType = ""
    @State private var customKey = ""
    @State private var customValue = ""
    @State private var customType: ValueType = .string
    @State private var status: String?
	@State private var shareItem: ShareItem?

    init() {
        let docs = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
        origURL = docs.appendingPathComponent("OriginalMobileGestalt.plist")
        modURL = docs.appendingPathComponent("ModifiedMobileGestalt.plist")
    }

    var body: some View {
        Form {
            Section("MobileGestalt") {
                Toggle("Action Button", isOn: toggle("cT44WE1EohiwRzhsZ8xEsw"))
                Toggle("Dynamic Island", isOn: toggle("YlEtTtHlNesRBMal1CqRaA"))
                Toggle("Stage Manager", isOn: toggle("qeaj75wk3HF4DwQ8qbIi7g"))
                Toggle("Apple Intelligence", isOn: toggle("A62OafQ85EJAiiqKn4agtg"))
            }

            Section("Device Spoofing") {
                TextField("Product type", text: $productType)
                    .textInputAutocapitalization(.never)
            }

            Section("Custom Key") {
                TextField("Key", text: $customKey)
                TextField("Value", text: $customValue)

                Picker("Type", selection: $customType) {
                    ForEach(ValueType.allCases) { t in
                        Text(t.rawValue).tag(t)
                    }
                }

                Button("Set", action: setKey)
                Button("Remove", role: .destructive, action: removeKey)
            }

            Section("Manage") {
                Button("Apply", action: apply)
                    .disabled(!mgr.kfsready)

                Button("Reset", action: reset)

                Button("Export Modified") { shareItem = ShareItem(url: modURL) }
				Button("Export Original") { shareItem = ShareItem(url: origURL) }
            }
        }
        .navigationTitle("MobileGestalt")
        .alert("Status", isPresented: .constant(status != nil)) {
            Button("OK") { status = nil }
        } message: {
            Text(status ?? "")
        }
        .onAppear(perform: load)
        .sheet(item: $shareItem) { item in
			ActivitySheet(url: item.url)
		}
    }

    private func cacheExtra() -> [String: Any] {
        mg["CacheExtra"] as? [String: Any] ?? [:]
    }

    private func toggle(_ key: String) -> Binding<Bool> {
        Binding(
            get: { cacheExtra()[key] != nil },
            set: { v in
                var c = cacheExtra()
                v ? (c[key] = 1) : c.removeValue(forKey: key)
                mg["CacheExtra"] = c
            }
        )
    }

    private func load() {
        let fm = FileManager.default
        let sys = URL(fileURLWithPath: systemPath)

        if !fm.fileExists(atPath: origURL.path) {
            try? fm.copyItem(at: sys, to: origURL)
        }
        if !fm.fileExists(atPath: modURL.path) {
            try? fm.copyItem(at: origURL, to: modURL)
        }

        mg = (NSDictionary(contentsOf: modURL) as? [String: Any]) ?? [:]
        productType = (cacheExtra()["h9jDsbgj7xIVeIQ8S3/X3Q"] as? String) ?? ""
    }

    private func apply() {
        var c = cacheExtra()
        if !productType.isEmpty { c["h9jDsbgj7xIVeIQ8S3/X3Q"] = productType }
        mg["CacheExtra"] = c

        do {
            let data = try PropertyListSerialization.data(fromPropertyList: mg, format: .xml, options: 0)
            try data.write(to: modURL)
            let ok = mgr.kfsoverwritewithdata(target: systemPath, data: data)
            status = ok ? "Applied." : "Failed."
        } catch {
            status = error.localizedDescription
        }
    }

    private func reset() {
        let fm = FileManager.default
        try? fm.removeItem(at: modURL)
        try? fm.copyItem(at: origURL, to: modURL)
        load()
        status = "Reset."
    }

    private func setKey() {
        var c = cacheExtra()

        let val: Any = switch customType {
        case .string: customValue
        case .int: Int(customValue) ?? 0
        case .bool: ["true","1","yes"].contains(customValue.lowercased())
        }

        c[customKey] = val
        mg["CacheExtra"] = c
        status = "Key set."
    }

    private func removeKey() {
        var c = cacheExtra()
        c.removeValue(forKey: customKey)
        mg["CacheExtra"] = c
        status = "Key removed."
    }
}

extension URL: Identifiable {
    public var id: String { absoluteString }
}

struct ActivitySheet: UIViewControllerRepresentable {
    let url: URL
    func makeUIViewController(context: Context) -> UIActivityViewController {
        UIActivityViewController(activityItems: [url], applicationActivities: nil)
    }
    func updateUIViewController(_ uiViewController: UIActivityViewController, context: Context) {}
}

struct ShareItem: Identifiable {
    let id = UUID()
    let url: URL
}

enum ValueType: String, CaseIterable, Identifiable {
    case string = "String"
    case int = "Int"
    case bool = "Bool"
    var id: String { rawValue }
}
```

However, still a crash when opening the newly compiled IPA. Apologies if there is something glaringly obvious missing, I know next-to-nothing about Swift.


---

## Timeline (Verbose)

- Mar 28, 2026: 🔴 Closed by @rooootdev


---

## Comments (1)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Mar 28, 2026

`ID: 4148898793`


lara currently does not support iOS 15, there are other, better kernel exploits for that version. there is no eta for iOS 15 support


---

*Exported on 10/14/2026, 2:47:02 PM from [https://github.com/rooootdev/lara/issues/10](https://github.com/rooootdev/lara/issues/10)*
