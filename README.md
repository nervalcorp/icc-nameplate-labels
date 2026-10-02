# ICC Nameplate Labels

Generates printable equipment nameplate labels from a pasted spreadsheet. Single HTML file, no build step, no dependencies, runs offline.

**Live:** https://nervalcorp.github.io/icc-nameplate-labels/

Built for ICC Energy nameplates.

**Author:** John L. — Nerval Corp

## Using it

1. Click the grid in step 1 and paste (Ctrl+V) the cells from Excel — header row included
2. Rows are sorted into label-type tabs by their **Label Type** column
3. Tick the rows you want in step 7, set copies and serial numbering in step 3
4. **Print / Save PDF**

The tool reads nothing but what you paste. There are no lookups and no bundled catalogue — the only sample data is behind **Load demo rows**, which exists to show the layout and should never be printed.

## The sheet grid

Step 1 is a small spreadsheet. Everything lives in this one sheet; the tabs are just views of it.

- **Paste** — click a cell and Ctrl+V. It fills right and down from there and grows the sheet as needed. One value pasted over a selected range fills the whole range
- **Replace the whole sheet** — click a header cell (or Ctrl+A) and paste. An empty sheet does this automatically, and the first row is read as headers
- **Edit** — double-click, press Enter / F2, or just start typing. Enter and Tab commit and move on, Esc cancels
- **Select** — click, drag, or Shift+click; click a row number to select the row
- **Copy out** — Ctrl+C copies the selection as tab-separated text, ready for Excel. **Copy for Excel** copies the whole sheet
- **Rows and columns** — add, duplicate or delete rows; add a column; double-click a header to rename it
- **Undo / Redo** — Ctrl+Z / Ctrl+Y, 40 steps. Print ticks and copy counts are not rolled back by undo
- The **Tab** column shows which label type each row will print on. An amber `?` means its Label Type wasn't recognised and it was placed on a tab by default
- Drag the divider beside the panel to make it wider; double-click it to reset

**Raw text** (collapsed under the grid) mirrors the sheet as tab-separated text. Editing it does nothing until you press **Read data**, which replaces the sheet.

## Editing on a label

Click any value on a label, type, and press Enter (Esc cancels). The change is written to the sheet, so the grid, the raw text and every other copy of that row update immediately.

- A change applies to the **row**, so all copies of it change together. For a one-off variant, duplicate the row
- The sheet keeps its own formatting. If the sheet stores `60HZ` and the label shows `60`, typing `50` stores `50HZ`; PH works the same way
- A value that the label borrows from another field (a blank Fan Voltage showing the main voltage) is written to its own column only if you change it
- If a field has no column in the sheet yet, one is added, named after the field
- The title and footer are not data. Set them in step 5 and step 6

## Serial numbers

Serials come from, in order: the **Serial** column (when ticked), then the pattern counter.

- Rows with **identical data** (ignoring the Serial column) are treated as the same label and **share one serial**. Untick *Rows with identical data share one serial* in step 3 to turn this off
- For several units of the same model use **Copies** — every copy gets its own serial
- Editing a serial on a label saves it to the Serial column (created if missing), and updates any identical rows with it. A row printing several copies has a serial per copy, so those aren't saved
- The counter runs per print: with *This type only*, each type counts from the start value. Use *Every type*, or change **Start at**, if serials must be unique across types

## Autosave

The sheet, custom label types, artwork and settings are saved in **this browser** (localStorage) a moment after every change, and restored when you reopen the page. Nothing is uploaded anywhere.

- **Clear saved data** (top right) wipes it and starts fresh — worth using on a shared computer
- Saved data does not travel between browsers or machines. Use **Export types** for label layouts, and **Copy for Excel** for the sheet

## Printing

- **One per page** emits pages at exactly 4″ × 5.5″ for a label printer
- **Letter sheet** puts 2 labels on an 8.5″ × 11″ page
- Set the printer to **100% / actual size** — never "fit to page", or the label comes out undersized
- The 2 pt border is the cut line

## Label types

Two are built in:

| Type | Style | Notes |
|---|---|---|
| Evaporator | open, no cell borders | heater block, min + max circuit amp |
| Condenser | ruled cells | RLA + LRA, min circuit ampacity, no heater |

Each type keeps its own column mapping, so one shared `FLA / RLA` column can feed **FLA** on evaporators and **RLA** on condensers.

### Adding a type

**+** on the tab bar duplicates the current type as a starting point. Set its name, which Label Type values route to it, and edit the layout in step 5. Layouts are JSON built from these blocks:

`head` · `heading` · `table` · `pair` · `kvlist` · `row` · `group` · `mark` · `foot` · `gap`

Step 5 has a reference panel listing every block option and field key. Two flow modes: `spread` shares leftover vertical space evenly and tolerates values that wrap; `top` places blocks by explicit `mt` margins for exact positioning and pins the footer to the bottom.

Any label taller than the stock is outlined in red with a warning — usually a value wrapping in a column that's too narrow.

**Export types** writes a `label-types.json` you can keep in this repo or share; **Import types** loads it back.

## Artwork

The brand logo and certification mark are embedded as data URIs. Both can be replaced in step 4 with your own PNG or SVG, with width sliders for each. An empty slot prints blank rather than substituting anything.

## Files

- `index.html` — the whole tool
