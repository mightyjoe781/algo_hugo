+++

Title = "Start Me up! The Boot Process"
weight = 4

+++

### Power-On

A computer needs to find and load its operating system. For many years BIOS provided the functionality but now its moving towards UEFI. Some hardware platform have console firmware  or bootroms that perform the same function.

### UEFI

UEFI searches for boot drive for a partition marked as a UEFI boot partition. The partition contains FAT fs with a specific directory and file layout. UEFI executes the file `/EFI/BOOT/BOOTX64.EFI`. This file might be a fancy multi-OS boot loader or might through you directly in OS, FreeBSD boot fires up the boot loader. `/boot/loader.efi`

### BIOS

It has enough brains to look for OS somewhere on disk. A BIOS searches for a disk partition marked active and then executes first section of that partition. For FreeBSD, the chunk is called as loader. Every FreeBSD system has a reference copy of loader as `/boot/loader`

Limitation in BIOS : boot loader must reside in specific section of disk, can't boot from disk greater than 2.2TB, target boot loader must be smaller than 512 KB ~ huge by 1980 Standard.

The installed loader is binary not a file that's why requires compilation on every change.

### The Loader

Loader brings up a menu to login : some of the options are

1.  Boot Multi User [Enter] : normal boot

2.  Boot Single User : Its minimal startup mode and very useful for damaged system especially when self inflicted. Like `rc.conf` fu** up.

   It loads the kernel and finds devices but doesn't automatically setup up your filesystem, start network, enable security or run any standard Unix Services.
   Note in single user mode root partition is mounted read only and no other disks are mounted.

   **UFS in Single User mode**

   To make all filesystems listen in filesystem table `/etc/fstab` usable, run

   ````
   fsck -p
   mount -o rw /
   mount -a
   ````

   `fsck(8)` : "cleans" the filesystem and confirms that they're internally consistent.

   Then we remount root filesystem read-write

   finally the -a flag activates every filesystem in `/etc/fstab`

   Note : if your system had NFS configured it might give u error in such cases consider mounting on UFS i.e. `mount -a -t ufs`. If there is trouble mounting using partition name, try mounting using disk name `mount /dev/ad0s1a`

   **ZIF in Single User Mode**

   ````
   zfs mount -a
   zfs set readonly=off zroot/ROOT/default
   ````

   ZFS already does all integrity tests.

   Commands that are available for use depend on partition which is mounted. Some basic commands are available on the root partition `/bin` and `/sbin` and are available in read-only mode too. While some requires file system to be in read-write enabled because they reside in `/usr`.

   Say you messed up you libraries , then nothing will work. but FreeBSD provide a statically linked version of many core utilities in `/rescue` directory.

   **Uses for Single User Mode**

   we can reset root password

   ```
   passwd
   ```

   or there is a typo in `fstab` file you can fix that.

   if there is a program that panics the system on boot and your need to stop that program from starting again, either edit `/etc/rc.conf` to disable or set the permission on the startup script to remove its execution.

   ````
   chmod a-x /usr/local/etc/rc.d/program.sh
   ````

3.  Escape to loader prompt : loader has a command line interpreter that u can use to modify you boot exactly you want.

   Note this is not full OS prompt , so expect only basic functionality. "?" for help.

   **Viewing Disks**

   TO view disks that loader has detected we use `lsdev`

   ````
   OK lsdev
   cd devices:
   	disk devices:
   disk0:	BIOS drive C (33333333 X 512)
   	disk0p1: FreeBSD boot
   	disk0p2: FreeBSD swap
   	disk0p3: FreeBSD ZFS
   zfs devices:
   	zfs:zroot
   ````

   Note : GPT partitions are denoted by `pn` where n is a number.

   **Loader Variables**

   loader has variables set within the kernel and by a config file.

   use `show` command to see these variables. These values includes low level kernel tunables  and information gleaned from the hardware BIOS or UEFI

   we  can change a value for a single boot using `set command`

   ```
   set console=comconsole
   ```

   `reboot` : to start again.

   `boot`: after changing the loader config and u wish to continue boot

   Now to make these changes you did permanent using a config file `/boot/loader.conf`

   Sometimes you will see values with just `""` for e.g. `kern.nfub=""` this mean loader lets kernel load its value normally, don't change it unless you want it.

   Some important variable

   - `boot_verbose="NO"` : print more summary during boot.
   - `autoboot_delay="10"` : that timer runs for auto selecting during boot
   - `beastie_disable="NO"` : controls the appearance of boot menu
   - `loader_logo="fbsdbw"` : lets you change logo that appears on side of boot menu

