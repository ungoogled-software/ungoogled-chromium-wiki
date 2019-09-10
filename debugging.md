---
layout: page
title: Debugging ungoogled-chromium
menu: Debugging
weight: 4
permalink: /debugging
---

## Introduction

This document describes various ways to debug ungoogled-chromium, including not widely known but useful ways to debug Chromium.

**IMPORTANT**: Some of these methods require you to be familiar with building and debugging Chromium/ungoogled-chromium. However, if something seems unclear or wrong, let us know.

Since ungoogled-chromium is pretty much Chromium, the same debugging workflows from Chromium still apply. (TODO: Link to debugging documentation)

## General debugging tips

(This section is incomplete. Feel free to contribute.)

Linux users: If applicable, try to reproduce your issue on Portable Linux first. This may help narrow down the problem faster.

## How do I get a stack trace from ungoogled-chromium?

You can get a stack trace when ungoogled-chromium crashes. To do that, rebuild ungoogled-chromium with the additional GN flags:

```
symbol_level=1
blink_symbol_level=1
exclude_unwind_tables=false
```

Note for Linux users: You can also set `is_debug=true`, but you may need to wary that this may break library unbundling [like it did for ICU](https://github.com/ungoogled-software/ungoogled-chromium-archlinux/issues/28#issuecomment-529076184). In this case, the easiest way is to try to reproduce with Portable Linux, then rebuild it.

Additionally, you can log a stack trace to the console at any point in the code by adding the following:

```c
#include "base/debug/stack_trace.h"
// ...
// At the point you want a trace, call:
base::debug::StackTrace().Print();
```

(Thanks @howaboutsynergy for the tip)
