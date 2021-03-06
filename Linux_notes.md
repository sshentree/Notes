# Linux 基础知识

__说明：以下提及到的文件包含目录及文本（mkdir \ touch），没有特殊标记及说明的情况下默认__

## Linux 基础知识介绍

### Linux 内核版本及发行版本

#### Linux 内核版本

1. Linux 内核官网：[网址](www.kernel.org)
   - 暂时由李纳斯所领导的内核开发小组维护

#### Linux 发行版本

1. Linux 主要的发行版本有两大分支
   - redhat 系列
     1. CentOS 系统（免费）
   - debian 系列
     1. Ubuntu 系统（免费）

### Linux 系统安装

#### VMware 虚拟机安装与使用

1. VMware 是一个虚拟 PC 的软件，可以在现有的操作系统上虚拟出一个新的硬件环境，相当于模拟了一台新的 PC，以实现在一台计算机上独立运行两个操作系统。

   - [官方网站 VMware](http:www.vmware.com)

2. VMware 主要特点

   - 不需要分区或重新开机，就能在同一台 PC 上使用两种以上的操作系统
   - 本机系统可以与虚拟机系统网络通信
   - 可以设定并修改虚拟机操作系统的硬件环境

3. 建议 VMware 的配置

   - CPU：建议主频为 1GBHz 以上
   - 内存：建议 1GB 以上
   - 硬盘：建议分区空闲空间 8GB 以上

4. 安装 VMware

   - 傻瓜式安装

5. 创建虚拟机步骤（安装操作系统的前提）

   - VMware 主页面（创建虚拟机）

     ![VMware主界面](git_picture/VMware主界面.png)

   - 创建虚拟机

     1. 选择 __稍后安装操作系统__ ：创建一个空白操作系统，稍后进行安装 `iso` 镜像文件（一般使用此方法安装，有安装的过程）

     2. __安装程序光盘映像文件（iso）__ ：在官网上下载镜像文件`iso` 进行安装，但其安装的是最简版系统

     3. __安装程序光驱__ ：是将 `iso` 镜像刻录到光盘上，使用硬件光驱进行安装

        ![创建虚拟机](git_picture/创建虚拟机.png)

   - 选择操作系统

     1. 选择操作系统

     2. 版本选择要安装的版本，如果没有，则可以选择相同的内核进行安装

        ![选择操作系统](git_picture/选择操作系统.png)

   - 虚拟机名称及安装的位置

     1. 安装位置，就是操作系统存放位置。__此文件可以进行复制，使用 VMware 中的 打开虚拟机 直接使用__

        ![创建虚拟机名及保存位置](git_picture/创建虚拟机名及保存位置.png)

   - 指定虚拟机磁盘大小

     1. 磁盘大小，即使系统将使用空间大小。

     2. 一般默认 20G，但是假如指定了 20G，但实际磁盘没有 20G 大小，也可以进行使用，因为系统不是一次使用 20G（只是在虚拟系统中查看为 20G），使用多少用多少（VMware 智能）

     3. 可以根据自己的需要设置磁盘大小（默认就好）

        ![指定虚拟机磁盘大小](git_picture/指定虚拟机磁盘大小.png)

   - 显示虚拟机硬件设备

     1. 可以根据自己的需求进行添加和删除一些不必要的设备（打印机、声卡等）

        ![虚拟机硬件显示及完成](git_picture/虚拟机硬件显示及完成.png)

   - 将镜像文件加载进去

     1. 在官网上下载 `iso` 镜像文件

     2. __进入虚拟机设置__

     3. 使用 `iso` 映像文件，不是使用光驱（麻烦）
     
        ![将Linux镜像文件加载虚拟计中](git_picture/将Linux镜像文件加载虚拟计中.png)
     
   - 完成虚拟机的创建
   
     1. __启动虚拟机__，完成对系统（Linux）的设置
     
        ![虚拟机开机](git_picture/虚拟机开机.png)

#### 系统分区

1. 系统分区（也叫磁盘分区）

   - 磁盘分区是使用 __分区编辑器（partition editor）__，在磁盘上划分几个逻辑部分。磁盘一旦划分成数个分区（partition），不同类的目录与文件可以存储不同的分区
   - 磁盘分区就是将一个完整硬盘划分成几个小的分区（如：C盘、E盘等……），不同的分区有不同的用法（存储不同的文件）

2. 分区类型

   __说明：对一块硬盘来说__

   - 主分区：最多只能有 4 个
     1. 可以格式化，可以写入数据
   - 扩展分区
     1. 最多只能有 1 个
     2. 主分区加扩展分区最多有 4 个
     3. 不能写入数据、不能格式化，只能包含逻辑分区
   - 逻辑分区
     1. 可以写入数据，可以格式化

3. 格式化

   - 格式化是为了写入文件系统；硬盘分区之后要进行格式化，才能写入数据

   - 格式化（高级格式化）又称逻辑格式化，它是指根据用户选定的文件系统（如 FAT16、FAT32、NTFS、EXT2、EXT3、EXT4等……），在磁盘的特定区域写入特定数据，在分区中划分出一片用于存放文件分配表、目录表等用于文件管理的磁盘空间
   - 直白来说格式化，就是在将每一个分区或逻辑分区划分成更小的 __数据块__ (block 一般数据块大小为 4KB)。如果数据大小为 10KB，就只能存储在 3 个数据块中（3 个数据块位置不用连续），而且第三个数据块不能再存储其他数据（Windows 磁盘数据整理）。

4. 硬件设备文件名

   - 给硬件设备设置文件名

   - 如图

     | 硬件               | 设备文件名               |
     | ------------------ | ------------------------ |
     | IDE 硬盘           | `/dev/hd[a-d]`           |
     | SCSI/SATA/USB 硬盘 | `/dev/sd[a-p]`           |
     | 光驱               | `/dev/cdrom 或 /dev/sr0` |
     | 软盘               | `/dev/fd[0-1]`           |
     | 打印机（25 针）    | `/dev/lp[0-2]`           |
     | 打印机（USB）      | `dev/usb/lp[0-15]`       |
     | 鼠标               | `/dev/mouse`             |

   - 给分区设备文件名

     1. 设备文件名（直接加数字即可）
        - `/dev/hda1` ，IDE 硬盘接口（最古老）
        - `/dev/sda1` ，SCSI 硬盘接口、SATA 硬盘接口（SATA类型 现在最常用）

5. 挂载

   - 挂载：将分区挂载到 Linux 的空目录目录上（与 Windows 不同）

   - 必须分区

     1. 根分区  `/`
     2. `swap` 分区，交换分区（虚拟内存），内存了 2 倍，不超过 2GB。作用就是当内存空间不足时，将不常使用的数据放入 `swap` 中。

   - 推荐分区

     1. `/boot` ，启动分区（不在向分区中写入任何数据）。200MB即可，再大些也可以，但是不用太大。

   - 文件系统结构

     1. 如图

        ![文件系统结构](git_picture/文件系统结构.png)

     2. 数据写入位置

        - 向 `/` 根分区写入数据，将存入 `/dev/sda3`
        - 向 `/home` 目录写入数据，将存入 `/dev/sda2`
        - 向 `/etc` 目录写入数据，将存入 `/dev/sda3`

     3. 与 Windows 最大的区别

        - 从目录上可以看出 `/boot` 和 `/home` 是 `/` 根分区的子分区，但是每一个目录都可以有自己的分区

#### Linux 系统安装

1. 启动虚拟机进行安装操作系统

   - 之前的操作过程，已经创建一个虚拟机，但是该虚拟机并没有操作系统。
   - 将操作系统的 `iso` 镜像文件，加载到虚拟机的光驱中
   - 点击 __播放虚拟机__ （相当于给虚拟机通电）

2. 进入 BIOS 启动界面 __（可以不进入）__

   - 在开机时，狂按 `F2` 键（有 VMware 界面时，不好进入看运气），进入 BIOS 界面

   - __在虚拟机中可以不选择以什么启动（硬盘、CD等……）即跳过此步或进入查看以下点击退出不保存也可。__ 但是真实计算机，要选择以 CD 启动，安装完操作系统时，要将其改回以硬盘启动（现在 VMware 智能可以自己选择）。

   - 开机时按 `ESC` 按键，查看本次启动顺序（很人性的）

   - 选择以光盘启动

     ![BIOS以光盘启动](git_picture/BIOS以光盘启动.png)

   - 保存退出

     ![保存退出BIOS](git_picture/保存退出BIOS.png)

3. 进入 Ubuntu（Linux）光盘启动界面

   - 虚拟机通电后，按 `ESC` 键；或者在退出 BIOS 界面时，按 `ESC` 键进入 Linux 光盘启动界面（很好进入）

     ![Linux光盘启动界面](git_picture/Linux光盘启动界面.png)

   - __`F2` 键，选择哪种语言安装（中文、English）。初学者可以试着中文安装，使用 `man` 命令时，一些解释是中文的__

   - __选择 `Install Ubuntu`__

   - 安装欢迎界面

     1. 试用但不安装（可是尝试一下最简介的 Ubnutu）
     2. 安装系统
     3. 检测硬盘为了缺陷（修复模式）
     4. 检测存储介质
     5. 使用硬盘启动（在没有安装系统时，报硬盘无系统可用错）

4. 进入系统安装

   - 语言包安装（简体中文）

     1. 选择中文安装

        ![选择语言包](git_picture/选择语言包.png)

   - 键盘布局（美式键盘）

     1. 选择美式键盘（方便键入英文）

        ![选择键盘布局](git_picture/选择键盘布局.png)


   - 选择安装方式

     1. 可以选择最小化（适合编程），也可以选择正常安装（初学者可以看一看）

        ![选择安装模式](git_picture/选择安装模式.png)

5. __以下步骤，如果不懂分区的概念，可以一值选择默认__

6. 自定义分区

   - 创建 `/boot` 分区，主分区。__注意：`/boot` 设备文件名为 `sda1`__

     1. 大小一般为 500MB-1GB 足够使用了

   - 创建 `swap` 交换分区，主分区。__注意分区格式为 `swap`__

     1. 大小一般为 1GB 足够，实际不能提上性能

   - 创建 `/home` 分区，主分区

     1. 根据自己需求使用，一般为 5-10GB

   - 创建 `/` 分区，逻辑分区（剩余全部空间）

   -  创建分区

     1. 新建分区表

        ![新建分区表](git_picture/新建分区表.png)

     2. 设置分区大小，和分区文件格式

        ![创建各个分区](git_picture/创建各个分区.png)

7. 选择时区及用户设置

   - 时区为 __shanghai__ (没有北京)

     1. 一般默认即可

        ![选择时区](git_picture/选择时区.png)

   - 用户设置

     1. 设置是登录 __普通__ 用户及密码（与其他 Linux 发行版不同）

        ![设置用户](git_picture/设置用户.png)

   - 等待安装完成

     ![等待安装](git_picture/等待安装.png)

#### 虚拟机网络连接

1. 网络设置

   - 在虚拟机设置中 __网络适配器中__ 网络连接分为 4 种情况

     1. 虚拟机设置

        ![虚拟机网络设置](git_picture/虚拟机网络设置.png)

   - __桥接__：虚拟机利用真实网卡与本机通信（本地机联网）。好处配置简单，只需要设置与真实机同一网段的 IP即可，还可以与局域网中同网段的计算机通信。在网络中相当于一个真正的计算机存在，所以本机联网则可以与其他计算机通信。

     1. 使用真实的网卡进行通信（好像桥接得联网，才可以与本地机通信和网络其他计算机）

        ![桥接](git_picture/桥接.png)

   - __NAT__：使用虚拟机的虚拟网卡 `VMnet8` 与本机通信（本地机不用联网）。不能与局域网中的同网段计算机通信。本地计算机联网虚拟机则可以正常访问互联网。

   - __Host-only__：只能与本机通信（本地机不用联网），使用虚拟网卡 `VMnet1` 与本机通信。不能与局域网中的同网段计算机通信。本地真实机联网虚拟机也不可以访问互联网。

     1. 选择此网络链接时，虚拟机的 IP 要与 `VMnet1` 的 IP 要在同一网段（Ubuntu 自动分配），可以只用 `ping IP` 查看是否可以进行通信。

        ![Host-only链接](git_picture/Host-only链接.png)

   - 自定义：

2. 根据 4 种网络链接得特性，只需要使用 __桥接__ 和 __Host-only__ 两种链接即可

## Linux 文件扩展名

### Linux 不靠扩展名区分文件类型

__说明：以下是扩展名类型只是使用习惯，方便使用者识别__

1. 表现

| 压缩包      | 二进制软件包 | 网页文件 | 脚本文件 | 配置文件 |
| ----------- | ------------ | -------- | -------- | -------- |
| `*.gz`      | `.rmp`       | `.html`  | `.sh`    | `*.conf` |
| `*.bz2`     |              | `.php`   |          |          |
| `*.tar.bz2` |              |          |          |          |

## Linux 目录及作用

### 以表格的形式说明主要目录作用

1. 主要介绍以下目录

   - `/bin` `/sbin` `/usr/bin/` `/boot` `/dev/` `/etc/` `/home/` `/lib/` `lost + found` `/media/` `/mnt/` `/misc` `/opt/` `/proc/` `/sys/` `/root/` `/srv/` `/temp/`   `/usr/` `/var/` 

   - 表格

     | 目录           | 目录作用                                                     |
     | -------------- | ------------------------------------------------------------ |
     | /bin/          | 存放系统命令的目录，普通用户和超级用户都可以操作。不过放在 /bin 下的命令在在单用户模式下也可以执行 |
     | /sbin/         | 保存和系统环境设置相关的命令，只有超级用户可以使用这些命令进行系统环境设置，但有些命令可以允许普通用户查看 |
     | /usr/bin/      | 存放系统命令的目录，普通用户和超级用户都可以操作。这些命令和系统启动无关，在单用户模式下不能执行 |
     | /usr/sbin/     | 存放根文件系统不必要的系统管理命令，例如多少服务程序。只用超级用户可以使用。<br> 其实可以注意到 Linux 的系统，所有在 sbin/ 目录中保存的命令只有超级用户可以使用，bin 目录保存的命令所有用户都可以使用 |
     | /boot/         | 系统启动目录，保存系统启动相关的文件，如内核文件和启动引导程序（grup）文件等 |
     | /dev/          | 设备文件保存位置，在 Linux 所有内容以文件的形式保存，包括硬件。那么这个目录就是保存所有硬件设备文件的 |
     | /etc/          | 配置文件保存位置，系统内所有默认安装方式（rpm 二进制安装）的服务的配置文件全部保存到这个目录当中，如用户账户和密码、服务的启动脚本和常用服务的配置文件 |
     | /home/         | 普通用户的家目录，建立每个用户时，每个用户要有一个默认登录位置，这个位置就是用户的家目录。所有普通用户的家目录，就是在 /home 下建立一个和用户名相同的目录，如用户名 ss 的家目录就是 /home/ss/ |
     | /lib/          | 系统调用的函数库保存位置                                     |
     | /lost + found/ | 当系统崩溃或意外关机，而产生一些文件碎片存放在这里。当系统启动的过程中 fsck 工具会检查这里，并修复已经损坏的文件系统。这个目录只在每个分区中出现。 |
     | /media/        | 挂在目录，系统建议是用来挂在媒体设备的，例如软盘和光盘       |
     | /mnt/          | 挂载目录，早期 Linux 只有这一个挂在目录，并没有细分，现在这个目录系统建议挂在额外设备，如 U 盘，移动硬盘和其他操作系统的分区 |
     | /misc/         | 挂在目录，系统建议用来挂在 NFS 服务的公共目录。只要是已建立好的空目录就可以作为挂在点，系统准备了 3 个挂载点 `/medaia /mnt /misc` ，根据自己的喜好选择挂在点，也可以在 `/mnt` 建立目录挂在 |
     | /opt/          | 第三方软件安装位置，这个目录就是放置和安装其他软件的位置，手动安装的源码包软件都可以安装到这个目录中。__不过我们更习惯于把软件放置到  `/usr/local`  目录中，也就说 `/usr/local` 目录也可以用来安装软件__ |
     | /proc/         | 虚拟文件系统，该目录的数据并不保存在硬盘当中，而是保存在内存当中。主要保存系统的内核、进程、外部设备和网络状态。如 `/proc/cpuinfo` 是保存 cpu 的信息；`/proc/devices` 是保存设备驱动的列表；`/proc/filesystem` 是保存文件列表的；`/proc/net` 是保存网络协议信息 |
     | /sys/          | 虚拟文件系统，和 `/proc` 相似，都是保存在内存当中，主要保存内核相关的信息 |
     | /root/         | 超级用户的家目录，普通用户的家目录 `/home/`ss 下，超级用户的家目录直接在 `/` （超级用户只有一个） |
     | /srv/          | 服务数据目录，一些系统启动之后，可以在这个目录保存所需要的数据 |
     | /tmp/          | 临时目录，系统存放临时文件的目录，该目录所有用户都可以访问和写入。__建议该目录不要保存重要数据，因为每次开机都要把该目录清空__ |
     | /usr/          | 系统软件资源目录（注意 `usr` 不是 `user` 的缩写，而是 `Unix Software Resource ` 的缩写），所以不是用来保存用户的数据，而是存放系统软件资源的目录，系统中安装的软件大部分保存在这里 |
     | /var/          | 动态数据保存位置，主要保存缓存、日志、软件运行所产生的文件   |


## Linux 常用命令

### 大纲

1. 文件处理命令
2. 权限管理命令
3. 文件搜索命令
4. 帮助命令
5. 用户管理命令
6. 压缩解压命令
7. 网络命令
8. 关机重启命令

### 命令格式

1. 命令格式

   - `命令 [-选项] [参数]` 选项：选择功能，参数：命令操作的对象

   - 例（多个参数写在一起 ）

     `ls -la /etx`

2. 参考

   - 个别命令使用不遵循此格式
   - 当有多个选项时，可以写在一起
   - 简化选项与完整选项 `-a` 等于 `-all`

## Linux 链接

### 软链接

1. 软链接介绍

   - 软链接类似于 windows 的快捷方式，只是源文件的一个指向，文件和目录都可以使用软链接，方便管理

   - 使用 `ls -l` 查看当前目录下的有所（包括目录和文件） long 信息，权限显示 `lrwxrwxrwx` 的则表示是软链接，且显示了源文件的指向

     ```shell
     ss@localcomputer:~/桌面$ ls -l
     总用量 4
     lrwxrwxrwx 1 ss ss   11 12月 10 22:17 a -> ./tmp/a.txt # 软连接，且指向源文件
     drwxr-xr-x 3 ss ss 4096 12月  9 22:31 tmp
     ```

   - `lrwxrwxrwx` 为 10 个子符，第 1 个字符 `l` 表示文件类型（文件、目录、软链接），第 2-4 的字符表示所有者权限，第 5-7 的字符表示所属组的权限，最后 3 个字符表示其他用户的权限，__然而软链接的操作权限最后是要源文件的操作权限决定的，和软链接显示的权限无管__

     | 字符位置 | 作用                                                    | 所属 | 权限(r\w\x)        |
     | -------- | ------------------------------------------------------- | ---- | ------------------ |
     | 1        | 表示文件类型(`-`表示文件、`d` 表示目录、`l` 表示软链接) |      |                    |
     | 2-4      | 表示 user（所属者） 的操作权限                          | u    | 可读、可写、可执行 |
     | 5-7      | 表示所属组（group）的操作权限                           | g    | 可读、可写、可执行 |
     | 8-10     | 表示其他用户（other）的操作权限                         | o    | 可读、可写、可执行 |

2. 使用

   - 命令 `ln -s 源文件 目标文件` （目录、文件都可以产生软链接），一般使用 `.soft` 表示软链接

     ```shell
     # 显示当前目录内容
     ss@localcomputer:~/桌面$ ls
     tmp
     # 设置软链接
     ss@localcomputer:~/桌面$ ln -s ./tmp/a.txt ./a.soft
     # 使用 -l 选项显示
     ss@localcomputer:~/桌面$ ls -l
     总用量 4
     lrwxrwxrwx 1 ss ss   11 12月 11 12:55 a.soft -> ./tmp/a.txt
     drwxr-xr-x 3 ss ss 4096 12月  9 22:31 tmp
     ```

   - 软链接目标文件几乎不占内存

### 硬链接

1. 硬链接介绍

   - 硬链接类似于 `cp -p` 复制文件或目录，且保留源文件属性（即文件类型、权限、最后修改时间），__但是有所不同硬链接是和源文件同步更新的__（硬链接等于 `copy -l` 也会同步更新）
   - __硬链接只能在文件中使用，目录不能使用硬链接，且只能在相同分区中使用__

2. 使用

   - 命令 `ln 源文件 目标文件` 只适用于文件

     ```shell
     # 使用 ls -l 显示长信息
     ss@localcomputer:~/桌面$ ls -l
     总用量 4
     lrwxrwxrwx 1 ss ss   11 12月 11 12:55 a.soft -> ./tmp/a.txt
     drwxr-xr-x 3 ss ss 4096 12月  9 22:31 tmp
     # 设置硬链接
     ss@localcomputer:~/桌面$ ln ./tmp/a.txt ./a.unsoft
     # 显示
     ss@localcomputer:~/桌面$ ls -l
     总用量 8
     lrwxrwxrwx 1 ss ss   11 12月 11 12:55 a.soft -> ./tmp/a.txt
     -rw-r--r-- 2 ss ss  146 12月  9 22:31 a.unsoft # 硬链接目标文件
     drwxr-xr-x 3 ss ss 4096 12月  9 22:31 tmp
     ss@localcomputer:~/桌面$ ls -l ./tmp/a.txt 
     -rw-r--r-- 2 ss ss 146 12月  9 22:31 ./tmp/a.txt # 源文件
     ```

   - 硬链接源文件和目标文件大小什么相同

3. 查看硬链接

   说明：硬链接和复制几乎相同，那怎么可以确定一个文件是硬链接呢？

   - 使用 `ls -i` 查看文件的 `inode` 节点，`inode` 节点相同的是硬链接（硬链接只适用于文件）

     ```shell
     ss@localcomputer:~/桌面$ ls -i ./a.soft ./a.unsoft ./tmp/a.txt 
     430 ./a.soft  461 ./a.unsoft  461 ./tmp/a.txt
     ```

   - 硬链接的 `inode` 相同，软链接不同

## Linux 权限问题

__说明：Linux 会有很多权限问题__

### 文件权限

1. 文件权限说明

   - r w x 解释（数字和二进制有关）

     | 代表字符 | 代表数字 | 权限     | 对文件的含义     | 对目录的含义               |
     | -------- | -------- | -------- | ---------------- | -------------------------- |
     | r        | 4        | 读权限   | 可以查看文件内容 | 可以列出目录中的内容       |
     | w        | 2        | 写权限   | 可以修改文件内容 | 可以在目录中创建、删除文件 |
     | x        | 1        | 执行权限 | 可以执行文件     | 可以进入目录               |
     
   - __注意：删除文件是需要对目录有 w 的权限的，对文件修改是要对文件有 w 权限__

   - 对目录的权限一般 r x 是同时出现的，如对文件有 rwx 权限，但对目录没有 rwx 权限，则查看不了，进入不了

2. 文件权限修改 __（只能超级管理员或所属者可以修改文件权限）__

   - 命令 `chmod [{ugoa} {+-=}] [文件或目录]` 

     1. u 所有者；g 所属组；o 其他人；a 表示所有人，使用 `+\-\=` 表示对文件的权限的添加、删除、等于哪些权限

     2. 演示

        ```shell
        ss@localcomputer:~/桌面/tmp$ ls -l
        总用量 8
        -rw-r--r-- 2 ss ss  146 12月  9 22:31 a.txt
        drwxr-xr-x 2 ss ss 4096 12月  9 22:01 test
        ss@localcomputer:~/桌面/tmp$ chmod o=rw a.txt # 修改权限 (chmod o+w a.txt 添加w权限)
        ss@localcomputer:~/桌面/tmp$ ls -l
        总用量 8
        -rw-r--rw- 2 ss ss  146 12月  9 22:31 a.txt
        drwxr-xr-x 2 ss ss 4096 12月  9 22:01 test
        ```

   - 命令 `chmod [421] [文件或目录]` 

     1. 使用数字表示操作权限，如 `-rw-r--r--` 则表示 `644`

     2. 演示

        ```shell
        ss@localcomputer:~/桌面/tmp$ ls -l
        总用量 8
        -rw-r--rw- 2 ss ss  146 12月  9 22:31 a.txt
        drwxr-xr-x 2 ss ss 4096 12月  9 22:01 test
        ss@localcomputer:~/桌面/tmp$ chmod 777 a.txt  # 修改权限
        ss@localcomputer:~/桌面/tmp$ ls -l
        总用量 8
        -rwxrwxrwx 2 ss ss  146 12月  9 22:31 a.txt
        drwxr-xr-x 2 ss ss 4096 12月  9 22:01 test
        ```

     3. 选项 `-R` 表示递归修改权限，改变了目录及目录下的文件权限

### 文件所有者更改

1. 介绍文件的所有者

   - 文件所有者即使创建文件的用户 `代号 o`

   - 文件的操作权限 __超级管理员、所有者__ 可以修改

   - 文件所有者只能由 __超级管理员__ 修改

   - 使用管理员创建一个用户 `useradd 用户名` ，设置密码（也可以不设置）  `passwd 用户名`

     ```shell
     root@localcomputer:/home/ss/桌面/tmp# useradd s # 创建用户 s
     root@localcomputer:/home/ss/桌面/tmp# passwd s # 为 s 设置密码
     输入新的 UNIX 密码： 
     重新输入新的 UNIX 密码： 
     passwd：已成功更新密码
     
     ```

2. 命令 `chown [用户] [文件及目录]`

   __说明：此 Linux 系统存在超级管理员 root、普通用户 ss、s 两个__

   - 只有 root 可以修改，文件所有者也没有权限修改

   - 演示；使用 ss 创建文件（文件所有者是 ss），使用 ss 用户修改文件所有者，将其修改为 s

     ```shell
     ss@localcomputer:~/桌面/tmp$ ls -l
     总用量 0
     -rw-r--r-- 1 ss ss 0 12月 13 22:01 a.txt
     ss@localcomputer:~/桌面/tmp$ chown s a.txt 
     chown: 正在更改'a.txt' 的所有者: 不允许的操作 # 不允许修改
     ```

     __不允许修改__

   - 演示；使用 root 修改文件所有者，将其修改为 s（__组也一起修改 `chown 用户:组 文件`__）

     ```shell
     root@localcomputer:/home/ss/桌面/tmp# chown s a.txt  # 修改成功
     root@localcomputer:/home/ss/桌面/tmp# ls -l
     总用量 0
     -rw-r--r-- 1 s ss 0 12月 13 22:01 a.txt
     ```
     

__修改文件所有者，只能 root 修改__

3. 注意事项

   - 修改的用户，必须系统已经存在

### 文件所属组更改

1. 介绍

   - 使用管理员创建一个所属组 `groupadd 组名`

2. 命令 `chgrp [用户组] [文件或目录]` 或者 `chown :[用户组名] 文件`

   - 修改文件所属组为 g

     ```shell
     ss@localcomputer:~/桌面/tmp$ ls -l
     总用量 0
     -rw-r--r-- 1 s ss 0 12月 13 22:01 a.txt              # 使用管理员修改
     root@localcomputer:/home/ss/桌面/tmp# chgrp g a.txt  # 更改所属组
     root@localcomputer:/home/ss/桌面/tmp# ls -l          # 显示已经更改
     总用量 0
     -rw-r--r-- 1 s g 0 12月 13 22:01 a.txt
     ```


### 创建文件的默认权限管理

1. 介绍

   - 有关新建文件及目录的默认权限问题
   - 目录和文件的默认权限有所不同。虽然两者使用的是一个默认权限，但文件的默认权限是不能有 `x` 权限。

2. 文件新建的默认权限

   - `-rw-rw-r-- 1 ss ss 0 12月 14 21:36 a` 
   - user 具有读写权限
   - group 具有读写权限
   - other 具有读权限

3. 目录新建的默认权限

   - `drwxrwxr-x 2 ss ss 4096 12月 14 21:38 b`
   - user 具有浏览、创建删除、进入目录权限
   - group 具有浏览、创建删除、进入目录权限
   - other 具有浏览、进入目录权限

4. 使用 `umask -S`  查看创建文件的默认权限

   - 演示

     ```shell
     ss@localcomputer:~/桌面/tmp$ umask -S
     u=rwx,g=rwx,o=rx
     ```

     user = rwx    g = rwx    o = rx

   - `umask -S` 显示创建文件默认权限

     1. 和文件默认权限对比，少了 x 这个权限
        - 在 Linux 中的定义，缺省创建的文件是不具有可执行的权限（任何用户都没有这个 x 权限）
        - 病毒、攻击程序都是可执行文件，如果是不可执行的，减少风险
     2. 和目录的创建默认权限相同
        - 在目录中 x 权限是进入目录的权限
   
5. 使用 `umask` 查看的 `0022` 含义

   - 演示（使用超级管理员），但有时根据 __用户的不同，显示的数字也会不一样，但是转换原理一样__

     ```shell
     root@localcomputer:/home/ss/桌面/tmp# umask -S
     u=rwx,g=rx,o=rx
     root@localcomputer:/home/ss/桌面/tmp# umask
     0022
     ```

   - 比对（超级管理员权限，普通用户 `0002` 比对原理一样）

     ```tex
     	0 0 2 2
     第一个 0:特殊权限
     
     777: rwx rwx rwx
     022: --- -w- -w-  (异或)       [r=4, w=2, x=1, wx=3……]
     ----------------------------
     	 rwx r-x r-x  (目录权限)
     	 rw- r-- r--  (文件权限)
     ```

     文件比目录少 `x` 权限，因为这是 `Linux` 的保护措施

6. 修改创建文件权限

   - 使用 `umask [002]` 修改默认创建文件权限

     ```shell
     ss@localcomputer:~/桌面/tmp$ umask	# 显示默认创建文件的权限
     0002
     ss@localcomputer:~/桌面/tmp$ touch a
     ss@localcomputer:~/桌面/tmp$ umask 077	# 修该默认权限
     ss@localcomputer:~/桌面/tmp$ umask
     0077
     ss@localcomputer:~/桌面/tmp$ touch b
     ss@localcomputer:~/桌面/tmp$ ls -l
     总用量 0
     -rw-rw-r-- 1 ss ss 0 12月 17 20:18 a		# 文件创建权限不一致
     -rw------- 1 ss ss 0 12月 17 20:19 b
     ```

   - 使用 `777` 于权限代码 __异或__ 得默认权限

   - 不推荐使用

   - 文件创建的权限默认没有 `x` ，即使有 `x`也会在创建时，抹掉；目录 `x` 权限是进入目录操作。

## Linux 文件搜索命令

### find 使用

1. find 命令原理及使用格式

   - 遍历整个分区及硬盘

   - `find 路径 [-选项] 文件名`

2. 特殊字符

   - `*name*` 包含 name 得文件都搜索
   - `name??` 以 name 开头得以 2 个位置字符结尾得文件

3. 选项

   - 表格

     | 选项         | 作用              | 注意                                                         | 演示                                                         |
     | ------------ | ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
     | -name        | 以文件名搜索      | `*` 表示任意多字符，`?` 表示一个位置字符                     |                                                              |
     | -iname       | 以文件名搜索      | 不区分大小写                                                 |                                                              |
     | -size        | 以文件大小搜索    | Linux 最小存储单元为数据块（512字节=0.5k），搜索大小需用数据块表示，+ 表示大于 - 表示小于 =表示正好等于 | 搜索根目录小大于 1MB 得文件 `find / -size +2048` <br> 2048 个数据块（2048 * 0.5k = 1024KB = 1MB） |
     | -user/-group | 以所有者查找文件  | 不是以文件名，而是以所有者为参数，查找所有者得文件           | 查找根目录下所有者为 ss 的文件 `find / -user ss`             |
     | -amin        | 访问时间          | 在 5 分钟之内访问的文件及目录 `-5` ，a 为 access 缩写        |                                                              |
     | -cmin        | 文件属性          | 在一定时间内修改文件属性的，c 为 change 缩写                 |                                                              |
     | -mmin        | 文件内容          | 在一定时间内修改文件内容，m 为 modify 缩写                   |                                                              |
     | -inum        | 以 inode 节点查找 | `ls -i` 查找 i 节点，以文件 inode 节点删除文件，__还可以查找硬链接__ |                                                              |

4. 两个条件在查找中的使用

   - `-a` 表示两个条件同时满足

   - `-o` 表示满足其中一个即可

   - `-type` 表示查找类型

     1. `d` 查找目录
     2. `f` 查找文件
     3. `-l` 软链接

   - 演示

     ```shell
     find / -name a* -a -type f	# 查找根目录下，以 a 开头的文件，-a 表示 and 
     ```

5. 对查找到的结果执行操作

   - 实例 `find / -name inittab -exec ls -l {} \;`

   - 在根目录下，查找 inittab 文件，并显示其详细信息

   - `-exec` 或者 `-ok 命令 {} \` 格式，`-0k` 询问一次，`-exec` 不询问

   - {} 表示结果不止一个，\ 表示转义，即替换查找结果，；表示结束

   - 实例

     ```shell
     ss@localcomputer:~/桌面/tmp$ find ./ -name a* -ok ls -l {} \;
     < ls ... ./a > ? y
     -rw-rw-r-- 1 ss ss 0 12月 17 20:18 ./a
     
     # 查找 a 文件，并删除
     ss@localcomputer:~/桌面/tmp$ find ./ -name a -type f -ok rm {} \;
     < rm ... ./a > ? y
     ```

6. 以 inode 查找文件，并删除（文件名有空格，使用 ""，但还是删除不掉，使用以下方法）

   - 演示

     ```shell
     ss@localcomputer:~/桌面$ ls -i
     456 tmp
     ss@localcomputer:~/桌面$ find ./ -inum 456 -ok rm -r {} \;	# 删除 inode 为 456 的文件
     < rm ... ./tmp > ? y
     find: ‘./tmp’: 没有那个文件或目录
     ```


### locate 使用

1. 使用原理及格式

   - 不会像 `find` 遍历整个目录、分区或硬盘，`locate` 会建立文件资料库，并定期跟新文件资料库（快表），且在建立的文件资料库中查找（速度快哦）

   - `locate 文件名称` 。`*` 表示任意多个字符（没有也可以），`?` 表示一个字符
   - __包含文件名的都会被查找出来，但是查找区分大小写（添加选项 `-i` 忽略大小写）__

2. 演示

   - 查找已建立好的文件（隔天建立的）

     ```shell
     ss@localcomputer:~/桌面/tmp$ ls
     a  abcdefg.txt
     ss@localcomputer:~/桌面/tmp$ locate abcdefg.txt 	# 秒搜
     /home/ss/桌面/tmp/abcdefg.txt
     ss@localcomputer:~/桌面/tmp$ 
     ```

   - 查找 locate 建立的文件资料库 `locate locate`

     ```shell
     ss@localcomputer:~/桌面/tmp$ locate locate
     ...
     ...
     /var/lib/mlocate/mlocate.db 			# 很多文件，只取一个
     ...
     ```

3. 问题及解决办法

   - 新建文件，`locate` 并没有刷新资料库，所以 `locate` 找不到文件（`locate` 基于文件资料库的查找）

   - 使用 `updatedb` （使用超级管理员权限）,更新资料库，再次使用 `locate` 就可以查找到新建的文件了

     ```shell
     ss@localcomputer:~/桌面/tmp$ touch abcde			# 创建一个文件
     ss@localcomputer:~/桌面/tmp$ ls
     a  abcde  abcdefg.txt
     ss@localcomputer:~/桌面/tmp$ locate abcde			# locate 搜索不到
     /home/ss/桌面/tmp/abcdefg.txt
     ss@localcomputer:~/桌面/tmp$ updatedb				# 普通用户无权限使用 updatedb
     updatedb: 无法为 `/var/lib/mlocate/mlocate.db' 打开临时文件
     ss@localcomputer:~/桌面/tmp$ sudo su				# 切换超级管理员
     [sudo] ss 的密码： 
     root@localcomputer:/home/ss/桌面/tmp# updatedb	 # 更新资料库
     root@localcomputer:/home/ss/桌面/tmp# su ss
     ss@localcomputer:~/桌面/tmp$ locate abcde			# 可以查找到
     /home/ss/桌面/tmp/abcde
     /home/ss/桌面/tmp/abcdefg.txt
     ss@localcomputer:~/桌面/tmp$ 
     ```

   - ~~如果新建文件在更目录下的临时文件目录下（`/tmp/`），`locate` 是无法找到的（`updatedb` 也不好用）~~

### which  \ whereis 用法

1. 介绍

   - 两个命令都是用于搜索 __命令__ 的，但是两者搜索结果有所不同
   - `which` 结果是命令所在路径
   - `whereis` 结果是命令所在路径及帮助文档（命令帮助信息、配置文件帮助信息）所在路径

2. 演示

   - which 演示

     ```shell
     ss@localcomputer:/$ which cp
     /bin/cp
     ss@localcomputer:/$ which rm
     /bin/rm
     ```

   - whereis 演示

     ```shell
     ss@localcomputer:/$ whereis cp
     cp: /bin/cp /usr/share/man/man1/cp.1.gz
     ss@localcomputer:/$ whereis rm
     rm: /bin/rm /usr/share/man/man1/rm.1.gz
     ```

3. 别名（Alias）

   - 命令存在别名，实际上我们运行的命令是使用的别名

   - 如删除命令 `rm` 别名 `rm -i` 就是询问是否删除

     ```shell
     ss@localcomputer:~/桌面/tmp$ rm -i abcde
     rm：是否删除普通空文件 'abcde'？ y			# 删除前询问
     ss@localcomputer:~/桌面/tmp$ 
     ```
     
- Linux 命令本身名由询问，执行命令时，会先去查找别名
  
- 使用 `which` 查找命令就可以显示别名（Ubunt 没有，不知道为什么）

### grep

1. 功能描述及用法

   - 在文件中所寻字符串匹配的行并输出

   - `grep [选项] [指定字符串][文件]`

   - 选项

     1. `-i`： 不区分大小写

     2. `-v`： 排除指定字符串

     3. `-n`： 输出行号
     4. `--color=auto`：搜索的关键字用颜色显示

2. 演示

   - `-i` 演示

     ```shell
     ss@localcomputer:~/桌面/tmp$ grep -i jack a
     jack
     JACK
     ```

   - `-v` 屏蔽指定字符串行，一般用于剔除注释 ，以 `#`  开头的行

     `grep -v ^# 文件名称` 以 `#` 开头的行

## Linux 帮助命令

__说明：帮助信息分为很多中，1  表示命令帮助；5 表示配置文件的帮助__

### man 使用

__说明：下载中文 man `sudo apt install manpages-zh`__

1. 功能描述及使用语法

   - __获取帮助信息，查看配置文件信息__
   - `man [命令或配置信息]`
   - 在 `man` 命令交互界面
     1.  `/字符串` 搜索字符串，使用 `n` 查看下一个，`shift+n` 上一个
     2. __空格__ 或 `f` 向下翻页，`b` 向上翻页
     3. `enter` 下一行

2. 注意

   - 查看配置文件信息，不要使用绝对路径（/etc/services）

   - 查看配置文件信息，直接使用配置文件名（services）就可以

   - 配置文件
     1. 查看配置文件的介绍
     2. 配置文件的格式
     3. `man services` 网络服务配置信息
     4. `man 5 passwd`  查看 __密码文件__
     
   - 帮助手册类型

     1. 使用 `whereis 命令` 查看各种帮助文档信息。__有的命令也会有相应配置文件信息，如下 `passwd` 命令__

        ```shell
        shen@shen-Ubuntu:~$ whereis passwd
        passwd: /usr/bin/passwd /etc/passwd /usr/share/man/man1/passwd.1ssl.gz /usr/share/man/man1/passwd.1.gz /usr/share/man/man5/passwd.5.gz
        ```

     2. `passwd.1.gz` 是 __命令 __ 帮助信息

     3. `passwd.5.gz` 是 __配置文件__  帮助信息

3. 使用其他命令查看帮助信息

   - `man` 显示内容全面，但过于全面。

   - `whatis 命令` ，简要介绍命令的使用目的

     ```shell
    ss@localcomputer:~$ whatis ls
     ls (1)               - 列目录内容
     ```
   
   - `apropos 命令` ，显示与命令有关（包含此命令字符串的各种命令）的各种帮助文档类型信息

     ```shell
   	 ss@localcomputer:~$ apropos inittab		# inittab 是配置文件信息
     inittab (5)          - 与 sysv 兼容的 init 进程使用的初始化文件...
     #####################################################
     shen@shen-Ubuntu:~$ apropos passwd 		# 与命令有关的各种帮助信息
     chpasswd (8)         - 批量更新密码
    gpasswd (1)          - 管理员 /etc/group 和 /etc/gshadow
     mkpasswd (1)         - 为用户产生新口令
     passwd (1)           - 更改用户密码
     passwd (5)           - 密码文件
     smbpasswd (5)        - Samba加密的口令文件。
     smbpasswd (8)        - 改变用户的SMB口令
     yppasswd (1)         - 修改你在NIS数据库中的密码
     chgpasswd (8)        - update group passwords in batch mode
     ```
   
   - 简单，很明了

### info 用法

1. `info` 与 `man` 命令没有本质的区别

### help 用法

__说明：查看内置命令，`man` 查看不了内置命令__

1. 用法

   - `help 内置命令`

2. 获取主要选项信息

   - `命令 --help`

3. 确定内置命令，`cd` 内置命令

   - `which 命令`

     ```shell
     ss@localcomputer:~$ which cd
     ss@localcomputer:~$ 
     
     ```

   - `whereis 命令`

     ```shell
     ss@localcomputer:~$ whereis cd
     cd:
     ss@localcomputer:~$ 
     ```


## Linux 用户管理

### useradd \ passwd 简单使用

1. 命令使用（超级管理员使用）
   - 创建用户 `useradd 用户名` 
     1. 只是增加了用户的基本信息，如：家目录，没有验证密码
   - 设置用户登录验证密码 `passwd 用户名` 
2. 注意事项
   - 普通用户可以更改自身密码 `passwd`，但是必须符合密码设置规范
   - 超级管理员，可以更改所有用户密码，不用遵循密码设置规范

### who  \ w 使用

1. who 功能描述及用法

   - 查看登录用户信息

   - 格式 `who` 

   - 登录显示格式（4 个显示项目）

     1. 表格

        | 用户名 | 登录终端                                                     | 登录时间         | 登录的主机 IP 地址 |
        | ------ | ------------------------------------------------------------ | ---------------- | ------------------ |
        | ss     | :0（表示本地登录）                                           | 2019-12-22 20:01 | (:0) 表示本地登录  |
        |        | tty 表示本地登录、pts 表示远程登录，使用不同的终端号表示不同的终端 |                  | 没有写则是本地登录 |

2. w 功能使用

   - 和 `who` 功能相同，但比 `w` 显示信息更为全面

   - 格式 `w`

   - 登录显示格式

     1. 表格（1）

        | 系统时间 | 连续运行时间                                 | 用户数 | 负载均衡指数                                                 |
        | -------- | -------------------------------------------- | ------ | ------------------------------------------------------------ |
        | 21:36:15 | up  1:35                                     | 1 user | load average: 0.00, 0.01, 0.00                               |
        |          | 衡量一个系统是否稳定（运行了多长时间不关机） |        | 3 个数值是 描述近期 1、5、15 分钟内系统负载情况（CPU、内存情况），可以判断系统负载情况 |

     2. 表格（2）

        | IDLE（累计系统空闲时间） | JCPU（）                                    | PCPU                          | WHAT           |
        | ------------------------ | ------------------------------------------- | ----------------------------- | -------------- |
        |                          | 累计占用 CPU 的是时间（从登录开始计算累计） | 当前执行的命令占用 CPU 的时间 | 当前执行的命令 |

   - 演示

     - `who` 演示

       ```shell
       root@localcomputer:/home/ss# who
       ss       :0           2019-12-22 20:01 (:0)
       # 用户   # ttp/0本地登录 # 登录时间      # 登录主机 IP (:0) 本地登录
       ####################
    shen@shen-Ubuntu:~$ who
       shen     :0           2020-04-12 20:58 (:0)		# 本地登录
   		 	shen     pts/1        2020-04-12 21:27 (192.168.253.1)		# 远程登录
       ```
     
     - `w` 演示
     
       ```shell
       root@localcomputer:/home/ss# w
        21:55:55 up  1:54,  1 user,  load average: 0.01, 0.00, 0.00
       USER     TTY      来自           LOGIN@   IDLE   JCPU   PCPU WHAT
       ss       :0       :0               20:01   ?xdm?  41.98s  0.02s /usr/lib/gdm3/gdm
       #################################
       shen@shen-Ubuntu:~$ w
        21:27:54 up 29 min,  2 users,  load average: 0.08, 0.05, 0.02
       USER     TTY      来自           LOGIN@   IDLE   JCPU   PCPU WHAT
       shen     :0       :0               20:58   ?xdm?  27.91s  0.01s /usr/lib/gdm3/gdm-x-session --run-scri		# 本地登录
       shen     pts/1    192.168.253.1    21:27   19.00s  0.04s  0.04s -bash		# 远程登录
       ```

## Linux 压缩解压命令

__说明：压缩便于备份、传输和病毒一般很难感染压缩文件，.zip 在 Linux 和 Windows 都可以直接使用__

### 压缩格式  .gz

__说明：在 Linux 中非常常见的一种压缩格式__

#### gzip \ gunzip(gzip -d) 使用，不保留原始文件

1. 压缩 `gzip` 命令的使用

   - 语法 `gzip 文件名`
   - __只能压缩文件，压缩比很理性，也不保存源文件__

2. 解压 `gunzip` 命令使用

   - 语法 `gunzip 压缩文件名`
   - 另一种 `gzip -d 压缩文件名`

3. 演示

   - 压缩

     ```shell
     ss@localcomputer:~/桌面/tmp$ ls -l
     总用量 4
     -rw-r--r-- 1 ss ss 163 12月 23 21:51 a
     ss@localcomputer:~/桌面/tmp$ gzip a		# 压缩文件
     ss@localcomputer:~/桌面/tmp$ ls -l
     总用量 4
     -rw-r--r-- 1 ss ss 154 12月 23 21:51 a.gz
     ```

   - 解压

     ```shell
     
     ss@localcomputer:~/桌面/tmp$ ls -l
     总用量 4
     -rw-r--r-- 1 ss ss 154 12月 23 21:51 a.gz
     ss@localcomputer:~/桌面/tmp$ gunzip a.gz 		# 解压文件
     ss@localcomputer:~/桌面/tmp$ ls -l
     总用量 4
     -rw-r--r-- 1 ss ss 163 12月 23 21:51 a
     
     ```

#### tar 使用（打包\压缩）

1. 介绍

   - `gzip` 命令不可以压缩目录，且不保留源文件
   - `tar` 可以压缩目录
     1. 先将目录打包（打包和压缩不同），目录打包后的文件格式`.tar`
     2. 在进行压缩
     3. 保留源文件

2. 压缩 `tar` 命令使用

   - 功能描述
     1. 打包目录
     2. 压缩后文件格式 `.tar.gz`

   - 选项表示

     1. `-c` ：打包
     2. `-v` ：显示详细信息
     3. `-f` ：指定文件名
     4. `-z` ：打包同时压缩

   - 语法

     1. 打包 `tar [-zcf] [压缩后文件名] [目录]` __（注意选项顺序）__
     2. 查看包里文件信息 `tar -tf [文件.tar.gz\.bz2]`

   - 演示

     1. 打包、压缩，分成两部完成

        ```shell
        ss@localcomputer:~/桌面$ tar -cvf tmp.tar tmp 	# 打包
        tmp/										  # 显示详细信息
        tmp/c/
        tmp/a
        tmp/b/
        ss@localcomputer:~/桌面$ ls -l
        总用量 16
        drwxr-xr-x 4 ss ss  4096 12月 23 22:07 tmp
        -rw-rw-r-- 1 ss ss 10240 12月 23 22:15 tmp.tar	# 打包文件
        
        ss@localcomputer:~/桌面$ gzip tmp.tar 			# 压缩
        ss@localcomputer:~/桌面$ ls -l
        总用量 8
        drwxr-xr-x 4 ss ss 4096 12月 23 22:07 tmp
        -rw-rw-r-- 1 ss ss  317 12月 23 22:15 tmp.tar.gz	  # 文件格式
        ```

     2. 打包压缩，一步完成

        ```shell
        ss@localcomputer:~/桌面$ tar -zcvf tmp.tar.gz tmp/	# 打包并进行压缩
        tmp/
        tmp/c/
        tmp/a
        tmp/b/
        ss@localcomputer:~/桌面$ ls -l
        总用量 8
        drwxr-xr-x 4 ss ss 4096 12月 23 22:07 tmp
        -rw-rw-r-- 1 ss ss  309 12月 23 22:21 tmp.tar.gz
        ```
        
     3. 查看包内信息
     
        ```shell
        ss@localcomputer:~/桌面$ tar -tf tmp.tar.gz	# 查看包内信息
        tmp/
        tmp/a.bz2
        tmp/c/
        tmp/a
        tmp/b/
        ```

3. 解压 `tar` 命令使用

   - 选项表示

     1. `-x` ：解包
     2. `-v` ：显示详细信息
     3. `-f` ：指定解压文件
     4. `-z` ：解压缩文件

   - 语法

     `tar -zxvf 解压文件.tar.gz`

   - 解压缩包

     1. 一步，完成

        ```shell
        ss@localcomputer:~/桌面$ ls
        tmp.tar.gz
        ss@localcomputer:~/桌面$ tar -zxvf tmp.tar.gz 	# 解压缩
        tmp/
        tmp/c/
        tmp/a
        tmp/b/
        ss@localcomputer:~/桌面$ ls -l
        总用量 8
        drwxr-xr-x 4 ss ss 4096 12月 23 22:07 tmp
        -rw-rw-r-- 1 ss ss  309 12月 23 22:21 tmp.tar.gz
        --------------------------------------
        # 解压到指定的目录（目录必须已存在），-C 指定目录
        shen@shen-Ubuntu:~$ tar -zxvf java.tar.gz -C javaTar
        ```

### 压缩格式 .zip

__说明：.zip linux 和 Windows 都支持，所以两个系统之间相互传文件可以使用__

#### zip \ unzip 使用

1. 介绍

   - 保留源文件
   - 可以解压缩文件，及目录使用 `-r` 选项
   - 压缩格式为 `.zip`

2. 功能及命令

   - 解压缩文件及目录

   - 压缩命令 `zip [-r] [压缩后文件] [文件及目录]` 

     `-r` 压缩目录

   - 解压缩命令 `unzip 文件`

3. 用法

   - 压缩文件

     ```shell
     ss@localcomputer:~/桌面/tmp$ ls -l
     总用量 12
     -rw-r--r-- 1 ss ss  163 12月 23 21:51 a
     ss@localcomputer:~/桌面/tmp$ zip a.zip a		# 压缩文件
       adding: a (deflated 18%)					# 显示压缩比
     ss@localcomputer:~/桌面/tmp$ ls -l
     总用量 16
     -rw-r--r-- 1 ss ss  163 12月 23 21:51 a
     -rw-r--r-- 1 ss ss  286 12月 24 22:09 a.zip	 # 文件格式 .zip
     ```

   - 压缩目录

     ```shell
     ss@localcomputer:~/桌面$ zip tmp.zip tmp
       adding: tmp/ (stored 0%)
     ss@localcomputer:~/桌面$ ls -l
     总用量 12
     drwxr-xr-x 4 ss ss 4096 12月 24 22:09 tmp
     -rw-rw-r-- 1 ss ss  309 12月 23 22:21 tmp.tar.gz
     -rw-r--r-- 1 ss ss  158 12月 24 22:12 tmp.zip
     ```

   - 解压缩

     ```shell
     ss@localcomputer:~/桌面$ unzip tmp.zip 
     Archive:  tmp.zip
        creating: tmp/
     ```

### 压缩格式 .bzip2

#### bzip2 \ bunzip2 使用

1. 介绍

   - 是 bzip 的升级模式
   - 可以保留源文件 `-k`
   - 压缩比惊人，比较大的文件推荐使用 bzip2
   - 压缩格式 `.bz2`
   - __只能压缩文件__
   - 可是和 `tar` 命令一起使用，先将目录打包，在进行压缩

2. 功能及命令

   - 解压缩文件

   - 压缩命令 `bzip2 [-k] [文件]` ，解压缩 `bunzip2 [-k] [压缩文件]`

     `-k` 表示压缩时，产生压缩文件保留源文件；解压时，保留压缩文件

   - 压缩目录 `tar -jcvf [压缩后文件名] [目录]` ，解压缩目录 `tar -jxvf 压缩文件.bz2`

3. 演示

   - 压缩保留源文件

     ```shell
     ss@localcomputer:~/桌面/tmp$ ls
     a  b  c
     ss@localcomputer:~/桌面/tmp$ bzip2 -k a
     ss@localcomputer:~/桌面/tmp$ ls
     a  a.bz2  b  c
     ```

   - 解压缩保留压缩文件

     ```shell
     ss@localcomputer:~/桌面/tmp$ ls
     a.bz2  b  c
     ss@localcomputer:~/桌面/tmp$ bunzip2 -k a.bz2 
     ss@localcomputer:~/桌面/tmp$ ls
     a  a.bz2  b  c
     ```

   - 和 `tar` 一起使用解压缩目录

     ```shell
     ss@localcomputer:~/桌面$ tar -jcvf tmp.tar.bz2 tmp/	# 压缩目录
     tmp/
     tmp/a.bz2
     tmp/c/
     tmp/a
     tmp/b/
     ss@localcomputer:~/桌面$ tar -jxvf tmp.tar.bz2 		# 解压目录
     tmp/
     tmp/a.bz2
     tmp/c/
     tmp/a
     tmp/b/
     ```
     

## Linux 网络命令

#### write 使用

1. 功能介绍及命令

   - 发送消息给登录的用户

   - 注意

     1. 超级管理员不允许远程登录

     2. __Linux 本地登录的用户，不接受 `write` 发送的消息（禁用 mesg），但是可以发送消息。远程登录用户可以使用 `write` 发送消息，并接受消息。如异常 `write: shen has messages disabled` ，使用 `mesg y` 开启 `write`__

        ```shell
        $ write ss
        write: ss has messages disabled
        ```

   - 命令 `write user`

     - `Enter` 发送    `ctrl + D`  结束

2. 演示

   - Linux 本机用户（管理员 root）可向远程登录用户发送消息

     ```shell
     # 发送消息
     root@localcomputer:/home/ss# write ss	# 命令格式
     hello
     world
     root@localcomputer:/home/ss# 		    # ctrl + D 结束
     
     # 接受消息
     ss@localcomputer:~$
     Message from ss@localcomputer on pts/0 at 21:44 ...    # 消息来源 pts 本地用户
     hello											   # pts/0 标识
     world
     EOF
     
     ss@localcomputer:~$
     ```

#### wall 使用

1. 功能及命令

   - `wall` 是 `write all` 的缩写。发送广播信息
   - 命令 `wall 消息`
   - 注意
     1. 同样 Linux 本地登录的收不到消息，但是可以发送广播
     2. __使用广播时，包括自己也会受到广播的消息__

2. 演示

   - 发送（Linux 本机发送）

     ```shell
     root@localcomputer:/home/ss# wall hello world    # 发送广播
     root@localcomputer:/home/ss# 
     ```

   - 接受

     ```shell
     # 接受一
     来自 ss@localcomputer (pts/0) (Fri Dec 27 21:57:38 2019) 的广播消息：
     
     hello world
     # 接受二
     来自 ss@localcomputer (pts/0) (Fri Dec 27 21:57:38 2019) 的广播消息：
     
     hello world
     ```

   - 同一用户，自己发送，自己接受

     ```shell
     ss@localcomputer:~$ wall hello shenyang		# 发送广播
     
     来自 ss@localcomputer (pts/1) (Fri Dec 27 22:01:10 2019) 的广播消息：	# 接受广播，来自第一个远程终端
     
     hello shenyang
     ```

#### ping 使用

1. 功能及命令

   - 测试网络连通性，向远程服务器发送请求包，查看远程主机是否响应
   - 命令 `ping [-选向] IP地址`
     1. `-c` 发送指定次数
   - 注意
     - Linux `ping` 命令与 Windows 不同，它会一直的 `ping` 下去，而 Windows 只会 `ping` 4 次
     - 查看 __0% packet loss__ 丢包率，有时候 `ping` 成功，但是会有丢包率，网络也是有问题的

2. 演示

   - 域名

     ```shell
     root@localcomputer:/home/ss# ping -c 2 www.baidu.com
     PING www.a.shifen.com (39.156.66.18) 56(84) bytes of data.
     64 bytes from 39.156.66.18 (39.156.66.18): icmp_seq=1 ttl=50 time=62.4 ms
     64 bytes from 39.156.66.18 (39.156.66.18): icmp_seq=2 ttl=50 time=57.7 ms
     # 传输包的统计信息
     --- www.a.shifen.com ping statistics ---
     2 packets transmitted, 2 received, 0% packet loss, time 1002ms
     rtt min/avg/max/mdev = 57.749/60.077/62.406/2.341 ms
     ```

   - IP

     ```shell
     root@localcomputer:/home/ss# ping -c 2 39.156.66.14
     PING 39.156.66.14 (39.156.66.14) 56(84) bytes of data.
     64 bytes from 39.156.66.14: icmp_seq=1 ttl=50 time=52.2 ms
     64 bytes from 39.156.66.14: icmp_seq=2 ttl=50 time=51.0 ms
     
     --- 39.156.66.14 ping statistics ---
     2 packets transmitted, 2 received, 0% packet loss, time 1001ms
     rtt min/avg/max/mdev = 51.062/51.674/52.286/0.612 ms
     ```

#### ifconfig 使用

1. 功能及命令

   - 查看和设置网卡信息（设置临时 IP 地址，有一些条件限制，桥接设置规范），源意 *interface configure*
   - 命令 `ifconfig 网卡名 IP地址`
   - 注意
     1. 使用权限 root 超级管理员
     2. __这条命令主要用来查看当前网络状态，直接使用 `ifconfig` __

2. 演示

   - 所有用户都可以使用，用来查看网络信息（其实就是 IP 地址）

     ```shell
     ss@localcomputer:~$ ifconfig
     # 网卡信息
     ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500	
     	   # IP 地址								  # 广播地址
             inet 122.168.22.78  netmask 255.255.255.0  broadcast 122.168.22.255
             inet6 fe80::6ce6:b88f:1b75:1234  prefixlen 64  scopeid 0x20<link>
             ether 00:0c:29:57:5a:31  txqueuelen 1000  (以太网)
             RX packets 91096  bytes 126328758 (126.3 MB)      # 接受包的数量、及大小
             RX errors 0  dropped 0  overruns 0  frame 0
             TX packets 64091  bytes 4965679 (4.9 MB)		 # 发送包的数量、及大小
             TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
     # 回环地址
     lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
             inet 127.0.0.1  netmask 255.0.0.0
             inet6 ::1  prefixlen 128  scopeid 0x10<host>
             loop  txqueuelen 1000  (本地环回)
             RX packets 574  bytes 35918 (35.9 KB)
             RX errors 0  dropped 0  overruns 0  frame 0
             TX packets 574  bytes 35918 (35.9 KB)
             TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
     ```


#### mail 使用

1. 功能及命令

   - 查看、发送和抄送（Cc）电子邮件
   - 发送邮件命令 `mail [用户名、邮箱地址]`
   - 查看邮件信息`mail` 进入交互界面
     1. N 表示未读
     2. R 表示已读
     3. 输入邮件序号，查看邮件内容 `1`
     4. 命令 `h` 显示邮件列表
     5. 删除邮件命令 `d [序号]` 
     6. `help` 查看帮助信息
     7. 退出
        - 退出 `q` ：邮件存入用户目录下 `/home/ss/mbox` 文件下，`/var/mail/ss` 不存在邮件，在使用 `mail` 提示没有邮件
        - 退出 `exit` ：邮件交互界面直接关闭，再次使用 `mail` 提示邮件没有阅读，邮件还是保存在 `/var/mail/ss` 文件下
   - 注意
     1. __比 `write` 和 `wall` 相对好用，可以给离线用户发送邮件__
     2. 这是邮件命令，不需要搭建邮件服务器（Windows 的邮件发送），直接使用内存发送
     3. 需要下载安装（可能我安装的是精简版）
     4. 邮件存储路径 `/var/mail/以用户名命令文件`
     5. __如使用 root 登录，接收到系统发送的邮件，注意查看，有可能是提示系统问题的，仔细阅读__

2. 演示

   - 发送邮件

     ```shell
     root@localcomputer:/home/ss/桌面# mail ss		# 向用户 ss 发送邮件
     Cc: s,root								   # 抄送给 s 和 root 用户；不抄送直接回车
     Subject: test-mail						   # 邮件标题
     hello world								   # 邮件内容
     										# ctrl + D 结束
     ```

   - 查看邮件 ，在 `/var/mail/ss` 下

     ```shell
     ss@localcomputer:~/桌面$ mail								# 进入邮件操作界面
     "/var/mail/ss": 1 message 1 new							  # 显示邮件数量及未读数量
     >N   1 root               六 12月 28 22:  14/466   test-mail # N 标识未读---时间  大小  邮件标题
     ? 1													   # 输入邮件序号
     Return-Path: <root@localcomputer>
     X-Original-To: ss@localcomputer
     Delivered-To: ss@localcomputer
     Received: by localcomputer (Postfix, from userid 0)
     	id 44D4222E32; Sat, 28 Dec 2019 22:15:38 +0800 (CST)
     To: <ss@localcomputer>								   # 接受用户
     Cc: <s@localcomputer>,<root@localcomputer>				# 抄送用户
     Subject: test-mail									  # 邮件标题
     X-Mailer: mail (GNU Mailutils 3.4)
     Message-Id: <20191228141538.44D4222E32@localcomputer>
     Date: Sat, 28 Dec 2019 22:15:38 +0800 (CST)				 # 发送时间
     From: root <root@localcomputer>							# 邮件来自---
     
     hello world											  # 邮件内容
     ? h
     >R   1 root               六 12月 28 22:  14/466   test-mail # R 标识已读
     ? q													# 退出邮件交互界面
     Saved 1 message in /home/ss/mbox					 # 将邮件存入用户目录下 mbox 文件下
     Held 0 messages in /var/mail/ss						 # 清空邮件
     ss@localcomputer:~/桌面$ cd /home/ss
     ```

   - 查看历史邮件，在 `/home/ss/mbox` ，使用 `cat [文件名]`

     ```shell
     ss@localcomputer:~$ cat /home/ss/mbox 
     From root@localcomputer  Sat Dec 28 22:15:38 2019
     Return-Path: <root@localcomputer>
     ```

#### last 使用

1. 功能及命令

   - 日志查询命令

   - 列出目前与过去登录系统的用户信息和重启时间
   - 命令 `last` 显示所有用户登录过的信息
   - 命令 `last [用户]` 显示特定用户登录过的信息
   - 注意
     1. 显示用户、本地\远程、登录来自、登录时间、登录持续时间

2. 演示

   - `last` 演示

     ```shell
     ss@localcomputer:~$ last
     ss       tty2         tty2             Sun Dec 29 18:55   still logged in # 正在登录，本地
     reboot   system boot  4.15.0-29-generi Sun Dec 29 18:54   still running	  # 重启
     ss       tty2         tty2             Sun Dec 29 18:49 - 18:52  (00:02)
     reboot   system boot  4.15.0-29-generi Sun Dec 29 18:47 - 18:54  (00:06)
     ss       tty2         tty2             Sun Dec 29 18:43 - 18:46  (00:02)
     reboot   system boot  4.15.0-29-generi Sun Dec 29 18:42 - 18:47  (00:04)
     ss       pts/1        192.168.43.17    Sun Dec 29 18:40 - 18:40  (00:00)# 远程登录，持续时间位 0
     s        pts/1        192.168.43.17    Sun Dec 29 18:39 - 18:39  (00:00)
     ss       :0           :0               Sun Dec 29 18:32 - down   (00:08)# 本地登录
     reboot   system boot  4.15.0-29-generi Sun Dec 29 18:32 - 18:41  (00:08)
     wtmp begins Wed Dec  4 15:04:47 2019		# 登录日志文件创建时间信息来源 /var/log/wtmp
     ```

   - `last user` 演示

     ```shell
     ss@localcomputer:~$ last s
     s        pts/1        192.168.43.17    Sun Dec 29 18:39 - 18:39  (00:00)
     s        pts/2        192.168.43.17    Fri Dec 27 21:42 - 22:06  (00:24)
     s        pts/2        192.168.43.17    Fri Dec 27 21:25 - 21:37  (00:11)
     wtmp begins Wed Dec  4 15:04:47 2019
     ```

     

#### lastlog

1. 功能及命令

   - 列出用户最后登录时间
   - 命令 `lastlog`
   - 注意
     1. __只显示远程用户最后登录时间__
     2. __会列出所有用户，包括从未登录过的伪用户__
   - 选项 `-u` 命令 `lastlog -u [u_ID]` 显示特定用户最后登录时间

2. 演示

   - `lastlog` 只显示远程登录的用户

     ```shell
     用户名           端口     来自             最后登陆时间
     root                                       **从未登录过**
     daemon                                     **从未登录过**
     ...
     gnome-initial-setup                           **从未登录过**
     gdm                                        **从未登录过**
     ss               pts/1    192.168.43.17    日 12月 29 18:40:21 +0800 2019
     sshd                                       **从未登录过**
     s                pts/1    192.168.43.17    日 12月 29 18:39:39 +0800 2019
     postfix                                    **从未登录过**
     ```

#### traceroute 使用

__说明：网络选择桥接__

1. 功能及命令

   - 显示数据包到主机间的路径
   - 需要自定义安装
   - 命令 `traceroute [主机]`
   - 实例
     1. `traceroute www.baidu.com`
     2. `traceroute 39.156.66.18`

2. 注意

   - 每次链接的路径，可能会有些不同（大部分是相同的）

   - 每一次跳转，就是经过一个网关
   - 默认向路径的每一个设备发送 3 次数据包，用于测试响应时间，使用 `-q` 设置
   - __有的使用 * 标识，表示防火墙封闭掉了返回信息__
   - __实际感觉，一行表示路径的一个节点，节点有可能不同，但是会使用时间最短的最为下一次数据包的发出点__
   - __下一行信息（IP 表示设备），表示从当前行时间响应最短的节点发出数据包的接受设备（接受节点有可能不同，一共发送 3 次）__

3. 实际作用

   - 可以测试网络节点中那个节点出现问题

4. 演示

   - `traceroute www.baidu.com.cn`

     ```shell
     root@localcomputer:/home/ss# traceroute www.baidu.com.cn
     # 域名解析的 IP    # 最多显示的节点设备数量    # 发送大小位 60 字节的数据包
     traceroute to www.baidu.com.cn (39.156.66.18), 30 hops max, 60 byte packets
     # 本地网络最后一个节点        此节点测试 3 次
      1  _gateway (192.168.43.1)  2.441 ms  3.170 ms  9.880 ms
      2  * * *      # 防火请屏蔽信息
      3  192.168.53.17 (192.168.53.17)  42.872 ms  43.073 ms  43.038 ms
      4  * * *
      5  10.64.72.209 (10.64.72.209)  42.732 ms  42.667 ms  42.606 ms
      6  211.137.47.41 (211.137.47.41)  42.533 ms  33.180 ms  33.078 ms
      7  221.183.39.225 (221.183.39.225)  42.834 ms * *
      8  221.183.37.145 (221.183.37.145)  47.779 ms  47.746 ms  47.604 ms
      9  * * *
     10  * 39.156.27.5 (39.156.27.5)  56.721 ms *
     # 上一行响应时间最短的节点设备，发出数据包，共发送 3 次，发送到了 3 个设备上，使用响应时间最短的最为下一个路径设备
     11  39.156.67.21 (39.156.67.21)  70.173 ms 39.156.27.5 (39.156.27.5)  79.121 ms 39.156.67.85 (39.156.67.85)  51.620 ms
     12  39.156.67.21 (39.156.67.21)  63.160 ms * *
     13  * * *
     14  * * *
     ...
     29  * * *
     30  * * *				# 最多显示 30 个节点设备
     ```

   - `traceroute www.sina.com.cn`

     ```shell
     root@localcomputer:/home/ss# traceroute www.sina.com.cn
     traceroute to www.sina.com.cn (221.180.220.34), 30 hops max, 60 byte packets
      1  _gateway (192.168.43.1)  1.629 ms  1.381 ms  1.824 ms
      2  * * *
      3  192.168.53.21 (192.168.53.21)  55.500 ms  55.456 ms  55.400 ms
      4  * * *
      5  10.64.72.213 (10.64.72.213)  40.467 ms  40.380 ms  40.300 ms
      6  211.137.47.169 (211.137.47.169)  40.225 ms  37.242 ms  37.175 ms
      7  221.180.232.6 (221.180.232.6)  36.994 ms  32.166 ms  43.590 ms
      8  * * *
      9  * * *
     10  221.180.220.34 (221.180.220.34)  34.585 ms  34.555 ms  44.920 ms
     root@localcomputer:/home/ss#   # 此路径共有 10 个节点设备
     ```

#### netstat 使用

1. 功能及命令

   - 显示网络相关信息
   - 命令 `netstat [选项]`

2. 选项信息

   - 表格

     | 选项 | 意义             |
     | ---- | ---------------- |
     | -t   | TCP 协议         |
     | -u   | UDP 协议         |
     | -l   | 监听             |
     | -r   | 路由             |
     | -n   | 显示 IP 和端口号 |

3. 范例（掌握 3 个基本够用）

   - `netstat -tlun`    查看本地监听的端口（服务）
   - `netstat -an`    查看本机所有网络链接（包括程序、服务、已连接服务）
   - `netstat -rn`    查看本机路由表

4. 演示

   - `netstat -tlun` 查看开启的服务有哪些（一般情况下，端接口号对应哪个服务是确定的）

     ```shell
     root@localcomputer:/home/ss# netstat -tlun
     激活Internet连接 (仅服务器)
     # 协议  # 发送状态、接受状态 0 正常  
                         # IP地址及端口号       # 远程连接地址及短空号        # TCP 需要监听 UDP 不需要
                         										# established 表示已建立连接 
     Proto Recv-Q Send-Q Local Address           Foreign Address         State      
     tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN     
     tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
     tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN     
     tcp        0      0 0.0.0.0:25              0.0.0.0:*               LISTEN     
     tcp6       0      0 :::22                   :::*                    LISTEN     
     tcp6       0      0 ::1:631                 :::*                    LISTEN     
     tcp6       0      0 :::25                   :::*                    LISTEN     
     udp        0      0 0.0.0.0:39828           0.0.0.0:*                          
     udp    30720      0 127.0.0.53:53           0.0.0.0:*                          
     udp     8704      0 0.0.0.0:68              0.0.0.0:*                          
     udp    30720      0 0.0.0.0:5353            0.0.0.0:*                          
     udp        0      0 0.0.0.0:631             0.0.0.0:*                          
     udp6    2432      0 :::5353                 :::*                               
     udp6       0      0 :::52688                :::*                               
     root@localcomputer:/home/ss# 
     ```

   - `netstat -an` 查看很多信息，需注意 __连接状态 established 表示已连接__ ，和 `netstat -tlun` 最大区别

     ```shell
     root@localcomputer:/home/ss# netstat -an
     激活Internet连接 (服务器和已建立连接的)
     Proto Recv-Q Send-Q Local Address           Foreign Address         State      
     tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN     
     tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
     tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN     
     tcp        0      0 0.0.0.0:25              0.0.0.0:*               LISTEN     
     tcp        0      0 127.0.0.1:6010          0.0.0.0:*               LISTEN     
     tcp        0      0 192.168.43.87:22        192.168.43.17:56697     ESTABLISHED # 已经连连接
     tcp        0      0 192.168.43.87:22        192.168.43.17:56696     ESTABLISHED
     tcp6       0      0 :::22                   :::*                    LISTEN     
     tcp6       0      0 ::1:631                 :::*                    LISTEN     
     tcp6       0      0 :::25                   :::*                    LISTEN     
     tcp6       0      0 ::1:6010                :::*                    LISTEN     
     udp        0      0 0.0.0.0:39828           0.0.0.0:*                          
     udp    33792      0 127.0.0.53:53           0.0.0.0:*                          
     udp    10880      0 0.0.0.0:68              0.0.0.0:*                          
     udp    33792      0 0.0.0.0:5353            0.0.0.0:*                          
     udp        0      0 0.0.0.0:631             0.0.0.0:*                          
     udp6    5632      0 :::5353                 :::*                               
     udp6       0      0 :::52688                :::*                               
     raw6       0      0 :::58                   :::*                    7      
     ...
     ...
     ```

#### setup 使用

__说明：redhat 专有命令，其他版本没有__

1. 功能及命令
   - 配置网络

#### mount 挂在命令 \ umount

1. 功能及命令
   - 将硬件设备挂载到空目录上
   - 命令 `mount [-t 文件系统] 设备文件名 挂载点` ，-`-t 文件系统` 可以省略，系统默认选择
   - __命令 `mount -l` 显示所有以挂载文件设备列表，自定义挂在设备在底部显示__
   - 命令 `umount 设备文件名` ，解除挂在
   - __挂载记录在 `/etc/mtab` 文件中，解除挂载记录删除__
   
2. 注意
   - __Ubuntu 自动挂载设备到 `/media/ss` 目录下（光盘、u盘）__
   - USB-U盘 ，设备文件名 `/dev/sdb1`
   - 光盘，设备文件名 `/dev/rs0` `/dev/rs1` ...
   - __解除挂载，需要退出挂载目录__
   
3. __Ubuntu 的 `mount` 的使用__

   - 插入光盘、U盘时，会自动进行挂载，即挂载点为 `/media/ss/光盘名称或盘名称` ，`/ss` 以用户名命名的目录
   - 如果，想要将其转移到其他挂载点（复制一下挂载点，原来的挂载点还可以继续使用），
     1. 使用 `mount -l` 查看所有以挂载的文件设备列表（自定义挂载设备，在底部显示）
     
     2. 使用 `mount --bind [设备挂载点] [复制到的挂载点]`
     
        ```shell
        mount --bind /media/shen/SS沈 /home # SS沈 是 U 盘名，/home 必须是空目录，命令执行之后，U 盘内容会在 /home 目录中
        ###############################################################
        mount -- bind /media/shen /home # SS沈，U 盘会在 /home 中
        ```
     
     3. 解除挂载点 `umount [挂载设备\挂载路径]` 但是只能解除一个挂载路径（`mount -l` 列表底部的最先解除）
   - 如果，想转移挂载点
     1. Ubnutu 默认挂载到 `/media/ss`
     2. 使用 `mount -l` 查看挂载设备，和挂载点，如果有一个设备有多个挂载点（使用 `mount --bind ` 命令实现），必须全部解除，才可以重新选用新的挂载点
     3. 解除挂载点 `umount [挂载设备\挂载路径]`
     4. 重新挂载 `mount [挂载设备] [挂载路径]`

4. 演示

   - 插入 U盘自动挂载

     ```shell
     root@localcomputer:/media# ls ss/SS沈/                # 显示 U盘内容 
      ajcb.rtf   nigao   nihoa.txt  'System Volume Information'
      
      # 使用 mount -l
     root@localcomputer:/media# mount -l  # 显示挂载设备、挂载点信息
     /dev/sdb1 on /media/ss/SS沈 type fuseblk (rw,nosuid,nodev,relatime,user_id=0,group_id=0,default_permissions,allow_other,blksize=4096,uhelper=udisks2) [SS沈] 
     ```

## Linux 关机重启命令

#### shutdown 使用（root  使用）

1. 功能及命令
   - 关机重启命令
   - 命令 `shutdown [选项] 时间`
   - 选项
     1. `-c` 取消前一个关机命令
     2. `-h` 关机，后面可以加时间，关闭系统并切断电源，__看一下操作手册__，客户机一般是关机并切断电源
     3. `-r` 重启，后面可以加时间
2. 注意
   1. 早期，关机命令只有 `shutdown` 关机重启会正确保存正在启动的服务，再开启保证数据不丢失
   2. 现在，其他关机命令也可以保存数据了
   3. 关机、重启时，先尽量停止服务，再使用 `shutdown` 关机、重启
3. 其他关机命令
   - `halt`     `poweroff`     `init 0` 
4. 其他重启命令
   - `reboot`     `init 6` 
5. 演示
   - 关机
     1. 马上关机 `shutdown -h now` 
     2. 具体时间关机 (晚上 8 点 30 关机)`shutdown -h 20:30`
     3. 间隔多长时间 `shutdown -h +30` 间隔 30 分钟
   - 重启
     1. 马上重启 `shutdown -r now`
     2. 指定时间重启 `shutdown -r 11:20`
     3. 间隔 30 分钟重启 `shutdown -r +30`
   - 取消上一个关机命令
     1. `shutdown -c`

### 系统运行级别

__说明：上述所说 `init 0` 关机命令、`init 6`  重启命令中的数字代表什么意思__  

1. 系统运行级别

   - 如表

     | num   | 意义                        |
     | ----- | --------------------------- |
     | __0__ | 关机                        |
     | __1__ | 单用户                      |
     | __2__ | 不完全多用户，不含 NFS 服务 |
     | __3__ | 完全多用户                  |
     | __4__ | 未分配                      |
     | __5__ | 图形界面                    |
     | __6__ | 重启                        |

2. 介绍运行级别

   - 代表启动 Linux 启动时，可进入的级别号

   - `init 0` 调用 0 运行级别，表示关机

   - `init 6` 调用 6 运行级别。表示重启

   - 单用户

     1. __启动最小的服务，其他附加的服务不启动（甚至连网络都不要），用来修复使用__
     2. 可根据 Windows 的安全模式来理解 Linux 的单用户模式
     3. 是以 root 超级管理员用户登录，其他用户不可登录，禁止远程登录
     4. 没有图形界面

   - 完全多用户、不完全多用户

     1. 之间相差是否启动 NFS 服务

     2. __登录后进入控制台命令行模式__

   - 图像界面

     1. __登陆后进入图形界面__

   - NFS 服务
     - NFS 是 Linux 之间共享文件的一个服务，但是有严重的安全隐患

3. 默认启动级别

   1. __默认启动级别不可以设置为 0 和 6。启动时，不可能重启和关机__

4. 查看 init 配置文件

   - `cat /etc/inittab` 查看 init 的配置文件，可以看到默认的启动级别
   - __Ubuntu 没有这个玩儿应，centos7 也没有这个玩儿应（文件）__

5. __查看 Linux 当前运行级别__

   - 命令 `runlevel`

   - 演示

     ```shell
     ss@localcomputer:~$ runlevel
     N 5								# 默认启动运行级别为 5 ,图形界面。N 表示上一个启动级别
     ss@localcomputer:~$ man runlevel
     ```

   - 切换运行级别 `init 3` 命令行模式，终端执行 `init 3` ，然后选择 __纯文本串行终端 __ `ctrl+alt+f1[1……6]`__（tty [tty1……6] 为 Ubuntu 的纯文本串行终端，通常用作访问计算机以修复内容的方式登录）__

     ![Linux命令行模式](git_picture/Linux命令行模式.png)

   - __切回图形界面必须使用 root 权限，再次查看运行级别__

     ```shell
     ss@localcomputer:~$ runlevel
     3 5							# 上一个运行级别是 3（命令行模式），当前级别 5 图形界面
     ss@localcomputer:~$ 
     ```

#### logout 退出登录

1. 介绍

   - 就是退出登录，没有别的

2. 注意

   - 图形界面下的终端，不能使用 `logout` 

     ```shell
     ss@localcomputer:~$ logout
     bash: logout: 不是登录 shell: 使用 `exit'
     ```

   - 可以使用 `exit` 退出终端，但只是退出终端

3. __在完全命令行模式下，就是 `init 3` 运行级别下，使用 `logout` 退出登录，或者也可以使用 `exit` 退出登录__

## 软件包管理

### 软件包管理简介

#### 软件包分类 2 类

1. 源码包（大部分基于 c 语言开发的、开源）
   - 特点
     1. 安装慢
     2. 但可查看源代码（的是大神级别的人物）
     3. __没有依赖性__
   - 脚本安装包（源码包的 plus 版）
     1. 源码包的二次开发，不需要手动安装，类似于 Windows 安装一样
     2. 有安装界面
     3. 脚本安装包，很少见到
2. 二进制包（称作：RPM包--Redhat / DEB包--Ubuntu 或 系统默认包）
   - 特点
     1. 安装快
     2. 经过源码编译的二进制文件
     3. 不可查看源代码
     4. __存在依赖性__
   - 两种管理方式
     1. rpm 命令管理（dpkg 命令管理）
     2. yum 在线管理（apt/apt-get 在线管理）

#### 源码包

1. 优点
   - 开源：如果有足够的能力，可以修改源代码
   - 可以自由选择所需的功能
   - 软件是编译安装，所以更加适合自己的系统，更加稳定也效率更高
   - 卸载方便
     1. 实际没有卸载命令，__直接删除软件目录，没有任何垃圾文件遗留__
2. 缺点
   - 安装过程步骤较多，尤其是安装较大的软件集合时（如 LAMP 环境搭建），容易出现拼写错误
   - 编译过程时间较长（编译非常浪费时间），安装比二进制文件安装时间长
   - 因为是编译安装，安装过程中一旦报错新手很难解决
   - __可以作为练习安装源码包，对 Linux 会有一个大的提升__

#### RPM 包、DEB 包

1. 二进制的优点
   - 包管理系统简单，只通过几个命令就可以实现包的安装、升级、查询和卸载
   - 安装速度比源码包安装快
2. 二进制包缺点
   - 经过编译，不可再看到源码
   - 功能选择不如源码包灵活
   - 依赖性（a 包的安装需要 b 包）

### RPM包管理--rpm命令管理

#### RPM 包的命名规则

1. 介绍 `httpd-2.2.15-15.el6.centos.l.i686.rpm`
   - httpd：软件包名
   - 2.2.15：软件版本
   - 15：软件发布次数
   - el6.centos：适合的 Linux 平台（el6 版本、Centos 版本）
   - i686：适合的硬件平台（这块不用管，主要注意是否区分 32、64 位，`x64` 表示64位机可以使用）
   - rmp：rpm包扩展名
   - __如果哪个说明没有写，则适合所有版本__
2. 包名、包全名
   - 包名：则是软件名，即 `httpd`
   - 包全名：则是上面 1 介绍
   - __有些命令严格区分包名、包全名__

#### RPM 包依赖性

1. 树形依赖 a --> b --> c
   - a 包，依赖 b 包；b 包，依赖 c 包
   - 安装 a 包，则需要先安装 c 包，在安装 b 包，最后安装 a 包
   - 卸载 a 包，则需要先卸载 a 包，在卸载 b 包，最后卸载 c 包
   - __依次安装即可以解决__
2. 环形依赖
   - a --> b --> c --> a
   - __a 包、b 包、c 包，同时安装，即可解决问题__
3. 模块依赖（库依赖）
   - [模块依赖查询网站](www.rpmfind.net) ，即搜索这个库属于哪个包下
   - 模块依赖是包的安装依赖于一个包（软件）下的一个库
   - 模块依赖的难点在于，要确定模块是属于哪个包的，将整个包安装即可
   - __`.so.0/1/2/3...`  结尾的应该就是模块依赖__

#### RPM命令管理-安装升级与卸载

1. 什么时候使用包名和包全名
   - __使用包全名__：操作的包是没有安装的软件包时，使用包全名，而且要注意路径（可让 Linux 可以找到）
     1. 安装、升级操作使用包全名
   - __使用包名__：操作已安装的软件包时，使用包名，实际是搜索 `/var/lib/rpm/` 中的数据库
     1. 查询、卸载是操作已经安装过的包，实际上是在 `/var/lib/rmp/` 下搜索的，任何路径都可以
2. RPM 安装
   - 命令 `rpm -ivh 包全名` 、`dpkg -i package`
   - 选项
     1. `-i`：install 安装
     2. `-v`：verbose 显示详细信息
     3. `-h`：hash 显示进度
     4. `--nodeps`： 不检测依赖性（不检测，就是不安装其底层包，会导致软件出错）
   - 如提示依赖其他包，正常安装其他包，
   - 如提示依赖库文件 `.so.0/1/2..` ，上网站查看库文件属于哪个包下，安装这个包即可
3. RPM 升级
   - 命令 `rpm -Uvh 包全名` ，`U` 是 upgrade 的缩写
   - 可以使用 __升级代替安装__  也是可以的
4. RPM 卸载
   - 命令 `rpm -e 包名`
     1. 卸载只使用包名，是在数据库中搜索
   - 选项
     1. `-e` ：erase 卸载
     2. `--nodeps` ：不检查依赖（就是不卸载此包的依赖包）
   - 卸载需要注意安装顺序（依赖性）
     1. 被依赖的包无法正常卸载

#### RPM 查询

1. 查询是否安装
   - 查询某一个 RPM 包 
     1. `rpm -q 包名` 之所以使用包名，是因为已经安装过了
     2. `dpkg -l 包名`、`dpkg-query -l package` Ubuntu 命令
   - 查询所有已安装的 RPM 包 
     1. `rpm -qa` a 为 all 缩写
     2. `dpkg -l` 、 `dpkg-query -l` Ubuntu 命令
   - 使用管道符，可以查询和这个包相关的所有包，而不是只显示主包，或显示全部的 RPM 包
     1. `rpm -qa | grep 包名`
2. 查询软件包详细信息
   - 命令 `rpm -qi 包名` 、`dpkg -s package` (Ubuntu 查询安装的包的详细信息)
   - 选项
     1. `-i` ：查询软件信息（information）
     2. `-p` ：查询未安装包信息（package）
        - `rpm -qip 包全名` 查询未安装包信息 
3. 查询包中文件安装位置
   - 命令 `rpm -ql 包名` 、`dpkg -L package` (Ubuntu 包的安装位置)
   - 选项
     1. `-l` list 列表
     2. `-p` 查询未安装信息 （package）
        - `rpm -qlp 包全名`
   - __包的安装位置和包文件位置是固定的，在生产包的时候就已经确定了，所以没有安装也可以查询包的安装位置__
   - __也可以指定安装位置，但会出现一系列问题__
4. 查询文件系统属于哪个 RPM 包
   - 命令 `rpm -qf 系统文件名` 、 `dpkg -S filename` (Ubuntu 查询文件属于哪个包的)
   - 选项
     1. `-f` ：查询系统文件属于哪个软件（file）
5. 查询软件包依赖性
   - 命令 `rpm -qR 包名` 、`dpkg --info 未安装包` （查询未安装包的依赖）
   - 选项
     1. `-R`：查询软件包的依赖性（requires）
     2. `-p`：查询未安装包信息（package）
        - `rpm -qRp 包全名`

#### RPM 包校验

1. 命令 `rpm -V 已安装的包名`

2. 选项

   - `-V`：校验指定 RPM 包的文件（verify）

3. __校验介绍__

   - 包安装完成之后保存包文件特征，与当前文件特征进行比较，判断其当前文件是否被修改

4. 使用

   - 如没有任何信息，则没有任何修改

   - 如显示提示信息，则表示被修改

   - 校验内容中 8 个信息具体内容如下（没有发生改变，使用 . 代替）

     | 字符 | 意义                                                |
     | ---- | --------------------------------------------------- |
     | S    | 文件大小是否改变                                    |
     | M    | 文件类型或文件权限（rwx）是否被修改                 |
     | 5    | 文件 MD5 校验和是否改变（可以看成文件内容是否改变） |
     | D    | 设备中，从代码是否被改变                            |
     | L    | 文件路径是否被改变                                  |
     | U    | 文件属性（所有者）是否改变                          |
     | G    | 文件属组是否改变                                    |
     | T    | 文件修改时间是否改变                                |

   - 包中文件类型

     | 字符 | 文件类型                                                     |
     | ---- | ------------------------------------------------------------ |
     | c    | 配置文件（config file）                                      |
     | d    | 普通文件（documentation）                                    |
     | g    | “鬼” 文件（ghost file）很少见，就是该文件不应被这个 RPM 包包含 |
     | l    | 授权文件（license file）                                     |
     | r    | 描述文件（read me）                                          |

RPM 包文件提取

1. 包文件提取介绍

   - 如包的某一个文件 __坏掉__，不需要重新安装整个包，只需要重新下载覆、盖掉出错的文件即可修复包

2. 命令  `rpm2cpio 包全名 | \ cpio -div . 文件绝对路径`

   __说明：从包中提取出这个路径下文件，存放在当前目录，然后再将此文件移动之前的位置__

   - `\` ：表示命令一行写不完回车换一行；`.` ：表示当前路径

   - `rpmcpio` ： 将 rpm 包转换未 cpio 格式的命令
   - `cpio` ：是一个标准工具，它用于创建软件档案文件和从档案文件中提取文件
     1. `cpio 选项 < [文件|设备]`
        - `-i`：copy-in 模式，还原
        - `-d` ：还原时自动新建目录
        - `-v` ：显示还原过程
        - `<` ：输入重定向

### RPM包管理--yum在线管理

__说明：在线可以，不在线也可以，主要是自动解决依赖性__

#### IP 地址配置和网络 yum 源

1. IP 地址配置

   - IP
   - 子网掩码
   - 网关
   - DNS

2. yum 源介绍

   - 在 `/etc/yum.repos.d/CentOS-Base.repo`

   - `.repo` 文件都是合法的 yum 源，共有 4 个 yum 源，[base、debuginfo、media、vault ].repo。文件格式说明。

     | 字符       | 意义                                                         |
     | ---------- | ------------------------------------------------------------ |
     | [base]     | 容器名称，一定要放在 []，软件池（类似于仓库）                |
     | name       | 容器说明，可以随便写（注释）                                 |
     | mirrorlist | 镜像站点，这个可以注释掉                                     |
     | baseurl    | 我们 yum 源服务器地址，默认是 CentOS 官方的 yum 源服务器，是可以使用的，如果慢可以换成自己喜欢的 yum 源地址 |
     | enable     | 此容器是否生效，如果不写或 `enable = 1` 都生效，写成 `enable = 0` 就不生效 |
     | gpgcheck   | 如果是 1 是指 RPM 的数字证书生效，如果是 1 则不生效          |
     | gpgkey     | 数字证书公钥保存位置，不用修改                               |

#### yum 命令

__说明：Ubuntu 的 `apt/apt-get` 的用法 [参考地址](https://www.sysgeek.cn/apt-vs-apt-get/)__

1. 查询
   - 命令 `yum list`
   - 功能：查询所有可用软件包列表（上远程服务器查询）
   - 命令 `yum search 关键字或包名`
   - 功能：搜索服务器上所有和关键字相关的包
   
2. 安装

   - 命令 `yum -y install 包名`

   - 功能：在线安装包，选项 `-y` 为自动确认（确认是否安装依赖包）

   - 选项

     1. `install` ：安装

     2. `-y` ：自动回答

   - 实例

     1. 安装 gcc ，为 c 的编译器，用来源码包安装编译的
     2. Linux 最小化安装，默认没有 gcc 编译器
     3. 命令 `yum -y install gcc` 

3. 升级

   - 命令 `yum -y update 包名`
   - 功能：在线升级包
   - 选型
     1. `update` ：升级
     2. `-y` ：自动回答
   - 注意
     1. 命令 `yum -y update` 将会升级所有软件包，包括 Linux 内核

4. 卸载

   - 命令 `yum -y remove 包名`
   - 功能：卸载已安装的软件包，以及这个包的依赖也会一起卸载

####  yum 软件组的管理命令

__说明：软件组安装，实际就是安装系统时，定制系统的安装功能__

1. 查询
   - 命令 `yum grouplist`
   - 功能：列出所有可用的软件组列表
2. 安装
   - 命令 `yum groupinstall 软件组名`
   - 功能：安装指定软件组，组名可由 `yum grouplist` 查出
3. 卸载
   - 命令 `yum groupremove 软件组名` ，如果软件组名中间有空格，则是由 "" 包起来即可
   - 功能：卸载指定软件组

#### 光盘 yum 源搭建

__说明：光盘挂载（Ubuntu 自动挂载）__

1. 挂载光盘

   - 光盘例存放 yum 源
   - 不一定是最新的

2. 让网络 yum 源配置文件失效

   __说明：Linux 是靠文件后缀名，区分的 yum 源配置（虽然 Linux 没有后缀名之说）__

   - yum 源配置文件保存位置 `/etc/yum.repos.d/`
   - 让 `Media.repos` yum 源配置文件生效，将其他 yum 源配置文件，后缀名改掉，让其失效
   - 修改 `Media.repos` 添加 `enable = 1` ，修改 `baseurl=file:///目录`，注释掉其他两个路径。__Linux 配置文件有严格格式要求，行尾不能加空格、注释不能缩进等...__
     1. `file://` 是文件传输协议、`/目录` 是 yum 源（光盘目录）

### 源码包管理

#### 源码包与 RPM（DEB）包的区别

1. 安装之前的区别：概念上的区别

2. 安装之后的区别：安装位置不同

   - RPM 包安装的位置默认，也可以自己指定，但后续会出现一些列问题（大部分 RPM 包）

     | 默认安装位置    | 文件作用                 |
     | --------------- | ------------------------ |
     | /etc/           | 配置文件安装目录         |
     | /usr/bin/       | 可执行命令安装目录       |
     | /usr/lib/       | 程序所使用函数库保存位置 |
     | /usr/share/doc/ | 基本软件使用手册保存位置 |
     | /usr/share/man/ | 帮助文档保存位置         |

   - 源码包指定安装位置，一般指定位置 `usr/local/软件名/`

3. 安装位置不同带来的影响

   - RPM 包管理的服务可以使用系统服务管理命令（service）来管理，例如 RPM 包安装的 apache 的启动方法是
     1. `/etc/rc.d/init.d/httpd start`  RPM 包服务，绝大部分都是绝对路径 + 命令 + start 启动服务
     2. `service httpd start` service 服务管理命令会到 __系统默认路径__ 去搜索相关服务
     3. service 服务管理命令，就不能管理源码包，因为安装路径不同
   - 源码包安装的服务则不能被服务管理命令管理，因为没有安装到默认路径中，__所以只能使用绝对路径进行服务的管理__，如
     1. `/usr/local/apache2/bin/apachectl start` 绝对路径管理

#### 源码包安装过程

1. 准备

   - 安装 gcc （c 语言编辑器）
   - 下载源码包（c 语言）

2. 安装注意事项

   - 源码保存位置：`/usr/local/src`，下载后的软件源码位置
   - 源码安装位置：`usr/local/包名`
   - 如何确定安装过程出现错误
     1. 安装过程停止
     2. 并出现 `error` 、 `warning` 或 `no` 的提示

3. 源码包安装过程

   - 下载源码包（上传的 Linux 中）

   - 解压缩下载的源码包

   - 进入解压缩目录
     1. INSTALL：安装说明
     2. README：使用说明

4. 源码包安装命令

   __说明：以下操作需要进入解压目录__

   - `./configure` 软件配置与检查（编译前准备）
     1. 定义需要的功能选项（`./configure --help` 查看帮助文档）
        - 安装路径： `./configure --prefix=/usr/local/软件包` 将软件安装在 `/usr/loacl/软件包`
        - 执行 `./configure --prefix=/usr/local/软件安装目录` ，并在解压目录产生 Makefile 文件
        - 以后 `make` 、 `make install` 都会依赖此文件
     2. 检测系统环境是否符合安装要求
     3. 把定义好的功能选项和检测系统环境的信息都写入 `Makefile`（命令执行完成自动产生） 文件，用于后续编译
   - make
     1. 将 c 语言编译成机器语言（占用大量时间）
     2. __以上两步操作，并没有向安装路径安装任何东西，出错使用 `make clear` 清空临时文件；`make distclear` 还会清除 Makefile 文件__
   - make install
     1. 向安装软件路径安装软件

5. 源码包卸载

   - 直接删除目录：`rm -rf /usr/loacl/bin` 
   - __卸载不会产生其他垃圾文件，因为软件所有的的文件全部在安装目录中，一般默认使 `/usr/loacl/软件包名(自定义)`__

### 脚本安装与软件包选择

1. 介绍脚本安装
   - 脚本安装并不是独立的软件包类型，常见的安装的是源码包
   - 是将安装过程写成了自动安装的脚本，只要执行脚本，定义简单参数，就可以完成安装
   - 非常类似于 Windows 下软件安装方式
   - 常见的脚本安装为 __硬件驱动程序__
2. 脚本安装实例
   - Webmin 
     1. Linux 系统管理界面，可以通过图像化方式设置用户账号、Apache、DNS、文件共享等服务
   - 安装过程
     1. 解压缩，进入压缩目录
     2. 执行脚本文件 `./setup.sh` `./`  表示当前路径下的命令
     3. 脚本一般是以 `.sh` 结尾 ; `setup.sh` 为执行脚本

### 短暂总结

#### 非源码包安装

1. 用户不能决定软件安装在哪个路径里，只能使用 Linux 的发行版本的 __包管理器__ 进行安装，软件在编译时就决定了安装到哪里
2. Debian、Redhat 两个 Linux 发性版本的包管理器是不同的
   - Debian 包管理器 `dpkg` 为安装 `.DEB`  的安装包，__待补----没有看过__
   - Redhat 包管理器 `rpm`  为安装 `.RPM` 的安装包，不能解决包安装的依赖问题，为解决依赖产生了 `yum` 命令
3. 无论哪个包管理器都有为解决包的 __依赖问题__ ，而发展出自动解决包安装依赖问题，在线安装 `apt/apt-get` 命令或 `yum` 命令（同样用户不能决定包文件安装在哪里）

#### 源码包安装

1. 源码包安装容易出错、相对复杂（没觉得有多复杂），__这个可以自己决定安装在哪里__，一般安装在 `/usr/local/软件包名`
2. 安装前，得到源码包，解压缩查看 INSTALL 安装说明文档、README 使用说明文档
3. __现在有的软件不提供源码包（如 VSCode），所以不能源码包安装，只能使用包管理器安装__

#### 小结

1. 使用包管理器和不使用包管理器，安装的路径会有不同。之所以使用包管理器（对应的 Linux 发性版本有默认的服务管理）最方便是一些命令安装之后可以在任意路径下使用，源码包安装路径改变了默认安装路径，所以包管理器无法提供服务，使用时只能在当前目录下 `./命令`  ，或者是添加环境变量
2. 源码包安装后，运行速度会比包管理器安装的软件运行高效

## 用户和用户组管理命令

### 用户配置文件

#### 用户信息文件 `/etc/passwd`

1. 用户管理介绍

   - 越是对安全性能越高的服务器，越是需要建立合理的用户等级制度和服务操作规范

   - 在 Linux 中主要是通过用户配置文件来查看和修改用户信息

   - Linux 的  `man` 命令对配置文件的查看 `man 5 配置文件`

     1. `5`  表示配置文件

        ```tex
        名称
               passwd - 密码文件
        
        描述
               /etc/passwd 为每个用户账户包含一行，包含使用冒号 (“:”) 分隔的七个字
               段，分别是：
        
               ·   登录名
        
               ·   可选的加密后的密码
        
               ·   数字用户 ID
        
               ·   数字组 ID
        
               ·   用户名和注释字段
        
               ·   用户主目录
        
               ·   可选的用户命令解释器
        
               加密的密码字段可以为空，此时使用指定的登录名登录时不会要求认证。然
               而，如果 password 为空，一些读取 /etc/passwd 文件的程序可能会不允许 任
               何 访问。如果 password 字段是一个小写的 “x”，那么加密的密码实际上存储于
               shadow(5) 中；在 /etc/shadow 文件中 必须 有对应的行，否则用户账户就会无
               效。如果 password 字段是其他任何字符串，将会被视为加密过的密码，如
               crypt(3) 中的说明。
        ```

2. 文件说明

   - 样式

     ```tex
     root:x:0:0:root:/root:/bin/bash    // 超级用户 root
     daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
     bin:x:2:2:bin:/bin:/usr/sbin/nologin
     sys:x:3:3:sys:/dev:/usr/sbin/nologin
     sync:x:4:65534:sync:/bin:/bin/sync
     ss:x:1000:1000:ss,,,:/home/ss:/bin/bash    // 普通用户 ss
     ```

   - 使用 “:” 划分 7 个字段

     | 字段      | 意义                                                         |
     | --------- | ------------------------------------------------------------ |
     | 第 1 字段 | 用户名称                                                     |
     | 第 2 字段 | 密码标志                                                     |
     | 第 3 字段 | UID（用户 ID）<br> 0: 超级用户 <br> 1-499: 系统用户（伪用户）<br> 500-65535: 普通用户 |
     | 第 4 字段 | GID（用户初始化组 ID）                                       |
     | 第 5 字段 | 用户说明                                                     |
     | 第 6 字段 | 家目录 <br/> 普通用户：/home/用户名/ <br/> 超级用户：/root/  |
     | 第 7 字段 | 登录之后的 shell                                             |

   - 解释重要字段
   
     1. 第 2 字段：
        - 密码标识，实际如帮助文档所说，x 表示密码加密处理后存储在 `shadow` 文件中，`shadow` 文件权限是 root 用户读写，其他用户无权限。
        - x 也表示此用户有密码，在 `shadow` 中存储。登陆时，去 `shadow` 中查询密码；如果 x 标志不写，则不会去 `shadow` 中寻找密码（免密登录）
        - 如 x 密码标志不写，则免密登录，但是只允许本地登录，远程不允许（ssh 协议不允许）
     2. 第 3 字段
        - Linux 系统只通过 UID 识别用户，用户名只是方便用户使用；如将一个普通用户的 UID 改成 `0` ，则它将会和 root 一样，系统便会将两个用户识别成一个用户
     3. 第 5 字段
        - 加用户说明是给管理员查看使用，加不加都可以，普通用户便没有添加
   
3. 初始组和附加组

   - __初始组__：就是指这个用户一登陆就立刻拥有这个组的相关权限，每个用户组的初始组只能有一个，一般就是和这个用户的用户名相同的组名为这个用户的初始组
   - __附加组__：指用户可以加入多个其他的用户组，并拥有这个组的权限，__附加组可以有多个__
   - 与 Windows 区别
     1. Windows：每添加一个用户，默认会将用户添加到 `users` 组（好像是这个组）
     2. Linux：每添加一个用户，就会默认添加一个与用户名相同的组，用户默认属于这个组
     3. __Linux：用户不能离开初始组，且只能有一个初始组；初始组可以改，但是不推荐改__
     4. __Linux 用普通用户变为超级用户：通过修改 UID  为 0（Window 是将用户加入管理员组中），家目录不变__
   - 初始组：即在 `/etc/passwd` 便是默认初始组，初始组要结合 `/etc/group` 文件结合查看

4. Shell 是什么

   - Shell 就是 Linux 的命令解释器
   - 在 `/etc/passwd` 当中，除了标准 Shell 是 `/bin/bash` 之外，还可以写成 `/sbin/nologin`
   - 使用的 Shell ，在 `/bin/bash` 的 shell 是可以登录的
   - __使用的 Shell，在 `/sbin/bash` 的 shell 是不可以登录的，如果手动修改了使用 shell 路径为 `/sbin/bash` ，则此用户则禁止登录__

5. __实例 `ss:x:1000:1000:ss,,,:/home/ss:/bin/bash` 说明__

   - 第 1 字段：`ss` 用户名
   - 第 2 字段：x 密码标识，表示此用户密码在 `/etc/shadow` 文件中加密存储
   - 第 3 字段：1000 UID 用户 ID
   - 第 4 字段：1000 GID 用户组 ID，默认组，创建用户时与用户同名
   - 第 5 字段：ss,,, 用户说明（方便记忆）
   - 第 6 字段：/home/ss 表现形式为用户登录默认路径
   - 第 7 字段：/bin/bash 使用的 shell 解释器的路径（/sbin/bash 不可登录）

#### 影子文件 `/etc/shadow`

__说明：真正存储用户密码的文件，已经加密__

1. 影子文件 `/etc/shadow`

   - 字段说明，共有 9 个字段

     | 字段      | 意义                                                         |
     | --------- | ------------------------------------------------------------ |
     | 第 1 字段 | 用户名                                                       |
     | 第 2 字段 | 加密密码 <br> 加密算法升级为 SHA512 散列加密算法 <br> 如果密码位是 "!!" 或 "*" 代表没有密码，__不能登录__ <br> 屏蔽某一用户密码 __可在密码前添加 ！以达到屏蔽该用户密码使其不能登录__ |
     | 第 3 字段 | 密码最后一次修改日期 <br> 使用 1970 年 1 月 1 日作为标准时间，每过一天时间戳加 1 <br> 即是距离 1970 年 1 月 1 日，过了多少天 |
     | 第 4 字段 | 两次密码的修改间隔时间（和第 3 字段相比）<br> 允许第一次修改密码和第二次修改密码的时间间隔 <br> 如果是 5 表示修改完密码，5 天之内不然允许再次修改 |
     | 第 5 字段 | 密码有效期（和第 3 字段相比）<br> 99999 大于 200 多年，表示永久有效 |
     | 第 6 字段 | 密码修改到期的警告天数（和第 5 字段相比）<br> 如果该字段为 7，表示密码有效期还有 7 天到期时，每次登录会有提示信息 |
     | 第 7 字段 | 密码过期后的宽限天数（和第 5 字段相比）<br> 0：代表密码过期后立即失效 <br> -1：则代表密码永远不会失效 <br> 空：表示到期立即失效 <br> 5：表示再宽限 5 天 |
     | 第 8 字段 | 账号失效时间，使用时间戳表示 <br> 时间一到，__不管其他就是封号__（不需要前面的字段设置） |
     | 第 9 字段 | 保留                                                         |

   - 实例 （密码有删减）

     ```tex
     root:$6$ovRUoBAm$oKNg0qIeX3g3tQei/dHuVMs8zsgQ06zi1moGl2vLZOGcPMrUDN.:18256:0:99999:7:::
     daemon:*:17737:0:99999:7:::
     bin:*:17737:0:99999:7:::
     ss:$6$ZY/WU02i$Gx.nqrtxuuYhlMwEczpdHsYo8PSzmnvU00gfK.JbaP/:18234:0:99999:7::: 
     ```

2. 时间戳计算公式

   - 介绍
     
   1. 使用 1970 年 1 月 1 日作为标准时间，没过一天时间戳加 1，即是距离 1970 年 1 月 1 日，过了多少天
   
   - 把时间戳换算位日期

     1. `date -d "1970-1-1 17000 days"`

        ```shell
        root@localcomputer:/etc# date -d "1970-1-1 18278 days"
        2020年 01月 17日 星期五 00:00:00 CST
        ```

   - 把日期换算位时间戳

     1. `echo $(($(date --date="2020/01/17" + %s)/86400+1))`

        ```shell
        root@localcomputer:/etc# echo $(($(date --date="2020/01/17" +%s)/86400+1))
        18278
        ```

3. 实例 `ss:$6$ZY/WUmnvU00gfK.JbaP/:18234:0:99999:7:1:30: ` 说明（密码有删减和其他修改）

   - 第 1 字段：ss 用户名称
   - 第 2 字段：$... ...P/ 密码加密过后
   - 第 3 字段：18234 最后修改密码时间戳（2019年 12月 04日），应该是初始密码，没有修改过
   - 第 4 字段：0 两次修改密码间隔时间，0 表示没有修改过密码
   - 第 5 字段：9999 密码有效期，从密码设置开始计算再过 9999 天，密码失效
   - 第 6 字段：7 表示密码还有 7 天时，每次登录都提醒用户更换密码（只是提醒）
   - 第 7 字段：1 表示密码有效期过了，密码还可已再使用 1 天
   - 第 8 字段：30 表示密码有效期就是 30 天。只要设置了第 8 字段，密码有效期就是他说的算，强制用户定期更换密码
   - 第 9 字段：预留

#### 组信息文件 `/etc/group`

1. 组信息文件 `/etc/group` 

   - 4 个字段表格

     | 字段      | 意义         |
     | --------- | ------------ |
     | 第 1 字段 | 组名         |
     | 第 2 字段 | 组密码标志   |
     | 第 3 字段 | GID          |
     | 第 4 字段 | 组中附加用户 |

   - 实例

     ```tex
     root:x:0:
     daemon:x:1:
     bin:x:2:
     sys:x:3:
     adm:x:4:syslog,ss
     
     ss:x:1000:
     ```

2. 理解

   - 查看用户的初始组，先查看 `/etc/passwd` 找到初始组 GID ，在上 `/etc/group` 查找对应 GID 的组名

#### 组密码文件 `/etc/gshadow`

1. 组密码文件 `/etc/gshadow`

   - 4 个字段

     | 字段      | 意义           |
     | --------- | -------------- |
     | 第 1 字段 | 组名           |
     | 第 2 字段 | 组密码         |
     | 第 3 字段 | 组管理员用户名 |
     | 第 4 字段 | 组中附加用户   |

   - 实例

     ```tex
     root:*::
     daemon:*::
     bin:*::
     sys:*::
     adm:*::syslog,ss
     
     ss:!::
     ```

2. 组的管理- __待补__

   - 密码：不推荐使用
   - 只有 root 有权限对组进行添加、删除
   - root 可以对组设置一个组长，组长有对组执行 root 权限

### 用户管理相关文件

#### 用户家目录

1. 用户家目录
   - 普通用户：`/home/用户名/` ，所有者和所属组都是此用户，权限是 700
   - 超级用户：`/root` ，所有者和所属组都是 root 用户，权限 550

#### 用户邮箱

1. 用户邮箱 `/var/spool/mail/用户名/`
   - 这里说明：Linux 的用户可以收发邮件，是通过 Linux 的 __内存__ 作为转发（用户是指使用同一台 Linux 的用户）
   - 而其他邮箱服务是需要服务器作为基础的（收发邮件是需要服务器作转发）
   - 每个用户邮箱地址 `/var/spool/mail/` ，每一个用户默认都会有一个邮箱

#### 用户模板目录

1. 用户模板目录 `/etc/skel/` 
   - 作用：每一个用户都会拷贝 `/etc/skel/` 文件到 __用户家目录__。可以提示用户使用规范、要求（每一个用户创建时都会在家目录拷贝 `/etc/skel/`）

### 用户管理命令

#### 用户添加命令 `useradd` 

1. `useradd` 命令格式
   
   - 命令 `useradd [选项] 用户名`
   
   - 选项
     
     | 选项 | 意义                                               |
     | ---- | -------------------------------------------------- |
     | `-u` | UID：手动指定用户的 UID                            |
     | `-d` | 用户说明：手动指定用户的说明                       |
     | `-c` | 用户说明：手动指定用户的说明                       |
     | `-g` | 组名：手动指定用户的初始组                         |
     | `-G` | 附加组组名：指定用户的附加组                       |
     | `-s` | shell：手动指定用户的登录 shell，默认是 `/bin/bash |
     
   - 实例
     1. 添加用户 `useradd shen` ；设置密码 `passwd shen` 
     2. 添加用户，不设置密码，用户添加不完整，无法登录
     3. __Ubuntu 的桌面版，好像可以添加用户，但是好像不能登录桌面版（Ubuntu 桌面版默认登录桌面）；远程连接登录（不是桌面版）可以。意思就是只要不是使用桌面版 （`init 5`）都可以登录__
   
2. 添加用户会修改什么文件呢？
   - `useradd shen` 到底执行了什么，以下文件文件正常会添加的
   
   - __passwd 文件__ `grep shen /etc/passwd` 
   
     ```tex
     shen:x:1001:1001::/home/shen:/bin/sh
     ```
   
   - __shadow文件__`grep shen /etc/shadow`
   
     ```shell
     shen:$6$rZUIxEBX$cgeCo0RBbArAlktFaz0WF/7wTAezY4AKAZeLxJKA7G8rJtgkmS9upDG70HXQpBnXtqVWaIyn.qNlkDTTkqGJE.:18279:0:99999:7:::
     ```
   
   - __group 文件__`grep shen /etc/group`
   
     ```shell
     shen:x:1001:
     ```
   
   - __gshadow 文件__`grep shen /etc/gshadow`
   
     ```shell
     shen:!::
     ```
   
   - __家目录__`ls -dl shen /home/shen` （无法创建家目录，所以无法登录。如果手动创建，需要修改权限）
   
     ```shell
     root@localcomputer:/etc# ls -d /home/shen
     ls: 无法访问'/home/shen': 没有那个文件或目录
     ```
   
   - __邮箱__`ls -a /var/spool/mail/shen` (没有邮箱，不知道为什么)
   
     ```shell
     root@localcomputer:/etc# ls -a /var/spool/mail/
     .  ..  root  ss
     ```
   
   - __所以创建用户时，会对用户配置文件和用户管理文件进行修改和创建__
   
3. 实例

   - 创建用户并手动定义几个配置和管理文件
     1. `useradd -u 1003 -G root,bin -d /home/shen -c "test user" -s /bin/bash shen`
     2. UID：指定 1003
     3. 附加组：root，bin
     4. 创建家目录：/home/shen，可以自定义目录，权限也会分配好
     5. 用户说明：test user
     6. 指定内核编译器：/bin/bash
   - __注意点__
     1. Linux 将用户添加附加组，和 Windows 添加进组不同，组有组的权限。如果想将用户变成  root 权限，只能修改 UID，将用户 UID 修改成 0 即可。
     2. __最好不要修改用户初始组__。想要进行其他组权限，可以使用附加组嘛

#### 创建用户默认初始值（仔细查看）

1. 用户默认值文件

   - __`/etc/default/useradd`（有写不同）__

     | 配置设置                   | 意义                                     |
     | -------------------------- | ---------------------------------------- |
     | `GROUP=100`                | 用户默认组                               |
     | `HOME=/home`               | 用户家目录的宿主目录                     |
     | `INACTIVE=-1`              | 用户密码过期宽限天数（shadow 第 7 字段） |
     | `EXPIRE=`                  | 密码失效时间（shadow 第 8 字段）         |
     | `SHELL=/bin/bash`          | 默认 shell                               |
     | `SKEL=/etc/skel`           | 模板目录                                 |
     | `CREATE_MAIL_SPOOL=yes/no` | 是否建立邮箱                             |

   - 解释

     1. Linux 有两种模式，一种 __共有模式__，另一种 __私有模式__。现在大部分 Linux 默认时私有模式，初始组会创建同名的初始组，而共有模式，用户初始组会指向 GID=100

   - __`/etc/login.defs` (有些内容不同，仔细查看以下)__

     | 配置设置                | 意义                             |
     | ----------------------- | -------------------------------- |
     | `PASS_MAX_DAYS 9999`    | 密码有效期（shadow 第 5 字段）   |
     | `PASS_MIN_DAYS 0`       | 密码修改间隔（shadow 第 4 字段） |
     | `PASS_MIN_LEN 5`        | 密码最小 5 为（PAM）             |
     | `PASS_WARN_AGE 7`       | 密码到期警告（shadow 第 6 字段） |
     | `UID_MIN`               | 最小和最大 UID 范围（500-6000）  |
     | `GID_MAX`               |                                  |
     | `ENCRYPT_METHOD SHA512` | 加密模式                         |

   - 解释

     1. 密码长度为 5 已经失效，密码 PAM 验证生效（升级）
     2. Ubuntu 的 UID 已经从 1000 开始了

#### 修改用户密码 `passwd`

__说明：创建用户没有设置密码，则无法正常登录__

1. `passwd` 命令格式

   - 命令 `passwd [选项] 用户名`

   - 选项

     | 选项      | 意义                                 |
     | --------- | ------------------------------------ |
     | `-S`      | 查询用户密码状态。仅 root 可用       |
     | `-l`      | 暂时锁定用户。仅 root 用户可用       |
     | `-u`      | 解锁用户。仅 root 用户可用           |
     | `--stdim` | 可以通过管道符输出的数据作为用户密码 |

   - 实例 1（root 用户修改密码，可以直接修改，不需要输入之前密码）

     ```shell
     root@localcomputer:/var/mail# passwd shen
     输入新的 UNIX 密码： 			# 直接输入新密码
     重新输入新的 UNIX 密码： 
     passwd：已成功更新密码
     ```

   - 实例 2 （普通用户修改密码，需要输入之前密码）

     ```shell
     ss@localcomputer:/var/mail$ passwd ss
     更改 ss 的密码。
     （当前）UNIX 密码： 			# 输入当前密码
     输入新的 UNIX 密码： 
     重新输入新的 UNIX 密码： 
     密码未更改
     输入新的 UNIX 密码： 
     ```

   - 实例 3 （`passwd` 修改当前用户密码，root 用户修改 root 用户密码）

     ```shell
     root@localcomputer:/home/ss# passwd
     输入新的 UNIX 密码： 			# root 用户修改密码，直接输入新密码
     重新输入新的 UNIX 密码： 
     passwd：已成功更新密码
     ```

2. 小结

   - root 用户：可以给所有用户修改密码，包括自己本身，__且不需要输入当前密码，直接输入新密码，可与当前密码相同__
   - 普通用户：只可以给自己修改密码，__必须输入当前密码，才可修改，不可与当前密码相同（相同提示密码未修改重新输入）__

3. 实例（超级用户）

   - 命令查看用户密码状态 `passwd -S ss` ，实际就是查看 `/etc/shadow` 对应用户行的时间相关设置

     ```shell
     root@localcomputer:/home/ss# passwd -S ss
     ss P 12/04/2019 0 99999 7 -1
     # 用户名 ss
     # 状态标志 P没有锁定；L 锁定
     # 用户密码设定时间 12/04/2019
     # 修改密码间隔时间 0
     # 密码有效时间 99999
     # 警告时间 7
     # 密码不失效 -1；0 时间到密码立即过期
     ```

   - 锁定用户 `passwd -l 用户`，__用户在不可登录__；解锁用户 `passwd -u 用户`，__用户可以登录__

     ```shell
     root@localcomputer:/home/ss# passwd -S ss		# 查看状态信息 P
     ss P 12/04/2019 0 99999 7 -1
     root@localcomputer:/home/ss# passwd -l ss		# 锁定用户
     passwd：密码过期信息已更改。
     root@localcomputer:/home/ss# passwd -S ss		# 查看状态信息 L
     ss L 12/04/2019 0 99999 7 -1
     root@localcomputer:/home/ss# passwd -u ss		# 解锁
     passwd：密码过期信息已更改。
     root@localcomputer:/home/ss# passwd -S ss		# 查看状态信息 P
     ss P 12/04/2019 0 99999 7 -1
     ```

   - 真正上锁的原理

     1. 上锁是将 `/etc/shadow` 对应用户行的密码加上  `!`，导致密码验证不通过，所以禁止登录

        ```tex
        ss:!$6$ZY/WU02i$Gx.nqrtxuuYhlMRmyEJpcT7vCVv6ZXE.Pji8Yo1u1YvAuEVWY9Y7z1J0CwEczpdHsYo8PSzmnvU00gfK.JbaP/:18234:0:99999:7:::
        ```

     2. 解锁（`!` 号去掉）

        ```shell
        ss:$6$ZY/WU02i$Gx.nqrtxuuYhlMRmyEJpcT7vCVv6ZXE.Pji8Yo1u1YvAuEVWY9Y7z1J0CwEczpdHsYo8PSzmnvU00gfK.JbaP/:18234:0:99999:7:::
        ```

   - 使用字符串作为用户密码

     __说明：shell 脚本，批量添加用户__

     ```shell
     echo "123" | passwd --stdin lamp
     ```

#### 修改用户信息 `usermod`

__说明：同 `useradd [-u/g/G/d]` 作用相同__

1. `usermod` 与 `useradd [-u/g/G/d]` 区别

   - `useradd` 添加用户时，设定用户相关选项。__针对新用户__
   - `usermod` 是修改已存在的用户相关选项。__针对已存在用户__

2. 命令 `usermod [选项] 用户名`

   - 选项 

     | 选项 | 意义                   |
     | ---- | ---------------------- |
     | `-u` | 修改用户 UID           |
     | `-c` | 修改用户的说明信息     |
     | `-G` | 修改用户附加组         |
     | `-L` | 临时锁定用户（Lock）   |
     | `-U` | 解锁用户锁定（Unlock） |

3. 实例

   - 修改 shen 用户

     ```shell
     usermod -c "test shen" shen
     usermod -u 1005 shen		# 修改已存在用户信息
     
     shen:x:1005:1001:test shen:/home/shen:/bin/sh		# /etc/passwd 文件
     ```

   - 锁定用户 `usermod -L shen` ；解锁 `usermod -U shen`

     1. 原理：也是将 `/etc/shadow` 的对应行的加密密码前加 `!` ，以达到密码不一致，无法登录

#### 修改用户密码状态 `chage`

1. 命令 `chage [选项] 用户名`

   - 选项

     | 选项      | 意义                                         |
     | --------- | -------------------------------------------- |
     | `-l`      | 列出用户详细密码状态                         |
     | `-d` 日期 | 修改密码最后一次更改日期（shadow 第 3 字段） |
     | `-m` 天数 | 两次密码修改间隔（shadow 第 4 字段）         |
     | `-M` 天数 | 密码有效期（shadow 第 5 字段）               |
     | `-W` 天数 | 密码过期警告天数（shadow 第 6 字段）         |
     | `-I` 天数 | 密码过期后宽限天数（shadow 第 7 字段）       |
     | `-E` 日期 | 密码失效时间（shadow 第 8 字段）             |

2. __注意__

   - 修改密码修改时间 `chage -d 0 用户`
     1. 这个密码其实是把密码修改日期归 0，（从 1970/01/01 就没有修改过密码）`/etc/shadow` 第 3 字段。
     2. 这样用户一登陆就要修改密码
   - 使用这条命令。可以时普通用户一登陆就需要修改密码，__系统强制修改密码__

#### 删除用户 `userdel`

1. 命令 `userdel [-r] 用户名`
   - 选项
     1. `-r` 删除用户同时删除用户家目录
2. 手动删除用户
   - 修改行对应用户 `vim /etc/passwd`
   - 修改行对应用户 `vim /etc/shadow`
   - 修改行对应用户 `vim /etc/group`
   - 修改行对应用户 `vim /etc/gshadow`
   - 删除 `rm -rf var/spool/mail/username`
   - 删除 `rm -rf /home/username`

#### 查看用户 ID

1.命令 `id 用户名`

2. 演示

   - 查看 shen 用户：UID；初始组 ID；附加组 ID

     ```shell
     root@localcomputer:/home/ss# id shen
     uid=1005(shen) gid=1001(shen) 组=1001(shen),0(root)
     ```

#### 用户切换命令 `su`

1. 命令 `su [选项] 用户名`

   - 选项

     1. `-` ：选项只使用 `-` 代表连带用户的环境变量一起切换

     2. `-c`  ：仅执行一次命令，而不切换用户身份

        `su - root -c "命令"` 以 root 身份执行命令（必须知晓 root 密码）

2. 演示

   - 切换用户（连带环境一起切换）`su - root`

     ```shell
     root@localcomputer:~# env		# 查看环境变量
     ...
     LC_MEASUREMENT=zh_CN.UTF-8
     LESSCLOSE=/usr/bin/lesspipe %s %s
     LC_PAPER=zh_CN.UTF-8
     LC_MONETARY=zh_CN.UTF-8
     LANG=zh_CN.UTF-8
     DISPLAY=:0
     COLORTERM=truecolor
     LC_NAME=zh_CN.UTF-8
     USER=root
     PWD=/root
     HOME=/root
     XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktop
     LC_ADDRESS=zh_CN.UTF-8
     LC_NUMERIC=zh_CN.UTF-8
     MAIL=/var/mail/root
     SHELL=/bin/bash
     TERM=xterm-256color
     SHLVL=1
     LANGUAGE=zh_CN:zh:en_US:en
     LC_TELEPHONE=zh_CN.UTF-8
     LOGNAME=root
     PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin
     LC_IDENTIFICATION=zh_CN.UTF-8
     LESSOPEN=| /usr/bin/lesspipe %s
     LC_TIME=zh_CN.UTF-8
     _=/usr/bin/env
     ```

   - 使用 root 用户身份，执行一次命令 `su - root -c "passwd -S shen"`

     ```shell
     ss@localcomputer:~$ su - root -c "passwd -S shen"
     密码： 
     shen P 01/18/2020 0 99999 7 -1
     ```

### 用户组管理命令

#### 添加用户组

1. 命令 `groupadd [选项] 组名`

   - 选项
     1. `-g` ：指定组 ID

2. 实例

   - 添加组 `groupadd testgroup`

     ```tex
     shen:x:1001:shen
     testgroup:x:1003:
     ```

   - 指定组 ID `groupadd -g 1009 newtestgroup`，组 ID，Ubuntu 的组 ID 是从 1000 开始这个累加的

     ```tex
     shen:x:1001:shen
     testgroup:x:1003:
     newtestgroup:x:1009:
     ```

#### 修改用户组

1. 命令 `groupmod [选项] 组名`

   - 选项

     1. `-g` ：修改组 ID

     2. `-n` ：修改组名

        `groupmod -n newgroup oldgroup` 

2. 实例

   - 修改组名 `groupmod -n new testgroup`

     ```tex
     shen:x:1001:shen
     newtestgroup:x:1009:
     new:x:1003:			# 原始 testgroup:x:1003: 
     ```

#### 删除用户组

1. 命令 `groupdel 组名`
   - 没有选项----直接可以删除
2. __注意__
   - 空组可以直接删除
   - 组中存在 __初始用户__ ，此组不可以删除
   - 组中存在 __附加用户，没有初始组__ ，此组可以删除

#### 把用户添加入组或从组中删除

__说明：此命令操作对象为 group，上面的命令是针对 user__

1. 命令 `gpasswd [选项] 组名`

   - 选项
     1. `-a` 用户名：把用户加入组中
     2. `-d` 用户名：把用户从组中删除
   - 注意
     1. 加入的用户是以附加用户加入的
     2. __/etc/group 文件中，可以查看到的用户都是附加组 __

2. 演示

   - 添加用户 `gpasswd -a ss root`

     ```shell
     root@localcomputer:~# gpasswd -a ss root
     正在将用户“ss”加入到“root”组中
     
     root:x:0:ss		# group 文件中
     ```

   - 删除用户 `gpasswd -d ss root`

     ```shell
     root@localcomputer:~# gpasswd -d ss root
     正在将用户“ss”从“root”组中删除
     ```

## 权限管理

### ACL 权限

#### ACL 权限简介与开启

1. ACL 解决问题描述

   - 问题如图

     ![ACL问题描述](git_picture/ACL问题描述.jpg)

   - 文件或目录有 __所有者权限、所属组权限、其他用户权限__，3 者的权限是所属者权限大于等于所属组（一般大于）、所属组权限大于其他用户权限

   - 如图： 3 者权限分配是 __770__，即所有者、所属组对文件或目录拥有全部权限，其他用户没有任何权限

   - 如图：此时 __用户 A__ 要对 文件或目录拥有 __5 权限__ 时，现有文件权限分配并不能满足用户 A，及一个文件或目录的权限分配不能满足实际情况（理论上一个文件或目录会有 3 中权限分配：所有者、所属组、其他人），所以 __ACL 权限__ 是对此问题的一个解决办法

   - 注意：__文件或目录只能有一个所属组__
   
2. ACL（与 Windows 相同） 对文件或目录的权限管理解释

   - 不考虑已存在的 __所有者、所属组、其他人__ 的权限问题
   - 即来一个用户，对这个用户单独分配对文件或目录的权限

3. 查看分区 ACL 权限是否开启

   __说明：ACL 是用户操作文件权限，但是否支持 ACL 权限，不是文件支持、也不是用户支持，而是文件所在的分区是否支持 ACL 权限__

   - 命令 `dumpe2fs -h /dev/sad3`

   - `dumpe2fs` 命令是查询指定分区详细文件系统信息的命令；__各个分区使用情况 `df -h`__

   - 选项

     1. `-h` ：仅显示超级快中信息，而不显示磁盘块组的详细信息

   - 验证分区是否开启 ACL（一般默认都会开启 ACL）

     1. 查看 `/` 分区是否支持 ACL

        ```shell
        root@localcomputer:~# dumpe2fs -h /dev/sda5  # 查看信息
        dumpe2fs 1.44.1 (24-Mar-2018)
        Filesystem volume name:   <none>
        Last mounted on:          /
        Filesystem UUID:          f4f8a8cd-3279-4366-8e69-5f7845a78446
        Filesystem magic number:  0xEF53
        Filesystem revision #:    1 (dynamic)
        Filesystem features:      has_journal ext_attr resize_inode dir_index filetype needs_recovery extent 64bit flex_bg sparse_super large_file huge_file dir_nlink extra_isize metadata_csum
        Filesystem flags:         signed_directory_hash 
        Default mount options:    user_xattr acl		# 支持 ACL
        Filesystem state:         clean
        ```

4. 临时开启分区 ACL 权限

   - 命令 `mount -o remount,acl /` 
   - 重新挂载根分区，并挂载加入 ACL 权限

5. 永久开启分区 ACL 权限

   - 命令 `vim /etc/fstab`
     1. 修改添加 `acl` ：`UUID=f4f8a8cd-3279-4366-8e69-5f7845a78446 /               ext4    defaults,acl        0       1`
     2. 但是我这个 `/etc/fstab` 中这行并不是使用 `defaults` ，而是使用 `errors=remount-ro` ，不知道为什么。可以修改为 `defaults` 不会出错
   - 重启系统、重新挂载文件系统（`mount -o remount /`）,使修改生效

#### 查看与设定 ACL 权限

__说明：一般分区都会支持 ACL 权限，所以以后可以直接使用，不需要查看是否支持 ACL 权限__

1. 查看 ACL 命令

   - 查看 ACL 权限命令 `getfacl 文件` 

2. 设定 ACL 权限的命令

   - 命令 `setfacl [选项] 文件名`

   - 选项

     | 选项 | 意义                                             |
     | ---- | ------------------------------------------------ |
     | `-m` | 设定 ACL 权限 `setfacl -m u/g:用户/组:权限 文件` |
     | `-x` | 删除指定 ACL 权限                                |
     | `-b` | 删除所有 ACL 权限                                |
     | `-d` | 设定默认 ACL 权限                                |
     | `-k` | 删除默认 ACL 权限                                |
     | `-R` | 递归设定 ACL 权限                                |

3. 演示（`setfacl -m`）

   - 根目录下创建目录 `mkdir /project`

     ```shell
     root@localcomputer:~# mkdir project
     root@localcomputer:~# ls
		mbox  project			# 新建 project 目录
     ```
     
   - 创建两个用户和一个组
   
     ```shell
     root@localcomputer:~# useradd a
     root@localcomputer:~# useradd b
     root@localcomputer:~# groupadd tgroup
     
     ################################### /etc/passwd 文件
     a:x:1001:1001::/home/a:/bin/sh
     b:x:1002:1010::/home/b:/bin/sh
     
     ################################### /etc/group
     newtestgroup:x:1009:
     new:x:1003:
     a:x:1001:
     b:x:1010:
     tgroup:x:1011:
     ```
   
   - 将两个用户添加到一个组中（是添加默认组）
   
     ```shell
     root@localcomputer:~# gpasswd -a b tgroup
     正在将用户“b”加入到“tgroup”组中
     root@localcomputer:~# gpasswd -a a tgroup
     正在将用户“a”加入到“tgroup”组中
     
     ############################################# /etc/group
     new:x:1003:
     a:x:1001:
     b:x:1010:
     tgroup:x:1011:b,a
     ```
   
   - 调整 `/project` 文件所有者和权限
   
     ```shell
     root@localcomputer:~# ls -dl ./project
     drwxr-xr-x 2 root root 4096 1月  22 19:29 ./project
     root@localcomputer:~# chown root:tgroup project/    # 修改所有者和组
     root@localcomputer:~# ls -dl ./project
     drwxr-xr-x 2 root tgroup 4096 1月  22 19:29 ./project  # 修改结果
     
     root@localcomputer:~# chmod 770 project/			# 修改文件权限问题
     root@localcomputer:~# ls -dl project/
     drwxrwx--- 2 root tgroup 4096 1月  22 19:29 project/   # 修改结果
     ```
   
   - __使用 `setfacl` 添加一个用户并赋予他其他权限 `setfacl -m u/g:用户/组:权限 文件`__
   
     ```shell
     # 先添加用户，其用户不是所有者、所属组、其他用户，是一个另类用户
     root@localcomputer:~# useradd c
     
     # 设置 acl 权限 rx
     root@localcomputer:~# setfacl -m u:c:rx ./project
     root@localcomputer:~# ls -ld ./project/
     drwxrwx---+ 2 root tgroup 4096 1月  22 19:29 ./project/    # 权限显示有一个 + 号，表示有acl权限
     root@localcomputer:~# getfacl ./project/	# 查看具体acl权限
     # file: project/
     # owner: root
     # group: tgroup
     user::rwx		# 所有者权限
     user:c:r-x		# acl权限
     group::rwx		# 所属组权限
     mask::rwx		# 应该是最大有效权限
     other::---		# 其他用户权限
     ```
   

#### 最大有效权限与删除 ACL 权限

1. 最大有效权限

   - mask 是用来指定最大有效权限的。如果 root 给用户赋予了 acl 权限，是需要与 __mask 权限 相与__ 才能得到用户真正的权限

   - 既可以通过 mask 权限来控制 ACl 真正的权限

   - mask 权限会与 acl 权限、组权限 __相与__

   - 修改 mask 权限 `setfacl -m m:权限 文件` (注释 `#effective` 写的很明白，真正的权限是什么)

     ```shell
     root@localcomputer:~# setfacl -m m:r ./project/
     root@localcomputer:~# getfacl ./project/
     # file: project/
     # owner: root
     # group: tgroup
     user::rwx
     user:c:r-x			#effective:r--
     group::rwx			#effective:r--
     group:t:rw-			#effective:r--
     mask::r--
     other::---
     ```

     `user:c:r-w` 与 mask 权限 `mask::r--` 相与，所以实际用户 c 的权限是 `r` ,__而 group 组也是受 mask 影响__

2. 最大权限作用

   - 保证目录权限在可控范围之内，即可以先设定 mask 权限，即使其他用户权限设定过高也没事，最后实际权限和 __最大权限有关__
   - 不受 mask 权限影响的 __所有者、其他用户__

3. __删除 ACL 权限__

   - 删除指定用户 ACL 权限命令 `setfacl -x u:用户名 文件名`
   - 删除指定用户组 ACL 权限命令 `setfacl -x g:组名 文件名`
   - 删除文件的所有 ACL 权限 `setfacl -b 文件`

4. 实例

   - 删除组 ACL 权限，删除 tgroup 组 ACL 权限

     ```shell
     root@localcomputer:~# getfacl ./project/
     # file: project/
     # owner: root
     # group: tgroup
     user::rwx
     user:c:r-x			#effective:r--
     group::rwx			#effective:r--
     group:t:rw-			#effective:r--
     mask::r--
     other::rwx
     
     root@localcomputer:~# setfacl -x g:t ./project/
     root@localcomputer:~# getfacl ./project/
     # file: project/
     # owner: root
     # group: tgroup
     user::rwx
     user:c:r-x
     group::rwx
     mask::rwx
     other::rwx
     ```

   - 删除文件所有 ACL 权限

     ```shell
     root@localcomputer:~# setfacl -x g:t ./project/
     root@localcomputer:~# getfacl ./project/
     # file: project/
     # owner: root
     # group: tgroup
     user::rwx
     user:c:r-x
     group::rwx
     mask::rwx
     other::rwx
     
     root@localcomputer:~# setfacl -b ./project/
     root@localcomputer:~# getfacl ./project/
     # file: project/
     # owner: root
     # group: tgroup
     user::rwx
     group::rwx
     other::rwx
     ```
     

#### 默认 ACL 权和递归 ACL 权限 

1. 递归 ACL 权限

   - 递归是父目录在设定 ACL 权限时，所有子文件和子目录也会拥有相同的 ACL 权限

   - 命令 `setfacl -m u:用户:权限 -R 目录` （只能是 __目录__ ,文件不可以，因为是递归嘛！！！）

   - 注意

     1. 递归只能对现有文件做 ACL 权限，__现有文件也只能使用 递归 ACL 权限__
     2. 新建文件使用默认 ACL 权限，会继承父目录 ACL 权限

   - 实例

     1. 使用递归 ACL 权限，__只能对已存在文件使用递归 ACL 权限，新建文件没用（新建文件使用默认 ACL 权限）__

        ```shell
        root@localcomputer:/home/ss# ls -dl ./project/
        drwxr-xrwx+ 2 root tgroup 4096 1月  26 21:59 ./project/		# 权限表示有 +，有 ACL 权限
        root@localcomputer:/home/ss# cd ./project/
        root@localcomputer:/home/ss/project# ls -l 
        总用量 0
        -rw-r--r-- 1 root root 0 1月  26 21:59 a
        -rw-r--r-- 1 root root 0 1月  26 21:59 b				# 新建文件没有 ACL 权限
        root@localcomputer:/home/ss/project# setfacl -m u:c:rwx -R ../project/  # 使用递归 ACL 权限
        root@localcomputer:/home/ss/project# ls -l
        总用量 0
        -rw-rwxr--+ 1 root root 0 1月  26 21:59 a
        -rw-rwxr--+ 1 root root 0 1月  26 21:59 b		# 有 ACL权限
        root@localcomputer:/home/ss/project# touch c	 # 新建文件 c
        root@localcomputer:/home/ss/project# ls -l
        总用量 0
        -rw-rwxr--+ 1 root root 0 1月  26 21:59 a
        -rw-rwxr--+ 1 root root 0 1月  26 21:59 b
        -rw-r--r--  1 root root 0 1月  26 22:07 c        # 新建文件 c ，没有 ACL 权限
        ```

2. 默认 ACL 权限

   - 默认 ACL 权限的作用是如果给父目录设定了默认 ACL 权限，那么父目录中所有新建的子文件都会继承父目录的 ACL 权限

   - 命令 `setfacl -m d:u:用户名:权限 文件名` ，`-R` 加不加不一样，默认 ACL 权限也会递归的

   - 注意

     1. 默认 ACL 权限，只针对新建文件
     2. 已有文件还得使用 __递归 ACL 权限__
     3. 在目录下创建的目录、文件都会遵守默认 ACL 权限

   - 实例 

     1. 使用默认 ACL 权限，新建文件继承父目录的 ACL 权限

        ```shell
        root@localcomputer:/home/ss# setfacl -m d:u:c:rwx  ./project/		# 设置默认 ACL 权限
        root@localcomputer:/home/ss# cd ./project/
        root@localcomputer:/home/ss/project# ls
        a  b  c
        root@localcomputer:/home/ss/project# ls -l
        总用量 0
        -rw-rwxr--+ 1 root root 0 1月  26 21:59 a
        -rw-rwxr--+ 1 root root 0 1月  26 21:59 b
        -rw-r--r--  1 root root 0 1月  26 22:07 c
        root@localcomputer:/home/ss/project# touch d			# 新建文件 d
        root@localcomputer:/home/ss/project# ls -l
        总用量 0
        -rw-rwxr--+ 1 root root 0 1月  26 21:59 a
        -rw-rwxr--+ 1 root root 0 1月  26 21:59 b
        -rw-r--r--  1 root root 0 1月  26 22:07 c
        -rw-rw-rw-+ 1 root root 0 1月  26 22:20 d				# 新建文件 d，继承父目录，ACL 权限	
        ```

### 文件特殊权限  

#### SetUID

1. SetUID 的功能

   - 只有可执行的二进制程序才能设定 SUID 权限，普通文件、目录不能设 SUID 权限（设置也不会报错，只是没有意义）
   - 命令执行者（一般是普通用户）要对该程序拥有 x（执行）权限
   - 命令执行者在执行该程序时获得该程序文件属主身份（__在执行程序的过程中灵魂附体为文件属主__）
   - SetUID 权限只在该程序执行过程有效，也就是说身份改变只在程序执行过程中有效
   - __SetUID 的作用是，任何一个用户在执行拥有 SetUID 权限程序时，会暂时获得文件所有者的身份__

2. 用此功能的实际作用

   - `passwd` 命令拥有 SetUID 权限，所以普通用户可以修改自己密码

     1. SetUID 中 U 是所有者的意思，__表现形式：可执行文件的所有者权限 `-rwsr-xr-x` 的 s 权限__ ，即是 SetUID 的作用（任何用户执行拥有 SetUID 权限程序时，会暂时获得所有者身份）

        ```shell
        root@localcomputer:/home/ss# whereis passwd
        passwd: /usr/bin/passwd  /passwd /usr/share/man/man5/passwd.5.gz /usr/share/man/man1/passwd.1.gz /usr/share/man/man1/passwd.1ssl.gz
        root@localcomputer:/home/ss# ls -l /usr/bin/passwd
        -rwsr-xr-x 1 root root 59640 1月  25  2018 /usr/bin/passwd
        ```
        
     2. 解释：程序 passwd 所有者拥有 SetUID 权限，使得任何用户运行 passwd 程序时，会获得 __所有者身份__，即修改密码会将密码写入 `/etc/shadow` 文件中，而 `/etc/shadow` 的权限是 `-rw-r----- 1 root shadow 1489 1月  22 20:02 /etc/shado` ，可以看出普通用户没有权限修改文件，所以这个权限的作用再次体现
     
   - `cat` 命令没有 SetUID 权限，所以普通用户不能查看 `/etc/shadow` 文件内容
   
     1. 查看 `/bin/cat` 文件程序权限，`cat` 所有者权限没有 SetUID 权限
   
        ```shell
        ss@localcomputer:~$ ls -l /bin/cat
        -rwxr-xr-x 1 root root 35064 1月  18  2018 /bin/cat
        ```
   
     2. 解释：当 `/bin/cat` 程序文件没有 SetUID 权限，普通用户执行 `cat` 命令时，不能获得 __所有者身份__，即没有权限查看 `/etc/shadow` 文件
   
        ```shell
        ss@localcomputer:~$ cat /etc/shadow
        cat: /etc/shadow: 权限不够
        ```
   
   - __SetUID 理解__
   
     1. SetUID 是为了，普通用户修改或者其他操作属于自己的设置，但这些设置又设计系统安全，所以给用户提供了这种权限
     2. 只适用于 __二进制程序文件__
   
3. 设定 SetUID 的方法

   - 4 代表 SUID
     1. 设定 SUID 权限 `chmod 4755 文件名` 
        - 4 表示 SUID 权限，7 表示所有者，5 表示所属组，5 表示其他，这时一个完整的文件表现形式
        - 2 表示 SGID
        - 1 表示 BIT 
        - 7 表示三个权限多拥有，但是没有意义，三个权限对 3 种文件操作
     2. `chmod u+s 文件名`

4. 取消 SetUID 的方法

   - 取消 SUID 命令 `chmod 755 文件名`
   - `chmod u-s`

5. 注意

   - 使用 SUID 权限，留意 __文件是二进制可执行程序、任何用户都用可在执行权限__

   - 演示

     ```shell
     root@localcomputer:/home/ss# ls -l tc
     -rw-r--r-- 1 root root 0 1月  28 21:48 tc		# tc 文件任何用户都没有可执行权限
     root@localcomputer:/home/ss# chmod 4755 tc		# 使用 chmod 4755 可以直接修改，即使文件不是二进制，不是可执行程序，用户没有执行权限（没有意义）
     root@localcomputer:/home/ss# ls -l tc
     -rwsr-xr-x 1 root root 0 1月  28 21:48 tc		# 修改成功
     root@localcomputer:/home/ss# chmod 755 tc		# 修改回去，去掉 SUID 权限
     root@localcomputer:/home/ss# ls -l tc
     -rwxr-xr-x 1 root root 0 1月  28 21:48 tc		# 显示去掉 SUID 权限，但其他权限使用 chmod 修改
     root@localcomputer:/home/ss# chmod 644 tc		# 彻底修改回去
     root@localcomputer:/home/ss# ls -l tc
     -rw-r--r-- 1 root root 0 1月  28 21:48 tc
     root@localcomputer:/home/ss# chmod u+s tc		# 这种方式修改 SUID 会报错，这里强调上面 2 种条件，缺一不可
     root@localcomputer:/home/ss# ls -l tc
     -rwSr--r-- 1 root root 0 1月  28 21:48 tc		# 这里 S 是报错提示，不是可执行的二进制文件，用户没有执行权限
     ```

6. __危险的 SetUID__

   - 关键目录应严格控制写权限。比如 `/` 、`/user` 等

   - 用户的密码设置要严格遵循密码三原则

   - 对系统中默认应该具有 SetUID 权限文件作一列表，定期检查有没有这之外的文件被设置了 SetUID 权限

   - 解释

     1. 查看 `vim` 命令文件权限

        ```shell
        ss@localcomputer:~$ whereis vim
        vim: /usr/bin/vim.basic /usr/bin/vim.tiny /usr/bin/vim /etc/vim /usr/share/vim /usr/share/man/man1/vim.1.gz
        ss@localcomputer:~$ ls -l /usr/bin/vim.basic 
        -rwxr-xr-x 1 root root 2671240 6月   7  2019 /usr/bin/vim.basic
        ```

     2. `vim` 没有 SUID 权限，如果将 `vim` 修改为拥有 SUID 权限。任何用户执行 `vim` 命令时，将会转换为所有者身份（root），将会有权限修改 `/etc/shadow` 等一些文件，这将是很可怕的。

#### SetGID

##### 针对文件

1. SetGID 针对文件作用
   - 只有可执行的二进制程序才能设置 SGID 权限
   - 命令执行者要对该程序拥有 x （执行）权限
   - 命令执行在执行程序的时候，组身份升级为该程序文件的属组
   - SetGID 权限同样只在该程序执行过程中有效，也就是说组身份改变只在程序执行过程中有效
   
2. 演示：命令 `locate` 是查询 `/var/lib/mlocate/mlocate.db ` 文件内容

   - 文件 `/var/lib/mlocate/mlocate.db ` 权限__（640、所有者 root、所属组 mlocate，其中所属组有读权限）__

     ```shell
     ss@localcomputer:~$ ls -l /var/lib/mlocate/mlocate.db 
     -rw-r----- 1 root mlocate 9794090 1月  29 15:19 /var/lib/mlocate/mlocate.db
     ```

   - `locate` 二进制程序文件的权限（Ubuntu 软连接，最总指向 `/usr/bin/mlocate`），__其所属组为 mlocate__

     ```shell
     ss@localcomputer:~$ whereis locate		# 查询 locate 位置
     locate: /usr/bin/locate /usr/share/man/man1/locate.1.gz
     ss@localcomputer:~$ ls -l /usr/bin/locate
     lrwxrwxrwx 1 root root 24 12月  4 14:14 /usr/bin/locate -> /etc/alternatives/locate # 软链接
     ss@localcomputer:~$ ls -l /etc/alternatives/locate
     lrwxrwxrwx 1 root root 16 12月  4 14:14 /etc/alternatives/locate -> /usr/bin/mlocate # 软连接
     ss@localcomputer:~$ ls -l /usr/bin/mlocate
     -rwxr-sr-x 1 root mlocate 43088 3月   2  2018 /usr/bin/mlocate  # 最终指向 
     ```

   - 对 `locate` 命令执行解释

     1. 文件 `/usr/bin/locate` 最终指向 `/usr/bin/mlocate` 具有 SGID 权限
     2. 命令 `locate` 操作的文件是 `/var/lib/mlocate/mlocate.db` 文件 ，而文件的权限和所属组（mlocate 组）值得注意（具有 SGID 权限）
     3. 当任何用户执行 `locate` 命令时，用户组身份将会变成 mlocate 组，此时用户对于 `/var/lib/mlocate/mlocate.db` 具有查看权限，所以普通用户可以使用 `locate` 命令

3. 设置 SetGID

   - 2 表示 SGID
     1. 设置命令 `chmod 2755 文件`（借鉴 SUID 设置特点）
     2. `chmod g+s 文件`

4. 取消 SetGID

   - 2 表示 SGID
     1. 取消命令 `chmod 755 文件`
     2. `chmod g-s 文件`

5. 总结

   - __注意不要修改这个权限__

   - `/usr/bin/locate` （软链接）是可执行的二进制程序文件，可以赋予 SGID 权限
   - 执行用户 test 对 `locate` 拥有可执行权限
   - 执行 `locate` 命令时，test 组身份会变成 mlocate 组，而 mlocate 组对 `/var/lib/mlocate/mlocate.db` 数据库有查询权限，所以普通用户可以使用 `locate` 查询数据库
   - 命令结束，test 用户组身份变回原来组

##### 针对目录

1. SetGID 针对目录作用
   - 普通用户必须对此目录拥有 r 和 x 权限，才能进入此目录和查看目录（没有 w 权不能创建文件或目录）
   - 普通用户在此目录中的有效组会变成此目录的所属组（这句话的意义，是在于有 w 权限的表现）
   - 若普通用户对此目录拥有 w 权限时，新建的文件的默认属组是这个目录的属组
   
2. 实例

   - 加入 root 创建目录，普通用户需要对此目录有 w 权限，才可以创建文件或目录

   - 目录拥有 SGID 权限时，普通用户创建文件或目录时，文件的所属组，实际是目录的所属组

     ```shell
     ss@localcomputer:~$ ls -dl test/
     drwxrwsrwx 2 root root 4096 1月  29 22:21 test/	# 目录拥有 SGID 权限，所有者 root、所属组 root
     ss@localcomputer:~$ cd test/
     ss@localcomputer:~/test$ touch a		# ss 普通用户创建文件
     ss@localcomputer:~/test$ ls -l 
     总用量 0
     -rw-rw-r-- 1 ss root 0 1月  29 22:28 a	# 文件所属组是目录的所属组
     ```

   - __实际使用中，感觉没有什么意义__

#### Sticky BIT

__说明：stricky 黏着；bit 位，简称：位黏着位__

1. SBIT 黏着位作用

   - 黏着位目前只对目录有效
   - 普通用户对该目录拥有 w 和 x 权限，即普通用户拥有对该目录的写入权限
   - 如果没有黏着位，因为普通用户拥有 w 权限，所以可以删除此目录下的所有文件，包括其他用户建立的文件。一旦赋予黏着位，除了 root 可以删除所有文件，普通用户就算拥有 w 权限，也只能删除自己建立的文件，但不能删除其他用户建立的文件

2. 注意

   - 黏着位主要还是限制普通用户对目录的操作权限
   - 对 root 不起作用

3. 实例讲解

   - 根下的 `/tmp` 目录就是一个拥有黏着位的目录

     1. 查看 `/tmp` 的权限表现形式

        ```shell
        ss@localcomputer:/$ ls -ld tmp/
        drwxrwxrwt 18 root root 4096 1月  30 21:03 tmp/
        ```

     2. 权限表型形式为 __其他用户权限 x 变成 t__ ，`/tmp` 使用数字表示权限为 `1777`

     3. __普通用户即使拥有对 `/tmp` 拥有 rwx（t），但是有 SBIT 权限的作用，普通用户也只能对自己创建的文件删除操作（删除文件是用户对目录拥有 w 权限，不是对文件拥有什么）。这样保障了普通用户的权限__

        ```shell
        ss@localcomputer:/tmp$ rm -rf a
        rm: 无法删除'a': 不允许的操作
        ss@localcomputer:/tmp$ mv a b
        mv: 无法将'a' 移动至'b': 不允许的操作
        ```

4. 设置与取消粘着位

   - 设置粘着位
     1. `chmod 1755 目录名`
     2. `chmod o+t 目录名`
   - 取消粘着位
     1. `chmod 755 目录名`
     2. `chmod o-t 目录名`

#### 总结

1. SUID 
   - 只能赋予文件（可执行文件）
2. SGID
   - 可以赋予可执行文件
   - 可以赋予目录（似乎没什么用）
3. SBIT
   - 只能赋予目录
4. 注意
   - 从以上三种特殊权限的描述，一个文件或目录不可能 __在特殊权限中出现 7 这个权限__ ，因为作用的不是可执行文件就是目录

### 文件系统属性 chattr 权限

__说明：之前讲的文件或目录的权限对 root 都是无效的（umask 为默认权限），但是 chattr 对 root 有效__

1. 介绍
   
   - chattr - 修改 Linux 文件系统中的文件属性
   - chattr 是防止，包括 root 用户在内对 __文件误操作__ 的一把锁，既然是锁，那么既能上锁，也能解锁
   
2. chattr 命令格式
   - 命令格式 `chattr [+-=] [选项] 文件或目录名``
     1. ``+` ：增加权限
     2. `=` ：等于某权限
     3. `-` ：删除某权限
   - 选项
     1. `i` ：如果对文件设置 i 属性，那么不允许对文件进行删除、改名、也不能添加和修改数据；如果对目录设置 i 属性，那么只能修改目录下文件的数据，但不允许建立和删除文件。
     2. `a` ：如果对文件设置 a 属性，那么只能在文件中添加数据，但是不能删除也不能修改数据；如果对目录设置 a 属性，那么只允许在目录中建立和修改文件，但不允许删除
   
3. 查看文件系统属性

   - 命令 `lsattr [选项] 文件名`
   - 选项
     1. `-a` ：显示所有文件和目录
     2. `-d` ：若目标是目录，仅列出目录本身属性，而不是子文件的

4. 实例

   - chattr 连 root 都可以限制，__root 创建文件操作实例，添加 i 属性__

     1.  root 创建的文件或目录都会有默认的权限（`-rw-r--r--` 就是默认的权限）

        ```shell
        root@localcomputer:~# ls -ld test 
        -rw-r--r-- 1 root root 72 1月  30 22:53 test
        ```

     2. 使用 `lsattr -a 文件` 查看文件系统属性（e 表示 ext4 文件系统下创建的文件）

        ```shell
        root@localcomputer:~# ls -ld test 
        -rw-r--r-- 1 root root 72 1月  30 22:53 test
        root@localcomputer:~# lsattr -a test 
        --------------e--- test					# e 表示此文件在 ext4 文件系统下创建，默认不能取消
        ```

     3. 使用 `chattr +i 文件` 命令给文件添加 i 属性

        ```shell
        root@localcomputer:~# chattr +i test 
        root@localcomputer:~# lsattr -a test 
        ----i---------e--- test					# 添加 i 属性
        ```

     4. root 也没有权限删除文件，说明此权限对 root 也有效

        ```shell
        root@localcomputer:~# rm -rf test 
        rm: 无法删除'test': 不允许的操作
        ```

     5. 去掉 chattr 权限

        ```shell
        root@localcomputer:~# lsattr -a test 
        ----i---------e--- test
        root@localcomputer:~# chattr -i test 		# 去掉文件属性 i 权限
        root@localcomputer:~# lsattr -a test 
        --------------e--- test
        ```

   - root 创建文件添加 a 属性

     1. 文件（touch 创建的）追加文件内容 `echo 内容 >> 文件名`

        ```shell
        root@localcomputer:~# echo permit to write >> a		# 向文件 a 追加 permit to write 内容
        ```

     2. 创建文件，添加 a 属性，__此时文件只能追加，不能修改删除__

        ```shell
        root@localcomputer:~# chattr +a a
        root@localcomputer:~# lsattr -a a
        -----a--------e--- a				# 文件 a 添加 a 属性
        ```

     3. 只能使用 `echo 内容 >> 文件` 添加数据，__不能使用向 Vim 这样的编辑器__

        ```shell
        root@localcomputer:~# echo you >> a
        ```

### 系统命令 sudo 权限

__说明：以上讲解的权限都是用户对文件的操作权限，这次的被操作的对象是系统命令（虽然也是文件）__

1. sudo 权限

   -  root 把本来只能超级用户执行的命令赋予普通用户执行
   - sudo 的操作对象是系统命令（Linux 中命令也是文件）

2. sudo 使用

   - 切换 root 用户，使用命令 `visudo` ，实际修改的文件是 `/etc/sudoers` 文件内容
   - 格式 `root ALL=(ALL)    ALL` 
     1. 用户名 被管理主机的地址=(可使用的身份)    授权命令(绝对路径)
     2. 允许 __某个用户__ 使用 __某一个命令__ 在 __这一台主机上__
     3. ALL 表示本机，或者使用本机 IP也表示本机，也可以是网段（但是是针对一些服务的）
     4. (ALL) 表示任何一个用户，即用户在本机上执行命令代表任何一个用户（主要是 root 用户），__写不写都一样__
   - 格式 `%wheel ALL=(ALL)    ALL` 
     1. %组名 被管理主机的地址=(可使用的身份)    授权命令(绝对路径)

3. 授权 ss 用户可以添加用户

   - `useradd` 普通用户无权执行，所以 root 授权普通用户可以执行

     1. 查看 root 授权的，给 ss 用户哪些命令（我也不知道什么意思）

        ```shell
        匹配 %2$s 上 %1$s 的默认条目：		# 默认的环境变量
            env_reset, mail_badpass,
            secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
        
        用户 ss 可以在 localcomputer 上运行以下命令：  # ss 可以执行的命令
            (ALL : ALL) ALL
        ```

     2. 使用 `visudo` 编辑 `/etc/sudoers` 文件（root 执行，直接修改，使用 ^O 写入保存，^X 退出），__命令精确，权限相对较小__

        ```shell
        # Host alias specification
        
        # User alias specification
        
        # Cmnd alias specification
        
        # User privilege specification
        root    ALL=(ALL:ALL) ALL
        ss      ALL=(ALL:ALL) /usr/sbin/useradd		# 给 ss 用户赋予 passwd 命令
        
        # Members of the admin group may gain root privileges
        %admin ALL=(ALL) ALL
        ```

     3. 使用 `sudo -l` (ss 用户执行)

        ```shell
        匹配 %2$s 上 %1$s 的默认条目：
            env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
        
        用户 ss 可以在 localcomputer 上运行以下命令：
            (ALL : ALL) /usr/sbin/useradd	# root 授权 ss 用户执行 passwd 命令，以任何用户身份，在本机上
            (ALL : ALL) ALL
        ```

     4. 执行授权命令 `sudo /usr/sbin/useradd 用户` ，ss 执行的（不是绝对路径也可以）

        ```shell
        ss@localcomputer:~$ sudo useradd e
        ss@localcomputer:/root$ su - root
        密码： 
        root@localcomputer:~# su e
        $ whoami
        e
        ```

   - 注意

     1. 不要修改 `vim` 命令，因为修改 `vim` 命令后，将对用户没有任何限制。
     2. 任何用户执行 `vim` 时，都会拥有 root 权限，一些系统配置文件将没有限制

## 文件系统管理

### 回顾分区和文件系统

#### 硬盘分区

1. 分区 4 个类型

   - 主分区：总共只能分 4 个
   - 扩展分区：只能有一个，也算作主分区的一种，也就是说主分区加扩展分区最多有四个。但是扩展分区不能存储数据和格式化，必须在划成逻辑分区才能使用。
   - 逻辑分区：逻辑分区是扩展分区中划分的，如果是 IDE 硬盘，Linux 最多支持 59 个逻辑分区；如果是 SCSI 硬盘，Linux 最多支持 11 个逻辑分区

2. 分区表示法及分区设备文件名

   - 分区方式 1

     1. 如图划分

        ![Linux分区表示法](git_picture/Linux分区表示法1.png)

     2. 每一个分区对 Linux 都会有一个 __分区设备文件名__（分区设备文件名固定的，能看懂即可）
   
        | 分区       | 分区设备文件名                                               |
        | ---------- | ------------------------------------------------------------ |
        | 主分区 1   | /dev/sda1 <br> __sd 代表硬盘 SCSI接口（hd代表 IDE 硬盘接口），a代表第 1 块硬盘，[1, 2, 3, 4] 代表主分区__ |
        | 主分区 2   | /dev/sda2                                                    |
        | 主分区 3   | /dev/sda3                                                    |
        | 扩展分区   | /dev/sda4                                                    |
     | 逻辑分区 1 | /dev/sda5                                                    |
        | 逻辑分区 2 | /dev/sda6                                                    |
   
   - 分区方式 2
   
     1. 如图划分
   
        ![Linux分区表示法2](git_picture/Linux分区表示法2.png)
   
     2. 分区设备文件名__（逻辑分区名是从 5 开始的）__
   
        | 分区      | 分区设备文件名 |
        | --------- | -------------- |
        | 主分区 1  | /dev/sda1      |
        | 扩展分区  | /dev/sda2      |
        | 逻辑分区1 | /dev/sda5      |
        | 逻辑分区3 | /dev/sda6      |

#### 联系

1. 硬盘的使用
   - 需要对硬盘进行分区
   - 对硬盘进行格式化，格式化的目的是写入文件系统

#### 文件系统（ext 系列）

1. 文件系统介绍
   - __ext2__：是 ext 文件系统的升级版，Red Hat Linux 7.2 版本以前的系统默认是 ext2 文件系统。1993 年发布，最大支持 16TB 的分区和最大 2TB 文件（一个分区不能大于 16TB，单个文件不能大于 2TB）
   - __ext3__：ext3 文件系统是 ext2 文件系统的升级版，__最大的区别就是带有日志功能__，以在系统突然停止时提高文件系统的可靠性。最大支持 16TB 的分区和最大 2TB 文件
   - __ext4__：它是 ext3 文件系统的升级版。ext4 在性能上、伸缩性和可靠性方面进行了大量改进。ext4 的变化可以说是翻天覆地的，比如向下兼容 ext3、最大 1EB 文件系统（分区）和 16TB 文件、无限数量子目录、Extents 连续数据概念、多块分配、延迟分配、持久预分配、快速 FSCK、日志校验、无日志模式、在线碎片整理、inode 增强、默认启用 barrier 等。

### 文件系统常用命令

#### df 命令、du 命令、fsck 命令和 dump2fs 命令

##### df 命令

1. 文件系统查看命令 df

   - 介绍

     1. 统计文件系统占用情况，分区使用情况等

   - 命令格式

     1. `df [选项] [挂载点]`
     2. 选项
        - -a：显示所有文件系统信息，包括特殊文件系统，如 `/proc` 、`/sysfs`
        - -h：使用习惯单位显示容量，如 KB、MB、GB等
        - -T：显示文件系统类型
        - -m：以 MB 为单位显示容量
        - -k：以 KB 为单位显示容量，默认是 KB

   - 实例

     1. 测试 `df` 命令 ，显示系统中所有分区（省去了很多），默认大小按照 KB 统计（不直观）

        ```shell
        ss@localcomputer:~$ df
        文件系统          1K-块    已用    可用 已用% 挂载点
        udev             979180       0  979180    0% /dev
        /dev/sda5      11922592 5483680 5813556   49% /
        /dev/sda3       3779640  387272 3180656   11% /home
        /dev/sda1       3778616   89512 3477444    3% /boot
        ```

     2. 测试 `df -h`，人性化显示所有分区使用情况

        ```shell
        ss@localcomputer:~$ df -h
        文件系统        容量  已用  可用 已用% 挂载点
        udev            957M     0  957M    0% /dev
        /dev/sda5        12G  5.3G  5.6G   49% /
        /dev/loop11      90M   90M     0  100% /snap/core/8268
        /dev/sda3       3.7G  379M  3.1G   11% /home
        /dev/sda1       3.7G   88M  3.4G    3% /boot
        ```

     3. 测试指定挂载（分区）使用情况，命令 `df -h 挂载点`

        ```shell
        ss@localcomputer:~$ df -h /
        文件系统        容量  已用  可用 已用% 挂载点
        /dev/sda5        12G  5.3G  5.6G   49% /
        ss@localcomputer:~$ df -h /home
        文件系统        容量  已用  可用 已用% 挂载点
        /dev/sda3       3.7G  379M  3.1G   11% /home
        ```

##### du 命令

1. 统计目录或文件大小命令 du

   - 介绍

     1. 统计文件或目录大小的命令
     2. 一般不使用其统计

   - 命令格式

     1. `du [选项] [目录或文件名]`

     2. 选项

        - -a：显示每个子文件的磁盘占用量。默认只统计子目录的磁盘占用量
        - -h：使用习惯单位显示磁盘占用量，如 KB、MB 或 GB
        - -s：统计总占有量，而不列出子目录和子文件的占有量

     3. 与 `ls -lh` 在查看 __目录__ 大小的区别

        - 命令 `ls -lh` 只会统计目录下的一级子目录、子文件的文件名大小

          ```shell
          ss@localcomputer:~$ ls -lh /
          总用量 88K
          drwxr-xr-x   2 root root 4.0K 12月  4 19:47 bin		# /bin 大小只能 4.0K（实际是一级文件名，目录名的大小）
          drwxr-xr-x   4 root root 4.0K 12月  4 14:57 boot
          ```

   - 实例

     1. 创建目录，`tree 目录` 命令查看目录构造

        ```shell
        ss@localcomputer:~$ tree test/
        test/
        ├── a
        ├── b
        └── tc
            ├── a
            ├── b
            └── t
                └── a
        
        2 directories, 5 files			# 5 个文件，2 个目录
        ```

     2. 命令 `du test/` ，只能查看目录中的目录

        ```shell
        ss@localcomputer:~$ du test/
        8	test/tc/t
        20	test/tc
        32	test/
        ```

     3. 命令 `du -a test` ,查看包括文件在内

        ```shell
        ss@localcomputer:~$ du -a test/
        4	test/tc/t/a
        8	test/tc/t
        4	test/tc/a
        4	test/tc/b
        20	test/tc
        4	test/a
        4	test/b
        32	test/				# 总量
        ```

     4. 命令 `du -ah test/`，`-h` 使用习惯单位

        ```shell
        ss@localcomputer:~$ du -ah test/
        4.0K	test/tc/t/a
        8.0K	test/tc/t
        4.0K	test/tc/a
        4.0K	test/tc/b
        20K	test/tc
        4.0K	test/a
        4.0K	test/b
        32K	test/
        ```

     5. 命令 `du -s test/`，目录总量

        ```shell
        ss@localcomputer:~$ du -s test/
        32	test/
        ```

     6. 查看文件 `du -ah 文件`

        ```shell
        ss@localcomputer:~/test$ du -h a
        4.0K	a
        ```

   - `df` 命令与 `du` 命令的区别

     1. 使用 `df -h /` 查看挂载点，根挂载点（已使用 5.3G）

        ```shell
        root@localcomputer:~# df -h /
        文件系统        容量  已用  可用 已用% 挂载点
        /dev/sda5        12G  5.3G  5.6G   49% /
        ```

     2. 使用 `du -sh /` 查看根目录的使用情况（已使用 8.9G）

        ```shell
        root@localcomputer:~# du -sh /
        du: 无法访问'/proc/7570/task/7570/fd/4': 没有那个文件或目录
        du: 无法访问'/proc/7570/task/7570/fdinfo/4': 没有那个文件或目录
        du: 无法访问'/proc/7570/fd/3': 没有那个文件或目录
        du: 无法访问'/proc/7570/fdinfo/3': 没有那个文件或目录
        du: 无法访问'/run/user/1000/gvfs': 权限不够
        8.9G	/
        ```

##### du 与 df 不同之处

1. 解释不同之处（self 理解）
   - `df` 命令是查询挂载点（分区的大小），根目录 `/` 只是一个挂载点
   - `du` 命令是查询根目录下的文件内容，包括其他挂载点，__而且消耗资源__
   - 就是 `du` 命令查询 根目录`/` 下所有文件大小，不区分不同的分区，而 `df` 查询是在 根目录 `/` 下的一个分区的大小，像 `/boot` 挂载点、`/home` 挂载点虽然也是根目录 `/` 的文件，但是 `df` 不查询（`du` 命令查询）
2. 官方解释
   - `df` 命令是从文件系统考虑的，不光要考虑文件占用的空间，还要统计被命令或程序占用的空间，进程空间（最常见的就是文件以删除，但是程序并没有释放空间），__所以 `df` 查看的可用空间才是真正的可以使用空间__
   - `du` 命令是面向文件的，只会计算文件或目录占用的空间，__所以 `du` 查看文件大小是准确的__
   - __假如：`df` 查询的文件大小比 `du` 查询的文件大，也不用惊讶，因为 `df` 命令查询分区大小使用情况中包含进程等一些占用资源信息。__
   - __但在我的 Ubuntu 系统中，`du` 命令查询的文件大__

##### fsck 命令（知道就好）

1. 文件系统修复命令 fasck
   - 介绍
     1. 如果出现文件系统出现异常可以尝试使用 `fsck` 命令修复
   - 命令格式
     1. `fsck [选项] 分区设备文件名`
     2. 选项
        - -a：不用显示用户提示，自动修复文件系统
        - -y：自动修复，和 -a 作用一致，__不过有些文件系统只支持 -y__
   - 注意
     1. 一般不需要手动执行，系统底层自动执行（有时没有执行还好，要是执行，则会报错）

##### dmpe2fs 命令

1. 显示磁盘状态命令 dumpe2fs

   - 命令格式 

     1. `dumpe2fs 分区设备文件名`

   - 注意

     1. 其实命令简单，但是命令输出内容大
     2. 硬盘要经过分区-》格式化后才可以使用，其中格式化实际将分区再进行划分，分成更小的 Block（一般大小 1KB、4KB）和写入文件系统
     3. Block（数据库）：一个数据库只能存放一个文件信息。例如一个文件 10KB，一个数据块 4 KB，存下此文件需要 3 个数据块，其中存放文件剩余 2KB 数据的第 3 个数据块不能再存放其他数据

   - 实例

     1. 命令 `dumpe2fs /dev/sda1` 查看第一块分区

        ```shell
        # 超级快信息（删除了一些信息）
        Filesystem volume name:   <none>	# 卷标
        Last mounted on:          /boot		# 挂载点
        Filesystem UUID:          42816a33-90c5-424a-bfb0-a936ee60c0a2	# 分区唯一识别符（UUID）
        Default mount options:    user_xattr acl		# 默认挂载选项
        Filesystem state:         clean
        Errors behavior:          Continue
        Filesystem OS type:       Linux
        Inode count:              244320		# 分区共有多少 inode 节点数
        Block count:              976384		# 默认有多少数据块
        Reserved block count:     48819
        Free blocks:              922276
        Free inodes:              244008
        First block:              0
        Block size:               4096			# 数据块大小，对 boot 分区默认是 4 KB 
        Flex block group size:    16
        Filesystem created:       Wed Dec  4 14:14:14 2019
        Last mount time:          Sun Feb  2 17:51:18 2020
        Last write time:          Sun Feb  2 17:51:18 2020
        Mount count:              99
        First inode:              11
        Inode size:	          256		# inode 大小
        
        # 数据块省略
        ```


#### 挂载命令

##### 查询

1. 查询与自动挂载
   - __查询系统中已挂载的设备，-l 会显示卷标名称__
     1. `mount [-l]` ，`-l` 可加可不加
   - __依据配置文件 `/etc/fsatb` 的内容，自动挂载__ 
     1. `mount -a`
     2. 光盘、U盘、移动硬盘不能做成自动挂载，因为不能保证每次开机这些设备都会有
   - 挂载解释
     1. Linux 每一个硬件都有一个设备文件名，例如光盘、U盘、软盘等都会有一个设备文件名，同时这些设备都会有一个挂载点（盘符），__挂载：就是将设备文件名和挂载点（盘符）联系起来，才能通过盘符访问硬件设备__

##### 挂载格式

1. 挂载命令格式

   - 命令 `mount [-t 文件系统] [-L 卷标名] [-o 特殊选项] 设备文件名 挂载点`

   - 选项

     1. -t 文件系统：加入文件系统类型来指定挂载的类型，可以是 ext3、ext4（硬盘默认 ext4）、iso9660（光驱默认） 等文件系统

     2. -L 卷标名：挂载指定卷标的分区，而不是按照设备文件名挂载

     3. -o 特殊选项：可以指定挂载的额外选项，针对分区__（有时候可执行文件不能执行，并不一定是权限问题，也有可能是文件系统的特殊选项没有给定）__

        | 参数            | 说明                                                         |
        | --------------- | ------------------------------------------------------------ |
        | `atime/noatime` | 更新访问时间/不更新访问时间。访问分区文件时，是否更新文件的访问时间，默认为更新 |
        | `async/sync`    | 异步/同步，默认是异步                                        |
        | `default`       | 自动/手动，`mount -a` 命令执行时，是否会自动安装 `/etc/fstab` 文件内容挂载，默认为自动 |
        | `exec/noexec`   | 执行/不执行，设定是否在文件系统中（分区中）执行可执行文件，默认是 exec 允许 |
        | `remount`       | __重新挂载已经挂载的文件系统，一般用于指定修改特殊权限__     |
        | `rw/ro`         | 读写/只读，文件系统挂载时，是否具有读写权限，默认是 rw       |
        | `suid/nosuid`   | 具有/不具有SUID 权限，设定文件是否具有 SUID 权限和 SGID 权限，默认是具有的 |
        | `user/nouser`   | 允许/不允许普通用户挂载，设定文件系统是否允许普通用户挂载，默认是不允许普通用户挂载，只有 root 可以挂载分区 |
        | `usrquota`      | 写入代表文件系统支持用户磁盘配额，默认不支持                 |
        | `grpquota`      | 写入代表文件系统支持组磁盘配额，默认不支持                   |

   - 实例
   
     1. 创建 shell 脚本 `#!/bin/bash echo "hello world"` （输出 hello world）
   
        ```shell
        root@localcomputer:/home/ss# vim hello.sh		# 创建 shell 脚本
        root@localcomputer:/home/ss# ls -ld hello.sh 
        -rw-r--r-- 1 root root 31 2月   3 15:13 hello.sh
        root@localcomputer:/home/ss# chmod 755 hello.sh 	# 赋予执行权限
        root@localcomputer:/home/ss# ./hello.sh 		# 执行 shell 脚本
        hello world		# 输出 hello world
        ```
   
     2. 重新挂载分区（/home 挂载点），__并使用 noecex 权限__ `mount -o remount,noexec /home` (最好不要改根分区，会出现问题)
   
        ```shell
        root@localcomputer:/home/ss# mount -o remount,noexec /home		# 重新挂载，设定 noexec选项
        root@localcomputer:/home/ss# ./hello.sh		# 执行 shell 脚本
        -su: ./hello.sh: 权限不够
        root@localcomputer:/home/ss# ls -ld hello.sh 
        -rwxr-xr-x 1 root root 31 2月   3 15:13 hello.sh
        root@localcomputer:/home/ss# mount -o remount,exec /home		# 重新挂载，设定 exec 选项
        root@localcomputer:/home/ss# ls -ld hello.sh 
        -rwxr-xr-x 1 root root 31 2月   3 15:13 hello.sh
        root@localcomputer:/home/ss# ./hello.sh 		# 执行脚本
        hello world
        ```
   

#### 挂载光盘与 U 盘

##### 挂载光盘

1. 挂载光盘

   - 建立挂载点（现在一般都会自动进行创建 `/media/ss` 挂载点）

     1. `mkdir /media/ss`

   - 挂载光盘（自动挂载）`mount -t iso9660 设备文件名 挂载点`

     1. `mount -t iso9660 /dev/chrom /media/ss` 
     2. `mount /dev/sr0 /media/ss` 
     3. 这是第一个光盘其设备文件名为（chrom、sr0），第二个光盘为（chrom1，rs1）等

   - 注意

     1. 两个设备文件名 `/dev/cdrom` 和 `/dev/sr0` 都是光盘文件设备名

        ```shell
        root@localcomputer:/dev# ls -ld /dev/cdrom
        lrwxrwxrwx 1 root root 3 2月   3 18:04 /dev/cdrom -> sr0
        ```

     2. 实际 `/dev/cdrom` 是 `/dev/sr0` 的软链接

2. 卸载光盘

   - 命令 

     1. `umount 设备文件名或挂载点`

   - 实例

     1. 卸载光盘

        ```shell
        root@localcomputer:/dev# umount /dev/sr0
        ```

3. 注意

   - 挂载点好像必须是空目录，但是 Ubuntu 的桌面版，光盘和 U 盘是自动挂载，且‘ 挂载点都是 `/media/ss/` 中

##### 挂载 U盘

1. 挂载 U 盘

   - 注意
   
1. U 盘和硬盘采用相同的命令规则，第一块硬盘为 sda，分区为 sda1、sda2；U 盘则是第二块硬盘 sdb，分区为 sdb1（自动识别）。
   
- 查看 U 盘设备文件名
  
  1. `fdisk -l` （关键信息）
  
        ```shell
        # 硬盘主要信息（第一块硬盘）
        Disk /dev/sda：20 GiB，21474836480 字节，41943040 个扇区
        单元：扇区 / 1 * 512 = 512 字节
        扇区大小(逻辑/物理)：512 字节 / 512 字节
        I/O 大小(最小/最佳)：512 字节 / 512 字节
        磁盘标签类型：dos
        磁盘标识符：0x591cf093
        	# 分区主要信息
        设备       启动     起点     末尾     扇区  大小 Id 类型
        /dev/sda1  *        2048  7813119  7811072  3.7G 83 Linux
        /dev/sda2        7813120  9766911  1953792  954M 82 Linux swap / Solaris
        /dev/sda3        9766912 17580031  7813120  3.7G 83 Linux
        /dev/sda4       17582078 41940991 24358914 11.6G  5 扩展
        /dev/sda5       17582080 41940991 24358912 11.6G 83 Linux
        
        # U盘主要信息（第二块硬盘）
        Disk /dev/sdb：29.8 GiB，32018268160 字节，62535680 个扇区
        单元：扇区 / 1 * 512 = 512 字节
        扇区大小(逻辑/物理)：512 字节 / 512 字节
        I/O 大小(最小/最佳)：512 字节 / 512 字节
        磁盘标签类型：dos
        磁盘标识符：0x486407cf
        	# 分区主要信息
        设备       启动    起点     末尾     扇区  大小 Id 类型
        /dev/sdb1  *    1593344 62535679 60942336 29.1G  7 HPFS/NTFS/exFAT
     ```
  
- 挂载 U 盘
  
     1. `mount -t vfat /dev/sdb1 /media/ss` 
     2. 文件系统 `vfat` 是 fat32 分区，`fat` 是 fat16分区（不懂）
     3. `mount /dev.sdb1 /media/ss` 如 U 盘的文件系统是 NTFS，不需要指定文件系统
     4. 这个挂载点和光盘的挂载点相同，__两个都是自动挂载__，之后访问挂载点即可
     5. __注意：Linux 默认不支持 NTFS 文件系统（现在好像是支持了）__

#### 支持 NTFS 文件系统

##### 驱动程序（device driver）

1. 解释
   - 操作系统与硬件之间的桥梁，协调两者之间的关系
2. 作用
   - 第一：将硬件本身的功能告诉操作系统
   - 第二：主要功能是将硬件设备 __电子信号__与操作系统及软件的 __高级编程语言__ 之间的互相 __转换__
   - 如播放音乐时，音乐播放器需使用声卡，操作系统控制器会向声卡驱动器发送指令，然后声卡驱动器将操作系统指令转换为声卡可以识别的电子信号

##### Linux 与 Win 在驱动方面的区别

1. Win 的驱动程序需要手动安装
   - Win 系统安装完成之后，需要手动安装所有硬件驱动，才可以正常使用
2. Linux 的驱动程序是在内核中，一般不需要手动安装。如需安装驱动程序，需要重新编译 Linux 内核
   - Linux 系统可以自动识别大部分硬件，因为 Linux 内核中包含了市面上的大部分驱动程序，系统会为硬件选择合适的驱动程序（显卡驱动好像就没有）
   - 需要手动安装驱动程序
     1. 驱动程序出现过晚，Linux 内核发布早于驱动程序，想使用驱动程序自然需要手动安装
     2. Linux 内核默认没有安装某种驱动程序
     3. 注意：手动安装驱动程序是给内核使用的，__所以需要重新编译内核__

##### 解决 Linux 内核默认没有的驱动程序

1. 使用第三方软甲
2. Linux 有的不支持 NTFS 文件系统（早期 Linux 内核），NTFS文件系统是驱动程序，在不重新编译内核的情况下，使用第三方工具 __NTFS-3G 插件__ ，可以解决 Linux不支持 NTFS文件系统
3. 安装 NTFS-3G
4. 使用 NTFS-3G
   - 命令 `mount -t ntfs-3g 分区设备文件名 挂载点` ，其中文件系统是此软件自东安装的文件系统

### fdisk 分区

#### fdisk 命令分区过程

1. 添加新硬盘

   - 虚拟机的好处是，只要真是机的硬盘空间足够大，添加几块硬盘都可以
   - 将虚拟机断电，才可以添加硬盘，否则不能添加
   - 设置虚拟机-->选择添加硬盘，之后默认即可。__需要注意的是，给虚拟机添加的硬盘大小时，不需要考虑真实机剩余多少，虚拟 Linux 实际使用多少才会占用多少__

2. 查看新硬盘

   - 命令 `fdisk -l` ，和查询 U 盘的命令一样

     1. __此命令就是查询系统中到底有多少硬盘、U 盘、软盘被识别__

   - 查看硬盘是否被识别

     1. 命令 `fdisk -l` ，硬盘 `sdb` 为系统第二块硬盘，但是并没有分区

        ```shell
        Disk /dev/sdb：5 GiB，5368709120 字节，10485760 个扇区
        单元：扇区 / 1 * 512 = 512 字节
        扇区大小(逻辑/物理)：512 字节 / 512 字节
        I/O 大小(最小/最佳)：512 字节 / 512 字节
        ```

   - 命令输出信息解释

     1. 输出硬盘分区信息

        ```shell
        	# 分区主要信息
        设备       启动     起点     末尾     扇区  大小 Id 类型
        /dev/sda1  *        2048  7813119  7811072  3.7G 83 Linux
        /dev/sda2        7813120  9766911  1953792  954M 82 Linux swap / Solaris
        /dev/sda3        9766912 17580031  7813120  3.7G 83 Linux
        /dev/sda4       17582078 41940991 24358914 11.6G  5 扩展
        /dev/sda5       17582080 41940991 24358912 11.6G 83 Linux
        ```

     2. Id 介绍

        - 硬盘标志分区 Id 为 83
        - swap 交换分区 Id 为 82
        - 扩展分区 Id 为 5

3. 使用 `fdisk` 命令分区

   - 命令 `fdisk /dev/sdb` ，sdb 为 第二块硬盘（b 表示第二块整个硬盘，并没有进行分区）

     1. 使用 `fdisk /dev/sdb` ，进入命令交互模式__（输入 m 提示帮助信息）__

        ```shell
        root@localcomputer:~# fdisk /dev/sdb
        
        欢迎使用 fdisk (util-linux 2.31.1)。
        更改将停留在内存中，直到您决定将更改写入磁盘。
        使用写入命令前请三思。
        
        设备不包含可识别的分区表。
        创建了一个磁盘标识符为 0x8137661f 的新 DOS 磁盘标签。
        
        命令(输入 m 获取帮助)： 		# 输入 m 获取帮助信息
        ```

     2. 帮助信息（交互指令说明），重要几个加粗

        | 命令  | 说明                                                         |
        | ----- | ------------------------------------------------------------ |
        | a     | 设置可引导标记                                               |
        | b     | 编辑 bsd 磁盘标签                                            |
        | c     | 设置 DOS 操作系统兼容标记                                    |
        | __d__ | __删除一个分区__                                             |
        | __l__ | __显示已知的文件系统类型。82 为 Linux swap 分区，83 为 Linux 标准分区__ |
        | __m__ | __显示帮助菜单__                                             |
        | __n__ | __新建分区__                                                 |
        | o     | 建立空白 DOS 分区表                                          |
        | __p__ | __显示分区列表__                                             |
        | __q__ | __不保存退出__                                               |
        | s     | 建立空白 SUN 磁盘标签                                        |
        | __t__ | __改变一个分区的系统 ID__ （改变的就是 82、83、5）           |
        | u     | 改变显示记录单位                                             |
        | v     | 验证分区表                                                   |
        | __w__ | __保存退出__                                                 |
        | x     | 附加功能（仅限专家）                                         |
     
   - 交互模式的命令介绍
   
     1. `l` 显示系统支持的分区 Id 号。最大识别到 `ff` 、83 标准分区等等
   
     2. `n` 创建主分区__（主分区最多有 4 个，扩展分区 + 主分区最多有 4 个，扩展分区最多有 1 个）__
   
        ```shell 
        命令(输入 m 获取帮助)： n		# 创建分区
        分区类型
           p   主分区 (0个主分区，0个扩展分区，4空闲)		# 主分区号（1-4）
           e   扩展分区 (逻辑分区容器)		# 扩展分区（逻辑分区的容器）
        选择 (默认 p)： p	# 默认建立主分区
        分区号 (1-4, 默认  1): 1		# 输入分区号（1），最好顺序选取分区号
        第一个扇区 (2048-10485759, 默认 2048): 2048		# 第一个分区从哪个扇区开始，最好从起始位置
        上个扇区，+sectors 或 +size{K,M,G,T,P} (2048-10485759, 默认 10485759): +1G	# 到哪个扇区结束，也可以使用 {K\M\G\T\P} 习惯用法表示分区的大小
        
        创建了一个新分区 1，类型为“Linux”，大小为 1 GiB。		# 创建第一个分区，分区号为 1，分区大小为 1G
        
        命令(输入 m 获取帮助)： p	# 查看分区
        Disk /dev/sdb：5 GiB，5368709120 字节，10485760 个扇区
        单元：扇区 / 1 * 512 = 512 字节
        扇区大小(逻辑/物理)：512 字节 / 512 字节
        I/O 大小(最小/最佳)：512 字节 / 512 字节
        磁盘标签类型：dos
        磁盘标识符：0xe33ec0d5
        
        设备       启动  起点    末尾    扇区 大小 Id 类型
        /dev/sdb1        2048 2099199 2097152   1G 83 Linux		# 显示分区信息，文件大小、文件类型……
        
        命令(输入 m 获取帮助)： 
        ```
   
     3. 创建扩展分区
   
        ```shell
        命令(输入 m 获取帮助)： n
        分区类型
           p   主分区 (1个主分区，0个扩展分区，3空闲)
           e   扩展分区 (逻辑分区容器)
        选择 (默认 p)： e		# 创建扩展分区
        分区号 (2-4, 默认  2): 2		# 第二个分区
        第一个扇区 (2099200-10485759, 默认 2099200): 		# 起始位置采用默认
        上个扇区，+sectors 或 +size{K,M,G,T,P} (2099200-10485759, 默认 10485759): 	# 终止位置采用默认（剩余空间全部分配）
        
        创建了一个新分区 2，类型为“Extended”，大小为 4 GiB。		# 剩余空间 4G 大小
        
        命令(输入 m 获取帮助)： p
        Disk /dev/sdb：5 GiB，5368709120 字节，10485760 个扇区
        单元：扇区 / 1 * 512 = 512 字节
        扇区大小(逻辑/物理)：512 字节 / 512 字节
        I/O 大小(最小/最佳)：512 字节 / 512 字节
        磁盘标签类型：dos
        磁盘标识符：0xe33ec0d5
        
        设备       启动    起点     末尾    扇区 大小 Id 类型
        /dev/sdb1          2048  2099199 2097152   1G 83 Linux
        /dev/sdb2       2099200 10485759 8386560   4G  5 扩展		# 扩展分区，Id 为 5
        ```
   
     4. 在扩展分区中创建逻辑分区（只能从 5 开始给逻辑分区号，并且自动）
   
        ```shell
        命令(输入 m 获取帮助)： n
        所有主分区的空间都在使用中。
        添加逻辑分区 5		# 分区号 5
        第一个扇区 (2101248-10485759, 默认 2101248): 
        上个扇区，+sectors 或 +size{K,M,G,T,P} (2101248-10485759, 默认 10485759): +1G	# 空间 1G
        
        创建了一个新分区 5，类型为“Linux”，大小为 1 GiB。
        
        命令(输入 m 获取帮助)： p
        Disk /dev/sdb：5 GiB，5368709120 字节，10485760 个扇区
        单元：扇区 / 1 * 512 = 512 字节
        扇区大小(逻辑/物理)：512 字节 / 512 字节
        I/O 大小(最小/最佳)：512 字节 / 512 字节
        磁盘标签类型：dos
        磁盘标识符：0xe33ec0d5
        
        设备       启动    起点     末尾    扇区 大小 Id 类型
        /dev/sdb1          2048  2099199 2097152   1G 83 Linux
        /dev/sdb2       2099200 10485759 8386560   4G  5 扩展
        /dev/sdb5       2101248  4198399 2097152   1G 83 Linux		# sdb5 就表示逻辑分区（1-4 不允许给逻辑分区）
        ```
   
     5. `w` 保存退出
   
        ```shell
        命令(输入 m 获取帮助)： w
        分区表已调整。
        将调用 ioctl() 来重新读分区表。
        正在同步磁盘。
        ```
   
   - 保存退出
   
     1. 当分区表没用被使用、占用时，可以使用 `w` 保存退出
   
     2. 当分区表被使用、占用时，使用 `w` 保存退出，会提示重启 Linux 才可以进行下一步
   
     3. 重启 Linux 麻烦，__可以使用 `partprobe` 重新读取分区表信息，此命令建议分区完成时执行一次__
   
        ```shell
        root@localcomputer:~# partprobe
        root@localcomputer:~# 
        ```

#### 格式化分区

1. 格式化分区

   - 介绍：分区完成之后，分区还不可使用，还必须进行格式化__（写入文件系统，对分区在进行划分的过程）__，即把数据块的大小，划分成指针的大小，一般是 4KB 大小
   - 命令 `mkfs -t ext4 /dev/sdb1` ，注意格式化的是 __分区__
     1. `-t` 后面接文件系统类型
     2. 设备文件名，格式化为 ext4 文件系统类型
   - __注意：扩展分区既不能写入数据也不能格式化__

2. 实例格式化

   - 使用 `fdisk` 查看硬盘 sdb 的分区情况（格式化的是分区，扩展分区不能进行格式化）

     ```shell
     Disk /dev/sdb：5 GiB，5368709120 字节，10485760 个扇区
     单元：扇区 / 1 * 512 = 512 字节
     扇区大小(逻辑/物理)：512 字节 / 512 字节
     I/O 大小(最小/最佳)：512 字节 / 512 字节
     磁盘标签类型：dos
     磁盘标识符：0xe33ec0d5
     
     设备       启动    起点     末尾    扇区 大小 Id 类型
     /dev/sdb1          2048  2099199 2097152   1G 83 Linux
     /dev/sdb2       2099200 10485759 8386560   4G  5 扩展
     /dev/sdb5       2101248  4198399 2097152   1G 83 Linux
     /dev/sdb6       4200448 10485759 6285312   3G 83 Linux
     ```

   - 格式化 `/dev/sdb1` 分区 `mkfs -t ext4 /dev/sdb1`

     ```shell
     root@localcomputer:~# mkfs -t ext4 /dev/sdb1
     mke2fs 1.44.1 (24-Mar-2018)
     创建含有 262144 个块（每块 4k）和 65536 个inode的文件系统
     文件系统UUID：b474c03f-2fbb-4ef0-8855-6d4cf7749160
     超级块的备份存储于下列块： 
     	32768, 98304, 163840, 229376
     
     正在分配组表： 完成                            
     正在写入inode表： 完成                            
     创建日志（8192 个块） 完成
     写入超级块和文件系统账户统计信息： 已完成
     ```

   - 不能格式化 `/dev/sdb2` ，因为它是扩展分区

     ```shell
     root@localcomputer:~# mkfs -t ext4 /dev/sdb2
     mke2fs 1.44.1 (24-Mar-2018)
     在 dos 中发现一个 /dev/sdb2 分区表
     Proceed anyway? (y,N) N
     ```

#### 建立挂载点并挂载分区

1. 创建挂载点（创建一个空目录）

   - 命令 `mkdir /disk1`

2. 挂载分分区

   - 命令 `mount /dev/sdb1 /dsik1`

     ```shell
     root@localcomputer:~# df
     文件系统          1K-块    已用    可用 已用% 挂载点
     /dev/sdb1        999320    2564  927944    1% /root/disk1
     ```

3. 查看是否被正常挂载 

   - 使用 `mount` 命令 
   - 使用 `df` 命名
   - 命令 `fdsik` 只能查看分区是否被正常分配，而不是查看分区是否被 Linux 挂载

4. __注意__

   - __每次系统重启分区挂载点都会丢失，所以每次重启都需要重新挂载分区__
   - 需实现分区的自动挂载

#### 分区自动挂载

##### 分区自动挂载

1. 介绍

   - 使用 `mount` 命令进行挂载的分区，每次系统重启，就必须再次进行挂载
   - 将挂载内容写入配置文件 `/etc/fstab` ，则会保证下次重启自动挂组分区

2. `/etc/fstab` 文件介绍

   - 文件字段介绍

     | 字段     | 说明                                                         | 查看内容命令                                                 |
     | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
     | 第一字段 | 分区设备文件名或 UUID（硬盘通用唯一识别码）                  | `dumpe2fs -h /dev/sdb1` <br> 只显示超级快信息<br>字段为 `Filesystem UUID` |
     | 第二字段 | 挂载点                                                       |                                                              |
     | 第三字段 | 文件系统名称                                                 | 格式化什么文件系统，写入哪种系统（ext4）                     |
     | 第四字段 | 挂在参数                                                     | defaule： 为分区挂载使用默认权限，和 `mount` 命令挂载表格的权限是通用的 <br>例子：`gid=5,mode=620` |
     | 第五字段 | 指定分区是否被 dump 备份（0 代表不备份、1 代表每天备份、2 代表不定期备份） | 每个分区备份的目录是 `lost+found` <br>备份是针对分区而言的   |
     | 第六字段 | 指定分区是否被 fsck 检测（0 代表不检测，其他数字代表检测优先级，1 的优先级比 2 高） | `fsck` 检测命令是底层在适当的时机进行自动检测的。谁优先级最高谁优先扫描。<br>根 `/` 分区优先级最高，则最先扫描，再去扫描其他分区。<br>手动添加的分区，优先级都不应该超过 1 |

   - 文件内容介绍 ，__`/etc/fstab` 为系统启动的重要文件，千万不能写错，错系统无法启动 __

     ```shell
     # <file system> <mount point>   <type>  <options>       <dump>  <pass>
     # / was on /dev/sda5 during installation
     UUID=f4f8a8cd-3279-4366-8e69-5f7845a78446 /               ext4    errors=remount-ro 0       1
     # /boot was on /dev/sda1 during installation
     UUID=42816a33-90c5-424a-bfb0-a936ee60c0a2 /boot           ext4    defaults        0       2
     # /home was on /dev/sda3 during installation
     UUID=a6fc133f-962c-443f-af83-334a0d028421 /home           ext4    defaults        0       2
     # swap was on /dev/sda2 during installation
     UUID=73c2fb14-9e4d-41d4-8245-f2eaf277499b none            swap    sw              0       0
     ```

     1. 第一字段，实际可以使用 __UUID__（早期没有 UUID，universal unique identification） 和 __设备文件名__ 都是可以的。设备文件名有一个问题：__分区设备颠倒，添加新的分区，会造成系统崩溃__。UUID 是给分区分配一个唯一标识码，和分区设备名无关，__UUID 更为可靠__。

3. 依据 `/etc/fstab` 配置文件，自动挂载分区

   - 必要性介绍
     1. `/etc/fsatb` 配置文件时系统启动的重要文件，不允许出现错误（错误会导致系统崩溃）
        - 错误：挂载点误删除
        - 错误：配置文件格式错误
     2. 使用 `mount -a` 在系统重启之前发现错误

   - 命令 `mount -a` 
     1. 依据配置文件 `etc/fstab` 的内容，自动进行挂载。如果出现配置错误，则可以提前发现，就地修改，不至于重启时出错。

### /etc/fstab 文件修复

##### 解决因 `/etc/fstab` 文件错误，而导致重启错误问题

1. `/etc/fstab` 文件报错
   - 会提示重新启动或 __输入密码__
   - 输入密码，进入操作界面
2. 修改 `etc/fstab` 文件
   - 在 `/etc/fstab` 文件错误，导致重启错误的情况，__`/etc/fstab` 文件所在的分区（也好像是 `/` 挂载点）是只读挂载__，所以即使是 root 也无权修改 `/etc/fstab` 配置文件
   - 我的 `/etc/fstab` 文件在 `/` 挂载点上的分区上
   - __只要将挂载点 `/` ，重新挂载成读写即可 `mount -o remont,rw /` ，就可以修改 `/etc/fstab` 配置文件__
3. __注意__
   - 此方法只能解决由 `/etc/fstab` 错误引发的重启错误
   - 挂载点 `/` (挂载的分区) 不能出错，否则系统直接崩溃（不给机会）

##### `etc/fstab` 配置文件注意

1. 只有写入 `etc/fstab` 配置文件的分区设置，系统启动时才自动进行挂载，否则重新挂载
2. 不管是 Linux 标准分区、swap 交换分区设置，都要写入 `etc/fstab` 配置文件，系统启动时自动生效，否则重新挂载
3. 写入 `etc/fstab` 配置文件的分区设置后，不管是 __挂载点、分区设备文件名等等__ 尽量不要更改（与配置文件保持一致）。
4. 每次修改 `etc/fstab` 配置文件后，最好执行 `mount -a` 命令，此命令是按照 `/etc/fstab` 配置内容重新

### 分配 swap 分区

##### 介绍 swap 分区

1. 安装时介绍

   - 安装 Linux 系统时，必须在硬盘上划分两个分区：一个分区挂载在 `/` 根目录；一个分区作为 swap 交换分区使用
   - 没有分区，则是安装时默认（现在软件都很人性化）

2. 使用说明

   - CPU 处理的数据是来自内存，而不与硬盘直接打交道，所有数据都要经过内存来缓存，最后输出到硬盘中去。
   - __内存__ 往往容量有限，有些时候程序需要的数据要高于内存的容量，这自然就会造成计算机运行的速度很慢，__swap__ 相当于是用 __硬盘__来延伸内存的记录空间。
   - 如果硬件的配备足够的话，__swap__ 不会被用到，只有内存不足时才会将内存中暂时不用的数据年搬到 swap 中去，空出来的内存给程序使用。

3. 使用 `free` 命令查看内存和 swap状态

   - `free` 显示不友好，`free -h` 友好显示

     ```shell
     root@localcomputer:~# free -h
                   总计         已用        空闲      共享    缓冲/缓存    可用
     内存：        1.9G        1.1G        369M        156M        486M        552M
     交换：        1.9G        105M        1.8G
     ```

##### 划分 swap 交换分区

1. 使用 `fdisk /dev/sdb` 进入交互模式

2. 进入交互模式：输入 `p` 查看分区情况

3. 进入交互模式：输入 `t` 修改已经建立好的分区 Id，swap 的 Id 为 82

4. 进入交互模式：输入 `w` 退出保存

5. 执行 `partprobe` ，重新读取分区表信息

   - 分区后 Linux 系统重启，而系统重启比较麻烦
   - 所以使用 `partprobe` 命令，重新读取分区信息

6. 执行格式化命令 `mkswap 设备文件名` 

   - `mkswap /dev/sdb1`

7. 启用 swap 分区 `swapon 设备文件名`

   - `swap /dev/sdb1`

8. 使用 `free -h` 查看内存和 swap使用情况

   - 实例

     ```shell
     ss@localcomputer:~$ free -h
                   总计         已用        空闲      共享    缓冲/缓存    可用
     内存：        1.9G        1.3G        121M         14M        531M        473M
     交换：        953M         11M        942M
     
     ```

9. 为使系统启动时自动启用 swap

   - __使用命令启动 swap 或使用 `mount` 挂载的设备，重启时必须重新挂载__
   - 系统启动时自动挂载或使用，就必须修改 `/etc/fstab` 配置文件

   - 修改 `/etc/fstab` 配置文件（UUID 或者是设备文件名 /dev/sdb1）

     ```shell
     # swap was on /dev/sda2 during installation
     UUID=73c2fb14-9e4d-41d4-8245-f2eaf277499b none            swap    sw              0       0	
     ```



## Vim 文编编辑器

__说明：Vim 是 Vi 的增强版，Ubuntu 默认安装了 Vi ，没有安装 Vim，但使用几乎一样。以下使用 Vim来介绍，最好使用 Vim，因为 Vi 有点难用__

### Vim 常用操作

#### vim 介绍

1. vim 是一个功能强大的全屏幕文本编辑器
2. 作用：建立、编辑、显示文本文件
3. vim 没有菜单，只有命令
4. vim 工作模式
   - 进入 `vi filename` ，已有文件，则进入文件命令模式；没有，则创建文件，再进入文件命令模式
   - 命令模式，
     1. 插入模式：使用 `i\a\o` 进入插入模式；使用 `esc` 回到命令模式
        - i 光标前插入
        - a 光标后插入
        - o 换行插入
     2. 编辑模式：命令以回车结束\运行
   - 退出 ，命令模式下输入命令 `wq` 退出并保存；`q!` 强制退出（如果：文件使用 `vi filename` 创建，退出没有保存，则文件也不会存在）；`w` 保存文件

#### 插入命令

1. 在命令模式下输入插入命令

   - 如表

     | 命令       | 作用                                 |
     | ---------- | ------------------------------------ |
     | a          | 光标后插入                           |
     | A          | 光标所在行末尾插入                   |
     | i          | 光标前插入                           |
     | I          | 光标所在行首插入                     |
     | o          | 光标下出入新行                       |
     | O          | 光标上出入新行                       |
     | ctrl + <n> | 自动补代码（只能补全文件中已出现的） |

#### 定位命令

1. 在命令模式下输入

   - 如表

     | 命令                     | 作用                      |
     | ------------------------ | ------------------------- |
     | :set number / set nu     | 设置行号                  |
     | :set nonumber / set nonu | 取消行号                  |
     | G / gg                   | 到最后一行 / 到第一行     |
     | n+G（n 为行数）          | 到第n行                   |
     | :n                       | 到第n行                   |
     | $                        | 移至行尾                  |
     | 0                        | 移至行首                  |
     | n+j / n+k                | 向下移动n行 / 向上移动n行 |

#### 删除命令

1. 在命令模式下输入

   - 如表

     | 命令      | 作用                           |
     | --------- | ------------------------------ |
     | x         | 删除光标所在处字符             |
     | nx        | 删除光标所在处后 n 个字符      |
     | dd        | 删除光标所在行，ndd 删除 n 行  |
     | dG        | 删除光标所在行及到文件末尾内容 |
     | D         | 删除光标所在处到行尾内容       |
     | :n_1,n_2d | 删除指定范围的行               |

#### 复制和剪切命令

1. 在命令模式下输入

   - 如表

     | 命令  | 作用                           |
     | ----- | ------------------------------ |
     | yy    | 复制当前行                     |
     | nyy   | 复制当前行一下的 n 行          |
     | dd    | 剪切当前行                     |
     | ndd   | 剪切当前行一下 n 行            |
     | p / P | 粘贴在当前光标行所在行下或行上 |

#### 替换和取消命令

1. 在命令模式输入

   - 如表

     | 命令 | 作用                                  |
     | ---- | ------------------------------------- |
     | r    | 取代光标所在字符                      |
     | R    | 从光标所在处开始替换字符，按 esc 结束 |
     | u    | 取消上一步操作                        |

#### 搜索和搜索替换命令

1. 在命令模式下输入

   - 如表

     | 命令                 | 作用                                                         |
     | -------------------- | ------------------------------------------------------------ |
     | /string <br> ?String | 搜索指定字符串 <br> 搜索时忽略大小写 :set ic <br> 键入 <n> 表示查看下一个搜索结果<br> shift + <n> 表示上一个结果 |
     | :%s/old/new/g        | 全文替换指定字符串（old） <br> 注意转义字符 `\/` 表示 `/`    |
     | :n_1,n_2s/old/new/g  | 在 n_1 到 n_2 行内替换 <br> 注意转义字符 `\/` 表示 `/`       |

#### 保存和退出命令

1. 在命令模式下输入

   - 表格

     | 命令            | 作用                                       |
     | --------------- | ------------------------------------------ |
     | :w              | 保存修改                                   |
     | :w new_filename | 另存为指定文件                             |
     | :wq             | 保存修改并退出                             |
     | ZZ              | 快捷键，保存修改并退出                     |
     | :q!             | 不保存修改退出                             |
     | :wq!            | 保存修改并退出（文件所有者及 root 可使用） |

### Vim 使用技巧

__说明：一下设置可以写入用户目录下的 `.vimrc` 文件写入配置信息，使用 vim 设置永久有效__

#### __导入命令执行结果 `:r !命令`__

1. 介绍
   - 将命令执行的结果写入，使用 vim 编辑器打开的文件
2. 实例
   - 使用 `cat 文件1` ，将 文件1导入 vim 打开的文件中
     1. `r !cat 文件1`
     2. __直接使用文件名 `:r !文件1` ，也可以导入文件1内容__ 
   - 导入当前时间 `date`
     1. `r !date`

#### __在 vim 中查看命令执行结果 `：！命令`__

1. 介绍

   - 不需要退出 vim 编辑器，直接使用 `:!命令` 查看执行结果

2. 实例

   - 在 vim 编辑器下，使用 `:!ls`

     ```shell
     ss@localcomputer:~/桌面/test$ vim a
     
     a  abc
     
     请按 ENTER 或其它命令继续
     ```

#### __定义快捷键 `:map 快捷键 触发命令`__

1. 介绍
   - 将一个或多个命令，使用一个快捷方式代替
2. 实例
   - 使用自定义快捷键注释代码
     1. 加注释：`:map ctrl+y I#<ESC>` 使用 `ctrl+y` 代替行首插入 `I` ,插入 #，按 ESC 键退出插入模式
     2. 解除：`:map ctrl+t 0x<ESC>` 使用 `ctrl+t` 代替光标移至行首，使用 `x` 删除光标所在处 #，按 ESC 键退出插入模式
   - 可是使用快捷键，输入邮箱等一些常用信息
     1. 插入邮箱：`:map ctrl+y iShenDeZ@163.com` 使用 `ctrl+y` 代替进入插入模式，插入邮箱地址

#### __连续注释 __

1. 介绍
   - 就是多行一起注释，会使用正则表达式
2. 实例
   - 以 # 为注释符的多行注释 
     1. `:n_1,n_2s/^/#/g` 	
     2. 相反 `:n_1,n_2s/^#//g`
   - 以 // 为注释符的多行注释
     1. `:n_1,n_2s/^/\/\//` 	
     2. 相反 `:n_1,n_2s/^\/\///g` ，__`\` 是转义字符__
   - 在行尾插入相同字符
     1. 在行尾插入 hello：`:%s/$/hello/g`

#### __替换 `ab 替代 原字符集`__

1. 介绍
   - 将一段常用字符集，使用简单的几个字符代替
   - vim 插入时，空格 / 回车，会有显示
2. 实例
   - 邮箱使用特殊字符代替
     1. `:ab mail ShenDeZ@163.com` ，vim 插入 mail 时，会自动转变成 `ShenDeZ@163.com`

### 分屏

#### 打开文件并且分屏

1. 垂直分屏
   - 命令 `vim -o[n] file1 [file2……]`
     1. n 表示分屏个数（可以省略）
     2. file 表示文件
   - 一般使用 `vim -o file1 file2` 垂直创建或打开两个文件
   - 如果写 n，但 n 与文件数相等，会创建无名文件
2. 水平分屏
   - 命令 `vim -O[n] file1 [file2……]`

#### Vim命令模式下分屏

__说明：也可以新建文件__

1. 垂直分屏
   - 命令 `:split [file1]`
     1. 写 file，将 file 与当前文件垂直分屏
     2. 不写 file，将当前文件垂直分开，同时分屏的文件同步
   - 命令 `:vs [file1]` 
     1. __只读，可还行__
2. 水平分屏
   - 命令 `:vsplit [file1]`
   - 命令 `:vs [file1]`
3. 新建文件垂直分屏
   - 命令 `:new [file1]`
     1. 写 file 会新建一个文件
     2. __不写 file 会新建一个无名文件__

#### 移动光标

1. 将光标移动到另一个屏幕中

   - 如表格

     | 光标方向     | 命令       |
     | ------------ | ---------- |
     | 上移         | `ctrl+w k` |
     | 下移         | `ctrl+w j` |
     | 左移         | `ctrl+w h` |
     | 右移         | `ctrl+w l` |
     | 移动到下一个 | `ctrl+w w` |
     | 移动上一个   | `ctrl+w p` |

#### 关闭分屏

__说明：此处使用的命令是在文件名由修改的情况下使用__

1. 关闭除当期分屏的所有分屏
   - 命令 `:only` 
     1. 分屏有修改不可关闭
2. 关闭所有分屏
   - 命令 `:qa`
     1. 同样分屏有修改不可关闭

#### 设置 vim 分屏

1. 当前窗口与下一个对调（优先上下，其次左右）
   - 命令 `ctrl+w x`
2. 所有窗口恢复均等
   - 命令 `ctrl+w =`

# shell 编程

## Shell 基础

### shell 概述

#### shell 是什么

1. Shell 是命令行解释器，将高级语言（abcd 命令行）转换为机器语言（01010）。它为用户提供了一个向 Linux 内核发送请求以便运行程序的界面 __系统及程序__ ，用户可以用 Shell 来启动、挂载、停止甚至是编写一些程序。 
   - 外层应用程序 --> Shell 命令解释器 --> 内核 --> 硬件。__内核使用机器语言调用硬件，Shell 就是一层内核的外壳。__
2. Shell 还是一个功能相当强大的编程的语言，易编写、易调试、灵活性较强。Shell 是解释执行的脚本语言，在 Shell 中可以直接调用 Linux 系统命令

#### Shell 的分类

1. Bourne Shell（BShell）：从 1979 年起 Unix 就开始使用 Bourne Shell，Bunrne Shell 的主文件名为 `.sh` 。
2. C Shell：C Shell 主要是在 BSD 版的 Unix 系统中使用，其语法和 C 语言相似而得名。
3. Shell 得两种主要语法类型有 Bourne 和 C。两种语法彼此不兼容。Bourne 家族主要包括：sh、ksh、Bash、psh、zsh；C 家族主要包括：csh、tcsh。
4. __Bash：Bash 与 sh 兼容，现在使用的 Linux 就是使用 Bash 作为用户的基本 Shell__

#### Linux 支持的 Shell

1. 文件 `/etc/shells` 

   - 查看 Linux系统支持哪种 Shell

     ```shell
     # /etc/shells: valid login shells
     /bin/sh
     /bin/bash
     /bin/rbash
     /bin/dash
     ```

   - 查看系统使用哪种 Shel ，命令 `echo $SHELL` ；直接输入 Shell 切换

     ```shell
     ss@localcomputer:~$ echo $SHELL
     /bin/bash
     ss@localcomputer:~$ sh		# 切换 Shell
     $ exit						# 退出
     ss@localcomputer:~$ 
     ```

### Shell 脚本执行方式

#### echo 输出命令

1. 命令格式

   - `echo [选项] [输出内容]`

   - 选项

     1. `-e` ：支持反斜杠控制的字符转换

        | 控制字符 | 作用                                                         |
        | -------- | ------------------------------------------------------------ |
        | `\\`     | 输出 \ 本身                                                  |
        | `\a`     | 输出警告音                                                   |
        | `\b`     | 退格键，就是向左删除键                                       |
        | `\c`     | 取消输出行末的换行符，和 -n 选项一致                         |
        | `\e`     | ESCAPE 键                                                    |
        | `\f`     | 换页符                                                       |
        | `\n`     | 换行符                                                       |
        | `\r`     | 回车键                                                       |
        | `\t`     | 制表符，也就是 tab 键                                        |
        | `\v`     | 垂直制表符                                                   |
        | `\0nnn`  | 按照八进制 ASCII 码表输出字符。其中 0 为数字零，nnn 是三位八进制数 |
        | `\xhh`   | 按照十六进制 ASCII 码表输出字符。其中 0 为数字零，hh 是两位十六进制数 |

   - 支持颜色输出
   
     1. 输出格式 `echo -e "\e[1;31m 字符 \e[0m"` __（字符：是要输出的内容，其他的是格式要求）__
   
        | 颜色 | 代码 |
        | ---- | ---- |
        | 黑色 | 30m  |
        | 红色 | 31m  |
        | 绿色 | 32m  |
        | 黄色 | 33m  |
        | 蓝色 | 34m  |
        | 洋红 | 35m  |
        | 青色 | 36m  |
        | 白色 | 37m  |
   
2. 演示

   - `\b` 退格键，向左删除一个字符

     ```shell
     ss@localcomputer:~$ echo "abc"
     abc
     ss@localcomputer:~$ echo -e "ab\bc"		# 使用 \b 向左删除一个字符
     ac
     ```

   - `\t` 制表符，`\n` 换行符

     ```shell
     ss@localcomputer:~$ echo -e "a\tb\tc\nd\te\tf"
     a	b	c
     d	e	f
     ```

   - `\x` 、`\0` 按照 十六、八进制输出 ASCII 表中对应的字符__（在 ASCII 中十六进制 61 表示字符 a）__

     ```shell
     ss@localcomputer:~$ echo -e "\x61"
     a
     ```

   - 支持颜色演示

     ```shell
     ss@localcomputer:~$ echo -e "\e[1;31m hello world-->red \e[0m"			# 红色输出
      hello world-->red
      ss@localcomputer:~$ echo -e "\e[1;33m hello world-->yellow \e[0m"		# 黄色输出
      hello world-->yellow 
     ```


#### 第一个脚本

1. 注意

   - Linux 不区分扩展名，但是 Linux 的脚本是 `.sh` 结尾，目的是告诉系统这是一个脚本，使用 Vim 编辑器时，会有颜色帮助信息
   - __Linux 不区分扩展名，非不写扩展名，也没有问题__
   - 脚本的第一行为 `#!/bin/Bash` 标志为 Shell 脚本（声明以下为 Shell 脚本）。可以省略，但是与其他语言一起使用时，会出现其他错误。

2. Shell 脚本样式

   - 简单实例

     ```shell
     #!/bin/bash		# Shell 脚本声明
     # program name	# 项目名称
     # Author:sshen(E-emai:ShenDeZ@163.com)		# 作者及联系方式
     
     echo "hello world"		# 项目体
     ```

#### 执行脚本

__说明：两种执行方式__

1. 赋予执行权限，直接运行

   - `chmod 755 demo.sh`

   - `./demo.sh`

   - 演示__（脚本需要有执行权限）__

     ```shell
     root@localcomputer:~/test# ./demo.sh
     -su: ./demo.sh: 权限不够
     root@localcomputer:~/test# chmod 755 demo.sh 
     root@localcomputer:~/test# ./demo.sh
     hello world
     ```

2. 通过 Bash 调用执行脚本

   - `bash demo.sh`

   - 演示__（使用 bash 调用脚本，则不需要执行权限）__

     ```shell
     root@localcomputer:~/test# bash demo.sh 
     hello world
     ```

#### Windows 脚本与 Linux 脚本的区别

__说明：`cat -A` 命令 显示文件中所有字符，包括回车符等……__

1. Windows 下编写脚本，在 Linux 上运行 __可能__ 报错

   - 回车符的区别（`^M$`）

     ```shell
     root@localcomputer:~/test# cat -A demo1.sh 
     #/bin/bash^M$
     # hello world^M$
     # Author:sshen E-mail:ShenDeZ@163.com^M$
     echo "hello world"^M$
     ```

2. Linux 下编写脚本

   - 回车符（`$`）

     ```shell
     root@localcomputer:~/test# cat -A demo.sh 
     #!/bin/bash$
     # hello world$
     # Author:sshen E-main:ShenDeZ@163.com$
     $
     echo "hello world"$
     ```

3. 分析及解决

   - 分析

     1. 不同的操作系统回车符有可能不同。在 Linux 上显示有所不同，Linux 的回车符是 `M`；Windows 的回车符是 `^M$` ，所以会造成 Linux 的 Shell 编译器不识别或识别错误，报错。
     2. 现在的 Ubuntu 好像自动解决了这个问题，就是不管是 dos 格式还是 unix 格式都照常执行，不会报错。

   - 解决办法

     1. 命令 `dos2unix file` ，将 dos 格式转换成 unix 格式__（dos2unix 需要手动下载）__。当然还有 `unix2dos` 命令 

        ```shell
        root@localcomputer:~/test# cat -A demo1.sh 
        #/bin/bash^M$
        # hello world^M$
        # Author:sshen E-mail:ShenDeZ@163.com^M$
        echo "hello world"^M$
        root@localcomputer:~/test# dos2unix demo1.sh		# 转换格式 
        dos2unix: 正在转换文件 demo1.sh 为Unix格式...
        root@localcomputer:~/test# cat -A demo1.sh 
        #/bin/bash$
        # hello world$
        # Author:sshen E-mail:ShenDeZ@163.com$
        echo "hello world"$
        ```

### Bash 的基本功能

#### 历史命令与命令补全

##### 历史命令

1. 历史命令

   - 命令格式 `history [选项] [历史命令保存文件]`
     1. 可以指定历史命令保存位置，默认保存在家目录下 `.bash_history` 文件下
   - 选项
     1. `-c` ：清空历史命令，包括缓存和文件中的历史命令一并清除__（一般不执行）__
     2. `-w` ：把缓存的历史命令写入历史命令保存文件（不等用户退出，直接将历史命令写入文件中） ，用户家目录下 `~/.bash_history` 。__文件 `.bash_history` b并不会实时对历史命令进行保存，会等到正常退出时，缓存中的历史命令才会保存到文件中__

2. 相关配置

   - 配置文件 `/etc/profile` __Ubuntu 这个文件没有以下参数__

     1. HISTFILE：保存历史命令的文件名，默认是 `~/.bash_history`（用户家目录下）

     2. HISTFILESIZE：历史文件中包含的最大行数，默认值是 500

     3. HISTSIZE：历史命令中保存的数量，就是缓存中保存的数量（默认值是 500）。

        - 例如：`HISTSIZE=1000` ，则使用 `history` 会显示 1000 条历史命令，但行号一般不会对应，第一行行号为 125，则最后一行为 1124，一共 1000 行（显示最近使用 1000 个命令）

          ```shell
          125  ls
          126  cd /
          ……
          1123  vim .bashrc 
          1124  history
          ```

     4. __在 Ubuntu 下这个配置文件，并没有这些参数，就是没有找到这个参数__

   - root 用户家目录 `~/.bashrc` 找到了 root 用户对命令 `history` 的配置，__但是标准的用户配置文件应该是 `~/.bash_profile` 可 Ubuntu 没有__

     1. 如下（两个参数，可以控制 root 用户的 `history` 配置）

        ```shell
        # for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
        HISTSIZE=1000
        HISTFILESIZE=2000
        ```

   - 普通用户 ss 家目录下并没有相关配置文件（`~/.bashrc` 或 `~/.bash_profile` 配置文件），从实际情况来看，ss 用户的 `history` 使用的是默认 500，和默认 `~/.bash_history` 文件（不知原因）。

   - __根据资料看，`/etc/profile` 是全局变量，`~/.bash_profile` 是局部变量，怎么使用、使用谁一目了然了，需要重新登录才能生效__
   
3. 历史命令调用

   - 使用上、下箭头调用以前的历史命令
   - 使用 `!n` 重复执行第 n 条历史命令
     1. n 是 `history` 命令输出的行号
   - 使用 `!!` 重复执行上一条命令
   - 使用 `!字串` 重复执行最后一条以该字串开头的命令
     1. 方便，复杂命令可以简写

##### 命令与文件补全

1. 在 Bash 中，命令与文件补全是非常方便与常用功能，我们只要在输入命令后文件时，__按 `Tab` 键会自动进行补全__

   - 如 `user` 开头的命令有多个，键入一次 `Tab` 并没有补全，再键入一次 `Tab` 会提以 `user` 开头的命令

     ```shell
     ss@localcomputer:~$ user # 一次 tab 没有补全，表示 user 开头的命令不止一个，再次 tab 显示所有
     useradd  userdel  usermod  users 
     ```

#### 命令别名与常用快捷键

##### 命令别名

1. 命令别名
   - 设定别名命令 
     
     1. `alias 别名='原命令'`
     
   - 查询别名命令
     
     1. `alias`
     
   - 删除别名 `unalias 别名`
   
   - 演示
   
     1. 添加别名 __（临时生效，切换用户都会失效）__ 最好不要与系统命令重名
   
        ```shell
        ss@localcomputer:~$ alias ls='ls --color'
        ss@localcomputer:~$ alias
        alias ls='ls --color'
        ```
   
     2. 删除别名
   
        ```shell
        ss@localcomputer:~$ alias
        alias ls='ls --color'
        ss@localcomputer:~$ unalias ls
        ss@localcomputer:~$ alias
        ```
   
        
   
2. 命令执行顺序

   - 第一顺序位执行绝对路径或相对路径执行的命令

     1. 如：在 `/bin` 下执行 `ls` 命令时，不会去找别名，只会执行单纯的 `ls` 命令

   - 第二顺位执行别名

   - 第三顺位执行 Bash 内部命令

     1. Bash 本身自己的功能，没有执行文件
     2. `whereis 命令` ，查不到执行文件，只有帮助文档

   - 第四顺位执行按照 $PATH 环境变量定义的目录查询顺序找到的第一个命令

     1. 环境变量，以 ":" 分割（在以下目录中查找是否有这些命令）

        ```shell
        ss@localcomputer:~$ echo $PATH
        /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
        ```

     2. 外部命令就是有执行文件，`whereis 命令` 可以找到执行文件，在环境变量的目录下

3. 让别名永久生效

   - 永久生效，因为使用 `alias 别名='命令'` 只会临时生效，系统重启就结束

   - 系统永久生效，归根结底要写入配置文件中

   - 家目录下的 `.bashrc` 配置文件

     1. `root/.bashrc`

        ```shell
        # some more ls aliases
        alias ll='ls -alF'
        alias la='ls -A'
        alias l='ls -CF'
        
        # Alias definitions.
        # You may want to put all your additions into a separate file like
        # ~/.bash_aliases, instead of adding them here directly.
        # See /usr/share/doc/bash-doc/examples in the bash-doc package.
        ```

     2. Ubuntu 下的普通用户家目录下 `/home/ss` 没有 `.bashrc` 文件。~~查看 `root/.bashrc` 文件注释，在用户目录下创建 `~/.bash_aliases` 文件，写入别名命令，重启系统即可生效。~~ 不好用

##### 常用快捷键

1. 快捷键，直接使用 `ctrl+字母` 不用管大小写（默认大写）

   - 如表（重点标粗）

     | 快捷键       | 作用                                                         |
     | ------------ | ------------------------------------------------------------ |
     | `ctrl+A`     | 把光标移动到命令行开头。如果我们输入的命令过长，想把光标移动到命令行开头时使用 |
     | `ctrl+E`     | 把光标移动到命令行结尾                                       |
     | __`ctrl+C`__ | 强制终止当前命令                                             |
     | `ctrl+L`     | 清屏，相当于 clear 命令                                      |
     | __`ctrl+U`__ | 删除或剪切光标之前的命令。如输入一个很长的命令，不用使用退格键一个一个删除字符，使用这个快捷键更加方便 |
     | __`ctrl+K`__ | 删除或剪切光标之后的内容                                     |
     | __`ctrl+Y`__ | 粘贴 `ctrl+K` 或 `ctrl+U` 的内容                             |
     | __`ctrl+R`__ | 在历史命令中搜索，按下 `ctrl+R` 之后，就会出现搜索界面，只要输入搜索内容，就会从历史命令中搜索 |
     | `ctrl+D`     | 退出当前终端                                                 |
     | `ctrl+Z`     | 暂停，并放入后台。这个快捷键牵扯工作管理的内容，在系统管理章节详细介绍 |
     | `ctrl+S`     | 暂停屏幕输出                                                 |
     | `ctrl+Q`     | 恢复屏幕输出                                                 |

#### 输入输出重定向

##### 正确与错误命令输出分开保存

1. 标准的输入输出

   - 标准（standard in、out、err）输入、输出、错误输出。设备文件名不太好记忆，所以有了 __文件描述符__ 0 表示键盘、1 表示标准输出、2 表示错误输出

     | 设备   | 设备文件名    | 文件描述符 | 类型         |
     | ------ | ------------- | ---------- | ------------ |
     | 键盘   | `/dev/stdin`  | 0          | 标准输入     |
     | 显示器 | `/dev/stdout` | 1          | 标准输出     |
     | 显示器 | `/dev/stderr` | 2          | 标准错误输出 |

2. 输出重定向

   - 输出重定向是，命令执行结果不在输出到屏幕上，而是输出到文件中。因为改变了输出方向，所以叫输出重定向

     | 类型               | 符号              | 作用                                                         |
     | ------------------ | ----------------- | ------------------------------------------------------------ |
     | 标准输出重定向     | 命令 > 文件       | 以 __覆盖__的方式，把命令的正确输出输出到指定 __文件中或设备当中__ |
     |                    | 命令 >> 文件      | 以 __追加__ 的方式，把命令的正确输出输出到指定 __文件中或设备当中__ |
     | 标准错误输出重定向 | 错误命令 2> 文件  | 以 __覆盖__ 的方式，把命令的错误输出输出到指定 __文件中或设备当中__ |
     |                    | 错误命令 2>> 文件 | 以 __追加__ 的方式，把命令的错误输出输出到指定 __文件中或设备当中__ |

3. 何时使用重定向

   - 需要保留命令执行结果，以供管理员参考
   - 命令得有输出结果

4. 实例

   - 重定向命令 `date` ，标准输出重定向

     ```shell
     ss@localcomputer:~$ date > time
     ss@localcomputer:~$ cat time
     2020年 02月 09日 星期日 18:22:02 CST
     ```

   - 错误命令得重定向 `lst` ，标准错误命令输出重定向（得使用 `2>` 或 `2>>`）

     ```shell
     ss@localcomputer:~$ lst		# 提示信息没有此命令
     
     Command 'lst' not found, but there are 17 similar ones.
     
     ss@localcomputer:~$ lst > time		# 重定向并没有作用
     
     Command 'lst' not found, but there are 17 similar ones.
     
     ss@localcomputer:~$ lst 2>time		# 使用 2> 表示错误命令提示信息
     ss@localcomputer:~$ cat time		# 显示重定向文件信息
     
     Command 'lst' not found, but there are 17 similar ones.
     
     ```

##### 正确与错误命令输出同时保存

1. 同时保存

   - 如表

     | 类型                       | 符号                    | 作用                                                     |
     | -------------------------- | ----------------------- | -------------------------------------------------------- |
     | 正确输出和错误输出同时保存 | 命令 > 文件 2>&1        | 以覆盖得方式，把正确输出和错误得输出都保存到同一个文件中 |
     |                            | 命令 >> 文件 2>&1       | 以追加得方式，把正确输出和错误得输出都保存到同一个文件中 |
     |                            | 命令 &> 文件            | 以覆盖得方式，把正确输出和错误得输出都保存到同一个文件中 |
     |                            | 命令 &>> 文件           | 以追加得方式，把正确输出和错误得输出都保存到同一个文件中 |
     |                            | 命令 >> 文件1 2>> 文件2 | 把正确得输出追加到文件 1 中，把错误得输出追加到文件 2 中 |

2. 实例

   - 个人感觉 `命令 &>> 文件` 好用

     ```shell
     ss@localcomputer:~$ date &>> time		# 正确命令输出
     ss@localcomputer:~$ cat time
     2020年 02月 09日 星期日 18:52:39 CST
     ss@localcomputer:~$ lst &>> time		# 错误命令输出
     ss@localcomputer:~$ cat time			# 显示所有
     2020年 02月 09日 星期日 18:52:39 CST
     
     Command 'lst' not found, but there are 17 similar ones.
     
     ```

   - __一般都会使用 `命令 >> file1 2>> file2`__

   - __/dev/null 像是个垃圾桶，不是文件也不是目录，就好比是一个垃圾桶。可以把一些命令输出得无用信息放入其中__

##### 输入重定向

1. 输入重构定向介绍

   - 一般键盘作为输入对象，现在使用文件作为输入对象，叫作输入重定向
   - __一般不常见__

2. 实例

   - 统计命令 `wc [选项] [文件名]` 

   - 选项

     1. `-c` ：统计字节
     2. `-w` ：统计单词
     3. `-l` ：统计行数

   - 使用 `wc` 统计命令，按 `ctrl+D` 结束，统计行数、单词、字符

     ```shell
     ss@localcomputer:~$ wc
     hello world
     I am lover
           2       5      23
     
     ```

   - 输入重定向使用 `wc < 文件`

     ```shell
     ss@localcomputer:~$ wc < time		# 将 time 文件作为 wc 得输入
       4  16 101
     ss@localcomputer:~$ wc time			# 命令格式有，规范
       4  16 101 time
     
     ```

   - `<<` 使用，`wc <<hello` ，不再以 `ctrl+D` 结束，而是以输入`hello` 结束，统计 `hello` 中间得输入符

     ```shell
     ss@localcomputer:~$ wc << hello
     > scs
     > vssv
     > svs
     > sdhello
     > hello
      4  4 21
     ```

#### 多命令顺序执行与管道符

##### 多命令顺序执行

1. 多命令顺序执行

   - 就是几条命令存在或不存在某种关系

     | 多命令执行符 | 格式             | 作用                                                         |
     | ------------ | ---------------- | ------------------------------------------------------------ |
     | ;            | 命令1 ; 命令2    | 多条命令顺序执行，命令之间没有任何逻辑关系（1错了，2也执行；2错了，1也会执行得） |
     | &&           | 命令1 && 命令2   | 逻辑与 <br> 当命令1 正确执行，则命令2 才会执行<br> 当命令1 不正确执行，命令2 不会执行 |
     | \|\|         | 命令1 \|\| 命令2 | 逻辑或<br> 当命令1 不正确执行，命令2 才会执行<br> 当命令1 正确执行，命令2 不会执行 |

2. 实例

   - 使用 `;` 分割命令，就是存粹为了方便几条命令缩写

     ```shell
     ss@localcomputer:~$ ls; cd /user; date; pwd
     mbox  time  VScode  公共的  模板  视频	图片  文档  下载  音乐	桌面
     bash: cd: /user: 没有那个文件或目录		# 命令报错，没有影响其他命令
     2020年 02月 09日 星期日 22:03:04 CST
     /home/ss
     ```

##### 多命令执行演示

1.  命令 `dd if=file1 of=file2 bs=字节数 count=个数 `

   - 作用：数据复制命令，可以复制分区（包括文件系统）、硬盘（包括文件系统）、文件及特殊文件

   - 选项

     1. `if=输入文件` ：指定源文件或原设备
     2. `of=输出文件` ：指定目标文件或目标设备
     3. `bs=字节数` ：指定一次输入、输出多少字节，即把这些字节看成一个数据块
     4. `count=个数` ：指定输入、输出多少个数据块

   - 解释命令

     1. 从 file1 中复制数据到 file2 中，一次复制一个数据块（bs 设置数据块大小），复制多少个数据块（count）
     2. $bs \times count = 复制数据大小$ ，如果复制数据大于源文件，则会复制源文件所有内容（目标文件大小=源文件大小）；如果复制数据小于源文件，则复制部分数据
     3. __不能复制目录__

   - 实例

     1. `/dev/zero` 与 `/dev/null` 都是特殊文件，一个是白洞，一个是黑洞

     2. `date ; dd if=/dev/zero of=/home/ss/testfile bs=1K count=100000 ; date` ，__复制 `/dev/zero` 等于创建将近 100MB 大小得空白文件，使用多长时间__

        ```shell
        ss@localcomputer:~$ date ; dd if=/dev/zero of=/home/ss/testfile bs=1K count=100000 ; date
        2020年 02月 09日 星期日 22:44:31 CST		# date 命令输出
        记录了100000+0 的读入
        记录了100000+0 的写出
        # 复制 102400000 字节（实际复制数据是 102MB 到 98MB 之间）使用了 0.337895 s
        102400000 bytes (102 MB, 98 MiB) copied, 0.337895 s, 303 MB/s	
        2020年 02月 09日 星期日 22:44:31 CST		# date 命令输出
        ss@localcomputer:~$ ls -ldh testfile 
        -rw-r--r-- 1 ss ss 98M 2月   9 22:44 testfile
        ss@localcomputer:~$ cat testfile 			# 文件什么都没有
        ss@localcomputer:~$ 
        
        ```

2. `&&` 逻辑与使用

   - 有些命令需要前一条命令正确执行，才可以执行
     
   1. 如：源码包安装 `./configure && make && make install` ，第一条命令正确执行，在执行第二条，最后执行第三条。这些命令又相互依赖关系。
   
   - `ls && echo yes` 

     1. 只有 `ls` 正确执行，才会输出 yes

        ```shell
        ss@localcomputer:~$ ls && echo yes		# 命令 ls 正确执行，输出 yes
        mbox  test  test1  time  VScode  公共的  模板  视频  图片  文档  下载  音乐  桌面
        yes						# 输出 yes
        ss@localcomputer:~$ lst user && echo yes	命令 lst 错误，yes不输出
        ls: 无法访问'user': 没有那个文件或目录
        ```

3. `||` 逻辑或使用

   - `ls || echo no`

     1. 只有 `ls` 报错，才会输出 no

        ```shell
        ss@localcomputer:~$ lst || echo no		# 命令 lst报错，输出 no
        
        Command 'lst' not found, but there are 17 similar ones.
        
        no		# 输出 no
        ss@localcomputer:~$ ls || echo no		# 命令 ls 正确执行
        mbox  test  test1  time  VScode  公共的  模板  视频  图片  文档  下载  音乐  桌面
        
        ```

4. `&&` 与 `||` 同时执行

   - `ls && echo yes || echo no` ，顺序两两判断是否正确。此命令判断 `ls` 是否正确执行。

     ```shell
     ss@localcomputer:~$ ls || echo no
     mbox  test  test1  time  VScode  公共的  模板  视频  图片  文档  下载  音乐  桌面
     ss@localcomputer:~$ ls && echo yes || echo no
     mbox  test  test1  time  VScode  公共的  模板  视频  图片  文档  下载  音乐  桌面
     yes
     ss@localcomputer:~$ lst && echo yes || echo no
     
     Command 'lst' not found, but there are 17 similar ones.
     
     no
     ```


##### 管道符

1. 管道符介绍

   - 格式 
     1. `命令1 | 命令 2`
   - 命令1 的正确输出作为命令2 的操作对象，__命令1必须有正确的文件输出__

2. 实例

   - `ls -al /etc | more`， `more` 命令是分屏显示 __文件内容__，不能直接显示命令输出内容，有了管道符就可以实现了。

   - `netstat -an` 查看系统所有网络连接，和网络套接字程序。__`netstat -an | grep --color=auto tcp` 查看所有网络连接中的监听端口中使用 tcp 协议__

     ```shell
     ss@localcomputer:~$ netstat -an | grep --color=auto tcp
     tcp        0      0 0.0.0.0:10000           0.0.0.0:*               LISTEN     
     tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN     
     tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
     tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN     
     tcp        0      0 0.0.0.0:25              0.0.0.0:*               LISTEN     
     tcp6       0      0 :::10000                :::*                    LISTEN     
     tcp6       0      0 :::22                   :::*                    LISTEN     
     tcp6       0      0 ::1:631                 :::*                    LISTEN     
     tcp6       0      0 :::25                   :::*                    LISTEN     
     ```

#### 通配符与其他特殊符

##### 通配符

1. 通配符介绍

   - 有点像正则表达式

   - 如表

     | 通配符 | 作用                                                         |
     | ------ | ------------------------------------------------------------ |
     | `?`    | 匹配一个任意字符                                             |
     | `*`    | 匹配 0 个或任意多个字符，也可以匹配任何内容。                |
     | `[]`   | 匹配中括号任意一个字符。例如：[abc] 代表一定匹配一个字符，或者是 a、或者是 b、或者是 c |
     | `[-]`  | 匹配中括号任意一个字符。- 代表一个范围。例如：[a-z]  匹配一个小写的字符。 |
     | `[^]`  | 逻辑非，代表匹配的不是括号中的一个字符。例如：`[^0-9]` 代表匹配一个不是数字的字符 |

2. 实例

   - 通配符是用来匹配文件名的

   - 通配符有利于查找、删除命令的方便执行

##### 其他符号

1. Bash 中其他特殊符号

   - 如表

     | 符号 | 作用                                                         |
     | ---- | ------------------------------------------------------------ |
     | ''   | 单引号。在单引号中所有的特殊符号，如 "$" 和 "`" 反引号都没有特殊含义 |
     | ""   | 双引号。在双引号中特殊符号都没有特殊含义，但是 "$"、"`"、"\\" 是例外，拥有 "调用变量的值"、"引用命令" 和 "转义符" 的特殊含义。 |
     | ``   | 反引号。反引号括起来的内容是系统命令，在 Bash 中会先执行它。和 `$()` 作用一样，不过推荐使用 `$()`，因为反引号特别容易看错 |
     | $()  | 和反引号作用一样，用来引用系统命令                           |
     | #    | 在 Shell 脚本中，# 开头的行表示注释                          |
     | $    | 用于调用变量的值，如果需要调用变量 name 的值时，需要使用 `$name` 的方式得到变量的值 |
     | \    | 转义符，跟在 `\` 之后的特舒服将失去含义，变为普通字符。<br> 如 `\$` 将输出 `$` 符号，而不是当变量的引用。 |

2. 实例

   - 测试 '' 单引号，"" 双引号

     ```shell
     ss@localcomputer:~$ name=sshen		# 给 name 变量复制
     ss@localcomputer:~$ echo "$name"	# 双引号使用
     sshen
     ss@localcomputer:~$ echo '$name'	# 单引号使用
     $name
     ```

   - 测试 `` 反引号 和 $() ，会先执行系统命令，再进行输出或赋值

     ```shell
     ss@localcomputer:~$ echo `date`		# 反引号使用
     2020年 02月 10日 星期一 15:39:37 CST
     ss@localcomputer:~$ echo $(date)	# $() 使用，推荐使用
     2020年 02月 10日 星期一 15:39:46 CST
     # 先执行，在进行赋值
     ss@localcomputer:~$ t=`date`
     ss@localcomputer:~$ echo "$t"	# 输出变量
     2020年 02月 10日 星期一 15:41:36 CST
     ```

### Bash 的变量

#### 变量介绍

##### 什么是变量

1. 变量
   - 变量是计算机的存储单元，其中存放的值是可以改变的。当 Shell 脚本中要保存一些信息时，如一个文件名、一个数字，就把他存放在一个变量中
   - 每一个变量都会有一个名字，所以很容易引用它。使用变量可以保存有用信息，使系统获知用户相关设置，变量也可以保存临时信息
2. 变量设置规则
   - 变量名称可以由字母、数字和下划线组成，但是不能以数字开头。如变量名使 `2name` 则是错误的
   - 在 Bash 中，变量的默认类型都是字符串，如果要进行数值运算，则必须指定变量类型为数值型
   - __变量用等号连接值，等号左右两边不能有空格__
   - 变量的值如果有空格，需要使用单引号或双引号包括
   - __在变量值中，可以使用 `\` 转移字符__
   - 如果需要增加变量的值，那么可以进行变量值的叠加。不过变量需要使用双引号包括
     1. `"$变量名"` 或者 `${变量名}` 包括
   - 如果把命令的结果作为变量的值赋予变量，则需要使用 __反引号__ 或`$()` 包含命令
     1. `name=$(date)` 
   - __环境变量名建议大写，便于区分__

##### 变量分类

1. 分类
   - 用户自定义变量
   - 环境变量：这种变量主要保存的是和系统操作环境相关的数据
     1. 不能修改环境变量名，但可以修改其值
     2. 可以添加环境变量
   - 位置参数变量：这种变量主要是用来向脚本当中传递参数或数据的，变量名不能自定义，变量作用固定
     1. 不允许添加，不允许修改变量名
     2. 只能修改其值
   - 预定义变量：在 Bash 中已经定义好的变量，变量名不能自定义，变量作用也是固定的
     1. 不允许添加，不允许修改变量名
     2. 只能修改其值

#### 用户自定义变量

1. 介绍

   - 用户自定义变量也叫本地变量

2. 变量定义

   - 格式 `name="sshen tree"`

   - 使用 `$` 加变量名

3. 变量叠加

   - 格式 `number="123"` ，在变量 `number` 叠加值 `number="$number""456"` 或者 `number=${number}"456"`

     ```shell
     ss@localcomputer:~$ echo "$name"
     ss
     ss@localcomputer:~$ name="$name""hen"
     ss@localcomputer:~$ echo "$name"
     sshen
     ss@localcomputer:~$ name=${name}"hen"		# 推荐使用这个方式叠加变量值
     ss@localcomputer:~$ echo "$name"
     sshenhen
     ```

4. 变量的查看与删除

   - 变量查看
     1. 命令 `set` 查看所有变量，包括环境、预定义、位置变量
   - 变量删除
     1. unset 变量名

#### 环境变量

##### 环境变量介绍

1. 介绍

   - 一部分环境变量为 __系统环境变量__，变量名称不允许改变，只允许修改变量值
   - 另一部分为自定义添加
   - 环境变量
     1. 用户自定义变量只在当前的 Shell 中生效，而环境变量会在当前 Shell 和这个 Shell 的所有子 Shell 当中生效。如果把环境变量写入相应的配置文件，那么这个环境变量就会在所有的 Shell 中生效
     2. 注意：用户自定义变量的作用域；命令行写入的环境变量作用域；写入配置文件的环境变量作用域。
     3. 命令得执行都需要路径（相对路径、绝对路径），而系统命令则没有显示使用路径（如：`ls`）。这里系统命令使用了 __环境变量__ ，使用系统命令是会在环境变量的路径查找，而这些系统命令的路径都在环境变量中。
     4. __使用变量叠加的方式，可以加入自定义的命令，而不是修改系统的环境变量__

2. 设置环境变量

   - 声明变量
     1. `export 变量名=变量值` ，__`export` 将变量声明为环境变量__
   - 查询变量（专用查看环境变量）
     1. `env`
   - 删除变量
     1. `unset 变量名`

3. 实例

   - 子 Shell 的使用，在一个 Shell 窗口切换 Shell （支持 `/binsh`）就是子 Shell ，使用 `exit` 退出子 Shell

     1. 创建子 Shell

        ```shell
        ss@localcomputer:~$ sh
        $ exit
        ```

     2. 查询子 Shell，命令 `pstree` 确定进程数

        ```shell
        # 查询子 Shell，两个 bash ，前面为父 Shell，后为子 Shell
        ├─gnome-terminal-─┬─bash───bash───pstree
                          └─3*[{gnome-terminal-}]
                          
        # 子 Shell 退出，只有一个 Shell
        ├─gnome-terminal-─┬─bash───pstree
                          └─3*[{gnome-terminal-}]
        
        ```

   - 验证，环境变量在子 Shell 也存在

     1. 声明环境变量和用户变量

        ```shell
        # 声明两个用户变量
        ss@localcomputer:~$ name="sshen"
ss@localcomputer:~$ email="ShenDeZ@163.com"
        
        ```
     
     2. 将变量 email 声明为环境变量
     
        ```shell
        ss@localcomputer:~$ export email
        ```
     
     3. 创建子 Shell
     
        ```shell
        ss@localcomputer:~$ bash
        ```
     
     4. set 查看变量，只有环境变量 email
     
        ```shell
        email=ShenDeZ@163.com   
        ```

##### 系统常见环境变量

1. 常见环境变量 `PATH`

   - `PATH`：系统查找命令的路径

     1. 输出 PATH 值 `echo "$PATH"` ，以 ":" 分割

        ```shell
        ss@localcomputer:~$ echo "$PATH"
        /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
        ```

   - `PATH` 变量叠加

     1. 命令 `PATH=${PATH}":root/sh"`

2. 常见系统提示符的变量 `PS1`

   - `PS1` 得使用 `set` 才可以看见，`env` 看不见

     ```shell
     ss@localcomputer:/home$ echo "$PS1"
     ${debian_chroot:+($debian_chroot)}\u@\h:\w\$ 
     ```

   - 如表格

     | 参数 | 作用                                                         |
     | ---- | ------------------------------------------------------------ |
     | `\d` | 显示日期。格式为 “星期 月 日”                                |
     | `\h` | 显示简写主机名。默认主机名 “localhost”                       |
     | `\t` | 显示 24 小时制时间。格式 “HH:MM:SS”                          |
     | `\T` | 显示 12 小时制时间。格式 “HH:MM:SS”                          |
     | `\A` | 显示 24 小时制时间。格式 “HH:MM”                             |
     | `\u` | 显示当前用户名                                               |
     | `\w` | 显示当前所在目录的完整名称                                   |
     | `\W` | 显示当前所在目录的最后一个目录                               |
     | `\#` | 执行的第几个命令                                             |
     | `\$` | 提示符。如果是 root 用户会显示提示符 `#` ，如果是普通用户会显示提示符 `$` |

   - 修改提示符（系统重启即恢复）

     1. 使用 `\W` 和 `\t` （可以修改颜色）

        ```shell
        ss@localcomputer:/etc/dpkg/origins$ echo "$PS1"
        ${debian_chroot:+($debian_chroot)}\u@\h:\w\$ 
        ss@localcomputer:/etc/dpkg/origins$ PS1='\u@\h:\W \t\$ '		# 修改 PS1
        ss@localcomputer:origins 23:00:56$ echo "$PS1"		# 提示信息改变
        \u@\h:\W \t\$ 
        ```

#### 位置参数变量

1. 位置参数变量

   - 用户向程序传入参数，以达到程序运行的不同效果。这个参数是相对编程来说的。

   - 如图（程序中代表位置的参数）
   
     | 位置参数变量 | 作用                                                         |
     | ------------ | ------------------------------------------------------------ |
     | `$n`         | n 为数字，`$0` 表示命令本身，`$0-$9` 表示第一到第九个参数，十个以上参数需要使用大括号包括，如 `${10}` |
     | `$*`         | 这个参数代表命令行所有参数，`$*` 把所有的参数看成一个整体（循环按照一个计算） |
   | `$@`         | 这个变量也代表命令行中所有参数，不过 `$@` 把每一个参数区别对待（循环可打印每一个变量） |
     | `$#`         | 这个参数代表命令行中所有参数的个数                           |
   
2. 实例 1

   - 编写脚本

     ```shell
     #!/bin/bash
     
     echo $0		# 代表执行时的命令
     echo $1		# 执行时跟的参数 1
     echo $2		……
     echo $3
     echo $4
     ```

   - 执行

     1. 不传递位置参数

        ```shell
        ss@localcomputer:~/test$ ./argutrain.sh 		# 没有传递参数
        ./argutrain.sh		# 只打印出执行命令本身
        
        
        
        
        
        ```

     2. 传递位置参数

        ```shell
        ss@localcomputer:~/test$ ./argutrain.sh 1 2 3 4		# 传递 4 个位置参数
        ./argutrain.sh		# 打印命令本身
        1					# 打印第一个位置参数
        2
        3
        4
        
        ```

3. 实例 2

   - 编写一个加法计算器（add.sh 文件）

     ```shell
     #!/bin/bash
     
     num1=$1		# 第一个位置变量
     num2=$2		# 第二个位置变量
     sum=$(($num1+$num2))	# 两个变量相加，记住格式两个括号
     echo $sum		# 打印变量 sum
     ```

   - 执行 add.sh 文件

     ```shell
     ss@localcomputer:~/test$ ./add.sh 2 4		# 执行 2+4 等于？
     6
     ```

4. 实例 3

   - 使用 `$#` 代表所有参数个数
     1. `echo "A total of $# parameters"`
   - 使用 `$*` 代表所有参数
     1. `echo "The parameters is:$*"`
   - 使用 `$@` 代表所有参数
     1. `echo "The parameters is:$@"`

#### 预定义变量

##### 预定义变量

1. 介绍

   - __位置参数就是预定义变量的一种，都是系统或程序预先定义好__

   - 预定义变量

     | 预定义变量 | 作用                                                         |
     | ---------- | ------------------------------------------------------------ |
     | `$?`       | 最后一次执行的命令的返回状态。<br> 如果这个变量值为 0，证明上一个命令正确执行；<br> 如果这个变量值为非 0（具体哪一个数，由命令自己决定），则证明上一个命令执行不正确 |
     | `$$`       | 当前进程的进程号（PID）                                      |
     | `$!`       | 后台运行的最后一个进程的进程号                               |

2. 实例

   - 上一个命令执行状态 `$?`，返回 0 正确执行；非 0 命令执行错误__（每一个错误对应这一个数字）__

     ```shell
     ss@localcomputer:~$ ls
     mbox  test  VScode  公共的  模板  视频	图片  文档  下载  音乐	桌面
     ss@localcomputer:~$ echo $?
     0
     ss@localcomputer:~$ lst
     
     Command 'lst' not found, but there are 17 similar ones.
     
     ss@localcomputer:~$ echo $?
     127
     ```

   - 打印进程号 `$$` ，和后台最后一个进程号 `$!`

     1. 编写脚本

        ```shell
        #!/bin/bash
        
        echo $$		# 打印此进程 PID
        
        find /root -name mo &		# & 表示命令放入后台执行
        echo $!			# 打印最后一个后台执行进程 PID
        ```

     2. 执行脚本

        ```shell
        root@localcomputer:/home/ss/test# ./pid.sh 
        3306		# ./pid.sh 进程号
        3307		# find 放入后台执行的命令
        ```

##### 接受键盘输入（类似 input）

1. 介绍

   - 命令 `read [选项] [变量名]`
   - 选项
     1. `-p 提示信息` ：在等待 `read` 输入时，输出提示信息
     2. `-t 秒数` ：`read` 命令会一直等候用户输入，使用此选项可以指定等待时间
     3. `-n` 字符数量：`read` 命令只接受指定的字符数，就会执行
     4. `-s` ：隐藏输入的数据，适用于机密信息输入

2. 实例 1

   - 编写脚本，提示 “请输入姓名” 并等待 20 秒，把用户的输入保存入变量 name 中

     ```shell
     #!/bin/bash
     
     read -t 20 -p "Please input your name " name
     echo "Name is $name"
     ```

   - 执行脚本

     ```shell
     root@localcomputer:/home/ss/test# ./input.sh 	# 执行脚本
     Please input your name sshen		# 提示信息，并输入 sshen
     Name is sshen			# 打印姓名
     ```

### Bash 的运算符

#### 数值运算与运算符

##### 类型变量声明

1. 介绍

   - 在 Linux 的 Shell 中，变量的类型默认是 __字符串__
   - 在 Linux 的 Shell 中，想要进行数值运算就需要使用特殊的 __运算符__

2. `declare` 声明变量类型

   - 格式 `declare [+/-] [选项] [变量名]` 
   - 选项
     1. `-` ：给变量设定类型属性
     2. `+` ：取消变量的类型属性
     3. `-i` ：将变量声明为整型（integer）
     4. `-x` ：将变量声明为环境变量
     5. `-p` ：显示指定变量的被声明的类型

3. 实例

   - `declare -p 变量名` 查看类型

     ```shell
     ss@localcomputer:~$ name="sshen"	# 设定用户变量
     ss@localcomputer:~$ declare -p name		# 查看变量声明类型
     declare -- name="sshen"
     ss@localcomputer:~$ export name		# 将用户变量声明为环境
     ss@localcomputer:~$ declare -p name		# 查看变量类型
     declare -x name="sshen"		# -x 表示环境变量（`export` 和 `-x` 一样）
     
     ```

##### 数值运算

1. 方法一
   - 声明变量，给变量赋值
     1. `aa=11`
     2. `bb=22`
   - `declare -i cc=$aa+$bb`，声明了变量 `cc` 为整数型，可是 `aa` 、`bb` 还是字符串啊。不是特别理解。
   
2. 方法二：`expr` 或 `let` 数值运算工具

   - 声明变量，给变量赋值
     1. `aa=11`
     2. `bb=22`
   - `dd=$(expr $aa + $bb)` ，`dd` 的值是 `aa` 和 `bb` 的和。__注意 `+` 号左右两侧必须有空格__

3. 方法三：`$((运算式))` 或 `$[运算式]` ，__推荐使用__

   - 声明变量，给变量赋值
     1. `aa=11`
     2. `bb=22`
   - `dd=$(($aa+$bb))` ，一般使用此格式（`$()` 系统命令）
   - `dd=$[$aa+$bb]` ，（`${}` 变量追加）

4. Shell 支持的运算符

   - 运算符（优先级从上到下）

     | 优先级 | 运算符                                                 | 说明                               |
     | ------ | ------------------------------------------------------ | ---------------------------------- |
     | 13     | - , +                                                  | 单目负数，单目正数                 |
     | 12     | ! , ~                                                  | 逻辑非，按位取反或补码             |
     | 11     | * , / , %                                              | 乘，除，取模                       |
     | 10     | + , -                                                  | 加，减                             |
     | 9      | << , >>                                                | 按位左移，按位右移                 |
     | 8      | <= , >= , < , >                                        | 小于或等于，大于或等于，小于，大于 |
     | 7      | == , !=                                                | 等于，不等于                       |
     | 6      | &                                                      | 按位与                             |
     | 5      | ^                                                      | 按位异或                           |
     | 4      | \|                                                     | 按位或                             |
     | 3      | &&                                                     | 逻辑与                             |
     | 2      | \|\|                                                   | 逻辑后                             |
     | 1      | = , += , -= , *= , /= , %= , &= , ^= , \|= , <<= , >>= | 运算且赋值                         |

#### 变量测试与内容替换

##### 变量检测与内容替换

1. 介绍

   - `echo` 是输出到屏幕上（用户查看）。__编写脚本时，得用测试变量的方法检测变量得值__

   - 使用系统支持的一种格式，来检测变量是否存在。这种方式完全可以自定义脚本使用 `if` 来完成

   - 固定格式（`:` 冒号的区别，当 y 为空，x得取值相反）

     | 变量置换方式   | 变量 y 没有设置                                | 变量 y 为空值            | 变量 y 设置值           |
     | -------------- | ---------------------------------------------- | ------------------------ | ----------------------- |
     | `x=${y-新值}`  | `x=新值`                                       | `x为空`                  | `x=$y`                  |
     | `x=${y:-新值}` | `x=新值`                                       | `x=新值`                 | `x=$y`                  |
     | `x=${y+新值}`  | `x为空`                                        | `x=新值`                 | `x=新值`                |
     | `x=${y:+新值}` | `x为空`                                        | `x为空值`                | `x=新值`                |
     | `x=${y=新值}`  | `x=新值` <br>`y=新值`                          | `x为空` <br>`y 的值不变` | `x=$y` <br>`y 的值不变` |
     | `x=${y:=新值}` | `x=新值` <br>`y=新值`                          | `x为新值` <br/>`y为新值` | `x=$y` <br>y 的值不变   |
     | `x=${y?新值}`  | 新值输出到标准的错误输出（当成报错输出到屏幕） | `x为空`                  | `x=$y`                  |
     | `x=${y:?新值}` | 新值输出到标准的错误输出                       | 心智输出到标准的错误输出 | `x=$y`                  |

2. 实例 

   - 测试 `x=${y-新值}`

     1. 变量 y 不存在

        ```shell
        ss@localcomputer:~$ unset y		# 删除变量 y
        ss@localcomputer:~$ x=${y-"new"}	# 进行测试
        ss@localcomputer:~$ echo $x		
        new			# 因为变量 y 不存在，所以 x=new
        ss@localcomputer:~$ 
        ```

     2. 变量 y 为空

        ```shell
        ss@localcomputer:~$ y=""		# 设变量 y 为空
        ss@localcomputer:~$ echo $y
        
        ss@localcomputer:~$ x=${y-"new"}
        ss@localcomputer:~$ echo $x
        							# 变量 x 也为空
        ss@localcomputer:~$ 
        
        ```

     3. 变量 y 有值

        ```shell
        ss@localcomputer:~$ y="old"
        ss@localcomputer:~$ echo $y
        old
        ss@localcomputer:~$ x=${y-"new"}
        ss@localcomputer:~$ echo $x
        old							# x 值为 y 的值
        ss@localcomputer:~$ 
        ```

     4. `:` 冒号的区别（y为空，x 为新值）

        ```shell
        ss@localcomputer:~$ y=""
        ss@localcomputer:~$ x=${y:-"new"}
        ss@localcomputer:~$ echo $x
        new						# x 不在为空，x 为新值
        ss@localcomputer:~$ 
        
        ```


### 环境变量配置文件

#### 环境变量得配置文件简介

##### `source` 命令

1. 命令 `source 配置文件` 或 `. 配置文件`
   - 修改配置文件系统必须重新登录，此命令解决系统重启
   - `.` 和 `source` 一样，`.` 是 `source` 得缩写
2. 介绍
   - 环境变量配置文件中主要是定义对系统得操作环境生效得系统默认环境变量，比如 PATH（定义系统查找命令得路径）、HISTSIZE（历史命令的显示命令条数）、PS1（提示符）、HOSTNAME（主机名） 等等默认环境变量。
   - __centOS__ （Linux也一样）的环境变量配置文件体系是一个层级体系。这与其他用户应用系统配置文件类似，有全局、有局部（用户），另外不同层级有时类似继承关系。

##### 5 类环境配置文件及启动过程

1. 5 类配置文件__（在 `/etc/` 下的配置文件对所有用户都生效，在某个用户家目录下的配置文件对这个用户有效）__
- __`/etc/profile`__
     1. 第一次系统启动后第一个用户登录时执行 `/etc/profile` 文件，该文件是 Shell 默认主启动文件。
     2. bash 执行该文件时，会读取 `/etc/profile.d/` 下所有 `*.sh` 文件，通过 `.` 或者 `source` 来加载变量
     3. bash 执行该文件时，会调用 `/etc/bashrc`（Ubuntu 下为 `/etc/bash.bashrc`） 文件来执行
     4. `/etc/profile` 、`/etc/bash.bashrc` 和 `/etc/profile.d/*.sh` 都是全局环境变量，对所有用户有效
     5. __`/etc/environment` 保存环境变量 PATH 的值。据说 `/etc/profile` 保存所有用户的环境变量，`/etc/environment` 保存系统的环境变量__
   
- `/etc/profile.d/*.sh`（为一组配置文件）
  
  1. 全局环境变量，对所有用户有效。
     2. 通过 `/etc/profile` 调用
  
- __`/etc/bashrc`（Ubuntu `/etc/` 下的文件是 `bash.bashrc`）__
  
  - 全局环境变量，对所有用户有效
- 通过 `/etc/profile` 调用
  
   - __`~/.bash_profile` (Ubuntu 用户家目录下的文件是 `.profile`)__
  1. 家目录下的启动文件都是用户专属启动文件，定义了该用户的环境变量
     2. bash 执行该文件，会调用 `/etc/.bashrc`
     3. bash 会按照下列顺序，执行第一个找到的文件，余下被忽略
        - `~/.bash_profile`
        - `~/.bash_login`
        - `~/.profile`
        - 仅对此用户有效
  
   - __`~.bashrc`__

     - 仅对此用户有效
- 通过 `~/.profile` 调用
  
2. 5 类配置文件的优先级（在 Ubuntu 系统下；每一个 Linux 的发行版本都不样）

   __说明：数字越大，优先级越低__

   - `/etc/profile` 的优先级为 5
   - `/etc/profile.d` 为 `/etc/profile` 调用，并继承上述配置文件，优先级为 4
   - /etc/bash.bashrc` 为 `/etc/profile` 调用，并继承上述配置文件，优先级为 3
   - `~/.profile` 的优先级为 2
   - `~/.bashrc` 为 `~/.profile` 调用，并继承上述配置文件，优先级为 1
   - 修改环境配置在以上 5 个文件都可以，只是优先级高的会覆盖掉优先级低的。

##### Shell bash 不同类别介绍

1. Shell bash 不同类别介绍

   - 分为 login shell（登录式 shell）、non-login shell（非登录式 shell）；interactive shell（交互 shell）和 non-interactive shell（非交互式 shell）。__这两种分类相互交叉。__
     1. 交互式，登录 shell
     2. 非交互式，登录 shell
     3. 交互式，非登录 shell
     4. 非交互式，非登录 shell
   - __注意：只要记住什么是 login shell、non-login shell 和 non-interactive 非登录式 shell__

2. 介绍 3 种

   - 什么是 login shell
     1. 使用 bash 时需要完整的登录流程。就是说通过输入账号和密码登录系统，此时取得的 shell 称为 login shell。例如 `ssh` 登录或者 `su - root` 切换 root 用户都会启动 login shell
     2. 在 shell bash下使用 `--login` 选项调用 bash（前一个 shell bash 的子进程 `bash --login`），可以获得一个交互式 login shell。
     3. 在脚本中使用 `--login` 选项调用 bash（shell 脚本第一行做如下指定：`#!/bin/bash --login`），此时得到一个非交互式的 login shell
     4. 使用 `su -` 切换到指定用户时，获得此用户的 login shell。如果不使用 `-`，则获得 non-login shell。
   - 什么是 non-login shell
     1. 使用 bash 接口的方法不需要重复登录的举动。
     2. 例如：以图形界面（`init 5`）登录 Linux 再启动终端机，此时这个终端机并没有需要输入账号和密码，这个bash 环境就是 non-login shell。
     3. 在原本的 bash 再次执行 `bash`命令，同样也没有输入账号密码就进入新的bash环境（前一个bash的子进程），新的bash也是non-login shell。
   - 什么是非交互，非登录式 shell
     - 执行脚本

3. 不同 shell bash 的启动过程

   - __不同的 shell bash 启动时会读取不同的配置文件，从而导致环境变量不一样。__
     1. 实际上，在 Ubuntu 系统中 `/etc/profile` 配置文件会调用 `/etc/profile.d/*.sh` 和 `/etc/bash.bashrc` 配置文件；`~/.profile` 配置文件会调用 `.bashrc` 配置文件。
     2. 在 `/etc` 下的环境配置文件，如同全局变量；在 `~/.` 下的配置文件，如同局部变量，__局部变量优先级大于全局变量__。

   - login shell 
     1. shell bash 启动时首先读取 `/etc/profile` 全局配置，然后查找（`~/.bash_profile` 、`~/.bash_login`、`~/.profile`）3 个文件任何一个配置文件（找到一个即可），login shell 退出时读取 `~/.bash_logout`。

   - non-login shell
     1. non-login shell 启动时读取 `~/.bashrc` 配置文件
     2. 不会读取任何 `profile` 文件
   - 非交互，non-login shell
     1. 非交互式，non-login shell 不会读取 上述 5 类配置文件，而是查找环境变量 BASH_ENV。

4. 测试使用何种 shell

   __说明：只区分 3 种 shell（login shell、non-login shell、非交互 non-login shell）；我是用的是图形化 Ubuntu 系统（init 5）；前一个 shell 是哪种类型与后一个 shell 是哪种类型无关，至于注入的命令有关。__

   - 登录图像化 Ubunut 系统后，开启终端是 non-login shell

   - 输入 `bash` 命令之后也是 non-login shell

   - 输入 `bash --login` 命令之后是 login shell

   - 输入 `su - root` 命令之后是 login shell

   - 输入 `su root` 命令之后是 non-login shell

   - __可以在 `/etc/profile` 中设置环境变量，理由 login 与 non-login 的区别在此__

     1. 添加环境变量在 `/etc/profile` 文件中 `spro="login shell"`

     2. 验证是否为 login 和 non-login

        ```shell
        ss@localcomputer:~$ echo $spro		# 开启 non-login shell
        
        ss@localcomputer:~$ su - root		# 开启 login shell
        密码： 
        root@localcomputer:~# echo $spro
        login shell
        root@localcomputer:~# bash
        ```

#### 环境变量配置文件作用（此处记录不清楚）

__说明：不同的发行版本不相同，从 `/etc/profile` 自己看就可以__

1. 环境变量配置文件区分优先级__（Ubuntu 系统下）__

   - `/etc/` 配置文件，对所有用户生效，但优先级低

   - `~` 用户家目录的配置文件，对用户本身生效，优先级高

   - 配置文件优先级 

     1. 示意图（是 Ubuntu 图像界面操作系统；centOS 与下图不同）

        ![Linux的Shell变量配置优先级](git_picture/Linux的Shell变量配置优先级.jpg)
   
     2. shell 的登录方式主要分为两种，两种的登录启动的环境配置文件不同，造成了环境变量的不一样。实际在 Ubuntu 中 `profile` 配置文件都是调用 `bashrc` 配置文件。`bashrc` 的优先级高于 `profile` 。
   
   - 在 centOS 的系统中各个环境配置文件对环境变量的定义（Ubuntu 中变量作用相同，但定义的文件有所变化）
   
     1. `/etc/profile` 的作用
        - 定义了哪些环境变量
          1. USER
          2. LOGNAME
          3. MAIL
          4. PATH
          5. HOSTNAME
          6. HISTSIZE
          7. umask
          8. 调用 `/etc/profile.d/*.sh`
     2. `~/.bash_profile` 的作用
        - 调用了 `~/.bashrc` 文件

#### 其他配置文件和登录信息

##### 其他配置文件

1. 注销时生效的环境配置变量文件 `~/.bash_logout`
   - 有则使用，没有也没有问题，但又不能读取则报错
   - 只有 login shell 登录 shell bash 退出登录时生效
   - 可以在 `~/.bash_logout` 编写 
     1. 清空历史命令
     2. 清空环境变量等等
   
2. 历史命令存放文件 `~/.bash_history`
   - 本次登录不如写入，只会写入内存中，当注销登录时才会写入文件
   - 不赞成清空（mysql 设置密码时，明文保存）
   

##### 登录信息

1. Shell 登录信息

   - 什么是终端登录提示信息，建议提示警告信息（无权用户进制登录等……），而不是欢迎信息。

     1. 就是使用 shell bash（`init 3`） 登录系统前的提示信息（如下实例）

        ![Linux本地登录提示信息](git_picture/Linux本地登录提示信息.png)

   - 终端提示信息分为：登陆前提示信息；登陆后提示信息。

   - 三种配置文件

     1. `/etc/issue`
     2. `/etc/issue.net`
     3. `/etc/update-motd.d/` (目录)

2. 本地终端登录信息 （登录前提示信息） ，配置文件 `/etc/issue` 作用

   - __只对本地用户登录有效__（远程登录无效），在提示输入用户名上方的提示信息配置文件（如上图实例）

   - 转移字符作用

     | 转义符 | 作用                             |
     | ------ | -------------------------------- |
     | `\d`   | 显示当前系统日期                 |
     | `\s`   | 显示操作系统名称                 |
     | `\l`   | 显示登录终端号（这个比较有用）   |
     | `\m`   | 显示硬件体系结构（如 i386\i686） |
     | `\n`   | 显示主机名                       |
     | `\o`   | 显示域名                         |
     | `\r`   | 显示内核版本                     |
     | `\t`   | 显示当前系统时间                 |
     | `\u`   | 显示当前登录用户的序列号         |

   - 查看 `/etc/issue` ，`\n` 表示 __显示主机名__；`\l` 表示 __显示登录终端号__

     ```shell
     Ubuntu 18.04.1 LTS \n \l
     ```

3. 远程终端信息（笔者这个也是登录后提示信息，有的系统是登录前信息），配置文件 `/etc/issue.net`

   - __对远程登录有效__，在提示输入用户名上方的提示信息配置文件（Ubuntu 系统）

     ![linux远程登录提示信息](git_picture/linux远程登录提示信息.png)

   - 上一个转移字符表在 `/etc/issue.net` 文件中不能使用

   - __是否显示此提示信息，由 ssh 配置文件 `/etc/ssh/ssh_config` 决定（Ubuntu 是 `/etc/ssh/shhd_config`），加入 `Banner /etc/issue.net` 行才能显示（重启 SSH 服务 `service sshd restart`）__

4. 登录后信息，配置文件 `/etc/motd`（Ubuntu 是 `/etc/update-motd.d` 目录）

   - 不管是本地登录，还是远程登录，都可以显示信息。此提示信息是登陆后的提示信息。

   - Ubuntu 系统是 `/etc/update-motd.d` 目录下的文件对终端登录后的提示信息

     ```shell
     root@localcomputer:/etc/update-motd.d# tree
     .
     ├── 00-header
     ├── 10-help-text
     ├── 50-motd-news
     ├── 80-esm
     ├── 80-livepatch
     ├── 90-updates-available
     ├── 91-release-upgrade
     ├── 95-hwe-eol
     ├── 98-fsck-at-reboot
     └── 98-reboot-required
     ```

   - 笔者对几个配置文件进行测试，查看了两个配置文件对提示信息的控制

     ![linux登陆后提示信息](git_picture/linux登陆后提示信息.png)

   - 对 `00-header` 文件进行修改后，登录提示信息

     ![linux登录测试信息](git_picture/linux登录测试信息.png)

## shell 编程

### 正则表达式

1. 正则表达式与通配符

   - 正则表达式是用来在文件中匹配符合条件的字符串，正则是 __包含匹配__（字符串包含匹配的内容即可，如，abcd、bcde 两个字符串，匹配带有 b 字符的字符串，结果这两个字符串都是搜索结果） 。`grep` 、`awk`、 `sed` 等命令可以支持正则表达式
   - 通配符是用来匹配符合条件的文件名，通配符是 __完全匹配__（搜索字符串，与结果一一对应）。`ls` 、`find` 、`cp` 这些命令不支持正则表达式，所以只能使用 shell 自己的通配符来进行匹配。

2. 基础正则表达式

   - 如表格

     | 元字符    | 作用                                                         |
     | --------- | ------------------------------------------------------------ |
     | `*`       | 匹配前一个字符 0 次或任意多次                                |
     | `.`       | 匹配除换行符外的任意一个字符                                 |
     | `^`       | 匹配行首。例如：`^#start` ，则会匹配行首的 `#start` 字符串，而不是其位置的相同字符串 |
     | `$`       | 匹配行尾。例如：`end$` ，则会匹配行尾的 `end` 字符串，而不是其他位置的相同字符串 |
     | `[]`      | 匹配中括号中指定的任意一个字符，且只会匹配一个字符。<br> 例如：`[abcd]` 匹配其中任意一个字符；`[0-9]` 匹配任意一个数字 |
     | `[^]`     | 匹配中括号字符意外的字符。<br> 例如：`[^0-9]` 匹配任意一个非数字的字符；`[^a-z]` 匹配非小写字母 |
     | `\`       | 转义符，用于取消特殊符号的含义                               |
     | `\{n\}`   | 表示其前面的字符恰好出现 n 次。<br> 例如：`[0-9]\{4,\}`  匹配一个 4 为数字 |
     | `\{n,\}`  | 表示其前面的字符出现不小于 n 次。<br/> 例如：`[0-9]\{2,\}`  匹配一个 2 位及 2 位以上的数字 |
     | `\{n,m\}` | 标识前面的字符至少出现 n 次，最多出现 m 次。<br> 例如：`[a-z]\{2, 4\}` 匹配 2 到 4 个字符 |

3. 实例

   __说明：命令 `grep` 对文件内容的搜索，返回匹配的行。__

   - `*` 前一个字符匹配 0 次，或任意多次
     1. `grep "a*" file` ：匹配所有内容，包括空白行。
     2. `grep "aa*" file` ：匹配至少包含一个 a 的行
     3. `grep "aaa*" file` ：匹配至少包含 2 个连续 a 的行
   - `.` 匹配除了换行符以外的任意一个字符
     1. `grep "s..d" file` ：会匹配在 s 和 d 这两个字符之间一定有 2 个字符的行
     2. `grep "s.*d" file` ：匹配在 s 和 d 字符之间有任意字符
     3. `grep ".*" file` ：匹配所有内容（所有行）
   - `^` 匹配行首，`$` 匹配行尾
     1. `grep "^M" file` ：匹配以大写 M 开头的行
     2. `grep "n$" file` ：匹配以小写 n 结尾的行
     3. `grep -n "^$" file` ：匹配空白行
   - `[]` 匹配中括号中指定的任意一个字符（只匹配一个字符）
     1. `grep "s[ao]id" file` ：匹配 said 或 soid 这两个字符串的行
     2. `grep "[0-9]" file` ：匹配含有一个数字的行
     3. `grep "^[a-z]"` ：匹配以小写字母开头的行
   - `[^]` 匹配除中括号以外的字符
     1. `grep "^[^a-z]" file` ：匹配不用小写字母开头的行
     2. `grep "^[^a-zA-Z] file"` ：匹配不是字母开头的行
   - `\` 转义符
     1. `grep "\.$" file` ：匹配使用 . 结尾的行
   - `\{n\}` 表示前面的数字正好出现 n 次
     1. `grep "a\{3\}" file` ：匹配 a 字符连续出现 3 次的行
     2. `grep "[0-9]\{3\}"` ：匹配包含连续出现 3 个数字的行
   - `\{n,m\}` 匹配其前面字符至少出现 n 次，最多出现 m 次
     1. `grep "sa\{1,2\}b" file` ：匹配在 s 和 i 字符之间 a 至少出现 1 次，最多出现 2 次。如出现 sab 或 saab 这两个字符。

###   字符截取命令

#### cut 字段提取命令

__说明：命令 `cut` 提取的是列，而命令 `grep` 提取的是行__

1. 命令格式 

   - `cut [选项] 文件名` ，一列作为分割对象，默认以 __制表符__ 作为分隔符，当然使用选项 `-d` 也可以自己指定分隔符。
   - 选项
     1. `-f` 列号：提取第几列
     2. `-d` 分隔符：按照指定分隔符分割列

2. 作用

   - 命令 `cut` 和 命令 `grep` 结合才能达到效果。一个对行一个对列。

   - 对 `/etc/passwd` 进行搜索，搜索 包含 `/bin/bash` 字符串行（含有 `bin/bash` 是可以登录的字段）

     1. 命令 `cat passwd | grep /bin/bash`

        ```shell
        root@localcomputer:/etc# cat passwd | grep /bin/bash
        root:x:0:0:root:/root:/bin/bash
        ss:x:1000:1000:ss,,,:/home/ss:/bin/bash
        ```

   - 在对上述结果剔除 root 用户

     1. 命令 `cat passwd | grep /bin/bash | grep -v root`

        ```shell
        root@localcomputer:/etc# cat passwd | grep /bin/bash | grep -v root
        ss:x:1000:1000:ss,,,:/home/ss:/bin/bash
        ```

   - 对上述结果提取用户名，最终达到筛选出除了 root 用户之外的可以正常登录的用户

     1. 命令 `cat passwd | grep /bin/bash | grep -v root | cut -d ":" -f 1`

        ```shell
        root@localcomputer:/etc# cat passwd | grep /bin/bash | grep -v root | cut -d ":" -f 1
        ss
        ```

3. `cut` 命令的局限性

   - `cut` 命令默认分割符 __制表符__，也可以使用 `-d` 指定分隔符，但是注意指定分割符后 `cut` 会严格按照指定的分割符分割列。

   - 对 `df -h` 命令输出的结果进行筛选，`df -h | grep /dev/sda5`

     1. 命令 `df -h` 输出分区信息（这里的空格不是制表符，是空格键）

        ```shell
        文件系统        容量  已用  可用 已用% 挂载点
        udev            957M     0  957M    0% /dev
        tmpfs           198M  1.8M  196M    1% /run
        /dev/sda5        12G  5.6G  5.3G   52% /
        tmpfs           986M     0  986M    0% /dev/shm
        tmpfs           5.0M  4.0K  5.0M    1% /run/lock
        ```

     2. 筛选设备文件名为 `/dev/sda5` 的分区，命令 `df -h | grep /dev/sda5`

        ```shell
        root@localcomputer:/etc# df -h | grep /dev/sda5
        /dev/sda5        12G  5.6G  5.3G   52% /
        ```

   - 在使用 `cut` 命令进行提取，但是可以看出 `df -h` __输出的结果列的间隔不一致__，命令 `cut` 无法很好的分割

     1. 使用 `cut -d " " -f 6` 命令，对 `df -h | grep /dev/sad5` 输出结果再次筛选（输出的是空格，因为 `cut` 是以一个空格分割的，但实际上 `/dev/sda5        12G  5.6G  5.3G   52% /` 每一个间隙是不一致的）

        ```shell
     root@localcomputer:/etc# df -h | grep /dev/sda5 | cut -d " " -f 6
        
        root@localcomputer:/etc# 
        ```
   
4. 实例

   - 使用 `-f` 选项，提取列信息

     1. 创建一个文件（文件格式类似表格形式，空格是 tab 键）

        ```shell
        ss@localcomputer:~/test$ cat grade 	# 构成如表格
        ID	name	grade
        1	tom		78
        2	jack	89
        ```

     2. 命令 `cut -f 1 grade` 查看第一列；命令 `cut -f 1,3 grade` 查看 1 列和 3 列

        ```shell
        ss@localcomputer:~/test$ cut -f 1 grade
        ID
        1
        2
        ss@localcomputer:~/test$ cut -f 1,3 grade
        ID	grade
        1	78
        2	89
        ```

   - 使用 `-d` 指定分割符。提取 `/etc/passwd` 文件

     1. `/etc/passwd` 文件样式，是以 `:`  分割字符的

        ```shell
        root:x:0:0:root:/root:/bin/bash
        daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
        bin:x:2:2:bin:/bin:/usr/sbin/nologin
        sys:x:3:3:sys:/dev:/usr/sbin/nologin
        sync:x:4:65534:sync:/bin:/bin/sync
        ```

     2. 使用 `-d` 指定分割符，命令 `cut -d ":" -f 1,7 /etc/passwd` 提取第一列和第二列。

        ```shell
        root@localcomputer:/etc# cut -d ":" -f 1,7 /etc/passwd
        root:/bin/bash
        daemon:/usr/sbin/nologin
        bin:/usr/sbin/nologin
        sys:/usr/sbin/nologin
        sync:/bin/sync
        games:/usr/sbin/nologin
        ```

#### printf 命令

__说明：参看 c 语言的 printf 用法__

1. 介绍

   - `printf` 为格式化输出命令
   - 命令 `printf '输出类型输出格式' 输出内容` 
   - 输出类型
     1. '%ns' ：输出字符串。n 是数字代之输出几个字符串
     2. `%ni` ：输出整数。n 是数字代之输出几个数字
     3. `%m.nf` ：输出浮点数。m 和 n 是数字代之输出数字的位数。`%4.2f` 表示输出 4 位数，其中 2 位为小数位。 
   - 输出格式
     1. `\a` ：输出警告音
     2. `\b` ：输出退格键，backspace 键
     3. `\f` ：清除屏幕
     4. `\n` ：换行
     5. `\r` ：回车，enter 键
     6. `\t` ：水平输出退格键，tab 键
     7. `\v` ：垂直输出退格键，tab 键
   - __注意__
     1. 命令 `printf` 不能与管道符连用
     2. 命令 `printf` 不能接文件名，会把文件名当成要输出的字符串

2. 实例

   __说明：参照 c printf__

   - 打印单个字符串

     1. 命令 `printf '%s\n' "hello world"` ，输出内容使用 `""` 括起来，代表一个字符串

        ```shell
        ss@localcomputer:~$ printf '%s\n' "hello world"
        hello world
        ss@localcomputer:~$ 
        ```

     2. 命令 `printf '%s\n' hello world` ，输出格式只有一个 `%s` ，所以值对应了 hello ，然后换行，在输出 world

        ```shell
        ss@localcomputer:~$ printf '%s\n' hello world
        hello
        world
        ss@localcomputer:~$ 
        ```

   - 打印多个字符串

     1. 命令 `printf '%s %s %s\n' welcome to china !` ，以空格作为分割打印 3 个字符串，然后换行，！没有对应格式

        ```shell
        ss@localcomputer:~$ printf '%s %s %s\n' welcome to china !
        welcome to china
        !  
        ss@localcomputer:~$ 
        ```

   - 输出文件内容

     1. 文件内容

        ```shell
        ss@localcomputer:~/test$ cat grade 
        ID	name	grade
        1	tom	78
        2	jack	89
        ```

     2. 命令 `printf '%s' $(cat 文件名)`

        ```shell
        ss@localcomputer:~/test$ printf '%s' $(cat grade)
        IDnamegrade1tom782jack89ss@localcomputer:~/test$ 
        ```

     3. 命令 `printf '%s\n' $(cat 文件名)`，这也证实了 `$(cat grade)` 变量不是一个整体

        ```shell
        ss@localcomputer:~/test$ printf '%s\n' $(cat grade)
        ID
        name
        grade
        1
        tom
        78
        2
        jack
        89
        ss@localcomputer:~/test$
        ```

     4. 命令 `printf "$(cat 文件名)\n"`

        ```shell
        ss@localcomputer:~/test$ printf  "$(cat grade)\n"
        ID	name	grade
        1	tom		78
        2	jack	89
        ```

#### awk 命令

1. 介绍

   - 命令 `awk` 和 `cut` 一样都是对列进行筛选，而命令 `grep` 是对行进行筛选，但是命令 `cut` 有一些局限性（会严格遵循分割符分割列，如同 `df -h` 的信息使用 `cut` 就不能很好的筛选出来）
   - 命令 `awk` 包含 `cut` 的使用技巧，但是相对更加复杂，如果内容格式相对整齐，可以考虑使用 `cut` ，因为其方便简洁。
   - 默认是 __制表符 或 空格（无论几个空格连用）作为分隔符__

2. 命令格式

   - `awk '条件1{动作1} 条件2{动作2} 条件3{动作3}……' 文件名`
   - 条件（Pattern）
     1. __一般使用关系表达式作为条件__
     2. `x>10` ：判断变量是否大于 10
     3. `x<=10` ：判断变量是否小于等于 10
     4. `cat file | grep -v ID | awk '$2>=78{print $2}'` ，文件 file 中去掉含有 name 的行（一般是标题行），`awk` 命令中，比较 file 文件中第二行数据大于 78 的，大于输出屏幕上。
   - 动作（Action）
     1. 格式化输出
     2. 流程控制语句
   - BEGIN
     1. 输出一个多余的动作，在输出匹配内容之前输出 BEGIN 的内容
     2. 命令 `awk` 实际上是按行读入数据的，先读入一行数据，在进行判断及匹配，而 BEGIN 在读入第一行数据之前完成，所以下面的指定分割符要与 BEGIN 连用。
     3. `awk 'BEGIN{print "The is first worlds"} {print $1"\t"$3}' 文件名`
   - FS 内置变量（指定分割符，与 BEGIN 连用）
     1. 在匹配时指定分割符，与 BEGIN 连用，才能把第一行也正常匹配。
     2. `awk 'BEGIN{FS=":"} {print $1"\t"$3}' 文件名`
   - END
     1. 与 BEGIN 作用相同，在所有数据匹配完成，输出结束语。
     2. `awk 'END{print "end end end"} {print $1"\t"$3}' 文件名`
   - __注意__
     1. 虽然 `awk` 是列提取命令，但是其读入数据时，是按行读取的，下面实例中可以感觉出来
     2. `$0` 代表一整行、`$1` 代表第一列……
     3. 格式化输出命令 `printf` 或 `print`
     4. `awk` 支持 `printf` (不能自动换行，换行使用 `\n`) 和 `print`（可以自动换行）。`printf '\n'` 等价于 `print`

3. 实例

   - 编辑文件（制表符 tab 键）

     ```shell
     ss@localcomputer:~/test$ cat grade 
     ID	name	grade
     1	tom		78
     2	jack	89
     ```

   - 对文件进行列提取

     1. 命令 `awk '{printf $1"\t"$3"\n"}' 文件名` ，`$n` 表示第几列

        ```shell
        ss@localcomputer:~/test$ awk '{printf $1"\t"$3"\n"}' grade 
        ID	grade
        1	78
        2	89
        ```

     2. 使用 `BEGIN` ，命令 `awk 'BEGIN{print "these are id and grades"} {print $1"\t"$3}' grade `

        ```shell
        ss@localcomputer:~/test$ awk 'BEGIN{print "these are id and grades"} {print $1"\t"$3}' grade 
        these are id and grades			# 先输出指定字符串
        ID	grade
        1	78
        2	89
        ss@localcomputer:~/test$ 
        ```

   - 指定分割符，使用 `/etc/passwd` 文件

     1. 搜索含有 `/bin/bash` 的行，`cat /etc/passwd | grep /bin/bash`

        ```shell
        ss@localcomputer:~/test$ cat /etc/passwd | grep /bin/bash
        root:x:0:0:root:/root:/bin/bash
        ss:x:1000:1000:ss,,,:/home/ss:/bin/bash
        ```

     2. 以 `:` 作为分割符，提取第 1 列和第 6 列，`cat /etc/passwd | grep /bin/bash | awk 'BEGIN{FS=":"} {print $1"\t"$6}'`。__如果不写 BEGIN ，则文件内容第一行将不会处理。__

        ```shell
        ss@localcomputer:~/test$ cat /etc/passwd | grep /bin/bash | awk 'BEGIN{FS=":"} {print $1"\t"$6}'
        root	/root
        ss	/home/ss
        
        ```

   - 可以和管道符一起使用

     1. 命令 `df -h | awk '{printf $1"\t"$3"\t"$4"\n"}'` ，__可以进行提取但是输出的格式有些不规整__

        ```shell
        文件系统	已用	可用
        udev	0	957M
        tmpfs	1.8M	196M
        /dev/sda5	5.6G	5.3G
        tmpfs	0	986M
        tmpfs	4.0K	5.0M
        tmpfs	0	986M
        
        ```

   - 与 `grep` 和 `cut` 连用，命令 `df -h | grep /dev/sda5 | awk '{print $5}' | cut -d "%" -f 1`

     1. 命令 `df -h` ，各个分区使用情况

        ```shell
        ss@localcomputer:~/test$ df -h
        文件系统        容量  已用  可用 已用% 挂载点
        udev            957M     0  957M    0% /dev
        tmpfs           198M  1.8M  196M    1% /run
        /dev/sda5        12G  5.6G  5.3G   52% /
        tmpfs           986M     0  986M    0% /dev/shm
        tmpfs           5.0M  4.0K  5.0M    1% /run/lock
        ```

     2. 命令 `df -h | grep /dev/sda5` ，提取含有 /dev/sda 的行（就是设备文件名为 /dev/sda5）的分区信息

        ```shell
        ss@localcomputer:~/test$ df -h | grep /dev/sda5 
        /dev/sda5        12G  5.6G  5.3G   52% /
        ```

     3. 命令 `df -f | grep /dev/sda5 | awk '{print $5}'` ，提取 __已用%__ 这一列

        ```shell
        ss@localcomputer:~/test$ df -h | grep /dev/sda5 | awk '{print $5}'
        52%
        ```

     4. 命令 `df -h | grep /dev/sda5 | awk '{print $5}' | cut -d "%" -f 1` ，第 5 列有 % 号，去掉 % 号

        ```shell
        ss@localcomputer:~/test$ df -h | grep /dev/sda5 | awk '{print $5}' | cut -d "%" -f 1
        52
        ```

#### sed 命令

1. 介绍
   - `sed` 是一种几乎包括所有 UNIX 平台（包括 Linux） 的轻量级流编辑器。主要是用来将数据进行筛选、替换、删除、新增的命令。
   - sed 几乎就是一个编辑器（vim 只能对文件进行编辑），但 sed 可以对文件和管道符（流编辑）连用，对命令的结果再进行筛选，修改……。
   - 命令 `sed` 一般是与管道符 `|` 连用，不直接修改文件内容（容易出错）
   
2. 命令格式

   - `sed [选项] '[动作]' 文件名`

   - 选项

     1. `-n` ：一般 sed 命令会把所有数据都输出到屏幕上，如果加入此选项，则会把经过 sed 命令处理的行输出到屏幕上
     2. `-e` ：允许对输入数据应用多条 `sed` 命令编辑
     3. `-i` ：用 `sed` 的修改结果直接修改读取数据的文件（源文件会随之改变），而不是由屏幕输出

   - 动作

     | 符号 | 作用                                                         |
     | ---- | ------------------------------------------------------------ |
     | `a\` | 追加，在当前行后添加一行或多行。添加多行时，除最后一行外，每一行末尾都需要使用 `\` 表示数据未完结。 |
     | `c\` | 行替换，用 c 后面的字符串替换源数据行，替换多行时，除最后一行外，每一行末尾都需要使用 `\` 表示数据未完结。 |
     | `i\` | 插入，在当前行前插入一行或多行。出入数据时，除最后一行外，每一行末尾都需要使用 `\` 表示数据未完结。 |
     | `d:` | 删除，删除指定的行。                                         |
     | `p:` | 打印，输出指定的行。                                         |
     | `s:` | 字符串替换，用一个字符串替换另一个字符串。<br> 格式为 `行的范围s/旧字符串/新字符串/g` （与 vim 相似，表示 `%` 全部行） |

3. 实例

   - 编写测试文件（空格是 __制表符__ tab 键）

     ```shell
     ss@localcomputer:~/test$ cat grade 
     ID	name	grade
     1	tom		78
     2	jack	89
     3	marry	100
     4	kacy	67
     ```

   - 测试动作 `d` 删除指定行

     1. 命令 `sed '2,4d' grade` ，删除第二行 __到__第四行数据，但不修改文件本身。是不是用 `""` 、`''` 都可以

        ```shell 
        ss@localcomputer:~/test$ sed '2,4d' grade 
        ID	name	grade
        4	kacy	67
        # "" \ '' 都可以使用
        ss@localcomputer:~/test$ sed 2,4d grade 
        ID	name	grade
        4	kacy	67
        ss@localcomputer:~/test$ sed '2,4d' "grade"
        ID	name	grade
        4	kacy	67
        ss@localcomputer:~/test$ sed "2,4d" "grade"
        ID	name	grade
        4	kacy	67
        
        ```

   - 测试动作 `p` 打印指定行（一般与选项 `-n` 连用）

     1. 命令 `sed -n '2p' grade` ，打印指定行，选项 `-n` 只打印指定行

        ```shell
        ss@localcomputer:~/test$ sed -n "2p" "grade"
        1	tom	78
        ```

     2. 命令 `sed '2p' grade` ，第 2 行打印了两遍，在没有 `-n` 的选项

        ```shell
        ss@localcomputer:~/test$ sed "2p" "grade"
        ID	name	grade
        1	tom		78
        1	tom		78
        2	jack	89
        3	marry	100
        4	kacy	67
        ```

     3. __命令 `df -h sed -n '2p' grade` ，`sed` 与管道符连用__

        ```shell
        ss@localcomputer:~/test$ df -h | sed -n '4p'
        /dev/sda5        12G  5.6G  5.3G   52% /
        ```

   - 测试动作 `a` 在当前行后追加字符串；动作 `i` 在当前行前插入字符串

     1. 命令 `sed '2a default' grade` ，在第 2 行后追加 default

        ```shell
        ss@localcomputer:~/test$ sed '2a default' grade
        ID	name	grade
        1	tom	78
        default			# 追加的数据
        2	jack	89
        3	marry	100
        4	kacy	67
        ```

     2. 命令 `sed '2i one \`

        tow`' grade` ，在第 2 行前插入两行数据（`\` enter 表示此行命令没有输完，下行接着说）

        ```shell
        ss@localcomputer:~/test$ sed '2i one \
        > two' grade
        ID	name	grade
        one 				# 插入的两行数据
        two					# 插入的两行数据
        1	tom	78
        2	jack	89
        3	marry	100
        4	kacy	67
        ss@localcomputer:~/test$
        ```

   - 测试动作 `c` 数据整行替换；测试动作 `s` 字符串替换

     1. 命令 `sed '2c Nothing' grade` ，替换第 2 行整行数据

        ```shell
        ss@localcomputer:~/test$ sed '2c Nothing' grade
        ID	name	grade
        Nothing				# 第 2 行数据替换
        2	jack	89
        3	marry	100
        4	kacy	67
        ```

     2. 命令 `sed '3s/89/99/g' grade` ，替换第 3 行中 89 替换成 99 数据

        ```shell
        ss@localcomputer:~/test$ sed '3s/89/99/g' grade
        ID	name	grade
        1	tom		78
        2	jack	99		# 将 89 替换成 99
        3	marry	100
        4	kacy	67
        ```

   - 测试动作 `s` 与选项 `-i` 和 `-e` 连用

     1. 命令 `sed -i '3s/89/99/g' grade` ，操作的数据直接写入文件（屏幕不会输出）

     2. 命令 `sed -e '2s/78//g';'3/89//g' grade` ，同时把 2 行 、3 行的分数置空

        ```shell
        ss@localcomputer:~/test$ sed -e '2s/78//g;3s/89//g' grade
        ID	name	grade
        1	tom				# 置空
        2	jack	
        3	marry	100
        4	kacy	67
        ```

### 字符处理命令

##### 排序命令 sort

1. 排序命令 sort 介绍

   - 格式 `sort [选项] 文件名`
   - 选项（一般情况不怎么使用）
     1. `-f` ：忽略大小写
     2. `-n` ：以数值型进行排序，默认使用字符串型排序
     3. `-r` ：反向排序
     4. `-t` ：指定分割符，默认分割符为制表符
     5. `-k n[,m]` ：按照指定的字段范围排序。从第 n 字段开始，m 结束字段（默认到行尾）
   - 注意
     1. 排序的是文件内容，以行信息为排序单位，默认使用行首字符进行排序
     2. 可以与管道符连用
     3. 默认使用字母顺序进行排序（abcd）

2. 实例

   - 排序用户信息文件 `/etc/passwd`

     1. 命令 `sort /etc/passwd` ，会按照字母顺序进行排序

        ```shell
        root@localcomputer:~# sort /etc/passwd
        _apt:x:104:65534::/nonexistent:/usr/sbin/nologin
        avahi-autoipd:x:106:112:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/usr/sbin/nologin
        avahi:x:116:122:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/usr/sbin/nologin
        backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
        bin:x:2:2:bin:/bin:/usr/sbin/nologin
        ```

   - 反向排序，使用选项 `-r`

     1. 命令 `sort -r /etc/passwd`

   - 指定分割符，指定字段进行排序

     1. 命令 `sort -t ":" -k 3,3 /etc/passwd` ，指定 `:` 为分隔符，使用选项 `-k` 指定使用 `:` 分割的字段（`3,3` 意思只是用第 3 个字段进行排序）。用户文件 `/etc/passwd` 使用 `:` 分割的第三个字段为 __用户的 UID__

        ```shell
        root@localcomputer:~# sort -t ":" -k 3,3 /etc/passwd
        root:x:0:0:root:/root:/bin/bash
        daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
        uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
        systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
        ss:x:1000:1000:ss,,,:/home/ss:/bin/bash
        systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin
        syslog:x:102:106::/home/syslog:/usr/sbin/nologin
        ```

        __可以看出排序结果不没有按照我们之前的想法进行排序，UID=102 竟然排在 UID=1000 的后面，原因为并没有把 UID 当成数字来进行排序，而还是当作字段（使用第一个字符排序）__

     2. 为指定排序字段使用数字进行排序，使用 `-n` ，解决了数字较大排序在数字较小前面

        ```shell
        root@localcomputer:~# sort -n -t ":" -k 3,3 /etc/passwd
        root:x:0:0:root:/root:/bin/bash
        daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
        bin:x:2:2:bin:/bin:/usr/sbin/nologin
        sys:x:3:3:sys:/dev:/usr/sbin/nologin
        sync:x:4:65534:sync:/bin:/bin/sync
        games:x:5:60:games:/usr/games:/usr/sbin/nologin
        man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
        lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
        mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
        news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
        uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
        ```

##### 统计命令 wc

1. 统计命令 wc 介绍（word count）

   - 格式 `wc [选项] 文件名`
   - 选项
     1. `-l` ：只统计行数
     2. `-w` ：只统计单词数
     3. `-m` ：只统计字符数
   - 注意
     1. 可以与管道符连用

2. 实例

   - 统计 `/etc/passwd` 文件

     1. 命令 `wc /etc/passwd` ，显示所有统计内容

        ```shel
        root@localcomputer:~# wc /etc/passwd
          43   71 2497 /etc/passwd
          行数  单词 字符
        ```

     2. 指定统计内容 `wc -l /etc/passwd` 统计行数

        ```shell
        root@localcomputer:~# wc -l /etc/passwd
        43 /etc/passwd
        ```

   - 与管道符连用

     1. `df -h | wc -l`

        ```shell
        root@localcomputer:~# df -h | wc -l
        28
        ```

### 条件判断

#### 按照文件类型判断

1. 按照文件类型进行判断

   - 如表格（加粗为常用选项）

     | 测试选项      | 作用                                                         |
     | ------------- | ------------------------------------------------------------ |
     | `-b file`     | 判断该文件是否存在，并且判断是否为 __块设备文件__（是块设备文件为真） |
     | `-c file`     | 判断该文件是否存在，并且判断是否为 __字符设备文件__（是字符设备文件为真） |
     | __`-d file`__ | 判断该文件是否存在，并且判断是否为 __目录文件__（是目录文件为真） |
     | __`-e file`__ | 判断该文件是否 __存在__（存在为真）                          |
     | __`-f file`__ | 判断该文件是否存在，并且判断是否为 __普通文件__（是普通文件为真） |
     | `-L file`     | 判断该文件是否存在，并且判断是否为 __符号链接文件__（是符号链接文件为真） |
     | `-p file`     | 判断该文件是否存在，并且判断是否为 __管道文件__（是管道文件为真） |
     | `-s file`     | 判断该文件是否存在，并且判断是否为 __非空__（非空为真）      |
     | `-S file`     | 判断该文件是否存在，并且判断是否为 __套接字文件__（是套接字文件为真） |

2. 两种判断格式

   - 命令 `test -e /root` 判断文件是否存在（`echo $?` 显示上一条命令是否正确执行）

     ```shell
     root@localcomputer:~# test -e test/
     root@localcomputer:~# echo $?		# 0 表示正确
     0
     root@localcomputer:~# test -e t
     root@localcomputer:~# echo $?		# 1 表示错误
     1
     ```

   - 命令 `[ -e /root ]` 判断文件是否存在（注意中括号与内容之间有空格），__习惯使用此种方法__

     ```shell
     root@localcomputer:~# [ -e ./test/ ]
     root@localcomputer:~# echo $?
     0
     root@localcomputer:~# [ -e ./tes ]
     root@localcomputer:~# echo $?
     1
     root@localcomputer:~# 
     ```

3. 方便书写（不适用 `echo $?`）

   - 命令 `[ -d /root ] && echo "yes" || echo "no"` ，正确直接打印 yes，反之打印 no

     ```shell 
     root@localcomputer:~# [ -d /root ] && echo "yes" || echo "no"
     yes
     root@localcomputer:~# [ -f /root ] && echo "yes" || echo "no"
     no					# 文件存在但不是普通文件（目录）打印 no
     root@localcomputer:~# 
     ```

#### 按照文件权限判断

1. 按照文件权限判断

   - 如表格（加粗为常用）

     | 测试选项      | 作用                                                         |
     | ------------- | ------------------------------------------------------------ |
     | __`-r file`__ | 判断该文件是否存在，并且判断是否为拥有读权限（有读权限为真） |
     | __`-w file`__ | 判断该文件是否存在，并且判断是否为拥有写权限（有写权限为真） |
     | __`-x file`__ | 判断该文件是否存在，并且判断是否为拥有执行权限（有执行权限为真） |
     | `-u file`     | 判断该文件是否存在，并且判断是否为拥有 SUID 权限（有 SUID 权限为真） |
     | `-g file`     | 判断该文件是否存在，并且判断是否为拥有 SGID 权限（有 SGID 权限为真） |
     | `-k file`     | 判断该文件是否存在，并且判断是否为拥有 SBit 权限（有 SBit 权限为真） |

   - __此命令对文件的权限判断不会区分是哪个用户拥有某种权限，而是只要有某种权限就返回真__

2. 实例

   - 命令 `[ -r ./mbox ] && echo "yes" || echo "no"` 

     ```shell
     ss@localcomputer:~$ ls -ld mbox 
     -rw------- 1 ss ss 976 12月 28 22:33 mbox
     ss@localcomputer:~$ [ -r ./mbox ] && echo "yes" || echo "no"
     yes
     ```

#### 两文件之间进行比较

1. 两文件之间进行比较

   - 如表格

     | 测试选项                     | 作用                                                         |
     | ---------------------------- | ------------------------------------------------------------ |
     | `file1 -nt file2` (new than) | 判断文件 1 的修改时间是否比文件 2 的新（如果新为真）         |
     | `file1 -ot file2`            | 判断文件 1 的修改时间是否比文件 2 的旧（如果旧为真）         |
     | `file1 -ef file2`            | 判断文件 1 是否和 文件 2 的 Inode 号一致。可以理解为两个文件是否为同一个文件，这个判断用于判断硬链接是很好的办法。 |

2. 实例

   - 硬链接只能通过 Inode 号来识别（软链接有标志）使用 `-ef` 可以解决此问题

     1. 命令  `[ grade -ef test ] && echo "yes" || echo "no"`

        ```shell
        ss@localcomputer:~/test$ ln grade test		# 创建软链接
        ss@localcomputer:~/test$ ls -i 				# 显示 Inode 节点
        357 grade  357 test
        ss@localcomputer:~/test$ ls -l				# 显示权限
        总用量 8
        -rw-r--r-- 2 ss ss 55 2月  19 20:22 grade
        -rw-r--r-- 2 ss ss 55 2月  19 20:22 test
        ss@localcomputer:~/test$ [ grade -ef test ] && echo "yes" || echo "no"	# 测试 Inode 节点
        yes
        ```

#### 两个整数之间比较

1. 两个整数之间比较

   - 如图

     | 测试选项          | 作用                                           |
     | ----------------- | ---------------------------------------------- |
     | `整数1 -eq 整数2` | 判断整数 1 是否和整数 2 相等（相等为真）       |
     | `整数1 -ne 整数2` | 判断整数 1 是否和整数 2 不相等（不相等为真）   |
     | `整数1 -gt 整数2` | 判断整数 1 是否大于整数 2 （大于为真）         |
     | `整数1 -lt 整数2` | 判断整数 1 是否小于整数 2 （小于为真）         |
     | `整数1 -ge 整数2` | 判断整数 1 是否大于等于整数 2 （大于等于为真） |
     | `整数1 -le 整数2` | 判断整数 1 是否小于等于整数 2 （小于等于为真） |

2. 实例

   - 判断大小

     1. 命令 `[ 99 -gt 9 ] && echo "yes" || echo "no"` ，比较两数大小

        ```shell
        ss@localcomputer:~/test$ [ 99 -gt 9 ] && echo "yes" || echo "no"
        yes
        ss@localcomputer:~/test$ [ 99 -gt 100 ] && echo "yes" || echo "no"
        no
        ss@localcomputer:~/test$ 
        ```

#### 字符串的判断（常用）

1. 字符串的判断

   - 如表格

     | 测试字符               | 作用                                  |
     | ---------------------- | ------------------------------------- |
     | `-z 字符串`            | 判断字符串是否为空（空为真）          |
     | `-n 字符串`            | 判断字符串是否为非空（非空为真）      |
     | `字符串1 == 字符串2`   | 判断字符串 1 与 字符串 2 是否相等     |
     | `字符串1 ！== 字符串2` | 判断字符串 1 与 字符串 2 是否不等相等 |

2. 实例

   - 测试 `-z` 和 `-n`

     1. 删除变量 name， 测试 `[ -z "$name" ] && echo "yes" || echo "no"` ，测试 变量 name 为空，和非空（注意变量加双引号）

        ```shell
        ss@localcomputer:~/test$ unset name
        ss@localcomputer:~/test$ [ -z "$name" ] && echo "yes" || echo "no" 
        yes						# 空返回 yes
        ss@localcomputer:~/test$ [ -n "$name" ] && echo "yes" || echo "no"
        no						# 空返回 no
        ss@localcomputer:~/test$ 
        ```

#### 多重条件判断

1. 多重条件判断

   - 如表格

     | 测试选项          | 作用                                                 |
     | ----------------- | ---------------------------------------------------- |
     | `判断1 -a 判断2`  | 逻辑与，两个判断都成立，结果为真                     |
     | `判断1 -o 判断2`  | 逻辑或，有一个判断成立（两个都成立也可以），结果为真 |
     | `! 判断` （空格） | 逻辑非，与原始判断结果相反                           |

2. 实例

   - 测试（变量 name 为空）

     1. 命令 `[ 22 -gt 10 -a -n "$name" ] && echo "yes" || echo "no"`

     2. 命令 `[ 22 -gt 10 -o -n "$name" ] && echo "yes" || echo "no"`

     3. 命令 `[ ! -n "$name" ] && echo "yes" || echo "no"` ，注意 `! 判断` 空格

        ```shell
        ss@localcomputer:~/test$ [ 22 -gt 10 -a -n "$name" ] && echo "yes" || echo "no"
        no
        ss@localcomputer:~/test$ [ 22 -gt 10 -o -n "$name" ] && echo "yes" || echo "no"
        yes
        ss@localcomputer:~/test$ [ ! -n "$name" ] && echo "yes" || echo "no"
        yes
        ss@localcomputer:~/test$ 
        
        ```

### 流程控制

 #### if 语句

__说明：判断语句 `[ 条件 ]`  `if` 的用法作为一个程序员应该了解 __

##### if 单分支

1. 单分支 if 条件语句介绍

   - 格式 1

     ```shell
     if [ 条件判断 ];then
     	程序
     fi
     ```

   - 格式 2

     ```shell
     if [ 条件判断 ]
     	then
     		程序
     fi
     ```

   - 单分支条条件语句要注意几点

     1. `if` 语句使用 `fi` （finish）结尾，和一般语言使用大括号结尾不同
     2. `[ 条件判断式 ]` 就是使用 `test` 命令判断，所以中括号和条件判断式之间 __必须有空格__
     3. `then` 后面跟符合条件之后执行的程序，可以放在 `[]` 之后，用 `;` 分割，也可以换行写入，就不需要 `;`

2. 实例

   - 判断分区使用率

     1. 脚本如下

        ```shell
        #!/bin/bash
        # 统计根分区使用率
        # author:       email:
        
        # 把根分区的使用率作为变量赋予变量 rate
        rate=$(df -h | grep "dev/sda5" | awk '{print $5}' | cut -d "%" -f 1)
        
        # 使用率大于等于 50% 就开始警告！！！
        if [ "$rate" -ge 50 ]
            then
                echo "Warning! /dev/sda5 is full!!!"
        fi
        ```

     2. 修改脚本权限

        ```shell
        ss@localcomputer:~/test$ chmod 755 jiaoben 
        ```

     3. 执行脚本

        ```shell
        ss@localcomputer:~/test$ ./jiaoben 
        Warning! /dev/sda5 is full!!!
        ```


##### if 双分支

1. 双分支 if 条件语句

   - 格式

     ```shell
     if [ 条件判断式 ]
     	then
     		条件成立时，执行程序
     	else
     		条件不成立时，执行程序
     fi
     ```

2. 实例

   - 判断 apache 是否启动

     1. 脚本

        ```shell
        #!/bin/bash
        # author:       email:
        
        # 使用 nmap 命令扫描服务器，并截取 apache 服务状态，赋予变量port
        # 此处就是筛选 nmap 命令的输出结果的，很简单（看一下 nmap 的输出即可）
        port=$(nmap -sT | grep "tcp" | grep "http" | awk '{print $2}')
        
        if [ "$port" == "open" ]
            then
            	# 重定向输入文件中
                echo "$(date) http is ok!" >> /tmp/apa.log
            else
                # 启动 apache 服务命令
                /etc/rc.d/init.d/httpd start &> /dev/null
                # 重定向输入文件中
                echo "$(date) restart httpd!!" >> /tmp/apa_err.log
        fi
        ```

     2. 命令 `nmap` 远程扫描工具（感觉很厉害的工具包）， [参考地址 1 ](https://www.cnblogs.com/nmap/p/6232207.html) 、[参考地址 2 ](https://www.cnblogs.com/nmap/p/6232969.html)

        - 此处使用 `nmap -sT 主机IP` ，`sT` 扫描指定服务上开启的 tcp 端口，state 为 open 为正常

          ```shell
          ss@localcomputer:~$ nmap -sT 192.168.43.87 
          
          Starting Nmap 7.60 ( https://nmap.org ) at 2020-02-21 21:17 CST
          Nmap scan report for localcomputer (192.168.43.87)
          Host is up (0.00012s latency).
          Not shown: 997 closed ports
          PORT      STATE SERVICE
          22/tcp    open  ssh
          25/tcp    open  smtp
          10000/tcp open  snet-sensor-mgmt
          
          Nmap done: 1 IP address (1 host up) scanned in 0.09 seconds
          ss@localcomputer:~$ 
          
          ```
     
     3. 定时运行 apache 服务

##### if 多分支

1. 多分支 if 条件语句

   - 格式

     ```shell
     if [ 条件判断式1 ]
     	then
     		当条件判断式1成立时，执行程序1
elif [条件判断2]
     	then
     		当条件判断式2成立时，执行程序2
     …………more…………
     else
     	当所有条件判断式都不成立时，执行程序
     fi		
     ```
     
   
2. 实例

   - 测试系统中文件的类型

     1. 脚本

        ```shell
        #!/bin/bash
        # author:       email:  
        
        # 接受键盘的输入，赋予变量 file
        read -p "Please input a filename:" file
        
        # 判断 file 变量是否为空
        if [ -z "$file" ]
            then
                echo "Error,please input a filename"
                exit 1
        # 判断 file 文件是否存在
        elif [ ! -e "$file" ]
            then
                echo "Input is not a file"
                exit 2
        # 判断 file 是否为普通文件
        elif [ -f "$file" ]
            then
                echo "A file of regulare"
        # 判断 file 是个目录
        elif [ -d "$file" ]
            then
                echo "Input is a directory"
        # 其他文件类型
        else
                echo "An other file"
        fi
        ```

#### case 语句

1. 多分支 case 条件语句介绍

   - `case` 语句和 `if...elif...else` 一样多是多分支条件语句，不过和 `if` 多分枝语句不同的是，`case` 语句只能判断一种条件关系，而 `if` 语句可以判断多种条件关系。

   - 格式

     ```shell
     case "$变量名" in
     	"值1")
     		如果变量值等于值1，执行程序1
     		;;
     	"值2")
             如果变量值等于值1，执行程序2
             ;;
         *)
     		如果变量值等于值1，执行程序1
     		;;
     esac
     ```

2. 实例

   - 随意的一个脚本

     1. 脚本

        ```shell
        #!/bin/bash
        # author:       email:
        
        # 输入 yes or no
        # 选项 -t 30，输入界面等待用户 10 秒，时间过后脚本自动执行（没有输入）
        read -p "Please yes or no: " -t 10 cho
        
        case "$cho" in
            "yes")
                echo "choose is yes"
                ;;
            "no")
                echo "choose is no"
                ;;
            *)
                echo "error"
                ;;
        esac  
        ```

     2. `chmod 755 文件名` ，修改文件执行权限

     3. `./文件名` ，执行文件

        ```shell
        ss@localcomputer:~/test$ ./tesany 
        Please yes or no: no
        choose is no
        ```

#### for 循环

##### 语法一

1. for 循环语法一介绍

   - 格式（感觉和 python 好相像）

     ```shell
     for 变量 in 值1 值2 值3……
     	do
     		程序
     	done
     ```

2. 实例

   - 打印数字 abcd，`for` 循环 4 次

     1. 脚本

        ```shell
        #!/bin/bash
        # author:       email:
        
        for x in a b c d
            do
                echo -e "$x"
            done     
        ```

     2. 执行结果

        ```shell
        ss@localcomputer:~/test$ ./tesfor 
        a
        b
        c
        d
        ```

   - 批量解压缩脚本

     1. 脚本

        ```shell
        #!/bin/bash
        # author:       email:
        
        # 切换目录
        cd /home/ss/train
        
        # 将压缩包文件名写入文件中
        ls *.tar.gz > ls.log
        
        # 循环解压缩
        for x in $(cat ls.log)
            do
                tar -zxvf $x > /dev/null
            done
        
        rm -rf /test/ls.log                     
        ```

##### 语法二

1. for 循环语法二介绍

   - 格式

     ```shell
     for ((初始值;循环控制变量;变量变化))
     	do
     		程序
     	done
     ```

2. 实例

   - 从 1 加到 100，在 shell 脚本中只有 `(())` 中可以进行数学运算

     1. 脚本

        ```shell
        #!/bin/bash
        # author:       email:
        
        sum=0
        
        for ((i=0;i<=100;i=i+1))
            do
                sum=$(("$sum" + "$i"))
            done
        
        echo "This is sum(1+2+3...+100): $sum"
        
        ```

     2. 执行结果

        ```shell
        ss@localcomputer:~/test$ ./tesfor1
        This is sum(1+2+3...+100): 5050
        ```

#### while 循环与 until 循环

##### while 循环

1. while 循环介绍

   - `while` 循环是不定循环，也称作条件循环。只要条件判断式成立，循环就会一直继续，直到条件判断式不成立，循环才会停止。就是和 `for` 的固定循环不太一样

   - 格式

     ```shell
     while [ 条件判断式 ]
     	do
     		程序
     	done
     ```

2. 实例

   - 从 1 加到 100

     1. 脚本

        ```shell
        #!/bin/bash
        # author:       email:
        
        i=0
        sum=0
        
        # 如果变量 i 小于 100 继续执行
        while [ "$i" -le 100 ]
            do
                sum=$(("$sum" + "$i"))
                i=$(("$i" + 1))
        
            done
        
        echo "sum=$sum"
        ```

     2. 执行结果

        ```shell
        ss@localcomputer:~/test$ chmod 755 teswhile 
        ss@localcomputer:~/test$ ./teswhile 
        sum=5050
        ```

##### until 循环

1. until 循环介绍

   - `until` 循环，和 `while` 循环相反，`until` 循环时只要条件判断时不成立进行循环，并执行循环程序。一旦循环条件成立，则终止循环

   - 格式

     ```shell
     until [ 条件判断式 ]
     	do
     		程序
     	done
     ```

2. 实例

   - 从 1 加到 100

     1. 脚本

        ```shell
        #!/bin/bash
        # author:       email:
        
        i=0
        sum=0
        
        # 如果变量 i 大于 100 不成立，继续执行
        until [ "$i" -gt 100 ]
            do
                sum=$(("$sum" + "$i"))
                i=$(("$i" + 1))
        
            done
        
        echo "sum=$sum"
        ```


## Linux 服务管理

### 服务简介与分类

##### 服务类别介绍

1. 服务分类介绍

   - 如图

     ![Linux服务分类](git_picture/Linux服务分类.jpg)

   - __服务分类__ 与 __软件包分类__ 相同都是分为 RPM 包（Linux 的发性版本有关）和源码包，所以其中特点也不尽相同。

   - 应用软件与服务是不相同的（具体解释不太清除）

   - RPM 包服务分为两种

     1. RPM 包一般为默认服务，如同系统服务一样。Linux 中的服务大部分都是独立的。
     2. 独立服务：服务在一直在内存中，当有用户访问，服务直接相应用户。相应快速，但消耗资源
     3. 基于 xinetd 服务：xinetd 服务本身为独立服务，一直在内存中，但 xinted 本身没有服务功能，它只是管理者其他服务。当访问由 xinetd 管理的服务时，先访问 xinetd，再由 xinetd 访问服务（因为被 xinetd 管理的服务没有在内存中）。响应相对较慢，但不消耗资源。

##### 启动与自启动区别

1. 启动与自启动

   - 服务启动

     1. 在当前系统中让服务运行，并提供服务

   - 服务自启动

     1. 自启动是指让系统开机或重启之后，随着系统的启动而自动启动服务

   - 以 Windows 服务为例

     1. 查看系统上的服务`ctrl+R` 输入 `cmd` 调用窗口 输入 `compmgmt.msc` 调用计算机管理，选择服务。如图

        ![Windows服务](git_picture/Windows服务.png)

     2. 查看服务的各个属性

        ![Windows服务属性](git_picture/Windows服务属性.png)

     3. __状态：启用、停止、暂停、恢复__

     4. __启动类型：手动启动 与 自动启动__

##### systemctl 与老命令的区别

1. 查询已安装的服务

   - RPM 包安装服务

     1. 查看服务自启动状态，__可以看到所有 RPM 包安装服务__

        `chkconfig --list`

   - 源码包安装的服务

     1. 查看服务安装位置，一般是 `usr/local`

   - > Systemd 初始化进程服务（`systemctl` 命令，可视为 `service` 与 `chkconfig` 命令的组合体）替换了较为 __古老__ 的 System V init 初始化进程服务。__需要注意的是：Systemd 中，服务名大都以 `.service`  结尾，一般在System V init 中的服务名加上 `.service` 就是在Systemd中的服务名。__
     >
     > [参考地址 1 ](https://blog.csdn.net/AsWeDo/article/details/90345065)
     >
     > > systemctl 是系统服务管理器命令，它实际上将 service 和 chkconfig 这两个命令组合到一起。
     > >
     > > | 任务                                                         | 旧指令                          | 新指令                                                       |
     > > | ------------------------------------------------------------ | ------------------------------- | ------------------------------------------------------------ |
     > > | 使某服务自动启动                                             | `chkconfig --level 3 httpd on`  | `systemctl enable httpd.service`                             |
     > > | 使某服务不自动启动                                           | `chkconfig --level 3 httpd off` | `systemctl disable httpd.service`                            |
     > > | 检查服务状态                                                 | `service httpd status`          | `systemctl status httpd.service` （服务详细信息）<br> `systemctl is-active httpd.service` （仅显示是否 Active) |
     > > | 显示所有已启动的服务（查看各个启动级别下服务的开机自动启动与禁用情况） | `chkconfig --list`              | `systemctl list-units --type=service`                        |
     > > | 启动某服务                                                   | `service httpd start`           | `systemctl start httpd.service`                              |
     > > | 停止某服务                                                   | `service httpd stop`            | `systemctl stop httpd.service`                               |
     > > | 重启某服务                                                   | `service httpd restart`         | `systemctl restart httpd.service`                            |
     > > | 查看所有已安装的服务                                         |                                 | `systemctl list-unit-files`                                  |
     > >
     > > [参考地址 2 ](http://linux.51yip.com/search/systemctl)

2. RPM 安装服务与源码包安装服务的区别

   - RPM 安装服务与源码包安装服务的区别就是 __安装位置的不同__
   - 源码包安装位置在指定位置，一般是 `/usr/local`
   - RPM 包安装在默认位置（系统的其他命令可以找到）
     1. RPM 包的配置文件一般在 `/etc/` 目录下
     2. RPM 包的启动文件一般在 `/etc/init.d/` 目录下（Ubuntu 是 `/etc/init.d` ，当然 Ubuntu 的包管理软件不是 RPM）
   - 因为安装位置的不同，所以导致管理方式不同
     - `systemctl` 命令搜索的是固定位置，所以使用 RPM 包安装的位置固定，命令可以搜索到（默认）
     - 源码包安装位置一般为 `/usr/local` 不是，`systemctl` 命令搜索位置（默认）

### RPM 包安装服务的管理

#### 独立服务管理

##### RPM 包的安装位置

1. RPM 包的安装位置一般有以下位置

   - 如表

     | 位置               | 作用                       |
     | ------------------ | -------------------------- |
     | `/etc/init.d/`     | 独立服务启动脚本位置       |
     | `etc/sysconfig/`   | 初始化环境配置文件内容     |
     | `/etc/`            | 配置文件                   |
     | `/etc/xinetd.conf` | xinetd 配置文件            |
     | `/etc/xinetd.d/`   | 基于 xinetd 服务的启动脚本 |
     | `/var/lib/`        | 服务产生的数据存放在这     |
     | `/var/log/`        | 日志                       |

##### 启动

1. 启动服务
   - 格式 1
     1. 命令 `/etc/init.d/独立服务名 start|stop|status|restart`
     2. 这个命令就是 `绝对路径+命令` 
   - 格式 2
     1. 命令 `systemctl start|stop|status|restart http.service(独立服务名)`
     2. 这个命令是 `systemctl` 的，废弃了 `service` 和 `chkconfig` 的合体

##### 自启动

1. 查看所有已安装的服务（也可以查看服务是否为自启动）
   - 命令
     1. `systemctl list-units-files`
2. 设置服务自启动
   - 格式 1
     1. 命令 `systemctl enable http.service(独立服务名)`
     2. 此命令只是设置自启动，当前系统是否启动与此无关
   - 格式 2（centOS 系统中）
     1. 修改 `/etc/rc.d/rc.local` 文件
     2. 当系统重启时，所有服务都启动时，在输入密码之前，执行此文件，所以在此文件中写入命令将会被执行
     3. 文件中命令还可以 是 `touch /start_time`  ，可以记录每次开机的时间（`touch` 创建文件，当文件存在时会修改文件的最后访问时间）
   - 格式 3（centOS 系统中）
     1. 使用 `ntsysv` 命令管理自启动
     2. 可以管理 RPM 包的两种服务的任何一种
3. 设置服务不自动
   - 格式
     1. 命令 `systemctl disable http.service(独立服务名)`

#### 基于 xinetd 服务的管理

__说明：xinetd 是 超级守护进程，现在基于 xinetd 的服务越来越少了，了解以下即可1__

1. 安装 xinetd 与 telnet

   - 安装 xinetd 格式
     1. `yum -y install xinetd`
   - 安装 telnet 格式
     1. `yum -y istall telnet-server`

2. xinetd 服务本身是独立服务，常驻内存

   - 所管理的服务响应相对独立服务较慢

3. xinetd 服务的启动

   - 修改在 `/etc/xinetd.d/telnet` 文件

   - `/etc/xinetd.d/telent` 格式

     ```shell
     service telnet		# 服务的名称为 telent
     {
     	flags    =REUSE        # 标志为 REUSE，设定 TCP/IP socket 可重用
     	socket_type    =stream        # 使用 TCP 协议数据包
     	wait    =no        # 允许多个连接多个同时连接
     	user    =root        # 启动服务的用户为 root
     	server    =/usr/sbin/in.telnet        # 服务的启动程序
     	log_no_failure    +=USERID        # 登录失败后，记录用户的 ID
     	disable    =no        # 服务不启动 [disable    =yes，服务启动] 
     }
     ```

   - 重启 xinetd 服务

     1. 命令 `systemctl restart xinetd`
     2. 因为 telnet 服务是基于 xinetd 服务的管理服务，所以没有独立的 telnet 服务。重启了 xinetd 服务，就启动了 telnet 服务

4. xinetd 服务的自启动

   - xinetd 得启动与自启动相同，启动就是自启动。

   - 格式
     1. `chkconfig telnet on` ，这个需要查询，怎么开启自启动这种服务

### 源码包安装服务管理

1. 源码包安装服务的启动

   - 使用绝对路径，调用启动脚本来启动。不同得源码包的启动脚本不同。__可以查看源码包的安装说明，查看脚本的方法。__
   - 如不同的服务启动脚本不同，例如 apache 服务
     1. `/usr/local/apache/bin/apachectl start|stop` ，绝对路径 + 开启与关闭

2. 源码包服务的自启动（推荐使用）

   - 修改 `/etc/rc.d/rc.local` 文件，加入 `/usr/local/apache/bin/apachectl start` 命令

3. 让源码包服务被服务管理命令识别（不推荐使用）

   - 让源码包的 apache 服务能被 service 命令管理启动
   - 使用软链接，将服务启动文件链接到 `/etc/init.d`
     1. 命令 `ln -s /usr/local/apache/bin/apachectl /etc/init.d/apache` 

4. 让源码包的 apache 服务能被 chkconfig 与 ntsysv 命令管理自启动（centOS 系统版本 7 以下，Ubuntu 没有 ntsysv）

   - 修改 `/etc/init.d/apache` 文件，加两句命令

     1. 指定 httpd 脚本可以被 chkconfig 命令管理。格式是：`chkconfig:运行级别 启动顺序 关闭顺序`

        `# chkconfig:35 86 76`，运行级别为 3 和 5，启动关闭顺序不能系统相同（`/etc/rc0.d` 目录的文件）

     2. 说明，内容随意

        `description:source package apache`

   - `/etc/rc0[1,2,3,4,5,6].d` 为系统启动级别

   - 将 apache 加入 `chkconfig` 命令

     `chkconfig --add apache`

## Linux 系统管理

### 进程管理

#### 进程查看

##### 查看系统中所有被进程

1. 进程介绍
   
   - 进程是正在执行的一个程序或命令，每一个进程都是一个运行的实体，都有自己的地址空间，并占用一定的系统资源
   
2. 进程管理的作用

   - 判断服务器健康状态
     1. 这是进程管理的最主要工作
   - 查看系统中所有进程
   - 杀死进程
     1. 不要随意使用 `kill` 杀死进程，要尝试正确关闭进程

3. 查看系统中所有进程（`man ps` 查看帮助手册）

   - 查看系统中所有进程，使用 BSD（unix） 操作系统格式（主要使用方式）

     1. 命令 `ps aux`

        - `a` 表示前台进曾
        - `u` 表示哪个用户产生的进程
        - `x` 表示后台进程

     2. 解释命令输出

        ```shell
        ss@localcomputer:~$ ps aux
        USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
        root          1  0.0  0.2 225532  5476 ?        Ss   13:58   0:03 /sbin/init splash
        root          2  0.0  0.0      0     0 ?        S    13:58   0:00 [kthreadd]
        root          3  0.0  0.0      0     0 ?        I    13:58   0:00 [kworker/0:0]
        
        # USER：启动进程的用户
        # PID：进程的 ID 号，init 是系统第一个进程是其他进程的父进程
        # %CPU：该进程占用 CPU 资源的百分比，占用越高，进程越消耗资源
        # %MEN：该进程占用物理内存的百分比，占用越高，进程越消耗资源
        # VSZ：该进程占用虚拟内存的大小，单位 KB
        # RSS：该进程占用实际物理内存大小，单位 KB
        # TYY：该进程是在哪个终端中运行的。其大小 tty1-tty7 代表本地终端，tty1-tty6 是本地的字符界面终端，tty7 是图像终端（init 5）。pts/0-255 代表虚拟终端（远程登录）。? 号是表示系统调用的。
        #ctrl+alt+f[1,2,3,4,5,6] 切换运行界面（Ubuntu 中 1 表示图像终端）
        
        # STAT：进程状态。常见的状态有：R 运行；S 睡眠；T 停止状态；s 包含子进程；+ 位于后台运行
        # START：该进程启动时间
        # TIME：该进程占用 CPU 的运算时间，注意不是系统时间
        # COMMAND：产生此进程的命令名
        ```

   - 查看系统中所有进程，使用 Linux 标准命令格式

     1. 命令 `ps -le`
        - `-l` 显示详细信息
        - `-e` 显示多有进程

##### 查看系统健康状况

1. 查看系统健康状态

   - 命令格式 `top [选项]`
   - 选项
     1. `-d` ：秒数，指定 top 命令每个几秒更新。默认是 3 秒

   - 在 `top` 命令的交互模式当中可以执行的命令
     1. `?` 或 `h` ：显示交互模式的帮助
     2. `P` ：以 CPU 的使用率排序，默认
     3. `M` ：以内存的使用率排序
     4. `N` ：以 PID 排序
     5. `q` ：退出 `top`

2.  top 命令输出介绍

   - __`top` 命令默认以 3 秒刷新一次，其主要信息为前 5 行__

   - 介绍前五行输出信息

     ```shell
     top - 16:27:05 up 30 min,  2 users,  load average: 0.19, 0.08, 0.05
     任务: 282 total,   1 running, 213 sleeping,   0 stopped,   0 zombie
     %Cpu(s):  1.3 us,  0.9 sy,  0.1 ni, 97.6 id,  0.1 wa,  0.0 hi,  0.0 si,  0.0 st
     KiB Mem :  2017328 total,   207240 free,  1371996 used,   438092 buff/cache
     KiB Swap:   976892 total,   952560 free,    24332 used.   477956 avail Mem 
     
     进程 USER      PR  NI    VIRT    RES    SHR �  %CPU %MEM     TIME+ COMMAND                   
       3254 ss        20   0  835788  55984  42688 S  12.5  2.8   0:00.77 gnome-terminal-           
       2911 ss        20   0 3481152 214456  99720 S   6.2 10.6   0:12.56 gnome-shell       
     ```

     1. 第一行信息为任务队列信息

        | 内容                           | 说明                                                         |
        | ------------------------------ | ------------------------------------------------------------ |
        | 16：27：05                     | 系统当前时间                                                 |
        | up 30 min                      | 系统的运行时间。本机以运行 30 分钟                           |
        | 2 users                        | 当前登录两个用户                                             |
        | load average: 0.19, 0.08, 0.05 | 当前系统之前 1 分钟、5 分钟、15 分钟的平均负载 <br> 一般认为小于 1 时，负载较小，大于 1 系统以超过负荷（1 这个临界点时是相对单核 CPU，双核 CPU 可以是 2……） |

     2. 第二行为进程信息
     
        | 内容            | 说明                                                         |
        | --------------- | ------------------------------------------------------------ |
        | 任务: 282 total | 系统中进程总数                                               |
        | 1 running       | 正在运行进程数                                               |
        | 213 sleeping    | 睡眠的进程数                                                 |
        | 0 stopped       | 已停止的进程数                                               |
        | 0 zombie        | 僵尸（将死）进程。进程正在终止，但是没有终止完全。__如果不是 0，需要手工检查__ |
     
     3. 第三行为 CPU 信息
     
        | 内容             | 说明                                                         |
        | ---------------- | ------------------------------------------------------------ |
        | %Cpu(s):  1.3 us | 用户模式占用的 CPU 百分比 <br> 用户占用                      |
        | 0.9 sy           | 系统模式占用 CPU 的百分比 <br> 系统占用                      |
        | 0.1 ni           | 改变过优先级的用户进程占用的 CPU 百分比 <br> 改变优先级进程（不太清楚） |
        | 97.6 id          | 空闲 CPU 的百分比 <br> 主要观察                              |
        | 0.1 wa           | 等待输入/等待输出的进程的占用 CPU 百分比                     |
        | 0.0 hi           | 硬中断请求服务占用的 CPU 百分比                              |
        | 0.0 si           | 软中断请求服务占用的 CPU 百分比                              |
        | 0.0 st           | st（steal time）虚拟时间百分比。就是当有虚拟机时，虚拟 CPU 等待实际 CPU 的时间百分比 |
     
     4. 第四行为物理内存信息（给 Linux 分配了 2 GiB 内存，实际大约可用为 1.9 GiB）
     
        | 内存                     | 说明                                                         |
        | ------------------------ | ------------------------------------------------------------ |
        | KiB Mem :  2017328 total | 单位 KB，物理内存的总量 <br> 我分配虚拟机 2 GiB 内存，可使用大约 1.9 GiB |
        | 207240 free              | 空闲的物理内存量 <br> 大约 0.19 GiB                          |
        | 1371996 used             | 已使用的物理内存量 <br> 大约 1.3 GiB                         |
        | 438092 buff/cache        | 作为缓冲的内存量 <br> 大约 4.1 GiB                           |
     
     5. 第五行为交换分区（swap）信息（给 Linux 的 swap忘了，大约为 1 GiB，可用 （950 MiB）
     
        | 内容                     | 说明                                     |
        | ------------------------ | ---------------------------------------- |
        | KiB Swap:   976892 total | 单位 KiB，交换分区（虚拟内存）的总体大小 |
        | 952560 free              | 已使用交换分区的大小                     |
        | 24332 used               | 空闲交换分区大小                         |
        | 477956 avail Mem         | 作为缓存的交换分区的大小                 |
     
     6. 内存参考（内存和 swap --》4 行、5 行）
     
        ```shell
        ss@localcomputer:~$ free -h
                      总计         已用        空闲      共享    缓冲/缓存    可用
        内存：        1.9G        1.3G        144M         12M        505M        481M
        交换：        953M         14M        939M
        ```
     
   - 以下信息和 `ps aux` 几乎相同，只是根据硬件（CPU、Memery）使用率排序
   
     1. `shift+m` ，就是大写 M，会以内存使用率进行排序
     2. 大写 P，会以 CPU 使用率排序
   
3. 注意 `top` 命令

   - __`top` 命令本身比较消耗资源，所以不要经常使用__
   - 使用 `q` 退出

##### 查看进程树

1. 命令 `pstree [选项]`

   - 选项
     1. `-p` ：显示进程的 PID
     2. `-u` ：显示进程的所属用户

2. 实例

   - 命令 `pstree`

     ```shell
     ss@localcomputer:~$ pstree
     systemd─┬─ManagementAgent───6*[{ManagementAgent}]	# 6* 表示此进程有 6 个子进程
             ├─ModemManager───2*[{ModemManager}]
             ├─NetworkManager───2*[{NetworkManager}]
             ├─VGAuthService
     ```

   - 命令 `pstree -p` 可以查看进程的 PID，__所有的进程__

     ```shell
     systemd(1)─┬─ManagementAgent(1259)─┬─{ManagementAgent}(1270)	# 显示所有进程，包括子进程和 PID
                │                       ├─{ManagementAgent}(1271)
                │                       ├─{ManagementAgent}(1272)
                │                       ├─{ManagementAgent}(1273)
                │                       ├─{ManagementAgent}(1275)
                │                       └─{ManagementAgent}(1276)
                ├─ModemManager(803)─┬─{ModemManager}(830)
                │                   └─{ModemManager}(842)
     ```

   - 从上述命令输出结果可以看出，进程 systemd 是所有进程的父进程（Ubuntu 系统中）

#### 进程管理

##### 终止进程

__说明：对进程的终止，最好使用程序的终止命令，如 `systemd` 的命令，只有在正常关闭命令不起作用时，使用此命令__

1. `kill` 命令

   - 查看可用的进程信号

     1. `kill -l`

   - 信号说明

     | 信号代码 | 信号名称 | 说明                                                         |
     | -------- | -------- | ------------------------------------------------------------ |
     | 1        | SIGHUP   | 该信号让进程立即关闭，然后重新读取配置文件之后立即重启 <br> __重启__ |
     | 2        | SIGINT   | 程序终止信号，用于终止前台程序。相当于输出 `ctrl+c` 快捷键   |
     | 8        | SIGFPE   | 在发生致命的算数运算错误时发出，不仅包括浮点运算错误，还包括溢出以及除数为 0 等其他所有算数错误 |
     | 9        | SIGKILL  | 用于立即结束程序的运行，本信号不能被堵塞、处理和忽略。一般用于强制终止程序 <br> __强制终止__ |
     | 14       | SIGALRM  | 时钟定时信号，计算的是实际的时间或时钟时间，alarm 函数使用该信号 |
     | 15       | SIGTERM  | 正常结束进程的信号，`kill` 命令的默认信号。有时如果进程已经发生问题，这个信号是无法正常终止程序的，我们才会尝试 `SIGKILL` 信号，也就是信号 9 <br> __正常终止__ |
     | 18       | SIGCONT  | 该信号可以让暂停的进程恢复执行，本信号不能被堵塞             |
     | 19       | SIGSTOP  | 该信号可以暂停前台进程，相当于输入 `ctrl+z` 快捷键。本信号不能被堵塞 |

   - `kill` 命令格式
   
     1. 正常终止进程 `kill -15 pid`
     2. 重启进程 `kill -1 pid`
     3. 强制终止进程 `kill -9 pid``
   
   - `kill` 使用注意
   
     1. 进程有父进程与子进程，可以单独终止子进程，如果终止父进程相关子进程也会终止
     2. 如遇不了解的进程不要随意终止，应先清除进程的作用，因为大部分的进程都是系统进程
     3. 必须使用进程的 PID，不要使用进程名称
     4. 如上介绍的是命令的信号，不是命令的选项
   
2. `killall` 命令介绍

   - 按照进程名终止进程格式
     1. `kill [选项] [信号] 进程名`
     2. 可以看出使用进程名，顾名思义杀死这个进程（直接杀死父进程）
     3. 信号作用与 `kill` 命令信号相同
   - 选项
     1. `-i` ：进入交互式，询问是否要杀死该个进程
     2. `-I` ：忽略进程名的大小写

3. `pkill` 命令

   - 按照进程名终止格式

     1. `pkill [选项][信号]进程名`
     2. 与 `killall` 相同

   - 选项

     1. `-t` ：中端号，按照终端号踢出用户

   - 实例演示 `-t` 作用

     1. 使用 `w` 命令查看系统登录用户（一个本地终端 tty2，一个远程终端 pts/1）

        ```shell
        root@localcomputer:~# w
         14:33:29 up  1:06,  2 users,  load average: 0.24, 0.06, 0.02
        USER     TTY      来自           LOGIN@   IDLE   JCPU   PCPU WHAT
        ss       tty2     tty2             13:28    1:06m 44.43s  0.19s update-notifier
        ss       pts/1    192.168.43.17    14:33    6.00s  0.01s  0.01s -bash
        ```

     2. 使用 `pkill -9 -t pts/1` 或 `pkill -t -9 pts/1` 都可以，成功踢掉远程登录

        ```shell
        root@localcomputer:~# pkill -9 -t pts/1
        root@localcomputer:~# w
         14:37:53 up  1:10,  1 user,  load average: 0.01, 0.03, 0.00
        USER     TTY      来自           LOGIN@   IDLE   JCPU   PCPU WHAT
        ss       tty2     tty2             13:28    1:10m 47.95s  0.19s update-notifier
        ```

### 工作管理

1. 工作管理介绍

   - Linux 中让允许把进程放入后台执行
   - Windows 最小化就是放入后台执行

2. 把进程放入后台

   - 第一种方法
     1. `执行的命令 &` ，在执行命令后加上 `&`
     2. 此方式，程序在后台 __继续执行__
   - 第二种方法
     1. 使用 `ctrl+z` 快捷键
     2. 此方式，程序在后台是 __暂停状态__

3. 实例

   - `command+&`

     1. 命令 `tar -zcf etc.tar.gz etc &` ，压缩 `/etc` 目录（后台执行，不要使用 `-v` 显示压缩文件信息，因为会输出）

        ```shell
        root@localcomputer:/# rm -rf etc.tar.gz 
        root@localcomputer:/# tar -zcf ./etc.tar.gz etc &
        [1] 6400
        root@localcomputer:/# 
        [1]+  已完成               tar -zcf ./etc.tar.gz etc		# 一个任务正确执行完成
        root@localcomputer:/# ls
        bin  boot  cdrom  dev  etc  etc.tar.gz  home  initrd.img  initrd.img.old  lib  lib64  media  mnt  opt  proc  root  run  sbin  snap  srv  sys  tmp  usr  var  vmlinuz  webmin-setup.out
        ```

4. 查看后台的工作

   - 命令格式 

     1. `jobs [选项]`

   - 选项

     1. `-l` ：显示工作的 PID

   - 注意

     1. `+` 表示最近一个放入后台的工作，也是工作恢复时，默认恢复的工作
     2. `-` 表四倒数第二个放入后的工作

   - 实例

     1. 使用快捷方式 `ctrl+z` 暂停工作，`[num]` 表示第几个放入后台的

        ```shell
        root@localcomputer:/# jobs
        [1]-  已停止               top
        [2]+  已停止               tree
        root@localcomputer:/# jobs -l		# 显示 PID
        [1]-  6738 停止 (信号)         top
        [2]+  6746 停止                  tree
        root@localcomputer:/# 
        ```

5. 将后台暂停的工作恢复到前台执行

   - 命令格式
     1. `fg [%工作号]`
   - 参数
     1. `%工作号` ：`%` 号可以省略，注意工作号和 PID 的区别
   
6. 将后台暂停的工作恢复到后台执行

   - 命令格式
     1. `bg [%工作号]`
   - 注意
     1. 后台要恢复执行命令，是不能和前台有交互的，否则不能恢复到后台执行。例如 `top` 命令，就不可以。

### 系统资源定位

__说明：本章介绍系统资源信息。如，内核版本、Linux 系统介绍等……__

##### vmstat 命令监控系统资源

1. `vmstat` 命令监控系统资源

   - 命令格式
     1. `vmstat [刷新延迟] [刷新次数]`

   - 注意
     1. 刷新延迟、刷新次数，例如 `vmstat 1 3` ，每间隔 1 秒，刷新一次，共刷新 3 次
     2. 几乎和 `top` 命令的作用相同，但比 `top` 命令的输出更加简洁

2. 实例

   - 每间隔 1 秒刷新一次，共刷新 3 次

     1. 命令 `vmstat 1 3` ，`free` 参考，主要参考的输出为 __memery 的空闲 和 cpu 的 id（空闲）__

        ```shell
        procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
         r  b 交换 空闲 缓冲 缓存   si   so    bi    bo   in   cs us sy id wa st
         0  0  60920 296180  27596 540936    0    8   160    32   54   99  1  1 98  0  0
         0  0  60920 296180  27596 540936    0    0     0     0  103  198  1  0 99  0  0
         1  0  60920 296180  27596 540936    0    0     0     0   88  147  1  0 99  0  0
        ss@localcomputer:~$ free -h
                      总计         已用        空闲      共享    缓冲/缓存    可用
        内存：        1.9G        1.1G        287M        193M        555M        503M
        交换：        953M         59M        894M
        ```

##### dmesg 开机时内核检测信息

__说明：查看硬件信息__

1. `dmesg` 开机时内核检测信息

   - 命令格式
     1. `dmesg`
   - 注意
     1. `dmseg` 输出的信息量非常大，所以很难从中找出有用信息
     2. 一般都是用 __管道符__ 与其连用

2. 实例

   - 与管道符连用，查询开机时 CPU 信息

     1. `dmesg | grep CPU` ，CPU 大写。输出信息省略部分信息。

        ```shell
        s@localcomputer:~$ dmesg | grep CPU
        [    0.000000] smpboot: Allowing 128 CPUs, 126 hotplug CPUs
        [    0.000000] setup_percpu: NR_CPUS:8192 nr_cpumask_bits:128 nr_cpu_ids:128 nr_node_ids:1
        [    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=128, Nodes=1
        [    0.004000] 	RCU restricting CPUs from NR_CPUS=8192 to nr_cpu_ids=128.
        ```

##### free 命令查看内存使用状态

1. `free` 命令查看内存使用状态

   - 命令格式
     1. `free [-b|-k|-m|-g|-h]`
   - 选项
     1. `-b` ：以字节为单位显示
     2. `-k` ：以 KB 为单位显示，__默认__
     3. `-m` ：以 MB 为单位显示
     4. `-g` ：以 GB 为单位显示
     5. `-h` ：以习惯的方式显示，__一般使用__

2. 缓冲和缓存的区别

   - 简单来说：缓存（cache）是用来加速数据从硬盘中 __读取__ ；缓冲（buffer）是用来加速数据 __写入__ 硬盘的。
   - 以上两者（cache、buffer）都是内存，但实际并没有被进程使用，而是由内核使用的

3. 实例

   - 以 MB 为单位显示

     ```shell
     ss@localcomputer:~$ free -m
                   总计         已用        空闲      共享    缓冲/缓存    可用
     内存：        1970        1123         281         193         564         505
     交换：         953          59         894
     ```

   - 以习惯单位显示

     ```shell
     ss@localcomputer:~$ free -h
                   总计         已用        空闲      共享    缓冲/缓存    可用
     内存：        1.9G        1.1G        281M        193M        564M        505M
     交换：        953M         59M        894M
     
     ```


##### 查看 CPU 信息

1. 查看 CPU 信息是从文件 `/proc/cpuinfo` 读取的

   - `/proc` 是内存挂载点。每次断电，文件信息就会消失；每次重启系统就会自动检测，重新写入信息

2. 实例

   - 查看 CPU 信息，主要还是注意 `mode name`

     1. `cat /proc/cpuinfo`

        ```shell
        model name	: Intel(R) Core(TM) i5-4200H CPU @ 2.80GHz
        stepping	: 3
        microcode	: 0x25
        cpu MHz		: 2793.536
        cache size	: 3072 KB
        ```

##### uptime 命令（系统启动时间、负载）

1. `uptime` 命令介绍

   - 命令格式
     1. `uptime`
   - 显示系统启动时间和平均负载，也就是 `top` 命令的第一行数据，`w` 命令也可以看到这些数据

2. 实例

   - 显示信息

     1. `uptime` 

        ```shell
        root@localcomputer:~# uptime
         14:09:28 up 37 min,  1 user,  load average: 0.01, 0.00, 0.00
        ```

##### 查看系统与内核相关信息

1. 系统内核相关信息

   - 命令格式
     1. `uname [选项]`
   - 选项
     1. `-a` ：查看系统所有相关信息
     2. `-r` ：查看内核版本
     3. `-s` ：查看内核名称

2. 实例

   - 主要注意查看系统内核版本

     1. `uname -r`

        ```shell
        root@localcomputer:~# uname -r
        4.15.0-29-generic				# 内核版本
        root@localcomputer:~# uname -s
        Linux							# 几乎无用
        root@localcomputer:~# uname -a		# 显示本机、内核版本、发布时间、适用的硬件
        Linux localcomputer 4.15.0-29-generic #31-Ubuntu SMP Tue Jul 17 15:39:52 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
        
        ```

##### 判断系统当前位数

1. Linux 没有命令可以直接查看系统当前位数，但是可以使用 `file` 查看 __系统外部命令__ `/bin/ls` ，这样可以顺带显示系统位数

   - 命令格式
     1. `file /bin/ls`
     2. `file` 命令，查看文件类型

2. 实例

   - 注意是系统外部命令

     1. `file /bin/ls` ，显示 64-bit

        ```shell
        root@localcomputer:~# file /bin/ls
        /bin/ls: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=9567f9a28e66f4d7ec4baf31cfbf68d0410f0ae6, stripped
        ```

##### 查看当前 Linux 的发行版本

1. 查看 Linux 的发行版本

   - 命令格式
     1. `lsb_release -a`

2. 实例

   - 主要查看 description

     1. `lsb_release -a`

        ```shell
        root@localcomputer:~# lsb_release -a
        No LSB modules are available.
        Distributor ID:	Ubuntu
        Description:	Ubuntu 18.04.1 LTS		# 长期支持版本（long term support）
        Release:	18.04
        Codename:	bionic		
        ```

##### 列出进程调用的文件

1. 列出进程打开或使用的文件信息
   - 命令格式
     1. `lsof [选项]`
     2. `list open files` 
   - 选项
     1. `-c` ：只列出以字母开头的进程打开的文件
     2. `-u` ：只列出某个用户的进程打开的文件
     3. `-p` ：列出某个 PID 进程打开的文件
2. 实例
   - 查看 `systemd` 进程（Ubuntu 系统中，有的是 `init` 进程），此进程 PID 为 1，为系统其他进程的父进程
     1. `lsof -c systemd` ，使用 `init` 报错
     2. `lsof -p 1` ，推荐使用

### 系统定时任务

##### crond 服务管理与访问控制

__说明：cron 为 计算机计时程序、d 为 daemon 守护进程。Ubuntu 称作 `cron`__

1. `cron` 服务管理与访问控制介绍（centOS 是 `crond`）
   - `cron` 是一个 Linux 定期执行指定命令的守护程序，可以在无需人工干预的情况下运行作业。`cron` 是被默认安装并自启动（随系统系统而启动）
   - 通常，`crontab` 储存的指令被守护进程激活。`cron` 常常在后台运行，每一分钟检查是否有预定的作业需要执行。这类作业一般称为 cron jobs。
   - `cron` 的配置文件称为 crontab，是 cron table 的简写。
2. `crond` 或 `cron` 的作用
   - 保证一些程序可以定时执行，不需要人为手动的执行
   - `cron` 用于设置周期性被执行的指令，它会读取它的所有配置文件（全局性配置文件`/etc/crontab` ，以及每个用户的配置文件 `/var/spool/cron/crontabs` ，以  `/etc/passwd` 中账户名命名的的  crontab  文件），然后 `cron` 会根据命令和执行时间来调度工作任务。 

##### crontab 设置

1. 用户的 `crontab` 设置

   - 命令格式
     1. `crontab [选项]`
   - 选项
     1. `-e` ：编辑 crontab 定时任务
     2. `-l` ：查询 crontab 任务
     3. `-r` ：删除当前用户所有的 crontab 任务

2. 不同用户调用文件不同
   - 普通用户调用 `/var/spool/cron/crontabs/用户名` 文件，为用户配置文件
   - root 用户（超级管理员）调用 `/etc/crontab` 文件，为全局的配置文件

3. 编辑 `crontab` 文件

   - 定时任务设定最小时间单位为分钟

   - __时间 `date +%y%m%d` 需要改写 `date +\%y\%m\%d` ，因为在定时任务中 `%` 是特殊符号__

   - 命令 `crontab -e` 编辑设置文件，按照标准格式写入命令（会用 vim 打开文件）
   
     1. `* * * * * command` ，解释 `*` 号的含义
   
        | 项目       | 含义                 | 范围                       |
        | ---------- | -------------------- | -------------------------- |
        | 第一个 `*` | 一小时当中的第几分钟 | 0-59                       |
        | 第二个 `*` | 一天当中的第几小时   | 0-23                       |
        | 第三个 `*` | 一个月当中的第几天   | 1-31                       |
   | 第四个 `*` | 一年当中的第几个月   | 1-12                       |
        | 第五个 `*` | 一周当中的星期几     | 0-7（0 和 7 都代表星期日） |
   
     2. 举例说明用法（`*` 表示任意时间）
   
        | 时间                 | 含义                                                         |
     | -------------------- | ------------------------------------------------------------ |
        | `45 22 * * * 命令`   | 在每天的 22 点 45 分执行                                     |
   | `0 17 * * 1 命令`    | 周一的 17 点 0 分执行                                        |
        | `0 5 1,15 * * 命令 ` | 每月的 1 号和 15 号凌晨 5 点 0 分执行命令                    |
        | `40 4 * * 1-5 命令`  | 每周一到周五的凌晨 4 点 40 分执行命令                        |
        | `*/10 4 * * * 命令`  | 每天的凌晨 4 点，每隔 10 分钟执行一次命令                    |
        | `0 0 1,15 * 1 命令`  | 每个月的 1 号和 15 号，每周一的 0 点 0 分都会执行命令（1 号、15 号、周一都会执行，3 种情况） <br> __星期几和每个月的几号，最好不要同时出现，因为都是对天的定义。容易造成混乱__ |
        
     3. 特殊符号使用
     
        | 特殊符号 | 含义                                                         |
        | -------- | ------------------------------------------------------------ |
        | `*`      | 代表任何时间。<br> 比如第一个 `*` 就代表一小时中每一分钟都执行一次命令 |
        | `,`      | 代表不连续的时间。<br> 比如 `0 8,12,16 command` 就代表每天的 8 点 0 分、12 点 0 分、16 点 0 分都执行一次命令 |
        | `-`      | 代表连续时间范围。<br> 比如 `0 5 * * 1-6 command`  就代表在周一到周六的凌晨 5 点 0 分执行一次命令 |
        | `*/n`    | 代表每隔多久执行一次命令。 <br> 比如 `*/10 * * * * command` 就代表每隔 10 分钟执行一次命令 |
   
4. 实例

   - 第一次使用 `crontab -e` 会选择对 `crontab` 的编辑器

     1. `crontab -e` ，第一次执行

        ```shell
        ss@localcomputer:~$ crontab -e
        no crontab for ss - using an empty one
        
        Select an editor.  To change later, run 'select-editor'.
          1. /bin/nano        <---- easiest
          2. /usr/bin/vim.basic
          3. /usr/bin/vim.tiny
          4. /bin/ed
        
        Choose 1-4 [1]: 2
        ```

     2. 一般使用 `vim.basic` 作为编辑器

     3. 命令 `select-editor`  (针对crontab的一个命令）， 可以让你重新选一次。

        ```shell
        ss@localcomputer:~$ select-editor 
        
        Select an editor.  To change later, run 'select-editor'.
          1. /bin/nano        <---- easiest
          2. /usr/bin/vim.basic
          3. /usr/bin/vim.tiny
          4. /bin/ed
        
        Choose 1-4 [1]: 2
        ```

   - 每分钟向 `/tmp/test` 文件写入数据，命令 `crontab -e`
     
     1. `* * * * * /bin/date >> /tmp/test`
     
     2. 间隔 2 分钟，查看 `/var/tmp/test` 文件
     
        ```shell
        root@localcomputer:/tmp# cat test 
        2020年 02月 29日 星期六 12:20:01 CST
        2020年 02月 29日 星期六 12:21:01 CST
        ```
     
     3. 查看 crontab 文件 `crontab -l`
     
   - 删除用户 `crontab` 文件
   
     1. `crontab -r` ，删除整个文件
   
   - 定时执行脚本
   
     1. `0 3 * * * /root/test.sh` ，每天凌晨 3 点 0 分执行脚本 `test.sh`

## 日志管理

### 日志管理介绍

#### 日志服务介绍

1. 日志服务

   - 在 CentOS 6.x 中日志服务已经由 `rsyslogd` 取代了原先的 `syslogd` 服务。`rsyslogd` 日志服务更加先进，更能更多。但是无论是该服务的使用，还是日志文件的格式其实都是和 `syslogd` 服务相兼容的，所以学习起来基本和 `syslogd` 服务一致。

2. `rsyslogd` 的新特点

   - 基于 TCP 网络协议传输日志文件
   - 更安全的网络传输协议
   - 有日志消息的及时分析框架
   - 后台数据库
   - 配置文件中可以编写简单的逻辑判断
   - 与 `syslogd` 配置文件相兼容

3. 确定服务是否启动

   - __Linux 中此服务默认开机自启动__ ，一般不用检测

   - `rsyslogd` 进程是否存在

     1. `ps aux | grep "rsyslogd"`，输出结果进程已启动

        ```shell
        ss@localcomputer:~$ ps aux | grep "rsyslogd"
        syslog      814  0.0  0.1 263036  2840 ?        Ssl  13:49   0:00 /usr/sbin/rsyslogd -n
        ss         5616  0.0  0.0  21536  1148 pts/0    R+   16:40   0:00 grep rsyslogd
        ```

   - 查看服务是否为自启动

     1. 查看是否服务自启动（`rsyslog。service` 服务），服务自启动

        ```shell
        ss@localcomputer:~$ systemctl list-unit-files | grep "rsyslog"
        rsyslog.service                                                  enabled                  
        ```

#### 日志服务作用

1. 常见日志的作用

   - 如表

     | 日志文件           | 说明                                                         |
     | ------------------ | ------------------------------------------------------------ |
     | `/var/log/cron`    | 记录了系统定时任务相关的日志                                 |
     | `/var/log/cups`    | 记录打印信息的日志                                           |
     | `/var/log/dmesg`   | 记录了系统在开机时内核自检的信息。也可以使用 `dmesg` 命令直接查看内核自检信息 |
     | `/var/log/btmp`    | 记录错误登录日志。这个是二进制文件，不能使用 vim 查看，而要使用 `lastb` 命令查看。 <br> __此文件不会记录正常开机登录时的错误记录__ |
     | `/var/log/lastlog` | 记录系统中所有用户最后一次等登录时间日志。这个文件也是二进制文件，不能使用 vim ，而要使用 `lastlog` 命令查看。 <br> __此文件会记录远程登录用户、`init 3` 启动级别，但是 Ubuntu 正常开机使用的登录用户不会记录。__ |
     | `/var/log/mailog`  | 记录邮件信息                                                 |
     | `/var/log/message` | 记录系统重要信息日志。这个日志文件中会记录 Linux 系统的绝大多数重要信息，如果系统出现问题时，首先要检测的就是这个日志文件。 |
     | `/var/log/secure`  | 记录验证和授权方面信息，只要涉及账号和密码程序都会记录。比如系统的登录，`ssh` 登录，`su` 切换用户，`sudo` 授权，甚至添加用户和修改用户密码都会记录在这个日志文件中 |
     | `/var/log/wtmp`    | 永久记录所有用户登录、注销信息，同时记录系统的启动、重启、关机事件。同时这个文件也是个二进制文件，不能直接 vim，而需要使用 `last` 命令查看 |
     | `/var/run/utmp`    | 记录当前已经登录的用户信息。这个文件会随着用户的登录和注销而不断变化，只记录当前登录用户的信息，同样这个文件不能直接使用 vim，而要使用 `w` 、`who` 、`user` 等命令查询 |

2. 其他服务的日志文件

   - 处理系统默认的日志之外，采用 `RPM` 方式安装的系统服务也会默认把日志记录在 `/var/log` 目录中（源码包安装的服务日志是在源码包指定目录中）。不过这些日志不是由 `rsyslogd` 服务来记录和管理的，__而是由各个服务使用自己的日志管理文档来记录自身日志__

   - 创建日志文件

     | 日志文件         | 说明                                   |
     | ---------------- | -------------------------------------- |
     | `/var/log/httpd` | `RPM` 包安装的 apache 服务默认日志记录 |
     | `/var/log/mail`  | `RPM` 包安装的邮件服务的额外日志记录   |
     | `/var/log/samba` | `RPM` 包安装的 sanba 服务日志记录      |
     | `/var/log/sssd`  | 守护进程安全服务目录                   |

### rsyslogd 日志服务

__说明：具体 Ubuntu 系统的 `rsyslogd` 的服务说明可以查看 `man 5 rsyslog.conf` 配置文件__

1. 日志文件格式
   - 基本日志格式包含以下四列
     1. 事件产生时间
     2. 发生事件的服务器主机名
     3. 产生事件的服务名或程序名
     4. 事件的具体信息
     
   - 实例
   
     1. `/var/log/auth.log` 日志服务记录
   
        ```shell
        事件产生事件		主机		程序名/服务名		具体信息
        Mar  2 14:51:12 localcomputer su[4185]: Successful su for root by ss
        Mar  2 14:51:12 localcomputer su[4185]: + /dev/pts/0 ss:root
        ```
   
2. rsyslogd 日志服务的配置文件

   - 记录哪些日志文件是通过此文件来设置的

   - `/etc/rsyslog.conf` 配置文件（Ubuntu 的 默认设置在 `/etc/rsyslog.d/50-default.conf` 文件）

     1. 实例

        ```shell
        # First some standard log files.  Log by facility.
        #
        auth,authpriv.*                 /var/log/auth.log
        *.*;auth,authpriv.none          -/var/log/syslog
        #cron.*                         /var/log/cron.log
        #daemon.*                       -/var/log/daemon.log
        kern.*                          -/var/log/kern.log
        #lpr.*                          -/var/log/lpr.log
        mail.*                          -/var/log/mail.log
        #user.*                         -/var/log/user.log
        ```

     2. 解释说明

        - `auth,authpriv.*` ：其中 `auth,authpriv` 为服务名称；`.` 为连接符号；`*` 为日志等级
        - `/var/log/auth.log` ：为日志记录位置
        - 此配置信息说明为：认证相关所有日志等级都记录在 `/var/log/auth.log` 中

   - 日志服务中支持的服务

     1. 如表

        | 服务名称        | 说明                                                         |
        | --------------- | ------------------------------------------------------------ |
        | `auth`          | 安全和认证相关日志（不推荐使用 `authpriv` 替代）             |
        | `authpriv`      | 安全和认证相关日志（私有）                                   |
        | `cron`          | 系统定时任务 `cron` 和 `at` 产生的日志                       |
        | `daemon`        | 和各种守护进程相关日志                                       |
        | `ftp`           | `ftp` 守护进程产生的日志                                     |
        | `kern`          | 内核产生的日志（不是用户进程产生的）                         |
        | `local0-local7` | 为本地使用预留的服务                                         |
        | `lpr`           | 打印产生的日志                                               |
        | `mail`          | 邮寄发送日志                                                 |
        | `newg`          | 与新闻相关的日志                                             |
        | `syslog`        | 由 `syslogd` 服务产生的日志信息（虽然服务名称修改为 `rsyslogd` ，但是很多配置都还是沿用 `syslogd` 的，这里并没有修改服务名称） |
        | `user`          | 用户等级类别日志                                             |
        | `uucp`          | `uucp` 子系统的日志信息，`uucp` 时早期 Linux 系统进程数据传递的协议，后来也常使用在新闻组服务中 |

   - 连接符号

     1. 连接符号可识别为
        - `*` 表示所有日志等级。
          1. 比如 `authpriv.*` 表示 `authpriv` 认证信息服务产生的日志，所有的日志等级都记录
        - `.` 表示只要比后面等级高的（包含该等级）日志都会记录下来
          1. 比如 `cron.info` 表示 `cron` 服务产生的日志，只要等级大于等于 `info` 级别，就纪录
        - `.=` 代表只记录所需等级日志，其他等级的都不记录
          1. 比如 `*.=emerg` 代表人和日志服务产生的日志，只要等级是 `emerg` 等级就纪录。这种用法极少见，了解就好
        - `.!` 表示不等于，也就是除了该等级的日志外，其他等级的日志都记录 
        - __`.none` 表示不记录__
          1. `mail.none` 不记录 mail 任何日志信息

   - 日志等级

     1. 如表

        | 等级名称  | 说明                                                         |
        | --------- | ------------------------------------------------------------ |
        | `debug`   | 一般的调试信息                                               |
        | `info`    | 基本的通知信息                                               |
        | `notice`  | 普通信息，但是有一定重要性                                   |
        | `warning` | 警告信息，但是还不会影响到服务或运行的运行                   |
        | `err`     | 错误信息，一般达到 err 等级的信息以及可以影响到服务或系统运行 |
        | `crit`    | 临界状况信息，比 err 等级还严重                              |
        | `alert`   | 警告状态信息，比 crit 还严重，必须立即采取行动               |
        | `emerg`   | 疼痛等级信息，系统已经无法使用了                             |
        |           |                                                              |

   - 日志记录位置

     1. 日志文件的绝对路径：`/var/log/auth.log`
     2. 系统设备文件：`dev/lp0` （给打印机，直接打印）
     3. 转发给远程主机：`@192.168.28:514`
     4. 用户名：`root` （用户必须在线，否则将丢弃日志）
     5. 忽略或丢弃日志：`~`
     6. `*` 表示发给任何在线用户

### 日志轮转

1. 介绍
   - 日志文件的问题
     
     1. 日志文件不做进一步处理的情况，会随着时间的推移无限的增加体量。这样会导致日志文件在操作系统中（Win10、Linux）中无法打开
     
   - 解决办法
     1. __日志切割__：按照天（24h）为时间单位，将日志存入不同的文件中
     2. __日志删除轮换__：将旧的日志删除，保留一定时间内的日志（保留时间自定义）
     
   - 定时任务分为两中
   
     1. 用户定义
     2. __系统定义：一般在凌晨 4 点 - 6 点之间，日志轮替便是系统定时任务__
   
   - 工具
     
     1. `/etc/logrotate.conf` 是日志轮替配置文件
     
     2. 一般服务程序，支持对日志的切割，但有的不支持旧日志的删除
     
     3. Linux 自带日志轮转工具，就可以满足日志的操作，而不需要在学习特定服务程序的日志工具
     
     4. __使用系统的软件包管理工具安装的软件默认支持日志轮替（使用 `/etc/logrotate.conf` 配置文件）__
   
2. 日志文件的命令规则
   - 如果配置文件中拥有 `dateext` 参数，那么日志文件会用日期来作为日志文件的后缀。例如 `secure-20200303` 。这样日志文件不会重名，所以也就不需要日志文件改名，只需要保存指定的日志个数，删除多余的日志文件即可。
   - 如果配置文件中没有 `dateext` 参数，那么日志文件就需要进行改名了。当第一次日志轮转时，当前的 `secure` 日志会自动改名为 `secure.1` ，然后新建 `secure` 日志，用来保存新的日志。当第二次进行轮替时，`secure.1` 会自动改名为 `secure.2` ，当前 `secure` 日志会自动改名为 `secure.1` ，然后也会新建 `secure` 日志，用来保存新的日志文件，依此类推。
   
3. logrotate 配置文件

   - 如表

     | 参数                      | 参数说明                                                     |
     | ------------------------- | ------------------------------------------------------------ |
     | `daily`                   | 日志的轮替周期是每天                                         |
     | `weekly`                  | 日志的轮替周期是每周                                         |
     | `monthly`                 | 日志的轮替周期是每月                                         |
     | `rotate` 数字             | 保留日志文件的个数，0 指没有备份                             |
     | `compress`                | 日志轮转时，旧的日志进行压缩 <br> Ubuntu 默认是压缩（注释掉也是默认） |
     | `create mode owner group` | 建立新日志，同时指定新日志的权限与所有者和所属组 <br> 如 `create 0600 root utmp` |
     | `mail address`            | 当日志轮替时，输出内容通过邮件发送到指定邮件地址。<br> 如 `mail ShenDeZ@163.com` |
     | `missingok`               | 如果日志不存在，则忽略该日志的警告信息                       |
     | `notifempty`              | 如果日志为空，则不进行日志轮替                               |
     | `minsize` 大小            | 日志轮替的最小值。也就是日志一定要到达最小值才会轮替，否则就算时间达到也不轮替 |
     | `size` 大小               | 日志只有大于指定大小才进行日志轮替，而不是按照时间轮替。<br> 如 `size 100` |
     | `dateext`                 | 使用日期作为日志轮替文件的后缀。<br> 如 `secure-20200303`    |

4. 实例

   - 源码包安装的服务需要手动加入日志轮替中

     1. 如 apache 的日志文件在  `/usr/local/apache/logs/access_log` 文件中，将其写入 `/etc/logrotate.conf` 配置文件

        ```shell
        # logrotate.conf 配置文件
        /usr/local/apach/logs/access_log {
        	daily	# 按天轮转
        	create	# 创建新文件
        	rotate 30	# 保存 30 天
        }
        ```

5. `logrotate` 命令

   - 格式

     1. `logrotate [选项] 配置文件名`

   - 选项

     1. 如果此命令没有选项，则会按照配置文件中的条件进行日志轮替
     2. `-v` ：显示日志轮替过程。使用 `-v` 选项，会显示日志的轮替过程
     3. `-f` ：强制进行日志轮替。不管日志轮替的条件是否已近符合，强制配置文件中所有的日志进行轮替。

   - 演示

     1. __使用 `logrotate -f logrotate.conf` 强制配置文件中的日志轮替，验证服务日志是否加入系统的日志轮替中__

     2. `logrotate -v /etc/logrotate.conf`

        ```shell
        Handling 15 logs		# 一共 15 个日志文件
        
        rotating pattern: /var/log/alternatives.log  monthly (12 rotations)
        empty log files are not rotated, old logs are removed
        switching euid to 0 and egid to 106
        considering log /var/log/alternatives.log
          Now: 2020-03-03 22:11
          Last rotated at 2020-03-01 19:30
          log does not need rotating (log has been rotated at 2020-3-1 19:30, that is not month ago yet)	# 不需要轮替
        switching euid to 0 and egid to 0
        ```

     3. 可以随便写入一个文件进行测试，使用 `logrotate -v /etc/logrotate.conf` 检测此日志是否轮替，再使用 `logrotate -f /etc/logrotate.conf` 强制日志轮替，最后检查日志是否轮替

## 启动管理

### 启动管理（CentOS 6.3）

#### 系统运行级别

1. 运行级别

   - 如表

     | 运行级别 | 含义                                                         |
     | -------- | ------------------------------------------------------------ |
     | 0        | 关机                                                         |
     | 1        | 单用户模式，类似于 Windows 的安全模式，主要用于系统修复 <br> 只启动最基本的服务 |
     | 2        | 不完全的命令行模式，不含 NFS 服务                            |
     | 3        | 完全的命令行模式，__就是标准的字符界面模式__                 |
     | 4        | 系统保留                                                     |
     | 5        | 图像模式                                                     |
     | 6        | 重启                                                         |

2. 运行级别命令

   - 查看运行级别命令

     1. `runlevel`

   - 改变运行级别命令

     1. `init num` num 为运行级别

        ```shell
        ss@localcomputer:~$ runlevel
        N 5					# N 为 None 缩写，表示从哪个级别进入 5 级别的，None 为开机进入图像界面
        ```

3. 系统默认运行级别

   - 修改配置文件 `/etc/inittab` __Linux 各个发行版本不一样，系统升级版本也不一样，自己看吧__
     1. `id:3:initdefault`

#### 系统启动过程

1. 启动流程图

   - 如图 __（启动过程非常复杂，Linux 的发行版本启动过程各个不同，发行版本的版本启动过程也不尽相同）__

     ![Linux启动流程图](git_picture/Linux启动流程图.jpg)

2. 解释部分启动过程

   - BIOS（Basic Iuput Output System，基本输入输出系统）
     1. 不属于任何操作系统
     2. BIOS 是固化到主板上的程序，计算机通电后第一个进入的系统，用于电脑刚接通电源时对硬件部分的检测。在完成自检后，BIOS 将按照设置中的启动顺序搜寻软硬盘驱动器及 CDROM、网络服务器等有效的启动驱动器 ，读入操作系统引导记录，然后将系统控制权交给引导记录，由引导记录完成系统的启动。
     3. 引导程序功能是引导 DOS 或其他操作系统。BIOS 先从软盘或硬盘的开始扇区读取引导记录，如果没有找到，则会在显示器上显示没有引导设备，如果找到引导记录会把电脑的控制权转给引导记录，由引导记录把操作系统装入电脑，在电脑启动成功后，BIOS 的这部分任务就完成了。
     
   - MBR（Master Boot Record，主引导记录）
     1. 不属于任何操作系统
     2. 启动 PC 机时，系统首先对硬件设备进行测试，测试成功后读系统磁盘 0 柱面、0 磁头、1 扇区的主引导记录（MBR）内容到内存指定单元，并执行 MBR 程序段。
     3. 组成
        - 引导程序（引导代码）
        - 硬盘分区表
     
   - MBR 中的引导程序
     
     1. 主引导记录最开头是第一阶段引导代码。其中的硬盘引导程序的主要作用是检查分区表是否正确并且在系统硬件完成自检以后将控制权交给硬盘上的引导程序（如GNU GRUB）。 它不依赖任何操作系统，而且启动代码也是可以改变的，从而能够实现多系统引导。
     2. 如果是多系统，MBR 中的引导程序会给出选项提示，以供选择（但系统默认直接加载内核）
     3. 作用
        - 加载内核
        - 加载 `/boot` 目录，系目录是启动分区
     
   - 内核解压缩并自检
   
     1. 内核一般都是压缩文件，减少占用大的启动空间
     2. 内核也会再进行一次自检（BIOS 做一次自检）。一般 Linux 会信任内核自检，把自建信息放入 `dmesg` 文件中
     3. Linux 的内核与 Windows 不同，一般使用的驱动程序 Linux 和嵌入内核中（Windows 需要下载），安装完 Linux 之后可以直接使用硬件系统，而 Windows 需要给硬件安装驱动程序才可以使用。但是要将所有驱动都放入 Linux 内核中，内核将变得比较笨重，所以解决办法为将不常见的硬件驱动变为函数模块放入硬盘中（一般放入 `/lib/` 目录中）
     4. 现在为止，一般的硬盘接口都是 `sata` ，而 Linux 认为 `sata` 硬盘驱动是不常用驱动，所以放在函数模块中。__现在 Linux 内核没有 `sata` 硬盘驱动（其驱动在硬盘 `/lib/` 目录中），所以就无法从硬盘中读取数据，但是要想正常启动系统，就必须从硬盘中读取数据，这时产生一个悖论。__
     5. 解决上面产生的悖论：在 `/boot/` 启动分区中有一个 initramfs 文件系统（是个压缩文件 cpio 格式），此文件系统是同 Linux 内核同时被加载内存（Ubuntu 好像使用的是 initrd 文件系统），而 `initramfs` 中就存储着包含 `sata` 接口的硬盘的驱动程序，这样就可以解决内核因无硬盘而无法加载硬盘的情况
   
   - `initramfs` 内存文件系统
   
     1. 这个玩应挺复杂（Ubuntu 使用 `initrd`），但是此文件一般是压缩文件，和内核一起会被引导程序加载到内存中，内核可以从此文件加载一些驱动，从而可以使用硬盘。
     2. 在 `/boot/` 目录下保存此文件，`/boot` 启动分区，就是和内核一起加载到内存中的（RAM） 
     3. CentOS 6.3 中使用 `initramfs` 内存文件系统取代了 CentOS 5.x 中的 `initrd RAM Disk`。它们的作用类似，可以通过启动引导程序加载到内存中，然后加载启动过程中所需要的内存模块，比如 `USB/STAT/SCSI` 硬盘的驱动和 `LVM/RAID` 文件系统的驱动。
   
   - 挂载真正的系统根目录
   
     1. 因为 `initramfs` 文件的存在，使得 Linux 的内核可以对硬盘进行数据读取
   
   - 调用 `/sbin/init`
   
     1. `init` 和 `systemd` 命令的区别，需要进一步了解
     2. `init` 是系统的第一个进程，是系统其他进程的父进程，但总感觉 `systemd` 有点问题。
     3. 因版本的升高，系统对 `init` 的作用进行了调整（作用减少）
   
   - 调用 `/etc/init/rcS.conf` 配置文件
   
     1. 主要功能有两个
   
        - 先调用 `/etc/rc.d/rc.sysinit` 然后由 `/etc/rc.d/rc.sysinit` 配置文件进行 Linux 系统初始化
   
          1. 初始化如下
   
             | 排序 | 作用                                       | 排序 | 作用                            |
             | ---- | ------------------------------------------ | ---- | ------------------------------- |
             | 1    | 获得网络环境                               | 13   | 初始化 LVM 的文件系统功能       |
             | 2    | 挂在设备                                   | 14   | 检验磁盘文件系统（fsck）        |
             | 3    | 开机启动画面 Plymouth（取代了过去的 RHGB） | 15   | 设置磁盘配额（quota）           |
             | 4    | 判断是否启动 SELinux                       | 16   | 重新以可读写模式挂载系统磁盘    |
             | 5    | 显示于开机过程的欢迎画面                   | 17   | 更新 quota（非必要）            |
             | 6    | 初始化硬件                                 | 18   | 启动系统虚拟随机数生成          |
             | 7    | 用户自定义模块加载                         | 19   | 配置机器（非必要）              |
             | 8    | 配置内核的参数                             | 20   | 清除开机过程的临时文件          |
     | 9    | 设置主机名                                 | 21   | 创建 ICE 目录                   |
             | 10   | 同步存储器                                 | 22   | 启动交换分区（swap）            |
        | 11   | 设备映射器及相关的初始化                   | 23   | 将开机信息写入 `/var/log/dmesg` |
             | 12   | 初始化软件磁盘阵列（RAID）                 | 24   |                                 |
             
        
        - 然后再调用 `/etc/inittab`，然后由 `/etc/inittab` 配置文件确定系统的默认运行级别
     
   - 调用 `/etc/inittab`
   
     1. 此文件为设置系统启动级别
     2. 调用 `rc[0-6].d`文件
     3. Ubuntu 没有此文件
   
   - 调用 `rc[0-6].d` 目录
   
     1. 此目录为系统相应的系统运行级别启动的文件
     2. 此目录中的以 `S` 开头为 `Start` 缩写，以顺序启动
     3. 以 `K` 开头为 `Kill` 缩写，以顺序关闭
   
   - `/etc/rc.d/rc.local` 
   
     1. 此文件为开机前，执行的文件，可在启动加入脚本
   
   - 如果启动的是图像界面则接着会启动图形服务
   
   - 输入用户名密码

### 启动引导程序 grup

#### Grup 配置文件

1. `grub` 中分区表示

   - 在 grub 中不管硬盘是什么接口（`hd` 表示 IDE 接口，`sd` 表示 SCSI 和 SATA 接口）统统表示为 `hd`

   - 如表

     | 硬盘             | 分区         | Linux 中设备文件名 | Grub 中设备文件名 |
     | ---------------- | ------------ | ------------------ | ----------------- |
     | 第一块 SCSI 硬盘 | 第一主分区   | `/dev/sda1`        | `hd(0,0)`         |
     |                  | 第二主分区   | `/dev/sda2`        | `hd(0,1)`         |
     |                  | 扩展分区     | `/dev/sda3`        | `hd(0,2)`         |
     |                  | 第一逻辑分区 | `/dev/sda5`        | `hd(0,4)`         |
     | 第二块 SCSI 硬盘 | 第一主分区   | `/dev/sdb1`        | `hd(1,0)`         |
     |                  | 第二主分区   | `/dev/sdb2`        | `hd(1,1)`         |
     |                  | 扩展分区     | `dev/sdb3`         | `hd(1,2)`         |
     |                  | 第一逻辑分区 | `dev/sdb5`         | `hd(1,4)`         |

2. `grub` 配置文件

   - 此处说明一下：`grub` 有两种开发分支，其中一种已经停止开发了，Ubuntu 应该使用的 `grub 2`（持续支持的） 。但是这里讲解的是，已经停止开发的版本
   - `default=0` ：默认启动第一个系统
   - `timeout-5` ：等待时间。默认是 5 秒（有 5 秒中的时间选择启动第几个系统）
   - `splashimage=(hd0,0)` ：这里是指定 `grub` 启动时的背景图像文件的保存位置的
   - `hiddenmenu` ：隐藏菜单（隐藏的就是对系统选择的界面）
   - 以下内容：加载内容的位置；加载内核定义；加载 `initramfs` 文件系统

#### Grup 加密与字符界面分辨率调整

### 系统修复模式

## 备份与恢复

### 备份概念

1. 备份数据
   - Linux 系统需要备份的数据
     1. `/root/` 目录
     2. `/home/` 目录
     3. `/var/spool/mail/` 目录
     4. `/etc/` 目录
     5. 其他目录
   - 安装服务的数据
     1. apache 需要备份的数据
        - 配置文件
        - 网页主目录
        - 日志文件
     2. mysql 需要备份的数据（就是数据库的数据）
        - 源码包安装的 mysql：`/usr/local/mysql/date/`
        - RPM 包安装的 mysql：`/var/lib/mysql/`
2. 备份策略
   - 完全备份：完全备份就是指把所有需要备份的数据全部备份，当然完全备份可以备份整个硬盘，整个分区或某个具体的目录（备份时消耗资源，但恢复简单）。
   - 增量备份：每次进行备份时，指备份与上一次备份增加的数据即可，而不是全部备份（因为只备份增量数据，所以备份时简单，但恢复时麻烦，需要一次一次的恢复备份数据）。
   - 差异备份：时完全备份与增量备份的折中办法

### 常见备份与恢复命令

#### 命令 dump

__说明：默认没有安装，需要手动安装__

1. `dump` 命令

   - 命令格式
     1. `dump [选项] 备份之后的文件名 源文件或目录`
   - 选项
     1. `-level` ：就是 0-9 十个备份级别（0 表示完全备份，1 表示第一次增量备份……）
     2. `-f 文件名` ：指定备份之后的文件名
     3. `-u` ：备份成功之后，把备份时间记录在 `/etc/dumpdates`  或者 `/var/lib/dumpdates`文件（常用）
     4. `-v` ：显示备份过程中更多的信息输出
     5. `-j` ：调用 `bzlib` 库压缩备份文件，其实就是把备份文件压缩为 `.bz2` 格式（常用）
     6. `-W` ：显示允许被 `dump` 的分区的备份等级及备份时间

2. 备份分区

   - 备份 `/boot` 分区

     1. 执行一次完全备份，并压缩和更新备份时间 `dump -0uj -f /root/boot.bak.bz2 /boot/`

     2. 查看 `/var/lib/dumpdates` 文件

        ```shell
        root@localcomputer:~# cat /var/lib/dumpdates 
        /dev/sda1 0 Sat Mar  7 13:09:26 2020 +0800
        ```

     3. 执行一次增量备份 `dump -1uj -f /root/boot.bak1.bz2 /boot/` 。该级别只会备份增量的数据

     4. 查看 `/var/lib/dumpdates` 文件

        ```shell
        root@localcomputer:/boot# cat /var/lib/dumpdates 
        /dev/sda1 0 Sat Mar  7 13:15:04 2020 +0800
        /dev/sda1 1 Sat Mar  7 13:15:32 2020 +0800
        ```

     5. 查询分区的备份时间及备份级别 `dump -W`

        ```shell
        root@localcomputer:~# dump -W
        Last dump(s) done (Dump '>' file systems):
          /dev/sda5	(     /) Last dump: never
          /dev/sda3	( /home) Last dump: never
          /dev/sda1	( /boot) Last dump: Level 1, Date Sat Mar  7 13:15:32 2020
        ```

   - 备份文件或目录

     1. 完全备份 `/etc/` 目录，只能使用 0 级别进行完全备份，__而不再支持增量备份__
        - `dump -0j -f /root/etc.dump.bz2 /etc/`

#### 命令 restore

1. `restore` 命令

   - 命令格式 `restore [模式选项] [选项]`
   - 模式选项：`restore` 命令常用的模式有以下四种，这四个模式不能混用。
     1. `-C` ：比较备份数据和实际数据的变化
     2. `-i` ：进入交互模式，手工选项需要恢复的文件
     3. `-t` ：查看模式，用于查看备份文件中拥有哪些数据
     4. `-r` ：还原模式，用于数据还原

   - 选项
     1. `-f` ：指定备份文件的文件名

2. 比较备份数据和实际数据的变化

   - 对比 `/boot` 分区备份数据和实际数据（对新增加的数据无法对比，只能比较出已有数据的变化）

     1. `restore -C -f /root/boot.bak.bz2`

        ```shell
        root@localcomputer:/boot# restore -C -f /root/boot.bak.bz2 
        Dump tape is compressed.
        Dump   date: Sat Mar  7 13:34:39 2020
        Dumped from: the epoch
        Level 0 dump of /boot on localcomputer:/dev/sda1
        Label: none
        filesys = /boot
        restore: unable to stat ./abi-4.15.0-29-generic: No such file or directory
        Some files were modified!  1 compare errors
        ```

3. 查看模式

   - 查看 `/boot` 分区
     1. `restore -t -f /root/boot.bak.bz2`

4. 还原模式

   - 还原 `/boot` 分区（最好新建目录，在此目录中恢复数据）

     1. 先还原完全备份的数据 `restore -r -f /root/boot.bak.bz2` （解压缩）
     2. 恢复增量备份数据 `restore -r -f /root/boot.bak1.bz2`

   - 还原 `/etc/` 目录的备份数据

     1. `restore -r -f etc.dump.bz2`

     



















