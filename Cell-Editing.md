_Editing cells._

## Editing One Cell at A Time

To edit a cell, hover your mouse over that cell. You should see a tiny blue button labeled "edit". Click it to edit the cell. That pops up a window with a bigger text field for you to edit.

When done, click "Apply" or press Enter. That changes that one cell. On the other hand, if you click "Apply to All Identical Cells" or press Ctrl-Enter, then for all cells in that column that contain the same text (to that cell) originally, they will all be replaced with the new value that you just entered. It's essentially a Find & Replace operation.

You could set the data type of the cell in the popup. For dates, we support [http:_en.wikipedia.org/wiki/ISO\_8601_](ISO+8601) as well as more readable strings, such as "today" and "yesterday". Readable strings can be whatever that the library [http:www.datejs.com/](datejs) can parse.

## Editing through Text Facets

You could also edit cells in a column sharing some common value using a default text facet created on that column. First, create a default text facet on that column. Then locate that value choice in the facet and hover your mouse over it. An "edit" link will appear next to the choice. Click it to get a popup. As this is a text facet, OpenRefine will only let you enter a text string.

Editing through a text facet performs a find and replace operation. Consider a data set with a column of people's names. Creating a text facet on that column might yield something like this:

| choice | count |
| --- | --- |
| Andy Anderson | 79 |
| Andy R. Anderson | 9 |
| Beatrice Beaufort | 28 |
| Cindy Mansfield | 67 |
| ... | ... |

If you click "edit" on the choice "Andy R. Anderson" in the facet and enter "Andy Anderson", what that does is to go over each cell in that name column that contains the string "Andy R. Anderson" and replaces it with the string "Andy Anderson". Nine (9) such cells will be changed, and no cell in that column will contain "Andy R. Anderson", but 9 more cells will contain "Andy Anderson". As a result, the text facet will be updated to show 88 "Andy Anderson"; and "Andy R. Anderson" will disappear:

| choice | count |
| --- | --- |
| Andy Anderson | 88 |
| Beatrice Beaufort | 28 |
| Cindy Mansfield | 67 |
| ... | ... |

## Editing by Transforming

The most common way to edit cells in a column in OpenRefine is by invoking the Transform command on that column: from the column's drop-down menu, pick <tt>Edit cells &gt; Transform ...</tt> In the dialog box, you enter an expression that gets evaluated on each row to compute the new cell value for that row.

| key | maximum |
| --- | --- |
| 1 | 13 |
| 2 | 34 |

To add or increment by 1 on the **maximum** column, enter the GREL expression

```
value + 1
```

and the data set will become:

| key | maximum |
| --- | --- |
| 1 | 14 |
| 2 | 35 |

Expressions can even be formed by chaining together functions that pass their output to the next function. Consider this data set:

| name | age |
| --- | --- |
| John Smith | 28 |
| Jane Doe | 33 |

Say we invoke the Transform command on the name column and enter the expression

```
value.split(" ").reverse().join(", ")
```

This causes OpenRefine to iterate through each of the 2 rows, and for each row, set the variable <tt>value</tt> to the row's cell in column "name", evaluate the expression, and put the result back into that cell. More specifically,

- when OpenRefine processes row 1, it sets <tt>value</tt> to the string <tt>John Smith</tt>, then splits that string into 2 strings (<tt>John</tt> and <tt>Smith</tt>), then reverses those two strings (<tt>Smith</tt> and <tt>John</tt>), and finally joins them back to yield <tt>Smith, John</tt>, and puts that string into that cell.
- when OpenRefine processes row 2, it sets <tt>value</tt> to the string <tt>Jane Doe</tt>, then splits that string into 2 strings (<tt>Jane</tt> and <tt>Doe</tt>), then reverses those two strings (<tt>Doe</tt> and <tt>Jane</tt>), and finally joins them back to yield <tt>Doe, Jane</tt>, and puts that string into that cell.

When you click OK, the data set will become

| name | age |
| --- | --- |
| Smith, John | 28 |
| Doe, Jane | 33 |

For more information on expressions, see [Understanding Expressions](Understanding+Expressions).

### Splitting Multiple Values within Cells to produce Records

OpenRefine has a feature called **Split multi-valued cells** that can essentially create fields similar to a database record. Take the following example,

| name | data type | data record |
| --- | --- | --- |
| Kate Moss | Person | profession:Model,href:"/0122-kate-moss",title="Kate Moss",hair:brown |
| Marilyn Monroe | Person | profession:Actor,href:"/1488-marilyn-monroe",title="Marilyn Monroe",hair:blond |

The 'data record' column has several fields that need to be split, but we do not necessarily want to create additional columns, instead we want those individual 'fields' for each Person record, since that seems to be how this dataset is arranged. We can use <tt>Edit Cells -&gt; Split multi-valued cells...</tt> on the 'data record' column using the comma <tt>,</tt> separator character. After applying, our table in OpenRefine now looks like this and you are taken from **rows** mode into **records** mode which is shown just above the **All** column,

* * *

Show as: rows **records** Show: 5 **10** 25 50 records

| All | name | data type | data record |
| --- | --- | --- | --- |
| | Kate Moss | Person | profession:Model |
| | | | href:"/0122-kate-moss" |
| | | | title="Kate Moss" |
| | | | hair:brown |
| | Marilyn Monroe | Person | profession:Actor |
| | | | href:"/1488-marilyn-monroe" |
| | | | title="Marilyn Monroe" |
| | | | hair:blond |

**NOTE:** You can split multi-valued cells on any column and even export those records, keeping the record format, but be aware of the current record model handling in OpenRefine 2.5 and prior: the left-most column is always the **"key column"** for the record set. If you move columns to the left-most column (the beginning), this will break the underlying record model. However, breakage is only because we have not yet incorporated support for interactive column grouping. (but planned for in a future release)

### Comparison with Spreadsheets Software

[Understanding Expressions](Expressions) in OpenRefine differ from formulas in popular spreadsheet software. In spreadsheets, formulas are entered into cells. That is, a cell can contain a formula (the formula is computed and its result is displayed in the cell). In OpenRefine, cells do **not** contain expressions. Rather, any expression you specify is evaluated to compute a value that is then stored into the cell. If the expression depends on other cells, and the other cells get changed later on, the expression is **not** evaluated again. Each transformation is done only once, **not** updated constantly.

Also, in spreadsheets, you have to copy formulas from one cell to other cells (typically using the Fill Down command or dragging down the selection rectangle from its bottom right corner). In OpenRefine, there is no copying of expressions. Rather, an [Understanding Expressions](expression) is applied to rows that are currently shown, as determined by constraints in facets and filters.

## Editing by Clustering

Perhaps the most powerful way to edit cells is through the Clustering feature, but that deserves [Clustering](a+whole+page+of+its+own).

