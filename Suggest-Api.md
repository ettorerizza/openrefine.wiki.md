_Specification for a Suggest API that's compatible with the Freebase Suggest widget_

A Suggest API that's compatible with the Freebase Suggest widget should have 2 entry points:

- <tt>suggest</tt>: translates some text that the user has typed, optionally constrained in some ways, to a ranked list of entities (this is very similar to a [Reconciliation Service Api](reconciliation+service+API) and can share the same implementation)
- <tt>flyout</tt> : renders a small view of an entity

See [http://code.google.com/p/freebase-suggest/](http://code.google.com/p/freebase-suggest/) for an illustration of these two entry points. The dropdown list on the left is populated by the data returned from the <tt>suggest</tt> entry point, and the block on the right is rendered from the HTML returned by a call to the <tt>flyout</tt> entry point given the identifier corresponding to Francis Ford Coppola.

For the <tt>suggest</tt> entry point, it is important to balance speed versus accuracy. The widget must respond in interactive time, meaning about 200 msec, for each change to the user's text input. At the same time, the ranked list of entities must seem quite relevant to what the user types.

Similarly, for the <tt>flyout</tt> entry point, it is important to respond quickly while providing enough essential details so that the user can visually check if the highlighted entity is the desired one. You probably would want to embed a thumbnail image, as we have found that images are excellent for visual identification.

# suggest Entry Point

The <tt>suggest</tt> entry point takes the following URL parameters

| "prefix" | a string the user has typed |
| "type" | optional, a single string, or an array of strings, specifying the types of result e.g., person, product, ... The actual format of each type depends on the service (e.g., "/government/policician" as a Freebase type) |
| "type\_strict" | optional, a string, one of "any", "all", "should" |
| "limit" | optional, an integer to specify how many results to return |
| "start" | optional, an integer to specify the first result to return (thus in conjunction with <tt>limit</tt>, support pagination) |

Here is a live example

[http://www.freebase.com/private/suggest?prefix=war&type=/book/book](http://www.freebase.com/private/suggest?prefix=war&type=/book/book)

The response is in JSON, and JSONP must be supported.

```
{
    "code" : "/api/status/ok",
    "status" : "200 OK",
    "prefix" : ... string, the prefix URL parameter echoed back ...
    "result" : [
      {
        "id" : ... string, identifier of entity ...
        "name" : ... string, nameof entity ...
        "n:type" : {
          "id" : ... string, identifier of type ...
          "name" : ... string, name of type ...
        }
      },
      ... more results ...
    ],
    ... potentially some useful envelope data, such as timing stats ...
  }
```

<tt>status</tt> should correspond to the HTTP response code. <tt>code</tt> is the error/success state of the API above the level of HTTP. Use "/api/status/error" ( [http:_www.freebase.com/private/suggest?prefix=love&type=/_](example)) if there's an error.

<tt>n:type</tt> is a whole JSON object literal that describes the most notable type of the entity. It is rendered in addition to the entity's name to provide more disambiguation details. In [http:_code.google.com/p/freebase-suggest/_](the+screenshot+here), the notable types are Film director, Film producer, and Film actor.

# flyout Entry Point

The <tt>flyout</tt> entry point takes a single URL parameter: <tt>id</tt>, which is the identifier of the entity to render, as a string. Actually, it also takes a <tt>callback</tt> parameter to support JSONP. It returns a JSON object literal with a single field: <tt>html</tt>, which is the rendered view of the given entity. Example:

[http://www.freebase.com/private/flyout?id=/en/google&callback=f](http://www.freebase.com/private/flyout?id=/en/google&callback=f)

The HTML code should use those <tt>fbs-</tt> CSS class names in order for the flyout to be rendered with style.

