---
layout: page
title: Statuses
weight: 1
permalink: /statuses
---

This page contains development and support statuses for various components.

**PLEASE NOTE**: All information in this document is for version **66.0.3359.139-1**, unless otherwise indicated.

Table of Contents:

* [Platform support](#platform-support)
* [Base config bundles](#base-config-bundles)
* [Packaging types](#packaging-types)

## Platform support

Official platform support is mostly associated with base config bundles. As such, each base bundle is labeled with their target platforms.

Packaging types are different ways to create distributable packages of build outputs.

For more details, see [DESIGN.md](//github.com/Eloston/ungoogled-chromium/blob/develop/DESIGN.md)

## Base config bundles

(These are directories in `resources/config_bundles`)

Brief statuses of base bundles:

* `archlinux` (for Arch Linux) - **Untested**
* `debian_stretch` (for Debian 9.0) - **Working**
* `ubuntu_bionic` (for Ubuntu 18.04) - **Untested**
* `linux_portable` (for any Linux system) - **Working**
    * NOTE: This config type is not optimized for any specific Linux distribution. It is designed for maximum compatibility and portability, unlike config types for specific distributions.
* `macos` (for macOS) - **Working**
* `opensuse` (for openSUSE) - **Untested**
* `windows` (for Microsoft Windows) - **Untested**

## Packaging types

(These are files in `resources/packaging`)

Some notes about the status of packaging types (see explanations in [DESIGN.md](//github.com/Eloston/ungoogled-chromium/blob/develop/DESIGN.md)):

* `archlinux` - **Working**
* `debian` - Flavor support statuses:
    * `minimal` - **Working**
    * `stretch` - **Working**
    * `buster` - **Untested**, but it won't be useful until a Debian buster base bundle is added.
* `linux_simple` - **Working**
* `macos` - **Working**
* `windows` - **Working**
