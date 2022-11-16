# linux学习--乌班图（ubuntu）

## 一、基础篇

### 1.linux入门

#### 修改终端

```bash
#修改这段为：
if [ "$color_prompt" = yes ]; then
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\W$(git_branch)\[\033[00m\]\$ '
#PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
PS1='${debian_chroot:+($debian_chroot)}\u@\h:\W$(git_branch)\$ '
#PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi



#在最后加上：
git_branch()
{

branch=`git rev-parse --abbrev-ref HEAD 2>/dev/null`
if [ "${branch}" != "" ]
then
if [ "${branch}" = "(no branch)" ]
then
branch="(`git rev-parse --short HEAD`...)"
fi
echo -e " \033[1;43;37m[$branch]\033[0m " #颜色设置
fi
}
```

### 2.网络连接的三种模式

- 桥接模式

   虚拟系统可以直接和外部系统通讯，但是容易造成IP冲突，因为直接分配一个当前网段的ip，可能会发生重复

-  NAT模式

  网络地址转换，会在当前主机生成一个虚拟，内部随便生成一个ip地址，在和外部进行通讯的时候，会转换成当前主机的IP地址，进行通讯

- 主机模式

  独立的系统.提供的是主机和虚拟机之间的访问。

**==桥接 通过使用物理机网卡，具有单独ip。在bridged模式下，VMWare虚拟出来的操作系统就像是局域网中的一台独立的主机，它可以访问网内任何一台机器。主机网卡和虚拟网卡的IP地址处于同一个网段，子网掩码、网关、DNS等参数都相同。==**

**==NAT 把物理机当作路由器进行上网==**

![image-20221103213128576](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221103213128576.png)

![image-20221103213754736](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221103213754736.png)



### 3.虚拟机快照

在使用虚拟机系统的时候，想要回到原先的一个状态，即担心某些操作会造成系统异常，需要回到原先某个正常运行的状态，vmware提供了快照管理的功能

### 4.vmtools

这个可以在主机和虚拟机之间建立一个共享文件夹，两者都可以使用。读取修改

### 5.linux目录结构

linux的目录结构采用的是层级的树状目录结构，在此结构中的最上层是根目录“/”，然后在此目录下创建其他的目录

==在linux中，一切皆文件==

具体的目录结构

- bin （/usr/bin,  /usr/local/bin）

  这个目录存放着经常使用的命令

- /sbin (/usr/sbin,  /usr/local/sbin)

  super user 存放的是系统管理员使用的系统管理程序

- /home

  存放的是普通用户的主目录，在linux中每个用户都有一个自己的目录，一般该目录是以用户名来进行命名的

- /root

  该目录是系统管理员，也称为超级权限者的用户主目录，它运行的程序在sbin里面

- /lib

  系统开机所需要的最基本的动态链接共享库，其作用类似于windows中的dll 文件，几乎所有的应用程序都需要用到这些共享库

- /lost + found 

  这个目录一般情况是空的， 当系统非法关机后，这里就存放了一些文件

- /etc

  所有的系统管理所需要的配置文件和子目录

- /usr

  ==这个目录非常重要==，用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录

- /boot 

  存放的是启动linux时使用的一些核心文件，包括一些链接文件和镜像文件

- /proc

  这个目录是一个虚拟的目录，他是系统内存的映射，访问这个目录来获取系统的信息

- /src

  service的缩写，存放的是一些服务启动之后所需要提取的数据，==不要动==

- /sys

  系统目录，不要动

- /tmp

  这个目录是存放一些临时文件的

- /dev 

  类似于windows的设备管理器，把所有的硬件用文件的形式存储

- /media

  linux系统会自动识别一些设备，例如U盘，光驱等，当识别后，linux会把识别的设备挂载到这个目录下

- /mnt

  系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将外部的存储挂载在/mnt/上，然后进入该目录就可以查看里面的内容了

- /opt

  这是给主机额外==安装软件==所摆放的目录，例如安装ORACLE数据库就可以放在该目录下，默认为空

