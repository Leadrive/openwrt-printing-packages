查找mkromfs.c
在前面添加
#define sprintf DO_NOT_USE_SPRINTF

# Printing packages for OpenWrt/LEDE

This is a [package feed] aiming at providing a complete printing stack
for [OpenWrt] or [LEDE].

Notably it has:
- Ghostscript 9.20
- Gutenprint 5.2.11
- Cups 2.2.1
- OpenPrinting's cups-filters 1.11.4
- poppler 0.47.0
- many other packages to make sure the ones above work...

[OpenWrt]: https://openwrt.org/

[LEDE]: https://www.lede-project.org/

[package feed]: http://wiki.openwrt.org/doc/devel/feeds

[timesys.com]: http://repository.timesys.com/buildsources/g/ghostscript/

### To use this feed,

- set up your router to use [external storage] for its root file
  system, as these packages require more than a 100 MB of space.

[external storage]: http://wiki.openwrt.org/doc/howto/extroot

- set up a [cross-compilation environment]

[cross-compilation environment]: http://wiki.openwrt.org/doc/devel/crosscompile

- add this line to your `feeds.conf` or `feeds.conf.default`

```
src-git printing git://github.com/Leadrive/openwrt-printing-packages.git
```

- to compile everything in this feed you should use the script `setup-buildsystem.sh` or some variation of those commands.

- copy compiled packages to your router (copy the whole directory as you need the files used to index the packages)

```
scp -r ./bin/$ARCH/packages root@openwrt.lan:/storage/printer/packages/
```

- add local package source to the opkg configuration `/etc/opkg.conf` with

```
src/gz printing file:/storage/printer/packages
```

- see `opkg-install-printing-packages.sh` to see a suggestion of what to install.

- tested against *Attitude Adjustment* (because that is what I have installed...).

- Avahi is notified of printers added to Cups, and these will appear as *Air Printer*'s in iOS devices.

### Issues / Missing / TODO

Caveat: Ghostscript lacks proper cross-compilation support. I used a
patch taken from [timesys.com]. If your architecture is not there,
compiling it just won't work for you.

The alternative for those who can't compile Ghostscript is to use a
different PDF backend, in this case Poppler. For instructions of how
to do this open the tar-ball of the `cups-filters-*.tar.bz2` and check
the section *1. Selection of the renderer: Ghostscript, Poppler, or
Adobe Reader* of the `README`.
