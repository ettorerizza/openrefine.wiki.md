_How to back up all Google Refine / OpenRefine's data_

You may wish a quick way to quickly backup all your projects, particularly if you are upgrading to a new version of OpenRefine. It is possible to backup each project individually by opening the project, then going to the <tt>export</tt> dropdown menu and selecting <tt>export project</tt>. However, there is a faster way to back up all your data.

This can be done by manually **making a copy** of the Refine workspace directory. **Do not move** that directory, just copy. The location of the OpenRefine workspace directory varies between operating systems, and these are listed below:

- MacOSX: {{{/Library/Application Support/OpenRefine }}}
- Windows: depending on the Windows version, the data is in one of these directories
- <tt>C:\Documents and Settings\(user id)\Local Settings\Application Data\OpenRefine</tt>
- <tt>C:\Users\(user id)\AppData\Roaming\OpenRefine</tt>
- <tt>C:\Users\(user id)\AppData\Local\OpenRefine</tt>
- <tt>C:\Users\(user id)\OpenRefine</tt>
- Linux: <tt>~/.local/share/openrefine/</tt> (the folder local is hidden, you can view it using the command <tt>ls -al</tt>)

You can quickly get to this directory from the OpenRefine start page by clicking on the <tt>Browse workspace directory</tt> link at the bottom left of the page.

