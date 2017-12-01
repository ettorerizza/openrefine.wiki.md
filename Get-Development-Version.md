_How to get the super latest development version._

If you're a user who just wants the latest features even if they have not been released yet, you can get the development "trunk" version. Beware that by its very nature that version can be unstable. Use it at your own risk.

# Precautions

The trunk version supposedly offers certain features that you want. You probably want those features only on some particular existing projects. We recommend that you export those projects (as .tar.gz files) using your current released OpenRefine version, and then re-import them when you run the trunk version. Then work on that new project copy using the trunk version. You can always shutdown the trunk version and start up the released version and work with the old project. Or if things go really bad, you can scrap everything and re-import the saved .tar.gz files.

# What You Need

To get the trunk version, you first need [http:_git-scm.com/_](Git). And since Open Refine is hosted on GitHub, you may instead want to download the [https:help.github.com/articles/set-up-git](GitHub+GUI+client) if you prefer to use a graphical interface.

You will also need [http:_ant.apache.org/_](Apache+Ant) to build the source code. Unzip, and point your path to it. Or you can try [http:wiki.apache.org/ant/AntOnWindows](the+Windows+installer), too.

On Windows, after installing Git and Ant, you may need to logoff, then logon for your user environment variables to take effect.

# Checking Out the Source

Open a Terminal window. On Windows, this is done by

- invoking the Start menu
- picking "Run ..."
- typing in: <tt>command</tt>
- pressing Enter

On MacOSX, use Spotlight to find "Terminal".

In the Terminal window, change the directory to wherever you want to put the OpenRefine source code. If you don't know what that means, you don't have to do anything.

Then in the Terminal window, type

```
git clone https://github.com/OpenRefine/OpenRefine.git
```

You will see a lot of lines scroll past as the source is getting downloaded.

# Building the Source

Change into the <tt>OpenRefine</tt> directory by typing into the Terminal window

```
cd OpenRefine
```

Then build the source code:

- on Windows, type: <tt>refine build</tt>
- on MacOSX or <tt>**</tt>nix, type: <tt>./refine build</tt>

# Running

To run, in the Terminal window

- on Windows, type: <tt>refine</tt>
- on MacOSX or <tt>**</tt>nix, type: <tt>./refine</tt>

# Shutting Down

To shut down OpenRefine, in the same Terminal window, press the key combination Ctrl-C.

You can shut down OpenRefine at any time using Ctrl-C and it will save your data and then will automatically lose the terminal. In an emergency, You can manually close the Terminal window in case of a stuck process (NOTE: this could cause a loss of data if OpenRefine has not auto saved yet). To re-start OpenRefine, open a new Terminal window if you already closed the old one, change to the directory where you put OpenRefine, and just following the instruction to run in the previous section.

# Updating

The trunk version will get updated as we work on it. To get the latest updates, shut down OpenRefine if it's running. Then in a Terminal window, already changed to the <tt>OpenRefine</tt> directory, type

```
git pull
```

Then follow the instructions to build and then to run.

Note: if you run into issues, the first thing to do is to clean your local copy with this command

```
./refine clean
```

and then build again.

