_Announcements of the latest features_

## Nov 2017

- OpenRefine 2.8 is released
- [https:_newslab.withgoogle.com/_](Google+Inc.+News+Lab) made a generous $100,000 grant to OpenRefine Foundation to begin working on features important to News Organziations that use OpenRefine.
- Owen Stephens is added as a new Core contributor

## June 2017

- Antonin Delpuche is added as a new Core contributor
- Jacky pushed some new functionality into our master branch that lets you transfrom cells across all columns

## October 2015

OpenRefine 2.6 release candidate 2 is released

## August 2013

OpenRefine 2.6 beta 1 is released

## October 2012

Project renamed to OpenRefine and moved to Github

## December 2011

Google Refine 2.5 is released ([ChangesFor2p5 change list])

## June 2011

Google Refine 2.1 is released([ChangesFor2p1 change list])

## November 2010

Google Refine 2.0 is released ([ChangesFor2p0 change list]).

## May 2010

### May 28th, 2010

Freebase Gridworks 1.1 is released.

- Features:
- Row/record sorting (Issue 32)
- CSV exporter (Issue 59)
- Mqlwrite exporter
- - Templating exporter (experimental)

- Fixes:
- Issue 34: "Behavior of Text Filter is unpredictable when "regular expression" mode is enabled." Regex was not compiled with case insensitivity flag.
- Issue 4: "Match All bug with ZIP code". Numeric values in cells were not stringified first before comparison.
- Issue 41: "Envelope quotation marks are removed by CSV importer"
- Issue 19: "CSV import is too basic"
- Issue 15: "Ability to rename projects"
- Issue 16: "Column name collision when adding data from Freebase"
- Issue 28: "mql-like preview is not properly unquoting numbers"
- Issue 45: "Renaming Cells with Ctrl-Enter produced ERROR" Tentative fix for a concurrent bug.
- Issue 46: "Array literals in GEL"
- Issue 55: "Use stable sorting for text facets sorted by count"
- Issue 53: "Moving the cursor inside the Text Filter box by clicking"
- Issue 58: "Meta facet". Supported by the function facetCount()
- Issue 14: "Limiting Freebase load to starred records". We load whatever rows that are filtered through, not particularly starred rows.
- Issue 49: "Add Edit Cells / Set Null"
- Issue 30: "Transform dialog should remember preferred language."
- Issue 62: "It'd be nice if URIs were hyperlinked in the data cells"

- Other Changes:
- Moved unit tests from JUnit to TestNG

### May 12th, 2010

Freebase Gridworks 1.0.1 is released.

- Fixes:
- Issue 2: "Undo History bug" - bulk row starring and flagging operations could not be undone.
- Issue 5: "Localized Windows cause save problems for Gridworks" -
- Windows user IDs that contain unicode characters were not retrieved correctly.
- Issue 10: "OAuth fails on sign in" - due to clock offset.
- Issue 11: "missing "lang" attribute in MQL generated in schema alignment"
- Issue 13: "float rejected from sandbox upload as Json object" - everything was sent as a string.
- Issue 17: "Conflated triples - all rows are producing triple with "s" :" $Name\_0"" The Create A New Topic for Each Cell command created shared recon objects.
- Issue 18: "Error converting russian characters during edit of single cell"
- {partial fix} Issue 19: "CSV import is too basic" - fixed for CSV, not for TSV

### May 5th, 2010

Freebase Gridworks 1.0 is released.

