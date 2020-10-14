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
sde                       8:64   0    1G  0 disk 
```

sudo su

> Зеркальрование на два диска

pvcreate /dev/sdc /dev/sdd /dev/sde

vgcreate vg_mirror /dev/sdc /dev/sdd /dev/sde

lvcreate -m 2 -L 900M -n lv_mirror vg_mirror

mkfs.ext4 /dev/vg_mirror/lv_mirror

> Перенесем содердимое var 

mkdir /mnt/var_tmp

mount /dev/vg_mirror/lv_mirror /mnt/var_tmp/

cp -av /var/* /mnt/var_tmp/

> Удалим даныне и смонтируем новый источник в var

rm -rf /var/*

umount /mnt/var_tmp/

mount /dev/vg_mirror/lv_mirror /var/

> Пропишем монтирование при старте системы

echo "/dev/mapper/vg_mirror-lv_mirror /var/ ext4 defaults 0 2" >> /etc/fstab

> Проверим

umount /var/

ls -la /var/

```
total 4
drwxr-xr-x.  3 root root  33 Oct 14 08:39 .
drwxr-xr-x. 18 root root 239 Oct 13 16:56 ..
drwxrwxrwt.  2 root root   6 Oct 14 08:38 tmp
-rw-r--r--.  1 root root 163 May 12  2018 .updated

```

mount -a

ls -la /var/

```
total 84
drwxr-xr-x. 19 root root  4096 Oct 14 08:38 .
drwxr-xr-x. 18 root root   239 Oct 13 16:56 ..
drwxr-xr-x.  2 root root  4096 Apr 11  2018 adm
drwxr-xr-x.  5 root root  4096 May 12  2018 cache
drwxr-xr-x.  3 root root  4096 May 12  2018 db
drwxr-xr-x.  3 root root  4096 May 12  2018 empty
drwxr-xr-x.  2 root root  4096 Apr 11  2018 games
drwxr-xr-x.  2 root root  4096 Apr 11  2018 gopher
drwxr-xr-x.  3 root root  4096 May 12  2018 kerberos
drwxr-xr-x. 28 root root  4096 Oct 13 16:56 lib
drwxr-xr-x.  2 root root  4096 Apr 11  2018 local
lrwxrwxrwx.  1 root root    11 Oct 13 16:56 lock -> ../run/lock
drwxr-xr-x.  8 root root  4096 Oct 14 08:07 log
drwx------.  2 root root 16384 Oct 14 08:36 lost+found
lrwxrwxrwx.  1 root root    10 Oct 13 16:56 mail -> spool/mail
drwxr-xr-x.  2 root root  4096 Apr 11  2018 nis
drwxr-xr-x.  2 root root  4096 Apr 11  2018 opt
drwxr-xr-x.  2 root root  4096 Apr 11  2018 preserve
lrwxrwxrwx.  1 root root     6 Oct 13 16:56 run -> ../run
drwxr-xr-x.  8 root root  4096 May 12  2018 spool
drwxrwxrwt.  6 root root  4096 Oct 14 08:08 tmp
drwxr-xr-x.  2 root root  4096 Apr 11  2018 yp
```

> Итог

lsblk

```
sda                              8:0    0   40G  0 disk 
├─sda1                           8:1    0    1M  0 part 
├─sda2                           8:2    0    1G  0 part /boot
└─sda3                           8:3    0   39G  0 part 
  ├─VolGroup00-LogVol00        253:0    0    8G  0 lvm  /
  ├─VolGroup00-LogVol01        253:1    0  1.5G  0 lvm  [SWAP]
  └─VolGroup00-home            253:2    0   10G  0 lvm  /home
sdb                              8:16   0   10G  0 disk 
sdc                              8:32   0    2G  0 disk 
├─vg_mirror-lv_mirror_rmeta_2  253:7    0    4M  0 lvm  
│ └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
└─vg_mirror-lv_mirror_rimage_2 253:8    0  900M  0 lvm  
  └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
sdd                              8:48   0    1G  0 disk 
├─vg_mirror-lv_mirror_rmeta_0  253:3    0    4M  0 lvm  
│ └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
└─vg_mirror-lv_mirror_rimage_0 253:4    0  900M  0 lvm  
  └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
sde                              8:64   0    1G  0 disk 
├─vg_mirror-lv_mirror_rmeta_1  253:5    0    4M  0 lvm  
│ └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
└─vg_mirror-lv_mirror_rimage_1 253:6    0  900M  0 lvm  
  └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var

```

