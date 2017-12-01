_A demonstration on how to use export templating to produce YAML._

# Introduction

For this tutorial, we are going to be working with a small set of data that looks like this:

| **Name** | **Height (m)** | **Occupation** |
| --- | --- | --- |
| Andy | 1.73 | Student |
| Bernd | 1.79 | Zoologist |
| Carol | 1.65 | Carpenter |

By default, OpenRefine exports JSON. JSON is a very effective way to transport data, but it can sometimes be difficult to read. Consider the difference between these two presentations of the same data:

## JSON
```
{
  "rows" : [
    {
      "Name" : "Andy",
      "Height (m)" : 1.73,
      "Occupation" : "Student"
    },
    {
      "Name" : "Bernd",
      "Height (m)" : 1.79,
      "Occupation" : "Zoologist"
    },
    {
      "Name" : "Carol",
      "Height (m)" : 1.65,
      "Occupation" : "Carpenter"
    }
  ]
}
```

## YAML
```
rows:

- Name: Andy
  Height (m): 1.73
  Occupation: Student

- Name: Bernd
  Height (m): 1.79
  Occupation: Zoologist

- Name: Carol
  Height (m): 1.65
  Occupation: Carpenter
```

You can see from these two examples that they are both fairly readable. However, YAML has no commas or curly braces that interfere with readability.

# Getting Started

You can follow along by pointing OpenRefine at the example spreadsheet:

- Go to [http://localhost:3333/](http://localhost:3333/) in your web browser
- Begin a new project with <tt>https://spreadsheets.google.com/pub?key=0Aq_3OYelM4ZUdEpYYmNKY0t1V0VjYlR5UjFYejVHbWc&amp;hl=en_GB&amp;single=true&amp;gid=0&amp;range=A1%3AC4&amp;output=csv</tt> as the URL. 
- Click on <tt>Export</tt> > <tt>Templating...</tt>

# Details

YAML is a very simple format. Its entire purpose is to be human readable. Also, it is focused on data, rather than documents. This makes it a perfect fit for sharing data from OpenRefine.

## Changes to make

### Prefix Field

**Basic** We only need a single word in the prefix field:

```
rows:
```

**Bonus points**

If we want to be nice to YAML parsers, we can add some optional dashes. These help to tell the computer that's reading it that it has entered a YAML document. The pound sign or octothorpe (#) is the comment character.

```
--- #people

rows:
```

### Row Field

Generating rows is much simpler than the current JSON. The main thing to watch out for is that there is a blank line between rows.

Try changing:

```
{
      "Person" : {{jsonize(cells["Person"].value)}},
      "Height (m)" : {{jsonize(cells["Height (m)"].value)}},
      "Occupation" : {{jsonize(cells["Occupation"].value)}}
    }
```

To:

```
- Name: {{cells["Person"].value}}
   Height (m): {{cells["Height (m)"].value}}
   Occupation: {{cells["Occupation"].value}}
```

Some points here:

- Keep the curly braces in place. That tells OpenRefine that you need to do some processing there.
- Remove the trailing commas from each part
- Make sure the cell headings match exactly. If you see "null" in the preview window instead of the correct value, you probably have the column name wrong.

### Row Separator

Change the row separator from the default comma (,) to a blank line. Confirm that there's a blank line between each record in the preview window.

### Suffix Field

Adding three dots is good practice. It tells the parser that we've finished.

```
...
```
