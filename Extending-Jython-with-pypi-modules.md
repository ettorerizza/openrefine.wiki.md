### Tutorial - How to extend OpenRefine with pypi modules

OpenRefine comes with Jython/Python expression support built-in. Great!

BUT... sometimes you want to easily use Pip and download python modules/libraries to use in your OpenRefine project without a lot of headache.

Quick Answer: Download and install Jython 2.7+ (install 'standard') to manage and run <tt>pip</tt> itself outside of OpenRefine, then do cool <tt>sys.path.append</tt>'s

- Download Jython 2.7x and install using 'standard' option (setuptools and pip will be installed)
- Add the <tt>jython2.7x\bin</tt> folder to your path
- Because of a current bug with Jython installer ( [http://bugs.jython.org/issue2521](http://bugs.jython.org/issue2521)), if on Windows, you might have to rename <tt>TEMP\pip_build_blah</tt> to <tt>TEMP\XXX_pip_build_blah</tt> 
- Open a Windows Command Prompt cmd.exe and install modules that you want to be able to use in OpenRefine.
```
C:\Users\Thad>pip install address-formatter
```

- **NOTE :** if you already have a Python version installed, <tt>pip install &lt;module&gt;</tt> will install the module in your first Python distribution, not in Jython. To work around the problem, use this command instead : 
```
C:\Users\Thad>jython -m pip install address-formatter
```

- Start OpenRefine
- Use the Expression dialog and append your path to include Jython 2.7 site-packages folder
```
import sys
  sys.path.append('E:\\jython2.7.0\\Lib\\site-packages')

  #or, if you don't want to escape each backslash in the folder path, use :
  #sys.path.append(r'E:\jython2.7.0\Lib\site-packages')
```

- Then import the package and/or .py files you want to work with
```
from address import AddressParser, Address
```

- Putting it all together with a usecase
```
import sys
sys.path.append('E:\\jython2.7.0\\Lib\\site-packages')
from address import AddressParser, Address

ap = AddressParser()

return ap.parse_address(value)
```

- How to replace diacritic characters
```
import sys
sys.path.append(r'E:\jython2.7.1rc1\Lib\site-packages')
from unidecode import unidecode
return unidecode(value)

# Boris Villazón -> Boris Villazon
```
