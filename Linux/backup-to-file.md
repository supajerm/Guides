To do the backup, be sure to mount your drive first.  
(Example given mounts to /mnt/usbbackup on the device)

List Disks

```
$ fdisk -l
```

The output will be something like below. In this case, the OS (SD card) is on `/dev/mmcblk0` (ignore the partitions). 

```
[root@pikvm ~]# fdisk -l
Disk /dev/mmcblk0: 29.72 GiB, 31914983424 bytes, 62333952 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xba98450d

Device         Boot    Start      End  Sectors  Size Id Type
/dev/mmcblk0p1             1   524287   524287  256M  c W95 FAT32 (LBA)
/dev/mmcblk0p2        524288 12500000 11975713  5.7G 83 Linux
/dev/mmcblk0p3      12500001 62333951 49833951 23.8G 83 Linux


Disk /dev/sda: 28.65 GiB, 30765219840 bytes, 60088320 sectors
Disk model:  SanDisk 3.2Gen1
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x00000000
```

Execute the following to copy the SD card to an image file.

```sh
$ sudo dd if=/dev/mmcblk0 of=/mnt/usbbackup/pikvm.bkup.img status=progress
```

The output will be something like below. In this case, the OS (SD card) is on `/dev/mmcblk0` (ignore the partitions). 
```
[root@pikvm usbbackup]# sudo dd if=/dev/mmcblk0 of=/mnt/usbbackup/pikvm.bkup.img status=progress
4283486720 bytes (4.3 GB, 4.0 GiB) copied, 202 s, 21.2 MB/s
dd: writing to '/mnt/usbbackup/pikvm.bkup.img': File too large
8388608+0 records in
8388607+1 records out
4294967295 bytes (4.3 GB, 4.0 GiB) copied, 202.303 s, 21.2 MB/s
```

To restore from the backup.
```sh
$ sudo dd if=/mnt/usbbackup/pikvm.bkup.img of=/dev/mmcblk0
```

***

If the size is going to be to large, you can have DD compress the output as well
```sh
$ sudo dd if=/dev/mmcblk0 | gzip -c > /mnt/usbbackup/pikvm.bkup.img.gz
```

## And to restore from compressed.
```sh
$ sudo gunzip -c /mnt/usbbackup/pikvm.bkup.img.gz | dd of=/dev/mmcblk0
```

