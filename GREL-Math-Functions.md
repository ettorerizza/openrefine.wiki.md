_Math functions supported by the [General Refine Expression Language](OpenRefine+Expression+Language) (GREL)_

See also: [GREL Functions](All+GREL+functions).

NOTE: For Integer division and precision you can use simple evaluations such as

<tt>1 / 2</tt>

which is equivalent to floor(1/2) i.e. it returns only whole number results. If either operand is a floating point number, they both get promoted to floating point and a floating point result is returned. You can use 1 / 2.0 or 1.0 / 2 or 1.0 \* x / y (if you're working with variables of unknown contents) to achieve the results you desire.

### floor(number d)

Returns the floor of a number. For example, <tt>floor(3.7)</tt> returns <tt>3</tt> and <tt>floor(-3.7)</tt> returns <tt>-4</tt>.

### ceil(number d)

Returns the ceiling of a number. For example, <tt>ceil(3.7)</tt> returns <tt>4</tt> and <tt>ceil(-3.7)</tt> returns <tt>-3</tt>.

### round(number d)

Rounds a number to the nearest integer. For example, <tt>round(3.7)</tt> returns <tt>4</tt> and <tt>round(-3.7)</tt> returns <tt>-4</tt>.

### min(number d1, number d2)

Returns the smaller of two numbers.

### max(number d1, number d2)

Returns the larger of two numbers.

### mod(number d1, number d2)

Returns <tt>d1</tt> modulus <tt>d2</tt>. For examples, <tt>mod(74, 9)</tt> returns <tt>2</tt>.

### ln(number d)

Returns the natural log of <tt>d</tt>.

### log(number d)

Returns the base 10 log of <tt>d</tt>.

### exp(number d)

Returns e raised to the power of <tt>d</tt>.

### pow(number d, number e)

Returns <tt>d</tt> raised to the power of <tt>e</tt>. For example, <tt>pow(2, 3)</tt> returns <tt>8</tt> (2 cubed) and <tt>pow(3, 2)</tt> returns <tt>9</tt> (3 squared).

The Square Root of any numeric value is expressed <tt>pow(value, 0.5)</tt>

### sum(array a)

Returns the sum of numbers in <tt>a</tt>.