- /usr/local

  这是另一个为主机额外安装 软件所安装的目录，一般是通过编译源码方式安装的程序

- /var

  这个目录存放着不断扩充着的东西，习惯将经常修改的目录放在这个目录下，包括各种日志文件

- /selinux

  SELinux是一种安全子系统，他能控制程序只能访问特定的文件，有三种工作模式，可以自行设定

> |── bin -> usr/bin # 用于存放二进制命令 
>
> ├── boot # 内核及引导系统程序所在的目录 
>
> ├── dev # 所有设备文件的目录（如磁盘、光驱等）
>
> ├── etc # 配置文件默认路径、服务启动命令存放目录\
>
> ├── home # 用户家目录，root用户为/root 
>
> ├── lib -> usr/lib # 32位库文件存放目录 
>
> ├── lib64 -> usr/lib64 # 64位库文件存放目录 
>
> ├── media # 媒体文件存放目录 
>
> ├── mnt # 临时挂载设备目录 
>
> ├── opt # 自定义软件安装存放目录 
>
> ├── proc # 进程及内核信息存放目录 
>
> ├── root # Root用户家目录 
>
> ├── run # 系统运行时产生临时文件，存放目录 
>
> ├── sbin -> usr/sbin # 系统管理命令存放目录 
>
> ├── srv # 服务启动之后需要访问的数据目录 
>
> ├── sys # 系统使用目录 
>
> ├── tmp # 临时文件目录 
>
> ├── usr # 系统命令和帮助文件目录 
>
> └── var # 存放内容易变的文件的目录



### 常用

- 配置清华源

  > ```
  > sudo sed -i "s@http://.*archive.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
  > sudo sed -i "s@http://.*security.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
  > 直接运行上面的即可
  > 
  > ```

- git分支显示

  ```bash
  #修改这段为：
  if [ "$color_prompt" = yes ]; then
  PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\W$(git_branch)\[\033[00m\]\$ '
  #PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
  else
  PS1='${debian_chroot:+($debian_chroot)}\u@\h:\W$(git_branch)\$ '
  #PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
  fi
  
  
  
  #在最后加上：
  git_branch()
  {
  
  branch=`git rev-parse --abbrev-ref HEAD 2>/dev/null`
  if [ "${branch}" != "" ]
  then
  if [ "${branch}" = "(no branch)" ]
  then
  branch="(`git rev-parse --short HEAD`...)"
  fi
  echo -e " \033[1;43;37m[$branch]\033[0m " #颜色设置
  fi
  }
  ```

  

- 

## 二、实际操作篇

### 1.远程登陆到linux服务器

使用XShell 和 Xftp 登录到远程服务器

### 2.关机& 重启命令

- 基本介绍

  |        命令        |                  作用                  |
  | :----------------: | :------------------------------------: |
  | shut down -h - now |          立刻进行关机 h:halt           |
  |   shutdown -h 1    | 一分钟后关机，直接shutdown也是这个效果 |
  |  shutdown -r now   |                现在重启                |
  |        halt        |                关机now                 |
  |       reboot       |                立刻重启                |
  |        sync        |         将内存的数据同步到磁盘         |

  

- 注意细节

  在进行关机和重启的时候，最好运行sync进行同步

### 3.用户登录和注销

1. 基本介绍
   - 登陆时尽量少用root账号登陆，可以利用普通用户登录，登陆后在使用==su - 用户名== 切换到root用户
   - 注销用户使用logout即可注销

### 4.用户管理

linux系统是要给多用户多任务操作系统，任何一个要使用系统资源的用户，都必须要首先向系统申请一个账号看，然后以这个账号的身份进入系统添加用户与删除用户

在linux中只能有一个root账号

![image-20221025091519763](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221025091519763.png)

==sudo全程super user do 干超级用户才能干的事情==

> sudo adduser+ 用户名 （然后在设置密码和其他的相关信息即可）
>
> ==最好使用adduser 可以直接创建用户并且生成home主文件，使用useradd并不会添加你家home，使用adduser创建的账号是系统账号，可以用来登录ubuntu系统==
>
> 添加一个用户，默认该用户的家目录在/home/用户名
>
> passwd + 用户名  
>
> 修改该用户的密码

