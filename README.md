# cxcore
A prebuilt Linux system use UEFI and f2fs for RaspberryPi all 64-Bit CPU serials.

# 使用UEFI启动的树莓派64位系统镜像

![image1](https://raw.githubusercontent.com/chainsx/PIXEL64-RPI/master/img/2020020512003372.png)
#### 64位pixel桌面环境

## [系统特性](https://github.com/chainsx/cxcore/blob/master/doc/feature.md)
## [64位pixel桌面环境（测试）](https://github.com/chainsx/cxcore/blob/master/doc/about-pixeldesktop.md)

## 系统详情

* 第一分区：VFAT用于EFI启动
* 第二分区：EXT4用于存放内核以及启动信息
* 第三分区：F2FS根目录系统分区

* 默认用户名：pi
* 默认密码：raspberrypi

### 我们提倡自行构建系统，这样你不仅知道本系统的所有概况，而且还能感受树莓派的乐趣,本系统完全开源，所有构建代码已公布，进行了详细的注释，本人代码写的不好请原谅。
### 我们已经在树莓派俱乐部发表关于系统构建的文章，让每一个人都能体验树莓派的乐趣。

### github是开源的地方，此处不提供系统下载，预构建公测版本下载请转至树莓派俱乐部，有问题请在相关评论区留言。

## 自行构建：

`sh build.sh`

#### 建议使用ubuntu-18.04，x86_64进行构建。
### 注意：构建主机需要支持f2fs文件系统。
#### （如何查看支不支持f2fs？ `cat /proc/filesystems | grep f2fs` 输出f2fs即为支持）


[树莓派俱乐部相应地址跳转(此版本不带桌面环境)](https://raspberrypi.club/341.html)

<p align="center">2018-2020 chainsx.cn</p>

由于原文档仓库主人已经不再维护,所以无法直接使用. 本人在原有的基础上作了少量更新,以为可用,具体如下:

这是一个用脚本制作镜像的例子此方法为USB  EFI grub 启动,  但非GPT (是msdos分区表)
git clone --depth=1 https://gitclone.com/github.com/wzwmzm/cxcore.git
必须在PC或aarch64机上运行
修改如下内容:
1,nano scripts/mkrootfs.sh	(准备rootfs的内容, 内核也在这个分区中)	
    更新版本:   KERNEL_VERSION=20200819-1_armhf
    更新版本:   FIRMWARE_NONFREE_VERSION=20200819
    168行-172行改为:
       wget $MIRROR/$FIRMWAREPATH/raspberrypi-firmware_1.$FIRMWARE_NONFREE_VERSION.orig.tar.gz
       tar -xvf raspberrypi-firmware_1.$FIRMWARE_NONFREE_VERSION.orig.tar.gz
       mv raspberrypi-firmware-1.$FIRMWARE_NONFREE_VERSION root/lib/firmware
       rm raspberrypi-firmware_1.$FIRMWARE_NONFREE_VERSION.orig.tar.gz
    39行增加;    sudo mount -t devpts /dev/pts root/dev/pts
    最后一行增加:  sudo umount root/dev/pts
2,nano scripts/mkinitroot.sh
    61行改成: cat <<EOF >> initroot/etc/fstab
3,nano scripts/dd_partition.sh
    先用losetup命令看看loop设备是否在用

使用方法:
cd cxcore
sudo  sh build.sh
最终结果是debain.img

注意事项:
 1,必须在PC或aarch64机上运行
 2,先用lsbkl命令看看loop设备是否在用,视情况修改scripts/dd_partition.sh文件头部选用的loop设备
