_Exploring data by applying multiple filters_

OpenRefine supports faceted browsing as a mechanism for

- seeing a big picture of your data, and
- filtering down to just the subset of rows that you want to change in bulk.

Typically, you create a facet on a particular column. The facet summarizes the cells in that column to give you a big picture on that column, and allows you to filter to some subset of rows for which their cells in that column satisfy some constraint. That's a bit abstract, so let's jump into some examples.

## Default Text Facets

Consider a data set like this

| president | died in office | party |
| --- | --- | --- |
| Abraham Lincoln | yes | Republican |
| Andrew Johnson | no | Democratic |
| Ulysses Grant | no | Republican |
| Rutherford Hayes | no | Republican |
| James Garfield | yes | Republican |
| Chester Arthur | no | Republican |
| Grover Cleveland | no | Democratic |
| Benjamin Harrison | no | Republican |
| William McKinley | yes | Republican |
| Theodore Roosevelt | no | Republican |
| William Taft | no | Republican |
| Woodrow Wilson | no | Democratic |
| Warren Harding | yes | Republican |
| Calvin Coolidge | no | Republican |
| ... | ... | ... |

Let's create a text facet on the "party" column (by clicking on that column's drop-down menu and invoke <tt>Facet &gt; Text facet</tt>). It will show something like this

| choice | count |
| --- | --- |
| Democratic | 15 |
| Republican | 18 |
| ... | ... |

What it is telling you is that there are 15 rows in which the "party" cells contain "Democratic", 18 rows in which the "party" cells contain "Republican", etc. And you, understanding the meaning of this data, might phrase that as: there are 15 democrat presidents, 18 republican presidents, etc. In this way, the facet gives you a big picture of your data, saving you the effort of counting the presidents by their parties yourself.

Now if you click on "Democratic" in the facet, then the data table on the right will display those 15 rows in which the "party" cells contain "Democratic". And you, understanding the meaning of this data, will think: I am currently looking at democrat presidents only. Now, commands that you invoke will affect those rows only, until you clear that facet's selection.

## Multiple Facets

In addition to the "party" facet, let's create a text facet on the column "died in office". It might show something like this

| choice | count |
| --- | --- |
| yes | 6 |
| no | 37 |

Selecting something in one facet affects the choices in the other(s). For example, if you select "yes" in the facet "died in office", then that filters the data table to only the 6 rows where the cells "died in office" contain "yes". Now, the other facet -- the facet "party" -- will operate only on those 6 rows. And it might now show something like this

| choice | count |
| --- | --- |
| Democratic | 2 |
| Republican | 4 |

You might phrase that as, "among those presidents who died while in office, 2 were democrats and 4 were republicans."

Likewise, if you don't select anything in the facet "died in office", but you select "Democratic" in the facet "party", then you might conclude something like, "among X democrat presidents, M died while in office while N did not (X = M + N)".

Now, if you select something in **both** facets, then you get the intersection of their effects. For example, if you select "yes" in the facet "died in office" and "Democratic" in the facet "party", then you are now looking at 2 democrat presidents who died while in office.

## Custom Text Facets

Now let's say you have some hypothesis about the first names that presidents usually have. There is no column for first names, and let's say that you don't want to create a new column for it. That's OK, because we can create a custom text facet that extracts out the first names. Invoke the "president" column's drop-down menu and pick <tt>Facet &gt; Custom text facet...</tt> and enter the expression

```
value.split(" ")[0]
```

That facet splits the value against the 1st space char found and returns the 1st part of the split and will show you something like this (sorted by count):

| choice | count |
| --- | --- |
| James | 5 |
| John | 4 |
| William | 4 |
| George | 3 |
| Andrew | 2 |
| Thomas | 1 |
| Franklin | 2 |
| ... | ... |

**IMPORTANT TIP:** <tt>[1]</tt> is equivalent to saying "The 2nd part of an array or list" in GREL since indexing of arrays or lists in Refine actually begins with <tt>[0]</tt> or "The 1st part of an array or list". For example, if current value in column is "James Garfield", then:

