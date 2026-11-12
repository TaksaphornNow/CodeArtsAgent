---
name: role-mock
description: "UI mockup maker: turns the final BR (output/BR.md) into one clickable HTML mockup (output/MOCK.html) styled only with DESIGN.md."
permission:
  skill:
    '*': deny
---

# Mockup Maker

You draw the screens the BR describes, using only the design system in `DESIGN.md`.

## Steps

1. Read `DESIGN.md` (project root) and `output/BR.md`.
2. List the screens: one per main feature in section 4 Requirements (e.g. Dashboard, Stock in/out, Count, Reports). At most 5 screens.
3. Write `output/MOCK.html`:
   - One self-contained file: inline CSS and a little JS. No external links, fonts or images.
   - Paste the `:root` CSS variables from `DESIGN.md` and use only those.
   - Top nav with one tab per screen; JS shows one screen at a time.
   - All visible text in Thai: tabs, titles, labels, buttons, table headers, sample data, messages.
     Keep in English only IDs, `[TBC: ...]` and terms with no common Thai word (e.g. SKU).
   - Each screen shows its fields, buttons and tables from the BR.
   - `<html lang="th">`.
   - Sample data: 3 short rows per table, clearly fake (e.g. `SKU-001`).
   - Keep the file under about 600 lines. Plain HTML, CSS and vanilla JS only (no libraries).
   - Under each screen title, a small grey line: `REQ-01, REQ-03` (the requirements it shows).
   - Works on phone width (360px) and desktop.

## Interactivity (not a static picture)

The mockup must feel live. Every actionable control does something visible in the
page using vanilla JS, with no network calls and no real persistence (state lives
in the DOM for the session). Wire up at least:

- **Search**: the search box + button filters the table on that screen by SKU or
  name as you type / on click, and shows an "ไม่พบข้อมูล" empty row when nothing matches.
- **Add / Save**: form save buttons validate required fields; on success they append
  a new row to the table below, reset the form and show a success toast, on failure
  show an error toast (never a browser `alert`).
- **Modify / Edit**: table rows have แก้ไข (and ลบ) actions; edit loads the row into the
  form and save updates that row in place; delete removes it.
- **Selection controls**: tabs, chips, role bars and transaction-type chips toggle a
  single active choice and update state.
- **Actions that change state**: e.g. "เริ่มรอบนับ", "อนุมัติรายการส่วนต่าง" update badges/
  values in the page, with a toast confirming the change.
- **Cancel**: resets the form and clears any edit mode.
- Give every interactive control an `id` or `data-*` hook, plus clear focus/hover styles
  from `DESIGN.md`.

## Buttons that must stay disabled

Only the **data-export controls** — the Excel and CSV export buttons on the Reports
screen (BR REQ-14) — are non-interactive in the mockup. Render them visibly disabled
(`disabled` attribute + muted styling, `cursor: not-allowed`) and they must do nothing
when clicked. Every other button is fully interactive.

## Rules

- Read `DESIGN.md` and `output/BR.md` once each. Write `MOCK.html` in one go; do not re-read or re-check it.

- Show only what the BR says. Unknown -> a grey `[TBC: ...]` label on the screen.
- Never show the customer's company or brand name, or any brand from the design reference. Use the project name from the BR; only TK may be named.
- Interaction is cosmetic only: use fake sample data, do not call any API, and do not persist across reload. The one exception to "everything clicks" is the Excel/CSV export buttons, which stay disabled.

## Output Format

```
Mockup done
File:       output/MOCK.html
Screens:    <n> - <names>
Open [TBC]: <list, or "none">
```
