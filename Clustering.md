_How to edit cells by clustering._

OpenRefine's text facets are a great mechanism for surfacing patterns from your data. Consider a data set that contains people's names entered in two different ways: "first\_name middle\_initial last\_name", and "last\_name, first\_name". A text facet on that column might reveal

| choice | count |
| --- | --- |
| Andy Anderson | 79 |
| Andy R. Anderson | 9 |
| Anderson, Andy | 57 |
| Beatrice Beaufort | 28 |
| Beatrice Mansfield | 67 |
| Beaufort, Beatrice | 19 |
| ... | ... |

Say you want to change every name to "first\_name last\_name", then using that facet, you would need to edit "Anderson, Andy" to "Andy Anderson", "Beaufort, Beatrice" to "Beatrice Beaufort", and so forth. And of course there are the occasional middle initials to worry about. While editing through the text facet is already orders of magnitude easier than editing the same data in a spreadsheet, there is a more automated way to do this: the Clustering feature.

The Clustering feature can be accessed in 2 different ways. If you have already created a default text facet on a column, the text facet will show a "Cluster" button near its top right corner. If you haven't, you can invoke the column's drop-down menu and pick <tt>Edit cells &gt; Cluster and edit...</tt>

The clustering feature works by trying to group the choices in the text facet, so that choices that "look similar" get grouped together. For instance, operating on our example data above, the clustering feature would generate the group

- Andy Anderson (79)
- Andy R. Anderson (9)
- Anderson, Andy (57)

and the group

- Beatrice Beaufort (28)
- Beaufort, Beatrice (19)

and so forth. You can select each group whose choices you want to "merge" and enter the text that all choices in that group will be replaced with. If we select the first group to be merged into "Andy Anderson", then 79 + 9 + 57 = 145 cells will contain the text "Andy Anderson".

You can adjust the way the clustering feature groups the choices--that is, how the feature determines that choices "look similar". When the feature is "conservative", it might consider "Andy Anderson" and "Anderson, Andy" to be similar, but not "Andy R. Anderson" and "Andy Anderson". When the feature is "liberal" or "aggressive", it might consider "Andy Anderson", "Sandy Anderson", and "Landy Sanderson" to all be similar. How conservative or liberal the feature should work depends on your data, but it is safer to be conservative and cautious in merging. Of course, you can always undo each cluster and edit operation.

For more of the theory behind clustering, see [Clustering In Depth](Clustering+in+Depth).

