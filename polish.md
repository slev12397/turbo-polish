# Polish rules

Small, checkable rules that make an interface feel finished. Each rule has an ID, the rule, the values, the code, and how to check it. Use them two ways:

- **Building:** apply every rule that touches what you're making.
- **Reviewing:** walk the review pass at the end, cite rule IDs in findings (`P-MOT-03 at src/menu.tsx:40`).

These sit under `taste.md`, `design-system.md` and `motion.md`. Where common advice differs, the rule says which value Turbo uses and why.


## Principles

- **P-PRI-01 Performance is design.** Perceived speed is a design decision. Shorten before you decorate; show the result before the work finishes where you can (optimistic UI, skeletons in the real shape).
- **P-PRI-02 Novelty budget.** Delight is spent, not sprinkled. One memorable moment per flow at most, placed where it's rare (first success, setup done). Everything used daily stays plain.
- **P-PRI-03 Structure outlasts surface.** Hierarchy, spacing and alignment age well; effects date fast. When choosing between a structural fix and a visual one, take the structural one.
- **P-PRI-04 Real references.** Work from real products and real content, not from memory of them. Screenshot, measure, then borrow the principle.
- **P-PRI-05 Taste is trained.** Look at the work slowed down, zoomed in, the next day. Write down every nudge you keep so it becomes a rule here.

## Typography

**P-TYPE-01 Tabular numbers.** Digits that change (timers, counters, prices, live metrics) or stack in a column use equal-width figures. Numbers inside a sentence keep proportional figures.
- Right-align numeric columns so different lengths share an edge.
- Don't switch to a monospace font to get stable digits; it changes the voice of the whole UI.
- If `tabular-nums` changes nothing, the font lacks `tnum`. Check the font's OpenType features.
- Inter redraws some numerals with `tnum` on. Expected.
```html
<span class="tabular-nums">12:45</span>
<td class="text-right tabular-nums">12,441</td>
```
Check: run the timer or counter; nothing beside it moves.

**P-TYPE-02 Font smoothing.** Set grayscale antialiasing once, on the body. macOS otherwise thickens light text on dark surfaces.
```html
<body class="antialiased">
```
- macOS only; Windows, Linux and mobile ignore it.
- Body text at weight 300 or lighter on dark: check on a 1x display first, it can get too thin.
- Opt a region out with `subpixel-antialiased`.
Check: dark theme headings match the design file's weight on a Mac.

**P-TYPE-03 Text wrapping.** `text-wrap: balance` on headings, `text-wrap: pretty` on paragraphs, set globally. No lonely last word on a title.

**P-TYPE-04 Letter spacing by size.** Large text tightens, small text opens slightly. Defaults: headings at the heading tracking (-0.3px), body at 0, 12px and below at +0.01em at most. Set in the text styles, never at the call site.

**P-TYPE-05 Optical sizes.** Leave `font-optical-sizing: auto` on (the browser default). Fonts with an `opsz` axis (Inter, for one) then draw the display design at large sizes and the text design at small ones. Never pin a text cut onto a display size.

**P-TYPE-06 Hanging punctuation.** A quote that opens with a quotation mark hangs the mark into the margin, so the first letter sits on the text edge.
```html
<blockquote class="indent-[-0.42em]">“Less, but done well.”</blockquote>
```
Safari supports `hanging-punctuation: first`; the negative indent covers the rest for a known opening character.

**P-TYPE-07 Line-height by role.** Unitless, so it scales with size. Display 1.1, headings 1.2 to 1.3, body 1.5 to 1.6. Anything that can wrap to three lines or more gets at least 1.4, even in a tight card. Pair each size with its line-height and weight in the text style, so a role is one decision.

**P-TYPE-08 Weight floors.** Below 18px, weight 400 or heavier. Weights 100 to 300 only at 28px and up, and checked on both themes there. Emphasis inside a role is one weight step up (400 to 500), not a size change.

**P-TYPE-09 Size floors and units.** Long-form reading text starts at 16px. UI text can go smaller: 14px for body, inputs and menus, 13px for metadata, rarely under 12px. Sizes in `rem`, never px, so the reader's own font size still applies.

