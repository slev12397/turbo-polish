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

## Sound

**P-SND-01 Interface sounds.** Off by default. Where a client turns them on: only to confirm an action the user just took (sent, done, error), never for passive events; under 200ms, quiet, and the same sound for the same meaning everywhere. Always a setting to turn them off, and silent when the system is muted.

**P-SND-02 Layered cues.** A fuller cue is a stack of short sources (a click plus a soft tone) trimmed and mixed into one file, not one long sample. Preload; play from a single audio context so it fires on the frame of the action.

## Data

**P-DAT-01 Living charts.** When data updates, the chart moves to the new values: the line morphs, bars grow, axes rescale on the same curve (180ms, `ease-standard`). Never a redraw from empty. New points slide in from the edge they arrive at.

**P-DAT-02 Curve smoothing.** Smooth lines must never invent data. Use monotone interpolation (`curveMonotoneX` in d3, `monotone` in Recharts) so the curve never overshoots a point or dips below zero. Use straight segments when there are few points or exact values matter.

**P-DAT-03 Numbers in charts.** Tabular figures on axes, tooltips and totals (P-TYPE-01). Right-aligned values in legends. Series colours from one OKLCH lightness (P-COL-01); red only for trouble.

## Review pass

Walk this in order on any screen before calling it done. Each line names the rules it checks.

1. Squint: one thing to look at first. (taste.md)
2. Every changing or stacked number holds still and lines up. (P-TYPE-01, P-DAT-03)
3. Headings balance, paragraphs have no orphan, text is antialiased. (P-TYPE-02, P-TYPE-03)
4. Every icon looks centred, and buttons with icons look evenly padded. (P-ALN-01 to 04)
5. No nested pair has a thick rim at the corner. (P-LAY-01)
6. Every image has its inner edge; nothing has a border beside a shadow. (P-COL-02, P-COL-03)
7. Every scroll container fades only where there's more. (P-LAY-03)
8. Overscroll in dark Safari shows no white. (P-COL-05)
9. Every control presses, nothing scales that shouldn't, every target is at least 40px. (P-MOT-01, P-LAY-02)
10. Toggle everything twice fast: it reverses from where it is, no click is dropped. (P-MOT-02)
11. Sweep the pointer across every list: nothing trails. Use the shortcuts: nothing waits. (P-MOT-03)
12. Reload: nothing moves that you didn't ask for. (P-MOT-10)
13. Turn on reduced motion and run 9 to 12 again.

Report findings as a table: severity, `file:line`, rule ID, what's there, the fix.
