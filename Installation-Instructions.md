_How to install and run Refine_

OpenRefine is a desktop application in that you download it, install it, and run it on your own computer. However, unlike most other desktop applications, it runs as a small web server on your own computer and you point your web browser at that web server in order to use Refine. So, think of Refine as a personal and private web application.

## Requirements

1. **Java JRE installed** (If your running a 64 bit operating system, then you'll need to install Java 64 bit JRE or JDK)

2. **A Supported OS** : Windows, Linux, OSX

NOTE: On Windows we do NOT support Cygwin, MSYS2, or Git Bash for running OpenRefine, instead just use Windows Terminal

## Release Version

1. OpenRefine requires you to have a working Java JRE, otherwise you will not be able to start OpenRefine. (the commmand window will just open and close quickly after you double click on OpenRefine.exe)
2. [https:_github.com/OpenRefine/OpenRefine/releases_](Download+OpenRefine+here).
3. Install it as detailed below for your operating system ( [Installation-Instructions#windows](Windows), [Installation-Instructions#macos](macOS), [Installation-Instructions#linux](Linux)).
4. As long as OpenRefine is running, you can point your browser at [http://127.0.0.1:3333/](http://127.0.0.1:3333/) to use it, and you can even use it in several browser tabs and windows.
5. If you're running a proxy or get a BindException, you can change the IP configuration with -i and -p, see **Running & Configuration** below, or use refine -help for options.

## Development Version

The development version hasn't had the same level of testing that the release version has, but we generally try to keep it in good shape. It will have bug fixes which are not yet available in the release version, but may also have new bugs as well.

- [Get Development Version](Step-by-step+to+get+%26+build+the+development+version).

### Install the Release Version on

### Windows

**Install:** Once you have downloaded the .zip file, uncompress it into a folder wherever you want (such as in <tt>C:\Open-Refine</tt>).

**Run:** Run the <tt>.exe</tt> file in that folder. You should see the Command window in which OpenRefine runs. By default, the Command window has a black background and text in monospace font in it.

**Shut down:** When you need to shut down OpenRefine, switch to that Command window, and press <tt>Ctrl-C</tt>. Wait until there's a message that says the shutdown is complete. That window might close automatically, or you can close it yourself. If you get asked, "Terminate all batch processes? Y/N", just press Y.

**Upgrading:** If you upgrade to a new version of OpenRefine, you may need to update your workspace (update reconciliation links, for example). Remove your workspace.json file located [https:_github.com/OpenRefine/OpenRefine/wiki/FAQ:-Where-Is-Data-Stored%3F_](at+a+data+storage+location). This file will be regenerated when OpenRefine starts.

### macOS

**Install via Disk Image:** Once you have downloaded the .dmg file, open it, and drag the OpenRefine icon into the Applications folder icon (just like you would normally install Mac applications).

**Install via Homebrew:** Follow our detailed [Homebrew](Homebrew+installation+guide), or follow quick steps below:

1. Install [http:_brew.sh_](Homebrew+from+here)

2. In Terminal enter

```
brew cask install openrefine
```

3. Then find OpenRefine in your Applications folder.

**Run:** To launch OpenRefine, go to the Applications folder and double click the OpenRefine app. You'll see the OpenRefine app appear in your dock.

**Shut down:** You can switch to the OpenRefine app (clicking on its icon in the dock) and invoke its Quit command.

**See also** [https:_github.com/OpenRefine/OpenRefine/issues/590_](Cannot+install+on+Mac+OS+X+10.8+%28Mountain+Lion%29+-+%22Google+Refine%22+is+damaged+and+can%27t+be+opened.+You+should+move+it+to+the+Trash.)

If you use Yosemite you will need to install [http:_support.apple.com/kb/DL1572_](Java+for+OS+X+2014-001) first.

### Linux

**Install / Run:** Once you have downloaded the <tt>tar.gz</tt> file, open a shell and type

```
tar xzf openrefine-linux-2.7.tar.gz
  cd openrefine-2.7
  ./refine
```

This will start OpenRefine and open your browser to its starting page.

**Shut down:** Press <tt>Ctrl-C</tt> in the shell.

### Running & Configuration

* * *

By default (and for security reasons) Refine only listens to TCP requests coming from localhost (127.0.0.1 on port 3333). If you want to respond to TCP requests coming to any IP address the machine has, run refine like this from the command line:

```
./refine -i 0.0.0.0
```

On Mac OS X, you can add a specific entry to the Info.plist file located within the app bundle (<tt>/Applications/OpenRefine.app/Contents/Info.plist</tt>):

```
<string>-Drefine.host=0.0.0.0</string>
```
