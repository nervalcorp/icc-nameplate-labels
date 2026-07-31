# ICC Nameplate Labels

Generates printable equipment nameplate labels from a pasted spreadsheet. Single HTML file, no build step, no dependencies, runs offline.

**Live:** https://icoolaca.github.io/icc-nameplate-labels/

Built for ICC Energy nameplates; hosted here for convenience.

## Using it

1. Select the cells in your line-up sheet — header row included — copy, paste into step 1, press **Read data**
2. Rows are sorted into label-type tabs by their **Label Type** column. Rows with no recognised type go to whichever tab is open
3. Tick the rows you want in step 7, set copies and serial numbering in step 3
4. **Print / Save PDF**

The tool reads nothing but what you paste. No lookups, no bundled catalogue — the only sample data is behind **Load demo rows**, which exists to show the layout and should never be printed.

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

## Saving custom types

**Export types** writes a `label-types.json` you can keep in this repo or share. **Import types** loads it back. Types are not persisted in the browser, so export anything you want to keep.

## Artwork

The brand logo and certification mark are embedded as data URIs. Both can be replaced in step 4 with your own PNG or SVG, with width sliders for each. An empty slot prints blank rather than substituting anything.

## Files

- `index.html` — the whole tool
