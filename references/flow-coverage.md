# Flow coverage: test every flow until it is complete

A screen-by-screen audit misses whatever the audit never reached. This file is the method for knowing the app completely: list every flow the code makes possible, walk each one on the device from its entry point to a sensible end, and keep a coverage matrix until nothing is left unwalked or unexplained.

A flow is **complete** ("tròn") when all of these hold:

1. **Reachable:** a user can find the entry point without developer knowledge.
2. **Feedback:** every action visibly says it worked (or failed) within about 100 ms, and slow actions show progress.
3. **Right destination:** after the action, the user lands where they expect, and the new data is visible there.
4. **Persisted:** the change survives an app relaunch. Verify it in storage, not only on screen.
5. **Reversible or confirmed:** destructive or completing actions either ask first or can be undone, and the way back is discoverable.
6. **Next action:** no dead end. Empty, completed, error and "not found" states all offer something to do next.
7. **Consistent everywhere:** every other surface that shows the same data (other tabs, widgets, notifications, lock screen, badges, counts) updates too.

## Step 1 — Build the flow inventory from the source

Do not rely on what you notice while tapping. Enumerate the flows the code makes possible:

- **Routes and screens:** every file in the router (`app/`, navigation stacks, `Navigator` registrations). Each screen needs at least one flow that reaches it and one that leaves it.
- **Entry points from outside the app:** deep links and URL schemes, notification tap handlers, home-screen and lock-screen widgets (every size or family), App Intents, Siri or Shortcuts actions, share extensions, quick actions, Android intents and app links.
- **User actions:** every `onPress`, `onLongPress`, swipe, pull-to-refresh, form submit and toggle. Long-press and swipe actions are the ones most often missed.
- **State mutations:** every store action, reducer, mutation or repository method. **Any mutation with no UI calling it is either a dead feature or a missing flow**; report it (for example, a `snooze` action that no button calls).
- **Permissions:** every permission request, and where it is triggered.
- **Background work:** scheduled notifications, sync, exports, timers. Each must be observed at least once.
- **Platform capabilities:** saving to photos, picking images, sharing, haptics, speech, camera, files. Each native call must actually execute once; an SDK upgrade can turn a call into a runtime throw that typecheck never sees.

Write the inventory as a table in the report before walking anything.

## Step 2 — Walk each flow in every data state

For each flow, cover the states that change its behaviour:

| State | How to get there |
|---|---|
| Fresh install | Uninstall, reinstall, launch. First-run prompts and empty states. |
| One item | Singular copy ("1 item"), layouts with a single entry. |
| Typical data | Realistic content in the user's language, including diacritics and long words. |
| Many items | 20–50 items: lists, "+N more", scrolling, performance of background work. |
| Long content | A title of 100+ characters: wrapping, truncation, alignment of neighbours. |
| Longest generated label | A short label built from data (a weekday, a name, a count, a date, a translation) at its longest value, for example "Chủ nhật" rather than "Thứ 2". Find the possible values in the source and force the longest one. Check it on the narrowest supported screen (iPhone SE, 375 pt) and with the largest text size. |
| Completed / emptied | Finish everything in a list: what is left on screen, and what can be done next. |
| Missing target | Open a deep link or notification whose item was deleted. |
| Permission allowed and denied | Both branches; denied must offer a way to Settings. |
| Interrupted | Dismiss sheets by swipe, go back mid-form, background the app mid-action. |

## Step 3 — Walk the out-of-app surfaces

These are where "it works in the app" most often turns out to be false:

- **Notifications:** schedule one a minute or two ahead, leave the app, wait for the banner, tap it, and check the destination.
- **Widgets:** add every family through the real widget gallery. Check each family fills its tile (see common findings), tap every interactive element, then open the app and confirm the change was written back.
- **Intents and Shortcuts:** run each action from the Shortcuts app and confirm from the system log that `perform()` finished without error and returned the expected output.
- **Deep links:** open each scheme and path, including with the app cold, warm and already on that screen.
- **Saved media and files:** check the file actually exists afterwards (for example, the photo library database), with the expected size.

## Step 4 — Keep a coverage matrix

One row per flow, in the report:

| Flow | Entry | States walked | Result | Evidence |
|---|---|---|---|---|
| Capture → classify | Home input | fresh, typical, many, long | Complete | Screenshots s02–s05, DB rows |
| Tick from widget | Home-screen widget | typical | Complete after fix W2 | DB row status = done |

Use exactly one result per row: **Complete**, **Fixed** (it was not complete, now it is, with the fix ID), **Product decision** (completing it needs a business choice), or **Not testable here**.

The result judges the flow, not how the screens on its path look. Walking a flow is also a visual check of every screen and state it passes through: crop the controls the flow uses, and record any spacing, clipping or alignment defect as a UI finding with its own ID, even when the row says Complete. In the Evidence column, link the UI findings raised on that path, or write "no UI findings".

**"Not testable here" needs proof of an attempt.** Before using it, try the flow: add the widget, wait for the notification, run the Shortcut, seed the photo library, record the screen. Use the label only when the environment physically cannot do it (no microphone, no lock-screen editor in the simulator, no Android SDK), and write what was tried. Never list a flow as untested because it looked hard.

## Step 5 — Close the loop

- Fix every flow that is not complete, following phase 4 of the skill.
- Re-walk the fixed flows in the same states on the device.
- Re-read the inventory at the end: every row must have a result. A flow that was never walked is a gap in the audit, not a pass.
- In the final reply, state the coverage plainly: how many flows, how many complete, fixed, awaiting a decision, and not testable, with the reason for each of the last.
