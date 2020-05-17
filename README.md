# MunkiCatalogSelectivePromote
Promotes pkginfo files from development to testing to production after a certain number of days

## Rationale
If you have a lot of Munki items to move through a development-testing-production process, it can be cumbersome to go into each pkginfo and add a new catalog to the pkginfo, even if you're doing it through a GUI like MunkiAdmin. MunkiCatalogPromote allows you to bulk-promote all your pkginfo files after a certain number of days.

There are actually a bunch of catalog promotion scripts for Munki. Ed Ravin has compiled [a fairly comprehensive list on the Munki Dev mailing list.](https://groups.google.com/d/msg/munki-dev/w5fAMwzeMmM/s_-ri2nGAgAJ)

I wanted to create something simple for what I want, which is just to move every pkginfo through after a certain number of days. I also wanted to practice some more Python, so this script was a good excuse to do so. If this script doesn't do what you want to do, definitely check out any of the other automated options from Ed Ravin's list.

## How to use MunkiCatalogPromote
### General Instructions
- Download the MunkiCatalogPromote.py and MCPPrefs.plist file to your Munki server.
- Edit the MCPPrefs.plist, being sure to modify (if necessary) the user-defined preferences. You may want to make the days between promotions longer or shorter, or you may have only testing and production catalogs (no development). Definitely be sure check your MUNKI_ROOT_PATH to match that of your server's.
- Decide if you want to apply the promotion logic only to certain subdirectories within your pkgsinfo directory and/or you want to exclude certain pkginfo files. If so, see the [this section](#Include-Directories-or-Exclude-Pkgs)
- Move MunkiCatalogPromote.py and the MCPPrefs.plist to /usr/local/mcp and make it executable.
- The first time you run MunkiCatalogPromote, it won't seem to do anything, because it's just tagging each pkginfo with a promotion date of now, and then each item's actual promotion will happen after a certain number of days (which you define) the next time you run the script.
- If you find MunkiCatalogPromote working the way you like it, you can automate it using a launch agent.
- *Note*: this new version uses Munki's embedded Python 3, so you must be using at least Munki version 4. If you don't have that installed already, you should download [the latest Munki release](https://github.com/munki/munki/releases/latest) and install that first.

### Include Directories or Exclude Pkgs
If you only want to apply the promotion to a subset of folders within you pkgsinfo directory when you edit the MCPPrefs.plist file set:
```
	<key>use_included_subdirectories</key>
	<true/>
```
Then configure a list of strings that match your subdirectories exactly.  If, for example, you only want pkgs in mozilla, google, and microsoft subdirectories to be promoted; set:
```
<key>included_subdirectories</key>
	<array>
		<string>mozilla</string>
		<string>google</string>
		<string>microsoft</string>
	</array>
```
If you want to specifically EXCLUDE some pkginfo files, when you edit the MCPPrefs.plist file set:
```
	<key>use_excluded_pkgs</key>
	<true/>
```
Then configure a list of strings to match the start of the pkginfo filenames. For example:
```
	<key>excluded_pkgs</key>
	<array>
		<string>Picasa</string>
		<string>GoogleDrive-1.32</string>
		<string>MSAutoUpdate-4.17.19111002.plist</string>
	</array>
```


### Example of configuration/installation via command line
Edit the MCPPrefs.plist file
```
nano ~/Downloads/MunkiCatalogPromote-master/MCPPrefs.plist
```

Create the directory
```
sudo mkdir /usr/local/mcp/
```

Move the files
```
sudo mv ~/Downloads/MunkiCatalogPromote-master/* /usr/local/mcp/
```

Change permissions and ownership
```
sudo chown -R root:wheel /usr/local/mcp
sudo chmod -R 755 /usr/local/mcp
```

## Acknowledgements
Forked from Aysiu's [AutoPromoter] (https://github.com/aysiu/MunkiCatalogPromote)
Some of the code is lifted/tweaked from [autopromoter](https://github.com/jessepeterson/autopromoter), some from [outset](https://github.com/chilcote/outset/), and some from [munki](https://github.com/munki/munki).