**P-TYPE-10 Headings descend with level.** h1 to h3 map to descending steps of the scale. A sub-heading never outweighs its parent, and a heading is never smaller than body text.

**P-TYPE-11 Cap the measure.** Long-form text runs 60 to 75 characters a line (`max-width: 65ch` is a good default). UI text stays `text-align: start`; never justify in an app.

**P-TYPE-12 Wrap and break on purpose.** `balance` on headings and short lines only (never paragraphs; Chromium ignores it past six lines), `pretty` on paragraphs. `overflow-wrap: break-word` wherever a long URL, ID or name could escape its box. `white-space: nowrap` on badges and short labels where a break looks broken.

**P-TYPE-13 Truncate with a way back.** One line: `truncate`. Several: `line-clamp-N` (it needs `display: -webkit-box`, which the utility sets). Any cut value the user needs has a way to the whole: a tooltip that also opens on keyboard focus, an expand control or a detail view.

**P-TYPE-14 Load what you use; properties over tags.** Every weight and style the design uses is loaded, so the browser never fakes a bold or italic. Use CSS properties (`font-weight: 650`, `font-variant-numeric: tabular-nums`) over `font-variation-settings` and `font-feature-settings`; keep raw tags for custom axes and stylistic sets only. Never `font-kerning: none`.

**P-TYPE-15 Typographic punctuation.** Rendered strings use curly quotes, the single ellipsis character (…), en dashes for ranges (2–4 days) and a non-breaking space inside a value and its unit (10&nbsp;MB). Code keeps straight quotes. Store copy in natural case and set case with CSS.

**P-TYPE-16 Underlines from the font.** Links take `text-underline-position: from-font` and `text-decoration-thickness: from-font`, or a set offset and thickness. Only the underline's colour animates. A dotted underline marks a term with more behind it.

**P-TYPE-17 Inputs at 16px on mobile.** iOS Safari zooms the page when an input's text is under 16px. Turbo default: 16px inputs below the `sm` breakpoint, the design size above (`text-base sm:text-sm`). Never fix it by disabling zoom in the viewport meta.

**P-TYPE-18 Trim text in tight boxes.** Buttons, badges and chips use `text-box: trim-both cap alphabetic` (progressive enhancement) so the label sits optically centred instead of low.

**P-TYPE-19 Language and direction.** Set `lang` on `<html>` (quotes, hyphenation, pronunciation) and `dir` wherever direction changes. Wrap a user-supplied name, number or URL inside a sentence in `<bdi>`.

**P-TYPE-20 Text stays selectable.** `user-select: none` only on a drag or gesture surface, never on a layout root or a text block. `::selection` may carry the brand if the pair stays legible.

## Icons and alignment

**P-ALN-01 Optical alignment.** Centre for the eye, not the box. Shapes carry weight off-centre: a play triangle sits left in its box, so it moves right; stars and arrows often need a pixel up or down. This is per icon, never a global offset.
- Test: put a heavy `filter: blur()` on the icon in the inspector. The blob shows where the weight is; nudge it back to centre.
- Prefer fixing it inside the SVG. Otherwise a `translate-x-px` style nudge on the icon.
```html
<button class="grid size-12 place-items-center rounded-full">
  <PlayIcon class="size-5 translate-x-px" />
</button>
```

**P-ALN-02 Icon-side padding.** An icon carries empty space inside its box, which adds to the padding beside it. A button with a leading icon takes a few pixels less padding on the icon side (default: 2px less, so `pl-2.5 pr-3` on a 12px button). Bake it into the button variant, not the call site.

**P-ALN-03 Shape weight.** A circle or triangle in the same box as a square looks smaller. Size marks by how big they look, not by their box: circles and triangles get a slightly larger box than squares in the same row.

**P-ALN-04 One icon set, one weight, sized to the text.** One set, one stroke style. Stroke weight follows icon size (see `design-system.md`), and an icon beside text matches the text's optical weight: a 16px icon with 14px text. Never mix outline and filled icons in one row.

**P-ALN-05 One SVG, recoloured per state.** Icons use `currentColor`; state changes the colour of the parent, never swaps the file.

## Colour and surfaces

