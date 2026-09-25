# Device QA playbook

The audit is done on a running app, not from source code. Screens must be seen, tapped, typed into and navigated.

## iOS simulator (Expo / React Native, SwiftUI, Flutter)

- Use `mcp__Claude_Code_iOS_Simulator__control` when it is available. Call `attach` first so the user can watch.
- The main actions are `screenshot`, `tap`, `swipe`, `text` and `open_url`. Coordinates are in device points; take a screenshot to locate targets before tapping.
- Pass `scale: 0.5` for overview screenshots and full scale for detail. **Never judge spacing, alignment or clipping from a downscaled screenshot:** a missing 16 pt gap, a 1 pt misalignment or a truncated label disappears below about 50 % scale.
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
- **Full-resolution crops:** for each screen, save a full-size screenshot (`xcrun simctl io booted screenshot file.png` or `adb exec-out screencap -p > file.png`) and crop the header, the first content, the edges of cards or widgets and the bottom area at 100 %. Review spacing and alignment only from these crops.
- **Keyboard:** focus the **last** input of every form, then check that it and the submit button stay visible.
- **Content length:** try long names, empty lists and one item versus many (for pluralisation).
- **Forms:** try an empty submit, invalid input, a valid submit, and what happens next (the destination and the feedback).
- **Completion and edge states:** finish an item, empty a list, hit a limit (paywall or free tier), and check that there is always a next action.
- **Motion:** perform the interaction and judge it live. A screenshot sequence alone cannot judge timing.
- **Processes:** reuse the running dev server and stop what you started when the work ends.
- **Quality gates:** run typecheck, lint and tests at the start (the baseline) and at the end. If lint is not configured, say so instead of claiming it passed.

## Seeding data and checking persistence

- **Seed realistic data through the app's own entry points,** such as a deep link (`<scheme>://?text=...`, URL-encoded), rather than typing. It is fast, and it can carry diacritics and symbols that the simulator's text tool cannot type (it also cannot send Backspace).
- **Verify persistence in storage, not on screen.** On iOS the app's data lives under `xcrun simctl get_app_container <udid> <bundle> data` (for example `Documents/SQLite/*.db` for expo-sqlite, readable with `sqlite3`). On Android use `adb shell run-as <package>`. Back up the database before editing it.
- **Separate dev artefacts from bugs.** When data looks duplicated, compare timestamps with the Metro log (reloads, Fast Refresh) and repeat after a clean relaunch before calling it a bug.

## Out-of-app surfaces on the iOS simulator

- **Notifications:** create an item due two or three minutes ahead, press HOME, wait with a condition-based poll (not a long fixed sleep), then tap the banner and check the destination. `xcrun simctl privacy <udid> grant|revoke|reset notifications <bundle>` controls the permission.
- **Home-screen widgets:**
  1. Long-press empty home-screen space.
  2. Tap **Edit** at the top left, then **Add Widget**.
  3. Search for or scroll to the app, then swipe through every family.
  4. Add one and tap its interactive elements.
  5. Relaunch the app after changing widget code so it pushes the new layout.
  6. Open the app and verify the write-back in storage.

  The long-press menu closes by itself after a few seconds, so tap straight away.
- **App Intents / Shortcuts:** open the Shortcuts app, go to the **Apps** section, pick the app and tap an action to run it. Confirm with `xcrun simctl spawn booted log show --last 1m --predicate 'eventMessage CONTAINS "<IntentName>"'`: look for `perform() finished` and the output item.
- **Photo library:** seed with `xcrun simctl addmedia <udid> <file>`. After a save, confirm the file in `data/Media/PhotoData/Photos.sqlite` (the `ZASSET` table has the size and date).
- **Lock screen:** `button LOCK` twice shows it. The simulator cannot open the lock-screen customiser, so lock-screen widgets and wallpaper steps need a real device; say so, and say what you tried.
- **Keyboard:** the simulator driver may keep a hardware keyboard attached even when `ConnectHardwareKeyboard` is off, so the on-screen keyboard never appears. If so, report keyboard overlap as not verified, and restore any setting you changed.

## Recording motion

`xcrun simctl io <udid> recordVideo --codec h264 -f motion.mp4 &`, perform the interaction, stop it with `SIGINT`, then turn the last seconds into a frame strip: `ffmpeg -sseof -3 -i motion.mp4 -vf "fps=20,scale=300:-1,tile=8x3" -frames:v 1 strip.png`. Count frames to judge duration and look for jumps.

## Dev-server gotchas

- After a JS error, Fast Refresh can keep running the old module. Terminate and relaunch the app before re-testing a fix.
- Formatters with default settings (for example, Prettier without the project's config) rewrite quotes and line width. Check for a config, or pass the project's style explicitly.
