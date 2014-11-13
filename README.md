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
