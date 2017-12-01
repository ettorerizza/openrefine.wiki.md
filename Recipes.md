_Useful recipes for achieving certain tasks in OpenRefine_

This page collects OpenRefine recipes, small workflows and code fragments that show you how to achieve specific things with OpenRefine.

## String Manipulation

Here are some examples of possible types of common string manipulation operations that you might encounter and how they can be achieved with the [GREL-Functions](Refine+Expression+Language+%28GREL%29). See also [GREL String Functions](GREL+String+Functions).

### Extract out the first value in a multi-valued cell

So if the cell was:

```
Doe, J; Smith, JD; Andersen SL
```

And you want your new cell to be:

```
Doe, J
```

You can use the GREL function 'split'. Because you want this in a new column, you need to use the 'Column->Add column based on this column' menu option. And use the GREL:

```
value.split(";")[0]
```

The first part (value.split(";")) will split the value into an array (list) using semicolon as the character to split it up. The [0] directs OpenRefine to take the first item from the list created by the 'split' command. (and you can guess, using [1] instead would take the second value, [2] the third, etc.)

### Change "2010-05-31T01:10:0Z" to "05/31/2010"

One way to do this is use GREL's slice function.

```
value.slice(5, 7) + '/' + value.slice(8, 10) + '/' + value.slice(0, 4)
```

Another way to do this is to convert this to a Date data type, then convert back to a string with the specified format:

```
toString(toDate(value),"dd/MM/yyyy")
```

### Trim whitespace from beginning and end of values
```
value.trim()
```

or if you have non-breaking spaces <tt>&amp;nbsp</tt> on both ends then you might try this instead:

```
split(escape(value,'xml'),"&#160;")[0]
```

If you don't know what kind of whitespace character is present, and they are not removed using 'trim' you can create a new column using:

```
value.escape("javascript")
```

This should reveal the types of whitespace that are in the original column, and allow you to target them for removal.

### Titlecase that works on hyphenated names

