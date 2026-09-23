# Motion audit checklist

Animation is not decoration. It communicates hierarchy, spatial relationships, state changes, feedback, navigation, continuity, and cause and effect. The goal is to **make the interface communicate through motion**, not to make the app animated.

## First principle

Before adding any animation, ask what information it communicates. If the answer is "it looks cool", don't add it. Good motion answers these questions:

- What happened? What changed?
- Where did this come from, and where did it go?
- What should I look at?
- Did my action succeed?

Don't wait to be told where motion belongs. During the walkthrough, flag moments like these:

- "This interaction feels abrupt."
- "This state change is instant and lacks feedback."
- "This transition loses spatial context."
- "This would benefit from a subtle spring."
- "This list needs a controlled insertion or removal."

## Opportunities

1. **Screen transitions.** Pick push, slide, fade, scale, a shared element or a modal by the relationship between the screens. List → detail should say "this item opened into a deeper view". Home → settings can use the standard transition. Don't use one transition for every relationship.
2. **Element entrance.** Cards, list items, hero content, empty states, sheets, dialogs, search results, newly loaded content. Use fade, fade plus translate, scale, stagger or spring. Prefer subtle coordinated motion (a light stagger that says "the list arrived") over animating every element independently.
3. **User feedback.** Button press (subtle scale or spring), toggles, checkboxes, favourites, add to cart, copy, save, delete. The user should feel "the app received my action".
4. **State transitions.** Loading → content, empty → content, disabled → enabled, unselected → selected, error → valid, collapsed → expanded, before → after. Don't swap the UI instantly when a transition would explain the change.
5. **Lists.** Insertion, deletion, reordering, filtering, sorting, pagination. A short exit says "this was removed". Avoid heavy stagger on long lists.
6. **Bottom sheets.** Entrance, backdrop, drag, snap points, dismissal, content transition, keyboard. The sheet should feel physically attached to the bottom edge, not fade in.
7. **Modals and dialogs.** A backdrop fade plus scale, slide or spring, meaning "temporarily above the current context". Dismissal reverses the presentation.
8. **Collapse and expand.** Accordions, expandable cards, FAQs, filters, dropdowns, advanced settings, "show more". Don't change height instantly.
9. **Scroll-based motion**, used sparingly: collapsing or sticky headers, parallax, FAB behaviour, toolbar appearance. It must improve hierarchy.
10. **Hero and detail transitions.** When an element exists on both screens (product image, avatar, card), consider a shared-element or matched-geometry transition, so the user feels "I opened this thing", not "a random new screen appeared".
11. **Micro-interactions.** Icon transformations, bookmarks, sliders, progress, pull-to-refresh, swipe actions, badges. Only where motion reinforces the action.
12. **Success and completion.** A checkmark, progress completion or a subtle celebration. Confetti only when the product context truly celebrates.
13. **Loading.** No indefinite spinners, blank screens, abrupt replacement, flashing skeletons or skeletons that don't match the layout. Prefer progressive reveal.
14. **Errors.** A subtle transition into the error state (normal → border change → message). No aggressive shaking by default.
15. **Gesture-driven UI.** Sheets, swipe-to-delete, carousels, pagers, draggable cards and sliders follow the finger directly, with no delayed animation after the gesture.

## Physics, timing, easing

- **Springs** suit sheets, draggables, toggles, cards, buttons and bounce-back. Not everything needs to feel physical.
- **Starting durations:**

| Kind of motion | Duration |
|---|---|
| Micro interaction | 100–200ms |
| Small state change | 150–250ms |
| Standard transition | 200–350ms |
| Large transition | 300–500ms |

  Tune by importance and distance; don't apply the numbers blindly. Tune springs by stiffness, damping and velocity.
- **Easing:** entering uses ease-out (deceleration); exiting uses ease-in (acceleration); moving between states uses ease-in-out or a spring; gestures follow the finger.
- **Motion hierarchy:** the primary CTA gets subtle but clear feedback, secondary buttons minimal feedback, and background decoration little or no motion. Decoration must never compete with important UI.
- **Reduced motion:** respect the OS setting. Minimise movement, prefer opacity or state changes, avoid parallax and aggressive springs.
- **Consistency:** once a pattern exists, reuse it. All equivalent primary buttons share one press interaction, and cards share one entrance. Build reusable primitives (FadeIn, SlideIn, ScaleIn, pressable feedback, spring transition, sheet transition, list-item transition) with the project's existing animation library. Add a new library only for a strong technical reason.

## Warning signs of over-animation

- Every screen fades, every component slides, every button bounces, every icon spins.
- Large staggers on every list, or excessive parallax.
- Long transitions, or animation that blocks interaction or delays the user.

If removing an animation makes the UI clearer, remove it.

## Evaluate on the device

Screenshots are not enough. For each important animation:

1. Perform it on the simulator at normal speed.
2. Judge whether it is too slow or too fast.
3. Check that it explains the state change.
4. Check that it doesn't block interaction.
5. Look for jitter and layout jumps.
6. Compare it with the other screens.

Capture before and after screenshots, and use screen recording when available.

For each animation, check purpose, timing, easing, direction, continuity, hierarchy, consistency, performance, accessibility (reduced motion) and interruption (can the user keep interacting?).

## Priority

| Priority | Meaning | Example |
|---|---|---|
| P0 | Missing motion causes misunderstanding | A destructive action changes the UI instantly with no indication |
| P1 | Motion significantly improves comprehension | The list → detail relationship |
| P2 | Motion improves perceived quality | Button press feedback |
| P3 | Pure polish | A subtle icon transformation |

Fix P0 and P1 before P2 and P3.

## Final motion pass

Walk the app once more, looking only for abrupt transitions, dead-feeling interactions, missing feedback, and animation that is inconsistent, excessive, slow, distracting, blocking, jumpy or unlike the platform. The app should feel responsive, coherent, fluid, intentional and premium, and never busy, slow, gimmicky or over-animated.
