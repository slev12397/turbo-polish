# Design system

How to build UI on a Turbo project. Applies to people and coding agents. For what to aim for, read [`taste.md`](taste.md) first; it wins when a judgment call isn't covered here. For how things move, read [`motion.md`](motion.md). The client file in `clients/` sets the brand values (fonts, colours, marks) and maps these roles to the project's real paths.

The values below are the Turbo defaults. A client changes a value by changing the token, never by restyling a component.

## Where things live

Every project fills in this map in its client file. The roles are fixed; the paths are the project's.

| What | Typical path |
| --- | --- |
| Tokens (colours, text styles, spacing, radius, elevation, motion) | `src/styles/theme.css` |
| Theme switch (`data-theme` on `<html>`, preference, `useTheme`) | `src/components/theme.ts` |
| Icons, mapped by role | `src/components/icons.tsx` |
| Shared components, one per file | `src/components/ui/` |
| Composed patterns | `src/components/patterns/` |
| Toast wrapper | `src/components/ui/toast.tsx` |
| Component catalog (gallery index) and examples | `src/catalog/index.ts`, `src/catalog/examples/` |
| The app: shell, views, sample data | `src/app/` |

Shared components and styles are portable: they never import from the app or the catalog, and hold no sample data or navigation.

## Tokens and themes

Colours are three layers in the theme file, and components only touch the last:

1. **Ramp:** one set of raw grey steps shared by every theme (`--gray-50` to `--gray-950`, plus `--white`). No colour in it at all: paper and charcoal.
2. **Theme blocks:** `:root, [data-theme="light"]` and `[data-theme="dark"]`. Each gives every semantic token (`--card`, `--muted-foreground`, `--border`, `--brand`…) a value. Both list the same tokens in the same order.
3. **Utilities:** `bg-card`, `text-muted-foreground`, `shadow-sheet`…, mapped from the semantic tokens.

- **Never branch on the theme.** No `dark:` classes, no `useTheme()` to choose a class or a colour. If something looks wrong in one theme, re-point a token or add one with a value in both blocks.
- **Adding a token:** declare it in both theme blocks, map it to a utility, and add it to the gallery's colour section. A token missing from one block silently keeps the other theme's value.
- **Never use a ramp step directly.** Add or reuse a semantic token.
- **Shadows follow the theme by themselves.** Depth layers are black at an alpha multiplied by one of two strengths: `--shadow-rest` for what rests on the page and `--shadow-float` for what floats above it. Light sets both to 1. Dark sets rest to 0 and float to about 3.5, so in dark only floating things cast a shadow. Edges and halos inside the roles are tokens too. Don't write a shadow with a literal colour in a component.
- **How the two themes separate things.** Light: a card by a hairline and soft depth on the same white, controls by a hairline, menus and dialogs by depth. Dark: a card by its lighter fill alone, controls by a hairline, menus and dialogs by depth alone, several times stronger. If one element needs a different treatment, change the family's token, not that element.
- **The same in both themes:** `on-color` (white on a solid colour), avatar colours, and the status hues (in dark the tints are the lighter step at 20%, same meaning).
- **Adding a theme:** copy a block, change its selector, give every token a value, and register the name with the theme switch. The preference is stored locally and applied by an inline script before first paint.

## Type

- **Rendering:** `antialiased` on the body; `<html>` takes the page background token so overscroll never shows a white canvas; `font-optical-sizing` stays `auto`. See `polish.md` (P-TYPE-02, P-COL-05).
- **Families:** a heading face (`--font-heading`, set per client, Medium 500) and a text face for everything else (Inter by default). Never apply the heading face to UI text. Every heading style shares one tight tracking (-0.3px by default).
- **Scale:** `type-h1` / `type-h2` / `type-h3` for headings; `type-body-lg` (16px: a chat message, lede text), `type-body` (14px), `type-body-sm` (13px: metadata and help text), `type-caption` (12px), each with a `-medium` variant; `type-mono-12` (+ `-medium`) for code and keys. No larger mono. Figma text styles use the same names.
- A page names itself with a page heading component: the title and a short subline, both `type-h2`, the subline in the subtle ink; on a phone the actions wrap under it.
- A group label over a list is `type-caption-medium` in the muted ink.
- Numbers that change or align get `tabular-nums`.