- 删除用户

  > （sudo） userdel + 用户名  这种方式只会删除用户，对应的文件夹不会删除
  >
  > （sudo） userdel  -r  用户名  这种方式可以删除用户有，并且对应的文件夹都会删除

#### 4.1查询用户信息

- 基本语法

  id  用户名

- 当用户不存在时，返回无此用户

#### 4.2查询当前登录用户是谁

> w who  who am i

#### 4.3 修改用户名

![image-20221029152637332](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221029152637332.png)

#### 4.4用户组

类似于角色，系统可以对有共性/权限 的多个用户进行统一的管理

- 新增组

  groupadd 组名

- 删除组

  groupdel 组名

- 增加用户时直接加上组

  ![image-20221025125305807](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221025125305807.png)

  > 首先创建一个用户组
  >
  > 然后将该用户添加到改组

- 修改用户的组

  usermod -g 用户组 用户名

  ![image-20221025130257133](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221025130257133.png)

​		如果单纯将一个用户添加到一个组，那么这个用户就同时拥有两个组的权限

​		使用usermod可以进行修改

- 用户组的相关文件

  1. /etc/passwd 文件

     用户的配置文件，记录用户的各种信息

     ![image-20221025131512946](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221025131512946.png)

     用户名：口令：用户标识号：组标识号：注释性描述：主目录 ：登录shell

  2. /etc/shadow 文件   只有在登录到root账号才可以看见其他的信息

     口令的配置文件

     ![image-20221025131614368](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221025131614368.png)

     登录名：加密口令（把密码进行加密操作）：最后一次修改时间：最小时间间隔：最大时间间隔：警告时间：不活动时间：失效时间：标志

  3. /etc/group 文件

     组的配置文件，记录linux包含的组的信息

     ![image-20221025131737576](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221025131737576.png)

     组名：口令：组标识号：组内成员列表（隐藏了看不见）

### 5.实用指令

#### 5.1运行级别

| 运行级别 |                 说明                 |
| :------: | :----------------------------------: |
|    0     |                 关机                 |
|    1     |        单用户（找回丢失密码）        |
|    2     | 多用户状态没有网络服务（不怎么使用） |
|    3     | 多用户状态有网络服务mult-user target |
|    4     |     系统未使用保留给用户（忽略）     |
|    5     |         图形界面graph target         |
|    6     |               系统重启               |

常用的运行级别时3，5 也可以指定默认的运行级别

systemctl get-default

systemctl ste-default  mult....

#### 5.2找回root密码

#### 5.3帮助指令

- man

  > 语法：man【命令或者配置文件】  获得帮助信息

- help

  > 语法：help 命令   获得shell内置命令的帮助信息

#### 5.4文件目录类

- pwd (print working directory)

  显示当前工作目录的绝对路径

- ls

  ls  【选项】  【目录或者是文件】

  常用选项：

  - -a  显示当前目录的所有文件和目录，包括隐藏的
  - -l   以列表的方式显示所有信息

- cd 

  切换到指定的目录

  cd ~ 或者 cd 空格       回到自己的家目录

  cd .. 回到当前目录的上级目录

- mkdir

  创建文件目录

  > sudo mkdir  【选项】 要创建的目录
  >
  > 常用选项： -p  创建多级目录

- rmdir （remove directory）

  删除空目录

  > sudo rmdir  【选项】 要删除的目录（这个不能有多级目录）
  >
  > ==如果要删除多级目录，那么就要使用rm -rf 然后加上要删除的目录==

- touch 

  创建 空文件

  > touch  文件名称   会创建一个空的这个文件

- cp

  cp指令拷贝文件（目录也是一种特殊的文件）到指定的目录

  > cp 【选项】 source dest
  >
  > 选项：- r  递归复制整个文件夹

- rm

  移除文件或者目录

  > rm  【选项】  要删除的文件或者目录
  >
  > 选项： -f  强制删除，不显示删除提示
  >
  > ​			-r  递归删除整个文件夹

