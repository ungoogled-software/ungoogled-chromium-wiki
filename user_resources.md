---
layout: page
title: User Resources
menu: User Resources
weight: 3
permalink: /user-resources
---

# Introduction

This page collects information about extensions and options for Chromium that may be useful for users.

Please submit a [PR to ungoogled-chromium-wiki](//github.com/ungoogled-software/ungoogled-chromium-wiki/pulls) if you want to contribute.

# Useful Extensions

*TODO: bulleted list of extensions, each with a short explanation of their purpose, benefits, and potential drawbacks*

* [chromium-web-store](https://github.com/NeverDecaf/chromium-web-store) - Check and/or install updates to CRX-based extensions (i.e. not exclusively from the Chrome Web Store). It supports optional automatic updates. This is an extension-based implementation of [Issue #285](https://github.com/ungoogled-software/ungoogled-chromium/issues/285).

# Disable JIT

The [JIT](https://en.wikipedia.org/wiki/Just-in-time_compilation) optimization of JavaScript is a [known attack vector](https://microsoftedge.github.io/edgevr/posts/Super-Duper-Secure-Mode/). It can be disabled with the launch flag `--js-flags='--jitless'`.

JavaScript will still run with JIT disabled, but websites may be slower, especially script-heavy apps like Google Maps. Disabling JIT also disables WebAssembly.

# Disable WebAssembly

*(Suggested in [#578](//github.com/ungoogled-software/ungoogled-chromium/issues/578))*

Like JavaScript, WebAssembly is another potential attack vector. See [#578](//github.com/ungoogled-software/ungoogled-chromium/issues/578) for the details of exploiting WebAssembly.

See [stevenspringett/disable-webassembly](//github.com/stevespringett/disable-webassembly) for instructions on disabling WebAssembly.

# Disable Weak SSL Cipher Suites

Disable weak SSL cipher suites to prevent usage in SSL-encrypted traffic like HTTPS.

The command-line flag is `--cipher-suite-blacklist`, with a comma-delimited list of cipher suites in hexadecimal. There must be exactly four hexadecimal digits per cipher suite. Example:

```
chromium --cipher-suite-blacklist=0x000a,0x009c,0x009d,0x002f,0x0035
```


# Disable Media Router

Disabling Media Router prevents the browser from sending UDP broadcasts. (NOTE: Broadcasts happen even if the media router extension is disabled). This can be achieved by setting the policies:

**Linux** 

Create or add the following JSON content to `/etc/chromium/policies/managed/policies.json`:

```json
{
  "EnableMediaRouter": false
}
```

Here is an example of `policies.json` with more configuration settings:

```json
{
  "EnableMediaRouter": false,
  "DefaultWebUsbGuardSetting": 2,
  "DefaultWebBluetoothGuardSetting": 2,
  "HomepageLocation": "http://site.com",
  "NativeMessagingUserLevelHosts": false,
  "WPADQuickCheckEnabled": false,
  "BackgroundModeEnabled": false
}
```

**macOS**

Consult the documentation on setting policies [here](https://www.chromium.org/administrators/configuring-other-preferences). Then, set `"EnableMediaRouter": false`.

**Windows** 

Consult the documentation [here](https://www.chromium.org/administrators/configuring-policy-for-extensions) for configuring policies. Then, set `"EnableMediaRouter": false`.
