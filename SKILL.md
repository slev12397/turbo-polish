---
name: turbo-polish
description: Turbo's taste, design system and motion rules for building AI-native product UI. Use for any UI work on a Turbo project: designing, building, reviewing or polishing screens, components, copy or motion, by a person or a coding agent.
---

# Turbo Polish

How Turbo builds product UI. This is the entry point. It holds the rules that always apply and says which document to read for the rest.

The package is the studio default. Each client gets a file in `clients/` that sets their brand values (fonts, colours, marks) and any product rules of their own. The defaults hold wherever a client file is silent.

## Read before you build

| When | Read |
| --- | --- |
| Any UI work: designing, building or reviewing | [`taste.md`](taste.md) (what good looks like), then [`design-system.md`](design-system.md) (what to build with) |
| Anything that moves: an entrance, an exit, a hover, a gesture, a change of size | [`motion.md`](motion.md) as well |
| Polishing or reviewing details (numbers, alignment, corners, edges, scroll fades, press, hover, data) | [`polish.md`](polish.md): rules by ID, and the review pass at its end |
| Work on a specific client | `clients/<client>.md` first, for their brand values and product rules |

## Order of precedence

1. The client file, for its brand values and its own product rules.
2. `taste.md`.
3. `design-system.md` (everything that stands still) and `motion.md` (everything that moves).
4. `polish.md`.
5. Any installed skill or personal preference.

A client file can change a value (a font, a colour, a radius step). It can't relax a rule in `taste.md` without saying so and why.

## Rules that always apply

1. **Reuse before you build.** Check the project's component catalog, then compose from its shared components and patterns.
2. **Tokens only.** No raw colours, px font sizes, literal shadows, or hard-coded durations and easings. Tokens live in one theme file; text styles are named classes (`type-*`).
3. **Never branch on the theme.** No `dark:` classes and no reading the theme to pick a style. If one theme looks wrong, fix or add a token.
4. **Concentric corners.** Outer radius = inner radius + the padding between them. Never the same radius on nested surfaces. Buttons (pills) are exempt.
5. **One spine.** Page text and the content inside rows and surfaces share one left and right edge. A row, a table or a card hangs past it by its own padding, so its content lines up, not its container. See Layout in `taste.md`.
6. **Radius and shadow by role.** Pick the role's token; never write a custom shadow. A control has a hairline and no shadow. A card has a soft shadow with a hairline as its last layer, never a border beside a shadow.
7. **Not everything in a box.** The page is white, and lists, sections, tables and traces sit straight on it. A card is for the one thing that should stand apart. No tinted layer behind content, and no divider that runs edge to edge.
8. **Numbers that change or line up use `tabular-nums`.**
9. **Motion replies to the user.** Use the duration tokens (120, 180, 280ms), never longer than 300ms, nothing on page load, and an exit is never slower than its entrance. Movement animates only `opacity`, `scale`, `translate` and a measured `height` or `width`; hover and focus may also ease colour, fill and shadow. List the properties: never `transition-all`.
10. **One source for each kind of thing.** One icon module with a `size` prop, one toast wrapper (a title, and at most one faded line under it naming what it was about), one spinner, one thinking state for an agent's reply, one page heading component, one table for records, one badge for tags.
11. **Copy names the action.** Short, plain, sentence case. Buttons start with a verb, errors say how to fix it, a headline or subline takes no full stop, and the same thing has the same name everywhere. See Writing in `taste.md`.
12. **No primitive for it yet?** Compose it from tokens and existing components. If it will be reused, add it as a shared component with a catalog entry. Say in your summary what you added and why; never leave a one-off restyle at the call site.
13. **Keep the system portable.** Shared components and styles import nothing from the app, hold no sample data and no navigation.

## Before you finish

- Changing something shared? List what uses it first (`rg -l 'from "@/components/ui/<name>"' src`).
- New or changed shared component: catalog entry, an example, and a use in a real view.
- Run the project's typecheck, lint and build.
- Check light and dark, about 375px wide, keyboard only, the checklist at the end of `taste.md`, and the review pass at the end of `polish.md`.
- Changed a token, a component's look or its motion? Update the document that describes it in the same change.

## What's in the package

| Path | What |
| --- | --- |
| `taste.md` | What good looks like: the feel, the five rules, the details, writing, AI moments, don'ts, pre-ship checklist |
| `design-system.md` | How to build it: token architecture, type, colour, radius, elevation, spacing, icons, components by role, accessibility |
| `motion.md` | How it moves: principles, tokens, recipes, rules, gestures, reduced motion, earned exceptions |
| `polish.md` | Polish rules with IDs: principles, typography, icons and alignment, colour and surfaces, layout, motion, sound, data, and a 13-step review pass |
| `clients/_template.md` | Start a new client file from this |
