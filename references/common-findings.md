# Common findings

These issues keep recurring in mobile prototypes and fast builds. Check for them early; each lists the usual fix.

## Flows

- **Dead end after completion.** After an item is finished (task done, project complete, order placed), the main screen shows a celebration and nothing else. Add the next action, for example "Switch item" or "Start new".
- **Limit reached without explanation.** A free-tier or paywall limit silently opens the paywall. Say why ("You have 3 active projects on Free"), then offer the upgrade.
- **Floating action button covers content.** A floating "Add" button hides list items or collides with the tab bar. Move it into the section header or a toolbar, or reserve space for it.
- **Disabled submit without a reason.** Show a hint under the button listing what is missing ("Still needed: name, quantity").
- **Actions without context.** Sheets and pickers that change data should name their target ("Pick a yarn for *Summer Cardigan*") and say the consequence ("taken out of your stash").

## Forms and keyboard

- The keyboard covers the last fields and the submit button. On React Native, give the scroll view `automaticallyAdjustKeyboardInsets`, `keyboardShouldPersistTaps="handled"` and `keyboardDismissMode="on-drag"`. On Flutter, use `Scaffold` `resizeToAvoidBottomInset` and a scrollable body. On iOS native, use keyboard layout guides.
- Two forms in the same app with different headers, inputs, radii and buttons. Consolidate them into shared form primitives: label, input, chip and a submit button with a hint.
- The first field is not focused when the form opens, so the user has to tap before typing.

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

## Copy

- Pluralisation: "×1 skeins", "1 entries".
- Zero or empty values shown as data: "$0.00 /ea", "0 min". Hide them or show a placeholder.
- Jargon or misleading labels (a domain term used wrongly), and static labels that carry no information ("Sorted by date" when there is no sort control). Replace them with useful meta such as counts.
- Two captions for one control (a label inside the button and another under it).
- The same statistic named differently on different screens ("Hours knit" versus "Time knitted").

## Motion

- Switching tabs, segments or sections swaps a large number or content block instantly. Fade or slide the new content in (about 200ms).
- An inline expansion (stepper, details) pops in. Fade in plus a layout transition.
- A newly added list item appears with no entrance. Fade in and move up (about 250ms).
- Celebrations: exactly one success haptic, and a dismissal that returns the user to a screen with a next action.
