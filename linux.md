

## [System V](https://en.wikipedia.org/wiki/UNIX_System_V)    
UNIX System V is one of the first commercial versions of the Unix operating system. It was originally developed by AT&T and first released in 1983. Four major versions of System V were released, numbered 1, 2, 3, and 4. System V Release 4, or SVR4, was commercially the most successful version, being the result of an effort, marketed as "Unix System Unification", which solicited the collaboration of the major Unix vendors. It was the source of several common commercial Unix features. System V is sometimes abbreviated to SysV.

system V 主要用 chkconfig和sevice(Redhat系列)、 update-rc.d(debian 系列)命令管理服务。

## [Systemd](https://fedoraproject.org/wiki/Systemd/zh-cn#System_V_init_.E4.B8.8E_systemd_.E7.9A.84.E5.AF.B9.E6.8E.A5)
systemd 是 Linux 下一个与 SysV 和 LSB 初始化脚本兼容的系统和服务管理器。systemd 使用 socket 和 D-Bus 来开启服务，提供基于守护进程的按需启动策略，保留了 Linux cgroups 的进程追踪功能，支持快照和系统状态恢复，维护挂载和自挂载点，实现了各服务间基于从属关系的一个更为精细的逻辑控制，拥有前卫的并行性能。systemd 无需经过任何修改便可以替代 sysvinit 。

[systemctl](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units) 是 systemd 最主要的工具。它融合 service 和 chkconfig 的功能于一体。你可以使用它永久性或只在当前会话中启用/禁用服务。

Systemctl 主要负责控制systemd系统和服务管理器。Systemd是一个系统管理守护进程、工具和库的集合，用于取代System V初始进程。   

systemd uses 'targets' instead of runlevels. By default, there are two main targets:b
* multi-user.target: analogous to runlevel 3
* graphical.target: analogous to runlevel 5

To view current default target, run:

    systemctl get-default

To set a default target, run:

    systemctl set-default TARGET.target


