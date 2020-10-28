---
id: qemu-img
title: Qemu Image
---

# Qemu Image

## Tips
* 参考
  * [Copying a 1TB sparse file](https://stackoverflow.com/questions/13252682)
    * 结论 - GNU tar 最快，内核 3.1+ 支持 SEEK_HOLE
  * [Sparse file](https://wiki.archlinux.org/index.php/sparse_file)

```bash
# 查看映射情况
qemu-img map delta.qcow2

# 创建 sparse 文件
qemu-img create -f raw test.raw 2G
dd if=/dev/zero of=test.raw seek=2G bs=1 count=1

# 传输 sparse 文件
# 还是相对慢
rsync -aS test.raw admin@server:~
# tar 支持 sparse
tar cSvfz - test.raw | ssh admin@server 'tar -C ~ -zvxf -'
# 如果是本地，则不压缩更快
tar cSvf - test.raw | ssh admin@server 'tar -C ~ -vxf -'
# 查看远程大小
ssh admin@server ls -lahs

# 压缩
tar cSvfz test.tar.gz test.raw
# 解压
mkdir test
tar -xvSf test.tar.gz -C test

# 复制
# 默认支持，不加 --sparse=always 也可以
cp --sparse=always test.raw tmp1.raw
# 将 sparse 去掉
cp --sparse=never test.raw tmp2.raw
# 恢复为 sparse
fallocate -d tmp2.raw

# dd 支持 sparse
# https://man7.org/linux/man-pages/man1/dd.1.html
dd if=test.raw of=tmp3.raw conv=sparse status=progress

# ddrescue 支持写入 sparse
ddrescue -S -b8M /dev/sda1 /mount/external/backup/sda1.raw
```

## 磁盘格式
- raw - 原始磁盘格式
  - 性能最好，占用空间最多
  - fallocate 可以预留空间
  - Linux 下如果文件系统支持 holes(ext2,ext3,NTFS 等) 则只有使用的空间才会被占用 - `ls -ls` 查看第一列 或 `qemu-img info` 查看
- bochs: Bochs disk image format
- cloop - compressed loopback disk image format
  - useful only to reuse directly compressed CD-ROM images present for example in the Knoppix CD-ROMs.
- cow - User Mode Linux Copy On Write image format
  - 为了兼容存在，不支持 Windows
- dmg - Mac disk image format
- iso - CDROM disk image format
- qcow - QEMU v1
  - 为了兼容存在
- qcow2 - QEMU v2
  - 功能最为齐全
  - AES 加密
  - zlib 压缩
  - 支持快照
- qed - QEMU Enhanced Disk image format
- vmdk - VMware disk image format
- vpc/vhd - Windows Virtual PC disk image format / Microsoft virtual hard disk image format
- nbd - Network block device
- parallels - Parallels virtualization disk image format
- vdi - Oracle VM VirtualBox hard disk image format
- vmdk - VMware 3 and 4 compatible image format
- vvfat - Virtual VFAT disk image format

```bash
# 检测是否支持 holes
# 如果是一瞬间就好
qemu-img create -f raw test.raw 2G
# 第一列为实际占用大小
ls -lsh test.raw
```

## holes
* virt-sparsify - libguestfs

## 磁盘压缩

- [Shrink Qcow2 Disk Files](https://pve.proxmox.com/wiki/Shrink_Qcow2_Disk_Files)
- http://blog.programster.org/qemu-img-cheatsheet

```bash
# 缩小
# ==========
# 主机内执行
fstrim -av

# 转换后会变小
qemu-img convert -O qcow2 alpine.img shrink.qcow2
# 也可以进行压缩，会更小，但启动时会恢复
qemu-img convert -O qcow2 alpine.img shrink.qcow2 -c
```

## LUKS
* QCOW2 支持 LUKS
* https://www.qemu.org/docs/master/system/qemu-block-drivers.html
* LUKS 实际占用磁盘空间会更大

```bash
# 创建无密码磁盘
qemu-img create -f qcow2 demo.qcow2 10M
# LUKS 加密，密码为 123
qemu-img create -f luks --object secret,data=123,id=sec0  \
 -o key-secret=sec0 demo.luks 10M
# 写入 LUKS
qemu-img convert --target-image-opts \
    --object secret,data=123,id=sec0 -f qcow2 demo.qcow2 -n \
    driver=luks,file.filename=demo.luks,key-secret=sec0

# QEMU 使用
# -drive file=demo.luks,format=luks,key-secret=sec0,if=virtio -object secret,data=123,id=sec0

# AES 加密
openssl rand -base64 32 > key.b64
KEY=$(base64 -d key.b64 | hexdump  -v -e '/1 "%02X"')
openssl rand -base64 16 > iv.b64
IV=$(base64 -d iv.b64 | hexdump  -v -e '/1 "%02X"')
printf "123" | openssl enc -aes-256-cbc -a -K $KEY -iv $IV > sec.b64

qemu-system-x86_64 \
  -object secret,id=secmaster0,format=base64,file=key.b64 \
  -object secret,id=sec0,keyid=secmaster0,format=base64,file=sec.b64,iv=$(<iv.b64) \
  -drive file=demo.luks,format=luks,key-secret=sec0,if=virtio

# printf "$SECRET" | openssl enc -d -aes-256-cbc -a -K $KEY -iv $IV
```

# FAQ

## 合并 backing 文件

- https://libvirt.org/kbase/backing_chains.html

```bash
# 查看 backing
qemu-img info --backing-chain test.qcow2
# 假设 test.qcow2 的 base 是 base.qcow2
cp base.qcow2 tmp.qcow2

# 修改 base
qemu-img rebase -b tmp.qcow2 test.qcow2
# 提交到 base
qemu-img commit test.qcow2
# 移除旧的文件
mv tmp.qcow2 test.qcow2
```
