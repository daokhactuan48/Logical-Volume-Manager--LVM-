Hướng Dẫn Tạo LVM
=================

##I. Thông tin LAB
> - Tạo máy ảo trên vmware Workstation cài hệ điều hành ubuntu server 12.04
> - Add thêm 2 ổ cứng có dung lượng 10GB vào máy ảo


**Mô tả bằng hình ảnh**
<br>
<p align="center"><img src="http://i.imgur.com/9rNADx4.png"></p>

##II. Các bước tiến hành 
**Trong bài này mình sẽ đặt tên các volume group, logical volume được như sau:**
> - Volume group: vgdemo
> - logical volume: lvdat

**Chú ý:** Bạn lên đặt tên của volume group là vg[ten] hoặc lv[ten] để dễ nhận dạng hơn

**B1: Trước khi cài đăt bạn lên kiểm tra xem có những hard drives nào bằng lệnh sau: #lsblk**

> root@controller:~# lsblk <br>
> NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT <br>
> sda      8:0    0    40G  0 disk <br>
> ├─sda1   8:1    0   1.9G  0 part [SWAP] <br>
> ├─sda2   8:2    0  18.6G  0 part / <br>
> ├─sda3   8:3    0   9.3G  0 part <br>
> └─sda4   8:4    0  10.2G  0 part /srv/node/sda4 <br>
> sdb      8:16   0    10G  0 disk <br>
> sdc      8:32   0    10G  0 disk <br>
> sr0     11:0    1   665M  0 rom <br>

Sau khi nhập vào #lsblk bạn sẽ thấy được 2 hard drives đã có là: sdb và sdc

**B2:  Tạo partition bằng cậu lệnh fdisk /dev/sdb**

1.	Tạo partition trong /dev/sdb:
> Command (m for help): n  <br>
> Partition type: <br>
>    p   primary (0 primary, 0 extended, 4 free)<br>
>    e   extended<br>
> Select (default p): p<br>
> Partition number (1-4, default 1): 1<br>
> First sector (2048-20971519, default 2048):<br>
> Using default value 2048 <br>
> Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519): +5G<br>

2.	Kiểm tra xem partition đã được tạo chưa 
> Command (m for help): p <br>
> Disk /dev/sdb: 10.7 GB, 10737418240 bytes <br>
> 255 heads, 63 sectors/track, 1305 cylinders, total 20971520 sectors <br>
> Units = sectors of 1 * 512 = 512 bytes <br>
> Sector size (logical/physical): 512 bytes / 512 bytes <br>
> I/O size (minimum/optimal): 512 bytes / 512 bytes <br>
> Disk identifier: 0xc8fe9a22 <br>
> Device Boot      Start         End      Blocks   Id  System <br>
> /dev/sdb1        2048     10487807    5242880   83  Linux <br>

Từ trên ta thấy được partition /dev/sdb1 đã được tạo. Sau đó ấn w để lưu lại quá trình tạo partition. Làm tương tự đối với hard drives sdc

**B3: Tạo physical volume là /dev/sdb1 và /dev/sdc1**

Sử dụng câu lệnh sau để tao physical volume: #pvcreate /dev/sdb1 và #pvcreate /dev/sdc1

> root@controller:~# pvcreate /dev/sdb1<br>
>   Physical volume "/dev/sdb1" successfully created<br>
> root@controller:~# pvcreate /dev/sdc1<br>
>    Physical volume "/dev/sdc1" successfully created<br>

**B4: Kiểm tra xem physical volume đã có chưa bằng lệnh sau: pvdisplay**

> root@controller:~# pvdisplay
>   "/dev/sdb1" is a new physical volume of "5.00 GiB"<br>
>   --- NEW Physical volume ---<br>
>   PV Name               /dev/sdb1<br>
>   VG Name<br>
>   PV Size               5.00 GiB<br>
>   Allocatable           NO<br>
>   PE Size               0<br>
>   Total PE              0<br>
>   Free PE               0<br>
>   Allocated PE          0<br>
>   PV UUID               wdzUgF-s77s-vanU-zqh7-TEcc-H3I5-n4nob0 <br>
>   "/dev/sdc1" is a new physical volume of "5.00 GiB"<br>
>   --- NEW Physical volume ---<br>
>   PV Name               /dev/sdc1<br>
>   VG Name<br>
>   PV Size               5.00 GiB<br>
>   Allocatable           NO<br>
>   PE Size               0<br>
>   Total PE              0<br>
>   Free PE               0<br>
>   Allocated PE          0<br>
>   PV UUID               oSuTNT-dX3U-grfr-UKOL-6Gyo-32s3-XsUTIc<br>

**B5: Tạo volume group với câu lệnh: vgcreate {tên volume group} { physical volume}**

> root@controller:~# vgcreate vgdemo /dev/sdb1 <br>
>   Volume group "vgdemo" successfully created <br>

**B6: Kiểm tra xem volume group đã có chưa bằng lệnh sau: vgdisplay**

> root@controller:~# vgdisplay
>   --- Volume group ---
>   VG Name               vgdemo
>   System ID
>   Format                lvm2
>   Metadata Areas        1
>   Metadata Sequence No  1
>   VG Access             read/write
>   VG Status             resizable
>   MAX LV                0
>   Cur LV                0
>   Open LV               0
>   Max PV                0
>   Cur PV                1
>   Act PV                1
>   VG Size               5.00 GiB
>   PE Size               4.00 MiB
>   Total PE              1279
>   Alloc PE / Size       0 / 0
>   Free  PE / Size       1279 / 5.00 GiB
>   VG UUID               vlL9ON-E63w-MdSZ-fxjJ-YO5U-L1cF-UrydJO

**B7: Tạo logical volume:**

> root@controller:~# lvcreate -L 3GB -n lvdata vgdemo
>   Logical volume "lvdata" created

-L:  Chỉ ra dung lượng của logical volume
-n: Tên của logical volume
