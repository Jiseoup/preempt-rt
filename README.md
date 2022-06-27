# PREEMPT RT PATCH on Ubuntu 18.04
This document introducing how to install **`PREEMPT RT`** on **`ubuntu 18.04`**.  
I got the information from [here](https://chenna.me/blog/2020/02/23/how-to-setup-preempt-rt-on-ubuntu-18-04/).

## INSTALL DEPENDENCIES
```bash
$ sudo apt-get install build-essential libncurses-dev bison flex libssl-dev libelf-dev git
```

## DOWNLOAD AND PATCH
Download the **`linux-5.4.193`** kernel and the **`RT patch`** from [kernel.org](https://kernel.org).
```bash
$ wget https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.4.193.tar.xz
$ wget https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.4/patch-5.4.193-rt74.patch.xz
```

Extract the file and apply the patch.
```bash    
$ xz -cd linux-5.4.193.tar.xz | tar xvf -
$ cd linux-5.4.193
$ xzcat ../patch-5.4.193-rt74.patch.xz | patch -p1
```
    
## CONFIGURATION
Copy the old config file.
```bash
$ cp -v /boot/config-$(uname -r) .config
```
    
## EDIT CONFIG FILE
You may have to edit the **`.config`** file.
```bash
$ vim .config
```
Search the **`CONFIG_SYSTEM_TRUSTED_KEYS`** and erase the contents inside the double quotation marks.  

### Before Modification
---------------------------
<img src="/img/before.png" width="50%" height="50%"/>

### After Modification
---------------------------
<img src="/img/after.png" width="50%" height="40%"/>

## MENUCONFIG SETTINGS
You have to set the menu configuration.
```bash
$ make menuconfig
```

### Essential Settings
```
General setup -> Preemption Model -> Fully Preemptible Kernel (Real-Time)
Processor type and features -> Timer frequency -> 1000 HZ
```

[uniprocessor]Processor type and features -> Symmetric multi-processing support [OFF]

Save -> Exit
    
## BUILD AND INSTALL
```bash
$ make -j $(nproc) deb-pkg
```

```bash
$ sudo dpkg -i ../linux-headers-5.4.193-rt74_5.4.193-rt74-1_amd64.deb ../linux-image-5.4.193-rt74_5.4.193-rt74-1_amd64.deb ../linux-libc-dev_5.4.193-rt74-1_amd64.deb
```
