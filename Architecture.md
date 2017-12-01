_OpenRefine's architecture._

OpenRefine is a web application, but unlike 99% of web applications, it is intended to be run on one's own machine and used by oneself. The server-side maintains states of the data (undo/redo history, long-running processes, etc.) while the client-side maintains states of the user interface (facets and their selections, view pagination, etc.). The client-side makes GET and POST ajax calls to cause changes to the data and to fetch data and data-related states from the server-side.

This architecture has emerged from our experience building similar systems such as Simile Longwell CSI, a faceted browser for RDF data. It provides a good separation of concerns (data vs. UI) and also makes it quick and easy to implement user interface features using familiar web technologies. It leaves doors open for collaborative editing support in the future. And it's possible to make the server-side scriptable from the command line.

- [Server Side Architecture](Server-Side): how the data is modeled, stored, changed, etc.
- [Client Side Architecture](Client-Side): how the UI is built
- [Faceted Browsing Architecture](Faceted+Browsing): how faceted browsing is implemented (this straddles the client and the server)
- [Reconciliation Service Api](Reconciliation+Service+API): describes a standard reconciliation service structure.

## Technology Stack

The server-side part of OpenRefine is implemented in Java as one single servlet which is executed by the [http:_jetty.codehaus.org/jetty/_](Jetty) web server + servlet container. The use of Java strikes a balance between performance and portability across operating system (there is very little OS-specific code and has mostly to do with starting the application).

The client-side part of OpenRefine is implemented in Javascript and uses [http:_jquery.com/_](jQuery) and [http:jqueryui.com/](jQueryUI) to simplify portability across modern browsers (old browsers, say NS4 or IE6, are not supported).

The most surprising architectural feature for many is the fact that OpenRefine has no database... or, more precisely, it runs off of its own in-memory data-store that is built up-front to be optimized for the operations required by faceted browsing and infinite undo.

The functional extensibility of OpenRefine is provided by the [http:_code.google.com/p/simile-butterfly/_](SIMILE+Butterfly) modular web application framework.

Several projects provide the functionality to read and write custom format files (POI, opencsv, jRDF, marc4j).

String clustering is provided by the [http:_code.google.com/p/simile-vicino/_](SIMILE+Vicino) project.

OAuth functionality is provided by the [http:_code.google.com/p/oauth-signpost/_](Signpost) project.

