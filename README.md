# Mobile App Studio

> A Claude Code skill that reviews a mobile app the way a picky senior team would before launch, then fixes what it finds.

**Mobile App Studio** makes the AI work, in a fixed order, as four people at once:

- **Senior Mobile UI/UX Designer:** typography, spacing, alignment, components, the design system.
- **Product Designer:** flows, hierarchy, friction, microcopy.
- **UX QA Engineer:** states, keyboard, safe areas, accessibility, regressions.
- **Senior Mobile Motion Designer:** transitions, feedback, timing, easing, reduced motion.

It does not stop at a list of opinions. It **runs your app on a simulator or emulator**, uses it like a first-time user, **walks every flow the code makes possible** (including notifications, widgets, Shortcuts and deep links), writes a prioritised audit (P0–P3), **fixes the issues in your code**, **re-tests every fix on the device**, and then does a second full pass.

Works with **Expo / React Native, Flutter, SwiftUI / UIKit and native Android**.

---

## Contents

- [What it does](#what-it-does)
- [Install](#install)
- [Usage](#usage)
- [How it decides](#how-it-decides)
- [Output](#output)
- [Requirements](#requirements)
- [Repository layout](#repository-layout)
- [Customising](#customising)
- [FAQ](#faq)
- [Changelog](#changelog)
- [License](#license)

## What it does

The skill runs eight phases in order. It never skips to fixing before it understands the product, and it never finishes without re-testing on the device.

| # | Phase | What happens |
|---|---|---|
| 0 | **Set up the device and the evidence** | Detects the stack, finds the design tokens or theme, gets the app running on a simulator or emulator, and records the typecheck, lint and test baseline. |
| 1 | **Understand the product** | Works out what the app is, who it is for, the primary goal, core and secondary screens, the actions users perform most often, and builds a **flow inventory from the source**: routes, actions, store mutations, permissions, background jobs, deep links, notifications, widgets and intents. |
| 2 | **Use it like a real user** | Walks complete journeys, then **every flow in the inventory** in every data state (fresh install, one item, many, long text, longest generated label, completed, missing target, permission denied, interrupted). Checks persistence in storage and fills in a coverage matrix. |
| 3 | **Audit and report** | Goes through the UI/UX checklists and the motion checklist, classifies every issue P0–P3, and writes a report. |
| 4 | **Fix** | Makes the smallest clean change for each issue, reuses the existing tokens and components, consolidates duplicated components, and adds no hacks or arbitrary offsets. |
| 5 | **Re-test on the device** | Re-runs the flows, checks every modified screen, performs animations at normal speed, and runs typecheck, lint and tests against the baseline. |
| 6 | **Second pass** | Walks the whole app again as a newcomer ("what still feels wrong?"), then does one pass for motion only. |
| 7 | **Final bar and report** | Checks the launch-quality bar and reports what was found, fixed and remaining, the major fixes, and what could **not** be verified. |

### What it checks

- **Flows:** obvious next step, primary action, predictable navigation, back behaviour, dead ends, lost progress, feedback after actions.
- **Flow completeness:** every flow is reachable, gives feedback, lands in the right place, persists across relaunch, can be undone or is confirmed, never dead-ends, and updates every other surface that shows the same data. Store actions that no screen calls are reported.
- **Out-of-app surfaces:** home-screen widgets in every family (no empty bands, interactive elements write back), notification taps, Shortcuts and App Intents, deep links, and saved photos or files.
- **Typography:** one hierarchy (display → heading → body → caption → button → tab), consistent sizes, weights and line heights.
- **Spacing:** accidental values (13, 17, 23…) consolidated into a scale, and every boundary (header to content, gaps between groups, card and widget edges, the padding inside each button and chip) checked in full-resolution crops.
- **Alignment:** headers, cards, lists and especially the **bottom tab bar** (widths, centres, safe area, active states).
- **Components:** the same concept looks and behaves the same way; duplicates are merged into the canonical version.
- **Design system:** uses the existing tokens and never invents random values.
- **Hierarchy:** the most important thing on each screen is what the eye sees first; no competing CTAs.
- **Mobile:** touch targets, safe areas, keyboard, gestures, small and large screens, dynamic text.
- **Platform conventions:** feels native on iOS and Android.
- **Microcopy:** concise, action-oriented, consistent terminology.
- **States:** default, pressed, disabled, loading, success, error, empty, selected; the list and form states.
- **Accessibility:** contrast, labels on icon-only buttons, text scaling, screen-reader order.
- **Motion:** screen transitions, entrances, feedback, state changes, lists, sheets, modals, expand and collapse, hero transitions, success, loading and error states, gesture-driven UI, springs, timing, easing, motion hierarchy, reduced motion, and over-animation.

## Install

Clone into your Claude Code skills folder:

```bash
git clone https://github.com/ngnthanhdev/mobile-app-studio.git ~/.claude/skills/mobile-app-studio
```

To install it for one project only:

```bash
git clone https://github.com/ngnthanhdev/mobile-app-studio.git .claude/skills/mobile-app-studio
```

To update:

```bash
git -C ~/.claude/skills/mobile-app-studio pull
```

Restart Claude Code, or start a new session, so it picks up the skill.

## Usage

Open your mobile project in Claude Code and run:

```text
/mobile-app-studio
```

You can also just ask in plain language. The skill triggers on requests such as *"review this app like a senior designer"*, *"audit the UI/UX"*, *"polish this before launch"* or *"kiểm tra lại dự án"*.

### Options

| Invocation | Effect |
|---|---|
| `/mobile-app-studio` | Full run: audit, fix, re-test and a second pass across the whole app |
| `/mobile-app-studio checkout flow` | Limits the audit and fixes to one screen or flow. The product-understanding phase still covers the whole app. |
| `/mobile-app-studio --flows` | Flow-coverage pass only: inventory every flow, walk each one in every data state, fix what leaves a flow incomplete, re-test, and report the coverage matrix |
| `/mobile-app-studio --report-only` | Audit and report only, with no code changes |
| `/mobile-app-studio --no-motion` | Skips the motion passes |

## How it decides

- **It doesn't protect the existing design.** "It's probably fine" and "that's subjective" are not allowed. The question is always: *if I downloaded this app today, what would feel confusing, cheap, inconsistent or unfinished?*
- **Every change needs a concrete reason:** usability, consistency, hierarchy, platform conventions, balance, accessibility, or the app's own visual language. It never redesigns for taste.
- **It is autonomous on UI and UX.** It does not ask you to approve every spacing or copy fix.
- **It asks you only about product decisions:** business rules, removing features, pricing, API behaviour, permissions, user data or product strategy. Those questions are batched at the end with options and a recommendation.
- **It doesn't over-design.** No gradients, glassmorphism, extra shadows or palette changes; it aims for polished and intentional.
- **Motion needs a purpose.** Every animation must communicate something: what happened, where something came from, whether the action succeeded. Otherwise it is not added, and existing animation that makes the UI less clear is removed.
- **It is honest.** It never claims something was tested when it was not. Compiling is not testing.

## Output

1. **Code changes** in your project, following its existing patterns, tokens and animation library.
2. **An audit report** at `plans/reports/ux-audit-<date>-<slug>.md` (or your project's reports folder), with:
   - the product model;
   - flow, UI, consistency and motion issues with P0–P3 priorities;
   - design-system notes;
   - items that need a product decision;
   - things deliberately left unchanged;
   - results, including files touched and what was **not** verified.
3. **A final summary in your language:**
   - **UX audit summary:** found, fixed, remaining, and anything needing your decision.
   - **Major fixes:** each as *problem → solution → result*.
   - **Remaining risks:** anything that could not be verified.

## Requirements

- [Claude Code](https://docs.claude.com/en/docs/claude-code).
- A mobile project that runs locally.
- A device to drive, one of:
  - **iOS:** Xcode with a simulator. The Claude Code iOS Simulator tool is used when available; otherwise `xcrun simctl`.
  - **Android:** an emulator with `adb`.
  - A web preview can stand in, but native behaviour (keyboard, haptics, safe areas) is then reported as not verified.
- Optional: typecheck, lint and test commands, used as the baseline and final quality gates.

## Repository layout

```text
mobile-app-studio/
├── SKILL.md                              # The sequential workflow (phases 0–7) and ground rules
├── README.md
├── LICENSE
└── references/
    ├── ux-audit-checklist.md             # Product, flow, UI, consistency, mobile, states, a11y, final bar
    ├── motion-audit-checklist.md         # Motion principles, opportunities, timing, easing, physics, checks
    ├── device-qa-playbook.md             # Driving the iOS simulator, Android emulator and dev servers
    ├── report-template.md                # Audit report structure and final reply format
    ├── common-findings.md                # Issues that keep recurring in mobile apps, with the usual fixes
    └── flow-coverage.md                  # Flow inventory, data states, out-of-app surfaces, coverage matrix
```

## Customising

- **House rules:** add your team's conventions to `references/common-findings.md` (for example "all sheets use our `<Sheet>` component"). The skill checks this file during the audit.
- **Report location:** change the path in `SKILL.md` phase 3 and in `references/report-template.md`.
- **Priorities or quality bar:** edit `references/ux-audit-checklist.md` §9 and the priority definitions in `SKILL.md`.

## FAQ

**Will it rewrite my whole app?**
No. It makes the smallest clean change for each issue, keeps your architecture, business logic and API behaviour, and only consolidates components that have clearly drifted apart.

**Will it change prices, limits or features?**
No. Those are product decisions. It lists them under "Needs a product decision" and asks you.

**Can I just get the report?**
Yes: `/mobile-app-studio --report-only`.

**Does it add a new animation library?**
Only with a strong technical reason. It uses what the project already has, for example Reanimated, Flutter's animation APIs or SwiftUI animations.

## Changelog

### v1.3.0

- **Padding inside controls is a boundary.** The icon and label of every button, pill, chip and input must keep their padding to the control's own border. Content that spills into the padding is a defect even when nothing is clipped.
- **Longest generated label.** A new data state: labels built from data (weekdays, names, counts, dates, translations) are tested at their longest value, on the narrowest screen and with the largest text size.
- **Equal-width buttons with unequal labels** is a new common finding: `flex: 1` siblings where one label is roomy and the other is cramped or overflows.
- **Complete flows still get a visual check.** A flow marked Complete records the UI findings on its path, so a working flow can no longer hide a visual defect.

### v1.2.0

- **Flow coverage.** A new `references/flow-coverage.md` and a `--flows` mode. The skill now builds a flow inventory from the source (routes, actions, store mutations, permissions, background work, deep links, notifications, widgets, intents), walks every flow in every data state, verifies persistence in storage, and reports a coverage matrix. A flow is only "complete" when it is reachable, gives feedback, lands in the right place, persists, is reversible or confirmed, never dead-ends, and updates every other surface.
- **"Not testable" needs an attempt.** The device playbook now covers testing real notifications, adding and tapping home-screen widgets, running Shortcuts and App Intents (with log checks), seeding and verifying the photo library, reading the app's database from the simulator container, and recording motion as frame strips.
- **35 new common findings from a real audit:**
  - swipe-to-dismiss losing edits, completion without undo, dead-end finished lists, and blank "not found" screens;
  - context-blind "Add" buttons, store actions no screen calls, and permission prompts at launch;
  - picker double-tap races and heavy background work on every change;
  - English autocorrect on other languages, and the last word dropped on quick entry;
  - status-bar collisions, magic tab-bar insets, toasts that shift layout, clipped chip rows and option panels, and misaligned grid neighbours;
  - widget empty bands and overflow, and native controls restyled by iOS 26;
  - rounded-down money, parser false positives, and stale "now" indicators;
  - deprecated APIs that throw at runtime, and Fast Refresh duplicates.
- Every reply in the user's language, including progress notes and task labels.

### v1.1.0

- Spacing boundaries are now an explicit check: header to first content, gaps between groups, the outer edges of cards and widgets, and the bottom area.
- Spacing, alignment and clipping are judged only from full-resolution crops, never from downscaled screenshots.
- New common findings: content flush against the header divider, holes and empty bands in fixed-size containers, and chips truncated beside artwork.
- Fix repeated spacing defects in the shared shell or component instead of patching screens one by one.

### v1.0.0

- First release.
- Sequential eight-phase workflow: set up, understand, walk through, audit, fix, re-test, second pass, report.
- UI/UX, motion and device QA checklists, the report template, and common findings.

## License

[MIT](LICENSE) © ngnthanhdev
