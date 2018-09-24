# Add Space to LVM Hardisk
this example using virtualbox to demonstrate step  
the main is to add new partition to lvm linux, expand the size so the partition become one greater partition

this using debian 8, with lvm default in installation

## 0. Add new Disk to VM or Physical Computer
![alt text](img/13-add-new-partition.png "Add new disk")

## 1. fdisk show new disk
this show  the new pyhsical disk read by computer  
```` 
$ fdisk -l 
````
![alt text](img/12-fdisk-show-raw-partition.png "the new disk read by computer")

## 2. fdisk create new partition
create new partition in /dev/sdb   
```` 
$ fdisk /dev/sdb
...
Command (m for help ): n
...
Select (default p): p
...
Partition Number (1-4, default 1): 1
First Sector ...: (Just Press Enter)
Last Sector  ...: (Just Press Enter)
...
Command (m for help ): t
Selected partition 1
Hex code ...: 8e
...
Command (m for help ): w

````
![alt text](img/11-fdisk-dev-sdb-create-new-partition-lvm.png "create new lvm partition in /dev/sdb1")

## 3. show new partition
show all partition in /dev/sd*   
```` 
$ ls -l /dev/sd*
````
![alt text](img/10-ls-l-show-partition.png "create new lvm partition in /dev/sdb1")

## 4. create new physical volume
change /dev/sdb1 to physical volume  
```` 
$ pvcreate /dev/sdb1
````
![alt text](img/09-pvcreate-new-partition.png "create new lvm partition in /dev/sdb1")

## 5. show physical volume
show all physical volume had create before   
```` 
$ pvdisplay
````
![alt text](img/08-pvdisplay-after-pvcreate.png "create new lvm partition in /dev/sdb1")

## 6. show volume group before
get the value  
VG Name **deb8-vg**  
VG Size **29.76 GB**  
Alloc PE **7618/29.76 GB**
Free PE **0 / 0**
```` 
$ vgdisplay
````
![alt text](img/07-vgdisplay-before.png "create new lvm partition in /dev/sdb1")

## 7. add partition /dev/sdb1 to Volume Group deb8-vg
show all partition in /dev/sd*   
```` 
$ vgextend -v deb8-vg /dev/sdb1
````
![alt text](img/06-vgextend-add-new-partition-to-vg.png "create new lvm partition in /dev/sdb1")

## 8. show volume group after vgextend
get the new value  
VG Name **deb8-vg**  
VG Size **59.75 GB**  
Alloc PE **7618/29.76 GB**  
Free PE **7679 / 30.00 GB**

```` 
$ vgdisplay
````
![alt text](img/05-vgdisplay-show-VG-Size-andFree-PE.png "create new lvm partition in /dev/sdb1")

## 9. get LV Path to add the space
get the value
LV Path **/dev/deb8-vg/root**
we will add space to this logical volume
```` 
$ lvdisplay
````
![alt text](img/04-lvdisplay-to-get-LV-PATH.png "lvdisplay to get LV PATH")

## 10. see the space before
value Size from /dev/dm-0 **28 GB**
```` 
$ df -h
````
![alt text](img/03-space-before.png "show space before")

## 11 add the free space
add the new space, value ``+7679`` get from **Free PE** in step ``8``
```` 
$ lvextend -l +7679 -v /dev/deb8-vg/root
````
![alt text](img/02-lvextend-command.png "create new lvm partition in /dev/sdb1")

## 12 show new size
the new Size add 30 GB become ``58GB`` 
```` 
$ df -h
````
![alt text](img/01-space-after.png "create new lvm partition in /dev/sdb1")