_Notes on upgrading from Freebase Gridwoks to Google Refine_

**NOTE** : This page is about upgrading from Freebase Gridworks 1.1 to Google Refine 2.0 or 2.1, so is primarily of historical interest.

If you have been using Freebase Gridworks 1.1 or earlier and you are now going to download and install Google Refine 2.1, you might want to take some precaution to protect your existing Gridworks projects. We have tried to ensure that the upgrade goes smoothly, but it's possible that we have not covered all cases. If you value your current Gridworks projects, then it is safest to back up your existing projects first.

## Backward Compatibility Issues

These are the backward compatibility issues that we know about:

- All Freebase reconciliation and loading functionalities have been substantially redesigned and reimplemented, and are thus not backward compatible. The redesign is to make Refine compatible with the [http:_wiki.freebase.com/wiki/Refinery_](Refinery+QA+conduit). This means that if you have an old project already reconciled to Freebase, you will not be able to load it.
- Freebase-related operations from old projects can be undone, but cannot be extracted.

## Precautions

Freebase Gridworks 1.1 or earlier store its projects as sub-directories of a "workspace" directory. Where the workspace directory is depends on your OS:

- MacOSX: <tt>~/Library/Application Support/Gridworks/</tt>
- Windows: depending on the Windows version, the data is in one of these directories
- <tt>C:\Documents and Settings\(user id)\Local Settings\Application Data\Gridworks</tt>
- <tt>C:\Users\(user id)\AppData\Roaming\Gridworks</tt>
- <tt>C:\Users\(user id)\Gridworks</tt>
- Linux: <tt>~/.local/share/gridworks/</tt>

**Make a copy** of that workspace directory. **Don't move** that directory.

After you install Google Refine 2.1 and run it for the first time, it will attempt to rename that workspace directory

- MacOSX: <tt>~/Library/Application Support/Google/Refine/</tt>
- Windows: depending on the Windows version, the data is in one of these directories
- <tt>C:\Documents and Settings\(user id)\Local Settings\Application Data\Google\Refine</tt>
- <tt>C:\Users\(user id)\AppData\Roaming\Google\Refine</tt>
- <tt>C:\Users\(user id)\Google\Refine</tt>
- Linux: <tt>~/.local/share/google/refine/</tt>

If all goes well, the existing projects will be available in Google Refine 2.1.

If the directory renaming is successful but some particular projects are broken, you can move that copy you have made of the old directory back to exactly where it was before, and then shut down Google Refine 2.1 and start up Freebase Gridworks 1.1 to access those projects. Because there is already a Refine workspace directory, the next time Google Refine is run, it will **not** attempt to rename the old Gridworks workspace directory. Thus, you can use both Freebase Gridworks 1.1 and Google Refine 2.1 on the same computer. But note that they will not share their projects.

When Google Refine 2.1 is first launched, the browser may continue to show the Gridworks page. This is due to your browser caching the old page. Hold down the [http:_lifehacker.com/5574852/shift%252Brefresh-is-like-the-restart-button-for-web-sites_](shift+key+while+pressing+the+Reload%2FRefresh+button) in your browser to clear the cache, and load the correct Refine page.

