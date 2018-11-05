---
layout: page
title: User Resources
menu: User Resources
weight: 3
permalink: /user-resources
---

This page collects information about extensions and options for Chromium that may be useful for users.

Please submit a [PR to ungoogled-chromium-wiki](//github.com/ungoogled-software/ungoogled-chromium-wiki/pulls) if you want to contribute.

# Extensions

*TODO: bulleted list of extensions, each with a short explanation of their purpose, benefits, and potential drawbacks*

# Disable WebAssembly

*(Suggested in [#578](//github.com/Eloston/ungoogled-chromium/issues/578))*

Like JavaScript, WebAssembly is another potential attack vector. See [#578](//github.com/Eloston/ungoogled-chromium/issues/578) for the details of exploiting WebAssembly.

See [stevenspringett/disable-webassembly](//github.com/stevespringett/disable-webassembly) for instructions on disabling WebAssembly.

# Disable weak cipher suites

Disable weak SSL cipher suites to prevent usage in SSL-encrypted traffic like HTTPS.

The command-line flag is `--cipher-suite-blacklist`, with a comma-delimited list of cipher suites in hexadecimal. Example:

```
chromium --cipher-suite-blacklist=0x000a,0x009c,0x009d,0x002f,0x0035
```
