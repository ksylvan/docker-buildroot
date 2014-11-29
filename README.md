docker-buildroot
================

buildroot based docker image instructions

Based on this post http://blog.docker.com/2013/06/create-light-weight-docker-containers-buildroot/

Steps to create the tiny docker image:

    curl http://buildroot.uclibc.org/downloads/buildroot-2014.08.tar.bz2 | tar jx
    cd buildroot-2014.08/
    make menuconfig
    # go to target architecture, select x86_64, exit and save
    make

Your image should land in output/images

    cd ouput/images
    ls -l
    rootfs.tar

Next, we have to replace the buildroot symlinks for /etc/resolv.conf and the /sbin/init one with regular files.

Based on https://gist.github.com/jpetazzo/b932fb0c753e69c73d31 we can do this:

    $ cp -a buildroot-2014.08/output/images output_images
    $ cd output_images/
    $ mkdir -p extra/etc extra/sbin
    $ touch extra/etc/resolv.conf extra/sbin/init
    $ cp rootfs.tar fixup.tar
    $ tar rvf fixup.tar -C extra .
    ./
    ./sbin/
    ./sbin/init
    ./lib64/
    ./lib/
    ./etc/
    ./etc/resolv.conf

And now, we can import this buildroot image into docker:

    $ docker import - dietfs < fixup.tar

We can use this as the base of other images now.
