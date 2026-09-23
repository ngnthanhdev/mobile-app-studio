# Device QA playbook

The audit is done on a running app, not from source code. Screens must be seen, tapped, typed into and navigated.

## iOS simulator (Expo / React Native, SwiftUI, Flutter)

- Use `mcp__Claude_Code_iOS_Simulator__control` when it is available. Call `attach` first so the user can watch.
- The main actions are `screenshot`, `tap`, `swipe`, `text` and `open_url`. Coordinates are in device points; take a screenshot to locate targets before tapping.
- Pass `scale: 0.5` for overview screenshots and full scale for detail.
- `inspect` gives the accessibility tree when it is available. Use it to check labels, but it is not always available.
- **Fresh first-launch state:** `xcrun simctl uninstall <udid> <bundle>` and then `install <app>`. Tell the user if their test data is affected.
- **Deep links** open any route directly (`<scheme>://path`). Use them to reach modals and hidden screens fast.
- **Expo dev client:**
  - Launch with `xcrun simctl openurl <udid> '<scheme>://expo-development-client/?url=http%3A%2F%2Flocalhost%3A8081'`.
  - Metro must run **without `CI=1`**, otherwise it ignores file edits.
  - After replacing image assets, terminate and relaunch the app.
- **Native build problems:** CocoaPods may need `LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8`.

## Android emulator

- `adb devices`, `adb shell input tap x y`, `adb shell input swipe x1 y1 x2 y2 ms`, `adb shell input text '...'`, `adb exec-out screencap -p > shot.png`.
- `adb shell am start -W -a android.intent.action.VIEW -d '<scheme>://path'` opens deep links.
- Check the system back button and the back gesture on every screen.

## Web previews of mobile layouts

Use the browser pane's `resize_window` with the mobile preset (375×812) only when there is no native build. Say that native behaviour (keyboard, haptics, safe areas) was not verified.

## Verification habits

- Photograph every screen and state you audit, and again after the fix. Keep the pairs in the report folder if the project stores verification images.
- **Keyboard:** focus the **last** input of every form, then check that it and the submit button stay visible.
- **Content length:** try long names, empty lists and one item versus many (for pluralisation).
- **Forms:** try an empty submit, invalid input, a valid submit, and what happens next (the destination and the feedback).
- **Completion and edge states:** finish an item, empty a list, hit a limit (paywall or free tier), and check that there is always a next action.
- **Motion:** perform the interaction and judge it live. A screenshot sequence alone cannot judge timing.
- **Processes:** reuse the running dev server and stop what you started when the work ends.
- **Quality gates:** run typecheck, lint and tests at the start (the baseline) and at the end. If lint is not configured, say so instead of claiming it passed.
