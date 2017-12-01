_How to start writing an extension_

(This document applies to v2.0 and later.)

Giuliano Tortoreto write a separate documentation detailling how to build extension for OpenRefine. A LaTeX and PDF version are available on his repository [https:_github.com/giTorto/OpenRefineExtensionDoc_](giTorto%2FOpenRefineExtensionDoc) (to do: convert the documentation to Creole and add it to this wiki)

Owen Stephens wrote a guide to developing an extension which specifically adds new GREL functions to OpenRefine. This is available as a blog post: [http:_www.meanboyfriend.com/overdue\_ideas/2017/05/writing-an-extension-to-add-new-grel-functions-to-openrefine/_](Writing+an+extension+to+add+new+GREL+functions+to+OpenRefine)

OpenRefine makes use of the [http:_code.google.com/p/simile-butterfly/_](Butterfly+framework) to provide an extension architecture. OpenRefine extensions are Butterfly modules. You don't really need to know about Butterfly itself, but you might encounter "butterfly" here and there in the code base.

Note that Butterfly was chosen over OSGi because

- Butterfly modules can contain both Java code (server-side) as well as webapp code (client-side); and
- Unlike OSGi plugins, Butterfly modules don't have to be jarred and copied into one specific directory--both of which tasks make development very painful.

Extensions that come with the code base are located under [https:_github.com/OpenRefine/OpenRefine/tree/master/extensions_](the+extensions+subdirectory), but when you develop your own extension, you can put its code anywhere as long as you point Butterfly to it. That is done by any one of the following methods

- refer to your extension's directory in [http:_github.com/OpenRefine/OpenRefine/blob/master/main/webapp/WEB-INF/butterfly.properties_](the+butterfly.properites+file) through a <tt>butterfly.modules.path</tt> setting.
- specify the butterfly.modules.path property on the command line when you run OpenRefine. This overrides the values in the property file, so you need to include the default values first e.g. <tt>-Dbutterfly.modules.path=modules,../../extensions,/path/to/your/extension</tt>

Please note that you should bundle any dependencies yourself, so you are insulated from Refine packaging changes over time.

# Directory Layout

A OpenRefine extension (implemented as a Butterfly module) sits in a file directory that contains the following files and sub-directories:

```
build.xml
  src/
      com/foo/bar/... *.java source files
  module/
      *.html, *.vt files
      scripts/... *.js files
      styles/... *.css and *.less files
      images/... image files
      MOD-INF/
          lib/*.jar files
          classes/... java class files
          module.properties
          controller.js
```

The file named module.properties (see [http:_github.com/OpenRefine/OpenRefine/blob/master/extensions/sample-extension/module/MOD-INF/module.properties_](example)) contains the extension's metadata. Of importance is the name field, which gives the extension a name that's used in many other places to refer to it. This can be different from the extension's directory name.

```
name = my-extension-name
```

Your extension's client-side resources (.html, .js, .css files) stored in the module/ subdirectory will be accessible from [http://127.0.0.1:3333/extension/my-extension-name/](http://127.0.0.1:3333/extension/my-extension-name/).

Also of importance is the dependency

```
requires = core
```

which makes sure that the core module of OpenRefine is loaded before the extension attempts to hook into it.

The file named controller.js is responsible for registering the extension's hooks into OpenRefine. Look at the sample-extension extension's [http:_github.com/OpenRefine/OpenRefine/blob/master/extensions/sample/module/MOD-INF/controller.js_](controller.js) file for an example. It should have a function called init() that does the hook registrations.

The <tt>build.xml</tt> file is an [http:_ant.apache.org/_](Apache+Ant) build file. You can make a copy of the sample extension's <tt>build.xml</tt> file to get started. The important point here is that the Java classes should be built into the <tt>module/MOD-INF/classes</tt> sub-directory.

Note that your extension's Java code would need to reference some libraries used in OpenRefine and Refine's Java classes themselves. The libraries are in [https:_github.com/OpenRefine/OpenRefine/tree/master/main/webapp/WEB-INF/lib_](trunk%2Fmain%2Fwebapp%2FWEB-INF%2Flib%2F) and Refine's classes are in [https:github.com/OpenRefine/OpenRefine/tree/master/main/webapp/modules/core/MOD-INF/classes](trunk%2Fmain%2Fwebapp%2Fmodules%2Fcore%2FMOD-INF%2Fclasses%2F).

