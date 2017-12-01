_Editing columns._

## Basics

A column can be renamed by invoking its drop-down menu and picking <tt>Edit column &gt; Rename this column</tt>. You cannot pick a name that is already taken by another column. Renaming a column changes the data and is thus tracked in the undo/redo history.

A column can be removed entirely by invoking its drop-down menu and picking <tt>Edit column &gt; Remove this column</tt>. Removing a column is also tracked in the undo/redo history and can thus be undone.

A column can be moved. The simple ways to move a column is available in its drop-down menu under the <tt>Edit column</tt> submenu:

- <tt>Move column to beginning</tt>
- <tt>Move column to end</tt>
- <tt>Move column left</tt>
- <tt>Move column right</tt>

For more elaborate rearrangement of columns, invoke the left-most drop-down menu in front of "All" and pick <tt>Edit columns &gt; Reorder columns...</tt> This command also lets you remove several columns in one operation. Moving and removing columns are considered to change the data, and are thus tracked in the undo/redo history.

A column can be "collapsed" so that it takes up much less space on screen. Invoke a column's drop-down menu <tt>View &gt; Collapse</tt> this column to collapse it. It will then be displayed as a sliver of vertical space. Clicking on that sliver in the table's header will expand that column. To collapse or expand all columns, use the left-most drop-down menu, in front of "All". Note that since collapsing and expanding columns don't change the data, they are **not** tracked in the undo/redo history.

## Adding Columns

A new column can be created based on zero, one, or more existing columns. Typically, you would create a column based on **one** existing column. Invoke that existing column's drop-down menu and pick <tt>Edit column &gt; Add column based on this column...</tt> If you intend to create a new column based on zero or two or more existing columns, you can invoke the drop-down menu of any column.

The command <tt>Add column based on this column...</tt> will open up a dialog box similar to the (Cell) Transform dialog box (see [Cell Editing](Cell+Editing)). Enter an expression that specifies, for each row, how to generate the cell in that new column. For example, consider this data set:

| name | age |
| --- | --- |
| John Smith | 28 |
| Jane Doe | 33 |

Say we invoke the command <tt>Add column based on this column...</tt> on the column "name" and enter the expression

```
value.split(" ")[1]
```

and name that new column "last name", then after that command executes, the data set will look like

| name | last name | age |
| --- | --- | --- |
| John Smith | Smith | 28 |
| Jane Doe | Doe | 33 |

For more information on expressions, see [Understanding Expressions](Understanding+Expressions).

## Merge / Concatenate Columns

see also [Variables](Variables) for further advice on how to work with cells, rows, records etc.

To merge or concatenate 2 columns together, you can Create a new column called "joinedColumn" or whatever and do this:

```
cells["Column 1"].value + cells["Column 2"].value
```

or if the column names do not contain spaces, you can easily do just this:

```
cells.MyCol1.value + cells.MyCol2.value
```

or using records mode instead of rows mode, like this:

```
row.record.cells.AuthorFirstName.value + " " + row.record.cells.AuthorLastName.value
```

## Splitting Columns

Several columns can be created in one shot by splitting one existing column. For example, in the previous example, we could split the "name" column into two columns by the space separator, and rename the first new column to "first name" and the second new column to "last name". To split a column, invoke its drop-down menu and pick <tt>Edit column &gt; Split into several columns...</tt>

### By Substring

A column can be split by a separator substring or regular expression pattern, or by a list of field lengths. For example, consider

| address |
| --- |
| 100 Main St., San Francisco, CA |
| 200 Broadway Blvd, Boston MA |

Splitting by the substring ", " (note the space following the comma yields 3 columns)

| address 1 | address 2 | address 3 |
| --- | --- | --- |
| 100 Main St. | San Francisco | CA |
| 200 Broadway Blvd | Boston MA | |

Note that the second row has only 1 comma.

You could set the maximum number of new columns, say, to 2, and get

| address 1 | address 2 |
| --- | --- |
| 100 Main St. | San Francisco, CA |
| 200 Broadway Blvd | Boston MA |

Note that the comma in the first row no longer causes any split.

### By Regular Expression

You could also specify a regular expression as the separator. This is useful if what separates the cells is not fixed or incorporates a special character like a tab. For example, this regular expression matches any sequence of tab or space:

```
[\t]+
```

\t matches a tab character. Of course that could even be simplified to use the whitespace character class \s

```
\s+
```

For more information on regular expressions, see [Understanding Regular Expressions](Understanding+Regular+Expressions). But note that whereas regular expressions within a GREL expression must start and end with a slash character /, the regular expressions you enter anywhere in OpenRefine outside a GREL expression must **not** start end with slashes.

### By Field Lengths

Say your data is field-length encoded, such as below

```
John Smith 26
  Peter Pan 13
```

There is nothing to separate the columns, but we can count the characters and determine that the first column is 9 character wide, the second 11 character wide, and the third 2. Then we can split that existing column by lengths: 9, 11, 2.

## Other Ways

There are a few other ways to create new columns, as documented on the [Extending Data](Extending+Data+page).

