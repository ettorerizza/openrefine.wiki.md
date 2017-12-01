_Understanding expressions._

## Introduction

OpenRefine supports "expressions" mostly to transform existing data or to create new data based on existing data, much like how spreadsheet software supports "formulas". There are, however, significant differences between [DocumentationForUsers#Expressions OpenRefine's expressions] and typical spreadsheet formulas.

### [[Variables]]

Consider this sample data set

| | friend | age |
| --- | --- | --- |
| 1. | John Smith | 28 |
| 2. | Jane Doe | 33 |

When you invoke the Transform command on, say, column "friend" and enter an expression, OpenRefine will go through each row in the data (matching facets and filters, if any), and evaluate that expression for that row in order to obtain a result for that row. Whereas in a spreadsheet, you would need to store a different formula in each cell of that column, in OpenRefine, you only need one single expression. And that's made possible through the use of [Variables](Variables), as explained next.

When evaluated on a row in the example above, the expression can access that row and the cell in the column "friend" of that row through [Variables](Variables). Think of a variable as a placeholder for something else. For example, there is a variable named "value" that is the placeholder for the cell's content. When the expression is evaluated on row 1, the variable "value" will stand for "John Smith"; when evaluated on row 2, it will stand for "Jane Doe". So, if the expression is

```
value.split(" ")[1]
```

then for row 1, it will yield "Smith" and for row 2, "Doe". That expression splits against the space char found and takes the 2nd part.

**IMPORTANT TIP:** <tt>[1]</tt> is equivalent to saying "The 2nd part of an array or list" in GREL since indexing of arrays or lists in Refine actually begins with <tt>[0]</tt> or "The 1st part of an array or list".

Using [Variables](Variables), a single expression yields different results for different rows.

### Base Column

Note that an expression is typically based on one particular column in the data--the column whose drop-down menu is invoked. A lot of [Variables](variables) are created to stand for things about the cell in that "base column" of the current row on which the expression is evaluated. But there are still [Variables](variables) about the whole row, and through them, you can access cells in other columns.

### Languages

Whereas each spreadsheet software has its own formula language, and only one language, OpenRefine is capable of supporting several languages for writing expressions. OpenRefine has its own native language called OpenRefine Expression Language (GREL), but you could also use [Jython](Jython), or other languages if you install OpenRefine extensions that support them.

Where in GREL you write

```
value.split(" ")[1]
```

in [Jython](Jython) you would write

```
return value.split(" ")[1]
```

For that example the two languages are similar enough, but they don't have to be. On this documentation wiki, we will focus mostly on GREL. If you use another language, like [Jython](Jython), please refer to its own documentation.

## OpenRefine Expression Language (GREL)

### Basics

GREL is designed to resemble Javascript. So you can expect these basic things to work, and know how they would work:

| example | description |
| --- | --- |
| <tt>value + " (approved)"</tt> | concatenate two strings; whatever is in <tt>value</tt> gets converted to a string first |
| <tt>value + 2.239</tt> | add two numbers; if <tt>value</tt> actually holds something other than a number, this becomes a string concatenation |
| <tt>value.trim().length()</tt> | trimming leading and trailing whitespace of <tt>value</tt> and than take the length of the result |
| <tt>value.substring(7, 10)</tt> | take the substring of <tt>value</tt> from character index 7 up to and excluding character index 10 |
| <tt>value.substring(13)</tt> | take the substring of <tt>value</tt> from character index 13 until the end of the string |

### Concatenation

If you're used to Excel, note that the operator for string concatenation is not <tt>&amp;</tt> but <tt>+</tt>.

### Function syntax

In OpenRefine expression language function can be invoked using either of these 2 forms:

- <tt>functionName(arg0, arg1, ...)</tt>
- <tt>arg0.functionName(arg1, ...)</tt>

The second form above is a shortcut to make expressions easier to read.

It's only syntactic sugar or shorthand, such as:

| dot shorthand notation | full notation |
| --- | --- |
| <tt>value.trim().length()</tt> | <tt>length(trim(value))</tt> |
| <tt>value.substring(7, 10)</tt> | <tt>substring(value, 7, 10)</tt> |
| <tt>value.substring(13)</tt> | <tt>substring(value, 13)</tt> |

The first argument to a function can be swapped out in front of the function to formulate the dot notation. That is easier to read as the functions occur from left to right in the order of calling, rather than in the reverse order.

The dot notation can also be used to access member fields:

| example | description |
| --- | --- |
| <tt>cell.value</tt> | same as just <tt>value</tt> because <tt>cell</tt> stands for the current cell |
| <tt>row.index</tt> | index of the current row |

For member fields whose names are not words (e.g., they contain spaces or other characters), use the bracket notation:

| <tt>cells["First Name"]</tt> | access the cell in the column called "First Name" of the current row |

### [[Array syntax|GREL Array Functions]]

Brackets can also be used to get substrings and sub-arrays, and their syntax resembles Python a bit more than Javascript:

| example | description |
| --- | --- |
| <tt>value[1,3]</tt> | access the substring of <tt>value</tt> starting from character index 1 up to but excluding character index 3 |
| <tt>"internationalization"[1,3]</tt> | return <tt>nt</tt> |
| <tt>"internationalization"[1,-2]</tt> | return <tt>nternationalizati</tt> (negative indexes are counted from the end) |

### [[Controls|GREL Controls]]

GREL supports branching and looping (e.g., "if" and "for") slightly differently than Javascript. To do branching and looping, you use GREL "controls", or sometimes also called "constructs", and their syntax is more like Excel's IF:

| example | description |
| --- | --- |
| <tt>if(value.length() &gt; 10, "big string", "small string")</tt> | if the length of what <tt>value</tt> stands for is great than 10 characters, then return "big string", otherwise, return "small string" |
| <tt>if(mod(row.index, 2) == 0, "even", "odd")</tt> | if the 2 modulus of the row index is zero, then output "even", otherwise, output "odd" |

Thus, the <tt>if</tt> control has this syntax

<tt>if (</tt> test\_condition <tt>,</tt> true\_result <tt>,</tt> false\_result <tt>)</tt>

The test\_condition sub-expression is evaluated. If it yields true, then the true\_result sub-expression is evaluated; otherwise, the false\_result sub-expression is evaluated.

GREL doesn't support looping in the conventional meaning, as supported by <tt>for</tt> in Javascript. However, you can still process arrays of things using the various <tt>for-</tt> controls, e.g.,

| <tt>forEach("Once upon a time in Mexico".split(" "), v, v.length())</tt> | return array of lengths of words, <tt>[4, 4, 1, 4, 2, 6]</tt> |

The <tt>forEach</tt> control has this syntax

<tt>for (</tt> array\_subexpr <tt>,</tt> element\_var\_name <tt>,</tt> element\_subexpr <tt>)</tt>

The array\_subexpr sub-expression is evaluated to an array. For each element in that array, element\_subexpr sub-expression will be evaluated with the variable named by element\_var\_name standing for that element. In the example above, when <tt>v.length()</tt> is evaluated for the first element--the string "Once", the variable called <tt>v</tt> stands for that string "Once", and the sub-expression <tt>v.length()</tt> evaluates to <tt>4</tt>.

Another useful control is <tt>with</tt> that can be used to define a new variable. For instance, if we want to compute the average word length for the string "Once upon a time in Mexico", then we might start with

```
forEach("Once upon a time in Mexico".split(" "), v, v.length()).sum() / "Once upon a time in Mexico".split(" ").length()
```

We have <tt>"Once upon a time in Mexico".split(" ")</tt> repeated twice. To shorten that expression, we can define a variable to stand in that sub-expression's place:

```
with("Once upon a time in Mexico".split(" "), a, forEach(a, v, v.length()).sum() / a.length())
```

Thus, the <tt>with</tt> control has this syntax

<tt>with (</tt> subexpr1 <tt>,</tt> var\_name <tt>,</tt> subexpr2 <tt>)</tt>

First, the sub-expression subexpr1 is evaluated, and then a new variable called var\_name is defined to stand in its place while the sub-expression subexpr2 is evaluated.

