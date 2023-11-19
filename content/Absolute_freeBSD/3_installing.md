+++

Title = "Installing"
weight = 3

+++

### Core Settings

- Boot multiuser for installation
- Choosing a keymap
- Choose a hostname

### Distribution Selection

`base-dbg` : Debugging symbols for the base system, useful to programmers

`kernel-dbg` : Debugging symbols for the kernel, useful to programmers

`lib32-dbg` : 32-bit compatibility libraries (only on 64-bit systems)

`src` : Source code of installed operating systems

`test` : FreeBSD's self-test tools

`doc` : FreeBSD's official documentation, such as Handbook (not in FreeBSD13.0)

I recommend always installing the operating system source code. It takes up very little space and can be an invaluable resource.

### Disk Partitioning

##### UFS Installs

we divide this disk as follows

- 512KB freebsd-boot EFI boot partition
- 1GB swap
- 4GB emergency dump space
- 1GB root(`/`)
- 512MB `/tmp`
- 2GB `/var`
- Everything else in `/usr`

##### ZFS Installs

Pool Name : default `zpool` is ok unless you want something 	

Enforce 4K sector : set to YES

Encrypt Disks : Note : FreeBSD uses GELI for ZFS encryption

Partition Scheme : GPT

Swap Space : 4GB is fine

Pool Type : choose stripe-no redundancy

### Network Configuration

Little obvious

### System Hardening

These options are very significant and ideally all of them should be enabled.
