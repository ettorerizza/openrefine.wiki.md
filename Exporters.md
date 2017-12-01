_Exporting data and projects_

## Basics

You can export data from an existing OpenRefine project in several formats:

- Tab separated values (TSV)
- Comma separated values (CSV)
- Excel
- HTML table

To export from a project, click the Export button at the top right corner and pick the format you want. Note that if you have selections within some facets or filters, then only the rows matching the facets and filters' constraints will be exported.

## Templating Exporter

If you pick <tt>Templating...</tt> from the Export menu, you can "roll your own" exporter. This is useful for formats that we don't support natively yet, or won't support. For example, right now, you can use the Templating exporter to generate JSON by default. We have a tutorial on using this approach [Export As YAML](export+your+data+as+YAML).

### Guides

These should be some good guides to getting started with exporting your data in different formats. You should look through the exporting to YAML

| Format | Prefix | Row Template | Row Separator | Suffix |
| --- | --- | --- | --- | --- |
| **MediaWiki tables** | <tt>{|</tt> | <tt>| {{cells["col1"]}}</tt> | <tt>|-</tt> | <tt>|}</tt> |
| **YAML** | <tt>----</tt> | <tt>- {{cells["col1"]}}</tt> | | <tt>...</tt> |

## Exporting Projects

You can also export a whole OpenRefine project as a .tar.gz file and then someone else can import it into their OpenRefine. The exported file will contain not only the data but also the history of changes. This way, the person who receives your project file knows exactly what operations you have performed, and can even undo them.

