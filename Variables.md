_Variables available in expressions_

## Variables

| variable name | meaning |
| --- | --- |
| <tt>value</tt> | the value of the cell in the base column of the current row; can be <tt>null</tt> |
| <tt>row</tt> | the current row; an object with more fields, with details below |
| <tt>cells</tt> | the cells of the current row, with fields that correspond to the column names; more details below |
| <tt>cell</tt> | the cell in the base column of the current row; an object with more fields, with details below |
| <tt>recon</tt> | the recon object of a cell returned from a reconciliation service or provider; an object with more fields, with details below |
| <tt>record</tt> | one or more rows grouped together to form a record; an object with more fields, with details below |

### Row

A <tt>row</tt> object has a few fields, which can be accessed with a dot operator or with square brackets: <tt>row.index</tt>, <tt>row["index"]</tt>, much like in Javascript.

| field name | meaning |
| --- | --- |
| <tt>row.index</tt> | zero-based index of the current row |
| <tt>row.cells</tt> | the cells of the row, same as the "cells" variable above |
| <tt>row.columnNames</tt> | the column names of the row (i.e. the column names in the project) |
| <tt>row.starred</tt> | boolean, indicating if the row is starred |
| <tt>row.flagged</tt> | boolean, indicating if the row is flagged |
| <tt>row.record</tt> | the Record object containing the current row, same as the <tt>record</tt> variable above |

### Cells

The <tt>cells</tt> object, which can also be accessed as <tt>row.cells</tt>, has fields that correspond to the data column names. For example, <tt>cells.Foo</tt> returns a <tt>cell</tt> object representing the cell in the column named Foo of the current row. If the column name has spaces, use the square bracket method, e.g., <tt>cells["Postal Code"]</tt>.

When you need to get the value of the cells variable itself, you need .value at the end, e.g.,

```
cells["column name"].value
```

When you need to set or mass edit the values of the columns cells, then you can simply use a GREL expression within quotes, such as just

```
"San Francisco Bay"
```

Alternatively, you can use a [Faceting](Facet) which has a built-in edit link next to each value in the Facet panel that allows you to replace large quantities of cell values in one shot.

### Cell

A <tt>cell</tt> object has two fields

| field name | meaning |
| --- | --- |
| <tt>cell.value</tt> | the value in the cell, which can be null, a string, a number, a boolean, or an error |
| <tt>cell.recon</tt> | an object encapsulating the reconciliation results for that cell |

### Recon

A <tt>recon</tt> object has a few fields

| field name | meaning | deeper fields |
| --- | --- | --- |
| <tt>recon.judgment</tt> | a string that is one of: "matched", "new", "none" | |
| <tt>recon.matched</tt> | a boolean, true if judgment is "matched" | |
| <tt>recon.match</tt> | null, or the recon candidate that has been matched against this cell | <tt>.id</tt> <tt>.name</tt> <tt>.type</tt> |
| <tt>recon.best</tt> | null, or the best recon candidate | <tt>.id</tt> <tt>.name</tt> <tt>.type</tt> <tt>.score</tt> |
| <tt>recon.features</tt> | an object encapsulating reconciliation features | <tt>.typeMatch</tt> <tt>.nameMatch</tt> <tt>.nameLevenshtein</tt> <tt>.nameWordDistance</tt> |
| <tt>recon.candidates</tt> | an object encapsulating the default 3 candidates | <tt>.id</tt> <tt>.name</tt> <tt>.type</tt> <tt>.score</tt> |

<tt>recon.candidates</tt> array can be accessed with something like:

```
forEach(cell.recon.candidates,v,v.id).join(",")
```

A recon candidate object has a few deeper fields: <tt>id</tt>, <tt>name</tt>, <tt>type</tt>, and <tt>score</tt>, whose meanings are obvious. <tt>type</tt> is an array of type IDs. So, the id of the best candidate can be accessed as any one of

- <tt>recon.best.id</tt>
- <tt>cell.recon.best.id</tt>
- <tt>row.cells["</tt> (current column's name here) <tt>"].recon.best.id</tt>

You get the idea.

A <tt>features</tt> object has the following fields:

- <tt>typeMatch</tt>, <tt>nameMatch</tt>: booleans, indicating whether the best candidate matches the intended reconciliation type and whether the best candidate's name matches the cell's text exactly
- <tt>nameLevenshtein</tt>, <tt>nameWordDistance</tt>: numbers computed by comparing the best candidate's name with the cell's text; larger numbers mean bigger difference

### Record

A <tt>record</tt> object encapsulates one or more rows that are grouped together. For example, the following data set has 2 records, the first grouping 2 rows and the second grouping 3 rows:

| row | author | book | date |
| --- | --- | --- | --- |
| 1. | Neal Stephenson | Anathem | 2009 |
| 2. | | Snow Crash | 2000 |
| 3. | J.K. Rowling | Harry Potter and the Half-Blood Prince | 2006 |
| 4. | | Harry Potter and the Order of the Phoenix (Book 5) | 2005 |
| 5. | | Harry Potter and the Chamber of Secrets (Book 2) | 2000 |

A <tt>record</tt> object has a few fields, which can be accessed with a dot operator or with square brackets: <tt>record.index</tt>, <tt>record["index"]</tt>, much like in Javascript.

| field name | meaning | example |
| --- | --- | --- |
| <tt>record.index</tt> | zero-based index of the current record | evaluating <tt>row.record.index</tt> on row 2 returns <tt>0</tt> |
| <tt>record.cells</tt> | the cells of the row | evaluating <tt>row.record.cells.book.value</tt> on row 2 returns <tt>["Anathem", "Snow Crash"]</tt> |
| <tt>record.fromRowIndex</tt> | zero based index of the first row in the record | evaluating <tt>row.record.fromRowIndex</tt> on row 2 returns <tt>0</tt> |
| <tt>record.toRowIndex</tt> | index (not zero based) of the last row in the record | evaluating <tt>row.record.toRowIndex</tt> on row 2 returns <tt>2</tt> |

