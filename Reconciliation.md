_This page is also available in [http:](Japanese)_qiita.com/yayamamo/items/eade3e5788e6f359bce7.

Reconciliation is a semi-automated process of matching text names to database IDs (keys). This is semi-automated because in some cases, machine alone is not sufficient and human judgement is essential. For example, given "Ocean's Eleven" as the name of a film, should it be matched to

- the original 1960 "Ocean's Eleven" ( [https:_www.wikidata.org/wiki/Q464933_](Wikidata+item+Q464933)), or
- the 2001 remake "Ocean's Eleven" starring George Clooney ( [https:_www.wikidata.org/wiki/Q205447_](Wikidata+item+Q205447))?

While we talk about "databases" here, perhaps a more accurate term would be "name registries". And conceptually, "reconciliation" isn't a new concept. In some fields, it's being done all the time. For example, when the police gets a name of a suspect, they run the name through their databases of known felons. More details about the suspect (e.g., race, height, last known address) would help the police narrow down the suspect among several database matches.

You can use OpenRefine to perform reconciliation of names in your data against any database that exposes a web service following this [Reconciliation Service Api](Reconciliation+Service+API) specification. One such database is [https:_www.wikidata.org/_](Wikidata), and in this document we will use it as our example service.

## Basics

Given a column containing names, we can reconcile those names against Wikidata. To do so, invoke the column's drop-down menu and pick _Reconcile_ > _Start reconciling_. If you want to reconcile only some cells in that column, then use filters and facets to isolate them first.

![Screenshot of the reconciliation menu](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_column.png)

In the Reconcile dialog box, pick _Wikidata Reconciliation Service_ on the left. That service comes by default, but you can install more services (see below to get a version of this service in your language).

There are a few options to configure the reconciliation process, which will help you get more reliable matches. These options are covered in the next sections. To start the reconciliation process, click the _Start Reconciling_ button.

![Screenshot of the reconciliation dialog](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_start.png)

This process will take a while depending on how much data you have. Currently, the Wikidata reconciliation service processes about 3 rows per second.

When the process is done, you will see that the reconciliation data in the cells.

- Either the cell was successfully matched: it displays a single dark blue link. In this case, the reconciliation is confident that the match is correct: you should not have to check it manually.

![(Screenshot of a matched item)](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_match.png)

- Or a few candidates are displayed, together with their reconciliation score, with light blue links. You need to pick manually the correct one. For each matching decision you make, you have two options:&nbsp;either match this cell only ( ![button to perform a single match](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_single_match.png)), or also use the same identifier for all other cells containing the same unreconciled value ( ![button to perform a multiple match](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_bulk_match.png)).

![(Screenshot of an unmatched item)](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_disambig.png)

