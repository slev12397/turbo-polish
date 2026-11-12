# Motion

How things move in a Turbo product. Read it before adding or changing anything that animates. [`taste.md`](taste.md) says what to aim for and wins on a judgment call; [`design-system.md`](design-system.md) covers everything that stands still.

Default to CSS: transitions and a few keyframes, driven by the motion tokens in the theme file. No animation library without a reason this document can't meet. Two kinds of motion may be paced from JavaScript: a simulated or streamed reply (it follows the work, not a duration token), and something thrown (a gesture carries speed, which a transition can't).

## Principles

1. **Motion replies to the user.** Every animation answers an action: a press, an open, a toggle, a drag. Nothing moves on page load, and nothing loops unless work is in progress.
2. **The more often it is seen, the less it moves.** Anything driven by the keyboard or repeated all day (moving through a list, typing, switching by shortcut) is instant: a sidebar toggled with ⌘B snaps, even though the same sidebar animates when its control is clicked. Hover and press change a fill or scale slightly, in 120ms. Things opened now and then (menus, dialogs, the drawer) get the recipes below. Nothing in a working product is rare enough to take longer than 300ms.
3. **Fast start, soft landing.** Things arrive with `ease-enter`, which covers most of the distance in the first few frames and then settles. Never use a curve that starts slowly: the user is watching the first frame.
4. **Leaving is never slower than arriving, and plainer.** The user has already decided. Large surfaces leave in less time and with less travel than they arrived. Small things that already take 120ms leave the way they came.
5. **Things come from somewhere.** A popup grows from its trigger (`transform-origin`). Entrances start at 95% scale and zero opacity, or a short distance away (8 to 48px). Never from scale 0, never across the screen.
6. **It can be reversed halfway.** Anything the user can toggle quickly animates from where it is, not from the start. That means CSS transitions; keyframes are for loops and one-shot sequences.
7. **Nothing jumps.** Anything that changes size on screen eases to its new size: a height that grows, and a label that changes width in place.
8. **One entrance per container.** A panel arrives with its content already in place. Don't slide a container in and then trickle its children in after it.
9. **Fast is felt.** A menu at 120ms feels more responsive than the same menu at 250ms, and a tooltip that skips its delay once its neighbour is open makes a whole toolbar feel faster. When something feels slow, shorten it before decorating it.

## Tokens

| Token | Value | Use for |
| --- | --- | --- |
| `--duration-fast` | 120ms | Hover, press, focus, tooltips, menus, selects, and most exits |
| `--duration-base` | 180ms | Changes of height or width, a switch's colour, the dialog scrim, content fading in (a message, a step of a trace, a page of settings) |
| `--duration-slow` | 280ms | Dialogs, the drawer, sliding selection (tabs, switch thumb), the sidebar's width, a second nav column |

| Token | Curve | Use for |
| --- | --- | --- |
| `ease-enter` | `cubic-bezier(0.23, 1, 0.32, 1)` | Things appearing, and things responding to the user |
| `ease-standard` | `cubic-bezier(0.2, 0, 0, 1)` | Things already on screen that move or resize |
| `ease-exit` | `cubic-bezier(0.4, 0, 1, 1)` | Content that leaves ahead of its container. Most exits are a shorter duration on the same curve instead |

A `transition-*` utility with no duration or curve gets `--duration-base` and `ease-standard`. Need a duration or curve that isn't here? Add a token and a row to this table. Never hard-code one in a component.

## Recipes

The values the shared components use. Building something similar: copy the nearest one.

