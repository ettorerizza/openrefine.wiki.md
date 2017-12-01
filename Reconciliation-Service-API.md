_This is a technical description of the mechanisms behind the reconciliation system in OpenRefine. For usage instructions, see [Reconciliation](Reconciliation)._

# Introduction

A reconciliation service is a web service that, given some text which is a name or label for something, and optionally some additional details, returns a ranked list of potential entities matching the criteria. The candidate text does not have to match each entity's official name perfectly, and that's the whole point of reconciliation--to get from ambiguous text name to precisely identified entities. For instance, given the text "apple", a reconciliation service probably should return the fruit apple, the Apple Inc. company, and New York city (also known as the Big Apple).

Entities are identified by strong identifiers in some particular identifier space. In the same identifier space, identifiers follow the same syntax. For example, given the string "apple", a reconciliation service might return entities identified by the strings " [https:_www.wikidata.org/wiki/Q89_](Q89)", " [https:www.wikidata.org/wiki/Q312](Q312)", and " [https:_www.wikidata.org/wiki/Q60_](Q60)", in the Wikidata ID space. Each reconciliation service can only reconcile to one single identifier space, but several reconciliation services can reconcile to the same identifier space.

OpenRefine defines a reconciliation API so that users can use the reconciliation features of OpenRefine with various databases.

Informally, the main function of any reconciliation service is to find good candidates in the underlying database, given the following data:

- A string, which is normally the name or title of the entity, in some language.
- Optionally, a type which can be used to narrow down the search to entities of this type. OpenRefine does not define a particular set of acceptable types: this choice is left to the reconciliation service (see the suggest API for that).
- Optionally, a list of properties and their values, which can be used to refine the search. For instance, when reconciling a database of books, the author name or the publication date are useful bits of information that can be transferred to the reconciliation service. This information will be sent to the reconciliation service if the user binds columns to properties. Again, the notion of property is not predefined in OpenRefine: its definition depends on the reconciliation service.

A standard reconciliation service is a HTTP-based RESTful JSON-formatted API. It consists of various endpoints, each of which fulfills a specific function. Only the first one is mandatory.

