meta-iojs
=========

OpenEmbedded/Ångstrom layer for latest stable io.js releases.

Installation
============

1. Add `meta-iojs` layer to `sources/layers.txt`

    ```
      meta-iojs,git://github.com/imyller/meta-iojs.git,master,HEAD
    ```
    
2. Add `meta-iojs` layer to `EXTRALAYERS` in `conf/bblayers.conf`

    ```
        EXTRALAYERS +=" \
            ${TOPDIR}/sources/meta-iojs \
        "
    ```
  
3. Run `oebb.sh update`

Usage
=====

### Building io.js package

1. To build latest stable io.js package:

```
    bitbake iojs
```

### io.js runtime as a dependency

Add io.js as a dependency in recipe with `RDEPENDS`.

Latest version:

```
    RDEPENDS_${PN} += "iojs"
```

### `npm install` buildable recipes

Inherit `iojs-npm` or `iojs-npm-install` build task classes in your recipe.

Bitbake classes 
===============

`meta-iojs` layer adds two new classes: `iojs-npm` and `iojs-npm-install`.

## `iojs-npm` class

`iojs-npm` defines the `oe_runnpm` command which will call cross-compiling `npm`.

For example:

```
  inherit iojs-npm
      
  do_install() {
    oe_runnpm install     # Installs dependencies defined in package.json 
  }
```

### Variables
      
You can define extra command line arguments for `npm` calls made by `oe_runnpm()` by appending them to `NPM_FLAGS` variable.
      
## `iojs-npm-install` class

`iojs-npm-install` class inherits `iojs-npm` class and adds following build tasks (listed in their run order):

  * `npm_install`: runs `npm install` in source directory
  * `npm_shrinkwrap`: runs `npm shrinkwrap` in source directory
  * `npm_dedupe`: runs `npm dedupe` in source directory

You can disable one or more of these build tasks in the recipe with `do_<taskname>[noexec] = "1"`:

```
  do_npm_shrinkwrap[noexec] = "1"
```

### Variables

* You can define extra command line arguments for `npm` command by appending them to `NPM_INSTALL_FLAGS` variable.

* you can define parameters for `npm install` command (such as specific package names) by appending them to `NPM_INSTALL` variable.

