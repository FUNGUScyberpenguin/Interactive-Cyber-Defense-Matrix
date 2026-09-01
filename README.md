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

* **Status** — mark each control Current (blue), Planned (green), Gap (red) or
  off. State persists in `localStorage`.
* **Group by** — Function, asset class, or a flat A–Z list.
* **Filter** — matches control names, aliases, CSF Category IDs and asset class.
* **Templates** — Table Stakes, Insurance Ready, Industry Average and
  Regulated / Bank pre-fill a maturity profile in the colour you pick.
* **Export** — PNG, PDF, or JSON. Loading a JSON export restores the board;
  exports from the pre-CSF-2.0 control list are remapped automatically.

## Editing the control list

`CONTROLS` in `index.html` is the single source of truth. Add an entry with an
`id`, `label`, `fn` (Function column), `rows` (`'all'` or `[first, last]`),
`alias` and `csf`, and `layoutControls()` places, sizes and wraps the bar for
you — there is no SVG to hand-edit. Bars are packed into lanes and are
guaranteed not to overlap.
