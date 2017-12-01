_Controls supported by the [General Refine Expression Language](OpenRefine+Expression+Language) (GREL)_

There are inline controls to support branching and essentially looping. They look like [GREL Functions](functions), but unlike functions, their arguments don't all get evaluated down to value before they get run. A control can decide which part of the code to execute and can affect the environment bindings. Functions, on the other hand, can't do either. Each control decides which of their arguments to evaluate to value, and how.

### if(expression o, expression eTrue, expression eFalse)

Expression <tt>o</tt> is evaluated to a value. If that value is true, then expression <tt>eTrue</tt> is evaluated and the result is the value of the whole <tt>if</tt> expression.

Examples:

| expression | result |
| --- | --- |
| <tt>if("internationalization".length() &gt; 10, "big string", "small string")</tt> | <tt>big string</tt> |
| <tt>if(mod(37, 2) == 0, "even", "odd")</tt> | <tt>odd</tt> |

### with(expression o, variable v, expression e)

Evaluates expression <tt>o</tt> and binds its value to variable name <tt>v</tt>. Then evaluates expression <tt>e</tt> and returns that result.

| expression | result |
| --- | --- |
| <tt>with("european union".split(" "), a, a.length())</tt> | <tt>2</tt> |
| <tt>with("european union".split(" "), a, forEach(a, v, v.length()))</tt> | <tt>[8, 5]</tt> |
| <tt>with("european union".split(" "), a, forEach(a, v, v.length()).sum() / a.length())</tt> | <tt>6.5</tt> |

### filter(expression a, variable v, expression test)

Evaluates expression <tt>a</tt> to an array. Then for each array element, binds its value to variable name <tt>v</tt>, evaluates expression <tt>test</tt> which should return a boolean. If the boolean is true, pushes <tt>v</tt> onto the result array.

| expression | result |
| --- | --- |
| <tt>filter([3, 4, 8, 7, 9], v, mod(v, 2) == 1)</tt> | <tt>[3, 7, 9]</tt> |

### forEach(expression a, variable v, expression e)

Evaluates expression <tt>a</tt> to an array. Then for each array element, binds its value to variable name <tt>v</tt>, evaluates expression <tt>e</tt>, and pushes the result onto the result array.

| expression | result |
| --- | --- |
| <tt>forEach([3, 4, 8, 7, 9], v, mod(v, 2))</tt> | <tt>[1, 0, 0, 1, 1]</tt> |

### forEachIndex(expression a, variable i, variable v, expression e)

Evaluates expression <tt>a</tt> to an array. Then for each array element, binds its index to variable <tt>i</tt> and its value to variable name <tt>v</tt>, evaluates expression <tt>e</tt>, and pushes the result onto the result array.

| expression | result |
| --- | --- |
| <tt>forEachIndex(["anne", "ben", "cindy"], i, v, (i + 1) + ". " + v).join(", ")</tt> | <tt>1. anne, 2. ben, 3. cindy</tt> |

### forRange(number from, number to, number step, variable v, expression e)

Iterates over the variable <tt>v</tt> starting at <tt>from</tt>, incrementing by <tt>step</tt> each time while less than <tt>to</tt>. At each iteration, evaluates expression <tt>e</tt>, and pushes the result onto the result array.

### forNonBlank(expression o, variable v, expression eNonBlank, expression eBlank)

Evaluates expression <tt>o</tt>. If it is non-blank, binds its value to variable name <tt>v</tt>, evaluates expression <tt>eNonBlank</tt> and returns the result. Otherwise (if <tt>o</tt> evaluates to blank), evaluates expression <tt>eBlank</tt> and returns that result instead.

Unlike other GREL functions beginning with 'for', forNonBlank is **not** an iterative function. forNonBlank essentially offers a shorter syntax to achieving the same outcome by using the 'isNonBlank' function within an 'if' statement.

### isBlank, isNonBlank, isNull, isNotNull, isNumeric, isError

<tt>isX(e)</tt> : evaluates expression <tt>e</tt> and returns whether its value is <tt>X</tt>.

Examples:

| expression | result |
| --- | --- |
| <tt>isBlank("abc")</tt> | <tt>false</tt> |
| <tt>isNonBlank("abc")</tt> | <tt>true</tt> |
| <tt>isNull("abc")</tt> | <tt>false</tt> |
| <tt>isNumeric(2)</tt> | <tt>true</tt> |
| <tt>isError(1)</tt> | <tt>false</tt> |
| <tt>isError("abc")</tt> | <tt>false</tt> |
| <tt>isError(1 / 0)</tt> | <tt>true</tt> |

Remember that these are controls and not functions. So you can **not** use the <tt>e.isX()</tt> syntax.

