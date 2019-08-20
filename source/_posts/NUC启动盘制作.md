---
title: NUC启动U盘制作
categories: 启动U盘
tags:
- NUC
---

### U盘制作步骤及镜像复制

1. CMD->diskpart
2. DISKPART> list disk

  磁盘 ###  状态           大小     可用     Dyn  Gpt
  --------  -------------  -------  -------  ---  ---
  磁盘 0    联机              931 GB  1024 KB        *
  磁盘 1    联机              119 GB      0 B        *
  磁盘 2    联机               14 GB      0 B

<!-- more -->


3. DISKPART> select disk 2

磁盘 2 现在是所选磁盘。

4. DISKPART> clean
DiskPart 成功地清除了磁盘。

5. DISKPART> create partition primary
DiskPart 成功地创建了指定分区。

6. DISKPART> select partition 1

分区 1 现在是所选分区。

7. DISKPART> active

DiskPart 将当前分区标为活动。

8. DISKPART> format fs=ntfs quick

  100 百分比已完成

DiskPart 成功格式化该卷。


9. 将ISO镜像文件打开,然后将其中的8个文件,复制到刚刚做好的U盘中
10. 开启BIOS按F12 选择UEFI即可




