**P-COL-01 OKLCH.** Define colour tokens in OKLCH. Equal L reads as equal brightness across hues, so a set of tags, tones or chart series with one L and different hues never has one heavier than the rest. Adjust chroma per hue if one looks loud.

**P-COL-02 Shadows, not borders, for edges.** A surface's edge is a transparent hairline inside a layered shadow, not a solid border. Transparent edges adapt to any background and to images. Turbo values are the role tokens (`shadow-sheet`, `shadow-outline`); Jakub's three-layer stack for reference:
```css
box-shadow:
  0 0 0 1px rgb(0 0 0 / 0.06),
  0 1px 2px -1px rgb(0 0 0 / 0.06),
  0 2px 4px 0 rgb(0 0 0 / 0.04);
```
Animate between states with `transition-property: box-shadow`, never `all`.

**P-COL-03 Image outlines.** Every image, avatar and logo tile gets a 1px line drawn inside its edge, so a pale image doesn't melt into a white page. Not a `border` (it changes the box).
- Turbo token: `media-edge` (ink at 20%) and `media-edge-light` (white at 20%). 10% also works; the useful range is 5% (vanishes) to 20% (reads as a frame). Pale avatars and logos are where it matters most.
```html
<img class="rounded-lg outline-1 -outline-offset-1 outline-black/10" />
```
Theme the colour through a token, never `dark:`.

**P-COL-04 Noise.** Grain only where the client file allows it: a large flat colour field or a gradient that bands.
- Values: `feTurbulence` `fractalNoise`, `baseFrequency 0.8`, `numOctaves 3`, `stitchTiles stitch`, desaturated (`feColorMatrix saturate 0`), overlay at 8% opacity, `mix-blend-mode: overlay`.
- The parent needs `isolation: isolate`, or the grain blends with whatever is behind it.
- Small cards: live SVG filter. Large, scrolling or animated surfaces: a 200px tile as a data URI (or a 2x PNG if seams show); the live filter repaints every frame.
- The overlay is `aria-hidden` and `pointer-events: none`.
- Never on surfaces that need exact colour, or behind body text.

**P-COL-05 HTML background.** Paint the root, not only the app wrapper, or overscroll in Safari shows a white strip behind a dark app.
```html
<html class="bg-background">
```
- Same token as the app's page, so it follows the theme.
- Keep `<meta name="theme-color">` in sync with the theme.
- Don't kill the overscroll bounce (`overscroll-behavior: none`) except on a surface that truly must not chain-scroll (a modal sheet).
Check: dark theme, Safari, pull past the top and bottom.

## Layout

**P-LAY-01 Nested radius.** Outer radius = inner radius + inset. The inset is the padding plus any border width. Starting from the outer radius: inner = outer − inset, never below 0.
```css
.inner { border-radius: max(0px, calc(var(--outer) - var(--inset))); }
```
- When the inset is over about 24px, the layers are separate surfaces; pick each radius on its own.
- Tune by eye where the inner element doesn't reach the corner or the shapes differ.
Check: no thick rim at the corners of any nested pair.

**P-LAY-02 Hit areas.** The clickable target is bigger than the visible control: at least 40px, 44px on touch, via an invisible pseudo-element. Neighbouring targets never overlap.
```html
<button class="relative size-8 after:absolute after:-inset-1">…</button>
```

**P-LAY-03 Scroll fades.** A scroll container with a hard edge reads as a list that ends. Fade the edge where there's more, and only that edge: never fade the first row at the top or the last row at the bottom.
- Use a `mask-image`, not an overlay: it works on any background and never blocks clicks.
- Fade size 3rem (48px) vertical, 2rem horizontal. Each side grows in over the first fade-length of scroll and out over the last.
- Horizontal rows of chips or tabs fade the same way and hide their scrollbar.
- The mask also fades a visible scrollbar. Hide the scrollbar, or put the mask on an inner wrapper and scroll the outer one.
```css
@property --fade-start { syntax: "<length>"; inherits: false; initial-value: 0px; }
@property --fade-end   { syntax: "<length>"; inherits: false; initial-value: 0px; }

.scroll-fade-y {
  mask-image: linear-gradient(to bottom,
    transparent, #000 var(--fade-start),
    #000 calc(100% - var(--fade-end)), transparent);
  animation: fade-start linear both, fade-end linear both;
  animation-timeline: scroll(self), scroll(self);
  animation-range: 0 3rem, calc(100% - 3rem) 100%;
}
@keyframes fade-start { from { --fade-start: 0px } to { --fade-start: 3rem } }
@keyframes fade-end   { from { --fade-end: 3rem }  to { --fade-end: 0px } }
/* scroll-fade-x: the same with `to right`, scroll(self inline) and 2rem */
```
- Where scroll-driven animations aren't supported (Firefox), the shared scroll-fade component sets the two properties from scroll position in JS.
Check: at the top, the first row is crisp; at the bottom, the last row is crisp.