| Component | Enter | Exit | Notes |
| --- | --- | --- | --- |
| Button, tab, nav row | Hover changes the fill in 120ms; press scales to 0.98 | Releases in 120ms | A `press` utility, with `scale` in the transition list. Not on popup triggers or full-bleed list rows |
| Tooltip | After a 500ms hover, 120ms, `ease-enter`: fades in and scales from 95%, from its trigger | The same, reversed | While one is showing, the next opens at once with no delay and no transition |
| Menu, select | 120ms, `ease-enter`: fades in and scales from 95%, from its trigger | The same, reversed | |
| Preview card | 120ms, `ease-enter`, from 95%, from its trigger, after a 300ms hover | The same, reversed, 120ms after the pointer leaves | Opens on keyboard focus too |
| Popover | The menu's values | The menu's values | |
| Dialog | Scrim fades in over 180ms. Panel takes 280ms, `ease-enter`: fades in, scales from 95% and rises 8px | Panel fades and scales to 95% in 120ms | Centred, so it scales from its centre |
| Search (⌘K) | A light dim fades in over 180ms; the sheet fades in from 98% and 4px up in 180ms, `ease-enter`. The list eases its height as results change (180ms); the highlight glides to the next row (120ms, `ease-standard`), measured against the list every time it changes size; a preview beside it fades to the next result (120ms) | Fades to 98% in 120ms | A moment, not a task: quicker and smaller than a dialog |
| Drawer | 280ms, `ease-enter`: arrives from 48px to the right while fading in | Drifts 24px right over 180ms while fading out in 120ms, `ease-standard` | Swiping follows the pointer 1:1. A swipe that dismisses leaves at the speed it was thrown. Backdrop fades in over 280ms, out in 120ms. Moving between views inside it, a step in fades in from the right and a step back from the left |
| Tabs | The fill slides and resizes to the new tab in 280ms, `ease-enter`; the label's colour changes in 120ms | | Tab widths must not change while it slides |
| Switch | Track colour changes in 180ms. The thumb crosses in 280ms, `ease-standard`, its leading edge 40ms ahead of the trailing one, so it stretches slightly | The same, reversed | The thumb doesn't react to hover or press |
| Sidebar | Width over 280ms, `ease-enter`; copy fades over 180ms | The same, reversed | While dragging it follows the pointer with no transition |
| Second nav column | Opens with the sidebar collapsing beside it, as one move: width over 280ms, `ease-enter`. Its rows wait 75ms, then fade in and slide 12px in 180ms | Rows leave in 120ms, `ease-exit`, while the width closes | Always mounted; its content keeps its full width so nothing reflows. Each page fades in over 180ms when picked |
| Height (an error line, a textarea, a step opening) | 180ms, `ease-standard`, to a measured height | The same: it shrinks the way it grew | Nothing animates on first render. Only what's past its foot is clipped, so focus rings near its edges are never cut |
| Width (a swapped name, a filter value, Add becoming Added) | 180ms, `ease-standard`, to a measured width; the new content fades in over the same time | The same | Measure `offsetWidth`, not the bounding box, or a parent that is scaling in reads short. A mark beside the name is outside the swap and changes at once |
| Disclosure (More / Less) | The region opens over 280ms, `ease-enter`, to a measured height, its content fading in. The new label rises 4px and fades in; a plus folds into a minus with it | Closes in 180ms, `ease-standard`, content fading out in 120ms | |
| Words changing in a line | The new words rise 4px into place and fade in, 280ms, `ease-enter`, in the same cell as the old | The old words lift 4px and fade, 120ms | For a value with more than two states. The words before it stay put |
| Fluid hover (rows, menus, selects) | One highlight per list glides to the nearest row on a transform, 180ms, `ease-standard`, taking the row's size and corner; gaps between rows never blink it off. Entering, it appears on the nearest row and fades in, 120ms | Fades out where it is, 120ms | Under reduced motion the glide drops out; it still fades |
| Composer, Home to chat | The composer doesn't travel or remount. Into a chat it fades in at the bottom and rises 8px in 120ms, `ease-enter`; the conversation fades in over 180ms | Back on Home everything is in place at once | A draft and the caret are kept |
| Message | A message that arrives while you watch grows in on its measured height while it fades in, 180ms. The agent's reply waits 180ms, for your message to land, so it reads as an answer. An embed follows its text 100ms later, fading in and rising 4px. Send and Stop trade places, the arriving icon landing from 75% in 120ms | | What is already in a chat when it opens doesn't animate. Follow the thread's own box, never `scrollHeight` |
| Streamed reply | Grows on one measured height (180ms, `ease-standard`) as words wrap onto each new line. However the tokens arrive, a pacer reveals words at a steady rate (about 40 a second, speeding up to 140 as it falls behind), each fading up over 180ms (opacity only) into space the text already holds, so nothing below reflows | | The actions under it are held, hidden, from the first word. Under reduced motion the words simply appear |
| Thinking trace | A step fades in and rises 4px in 180ms as it arrives. The current step reads in the present tense and shimmers (a band sweeping faint text, 1100ms loop) only while the agent works; the two tenses cross-fade in place over 180ms. A step's icon lands from 75% | The shimmer stops the moment work ends | Nothing opens by itself. Folded, the row's words fade to the next step in 180ms; opening and closing ease the measured height (180ms) with the content fading |
| An ask answered | The card folds to one line: its height eases to the line's over 180ms, `ease-standard`, and the line fades in | Undo opens it again the same way | Card and line share one height animation, so the chat below glides up |
| Pill into card | One surface changing size, not one leaving and another arriving: measured width and height ease (280ms `ease-enter` opening, 180ms folding), anchored, clipped so the larger face is revealed. The old face fades at once (120ms); the new one fades and rises 6px a beat behind (60ms) | The same, reversed | |
| Side panel | A FLIP: the panel takes its width at once and slides in from the window's edge over 280ms, `ease-enter`, while the content beside it, rewrapped once at its new width, glides to its place on the same curve. Nothing squeezes frame by frame | Closes over 180ms | Resizes from its edge 1:1 while dragging. Under 1024px it is a wide drawer |
| Suggestions shuffle | Each new row fades in and rises 4px in 180ms, 40ms after the one above; the last lands by 300ms | Replaced at once | The model stagger: a few items, landing within 300ms |
| Toast | The toast library's own motion, left as it ships | | |
| Spinner | The one loop for a control: a steady turn with a 1400ms stretch | | Rests as a short arc under reduced motion |
| Skeleton | A muted bar over exactly the content's box, swept by a soft band on a 1400ms loop. When the content arrives the bar fades out as it fades in, 180ms, `ease-enter`; nothing moves | | A plain bar under reduced motion. Several arriving together come 120ms apart |
| Icon swap (copy to check, play to pause) | Cross-fade in place, 120ms, `ease-enter`: the new icon from 75% and a 4px blur | The old icon to 75% and a 4px blur, 120ms | The one use of `filter` in product motion; icons 24px and under. See P-MOT-08 in `polish.md` |
| A refused entry | Its boxes clear and shake side to side once (`translate` only, 280ms, settling), keyed to each attempt; the first box takes focus again | | Nothing else shakes |

