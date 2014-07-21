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

**B2:  Tạo partition bằng cậu lệnh fdisk /dev/sdb và fdisk /dev/sdc**
1.	Tạo partition trong /dev/sdb:
> Command (m for help): n

> Partition type:

>    p   primary (0 primary, 0 extended, 4 free)
>    e   extended
> Select (default p): p
> Partition number (1-4, default 1): 1
> First sector (2048-20971519, default 2048):
> Using default value 2048> 
> Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519): +5G