* 延伸阅读    
[Centos7下的systemctl命令与service和chkconfig](https://blog.csdn.net/cds86333774/article/details/51165361)     
[systemd，upstart， systemV服务启动编写](https://www.jianshu.com/p/d856428bc43f)     
[浅析 Linux 初始化 init 系统，第 1 部分](https://www.ibm.com/developerworks/cn/linux/1407_liuming_init1/index.html?ca=drs-)   
[浅析 Linux 初始化 init 系统，第 2 部分](https://www.ibm.com/developerworks/cn/linux/1407_liuming_init2/index.html)   
[浅析 Linux 初始化 init 系统，第 3 部分](https://www.ibm.com/developerworks/cn/linux/1407_liuming_init3/index.html?ca=drs-)   


chkconfig, service 与 systemctl 命令对照

| 任务	           |              旧指令	          |         新指令            |
|-------            | ----------                      | --------------            |
|使某服务自启	        |  chkconfig --level 3 httpd on	  |   systemctl enable httpd.service |
|使某服务不自动启动	  |  chkconfig --level 3 httpd off	|   systemctl disable httpd.service |
|检查服务状态	        |  service httpd status	          |   systemctl status httpd.service 或者 systemctl is-active httpd.service |
|显示所有已启动服务	  |  chkconfig --list	            |   systemctl list-units --type=service |
|启动某服务	         |  service httpd start	           |   systemctl start httpd.service |
|停止某服务	         |  service httpd stop	           |   systemctl stop httpd.service |
|重启某服务	         |  service httpd restart	       |   systemctl restart httpd.service |


> SysV init守护进程（sysv init软件包）是一个基于运行级别的系统，它使用运行级别（单用户、多用户以及其他更多级别）和链接（位于/etc/rc?.d目录中，分别链接到/etc/init.d中的init脚本）来启动和关闭系统服务。Upstart init守护进程（upstart软件包）则是基于事件的系统，它使用事件来启动和关闭系统服务。

## linux 基础知识

#### linux 启动过程
1. 读取 MBR 的信息,启动 Boot Manager。Windows 使用 NTLDR 作为 Boot Manager,如果您的系统中安装多个版本的 Windows,您就需要在 NTLDR 中选择您要进入的系统。Linux 通常使用功能强大,配置灵活的 GRUB 作为 Boot Manager。
2. 加载系统内核,启动 init 进程。init 进程是 Linux 的根进程,所有的系统进程都是它的子进程。
3. init 进程读取 /etc/inittab 文件中的信息,并进入预设的运行级别,按顺序运行该运行级别对应文件夹下的脚本。脚本通常以 start 参数启动,并指向一个系统中的程序。通常情况下, /etc/rcS.d/ 目录下的启动脚本首先被执行,然后是/etc/rcN.d/ 目录。例如您设定的运行级别为 3,那么它对应的启动目录为 /etc/rc3.d/ 。
4. 根据 /etc/rcS.d/ 文件夹中对应的脚本启动 Xwindow 服务器 xorg。Xwindow 为 Linux 下的图形用户界面系统。
5. 启动登录管理器,等待用户登录。Ubuntu 系统默认使用 GDM 作为登录管理器,您在登录管理器界面中输入用户名和密码后,便可以登录系统。(您可以在 /etc/rc3.d/ 文件夹中找到一个名为 S13gdm 的链接)

#### Login shells 与 Nonlogin shells 执行过程

Login shells

    /etc/profile
        /etc/profile.d
    ~/.bash_profile
        ~/.bashrc
            /etc/bashrc

Non-login shells

        ~/.bashrc
            /etc/bashrc
                /etc/profile.d

.bash_logout 在每次登陆shell退出时被读取并执行。

#### linux 中变量的含义

* $# 是传给脚本的参数个数
* $0 是脚本本身的名字
* $1 是传递给该shell脚本的第一个参数
* $2 是传递给该shell脚本的第二个参数
* $@ 是传给脚本的所有参数的列表
* $* 是以一个单字符串显示所有向脚本传递的参数，与位置变量不同，参数可超过9个
* $$ 是脚本运行的当前进程ID号
* $? 是显示最后命令的退出状态，0表示没有错误，其他表示有错误

#### linux 目录

* /usr：系统级的目录，可以理解为C:/Windows/，/usr/lib理解为C:/Windows/System32。
* /usr/local：用户级的程序目录，可以理解为C:/Progrem Files/。用户自己编译的软件默认会安装到这个目录下。
* /opt：用户级的程序目录，可以理解为D:/Software，opt有可选的意思，这里可以用于放置第三方大型软件（或游戏），当你不需要时，直接rm -rf掉即可。在硬盘容量不够时，也可将/opt单独挂载到其他磁盘上使用。
*源码目录*  
* /usr/src：系统级的源码目录。  
* /usr/local/src：用户级的源码目录。  

**/usr文件系统** 
* /usr 文件系统经常很大，因为所有程序安装在这里. /usr 里的所有文件一般来自Linux distribution；本地安装的程序和其他东西在/usr/local 下.这样可能在升级新版系统或新distribution时无须重新安装全部程序. 

* /usr/X11R6    
X Window系统的所有文件.为简化X的开发和安装，X的文件没有集成到系统中. X自己在/usr/X11R6 下类似/usr .  

* /usr/X386    
类似/usr/X11R6 ，但是给X11 Release 5的.  

* /usr/bin  
几乎所有用户命令.有些命令在/bin 或/usr/local/bin 中. 

* /usr/sbin   
根文件系统不必要的系统管理命令，例如多数服务程序.  

* /usr/man , /usr/info , /usr/doc   
手册页、GNU信息文档和各种其他文档文件.  

* /usr/include   
C编程语言的头文件.为了一致性这实际上应该在/usr/lib 下，但传统上支持这个名字. 

* /usr/lib   
程序或子系统的不变的数据文件，包括一些site-wide配置文件.名字lib来源于库(library); 编程的原始库存在/usr/lib 里.  

* /usr/local   
本地安装的软件和其他文件放在这里.  用户自己编译的软件默认会安装到这个目录下。这里主要存放那些手动安装的软件，即不是通过“新立得”或apt-get安装的软件。它和/usr目录具有相类似的目录结构。让软件包管理器来管理/usr目录，而把自定义的脚本(scripts)放到/usr/local目录下面

**/var文件系统** 

* /var 包括系统一般运行时要改变的数据.每个系统是特定的，即不通过网络与其他计算机共享.  

* /var/catman   
当要求格式化时的man页的cache.man页的源文件一般存在/usr/man/man* 中；有些man页可能有预格式化的版本，存在/usr/man/cat* 中.而其他的man页在第一次看时需要格式化，格式化完的版本存在/var/man 中，这样其他人再看相同的页时就无须等待格式化了. (/var/catman 经常被清除，就象清除临时目录一样.)  

* /var/lib   
系统正常运行时要改变的文件.  

* /var/local   
/usr/local 中安装的程序的可变数据(即系统管理员安装的程序).注意，如果必要，即使本地安装的程序也会使用其他/var 目录，例如/var/lock .  

* /var/lock   
锁定文件.许多程序遵循在/var/lock 中产生一个锁定文件的约定，以支持他们正在使用某个特定的设备或文件.其他程序注意到这个锁定文件，将不试图使用这个设备或文件.  

* /var/log   
各种程序的Log文件，特别是login  (/var/log/wtmp log所有到系统的登录和注销) 和syslog (/var/log/messages 里存储所有核心和系统程序信息. /var/log 里的文件经常不确定地增长，应该定期清除.  

* /var/run   
保存到下次引导前有效的关于系统的信息文件.例如， /var/run/utmp 包含当前登录的用户的信息. 

* /var/spool   
mail, news, 打印队列和其他队列工作的目录.每个不同的spool在/var/spool 下有自己的子目录，例如，用户的邮箱在/var/spool/mail 中.  

* /var/tmp   
比/tmp 允许的大或需要存在较长时间的临时文件. (虽然系统管理员可能不允许/var/tmp 有很旧的文件.) 

/opt：用户级的程序目录
这里主要存放那些可选的程序。你想尝试最新的firefox测试版吗?那就装到/opt目录下吧，这样，当你尝试完，想删掉firefox的时候，你就可以直接删除它，而不影响系统其他任何设置。安装到/opt目录下的程序，它所有的数据、库文件等等都是放在同个目录下面。
举个例子：刚才装的测试版firefox，就可以装到/opt/firefox_beta目录下，/opt/firefox_beta目录下面就包含了运 行firefox所需要的所有文件、库、数据等等。要删除firefox的时候，你只需删除/opt/firefox_beta目录即可，非常简单。
在硬盘容量不够时，也可将/opt单独挂载到其他磁盘上使用。

***


## linux 命令

#### [nohup](https://www.cnblogs.com/baby123/p/6477429.html)    
　　用途：不挂断地运行命令。

　　语法：nohup Command [ Arg ... ] [　& ]

　　描述：nohup 命令运行由 Command 参数和任何相关的 Arg 参数指定的命令，忽略所有挂断（SIGHUP）信号。在注销后使用 nohup 命令运行后台中的程序。要运行后台中的 nohup 命令，添加 & （ 表示"and"的符号）到命令的尾部。
例如后台执行Spring boot:

    nohup java -jar crm-1.0.jar --server.port=8080 --spring.profiles.active=pro >/home/nginx/crm/logs/crm.log &

#### ldconfig
ldconfig 命令的用途,主要是在默认搜寻目录(/lib和/usr/lib)以及动态库配置文件/etc/ld.so.conf内所列的目录下,搜索出可共享的动态链接库(格式如前介绍,lib*.so*)，进而创建出动态装入程序(ld.so)所需的连接和缓存文件，缓存文件默认为 /etc/ld.so.cache，此文件保存已排好序的动态链接库名字列表。

#### 向文件添加和追加内容

    ivan@jiefang:~/test$ echo Hello > a.txt
    ivan@jiefang:~/test$ echo World >> a.txt
其中，> 是覆盖，>> 是追加。

#### find
To find all socket files on your system run

    sudo find / -type s

#### ps

ps -l

    F S UID PID PPID C PRI NI ADDR SZ WCHAN TTY TIME CMD
    0 S 0 5881 5654 0 76 0 - 1303 wait pts/0 00:00:00 su
    4 S 0 5882 5881 0 75 0 - 1349 wait pts/0 00:00:00 bash
    4 R 0 6037 5882 0 76 0 - 1111 - pts/0 00:00:00 ps
 

上面这个信息其实很多喔！各相关信息的意义为：
* F 代表这个程序的旗标 (flag)， 4 代表使用者为 super user；
* S 代表这个程序的状态 (STAT)，关于各 STAT 的意义将在内文介绍；
* PID 没问题吧！？就是这个程序的 ID 啊！底下的 PPID 则上父程序的 ID；
* C CPU 使用的资源百分比
* PRI 这个是 Priority (优先执行序) 的缩写，详细后面介绍；
* NI 这个是 Nice 值，在下一小节我们会持续介绍。
* ADDR 这个是 kernel function，指出该程序在内存的那个部分。如果是个 running 的程序，一般就是『 - 』的啦！
* SZ 使用掉的内存大小；
* WCHAN 目前这个程序是否正在运作当中，若为 - 表示正在运作；
* TTY 登入者的终端机位置啰；
* TIME 使用掉的 CPU 时间。
* CMD 所下达的指令为何！？


检测后台进程是否存在

    ps -ef | grep redis
    ps aux | grep redis


#### netstat
检测6379端口是否在监听

    netstat -lntp | grep 6379

#### top 
top 的全屏对话模式可分为3部分：系统信息栏、命令输入栏、进程列表栏。
第一部分 — 最上部的 系统信息栏 ：
第一行（top）：
“14:55:59”为系统当前时刻；
“4 days,  5:52”为系统启动后到现在的运作时间；
“1 user”为当前登录到系统的用户，更确切的说是登录到用户的终端数 — 同一个用户同一时间对系统多个终端的连接将被视为多个用户连接到系统，这里的用户数也将表现为终端的数目；
“load average”为当前系统负载的平均值，后面的三个值分别为1分钟前、5分钟前、15分钟前进程的平均数，一般的可以认为这个数值超过 CPU 数目时，CPU 将比较吃力的负载当前系统所包含的进程；
第二行（Tasks）：
“12 total”为当前系统进程总数；
“1 running”为当前运行中的进程数；
“11 sleeping”为当前处于等待状态中的进程数；
“0 stoped”为被停止的系统进程数；
“0 zombie”为被复原的进程数；
第三行（Cpus）：
分别表示了 CPU 当前的使用率；
第四行（Mem）：
分别表示了内存总量、当前使用量、空闲内存量、以及缓冲使用中的内存量；
第五行（Swap）：
表示类别同第四行（Mem），但此处反映着交换分区（Swap）的使用情况。通常，交换分区（Swap）被频繁使用的情况，将被视作物理内存不足而造成的。
第二部分 — 中间部分的内部命令提示栏：
top 运行中可以通过 top 的内部命令对进程的显示方式进行控制。内部命令如下表：
s – 改变画面更新频率
l – 关闭或开启第一部分第一行 top 信息的表示
t – 关闭或开启第一部分第二行 Tasks 和第三行 Cpus 信息的表示
m – 关闭或开启第一部分第四行 Mem 和 第五行 Swap 信息的表示
N – 以 PID 的大小的顺序排列表示进程列表
P – 以 CPU 占用率大小的顺序排列进程列表
M – 以内存占用率大小的顺序排列进程列表
h – 显示帮助
n – 设置在进程列表所显示进程的数量
q – 退出 top
s – 改变画面更新周期
第三部分 — 最下部分的进程列表栏：
以 PID 区分的进程列表将根据所设定的画面更新时间定期的更新。通过 top 内部命令可以控制此处的显示方式。
一般的，我们通过远程监控的方式对服务器进行维护，让服务器本地的终端实时的运行 top ，是在服务器本地监视服务器状态的快捷便利之一。

按 f 键，再按某一列的代表字母，即可选中或取消显示


#### time

#### tee

#### tree
以树型结构显示当前文件夹下的文件及其子文件夹下的内容。    

忽略某些文件下命令：

    tree -I '*svn|*node_module*'

#### nmap

#### [getconf](https://www.cnblogs.com/MYSQLZOUQI/p/5552238.html)
将系统配置变量值写入标准输出。

查看 Linux 系统的位数

    getconf LONG_BIT
    


#### 参考  
[鸟哥的 Linux 私房菜](http://cn.linux.vbird.org/linux_server/)    
[debian下服务控制命令](http://www.cnblogs.com/Joans/articles/4939002.html)

#### linux 突然断网
linux 网络断了，重启后宝如下错误：

    Failed to start LSB: Bring up/down networking.
解决方法：

    systemctl restart NetworkManager
    sudo service network restart


#### linux 解压缩
* tar 文件解压缩

        tar -cvf jpg.tar *.jpg
        tar -xvf file.tar
* tar.gz 文件解压缩

        tar -zcvf jpg.tar.gz *.jpg
        tar -zxvf file.tar.gz
* tar.bz2 文件解压缩

        tar -jcvf jpg.tar.bz2 *.jpg
        tar -jxvf file.tar.bz2
* tar.Z 文件解压缩

        tar -Zcvf jpg.tar.Z *.jpg
        tar -Zxvf file.tar.Z
* rar 文件解压缩

        rar a jpg.rar *.jpg
        unrar e file.rar
* zip 文件解压缩

        zip jpg.zip *.jpg
        unzip file.zip 
* xz 文件解压缩

        xz -z 要压缩的文件
        xz -d 要解压的文件
        使用 -k 参数来保留被解压缩的文件
其他

        *.gz 用 gzip -d或者gunzip 解压
        *.bz2 用 bzip2 -d或者用bunzip2 解压
        *.Z 用 uncompress 解压


##3## linux 历史记录
查询历史命令记录并执行某条命令

    history
    !100

> ~/.bash_histroy里面是记录的上次注销前的历史记录（最大保存1000条，且是上次注销前最近的1000条记录）  
  HISTSIZE 变量记录了保存的数量

## linux 文件

#### /etc/sudoers
sudo的配置文件。该文件允许特定用户像root用户一样使用各种各样的命令，而不需要root用户的密码。


***
## Linux 源码安装
这些都是典型的使用 GNU 的 AUTOCONF 和 AUTOMAKE 产生的程序的安装步骤。

Makefile介绍 

Makefile是用于自动编译和链接的，一个工程有很多文件组成，每一个文件的改变都会导致工程的重新链接，但是不是所有的文件都需要重新编译，Makefile中纪录有文件的信息，在make时会决定在链接的时候需要重新编译哪些文件。 

Makefile的宗旨就是：让编译器知道要编译一个文件需要依赖其他的哪些文件。当那些依赖文件有了改变，编译器会自动的发现最终的生成文件已经过时，而重新编译相应的模块。 

Makefile的基本结构不是很复杂，但当一个程序开发人员开始写Makefile时，经常会怀疑自己写的是否符合惯例，而且自己写的 Makefile 经常和自己的开发环境相关联，当系统环境变量或路径发生了变化后，Makefile可能还要跟着修改。这样就造成了手工书写Makefile的诸多问题，automake恰好能很好地帮助我们解决这些问题。 

使用automake，程序开发人员只需要写一些简单的含有预定义宏的文件，由autoconf根据一个宏文件生成configure，由 automake根据另一个宏文件生成Makefile.in，再使用configure依据Makefile.in来生成一个符合惯例的 Makefile。

#### ./configure, make, make install 的作用 
configure，这一步一般用来生成 Makefile，为下一步的编译做准备，你可以通过在 configure 后加上参数来对安装进行控制，比如代码:

    ./configure –prefix=/usr 
意思是将该软件安装在 /usr 下面，执行文件就会安装在 /usr/bin （而不是默认的 /usr/local/bin),资源文件就会安装在 /usr/share（而不是默认         的/usr/local/share）。同时一些软件的配置文件你可以通过指定 –sys-config= 参数进行设定。有一些软件还可以加上 –with、–enable、–without、–       disable 等等参数对编译加以控制，你可以通过允许 ./configure –help 察看详细的说明帮助。

./configure是用来检测你的安装平台的目标特征的。比如它会检测你是不是有CC或GCC，并不是需要CC或GCC，它是个shell脚本。

make，这一步就是编译，大多数的源代码包都经过这一步进行编译（当然有些perl或python编写的软件需要调用perl或python来进行编译）。如果 在 make 过程中出现 error ，你就要记下错误代码（注意不仅仅是最后一行），然后你可以向开发者提交 bugreport（一般在 INSTALL 里有提交地址），或者你的系统少了一些依赖库等，这些需要自己仔细研究错误代码。

make insatll，这条命令来进行安装（当然有些软件需要先运行 make check 或 make test 来进行一些测试），这一步一般需要你有 root 权限（因为要向系统写入文件）。

利用 configure 所产生的 Makefile 文件有几个预设的目标可供使用，其中几个重要的简述如下：
* make all：产生我们设定的目标，即此范例中的可执行文件。只打make也可以，此时会开始编译原始码，然后连结，并且产生可执行文件。
* make clean：清除编译产生的可执行文件及目标文件(object file，*.o)。
* make distclean：除了清除可执行文件和目标文件外，把configure所产生的Makefile也清除掉* 。
* make install：将程序安装至系统中。如果原始码编译无误，且执行结果正确，便可以把程序安装至系统预设的可执行文件存放路径。如果用bin_PROGRAMS宏的话，程序会被安装至/usr/local/bin这个目录。
* make dist：将程序和相关的档案包装成一个压缩文件以供发布。执行完在目录下会产生一个以PACKAGE-VERSION.tar.gz为名称的文件。 PACKAGE和VERSION这两个变数是根据configure.in文件中AM_INIT_AUTOMAKE(PACKAGE，VERSION)的定义。在此范例中会产生test-1.0.tar.gz的档案。
* make distcheck：和make dist类似，但是加入检查包装后的压缩文件是否正常。这个目标除了把程序和相关文件包装成tar.gz文件外，还会自动把这个压缩文件解开，执行 configure，并且进行make all 的动作，确认编译无误后，会显示这个tar.gz文件可供发布了。这个检查非常有用，检查过关的包，基本上可以给任何一个具备GNU开发环境-的人去重新编译。

[例解 autoconf 和 automake 生成 Makefile 文件](https://www.ibm.com/developerworks/cn/linux/l-makefile/index.html)      

***

## redhat 系列

#### CentOS 7 安装 google-chrome 浏览器
1，下载 chrome 的 rpm 包 google-chrome-stable_current_x86_64.rpm       
2，切换到安装包目录，执行命令：        

    yum localinstall google-chrome-stable_current_x86_64.rpm 
3， 在 CentOS 7 下可能会报以下错误  
    
    Couldn't open file /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6    
4，到 CentOS 官网上 https://www.centos.org/keys/ 下载 CentOS 6 的 key，在 /etc/pki/rpm-gpg/ 目录下 创建 RPM-GPG-KEY-CentOS-6，拷贝 key，用以下命令验证以下 key 值：

    gpg --quiet --with-fingerprint /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
5，继续执行安装命令，如果报以下错误：   

    NOKEY Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
6，执行命令： 

    rpm --import /etc/pki/rpm-gpg/RPM*    
7，再执行安装命令即可。

***

## debian 系列
### deb 包安装方法  

#### 安装一个 Debian 软件包

    dpkg -i <package.deb>

#### 列出 <package.deb> 的内容

    dpkg -c <package.deb>

#### 从 <package.deb> 中提取包裹信息

    dpkg -I <package.deb>

#### 移除一个已安装的包裹

    dpkg -r <package>

#### 完全清除一个已安装的包裹。和 remove 不同的是，remove 只是删掉数据和可执行文件，purge 另外还删除所有的配制文件

    dpkg -P <package>

#### 列出 <package> 安装的所有文件清单。同时请看 dpkg -c 来检查一个 .deb 文件的内容

    dpkg -L <package>

#### 显示已安装包裹的信息。同时请看 apt-cache 显示 Debian 存档中的包裹信息，以及 dpkg -I 来显示从一个 .deb 文件中提取的包裹信息

    dpkg -s <package>

#### 重新配制一个已经安装的包裹，如果它使用的是 debconf (debconf 为包裹安装提供了一个统一的配制界面)

    dpkg-reconfigure <package>
 
 
> apt-cache 是linux下的一个apt软件包管理工具，它可查询apt的二进制软件包缓存文件。
参见： http://zwkufo.blog.163.com/blog/static/258825120092245519896/

## [debian 软件源更新](http://www.cnblogs.com/beanmoon/p/3387652.html)
修改 /etc/apt/sources.list 之后一般会运行下面两个命令进行更新升级：

        sudo apt-get update
        sudo apt-get dist-upgrade
其中 ：
   update - 取回更新的软件包列表信息
   dist-upgrade - 发布版升级
第一个命令仅仅更新的软件包列表信息，所以很快就能完成。
第二个命令是全面更新发布版，一般会下载几百兆的新软件包。
其实在运行完第一个命令后系统就会提示你进行更新升级。因为修改了源，所有这次更新的改动可能会很大，比如安装某个包可能会删除太多的其他包，所有系统会提示你运行“sudo apt-get dist-upgrade”进行全面升级或使用软件包管理器中的“标记全部软件包以便升级”功能进行升级。两者效果是一样的。

#### apt-get指令的 autoclean,clean,autoremove的[区别](http://blog.csdn.net/flydream0/article/details/8620396) 
apt-get autoclean:
    如果你的硬盘空间不大的话，可以定期运行这个程序，将已经删除了的软件包的.deb安装文件从硬盘中删除掉。如果你仍然需要硬盘空间的话，可以试试apt-get clean，这会把你已安装的软件包的安装包也删除掉，当然多数情况下这些包没什么用了，因此这是个为硬盘腾地方的好办法。

apt-get clean:
    类似上面的命令，但它删除包缓存中的所有包。这是个很好的做法，因为多数情况下这些包没有用了。但如果你是拨号上网的话，就得重新考虑了。

apt-get autoremove:
    删除为了满足其他软件包的依赖而安装的，但现在不再需要的软件包。

apt-get remove 软件包名称：
    删除已安装的软件包（保留配置文件）。

apt-get --purge remove 软件包名称：
     删除已安装包（不保留配置文件)。


## debian 网络配置

配置网卡
修改 /etc/network/interfaces 添加如下

auto eth0 #开机自动激活
iface eth0 inte static #静态IP
address 192.168.0.56 #本机IP
netmask 255.255.255.0 #子网掩码
gateway 192.168.0.254 #路由网关
 
如果是用DHCP自动获取，请在配置文件里添加如下：
auto eth0
iface eth0 inet dhcp

设置DNS
echo "nameserver 202.96.128.86" >> /etc/resolv.conf

重启一下网卡。
/etc/init.d/networking restart

## Debian 系命令
        
debian 的 runlevel级别定义如下：

    0 – Halt，关机模式
    1 – Single，单用户模式
    2 - Full multi-user with display manager (GUI)
    3 - Full multi-user with display manager (GUI)
    4 - Full multi-user with display manager (GUI)
    5 - Full multi-user with display manager (GUI)
    6 – Reboot，重启
可以发现2~5级是没有任何区别的。他们为多用户模式，这和一般的linux不一样。

sysv-rc-conf 是一个服务管理程序。

update-rc.d 类似与 RHEL 中的 chkconfig。

invoke-rc.d 类似与 RHEL 中的 service。


    echo $HISTSIZE
可以修改该参数，但重启电脑后失效，如需长久有效，在 /etc/profile 文件中配置。



## 常见问题
* error: watch ENOSPC
Run the below command:

    echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p