## Rules

- **Movement animates only `opacity`, `scale`, `translate` and a measured `height` or `width`.** Two additions: `clip-path` to reveal something whose size is already known without reflowing the page, and `filter: blur` on an icon swap. Not `margin`, `top` or `left`, and not an unmeasured size, unless it's a listed exception (a sidebar's width, a switch thumb's edges). Hover, focus and selection may also ease `color`, `background-color` and `box-shadow`.
- **List the properties.** `transition-[opacity,scale]`, never `transition-all`.
- **Popups are transitions.** Every popup (tooltip, menu, select, dialog, drawer) is a CSS transition, so it reverses from where it is when toggled mid-animation. Put the transition on the popup and the off-screen state on its starting and ending styles: `transition-[opacity,scale] duration-(--duration-fast) ease-enter data-starting-style:scale-95 data-starting-style:opacity-0 data-ending-style:scale-95 data-ending-style:opacity-0`. Content that only ever arrives and can't be toggled back (a message, a step of a trace) may use a keyframe fade with the tokens.
- **Nothing jumps.** Content that appears, disappears or grows goes through a measured height component. A label that changes in place goes through a measured width component, or cross-fades in one grid cell where the control shouldn't resize. A control that appears on hover is laid over what's there. Never animate to `auto`, and never reset `scrollHeight` on each keystroke.
- **A fold opened low on the page grows upward into view.** While it opens, the page scrolls by however much of it has grown past the foot of the view, so its foot holds still: one move, never a second scroll after it opens. It stops before the row that opened it leaves the top.
- **One menu hands over to the next.** When choosing in one menu opens another, the first goes at once (no exit, no focus return) and the second is there in the same frame with no entrance, in the same place and width, starting at the first one's height and growing to its own. Remount the trigger so an open fill isn't carried over. Check it frame by frame: a 4px shift or a one-frame wrong fill reads as a flicker.
- **Hover never trails the pointer.** On rows and menu items swept past, the highlight is there at once or one shared highlight glides; never a per-row fade-in.
- **Never gate input on motion.** No flags that drop clicks while something animates, no `pointer-events: none` during a transition.
- **Press scale.** Compact controls scale to 0.98 on press. Popup triggers and full-bleed rows don't scale; rows press with the accent fill.
- **Glass.** On a tooltip, the blur goes on an ancestor that doesn't animate, never on the popup that fades: an element below full opacity loses its backdrop and flickers.
- **Motion is never the only signal.** A state that animates also changes a fill, an icon or a label, so it reads with motion off.
- **`will-change` only after you have seen a stutter,** and only for `transform`, `opacity` or `filter`.
- **Stagger rarely.** Only for a staged entrance seen occasionally: a few items, tens of milliseconds apart, landing within 300ms. Never on something opened all day.
- **Loops belong to work in progress.** The spinner, a skeleton's sheen, a dictation wave while it listens, the shimmer on a current step, a working figure. All stop when the work stops and rest under reduced motion. Anything ambient is an exception and goes in the client file.