**P-LAY-04 Clip, don't resize, to reveal.** Revealing something whose size is already known (an image, a panel sliding open over content, a highlight) animates `clip-path: inset(…)`, not width or height, so nothing reflows. Content that changes the page's layout still goes through a measured height.

**P-LAY-05 Squircles.** Not the default. Turbo corners are round. A client file may turn on `corner-shape: squircle` (or a superellipse mask) for marks and app icons only, with round corners as the fallback. Never mix the two shapes on surfaces of one family.

**P-LAY-06 Space between groups is at least twice the space inside.** 8px inside a group means 16px or more between groups. Space first, a background second, a divider last.

**P-LAY-07 Clearance between controls.** 12px between adjacent filled or outlined controls when no density system says otherwise. Borderless text and icon buttons need 24px between their visible glyphs, padding included, or keep their extended hit areas from touching. Turbo's dense rows keep their own spacing as long as targets don't overlap.

**P-LAY-08 Logical properties.** Anything that would mirror in a right-to-left language uses inline start and end: `ms-*`, `pe-*`, `start-*`, `text-start`. Physical left and right only for physical geometry (safe areas).

**P-LAY-09 Visual order is DOM order.** Never reorder with `order`, `flex-row-reverse` or grid placement: keyboard and screen readers follow the source.

**P-LAY-10 Content bleeds, controls float.** Backgrounds and media may run to the viewport's edge; text and controls stay inside the margins and safe areas (`env(safe-area-inset-*)`, which needs `viewport-fit=cover`). Full-width buttons on mobile sit 16px in from the edges, never glued to them.

**P-LAY-11 Nothing hides under sticky chrome.** A sticky header or footer sets `scroll-padding-block-start` / `-end` on the scroller to its height, so a focused field or an anchor target never lands under it. Anchored headings take `scroll-margin-block-start`.

**P-LAY-12 Breakpoints from content.** Collapse a layout where it stops fitting, not at a device width. Reusable components adapt with container queries (`@container`), not viewport media queries. Test the smallest and largest supported sizes first.

**P-LAY-13 Plan for growth.** Text containers take `min-height` and `max-width`, never a fixed height or width; button rows wrap. Grid tracks holding long content use `minmax(0, 1fr)`, flex children `min-w-0`. Full-height panes use `100dvh`, never `100vh`; widths use `100%`, never `100vw`. A modal taller than the screen scrolls its body and keeps its actions outside the scroll. Test with a long-string language and at 200% zoom.

## Motion

**P-MOT-01 Button press.** Compact controls scale down while pressed and ease back on release.
- Turbo: 0.98, 120ms, `ease-enter`, `scale` only in the transition. Common values range 0.96 to 0.98; below 0.95 it reads as an effect.
- Larger surfaces scale less: a full-width card at 0.98 moves its edges several pixels. Rows don't scale; they press with the accent fill.
- Disabled controls don't press. Clear the iOS tap highlight. Respect reduced motion.
```html
<button class="transition-[scale] duration-(--duration-fast) ease-enter active:scale-[0.98] disabled:active:scale-100 [-webkit-tap-highlight-color:transparent]">
```
- Outside React, iOS Safari needs an empty `touchstart` listener for `:active` to fire.
Check: you feel the press but can't see the animation. If you notice the scale, it's too much.

