From time to time it is necesarry to securely erase a disk.
This could be a disk I want to sell, or a second hand disk I have bought.

### Finding the disk

First thing we need to do is find out the device name of the disk

We do this with the "lsblk command".
In this example, I know I mounted a 120G disk in to my USB Harddrive enclosure, and attached it.

```bash
[ansibleuser@testansible ~]$ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda           8:0    0    32G  0 disk 
├─sda1        8:1    0     1G  0 part /boot
└─sda2        8:2    0    31G  0 part 
  ├─vg-root 253:0    0    29G  0 lvm  /
  └─vg-swap 253:1    0     2G  0 lvm  [SWAP]
sdb           8:16   0 119.2G  0 disk 
├─sdb1        8:17   0   524M  0 part 
├─sdb2        8:18   0  15.8G  0 part 
└─sdb3        8:19   0 102.9G  0 part 
sr0          11:0    1   1.5G  0 rom  
```

With the knowledge of the disk size, I can deduce that the disk I want to erase i "sdb"

### Erasing the disk with dd

The first tool we can use the the program called "dd"

From man:

```bash
NAME
       dd - convert and copy a file
```

This program is also sometimes affectionally called "Disk Destroyer". It has earned this name by alot of people accidently copying over their system drive, because they made a typing error.

```bash
tldr dd

  dd

  Convert and copy a file.
  More information: https://www.gnu.org/software/coreutils/dd.

  - Make a bootable USB drive from an isohybrid file (such like `archlinux-xxx.iso`) and show the progress:
    dd if=path/to/file.iso of=/dev/usb_drive status=progress
  - Clone a drive to another drive with 4 MiB block, ignore error and show the progress:
    dd if=/dev/source_drive of=/dev/dest_drive bs=4M conv=noerror status=progress
  - Generate a file of 100 random bytes by using kernel random driver:
    dd if=/dev/urandom of=path/to/random_file bs=100 count=1
  - Benchmark the write performance of a disk:
    dd if=/dev/zero of=path/to/file_1GB bs=1024 count=1000000
  - Generate a system backup into an IMG file and show the progress:
    dd if=/dev/drive_device of=path/to/file.img status=progress
  - Restore a drive from an IMG file and show the progress:
    dd if=path/to/file.img of=/dev/drive_device status=progress
  - Check the progress of an ongoing dd operation (run this command from another shell):
    kill -USR1 $(pgrep ^dd)
```

One of its many uses, is to Zero a disk. we can do this by copying the contents of /dev/zero to the disk ( /dev/zero continuously generates Zeros)

```bash
[ansibleuser@testansible ~]$ sudo dd if=/dev/zero of=/dev/sdb bs=10M status=progress
127999672320 bytes (128 GB, 119 GiB) copied, 722 s, 177 MB/s
dd: error writing '/dev/sdb': No space left on device
12211+0 records in
12210+0 records out
128035676160 bytes (128 GB, 119 GiB) copied, 724.058 s, 177 MB/s
```

In the above example we can see that dd has written 128GB of zeroes to the disk. This means that it has effectively overwritten every sector of the disk with zeroes.

##### Command in parts:

- dd - the program itself
- if=/dev/zero - if stands for "input file" (remember that everything is a file in Linux). It takes the contents of /dev/zero as input for our command.
- of=/dev/sdb - of stands for "output file". here we designate the file /dev/sdb (which is our harddrive, that we identified earlier)
- bs=10M - We tell dd to write in blocks of 10MB at a time. This speeds up the process by a significant margin.
- status=progress - Gives us a nice status, that updates to tell us how long the command has run.

##### Can it be done even more securely with dd?

overwriting with zeroes is pretty secure, but if you realy want to be even more sure, use /dev/random instead. This takes longer, but will make data recovery even harder.

### Erasing the disk with shred.

The second tool we can use is called "shred" (imagine a paper shredder)

From man:

```bash
NAME
       shred - overwrite a file to hide its contents, and optionally delete it
```

This program can be used both to securely erase individual files and hard drives.

for this example we will use it to erase the disk (sdb), we located above.

shred can even overwrite several times if need be

```bash
tldr shred

  shred

  Overwrite files to securely delete data.
  More information: https://www.gnu.org/software/coreutils/shred.

  - Overwrite a file:
    shred path/to/file
  - Overwrite a file, leaving zeroes instead of random data:
    shred --zero path/to/file
  - Overwrite a file 25 times:
    shred -n25 path/to/file
  - Overwrite a file and remove it:
    shred --remove path/to/file
```

```bash
[ansibleuser@testansible ~]$ sudo shred -vfz /dev/sdb
shred: /dev/sdb: pass 1/4 (random)...
shred: /dev/sdb: pass 1/4 (random)...410MiB/466GiB 0%
shred: /dev/sdb: pass 1/4 (random)...940MiB/466GiB 0%
shred: /dev/sdb: pass 1/4 (random)...1.4GiB/466GiB 0%

...
...
...
shred: /dev/sdb: pass 4/4 (000000)...462GiB/466GiB 99%
shred: /dev/sdb: pass 4/4 (000000)...463GiB/466GiB 99%
shred: /dev/sdb: pass 4/4 (000000)...464GiB/466GiB 99%
shred: /dev/sdb: pass 4/4 (000000)...465GiB/466GiB 99%
shred: /dev/sdb: pass 4/4 (000000)...466GiB/466GiB 100%
```

##### Command in parts:

- shred - the program itself.
- -v - Verbose = show progress
- -f - force = change permissions to allow writing if necessary
- -z - zero = add a final overwrite with zeros to hide shredding
