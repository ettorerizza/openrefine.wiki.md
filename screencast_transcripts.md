_Textual transcripts for the speech over the screencasts_

# Screencast 1 - Introduction to Gridworks

(coming soon)

# Screencast 2 - Faceting with Gridworks

### Part 1 - Using Facets to understand the data

In this screencast we'll show you how to use Gridworks and its faceting engine to obtain insights into rectangular datasets.

We'll start by looking at a dataset of various foods and their contents.

<tt> (click on food)</tt>

The dataset contains more than 7 thousand values.

For that reason, it's hard to obtain a global understanding of its content via pagination

(click on a few pages)

Gridworks has been designed around the concept of 'facets' which are condensed summaries of a particular column where only unique values are shown.

For example, we can turn a column of more than 7 thousand numbers into a graphical distribution that is much easier to grasp.

(create a numeric facet on water content)

Once we have a facet, we can use it to focus and filter only on the part of the distribution we're interested in.

(filter on the numeric facet -> high content)

We can have more than one facet at the same time.

(create a numeric facet on energy content)

And discover relationships inside the data that were hard to appreciate simply by looking at the grid of rows.

For example, we can focus only on the foods with low water content.

(focus on foods with low water content)

Which leads us to discover that foods that have high energetic content tend to have low water content.

But which ones are the foods with low water content?

The 'short description' column gives us that information, so we can create a textual facet over it.

(create facet on food names)

Unfortunately, these are very detailed names and don't repeat. We can see that by ordering the facet by counts.

(change facet sort order)

which makes this facet not more useful than its original column.

But we notice that the beginning part of foods are repeated.

We can use a custom textual facet and process the cells of a column thru an expression.

(open custom textual facet dialog)

Here, we want the string before the first comma, which can be obtained by splitting by ',' and then taking the first value of the resulting array.

(create a new custom text facet with value.split(",")[0])

This dialog is interactive

and it's useful to experiment in real time with the Gridworks expression language

(do some changes)

and find information on the available functions

(show help and scroll)

Once we're satisfied with the results, we click ok to generate the facet.

(click ok)

This new facet is more useful that the previous one as the values now repeat. We can use it to understand the types of foods that are contained in the current selection.

(sort by count)

We noticed that the most common type of food with low water content in this dataset is 'cereals'. We can click on the facet value and restrict the grid to show only cereals.

(click on 'cereals')

By inspecting the grid, we note that the description contains the cereal brand and we wonder how many brands are covered in this dataset.

We can use again the custom textual facet for this task.

(create a new custom facet with value.split(',')[1])

And see a breakdown of all brands of cereals included in the dataset.

* * *

The expression language can be used on numeric facets as well.

The distribution of cholesterol values, for example is very skewed

(make facet of cholesterol values)

which makes it harder to discern trends as most foods have low cholesterol content.

We can create a custom numeric facet using the logarithm of the cholesterol value

(make custom facet of log of cholesterol value)

to show details that were not perceivable in the linear distribution.

### Part 2 - Using Facets to cleanup the data

We like to believe that all datasets are clean and internally consistent, but in real life, data gets integrated from various sources and internal consistency gets degraded.

Let's see how we can use Gridworks to find and correct internal consistency in a rectangular dataset.

(select the US Government contracts dataset)

We'll work on a dataset that contains contracts between the U.S. government and private contractors, obtained from the US government ID dashboard web site.

The first we want to focus on is to understand how many different types of contracts there are.

To do that, we create a text facet over the 'contract type' column.

(make a text facet over the contract type column)

Then sort by count to highlight the most common types

(sort by count)

The first thing we notice is that the first three values "FFP", "FFP: Firm Fixed Price" and "Firm Fixed Price" are very likely to be three different ways to indicate the same type of contract.

In other programs, fixing these inconsistencies would mean running several 'search and replace' operations across all the rows, with the risk of replacing text present in other cells.