**P-MOT-02 Interruptibility.** Say where it ends, not how to get there.
- Toggled things (sheets, menus, drawers, sidebars): CSS transitions. They turn around from where they are.
- Keyframes only for one-shots nothing can interrupt (a spinner, a message arriving).
- Springs where speed must carry through: something thrown, or a control flipped so fast the transition's stop-and-reverse shows.
- Lists where siblings enter and leave mid-flight: a layout animation that re-measures.
- Entering from `display: none`: `@starting-style`, not keyframes.
- Never gate input on animation: no `isAnimating` flags that drop clicks, no `pointer-events: none` while it moves.
Check: toggle twice fast (it reverses from where it is); tap five times fast (every tap counts).

**P-MOT-03 Hover restraint.** The more often something happens, the less it moves.
- Hover on things swept past (nav rows, list rows, menu items): the highlight is there at once, or one shared highlight glides (fluid hover). Never a per-row fade-in that trails the pointer. Turbo's 120ms applies to colour changes on a single control, not to fading a highlight in.
- Keyboard-triggered UI is instant. A sidebar opened from ⌘B snaps; the same sidebar opened by clicking its control may animate.
- Tooltips: the first waits 400 to 700ms (Turbo default 500ms), then neighbours open at once with no transition while one is showing.
- Something opened a few times a session (a dialog) can animate; a panel toggled all day can't.
- Hover is unreliable on touch; nothing important lives only on hover.

**P-MOT-04 Easing.** Anything the user triggers eases out (`ease-enter`): fast first frame, soft landing. Never ease-in on an entrance. Never linear except for loops and scroll-linked motion.

**P-MOT-05 Scale entrances.** Popups grow from their trigger (`transform-origin` at the trigger) from 95%, never from 0, never from the screen's centre unless they are centred (dialogs).

**P-MOT-06 Exits are quieter.** Leave faster, travel less, and drop flourishes. A drawer that slid in 48px drifts 24px out. Fading out with a small offset beats sliding the whole way off screen.

**P-MOT-07 Shared layout.** One element moving beats two swapping. A tab indicator, a selection highlight or a pill growing into a card is one element changing place or size, not one fading out while another fades in.

**P-MOT-08 Icon morph.** Swapping one icon for another in place (copy → check, play → pause): cross-fade with scale (0.75 → 1) and a small blur (4px → 0) in 120ms. This is the one place `filter: blur` animates in product UI; keep it to icons 24px and under.
```tsx
<AnimatePresence initial={false} mode="popLayout">
  <motion.span key={copied ? "check" : "copy"}
    initial={{ opacity: 0, scale: 0.75, filter: "blur(4px)" }}
    animate={{ opacity: 1, scale: 1, filter: "blur(0px)" }}
    exit={{ opacity: 0, scale: 0.75, filter: "blur(4px)" }}
    transition={{ duration: 0.12, ease: [0.23, 1, 0.32, 1] }} />
</AnimatePresence>
```

**P-MOT-09 Stagger.** Product UI: only for a staged entrance seen occasionally, a few items, 40ms apart, done by 300ms. Marketing pages may split a hero into title, description and actions 80 to 100ms apart, with `translateY(8px)` and an 8px blur, up to 800ms total. Never stagger anything opened daily.

**P-MOT-10 No animation on load.** Nothing animates on first render that the user didn't ask for. Mount with `initial={false}` or without the starting style.

**P-MOT-11 Never `transition: all`.** List the properties. `will-change` only after you've seen a stutter, only for `transform`, `opacity` or `filter`.

## Details

**P-DET-01 No transitions during a theme switch.** Switching theme turns every transition off for one frame, then back on, or every colour fade fires at once and the page smears. Apply a `*{transition:none!important}` style, flip the theme, force a style read (`document.body.offsetHeight`), and remove the style two animation frames later. Same for the system theme changing underneath.

**P-DET-02 Hover only where hover exists.** Hover styles live behind `@media (hover: hover)` (Tailwind 4's `hover:` already does this; Tailwind 3's does not). On touch, a hover state sticks after a tap and reads as selected. Controls that draw their own pressed state clear the grey tap flash with `-webkit-tap-highlight-color: transparent`.

**P-DET-03 Scroll stays inside overlays.** Any dialog, drawer, menu or side panel that scrolls takes `overscroll-behavior: contain`, so reaching its end never scrolls the page behind.

