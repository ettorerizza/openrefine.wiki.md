_How extensions can hook into various points in OpenRefine._

Extensions were first introduced in v1.5, so this document is not applicable for earlier versions.

# Client-side Resources

## Javascript and CSS

The UI in OpenRefine for working with a project is coded in [http:_github.com/OpenRefine/OpenRefine/blob/master/main/webapp/modules/core/project.vt_](this+file). The file is quite small, and that's because almost all of its content is to be expanded dynamically through the Velocity variables $scriptInjection and $styleInjection. So that your own Javascript and CSS files get loaded, you need to register them with the ClientSideResourceManager. See [http:github.com/OpenRefine/OpenRefine/blob/master/extensions/sample-extension/module/MOD-INF/controller.js](this+file) for an example.

In the registration call, the variable <tt>module</tt> is already available to your code by default, and it refers to your own extension.

```
ClientSideResourceManager.addPaths(
        "project/scripts",
        module,
        [
            "scripts/foo.js",
            "scripts/subdir/bar.js"
        ]
    );
```

You can specify one or more files for registration, and their paths are relative to the <tt>module</tt> subdirectory of your extension. They are included in the order listed.

Javascript Bundling: Note that <tt>project.vt</tt> belongs to the core module and is thus under the control of the core module's [http:_github.com/OpenRefine/OpenRefine/blob/master/main/webapp/modules/core/MOD-INF/controller.js_](controller.js+file). The Javascript files to be included in <tt>project.vt</tt> are by default bundled together for performance. When debugging, you can prevent this bundling behavior by setting <tt>bundle</tt> to <tt>false</tt> near the top of that <tt>controller.js</tt> file. (If you have commit access to this code base, be sure not to check that change in.)

## Images

We recommend that you always refer to images through your CSS files rather than in your Javascript code. URLs to images will thus be relative to your CSS files, e.g.,

```
.foo {
    background: url(../images/x.png);
  }
```

If you really really absolutely need to refer to your images in your Javascript code, then look up your extension's URL path in the global Javascript variable <tt>ModuleWirings</tt>:

```
ModuleWirings["my-extension"] + "images/x.png"
```

## Client-side HTML Templates

Beside Javascript, CSS, and images, your extension might also include HTML templates that get loaded on the fly by your Javascript code and injected into the page's DOM. For example, here is [http:_github.com/OpenRefine/OpenRefine/blob/master/main/webapp/modules/core/scripts/dialogs/clustering-dialog.html_](the+Cluster%2FEdit+dialog+template), which gets loaded by code in [http:github.com/OpenRefine/OpenRefine/blob/master/main/webapp/modules/core/scripts/dialogs/clustering-dialog.js](this+Javascript+file):

```
var dialog = $(DOM.loadHTML("core", "scripts/dialogs/clustering-dialog.html"));
```

<tt>DOM.loadHTML</tt> returns the content of the file as a string, and <tt>$(...)</tt> turns it into a DOM fragment. Where <tt>"core"</tt> is, you would want your extension's name. The path of the HTML file is relative to your extension's <tt>module</tt> subdirectory.

# Project UI Extension Points

Getting your extension's Javascript code included in <tt>project.vt</tt> doesn't accomplish much by itself unless your code also registers hooks into the UI. For example, you can surely implement an exporter in Javascript, but unless you add a corresponding menu command in the UI, your user can't use your exporter.

## Main Menu

The main menu can be extended by calling any one of the methods <tt>MenuBar.appendTo</tt>, <tt>MenuBar.insertBefore</tt>, and <tt>MenuBar.insertAfter</tt>. Each method takes 2 arguments: an array of strings that identify a particular existing menu item or submenu, and one new single menu item or submenu or an array of menu items and submenus. For example, to insert 2 menu items and a menu separator before the menu item Project > Export Filtered Rows > Templating..., write this Javascript code wherever that would execute when your Javascript files get loaded:

```
MenuBar.insertBefore(
        ["core/project", "core/export", "core/export-templating"],
        [
            {
                "label":"Menu item 1",
                "click": function() { ... }
            },
            {
                "label":"Menu item 2",
                "click": function() { ... }
            },
            {} // separator
        ]
    );
```

The array <tt>["core/project", "core/export", "core/export-templating"]</tt> pinpoints the reference menu item.

See the beginning of [http:_github.com/OpenRefine/OpenRefine/blob/master/main/webapp/modules/core/scripts/project/menu-bar.js_](this+file) for IDs of menu items and submenus.

## Column Header Menu

The drop-down menu of each column can also be extended, but the mechanism is slightly different compared to the main menu. Because the drop-down menu for a particular column is constructed on the fly when the user actually clicks the drop-down menu button, extending the column header menu can't really be done once at start-up time, but must be done every time a column header menu gets created. So, registration in this case involves providing a function that gets called each such time:

```
DataTableColumnHeaderUI.extendMenu(function(column, columnHeaderUI, menu) { ... do stuff to menu ... });
```