- The root URL. This is the URL that users will need to add the service to OpenRefine. For instance, the Wikidata reconciliation interface in English has the following URL: [https://tools.wmflabs.org/openrefine-wikidata/en/api](https://tools.wmflabs.org/openrefine-wikidata/en/api)
- _Optional._ The suggest API, which enables autocompletion at various places in OpenRefine;
- _Optional._ The preview API, which lets users preview the reconciled items directly from OpenRefine;
- _Optional._ The data extension API, which lets users add columns from reconciled values based on the properties of the items in the reconciliation service.

The specification of each of these endpoints is given in the following sections.

## Workflow overview

OpenRefine communicates with reconciliation services in the following way.

- The user adds the service by inputting its endpoint in the dialog. OpenRefine queries this URL to retrieve its [Reconciliation Service API#service-metadata](service+metadata) which contains basic metadata about the service (such as its name and the available features).
- The user selects a service to configure reconciliation using this service. OpenRefine queries the reconciliation service in [Reconciliation Service API#multiple-query-mode](batch+mode) on the first ten items of the column to be reconciled. The reconciliation results are not presented to the user, but the types of the candidate items are aggregated and proposed to the user to restrict the matching to one of them. For example, if your service reconciles both people and companies, and the query for the first 10 names returns 15 candidates which are people and 7 candidates which are companies, the types will be presented to the user in that order. They can override the order and pick whichever type they want, as well as chosen a different type by hand or choose to reconcile without any type information.
- The user configures the reconciliation. If a [Reconciliation Service API#suggest-apis](suggest+service) is available, it will be used to provide auto-completion in the dialog, to choose types or properties.
- When reconciliation starts, OpenRefine queries the service in [Reconciliation Service API#multiple-query-mode](batch+mode) for small batches of rows and stores the responses of the service.
- Once reconciliation is complete, the results are displayed. The user makes reconciliation decisions based on the choices provided. If a [Reconciliation Service API#suggest-apis](suggest+service) is available, it will be used to input custom reconciliation decisions. If a [Reconciliation Service API#preview-api](preview+service) is available, the user will be able to preview the reconciliation candidates without leaving OpenRefine.

# Main reconciliation service

The root URL has three functions:

- it returns the [Reconciliation Service API#service-metadata](service+metadata) if no query is provided.
- when given the <tt>queries</tt> parameter, it performs a [Reconciliation Service API#multiple-query-mode](batch+of+queries) and returns the results for each query. This makes it efficient
- it performs a single query if the <tt>query</tt> parameter is given. This mode is deprecated: it is not used by OpenRefine and other API consumers should not rely on it.

## Service metadata

When a service is called with just a JSONP <tt>callback</tt> parameter and no other parameters, it must return its _service metadata_ as a JSON object literal with the following fields:

- <tt>"name"</tt>: the name of the service, which will be used to display the service in the reconciliation menu;
- <tt>"identifierSpace"</tt>: an URI for the type of identifiers returned by the service;
- <tt>"schemaSpace"</tt>: an URI for the type of types understood by the service.
- <tt>"view"</tt> an object with a template URL to view a given item from its identifier: <tt>"view": {"url":"http://example.com/object/{{id}}"} </tt> 

The last two parameters are mainly useful to assert that the identifiers returned by two different reconciliation services mean the same thing. Other fields are optional: they are used to specify the URLs for the other endpoints (suggest, preview and extend) described in the next sections.

Here are two live examples:

1. [https://tools.wmflabs.org/openrefine-wikidata/en/api](https://tools.wmflabs.org/openrefine-wikidata/en/api)
2. [http://refine.codefork.com/reconcile/viaf](http://refine.codefork.com/reconcile/viaf)
```
{
  "name" : "Wikidata Reconciliation for OpenRefine (en)",
  "identifierSpace" : "http://www.wikidata.org/entity/",
  "schemaSpace" : "http://www.wikidata.org/prop/direct/",
  "view" : {
    "url" : "https://www.wikidata.org/wiki/{{id}}"
  },
  "defaultTypes" : [],
  "preview" : {
    ...
  },
  "suggest" : {
    ...
  },
  "extend" : {
    ...
  },
}
```

## Query Request

### Single Query Mode

A call to a reconciliation service API for a single query looks like either of these:

```
http://foo.com/bar/reconcile?query=...string...
  http://foo.com/bar/reconcile?query={...json object literal...}
```

If the query parameter is a string, then it's an abbreviation of <tt>query={"query":...string...}</tt>. Here are two live examples:

1. [https://tools.wmflabs.org/openrefine-wikidata/en/api?query=boston](https://tools.wmflabs.org/openrefine-wikidata/en/api?query=boston)
2. [https://tools.wmflabs.org/openrefine-wikidata/en/api?query={%22query%22:%22boston%22,%22type%22:%22Q515%22}](https://tools.wmflabs.org/openrefine-wikidata/en/api?query={%22query%22:%22boston%22,%22type%22:%22Q515%22})

The query json object literal has a few fields

| Parameter | Description |
| --- | --- |
| "query" | A string to search for. Required. |
| "limit" | An integer to specify how many results to return. Optional. |
| "type" | A single string, or an array of strings, specifying the types of result e.g., person, product, ... The actual format of each type depends on the service (e.g., "Q515" as a Wikidata type). Optional. |
| "type\_strict" | A string, one of "any", "all", "should". Optional. |
| "properties" | Array of json object literals. Optional |

Each json object literal of the "properties" array is of this form

```
{
    "p" : string, property name, e.g., "country", or
    "pid" : string, property ID, e.g., "P17" as a Wikidata property ID
    "v" : a single, or an array of, string or number or object literal, e.g., "Japan"
  }
```

A "v" object literal would have a single key "id" whose value is an identifier resolved previously to the same identity space.

Here is an example of a full query parameter:

```
{
    "query" : "Ford Taurus",
    "limit" : 3,
    "type" : "/automotive/model",
    "type_strict" : "any",
    "properties" : [
      { "p" : "year", "v" : 2009 },
      { "pid" : "/automotive/model/make" , "v" : { "id" : "/en/ford" } }
    ]
  }
```

### Multiple Query Mode

A call to a standard reconciliation service API for multiple queries looks like this:

```
http://foo.com/bar/reconcile?queries={...json object literal...}
```

The json object literal has zero or more key/value pairs with arbitrary keys where the value is in the same format as a single query, e.g.

```
http://foo.com/bar/reconcile?queries={ "q0" : { "query" : "foo" }, "q1" : { "query" : "bar" } }
```

"q0" and "q1" can be arbitrary strings. They will be used to key the results returned. Here is a live example:

[http://standard-reconcile.freebaseapps.com/reconcile?queries={%22q0%22:{%22query%22:%22boston%22,%22type%22:%22/music/musical\_group%22},%22q1%22:{%22query%22:%22jaguar%22}}](http://standard-reconcile.freebaseapps.com/reconcile?queries={%22q0%22:{%22query%22:%22boston%22,%22type%22:%22/music/musical_group%22},%22q1%22:{%22query%22:%22jaguar%22}})

## Query Response

The response for a single query is a JSON literal object

```
{
    "result" : [
      {
        "id" : ... string, database ID ...
        "name" : ... string ...
        "type" : ... array of strings ...
        "score" : ... double ...
        "match" : ... boolean, true if the service is quite confident about the match ...
      },
      ... more results ...
    ],
    ... potentially some useful envelope data, such as timing stats ...
  }
```

For multiple queries, the response is a JSON literal object with the same keys as in the request

```
{
    "q0" : {
      "result" : { ... }
    },
    "q1" : {
      "result" : { ... }
    }
  }
```

The service must also support JSONP through a callback parameter ie &callback=foo.

# Preview API

The preview service API (complementary to the reconciliation service API) is quite simple. Pass it an identifier and it renders information about the corresponding entity in an HTML page, which will be shown in an iframe inside OpenRefine. The given width and height dimensions tell OpenRefine how to size that iframe.

If there is no preview service specified in the reconciliation service's metadata, and if the entity identifier space is a Freebase ID or Mid identifier space, then OpenRefine knows to use Freebase's topicblock service. Here is an example of how an entity's preview looks like

[http://www.freebase.com/widget/topic/en/apple\_inc?mode=content](http://www.freebase.com/widget/topic/en/apple_inc?mode=content)

NOTE: The Freebase Topic Blocks widget is deprecated and will go away when the old API is turned off (soon!). New code should use the Topic API in this fashion:

[https://www.googleapis.com/freebase/v1/topic/en/apple\_inc?filter=suggest&key=${key}](https://www.googleapis.com/freebase/v1/topic/en/apple_inc?filter=suggest&key=${key})

# Suggest APIs

In the "Start Reconciling" dialog box in OpenRefine, you can specify which type of entities the column in question contains. For instance, the column might contains names of politicians. But you don't know the identifier corresponding to the "politician" type. So we need a suggest API that translates "politician" to something like, say, "/government/politician" if we're reconciling against Freebase.

In the same dialog box, you can specify that other columns should be used to provide more details for the reconciliation. For instance, if there is a column specifying the politicians' home city, passing that data onto the reconciliation service might make reconciliation more accurate. You might want to specify how that second column is related to the column being reconciled, but you might not now how to specify "home city" as a precise relationship. So we need a suggest API that translates "home city" to something like "/people/person/places\_lived".

There is also a need for a suggest service for entities rather than just for types and properties. When a cell has no good candidate, then you would want to perform a search yourself (by clicking on "search for match" in that cell).

These suggest APIs are required to work with a modified version of the [http:_code.google.com/p/freebase-suggest/_](Freebase+Suggest+widget). As illustrated on that site, each suggest API has 2 jobs to do:

- translate what the user type into a ranked list of entities (and this is similar to the core reconciliation service and might share the same implementation)
- render a flyout when an entity is moused over or highlighted using arrow keys (and this is similar to the preview API and might share the same implementation)

The metadata for each suggest API (type, property, or entity) is as follows:

```
{
  "service_url" : "... url including only the domain ...",
  "service_path" : "... optional relative path ...",
  "flyout_service_url" : "... optional url including only the domain ...",
  "flyout_service_path" : "... optional relative path ..."
}
```

The <tt>service_url</tt> field is required and it should look like this: <tt>http://foo.com</tt>. There should be no trailing / at the end. The other fields are optional and have defaults if not provided:

- <tt>service_path</tt> defaults to <tt>/private/suggest</tt>
- <tt>flyout_service_url</tt> defaults to the provided <tt>service_url</tt> field
- <tt>flyout_service_path</tt> defaults to <tt>/private/flyout</tt>

The Freebase Suggest widgets embedded in OpenRefine will concatenate <tt>_url</tt> and <tt>_path</tt> to make the full URL. Refer to [Suggest Api](the+specification+for+a+Suggest+API) for details.

# Data Extension

From OpenRefine 2.8 it is possible to fetch values from reconcilied sources natively. This is only possible for the reconciliation endpoints that support this additional feature, described at [Data Extension API](Data+Extension+API).

# OpenRefine Integration & Testing

OpenRefine includes a mechanism for adding reconciliation service API URLs. After choosing Reconcile->Start Reconciling for a column, look at the bottom of the page for two buttons : "Add Standard Service..." and "Add Namespaced Service..."

Each column in OpenRefine should only be reconciled against one identity space.

There is a testing dashboard available as part of the standard reconciliation service at:

[http://standard-reconcile.freebaseapps.com/](http://standard-reconcile.freebaseapps.com/)

It will allow you to see the format of various example queries as well as the responses that the return for both the standard reconciliation service and a number of example reconciliation services.

To test and debug a new reconciliation service ... TODO

# Examples

We've cloned a number of the Refine reconciliation services as a way of providing them visibility. They can be found at [https://github.com/OpenRefine](https://github.com/OpenRefine)

Some of them include:

- [https://github.com/dergachev/redmine-reconcile](https://github.com/dergachev/redmine-reconcile) - Python & Flask implementation that just returns the given name/number with a base url prepended
- [https://github.com/okfn/helmut](https://github.com/okfn/helmut) - A generic Refine reconciliation API implementation using Python & Flask
- [https://github.com/mblwhoi/reconciliation\_service\_skeleton](https://github.com/mblwhoi/reconciliation_service_skeleton) - Skeleton for Standalone Python & Flask Reconciliation Service for Refine
- [https://github.com/mikejs/reconcile-demo](https://github.com/mikejs/reconcile-demo) 
- [https://github.com/rdmpage/phyloinformatics/tree/master/services](https://github.com/rdmpage/phyloinformatics/tree/master/services) - PHP examples (reconciliation\_\*.php)
- [https://github.com/granoproject/grano-reconcile](https://github.com/granoproject/grano-reconcile) - python example
- [https://github.com/codeforkjeff/refine\_viaf](https://github.com/codeforkjeff/refine_viaf) - reconciliation service that queries [http:_viaf.org_](VIAF), a name authority administered by [http:oclc.org](OCLC). 

The open-reconcile project provides a complete Java based reconciliation service which queries a SQL database. [https://code.google.com/p/open-reconcile](https://code.google.com/p/open-reconcile)

The [http:_refine.deri.ie_](RDF+extension) incorporates, among other things, reconciliation support with different approaches:

- a service to reconciliate against querying a SPARQL endpoint
- reconcile against a provided RDF file
- based on Apache Stanbol ( [https:_github.com/fadmaa/grefine-rdf-extension/pull/59_](implementation+details))

[https:_github.com/sunlightlabs_](Sunlight+Labs) implemented a reconciliation service using Piston on Django for their [http:influenceexplorer.com/](Influence+Explorer) [https://github.com/sunlightlabs/datacommons/blob/master/dcapi/reconcile/handlers.py](https://github.com/sunlightlabs/datacommons/blob/master/dcapi/reconcile/handlers.py)

Also look at the [Reconcilable Data Sources](Reconcilable+Data+Sources) page for other examples of available reconciliation services that are compatible with Refine. Not all of them are open source, but they might spark some ideas.

* * *

(Much of this standard is based on the old [http:_api.freebase.com/api/service/search?help_](Freebase+relevance+service) and the [http:data.labs.freebase.com/recon/](Freebase+experimental+recon+service).)

