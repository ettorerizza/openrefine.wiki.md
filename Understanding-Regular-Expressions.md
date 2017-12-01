_Understanding regular expressions_

## Introduction

A regular expression is a string that describes a text pattern occurring in other strings. Consider the text

```
Once upon a time in the land of King Arthur
```

The number of spaces between consecutive words vary. In order to change the text so that there is precisely 1 space between any 2 consecutive words, conceptually, we want to replace any continuous sequence of one or more spaces (no matter how many) with exactly one space. We can use a regular expression to describe "any continuous sequence of one or more spaces (no matter how many)".

OpenRefine and OpenRefine Expression Language (GREL) support regular expressions in the syntax of Java regular expressions: [http:_docs.oracle.com/javase/tutorial/essential/regex/_](see+Java+Regex+Tutorial).

#### Jython supported Regex

You can also use [http:_www.jython.org/docs/library/re.html_](Jython+Regex) instead of GREL functions and use a Custom Text Facet, etc with something like this for example: ```python import re g = re.search(ur"\u2014 (.\*),\s\*BWV", value) return g.group(1) ```

#### Clojure supported Regex

Clojure uses the same regex engine as Java and can be invoked with [http:_clojure.github.io/clojure/clojure.core-api.html#clojure.core/re-find_](re-find), [http:clojure.github.io/clojure/clojure.core-api.html#clojure.core/re-matches](re-matches), etc. You can use <tt>#"pattern"</tt> reader macro. See [http:_clojure.org/other\_functions#Other Useful Functions and Macros-Regex Support_](Regex+support).

To get n-th element of returned sequence, you can use <tt>nth</tt> function.

```clojure (nth (re-find #"\u2014 (.\*),\s\*BWV" value) 1) ```

#### GREL supported Regex

To write a regular expression inside a GREL expression, wrap it between a pair of forward slashes /. For example, in

```
value.replace(/\s+/, " ")
```

the regular expression is

```
\s+
```

Note for advanced users: That (wrapping with forward slashes) is similar to how you write regular expressions in Javascript. On the other hand, in Java, there is no native syntax for regular expressions, so you would have to write them as strings and escape them properly (e.g., <tt>"\\s+"</tt>). So even though the regular expression syntax in OpenRefine follows that of Java (due to implementation in Java), how you write a regular expression within a GREL expression is similar to Javascript, for convenience.

Elsewhere in OpenRefine, i.e., not within a GREL expression, do not use slashes to wrap regular expressions.

##### GREL functions that support Regex:

1. replace
2. match
3. partition
4. rpartition
5. split

## Basic Examples

To describe a pattern, you typically need concepts like something repeating again and again, or some things A and B occurring interchangeably. For example, if in a scientific paper you ever see a sequence of letters A, C, T, G, you know that's DNA being discussed. As the first try, we might formulate this regular expression to match a DNA sequence

```
[ACTG]+
```

The square brackets mean "one of the characters inside" and the + sign means "occurring one or more times". Let's use that to test whether a cell's text value contains any DNA sequence:

```
isNotNull(value.match(/[ACTG]+/))
```

That almost works, except that it also finds single letters A, T, C, G, double-letter substrings like AT, 3-letter substrings like CAT, TAG, ... and the movie name GATTACA. If we were to avoid these "false positives" (matches that are not wanted), then we have to make the regular expression stricter, by changing "occurring one or more times" to something like "occurring at least 8 or more times":

```
[ACTG]{8,}+
```

DNA sequences don't follow a very tricky pattern. Let's consider something a bit more complicated: decimal numbers. These are valid decimal numbers:

- 120.0232898
- -0.2909
- +2.343

We can describe a decimal number as follows:

- It optionally starts with a sign (minus or plus)
- Then it consists of a sequence of one or more digits
- Then optionally if it has a decimal part, it contains
  - a period, followed by
  - one or more digits

We can express that as this regular expression

```
[-+]?[0-9]+(\.[0-9]+)?
```

The question mark <tt>?</tt> denotes an optional character or group of characters. The period <tt>.</tt> needs to be escaped by preceding it with a backslash <tt>\</tt>. A digit is denoted as the range of character from 0 to 9.

We have encoded a digit as <tt>[0-9]</tt> but there is a shortcut: <tt>\d</tt>. There are several shortcuts:

- <tt>\d</tt>, same as <tt>[0-9]</tt>
- <tt>\D</tt>, same as <tt>[^0-9]</tt>, meaning any character other than a digit
- <tt>\s</tt>, meaning any whitespace character (space, tab, new line, carriage return)
- <tt>\S</tt>, meaning any character other than whitespace characters
- <tt>\w</tt>, same as <tt>[a-zA-Z_0-9]</tt>, meaning any character that can be part of a word (where the meaning of "word" is more liberal than an English word)
- <tt>\W</tt>, meaning any non-word character
- <tt>.</tt> meaning any single character

Note that any character that is used to describe patterns within regular expressions must be escaped. For example, if you want to say "a period character" rather than "any character", then you must write <tt>\.</tt> because just writing <tt>.</tt> means "any character". Similarly,

- <tt>+</tt> must be written as <tt>\+</tt>
- <tt>?</tt> must be written as <tt>\?</tt>
- <tt>[</tt> must be written as <tt>\[</tt>
- and so forth.
