_Server-side architecture._

OpenRefine's server-side is written entirely in Java (<tt>main/src/</tt>) and its entry point is the Java servlet <tt>com.google.refine.RefineServlet</tt>. By default, the servlet is hosted in the lightweight Jetty web server instantiated by <tt>server/src/com.google.refine.Refine</tt>. Note that the server class itself is under <tt>server/src/</tt>, not <tt>main/src/</tt>; this separation leaves the possibility of hosting <tt>RefineServlet</tt> in a different servlet container.

The web server configuration is in <tt>main/webapp/WEB-INF/web.xml</tt>; that's where <tt>RefineServlet</tt> is hooked up. <tt>RefineServlet</tt> itself is simple: it just reacts to requests from the client-side by routing them to the right <tt>Command</tt> class in the packages <tt>com.google.refine.commands.**</tt>.

As mentioned before, the server-side maintains states of the data, and the primary class involved is <tt>com.google.refine.ProjectManager</tt>.

## Projects

In OpenRefine there's the concept of a workspace similar to that in Eclipse. When you run OpenRefine it manages projects within a single workspace, and the workspace is embodied in a file directory with sub-directories, which by default is specified [FAQ:-Where-Is-Data-Stored%3F]. You could get OpenRefine to use a different directory by specifying a -d parameter at the command line.

The class <tt>ProjectManager</tt> is what manages the workspace. It keeps in memory the metadata of every project (in the class <tt>ProjectMetadata</tt>). This metadata includes the project's name and last modified date, and any other information necessary to present and let the user interact with the project as a whole. Only when the user decides to look at the project's data would <tt>ProjectManager</tt> load the project's actual data. The separation of project metadata and data is to minimize the amount of stuff loaded into memory.

A project's _actual_ data includes the columns, rows, cells, reconciliation records, and history entries.

A project is loaded into memory when it needs to be displayed or modified, and it remains in memory until 1 hour after the last time it gets modified. Periodically the project manager tries to save modified projects, and it saves as many modified projects as possible within 30 seconds.

## Data Model

A project's data consists of

- _raw data_: a list of rows, each row consisting of a list of cells
- _models_ on top of that raw data that give high-level presentation or interpretation of that data. This design lets the same raw data be viewed in different ways by different models, and let the models be changed without costly changes to the raw data.

### Column Model

Cells in rows are not named and can only be addressed by their list position indices. So, a _column model_ is needed to give a name to each list position. The column model also stores other metadata for each column, including the type that cells in the column have been reconciled to and the overall reconciliation statistics of those cells.

Each column also acts as a cache for data computed from the raw data related to that column.

Columns in the column model can be removed and re-ordered without changing the raw data--the cells in the rows. This makes column removal and ordering operations really quick.

#### Column Groups

Consider the following data:

