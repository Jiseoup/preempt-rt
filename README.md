# PREEMPT RT PATCH on Ubuntu 18.04
This document introducing how to install **`PREEMPT RT`** on **`ubuntu 18.04`**.  
[REFERENCE PAGE](https://chenna.me/blog/2020/02/23/how-to-setup-preempt-rt-on-ubuntu-18-04/)

## INSTALL DEPENDENCIES
    $ sudo apt-get install build-essential libncurses-dev bison flex libssl-dev libelf-dev git

## DOWNLOAD AND PATCH
```terminal
$ wget https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.4.193.tar.xz
$ wget https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.4/patch-5.4.193-rt74.patch.xz
    
$ xz -cd linux-5.4.193.tar.xz | tar xvf -
$ cd linux-5.4.193
$ xzcat ../patch-5.4.193-rt74.patch.xz | patch -p1
```
    
## CONFIGURATION
    $ cp -v /boot/config-$(uname -r) .config
    
## EDIT CONFIG FILE
ERROR

## EDIT CONFIG FILE
    $ make menuconfig
    
    General setup -> Preemption Model -> Fully Preemptible Kernel (Real-Time)
    Processor type and features -> Timer frequency -> 1000 HZ
    
    [uniprocessor]Processor type and features -> Symmetric multi-processing support [OFF]
    
    Save -> Exit
    
## BUILD AND INSTALL
    $ make -j $(nproc) deb-pkg
