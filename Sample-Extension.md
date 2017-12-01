_Explains the sample extension._

The sample extension is included in the code base so that you can copy it and get started on writing your own extension. After you copy it, make sure you change its name inside its <tt>module/MOD-INF/controller.js</tt> file.

# Basic Structure

The sample extension's code is in <tt>refine/extensions/sample/</tt>. In that directory, Java source code is contained under the <tt>src</tt> sub-directory, and webapp code is under the <tt>module</tt> sub-directory. Here is the full directory layout:

```
refine/extensions/sample/
      build.xml (ant build script)
      src/
          com/google/refine/sampleExtension/
              ... Java source code ...
      module/
          MOD-INF/
              module.properties (module settings)
              controller.js (module init and routing logic in Javascript)
          classes/
              ... compiled Java classes ...
          lib/
              ... Java jars ...
          ... velocity templates (.vt) ...
          ... LESS css files ...
          ... client-side files (.html, .css, .js, image files) ...
```

The sub-directory <tt>MOD-INF</tt> contains the Butterfly module's metadata and is what Butterfly looks for when it scans directories for modules. <tt>MOD-INF</tt> serves similar functions as <tt>WEB-INF</tt> in other web frameworks.

Java code is built into the sub-directory <tt>classes</tt> inside <tt>MOD-INF</tt>, and supporting external Java jars are in the <tt>lib</tt> sub-directory. Those will be automatically loaded by Butterfly. (The build.xml script is wired to compile into the <tt>classes</tt> sub-directory.)

Client-side code is in the inner <tt>module</tt> sub-directory. They can be plain old .html, .css, .js, and image files, or they can be [http:_lesscss.org/_](LESS) files that get processed into CSS. There are also Velocity .vt files, but they need to be routed inside <tt>MOD-INF/controller.js</tt>.

<tt>MOD-INF/controller.js</tt> lets you configure the extension's initialization and URL routing in Javascript rather than in Java. For example, when the requested URL path is either <tt>/</tt> or an empty string, we process and return <tt>MOD-INF/index.vt</tt> ( [http:_localhost:3333/extension/sample/_](see+http%3A%2F%2Flocalhost%3A3333%2Fextension%2Fsample%2F) if OpenRefine is running).

The <tt>init()</tt> function in <tt>controller.js</tt> allows the extension to register various client-side handlers for augmenting pages served by Refine's core. These handlers are feature-specific. For example, [https:_github.com/OpenRefine/OpenRefine/blob/master/extensions/jython/module/MOD-INF/controller.js#L46_](here+is+where+and+how) the jython extension adds its parser. As for the sample extension, it adds its script <tt>project-injection.js</tt> and style <tt>project-injection.less</tt> into the <tt>/project</tt> page. If you [view-source:http:localhost:3333/project view source the /project page], you'd see references to those two files.

# Wiring Up the Extension

Butterfly is able to locate the sample extension because of [https:_github.com/OpenRefine/OpenRefine/blob/master/main/webapp/WEB-INF/butterfly.properties#L27_](the+path+provided+in+butterfly.properties). Butterfly simply descends into each of those paths and look for <tt>MOD-INF</tt> directories.

For more information, see [Extension Points](Extension+Points).