**P-DET-04 Edges survive forced colours.** A hairline drawn only as a `box-shadow` vanishes in Windows high contrast. Every control and card that relies on a shadow hairline also carries `border: 1px solid transparent`, which forced-colours mode paints.

**P-DET-05 Icons drawn for their size.** Check every icon at the smallest size it renders (often 16px). Use the set's own size grid where it ships one rather than scaling a 24px drawing down. SVG only. Colours come from `currentColor`; no hex fills inside icon files. Turbo keeps outline icons in every state; the row's fill marks the active one.

**P-DET-06 Icons in right-to-left.** Under `dir="rtl"` flip only what points with reading direction: back and forward arrows, chevrons, send, text-alignment and list glyphs. Never flip logos, checkmarks, clocks, physical objects or media playback controls. One flip per icon (`rtl:-scale-x-100`).

**P-DET-07 Selected text looks like the brand, not broken.** If `::selection` is styled, the pair keeps at least 4.5:1.

## Accessibility

Findings cite a WCAG 2.2 A or AA criterion, or a task an assistive-technology user can't finish. Anything else is a recommendation.

**P-A11Y-01 Native elements first.** `<button>` for actions, `<a href>` for navigation (it must open in a new tab on Cmd/Ctrl/middle click), native inputs for input. ARIA only where no element exists, and then the full keyboard map comes with it.

**P-A11Y-02 Focus is visible and never hidden.** Style `:focus-visible`, never remove the outline without a replacement. A custom ring reaches 3:1 against everything it touches (1.4.11). A focused element is never fully covered by sticky chrome (2.4.11; P-LAY-11). Forced-colours mode keeps system colours.

**P-A11Y-03 Keyboard reaches everything.** Every pointer action has a keyboard path. Tab moves between widgets, arrows move inside them (one tab stop per group, roving `tabindex`), Enter and Space activate, Escape closes whatever opened last. Only `tabindex` 0 and -1; never positive.

| Widget | Keys |
| --- | --- |
| Dialog | Tab cycles inside, Escape closes |
| Tabs | Arrows move and wrap, Home and End jump, Tab goes to the panel |
| Menu button | Enter, Space or Down opens on the first item; Up opens on the last; Escape closes and returns focus |
| Combobox | Down opens the list, Enter accepts, Escape closes back to the input; focus stays in the input (`aria-activedescendant`) |
| Listbox, radio group | Arrows move the selection; one tab stop |

**P-A11Y-04 Focus moves with the user.** Dialogs trap focus, make the page behind `inert`, and return focus to the trigger on close (native `<dialog>` with `showModal()` does all three). A client-side route change updates `document.title` and moves focus to the new view's `<h1>` (with `tabindex="-1"`) or `<main>`.

**P-A11Y-05 Skip link and landmarks.** The first focusable element is "Skip to content", hidden until focused, pointing at `<main id="main">`. One `<main>`. Headings describe their sections and form an outline.

**P-A11Y-06 Targets.** The AA floor is 24×24px (2.5.8); Turbo's target is 40px on desktop and 44px on touch (P-LAY-02). Every drag has a single-pointer alternative (2.5.7).

**P-A11Y-07 Every control has a name.** Inputs have a visible `<label>`; the label and the control share one hit area. Icon-only buttons have an `aria-label` that names the action ("Delete project", never "Trash icon"). When a control has visible text, its accessible name starts with that text. `aria-hidden` never sits on, or above, anything focusable.

**P-A11Y-08 Forms that work with tools.** A real `<form>`, a valid `autocomplete` token on every field about the person (1.3.5: `email`, `name`, `current-password`, `new-password`, `one-time-code`…), and the `type` and `inputmode` that bring up the right keyboard (`inputmode="numeric"` for codes, `"decimal"` for money). `spellcheck="false"` on emails, codes and usernames. Never block paste.

**P-A11Y-09 Errors that announce.** Validate on submit. Failing fields get `aria-invalid="true"` and `aria-describedby` pointing at the error under them; focus moves to the first one. While a request runs, the button keeps its label with the spinner beside it, takes `aria-disabled="true"` (not `disabled`, which drops focus) and ignores repeat presses. Warn before leaving unsaved changes; a re-render never wipes what was typed.