Gridworks provides a way to edit just the cell values of particular filtered rows, removing the risk of replacing the wrong values.

To do this, simply click 'edit' next to the facet value that we want to modify type the new value and hit enter.

(click edit on the facet value, change text and hit enter)

All the cells in that column that contained precisely that string now contain the new string value, and no other cells have been modified in the grid.

(do a few more)

In-place editing of facet values is a very powerful mechanism to fix few inconsistencies, but what about when the number of values and potential inconsistencies is overwhelming or might be hard to spot?

Gridworks contains a sophisticated clustering engine that helps finding values inconsistencies.

(open the clustering dialog on the facet)

Clustering is the action of finding groups of values that are similar to one another and are likely to be variations of the same value.

Here we see how the Gridworks clustering engine was able to find sets of cell values that are all various ways to write the same thing.

By clicking on one of the cluster values, we are instructing Gridworks to take all the cells that contain the values in the cluster and change them all with the value we clicked on.

Note how Gridworks automatically picks the most common value in the cluster as the most promising candidate.

(click on a few values)

Sometimes the clustering engine picks all clusters that are good, so we can just select all and apply the changes.

(select all and apply the clustering)

No further clusters are found with the fingerprinting method (which is the fastest and most conservative and the one used first) so we move on to the next

(select ngram fingerprinting)

This method uses more relaxed constrains so it found clusters that the previous method didn't found. Note how the second cluster has all those html entities in there, keep that in mind as we'll come back to that later.

As they look good, we select them all and apply.

(select all and apply)

Even this method found no more clusters, so we can decide to change method or to try changing parameters.

Let's make relax the constrains even more by changing the ngram size value to 1.

(change ngram size to 1)

Here we need to be careful because the method can be too lax and find clusters of values that should not be merged together.

For example, the "Time and Materials" cluster is good, but certainly "Financial & Management Support: Fixed Price" is not the same thing as "Engineering and Implementation Support - Fixed Price", so it should not be selected.

(select a few that are good)

Once we're satisfied with our selection we can apply and close to go back to the grid view.

(apply & close)

It's important to note that the clustering engine works exclusively on the string values and how closely related they are to one another.

For that reason there are things that it won't catch, like the fact that "T&M" is likely to mean "Time & Materials". We need to do that by hand.

(do a few more edits)

If we look at the history

(look at the history)

we see that we have selectively changed the values of more than 2 thousands cells just with a few clicks of the mouse.

* * *

Another facet that can be very useful is the textual search facet.

Here we can use it to spot HTML entities that might need to be escaped.

(create a text regexp search facet with &([a-z]\*);)

We have used a regular expression to look for all strings that can be interpreted as HTML entities and found 337 rows that contain them.

(highlight the row count)

Now that we've filtered only to the rows we want to work on, we can use the cell transformation command to clean it up

(transform with value.unescape('html') and apply)

Unfortunately, there are still 54 cells that contain entities, many of which were recursively escaped.

(highlight 54)

Instead of running the same operation multiple times, gridworks offers the ability to run a given transformation expression multiple times until the cell value doesn't change any longer.

(find the last command in the history and apply it recursively)

All the HTML entities have been unescaped back into characters but there are a few more left.

The remaining ones are actually XML entities and not HTML, so we need to escape them properly

