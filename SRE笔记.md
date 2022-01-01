# 1 Linux 安装

### 1.1 Linux 生产主流版本 

###### 1.1.1Linux 各种版本 

CentOS 各版本介绍

 https://zh.wikipedia.org/wiki/CentOS 

RHEL各版本介绍 

 https://zh.wikipedia.org/wiki/Red_Hat_Enterprise_Linux 

Ubuntu 各版本介绍

 https://zh.wikipedia.org/wiki/Ubuntu https://blog.csdn.net/songfulu/article/details/85310273 

获取发行版

 CentOS

 https://wiki.centos.org/Download

 http://mirrors.aliyun.com 

http://mirrors.sohu.com 

http://mirrors.163.com

 https://mirrors.tuna.tsinghua.edu.cn/centos/ 

Ubuntu  

http://cdimage.ubuntu.com/releases/ 

Server版

 https://mirrors.tuna.tsinghua.edu.cn/ubuntu-cdimage/releases/

 http://releases.ubuntu.com/ 

Desktop版

 http://mirrors.aliyun.com/ubuntu-releases/

 https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/ 

Ubuntu20.04下载

http://mirrors.aliyun.com/ubuntu-releases/20.04.1/ubuntu-20.04.1-live-server-amd64.iso

# 2 Linux 基础入门 

### 内容概述 

* 用户 

* 终端 

* Shell介绍 

* 执行命令 

* 简单命令 

* Tab键补全 

* 命令行历史 

* bash快捷键 

* 帮助用法 

### 2.1 Linux 基础

#### 2.1.1 用户类型

* root 用户

  一个特殊的管理帐户 

  也被称为超级用户 

  root已接近完整的系统控制 

  对系统损害几乎有无限的能力 

  除非必要,不要登录为 root  

* 普通（ 非特权 ）用户 

  权限有限 

  造成损害的能力比较有限

#### 2.1.2 终端 terminal

设备终端：键盘、鼠标、显示器

##### 2.1.2.1 终端类型 

* 控制台终端： /dev/console  
* 串行终端：/dev/ttyS#  
* 虚拟终端：tty：teletypewriters， /dev/tty#，tty 可有n个，Ctrl+Alt+F# 
* 图形终端：startx, xwindows CentOS 6: Ctrl + Alt + F7 CentOS 7: 在哪个终端启动，即位于哪个虚拟终端 
* 伪终端：pty：pseudo-tty ， /dev/pts/# 如：SSH远程连接

##### 2.1.2.2 查看当前的终端设备

tty 命令可以查看当前所在终端

```bash
tty
```

范例：

```bash
[root@sh-lzy-Centos8 ~]# tty
/dev/pts/0
[root@sh-lzy-Centos8 ~]# 
```

#### 2.1.3 交互式接口

交互式接口：启动终端后，在终端设备附加一个交互式应用程序

##### 2.1.3.1 交互式接口类型

* GUI：Graphic User Interface 

  X protocol, window manager, desktop 

  Desktop: GNOME (C, 图形库gtk)， 

  KDE (C++,图形库qt) ,

  XFCE (轻量级桌面) 

* CLI：Command Line Interface shell程序

##### 2.1.3.2 什么是shell

Shell 是Linux系统的用户界面，提供了用户与内核进行交互操作的一种接口。

它接收用户输入的命令并 把它送入内核去执行 

 shell也被称为LINUX的命令解释器（command interpreter），Shell 本身是一个程序。

将用户输入的 命令行拆解为”命令名“与”参数“。

接着，根据命令名找到对应要执行的程序，对被执行的程序进行初始化，

然后将刚才解析出来的参数传给该程序并执行 

shell是一种高级程序设计语言，

提供了变量，函数，条件判断，循环等开发语言的功能 

由于Shell本身是个程序，所以它可以被任何用户自己开发的各种Shell所代替

##### 2.1.3.3 各种Shell

* sh：Steve Bourne  
* bash：Bourne-Again Shell，GPL，CentOS 和 Ubuntu 默认使用 
* csh：c shell , C 语言风格 
* tcsh 
* ksh ：Korn Shell, AIX 默认 shell  
* zsh： MacOS默认shell

##### 2.1.3.4 bash shell 

GNU Bourne-Again Shell(bash)是GNU计划中重要的工具软件之一，目前也是 Linux标准的shell，与 sh兼容 

显示当前使用的shell

```bash
[root@sh-lzy-Centos8 ~]# echo ${SHELL}
/bin/bash
```

显示当前系统所有的shell

```bash
[root@sh-lzy-Centos8 ~]# cat /etc/shells 
/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash
[root@sh-lzy-Centos8 ~]# 
```

#### 2.1.4 设置主机名

```bash
#临时生效 
hostname NAME 
#持久生效,支持CentOS7和Ubuntu18.04以上版本 
hostnamectl set-hostname NAME
```

范例

```bash
[root@sh-lzy-Centos8 ~]# hostname TJ-hxy-I-Love-You
[root@sh-lzy-Centos8 ~]# 
[root@sh-lzy-Centos8 ~]# hostname
TJ-hxy-I-Love-You
[root@sh-lzy-Centos8 ~]# 
```

注意： 

* 主机名不支持使用下划线，但支持横线，可使用字母，横线或数字组合 
* 有些软件对主机名有特殊要求

#### 2.1.5 命令提示符 prompt

登录Linux后，默认的系统命令提示符毫无没有个性，无法明显辨别生产和测试环境，而导致误操作。 

可以通过修改PS1变量实现个性的提示符格式，避免这种低级错误

范例：默认的提示符

```bash
#CentOS默认提示符 
[root@sh-lzy-Centos8 ~]# 

#Ubuntu默认提示符 
root@ubuntu1804:~#
```

\# 管理员 

$ 普通用户

**显示提示符格式**

```bash
[root@sh-lzy-Centos8 ~]# echo $PS1
[\u@\h \W]\$
[root@sh-lzy-Centos8 ~]# 
```

**修改提示符格式范例**

![image-20220101115411431](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220101115411431.png)

```bash
[root@sh-lzy-Centos8 ~]# PS1="\[\e[1;5;41;33m\][\u@\h \W]\\$\[\e[0m\]"
[root@sh-lzy-Centos8 ~]#
[root@sh-lzy-Centos8 ~]#
```

```bash
PS1="\[\e[1;32m\][\t \[\e[1;33m\]\u\[\e[35m\]@\h\[\e[1;31m\] \W\[\e[1;32m\]]\ [\e[0m\]\\$"
```

**提示符格式说明：** 

* \e 控制符\033  
* \u 当前用户  
* \h 主机名简称 
* \H 主机名 
* \w 当前工作目录 
* \W 当前工作目录基名 
* \t 24小时时间格式

* \T 12小时时间格式 

* ! 命令历史数 

* \# 开机后命令历史数

* |      |                      PS1：命令提示字符                       |
  | ---- | :----------------------------------------------------------: |
  | \d   |          可显示[星期 月 日]的日期格式，如【Mon Feb           |
  | \H   |                        完整的主机名；                        |
  | \h   |             仅取主机名在第一个小数点之前的名字；             |
  | \t   |            显示时间，为24小时格式【HH：MM：SS】；            |
  | \T   |            显示时间，为12小时格式【HH：MM：SS】；            |
  | \A   |              显示时间，为24小时格式【HH：MM】；              |
  | \@   |              显示时间，为12小时格式【am/pm】；               |
  | \u   |                     当前用户的账号名称；                     |
  | \v   |                       BASH的版本信息；                       |
  | \w   | 完整的工作目录名称，由根目录写起的目录名称，但是根目录会以~替换; |
  | \W   |   利用bashname函数取得的工作目录名称，仅列出最后一目录名；   |
  | \\#  |                      执行的第几个命令；                      |
  | \\$  |        提示字符，如果是root，提示字符为#，否则就是$；        |

范例：在CentOS系统实现持久保存提示符格式

```bash
[root@sh-lzy-Centos8 ~]# echo "PS1='[\u@\h \A \W]\$'" > /etc/profile.d/env.sh 
[root@sh-lzy-Centos8 ~]# 
[root@sh-lzy-Centos8 ~]# cat /etc/profile.d/env.sh 
PS1='[\u@\h \A \W]$'
[root@sh-lzy-Centos8 ~]# source /etc/profile.d/env.sh 
[root@sh-lzy-Centos8 12:40 ~]$
[root@sh-lzy-Centos8 12:40 ~]$
[root@sh-lzy-Centos8 12:40 ~]$
```

#### 2.1.6 执行命令

##### 2.1.6.1 执行命令过程

输入命令后回车

提请shell程序找到键入命令所对应的可执行程序或代码

并由其分析后提交给内核分 配资源将其运行起来

##### 2.1.6.2 shell中可执行的两类命令

* 内部命令：由shell自带的，而且通过某命令形式提供, ,用户登录后自动加载并常驻内存中
* 外部命令：在文件系统路径下有对应的可执行程序文件,当执行命令时才从磁盘加载至内存中,执行 完毕后从内存中删除

**区别指定的命令是内部或外部命令**

```bash
type command
```

范例: 查看是否存在对应内部和外部命令

```bash
[root@sh-lzy-Centos8 12:40 ~]$type -a echo 
echo is a shell builtin
echo is /usr/bin/echo
[root@sh-lzy-Centos8 12:47 ~]$
```

###### 2.1.6.2.1 内部命令相关

查看外部命令路径

```bash
which -a |--skip-alias  
whereis
```

**Hash缓存表**

系统初始hash表为空

当外部命令执行时，默认会从PATH路径下寻找该命令

找到后会将这条命令的 *路径* 记录到hash表中

当再次使用该命令时，shell解释器首先会查看hash表

*存在将执行之*

*如果不 存在，将会去PATH路径下寻找*

利用hash缓存表可大大提高命令的调用速率

hash 命令常见用法 

* hash 显示hash缓存 

```bash
[root@sh-lzy-Centos8 12:57 profile.d]#hash 
hits	command
   1	/usr/bin/clear
[root@sh-lzy-Centos8 12:57 profile.d]#
```

