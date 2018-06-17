# kernel_config

all kernel config for kernel has already patch with aufs patch and it for  x86_64 

to use you must patch your kernel first

to use for x86/32 bit unset 
  [] 64-bit kernel  

# for other aufs4 patch 
  https://github.com/sfjro/aufs4-standalone

# how to patch aufs 


-- downloading 

o aufs4-standalone tree
$ git clone git://github.com/sfjro/aufs4-standalone.git aufs4-standalone.git
$ cd aufs4-standalone.git
$ git checkout origin/aufs4.0

o aufs-util tree
$ git clone git://git.code.sf.net/p/aufs/aufs-util aufs-util.git
- note that the public aufs-util.git is on SourceForge instead of
  GitHUB.
$ cd aufs-util.git
$ git checkout origin/aufs4.0

Note: The 4.x-rcN branch is to be used with `rc' kernel versions ONLY.
The minor version number, 'x' in '4.x', of aufs may not always
follow the minor version number of the kernel.
Because changes in the kernel that cause the use of a new
minor version number do not always require changes to aufs-util.

Since aufs-util has its own minor version number, you may not be
able to find a GIT branch in aufs-util for your kernel's
exact minor version number.
In this case, you should git-checkout the branch for the
nearest lower number.

For (an unreleased) example:
If you are using "linux-4.10" and the "aufs4.10" branch
does not exist in aufs-util repository, then "aufs4.9", "aufs4.8"
or something numerically smaller is the branch for your kernel.

Also you can view all branches by
	$ git branch -a


 Configuration and Compilation
----------------------------------------
Make sure you have git-checkout'ed the correct branch.

For aufs4-linux tree,
- enable CONFIG_AUFS_FS.
- set other aufs configurations if necessary.

For aufs4-standalone tree,
There are several ways to build.

patch 
   patch -Np1 -i ../file/patch/loction.patch

note: 
Never copy aufs4-patch-folder/include/uapi/linux/Kbuild


1.
- apply ./aufs4-kbuild.patch to your kernel source files.
- apply ./aufs4-base.patch too.
- apply ./aufs4-mmap.patch too.
- apply ./aufs4-standalone.patch too, if you have a plan to set
  CONFIG_AUFS_FS=m. otherwise you don't need ./aufs4-standalone.patch.
- copy ./{Documentation,fs,include/uapi/linux/aufs_type.h} files to your
  kernel source tree. Never copy $PWD/include/uapi/linux/Kbuild.
- enable CONFIG_AUFS_FS, you can select either
  =m or =y.
- and build your kernel as usual.
- install the built kernel.
  Note: Since linux-3.9, every filesystem module requires an alias
  "fs-<fsname>". You should make sure that "fs-aufs" is listed in your
  modules.aliases file if you set CONFIG_AUFS_FS=m.
- install the header files too by "make headers_install" to the
  directory where you specify. By default, it is $PWD/usr.
  "make help" shows a brief note for headers_install.
- and reboot your system.

2.
- module only (CONFIG_AUFS_FS=m).
- apply ./aufs4-base.patch to your kernel source files.
- apply ./aufs4-mmap.patch too.
- apply ./aufs4-standalone.patch too.
- build your kernel, don't forget "make headers_install", and reboot.
- edit ./config.mk and set other aufs configurations if necessary.
  Note: You should read $PWD/fs/aufs/Kconfig carefully which describes
  every aufs configurations.
- build the module by simple "make".
  Note: Since linux-3.9, every filesystem module requires an alias
  "fs-<fsname>". You should make sure that "fs-aufs" is listed in your
  modules.aliases file.
- you can specify ${KDIR} make variable which points to your kernel
  source tree.
- install the files
  + run "make install" to install the aufs module, or copy the built
    $PWD/aufs.ko to /lib/modules/... and run depmod -a (or reboot simply).
  + run "make install_headers" (instead of headers_install) to install
    the modified aufs header file (you can specify DESTDIR which is
    available in aufs standalone version's Makefile only), or copy
    $PWD/usr/include/linux/aufs_type.h to /usr/include/linux or wherever
    you like manually. By default, the target directory is $PWD/usr.
- no need to apply aufs4-kbuild.patch, nor copying source files to your
  kernel source tree.