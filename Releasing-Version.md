_Releasing a new version of Refine_

# Introduction

When releasing a new version of Refine, the following steps should be followed:

1. Make sure the `master` branch is stable and nothing has broken since the previous version. We need developers to stabilize the trunk and some volunteers to try out `master` for a few days.
2. Change the version number in [http:_github.com/OpenRefine/OpenRefine/blob/master/main/src/com/google/refine/RefineServlet.java#L62_](RefineServlet.java).
3. Compose the list of changes in the code and on the wiki. If the issues have been updated with the appropriate milestone, the Github issue tracker should be able to provide a good starting point for this.
4. Tag the release candidate in git and push the tag to Github
```
git tag -a -m "Second beta" 2.6-beta.2
    git push origin --tags
```

1. Set up build machine. This needs to be Mac OS X, but can be run in a VM. The machine also needs a code signing certificate with the name "OpenRefine Code Signing" installed, but this can be a self-signed certificate. It can be created using the OS X Keychain Access utility.
2. Build the release candidate kits using the shell script (not just Ant). This must be done on Mac OS X to be able to build all 3 kits, although Linux can be used for Linux & Windows builds:
```
./refine dist 2.6-beta.2
```

1. Upload the kits to Github releases [https://github.com/OpenRefine/OpenRefine/releases/](https://github.com/OpenRefine/OpenRefine/releases/) If running OS X in a VM, it's probably quicker and more reliable to transfer the kits to the host machine first and then to Github. Finder -> Go -> Connect -> smb:_10.0.2.2_
2. Announce the beta/release candidate for testing
3. Repeat build/release candidate/testing cycle, if necessary.
4. Tag the release in git. Build the distributions and upload them. 
5. Update the [http:_code.google.com/p/google-refine/source/browse/support/releases.js_](releases.js) file. This triggers the prompt to users to update their installations, so it should be the final step.
6. Verify that the correct versions are shown in the widget at [http://openrefine.org/download](http://openrefine.org/download)

* * *

**NOTE** : During the transition to Github, the releases.js file on Google Code needs to be updated because that's the one that existing installations are looking at. It also needs a fake revision number which is higher than the 2.5 revision to trigger the update. Post 2.6 only Github needs to be updated.

* * *

1. Announce on the mailing list.