* hash -l 显示hash缓存，可作为输入使用 
* hash -p path name 将命令全路径path起别名为name 
* hash -t name 打印缓存中name的路径 
* hash -d name 清除name缓存 
* hash -r 清除缓存

##### 2.1.6.3 命令别名

对于经常执行的较长的命令，可以将其定义成较短的别名，以方便执行 

显示当前shell进程所有可用的命令别名

```bash
[root@sh-lzy-Centos8 13:00 /]#alias 
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias xzegrep='xzegrep --color=auto'
alias xzfgrep='xzfgrep --color=auto'
alias xzgrep='xzgrep --color=auto'
alias zegrep='zegrep --color=auto'
alias zfgrep='zfgrep --color=auto'
alias zgrep='zgrep --color=auto'
[root@sh-lzy-Centos8 13:00 /]#
```

定义别名NAME，其相当于执行命令VALUE

```bash
alias NAME='VALUE'
```

```bash
alias scandisk='echo - - - > /sys/class/scsi_host/host0/scan;echo - - - > /sys/class/scsi_host/host1/scan;echo - - - > /sys/class/scsi_host/host2/scan'
```

撤消别名：unalias

```bash
[root@sh-lzy-Centos8 13:03 /]#alias 
...
alias rm='rm -i'
alias scandisk='echo - - - > /sys/class/scsi_host/host0/scan;echo - - - > /sys/class/scsi_host/host1/scan;echo - - - > /sys/class/scsi_host/host2/scan'
alias xzegrep='xzegrep --color=auto'
...
[root@sh-lzy-Centos8 13:03 /]#unalias scandisk
[root@sh-lzy-Centos8 13:04 /]#alias 
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias xzegrep='xzegrep --color=auto'
alias xzfgrep='xzfgrep --color=auto'
alias xzgrep='xzgrep --color=auto'
alias zegrep='zegrep --color=auto'
alias zfgrep='zfgrep --color=auto'
alias zgrep='zgrep --color=auto'
[root@sh-lzy-Centos8 13:04 /]#
```

**注意**：在命令行中定义的别名，仅对当前shell进程有效 

如果想永久有效，要定义在配置文件中 

仅对当前用户：~/.bashrc 

对所有用户有效：/etc/bashr

* 编辑配置给出的新配置不会立即生效，bash进程重新读取配置文件

```bash
source /path/to/config_file 

. /path/to/config_file
```

##### 2.1.6.4 命令格式

```bash
COMMAND [OPTIONS...] [ARGUMENTS...] 
COMMAND [COMMAND] [COMMAND] ....
```

选项：用于启用或关闭命令的某个或某些功能 

* 短选项：UNIX 风格选项，-c 例如：-l, -h 
* 长选项：GNU风格选项，--word 例如：--all, --human 
* BSD风格选项： 一个字母，例如：a，使用相对较少

参数：命令的作用对象，比如:文件名，用户名等

范例：

```bash
[root@sh-lzy-Centos8 13:04 /]#id -u Lzy
1000
[root@sh-lzy-Centos8 13:17 /]#id -u root
0
[root@sh-lzy-Centos8 13:17 /]#ls -a
.   bin   data  etc   lib    media  opt   root  sbin  sys  usr
..  boot  dev   home  lib64  mnt    proc  run   srv   tmp  var
[root@sh-lzy-Centos8 13:18 /]#ls --all
.   bin   data  etc   lib    media  opt   root  sbin  sys  usr
..  boot  dev   home  lib64  mnt    proc  run   srv   tmp  var
[root@sh-lzy-Centos8 13:18 /]#ls -all
total 24
dr-xr-xr-x.  18 root root  236 Dec 19 12:01 .
dr-xr-xr-x.  18 root root  236 Dec 19 12:01 ..
lrwxrwxrwx.   1 root root    7 Jun 22  2021 bin -> usr/bin
dr-xr-xr-x.   6 root root 4096 Dec 19 12:32 boot
drwxr-xr-x.   2 root root    6 Dec 19 12:00 data
drwxr-xr-x.  19 root root 3220 Jan  1 12:49 dev
drwxr-xr-x. 142 root root 8192 Jan  1 12:49 etc
drwxr-xr-x.   3 root root   17 Dec 19 12:06 home
lrwxrwxrwx.   1 root root    7 Jun 22  2021 lib -> usr/lib
lrwxrwxrwx.   1 root root    9 Jun 22  2021 lib64 -> usr/lib64
drwxr-xr-x.   2 root root    6 Jun 22  2021 media
drwxr-xr-x.   3 root root   18 Dec 19 12:03 mnt
drwxr-xr-x.   2 root root    6 Jun 22  2021 opt
dr-xr-xr-x. 318 root root    0 Jan  1 12:49 proc
dr-xr-x---.   5 root root  240 Jan  1 12:57 root
drwxr-xr-x.  42 root root 1220 Jan  1 12:51 run
lrwxrwxrwx.   1 root root    8 Jun 22  2021 sbin -> usr/sbin
drwxr-xr-x.   2 root root    6 Jun 22  2021 srv
dr-xr-xr-x.  13 root root    0 Jan  1 12:49 sys
drwxrwxrwt.  21 root root 4096 Jan  1 12:57 tmp
drwxr-xr-x.  13 root root  158 Dec 19 12:02 usr
drwxr-xr-x.  21 root root 4096 Dec 19 12:31 var
```

**注意：** 

* 多个选项以及多参数和命令之间使用空白字符分隔 
* 取消和结束命令执行：Ctrl+c，Ctrl+d 
* 多个命令可以用 ";" 符号分开 
* 一个命令可以用\分成多行

#### 2.1.7 常见命令

##### 2.1.7.1 查看硬件信息

###### 2.1.7.1.1 查看 cpu 

lscpu命令可以查看cpu信息 

cat /proc/cpuinfo也可看查看到

```bash
[root@sh-lzy-Centos8 13:18 /]#lscpu 
Architecture:        x86_64								#显示CPU架构类型
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
CPU(s):              2									#显示CPU核数
On-line CPU(s) list: 0,1
Thread(s) per core:  1
Core(s) per socket:  1
Socket(s):           2
NUMA node(s):        1
Vendor ID:           GenuineIntel
BIOS Vendor ID:      GenuineIntel
CPU family:          6
Model:               158
Model name:          Intel(R) Core(TM) i7-9700K CPU @ 3.60GHz
BIOS Model name:     Intel(R) Core(TM) i7-9700K CPU @ 3.60GHz
Stepping:            13
CPU MHz:             3600.006
BogoMIPS:            7200.01
Hypervisor vendor:   VMware
Virtualization type: full
L1d cache:           32K
L1i cache:           32K
L2 cache:            256K
L3 cache:            12288K
NUMA node0 CPU(s):   0,1
Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon nopl xtopology tsc_reliable nonstop_tsc cpuid pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch cpuid_fault invpcid_single ssbd ibrs ibpb stibp ibrs_enhanced fsgsbase tsc_adjust bmi1 avx2 smep bmi2 invpcid mpx rdseed adx smap clflushopt xsaveopt xsavec xsaves arat md_clear flush_l1d arch_capabilities

```

```bash
[root@sh-lzy-Centos8 13:22 /]#cat /proc/cpuinfo 
processor	: 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 158
model name	: Intel(R) Core(TM) i7-9700K CPU @ 3.60GHz
stepping	: 13
microcode	: 0xbe
cpu MHz		: 3600.006
cache size	: 12288 KB
physical id	: 0
siblings	: 1
core id		: 0
cpu cores	: 1
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 22
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon nopl xtopology tsc_reliable nonstop_tsc cpuid pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch cpuid_fault invpcid_single ssbd ibrs ibpb stibp ibrs_enhanced fsgsbase tsc_adjust bmi1 avx2 smep bmi2 invpcid mpx rdseed adx smap clflushopt xsaveopt xsavec xsaves arat md_clear flush_l1d arch_capabilities
bugs		: spectre_v1 spectre_v2 spec_store_bypass swapgs itlb_multihit srbds
bogomips	: 7200.01
clflush size	: 64
cache_alignment	: 64
address sizes	: 43 bits physical, 48 bits virtual
power management:

processor	: 1
vendor_id	: GenuineIntel
cpu family	: 6
model		: 158
model name	: Intel(R) Core(TM) i7-9700K CPU @ 3.60GHz
stepping	: 13
microcode	: 0xbe
cpu MHz		: 3600.006
cache size	: 12288 KB
physical id	: 2
siblings	: 1
core id		: 0
cpu cores	: 1
apicid		: 2
initial apicid	: 2
fpu		: yes
fpu_exception	: yes
cpuid level	: 22
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon nopl xtopology tsc_reliable nonstop_tsc cpuid pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch cpuid_fault invpcid_single ssbd ibrs ibpb stibp ibrs_enhanced fsgsbase tsc_adjust bmi1 avx2 smep bmi2 invpcid mpx rdseed adx smap clflushopt xsaveopt xsavec xsaves arat md_clear flush_l1d arch_capabilities
bugs		: spectre_v1 spectre_v2 spec_store_bypass swapgs itlb_multihit srbds
bogomips	: 7200.01
clflush size	: 64
cache_alignment	: 64
address sizes	: 43 bits physical, 48 bits virtual
power management:

[root@sh-lzy-Centos8 13:22 /]#
```

###### 2.1.7.1.2 查看内存大小

```bash
[root@sh-lzy-Centos8 13:24 /]#free -mlh
              total        used        free      shared  buff/cache   available
Mem:          3.6Gi       1.0Gi       1.8Gi        13Mi       773Mi       2.4Gi
Low:          3.6Gi       1.8Gi       1.8Gi
High:            0B          0B          0B
Swap:         4.0Gi          0B       4.0Gi
[root@sh-lzy-Centos8 13:26 /]#
```

