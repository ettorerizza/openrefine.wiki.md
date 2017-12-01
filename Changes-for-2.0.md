_Change list for Google Refine 2.0_

## Major Changes

- New extension architecture.
- Generalized reconciliation framework that allows plugging in [Reconciliation Service Api](standard+reconciliation+services).
- Support for QA on data loads into Freebase.

## Features

- New commands:
- Fill Down
- Blank Down
- Transpose Cells in Columns into Rows
- Transpose Cells in Rows into Columns (Issue 82)
- Move Column to Beginning, Move Column to End, Move Column Left, Move Column Right, Reorder Columns
- Add Column by Fetching URLs
- Recon commands:
- Clear recon data for all matching rows
- Clear recon data for one cell
- Clear recon data for similar cells
- Copy recon judgments across columns
- Google Refine Expression Language (GREL):
- JSON support
- New functions: smartSplit, escape, parseJson, hasField, uniques
- New controls: forEachIndex, forRange, filter
- New parameters:
- preserveAllTokens on split function
- Regexp groups capturing GEL function
- Importers
- New: Json importer
- CSV and TSV importers: added support for ignoring quotation marks
- Added support for creating a project by pointing to a data file URL.
- Text facet's choice count limit is now configurable through preference page
- Select All and Unselect All buttons in History Extract dialog
- Schema aligment skeleton: support for multiple cells per cell-as nodes, and for conditional links

## Bug Fixes

- TSV/CSV exporter bug: Gridworks crashed when there were empty cells.
- Issue 29: Delievered "Collapse whitespace" transformation does not work
- Issue 69: ControlFunctionRegistry now correctly registers Chomp expression as "chomp" key.
- Issue 66: Records not excluded with inverted text facet
- Issue 86: Parse cells after splitting columns
- Issue 99: Diff for dates fails with "unknown error" always
- Issue 110: Import of single column text file with Postal Codes shows only 1 row with lots of � chars (?)
- Issue 113: Export filtered rows as tsv or csv fails; html and excel OK
- Issue 116: CSV/TSV export data includes blank fields for deleted columns
- Issue 121: Importing attached file strips backslashes
- Issue 122: Exporting to Excel on attached project raises server exception
- Issue 126: Large integers formatted in scientific notation in formulas
- Issue 135: Hangs when setting cell value to large JSON string
- Issue 138: Numbers should be right-justified
- Issue 146: In "Cluster and Edit Column", clicking on entry value to set "Merge?" checkbox does not reflect the final value of operation
