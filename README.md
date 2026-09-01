# Interactive-Cyber-Defense-Matrix

An interactive version of the Cyber Defense Matrix, aligned to **NIST CSF 2.0**.

## ▶ [Try it live](https://funguscyberpenguin.github.io/Interactive-Cyber-Defense-Matrix/)

No install and no sign-in — it runs entirely in your browser, and your board is
kept in that browser's local storage. Use **Export → JSON** to save or share a
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
* **Templates** — Table Stakes, Insurance Ready, Industry Average and
  Regulated / Bank pre-fill a maturity profile in the colour you pick.
* State persists in `localStorage`; boards from the earlier control list are
  migrated on first load.

## Three views

A toggle in the header switches what the bars are named after. The bars
themselves never move, so a control keeps its position and your eye keeps its
place across the switch.

**Controls** — the assessed control set, coloured by status. The gap analysis.

**Vendors** — the products, named at the control they answer. Plotting the
product landscape onto the matrix is what Sounil Yu built it for: where the
stack is dense, where it is empty, and which vendors span half the board.

**Both** — controls with their products underneath.

Products are recorded against the control they deliver (open a control, fill
in *Products in place*), so they inherit that control's position and the views
stay one dataset rather than three. **Stack concentration** below the matrix
ranks products by how much of the board each one covers — the consolidation
conversation, and the concentration risk.

**Market examples** fills any control you have not recorded a product against
with common products for its category, drawn muted so a suggestion is never
mistaken for something the client owns. Untick it for a clean client view of
only what they actually have. A control left *off* is hidden in the Controls
view — that is what out of scope means — but shows in neutral in the other two
once something is named on it, so the grid is populated before anyone has
assessed a thing.

**Overlay** is the other way round: it keeps the control view and lights up
only the controls a chosen product covers, dimming the rest. One product
lighting up nine controls is either good consolidation or a single point of
failure, depending on which nine.

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

So `VENDOR_LIBRARY` is a **starting point, not a source of truth**: common
examples per category, not a ranking, not an endorsement, not exhaustive.
Vendors get acquired, renamed and repositioned constantly — check it before it
goes in front of a client, and edit it freely. Typing a product that is not in
the list works exactly as well; the library only saves typing.

Categories that are advisory or architectural rather than a product purchase —
vCISO services, zero trust, breach notification — are deliberately left empty.

## The dependency continuum

Beneath the grid, in both views, is the continuum from the original matrix:
reliance on **technology** is heaviest at the left and falls away to the right,
reliance on **people** does the reverse, and **process** stays level throughout.
The left-hand functions are *structural* — always running — while the
right-hand ones are *situational*, invoked by an event.

It is there because it changes how the coverage numbers read. A thin Recover
column is not a shopping list; that end of the matrix is where people and
rehearsal matter more than product.

The Cyber Defense Matrix is Sounil Yu's framework — see
[cyberdefensematrix.com](https://cyberdefensematrix.com/) and his book
*Cyber Defense Matrix: The Essential Guide to Navigating the Cybersecurity
Landscape*. This repository is an independent interactive implementation.

## Coverage

The matrix carries a scoreboard: coverage per CSF Function above each column,
per asset class down the right-hand side, and overall in the top-left. Each bar
shows current coverage in blue with the planned position behind it in green —
the distance between the two edges is what the roadmap still owes.

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
not a shortlist, not exhaustive, not an endorsement. `docs/category-provenance.md`
records which category names were verified against a published source and which
were carried from general knowledge.

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
of product it needs — but the vendor links themselves live only in the working
UI and the CSV. An assessment that arrives with product names already on it
reads as a sales document; the vendor conversation is a separate one, after the
gaps are agreed.

## Exports

* **PNG** — the matrix, coverage scoreboard included.
* **PDF Report** — the matrix, then a written gap register: every in-scope
  control with status, priority, Function, asset class, CSF Categories and your
  notes, ordered P1 first and open gaps ahead of scheduled work. Self-contained,
  no libraries.
* **CSV** — the same register for Excel, where roadmaps actually get built.
* **JSON** — the full board including coverage figures, notes and priorities.
  Loading one restores it; exports from the pre-CSF-2.0 control list are
  remapped automatically.

## Editing the control list

`CONTROLS` in `index.html` is the single source of truth. Add an entry with an
`id`, `label`, `fn` (Function column), `rows` (`'all'` or `[first, last]`),
`alias` and `csf`, and `layoutControls()` places, sizes and wraps the bar for
you — there is no SVG to hand-edit. Bars are packed into lanes and are
guaranteed not to overlap.
