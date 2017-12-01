_Other functions supported by the [General Refine Expression Language](OpenRefine+Expression+Language) (GREL)_

See also: [GREL Functions](All+GREL+functions).

### type(o)

Returns the type of <tt>o</tt>, such as <tt>undefined</tt>, <tt>string</tt>, <tt>number</tt>, etc.

### hasField(o, string name)

Returns a boolean indicating whether <tt>o</tt> has a field called <tt>name</tt>. For example, <tt>cell.hasField("value")</tt> always returns <tt>true</tt>, as every cell has a <tt>value</tt> field.

### jsonize(value)

Quotes a value as a JSON literal value.

### parseJson(string s)

Parses <tt>s</tt> as JSON. <tt>get</tt> can then be used with <tt>parseJson</tt>, e.g.,

```
parseJson(" { 'a' : 1 } ").get("a")
```

returns 1.

To get all instances from a JSON array called "keywords" having the same object string name of "text", combine with the <tt>forEach()</tt> function to iterate over the array, e.g.,

JSON:

```
{
   "status":"OK",
   "url":"",
   "language":"english",
   "keywords":[
      {
         "text":"York en route",
         "relevance":"0.974363"
      },
      {
         "text":"Anthony Eden",
         "relevance":"0.814394"
      },
      {
         "text":"President Eisenhower",
         "relevance":"0.700189"
      }
   ]
}
```

GREL expression:

```
forEach(value.parseJson().keywords,v,v.text).join(":::")
```

will output like this:

<tt>York en route:::Anthony Eden:::President Eisenhower</tt>

### cross(cell c, string projectName, string columnName)

Returns an array of zero or more rows in the project <tt>projectName</tt> for which the cells in their column <tt>columnName</tt> have the same content as cell <tt>c</tt> (similar to a lookup).

**NOTE: There is also a VIB-BITS OpenRefine extension that automates some of the GREL <tt>cross()</tt> functionality for ease of use with its "Add column(s) from other projects..." function.**

- [https://groups.google.com/forum/#!topic/openrefine/unN6dQsyYUs](https://groups.google.com/forum/#!topic/openrefine/unN6dQsyYUs)
- [https://www.bits.vib.be/index.php/software-overview/openrefine](https://www.bits.vib.be/index.php/software-overview/openrefine)

**NOTE: If you don't get matches when using cross(), then you might need to first do a few things, such as**

- trim() your key column before doing cross()
- Deduplicate values in your key column if necessary

OK, with that out the way, lets show you an example of how to use the cross() function. Consider 2 projects with the following data:

**My Address Book**

| friend | address |
| --- | --- |
| john | 120 Main St. |
| mary | 50 Broadway Ave. |
| anne | 17 Morning Crescent |

**Christmas Gifts**

| gift | recipient |
| --- | --- |
| lamp | mary |
| clock | john |

Now in the project "Christmas Gifts", we want to add a column containing the addresses of the recipients, which are in project "My Address Book". So we can invoke the command <tt>Add column based on this column</tt> on the "recipient" column in "Christmas Gifts" and enter this expression:

```
cell.cross("My Address Book", "friend")[0].cells["address"].value
```

When that command is applied, the result is

**Christmas Gifts**

| gift | recipient | address |
| --- | --- | --- |
| lamp | mary | 50 Broadway Ave. |
| clock | john | 120 Main St. |

To extract more than one value from the result of a cross, combine with the <tt>forEach()</tt> function to iterate over the array and the <tt>forNonBlank()</tt> function to prevent errors caused by null values present in the looked up column, e.g.

```
forEach(cell.cross("My Address Book", "friend"),r,forNonBlank(r.cells["address"].value,v,v,"")).join("|")
```

### facetCount(choiceValue, string facetExpression, string columnName)

Returns the facet count corresponding to the given choice value

| gift | recipient | price |
| --- | --- | --- |
| lamp | mary | 20 |
| clock | john | 54 |
| watch | amit | 80 |
| clock | claire | 62 |

If we wanted to show how many of each gift we had given, we could use the following expression:

```
facetCount(value, "value", "gift")
```

This would then complete the table as so:

| gift | recipient | price | count |
| --- | --- | --- | --- |
| lamp | mary | 20 | 1 |
| clock | john | 54 | 2 |
| watch | amit | 80 | 1 |
| clock | claire | 62 | 2 |

## Jsoup HTML parsing functions

* * *

A tutorial of the below HTML functions is shown [StrippingHTML](here.)

### parseHtml(string s)

returns a full HTML document and adds any missing closing tags.

You can then extract or <tt>.select()</tt> which portions of the HTML document you need for further splitting, partitioning, etc.

An example of extracting all table rows <tt>&lt;tr&gt;</tt> from a <tt>&lt;div&gt;</tt> using <tt>parseHtml().select()</tt> together is described more in [StrippingHTML](Extract+HTML+attributes%2C+text%2C+links+with+integrated+GREL+commands).

### select(Element e, String s)

returns an element from an HTML doc element using selector syntax.

Example:

<tt>value.parseHtml().select("div#content")[0].select("tr").toString()</tt>

This function can be used with most of the Jsoup selector syntax as documented here: [http://jsoup.org/cookbook/extracting-data/selector-syntax](http://jsoup.org/cookbook/extracting-data/selector-syntax)

### htmlAttr(Element e, String s)

returns a value from an attribute on an HTML Element.

### htmlText(Element e)

returns the text from within an element (including all child elements).

### innerHtml(Element e)

returns the innerHtml of an HTML element.

### outerHtml(Element e)

returns the outerHtml of an HTML element. (Note: outerHtml Indicates the rendered text and HTML tags including the start and end tags, of the current element.)

### ownText(Element e)

Gets the text owned by this HTML element only; does not get the combined text of all children.

