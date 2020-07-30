# A simple wrapper to make dpkg faster (and unsafe)

This package's purpose is to trade safety for execution speed in
operations of _dpkg_ (and any program it may execute) by using
[eatmydata](https://launchpad.net/libeatmydata) to disable disk
synchronization calls. This is useful in situations where data loss in
case of a power outage is not a concert, e.g. in setting up ephemeral
_chroot_ environments for building Debian packages.

**Do not use in production environments where you might care about
data integrity!**

On installation, `/usr/bin/dpkg` is moved aside and replaced with a
wrapper script using _dpkg-divert(1)_, this process is of course
reversed when removing this package.


debootstrap --include=eatmydata .....

- $R is your chroot directory

```bash
for archive in $R/var/cache/apt/archives/*eatmydata*.deb; do
  dpkg-deb --fsys-tarfile "$archive" >$R/eatmydata
  tar -xkf $R/eatmydata -C $R
  rm -f $R/eatmydata
done
```
