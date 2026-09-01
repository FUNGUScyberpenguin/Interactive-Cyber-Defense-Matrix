# Interactive-Cyber-Defense-Matrix

An interactive version of the Cyber Defense Matrix, aligned to **NIST CSF 2.0**.

## ▶ [Try it live](https://funguscyberpenguin.github.io/Interactive-Cyber-Defense-Matrix/)

No install, no sign-in and no network calls — it runs entirely in your browser,
including the fonts, and your board is kept in that browser's local storage. Use **Export → JSON** to save or share a
board, and **Load JSON** to pick it back up.

To run it locally instead, clone the repo and open `index.html` — everything
(layout, export, persistence) is client-side with no build step and no
dependencies.

## Structure

Columns are the six NIST CSF 2.0 Functions — **Govern, Identify, Protect,
Detect, Respond, Recover** — and rows are the Cyber Defense Matrix asset
classes — **Devices, Applications, Networks, Data, Users**.

Each control is tagged with the CSF 2.0 Categories it supports (`GV.SC`,
`PR.AA`, `DE.CM`, …) and with a list of **aliases**: the equivalent market
terms for the same capability. One control therefore covers a whole family of
vendor labels — `SASE / SSE` also matches SWG, CASB, FWaaS, SD-WAN and
protective DNS; `Immutable Backup` also matches DRaaS, tape, VM/DB backup and
air-gapped recovery. Aliases are searchable, shown under each control in the
sidebar, and included in JSON exports, so a matrix reads the same whichever
vocabulary a client uses.

Controls that apply to every asset class (governance, SIEM, incident response)
are drawn as vertical bands spanning their Function column. Asset-specific
controls are horizontal bars inside their cell.

## Getting a board populated

Start with **What do you run?** in the sidebar. Type a product, pick it, and
the matrix marks every control that product answers and records it there. Nine
products *proposes* a quarter of the board in under three seconds — a CISO
knows their stack cold and would need an hour to click through fifty-nine
controls. Suites work too: `Microsoft 365 E5` expands into its component SKUs
rather than being forced into one category it doesn't fit.

A product you type that isn't in the library is not a dead end. It asks which
category, and the picker searches the way a CISO talks — `pam`, `sso`, `waf`,
`xdr` all resolve, because it matches control aliases and not just the analyst's
name for the market.

### Owning a product is not operating a control

Naming a product **proposes**; it never asserts. Those controls land as
**Unconfirmed**: drawn hatched, counted in the denominator, and deliberately
excluded from current coverage until a human says the control actually
operates. A backup licence is not evidence of a rehearsed DR plan, and a
compliance subscription is not evidence that policy exists — that gap is the
whole job, and it is the first thing a client's auditor goes after.

Confirming is one click for the sweep (**Confirm all N** under the stack) or
one click per control in its detail panel, which asks the question directly and
offers *Gap* as the other answer. Confirming is easy; automatic is impossible.

Every control starts **not assessed**: visible on the grid in neutral, carrying
no judgement. That way the matrix teaches the framework the moment it opens
rather than showing an empty six-column box. Mark a control *not applicable*
and it drops off the grid; that is the only thing that hides one.

Coverage is scored over every **applicable** control, so an unassessed or
unconfirmed control counts as not covered. A board with fifty controls
untouched reads 27%, not 100% — the number a client sees is the real one.

## Using it

* **Status** — mark each control Current (blue), Planned (green), Gap (red), or
  leave it off. Off means *out of scope for this client*, not a zero: coverage
  is only ever scored over the controls you scoped in, so a company with no OT
  is not punished for owning no OT security.
* **Detail** — click a control name to open its panel: the CSF 2.0 Categories
  it supports, the full alias list, a **P1/P2/P3 priority**, and a **notes**
  field for findings, scope caveats, owners and evidence. Notes and priorities
  travel with every export.
* **Group by** — Function, asset class, or a flat A–Z list.
* **Filter** — matches control names, aliases, CSF Category IDs and asset class.
* **Profiles** — Table stakes, Insurance ready, Commonly seen and Everything
  pre-fill a maturity profile in the colour you pick. They are checklists, not
  percentiles: an opinionated set, not survey data. *Commonly seen* was called
  "Industry Average", which claimed a population nobody here has measured;
  *Everything* was called "Regulated / Bank", which implied a regulatory basis
  it never had — it is every control in the tool, so it always tracked overall
  coverage exactly. `TEMPLATES` in `index.html` says where each one comes from.
* State persists in `localStorage`; boards from the earlier control list are
  migrated on first load.

## Two views

A toggle in the header switches what the bars are named after. The bars
themselves never move, so a control keeps its position and your eye keeps its
place across the switch.

**Controls** — the assessed control set, coloured by status. The gap analysis.

**Vendors** — the products, named at the control they answer. Plotting the
product landscape onto the matrix is what Sounil Yu built it for: where the
stack is dense, where it is empty, and which vendors span half the board.

