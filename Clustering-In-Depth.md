_Methods and theory behind the clustering functionality in OpenRefine._

# Introduction

In OpenRefine, clustering refers to the operation of "finding groups of different values that might be alternative representations of the same thing". For example, the two strings "New York" and "new york" are very likely to refer to the same concept and just have capitalization differences. Likewise, "Gödel" and "Godel" probably refer to the same person.

It is worth noting that clustering in OpenRefine works only at the syntactic level (the character composition of the cell value) and while very useful to spot errors, typos, and inconsistencies it's by no means enough to perform effective semantically-aware reconciliation. This is why OpenRefine uses external semantically-aware reconciliation services (such as Freebase's) to compensate for the deficiencies of syntax-level clustering alone.

# Methodologies

In order to strike a balance between general applicability and usefulness, OpenRefine ships with a selected number of clustering methods and algorithms that have proven effective and fast enough to use in a wide variety of situations and are ordered from strict to lax and should be used in this order.

## Key Collision Methods

"Key Collision" methods are based on the idea of creating an alternative representation of a value (a "key") that contains only the most valuable or meaningful part of the string and 'bucket's (or 'bin' as it's described inside OpenRefine's code) together different strings based on the fact that their key is the same (hence the name "key collision").

This class of methods is the fastest in OpenRefine because its computational complexity is linear in the number of values processed and can produce results in seconds even with millions of values to cluster.

### Fingerprint

The fingerprinting method is fast and simple yet works relatively well in a variety of contexts and it's the least likely to produce false positives, which is why it is the default method.

The process that generates the key from a string value is the following (note that the order of these operations is significant):

- remove leading and trailing whitespace
- change all characters to their lowercase representation
- remove all punctuation and control characters
- normalize extended western characters to their ASCII representation (for example "gödel" → "godel")
- split the string into whitespace-separated tokens
- sort the tokens and remove duplicates
- join the tokens back together

If you're curious, the code that performs this is [http:_github.com/OpenRefine/OpenRefine/blob/master/main/src/com/google/refine/clustering/binning/FingerprintKeyer.java_](here).

There are several factors that play a role in this fingerprint:

- because whitespace is normalized, characters are lowercased, and punctuation is removed, those parts don't play a differentiation role in the fingerprint. Because these attributes of the string are the least significant in terms of meaning differentiation, these turn out to be the most varying parts of the strings and removing them has a substantial benefit in emerging clusters.
- because the string parts are sorted, the given order of tokens doesn't matter (so "Cruise, Tom" and "Tom Cruise" both end up with a fingerprint "cruise tom" and therefore end up in the same cluster)
- normalizing extended western characters plays the role of reproducing data entry mistakes performed when entering extended characters with an ASCII-only keyboard. Note that this procedure can also lead to false positives, for example "gödel" and "godél" would both end up with "godel" as their fingerprint but they're likely to be different names, so this might work less effectively for datasets where extended characters play substantial differentiation role.

## N-Gram Fingerprint

The [http:_en.wikipedia.org/wiki/N-gram_](n-gram) fingerprint method is similar to the fingerprint method described above but instead of using whitespace separated tokens, it uses n-grams, where the <tt>n</tt> (or the size in chars of the token) can be specified by the user.

