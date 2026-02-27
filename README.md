# An Unofficial Mirror of Leidos's GSFLib

## Generic Sensor Format

The _Generic Sensor Format_ (GSF) is a file format for the storage and interchange of bathymetric sonar data. The format is controlled and specified by Leidos. For more information, including the official GSF spec, see the [Leidos website](https://www.leidos.com/products/ocean-marine#gsf).

## The GSFLib Library

`GSFLib` is an open-source C library supporting reading and writing of GSF files. Leidos develops the library, holds all copyright, and releases it under an [LGPL 2.1 licence](LICENSE.md). For official releases and documentation, again, visit the [Leidos website](https://www.leidos.com/products/ocean-marine#gsf).

## This Repository

This repository is an unofficial mirror of recent `GSFLib` releases. It is not a fork of the library or an attempt to take over `GSFLib` development. It aims to be a Git alternative to official `.zip` releases available from Leidos.

It has two primary branches:

1. The `leidos-releases` branch (and the `leidos-v3.xx` tags) contain the `GSFLib` source code, exactly as released by Leidos.
2. The `main` branch (and the `v3.xx` tags) contain the `GSFLib` source code with minor modifications.

The changes on `main` largely focus on easing integration of `GSFLib` with modern development environments. It contains no changes to library behaviour or the format of the GSF files it reads or creates. Changes include:

- The addition of a basic CMake build system to both build and deploy the library.
- Modifications to allow the code to build and link on modern Windows, Linux and macOS development environments.