## Gestures

- **It is the pointer.** While dragging, the element tracks the pointer 1:1 with no transition.
- **A flick counts.** Dismiss on speed as well as distance: a short, fast swipe closes it, and it leaves at the speed it was thrown.
- **Resistance, not a wall.** Dragged past where it can go, it follows with increasing resistance.
- **It keeps hold.** Once a drag starts, it keeps the pointer even when the pointer leaves the element, and ignores a second finger.
- **There is another way.** Anything draggable can also be done without dragging: a close button, a collapse control, arrow keys, a double click back to its default.
- **Reordering a list.** After 4px the row follows the pointer 1:1, the others glide aside (translate, 180ms, `ease-standard`) to show where it will land, and past either end it follows with resistance. Let go, it eases into place from where it was let go (180ms). A keyboard shortcut moves it a place.
- **Thrown things land on a spring.** Held 1:1, then let go into a spring (stiffness around 320, damping a little under critical, about 0.86) at the speed it was let go, settling in about 280ms. Caught mid-throw, it's held from where it is. Write positions straight to the element each frame, never through a render.

## Making something new

When no recipe fits, answer these in order before writing any animation code.

1. **Should it move at all?** If the user triggers it constantly or from the keyboard, no.
2. **What is it replying to?** Say the purpose in one sentence: feedback for a press, showing where something came from, softening a change of size. "It looks nice" is not a purpose.
3. **Which recipe is nearest?** Start from its values and change as little as you can.
4. **How big is it and how far does it travel?** Small and near the pointer: 120ms. A change of height or state: 180ms. A large surface: 280ms.
5. **Is it arriving or already here?** Arriving or responding: `ease-enter`. Moving or resizing in place: `ease-standard`.
6. **Where does it come from?** Its trigger, or a short distance in the direction it lives. It leaves the way it came, with less travel and in less time.
7. **What happens if the user reverses it halfway?** It should carry on from where it is.
8. **Does it still read with motion off?**

Then look at it slowed down in the browser's animation tools (10% speed), in both themes, and while toggling it quickly. Look again the next day.

## Reduced motion

Gentler, not zero. Handle it once in the theme file: under `prefers-reduced-motion: reduce`, fades and colour changes still ease at the token durations, because they help the eye follow a change. Movement does not: `translate`, `scale` and size are dropped from every transition, and keyframe animations are cut to a single frame. A dialog still fades in; it no longer rises or scales.

Anything built with CSS transitions and the tokens complies without extra work. Check yourself:

- Motion driven from JavaScript (a drag, a measured height) must not wait for a transition to finish.
- Motion run from script reads its timing from the theme through a helper, never `parseFloat` on the token: a production build can write 280ms as `.28s`, and read as a number that's a fraction of a millisecond.
- A keyframe animation that carries meaning needs a resting state, as the spinner's short arc does.
- The state must be readable without the movement.

## Earned exceptions

A moment may break a rule above (run longer, loop at rest, play on load, rotate) only when all of these hold:

- **It's rare.** Seen a handful of times per user: a reward for a setup step, making something once per workspace. Never on a screen used all day.
- **It's a picture, not a control.** Nothing waits on it to be pressed.
- **It shows something.** What connecting a tool would do, what the agent is about to become. Not decoration.
- **It plays only when invited**, where it can (while pointed at), and stops the moment attention leaves.
- **Each movement still uses the tokens.** Only the hold is long.
- **It has a reduced-motion version** that is simply the end state, still.
- **It's written down.** Every exception is named in the client file with where it lives, so nothing else borrows its timings.

Typical examples: an onboarding reward, a once-per-workspace reveal, a cursor storyboard on hover, a self-typing composer, an ambient map of agents.

## Not covered yet

- Toast motion is the library's default, not our tokens.
- Whether toasts with an error or an action should stay until dismissed is undecided.
