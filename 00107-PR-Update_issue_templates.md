# Update issue templates

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#107](https://github.com/rooootdev/lara/pull/107)  
> **State:** 🟣 merged  
> **Branch:** `rooootdev:Pro42good-patch-1` → `rooootdev:main`  
> **Merged by:** [@Pro42good](https://github.com/Pro42good) on Apr 16, 2026  
> **Changes:** +94 / -0 across 2 files  
> **Created:** Apr 16, 2026  
> **Updated:** Apr 16, 2026  
> **Closed:** Apr 16, 2026  

---

## Description

*Opened by [@Pro42good](https://github.com/Pro42good)* *on Apr 16, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (1)

- ✅ [build](https://github.com/rooootdev/lara/actions/runs/24527085855/job/71700077682) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 16, 2026

## Pull request overview

Adds GitHub issue templates to standardize how users file bugs and request features.

**Changes:**
- Introduces a Feature Request issue template with scope checkboxes and a pre-submission checklist.
- Introduces a Bug Report issue template that captures reproduction steps, device info, logs, and a pre-submission checklist.

### Reviewed changes

Copilot reviewed 2 out of 2 changed files in this pull request and generated 1 comment.

| File | Description |
| ---- | ----------- |
| .github/ISSUE_TEMPLATE/feature_request.md | New feature request template with structured prompts and scope categories. |
| .github/ISSUE_TEMPLATE/bug_report.md | New bug report template with required debugging details (device info/logs) and checklist. |





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (1)

*Inline comments on code changes*

### `.github/ISSUE_TEMPLATE/bug_report.md`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 16, 2026 (line 3) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,50 @@
+---
+name: Bug report
+about: Create a report to help us improve lara
```

</details>

The project name is lowercase here ("lara"), but elsewhere in this template (and the feature template) it’s capitalized as "Lara". Please capitalize for consistency and professionalism in the issue UI.
```suggestion
about: Create a report to help us improve Lara
```



---

## PR Commits (2)

- [`f3107b0`](https://github.com/rooootdev/lara/commit/f3107b07bc5e915ee62498afab07dac6c8897f03) Update issue templates — *Pro42good DQ* · Apr 16, 2026
- [`57bdecc`](https://github.com/rooootdev/lara/commit/57bdecc6ce2f3f7095f55e4864357b6716d5ad26) Update .github/ISSUE_TEMPLATE/bug_report.md — *Pro42good DQ* · Apr 16, 2026


---

## Changed Files (2)

- 🟢 `.github/ISSUE_TEMPLATE/bug_report.md` (added) — +50 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,50 @@
+---
+name: Bug report
+about: Create a report to help us improve Lara
+title: "[BUG] Insert Title Here"
+labels: bug
+assignees: ''
+
+---
+
+### Describe the bug:
+<!-- A clear and concise description of what the bug is. -->
+``[Insert here]``
+
+### To Reproduce:
+Steps to reproduce the behavior:
+1. Go to '...'
+2. Tap '...'
+3. Scroll down to '...'
+4. See error
+
+### Expected behavior:
+<!-- What you expected to happen. -->
+``[Insert here]``
+
+### Screenshots:
+<!-- If applicable, add screenshots to help explain your problem. -->
+
+### Device Info:
+- **Device:** ``[Insert here]`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
+- **Chip:** ``[Insert here]`` <!-- e.g. A15, A14, M1 -->
+- **iOS Version:** ``[Insert here]`` <!-- e.g. 16.5 (20F66) — include the build number -->
+- **Jailbroken before?** ``[Insert here]`` <!-- Yes / No / First attempt on this device -->
+- **Lara version / commit:** ``[Insert here]`` <!-- e.g. v0.3 or commit hash -->
+
+### Logs:
+<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
+```
+(logs here)
+```
+
+### Additional context:
+<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
+``[Insert here]``
+
+### Pre-submission checklist:
+<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
+- [ ] I have searched existing issues (open and closed) and confirmed this is not a duplicate
+- [ ] I can reproduce this on the latest version / commit of Lara
+- [ ] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
+- [ ] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
  ```

  </details>
- 🟢 `.github/ISSUE_TEMPLATE/feature_request.md` (added) — +44 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,44 @@
+---
+name: Feature request
+about: Suggest an idea for this project
+title: "[Feature] Insert Title Here"
+labels: enhancement
+assignees: ''
+
+---
+
+### Is your feature request related to a problem?
+<!-- A clear and concise description of what the problem is. e.g. "It's frustrating when..." -->
+``[Insert here]``
+### Describe the solution you'd like:
+<!-- What you want to happen. Be specific about the behavior, UI, or capability. -->
+``[Insert here]``
+### Describe alternatives you've considered:
+<!-- Other approaches, workarounds, or existing tools that partially solve this. -->
+``[Insert here]``
+### Scope:
+<!-- Check all that apply. -->
+- [ ] UI / UX change
+- [ ] New exploit capability or device support
+- [ ] Tweak / package management
+- [ ] Persistence or boot behavior
+- [ ] Logging, diagnostics, or error handling
+- [ ] Build system / CI
+- [ ] Documentation
+- [ ] Other
+### Additional context:
+<!-- Mockups, references to similar features in other jailbreaks, related issues, links to research. -->
+```
+(paste references, logs, or snippets here if useful)
+```
+### Are you willing to help implement this?
+- [ ] Yes, I can submit a PR
+- [ ] I can help test
+- [ ] I'm just suggesting the idea
+
+### Pre-submission checklist:
+<!-- Please confirm all of the following before submitting. Requests missing these may be closed. -->
+- [ ] I have searched existing issues (open and closed) and confirmed this feature has not already been requested
+- [ ] This request is relevant to the latest version / commit of Lara
+- [ ] I have provided sufficient detail (problem, proposed solution, scope) for a maintainer to evaluate
+- [ ] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
  ```

  </details>


---

## References

### Commits (1)

- [1cf2e5c](https://github.com/danosito/lara-m-chips-fix/commit/1cf2e5c8f548faa5a21306c4b2468306c66bc916) — Merge pull request #107 from rooootdev/Pro42good-patch-1


---

## Timeline (Verbose)

- : 📝 Commit f3107b0: Update issue templates
- Apr 16, 2026: 👀 @Pro42good requested review from @Copilot
- : 💬 someone reviewed: commented
- : 📝 Commit 57bdecc: Update .github/ISSUE_TEMPLATE/bug_report.md
- Apr 16, 2026: 🟣 @Pro42good merged
- Apr 16, 2026: 🔴 Closed by @Pro42good
- Apr 19, 2026: 🔗 Commit 1cf2e5c — Merge pull request #107 from rooootdev/Pro42good-patch-1


---

*Exported on 10/14/2026, 2:54:50 PM from [https://github.com/rooootdev/lara/pull/107](https://github.com/rooootdev/lara/pull/107)*
