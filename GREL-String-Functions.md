_String functions supported by [General Refine Expression Language](OpenRefine+Expression+Language) (GREL)_

See also: [GREL functions](All+GREL+Functions).

# Basic

### length(string s)

Returns the length of <tt>s</tt> as a number.

# Testing String Characteristics

### startsWith(string s, string sub)

Returns boolean indicating whether <tt>s</tt> starts with <tt>sub</tt>. For example, <tt>startsWith("food", "foo")</tt> returns <tt>true</tt>, whereas <tt>startsWith("food", "bar")</tt> returns <tt>false</tt>. You could also write the first case as <tt>"food".startsWith("foo")</tt>.

### endsWith(string s, string sub)

Returns boolean indicating whether <tt>s</tt> ends with <tt>sub</tt>. For example, <tt>endsWith("food", "ood")</tt> returns <tt>true</tt>, whereas <tt>endsWith("food", "odd")</tt> returns <tt>false</tt>. You could also write the first case as <tt>"food".endsWith("ood")</tt>.

### contains(string s, string sub)

Returns boolean indicating whether <tt>s</tt> contains <tt>sub</tt>. For example, <tt>contains("food", "oo")</tt> returns <tt>true</tt> whereas <tt>contains("food", "ee")</tt> returns <tt>false</tt>. You could also write the first case as <tt>"food".contains("oo")</tt>.

### indexOf(string s, string sub)

Returns integer indicating postion of <tt>sub</tt> within <tt>s</tt> or <tt>-1</tt> if <tt>sub</tt> is not found. For example, <tt>indexOf("food", "oo")</tt> returns <tt>2</tt>, whereas <tt>indexOf("food", "ee")</tt> returns <tt>-1</tt>.

Returning <tt>-1</tt> is equivalent to returning boolean <tt>false</tt>, which is very useful for finding strings that do **NOT** contain <tt>sub</tt>.

# Basic String Modification

## Case Conversion

### toLowercase(string s)

Returns <tt>s</tt> converted to lowercase.

### toUppercase(string s)

Returns <tt>s</tt> converted to uppercase.

### toTitlecase(string s)

Returns <tt>s</tt> converted to titlecase. For example, <tt>toTitlecase("Once upon a midnight dreary")</tt> returns the string <tt>Once Upon A Midnight Dreary</tt>.

## Trimming

### trim(string s) and strip(string s)

Returns a copy of the string, with leading and trailing whitespace removed. For example, <tt>trim(" island ")</tt> returns the string <tt>island</tt>.

### chomp(string s, string sep)

Returns a copy of <tt>s</tt> with <tt>sep</tt> removed from the end if <tt>s</tt> ends with <tt>sep</tt>; otherwise, just returns <tt>s</tt>. For example, <tt>chomp("hardly", "ly")</tt> and <tt>chomp("hard", "ly")</tt> both return the string <tt>hard</tt>.

## Substring

### substring(s, number from, optional number to)

Returns the substring of <tt>s</tt> starting from character index <tt>from</tt> and upto character index <tt>to</tt>. If <tt>to</tt> is omitted, it's understood as the end of the string <tt>s</tt>. For example, <tt>substring("profound", 3)</tt> returns the string <tt>found</tt>, and <tt>substring("profound", 2, 4)</tt> returns the string <tt>of</tt>.

Character indexes start from zero. Negative character indexes are understood as counting from the end of the string. For example, <tt>substring("profound", 1, -1)</tt> returns the string <tt>rofoun</tt>.

### slice(s, number from, optional number to)

See <tt>substring</tt> function above.

### get(o, number or string from, optional number to)

See <tt>substring</tt> function above.

## Find and Replace

### indexOf(string s, string sub)

Returns the index of <tt>sub</tt> first ocurring in <tt>s</tt> as a character index; or <tt>-1</tt> if <tt>s</tt> does not contain <tt>sub</tt>. For example, <tt>indexOf("internationalization", "nation")</tt> returns <tt>5</tt>, whereas <tt>indexOf("internationalization", "world")</tt> returns <tt>-1</tt>.

### lastIndexOf(string s, string sub)

Returns the index of <tt>sub</tt> last ocurring in <tt>s</tt> as a character index; or <tt>-1</tt> if <tt>s</tt> does not contain <tt>sub</tt>. For example, <tt>lastIndexOf("parallel", "a")</tt> returns <tt>3</tt> (pointing at the second character "a").

### replace(string s, string f, string r)

