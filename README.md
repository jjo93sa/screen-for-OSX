screen-for-OSX
==============

A version of screen that supports vertical splitting (Shortcut key `Ctrl+A |`), ready for building on OS X (all Apple patches pre-applied)

**Why use this version of screen?**

1. Because the one installed by default on OS X is really old, and doesn't support vertical splitting
2. Vertical splitting is a killer feature that you desperately need
3. You're on a Mac by necessity or choice, and you want to use screen with vertical splits, but don't want to mess with installing the [Apple patches](https://www.opensource.apple.com/source/screen/screen-16/patches/) required for building on OS X
4. Because building and installing your own software is cool


Installation
============

*Note: This requires developer tools to be installed in order to build successfully.  You might need to `brew install automake` if you're still missing `autoreconf` for some reason*

The following commands will download, build, and install `screen` into `/usr/local/bin/screen`.  *Note that this does not replace the default `screen` which is located in `/usr/bin/`*.  Make sure to read the section on "Post installation" below in order to use the newly installed screen instead of the old one.

Separate steps:

    git clone https://github.com/FreedomBen/screen-for-OSX.git
    cd screen-for-OSX
    ./install.sh

All in one command:

    git clone https://github.com/FreedomBen/screen-for-OSX.git && cd screen-for-OSX && ./install.sh
    

Post installation
=================

In order to run the new version of screen after it is installed (instead of the default old one), you will either need to reference it by full path (recommended) or rearrange your `PATH` variable so `/usr/local/bin` comes before `/usr/bin` so the new binary is found first (not recommended as it may have unintended side effects).

I recommend creating an alias in your `~/.bash_profile` or `~/.bashrc` that simply points `screen` to the new version.  This will allow you to still reference screen with "screen" but refer to the binary by its full path:

    echo 'alias screen="/usr/local/bin/screen"' >> ~/.bash_profile
    
If you don't care about the old version of screen, you can also replace it with the new binary:

    sudo cp /usr/local/bin/screen /usr/bin/screen


Differences from real Screen
============================

These are the things I did to this that make it vary from what you would get if you downloaded the official screen source code:

1. Applied all of the [Apple patches](https://www.opensource.apple.com/source/screen/screen-16/patches/) for `screen`
2. Downloaded and included the required Apple header file [vproc_priv.h](http://www.opensource.apple.com/source/launchd/launchd-328/launchd/src/vproc_priv.h) into the repo so it matches the source here.  **Note: This file is not usually included with distributions of screen because its license (Apple Apache) is incompatible with the GPLv3**
3. Added an install script to make the build and install process easier

Addendum
========

Building this software requires the Xcode development tools, but these no longer include automake and autoconf, which are dependencies. Instead of using Homebrew, we can build these tools from source easily enough. See https://superuser.com/a/897316

## autoconf
```
curl -O -L http://ftpmirror.gnu.org/autoconf/autoconf-2.69.tar.gz
tar -xzf autoconf-2.69.tar.gz
cd autoconf-*
./configure
make
sudo make install
autoconf --version
```

## automake
```
curl -O -L http://ftpmirror.gnu.org/automake/automake-1.15.tar.gz
tar -xzf automake-1.15.tar.gz
cd automake-*
./configure
make
sudo make install
automake --version
```
