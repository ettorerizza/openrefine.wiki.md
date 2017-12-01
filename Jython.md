### Using Jython as your Expression Language

#### Tutorials

- [Jython#tutorial---working-with-phone-numbers-using-java-libraries-inside-python](Using+Jython+with+Java+libraries)
- [Extending-Jython-with-pypi-modules](PIP+-+Extending+Jython+with+pypi+modules)
- [StrippingHTML](Using+Jython+and+BeautifulSoup+to+handle+Entity+Extraction+and+HTML+markup+removal)

Full docs on the Jython language are at its official site [http:_www.jython.org_](http://www.jython.org).

You can use almost any Python <tt>(.py)</tt><tt>(.pyc)</tt> files compatible with the bundled Jython 2.5.1 and drop them into the path. Since Jython is essentially Java, you can even [Jython#tutorial---working-with-phone-numbers-using-java-libraries-inside-python](import+Java+libraries+and+utilize+those%21)

**NOTE: Python libraries or code that uses C bindings will not work in OpenRefine which uses Jython / Java only, and has no CPython interpreter built-in**

**NOTE: OpenRefine now has most of the Jsoup.org library built in for parsing and working with HTML elements and extraction - [GREL Other Functions](Built-in+GREL+Jsoup+functions)**

**NOTE: Remember to restart OpenRefine, so that new Jython/Python libraries are initialized during Butterfly's startup.**

* * *

Expressions in Jython must have a <tt>return</tt> statement:

```
return value[1:-1]
```
```
return rowIndex%2
```

Fields have to be accessed using the bracket operator rather than the dot operator:

```
return cells["col1"]["value"]
```

To access the Levenshtein distance between the reconciled value and the cell value (?) use the Recon [Variables](variable):

```
return cell["recon"]["features"]["nameLevenshtein"]
```

To return the lower case of value (if the value is not null):

```
if value is not None:
    return value.lower()
  else:
    return None
```

### Tutorial - Working with Phone numbers using Java libraries inside Python

As mentioned before, you can drop in JAVA .jar files into the classpath and utilize them from OpenRefine with Python/Jython as your expression language.

1. Download the [http:_repo1.maven.org/maven2/com/googlecode/libphonenumber/libphonenumber/8.3.1/libphonenumber-8.3.1.jar_](libphonenumber+8.3.1+.jar+file+from+Maven+Central) (repo and docs here: [https:github.com/googlei18n/libphonenumber](Google+libphonenumber+on+Github))
2. Copy the .jar file into your <tt>openrefine-2.xx/webapp/WEB-INF/lib</tt> folder ![libphonenumber_jar](https://cloud.githubusercontent.com/assets/986438/23411643/859b7360-fd98-11e6-8dfb-42c25c328650.PNG)
3. Start OpenRefine
4. Choose **Edit column** -> **Add column based on this column...**
5. Give it a new column name such as VALID\_FORMATTED\_NUMBER
6. Choose **Python/Jython** as your Expression Language.
7. Copy and paste the following code into the Expression input box:
```
from com.google.i18n.phonenumbers import PhoneNumberUtil
   from com.google.i18n.phonenumbers.PhoneNumberUtil import PhoneNumberFormat

   phoneUtil = PhoneNumberUtil.getInstance()
   number = phoneUtil.parse(value, 'US')
   formatted = phoneUtil.format(number, PhoneNumberFormat.NATIONAL)
   valid = phoneUtil.isValidNumber(number)

   if valid == 1:
     return formatted
```

- You can now see in the Preview a nicely formatted number if your original string in your column was a valid US number. ![working with Jython and Java Libraries in OpenRefine](https://cloud.githubusercontent.com/assets/986438/23411647/884f0b62-fd98-11e6-8eb2-b089bb9020b3.PNG)
- OPTIONAL - [http:_repo1.maven.org/maven2/com/googlecode/libphonenumber/libphonenumber/8.3.1/libphonenumber-8.3.1-javadoc.jar_](Download+the+Javadoc+.jar), unzip and extract it, then open the index.html file and read and experiment with other methods and classes in Google's libphonenumber library.
- OPTIONAL - If you want a list that you can later split into record rows, you can do something like this.
```
from com.google.i18n.phonenumbers import PhoneNumberUtil
   from com.google.i18n.phonenumbers.PhoneNumberUtil import PhoneNumberFormat

   phoneUtil = PhoneNumberUtil.getInstance()
   number = phoneUtil.parse(value, 'US')
   formatted = phoneUtil.format(number, PhoneNumberFormat.INTERNATIONAL)
   valid = phoneUtil.isValidNumber(number)
   output = valid, formatted, number

   return '|||'.join(map(str,output))
```
