# 配置静态网:
- 查看ip : ip addr 
- vi /etc/sysconfig/network-scripts/ifcfg-ens33
  - BOOTPROTO=static
  - ONBOOT=yes
  - IPADDR=
  - NETMASK=255.255.255.0
  - GATEWAY=
  - DNS1=114.114.114.114
- 重启网络设置 : systemctl restart network
- 192.168.120.200/24 : 24则是子网掩码:255.255.255.0
  - 因为三个255 就是3*8个1
- 本地cmd查看IP地址 : ipconfig -all
  
# final连接虚拟机

# shell
- Shell（也称为壳层）在计算机科学中指“为用户提供用户界面”的软件，通常指的是命令行界面的解析器。一般来说，这个词是指操作系统中提供访问内核所提供之服务的程序。Shell也用于泛指所有为用户提供操作界面的程序，也就是程序和用户交互的层面。因此与之相对的是内核（英语：Kernel），内核不提供和用户的交互功能。

- 不过这个词也拿来指应用软件，或是任何在特定组件外围的软件，例如浏览器或电子邮件软件是HTML排版引擎的Shell。Shell这个词是来自于操作系统（内核）与用户界面的外层界面。

- 通常将shell分为两类：命令行与图形界面。命令行壳层提供一个命令行界面（CLI）；而图形壳层提供一个图形用户界面（GUI）。

# 命令:

## 目录相关命令
- 显示绝对路径 : pwd
- 显示当前路径中的文件 : ls
  - 显示全部文件,包括隐藏文件 : ls -a(all)
  - 显示文件的详细内容 : ls -l(long)
    - 缩写就是 : ll