Products are recorded against the control they deliver (open a control, fill
in *Products in place*), so they inherit that control's position and the views
stay one dataset rather than two. **Stack concentration** below the matrix
ranks products by how much of the board each one covers — the consolidation
conversation, and the concentration risk.

**Sample vendors** is a working-view switch and is **off by default**. On, it
fills any control you have not recorded a product against with common products
for its category — each one hollow, dashed, grey and prefixed `e.g.`, so a
suggestion cannot be mistaken for something the client owns. It is off by
default because on, it puts two named products against every gap on the board,
which is a procurement recommendation however it is captioned — and a client
screenshots the picture, not the tooltip.

There used to be a third view, **Both**, which printed the sample inline in the
control's own label. That made a suggestion and a fact pixel-identical, so it
was removed rather than patched.

**Overlay** is the other way round: it keeps the control view and lights up
only the controls a chosen product covers, dimming the rest. One product
lighting up nine controls is either good consolidation or a single point of
failure, depending on which nine. Its caption names what is shown, and the
dimming is written inline so it survives into the exported picture.

## Adding a vendor

The list will never be complete, and it does not need to be — this is open
source, so a missing vendor is a one-line pull request.

`VENDOR_LIBRARY` in `index.html` is a flat map of market category to product
names:

```js
'SIEM': ['Splunk', 'Microsoft Sentinel', 'Google SecOps', …],
```

Add the product to the category it sells into. That is the whole change: the
category already knows which controls it answers and where they sit on the
grid, so a new entry is immediately searchable in **What do you run?** and
lands in the right cells.

A suite that spans several categories goes in `PRODUCT_BUNDLES` instead, as a
list of the component products it expands into — forcing one category on a
bundle would be wrong, and the consultant can trim what the client does not
actually light up.

Three conventions worth keeping:

* **Order is not a ranking.** The first three show on the control as samples,
  so put the most widely recognised first — but nothing here is a shortlist,
  a recommendation, or a top three.
* **One product, one category** unless it genuinely sells into several.
  Listing a vendor everywhere makes the concentration analysis meaningless.
* **Use the product name a buyer would type** — `Cortex XDR`, not
  `Palo Alto Networks Cortex XDR`.

If the category itself is missing, that is a bigger change: see
`docs/category-provenance.md`, which records what each category name is worth
and where it came from.

**You do not have to wait for a pull request to use an unlisted vendor.** Type
it into **What do you run?**, pick its category when asked, and it goes on the
board immediately — it just will not be there for the next person until it is
contributed back.

## Where the vendor mapping comes from

Each control carries a market category, and `VENDOR_LIBRARY` lists well-known
products per category, offered as one-click suggestions when you fill in
*Products in place*. The chain is:

    vendor → market category → control → grid cell

Those two halves rest on very different ground, and it is worth being clear
about which is which.

