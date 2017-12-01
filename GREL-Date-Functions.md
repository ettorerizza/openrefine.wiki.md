_Date functions supported by the [General Refine Expression Language](OpenRefine+Expression+Language) (GREL)_

See also: [GREL Functions](All+GREL+functions).

### now()

Returns the current time.

### toDate(o, boolean month\_first / format1, format2, ... )

Returns <tt>o</tt> converted to a date object.

All other arguments are optional:

- <tt>month_first</tt>: set false if the date is formatted with the day before the month.
- <tt>formatN</tt>: attempt to parse the date using an ordered list of possible formats. See [https:_docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html_](SimpleDateFormat) for the syntax.

Examples: You can parse the cells "Nov-09" and "11/09" using

```
value.toDate('MM/yy','MMM-yy').toString('yyyy-MM')
```

For a date of the form: "1/4/2012 13:30:00" use GREL function:

```
value.toDate('d/M/y H:m:s')
```

Here is the list of pattern letters:

| Letter | Date or Time Component | Presentation | Examples |
| --- | --- | --- | --- |
| G | Era designator | Text | AD |
| y | Year | Year | 1996; 96 |
| Y | Week year | Year | 2009; 09 |
| M | Month in year | Month | July; Jul; 07 |
| w | Week in year | Number | 27 |
| W | Week in month | Number | 2 |
| D | Day in year | Number | 189 |
| d | Day in month | Number | 10 |
| F | Day of week in month | Number | 2 |
| E | Day name in week | Text | Tuesday; Tue |
| u | Day number of week (1 = Monday, ..., 7 = Sunday) | Number | 1 |
| a | Am/pm marker | Text | PM |
| H | Hour in day (0-23) | Number | 0 |
| k | Hour in day (1-24) | Number | 24 |
| K | Hour in am/pm (0-11) | Number | 0 |
| h | Hour in am/pm (1-12) | Number | 12 |
| m | Minute in hour | Number | 30 |
| s | Second in minute | Number | 55 |
| S | Millisecond | Number | 978 |
| z | Time zone | General time zone | Pacific Standard Time; PST; GMT-08:00 |
| Z | Time zone | RFC 822 time zone | -0800 |
| X | Time zone | ISO 8601 time zone | -08; -0800; -08:00 |

### toString(o, optional string format)

When <tt>o</tt> is a date, format specifies how to format the date.

### diff(date d1, date d2, string timeUnit)

For dates, returns the difference in given time unit (eg. "minutes", "hours", "days", "weeks", "months", "years").

Example:

<tt>diff(cells['date1'].value, cells['date2'].value, "days")</tt>

In case the value is negative you should invert date1 and date2 or multiply by -1.

### inc(date d, number value, string unit)

Returns a date changed by the given amount in the given unit of time. Unit defaults to 'hour'.

For example, if you want to decrement a date by 2 months:

```
value.inc(-2,'month')
```

### datePart(date d, string unit)

Returns part of a date. Data type returned depends on the unit. Units supported are:

| Unit | Date part returned | Returned data type | Example using [date 2014-03-14T05:30:04Z] as value |
| --- | --- | --- | --- |
| years | Year | Number | value.datePart("years") -> 2014 |
| year | Year | Number | value.datePart("year") -> 2014 |
| months | Month | Number | value.datePart("months") -> 2 |
| month | Month | Number | value.datePart("month") -> 2 |
| weeks | Week (of the year) | Number | value.datePart("weeks") -> 2 |
| week | Week (of the year) | Number | value.datePart("week") -> 3 |
| w | Week (of the year) | Number | value.datePart("w") -> 3 |
| weekday | Day of the week | String | value.datePart("weekday") -> Friday |
| hours | Hour | Number | value.datePart("hours") -> 5 |
| hour | Hour | Number | value.datePart("hour") -> 5 |
| h | Hour | Number | value.datePart("h") -> 5 |
| minutes | Minute | Number | value.datePart("minutes") -> 30 |
| minute | Minute | Number | value.datePart("minute") -> 30 |
| min | Minute | Number | value.datePart("min") -> 30 |
| seconds | Seconds | Number | value.datePart("seconds") -> 04 |
| sec | Seconds | Number | value.datePart("sec") -> 04 |
| s | Seconds | Number | value.datePart("s") -> 04 |
| milliseconds | Millseconds | Number | value.datePart("milliseconds") -> 0 |
| ms | Millseconds | Number | value.datePart("ms") -> 0 |
| S | Millseconds | Number | value.datePart("S") -> 0 |
| time | Date expressed as milliseconds since the Unix Epoch | Number | value.datePart("time") -> 1394775004000 |

