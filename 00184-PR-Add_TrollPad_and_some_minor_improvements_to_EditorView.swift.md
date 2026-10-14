# Add TrollPad and some minor improvements to EditorView.swift

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#184](https://github.com/rooootdev/lara/pull/184)  
> **State:** 🟣 merged  
> **Branch:** `jurre111:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 24, 2026  
> **Changes:** +115 / -2 across 3 files  
> **Created:** Apr 24, 2026  
> **Updated:** Apr 24, 2026  
> **Closed:** Apr 24, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on Apr 24, 2026*


copied from SparseBox, so credits to Duy


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/24862340009/job/72790693625) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24862340009/job/72790568476) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 24, 2026

## Pull request overview

This PR extends the MobileGestalt editor UI by adding a “TrollPad”-style toggle to spoof iPadOS-related capabilities (via CacheExtra + CacheData), plus a couple of small UI gating improvements for existing toggles.

**Changes:**
- Gate “Stage Manager” UI to iPad devices and add an iPhone SE–specific “Tap to Wake” toggle.
- Add a “Become iPadOS” toggle that mutates `CacheExtra` capability keys and writes `DeviceClassNumber` via `CacheData`.
- Introduce `FindCacheDataOffset` (Objective-C) and expose it + a private `UIDevice` selector via the bridging header.

### Reviewed changes

Copilot reviewed 3 out of 3 changed files in this pull request and generated 6 comments.

| File | Description |
| ---- | ----------- |
| lara/views/EditorView.swift | Adds the new “Become iPadOS” binding + UI gating for Stage Manager and Tap to Wake. |
| lara/lara-Bridging-Header.h | Exposes `FindCacheDataOffset` and declares `UIDevice._hasHomeButton()`. |
| lara/FindCacheDataOffset.m | Implements offset discovery by scanning libMobileGestalt Mach-O sections. |





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (6)

*Inline comments on code changes*

### `lara/views/EditorView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 24, 2026 (line 282) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -233,7 +245,44 @@ struct EditorView: View {
             status = "serialization failed: \(error.localizedDescription)"
         }
     }
