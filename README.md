[![Build Status](https://travis-ci.org/zwilias/elm-json.svg?branch=master)](https://travis-ci.org/zwilias/elm-json) [![Build status](https://ci.appveyor.com/api/projects/status/0vub0mf2oenp5lbd/branch/master?svg=true)](https://ci.appveyor.com/project/zwilias/elm-json/branch/master) [![npm version](https://badge.fury.io/js/elm-json.svg)](https://badge.fury.io/js/elm-json)

> Deal with your elm.json

> **NOTE:** This is very much a work in progress. May mess up your files
> completely. Use with care.

`elm-json` provides a bunch of tools to make common tasks involving your
`elm.json` files a little easier: upgrading your dependencies, installing
specific versions of packages, removing dependencies or initializing new
packages, to name a few.

`elm-json` never writes to files used by the official toolchain, except
`elm.json` itself. It may, however, read some files from your `ELM_HOME` when
possible, to prevent downloading things you may already have on your filesystem.

<!--ts-->
   * [Installation](#installation)
   * [Usage](#usage)
      * [elm-json help](#elm-json-help)
      * [Adding dependencies: elm-json install](#adding-dependencies-elm-json-install)
         * [Examples](#examples)
      * [Removing dependencies: elm-json uninstall](#removing-dependencies-elm-json-uninstall)
         * [Examples](#examples-1)
      * [Upgrading dependencies: elm-json upgrade](#upgrading-dependencies-elm-json-upgrade)
      * [Initializing applications/packages elm-json new](#initializing-applicationspackages-elm-json-new)
      * [For tooling: elm-json solve](#for-tooling-elm-json-solve)
      * [Generating shell completions: elm-json completions](#generating-shell-completions-elm-json-completions)

<!-- Added by: ilias, at: Mon Apr 22 17:07:44 CEST 2019 -->

<!--te-->

# Installation

Binaries are attached to the github releases and distributed for Windows, OS X
and Linux (statically linked with musl).

For ease of installation, an npm installer also exists:

```
npm install --global elm-json
```

# Usage

`elm-json` offers a bunch of subcommands to make life a little easier.

## `elm-json help`

```
USAGE:
    elm-json [FLAGS] <SUBCOMMAND>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
    -v, --verbose    Sets the level of verbosity

SUBCOMMANDS:
    help         Prints this message or the help of the given subcommand(s)
    install      Install a package
    new          Create a new elm.json file
    uninstall    Uninstall a package
    upgrade      Bring your dependencies up to date
```

Gives a quick overview of the more common subcommands. This can also be used for
finding documentation about specific subcommands.

## Adding dependencies: `elm-json install`

```
USAGE:
    elm-json install [FLAGS] <PACKAGE>... [-- <INPUT>]

FLAGS:
    -h, --help       Prints help information
        --test       Install as a test-dependency
    -V, --version    Prints version information
        --yes        Answer "yes" to all questions

ARGS:
    <PACKAGE>...    Package to install, e.g. elm/core or elm/core@1.0.2
    <INPUT>         The elm.json file to upgrade [default: elm.json]
```

`elm-json install` allows installing dependencies, at the latest version that
works given your existing dependencies, or a particular version if you so
choose. By adding the `--test` flag, the chosen package(s) will be added to your
`test-dependencies` rather than your regular `dependencies`.

### Examples

```
elm-json install elm/http
```

Adds the latest version of `elm/http` to your dependencies.

For packages, it will use the latest possible version as the lowerbound, and the
next major as the exclusive upper bound. This mirrors the behaviour of `elm
install`.

For applications, this will pick the latest available version, adding all
indirect dependencies as well.


```
elm-json install --test elm/http@2.0.0
```

Adds version 2.0.0 of `elm/http` to your test-dependencies.

For packages, the provided version is used as the lower bound, with the next
major being used as the exclusive upper bound.

For applications, this will install exactly the specified version.

```
elm-json install elm/http elm/json -- elm/elm.json
```

Add the latest possible versions of `elm/http` and `elm/json` to
`./elm/elm.json`.

## Removing dependencies: `elm-json uninstall`

```
USAGE:
    elm-json uninstall [FLAGS] <PACKAGE>... [-- <INPUT>]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
        --yes        Answer "yes" to all questions

ARGS:
    <PACKAGE>...    Package to uninstall, e.g. elm/html
    <INPUT>         The elm.json file to upgrade [default: elm.json]
```

Uninstall dependencies. This is the inverse of `elm-json install` and its API is
similar but slightly simpler.

Version bounds may not be specified and `--test` is not an allowed flag for this
command.

### Examples

```
elm-json uninstall elm/html
```

Removes the `elm/html` package from your dependencies.

> **NOTE**: This subcommand does not yet support `elm.json` files with type
> `package`.

## Upgrading dependencies: `elm-json upgrade`

```
USAGE:
    elm-json upgrade [FLAGS] [INPUT]

FLAGS:
    -h, --help       Prints help information
        --unsafe     Allow major versions bumps
    -V, --version    Prints version information
        --yes        Answer "yes" to all questions

ARGS:
    <INPUT>    The elm.json file to upgrade [default: elm.json]
```

Upgrade your dependencies.

By default, this will only allow patch and minor changes for direct (test)
dependencies.

When the `--unsafe` flag is provided, major version bumps are also allowed. Note
that this may very well break your application. Use with care!

> **NOTE**: This subcommand does not yet support `elm.json` files with type
> `package`.

## Initializing applications/packages `elm-json new`

```
USAGE:
    elm-json new

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```

Create a new `elm.json` file, for applications or packages.

This is very rudimentary right now.

## For tooling: `elm-json solve`

```
USAGE:
    elm-json solve [FLAGS] [OPTIONS] [--] [INPUT]

FLAGS:
    -h, --help        Prints help information
    -m, --minimize    Choose lowest available versions rather than highest
        --test        Promote test-dependencies to top-level dependencies
    -V, --version     Prints version information

OPTIONS:
    -e, --extra <PACKAGE>...    Specify extra dependencies, e.g. elm/core or
                                elm/core@1.0.2

ARGS:
    <INPUT>    The elm.json file to solve [default: elm.json]
```

Documentation TBD. Intended for other tooling to use, not meant for human
consumption.

## Generating shell completions: `elm-json completions`

```
USAGE:
    elm-json completions <SHELL>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <SHELL>    The shell to generate the script for [possible values: bash,
               fish, zsh]
```

Create completion scripts for `elm-json` for `bash`/`fish`/`zsh`.