<tt>"James Garfield".split(" ")[0]</tt> returns <tt>James</tt>

and

<tt>"James Garfield".split(" ")[1]</tt> returns <tt>Garfield</tt>.

## Row-based Text Facets

Sometimes you want to filter the rows based on no particular column. For example, let's say that you have to apply some operation on a very complicated subset of rows. You can do so by applying some simple facets and filters and star those rows, then apply some other simple facets and filters and star those rows, and so forth. The starring operations are cumulative, producing a union of all those subsets of rows. At the end, you want to select all starred rows to apply an operation on them. You would need to create a text facet with the expression

```
row.starred
```

and select "true" in it. You can create such a facet by invoking **any** column's drop-down menu. Or, of course, you could use the very left-most drop-down menu before "All".

## Multi-column-based Text Facets

Sometimes you want to create a facet that references several columns. For example, consider a data set with two columns, "First Name" and "Last Name". You're interested in finding out how many people have the same initial letter for both names (e.g., Marilyn Monroe, Steven Segal, Ophelia Oliver). To do so, create a custom text facet on either column and enter the expression

```
cells["First Name"].value[0] == cells["Last Name"].value[0]
```

That shows 2 choices: <tt>true</tt> and <tt>false</tt>. Select "true" for the people you want.

## Numeric Facets

Whereas a text facet groups unique text values into groups, a numeric facet groups numbers into numeric range bins. For example, consider a data set with a column of "humidity" (which ranges from zero to 100). A default numeric facet created on that column would group the values into 100 bins: 0 to 1, 1 to 2, 2 to 3, ..., 99 to 100. A value of 98.3 will fall into the 99th bin. Then you can select any range that spans some consecutive number of bins.

You can customize numeric facets much like you can customize text facets. For example, if the numeric values in a column are drawn from a power law distribution, then it's better to group them by their logs using this expression

```
value.log()
```

Or if you suspect that the values are periodic you could take the modulus by the period to understand if there's a pattern, e.g.,

```
mod(value, 7)
```

Even for a column that contains text, you could still create a numeric facet by taking the length of the string

```
value.length()
```

## Text Filters

If you need to filter to rows whose cells in a particular column contain some precise text string, or match some regular expression pattern, apply a text filter from the columns' drop down menu. It works like most text editor's "Find" or "Search" feature, except that instead of jumping the text cursor to the occurrence of the query, it filters the data table to only rows whose cells in that column match the query.

Regular expression support in text filters lets you do fancy matching. For example, to search for cells starting with a letter and ending with a digit, enter this regular expression pattern:

```
^\w.*\d$
```

The regular expression language supported here is [http:_download.oracle.com/javase/tutorial/essential/regex/_](Java%27s+regular+expression).

## Miscellany

Each text facet shows up to 2000 choices by default. You can increase this limit if you think your computer can handle it. Whether "your computer" can handle it or not depends mostly on your web browser. In general, we recommend using a fast web browser anyhow. If the facet has more choices than the current limit, you'll be offered the option to increase the limit. If you find the new limit is too much for your computer to handle, you can decrease it again by going to [http://127.0.0.1:3333/preferences](http://127.0.0.1:3333/preferences) and changing the preference with key <tt>ui.browsing.listFacet.limit</tt> and value equal the new limit, such as <tt>3000</tt>.

The choices and counts displayed in each facet can be copied out, as they serve as a good summary of your data. To do so, click on the "X choices" link near the top left corner of the facet.

Facets are **not** saved in the project along with the data. But you could save a **permalink** to the current state of the application (much like you can save permalinks in any decent web application). Find the "permalink" after the project's name, at the top.

Other facets to be documented: - Timeline - select by date - Scatterplot - plot x/y graph and select by rectangular area - Duplicates - Word, Text Length, Character code, etc