Returns the string obtained by replacing <tt>f</tt> with <tt>r</tt> in <tt>s</tt>. <tt>f</tt> can be a regular expression, in which case <tt>r</tt> can also contain capture groups declared in f.

For a simple example, <tt>replace("The cow jumps over the moon and moos", "oo", "ee")</tt> returns the string <tt>The cow jumps over the meen and mees</tt>.

More info on [Understanding-Regular-Expressions](Regex)

### replaceChars(string s, string f, string r)

Returns the string obtained by replacing any character in <tt>s</tt> that is also in <tt>f</tt> with the character <tt>r</tt>. For example, <tt>replaceChars("commas , and semicolons ; are separators", ",;", " **")</tt> returns the string <tt>commas** and semicolons ** are separators</tt>.

### match(string s, regexp p)

Attempts to match the string <tt>s</tt> in its entirety against the regex pattern <tt>p</tt> and returns an array of capture groups. For example, <tt>match("230.22398, 12.3480", /.*(\d\d\d\d)/)</tt> returns an array of 1 string: <tt>3480</tt>. <tt>match("230.22398, 12.3480", /.*\.(\d+).*\.(\d+)/)</tt> returns an array of 2 strings: <tt>22398</tt> and <tt>3480</tt>.

Another <tt>match()</tt> example:

<tt>isNotNull(value.match(/(a.c)/))</tt>

returns True or False if value is like <tt>abc</tt> or <tt>azc</tt> , etc but would not match a value of <tt>ac</tt>. Remember to use Parentheses when using match() to denote the Capture Group or Groups inside your Regex expression so that you can get output when needed otherwise an empty array [] is shown for no match.

Example: Cell contains value:

hello 123456 goodbye

<tt>value.match(/\d{6}/)</tt> = null

<tt>value.match(/.*\d{6}.*/)</tt> = [] (empty array)

<tt>value.match(/.*(\d{6}).*/)</tt> = ["123456"] (array with one captured value)

<tt>value.match(/(.*)(\d{6})(.*)/)</tt> = ["hello ", "123456", " goodbye"] (array with three captured values)

More info on [Understanding-Regular-Expressions](Regex)

And a nice online learning & testing tool for Regex: [http:_regexr.com/_](RegExr)

# String Parsing and Splitting

### toNumber(o)

Returns <tt>o</tt> converted to a number.

### split(s, sep)

Returns the array of strings obtained by splitting <tt>s</tt> at wherever <tt>sep</tt> is found in it. <tt>sep</tt> can be either a string or a regular expression. For example, <tt>split("fire, water, earth, air", ",")</tt> returns the array of 4 strings: "<tt>fire</tt>", "<tt> water</tt>", " <tt> earth</tt>" , and "<tt> air</tt>". The double quotation marks are shown here only to highlight the fact that the spaces are retained.

More info on [Understanding-Regular-Expressions](Regex)

### splitByLengths(string s, number n1, number n2, ...)

Returns the array of strings obtained by splitting s into substrings with the given lengths. For example, <tt>splitByLengths("internationalization", 5, 6, 3)</tt> returns an array of 3 strings: <tt>inter</tt>, <tt>nation</tt>, and <tt>ali</tt>.

### smartSplit(string s, optional string sep)

Returns the array of strings obtained by splitting <tt>s</tt> by the separator <tt>sep</tt>. Handles quotes properly. Guesses tab or comma separator if <tt>sep</tt> is not given. Also, <tt>value.escape('javascript')</tt> is useful for previewing unprintable chars prior to using smartSplit.

```
smartSplit(value,"\n") //split cell at Carriage Return or New Line char
```

### splitByCharType(s)

Returns an array of strings obtained by splitting <tt>s</tt> into groups of consecutive characters where the characters within each group share the same unicode type, and consecutive groups differ in their unicode types.

for example the string <tt>HenryCTaylor</tt> will be split as follow

- <tt>splitByCharType(value)[0]</tt> will return H
- <tt>splitByCharType(value)[1]</tt> will return enry
- <tt>splitByCharType(value)[2]</tt> will return CT
- <tt>splitByCharType(value)[3]</tt> will return aylor

### partition(string s, string or regex frag, optional boolean omitFragment)