- mv

  移动文件与目录或者重命名

  > 基本语法：
  >
  > mv    oldNameFile   newNameFile (重命名---在同一目录下 )
  >
  > mv    文件或者目录    目录   移动文件或者目录到另一个目录下，有同名就是移动，没有同名的就是重命名

- cat

  查看文件内容

  > cat 【选项】 要查看的文件
  >
  > 选项：- n    显示行号

  ==cat只能浏览文件，而不能修改文件。比较安全。为了查看方便，一般结合管道命令使用   cat  -n  要查看的文件名   |    more==

- more

  more指令是要给基于vi编辑器的文本过滤器，他以全屏幕的方式按页显示文本文件的内容

  > more  要查看的文件

|   操作   |             功能说明             |
| :------: | :------------------------------: |
|  space   |             向下翻页             |
|  enter   |            向下翻一行            |
|    q     | 立刻离开more，不再显示该文件内容 |
| Ctrl + F |           向下滚动一屏           |
| Ctrl + B |            返回上一屏            |
|    =     |         输出当前行的行号         |
|    :f    |     输出文件名和当前行的行号     |

- less

  less指令是用来分屏查看文件内容的，他的功能与more相似，但是比more指令更加强大，支持各种显示终端。less指令在显示文件内容时，并不是一次将整个文件加载之后才显示，二十根据显示需要加载内容，对于显示大型文件具有较高的效率

  > 语法：
  >
  > less   要查看的文件

  |   操作   |               功能                |
  | :------: | :-------------------------------: |
  |  空白键  |           向下翻动一页            |
  | pagedown |           向下翻动一页            |
  |  pageup  |           向上翻动一页            |
  |  /字串   | ==向下==搜索 n 向下查找 N向上查找 |
  |  ?字串   |             向上搜索              |
  |    q     |               离开                |

- echo

  输出内容到控制台

  > 语法：
  >
  > echo 【选项】  【输出内容】

- head

  用于显示文件的开头部分内容，默认显示文件的前10行内容

  > 语法：
  >
  > head  文件
  >
  > head -n 5  文件   （自定义指定查看行数）

- tail

  输出文件尾部的内容。默认显示的是最后的10行内容

  > 语法：
  >
  > tail    文件     
  >
  > tail   - n   5    文件    （查看最后5行的内容）
  >
  > tail   - f     文件   （实时追踪该文档的所有更新）

- 输出重定向 >   和   >> 追加指令

  例如  echo  ”print(“hello world”)  hello.py 就会把这个代码写入到该文件中。==注意这个写入不是追加的形式，会直接进行覆盖。使用  >> 这个就可以把内容追加到里面去了==

  > 输出重定向就是把本应该输出到控制台的东西，重新向到另一个地方，如果是个文件，就会把内容写入
  >
  > ls  - l  > 文件   （把列表的内容写入到文件中）
  >
  > ls  - al >> 文件   （把列表的内容追加到文件中）
  >
  > cat 文件1   >  文件2    (把文件1的内容覆盖到文件2中去)
  >
  > echo “内容”  >> 文件   (把内容追加到文件中)

- ln

  软链接也成为符号链接，类似于windows下的快捷方式，存放了其他文件的路径

  > ln   - s     【原文件或目录】  【软链接名】  （给源文件创建一个软连接）

- history

  查看执行过的历史命令

#### 5.5时间日期类

- date

  显示当前日期

  > date			显示当前日期
  >
  > date    + %y		显示当前年份
  >
  > date	+%m		显示当前月份
  >
  > date	+%d		显示当前是那一天
  >
  > date	”+%y-%m-%d %H:%M:%S”		具体到秒数

- cal

  查看日历指令

  > cal     选项    （不加选项，显示本月日历）
  >
  > cal   2020  可以显示该年的所有日历

#### 5.6搜索查找类

