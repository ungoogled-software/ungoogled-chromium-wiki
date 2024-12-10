---
layout: page
title: Frequently Asked Questions
menu: FAQ
weight: 2
permalink: /faq
---

## Why do I have to login to websites every time I open ungoogled-chromium?

Under `chrome://settings/content/siteData` there is a setting `Delete data sites have saved to your device when you close all windows`. In ungoogled-chromium this is selected by default. Switch this setting to "Allow sites to save data on your device" will prevent this behaviour.

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
* [Bookmarklet](https://en.wikipedia.org/wiki/Bookmarklet) (proposed in [Issue #869](https://github.com/ungoogled-software/ungoogled-chromium/issues/869)):
    * Go to [chrome://bookmarks/](chrome://bookmarks/)
    * Right click anywhere to select 'Add new Bookmark'. Copy the following into the URL field:

	```javascript
	javascript:location.href='https://clients2.google.com/service/update2/crx?response=redirect&acceptformat=crx2,crx3&prodversion='+(navigator.appVersion.match(/Chrome\/(\S+)/)[1])+'&x=id%'+'3D'+(document.querySelector('a[href^="./detail"][href$="/report"]').pathname.match(/([^\/]+)\/report$/)[1])+'%'+'26installsource%'+'3Dondemand%'+'26uc';
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

    **NOTE for Chromium 67 and newer**: If the Material Design page is used (which has been default before 67), "Developer mode" of `chrome://extensions/` (a switch at the top right corner) must be enabled for drag and drop to function. (Discovered in [#423](https://github.com/ungoogled-software/ungoogled-chromium/issues/423))

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

You can use the [chromium-web-store](https://github.com/NeverDecaf/chromium-web-store) extension. It also allows updates from non-Chrome Web Store sources (based on the proposal in [Issue #285](https://github.com/ungoogled-software/ungoogled-chromium/issues/285)).

There is currently no built-in functionality for auto-updates. [Issue #285](https://github.com/ungoogled-software/ungoogled-chromium/issues/285) proposes a solution.

## Do plugins work?

Yes. All plugins including PepperFlash and Widevine DRM should work. See the relevant question for specific installation instructions.

## Why are there URLs with the `qjz9zk` domain in them? Why use domain substitution?

`qjz9zk` is the common top-level domain name used by domain substitution. It is a relatively trivial way of disabling unwanted requests and notifying the user if any of these URLs attempt to connect without having to look through the many changes that happen to Chromium each version.

## Does domain substitution break the HSTS preload list?

No, the list (which is located in `net/http/transport_security_state_static.json`) is explicitely excluded when generating the domain substitution list. In `developer_utilities/update_helper.py`, see the  `generate_domain_substitution_list()` function for what files are excluded from domain substitution.

## Why is Safe Browsing disabled?

See [this Wikipedia article](//en.wikipedia.org/wiki/Google_Safe_Browsing) for info about Safe Browsing.

Safe Browsing communicates with Google servers in order to download the blacklists. If you are looking for a feature like Safe Browsing, I recommend uBlock Origin or uMatrix.

## Why can't I change settings because of a message like "This setting is managed by your administrator" or "This setting is disabled on managed browsers"

Among other, this happens often with the "Use secure DNS" setting.

The settings can be modified via a system-wide configuration. On Linux that is likely in directory `/etc/chromium/policies/managed`, file `policies.json`.

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

[Visit this page](https://chromium.woolyss.com/#widevine), and expand section "How to install the Widevine CDM plugin?"

### macOS

1. [Download the latest Google Chrome for macOS (.dmg file)](https://dl.google.com/chrome/mac/universal/stable/GGRO/googlechrome.dmg)
2. Mount the downloaded .dmg file
3. Run the following command in the Terminal:

`cp -R /Volumes/Google\ Chrome/Google\ Chrome.app/Contents/Frameworks/Google\ Chrome\ Framework.framework/Libraries/WidevineCdm /Applications/Chromium.app/Contents/Frameworks/Chromium\ Framework.framework/Libraries/WidevineCdm`

Note that there are no slashes after `WidevineCdm`.

## How do I get the Namespace Sandbox to work on Linux?

Enable the kernel option `unprivileged_userns_clone`

## How to get FIDO U2F security keys to work in Google sign in?

Google's sign in currently relies on an internal `googleLegacyAppIdSupport` extension as the previous non standard api [has been removed](https://groups.google.com/a/chromium.org/g/blink-dev/c/xHC3AtU_65A/m/yg20tsVFBAAJ). ungoogled-chromium breaks this extension which makes sign in not possible at this moment. Ideally Google will move to native WebAuthn soon.

## Why is my microphone not working?

There may be multiple causes:

1. You set your preferences or the master preferences with `"audio_capture_enabled": false`. This was the default in Debian/Ubuntu up to and including 81.0.4044.138.
2. Your application uses the built-in Google Speech API, which is disabled in ungoogled-chromium. This can be identified with an error message from Chromium such as:

```
[11883:11886:0904/222052.856218:ERROR:trk_protocol_handler.cc(17)] Blocked URL in
                 TrkProtocolHandler:
trk:184:https://www.9oo91e.qjz9zk/speech-api/full-duplex/v1/down?key=dummytoken&pair=52970E410A529E6C&output=pb
```

3. Applications like Skype for Web parse the name of the built-in PDF viewing plugin from the JavaScript API `navigator.plugins` ([See Issue #1010 comment](https://github.com/ungoogled-software/ungoogled-chromium/issues/1010#issuecomment-643740388)). On ungoogled-chromium 83.0.4103.106-1 and newer, go to `chrome://flags/#pdf-plugin-name` and set the name to "Google Chrome" or "Microsoft Edge".

## How do I fix the spell checker?

1. Go to <https://chromium.googlesource.com/chromium/deps/hunspell_dictionaries/+/master>
2. Find a bdic you want, click on it. You will see a mostly empty page aside from "X-byte binary file"
3. On the bottom right corner, click "txt". For en-US-10-1.bdic, you will get a link https://chromium.googlesource.com/chromium/deps/hunspell_dictionaries/+/master/en-US-9-0.bdic?format=TEXT
4. This is a base64-encoded file that needs to be decoded.
	* On Linux, simply run `base64 -d en-US-10-1.txt > ~/.config/chromium/Dictionaries/en-US-10-1.bdic` (assuming you want the dictionary to be in the default profile)
 	* On macOS, run `base64 -d -i en-US-10-1.txt > ~/Library/Application\ Support/Chromium/Dictionaries/en-US-10-1.bdic`
	* On Windows, run `mkdir %LOCALAPPDATA%\Chromium\Application\Dictionaries` and `certutil.exe -decode en-US-10-1.txt %LOCALAPPDATA%\Chromium\Application\Dictionaries\en-US-10-1.bdic` in cmd
5. Toggle spell check in `chrome://settings/languages`, or restart the browser for the dictionaries to take effect.

## How do I enable Chromecasting?

Navigate to `chrome://flags/#cast-media-route-provider` and set the flag to Enabled. Then relaunch the browser as the page indicates.

## How do I fix WebRTC?
WebRTC can leak your actual IP address even when using a VPN. In ungoogled-chromium WebRTC is limited by default, which can affect some web applications. This could be changed as follows:
1. Navigate to `chrome://flags/#webrtc-ip-handling-policy`
2. Select the desired policy from the dropdown.
3. Restart the browser to apply the changes.

## I have a problem building ungoogled-chromium

See if the [Building FAQ](https://github.com/ungoogled-software/ungoogled-chromium/blob/master/docs/building.md#building-faq) can address your problem. If not, check other resources in the [Support document](https://github.com/ungoogled-software/ungoogled-chromium/blob/master/SUPPORT.md).
