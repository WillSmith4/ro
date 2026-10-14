# [Feature] SystemColor Patcher Themes/Presets

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#287](https://github.com/rooootdev/lara/issues/287)  
> **State:** 🟢 open  
> **Labels:** `enhancement`  
> **Created:** Jun 1, 2026  
> **Updated:** Jul 8, 2026  

---

## Description

*Opened by [@adrilyphia](https://github.com/adrilyphia)* *on Jun 1, 2026*


### Is your feature request related to a problem?
<!-- A clear and concise description of what the problem is. e.g. "It's frustrating when..." -->
``Changing the colors for everything and bringing them back to their original state is a bit tedious.``
### Describe the solution you'd like:
<!-- What you want to happen. Be specific about the behavior, UI, or capability. -->
``A theme/preset section either above or under the color selectors. A list of buttons with the names of the themes as labels and a little preview of the color scheme on the right side.
Whenever the user presses one of them, the colors automatically change and are applied.
A reset button that resets and applies the colors back to their original state upon tapping.
A tiny preview for the patching with typical iOS elements (badges, labels, elements derived from other apps, etc.)
Some examples of themes are the typical colorschemes found everywhere: Catpuccin, Dracula, Gruvbox, Nord, One Dark, Rosé Pine, Solarized, Tokyo Night, and much more.
Documentation could be made for creating themes/presets and uploading them as repos, similar to how user-published fonts are managed.``

### Describe alternatives you've considered:
<!-- Other approaches, workarounds, or existing tools that partially solve this. -->
``Adding colors to the system's color picker and applying them from there.``
### Scope:
<!-- Check all that apply. -->
- [x] UI / UX change
- [ ] New exploit capability or device support
- [x] Tweak / package management
- [ ] Persistence or boot behavior
- [ ] Logging, diagnostics, or error handling
- [ ] Build system / CI
- [x] Documentation
- [ ] Other
### Additional context:
<!-- Mockups, references to similar features in other jailbreaks, related issues, links to research. -->
```
¯\_(ツ)_/¯ There's nothing much I have to say about this.
```
### Are you willing to help implement this?
- [ ] Yes, I can submit a PR
- [x] I can help test
- [x] I'm just suggesting the idea

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Requests missing these may be closed. -->
- [x] I have searched existing issues (open and closed) and confirmed this feature has not already been requested
- [x] This request is relevant to the latest version / commit of Lara
- [x] I have provided sufficient detail (problem, proposed solution, scope) for a maintainer to evaluate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- Jun 1, 2026: 🏷️ @adrilyphia added label `enhancement`


---

## Comments (1)

### Comment #1 — [@adrilyphia](https://github.com/adrilyphia) · Jun 21, 2026

`ID: 4763302826`


I forgot to mention one more significant feature:

The ability to **extract dominant colors from a user-defined image** (usually the device's wallpaper or a color palette image) in order to generate a color scheme, similar to [matugen](https://github.com/InioX/matugen) or [pywal](https://github.com/dylanaraps/pywal).

The user uploads an image and defines how many dominant colors to extract. They can then either let the app select the colors using algorithms such as k-means or median cut, or select them manually. The app then adjusts these colors (brightening, dimming, making them more or less vibrant, shifting the hue, and so on).


---

*Exported on 10/14/2026, 3:10:30 PM from [https://github.com/rooootdev/lara/issues/287](https://github.com/rooootdev/lara/issues/287)*
