---
layout: page
title: Frequently Asked Questions
menu: FAQ
weight: 2
permalink: /faq
---

## Can I install extensions or themes from the Chrome Webstore?

Yes, but not via the Chrome Webstore interface. Instead, the URL used by the Webstore to download CRX files (Chrome/Chromium extension packages, used by all extensions in the Chrome Webstore) can be used.

If you want a comprehensive solution that handles **installation and updates**, see the [chromium-web-store](https://github.com/NeverDecaf/chromium-web-store) extension.

The following sections describe a method using built-in functionality.

### Downloading the CRX file

CRX files are downloaded using the following template CRX URL:

    https://clients2.google.com/service/update2/crx?response=redirect&acceptformat=crx2,crx3&prodversion=[VERSION]&x=id%3D[EXTENSION_ID]%26installsource%3Dondemand%26uc

Where:

* `[EXTENSION_ID]` is the extension ID from the Chrome Webstore. This can be retrieved from the Chrome Webstore URL for that extension, which has the form `https://chrome.google.com/webstore/detail/[...]/[EXTENSION_ID]`
* `[VERSION]` is the Chromium browser version.

For example, `cjpalhdlnbpafiamejdnhcphjbkeiagm` is the extension id of uBlock Origin, and `69.0` is for the 69.0.x.x browser versions.

This URL can be accessed directly by CLI utilities like `curl` and `wget`, but it can also be accessed in two other ways:

* Custom search engine: Create a new entry in `chrome://settings/searchEngines`, using the template CRX URL as the search URL above after replacing `[EXTENSION_ID]` with `%s`. Then, set `chrome://flags/#extension-mime-request-handling` to `Download as regular file`.
* [Bookmarklet](https://en.wikipedia.org/wiki/Bookmarklet) (proposed in [Issue #869](https://github.com/Eloston/ungoogled-chromium/issues/869)):
    * Go to [chrome://bookmarks/](chrome://bookmarks/)
    * Right click anywhere to select 'Add new Bookmark'. Copy the following into the URL field:

	```javascript
	javascript:location.href='https://clients2.google.com/service/update2/crx?response=redirect&acceptformat=crx2,crx3&prodversion='+(navigator.appVersion.match(/Chrome\/(\S+)/)[1])+'&x=id%'+'3D'+(document.querySelector('a[href^="https://chrome.google.com/webstore/report/"]').pathname.match(/[^\/]+\/*$/)[0])+'%'+'26installsource%'+'3Dondemand%'+'26uc';
	```

	Then, go to the extension page in the Chrome Web Store and click on the bookmark.

### Installing the CRX file

There are several methods to install CRX file:

1. **Always install extension MIME type requests**

    Change the flag `chrome://flags/#extension-mime-request-handling` to `Always prompt for install`. Then when using the CRX URL from the omnibox, the custom search engine, or the Bookmarklet, the browser will prompt for installation.

2. **The `file://` URI scheme**

    Launch ungoogled-chromium with the path to the CRX file as a command-line argument (this creates and navigates to a `file://` URL automatically). Invoking an "open with" command (or equivalent name) on the CRX file should have the same effect.

    Alternatively, navigate to the URL `file://PATH_TO_CRX` in the Omnibox, where `PATH_TO_CRX` is the *absolute path* to the CRX file using *forward slashes*. On Windows, you will need to add the drive letter. For examples and more details, see [file URI scheme on Wikipedia](https://en.wikipedia.org/wiki/File_URI_scheme).

3. **Drag and drop**

    **NOTE**: There are certain circumstances where this method fails on KDE Plasma.

    **NOTE for Chromium 67 and newer**: If the Material Design page is used (which has been default before 67), "Developer mode" of `chrome://extensions/` (a switch at the top right corner) must be enabled for drag and drop to function. (Discovered in [#423](https://github.com/Eloston/ungoogled-chromium/issues/423))

    Steps:

    1. Have the CRX downloaded to your file system
    2. Open `chrome://extensions`.  Refresh if you just enabled Developer Mode.
    3. Drag-and-drop the CRX from a file browser into the page of the extensions tab. While dragging over the page, it should state to drop the file to install.

4. **External Extension Descriptor (Linux systems only)**

    This example assumes the CRX is downloaded as `/home/share/extension_1_0_0.crx`. Modify the path as necessary.

    To install an extension with ID `aaaaaaaaaabbbbbbbbbbcccccccccc`, create the file

    `/usr/share/chromium/extensions/aaaaaaaaaabbbbbbbbbbcccccccccc.json`

    with following content:
    ```json
    {
        "external_crx": "/home/share/extension_1_0_0.crx",
        "external_version": "1.0.0"
    }
    ```
    After restarting the browser, the extension should be loaded automatically.

*This FAQ answer was adapted and extended from [Inox browser](https://raw.githubusercontent.com/gcarq/inox-patchset/master/README.md).*

## Will extensions auto-update?

You can use the [chromium-web-store](https://github.com/NeverDecaf/chromium-web-store) extension. It also allows updates from non-Chrome Web Store sources (based on the proposal in [Issue #285](https://github.com/Eloston/ungoogled-chromium/issues/285)).

There is currently no built-in functionality for auto-updates. [Issue #285](https://github.com/Eloston/ungoogled-chromium/issues/285) proposes a solution.

## Do plugins work?

Yes. All plugins including PepperFlash and Widevine DRM should work. See the relevant question for specific installation instructions.

## Why are there URLs with the `qjz9zk` domain in them? Why use domain substitution?

`qjz9zk` is the common top-level domain name used by domain substitution. It is a relatively trivial way of disabling unwanted requests and notifying the user if any of these URLs attempt to connect without having to look through the many changes that happen to Chromium each version.

## Does domain substitution break the HSTS preload list?

No, the list (which is located in `net/http/transport_security_state_static.json`) is explicitely excluded when generating the domain substitution list. In `developer_utilities/update_helper.py`, see the  `generate_domain_substitution_list()` function for what files are excluded from domain substitution.

## Why is Safe Browsing disabled?

See [this Wikipedia article](//en.wikipedia.org/wiki/Google_Safe_Browsing) for info about Safe Browsing.

Safe Browsing communicates with Google servers in order to download the blacklists. If you are looking for a feature like Safe Browsing, I recommend uBlock Origin or uMatrix.

## How do I install Flash player?

Adobe's version of Flash player (as opposed to Google's Flash player bundled with Chrome) on [Windows and macOS has an auto-update feature](https://helpx.adobe.com/flash-player/kb/flash-player-background-updates.html). Linux users will have to install updates manually, or use a PPAPI Flash player package available from their distribution. The following instructions are for installing Adobe's version of Flash player.

1. Go to https://get.adobe.com/flashplayer/otherversions/
2. Select the target platform for running Flash in Step 1.
3. For Step 2, select one of the following:
    * macOS: `FP 23 Mac for Opera and Chromium - PPAPI` (or the latest version)
    * Windows: `FP 23 for Opera and Chromium - PPAPI` (or the latest version)
    * Linux: `FP 23.0 for other Linux 64-bit (.tar.gz) - PPAPI` (or latest version and appropriate CPU architecture)
4. Click the "Download now" button, then install.

There are also ways to get Google's Flash player or other versions. See http://chromium.woolyss.com/#flash for more details.

## How do I install Widevine CDM?

These instructions are platform-specific.

**WARNING**: For all platforms, it is recommended to download the Google Chrome version that has the same major version as ungoogled-chromium. Otherwise, there may be stability issues or crashes.

### Linux

1. [Download the latest Google Chrome for Linux (.deb file)](https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb)
    * [Use this link for the latest unstable version](https://dl.google.com/linux/direct/google-chrome-unstable_current_amd64.deb)
2. Inside `data.tar.xz`, extract `./opt/google/chrome/WidevineCdm`
3. Move `WidevineCdm` to `/usr/lib/chromium`, where all other Chromium files should be. For portable or custom-built versions, it should be placed alongside the other Chromium files.

In ungoogled-chromium-debian, you can install Widevine DRM to additional locations. See `/usr/share/doc/ungoogled-chromium/README.Debian` for more details.

Here is a script that can automate this. It requires Debian because of `dpkg-deb`, but it can be modified to work on any sytsem:

```sh
#!/bin/bash -eux
# Replace with current Chromium version
_chrome_ver=79.0.3945.79

# Debian's Chromium has a patch to read libwidevinecdm.so in ~/.local/lib
# However, in 79 and newer, you must use the WidevineCdm directory instead of
# the libwidevinecdm.so file
_target_dir=~/.local/lib/WidevineCdm
_move_type=user_directory
# To have it accessible by all users, uncomment the below instead
#_target_dir=/usr/lib/chromium/WidevineCdm
#_move_type=system_directory

mkdir -p /tmp/chromium_widevine
pushd /tmp/chromium_widevine

# Download deb, which has corresponding Widevine version
# Support resuming partially downloaded (or skipping re-download) with -c flag
wget -c https://dl.google.com/linux/deb/pool/main/g/google-chrome-stable/google-chrome-stable_${_chrome_ver}-1_amd64.deb
# Use below link for unstable Chrome versions
#wget -c https://dl.google.com/linux/deb/pool/main/g/google-chrome-unstable/google-chrome-unstable_${_chrome_ver}-1_amd64.deb

# Unpack deb
rm -r unpack_deb || true
mkdir unpack_deb
dpkg-deb -R google-chrome-stable_${_chrome_ver}-1_amd64.deb unpack_deb

if [[ "$_move_type" == 'shared_obj' ]]; then
	# Move libwidevinecdm.so to target dir
	mkdir -p $_target_dir
	mv unpack_deb/opt/google/chrome/WidevineCdm/_platform_specific/linux_x64/libwidevinecdm.so $_target_dir
elif [[ "$_move_type" == 'user_directory' ]]; then
	# Move WidevineCdm to target dir owned by current user
	rm -r $_target_dir || true
	mv unpack_deb/opt/google/chrome/WidevineCdm $_target_dir
elif [[ "$_move_type" == 'system_directory' ]]; then
	# Move WidevineCdm to target dir in root-owned location
	sudo rm -r $_target_dir || true
	sudo mv unpack_deb/opt/google/chrome/WidevineCdm $_target_dir
	sudo chown -R root:root $_target_dir
else
	printf 'ERROR: Unknown value for $_move_type: %s\n' "$_move_type"
	exit 1
fi

popd
rm -r /tmp/chromium_widevine
```

As of version 67, `libwidevinecdmadapter.so` has been deprecated, and the Debian package `ungoogled-chromium-widevine` no longer exists.

### Windows

1. [Download the versions.txt file containing the list of Widevine versions](https://dl.google.com/widevine-cdm/versions.txt) 
    * [Alternatively, you can use the Arch repo to get the latest version via pkgver, and see if it's available for next step](https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=chromium-widevine)
2. Use the following link to get the latest WideVineCDM and replace **CURRENT** by the latest version from the txt file https://dl.google.com/widevine-cdm/CURRENT-win-x64.zip (for Win 32 use ia32 instead of x64)
3. Create a directory under your chromium installation (**NOT** in your AppData) call it WidevineCdm 
4. Inside that create another folder called _plaform_specific. Also, inside the _platform_specific folder, create win_x64 directory (x64 -> x86 if Win32). 
5. Place the manifest.json / LICENSE.txt under the WidevineCdm and place widevinecdm.dll/widevinecdm.sig under the win_x64

### macOS

1. [Download the latest Google Chrome for macOS (.dmg file)](https://dl.google.com/chrome/mac/stable/GGRO/googlechrome.dmg)
2. Open `Google Chrome.app` at least once.
3. Run the following command in the Terminal:

`cp -R ~/Library/Application\ Support/Google/Chrome/WidevineCDM ~/Library/Application\ Support/Chromium/WidevineCdm`

Note that there are no slashes after `WidevineCdm`.

## How do I get the Namespace Sandbox to work on Linux?

Enable the kernel option `unprivileged_userns_clone`

## How to get FIDO U2F security keys to work in Google sign in?

Google sign in uses a specific extension to access the security key's information. You'll need to install [this extension](https://chrome.google.com/webstore/detail/gnubbyd/beknehfpfkghjoafdifaflglpjkojoco) to make this function. After installation you might need to restart your computer to make it work.

## Why is my microphone not working?

There may be multiple causes:

1. You set your preferences or the master preferences with `"audio_capture_enabled": false`. This was the default in Debian/Ubuntu up to and including 81.0.4044.138.
2. Your application uses the built-in Google Speech API, which is disabled in ungoogled-chromium. This can be identified with an error message from Chromium such as:

```
[11883:11886:0904/222052.856218:ERROR:trk_protocol_handler.cc(17)] Blocked URL in
                 TrkProtocolHandler:
trk:184:https://www.9oo91e.qjz9zk/speech-api/full-duplex/v1/down?key=dummytoken&pair=52970E410A529E6C&output=pb
```

3. Applications like Skype for Web parse the name of the built-in PDF viewing plugin from the JavaScript API `navigator.plugins` ([See Issue #1010 comment](https://github.com/Eloston/ungoogled-chromium/issues/1010#issuecomment-643740388)). On ungoogled-chromium 83.0.4103.106-1 and newer, go to `chrome://flags/#pdf-plugin-name` and set the name to "Google Chrome" or "Microsoft Edge".

## I have a problem building ungoogled-chromium

See if the [Building FAQ](https://github.com/Eloston/ungoogled-chromium/blob/master/docs/building.md#building-faq) can address your problem. If not, check other resources in the [Support document](https://github.com/Eloston/ungoogled-chromium/blob/master/SUPPORT.md).
