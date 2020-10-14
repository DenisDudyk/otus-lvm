sudo su


lsblk

```
NAME                           MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                              8:0    0   40G  0 disk 
├─sda1                           8:1    0    1M  0 part 
├─sda2                           8:2    0    1G  0 part /boot
└─sda3                           8:3    0   39G  0 part 
  ├─VolGroup00-LogVol00        253:0    0    8G  0 lvm  /
  ├─VolGroup00-LogVol01        253:1    0  1.5G  0 lvm  [SWAP]
  └─VolGroup00-home            253:4    0   10G  0 lvm  /home
sdb                              8:16   0   10G  0 disk 
sdc                              8:32   0    2G  0 disk 
├─vg_mirror-lv_mirror_rmeta_2  253:7    0    4M  0 lvm  
│ └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
└─vg_mirror-lv_mirror_rimage_2 253:8    0  900M  0 lvm  
  └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
sdd                              8:48   0    1G  0 disk 
├─vg_mirror-lv_mirror_rmeta_0  253:2    0    4M  0 lvm  
│ └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
└─vg_mirror-lv_mirror_rimage_0 253:3    0  900M  0 lvm  
  └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
sde                              8:64   0    1G  0 disk 
├─vg_mirror-lv_mirror_rmeta_1  253:5    0    4M  0 lvm  
│ └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
└─vg_mirror-lv_mirror_rimage_1 253:6    0  900M  0 lvm  
  └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
```

touch /home/test{0..30}.txt

lvcreate -n ss_home -s -L 100M /dev/VolGroup00/home

```
Rounding up size to full physical extent 128.00 MiB
Logical volume "ss_home" created.
```

lsblk

```
NAME                           MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                              8:0    0   40G  0 disk 
├─sda1                           8:1    0    1M  0 part 
├─sda2                           8:2    0    1G  0 part /boot
└─sda3                           8:3    0   39G  0 part 
  ├─VolGroup00-LogVol00        253:0    0    8G  0 lvm  /
  ├─VolGroup00-LogVol01        253:1    0  1.5G  0 lvm  [SWAP]
  ├─VolGroup00-home-real       253:10   0   10G  0 lvm  
  │ ├─VolGroup00-home          253:4    0   10G  0 lvm  /home
  │ └─VolGroup00-ss_home       253:12   0   10G  0 lvm  
  └─VolGroup00-ss_home-cow     253:11   0  128M  0 lvm  
    └─VolGroup00-ss_home       253:12   0   10G  0 lvm  
sdb                              8:16   0   10G  0 disk 
sdc                              8:32   0    2G  0 disk 
├─vg_mirror-lv_mirror_rmeta_2  253:7    0    4M  0 lvm  
│ └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
└─vg_mirror-lv_mirror_rimage_2 253:8    0  900M  0 lvm  
  └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
sdd                              8:48   0    1G  0 disk 
├─vg_mirror-lv_mirror_rmeta_0  253:2    0    4M  0 lvm  
│ └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
└─vg_mirror-lv_mirror_rimage_0 253:3    0  900M  0 lvm  
  └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
sde                              8:64   0    1G  0 disk 
├─vg_mirror-lv_mirror_rmeta_1  253:5    0    4M  0 lvm  
│ └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
└─vg_mirror-lv_mirror_rimage_1 253:6    0  900M  0 lvm  
  └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
```

rm -f /home/test{0..15}.txt

ls -la

```
total 8
drwxr-xr-x.  3 root    root    4096 Oct 14 11:06 .
drwxr-xr-x. 18 root    root     239 Oct 14 11:02 ..
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test16.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test17.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test18.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test19.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test20.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test21.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test22.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test23.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test24.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test25.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test26.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test27.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test28.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test29.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test30.txt
drwx------.  3 vagrant vagrant 4096 May 12  2018 vagrant
```

cd /

umount -l /home/

lvconvert --merge /dev/VolGroup00/ss_home 

```
  Delaying merge since origin is open.
  Merging of snapshot VolGroup00/ss_home will occur on next activation of VolGroup00/home.
```
  
mount /home/

lsblk

```
NAME                           MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                              8:0    0   40G  0 disk 
├─sda1                           8:1    0    1M  0 part 
├─sda2                           8:2    0    1G  0 part /boot
└─sda3                           8:3    0   39G  0 part 
  ├─VolGroup00-LogVol00        253:0    0    8G  0 lvm  /
  ├─VolGroup00-LogVol01        253:1    0  1.5G  0 lvm  [SWAP]
  ├─VolGroup00-home-real       253:10   0   10G  0 lvm  
  │ ├─VolGroup00-home          253:4    0   10G  0 lvm  /home
  │ └─VolGroup00-ss_home       253:12   0   10G  0 lvm  
  └─VolGroup00-ss_home-cow     253:11   0  128M  0 lvm  
    └─VolGroup00-ss_home       253:12   0   10G  0 lvm  
sdb                              8:16   0   10G  0 disk 
sdc                              8:32   0    2G  0 disk 
├─vg_mirror-lv_mirror_rmeta_2  253:7    0    4M  0 lvm  
│ └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
└─vg_mirror-lv_mirror_rimage_2 253:8    0  900M  0 lvm  
  └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
sdd                              8:48   0    1G  0 disk 
├─vg_mirror-lv_mirror_rmeta_0  253:2    0    4M  0 lvm  
│ └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
└─vg_mirror-lv_mirror_rimage_0 253:3    0  900M  0 lvm  
  └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
sde                              8:64   0    1G  0 disk 
├─vg_mirror-lv_mirror_rmeta_1  253:5    0    4M  0 lvm  
│ └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
└─vg_mirror-lv_mirror_rimage_1 253:6    0  900M  0 lvm  
  └─vg_mirror-lv_mirror        253:9    0  900M  0 lvm  /var
```


> Т.к. устройство занято, то желаем ребут

reboot


> Итог

ls -la /home/

```
total 8
drwxr-xr-x.  3 root    root    4096 Oct 14 11:03 .
drwxr-xr-x. 18 root    root     239 Oct 14 11:02 ..
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test0.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test10.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test11.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test12.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test13.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test14.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test15.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test16.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test17.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test18.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test19.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test1.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test20.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test21.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test22.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test23.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test24.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test25.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test26.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test27.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test28.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test29.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test2.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test30.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test3.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test4.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test5.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test6.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test7.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test8.txt
-rw-r--r--.  1 root    root       0 Oct 14 11:03 test9.txt
drwx------.  3 vagrant vagrant 4096 May 12  2018 vagrant
```

