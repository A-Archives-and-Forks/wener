---
slug: rescue-root-disk
title: 系统盘恢复
tags:
  - AlpineLinux
  - 运维
---

# 系统盘恢复

喜欢使用 闪存盘/U 盘 作为 Linux 系统盘，但 U 盘 使用寿命有限，当异常后需要对系统盘进行更换。

<!-- more -->

## 现状

使用 SanDisk CZ430 作为 系统盘/Root 分区

**lsblk**

```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    1 114.6G  0 disk
├─sda1   8:1    1   128M  0 part /boot
└─sda2   8:2    1 114.4G  0 part /
```

**df -h /**

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2       113G  2.2G  106G   2% /
```

在使用一段时间后， U 盘 异常，系统被重新挂载为只读。

```bash
touch ~/test
```

```
touch: cannot touch '/home/admin/test': Read-only file system
```

dmesg 可看到异常信息，几个月前已经出问题了，因为系统还能正常使用暂且没管。

```dmesg title="dmesg -T"
[Tue Sep  7 16:48:16 2021] sd 2:0:0:0: [sda] tag#0 UNKNOWN(0x2003) Result: hostbyte=0x00 driverbyte=0x08 cmd_age=0s
[Tue Sep  7 16:48:16 2021] sd 2:0:0:0: [sda] tag#0 Sense Key : 0x7 [current]
[Tue Sep  7 16:48:16 2021] sd 2:0:0:0: [sda] tag#0 ASC=0x27 ASCQ=0x0
[Tue Sep  7 16:48:16 2021] sd 2:0:0:0: [sda] tag#0 CDB: opcode=0x2a 2a 00 00 04 16 90 00 00 08 00
[Tue Sep  7 16:48:16 2021] blk_update_request: critical target error, dev sda, sector 267920 op 0x1:(WRITE) flags 0x103000 phys_seg 1 prio class 0
[Tue Sep  7 16:48:16 2021] Buffer I/O error on dev sda2, logical block 466, lost async page write
```

## 备份恢复方案

将现在数据完整备份到新的 U 盘，重启即可，但恢复备份有好几种方式。

1.  全盘 dd

- 速度慢
- 简单暴力
- 需要提供至少现在磁盘大小的存储

2. 重装 - 仅拷贝数据

- 麻烦

3. 恢复分区、恢复数据

- 高效
- 需要多一点操作
- 新磁盘只需要已用空间大小即可

## 恢复

因为这是已经第三四次恢复了，之前都是 dd，但这次打算用更高效的方式，且这次的系统盘为 128G，但目前只有 64G 的空闲 U 盘，故选择方案 3。

**恢复过程**

0. 准备备份盘
1. 分区表备份 & 启动分区备份
2. root 分区 备份 & 修复
3. 验证
4. 关机
5. 取掉坏的 U 盘
6. 开机

:::tip 恢复备份注意事项

- 选择一个可写目录 - tmpfs,shm - 例如: /run, /var/run, /tmp, /dev/shm

:::

### 准备备份盘

- 备份盘 /dev/sdb 64G，同 SanDisk CZ430

```pre title="lsblk"
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    1 114.6G  0 disk
├─sda1   8:1    1   128M  0 part /boot
└─sda2   8:2    1 114.4G  0 part /
sdb      8:16   1  57.3G  0 disk
```

### 分区表&启动分区备份

```bash
# 确保使用 root 用户操作
sudo su
# 进入可写目录
cd /run

# 正常情况分区表的备份恢复
# sfdisk -d /dev/sda | sfdisk -f /dev/sdb

# 选择简单方式备份后再修改最后分区
# 这里 dd 130MB - 同时把 boot 分区备份了
dd if=/dev/sda of=/dev/sdb bs=1M count=130 status=progress

# 转储分区表
sfdisk -d /dev/sda > sda.partition.table.txt
# 删除第二个分区的大小
sed -r '$ s/size=\s*\d+,//' -i sda.partition.table.txt
# 重建分区
sfdisk -f /dev/sdb < sda.partition.table.txt
# 现在分区大小正常
fdisk -l /dev/sdb

# 分区变化重新扫描
mdev -s
partprobe
# 出现分区
ls /dev/sdb*
```

### root 分区备份

- root 分区一般较大，直接 dd 慢且伤 U 盘
- 直接 dd 需要目标相同大小，否则丢数据
- 这里使用 clone 数据方式

:::caution 由于目标分区更小，不能直接复制

```bash
# ext4 数据拷贝 - 基于 ext4 block，只拷贝使用部分
# 也可以用于生成 img 备份镜像之后用
e2image -ra -p /dev/sda2 /dev/sdb2
```

- 如果分区大小足够，直接复制即可

:::

因为 root 分区为 120G，但实际只用了 2.2G， 先镜像分区，缩小分区，然后恢复到新的分区。

索性内存有 4G，因此直接在 /dev/shm 操作，否则需要外置存储。

```bash
# 从新挂载为 3G - 默认 2G
mount -o remount,size=3G /dev/shm
# 制作镜像
e2image -rap /dev/sda2 sda2.raw

# 修复分区 - 一般 U盘 损坏都会导致 fs 异常
e2fsck -y sda2.raw
# 缩小分区
resize2fs sda2.raw 4G
# 恢复
e2image -rap sda2.raw /dev/sdb2
# 扩展分区
resize2fs /dev/sdb2
```

### 验证

```bash
fsck /dev/sdb1
fsck /dev/sdb2

mount /dev/sdb2 /mnt
mount /dev/sdb1 /mnt/boot
# 不应该有变化
rsync -vna /boot/ /mnt/boot/

# 由于 fsck 修复，可能 root 分区大小不同
df / /mnt /boot /mnt/boot
```

如果装有 qemu，还可以直接启用 qemu 验证，目前就简单确认文件没问题即可。

```bash
# 准备关机重启
umount -R /mnt
eject /dev/sdb

poweroff
```

取掉 坏的 U 盘，启动。

一切恢复正常。🎉

## 总结

系统盘恢复还是相当容易，但 CZ430 确实有点老了，在 2017 年左右上市，相同规格已经卖 4、5 年了，性能各方面有所欠缺，之后会尽量选择 Lexar S47。

- https://wener.me/notes/ops/admin/fio-out/
  - SanDisk CZ430, Lexar S47 性能对比

此外使用 U 盘 做系统盘的时候，一定注意修改 docker 之类的数据目录为其他存储，因为跑实际应用的时候 U 盘 性能是不足的，且数据库类型应用的小 BlockSize IO 不适合 U 盘。
