# Fix MobileGestalt ProductType and original subtype handling

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#243](https://github.com/rooootdev/lara/pull/243)  
> **State:** 🟣 merged  
> **Branch:** `hxhlb:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on May 13, 2026  
> **Changes:** +15 / -7 across 1 files  
> **Created:** May 13, 2026  
> **Updated:** May 13, 2026  
> **Closed:** May 13, 2026  

---

## Description

*Opened by [@hxhlb](https://github.com/hxhlb)* *on May 13, 2026*


## Summary

Fix MobileGestalt editor state issues that could cause unintended writes or misleading picker state when applying changes.

1. `mgProductType` was initialized as an empty string and was not populated from the current MobileGestalt data before applying. If the user did not touch the spoofing picker, applying MobileGestalt could overwrite `h9jDsbgj7xIVeIQ8S3/X3Q` with an empty string.

2. `mgOriginalSubtype` was stored in `@AppStorage`, so a previously persisted incorrect value could continue to affect the "Original" subtype option. This made the original subtype picker state depend on stale app preferences instead of the saved original MobileGestalt data.

3. The current subtype picker selection was also loaded from `SavedGestalt.plist`. That made the UI show the original subtype even if the current MobileGestalt cache had already been changed to another subtype.

This PR initializes `mgProductType` from the current MobileGestalt cache and falls back to `machineName()` if missing. It also makes `mgOriginalSubtype` derived from the saved original MobileGestalt data, while `mgSubtype` is loaded from the current MobileGestalt cache.

## Changes

- Initialize `mgProductType` from `CacheExtra["h9jDsbgj7xIVeIQ8S3/X3Q"]`.
- Avoid writing an empty ProductType back to MobileGestalt.
- Change `mgOriginalSubtype` from `@AppStorage` to `@State`.
- Refresh `mgOriginalSubtype` from `SavedGestalt.plist` each time MobileGestalt data is prepared.
- Load the current picker subtype from the active MobileGestalt cache instead of the saved original backup.

## Testing

Tested on an iPhone 13 Pro Max running iOS 17.3.1.

- Applying MobileGestalt without changing the spoofing picker no longer clears `h9jDsbgj7xIVeIQ8S3/X3Q`.
- The original subtype remains `2778` instead of being overwritten with a stale persisted value.
- Exported MobileGestalt confirmed:
  - `h9jDsbgj7xIVeIQ8S3/X3Q = iPhone14,3`
  - `ArtworkDeviceSubType = 2778`
  - Dynamic Island key was not added.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25791641010/job/75758982366) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25791641010/job/75758430815) — **success** — *GitHub Actions*


---

## PR Commits (2)

- [`03f3008`](https://github.com/rooootdev/lara/commit/03f300866600bde7cf30bef8727a40f1d73c946e) Fix MobileGestalt ProductType and original subtype state — *Hades* · May 13, 2026
- [`74a3b1d`](https://github.com/rooootdev/lara/commit/74a3b1d12f0ce98eda0c0c4f054bec7e45c816f0) Load current MobileGestalt subtype separately — *Hades* · May 13, 2026


---

## Changed Files (1)

- 🟡 `lara/views/tweaks/ui/mobilegestalt/GestaltView.swift` (modified) — +15 / -7
  <details><summary>Diff</summary>

  ```diff
  @@ -19,7 +19,7 @@ struct GestaltView: View {
     @State private var isGestaltVaild: Bool = false
     
     @State private var mgSubtype: Int = 0
-    @AppStorage("mgOriginalSubtype") private var mgOriginalSubtype: Int = 0
+    @State private var mgOriginalSubtype: Int = 0
     @State private var mgEnableDeviceName: Bool = false
     @AppStorage("mgDeviceName") private var mgDeviceName: String = ""
     @State private var mgProductType: String = ""
@@ -250,11 +250,17 @@ struct GestaltView: View {
             let cacheExtra = mgSavedDict["CacheExtra"] as? NSMutableDictionary ?? NSMutableDictionary()
             let ArtworkDict = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary ?? NSMutableDictionary()
             
-            guard let subType = ArtworkDict["ArtworkDeviceSubType"] as? Int else { throw "Failed to get ArtworkDeviceSubType!" }
-            mgSubtype = subType
-            
-            if mgOriginalSubtype == 0 {
-                mgOriginalSubtype = subType
+            guard let originalSubType = ArtworkDict["ArtworkDeviceSubType"] as? Int else { throw "Failed to get ArtworkDeviceSubType!" }
+            mgOriginalSubtype = originalSubType
+
+            let currentCacheExtra = mgCurrentDict["CacheExtra"] as? NSMutableDictionary ?? NSMutableDictionary()
+            let currentArtworkDict = currentCacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary ?? NSMutableDictionary()
+            mgSubtype = currentArtworkDict["ArtworkDeviceSubType"] as? Int ?? originalSubType
+
+            if let productType = currentCacheExtra["h9jDsbgj7xIVeIQ8S3/X3Q"] as? String, !productType.isEmpty {
+                mgProductType = productType
+            } else {
+                mgProductType = machineName()
             }
             
             guard let deviceName = ArtworkDict["ArtworkDeviceProductDescription"] as? String else { throw "Failed to get ArtworkDeviceProductDescription!" }
@@ -277,7 +283,9 @@ struct GestaltView: View {
         do {
             // first, update the dictionary with some specific properties.
             let cacheExtra = mgCurrentDict["CacheExtra"] as? NSMutableDictionary ?? NSMutableDictionary()
-            cacheExtra["h9jDsbgj7xIVeIQ8S3/X3Q"] = mgProductType
+            if !mgProductType.isEmpty {
+                cacheExtra["h9jDsbgj7xIVeIQ8S3/X3Q"] = mgProductType
+            }
             
             let ArtworkDict = cacheExtra["oPeik/9e8lQWMszEjbPzng"] as? NSMutableDictionary ?? NSMutableDictionary()
             ArtworkDict["ArtworkDeviceSubType"] = mgSubtype
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 03f3008: Fix MobileGestalt ProductType and original subtype state
- : 📝 Commit 74a3b1d: Load current MobileGestalt subtype separately
- May 13, 2026: 🟣 @rooootdev merged
- May 13, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:06:38 PM from [https://github.com/rooootdev/lara/pull/243](https://github.com/rooootdev/lara/pull/243)*