(transform with value.unescape('xml')

Now we can remove the text facet which is no longer useful

(remove the text facet)

and try to re-cluster again and see if our changes have made new clusters appear

(cluster)

As we can see, various tools work in synergy inside Gridworks to provide powerful cleanup methods.

* * *

Sometimes facets can be used to find inconsistencies that are more structural, like data that was placed in the wrong column or that it was carelessly reconciled.

If we create a numeric facet with the length of the cell values, we can look for outliers and potentially spot values that were placed in the wrong column or that might have needed a different column altogether.

(create text length facet)

If we focus on the big strings, we see that these type of contracts were really either too spelled out or merged improperly.

* * *

Another common (but very serious) inconsistency is using different dimensions for numerical values in the same column.

Here too Gridworks can help spot problems.

Let's create a numeric facet for the contract values.

(create numeric facet on contract values)

Here we notice that the range goes from 0 to 230 millions. Since it's highly unlikely that the government would have a contact for a few dollars and since most of them are found in that region, it's more likely that some values are expressed in millions of dollars and others in dollars.

Spotting and fixing these inconsistencies is critical to establish the usefulness or credibility of a dataset and we have seen how Gridworks can help there.

### Part 3 - Conclusion

We have shown how Gridworks can use faceting, clustering and transformations together to inspect, understand and cleanse rectangular datasets with unprecedented ease and flexibility.

In future screencasts, we'll show how Gridworks can help in reconciling your data with Freebase in order to augment your own data with data coming from the web of data.

Thanks for listening.

# Screencast 3 - Advanced Faceting with Gridworks

### Part 1 - Scatterfacets

In this screencast, we'll show you how to use Freebase Gridworks and its advanced faceting features to gain even deeper insights into your datasets.

If you don't already know about Gridworks, we reccomend watching the two other introductory screencast first.

Let us continue to inspect the dataset of various foods that we used previously.

(click on food)

As we can see, a lot of the grid cells contain numeric values.

We have seen before how we can use Gridworks's numeric facets

(build a numeric facets on energy)

to obtain a condensed view of the entire column, but this facet doesn't tell us anything about the relationship between columns.

A powerful visual tool to gain insights about relationships between numeric columns is the "scatterplot".

(show the picture of a scatterplot)

A scatterplot is a cloud of dots painted on a surface.

Each dot represents a pair of values, one taken from one column and the other from another, but from the same row.

This visualization is useful because the human brain can quickly and effectively grasp patterns from visual clues.

Scatterplots can quickly show us if the values on two columns have a relationship and if so which kind.

For example, we can have a linear relationship

(show a linear scatterplot)

an inverse bounded relationship

(show a inverse bounded linear scatterplot)

a multiple linear relationship

(show a multiple linar scatterplot)

or no apparent relationship at all

(show a spread out scatterplot)

Another useful property is that it makes is easier to spot 'outliers', which are the pairs of values that stand out.

(show a scatterplot with outliers)

Here we can see that most of the data follows a certain relationship, but some values stand out. Those points tickle our curiosity. Wouldn't it be useful if we could somehow select an area of the scatterplot and filter the rows accordingly?

* * *

Enter the scatterplot facet (or scatterfacet for short)

(show an example of a scatterfacet)

A scatterfacet is an interactive scatterplot that can be used to filter data. In Gridworks, the filtering is is done by clicking on the scatterplot and dragging to create a rectangular selection.

(show the rectangular selection)

With that operation we have told Gridworks to keep the rows represented in the dots inside the rectangular selection and filter out all the other rows

(show the number of rows decreasing)

Like any other facet in Gridworks, scatterfacets don't modify only the rows, but also the content of the other facets accordingly

(show how another facet changes because of a scatterfacet selection)

Unlike other facets that operate on a single column, scatterfacets operate on two different columns. But how do you know which pair of columns will be interesting to look thru a scatterfacet?

Here Gridworks helps you by providing the 'scatterplot matrix' which shows in one compact view thumbnails of the scatterplots of all the pairs of numeric columns in your dataset.

(show the scatterplot matrix)

While it might feel overwhelming, this dialog provides you with a way to glance at scatterplots that might exhibit interesting shapes and focus on them for more exploration.

Clicking on the desired scatterplot closes this dialog and creates the scatterfacet.

* * *

Let's look at what we can accomplish with these scatterfacets.

Let's start with the food dataset and a text facet that shows us what are the most foods in the current selection.

(show the starting point)

Now let's focus on the "energy" column and invoke the scatterplot facet dialog

(show scatterplot matrix)

We can see that since we started from the 'energy' column, all the scatterplot where that column participates are highlighted (in the L shape pattern). We can also mouse over to each thumbnail to get a tooltip that tells us which pairs of columns particpate in the scatterplot.

Let's see what kind of correlation there is between energy content and cholesterol.

(click on the energy vs. cholesterol scatterplot)

We ca see that most foods have low cholesterol contents, but there are some outliers.

Let's increase the dot size to focus on those.

(increase dot size)

Let's focus on those outliers that have the highest cholesterol and the highest energy

(select the top right outliers)

And we found, probably as we expected, to be 'eggs'.

We can also use the selection as a window into the dataset. By dragging it from the center, we can move it around and change the selection.

(move the selection to the top left corner)

And we see that meats have lower energetic content but even higher cholesterol than eggs.

(move the selection to the bottom right corner)

and that fish oil is the food that has the most cholesterol of the very high energy ones.

(remove the scatterfacet)

* * *

Sometimes most dots are located in one area of the scatterplot and make it hard to distinguish further structure. The logarithmic scatterplot plots can help us in that case.

Let's look at the scatterfacet between Vitamin A and Beta Carothene.

(select the "vit\_a\_IU vs. beta carot" scatterfacet)

At first, even enlarging the dots

(elarge dots)

we don't see anything special and we see a direct relationship between the two and some outliers.

But if we look at the plot in log mode

(change to log mode)

then we see a very different story: there is a clear direct relationship between the two, but there are considerable outliers and most are located in the bottom quadrant (meaning that there are foods with more Vitamin A than Beta Carotene).

By rotating the plot, we can perform rectangular selections that follow the relationship better.

(rotate and select the area above the median)

which shows that there are 4 foods that have more Beta Carotene than vitamin A (and might be potential mistakes or worthwhile discoveries).

(remove scatterfacet)

* * *

Another important use of scatterfacets is to use multiple ones at the same time to show how areas in one could impact the shape in another.

Let's use the "energy vs. lipids" scatterfacet

(select the 'energy vs. lipids' scatterfacet)

Here we see that there is a relationship between lipids and energy but it's unclear whether there is more structure inside.

We can know for sure by selecting certain categories of food, say, "beef"

(select "beef")

and we see that while the original scatterplot is now draw in gray, some of the dots are still colored, which represent the rows that were selected (in this case, the ones starting with "BEEF" in the food description).

We can see that all beef tends to be located on one 'branch' of the plot. Let's see if others food exhibit the same localization.

(select "pork")

Pork ends up in the same place, but cereals

(select "cereals")

end up in a different branch. What's interesting is that baby food ends up on both branches, and we make an hypothesis that it's because some baby food contains meats and some doesn't.

Inspecting the results with further selection on the scatterfacet confirms our suspition.

### Part 2 - Faceting for Encoding Analysis

Faceting is not only used to understand and gather insights about the structure of the data but also to estimate and locate potential problems by focusing on outliers.

One interesting example is the use of numeric facets to spot encoding issues, those annoying cases where one software encodes characters in one way and the one reading them interprets it in another way.

The result is normally garbled characters that feel obviously out of place.

(show an example of wrongly encoded content)

Gridworks contains a handy custom numeric facets that shows the unicode numeric values of each character in a column.

(create the custon numeric facet for unicode chars)

Normally, for western languages, most characters are in the ASCII range, that is less than 256 and we see here that most characters are located in that area as well.

But there are outliers and if we focus on them

(select the outliers)

and remove the empty ones

(remove the empty ones)

we see that the 'location" column does, in fact, contain garbled content.

Now that we found content that is not properly encoded, we can try to re-interpret it with a different encoding and see which one was the original one. There is no 100% certain automated method of doing that, but Gridworks helps us with custom expression functionality.

Let's open the transformation dialog for this column

(open the transformation dialog)

and try to reinterpret them as UTF-8 which is the most common encoding.

(type "value.reinterpret('utf8')")

and voila', all the content is now uniformly encoded.

