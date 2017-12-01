_Changes for v2.5 release_

# New Features

- Completely new user interface for project creation, including:
- Live preview with interactive settings before creating project 
- Fixed-width importer that allows for specifying column widths by clicking ( [https:_github.com/OpenRefine/OpenRefine/issues/85_](issue+%2385))
- XML and JSON importers that allow for interactive selection of elements to import
- Direct access to Google Spreadsheets ( [https:_github.com/OpenRefine/OpenRefine/issues/278_](issue+%23278)) and Google Fusion Tables ( [https:github.com/OpenRefine/OpenRefine/issues/279](issue+%23279))
- Import and export to private Google Spreadsheets & Fusion Tables for logged-in users 
- Import using results of Google spreadsheet visualisation API query ( [https:_github.com/OpenRefine/OpenRefine/issues/375_](issue+375)
- Sheet selection for import from Excel ( [https:_github.com/OpenRefine/OpenRefine/issues/280_](issue+280) & Google Spreadsheets ( [https:github.com/OpenRefine/OpenRefine/issues/281](issue+281)) 
- Support for directly selecting files within zip/archive file without unpacking them first ( [https:_github.com/OpenRefine/OpenRefine/issues/131_](Issue+131))
- Support for creating a project using contents of the paste buffer ( [https:_github.com/OpenRefine/OpenRefine/issues/88_](Issue+84))
- Better progress feedback during upload & processing ( [https:_github.com/OpenRefine/OpenRefine/issues/179_](issue+179))

- Custom tabular exporter that allows for selecting and configuring columns to export, and direct upload to Google Spreadsheets and Fusion Tables
- Support for IE8 and IE9 through Google Chrome Frame
- New command "Key/value Columnize" (see [http:_code.google.com/p/google-refine/source/detail?r=2356_](explanation))
- New importer for PC-Axis file format.

# Enhancements

- Issue 31: Maximum number of facet values should be configurable.
- Issue 38: [Wishlist](Wishlist) Fix the table header so that it's always visible when scrolling a long page
- Issue 97: Exporting CSV should allow for optional columns
- Issue 447: Extend toTitlecase() function with support for custom delimiters
- Operation "Transpose columns into rows" now supports an additional mode of generating 2 key and value columns (rather than generating just one combined column); it also has an option for filling in other columns.
- reinterpret() now supports optionally specifying a source encoding to be used instead of the project encoding - reinterpret("target encoding", "source encoding")

# Bug Fixes

- Issue 149: rows button stays strike-through when clicking on it
- Issue 349: Clustering not finding duplicates when facet is showing groupings
- Issue 394: Add a reset button to Text filter
- Issue 419: Values with Characters like é split across several lines
- Issue 424: Duplicate column names don't allow user to recover
- Issue 426: filter with custom facet adds zero lines choice
- Issue 428: Excel import sometimes drops last row of data
- Issue 433: Usability: use HTML label (e.g., for checkboxes)
- Issue 440: Fetch URLs aborts while saving/loading/computing facets
- Issue 441: onError - "keep-original" / "store-blank" working oddly for value.toDate()
- Issue 442: Two column transforms to date on the same column turns the cells blank
- Issue 449: Uncaught exception from Excel importer
- [https:_github.com/OpenRefine/OpenRefine/issues/502_](Issue+502) - Fetch URLs does not return a correct HTTP response

## Miscellaneous

- Issue 233: 404 Error upon Launch
- Issue 362: Augmentation screencast (#3) is now missing/private
- Issue 410: Convert line-endings to DOS format
- Issue 435: refine sh script only checks for java 1.6, not > 1.6
- Issue 444: Patch for /trunk/main/webapp/modules/core/scripts/dialogs/custom-tabular-exporter-dialog.html
- Issue 445: Patch for /trunk/main/webapp/modules/core/scripts/dialogs/custom-tabular-exporter-dialog.html
- Issue 453: Patch for /trunk/main/tests/server/src/com/google/refine/tests/importers/JsonImporterTests.java

[https:_github.com/OpenRefine/OpenRefine/issues?milestone=2&state=closed_](Full+list+of+Issues).

