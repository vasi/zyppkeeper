# zyppkeeper

Declarative package management for OpenSUSE's zypper.

## Configuration

List the packages you want to keep in one or more files named `something.keep` in the ~/.config/zyppkeeper directory. Each of these files should contain a list of package names, one per line. You can format the files nicely: whitespace is ignored, and lines beginning with `#` are treated as comments.

## Usage

To start our, you'll want an initial idea of what zypper thinks you installed explicitly. Before running any other zyppkeeper commands, run this to output a good guess:

```
zyppkeeper init
```

Some packages that are declared may not be installed on your system yet. To fix that:

```
zyppkeeper install
```

Some packages may be installed, but not declared. These should either be removed, or added to a .keep file. To list the top-level unneeded packages:

```
zyppkeeper unneeded
```

To actually remove all unneeded packages, including their dependencies:

```
zyppkeeper clean
```

## Installation

It's just a Ruby script. Put it somewhere in your PATH.

Then try running `zyppkeeper unneeded` to output a guess at an initial set of keepers.

## Notes

Zyppkeeper considers a package needed if it is declared explicitly, or if it is pulled in by another needed package. This can be done via an RPM `Depends` specifier, or via a `Recommends`/`Enhances` specifiers (if zypper is configured to use them).

Zyppkeeper needs to modify the file `/var/lib/zypp/AutoInstalled` to update zypper's idea of what was requested explicitly. This means commands will generally require `sudo`. Every time this file is modified, zyppkeeper will backup the previous version with a `.bak` extension.

## Inspiration

* [debfoster](https://packages.debian.org/sid/debfoster) for apt-based distros (Debian, Ubuntu, etc)
* [pacdef](https://github.com/steven-omaha/pacdef) for Arch-based distros
* [pacmanfile](https://github.com/cloudlena/pacmanfile), also for Arch-based distros
* The [tab](https://docs.brew.sh/Manpage#tab-options-installed_formulainstalled_cask-) and [autoremove](https://docs.brew.sh/Manpage#autoremove---dry-run) subcommands of Homebrew
* The [set -A](https://man.freebsd.org/cgi/man.cgi?query=pkg-set&sektion=8&apropos=0&manpath=FreeBSD+14.2-RELEASE+and+Ports) and [autoremove](https://man.freebsd.org/cgi/man.cgi?query=pkg-autoremove&sektion=8&apropos=0&manpath=FreeBSD+14.2-RELEASE+and+Ports) subcommands of FreeBSD's pkg
* The [selected set](https://wiki.gentoo.org/wiki/Selected_set_(Portage)) of Gentoo's Portage, along with the `--select`, `--deselect` and `--depclean` options.
* The [mark](https://dnf.readthedocs.io/en/latest/command_ref.html#mark-command-label) and [autoremove](https://dnf.readthedocs.io/en/latest/command_ref.html#autoremove-command-label) subcommands of Fedora's dnf
* Of course, the declarative systems of [Nix](https://nixos.org/) and [Guix](https://guix.gnu.org/)

Also some obsolete previous attempts of mine at this sort of thing:
* [brewfoster](https://github.com/vasi/brewfoster) for Homebrew
* [rpmkeeper and yumkeeper](https://github.com/vasi/rpmkeeper) for yum-based distros
