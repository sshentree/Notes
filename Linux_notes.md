# Linux 基础知识

__说明：以下提及到的文件包含目录及文本（mkdir \ touch），没有特殊标记及说明的情况下默认__

## Linux 文件扩展名

### Linux 不靠扩展名区分文件类型

__说明：以下是扩展名类型只是使用习惯，方便使用者识别__

1. 表现

| 压缩包      | 二进制软件包 | 网页文件 | 脚本文件 | 配置文件 |
| ----------- | ------------ | -------- | -------- | -------- |
| `*.gz`      | `.rmp`       | `.html`  | `.sh`    | `*.conf` |
| `*.bz2`     |              | `.php`   |          |          |
| `*.tar.bz2` |              |          |          |          |

## Linux 个目录的作用

### 以表格的形式说明主要目录作用

1. 主要介绍以下目录

   - `/bin` `/sbin` `/usr/bin/` `/boot` `/dev/` `/etc/` `/home/` `/lib/` `lost + found` `/media/` `/mnt/` `/misc` `/opt/` `/proc/` `/sys/` `/root/` `/srv/` `/temp/`   `/usr/` `/var/` 

   - 表格

     | 目录           | 目录作用                                                     |
     | -------------- | ------------------------------------------------------------ |
     | /bin/          | 存放系统命令的目录，普通用户和超级用户都可以操作。不过放在 /bin 下的命令在在单用户模式下也可以执行 |
     | /sbin/         | 保存和系统环境设置相关的命令，只有超级用户可以使用这些命令进行系统环境设置，但有些命令可以允许普通用户查看 |
     | /usr/bin/      | 存放系统命令的目录，普通用户和超级用户都可以操作。这些命令和系统启动无关，在单用户模式下不能执行 |
     | /usr/sbin/     | 存放根文件系统不必要的系统管理命令，例如多少服务程序。只用超级用户可以使用。<br> 其实可以注意到 Linux 的系统，所有在 sbin 目录中保存大命令只有超级用户可以使用，bin 目录保存的命令所有用户都可以使用 |
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

   - 例

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
     | 1        | 表示文件类型(`-`表示文件、`d` 表示目录、`l` 表示软链接) | \    | \                  |
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

   - 硬链接类似于 `cp -p` 复制文件或目录，且保留源文件属性（即文件类型、权限、最后修改时间），__但是有所不同硬链接是和源文件同步更新的__
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

     1. u 所有者；g 所属组；o 其他人；a 表示所有人，使用 `+-=` 表示对文件的权限修改

     2. 演示

        ```shell
        ss@localcomputer:~/桌面/tmp$ ls -l
        总用量 8
        -rw-r--r-- 2 ss ss  146 12月  9 22:31 a.txt
        drwxr-xr-x 2 ss ss 4096 12月  9 22:01 test
        ss@localcomputer:~/桌面/tmp$ chmod o=rw a.txt # 修改权限
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

   - 使用管理员创建一个用户 `useradd 用户名`  设置密码（也可以不设置）  `passwd 用户名`

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

   - 演示；使用 root 修改文件所有者，将其修改为 s

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

2. 命令 `chgrp [用户组] [文件或目录]`

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
     0:特殊权限
     
     777: rwx rwx rwx
     022: --- -w- -w-  (异或)
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

   - 文件创建得权限默认没有 `x` ，由也会在创建时，抹掉；目录 `x` 权限是进入目录操作

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

     | 选项         | 作用              | 注意                                                         | 演示                                             |
     | ------------ | ----------------- | ------------------------------------------------------------ | ------------------------------------------------ |
     | -name        | 以文件名搜索      | `*` 表示任意多字符，`?` 表示一个位置字符                     |                                                  |
     | -iname       | 以文件名搜索      | 不区分大小写                                                 |                                                  |
     | -size        | 以文件大小搜索    | Linux 最小存储单元为数据块（512字节=0.5k），搜索大小需用数据块表示，+ 表示大于 - 表示小于 =表示正好等于 | 搜索根目录小大于 1MB 得文件 `find / -size +2048` |
     | -user/-group | 以所有者查找文件  | 不是以文件名，而是以所有者为参数，查找所有者得文件           | 查找根目录下所有者为 ss 的文件 `find / -user ss` |
     | -amin        | 访问时间          | 在 5 分钟之内访问的文件及目录 `-5` ，a 为 access 缩写        |                                                  |
     | -cmin        | 文件属性          | 在一定数间内修改文件属性的，c 为 change 缩写                 |                                                  |
     | -mmin        | 文件内容          | 在一定时间内修改文件内容，m 为 modify 缩写                   |                                                  |
     | -inum        | 以 inode 节点查找 | `ls -i` 查找 i 节点，以文件 inode 节点删除文件，__还可以查找硬链接__ |                                                  |

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

   - `locate 文件名称`
   - __包含文件名的都会被查找出来，但是查找区分大小写（添加选项 `-i`）__

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
   - `grep -iv [指定字符串][文件]`
   - `-i` 不区分大小写
   - `-v` 排除指定字符串

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
   - `/字符串` 搜索字符串，使用 `n` 查看下一个

2. 注意

   - 查看配置文件信息，不要使用绝对路径（/etc/services）
   - 查看配置文件信息，直接使用配置文件名（services）就可以
   - 配置文件
     1. 查看配置文件的介绍
     2. 配置文件的格式
     3. `man services` 网络服务配置信息
     4. `man 5 passwd`  查看 __密码文件__

3. 使用 `man` 查看命令、配置帮助信息

   - 显示内容全面

   - 但过于全面

   - `whatis 命令`

     ```shell
     ss@localcomputer:~$ whatis ls
     ls (1)               - 列目录内容
     ```

   - `apropos 配置文件名`

     ```shell
     ss@localcomputer:~$ apropos inittab
     inittab (5)          - 与 sysv 兼容的 init 进程使用的初始化文件...
     ```

   - 简单，很明了

### info 用法

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
       ```

     - `w` 演示

       ```shell
       root@localcomputer:/home/ss# w
        21:55:55 up  1:54,  1 user,  load average: 0.01, 0.00, 0.00
       USER     TTY      来自           LOGIN@   IDLE   JCPU   PCPU WHAT
       ss       :0       :0               20:01   ?xdm?  41.98s  0.02s /usr/lib/gdm3/gdm
       ```

