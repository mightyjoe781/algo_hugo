+++

Title = "Before you Install"
weight = 2

+++

A successful install is one that works for its intended purpose.

### Default Files

FreeBSD separates configuration files into default files and customization files. The *default files* contains variable assignments and aren't intended to edited; instead they are designed to be overridden by another file of the same name.

Default configuration are kept in a directory called default. For example boot loader configuration files is `/boot/loader.config` and the default configuration file is in `/boot/defaults/loader.conf`.

During upgrades the installer replaces the default config files but doesn't touch local configuration files.

You can use a VCS to keep track of local configuration files.

Note : Don't copy the default config ! don't copy the default configuration to the override file and then make changes there directly. Such copying will cause problems in certain parts of system.

### Configuration with UCL

The *universal configuration language* is a common language for managing UNIX-style configuration files. FreeBSD uses UCL for core functions such as packaging system.

### FreeBSD Hardware

FreeBSD supports most RAID controllers and includes software to manage most of them. However people running UFS filesystem to use FreeBSD's RAID options rather than a hardware RAID controllers.

If you're using ZFS, the warnings against RAID controllers become "just don't". ZFS expects to have direct access to the disks. Using a RAID controller disables much of ZFS's self-healing and error-correction abilities.

### Hardware Requirements

Any server grade i386 system built this millennium can run FreeBSD. FreeBSD runs fine on hypervisors, such as VMware, Virtual Box, Xen and KVM. Legitimate cloud providers offer FreeBSD images and ISOs. FreeBSD runs just fine integrated `bhyve(8)` hypervisor and OpenBSD's `vmm(8)`. You can do a base install with 128M of RAM and 1GB of disk.

#### BIOS v/s EFI

FreeBSD boots just fine from EFI and using EFI permits FreeBSD to do some interesting things, like full-disk encryption.

### FreeBSD Filesystems

FreeBSD supports two major filesystems, UFS and ZFS. Which should you use ? That depends on what is the use case.

FreeBSD's Unix File System (UFS) is a direct descendent of filesystem that was shipped with 4.4 BSD and still under development. UFS's place as a primordial FreeBSD filesystem lets it extend throughout the kernel's virtual memory system through infrastructure created for UFS. UFS is designed to handle most common situations effectively while reliably supporting unusual configs.

ZFS was introduced by Solaris in 2005 and integrated into FreeBSD in 2007. Its youth seems to be a disadvantage, but it combines technologies and concepts that have been used for much longer. ZFS computes a checksum of every block of data or metadata and can use it for error correction. Storage is pooled, meaning that you can dynamically add more disks to an existing ZFS filesystem without recreating the filesystem.

Author resists using ZFS on host with less than 4GB of RAM and refuse to run it on less than 2GB of RAM. UFS servers small and embedded systems better than ZFS can.

ZFS makes a great storage system for a virtualization server, but it isn't necessarily right for virtual machines that use disk images. Many virtual machines don't get enough memory to effectively run ZFS. Additionally more than one KVM based virtualization system fail to migrate ZFS-based virtual machines.

UFS isn't perfect either. A power failure or system crash can damage a UFS filesystem. Repairing that filesystem takes time and system memory.

### Filesystem Encryption

FreeBSD supports two disk encryption systems : GEOM-Based  Disk Encryption (GBDE) and GELI.

### Disk Partitioning Methods

Older and smaller hardware uses master boot record (MBR) and is always limited to 2TB or smaller. Newer and larger hardware uses the more flexible and generally better GUID partition Tables (GPT) scheme.

FreeBSD manages both types of partition with `gpart(8)`

### Partitioning with UFS

At a minimum, separate your OS from your data. If this has multiple accounts, create a `/home` partition. If you're running a database, create partition for the database. Web servers should have another partition web data and another one for logs. 

### Multiple Hard Drives

If you have multiple drives you should use them for storage redundancy. If you're using ZFS, use a mirror or some sort of RAID-Z. If you are using UFS, FreeBSD supports software RAID.

### Swap Space

When FreeBSD uses up all the physical RAM, it can move information that's been sitting idle from memory into swap.

Well these days you will rarely run out of memory. 

The main use for swap on modern systems is to have a place to store a kernel memory dump should the system panic and crash. FreeBSD uses kernel minidumps, so they dump only kernel memory. A minidump is much smaller than a full dump: a host with 8GB RAM has an average minidump of size 250 MB. Provisioning a GB of swap per 10 GB of RAM should be sufficient for most situation.'

### Getting FreeBSD

Go to https://www.FreeBSD.org/ and look for GetFreeBSD section. download the stable release.

### Choosing Installation Images

You can choose between several different formats of FreeBSD installation media.

Mainly there are two kinds of installer!

- The first one contains enough to boot FreeBSD installer and bring up the network to download the OS files from FreeBSD mirror site.
- The second one already contains these files.

### Network Installs

If your network runs DHCP, the installer should just pick up your network configuration. If not, your FreeBSD host will need a valid network configuration. Before Installation gather this data

- A valid IP address and netmask
- The default gateway for your network
- The nameserver IP address