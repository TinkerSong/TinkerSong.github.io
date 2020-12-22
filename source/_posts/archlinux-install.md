---
title: ArchLinux安装配置手册[系统篇]
---

# 安装前的准备
镜像下载地址：https://archlinux.org/download/
:::default
未使用过Linux的用户，建议先了解一下Linux
:::

# 启动Live环境
（1）制作启动盘

如果你是Windows用户，你可以使用Rufus来制作，具体使用方法你可以查阅一下百度。

如果你是Linux用户，你只需要使用dd命令即可，相信你肯定会的。

（2）进入Live环境

选择从带有 Arch 安装文件的媒介启动通常是在你电脑开机的时候按下某个按键，一般会在启动画面有提示。不同的主板按键不同。

当 Arch 菜单出现时，选择 Boot Arch Linux 并按 Enter 进入安装环境。

# 验证启动模式（重要）
可以列出 efivars 目录以验证启动模式：
``` bash
ls /sys/firmware/efi/efivars
```
如果目录不存在，系统可能以 BIOS 或 CSM 模式启动，详见您的主板手册。

如果目录存在。系统就是以UEFI启动。

请记住你的启动模式！！！

# 连接网络
:::default
请选择其中一种方式连接
:::
1.连接网线
``` bash
dhcpcd
```
2.连接WiFi
``` bash
wifi-menu
```
目前wifi-menu连接工具已经被移除，请参照最新的wiki文档使用新的工具

# 分区方案
BIOS with MBR			
|挂载点|分区|分区类型|建议大小|
|  :----:     | :----:  | :----:| :----:   |
|/mnt|/dev/sda1|Linux|剩余空间|
|[SWAP]|/dev/sda2|Linux swap|More than 512 MiB|
UEFI with GPT			
|挂载点|分区|分区类型|建议大小|
|  :----:     | :----:  | :----:| :----:   |
|/mnt/boot|/dev/sda1|EFI 系统分区|265–512 MiB|
|/mnt|/dev/sda2|Linux x86-64 root (/)|剩余空间|
|[SWAP]|/dev/sda3|Linux swap|More than 512 MiB|

:::warning
分区方案解读：如果你是以BIOS模式启动，你就无须创建和挂载/boot分区。

如果你是以UEFI模式启动，那你就必须创建和挂载/boot分区
:::

#开始分区
::: default
在那之前，建议你先了解一下以下分区的作用
:::
``` bash
/ #根目录
/home #家目录
/boot #引导分区
swap #交换分区
```

# 格式化分区
当分区建立好了，这些分区都需要使用适当的文件系统进行格式化
:::warning
注意：请根据自己的分区情况进行对应的格式化,这里的硬盘分区只是作为例子，请根据自己实际情况进行操作
:::
/
``` bash
mkfs.ext4 /dev/sda1
```
/home
``` bash
mkfs.ext4 /dev/sda2
```
/boot（BIOS启动的不需要)
```bash
mkfs.fat -F32 /dev/sda3
```
swap
``` bash
mkswap /dev/sda4
swapon /dev/sda4
```
# 挂载分区
/
将/分区挂载到/mnt
``` bash
mount /dev/sda1 /mnt
```
/home
/mnt下创建/home文件
``` bash
mkdir /mnt/home
```
挂载/home分区
``` bash
mount /dev/sda2 /mnt/home
```
/boot（BIOS启动的不需要）
/mnt下创建/boot
``` bash
mkdir /mnt/boot
```
挂载引导分区
``` bash
mount /dev/sda3 /mnt/boot
```
安装基础软件包
base 软件包并没有包含全部 live 环境中的程序，packages.x86_64 页面包含了它们的差异。需要额外安装：

管理所用文件系统 的用户工具
访问 RAID 或 LVM 分区的工具
未包含在 linux-firmware 中的额外固件
联网需要的程序
文本编辑器,
访问 man 和 info 页面的工具： man-db, man-pages 和 texinfo.
如果你还想安装其他软件包组比如 base-devel，请将他们的名字添加到 pacstrap 后，并用空格隔开。你也可以在 #Chroot 之后使用 pacman 手动安装软件包或组。
:::default
如果你看不懂上面在说什么，那你跟着我运行下面的命令就行了
:::
``` bash
pacstrap /mnt base linux linux-firmware base-devel vi vim nano dhcpcd
```
# 配置系统
Fstab
用以下命令生成 fstab 文件 (用 -U 或 -L 选项设置UUID 或卷标)：
``` bash
genfstab -U /mnt >> /mnt/etc/fstab
```
Change root 到新安装的系统：
```bash
arch-chroot /mnt
```
设置时区
```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
```
本地化设置
/etc/locale.gen 是一个仅包含注释文档的文本文件。指定您需要的本地化类型，只需移除对应行前面的注释符号（＃）即可，建议选择带 UTF-8 的项：
```bash
vim /etc/locale.gen
en_US.UTF-8 UTF-8 #去掉对应编码前面的井号
zh_CN.UTF-8 UTF-8
```
保存退出
接着执行 locale-gen 以生成 locale 讯息：
```bash
locale-gen
```
将 LANG=en_US.UTF-8 加入 /etc/locale.conf
```bash
vim /etc/locale.conf
```
设置主机名
xxxxs是你的主机名（自己取个喜欢的名字）
```bash
echo "xxxx" >> /etc/hostname
```
设置root密码
使用passwd命令为root用户增加密码
```bash
passwd
```
安装Intel-ucode（非Inter C PU不需要）
```bash
pacman -S intel-ucode
```
:::default
如果你的下载出错，可能是网络断开了，那就再连接一次网络即可
:::
安装引导
如果你硬盘上还有别的系统，需要安装 os-prober。如果你的系统在别的硬盘单独引导，则不需要。
```bash
pacman -S os-prober
```
对于 BIOS 系统：
```bash
pacman -S grub
grub-install --target=i386-pc /dev/sdX    # sdX 为你的安装硬盘
grub-mkconfig -o /boot/grub/grub.cfg
```
对于 UEFI 系统：
```bash
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```
# 重启
:::default
系统到这里就安装完毕了
:::
```bash
exit
umount -R /mnt
reboot
```

:::default
系统的美化就不做过多的赘述，因为可供的选择太多，例如KDE，GNOME，MATE，i3，dwm，openbox等。
每一个桌面的环境都不尽相同，大家根据自己的喜欢进行选择即可！
:::