## Linux 压缩解压命令

__说明：压缩便于备份、传输和并入一般很难感染压缩文件，.zip 在 Linux 和 Windows 都可以直接使用__

### 压缩格式  .gz

__说明：在 LInux 中非常常见的一种压缩格式__

#### gzip \ gunzip(gzip -d) 使用

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
     1. 先将目录打包（打包和目录不同），目录打包后的文件格式`.tar`
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

     1. 压缩，分成两部完成

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

     2. 压缩，一步完成

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

     1. `-v` ：解包
     2. `-v` ：显示详细信息
     3. `-f` ：指定解压文件
     4. `-z` ：压缩文件

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

#### bzip \ bunzip使用

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

   - 压缩目录 `tar -jcvf [压缩后文件名] [目录]` ，解压缩目录 `tar -jcvf 压缩文件.bz2`

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

     2. Linux 本机登录的用户，不接受 `write` 发送的消息（禁用 mesg），但是可以发送消息

        ```shell
        $ write ss
        write: ss has messages disabled
        ```

   - 命令 `write user`

     - `Enter` 发送    `ctrl + D`  结束

2. 演示

   - Linux 本机用户向远程登录用户发送消息

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
     1. 同样 Linux 本机登录的收不到消息，但是可以广播
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

   - 统一用户，发送并接受

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

   - 查看和设置网卡信息，愿意 *interface configure*
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

     











 