That function takes in the column object (which contains the column's name), the column header UI object (generally not so useful), and the menu to extend. In the previous code line where it says "do stuff to menu", you can write something like this:

```
MenuSystem.appendTo(menu, ["core/facet"], [
        {
            id: "core/text-facet",
            label: "My Facet on " + column.name,
            click: function() {
                ... use column.name and do something ...
            }
        },
    ]);
```

In addition to <tt>MenuSystem.appendTo</tt>, you can also call <tt>MenuSystem.insertBefore</tt> and <tt>MenuSystem.insertAfter</tt> which the same 3 arguments. To see what IDs you can use, see the function <tt>DataTableColumnHeaderUI.prototype._createMenuForColumnHeader</tt> in [http:_github.com/OpenRefine/OpenRefine/blob/master/main/webapp/modules/core/scripts/views/data-table/column-header-ui.js_](this+file).

# Server-side Extension Points

## Ajax Commands

The client-side of OpenRefine gets things done by calling AJAX commands on the server-side. These commands must be registered with the OpenRefine servlet, so that the servlet knows how to route AJAX calls from the client-side. This can be done inside the <tt>init</tt> function in your extension's <tt>controller.js</tt> file, e.g.,

```
function init() {
      var RefineServlet = Packages.com.google.refine.RefineServlet;
      RefineServlet.registerCommand(module, "my-command", new Packages.com.foo.bar.MyCommand());
  }
```

Your command will then be accessible at [http://127.0.0.1:3333/command/my-extension/my-command](http://127.0.0.1:3333/command/my-extension/my-command).

## Operations

Most commands change the project's data. Most of them do so by creating abstract operations. See the Changes, History, Processes, and Operations section of the [Server Side Architecture](Server-side+Architecture) document.

You can register an operation **class** in the <tt>init</tt> function as follows:

```
Packages.com.google.refine.operations.OperationRegistry.registerOperation(
      module, 
      "operation-name",
      Packages.com.foo.bar.MyOperation
  );
```

Do not call <tt>new</tt> to construct an operation instance. You must register the class itself. The class should have a static function for reconstructing an operation instance from a JSON blob:

```
static public AbstractOperation reconstruct(Project project, JSONObject obj) throws Exception {
      ...
  }
```

## GREL

GREL can be extended with new functions. This is also done in the <tt>init</tt> function in <tt>controller.js</tt>, e.g.,

```
Packages.com.google.refine.grel.ControlFunctionRegistry.registerFunction(
        "functionName", new Packages.com.foo.bar.TheFunctionClass());
```

You might also want to provide new variables (beyond just <tt>value</tt>, <tt>cells</tt>, <tt>row</tt>, etc.) available to expressions. This is done by registering a binder that implements the interface <tt>com.google.refine.expr.Binder</tt>:

```
Packages.com.google.refine.expr.ExpressionUtils.registerBinder(
        new Packages.com.foo.bar.MyBinder());
```

## Importers

You can register an importer as follows:

```
Packages.com.google.refine.importers.ImporterRegistry.registerImporter(
      "importer-name", new Packages.com.foo.bar.MyImporter());
```

The string <tt>"importer-name"</tt> isn't important at all. It's not really related to file extension or mime-type. Just use something unique. Your importer will be explicitly called to test if it can import something.

## Exporters

You can register an exporter as follows:

```
Packages.com.google.refine.exporters.ExporterRegistry.registerExporter(
      "exporter-name", new Packages.com.foo.bar.MyExporter());
```

The string <tt>"exporter-name"</tt> isn't important at all. It's only used by the client-side to tell the server-side which exporter to use. Just use something unique and, of course, relevant.

## Overlay Models

Overlay models are objects attached onto a core Project object to store and manage additional data for that project. For example, the schema alignment skeleton is managed by the Protograph overlay model. An overlay model implements the interface <tt>com.google.refine.model.OverlayModel</tt> and can be registered like so:

```
Packages.com.google.refine.model.Project.registerOverlayModel(
      "model-name",
      Packages.com.foo.bar.MyOverlayModel);
```

Note that you register the **class** , not an instance. The class should implement the following static method for reconstructing an overlay model instance from a JSON blob:

```
static public OverlayModel reconstruct(JSONObject o) throws JSONException {
      ...
  }
```

When the project gets saved, the overlay model instance's <tt>write</tt> method will be called:

```
public void write(JSONWriter writer, Properties options) throws JSONException {
      ...
  }
```

## Scripting Languages

A scripting language (such as Jython) can be registered as follows:

```
Packages.com.google.refine.expr.MetaParser.registerLanguageParser(
        "jython",
        "Jython",
        Packages.com.google.refine.jython.JythonEvaluable.createParser(),
        "return value"
    );
```

The first string is the prefix that gets prepended to each expression so that we know which language the expression is in. This should be short, unique, and identifying. The second string is a user-friendly name of the language. The third is an object that implements the interface <tt>com.google.refine.expr.LanguageSpecificParser</tt>. The final string is the default expression in that language that would return the cell's value.