- find

  find 指令将从指定目录向下递归的遍历各个子目录，将满足条件的文件或者目录显示在终端

  > 语法：
  >
  > find	【搜索范围】  【选项】
  >
  > |             选项             |              功能              |
  > | :--------------------------: | :----------------------------: |
  > | -name <查询方式>可以使用正则 | 按照指定文件名查找模式查找文件 |
  > |        -user<用户名>         |   查找属于指定用户的所有文件   |
  > |       -size<文件大小>        |      按照文件大小进行查找      |

- locate

  可以快速定位文件路径。locate指令利用时间建立的系统中所有文件名称以及路径的locate数据库实现快速定位给定的文件。Locate指令无需遍历整个文件系统，查询速度较快。为了保证查询结果的准确度，管理员必须更新locate

  特别说明： 由于locate指令基于数据库进行查询，所以第一次运行前，必须使用updatedb创建locate数据库

- grep 和 管道符号 |

  grep过滤查找，管道符 | 表示将前一个命令的处理结果输出传递给后面的命令处理

  > 语法：
  >
  > grep	【选项】 查找内容	源文件
  >
  > 选项：  
  >
  > -n	显示匹配行即行号
  >
  > -i		忽略字母大小写

  cat a.txt | grep “hello”   在给定文件中查找hello关键字

  grep -n  “yes”   /home/hello.py

  ![image-20221025170638005](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221025170638005.png)

  ![image-20221025171030543](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221025171030543.png)

#### 5.7压缩和解压类

- gzip / gunzip

  gzip用于压缩文件，gunzip 用于解压文件

  压缩之后原文件不见了

  > 语法：
  >
  > gzip  文件   （压缩文件，只能将文件压缩为   .gz   文件）
  >
  > gunzip  文件.gz    （解压文件）

- zip 和 unzip 

  zip用来压缩文件，unzip用来解压的，

  > 语法：
  >
  > zip    【选项】   xxx.zip      将要压缩的内容 
  >
  > unzip  【选项】  xxx.zip     解压文件
  >
  > 【选项】：  -r   递归压缩，即压缩目录
  >
  > ​                      -d <目录>  指定解压后文件的存放目录

  zip   -r  myhome.zip   / home/

  unzip -d  /opt/    myhome.zip

- tar 

  tar是打包指令，最后打包的文件时==.tra.gz==文件

  > 语法：
  >
  > tar  【选项】    xxx.tar.gz    打包的内容         （打包目录，压缩后的文件格式为.tar.gz）
  >
  > 将某个文件解压到具体的目录
  >
  > -ZCVF     -ZXVF
  >
  > ==tar  -zxvf   要解压的文件      -C  指定解压的目录==
  >
  > | 选项 |        功能        |
  > | :--: | :----------------: |
  > |  -c  |    打包tar文件     |
  > |  -v  |    显示详细信息    |
  > |  -f  | 指定压缩后的文件名 |
  > |  -z  |    打包同时压缩    |
  > |  -x  |    解包.tar文件    |
  >
  > ![image-20221026135504743](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221026135504743.png)

### 6.组管理和权限管理

在linux中，每个用户必须属于一个 组，不能独立于组外。在linux中每个文件存在   

- 所有者  创建文件的用户
- 所在组  和该创建用户在一个组的用户
- 其他组  和该创建文件的用户不在一个组的用户

三个概念

#### 6.1 文件/目录所有者

文件的创建者，但是可以将所有权转给其他人

- 查看文件的所有者

  > ls   - al  / -ahl    
  >
  > ![image-20221026143007719](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221026143007719.png)



- 修改文件(目录)的所有者

  > 语法：
  >
  > chown   用户名    文件名       （change owner  改变所有者）
  >
  > chown    fengtengfei    hello.py      （将后面这个文件的所有者变为前面的用户）
  >
  > <img src="C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221026143355288.png" alt="image-20221026143355288" style="zoom:100%;" >

#### 6.2组的创建与修改

- groupadd   组名

  groupadd monster

- 创建用户并且将用户指定到组中

  useradd 用户

  usermod -g 组名 用户名

- 修改文件所在的组

  > chgrp   （-R递归目录）    新组名    文件     （change group）
  >
  >   ![image-20221026183202593](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221026183202593.png)

