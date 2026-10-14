# Update README.md

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#56](https://github.com/rooootdev/lara/pull/56)  
> **State:** 🔴 closed  
> **Branch:** `Pro42good:patch-4` → `rooootdev:main`  
> **Changes:** +1 / -0 across 1 files  
> **Created:** Apr 8, 2026  
> **Updated:** Apr 9, 2026  
> **Closed:** Apr 9, 2026  

---

## Description

*Opened by [@Pro42good](https://github.com/Pro42good)* *on Apr 8, 2026*


Added a known issue about reseting MobileGestalt and it bricking phones.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (1)

- 🔴 [build](https://github.com/rooootdev/lara/actions/runs/24149413798/job/70471973889) — **failure** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 8, 2026

## Pull request overview

Adds a new known-issue warning to the README to prevent users from triggering a MobileGestalt reset that can brick devices.

**Changes:**
- Documented a known issue: resetting MobileGestalt within the app can brick phones.





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (1)

*Inline comments on code changes*

### `README.md`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 8, 2026 (line 61) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -58,6 +58,7 @@ If lara doesnt work on your device, and you want to help the project, please als
 - doesnt work on ipad m2?
 - doesnt work on a18 pro?
 - kernelcache download broken for some versions.
+- Resetting MobileGestalt within the app bricks phones. (*Dont use the build in reset, no matter what*)
```

</details>

Typo/grammar in this warning: "Dont" should be "Don't", and "build in" should be "built-in" (or "built in" as a verb, but here it reads as an adjective for "reset"). Since this is a safety/brick warning, wording should be unambiguous.



---

## PR Commits (3)

- [`c5504f1`](https://github.com/rooootdev/lara/commit/c5504f11379b4ece56618888f5bd29f2cf4f9269) Update README.md — *Pro42good DQ* · Apr 8, 2026
- [`1cfc50c`](https://github.com/rooootdev/lara/commit/1cfc50c9a5df9ffeed57d4cf4105296f7bbed256) Fix typo in MobileGestalt reset warning — *Pro42good DQ* · Apr 8, 2026
- [`2a5196c`](https://github.com/rooootdev/lara/commit/2a5196c600853d7dc111b656c79f025f04c1b451) Fix typo in MobileGestalt warning message — *Pro42good DQ* · Apr 8, 2026


---

## Changed Files (1)

- 🟡 `README.md` (modified) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -58,6 +58,7 @@ If lara doesnt work on your device, and you want to help the project, please als
 - doesnt work on ipad m2?
 - doesnt work on a18 pro?
 - kernelcache download broken for some versions.
+- Resetting MobileGestalt within the app bricks phones. (*Don't use the built-in reset, current build*)
 
 ## installation:
 <a href="https://celloserenity.github.io/altdirect/?url=https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/source.json" target="_blank">
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit c5504f1: Update README.md
- Apr 8, 2026: 👀 @Pro42good requested review from @Copilot
- : 💬 someone reviewed: commented
- : 📝 Commit 1cfc50c: Fix typo in MobileGestalt reset warning
- : 📝 Commit 2a5196c: Fix typo in MobileGestalt warning message
- Apr 9, 2026: 🔴 Closed by @Pro42good


---

## Comments (2)

### Comment #1 — [@XtraCube](https://github.com/XtraCube) · Apr 8, 2026

`ID: 4209225939`


pretty sure this was fixed


---

### Comment #2 — [@Pro42good](https://github.com/Pro42good) · Apr 9, 2026

`ID: 4210862575`


I'll Close my PR for the time being.


---

*Exported on 10/14/2026, 2:50:49 PM from [https://github.com/rooootdev/lara/pull/56](https://github.com/rooootdev/lara/pull/56)*
