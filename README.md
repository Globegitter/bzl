# bzl

`bzl` is a command-line application that adds extra functionality to
[bazel](https://bazel.build). In practice, it acts as a drop-in
replacement for bazel itself (and shorter to type!). Any command or
command-line-option not specifically interpreted by bzl is passed
directly to bazel itself.

For example: `$ bzl build :app` works like you'd expect.

## Status

Experimental.

## Install bzl

`bzl` ships as a single executable go binary. Download the file
directly from the [Github Releases
Page](https://github.com/bzl-io/bzl/releases) for the precompiled
platform of your choice (or build from source).

## How is `bzl` pronounced?

`bzl` is pronounced like bezel, as in "*the bezel of a watch*". The
name invokes it's function (a wrapper around bazel).

> In the 1950s, watchmakers realized that an external bezel was the
> best way to add functions to a watch without complicating the
> movement, and so the external watch bezel was born.

## `bzl` commands

### `$ bzl --help`

Show help.

### `$ bzl install`

List or install available bazel installs.

Examples:

| Command | Description |
| --- | --- |
| `$ bzl install` | List all available releases |
| `$ bzl install 0.8.0` | Install bazel release 0.8.0 |
| `$ bzl install --list 0.8.0` | Show the assets bundled in install 0.8.0 |

### `$ bzl target`

Pretty-print available targets in the current workspace.

| Command | Description |
| --- | --- |
| `$ bzl targets` | List all available targets |
| `$ bzl targets //my/package` | List targets in `//my/package` |