- cd 切换到指定目录
  - cd ./ : 当前目录
  - cd .// : 上一级目录
  - cd ` : 切换到当前用户的家目录
    - root为/root
    - 普通用户为/host
  - 只要带斜杠的都是绝对路径
- 创建目录 : mkdir
  - mkdir -p : 创建多级目录
- 删除目录 : rmdir
  - rmdir -p : 删除多级目录
- 拷贝 : cp
  - cp 源目录或文件 目标目录或文件
  - cd -r : 递归复制文件夹,复制文件夹时夹
- 移动或者重命名 : mv
  - mv 源目录 目标目录
- 删除 : rm
  - rm -r : 递归删除
  - rm -f : 强制删除
- 匹配全部 : * 


## 文件相关命令
- 创建文件 : touch
- 打印 : echo
  - echo aaa >> lin.txt : 将文字打入文件中
- 查看文件内容 : cat
  - cat -A : 列出特殊字符而非空行
  - cat -b : 列出行号,空白行不算行号
  - cat -n : 列出行号,空白行算行号
- 查看文件内容,一页一页显示 : more
  - 空格 : 向下翻一页
  - enter : 向下翻一行
- 查看文件头几行 : head
  - head -n 3 : 查看头三行
- 查看文件尾几行 : tail
  - tail -n 3 : 查看尾三行
  - tail -f : 查看文件修改的内容
- 查看 行数,单词数,字节数 : wc
  - wc -l : 查看行数
  - wc -w : 查看单词数
  - wc -c : 查看字节数
- 查看文件类型 : file
- 下载 : wget


## 查找相关命令
- find : 查找文件或目录
  - find -name : 按文件名查找
  - find -user : 按文件拥有者查找
  - find -size : 按文件大小查找
  - 用法 : find 查找范围 命令 条件
  - find / -name 1.txt
- grep : 查找内容
  - grep -c : 显示目标所在行
  - grep -n : 显示目标所在行以及行号
- which : 查找命令所在的目录


## 日期相关命令
- date
  - date -s : 设置时间


## 进程相关命令
- ps : 查看系统进程
  - ps -aux : 显示全部详细进程
  - ps -aux | grep *** : 结合管道命令查看有 ** 的进程

- kill : 关闭进程
  - kill -9 : 强迫进程结束


## 压缩解压
- tar : 打包
  - tar 命令 包名.tar 带打包内容  
  - -c create  :  生成tar打包文件
  - -x extract  :  解压tar文件
  - -v verbose  :  显示详细信息
  - -f file  :  指定解压后的名字
  - -z   :  打包同时压缩
  - -C  :  解压导指定目录
- zip . unzip : 需要yum install 安装下


## 系统状态检测命令
- ip addr : 查看系统ip
- netstat -nplt : 查看谁在连接了自己,并显示进程号
  - 但是需要 yum install -y net-tools
- uname : 查看系统内核等信息
  - -a : 查看系统完整信息
  - -s : 系统内核名称
  - -n : 节点名称
  - -r : 内核发行版
  - -v : 内核版本
  - -m : 硬件名称
  - -i : 硬件平台
  - -p : 处理器类型
  - -o : 操作系统名称
- uptime : 查看系统的负载信息
  - 负载值越低越好
- free : 查看内存使用情况
- who : 查看当前登入主机的用户终端信息
- history : 历史执行过的命令
- df : 查看磁盘容量
  - -h : 给人看的

## 关机命令
- reboot : 重启
- poweroff : 关机
- halt : 关机
- shutdown : 多选项关机
  - -h : 关机
  - -r : 重启
  - shutdown -h 1 "1 minites shutdown"
    - 会通知全部在使用该系统的人  "1 minites shutdown"

# 端口转发
- 原理
  - 建立两个局域网,即:
  - 将linux端口和自己本地端口连接,然后别人跟自己本地端口连接,即两个局域网
- 步骤
  - 在VMware上的 虚拟网络编辑器 上的 NAT设置 上的 端口转发

# 用户和组
- useradd ： 创建用户
  - 例如 ： useradd lh
  - 默认的组id ：lh
  - 顺带加入一个组的话：useradd -g 组名 用户名
- passwd : 创建密码
  - 例如 ： passwd lh
- su : 切换用户
  - su lh
- id ：查看用户信息
  - id lh
- groupadd : 创建组

- usermod ：修改用户信息
  - -l ：修改用户名
    - usermod -l lin lh
  - -g ：修改主组
    - 一个用户只能拥有一个主组
  - -G ：修改副组
    - 一个用户可以拥有多个副组
  - -u ：修改用户id

- userdel : 删除用户
- groupdel ：删除组

- **文件类型**
  - 普通文件类型  **-**
    - Linux中最多的一种文件类型, 包括 纯文本文件(ASCII)；二进制文件(binary)；数据格式的文件(data);各种压缩文件。第一个属性为 [-] 。
  - 目录文件类型  **d**
    - 在linux中，它的思想是一切皆是文件，目录文件也就是Windows中的目录，也就是能用 cd 命令进入的。第一个属性为 [d]，例如 [drwxr-xr-x]。
  - 字符设备文件  **c**
    - 即串行端口的接口设备，例如键盘、鼠标等等。第一个属性为 [c]。
  - 块设备文件  **b**
    - 即存储数据以供系统存取的接口设备，简单而言就是硬盘。例如一号硬盘的代码是 /dev/hda1等文件。第一个属性为 [b]。
  - 套接字文件  **s**
    - 这类文件通常用在网络数据连接。可以启动一个程序来监听客户端的要求，客户端就可以通过套接字来进行数据通信。第一个属性为 [s]，最常在 /var/run目录- 到这种文件类型。
  - 管道文件  **p**
    - FIFO也是一种特殊的文件类型，它主要的目的是，解决多个程序同时存取一个文件所造成的错误。FIFO是first-in-first-out(先进先出)的缩写。第一个属性为[p]。
  - 链接文件  **l**
    - 类似Windows下面的快捷方式。第一个属性为 [l]，例如 [lrwxrwxrwx]。


- u：用户本身；g：用户所在组；o：其他组
- r：可读；w：可写；x：可执行
- linux里面一切皆文件
  
- chmod ；改变文件权限
  - 添加权限：chmod +x lin.txt
  - 删除权限：chmod -x lin.txt
  - 特定的增减权限：chmod u+x lin.txt
- chown : 改变文件所属者
  - chown lh sup.txt

- 权限提升
  - sudoers ：在/etc/sudoers
  - 修改sudoers ：visudo
    - root  ALL=(ALL:ALL)  ALL
      - root用户  ALL 表示任何的主机都能执行  ALL：ALL 表示以root可以代表任何人的身份执行  ALL 表示任何命令
    - lh   192.168.120.1/24=(root) /usr/sbin/useradd
      - lh 在192.168.120.1/24网段上连接主机，并拥有root权限，并只能使用useradd的命令
  - 1.将用户添加进wheel 这个组里面，就能拥有很多权限
  - 2.或者在sudoers 里面的root后面加上用户名

- 创建快捷方式：
  - ln -s 源文件 现文件

# 安装jdk

- 配置环境变量
  1. 编辑配置文件：/etc/profile
  2. vim 进去， 拉到最底下，加上：
     1. JAVA_HOME=jdk的文件夹
     2. CLASSPATH=$JAVA_HOME/lib
     3. PATH=$PATH:$JAVA_PATH/bin
  3. 重新加载环境变量： source /etc/profile

# yum 常用命令：
- yum check-update : 列出需要更新的软件清单
- yum update ：更新所有需要更新的软件
- yum install <软件名>：安装指定软件
- yum update <软件名>: 更新指定软件
- yum list : 软件中心
- yum remove <软件名>: 卸载指定软件
- yum search <条件>: 查询条件软件 
- yum clear : 清楚缓存
  
- 更换源: 
- /etc/yum.repos.d/
- 默认选择CentOS-Base.repo 里面的网址下载
- 更换
  1. 备份 CentOs-Base.repo
  2. wget  http://mirrors.aliyun.com/repo/Centos-7.repo
  3. 清理缓存:  yum clean all
  4. 更新新的元数据缓存: yum makecache


# 装mysql
- 使用rpm安装
  - 网上下载时要中间带有rpm的tar安装包
  - [下载网址](https://dev.mysql.com/downloads/mysql/)
  1. 然后传进去并解包
  2. 安装依赖包
    -  yum -y install make gcc-c++ cmake bison-devel ncurses-devel libaio libaio-devel net-tools
  3. 卸载原有自带数据库:
    - rpm -qa | grep mariadb 
    - yum -y remove mariadb-libs
  4. 开始安装mysql(CentOS7安装MySQL8.0教程)
     1. rpm -ivh mysql-community-common-8.0.11-1.el7.x86_64.rpm --nodeps --force 命令安装 common
     2. rpm -ivh mysql-community-libs-8.0.11-1.el7.x86_64.rpm --nodeps --force 命令安装 libs
     3. rpm -ivh mysql-community-client-8.0.11-1.el7.x86_64.rpm --nodeps --force 命令安装 client
     4. rpm -ivh mysql-community-server-8.0.11-1.el7.x86_64.rpm --nodeps --force 命令安装 server
     5.  rpm -qa | grep mysql 命令查看 mysql 的安装包
  5. 启动服务
     1. systemctl start mysqld 启动服务
     2. systemctl enable mysqld 加入开机启动
     3. systemctl status mysqld 查看服务状态
     4. systemctl stop mysqld 停止服务
     5. systemctl restart mysqld 重启服务
     6. systemctl diable mysqld 去除开启启动


# 然后...
> 后面直接给我报错，于是就，不做笔记了！<br>
可能是MySQL版本的问题,但最后也就那么一丢丢内容了<br>
我相信你可以想象出来下面的笔记的 哈哈哈哈<br>