Here is what it does (or [http:_github.com/OpenRefine/OpenRefine/blob/master/main/src/com/google/refine/clustering/binning/NGramFingerprintKeyer.java_](check+the+code) if you're curious about the details, again the order is significant):

- change all characters to their lowercase representation
- remove all punctuation, whitespace, and control characters
- obtain all the string n-grams
- sort the n-grams and remove duplicates
- join the sorted n-grams back together
- normalize extended western characters to their ASCII representation

So, for example, the 2-gram fingerprint of "Paris" is "arispari" and the 1-gram fingerprint is "aiprs".

Why is this useful? In practice, using big values for n-grams doesn't yield any advantage over the previous fingerprint method, but using 2-grams and 1-grams, while yielding many false positives, can find clusters that the previous method didn't find even with strings that have small differences, with a very small performance price.

For example "Krzysztof", "Kryzysztof" and "Krzystof" have different lengths and different regular fingerprints, but share the same 1-gram fingerprint because they use the same letters.

### Phonetic Fingerprint

A third keying method uses a phonetic fingerprinting (specifically, [http:_en.wikipedia.org/wiki/Double\_Metaphone#Metaphone\_3_](Metaphone3) method for English and the [http:commons.apache.org/codec/apidocs/org/apache/commons/codec/language/ColognePhonetic.html](Cologne+phonetic+keyer) for German), which is a way to transform tokens into the way they are pronounced. This is useful to spot errors that are due to people misunderstanding or not knowing the spelling of a word after only hearing it. The idea being that similar sounding words will end up sharing the same key and thus being binned in the same cluster.

For example, "Reuben Gevorkiantz" and "Ruben Gevorkyants" share the same phonetic fingerprint for English pronounciation but they have different fingerprints for both the regular and n-gram fingerprinting methods above, no matter the size of the n-gram.

## Nearest Neighbor Methods

While key collisions methods are very fast, they tend to be either too strict or too lax with no way to fine tune how much difference between strings we are willing to tolerate.

The [http:_en.wikipedia.org/wiki/K-nearest\_neighbor\_algorithm_](Nearest+Neighbor) methods (also known as kNN), on the other hand, provide a parameter (the radius, or <tt>k</tt>) which represents a distance threshold: any pair of strings that is closer than a certain value will be binned together.

Unfortunately, given n strings, there are <tt>n(n-1)/2</tt> pairs of strings (and relative distances) that need to be compared and this turns out to be too slow even for small datasets (a dataset with 3000 rows require 4.5 million distance calculations!)

We have tried various methods to speed up this process but the one that works the best is called 'blocking' and is, in fact, a hybrid between key collision and kNN. This works by performing a first pass over the sequence of strings to evaluate and obtain 'blocks' in which all strings share a substring of a given 'blocking size' (which defaults to 6 chars in OpenRefine).

Blocking doesn't change the computational complexity of the kNN method but drastically reduces the number of strings that will be matched against one another (because strings are matched only inside the block that contains them). So instead of <tt>n(n-1)/2</tt> we now have <tt>nm(m-1)/2</tt> but <tt>n</tt> is the number of blocks and <tt>m</tt> is the average size of the block. In practice, this turns out to be dramatically faster because the block size is comparable to the number of strings and the blocks are normally much smaller. For example, for 3000 strings, you can have a thousand blocks composed of 10 strings each, which requires 45k distances to calculate instead of 4.5M!

If you're not in a hurry, OpenRefine lets you select the size of the blocking substring and you can lower it down to 2 or 1 and make sure that blocking is not hiding a potential pair from your search... although in practice, anything lower than 3 normally turns out to be a waste of time.

All the above is shared between all the kNN methods, the difference of operation lies in the method used to evaluate the distance between the two strings.

### Levenshtein Distance

The [http:_en.wikipedia.org/wiki/Levenshtein\_distance_](Levenshtein) distance (also known as "edit distance") is probably the simplest and most intuitive distance function between strings and is often still very effective due to its general applicability.

It measures the minimal number of 'edit operations' that are required to change one string into the other.

For example, "Paris" and "paris" have an edit distance of 1 as changing P into p is the only operation required. "New York" and "newyork" has edit distance 3: 2 substitutions and 1 removal. "Al Pacino" and "Albert Pacino" have an edit distance of 4 because it requires 4 insertions.

In practice, this distance is useful to spot typos, spelling mistakes or anything that the previous methods didn't catch, although large distances yield many false positives (especially for short strings) and are not as useful.

It's worth noting that there are many flavors of edit-based distance functions (say, the [http:_en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein\_distance_](Damerau-Levenshtein+distance), which considers 'transposition' as a single operation) but in practice, for clustering purposes, they tend to be equally functional (as long as the user has control over the distance threshold).

### PPM

This distance is an implementation of [http:_arxiv.org/abs/cs/0111054_](a+seminal+paper) about the use of the [http:en.wikipedia.org/wiki/Kolmogorov\_complexity](Kolmogorov+complexity) to estimate 'similarity' between strings and has been widely applied to the comparison of strings originating from DNA sequencing.

The idea is that because text compressors work by estimating the information content of a string, if two strings A and B are identical, compressing A or compressing A+B (concatenating the strings) should yield very little difference (ideally, a single extra bit to indicate the presence of the redundant information). On the other hand, if A and B are very different, compressing A and compressing A+B should yield dramatic differences in length.

OpenRefine uses a normalized version of the algorithm, where the distance between A and B is given by

```
d(A,B) = comp(A+B) + comp(B+A) / (comp(A+A) + comp(B+B));
```

where <tt>comp(s)</tt> is the length of bytes of the compressed sequence of the string <tt>s</tt> and <tt>+</tt> is the append operator. This is used to account for deviation in optimality of the given compressors.

While many different compressors can be used, the closer to Kolmogorov optimality they are (meaning, the better they encode) the more effective their result.

For this reason we have used [http:_en.wikipedia.org/wiki/Prediction\_by\_Partial\_Matching_](Prediction+by+Partial+Matching) as the compressor algorithm as it is one of the most effective compression algorithms for text and works by performing statistical analysis and predicting what character will come next in a string.

In practice, this method is very lax even for small radius values and tends to generate many false positives, but because it operates at a sub-character level it is capable of finding substructures that are not easily identifiable by distances that work at the character level. So it should be used as a 'last resort' clustering method; that's why it is listed last here despite its phenomenal efficacy in other realms.

It is also important to note that in practice similarity distances are more effective on longer strings than on shorter ones; this is mostly an artifact of the need for the statistical compressors to 'warm up' and gather enough statistics to start performing well.

# Cluster New Value Suggestions

For each cluster identified, one value is chosen as the initial 'New Cell Value' to use as the common value for all values in the cluster. The value chosen is the first value in the Cluster (see the ClusteringDialog.prototype.\_updateData function in /main/webapp/modules/core/scripts/dialogs/clustering-dialog.js)

The first value in the Cluster is determined by two steps:

- a) The order of the items in the Cluster as the Cluster is built
- b) The order of the items in the Cluster after sorting by the count of the occurrences of each value

(a) is achieved via a Collections.sort - which is [https:_docs.oracle.com/javase/7/docs/api/java/util/Collections.html#sort(java.util.List,%20java.util.Comparator)_](%22guaranteed+to+be+stable%3A+equal+elements+will+not+be+reordered+as+a+result+of+the+sort.%22) (b) is achieved by different methods depending on whether you are doing a Nearest Neighbour or Key Collisions (aka Binning) cluster

If you are using Key Collision/Binning then the Cluster is created using a TreeMap which by default [https:_docs.oracle.com/javase/7/docs/api/java/util/TreeMap.html]. The key is the string in the cell - so that means it will sort by natural ordering of the strings in the cluster - which means that it uses a [['lexicographical' order|http:_docs.oracle.com/javase/7/docs/api/java/lang/String.html#compareTo%28java.lang.String%29](%22is+sorted+according+to+the+natural+ordering+of+its+keys%22) - basically based on the Unicode values in the string

If you are using a Nearest Neighbour sort the Cluster is created in a different way which is (as yet) undocumented. Testing indicates that it may be something like reverse natural ordering.

# Final Notes

- follow the order of the methods as described in this page (which goes from more strict to more lax), this has proved to be the most effective workflow for us;
- we've been focusing mostly on English content or data ported to English, we know some of the methods might be biased towards it but we're willing to eliminate that bias or to introduce more methods once the OpenRefine community gathers more insights into these problems;
- for kNN distances, we found that blocking with less than 3 or 4 chars explodes the amount of time clustering takes and yields very few new valuable results, but your mileage may vary;
- OpenRefine's internals support a lot more methods but we have turned off many of them because they don't seem to have much practical advantage over the ones described here. But if you think that OpenRefine should use other methods, feel free to suggest them to us because we might have overlooked them.

# Suggested Reading

A lot of the code that OpenRefine uses for clustering originates from research done by the [http:_simile.mit.edu/_](SIMILE+Project) at MIT which later [http:code.google.com/p/simile-vicino/](graduated+as+the+Vicino+project) ('vicino', pronounced "vitch-ee-no", means 'near' in Italian).

For more information on clustering methods and related research we suggest you look at the [http:_code.google.com/p/simile-vicino/source/browse/#svn/trunk/papers_](bibliography+of+the+Vicino+project).

