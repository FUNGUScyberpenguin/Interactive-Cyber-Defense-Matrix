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

## Two views

A toggle in the header switches what the grid is populated with.

**Controls** — the assessed control set, coloured by status. This is the gap
analysis.

**Vendors** — the same grid populated with the products actually in place.
Plotting the product landscape onto the matrix is what Sounil Yu built it for:
it shows at a glance where the stack is dense, where it is empty, and which
vendors span half the board.

Products are recorded against the control they deliver (open a control,
fill in *Products in place*), so they inherit that control's position and the
two views stay one dataset rather than two. A product answering several
controls appears in each of their cells, and **Stack concentration** below the
matrix ranks products by how much of the board each one covers — the
consolidation conversation, and the concentration risk.

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

Each control carries a **market category** — the term a buyer actually searches
(`CNAPP`, `ITDR`, `SASE`, `Privileged Access Management`) — shown in the detail
panel next to a link to the
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