```bash
[root@sh-lzy-Centos8 13:26 /]#cat /proc/meminfo 
MemTotal:        3798596 kB
MemFree:         1939576 kB
MemAvailable:    2483952 kB
Buffers:            4392 kB
Cached:           728024 kB
SwapCached:            0 kB
Active:           301640 kB
Inactive:        1032316 kB
Active(anon):       2724 kB
Inactive(anon):   612424 kB
Active(file):     298916 kB
Inactive(file):   419892 kB
Unevictable:           0 kB
Mlocked:               0 kB
SwapTotal:       4194300 kB
SwapFree:        4194300 kB
Dirty:                 0 kB
Writeback:             0 kB
AnonPages:        599476 kB
Mapped:           258728 kB
Shmem:             13612 kB
KReclaimable:      59340 kB
Slab:             145124 kB
SReclaimable:      59340 kB
SUnreclaim:        85784 kB
KernelStack:       10528 kB
PageTables:        38052 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     6093596 kB
Committed_AS:    3119296 kB
VmallocTotal:   34359738367 kB
VmallocUsed:           0 kB
VmallocChunk:          0 kB
Percpu:           101376 kB
HardwareCorrupted:     0 kB
AnonHugePages:    292864 kB
ShmemHugePages:        0 kB
ShmemPmdMapped:        0 kB
FileHugePages:         0 kB
FilePmdMapped:         0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
Hugetlb:               0 kB
DirectMap4k:      243520 kB
DirectMap2M:     2902016 kB
DirectMap1G:     3145728 kB
[root@sh-lzy-Centos8 13:27 /]#
```

###### 2.1.7.1.3 查看硬盘和分区情况

```bash
[root@sh-lzy-Centos8 13:27 /]#lsblk 
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  500G  0 disk 
├─sda1   8:1    0  250G  0 part /
├─sda2   8:2    0  200G  0 part /data
├─sda3   8:3    0   10G  0 part /boot
├─sda4   8:4    0    1K  0 part 
└─sda5   8:5    0    4G  0 part [SWAP]
sr0     11:0    1 10.1G  0 rom  /run/media/Lzy/CentOS-8-5-2111-x86_64-dvd
[root@sh-lzy-Centos8 13:29 /]#
```

```bash
[root@sh-lzy-Centos8 13:29 /]#cat /proc/partitions 
major minor  #blocks  name

   8        0  524288000 sda
   8        1  262144000 sda1
   8        2  209715200 sda2
   8        3   10485760 sda3
   8        4          1 sda4
   8        5    4194304 sda5
  11        0   10541056 sr0
[root@sh-lzy-Centos8 13:30 /]#
```

##### 2.1.7.2 查看系统版本信息

###### 2.1.7.2.1 查看系统架构

```bash
[root@sh-lzy-Centos8 13:32 /]#arch 
x86_64
[root@sh-lzy-Centos8 13:32 /]#
```

###### 2.1.7.2.2 查看内核版本

```bash
[root@sh-lzy-Centos8 13:32 /]#uname -r
4.18.0-348.el8.x86_64
[root@sh-lzy-Centos8 13:33 /]#
```

###### 2.1.7.2.3 查看操作系统发行版本

```bash
[root@sh-lzy-Centos8 13:33 /]#cat /etc/redhat-release 
CentOS Linux release 8.5.2111
[root@sh-lzy-Centos8 13:34 /]#cat /etc/os-release 
NAME="CentOS Linux"
VERSION="8"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="8"
PLATFORM_ID="platform:el8"
PRETTY_NAME="CentOS Linux 8"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:8"
HOME_URL="https://centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"
CENTOS_MANTISBT_PROJECT="CentOS-8"
CENTOS_MANTISBT_PROJECT_VERSION="8"
```

##### 2.1.7.3 日期和时间

Linux的两种时钟 

* 系统时钟：由Linux内核通过CPU的工作频率进行的 
* 硬件时钟：主板

**相关命令** 

* date 显示和设置系统时间

```bash
[root@sh-lzy-Centos8 13:35 /]#date 
Sat Jan  1 13:38:54 CST 2022

[root@sh-lzy-Centos8 13:39 /]#date +%s
1641015576

[root@sh-lzy-Centos8 13:40 /]#date -d @`date +%s`
Sat Jan  1 13:40:51 CST 2022
[root@sh-lzy-Centos8 13:40 /]#
```

* clock，hwclock: 显示硬件时钟

```
-s, --hctosys #以硬件时钟为准，校正系统时钟 
-w, --systohc #以系统时钟为准，校正硬件时钟
```

* 时区：

```bash
[root@sh-lzy-Centos8 13:44 /]#ll /etc/localtime 
lrwxrwxrwx. 1 root root 35 Dec 19 13:24 /etc/localtime -> ../usr/share/zoneinfo/Asia/Shanghai
```

* 显示日历：

```bash
[root@sh-lzy-Centos8 13:45 /]#cal -y
                               2022                               

       January               February                 March       

Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa
                   1          1  2  3  4  5          1  2  3  4  5
 2  3  4  5  6  7  8    6  7  8  9 10 11 12    6  7  8  9 10 11 12
 9 10 11 12 13 14 15   13 14 15 16 17 18 19   13 14 15 16 17 18 19
16 17 18 19 20 21 22   20 21 22 23 24 25 26   20 21 22 23 24 25 26
23 24 25 26 27 28 29   27 28                  27 28 29 30 31      
30 31                                                             
        April                   May                   June        
Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa
                1  2    1  2  3  4  5  6  7             1  2  3  4
 3  4  5  6  7  8  9    8  9 10 11 12 13 14    5  6  7  8  9 10 11
10 11 12 13 14 15 16   15 16 17 18 19 20 21   12 13 14 15 16 17 18
17 18 19 20 21 22 23   22 23 24 25 26 27 28   19 20 21 22 23 24 25
24 25 26 27 28 29 30   29 30 31               26 27 28 29 30      
                                                                  

        July                  August                September     

Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa
                1  2       1  2  3  4  5  6                1  2  3
 3  4  5  6  7  8  9    7  8  9 10 11 12 13    4  5  6  7  8  9 10
10 11 12 13 14 15 16   14 15 16 17 18 19 20   11 12 13 14 15 16 17
17 18 19 20 21 22 23   21 22 23 24 25 26 27   18 19 20 21 22 23 24
24 25 26 27 28 29 30   28 29 30 31            25 26 27 28 29 30   
31                                                                
       October               November               December      
Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa
                   1          1  2  3  4  5                1  2  3
 2  3  4  5  6  7  8    6  7  8  9 10 11 12    4  5  6  7  8  9 10
 9 10 11 12 13 14 15   13 14 15 16 17 18 19   11 12 13 14 15 16 17
16 17 18 19 20 21 22   20 21 22 23 24 25 26   18 19 20 21 22 23 24
23 24 25 26 27 28 29   27 28 29 30            25 26 27 28 29 30 31
30 31      
```

##### 2.1.7.4 关机和重启

关机： 

* halt 
* poweroff 

重启： reboot

* -f: 强制，不调用shutdown 

* -p: 切断电源 

关机或重启：shutdown

```bash
shutdown [OPTION]... [TIME] [MESSAGE]
```

-r: reboot 

-h: halt 

-c：cancel 

TIME：无指定，默认相当于+1（CentOS7） 

​	now: 立刻,相当于+0 

​	+#: 相对时间表示法，几分钟之后；例如 +3 

​	hh:mm: 绝对时间表示，指明具体时间

##### 2.1.7.5 用户登录信息查看命令

* whoami: 显示当前登录有效用户 
* who: 系统当前所有的登录会话 
* w: 系统当前所有的登录会话及所做的操作

##### 2.1.7.6 文本编辑

* nano 工具可以实现文本的编辑，上手容易，适合初学者 

* gedit 工具是图形工具

##### 2.1.7.7 会话管理

命令行的典型使用方式是，打开一个终端窗口（terminal window，以下简称"窗口"），在里面输入命 令。用户与计算机的这种临时的交互，称为一次"会话"（session）

会话的一个重要特点是，窗口与其中启动的进程是连在一起的。打开窗口，会话开始；关闭窗口，会话 结束，会话内部的进程也会随之终止，不管有没有运行完

* screen

利用screen 可以实现会话管理,如：新建会话,共享会话等 

**注意：**CentOS7 来自于base源，CentOS8 来自于epel源

安装screen

```bash
#CentOS7 安装screen 
[root@centos7 ~]#yum -y install screen  

#CentOS8 安装screen
[root@centos8 ~]#dnf -y install epel-release
[root@centos8 ~]#dnf -y install screen
```

screen命令常见用法： 

* 创建新screen会话  

  screen –S [SESSION] 

* 加入screen会话 

  screen –x [SESSION] 

* 退出并关闭screen会话 

  exit 

* 剥离当前screen会话 

  Ctrl+a,d 

* 显示所有已经打开的screen会话 

  screen -ls 

* 恢复某screen会话 

  screen -r [SESSION]

##### 2.1.7.8 输出信息 echo

###### 2.1.7.8.1 echo 基本用法

* echo 命令可以将后面跟的字符进行输出 

功能：显示字符

echo会将输入的字符串送往标准输出。

输出的字符串间以空白字符隔开, 并在最后加 上换行号 

* 语法：

```
echo [-neE][字符串]
选项：
-E （默认）不支持 \ 解释功能
-n 不自动换行
-e 启用 \ 字符的解释功能

显示变量
echo "$VAR_NAME”   #用变量值替换，弱引用
echo '$VAR_NAME’   #变量不会替换，强引
```

启用命令选项-e，若字符串中出现以下字符，则特别加以处理，而不会将它当成一般文字输出 

* \a 发出警告声 
* \b 退格键 
* \c 最后不加上换行符号 
* \e escape，相当于\033 
* \n 换行且光标移至行首 
* \r  回车，即光标移至行首，但不换行 
* \t  插入tab 
* \\\  插入\字符 
* \0nnn 插入nnn（八进制）所代表的ASCII字符 
* \x HH插入HH（十六进制）所代表的ASCII数字（man 7 ascii）

```bash
[root@sh-lzy-Centos8 14:51 /]#echo -e 'a\x0Ab'
a
b
[root@sh-lzy-Centos8 14:59 /]#echo \PATH
PATH
[root@sh-lzy-Centos8 15:00 /]#echo \$PATH
$PATH
[root@sh-lzy-Centos8 15:00 /]#echo '$PATH'
$PATH
[root@sh-lzy-Centos8 15:00 /]#echo "$PATH"
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
[root@sh-lzy-Centos8 15:00 /]#
```

