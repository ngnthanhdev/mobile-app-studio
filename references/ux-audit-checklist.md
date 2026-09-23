# UX audit checklist

## §1 Understand the product

- What is the app? Who is the target user? What is the primary user goal?
- What are the core features and the primary navigation structure?
- What are the important user journeys?
- Which screens are core and which are secondary?
- Which actions do users perform most frequently?

## §2 Act like a real user and test journeys

For each important flow:

1. Start from the entry point and pretend you have never seen the app.
2. Work out what to do without developer knowledge, tapping through naturally.
3. Complete the main task.
4. Try back navigation and edge cases.
5. Observe the loading, empty and error states, the keyboard, scrolling, modals and bottom sheets, and screen transitions.

Journeys to walk (adapt to the app):

- onboarding → home → feature → detail → action → confirmation → result;
- home → tab → list → detail → back;
- home → create → form → validation → submit → success;
- any other major flow you discover.

For every flow ask:

- Is the next step obvious? Is the primary action obvious?
- Is the navigation predictable? Is there unnecessary friction?
- Does the app make me think too much? Can I accidentally lose progress?
- Does the UI say what happened? Does back behave as expected?
- Is the destination obvious after an action?
- Are there unnecessary screens or redundant actions? Is information in the right order?

## §3 Consistency

**Typography.** Check family, size, weight, line height, letter spacing, capitalisation, heading and body hierarchy, button text, tab labels, placeholders and secondary text. Look for:

- equivalent titles at different sizes;
- similar buttons with different type;
- text that sits vertically off;
- inconsistent line heights;
- text too close to icons or screen edges;
- an unclear hierarchy.

Define one hierarchy adapted to the product: display, heading, subheading, body, secondary, caption, button, tab.

**Spacing.** Check screen padding, section gaps, card padding, icon-to-text, title-to-subtitle, buttons, list items, inputs, top and bottom spacing, and modal padding. Replace accidental values (12 here, 13, 17 or 23 elsewhere) with a coherent scale and tokens.

**Alignment.** Check tabs, headers, titles, cards, buttons, inputs, icons, avatars, lists and horizontal padding.

- **Bottom tab bar:** equal tab widths and spacing, centred icons and labels, vertical position, safe area, active and inactive states, the icon–label relationship, top and bottom padding, height, and consistency across screens. If it looks even slightly off-centre, fix it.
- Never fix alignment with arbitrary pixel offsets without a real reason.

**Components.** Buttons, cards, inputs, dropdowns, chips, tabs, headers, bottom sheets, modals, list items, avatars, badges, empty states and loading states. If they represent the same concept, do they look and behave the same? If not, pick the canonical version and refactor the duplicates. Prefer reusable components over screen-specific hacks.

**Design system.** Colours, typography, spacing, radii, shadows, borders, icon sizes, component sizes, elevation, opacity and animation timing. If a system exists, **use it** and introduce no random values. If none exists, infer one from the UI and consolidate gradually. No massive architectural rewrite.

## §4 Visual hierarchy

For every screen: what is the most important thing, and is it what the eye notices first? Check the primary CTA, the page title, key information, secondary actions, supporting information and navigation. Look for:

- overly dominant elements;
- weak primary actions;
- visual noise;
- excessive cards, borders, colours or icons;
- competing CTAs.

A screen should not give everything equal importance.

## §5 UX friction

Look for:

- unnecessary taps and redundant confirmations;
- confusing terminology and hidden actions;
- unclear CTAs, unclear or unexpected navigation, inconsistent back behaviour;
- excessive scrolling and forms that ask too much;
- poor keyboard handling;
- unclear validation, success or error feedback;
- dead ends, and screens with no obvious next action.

For each, ask: can this be simpler without losing clarity?

## §6 Mobile-specific and platform conventions

- **Touch targets:** comfortably tappable, about 44pt on iOS or 48dp on Android.
- **Safe areas:** notch, status bar, home indicator, bottom navigation, keyboard.
- **Keyboard:** inputs are not hidden, the CTA is not covered, scrolling is sensible, dismissing feels natural.
- **Gestures:** swipe, scrolling, bottom sheets, horizontal lists, pull-to-refresh, back gesture.
- **Device sizes:** small and large phones, dynamic text, long titles, long translations.
- **Platform conventions:** navigation, back, sheets, alerts, keyboard, tab bars, status bar, touch feedback, scrolling and loading indicators should feel native to iOS and Android. No web patterns without a deliberate product reason.

## §7 Content and microcopy

Fix confusing wording, inconsistent terminology, overly technical language, inconsistent capitalisation, unclear CTA labels, awkward error messages, redundant text and long labels. Prefer concise, action-oriented copy: "Continue", not "Click here in order to continue to the next step". Don't rewrite copy that already works.

## §8 States, accessibility, perceived performance

**States.** A missing state is a real UX bug.

- Components: default, pressed, focused, disabled, loading, success, error, empty, selected, unselected.
- Lists: loading, populated, empty, error, refreshing, loading more.
- Forms: empty, focused, filled, invalid, valid, submitting, submitted.

**Accessibility.** Contrast, touch target size, readable text, text scaling, semantic labels, icon-only buttons (they need labels), focus states, screen-reader order. Optimise for real users, not screenshots.

**Perceived performance.** Unnecessary loading screens, flashing UI, layout jumps, content shifting, delayed feedback, animations that feel too slow or too abrupt, skeletons that don't match the content, inconsistent navigation transitions.

## §9 Final quality bar

- **Visual:** consistent typography, spacing, alignment and components; balanced hierarchy; polished navigation and bottom tabs; no obvious visual defects.
- **UX:** clear navigation and CTAs; predictable interactions; minimal friction; sensible flows; clear feedback; good empty, loading and error states.
- **Engineering:** no unnecessary hacks; reusable components; the existing architecture respected; business logic preserved; no obvious regressions; a clean implementation.
- **Principle:** a real person opening the app for the first time immediately understands how to use it, and the whole product feels coherent, intentional and professionally designed.
