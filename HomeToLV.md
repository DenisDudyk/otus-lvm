lsblk

```
NAME                    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                       8:0    0   40G  0 disk 
├─sda1                    8:1    0    1M  0 part 
├─sda2                    8:2    0    1G  0 part /boot
└─sda3                    8:3    0   39G  0 part 
  ├─VolGroup00-LogVol00 253:0    0    8G  0 lvm  /
  └─VolGroup00-LogVol01 253:1    0  1.5G  0 lvm  [SWAP]
sdb                       8:16   0   10G  0 disk 
sdc                       8:32   0    2G  0 disk 
sdd                       8:48   0    1G  0 disk 
sde                       8:64   0    1G  0 disk 
```

sudo su

lvcreate -n home -L 10G VolGroup00

mkfs.ext4 /dev/VolGroup00/home 

mkdir /mnt/home_tmp

mount /dev/VolGroup00/home /mnt/home_tmp/

cp -av /home/* /mnt/home_tmp/

umount /mnt/home_tmp/

rm -rf /home/*

mount /dev/VolGroup00/home /home/

lsblk

```
NAME                    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                       8:0    0   40G  0 disk 
├─sda1                    8:1    0    1M  0 part 
├─sda2                    8:2    0    1G  0 part /boot
└─sda3                    8:3    0   39G  0 part 
  ├─VolGroup00-LogVol00 253:0    0    8G  0 lvm  /
  ├─VolGroup00-LogVol01 253:1    0  1.5G  0 lvm  [SWAP]
  └─VolGroup00-home     253:2    0   10G  0 lvm  /home
sdb                       8:16   0   10G  0 disk 
sdc                       8:32   0    2G  0 disk 
sdd                       8:48   0    1G  0 disk 
sde 
```

echo "/dev/VolGroup00/home /home/ ext4 defaults 0 2" >> /etc/fstab

> Проверим

umount /home/

ls -la /home/

```
total 0
drwxr-xr-x.  2 root root   6 Oct 13 17:59 .
drwxr-xr-x. 18 root root 239 Oct 13 16:56 ..
```

mount -a

ls -la /home/

```
total 8
drwxr-xr-x.  3 root    root    4096 Oct 13 17:49 .
drwxr-xr-x. 18 root    root     239 Oct 13 16:56 ..
drwx------.  3 vagrant vagrant 4096 May 12  2018 vagrant
```
