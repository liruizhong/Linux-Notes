
### 删除LVM-逻辑卷

#### 1. 取消挂载
```
umount /mnt/lv0
```

#### 2. 取消逻辑卷
```
lvremove /dev/vg0/lv0
```

#### 3. 取消卷组（直接写卷组名称就可以）
```
vgremove vg0
```

#### 4. 取消物理卷
```
pvremove /dev/sd{b,c}
```

#### 5. 取消/etc/fstab配置文件中对应的挂载信息

#### 6. 服务器实战记录
```
[root@ali-f-ops-lark-runner03 /]# vgs
  VG     #PV #LV #SN Attr   VSize    VFree
  a8data   1   1   0 wz--n- <500.00g <10.00g
  data     1   1   0 wz--n-    4.88t <10.00g
[root@ali-f-ops-lark-runner03 /]# lvs
  LV       VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  a8data01 a8data -wi-ao---- 490.00g
  data01   data   -wi-a-----   4.87t
[root@ali-f-ops-lark-runner03 /]# lvremove /dev/data/data01
Do you really want to remove active logical volume data/data01? [y/n]: y
  Logical volume "data01" successfully removed
[root@ali-f-ops-lark-runner03 /]# vgremove data
  Volume group "data" successfully removed
[root@ali-f-ops-lark-runner03 /]# pvs
  PV         VG     Fmt  Attr PSize    PFree
  /dev/vdb1  a8data lvm2 a--  <500.00g <10.00g
  /dev/vdc1         lvm2 ---     4.88t   4.88t
[root@ali-f-ops-lark-runner03 /]# pvremove /dev/vdc1
  Labels on physical volume "/dev/vdc1" successfully wiped.
[root@ali-f-ops-lark-runner03 /]# pvs
  PV         VG     Fmt  Attr PSize    PFree
  /dev/vdb1  a8data lvm2 a--  <500.00g <10.00g
[root@ali-f-ops-lark-runner03 /]# cat /etc/fstab
UUID=b7792c31-ad03-4f04-a650-a72e861c892d /                       ext4    defaults        1 1
/dev/a8data/a8data01 /a8root ext4 defaults 0 1
```
