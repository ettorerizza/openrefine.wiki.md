<<<<<<< HEAD
_Array functions supported by the [General Refine Expression Language](OpenRefine+Expression+Language) (GREL)_

See also: [GREL Functions](All+GREL+functions).

### length(array a)

Returns the length of array <tt>a</tt>.

### slice(array a, number from, optional number to)

Returns the sub-array of <tt>a</tt> from its index <tt>from</tt> up to but not including index <tt>to</tt>. If <tt>to</tt> is omitted, it is understood to be the end of the array <tt>a</tt>. For example, <tt>slice([0, 1, 2, 3, 4], 1, 3)</tt> returns <tt>[1, 2]</tt>, and <tt>slice([0, 1, 2, 3, 4], 1)</tt> returns <tt>[1, 2, 3, 4]</tt>.

### get(array a, number from, optional number to)

See <tt>slice</tt> function above.

### reverse(array a)

Reverses array <tt>a</tt>. For example, <tt>reverse([0, 1, 2, 3])</tt> returns the array <tt>[3, 2, 1, 0]</tt>.

### sort(array a)

Sorts array <tt>a</tt> in ascending order. For example, <tt>sort([2, 1, 0, 3])</tt> returns the array <tt>[0, 1, 2, 3]</tt>.

### sum(array a)

Return the sum of the numbers in the array <tt>a</tt>. For example, <tt>sum([2, 1, 0, 3])</tt> returns <tt>6</tt>.

### join(array a, string sep)

Returns the string obtained by joining the array <tt>a</tt> with the separator <tt>sep</tt>. For example, <tt>join(["foo", "bar", "baz"], ";")</tt> returns the string <tt>foo;bar;baz</tt>.

### uniques(array a)

Returns array <tt>a</tt> with duplicates removed. For example, <tt>uniques([2, 1, 2, 0, 1, 4, 3])</tt> returns the array <tt>[0, 1, 2, 3, 4]</tt>.

=======
_Array functions supported by the [General Refine Expression Language](OpenRefine+Expression+Language) (GREL)_

See also: [GREL Functions](All+GREL+functions).

### length(array a)

Returns the length of array <tt>a</tt>.

### slice(array a, number from, optional number to)

Returns the sub-array of <tt>a</tt> from its index <tt>from</tt> up to but not including index <tt>to</tt>. If <tt>to</tt> is omitted, it is understood to be the end of the array <tt>a</tt>. For example, <tt>slice([0, 1, 2, 3, 4], 1, 3)</tt> returns <tt>[1, 2]</tt>, and <tt>slice([0, 1, 2, 3, 4], 1)</tt> returns <tt>[1, 2, 3, 4]</tt>.

### get(array a, number from, optional number to)

See <tt>slice</tt> function above.

### reverse(array a)

Reverses array <tt>a</tt>. For example, <tt>reverse([0, 1, 2, 3])</tt> returns the array <tt>[3, 2, 1, 0]</tt>.

### sort(array a)

Sorts array <tt>a</tt> in ascending order. For example, <tt>sort([2, 1, 0, 3])</tt> returns the array <tt>[0, 1, 2, 3]</tt>.

### sum(array a)

Return the sum of the numbers in the array <tt>a</tt>. For example, <tt>sum([2, 1, 0, 3])</tt> returns <tt>6</tt>.

### join(array a, string sep)

Returns the string obtained by joining the array <tt>a</tt> with the separator <tt>sep</tt>. For example, <tt>join(["foo", "bar", "baz"], ";")</tt> returns the string <tt>foo;bar;baz</tt>.

### uniques(array a)

Returns array <tt>a</tt> with duplicates removed. For example, <tt>uniques([2, 1, 2, 0, 1, 4, 3])</tt> returns the array <tt>[0, 1, 2, 3, 4]</tt>.

>>>>>>> e11eae1f90dee695ec75c7a3edc17ec3487c923e