### Boot Options

A host can have multiple kernel in its `/boot` directory. Hitting kernel option tells the loader to cycle between available options or you can use loader.conf

```bash
KERNERLS="kernel kernel.old kernel.GENERIC"
```

Menu recognizes only kernels in directories beginning with `/boot/kernal`. If you want to load kernel from `/boot/smk` you'll need to load it from loader prompt.

- Load System defaults : you messed up your config by this u can at least login in single user mode and fix loader.conf
- ACPI : Advanced Configuration and Power Interface, and Intel/Toshiba/MS standard for hardware configuration. It replaces and subsumes a whole but of obscure standards. ACPI has been a standard for many years now, but if a particular hardware is having trouble running FreeBSD, you can turn it off and see what happens.
- Safe Mode : It turns off DMA and write caching on hard disks, limiting their speed but increasing their reliability. It turns off ACPI. 32-bit systems disable SMP. USB keyboards no longer work in safe mode. This option is useful for debugging older hardware.
- Verbose : FreeBSD kernel probes every piece of hardware as it boots. Most of info is not useful to displayed. When in verbose mode, kernel prints everything and makes it available in `/var/run/dmesg.boot` after boot.

### Multiuser Startup

This is standard operating mode for a Unix-like OS. When FreeBSD finishes probing the hardware and attaching all device drivers properly it runs the shell script `/etc/rc`. This script mounts all filesystems, brings up the network interfaces, configure device nodes, identifies available shared libraries, and does all the other work need to be done to get the system ready.

`/etc/rc` is very flexible in terms that shell script is responsible for specific aspect of system.

The `/etc/rc` script is controlled by the files `/etc/defaults/rc.conf` and `/etc/rc.conf`.

`/etc/defaults/rc.conf` is a huge file and contains quite a few variables, frequently called `knobs` or *tunables*. 

To change `rc.conf` settings, you can use a text editor or `sysrc(8)` only that `sysrc` provides automation for setting up many servers.

```
sysrc -a
```

to make sysrc enable a service, give it a variable name,

```
sysrc rc_startmsgs=NO
```

we can remove it using

```
sysrc -x rc_startmsgs
```

**/etc/rc.conf.d**

If you are using server configuration system such as Puppet or Ansible, you might trust copying entire files more than editing them. Use `etc/rc.conf.d` files to enable services through such tools.

**Startup options**

- `rc_debug="NO"`
- `rc_info="NO"`
- `rc_startmsgs="NO"`

**Filesystem Options**

FreeBSD can use memory as a filesystem, One common usecase is to make `/tmp` really fast by using memory rather that a hard drive as its backend.

````
tmpmfs="AUTO"
tmpsize="20m"
tmpmfs_flags="-S"
````

Another feature of FreeBSD filesystem is its encrypted partition. FreeBSD has 2 of them GEOM and GELI. GEOM is used for military grade protection, while GELI can be used by normal users.

````
geli_devices=""
geli_tries=""
geli_default_flags=""
geli_autodetach="YES"
````

By default FreeBSD mounts root partition `read-write` upon achieving multiuser mode. If you wanna run in read only mode you can do this. Note some software might not work as expected.

`root_rw_mount="YES"`

We check consistency of filesystem using `fsck_y_enable="NO"`. while kernel also fixes some minor issues using background fsck.

**Miscellaneous Network Daemons**

FreeBSD includes many smaller programs, or daemons, that run in the background to provide specific services. Logs are a Good Thing.

```
syslogd_enable="YES"
```

Another popular daemon is `inetd(8)`, the server for small network services.

```
inetd_enable="NO"
```

Most systems use the Secure Shell (SSH) daemon for remote logins

```
sshd_enable="NO"
```

better use configs in `/etc/ssh`.

`sshd_flags=""`

**Network Options**

Some of options are

