# fix: dispatch rcinitDaemon completion handler back to main queue

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#179](https://github.com/rooootdev/lara/pull/179)  
> **State:** 🔴 closed  
> **Branch:** `tyhallcsu:fix/rcinitdaemon-completion-threading` → `rooootdev:main`  
> **Changes:** +2 / -2 across 1 files  
> **Created:** Apr 23, 2026  
> **Updated:** Apr 23, 2026  
> **Closed:** Apr 23, 2026  

---

## Description

*Opened by [@tyhallcsu](https://github.com/tyhallcsu)* *on Apr 23, 2026*


## Problem

`rcinitDaemon` in `laramgr.swift` fires its `completion` callback on a background `DispatchQueue.global` queue:

```swift
DispatchQueue.global(qos: .userInitiated).async { [weak self] in
    // ...
    let proc = RemoteCall(process: process, useMigFilterBypass: migbypass)
    completion?(proc)   // ← background thread
    // ...
}
```

Every other completion callback in this file (`run`, `vfsinit`, `sbxescape`, `rcinit`) dispatches back to `DispatchQueue.main` before calling its completion. `rcinitDaemon` is the only one that doesn't.

Any caller that updates `@Published` properties or SwiftUI bindings from this completion will produce a **"Publishing changes from background threads is not allowed"** runtime warning on Xcode 15+, and can cause undefined state mutation behavior (race on `@Published` property observed from main thread).

## Fix

Wrap `completion?(proc)` in `DispatchQueue.main.async { }`, consistent with all other completion handlers in the file.

```swift
// Before
completion?(proc)

// After
DispatchQueue.main.async { completion?(proc) }
```

## Verification

Add a `rcinitDaemon` caller that updates a `@Published` var in the completion block. Before this fix: Xcode runtime warning logged. After: no warning.


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`d0fed5e`](https://github.com/rooootdev/lara/commit/d0fed5e919f53b1a0295aa539ea5e628b92418d5) fix: dispatch rcinitDaemon completion to main queue — *tyhallcsu* · Apr 23, 2026


---

## Changed Files (1)

- 🟡 `lara/classes/laramgr.swift` (modified) — +2 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -509,8 +509,8 @@ final class laramgr: ObservableObject {
             }
             
             let proc = RemoteCall(process: process, useMigFilterBypass: migbypass)
-            completion?(proc)
-            
+            DispatchQueue.main.async { completion?(proc) }
+
             DispatchQueue.main.async {
                 guard let self = self else { return }
                 let success = proc != nil
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit d0fed5e: fix: dispatch rcinitDaemon completion to main queue
- Apr 23, 2026: 🔴 Closed by @rooootdev


---

## Comments (1)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Apr 23, 2026

`ID: 4305576446`


pull request closed. relevance.


---

*Exported on 10/14/2026, 3:01:05 PM from [https://github.com/rooootdev/lara/pull/179](https://github.com/rooootdev/lara/pull/179)*