**P-A11Y-10 Announce what changes.** Toasts, "Saved" and result counts go through a polite region (`role="status"`) that is already in the DOM, empty, before its text changes. `role="alert"` is for urgent errors not tied to a field, nothing else. Never move focus to a toast. A region that's loading takes `aria-busy="true"`, then announces the outcome ("12 results").

**P-A11Y-11 Nothing important runs on a timer.** A toast with an action or an error stays until dismissed. A plain confirmation toast stays at least 5 seconds and pauses while hovered or focused. Anything that moves or updates on its own for more than 5 seconds has a visible pause (2.2.2).

**P-A11Y-12 Contrast.** Text 4.5:1, large text (24px, or 18.67px bold) 3:1, component boundaries, states and meaningful graphics 3:1 (1.4.3, 1.4.11). Measure the pair as it renders, on the surface it actually sits on, in both themes. Disabled controls and logos are exempt. Colour never carries meaning alone (1.4.1; design-system.md).

**P-A11Y-13 Alt text by purpose.** Decorative: `alt=""` (present, empty). Informative: what it means. Functional (the image is the button or link): the action. A chart: a one-line summary, with the data in a table or text nearby. Never leave `alt` off.

**P-A11Y-14 Zoom and reflow.** Text survives 200% resize (1.4.4), and the page reflows at 320px wide with no sideways scroll (1.4.10). Never `maximum-scale=1` or `user-scalable=no`.

**P-A11Y-15 Disabled means unavailable.** Native `disabled` only when the control truly can't be used. If it needs to stay focusable (to show why it's off), use `aria-disabled="true"`, block its behaviour in code and say why in text beside it; tooltips on a natively disabled control never show.

## Writing

These extend Writing in `taste.md`.

**P-WRI-01 Inventory first.** Before writing a string, find where copy lives (locale files, `t(` calls, the CMS) and list the noun for each object and the verb for each action. New copy uses those words. A multi-step flow keeps one set: Get started, Continue, Done.

**P-WRI-02 Build strings whole.** Never glue a sentence around a variable (`"You have " + n + " messages"`). One message with placeholders and the locale's plural rules (ICU `{count, plural, one {# message} other {# messages}}`, or `Intl.PluralRules`). Many languages have more than two forms.

**P-WRI-03 Format through `Intl`.** Numbers, currency, units, dates, relative times and lists go through `Intl` with the user's locale (`NumberFormat`, `DateTimeFormat`, `RelativeTimeFormat`, `ListFormat`). Never hand-build a date or join names with ", " and " and ".

**P-WRI-04 One case per element type.** Sentence case everywhere unless the project set another policy. "Save Changes" beside "Discard changes" is a bug.

**P-WRI-05 Words for the device.** "Tap" on touch, "click" with a pointer, "select" when either. No idioms or jokes that won't translate. Possessives sparingly ("Favorites", not "Your favorites"), and never "My" and "Your" in one flow.

**P-WRI-06 Errors phrased as the fix.** "Use at least 8 characters", not "Password too short" and not "Don't use fewer than 8". "Enter a date as DD/MM/YYYY", not "Invalid date". If the same error keeps firing, change the interaction so it can't happen.

**P-WRI-07 Destructive copy.**

| Case | Copy |
| --- | --- |
| Reversible and frequent | Act at once; toast "Project archived" with Undo |
| Irreversible, one item | Title "Delete 'Q3 report'?"; body says what goes with it, with counts; buttons "Delete report", "Cancel" |
| Irreversible, many | "Delete 12 files?"; "You can't undo this."; "Delete 12 files" |
| Affects others | The body names who: "8 members lose access." |
| A workspace, account or repo | Type its name to confirm; the button stays off until it matches |

**P-WRI-08 Status copy.**

| State | Pattern |
| --- | --- |
| Working | -ing verb and object: "Saving changes…" |
| Done | Object and past verb: "Changes saved" |
| Failed | What didn't happen, then the next step: "Changes not saved. Check your connection and try again." |
| Unavailable | Why, and what unlocks it, beside the control: "Add a payment method to publish" |
| Partial | Both counts: "9 of 12 uploaded. Retry 3" |