In addition to changing the ways the reconciled cells are displayed, OpenRefine also automatically creates two facets for you to use to filter the cells based on the reconciliation data. One is a numeric facet for the reconciliation scores of the best candidates of the cells. Higher scores mean better matches. You could filter for the higher scores, and approve them all in bulk (invoking that column's drop-down menu and using the _Reconcile_ > _Actions_ submenu).

There is also the _judgment_ facet, which lets you filter for the cells that haven't been matched (pick "None" in the facet). As you process each cell, its judgment changes from "None" to "Matched" and it disappears from the view, because it no longer fits the facet's selection.

## Restricting matches by type

One simple way to improve the quality of the matches is to restrict them to a given type. If your dataset contains universities, then you know that "Heidelberg" does not refer to the [https:_www.wikidata.org/wiki/_](city+of+Heidelberg+%28Q2966%29) but rather the [https:www.wikidata.org/wiki/Q151510](university+of+Heidelberg+%28Q151510%29).

In Wikidata, types are items themselves. For instance, the [https:_www.wikidata.org/wiki/Q1377_](university+of+Ljubljana+%28Q1377%29) has type [https:www.wikidata.org/wiki/Q875538](public+university+%28Q875538%29), which is witnessed by a statement on [https:_www.wikidata.org/wiki/Q1377_](Q1377) using the [https:www.wikidata.org/wiki/Property:P31](instance+of+%28P31%29) property.

Types are organized into an ontology, by specifying which types are subclasses of the others, using the [https:_www.wikidata.org/wiki/Property:P279_](subclass+of+%28P279%29) property. For instance, [https:www.wikidata.org/wiki/Q875538](public+university+%28Q875538%29) is a subclass of [https:_www.wikidata.org/wiki/Q3918_](university+%28Q3918%29).

![(diagram explaining types in Wikidata)](https://tools.wmflabs.org/openrefine-wikidata/static/explanation_types_wikidata.svg)

One useful tool to visualize the subclasses of a given type is the [https:_angryloki.github.io/wikidata-graph-builder/_](Wikidata+Graph+Builder). For instance, here are [https:angryloki.github.io/wikidata-graph-builder/?property=P279&item=Q4671277&mode=reverse](all+the+subclasses+of+academic+institution+%28Q4671277%29).

The reconciliation dialog allows you to select a type for your rows. This will restrict the matches to items which are instances of any subclass of the given type. For instance, if you select [https:_www.wikidata.org/wiki/Q875538_](public+institution+%28Q875538%29), then [https:www.wikidata.org/wiki/Q1377](university+of+Ljubljana+%28Q1377%29) will be a possible match, because of the path depicted above.

By default, OpenRefine will propose you some types based on the first few items of your dataset. It is quite likely that these types will be too specific for your needs. You can specify a broader type that will encompass more items. For instance, if you have a dataset of universities, it might be better to pick [https:_www.wikidata.org/wiki/Q2385804_](educational+institution+%28Q2385804%29) rather than [https:www.wikidata.org/wiki/Q3918](university+%28Q3918%29), for instance if some entry happens to be marked as a [https:_www.wikidata.org/wiki/Q189004_](college+%28Q189004%29) in Wikidata.

Some items are not marked as the instances of anything, because no one has taken the time to add these statements yet. If you restrict the reconciliation to a type, these items will not appear in the results, except when no items of the given type could be found for the particular name. In this case, items with no type can be returned: they will have a low score and will not be matched automatically.

## Refining by property

Names are ambiguous, and sometimes your dataset contains other columns that can help identify which item is being referred to. The reconciliation interface can be configured to take these columns into account in the matching scores. For instance, say you have a dataset of sportspeople with a column indicating which sport they play.

![(example dataset)](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_name_sport.png)

If you want to use the second column in the reconciliation process, you need to identify how this additional information is represented in Wikidata. So you need to find out which Wikidata property is used to link sportspeople to the sport they play. Looking at [https:_www.wikidata.org/wiki/Q1189_](Usain+Bolt+%28Q1189%29) it seems that [https:www.wikidata.org/wiki/Property:P641](sport+%28P641%29) can be used for that.

In the reconciliation dialog, tick the box to include your column and bind it to the relevant Wikidata property.

![(dialog to bind a column to a property)](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_name_sport_dialog.png)

The reconciliation service will then fuzzy-match on both columns.

![(example dataset)](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_name_sport_disambiguated.png)

As you can see, the score is boosted when the sport matches. In the first row, there is a slight mismatch between the sport in the table ("rugby") and on Wikidata ("rugby league"), which explains why the first match does not reach the perfect score (100) and is therefore not matched automatically.

## Advanced usage: property paths

**This feature is specific to the reconciliation interface for Wikidata.**

Sometimes, the relation between the reconciled item and the disambiguating column is not direct: it is not represented as a property itself. Let us consider this dataset of cities:

![(an example dataset with cities and country codes)](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_city_country.png)

To fetch the country code from an item representing a city, you need to follow two properties. First, follow [https:_www.wikidata.org/wiki/Property:P17_](country+%28P17%29) to get to the item for the country in which this city is located, then follow [https:www.wikidata.org/wiki/Property:P297](ISO+3166-1+alpha-2+code+%28P297%29) to get the two-letter code string.

![(an example dataset with cities and country codes)](https://tools.wmflabs.org/openrefine-wikidata/static/sparql_path_1.svg)

This is supported by the reconciliation interface, with a syntax inspired by [https:_www.w3.org/TR/sparql11-property-paths/_](SPARQL+property+paths): just type the sequence of property identifiers separated by slashes, such as <tt>P17/P297</tt>:

![(inputting a property path in the reconciliation interface)](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_city_country_dialog.png)

This additional information allows to distinguish between namesakes, to some extent. As "Cambridge, US" is still ambiguous, there are multiple items with a perfect matching score, but "Oxford, GB" successfully disambiguates one particular city from its namesakes. ![(the disambiguated dataset)](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_city_country_disambiguated.png)

The endpoint currently supports two property combinators: <tt>/</tt>, to concatenate two paths as above, and <tt>|</tt>, to compute the union of the values yielded by two paths. Concatenation <tt>/</tt> has precedence over disjunction <tt>|</tt>. The dot character <tt>.</tt> can be used to denote the empty path. For instance, the following property paths are equivalent:

- <tt>P17|P749/P17</tt>
- <tt>P17|(P749/P17)</tt>
- <tt>(.|P749)/P17</tt>

They fetch the [https:_www.wikidata.org/wiki/Property:P17_](country+%28P17%29) of an item or that of its [https:www.wikidata.org/wiki/Property:P17](parent+organization+%28P749%29).

### Comparing values

By default, the values supplied in OpenRefine and the ones present in Wikidata are compared by string fuzzy-matching. There are some exceptions to this:

- If the value is an identifier, then exact string matching is used.
- If the values are integers, exact equality between integers is used.
- If the values are floating point numbers, the score is 100 if they are equal and decreases towards 0 as their absolute difference increases.
- If the values are coordinates (specified in the <tt>"lat,lng"</tt> format on OpenRefine's side), then the matching score is 100 when they are equal and decreases as their distance increases. Currently a score of 0 is reached when the points are 1km away from each other.

Sometimes, we need a more specific matching on sub-parts of these values. It is possible to select these parts for matching by appending a modifier at the end of the property path:

- <tt>@lat</tt> and <tt>@lng</tt>: latitude and longitude of geographical coordinates (float)
- <tt>@year</tt>, <tt>@month</tt>, <tt>@day</tt>, <tt>@hour</tt>, <tt>@minute</tt> and <tt>@second</tt>: parts of a time value (int). They are returned only if the precision of the Wikidata value is good enough to define them.
- <tt>@isodate</tt>: returns a date in the ISO format <tt>1987-08-23</tt> (string). A value is always returned.
- <tt>@iso</tt>: returns the date and time in the ISO format <tt>1996-03-17T04:15:00+00:00</tt>. A value is always returned. For times and dates, all values are returned in the UTC time zone.
- <tt>@urlscheme</tt> (<tt>"https"</tt>), <tt>@netloc</tt> (<tt>"www.wikidata.org"</tt>) and <tt>@urlpath</tt> (<tt>"/wiki/Q42"</tt>) can be used to perform exact matching on parts of URLs.

For instance, if you want to refine people by their birth dates, but you only have the month and day. First, split the birthday dates in two columns, for month and day.

![(the initial dataset)](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_birthdates.png)

Reconcile using the following parameters (<tt>P569</tt> can be found with the autocompletion first):

![(the reconciliation parameters: P569@month and P569@day)](https://tools.wmflabs.org/openrefine-wikidata/static/screenshot_birthdates_recon.png)

## Reconciling via unique identifiers

Many Wikidata items come with external unique identifiers, so if you have some in your dataset they can be very useful to get a precise reconciliation. When a Wikidata property corresponding to a unique identifier is used, the reconciliation interface uses a different matching strategy. It first retrieves any items with this identifier and gives it the perfect matching score (100). All the other properties are ignored, including the original name in the column being reconciled. If no item bears the supplied unique identifier, the reconciliation interface falls back on the standard strategy, using string search and fuzzy matching.

**Note:** Sometimes the same external identifier is assigned to different Wikidata items (in which case the conflict will be flagged in Wikidata as a uniqueness constraint violation). In this case, the multiple items are returned but are not automatically matched.

**Note:** It is important to supply the unique identifiers in the same format as they are stored on Wikidata. Matching of identifiers is done with strict string equality. Case differences, spaces or leading zeros all matter. Check the documentation of the relevant Wikidata property for the expected format.

This alternative matching strategy is only used for unique identifiers belonging to the reconciled items. For instance, while the [https:_www.wikidata.org/wiki/Property:P297_](ISO+3166-1+alpha-2+code+%28P297%29) is a unique identifier for countries, it will not be used as a unique identifier for cities reconciled with the SPARQL property path <tt>P17/P297</tt> demonstrated above.

However, disjunctions of unique identifiers can be still used: for instance, suppose you have a column containing sometimes a [https:_www.wikidata.org/wiki/Property:P356_](DOI+%28P356%29), and sometimes a [https:www.wikidata.org/wiki/Property:P3784](CiteSeerX+ID+%28P3784%29). If you are confident that such ids are not ambiguous (which is the case in this example as the formats are incompatible), then you can reconcile with this column mapped to <tt>(P356|P3784)</tt>. For each row, any item with the given value as either a DOI or a CiteSeerX id will be matched, ignoring the other values.

## Fetching values from the reconciled database

One reason to reconcile a dataset to some database is that it allows you to pull data from the database into OpenRefine. There are essentially two ways to do this.

- If the reconciliation service supports the [Data Extension API](Data+Extension+API), then you can use the "Add columns from reconciled values" action to augment your OpenRefine project with new columns: ![Screencast of the data extension workflow](https://camo.githubusercontent.com/50b7b519f055af6a8d81951f7a7f652c49e704e6/687474703a2f2f70696e746f63682e756c6d696e666f2e66722f393264636464323066332f7265636f726465642e676966)
- If the reconciliation service does not support this feature, look out for a generic web API for that data source. You can use the "Add column by fetching URLs" operation to call this API with the ids obtained from the reconciliation process. For instance, if you have used the OpenCorporates service, you can use [https:_api.opencorporates.com/documentation/API-Reference_](their+API) to fetch JSON-formatted data about the reconciled companies. The GREL expression <tt>"https://api.opencorporates.com/companies/"+cell.recon.match.id</tt> will be translated to URLs like [https://api.opencorporates.com/companies/17087985](https://api.opencorporates.com/companies/17087985), whose content can then be parsed in OpenRefine.

## Keep all the suggestions made

To keep all suggestion made by the reconciliation service and not match against the best one:

1. Before starting reconciliation un-check option Auto-match candidates with high confidence in Reconcile dialog (it's at the bottom). 
  1. After reconciliation you should be able to see all three suggested reconciliation candidates. This step is optional - it is just for you to see what will be extracted.

2. After reconciliation click on the reconciled column and choose Add column based on this column.
  1. Put following expression in the Expression field if you want the value: <tt>cell.recon.candidates[0].name</tt>
  2. if you want a link to Wikidata use this expression: <tt>cell.recon.candidates[0].id</tt>
  3. To get all of the IDs at the same time use the expression: <tt>cell.recon.candidates.join(',')</tt> , which will give you a comma separated list of all the IDs. Then use the command "Split multi-valued cells" to split the identifiers into individual rows.

## Language settings for Wikidata reconciliation

You can install a version of the Wikidata reconciliation service that uses your language.

First, you need to find your language code: this is the first letters in the domain name of your Wikipedia (for instance "fr" if your Wikipedia is [https://fr.wikipedia.org/wiki/](https://fr.wikipedia.org/wiki/)).

Then, open the reconciliation dialog and click _Add Standard Service_. The URL is <tt>https://tools.wmflabs.org/openrefine-wikidata/fr/api</tt> where "fr" is replaced by your language code.

When reconciling using this interface, items and properties will be displayed in your language if a translation is available. The matching score of the reconciliation is not influenced by your choice of language: items are matched by considering all labels and keeping the best possible match. So the language of your dataset is irrelevant to the choice of the language for the reconciliation interface.

## Additional Resources

See also:

- Recon section in the [Variables](Variables) page. 
- The different Data Sources available for [Reconcilable-Data-Sources](Reconcilable-Data-Sources)