````
hostname=""
pf_enable="NO"
````

FreeBSD includes a few different integrated firewall packages. Packet Filters can be enable/disabled in rc.conf

You might be interested in failed attempts to connect to your system over a network. This will help detect port scans and network intrusion. Set to 1 for log failed connection attempts.

```
log_in_vain="0"
```

Routers use ICMP redirects to inform client machines of proper gateways for particular routes. While this is completely legitimate on some networks intruders can use this to capture data. Ask your network administrator if you need this option.

```bash
icmp_drop_redirect="NO"
```

To get on network, you'll need to assign each interface an IP address.

```
ifconfig_em0="inet 172.18.11.3 netmask 255.255.254.0"
```

If network uses DHCP set it to `"dhcp"`

similarly we can add alias for network cards.

**Network Routing Options**

While assigning a valid IP address to a network interface gets you on the local network, a default router will give you access to everything beyond your LAN.

```
defaultrouter=""
```

Network control devices, such as firewalls, must pass traffic between different interfaces. While BSD won't do this by default, it's simple to enable.

```
gateway_enable="NO"
```

**Console Options**

These are mostly related how to control your Keypad, monitor, etc.

```
keymap="NO" or keymap="us-dvorak"
```

FreeBSD turn monitor dark when keyboard is idle for a time specific in the `blanktime` knob.

we can also configure fonts by choosing one from `/usr/share/syscons/fonts`

````
font8x16="NO"
font8x16="NO"
font8x16="YES"
````

FreeBSD can detect mouse on console even without any GUI. Just enable them in rc.conf

````
moused_enable="NO"
moused_type="AUTO"
````

You can also adjust display on your monitor according to your needs. You can get a full list of different options in man `vidcontrols(1)`.

````
allscreens_flags=""
````

**Other Options**

- Printer daemon `lpd(8)` : `lpd_enable="NO"`

- `sendmail(8)` daemon manages transmission and receipt of email between systems.

  ````
  sendmail_enable="NO"
  sendmail_submit_enable="YES"
  ````

- Enabling linux compatibility layer. : `linux_enable="NO"`

- A vital part of Unix-like OS is shared libraries. You can control where FreeBSD looks for shared libraries. Although setting is usually quite adequate, but if you find yourself regularly setting up `LD_LIBRARY_PATH` env variable for your users, use this

  ```
  ldconfig_paths="/usr/lib/usr/local/lib"
  ```

- FreeBSD has a profile system that allows admins to control basic system features.

  ````
  kern_securelevel_enable="NO"
  kern_securelevel="-1"
  ````

  

**The rc.d Startup System**

FreeBSD bridges gap between single user and multi user using `etc/rc`. This script reads config at two files, user and default and runs a collection of other scripts based on what it finds there.

For example if you have enable network time daemon `/etc/rc` runs a script for starting services.

These scripts live in `/etc/rc.d` and `/usr/local/etc/rc.d/`. Control these scripts with `service(8)`.

**The service(8) command**

Listing and Identifying Enable Services. Below commands print them in order they'll run at boot.

```
service -e
```

General Syntax : `service name command`

For example to restart `sshd(8)` service we do something like this : `service sshd describe`

Ok cool we confirmed that its a SSH daemon now : `service sshd restart`

**System Shutdown**

FreeBSD makes rc.d startup system do double duty, it also handles shutting down all those programs properly.

System uses this script to properly turn off programs. `/etc/rc.shutdown`. Some programs like sshd won't care if u terminate abruptly but a database might not be happy with abrupt kill signal.

**Serial Console**

All this console stuff is nice but when FreeBSD is in a colocation facility on another continent, you can't just walk up and type stuff :D. How do you reset the machine remotely when it won't respond to the network. Using serial console to redirect the computer's keyboard and video to the serial port instead of keyboard and monitor helps with all of these problems.

Serial Consoles can be physical, such as a serial port on the back of a computer.

**Serial Protocol**

Some of the first computer consoles were serial ports connected to teletypes.

Serial protocols also include bunch of settings beyond their speed. It's possible to muck with them, but the standard settings of 8 data bits, no parity, and 1 stop bit are the most widely used. You can't change these in FreeBSD without recompiling the kernel, so don't' much with them.

