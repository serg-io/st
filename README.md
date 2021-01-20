## Description ##

This repository is a fork of the Suckless [st](https://st.suckless.org/) (simple terminal) project
and was created to manage patches. To avoid any conflicts with the original _master_ branch from
st, this repository does not contain a _master_ branch. Instead, it has a _main_ branch which
contains a [PKGBUILD](https://wiki.archlinux.org/index.php/PKGBUILD) file that makes it easy to
install a patched version of st in [Arch Linux](https://www.archlinux.org/). All patches are stored
seperately in branches with names that start with "patch-".

## Getting Started ##

To get started, clone this repository and rename the default `origin` remote.

	git clone git@github.com:serg-io/st.git
	cd st
	git remote rename origin github

Now add a remote for the original `suckless` repository.

	git remote add suckless https://git.suckless.org/st

## Installation ##

To install a patched version of st in Arch Linux first make sure you have all the latest changes
from the `suckless` repository.

	git fetch suckless

Create a `build` branch using `suckless/master` as a starting point and merge the `github/main`
branch.

	git branch --no-track build suckless/master
	git checkout build
	git merge github/main

If you already have a `build` branch reset its `HEAD` and merge `github/main`.

	git checkout build
	git reset --hard suckless/master
	git merge github/main

Now that you have a `build` branch with the files from `github/main` you can use
[makepkg](https://wiki.archlinux.org/index.php/Makepkg) to build and install st.

	makepkg -sci

Before st is built, the `PKGBUILD` script will merge all the remote branches that start with
"patch-".

## Patch Branches ##

If you fork this repo, and use your own forked repo instead of this one, you'll be able to create
your own patch branches. To create a patch branch, you must first fetch the most recent changes
from `suckeless`.

	git fetch suckless

Then create a new branch using `suckless/master` as the starting point and push it to your
repo. Make sure the name of the branch starts with "patch-".

	git branch --no-track patch-config suckless/master
	git push -u github patch-config
	git checkout patch-config

At this point you could apply any of the patches listed in the [Suckless
website](https://st.suckless.org/patches/). However, in this example we're going to create a custom
`config.h` file to increase the size of the font. Start by copying the `config.def.h` file.

	cp config.def.h config.h

Then, edit the `config.h` file. To increase the font size, you could simpy increase the value of
`pixelsize` or keep the same value but replace `pixelsize` with `size`.

Commit and push your changes.

	git add .
	git commit -m "Increased font size."
	git push

You can now follow the installation instructions explained above and your patch branch will be
automatically merged during the installation.