Returns an array of strings <tt>[</tt> <tt>a</tt>, <tt>frag</tt>, <tt>b</tt> <tt>]</tt> where <tt>a</tt> is the substring within <tt>s</tt> before the first occurrence of <tt>frag</tt> in <tt>s</tt>, and <tt>b</tt> is the substring after <tt>frag</tt> in <tt>s</tt>. For example, <tt>partition("internationalization", "nation")</tt> returns 3 strings: <tt>inter</tt>, <tt>nation</tt>, and <tt>alization</tt>. If <tt>s</tt> does not contain <tt>frag</tt>, it returns an array of <tt>[</tt> <tt>s</tt>, "", "" <tt>]</tt> (the first string is the original <tt>s</tt> and the second and third strings are empty).

If <tt>omitFragment</tt> is <tt>true</tt>, <tt>frag</tt> is not returned. That is, the result is an array of only 2 elements.

Another Example using Regex with partition():

<tt>value.partition(/c.e/)[1]</tt> used on a cell with a value of "abcdefgh" will output <tt>"cde"</tt>

<tt>"abcdefgh".partition(/d..g/)[0]</tt> will output <tt>"abc"</tt>, since "abc" is the 1st part <tt>[0]</tt>, not the middle part <tt>[1]</tt> which is "defg", or the tail or last part <tt>[-1]</tt> which is "h".

More info on [Understanding-Regular-Expressions](Regex)

### rpartition(string s, string or regex frag, optional boolean omitFragment)

Returns an array of strings <tt>[</tt> <tt>a</tt>, <tt>frag</tt>, <tt>b</tt> <tt>]</tt> where <tt>a</tt> is the substring within <tt>s</tt> before the **last** occurrence of <tt>frag</tt> in <tt>s</tt>, and <tt>b</tt> is the substring after <tt>frag</tt> in <tt>s</tt>. For example, <tt>partition("parallel", "a")</tt> returns 3 strings: <tt>par</tt>, <tt>a</tt>, and <tt>llel</tt>. If <tt>s</tt> does not contain <tt>frag</tt>, it returns an array of <tt>[</tt> <tt>s</tt>, "", "" <tt>]</tt> (the first string is the original <tt>s</tt> and the second and third strings are empty).

More info on [Understanding-Regular-Expressions](Regex)

# Encoding and Hashing

### diff(o1, o2, optional string timeUnit)

For strings, returns the portion where they differ. For dates, it returns the difference in given time units.

### escape(string s, string mode)

Escapes <tt>s</tt> in the given escaping mode: <tt>html</tt>, <tt>xml</tt>, <tt>csv</tt>, <tt>url</tt>, <tt>javascript</tt>.

### unescape(string s, string mode)

Unescapes <tt>s</tt> in the given escaping mode: <tt>html</tt>, <tt>xml</tt>, <tt>csv</tt>, <tt>url</tt>, <tt>javascript</tt>.

### md5(string s)

Returns the MD5 hash of <tt>s</tt>.

### sha1(string s)

Returns the SHA-1 hash of <tt>s</tt>.

### phonetic(string s, string encoding)

Returns the a phonetic encoding of <tt>s</tt>, string encoding to use). Example:

- phonetic(value,'doublemetaphone')
- phonetic(value,'metaphone')
- phonetic(value,'metaphone3')
- phonetic(value,'soundex')
- phonetic(value,'cologne')

### reinterpret(string s, string encoder)

Returns <tt>s</tt> reinterpreted thru the given encoder. Supported encodings [http:_java.sun.com/j2se/1.5.0/docs/guide/intl/encoding.doc.html_](here).

### fingerprint(string s)

Returns the fingerprint of <tt>s</tt>, a derived string that aims to be a more canonical form of it (this is mostly useful for finding clusters of strings related to the same information).

### ngram(string s, number n)

Returns an array of the word ngrams of <tt>s</tt>.

### ngramFingerprint(string s, number n)

Returns the n-gram fingerprint of <tt>s</tt>.

### unicode(string s)

Returns an array of strings describing each character of <tt>s</tt> in their full unicode notation.

### unicodeType(string s)

Returns an array of strings describing each character of <tt>s</tt> in their full unicode notation.

# Freebase Specific

### mqlKeyQuote(string s)

Quotes <tt>s</tt> so that it can be used as a Freebase key. More info: [http:_wiki.freebase.com/wiki/MQL\_key\_escaping_](MQL+key+escaping)

### mqlKeyUnquote(string key)

Unquotes the MQL quoted string <tt>key</tt> back to its original form. More info: [http:_wiki.freebase.com/wiki/MQL\_key\_escaping_](MQL+key+escaping)

