_Boolean functions supported by [General Refine Expression Language](OpenRefine+Expression+Language) (GREL)_

See also: [GREL functions](All+GREL+functions).

### and(boolean b1, boolean b2, ...etc)

Logically AND two or more booleans to yield a boolean. For example, <tt>and(1 &lt; 3, 1 &gt; 0)</tt> returns <tt>true</tt> because both conditions are true.

### or(boolean b1, boolean b2, ...etc)

Logically OR two or more booleans to yield a boolean. For example, <tt>or(1 &lt; 3, 1 &gt; 7)</tt> returns <tt>true</tt> because at least one of the conditions (the first one) is true.

### not(boolean b)

Logically NOT a boolean to yield another boolean. For example, <tt>not(1 &gt; 7)</tt> returns <tt>true</tt> because <tt>1 &gt; 7</tt> itself is false.

### xor(boolean b1, boolean b2, ...etc)

Logically XOR (exclusive-or) two or more booleans to yield a boolean. For example, <tt>xor(1 &lt; 3, 1 &gt; 7)</tt> returns <tt>true</tt> because only one of the conditions (the first one) is true. <tt>xor(1 &lt; 3, 1 &lt; 7)</tt> returns <tt>false</tt> because more than one of the conditions is true.

