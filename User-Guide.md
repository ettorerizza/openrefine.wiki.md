_How to use OpenRefine_

If you haven't done so already, we strongly suggest you to watch the [Screencasts](screencasts) first as they will give you an idea of how to use OpenRefine.

## The Basics

First, although OpenRefine might start out looking like a spreadsheet program (Microsoft Excel, Google Spreadsheets, etc.), don't expect it to work like a spreadsheet program. That's almost like expecting a database to work like a text editor.

OpenRefine is NOT for entering new data one cell at a time. It is NOT for doing accounting.

OpenRefine is for applying transformations over many existing cells in bulk, for the purpose of cleaning up the data, extending it with more data from other sources, and getting it to some form that other tools can consume.

To use OpenRefine, think in big patterns. For example, to spot errors, think

- Show me every row where the string length of the customer's name is longer than 50 characters (because I suspect that the customer's address is mistakenly included in the name field)
- Show me every row where the contract fee is less than 1 (because I suspect the fee was entered in unit of thousand dollars rather than dollars)
- Show me every row where the description field (scraped from some web site) contains "&amp;" (because I suspect it wasn't decoded properly)

To edit data, think

- For every row where the contract fee is less than 1, multiply the fee by 1000.
- For every row where the customer name contains a comma (it has been entered as "last\_name, first\_name"), split the name by the comma, reverse the array, and join it back with a space (producing "first\_name last\_name")

To specify patterns, use filters and facets. Typically, you create a filter or facet on a particular column. For example, you can create a numeric facet on the "contract fee" column and adjust its range selector to select values less than 1. If the default facet doesn't do what you want, you can configure it (by clicking "change" on the facet's header). For example, you can create a text facet with on the same "contract fee" column with this expression:

```
value < 1
```

It will show 2 choices: true and false. Just select true. Then, invoke the Transform command on that same column and enter the expression

```
value * 1000
```

That Transform command affects only rows where the "contract fee" cell contains a value less than 1.

You can use several filters and facets together. Only rows that are selected by all facets and filters will be shown in the data table. For example, say you have two text facets, one on the "contract fee" column with the expression

```
value < 1
```

and another on the "state" column (with the default expression). If you select "true" in the first facet and "Nevada" in the second, then you will only see rows for contracts in Nevada with fees less than 1.

## Analogies

### Databases

If you have programmed databases before (performing SQL queries), then what OpenRefine works should be quite familiar to you. Creating filters and facets and selecting something in them is like performing this SELECT statement:

```
SELECT *
  WHERE ... constraints determined by selection in facets and filters ...
```

And invoking the Transform command on a column while having some filters and facets selected is like performing this UPDATE statement

```
UPDATE whole_table SET column_X = ... expression ...
  WHERE ... constraints determined by selection in facets and filters ...
```

The difference between OpenRefine and databases is that the facets show you choices that you can select, whereas databases assume that you already know what's in the data.

### Adobe Photoshop

If you have used advanced image editing software, such as Adobe Photoshop, then you could also see similarities with OpenRefine. For example, in Photoshop, you don't edit one pixel at a time. Instead, you create selections based on color hues or by edge detection, and then apply effects to pixels within those selections, or use those selections to limit the extent of manual brushworks. That way of working makes sense because there are just too many pixels to deal with individually, and generally the effect you want to apply is targeting several pixels that are similar in some ways (e.g., pixels making up the face in the portrait, pixels making up the shadows in the landscape). When using OpenRefine, learn to see patterns within your data so that you can specify how to select several cells together based on some characteristics common among them.

