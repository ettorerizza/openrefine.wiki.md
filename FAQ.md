_Frequently Asked Questions_

This is a collection of frequently asked questions on OpenRefine. Feel free to ask your own question on [http:_groups.google.com/group/openrefine/_](the+OpenRefine+mailing+list) and we'll try to answer to the best of our abilities and add them to this list.

#### OpenRefine does not start after clicking the .exe, it only opens and closes a window

- Ensure that you have a Java JRE installed on your system. And have at least 1 GB of RAM available for it.

#### Out of Memory Errors - Feels Slow - Could not reserve enough space for object heap

- [FAQ: Allocate More Memory](I+get+out+of+memory+errors+-+Symptoms%3A+%7B%7B%7Bjava.lang.OutOfMemoryError%7D%7D%7D+or+Refine+feels+slow.)

#### Where is my data stored?

- [FAQ: Where Is Data Stored?](Where+is+the+data+stored%3F)

#### I have a question. Where do I ask?

Send your question to [http:_groups.google.com/group/openrefine/_](the+OpenRefine+mailing+list).

#### I've found a bug or want a new feature. What should I do?

Consider first discussing it on [http:_groups.google.com/group/openrefine/_](the+mailing+list). This will likely help characterize the issue for a good quality bug report or feature request which you can file on the [http:github.com/OpenRefine/OpenRefine/issues](issue+tracker).

#### How do I change the workspace directory that I want Refine to use for its project storage ?

- On Linux, If you run Refine from the terminal, you can point to the workspace directory through the -d parameter, e.g.,
```
./refine -p 3333 -i 0.0.0.0 -m 6000M -d /where/you/want/the/workspace
```

- Alternatively, you can update and add a preference at [http:_127.0.0.1:3333/preferences_](http://127.0.0.1:3333/preferences) ,
```
KEY = refine.data_dir
  VALUE = T:\MyOpenRefineDataFolder
```

#### How do I change the IP address that OpenRefine uses?

On Linux, Mac from the command line,

<tt>./refine -i 127.0.0.1</tt>.

On Windows use a slash character like,

<tt>C:&gt;refine /i 127.0.0.1:8088</tt>)

#### How do I change the Port that OpenRefine uses?

On Linux, Mac from the command line,

<tt>./refine -i 127.0.0.1 -p 3334</tt>

On Windows, use a slash character like,

<tt>C:&gt;refine /i 127.0.0.1 /p 3334</tt>)

You can also edit the <tt>refine.ini</tt> file to permanently set the IP Address and Port.

#### I am having trouble connecting to OpenRefine with my browser

You might need to double check your Chrome or Firefox proxy settings. In Firefox options->advanced->network->connection->settings and switch from "use system proxy settings" to "auto-detect proxy settings for this network".

If you get a message "Network Error (tcp\_error)" in your browser, you might also try to uncheck "automatically detect settings" and also add an exception to your firewall rules to allow 127.0.0.1 (or whatever IP address you decide to run OpenRefine with)

#### What syntax of regular expression (regex) does OpenRefine support?

The regular expression syntax for GREL is that of Java regex, not of Javascript. See [Understanding Regular Expressions](GREL+Regular+Expressions).

You can also use [http:_www.jython.org/docs/library/re.html_](Jython+Regex) instead of GREL functions and use a Custom Text Facet with something like this:

```
import re
g = re.search(ur"\u2014 (.*),\s*BWV", value)
return g.group(1)
```

#### What syntax should I use with GREL for constructing URLs correctly and avoid HTTP errors and other pitfalls, for instance, when working with JSON strings within a URL or to create a HYPERLINK, etc ?

A good practice is to use <tt>'</tt> single quotes for your Refine Expression syntax and reserve <tt>"</tt> double quotes for the URL syntax parts. Also make sure to escape() your cell values used, where necessary.

EXAMPLES:

```
'https://www.googleapis.com/freebase/v1/mqlread?query={"mid":null,"/type/object/key":{"namespace":"/authority/fmd/model","value":"'+escape(cells.ModelName.value, "url")+'"}}'
```
```
'=HYPERLINK("http://listings.listhub.net/pages/BHAMMLSAL/' + value + '",' + value + ')'
```

#### How can I delete a whole row or several rows?

1. Flag (or star) the row(s)
2. In the dropdown above the flag you can get a facet, by going to Facet > Facet by flag.
3. From the facet that opens select the 'true' option.
4. In the dropdown menu above the flag you can go to Edit Rows > Remove all matching rows.

#### How do I make a Text Facet show more than 2000 choices?

You can go to [http://127.0.0.1:3333/preferences](http://127.0.0.1:3333/preferences) and set the facet limit using the preference key "ui.browsing.listFacet.limit".

#### How do I find duplicates in a column?

Several options:

- There is a shortcut for this, Facet → Customized facets → Duplicates facet
- Create a Text Facet on a column and then in the facet click "Sort by: count". Any facets with a count of 2 or more are duplicates
- Use the [GREL Other Functions](facetCount%28%29) function like <tt>facetCount(value, 'value', 'column name') &gt; 1</tt> and select 'true' to show all rows that have duplicates

#### Can OpenRefine be used as a piece of a larger [[ETL|http://en.wikipedia.org/wiki/Extract,\_transform,\_load]] pipeline?

You can use one of the [https:_github.com/OpenRefine/OpenRefine/wiki/Documentation-For-Developers#known-client-libraries-for-refine_](OpenRefine+client+libraries) for automating OpenRefine programatically. If you like docker then you might like [https:github.com/felixlohmeier/openrefine-batch](this+container+approach) to batch processing.

It's worth pointing out that not all Refine features can work unsupervised and without human interaction (clustering, for example), but some can.

Here is some further discussion and a project:

- [http://groups.google.com/group/openrefine/msg/ee29cf8d660e66a9?hl=en](http://groups.google.com/group/openrefine/msg/ee29cf8d660e66a9?hl=en)
- [https://groups.google.com/group/openrefine-dev/browse\_thread/thread/33374842ccfebfcd#](https://groups.google.com/group/openrefine-dev/browse_thread/thread/33374842ccfebfcd#)
- [https://github.com/dfhuynh/grefine-proxy](https://github.com/dfhuynh/grefine-proxy)

#### cross() function does not work for me

You might be missing a few steps that need to be performed before you can use the <tt>cross()</tt> function and expect it to match the keys between the 2 projects correctly.

- <tt>trim()</tt> your key column before doing <tt>cross()</tt>
- De-duplicate values in your key column if necessary
