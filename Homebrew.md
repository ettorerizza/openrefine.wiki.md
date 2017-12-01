_OpenRefine's Homebrew Cask_

## Introduction

[http:_brew.sh_](Homebrew) is a popular command-line package manager for macOS. Installing Homebrew is accomplished by pasting the installation command on the Homebrew website into a Terminal window. Once Homebrew is installed, applications like OpenRefine can be installed via a simple command:

## Usage

These instructions assume you've installed Homebrew. Instructions for installing Homebrew are prominently displayed on the project's homepage, [http://brew.sh](http://brew.sh).

### Install OpenRefine

Install OpenRefine with this command:

```
brew cask install openrefine
```

You should see output like this:

```
==> Downloading https://github.com/OpenRefine/OpenRefine/releases/download/2.7/openrefine-mac-2.7.dmg
  ######################################################################## 100.0%
  ==> Verifying checksum for Cask openrefine
  ==> Installing Cask openrefine
  ==> Moving App 'OpenRefine.app' to '/Applications/OpenRefine.app'.
  🍺 openrefine was successfully installed!
```

Behind the scenes, this command causes Homebrew to download the OpenRefine installer (displaying progress as the download completes), verify that the SHA-256 fingerprint of the download matches the value stored in the Homebrew project, mount the disk image, copy the OpenRefine.app application bundle into the Applications folder, unmount the disk image, and save a copy of the installer and metadata about the installation for future use.

If an existing OpenRefine.app is found in the Applications folder, Homebrew will not overwrite it, so installing via Homebrew requires either deleting or renaming previously installed copies.

### Uninstalling OpenRefine

To uninstall OpenRefine, paste this command into the Terminal:

```
brew cask uninstall openrefine
```

You should see output like this:

```
==> Removing App '/Applications/OpenRefine.app'.
```

### Updating OpenRefine

To update to the latest version of OpenRefine, paste this command into the Terminal:

```
brew cask reinstall openrefine
```

You should see output like this:

```
==> Downloading https://github.com/OpenRefine/OpenRefine/releases/download/2.7/openrefine-mac-2.7.dmg
  ######################################################################## 100.0%
  ==> Verifying checksum for Cask openrefine
  ==> Removing App '/Applications/OpenRefine.app'.
  ==> Moving App 'OpenRefine.app' to '/Applications/OpenRefine.app'.
  🍺 openrefine was successfully installed!
```

If you had previously installed the "openrefine-dev" cask (containing a release candidate) and you want to move to the stable release, you need to first uninstall the old cask and then install the new one:

```
brew cask uninstall openrefine-dev
  brew cask install openrefine
```

### Maintaining OpenRefine's Homebrew Cask

OpenRefine's presence on Homebrew is found in the Caskroom project, as a "cask", at [https://github.com/caskroom/homebrew-cask/blob/master/Casks/openrefine.rb](https://github.com/caskroom/homebrew-cask/blob/master/Casks/openrefine.rb).

**Terminology:**"Caskroom" is the Homebrew extension project where pre-built binaries and GUI applications go, whereas the original, plain old "Homebrew" project is reserved for command-line utilities that can be built from source. So OpenRefine clearly belongs in Caskroom.

When there is a new development release of OpenRefine, a member of the community can submit a pull request with the necessary changes to the OpenRefine cask. [https:_github.com/caskroom/homebrew-cask/blob/master/CONTRIBUTING.md#updating-a-cask_](Follow+these+directions) - summarized here adapted to OpenRefine:

```
# install and setup script - only needed once
  brew install vitorgalvao/tiny-scripts/cask-repair
  cask-repair --help

  # use to update openrefine
  cask-repair openrefine
```

The cask-repair tool will prompt you to enter the new version number. It will then use this version number to construct a download URL using the formula (where <tt>{version}</tt> represents the version number):

```
https://github.com/OpenRefine/OpenRefine/releases/download/#{version}/openrefine-mac-#{version}.dmg
```

**Note:** It is important that both version number components (the tag and version number) match, so that the formula can find the installer's URL.

Once cask-repair has successfully downloaded the new installer, it will calculate the new SHA-256 fingerprint value and construct a pull request, like this one: [https://github.com/caskroom/homebrew-versions/pull/3319](https://github.com/caskroom/homebrew-versions/pull/3319).

#### Stable vs. development versions

As of the writing of this page, OpenRefine 2.7 is the current stable release version. If a release candidate cycle for a future version is started (e.g., 2.8-RC1)—particularly if the release cycle is projected to be long—we could consider submitting an "openrefine-dev" cask to the "caskroom/homebrew-versions" repository, which houses the "caskroom/versions" tap, reserved for betas and release candidates. This is in contrast to the "caskroom/homebrew-cask" repository, which houses stable releases. We used this approach during the long 2.6-era release candidate cycle.

Once a PR containing the release candidate is merged into caskroom/versions, users will need to add the `caskroom/versions` tap before they can install the release candidate cask. To add this tap, paste this command into the Terminal:

```
brew tap caskroom/versions
```

Users will see output like this:

```
==> Tapping caskroom/versions
  Cloning into '/usr/local/Homebrew/Library/Taps/caskroom/homebrew-versions'...
  remote: Counting objects: 195, done.
  remote: Compressing objects: 100% (193/193), done.
  remote: Total 195 (delta 22), reused 32 (delta 0), pack-reused 0
  Receiving objects: 100% (195/195), 74.13 KiB | 0 bytes/s, done.
  Resolving deltas: 100% (22/22), done.
  Tapped 0 formulae (213 files, 276.6KB)
}}

Then, users can install OpenRefine with this command:

{{{
  brew cask install openrefine-dev
```

Each subsequent release candidate can be updated with cask-repair. Once the final release version is out, maintainers should update the main "openrefine" cask and delete the "openrefine-dev" cask, until a new development cycle begins. We should not leave a stale release candidate version of an already-released app, nor should we update "openrefine-dev" to use the final release version (it would duplicate efforts).