**Category to cell is standards-backed.** The grid is Sounil Yu's Cyber Defense
Matrix over NIST CSF 2.0 Functions and asset classes, every control is tagged
with the CSF 2.0 Categories it supports, and
[CIS Controls v8.1](https://www.cisecurity.org/controls/v8-1) — which realigned
to CSF 2.0 and added Govern — publishes an
[official mapping](https://www.cisecurity.org/insights/white-papers/cis-controls-v8-1-mapping-to-nist-csf-2-0)
to the same framework.

**Vendor to category is not.** There is no free authoritative register of which
vendor sells into which category. The comprehensive ones are commercial:
[IT-Harvest](https://it-harvest.com/) tracks 4,000+ vendors and 11,300+
products mapped to CSF 2.0, MITRE ATT&CK and CIS Controls by subscription, and
Gartner and Forrester define the category names most of the industry uses.

Gartner **Peer Insights** does publish a public per-market vendor directory,
and Magic Quadrants and Forrester Waves name vendors — but a curated market
vendor list is the thing those firms license, so reproducing one here would not
be right. Linking out per category would be, and is worth adding once the
market URLs can be checked against the live site.

So `VENDOR_LIBRARY` is a **starting point, not a source of truth**: common
examples per category, not a ranking, not an endorsement, not exhaustive.
Vendors get acquired, renamed and repositioned constantly — check it before it
goes in front of a client, and edit it freely. Typing a product that is not in
the list works exactly as well; the library only saves typing.

Categories that are advisory or architectural rather than a product purchase —
vCISO services, zero trust, breach notification — are deliberately left empty.

## The dependency continuum

Beneath the grid, in the working view, is the continuum from the original matrix:
reliance on **technology** is heaviest at the left and falls away to the right,
reliance on **people** does the reverse, and **process** stays level throughout.
The left-hand functions are *structural* — always running — while the
right-hand ones are *situational*, invoked by an event.

It is there because it changes how the coverage numbers read. A thin Recover
column is not a shopping list; that end of the matrix is where people and
rehearsal matter more than product.

It is deliberately **not on the exported picture**. On screen you talk over it;
on a static export, three full-width bars in the same palette as the status
colours sitting directly under a colour-scored grid read as "technology 100%" —
and that misread flatters the client's posture, which is the opposite of what
an assessment is for. The export uses that space for the coverage figures and
the key instead.

The Cyber Defense Matrix is Sounil Yu's framework — see
[cyberdefensematrix.com](https://cyberdefensematrix.com/) and his book
*Cyber Defense Matrix: The Essential Guide to Navigating the Cybersecurity
Landscape*. This repository is an independent interactive implementation.

## Coverage

The sidebar carries a scoreboard: coverage per CSF Function and overall. Each
bar shows current coverage in solid blue, the planned position behind it in
green, and — when the stack has proposed controls nobody has confirmed — a
faint blue edge for what the board *would* read if they all checked out. The
distance between the edges is what the roadmap still owes, and what is still
only claimed.

This is the point of the Cyber Defense Matrix. A board that reads *Protect 59%,
Respond 20%, Recover 25%* is telling you where the money went and where the
recovery conversation has not happened yet, and it says so on the picture the
client gets handed rather than in someone's notes.

**Benchmark** compares the board against each profile — current, plus what the
plan would add — and **Show N missing** filters the sidebar down to exactly the
controls that profile still wants.

## Vendor lookup

Each control carries the **analyst market category** a CISO already reads in
(`CNAPP`, `WAAP`, `ITDR`, `DSPM`, `PTaaS`), tagged with how much weight that
name carries — Magic Quadrant market, Market Guide, emerging Hype Cycle entry,
or merely a common industry term with no analyst market behind it. Four
controls carry no category at all, because security leadership, zero trust,
breach notification and network restoration are advisory or process work
rather than a product purchase.

Three **sample vendors** per category make the matrix answerable by recognition
rather than recall — a CISO knows their own stack cold and a control taxonomy
only vaguely. These are samples in Gartner's sense of the word: not a ranking,
not a shortlist, not exhaustive, not an endorsement.

The panel also says, in words rather than a tooltip, what the tier code means
and whether that category name was **✓ verified** against a published source
during this work or is **not re-verified** and carried from general knowledge.
Two of the forty-seven are verified. `docs/category-provenance.md` has the
detail; the panel has the warning, because nobody opens a markdown file across
a conference table.

The detail panel also links to the
[Charting Cyber vendor catalog](https://www.chartingcyber.com/vendors), along
with a few adjacent category names worth a look.

Categories rather than a baked-in vendor list, deliberately: the list needs no
upkeep as the market churns, and the tool never reads as a recommendation.

The catalog filters client-side with no shareable query string, so the link
opens the catalog and the terms sit beside it as the vocabulary to filter on.
If a search parameter ever appears, set `VENDOR_SOURCE.SEARCH` in `index.html`
and the terms become direct links.

**Named vendors stay off the client-facing exports.** The PDF gap register
carries the market category for each gap — so the client can see what *class*
of product it needs — but the vendor names themselves live only in the working
UI and the CSV. An assessment that arrives with product names already on it
reads as a sales document; the vendor conversation is a separate one, after the
gaps are agreed. (The PDF used to grow a Products column the moment a board
recorded anything, which is always, since intake is the front door — so it
contradicted this rule on every real board. It no longer does.)

## Exports

The exported picture is what survives the meeting: it gets forwarded,
screenshotted, and read back a year later by someone who was not in the room.
So it carries the client's name, the date, your firm's name if you set one, the
coverage figures per Function, a five-swatch key, and the credit to Sounil Yu.

* **PNG** — the matrix with all of the above. No client name set, and it says
  *Unnamed client* rather than pretending.
* **PDF Report** — the matrix, then a written gap register: **every applicable
  control**, including the ones nobody has assessed yet, with status, priority,
  Function, asset class, CSF Categories and your notes, ordered P1 first and
  open gaps ahead of scheduled work. Self-contained, no libraries. The row
  count matches the "N controls applicable" in the heading, because a client
  who counts them and finds a shortfall has found a hole in the one number the
  assessment turns on.
* **CSV** — the same register for Excel, where roadmaps actually get built.
* **JSON** — the full board including coverage figures, notes and priorities.
  Loading one restores it exactly — *not applicable* included, which used to be
  dropped on load, silently moving the percentages between Friday's export and
  Monday's reload. Exports from the pre-CSF-2.0 control list are remapped
  automatically.

Client and firm names persist with the board, so a reload cannot leave you
generating `CDM-CDM-report.pdf` for a board you spent an hour on.

## Editing the control list

`CONTROLS` in `index.html` is the single source of truth. Add an entry with an
`id`, `label`, `fn` (Function column), `rows` (`'all'` or `[first, last]`),
`alias`, `csf` and a `vendor` market category (empty if it is advisory rather
than a product purchase), and `layoutControls()` places, sizes and wraps the
bar for you — there is no SVG to hand-edit. Controls covering every asset class
become one vertical band; the rest are chips in each cell they cover. A control
whose coverage is not a contiguous run of rows goes in `COVERAGE_FIX`, which
the `[first, last]` form cannot express.
