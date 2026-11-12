# Taste profile

What good looks like in a Turbo product. Read it before designing, building or reviewing UI. It outranks every skill and personal preference; [`design-system.md`](design-system.md) says how to build and [`motion.md`](motion.md) how things move; this says what to aim for. A client file in `clients/` sets brand values on top of it.

**What we build:** AI-native work tools. People live in them for hours, so they should feel calm, fast and exact: closer to a well-made instrument than a marketing page.

## What we are striving for

Craft is the product. An AI product asks people to hand real work to an agent, and they judge whether it can be trusted by how the interface feels long before they can judge the work. So every screen should feel:

- **Light.** A white page, neutral ink, and as little drawn around the content as it can carry. If something feels heavy, it is usually a box, a shadow or a second background that doesn't need to be there.
- **Calm.** One thing to look at first, and room around it. Nothing competes with the work.
- **Friendly, never a circus.** Warm and approachable through shape, spacing, words and a little colour used with intent. Never through ornament for its own sake.
- **Fast.** It answers at once. Motion is short and always a reply to the user.
- **Exact.** Edges line up, corners nest, numbers hold still. The details nobody consciously notices are the ones that make it feel made with care.

**Where we look.** The best working tools: white pages, quiet tables and soft tags; AI-native tools with almost nothing drawn around the content; restraint, speed and calm density. We borrow principles, never pixels.

**The principle.** Craft is care in the details, not emptiness. Start from what the person needs on this screen, give it a clear place and enough room, and then get the small things right: the spacing, the corner, the word, the reply to a click. Take things away before adding them: a row on the page before a card, space before a divider, a hairline before a shadow. A calm, neutral base is what lets a tag, a card or a moment of motion land when it matters.

## The five rules

1. **Quiet by default.** The page is white, and so is the sidebar beside it: one surface, parted by a hairline. Most content sits straight on that page, and a card is the same white, set apart by a hairline and a soft shadow. In the dark theme nearer surfaces are a step lighter instead. Hierarchy comes from the three-step ink ramp and weight (400 body, 500 for labels and headings; 600 is reserved for numbers that must shout), not from boxes or big size jumps. If a screen looks busy, remove a container before adding anything.
2. **Dense, with room to breathe.** 32px controls, 14px text, 13px for metadata. Tight inside a group (2 to 8px), generous between groups (16 to 32px). Density is a feature; cramped is a bug.
3. **Not everything in a box.** A list, a settings section, a table and a trace all sit straight on the page; a row shows a fill only when it is hovered. A card is for the one thing that should stand apart: the composer, something an agent made, an approval. A control (an input, a select, an outline button) has a hairline and no shadow. Something that floats (a menu, a dialog, the drawer) has a larger soft shadow and no edge. Every shadow is soft, with far more blur than offset. Never a hard drop shadow.
4. **Colour means something.** Ink is the primary action. Blue is selection, focus and "on". Status is a small set of tags: a light, clearly coloured fill with deep text of the same hue. Chrome and controls stay neutral so that colour lands where it is used. Colour with a job is welcome; colour as filler is not.
5. **Motion answers the user.** Every animation is a reply to an action: press scales down, menus grow from their trigger, selection slides, dialogs rise. 120 to 280ms, eased out, never bouncy for its own sake, and nothing loops unless work is in progress. And nothing *jumps*: anything that changes size on screen (an error line appearing, a name changing in a sentence, a row expanding) eases to its new size. Measure and animate to a value; never animate to `auto`.

## Details that separate crafted from sloppy

