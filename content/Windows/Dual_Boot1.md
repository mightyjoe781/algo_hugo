+++

Title = "Partitions Records and Bootloaders"
weight = 4

+++

#### MBR 

 A master boot record (MBR) is special type of boot sector at very beginning of partitioned computer storage device. MBR holds information on how the logical partitions, containing file systems, are organized on that medium.

It also contained executable code to function as a loader for installed OS usually by passing the control over to the loader's second page or in conjunction with each partitions volume boot record.

MBR code is usually referred as boot loader.

MBR limits maximum addressable storage space to 2 TB that's why its superseded by GUID partition Table (GPT).

#### GPT 

 It is standard layout of partition table on a physical hard disk, using globally unique identifiers (GUID). It forms a part of UEFI standard, it is also used on some BIOS systems because of limitations of MBR tables.

MBR based partition table insert the partitioning information for (usually) 4 "primary" partition in the master boot record. In a GPT first sector of the disk is reserved for a "protective MBR" such that booting a BIOS based computer from a GPT disk is supported, but the boot loader and O/S must both be GPT-aware. GPT header begins on second logical block of device.

#### MBR vs GPT 

A GPT disk supports up to 128 partitions while MBR is limited to 4 primary partition.

GPT keeps backup of partition table at the end of disk and provides greater reliability due to replication and cyclical redundancy check protection of partition table.

#### BIOS

In IBM PC compatible computer , the Basic I/O System (BIOS), is de facto standard defining a firmware interface. BIOS software is built into the PC, and is the first software run by a PC when powered on.

Fundamental purposes of BIOS are

- Initialize and test hardware components
- Load a bootloader or an operating system from mass memory device.
- Abstraction layer for hardware's
- Modern O/S doesn't rely on hardware abstraction rather access hardware components directly.

#### UEFI

The Unified Extensible Firmware Interface is a specification that defines a software interface between an O/S and platform firmware. UEFI is a replacement for BIOS.

In practice, most UEFI images provide legacy support for BIOS services. UEFI can support remote diagnostic and repair of computers, even without another O/S.

The original EFI specification was developed by Intel. Some of its practices and data formats mirror one from Windows.

This is maintained by UEFI forum committee.

#### BIOS vs. UEFI

UEFI enables batter use of bigger hard drives. UEFI supports both MBR and GPT

UEFI may be faster than BIOS along with tweaks and optimizations that can boot system very quickly.

UEFI has room for more useful and usable features that could ever be crammed into BIOS. Among these are cryptography, network authentication, support for extensions stored on non-volatile media, an integrated boot manager, and a shell environment for running other EFI application such as diagnostic utilities or flash updates.

However UEFI is still not widespread. Though major hardware companies have switched over almost exclusively UEFI use.

##### MBR vs. GPT and BIOS vs. UEFI

Usually MBR+BIOS and GPT  + UEFI go hand in hand! This is compulsory for windows while its optional for Linux.

#### Secure Boot

This feature was actually introduced by Microsoft quite early in days as a method to acquire monopoly over hardware (x-86 and x-64 and arm platforms) as it was afraid GNU/Linux can take over its hardware share as Microsoft never really produced native hardware for their OS like Apple does.

Now wait why does it exists ? Because Microsoft was part of UEFI committee at the time so it was included in specification.

Secure Boot provides a way to ensure that only authorized EFI binaries are loaded by a computer's firmware. This ensures that no malicious code can run before the OS is loaded.
Considering the Sheer amount of Linux Distros and their methodology of implementation varies too much that's why getting binaries signed by ( Microsoft) is quite hard!

<u>Majority of Linux Distro's require Secure Boot to be disabled!</u>

Exceptions to this is Ubuntu and Fedora, as they have managed to get their binaries signed by Microsoft.

[Excellent Resource on Managing EFI Secure Boot](https://www.rodsbooks.com/efi-bootloaders/secureboot.html)



In some system secure boot option is locked until you set supervisor password.

In case you forget your supervisor password, there are 2 ways to fix this

- remove the cell in your motherboard
- enter password three times wrong and it will spit a code insert that code here https://bios-pw.org/