-    
+    private func bindingForTrollPad() -> Binding<Bool> {
+        // We're going to overwrite DeviceClassNumber but we can't do it via CacheExtra, so we need to do it via CacheData instead
+        guard let cacheData = mg["CacheData"] as? NSMutableData,
+              let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary else {
+            return State(initialValue: false).projectedValue
+        }
+        let valueOffset = FindCacheDataOffset("mtrAoWJ3gsq+I90ZnQ0vQw")
+        //print("Read value from \(cacheData.mutableBytes.load(fromByteOffset: valueOffset, as: Int.self))")
+        
+        let keys = [
+            "uKc7FPnEO++lVhHWHFlGbQ", // ipad
+            "mG0AnH/Vy1veoqoLRAIgTA", // MedusaFloatingLiveAppCapability
+            "UCG5MkVahJxG1YULbbd5Bg", // MedusaOverlayAppCapability
+            "ZYqko/XM5zD3XBfN5RmaXA", // MedusaPinnedAppCapability
+            "nVh/gwNpy7Jv1NOk00CMrw", // MedusaPIPCapability,
+            "qeaj75wk3HF4DwQ8qbIi7g", // DeviceSupportsEnhancedMultitasking
+        ]
+        return Binding(
+            get: {
+                if let value = cacheExtra[keys.first!] as? Int? {
+                    return value == 1
+                }
+                return false
+            },
+            set: { enabled in
+                cacheData.mutableBytes.storeBytes(of: enabled ? 3 : 1, toByteOffset: valueOffset, as: Int.self)
+                for key in keys {
+                    if enabled {
+                        cacheExtra[key] = 1
+                    } else {
+                        // just remove the key as it will be pulled from device tree if missing
+                        cacheExtra.removeObject(forKey: key)
+                    }
+                }
```

</details>

The `Binding` setter for "Become iPadOS" mutates `cacheExtra`/`cacheData` but doesn’t update any SwiftUI state (e.g., `valid = validate(mg)` like `mgkeybinding` does). Since `mg` is a reference type, these mutations may not trigger view updates for other UI that depends on `mg`/`valid`; consider updating `valid` (and/or reassigning `mg`) in the setter for consistency.
```suggestion
                }
                
                valid = validate(mg)
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 24, 2026 (line 109) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -104,12 +104,24 @@ struct EditorView: View {
                     // Toggle("Internal stuff", isOn: bindingForInternalStuff())
                     Toggle("Security Research Device", isOn: mgkeybinding(["XYlJKKkj2hztRP1NWWnhlw"]))
                     Toggle("Metal HUD for all apps", isOn: mgkeybinding(["EqrsVvjcYDdxHBiQmGhAWw"]))
-                    Toggle("Stage Manager (iPad Only?)", isOn: mgkeybinding(["qeaj75wk3HF4DwQ8qbIi7g"]))
+                    Toggle("Stage Manager", isOn: mgkeybinding(["qeaj75wk3HF4DwQ8qbIi7g"]))
+                        .disabled(UIDevice.current.userInterfaceIdiom != .pad)
+                    if UIDevice._hasHomeButton() {
```

</details>

`UIDevice._hasHomeButton()` is a private selector; if it’s not present on some OS versions this call will crash with an unrecognized selector. Consider guarding with `UIDevice.responds(to:)`/`instancesRespond(to:)` (or using a safer feature-detection approach) before calling it.
```suggestion
                    let hasHomeButtonSelector = Selector(("_hasHomeButton"))
                    if UIDevice.instancesRespond(to: hasHomeButtonSelector),
                       let value = UIDevice.current.perform(hasHomeButtonSelector)?.takeUnretainedValue() as? Bool,
                       value {
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 24, 2026 (lines 255-275) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -233,7 +245,44 @@ struct EditorView: View {
             status = "serialization failed: \(error.localizedDescription)"
         }
     }
-    
+    private func bindingForTrollPad() -> Binding<Bool> {
+        // We're going to overwrite DeviceClassNumber but we can't do it via CacheExtra, so we need to do it via CacheData instead
+        guard let cacheData = mg["CacheData"] as? NSMutableData,
+              let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary else {
+            return State(initialValue: false).projectedValue
+        }
+        let valueOffset = FindCacheDataOffset("mtrAoWJ3gsq+I90ZnQ0vQw")
+        //print("Read value from \(cacheData.mutableBytes.load(fromByteOffset: valueOffset, as: Int.self))")
+        
+        let keys = [
+            "uKc7FPnEO++lVhHWHFlGbQ", // ipad
+            "mG0AnH/Vy1veoqoLRAIgTA", // MedusaFloatingLiveAppCapability
+            "UCG5MkVahJxG1YULbbd5Bg", // MedusaOverlayAppCapability
+            "ZYqko/XM5zD3XBfN5RmaXA", // MedusaPinnedAppCapability
+            "nVh/gwNpy7Jv1NOk00CMrw", // MedusaPIPCapability,
+            "qeaj75wk3HF4DwQ8qbIi7g", // DeviceSupportsEnhancedMultitasking
+        ]
+        return Binding(
+            get: {
+                if let value = cacheExtra[keys.first!] as? Int? {
+                    return value == 1
+                }
+                return false
+            },
+            set: { enabled in
+                cacheData.mutableBytes.storeBytes(of: enabled ? 3 : 1, toByteOffset: valueOffset, as: Int.self)
+                for key in keys {
```

</details>

`FindCacheDataOffset(...)` is used as a byte offset into `CacheData`, but the code doesn’t validate that the returned offset is within `cacheData.length` before writing. If the offset is wrong (or the lookup fails), `storeBytes` can write out of bounds and crash/corrupt data; add bounds checks and a failure path (e.g., disable the toggle / show an error).


### `lara/FindCacheDataOffset.m`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 24, 2026 (lines 1-16) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,54 @@
+@import Foundation;
+@import Darwin;
+@import MachO;
+
+long FindCacheDataOffset(const char *mgKey) {
+    /*
+     * TL;DR: finding CacheData value offset is as follows:
+     * - Get a pointer to the corresponding obfuscated key in libMobileGestalt
+     * - Get a pointer to an unknown struct, whose first pointer is the pointer to the obfuscated key
+     * - Offset it by 0x9a (FIXME this lol), read it as uint16_t
+     * - Shift left the resulting offset by 3 bits
+     */
+
+    const struct mach_header_64 *header = NULL;
+    const char *mgName = "/usr/lib/libMobileGestalt.dylib";
+    dlopen(mgName, RTLD_GLOBAL);
```

</details>

This file uses `dlopen`, `assert`, `strncmp`, and `strlen` but doesn’t include the corresponding headers (`<dlfcn.h>`, `<assert.h>`, `<string.h>`). Depending on build settings (e.g., treating implicit declarations as errors), this can fail to compile; add the explicit includes.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 24, 2026 (line 24) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,54 @@
+@import Foundation;
+@import Darwin;
+@import MachO;
+
+long FindCacheDataOffset(const char *mgKey) {
+    /*
+     * TL;DR: finding CacheData value offset is as follows:
+     * - Get a pointer to the corresponding obfuscated key in libMobileGestalt
+     * - Get a pointer to an unknown struct, whose first pointer is the pointer to the obfuscated key
+     * - Offset it by 0x9a (FIXME this lol), read it as uint16_t
+     * - Shift left the resulting offset by 3 bits
+     */
+
+    const struct mach_header_64 *header = NULL;
+    const char *mgName = "/usr/lib/libMobileGestalt.dylib";
+    dlopen(mgName, RTLD_GLOBAL);
+
+    for (int i = 0; i < _dyld_image_count(); i++) {
+        if (!strncmp(mgName, _dyld_get_image_name(i), strlen(mgName))) {
+            header = (const struct mach_header_64 *)_dyld_get_image_header(i);
+            break;
+        }
+    }
+    assert(header);
```

</details>

Using `assert(header)` here is risky: in Release builds `assert` can be compiled out, leaving `header` as NULL and causing undefined behavior later. Prefer an explicit runtime check (e.g., return a sentinel like `-1`) and have the Swift caller handle failure.
```suggestion
    if (!header) {
        return -1;
    }
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 24, 2026 (lines 26-34) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,54 @@
+@import Foundation;
+@import Darwin;
+@import MachO;
+
+long FindCacheDataOffset(const char *mgKey) {
+    /*
+     * TL;DR: finding CacheData value offset is as follows:
+     * - Get a pointer to the corresponding obfuscated key in libMobileGestalt
+     * - Get a pointer to an unknown struct, whose first pointer is the pointer to the obfuscated key
+     * - Offset it by 0x9a (FIXME this lol), read it as uint16_t
+     * - Shift left the resulting offset by 3 bits
+     */
+
+    const struct mach_header_64 *header = NULL;
+    const char *mgName = "/usr/lib/libMobileGestalt.dylib";
+    dlopen(mgName, RTLD_GLOBAL);
+
+    for (int i = 0; i < _dyld_image_count(); i++) {
+        if (!strncmp(mgName, _dyld_get_image_name(i), strlen(mgName))) {
+            header = (const struct mach_header_64 *)_dyld_get_image_header(i);
+            break;
+        }
+    }
+    assert(header);
+
+    // Get a pointer to the corresponding obfuscated key in libMobileGestalt
+    size_t textCStringSize;
+    const char *textCStringSection = (const char *)getsectiondata(header, "__TEXT", "__cstring", &textCStringSize);
+    for (size_t size = 0; size < textCStringSize; size += strlen(textCStringSection + size) + 1) {
+        if (!strncmp(mgKey, textCStringSection + size, strlen(mgKey))) {
+            textCStringSection += size;
+            break;
+        }
+    }
```

</details>

If `mgKey` isn’t found in `__cstring`, `textCStringSection` remains pointing at the start of the section and the function will likely return an unrelated offset. Track whether a match was found and fail fast (return `-1`) when the key lookup (or later `constSection` lookup) doesn’t succeed.



---

## PR Commits (9)

- [`16690b7`](https://github.com/rooootdev/lara/commit/16690b7b16d8f95c08b64bb2127c0578541f0619) testrun for Trollpad — *jurre111* · Apr 23, 2026
- [`64fde64`](https://github.com/rooootdev/lara/commit/64fde64b2009eaa3344db2ee4560b00a74288236) stuff — *jurre111* · Apr 23, 2026
- [`3813702`](https://github.com/rooootdev/lara/commit/3813702658be4074629a0011b6ce69fc767d9754) oops — *jurre111* · Apr 23, 2026
- [`20df6ad`](https://github.com/rooootdev/lara/commit/20df6ad4e3140f880727d0480c6c996038fa79fe) again — *jurre111* · Apr 23, 2026
- [`2774a56`](https://github.com/rooootdev/lara/commit/2774a5623a49b82eb8f03b65c34d4f512485fa75) test on real mbg — *jurre111* · Apr 24, 2026
- [`09c2056`](https://github.com/rooootdev/lara/commit/09c205628ab57e5e3d3be7a8e61d6edc37274e36) Merge branch 'rooootdev:main' into main — *Jurre* · Apr 24, 2026
- [`8204141`](https://github.com/rooootdev/lara/commit/8204141953ed354048d18bd6277fe411e8d8214d) Fix respring? — *jurre111* · Apr 24, 2026
- [`5833302`](https://github.com/rooootdev/lara/commit/5833302b51668a348725037c8486a4a3a18e82a2) Merge branch 'main' of https://github.com/jurre111/lara — *jurre111* · Apr 24, 2026
- [`46dced9`](https://github.com/rooootdev/lara/commit/46dced9ec03730a9ae32b3fcce83314b005ac37b) Validate mg after setting TrollPad — *Jurre* · Apr 24, 2026


---

## Changed Files (3)

- 🟢 `lara/FindCacheDataOffset.m` (added) — +54 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,54 @@
+@import Foundation;
+@import Darwin;
+@import MachO;
+
+long FindCacheDataOffset(const char *mgKey) {
+    /*
+     * TL;DR: finding CacheData value offset is as follows:
+     * - Get a pointer to the corresponding obfuscated key in libMobileGestalt
+     * - Get a pointer to an unknown struct, whose first pointer is the pointer to the obfuscated key
+     * - Offset it by 0x9a (FIXME this lol), read it as uint16_t
+     * - Shift left the resulting offset by 3 bits
+     */
+
+    const struct mach_header_64 *header = NULL;
+    const char *mgName = "/usr/lib/libMobileGestalt.dylib";
+    dlopen(mgName, RTLD_GLOBAL);
+
+    for (int i = 0; i < _dyld_image_count(); i++) {
+        if (!strncmp(mgName, _dyld_get_image_name(i), strlen(mgName))) {
+            header = (const struct mach_header_64 *)_dyld_get_image_header(i);
+            break;
+        }
+    }
+    assert(header);
+
+    // Get a pointer to the corresponding obfuscated key in libMobileGestalt
+    size_t textCStringSize;
+    const char *textCStringSection = (const char *)getsectiondata(header, "__TEXT", "__cstring", &textCStringSize);
+    for (size_t size = 0; size < textCStringSize; size += strlen(textCStringSection + size) + 1) {
+        if (!strncmp(mgKey, textCStringSection + size, strlen(mgKey))) {
+            textCStringSection += size;
+            break;
+        }
+    }
+
+    // Get a pointer to an unknown struct, whose first pointer is the pointer to the obfuscated key
+    size_t constSize;
+    // arm64e
+    const uintptr_t *constSection = (const uintptr_t *)getsectiondata(header, "__AUTH_CONST", "__const", &constSize);
+    if (!constSection) {
+        // arm64, FIXME: is this correct?
+        constSection = (const uintptr_t *)getsectiondata(header, "__DATA_CONST", "__const", &constSize);
+    }
+    for (int i = 0; i < constSize / 8; i++) {
+        if (constSection[i] == (uintptr_t)textCStringSection) {
+            constSection += i;
+            break;
+        }
+    }
+
+    // FIXME: is offset of offset consistent?
+    return (int)((uint16_t *)constSection)[0x9a/2] << 3;
+    //[NSUserDefaults.standardUserDefaults setInteger:offset forKey:@"MGCacheDataDeviceClassNumberOffset"];
+}
  ```

  </details>
- 🟡 `lara/lara-Bridging-Header.h` (modified) — +7 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -3,6 +3,7 @@
 //  lara
 //
 
+@import UIKit;
 #import <Foundation/Foundation.h>
 
 #import "darksword.h"
@@ -14,6 +15,12 @@
 #import "rc.h"
 #import "RemoteCall.h"
 
+long FindCacheDataOffset(const char *mgKey);
+
+@interface UIDevice(Private)
++ (BOOL)_hasHomeButton;
+@end
+
 void test(NSString *path);
 
 NS_ASSUME_NONNULL_BEGIN
  ```

  </details>
- 🟡 `lara/views/EditorView.swift` (modified) — +54 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -104,12 +104,24 @@ struct EditorView: View {
                     // Toggle("Internal stuff", isOn: bindingForInternalStuff())
                     Toggle("Security Research Device", isOn: mgkeybinding(["XYlJKKkj2hztRP1NWWnhlw"]))
                     Toggle("Metal HUD for all apps", isOn: mgkeybinding(["EqrsVvjcYDdxHBiQmGhAWw"]))
-                    Toggle("Stage Manager (iPad Only?)", isOn: mgkeybinding(["qeaj75wk3HF4DwQ8qbIi7g"]))
+                    Toggle("Stage Manager", isOn: mgkeybinding(["qeaj75wk3HF4DwQ8qbIi7g"]))
+                        .disabled(UIDevice.current.userInterfaceIdiom != .pad)
+                    if UIDevice._hasHomeButton() {
+                        Toggle("Tap to Wake (iPhone SE)", isOn: mgkeybinding(["yZf3GTRMGTuwSV/lD7Cagw"]))
+                    }
                 } header: {
                     Text("MobileGestalt")
                 } footer: {
                     Text("Note: some tweaks may not work or cause instability.\nWARNING: Never enable features your device doesn't support.")
                 }
+                Section {
+                    let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary
+                    Toggle("Become iPadOS", isOn: bindingForTrollPad())
+                    // validate DeviceClass
+                        .disabled(cacheExtra?["+3Uf0Pm5F8Xy7Onyvko0vA"] as? String != "iPhone")
+                } footer: {
+                    Text("Override user interface idiom to iPadOS, so you could use all iPadOS multitasking features on iPhone. Gives you the same capabilities as TrollPad, but may cause some issues.\nPLEASE DO NOT TURN OFF SHOW DOCK IN STAGE MANAGER OTHERWISE YOUR PHONE WILL BOOTLOOP WHEN ROTATING TO LANDSCAPE.")
+                }
                 Section {
                     HStack {
                         Text("Status")
@@ -177,6 +189,7 @@ struct EditorView: View {
             .alert("Done", isPresented: .constant(alert != nil)) {
                 Button("Cancel") { alert = nil }
                 Button("Respring") {
+                    alert = nil
                     mgr.respring()
                 }
             } message: {
@@ -233,7 +246,46 @@ struct EditorView: View {
             status = "serialization failed: \(error.localizedDescription)"
         }
     }
-    
+    private func bindingForTrollPad() -> Binding<Bool> {
+        // We're going to overwrite DeviceClassNumber but we can't do it via CacheExtra, so we need to do it via CacheData instead
+        guard let cacheData = mg["CacheData"] as? NSMutableData,
+              let cacheExtra = mg["CacheExtra"] as? NSMutableDictionary else {
+            return State(initialValue: false).projectedValue
+        }
+        let valueOffset = FindCacheDataOffset("mtrAoWJ3gsq+I90ZnQ0vQw")
+        //print("Read value from \(cacheData.mutableBytes.load(fromByteOffset: valueOffset, as: Int.self))")
+        
+        let keys = [
+            "uKc7FPnEO++lVhHWHFlGbQ", // ipad
+            "mG0AnH/Vy1veoqoLRAIgTA", // MedusaFloatingLiveAppCapability
+            "UCG5MkVahJxG1YULbbd5Bg", // MedusaOverlayAppCapability
+            "ZYqko/XM5zD3XBfN5RmaXA", // MedusaPinnedAppCapability
+            "nVh/gwNpy7Jv1NOk00CMrw", // MedusaPIPCapability,
+            "qeaj75wk3HF4DwQ8qbIi7g", // DeviceSupportsEnhancedMultitasking
+        ]
+        return Binding(
+            get: {
+                if let value = cacheExtra[keys.first!] as? Int? {
+                    return value == 1
+                }
+                return false
+            },
+            set: { enabled in
+                cacheData.mutableBytes.storeBytes(of: enabled ? 3 : 1, toByteOffset: valueOffset, as: Int.self)
+                for key in keys {
+                    if enabled {
+                        cacheExtra[key] = 1
+                    } else {
+                        // just remove the key as it will be pulled from device tree if missing
+                        cacheExtra.removeObject(forKey: key)
+                    }
+                }
+                
+                valid = validate(mg)
+            }
+        )
+    }
+
     private func mgkeybinding<T: Equatable>(_ keys: [String], type: T.Type = Int.self, default: T? = 0, enable: T? = 1) -> Binding<Bool> {
         guard let cachextra = mg["CacheExtra"] as? NSMutableDictionary else {
             return State(initialValue: false).projectedValue
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

- : 📝 Commit 16690b7: testrun for Trollpad
- : 📝 Commit 64fde64: stuff
- : 📝 Commit 3813702: oops
- : 📝 Commit 20df6ad: again
- : 📝 Commit 2774a56: test on real mbg
- : 📝 Commit 09c2056: Merge branch 'rooootdev:main' into main
- Apr 24, 2026: 👀 @jurre111 requested review from @Copilot
- : 💬 someone reviewed: commented
- : 📝 Commit 8204141: Fix respring?
- : 📝 Commit 5833302: Merge branch 'main' of https://github.com/jurre111/lara
- : 📝 Commit 46dced9: Validate mg after setting TrollPad
- Apr 24, 2026: 🟣 @rooootdev merged
- Apr 24, 2026: 🔴 Closed by @rooootdev
- Apr 26, 2026: 🔗 Commit 930a0c7 — Squashed commit of the following:
- Apr 27, 2026: 🔗 Commit 07c3a6c — Squashed commit of the following:
- Apr 29, 2026: 🔗 Commit e0da526 — Squashed commit of the following:
- Apr 29, 2026: 🔗 Commit 9fc6ffb — Squashed commit of the following:


---

## Comments (1)

### Comment #1 — [@jurre111](https://github.com/jurre111) · Apr 24, 2026

`ID: 4308723525`


Had to set alert to nil so that the alert would'nt cancel the respring webview


---

*Exported on 10/14/2026, 3:01:39 PM from [https://github.com/rooootdev/lara/pull/184](https://github.com/rooootdev/lara/pull/184)*