**Type**
- Two families: a heading face (the client's, set in their file) and a neutral text face for everything else (Inter by default). Headings share one tight tracking and the text face tightens slightly as it grows; the text styles do this, not the call site.
- **A page names itself in two lines.** The title, and under it a short line at the same size in the faintest ink that says what the page is for: "Providers / What your agents can use". One heading, two inks, no separate description.
- Headings use `text-wrap: balance`, paragraphs `pretty` (set globally).
- Numbers that change or line up use `tabular-nums`.
- Sentence case everywhere. No uppercase labels, no letter-spaced eyebrows. A group label over a list is small, medium and muted.
- Secondary text is the muted ink. The subtle ink is for placeholders, counters, separators and the subline of a page heading.

**Layout**
- **One spine.** Page text and the content inside rows and surfaces share one left edge and one right edge. A row, a table or a card hangs past that edge by its own padding, so its content, not its container, sits on the spine: the page title, a logo in a row and the first cell of a table all start on the same line. This is optical alignment, not mathematical. Controls (buttons, tabs, inputs) sit on the spine with their own edge, and surfaces inside a drawer or dialog don't hang. Where the page gutter is too narrow to hang into, it stays inside it.
- **One column per area.** Pages in the same area (every settings page) share one width, so the title and the top of the content stay put as you move between them.
- **Fixed columns in a table that can be filtered.** Set the widths on the headers, so nothing shifts as rows come and go.
- Icons sit optically centred on the first line of text.
- **Concentric corners.** When one rounded thing sits inside another, outer radius = inner radius + the padding between them. Never equal radii on nested surfaces: that reads as a thick, uneven rim. Default pairs: menu 16 = item 12 + 4 · a 32 or 36px mark (12) with 6px around it in an 18 row · that row 6px inside a 24 card · a 20px mark (8) with 6px around it in a 14 row, 4px inside an 18 menu · a 20px mark (8) 2px inside a 24px chip (10) · a filter token (12) with its pill (8) 4px inside. When the padding is over about 24px the layers are separate surfaces and each picks its own radius. Buttons are exempt: they stay pills inside any container.
- **Tile rows.** When a mark sets the height of a row, the padding above, below and on the mark's side is equal. The row's radius is the mark's radius plus that padding. Whatever sits at the end of the row (a plus, a menu button) is the same distance from the top, the bottom and the end.
- One primary button per view. The next action down is `secondary` (the quiet fill). Dismissals and tertiary actions (Discard, Skip, an inline Cancel) are `ghost`; a dialog's Cancel is `secondary`. `outline` is for controls that sit in a row of outlined fields (a Filter beside a search and a select).
- Group with space first, a group label second, and a divider only where space can't carry the structure. Prefer rows over cards, and never cards inside cards.
- Most important first: top and leading edge. In a row, the name leads; metadata and actions trail. The order you see is the order underneath, so keyboard and screen readers meet things in the same sequence.
- Hidden content shows a cue: a disclosure control, or the next item peeking past the scroll edge. A truncated value the user needs has a way to the full text.
- More than three secondary actions go behind a menu.
- Anything you can press looks pressable: a fill, an edge, an underline, or a place where controls always sit. A whole row that can be pressed shows a fill on hover and something at its end that says what pressing does (a faded plus that darkens).
- Controls in a row share one height (32px by default) and one treatment: all outlined, or all quiet.

**Surfaces**
- **White, and flat.** Fills are matte. The page, a card and a field are all white; depth comes from a hairline and a soft shadow, never from a tinted layer or an effect.
- **Rows before cards.** A list is rows on the page under a small label. A settings section is a heading over its fields. A table is a soft header bar over plain rows, with no box and no rules between them; a hairline under the last row and a count say where it ends.
- **A card earns its place.** At most one or two on a screen, for the thing that should stand apart. Its edge is a 1% hairline that is part of its shadow, so the two read as one soft edge, never a border with a shadow beside it. Rows inside it sit 6px in and fill, rounded, on hover.
- **Dividers are a last resort.** Spacing and alignment first. They are hairline, pale and never run edge to edge: a divider between two areas starts and ends level with what is beside it, and a menu's divider is inset to its items.
- Page, then card, then overlay. Don't skip levels, and don't invent new ones. Inputs are white with a hairline, the same in a row of filters as in a form; in dark they are a faint lift with a hairline.
- **Two themes, one design.** Dark is the light theme's structure with the values mirrored: nearer surfaces are lighter, ink is near white, fills and edges are white at low alpha, and the primary button is still ink (so it turns near white). Dark is flat: resting surfaces cast no shadow and have no hairline, so a card is told apart by its lighter fill alone, and only what floats has a shadow, several times stronger than in light. Both themes use one grey ramp with no colour in it: paper at the top, charcoal at the bottom, never tinted and never pure black. Tooltips and toasts stay dark in both themes.
- **The shell.** Navigation is on the canvas with an 8px margin. The page shares the sidebar's fill and runs to the window's edge: no frame, no corner, no shadow, only a hairline between the two that starts and ends level with the sidebar's marks. Elevation is kept for what sits on the page: the composer, and the cards an agent puts in a chat. A top-level page has no header bar and names itself in the body; a page that belongs to something keeps a 44px bar for its breadcrumb and actions, and the page goes soft as it scrolls up under it, never against a hard edge. A long form keeps its last actions (Cancel, Create) at its end; until you get there they float at the foot of the page over a fade from the page's white and settle into place at the end, so they never jump between the bar and the form.
- Fills are ink at low alpha (hover 3%, muted 5%, accent 8%) so they read the same on every surface. A control's hairline is ink at 6%, and it is there at rest only: hovered or open, the control shows its fill and the hairline goes. Strokes on controls are inner borders. Filled buttons are flat: no highlight, no drop.
- Menus and selects are opaque with the overlay shadow: white in light, the raised fill in dark, and no edge in either. The shadow sets them apart.
- **A logo that is its own square** (a brand colour filling its whole square) takes a small rounded corner wherever it's shown alone, so it reads as a rounded square beside the others; on a tile, the tile's corner clips it.
- **Tags are rounded squares.** A badge and a chip take a small corner that follows their height, not a pill. Each colour comes strong (the full tint: a state to be seen, like Active) and soft (the tint at 40%: a value picked out in a sentence). The colours are one family: the same lightness for every fill and for every text, with only the hue changing. A chip that names a person, an account or an agent is its mark and its name together on the quiet fill.
- The sidebar is part of the canvas, not a surface. Rows are full ink at rest; the active row is a fill, never a filled icon. It resizes from its edge and collapses to a 40px rail. A second column of navigation (Settings) opens beside the rail, the same width and rhythm, behind a hairline.
- Tooltips and toasts share one surface, inverted glass (translucent dark ink over a blurred backdrop, in both themes), with white text. A toast says one thing: a title for what happened (Skill added), and where it helps, one line under it in the faded ink, a step smaller, naming what it happened to, never a sentence; and an action if there is one.

**Interaction**
- Hover changes fill, not size. Press scales to 0.98: buttons, tabs, nav rows and other compact controls. Not popup triggers, not full-bleed list rows.
- Focus is always visible: a 2px blue outline, or the blue edge and halo on text inputs.
- Icon-only buttons get a tooltip and an `aria-label`. One hover for everything with no fill of its own (rows, ghost and icon buttons, menu items). Selected and open rows take the muted fill; pressed takes the accent fill.
- **A control that changes in place stays in place.** A button that has done its job becomes its own result where it stands (Add becomes Added, in the soft green, and eases to the new width). An ask that has been answered gets out of the way: an approval card folds to one line in the chat's history ("Approved calendar access", with Undo where it can be undone). An ask that can no longer be answered as it stood (it expired, timed out or failed) keeps its card with one way on (Ask again), under a strip that says what became of it, never the card with a dead button. A name that changes inside a sentence eases to its new width so the words after it glide over.
- **Show less until asked.** Something only needed now and then (the way to remove a filter) appears on hover and focus, laid over what is there rather than pushing it aside, and is always there on touch.
- Glass (blur) belongs to the inverted glass of tooltips and toasts, and at most one other surface that has to stand out from a quiet column. Nowhere else.
- Disclosures animate height both ways.
- Settings that apply immediately use a Switch; settings that need review use a form with Save.
- Destructive actions are quiet until the confirming step, which is the red tint with red text: firm, never a loud solid red.
- **Dictation is a tap, not a hold.** The microphone sits beside send. One tap and it listens: the microphone stays lit, a short wave grows in beside it, and the words land in the field as they are heard, fading in, the unsure ones muted until they settle. Tap again to stop and read it over, or send to send it at once.
- Show shortcuts where they help (tooltips, menus), using a `Kbd` component.

**Writing**
- Short, plain, specific. Use words a tired reader gets on the first pass, and cut every word that does no work.
- Name the action. Buttons start with a verb ("Send", "Save draft"), and a button on a page that is already about the thing can be the verb alone ("Add" on Providers). In a dialog they are verb plus object ("Delete agent", "Cancel"), never "OK", "Yes" or "No".
- One vocabulary. Read the copy around your change first, and use the same noun and verb for the same thing everywhere: if the row says "Connect", the toast says "Calendar connected" and the group is "Connected".
- A headline and a page subline take no full stop.
- Write to "you", never "the user". No "we" in an error; it reads as an excuse.
- Errors say what didn't happen and how to fix it, beside where it broke: "Couldn't save changes. Check your connection and try again." No blame, no "oops". If a requirement is known in advance, show it as help text before the error can happen.
- Confirmations can be answered from the title alone: it names the action and the object ("Delete the Support triage agent?", "Want me to send this renewal offer?"), never "Are you sure?". The body says what is lost, or what will be done.
- Undo beats confirmation. Something reversible and frequent (archive, disconnect, approve) happens at once, with Undo in the toast. Confirm only what can't be undone, affects other people, or touches many items.
- Empty states point forward: say what this place is for and offer one next action. A filtered empty state offers a way out ("Clear filters").
- Status copy is parallel: "Saving changes…" becomes "Changes saved". Never "successfully". A partial result gives both counts.
- A switch's label describes what happens when it is on. A link names where it goes, never "Click here" or a bare "Learn more".
- Tone follows the stakes: it can be warm on Home and in onboarding ("Good morning"), neutral in settings, and is calm and plain in errors and anything destructive.
- No helper copy that explains the product to itself ("Save a task once, run it on a schedule"). If a real product wouldn't print it, cut it.
- No exclamation marks, no "please", no filler.

**AI moments**
- **The work is shown as a trace.** In a chat it is folded to one quiet row above the answer: a chevron and a short line about the work, not each step ("Used 2 tools", or "Thought for 4s" when it used none), muted, taking the hover fill. While it works the chevron's place is a small working figure and the words are the current step, shimmering. Opened, each step is a compact row on the chevron's column, narrow (a glance, not a table): a 16px mark (the tool's logo when it worked in one; an icon for the kind of work otherwise) and what it did, in regular full ink. Rows hang past the text by their padding, so the marks sit on the text's edge whether a row is lit or not. A step with more behind it opens too: on hover its mark gives way to a chevron, and the detail hangs under it off one hairline, in muted ink. The trace sits 8px above the answer. Blue stays out of it: blue is for what you can press.
- **Unfolded, the trace is the page.** Where the steps are the point (setting up an automation), what the agent did sits as steps down the page, each one line led by an icon for the kind of work, joined by one hairline. While it works, steps arrive one at a time; the current one reads in the present tense ("Searching…") and shimmers, then settles into the past ("Searched…"). Nothing opens by itself while it loads. Then the trace rests and the answer streams in.
- **The answer is plain text on the page.** No bubble around the agent's words. What you sent sits to the right on the blue tint.
- **What it made, and what it needs, are cards in the conversation.** A document it produced is a small card with the tool's logo and one action. A yes it needs before acting is a card you can answer from its title: the action as a sentence with the names picked out as chips, the start of what it would send, how sure it is, and the two things you can do.
- **Asking to go further is the easy yes.** Where the product asks to be let in somewhere, the default is the wider answer and the button names the place, not the scope (Add to workspace chat, with the line saying where). The narrower answer is a press away beside it; the line follows the pick, and the button's words never change.
- **Saying yes in a chat is your turn.** Accepting a card the agent offered sends that as your message, and the card folds away. The agent answers in one reply: its trace holds the work, and its words are the job it asked for this for, done. The card never shows its own progress.
- **Useful the minute it's in.** When something is connected, the agent doesn't ask what you'd like to do with it. It does the first useful job by itself, from what made it worth connecting, and comes back with it done: the trace, what it found in a sentence or two with the numbers, what it made as a card, and what it will keep doing. No closing question.
- **Outside a conversation, it suggests what would let it do more.** Small and low, never competing with the composer. Say plainly what it would let the agent do, the work that made it ask, and an action that names what's connected ("Connect sandbox"). Real words and numbers in any picture of it, no skeleton lines. Saying yes is one click, never a prompt to copy. "Not now" sends it away.
- **When it needs your yes and you're elsewhere, it asks once, in one place.** The agent's mark and the chat it's asking from, the ask as a question, what yes lets it do, and the yes as one button; the chat's name is the way into the chat. It's there only while there's something to answer, and hides while its chat is open, where the same ask is a card.
- **Watching never takes over.** While the agent uses a computer, its screen is a picture-in-picture window in a corner, so you can watch and keep typing; the chat has a line saying what it's doing. It can be dragged or flicked to another corner and resized from its free corners.
- **Search finds everything, and asks.** One search for the workspace (⌘K), a light sheet near the top over a page dimmed only a little: the field first, results in groups, the keys at its foot. Every row is one line on one left edge, and nothing names a row's kind when its group already does. Empty, it offers where you were and what people start most. Typed into, the best group comes first; past the list of names, it offers a deeper search and a way to just ask. The lit row says what Enter does there.
- **The agent is named where you can change it.** A chat stays with the agent it was started with, headed by the agent's mark with the chat's name in a chip under it.
- No sparkle overload, no rainbow gradients, no glowing borders.
- Streaming and thinking states animate gently and stop the instant the work stops.

## Don't

- `dark:` classes, or a component that checks which theme is on. A theme is a block of token values; if something looks wrong in dark, fix or add a token.
- A design that only works in one theme. White that is hard-coded, a shadow doing the work of an edge, colour that only reads on a light surface.
- A box by default. A card round a list, a panel round a settings section, a border round a table.
- A tinted layer behind content to set it apart. The page is white; use a hairline and a soft shadow, or nothing. One exception: a card in a reply that asks for something may have a muted strip above it that says why, the card's width and tucked behind its top edge, never wrapping it. When it says what became of an ask, it takes that status's soft tint.
- A border and a shadow side by side on one surface. The hairline is part of the shadow.
- A divider that runs edge to edge.
- Decorative gradients or neon accents. A gradient is allowed only where it does a job: a fade at a scroll edge or under a sticky bar, the shimmer on work in progress, a skeleton's sheen, the fill under a chart's line, a colour picker. Each project lists the ones it uses in its client file; anything not on the list is out.
- Blur anywhere except the inverted glass of tooltips and toasts, and the one surface a client file names.
- Texture, except a faint grain where a colour has to read as a material. Named in the client file, or not used.
- Pure black text, or a grey that isn't on the ramp. Ink is soft charcoal, and every grey is a named step of the one ramp both themes share.
- Radii above 24px. 24 is for dialogs, the composer and large wells; cards are 18. Pills are for buttons; tabs, tags, inputs, menus, rows and surfaces keep a corner.
- A shadow on a control.
- A filled icon to mark the active item. The fill behind the row does that.
- More than one accent colour on a screen. Tags are status, not accents: don't use their colours to decorate.
- A second line on a toast that is a sentence, or a description under a page heading as well as a subline.
- Centred layouts for working screens, oversized headings, hero-style spacing, decorative avatars or mascots on records.
- Animation longer than 300ms, or any animation on page load that the user didn't ask for.
- Emoji as icons.

## Before you ship

- [ ] Squint: is there one clear thing to look at first?
- [ ] Could a container, border, label or colour be removed without losing meaning?
- [ ] Is anything in a box that could sit on the page?
- [ ] Do all controls in a row share a height and a treatment?
- [ ] Does the content, not its container, sit on the page's spine?
- [ ] Works by keyboard alone, and at about 375px wide.
- [ ] Only tokens: no raw colours, sizes or durations.
- [ ] Reads the same in light and dark.
- [ ] Motion replies to an action and is still fine with reduced motion on.
- [ ] Copy is sentence case, short, and names the action.
- [ ] The review pass in `polish.md` is clean.
