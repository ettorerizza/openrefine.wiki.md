_Changes for v2.1 release_

# Google Refine 2.1

Google Refine 2.1 is a maintenance release primarily focused on bug fixes.

## New Features

- JSoup-based HTML parsing ([issue 220]( [https://github.com/OpenRefine/OpenRefine/issues#issue/220](https://github.com/OpenRefine/OpenRefine/issues#issue/220)) & [issue 338]( [https://github.com/OpenRefine/OpenRefine/issues#issue/338](https://github.com/OpenRefine/OpenRefine/issues#issue/338))) - new functions: parseHtml, select, htmlAttr, htmlText, innerHtml, ownText
- New codec & clustering options: Metaphone3 (American English) and Cologne Phonetic (German - [issue 399]( [https://github.com/OpenRefine/OpenRefine/issues#issue/399](https://github.com/OpenRefine/OpenRefine/issues#issue/399)))
- Google Fusion Table import support
- new facet for exact duplicates ([issue 398]( [https://github.com/OpenRefine/OpenRefine/issues#issue/398](https://github.com/OpenRefine/OpenRefine/issues#issue/398)))
- save favorite (starred) transforms ([issue 222]( [https://github.com/OpenRefine/OpenRefine/issues#issue/222](https://github.com/OpenRefine/OpenRefine/issues#issue/222)))
- new math functions ([issue 224]( [https://github.com/OpenRefine/OpenRefine/issues#issue/224](https://github.com/OpenRefine/OpenRefine/issues#issue/224))) - acos, asin, atan, atan2, cos, cosh, sin, sinh, tan, tanh, even, odd, abs, fact, factn, combin, degrees, radian, gcd (greatest common denominator), lcm (least common mulitple), multinomial, quotient
- new operator: modulo (%)
- new constant: PI
- update to Apache POI 3.7 (Excel library)
- update to Apache Commons Codec 1.5

## Bug Fixes

- [issue 415]( [https://github.com/OpenRefine/OpenRefine/issues#issue/415](https://github.com/OpenRefine/OpenRefine/issues#issue/415)) - Operand evaluation order wrong for + and - operators
- [issue 404]( [https://github.com/OpenRefine/OpenRefine/issues#issue/404](https://github.com/OpenRefine/OpenRefine/issues#issue/404)) - Character set incorrectly detected during project create
- [issue 401]( [https://github.com/OpenRefine/OpenRefine/issues#issue/401](https://github.com/OpenRefine/OpenRefine/issues#issue/401)) - Export error reporting masks real error
- [issue 374]( [https://github.com/OpenRefine/OpenRefine/issues#issue/374](https://github.com/OpenRefine/OpenRefine/issues#issue/374)) - INC function doesn't work for dates
- [issue 351]( [https://github.com/OpenRefine/OpenRefine/issues#issue/351](https://github.com/OpenRefine/OpenRefine/issues#issue/351)) - unable to export to Excel for projects with more than 256 columns
- [issue 334]( [https://github.com/OpenRefine/OpenRefine/issues#issue/334](https://github.com/OpenRefine/OpenRefine/issues#issue/334)) - Fusion Table CSV import causes NPE
- [issue 294]( [https://github.com/OpenRefine/OpenRefine/issues#issue/294](https://github.com/OpenRefine/OpenRefine/issues#issue/294)) - Export date value in CSV/TSV
- [issue 263]( [https://github.com/OpenRefine/OpenRefine/issues#issue/263](https://github.com/OpenRefine/OpenRefine/issues#issue/263)) - & character parsed incorrectly in XML import
- [issue 358]( [https://github.com/OpenRefine/OpenRefine/issues#issue/358](https://github.com/OpenRefine/OpenRefine/issues#issue/358)) - attempts to compare two values with all stop words returns NaN
- [issue 276]( [https://github.com/OpenRefine/OpenRefine/issues#issue/276](https://github.com/OpenRefine/OpenRefine/issues#issue/276)) - Character encoding issue with project create
- [issue 228]( [https://github.com/OpenRefine/OpenRefine/issues#issue/228](https://github.com/OpenRefine/OpenRefine/issues#issue/228)) - partial fix?
- [issue 202]( [https://github.com/OpenRefine/OpenRefine/issues#issue/202](https://github.com/OpenRefine/OpenRefine/issues#issue/202)) - sort text with accents 
- [issue 197]( [https://github.com/OpenRefine/OpenRefine/issues#issue/197](https://github.com/OpenRefine/OpenRefine/issues#issue/197)) - date wraparound at year boundary
- [issue 196]( [https://github.com/OpenRefine/OpenRefine/issues#issue/196](https://github.com/OpenRefine/OpenRefine/issues#issue/196)) - removing columns causes error and disables undo
- [issue 185]( [https://github.com/OpenRefine/OpenRefine/issues#issue/185](https://github.com/OpenRefine/OpenRefine/issues#issue/185)) - Same reconciliation candidate for multiple cells causes problems
- [issue 184]( [https://github.com/OpenRefine/OpenRefine/issues#issue/184](https://github.com/OpenRefine/OpenRefine/issues#issue/184)) - date.toString() doesn't work
- [issue 107]( [https://github.com/OpenRefine/OpenRefine/issues#issue/107](https://github.com/OpenRefine/OpenRefine/issues#issue/107)) - non-ASCII characters don't display correctly during reconciliation
- issue 61 - multi-line text literals don't work in XML importer

[https:_github.com/OpenRefine/OpenRefine/issues?milestone=6&state=closed_](Full+list+on+Google+Code) [http:code.google.com/p/google-refine/issues/list?can=1&q=milestone%3A2.1&colspec=ID+Type+Status+Priority+Milestone+Owner+Summary&cells=tiles](Full+list+on+Google+Code) of changes

