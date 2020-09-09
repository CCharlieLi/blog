---
title: Loongson 8089D install OpenBSD 5.7
date: "2015-10-18T23:46:37.121Z"
template: "post"
draft: false
slug: "Loongson-OpenBSD"
category: "Tech"
tags:
  - "Tech"
description: "Step by step: setup Loongson 8089D with OpenBSD 5.7"
socialImage: "/media/image-book.jpg"
---

![m](/media/20151019.png)

### Install OpenBSD 5.7

- Make a USB stick in FAT format

- Copy all install files into USB

- Input ‘Del’ to get in PMON

- ‘devls’ to check disk

- ‘boot -k /dev/fat/disk@usb0/bsd.rd’ to boot (here I use usb stick in `fat32` format)

- (I)nstall, (U)pgrade or (S)hell? 我们输入 i 进入安装。

  -  Choose Keyboard Layout，直接回车（也可以输入L列出可用的布局再选择，不过我输入L后没列出什么）；

  -  System Hostname，主机名，自己定义吧（我输入的是xinlan，纪念我的第一台计算机）；

  -  Available Network interfaces are: ..... 列出可用的网卡，两个；

  -  Which do you wish to configure? 你想配置哪一个网卡？默认给出的是以太网网卡，直接回车；

  -  IPv4 address for...? 设置网卡的 IPv4 地址。我选择 dhcp。之后检测一阵子，因为我没联网，所以当然检测不到；

  -  IPv6 address for...? 设置网卡的IPv6地址。我选择none。

  -  Available Network interfaces are: ..... 再一次列出可用的网卡，两个；

  -  Which do you wish to configure? 如果还想配置无线网卡，则输入无线网卡名。我不想配置，所以输入了done。

  -  DNS domain name? 域名系统，填写自己喜欢的本地域名。

  -  DNS nameservers? 域名解析服务器的地址，我写了 none ；

  -  Password for root account? (will not echo) 填写ROOT用户的密码；

  -  assword for root account? (again) 再次填写ROOT用户的密码，就是确认一次；

  -  Start sshd(8) by default? [yes] 默认回车；

  -  Start ntpd(8) by default? [no] 默认回车；

  -  Setup a user? 创建一个普通用户，我用自己的名字创建了一个；要求输入全名、密码并确认；

  -  What timezone are you in? 选择时区。输入 Asia，然后输入 Shanghai ；

  -  Available disks are: wd0.

  -  Which one is the root disk? 要把OpenBSD安装到哪一个盘？输入 wd0 并回车；

  -  Use DUIDs rather than device names in fstab? 这个看个人喜好，我输入 no ；

  -  Use (W)hole disk or (E)dit the MBR? [whole] 使用整块硬盘还是自己分区？我直接回车，使用整块硬盘；

  -  OpenBSD 根据硬盘大小（2G，囧）自动产生了分区方案。注意，OpenBSD 的分区名称，c 代表整个磁盘（其实这不是一个分区），b 代表 swap 分区，剩余 a、d、e.... 直到 p 才是可用的分区名称。在这里，OpenBSD分了一个a区，挂载到 / ，一个 b 分区（swap分区），还有一个1M的 i 分区用于存放 boot 文件。

  -  Use (A)uto layout, (E)dit auto layout, or create (C)ustom layout? 我输入 a 使用自动分区方案；然后 OpenBSD 把分区方案写入；

  -  Location of sets? 安装文件所在的位置。输入 disk ；

  -  询问所选的分区是否挂载了。默认是 no，回车；

  -  下面列出 wd0 sdb sdb1，输入 sdb1 并回车；

  -  Pathname to the sets? 安装文件的路径，输入指定路径（就是指向最初U盘中的文件）；

  -  下面列出来找到的 tgz 文件。我不想安装 X Window System，所以我输入“-xbase54.tgz ”等，把所有 x...tgz 的文件前的 [X] 变成了 [ ]；剩下的带[X]标记的就是要安装的包；最后输入 done，开始安装；

  -  重启后进入PMON，我们需要执行以下操作：

```shell
PMON> set al /dev/fs/ext2@wd0/boot
PMON> set bsd /bsd
PMON> set moresz 30
PMON> poweroff
```


### OpenBSD configuration tips

- 开启无线网卡:

```shell
ifconfig wpi0 nwid dlink wpa wpapsk $(wpa-psk dlink xxx)
dhclient wpi0
```

其中wpi0为无线网卡名，dlink为接入网络名，xxx为无线密码，该网络为DHCP配置

- 关闭ksh的鸣笛声音

```shell
wsconsctl keyboard.bell.pitch=0
wsconsctl keyboard.bell.volume=0
```

- 配置OpenBSD的源

```shell
export PKG_PATH=ftp://ftp.OpenBSD.org/pub/OpenBSD/5.7/packages/mipsel/
```

- 更改命令提示符

```shell
export PS1='\[\e[1m\][\u@\h \W]\[\e[0m\]\$ '
```

- 关机并且关闭电源

```shell
shutdown -ph now
```