**P-WRI-09 Links and settings.** Repeated "Learn more" links each say what about ("Learn more about exports"). Link straight to a setting instead of describing the path to it. A filtered empty state names the query: "No results for 'quarterly'. Clear filters". Never keep lasting information in an empty state; it disappears once content arrives.

## Sound

**P-SND-01 Interface sounds.** Off by default. Where a client turns them on: only to confirm an action the user just took (sent, done, error), never for passive events; under 200ms, quiet, and the same sound for the same meaning everywhere. Always a setting to turn them off, and silent when the system is muted.

**P-SND-02 Layered cues.** A fuller cue is a stack of short sources (a click plus a soft tone) trimmed and mixed into one file, not one long sample. Preload; play from a single audio context so it fires on the frame of the action.

## Data

**P-DAT-01 Living charts.** When data updates, the chart moves to the new values: the line morphs, bars grow, axes rescale on the same curve (180ms, `ease-standard`). Never a redraw from empty. New points slide in from the edge they arrive at.

**P-DAT-02 Curve smoothing.** Smooth lines must never invent data. Use monotone interpolation (`curveMonotoneX` in d3, `monotone` in Recharts) so the curve never overshoots a point or dips below zero. Use straight segments when there are few points or exact values matter.

**P-DAT-03 Numbers in charts.** Tabular figures on axes, tooltips and totals (P-TYPE-01). Right-aligned values in legends. Series colours from one OKLCH lightness (P-COL-01); red only for trouble.

## Review pass

Walk this in order on any screen before calling it done. Foundations first, so polish doesn't hide a broken flow. Each line names the rules it checks.

1. Tab through the whole flow: every stop has a visible ring, nothing is unreachable or hidden under a sticky bar, Escape closes what opened last. (P-A11Y-02 to 05)
2. Every control has a name and a label; icon buttons name the action. (P-A11Y-07)
3. Submit an empty form: errors sit beside their fields, focus lands on the first, nothing is disabled up front. (P-A11Y-09, P-WRI-06)
4. Zoom to 200% and narrow to 320px: nothing clips, nothing scrolls sideways, actions stay reachable. (P-A11Y-14, P-LAY-13)
5. Squint: one thing to look at first. (taste.md)
6. Every changing or stacked number holds still and lines up. (P-TYPE-01, P-DAT-03)
7. Headings balance, paragraphs have no orphan, line-heights fit their role, nothing under 18px is thin. (P-TYPE-03, P-TYPE-07, P-TYPE-08)
8. Every icon looks centred, and buttons with icons look evenly padded. (P-ALN-01 to 04)
9. No nested pair has a thick rim at the corner; groups are spaced at least twice their inside gap. (P-LAY-01, P-LAY-06)
10. Every image has its inner edge; nothing has a border beside a shadow; shadow-only edges survive forced colours. (P-COL-02, P-COL-03, P-DET-04)
11. Every scroll container fades only where there's more, and overlays don't scroll the page. (P-LAY-03, P-DET-03)
12. Overscroll in dark Safari shows no white; switching theme doesn't smear. (P-COL-05, P-DET-01)
13. Every control presses, nothing scales that shouldn't, every target is at least 40px. (P-MOT-01, P-LAY-02)
14. Toggle everything twice fast: it reverses from where it is, no click is dropped. (P-MOT-02)
15. Sweep the pointer across every list: nothing trails. Use the shortcuts: nothing waits. On a phone, nothing stays stuck in hover. (P-MOT-03, P-DET-02)
16. Reload: nothing moves that you didn't ask for. Turn on reduced motion and run 13 to 15 again. (P-MOT-10)
17. Read every string: one vocabulary, one case, verbs on buttons, no sentence glued around a variable, dates and numbers formatted by locale. (P-WRI-01 to 05)

Report findings as a table: severity, `file:line`, rule ID, what's there, the fix. Severity is user impact: HIGH blocks a task, hides content or is a WCAG failure; MEDIUM makes it meaningfully harder; LOW is polish. One root cause is one finding with every location listed. A review changes nothing unless asked to fix.