### 7.权限

#### 7.1认识权限

![image-20221026183928126](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221026183928126.png)

前面总共10位

rwx权限详解

创建文件默认权限

![image-20221026192905533](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221026192905533.png)

- rwx作用到文件

  r   read   可以读取查看文件

  w write   可以修改，但是不代表可以删除，删除一个文件的前提是对该文件所在的目录有写权限，才能删除该文件

  x  execute  这个文件可以被执行

- rwx作用到目录

  r  可以读取， ls查看目录内容

  w  可以修改，对目录内创建、删除、重命名目录

  x  可以进入到该目录

| 第多少位 | 作用                                                         |
| :------: | :----------------------------------------------------------- |
|    0     | 确定文件类型<br />l  （link）是链接，相当于快捷方式<br />d  dircetory  目录，相当于是文件夹   一般是蓝色<br />c   char   是字符设备文件、鼠标、键盘<br />b   block 是块设备 比如硬盘<br />-    代表普通文件 |
|  1---3   | 确定所有者的权限   --user     ==权限   -     表示没有权限==  |
|  4---6   | 确定所属组的权限   --group                                   |
|  7---9   | 确定其他用户的权限                                           |
|          |                                                              |

#### 7.2修改权限

- chmod

  通过chmod指令，可以修改文件或者目录的权限

  > 第一种方式： +  、-  、= 变更权限
  >
  > u：所有者	g：所有组	o：其他人	 a:所有人（u，g，o的总和）
  >
  > eg:	chmod   u =rwx  ， g = rx ， o = x  加文件名
  >
  > 第二种方式：使用数字来变更权限
  >
  > **linux中r=4，w=2，x=1**
  >
  > chmod  751  文件/目录   7 = 4 2 1 rwx，  5 = 41 rx，  1 x

- chown  

  修改文件所有者

  > chown 新的所有者     文件/目录
  >
  > chown  新的文件所有者：新的组    文件/目录   改变所有者和所在组
  >
  > 如果是目录，- R  递归修改所有者和所在组
  >
  > eg:    chown tom /home/abc.txt
  >           chown -R tom /home/kkk



总结

usermod -g 组名 用户名  修改用户所在组

chown   修改文件目录所有者

chgrp   修改文件或目录的组

chmod   修改文件或目录的用户权限

### 8.crond任务调度

![image-20221028205901232](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221028205901232.png)

任务调度：是指系统在某个时间执行特定的命令或者程序

- 任务调度分类

  - 系统工作 

    有些重要的工作必须周而复始的执行，如病毒扫描等

  - 个别用户工作

    个别用户可能希望执行某些程序在特定的时间

- 语法

  ```
  crontab [选项]        crond定时任务 
  -e  编辑crontab定时任务
  -l   查询crontab定时任务
  -r 删除当前用户的所有crontab任务
  
  
  
  */1****  ls -l /ect/ > /tmp/to.txt
  
  ```

  ![image-20221028210324836](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221028210324836.png)

  ![image-20221028211440281](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221028211440281.png)

- crond相关指令

  > crontab -r ：终止所有的任务调度
  > crontab -l  列出所有的任务调度
  >
  > **service crond restart     重启任务调度**

#### at定时任务

at定时任务是一次性的，要保证atd运行

![image-20221028213655531](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221028213655531.png)

- 命令格式

  > at  【选项】  【时间】
  >
  > Ctrl + D 结束at命令的输入
  >
  >  at命令选项![image-20221028213808121](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221028213808121.png)
  >
  > ![image-20221028214003287](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221028214003287.png)
  >
  > **可以使用atq查看当前的at定时任务**
  >
  > 

- atrm 编号  

  删除指定的定时任务

## 三、磁盘分区 挂载

Linux来说无论有几个分区，分给哪一个目录使用，它归根结底只有一个根目录，一个独立且唯一的文件结构。Linux中每个分区都是用来组成整个文件系统的一部分。

linux采用了一种叫”载入“的处理方式，他的整个文件系统包含了一整套的文件和目录，且将一个分区和一个目录联系起来，这是要载入的一个分区加你个是他的存储空间在一个目录下获得