If you have a "name" column with values like "FOTHERINGTON-THOMAS", this will convert correctly to "Fotherington-Thomas" (more detailed explanation [http:_www.rmorrison.net/mnemozzyne/google-refine-and-fotherington-thomas-issue_](here)

replace(toTitlecase(replace(value,"-",".")),".","-")

### Replacing Chars, Punctuation, etc using regular expressions

Replace will take a regular expression, e.g.

```
value.replace(/[%@#!]/,' ').
```

You can also use character classes to replace all punctuation characters with spaces

```
value.replace(/\p{Punct}/,' ')
```

If you ever do have to do multiple replaces, you can chain them together e.g.

```
value.replace('w','x').replace('y','z')
```

### "someprefix\_a2343" -> "a2343"

You can use

```
value.replace("someprefix_","")
```

basically replacing what you don't want with the empty string.

Note that "replace" replaces all instances of that first string in the cell value so see the next recipe for that case.

#### "blah\_2342\_blah\_1232" -> "2342\_blah\_1232"

Here we can't replace "blah" with "" because it would change the second occurrence as well. So we can use this

```
value.partition("blah_")[2]
```

that will chop the value in an array of three strings

```
["", "blah_", "2342_blah_1232"]
```

and we get the 3rd value (remember that the arrays in OpenRefine, like in most programming languages start from 0, so <tt>[2]</tt> is actually the third value).

### "a:b:c:d:e" -> "b:c:d"

Sometimes the strings are more complicated and the only thing they have in common is a character separator. In that case it's handy to use "split" and "join" together like this:

```
value.split(":")[1,3].join(":")
```

which "splits" the string around the ":" separating character in an array of 5 strings

```
["a","b","c","d","e"]
```

then you select only the range between the 2nd and 4th and you get

```
["b","c","d"]
```

then you join them together with the same separator character ":" and you get what you wanted "b:c:d".

### Unique separator for split / join multi-valued cells

Do not choose a string as separator that might be present somewhere in your data. Consider using these uncommon UTF-8 strings via copy & paste:

- Unit Separator: ␟ (U+241F)
- Record Separator: ␞ (U+241E) 

### Pad with leading zeroes

If you need to fill out a number with for example leading zeroes up to four characters try,

```
"0000"[0,4-value.length()] + value
```

This provides a string of zeroes from which we ask for the first 4-length(value) characters using the string slicing action of [start, offset].

### Separate letters and digits e.g. Aug13 -> Aug 13
```
value.replace(/(\p{IsAlphabetic})(?=\d)/,'$1 ')
```

- (\p{IsAlphabetic}) is any Unicode alphabetic character, wrapping it in parens makes it a "matching group"
- (?=\d) is a "lookahead". it basically means "followed by any digit"
- $1 means "first matching group", or the letter that was matched by the first bullet above. Adapted from this [http:_stackoverflow.com/questions/11941487/javascript-regexp-if-a-letter-is-adjacent-to-a-number-add-an-underscore_](StackOverflow+answer).

### Parse an IP address to extract a Country Name, Latitude & Longitude

Create a new column by fetching URLs based on a column with IP Addresses using an expression like:

```
"http://freegeoip.net/json/\"+value
```

There are many such services such as that one. Then parse the resulting **json** to create new columns with expressions such as:

```
* value.parseJson().country_name
* value.parseJson().latitude
* value.parseJson().longitude
```

### Get HTTP Response Header field (e.g. redirect location)

This clever hack from @atomotic shows how to use Python to get the location header field from a redirect response (in this case from the DOI API).

```
import httplib
conn = httplib.HTTPConnection("dx.doi.org")
doi = "/"+value
conn.request("HEAD", doi)
res = conn.getresponse()
return res.getheader('location')
```

or in a single line version that you could paste into your Undo history

<tt>jython:import httplib\nconn = httplib.HTTPConnection(\"dx.doi.org\")\ndoi = \"/\"+value\nconn.request(\"HEAD\", doi)\nres = conn.getresponse()\nreturn res.getheader('location')"</tt>

### Parse a URL or URI to extract a Host, Path, Query, or convert to a URI

Use Clojure as your expression language and use one of these expressions (consult [http:_docs.oracle.com/javase/6/docs/api/java/net/URL.html_](java+URL+or+URI+docs) for other methods besides just the .getHost() method.

```
(.getHost(java.net.URL. http://www.example.com/index/search/q=%22OpenRefine%22))
```
```
(.getHost(java.net.URI. value))
```
```
(.getPath (java.net.URI. value))
```
```
(.getQuery (java.net.URI. value))
```
```
(.toURI (java.net.URL. value))
```

### Get HTTP header field value (e.g. redirect location)

This clever hack

### Parse JSON and Create Custom Arrays using forEach()
```
forEach(value.parseJson().result,v,[v.score,v.name,v.mid])
```

then getting your 1st custom result:

```
forEach(value.parseJson().result,v,[v.score,v.name,v.mid])[0]
```

to get all instances from a JSON array called "keywords" having the same object string name of "text", combine with the forEach() function to iterate over the array:

```
forEach(value.parseJson().keywords,v,v.text).join(":::")
```

### Removing duplicate rows when Exact values are found in a column

In Release 2.1+ there is now a Facet -> Customized Facets -> Duplicates Facet that can be applied on more than one column as needed. This easily applies for you a facetCount(value, 'value', 'Column') > 1 which shows ALL rows having more than 1 duplicate value in the column.

However, to ONLY remove subsequent instances of duplicates (any string), use this method:

1. Sort (on the column that has duplicates or potentially)
2. Reorder rows permanently (this option is in blue color SORT link in header)
3. Edit cells -> Blank down
4. Facet -> Customized Facets -> Facet by blank (click True)
5. All -> Edit rows -> Remove all matching rows

### Handling duplicate patterns found in cells within a column

The canonical recipe for stuff like this is to Move Column of interest to the beginning, Sort, Reorder Rows Permanently, Blank Down, <do stuff>, Fill Down. This effectively converts your rows to OpenRefine pseudo-"records" so that you can do further record based operations. GREL supports special record based commands.

### Facet and Count duplicate patterns found in a cell value at each row

With strings that have duplicate words in them such as "Beef, Ground Beef", you may wish to see a count of the duplicates in each column's row. Using value.ngram(1) only separates the words, but doesn't give a count. So, first, standardize spacing in the cell value or find good alternate separation chars to use on your particular pattern if needed, and perhaps clean up spaces with GREL tranform of

```
value.replace(/\s+/,' ')
```

then using Jython, you can create a Custom Facet using something such as:

```
from sets import Set
# function to show count of Duplicates in a List returning an array
def countDuplicatesInList(dupedList):
   uniqueSet = Set(item for item in dupedList)
   return [(item, dupedList.count(item)) for item in uniqueSet]

v = countDuplicatesInList(value.split(" "))
for x in v[:]: # makes a slice copy of the entire list
  if x[1]>1: # compares if the 2nd value in each mini list (x) is greater than 1, such as ["Beef", 2] 
    return x
```

### Create a new column based on the value of "Star" or "Flag"

Starring or Flagging rows is a handy way of accumulating insights based on many different facets about messy data. Create a new column based on the Star, use:

```
if(row.starred, "yes", "no")
```

### Find a sub pattern that exists at the end of a string

Sometimes you have a need beyond just <tt>endsWith</tt> to find a varying substring and want to know if that variable pattern is found at the end of a string. You can add a Custom Text Facet and find this variable pattern with regex delmited by <tt>/</tt> characters such as <tt>/.*(PR\d+)/</tt>

```
value.endsWith(get(value.match(/.*(PR\d+)/),0))
```

Here you have to wrap with <tt>get()</tt> in order to ask if it <tt>endsWith</tt> the string version of the returned array from <tt>match</tt>. <tt>endsWith</tt> then returns a boolean of True or False if the string ends with your regex pattern, in this case, cells that end with reference numbers that begin with PR followed by any number of decimal digits.

### Remove the last word in a string

This uses <tt>smartSplit</tt> to find the last word <tt>[-1]</tt> and then wraps <tt>partition</tt> to keep only the first phrase or part of the string remaining with <tt>[0]</tt>.

```
value.partition(smartSplit(value," ")[-1])[0]
```

### "00003400340300004" -> ["000034","0034","03","00004"]

Sometimes your content has no char separator because the separation is performed by alignment (in the case of non-delimited files) and padded to certain sizes. In that case you can use

```
value.splitByLengths(6,4,2,5)
```

where those numbers are the length of the fields respectively.

### split / map / join

Running this expression

```
forEach(value.split(" "), v, v + v.length()).join(";")
```

on the text

```
abc defg hi
```

yields

```
abc3;defg4;hi2
```

A similar trick can be used to compute, say, word length average

```
with(value.split(" "), a, sum(forEach(a, v, v.length())) / a.length())
```

### Merging several columns

You'd need to use the 'cells' variable:

```
cells["col1"].value + ", " + cells["col2"].value
```

If any of cells in the columns are blank, the merge will fail for that row. To fix, create a facet of blank cells with "Text Facet" ⇒ "Customized Facets" ⇒ "Facet by Blank". Then use "Edit Cells" ⇒ "Transform ..." and enter a string with a space: <tt>' '</tt>.

### Merging all or more than two columns in a project

To merge all values across a row, you can use 'row.columnNames' (which retrieves all the column names in the project) with the 'cells' variable:

```
forEach(row.columnNames,cn,cells[cn].value).join("|")
```

If there are 'null' cells in the row, the row will still merge, but where the cells were null you will get the phrase "Cannot retrieve field from null|" If there are empty strings ("") in any cell in the row, these will be merged as empty strings.

To make null cells and cells with empty strings behave in the same way you can enhance the GREL given above with a check for null cells:

```
forEach(row.columnNames,cn,if(isNull(cells[cn]),"",cells[cn].value))
```

Obviously, you can use <tt>filter()</tt>, <tt>[]</tt> or <tt>slice()</tt> to select which column to merge. Example : merge all columns except the column ID :

```
forEach(filter(row.columnNames, v, v!="ID"),cn,if(isNull(cells[cn]),"",cells[cn].value)).join("|")
```

Or : merge all columns except the first one :

```
forEach(row.columnNames.slice(1),cn,if(isNull(cells[cn]),"",cells[cn].value)).join("|")
```

Another example using <tt>.match()</tt> (Thanks to Stephens Owen for the "lenght()>0" trick)

```
forEach(filter(row.columnNames, v, v.match(/(colname1|colname2|colname3|colname4|colname5)/).length()>0),cn,if(isNull(cells[cn]),"",cells[cn].value)).join("|")
```

### Facet for rows with a certain number of blank cells

If you want to find rows that have a certain number of blank cells, including 'all blank' or 'no blanks' you can create a custom facet:

Facet->Custom text facet (from any column in the project)

use the GREL:

```
filter(row.columnNames,cn,isBlank(cells[cn].value)).length()
```

This will give a facet with the number of blank (null or empty strings) cells in each row. If you want to limit by a particular number you can enhance the GREL with a check for a specific length: **Rows with more than one blank cell (true/false facet)**

```
filter(row.columnNames,cn,isBlank(cells[cn].value)).length()>1
```

**Rows with exactly one blank cell (true/false facet)**

```
filter(row.columnNames,cn,isBlank(cells[cn].value)).length()==1
```

**Rows with less than two blank cells (true/false facet)**

```
filter(row.columnNames,cn,isBlank(cells[cn].value)).length()<2
```

### "AT&amp;amp;T" --> "AT&T"

Sometimes you get content that was extracted from web pages or XML documents and contain entities. These are very hard to deal with manually, mostly because there are tons of potential string values between "&" and ";". OpenRefine provides you a convenient command to do this in GREL

```
value.unescape("html")
```

Note the few entities are not part of HTML and are part of XML, so if you want to be safe, you can do

```
value.unescape("html").unescape("xml")
```

which will take care of them both.

Note that sometimes a string can get escaped several times: "AT&amp;amp;amp;amp;amp;T" (yes, trust us, we've seen that in presumably good data sets). When you do a transform on such cells, make sure you check the "Re-transform until no change" checkbox. This causes the expression to get run on each cell's content until the output is the same as the input (or up to 10 times).

### XML parsing & stripping

Suppose you have XML or ATOM feed data in your cells that you may have gathered with the Add Column by Fetching URLs feature in Refine or some other source and you need to parse and strip certain tag elements such as:

```
<entry>
        <title>San Francisco chronicle.</title>
        <link href="http://chroniclingamerica.loc.gov/lccn/sn82003402/" />
        <id>info:lccn/sn82003402</id>
        <author><name>Library of Congress</name></author>
        <updated>2010-09-03T00:01:07-04:00</updated>
    </entry>

    <entry>
        <title>The San Francisco call.</title>
        <link href="http://chroniclingamerica.loc.gov/lccn/sn85066387/" />
        <id>info:lccn/sn85066387</id>
        <author><name>Library of Congress</name></author>
        <updated>2010-09-03T00:18:25-04:00</updated>
    </entry>
```

You can strip out the elements that your interested in from any XML or ATOM feed and create an array of those elements values for further processing. In the above example, I only want the lccn id and title for all the **entry** records. This is useful when working with the [http://chroniclingamerica.loc.gov](http://chroniclingamerica.loc.gov) API or any XML or ATOM feed.

```
forEach(value.split("<entry>"), v, v.partition("<id>info:lccn/")[2].partition("</id>")[0]+
","+v.partition("<title>")[2].partition("</title>")[0])
```

The above basically says that for each entry record (which creates an array), I want only the string partition between my **id** tags and **title** tags. The resulting output then looks something like:

```
[ ",Chronicling America Title Search Results",
 "sn82003402,San Francisco chronicle.",
 "sn85066387,The San Francisco call.",
 "sn86091369,San Francisco Sunday examiner &amp; chronicle.",
 "sn82006825,The San Francisco examiner." ]
```

### Replacing diacritic (accent) characters

Boris Villazón -> Boris Villazon

See Jython use case [Extending-Jython-with-pypi-modules](Extending-Jython-with-pypi-modules)

### "AÃ¯n TÃ©muchent" ---> "Aïn Témuchent"

Sometimes content is aggregated from various sources and some where improperly decoded. This results in strings that contain 'garbled' content in the form of weird spurious characters. Fixing that is normally a very tedious and time consuming process, but OpenRefine contains a handy GREL command specifically for that

```
value.reinterpret("utf-8")
```

that does exactly that.

As a tip, if you see two weird characters where you think there should be one, try reinterpreting with "utf-8" which normally solves it. Other encoding values that are useful to try are "latin-1" (for european content), "Big5" (for chinese content"), etc. You can get a list of all the supported encodings [http:_java.sun.com/j2se/1.5.0/docs/guide/intl/encoding.doc.html_](here). Re-encoding can also be done outside of OpenRefine with text tools such as PSPad, Notepad++, etc.

### Question Marks � showing in your data

This is the "Replacement Character - used to replace an incoming character whose value is unknown or unrepresentable." These will show if there are encoding issues with your data. Because the data is actually changed at import time, this may not be recoverable without re-importing, but you can try reinterpreting as above. Also there may be non-breaking spaces <tt>&amp;nbsp</tt> Unicode(160). You can inspect with <tt>unicode(value)</tt> and replace with regular spaces Unicode(32).

You can also try this quick fix to remove non-breaking spaces on both ends of your string:

```
split(escape(value,'xml'),"&#160;")[0]
```

**NOTE** To access and use the Non-breaking space character itself such as <tt>&amp;nbsp</tt> or <tt>\u00A0</tt> or <tt>[160]</tt> in GREL expressions on Windows systems type on the numeric keypad: ALT 2 5 5 ,which will still display as a regular space in the expression window, but will actually be a non-breaking space character. Example: <tt>value.replaceChars(" ", " ")</tt>

## Numerical Conversions

### Convert to Decimal Latitude or Longitude

For a string holding longitude/latitude information in the format XX degrees, XX minutes, XX.XX seconds the following can be used.

```
forNonBlank(value[0,2], v, v.toNumber(), 0) +
   forNonBlank(value[2,4], v, v.toNumber()/60, 0) +
   forNonBlank(value[4,6], v, v.toNumber()/3600, 0) +
   forNonBlank(value[6,8], v, v.toNumber()/360000, 0)
```

### Convert Epoch time to Date/Time as String

For a string holding an Epoch time (number of seconds that have elapsed since January 1, 1970 midnight UTC/GMT) convert into an ISO8601 string. This can be done using a [Jython]( [https://github.com/OpenRefine/OpenRefine/wiki/Jython](https://github.com/OpenRefine/OpenRefine/wiki/Jython)) transformation: ``` import time; epochlong = int(float(value)); datetimestamp = time.strftime('%Y-%m-%dT%H:%M:%SZ', time.localtime(epochlong)); return datetimestamp ```

This creates a String. You can subsequently transforms this string with ``` value.toDate() ``` To covert it to an OpenRefine Date

## Error Handling

### forNonBlank

Sometimes the cells in a column are not uniform, but your expression has to be flexible enough to handle them all. One case of nonuniformity is that the strings in the cells are not all of the same length, e.g., 3 cells in a column might contain

```
200810
  20090312
  2010
```

This means that this formula

```
value[0,4] + "-" + value[4,6] + "-" + value[6,8]
```

only works on the second cell. To catch the other cases, use the forNonBlank control:

```
value[0,4] + forNonBlank(value[4,6], v, "-" + v, "") + forNonBlank(value[6,8], v, "-" + v, "")
```

This new expression transforms those cells to

```
2008-10
  2009-03-12
  2010
```

What forNonBlank does is test its first argument and decides what to do based on whether that is blank (null or empty string) or not. If it's not blank, bind that value to the variable name in the second argument (v) and evaluates the third argument ("-" + v). Otherwise, it evaluates the fourth argument ("").

### Replace string list entries

You have a cell with joined image URL entries that you have downloaded and named by an <tt>identifier</tt> + <tt>-Image-N</tt>.

``` " [https://url/2a4d7bd5c8e04c.png](https://url/2a4d7bd5c8e04c.png)", " [https://url/bbcfb4168a3.png](https://url/bbcfb4168a3.png)", " [https://url/03b9239f6c514ec.png](https://url/03b9239f6c514ec.png)", " [https://url/c02ae6689f1d4b14.png](https://url/c02ae6689f1d4b14.png)", " [https://url/67438c.png](https://url/67438c.png)" ```

You like to rename the list to <tt>"identifier-Image-N", "..", ".."</tt> so you get this result

``` "identifier-Image-0.png","identifier-Image-1.png","identifier-Image-2.png","identifier-Image-3.png","identifier-Image-4.png" ```

You can do this with this expression:

``` join(forEachIndex(split(value,","),i,v,"\"" + cells['Identifier'].value + "-Image-" + i + "." + split(v,".")[length(split(v,"."))-1]), ",") ```

## Spot Potential Encoding Issues

While OpenRefine offers a convenient way to fix encoding issues with the "reinterpret" GREL command, it also helps you find where these problems could be since they can be hidden if most of the content has been properly decoded.

To do this, there is a special numeric facet called "Unicode charcode Facet" that generates a distribution of which unicode characters are used in a particular column. That distribution will allow you to spot outliers, meaning characters that are used infrequently and that might suggest encoding issues. You can use that char distribution facet to 'scan' and inspect the values for yourself and then use fix their encoding with the 'reinterpret' GREL function.

## Spot Values Potentially Placed in the wrong Column

Another useful numeric facet is the "Text Length Facet" which builds a distribution of lengths of the strings contained in a particular column. Here too spotting outliers, meaning strings that are too big or too small, might be an indication of a problem.

## ISBN Calculations & Manipulations

### ISBN-10

Compute ISBN-10 check digit:<tt>grel:sum(forRange(1,10,1,i,toNumber(value[i])**i))%11</tt>

Check computed value against check digit: <tt>grel:if(value==10,'X',toString(round(value)))==cells['ISBN10'].value[10]</tt>

Convert ISBN-10 check digit to number ('X' == 10) <tt>grel:if(toLowercase(value[10])=='x',10,toNumber(value[10]))</tt>

### Convert an ISBN-10 to ISBN-13

Take the first 9 digits and prepend 978: <tt>grel:'978'+value[0,9]</tt>

Compute the ISBN-13 check digit: <tt>grel:((10-(sum(forRange(0,12,1,i,toNumber(v[i])*(1+(i%2*2)) )) %10)) %10).toString()[0]</tt>

Append the check digit to the 12-digit base

Or, do it all in one expression: <tt>with('978'+value[0,9],v,v+((10-(sum(forRange(0,12,1,i,toNumber(v[i])*(1+(i%2*2)) )) %10)) %10).toString()[0] )</tt>

## Shift values in multiple rows

1. Add a new column based on the Name col:

```
Name > Edit column > Add column based on this column...
```

Call the column 'index' (or similar).

In expression, type: <tt>""</tt>

This creates a new but empty column.

2. Move the new 'index' column to be the first column in the project:

```
index > Edit column > Move column to beginning
```

3. Edit the cell in first row of the 'index' column to have some value in it. It could be anything, e.g. 'first'.

4. Change to 'Record' mode by clicking 'Show as: records' link (top left of project).

This should create you a single record based on the index column only having a value in the first row.

If this doesn't work, then use 'Fill down' and then 'Blank down' on the Index column, and you should find this forces the shift to a single record.

5. Modify the cells in the 'Age' column so empty cells have a placeholder value in them - this is to make sure the next step (which ignores empty cells) works correctly.

```
Age > Edit cells > Transform
```

In the Expression box type:

```
if(isBlank(value),"null",value)
```

6. Move the values in the Age column down one cell

```
Age > Edit cells > Transform
```

In expression type:

```
row.record.cells["Age"].value[rowIndex-1]
```

The <tt>row.record.cells["Age"].value</tt> part of this creates an array of values using all the values in the Age column - because they are now all part of the same Record (which is what steps 1-5 achieved). You can then extract the value from the row above using 'rowIndex' which gives you a row number.

Note that the first row in the project will get the value from the last row in the project using this expression. If this is 'null' you don't need to worry about this.

7. Remove the dummy values in the Age column

```
Age > Edit cells > Transform
```

In expression type:

```
if(value=="null","",value)
```

8. Remove the index column

```
index > Edit column > Remove this column
```

9. Make sure you are back in Row mode by clicking 'Show as: rows' link

### Parse and format Phone Numbers

[Jython#tutorial---working-with-phone-numbers-using-java-libraries-inside-python](Jython%23tutorial---working-with-phone-numbers-using-java-libraries-inside-python)

### Remove/extract words contained in a file

Example: you want to remove from a text all stopwords contained in a file on your desktop. In this case, use Jython.

```
with open(r"C:\Users\ettor\Desktop\stopwords.txt",'r') as f :
        stopwords = [name.rstrip() for name in f]

    return " ".join([x for x in value.split(' ') if x not in stopwords])
```

Obviously, you can use the same method to extract words from cells only if they match those contained in a file. Here is another example with a list of words contained in the first column of a CSV.

```
import csv
    with open(r"C:\Users\ettor\Desktop\list_of_words.csv",'r') as f:
        reader = csv.reader(f)   
        words_to_match = [col[0] for col in reader]

    return [x for x in value.split(' ') if x in words_to_match]
```
