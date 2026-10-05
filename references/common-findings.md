# Common findings

These issues keep recurring in mobile prototypes and fast builds. Check for them early; each lists the usual fix.

## Flows

- **Dead end after completion.** After an item is finished (task done, project complete, order placed), the main screen shows a celebration and nothing else. Add the next action, for example "Switch item" or "Start new".
- **Limit reached without explanation.** A free-tier or paywall limit silently opens the paywall. Say why ("You have 3 active projects on Free"), then offer the upgrade.
- **Floating action button covers content.** A floating "Add" button hides list items or collides with the tab bar. Move it into the section header or a toolbar, or reserve space for it.
- **Disabled submit without a reason.** Show a hint under the button listing what is missing ("Still needed: name, quantity").
- **Actions without context.** Sheets and pickers that change data should name their target ("Pick a yarn for *Summer Cardigan*") and say the consequence ("taken out of your stash").

- **Swipe-to-dismiss discards edits.** A sheet saves only through a button at the bottom, so swiping it down loses changes, even a whole new item. Save on dismiss (as Notes and Reminders do) when the draft changed and is valid, and put the primary action in the sheet header.
- **Completing removes the item with no way back.** Ticking a task makes it vanish instantly. Animate it out and offer "Undo" for a few seconds, or keep it visible, struck through, until the list is left.
- **A finished list becomes a dead end.** Everything is bought or done, and the card is a wall of struck-through lines with nothing to do. Show a completion message and a "Clear N done" action.
- **"Not found" is a blank screen.** A notification or deep link opens an item that was deleted, and the screen shows one line of text with no way out. Explain what happened and give a close or back button.
- **Context-blind "Add" buttons.** "+" on a calendar ignores the selected day, and "+" on a filtered list ignores the filter. Preset the new item from the context the user is looking at.
- **A mutation no screen calls.** The store has an action (snooze, archive, duplicate) that no button uses, so the feature silently does not exist. Wire it where users expect it, or remove it.
- **Permission asked at launch.** The system prompt appears before the user has done anything, so it is likely to be denied. Ask the first time the feature needs it, right after the action that explains why.
- **Batch items out of order.** Items created by one action get timestamps milliseconds apart, and "most recent first" shows them scrambled. Stamp one batch with one time.
- **Double-tap race on system pickers.** The image or document picker takes a second to appear; a second tap opens another picker and the first result is lost. Ignore taps while picking and show a spinner in the trigger.
- **Heavy background work on every change.** Re-rendering images, exports or widgets after every edit blocks the JS thread for seconds, and taps land late on the wrong control. Skip unchanged outputs, debounce, and read the latest state when a coalesced run starts.

## Forms and keyboard

- The keyboard covers the last fields and the submit button. On React Native, give the scroll view `automaticallyAdjustKeyboardInsets`, `keyboardShouldPersistTaps="handled"` and `keyboardDismissMode="on-drag"`. On Flutter, use `Scaffold` `resizeToAvoidBottomInset` and a scrollable body. On iOS native, use keyboard layout guides.
- Two forms in the same app with different headers, inputs, radii and buttons. Consolidate them into shared form primitives: label, input, chip and a submit button with a hint.
- The first field is not focused when the form opens, so the user has to tap before typing.

- **English autocorrect on another language.** The system keyboard "corrects" words typed without diacritics or in another language ("anh" → "Ang"), and can re-insert the old text right after a submit clears the field. Turn off `autoCorrect` and `spellCheck` on free-text capture fields when the content is not English, and clear the native input as well as the state.
- **The last word of a quick entry is dropped.** "Add line" reads component state on submit, which lags a keystroke behind. Read the text from the submit event.

## Visual consistency

- **One status, many colours.** "In progress" is green on one screen and pink on another, while green means done elsewhere. Give each semantic state one colour app-wide.
- **Non-functional chrome:** decorative icons in the app bar, and bookmark or "more" buttons with no handler. Remove them or make them work.
- **Duplicated badges:** a "PRO" ribbon plus "PRO" text plus a lock on the same tile. Keep one signal.
- **Headlines that wrap into fragments** ("My Stash · 8 yarns ·" / "1.2 kg"). Split them into a title and a meta line.
- **Fractional dashed borders** (1.5px) render inconsistently on iOS, and some dividers disappear. Use 1px for dashed borders.
- **Equivalent tiles with different vertical alignment.** One tile's value sits at the top and another's at the bottom. Top-align them all.
- **Arbitrary truncation** such as a fixed `maxWidth` on a name while space is left. Let the text flex and shrink.
- **Content flush against the header divider.** The screen shell adds no top padding under the app bar, so some screens patch their own gap and others (often forms and sheets) touch the line. Add the gap once in the shell and remove the patches.
- **Holes and empty bands in fixed-size containers** such as cards and home-screen widgets. Spacers push groups to the top and bottom, leaving a hole in the middle; or a small group is centred, leaving empty bands at the edges. Size the content (artwork, type, buttons) to fill the container and use fixed gaps between groups.
- **Chips truncated beside artwork** ("Section do…"). In tight containers, drop the chip's label or icon, or move it to its own row.
- **Emoji mixed into an icon set,** which breaks the icon style. Use the icon library.
- **Raster art with baked-in text:** labels or captions from a generated sheet still visible under an illustration. Clean the asset.