![](https://raw.github.com/OpenRefine/OpenRefine/2.0/graphics/row-groups.png)

Although the data is in a grid, we humans can understand that it is a tree. First of all, all rows contain data ultimately linked to the movie Austin Powers, although only one row contains the text "Austin Powers" in the "movie title" column. We also know that "USA" and "Germany" are not related to Elizabeth Hurley and Mike Myers respectively (say, as their nationality), but rather, "USA" and "Germany" are related to the movie (where it was released). We know that Mike Myers played both the character "Austin Powers" and the character "Dr. Evil"; and for the latter he received 2 awards. We humans can understand how to interpret the grid as a tree based on its visual layout as well as some knowledge we have about the movie domain but is not encoded in the table.

OpenRefine can capture our knowledge of this transformation from grid to tree using _column groups_, also stored in the column model. Each column group illustrated as a blue bracket above specifies which columns are grouped together, as well as which of those columns is the key column in that group (blue triangle). One column group can span over columns grouped by another column group, and in this way, column groups form a hierarchy determined by which column group envelopes another. This hierarchy of column groups allows the 2-dimensional (grid-shaped) table of rows and cells to be interpreted as a list of hierarchical (tree-shaped) data records.

Blank cells play a very important role. The blank cell in a key column of a row (e.g., cell "character" on row 4) makes that row (row 4) _depend_ on the first preceding row with that column filled in (row 3). This means that "Best Comedy Perf" on row 4 applies to "Dr. Evil" on row 3. Row 3 is said to be a _context row_ for row 4. Similarly, since rows 2 - 6 all have blank cells in the first column, they all depend on row 1, and all their data ultimately applies to the movie Austin Powers. Row 1 depends on no other row and is said to be a _record row_. Rows 1 - 6 together form one _record_.

(As of 2010/05/13, only the XML importer creates column groups. The data table view does display column groups but it doesn't support modifying them.)

### Protograph

The _protograph model_ is similar to column groups, but it's intended to transform the grid-shaped data to fit Freebase graph schemas for loading into Freebase. Protograph is the internal technical term; in the UI, it's referred to as the schema alignment skeleton.

Ideally, the protograph model should be more closely connected with the column groups, so that changes to one would somehow be propagated to the other. For example, creating the protograph would also create column groups if they don't already exist; and creating column groups would create a protograph with the same shape (but without Freebase schema information). As of 2010/05/13, the protograph model and the column groups are not connected in any way.

## Changes, History, Processes, and Operations

Changes to a project's metadata (e.g., name) are not tracked. Only changes to the project's data are tracked. Changes are stored as <tt>com.google.refine.history.Change</tt> objects. <tt>com.google.refine.history.Change</tt> is an interface, and implementing classes are in <tt>com.google.refine.model.changes.**</tt>. Each change object stores enough data to modify the project's data when its <tt>apply()</tt> method is called, and enough data to revert its effect when its <tt>revert()</tt> method is called. It's only supposed to _store_ data, not to actually _compute_ the change. In this way, it's like a .diff patch file for a code base.

Some change objects can be huge, as huge as the project itself. So change objects are not kept in memory except when they are to be applied or reverted. However, since we still need to show the user some information about changes (as displayed in the History panel in the UI), we keep metadata of changes separate from the change objects. For each change object there is one corresponding <tt>com.google.refine.history.HistoryEntry</tt> for storing its metadata, such as the change's human-friendly description and timestamp.

Each project has a <tt>com.google.refine.history.History</tt> object that contains an ordered list of all <tt>HistoryEntry</tt> objects storing metadata for all changes that have been done since after the project was created. Actually, there are 2 ordered lists: one for done changes that can be reverted (undone), an done for undone changes that can be re-applied (redone). Changes must be done or redone in their exact orders in these lists because each change makes certain assumptions about the state of the project before and after it is applied. As changes cannot be undone/redone out of order, when one change fails to revert, it blocks the whole history from being reverted to any state preceding that change (as happened in Issue #2).

As mentioned before, a change contains only the diff and does not actually compute that diff. The computation is performed by a <tt>com.google.refine.process.Process</tt> object--every change object is created by a process object. A process can be immediate, producing its change object synchronously within a very short period of time (e.g., starring one row); or a process can be long-running, producing its change object after a long time and a lot of computation, including network calls (e.g., reconciling a column).

As the user interacts with the UI on the client-side, their interactions trigger ajax calls to the server-side. Some calls are meant to modify the project. Those are handled by commands that instantiates processes. Processes are queued in a first-in-first-out basis. The first-in process gets run and until it is done all the other processes are stuck in the queue.

A process can effect a change in one thing in the project (e.g., edit one particular cell, star one particular row), or a process can effect changes in _potentially_ many things in the project (e.g., edit zero or more cells sharing the same content, starring all rows filtered by some facets). The latter kind of process is generalizable: it is meaningful to apply them on another similar project. Such a process is associated with an _abstract operation_ <tt>com.google.refine.model.AbstractOperation</tt> that encodes the information necessary to create another instance of that process, but potentially for a different project. When you click "extract" in the History panel, these abstract operations are called to serialize their information to JSON; and when you click "apply" in the History panel, the JSON you paste in is used to re-construct these abstract operations, which in turn create processes, which get run sequentially in a queue to generate change object and history entry pairs.

In summary,

- change objects store diffs
- history entries store metadata of change objects
- processes compute diffs and create change object and history entry pairs
- some processes are long-running and some are immediate; processes are run sequentially in a queue
- generalizable processes can be re-constructed from abstract operations

## Expressions
