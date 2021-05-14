+++

Title = "diskpart"
weight = 2

+++



#### Diskpart

Diskpart command interpreter helps managing the computer's drive (disk partitions, volumes, or virtual hard disks).

In case you have corrupted a disk and wish  to completely clean the disk or wish to create a windows bootable drive.

Step : Selecting a correct disk (Important : selecting wrong disk can be disastrous) 

Run `cmd` as an administrator

```power
diskpart
list disk
```

output should be a table of disks, carefully select disk you wish to clean

````powershell
select disk 1
list disk
````

This time disk1 should have an asterisk (*) next to it representing that its selected disk

````power
clean
create partition primary
format fs=ntfs label="smk" quick
````

(Optional) This should make your disk available in explorer! In case it doesn't you can add a custom mount letter

````power
assign letter=G
````

(Optional) Now disk should be visible in explorer menu.

- We can make disk windows bootable disk by marking it as active

```power
active
```

