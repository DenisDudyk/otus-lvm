sudo su
yum install xfsdump -y

lsblk
```
NAME                    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                       8:0    0   40G  0 disk 
├─sda1                    8:1    0    1M  0 part 
├─sda2                    8:2    0    1G  0 part /boot
└─sda3                    8:3    0   39G  0 part 
  ├─VolGroup00-LogVol00 253:0    0 37.5G  0 lvm  /
  └─VolGroup00-LogVol01 253:1    0  1.5G  0 lvm  [SWAP]
sdb                       8:16   0   10G  0 disk 
sdc                       8:32   0    2G  0 disk 
sdd                       8:48   0    1G  0 disk 
sde                       8:64   0    1G  0 disk 
```

pvcreate /dev/sdb
```
Physical volume "/dev/sdb" successfully created.
```

vgcreate vg_tmp_root /dev/sdb
```
Volume group "vg_tmp_root" successfully created
```

lvcreate -n lv_tmp_root -L 8G vg_tmp_root
```
Logical volume "lv_tmp_root" created.
```

lsblk
```
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                         8:0    0   40G  0 disk 
├─sda1                      8:1    0    1M  0 part 
├─sda2                      8:2    0    1G  0 part /boot
└─sda3                      8:3    0   39G  0 part 
  ├─VolGroup00-LogVol00   253:0    0 37.5G  0 lvm  /
  └─VolGroup00-LogVol01   253:1    0  1.5G  0 lvm  [SWAP]
sdb                         8:16   0   10G  0 disk 
└─vg_tmp_root-lv_tmp_root 253:2    0    8G  0 lvm  
sdc                         8:32   0    2G  0 disk 
sdd                         8:48   0    1G  0 disk 
sde                         8:64   0    1G  0 disk 
```

mkfs.xfs /dev/vg_tmp_root/lv_tmp_root 
```
meta-data=/dev/vg_tmp_root/lv_tmp_root isize=512    agcount=4, agsize=524288 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=2097152, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```


mkdir /mnt/root_tmp/
mount /dev/vg_tmp_root/lv_tmp_root /mnt/root_tmp/

xfsdump -J - /dev/VolGroup00/LogVol00 | xfsrestore -J - /mnt/root_tmp/
```
xfsdump: using file dump (drive_simple) strategy
xfsdump: version 3.1.7 (dump format 3.0)
............
xfsrestore: Restore Status: SUCCESS
```

ls /mnt/root_tmp/
```
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  vagrant  var
```

for i in /proc/ /sys/ /dev/ /run/ /boot/; do mount --bind $i /mnt/root_tmp$i; done
chroot /mnt/root_tmp/

> Новый загрузчик

grub2-mkconfig -o /boot/grub2/grub.config
```
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.10.0-862.2.3.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-862.2.3.el7.x86_64.img
done
```

> Обновим образы загрузки

cd /boot ; for i in `ls initramfs-*img`; do dracut -v $i `echo $i|sed "s/initramfs-//g; s/.img//g"` --force; done
```
Executing: /sbin/dracut -v initramfs-3.10.0-862.2.3.el7.x86_64.img 3.10.0-862.2.3.el7.x86_64 --force
dracut module 'busybox' will not be installed, because command 'busybox' could not be found!
dracut module 'crypt' will not be installed, because command 'cryptsetup' could not be found!
....
*** Creating image file ***
*** Creating image file done ***
*** Creating initramfs image file '/boot/initramfs-3.10.0-862.2.3.el7.x86_64.img' done ***
```

> root=/dev/mapper/VolGroup00-LogVol00 зменить на /dev/mapper/vg_tmp_root-lv_tmp_root
> rd.lvm.lv=VolGroup00/LogVol00 зменить на rd.lvm.lv=vg_tmp_root/lv_tmp_root

vi /boot/grub2/grub.cfg


exit 
reboot

_______________________________________________________________________________

sudo su

