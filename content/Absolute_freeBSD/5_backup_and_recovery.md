+++

Title = "Read this before you break something"
weight = 5

+++

### System Backups

You need a system backup only if you care about your data. Online backups can easily be damaged or destroyed by whatever ruins the live server. Proper backups are stored safely offline. Tools like rsync(1), and even ZFS replication, don't create actual backups, they create convenient online copies.

A complete backup/restore operation requires a tape drive and media.

**Backup Tapes**

FreeBSD supports SCSI and USB tape drives.

Once physically installed your tape drive, you need to configure it so FreeBSD can recognize it. The simples way is to check `/var/run/dmesg.boot` and search for `sa` devices.

**Tape Drive Device Nodes, Rewinding, and Ejecting**

Tape is a linear storage medium. Each section holds piece of data. As with many Unix devices with decades worth of history, the way you access tape drive controls how it behaves.

Normal tape drives has 3 modes. `esa, nsa, sa`

**THE $TAPE variable**

Many programs assume that your tape drive is `/dev/sa0` but it isn't always the case.

**Tape Status with mt(1)**

now since you know how to find the tape drive, lets perform common option such as rewinding, retensioning, erasing, and so on.

```
mt status
```

### BSD tar(1)

The most popular tool for backing up system to tape is `tar(1)`. Tar is short for tape achiever - it's written literally for backup.

Backup file containing tarred files is called as `tarball`

Note : freeBSD uses a specific version of tar called `bsdtar`.

### tar Modes

**Create an Archive**

```
tar -c /
```

**List Archive Contents** : list all the files in tar

```
tar -t
```

**Extract Files from a Backup** : Tar extracts files in your current location if you want to overwrite the existing /etc directory of your system with files from your backup.

```
cd /home/smk
tar -x etc
```

### **Other tar Features**

**Use Non-default storage**

the `-f` flag indicates destination folder or files

```
tar -c -f /dev/east0 /
```

or you can also backup to a tarball instead of using a tape

```
tar -cf bookbackup.tar /home/smk/af3o
```

Verbose : use `-v` flag.

### Compression

Note FreeBSD uses `libarchive(3)` for compressions library

**XZ Compression **

Its new hotness, and we can enable it using `-J`

**bzip Compression**

this is using `-j` flag , it uses more CPU time than gzip but these days CPU time is cheaper.

**gzip Compression**

this is done by using `-z` flag. These tarballs usually have `.tar.gz` or `.tgz` extensions.

**Permission Restore**

The `-p` flag restores the original permission on extracted files.

### **Recording what happened**

You can now backup entire systems in a single file. One such rarely command is `script(1)`. It logs everything as you type and everything that appears on on the screen.

To start *script(1)* just type script. when u type stop to stop recording. Script is located at 

```
script /home/smk/debug.txt
```

