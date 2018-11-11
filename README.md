Yocto Project meta-cpan Docker build
====================================

This tiny repository should help people who want to play with Yocto and
meta-cpan on their own machines to get a nice bootstrap managed.

It allowes builds and run via qemu - for everything else a dedicated
network boot image is required which needs partial network control.
Because of that is dangerous, it may come later or never.

Unfortuantely there is no real chance to provide dhcpd, tftpd or nfs
from a Docker instance for some VLAN only. The nature of those layer
2b services (okay, for NFS it could work, but without ground support
it can't be preconfigured in a sane way).

Provided images
---------------

Currently are following images available:
* build-meta-cpan
* update-meta-cpan

Build Docker Image
------------------

For each of the provided images, the commands are the same. The examples
will use `build-meta-cpan`.

### First thing is to build the image from construction guide

```bash
docker build -t build-meta-cpan build-meta-cpan/
```

This step includes slide 3 from [Perl on embedded Devices](https://www.slideshare.net/JensRehsack/perl-onembeddeddevices)

### Second step is to use it

```bash
docker run -ti build-meta-cpan
```

From here you can either follow the [Perl on embedded Devices](https://www.slideshare.net/JensRehsack/perl-onembeddeddevices) slides
or improve your network configuration and shared folders of the docker image.

Deployment of resulting images
------------------------------

Without a managable switch, properly configured dhcpd, tftpd and nfs you're
sticked to qemu.