- **Content scrolls under the status bar.** Titles collide with the clock and Dynamic Island. Add a short scrim at the top of the shared screen shell.
- **Floating elements with magic insets.** The tab bar height, the FAB offset and the list's bottom padding are three separate numbers. Derive them from one measured tab-bar height plus the safe area, so the last row always clears the bar.
- **Transient banners inside the layout.** A toast or banner is inserted into the page, and when it disappears everything below jumps and taps land on the wrong item. Float it above the content.
- **Horizontal chip rows clipped at the page padding.** Let horizontal scrollers bleed to the screen edge while the first item lines up with the padding; one shared component, not three copies.
- **Fixed-height option panels.** A panel with a fixed `maxHeight` clips its last controls behind the CTA. Let it flex to the remaining space.
- **Sheets that add the status-bar inset.** An iOS page sheet already starts below the status bar; adding `insets.top` leaves an empty band.
- **Grid neighbours out of line.** One card's title wraps to two lines and its button drops below its neighbour's. Reserve the maximum number of lines.
- **Equal-width buttons with unequal labels.** Two or three buttons share a row with `flex: 1`, so each gets the same width whatever its label. The short one ("Today") looks roomy while the long one ("Next Friday") is cramped, its icon and text pushed into the padding, and a longer value ("Next Sunday") spills past the border. Shorten the label when the context already says the rest, let each button size to its content and share the leftover space, or stack the buttons. Also give pill labels one line and let them shrink, so no value can overflow.
- **Count badges that vanish on their background.** A badge in the same tint as the page (for example peach on peach) disappears. Give it contrast.
- **Emoji that read as data.** 📅 renders as a calendar showing a real date ("JUL 17"), which looks like a wrong due date. Prefer the icon set, or at least an emoji without text.
- **Native controls restyled by a new OS.** iOS 26+ wraps default buttons in glass capsules and redraws switches and sliders; rows designed for the old size overflow. Re-check every native control after an OS or SDK upgrade, and set plain button styles where a bare icon is intended.

## Home-screen and lock-screen widgets

- **Empty bands around widget content.** The root stack has no frame, so it is sized to its content and centred in the tile, and a `Spacer` inside it does nothing. Give the root `frame(maxWidth: .infinity, maxHeight: .infinity, alignment: .topLeading)` (or the framework's equivalent) in every family.
- **A tile with too little to show.** The large family shows three rows and is half empty on a light day. Fill it with the next most useful data (upcoming items, totals) rather than stretching the spacing.
- **Overflow in the medium family.** Around 130 pt of height fits a header and about four rows; a separate footer button gets clipped. Put secondary actions in the header row.
- **Interactive rows that never reach the app.** A tick on the widget updates the widget but not the app's data. Write the change back and verify it after the next foreground.
- **Checking only one family.** Open the widget gallery and inspect every family at full resolution, with real data, after the app has pushed its latest layout.

## Copy

- Pluralisation: "×1 skeins", "1 entries".
- Zero or empty values shown as data: "$0.00 /ea", "0 min". Hide them or show a placeholder.
- Jargon or misleading labels (a domain term used wrongly), and static labels that carry no information ("Sorted by date" when there is no sort control). Replace them with useful meta such as counts.
- Two captions for one control (a label inside the button and another under it).
- The same statistic named differently on different screens ("Hours knit" versus "Time knitted").

- **Rounded-down money.** 5,450,000 shown as "5.4M", 1,500 as "2k". Keep enough digits that a total never looks smaller than it is, and add unit tests for the formatter.
- **The amount repeated.** "Internet bill 250k · 250k" when the title already contains the number. Append the amount only when the title has none.
- **Stale "now".** A "now" line or relative time is computed once at render and goes stale while the screen stays open. Refresh it on a timer or on focus.
- **Natural-language parsing false positives.** Date words inside other phrases ("this year" containing "this", "năm nay" containing "nay") become due dates, and topic words ("call the teacher about the class fund") pick the wrong category. Test the parser with realistic sentences and add each false positive as a test.

## Motion

- Switching tabs, segments or sections swaps a large number or content block instantly. Fade or slide the new content in (about 200ms).
- An inline expansion (stepper, details) pops in. Fade in plus a layout transition.
- A newly added list item appears with no entrance. Fade in and move up (about 250ms).
- Celebrations: exactly one success haptic, and a dismissal that returns the user to a screen with a next action.
- A card or row that disappears after an answer pulls the content below up with a jump. Fade it out and animate the layout of the siblings.
- Verify motion from a screen recording (for example, 20 fps frame strips), not from before-and-after screenshots.

## Runtime and platform

- **Deprecated APIs that throw at runtime.** An SDK upgrade turns an old call (for example, a media-library save) into an exception that typecheck does not catch. Run every native capability once on the device and read the logs.
- **Development-only duplicates.** Fast Refresh re-evaluates modules and can replay a deep-link handler, creating duplicate data while testing. Keep "already handled" guards outside the module (for example on `globalThis`), and tell dev artefacts apart from real bugs by relaunching cleanly before concluding.