## Ink

`text-foreground` → `text-muted-foreground` → `text-subtle-foreground` (placeholders, counters, separators, and the subline of a page heading). Ink is soft charcoal (gray-900), never pure black.

## Surfaces and fills

- `bg-canvas` (app chrome: the sidebar) and `bg-background` (the page) are one white fill, apart by a hairline.
- `bg-card` (a card on the page, also white) and `bg-field` (white inputs and outline buttons, hovering to `bg-field-hover`).
- `bg-raised` is a piece inside a filled track (the selected segment).
- `bg-popover` is menus, selects, popovers, dialogs and the drawer.
- Fills are ink at low alpha so they work on any surface: `bg-hover` (3%: hover, a table's header bar), `bg-muted` (5%: the quiet fill, selected rows, the selected tab), `bg-accent` (8%: pressed). Never write a hover as an opacity of another fill (`bg-muted/60`).
- In a list of rows you can press, the hover is one fill that glides to the nearest row (a fluid hover component), not a fill on each row. A row a step in sets an inset so the fill hugs it. Menus and selects have it built in, following pointer and keyboard alike.
- **Most content has no surface of its own.** A list, a settings section, a table and a trace sit straight on the page.
- Dividers are ink at 6%: `border-subtle` (inside a surface, and the line that ends a list) and `border` (free-standing) share the value; `border-strong` (dashed placeholders, the line of a trace) is a ramp step.
- A control's hairline (`--outline-edge`, ink at 6%) is there at rest only: hovered or open, the control shows its fill and the hairline goes. A select or filter whose menu is open shows no focus ring either.
- In dark, the same roles walk up the dark end of the ramp (page and sidebar, then one raised fill for cards, menus and dialogs); fills and edges are white at low alpha; a field has no fill at rest, only its hairline; resting surfaces have neither shadow nor hairline.
- **Tooltips and toasts** are inverted glass: translucent dark ink in both themes, always with the blur (24px, saturate 1.5). A translucent surface never goes without it. Text and icons on it are `text-on-color`, at lower alpha for quieter text (62% descriptions, 42% shortcuts). On a tooltip the blur goes on an ancestor that doesn't animate.

## Colour roles

- `primary` is ink (gray-900, near white in dark): the one primary action, and dark marks.
- `link` (blue) is selection, focus, links and "on". `link-subtle` is that blue at 16% (a deeper blue at 30% in dark, so it doesn't go navy), with `link-strong` as text on it and `link-bright` for something live on it.
- `on-color` is white on a solid colour or the inverted glass.
- `brand` is informational.
- **Status comes in threes:** `success` (marks and solid fills), `success-subtle` (the tint behind a tag), `success-strong` (text on the tint); the same for `warning` and `destructive`. The tints are literal OKLCH colours in one family: every fill the same lightness (0.93 in light; 0.75 at 20% in dark) and every text the same lightness (0.36; 0.86 in dark), so only the hue changes and no tag is heavier than the one beside it.
- A `Badge` uses them at two strengths: `strong` (the full tint: a state to be seen) and `soft` (the tint at 40%: a value picked out in a sentence). A dot or icon in a badge is the same badge.
- Colour used for identity (an agent's mark, a person's avatar) uses the status pairs, never the destructive pink.
- A destructive menu item is `destructive` for text and icon, highlighting on the soft pink. The destructive button is the pink tint at 70% (full on hover) with a red a notch calmer than `destructive` as its text: firm without shouting.
- **Content colours** (a tool's brand, a product tone on a card's stage) are passed in, not kept in the theme. They tell a few things apart; they never mean status.
- Code, highlighted, takes syntax inks that point at roles already readable in both themes (keyword, string, function, number, comment); plain names stay in ink.

## Radius, by role

`rounded-xs` 4 the smallest marks · `rounded-sm` 6 kbd, small tiles and avatars · `rounded-md` 8 20px tiles, a filter token's pill · `rounded-lg` 10 24px tiles · `rounded-xl` 12 inputs, selects, textareas, tabs, menu items, sidebar rows, table rows and header, 32/36px tiles · `rounded-2xl` 14 rows that hold a 20 or 24px tile · `rounded-3xl` 16 menus, popovers and toasts · `rounded-4xl` 18 cards, the drawer, rows that hold a 32 or 36px tile · `rounded-5xl` 24 dialogs, the composer and large wells · `rounded-6xl` 32 large dialogs that open on a picture.

Tags have their own three, following the badge's height: `rounded-badge-sm` 6, `-md` 8, `-lg` 10 (also chips).

**Roundness is one system:** buttons are pills at every size and icon buttons are circles. Everything else keeps a corner. Don't set a radius on a button or a sidebar row at the call site. A nested corner is its container's corner less the padding between them.

**Concentric pairs (defaults):** menu `rounded-3xl` + `p-1` → item `rounded-xl` · a menu of tile rows `rounded-4xl` + `p-1` → row `rounded-2xl` + `pl-1.5` → 20px mark `rounded-md` · card `rounded-5xl` + `p-1.5` → row `rounded-4xl` + `p-1.5` → 32/36px mark `rounded-xl` · a card led by a tile `rounded-5xl` + `p-2.5` → 40px tile `rounded-2xl` · filter token `rounded-xl` + `p-1` → pill `rounded-lg` · chip `rounded-badge-lg` + 2px → 20px mark `rounded-md`.

## Strokes

A control's hairline (and the rim on tooltips and toasts) is an inner border: `inset 0 0 0 1px`, painted on top of the control. A resting surface's hairline (a card) is the last layer of its shadow, drawn outside the surface, so edge and depth read as one. Don't add a ring or a CSS border on top of a role shadow. Use `border` only for dividers.

## Elevation, by role

Each role separates one way. Every shadow is soft, with far more blur than offset.

| Role | Token | Default |
| --- | --- | --- |
| Raised tiles | `shadow-control` | `--outline-edge`, ink at 6%, no shadow |
| Outline buttons, kbd, inputs, selects, textareas, filter tokens | `shadow-outline` | the same hairline |
| Filled buttons | `shadow-solid` | flat |
| A card on the page | `shadow-sheet` | six soft layers, the largest `0 18px 47px` at 2.2%, one faint upward layer `0 -1px 10px` at 1.5% so the top edge holds, then `--sheet-edge` (ink at 1%) |
| A hovered card | `shadow-lifted` | `0 6px 32px` at 12% |
| Menus, selects, popovers | `shadow-overlay` | `0 4px 32px` at 14%, no edge |
| Dialogs, the drawer | `shadow-modal` | `0 8px 48px` at 15%, no edge |
| Tooltips, toasts | `shadow-menu` | a faint light rim plus `0 4px 24px` at 15% |
| Text inputs | `shadow-focus`, `shadow-invalid` | halos mixed from `--link` and `--destructive` |

## Spacing

- The 4px scale (`gap-2` = 8px).
- Controls are 32px tall; 28px (`size="sm"`) in a header bar, a table row or a card's footer. Rows in a menu or list are at least 32px; a table row is 48px.
- Between a page heading and its content 32px; between a toolbar and its list 16px; between settings sections 32px either side of a hairline.
- Recurring relationships belong to components, never ad-hoc classes: field gaps (`FieldGroup`, `Field`), a second way to do the same thing after a separator with "or" in it, row rhythm (`SettingRow`, `Table`), a long list cut to its first rows (`ShowMore`), dialog padding, the title and its actions (page heading).

## Icons

- One icon module, imported by role (`CloseIcon`), never the icon package directly (lint blocks it). One set, used as it ships (Hugeicons Stroke Rounded by default; a client can name another).
- Sizes: 16 (base), 20, 24, 40, plus 12 and 14 for the smallest controls only. Strokes: 12 → 1px, 14 → 1px, 16 → 1.25px, 20 → 1.5px, 24 → 2px, 40 → 2.5px, held in `icon-size-*` utilities.
- Inside buttons and menu items, leave `size` off: the component sizes it. Don't size icons with `size-*` classes.
- The active item in navigation is marked by its row's fill, not a filled icon.
- Icon-only buttons need an `aria-label` that names the action ("Close dialog"), and a tooltip.

## Components by role

**Buttons.** One `default` (primary) per view; the next action is `secondary`; dismissals and tertiary actions are `ghost`, except a dialog footer's Cancel, which is `secondary`; `destructive` is the confirming step of a delete. `outline` is for a button in a row of outlined fields, or where the secondary fill would vanish into a tinted surface; it is not the everyday second button. A button that becomes its result ("Add" → "Added") wraps its words in a swap label so it keeps its width and nothing beside it rewraps.

**Drawer, dialog, popover, preview card, menu.**
- `Drawer`: a floating right-edge panel for secondary content while the page stays in view. Popover fill and `shadow-modal`, no edge. One width (480px) everywhere; a wider one only where it stands in for a side panel on a narrow screen. The page behind fades slightly (ink at 12%) and stays readable; one being edited takes the full scrim. Its footer has no divider: what scrolls under it fades into the drawer's fill. While it's open, toasts move out of its way and stay there until they've gone.
- `Dialog`: a focused task.
- `Popover`: a moment's work beside what it's about (a colour picker from its swatch, a few fields from a button). A menu's surface and motion; it holds fields, so 12px of room and a 24px corner; takes focus as it opens and gives it back as it closes; flips or shifts to stay in the window.
- `PreviewCard`: a look on hover.
- `DropdownMenu`: a list of actions.

**Menus and selects.** Popover surface, `shadow-overlay`. An item led by an icon has it at 20px in full ink, 6px in, set by the item component. A separator is inset to its items. Every pick ends in a 16px disc, one value or several: nothing when off; on, it fills with the link blue and a white check lands in it. One menu choosing into another hands over in place (see `motion.md`).

**Search.** A command palette component holds the field, the grouped list and the keys; the app decides what it finds. It dims the page lighter than a dialog does (12% ink, 30% in dark). A row's icon is foreground ink at 20px; a logo keeps its colours.

**Loading.** `Spinner` is the loader for a control or a region: one size (16px), the text colour. Buttons use their `loading` state, toasts their loading call; never spin an icon. An agent's reply loads as a `Thinking` trace, then streams. Content whose shape is known while it's on its way holds its place with `Skeleton`, which wraps the real content and lays a muted sweeping bar over its exact box, then fades to it. Don't draw skeleton shapes by hand.

**Scrollbars.** One global style: thin, a quiet thumb, no track, its colour a token. Never style a scrollbar in a component. Two variants: a ghost thumb that shows only while hovered or focused, and a fine 4px one for a narrow list beside its own rows.

**Lists that outgrow their box.** Every scroll container fades only the edge with more past it, by mask, never an overlay (P-LAY-03 in `polish.md`). A list of several in a dialog or card scrolls in place in a scroll-fade container: capped partway through a row so the next one peeks out, fading at an edge only while there's more past it. A control over the whole list (All 14) sits above it, outside the scroll.

**Tile rows.** An icon or logo in a row goes in a `Tile` (20, 24, 32, 36 or 40px). The row pads it equally on the top, bottom and tile side (`p-1.5` for 32/36px) and takes the tile radius plus the padding. What ends the row sits the same distance from its top, bottom and end: measure the row, then set the end padding to match. Quiet at rest, the hover fill on hover. Pull it out by its side padding so the tile and text sit on the spine. When the whole row can be pressed, the name is the button and covers the row; any other control in it sits above.

**Chips and marks.** A person, account or agent named in a line or a row is a `Chip`: a 20px mark and the name on the quiet fill (or a soft tint of the avatar's colour). Never build one from a span. People are `Avatar`, the organisation is a brand mark, an agent is an agent mark.

**Done, in a chat.** Anything finished in a chat wears one done mark: a 16px disc in the subtle grey with the check cut out of it. Not green, and no bare check marks. An answered ask folds to its line; the line is the confirmation. An ask left too long or gone wrong keeps its card with one way on, under a strip in the status's soft tint.

**Images.** A picture in the UI has no frame: a 1px edge inside it at 20% ink, and the radius of where it sits; a picture that's mostly colour or dark takes the same edge in white. Labels and controls over a picture go on the inverted glass.

**Logos.** A logo drawn in one dark colour  takes an `ink-mark` utility that turns it light in dark. A logo with its own colours is left as it is. Never pick the logo file by theme.

**Cut text.** Text cut to one line where the rest matters gets an ellipsis and the whole of it in a tooltip, only when it's cut.

**Dotted underline.** Link-like text that underlines on hover takes a line of round dots in the text's colour, drawn under its padding; not `decoration-dotted`, whose dots are square.

**Forms.**
- Every input has a visible label. A placeholder is an example, never the label.
- Don't disable submit until the form is valid: validate on submit, show each error beside its field, and move focus to the first field that failed.
- Set the right `type` and `autocomplete`, and never block paste.
- A key is always asked for with one key-field pattern: where to get it at the label's end, and "Stored encrypted" under the field.
- A choice between a few things that each have a mark is choice tiles (rows, one gliding hover, the picked one on the muted fill with the blue disc at its end); words alone are `Tabs`; a longer list is a `Select`.
- A setup of a few decisions in order is `Steps`: only the current step open, done ones checked with what was decided.
- Settings most people leave alone fold under `Advanced`, below the ones that matter, and open by themselves when one of them is set.
- Leaving something that holds unsaved changes asks first (Keep editing or Discard).

**Tables.** `Table` for records. Fixed column widths when it can be filtered. Selectable rows put their boxes in the gutter, and what you can do with the picked rows is a bar at the window's foot with the count, small actions and Clear selection; Escape clears.

**First run.** A page before its first item is one card on the spine: what to do or why it's worth it as the title, a line on what it does, the create action first. The page header's own create button hides while it shows, so there's one way in.

## External packages

- Don't edit or fork them. Wrap each one in a single local file, connected to tokens through its supported styling API. Product code imports the wrapper, never the package.
- **shadcn CLI:** generated components arrive with Lucide imports, `dark:` classes and the preset's look. Replace the icons with the icon module, delete every `dark:` class and any `next-themes` use, then restyle with the role tokens so it matches.

## Accessibility

- Build on accessible primitives (Base UI, Radix): they handle keyboard behaviour, focus trapping and return, and roles. Don't replace them with divs. A button for an action, a link for navigation, never a clickable div.
- Visible focus on everything: a 2px ring, or the focus halo on text inputs. A row that fills a list that scrolls or clips takes the ring inside its edge, so it's never cut off.
- **Hit areas.** A control can look 32px tall and still be easy to hit: where there's room, extend its clickable area to at least 40px (44px on touch) with an invisible pseudo-element. Extended areas never overlap a neighbour's. Decorative layers over content take `pointer-events-none`.
- **Colour is never the only signal.** A status also has an icon or a word.
- **Text containers grow.** A box that holds text gets a `min-h-*`, never a fixed height.
- **Dragging has another way.** Anything that can be dragged can also be done without it.

## Keeping the system in sync

- One component per file. Register it in the catalog (summary, guidance, variants, setup notes), add an example file (the gallery shows its real source; never paste code strings into the catalog), and use it in a real view.
- Before changing anything shared, list what uses it and name the affected views in your summary.
- **Pages:** every view renders inside one page component. A top-level page has no header bar and names itself with the page heading. A page that belongs to something keeps the 44px bar: breadcrumb and title on the left, page actions at `sm` on the right. Pages in one area share one width. Empty states centre.
- **The shell:** sidebar provider → sidebar → resize handle → optional second nav column → the page. The sidebar has an 8px margin; the page shares its fill behind a hairline and runs to the window's edge. The hairline is what you drag to resize. Collapsed is an icon rail with tooltips; the mark becomes the expand control. A row that opens something of its own ends in a chevron. The account menu is name and email, then Theme, then Sign out.
- Tuning panels and prototype controls are for exploring only. Move the chosen values into tokens or variants before finishing.

## Verify

Typecheck, lint and build. Check desktop and about 375px, light and dark, keyboard only, and the checklist at the end of `taste.md`.

## Skills

Optional. These documents stand on their own. If you use a skill, its output still follows `taste.md`, this document and `motion.md`: our tokens, components, icons and motion values win.

**Visual feedback.** A dev-only annotation toolbar is the default way to point at an element and leave a note for an agent. An agent receiving its output uses the selectors to find the element, then makes the change in the shared component or token that owns it, not only at that one spot.