![image-20221028215425762](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221028215425762.png)

![image-20221028215537493](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221028215537493.png)

- 查看所有设备挂载情况

  > lsblk   lsblk -f

#### 磁盘情况查询

> df -h    （disk free）
>
> ![image-20221028221410831](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221028221410831.png)



- 查询指定目录的情况

  > du -ha   目录

#### 磁盘实用指令

树状显示目录结构

## 四、网络配置



## 五、进程管理

#### 进程管理

在linux中，每一个执行的程序都称为一个进程，每一个进程都分配一个id号（PID 进程号）

每个进程都可能以两种方式存在，前台和后台，一般系统服务进程以后台进程的方式存在

- 查看系统运行的进程

  > ps  这种方式显示的很少
  >
  > ps - a  显示当前终端所有的进程信息
  >
  > ps -u   以用户的格式显示进程信息
  >
  > ps -x 显示后台进程运行的参数
  >
  > 一般组合使用 ps -axu
  >
  > ![image-20221029100710288](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221029100710288.png)
  >
  > ![image-20221029100846832](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221029100846832.png)

**可以通过grep过滤，仅查看自己关心的进程**

- 以全格式显示当前的所有进程，查看进程的父进程

  > ps -ef  查看所有的
  >
  > ps -ef | grep 查看我们指定的
  >
  > ![image-20221029101246956](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221029101246956.png)

- 终止进程

  某个进程执行了一半需要停止时，或是已经消耗了很大的系统资源时，可以考虑停止该进程，使用kill指令来完成

  > 语法：
  >
  > kill   【选项】 进程号     （通过进程号杀死进程）
  >
  > killall   进程名称      通过进程名称杀死进程，支持通配符
  >
  > 选项：
  >
  > -9   表示强迫进程立即停止

- 查看进程树

  pstree  【选项】     （可以更加直观的查看进程信息）

  选项：

  -p      显示进程的pid

  -u      显示进程的所属用户

  ![image-20221029103834120](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221029103834120.png)

#### 服务管理

服务，service 本质就是进程，但是是运行在后台的，通常都会 监听某个端口，等他 其他的程序的请求。比如mysql，sshd等，因此我们又称为守护进程，是linux中非常重要知识

- service 管理智力高

  > service    服务名   【start  |  stop | restart | reload | status】

- 服务的运行级别 run -level

  常用的运行级别是 3 和 5

- systemctl 服务管理指令

  > systemctl  【start | stop |  restart |  status 】 服务名
  >
  > 查看防火墙的服务
  >
  > 在ubuntu系统中，默认安装了ufw防火墙，使用命令
  >
  > sudo ufw status 查看防火墙的状态
  >
  > sudo ufw enable   开启防火墙
  >
  > sudo ufw disable   关闭防火墙
  >
  > sudo ufw  reload   重启防火墙

#### 动态监控进程

- top 命令

  top与ps命令很相似，都是用来显示正在执行的进程，top与ps最大的不同之处在于top执行一段时间可以更新正在运行的进程

  > top   【选项】
  >
  > -d  秒数    （指定top命令每隔几秒更新、默认是三秒）
  >
  > -i          （使top不显示任何闲置或者僵死进程）
  >
  > -p         （通过指定监控进程id来仅仅监控某个进程的状态）
  >
  > ![image-20221029111720632](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221029111720632.png)

## 六、ubuntu APT软件管理

apt是advanced packaging tool的简称，是一款安装包管理工具。在ubuntu，我们可以使用apt进行软件的安装删除，清理等。类似一windows下的软件管理工具

![image-20221029114235614](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221029114235614.png)

![image-20221029114520788](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221029114520788.png)

![image-20221029114530644](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221029114530644.png)

在 /etc/apt/source.list 是apt管理的软件的安装网址。可以先备份一个，然后在修改成清华的

![image-20221029115028110](C:\Users\AryaFeng\AppData\Roaming\Typora\typora-user-images\image-20221029115028110.png)