###### 2.1.7.8.2 echo 高级用法

在终端中，ANSI定义了用于屏幕显示的Escape屏幕控制码 

具有颜色的字符，其格式如下:

```
"\033[字符背景颜色;字体颜色m字符串\033[0m" 
```

\033[30m -- \033[37m 设置前景色 

\033[40m -- \033[47m 设置背景色 

```
#字符背景颜色范围: 40--47           
40:黑             
41:红             
42:绿 
43:黄 
44:蓝 
45:紫 
46:深绿            
47:白色 
```

```
#字体颜色: 30--37 
30: 黑 
31: 红 
32: 绿 
33: 黄 
34: 蓝 
35: 紫 
36: 深绿 
```

加颜色只是以下控制码中的一种，下面是常见的一些ANSI控制码： 

```
\033[0m  关闭所有属性  
\033[1m  设置高亮度  
\033[4m  下划线  
\033[5m  闪烁  
\033[7m  反显  
\033[8m  消隐  
\033[nA  光标上移n行  
\033[nB  光标下移n行  
\033[nC  光标右移n列  
\033[nD  光标左移n列  
\033[x;yH 设置光标位置x行y列  
\033[2J  清屏  
\033[K  清除从光标到行尾的内容  
\033[s  保存光标位置  
\033[u  恢复光标位置  
\033[?25l  隐藏光标  
\033[?25h  显示光标 
\033[2J\033[0;0H 清屏且将光标置顶
```

#### 2.1.8 字符集和编码 

许多场合下，字符集与编码这两个概念常被混为一谈，但两者是有差别的。

字符集与字符集编码是两个 不同层面的概念 

charset是character set的简写，即字符集，即二进制和字符的对应关系，不关注最终的存储形式 

encoding是charset encoding的简写，即字符集编码，简称编码，实现如何将字符转化为实际的二进制 进行存储或相反，编码决定了空间的使用的大小 

##### 2.1.8.1 ASCII码 

查看ASCII码表

```bash
[root@sh-lzy-Centos8 15:00 /]#dnf -y install man-pages
Last metadata expiration check: 0:11:16 ago on Sat 01 Jan 2022 02:57:46 PM CST.
Package man-pages-4.15-6.el8.x86_64 is already installed.
Dependencies resolved.
Nothing to do.
Complete!
[root@sh-lzy-Centos8 15:09 /]#man ascii
```

##### 2.1.8.2 Unicode

由于计算机是美国人发明的，因此，最早只有127个字母被编码到计算机里，即ASCII编码

但是要处理 中文显然一个字节是不够的，至少需要两个字节，而且还不能和ASCII编码冲突，所以，中国制定了 GB2312编码，用来把中文编进去。 

全世界有上百种语言，日本把日文编到Shift_JIS里，韩国把韩文编到Euc-kr里，各国有各国的标准，就 会不可避免地出现冲突，结果就是，在多语言混合的文本中，显示出来会有乱码 

为了表示世界上所有语言中的所有字符。每一个符号都给予一个独一无二的编码数字，Unicode 是一个 很大的集合，现在的规模可以容纳100多万个符号。Unicode 仅仅只是一个字符集，规定了每个字符对 应的二进制代码，至于这个二进制代码如何存储则没有规定

**Unicode编码方案：** 

* UTF-8：变长，1到4个字节 
* UTF-16：变长，2或4个字节 
* UTF-32：固定长度，4个字节

UTF-8 是目前互联网上使用最广泛的一种 Unicode 编码方式，可变长存储。使用 1 - 4 个字节表示一个 字符，根据字符的不同变换长度。

编码规则如下：

 对于单个字节的字符，第一位设为 0，后面的 7 位对应这个字符的 Unicode 码。因此，对于英文中的 0  - 127 号字符，与 ASCII 码完全相同。这意味着 ASCII 码的文档可用 UTF-8 编码打开 

对于需要使用 N 个字节来表示的字符（N > 1），第一个字节的前 N 位都设为 1，第 N + 1 位设为0，剩 余的 N - 1 个字节的前两位都设位 10，剩下的二进制位则使用这个字符的 Unicode 码来填充

**编码转换和查询参考链接：**

https://home.unicode.org/

https://unicode.yunser.com/unicode

http://www.chi2ko.com/tool/CJK.htm

https://www.bejson.com/convert/unicode_chinese/

https://javawind.net/tools/native2ascii.jsp?action=transform

http://tool.oschina.net/encode

http://web.chacuo.net/charsetescape

**Unicode和UTF-8：**

| Unicode符号范围(十六进制) | UTF-8编码方式二进制）               |
| ------------------------- | ----------------------------------- |
| 0000 0000-0000 007F       | 0xxxxxxx                            |
| 0000 0080-0000 07FF       | 110xxxxx 10xxxxxx                   |
| 0000 0800-0000 FFFF       | 1110xxxx 10xxxxxx 10xxxxxx          |
| 0001 0000-0010 FFFF       | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx |

#### 2.1.9 命令行扩展和被括起来的集合

##### 2.1.9.1 命令行扩展：`` 和 $(）

把一个命令的输出打印给另一个命令的参数

放在``中的一定是有输出信息的命令

```bash
$(COMMAND) 或 `COMMAND`
```

```bash
[root@sh-lzy-Centos8 15:21 /]#echo "echo $HOSTNAME"
echo sh-lzy-Centos8.5
[root@sh-lzy-Centos8 15:21 /]#echo 'echo $HOSTNAME'
echo $HOSTNAME
[root@sh-lzy-Centos8 15:22 /]#echo `echo $HOSTNAME`
sh-lzy-Centos8.5
[root@sh-lzy-Centos8 15:22 /]#

#结论：
#单引号：强引用,六亲不认，变量和命令都不识别，都当成了普通的字符串,"最傻"
#双引号：弱引用,不能识别命令，可以识别变量,"半傻不精"
#反向单引号：里面的内容必须是能执行的命令并且有输出信息,变量和命令都识别，并且会将反向单引号的内
#容当成命令进行执行后，再交给调用反向单引号的命令继续,"最聪明"
```

范例：

```bash
[root@sh-lzy-Centos8 15:22 /]#echo "This system's name is $HOSTNAME"
This system's name is sh-lzy-Centos8.5
[root@sh-lzy-Centos8 15:24 /]#echo "I am `whoami`"
I am root
[root@sh-lzy-Centos8 15:25 /]#cd /data/
[root@sh-lzy-Centos8 15:25 data]#touch $(date +%F).log
[root@sh-lzy-Centos8 15:25 data]#ls
2022-01-01.log
[root@sh-lzy-Centos8 15:25 data]#ll
total 0
-rw-r--r--. 1 root root 0 Jan  1 15:25 2022-01-01.log
[root@sh-lzy-Centos8 15:25 data]#touch `date +%F`.txt
[root@sh-lzy-Centos8 15:26 data]#ls
2022-01-01.log  2022-01-01.txt
[root@sh-lzy-Centos8 15:26 data]#
```

##### 2.1.9.2 括号扩展：{ }

{} 可以实现打印重复字符串的简化形式

```
{元素1,元素2,元素3} 
{元素1..元素2}
```

```bash
#范例：
[root@sh-lzy-Centos8 15:26 data]#echo file{1,3,5}
file1 file3 file5
[root@sh-lzy-Centos8 15:29 data]#rm -f file{1,3,5}
[root@sh-lzy-Centos8 15:29 data]#echo {1..10}
1 2 3 4 5 6 7 8 9 10
```

```bash
#范例：
[root@sh-lzy-Centos8 15:29 data]#echo {00..30..2}
00 02 04 06 08 10 12 14 16 18 20 22 24 26 28 30
[root@sh-lzy-Centos8 15:31 data]#echo {A..Z}
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
[root@sh-lzy-Centos8 15:31 data]#echo {A..z}
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z [  ] ^ _ ` a b c d e f g h i j k l m n o p q r s t u v w x y z
[root@sh-lzy-Centos8 15:31 data]#
```

#### 2.1.10 tab 键补全

tab 键可以实现命令、路径、文件等补全，提高输入效率，避免出错

##### 2.1.10.1 命令补全

* 内部命令： 
* 外部命令：bash根据PATH环境变量定义的路径，自左而右在每个路径搜寻以给定命令名命名的文 件，第一次找到的命令即为要执行的命令 
* 命令的子命令补全，需要安装 bash-completion

**注意：**用户给定的字符串只有一条惟一对应的命令，直接补全，否则，再次Tab会给出列表

##### 2.1.10.2 路径补全

把用户给出的字符串当做路径开头，并在其指定上级目录下搜索以指定的字符串开头的文件名 

如果惟一：则直接补全 

否则：再次Tab给出列表

##### 2.1.10.3 双击Tab键

* command 2Tab 所有子命令或文件补全 
* string2Tab 以string开头命令 
* /2Tab 显示所有根目录下一级目录，包括隐藏目录 
* ./2Tab 当前目录下子目录，包括隐藏目录 
* *2Tab 当前目录下子目录，不包括隐藏目录 
* ~2Tab 所有用户列表 
* $2Tab 所有变量 
* @2Tab /etc/hosts记录 （centos7 不支持） 
* =2Tab 相当于ls –A （centos7不支持）

#### 2.1.11 命令行历史

当执行命令后，系统默认会在内存记录执行过的命令 

当用户正常退出时，会将内存的命令历史存放对应历史文件中，默认是 ~/.bash_history 

登录shell时，会读取命令历史文件中记录下的命令加载到内存中 

登录进shell后新执行的命令只会记录在内存的缓存区中；这些命令会用户正常退出时“追加”至命令历史 文件中 

利用命令历史。可以用它来重复执行命令，提高输入效率

* 命令：history

```bash
history [-c] [-d offset] [n]
history -anrw [filename]
history -ps arg [arg...]

#常用选项
-c: 清空命令历史
-d offset: 删除历史中指定的第offset个命令
n: 显示最近的n条历史
-a: 追加本次会话新执行的命令历史列表至历史文件
-r: 读历史文件附加到历史列表
-w: 保存历史列表到指定的历史文件
-n: 读历史文件中未读过的行到历史列表
-p: 展开历史参数成多行，但不存在历史列表中
-s: 展开历史参数成一行，附加在历史列表后
```

* 命令历史相关环境变量

```
HISTSIZE：命令历史记录的条数 
HISTFILE：指定历史文件，默认为~/.bash_history 
HISTFILESIZE：命令历史文件记录历史的条数 
HISTTIMEFORMAT="%F %T `whoami` "  显示时间和用户 
HISTIGNORE="str1:str2*:…" 忽略str1命令，str2开头的历史 

HISTCONTROL：控制命令历史的记录方式 
ignoredups 是默认值，可忽略重复的命令，连续且相同为“重复” 
ignorespace 忽略所有以空白开头的命令 
ignoreboth  相当于ignoredups, ignorespace的组合 
erasedups  删除重复命令
```

* 持久保存变量

以上变量可以 export 变量名="值" 形式存放在 /etc/profile 或 ~/.bash_profile

```bash
[root@sh-lzy-Centos8 16:01 ~]#cat .bash_profile

# .bash_profile

# Get the aliases and functions

if [ -f ~/.bashrc ]; then
	. ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/bin

export PATH
export HISTCONTROL=ignoreboth
export HISTTIMEFORMAT="%F %T "
[root@sh-lzy-Centos8 16:02 ~]#history 5
  261  2022-01-01 16:01:19 vi .bash_profile 
  262  2022-01-01 16:01:42 source .bash_profile 
  263  2022-01-01 16:01:45 history 
  264  2022-01-01 16:02:56 cat .bash_profile
  265  2022-01-01 16:03:00 history 5
[root@sh-lzy-Centos8 16:03 ~]#
```

#### 2.1.12 调用命令行历史

```
#重复前一个命令方法 
重复前一个命令使用上方向键，并回车执行 
按 !! 并回车执行 
输入!-1 并回车执行 
按 Ctrl+p 并回车执行 

!:0 执行前一条命令（去除参数） 
!n 执行history命令输出对应序号n的命令 
!-n 执行history历史中倒数第n个命令 
!string 重复前一个以“string”开头的命令 
!?string 重复前一个包含string的命令 
!string:p 仅打印命令历史，而不执行 
!$:p 打印输出 !$ （上一条命令的最后一个参数）的内容 
!*:p 打印输出 !*（上一条命令的所有参数）的内容 
^string 删除上一条命令中的第一个string 
^string1^string2 将上一条命令中的第一个string1替换为string2 
!:gs/string1/string2 将上一条命令中所有的string1都替换为 string2 
使用up（向上）和down（向下）键来上下浏览从前输入的命令 
ctrl-r来在命令历史中搜索命令 （reverse-i-search）`’： 
Ctrl+g：从历史搜索模式退出 
```

```
#要重新调用前一个命令中最后一个参数 
!$ 表示前一个命令中最后一个参数 
Esc, .  点击Esc键后松开，然后点击 . 键 
Alt+ .  按住Alt键的同时点击 . 键 

command !^  利用上一个命令的第一个参数做command的参数 
command !$  利用上一个命令的最后一个参数做command的参数 command 
!*  利用上一个命令的全部参数做command的参数 command 
!:n 利用上一个命令的第n个参数做command的参数 command 
!n:^ 调用第n条命令的第一个参数 command 
!n:$ 调用第n条命令的最后一个参数 command 
!n:m 调用第n条命令的第m个参数 command 
!n:* 调用第n条命令的所有参数 command 
!string:^ 从命令历史中搜索以 string 开头的命令，并获取它的第一个参数 command 
!string:$ 从命令历史中搜索以 string 开头的命令,并获取它的最后一个参数 command 
!string:n 从命令历史中搜索以 string 开头的命令，并获取它的第n个参数 command 
!string:* 从命令历史中搜索以 string 开头的命令，并获取它的所有参数
```

#### 2.1.13 bash的快捷键

+ Ctrl+l 清屏，相当于clear命令 
+ Ctrl + o 执行当前命令，并重新显示本命令 
+ Ctrl + s 阻止屏幕输出，锁定 
+ Ctrl + q 允许屏幕输出，解锁 
+ Ctrl + c 终止命令 
+ Ctrl + z 挂起命令 
+ Ctrl + a 光标移到命令行首，相当于Home 
+ Ctrl + e 光标移到命令行尾，相当于End 
+ Ctrl + f 光标向右移动一个字符 
+ Ctrl + b 光标向左移动一个字符 
+ Ctrl + xx 光标在命令行首和光标之间移动 
+ Alt + f 光标向右移动一个单词尾 
+ Alt + b 光标向左移动一个单词首 
+ Ctrl + u 从光标处删除至命令行首 
+ Ctrl + k 从光标处删除至命令行尾 
+ Alt + r   删除当前整行 
+ Ctrl + w 从光标处向左删除至单词首 
+ Alt + d 从光标处向右删除至单词尾 
+ Alt + Backspace 删除左边单词 
+ Ctrl + d 删除光标处的一个字符 
+ Ctrl + h 删除光标前的一个字符 
+ Ctrl + y 将删除的字符粘贴至光标后 
+ Alt + c 从光标处开始向右更改为首字母大写的单词 
+ Alt + u 从光标处开始，将右边一个单词更改为大写 
+ Alt + l 从光标处开始，将右边一个单词更改为小写 
+ Ctrl + t 交换光标处和之前的字符位置 
+ Alt + t 交换光标处和之前的单词位置 
+ Alt + # 提示输入指定字符后，重复显示该字符#次

**注意：**Alt 组合快捷键经常和其它软件冲突 

### 2.2 获得帮助

* whatis 
* command --help 
* man and info 
* /usr/share/doc/ 
* Red Hat documentation 、Ubuntu documentation 
* 软件项目网站 
* 其它网站 
* 搜索

#### 2.2.1 whatis

whatis 使用数据库来显示命令的简短描述 

此工具在系统刚安装后，不可立即使用，需要制作数据库后才可使用 

* 执行下面命令生成数据库

```bash
#CentOS 7 版本以后 
mandb  
#CentOS 6 版本之前 
makewhatis
```

```bash
[root@sh-lzy-Centos8 16:19 ~]#mandb
Processing manual pages under /usr/share/man/overrides...
Updating index cache for path `/usr/share/man/overrides/man8'. Wait...done.
Checking for stray cats under /usr/share/man/overrides...
Checking for stray cats under /var/cache/man/overrides...
......
119 man subdirectories contained newer manual pages.
7828 manual pages were added.
0 stray cats were added.
0 old database entries were purged.
[root@sh-lzy-Centos8 16:19 ~]#whatis cd
cd (1)               - bash built-in commands, see bash(1)
cd (1p)              - change the working directory
[root@sh-lzy-Centos8 16:20 ~]#
```

#### 2.2.2 查看命令的帮助

##### 2.2.2.1 内部命令帮助

* help COMMAND  
* man bash

```bash
[root@sh-lzy-Centos8 16:20 ~]#type history 
history is a shell builtin
[root@sh-lzy-Centos8 16:24 ~]#type cd
cd is a shell builtin
[root@sh-lzy-Centos8 16:24 ~]#type screen
-bash: type: screen: not found
[root@sh-lzy-Centos8 16:24 ~]#type man
man is /usr/bin/man
```

##### 2.2.2.2 外部命令及软件帮助

* COMMAND --help 或 COMMAND -h 

* 使用 man 手册(manual)： man COMMAND 

* 信息页：info COMMAND 

* 程序自身的帮助文档：README、INSTALL、ChangeLog 

* 程序官方文档 相关网站，如：技术论坛 

* 搜索引擎

#### 2.2.3 外部命令的--help 或 -h 选项

显示用法总结和参数列表，大多数命令使用，但并非所有的

```bash
[root@sh-lzy-Centos8 16:24 ~]#cd --help
cd: cd [-L|[-P [-e]] [-@]] [dir]
    Change the shell working directory.
    
    Change the current directory to DIR.  The default DIR is the value of the
    HOME shell variable.
    
    The variable CDPATH defines the search path for the directory containing
    DIR.  Alternative directory names in CDPATH are separated by a colon (:).
    A null directory name is the same as the current directory.  If DIR begins
    with a slash (/), then CDPATH is not used.
    
    If the directory is not found, and the shell option `cdable_vars' is set,
    the word is assumed to be  a variable name.  If that variable has a value,
    its value is used for DIR.
    
    Options:
      -L	force symbolic links to be followed: resolve symbolic
    		links in DIR after processing instances of `..'
      -P	use the physical directory structure without following
    		symbolic links: resolve symbolic links in DIR before
    		processing instances of `..'
      -e	if the -P option is supplied, and the current working
    		directory cannot be determined successfully, exit with
    		a non-zero status
      -@	on systems that support it, present a file with extended
    		attributes as a directory containing the file attributes
    
    The default is to follow symbolic links, as if `-L' were specified.
    `..' is processed by removing the immediately previous pathname component
    back to a slash or the beginning of DIR.
    
    Exit Status:
    Returns 0 if the directory is changed, and if $PWD is set successfully when
    -P is used; non-zero otherwise.
    
[root@sh-lzy-Centos8 16:28 ~]#type --help
type: type [-afptP] name [name ...]
    Display information about command type.
    
    For each NAME, indicate how it would be interpreted if used as a
    command name.
    
    Options:
      -a	display all locations containing an executable named NAME;
    		includes aliases, builtins, and functions, if and only if
    		the `-p' option is not also used
      -f	suppress shell function lookup
      -P	force a PATH search for each NAME, even if it is an alias,
    		builtin, or function, and returns the name of the disk file
    		that would be executed
      -p	returns either the name of the disk file that would be executed,
    		or nothing if `type -t NAME' would not return `file'
      -t	output a single word which is one of `alias', `keyword',
    		`function', `builtin', `file' or `', if NAME is an alias,
    		shell reserved word, shell function, shell builtin, disk file,
    		or not found, respectively
    
    Arguments:
      NAME	Command name to be interpreted.
    
    Exit Status:
    Returns success if all of the NAMEs are found; fails if any are not found.
```

格式说明： 

* [] 表示可选项 
* CAPS或 <> 表示变化的数据 
* ... 表示一个列表 
* x |y| z 的意思是“ x 或 y 或 z ” 
* -abc的 意思是 -a -b –c 
* { } 表示分组

#### 2.2.4 man 命令

man 提供命令帮助的文件,手册页存放在/usr/share/man 

几乎每个命令都有man的“页面” 

中文man需安装包 

* man-pages 
* man-pages-zh-CN

**man 页面分组**

不同类型的帮助称为不同的“章节”，统称为Linux手册，man 1 

* 1：用户命令 
* 2：系统调用 
* 3：C库调用 
* 4：设备文件及特殊文件 
* 5：配置文件格式 
* 6：游戏 
* 7：杂项 
* 8：管理类的命令 
* 9：Linux 内核API

**man命令的配置文件：**

```bash
#CentOS 6 之前版 man 的配置文件 
/etc/man.config  
#CentOS 7 之后版 man 的配置文件 
/etc/man_db.conf 
#ubuntu man 的配置文件 
/etc/manpath.config

#格式：
MANPATH /PATH/TO/SOMEWHERE   #指明man文件搜索位置
#也可以指定位置下搜索COMMAND命令的手册页并显示
man -M /PATH/TO/SOMEWHERE COMMAND
#查看man手册页
man [OPTION...] [SECTION] PAGE...
man [章节] keyword
```

**man 帮助段落说明**

* NAME 名称及简要说明 
* SYNOPSIS 用法格式说明 
* [] 可选内容 
* <> 必选内容 
* a|b 二选一 
* { } 分组 
* ... 同一内容可出现多次 
* DESCRIPTION 详细说明 
* OPTIONS 选项说明 
* EXAMPLES 示例 
* FILES 相关文件 
* AUTHOR 作者 
* COPYRIGHT 版本信息 
* REPORTING BUGS bug信息 
* SEE ALSO 其它帮助参考

**常用选项**

* 列出所有帮助

```bash
[root@sh-lzy-Centos8 16:46 ~]#man -a ls
--Man-- next: ls(1p) [ view (return) | skip (Ctrl-D) | quit (Ctrl-C) ]
```

* 搜索man手册

```bash
#列出所有匹配的页面，使用 whatis 数据库
[root@sh-lzy-Centos8 16:47 ~]#man -k cd
cd (1)               - bash built-in commands, see bash(1)
cd (1p)              - change the working directory
cd-convert (1)       - Color Manager Testing Tool
cd-create-profile (1) - Color Manager Profile Creation Tool
cd-drive (1)         - show CD-ROM drive characteristics
cd-fix-profile (1)   - Color Manager Testing Tool
cd-info (1)          - shows Information about a CD or CD-image
cd-it8 (1)           - Color Manager Testing Tool
cd-paranoia (1)      - 9.8 (Paranoia release III via libcdio) - an audio CD reading utility which includes ext...
cd-read (1)          - reads Information from a CD or CD-image
mcd (1)              - change MSDOS directory
nscd (8)             - name service cache daemon
nscd.conf (5)        - name service cache daemon configuration file
perlebcdic (1)       - Considerations for running Perl on EBCDIC platforms
pitchplay (1)        - wrapper script to play audio tracks with cdda2wav with different pitches through a soun...
readmult (1)         - a multitrack wrapper for cdda2wav
rpcdebug (8)         - set and clear NFS and RPC kernel debug flags
scdaemon (1)         - Smartcard daemon for the GnuPG system
tcdrain (3)          - get and set terminal attributes, line control, get and set baud rate
tcdrain (3p)         - wait for transmission of output
Unicode::UCD (3pm)   - Unicode character database
utf8 (3pm)           - Perl pragma to enable/disable UTF-8 (or UTF-EBCDIC) in source code
```

* 相当于 whatis

```bash
[root@sh-lzy-Centos8 16:47 ~]#man -f cd
cd (1)               - bash built-in commands, see bash(1)
cd (1p)              - change the working directory
```

* 查看passwd相关命令和文件,man帮助文件路径

```bash
[root@sh-lzy-Centos8 16:48 ~]#whereis man
man: /usr/bin/man /usr/share/man /usr/share/man/man7/man.7.gz /usr/share/man/man1/man.1.gz /usr/share/man/man1p/man.1p.gz
[root@sh-lzy-Centos8 16:50 ~]#whereis passwd
passwd: /usr/bin/passwd /etc/passwd /usr/share/man/man1/passwd.1.gz /usr/share/man/man5/passwd.5.gz
```

#### 2.2.5 info

man常用于命令参考 ，GNU工具 info 适合通用文档参考 

没有参数,列出所有的页面 

info 页面的结构就像一个网站 

每一页分为“节点” 链接节点之前 * 

**info 命令格式**

```
info [ 命令 ]
```

#### 2.2.6 命令自身提供的官方使用指南

```bash
[root@sh-lzy-Centos8 16:53 doc]#pwd
/usr/share/doc
[root@sh-lzy-Centos8 16:53 doc]#ls
abattis-cantarell-fonts        libasyncns                      osinfo-db-tools
accountsservice                libatasmart                     os-prober
adcli                          libavc1394                      ostree
adobe-mappings-cmap            libbasicobjects                 p11-kit
adobe-mappings-pdf             libblkid                        PackageKit
alsa-lib                       libbytesize                     pakchois
[root@sh-lzy-Centos8 16:54 doc]#ll -a |grep passwd*
drwxr-xr-x.   2 root root    50 Dec 19 12:03 passwd
[root@sh-lzy-Centos8 16:54 doc]#
```

#### 2.2.7 系统及第三方应用官方文档



# 3 文件管理和IO重定向

### 内容概述

* 文件系统目录结构 

* 创建和查看文件 

* 复制、转移和删除文件 

* 软和硬链接 

* IO 重定向和管道

### 3.1 文件系统目录结构

#### 3.1.1 文件系统的目录结构

* 文件和目录被组织成一个单根倒置树结构 
* 文件系统从根目录下开始，用“/”表示 
* 根文件系统(rootfs)：root filesystem 
* 标准Linux文件系统（如：ext4），文件名称大小写敏感，例如：MAIL, Mail, mail, mAiL 
* 以 . 开头的文件为隐藏文件 
* 路径分隔的 / 文件名最长255个字节 
* 包括路径在内文件名称最长4095个字节 
* 蓝色-->目录 绿色-->可执行文件 红色-->压缩文件 浅蓝色-->链接文件 灰色-->其他文件 
* 除了斜杠和NUL,所有字符都有效.但使用特殊字符的目录名和文件不推荐使用，有些字符需要用引 号来引用 
* 每个文件都有两类相关数据：元数据：metadata，即属性， 数据：data，即文件内容

Linux的文件系统分层结构：FHS Filesystem Hierarchy Standard 

参考文档：http://www.pathname.com/fhs/

#### 3.1.2 常见的文件系统目录功能

```
/boot：引导文件存放目录，内核文件(vmlinuz)、引导加载器(bootloader, grub)都存放于此目录
/bin：所有用户使用的基本命令；不能关联至独立分区，OS启动即会用到的程序
/sbin：管理类的基本命令；不能关联至独立分区，OS启动即会用到的程序
/lib：启动时程序依赖的基本共享库文件以及内核模块文件(/lib/modules)
/lib64：专用于x86_64系统上的辅助共享库文件存放位置
/etc：配置文件目录
/home/USERNAME：普通用户家目录
/root：管理员的家目录
/media：便携式移动设备挂载点
/mnt：临时文件系统挂载点
/dev：设备文件及特殊文件存储位置
 b: block device，随机访问
 c: character device，线性访问
/opt：第三方应用程序的安装位置
/srv：系统上运行的服务用到的数据
/tmp：临时文件存储位置
/usr: universal shared, read-only data
 bin: 保证系统拥有完整功能而提供的应用程序
 sbin:
 lib：32位使用
 lib64：只存在64位系统
 include: C程序的头文件(header files)
 share：结构化独立的数据，例如doc, man等
       local：第三方应用程序的安装位置
   bin, sbin, lib, lib64, etc, share
/var: variable data files
 cache: 应用程序缓存数据目录
 lib: 应用程序状态信息数据
 local：专用于为/usr/local下的应用程序存储可变数据
 lock: 锁文件
 log: 日志目录及文件
 opt: 专用于为/opt下的应用程序存储可变数据
 run: 运行中的进程相关数据,通常用于存储进程pid文件
 spool: 应用程序数据池
 tmp: 保存系统两次重启之间产生的临时数据
/proc: 用于输出内核与进程信息相关的虚拟文件系统
/sys：用于输出当前系统上硬件设备相关信息虚拟文件系统
/selinux: security enhanced Linux，selinux相关的安全策略等信息的存储位置
```

#### 3.1.3 应用程序的组成部分

```bash
二进制程序：/bin, /sbin, /usr/bin, /usr/sbin, /usr/local/bin, /usr/local/sbin
库文件：/lib, /lib64, /usr/lib, /usr/lib64, /usr/local/lib, /usr/local/lib64
配置文件：/etc, /etc/DIRECTORY, /usr/local/etc
帮助文件：/usr/share/man, /usr/share/doc, /usr/local/share/man, 
/usr/local/share/doc
```

#### 3.1.4 CentOS 7 以后版本目录结构变化

* /bin 和 /usr/bin  
* /sbin 和 /usr/sbin  
* /lib 和/usr/lib  
* /lib64 和 /usr/lib64 

#### 3.1.5 Linux下的文件类型

* -普通文件 
* d 目录文件directory 
* l 符号链接文件link 
* b 块设备block  
* c 字符设备character 
* p 管道文件pipe 
* s 套接字文件socket

```bash
[root@sh-lzy-Centos8 16:54 doc]#ll -l /run/
total 44
-rw-------.  1 root           root             11 Jan  1 12:49 alsactl.pid
-rw-r--r--.  1 root           root              5 Jan  1 12:49 atd.pid
-rw-r--r--.  1 root           root              4 Jan  1 12:49 auditd.pid
drwxr-xr-x.  2 avahi          avahi            80 Jan  1 12:49 avahi-daemon
drwxr-xr-x.  2 root           root             60 Jan  1 12:49 chrony-helper
drwxr-xr-x.  2 root           root            100 Jan  1 12:49 cockpit
drwxr-xr-x.  2 root           root             80 Jan  1 12:51 console
drwxr-xr-x.  2 root           root             40 Jan  1 12:49 criu
----------.  1 root           root              0 Jan  1 12:49 cron.reboot
drwx------.  2 root           root             40 Jan  1 12:49 cryptsetup
drwxr-xr-x.  3 root           lp               80 Jan  1 12:49 cups
drwxr-xr-x.  2 root           root             60 Jan  1 12:49 dbus
prw-------.  1 root           root              0 Jan  1 12:49 dmeventd-client
prw-------.  1 root           root              0 Jan  1 12:49 dmeventd-server
drwxr-xr-x.  2 root           root             40 Jan  1 12:49 faillock
drwxr-x---.  2 root           root             40 Jan  1 12:49 firewalld
drwxr-xr-x.  2 root           root             60 Jan  1 12:49 fsck
drwx--x--x.  3 root           gdm              80 Jan  1 12:49 gdm
......
```

### 3.2 文件操作命令

#### 3.2.1 显示当前工作目录

每个shell和系统集成都有一个当前的工作目录CWD：current work directory

显示当前shell CWD的绝对路径

**pwd命令: printing working directory**

* -P 显示真实物理路径 
* -L 显示链接路径（默认）

```bash
[root@sh-lzy-Centos8 17:10 doc]#pwd
/usr/share/doc
[root@sh-lzy-Centos8 17:15 doc]#pwd -P
/usr/share/doc
[root@sh-lzy-Centos8 17:15 doc]#
```

#### 3.2.2 绝对和相对路径

* 绝对路径 

  以正斜杠/ 即根目录开始 

  完整的文件的位置路径 

  可用于任何想指定一个文件名的时候 

* 相对路径名 

  不以斜线开始 

  一般情况下，是指相对于当前工作目录的路径，特殊场景下，是相对于某目录的位置 

  可以作为一个简短的形式指定一个文件名

基名：basename，只取文件名而不要路径 

目录名：dirname，只取路径，不要文件名

```bash
[root@sh-lzy-Centos8 17:15 doc]#basename /etc/sysconfig/network-scripts/ifcfg-ens160 
ifcfg-ens160
[root@sh-lzy-Centos8 17:21 doc]#dirname /etc/sysconfig/network-scripts/ifcfg-ens160 
/etc/sysconfig/network-scripts
```

#### 3.2.3 更改目录

**命令 cd ： change directory 改变目录**

选项：-P 切换至物理路径，而非软链接目录

可以使用绝对或相对路径 

* 切换至父目录： cd .. 
* 切换至当前用户主目录： cd 
* 切换至以前的工作目录： cd-

```bash
[root@sh-lzy-Centos8 17:22 doc]#cd -
/root
[root@sh-lzy-Centos8 17:27 ~]#
```

相关的环境变量： 

* PWD：当前目录路径 
* OLDPWD：上一次目录路径

```bash
[root@sh-lzy-Centos8 17:27 ~]#echo $PWD
/root
[root@sh-lzy-Centos8 17:28 ~]#echo $OLDPWD
/usr/share/doc
[root@sh-lzy-Centos8 17:28 ~]#
```

#### 3.2.4 列出目录内容

ls 命令可以列出当前目录的内容或指定目录

```bash
ls [options] [files_or_dirs]
```

常见选项： 

* -a 包含隐藏文件 
* -l 显示额外的信息 
* -R 目录递归 
* -ld 目录和符号链接信息 
* -1 文件分行显示 
* -S 按从大到小排序 
* -t 按mtime排序 
* -u 配合-t选项，显示并按atime从新到旧排序 
* -U 按目录存放顺序显示 
* -X 按文件后缀排序 
* -F 对不同类型文件显示时附加不同的符号：*/=>@| 
* -C 文件多时，以多列的方式显示文件，默认是一列（标准输出）

**注意：**

ls 查看不同后缀文件时的颜色由 /etc/DIR_COLORS 和@LS_COLORS变量定义 

ls -l 看到文件的大小,不一定是实际文件真正占用空间的大小

#### 3.2.5 查看文件状态 stat

文件相关信息：metadata, data 

每个文件有三个时间戳： 

* access time 访问时间，atime，读取文件内容 
* modify time 修改时间，mtime，改变文件内容（数据） 
* change time 改变时间，ctime，元数据发生改变

```
[root@sh-lzy-Centos8 17:38 etc]#stat /etc/profile.d/env.sh 
  File: /etc/profile.d/env.sh
  Size: 22        	Blocks: 8          IO Block: 4096   regular file
Device: 801h/2049d	Inode: 403279337   Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Context: unconfined_u:object_r:bin_t:s0
Access: 2022-01-01 12:52:45.213990909 +0800
Modify: 2022-01-01 12:52:20.446993149 +0800
Change: 2022-01-01 12:52:20.446993149 +0800
 Birth: 2022-01-01 12:52:20.446993149 +0800
[root@sh-lzy-Centos8 17:39 etc]#
```

#### 3.2.6 确定文件内容

文件可以包含多种类型的数据，使用file命令检查文件的类型，然后确定适当的打开命令或应用程序使用

```
file [options] <filename>...
```

常用选项: 

* -b 列出文件辨识结果时，不显示文件名称 
* -f filelist 列出文件filelist中文件名的文件类型 
* -F 使用指定分隔符号替换输出文件名后默认的”:”分隔符 
* -L 查看对应软链接对应文件的文件类型 
* --help 显示命令在线帮助

Windows的文本格式与Linux的文本格式的区别

```bash
[root@sh-lzy-Centos8 18:48 data]#ll
total 8
-rw-r--r--. 1 root root 7 Jan  1 18:48 linux.txt
-rw-r--r--. 1 root root 9 Jan  1 18:47 win.txt
[root@sh-lzy-Centos8 18:48 data]#file win.txt linux.txt 
win.txt:   ASCII text, with CRLF line terminators
linux.txt: ASCII text
[root@sh-lzy-Centos8 18:48 data]#

#查看两者文件中文本对应的ASCII码
[root@sh-lzy-Centos8 18:50 data]#hexdump -C win.txt 
00000000  61 0d 0a 62 0d 0a 63 0d  0a                       |a..b..c..|
00000009
[root@sh-lzy-Centos8 18:50 data]#hexdump -C linux.txt 
00000000  61 0a 62 0a 63 0a 0a                              |a.b.c..|
00000007

#将win格式的转换为Linux格式
[root@sh-lzy-Centos8 18:50 data]#dnf -y install dos2unix
Last metadata expiration check: 3:53:50 ago on Sat 01 Jan 2022 02:57:46 PM CST.
Package dos2unix-7.4.0-3.el8.x86_64 is already installed.
Dependencies resolved.
Nothing to do.
Complete!
[root@sh-lzy-Centos8 18:51 data]#dos2unix win.txt 
dos2unix: converting file win.txt to Unix format...
[root@sh-lzy-Centos8 18:51 data]#file win.txt linux.txt 
win.txt:   ASCII text
linux.txt: ASCII text
[root@sh-lzy-Centos8 18:52 data]#
```

#### 3.2.7 文件通配符模式 wildcard pattern

文件通配符可以用来匹配符合条件的多个文件，方便批量管理文件 

通配符采有特定的符号，表示特定的含义，此特符号称为元 meta 字符 

* 常见的通配符如下：

```
* 匹配零个或多个字符，但不匹配 "." 开头的文件，即隐藏文件
? 匹配任何单个字符,一个汉字也算一个字符
~ 当前用户家目录
~mage 用户mage家目录
. 和 ~+ 当前工作目录
~-   前一个工作目录
[0-9] 匹配数字范围
[a-z] 一个字母
[A-Z] 一个字母
[wang] 匹配列表中的任何的一个字符
[^wang] 匹配列表中的所有字符以外的字符
[^a-z] 匹配列表中的所有字符以外的字符
```

别外还有在Linux系统中预定义的字符类：man 7 glob

```
[:digit:]：任意数字，相当于0-9
[:lower:]：任意小写字母,表示 a-z
[:upper:]: 任意大写字母,表示 A-Z 
[:alpha:]: 任意大小写字母
[:alnum:]：任意数字或字母
[:blank:]：水平空白字符
[:space:]：水平或垂直空白字符
[:punct:]：标点符号
[:print:]：可打印字符
[:cntrl:]：控制（非打印）字符
[:graph:]：图形字符
[:xdigit:]：十六进制字符
```

#### 3.2.8 创建空文件和刷新时间

**touch命令可以用来创建空文件或刷新文件的时间**

```
touch [OPTION]... FILE...
```

选项说明：  

* -a 仅改变 atime和ctime 
* -m 仅改变 mtime和ctime 
* -t [[CC]YY]MMDDhhmm[.ss] 指定atime和mtime的时间戳 
* -c 如果文件不存在，则不予创建

```bash
[root@sh-lzy-Centos8 20:17 data]#ll
total 8
-rw-r--r--. 1 root root 0 Jan  1 20:00 f1.txt
-rw-r--r--. 1 root root 7 Jan  1 18:48 linux.txt
-rw-r--r--. 1 root root 6 Jan  1 18:51 win.txt
[root@sh-lzy-Centos8 20:18 data]#date 
Sat Jan  1 20:19:04 CST 2022
[root@sh-lzy-Centos8 20:19 data]#touch `date -d "-1 day" +%F-%T`.log
[root@sh-lzy-Centos8 20:19 data]#ll
total 8
-rw-r--r--. 1 root root 0 Jan  1 20:19 2021-12-31-20:19:53.log
-rw-r--r--. 1 root root 0 Jan  1 20:00 f1.txt
-rw-r--r--. 1 root root 7 Jan  1 18:48 linux.txt
-rw-r--r--. 1 root root 6 Jan  1 18:51 win.txt
[root@sh-lzy-Centos8 20:19 data]#sta
start-pulseaudio-x11  start-statd           startx                stat                  states
[root@sh-lzy-Centos8 20:19 data]#stat 2021-12-31-20\:19\:53.log 
  File: 2021-12-31-20:19:53.log
  Size: 0         	Blocks: 0          IO Block: 4096   regular empty file
Device: 802h/2050d	Inode: 135         Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Context: unconfined_u:object_r:default_t:s0
Access: 2022-01-01 20:19:53.011564519 +0800
Modify: 2022-01-01 20:19:53.011564519 +0800
Change: 2022-01-01 20:19:53.011564519 +0800
 Birth: 2022-01-01 20:19:53.011564519 +0800
[root@sh-lzy-Centos8 20:20 data]#
```

#### 3.2.9 复制文件和目录

**利用 cp（copy）命令可以实现文件或目录的复制**

```
cp [OPTION]... [-T] SOURCE DEST
cp [OPTION]... SOURCE... DIRECTORY
cp [OPTION]... -t DIRECTORY SOURCE..
```

常用选项 

* -i 如果目标已存在，覆盖前提示是否覆盖  

* -n 不覆盖，注意两者顺序 

* -r, -R 递归复制目录及内部的所有内容 

* -a 归档，相当于-dR --preserv=all，常用于备份功能 

* -d --no-dereference --preserv=links 不复制原文件，只复制链接名 

* --preserv[=ATTR_LIST] 

  mode: 权限 

  ownership: 属主属组 

  timestamp:  

  links 

  xattr 

  context 

  all

* -p 等同--preserv=mode,ownership,timestamp 

* -v --verbose 

* -f --force  

* -u --update 只复制源比目标更新文件或目标不存在的文件 

* -b 目标存在，覆盖前先备份，默认形式为 filename~ ,只保留最近的一个备份

* --backup=numbered 目标存在，覆盖前先备份加数字后缀，形式为 filename.~#~ ，可以保留多 个版本

范例：

```bash
[root@sh-lzy-Centos8 21:17 /]#cp -r /etc/sysconfig/ /data/sysconfig-bak
[root@sh-lzy-Centos8 21:18 /]#ll /data/
total 12
-rw-r--r--. 1 root root    0 Jan  1 20:19 2021-12-31-20:19:53.log
-rw-r--r--. 1 root root    0 Jan  1 20:00 f1.txt
-rw-r--r--. 1 root root    7 Jan  1 18:48 linux.txt
drwxr-xr-x. 5 root root 4096 Jan  1 21:18 sysconfig-bak
-rw-r--r--. 1 root root    6 Jan  1 18:51 win.txt
[root@sh-lzy-Centos8 21:18 /]#
```

* 将/etc/目录下所有文件，备份到/data独立的子目录下，并要求子目录格式为 backupYYYY-mm-dd，备份过程可见

```bash
[root@sh-lzy-Centos8 21:28 data]#cp -iav /etc/ /data/bak`date +%F`

[root@sh-lzy-Centos8 21:29 data]#cp -iav /etc/ /data/etc-bak`date +%F-%H-%M-%S`
[root@sh-lzy-Centos8 21:29 data]#ll
total 40
-rw-r--r--.   1 root root    0 Jan  1 20:19 2021-12-31-20:19:53.log
drwxr-xr-x. 143 root root 8192 Jan  1 21:28 bak2022-01-01
drwxr-xr-x. 142 root root 8192 Jan  1 18:46 etc-bak2022-01-01-21-29-39
-rw-r--r--.   1 root root    0 Jan  1 20:00 f1.txt
-rw-r--r--.   1 root root  709 Jan  1 21:21 fstab
-rw-r--r--.   1 root root    7 Jan  1 18:48 linux.txt
drwxr-xr-x.   5 root root 4096 Jan  1 21:18 sysconfig-bak
drwxr-xr-x.   2 root root   32 Jan  1 21:22 test
-rw-r--r--.   1 root root    6 Jan  1 18:51 win.txt
```

* 创建/data/rootdir目录，并复制/root下所有文件到该目录内，要求保留原有权限

```bash
[root@sh-lzy-Centos8 21:32 data]#cp -a /root/ /data/rootdir
[root@sh-lzy-Centos8 21:32 data]#ll /data/rootdir/
total 8
-rw-------. 1 root root 1491 Dec 19 12:06 anaconda-ks.cfg
-rw-r--r--. 1 root root 1783 Dec 19 13:24 initial-setup-ks.cfg
[root@sh-lzy-Centos8 21:32 data]#
```

#### 3.2.10 移动和重命名文件

**mv 命令可以实现文件或目录的移动和改名**

同一分区移动数据,速度很快:数据位置没有变化 

不同分区移动数据,速度相对慢:数据位置发生了变化

```
mv [OPTION]... [-T] SOURCE DEST
mv [OPTION]... SOURCE... DIRECTORY
mv [OPTION]... -t DIRECTORY SOURCE...
```

常用选项： 

* -i 交互式 
* -f 强制 
* -b 目标存在，覆盖前先备份

**利用 rename 可以批量修改文件名**

```
rename [options] <expression> <replacement> <file>...
```

#### 3.2.11 删除文件

**使用 rm 命令可以删除文件**

```
rm [OPTION]... FILE...
```

常用选项：

* -i 交互式 
* -f 强制删除 
* -r 递归 
* --no-preserve-root 删除/ 

rm 虽然删除了文件，但是被删除的文件仍然可能被恢复，在安全要求较高的场景下，可以使用shred安 全删除文件

```
shred [OPTION]... FILE...
```

```bash
[root@sh-lzy-Centos8 21:43 data]#ll
total 40
-rw-r--r--.   1 root root    0 Jan  1 20:19 2021-12-31-20:19:53.log
drwxr-xr-x. 143 root root 8192 Jan  1 21:28 bak2022-01-01
drwxr-xr-x. 142 root root 8192 Jan  1 18:46 etc-bak2022-01-01-21-29-39
-rw-r--r--.   1 root root    0 Jan  1 20:00 f1.txt
-rw-r--r--.   1 root root 4096 Jan  1 21:43 fstab
-rw-r--r--.   1 root root    7 Jan  1 18:48 linux.txt
dr-xr-x---.   5 root root  240 Jan  1 16:01 rootdir
drwxr-xr-x.   5 root root 4096 Jan  1 21:18 sysconfig-bak
drwxr-xr-x.   2 root root   32 Jan  1 21:22 test
-rw-r--r--.   1 root root    6 Jan  1 18:51 win.txt
[root@sh-lzy-Centos8 21:44 data]#shred -zuv fstab 
shred: fstab: pass 1/4 (random)...
shred: fstab: pass 2/4 (random)...
shred: fstab: pass 3/4 (random)...
shred: fstab: pass 4/4 (000000)...
shred: fstab: removing
shred: fstab: renamed to 00000
shred: 00000: renamed to 0000
shred: 0000: renamed to 000
shred: 000: renamed to 00
shred: 00: renamed to 0
shred: fstab: removed
[root@sh-lzy-Centos8 21:44 data]#ll
total 36
-rw-r--r--.   1 root root    0 Jan  1 20:19 2021-12-31-20:19:53.log
drwxr-xr-x. 143 root root 8192 Jan  1 21:28 bak2022-01-01
drwxr-xr-x. 142 root root 8192 Jan  1 18:46 etc-bak2022-01-01-21-29-39
-rw-r--r--.   1 root root    0 Jan  1 20:00 f1.txt
-rw-r--r--.   1 root root    7 Jan  1 18:48 linux.txt
dr-xr-x---.   5 root root  240 Jan  1 16:01 rootdir
drwxr-xr-x.   5 root root 4096 Jan  1 21:18 sysconfig-bak
drwxr-xr-x.   2 root root   32 Jan  1 21:22 test
-rw-r--r--.   1 root root    6 Jan  1 18:51 win.txt
```

#### 3.2.12 目录操作

##### 3.2.12.1 显示目录树 tree

常见选项： 

* -d: 只显示目录 
* -L level：指定显示的层级数目 
* -P pattern: 只显示由指定wild-card pattern匹配到的路径

##### 3.2.12.2 创建目录 mkdir

常见选项： 

* -p: 存在于不报错，且可自动创建所需的各目录 
* -v: 显示详细信息 
* -m MODE: 创建目录时直接指定权限

##### 3.2.11.3 删除空目录 rmdir

常见选项： 

* -p 递归删除父空目录 
* -v 显示详细信息

**注意：**rmdir只能删除空目录，如果想删除非空目录，可以使用rm -r 命令，递归删除目录树

```
alias rm='DIR=/data/backup`date +%F%T`
mkdir $DIR;mv -t $DIR'
```

### 3.3 文件元数据和节点表结构

#### 3.3.1 inode 表结构

每个文件的属性信息，比如：文件的大小，时间，类型等，称为文件的元数据(meta data)。

这此元数 据是存放在inode（index node）表中。

inode 表中有很多条记录组成，第一条记录对应的存放了一个 文件的元数据信息。

每一个inode表记录对应的保存了以下信息：

* inode number 节点号 
* 文件类型 
* 权限 
* UID 
* GID 
* 链接数（指向这个文件名路径名称个数） 
* 该文件的大小和不同的时间戳 
* 指向磁盘上文件的数据块指针 
* 有关文件的其他数据

**目录** 

目录是个特殊文件，目录文件的内容保存了此目录中文件的列表及inode number对应关系 

* 文件引用一个是 inode号 
* 人是通过文件名来引用一个文件 
* 一个目录是目录下的文件名和文件inode号之间的映射

**cp和inode**

cp 命令： 

* 分配一个空闲的inode号，在inode表中生成新条目 
* 在目录中创建一个目录项，将名称与inode编号关联 
* 拷贝数据生成新的文件

**rm和inode**