lsblk
```
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                         8:0    0   40G  0 disk 
├─sda1                      8:1    0    1M  0 part 
├─sda2                      8:2    0    1G  0 part /boot
└─sda3                      8:3    0   39G  0 part 
  ├─VolGroup00-LogVol01   253:1    0  1.5G  0 lvm  [SWAP]
  └─VolGroup00-LogVol00   253:2    0 37.5G  0 lvm  
sdb                         8:16   0   10G  0 disk 
└─vg_tmp_root-lv_tmp_root 253:0    0    8G  0 lvm  /
sdc                         8:32   0    2G  0 disk 
sdd                         8:48   0    1G  0 disk 
sde                         8:64   0    1G  0 disk 
```

lvremove /dev/VolGroup00/LogVol00 
```
Do you really want to remove active logical volume VolGroup00/LogVol00? [y/n]: y
Logical volume "LogVol00" successfully removed
```

lvcreate -n LogVol00 -L 8G VolGroup00
```
WARNING: xfs signature detected on /dev/VolGroup00/LogVol00 at offset 0. Wipe it? [y/n]: y
  Wiping xfs signature on /dev/VolGroup00/LogVol00.
  Logical volume "LogVol00" created.
```

mkfs.xfs /dev/VolGroup00/LogVol00
```
meta-data=/dev/VolGroup00/LogVol00 isize=512    agcount=4, agsize=524288 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=2097152, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```

mount /dev/VolGroup00/LogVol00 /mnt/root_tmp/

xfsdump -J - /dev/vg_tmp_root/lv_tmp_root | xfsrestore -J - /mnt/root_tmp/
```
xfsrestore: using file dump (drive_simple) strategy
xfsrestore: version 3.1.7 (dump format 3.0)
xfsdump: using file dump (drive_simple) strategy
........................
xfsdump: dump complete: 12 seconds elapsed
xfsdump: Dump Status: SUCCESS
xfsrestore: restore complete: 12 seconds elapsed
xfsrestore: Restore Status: SUCCESS
```

for i in /proc/ /sys/ /dev/ /run/ /boot/; do mount --bind $i /mnt/root_tmp$i; done

chroot /mnt/root_tmp/

grub2-mkconfig -o /boot/grub2/grub.cfg
```
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.10.0-862.2.3.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-862.2.3.el7.x86_64.img
done
```

cd /boot ; for i in `ls initramfs-*img`; do dracut -v $i `echo $i|sed "s/initramfs-//g; s/.img//g"` --force; done
```
Executing: /sbin/dracut -v initramfs-3.10.0-862.2.3.el7.x86_64.img 3.10.0-862.2.3.el7.x86_64 --force
dracut module 'busybox' will not be installed, because command 'busybox' could not be found!
dracut module 'crypt' will not be installed, because command 'cryptsetup' could not be found!
dracut module 'dmraid' will not be installed, because command 'dmraid' could not be found!
.................................................
*** Store current command line parameters ***
*** Creating image file ***
*** Creating image file done ***
*** Creating initramfs image file '/boot/initramfs-3.10.0-862.2.3.el7.x86_64.img' done ***
```

> Проверим запись, должны быть root=/dev/mapper/VolGroup00-LogVol00 и rd.lvm.lv=VolGroup00/LogVol00

vi /boot/grub2/grub.cfg


exit
reboot

_______________________________________________________________________________

lsblk
```
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                         8:0    0   40G  0 disk 
├─sda1                      8:1    0    1M  0 part 
├─sda2                      8:2    0    1G  0 part /boot
└─sda3                      8:3    0   39G  0 part 
  ├─VolGroup00-LogVol00   253:0    0    8G  0 lvm  /
  └─VolGroup00-LogVol01   253:1    0  1.5G  0 lvm  [SWAP]
sdb                         8:16   0   10G  0 disk 
└─vg_tmp_root-lv_tmp_root 253:2    0    8G  0 lvm  
sdc                         8:32   0    2G  0 disk 
sdd                         8:48   0    1G  0 disk 
sde                         8:64   0    1G  0 disk 
```
sudo su

lvremove /dev/vg_tmp_root/lv_tmp_root
```
Do you really want to remove active logical volume vg_tmp_root/lv_tmp_root? [y/n]: y
Logical volume "lv_tmp_root" successfully removed
```

vgremove vg_tmp_root
```
Volume group "vg_tmp_root" successfully removed
```

pvremove /dev/sdb
```
Labels on physical volume "/dev/sdb" successfully wiped.
```
