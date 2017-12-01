_How to use the undo/redo change history_

## Basics

Any operation in OpenRefine that changes the data can be undone. Changes are tracked from the very beginning when a project is first created. The undo/redo change history of each project is saved together with the project's data, so closing down your browser and quitting OpenRefine does not disturb the history. When you restart OpenRefine, you can still undo changes that you had made before you closed down your browser and/or quit OpenRefine.

The history is shown as a list of indexed changes, with the first "change" being the action of creating the project itself. (That first change, indexed zero, cannot be undone.) Here is a sample history with 3 changes

```
0. Create project
  1. Remove 7 rows
  2. Create new column Last Name based on column Name by filling 67 rows with grel:value.split(" ")[1] 
  3. Split 230 cell(s) in column Address into several columns by separator
```

There should be exactly one change with a blue background. That is the last performed change. The state of the project is when that change has just been performed.

To revert the state of the project to after some change X has been done, click on X in the history. For example, in the sample history above, suppose we only want to perform all changes up to and including the removal of 7 rows, then click on "Remove 7 rows". The last 2 changes will be undone in order to bring the project back to state #1.

When you've undone some changes, then they are grayed out. In this example, changes #2 and #3 will be grayed out. You can redo up to and including a change by clicking on it in the history. Once you execute a new operation however, all the remaining changes from the redo stack will be cleared and are no longer available.

## Replaying Operations

Operations that you perform in OpenRefine to create changes to the data can be reused. An operation is different from the change that it creates. For example, an operation to transform the cells in column X with some expression creates a change that sets the values of some number of cells. That same operation can be performed on another, similar project, and it would create a different change that sets the values of a different number of cells. (In computer science terms, abstract operations create concrete changes.)

For example, consider two different projects, each containing a column Name that contains people's names in the format "first\_name last\_name". The first project contains 20 names and the second contains 45 names. You can perform the same operation "Split column into several columns" by the separator space on the column Name of each project. In the first project, 20 rows x 2 cells per row = 40 cells will be created, and in the second project, 45 rows x 2 cells per row = 90 cells will be created. Although the two changes are different, they both result from the same abstract, conceptual operation of splitting "first\_name last\_name" by space.

To reuse operations performed in one project in another project, first, extract them out from the first project. In this undo/redo history, click Extract... This brings up a dialog box that lists all operations corresponding to changes that have been done (it does not show undone changes). Select the operations that you want to extract on the left, and they will be encoded as JSON on the right. Copy that JSON off to the clipboard. Then in the second project, in the undo/redo history, click Apply... and paste in that JSON.

Currently, not all changes have operations that can be extracted. Any change performed specifically on some single cell (such as editing a cell on row 213 in the first project) does not have an extractable operation.

There is a limit, if the history is bigger then 100 MB (output file), google chrome will crash and you will not be able to extract the JSON. Firefox can handle around 200 MB(output file), be sure to disable the spellchecker.

