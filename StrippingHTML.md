_Extracting & Converting HTML markup_

# Introduction

At some point you may have a dataset where one or more of the columns contain HTML markup. The markup tags may interfere with data extraction/cleansing, word counts, etc.

It's very easy to create a new column with the HTML tags removed using OpenRefine's support for regular expressions.

- Attribution note: the regular expression below comes from [http:_haacked.com/archive/2004/10/25/usingregularexpressionstomatchhtml.aspx_](Phil+Haack%27s+blog).\*

# Details

Assume you have a column **Col1** containing HTML markup. Click on the dropdown next to **Col1** and choose "Edit Column > Add Column Based On This Column...". Pick a name for the new column, and use the following expression:

```
replace(value,/<\/?\w+((\s+\w+(\s*=\s*(?:".*?"|'.*?'|[^'">\s]+))?)+\s*|\s*)\/?>/,'')
```

Press OK, and you'll soon have a new column with the plain text extracted from **Col1**.

* * *

# Extract HTML attributes, text, links with integrated GREL Jsoup commands

**WARNING** : Make sure to use .toString() suffixes when needed to output strings into Refine cells while working with the built-in HTML GREL commands (the default output is org.jsoup.nodes objects). Otherwise you'll get a preview just fine in the Expression Editor, BUT no data shown in the Refine cells when you apply it!

**Note** : Now included in 2.1 version are HTML functions (Thanks Iain Sproat!) built upon [http:_jsoup.org_](jsoup.org) which is a Java library built on [http:www.crummy.com/software/BeautifulSoup/](BeautifulSoup).

**IMPORTANT TIP** For full documentation on using the integrated jsoup commands, you will probably want to refer back to jsoup's selector syntax itself [http://jsoup.org/cookbook/extracting-data/selector-syntax](http://jsoup.org/cookbook/extracting-data/selector-syntax) .

Useful Common Examples:

Extract all the <tt>&lt;table&gt;</tt> rows from a <tt>&lt;div ID=content&gt;</tt>:

```
value.parseHtml().select("div#content")[0].select("tr").toString()
```

Simple Web Scraping (Web Scraper) can be made in OpenRefine with a pattern. For example, you can add a new column based on an HTML page that has a bunch of links, where you need to Loop and Extract (using <tt>forEach()</tt>) all the <tt>&lt;a href=&gt;</tt> links based upon a regex pattern (such as those links containing a number digit   
d+) and join the array so that you can split and Fetch URLs on all those extracted links to scrape even more data off each HTML link with another Fetch URLs pass. :

```
forEach(value.parseHtml().select("a[href~=\\d+]"),e,e.htmlAttr("href")).join("SplitCharsGoHere")
```

After adding that new column with the above GREL snippet, you can then do Edit Cells -> Split multi-valued cells... and use your separator chars phrase, such as <tt>"SplitCharsGoHere"</tt> or <tt>"--SPLITME--"</tt> or <tt>"|||||"</tt> ; Whatever chars you choose to do your <tt>join()</tt>.

After the Split multi-valued cells operation finishes, you should be left in records mode view (rather than row mode view) and you now have a bunch of URLs extracted from EACH original HTML record where you can perform another Fetch URLs operation on all of them.

