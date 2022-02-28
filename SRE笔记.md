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

```bash
[root@sh-lzy-Centos8 12:50 ~]#echo I love hxy
I love hxy

[root@sh-lzy-Centos8 12:59 ~]#sleep 5;echo -e "\a";echo sleep complete!

sleep complete!											#配合命令输出提示
```

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

```bash
[root@sh-lzy-Centos8 13:05 ~]#echo -e 'aaaaaa\r'
aaaaaa
[root@sh-lzy-Centos8 13:09 ~]#echo -e 'aaaaaa\rbbbb'
bbbbaa
[root@sh-lzy-Centos8 13:09 ~]#echo -e 'aaaaaa\n bbbb'
aaaaaa
 bbbb										#换行 与 回车是两个完全不同的步骤
```

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



# 3. 文件管理和IO重定向

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
/bin：所有用户使用的基本命令；不能关联至独立分区，OS启动即会用到的程序			#均为二进制程序
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
/proc: 用于输出内核与进程信息相关的虚拟文件系统					#记录内存中的信息
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

**pwd命令: printing working directory**								#pwd为shell的内建命令

* -P 显示真实物理路径 
* -L 显示链接路径（默认）

```bash
[root@sh-lzy-Centos8 17:10 doc]#pwd
/usr/share/doc
[root@sh-lzy-Centos8 17:15 doc]#pwd -P			#改参数在对于链接文件时显示真是路径
/usr/share/doc
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
* 切换至上一个工作目录： cd-

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

```bash
[root@sh-lzy-Centos8 23:17 /]#which basename 			#取路径中文件/文件夹本身
/usr/bin/basename
[root@sh-lzy-Centos8 23:18 /]#which dirname 			#取路径中文件/文件夹的上级路径
/usr/bin/dirname
[root@sh-lzy-Centos8 23:18 /]#basename /etc/sysconfig/network-scripts/
network-scripts
[root@sh-lzy-Centos8 23:18 /]#dirname /etc/sysconfig/network-scripts/
/etc/sysconfig
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
* -r 倒序显示

```bash
[root@sh-lzy-Centos8 23:30 /]#ls -R data/test/
data/test/:
fstab  vimrc
```

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

```bash
# * 匹配零个或多个字符，但不匹配 "." 开头的文件，即隐藏文件
# ? 匹配任何单个字符,一个汉字也算一个字符
# ~ 当前用户家目录
# ~mage 用户mage家目录
# . 和 ~+ 当前工作目录
# ~-   前一个工作目录
# [0-9] 匹配数字范围
# [a-z] 一个字母
[root@sh-lzy-Centos8 20:04 data]#touch file_{a..z}
[root@sh-lzy-Centos8 20:05 data]#touch file_{A..Z}
[root@sh-lzy-Centos8 20:05 data]#ll
total 0
-rw-r--r--. 1 root root 0 Feb  3 20:05 file_a
-rw-r--r--. 1 root root 0 Feb  3 20:05 file_A
-rw-r--r--. 1 root root 0 Feb  3 20:05 file_b
-rw-r--r--. 1 root root 0 Feb  3 20:05 file_B
-rw-r--r--. 1 root root 0 Feb  3 20:05 file_c
....
[root@sh-lzy-Centos8 20:06 data]#ls file_[a-c]		#该通配符的字母匹配是按照ASCII的顺序区间匹配
file_a  file_A  file_b  file_B  file_c

# [A-Z] 一个字母
# [wang] 匹配列表中的任何的一个字符
# [^wang] 匹配列表中的所有字符以外的字符
[root@sh-lzy-Centos8 20:16 data]#ls
file_0   file_13  file_4  file_9  file_c  file_E  file_h  file_J  file_m  file_O  file_r  file_T  file_w  file_Y
file_1   file_14  file_5  file_a  file_C  file_f  file_H  file_k  file_M  file_p  file_R  file_u  file_W  file_z
file_10  file_15  file_6  file_A  file_d  file_F  file_i  file_K  file_n  file_P  file_s  file_U  file_x  file_Z
file_11  file_2   file_7  file_b  file_D  file_g  file_I  file_l  file_N  file_q  file_S  file_v  file_X
file_12  file_3   file_8  file_B  file_e  file_G  file_j  file_L  file_o  file_Q  file_t  file_V  file_y
[root@sh-lzy-Centos8 20:16 data]#ls file_[Lzy]
file_L  file_y  file_z
[root@sh-lzy-Centos8 20:16 data]#ls file_[^Lzy]
file_0  file_4  file_8  file_b  file_d  file_f  file_h  file_j  file_l  file_N  file_P  file_R  file_T  file_V  file_X
file_1  file_5  file_9  file_B  file_D  file_F  file_H  file_J  file_m  file_o  file_q  file_s  file_u  file_w  file_Y
file_2  file_6  file_a  file_c  file_e  file_g  file_i  file_k  file_M  file_O  file_Q  file_S  file_U  file_W  file_Z
file_3  file_7  file_A  file_C  file_E  file_G  file_I  file_K  file_n  file_p  file_r  file_t  file_v  file_x

# [^a-z] 匹配列表中的所有字符以外的字符
```

别外还有在Linux系统中预定义的字符类：man 7 glob

```bash
# [:digit:]：任意数字，相当于0-9
[root@sh-lzy-Centos8 20:19 data]#ls file_[[:digit:]]		#外围添加[]表示预定义的字符类
file_0  file_1  file_2  file_3  file_4  file_5  file_6  file_7  file_8  file_9
[root@sh-lzy-Centos8 20:20 data]#ls file_[:digit:]
file_d  file_g  file_i  file_t

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

rm 命令： 

* 链接数递减，从而释放的inode号可以被重用 
* 把数据块放在空闲列表中 
* 删除目录项 
* 数据实际上不会马上被删除，但当另一个文件使用数据块时将被覆盖

**mv和inode**

* 如果mv命令的目标和源在相同的文件系统，作为mv 命令 

  用新的文件名创建对应新的目录项 

  删除旧目录条目对应的旧的文件名 

  不影响inode表（除时间戳）或磁盘上的数据位置：没有数据被移动！ 

* 如果目标和源在一个不同的文件系统， mv相当于cp和rm

#### 3.3.2 硬（hard）链接

硬链接本质上就给一个文件起一个新的名称，实质是同一个文件 

硬链接特性 

* 创建硬链接会在对应的目录中增加额外的记录项以引用文件 
* 对应于同一文件系统上一个物理文件 
* 每个目录引用相同的inode号 
* 创建时链接数递增 删除文件时：rm命令递减计数的链接，文件要存在，至少有一个链接数，当链接数为零时，该文 件被删除 
* 不能跨越驱动器或分区 
* 不支持对目录创建硬链接

```
ln filename [linkname ]
```

#### 3.3.3 符号 symbolic （或软 soft）链接

一个符号链接指向另一个文件,就像 windows 中快捷方式，软链接文件和原文件本质上不是同一个文件 

软链接特点 

* 一个符号链接的内容是它引用文件的名称 
* 可以对目录创建软链接 
* 可以跨分区的文件实现 
* 指向的是另一个文件的路径；其大小为指向的路径字符串的长度；不增加或减少目标文件inode的 引用计数 
* 在创建软链接时, 如果源文件使用相对路径，是相对于软链接文件的路径，而非相对于当前工作目 录,但是软链接的路径如果是相对路径,则是相对于当前工作目录

```
ln -s filename [linkname]
```

#### 3.3.4 硬链接和软链接区别总结

1.本质： 

硬链接：本质是同一个文件 

软链接：本质不是同一个文件 

2.跨设备 

硬链接：不支持 

软链接：支持 

3.inode 

硬链接：相同 

软链接：不同 

4.链接数 

硬链接：创建新的硬链接,链接数会增加,删除硬链接,链接数减少 

软链接：创建或删除,链接数不会变化 

5.文件夹

硬链接：不支持 

软链接：支持 

6.相对路径 

硬链接：原始文件相对路径是相对于当前工作目录 

软链接：原始文件的相对路径是相对于链接文件的相对路径 

7.删除源文件 

硬链接：只是链接数减一,但链接文件的访问不受影响 

软链接：链接文件将无法访问 

8.文件类型 

硬链接：和源文件相同 

软链接：链接文件,和源文件无关 

9.文件大小 

硬链接: 和源文件相同 

软链接: 源文件的路径的长度

### 3.4 IO 重定向和管道

#### 3.4.1 标准输入和输出

程序：指令+数据 

读入数据：Input 

输出数据：Output 

打开的文件都有一个fd: file descriptor (文件描述符) 

Linux给程序提供三种 I/O 设备 

* 标准输入（STDIN） －0 默认接受来自终端窗口的输入 
* 标准输出（STDOUT）－1 默认输出到终端窗口 
* 标准错误（STDERR） －2 默认输出到终端窗口

#### 3.4.2 I/O重定向 redirect

I/O重定向：将默认的输入，输出或错误对应的设备改变，指向新的目标

##### 3.4.2.1 标准输出和错误重新定向

STDOUT和STDERR可以被重定向到指定文件,而非默认的当前终端

```
命令 操作符号 文件名
```

**支持的操作符号包括：**

```
1> 或 >     把STDOUT重定向到文件
2> 把STDERR重定向到文件
&> 把标准输出和错误都重定向
>& 和上面功能一样，建议使用上面方式
```

**以上如果文件已存在，文件内容会被覆盖**

```
set  -C 禁止将内容覆盖已有文件,但可追加， 利用 >| 仍可强制覆盖
set  +C 允许覆盖，默认
```

**追加**

\>> 可以在原有内容基础上，追加内容 
把输出和错误重新定向追加到文件

```
>> 追加标准输出重定向至文件
2>> 追加标准错误重定向至文件
```

**标准输出和错误输出各自定向至不同位置**

```
COMMAND > /path/to/file.out 2> /path/to/error.out
```

**合并标准输出和错误输出为同一个数据流进行重定向**

&> 覆盖重定向 

&>> 追加重定向

```
COMMAND > /path/to/file.out 2>&1 （顺序很重要）
COMMAND >> /path/to/file.out 2>&1
```

**合并多个程序**

 (CMD1;CMD2......) 或者{ CMD1;CMD2;....; }合并多个程序的STDOUT

##### 3.4.2.2 标准输入重定向

从文件中导入STDIN，代替当前终端的输入设备，使用 < 来重定向标准输入 

某些命令能够接受从文件中导入的STDIN

###### 3.4.2.2.1 tr 命令

tr 转换和删除字符

```
tr [OPTION]... SET1 [SET2]
```

选项：

```
-d --delete：删除所有属于第一字符集的字符
-s --squeeze-repeats：把连续重复的字符以单独一个字符表示,即去重
-t  --truncate-set1：将第一个字符集对应字符转化为第二字符集对应的字符
-c –C --complement：取字符集的补集
 \NNN           character with octal value NNN (1 to 3 octal digits)
 \\             backslash
 \a             audible BEL
 \b             backspace
 \f             form feed
 \n             new line
 \r             return
 \t             horizontal tab
 \v             vertical tab
[:alnum:]：字母和数字
[:alpha:]：字母
[:digit:]：数字
[:lower:]：小写字母
[:upper:]：大写字母
[:space:]：空白字符
[:print:]：可打印字符
[:punct:]：标点符号
[:graph:]：图形字符
[:cntrl:]：控制（非打印）字符
[:xdigit:]：十六进制字符
```

###### 3.4.2.2.2 标准输入重定向

实现标准输入重定向的符号

```
COMMAND 0< FILE
COMMAND < FILE
```

```bash
[root@sh-lzy-Centos8 00:02 data]#echo 2^3 > bc.log
[root@sh-lzy-Centos8 00:28 data]#ll
total 40
-rw-r--r--.   1 root root    0 Jan  1 20:19 2021-12-31-20:19:53.log
drwxr-xr-x. 143 root root 8192 Jan  1 21:28 bak2022-01-01
-rw-r--r--.   1 root root    4 Jan  3 00:28 bc.log
lrwxrwxrwx.   1 root root    9 Jan  2 22:52 dirlink -> /data/dir
drwxr-xr-x. 142 root root 8192 Jan  1 18:46 etc-bak2022-01-01-21-29-39
-rw-r--r--.   1 root root    0 Jan  1 20:00 f1.txt
-rw-r--r--.   1 root root    7 Jan  1 18:48 linux.txt
dr-xr-x---.   5 root root  240 Jan  1 16:01 rootdir
drwxr-xr-x.   5 root root 4096 Jan  1 21:18 sysconfig-bak
drwxr-xr-x.   2 root root   32 Jan  1 21:22 test
-rw-r--r--.   1 root root    6 Jan  1 18:51 win.txt          
[root@sh-lzy-Centos8 00:28 data]#cat bc.log 
2^3
[root@sh-lzy-Centos8 00:28 data]#bc < bc.log 
8
[root@sh-lzy-Centos8 00:28 data]#
```

###### 3.4.2.2.3 把多行重定向

使用 "<<终止词" 命令从键盘把多行重导向给STDIN，直到终止词位置之前的所有文本都发送给 STDIN，有时被称为就地文本（here documents） 

其中终止词可以是任何一个或多个符号，比如：!，@，$，EOF（End Of File），magedu等，其中EOF 比较常用

#### 3.4.3 管道

##### 3.4.3.1 管道

管道（使用符号“|”表示）用来连接多个命令

```
命令1 | 命令2 | 命令3 | …
```

功能说明： 

* 将命令1的STDOUT发送给命令2的STDIN，命令2的STDOUT发送到命令3的STDIN 
* 所有命令会在当前shell进程的子shell进程中执行 
* 组合多种工具的功能

**注意：**STDERR默认不能通过管道转发，可利用2>&1 或 |& 实现，格式如下

```
命令1 2>&1 | 命令2 
命令1 |& 命令2 
```

##### 3.4.3.2 tee 命令

利用 tee 命令可以重定向到多个目标，经常配合管道符一起使用

```
命令1 | tee [-a ] 文件名 | 命令2 
```

以上可以把命令1的STDOUT保存在文件中，做为命令2的输入

选项：

* -a 追加

功能： 

* 保存不同阶段的输出 
* 复杂管道的故障排除 
* 同时查看和记录输出

#### 3.4.4 重定向中的 - 符号

重定向有时会使用 - 符号

# 4. 用户、组和权限

### 内容概述

* Linux的安全模型 
* 用户和组相关文件 
* 用户和组管理命令 
* 理解并设置文件权限 
* 默认权限 
* 特殊权限 
* 文件访问控制列表

### 4.1 Linux安全模型

资源分派： 

* Authentication：认证，验证用户身份 
* Authorization：授权，不同的用户设置不同权限 
* Accouting|Audition：审计 

当用户登录成功时，系统会自动分配令牌 token，包括：用户标识和组成员等信息

#### 4.1.1 用户

Linux中每个用户是通过 User Id （UID）来唯一标识的

* 管理员：root, 0 

* 普通用户：1-60000 自动分配

  系统用户：1-499 （CentOS 6以前）, 1-999 （CentOS 7以后）  

  ​		对守护进程获取资源进行权限分配 

  登录用户：500+ （CentOS6以前）, 1000+（CentOS7以后）  

  ​		给用户进行交互式登录使用

#### 4.1.2 用户组

Linux中可以将一个或多个用户加入用户组中，用户组是通过Group ID（GID） 来唯一标识的。

* 管理员组：root, 0 

* 普通组： 

  ​		系统组：1-499（CentOS 6以前）, 1-999（CentOS7以后）, 对守护进程获取资源进行权限分 配 

  ​		普通组：500+（CentOS 6以前）, 1000+（CentOS7以后）, 给用户使用

#### 4.1.3 用户和组的关系

* 用户的主要组(primary group)：用户必须属于一个且只有一个主组，默认创建用户时会自动创建 和用户名同名的组，做为用户的主要组，由于此组中只有一个用户，又称为私有组 
* 用户的附加组(supplementary group)： 一个用户可以属于零个或多个辅助组，附属组

```bash
[root@sh-lzy-Centos8 11:40 ~]#id root
uid=0(root) gid=0(root) groups=0(root)
[root@sh-lzy-Centos8 11:40 ~]#id Lzy
uid=1000(Lzy) gid=1000(Lzy) groups=1000(Lzy)
[root@sh-lzy-Centos8 11:40 ~]#
```

#### 4.1.4 安全上下文

Linux安全上下文Context：运行中的程序，即进程 (process)，以进程发起者的身份运行，进程所能够 访问资源的权限取决于进程的运行者的身份 

比如：分别以root 和wang 的身份运行 /bin/cat /etc/shadow ，得到的结果是不同的，资源能否能 被访问，是由运行者的身份决定，非程序本身

```bash
[root@sh-lzy-Centos8 11:40 ~]#cat /etc/shadow
root:$6$WmdYhVIqDUe1/XkK$eK6m9.IzbKVE.3mLZjzU7G6zVc1giZp0thPXj0YNVB6NbEboqEwEwoZ3Y./Xzbb0hW1xj7emypOXDj7rCKzmE0::0:99999:7:::
bin:*:18397:0:99999:7:::
daemon:*:18397:0:99999:7:::
adm:*:18397:0:99999:7:::
lp:*:18397:0:99999:7:::
sync:*:18397:0:99999:7:::
shutdown:*:18397:0:99999:7:::
halt:*:18397:0:99999:7:::
mail:*:18397:0:99999:7:::
......
setroubleshoot:!!:18980::::::
flatpak:!!:18980::::::
gdm:!!:18980::::::
clevis:!!:18980::::::
gnome-initial-setup:!!:18980::::::
tcpdump:!!:18980::::::
sshd:!!:18980::::::
Lzy:$6$UYpnH0OfzqxRxdfQ$GcrBbp6ZgFoFDVfKO/gQLkERLEevaRJLnB6v2sbJ.O/1.G5kSNv29BjxqRpgIVUe7ibIM5SZ.H29HHAS7nv5q0::0:99999:7:::
[root@sh-lzy-Centos8 11:42 ~]#su Lzy
[Lzy@sh-lzy-Centos8 11:45 ~]$cat /etc/shadow
cat: /etc/shadow: Permission denied
[Lzy@sh-lzy-Centos8 11:46 ~]$
```

### 4.2 用户和组的配置文件

#### 4.2.1 用户和组的主要配置文件

* /etc/passwd：用户及其属性信息(名称、UID、主组ID等） 
* /etc/shadow：用户密码及其相关属性 
* /etc/group：组及其属性信息 
* /etc/gshadow：组密码及其相关属性

#### 4.2.2 passwd文件格式

* login name：登录用名（Lzy） 
* passwd：密码 (x) 
* UID：用户身份编号 (1000) 
* GID：登录默认所在组编号 (1000) 
* GECOS：用户全名或注释 
* home directory：用户主目录 (/home/wang) 
* shell：用户默认使用shell (/bin/bash)

#### 4.2.3 shadow文件格式

* 登录用名 
* 用户密码:一般用sha512加密 
* 从1970年1月1日起到密码最近一次被更改的时间 
* 密码再过几天可以被变更（0表示随时可被变更） 
* 密码再过几天必须被变更（99999表示永不过期） 
* 密码过期前几天系统提醒用户（默认为一周） 
* 密码过期几天后帐号会被锁定 
* 从1970年1月1日算起，多少天后帐号失效

更改密码加密算法：

```
authconfig   --passalgo=sha256 --update
```

密码的安全策略 

* 足够长 
* 使用数字、大写字母、小写字母及特殊字符中至少3种 
* 使用随机密码 
* 定期更换,不要使用最近曾经使用过的密码

#### 4.2.4 group文件格式

* 群组名称：就是群组名称 
* 群组密码：通常不需要设定，密码是被记录在 /etc/gshadow  
* GID：就是群组的 ID  
* 以当前组为附加组的用户列表(分隔符为逗号)

```bash
[root@sh-lzy-Centos8 12:00 data]#cat /etc/group
root:x:0:
bin:x:1:
daemon:x:2:
sys:x:3:
adm:x:4:
tty:x:5:
......
tcpdump:x:72:
sshd:x:74:
slocate:x:21:
Lzy:x:1000:
[root@sh-lzy-Centos8 12:05 data]#
```

#### 4.2.5 gshdow文件格式

* 群组名称：就是群的名称 
* 群组密码： 
* 组管理员列表：组管理员的列表，更改组密码和成员 
* 以当前组为附加组的用户列表：多个用户间用逗号分隔

```bash
[root@sh-lzy-Centos8 12:05 data]#cat /etc/gshadow
root:::
bin:::
daemon:::
sys:::
adm:::
tty:::
disk:::
lp:::
mem:::
......
clevis:!::
gnome-initial-setup:!::
tcpdump:!::
sshd:!::
slocate:!::
Lzy:!::
[root@sh-lzy-Centos8 12:08 data]#
```

#### 4.2.6 文件操作

* vipw和vigr 
* pwck和grpck

### 4.3 用户和组管理命令

用户管理命令 

* useradd 
* usermod 
* userdel

组帐号维护命令

* groupadd 
* groupmod 
* groupdel

#### 4.3.1 用户创建

useradd 命令可以创建新的Linux用户

```
useradd [options] LOGIN
```

常见选项：

```
-u UID 
-o 配合-u 选项，不检查UID的唯一性
-g GID 指明用户所属基本组，可为组名，也可以GID
-c "COMMENT“ 用户的注释信息
-d HOME_DIR 以指定的路径(不存在)为家目录
-s SHELL 指明用户的默认shell程序，可用列表在/etc/shells文件中
-G GROUP1[,GROUP2,...] 为用户指明附加组，组须事先存在
-N 不创建私用组做主组，使用users组做主组
-r 创建系统用户 CentOS 6之前: ID<500，CentOS7 以后: ID<1000
-m 创建家目录，用于系统用户
-M 不创建家目录，用于非系统用户
-p 指定加密的密码
```

useradd 命令默认值设定由/etc/default/useradd定义

```bash
[root@sh-lzy-Centos8 12:20 data]#cat /etc/default/useradd 
# useradd defaults file
GROUP=100
HOME=/home
INACTIVE=-1						#对应/etc/shadow文件第7列，即用户密码过期的宽限期
EXPIRE=							#对应/etc/shadow文件第8列，即用户账号的有效期
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes

[root@sh-lzy-Centos8 12:32 data]#
```

显示或更改默认设置

```
useradd -D 
useradd –D -s SHELL
useradd –D –b BASE_DIR
useradd –D –g GROUP
```

新建用户的相关文件

* /etc/default/useradd
* /etc/skel/*
* /etc/login,defs

批量创建用户

```
newusers passwd 格式文件 
```

批量修改用户口令

```
echo username:passwd | chpasswd 
```

#### 4.3.2 用户属性修改

usermod 命令可以修改用户属性

```
usermod [OPTION] login
```

常见选项：

```
-u UID: 新UID
-g GID: 新主组
-G GROUP1[,GROUP2,...[,GROUPN]]]：新附加组，原来的附加组将会被覆盖；若保留原有，则要同时使
用-a选项
-s SHELL：新的默认SHELL
-c 'COMMENT'：新的注释信息
-d HOME: 新家目录不会自动创建；若要创建新家目录并移动原家数据，同时使用-m选项
-l login_name: 新的名字
-L: lock指定用户,在/etc/shadow 密码栏的增加 ! 
-U: unlock指定用户,将 /etc/shadow 密码栏的 ! 拿掉
-e YYYY-MM-DD: 指明用户账号过期日期
-f INACTIVE: 设定非活动期限，即宽限期
```

#### 4.3.3 删除用户

userdel 可删除Linux 用户

```
userdel [OPTION]... Login
```

常见选项：

```
-f, --force   强制
-r, --remove 删除用户家目录和邮箱
```

#### 4.3.4 查看用户相关的ID信息

id 命令可以查看用户的UID，GID等信息

```
id [OPTION]... [USER]
```

常见选项：

```
-u: 显示UID
-g: 显示GID
-G: 显示用户所属的组的ID
-n: 显示名称，需配合ugG使用
```

#### 4.3.5 切换用户或以其他用户身份执行命令

su: 即 switch user，命令可以切换用户身份，并且以指定用户的身份执行命令

```
su [options...] [-] [user [args...]]
```

常见选项：

```
-l  --login   su -l UserName   相当于 su - UserName
-c, --command <command>         pass a single command to the shell with -c
```

切换用户的方式：

* su UserName：非登录式切换，即不会读取目标用户的配置文件，不改变当前工作目录，即不完 全切换 
* su - UserName：登录式切换，会读取目标用户的配置文件，切换至自已的家目录，即完全切换

**说明：**root su至其他用户无须密码；非root用户切换时需要密码 

**注意：**su 切换新用户后，使用 exit 退回至旧的用户身份，而不要再用 su 切换至旧用户，否则会生成很 多的bash子进程，环境可能会混乱。

换个身份执行命令：

```
su [-] UserName -c 'COMMAND'
```

#### 4.3.6 设置密码

passwd 可以修改用户密码

```
passwd [OPTIONS] UserName
```

常用选项：

```
-d：删除指定用户密码
-l：锁定指定用户
-u：解锁指定用户
-e：强制用户下次登录修改密码
-f：强制操作
-n mindays：指定最短使用期限
-x maxdays：最大使用期限
-w warndays：提前多少天开始警告
-i inactivedays：非活动期限
--stdin：从标准输入接收用户密码,Ubuntu无此选项
```

#### 4.3.7 修改用户密码策略

chage 可以修改用户密码策略

```
chage [OPTION]... LOGIN
```

常见选项：

```
-d LAST_DAY               #更改密码的时间
-m --mindays MIN_DAYS
-M --maxdays MAX_DAYS
-W --warndays WARN_DAYS
-I --inactive INACTIVE #密码过期后的宽限期
-E --expiredate EXPIRE_DATE #用户的有效期
-l 显示密码策略
```

```bash
[root@sh-lzy-Centos8 18:27 data]#chage -l Lzy
Last password change					: never
Password expires					: never
Password inactive					: never
Account expires						: never
Minimum number of days between password change		: 0
Maximum number of days between password change		: 99999
Number of days of warning before password expires	: 7
[root@sh-lzy-Centos8 18:27 data]#
```

#### 4.3.8 用户相关的其它命令

* chfn 指定个人信息 
* chsh 指定shell，相当于usermod -s  
* finger 可看用户个人信息

#### 4.3.9 创建组

groupadd实现创建组

```
groupadd [OPTION]... group_name
```

常见选项：

```
-g GID 指明GID号；[GID_MIN, GID_MAX]
-r 创建系统组，CentOS 6之前: ID<500，CentOS 7以后: ID<1000
```

#### 4.3.10 修改组

 groupmod 组属性修改

```
groupmod [OPTION]... group
```

常见选项：

```
-n group_name: 新名字
-g GID: 新的GID
```

#### 4.3.11 组删除

groupdel 可以删除组

```
groupdel [options] GROUP
```

常见选项：

```
-f, --force 强制删除，即使是用户的主组也强制删除组,但会导致无主组的用户不可用无法登录
```

#### 4.3.12 更改组密码

gpasswd命令，可以更改组密码，也可以修改附加组的成员关系

```
gpasswd [OPTION] GROUP
```

常见选项：

```
-a user 将user添加至指定组中
-d user 从指定附加组中移除用户user
-A user1,user2,... 设置有管理权限的用户列表
```

#### 4.3.13 临时切换主组

newgrp 命令可以临时切换主组， 如果用户本不属于此组，则需要组密码

```
newgrp [-] [group]
```

#### 4.3.14 更改和查看组成员

groupmems 可以管理附加组的成员关系

```
groupmems [options] [action]
```

常见选项：

```
-g, --group groupname   #更改为指定组 (只有root)
-a, --add username     #指定用户加入组
-d, --delete username #从组中删除用户
-p, --purge               #从组中清除所有成员
-l,  --list                 #显示组成员列表
```

groups 可查看用户组关系

```
#查看用户所属组列表
groups [OPTION].[USERNAME]... 
```

### 4.4 文件权限管理

程序访问文件时的权限，取决于此程序的发起者

* 进程的发起者，同文件的属主：则应用文件属主权限 
* 进程的发起者，属于文件属组；则应用文件属组权限 
* 应用文件“其它”权限

#### 4.4.1 文件所有者和属组属性操作

##### 4.4.1.1 设置文件的所有者chown

chown 命令可以修改文件的属主，也可以修改文件属组

```
chown [OPTION]... [OWNER][:[GROUP]] FILE...
chown [OPTION]... --reference=RFILE FILE...
```

用法说明：

```
OWNER   #只修改所有者
OWNER:GROUP #同时修改所有者和属组
:GROUP   #只修改属组，冒号也可用 . 替换
--reference=RFILE  #参考指定的的属性，来修改   
-R #递归，此选项慎用，非常危险！
```

##### 4.4.1.2 设置文件的属组信息chgrp

chgrp 命令可以只修改文件的属组

```
 chgrp [OPTION]... GROUP FILE...
 chgrp [OPTION]... --reference=RFILE FILE...
 
 -R			#递归
```

#### 4.4.2 文件权限

##### 4.4.2.1 文件权限说明

文件的权限主要针对三类对象进行定义

```
owner 属主, u
group 属组, g
other 其他, o
```

**注意：**

```
用户的最终权限，是从左向右进行顺序匹配，即，所有者，所属组，其他人，一旦匹配权限立即生效，不再向
右查看其权限
r和w权限对root 用户无效
只要所有者,所属组或other三者之一有x权限,root就可以执行
```

每个文件针对每类访问者都定义了三种常用权限 

每个文件针对每类访问者都定义了三种权限

```
r Readable
w Writable
x eXcutable
```

**对文件的权限：**

```
r 可使用文件查看类工具，比如：cat，可以获取其内容
w 可修改其内容
x 可以把此文件提请内核启动为一个进程，即可以执行（运行）此文件（此文件的内容必须是可执行）
```

**对目录的权限：**

```
r 可以使用ls查看此目录中文件列表
w 可在此目录中创建文件，也可删除此目录中的文件，而和此被删除的文件的权限无关
x 可以cd进入此目录，可以使用ls -l查看此目录中文件元数据（须配合r权限），属于目录的可访问的最小权限
X 只给目录x权限，不给无执行权限的文件x权限
```

##### 4.4.2.2 修改文件权限chmod

```
chmod [OPTION]... MODE[,MODE]... FILE...
chmod [OPTION]... OCTAL-MODE FILE...
#参考RFILE文件的权限，将FILE的修改为同RFILE
chmod [OPTION]... --reference=RFILE FILE...
```

#### 4.4.3 新建文件和目录的默认权限

umask 的值可以用来保留在创建文件权限

实现方式：

* 新建文件的默认权限: 666-umask，如果所得结果某位存在执行（奇数）权限，则将其权限+1,偶 数不变 
* 新建目录的默认权限: 777-umask 

非特权用户umask默认是 002 

root的umask 默认是 022 

查看umask

```
umask
#模式方式显示
umask –S 
#输出可被调用
umask –p 
```

修改umask

```
umask #
```

持久保存umask 

* 全局设置： /etc/bashrc  
* 用户设置：~/.bashrc

#### 4.4.4 Linux文件系统上的特殊权限

前面介绍了三种常见的权限：r, w, x 还有三种特殊权限：SUID, SGID, Sticky 

特殊权限 

* SUID 作用于二进制可执行文件上,用户将继承此程序所有者的权限 

* SGID  

  作用于二进制可执行文件上,用户将继承此程序所有组的权限 

  作于于目录上, 此目录中新建的文件的所属组将自动从此目录继承 

* STICKY 作用于目录上,此目录中的文件只能由所有者自已来删除

##### 4.4.4.1 特殊权限SUID

前提：进程有属主和属组；文件有属主和属组 

* 任何一个可执行程序文件能不能启动为进程,取决发起者对程序文件是否拥有执行权限 
* 启动为进程之后，其进程的属主为发起者,进程的属组为发起者所属的组 
* 进程访问文件时的权限，取决于进程的发起者 

二进制的可执行文件上SUID权限功能： 

* 任何一个可执行程序文件能不能启动为进程：取决发起者对程序文件是否拥有执行权限 
* 启动为进程之后，其进程的属主为原程序文件的属主 
* SUID只对二进制可执行程序有效 
* SUID设置在目录上无意义 

SUID权限设定：

```
chmod u+s FILE...
chmod 4xxx FILE
chmod u-s FILE...
```

##### 4.4.4.2 特殊权限SGID

二进制的可执行文件上SGID权限功能： 

* 任何一个可执行程序文件能不能启动为进程：取决发起者对程序文件是否拥有执行权限 
* 启动为进程之后，其进程的属组为原程序文件的属组

SGID权限设定：

```
chmod g+s FILE... 
chmod 2xxx FILE
chmod g-s FILE...
```

目录上的SGID权限功能： 

默认情况下，用户创建文件时，其属组为此用户所属的主组，一旦某目录被设定了SGID，则对此目录有 写权限的用户在此目录中创建的文件所属的组为此目录的属组，通常用于创建一个协作目录 

SGID权限设定：

```
chmod g+s DIR...
chmod 2xxx DIR
chmod g-s DIR...
```

##### 4.4.4.3 特殊权限 Sticky 位

具有写权限的目录通常用户可以删除该目录中的任何文件，无论该文件的权限或拥有权 

在目录设置Sticky 位，只有文件的所有者或root可以删除该文件 

sticky 设置在文件上无意义 

* Sticky权限设定：

```
chmod o+t DIR...
chmod 1xxx DIR
chmod o-t DIR...
```

##### 4.4.4.4 特殊权限数字法

SUID SGID STICKY

```
000 0
001 1
010 2
011 3
100 4
101 5
110 6
111 7
```

权限位映射 

SUID: user,占据属主的执行权限位 

* s：属主拥有x权限 
* S：属主没有x权限 

SGID: group,占据属组的执行权限位 

* s： group拥有x权限 
* S：group没有x权限 

Sticky: other,占据other的执行权限位 

* t：other拥有x权限 
* T：other没有x权限

#### 4.4.5 设定文件特殊属性

设置文件的特殊属性，可以访问 root 用户误操作删除或修改文件 

不能删除，改名，更改

```
chattr +i file
```

只能追加内容，不能删除，改名

```
chattr +a file
```

显示特定属性

```
lsattr 
```

#### 4.4.6 访问控制列表 ACL

##### 4.4.6.1 ACL权限功能

ACL：Access Control List，实现灵活的权限管理 

除了文件的所有者，所属组和其它人，可以对更多的用户设置权限 

CentOS7 默认创建的xfs和ext4文件系统具有ACL功能 

CentOS7 之前版本，默认手工创建的ext4文件系统无ACL功能,需手动增加

```bash
tune2fs –o acl /dev/sdb1
mount –o acl /dev/sdb1 /mnt/test
```

**ACL生效顺序：**

```
所有者，自定义用户，所属组|自定义组，其他人
```

##### 4.4.6.2 ACL相关命令

setfacl 可设置ACL权限 

getfacl 可查看设置的ACL权限

**mask 权限** 

* mask只影响除所有者和other的之外的人和组的最大权限 
* mask需要与用户的权限进行逻辑与运算后，才能变成有限的权限(Effective Permission) 
* 用户或组的设置必须存在于mask权限设定范围内才会生效 

--set选项会把原有的ACL项都删除，用新的替代，需要注意的是一定要包含UGO的设置，不能象-m一样 

只是添加ACL就可以

##### 4.4.6.3 备份和还原ACL

主要的文件操作命令cp和mv都支持ACL，只是cp命令需要加上-p 参数。但是tar等常见的备份工具是不 会保留目录和文件的ACL信息

# 5. 文本处理工具和正则表达式

### 内容概述 

* 文本编辑工具VIM 
* 各种文本工具 
* 基本正则表达式和扩展正则表达式 
* 文本处理三剑客之grep 
* 文本处理三剑客之sed 
* 文本处理三剑客之awk

### 5.1 文本编辑工具之神VIM

#### 5.1.1 vi和vim简介

在Linux中我们经常编辑修改文本文件，即由ASCII, Unicode 或其它编码的纯文字的文件。之前介绍过 nano，实际工作中我们会使用更为专业，功能强大的工具 

文本编辑种类： 

* 全屏编辑器：nano（字符工具）, gedit(图形化工具)，vi,vim 
* 行编辑器：sed

**vi**

Visual editor，文本编辑器，是 Linux 必备工具之一，功能强大，学习曲线较陡峭，学习难度大

**vim**

VIsual editor iMproved ，和 vi 使用方法一致，但功能更为强大，不是必备软件

其他相关编辑器：gvim 一个Vim编辑器的图形版本

**vim 小抄**

参考链接

```
https://www.w3cschool.cn/vim/
```

#### 5.1.2 使用 vim 初步

##### 5.1.2.1 vim 命令格式

```
vim [OPTION]... FILE...
```

常用选项

```
+# 打开文件后，让光标处于第#行的行首，+默认行尾
+/PATTERN 让光标处于第一个被PATTERN匹配到的行行首
-b file 二进制方式打开文件
-d file1 file2… 比较多个文件，相当于 vimdiff
-m file   只读打开文件
-e file   直接进入ex模式，相当于执行ex file 
-y file   Easy mode (like "evim", modeless)，直接可以操作文件，ctrl+o:wq|q! 保存和不
保存退出
```

说明：

* 如果该文件存在，文件被打开并显示内容 
* 如果该文件不存在，当编辑后第一次存盘时创建它

##### 5.1.2.2 三种主要模式和转换

vim 是 一个模式编辑器，击键行为是依赖于 vim的 的“模式”

**三种常见模式：** 

* 命令或普通(Normal)模式：默认模式，可以实现移动光标，剪切/粘贴文本 
* 插入(Insert)或编辑模式：用于修改文本 
* 扩展命令(extended command )或命令(末)行模式：保存，退出等

**模式转换**

* 命令模式 --> 插入模式

```
i insert, 在光标所在处输入
I 在当前光标所在行的行首输入
a append, 在光标所在处后面输入
A 在当前光标所在行的行尾输入
o 在当前光标所在行的下方打开一个新行
O 在当前光标所在行的上方打开一个新行
```

* 插入模式 --- ESC-----> 命令模式 
* 命令模式 ---- : ----> 扩展命令模式 
* 扩展命令模式 ----ESC,enter----> 命令模式

#### 5.1.3 扩展命令模式

按“:”进入Ex模式 ，创建一个命令提示符: 处于底部的屏幕左侧

##### 5.1.3.1 扩展命令模式基本命令

```
w 写（存）磁盘文件
wq 写入并退出
x 写入并退出
X   加密
q 退出
q！ 不存盘退出，即使更改都将丢失 
r   filename 读文件内容到当前文件中
w   filename 将当前文件内容写入另一个文件
!command 执行命令
r!command 读入命令的输出
```

##### 5.1.3.2 地址定界

```
:start_pos,end_pos CMD
```

###### 5.1.3.2.1 地址定界格式

```
# #具体第#行，例如2表示第2行
#,# #从左侧#表示起始行，到右侧#表示结尾行 
#,+# #从左侧#表示的起始行，加上右侧#表示的行数，范例：2,+3 表示2到5行
.   #当前行
$ #最后一行
.,$-1 #当前行到倒数第二行
% #全文, 相当于1,$
/pattern/   #从当前行向下查找，直到匹配pattern的第一行,即:正则表达式
/pat1/,/pat2/ #从第一次被pat1模式匹配到的行开始，一直到第一次被pat2匹配到的行结束
#,/pat/     #从指定行开始，一直找到第一个匹配pattern的行结束
/pat/,$     #向下找到第一个匹配patttern的行到整个文件的结尾的所有行
```

###### 5.1.3.2.2 地址定界后跟一个编辑命令

```bash
d       #删除
y #复制
w file #将范围内的行另存至指定文件中
r file #在指定位置插入指定文件中的所有内容
```

##### 5.1.3.3 查找并替换

```
s/要查找的内容/替换为的内容/修饰符
```

说明：

```
要查找的内容：可使用基本正则表达式模式
替换为的内容：不能使用模式，但可以使用\1, \2, ...等后向引用符号；还可以使用“&”引用前面查找时查
找到的整个内容
```

修饰符：

```
i #忽略大小写
g #全局替换，默认情况下，每一行只替换第一次出现
gc #全局替换，每次替换前询问
```

查找替换中的分隔符/可替换为其它字符，如：#,@ 

##### 5.1.3.4 定制vim的工作特性

扩展命令模式的配置只是对当前vim进程有效，可将配置存放在文件中持久保存

配置文件：

```
/etc/vimrc #全局
~/.vimrc #个人
```

###### 5.1.3.4.1 行号

```
显示：set number，简写 set nu
取消显示：set nonumber, 简写 set nonu
```

###### 5.1.3.4.2 忽略字符的大小写

```
启用：set ignorecase，简写 set ic
不忽略：set noic
```

###### 5.1.3.4.3 自动缩进

```
启用：set autoindent，简写 set ai
禁用：set noai
```

###### 5.1.3.4.4 复制保留格式

```
启用：set paste
禁用：set nopaste
```

###### 5.1.3.4.5 显示Tab ^I和换行符 和$显示

```
启用：set list
禁用：set nolist
```

###### 5.1.3.4.6 高亮搜索

```
启用：set hlsearch
禁用：set nohlsearch 简写：nohl
```

###### 5.1.3.4.7 语法高亮

```
启用：syntax on
禁用：syntax off
```

###### 5.1.3.4.8 文件格式

```
启用windows格式：set fileformat=dos
启用unix格式：set fileformat=unix
简写 set ff=dos|unix
```

###### 5.1.3.4.9 Tab 用空格代替

```
启用：set expandtab   默认为8个空格代替Tab
禁用：set noexpandtab
简写：set et 
```

###### 5.1.3.4.10 Tab用指定空格的个数代替

```
启用：set tabstop=# 指定#个空格代替Tab
简写：set ts=4
```

###### 5.1.3.4.11 设置缩进宽度

```
#向右缩进 命令模式>>
#向左缩进 命令模式<<
#设置缩进为4个字符
set shiftwidth=4
```

###### 5.1.3.4.12 设置文本宽度

```
set textwidth=65 (vim only) #从左向右计数
set wrapmargin=15           #从右到左计数
```

###### 5.1.3.4.13 设置光标所在行的标识线

```
启用：set cursorline，简写 set cul
禁用：set nocursorline
```

###### 5.1.3.4.14 加密

```
启用： set key=password
禁用： set key=
```

###### 5.1.3.4.15 了解更多

set 帮助

```
:help option-list 
:set or :set all
```

#### 5.1.4 命令模式

命令模式，又称为Normal模式，功能强大，只是此模式输入指令并在屏幕上显示，所以需要记忆大量 的快捷按键才能更好的使用

##### 5.1.4.1 退出VIM

```
ZZ 保存退出
ZQ 不保存退出
```

##### 5.1.4.2 光标跳转

* **字符间跳转：**

```
h: 左 
L: 右 
j: 下 
k: 上
#COMMAND：跳转由#指定的个数的字符
```

* **单词间跳转：**

```
w：下一个单词的词首
e：当前或下一单词的词尾
b：当前或前一个单词的词首
#COMMAND：由#指定一次跳转的单词数
```

* **当前页跳转：**

```
H：页首     
M：页中间行     
L：页底
zt：将光标所在当前行移到屏幕顶端
zz：将光标所在当前行移到屏幕中间
zb：将光标所在当前行移到屏幕底端
```

* **行首行尾跳转：**

```
^ 跳转至行首的第一个非空白字符
0 跳转至行首
$ 跳转至行尾
```

* **行间移动：**

```
#G 或者扩展命令模式下 
:#   跳转至由第#行
G 最后一行
1G, gg 第一行
```

* **句间移动：**

```
) 下一句 
( 上一句
```

* **段落间移动：**

```
} 下一段 
{ 上一段
```

* **命令模式翻屏操作**

```
Ctrl+f 向文件尾部翻一屏,相当于Pagedown
Ctrl+b 向文件首部翻一屏,相当于Pageup
Ctrl+d 向文件尾部翻半屏
Ctrl+u 向文件首部翻半屏
```

##### 5.1.4.3 字符编辑

```
x 剪切光标处的字符
#x 剪切光标处起始的#个字符
xp 交换光标所在处的字符及其后面字符的位置
~ 转换大小写
J 删除当前行后的换行符
```

##### 5.1.4.4 替换命令(replace)

```
r 只替换光标所在处的一个字符
R 切换成REPLACE模式（在末行出现-- REPLACE -- 提示）,按ESC回到命令模式
```

##### 5.1.4.5 删除命令(delete)

```
d 删除命令，可结合光标跳转字符，实现范围删除
d$ 删除到行尾
d^ 删除到非空行首
d0 删除到行首
dw
de
db
#COMMAND
dd：   剪切光标所在的行
#dd 多行删除
D：从当前光标位置一直删除到行尾，等同于d$
```

##### 5.1.4.6 复制命令(yank)

```
y 复制，行为相似于d命令
y$
y0
y^
ye
yw
yb
#COMMAND
yy：复制行
#yy 复制多行
Y：复制整行
```

##### 5.1.4.7 粘贴命令(paste)

```
p 缓冲区存的如果为整行，则粘贴当前光标所在行的下方；否则，则粘贴至当前光标所在处的后面
P 缓冲区存的如果为整行，则粘贴当前光标所在行的上方；否则，则粘贴至当前光标所在处的前面
```

##### 5.1.4.8 改变命令(change)

命令 c 删除后切换成插入模式

```
c$
c^
c0
cb
ce
cw
#COMMAND
cc  #删除当前行并输入新内容，相当于S
#cc  
C   #删除当前光标到行尾，并切换成插入模式,相当于c$
```

##### 5.1.4.9 查找

```
/PATTERN：从当前光标所在处向文件尾部查找
?PATTERN：从当前光标所在处向文件首部查找
n：与命令同方向
N：与命令反方向
```

##### 5.1.4.10 撤消更改

```
u 撤销最近的更改，相当于windows中ctrl+z
#u 撤销之前多次更改
U 撤消光标落在这行后所有此行的更改
Ctrl-r 重做最后的“撤消”更改，相当于windows中crtl+y
. 重复前一个操作
#. 重复前一个操作#次
```

#### 5.1.5 可视化模式

在末行有”-- VISUAL -- “指示，表示在可视化模式 

允许选择的文本块 

* v 面向字符，-- VISUAL -- 
* V 面向整行，-- VISUAL LINE --  
* ctrl-v 面向块，-- VISUAL BLOCK -- 

可视化键可用于与移动键结合使用 

w ) } 箭头等 

突出显示的文字可被删除，复制，变更，过滤，搜索，替换等

**范例：**在文件指定行的行首插入#

```
1、先将光标移动到指定的第一行的行首
2、输入ctrl+v 进入可视化模式
3、向下移动光标，选中希望操作的每一行的第一个字符
4、输入大写字母 I 切换至插入模式
5、输入 # 
6、按 ESC 键
```

**范例：**在指定的块位置插入相同的内容

```
1、光标定位到要操作的地方
2、CTRL+v 进入“可视 块”模式，选取这一列操作多少行
3、SHIFT+i(I)
4、输入要插入的内容
5、按 ESC 键
```

#### 5.1.6 多文件模式

```
vim FILE1 FILE2 FILE3 ...
:next 下一个
:prev 前一个
:first 第一个
:last 最后一个
:wall 保存所有
:qall 不保存退出所有
:wqall保存退出所有
```

#### 5.1.7 多窗口模式

##### 5.1.7.1 多文件分割

```
vim -o|-O FILE1 FILE2 ...
-o: 水平或上下分割
-O: 垂直或左右分割（vim only）
在窗口间切换：Ctrl+w, Arrow
```

##### 5.1.7.2 单文件窗口分割

```
Ctrl+w,s：split, 水平分割，上下分屏
Ctrl+w,v：vertical, 垂直分割，左右分屏
ctrl+w,q：取消相邻窗口
ctrl+w,o：取消全部窗口
:wqall 退出
```

#### 5.1.8 vim的寄存器

有26个命名寄存器和1个无命名寄存器，常存放不同的剪贴版内容，可以在同一个主机的不同会话（终 端窗口）间共享 

寄存器名称a，b,…,z,格式： ”寄存器 放在数字和命令之间

**范例：**

```
3"tyy 表示复制3行到t寄存器中 ，末行显示`3 lines yanked into "t`
"tp 表示将t寄存器内容粘贴
```

未指定，将使用无命名寄存器 

有10个数字寄存器，用0，1，…，9表示，0存放最近复制内容，1存放最近删除内容。当新的文本变更 和删除时，1转存到2，2转存到3，以此类推。数字寄存器不能在不同会话间共享

#### 5.1.9 标记和宏(macro)

* ma 将当前位置标记为a，26个字母均可做标记， mb 、 mc 等等 
* 'a 跳转到a标记的位置，实用的文档内标记方法，文档中跳跃编辑时很有用 
* qa 录制宏 a，a为宏的名称，末行提示： recording @a 
* q 停止录制宏 
* @a 执行宏 a 
* @@ 重新执行上次执行的宏

#### 5.1.10 编辑二进制文件

```
#以二进制方式打开文件
vim -b binaryfile
#扩展命令模式下，利用xxd命令转换为可读的十六进制
:%!xxd
#切换至插入模式下，编辑二进制文件
#切换至扩展命令模式下，利用xxd命令转换回二进制
:%!xxd  -r
#保存退出
```

### 5.2 文本常见处理工具

#### 5.2.1 文件内容查看命令

##### 5.2.1.1 查看文本文件内容 

###### 5.2.1.1.1 cat

cat 可以查看文本内容

```
cat [OPTION]... [FILE]...
```

常见选项

```
-E：显示行结束符$
-A：显示所有控制符
-n：对显示出的每一行进行编号
-b：非空行编号
-s：压缩连续的空行成一行
```

###### 5.2.1.1.2 nl

显示行号，相当于cat -b 

###### 5.2.1.1.3 tac

逆向显示文本内容

###### 5.2.1.1.4 rev

将同一行的内容逆向显示

##### 5.2.1.2 查看非文本文件内容

###### 5.2.1.2.1 hexdump

###### 5.2.1.2.2 od

od 即 dump files in octal and other formats

###### 5.2.1.2.3 xxd

#### 5.2.2 分页查看文件内容

##### 5.2.2.1 more 

可以实现分页查看文件，可以配合管道实现输出信息的分页

```
more [OPTIONS...] FILE...
```

选项：

```
-d: 显示翻页及退出提示
```

##### 5.2.2.2 less

less 也可以实现分页查看文件或STDIN输出，less 命令是man命令使用的分页器

查看时有用的命令包括：

```
/文本 搜索 文本
n/N 跳到下一个 或 上一个匹配
```

#### 5.2.3 显示文本前或后行内容

##### 5.2.3.1 head

可以显示文件或标准输入的前面行

```
head [OPTION]... [FILE]...
```

选项：

```
-c # 指定获取前#字节
-n # 指定获取前#行,#如果为负数,表示从文件头取到倒数第#前
-# 同上
```

```bash
[root@sh-lzy-Centos8 18:30 data]#head -n 10 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
[root@sh-lzy-Centos8 23:58 data]#
```

##### 5.2.3.2 tail

tail 和head 相反，查看文件或标准输入的倒数行

```
tail [OPTION]... [FILE]...
```

常用选项：

```
-c # 指定获取后#字节
-n # 指定获取后#行,如果#是负数,表示从第#行开始到文件结束
-# 同上
-f 跟踪显示文件fd新追加的内容,常用日志监控，相当于 --follow=descriptor,当文件删除再新
建同名文件,将无法继续跟踪文件
-F 跟踪文件名，相当于--follow=name --retry，当文件删除再新建同名文件,将可以继续跟踪文
件
tailf 类似 tail –f，当文件不增长时并不访问文件,节约资源,CentOS8无此工具
```

#### 5.2.4 按列抽取文本cut

cut 命令可以提取文本文件或STDIN数据的指定列

```
cut [OPTION]... [FILE]...
```

常用选项：

```
-d DELIMITER: 指明分隔符，默认tab
-f FILEDS:
     #: 第#个字段,例如:3
     #,#[,#]：离散的多个字段，例如:1,3,6
     #-#：连续的多个字段, 例如:1-6
     混合使用：1-3,7
-c 按字符切割
--output-delimiter=STRING指定输出分隔符
```

#### 5.2.5 合并多个文件 paste

paste 合并多个文件同行号的列到一行

```
paste [OPTION]... [FILE]...
```

常用选项：

```
-d  #分隔符：指定分隔符，默认用TAB
-s  #所有行合成一行显示
```

#### 5.2.6 分析文本的工具

文本数据统计：wc 

整理文本：sort 

比较文件：diff和patch

##### 5.2.6.1 收集文本统计数据 wc

wc 命令可用于统计文件的行总数、单词总数、字节总数和字符总数 

可以对文件或STDIN中的数据统计

常用选项：

```
-l 只计数行数
-w 只计数单词总数
-c 只计数字节总数
-m 只计数字符总数
-L 显示文件中最长行的长度
```

```bash
[root@sh-lzy-Centos8 23:58 data]#wc -l /etc/passwd
47 /etc/passwd
[root@sh-lzy-Centos8 00:06 data]#wc /etc/passwd
  47  106 2591 /etc/passwd
[root@sh-lzy-Centos8 00:06 data]#
```

##### 5.2.6.2 文本排序 sort

把整理过的文本显示在STDOUT，不改变原始文件

```
sort [options] file(s)
```

常用选项：

```
-r 执行反方向（由上至下）整理
-R 随机排序
-n 执行按数字大小整理
-h 人类可读排序,如: 2K 1G 
-f 选项忽略（fold）字符串中的字符大小写
-u 选项（独特，unique），合并重复项，即去重
-t c 选项使用c做为字段界定符
-k # 选项按照使用c字符分隔的 # 列来整理能够使用多次
```

##### 5.2.6.3 去重uniq

uniq命令从输入中删除前后相接的重复的行

```
uniq [OPTION]... [FILE]...
```

常见选项：

```
-c: 显示每行重复出现的次数
-d: 仅显示重复过的行
-u: 仅显示不曾重复的行
```

uniq常和sort 命令一起配合使用：

##### 5.2.6.4 比较文件

###### 5.2.6.4.1 diff

diff 命令比较两个文件之间的区别

```
-u 选项来输出“统一的（unified）”diff格式文件，最适用于补丁文件
```

###### 5.2.6.4.2 patch

patch 复制在其它文件中进行的改变（要谨慎使用）

```
-b 选项来自动备份改变了的文件
```

###### 5.2.6.4.3 vimdiff

相当于 vim -d

###### 5.2.6.4.4 cmp

### 5.3 正则表达式

REGEXP： Regular Expressions，由一类特殊字符及文本字符所编写的模式，其中有些字符（元字符） 不表示字符字面意义，而表示控制或通配的功能，类似于增强版的通配符功能，但与通配符不同，通配符功能是用来**处理文件名**，而正则表达式是处理**文本内容**中字符

正则表达式被很多程序和开发语言所广泛支持：vim, less,grep,sed,awk, nginx,mysql 等

正则表达式分两类： 

* 基本正则表达式：BRE Basic Regular Expressions 
* 扩展正则表达式：ERE Extended Regular Expressions

正则表达式引擎： 

采用不同算法，检查处理正则表达式的软件模块，如：PCRE（Perl Compatible Regular  Expressions） 

正则表达式的元字符分类：字符匹配、匹配次数、位置锚定、分组 

帮助：man 7 regex

#### 5.3.1 基本正则表达式元字符

##### 5.3.1.1 字符匹配

```bash
.   匹配任意单个字符，可以是一个汉字
[]   匹配指定范围内的任意单个字符，示例：[wang]   [0-9]   [a-z]   [a-zA-Z]
[^] 匹配指定范围外的任意单个字符,示例：[^wang] 
[:alnum:] 字母和数字
[:alpha:] 代表任何英文大小写字符，亦即 A-Z, a-z
[:lower:] 小写字母,示例:[[:lower:]],相当于[a-z]
[:upper:] 大写字母
[:blank:] 空白字符（空格和制表符）
[:space:] 包括空格、制表符(水平和垂直)、换行符、回车符等各种类型的空白,比[:blank:]包含的范围
广
[:cntrl:] 不可打印的控制字符（退格、删除、警铃...）
[:digit:] 十进制数字
[:xdigit:]十六进制数字
[:graph:] 可打印的非空白字符
[:print:] 可打印字符
[:punct:] 标点符号
\w 		#匹配单词构成部分，等价于[_[:alnum:]]
\W 		#匹配非单词构成部分，等价于[^_[:alnum:]]
\S     	#匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。
\s     	#匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。注意
Unicode 正则表达式会匹配全角空格符
```

范例：

```bash
[root@sh-lzy-Centos8 19:30 data]#ls /etc/ |grep "rc[.0-6]"		#添加“”为标准正则表达式的写法
rc0.d
rc1.d
rc2.d
rc3.d
rc4.d
rc5.d
rc6.d
rc.d
rc.local
[root@sh-lzy-Centos8 19:30 data]#ls /etc/ |grep "rc[0-6]"		# .表示匹配含rc.的文本内容
rc0.d
rc1.d
rc2.d
rc3.d
rc4.d
rc5.d
rc6.d
[root@sh-lzy-Centos8 19:46 data]#ls /etc/ |grep "rc."
rc0.d
rc1.d
rc2.d
rc3.d
rc4.d
rc5.d
rc6.d
rc.d
rc.local
[root@sh-lzy-Centos8 22:25 data]#ls /etc/ |grep "rc...."
rc.local
```

##### 5.3.1.2 匹配次数

用在要指定次数的字符后面，用于指定前面的字符要出现的次数

```
* #匹配前面的字符任意次，包括0次，贪婪模式：尽可能长的匹配
.* #任意长度的任意字符
\? #匹配其前面的字符出现0次或1次,即:可有可无
\+ #匹配其前面的字符出现最少1次,即:肯定有且 >=1 次
\{n\} #匹配前面的字符n次
\{m,n\} #匹配前面的字符至少m次，至多n次
\{,n\}  #匹配前面的字符至多n次,<=n
\{n,\}  #匹配前面的字符至少n次
```

##### 5.3.1.3 位置锚定

位置锚定可以用于定位出现的位置

```bash
^ 			#行首锚定, 用于模式的最左侧
$ 			#行尾锚定，用于模式的最右侧
^PATTERN$ 	#用于模式匹配整行
^$ 			#空行
^[[:space:]]*$ #空白行
\< 或 \b   	#词首锚定，用于单词模式的左侧
\> 或 \b     #词尾锚定，用于单词模式的右侧
\<PATTERN\>  #匹配整个单词
#注意: 单词是由字母,数字,下划线组成
```

范例：

```bash
[root@sh-lzy-Centos8 22:42 data]#grep root /etc/passwd
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
[root@sh-lzy-Centos8 22:44 data]#grep "^root" /etc/passwd		#行首锚定，过滤条件记得加“”
root:x:0:0:root:/root:/bin/bash

[root@sh-lzy-Centos8 22:46 data]#grep "^#" /etc/fstab 
#
# /etc/fstab
# Created by anaconda on Sun Dec 19 04:00:30 2021
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
[root@sh-lzy-Centos8 22:47 data]#grep "^[^#]" /etc/fstab		#利用字符匹配[^]的功能做行首排除；第一个^表示行首，第二个^表示排除；[^#]表示一个任意非#字符
UUID=5883f118-1366-4365-982a-a07f6543a2a8 /                       xfs     defaults        0 0
UUID=0c1c98d5-8f02-4d2e-866a-cbea4f28ef65 /boot                   ext4    defaults        1 2
UUID=4ddef7a3-b2f0-4ce7-9cea-d778bb3832c3 /data                   xfs     defaults        0 0
UUID=32c0f33e-61bb-4b66-8ec3-a73d5a0ebe4a none                    swap    defaults        0 0

[root@sh-lzy-Centos8 22:51 data]#grep "bash$" /etc/passwd		# 字符串$，表示过滤该字符串结尾的行；行尾锚定
root:x:0:0:root:/root:/bin/bash
Lzy:x:1000:1000:Lzy:/home/Lzy:/bin/bash

#创建文件test.txt
[root@sh-lzy-Centos8 22:54 data]#grep "^google$" test.txt 
google
[root@sh-lzy-Centos8 22:55 data]#grep "le$" test.txt 
google
jsgoogle
hxy gooooogle
[root@sh-lzy-Centos8 22:55 data]#grep "google" test.txt 
google
jsgoogle

#通用取网卡IP数据
[root@sh-lzy-Centos8 23:24 data]#ifconfig ens160 |grep -i mask |grep -o "[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}" |head -1
10.0.0.104
[root@sh-lzy-Centos8 23:40 data]#ifconfig ens160 |grep -i mask |grep -o "\([0-9]\{1,3\}\.\)\{3\}[0-9]\{1,3\}" |head -1				#利用分组的方法合并
10.0.0.104
# grep -E 选项表示使用扩展正则表达式
[root@sh-lzy-Centos8 23:40 data]#ifconfig ens160 |grep -i mask |grep -oE "([0-9]{1,3}\.){3}[0-9]{1,3}" |head -1
10.0.0.104
```



##### 5.3.1.4 分组其它

###### 5.3.1.4.1 分组

分组：() 将多个字符捆绑在一起，当作一个整体处理，如：\(root\)+ 

后向引用：分组括号中的模式匹配到的内容会被正则表达式引擎记录于内部的变量中，这些变量的命名 方式为: \1, \2, \3, ... 

\1 表示从左侧起第一个左括号以及与之匹配右括号之间的模式所匹配到的字符

**注意：** 后向引用 引用前面的分组括号中的模式所匹配字符，而非模式本身

###### 5.3.1.4.2 或者

或者： \\|

示例：

```
a\|b #a或b  
C\|cat #C或cat   
\(C\|c\)at #Cat或cat
```

**正则表达式练习**

1、显示/proc/meminfo文件中以大小s开头的行(要求：使用两种方法) 

2、显示/etc/passwd文件中不以/bin/bash结尾的行 

3、显示用户rpc默认的shell程序 

4、找出/etc/passwd中的两位或三位数 

5、显示CentOS7的/etc/grub2.cfg文件中，至少以一个空白字符开头的且后面有非空白字符的行 

6、找出“netstat -tan”命令结果中以LISTEN后跟任意多个空白字符结尾的行 

7、显示CentOS7上所有UID小于1000以内的用户名和UID 

8、添加用户bash、testbash、basher、sh、nologin(其shell为/sbin/nologin),找出/etc/passwd用户 名和shell同名的行 

9、利用df和grep，取出磁盘各分区利用率，并从大到小排序

#### 5.3.2 扩展正则表达式元字符

##### 5.3.2.1 字符匹配

```
. 任意单个字符
[wang] 指定范围的字符
[^wang] 不在指定范围的字符
[:alnum:] 字母和数字
[:alpha:] 代表任何英文大小写字符，亦即 A-Z, a-z
[:lower:] 小写字母,示例:[[:lower:]],相当于[a-z]
[:upper:] 大写字母
[:blank:] 空白字符（空格和制表符）
[:space:] 水平和垂直的空白字符（比[:blank:]包含的范围广）
[:cntrl:] 不可打印的控制字符（退格、删除、警铃...）
[:digit:] 十进制数字
[:xdigit:]十六进制数字
[:graph:] 可打印的非空白字符
[:print:] 可打印字符
[:punct:] 标点符号
```

##### 5.3.2.2 次数匹配

```
*   匹配前面字符任意次
? 0或1次
+ 1次或多次
{n} 匹配n次
{m,n} 至少m，至多n次
```

##### 5.3.2.3 位置锚定

```
^ 行首
$ 行尾
\<, \b 语首
\>, \b 语尾
```

##### 5.3.2.4 分组其它

```
() 分组
后向引用：\1, \2, ...
| 或者
a|b #a或b
C|cat #C或cat
(C|c)at #Cat或cat
```

**扩展正则表达式练习** 

1、显示三个用户root、mage、wang的UID和默认shell 

2、找出/etc/rc.d/init.d/functions文件中行首为某单词(包括下划线)后面跟一个小括号的行 

3、使用egrep取出/etc/rc.d/init.d/functions中其基名 

4、使用egrep取出上面路径的目录名 

5、统计last命令中以root登录的每个主机IP地址登录次数 

6、利用扩展正则表达式分别表示0-9、10-99、100-199、200-249、250-255 

7、显示ifconfig命令结果中所有IPv4地址 

8、将此字符串：welcome to magedu linux 中的每个字符去重并排序，重复次数多的排到前面

### 5.4 文本处理三剑客

grep 命令主要对文本的（正则表达式）行基于模式进行过滤 

sed：stream editor，文本编辑工具 

awk：Linux上的实现gawk，文本报告生成器

#### 5.4.1 文本处理三剑客之 grep

grep: Global search REgular expression and Print out the line 

作用：文本搜索工具，根据用户指定的“模式”对目标文本逐行进行匹配检查；打印匹配到的行 

模式：由正则表达式字符及文本字符所编写的过滤条件

```http
grep [OPTIONS] PATTERN [FILE...]
```

常见选项：

```
-color=auto 对匹配到的文本着色显示
-m  # 匹配#次后停止
-v 显示不被pattern匹配到的行,即取反
-i 忽略字符大小写
-n 显示匹配的行号
-c 统计匹配的行数
-o 仅显示匹配到的字符串
-q 静默模式，不输出任何信息						# 配合echo $? 可以判断命令是否执行成功
-A # after, 后#行
-B # before, 前#行
-C # context, 前后各#行
-e 实现多个选项间的逻辑or关系,如：grep –e ‘cat ' -e ‘dog' file
-w 匹配整个单词
-E 使用ERE，相当于egrep
-F 不支持正则表达式，相当于fgrep				# 表示扩展正则表达式
-f file 根据模式文件处理
-r   递归目录，但不处理软链接
-R   递归目录，但处理软链接
```

范例：

```bash
[root@sh-lzy-Centos8 21:39 data]#grep "^#" /etc/fstab 
#
# /etc/fstab
# Created by anaconda on Sun Dec 19 04:00:30 2021
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
[root@sh-lzy-Centos8 21:40 data]#grep -v "^#" /etc/fstab			#选项 -v 取反

UUID=5883f118-1366-4365-982a-a07f6543a2a8 /                       xfs     defaults        0 0
UUID=0c1c98d5-8f02-4d2e-866a-cbea4f28ef65 /boot                   ext4    defaults        1 2
UUID=4ddef7a3-b2f0-4ce7-9cea-d778bb3832c3 /data                   xfs     defaults        0 0
UUID=32c0f33e-61bb-4b66-8ec3-a73d5a0ebe4a none                    swap    defaults        0 0

# 多个条件混合筛选过滤
[root@sh-lzy-Centos8 19:30 ~]#grep -Ev "^[[:space:]]*#|^$" /etc/host.conf		
multi on

# 显示行号、不区分大小写
[root@sh-lzy-Centos8 19:34 ~]#grep -in "ROOT" /etc/passwd
1:root:x:0:0:root:/root:/bin/bash
10:operator:x:11:0:operator:/root:/sbin/nologin

# -q参数，配合echo $? 可以判断命令是否执行成功
[root@sh-lzy-Centos8 19:44 ~]#grep -q "root" /etc/passwd
[root@sh-lzy-Centos8 19:45 ~]#echo $?
0
[root@sh-lzy-Centos8 19:45 ~]#grep -q "ROOT" /etc/passwd
[root@sh-lzy-Centos8 19:45 ~]#echo $?
1
```



#### 5.4.2 文本处理三剑客之 sed

官网:

```
http://sed.sourceforge.net/
```

##### 5.4.2.1 sed 工作原理

sed 即 Stream EDitor，和 vi 不同，sed是行编辑器

Sed是从文件或管道中读取一行，处理一行，输出一行；再读取一行，再处理一行，再输出一行，直到 最后一行。每当处理一行时，把当前处理的行存储在临时缓冲区中，称为模式空间（Pattern  Space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下 一行，这样不断重复，直到文件末尾。一次处理一行的设计模式使得sed性能很高，sed在读取大文件时 不会出现卡顿的现象。如果使用vi命令打开几十M上百M的文件，明显会出现有卡顿的现象，这是因为 vi命令打开文件是一次性将文件加载到内存，然后再打开。Sed就避免了这种情况，一行一行的处理， 打开速度非常快，执行速度也很快

##### 5.4.2.2 sed 基本用法

```
sed [option]... 'script;script;...' [inputfile...]
```

常用选项：

```
-n 不输出模式空间内容到屏幕，即不自动打印
-e 多点编辑
-f FILE 从指定文件中读取编辑脚本
-r, -E 使用扩展正则表达式
-i.bak 备份文件并原处编辑
-s           将多个文件视为独立文件，而不是单个连续的长文件流
#说明: 
-ir 不支持
-i -r 支持
-ri   支持
-ni   会清空文件
```

**script格式：**

```
'地址命令'
```

**地址格式：**

```
1. 不给地址：对全文进行处理
2. 单地址：
   #：指定的行，$：最后一行
   /pattern/：被此处模式所能够匹配到的每一行
3. 地址范围：
   #,#     #从#行到第#行，3，6 从第3行到第6行
   #,+#   #从#行到+#行，3,+4 表示从3行到第7行
   /pat1/,/pat2/
   #,/pat/
   /pat/,#
4. 步进：~
     1~2 奇数行
     2~2 偶数行
```

**命令：**

```
p 打印当前模式空间内容，追加到默认输出之后
Ip 忽略大小写输出
d 删除模式空间匹配的行，并立即启用下一轮循环
a [\]text 在指定行后面追加文本，支持使用\n实现多行追加
i [\]text 在行前面插入文本
c [\]text 替换行为单行或多行文本
w file 保存模式匹配的行至指定文件
r file 读取指定文件的文本至模式空间中匹配到的行后
= 为模式空间中的行打印行号
! 模式空间中匹配行取反处理
q           结束或退出sed
```

**查找替代**

```
s/pattern/string/修饰符 查找替换,支持使用其它分隔符，可以是其它形式：s@@@，s###
替换修饰符：
g 行内全局替换
p 显示替换成功的行
w   /PATH/FILE 将替换成功的行保存至文件中
I,i   忽略大小写
```

##### 5.4.2.3 sed 高级用法

sed 中除了模式空间，还另外还支持保持空间（Hold Space）,利用此空间，可以将模式空间中的数 据，临时保存至保持空间，从而后续接着处理，实现更为强大的功能。

常见的高级命令

```
P 打印模式空间开端至\n内容，并追加到默认输出之前
h 把模式空间中的内容覆盖至保持空间中
H 把模式空间中的内容追加至保持空间中
g 从保持空间取出数据覆盖至模式空间
G 从保持空间取出内容追加至模式空间
x 把模式空间中的内容与保持空间中的内容进行互换
n 读取匹配到的行的下一行覆盖至模式空间
N 读取匹配到的行的下一行追加至模式空间
d 删除模式空间中的行
D 如果模式空间包含换行符，则删除直到第一个换行符的模式空间中的文本，并不会读取新的输入行，而使
用合成的模式空间重新启动循环。如果模式空间不包含换行符，则会像发出d命令那样启动正常的新循环
```

**练习：** 

1、删除centos7系统/etc/grub2.cfg文件中所有以空白开头的行行首的空白字符 

2、删除/etc/fstab文件中所有以#开头，后面至少跟一个空白字符的行的行首的#和空白字符 

3、在centos6系统/root/install.log每一行行首增加#号 

4、在/etc/fstab文件中不以#开头的行的行首增加#号 

5、处理/etc/fstab路径,使用sed命令取出其目录名和基名 

6、利用sed 取出ifconfig命令中本机的IPv4地址 

7、统计centos安装光盘中Package目录下的所有rpm文件的以.分隔倒数第二个字段的重复次数 

8、统计/etc/init.d/functions文件中每个单词的出现次数，并排序（用grep和sed两种方法分别实现） 

9、将文本文件的n和n+1行合并为一行，n为奇数行

#### 5.4.3 文本处理三剑客之 awk

##### 5.4.3.1 awk 工作原理和基本用法说明

awk：Aho, Weinberger, Kernighan，报告生成器，格式化文本输出，GNU/Linux发布的AWK目前由自 由软件基金会（FSF）进行开发和维护，通常也称它为 GNU AWK

有多种版本： 

* AWK：原先来源于 AT & T 实验室的的AWK 
* NAWK：New awk，AT & T 实验室的AWK的升级版 
* GAWK：即GNU AWK。所有的GNU/Linux发布版都自带GAWK，它与AWK和NAWK完全兼容 

GNU AWK 用户手册文档

```
https://www.gnu.org/software/gawk/manual/gawk.html
```

gawk：模式扫描和处理语言，可以实现下面功能 

* 文本处理 
* 输出格式化的文本报表 
* 执行算数运算 
* 执行字符串操作

```
awk [options]   'program' var=value   file…
awk [options]   -f programfile    var=value file…
```

**说明：** 

program通常是被放在单引号中，并可以由三种部分组成 

* BEGIN语句块 
* 模式匹配的通用语句块 
* END语句块

常见选项：

*  -F “分隔符” 指明输入时用到的字段分隔符，默认的分隔符是若干个连续空白符 
* -v var=value 变量赋值

**Program格式：**

```
pattern{action statements;..}
```

pattern：决定动作语句何时触发及触发事件，比如：BEGIN,END,正则表达式等 

action statements：对数据进行处理，放在{}内指明，常见：print, printf

**awk 工作过程**

第一步：执行BEGIN{action;… }语句块中的语句 

第二步：从文件或标准输入(stdin)读取一行，然后执行pattern{ action;… }语句块，它逐行扫描文件， 从第一行到最后一行重复这个过程，直到文件全部被读取完毕。 

第三步：当读至输入流末尾时，执行END{action;…}语句块 

BEGIN语句块在awk开始从输入流中读取行之前被执行，这是一个可选的语句块，比如变量初始化、打 印输出表格的表头等语句通常可以写在BEGIN语句块中 

END语句块在awk从输入流中读取完所有的行之后即被执行，比如打印所有行的分析结果这类信息汇总 都是在END语句块中完成，它也是一个可选语句块 

pattern语句块中的通用命令是最重要的部分，也是可选的。如果没有提供pattern语句块，则默认执行{  print }，即打印每一个读取到的行，awk读取的每一行都会执行该语句块

**分割符、域和记录**

* 由分隔符分隔的字段（列column,域field）标记$1,$2...$n称为域标识，$0为所有域，注意：和 shell中变量$符含义不同 
* 文件的每一行称为记录record
* 如果省略action，则默认执行 print $0 的操作

**常用的action分类** 

* output statements：print,printf 
* Expressions：算术，比较表达式等 
* Compound statements：组合语句 
* Control statements：if, while等 
* input statements

**awk控制语句** 

* { statements;… } 组合语句 
* if(condition) {statements;…}  
* if(condition) {statements;…} else {statements;…} 
* while(conditon) {statments;…} 
* do {statements;…} while(condition) 
* for(expr1;expr2;expr3) {statements;…} 
* break 
* continue 
* exit 

##### 5.4.3.2 动作 print

```
print item1, item2, ...
```

**说明：** 

* 逗号分隔符 
* 输出item可以字符串，也可是数值；当前记录的字段、变量或awk的表达式 
* 如省略item，相当于print $0 
* 固定字符符需要用“ ” 引起来，而变量和数字不需要

##### 5.4.3.3 awk 变量 

awk中的变量分为：内置和自定义变量

###### 5.4.3.3.1 常见的内置变量

* FS：输入字段分隔符，默认为空白字符,功能相当于 -F

* OFS：输出字段分隔符，默认为空白字符

* RS：输入记录record分隔符，指定输入时的换行符
* ORS：输出记录分隔符，输出时用指定符号代替换行符
* NF：字段数量
* NR：记录的编号
* FNR：各文件分别计数，记录的编号
* FILENAME：当前文件名
* ARGC：命令行参数的个数
* ARGV：数组，保存的是命令行所给定的各参数，每一个参数：ARGV[0]，......

###### 5.4.3.3.2 自定义变量

自定义变量是区分字符大小写的,使用下面方式进行赋值 

* -v var=value  
* 在program中直接定义

##### 5.4.3.4 动作 printf

printf 可以实现格式化输出

```
printf “FORMAT”, item1, item2, ...
```

**说明：** 

* 必须指定FORMAT 
* 不会自动换行，需要显式给出换行控制符 \n 
* FORMAT中需要分别为后面每个item指定格式符

格式符：与item一一对应

```
%s：显示字符串
%d, %i：显示十进制整数
%f：显示为浮点数
%e, %E：显示科学计数法数值 
%c：显示字符的ASCII码
%g, %G：以科学计数法或浮点形式显示数值
%u：无符号整数
%%：显示%自身
```

修饰符

```
#[.#] 第一个数字控制显示的宽度；第二个#表示小数点后精度，如：%3.1f
- 左对齐（默认右对齐） 如：%-15s
+   显示数值的正负符号   如：%+d
```

##### 5.4.3.5 操作符

* 算术操作符：

```
x+y, x-y, x*y, x/y, x^y, x%y
-x：转换为负数
+x：将字符串转换为数值
```

字符串操作符：没有符号的操作符，字符串连接 

* 赋值操作符：

```
=, +=, -=, *=, /=, %=, ^=，++, --
```

* 比较操作符：

```
==, !=, >, >=, <, <=
```

* 模式匹配符：

```
~ 左边是否和右边匹配，包含关系
!~ 是否不匹配
```

* 逻辑操作符：

```
与：&&，并且关系
或：||，或者关系
非：!，取反
```

* 条件表达式（三目表达式）

```
selector?if-true-expression:if-false-expression
```

##### 5.4.3.6 模式PATTERN

PATTERN:根据pattern条件，过滤匹配的行，再做处理 

* 如果未指定：空模式，匹配每一行

* /regular expression/：仅处理能够模式匹配到的行，需要用/ /括起来

* relational expression: 关系表达式，结果为“真”才会被处理 

  真：结果为非0值，非空字符串 

  假：结果为空字符串或0值

* BEGIN/END模式 

  BEGIN{}：仅在开始处理文件中的文本之前执行一次 

  END{}：仅在文本处理完成之后执行一次

##### 5.4.3.7 条件判断 if-else

语法：

```
if(condition){statement;…}[else statement]
if(condition1){statement1}else if(condition2){statement2}else if(condition3)
{statement3}...... else {statementN}
```

使用场景：对awk取得的整行或某个字段做条件判断

##### 5.4.3.8 条件判断 switch

语法：

```
switch(expression) {case VALUE1 or /REGEXP/: statement1; case VALUE2 or 
/REGEXP2/: statement2; ...; default: statementn}
```

##### 5.4.3.9 循环 while

语法：

```
while (condition) {statement;…}
```

条件“真”，进入循环；条件“假”，退出循环 

使用场景： 

* 对一行内的多个字段逐一类似处理时使用 
* 对数组中的各元素逐一处理时使用

##### 5.4.3.10 循环 do-while

语法：

```
do {statement;…}while(condition)
```

意义：无论真假，至少执行一次循环体

do-while循环 

语法：do {statement;…}while(condition) 

意义：无论真假，至少执行一次循环体

##### 5.4.3.11 循环 for

语法：

```
for(expr1;expr2;expr3) {statement;…}
```

常见用法：

```
for(variable assignment;condition;iteration process) {for-body}
```

特殊用法：能够遍历数组中的元素

```
for(var in array) {for-body}
```

##### 5.4.3.12 continue 和 break

continue 中断本次循环 

break 中断整个循环

格式：

```
continue [n]
break [n]
```

##### 5.4.3.13 next

next 可以提前结束对本行处理而直接进入下一行处理（awk自身循环）

##### 5.4.3.14 数组 

awk的数组为关联数组

```
array_name[index-expression]
```

index-expression 

* 利用数组，实现 k/v 功能 
* 可使用任意字符串；字符串要使用双引号括起来 
* 如果某数组元素事先不存在，在引用时，awk会自动创建此元素，并将其值初始化为“空串” 
* 若要判断数组中是否存在某元素，要使用“index in array”格式进行遍历

##### 5.4.3.15 awk 函数 

awk 的函数分为内置和自定义函数

官方文档

```
https://www.gnu.org/software/gawk/manual/gawk.html#Functions
```

###### 5.4.3.15.1 常见内置函数 

* 数值处理：

```
rand()：返回0和1之间一个随机数
srand()：配合rand() 函数,生成随机数的种子
int()：返回整数
```

* 字符串处理：

```
length([s])：返回指定字符串的长度
sub(r,s,[t])：对t字符串搜索r表示模式匹配的内容，并将第一个匹配内容替换为s
gsub(r,s,[t])：对t字符串进行搜索r表示的模式匹配的内容，并全部替换为s所表示的内容
split(s,array,[r])：以r为分隔符，切割字符串s，并将切割后的结果保存至array所表示的数组中，第
一个索引值为1,第二个索引值为2,…
```

* 可以awk中调用shell命令

```
system('cmd') 
```

空格是awk中的字符串连接符，如果system中需要使用awk中的变量可以使用空格分隔，或者说 除了awk的变量外其他一律用""引用起来

* 时间函数 

官方文档: 时间函数

```
https://www.gnu.org/software/gawk/manual/gawk.html#Time-Functions
```

```
systime() 当前时间到1970年1月1日的秒数
strftime() 指定时间格式  
```

###### 5.4.3.15.2 自定义函数

自定义函数格式：

```
function name ( parameter, parameter, ... ) {
   statements
   return expression
}
```

##### 5.4.3.16 awk 脚本 

将awk程序写成脚本，直接调用或执行

向awk脚本传递参数

```
awkfile  var=value  var2=value2... Inputfile
```

**注意：**

在BEGIN过程中不可用。直到首行输入完成以后，变量才可用。可以通过-v 参数，让awk在执行 BEGIN之前得到变量的值。命令行中每一个指定的变量都需要一个-v参数

**练习**

1、文件host_list.log 如下格式，请提取”.magedu.com”前面的主机名部分并写入到回到该文件中

```
1 www.magedu.com
2 blog.magedu.com
3 study.magedu.com
4 linux.magedu.com
5 python.magedu.com
......
999 study.magedu.com
```

2、统计/etc/fstab文件中每个文件系统类型出现的次数 

3、统计/etc/fstab文件中每个单词出现的次数 

4、提取出字符串Yd$C@M05MB%9&Bdh7dq+YVixp3vpw中的所有数字 

5、有一文件记录了1-100000之间随机的整数共5000个，存储的格式100,50,35,89…请取出其中最大和 最小的整数 

6、解决Dos攻击生产案例：根据web日志或者或者网络连接数，监控当某个IP并发连接数或者短时内 PV达到100，即调用防火墙命令封掉对应的IP，监控频率每隔5分钟。防火墙命令为：iptables -A  INPUT -s IP -j REJECT 

# 6.shell脚本编程

### 内容概述

* 编程基础 
* 脚本基本格式 
* 变量 
* 运算 
* 条件测试 
* 配置用户环境 
* 循环 
* 信号捕捉 
* 函数 
* 数组 
* 高级字符串操作 
* 高级变量
* 其它相关工具

### 6.1 编程基础

#### 6.1.1 程序组成

* 程序：算法+数据结构 
* 数据：是程序的核心 
* 数据结构：数据在计算机中的类型和组织方式 
* 算法：处理数据的方式

#### 6.1.2 程序编程风格

* 面向过程语言 

  @ 做一件事情，排出个步骤，第一步干什么，第二步干什么，如果出现情况A，做什么处理，如 果出现了情况B，做什么处理 

  @ 问题规模小，可以步骤化，按部就班处理 

  @ 以指令为中心，数据服务于指令 

  @ C，shell

* 面向对象语言 

  @ 将编程看成是一个事物，对外界来说，事物是直接使用的,不用关心事物内部的情况。而编程 就是设置事物能够完成功能。 

  @ 一种认识世界、分析世界的方法论。将万事万物抽象为各种对象 

  @ 类是抽象的概念，是万事万物的抽象，是一类事物的共同特征的集合 

  @ 对象是类的具象，是一个实体 

  @ 问题规模大，复杂系统 

  @ 以数据为中心，指令服务于数据 

  @ java，C#，python，golang等

#### 6.1.3 编程语言

编程语言排名链接

```
https://www.tiobe.com/tiobe-index/
```

计算机：运行二进制指令 

编程语言：人与计算机之间交互的语言。分为两种：低级语言和高级语言

* 低级编程语言： 

  机器：二进制的0和1的序列，称为机器指令。与自然语言差异太大，难懂、难写 

  汇编：用一些助记符号替代机器指令，称为汇编语言 

  ​		如：ADD A,B 将寄存器A的数与寄存器B的数相加得到的数放到寄存器A中 

  ​		汇编语言写好的程序需要汇编程序转换成机器指令 

  ​		汇编语言稍微好理解，即机器指令对应的助记符，助记符更接近自然语言 

* 高级编程语言： 

  编译：高级语言-->编译器-->机器代码文件-->执行，如：C，C++ 

  解释：高级语言-->执行-->解释器-->机器代码，如：shell，python，php，JavaScript，perl

编译和解释型语言

#### 6.1.4 编程逻辑处理方式

三种处理逻辑 

* 顺序执行：程序按从上到下顺序执行 
* 选择执行：程序执行过程中，根据条件的不同，进行选择不同分支继续执行 
* 循环执行：程序执行过程中需要重复执行多次某段语句

### 6.2 shell 脚本语言的基本用法

#### 6.2.1 shell 脚本的用途

* 将简单的命令组合完成复杂的工作,自动化执行命令,提高工作效率 
* 减少手工命令的重复输入，一定程度上避免人为错误 
* 将软件或应用的安装及配置实现标准化 
* 用于实现日常性的,重复性的运维工作,如:文件打包压缩备份,监控系统运行状态并实现告警等

#### 6.2.2 shell 脚本基本结构

shell脚本编程：是基于过程式、解释执行的语言 

编程语言的基本结构： 

* 各种系统命令的组合 
* 数据存储：变量、数组 
* 表达式：a + b 
* 控制语句：if  
* shell脚本：包含一些命令或声明，并符合一定格式的文本文件 

格式要求：首行shebang机制

```bash
#!/bin/bash
#!/usr/bin/python
#!/usr/bin/perl 
```

#### 6.2.3 shell脚本创建过程

第一步：使用文本编辑器来创建文本文件 

第一行必须包括shell声明序列：#!

```bash
#!/bin/bash 
```

添加注释,注释以#开头 

第二步：加执行权限 

给予执行权限，在命令行上指定脚本的绝对或相对路径 

第三步：运行脚本 

直接运行解释器，将脚本作为解释器程序的参数运行

#### 6.2.4 shell 脚本注释规范

1、第一行一般为调用使用的语言 

2、程序名，避免更改文件名为无法找到正确的文件 

3、版本号 

4、更改后的时间 

5、作者相关信息 

6、该程序的作用，及注意事项 

7、最后是各版本的更新简要说明

#### 6.2.5 第一个 shell 脚本

```
#!SHEBANG
CONFIGURATION_VARIABLES
FUNCTION_DEFINITIONS
MAIN_CODE
```

范例：第一个 Shell 脚本 hello world

```bash
#第一步：编写相应文件
[root@sh-lzy-Centos8 23:26 data]#vim hello.sh 

#!bin/bash
#----------------------------------------------
#Filename:      hello.sh
#Version:       1.0
#Date:          2022/01/07
#Author:        Lzy
#---------------------------------------------
echo "hello,world!"

#第二步，赋权
[root@sh-lzy-Centos8 23:27 data]#chmod 755 hello.sh 
[root@sh-lzy-Centos8 23:27 data]#ll
total 44
-rw-r--r--.   1 root root    0 Jan  1 20:19 2021-12-31-20:19:53.log
drwxr-xr-x. 143 root root 8192 Jan  1 21:28 bak2022-01-01
-rw-r--r--.   1 root root    4 Jan  3 00:28 bc.log
lrwxrwxrwx.   1 root root    9 Jan  2 22:52 dirlink -> /data/dir
drwxr-xr-x. 142 root root 8192 Jan  1 18:46 etc-bak2022-01-01-21-29-39
-rw-r--r--.   1 root root    0 Jan  1 20:00 f1.txt
-rwxr-xr-x.   1 root root  192 Jan  7 23:25 hello.sh
-rw-r--r--.   1 root root    7 Jan  1 18:48 linux.txt
dr-xr-x---.   5 root root  240 Jan  1 16:01 rootdir
drwxr-xr-x.   5 root root 4096 Jan  1 21:18 sysconfig-bak
drwxr-xr-x.   2 root root   32 Jan  1 21:22 test
-rw-r--r--.   1 root root    6 Jan  1 18:51 win.txt
[root@sh-lzy-Centos8 23:27 data]#

#第三步：执行文件
[root@sh-lzy-Centos8 23:27 data]#bash hello.sh 
hello,world!
[root@sh-lzy-Centos8 23:29 data]#
```

#### 6.2.6 shell 脚本调试

只检测脚本中的语法错误，但无法检查出命令错误，但不真正执行脚本

```
bash -n /path/to/some_script
```

调试并执行

```
bash -x /path/to/some_script
```

**总结：脚本错误常见的有三种** 

* 语法错误，会导致后续的命令不继续执行，可以用bash -n 检查错误，提示的出错行数不一定是准 确的 
* 命令错误，默认后续的命令还会继续执行，用bash -n 无法检查出来 ，可以使用 bash -x 进行观察 
* 逻辑错误：只能使用 bash -x 进行观察

#### 6.2.7 变量

##### 6.2.7.1 变量

变量表示命名的内存空间，将数据放在内存空间中，通过变量名引用，获取数据

##### 6.2.7.2 变量类型

变量类型： 

* 内置变量，如：PS1，PATH，UID，HOSTNAME，$$，BASHPID，PPID，$?，HISTSIZE 
* 用户自定义变量

不同的变量存放的数据不同，决定了以下 

* 数据存储方式 
* 参与的运算 
* 表示的数据范围

变量数据类型： 

* 字符 
* 数值：整型、浮点型,bash 不支持浮点数

##### 6.2.7.3 编程语言分类

**静态和动态语言** 

* 静态编译语言：使用变量前，先声明变量类型，之后类型不能改变，在编译时检查，如：java，c 
* 动态编译语言：不用事先声明，可随时改变类型，如：bash，Python

**强类型和弱类型语言** 

* 强类型语言：不同类型数据操作，必须经过强制转换才同一类型才能运算，如java ， c# ， python 

  如：参考以下 python 代码 

  print('magedu'+ 10) 提示出错，不会自动转换类型 

  print('magedu'+str(10)) 结果为magedu10，需要显示转换类型 

* 弱类型语言：语言的运行时会隐式做数据类型转换。无须指定类型，默认均为字符型；参与运算会 自动进行隐式类型转换；变量无须事先定义可直接调用 如：bash ，php，javascript

##### 6.2.7.4 Shell中变量命名法则

###### 6.2.7.4.1 命名要求

* 区分大小写 
* 不能使程序中的保留字和内置变量：如：if, for
* 只能使用数字、字母及下划线，且不能以数字开头，注意：不支持短横线 “ - ”，和主机名相反

###### 6.2.7.4.2 命名习惯

* 见名知义，用英文单词命名，并体现出实际作用，不要用简写，如：ATM 
* 变量名大写 
* 局部变量小写 
* 函数名小写 
* 大驼峰StudentFirstName  
* 小驼峰studentFirstName  
* 下划线: student_name

##### 6.2.7.5 变量定义和引用

变量的生效范围等标准划分变量类型 

* 普通变量：生效范围为当前shell进程；对当前shell之外的其它shell进程，包括当前shell的子shell 进程均无效 
* 环境变量：生效范围为当前shell进程及其子进程 
* 本地变量：生效范围为当前shell进程中某代码片断，通常指函数

**变量赋值：**

```bash
name='value'
```

value 可以是以下多种形式

```
直接字串：name='root'
变量引用：name="$USER"
命令引用：name=`COMMAND` 或者 name=$(COMMAND)
```

**注意：**变量赋值是临时生效，当退出终端后，变量会自动删除，无法持久保存，脚本中的变量会随着脚 本结束，也会自动删除

**变量引用：**

```
$name
${name} 
```

弱引用和强引用 

* "$name " 弱引用，其中的变量引用会被替换为变量值 

* '$name ' 强引用，其中的变量引用不会被替换为变量值，而保持原字符串

显示已定义的所有变量：

```bash
set
```

**删除变量：**

```bash
unset <name>
```

**练习：** 

 1、编写脚本 systeminfo.sh，显示当前主机系统信息，包括:主机名，IPv4地址，操作系统版本，内核 版本，CPU型号，内存大小，硬盘大小

2、编写脚本 backup.sh，可实现每日将 /etc/ 目录备份到 /backup/etcYYYY-mm-dd中 

3、编写脚本 disk.sh，显示当前硬盘分区中空间利用率最大的值 

4、编写脚本 links.sh，显示正连接本主机的每个远程主机的IPv4地址和连接数，并按连接数从大到小排 序

##### 6.2.7.6 环境变量

**环境变量：** 

* 可以使子进程（包括孙子进程）继承父进程的变量，但是无法让父进程使用子进程的变量 
* 一旦子进程修改从父进程继承的变量，将会新的值传递给孙子进程 
* 一般只在系统配置文件中使用，在脚本中较少使用

变量声明和赋值：

```
#声明并赋值
export name=VALUE
declare -x name=VALUE

#或者分两步实现
name=VALUE
export name
```

变量引用：

```bash
$name
${name}
```

显示所有环境变量：

```bash
env
printenv
export
declare -x
```

查看指定进程的环境变量：

```bash
cat /proc/$PID/environ
```

删除变量：

```bash
unset name
```

bash内建的环境变量：

```bash
PATH
SHELL
USER
UID
HOME
PWD
SHLVL #shell的嵌套层数，即深度
LANG
MAIL
HOSTNAME
HISTSIZE
_   #下划线,表示前一命令的最后一个参数
```

##### 6.2.7.7 只读变量（常量）

只读变量：只能声明定义，但后续不能修改和删除，即常量

声明只读变量：

```bash
readonly name
declare -r name
```

查看只读变量：

```bash
readonly [-p]
declare -r
```

##### 6.2.7.8 位置变量

位置变量：在bash shell中内置的变量, 在脚本代码中调用通过命令行传递给脚本的参数

```
$1, $2, ... 对应第1个、第2个等参数，shift [n]换位置
$0 命令本身,包括路径
$* 传递给脚本的所有参数，全部参数合为一个字符串
$@ 传递给脚本的所有参数，每个参数为独立字符串
$# 传递给脚本的参数的个数
注意：$@ $* 只在被双引号包起来的时候才会有差异
```

清空所有位置变量

```bash
set --
```

##### 6.2.7.9 退出状态码变量

当我们浏览网页时，有时会看到下图所显示的数字，表示网页的错误信息，我们称为状态码，在shell脚 本中也有相似的技术表示程序执行的相应状态。

进程执行后，将使用变量 $? 保存状态码的相关数字，不同的值反应成功或失败，$?取值范例 0-255 

```
$?的值为0 #代表成功
$?的值是1到255   #代表失败
```

用户可以在脚本中使用以下命令自定义退出状态码

```bash
exit [n]
```

**注意：** 

* 脚本中一旦遇到exit命令，脚本会立即终止；终止退出状态取决于exit命令后面的数字 
* 如果exit后面无数字,终止退出状态取决于exit命令前面命令执行结果 
* 如果没有exit命令, 即未给脚本指定退出状态码，整个脚本的退出状态码取决于脚本中执行的最后 一条命令的状态码

##### 6.2.7.10 展开命令行

**展开命令执行顺序**

```
把命令行分成单个命令词
展开别名
展开大括号的声明{}
展开波浪符声明 ~
命令替换$() 和 ``
再次把命令行分成命令词
展开文件通配*、?、[abc]等等
准备I/0重导向 <、>
运行命令
```

**防止扩展**

```
反斜线（\）会使随后的字符按原意解释
```

**加引号来防止扩展**

```
单引号（’’）防止所有扩展
双引号（”“）也可防止扩展，但是以下情况例外：$（美元符号）
```

**变量扩展**

```
`` : 反引号，命令替换
\：反斜线，禁止单个字符扩展
!：叹号，历史命令替换
```

##### 6.2.7.11 脚本安全和 set

set 命令：可以用来定制 shell 环境

**$- 变量** 

* h：hashall，打开选项后，Shell 会将命令所在的路径hash下来，避免每次都要查询。通过set +h将h选 项关闭 
* i：interactive-comments，包含这个选项说明当前的 shell 是一个交互式的 shell。所谓的交互式shell, 在脚本中，i选项是关闭的 
* m：monitor，打开监控模式，就可以通过Job control来控制进程的停止、继续，后台或者前台执行等 
* B：braceexpand，大括号扩展 
* H：history，H选项打开，可以展开历史列表中的命令，可以通过!感叹号来完成，例如“!!”返回上最近的 一个历史命令，“!n”返回第 n 个历史命令

**set 命令实现脚本安全** 

* -u 在扩展一个没有设置的变量时，显示错误信息， 等同set -o nounset 

* -e 如果一个命令返回一个非0退出状态值(失败)就退出， 等同set -o errexit 

* -o option 显示，打开或者关闭选项 

  显示选项：set -o 

  打开选项：set -o 选项 

  关闭选项：set +o 选项

* -x 当执行命令时，打印命令及其参数,类似 bash -x 

#### 6.2.8 格式化输出 printf

```bash
printf "指定的格式" "文本1" ”文本2“……
```

**常用格式替换符**

| 替换符 |                             功能                             |
| :----: | :----------------------------------------------------------: |
|   %s   |                            字符串                            |
| %d,%i  |                          十进制整数                          |
|   %f   |                           浮点格式                           |
|   %c   |            ASCII字符，即显示对应参数的第一个字符             |
|   %b   | 相对应的参数中包含转义字符时，可以使用此替换符进行替换，对应的转义字符会被转 义 |
|   %o   |                           八进制值                           |
|   %u   |                     不带正负号的十进制值                     |
|   %x   |                      十六进制值（a-f）                       |
|   %X   |                      十六进制值（A-F）                       |
|   %%   |                          表示%本身                           |

**说明：**

```
%#s 中的数字代表此替换符中的输出字符宽度，不足补空格，默认是右对齐,%-10s表示10个字符宽，- 表示
左对齐
%03d 表示3位宽度,不足前面用0补全,超出位数原样输出
%.2f 中的2表示小数点后显示的小数位数
```

**常用转义字符**

| 转义符 |              功能              |
| :----: | :----------------------------: |
|   \a   | 警告字符，通常为ASCII的BEL字符 |
|   \b   |              后退              |
|   \f   |              换页              |
|   \n   |              换行              |
|   \r   |              回车              |
|   \t   |           水平制表符           |
|   \v   |           垂直制表符           |
|   \    |           表示\本身            |

#### 6.2.9 算术运算

Shell允许在某些情况下对算术表达式进行求值，比如：let和declare 内置命令，(( ))复合命令和算术扩 展。求值以固定宽度的整数进行，不检查溢出，尽管除以0 被困并标记为错误。运算符及其优先级，关 联性和值与C语言相同。以下运算符列表分组为等优先级运算符级别。级别按降序排列优先。

**注意：bash 只支持整数，不支持小数**

```
* / % multiplication, division, remainder, %表示取模，即取余数，示例：9%4=1，5%3=2
+ -   addition, subtraction
i++ i-- variable post-increment and post-decrement
++i --i variable pre-increment and pre-decrement
= *= /= %= += -= <<= >>= &= ^= |=         assignment
- +   unary minus and plus
! ~   logical and bitwise negation
**     exponentiation 乘方,即指数运算
<< >> left and right bitwise shifts
<= >= < >       comparison
== != equality and inequality
&     bitwise AND
|     bitwise OR
^     bitwise exclusive OR
&&     logical AND
||     logical OR
expr?expr:expr       conditional operator
expr1 , expr2     comma
```

乘法符号有些场景中需要转义 

实现算术运算：

```
(1) let var=算术表达式
(2) ((var=算术表达式)) 和上面等价
(3) var=$[算术表达式]
(4) var=$((算术表达式))
(5) var=$(expr arg1 arg2 arg3 ...)
(6) declare -i var = 数值
(7) echo '算术表达式' | bc 
```

内建的随机数生成器变量：

```
$RANDOM   取值范围：0-32767
```

增强型赋值：

```
+= i+=10 相当于 i=i+10
-= i-=j   相当于 i=i-j
*=
/=
%=
++ i++,++i   相当于 i=i+1
-- i--,--i   相当于 i=i-1
```

格式：

```bash
let varOPERvalue
```

#### 6.2.10 逻辑运算

​					**与逻辑**																**或逻辑**													 **非逻辑**

|  A   |  B   | Fault |      |  A   |  B   | Fault |      |  A   | Fault |
| :--: | :--: | :---: | ---- | :--: | :--: | :---: | ---- | :--: | :---: |
|  0   |  0   |   0   |      |  0   |  0   |   0   |      |  1   |   0   |
|  0   |  1   |   0   |      |  0   |  1   |   1   |      |  0   |   1   |
|  1   |  0   |   0   |      |  1   |  0   |   1   |      |      |       |
|  1   |  1   |   1   |      |  1   |  1   |   1   |      |      |       |

**true, false**

```
1,真
0,假
#注意,以上为二进制
```

**与：&：和0相与结果为0，和1相与结果保留原值, 一假则假,全真才真**

**或：|：和1相或结果为1，和0相或结果保留原值,一真则真,全假才假**

**非：！**

**异或：^**

异或的两个值，相同为假，不同为真。两个数字X,Y异或得到结果Z，Z再和任意两者之一X异或，将得出 另一个值Y

```
0 ^ 0 = 0
0 ^ 1 = 1
1 ^ 0 = 1
1 ^ 1 = 0
```

**短路运算**

* 短路与 &&

```
CMD1 短路与 CMD2
第一个CMD1结果为真 (1)，第二个CMD2必须要参与运算，才能得到最终的结果 
第一个CMD1结果为假 (0)，总的结果必定为0，因此不需要执行CMD2
```

* 短路或 ||

```
CMD1 短路或 CMD2
第一个CMD1结果为真 (1)，总的结果必定为1，因此不需要执行CMD2
第一个CMD1结果为假 (0)，第二个CMD2 必须要参与运算,才能得到最终的结果
```

#### 6.2.11 条件测试命令

条件测试：判断某需求是否满足，需要由测试机制来实现，专用的测试表达式需要由测试命令辅助完成 测试过程，实现评估布尔声明，以便用在条件性环境下进行执行

若真，则状态码变量 $? 返回0 

若假，则状态码变量 $? 返回1

条件测试命令

* test EXPRESSION 
* [ EXPRESSION ] #和test 等价，建议使用 [ ] 
* [[ EXPRESSION ]] 相关于增强版的 [ ], 支持[]的用法,且支持扩展正则表达式和通配符

注意：EXPRESSION前后必须有空白字符

##### 6.2.11.1 变量测试

```
#判断 NAME 变量是否定义
[ -v NAME ] 
#判断 NAME 变量是否定义并且是名称引用,bash 4.4新特性
[ -R NAME ]
```

##### 6.2.11.2 数值测试

```
-eq 是否等于
-ne 是否不等于
-gt 是否大于
-ge 是否大于等于
-lt 是否小于
-le 是否小于等于
```

算术表达式比较

```
==   #相等
!=   #不相等
<= 
>= 
< 
> 
```

##### 6.2.11.3 字符串测试

**test和 [ ]用法**

```
test和 [ ]用法
-z STRING 字符串是否为空，没定义或空为真，不空为假，
-n STRING 字符串是否不空，不空为真，空为假 
   STRING   同上
STRING1 = STRING2 是否等于，注意 = 前后有空格
STRING1 != STRING2 是否不等于
> ascii码是否大于ascii码
< 是否小于
```

**[[]] 用法**

```
[[ expression ]] 用法
== 左侧字符串是否和右侧的PATTERN相同
 注意:此表达式用于[[ ]]中，PATTERN为通配符
=~ 左侧字符串是否能够被右侧的正则表达式的PATTERN所匹配
 注意: 此表达式用于[[ ]]中；扩展的正则表达式
```

**建议：当使用正则表达式或通配符使用[[ ]]，其它情况一般使用 [ ]**

##### 6.2.11.4 文件测试

存在性测试：

```
-a FILE：同 -e
-e FILE: 文件存在性测试，存在为真，否则为假
-b FILE：是否存在且为块设备文件
-c FILE：是否存在且为字符设备文件
-d FILE：是否存在且为目录文件
-f FILE：是否存在且为普通文件
-h FILE 或 -L FILE：存在且为符号链接文件
-p FILE：是否存在且为命名管道文件
-S FILE：是否存在且为套接字文件
```

文件权限测试：

```
-r FILE：是否存在且可读
-w FILE: 是否存在且可写
-x FILE: 是否存在且可执行
-u FILE：是否存在且拥有suid权限
-g FILE：是否存在且拥有sgid权限
-k FILE：是否存在且拥有sticky权限
```

**注意：最终结果由用户对文件的实际权限决定，而非文件属性决定**

文件属性测试：

```
-s FILE #是否存在且非空
-t fd #fd 文件描述符是否在某终端已经打开
-N FILE #文件自从上一次被读取之后是否被修改过
-O FILE #当前有效用户是否为文件属主
-G FILE #当前有效用户是否为文件属组
FILE1 -ef FILE2 #FILE1是否是FILE2的硬链接
FILE1 -nt FILE2 #FILE1是否新于FILE2（mtime）
FILE1 -ot FILE2 #FILE1是否旧于FILE2
```

#### 6.2.12 关于 () 和 {}

（CMD1;CMD2;...）和 { CMD1;CMD2;...; } 都可以将多个命令组合在一起，批量执行

```bash
[root@sh-lzy-Centos8 13:38 data]#man bash
......
   Compound Commands
       A  compound  command  is  one of the following.  In most cases a list in a command's description may be
       separated from the rest of the command by one or more newlines, and may be followed  by  a  newline  in
       place of a semicolon.

       (list) list  is executed in a subshell environment (see COMMAND EXECUTION ENVIRONMENT below).  Variable
              assignments and builtin commands that affect the shell's environment do  not  remain  in  effect
              after the command completes.  The return status is the exit status of list.

       { list; }
              list  is  simply executed in the current shell environment.  list must be terminated with a new‐
              line or semicolon.  This is known as a group command.  The return status is the exit  status  of
              list.   Note  that  unlike the metacharacters ( and ), { and } are reserved words and must occur
              where a reserved word is permitted to be recognized.  Since they do not cause a word break, they
              must be separated from list by whitespace or another shell metacharacter.
......
```

```
( list ) 会开启子shell,并且list中变量赋值及内部命令执行后,将不再影响后续的环境
帮助参看:man bash 搜索(list)
{ list; } 不会启子shell, 在当前shell中运行,会影响当前shell环境
帮助参看:man bash 搜索{ list; }
```

#### 6.2.13 组合测试条件

##### 6.2.13.1 第一种方式

```
[ EXPRESSION1 -a EXPRESSION2 ] 并且，EXPRESSION1和EXPRESSION2都是真，结果才为真
[ EXPRESSION1 -o EXPRESSION2 ] 或者，EXPRESSION1和EXPRESSION2只要有一个真，结果就为
真
[ ! EXPRESSION ] 取反
```

**说明： -a 和 -o 需要使用测试命令进行，[[ ]] 不支持**

##### 6.2.13.2 第二种方式

```
COMMAND1 && COMMAND2 #并且，短路与，代表条件性的AND THEN
如果COMMAND1 成功,将执行COMMAND2,否则,将不执行COMMAND2

COMMAND1 || COMMAND2 #或者，短路或，代表条件性的OR ELSE
如果COMMAND1 成功,将不执行COMMAND2,否则,将执行COMMAND2

! COMMAND   #非,取反
```

**练习** 

1、编写脚本 argsnum.sh，接受一个文件路径作为参数；如果参数个数小于1，则提示用户“至少应该给 一个参数”，并立即退出；如果参数个数不小于1，则显示第一个参数所指向的文件中的空白行数 

2、编写脚本 hostping.sh，接受一个主机的IPv4地址做为参数，测试是否可连通。如果能ping通，则提 示用户“该IP地址可访问”；如果不可ping通，则提示用户“该IP地址不可访问” 

3、编写脚本 checkdisk.sh，检查磁盘分区空间和inode使用率，如果超过80%，就发广播警告空间将满 

4、编写脚本 per.sh，判断当前用户对指定参数文件，是否不可读并且不可写 

5、编写脚本 excute.sh ，判断参数文件是否为sh后缀的普通文件，如果是，添加所有人可执行权限， 否则提示用户非脚本文件 

6、编写脚本 nologin.sh和 login.sh，实现禁止和允许普通用户登录系统

#### 6.2.14 使用read命令来接受输入

使用read来把输入值分配给一个或多个shell变量，read从标准输入中读取值，给每个单词分配一个变 量，所有剩余单词都被分配给最后一个变量，如果变量名没有指定，默认标准输入的值赋值给系统内置 变量REPLY

```
read [options] [name ...]
```

常见选项：

```
-p   指定要显示的提示
-s   静默输入，一般用于密码
-n N 指定输入的字符长度N
-d '字符'   输入结束符
-t N TIMEOUT为N秒
```

### 6.3 bash shell 的配置文件

bash shell的配置文件很多，可以分成下面类别

#### 6.3.1 按生效范围划分两类

全局配置：针对所有用户皆有效

```
/etc/profile
/etc/profile.d/*.sh
/etc/bashrc
```

个人配置：只针对特定用户有效

```
~/.bash_profile
~/.bashrc
```

#### 6.3.2 shell登录两种方式分类

##### 6.3.2.1 交互式登录

* 直接通过终端输入账号密码登录 
* 使用 su - UserName 切换的用户

**配置文件生效和执行顺序：**

```bash
#放在每个文件最前
/etc/profile
/etc/profile.d/*. sh
/etc/bashrc
~/ .bash_ profile
~/ .bashrc
/etc/bashrc

#放在每个文件最后
/etc/profile.d/*.sh
/etc/bashrc
/etc/profile
/etc/bashrc    #此文件执行两次
~/.bashrc
~/.bash_profile
```

**注意：文件之间的调用关系，写在同一个文件的不同位置，将影响文件的执行顺序**

##### 6.3.2.2 非交互式登录

* su UserName 
* 图形界面下打开的终端 
* 执行脚本 
* 任何其它的bash实例

执行顺序：

```bash
/etc/profile.d/*.sh
/etc/bashrc
~/.bashrc
```

#### 6.3.3 按功能划分分类

profile类和bashrc类

##### 6.3.3.1 Profile类 

profile类为交互式登录的shell提供配置

```bash
全局：/etc/profile, /etc/profile.d/*.sh
个人：~/.bash_profile
```

功用：

(1) 用于定义环境变量 

(2) 运行命令或脚本

##### 6.3.3.2 Bashrc类

bashrc类：为非交互式和交互式登录的shell提供配置

```
全局：/etc/bashrc
个人：~/.bashrc
```

功用： 

(1) 定义命令别名和函数 

(2) 定义本地变量

#### 6.3.4 编辑配置文件生效

修改profile和bashrc文件后需生效两种方法: 

1. 重新启动shell进程 

​	2. source|. 配置文件

注意:source 会在当前shell中执行脚本,所有一般只用于执行置文件,或在脚本中调用另一个脚本的场景

#### 6.3.5 Bash 退出任务

保存在~/.bash_logout文件中（用户）,在退出登录shell时运行

功能： 

* 创建自动备份 
* 清除临时文件

**练习** 

1、让所有用户的PATH环境变量的值多出一个路径，例如：/usr/local/apache/bin 

2、用户 root 登录时，将命令指示符变成红色，并自动启用如下别名： 

​	rm=‘rm -i’ 

​	cdnet=‘cd /etc/sysconfig/network-scripts/’ 

​	editnet=‘vim /etc/sysconfig/network-scripts/ifcfg-eth0’ 

​	editnet=‘vim /etc/sysconfig/network-scripts/ifcfg-eno16777736 或 ifcfg-ens33 ’ (如果系统是 CentOS7) 

3、任意用户登录系统时，显示红色字体的警示提醒信息“Hi,dangerous！” 

4、编写生成脚本基本格式的脚本，包括作者，联系方式，版本，时间，描述等

### 6.4 流程控制

#### 6.4.1 条件选择

##### 6.4.1.1 条件判断分绍

###### 6.4.1.1.1 单分支条件

![](C:\Users\Administrator\Desktop\SRE笔记图片\6.4.1.1.1 单分支条件.jpg)

###### 6.4.1.1.2 多分支条件

![](C:\Users\Administrator\Desktop\SRE笔记图片\6.4.1.1.2 多分支条件.jpg)

![](C:\Users\Administrator\Desktop\SRE笔记图片\6.4.1.1.2 多分支条件-2.jpg)

##### 6.4.1.2 选择执行 if 语句

格式：

```bash
if COMMANDS; then COMMANDS; [ elif COMMANDS; then COMMANDS; ]... [ else 
COMMANDS; ] fi
```

**单分支**

```bash
if 判断条件;then
   条件为真的分支代码
fi
```

**双分支**

```bash
if 判断条件; then
 条件为真的分支代码
else
 条件为假的分支代码
fi
```

**多分支**

```bash
if 判断条件1; then
 条件1为真的分支代码
elif 判断条件2; then
 条件2为真的分支代码
elif 判断条件3; then
 条件3为真的分支代码
...
else
 以上条件都为假的分支代码
fi
```

说明： 

* 多个条件时，逐个条件进行判断，第一次遇为“真”条件时，执行其分支，而后结束整个if语句

* if 语句可嵌套

##### 6.4.1.3 条件判断 case 语句

格式：

```bash
case WORD in [PATTERN [| PATTERN]...) COMMANDS ;;]... esac
```

```bash
case 变量引用 in
PAT1)
 分支1
 ;;
PAT2)
 分支2
 ;;
...
*)
 默认分支
 ;;
esac
```

case支持glob风格的通配符：

```
* 任意长度任意字符
? 任意单个字符
[] 指定范围内的任意单个字符
|   或者，如: a|b
```

**练习 **

1、编写脚本 createuser.sh，实现如下功能：使用一个用户名做为参数，如果指定参数的用户存在，就 显示其存在，否则添加之。并设置初始密码为123456，显示添加的用户的id号等信息，在此新用户第一 次登录时，会提示用户立即改密码，如果没有参数，就提示：请输入用户名 

2、编写脚本 yesorno.sh，提示用户输入yes或no,并判断用户输入的是yes还是no,或是其它信息 

3、编写脚本 filetype.sh，判断用户输入文件路径，显示其文件类型（普通，目录，链接，其它文件类 型） 

4、编写脚本 checkint.sh，判断用户输入的参数是否为正整数 

5、编写脚本 reset.sh，实现系统安装后的初始化环境，包括：1、别名 2、环境变量，如PS1等 3、 安装常用软件包，如：tree 5、实现固定的IP的设置，6、vim的设置等

#### 6.4.2 循环

##### 6.4.2.1 循环执行介绍

将某代码段重复运行多次，通常有进入循环的条件和退出循环的条件

重复运行次数 

* 循环次数事先已知 
* 循环次数事先未知

常见的循环的命令：for, while, until

![](C:\Users\Administrator\Desktop\SRE课程资料\SRE笔记图片\6.4.2.1 循环执行介绍.jpg)

##### 6.4.2.2 循环 for

**格式1：**

```bash
for NAME [in WORDS ... ] ; do COMMANDS; done
#方式1
for 变量名  in 列表;do
 循环体
done
#方式2
for 变量名  in 列表
do
 循环体
done
```

**执行机制：** 

* 依次将列表中的元素赋值给“变量名”; 每次赋值后即执行一次循环体; 直到列表中的元素耗尽，循环 结束 
* 如果省略 [in WORDS ... ] ，此时使用位置参数变量 in "$@"

**for 循环列表生成方式：**

* 直接给出列表 
* 整数列表：

```
{start..end}
$(seq [start [step]] end)
```

* 返回列表的命令:

```
$(COMMAND)
```

* 使用glob，如：*.sh *
* *变量引用，如：$@，$*，$#

**格式2**

双小括号方法，即((…))格式，也可以用于算术运算，双小括号方法也可以使bash Shell实现C语言风格

的变量操作

I=10;((I++)) 

![](C:\Users\Administrator\Desktop\SRE课程资料\SRE笔记图片\6.4.2.2循环for.jpg)

```bash
for ((: for (( exp1; exp2; exp3 )); do COMMANDS; done
for ((控制变量初始化;条件判断表达式;控制变量的修正表达式))
do
 循环体
done
```

说明：

* 控制变量初始化：仅在运行到循环代码段时执行一次

* 控制变量的修正表达式：每轮循环结束会先进行控制变量修正运算，而后再做条件判断

**练习：用 for 实现**

1、判断/var/目录下所有文件的类型

2、添加10个用户user1-user10，密码为8位随机字符

3、/etc/rc.d/rc3.d目录下分别有多个以K开头和以S开头的文件；分别读取每个文件，以K开头的输出为文件加stop，以S开头的输出为文件名加start，如K34filename stop S66filename start

4、编写脚本，提示输入正整数n的值，计算1+2+…+n的总和

5、计算100以内所有能被3整除的整数之和

6、编写脚本，提示请输入网络地址，如192.168.0.0，判断输入的网段中主机在线状态

7、打印九九乘法表

8、在/testdir目录下创建10个html文件,文件名格式为数字N（从1到10）加随机8个字母，如：1AbCdeFgH.html

9、打印等腰三角形10、猴子第一天摘下若干个桃子，当即吃了一半，还不瘾，又多吃了一个。第二天早上又将剩下的桃子吃掉一半，又多吃了一个。以后每天早上都吃了前一天剩下的一半零一个。到第10天早上想再吃时，只剩下一个桃子了。求第一天共摘了多少？

##### 6.4.2.3 循环 while

格式：

```bash
while COMMANDS; do COMMANDS; done
while CONDITION; do
 循环体
done
```

说明：

CONDITION：循环控制条件；进入循环之前，先做一次判断；每一次循环之后会再次做判断；条件为“true”，则执行一次循环；直到条件测试状态为“false”终止循环，因此：CONDTION一般应该有循环控制变量；而此变量的值会在循环体不断地被修正

进入条件：CONDITION为 true

退出条件：CONDITION为 false

**无限循环**

```bash
while true; do
 循环体
done
while : ; do
 循环体
done
```

**练习：用while实现**

1、编写脚本，求100以内所有正奇数之和

2、编写脚本，提示请输入网络地址，如:192.168.0.0，判断输入的网段中主机在线状态，并统计在线和离线主机各多少

3、编写脚本，打印九九乘法表

4、编写脚本，利用变量RANDOM生成10个随机数字，输出这个10数字，并显示其中的最大值和最小值

5、编写脚本，实现打印国际象棋棋盘6、后续六个字符串：efbaf275cd、4be9c40b8b、44b2395c46、f8c8873ce0、b902c16c8b、ad865d2f63是通过对随机数变量RANDOM随机执行命令： echo $RANDOM|md5sum|cut -c1-10 后的结果，请破解这些字符串对应的RANDOM值

##### 6.4.2.4 循环 until

格式：

```bash
until COMMANDS; do COMMANDS; done
until CONDITION; do
 循环体
done
```

说明：

进入条件： CONDITION 为false

退出条件： CONDITION 为true

**无限循环**

```bash
until false; do
 循环体
Done
```

##### 6.4.2.5 循环控制语句 continue

continue [N]：提前结束第N层的本轮循环，而直接进入下一轮判断；最内层为第1层

格式：

```bash
while CONDITION1; do
 CMD1
 ...
 if CONDITION2; then
 continue
 fi
 CMDn
 ...
done
```

##### 6.4.2.6 循环控制语句 break

break [N]：提前结束第N层整个循环，最内层为第1层

格式：

```bash
while CONDITION1; do
 CMD1
 ...
 if CONDITION2; then
 break
 fi
 CMDn
 ...
done
```

##### 6.4.2.7 循环控制 shift 命令

shift [n] 用于将参量列表 list 左移指定次数，缺省为左移一次。

参量列表 list 一旦被移动，最左端的那个参数就从列表中删除。while 循环遍历位置参量列表时，常用到 shift

**练习**

1、每隔3秒钟到系统上获取已经登录的用户的信息；如果发现用户hacker登录，则将登录时间和主机记录于日志/var/log/login.log中,并退出脚本

2、随机生成10以内的数字，实现猜字游戏，提示比较大或小，相等则退出

3、用文件名做为参数，统计所有参数文件的总行数

4、用二个以上的数字为参数，显示其中的最大值和最小值

##### 6.4.2.8 while 特殊用法 while read

while 循环的特殊用法，遍历文件或文本的每一行

格式：

```bash
while read line; do
 循环体
done < /PATH/FROM/SOMEFILE
```

说明：依次读取/PATH/FROM/SOMEFILE文件中的每一行，且将行赋值给变量line

##### 6.4.2.9 循环与菜单 select

格式：

```bash
select NAME [in WORDS ... ;] do COMMANDS; done
select variable in list ;do 
 循环体命令
done
```

说明：

* select 循环主要用于创建菜单，按数字顺序排列的菜单项显示在标准错误上，并显示 PS3 提示符，等待用输入

* 用户输入菜单列表中的某个数字，执行相应的命令

* 用户输入被保存在内置变量 REPLY 中

* select 是个无限循环，因此要用 break 命令退出循环，或用 exit 命令终止脚本。也可以按 ctrl+c 退出循环

* select 经常和 case 联合使用

* 与 for 循环类似，可以省略 in list，此时使用位置参量

### 6.5 函数 function

#### 6.5.1 函数介绍

函数function是由若干条shell命令组成的语句块，实现代码重用和模块化编程

它与shell程序形式上是相似的，不同的是它不是一个单独的进程，不能独立运行，而是shell程序的一部分

**函数和shell程序区别**

* Shell程序在子Shell中运行

* 函数在当前Shell中运行。因此在当前Shell中，函数可对shell中变量进行修改

#### 6.5.2 管理函数

函数由两部分组成：函数名和函数体

帮助参看：help function

##### 6.5.2.1 定义函数

```bash
#语法一：
func_name （）{
 ...函数体...
}#语法二：
function func_name {
 ...函数体...
} 
#语法三：
function func_name （） {
 ...函数体...
}
```

##### 6.5.2.2 查看函数

```bash
#查看当前已定义的函数名
declare -F
#查看当前已定义的函数定义
declare -f
#查看指定当前已定义的函数名
declare -f func_name 
#查看当前已定义的函数名定义
declare -F func_name
```

##### 6.5.2.3 删除函数

格式：

```bash
unset func_name
```

#### 6.5.3 函数调用

函数的调用方式

* 可在交互式环境下定义函数

* 可将函数放在脚本文件中作为它的一部分

* 可放在只包含函数的单独文件中

调用：函数只有被调用才会执行，通过给定函数名调用函数，函数名出现的地方，会被自动替换为函数代码

函数的生命周期：被调用时创建，返回时终止

##### 6.5.3.1 交互式环境调用函数

交互式环境下定义和使用函数

##### 6.5.3.2 在脚本中定义及使用函数

函数在使用前必须定义，因此应将函数定义放在脚本开始部分，直至shell首次发现它后才能使用，调用函数仅使用其函数名即可

##### 6.5.3.3 使用函数文件

可以将经常使用的函数存入一个单独的函数文件，然后将函数文件载入shell，再进行调用函数

函数文件名可任意选取，但最好与相关任务有某种联系，例如：functions

一旦函数文件载入shell，就可以在命令行或脚本中调用函数。可以使用delcare -f 或set 命令查看所有定义的函数，其输出列表包括已经载入shell的所有函数

若要改动函数，首先用unset命令从shell中删除函数。改动完毕后，再重新载入此文件

实现函数文件的过程：

​	1. 创建函数文件，只存放函数的定义

​	2. 在shell脚本或交互式shell中调用函数文件，格式如下：

```bash
. filename 
source   filename  
```

#### 6.5.4 函数返回值

函数的执行结果返回值：

* 使用echo等命令进行输出

* 函数体中调用命令的输出结果

函数的退出状态码：

* 默认取决于函数中执行的最后一条命令的退出状态码

* 自定义退出状态码，其格式为：

​		return 从函数中返回，用最后状态命令决定返回值

​		return 0 无错误返回

​		return 1-255 有错误返回

#### 6.5.5 环境函数

类拟于环境变量，也可以定义环境函数，使子进程也可使用父进程定义的函数

定义环境函数：

```bash
export -f function_name 
declare -xf function_name
```

查看环境函数：

```bash
export -f 
declare -xf
```

#### 6.5.6 函数参数

函数可以接受参数：

* 传递参数给函数：在函数名后面以空白分隔给定参数列表即可，如：testfunc arg1 arg2 ...

* 在函数体中当中，可使用$1, $2, ...调用这些参数；还可以使用$@, $*, $#等特殊变量

#### 6.5.7 函数变量

变量作用域：

* 普通变量：只在当前shell进程有效，为执行脚本会启动专用子shell进程；因此，本地变量的作用范围是当前shell脚本程序文件，包括脚本中的函数

* 环境变量：当前shell和子shell有效

* 本地变量：函数的生命周期；函数结束时变量被自动销毁

注意：

* 如果函数中定义了普通变量，且名称和局部变量相同，则使用本地变量

* 由于普通变量和局部变量会冲突，建议在函数中只使用本地变量

在函数中定义本地变量的方法

```bash
local NAME=VALUE
```

#### 6.5.8 函数递归

函数递归：函数直接或间接调用自身，注意递归层数，可能会陷入死循环

递归特点: 

* 函数内部自已调用自已

* 必须有结束函数的出口语句,防止死循环

**递归示例：**

阶乘是基斯顿·卡曼于 1808 年发明的运算符号，是数学术语

一个正整数的阶乘（factorial）是所有小于及等于该数的正整数的积，并且0和1的阶乘为1，自然数n的

阶乘写作n!

n!=1×2×3×...×n

阶乘亦可以递归方式定义：0!=1，n!=(n-1)!×n

n!=n(n-1)(n-2)...1

n(n-1)! = n(n-1)(n-2)!

**练习**

1. 编写函数，实现OS的版本判断

2. 编写函数，实现取出当前系统eth0的IP地址

3. 编写函数，实现打印绿色OK和红色FAILED

4. 编写函数，实现判断是否无位置参数，如无参数，提示错误

5. 编写函数，实现两个数字做为参数，返回最大值

6. 编写服务脚本/root/bin/testsrv.sh，完成如下要求

​		(1) 脚本可接受参数：start, stop, restart, status 

​		(2) 如果参数非此四者之一，提示使用格式后报错退出

​		(3) 如是start:则创建/var/lock/subsys/SCRIPT_NAME, 并显示“启动成功”

​			考虑：如果事先已经启动过一次，该如何处理？

​		(4) 如是stop:则删除/var/lock/subsys/SCRIPT_NAME, 并显示“停止完成”

​			考虑：如果事先已然停止过了，该如何处理？

​		(5) 如是restart，则先stop, 再start

​			考虑：如果本来没有start，如何处理？

​		(6) 如是status, 则如果/var/lock/subsys/SCRIPT_NAME文件存在，则显示“SCRIPT_NAME is running...”，如果/var/lock/subsys/SCRIPT_NAME文件不存在，则显示“SCRIPT_NAME is stopped...”

​		(7)在所有模式下禁止启动该服务，可用chkconfig 和 service命令管理

​			说明：SCRIPT_NAME为当前脚本名

7. 编写脚本/root/bin/copycmd.sh

​		(1) 提示用户输入一个可执行命令名称

​		(2) 获取此命令所依赖到的所有库文件列表

​		(3) 复制命令至某目标目录(例如/mnt/sysroot)下的对应路径下

​			 如：/bin/bash ==> /mnt/sysroot/bin/bash

 			/usr/bin/passwd ==> /mnt/sysroot/usr/bin/passwd

​		(4) 复制此命令依赖到的所有库文件至目标目录下的对应路径下： 如：/lib64/ld-linux-x86-64.so.2 ==> /mnt/sysroot/lib64/ld-linux-x86-64.so.2

​		(5)每次复制完成一个命令后，不要退出，而是提示用户键入新的要复制的命令，并重复完成上述功能；直到用户输入quit退出

8. 斐波那契数列又称黄金分割数列，因数学家列昂纳多·斐波那契以兔子繁殖为例子而引入，故又称为“兔子数列”，指的是这样一个数列：0、1、1、2、3、5、8、13、21、34、……，斐波纳契数列以如下被以递归的方法定义：F（0）=0，F（1）=1，F（n）=F(n-1)+F(n-2)（n≥2），利用函数，求n阶斐波那契数列

9. 汉诺塔（又称河内塔）问题是源于印度一个古老传说。大梵天创造世界的时候做了三根金刚石柱子，在一根柱子上从下往上按照大小顺序摞着64片黄金圆盘。大梵天命令婆罗门把圆盘从下面开始按大小顺序重新摆放在另一根柱子上。并且规定，在小圆盘上不能放大圆盘，在三根柱子之间一次只能移动一个圆盘，利用函数，实现N片盘的汉诺塔的移动步骤

### 6.6 其它脚本相关工具

#### 6.6.1 信号捕捉 trap

trap 命令可以捕捉信号,修改信号原来的功能,实现自定义功能

```bash
#进程收到系统发出的指定信号后，将执行自定义指令，而不会执行原操作
trap '触发指令' 信号
#忽略信号的操作
trap '' 信号
 
#恢复原信号的操作
trap '-' 信号
#列出自定义信号操作
trap -p
#当脚本退出时，执行finish函数
trap finish EXIT
```

#### 6.6.2 创建临时文件 mktemp

mktemp 命令用于创建并显示临时文件，可避免冲突

格式：

```
mktemp [OPTION]... [TEMPLATE]
说明：TEMPLATE: filenameXXX，X至少要出现三个
```

常见选项：

```
-d #创建临时目录
-p DIR或--tmpdir=DIR   #指明临时文件所存放目录位置
```

#### 6.6.3 安装复制文件 install

install 功能相当于cp，chmod，chown，chgrp ,mkdir 等相关工具的集合

install命令格式：

```bash
install [OPTION]... [-T] SOURCE DEST 单文件
install [OPTION]... SOURCE... DIRECTORY
install [OPTION]... -t DIRECTORY SOURCE...
install [OPTION]... -d DIRECTORY... #创建空目录
```

选项：

```
-m MODE，默认755
-o OWNER
-g GROUP
-d DIRNAME 目录
```

#### 6.6.4 交互式转化批处理工具 expect

expect 是由Don Libes基于 Tcl（ Tool Command Language ）语言开发的，主要应用于自动化交互式操作的场景，借助 expect 处理交互的命令，可以将交互过程如：ssh登录，ftp登录等写在一个脚本上，使之自动化完成。尤其适用于需要对多台服务器执行相同操作的环境中，可以大大提高系统管理人员的工作效率

expect 语法：

```
expect [选项] [ -c cmds ] [ [ -[f|b] ] cmdfile ] [ args ]
```

常见选项：

```
-c：从命令行执行expect脚本，默认expect是交互地执行的
-d：可以调试信息
```

示例：

```
expect  -c 'expect "\n" {send "pressed enter\n"}'
expect  -d ssh.exp
```

expect中相关命令

* spawn 启动新的进程

* expect 从进程接收字符串

* send 用于向进程发送字符串

* interact 允许用户交互

* exp_continue 匹配多个字符串在执行动作后加此命令

expect最常用的语法(tcl语言:模式-动作)

### 6.7 数组 array

#### 6.7.1 数组介绍

变量：存储单个元素的内存空间

数组：存储多个元素的连续的内存空间，相当于多个变量的集合

数组名和索引

* 索引的编号从0开始，属于数值索引

* 索引可支持使用自定义的格式，而不仅是数值格式，即为**关联索引**，bash 4.0版本之后开始支持

* bash的数组支持稀疏格式（索引不连续）

#### 6.7.2 声明数组

```bash
#普通数组可以不事先声明,直接使用
declare -a ARRAY_NAME
#关联数组必须先声明,再使用
declare -A ARRAY_NAME
```

注意：两者不可相互转换

#### 6.7.3 数组赋值

数组元素的赋值

(1) 一次只赋值一个元素

```
ARRAY_NAME[INDEX]=VALUE
```

(2) 一次赋值全部元素

```
ARRAY_NAME=("VAL1" "VAL2" "VAL3" ...)
```

(3) 只赋值特定元素

```
ARRAY_NAME=([0]="VAL1" [3]="VAL2" ...)
```

(4) 交互式数组值对赋值

```
read -a ARRAY
```

#### 6.7.4 显示所有数组

显示所有数组：

```bash
declare -a
```

#### 6.7.5 引用数组

引用特定的数组元素

```
${ARRAY_NAME[INDEX]}
#如果省略[INDEX]表示引用下标为0的元素
```

引用数组所有元素

```
${ARRAY_NAME[*]}
${ARRAY_NAME[@]}
```

数组的长度，即数组中元素的个数

```
${#ARRAY_NAME[*]}
${#ARRAY_NAME[@]}
```

#### 6.7.6 删除数组

删除数组中的某元素，会导致稀疏格式

```
unset ARRAY[INDEX]
```

删除整个数组

```
unset ARRAY
```

#### 6.7.7 数组数据处理

数组切片：

```
${ARRAY[@]:offset:number}
offset #要跳过的元素个数
number #要取出的元素个数
#取偏移量之后的所有元素 
{ARRAY[@]:offset}
```

向数组中追加元素：

```
ARRAY[${#ARRAY[*]}]=value
ARRAY[${#ARRAY[@]}]=value
```

#### 6.7.8 关联数组

```
declare -A ARRAY_NAME 
ARRAY_NAME=([idx_name1]='val1' [idx_name2]='val2‘...)
```

注意：关联数组必须先声明再调用

#### 6.7.9 范例

### 6.8 字符串处理

#### 6.8.1 字符串切片

##### 6.8.1.1 基于偏移量取字符串

```bash
#返回字符串变量var的字符的长度,一个汉字算一个字符
${#var} 
#返回字符串变量var中从第offset个字符后（不包括第offset个字符）的字符开始，到最后的部分，
offset的取值在0 到 ${#var}-1 之间(bash4.2后，允许为负值)
${var:offset} 
#返回字符串变量var中从第offset个字符后（不包括第offset个字符）的字符开始，长度为number的部分
${var:offset:number}
#取字符串的最右侧几个字符,取字符串的最右侧几个字符, 注意：冒号后必须有一空白字符
${var: -length}
#从最左侧跳过offset字符，一直向右取到距离最右侧lengh个字符之前的内容,即:掐头去尾
${var:offset:-length}
#先从最右侧向左取到length个字符开始，再向右取到距离最右侧offset个字符之间的内容,注意：-
length前空格,并且length必须大于offset
${var: -length:-offset}
```

##### 6.8.1.2 基于模式取子串

```
#其中word可以是指定的任意字符,自左而右，查找var变量所存储的字符串中，第一次出现的word, 删除字
符串开头至第一次出现word字符串（含）之间的所有字符,即懒惰模式,以第一个word为界删左留右
${var#*word}
#同上，贪婪模式，不同的是，删除的是字符串开头至最后一次由word指定的字符之间的所有内容,即贪婪模
式,以最后一个word为界删左留右
${var##*word}
```

```
#其中word可以是指定的任意字符,功能：自右而左，查找var变量所存储的字符串中，第一次出现的word, 
删除字符串最后一个字符向左至第一次出现word字符串（含）之间的所有字符,即懒惰模式,以从右向左的第
一个word为界删右留左
${var%word*}
#同上，只不过删除字符串最右侧的字符向左至最后一次出现word字符之间的所有字符,即贪婪模式,以从右向
左的最后一个word为界删右留左
${var%%word*}
```

#### 6.8.2 查找替换

```
#查找var所表示的字符串中，第一次被pattern所匹配到的字符串，以substr替换之
${var/pattern/substr}
#查找var所表示的字符串中，所有能被pattern所匹配到的字符串，以substr替换之
${var//pattern/substr}
#查找var所表示的字符串中，行首被pattern所匹配到的字符串，以substr替换之
${var/#pattern/substr}
#查找var所表示的字符串中，行尾被pattern所匹配到的字符串，以substr替换之
${var/%pattern/substr}
```

#### 6.8.3 查找并删除

```
#删除var表示的字符串中第一次被pattern匹配到的字符串
${var/pattern}
删除var表示的字符串中所有被pattern匹配到的字符串
${var//pattern}
删除var表示的字符串中所有以pattern为行首匹配到的字符串
${var/#pattern}
删除var所表示的字符串中所有以pattern为行尾所匹配到的字符串
${var/%pattern}
```

#### 6.8.4 字符大小写转换

```
#把var中的所有小写字母转换为大写
${var^^}
#把var中的所有大写字母转换为小写
${var,,}
```

### 6.9 高级变量

#### 6.9.1 高级变量赋值

#### 6.9.2 高级变量用法-有类型变量

Shell变量一般是无类型的，但是bash Shell提供了declare和typeset两个命令用于指定变量的类型，两个命令是等价的

```
declare [选项] 变量名
选项：
-r 声明或显示只读变量
-i 将变量定义为整型数
-a 将变量定义为数组
-A 将变量定义为关联数组
-f 显示已定义的所有函数名及其内容
-F 仅显示已定义的所有函数名
-x 声明或显示环境变量和函数,相当于export 
-l 声明变量为小写字母 declare -l var=UPPER
-u 声明变量为大写字母 declare -u var=lower
-n  make NAME a reference to the variable named by its value
```

#### 6.9.3 变量间接引用

##### 6.9.3.1 eval命令

eval命令将会首先扫描命令行进行所有的置换，然后再执行该命令。该命令适用于那些一次扫描无法实现其功能的变量,该命令对变量进行两次扫描

##### 6.9.3.2 间接变量引用

如果第一个变量的值是第二个变量的名字，从第一个变量引用第二个变量的值就称为间接变量引用variable1的值是variable2，而variable2又是变量名，variable2的值为value，间接变量引用是指通过variable1获得变量value的行为

```
variable1=variable2
variable2=value
#示例: i=1
$1=wang
```

bash Shell提供了两种格式实现间接变量引用

```
#方法1 #变量赋值
eval tempvar=\$$variable1
#显示值
eval echo \$$variable1
eval echo '$'$variable1
#方法2 #变量赋值
tempvar=${!variable1}
#显示值
echo ${!variable1}
```

##### 6.9.3.3 变量引用 reference

# 7.文件查找和打包压缩

### 内容概述

* locate

* find

* xargs

* compress和uncompress

* gzip和gunzip

* bzip2和bunzip2

* xz和unxz

* zip和unzip

* tar

* cpio

### 7.1 文件查找

在文件系统上查找符合条件的文件

文件查找：

* 非实时查找(数据库查找)：locate

* 实时查找：find

#### 7.1.1 locate

* locate 查询系统上预建的文件索引数据库 /var/lib/mlocate/mlocate.db

* 索引的构建是在系统较为空闲时自动进行(周期性任务)，执行updatedb可以更新数据库

* 索引构建过程需要遍历整个根文件系统，很消耗资源

* locate和updatedb命令来自于mlocate包

**工作特点:**

* 查找速度快

* 模糊查找

* 非实时查找

* 搜索的是文件的全路径，不仅仅是文件名

* 可能只搜索用户具备读取和执行权限的目录

格式：

```
locate [OPTION]... [PATTERN]...
```

常用选项：

```
-i 不区分大小写的搜索
-n N 只列举前N个匹配项目
-r 使用基本正则表达式
```

范例：

```bash
[root@sh-lzy-Centos8 23:18 ~]#locate -r '\.log$' -n 10
/data/.log
/data/2021-12-31-20:19:53.log
/data/bc.log
/home/Lzy/.cache/mozilla/firefox/o3wafguc.default-release/cache2/index.log
/home/Lzy/.local/share/gvfs-metadata/home-895a8e8f.log
/home/Lzy/.local/share/gvfs-metadata/root-7dc209d3.log
/usr/lib/rpm/rpm.log
/var/log/Xorg.9.log
/var/log/boot.log
/var/log/dnf.librepo.log
[root@sh-lzy-Centos8 23:19 ~]#
```

#### 7.1.2 find

find 是实时查找工具，通过遍历指定路径完成文件查找

工作特点：

* 查找速度略慢

* 精确查找

* 实时查找

* 查找条件丰富

* 可能只搜索用户具备读取和执行权限的目录

格式：

```
find [OPTION]... [查找路径] [查找条件] [处理动作]
```

查找路径：指定具体目标路径；默认为当前目录

查找条件：指定的查找标准，可以文件名、大小、类型、权限等标准进行；默认为找出指定路径下的所有文件

处理动作：对符合条件的文件做操作，默认输出至屏幕

##### 7.1.2.1 指定搜索目录层级

```
-maxdepth level 最大搜索目录深度,指定目录下的文件为第1级
-mindepth level 最小搜索目录深度
```

##### 7.1.2.2 对每个目录先处理目录内的文件，再处理目录本身

```
-depth  
-d #warning: the -d option is deprecated; please use -depth instead, because the 
latter is a POSIX-compliant feature
```

##### 7.1.2.3 根据文件名和inode查找

```
-name "文件名称" #支持使用glob，如：*, ?, [], [^],通配符要加双引号引起来
-iname "文件名称"  #不区分字母大小写
-inum n #按inode号查找
-samefile name #相同inode号的文件
-links n   #链接数为n的文件
-regex “PATTERN”    #以PATTERN匹配整个文件路径，而非文件名称
```

##### 7.1.2.4 根据属主、属组查找

```
-user USERNAME #查找属主为指定用户(UID)的文件
-group GRPNAME #查找属组为指定组(GID)的文件
-uid UserID #查找属主为指定的UID号的文件
-gid GroupID #查找属组为指定的GID号的文件
-nouser #查找没有属主的文件
-nogroup #查找没有属组的文件
```

##### 7.1.2.5 根据文件类型查找

```
-type TYPE
TYPE可以是以下形式：
f: 普通文件
d: 目录文件
l: 符号链接文件
s：套接字文件
b: 块设备文件
c: 字符设备文件
p: 管道文件
```

##### 7.1.2.6 空文件或目录

```
-empty
```

##### 7.1.2.7 组合条件

```
与：-a ，默认多个条件是与关系
或：-o
非：-not   !
```

德·摩根定律：

* (非 A) 或 (非 B) = 非(A 且 B) 

* (非 A) 且 (非 B) = 非(A 或 B) 

示例：

```
!A -a !B = !(A -o B)
!A -o !B = !(A -a B)
```

##### 7.1.2.8 排除目录

```
范例：
#查找/etc/下，除/etc/security目录的其它所有.conf后缀的文件
find /etc -path '/etc/security' -a -prune -o -name "*.conf"
#查找/etc/下，除/etc/security和/etc/systemd,/etc/dbus-1三个目录的所有.conf后缀的文件
find /etc \( -path "/etc/security" -o -path "/etc/systemd"  -o -path "/etc/dbus-
1" \) -a -prune -o -name "*.conf"
#排除/proc和/sys目录
find / \( -path "/sys" -o -path "/proc" \) -a -prune -o -type f -a -mmin -1
```

##### 7.1.2.9 根据文件大小来查找

```
-size [+|-]#UNIT #常用单位：k, M, G，c（byte）,注意大小写敏感
#UNIT: #表示(#-1, #],如：6k 表示(5k,6k]
-#UNIT #表示[0,#-1],如：-6k 表示[0,5k]
+#UNIT #表示(#,∞),如：+6k 表示(6k,∞)
```

##### 7.1.2.10 根据时间戳

```
#以“天”为单位
-atime [+|-]# 
# #表示[#,#+1)
+# #表示[#+1,∞]
-# #表示[0,#)
-mtime
-ctime
#以“分钟”为单位
-amin
-mmin
-cmin
```

##### 7.1.2.11 根据权限查找

```
-perm [/|-]MODE
MODE  #精确权限匹配
/MODE #任何一类(u,g,o)对象的权限中只要能一位匹配即可，或关系，+ 从CentOS 7开始淘汰
-MODE #每一类对象都必须同时拥有指定权限，与关系
0 表示不关注
```

说明：

find -perm 755 会匹配权限模式恰好是755的文件

只要当任意人有写权限时，find -perm /222就会匹配

只有当每个人都有写权限时，find -perm -222才会匹配

只有当其它人（other）有写权限时，find -perm -002才会匹配

##### 7.1.2.12 正则表达式

##### 7.1.2.13 处理动作

```
-print：默认的处理动作，显示至屏幕
-ls：类似于对查找到的文件执行"ls -dils"命令格式输出
-fls file：查找到的所有文件的长格式信息保存至指定文件中，相当于 -ls > file
-delete：删除查找到的文件，慎用！
-ok COMMAND {} \; 对查找到的每个文件执行由COMMAND指定的命令，对于每个文件执行命令之前，都会
交互式要求用户确认
-exec COMMAND {} \; 对查找到的每个文件执行由COMMAND指定的命令
{}: 用于引用查找到的文件名称自身
```

#### 7.1.3 参数替换 xargs

由于很多命令不支持管道|来传递参数，xargs用于产生某个命令的参数，xargs 可以读入 stdin 的数据，并且以空格符或回车符将 stdin 的数据分隔成为参数

另外，许多命令不能接受过多参数，命令执行可能会失败，xargs 可以解决

注意：文件名或者是其他意义的名词内含有空格符的情况

find 经常和 xargs 命令进行组合,形式如下：

```bash
find | xargs COMMAND
```

**练习：**

1、查找/var目录下属主为root，且属组为mail的所有文件

2、查找/var目录下不属于root、lp、gdm的所有文件

3、查找/var目录下最近一周内其内容修改过，同时属主不为root，也不是postfix的文件

4、查找当前系统上没有属主或属组，且最近一个周内曾被访问过的文件

5、查找/etc目录下大于1M且类型为普通文件的所有文件

6、查找/etc目录下所有用户都没有写权限的文件

7、查找/etc目录下至少有一类用户没有执行权限的文件

8、查找/etc/init.d目录下，所有用户都有执行权限，且其它用户有写权限的文件

### 7.2 压缩和解压缩

主要针对单个文件压缩,而非目录

#### 7.2.1 compress 和 uncompress

此工具来自于ncompress包,此工具目前已经很少使用

对应的文件是 .Z 后缀

格式：

```
compress Options [file ...]
uncompress file.Z #解压缩
```

常用选项

```
-d 解压缩，相当于uncompress
-c 结果输出至标准输出,不删除原文件
-v 显示详情
```

zcat file.Z 不显式解压缩的前提下查看文本文件内容

#### 7.2.2 gzip和gunzip

来自于 gzip 包

对应的文件是 .gz 后缀

```
gzip [OPTION]... FILE ...
```

常用选项：

```
-k keep, 保留原文件,CentOS 8 新特性
-d 解压缩，相当于gunzip
-c 结果输出至标准输出，保留原文件不改变
-# 指定压缩比，#取值为1-9，值越大压缩比越大
```

#### 7.2.3 bzip2和bunzip2

来自于 bzip2 包

对应的文件是 .bz2 后缀

```
bzip2 [OPTION]... FILE ...
```

常用选项

```
-k keep, 保留原文件
-d 解压缩
-c 结果输出至标准输出，保留原文件不改变
-# 1-9，压缩比，默认为9
```

#### 7.2.4 xz 和 unxz

来自于 xz 包

对应的文件是 .xz 后缀

```bash
xz [OPTION]... FILE ...
```

常用选项

```
-k keep, 保留原文件
-d 解压缩
-c 结果输出至标准输出，保留原文件不改变
-# 压缩比，取值1-9，默认为6
```

#### 7.2.5 zip 和 unzip

zip 可以实现打包目录和多个文件成一个文件并压缩，但可能会丢失文件属性信息，如：所有者和组信息，一般建议使用 tar 代替

分别来自于 zip 和 unzip 包

对应的文件是 .zip 后缀

范例：

```bash
#打包并压缩
zip  -r /backup/sysconfig.zip /etc/sysconfig/
#不包括目录本身，只打包目录内的文件和子目录
cd /etc/sysconfig; zip -r /root/sysconfig.zip * 
#默认解压缩至当前目录
unzip /backup/sysconfig.zip  
#解压缩至指定目录,如果指定目录不存在，会在其父目录（必须事先存在）下自动生成
unzip /backup/sysconfig.zip  -d /tmp/config  
cat /var/log/messages | zip messages  -
#-p 表示管道
unzip -p message.zip   > message
```

### 7.3 打包和解包

#### 7.3.1 tar

tar 即 Tape ARchive 磁带归档，可以对目录和多个文件打包一个文件，并且可以压缩，保留文件属性不丢失，常用于备份功能，推荐使用

对应的文件是 .tar 后缀

格式：

```bash
tar [-ABcdgGhiklmMoOpPrRsStuUvwWxzZ][-b <区块数目>][-C <目的目录>][-f <备份文件>][-F 
<Script文件>][-K <文件>][-L <媒体容量>][-N <日期时间>][-T <范本文件>][-V <卷册名称>][-X 
<范本文件>][-<设备编号><存储密度>][--after-date=<日期时间>][--atime-preserve][--
backuup=<备份方式>][--checkpoint][--concatenate][--confirmation][--delete][--
exclude=<范本样式>][--force-local][--group=<群组名称>][--help][--ignore-failedread][--new-volume-script=<Script文件>][--newer-mtime][--no-recursion][--null][--
numeric-owner][--owner=<用户名称>][--posix][--erve][--preserve-order][--preservepermissions][--record-size=<区块数目>][--recursive-unlink][--remove-files][--rshcommand=<执行指令>][--same-owner][--suffix=<备份字尾字符串>][--totals][--usecompress-program=<执行指令>][--version][--volno-file=<编号文件>][文件或目录...]
```

选项：

```bash
-A或--catenate 新增文件到已存在的备份文件。
-b<区块数目>或--blocking-factor=<区块数目> 设置每笔记录的区块数目，每个区块大小为12Bytes。
-B或--read-full-records 读取数据时重设区块大小。
-c或--create 建立新的备份文件。
-C<目的目录>或--directory=<目的目录> 切换到指定的目录。
-d或--diff或--compare 对比备份文件内和文件系统上的文件的差异。
-f<备份文件>或--file=<备份文件> 指定备份文件。
-F<Script文件>或--info-script=<Script文件> 每次更换磁带时，就执行指定的Script文件。
-g或--listed-incremental 处理GNU格式的大量备份。
-G或--incremental 处理旧的GNU格式的大量备份。
-h或--dereference 不建立符号连接，直接复制该连接所指向的原始文件。
-i或--ignore-zeros 忽略备份文件中的0 Byte区块，也就是EOF。
-k或--keep-old-files 解开备份文件时，不覆盖已有的文件。
-K<文件>或--starting-file=<文件> 从指定的文件开始还原。
-l或--one-file-system 复制的文件或目录存放的文件系统，必须与tar指令执行时所处的文件系统相
同，否则不予复制。
-L<媒体容量>或-tape-length=<媒体容量> 设置存放每体的容量，单位以1024 Bytes计算。
-m或--modification-time 还原文件时，不变更文件的更改时间。
-M或--multi-volume 在建立，还原备份文件或列出其中的内容时，采用多卷册模式。
-N<日期格式>或--newer=<日期时间> 只将较指定日期更新的文件保存到备份文件里。
-o或--old-archive或--portability 将资料写入备份文件时使用V7格式。
-O或--stdout 把从备份文件里还原的文件输出到标准输出设备。
-p或--same-permissions 用原来的文件权限还原文件。
-P或--absolute-names 文件名使用绝对名称，不移除文件名称前的"/"号。
-r或--append 新增文件到已存在的备份文件的结尾部分。
-R或--block-number 列出每个信息在备份文件中的区块编号。
-s或--same-order 还原文件的顺序和备份文件内的存放顺序相同。
-S或--sparse 倘若一个文件内含大量的连续0字节，则将此文件存成稀疏文件。
-t或--list 列出备份文件的内容。
-T<范本文件>或--files-from=<范本文件> 指定范本文件，其内含有一个或多个范本样式，让tar解开或
建立符合设置条件的文件。
-u或--update 仅置换较备份文件内的文件更新的文件。
-U或--unlink-first 解开压缩文件还原文件之前，先解除文件的连接。
-v或--verbose 显示指令执行过程。
-V<卷册名称>或--label=<卷册名称> 建立使用指定的卷册名称的备份文件。
-w或--interactive 遭遇问题时先询问用户。
-W或--verify 写入备份文件后，确认文件正确无误。
-x或--extract或--get 从备份文件中还原文件。
-X<范本文件>或--exclude-from=<范本文件> 指定范本文件，其内含有一个或多个范本样式，让ar排除
符合设置条件的文件。
-z或--gzip或--ungzip 通过gzip指令处理备份文件。
-Z或--compress或--uncompress 通过compress指令处理备份文件。
-<设备编号><存储密度> 设置备份用的外围设备编号及存放数据的密度。
--after-date=<日期时间> 此参数的效果和指定"-N"参数相同。
--atime-preserve 不变更文件的存取时间。
--backup=<备份方式>或--backup 移除文件前先进行备份。
--checkpoint 读取备份文件时列出目录名称。
--concatenate 此参数的效果和指定"-A"参数相同。
--confirmation 此参数的效果和指定"-w"参数相同。
--delete 从备份文件中删除指定的文件。
--exclude=<范本样式> 排除符合范本样式的文件。
--group=<群组名称> 把加入设备文件中的文件的所属群组设成指定的群组。
--help 在线帮助。
--ignore-failed-read 忽略数据读取错误，不中断程序的执行。
--new-volume-script=<Script文件> 此参数的效果和指定"-F"参数相同。
--newer-mtime 只保存更改过的文件。
--no-recursion 不做递归处理，也就是指定目录下的所有文件及子目录不予处理。
--null 从null设备读取文件名称。
--numeric-owner 以用户识别码及群组识别码取代用户名称和群组名称。
--owner=<用户名称> 把加入备份文件中的文件的拥有者设成指定的用户。
--posix 将数据写入备份文件时使用POSIX格式。
--preserve 此参数的效果和指定"-ps"参数相同。
--preserve-order 此参数的效果和指定"-A"参数相同。
--preserve-permissions 此参数的效果和指定"-p"参数相同。
--record-size=<区块数目> 此参数的效果和指定"-b"参数相同。
--recursive-unlink 解开压缩文件还原目录之前，先解除整个目录下所有文件的连接。
--remove-files 文件加入备份文件后，就将其删除。
--rsh-command=<执行指令> 设置要在远端主机上执行的指令，以取代rsh指令。
--same-owner 尝试以相同的文件拥有者还原文件。
--suffix=<备份字尾字符串> 移除文件前先行备份。
--totals 备份文件建立后，列出文件大小。
--use-compress-program=<执行指令> 通过指定的指令处理备份文件。
--version 显示版本信息。
--volno-file=<编号文件> 使用指定文件内的编号取代预设的卷册编号。
```

(1) 创建归档，保留权限

```bash
tar -cpvf /PATH/FILE.tar FILE...
```

(2) 追加文件至归档： 注：不支持对压缩文件追加

```bash
tar -rf /PATH/FILE.tar FILE...
```

(3) 查看归档文件中的文件列表

```bash
tar -t -f /PATH/FILE.tar
```

(4) 展开归档

```bash
tar xf /PATH/FILE.tar
tar xf /PATH/FILE.tar -C /PATH/
```

(5) 结合压缩工具实现：归档并压缩 

```
-z 相当于gzip压缩工具
-j 相当于bzip2压缩工具
-J 相当于xz压缩工具
```

```
--exclude 排除文件
```

范例：

```bash
tar zcvf /root/a.tgz  --exclude=/app/host1 --exclude=/app/host2 /app
```

------



```
-T 选项指定输入文件
-X 选项指定包含要排除的文件列表
```

范例：

```bash
tar zcvf mybackup.tgz -T /root/includefilelist -X /root/excludefilelist
```

#### 7.3.2 split

split 命令可以分割一个文件为多个文件

将多个切割的小文件合并成一个大文件

```
cat mybackup-parts* > mybackup.tar.gz
```

#### 7.3.3 cpio

cpio 是历史悠久的打包和解包工具，不过目前也已较少使用

cpio 命令是通过重定向的方式将文件进行打包备份，还原恢复的工具，它可以解压以“.cpio”或者“.tar”结尾的文件

格式：

```
cpio [选项] > 文件名或者设备名
cpio [选项] < 文件名或者设备名
```

常用选项：

```
-o   output模式，打包，将标准输入传入的文件名打包后发送到标准输出
-i input模式，解包，对标准输入传入的打包文件名解包到当前目录
-t 预览，查看标准输入传入的打包文件中包含的文件列表
-O filename 输出到指定的归档文件名
-A 向已存在的归档文件中追加文件
-I filename 对指定的归档文件名解压
-F filename 使用指定的文件名替代标准输入或输出
-d 解包生成目录，在cpio还原时，自动的建立目录
-v 显示打包过程中的文件名称
```

# 8.软件管理

### 内容概述

* 软件运行环境

* 软件包基础

* rpm包管理

* yum和dnf 管理

* 定制yum仓库

* 编译安装

* Ubuntu软件管理

### 8.1 软件运行和编译

#### 8.1.1 软件相关概念

##### 8.1.1.1 ABI

ABI : Application Binary Interface

Windows与Linux不兼容

* ELF(Executable and Linkable Format) Linux 

* PE（Portable Executable）Windows 

库级别的虚拟化：

* Linux: WINE

* Windows: Cygwin

##### 8.1.1.2 API

API即Application Programming Interface，API可以在各种不同的操作系统上实现给应用程序提供完全相同的接口，而它们本身在这些系统上的实现却可能迥异，主流的操作系统有两种，一种是Windows系统，另一种是Linux系统。由于操作系统的不同，API又分为Windows API和Linux API。在Windows平台开发出来的软件在Linux上无法运行，在Linux上开发的软件在Windows上又无法运行，这就导致了软件移植困难，POSIX 标准的出现就是为了解决这个问题

POSIX：Portable Operating System Interface 可移植操作系统接口，定义了操作系统应该为应用程序提供的接口标准，是IEEE为要在各种UNIX操作系统上运行的软件而定义的一系列API标准的总称。

Linux和windows都要实现基本的posix标准，程序就在源代码级别可移植了

##### 8.1.1.3 开发语言

系统级开发

* 汇编语言

* C

* C++

应用级开发

* java

* Python

* go

* php

* perl

* delphi

* basic

* ruby

* bash 

#### 8.1.2 C 语言程序的实现过程

C 程序源代码 --> 预处理 --> 编译 --> 汇编 --> 链接

C语言的程序编译主要经过四个过程：

* 预处理（Pre-Processing） 

​	1）将所有的#define删除，并且展开所有的宏定义

​	2）处理所有的条件预编译指令，比如#if #ifdef #elif #else #endif等 

​	3）处理#include 预编译指令，将被包含的文件插入到该预编译指令的位置。

​	4）删除所有注释 "//"和"/* */".

​	5）添加行号和文件标识，以便编译时产生调试用的行号及编译错误警告行号。

​	6）保留所有的#pragma编译器指令，因为编译器需要使用它们

* 编译 （Compiling）

​	编译过程就是把预处理完的文件进行一系列的词法分析，语法分析，语义分析及优化后，最后生成

​	相应的汇编代码

* 汇编 （Assembling）

​	汇编器是将汇编代码转变成机器可以执行的命令，每一个汇编语句几乎都对应一条机器指令。汇编

​	相对于编译过程比较简单，根据汇编指令和机器指令的对照表一一翻译即可

* 链接 （Linking）

​	通过调用链接器ld来链接程序运行需要的一大堆目标文件，以及所依赖的其它库文件，最后生成可

​	执行文件

#### 8.1.3 软件模块的静态和动态链接

链接主要作用是把各个模块之间相互引用的部分处理好，使得各个模块之间能够正确地衔接，分为静态和动态链接

##### 8.1.3.1 静态链接

* 把程序对应的依赖库复制一份到包

* 生成模块文件libxxx.a

* 嵌入程序包

* 升级难，需重新编译

* 占用较多空间，迁移容易

##### 8.1.3.2 动态链接

* 只把依赖加做一个动态链接

* 生成模块文件libxxx.so

* 连接指向

* 占用较少空间，升级方便

##### 8.1.3.3 模块（库）文件

查看二进制程序所依赖的库文件

```
ldd /PATH/TO/BINARY_FILE
```

管理及查看本机装载的库文件

```
#加载配置文件中指定的库文件
ldconfig
#显示本机已经缓存的所有可用库文件名及文件路径映射关系
/sbin/ldconfig –p
```

配置文件：

```bash
/etc/ld.so.conf
/etc/ld.so.conf.d/*.conf
```

缓存文件：

```
/etc/ld.so.cache
```

#### 8.1.4 Java程序编译运行过程

### 8.2 软件包和包管理器

#### 8.2.1 软件包介绍

开源软件最初只提供了.tar.gz的打包的源码文件，用户必须自已编译每个想在GNU/Linux上运行的软件。用户急需系统能提供一种更加便利的方法来管理这些软件，当Debian诞生时，这样一个管理工具dpkg也就应运而生，可用来管理deb后缀的"包"文件。从而著名的"package"概念第一次出现在GNU/Linux系统中，稍后Red Hat才开发自己的rpm包管理系统

#### 8.2.2 软件包中的文件分类

* 二进制文件

* 库文件

* 配置文件

* 帮助文件

#### 8.2.3 程序包管理器

软件包管理器功能：

将编译好的应用程序的各组成文件打包一个或几个程序包文件，利用包管理器可以方便快捷地实现程序包的安装、卸载、查询、升级和校验等管理操作

主流的程序包管理器

* redhat：rpm文件, rpm 包管理器，rpm：Redhat Package Manager，RPM Package Manager 

* debian：deb文件, dpkg 包管理器

#### 8.2.4 包命名

源代码打包文件命名：

```
name-VERSION.tar.gz|bz2|xz
VERSION: major.minor.release
```

rpm包命名方式：

```
name-VERSION-release.arch.rpm
VERSION: major.minor.release
release：release.OS
```

常见的arch：

* x86: i386, i486, i586, i686

* x86_64: x64, x86_64, amd64

* powerpc: ppc

* 跟平台无关：noarch

#### 8.2.5 分类和拆包

软件包为了管理和使用的便利，会将一个大的软件分类，放在不同的子包中。

包的分类

* Application-VERSION-ARCH.rpm: 主包

* Application-devel-VERSION-ARCH.rpm 开发子包

* Application-utils-VERSION-ARHC.rpm 其它子包

* Application-libs-VERSION-ARHC.rpm 其它子包

#### 8.2.6 包的依赖

软件包之间可能存在依赖关系，甚至循环依赖，即：A包依赖B包，B包依赖C包，C包依赖A包

安装软件包时，会因为缺少依赖的包，而导致安装包失败。

解决依赖包管理工具：

* yum：rpm包管理器的前端工具

* dnf：Fedora 18+ rpm包管理器前端管理工具，CentOS 8 版代替 yum

* apt：deb包管理器前端工具

* zypper：suse上的rpm前端管理工具

#### 8.2.7 程序包管理器相关文件

1. 包文件组成 (每个包独有)

* 包内的文件

* 元数据，如：包的名称，版本，依赖性，描述等

* 可能会有包安装或卸载时运行的脚本

2. 数据库(公共)：/var/lib/rpm

* 程序包名称及版本

* 依赖关系

* 功能说明

* 包安装后生成的各文件路径及校验码信息

#### 8.2.8 获取程序包的途径

软件包需要事先将源码进行编译后打包形成，获取包的途径如下：

##### 8.2.7.1 系统发版的光盘或官方网站

CentOS 镜像：

https://www.centos.org/download/

http://mirrors.aliyun.com

https://mirrors.huaweicloud.com/

https://mirror.tuna.tsinghua.edu.cn/

http://mirrors.sohu.com

http://mirrors.163.com

Ubuntu 镜像：

http://cdimage.ubuntu.com/releases/

http://releases.ubuntu.com

##### 8.2.7.2 第三方组织提供

* Fedora-EPEL：Extra Packages for Enterprise Linux

https://fedoraproject.org/wiki/EPEL

https://mirrors.aliyun.com/epel/

https://mirrors.cloud.tencent.com/epel/

* Rpmforge：官网：http://repoforge.org/, RHEL推荐，包很全，即将关闭

* Community Enterprise Linux Repository：http://www.elrepo.org，支持最新的内核和硬件相关包

##### 8.2.7.3 软件项目官方站点

http://yum.mariadb.org/10.4/centos8-amd64/rpms/

http://repo.mysql.com/yum/mysql-8.0-community/el/8/x86_64/

##### 8.2.7.4 搜索引擎

http://pkgs.org

http://rpmfind.net

http://rpm.pbone.net

https://sourceforge.net/

注意：第三方包建议要检查其合法性，来源合法性,程序包的完整性

##### 8.2.7.5 自己制作

将源码文件，利用工具，如：rpmbuild，fpm 等工具制作成rpm包文件

### 8.3 rpm 包管理器

CentOS 系统上使用rpm命令管理程序包

功能：

安装、卸载、升级、查询、校验、数据库维护

#### 8.3.1 安装

格式：

```bash
rpm {-i|--install} [install-options] PACKAGE_FILE…
```

选项：

```
-v: verbose
-vv: 
-h: 以#显示程序包管理执行进度
```

常用组合：

```
rpm -ivh PACKAGE_FILE ...
```

rpm包安装[install-options]

```bash
--test: 测试安装，但不真正执行安装，即dry run模式
--nodeps：忽略依赖关系
--replacepkgs | replacefiles
--nosignature: 不检查来源合法性
--nodigest：不检查包完整性
--noscripts：不执行程序包脚本
 %pre: 安装前脚本 --nopre
 %post: 安装后脚本 --nopost
 %preun: 卸载前脚本 --nopreun
 %postun: 卸载后脚本 --nopostu
```

#### 8.3.2 升级和降级

rpm包升级

```
rpm {-U|--upgrade} [install-options] PACKAGE_FILE...
rpm {-F|--freshen} [install-options] PACKAGE_FILE...
```

对应选项:

```
upgrade：安装有旧版程序包，则"升级"，如果不存在旧版程序包，则"安装"
freshen：安装有旧版程序包，则"升级"， 如果不存在旧版程序包，则不执行升级操作
--oldpackage：降级
--force: 强制安装
```

常用组合

```
rpm -Uvh PACKAGE_FILE ...
rpm -Fvh PACKAGE_FILE ...
```

升级注意项：

(1) 不要对内核做升级操作；Linux支持多内核版本并存，因此直接安装新版本内核

(2) 如果原程序包的配置文件安装后曾被修改，升级时，新版本提供的同一个配置文件不会直接覆盖老版本的配置文件，而把新版本文件重命名(FILENAME.rpmnew)后保留

#### 8.3.3 包查询

```bash
rpm {-q|--query} [select-options] [query-options]
```

```bash
[select-options]
-a：所有包
-f：查看指定的文件由哪个程序包安装生成
-p rpmfile：针对尚未安装的程序包文件做查询操作
[query-options]
--changelog：查询rpm包的changelog
-c：查询程序的配置文件
-d：查询程序的文档
-i：information
-l：查看指定的程序包安装后生成的所有文件
--scripts：程序包自带的脚本
#和CAPABILITY相关
--whatprovides CAPABILITY：查询指定的CAPABILITY由哪个包所提供
--whatrequires CAPABILITY：查询指定的CAPABILITY被哪个包所依赖
--provides：列出指定程序包所提供的CAPABILITY
-R：查询指定的程序包所依赖的CAPABILITY
```

常用查询用法：

```bash
-qa
-q PACKAGE
-qi PACKAGE
-qc PACKAGE
-ql PACKAGE
-qd PACKAGE
-q --scripts PACKAGE
-qf FILE
-qpi PACKAGE_FILE
-qpl PACKAGE_FILE, ...
```

#### 8.3.4 包卸载

格式：

```bash
rpm {-e|--erase} [--allmatches] [--nodeps] [--noscripts] [--notriggers] [--test] 
PACKAGE_NAME ...
```

注意：当包卸载时，对应的配置文件不会删除， 以FILENAME.rpmsave形式保留

#### 8.3.5 包校验

在安装包时，系统也会检查包的来源是否是合法的

检查包的完整性和签名

```
rpm  -K|--checksig rpmfile
```

在检查包的来源和完整性前，必须导入所需要公钥

#### 8.3.6 数据库维护

rpm包安装时生成的信息，都放在rpm数据库中

```
/var/lib/rpm
```

可以重建数据库

```
rpm {--initdb|--rebuilddb}
initdb: 初始化，如果事先不存在数据库，则新建之，否则，不执行任何操作
rebuilddb：重建已安装的包头的数据库索引目录
```

### 8.4 yum和dnf

CentOS 使用 yum, dnf 解决rpm的包依赖关系

YUM: Yellowdog Update Modifier，rpm的前端程序，可解决软件包相关依赖性，可在多个库之间定位软件包，up2date的替代工具，CentOS 8 用dnf 代替了yum ,不过保留了和yum的兼容性，配置也是通用的

#### 8.4.1 yum/dnf 工作原理

yum/dnf 是基于C/S 模式

* yum 服务器存放rpm包和相关包的元数据库

* yum 客户端访问yum服务器进行安装或查询等

**yum 实现过程**

先在yum服务器上创建 yum repository（仓库），在仓库中事先存储了众多rpm包，以及包的相关的元数据文件（放置于特定目录repodata下），当yum客户端利用yum/dnf工具进行安装时包时，会自动下载repodata中的元数据，查询远数据是否存在相关的包及依赖关系，自动从仓库中找到相关包下载并安装。

#### 8.4.2 yum客户端配置

**yum客户端配置文件**

```bash
/etc/yum.conf #为所有仓库提供公共配置
/etc/yum.repos.d/*.repo： #为每个仓库的提供配置文件
```

**帮助参考：** man 5 yum.conf

**repo仓库配置文件指向的定义：**

```
[repositoryID]
name=Some name for this repository
baseurl=url://path/to/repository/
enabled={1|0}
gpgcheck={1|0}
gpgkey=URL
enablegroups={1|0}
failovermethod={roundrobin|priority}
 roundrobin：意为随机挑选，默认值
 priority:按顺序访问
cost= 默认为1000
```

**yum服务器的baseurl形式：**

```
file:// 本地路径
http://
https://
ftp://
```

**注意：yum仓库指向的路径一定必须是repodata目录所在目录**

**相关变量**

```
yum的repo配置文件中可用的变量：
$releasever: 当前OS的发行版的主版本号，如：8，7，6
$arch: CPU架构，如：aarch64, i586, i686，x86_64等
$basearch：系统基础平台；i386, x86_64
$contentdir：表示目录，比如：centos-8，centos-7
$YUM0-$YUM9:自定义变量
```

**baseurl 指向的路径**

阿里云提供了写好的CentOS和ubuntu的仓库文件下载链接

```
http://mirrors.aliyun.com/repo/
```

CentOS系统的yum源

```
#阿里云
https://mirrors.aliyun.com/centos/$releasever/
#腾讯云
https://mirrors.cloud.tencent.com/centos/$releasever/
#华为云
https://repo.huaweicloud.com/centos/$releasever/
#清华大学
https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/
```

EPEL的yum源

```
#阿里云
https://mirrors.aliyun.com/epel/$releasever/x86_64
#腾讯云
https://mirrors.cloud.tencent.com/epel/$releasever/x86_64
#华为云
https://mirrors.huaweicloud.com/epel/$releasever/x86_64
#清华大学
https://mirrors.tuna.tsinghua.edu.cn/epel/$releasever/x86_64
```

阿里巴巴开源软件

```
https://developer.aliyun.com/mirror/
```

**yum-config-manager命令** 

可以生成yum仓库的配置文件及启用或禁用仓库，来自于yum-utils包

格式：

```bash
#增加仓库
yum-config-manager --add-repo URL或file 
#禁用仓库
yum-config-manager --disable "仓库名"
#启用仓库
yum-config-manager --enable  "仓库名"
```

#### 8.4.3 yum命令

yum命令的用法：

```bash
yum [options] [command] [package ...]
```

yum的命令行选项：

```
-y #自动回答为"yes"
-q #静默模式
--nogpgcheck #禁止进行gpg check
--enablerepo=repoidglob    #临时启用此处指定的repo，支持通配符，如："*"
--disablerepo=repoidglob #临时禁用此处指定的repo,和上面语句同时使用，放在后面的生效
```

##### 8.4.3.1 显示仓库列表

```bash
yum repolist [all|enabled|disabled]
```

```bash
[root@node-1 ~]# yum repolist all					#显示所有的yum源
Loaded plugins: fastestmirror
Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
Loading mirror speeds from cached hostfile
repo id                                              repo name                                                status
!cuda                                                CUDA repository                                          enabled:   6
!docker-ce                                           docker-ce                                                enabled:   5
!extras                                              extras                                                   enabled:   0
!infra                                               infra repository                                         enabled:   0
!kubernetes                                          kubernetes                                               enabled:   5
!localepel                                           epel                                                     enabled:   7
!localextra                                          centos7 extra                                            enabled:   4
!localupdates                                        centos7 updates                                          enabled:   8
!localyum                                            centos7 os                                               enabled:   9
!localyum2                                           localyum2                                                enabled: 188
!nvidia-docker                                       nvidia-docker                                            enabled:   4
!updates                                             updates repository                                       enabled:   2
repolist: 238

[root@node-1 ~]# yum repolist -v				#显示仓库的详细信息
Loading "fastestmirror" plugin
Config time: 0.009
Yum version: 3.4.3
Loading mirror speeds from cached hostfile
Setting up Package Sacks
pkgsack time: 0.004
Repo-id      : cuda
Repo-name    : CUDA repository
Repo-revision: 1619076710
Repo-updated : Thu Apr 22 15:31:50 2021
Repo-pkgs    : 6
Repo-size    : 66 M
Repo-baseurl : http://10.242.212.164:6000/cuda/
Repo-expire  : 21,600 second(s) (last: Wed Nov 17 10:12:20 2021)
  Filter     : read-only:present
Repo-filename: /etc/yum.repos.d/galaxias.repo

Repo-id      : docker-ce
Repo-name    : docker-ce
Repo-revision: 1619076712
Repo-updated : Thu Apr 22 15:31:53 2021
Repo-pkgs    : 5
Repo-size    : 161 M
Repo-baseurl : http://10.242.212.164:6000/docker-ce/
Repo-expire  : 21,600 second(s) (last: Wed Nov 17 10:12:20 2021)
  Filter     : read-only:present
Repo-filename: /etc/yum.repos.d/galaxias.repo
```

##### 8.4.3.2 显示程序包

```
yum list
yum list [all | glob_exp1] [glob_exp2] [...]
yum list {available|installed|updates} [glob_exp1] [...]
```

##### 8.4.3.3 安装程序包

```bash
yum install package1 [package2] [...] 
yum reinstall package1 [package2] [...]  #重新安装 

--downloadonly  #只下载相关包默认至/var/cache/yum/x86_64/7/目录下,而不执行 install/upgrade/erase 
--downloaddir=, --destdir=  #--downloaddir选项来指定下载的目录,如果不存在 自动创建
```

###### 8.4.3.3.1 安装epel源包

```bash
#安装epel源
[root@centos7 ~]#yum -y install epel-release
[root@centos7 ~]#yum -y install sl
[root@centos7 ~]#rpm -ql sl
/usr/bin/sl
/usr/share/doc/sl-5.02
/usr/share/doc/sl-5.02/LICENSE
/usr/share/doc/sl-5.02/README.ja.md
/usr/share/doc/sl-5.02/README.md
/usr/share/man/ja/man1/sl.1.ja.gz
/usr/share/man/man1/sl.1.gz
#运行安装sl程序，可以看到下面火车，这标志着我们可以当老司机了
[root@centos7 ~]#sl -a
```

###### 8.4.3.3.2 升级最新内核

```bash
#范例：利用elrepo源在CentOS 7 安装新版内核
[root@centos7 ~]#yum install https://www.elrepo.org/elrepo-release-7.0-
4.el7.elrepo.noarch.rpm
[root@centos7 ~]#rpm -ql elrepo-release-7.0-4.el7.elrepo
/etc/pki/elrepo
/etc/pki/elrepo/SECURE-BOOT-KEY-elrepo.org.der
/etc/pki/rpm-gpg
/etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org
/etc/yum.repos.d
/etc/yum.repos.d/elrepo.repo
[root@centos7 ~]#yum repolist
```

###### 8.4.3.3.3 只下载相关的依赖包,而不安装

```bash
#/data/目录如果不存在,会自动创建
[root@centos8 ~]#yum -y install --downloadonly --downloaddir=/data/httpd httpd
```

**注意:** 下载包也可以通过启用配置文件实现

```bash
[root@centos7 ~]# cat /etc/yum.conf 
[main]
cachedir=/var/cache/yum/$basearch/$releasever  #缓存路径
keepcache=1  #如果为1,则下载rpm并缓存下来,不删除,默认安装rpm后会删除rpm包
```

##### 8.4.3.4 卸载程序包

```bash
yum remove | erase package1 [package2] [...]
```

##### 8.4.3.5 升级和降级

检查可用升级：

```bash
yum check-update
```

升级和降级

```
yum upgrade|update [package1] [package2] [...]
yum upgrade-minimal   #最小化升级
yum downgrade package1 [package2] [...] (降级)
```

##### 8.4.3.6 查询

查看程序包information：

```
yum info [...]  
```

查看指定的特性(可以是某文件)是由哪个程序包所提供：

```
yum provides | whatprovides feature1 [feature2] [...]
```

**注意：文件要写全路径，而不只是文件名，否则可能无法查询到**

以指定的关键字搜索程序包名及summary信息

```
yum search string1 [string2] [...]
```

查看指定包所依赖的capabilities：

```
yum deplist package1 [package2] [...]
```

##### 8.4.3.7 仓库缓存

清除目录/var/cache/yum/缓存

```bash
yum clean [ packages | metadata | expire-cache | rpmdb | plugins | all ]
```

构建缓存：

```
yum makecache
```

##### 8.4.3.8 查看yum事务历史

yum 执行安装卸载命令会记录到相关日志中 

日志 文件：

```bash
#CentOS 7以前版本日志
/var/log/yum.log
#CentOS 8 版本日志
/var/log/dnf.rpm.log
/var/log/dnf.log
```

日志命令

```
yum history [info|list|packages-list|packages-info|summary|addoninfo|redo|undo|rollback|new|sync|stats]
```

##### 8.4.3.9 安装及升级本地程序包

```
yum localinstall|install rpmfile1 [rpmfile2] [...]
yum localupdate|update rpmfile1 [rpmfile2] [...]
```

##### 8.4.3.10 查看包的安全警报

```
yum updateinfo --summary|--list|--info
```

##### 8.4.3.11 包组管理的相关命令

```
yum grouplist [hidden] [groupwildcard] [...]
yum groupinstall group1 [group2] [...]
yum groupupdate group1 [group2] [...]
yum groupremove group1 [group2] [...]
yum groupinfo group1 [...]
```

##### 8.4.3.12 实现私用 yum 仓库

下载所有yum仓库的相关包和meta 数据

```bash
#CentOS 8 dnf 工具集成
dnf reposync --help #查看帮助
#默认只下载rpm包，不下载 meta数据，需要指定--download-metadata 才能下载 meta
dnf reposync  --repoid=REPOID --download-metadata -p /path 
#CentOS 7 以前版本，reposync工具来自于yum-utils包
reposync --repoid=REPOID --download-metadata -p /path  
```

创建私有yum仓库：

```bash
createrepo [options] <directory>
```

##### 8.4.3.13 DNF 介绍

DNF，即DaNdiFied，是新一代的RPM软件包管理器。DNF 发行日期是2015年5月11日，DNF 包管理 器采用Python 编写，发行许可为GPL v2，首先出现在Fedora 18 发行版中。在 RHEL 8.0 版本正式取代 了 YUM，DNF包管理器克服了YUM包管理器的一些瓶颈，提升了包括用户体验，内存占用，依赖分 析，运行速度等

配置文件：

```
/etc/dnf/dnf.conf  
```

仓库文件：

```
/etc/yum.repos.d/ *.repo
```

日志：

```
/var/log/dnf.rpm.log
/var/log/dnf.log
```

DNF 使用帮助：man dnf 

dnf 用法与yum一致

```
dnf [options] <command> [<arguments>...]
dnf --version
dnf repolist
dnf reposync
dnf install httpd
dnf remove httpd
dnf clean all
dnf makecache
dnf list installed
dnf list available
dnf search nano
dnf history undo 1
```

##### 8.4.3.14 yum Troubleshooting

yum 和 dnf 失败最主要原因： 

* yum的配置文件格式或路径错误 

  解决方法：检查/etc/yum.repos.d/*.repo文件格式 

* yum cache 

  解决方法：yum clean all 

* 网络不通： 
* 解决方法：网卡配置

### 8.5 程序包编译

#### 8.5.1 源码编译介绍

程序包编译安装： 

源代码-->预处理-->编译-->汇编-->链接-->执行 

多文件：文件中的代码之间，很可能存在跨文件依赖关系 

虽然有很多开源软件将软件打成包，供人们使用，但并不是所有源代码都打成包，如果想使用开源软 件，可能需要自已下载源码，进行编译安装。另外即使提供了包，但是生产中需要用于软件的某些特 性，仍然需要自行编译安装。但是利用源代码编译安装是比较繁琐的，庆幸的是有相关的项目管理工具 可以大大减少编译过程的复杂度

#### 8.5.2 开源程序源代码的获取

项目官方自建站点： 

​	apache.org (ASF：Apache Software Foundation) 

​	mariadb.org 

​	... 

代码托管： 

​	Github.com 

​	gitee.com

​	SourceForge.net 

​	code.google.com

#### 8.5.3 编译源码的项目工具

* C、C++的源码编译：使用 make 项目管理器 

  configure脚本 --> Makefile.in --> Makefile 

  相关开发工具： 

  autoconf: 生成configure脚本 

  automake：生成Makefile.in 

* java的源码编译: 使用 maven

#### 8.5.4 C 语言源代码编译安装过程

利用编译工具，通常只需要三个大的步骤 

* ./configure 

  (1) 通过选项传递参数，指定安装路径、启用特性等；执行时会参考用户的指定以及Makefile.in文 件生成Makefile 

  (2) 检查依赖到的外部环境，如依赖的软件包 

* make 根据Makefile文件，会检测依赖的环境，进行构建应用程序 
* make install 复制文件到相应路径 

**注意：**安装前可以通过查看README，INSTALL获取帮助

##### 8.5.4.1 编译安装准备

准备：安装相关的依赖包 

* 开发工具：make, gcc (c/c++编译器GNU C Complier) 
* 开发环境：开发库（glibc：标准库），头文件，可安装开发包组 Development Tools 
* 软件相关依赖包 

生产实践：基于最小化安装的系统建议安装下面相关包

```bash
yum install  gcc make autoconf gcc-c++ glibc glibc-devel pcre pcre-devel openssl 
openssl-devel systemd-devel zlib-devel  vim lrzsz tree tmux lsof tcpdump wget 
net-tools iotop bc bzip2 zip unzip nfs-utils man-pages
```

##### 8.5.4.2 编译安装

第一步：运行 configure 脚本，生成 Makefile 文件 

其选项主要功能： 

* 可以指定安装位置 
* 指定启用的特性 

获取其支持使用的选项

```bash
./configure --help
```

选项分类：

* 安装路径设定 

​	--prefix=/PATH：指定默认安装位置,默认为/usr/local/ 

​	--sysconfdir=/PATH：配置文件安装位置 

​	System types：支持交叉编译 

* 软件特性和相关指定： 

  Optional Features: 可选特性 

  ​	--disable-FEATURE 

  ​	--enable-FEATURE[=ARG] 

  Optional Packages: 可选包 

  ​	--with-PACKAGE[=ARG] 依赖包 

  ​	--without-PACKAGE 禁用依赖关系

注意：通常被编译操作依赖的程序包，需要安装此程序包的"开发"组件，其包名一般类似于name-devel-VERSION

第二步：make 

第三步：make install

##### 8.5.4.3 安装后的配置

1. 二进制程序目录导入至PATH环境变量中 

   编辑文件/etc/profile.d/NAME.sh

```
export PATH=/PATH/TO/BIN:$PATH
```

2. 相关用户及文件 

   有些开源软件编译完成后，还需要创建相关的用户及文件 

​	3.导入帮助手册 

​		编辑/etc/man.config|man_db.conf文件,添加一个MANPATH 

##### 8.5.4.4 编译安装实战案例

###### 8.5.4.4.1 官网下载并编译安装新版 tree

###### 8.5.4.4.2 编译安装 cmatrix

###### 8.5.4.4.3 编译安装 httpd 2.4

# 9 磁盘存储和文件系统

### 内容概述

* 磁盘结构 
* 分区类型 
* 管理分区 
* 管理文件系统 
* 挂载设备 
* 管理swap空间
* RAID管理 
* LVM管理 
* LVM快照

### 9.1 磁盘结构

#### 9.1.1 设备文件 

一切皆文件：open(), read(), write(), close() 

设备文件：关联至一个设备驱动程序，进而能够跟与之对应硬件设备进行通信 

设备号码： 

* 主设备号：major number, 标识设备类型 
* 次设备号：minor number, 标识同一类型下的不同设备

设备类型： 

* 块设备：block，存取单位“块”，磁盘 
* 字符设备：char，存取单位“字符”，键盘

磁盘设备的设备文件命名：

```bash
/dev/DEV_FILE
/dev/sdX   # SAS,SATA,SCSI,IDE,USB 
/dev/nvme0n#   #nvme协议硬盘，如：第一个硬盘：nvme0n1，第二个硬盘：nvme0n2
```

虚拟磁盘：

```
/dev/vd 
/dev/xvd
```

不同磁盘标识：a-z,aa,ab…

#### 9.1.2 硬盘类型

**硬盘接口类型** 

* IDE：133MB/s，并行接口，早期家用电脑 
* SCSI：640MB/s，并行接口，早期服务器 
* SATA：6Gbps，SATA数据端口与电源端口是分开的，即需要两条线，一条数据线，一条电源线 
* SAS：6Gbps，SAS是一整条线，数据端口与电源端口是一体化的，SAS中是包含供电线的，而 SATA中不包含供电线。SATA标准其实是SAS标准的一个子集，二者可兼容，SATA硬盘可以插入 SAS主板上，反之不行 
* USB：480MB/s 
* M.2：

**注意**：速度不是由单纯的接口类型决定，支持Nvme协议硬盘速度是最快的

**服务器硬盘大小** 

* LFF：3.5寸，一般见到的那种台式机硬盘的大小 
* SFF：Small Form Factor 小形状因数，2.5寸，注意不同于2.5寸的笔记本硬盘 

L、S分别是大、小的意思，目前服务器或者盘柜采用sff规格的硬盘主要是考内虑增大单位密度内的磁盘 容量、增强散热、减小功耗

#### 9.1.3 机械硬盘和固态硬盘

* 机械硬盘（HDD）：

Hard Disk Drive，即是传统普通硬盘，主要由：盘片，磁头，盘片转轴及控制电 机，磁头控制器，数据转换器，接口，缓存等几个部分组成。机械硬盘中所有的盘片都装在一个旋转轴 上，每张盘片之间是平行的，在每个盘片的存储面上有一个磁头，磁头与盘片之间的距离比头发丝的直 径还小，所有的磁头联在一个磁头控制器上，由磁头控制器负责各个磁头的运动。磁头可沿盘片的半径 方向运动，加上盘片每分钟几千转的高速旋转，磁头就可以定位在盘片的指定位置上进行数据的读写操 作。数据通过磁头由电磁流来改变极性方式被电磁流写到磁盘上，也可以通过相反方式读取。硬盘为精 密设备，进入硬盘的空气必须过滤 

* 固态硬盘（SSD）：

Solid State Drive，用固态电子存储芯片阵列而制成的硬盘，由控制单元和存储单 元（FLASH芯片、DRAM芯片）组成。固态硬盘在接口的规范和定义、功能及使用方法上与普通硬盘的 完全相同，在产品外形和尺寸上也与普通硬盘一致 相较于HDD，SSD在防震抗摔、传输速率、功耗、重量、噪音上有明显优势，SSD传输速率性能是HDD 的2倍 相较于SSD，HDD在价格、容量占有绝对优势 硬盘有价，数据无价，目前SSD不能完全取代HHD

**硬盘存储术语 CHS** 

* head：磁头 磁头数=盘面数 
* track：磁道 磁道=柱面数 
* sector：扇区，512bytes 
* cylinder：柱面 1柱面=512 * sector数/track*head数=512*63*255=7.84M

**CHS** 

* CHS采用 24 bit位寻址 
* 其中前10位表示cylinder，中间8位表示head，后面6位表示sector 
* 最大寻址空间 8 GB

**LBA（logical block addressing）**

* LBA是一个整数，通过转换成 CHS 格式完成磁盘具体寻址 
* ATA-1规范中定义了28位寻址模式，以每扇区512位组来计算，ATA-1所定义的28位LBA上限达到 128 GiB。2002年ATA-6规范采用48位LBA，同样以每扇区512位组计算容量上限可达128  Petabytes

由于CHS寻址方式的寻址空间在大概8GB以内，所以在磁盘容量小于大概8GB时，可以使用CHS寻址方 式或是LBA寻址方式；在磁盘容量大于大概8GB时，则只能使用LBA寻址方式

### 9.2 管理存储

使用磁盘空间过程 

1. 设备分区 
2. 创建文件系统 
3. 挂载新的文件系统

#### 9.2.1 磁盘分区

##### 9.2.1.1 为什么分区 

* 优化I/O性能 
* 实现磁盘空间配额限制 
* 提高修复速度 
* 隔离系统和程序 
* 安装多个OS 
* 采用不同文件系统

##### 9.2.1.2 分区方式 

两种分区方式：MBR，GPT

###### 9.2.1.2.1 MBR分区

MBR：Master Boot Record，1982年，使用32位表示扇区数，分区不超过2T 

划分分区的单位： 

* CentOS 5 之前按整柱面划分 
* CentOS 6 版本后可以按Sector划分 

0磁道0扇区：512bytes 

​	446bytes: boot loader 启动相关 

​	64bytes：分区表，其中每16bytes标识一个分区 

​	2bytes: 55AA 

MBR分区中一块硬盘最多有4个主分区，也可以3主分区+1扩展(N个逻辑分区) 

MBR分区：主和扩展分区对应的1--4，/dev/sda3，逻辑分区从5开始，/dev/sda5

**硬盘主引导记录MBR由4个部分组成**

* 主引导程序（偏移地址0000H--0088H），它负责从活动分区中装载，并运行系统引导程序 
* 出错信息数据区，偏移地址0089H--00E1H为出错信息，00E2H--01BDH全为0字节 
* 分区表（DPT,Disk Partition Table）含4个分区项，偏移地址01BEH--01FDH,每个分区表项长16个 字节，共64字节为分区项1、分区项2、分区项3、分区项4 
* 结束标志字，偏移地址01FE--01FF的2个字节值为结束标志55AA

MBR中DPT结构

###### 9.2.1.2.2 GPT分区

GPT：GUID（Globals Unique Identifiers） partition table 支持128个分区，使用64位，支持8Z（ 512Byte/block ）64Z （ 4096Byte/block） 

使用128位UUID(Universally Unique Identifier) 表示磁盘和分区 GPT分区表自动备份在头和尾两份， 并有CRC校验位 

UEFI (Unified Extensible Firmware Interface 统一可扩展固件接口)硬件支持GPT，使得操作系统可以 启动

**GPT分区结构**

GPT分区结构分为4个区域：

* GPT头

* 分区表

* GPT分区

* 备份区域

##### 9.2.1.3 BIOS和UEFI

BIOS是固化在电脑主板上的程序，主要用于开机系统自检和引导操作系统。目前新式的电脑基本上都是

UEFI启动

BIOS（Basic Input Output System 基本输入输出系统）主要完成系统硬件自检和引导操作系统，操作系统开始启动之后，BIOS的任务就完成了。系统硬件自检：如果系统硬件有故障，主板上的扬声器就会发出长短不同的“滴滴”音，可以简单的判断硬件故障，比如“1长1短”通常表示内存故障，“1长3短”通常表示显卡故障

BIOS在1975年就诞生了，使用汇编语言编写，当初只有16位，因此只能访问1M的内存，其中前640K称为基本内存，后384K内存留给开机和各类BIOS本身使用。BIOS只能识别到主引导记录（MBR）初始化的硬盘，最大支持2T的硬盘，4个主分区（逻辑分区中的扩展分区除外），而目前普遍实现了64位系统，传统的BIOS已经无法满足需求了，这时英特尔主导的EFI就诞生了

EFI（Extensible Firmware Interface）可扩展固件接口，是 Intel 为 PC 固件的体系结构、接口和服务提出的建议标准。其主要目的是为了提供一组在 OS 加载之前（启动前）在所有平台上一致的、正确指定的启动服务，被看做是BIOS 的继任者，或者理解为新版BIOS。

UEFI是由EFI1.10为基础发展起来的，它的所有者已不再是Intel，而是一个称作Unified EFI Form的国际组织

UEFI(Unified Extensible Firmware Interface)统一的可扩展固件接口， 是一种详细描述类型接口的标准。UEFI 相当于一个轻量化的操作系统，提供了硬件和操作系统之间的一个接口，提供了图形化的操作界面。最关键的是引入了GPT分区表，支持2T以上的硬盘，硬盘分区不受限制

**BIOS和UEFI区别**

BIOS采用了16位汇编语言编写，只能运行在实模式（内存寻址方式由16位段寄存器的内容乘以16(10H)当做段基地址，加上16位偏移地址形成20位的物理地址）下，可访问的内存空间为1MB，只支持字符操作界面

UEFI采用32位或者64位的C语言编写，突破了实模式的限制，可以达到最大的寻址空间，支持图形操作界面，使用文件方式保存信息，支持GPT分区启动，适合和较新的系统和硬件的配合使用

**BIOS+MBR与UEFI+GPT**

MSDN (Microsoft Developer Network) 指出，Windows 只能安装于 BIOS + MBR 或是 UEFI + GPT 的组合上，而 BIOS + GPT 和 UEFI + MBR 是不允许的。但是 BIOS + GPT + GRUB 启动Linux 是可以的

##### 9.2.1.4 管理分区

列出块设备

```bash
lsblk
```

创建分区命令

```
fdisk 管理MBR分区
gdisk 管理GPT分区
parted 高级分区操作，可以是交互或非交互方式
```

重新设置内存中的内核分区表版本，适合于除了CentOS 6 以外的其它版本 5，7，8

```
partprobe
```

###### 9.2.1.4.1 parted 命令

**注意：parted的操作都是实时生效的，小心使用**

格式：

```
parted [选项]... [设备 [命令 [参数]...]...]
```

###### 9.2.1.4.2 分区工具fdisk和gdisk

```
fdisk -l [-u] [device...]     查看分区
fdisk [device...]             管理MBR分区
gdisk [device...]             类fdisk 的GPT分区工具
```

子命令：

```
p 分区列表
t 更改分区类型
n 创建新分区
d 删除分区
v 校验分区
u 转换单位
w 保存并退出
q 不保存并退出
```

查看内核是否已经识别新的分区

```
cat /proc/partations
```

**CentOS 7,8 同步分区表:**

```
partprobe
```

**Centos6 通知内核重新读取硬盘分区表**

新增分区用

```
partx -a /dev/DEVICE 
kpartx -a /dev/DEVICE -f: force
#示例：
[root@centos6 ~]#partx -a /dev/sda
```

删除分区用

```
partx -d --nr M-N /dev/DEVICE
#示例：
[root@centos6 ~]#partx -d --nr 6-8 /dev/sda
```

#### 9.2.2 文件系统

##### 9.2.2.1 文件系统概念

文件系统是操作系统用于明确存储设备或分区上的文件的方法和数据结构；即在存储设备上组织文件的方法。操作系统中负责管理和存储文件信息的软件结构称为文件管理系统，简称文件系统从系统角度来看，文件系统是对文件存储设备的空间进行组织和分配，负责文件存储并对存入的文件进行保护和检索的系统。具体地说，它负责为用户建立文件，存入、读出、修改、转储文件，控制文件的存取，安全控制，日志，压缩，加密等

支持的文件系统：

```
/lib/modules/`uname -r`/kernel/fs
```

各种文件系统：https://en.wikipedia.org/wiki/Comparison_of_file_systems

##### 9.2.2.2 文件系统类型

**Linux 常用文件系统**

* ext2：Extended file system 适用于那些分区容量不是太大，更新也不频繁的情况，例如 /boot 分 区

* ext3：是 ext2 的改进版本，其支持日志功能，能够帮助系统从非正常关机导致的异常中恢复

* ext4：是 ext 文件系统的最新版。提供了很多新的特性，包括纳秒级时间戳、创建和使用巨型文件(16TB)、最大1EB的文件系统，以及速度的提升

* xfs：SGI，支持最大8EB的文件系统

* swap

* iso9660 光盘

* btrfs（Oracle）

* reiserfs

**Windows 常用文件系统**

* FAT32

* NTFS

* exFAT

**Unix：**

* FFS（fast）

* UFS（unix）

* JFS2 

**网络文件系统：**

* NFS

* CIFS

**集群文件系统：**

* GFS2

* OCFS2（oracle）

**分布式文件系统：**

* fastdfs

* ceph

* moosefs

* mogilefs

* glusterfs

* Lustre

**RAW：**

裸文件系统,未经处理或者未经格式化产生的文件系统

**常用的文件系统特性：**

**FAT32**

* 最多只能支持16TB的文件系统和4GB的文件

**NTFS**

* 最多只能支持16EB的文件系统和16EB的文件

**EXT3**

* 最多只能支持32TB的文件系统和2TB的文件，实际只能容纳2TB的文件系统和16GB的文件

* Ext3目前只支持32000个子目录

* Ext3文件系统使用32位空间记录块数量和 inode数量

* 当数据写入到Ext3文件系统中时，Ext3的数据块分配器每次只能分配一个4KB的块

**EXT4：**

* EXT4是Linux系统下的日志文件系统，是EXT3文件系统的后继版本

* Ext4的文件系统容量达到1EB，而支持单个文件则达到16TB

* 理论上支持无限数量的子目录

* Ext4文件系统使用64位空间记录块数量和 inode数量

* Ext4的多块分配器支持一次调用分配多个数据块

* 修复速度更快

**XFS**

* 根据所记录的日志在很短的时间内迅速恢复磁盘文件内容

* 用优化算法，日志记录对整体文件操作影响非常小

* 是一个全64-bit的文件系统，最大可以支持8EB的文件系统，而支持单个文件则达到8EB

* 能以接近裸设备I/O的性能存储数据

查前支持的文件系统：

```
cat /proc/filesystems
```

##### 9.2.2.3 文件系统的组成部分

* 内核中的模块：ext4, xfs, vfat

* Linux的虚拟文件系统：VFS

* 用户空间的管理工具：mkfs.ext4, mkfs.xfs,mkfs.vfat

##### 9.2.2.4 文件系统选择管理

###### 9.2.2.4.1 创建文件系统

创建文件管理工具

* mkfs命令：

​	(1) mkfs.FS_TYPE /dev/DEVICE

 	ext4

​	 xfs

 	btrfs
 	
 	vfat

​	(2) mkfs -t FS_TYPE /dev/DEVICE

 	-L 'LABEL' 设定卷标

* mke2fs：ext系列文件系统专用管理工具

常用选项

```
-t {ext2|ext3|ext4|xfs} 指定文件系统类型
-b {1024|2048|4096} 指定块 block 大小
-L ‘LABEL’ 设置卷标
-j 相当于 -t ext3， mkfs.ext3 = mkfs -t ext3 = mke2fs -j = mke2fs -t ext3
-i  # 为数据空间中每多少个字节创建一个inode；不应该小于block大 小
-N  # 指定分区中创建多少个inode
-I 一个inode记录占用的磁盘空间大小，128---4096
-m  # 默认5%,为管理人员预留空间占总空间的百分比
-O FEATURE[,...] 启用指定特性
-O ^FEATURE 关闭指定特性
```

###### 9.2.2.4.2 查看和管理分区信息

blkid 可以查看块设备属性信息

格式：

```
blkid [OPTION]... [DEVICE]
```

常用选项：

-U UUID 根据指定的UUID来查找对应的设备

-L LABEL 根据指定的LABEL来查找对应的设备

e2label：管理ext系列文件系统的LABEL

```
e2label DEVICE [LABEL]
```

findfs ：查找分区

```
findfs [options] LABEL=<label>
findfs [options] UUID=<uuid>
```

tune2fs：重新设定ext系列文件系统可调整参数的值

```
-l 查看指定文件系统超级块信息；super block
-L 'LABEL’ 修改卷标
-m # 修预留给管理员的空间百分比
-j 将ext2升级为ext3
-O 文件系统属性启用或禁用, -O ^has_journal
-o 调整文件系统的默认挂载选项，-o ^acl 
-U UUID 修改UUID号
```

dumpe2fs：显示ext文件系统信息，将磁盘块分组管理

-h：查看超级块信息，不显示分组信息

xfs_info：显示示挂载或已挂载的 xfs 文件系统信息

```
xfs_info mountpoint|devname
```

**块组描述符表(GDT)**

ext文件系统每一个块组信息使用32字节描述，这32个字节称为块组描述符，所有块组的块组描述符组成块组描述符表GDT(group descriptor table)。虽然每个块组都需要块组描述符来记录块组的信息和属性元数据，但是不是每个块组中都存放了块组描述符。将所有块组的块组信息组成一个GDT保存,并将该GDT存放于某些块组中，类似存放superblock和备份superblock的块

###### 9.2.2.4.3 文件系统检测和修复

文件系统夹故障常发生于死机或者非正常关机之后，挂载为文件系统标记为“no clean”

**注意：**一定不要在挂载状态下执行下面命令修复

fsck: File System Check

```
fsck.FS_TYPE
fsck -t FS_TYPE
```

**注意：**FS_TYPE 一定要与分区上已经文件类型相同

常用选项：

```
-a 自动修复
-r 交互式修复错误
```

e2fsck：ext系列文件专用的检测修复工具

```
-y 自动回答为yes
-f 强制修复
-p 自动进行安全的修复文件系统问题
```

xfs_repair：xfs文件系统专用检测修复工具

常用选项：

```
-f 修复文件，而设备
-n 只检查
-d 允许修复只读的挂载设备，在单用户下修复 / 时使用，然后立即reboot
```

#### 9.2.3 挂载

挂载:将额外文件系统与根文件系统某现存的目录建立起关联关系，进而使得此目录做为其它文件访问入口的行为

卸载:为解除此关联关系的过程

把设备关联挂载点：mount Point

挂载点下原有文件在挂载完成后会被临时隐藏，因此，挂载点目录一般为空

进程正在使用中的设备无法被卸载

##### 9.2.3.1 挂载文件系统 mount

格式：

```
mount [-fnrsvw] [-t vfstype] [-o options] device mountpoint
```

**device：指明要挂载的设备**

* 设备文件：例如:/dev/sda5

* 卷标：-L 'LABEL', 例如 -L 'MYDATA'

* UUID： -U 'UUID'：例如 -U '0c50523c-43f1-45e7-85c0-a126711d406e'

* 伪文件系统名称：proc, sysfs, devtmpfs, configfs

**mountpoint：**挂载点目录必须事先存在，建议使用空目录

**mount 常用命令选项**

```
-t fstype 指定要挂载的设备上的文件系统类型,如:ext4,xfs
-r readonly，只读挂载
-w read and write, 读写挂载,此为默认设置,可省略
-n 不更新/etc/mtab，mount不可见
-a 自动挂载所有支持自动挂载的设备(定义在了/etc/fstab文件中，且挂载选项中有
auto功能)
-L 'LABEL' 以卷标指定挂载设备
-U 'UUID' 以UUID指定要挂载的设备
-B, --bind 绑定目录到另一个目录上
-o options：(挂载文件系统的选项)，多个选项使用逗号分隔
 	async   异步模式,内存更改时,写入缓存区buffer,过一段时间再写到磁盘中，效率高，但不安全
   	sync   同步模式,内存更改时，同时写磁盘，安全，但效率低下
	atime/noatime 包含目录和文件
 	diratime/nodiratime 目录的访问时间戳
 	auto/noauto 是否支持开机自动挂载，是否支持-a选项
 	exec/noexec 是否支持将文件系统上运行应用程序
 	dev/nodev 是否支持在此文件系统上使用设备文件
 	suid/nosuid 是否支持suid和sgid权限
 	remount 重新挂载
 	ro/rw 只读、读写   
 	user/nouser 是否允许普通用户挂载此设备，/etc/fstab使用
 	acl/noacl 启用此文件系统上的acl功能
 	loop 使用loop设备
 	_netdev   当网络可用时才对网络资源进行挂载，如：NFS文件系统
 	defaults 相当于rw, suid, dev, exec, auto, nouser, async
```

**挂载规则:**

* 一个挂载点同一时间只能挂载一个设备

* 一个挂载点同一时间挂载了多个设备，只能看到最后一个设备的数据，其它设备上的数据将被隐藏

* 一个设备可以同时挂载到多个挂载点

* 通常挂载点一般是已存在空的目录

##### 9.2.3.2 卸载文件系统 umount

卸载时：可使用设备，也可以使用挂载点

```
umount 设备名|挂载点
```

##### 9.2.3.3 查看挂载情况

查看挂载

```
#通过查看/etc/mtab文件显示当前已挂载的所有设备
mount
#查看内核追踪到的已挂载的所有设备
cat /proc/mounts
```

查看挂载点情况

```
findmnt   MOUNT_POINT|device
```

查看正在访问指定文件系统的进程

```
lsof MOUNT_POINT
fuser -v MOUNT_POINT
```

终止所有在正访问指定的文件系统的进程

```
fuser -km MOUNT_POINT
```

##### 9.2.3.4 持久挂载

将挂载保存到 /etc/fstab 中可以下次开机时，自动启用挂载

/etc/fstab格式帮助：

```
man 5 fstab
```

每行定义一个要挂载的文件系统,，其中包括共 6 项

* 要挂载的设备或伪文件系统

​		 设备文件

 		LABEL：LABEL=""
 	
 		UUID：UUID=""
 	
 		伪文件系统名称：proc, sysfs

* 挂载点：必须是事先存在的目录

* 文件系统类型：ext4，xfs，iso9660，nfs，none

* 挂载选项：defaults ，acl，bind

* 转储频率：0：不做备份 1：每天转储 2：每隔一天转储

* fsck检查的文件系统的顺序：允许的数字是0 1 2

​		0：不自检 ，1：首先自检；一般只有rootfs才用 2：非rootfs使用

添加新的挂载项，需要执行下面命令生效

```
mount -a
```

#### 9.2.4 处理交换文件和分区

##### 9.2.4.1 swap 介绍

swap交换分区是系统RAM的补充，swap 分区支持虚拟内存。当没有足够的 RAM 保存系统处理的数据时会将数据写入 swap 分区，当系统缺乏 swap 空间时，内核会因 RAM 内存耗尽而终止进程。配置过多 swap 空间会造成存储设备处于分配状态但闲置，造成浪费，过多 swap 空间还会掩盖内存泄露

注意：为优化性能，可以将swap 分布存放，或高性能磁盘存放

**官方推荐推荐系统 swap 空间**

https://access.redhat.com/documentation/zh-cn/red_hat_enterprise_linux/7/html/installation_guide/sect-disk-partitioning-setup-ppc#sect-recommended-partitioning-scheme-ppc

##### 9.2.4.2 交换分区实现过程

1. 创建交换分区或者文件

2. 使用mkswap写入特殊签名

3. 在/etc/fstab文件中添加适当的条目

4. 使用swapon -a 激活交换空间

启用swap分区

```
swapon [OPTION]... [DEVICE]
```

选项：

```
-a：激活所有的交换分区
-p PRIORITY：指定优先级，也可在/etc/fstab 在第4列指定：pri=value
```

禁用swap分区：

```
swapoff [OPTION]... [DEVICE]
```

**SWAP的优先级**

可以指定swap分区0到32767的优先级，值越大优先级越高

如果用户没有指定，那么核心会自动给swap指定一个优先级，这个优先级从-1开始，每加入一个新的

没有用户指定优先级的swap，会给这个优先级减一

先添加的swap的缺省优先级比较高，除非用户自己指定一个优先级，而用户指定的优先级(是正数)永远高于核心缺省指定的优先级(是负数)

##### 9.2.4.3 swap的使用策略

/proc/sys/vm/swappiness 的值决定了当内存占用达到一定的百分比时，会启用swap分区的空间

说明：内存在使用到100-30=70%的时候，就开始出现有交换分区的使用。简单地说这个参数定义了系统对swap的使用倾向，默认值为30，值越大表示越倾向于使用swap。可以设为0，这样做并不会禁止对swap的使用，只是最大限度地降低了使用swap的可能性

#### 9.2.5 移动介质

挂载意味着使外来的文件系统看起来如同是主目录树的一部分，所有移动介质也需要挂载，挂载点通常在/media 或/mnt下

访问前，介质必须被挂载

摘除时，介质必须被卸载

按照默认设置，非根用户只能挂载某些设备（光盘、DVD、软盘、USB等等）

##### 9.2.5.1 使用光盘

在图形环境下自动启动挂载/run/media//

手工挂载

```
mount /dev/cdrom /mnt/
```

操作光盘

```
eject #弹出光盘
eject -t #弹入光盘
```

创建ISO文件

```
cp /dev/cdrom /root/centos.iso
mkisofs  -r  -o /root/etc.iso /etc  #来自于genisoimage包
```

刻录光盘

```
wodim -v -eject centos.iso
```

**将ISO制作为U盘工具Rufus**

官网: http://rufus.ie/

Rufus 是一个开源免费的快速制作 U 盘系统启动盘和格式化 USB 的实用小工具，它可以快速把 ISO 格式的系统镜像文件快速制作成可引导的 USB 启动安装盘，支持 Windows 或 Linux 启动。Rufus 小巧玲珑，软件体积仅 7 百多 KB，然而麻雀虽小，它却五脏俱全

##### 9.2.5.2 USB介质

查看USB设备是否识别

```
lsusb  #来自于usbuntils包
```

被内核探测为SCSI设备

/dev/sdaX、/dev/sdbX或类似的设备文件

在图形环境中自动挂载在/run/media//

手动挂载

```
mount /dev/sdX# /mnt
```

#### 9.2.6 磁盘常见工具

##### 9.2.6.1 文件系统空间实际真正占用等信息的查看工具 df

```
df [OPTION]... [FILE]...
```

常用选项：

```
-H 以10为单位
-T 文件系统类型
-h human-readable
-i inodes instead of blocks
-P 以Posix兼容的格式输出
```

##### 9.2.6.2 查看某目录总体空间实际占用状态 du

显示指定目录下面各个子目录的大小,单位为KB

```
du [OPTION]... DIR
```

常用选项

```
-h human-readable
-s   summary  
--max-depth=#   指定最大目录层级
-x, --one-file-system   #忽略不在同一个文件系统的目录
```

大厂面试题: df 和 du 区别,什么时候df >du 什么时候df < du 

```
目录内挂载有其它分区时的情况
当删除文件但不释放空间时,有什么不同?(du 查看文件空间释放,df不释放)
```

##### 9.2.6.3 工具 dd

dd 命令：convert and copy a file

格式：

```
dd if=/PATH/FROM/SRC of=/PATH/TO/DEST  bs=# count=#
```

常用选项

```
if=file 从所命名文件读取而不是从标准输入
of=file 写到所命名的文件而不是到标准输出
ibs=size   一次读size个byte
obs=size       一次写size个byte
bs=size block size, 指定块大小（既是是ibs也是obs)
cbs=size       一次转化size个byte
skip=blocks   从开头忽略blocks个ibs大小的块
seek=blocks   从开头忽略blocks个obs大小的块
count=n         复制n个bs
conv=conversion[,conversion...] 用指定的参数转换文件

conversion 转换参数: 
ascii 转换 EBCDIC 为 ASCII
ebcdic 转换 ASCII 为 EBCDIC
lcase 把大写字符转换为小写字符
ucase 把小写字符转换为大写字符
nocreat 不创建输出文件
noerror 出错时不停止
notrunc 不截短输出文件
sync 把每个输入块填充到ibs个字节，不足部分用空(NUL)字符补齐
fdatasync 写完成前，物理写入输出文件
```

**练习**

1、创建一个2G的文件系统，块大小为2048byte，预留1%可用空间,文件系统ext4，卷标为TEST，要求此分区开机后自动挂载至/test目录，且默认有acl挂载选项

2、写一个脚本，完成如下功能：

 	(1) 列出当前系统识别到的所有磁盘设备
 	
 	(2) 如磁盘数量为1，则显示其空间使用信息

​	否则，则显示最后一个磁盘上的空间使用信息

3、将CentOS6的CentOS-6.10-x86_64-bin-DVD1.iso和CentOS-6.10-x86_64-bin-DVD2.iso两个文件，合并成一个CentOS-6.10-x86_64-Everything.iso文件，并将其配置为yum源

### 9.3 RAID

#### 9.3.1 什么是RAID

"RAID"一词是由David Patterson, Garth A. Gibson, Randy Katz 于1987年在加州大学伯克利分校发明的。在1988年6月SIGMOD会议上提交的论文"A Case for Redundant Arrays of Inexpensive Disks”"中提出，当时性能最好的大型机不断增长的个人电脑市场开发的一系列廉价驱动器的性能所击败。尽管故障与驱动器数量的比例会上升，但通过配置冗余，阵列的可靠性可能远远超过任何大型单个驱动器的可靠性

**独立硬盘冗余阵列**（RAID, Redundant Array of Independent Disks），旧称廉价磁盘冗余阵列（Redundant Array of Inexpensive Disks），简称磁盘阵列。利用虚拟化存储技术把多个硬盘组合起来，成为一个或多个硬盘阵列组，目的为提升性能或数据冗余，或是两者同时提升。

RAID 层级不同，数据会以多种模式分散于各个硬盘，RAID 层级的命名会以 RAID 开头并带数字，例如：RAID 0、RAID 1、RAID 5、RAID 6、RAID 7、RAID 01、RAID 10、RAID 50、RAID 60。每种等级都有其理论上的优缺点，不同的等级在两个目标间获取平衡，分别是增加数据可靠性以及增加存储器（群）读写性能。

简单来说，RAID把多个硬盘组合成为一个逻辑硬盘，因此，操作系统只会把它当作一个实体硬盘。RAID常被用在服务器电脑上，并且常使用完全相同的硬盘作为组合。由于硬盘价格的不断下降与RAID功能更加有效地与主板集成，它也成为普通用户的一个选择，特别是需要大容量存储空间的工作，如：视频与音频制作。

**RAID功能实现**

* 提高IO能力,磁盘并行读写

* 提高耐用性,磁盘冗余算法来实现

**RAID实现的方式**

* 外接式磁盘阵列：通过扩展卡提供适配能力

* 内接式RAID：主板集成RAID控制器，安装OS前在BIOS里配置

* 软件RAID：通过OS实现，比如：群晖的NAS

#### 9.3.2 RAID级别

级别：多块磁盘组织在一起的工作方式有所不同

参考链接: https://zh.wikipedia.org/wiki/RAID

RAID-0：条带卷，strip

RAID-1：镜像卷，mirror

RAID-2

......

RAID-5

RAID-6

RAID-7

RAID-10

RAID-01

RAID-50

......

##### 9.3.2.1 RAID-0

以 chunk 单位,读写数据,因为读写时都可以并行处理，所以在所有的级别中，RAID 0的速度是最快的。但是RAID 0既没有冗余功能，也不具备容错能力，如果一个磁盘（物理）损坏，所有数据都会丢失读、写性能提升

可用空间：N*min(S1,S2,...)

无容错能力

最少磁盘数：1+

##### 9.3.2.2 RAID-1

也称为镜像, 两组以上的N个磁盘相互作镜像，在一些多线程操作系统中能有很好的读取速度，理论上读取速度等于硬盘数量的倍数，与RAID 0相同。另外写入速度有微小的降低。

读性能提升、写性能略有下降

可用空间：1*min(S1,S2,...)

磁盘利用率 50%

有冗余能力

最少磁盘数：2+

##### 9.3.2.3 RAID-4

多块数据盘异或运算值存于专用校验盘

磁盘利用率 (N-1)/N

有冗余能力

至少3块硬盘才可以实现

##### 9.3.2.4 RAID-5

读、写性能提升

可用空间：(N-1)*min(S1,S2,...)

有容错能力：允许最多1块磁盘损坏

最少磁盘数：3, 3+

##### 9.3.2.5 RAID-6

双份校验位,算法更复杂

读、写性能提升

可用空间：(N-2)*min(S1,S2,...)

有容错能力：允许最多2块磁盘损坏

最少磁盘数：4, 4+

##### 9.3.2.6 RAID-10

读、写性能提升

可用空间：N*min(S1,S2,...)/2

有容错能力：每组镜像最多只能坏一块

最少磁盘数：4, 4+

##### 9.3.2.7 RAID-01

多块磁盘先实现RAID0,再组合成RAID1

##### 9.3.2.8 RAID-50

多块磁盘先实现RAID5,再组合成RAID0

##### 9.3.2.9 RAID-60

##### 9.3.2.10 其它级别

**JBOD：Just a Bunch Of Disks 只是一堆磁盘**

功能：将多块磁盘的空间合并一个大的连续空间使用

第一块硬盘存放所有磁盘的分段信息,如果损坏,整个阵列会失败

后续磁盘损坏只会影响本块磁盘的数据

可用空间：sum(S1,S2,...)

**RAID7**

RAID 7并非公开的RAID标准，而是美国公司的Storage Computer Corporation的专利硬件产品名称，RAID 7是以RAID 3及RAID 4为基础所发展，但是经过强化以解决原来的一些限制。另外，在实现中使用大量的缓冲存储器以及用以实现异步数组管理的专用即时处理器，使得RAID 7可以同时处理大量的IO要求，所以性能甚至超越了许多其他RAID标准的实际产品。但也因为如此，在价格方面非常的高昂.RAID7 可以理解为一个独立存储计算机，自身带有操作系统和管理工具，可以独立运行，理论上性能最高的RAID模式

**SHR(Synology Hybrid RAID)**

群晖公司的技术,适合不了解RAID的普通用户

根据磁盘个数自动组成不同的RAID,1块普通磁盘,2块RAID1,3块RAID4,SHR2类似于RAID6

只支持群晖系统

**9.3.2.11 RAID 总结**

常用级别：RAID-0, RAID-1, RAID-5, RAID-10, RAID-50,RAID-60

#### 9.3.3 实现软RAID

mdadm工具：为软RAID提供管理界面，为空余磁盘添加冗余，结合内核中的md(multi devices)RAID设备可命名为/dev/md0、/dev/md1、/dev/md2、/dev/md3等

mdadm：模式化的工具,支持的RAID级别：LINEAR, RAID0, RAID1, RAID4, RAID5, RAID6, RAID10 

命令的语法格式：

```
mdadm [mode] <raiddevice> [options] <component-devices>
```

常用选项说明

```
模式：
 创建：-C
 装配：-A
 监控：-F
 管理：-f, -r, -a
<raiddevice>: /dev/md#
<component-devices>: 任意块设备
-C: 创建模式
 	-n #: 使用#个块设备来创建此RAID
 	-l #：指明要创建的RAID的级别
 	-a {yes|no}：自动创建目标RAID设备的设备文件
 	-c CHUNK_SIZE: 指明块大小,单位k
 	-x #: 指明空闲盘的个数
-D：显示raid的详细信息
 	mdadm -D /dev/md#
管理模式：
 	-f: 标记指定磁盘为损坏
 	-a: 添加磁盘
 	-r: 移除磁盘
 	
观察md的状态： cat /proc/mdstat
```

生成配置文件：

```
mdadm -D -s >> /etc/mdadm.conf
```

停止设备：

```
mdadm -S /dev/md0
```

激活设备：

```
mdadm -A -s /dev/md0
```

强制启动：

```
mdadm -R /dev/md0
```

删除raid信息：

```
mdadm --zero-superblock /dev/sdb1
```

练习

1：创建一个可用空间为1G的RAID1设备，文件系统为ext4，有一个空闲盘，开机可自动挂载至/backup目录

2：创建由三块硬盘组成的可用空间为2G的RAID5设备，要求其chunk大小为256k，文件系统为ext4，开机可自动挂载至/mydata目录

### 9.4 逻辑卷管理器（LVM）

#### 9.4.1 LVM介绍

LVM: Logical Volume Manager 可以允许对卷进行方便操作的抽象层，包括重新设定文件系统的大小，允许在多个物理设备间重新组织文件系统

LVM可以弹性的更改LVM的容量

通过交换PE来进行资料的转换，将原来LV内的PE转移到其他的设备中以降低LV的容量，或将其他设备中的PE加到LV中以加大容量

**实现过程**

* 将设备指定为物理卷

* 用一个或者多个物理卷来创建一个卷组，物理卷是用固定大小的物理区域（Physical Extent，PE）来定义的

* 在物理卷上创建的逻辑卷， 是由物理区域（PE）组成

* 可以在逻辑卷上创建文件系统并挂载

第一个逻辑卷对应设备名：/dev/dm-#

dm: device mapper，将一个或多个底层块设备组织成一个逻辑设备的模块

软链接：

* /dev/mapper/VG_NAME-LV_NAME

* /dev/VG_NAME/LV_NAME

#### 9.4.2 实现逻辑卷

相关工具来自于 lvm2 包

##### 9.4.2.1 pv管理工具

显示pv信息

```
pvs：简要pv信息显示
pvdisplay
```

创建pv

```
pvcreate /dev/DEVICE
```

删除pv

```
pvremove /dev/DEVICE
```

##### 9.4.2.2 vg管理工具

显示卷组

```
vgs
vgdisplay
```

创建卷组

```
vgcreate [-s #[kKmMgGtTpPeE]] VolumeGroupName PhysicalDevicePath 
[PhysicalDevicePath...]
#示例
vgcreate -s 16M vg0 /dev/sdb /dev/sdc  #指定PE的大小,默认4M
```

管理卷组

```
vgextend VolumeGroupName PhysicalDevicePath [PhysicalDevicePath...]
vgreduce VolumeGroupName PhysicalDevicePath [PhysicalDevicePath...]
```

删除卷组

* 先做pvmove

* 再做vgremove

##### 9.4.2.3 lv管理工具

显示逻辑卷

```
lvs
Lvdisplay
```

创建逻辑卷

```
lvcreate -L #[mMgGtT] -n NAME VolumeGroup
```

删除逻辑卷

```
lvremove /dev/VG_NAME/LV_NAME
```

重设文件系统大小

```
fsadm [options] resize device [new_size[BKMGTEP]]
resize2fs [-f] [-F] [-M] [-P] [-p] device [new_size]
xfs_growfs /mountpoint
```

##### 9.4.2.4 扩展和缩减逻辑卷

###### 9.4.2.4.1 在线扩展逻辑卷

```
#两步实现
#第一步实现逻辑卷的空间扩展
lvextend -L [+]#[mMgGtT] /dev/VG_NAME/LV_NAME
#第二步实现文件系统的扩展
#针对ext
resize2fs /dev/VG_NAME/LV_NAME
#针对xfs 
xfs_growfs MOUNTPOINT 
#一步实现容间和文件系统的扩展
lvresize -r -l +100%FREE /dev/VG_NAME/LV_NAME
```

###### 9.4.2.4.2 缩减逻辑卷

**注意**：缩减有数据损坏的风险，建议先备份再缩减，xfs文件系统不支持缩减

```
umount /dev/VG_NAME/LV_NAME
e2fsck -f /dev/VG_NAME/LV_NAME
resize2fs /dev/VG_NAME/LV_NAME #[mMgGtT]
lvreduce -L [-]#[mMgGtT] /dev/VG_NAME/LV_NAME
mount /dev/VG_NAME/LV_NAME mountpoint
```

##### 9.4.2.5 跨主机迁移卷组

源计算机上

1 在旧系统中，umount 所有卷组上的逻辑卷

2 禁用卷组

```
vgchange -a n vg0 
lvdisplay
```

3 导出卷组

```
vgexport vg0 
pvscan
vgdisplay
```

4 拆下旧硬盘在目标计算机上,并导入卷组：

```
vgimport vg0
```

5 启用

```
vgchange -ay vg0  
```

6 mount 所有卷组上的逻辑卷

##### **9.4.2.6 拆除指定的PV存储设备**

#### 9.4.3 逻辑卷快照

##### 9.4.3.1 逻辑卷快照原理

快照是特殊的逻辑卷，它是在生成快照时存在的逻辑卷的准确拷贝,对于需要备份或者复制的现有数据临时拷贝以及其它操作来说，快照是最合适的选择,快照只有在它们和原来的逻辑卷不同时才会消耗空间，建立快照的卷大小小于等于原始逻辑卷,也可以使用lvextend扩展快照

逻辑卷管理器快照

快照就是将当时的系统信息记录下来，就好像照相一般，若将来有任何数据改动了，则原始数据会被移动到快照区，没有改动的区域则由快照区和文件系统共享

逻辑卷快照工作原理

* 在生成快照时会分配给它一定的空间，但只有在原来的逻辑卷或者快照有所改变才会使用这些空间

* 当原来的逻辑卷中有所改变时，会将旧的数据复制到快照中

* 快照中只含有原来的逻辑卷中更改的数据或者自生成快照后的快照中更改的数据

由于快照区与原本的LV共用很多PE的区块，因此快照与被快照的LV必须在同一个VG中.系统恢复的时候的文件数量不能高于快照区的实际容量

快照特点：

* 备份速度快，瞬间完

* 应用场景是测试环境，不能完成代替备份

* 快照后，逻辑卷的修改速度会一定有影响

##### 9.4.3.2 实现逻辑卷快照

**练习**

1、创建一个至少有两个PV组成的大小为20G的名为testvg的VG；要求PE大小为16MB, 而后在卷组中创建大小为5G的逻辑卷testlv；挂载至/users目录

2、 新建用户archlinux，要求其家目录为/users/archlinux，而后su切换至archlinux用户，复制/etc/pam.d目录至自己的家目录

3、扩展testlv至7G，要求archlinux用户的文件不能丢失

4、收缩testlv至3G，要求archlinux用户的文件不能丢失

5、对testlv创建快照，并尝试基于快照备份数据，验证快照的功能

# 10 网络协议和管理

### 内容概述

* 网络概念

* OSI模型

* 网络设备

* TCP/IP

* IP地址规划

* 配置网络

* 多网卡绑定

* 网桥

* 网络测试工具

* Ubuntu网络配置

### 10.1 网络基础

#### 10.1.1 网络概念

计算机网络是一组计算机或网络设备通过有形的线缆或无形的媒介如无线，连接起来，按照一定的规 则，进行通信的集合。

网络功能和优点 

* 数据和应用程序 
* 资源 
* 网络存储 
* 备份设备

作用范围分类 

* 广域网(WAN,Wide Area Network) 
* 城域网(MAN,Metropolitan Area Network) 
* 局域网(LAN,Local Area Network)

#### 10.1.2 常见的网络物理组件

RJ-45连接器、网络接口卡、PC、服务器、交换机、路由器

#### 10.1.3 网络应用程序

##### 10.1.3.1 各种网络应用

* Web 浏览器（Chrome、IE、Firefox等） 
* 即时消息（QQ、微信、钉钉等） 
* 电子邮件（Outlook、foxmail 等） 
* 协作（视频会议、VNC、Netmeeting、WebEx 等） 
* web网络服务（apache，nginx，IIS） 
* 文件网络服务（ftp ，nfs，samba） 
* 数据库服务（ MySQL，MariaDB，MongoDB) 
* 中间件服务（Tomcat，JBoss） 
* 安全服务（Netfilter）

##### 10.1.3.2 应用程序对网络的要求 

* 批处理应用程序 

  FTP、TFTP、库存更新

  无需直接人工交互 

  带宽很重要，但并非关键性因素

* 交互式应用程序

  库存查询、数据库更新 

  人机交互 

  因为用户需等待响应，所以响应时间很重要，但并非关键性因素，除非要等待很长时间

* 实时应用程序 

  VoIP、视频 

  人与人的交互 

  端到端的延时至关重要

#### 10.1.4 网络的特征

* 速度 
* 成本 
* 安全性 
* 可靠性 
* 可用性 
* 可扩展性 
* 拓扑

##### 10.1.4.1 速度(带宽)

##### 10.1.4.2 网络拓扑

拓扑结构一般是指由点和线排列成的几何图形 

计算机网络的拓扑结构是指一个网络的通信链路和计算机结点相互连接构成的几何图形

**拓扑分类** 

* 物理拓扑描述了物理设备的布线方式

* 逻辑拓扑描述了信息在网络中流动的方式

**拓扑结构分类**

* 总线拓扑：所有设备均可接收信号 
* 星型拓扑：通过中心点传输，单一故障点 
* 扩展星型拓扑：比星型拓扑的复原能力更强 
* 环拓扑：信号绕环传输，单一故障点 
* 双环拓扑：信号沿相反方向传输，比单环的复原能力更强 
* 全网状拓扑：容错能力强，实施成本高 
* 部分网状拓扑：在容错能力与成本之间寻求平衡

#### 10.1.5 网络标准

##### 10.1.5.1 网络标准和分层

旧模型：专有产品，由一个厂商控制应用程序和嵌入的软件 

基于标准的模型：多厂商软件，分层方法

**层次划分的必要性** 

计算机网络是由许多硬件、软件和协议交织起来的复杂系统。由于网络设计十分复杂，如何设计、组织 和实现计算机网络是一个挑战，必须要采用科学有效的方法

**层次划分的方法** 

* 网络的第一层应当具有相对独立的功能 
* 梳理功能之间的关系，使一个功能可以为实现另一个功能提供必要的服务，从而形成系统的层次结 构 
* 为提高系统的工作效率，相同或相近的功能仅在一个层次中实现，而且尽可能在较高的层次中实现 
* 每一层只为相邻的上一层提供服务

**层次划分的优点**

* 各层之间相互独立，每一层只实现一种相对独立的功能，使问题复杂程度降低 
* 灵活性好，各层内部的操作不会影响其他层 
* 结构上可分割开，各层之间都可以采用最合适的技术来实现 
* 易于实现和维护，因为整个系统已被分解成相对独立的子系统 
* 能促进标准化工作，因为每一层的功能及其提供的服务都有了精确的说明

##### 10.1.5.2 开放系统互联 OSI

| 层级 |    名称    |   专业词汇   |
| :--: | :--------: | :----------: |
|  7   |   应用层   | application  |
|  6   |   表示层   | presentation |
|  5   |   会话层   |   session    |
|  4   |   传输层   |  transport   |
|  3   |   网络层   |   network    |
|  2   | 数据链路层 |  data link   |
|  1   |   物理层   |   physical   |

在制定计算机网络标准方面，起着重大作用的两大国际组织是：国际电信联盟电信标准化部门，与国际 标准组织（ISO），虽然它们工作领域不同，但随着科学技术的发展，通信与信息处理之间的界限开始 变得比较模糊，这也成了国际电信联盟电信标准化部门和ISO共同关心的领域。1984年，ISO发布了著 名的OSI(Open System Interconnection)标准，它定义了网络互联的7层框架，物理层、数据链路层、 网络层、传输层、会话层、表示层和应用层），即OSI开放系统互连参考模型

**OSI 模型的七层结构**

* 第7层 应用层 应用层（Application Layer）提供为应用软件而设的接口，以设置与另一应用软件之间的通信。例如:  HTTP、HTTPS、FTP、TELNET、SSH、SMTP、POP3、MySQL等 

* 第6层 表示层 主条目：表示层（Presentation Layer）把数据转换为能与接收者的系统格式兼容并适合传输的格式 
* 第5层 会话层 会话层（Session Layer）负责在数据传输中设置和维护电脑网络中两台电脑之间的通信连接。 
* 第4层 传输层 传输层（Transport Layer）把传输表头（TH）加至数据以形成数据包。传输表头包含了所使用的协议 等发送信息。例如:传输控制协议（TCP）等。 
* 第3层 网络层 网络层（Network Layer）决定数据的路径选择和转寄，将网络表头（NH）加至数据包，以形成报文。 网络表头包含了网络数据。例如:互联网协议（IP）等。 
* 第2层 数据链接层 数据链路层（Data Link Layer）负责网络寻址、错误侦测和改错。当表头和表尾被加至数据包时，会形 成信息框（Data Frame）。数据链表头（DLH）是包含了物理地址和错误侦测及改错的方法。数据链 表尾（DLT）是一串指示数据包末端的字符串。例如以太网、无线局域网（Wi-Fi）和通用分组无线服务 （GPRS）等。分为两个子层：逻辑链路控制（logical link control，LLC）子层和介质访问控制 （Media access control，MAC）子层 
* 第1层 物理层 物理层（Physical Layer）在局部局域网上传送数据帧（Data Frame），它负责管理电脑通信设备和网 络媒体之间的互通。包括了针脚、电压、线缆规范、集线器、中继器、网卡、主机接口卡等

##### 10.1.5.3 网络的通信过程 

###### 10.1.5.3.1 数据封装和数据解封

###### 10.1.5.3.2 协议数据单元 PDU

PDU: Protocol Data Unit,协议数据单元是指对等层次之间传递的数据单位 

* 物理层的 PDU是数据位 bit 
* 数据链路层的 PDU是数据帧 frame 
* 网络层的PDU是数据包 packet 
* 传输层的 PDU是数据段 segment 
* 其他更高层次的PDU是消息 message

###### 10.1.5.3.3 三种通讯模式 

unicast: 单播,目标设备是一个 

broadcast: 广播,目标设备是所有 

multicast: 多播,组播,目标设备是多个

###### 10.1.5.3.4 冲突域和广播域

冲突域：两个网络设备同时发送数据,如果发生了冲突,则两个设备处于同一个冲突域,反之,则各自处于不 同的冲突域 

广播域：一个网络设备发送广播，另一个设备收到了,则两个设备处于同一个广播域,反之,则各自处于不 同的广播域

###### 10.1.5.3.5 三种通讯机制

* 单工通信：只有一个方向的通信,比如: 收音机 
* 半双工通信：通信双方都可以发送和接收信息，但不能同时发送，也不能同时接收,比如:对讲机 
* 全双工通信：通信双方可以同时发送和同时接收,比如: 手机

### 10.2 局域网 Local Area Network 

#### 10.2.1 概述 

##### 10.2.1.1 特点 

* 网络为一个单位所拥有 
* 地理范围和站点数目均有限

##### 10.2.1.2 主要功能 

* 资源共享和数据通信

##### 10.2.1.3 优点 

* 能方便地共享昂贵的外部设备、主机以及软件、数据。从一个站点可以访问全网 
* 便于系统的扩展和逐渐演变，各设备的位置可灵活的调整和改变 
* 提高系统的可靠性、可用性和易用性

##### 10.2.1.4 标准

IEEE（国际电子电气工程师协会）于1980年2月成立了局域网标准委员会（简称IEEE802委员会），专 门从事局域网标准化工作，并制定了IEEE802标准。802标准所描述的局域网参考模型只对应OSI参考模 型的数据链路层与物理层，它将数据链路层划分为逻辑链路层LLC子层和介质访问控制MAC子层. 

LLC子层负责向其上层提供服务 

MAC子层的主要功能包括数据帧的封装/卸装，帧的寻址和识别，帧的接收与发送，链路的管理，帧的 差错控制等。MAC子层的存在屏蔽了不同物理链路种类的差异性。

**局域网标准**

（1）IEEE 802.1标准

局域网体系结构、网络互连、以及网络管理和性能测试

（2）IEEE 802.2标准 

逻辑链路控制LLC子层功能与服务 

（3）IEEE 802.3标准 

带冲突检测的载波侦听多路访问CSMA/CD总线介质访问控制子层与物理层规范 

（4）IEEE 802.4标准 

令牌总线（Token Bus）介质访问控制子层与物理层规范 

（5）IEEE 802.5标准 

令牌环（Token Ring）介质访问控制子层与物理层规范 

（6）IEEE 802.6标准

城域网MAN介质访问控制子层与物理层规范

（7）IEEE 802.7标准 

宽带网络技术 

（8）IEEE 802.8标准 

光纤传输技术 

（9）IEEE 802.9标准 

综合语音与数据局域网（IVD LAN）技术 

（10）IEEE 802.10标准 

可互操作的局域网安全性规范（SILS） 

（11）IEEE 802.11标准 

无线局域网技术 

（12）IEEE 802.12标准 

优先度要求的访问控制方法 

（13）IEEE 802.13标准 

未使用 

（14）IEEE 802.14标准 

交互式电视网 

（15）IEEE 802.15标准 

无线个人局域网（WPAN）的MAC子层和物理层规范。代表技术为蓝牙（Bluetooth） 

（16）IEEE 802.16标准 

宽带无线局域网网络 

（17）IEEE802.20标准 

移动宽带无线接入系统(MBWA，Mobile Broadband Wireless Access) 

（18）IEEE 802.22标准

无线地域网络（Wireless Regional Area Networks，WRAN）

**无线网络标准** 

中国国家无线网络标准：WAPI 

Wi-Fi：无线保真（Wireless Fidelity），是Wi-Fi联盟制造商的商标做为产品的品牌认证，是一个创建于 IEEE 802.11标准的无线局域网技术，Wi-Fi联盟成立于1999年，当时的名称叫做Wireless Ethernet  Compatibility Alliance（WECA）。在2002年10月，正式改名为Wi-Fi Alliance。

```
802.11b — WiFi 1 (1999)
802.11a — WiFi 2 (1999)
802.11g — WiFi 3 (2003)
802.11n — WiFi 4 (2009)
802.11ac — WiFi 5(2014)
802.11ax — WiFi 6(2018)
```

#### 10.2.2 组网设备

##### 10.2.2.1 网络线缆和接口

###### 10.2.2.2.1 网线标准

上世纪80年代初，诞生了最早的网线标准（CAT）,这个标准一直沿用至今，主要根据带宽和传输速率来 区分，从一类网线CAT1——八类网线CAT8

1、 一类网线：主要用于传输语音，不同于数据传输主要用于八十年代初之前的电话线缆，已淘汰。

2、 二类网线：传输带宽为1MHZ，用于语音传输，最高数据传输速率4Mbps，常见于使用4Mbps规范 令牌传递协议的旧的令牌网（Token Ring），已被淘汰 

3、 三类网线：该电缆的传输带宽16MHz，用于语音传输及最高传输速率为10Mbps的数据传输，主要 用于10BASE--T，被ANSI/TIA-568.C.2作为最低使用等级 。 

4、 四类网线：该类电缆的传输频率为20MHz，用于语音传输和最高传输速率16Mbps（指的是 16Mbit/s令牌环）的数据传输，主要用于基于令牌的局域网和 10BASE-T/100BASE-T。最大网段长为 100m，采用RJ形式的连接器，未被广泛采用。 

5、 五类线：可追溯到1995年，传输带宽为100MHz，可支持10Mbps和100Mbps传输速率（虽然现实 中与理论值有一定差距），主要用于双绞线以太网（10BASE-T/100BASE-T），目前仍可使用，不过在 新网络建设中已经很难看到。 

6、 超五类线：标准于2001年被提出，传输带宽为100MHz，近距离情况下传输速率已可达 1000Mbps。它具有衰减小，串扰少，比五类线增加了近端串音功率和测试要求，所以它也成为了当前 应用最为广泛的网线 

7、 六类线：继CAT5e之后，CAT6标准被提出，传输带宽为250MHz，最适用于传输速率为1Gbps的应 用。改善了在串扰以及回波损耗方面的性能，这一点对于新一代全双工的高速网络应用而言是极重要 的，还有一个特点是在4个双绞线中间加了十字形的骨架。 

8、 超六类线：超六类线是六类线的改进版，发布于2008年，同样是ANSI/TIA-568C.2和ISO/IEC 11801 超六类/EA级标准中规定的一种双绞线电缆，主要应用于万兆位网络中。传输频率500 MHz，最大传输 速度也可达到10Gbps ，在外部串扰等方面有较大改善。 

9、 七类线：该线是ISO/IEC 11801 7类/F级标准中于2002年认可的一种双绞线，它主要为了适应万兆 以太网技术的应用和发展。但它不再是一种非屏蔽双绞线了，而是一种屏蔽双绞线，所以它的传输频率 至少可达600 MHz，传输速率可达10 Gbps。 

10、 超七类线:相对于CAT7最大区别在于，支持的频率带宽提升到了1000MHz，在国内而言，七类网 线已经有很少地方使用了，超七类就更加没有广泛的进入人们的生活，目前使用范围最广的是超五类、 六类等网线 

11、 八类线CAT8：相关标准由美国通信工业协会 （TIA）TR-43委员会在2016年正式发布，支持 2000MHz带宽，支持40Gbps以太网络，主要应用于数据中心

###### 10.2.2.2.2 网线线序和规范 

**非屏蔽式双绞线Unshielded Twisted-Pair Cable UTP** 

T568B和T568A

RJ-45 Connector 和 Jack

**UTP直通线(Straight-Through)**

**UTP交叉线(Crossover)**

**UTP 直通线和交叉线**

**双绞线针脚定义**

**注：**BI=双向数据 RX=接收数据 Receive Data TX=传送数据 Transmit Data

###### 10.2.2.2.3 光纤和接口Fiber-Optic

* Short wavelength (1000BASE-SX) 最远几百米 
* Long wavelength/long haul (1000BASE-LX/LH) 最远几公里 
* Extended distance (1000BASE-ZX) 最远上百公里

##### 10.2.2.2 网络适配器（网卡）

**作用** 

* 进行串行/并行转换 
* 数据缓存 
* 在计算机操作系统中安装设备驱动程序 
* 实现以太网协议

**类型** 

（1）按总线接口类型进行分类 

分为ISA、PCI、PCI-X 、PCMCIA、PCI-E 和USB等几种类型 

（2）按传输介质接口分类 

细同轴电缆的BNC接口网卡、粗同轴电缆AUI接口网卡、以太网双绞线RJ-45接口网卡、光纤F/O接口网 卡、无线网卡等 

（3）按传输速率（带宽）分类 

10Mbps网卡、100Mbps以太网卡、10Mbps/100Mbps自适应网卡、1000Mbps千兆以太网卡、 40Gbps自适应网卡等

##### 10.2.2.3 中继器和集线器

###### 10.2.2.3.1 中继器 repeater

实际上是一种信号再生放大器，可将变弱的信号和有失真的信号进行整形与放大，输出信号比原信号的 强度将大大提高,中继器不解释、不改变收到的数字信息，而只是将其整形放大后再转发出去

优点 

（1）易于操作 

（2）很短的等待时间 

（3）价格便宜 

（4）突破线缆的距离限制来扩展局域网段的距离 

（5）可用来连接不同的物理介质

缺点

（1）采用中继器连接网络分支的数目要受具体的网络体系结构限制 

（2）中继器不能连接不同类型的网络 

（3）中继器没有隔离和过滤功能，无路由选择、交换、纠错／检错功能，一个分支出现故障可能会影 响到其他的每一个网络分支 

（4）使用中继器扩充网络距离是最简单最廉价的方法，但当负载增加时，网络性能急剧下降，所以只 有当网络负载很轻和网络时延要求不高的条件下才能使用

###### 10.2.2.3.2 集线器 hub

集线器（Hub）工作在物理层，是中继器的一种形式，是一种集中连接缆线的网络组件，可以认为集线 器是一个多端口的中继器，集线器提供多端口连接，主要功能是对接收到的信号进行再生整形放大，以 扩大网络的传输距离，同时把所有节点集中在以它为中心的节点上 

Hub并不记忆报文是由哪个MAC地址发出，哪个MAC地址在Hub的哪个端口 

Hub的特点： 

* 共享带宽 
* 半双工

##### 10.2.3.4 网桥和交换机 

###### 10.2.3.4.1 网桥 Bridge

网桥（Bridge）也叫桥接器，是连接两个局域网的一种存储/转发设备，根据MAC地址表对数据帧进行 转发，可隔离碰撞域 

网桥将网络的多个网段在数据链路层连接起来，并对网络数据帧进行管理 

网桥的内部结构

优点 

* 过滤通信量 
* 扩大了物理范围 
* 提高了可靠性 
* 可互连不同物理层、不同 MAC 子层和不同速率（如10 Mb/s 和 100 Mb/s 以太网）的局域网

缺点 

* 存储转发增加了时延 
* 在MAC 子层并没有流量控制功能 
* 具有不同 MAC 子层的网段桥接在一起时时延更大 
* 网桥只适合于用户数不太多(不超过几百个)和通信量不太大的局域网，否则有时还会因传播过多的 广播信息而产生网络拥塞。这就是所谓的广播风暴

###### 10.2.3.4.2 交换机 switch

交换机是工作在OSI参考模型数据链路层的设备，外表和集线器相似 

它通过判断数据帧的目的MAC地址，从而将数据帧从合适端口发送出去 

交换机是通过MAC地址的学习和维护更新机制来实现数据帧的转发 

工作原理 

（1）交换机根据收到数据帧中的源MAC地址建立该地址同交换机端口的映射，并将其写入MAC地址表 中 

（2）交换机将数据帧中的目的MAC地址同已建立的MAC地址表进行比较，以决定由哪个端口进行转发 

（3）如数据帧中的目的MAC地址不在MAC地址表中，则向所有端口转发。这一过程称为泛洪 （flood） 

（4）广播帧和组播帧向所有的端口转发

###### 10.2.3.4.3 集线器与交换机的比较

（1）交换机属于数据链路层设备，而集线器属于物理层设备 

（2）集线器在转发帧时，不对传输介质进行检测，交换机在转发帧之前必须执行 CSMA/CD 算法。若 在发送过程中出现碰撞，就必须停止发送和进行退避。所以交换机能隔离冲突，而集线器却只能增加冲 突 

（3） 交换机的每个端口可提供专用的带宽，而集线器的所有端口只能共享带宽 

（4）集线器只能实现半双工传送，而交换机可支持全双工传送 

（5）集线器和交换机都无法隔离广播域

##### 10.2.3.5 路由器 router

路由：把一个数据包从一个设备发送到不同网络里的另一个设备上去。这些工作依靠路由器来完成。路 由器只关心网络的状态和决定网络中的最佳路径。路由的实现依靠路由器中的路由表来完成 

路由器功能： 

* 工作在网络层 
* 分隔广播域和冲突域 
* 选择路由表中到达目标最好的路径 
* 维护和检查路由信息 
* 连接广域网

#### 10.2.3 以太网技术

##### 10.2.3.1 概述

以太网（Ethernet）是一种产生较早且使用相当广泛的局域网，由美国Xerox（施乐）公司的Palo Alto 研究中心（简称为PARC）于20世纪70年代初期开始研究并于1975年研制成功

##### 10.2.3.2 以太网MAC帧格式

##### 10.2.3.3 MAC地址

在局域网中，硬件地址又称为物理地址或MAC地址（因为这种地址用在MAC帧中） 

IEEE 802标准为局域网规定了一种48位的全球地址（一般都简称为“地址”)，是局域网中每一台计算机固 化在网卡ROM中的地址 

IEEE 的注册管理机构 RA 负责向厂家分配地址字段的前三个字节(即高位 24 位) 

地址字段中的后三个字节(即低位 24 位)由厂家自行指派，称为扩展标识符，必须保证生产出的适配器没 有重复地址

**各大厂商MAC识别码:**

```
http://standards-oui.ieee.org/oui/oui.txt
```

##### 10.2.3.4 冲突检测的载波侦听多路访问 CSMA/CD

工作原理 

* 先听后发 
* 边发边听 
* 冲突停止 
* 延迟重发

#### 10.2.4 虚拟局域网 VLAN

##### 10.2.4.1 VLAN 原理

虚拟局域网 VLAN 是由一些局域网网段构成的与物理位置无关的逻辑组 

这些网段具有某些共同的需求。每一个 VLAN 的帧都有一个明确的标识符，指明发送这个帧的工作站是 属于哪一个 VLAN。虚拟局域网其实只是局域网给用户提供的一种服务，而并不是一种新型局域网 

优点 

（1）更有效地共享网络资源。如果用交换机构成较大的局域网，大量的广播报文就会使网络性能下 降。VLAN能将广播报文限制在本VLAN范围内，从而提升了网络的效能 

（2）简化网络管理。当结点物理位置发生变化时，如跨越多个局域网，通过逻辑上配置VLAN即可形成 网络设备的逻辑组，无需重新布线和改变IP地址等。这些逻辑组可以跨越一个或多个二层交换机 

（3）提高网络的数据安全性。一个VLAN中的结点接收不到另一个VLAN中其他结点的帧 虚拟局域网的实现技术 

* 基于端口的VLAN 
* 基于MAC地址的VLAN 
* 基于协议的VLAN 
* 基于网络地址的VLAN

##### 10.2.4.2 IEEE 802.1Q 帧结构

**VLAN 标签各字段含义** 

TPID：Tag Protocol Identifier（标签协议标识符），2Byte，表示帧类型，取值为0x8100时表示IEEE  802.1Q的VLAN数据帧。如果不支持802.1Q的设备收到这样的帧，会将其丢弃，各设备厂商可以自定义 该字段的值。当邻居设备将TPID值配置为非0x8100时， 为了能够识别这样的报文，实现互通，必须在 本设备上修改TPID值，确保和邻居设备的TPID值配置一致 

PRI：Priority，3bit，表示数据帧的802.1p（是IEEE 802.1Q的扩展协议）优先级。取值范围为0～7， 值越大优先级越高。当网络阻塞时，交换机优先发送优先级高的数据帧 

CFI：Canonical Format Indicator（标准格式指示位），1bit,表示MAC地址在不同的传输介质中是否以 标准格式进行封装，用于兼容以太网和令牌环网。CFI取值为0表示MAC地址以标准格式进行封装，为1 表示以非标准格式封装。在以太网中，CFI的值为0 

VID：VLAN ID，12bit，表示该数据帧所属VLAN的编号。VLAN ID取值范围是0～4095。由于0和4095 为协议保留取值，所以VLAN ID的有效取值范围是1～4094

#### 10.2.5 分层的网络架构

### 10.3 TCP/IP 协议栈 

#### 10.3.1 TCP/IP 标准 

##### 10.3.1.1 TCP/IP 介绍

Transmission Control Protocol/Internet Protocol 传输控制协议/因特网互联协议 TCP/IP是一个Protocol Stack，包括TCP、IP、UDP、ICMP、RIP、TELNET、FTP、SMTP、ARP等许多 协议 

最早发源于1969年美国国防部（缩写为DoD）的因特网的前身ARPA网络项目，1983年1月1日，TCP/IP 取代了旧的网络控制协议NCP，成为今天的互联网和局域网的基石和标准,由互联网工程任务组负责维护 

国防高级研究计划局DARPA与BBN技术公司、斯坦福大学和伦敦大学学院签约，在多个硬件平台上开发 协议的操作版本。 在协议开发过程中，数据包路由层的版本号从版本 1 进展到版本 4，后者于 1983 年 安装在 ARPANET 中。它被称为互联网协议版本4（IPv4）作为协议，仍在互联网使用，连同其目前的 继承，互联网协议版本6（IPv6）。 

RFC 文档: https://www.ietf.org/rfc/rfc1180.html

##### 10.3.1.2 TCP/IP 分层

共定义了四层，和 OSI参考模型的分层有对应关系 

RFC文档: https://www.ietf.org/rfc/rfc1122#section-1.3.3 

RFC官方分为四层: 

* Application Layer 
* Transport Layer 
* Internet Layer 
* Link Layer(media-access)

##### 10.3.1.3 TCP/IP 通信过程

##### 10.3.1.4 TCP/IP和OSI模型的比较

* 相同点 

  两者都是以协议栈的概念为基础 

  协议栈中的协议彼此相互独立 

  下层对上层提供服务 

* 不同点 

  OSI是先有模型；TCP/IP是先有协议，后有模型 

  OSI是国际标准，适用于各种协议栈；TCP/IP实际标准，只适用于TCP/IP网络 

  层次数量不同

#### 10.3.2 transport 层

##### 10.3.2.1 TCP Transmission Control Protocol 

###### 10.3.2.1.1 TCP特性

工作在传输层 

面向连接协议 

全双工协议 

半关闭 

错误检查 

将数据打包成段，排序 

确认机制 

数据恢复，重传 

流量控制，滑动窗口 

拥塞控制，慢启动和拥塞避免算法 

@更多关于tcp的内核参数，可参看man 7 tcp

###### 10.3.2.1.2 TCP包头结构

**TCP 包头**

* 源端口、目标端口：计算机上的进程要和其他进程通信是要通过计算机端口的，而一个计算机端口 某个时刻只能被一个进程占用，所以通过指定源端口和目标端口，就可以知道是哪两个进程需要通 信。源端口、目标端口是用16位表示的，可推算计算机的端口个数为2^16个,即65536 
* 序列号：表示本报文段所发送数据的第一个字节的编号。在TCP连接中所传送的字节流的每一个字 节都会按顺序编号。由于序列号由32位表示，所以每2^32个字节，就会出现序列号回绕，再次从 0 开始 
* 确认号：表示接收方期望收到发送方下一个报文段的第一个字节数据的编号。也就是告诉发送方： 我希望你（指发送方）下次发送的数据的第一个字节数据的编号为此确认号 
* 数据偏移：表示TCP报文段的首部长度，共4位，由于TCP首部包含一个长度可变的选项部分，需 要指定这个TCP报文段到底有多长。它指出 TCP 报文段的数据起始处距离 TCP 报文段的起始处有 多远。该字段的单位是32位(即4个字节为计算单位），4位二进制最大表示15，所以数据偏移也就 是TCP首部最大60字节 
* URG：表示本报文段中发送的数据是否包含紧急数据。后面的紧急指针字段（urgent pointer）只 有当URG=1时才有效 
* ACK：表示是否前面确认号字段是否有效。只有当ACK=1时，前面的确认号字段才有效。TCP规 定，连接建立后，ACK必须为1,带ACK标志的TCP报文段称为确认报文段 
* PSH：提示接收端应用程序应该立即从TCP接收缓冲区中读走数据，为接收后续数据腾出空间。如 果为1，则表示对方应当立即把数据提交给上层应用，而不是缓存起来，如果应用程序不将接收到 的数据读走，就会一直停留在TCP接收缓冲区中 
* RST：如果收到一个RST=1的报文，说明与主机的连接出现了严重错误（如主机崩溃），必须释放 连接，然后再重新建立连接。或者说明上次发送给主机的数据有问题，主机拒绝响应，带RST标志 的TCP报文段称为复位报文段 
* SYN：在建立连接时使用，用来同步序号。当SYN=1，ACK=0时，表示这是一个请求建立连接的报 文段；当SYN=1，ACK=1时，表示对方同意建立连接。SYN=1，说明这是一个请求建立连接或同 意建立连接的报文。只有在前两次握手中SYN才置为1，带SYN标志的TCP报文段称为同步报文段 
* FIN：表示通知对方本端要关闭连接了，标记数据是否发送完毕。如果FIN=1，即告诉对方：“我的 数据已经发送完毕，你可以释放连接了”，带FIN标志的TCP报文段称为结束报文段 
* 窗口大小：表示现在允许对方发送的数据量，也就是告诉对方，从本报文段的确认号开始允许对方 发送的数据量，达到此值，需要ACK确认后才能再继续传送后面数据，由Window size value *  Window size scaling factor（此值在三次握手阶段TCP选项Window scale协商得到）得出此值 
* 校验和：提供额外的可靠性

* 紧急指针：标记紧急数据在数据字段中的位置 
* 选项部分：其最大长度可根据TCP首部长度进行推算。TCP首部长度用4位表示，选项部分最长 为：(2^4-1)*4-20=40字节

**TCP包头常见选项：**

* 最大报文段长度MSS（Maximum Segment Size），通常1460字节 

  指明自己期望对方发送TCP报文段时那个数据字段的长度。比如：1460字节。数据字段的长度加 上TCP首部的长度才等于整个TCP报文段的长度。MSS不宜设的太大也不宜设的太小。若选择太 小，极端情况下，TCP报文段只含有1字节数据，在IP层传输的数据报的开销至少有40字节（包括 TCP报文段的首部和IP数据报的首部）。这样，网络的利用率就不会超过1/41。若TCP报文段非常 长，那么在IP层传输时就有可能要分解成多个短数据报片。在终点要把收到的各个短数据报片装配 成原来的TCP报文段。当传输出错时还要进行重传，这些也都会使开销增大。因此MSS应尽可能 大，只要在IP层传输时不需要再分片就行。在连接建立过程中，双方都把自己能够支持的MSS写入 这一字段。 MSS只出现在SYN报文中。即：MSS出现在SYN=1的报文段中 MTU和MSS值的关系：MTU=MSS+IP Header+TCP Header 通信双方最终的MSS值=较小MTU-IP Header-TCP Header 

* 窗口扩大 Window Scale 

  为了扩大窗口，由于TCP首部的窗口大小字段长度是16位，所以其表示的最大数是65535。但是随 着时延和带宽比较大的通信产生（如卫星通信），需要更大的窗口来满足性能和吞吐率，所以产生 了这个窗口扩大选项 

* 时间戳 Timestamps 

  可以用来计算RTT(往返时间)，发送方发送TCP报文时，把当前的时间值放入时间戳字段，接收方 收到后发送确认报文时，把这个时间戳字段的值复制到确认报文中，当发送方收到确认报文后即可 计算出RTT。也可以用来防止回绕序号PAWS，也可以说可以用来区分相同序列号的不同报文。因 为序列号用32为表示，每2^32个序列号就会产生回绕，那么使用时间戳字段就很容易区分相同序 列号的不同报文

###### 10.3.2.1.3 TCP协议PORT

传输层通过port号，确定应用层协议，范围0-65535 

维基百科：https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers 

IANA互联网数字分配机构负责域名，数字资源，协议分配

* 0-1023：系统端口或特权端口(仅管理员可用) ，众所周知，永久的分配给固定的系统应用使用， 22/tcp(ssh), 80/tcp(http), 443/tcp(https) 
* 1024-49151：用户端口或注册端口，但要求并不严格，分配给程序注册为某应用使用， 1433/tcp(SqlServer), 1521/tcp(oracle),3306/tcp(mysql),11211/tcp/udp (memcached) 
* 49152-65535：动态或私有端口，客户端随机使用端口，范围定 义：/proc/sys/net/ipv4/ip_local_port_range

###### 10.3.2.1.4 三次握手和四次挥手

###### 10.3.2.1.5 有限状态机 FSM:Finite State Machine

* CLOSED 没有任何连接状态 
* LISTEN 侦听状态，等待来自远方TCP端口的连接请求 
* SYN-SENT 在发送连接请求后，等待对方确认 
* SYN-RECEIVED 在收到和发送一个连接请求后，等待对方确认 
* ESTABLISHED 代表传输连接建立，双方进入数据传送状态 
* FIN-WAIT-1 主动关闭,主机已发送关闭连接请求，等待对方确认 
* FIN-WAIT-2 主动关闭,主机已收到对方关闭传输连接确认，等待对方发送关闭传输连接请求 
* TIME-WAIT 完成双向传输连接关闭，等待所有分组消失 
* CLOSE-WAIT 被动关闭,收到对方发来的关闭连接请求，并已确认 
* LAST-ACK 被动关闭,等待最后一个关闭传输连接确认，并等待所有分组消失 
* CLOSING 双方同时尝试关闭传输连接，等待对方确认

客户端先发送一个FIN给服务端，自己进入FIN_WAIT_1状态，这时等待接收服务端报文，该报文会有三 种可能： 

* 只有服务端的ACK 
* 只有服务端的FIN 
* 基于服务端的ACK，又有FIN

1、只收到服务器的ACK，客户端会进入FIN_WAIT_2状态，后续当收到服务端的FIN时，回应发送一个 ACK，会进入到TIME_WAIT状态，这个状态会持续2MSL(TCP报文段在网络中的最大生存时间, RFC  1122标准的建议值是2min).客户端等待2MSL，是为了当最后一个ACK丢失时，可以再发送一次。因为 服务端在等待超时后会再发送一个FIN给客户端，进而客户端知道ACK已丢失 

2、只有服务端的FIN时，回应一个ACK给服务端，进入CLOSING状态，然后接收到服务端的ACK时，进 入TIME_WAIT状态 

3、同时收到服务端的ACK和FIN，直接进入TIME_WAIT状态

**客户端的典型状态转移** 

客户端通过connect系统调用主动与服务器建立连接connect系统调用首先给服务器发送一个同步报文 段，使连接转移到SYN_SENT状态 

此后connect系统调用可能因为如下两个原因失败返回： 

1、如果connect连接的目标端口不存在（未被任何进程监听），或者该端口仍被处于TIME_WAIT状态 的连接所占用，则服务器将给客户端发送一个复位报文段，connect调用失败。 

2、如果目标端口存在，但connect在超时时间内未收到服务器的确认报文段，则connect调用失败。 connect调用失败将使连接立即返回到初始的CLOSED状态。如果客户端成功收到服务器的同步报文段 和确认，则connect调用成功返回，连接转移至ESTABLISHED状态 

当客户端执行主动关闭时，它将向服务器发送一个结束报文段，同时连接进入FIN_WAIT_1状态。若此 时客户端收到服务器专门用于确认目的的确认报文段，则连接转移至FIN_WAIT_2状态。当客户端处于 FIN_WAIT_2状态时，服务器处于CLOSE_WAIT状态，这一对状态是可能发生半关闭的状态。此时如果服 务器也关闭连接（发送结束报文段），则客户端将给予确认并进入TIME_WAIT状态 客户端从FIN_WAIT_1状态可能直接进入TIME_WAIT状态（不经过FIN_WAIT_2状态），前提是处于 FIN_WAIT_1状态的服务器直接收到带确认信息的结束报文段（而不是先收到确认报文段，再收到结束 报文段） 

处于FIN_WAIT_2状态的客户端需要等待服务器发送结束报文段，才能转移至TIME_WAIT状态，否则它 将一直停留在这个状态。如果不是为了在半关闭状态下继续接收数据，连接长时间地停留在 FIN_WAIT_2状态并无益处。连接停留在FIN_WAIT_2状态的情况可能发生在：客户端执行半关闭后，未 等服务器关闭连接就强行退出了。此时客户端连接由内核来接管，可称之为孤儿连接（和孤儿进程类 似）

Linux为了防止孤儿连接长时间存留在内核中，定义了两个内核参数：

```bash
/proc/sys/net/ipv4/tcp_max_orphans #指定内核能接管的孤儿连接数目
/proc/sys/net/ipv4/tcp_fin_timeout #指定孤儿连接在内核中生存的时间
```

**客户机端的三次握手和四次挥手状态转换**

**服务器端的三次握手和四次挥手状态转换**

###### 10.3.2.1.6 sync半连接和accept全连接队列

```bash
/proc/sys/net/ipv4/tcp_max_syn_backlog    #未完成连接队列大小，默认值128,建议调整大小为1024以上
/proc/sys/net/core/somaxconn      #完成连接队列大小，默认值128,建议调整大小为1024以上
```

###### 10.3.2.1.7 TCP超时重传

异常网络状况下（开始出现超时或丢包），TCP控制数据传输以保证其承诺的可靠服务 TCP服务必须能够重传超时时间内未收到确认的TCP报文段。为此，TCP模块为每个TCP报文段都维护一 个重传定时器，该定时器在TCP报文段第一次被发送时启动。如果超时时间内未收到接收方的应答， TCP模块将重传TCP报文段并重置定时器。至于下次重传的超时时间如何选择，以及最多执行多少次重 传，就是TCP的重传策略 

与TCP超时重传相关的两个内核参数：

```bash
/proc/sys/net/ipv4/tcp_retries1 #指定在底层IP接管之前TCP最少执行的重传次数，默认值是3
/proc/sys/net/ipv4/tcp_retries2 #指定连接放弃前TCP最多可以执行的重传次数，默认值15（一般对应13～30min）
```

###### 10.3.2.1.8 拥塞控制

网络中的带宽、交换结点中的缓存和处理机等，都是网络的资源。在某段时间，若对网络中某一资源的 需求超过了该资源所能提供的可承受的能力，网络的性能就会变坏。此情况称为拥塞 

TCP为提高网络利用率，降低丢包率，并保证网络资源对每条数据流的公平性。即所谓的拥塞控制 TCP拥塞控制的标准文档是RFC 5681，其中详细介绍了拥塞控制的四个部分：慢启动（slow start）、 拥塞避免（congestion avoidance）、快速重传（fast retransmit）和快速恢复（fast recovery）。

拥 塞控制算法在Linux下有多种实现，比如reno算法、vegas算法和cubic算法等。它们或者部分或者全部 实现了上述四个部分 

当前所使用的拥塞控制算法

```
/proc/sys/net/ipv4/tcp_congestion_control
```

3.2.1.9 内核TCP参数优化

参看帮助： man 7 tcp 

编辑文件/etc/sysctl.conf，加入以下内容：然后执行 sysctl -p 让参数生效。

```bash
net.ipv4.tcp_fin_timeout = 2
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_keepalive_time = 600
net.ipv4.ip_local_port_range = 2000 65000
net.ipv4.tcp_max_syn_backlog = 16384
net.ipv4.tcp_max_tw_buckets = 36000
net.ipv4.route.gc_timeout = 100
net.ipv4.tcp_syn_retries = 1
net.ipv4.tcp_synack_retries = 1
net.ipv4.tcp_max_orphans = 16384
net.core.somaxconn = 16384
net.core.netdev_max_backlog = 16384
```

作用说明： 

* net.ipv4.tcp_fin_timeout 表示套接字由本端要求关闭，这个参数决定了它保持在FIN-WAIT-2状态 的时间，默认值是60秒。 该参数对应系统路径为：/proc/sys/net/ipv4/tcp_fin_timeout 60 
* net.ipv4.tcp_tw_reuse 表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认值 为0，表示关闭。 该参数对应系统路径为：/proc/sys/net/ipv4/tcp_tw_reuse 0  
* net.ipv4.tcp_tw_recycle 表示开启TCP连接中TIME-WAIT sockets的快速回收。 该参数对应系统路 径为：/proc/sys/net/ipv4/tcp_tw_recycle，默认为0，表示关闭。 提示：reuse和recycle这两个 参数是为防止生产环境下服务器time_wait网络状态数量过多设置的。 
* net.ipv4.tcp_syncookies 表示开启SYN Cookies功能。当出现SYN等待队列溢出时，启用Cookies 来处理，可防范少量SYN攻击，这个参数也可以不添加。 该参数对应系统路径 为：/proc/sys/net/ipv4/tcp_syncookies,默认值为1 
* net.ipv4.tcp_keepalive_time 表示当keepalive启用时，TCP发送keepalive消息的频度。默认是2 小时，建议改为10分钟。 该参数对应系统路径为：/proc/sys/net/ipv4/tcp_keepalive_time,默认为7200秒。

* net.ipv4.ip_local_port_range 该选项用来设定允许系统打开的端口范围，即用于向外连接的端口 范围。 该参数对应系统路径为：/proc/sys/net/ipv4/ip_local_port_range 32768 61000 
* net.ipv4.tcp_max_syn_backlog 表示SYN队列的长度，即半连接队列长度，默认为1024，建议加 大队列的长度为8192或更多，这样可以容纳更多等待连接的网络连接数。 该参数为服务器端用于 记录那些尚未收到客户端确认信息的连接请求最大值。 该参数对象系统路径 为：/proc/sys/net/ipv4/tcp_max_syn_backlog 
* net.ipv4.tcp_max_tw_buckets 表示系统同时保持TIME_WAIT套接字的最大数量，如果超过这个数 值，TIME_WAIT套接字将立刻被清除并打印警告信息。 默认为180000，对于Apache、Nginx等服 务器来说可以将其调低一点，如改为5000~30000，不通业务的服务器也可以给大一点，比如 LVS、Squid。 此项参数可以控制TIME_WAIT套接字的最大数量，避免Squid服务器被大量的 TIME_WAIT套接字拖死。 该参数对应系统路径为：/proc/sys/net/ipv4/tcp_max_tw_buckets 
* net.ipv4.tcp_synack_retries 参数的值决定了内核放弃连接之前发送SYN+ACK包的数量。 该参数 对应系统路径为：/proc/sys/net/ipv4/tcp_synack_retries,默认值为5 
* net.ipv4.tcp_syn_retries 表示在内核放弃建立连接之前发送SYN包的数量。 该参数对应系统路径 为：/proc/sys/net/ipv4/tcp_syn_retries,默认值为6 
* net.ipv4.tcp_max_orphans 用于设定系统中最多有多少个TCP套接字不被关联到任何一个用户文 件句柄上。 如果超过这个数值，孤立连接将被立即被复位并打印出警告信息。 这个限制只有为了 防止简单的DoS攻击。不能过分依靠这个限制甚至认为减少这个值，更多的情况是增加这个值。 该参数对应系统路径为：/proc/sys/net/ipv4/tcp_max_orphans ，默认值8192 
* net.core.somaxconn 同时发起的TCP的最大连接数，即全连接队列长度，在高并发请求中，可能 会导致链接超时或重传，一般结合并发请求数来调大此值。 该参数对应系统路径 为：/proc/sys/net/core/somaxconn ，默认值是128 
* net.core.netdev_max_backlog 表示当每个网络接口接收数据包的速率比内核处理这些包的速率 快时，允许发送到队列的数据包最大数。 该参数对应系统路径 为：/proc/sys/net/core/netdev_max_backlog，默认值为1000

##### 10.3.2.2 UDP User Datagram Protocol

###### 10.3.2.2.1 UDP特性 

工作在传输层 

提供不可靠的网络访问 

非面向连接协议 

有限的错误检查 

传输性能高 

无数据恢复特性 

更多关于udp的内核参数，可参看man 7 udp

###### 10.3.2.2.2 UDP包头

#### 10.3.3 Internet层

##### 10.3.3.1 Internet Control Message Protocol

##### 10.3.3.2 Address Resolution Protocol

###### 10.3.2.2.1 ARP

ARP 地址解析协议由互联网工程任务组（IETF）在1982年11月发布的RFC 826中描述制定，是根据IP地 址获取物理地址的一个TCP/IP协议。 

主机发送信息时将包含目标IP地址的ARP请求广播到局域网络上的所有主机，并接收返回消息，以此确 定目标的物理地址；收到返回消息后将该IP地址和物理地址存入本机ARP缓存中并保留一定时间，下次 请求时直接查询ARP缓存以节约资源。地址解析协议是建立在网络中各个主机互相信任的基础上的，局 域网络上的主机可以自主发送ARP应答消息，其他主机收到应答报文时不会检测该报文的真实性就会将 其记入本机ARP缓存

**同网段的ARP**

**跨网段的ARP**

###### 10.3.2.2.2 Gratuitous ARP

Gratuitous ARP也称为免费ARP，无故ARP。Gratuitous ARP 不同于一般的ARP请求，它并非期待得到 ip对应的mac地址，而是当主机启动的时候，将发送一个Gratuitous arp请求，即请求自己的ip地址的 mac地址 

免费ARP可以有两个方面的作用： 

* 验证IP是否冲突：一个主机可以通过它来确定另一个主机是否设置了相同的 IP地址 
* 更换物理网卡：如果发送ARP的主机正好改变了物理地址（如更换物理网卡），可以使用此方法通 知网络中其它主机及时更新ARP缓存

##### 10.3.3.3 Reverse Address Resolution Protocol

RARP 即将MAC转换成IP

##### 10.3.3.4 internet 协议

###### 10.3.3.4.1 Internet 协议特征

* 运行于 OSI 网络层 
* 面向无连接的协议 
* 独立处理数据包 
* 分层编址 
* 尽力而为传输 
* 无数据恢复功能

###### 10.3.3.4.2 IP PDU 报头

**IP PDU 报头格式** 

* 版本:占4位,指 IP 协议的版本目前的IP协议版本号为4 

* 首部长度:占4位,可表示的最大数值是15个单位，一个单位为4字节，因此IP 的首部长度的最大值是 60字节

​		区分服务:占8位,用来获得更好的服务,在旧标准中叫做服务类型,但实际上一直未被使用过.后改名为 

​		区分服务.只有在使用区分服务(DiffServ)时,这个字段才起作用.一般的情况下不使用

* 总长度:占16位,指首部和数据之和的长度,单位为字节,因此数据报的最大长度为 65535 字节.总长度 必须不超过最大传送单元 MTU 

* 标识:占16位,它是一个计数器,通常，每发送一个报文，该值会加1， 也用于数据包分片，在同一个 包的若干分片中，该值是相同的 

* 标志(flag):占3位,目前只有后两位有意义 

  DF： Don’t Fragment 中间的一位，只有当 DF=0 时才允许分片 

  MF： More Fragment 最后一位，MF=1表示后面还有分片,MF=0 表示最后一个分片 

  IP PDU 报头 

* 片偏移:占13位,指较长的分组在分片后，该分片在原分组中的相对位置.片偏移以8个字节为偏移单 位 

* 生存时间:占8位,记为TTL (Time To Live) 数据报在网络中可通过的路由器数的最大值,TTL 字段是由 发送端初始设置一个 8 bit字段.推荐的初始值由分配数字 RFC 指定,当前值为 64.发送 ICMP 回显应 答时经常把 TTL 设为最大值 255 

* 协议:占8位,指出此数据报携带的数据使用何种协议以便目的主机的IP层将数据部分上交给哪个处理 过程, 1表示为 ICMP 协议, 2表示为 IGMP 协议, 6表示为 TCP 协议, 17表示为 UDP 协议 

* 首部检验和:占16位,只检验数据报的首部不检验数据部分.这里不采用 CRC 检验码而采用简单的计 算方法 

* 源地址和目的地址:都各占4字节,分别记录源地址和目的地址

#### 10.3.4 主机到主机的包传递完整过程

#### 10.3.5 IP地址

##### 10.3.5.1 IP地址组成

它们可唯一标识 IP 网络中的每台设备 ，每台主机（计算机、网络设备、外围设备）必须具有唯一的地 址 

IP地址由两部分组成

* 网络 ID：标识网络，每个网段分配一个网络ID，处于高位 
* 主机 ID：标识单个主机，由组织分配给各设备，处于低位

##### 10.3.5.2 IP地址分类

A类： 

​	0 0000000 - 0 1111111.X.Y.Z : 0-127.X.Y.Z 

​	网络ID位是最高8位,主机ID是24位低位 

​	网络数：126=2^7(可变是的网络ID位数)-2 

​	每个网络中的主机数：2^24-2=16777214 

​	默认子网掩码：255.0.0.0 

​	私网地址：10.0.0.0 

​	范例:114.114.114.114,8.8.8.8,1.1.1.1,123.56.174.200,119.29.29.29 

B类： 

​	10 000000 - 10 111111.X.Y.Z：128-191.X.Y.Z 

​	网络ID位是最高16位,主机ID是16位低位 

​	网络数：2^14=16384 

​	每个网络中的主机数：2^16-2=65534 

​	默认子网掩码：255.255.0.0 

​	私网地址：172.16.0.0-172.31.0.0 

​	范例:180.76.76.76，172.16.0.1

C类： 

​	110 0 0000 - 110 1 1111.X.Y.Z: 192-223.X.Y.Z 

​	网络ID位是最高24位,主机ID是8位低位 

​	网络数：2^21=2097152 

​	每个网络中的主机数：2^8-2=254 

​	默认子网掩码：255.255.255.0 

​	私网地址：192.168.0.0-192.168.255.0 

​	范例: 223.6.6.6,223.5.5.5 

D类：组（多）播，1110 0000 - 1110 1111.X.Y.Z: 224-239.X.Y.Z 

E类：保留未使用，240-255

##### 10.3.5.3 公共和私有IP地址

私有IP地址：不直接用于互联网，通常在局域网中使用

|  类  |         私有地址范围         |
| :--: | :--------------------------: |
|  A   |   10.0.0.0到10.255.255.255   |
|  B   |  172.16.0.0到172.31.255.255  |
|  C   | 192.168.0.0到192.168.255.255 |

公共IP地址：互联网上设备拥有的唯一地址

|  类  |                      公共IP地址范围                      |
| :--: | :------------------------------------------------------: |
|  A   |    1.0.0.0到9.255.255.255；11.0.0.0到126.255.255.255     |
|  B   |  128.0.0.0到172.15.255.255；172.32.0.0到191.255.255.255  |
|  C   | 192.0.0.0到192.167.255.255；192.169.0.0到223.255.255.255 |

##### 10.3.5.4 特殊地址

* 0.0.0.0 

  0.0.0.0不是一个真正意义上的IP地址。它表示所有不清楚的主机和目的网络 

* 255.255.255.255 

  限制广播地址。对本机来说，这个地址指本网段内(同一广播域)的所有主机 

* 127.0.0.1～127.255.255.254 

  本机回环地址，主要用于测试。在传输介质上永远不应该出现目的地址为“127.0.0.1”的 数据包 

* 224.0.0.0到239.255.255.255 

  组播地址，224.0.0.1特指所有主机，224.0.0.2特指所有路由器。224.0.0.5指OSPF 路由器，地址 多用于一些特定的程序以及多媒体程序 

* 169.254.x.x 

  如果Windows主机使用了DHCP自动分配IP地址，而又无法从DHCP服务器获取地址，系统会为主 机分配这样地址

##### 10.3.5.5 保留地址

##### 10.3.5.6 子网掩码

CIDR：无类域间路由，目前的网络已不再按A，B，C类划分网段，可以任意指定网段的范围 

CIDR 无类域间路由表示法：IP/网络ID位数，如：172.16.0.100/16 

netmask子网掩码：32位或128位（IPv6）的数字，和IP成对使用，用来确认IP地址中的网络ID和主机 ID，对应网络ID的位为1，对应主机ID的位为0,范例:255.255.255.0 ，表现为连续的高位为1，连续的低 位为0

**相关公式：**

* 一个网络的最多的主机数＝2^主机ID位数-2 
* 网络（段）数=2^网络ID中可变的位数 
* 网络ID=IP与netmask

**判断对方主机是否在同一个网段：** 

用自已的子网掩码分别和自已的IP及对方的IP相与，比较结果，相同则同一网络，不同则不同网段 

范例:  

netmask: 255.255.224.0,网络ID位:19 主机ID位:13,主机数=2^13-2=8190 

范例：判断A和B是否在网一个网段? 

A: 192.168.1.100 netmask:255.255.255.0 

B: 192.168.2.100 netmask:255.255.0.0

##### 10.3.5.7 划分子网

划分子网：将一个大的网络（主机数多）划分成多个小的网络（主机数少），主机ID位数变少，网络ID 位数变多，网络ID位向主机ID位借n位,将划分2^n个子网

##### 10.3.5.8 优化IP地址分配

合并超网：将多个小网络合并成一个大网，主机ID位向网络ID位借位

##### 10.3.5.9 跨网络通信

跨网络通信：路由,选择路径 

路由分类： 

* 主机路由 
* 网络路由 
* 默认路由

优先级：精度越高，优先级越高

##### 10.3.5.10 动态主机配置协议 DHCP

### 10.4 网络配置

#### 10.4.1 基本网络配置

将Linux主机接入到网络，需要配置网络相关设置 

一般包括如下内容： 

* 主机名 

* IP/netmask 

* 路由：默认网关 

* DNS服务器 

  主DNS服务器 

  次DNS服务器 

  第三个DNS服务器

#### 10.4.2 CentOS 6 之前版本网卡名称

接口命名方式：CentOS 6

```
以太网：eth[0,1,2,...]
ppp：ppp[0,1,2,...]
```

网络接口识别并命名相关的udev配置文件：

```
/etc/udev/rules.d/70-persistent-net.rules
```

查看网卡：

```
dmesg |grep –i eth
ethtool -i eth0
```

卸载网卡驱动：

```
modprobe -r e1000
rmmod e1000
```

装载网卡驱动：

```
modprobe -r e1000 rmmod e1000
```

#### 10.4.3 网络配置命令

##### 10.4.3.1 网络配置方式

* 静态指定: 

  ifconfig, route, netstat 

  ip: object {link, addr, route}, ss, tc 

  system-config-network-tui，setup 

  配置文件 

* 动态分配：DHCP: Dynamic Host Configuration Protocol

##### 10.4.3.2 ifconfig命令

来自于net-tools包，建议使用 ip 代替

注意：立即生效 

启用混杂模式：[-]promisc

##### 10.4.3.3 route 命令

路由表管理命令 

路由表主要构成: 

* Destination: 目标网络ID,表示可以到达的目标网络ID,0.0.0.0/0 表示所有未知网络,又称为默认路 由,优先级最低 
* Genmask:目标网络对应的netmask 
* Iface: 到达对应网络,应该从当前主机哪个网卡发送出来 
* Gateway: 到达非直连的网络,将数据发送到临近(下一个)路由器的临近本主机的接口的IP地址,如果 是直连网络,gateway是0.0.0.0 
* Metric: 开销cost,值越小,路由记录的优先级最高

查看路由表：

```bash
route
route -n
```

```bash
[root@sh-lzy-Centos8 21:22 ~]#route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         10.0.0.2        0.0.0.0         UG    100    0        0 ens160
10.0.0.0        0.0.0.0         255.255.255.0   U     100    0        0 ens160
192.168.122.0   0.0.0.0         255.255.255.0   U     0      0        0 virbr0
[root@sh-lzy-Centos8 21:50 ~]#
```

##### 10.4.3.4 配置动态路由

通过守护进程获取动态路由，安装quagga包,通过命令vtysh配置 

支持多种路由协议： 

RIP：Routing Information Protocol，路由信息协议 

OSPF：Open Shortest Path First，开放式最短路径优先 

BGP：Border Gateway Protocol，边界网关协议

##### 10.4.3.5 netstat命令

来自于net-tools包，建议使用 ss 代替 

显示网络连接：

```
netstat [--tcp|-t] [--udp|-u] [--raw|-w] [--listening|-l] [--all|-a] [--
numeric|-n] [--extend|-e[--extend|-e]] [--program|-p]
```

常用选项

```
-t: tcp协议相关
-u: udp协议相关
-w: raw socket相关
-l: 处于监听状态
-a: 所有状态
-n: 以数字显示IP和端口
-e：扩展格式
-p: 显示相关进程及PID
```

常用组合

```
-tan, -uan, -tnl, -unl
```

```bash
[root@sh-lzy-Centos8 21:50 ~]#netstat -ntl
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN     
tcp        0      0 192.168.122.1:53        0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN     
tcp        0      0 127.0.0.1:6010          0.0.0.0:*               LISTEN     
tcp6       0      0 :::111                  :::*                    LISTEN     
tcp6       0      0 :::22                   :::*                    LISTEN     
tcp6       0      0 ::1:631                 :::*                    LISTEN     
tcp6       0      0 ::1:6010                :::*                    LISTEN     
[root@sh-lzy-Centos8 21:54 ~]#
```

显示路由表：

```
netstat {--route|-r} [--numeric|-n]
-r: 显示内核路由表
-n: 数字格式
```

##### 10.4.3.6 显示接口统计数据

```
netstat {--interfaces|-I|-i} [iface] [--all|-a] [--extend|-e] [--program|-p] [-
-numeric|-n] 
netstat -i
netstat –I=IFACE
ifconfig -s IFACE
```

```bash
[root@sh-lzy-Centos8 21:57 ~]#netstat -Iens160
Kernel Interface table
Iface             MTU    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
ens160           1500    29358      0      0 0          4172      0      0      0 BMRU
[root@sh-lzy-Centos8 21:58 ~]#
```

##### 10.4.3.7 ip命令

来自于iproute包，可用于代替ifconfig

###### 10.4.3.7.1 配置Linux网络属性

```
ip [ OPTIONS ] OBJECT { COMMAND | help }
```

ip 命令说明：

```
OBJECT := { link | addr | route }
ip link - network device configuration
set dev IFACE，可设置属性：up and down：激活或禁用指定接口，相当于 ifup/ifdown
show [dev IFACE] [up]：：指定接口 ，up 仅显示处于激活状态的接口

man帮助:ip(8), ip-address(8), ip-link(8), ip-route(8)
```

ip 地址管理

```
ip addr { add | del } IFADDR dev STRING [label LABEL] [scope {global|link|host}]
[broadcast ADDRESS]
[label LABEL]：添加地址时指明网卡别名
[scope {global|link|host}]：指明作用域,global: 全局可用.link: 仅链接可用,host: 本机可
用
[broadcast ADDRESS]：指明广播地址 
ip address show 
ip addr flush 
```

###### 10.4.3.7.2 管理路由

ip route 用法

```
#添加路由：
ip route add TARGET via GW dev IFACE src SOURCE_IP
 TARGET:
 主机路由：IP
 网络路由：NETWORK/MASK
#添加网关：
ip route add default via GW dev IFACE
#删除路由：
ip route del TARGET 
#显示路由：
ip route show|list
#清空路由表：
ip route flush [dev IFACE] [via PREFIX]
```

##### 10.4.3.8 ss 命令

来自于iproute包，代替netstat，netstat 通过遍历 /proc来获取 socket信息，ss 使用 netlink与内核 tcp_diag 模块通信获取 socket 信息

格式：

```
ss [OPTION]... [FILTER]
```

选项：

```
-t: tcp协议相关
-u: udp协议相关
-w: 裸套接字相关
-x：unix sock相关
-l: listen状态的连接
-a: 所有
-n: 数字格式
-p: 相关的程序及PID
-e: 扩展的信息
-m：内存用量
-o：计时器信息
```

格式说明：

```
FILTER : [ state TCP-STATE ] [ EXPRESSION ]
TCP的常见状态：
 tcp finite state machine:
 LISTEN: 监听
 ESTABLISHED：已建立的连接
 FIN_WAIT_1
 FIN_WAIT_2
 SYN_SENT
 SYN_RECV
 CLOSED
EXPRESSION:
 dport =
 sport =
```

常用组合：

```
-tan, -tanl, -tanlp, -uan
```

#### 10.4.4 网络配置文件

##### 10.4.4.1 网络基本配置文件 

IP、MASK、GW、DNS相关的配置文件：

```
/etc/sysconfig/network-scripts/ifcfg-IFACE
```

说明参考：

```
/usr/share/doc/initcripts-*/sysconfig.txt
```

常用配置

|     设置      |                             说明                             |
| :-----------: | :----------------------------------------------------------: |
|     TYPE      |              接口类型；常见有的Ethernet, Bridge              |
|     NAME      |                    此配置文件应用到的设备                    |
|    DEVICE     |                            设备名                            |
|    HWADDR     |                     对应的设备的MAC地址                      |
|     UUID      |                        设备的惟一标识                        |
|   BOOTPROTO   | 激活此设备时使用的地址配置协议，常用的dhcp, static, none, bootp |
|    IPADDR     |                          指明IP地址                          |
|    NETMASK    |                  子网掩码,如:255.255.255.0                   |
|    PREFIX     |                     网络ID的位数, 如:24                      |
|    GATEWAY    |                           默认网关                           |
|     DNS1      |                     第一个DNS服务器地址                      |
|     DNS2      |                     第二个DNS服务器地址                      |
|    DOMAIN     |                     第二个DNS服务器地址                      |
|    ONBOOT     |                  在系统引导时是否激活此设备                  |
|    USERCTL    |                   普通用户是否可控制此设备                   |
|    PEERDNS    | 如果BOOTPROTO的值为“dhcp”，YES将允许dhcp server分配的dns服务 器信息直接覆盖至/etc/resolv.conf文件，NO不允许修改resolv.conf |
| NM_CONTROLLED |        NM是NetworkManager的简写，此网卡是否接受NM控制        |

##### 10.4.4.2 配置当前主机的主机名

```
#centos6 之前版本
/etc/sysconfig/network
HOSTNAME=
#centos7 以后版
/etc/hostname 
HOSTNAME
```

##### 10.4.4.3 本地主机名数据库和IP地址的映射

优先于使用DNS前检查 getent hosts 

查看/etc/hosts 内容

```
/etc/hosts
```

##### 10.4.4.4 DNS域名解析

```
/etc/resolv.conf
nameserver DNS_SERVER_IP1
nameserver DNS_SERVER_IP2
nameserver DNS_SERVER_IP3
search DOMAIN
```

##### 10.4.4.5 修改 /etc/hosts和DNS的优先级

```
/etc/nsswitch.conf 
hosts: files dns
```

##### 10.4.4.6 路由相关的配置文件

```
/etc/sysconfig/network-scripts/route-IFACE

两种风格：
(1) TARGET via GW
如：10.0.0.0/8 via 172.16.0.1

(2) 每三行定义一条路由
ADDRESS#=TARGET
NETMASK#=mask
GATEWAY#=GW
```

#### 10.4.5 网卡别名

将多个IP地址绑定到一个NIC上 

每个IP绑定到独立逻辑网卡，即网络别名，命名格式： ethX:Y，如：eth0:1 、eth0:2、eth0:3

注意： 

* 建议 CentOS 6 关闭 NetworkManager 服务 
* 网卡别名必须使用静态地址

#### 10.4.6 多网卡 bonding

将多块网卡绑定同一IP地址对外提供服务，可以实现高可用或者负载均衡。直接给两块网卡设置同一IP 地址是不可以的。通过 bonding，虚拟一块网卡对外提供连接，物理网卡的被修改为相同的MAC地址

##### 10.4.6.1 Bonding 聚合链路工作模式

bond聚合链路模式共7种模式：0-6 Mode

* mod=0 ，即：(balance-rr) Round-robin policy（轮询）聚合口数据报文按包轮询从物理接口转 发。 

  负载均衡—所有链路处于负载均衡状态，轮询方式往每条链路发送报文这模式的特点增加了带宽， 同时支持容错能力，当有链路出问题，会把流量切换到正常的链路上。 

  性能问题—一个连接或者会话的数据包如果从不同的接口发出的话，中途再经过不同的链路，在客 户端很有可能会出现数据包无序到达的问题，而无序到达的数据包需要重新要求被发送，这样网络 的吞吐量就会下降。Bond0在大压力的网络传输下，性能增长的并不是很理想。 

  需要交换机进行端口绑定 

* mod=1，即： (active-backup) Active-backup policy（主-备份策略）只有Active状态的物理接口 才转发数据报文。 

  容错能力—只有一个slave是激活的(active)。也就是说同一时刻只有一个网卡处于工作状态，其他 的slave都处于备份状态，只有在当前激活的slave故障后才有可能会变为激活的(active)。 

  无负载均衡—此算法的优点是可以提供高网络连接的可用性，但是它的资源利用率较低，只有一个 接口处于工作状态，在有 N 个网络接口的情况下，资源利用率为1/N。 

* mod=2，即：(balance-xor) XOR policy（平衡策略）聚合口数据报文按源目MAC、源目IP、源目 端口进行异或HASH运算得到一个值，根据该值查找接口转发数据报文 

  负载均衡—基于指定的传输HASH策略传输数据包。 

  容错能力—这模式的特点增加了带宽，同时支持容错能力，当有链路出问题，会把流量切换到正常 的链路上。 

  性能问题—该模式将限定流量，以保证到达特定对端的流量总是从同一个接口上发出。既然目的地 是通过MAC地址来决定的，因此该模式在“本地”网络配置下可以工作得很好。如果所有流量是通过 单个路由器，由于只有一个网关，源和目标mac都固定了，那么这个算法算出的线路就一直是同一 条，那么这种模式就没有多少意义了。 

  需要交换机配置为port channel 

* mod=3，即：broadcast（广播策略）这种模式的特点是一个报文会复制两份往bond下的两个接 口分别发送出去， 

  当有对端交换机失效，感觉不到任何downtime，但此法过于浪费资源；不过这种模式有很好的容 错机制。此模式适用于金融行业，因为他们需要高可靠性的网络，不允许出现任何问题。 

* mod=4，即：(802.3ad) IEEE 802.3ad Dynamic link aggregation（IEEE 802.3ad 动态链接聚 合） 

* 在动态聚合模式下，聚合组内的成员端口上均启用LACP（链路汇聚控制协议）协议，其端口状态 通过该协议自动进行维护。 

  负载均衡—基于指定的传输HASH策略传输数据包。默认算法与blance-xor一样。 

  容错能力—这模式的特点增加了带宽，同时支持容错能力，当有链路出问题，会把流量切换到正常 的链路上。对比blance-xor，这种模式定期发送LACPDU报文维护链路聚合状态，保证链路质量。 

  需要交换机支持LACP协议 

* mod=5，即：(balance-tlb) Adaptive transmit load balancing（适配器传输负载均衡） 

  在每个物理接口上根据当前的负载（根据速度计算）分配外出流量。如果正在接收数据的物理接口 口出故障了，另一个物理接口接管该故障物理口的MAC地址。 需要ethtool支持获取每个slave的速率 

* mod=6，即：(balance-alb) Adaptive load balancing（适配器适应性负载均衡）

  该模式包含了balance-tlb模式，同时加上针对IPV4流量的接收负载均衡，而且不需要任何 switch(交换机)的支持。接收负载均衡是通过ARP协商实现的。bonding驱动截获本机发送的ARP 应答，并把源硬件地址改写为bond中某个物理接口的唯一硬件地址，从而使得不同的对端使用不 同的硬件地址进行通信。 

  mod=6与mod=0的区别：mod=6，先把eth0流量占满，再占eth1，….ethX；而mod=0的话，会 发现2个口的流量都很稳定，基本一样的带宽。而mod=6，会发现第一个口流量很高，第2个口只 占了小部分流量

说明：

```
常用的模式为 0,1,3,6
mode 1、5、6 不需要交换机设置
mode 0、2、3、4需要交换机设置
active-backup、balance-tlb 和 balance-alb 模式不需要交换机的任何特殊配置。其他绑定模式需
要配置交换机以便整合链接。如：Cisco 交换机需要在模式 0、2 和 3 中使用 EtherChannel，但在模
式4中需要 LACP和 EtherChannel
```

##### 10.4.6.2 Bonding 配置

详细帮助：

```
/usr/share/doc/kernel-doc-version/Documentation/networking/bonding.txt
https://www.kernel.org/doc/Documentation/networking/bonding.txt
```

创建bonding设备的配置文件

```bash
/etc/sysconfig/network-scripts/ifcfg-bond0
NAME=bond0
TYPE=bond
DEVICE=bond0
BOOTPROTO=none
IPADDR=10.0.0.100
PREFIX=8
#miimon指定链路监测时间间隔。如果miimon=100，那么系统每100ms 监测一次链路连接状态，如果有一
条线路不通就转入另一条线路
BONDING_OPTS="mode=1 miimon=100 fail_over_mac=1"

/etc/sysconfig/network-scripts/ifcfg-eth0
NAME=eth0
DEVICE=eth0
BOOTPROTO=none
MASTER=bond0
SLAVE=yes
ONBOOT=yes

/etc/sysconfig/network-scripts/ifcfg-eth1
NAME=eth1
DEVICE=eth1
BOOTPROTO=none
MASTER=bond0
SLAVE=yes
ONBOOT=yes
```

查看bond0状态：

```
/proc/net/bonding/bond0
```

删除bond0

```
ifconfig bond0 down
rmmod bonding
```

#### 10.4.7 CentOS 7 以上版网络配置

CentOS 6之前，网络接口使用连续号码命名：eth0、eth1等,当增加或删除网卡时，名称可能会发生变 化，CentOS 7 以上版使用基于硬件，设备拓扑和设置类型命名 

CentOS 8 中已弃用network.service，采用NetworkManager（NM）为网卡启用命令。CentOS 8 仍可 以安装network.service作为网卡服务，只是默认没有安装，具体方法为： dnf install networkscripts ，不过官方已明确在下一个大版本中，将彻底放弃network.service，不建议继续使用 network.service管理网络。

##### 10.4.7.1 网卡命名机制 

systemd对网络设备的命名方式 

1. 如果Firmware或BIOS为主板上集成的设备提供的索引信息可用，且可预测则根据此索引进行命 名，如：eno1 
2.  如果Firmware或BIOS为PCI-E扩展槽所提供的索引信息可用，且可预测，则根据此索引进行命 名，如：ens1 
3. 如果硬件接口的物理位置信息可用，则根据此信息命名，如：enp2s0 
4.  如果用户显式启动，也可根据MAC地址进行命名，如：enx2387a1dc56 
5. 上述均不可用时，则使用传统命名机制

**基于BIOS支持启用biosdevname软件**

```
内置网卡：em1,em2  
pci卡：pYpX Y：slot ,X:port 
```

**网卡组成格式**

```
en: Ethernet 有线局域网
wl: wlan 无线局域网
ww: wwan无线广域网
o<index>: 集成设备的设备索引号
s<slot>: 扩展槽的索引号
x<MAC>: 基于MAC地址的命名
p<bus>s<slot>: enp2s1
```

使用传统命名方式：

 (1) 编辑/etc/default/grub配置文件

```
GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"
```

(2) 为grub2生成其配置文件

```
grub2-mkconfig -o /etc/grub2.cfg
```

(3) 重启系统

```
reboot
```

##### 10.4.7.2 主机名

配置文件:

```
/etc/hostname 
```

默认没有此文件，通过DNS反向解析获取主机名，主机名默认为：localhost.localdomain

设置主机名

```
hostnamectl set-hostname centos7.magedu.com
```

删除文件/etc/hostname，恢复主机名localhost.localdomain

**显示主机名信息**

```
hostname
hostnamectl status
```

##### 10.4.7.3 网络配置工具 nmcli

图形工具：nm-connection-editor 

字符配置 tui工具： 

* nmtui 
* nmtui-connect  
* nmtui-edit  
* nmtui-hostname 

命令行工具：nmcli 

以上工具都依赖NetworkManager服务，此服务是管理和监控网络设置的守护进程

**nmcli命令** 

nmcli命令相关术语 

* 设备即网络接口 
* 连接是对网络接口的配置，一个网络接口可有多个连接配置，但同时只有一个连接配置生效

格式：

```
nmcli [ OPTIONS ] OBJECT { COMMAND | help }
  device - show and manage network interfaces
  nmcli device help
  connection - start, stop, and manage network connections
  nmcli connection help
```

修改IP地址等属性：

```
nmcli connection modify IFACE [+|-]setting.property value
setting.property: ipv4.addresses ipv4.gateway ipv4.dns1 ipv4.method manual | 
auto
```

修改配置文件执行生效：

```
nmcli con reload
nmcli con up con-name
```

|          nmcli con mod           |       ifcfg**-*** 文件       |
| :------------------------------: | :--------------------------: |
|        ipv4.method manual        |        BOOTPROTO=none        |
|         ipv4.method auto         |        BOOTPROTO=dhcp        |
|  ipv4.addresses 192.168.2.1/24   | IPADDR=192.168.2.1 PREFIX=24 |
|    ipv4.gateway 172.16.0.200     |     GATEWAY=192.0.2.254      |
|         ipv4.dns 8.8.8.8         |         DNS0=8.8.8.8         |
|   ipv4.dns-search example.com    |      DOMAIN=example.com      |
|    ipv4.ignore-auto-dns true     |          PEERDNS=no          |
|    connection.autoconnect yes    |          ONBOOT=yes          |
|        connection.id eth0        |          NAME=eth0           |
|  connection.interface-name eth0  |          NAME=eth0           |
| 802-3-ethernet.mac-address . . . |        HWADDR= . . .         |

##### 10.4.7.4 nmcli实现 bonding

```bash
#添加bonding接口
nmcli con add type bond con-name mybond0 ifname bond0 mode active-backup 
ipv4.method manual ipv4.addresses 10.0.0.100/24 
#添加从属接口
nmcli con add type bond-slave ifname ens7 master bond0
nmcli con add type bond-slave ifname ens3 master bond0
#注：如无为从属接口提供连接名，则该名称是接口名称加类型构成
#要启动绑定，则必须首先启动从属接口
nmcli con up bond-slave-eth0
nmcli con up bond-slave-eth1
#启动绑定
nmcli con up mybond0
```

##### 10.4.7.5 网络组 Network Teaming

网络组：是将多个网卡聚合在一起方法，从而实现冗错和提高吞吐量 

网络组不同于旧版中bonding技术，提供更好的性能和扩展性 

网络组由内核驱动和teamd守护进程实现

多种方式 runner 

* broadcast 
* roundrobin 
* activebackup 
* loadbalance 
* lacp (implements the 802.3ad Link Aggregation Control Protocol)

网络组特点

* 启动网络组接口不会自动启动网络组中的port接口 
* 启动网络组接口中的port接口总会自动启动网络组接口 
* 禁用网络组接口会自动禁用网络组中的port接口 
* 没有port接口的网络组接口可以启动静态IP连接 
* 启用DHCP连接时，没有port接口的网络组会等待port接口的加入

```bash
#创建网络组接口
nmcli con add type team con-name CNAME ifname INAME [config JSON]
CNAME 连接名
INAME 接口名
JSON 指定runner方式,格式：'{"runner": {"name": "METHOD"}}'
METHOD 可以是broadcast, roundrobin, activebackup, loadbalance, lacp
#创建port接口
nmcli con add type team-slave con-name CNAME ifname INAME master TEAM
CNAME 连接名,连接名若不指定，默认为team-slave-IFACE
INAME 网络接口名
TEAM 网络组接口名
#断开和启动
nmcli dev dis INAME
nmcli con up CNAME
INAME 设备名 CNAME 网络组接口名或port接口
```

网络组示例

```
nmcli con add type team con-name myteam0 ifname team0 config '{"runner": 
{"name": "loadbalance"}}' ipv4.addresses 192.168.1.100/24 ipv4.method manual
nmcli con add con-name team0-eth1 type team-slave ifname eth1 master team0
nmcli con add con-name team0-eth2 type team-slave ifname eth2 master team0
nmcli con up myteam0
nmcli con up team0-eth1
nmcli con up team0-eth2
teamdctl team0 state
ping -I team0 192.168.0.254
nmcli dev dis eth1
teamdctl team0 state
nmcli con up team0-port1
nmcli dev dis eth2
teamdctl team0 state
nmcli con up team0-port2
teamdctl team0 state
```

管理网络组配置文件

```
/etc/sysconfig/network-scripts/ifcfg-team0
DEVICE=team0
DEVICETYPE=Team
TEAM_CONFIG="{\"runner\": {\"name\": \"broadcast\"}}"
BOOTPROTO=none
IPADDR0=172.16.0.100
PREFIX0=24
NAME=team0
ONBOOT=yes
```

管理网络组配置文件

```
/etc/sysconfig/network-scripts/ifcfg-team0-eth1
DEVICE=eth1
DEVICETYPE=TeamPort
TEAM_MASTER=team0
NAME=team0-eth1
ONBOOT=yes
```

删除网络组

```
nmcli connection down team0
teamdctl team0 state
nmcli connection show
nmcli connectioni delete team0-eth0
nmcli connectioni delete team0-eth1
nmcli connection show
```

#### 10.4.8 网桥

##### 10.4.8.1 桥接原理

桥接：把一台机器上的若干个网络接口“连接”起来。其结果是，其中一个网口收到的报文会被复制给其 他网口并发送出去。以使得网口之间的报文能够互相转发。网桥就是这样一个设备，它有若干个网口， 并且这些网口是桥接起来的。与网桥相连的主机就能通过交换机的报文转发而互相通信。

主机A发送的报文被送到交换机S1的eth0口，由于eth0与eth1、eth2桥接在一起，故而报文被复制到 eth1和eth2，并且发送出去，然后被主机B和交换机S2接收到。而S2又会将报文转发给主机C、D

CentOS 8 取消brctl 工具,可以用下面方法查看网桥

```bash
#查看桥接情况
[root@centos8 ~]#ip link show master virbr0
4: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc fq_codel master virbr0 state 
DOWN mode DEFAULT group default qlen 1000
link/ether 52:54:00:52:f2:5c brd ff:ff:ff:ff:ff:ff
8: vnet0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel master 
virbr0 state UNKNOWN mode DEFAULT group default qlen 1000
   link/ether fe:54:00:95:25:fb brd ff:ff:ff:ff:ff:ff
9: vnet1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel master 
virbr0 state UNKNOWN mode DEFAULT group default qlen 1000
   link/ether fe:54:00:83:9d:51 brd ff:ff:ff:ff:ff:ff
   
[root@centos8 ~]#bridge link show
4: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 master virbr0 state disabled 
priority 32 cost 100
8: vnet0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 master virbr0 state 
forwarding priority 32 cost 100
9: vnet1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 master virbr0 state 
forwarding priority 32 cost 100
```

##### 10.4.8.2 配置实现网桥

工具包：bridge-utils,目前 CentOS 8 无此包

```bash
yum install bridge-utils
#查看网桥 
brctl show
#查看CAM(content addressable memory内容可寻址存储器)表 
brctl showmacs br0
#添加和删除网桥 
brctl addbr | delbr br0 
#添加和删除网桥中网卡 
brctl addif | delif br0 eth0
#默认br0 是down,必须启用
ifconfig br0 up 
#启用STP
[root@centos7 ~]#brctl show
bridge name bridge id STP enabled interfaces
br0 8000.000c297e67a3 no eth1
 eth2
[root@centos7 ~]#brctl stp br0 on
[root@centos7 ~]#brctl show
bridge name bridge id STP enabled interfaces
br0 8000.000c297e67a3 yes eth1
 						  eth2
```

注意：NetworkManager只支持以太网接口接口连接到网桥，不支持聚合接口

nmcli命令创建软件网桥

```
nmcli con add con-name mybr0 type bridge ifname br0
nmcli con modify mybr0 ipv4.addresses 10.0.0.100/24 ipv4.method manual
nmcli con add con-name br0-port0 type bridge-slave ifname eth0 master br0
```

查看配置文件

```
cat /etc/sysconfig/network-scripts/ifcfg-br0
cat /etc/sysconfig/network-scripts/ifcfg-br0-port0
```

#### 10.4.9 网络测试诊断工具

* 测试网络连通性 

  ping  

* 显示正确的路由表 

  ip route 

  route 

* 跟踪路由 

  traceroute 

  tracepath 

  mtr 

* 确定名称服务器使用 

  nslookup 

  host 

  dig 

* 抓包工具 

  tcpdump  

  wireshark 

* 安全扫描工具 

  nmap 

  netcat ：网络界的瑞士军刀，即nc 

* 流量控制工具

  tc

##### 10.4.9.1 fping

fping是一个程序，用于将ICMP探测发送到网络主机，类似于ping，fping的历史由来已久：Roland  Schemers在1992年确实发布了它的第一个版本，从那时起它就确立了自己的地位，成为网络诊断和统 计的标准工具 

相对于ping多个主机时性能要高得多。 fping完全不同于ping，可以在命令行上定义任意数量的主机， 或者指定包含要ping的IP地址或主机列表的文件, 常在shell 脚本中使用

CentOS 中由EPEL源提供 

官网：http://www.fping.org/

##### 10.4.9.2 tcpdump

网络数据包截获分析工具。支持针对网络层、协议、主机、网络或端口的过滤。并提供and、or、not 等逻辑语句帮助去除无用的信息。

语法：

```
tcpdump [-adeflnNOpqStvx][-c<数据包数目>][-dd][-ddd][-F<表达文件>][-i<网络界面>][-r<
数据包文件>][-s<数据包大小>][-tt][-T<数据包类型>][-vv][-w<数据包文件>][输出数据栏位]
参数说明：
-a 尝试将网络和广播地址转换成名称。
-c<数据包数目> 收到指定的数据包数目后，就停止进行倾倒操作。
-d 把编译过的数据包编码转换成可阅读的格式，并倾倒到标准输出。
-dd 把编译过的数据包编码转换成C语言的格式，并倾倒到标准输出。
-ddd 把编译过的数据包编码转换成十进制数字的格式，并倾倒到标准输出。
-e 在每列倾倒资料上显示连接层级的文件头。
-f 用数字显示网际网络地址。
-F<表达文件> 指定内含表达方式的文件。
-i<网络接口> 使用指定的网络截面送出数据包。
-l 使用标准输出列的缓冲区。
-n 不把主机的网络地址转换成名字。
-N 不列出域名。
-O 不将数据包编码最佳化。
-p 不让网络界面进入混杂模式。
-q 快速输出，仅列出少数的传输协议信息。
-r<数据包文件> 从指定的文件读取数据包数据。
-s<数据包大小> 设置每个数据包的大小。
-S 用绝对而非相对数值列出TCP关联数。
-t 在每列倾倒资料上不显示时间戳记。
-tt 在每列倾倒资料上显示未经格式化的时间戳记。
-T<数据包类型> 强制将表达方式所指定的数据包转译成设置的数据包类型。
-v 详细显示指令执行过程。
-vv 更详细显示指令执行过程。
-x 用十六进制字码列出数据包资料。
-w<数据包文件> 把数据包数据写入指定的文件。
```

##### 10.4.9.3 nmap

扫描远程主机工具，功能远超越用世人皆知的 Ping 工具发送简单的 ICMP 回声请求报文 

官方帮助：https://nmap.org/book/man.html

格式：

```
nmap [Scan Type(s)] [Options] {target specification}
命令选项
-sT   TCP connect() 扫描，这是最基本的 TCP 扫描方式。这种扫描很容易被检测到，在目标主机的
日志中会记录大批的连接请求以及错误信息    
-sS   TCP 同步扫描 (TCP SYN)，因为不必全部打开一个 TCP 连接，所以这项技术通常称为半开扫描
(half-open)。这项技术最大的好处是，很少有系统能够把这记入系统日志   
-sF,-sX,-sN   秘密 FIN 数据包扫描、圣诞树 (Xmas Tree)、空 (Null) 扫描模式。这些扫描方式
的理论依据是：关闭的端口需要对你的探测包回应 RST 包，而打开的端口必需忽略有问题的包    
-sP    ping 扫描，用 ping 方式检查网络上哪些主机正在运行。当主机阻塞 ICMP echo 请求包是
ping 扫描是无效的。nmap 在任何情况下都会进行 ping 扫描，只有目标主机处于运行状态，才会进行后
续的扫描    
-sU   UDP 的数据包进行扫描，想知道在某台主机上提供哪些 UDP 服务，可以使用此选项    
-sA   ACK 扫描，这项高级的扫描方法通常可以用来穿过防火墙。    
-sW   滑动窗口扫描，非常类似于 ACK 的扫描    
-sR   RPC 扫描，和其它不同的端口扫描方法结合使用。    
-b     FTP 反弹攻击 (bounce attack)，连接到防火墙后面的一台 FTP 服务器做代理，接着进行端口
扫描。
-P0   在扫描之前，不 ping 主机。    
-PT   扫描之前，使用 TCP ping 确定哪些主机正在运行    
-PS   对于 root 用户，这个选项让 nmap 使用 SYN 包而不是 ACK 包来对目标主机进行扫描。 
-PI   设置这个选项，让 nmap 使用真正的 ping(ICMP echo 请求）来扫描目标主机是否正在运行。  
-PB   这是默认的 ping 扫描选项。它使用 ACK(-PT) 和 ICMP(-PI) 两种扫描类型并行扫描。如果
防火墙能够过滤其中一种包，使用这种方法，你就能够穿过防火墙。    
-O   这个选项激活对 TCP/IP 指纹特征 (fingerprinting) 的扫描，获得远程主机的标志，也就是操
作系统类型
-I   打开 nmap 的反向标志扫描功能。    
-f   使用碎片 IP 数据包发送 SYN、FIN、XMAS、NULL。包增加包过滤、入侵检测系统的难度，使其无
法知道你的企图    
-v   冗余模式。强烈推荐使用这个选项，它会给出扫描过程中的详细信息。    
-S <IP>   在一些情况下，nmap 可能无法确定你的源地址 。在这种情况使用这个选项给出指定 IP 地
址   
-g port   设置扫描的源端口
-oN   把扫描结果重定向到一个可读的文件 logfilename 中  
-oS   扫描结果输出到标准输出。    
--host_timeout   设置扫描一台主机的时间，以毫秒为单位。默认的情况下，没有超时限制    
--max_rtt_timeout   设置对每次探测的等待时间，以毫秒为单位。如果超过这个时间限制就重传或者
超时。默认值是大约 9000 毫秒    
--min_rtt_timeout   设置 nmap 对每次探测至少等待你指定的时间，以毫秒为单位    
-M count   置进行 TCP connect() 扫描时，最多使用多少个套接字进行并行的扫描
```

# 11 进程，系统性能和计划任务

### 内容概述

* 进程相关概念 
* 进程工具 
* 系统性能相关工具 
* 计划任务

### 11.1 进程和内存管理

内核功用：进程管理、内存管理、文件系统、网络功能、驱动程序、安全功能等

#### 11.1.1 什么是进程

Process: 运行中的程序的一个副本，是被载入内存的一个指令集合，是资源分配的单位 

* 进程ID（Process ID，PID）号码被用来标记各个进程 
* UID、GID、和SELinux语境决定对文件系统的存取和访问权限 
* 通常从执行进程的用户来继承 
* 存在生命周期

进程创建： 

* init：第一个进程，从 CentOS7 以后为systemd  
* 进程：都由其父进程创建，fork()，父子关系，CoW：Copy On Write 

**进程，线程和协程**

* 进程

```
进程是一个具有一定独立功能的程序在一个数据集上的一次动态执行的过程，是操作系统进行资源分配和调度
的一个独立单位，是应用程序运行的载体。进程是一种抽象的概念，从来没有统一的标准定义。
进程的组成
进程一般由程序、数据集合和进程控制块三部分组成。
程序用于描述进程要完成的功能，是控制进程执行的指令集；
数据集合是程序在执行时所需要的数据和工作区；
程序控制块(Program Control Block，简称PCB)，包含进程的描述信息和控制信息，是进程存在的唯一
标志。
进程具有的特征：
动态性：进程是程序的一次执行过程，是临时的，有生命期的，是动态产生，动态消亡的；
并发性：任何进程都可以同其他进程一起并发执行；
独立性：进程是系统进行资源分配和调度的一个独立单位；
结构性：进程由程序、数据和进程控制块三部分组成。
```

* 线程

```
在早期的操作系统中并没有线程的概念，进程是能拥有资源和独立运行的最小单位，也是程序执行的最小单
位。任务调度采用的是时间片轮转的抢占式调度方式，而进程是任务调度的最小单位，每个进程有各自独立的
一块内存，使得各个进程之间内存地址相互隔离。
后来，随着计算机的发展，对CPU的要求越来越高，进程之间的切换开销较大，已经无法满足越来越复杂的程
序的要求了。于是就发明了线程。
线程是程序执行中一个单一的顺序控制流程，是程序执行流的最小单元，是处理器调度和分派的基本单位。一
个进程可以有一个或多个线程，各个线程之间共享程序的内存空间(也就是所在进程的内存空间)。一个标准的
线程由线程ID、当前指令指针(PC)、寄存器和堆栈组成。而进程由内存空间(代码、数据、进程空间、打开的
文件)和一个或多个线程组成。
```

* 协程

```
协程，英文Coroutines，是一种基于线程之上，但又比线程更加轻量级的存在，这种由程序员自己写程序来
管理的轻量级线程叫做『用户空间线程』，具有对内核来说不可见的特性。
因为是自主开辟的异步任务，所以很多人也更喜欢叫它们纤程（Fiber），或者绿色线程
（GreenThread）。正如一个进程可以拥有多个线程一样，一个线程也可以拥有多个协程。
协程的目的
在传统的J2EE系统中都是基于每个请求占用一个线程去完成完整的业务逻辑（包括事务）。所以系统的吞吐能
力取决于每个线程的操作耗时。如果遇到很耗时的I/O行为，则整个系统的吞吐立刻下降，因为这个时候线程
一直处于阻塞状态，如果线程很多的时候，会存在很多线程处于空闲状态（等待该线程执行完才能执行），造
成了资源应用不彻底。
最常见的例子就是JDBC（它是同步阻塞的），这也是为什么很多人都说数据库是瓶颈的原因。这里的耗时其实
是让CPU一直在等待I/O返回，说白了线程根本没有利用CPU去做运算，而是处于空转状态。而另外过多的线
程，也会带来更多的ContextSwitch开销。
对于上述问题，现阶段行业里的比较流行的解决方案之一就是单线程加上异步回调。其代表派是node.js以及
Java里的新秀Vert.x。
而协程的目的就是当出现长时间的I/O操作时，通过让出目前的协程调度，执行下一个任务的方式，来消除
ContextSwitch上的开销。
协程的特点
线程的切换由操作系统负责调度，协程由用户自己进行调度，因此减少了上下文切换，提高了效率。
线程的默认Stack大小是1M，而协程更轻量，接近1K。因此可以在相同的内存中开启更多的协程。
由于在同一个线程上，因此可以避免竞争关系而使用锁。
适用于被阻塞的，且需要大量并发的场景。但不适用于大量计算的多线程，遇到此种情况，更好实用线程去解
决。
协程的原理
当出现IO阻塞的时候，由协程的调度器进行调度，通过将数据流立刻yield掉（主动让出），并且记录当前栈
上的数据，阻塞完后立刻再通过线程恢复栈，并把阻塞的结果放到这个线程上去跑，这样看上去好像跟写同步
代码没有任何差别，这整个流程可以称为coroutine，而跑在由coroutine负责调度的线程称为Fiber。比
如Golang里的 go关键字其实就是负责开启一个Fiber，让func逻辑跑在上面。
由于协程的暂停完全由程序控制，发生在用户态上；而线程的阻塞状态是由操作系统内核来进行切换，发生在
内核态上。
因此，协程的开销远远小于线程的开销，也就没有了ContextSwitch上的开销。
```

* 进程与线程的区别

```
线程是程序执行的最小单位，而进程是操作系统分配资源的最小单位；
一个进程由一个或多个线程组成，线程是一个进程中代码的不同执行路线；
进程之间相互独立，但同一进程下的各个线程之间共享程序的内存空间(包括代码段、数据集、堆等)及一些进
程级的资源(如打开文件和信号)，某进程内的线程在其它进程不可见；
调度和切换：线程上下文切换比进程上下文切换要快得多。
```

* 协程和线程的比较

|  比较项  |                             线程                             |                             协程                             |
| :------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 占用资源 |                  初始单位为1MB，固定不可变                   |                初试一般为2KB，可随需要而增大                 |
| 调度所属 |                        由OS的内核完成                        |                          由用户完成                          |
| 切换开销 | 涉及模式切换（从用户态切换到内核态）、16个寄存机器、PC、SP...等寄存器的刷新等 |               只有三个寄存器的值修改-PC/SP/DX                |
| 性能问题 |        资源占用太高，频繁创建销毁会带来严重的性能问题        |              资源占用小，不会带来严重的性能问题              |
| 数据同步 |            需要用锁等机制确保数据的一致性和可见性            | 不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率比多线程高很多 |

**面试题: 查看进程中的线程**

```
grep -i threads /proc/PID/status
```

#### 11.1.2 进程结构

内核把进程存放在叫做任务队列（task list)的双向循环链表中 

链表中的每一项都是类型为task_struct，称为进程控制块（Processing Control Block），PCB中包含 一个具体进程的所有信息

**进程控制块PCB包含信息：** 

* 进程id、用户id和组id 
* 程序计数器 
* 进程的状态(有就绪、运行、阻塞) 
* 进程切换时需要保存和恢复的CPU寄存器的值 
* 描述虚拟地址空间的信息 
* 描述控制终端的信息 
* 当前工作目录 
* 文件描述符表，包含很多指向file结构体的指针 
* 进程可以使用的资源上限(ulimit –a命令可以查看) 
* 输入输出状态：配置进程使用I/O设备

#### 11.1.3 进程相关概念

Page Frame: 页框，用存储页面数据，存储Page 4k

##### 11.1.3.1 物理地址空间和虚拟地址空间

MMU：Memory Management Unit 负责虚拟地址转换为物理地址 

程序在访问一个内存地址指向的内存时,CPU不是直接把这个地址送到内存总线上,而是被送到 MMU（Memory Management Unit),然后把这个内存地址映射到实际的物理内存地址上，然后通过总 线再去访问内存，程序操作的地址称为虚拟内存地址 

TLB：Translation Lookaside Buffer 翻译后备缓冲区，用于保存虚拟地址和物理地址映射关系的缓存

##### 11.1.3.2 用户和内核空间

##### 11.1.3.3 C代码和内存布局之间的对应关系

每个进程都包括5种不同的数据段 

* 代码段：用来存放可执行文件的操作指令，也就是说是它是可执行程序在内存中的镜像。代码段需 要防止在运行时被非法修改，所以只准许读取操作，而不允许写入（修改）操作——它是不可写的 
* 数据段：用来存放可执行文件中已初始化全局变量，换句话说就是存放程序静态分配的变量和全局 变量 
* BSS段：Block Started by Symbol”的缩写,意为“以符号开始的块，BSS段包含了程序中未初始化的 全局变量，在内存中 bss段全部置零

* 堆（heap）：存放数组和对象，堆是用于存放进程运行中被动态分配的内存段，它的大小并不固 定，可动态扩张或缩减。当进程调用malloc等函数分配内存时，新分配的内存就被动态添加到堆 上（堆被扩张）；当利用free等函数释放内存时，被释放的内存从堆中被剔除（堆被缩减） 
* 栈（stack）：栈是用户存放程序临时创建的局部变量，也就是说我们函数括弧“{}”中定义的变量 （但不包括static声明的变量，static意味着在数据段中存放变量）。除此以外，在函数被调用时， 其参数也会被压入发起调用的进程栈中，并且待到调用结束后，函数的返回值也会被存放回栈中。 由于栈的后进先出特点，所以栈特别方便用来保存/恢复调用现场。可以把堆栈看成一个寄存、交 换临时数据的内存区

**喝多了吐就是栈,吃多了拉就是队列**

##### 11.1.3.4 进程使用内存问题

###### 11.1.3.4.1 内存泄漏：Memory Leak

指程序中用malloc或new申请了一块内存，但是没有用free或delete将内存释放，导致这块内存一直处 于占用状态

###### 11.1.3.4.2 内存溢出：Memory Overflow

指程序申请了10M的空间，但是在这个空间写入10M以上字节的数据，就是溢出,类似红杏出墙(N50学 炳语录)

###### 11.1.3.4.3 内存不足：OOM

OOM 即 Out Of Memory，“内存用完了”,在情况在java程序中比较常见。系统会选一个进程将之杀死， 在日志messages中看到类似下面的提示 

Jul 10 10:20:30 kernel: Out of memory: Kill process 9527 (java) score 88 or sacrifice child 

当JVM因为没有足够的内存来为对象分配空间并且垃圾回收器也已经没有空间可回收时，就会抛出这个 error，因为这个问题已经严重到不足以被应用处理）。

原因： 

* 给应用分配内存太少：比如虚拟机本身可使用的内存（一般通过启动时的VM参数指定）太少。 
* 应用用的太多，并且用完没释放，浪费了。此时就会造成内存泄露或者内存溢出。

使用的解决办法： 

1，限制java进程的max heap，并且降低java程序的worker数量，从而降低内存使用 

2，给系统增加swap空间

设置内核参数（不推荐），不允许内存申请过量：

```
echo 2 > /proc/sys/vm/overcommit_memory
echo 80 > /proc/sys/vm/overcommit_ratio
echo 2 > /proc/sys/vm/panic_on_oom 
```

说明： 

Linux默认是允许memory overcommit的，只要你来申请内存我就给你，寄希望于进程实际上用不到 那么多内存，但万一用到那么多了呢？Linux设计了一个OOM killer机制挑选一个进程出来杀死，以腾 出部分内存，如果还不够就继续。也可通过设置内核参数 vm.panic_on_oom 使得发生OOM时自动重 启系统。这都是有风险的机制，重启有可能造成业务中断，杀死进程也有可能导致业务中断。所以 Linux 2.6之后允许通过内核参数 vm.overcommit_memory 禁止memory overcommit。

vm.panic_on_oom 决定系统出现oom的时候，要做的操作。接受的三种取值如下：

```
0 - 默认值，当出现oom的时候，触发oom killer
1 - 程序在有cpuset、memory policy、memcg的约束情况下的OOM，可以考虑不panic，而是启动OOM 
killer。其它情况触发 kernel panic，即系统直接重启
2 - 当出现oom，直接触发kernel panic，即系统直接重启
```

vm.overcommit_memory 接受三种取值:

```
0 – Heuristic overcommit handling. 这是缺省值，它允许overcommit，但过于明目张胆的
overcommit会被拒绝，比如malloc一次性申请的内存大小就超过了系统总内存。Heuristic的意思是“试
探式的”，内核利用某种算法猜测你的内存申请是否合理，它认为不合理就会拒绝overcommit。
1 – Always overcommit. 允许overcommit，对内存申请来者不拒。内核执行无内存过量使用处理。使
用这个设置会增大内存超载的可能性，但也可以增强大量使用内存任务的性能。
2 – Don’t overcommit. 禁止overcommit。 内存拒绝等于或者大于总可用 swap 大小以及
overcommit_ratio 指定的物理 RAM 比例的内存请求。如果希望减小内存过度使用的风险，这个设置就是
最好的。
```

Heuristic overcommit算法： 

单次申请的内存大小不能超过以下值，否则本次申请就会失败。

```
free memory + free swap + pagecache的大小 + SLAB
```

vm.overcommit_memory=2 禁止overcommit,那么怎样才算是overcommit呢？ 

kernel设有一个阈值，申请的内存总数超过这个阈值就算overcommit，在/proc/meminfo中可以看到 这个阈值的大小：

```
grep -i commit /proc/meminfo
CommitLimit:     5967744 kB
Committed_AS:    5363236 kB
```

CommitLimit 就是overcommit的阈值，申请的内存总数超过CommitLimit的话就算是overcommit。 此值通过内核参数vm.overcommit_ratio或vm.overcommit_kbytes间接设置的，公式如下：

```
CommitLimit = (Physical RAM * vm.overcommit_ratio / 100) + Swap
```

vm.overcommit_ratio 是内核参数，缺省值是50，表示物理内存的50%。如果你不想使用比率，也可 以直接指定内存的字节数大小，通过另一个内核参数 vm.overcommit_kbytes 即可； 如果使用了huge pages，那么需要从物理内存中减去，公式变成：

```
CommitLimit = ([total RAM] – [total huge TLB RAM]) * vm.overcommit_ratio / 100 + 
swap
```

/proc/meminfo中的 Committed_AS 表示所有进程已经申请的内存总大小，（注意是已经申请的，不 是已经分配的），如果 Committed_AS 超过 CommitLimit 就表示发生了 overcommit，超出越多表示 overcommit 越严重。Committed_AS 的含义换一种说法就是，如果要绝对保证不发生OOM (out of  memory) 需要多少物理内存。

#### 11.1.4 进程状态

**进程的基本状态** 

* 创建状态：进程在创建时需要申请一个空白PCB(process control block进程控制块)，向其中填写 控制和管理进程的信息，完成资源分配。如果创建工作无法完成，比如资源无法满足，就无法被调 度运行，把此时进程所处状态称为创建状态 
* 就绪状态：进程已准备好，已分配到所需资源，只要分配到CPU就能够立即运行 
* 执行状态：进程处于就绪状态被调度后，进程进入执行状态 
* 阻塞状态：正在执行的进程由于某些事件（I/O请求，申请缓存区失败）而暂时无法运行，进程受 到阻塞。在满足请求时进入就绪状态等待系统调用 
* 终止状态：进程结束，或出现错误，或被系统终止，进入终止状态。无法再执行

**状态之间转换六种情况** 

运行——>就绪：

1，主要是进程占用CPU的时间过长，而系统分配给该进程占用CPU的时间是有限的； 

2，在采用抢先式优先级调度算法的系统中,当有更高优先级的进程要运行时，该进程就被迫让出CPU， 该进程便由执行状态转变为就绪状态 

就绪——>运行：运行的进程的时间片用完，调度就转到就绪队列中选择合适的进程分配CPU 

运行——>阻塞：正在执行的进程因发生某等待事件而无法执行，则进程由执行状态变为阻塞状态，如 发生了I/O请求 

阻塞——>就绪:进程所等待的事件已经发生，就进入就绪队列 

以下两种状态是不可能发生的： 

阻塞——>运行：即使给阻塞进程分配CPU，也无法执行，操作系统在进行调度时不会从阻塞队列进行挑选，而是从就绪队列中选取 

就绪——>阻塞：就绪态根本就没有执行，谈不上进入阻塞态

**进程更多的状态**： 

* 运行态：running 
* 就绪态：ready 
* 睡眠态：分为两种，可中断：interruptable，不可中断：uninterruptable 
* 停止态：stopped，暂停于内存，但不会被调度，除非手动启动 
* 僵死态：zombie，僵尸态，结束进程，父进程结束前，子进程不关闭，杀死父进程可以关闭僵死 态 的子进程

#### 11.1.5 LRU 算法

LRU：Least Recently Used 近期最少使用算法（喜新厌旧），释放内存

范例: 

假设序列为 4 3 4 2 3 1 4 2， 物理块有3个，则 

第1轮 4调入内存 4 

第2轮 3调入内存 3 4 

第3轮 4调入内存 4 3 

第4轮 2调入内存 2 4 3 

第5轮 3调入内存 3 2 4 

第6轮 1调入内存 1 3 2 

第7轮 4调入内存 4 1 3 

第8轮 2调入内存 2 4 1

#### 11.1.6 IPC 进程间通信

IPC: Inter Process Communication

* 同一主机

```
pipe 管道,单向传输
socket   套接字文件,双工通信
Memory-maped file   文件映射,将文件中的一段数据映射到物理内存，多个进程共享这片内存
shm shared memory 共享内存
signal 信号
Lock   对资源上锁，如果资源已被某进程锁住，则其它进程想修改甚至读取这些资源，都将被
阻塞，直到锁被打开
semaphore 信号量，一种计数器
```

* 不同主机：socket=IP和端口号

```
RPC remote procedure call
MQ 消息队列，生产者和消费者，如：Kafka，RabbitMQ，ActiveMQ
```

#### 11.1.7 进程优先级

进程优先级：

```
系统优先级：0-139, 数字越小，优先级越高,各有140个运行队列和过期队列
实时优先级: 99-0   值最大优先级最高
nice值：-20到19，对应系统优先级100-139或
```

Big O：时间（空间）复杂度，用时（空间）和规模的关系

```
O(1), O(logn), O(n)线性, O(n^2)抛物线, O(2^n)
```

#### 11.1.8 进程分类

**操作系统分类：** 

* 协作式多任务：早期 windows 系统使用，即一个任务得到了 CPU 时间，除非它自己放弃使用 CPU ，否则将完全霸占 CPU ，所以任务之间需要协作——使用一段时间的 CPU ，主动放弃使用 
* 抢占式多任务：Linux内核，CPU的总控制权在操作系统手中，操作系统会轮流询问每一个任务是 否需要使用 CPU ，需要使用的话就让它用，不过在一定时间后，操作系统会剥夺当前任务的 CPU  使用权，把它排在询问队列的最后，再去询问下一个任务

**进程类型：** 

* 守护进程: daemon,在系统引导过程中启动的进程，和终端无关进程 
* 前台进程：跟终端相关，通过终端启动的进程

注意：两者可相互转化

**按进程资源使用的分类：** 

* CPU-Bound：CPU 密集型，非交互 
* IO-Bound：IO 密集型，交互

#### 11.1.9 IO调度算法

在LINUX 2.6中有四种关于IO的调度算法: 

* NOOP  

  NOOP算法的全写为No Operation。该算法实现了最简单的FIFO队列，所有IO请求大致按照先来 后到的顺序进行操作。之所以说“大致”，原因是NOOP在FIFO的基础上还做了相邻IO请求的合并， 并不是完完全全按照先进先出的规则满足IO请求。NOOP假定I/O请求由驱动程序或者设备做了优 化或者重排了顺序(就像一个智能控制器完成的工作那样)。在有些SAN环境下，这个选择可能是最 好选择。Noop 对于 IO 不那么操心，对所有的 IO请求都用 FIFO 队列形式处理，默认认为 IO 不会 存在性能问题。这也使得 CPU 也不用那么操心。当然，对于复杂一点的应用类型，使用这个调度 器，用户自己就会非常操心。 

* CFQ  

  CFQ算法的全写为Completely Fair Queuing。该算法的特点是按照IO请求的地址进行排序，而不 是按照先来后到的顺序来进行响应。 在传统的SAS盘上，磁盘寻道花去了绝大多数的IO响应时 间。CFQ的出发点是对IO地址进行排序，以尽量少的磁盘旋转次数来满足尽可能多的IO请求。在 CFQ算法下，SAS盘的吞吐量大大提高了。但是相比于NOOP的缺点是，先来的IO请求并不一定能 被满足，可能会出现饿死的情况。 Completely Fair Queuing （cfq, 完全公平队列) 在 2.6.18 取代了 Anticipatory scheduler 成为 Linux Kernel 默认的 IO scheduler 。cfq 对每个进程维护一个 IO 队列，各个进程发来的 IO 请求 会被 cfq 以轮循方式处理。也就是对每一个 IO 请求都是公平的。这使得 cfq 很适合离散读的应用 (eg: OLTP DB) 

* Deadline scheduler  

  DEADLINE在CFQ的基础上，解决了IO请求饿死的极端情况。deadline 算法保证对于既定的 IO 请 求以最小的延迟时间，除了CFQ本身具有的IO排序队列之外，DEADLINE额外分别为读IO和写IO提 供了FIFO队列。读FIFO队列的最大等待时间为500ms，写FIFO队列的最大等待时间为5s。FIFO队 列内的IO请求优先级要比CFQ队列中的高，，而读FIFO队列的优先级又比写FIFO队列的优先级 高。优先级可以表示如下： FIFO(Read) > FIFO(Write) > CFQ  

* Anticipatory scheduler  

  CFQ和DEADLINE考虑的焦点在于满足零散IO请求上。对于连续的IO请求，比如顺序读，并没有做 优化。为了满足随机IO和顺序IO混合的场景，Linux还支持ANTICIPATORY调度算法。 ANTICIPATORY的在DEADLINE的基础上，为每个读IO都设置了6ms 的等待时间窗口。如果在这 6ms内OS收到了相邻位置的读IO请求，就可以立即满足 Anticipatory scheduler（as) 曾经一度 是 Linux 2.6 Kernel 的 IO scheduler 。Anticipatory 的中文含义是”预料的, 预想的”, 这个词的确揭 示了这个算法的特点，简单的说，有个 IO 发生的时候，如果又有进程请求 IO 操作，则将产生一 个默认的 6 毫秒猜测时间，猜测下一个进程请求 IO 是要干什么的。这对于随即读取会造成比较大 的延时，对数据库应用很糟糕，而对于 Web Server 等则会表现的不错。这个算法也可以简单理解 为面向低速磁盘的，因为那个”猜测”实际上的目的是为了减少磁头移动时间。

### 11.2 进程管理和性能相关工具

Linux系统状态的查看及管理工具： 

pstree 

ps 

pidof 

pgrep 

top 

htop 

glance 

pmap 

vmstat 

dstat 

kill 

pkill 

job 

bg

fg 

nohup

#### 11.2.1 进程树 pstree

pstree 可以用来显示进程的父子关系，以树形结构显示

格式：

```
pstree 可以用来显示进程的父子关系，以树形结构显示
```

常用选项：

```
-p 显示PID
-T 不显示线程thread,默认显示线程
-u 显示用户切换
-H pid 高亮显示指定进程及其前辈进程
```

#### 11.2.2 进程信息 ps

ps 即 process state，可以进程当前状态的快照，默认显示当前终端中的进程，Linux系统各进程的相 关信息均保存在/proc/PID目录下的各文件中

ps格式

```
ps [OPTION]...
```

支持三种选项： 

* UNIX选项 如: -A -e 
* GNU选项 如: --help 
* BSD选项 如: a

常用选项：

```
a 选项包括所有终端中的进程
x 选项包括不链接终端的进程
u 选项显示进程所有者的信息
f 选项显示进程树,相当于 --forest
k|--sort 属性 对属性排序,属性前加 - 表示倒序
o 属性… 选项显示定制的信息 pid、cmd、%cpu、%mem
L 显示支持的属性列表
-C cmdlist 指定命令，多个命令用，分隔
-L 显示线程
-e 显示所有进程，相当于-A
-f 显示完整格式程序信息
-F 显示更完整格式的进程信息
-H 以进程层级格式显示进程相关信息
-u userlist 指定有效的用户ID或名称
-U userlist 指定真正的用户ID或名称
-g gid或groupname 指定有效的gid或组名称
-G gid或groupname 指定真正的gid或组名称
-p pid 显示指pid的进程
--ppid pid 显示属于pid的子进程
-t ttylist 指定tty,相当于 t
-M 显示SELinux信息，相当于Z
```

**ps 输出属性**

```
C :  ps -ef 显示列 C 表示cpu利用率
VSZ: Virtual memory SiZe，虚拟内存集，线性内存
RSS: ReSident Size, 常驻内存集
STAT：进程状态
  R：running
  S: interruptable sleeping
  D: uninterruptable sleeping
  T: stopped
  Z: zombie
  +: 前台进程
  l: 多线程进程
  L：内存分页并带锁
  N：低优先级进程
  <: 高优先级进程
  s: session leader，会话（子进程）发起者
  I：Idle kernel thread，CentOS 8 新特性
ni: nice值
pri: priority 优先级
rtprio: 实时优先级
psr: processor CPU编号
```

**常用组合：**

```
aux
-ef
-eFH
-eo pid,tid,class,rtprio,ni,pri,psr,pcpu,stat,comm
 axo stat,euid,ruid,tty,tpgid,sess,pgrp,ppid,pid,pcpu,comm
```

#### 11.2.3 查看进程信息 prtstat

可以显示进程信息,来自于psmisc包

格式：

```
可以显示进程信息,来自于psmisc包
```

选项：

-r raw 格式显示

#### 11.2.4 设置和调整进程优先级

进程优先级调整 

* 静态优先级：100-139 
* 进程默认启动时的nice值为0，优先级为120 
* 只有根用户才能降低nice值（提高优先性）

**nice命令** 

以指定的优先级来启动进程

```
nice [OPTION] [COMMAND [ARG]...]
-n, --adjustment=N   add integer N to the niceness (default 10)
```

**renice命令** 

可以调整正在执行中的进程的优先级

```
renice [-n] priority pid...
```

查看

ps axo pid,comm,ni

#### 11.2.5 搜索进程

按条件搜索进程 

* ps 选项 | grep 'pattern' 灵活 
* pgrep 按预定义的模式 
* /sbin/pidof 按确切的程序名称查看pid 

##### 11.2.5.1 pgrep

命令格式

```
pgrep [options] pattern
```

常用选项

```
-u uid: effective user，生效者
-U uid: real user，真正发起运行命令者
-t terminal: 与指定终端相关的进程
-l: 显示进程名
-a: 显示完整格式的进程名
-P pid: 显示指定进程的子进程
```

##### 11.2.5.2 pidof

命令格式

```
pidof [options] [program [...]]
```

常用选项

```
-x 按脚本名称查找pid
```

#### 11.2.6 负载查询 uptime

/proc/uptime 包括两个值，单位 s 

* 系统启动时长 
* 空闲进程的总时长（按总的CPU核数计算）

uptime 和 w 显示以下内容 

* 当前时间 
* 系统已启动的时间 
* 当前上线人数 
* 系统平均负载（1、5、15分钟的平均负载，一般不会超过1，超过5时建议警报）

uptime 和 w 显示以下内容 当前时间 系统已启动的时间 当前上线人数 系统平均负载（1、5、15分钟的平均负载，一般不会超过1，超过5时建议警报）

#### 11.2.7 显示CPU相关统计 mpstat

来自于sysstat包

```bash
[root@node-1 ~]# mpstat
Linux 3.10.0-693.el7.x86_64 (node-1)    01/21/2022      _x86_64_        (64 CPU)

09:14:40 AM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
09:14:40 AM  all    2.70    0.00    0.80    0.01    0.00    0.05    0.00    0.00    0.00   96.44
[root@node-1 ~]#
```

#### 11.2.8 查看进程实时状态 top 和 htop

##### 11.2.8.1 top

```bash
[root@node-1 ~]# top
top - 09:16:32 up 300 days, 18:16,  2 users,  load average: 73.61, 73.59, 73.74
Tasks: 975 total,   1 running, 973 sleeping,   0 stopped,   1 zombie
%Cpu(s):  0.9 us,  0.4 sy,  0.0 ni, 98.6 id,  0.1 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem : 32863088+total, 14939692+free, 10536587+used, 73868080 buff/cache
KiB Swap:        0 total,        0 free,        0 used. 21657190+avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
15012 root      20   0  9.845g 186284  38632 S  21.3  0.1  23187:08 kubelet
14815 root      20   0  582404 398844  43176 S   9.2  0.1   7040:18 kube-apiserver
38633 1000      20   0 38.029g 0.033t  30120 S   7.9 10.7 717:56.56 java
39059 root      20   0 5284956 180588  30152 S   5.6  0.1  67831:32 dockerd
63493 root      20   0 10.107g 102740  26856 S   4.3  0.0   3073:00 etcd
 9915 33        20   0 11.992g 1.977g  29160 S   3.9  0.6 150:27.48 java
13997 root      20   0  290832 111428  33008 S   3.3  0.0   2914:56 kube-controller
 8469 33        20   0 11.535g 3.055g  24912 S   3.0  1.0 880:36.16 java
57899 polkitd   20   0 15.564g 0.014t  20600 S   3.0  4.5  10758:22 mongod
15875 346       20   0 8326648 647128  13044 S   2.6  0.2   1093:09 java
38144 polkitd   20   0 5587696 1.254g  14252 S   2.3  0.4   1652:37 mysqld
12225 33        20   0 11.249g 1.262g  26116 S   2.0  0.4 203:04.86 java
51844 346       20   0 12.084g 641948  15088 S   1.6  0.2 633:51.77 java
60380 9999      20   0 37.592g 0.016t  21024 S   1.3  5.3 411:15.07 java
63054 root      20   0  158536   3148   1536 R   1.3  0.0   0:00.40 top
   10 root      20   0       0      0      0 S   1.0  0.0   3653:08 rcu_sched
 4136 33        20   0 9935.9m 875456  23720 S   1.0  0.3 223:48.27 java
11005 33        20   0 9846.0m 434492  23748 S   1.0  0.1 222:28.31 java
24597 33        20   0 13.474g 4.271g  25836 S   1.0  1.4 244:57.87 java
30548 33        20   0 9352752 1.000g  23176 S   1.0  0.3 180:31.11 java
43657 33        20   0  9.984g 921392  17332 S   1.0  0.3 330:24.88 java
46639 33        20   0  9.812g 633596  24880 S   1.0  0.2 390:39.31 java
59229 9999      20   0 8317628 1.233g  20748 S   1.0  0.4 355:36.87 java
62647 33        20   0  9.781g 1.905g  24456 S   1.0  0.6 415:11.19 java
 3301 root      20   0   46672   6968    796 S   0.7  0.0   2463:01 hasplmd
14048 33        20   0 9788.0m 441816  22832 S   0.7  0.1 197:10.93 java
14086 root      20   0  153572  55136  18580 S   0.7  0.0 650:30.73 kube-scheduler
14384 33        20   0  9.858g 728084  23704 S   0.7  0.2  94:36.78 java
```

top 提供动态的实时进程状态 

有许多内置命令

```
帮助：h 或 ？ ，按 q 或esc 退出帮助

排序：
P：以占据的CPU百分比,%CPU
M：占据内存百分比,%MEM
T：累积占据CPU时长,TIME+

首部信息显示：
uptime信息：l命令
tasks及cpu信息：t命令
cpu分别显示：1 (数字)
memory信息：m命令

退出命令：q
修改刷新时间间隔：s
终止指定进程：k
保存文件：W
```

top命令栏位信息简介

```
us：用户空间
sy：内核空间
ni：调整nice时间
id：空闲
wa：等待IO时间
hi：硬中断
si：软中断（模式切换）
st：虚拟机偷走的时间
```

top选项：

```
-d # 指定刷新时间间隔，默认为3秒
-b 全部显示所有进程
-n # 刷新多少次后退出
-H   线程模式
```

##### 11.2.8.2 htop

htop 命令是增强版的TOP命令，来自EPEL源，比top功能更强

选项：

```
-d #: 指定延迟时间；
-u UserName: 仅显示指定用户的进程
-s COLUME: 以指定字段进行排序
```

子命令：

```
s：跟踪选定进程的系统调用
l：显示选定进程打开的文件列表
a：将选定的进程绑定至某指定CPU核心
t：显示进程树
```

#### 11.2.9 内存空间 free

free 可以显示内存空间使用状态

格式：

```
free [OPTION]
```

常用选项：

```
-b 以字节为单位
-m 以MB为单位
-g 以GB为单位
-h 易读格式
-o 不显示-/+buffers/cache行
-t   显示RAM + swap的总和
-s n 刷新间隔为n秒
-c n 刷新n次后即退出
```

向/proc/sys/vm/drop_caches中写入相应的修改值，会清理缓存。建议先执行sync（sync 命令将所有 未写的系统缓冲区写到磁盘中，包含已修改的 i-node、已延迟的块 I/O 和读写映射文件）。执行echo  1、2、3 至 /proc/sys/vm/drop_caches, 达到不同的清理目的

如果因为是应用有像内存泄露、溢出的问题时，从swap的使用情况是可以比较快速可以判断的，但通 过执行free 反而比较难查看。但核心并不会因为内存泄露等问题并没有快速清空buffer或cache（默认 值是0），生产也不应该随便去改变此值。

一般情况下，应用在系统上稳定运行了，free值也会保持在一个稳定值的。当发生内存不足、应用获取 不到可用内存、OOM错误等问题时，还是更应该去分析应用方面的原因，否则，清空buffer，强制腾 出free的大小，可能只是把问题给暂时屏蔽了。

排除内存不足的情况外，除非是在软件开发阶段，需要临时清掉buffer，以判断应用的内存使用情况； 或应用已经不再提供支持，即使应用对内存的时候确实有问题，而且无法避免的情况下，才考虑定时清 空buffer。

```bash
[root@node-1 ~]# free -mhl
              total        used        free      shared  buff/cache   available
Mem:           313G        100G        142G        4.0G         70G        206G
Low:           313G        170G        142G
High:            0B          0B          0B
Swap:            0B          0B          0B
[root@node-1 ~]#
```

**清理缓存**

```bash
echo 3 > /proc/sys/vm/drop_caches
```

#### 11.2.10 进程对应的内存映射 pmap

格式：

```
pmap [options] pid [...]
```

常用选项：

```
-x: 显示详细格式的信息
```

#### 11.2.11 虚拟内存信息 vmstat

格式：

```
vmstat [options] [delay [count]] 
```

显示项说明：

```
procs:
  r：可运行（正运行或等待运行）进程的个数，和核心数有关
  b：处于不可中断睡眠态的进程个数(被阻塞的队列的长度)
memory：
  swpd: 交换内存的使用总量
  free：空闲物理内存总量
  buffer：用于buffer的内存总量
  cache：用于cache的内存总量
swap:
  si：从磁盘交换进内存的数据速率(kb/s)
  so：从内存交换至磁盘的数据速率(kb/s)
io：
  bi：从块设备读入数据到系统的速率(kb/s)
  bo: 保存数据至块设备的速率
system：
  in: interrupts 中断速率，包括时钟
  cs: context switch     进程切换速率
cpu：
  us:Time spent running non-kernel code
  sy: Time spent running kernel code
  id: Time spent idle. Linux 2.5.41前,包括IO-wait time.
  wa: Time spent waiting for IO.  2.5.41前，包括in idle.
  st: Time stolen from a virtual machine.  2.6.11前, unknown.
```

选项：

```
-s 显示内存的统计数据
```

#### 11.2.12 统计CPU和设备IO信息 iostat

iostat 可以提供更丰富的IO性能状态数据 

此工具由sysstat包提供

```
常用选项:
-c 只显示CPU行
-d 显示设备〈磁盘)使用状态
-k 以千字节为为单位显示输出
-t 在输出中包括时间戳
-x 在输出中包括扩展的磁盘指标
```

```bash
[root@node-1 proc]# iostat
Linux 3.10.0-693.el7.x86_64 (node-1)    01/21/2022      _x86_64_        (64 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           2.70    0.00    0.85    0.01    0.00   96.44

Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
sda               4.08         0.33        64.02    8683845 1663861752
sdb              11.76         0.19       137.88    4868684 3583309864
dm-0              4.18         0.33        64.02    8648456 1663858820

[root@node-1 proc]#

tps：该设备每秒的传输次数（Indicate the number of transfers per second that were issued to the device.）。"一次传输"意思是"一次I/O请求"。多个逻辑请求可能会被合并为"一次I/O请求"。"一次传输"请求的大小是未知的。

kB_read/s：每秒从设备（drive expressed）读取的数据量；
kB_wrtn/s：每秒向设备（drive expressed）写入的数据量；
kB_read：读取的总数据量；
kB_wrtn：写入的总数量数据量；这些单位都为Kilobytes。

#输出说明
r/s: 每秒合并后读的请求数
w/s: 每秒合并后写的请求数
rsec/s：每秒读取的扇区数；
wsec/：每秒写入的扇区数。
rKB/s：The number of read requests that were issued to the device per second；
wKB/s：The number of write requests that were issued to the device per second；
rrqm/s：每秒这个设备相关的读取请求有多少被Merge了（当系统调用需要读取数据的时候，VFS将请求发到各个FS，如果FS发现不同的读取请求读取的是相同Block的数据，FS会将这个请求合并Merge）；
wrqm/s：每秒这个设备相关的写入请求有多少被Merge了。
%rrqm: The percentage of read requests merged together before being sent to the device.
%wrqm: The percentage of write requests merged together before being sent to the device.
avgrq-sz 平均请求扇区的大小
avgqu-sz 是平均请求队列的长度。毫无疑问，队列长度越短越好。    
await： 每一个IO请求的处理的平均时间（单位是微秒毫秒）。这里可以理解为IO的响应时间，一般地系统IO响应时间应该低于5ms，如果大于10ms就比较大了。这个时间包括了队列时间和服务时间，也就是说，一般情况下，await大于svctm，它们的差值越小，则说明队列时间越短，反之差值越大，队列时间越长，说明系统出了问题。
svctm   表示平均每次设备I/O操作的服务时间（以毫秒为单位）。如果svctm的值与await很接近，表示几乎没有I/O等待，磁盘性能很好，如果await的值远高于svctm的值，则表示I/O队列等待太长，系统上运行的应用程序将变慢。
%util： 在统计时间内所有处理IO时间，除以总共统计时间。例如，如果统计间隔1秒，该设备有0.8秒在处理IO，而0.2秒闲置，那么该设备的%util = 0.8/1 = 80%，所以该参数暗示了设备的繁忙程度。一般地，如果该参数是100%表示设备已经接近满负荷运行了（当然如果是多磁盘，即使%util是100%，因为磁盘的并发能力，所以磁盘使用未必就到了瓶颈）。
```

#### 11.2.13 监视磁盘I/O iotop

iotop命令是一个用来监视磁盘I/O使用状况的top类工具iotop具有与top相似的UI，其中包括PID、用 户、I/O、进程等相关信息，可查看每个进程是如何使用IO

iotop输出 

* 第一行：Read和Write速率总计 

* 第二行：实际的Read和Write速率 

* 第三行：参数如下： 

  线程ID（按p切换为进程ID） 

  优先级 

  用户 

  磁盘读速率

  磁盘写速率 

  swap交换百分比 

  IO等待所占的百分比

iotop常用参数

```
-o, --only只显示正在产生I/O的进程或线程，除了传参，可以在运行过程中按o生效
-b, --batch非交互模式，一般用来记录日志
-n NUM, --iter=NUM设置监测的次数，默认无限。在非交互模式下很有用
-d SEC, --delay=SEC设置每次监测的间隔，默认1秒，接受非整形数据例如1.1
-p PID, --pid=PID指定监测的进程/线程
-u USER, --user=USER指定监测某个用户产生的I/O
-P, --processes仅显示进程，默认iotop显示所有线程
-a, --accumulated显示累积的I/O，而不是带宽
-k, --kilobytes使用kB单位，而不是对人友好的单位。在非交互模式下，脚本编程有用
-t, --time 加上时间戳，非交互非模式
-q, --quiet 禁止头几行，非交互模式，有三种指定方式
-q 只在第一次监测时显示列名
-qq 永远不显示列名
-qqq 永远不显示I/O汇总
```

交互按键

```
left和right方向键：改变排序
r：反向排序
o：切换至选项--only
p：切换至--processes选项
a：切换至--accumulated选项
q：退出
i：改变线程的优先级
```

#### 11.2.14 显示网络带宽使用情况 iftop

```bash
[root@centos8 ~]#iftop -ni eth0 
```

#### 11.2.15 查看网络实时吞吐量 nload

nload 是一个实时监控网络流量和带宽使用情况，以数值和动态图展示进出的流量情况,通过EPEL源安 装 

界面操作

```
上下方向键、左右方向键、enter键或者tab键都就可以切换查看多个网卡的流量情况
按 F2 显示选项窗口
按 q 或者 Ctrl+C 退出 nload
```

范例：

```bash
#默认只查看第一个网络的流量进出情况
nload
#在nload后面指定网卡，可以指定多个,按左右键分别显示网卡状态
nload eth0 eth1
#设置刷新间隔：默认刷新间隔是100毫秒，可通过 -t 命令设置刷新时间（单位是毫秒）
nload -t 500 eth0
#设置单位：显示两种单位一种是显示Bit/s、一种是显示Byte/s，默认是以Bit/s，也可不显示/s
#-u h|b|k|m|g|H|B|K|M|G 表示的含义： h: auto, b: Bit/s, k: kBit/s, m: MBit/s, H: 
auto, B: Byte/s, K: kByte/s, M: MByte/s
nload -u M eth
```

#### 11.2.16 网络监视工具iptraf-ng

来自于iptraf-ng包,可以进网络进行监控,对终端窗口大小有要求.

#### 11.2.17 系统资源统计 dstat

dstat由pcp-system-tools包提供，但安装dstat包即可, 可用于代替 vmstat,iostat功能

格式：

```
dstat [-afv] [options..] [delay [count]]
```

常用选项

```
-c 显示cpu相关信息
-C #,#,...,total
-d 显示disk相关信息
-D total,sda,sdb,...
-g 显示page相关统计数据
-m 显示memory相关统计数据
-n 显示network相关统计数据
-p 显示process相关统计数据
-r 显示io请求相关的统计数据
-s 显示swapped相关的统计数据
--tcp
--udp
--unix
--raw
--socket
--ipc
--top-cpu：显示最占用CPU的进程
--top-io: 显示最占用io的进程
--top-mem: 显示最占用内存的进程
--top-latency: 显示延迟最大的进程
```

#### 11.2.18 综合监控工具 glances

此工具可以通过EPEL源安装,CentOS 8 目前没有提供(已提供,但测试问题) 

格式：

```
glances [-bdehmnrsvyz1] [-B bind] [-c server] [-C conffile] [-p port] [-P 
password] [--password] [-t refresh] [-f file] [-o output]
```

内建命令

```
a Sort processes automatically     l Show/hide logs
c Sort processes by CPU%           b Bytes or bits for network I/O
m Sort processes by MEM%         w Delete warning logs
p Sort processes by name           x Delete warning and critical logs
i Sort processes by I/O rate       1 Global CPU or per-CPU stats
d Show/hide disk I/O stats         h Show/hide this help screen
f Show/hide file system stats     t View network I/O as combination
n Show/hide network stats         u View cumulative network I/O
s Show/hide sensors stats         q Quit (Esc and Ctrl-C also work)
y Show/hide hddtemp stats
```

常用选项：

```
-b: 以Byte为单位显示网卡数据速率
-d: 关闭磁盘I/O模块
-f /path/to/somefile: 设定输入文件位置
-o {HTML|CSV}：输出格式
-m: 禁用mount模块
-n: 禁用网络模块
-t #: 延迟时间间隔
-1：每个CPU的相关数据单独显示
```

**C/S模式下运行glances命令** 

* 服务器模式： 

  glances -s -B IPADDR 

  ​	IPADDR: 指明监听的本机哪个地址,端口默认为61209/tcp 

* 客户端模式： 

  glances -c IPADDR 

  ​	IPADDR：要连入的服务器端地址

#### 11.2.19 查看进程打开文件 lsof

lsof：list open files，查看当前系统文件的工具。在linux环境下，一切皆文件，用户通过文件不仅可以 访问常规数据，还可以访问网络连接和硬件如传输控制协议 (TCP) 和用户数据报协议 (UDP)套接字等， 系统在后台都为该应用程序分配了一个文件描述符

命令选项：

```
-a：列出打开文件存在的进程
-c<进程名>：列出指定进程所打开的文件
-g：列出GID号进程详情
-d<文件号>：列出占用该文件号的进程
+d<目录>：列出目录下被打开的文件
+D<目录>：递归列出目录下被打开的文件
-n<目录>：列出使用NFS的文件
-i<条件>：列出符合条件的进程(4、6、协议、:端口、 @ip )
-p<进程号>：列出指定进程号所打开的文件
-u：列出UID号进程详情
-h：显示帮助信息
-v：显示版本信息。
-n: 不反向解析网络名字
```

#### 11.2.20 综合管理平台 webmin

官网:http://www.webmin.com/ 

下载:http://www.webmin.com/download.html 

Webmin是目前功能最强大的基于Web的Unix系统管理工具。管理员通过浏览器访问Webmin的各种管 理功能并完成相应的管理动作。目前Webmin支持绝大多数的Unix系统，这些系统除了各种版本的linux 以外还包括：AIX、HPUX、Solaris、Unixware、Irix和FreeBSD等

#### 11.2.21 CentOS 8 新特性 cockpit

由cockpit包提供,当前Ubuntu和CentOS7也支持此工具 

Cockpit 是CentOS 8 取入的新特性，是一个基于 Web 界面的应用，它提供了对系统的图形化管理

* 监控系统活动（CPU、内存、磁盘 IO 和网络流量） 
* 查看系统日志条目 
* 查看磁盘分区的容量 
* 查看网络活动（发送和接收） 
* 查看用户帐户 
* 检查系统服务的状态 
* 提取已安装应用的信息 
* 查看和安装可用更新（如果以 root 身份登录）
* 并在需要时重新启动系统 打开并使用终端窗口

#### 11.2.22 信号发送 kill

kill：内部命令，可用来向进程发送控制信号，以实现对进程管理,每个信号对应一个数字，信号名称以 SIG开头（可省略），不区分大小写 

显示当前系统可用信号：

```
kill -l   
trap -l
```

常用信号：

```
1) SIGHUP 无须关闭进程而让其重读配置文件
2) SIGINT 中止正在运行的进程；相当于Ctrl+c
3) SIGQUIT 相当于ctrl+\
9) SIGKILL 强制杀死正在运行的进程,可能会导致数据丢失,慎用!
15) SIGTERM 终止正在运行的进程，默认信号
18) SIGCONT 继续运行
19) SIGSTOP 后台休眠
```

指定信号的方法 : 

* 信号的数字标识：1, 2, 9 
* 信号完整名称：SIGHUP，sighup 
* 信号的简写名称：HUP，hup

向进程发送信号： 

按PID：

```
kill [-s sigspec | -n signum | -sigspec] pid | jobspec ... or kill -l [sigspec]
```

按名称：killall 来自于psmisc包

```
killall [-SIGNAL] comm…
```

按模式：

```
pkill [options] pattern
```

常用选项：

```
-SIGNAL
-u uid: effective user，生效者
-U uid: real user，真正发起运行命令者
-t terminal: 与指定终端相关的进程
-l: 显示进程名（pgrep可用）
-a: 显示完整格式的进程名（pgrep可用）
-P pid: 显示指定进程的子进程
```

#### 11.2.23 作业管理

Linux的作业控制 

* 前台作业：通过终端启动，且启动后一直占据终端 
* 后台作业：可通过终端启动，但启动后即转入后台运行（释放终端）

让作业运行于后台 

* 运行中的作业： Ctrl+z 
* 尚未启动的作业： COMMAND &

后台作业虽然被送往后台运行，但其依然与终端相关；退出终端，将关闭后台作业。如果希望送往后台 后，剥离与终端的关系 

* nohup COMMAND &>/dev/null &  
* screen；COMMAND 
* tmux；COMMAND

查看当前终端所有作业：

```
jobs
```

作业控制

```
fg [[%]JOB_NUM]：把指定的后台作业调回前台
bg [[%]JOB_NUM]：让送往后台的作业在后台继续运行
kill [%JOB_NUM]： 终止指定的作业
```

#### 11.2.24 并行运行

利用后台执行，实现并行功能，即同时运行多个进程，提高效率

方法1

```
cat all.sh
f1.sh&
f2.sh&
f3.sh&
```

方法2

```
(f1.sh&);(f2.sh&);(f3.sh&)
```

方法3

```
f1.sh&f2.sh&f3.sh& 
```

### 11.3 任务计划

通过任务计划，可以让系统自动的按时间或周期性任务执行任务 

注意: 学习本节需要实现邮件通知,学习内容前必须安装并启动邮件服务

范例：环境准备

```bash
[root@centos8 ~]#yum -y install postfix
[root@centos8 ~]#systemctl enable --now postfix
```

未来的某时间点执行一次任务 

* at 指定时间点，执行一次性任务 
* batch 系统自行选择空闲时间去执行此处指定的任务

周期性运行某任务 

* cron

#### 11.3.1 一次性任务

at 工具 

* 由包 at 提供 
* 依赖与atd服务,需要启动才能实现at任务 
* at队列存放在/var/spool/at目录中,ubuntu存放在/var/spool/cron/atjobs目录下 
* 执行任务时PATH变量的值和当前定义任务的用户身份一致

at 命令：

```
at [option] TIME
```

常用选项：

```
-V 显示版本信息
-t time   时间格式 [[CC]YY]MMDDhhmm[.ss] 
-l 列出指定队列中等待运行的作业；相当于atq
-d N 删除指定的N号作业；相当于atrm
-c N 查看具体作业N号任务
-f file 指定的文件中读取任务
-m 当任务被完成之后，将给用户发送邮件，即使没有标准输出
```

注意： 

* 作业执行命令的结果中的标准输出和错误以执行任务的用户身份发邮件通知给 root  
* 默认CentOS 8 最小化安装没有安装邮件服务,需要自行安装

TIME：定义出什么时候进行 at 这项任务的时间

```
HH:MM [YYYY-mm-dd]
noon, midnight, teatime（4pm）,tomorrow
now+#{minutes,hours,days, OR weeks}
```

at 任务执行方式： 

* 交互式 
* 输入重定向 
* at -f file

/etc/at.{allow,deny} 控制用户是否能执行at任务 

* 白名单：/etc/at.allow 默认不存在，只有该文件中的用户才能执行at命令 
* 黑名单：/etc/at.deny 默认存在，拒绝该文件中用户执行at命令，而没有在at.deny 文件中的使用 者则可执行 
* 如果两个文件都不存在，只有 root 可以执行 at 命令

#### 11.3.2 周期性任务计划 cron

周期性任务计划cron相关的程序包：

* cronie：主程序包，提供crond守护进程及相关辅助工具 
* crontabs：包含CentOS提供系统维护任务 
* cronie-anacron：cronie的补充程序，用于监控cronie任务执行状况，如:cronie中的任务在过去 该运行的时间点未能正常运行，则anacron会随后启动一次此任务

cron 依赖于crond服务，确保crond守护处于运行状态：

```bash
#CentOS 7 以后版本:
systemctl status crond

#CentOS 6:
service crond status
```

cron任务分为 

* 系统cron任务：系统维护作业，/etc/crontab 主配置文件， /etc/cron.d/ 子配置文件 
* 用户cron任务：保存在 /var/spool/cron/USERNAME(ubuntu 系统存放 在/var/spool/cron/crontabs/USERNAME)，利用 crontab 命令管理 

计划任务日志：/var/log/cron

##### 11.3.2.1 系统cron计划任务 

/etc/crontab 格式说明，详情参见 man 5 crontab

注释行以#开头

```bash
[root@node-1 ~]# cat /etc/crontab
SHELL=/bin/bash											#默认的shell类型
PATH=/sbin:/bin:/usr/sbin:/usr/bin						#默认的PATH变量值，可修改为其他路径
MAILTO=root												#默认标准输出和错误发邮件给root，可以指向其他用户

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

[root@node-1 ~]#
```

**计划任务时间表示法：**

```
(1) 特定值
 给定时间点有效取值范围内的值
(2) *
 给定时间点上有效取值范围内的所有值,表示“每...”,放在星期的位置表示不确定
(3) 离散取值
 #,#,#
(4) 连续取值
 #-#
(5) 在指定时间范围上，定义步长
 /#: #即为步长
(6) 特定关健字
@yearly 0 0 1 1 *
@annually 0 0 1 1 *
@monthly 0 0 1 * *
@weekly 0 0 * * 0
@daily 0 0 * * *
@hourly 0 * * * *
@reboot Run once after reboot
```

crond任务相关文件：

```bash
/etc/crontab 			#配置文件
/etc/cron.d/ 			#配置文件
/etc/cron.hourly/ 		#脚本
/etc/cron.daily/ 		#脚本
/etc/cron.weekly/ 		#脚本
/etc/cron.monthly/ 		#脚本
```

##### 11.3.2.2 anacron

运行计算机关机时cron不运行的任务，CentOS6以后版本取消anacron服务，由crond服务管理，对笔 记本电脑、台式机、工作站、偶尔要关机的服务器及其它不一直开机的系统很重要对很有用

由/etc/cron.hourly/0anacron执行，当执行任务时，更新/var/spool/anacron/cron.daily 文件的时间 戳

配置文件：/etc/anacrontab，负责执行/etc/ cron.daily /etc/cron.weekly /etc/cron.monthly中系统任 务 

/etc/anacrontab格式说明

```
字段1：如果在这些日子里没有运行这些任务
字段2：在重新引导后等待这么多分钟后运行它
字段3：任务识别器，在日志文件中标识
字段4：要执行的任务
```

##### 10.3.2.3 管理临时文件

CentOS 7 使用 systemd-tmpfiles-setup服务实现 

CentOS 6 使用/etc/cron.daily/tmpwatch定时清除临时文件

配置文件：

```
/etc/tmpfiles.d/*.conf
/run/tmpfiles.d/*.conf
/usr/lib/tmpfiles/*.conf
```

/usr/lib/tmpfiles.d/tmp.conf

```
d /tmp 1777 root root 10d
d /var/tmp 1777 root root 30d
```

命令：

```
systemd-tmpfiles –clean|remove|create configfile
```

##### 11.3.2.4 用户计划任务

crontab命令:  

* 每个用户都有专用的cron任务文件：/var/spool/cron/USERNAME 
* 默认标准输出和错误会被发邮件给对应的用户,如：wang创建的任务就发送至wang的邮箱 
* root能够修改其它用户的作业 
* 用户的cron 中默认 PATH=/usr/bin:/bin,如果使用其它路径,在任务文件的第一行加PATH=/path或 者加入到计划任务执行的脚本中 
* 第六个字段指定要运行的命令。 该行的整个命令部分，直至换行符或“％”字符，指定的shell执行.  除非使用反斜杠（\）进行转义，否则该命令中的“％”字符将变为换行符，并且第一个％之后的所 有数据将作为标准输入发送到该命令。

crontab命令格式：

```bash
crontab [-u user] [-l | -r | -e] [-i] 
```

常用选项：

```
-l 列出所有任务
-e 编辑任务
-r 移除所有任务
-i 同-r一同使用，以交互式模式移除指定任务
-u user 指定用户管理cron任务,仅root可运行
```

控制用户执行计划任务： 

/etc/cron.{allow,deny}

注意：运行结果的标准输出和错误以邮件通知给相关用户

```
(1) COMMAND > /dev/null 
(2) COMMAND &> /dev/null
```

cron任务中不建议使用%，它有特殊用途，它表示换行的特殊意义，且第一个%后的所有字符串会被将 成当作命令的标准输入,如果在命令中要使用%，则需要用 \ 转义 

注意：将%放置于单引号中是不支持的

# 12 系统启动和内核管理

### 内容概述

* CentOS 6 之前版本的启动流程 
* 服务管理 
* Grub管理 
* 启动排错 
* 编译安装内核 
* CentOS 7 以后版本启动流程 
* Unit 介绍 
* 服务管理和查看 
* 启动排错 
* 破解口令 
* 修复Grub2

### 12.1 CentOS 6 的启动管理

#### 12.1.1 Linux 组成

* kernel 实现进程管理、内存管理、网络管理、驱动程序、文件系统、安全功能等功能 

* rootfs 包括程序和 glibc 库 

  程序：二进制执行文件 

  库：函数集合, function, 调用接口（头文件负责描述）

#### 12.1.2 内核设计流派

* 宏内核(monolithic kernel)：又称单内核和强内核，Unix，Linux 

  把所有系统服务都放到内核里，所有功能集成于同一个程序，分层实现不同功能，系统庞大复杂， Linux其实在单内核内核实现了模块化，也就相当于吸收了微内核的优点 

* 微内核(micro kernel)：Windows，Solaris，HarmonyOS 

  简化内核功能，在内核之外的用户态尽可能多地实现系统服务，同时加入相互之间的安全保护，每 种功能使用一个单独子系统实现，将内核功能移到用户空间，性能差

#### 12.1.3 CentOS 6启动流程

##### 12.1.3.1 CentOS 6 启动流程

1. 加载BIOS的硬件信息，获取第一个启动设备 

2. 读取第一个启动设备MBR的引导加载程序(grub)的启动信息 
3.  加载核心操作系统的核心信息，核心开始解压缩，并尝试驱动所有的硬件设备 
4. 核心执行init程序，并获取默认的运行信息 
5. init程序执行/etc/rc.d/rc.sysinit文件，重新挂载根文件系统 
6. 启动核心的外挂模块 
7. init执行运行的各个批处理文件(scripts) 
8. init执行/etc/rc.d/rc.local 
9. 执行/bin/login程序，等待用户登录 
10. 登录之后开始以Shell控制主机

##### 12.1.3.2 硬件启动POST 

POST：Power-On-Self-Test，加电自检，是BIOS功能的一个主要部分。负责完成对CPU、主板、内 存、硬盘子系统、显示子系统、串并行接口、键盘等硬件情况的检测

主板的ROM：BIOS，Basic Input and Output System，保存着有关计算机系统最重要的基本输入输出 程序，系统信息设置、开机加电自检程序和系统启动自举程序等 

主板的RAM：CMOS互补金属氧化物半导体，保存各项参数的设定，按次序查找引导设备，第一个有引 导程序的设备为本次启动设备

##### 12.1.3.3 启动加载器 bootloader

###### 12.1.3.3.1 grub 功能和组成

bootloader: 引导加载器，引导程序 

* Windows: ntloader，仅是启动OS 
* Linux：功能丰富，提供菜单，允许用户选择要启动系统或不同的内核版本；把用户选定的内核装 载到内存中的特定空间中，解压、展开，并把系统控制权移交给内核 

Linux的bootloader 

* LILO：LInux LOader，早期的bootloader，功能单一 
* GRUB: GRand Unified Bootloader, CentOS 5,6 GRUB 0.97: GRUB Legacy， CentOS 7 以后使 用GRUB 2.02

GRUB 启动阶段 

* primary boot loader :  

  1st stage：MBR的前446个字节 

  1.5 stage：MBR 之后的扇区，让stage1中的bootloader能识别stage2所在的分区上的文件系统 

* secondary boot loader ：2nd stage，分区文件/boot/grub/

###### 12.1.3.3.2 CentOS 6 grub 安装

安装 grub的两种方法：

 (1) grub-install 安装grub stage1和stage1_5到/dev/DISK磁盘上，并复制GRUB相关文件到 DIR/boot 目录下

```
grub-install --root-directory=DIR /dev/DISK
```

(2) grub命令

```
#grub
grub> root (hd#,#)
grub> setup (hd#)
```

###### 12.1.3.3.3 grub legacy 管理 

配置文件：/boot/grub/grub.conf <-- /etc/grub.conf 

stage2及内核等通常放置于一个基本磁盘分区

grub legacy 功用：

 (1) 提供启动菜单、并提供交互式接口 

​	a：内核参数 

​	e：编辑模式，用于编辑菜单 

​	c：命令模式，交互式接口 

(2) 加载用户选择的内核或操作系统 

​	允许传递参数给内核 

​	可隐藏启动菜单 

(3) 为菜单提供了保护机制 

​	为编辑启动菜单进行认证 

​	为启用内核或操作系统进行认证

grub的命令行接口

```
help: 获取帮助列表
help KEYWORD: 详细帮助信息
find (hd#,#)/PATH/TO/SOMEFILE：
root (hd#,#)
kernel /PATH/TO/KERNEL_FILE: 设定本次启动的内核文件；额外还可添加许多内核支持使用的
cmdline参数
 例如：max_loop=100 selinux=0 init=/path/to/init
initrd /PATH/TO/INITRAMFS_FILE: 设定为选定的内核提供额外文件的ramdisk
boot: 引导启动选定的内核
```

cat /proc/cmdline 内核参数

内核参数文档：

```
/usr/share/doc/kernel-doc-2.6.32/Documentation/kernel-parameters.txt
```

grub legacy识别硬盘设备

```
(hd#,#)
hd#: 磁盘编号，用数字表示；从0开始编号
#: 分区编号，用数字表示; 从0开始编号
示例：
(hd0,0) 第一块硬盘，第一个分区
```

手动在grub命令行接口启动系统

```
grub> root (hd#,#)
grub> kernel /vmlinuz-VERSION-RELEASE ro root=/dev/DEVICE 
grub> initrd /initramfs-VERSION-RELEASE.img
grub> boot
```

grub legacy配置文件：/boot/grub/grub.conf

```
default=#: #设定默认启动的菜单项；落单项(title)编号从0开始
timeout=#： #指定菜单项等待选项选择的时长
splashimage=(hd#,#)/PATH/XPM_FILE：#菜单背景图片文件路径
password [--md5| --encrypt] STRING: #启动菜单编辑认证
hiddenmenu：#隐藏菜单
title TITLE：#定义菜单项“标题”, 可出现多次
root (hd#,#)：#查找stage2及kernel文件所在设备分区；为grub的根
kernel /PATH/TO/VMLINUZ_FILE [PARAMETERS]：#启动的内核
initrd /PATH/TO/INITRAMFS_FILE: #内核匹配的ramfs文件
password [--md5|--encrypted ] STRING: #启动选定的内核或操作系统时进行认证
```

grub加密生成grub口令

```
grub-md5-crypt
grub-crypt
```

破解root口令：

```
(1) 编辑grub菜单(选定要编辑的title，而后使用a 或 e 命令)
(2) 在选定的kernel后附加1, s, S，single 都可以进入单用户模式
(3) 在kernel所在行，键入“b”命令
```

##### 12.1.3.4 加载 kernel

kernel 自身初始化过程 

1. 探测可识别到的所有硬件设备 
2. 加载硬件驱动程序（借助于ramdisk加载驱动） 
3. 以只读方式挂载根文件系统 
4. 运行用户空间的第一个应用程序：/sbin/init

Linux内核特点： 

* 支持模块化：.ko（内核对象），如：文件系统，硬件驱动，网络协议等 
* 支持内核模块的动态装载和卸载

内核组成部分： 

* 核心文件：/boot/vmlinuz-VERSION-release 

  ​	ramdisk：辅助的伪根系统，加载相应的硬件驱动，ramdisk --> ramfs 提高速度 

  ​	CentOS 5 /boot/initrd-VERSION-release.img 

  ​	CentOS 6 以后版本 /boot/initramfs-VERSION-release.img  

* 模块文件：/lib/modules/VERSION-release

 ramdisk文件的制作： 

* mkinitrd命令

```
mkinitrd /boot/initramfs-$(uname -r).img $(uname -r)
```

* dracut命令

```
dracut /boot/initramfs-$(uname -r).img $(uname -r) 
```

##### 12.1.3.5 init初始化

POST --> BootSequence (BIOS) --> Bootloader(MBR) --> kernel(ramdisk) --> rootfs(只读) -->  init（systemd） 

init程序的类型： 

SysV: init, CentOS 5之前 

​		配置文件：/etc/inittab 

Upstart: init,CentOS 6 

​		配置文件：/etc/inittab, /etc/init/*.conf 

Systemd：systemd, CentOS 7 

​		配置文件：/usr/lib/systemd/system /etc/systemd/system

###### 12.1.3.5.1 运行级别

运行级别：为系统运行或维护等目的而设定；0-6：7个级别，一般使用3, 5做为默认级别

```
0：关机
1：单用户模式(root自动登录), single, 维护模式
2：多用户模式，启动网络功能，但不会启动NFS；维护模式
3：多用户模式，正常模式；文本界面
4：预留级别；可同3级别
5：多用户模式，正常模式；图形界面
6：重启
```

切换级别：

```
init #
```

查看级别：

```
runlevel 
who -r
```

定义运行级别

```
/etc/inittab
```

CentOS 5 的inittab文件还定义以下内容

```
初始运行级别(RUN LEVEL)
系统初始化脚本
对应运行级别的脚本目录
捕获某个关键字顺序
定义UPS电源终端/恢复脚本
在虚拟控制台生成gett
```

CentOS 5 的inittab文件每一行格式：

```
id:runlevel:action:process

id：是惟一标识该项的字符序列
runlevels： 定义了操作所使用的运行级别
action： 指定了要执行的特定操作
     wait: 切换至此级别运行一次
 	 respawn：此process终止，就重新启动之
 	 initdefault：设定默认运行级别；process省略
 	 sysinit：设定系统初始化方式
process：定义了要执行的进程
```

CentOS 6 /etc/inittab和相关文件 

CentOS 6 init程序为 upstart, 其配置文件/etc/inittab, /etc/init/*.conf，配置文件的语法 遵循 upstart 配置文件语法格式，和CentOS5不同

```
/etc/inittab 设置系统默认的运行级别
/etc/init/control-alt-delete.conf
/etc/init/tty.conf
/etc/init/start-ttys.conf
/etc/init/rc.conf
/etc/init/prefdm.conf
```

###### 12.1.3.5.2 初始化脚本 sysinit

```
/etc/rc.d/rc.sysinit
```

系统初始化脚本功能

```
(1) 设置主机名
(2) 设置欢迎信息
(3) 激活udev和selinux 
(4) 挂载/etc/fstab文件中定义的文件系统
(5) 检测根文件系统，并以读写方式重新挂载根文件系统
(6) 设置系统时钟
(7) 激活swap设备
(8) 根据/etc/sysctl.conf文件设置内核参数
(9) 激活lvm及software raid设备
(10)加载额外设备的驱动程序
(11)清理操作
```

###### 12.1.3.5.3 服务管理

service 命令：手动管理服务

```
service 服务 start|stop|restart
service --status-all
```

/etc/rc.d/rc 控制服务脚本的开机自动运行

```bash
for srv in /etc/rc.d/rcN.d/K*; do
	$srv stop
done
for srv in /etc/rc.d/rcN.d/S*; do
	$srv start
done
```

说明：rc N --> 意味着读取/etc/rc.d/rcN.d/ 

K: K##：##运行次序；数字越小，越先运行；数字越小的服务，通常为依赖到别的服务 

S: S##：##运行次序；数字越小，越先运行；数字越小的服务，通常为被依赖到的服务

配置服务开机启动 

* chkconfig命令 
* ntsysv命令

chkconfig 命令管理服务

```bash
#查看服务在所有级别的启动或关闭设定情形：
chkconfig [--list] [name]

#添加服务
SysV的服务脚本放置于/etc/rc.d/init.d (/etc/init.d)
#!/bin/bash
chkconfig: LLLL nn nn  #LLLL 表示初始在哪个级别下启动，-表示都不启动
description : 描述信息
chkconfig --add name

#删除服务
chkconfig --del name

#修改指定的运行级别
chkconfig [--level levels] name <on|off|reset>
说明：--level LLLL: 指定要设置的级别；省略时表示2345
```

###### 12.1.3.5.4 非独立服务

服务分为独立服务和非独立服务 

瞬态（Transient）服务被超级守护进程 xinetd 进程所管理，也称为非独立服务 

进入的请求首先被xinetd代理

配置文件：

```
/etc/xinetd.conf
/etc/xinetd.d/<service>
```

用chkconfig控制非独立服务开机启动 

示例：

```bash
chkconfig tftp on
```

###### 12.1.3.5.5 开机启动文件 rc.local

```
/etc/rc.local
/etc/rc.d/rc.local
```

注意：正常级别下，最后启动一个服务S99local没有链接至/etc/rc.d/init.d一个服务脚本，而是指向 了/etc/rc.d/rc.local脚本 

不便或不需写为服务脚本放置于/etc/rc.d/init.d/目录，且又想开机时自动运行的命令，可直接放置 于/etc/rc.d/rc.local文件中 

/etc/rc.d/rc.local在指定运行级别脚本后运行

**注意:**  默认Ubuntu 无 /etc/rc.local 文件,可以创建此脚本文件并添加执行权限,rc.local的首行必须有 shebang机制

##### 12.1.3.6 CentOS 启动过程总结

```
POST
boot loader
vmlinux(initramfs.img)
roofs
/sbin/init
/etc/inittab 设置默认运行级别
/etc/rc.d/rc.sysinit 运行系统初始脚本完成系统初始化
/etc/rc#.d/Sxxxx 启动需要启动服务关闭对应下需要关闭的服务
/etc/rc.d/rc.local
设置登录终端

参看：http://s4.51cto.com/wyfs02/M02/87/20/wKiom1fVBELjXsvaAAUkuL83t2Q304.jpg
```

#### 12.1.4 启动过程的故障排错

##### 12.1.4.1 实战案例 

故障: 删除 /sbin/init 无法启动 

恢复过程 

方法1: 从同一个版本的另的主机复制init文件

```
光盘启动进入secure mode
ifconfig eth0 10.0.0.6/24
scp 10.0.0.16:/sbin/init /mnt/sysimages/sbin/
```

方法2:

```
1 先进入grub菜单，在kernel参数后加 selinux=0 init=/bin/bash 
2 mount -o remount,rw /
3 mount /dev/sr0 /mnt/
方法1
4 rpm2cpio /mnt/Packages/upstart.xxx.rpm | cpio -idv ./sbin/init 
5 mv ./sbin/init /sbin/ 
方法2
4 rpm -ivh /mnt/Packages/upstart.xxx.rpm --force
```

##### 12.1.4.2 实战案例

故障：rm -rf /boot/* 和 /etc/fstab 进行恢复 

恢复过程 

1. 用光盘进入 rescue mode，找到/ 所在分区并恢复/etc/fstab 

```
fdisk -l
mkdir /mnt/rootdir
mount /dev/sdaN /mnt/rootdir
ls /mnt/rootdir
mount /dev/sda2 /mnt/rootdir

vim /mnt/rootdir/etc/fstab 
/dev/sda1 /boot ext4 defaults 0 0
/dev/sda2 /   ext4 defaults 0 0
/dev/sda3 /data ext4 defaults 0 0
/dev/sda5 swap swap defaults 0 0

reboot 
```

2. rescue mode 恢复内核和initrd 文件 /dev/sda2 --> /mnt/sysimage

```bash
chroot /mnt/sysimage
mount /dev/sr0 /mnt/

#方法1 
rpm -ivh /mnt/Packages/kernel.xxxx.rpm --force  

#方法2 
cp /mnt/isolinux/vmlinuz /boot/
mkinitrd /boot/initramfs.img  `uname -r`
```

3. 修复 grub 

```
grub-install /dev/sda 
vim /boot/grub/grub.conf 方法2
[root@centos6 ~]#cat /boot/grub/grub.conf 
default=0
timeout=5
title centos
kernel /vmlinuz root=/dev/sda2
initrd /initramfs.img 
```

4. reboot

### 12.2 /proc 目录和内核参数管理

/proc目录：内核把自己内部状态信息及统计信息，以及可配置参数通过proc伪文件系统加以输出

帮助：man proc 

内核参数： 

* 只读：只用于输出信息 
* 可写：可接受用户指定“新值”来实现对内核某功能或特性的配置

/proc/sys 设置

```
sysctl是一个允许改变正在运行中的Linux系统的接口，修改的是针对整个系统的内核参数。sysctl的修改
是立即且临时的（重启后失效）。也可以通过修改sysctl.conf配置文件，达到永久生效。
```

* sysctl 命令用于查看或设定此目录中诸多参数

```
sysctl -w path.to.parameter=VALUE
```

* 默认配置文件：/etc/sysctl.conf 及以下文件

```
/run/sysctl.d/*.conf
/etc/sysctl.d/*.conf
/usr/local/lib/sysctl.d/*.conf
/usr/lib/sysctl.d/*.conf
/lib/sysctl.d/*.conf
/etc/sysctl.conf
```

* echo命令通过重定向方式也可以修改大多数参数的值

```
echo "VALUE" > /proc/sys/path/to/parameter
```

sysctl命令：

 (1) 临时设置某参数

```
sysctl -w parameter=VALUE
```

(2) 通过读取配置文件设置参数

```
sysctl -p [/path/to/conf_file]
```

(3) 查看指定参数当前值

```
sysctl [/path/to/conf_file]
```

(4) 查看所有生效参数

```
sysctl -a
```

常用的内核参数：

```
net.ipv4.ip_forward
net.ipv4.icmp_echo_ignore_all
net.ipv4.ip_nonlocal_bind   #允许应用程序可以监听本地不存在的IP
vm.drop_caches
fs.file-max = 1020000           #全局打开文件的最大数
vm.overcommit_memory = 0  
#0表示内核将检查是否有足够可用内存供应用进程使用；如果有足够的可用内存，内存申请允许；否则内存申
请失败，并把错误返回给应用进程。
#1表示内核允许分配所有的物理内存，而不管当前的内存状态如何。
#2表示内核允许分配超过所有物理内存和交换空间总和的内存。

vm.swappiness = 10

#禁用IPv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
```

### 12.3 /sys 目录

/sys目录： 

使用sysfs文件系统，为用户使用的伪文件系统，输出内核识别出的各硬件设备的相关属性信息，也有内 核对硬件特性的设定信息；有些参数是可以修改的，用于调整硬件工作特性 

udev通过此路径下输出的信息动态为各设备创建所需要设备文件，udev是运行用户空间程序 专用工具：udevadmin, hotplug 

udev为设备创建设备文件时，会读取其事先定义好的规则文件，一般在/etc/udev/rules.d 及/usr/lib/udev/rules.d目录下

### 12.4 内核模块管理和编译

单内核体系设计、但充分借鉴了微内核设计体系的优点，为内核引入模块化机制 

内核组成部分： 

* kernel：内核核心，一般为bzImage，通常在/boot目录

```
vmlinuz-VERSION-RELEASE
```

* kernel object：内核对象，一般放置于

```
/lib/modules/VERSION-RELEASE/
```

* 辅助文件：ramdisk

```
initrd-VERSION-RELEASE.img：从CentOS 5 版本以前
initramfs-VERSION-RELEASE.img：从CentOS6 版本以后
```

#### 12.4.1 内核版本

运行中的内核： 

uname命令：print system information

```
uname [OPTION]...
```

常用选项：

```
-n 显示节点名称
-r 显示VERSION-RELEASE
-a 显示所有信息
```

#### 12.4.2 内核模块命令

lsmod 命令： 

* 显示由核心已经装载的内核模块 
* 显示的内容来自于: /proc/modules文件

```bash
[root@node-1 ~]# lsmod
Module                  Size  Used by
mpt3sas               222613  0
raid_class             13554  1 mpt3sas
xt_nat                 12681  1
rpcsec_gss_krb5        35549  0
auth_rpcgss            59415  1 rpcsec_gss_krb5
nfsv4                 574651  2
dns_resolver           13140  1 nfsv4
nfs                   261909  2 nfsv4
lockd                  93827  1 nfs
grace                  13515  1 lockd
sunrpc                348674  9 nfs,rpcsec_gss_krb5,auth_rpcgss,lockd,nfsv4
fscache                64935  2 nfs,nfsv4
binfmt_misc            17468  1
tcp_diag               12591  0
inet_diag              18949  1 tcp_diag
.....

#显示：名称、大小，使用次数，被哪些模块依赖
```

modinfo命令： 

功能：管理内核模块 

配置文件：/etc/modprobe.conf, /etc/modprobe.d/*.conf

* 显示模块的详细描述信息

```
modinfo [ -k kernel ] [ modulename|filename... ]
```

常用选项：

```
-n：只显示模块文件路径
-p：显示模块参数
-a：作者
-d：描述
```

* 装载或卸载内核模块

```
modprobe [ -C config-file ] [ modulename ] [ module parame-ters... ]
modprobe [ -r ] modulename…
```

depmod命令：内核模块依赖关系文件及系统信息映射文件的生成工具 

insmod命令：可以安装模块，需要指定模块文件路径，并且不自动解决依赖模块

```
insmod [ filename ] [ module options... ]
```

rmmod命令：卸载模块

```
rmmod [ modulename ]
```

#### 12.4.3 编译内核

编译安装内核准备： 

(1) 准备好开发环境 

(2) 获取目标主机上硬件设备的相关信息 

(3) 获取目标主机系统功能的相关信息，例如:需要启用相应的文件系统 

(4) 获取内核源代码包，www.kernel.org

##### 12.4.3.1 编译准备 

###### 12.4.3.1.1 目标主机硬件设备相关信息

CPU:

```
cat /proc/cpuinfo
x86info -a
lscpu
```

PCI设备：lspci -v ，-vv

USB设备：lsusb -v，-vv

lsblk 块设备 

全部硬件设备信息：hal-device：CentOS 6

###### 12.4.3.1.2 开发环境相关包

gcc make ncurses-devel flex bison openssl-devel elfutils-libelf-devel

###### 12.4.3.1.3 内核编译安装实现

* 下载源码文件 
* 准备文本配置文件/boot/config-`uname -r` 
* make menuconfig：配置内核选项 ，相当于./configure 

```
[ ]: N
[M]: M
[*]: Y
```

* make [-j #] 或者用以下两步实现： 

  make -j # bzImage  

  make -j # modules 

* 安装模块：make modules_install 

* 安装内核相关文件：make install  

  安装bzImage为 /boot/vmlinuz-VERSION-RELEASE 

  生成initramfs文件 

  编辑grub的配置文件

###### 12.4.3.1.4 内核编译说明

**配置内核选项**

支持“更新”模式进行配置：make help

```
(a) make config：基于命令行以遍历的方式配置内核中可配置的每个选项
(b) make menuconfig：基于curses的文本窗口界面
(c) make gconfig：基于GTK (GNOME）环境窗口界面
(d) make xconfig：基于QT(KDE)环境的窗口界面
```

支持“全新配置”模式进行配置

```
(a) make defconfig：基于内核为目标平台提供的“默认”配置进行配置
(b) make allyesconfig: 所有选项均回答为“yes“
(c) make allnoconfig: 所有选项均回答为“no“
```

**编译内核**

* 全编译

```
make [-j #] 
```

* 编译内核的一部分功能：

  (a) 只编译某子目录中的相关代码

  ```
  cd /usr/src/linux
   make dir/
  ```

  (b) 只编译一个特定的模块

  ```
  cd /usr/src/linux
   make dir/
  ```

**交叉编译内核**

编译的目标平台与当前平台不相同

```
make ARCH=arch_name
```

要获取特定目标平台的使用帮助

```
make ARCH=arch_name help
```

**重新编译需要事先清理操作**

```
make clean：清理大多数编译生成的文件，但会保留.config文件等
make mrproper: 清理所有编译生成的文件、config及某些备份文件
make distclean：包含 make mrproper,并清理patches以及编辑器备份文件
```

###### 12.4.3.1.5 卸载内核

* 删除/usr/src/linux/目录下不需要的内核源码 
* 删除/lib/modules/目录下不需要的内核库文件 
* 删除/boot目录下启动的内核和内核映像文件 
* 更改grub的配置文件，删除不需要的内核启动列表 grub2-mkconfig -o /boot/grub2/grub.cfg 
* CentOS 8 还需要删除 /boot/loader/entries/5b85fc7444b240a992c42ce2a9f65db5-新内核版 本.conf

### 12.5 systemd

#### 12.5.1 systemd 特性

Systemd：从 CentOS 7 版本之后开始用 systemd 实现init进程，系统启动和服务器守护进程管理器， 负责在系统启动或运行时，激活系统资源，服务器进程和其它进程

**Systemd新特性** 

* 系统引导时实现服务并行启动 
* 按需启动守护进程 
* 自动化的服务依赖关系管理 
* 同时采用socket式与D-Bus总线式激活服务 
* socket与服务程序分离 
* 向后兼容sysv init脚本 
* 使用systemctl 命令管理，systemctl命令固定不变，不可扩展，非由systemd启动的服务， systemctl无法与之通信和控制 
* 系统状态快照

systemd 核心概念：unit 

unit表示不同类型的systemd对象，通过配置文件进行标识和配置；文件中主要包含了系统服务、监听 socket、保存的系统快照以及其它与init相关的信息

Unit类型：

```
#查看unit类型
[root@centos8 ~]#systemctl -t help 
Available unit types:
service
socket
target
device
mount
automount
swap
timer
path
slice
scope
```

* service unit: 文件扩展名为.service, 用于定义系统服务 
* Socket unit: .socket, 定义进程间通信用的socket文件，也可在系统启动时，延迟启动服务，实现 按需启动 
* Target unit: 文件扩展名为.target，用于模拟实现运行级别 
* Device unit: .device, 用于定义内核识别的设备 
* Mount unit: .mount, 定义文件系统挂载点 
* Snapshot unit: .snapshot, 管理系统快照 
* Swap unit: .swap, 用于标识swap设备 
* Automount unit: .automount，文件系统的自动挂载点 
* Path unit: .path，用于定义文件系统中的一个文件或目录使用,常用于当文件系统变化时，延迟激 活服务，如：spool 目录

unit的配置文件

```
/usr/lib/systemd/system #每个服务最主要的启动脚本设置，类似于之前的/etc/init.d/
/lib/systemd/system #ubutun的对应目录

/run/systemd/system #系统执行过程中所产生的服务脚本，比上面目录优先运行
/etc/systemd/system #管理员建立的执行脚本，类似于/etc/rcN.d/Sxx的功能，比上面目录优先运行
```

#### 12.5.2 systemctl管理系统服务service unit

命令：

```
systemctl COMMAND name.service
```

```bash
#启动：相当于service name start 
systemctl start name.service   
#停止：相当于service name stop
systemctl stop name.service
#重启：相当于service name restart 
systemctl restart name.service 
#查看状态：相当于service name status
systemctl status name.service
#禁止自动和手动启动：
systemctl mask name.service
#取消禁止
systemctl unmask name.service
#查看某服务当前激活与否的状态：
systemctl is-active name.service
#查看所有已经激活的服务：
systemctl list-units --type|-t service
#查看所有服务：
systemctl list-units --type service --all|-a
#设定某服务开机自启，相当于chkconfig name on 
systemctl enable name.service 
#设定某服务开机禁止启动：相当于chkconfig name off
systemctl disable name.service
#查看所有服务的开机自启状态，相当于chkconfig --list
systemctl list-unit-files --type service
#用来列出该服务在哪些运行级别下启用和禁用：chkconfig –list name
ls /etc/systemd/system/*.wants/name.service
#查看服务是否开机自启：
systemctl is-enabled name.service
#列出失败的服务
systemctl --failed --type=service
 
#开机并立即启动或停止
systemctl enable --now postfix 
systemctl disable  --now postfix
#查看服务的依赖关系：
systemctl list-dependencies name.service
#杀掉进程：
systemctl kill unitname
```

服务状态

```bash
#显示状态
systemctl list-unit-files --type service --all
```

* oaded Unit配置文件已处理 
* active(running) 一次或多次持续处理的运行 
* active(exited) 成功完成一次性的配置 
* active(waiting) 运行中，等待一个事件 
* inactive 不运行 
* enabled 开机启动 
* disabled 开机不启动 
* static 开机不启动，但可被另一个启用的服务激活 
* indirect 重定向到别处

#### 12.5.3 service unit文件格式

/etc/systemd/system：系统管理员和用户使用 

/usr/lib/systemd/system：发行版打包者使用

帮助参考： 

systemd.directives（7），systemd.unit(5),systemd.service(5), systemd.socket(5),  systemd.target(5),systemd.exec(5)

参考链接：

```
http://www.jinbuguo.com/systemd/systemd.service.html
```

unit 格式说明： 

* 以 “#” 开头的行后面的内容会被认为是注释 
* 相关布尔值，1、yes、on、true 都是开启，0、no、off、false 都是关闭 
* 时间单位默认是秒，所以要用毫秒（ms）分钟（m）等须显式说明 

service unit file文件通常由三部分组成： 

* [Unit]：定义与Unit类型无关的通用选项；用于提供unit的描述信息、unit行为及依赖关系等 
* [Service]：与特定类型相关的专用选项；此处为Service类型 
* [Install]：定义由“systemctl enable”以及"systemctl disable“命令在实现服务启用或禁用时用到 的一些选项

Unit段的常用选项： 

* Description：描述信息 
* After：定义unit的启动次序，表示当前unit应该晚于哪些unit启动，其功能与Before相反 
* Requires：依赖到的其它units，强依赖，被依赖的units无法激活时，当前unit也无法激活 
* Wants：依赖到的其它units，弱依赖 
* Conflicts：定义units间的冲突关系

Service段的常用选项： 

* Type：定义影响ExecStart及相关参数的功能的unit进程启动类型 

  ​	simple：默认值，这个daemon主要由ExecStart接的指令串来启动，启动后常驻于内存中 

  ​	forking：由ExecStart启动的程序透过spawns延伸出其他子程序来作为此daemon的主要服 务。原生父程序在启动结束后就会终止 

  ​	oneshot：与simple类似，不过这个程序在工作完毕后就结束了，不会常驻在内存中 

  ​	dbus：与simple类似，但这个daemon必须要在取得一个D-Bus的名称后，才会继续运作.因 此通常也要同时设定BusNname= 才行 

  ​	notify：在启动完成后会发送一个通知消息。还需要配合 NotifyAccess 来让 Systemd 接收消 息 

  ​	idle：与simple类似，要执行这个daemon必须要所有的工作都顺利执行完毕后才会执行。这 类的daemon通常是开机到最后才执行即可的服务 

* EnvironmentFile：环境配置文件 
* ExecStart：指明启动unit要运行命令或脚本的绝对路径 
* ExecStartPre： ExecStart前运行 
* ExecStartPost： ExecStart后运行 
* ExecStop：指明停止unit要运行的命令或脚本 
* Restart：当设定Restart=1 时，则当次daemon服务意外终止后，会再次自动启动此服务 
* RestartSec: 设置在重启服务( Restart= )前暂停多长时间。 默认值是100毫秒(100ms)。 如果未指 定时间单位，那么将视为以秒为单位。 例如设为"20"等价于设为"20s"。 
* PrivateTmp：设定为yes时，会在生成/tmp/systemd-private-UUID-NAME.service-XXXXX/tmp/ 目录

Install段的常用选项： 

* Alias：别名，可使用systemctl command Alias.service 
* RequiredBy：被哪些units所依赖，强依赖 
* WantedBy：被哪些units所依赖，弱依赖 
* Also：安装本服务的时候还要安装别的相关服务

**注意**：对于新创建的unit文件，或者修改了的unit文件，要通知systemd重载此配置文件,而后可以选择 重启

```
systemctl daemon-reload
```

#### 12.5.4 运行级别

target units：相当于CentOS 6之前的runlevel ,unit配置文件：.target

```bash
ls /usr/lib/systemd/system/*.target
systemctl list-unit-files --type target  --all
```

和运行级别对应关系

```
0  ==> runlevel0.target, poweroff.target
1  ==> runlevel1.target, rescue.target
2  ==> runlevel2.target, multi-user.target
3  ==> runlevel3.target, multi-user.target
4  ==> runlevel4.target, multi-user.target
5  ==> runlevel5.target, graphical.target
6  ==> runlevel6.target, reboot.target
```

查看依赖性：

```bash
systemctl list-dependencies graphical.target
```

级别切换：相当于 init N 

```bash
systemctl isolate name.target
```

进入默认target

```bash
systemctl default 
```

```bash
#切换至字符模式
systemctl isolate multi-user.target
```

注意：只有/lib/systemd/system/*.target文件中AllowIsolate=yes 才能切换(修改文件需执行systemctl  daemon-reload才能生效)

获取默认运行级别： 相当于查看 /etc/inittab

```
systemctl get-default
```

修改默认级别：相当于修改 /etc/inittab 

```
systemctl set-default name.target
```

切换至紧急救援模式：

```
systemctl rescue
```

切换至emergency模式：

```
systemctl emergency
```

说明：rescue.target 比emergency 支持更多的功能，例如日志等 

传统命令init，poweroff，halt，reboot都成为systemctl的软链接

```
#关机
systemctl halt、systemctl poweroff
 
#重启：
systemctl reboot
#挂起：
systemctl suspend
#休眠：
systemctl hibernate
#休眠并挂起：
systemctl hybrid-sleep
```

#### 12.5.5 设置内核参数

设置内核参数，只影响当次启动 

启动时，到启动菜单，按e键，找到在linux 开头的行后添加systemd.unit=desired.target

```
systemd.unit=emergency.target 
systemd.unit=rescue.target
```

#### 12.5.6 CentOS 7之后版本引导顺序

1. UEFi或BIOS初始化，运行POST开机自检 
2. 选择启动设备 
3. 引导装载程序, centos7是grub2,加载装载程序的配置文件： /etc/grub.d/ /etc/default/grub  /boot/grub2/grub.cfg 
4. 加载initramfs驱动模块 
5. 加载内核选项 
6. 内核初始化，centos7使用systemd代替init  
7. 执行initrd.target所有单元，包括挂载/etc/fstab 
8. 从initramfs根文件系统切换到磁盘根目录 
9. systemd执行默认target配置，配置文件/etc/systemd/system/default.target 
10. systemd执行sysinit.target初始化系统及basic.target准备操作系统 
11. systemd启动multi-user.target下的本机与服务器服务 
12. systemd执行multi-user.target下的/etc/rc.d/rc.local
13. Systemd执行multi-user.target下的getty.target及登录服务 
14. systemd执行graphical需要的服务

通过systemd-analyze 工具可以了解启动的详细过程

#### 12.5.7 破解 CentOS 7和8的 root 密码

方法一：

```bash
启动时任意键暂停启动
按e键进入编辑模式
将光标移动linux 开始的行，添加内核参数 rd.break
按ctrl-x启动
mount –o remount,rw /sysroot
chroot /sysroot
passwd root

#如果SELinux是启用的,才需要执行下面操作,如查没有启动,不需要执行
touch /.autorelabel

exit
reboot
```

方法二：

```bash
启动时任意键暂停启动
按e键进入编辑模式
将光标移动linux 开始的行，改为 rw init=/sysroot/bin/sh
按ctrl-x启动
chroot /sysroot
passwd root

#如果SELinux是启用的,才需要执行下面操作,如查没有启动,不需要执行
touch /.autorelabel

exit
reboot
```

#### 12.5.8 实现 GRUB2 安全

#### 12.5.9 修复 GRUB2

GRUB2：CentOS 7，8及ubuntu1804都使用 

引导提示时可以使用命令行界面，可从文件系统引导 

主要配置文件：/boot/grub2/grub.cfg 

修复配置文件：grub2-mkconfig > /boot/grub2/grub.cfg 

修复grub

```
grub2-install /dev/sda #BIOS环境
grub2-install #UEFI环境
```

#### 12.5.10 故障排错实战案例

##### 12.5.10.1 实战案例1：CentOS 7,8 破坏MBR后进行恢复

```bash
dd if=/dev/zero of=/dev/sda bs=1 count=446
光盘进入救援模式
grub2-install --root-directory=/mnt/sysimage /dev/sda
```

##### 12.5.10.2 实战案例2：CentOS 7,8 删除 /boot/grub2/ 所有内容进行 恢复

```bash
光盘进入救援模式
chroot /mnt/sysimage
grub2-install /dev/sda
grub2-mkconfig -o /boot/grub2/grub.cfg
```

##### 12.5.10.3 实战案例3：CentOS 7,8 删除 /boot/ 下所有文件后进行恢复

```bash
1光盘救援模式下安装grub2
特别说明：Centos8 必须先修复grub，再安装kernel,否则安装kernel-core时会提示grub出错
#centos8
chroot /mnt/sysroot
chroot /mnt/sysimage
#centos7
chroot /mnt/sysimage
grub2-install /dev/sda

2安装Kernel
#CentOS 7 
mount /dev/sr0 /mnt
rpm –ivh /mnt/Packages/kernel-3.10.0-1062.el7.x86_64.rpm --force
#CentOS 8 
mount /dev/sr0 /mnt
rpm -ivh /mnt/BaseOS/Packages/kernel-core-4.18.0-147.el8.x86_64.rpm --force

3修复grub配置文件
生成grub2.cfg文件
grub2-mkconfig –o /boot/grub2/grub.cfg

4 退出重启
sync
sync
exit
exit
```

# 13 加密和安全

### 内容概述

* 安全机制 
* 对称和非对称加密 
* 散列算法 
* PKI和CA 
* openssl 
* 证书管理

* ssh服务 
* 轻量级自动化运维工具 
* 权限授权管理Sudo 
* PAM模块 
* 时间同步服务 
* SELinux安全加强

### 13.1 安全机制

#### 13.1.1 墨菲定律

墨菲定律：一种心理学效应，是由爱德华·墨菲（Edward A. Murphy）提出的，

原话：如果有两种或两 种以上的方式去做某件事情，而其中一种选择方式将导致灾难，则必定有人会做出这种选择 

**主要内容：** 

* 任何事都没有表面看起来那么简单 
* 所有的事都会比你预计的时间长 
* 会出错的事总会出错 
* 如果你担心某种情况发生，那么它就更有可能发生

#### 13.1.2 信息安全防护的目标

* 保密性 Confidentiality 
* 完整性 Integrity 
* 可用性 Usability 
* 可控制性 Controlability 
* 不可否认性 Non-repudiation

#### 13.1.3 安全防护环节

* 物理安全：各种设备/主机、机房环境 
* 系统安全：主机或设备的操作系统 
* 应用安全：各种网络服务、应用程序 
* 网络安全：对网络访问的控制、防火墙规则 
* 数据安全：信息的备份与恢复、加密解密 
* 管理安全：各种保障性的规范、流程、方法

#### 13.1.4 常见的安全攻击 STRIDE

* Spoofing 假冒 
* Tampering 篡改 
* Repudiation 否认 
* Information Disclosure 信息泄漏 
* Denial of Service 拒绝服务 
* Elevation of Privilege 提升权限

#### 13.1.5 安全设计基本原则

* 使用成熟的安全系统 
* 以小人之心度输入数据 
* 外部系统是不安全的 
* 最小授权 
* 减少外部接口 
* 缺省使用安全模式 
* 安全不是似是而非 
* 从STRIDE思考 
* 在入口处检查 
* 从管理上保护好你的系统

#### 13.1.6 常用安全技术

* 认证 
* 授权 
* 审计 
* 安全通信

#### 13.1.7 加密算法和协议

* 对称加密 
* 非对称（公钥）加密 
* 单向加密 
* 认证协议

##### 13.1.7.1 对称加密算法

对称加密：加密和解密使用同一个密钥 

特性： 

* 加密、解密使用同一个密钥，效率高 
* 将原始数据分割成固定大小的块，逐个进行加密

缺陷： 

* 密钥过多 
* 密钥分发 
* 数据来源无法确认

常见对称加密算法: 

* DES：Data Encryption Standard，56bits 
* 3DES： 
* AES：Advanced (128, 192, 256bits) 
* Blowfish，Twofish 
* IDEA，RC6，CAST5

##### 13.1.7.2 非对称加密算法 

###### 13.1.7.2.1 非对称加密算法介绍

非对称加密：密钥是成对出现 

* 公钥：public key，公开给所有人，主要给别人加密使用 
* 私钥：secret key，private key 自己留存，必须保证其私密性，用于自已加密签名 
* 特点：用公钥加密数据，只能使用与之配对的私钥解密；反之亦然 

功能： 

* 数据加密：适合加密较小数据,比如: 加密对称密钥
* 数字签名：主要在于让接收方确认发送方身份 

缺点： 

* 密钥长,算法复杂 
* 加密解密效率低下 

常见算法： 

* RSA：由 RSA 公司发明，是一个支持变长密钥的公共密钥算法，需要加密的文件块的长度也是可变 的,可实现加密和数字签名 
* DSA（Digital Signature Algorithm）：数字签名算法，是一种标准的 DSS（数字签名标准） 
* ECC（Elliptic Curves Cryptography）：椭圆曲线密码编码学，比RSA加密算法使用更小的密钥， 提供相当的或更高等级的安全

###### 13.1.7.2.2 非对称加密实现加密

接收者 

* 生成公钥/密钥对：P和S 
* 公开公钥P，保密密钥S 

发送者 

* 使用接收者的公钥来加密消息M 
* 将P(M)发送给接收者 

接收者 

* 使用密钥S来解密：M=S(P(M))

###### 13.1.7.2.3 非对称加密实现数字签

发送者 

* 生成公钥/密钥对：P和S 
* 公开公钥P，保密密钥S 
* 使用密钥S来加密消息M 
* 发送给接收者S(M) 

接收者 

* 使用发送者的公钥来解密M=P(S(M))

###### 13.1.7.2.4 RSA和DSA 

RSA：公钥加密算法是1977年由Ron Rivest、Adi Shamirh和LenAdleman在（美国麻省理工学院）开发 的，RSA取名来自开发他们三者的名字，后成立RSA数据安全有限公司。RSA是目前最有影响力的公钥加 密算法，它能够抵抗到目前为止已知的所有密码攻击，已被ISO推荐为公钥数据加密标准。RSA算法基于 一个十分简单的数论事实：将两个大素数相乘十分容易，但那时想要对其乘积进行因式分解却极其困 难，因此可以将乘积公开作为加密密钥 

DSA (Digital Signature Algorithm)：1991年7月26日提交，并归属于David W. Kravitz前NSA员工， DSA是Schnorr和ElGamal签名算法的变种，被美国NIST作为SS(DigitalSignature Standard)， DSA是基 于整数有限域离散对数难题的，其安全性与RSA相比差不多。DSA只是一种算法，和RSA不同之处在于它 不能用作加密和解密，也不能进行密钥交换，只用于签名,它比RSA要快很多

##### 13.1.7.3 单向哈希算法

哈希算法：也称为散列算法，将任意数据缩小成固定大小的“指纹”，称为digest，即摘要 

特性： 

* 任意长度输入，固定长度输出 
* 若修改数据，指纹也会改变，且有雪崩效应，数据的一点微小改变，生成的指纹值变化非常大。 
* 无法从指纹中重新生成数据，即不要逆，具有单向性 

功能：数据完整性 

常见算法 

md5: 128bits、sha1: 160bits、sha224 、sha256、sha384、sha512 

常用工具 

* md5sum | sha1sum [ --check ] file 
* openssl、gpg 
* rpm -V

**数字签名**

RPM 文件完整性 

rpm --verify package_name (or -V) 

rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat* 

rpm --checksig pakage_file_name (or -K)

##### 13.1.7.4 综合应用多种加密算法

###### 13.1.7.4.1 实现数据加密

实现数据加密，无法验证数据完整性和来源

###### 13.1.7.4.2 实现数字签名

不加密数据，可以保证数据来源的可靠性、数据的完整性和一致性

###### 13.1.7.4.3 综合加密和签名

即实现数据加密，又可以保证数据来源的可靠性、数据的完整性和一致性

**方法1：Pb{Sa[hash(data)]+data}**

**方法2：对称key{Sa[hash(data)]+data}+Pb(对称key)**

##### 13.1.7.5 密码交换

密钥交换：IKE（ Internet Key Exchange ） 

* 公钥加密：用目标的公钥加密对称密钥 
* DH (Deffie-Hellman)：生成对称（会话）密钥 

​		参看：https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange

DH 介绍 

* 这个密钥交换方法，由惠特菲尔德·迪菲（Bailey Whitfield Diffie）和马丁·赫尔曼（Martin  Edward Hellman）在1976年发表 
* 它是一种安全协议，让双方在完全没有对方任何预先信息的条件下通过不安全信道建立起一个密 钥，这个密钥一般作为“对称加密”的密钥而被双方在后续数据传输中使用。 
* DH数学原理是base离散对数问题。做类似事情的还有非对称加密类算法，如：RSA。 
* 其应用非常广泛，在SSH、VPN、Https...都有应用，勘称现代密码基石

DH实现过程：

```
A: g,p 协商生成公开的整数g, 大素数p
B: g,p
A:生成隐私数据:a (a<p)，计算得出 g^a%p，发送给B
B:生成隐私数据:b,(b<p)，计算得出 g^b%p，发送给A
A:计算得出 [(g^b%p)^a]%p = g^ab%p，生成为密钥
B:计算得出 [(g^a%p)^b]%p = g^ab%p，生成为密钥
```

DH特点

泄密风险：私密数据a，b在生成K后将被丢弃，因此不存在a，b过长时间存在导致增加泄密风险。 中间人攻击：由于DH在传输p，g时并无身份验证，所以有机会被实施中间人攻击，替换双方传输时的 数据

#### 13.1.8 CA和证书

##### 13.1.8.1 中间人攻击

Man-in-the-middle，简称为 MITM，中间人

```
https://www.zhihu.com/question/304363663
```

##### 13.1.8.2 CA和证书

PKI：Public Key Infrastructure 公共密钥加密体系 

签证机构：CA（Certificate Authority） 

注册机构：RA 

证书吊销列表：CRL 

证书存取库： 

X.509：定义了证书的结构以及认证协议标准

* 版本号 
* 序列号 
* 签名算法 
* 颁发者 
* 有效期限 
* 主体名称

证书类型： 

* 证书授权机构的证书

* 服务器证书 
* 用户证书

获取证书两种方法： 

* 自签名的证书： 自已签发自己的公钥 

* 使用证书授权机构： 

  ​	生成证书请求（csr） 

  ​	将证书请求csr发送给CA 

  ​	CA签名颁发证书

##### 13.1.8.3 安全协议 SSL/TLS

###### 13.1.8.3.1 TLS 介绍

SSL：Secure Socket Layer，TLS: Transport Layer Security 

1994年，NetScape公司设计了SSL协议（Secure Sockets Layer）的1.0版，但是未发布 

1995：SSL 2.0 Netscape 开发 

1996：SSL 3.0 

1999：TLS 1.0  

2006：TLS 1.1 IETF(Internet工程任务组) RFC 4346，从2020年3月起，停止支持TLS 1.1及TLS 1.0版本 安全协议，谷歌（Chrome）、Mozilla（Firefox）、微软（IE和Edge） 、苹果（Safari） 都会发布新版 浏览器执行这个策略

2008：TLS 1.2 当前主要使用 

2018：TLS 1.3 

功能：

* 机密性 
* 认证 
* 完整性 
* 重放保护

###### 13.1.8.3.2 SSL/TLS组成

* Handshake协议：包括协商安全参数和密码套件、服务器身份认证（客户端身份认证可选）、密钥 交换 
* ChangeCipherSpec 协议：一条消息表明握手协议已经完成 
* Alert 协议：对握手协议中一些异常的错误提醒，分为fatal和warning两个级别，fatal类型错误会 直接中断SSL链接，而warning级别的错误SSL链接仍可继续，只是会给出错误警告 
* Record 协议：包括对消息的分段、压缩、消息认证和完整性保护、加密等

##### 13.1.8.4 HTTPS

HTTPS 协议：就是“HTTP 协议”和“SSL/TLS 协议”的组合。HTTP over SSL 或 HTTP over TLS ，对http协 议的文本数据进行加密处理后，成为二进制形式传输

###### 13.1.8.4.1 HTTPS 结构

###### 13.1.8.4.2 HTTPS 工作的简化过程

1. 客户端发起HTTPS请求 

   用户在浏览器里输入一个https网址，然后连接到服务器的443端口 

2. 服务端的配置 

   采用HTTPS协议的服务器必须要有一套数字证书，可以自己制作，也可以向组织申请。区别就是自 己颁发的证书需要客户端验证通过，才可以继续访问，而使用受信任的公司申请的证书则不会弹出 提示页面。这套证书其实就是一对公钥和私钥 

3. 传送服务器的证书给客户端 

   证书里其实就是公钥，并且还包含了很多信息，如证书的颁发机构，过期时间等等 

4. 客户端解析验证服务器证书 

   这部分工作是由客户端的TLS来完成的，首先会验证公钥是否有效，比如：颁发机构，过期时间等 等，如果发现异常，则会弹出一个警告框，提示证书存在问题。如果证书没有问题，那么就生成一 个随机值。然后用证书中公钥对该随机值进行非对称加密 

5. 客户端将加密信息传送服务器 

   这部分传送的是用证书加密后的随机值，目的就是让服务端得到这个随机值，以后客户端和服务端 的通信就可以通过这个随机值来进行加密解密了 

6. 服务端解密信息 

   服务端将客户端发送过来的加密信息用服务器私钥解密后，得到了客户端传过来的随机值 

7. 服务器加密信息并发送信息 

   服务器将数据利用随机值进行对称加密,再发送给客户端 

8. 客户端接收并解密信息 

   客户端用之前生成的随机值解密服务段传过来的数据，于是获取了解密后的内容

### 13.2 OpenSSL 

#### 13.2.1 OpenSSL 介绍

官网：https://www.openssl.org/

OpenSSL计划在1998年开始，其目标是发明一套自由的加密工具，在互联网上使用。OpenSSL以Eric  Young以及Tim Hudson两人开发的SSLeay为基础，随着两人前往RSA公司任职，SSLeay在1998年12月 停止开发。因此在1998年12月，社群另外分支出OpenSSL，继续开发下去 

OpenSSL管理委员会当前由7人组成,有13个开发人员具有提交权限（其中许多人也是OpenSSL管理委员 会的一部分）。只有两名全职员工（研究员），其余的是志愿者 

该项目每年的预算不到100万美元，主要依靠捐款。 TLS 1.3 的开发由 Akamai 赞助 

OpenSSL是一个开放源代码的软件库包，应用程序可以使用这个包来进行安全通信，避免窃听，同时确 认另一端连线者的身份。这个包广泛被应用在互联网的网页服务器上 

其主要库是以C语言所写成，实现了基本的加密功能，实现了SSL与TLS协议。OpenSSL可以运行在 OpenVMS、 Microsoft Windows以及绝大多数类Unix操作系统上（包括Solaris，Linux，Mac OS X与 各种版本的开放源代码BSD操作系统） 

心脏出血漏洞：OpenSSL 1.0.1版本（不含1.0.1g）含有一个严重漏洞，可允许攻击者读取服务器的内存 信息。该漏洞于2014年4月被公诸于世，影响三分之二的活跃网站 

包括三个组件： 

* libcrypto：用于实现加密和解密的库 
* libssl：用于实现ssl通信协议的安全库 
* openssl：多用途命令行工具

#### 13.2.2 Base64 编码 

Base64是网络上最常见的用于传输 8Bit 字节码的编码方式之一，Base64就是一种基于64个可打印字符 来表示二进制数据的方法

**base64的编码过程如下：**

将每3个字节放入一个24位的缓冲区中，最后不足3个字节的，缓冲区的剩余部分用0来填补。然后每次 取出6位（2的6次方为64，使用64个字符即可表示所有），将高2位用0来填充，组成一个新的字节，计 算出这个新字节的十进制值，对应上面的编码表，输出相应的字符。这样不断地进行下去，就可完成对 所有数据的编码工作。

#### 13.2.3 openssl 命令 

两种运行模式： 

* 交互模式 
* 批处理模式 

三种子命令： 

* 标准命令 

* 消息摘要命令 

* 加密命令

##### 13.2.3.1 openssl命令对称加密 

工具：openssl enc, gpg 

算法：3des, aes, blowfish, twofish 

enc命令：帮助：man enc

加密：

```bash
openssl enc -e -des3 -a -salt -in testfile -out testfile.cipher
```

解密：

```bash
openssl enc -d -des3 -a -salt -in testfile.cipher -out testfile
```

##### 13.2.3.2 openssl命令单向哈希加密

工具：openssl dgst 

算法：md5sum, sha1sum, sha224sum,sha256sum… 

dgst 命令：帮助：man dgst

```bash
openssl dgst -md5 [-hex默认] /PATH/SOMEFILE
openssl dgst -md5 testfile
md5sum /PATH/TO/SOMEFILE
```

补充知识：

```
MAC: Message Authentication Code，单向加密的一种延伸应用，用于实现网络通信中保证所传输数据
的完整性机制
HMAC：hash-based MAC，使用哈希算法
```

##### 13.2.3.3 openssl 命令生成用户密码

passwd命令:帮助：man sslpasswd

##### 13.2.3.4 openssl命令生成随机数 

随机数生成器：伪随机数字，利用键盘和鼠标，块设备中断生成随机数

```
/dev/random #仅从熵池返回随机数；随机数用尽，阻塞
/dev/urandom #从熵池返回随机数；随机数用尽，会利用软件生成伪随机数，非阻塞
```

帮助：man sslrand

```bash
openssl rand -base64|-hex NUM
NUM: 表示字节数，使用-hex，每个字符为十六进制，相当于4位二进制，出现的字符数为NUM*2
```

13.2.3.5 openssl命令实现 PKI 

公钥加密： 

​	算法：RSA, ELGamal 

​	工具：gpg, openssl rsautl（man rsautl） 

数字签名： 

​	算法：RSA, DSA, ELGamal 

密钥交换： 

​	算法：dh 

​	DSA：Digital Signature Algorithm 

​	DSS：Digital Signature Standard 

​	RSA： 

openssl命令生成密钥对儿：man genrsa

**生成私钥**

```bash
openssl genrsa -out /PATH/TO/PRIVATEKEY.FILE [-aes128] [-aes192] [-aes256] [-
des3] [NUM_BITS,默认2048]

@对称加密算法:man genrsa
-aes128, -aes192, -aes256, -aria128, -aria192, -aria256, -camellia128, -
camellia192, -camellia256, -des, -des3, -idea
```

解密加密的私钥

```bash
openssl rsa -in /PATH/TO/PRIVATEKEY.FILE -out /PATH/TO/PRIVATEKEY2.FILE
```

从私钥中提取出公钥

```bash
openssl rsa -in PRIVATEKEYFILE -pubout -out PUBLICKEYFILE
```

#### 13.2.4 建立私有CA实现证书申请颁发

建立私有CA： 

* OpenCA：OpenCA开源组织使用Perl对OpenSSL进行二次开发而成的一套完善的PKI免费软件 
* openssl：相关包 openssl和openssl-libs  

证书申请及签署步骤： 

​	1、生成证书申请请求 

​	2、RA核验 

​	3、CA签署 

​	4、获取证书

openssl的配置文件：

```
/etc/pki/tls/openssl.cnf
```

三种策略：match匹配、optional可选、supplied提供

```
match：要求申请填写的信息跟CA设置信息必须一致
optional：可有可无，跟CA设置信息可不一致
supplied：必须填写这项申请信息
```

##### 13.2.4.1 创建私有CA 

1、创建CA所需要的文件

```bash
#生成证书索引数据库文件
touch /etc/pki/CA/index.txt 

#指定第一个颁发证书的序列号
echo 01 > /etc/pki/CA/serial 
```

2、 生成CA私钥

```bash
cd /etc/pki/CA/
(umask 066; openssl genrsa -out private/cakey.pem 2048)
```

3、生成CA自签名证书

```bash
openssl req -new -x509 -key /etc/pki/CA/private/cakey.pem -days 3650 -out     
/etc/pki/CA/cacert.pem
```

选项说明：

```
-new：生成新证书签署请求
-x509：专用于CA生成自签证书
-key：生成请求时用到的私钥文件
-days n：证书的有效期限
-out /PATH/TO/SOMECERTFILE: 证书的保存路径
```

国家代码：https://country-code.cl/

##### 13.2.4.2 申请证书并颁发证书 

1、为需要使用证书的主机生成生成私钥

```bash
(umask 066; openssl genrsa -out   /data/test.key 2048)
```

2、为需要使用证书的主机生成证书申请文件

```bash
openssl req -new -key /data/test.key -out /data/test.csr
```

3、在CA签署证书并将证书颁发给请求者

```bash
openssl ca -in /data/test.csr  -out   /etc/pki/CA/certs/test.crt -days 100
```

注意：默认要求 国家，省，公司名称三项必须和CA一致

4、查看证书中的信息：

```bash
openssl x509 -in /PATH/FROM/CERT_FILE -noout   -text|issuer|subject|serial|dates

#查看指定编号的证书状态
openssl ca -status SERIAL  
```

##### 13.2.4.3 吊销证书

在客户端获取要吊销的证书的serial

```bash
openssl x509 -in /PATH/FROM/CERT_FILE   -noout   -serial  -subject
```

在CA上，根据客户提交的serial与subject信息，对比检验是否与index.txt文件中的信息一致，吊销证 书：

```bash
openssl ca -revoke /etc/pki/CA/newcerts/SERIAL.pem
```

指定第一个吊销证书的编号,注意：第一次更新证书吊销列表前，才需要执行

```bash
echo 01 > /etc/pki/CA/crlnumber
```

更新证书吊销列表

```bash
openssl ca -gencrl -out /etc/pki/CA/crl.pem
```

查看crl文件：

```bash
openssl crl -in /etc/pki/CA/crl.pem -noout -text
```

##### 13.2.4.4 CentOS 7 创建自签名证书

##### 13.2.4.5 实战案例：在CentOS8上实现私有CA和证书申请 

###### 13.2.4.5.1 创建CA相关目录和文件

index.txt和serial文件在颁发证书时需要使用，如果不存在，会出现以下错误提示

###### 13.2.4.5.2 创建CA的私钥

###### 13.2.4.5.3 给CA颁发自签名证书

###### 13.2.4.5.4 用户生成私钥和证书申请

默认有三项内容必须和CA一致：国家，省份，组织，如果不同，会出现下面的提示

###### 13.2.4.5.5 CA颁发证书

###### 13.2.4.5.6 查看证书

###### 13.2.4.5.7 将证书相关文件发送到用户端使用

###### 13.2.4.5.8 证书的信任 

默认生成的证书，在windows上是不被信任的，可以通过下面的操作实现信任

###### 13.2.4.5.9 证书的吊销

###### 13.2.4.5.10 生成证书吊销列表文件

### 13.3 ssh服务 

#### 13.3.1 ssh服务介绍 

ssh: secure shell protocol, 22/tcp, 安全的远程登录，实现加密通信，代替传统的 telnet 协议

具体的软件实现： 

* OpenSSH：ssh协议的开源实现，CentOS 默认安装 
* dropbear：另一个ssh协议的开源项目的实现 

SSH 协议版本 

* v1：基于CRC-32做MAC，不安全；man-in-middle 

* v2：双方主机协议选择安全的MAC方式，基于DH算法做密钥交换，基于RSA或DSA实现身份认证

##### 13.3.1.1 公钥交换原理

* 客户端发起链接请求 
* 服务端返回自己的公钥，以及一个会话ID（这一步客户端得到服务端公钥） 
*  客户端生成密钥对 
* 客户端用自己的公钥异或会话ID，计算出一个值Res，并用服务端的公钥加密 
* 客户端发送加密后的值到服务端，服务端用私钥解密，得到Res 
* 服务端用解密后的值Res异或会话ID，计算出客户端的公钥（这一步服务端得到客户端公钥） 
* 最终：双方各自持有三个秘钥，分别为自己的一对公、私钥，以及对方的公钥，之后的所有通讯都 会被加密

##### 13.3.1.2 ssh加密通讯原理

#### 13.3.2 openssh 服务

OpenSSH是SSH （Secure SHell） 协议的免费开源实现，一般在各种Linux版本中会默认安装，基于C/S 结构 

Openssh软件相关包： 

* openssh 
* openssh-clients 
* openssh-server

服务器端程序：/usr/sbin/sshd 

Unit 文件：/usr/lib/systemd/system/sshd.service 

客户端： 

* Linux Client: ssh, scp, sftp，slogin 
* Windows Client：xshell, MobaXterm,putty, securecrt, sshsecureshellclient

##### 13.3.2.1 客户端 ssh命令 

ssh命令是ssh客户端，允许实现对远程系统经验证地加密安全访问 

当用户远程连接ssh服务器时，会复制ssh服务器/etc/ssh/ssh_host*key.pub文件中的公钥到客户机的 ~/.ssh/know_hosts中。下次连接时，会自动匹配相对应的私钥，不能匹配，将拒绝连接 

ssh客户端配置文件： /etc/ssh/ssh_config 

主要配置

```bash
#StrictHostKeyChecking ask
#首次登录不显示检查提示
StrictHostKeyChecking no 
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   IdentityFile ~/.ssh/id_ecdsa
#   IdentityFile ~/.ssh/id_ed25519
#   Port 22
```

格式：

```bash
ssh [user@]host [COMMAND]
ssh [-l user] host [COMMAND]
```

常见选项：

```bash
-p port #远程服务器监听的端口
-b #指定连接的源IP
-v #调试模式
-C #压缩方式
-X #支持x11转发
-t #强制伪tty分配，如：ssh -t remoteserver1 ssh -t remoteserver2   ssh   
remoteserver3
-o option   如：-o StrictHostKeyChecking=no 
-i <file>  #指定私钥文件路径，实现基于key验证，默认使用文件： ~/.ssh/id_dsa, 
~/.ssh/id_ecdsa, ~/.ssh/id_ed25519，~/.ssh/id_rsa等
```

##### 13.3.2.2 ssh登录验证方式介绍 

ssh服务登录的常用验证方式 

* 用户/口令  
* 基于密钥 

**基于用户和口令登录验证**

1. 客户端发起ssh请求，服务器会把自己的公钥发送给用户 
2. 用户会根据服务器发来的公钥对密码进行加密 
3. 加密后的信息回传给服务器，服务器用自己的私钥解密，如果密码正确，则用户登录成功

**基于密钥的登录方式**

1. 首先在客户端生成一对密钥（ssh-keygen） 
2. 并将客户端的公钥ssh-copy-id 拷贝到服务端 
3. 当客户端再次发送一个连接请求，包括ip、用户名 
4. 服务端得到客户端的请求后，会到authorized_keys中查找，如果有响应的IP和用户，就会随机生 成一个字符串，例如：magedu 
5. 服务端将使用客户端拷贝过来的公钥进行加密，然后发送给客户端 
6. 得到服务端发来的消息后，客户端会使用私钥进行解密，然后将解密后的字符串发送给服务端 
7. 服务端接受到客户端发来的字符串后，跟之前的字符串进行对比，如果一致，就允许免密码登录

##### 13.3.2.3 实现基于密钥的登录方式

在客户端生成密钥对

```bash
ssh-keygen -t rsa [-P 'password'] [-f “~/.ssh/id_rsa"]
```

把公钥文件传输至远程服务器对应用户的家目录

```bash
ssh-copy-id [-i [identity_file]] [user@]host
```

重设私钥口令：

```
ssh-keygen -p
```

验证代理（authentication agent）保密解密后的密钥，口令就只需要输入一次，在GNOME中，代理被 自动提供给root用户

```bash
#启用代理
ssh-agent bash
#钥匙通过命令添加给代理
ssh-add
```

在SecureCRT或Xshell实现基于key验证 

在SecureCRT工具—>创建公钥—>生成Identity.pub文件 

转化为openssh兼容格式（适合SecureCRT，Xshell不需要转化格式），并复制到需登录主机上相应文 件authorized_keys中,注意权限必须为600，在需登录的ssh主机上执行：

```bash
ssh-keygen  -i -f Identity.pub >> .ssh/authorized_keys
```

#### 13.3.3 其它ssh客户端工具

##### 13.3.3.1 scp命令

```bash
scp [options] SRC... DEST/
```

方式：

```bash
scp [options] [user@]host:/sourcefile /destpath
scp [options] /sourcefile [user@]host:/destpath
scp [options] [user@]host1:/sourcetpath [user@]host2:/destpath
```

常用选项：

```
-C 压缩数据流
-r 递归复制
-p 保持原文件的属性信息
-q 静默模式
-P PORT 指明remote host的监听的端口
```

##### 13.3.3.2 rsync 命令

rsync工具可以基于ssh和rsync协议实现高效率的远程系统之间复制文件，使用安全的shell连接做为传 输方式，比scp更快，基于增量数据同步，即只复制两方不同的文件，此工具来自于rsync包

**注意**：通信两端主机都需要安装 rsync 软件

```bash
rsync  -av /etc server1:/tmp #复制目录和目录下文件
rsync  -av /etc/ server1:/tmp #只复制目录下文件
```

常用选项：

```
-n 模拟复制过程
-v 显示详细过程
-r 递归复制目录树
-p 保留权限
-t 保留修改时间戳
-g 保留组信息
-o 保留所有者信息
-l 将软链接文件本身进行复制（默认）
-L 将软链接文件指向的文件复制
-u 如果接收者的文件比发送者的文件较新，将忽略同步
-z 压缩，节约网络带宽
-a 存档，相当于-rlptgoD，但不保留ACL（-A）和SELinux属性（-X）
--delete 源数据删除，目标数据也自动同步删除
```

##### 13.3.3.3 sftp命令

交互式文件传输工具，用法和传统的ftp工具相似，利用ssh服务实现安全的文件上传和下载 使用ls cd mkdir rmdir pwd get put等指令，可用？或help获取帮助信息

```bash
sftp [user@]host
sftp> help
```

##### 13.3.3.4 ssh 高级应用

SSH 会自动加密和解密所有 SSH 客户端与服务端之间的网络数据。此外，SSH 还能够将其他 TCP 端口 的网络数据通过 SSH 连接来转发，并且自动提供了相应的加密及解密服务。这一过程也被叫做“隧道” （tunneling），这是因为 SSH 为其他 TCP 链接提供了一个安全的通道来进行传输而得名。

例如， Telnet，SMTP，LDAP 这些 TCP 应用均能够从中得益，避免了用户名，密码以及隐私信息的明文传输。 而与此同时，如果工作环境中的防火墙限制了一些网络端口的使用，但是允许 SSH 的连接，也能够通过 将 TCP 端口转发来使用 SSH 进行通讯 

SSH 端口转发能够提供两大功能： 

* 加密 SSH Client 端至 SSH Server 端之间的通讯数据 
* 突破防火墙的限制完成一些之前无法建立的 TCP 连接

###### 13.3.3.4.1 SSH 本地端口转发 

SSH本地端口转发

```
ssh -L localport:remotehost:remotehostport   sshserver
```

选项：

```
-f 后台启用
-N 不打开远程shell，处于等待状态
-g 启用网关功能
```

###### 13.3.3.4.2 SSH远程端口转发

```
ssh -R sshserverport:remotehost:remotehostport sshserver
```

###### 13.3.3.4.3 SSH动态端口转发

```bash
#当用firefox访问internet时，本机的1080端口做为代理服务器，firefox的访问请求被转发到
sshserver上，由sshserver替之访问internet

ssh -D 1080 root@sshserver  -fNg

#在本机firefox设置代理socket proxy:127.0.0.1:1080
curl  --socks5 127.0.0.1:1080 http://www.google.com
```

###### 13.3.3.4.4 X 协议转发

所有图形化应用程序都是X客户程序,能够通过tcp/ip连接远程X服务器,数据没有加密，但是它通过ssh连 接隧道安全进行

```
#remotehost主机上的gedit工具，将会显示在本机的X服务器上,传输的数据将通过ssh连接加密
ssh -X user@remotehost gedit 
```

##### 13.3.3.5 ssh服务器配置 

服务器端：sshd 

服务器端的配置文件: /etc/ssh/sshd_config 

服务器端的配置文件帮助：man 5 sshd_config

**ssh服务的最佳实践** 

* 建议使用非默认端口 
* 禁止使用protocol version 1 
* 限制可登录用户 
* 设定空闲会话超时时长 
* 利用防火墙设置ssh访问策略 
* 仅监听特定的IP地址 
* 基于口令认证时，使用强密码策略，比如：tr -dc A-Za-z0-9_ < /dev/urandom | head -c 12|  xargs 
* 使用基于密钥的认证 
* 禁止使用空密码 
* 禁止root用户直接登录 
* 限制ssh的访问频度和并发在线数 
* 经常分析日志

#### 13.3.4 ssh 其它相关工具 

##### 13.3.4.1 挂载远程ssh目录 sshfs 

由EPEL源提供，目前CentOS8 还没有提供，可以利用ssh协议挂载远程目录

##### 13.3.4.2 自动登录ssh工具 sshpass

由EPEL源提供，ssh登陆不能在命令行中指定密码。sshpass的出现，解决了这一问题。sshpass用于非 交互SSH的密码验证，一般用在sh脚本中，无须再次输入密码（本机known_hosts文件中有的主机才能 生效）。它允许你用 -p 参数指定明文密码，然后直接登录远程服务器，它支持密码从命令行、文件、环 境变量中读取。

格式：

```
sshpass [option] command parameters
```

常见选项：

```
-p password #后跟密码它允许你用 -p 参数指定明文密码，然后直接登录远程服务器
-f filename #后跟保存密码的文件名，密码是文件内容的第一行
-e #将环境变量SSHPASS作为密码
```

##### 13.3.4.3 轻量级自动化运维工具 pssh

EPEL源中提供了多个自动化运维工具 

* pssh：基于python编写，可在多台服务器上执行命令的工具，也可实现文件复制，提供了基于ssh 和scp的多个并行工具，项目：http://code.google.com/p/parallel-ssh/ 
* pdsh：Parallel remote shell program，是一个多线程远程shell客户端，可以并行执行多个远程 主机上的命令。 可使用几种不同的远程shell服务，包括rsh，Kerberos IV和ssh，项目： https://pdsh.googlecode.com/ 
* mussh：Multihost SSH wrapper，是一个shell脚本，允许使用命令在多个主机上通过ssh执行命 令。 可使用ssh-agent和RSA/DSA密钥，以减少输入密码，项目：http://www.sourceforge.net/pr ojects/mussh 

**pssh 命令**

选项如下：

```
-H：主机字符串，内容格式”[user@]host[:port]” 
-h file：主机列表文件，内容格式”[user@]host[:port]” 
-A：手动输入密码模式
-i：每个服务器内部处理信息输出
-l：登录使用的用户名
-p：并发的线程数【可选】
-o：输出的文件目录【可选】
-e：错误输出文件【可选】
-t：TIMEOUT 超时时间设置，0无限制【可选】
-O：SSH的选项
-P：打印出服务器返回信息
-v：详细模式
--version：查看版本
```

**pscp.pssh命令** 

pscp.pssh功能是将本地文件批量复制到远程主机

```
pscp.pssh [-vAr] [-h hosts_file] [-H [user@]host[:port]] [-l user] [-p par] [-o 
outdir] [-e errdir] [-t timeout] [-O options] [-x args] [-X arg] local remote
```

pscp-pssh选项

```
-v 显示复制过程
-r 递归复制目录
```

**pslurp命令** 

pslurp功能是将远程主机的文件批量复制到本地

```
pslurp [-vAr] [-h hosts_file] [-H [user@]host[:port]] [-l user] [-p par][-o 
outdir] [-e errdir] [-t timeout] [-O options] [-x args] [-X arg] [-L localdir] 
remote local（本地名）
```

pslurp选项

```
-L 指定从远程主机下载到本机的存储的目录，local是下载到本地后的名称
-r 递归复制目录
```

### 13.4 利用 sudo 实现授权

#### 13.4.1 sudo 介绍

sudo 即superuser do，允许系统管理员让普通用户执行一些或者全部的root命令的一个工具，如halt， reboot，su等等。这样不仅减少了root用户的登录 和管理时间，同样也提高了安全性

在最早之前，一般用户管理系统的方式是利用su切换为超级用户。但是使用su的缺点之一在于必须要先 告知超级用户的密码。sudo于1980年前后推出，sudo使一般用户不需要知道超级用户的密码即可获得 权限。首先超级用户将普通用户的名字、可以执行的特定命令、按照哪种用户或用户组的身份执行等信 息，登记在特殊的文件中（通常是/etc/sudoers），即完成对该用户的授权（此时该用户称为 “sudoer”）；在一般用户需要取得特殊权限时，其可在命令前加上“sudo”，此时sudo将会询问该用户自 己的密码（以确认终端机前的是该用户本人），回答后系统即会将该命令的进程以超级用户的权限运 行。之后的一段时间内（默认为5分钟，可在/etc/sudoers自定义），使用sudo不需要再次输入密码。

由于不需要超级用户的密码，部分Unix系统甚至利用sudo使一般用户取代超级用户作为管理帐号，例如 Ubuntu、Mac OS X等。

sudo特性： 

* sudo能够授权指定用户在指定主机上运行某些命令。如果未授权用户尝试使用 sudo，会提示联系 管理员 
* sudo提供了丰富的日志，详细地记录了每个用户干了什么。它能够将日志传到中心主机或者日志服 务器 
* sudo使用时间戳文件来执行类似的“检票”系统。当用户调用sudo并且输入它的密码时，用户获得 了一张存活期为5分钟的票 
* sudo的配置文件是sudoers文件，它允许系统管理员集中的管理用户的使用权限和使用的主机。它 所存放的位置默认是在/etc/sudoers，属性必须为0440

#### 13.4.2 sudo 组成

包：sudo  

配置文件：/etc/sudo.conf 

授权规则配置文件：

```
/etc/sudoers
/etc/sudoers.d
```

安全编辑授权规则文件和语法检查工具

```
/usr/sbin/visudo
```

授权编辑规则文件的工具：/usr/bin/sudoedit 

执行授权命令：/usr/bin/sudo 

时间戳文件：/var/db/sudo 

日志文件：/var/log/secure

#### 13.4.3 sudo 命令

```
sudo命令
ls -l /usr/bin/sudo
sudo -i -u wang 切换身份功能和 su 相似,但不一样,sudo必须提前授权,而且要输入自已的密码
sudo [-u user] COMMAND 
-V 显示版本信息等配置信息
-u user 默认为root
-l,ll 列出用户在主机上可用的和被禁止的命令
-v 再延长密码有效期限5分钟,更新时间戳
-k 清除时间戳（1970-01-01），下次需要重新输密码
-K 与-k类似，还要删除时间戳文件
-b 在后台执行指令
-p 改变询问密码的提示符号
  示例：-p "password on %h for user %p: "
```

#### 13.4.4 sudo 授权规则配置

配置文件格式说明：/etc/sudoers, /etc/sudoers.d/  

配置文件中支持使用通配符 glob

```
? 任意单一字符
* 匹配任意长度字符
[wxc] 匹配其中一个字符
[!wxc] 除了这三个字符的其它字符
\x 转义 
[[alpha]] 字母  
```

配置文件规则有两类 

1、别名定义：不是必须的 

2、授权规则：必须的 

sudoers 授权规则格式：

```
用户 登入主机=(代表用户) 命令
user host=(runas) command
```

格式说明：

```
user: 运行命令者的身份
host: 通过哪些主机
(runas)：以哪个用户的身份
command: 运行哪些命令
```

sudoers的别名

```
User和runas: 
	username
	#uid
	%group_name
	%#gid
	user_alias|runas_alias
host:
	ip或hostname
	network(/netmask)
	host_alias
command:
	command name
	directory
	sudoedit
	Cmnd_Alias
```

sudo别名有四种类型： 

* User_Alias 
* Runas_Alias

* Host_Alias 
* Cmnd_Alias

别名格式：

```
[A-Z]([A-Z][0-9]_)*
```

别名定义：

```
Alias_Type NAME1 = item1,item2,item3 : NAME2 = item4, item5
```

#### 13.4.5 实战案例

### 13.5 PAM认证机制

#### 13.5.1 PAM 介绍 

认证库：文本文件，MySQL，NIS，LDAP等

PAM：Pluggable Authentication Modules，插件式的验证模块，Sun公司于1995 年开发的一种与认证 相关的通用框架机制。PAM 只关注如何为服务验证用户的 API，通过提供一些动态链接库和一套统一的 API，将系统提供的服务和该服务的认证方式分开，使得系统管理员可以灵活地根据需要给不同的服务配 置不同的认证方式而无需更改服务程序一种认证框架，自身不做认证

```
官网：http://www.linux-pam.org/
```

#### 13.5.2 PAM架构

PAM提供了对所有服务进行认证的中央机制，适用于本地登录，远程登录，如：telnet,rlogin,fsh,ftp,点 对点协议PPP，su等应用程序中，系统管理员通过PAM配置文件来制定不同应用程序的不同认证策略； 应用程序开发者通过在服务程序中使用PAM API(pam_xxxx( ))来实现对认证方法的调用；而PAM服务模 块的开发者则利用PAM SPI来编写模块（主要调用函数pam_sm_xxxx( )供PAM接口库调用，将不同的认 证机制加入到系统中；PAM接口库（libpam）则读取配置文件，将应用程序和相应的PAM服务模块联系 起来

#### 13.5.3 PAM相关文件

* 包名: pam  

* 模块文件目录：/lib64/security/*.so  

* 特定模块相关的设置文件：/etc/security/ 

* 应用程序调用PAM模块的配置文件 

  ​	主配置文件：/etc/pam.conf，默认不存在，一般不使用主配置 

  ​	为每种应用模块提供一个专用的配置文件：/etc/pam.d/APP_NAME 

  ​	注意：如/etc/pam.d存在，/etc/pam.conf将失效

#### 13.5.4 PAM工作原理 

PAM认证一般遵循这样的顺序：Service(服务)→PAM(配置文件)→pam_*.so 

PAM认证首先要确定那一项服务，然后加载相应的PAM的配置文件(位于/etc/pam.d下)，最后调用认证 文件(位于/lib64/security下)进行安全认证

PAM认证过程示例：

```
1.使用者执行/usr/bin/passwd 程序，并输入密码
2.passwd开始调用PAM模块，PAM模块会搜寻passwd程序的PAM相关设置文件，这个设置文件一般是在/etc/pam.d/里边的与程序同名的文件，即PAM会搜寻/etc/pam.d/passwd此设置文件
3.经由/etc/pam.d/passwd设定文件的数据，取用PAM所提供的相关模块来进行验证
4.将验证结果回传给passwd这个程序，而passwd这个程序会根据PAM回传的结果决定下一个动作（重新输入密码或者通过验证）
```

#### 13.5.5 PAM 配置文件格式说明

通用配置文件/etc/pam.conf格式,此格式不使用

```
application type control module-path arguments
```

专用配置文件/etc/pam.d/ 格式

```
type control module-path arguments

application：指服务名，如：telnet、login、ftp等，服务名字“OTHER”代表所有没有在该文件中明确
配置的其它服务
type：指模块类型，即功能
control ：PAM库该如何处理与该服务相关的PAM模块的成功或失败情况，一个关健词实现
module-path： 用来指明本模块对应的程序文件的路径名
Arguments： 用来传递给该模块的参
```

**模块类型（module-type）**

* Auth 账号的认证和授权 
* Account 帐户的有效性，与账号管理相关的非认证类的功能，如：用来限制/允许用户对某个服务 的访问时间，限制用户的位置(例如：root用户只能从控制台登录) 
* Password 用户修改密码时密码复杂度检查机制等功能 
* Session 用户会话期间的控制，如：最多打开的文件数，最多的进程数等 
* -type 表示因为缺失而不能加载的模块将不记录到系统日志,对于那些不总是安装在系统上的模块有 用 

**Control:** 

* required ：一票否决，表示本模块必须返回成功才能通过认证，但是如果该模块返回失败，失败结 果也不会立即通知用户，而是要等到同一type中的所有模块全部执行完毕，再将失败结果返回给应 用程序，即为必要条件 
* requisite ：一票否决，该模块必须返回成功才能通过认证，但是一旦该模块返回失败，将不再执 行同一type内的任何模块，而是直接将控制权返回给应用程序。是一个必要条件 
* sufficient ：一票通过，表明本模块返回成功则通过身份认证的要求，不必再执行同一type内的其 它模块，但如果本模块返回失败可忽略，即为充分条件，优先于前面的required和requisite 
* optional ：表明本模块是可选的，它的成功与否不会对身份认证起关键作用，其返回值一般被忽略 
* include： 调用其他的配置文件中定义的配置信息 

**module-path:** 

* 模块文件所在绝对路径： 
* 模块文件所在相对路径：/lib64/security目录下的模块可使用相对路径，如：pam_shells.so、 pam_limits.so 
* 有些模块有自已的专有配置文件，在/etc/security/*.conf目 录下 

**Arguments**  

* debug ：该模块应当用syslog( )将调试信息写入到系统日志文件中 
* no_warn ：表明该模块不应把警告信息发送给应用程序 
* use_first_pass ：该模块不能提示用户输入密码，只能从前一个模块得到输入密码 
* try_first_pass ：该模块首先用前一个模块从用户得到密码，如果该密码验证不通过，再提示用户 输入新密码 
* use_mapped_pass 该模块不能提示用户输入密码，而是使用映射过的密码 
* expose_account 允许该模块显示用户的帐号名等信息，一般只能在安全的环境下使用，因为泄漏 用户名会对安全造成一定程度的威胁

注意：修改PAM配置文件将马上生效 

建议：编辑pam规则时，保持至少打开一个root会话，以防止root身份验证错误

#### 13.5.6 PAM模块帮助

官方在线文档：http://www.linux-pam.org/Linux-PAM-html/ 

官方离线文档：http://www.linux-pam.org/documentation/ 

pam模块文档说明：/user/share/doc/pam-*

rpm -qd pam 

man -k pam_ 

man 模块名 如：man 8 rootok

#### 13.5.7 常用PAM模块 

##### 13.5.7.1 pam_shells 模块 

功能：检查有效shell 

帮助：man pam_shells

案例：不允许使用/bin/csh的用户本地登录

```bash
[root@centos8 ~]#yum -y install csh
[root@centos8 ~]#vim /etc/pam.d/login
auth required pam_shells.so 
[root@centos8 ~]#vim /etc/shells
去掉 /bin/csh
[root@centos8 ~]#useradd -s /bin/csh testuser
#testuser将不可登录
[root@centos8 ~]#tail /var/log/secure
```

##### 13.5.7.2 pam_securetty.so模块

功能：只允许root用户在/etc/securetty列出的安全终端上登陆

##### 13.5.7.3 pam_nologin.so 模块

功能：如果/etc/nologin文件存在,将导致非root用户不能登陆,当该用户登陆时，会显示/etc/nologin文 件内容，并拒绝登陆

##### 13.5.7.4 pam_limits.so 模块

功能：在用户级别实现对其可使用的资源的限制，例如：可打开的文件数量，可运行的进程数量，可用 内存空间

修改限制的实现方式： 

(1) ulimit命令 

ulimit是linux shell的内置命令，它具有一套参数集，用于对shell进程及其子进程进行资源限制。 

ulimit的设定值是 per-process 的，也就是说，每个进程有自己的limits值。 

使用ulimit进行修改，立即生效。 

ulimit只影响shell进程及其子进程，用户登出后失效。 

可以在profile中加入ulimit的设置，变相的做到永久生效。

```bash
-H 设置硬件资源限制.
-S 设置软件资源限制.
-a 显示当前所有的资源限制.
-c size:设置core文件的最大值.单位:blocks
-d size:设置数据段的最大值.单位:kbytes
-f size:设置创建文件的最大值.单位:blocks
-l size:设置在内存中锁定进程的最大值.单位:kbytes
-m size:设置可以使用的常驻内存的最大值.单位:kbytes
-n size:设置内核可以同时打开的文件描述符的最大值.单位:n
-p size:设置管道缓冲区的最大值.单位:kbytes
-s size:设置堆栈的最大值.单位:kbytes
-t size:设置CPU使用时间的最大上限.单位:seconds
-u size:最大用户进程数
-v size:设置虚拟内存的最大值.单位:kbytes
unlimited 是一个特殊值，用于表示不限制

#说明
查询时，若不加H或S参数，默认显示的是软限制
修改时，若不加H或S参数，两个参数一起改变
```

(2) 配置文件： 

pam_limits的设定值是基于 per-process 的

```
/etc/security/limits.conf
/etc/security/limits.d/*.conf
```

配置文件格式：

```
#每行一个定义
<domain>       <type> <item> <value>
```

格式说明： 

应用于哪些对象

```
Username 单个用户
@group 组内所有用户
* 所有用户
% 仅用于限制 maxlogins limit , 可以使用 %group 语法. 只用 % 相当于 * 对所有用户
maxsyslogins limit限制. %group 表示限制此组中的所有用户总的最大登录数
```

限制的类型

```
Soft 软限制,普通用户自己可以修改
Hard 硬限制,由root用户设定，且通过kernel强制生效
- 二者同时限定
```

限制的资源

```
nofile 所能够同时打开的最大文件数量,默认为1024
nproc 所能够同时运行的进程的最大数量,默认为1024
```

指定具体值

**生产案例**

```
vim /etc/security/limits.conf  
*    -   core       unlimited
*    -   nproc       1000000
*    -   nofile      1000000
*    -   memlock     32000
*    -   msgqueue    8192000
```

##### 13.5.7.5 pam_succeed_if 模块

功能：根据参数中的所有条件都满足才返回成功

##### 13.5.7.6 pam_google_authenticator 模块

```
什么是 MFA ?
Multi-Factor Authentication (MFA) 是一种简单有效的最佳安全实践方法，它能够在用户名和密码之外再额外增加一层安全保护。
```

功能：实现SSH登录的两次身份验证，先验证APP的数字码，再验证root用户的密码，都通过才可以登 录。 

官方网站：https://github.com/google/google-authenticator-android

### 13.6 时间同步服务

加密和安全当前都离不开时间的同步，否则各种网络服务可能不能正常运行

#### 13.6.1 计时方式 

##### 13.6.1.1 古代计时方式 

在远古时期，人类用来确定时间的方式是一些自然界“相对”亘古不变的周期。如：地球的公转是为一 年，月球的公转是为一月，地球的自转是为一天等，最早的计时可以追溯到公元前大约2000年，古埃及 人利用光线留下的影子用作计时的工具。影子拉得越长，计时越精确。古埃及人修建高耸入云的大型方 尖碑，来追踪太阳的移动，随后人们又利用了沙漏、日晷、钟摆等工具，巧妙地利用一些相对固定而准 确的周期来计时 

商朝人开发并使用了一种泄水型水钟——漏壶。后来又有用蜡烛和线香计时的 

北宋元祐元年（1086年），天文学家苏颂将浑仪、浑象和报时装置结合，建造一个划时代的计时工具 ——“水运仪象台” 

14世纪时，西方国家广泛使用机械钟。在十六世纪，奥斯曼帝国的科学家达兹·艾-丁（Taqi al-Din）发 明出了机械闹钟 

1583年，伽利略提出了著名的等时性理论，即不论摆动幅度的大小，完成一次摆动的时间是相同的。 

1656年，荷兰科学家克里斯蒂安·惠更斯（Christiaan Huygens）应用他的理论，设计出了世界第一只钟 摆 

1868年，百达翡丽（Patek Philippe）发明了手表

##### 13.6.1.2 现代计时方式 

石英晶体受到电池的电力影响时，会产生规律的振动。每秒的振动次数是32768次，可以设计电路来计 算振动次数，当计数到32768次时，即计时1秒。1967年，瑞士人发布了世界上首款石英表 

当原子从一个相对高的“能量态”迁至低的“能量态”时，会释放出电磁波，产生共振频率。依据此原理，拉 比构想出了一种全新的计时仪器——原子钟（Atomic clock） 

因为原子的共振频率是固定的。如：铯原子（Caesium133）的固有频率是9192631770赫兹，约合92亿 赫兹，对铯原子钟计数9192631770次，即可测量出一秒钟。很多国家（包括我国和美国NIST）的标准 局，就是用铯原子钟来作为时间精度标准的。GPS系统也是用铯原子钟来计时 

2008年诞生的锶（Strontium87）原子钟，固有频率为429228004229873，约合430万亿赫兹，将精度 提高到了10的17次方 

2013年镱元素（ytterbium）制成的原子钟问世，镱原子钟的固有频率约合518万亿赫兹，精度高达10 的18次方。宇宙的年龄为138亿年。如果这台镱原子钟从宇宙诞生之初就开始计时，直到今天也不会发 生1秒的误差

#### 13.6.2 时间同步服务

时间同步服务 

多主机协作工作时，各个主机的时间同步很重要，时间不一致会造成很多重要应用的故障，如：加密协 议，日志，集群等， 利用NTP（Network Time Protocol） 协议使网络中的各个计算机时间达到同步。 目前NTP协议属于运维基础架构中必备的基本服务之一 

时间同步软件实现： 

* ntp 
* chrony 

**ntp：** 

将系统时钟和世界协调时UTC同步，精度在局域网内可达0.1ms，在互联网上绝大多数的地方精度可以 达到1-50ms 

项目官网：http://www.ntp.org 

**chrony：** 

实现NTP协议的的自由软件。可使系统时钟与NTP服务器，参考时钟（例如GPS接收器）以及使用手表 和键盘的手动输入进行同步。还可以作为NTPv4（RFC 5905）服务器和对等体运行，为网络中的计算机 提供时间服务。设计用于在各种条件下良好运行，包括间歇性和高度拥挤的网络连接，温度变化（计算 机时钟对温度敏感），以及不能连续运行或在虚拟机上运行的系统。 

通过Internet同步的两台机器之间的典型精度在几毫秒之内，在LAN上，精度通常为几十微秒。利用硬 件时间戳或硬件参考时钟，可实现亚微秒的精度

#### 13.6.3 chrony 

##### 13.6.3.1 chrony介绍

chrony 的优势： 

* 更快的同步只需要数分钟而非数小时时间，从而最大程度减少了时间和频率误差，对于并非全天 24 小时运行的虚拟计算机而言非常有用 
* 能够更好地响应时钟频率的快速变化，对于具备不稳定时钟的虚拟机或导致时钟频率发生变化的节 能技术而言非常有用 
* 在初始同步后，它不会停止时钟，以防对需要系统时间保持单调的应用程序造成影响 
* 在应对临时非对称延迟时（例如，在大规模下载造成链接饱和时）提供了更好的稳定性 
* 无需对服务器进行定期轮询，因此具备间歇性网络连接的系统仍然可以快速同步时钟 

chrony官网：https://chrony.tuxfamily.org

chrony官方文档：https://chrony.tuxfamily.org/documentation.html

##### 13.6.3.2 chrony 文件组成 

包：chrony 

两个主要程序：chronyd和chronyc 

* chronyd：后台运行的守护进程，用于调整内核中运行的系统时钟和时钟服务器同步。它确定计算 机增减时间的比率，并对此进行补偿 
* chronyc：命令行用户工具，用于监控性能并进行多样化的配置。它可以在chronyd实例控制的计 算机上工作，也可在一台不同的远程计算机上工作 

服务unit 文件： /usr/lib/systemd/system/chronyd.service 

监听端口： 服务端: 123/udp,客户端: 323/udp 

配置文件： /etc/chrony.conf

##### 13.6.3.3 配置文件chrony.conf 

官方文档: https://chrony.tuxfamily.org/doc/3.5/chrony.conf.html

```bash
server  	#可用于时钟服务器，iburst 选项当服务器可达时，发送一个八个数据包而不是通常的一个数据包。 包间隔通常为2秒,可加快初始同步速度
pool     	#该指令的语法与server 指令的语法相似，不同之处在于它用于指定NTP服务器池而不是单个
NTP服务器。池名称应解析为随时间可能会变化的多个地址
driftfile 	#根据实际时间计算出计算机增减时间的比率，将它记录到一个文件中，会在重启后为系统时钟作出补偿
rtcsync  	#启用内核模式，系统时间每11分钟会拷贝到实时时钟（RTC）
allow / deny 			#指定一台主机、子网，或者网络以允许或拒绝访问本服务器
cmdallow / cmddeny 		#可以指定哪台主机可以通过chronyd使用控制命令
bindcmdaddress 			#允许chronyd监听哪个接口来接收由chronyc执行的命令
makestep 				# 通常chronyd将根据需求通过减慢或加速时钟，使得系统逐步纠正所有时间偏差。在某些特定情况下，系统时钟可能会漂移过快，导致该调整过程消耗很长的时间来纠正系统时钟。该指令强制chronyd在调整期大于某个阀值时调整系统时钟
local stratum 10  		#即使server指令中时间服务器不可用，也允许将本地时间作为标准时间授时给其它客户端
```

##### 13.6.3.4 ntp客户端工具 

chronyc 可以运行在交互式和非交互式两种方式，支持以下子命令

```
help 	命令可以查看更多chronyc的交互命令
accheck 检查是否对特定主机可访问当前服务器
activity 显示有多少NTP源在线/离线
sources [-v]   显示当前时间源的同步信息
sourcestats [-v]显示当前时间源的同步统计信息
add server 手动添加一台新的NTP服务器
clients 报告已访问本服务器的客户端列表
delete 手动移除NTP服务器或对等服务器
settime 手动设置守护进程时间
tracking 显示系统时间信息
```

##### 13.6.3.5 公共NTP服务

* pool.ntp.org：项目是一个提供可靠易用的NTP服务的虚拟集群cn.pool.ntp.org，0- 3.cn.pool.ntp.org 

* 阿里云公共NTP服务器 

  Unix/linux类：ntp.aliyun.com，ntp1-7.aliyun.com 

  windows类： time.pool.aliyun.com 

* 腾讯公共NTP 

  time1-5.cloud.tencent.com 

* 大学ntp服务 

  s1a.time.edu.cn 北京邮电大学 

  s1b.time.edu.cn 清华大学 

  s1c.time.edu.cn 北京大学 

* 国家授时中心服务器：210.72.145.44  
* 美国标准技术院: time.nist.gov

##### 13.6.3.6 时间工具 

* timedatectl 时间和时区管理

```bash
#查看日期时间、时区及NTP状态：
timedatectl

#查看时区列表：
timedatectl list-timezones

#修改时区：
timedatectl set-timezone Asia/Shanghai

#修改时区
root@ubuntu2004:~# rm -f /etc/localtime
root@ubuntu2004:~# ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

#修改日期时间：
timedatectl set-time "2017-01-23 10:30:00"

#开启NTP：
timedatectl set-ntp true/false
```

* ntpdate 时间同步命令,CentOS8版本此命令已淘汰 
* system-config-date：图形化配置chrony服务的工具

##### 13.6.3.7 实战案例: 实现私有的时间服务器

### 13.7 SELinux 

#### 13.7.1 SELinux 介绍和工作原理 

##### 13.7.1.1 SELinux 介绍 

SELinux：Security-Enhanced Linux， 是美国国家安全局(NSA=The National Security Agency)和 SCC(Secure Computing Corporation) 

开发的 Linux的一个强制访问控制的安全模块。2000年以GNU GPL发布，Linux内核2.6版本后集成在内 核中

DAC：Discretionary Access Control 自由访问控制 

MAC：Mandatory Access Control 强制访问控制 

DAC环境下进程是无束缚的 

MAC环境下策略的规则决定控制的严格程度 

MAC环境下进程可以被限制的 

策略被用来定义被限制的进程能够使用那些资源（文件和端口）

##### 13.7.1.2 SELinux 相关概念 

对象(object)：所有可以读取的对象，包括文件、目录和进程，端口等 

主体：进程称为主体(subject) 

SELinux中对所有的文件都赋予一个type的文件类型标签，对于所有的进程也赋予各自的一个domain的 

标签。domain标签能够执行的操作由安全策略里定义 

当一个subject试图访问一个object，Kernel中的策略执行服务器将检查AVC (访问矢量缓存Access  Vector Cache), 在AVC中，subject和object的权限被缓存(cached)，查找“应用+文件”的安全环境。然后 根据查询结果允许或拒绝访问 

安全策略：定义主体读取对象的规则数据库，规则中记录了哪个类型的主体使用哪个方法读取哪一个对 象是允许还是拒绝的，并且定义了哪种行为是充许或拒绝

##### 13.7.1.3 SELinux有四种工作类型 

* Strict：CentOS 5,每个进程都受到selinux的控制 
* targeted：用来保护常见的网络服务,仅有限进程受到selinux控制，只监控容易被入侵的进程， CentOS 4只保护13个服务，CentOS 5保护88个服务 
* minimum：CentOS 7,修改的 targeted，只对选择的网络服务 
* mls：提供MLS（多级安全）机制的安全性 

targeted为默认类型，minimum和mls稳定性不足，未加以应用，strict已不再使用

##### 13.7.1.4 SELinux安全上下文 

传统Linux，一切皆文件，由用户，组，权限控制访问，在SELinux中，一切皆对象（object），由存放 在inode的扩展属性域的安全元素所控制其访问，所有文件和端口资源和进程都具备安全标签：安全上 下文（security context） 

安全上下文有五个元素组成：

```
user:role:type:sensitivity:category
```

实际上下文：存放在文件系统中，ls -Z;ps -Z 

期望(默认)上下文：存放在二进制的SELinux策略库（映射目录和期望安全上下文）中 semanage  fcontext -l 

五个安全元素 

* User：指示登录系统的用户类型,进程：如system_u为系统服务进程，是受到管制的， unconfined_u为不管制的进程，用户自己开启的，如 bash，文件：system_u系统进程创建的文 件， unconfined_u为用户自已创建的文件 
* Role：定义文件，进程和用户的用途：进程：system_r为系统服务进程，受到管制。 unconfined_r 为不管制进程，通常都是用户自己开启的，如 bash，文件:object_r 
* Type：指定数据类型，规则中定义何种进程类型访问何种文件Target策略基于type实现,多服务共 用：public_content_t 
* Sensitivity：限制访问的需要，由组织定义的分层安全级别，如unclassified,secret,top,secret, 一 个对象有且只有一个sensitivity,分0-15级，s0最低,Target策略默认使用s0 
* Category：对于特定组织划分不分层的分类，如FBI Secret，NSA secret, 一个对象可以有多个 categroy， c0-c1023共1024个分类， Target 策略不使用category

#### 13.7.2 启用和禁用SELinux

SELinux的状态： 

* enforcing：强制，每个受限的进程都必然受限 
* permissive：允许，每个受限的进程违规操作不会被禁止，但会被记录于审计日志 
* disabled：禁用 

相关命令： 

* getenforce: 获取selinux当前状态 
* sestatus :查看selinux状态 
* setenforce 0|1 0: 设置为permissive 1: 设置为enforcing 

配置文件:

```
/boot/grub/grub.conf 在kernel行使用selinux=0禁用SELinux
/boot/grub2/grub.cfg 在linux16行使用selinux=0禁用SELinux
/etc/selinux/config 或 /etc/sysconfig/selinux 中 SELINUX=
{disabled|enforcing|permissive}
```

#### 13.7.3 管理文件安全标签 

给文件重新打安全标签chcon：

```
chcon [OPTION]... [-u USER] [-r ROLE] [-t TYPE] FILE...
chcon [OPTION]... --reference=RFILE FILE...
-R：递归打标
```

恢复目录或文件默认的安全上下文：

```
restorecon [-R] /path/to/somewhere
```

默认安全上下文查询与修改semanage：来自policycoreutils-python包

```bash
#查看默认的安全上下文
semanage fcontext -l

#添加安全上下文
semanage fcontext  -a -t httpd_sys_content_t   ‘/testdir(/.*)?’
restorecon -Rv /testdir

#删除安全上下文
semanage fcontext  -d -t httpd_sys_content_t   ‘/testdir(/.*)?’
```

#### 13.7.4 管理端口标签

```bash
#查看端口标签
semanage port -l

#添加端口
semanage port -a -t port_label -p tcp|udp PORT
semanage port -a -t http_port_t  -p tcp 9527

#删除端口
semanage port -d -t port_label -p tcp|udp PORT
semanage port -d -t http_port_t  -p tcp 9527

#修改现有端口为新标签
semanage port -m -t port_label -p tcp|udp PORT
semanage port -m -t http_port_t -p tcp 9527
```

#### 13.7.5 管理SELinux布尔值开关

```bash
#布尔型规则：
getsebool
setsebool

#查看bool命令：
getsebool [-a] [boolean]
semanage boolean -l
semanage boolean -l -C 查看修改过的布尔值

#设置bool值命令：
setsebool [-P] boolean value（on,off）
setsebool [-P] Boolean=value（1，0）
```

#### 13.7.6 管理日志

yum install setroubleshoot（重启生效） 

将错误的信息写入/var/log/message

```bash
grep setroubleshoot /var/log/messages
```

查看安全事件日志说明

```bash
sealert -l UUID 
```

扫描并分析日志

```bash
sealert -a /var/log/audit/audit.log
```

#### 13.7.7 查看SELinux帮助

```bash
yum -y install selinux-policy-devel ( centos7.2) 
yum -y install selinux-policy-doc 
mandb | makewhatis
man -k _selinux
```

# 14 运维自动化之系统部署

### 内容概述

* 系统安装过程和配置anaconda 
* 制作自动化安装系统的应答文件 
* 制作引导光盘 
* DHCP服务 
* TFTP服务 
* 利用PXE实现自动化的系统部署 
* 利用cobbler实现自动化的系统部署

### 14.1 系统安装过程 

#### 14.1.1 运维自动化发展历程及技术应用

#### 14.1.2 系统安装过程 

Linux的安装过程如下： 

* 加载boot loader 
* 加载启动安装菜单 
* 加载内核和initrd文件 
* 加载根系统 
* 运行anaconda的安装向导

##### 14.1.2.1 Linux安装光盘的安装相关文件 

在系统光盘的isolinux目录下有和安装相关的文件 

* boot.cat: 相当于grub的第一阶段 
* isolinux.bin：光盘引导程序，在mkisofs的选项中需要明确给出文件路径，这个文件属于 SYSLINUX项目 
* isolinux.cfg：启动菜单的配置文件，当光盘启动后（即运行isolinux.bin），会自动去找 isolinux.cfg文件 
* vesamenu.c32：是光盘启动后的启动菜单图形界面，也属于SYSLINUX项目，menu.c32提供纯文 本的菜单 
* memtest：内存检测程序 
* splash.png：光盘启动菜单界面的背景图 
* vmlinuz：是内核映像 
* initrd.img：ramfs文件

##### 14.1.2.2 安装菜单的内核参数 

安装光盘的启动菜单配置文件：isolinux/isolinux.cfg中设置相关的内核加载参数，实现不同的安装过程 

**isolinux.cfg文件中每个安装对应菜单选项：** 

* 加载内核：isolinuz/vmlinuz 
* 向内核传递参数：append initrd=initrd.img 参数设置 

**指定内核参数方法** 

* 在启动菜单界面，选中一项安装方法，按tab键,在后面增加参数 
* 在启动菜单界面，任意选中一项安装方法，按ESC键：boot: linux 参数设置 

**常见的内核参数：** 

* text：默认启动GUI安装接口，可以指定文本方式的安装界面 

* rescue：进入救援模式 

* inst.repo=path：指定安装源文件的路径，可以是以下格式 

  Centos 6

```
DVD drive 	repo=cdrom :device
Hard Drive 	repo=hd:device/path
HTTP Server 	repo=http://host/path
HTTPS Server 	repo=https://host/path
FTP Server 	repo=ftp://username:password@host/path
NFS Server 	repo=nfs:server:/path
ISO images on an NFS Server repo=nfsiso:server:/path
```

​	 Centos 7 以上版本

```
Any CD/DVD drive   inst.repo=cdrom
Hard Drive 	inst.repo=hd:device:/path
HTTP Server 	inst.repo=http://host/path
HTTPS Server 	inst.repo=https://host/path
FTP Server 	inst.repo=ftp://username:password@host/path
NFS Server 	inst.repo=nfs:[options:]server:/path
```

* askmethod：选择安装源文件的获取方法，提供了光盘，本地硬盘，NFS，FTP，HTTP多种安装 源，此项Centos 7 以后版已废弃 
* ks=path: 指定自动化安装应答文件路径，

```
initrd=initrd.img inst.ks=http://10.0.0.8/ksdir/centos8.cfg
```

* ip= : 指定IP地址信息

```
ip=method，method #可以为dhcp
ip=interface:method #指定特定接口
ip=ip::gateway:netmask:hostname:interface:none   #静态IP
```

##### 14.1.2.3 anaconda安装向导 

anaconda是Linux系统安装程序,可以提供两种风格的安装界面 

* gui：图形窗口 
* tui: 基于图形库curses的文本窗口 

**anaconda工作过程** 

* 安装过程使用的语言 

* 键盘类型

* 时区和时间 

* 安装源文件路径 

* 选定要安装的程序包 

* 安装目标存储设备及分区设置 

  Basic Storage：本地磁盘 

  特殊设备：iSCSI 

* KDUMP功能 

* 设定主机名和配置网络接口 

* 安全策略 

* 管理员密码 

* 创建一个普通用户 

anaconda的配置方式： 

* 交互式配置方式 
* 通过读取事先给定的配置文件自动完成配置，加内核参数：ks=/path实现指明kickstart文件的位 置，各种路径格式如下：

```
DVD drive: ks=cdrom:/PATH/TO/KICKSTART_FILE
Hard drive: ks=hd:device:/directory/KICKSTART_FILE
HTTP server: ks=http://host:port/path/to/KICKSTART_FILE
FTP server: ks=ftp://host:port/path/to/KICKSTART_FILE
HTTPS server: ks=https://host:port/path/to/KICKSTART_FILE
NFS server:ks=nfs:host:/path/to/KICKSTART_FILE
```

### 14.2 自动安装的应答文件 

实现自动安装前，需要制作对应的安装应答文件，称为kickstart文件，用于保存安装过程需要指定的选 项。

#### 14.2.1 kickstart文件使用过程 

1. Create a Kickstart file. 
2. Make the Kickstart file available on removable media, a hard drive or a network location. 
3. Create boot media, which will be used to begin the installation. 
4. Make the installation source available. 
5. Start the Kickstart installation.

#### 14.2.2 kickstart文件的格式 

##### 14.2.2.1 Kickstart文件格式官方说明 

https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/performing_an_advanced_rhel_installation/index 

https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/installation_guide/sect-kickstart-syntax 

https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/installation_guide/s1-kickstart2-options 

##### 14.2.2.2 kickstart文件格式说明 

kickstart文件主要包括三个部分：

* 命令段，程序包段，脚本段 

  命令段：指明各种安装前配置，如键盘类型等 

  命令段中的常见命令：

  keyboard: 设定键盘类型 

  lang: 语言类型 

  zerombr：清除mbr 

  clearpart：清除分区 

  part: 创建分区 

  rootpw: 指明root的密码 

  timezone: 时区 

  text: 文本安装界面 

  network:指定网络设置 

  firewall：设置防火墙设置 

  selinux：设置selinux设置 

  reboot：安装完自动重启 

  user：安装完成后为系统创建新用户 

  url: 指明安装源 

* 程序包段：指明要安装的程序包组或程序包，不安装的程序包等 

  %packages 

  @^environment group： 指定环境包组，如：@^minimal-environment 

  @group_name 

  package 

  package 

  %end 

* 脚本段： 

  %pre: 安装前脚本  

  %post: 安装后脚本 

**注意：** 

* CentOS 8,7,6 不同版本的kickstart文件格式不尽相同，不可混用 
* %addon, %packages, %onerror, %pre 、 %post 必须以%end结束，否则安装失败

#### 14.2.3 kickstart文件创建 

**创建kickstart文件的方式** 

* 可使用创建工具：system-config-kickstart ，注意：此方法 CentOS 8 不再支持 
* 依据某模板修改并生成新配置，CentOS安装完后，会自动参考当前系统的安装过程，生成一个 kickstart文件 /root/anaconda-ks.cfg  

检查ks文件的语法错误： 

使用 ksvalidator 工具可以检查kickstart的文件格式是否有语法错误，来自于 pykickstart 包 

格式：

```
ksvalidator /PATH/TO/KICKSTART_FILE
```

### 14.3 制作引导光盘和U盘

可以将定制安装光盘，并结合kickstart实现基于光盘启动的半自动化安装

**实现过程**

**mkisofs选项说明**

|         [OPTION]         |                             意义                             |
| :----------------------: | :----------------------------------------------------------: |
|            -o            |                     指定映像文件的名称。                     |
|            -b            |          指定在制作可开机光盘时所需的开机映像文件。          |
|            -c            | 制作可开机光盘时，会将开机映像文件中的 no-eltorito-catalog 全部内 容作成一个文件。 |
|      -no-emul-boot       |                       非模拟模式启动。                       |
|    -boot-load-size 4     |                      设置载入部分的数量                      |
|     -boot-info-table     |                    在启动的图像中现实信息                    |
|       -R 或 -rock        |                  使用 Rock RidgeExtensions                   |
|      -J 或 -joliet       |               使用 Joliet 格式的目录与文件名称               |
|      -v 或 -verbose      |               使用 Joliet 格式的目录与文件名称               |
| -T 或 -translation-table | 建立文件名的转换表，适用于不支持 Rock Ridge Extensions 的系统上 |

### 14.4 实现DHCP服务 

主机获取网络配置可以通过两种方式： 

* 静态指定 

* 动态获取:  

  bootp：boot protocol, MAC与IP一一静态对应 

  dhcp：增强的bootp，支持静态和动态

#### 14.4.1 DHCP工作原理 

DHCP: Dynamic Host Configuration Protocol，动态主机配置协议 

UDP协议，C/S模式，dhcp server：67/udp,dhcpv4 client :68/udp，dhcpv6 client：546/udp 

主要用途： 

* 用于内部网络和网络服务供应商自动分配IP地址给用户 
* 用于内部网络管理员作为对所有电脑作集中管理的手段 
* 自动化安装系统 
* 解决IPV4资源不足问题

**DHCP共有八种报文**

* DHCP DISCOVER：客户端到服务器 
* DHCP OFFER ：服务器到客户端 
* DHCP REQUEST：客户端到服务器 
* DHCP ACK ：服务器到客户端 
* DHCP NAK：服务器到客户端,通知用户无法分配合适的IP地址 
* DHCP DECLINE ：客户端到服务器，指示地址已被使用 
* DHCP RELEASE：客户端到服务器，放弃网络地址和取消剩余的租约时间 
* DHCP INFORM：客户端到服务器, 客户端如果需要从DHCP服务器端获取更为详细的配置信息，则 发送Inform报文向服务器进行请求，极少用到 

**DHCP服务续租** 

* 50% ：租赁时间达到50%时来续租，刚向DHCP服务器发向新的DHCPREQUEST请求。如果dhcp服 务没有拒绝的理由，则回应DHCPACK信息。当DHCP客户端收到该应答信息后，就重新开始新的租 用周期 
* 87.5%：如果之前DHCP Server没有回应续租请求，等到租约期的7/8时，主机会再发送一次广播请 求 

**同网段多DHCP服务**

* DHCP服务必须基于本地 
* 先到先得的原则

**跨网段**

* RFC 1542 Compliant Routers  
* dhcp relay agent: 中继代理 

**相关协议** 

* arp 
* rarp 

**租期：** 

长租期：IP相对稳定，网络资源消耗较少，但是浪费IP资源 

短租期：IP相对不稳定，网络资源消耗较多，但是IP资源可以充分利用，可以实现较少IP为较多的主机 服务 

#### 14.4.2 DHCP实现 

注意：实现DHCP服务前，先将网络已有DHCP服务，如：vmware中的DHCP关闭，访止冲突 

DHCP服务的实现软件： 

* dhcp（CentOS 7 之前版本） 或 dhcp-server（CentOS 8 中的包名） 
* dnsmasq：小型服务软件，可以提供dhcp和dns功能 

##### 14.4.2.1 DHCP相关文件组成 

**dhcp或dhcp-server 包文件组成** 

/usr/sbin/dhcpd dhcp服务主程序 

/etc/dhcp/dhcpd.conf dhcp服务配置文件 

/usr/share/doc/dhcp-server/dhcpd.conf.example #dhcp服务配置范例文件 

/usr/lib/systemd/system/dhcpd.service #dhcp服务service文件 

/var/lib/dhcpd/dhcpd.leases 地址分配记录 

**dhcp-client客户端包** 

/usr/sbin/dhclient #客户端程序 

/var/lib/dhclient #自动获取的IP信息 

**windows 工具** 

ipconfig /release #释放DHCP获取的IP，重新申请IP 

ipconfig/renew #刷新租约，续约 

##### 14.4.2.2 DHCP服务器配置文件 

帮助参考：man 5 dhcpd.conf 

注意: 

* DHCP服务器本身采用静态IP 
* 必须配置和DHCP网卡的静态IP所在网段的subnet 段,否则DHCP服务无法启动 

/etc/dhcp/dhcpd.conf 格式

```
全局配置
subnet {
 ...
 }
host {
}
```

检查语法命令：service dhcpd configtest （CentOS 6 之前版本支持）

DHCP配置文件其它配置选项： 

* next-server：提供引导文件的服务器IP地址 
* filename: 指明引导文件名称

### 14.5 实现TFTP服务 

#### 14.5.1 TFTP介绍 

TFTP：Trivial File Transfer Protocol ，是一种用于传输文件的简单高级协议，是文件传输协议（FTP） 的简化版本。用来传输比文件传输协议（FTP）更易于使用但功能较少的文件 

TFTP和FTP的区别 

1、安全性区别 

FTP支持登录安全，具有适当的身份验证和加密协议，在建立连接期间需要与FTP身份验证通信 TFTP是一种开放协议，缺乏安全性，没有加密机制，与TFTP通信时不需要认证 

2、传输层协议的区别 

FTP使用TCP作为传输层协议，TFTP使用UDP作为传输层协议 

3、使用端口的区别 

FTP使用2个端口：TCP端口21，是个侦听端口；TCP端口20或更高TCP端口1024以上用于源连接 TFTP仅使用一个具有停止和等待模式的端口：端口：69/udp 

4、RFC的区别 

FTP是基于RFC 959文档，带有其他RFC涵盖安全措施；TFTP基于RFC 1350文档 

5、执行命令的区别 

FTP有许多可以执行的命令（get，put，ls，dir，lcd）并且可以列出目录等 TFTP只有5个指令可以执行（rrq，wrq，data，ack，error） 

#### 14.5.2 安装和使用TFTP 

**安装包：** 

* tftp-server 服务器包 
* tftp 客户端包

### 14.6 利用 PXE 实现自动化系统部署 

#### 14.6.1 PXE介绍 

PXE：Preboot Excution Environment，预启动执行环境，是由Intel公司研发，基于Client/Server的网 络模式，支持远程主机通过网络从远端服务器下载映像，并由此支持通过网络启动操作系统，可以引导 和安装Windows，linux等多种操作系统 

**PXE启动工作原理**

#### 14.6.2 利用PXE实现自动化安装流程

1. Client向PXE Server上的DHCP发送IP地址请求消息，DHCP检测Client是否合法（主要是检测Client 的网卡MAC地址），如果合法则返回Client的IP地址，同时将启动文件pxelinux.0的所在TFTP服务 器地址信息一并传送给Client 
2. Client向TFTP服务器发送获取pxelinux.0请求消息，TFTP服务器接收到消息之后，向Client发送 pxelinux.0大小信息，试探Client是否满意，当TFTP收到Client发回的同意大小信息之后，正式向 Client发送pxelinux.0 
3. Client执行接收到的pxelinux.0文件，并利用此文件启动 
4. Client向TFTP 服务器发送请求针对本机的配置信息文件（在TFTP 服务器的pxelinux.cfg目录下）， TFTP服务器将启动菜单配置文件发回Client，继而Client根据启动菜单配置文件执行后续操作 
5. Client根据启动菜单配置文件里的信息，向TFTP发送Linux内核和initrd文件请求信息，TFTP接收到 消息之后将内核和initrd文件发送给Client 
6. Client向TFTP发送根文件请求信息，TFTP接收到消息之后返回Linux根文件系统 
7. Client启动Linux内核,加载相关的内核参数 
8. Client通过内核参数下载kickstart文件，并根据kickstart文件里的安装信息，下载安装源文件进行 自动化安装 

**UEFI 客户端的安装文档：**

 https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/performing_an_advanced_rhel_installation/preparing-for-a-network-install_installing-rhel-as-an-experienced-user#configuring-a-tftp-server-for-uefi-based-clients_preparing-for-a-network-install

#### 14.6.3 在CentOS 6 上实现PXE自动化安装CentOS 

##### 14.6.3.1 安装前准备 

关闭防火墙和SELINUX，DHCP服务器静态IP 

网络要求：关闭Vmware软件中的DHCP服务 

##### 14.6.3.2 安装相关软件包并启动

```bash
[root@centos6 ~]#yum install dhcp httpd tftp-server syslinux
[root@centos6 ~]#chkconfig tftp on
[root@centos6 ~]#chkconfig httpd on
[root@centos6 ~]#chkconfig dhcpd on
[root@centos6 ~]#service httpd start
[root@centos6 ~]#service xneted start?
```

##### 14.6.3.3 准备yum 源和相关目录

```bash
[root@centos6 ~]#mkdir -pv /var/www/html/centos/6/os/x86_64
[root@centos6 ~]#mount /dev/sr0 /var/www/html/centos/6/os/x86_64
```

##### 14.6.3.4 准备kickstart文件

```bash
[root@centos6 ~]#mkdir /var/www/html/ks/
[root@centos6 ~]#vim /var/www/html/centos/ks/centos6.cfg
[root@centos6 ~]#grep -vE '^#|^$' /var/www/html/centos/ks/centos6.cfg
install
text
reboot
...
```

##### 14.6.3.5 配置DHCP服务

```bash
[root@centos6 ~]#cp /usr/share/doc/dhcp-4.1.1/dhcpd.conf.sample 
/etc/dhcp/dhcpd.conf
[root@centos6 ~]#vim /etc/dhcp/dhcpd.conf
[root@centos6 ~]#cat /etc/dhcp/dhcpd.conf
option domain-name "example.com";
option domain-name-servers 10.0.0.1;
subnet 10.0.0.0 netmask 255.255.255.0 {
   range 10.0.0.1 10.0.0.200;
   option routers 10.0.0.1;
   filename "pxelinux.0";
   next-server 10.0.0.100;
}
[root@centos6 ~]#service dhcpd start
```

##### 14.6.3.6 准备PXE启动相关文件

```bash
[root@centos6 ~]#mkdir /var/lib/tftpboot/pxelinux.cfg/
[root@centos6 ~]#cp /usr/share/syslinux/pxelinux.0   /var/lib/tftpboot/
[root@centos6 ~]#cd /misc/cd/images/pxeboot/
[root@centos6 ~]#cp vmlinuz initrd.img /var/lib/tftpboot
[root@centos6 ~]#Cd /misc/cd/isolinux/
[root@centos6 ~]#cp boot.msg vesamenu.c32 splash.jpg /var/lib/tftpboot
[root@centos6 ~]#cp /misc/cd/isolinux/isolinux.cfg 
/var/lib/tftpboot/pxelinux.cfg/default
[root@centos6 ~]#tree /var/lib/tftpboot/
/var/lib/tftpboot/
...
```

##### 14.6.3.7 准备启动菜单文件

```bash
[root@centos6 ~]#vim /var/lib/tftpboot/pxelinux.cfg/default 
default vesamenu.c32 #指定菜单风格
#prompt 1
timeout 600?
display boot.msg?
...
```

##### 14.6.3.8 测试客户端基于PXE实现自动安装 

新准备一台主机，设置网卡引导，可看到看启动菜单，并实现自动安装 

#### 14.6.4 在CentOS7 上实现PXE自动化安装 CentOS 

##### 14.6.4.1 安装前准备 

关闭防火墙和SELINUX，DHCP服务器静态IP 

网络要求：关闭Vmware软件中的DHCP服务 

##### 14.6.4.2 安装相关软件包并启动服务

```bash
[root@centos7 ~]#yum -y install httpd tftp-server dhcp syslinux system-configkickstart
[root@centos7 ~]#systemctl enable --now httpd tftp dhcpd
```

**注意**：由于dhcp还没有配置，此时还无法立即启动

##### 14.6.4.3 准备yum源和相关目录

```bash
[root@centos7 ~]#mkdir -pv /var/www/html/centos/7/os/x86_64
[root@centos7 ~]#mount /dev/sr0 /var/www/html/centos/7/os/x86_64
```

##### 14.6.4.4 准备kickstart文件

```bash
[root@centos7 ~]#mkdir /var/www/html/ks/
[root@centos7 ~]#vim /var/www/html/ks/centos7.cfg  
[root@centos7 ~]#grep -vE '^#|^$' /var/www/html/ks/centos7.cfg 
install
xconfig  --startxonboot
keyboard --vckeymap=us --xlayouts='us'
...
```

##### 14.6.4.5 配置DHCP服务

##### 14.6.4.6 准备PXE启动相关文件

##### 14.6.4.7 准备PXE启动相关文件

##### 14.6.4.8 测试客户端基于PXE实现自动安装 

新准备一台主机，设置网卡引导，可看到看启动菜单，并实现自动安装

#### 14.6.5 在 CentOS 8 上实现PXE自动化安装 CentOS 6,7,8 

##### 14.6.5.1 安装前准备 

关闭防火墙和SELINUX，DHCP服务器静态IP 

网络要求：关闭Vmware软件中的DHCP服务，基于NAT模式 

**注意：使用 1G 以下内存的主机安装CentOS 7，8 会提示空间不足，建议2G以上**

##### 14.6.5.2 安装相关软件包并启动

```bash
[root@centos8 ~]#dnf -y install dhcp-server tftp-server httpd syslinuxnonlinux(或者syslinux-tftpboot)
[root@centos8 ~]#systemctl enable --now httpd tftp dhcpd 
```

##### 14.6.5.3 配置DHCP服务

##### 14.6.5.4 准备yum 源和相关目录

##### 14.6.5.5 准备kickstart文件

##### 14.6.5.6 准备PXE启动相关文件

##### 14.6.5.7 准备启动菜单文件

##### 14.6.5.8 测试客户端基于PXE实现自动安装

新准备一台主机，设置网卡引导，可看到看启动菜单，并实现自动安装 

注意：VMware workstation 对于不同的CentOS 版本，生成的虚拟机的硬件并不兼容

### 14.7 利用cobbler实现自动化安装 

#### 14.7.1 Cobbler简介 

* Cobbler是一款Linux生态的自动化运维工具，基于Python2开发，用于自动化批量部署安装操作系 统；其提供基于CLI的管理方式和WEB配置界面，其中WEB配置界面是基于Python2和Django框架 开发。另外，cobbler还提供了API，方便二次开发。Cobbler属于C/S模型(客户端/服务器模型) 
* Cobbler主要用于快速网络安装linux操作系统，支持众多的Linux发行版如：Red Hat、Fedora、 CentOS、Debian、Ubuntu和SuSE等，甚至支持Windows的安装 
* Cobbler实质是PXE的二次封装，将多种安装参数封装到一起，并提供统一的管理方法 
* 官方文档: https://cobbler.readthedocs.io/en/latest/index.html 

#### 14.7.2 Cobbler 的相关服务 

* 使用Cobbler安装系统需要一台专门提供各种服务的服务器，提供的服务包括 (HTTP/FTP/NFS,TFTP,DHCP),也可以将这几个服务分别部署到不同服务器。事实上在实际应用中， 总是将不同的服务分别部署到专门的服务器。 
* Cobbler是在HTTP、TFTP、DHCP等各种服务的基础上进行相关操作的，实际安装的大体过程类似 于基于PXE的网络安装:客户端(裸机)开机使用网卡引导启动，其请求DHCP分配一个地址后从TFTP 服务器获取启动文件,加载到客户端本地内存中运行，并显示出可安装的系统列表；在人为的选定安 装的操作系统类型后,客服端会到HTTP服务器下载相应的系统安装文件并执行自动安装

#### 14.7.3 Cobbler的工作原理

* client裸机配置了从网络启动后，开机后会广播包请求DHCP服务器（cobbler server）发送其分配 好的一个IP 
* DHCP服务器（cobbler server）收到请求后发送responese，包括其ip地址 
* client裸机拿到ip后再向cobbler server发送请求OS引导文件的请求 
* cobbler server告诉裸机OS引导文件的名字和TFTP server的ip和port 
* client裸机通过上面告知的TFTP server地址通信，下载引导文件 
* client裸机执行执行该引导文件，确定加载信息，选择要安装的os，期间会再向cobbler server请求 kickstart文件和os image 
* cobbler server发送请求的kickstart和os iamge 
* client裸机加载kickstart文件 
* client裸机接收os image，安装该os image 

#### 14.7.4 安装Cobbler及其相关的服务和组件 

Cobbler所依赖的服务包括HTTPD,TFTP,DHCP等，如果有web界面要求，还需要安装相关的组件 

CentOS 8 目前还没有提供Cobbler相关包

```bash
[root@centos7 ~]#yum -y install dhcp cobbler cobbler-web pykickstart 
[root@centos7 ~]#systemctl enable --now cobblerd httpd tftp dhcpd 
```

相关包说明： 

* httpd：提供yum源，并配合cobbler-web使得cobbler可以通过web网页界面进行配置管理 
* tftp-server：提供启动和菜单等相关文件网络下载功能 
* cobbler-web : 提供基于web的cobbler管理界面 
* pykickstart.noarch : 基于python的管理kickstart文件的库 

说明： 

* Cobbler依赖于epel源，在安装cobbler之前需要配置epel源

* 在安装cobbler时会自因为依赖而安装httpd,tftp-server相关包

#### 14.7.5 Cobbler配置文件及各目录情况 

##### 14.7.5.1 配置文件

```bash
/etc/cobbler/settings  #cobbler 主配置文件
/etc/cobbler/iso/  #iso模板配置文件
/etc/cobbler/pxe   #pxe模板文件
/etc/cobbler/power  #电源配置文件
/etc/cobbler/user.conf   #web服务授权配置文件
/etc/cobbler/users.digest  #web访问的用户名密码配置文件
/etc/cobbler/dhcp.template #dhcp服务器的的配置模板
/etc/cobbler/dnsmasq.template #dns服务器的配置模板
/etc/cobbler/tftpd.template  #tftp服务的配置模板
/etc/cobbler/modules.conf #cobbler模块的配置文件
```

##### 14.7.5.2 数据目录

```bash
/var/lib/cobbler/config/     #用于存放distros，system，profiles 等信息的配置文件
/var/lib/cobbler/triggers/   #用于存放用户定义的cobbler命令
/var/lib/cobbler/kickstarts/  # 默认存放kickstart文件
/var/lib/cobbler/loaders/     #存放各种引导程序
```

##### 14.7.5.3 镜像目录

```bash
/var/www/cobbler/ks_mirror/    #导入的发行版系统的所有数据
/var/www/cobbler/images/       #导入发行版kernel和initrd镜像用于远程网络启动
/var/www/cobbler/repo_mirror/   #yum 仓库存储目录
```

##### 14.7.5.4 日志目录

```bash
/var/log/cobbler/installing  #客户端安装日志
/var/log/cobbler/cobbler.log #cobbler日志
```

#### 14.7.6 配置及启动cobblerd服务 

检测cobbler的运行环境,并根据提示逐步配置cobbler

#### 14.7.7 cobbler命令用法

常见用法：

```
#列出当前导入的linux发行版条目
cobbler distro list 
#报告当前所有的linux发行版详细信息
cobbler distro report 
#导入系统源文件生成仓库
cobbler import --name=centos-8.0-x86_64 --path=/mnt --arch=x86_64
cobbler import --name=centos7-x86_64 --path=/mnt --arch=x86_64
#将linux发行版系统镜像与其对应的ks文件建立关联
cobbler profile add --name=centos7 --distro=centos7-x86_64 --
kickstart=/var/lib/cobbler/kickstarts/ks7.cfg 
```

#### 14.7.8 将linux发行版导入到cobbler在httpd服务的文件夹下 

cobbler将系统yum源文件存放在 /var/www/cobbler/ks_mirror目录下

```bash
cobbler import --name=centos6 --path=/var/www/html/centos/6/isos/x86_64/ --
arch=x86_64
cobbler import --name=centos7 --path=/var/www/html/centos/7/isos/x86_64/ --
arch=x86_64
cobbler import --name=centos8 --path=/var/www/html/centos/8/isos/x86_64/ --
arch=x86_64
```

导入后重启并同步

```bash
systemctl restart cobblerd
cobbler sync
```

#### 14.7.9 配置linux发行版和关联相应的ks文件 

拷贝事先准备好的ks文件至/var/lib/cobbler/kickstarts目录下

```bash
[root@centos7 ~]#cp /var/www/html/ks/centos{6,7,8}.ks 
/var/lib/cobbler/kickstarts
```

检查ks文件语法是否有误

```bash
[root@centos7 ~]#cobbler validateks
```

将linux发行版系统镜像与其对应的ks文件建立关联

```bash
cobbler profile add --name=centos6 --distro=centos6 --
kickstart=/var/lib/cobbler/kickstarts/centos6.cfg 
cobbler profile add --name=centos7 --distro=centos7 --
kickstart=/var/lib/cobbler/kickstarts/centos7.cfg 
cobbler profile add --name=centos8 --distro=centos8 --
kickstart=/var/lib/cobbler/kickstarts/centos8.cfg
```

注意，在导入distro时，cobbler会自动生成distro条目，这些并未和ks文件关联，可以使用

```
cobbler profile remove --name=PROFILE_NAME
```

删除后，再关联ks文件。 

建立关联后重启并同步

```
systemctl restart cobblerd
cobbler sync
```

查看详细信息

```
cobbler report
```

#### 14.7.10 启动菜单优化

修改/etc/cobbler/pxe/pxedefault.template模板文件，重启同步

重启同步后cobbler更新文件/var/lib/tftpboot/pxelinux.cfg/default

#### 14.7.11 基于web界面来管理配置cobbler 

##### 14.7.11.1 安装cobbler-web

```bash
yum install cobbler-web`
systemctl restart httpd
```

##### 14.7.11.2 访问web界面

用浏览器访问：https://cobblerserver/cobbler_web 

cobbler-web界面的默认账号，用户名：cobbler 密码:cobbler

##### 14.7.11.3 WEB的登录认证方式 

认证方法配置文件：/etc/cobbler/modules.conf 

支持多种认证方法： 

* authn_configfile，此为默认的认证方法 
* authn_pam 

**使用authn_configfile模块认证cobbler_web用户**

```
vim /etc/cobbler/modules.conf 
[authentication]
module=authn_configfile
```

创建其认证文件/etc/cobbler/users.digest，并添加所需的用户

```
htdigest -c /etc/cobbler/users.digest Cobbler admin 
```

使用已有用户文件，在其中添加新用户

```
htdigest /etc/cobbler/users.digest Cobbler admin2 
```

注意： 

* 使用“-c”选项用于创建用户文件，如果文件已存在，将覆盖原文件 
* cobbler_web的realm只能为Cobbler

**使用authn_pam模块认证cobbler_web用户**

```bash
vim /etc/cobbler/modules.conf
[authentication] 
module = authn_pam

systemctl restart cobblerd
```

创建cobbler用户：

```bash
useradd -s /sbin/nologin cobbleruser
echo magedu | passwd --stdin cobbleruser 
vim /etc/cobbler/users.conf 
[admins]
admin = "cobbleruser"
```

#### 14.7.12 Trouble Shooting

#### 14.7.13 实战案例：CentOS 7 基于cobbler实现系统的自动化安装

# 15 域名系统 DNS

### 内容概述

* 名字解析介绍
* DNS服务工作原理 
* 实现主服务器 
* 实现反向解析区域 
* 实现从服务器 
* 实现子域 
* 实现转发 
* 实现智能DNS 
* DNS排错 
* 实现 Internet 的DNS构架

### 15.1 名字解析介绍和DNS

当前TCP/IP网络中的设备之间进行通信，是利用和依赖于IP地址实现的。但数字形式的IP地址是很难记 忆的。当网络设备众多，想要记住每个设备的IP地址，可以说是"不可能完成的任务"。那么如何解决这 一难题呢？我们可以给每个网络设备起一个友好的名称，如：www.magedu.org，这种由文字组成的名 称，显而易见要更容易记忆。但是计算机不会理解这种名称的，我们可以利用一种名字解析服务将名称 转化成（解析）成IP地址。从而我们就可以利用名称来直接访问网络中设备了。除此之外还有一个重要 功能，利用名称解析服务可以实现主机和IP的解耦，即：当主机IP变化时，只需要修改名称服务即可， 用户仍可以通过原有的名称进行访问而不受影响。 

实现此服务的方法是多样的。如下面所述： 

本地名称解析配置文件：hosts

```bash
Linux: /etc/hosts
windows: %WINDIR%/system32/drivers/etc/hosts

#格式
122.10.117.2 www.magedu.org.
93.46.8.89   www.google.com.
```

DNS：Domain Name System 域名系统,应用层协议,是互联网的一项服务。它作为将域名和IP地址相互 映射的一个分布式数据库，能够使人更方便地访问互联网,基于C/S架构，服务器端：53/udp, 53/tcp 

BIND：Bekerley Internet Name Domain,由 ISC （www.isc.org）提供的DNS软件实现DNS域名结构

* 根域: 全球根服务器节点只有13个,10个在美国，1个荷兰，1个瑞典，1个日本 
* 一级域名：Top Level Domain: tld 三类：组织域、国家域(.cn, .ca, .hk, .tw)、反向域 com, edu, mil, gov, net, org, int,arpa  
* 二级域名：magedu.com 
* 三级域名：study.magedu.com 
* 最多可达到127级域名

ICANN（The Internet Corporation for Assigned Names and Numbers）互联网名称与数字地址分配 机构，负责在全球范围内对互联网通用顶级域名（gTLD）以及国家和地区顶级域名（ccTLD）系统的管 理、以及根服务器系统的管理

#### 15.1.2 DNS服务工作原理

根服务器的安全

**雪人计划**

```
在与现有IPv4根服务器体系架构充分兼容基础上，"雪人计划"于2016年在美国、日本、印度、俄罗斯、德国、法国等全球16个国家完成25台IPv6根服务器架设，其中1台主根和3台辅根部署在中国，事实上形成了13台原有根加25台IPv6根的新格局
```

#### 15.1.3 DNS查询类型

* 递归查询：一般客户机和本地DNS服务器之间属于递归查询，即当客户机向DNS服务器发出请求后, 若DNS服务器本身不能解析，则会向另外的DNS服务器发出查询请求，得到最终的肯定或否定的结 果后转交给客户机。此查询的源和目标保持不变,为了查询结果只需要发起一次查询 
* 迭代查询：一般情况下(有例外)本地的DNS服务器向其它DNS服务器的查询属于迭代查询,如：若对 方不能返回权威的结果，则它会向下一个DNS服务器(参考前一个DNS服务器返回的结果)再次发起 进行查询，直到返回查询的结果为止。此查询的源不变,但查询的目标不断变化,为查询结果一般需 要发起多次查询

#### 15.1.4 名称服务器 

Name Server,域内负责解析本域内的名称的DNS服务器 

IPv4的根名称服务器：全球共13个负责解析根域的DNS服务器，美国10个，荷兰1，瑞典1，日本1 

IPv6的根名称服务器：全球共25个，中国1主3从，美国1主2从

#### 15.1.5 解析类型 

* FQDN --> IP 正向解析 

* IP --> FQDN 反向解析

注意：正反向解析是两个不同的名称空间，是两棵不同的解析树

#### 15.1.6 完整的查询请求经过的流程

```
Client -->hosts文件 --> Client DNS Service Local Cache --> DNS Server (recursion递归) --> DNS Server Cache -->DNS iteration(迭代) --> 根--> 顶级域名DNS-->二级域名DNS… 
```

### 15.2 DNS 服务相关概念和技术 

#### 15.2.1 DNS服务器的类型

* 主DNS服务器 
* 从DNS服务器 
* 缓存DNS服务器（转发器）

##### 15.2.1.1 主DNS服务器 

管理和维护所负责解析的域内解析库的服务器

##### 15.2.1.2 从DNS服务器 

从主服务器或从服务器"复制"（区域传输）解析库副本 

* 序列号：解析库版本号，主服务器解析库变化时，其序列递增 
* 刷新时间间隔：从服务器从主服务器请求同步解析的时间间隔 
* 重试时间间隔：从服务器请求同步失败时，再次尝试时间间隔 
* 过期时长：从服务器联系不到主服务器时，多久后停止服务 
* 通知机制：主服务器解析库发生变化时，会主动通知从服务器

#### 15.2.2 区域传输 

* 完全传输：传送整个解析库 
* 增量传输：传递解析库变化的那部分内容

#### 15.2.3 解析形式 

* 正向：FQDN（ Fully Qualified Domain Name） --> IP 
* 反向: IP --> FQDN

#### 15.2.4 负责本地域名的正向和反向解析库 

* 正向区域 
* 反向区域

#### 15.2.5 解析答案 

* 肯定答案：存在对应的查询结果 
* 否定答案：请求的条目不存在等原因导致无法返回结果 
* 权威答案：直接由存有此查询结果的DNS服务器（权威服务器）返回的答案 
* 非权威答案：由其它非权威服务器返回的查询答案

#### 15.2.6 各种资源记录 

区域解析库：由众多资源记录RR(Resource Record)组成 

记录类型：A, AAAA, PTR, SOA, NS, CNAME, MX 

* SOA：Start Of Authority，起始授权记录；一个区域解析库有且仅能有一个SOA记录，必须位于解 析库的第一条记录 
* A：internet Address，作用，FQDN --> IP 
* AAAA：FQDN --> IPv6 
* PTR：PoinTeR，IP --> FQDN 
* NS：Name Server，专用于标明当前区域的DNS服务器 
* CNAME ： Canonical Name，别名记录 
* MX：Mail eXchanger，邮件交换器 
* TXT：对域名进行标识和说明的一种方式，一般做验证记录时会使用此项，如：SPF（反垃圾邮 件）记录，https验证等，如下示例：

```
_dnsauth TXT 2012011200000051qgs69bwoh4h6nht4n1h0lr038x
```

##### 15.2.6.1 资源记录定义的

```
name [TTL] IN rr_type value
```

**注意：** 

1. TTL可从全局继承 
2. 使用 "@" 符号可用于引用当前区域的域名 
3. 同一个名字可以通过多条记录定义多个不同的值；此时DNS服务器会以轮询方式响应 
4. 同一个值也可能有多个不同的定义名字；通过多个不同的名字指向同一个值进行定义；此仅表示通 过多个不同的名字可以找到同一个主机

面试题：

```
1. 我的网站域名需要更改，如何使其更快的生效？
2. 更改TTL值为多少比较合适呢？是如何生效的？
```

##### 15.2.6.2 SOA记录

name: 当前区域的名字，例如"magedu.org." 

value: 有多部分组成

**注意：** 

1. 当前区域的主DNS服务器的FQDN，也可以使用当前区域的名字 
2. 当前区域管理员的邮箱地址；但地址中不能使用@符号，一般用.替换 例如：admin.magedu.org 
3. 主从服务区域传输相关定义以及否定的答案的统一的TTL

##### 15.2.6.3 NS记录 

name: 当前区域的名字 

value: 当前区域的某DNS服务器的名字，例如: ns.magedu.org. 

**注意：**

1. 相邻的两个资源记录的name相同时，后续的可省略 
2. 对NS记录而言，任何一个ns记录后面的服务器名字，都应该在后续有一个A记录 
3. 一个区域可以有多个NS记录

##### 15.2.6.4 MX记录 

name: 当前区域的名字 

value: 当前区域的某邮件服务器(smtp服务器)的主机名 

**注意：** 

1. 一个区域内，MX记录可有多个；但每个记录的value之前应该有一个数字(0-99)，表示此服务器的 优先级；数字越小优先级越高 
2. 对MX记录而言，任何一个MX记录后面的服务器名字，都应该在后续有一个A记录

##### 15.2.6.5 A记录 

name: 某主机的FQDN，例如：www.magedu.org. 

value: 主机名对应主机的IP地址 

避免用户写错名称时给错误答案，可通过泛域名解析进行解析至某特定地址

##### 15.2.6 6 AAAA记录

```
name: FQDN
value: IPv6
```

##### 15.2.6.7 PTR记录

```
name: IP，有特定格式，把IP地址反过来写，1.2.3.4，要写作4.3.2.1；而有特定后缀：inaddr.arpa.，所以完整写法为：4.3.2.1.in-addr.arpa.
value: FQDN
```

**注意**：网络地址及后缀可省略；主机地址依然需要反着写

##### 15.2.6.8 CNAME别名记录

```
name: 别名的FQDN
value: 真正名字的FQDN
```

#### 15.2.7 子域授权 

每个域的名称服务器，都是通过其上级名称服务器在解析库进行授权,类似根域授权tld

glue record：粘合记录，父域授权子域的记录

#### 15.2.8 互联网域名 

1. 域名注册 

   代理商：万网, 新网, godaddy 

2. 注册完成以后，想自己用专用服务来解析

管理后台：把NS记录指向的服务器名称，和A记录指向的服务器地址

#### 15.2.9 whois

可以从网站查询信息,查询链接

```
https://www.toolnb.com/domaininfo/wangxiaochun.com.html
```

### 15.3 DNS软件 bind 

DNS服务器软件：bind，powerdns，dnsmasq，unbound，coredns 

#### 15.3.1 BIND相关程序包 

yum list all bind* 

* bind：服务器 
* bind-libs：相关库 
* bind-utils: 客户端 
* bind-chroot: 安全包，将dns相关文件放至 /var/named/chroot/

#### 15.3.2 BIND包相关文件

* BIND主程序：/usr/sbin/named 

* 服务脚本和Unit名称：/etc/rc.d/init.d/named，/usr/lib/systemd/system/named.service 

* 主配置文件：/etc/named.conf, /etc/named.rfc1912.zones, /etc/rndc.key 

* 管理工具：/usr/sbin/rndc：remote name domain controller，默认与bind安装在同一主机，且 只能通过127.0.0.1连接named进程，提供辅助性的管理功能；953/tcp 

* 解析库文件：/var/named/ZONE_NAME.ZONE 

  注意：

   (1) 一台物理服务器可同时为多个区域提供解析

   (2) 必须要有根区域文件；named.ca 

  (3) 应该有两个（如果包括ipv6的，应该更多）实现localhost和本地回环地址的解析库

#### 15.3.3 主配置文件 

* 全局配置：options {}; 

* 日志子系统配置：logging {}; 

* 区域定义：本机能够为哪些zone进行解析，就要定义哪些zone 

  zone "ZONE_NAME" IN {}; 

**注意：** 

* 任何服务程序如果期望其能够通过网络被其它主机访问，至少应该监听在一个能与外部主机通信的 IP地址上 
* 缓存名称服务器的配置：监听外部地址即可 
* dnssec: 建议关闭dnssec，设为no

### 15.4 实现主DNS服务器 

#### 15.4.1 主DNS服务器配置 

1. 在主配置文件中定义区域

```bash
vim /etc/named.conf             
#注释掉下面两行
```

```
// listen-on port 53 { 127.0.0.1; };

// allow-query { localhost; };
 zone "ZONE_NAME" IN {
 type {mater|slave|hint|forward};
 file "ZONE_NAME.zone";
 };
```

2. 定义区域解析库文件    

内容包括 :   

- -宏定义   
- -资源记录

#### 15.4.2 主配置文件语法检查

```
named-checkconf  
```

#### 15.4.3 解析库文件语法检查

```
named-checkzone "magedu.org" /var/named/magedu.org.zone
```

#### 15.4.4 配置生效

```bash
#三种方式
#rndc reload 
#systemctl reload named
#service named reload
```

#### 15.4.5 测试和管理工具 

##### 15.4.5.1 dig 命令 

dig只用于测试dns系统，不会查询hosts文件进行解析 

命令格式：

```bash
dig [-t type] name [@SERVER] [query options]
query options：
 +[no]trace：跟踪解析过程 : dig +trace magedu.org
 +[no]recurse：进行递归解析
```

##### 15.4.5.2 host命令 

命令格式：

```
host [-t type] name [SERVER]
```

##### 15.4.5.3 nslookup命令

nslookup 可以支持交互和非交互式两种方式执行 

全令格式：

```
nslookup [-option] [name | -] [server]
```

交互式模式：

```
nslookup>
server IP: 指明使用哪个DNS server进行查询
set q=RR_TYPE: 指明查询的资源记录类型
NAME: 要查询的名称
```

##### 15.4.5.4 rndc 命令

利用rndc工具可以实现管理DNS功能 

rndc 监听端口: 953/tcp 

命令格式:

```
rndc COMMAND
COMMAND:
   status: 查看状态
   reload: 重载主配置文件和区域解析库文件
   reload zonename: 重载区域解析库文件
   retransfer zonename: 手动启动区域传送，而不管序列号是否增加
   notify zonename: 重新对区域传送发通知
   reconfig: 重载主配置文件
   querylog: 开启或关闭查询日志文件/var/log/message
   trace: 递增debug一个级别
   trace LEVEL: 指定使用的级别
   notrace：将调试级别设置为 0
   flush：清空DNS服务器的所有缓存记录
```

#### 15.4.6 实战案例：实现DNS正向主服务器

#### 15.4.7 允许动态更新

动态更新：可以通过远程更新区域数据库的资源记录 

实现动态更新，需要在指定的zone语句块中：

```
动态更新：可以通过远程更新区域数据库的资源记录
实现动态更新，需要在指定的zone语句块中：
```

#### 15.4.8 启用DNS客户端缓存功能 

在高并发的服务器场景中,对DNS的服务器查询性能有较高的要求,如果在客户端启用DNS缓存功能,可以 大幅减轻DNS服务器的压力,同时也能提高DNS客户端名称解析速度

##### 15.4.8.1 CentOS 启用DNS客户端缓存 

CentOS 默认没有启用DNS客户端缓存,安装nscd（Name Service Cache Daemon,名称服务缓存守护进 程）包可以支持DNS缓存功能 

减少DNS服务器压力,提高DNS查询速度

```bash
[root@centos7 ~]#yum -y install nscd
[root@centos7 ~]#systemctl enable --now nscd

#查看缓存统计信息
[root@centos7 ~]#nscd -g
nscd configuration:
...

#清除DNS客户端缓存
[root@centos7 ~]#nscd -i hosts
```

##### 15.4.8.2 Ubuntu 启用DNS客户端缓存 

ubuntu 默认会启用DNS客户端缓存

```bash
[root@ubuntu1804 ~]#systemctl status systemd-resolved.service 
● systemd-resolved.service - Network Name Resolution
   Loaded: loaded (/lib/systemd/system/systemd-resolved.service; enabled; vendor 
preset: enabled)
...

[root@ubuntu1804 ~]#systemd-resolve --help
systemd-resolve [OPTIONS...] HOSTNAME|ADDRESS...
systemd-resolve [OPTIONS...] --service [[NAME] TYPE] DOMAIN

[root@ubuntu1804 ~]#systemd-resolve --statistics
DNSSEC supported by current servers: no
Transactions
Current Transactions: 0
 Total Transactions: 53
 
 #清空缓存
[root@ubuntu1804 ~]#systemd-resolve --flush-caches
[root@ubuntu1804 ~]#systemd-resolve --statistics
DNSSEC supported by current servers: no
Transactions
Current Transactions: 0
 Total Transactions: 53
...
```

### 15.5 实现反向解析区域 

#### 15.5.1 反向解析配置 

反向区域：即将IP反向解析为FQDN 

区域名称：网络地址反写.in-addr.arpa.

示例：

```
172.16.100. --> 100.16.172.in-addr.arpa.
```

(1) 定义区域

```shell
zone "ZONE_NAME" IN {
	type {master|slave|forward}；
	file "网络地址.zone"
};
```

(2) 定义区域解析库文件 

注意：不需要MX,以PTR记录为主

#### 15.5.2 实战案例: 反向解析

### 15.6 实现从服务器

只有一台主DNS服务器，存在单点失败的问题，可以建立主DNS服务器的备份服务器，即从服务器来实 现DNS服务的容错机制。从服务器可以自动和主服务器进行单向的数据同步，从而和主DNS服务器一 样，也可以对外提供查询服务，但从服务器不提供数据更新服务。

#### 15.6.1 DNS从服务器 

1. 应该为一台独立的名称服务器 
1. 主服务器的区域解析库文件中必须有一条NS记录指向从服务器 
1. 从服务器只需要定义区域，而无须提供解析库文件；解析库文件应该放置于/var/named/slaves/目 录中 
1. 主服务器得允许从服务器作区域传送 
1. 主从服务器时间应该同步，可通过ntp进行 
1. bind程序的版本应该保持一致；否则，应该从高，主低 

#### 15.6.2 定义从区域

格式：

```shell
zone "ZONE_NAME" IN {
	type slave;
	masters { MASTER_IP; };
	file "slaves/ZONE_NAME.zone";
};
```

#### 15.6.3 实战案例：实现DNS从服务器

### 15.7 实现子域 

#### 15.7.1 子域委派授权 

将子域委派给其它主机管理，实现分布式DNS数据库 

正向解析区域子域方法

#### 15.7.2 范例：实现DNS父域和子域服务

### 15.8 实现DNS转发（缓存）服务器 

#### 15.8.1 DNS转发 

利用DNS转发，可以将用户的DNS请求，转发至指定的DNS服务，而非默认的根DNS服务器，并将指定 服务器查询的返回结果进行缓存，提高效率。 

**注意**： 

1. 被转发的服务器需要能够为请求者做递归，否则转发请求不予进行 

2. 在/etc/named.conf的全局配置块中，关闭dnssec功能

   ```bash
   dnssec-enable no;
   dnssec-validation no;
   ```

#### 15.8.2 转发方式 

##### 15.8.2.1 全局转发 

对非本机所负责解析区域的请求，全转发给指定的服务器 

在全局配置块中实现：

```bash
Options {
	forward first|only;
	forwarders { ip;};
};
```

##### 15.8.2.2 特定区域转发 

仅转发对特定的区域的请求，比全局转发优先级高

```bash
zone "ZONE_NAME" IN {
	type forward;
	forward first|only;
	forwarders { ip;};
};
```

**first**：先转发至指定DNS服务器，如果无法解析查询请求，则本服务器再去根服务器查询 

**only**: 先转发至指定DNS服务器，如果无法解析查询请求，则本服务器将不再去根服务器查询

#### 15.8.3 实战案例：实现DNS forward（缓存）服务器

### 15.9 实现智能DNS

#### 15.9.1 GSLB 

GSLB：Global Server Load Balance全局负载均衡 

GSLB是对服务器和链路进行综合判断来决定由哪个地点的服务器来提供服务，实现异地服务器群服务质 量的保证 

GSLB主要的目的是在整个网络范围内将用户的请求定向到最近的节点（或者区域） 

GSLB分为基于DNS实现、基于重定向实现、基于路由协议实现，其中最通用的是基于DNS解析方式

#### 15.9.2 CDN （Content Delivery Network）内容分发网络

##### 15.9.2.1 CDN工作原理 

1. 用户向浏览器输入www.a.com这个域名，浏览器第一次发现本地没有dns缓存，则向网站的DNS服 务器请求 
2. 网站的DNS域名解析器设置了CNAME，指向了www.a.tbcdn.com,请求指向了CDN网络中的智能 DNS负载均衡系统 
3. 智能DNS负载均衡系统解析域名，把对用户响应速度最快的IP节点返回给用户； 
4. 用户向该IP节点（CDN服务器）发出请求 
5. 由于是第一次访问，CDN服务器会通过Cache内部专用DNS解析得到此域名的原web站点IP，向原 站点服务器发起请求，并在CDN服务器上缓存内容 
6. 请求结果发给用户

##### 15.9.2.2 CDN服务商 

* 服务商：阿里，腾讯，蓝汛，网宿，帝联等 
* 智能DNS: dnspod dns.la

#### 15.9.3 智能DNS相关技术 

##### 15.9.3.1 bind中ACL 

ACL：把一个或多个地址归并为一个集合，并通过一个统一的名称调用 

注意：只能先定义后使用；因此一般定义在配置文件中，处于options的前面 

格式：

```bash
acl acl_name {
	ip;
	net/prelen;
	……
};
```

##### 15.9.3.2 bind有四个内置的acl 

* none 没有一个主机 
* any 任意主机 
* localhost 本机 
* localnet 本机的IP同掩码运算后得到的网络地址

##### 15.9.3.3 访问控制的指令 

* allow-query {}： 允许查询的主机；白名单 
* allow-transfer {}：允许区域传送的主机；白名单 
* allow-recursion {}: 允许递归的主机,建议全局使用 
* allow-update {}: 允许更新区域数据库中的内容

##### 15.9.3.4 view 视图 

###### 15.9.3.4.1 View：视图，将ACL和区域数据库实现对应关系，以实现智能DNS 

* 一个bind服务器可定义多个view，每个view中可定义一个或多个zone 
* 每个view用来匹配一组客户端 
* 多个view内可能需要对同一个区域进行解析，但使用不同的区域解析库文件 

注意： 

* 一旦启用了view，所有的zone都只能定义在view中 
* 仅在允许递归请求的客户端所在view中定义根区域 
* 客户端请求到达时，是自上而下检查每个view所服务的客户端列表

###### 15.9.3.4.2 view 格式

```shell
view VIEW_NAME {
 match-clients { beijingnet; };
 zone "magedu.org" {
 type master;
 file "magedu.org.zone.bj"; 
 };
 include "/etc/named.rfc1912.zones";
};
view VIEW_NAME {
 match-clients { shanghainet; };
 zone "magedu.org" {
 type master;
 file "magedu.org.zone.sh"; 
 };
 include "/etc/named.rfc1912.zones";
};
```

#### 15.9.4 实战案例：利用view实现智能DNS

### 15.10 DNS排错

* SERVFAIL：The nameserver encountered a problem while processing the query.  可使用dig +trace排错，可能是网络和防火墙导致 
* NXDOMAIN：The queried name does not exist in the zone. 可能是CNAME对应的A记录不存在导致 
* REFUSED：The nameserver refused the client's DNS request due to policy restrictions. 可能是DNS策略导致

### 15.11 实战案例：综合案例，实现 Internet 的 DNS 服务架构

# 16 Linux 防火墙 

### 内容概述

* 防火墙的概念 
* iptables的基本认识 
* iptables的组成 
* iptables的基本语法 
* iptables之forward的概念 
* iptables之地址转换法则 
* SNAT源地址转换的具体实现 
* DNAT目标地址转换的具体实现 
* firewalld介绍和基本配置 
* 最新防火墙技术 nft介绍和基本用法

### 16.1 安全技术和防火墙 

#### 16.1.1 安全技术 

* 入侵检测系统（Intrusion Detection Systems）：特点是不阻断任何网络访问，量化、定位来自内 外网络的威胁情况，主要以提供报警和事后监督为主，提供有针对性的指导措施和安全决策依据,类 似于监控系统一般采用旁路部署方式 
* 入侵防御系统（Intrusion Prevention System）：以透明模式工作，分析数据包的内容如：溢出攻 击、拒绝服务攻击、木马、蠕虫、系统漏洞等进行准确的分析判断，在判定为攻击行为后立即予以 阻断，主动而有效的保护网络的安全，一般采用在线部署方式 
* 防火墙（ FireWall ）：隔离功能，工作在网络或主机边缘，对进出网络或主机的数据包基于一定的 规则检查，并在匹配某规则时由规则定义的行为进行处理的一组功能的组件，基本上的实现都是默 认情况下关闭所有的通过型访问，只开放允许访问的策略,会将希望外网访问的主机放在DMZ  (demilitarized zone)网络中.

```
防水墙
广泛意义上的防水墙：防水墙（Waterwall），与防火墙相对，是一种防止内部信息泄漏的安全产品。网络、外设接口、存储介质和打印机构成信息泄漏的全部途径。防水墙针对这四种泄密途径，在事前、事中、事后进行全面防护。其与防病毒产品、外部安全产品一起构成完整的网络安全体系。
```

#### 16.1.2 防火墙的分类

按保护范围划分： 

* 主机防火墙：服务范围为当前一台主机 
* 网络防火墙：服务范围为防火墙一侧的局域网 

按实现方式划分: 

* 硬件防火墙：在专用硬件级别实现部分功能的防火墙；另一个部分功能基于软件实现，如：华为， 山石hillstone,天融信，启明星辰，绿盟，深信服, PaloAlto , fortinet, Cisco, Checkpoint， NetScreen(Juniper2004年40亿美元收购)等 
* 软件防火墙：运行于通用硬件平台之上的防火墙的应用软件，Windows 防火墙 ISA --> Forefront  TMG 

按网络协议划分： 

* 网络层防火墙：OSI模型下四层，又称为包过滤防火墙
* 应用层防火墙/代理服务器：proxy 代理网关，OSI模型七层

**包过滤防火墙**

网络层对数据包进行选择，选择的依据是系统内设置的过滤逻辑，被称为访问控制列表（ACL），通过 检查数据流中每个数据的源地址，目的地址，所用端口号和协议状态等因素，或他们的组合来确定是否 允许该数据包通过 

优点：对用户来说透明，处理速度快且易于维护 

缺点：无法检查应用层数据，如病毒等

**应用层防火墙**

应用层防火墙/代理服务型防火墙，也称为代理服务器（Proxy Server)  将所有跨越防火墙的网络通信链路分为两段 内外网用户的访问都是通过代理服务器上的“链接”来实现 

优点：在应用层对数据进行检查，比较安全 

缺点：增加防火墙的负载 

提示：现实生产环境中所使用的防火墙一般都是二者结合体，即先检查网络数据，通过之后再送到应用 层去检查

#### 16.1.3 网络架构

### 16.2 Linux 防火墙的基本认识 

#### 16.2.1 Netfilter

Linux防火墙是由Netfilter组件提供的，Netfilter工作在内核空间，集成在linux内核中 

Netfilter 是Linux 2.4.x之后新一代的Linux防火墙机制，是linux内核的一个子系统。Netfilter采用模块 化设计，具有良好的可扩充性，提供扩展各种网络服务的结构化底层框架。Netfilter与IP协议栈是无缝 契合，并允许对数据报进行过滤、地址转换、处理等操作 

Netfilter官网文档：https://netfilter.org/documentation/

#### 16.2.2 防火墙工具介绍 

##### 16.2.2.1 iptables 

由软件包iptables提供的命令行工具，工作在用户空间，用来编写规则，写好的规则被送往netfilter，告 诉内核如何去处理信息包

##### 16.2.2.2 firewalld 

从CentOS 7 版开始引入了新的前端管理工具 

软件包： 

* firewalld 
* firewalld-config 

管理工具： 

* firewall-cmd 命令行工具 
* firewall-config 图形工作

##### 16.2.2.3 nftables 

此软件是CentOS 8 新特性,Nftables最初在法国巴黎的Netfilter Workshop 2008上发表，然后由长期的 netfilter核心团队成员和项目负责人Patrick McHardy于2009年3月发布。它在2013年末合并到Linux内 核中，自2014年以来已在内核3.13中可用。

它重用了netfilter框架的许多部分，例如连接跟踪和NAT功能。它还保留了命名法和基本iptables设计的 几个部分，例如表，链和规则。就像iptables一样，表充当链的容器，并且链包含单独的规则，这些规 则可以执行操作，例如丢弃数据包，移至下一个规则或跳至新链。 

从用户的角度来看，nftables添加了一个名为nft的新工具，该工具替代了iptables，arptables和 ebtables中的所有其他工具。从体系结构的角度来看，它还替换了内核中处理数据包过滤规则集运行时 评估的那些部分。

#### 16.2.3 netfilter 中五个勾子函数和报文流向

Netfilter在内核中选取五个位置放了五个hook(勾子) function(INPUT、OUTPUT、FORWARD、 PREROUTING、POSTROUTING)，而这五个hook function向用户开放，用户可以通过一个命令工具 （iptables）向其写入规则 由信息过滤表（table）组成，包含控制IP包处理的规则集（rules），规则被分组放在链（chain）上

提示：从 Linux kernel 4.2 版以后，Netfilter 在prerouting 前加了一个 ingress 勾子函数。可以使用这 个新的入口挂钩来过滤来自第2层的流量，这个新挂钩比预路由要早，基本上是 tc 命令（流量控制工 具）的替代品

**三种报文流向** 

* 流入本机：PREROUTING --> INPUT-->用户空间进程 
* 流出本机：用户空间进程 -->OUTPUT--> POSTROUTING 
* 转发：PREROUTING --> FORWARD --> POSTROUTING

#### 16.2.4 iptables的组成 

iptables由五个表table和五个链chain以及一些规则组成

**链 chain：** 

* 内置链：每个内置链对应于一个钩子函数 
* 自定义链：用于对内置链进行扩展或补充，可实现更灵活的规则组织管理机制；只有Hook钩子调 用自定义链时，才生效 

**五个内置链chain:**

```
INPUT,OUTPUT,FORWARD,PREROUTING,POSTROUTING
```

**五个表table**：filter、nat、mangle、raw、security 

* filter：过滤规则表，根据预定义的规则过滤符合条件的数据包,默认表 
* nat：network address translation 地址转换规则表 
* mangle：修改数据标记位规则表 
* raw：关闭启用的连接跟踪机制，加快封包穿越防火墙速度 
* security：用于强制访问控制（MAC）网络规则，由Linux安全模块（如SELinux）实现

**优先级由高到低的顺序为：**

```
security -->raw-->mangle-->nat-->filter
```

**表和链对应关系**

**数据包过滤匹配流程**

**内核中数据包的传输过程** 

* 当一个数据包进入网卡时，数据包首先进入PREROUTING链，内核根据数据包目的IP判断是否需要 转送出去 
* 如果数据包是进入本机的，数据包就会沿着图向下移动，到达INPUT链。数据包到达INPUT链后， 任何进程都会收到它。本机上运行的程序可以发送数据包，这些数据包经过OUTPUT链，然后到达 POSTROUTING链输出 
* 如果数据包是要转发出去的，且内核允许转发，数据包就会向右移动，经过FORWARD链，然后到 达POSTROUTING链输出

#### 16.2.5 netfilter 完整流程

### 16.3 iptables 

#### 16.3.1 iptables 规则说明 

##### 16.3.1.1 iptables 规则组成 

* 规则rule：根据规则的匹配条件尝试匹配报文，对匹配成功的报文根据规则定义的处理动作作出处理， 规则在链接上的次序即为其检查时的生效次序 

* 匹配条件：默认为与条件，同时满足 
* 基本匹配：IP，端口，TCP的Flags（SYN,ACK等） 
* 扩展匹配：通过复杂高级功能匹配 
* 处理动作：称为target，跳转目标
  * 内建处理动作：ACCEPT,DROP,REJECT,SNAT,DNAT,MASQUERADE,MARK,LOG... 
  * 自定义处理动作：自定义chain，利用分类管理复杂情形

规则要添加在链上，才生效；添加在自定义链上不会自动生效 

白名单:只有指定的特定主机可以访问,其它全拒绝 

黑名单:只有指定的特定主机拒绝访问,其它全允许,默认方式

##### 16.3.1.2 iptables规则添加时考量点 

* 要实现哪种功能：判断添加在哪张表上 
* 报文流经的路径：判断添加在哪个链上 
* 报文的流向：判断源和目的 
* 匹配规则：业务需要

##### 16.3.1.3 本章学习环境准备

Centos 7,8

```bash
systemctl stop firewalld.service 
systemctl disable firewalld. service

#或者
systemctl disable --now firewalld. service
```

Centos 6

```bash
service iptables stop
chkconfig iptables off
```

#### 16.3.2 iptables 用法说明 

帮助：man 8 iptables

格式：

```bash
iptables [-t table] {-A|-C|-D} chain rule-specification
iptables [-t table] -I chain [rulenum] rule-specification
iptables [-t table] -R chain rulenum rule-specification
iptables [-t table] -D chain rulenum
iptables [-t table] -S [chain [rulenum]]
iptables [-t table] {-F|-L|-Z} [chain [rulenum]] [options...]
iptables [-t table] -N chain
iptables [-t table] -X [chain]
iptables [-t table] -P chain target
iptables [-t table] -E old-chain-name new-chain-name
rule-specification = [matches...] [target]
match = -m matchname [per-match-options]
target = -j targetname [per-target-options]
```

iptables命令格式详解：

```bash
iptables   [-t table]   SUBCOMMAND   chain   [-m matchname [per-match-options]]   -j targetname [per-target-options]
```

1、-t table：指定表 

raw, mangle, nat, [filter]默认 

2、SUBCOMMAND：子命令 

**链管理类**：

```
-N：new, 自定义一条新的规则链
-E：重命名自定义链；引用计数不为0的自定义链不能够被重命名，也不能被删除
-X：delete，删除自定义的空的规则链
-P：Policy，设置默认策略；对filter表中的链而言，其默认策略有：ACCEPT：接受, DROP：丢弃
```

**查看类：**

```
-L：list, 列出指定鏈上的所有规则，本选项须置后
-n：numberic，以数字格式显示地址和端口号
-v：verbose，详细信息
-vv 更详细
-x：exactly，显示计数器结果的精确值,而非单位转换后的易读值
--line-numbers：显示规则的序号
-S selected,以iptables-save 命令格式显示链上规则
```

**常用组合：**

```
-vnL 
-vvnxL --line-numbers
```

**规则管理类：**

```
-A：append，追加
-I：insert, 插入，要指明插入至的规则编号，默认为第一条
-D：delete，删除
   (1) 指明规则序号
   (2) 指明规则本身
-R：replace，替换指定链上的指定规则编号
-F：flush，清空指定的规则链
-Z：zero，置零
   iptables的每条规则都有两个计数器
   (1) 匹配到的报文的个数
   (2) 匹配到的所有报文的大小之和
```

3、chain： 

PREROUTING，INPUT，FORWARD，OUTPUT，POSTROUTING 

4、匹配条件 

* 基本：通用的，PARAMETERS 
* 扩展：需加载模块，MATCH EXTENTIONS 

5、处理动作：

```
-j targetname [per-target-options]
```

简单动作：

```
ACCEPT
DROP
```

扩展动作：

```
REJECT：--reject-with:icmp-port-unreachable默认
RETURN：返回调用链
REDIRECT：端口重定向
LOG：记录日志，dmesg
MARK：做防火墙标记
DNAT：目标地址转换
SNAT：源地址转换
MASQUERADE：地址伪装
自定义链
```

#### 16.3.3 iptables 基本匹配条件 

基本匹配条件：无需加载模块，由iptables/netfilter自行提供

```
[!] -s, --source address[/mask][,...]：源IP地址或者不连续的IP地址
[!] -d, --destination address[/mask][,...]：目标IP地址或者不连续的IP地址
[!] -p, --protocol protocol：指定协议，可使用数字如0（all）
	protocol: tcp, udp, icmp, icmpv6, udplite,esp, ah, sctp, mh or“all“  
	参看：/etc/protocols
[!] -i, --in-interface name：报文流入的接口；只能应用于数据报文流入环节，只应用于INPUT、FORWARD、PREROUTING链
[!] -o, --out-interface name：报文流出的接口；只能应用于数据报文流出的环节，只应用于FORWARD、OUTPUT、POSTROUTING链
```

#### 16.3.4 iptables 扩展匹配条件

扩展匹配条件：需要加载扩展模块（/usr/lib64/xtables/*.so），方可生效 

扩展模块的查看帮助 ：man iptables-extensions 

扩展匹配条件：

* 隐式扩展 
* 显式扩展

##### 16.3.4.1 隐式扩展 

iptables 在使用-p选项指明了特定的协议时，无需再用-m选项指明扩展模块的扩展机制，不需要手动加 载扩展模块 

**tcp 协议的扩展选项**

```
[!] --source-port, --sport port[:port]：匹配报文源端口,可为端口连续范围
[!] --destination-port,--dport port[:port]：匹配报文目标端口,可为连续范围
[!] --tcp-flags mask comp
     mask 需检查的标志位列表，用,分隔 , 例如 SYN,ACK,FIN,RST
     comp 在mask列表中必须为1的标志位列表，无指定则必须为0，用,分隔tcp协议的扩展选项
```

[!] --syn：用于匹配第一次握手, 相当于：--tcp-flags SYN,ACK,FIN,RST SYN

**udp 协议的扩展选项**

```
[!] --source-port, --sport port[:port]：匹配报文的源端口或端口范围
[!] --destination-port,--dport port[:port]：匹配报文的目标端口或端口范围
```

**icmp 协议的扩展选项**

```
[!] --icmp-type {type[/code]|typename}
	type/code
	0/0   echo-reply icmp应答
	8/0   echo-request icmp请求 
```

##### 16.3.4.2 显式扩展及相关模块 

显示扩展即必须使用-m选项指明要调用的扩展模块名称，需要手动加载扩展模块

```
[-m matchname [per-match-options]]
```

**扩展模块的使用帮助**： 

* CentOS 7,8: man iptables-extensions  
* CentOS 6: man iptables

###### 16.3.4.2.1 multiport扩展

以离散方式定义多端口匹配,最多指定15个端口

```
#指定多个源端口
[!] --source-ports,--sports port[,port|,port:port]...

# 指定多个目标端口
[!] --destination-ports,--dports port[,port|,port:port]...

#多个源或目标端
[!] --ports port[,port|,port:port]...
```

###### 16.3.4.2.2 iprange扩展 

指明连续的（但一般不是整个网络）ip地址范围

```
[!] --src-range from[-to] 源IP地址范围
[!] --dst-range from[-to] 目标IP地址范围
```

###### 16.3.4.2.3 mac扩展 

mac 模块可以指明源MAC地址,，适用于：PREROUTING, FORWARD，INPUT chains

```
[!] --mac-source XX:XX:XX:XX:XX:XX
```

###### 16.3.4.2.4 string扩展 

对报文中的应用层数据做字符串模式匹配检测

```
--algo {bm|kmp} 字符串匹配检测算法
	bm：Boyer-Moore
	kmp：Knuth-Pratt-Morris
--from offset 开始偏移
--to offset   结束偏移
[!] --string pattern 要检测的字符串模式
[!] --hex-string pattern要检测字符串模式，16进制格式
```

###### 16.3.4.2.5 time扩展 

**注意：CentOS 8 此模块有问题** 

根据将报文到达的时间与指定的时间范围进行匹配

```
--datestart YYYY[-MM[-DD[Thh[:mm[:ss]]]]] 日期
--datestop YYYY[-MM[-DD[Thh[:mm[:ss]]]]]
--timestart hh:mm[:ss]       时间
--timestop hh:mm[:ss]
[!] --monthdays day[,day...]   每个月的几号
[!] --weekdays day[,day...]   星期几，1 – 7 分别表示星期一到星期日
--kerneltz：内核时区（当地时间），不建议使用，CentOS 7版本以上系统默认为 UTC
注意： centos6 不支持kerneltz ，--localtz指定本地时区(默认)
```

###### 16.3.4.2.6 connlimit扩展 

根据每客户端IP做并发连接数数量匹配 

可防止Dos(Denial of Service，拒绝服务)攻击

```
--connlimit-upto N #连接的数量小于等于N时匹配
--connlimit-above N #连接的数量大于N时匹配
```

###### 16.3.4.2.7 limit扩展 

基于收发报文的速率做匹配 , 令牌桶过滤器

```
--limit-burst number #前多少个包不限制
--limit #[/second|/minute|/hour|/day]
```

###### 16.3.4.2.8 state扩展 

state 扩展模块，可以根据”连接追踪机制“去检查连接的状态，较耗资源 

conntrack机制：追踪本机上的请求和响应之间的关系

**状态类型：** 

* NEW：新发出请求；连接追踪信息库中不存在此连接的相关信息条目，因此，将其识别为第一次发 出的请求 

* ESTABLISHED：NEW状态之后，连接追踪信息库中为其建立的条目失效之前期间内所进行的通信 状态 

* RELATED：新发起的但与已有连接相关联的连接，如：ftp协议中的数据连接与命令连接之间的关 系 

* INVALID：无效的连接，如flag标记不正确 
* UNTRACKED：未进行追踪的连接，如：raw表中关闭追踪

已经追踪到的并记录下来的连接信息库

```bash
[root@centos8 ~]#cat /proc/net/nf_conntrack
ipv4     2 tcp      6 431325 ESTABLISHED src=10.0.0.7 dst=10.0.0.8 sport=49900
dport=80 src=10.0.0.8 dst=10.0.0.7 sport=80 dport=49900 [ASSURED] mark=0 zone=0
use=2
ipv4     2 tcp      6 431325 ESTABLISHED src=10.0.0.7 dst=10.0.0.8 sport=49886
dport=80 src=10.0.0.8 dst=10.0.0.7 sport=80 dport=49886 [ASSURED] mark=0 zone=0
use=2
ipv4     2 tcp      6 431325 ESTABLISHED src=10.0.0.7 dst=10.0.0.8 sport=49892
dport=80 src=10.0.0.8 dst=10.0.0.7 sport=80 dport=49892 [ASSURED] mark=0 zone=0
use=2
...
```

调整连接追踪功能所能够容纳的最大连接数量

```bash
[root@centos8 ~]#cat /proc/sys/net/netfilter/nf_conntrack_max
26624
[root@centos8 ~]#cat /proc/sys/net/nf_conntrack_max
26624
```

查看连接跟踪有多少条目

```bash
[root@centos8 ~]#cat /proc/sys/net/netfilter/nf_conntrack_count
10
```

不同的协议的连接追踪时长

```bash
[root@centos8 ~]#ll /proc/sys/net/netfilter/
total 0
-rw-r--r-- 1 root root 0 Mar 19 18:14 nf_conntrack_acct
-rw-r--r-- 1 root root 0 Mar 19 18:14 nf_conntrack_buckets
-rw-r--r-- 1 root root 0 Mar 19 18:14 nf_conntrack_checksum
-r--r--r-- 1 root root 0 Mar 19 18:14 nf_conntrack_count
...
```

说明： 

* 连接跟踪，需要加载模块： modprobe nf_conntrack_ipv4 
* 当服务器连接多于最大连接数时dmesg 可以观察到 ：kernel: ip_conntrack: table full, dropping  packet错误,并且导致建立TCP连接很慢。 
* 各种状态的超时后，链接会从表中删除

连接过多的解决方法两个： 

(1) 加大nf_conntrack_max 值

```
vi /etc/sysctl.conf
net.nf_conntrack_max = 393216
net.netfilter.nf_conntrack_max = 393216 
```

(2) 降低 nf_conntrack timeout时间

```
vi /etc/sysctl.conf
net.netfilter.nf_conntrack_tcp_timeout_established = 300
net.netfilter.nf_conntrack_tcp_timeout_time_wait = 120
net.netfilter.nf_conntrack_tcp_timeout_close_wait = 60
net.netfilter.nf_conntrack_tcp_timeout_fin_wait = 120
iptables -t nat -L -n
```

格式：

```
[!] --state state
```

**案例：开放被动模式的ftp服务** 

**CentOS 8 此模块有bug** 

(1) 装载ftp连接追踪的专用模块： 

​	跟踪模块路径： /lib/modules/kernelversion/kernel/net/netfilter

```
vim /etc/sysconfig/iptables-config 
IPTABLES_MODULES=“nf_conntrack_ftp"
modprobe nf_conntrack_ftp
```

(2) 放行请求报文： 

​	命令连接：NEW, ESTABLISHED 

​	数据连接：RELATED, ESTABLISHED

```bash
iptables -I INPUT -d LocalIP -p tcp -m state --state ESTABLISHED,RELATED -j ACCEPT 
iptables -A INPUT -d LocalIP -p tcp --dport 21 -m state --state NEW -j ACCEPT
```

(3) 放行响应报文：

```bash
iptables -I OUTPUT -s LocalIP -p tcp -m state --state ESTABLISHED -j ACCEPT 
```

#### 16.3.5 Target 

target 包括以下类型：

```bash
自定义链, ACCEPT， DROP， REJECT，RETURN,LOG，SNAT，DNAT，REDIRECT，MASQUERADE

LOG：非中断target,本身不拒绝和允许,放在拒绝和允许规则前，并将日志记录在/var/log/messages系
统日志中
--log-level level   级别： debug，info，notice, warning, error, crit, alert,emerg
--log-prefix prefix 日志前缀，用于区别不同的日志，最多29个字符
```

#### 16.3.6 规则优化最佳实践 

1. 安全放行所有入站和出站的状态为ESTABLISHED状态连接,建议放在第一条，效率更高 

2. 谨慎放行入站的新请求 

3. 有特殊目的限制访问功能，要在放行规则之前加以拒绝 

4. 同类规则（访问同一应用，比如：http ），匹配范围小的放在前面，用于特殊处理 

5. 不同类的规则（访问不同应用，一个是http，另一个是mysql ），匹配范围大的放在前面，效率更 高

   ```
   -s 10.0.0.6 -p tcp --dport 3306 -j REJECT
   -s 172.16.0.0/16 -p tcp --dport 80 -j REJECT
   ```

6. 应该将那些可由一条规则能够描述的多个规则合并为一条,减少规则数量,提高检查效率 

7. 设置默认策略，建议白名单（只放行特定连接） 

   * iptables -P，不建议，容易出现“自杀现象” 
   * 规则的最后定义规则做为默认策略，推荐使用，放在最后一条

#### 16.3.7 iptables规则保存 

使用iptables命令定义的规则，手动删除之前，其生效期限为kernel存活期限 

**持久保存规则：** 

CentOS 7,8

```
iptables-save > /PATH/TO/SOME_RULES_FILE
```

CentOS 6 

```
#将规则覆盖保存至/etc/sysconfig/iptables文件中
service iptables save 
```

**加载规则** 

CentOS 7,8 重新载入预存规则文件中规则： 

```
iptables-restore < /PATH/FROM/SOME_RULES_FILE
```

iptables-restore选项

```
-n, --noflush：不清除原有规则
-t, --test：仅分析生成规则集，但不提交
```

-n, --noflush：不清除原有规则 -t, --test：仅分析生成规则集，但不提交

CentOS 6：

```
#会自动从/etc/sysconfig/iptables 重新载入规则
service iptables  restart
```

**开机自动重载规则** 

* 用脚本保存各个iptables命令；让此脚本开机后自动运行 

  /etc/rc.d/rc.local文件中添加脚本路径 /PATH/TO/SOME_SCRIPT_FILE 

* 用规则文件保存各个规则，开机时自动载入此规则文件中的规则 

  在/etc/rc.d/rc.local文件添加

```
iptables-restore < /PATH/FROM/IPTABLES_RULES_FILE
```

* `定义Unit File, CentOS 7，8 可以安装 iptables-services 实现iptables.service

#### 16.3.8 网络防火墙 

iptables/netfilter 利用filter表的FORWARD链,可以充当网络防火墙： 

注意的问题：

(1) 请求-响应报文均会经由FORWARD链，要注意规则的方向性 

(2) 如果要启用conntrack机制，建议将双方向的状态为ESTABLISHED的报文直接放行

##### 16.3.8.1 FORWARD 链实现内外网络的流量控制

##### 16.3.8.2 NAT 表

NAT: network address translation，支持PREROUTING，INPUT，OUTPUT，POSTROUTING四个链 

请求报文：修改源/目标IP，由定义如何修改 

响应报文：修改源/目标IP，根据跟踪机制自动实现 

NAT的实现分为下面类型： 

* SNAT：source NAT ，支持POSTROUTING, INPUT，让本地网络中的主机通过某一特定地址访问 外部网络，实现地址伪装,请求报文：修改源IP 
* DNAT：destination NAT 支持PREROUTING , OUTPUT，把本地网络中的主机上的某服务开放给外 部网络访问(发布服务和端口映射)，但隐藏真实IP,请求报文：修改目标IP 
* PNAT: port nat，端口和IP都进行修改

##### 16.3.8.3 SNAT 

SNAT：基于nat表的target，适用于固定的公网IP 

SNAT选项： 

* --to-source [ipaddr[-ipaddr]][:port[-port]]

* --random

```bash
iptables -t nat -A POSTROUTING -s LocalNET ! -d LocalNet -j SNAT --to-source ExtIP
```

**注意: 需要开启 ip_forward**

MASQUERADE：基于nat表的target，适用于动态的公网IP，如：拨号网络 

MASQUERADE选项：

* --to-ports port[-port] 
* --random

```bash
iptables -t nat -A POSTROUTING -s LocalNET ! -d LocalNet -j MASQUERADE
```

##### 16.3.8.4 DNAT 

DNAT：nat表的target，适用于端口映射，即可重定向到本机，也可以支持重定向至不同主机的不同端 口，但不支持多目标，即不支持负载均衡功能 

DNAT选项： 

```
--to-destination [ipaddr[-ipaddr]][:port[-port]]
```

DNAT 格式:

```
iptables -t nat -A PREROUTING -d ExtIP -p tcp|udp --dport PORT -j DNAT --todestination InterSeverIP[:PORT]
```

**注意: 需要开启 ip_forward**

##### 16.3.8.5 REDIRECT 转发 

REDIRECT，是NAT表的 target，通过改变目标IP和端口，将接受的包转发至同一个主机的不同端口，可 用于PREROUTING OUTPUT链 

REDIRECT选项：

```
--to-ports port[-port]
```

**注意: 无需开启 ip_forward**

##### 16.3.8.6 综合案例: 两个私有网络的互相通迅

#### 16.3.9 实战案例：马哥教育原校区的防火墙配置设置

### 16.4 firewalld服务 

#### 16.4.1 firewalld 介绍 

firewalld是CentOS 7.0新推出的管理netfilter的用户空间软件工具,也被ubuntu18.04版以上所支持(apt  install firewalld安装即可) 

firewalld是配置和监控防火墙规则的系统守护进程。可以实iptables,ip6tables,ebtables的功能 

firewalld服务由firewalld包提供 

firewalld支持划分区域zone,每个zone可以设置独立的防火墙规则 

**归入zone顺序：** 

* 先根据数据包中源地址，将其纳为某个zone 
* 纳为网络接口所属zone 
* 纳入默认zone，默认为public zone,管理员可以改为其它zone 
* 网卡默认属于public zone,lo网络接口属于trusted zone 

**firewalld zone 分类**

| zone名称 | 默认配置                                                     |
| :------: | :----------------------------------------------------------- |
| trusted  | 允许所有流量                                                 |
|   home   | 拒绝除和传出流量相关的，以及ssh,mdsn,ipp-client,samba-client,dhcpv6-client预 定义服务之外其它所有传入流量 |
| internal | 和home相同                                                   |
|   work   | 拒绝除和传出流量相关的，以及ssh,ipp-client,dhcpv6-client预定义服务之外的其它 所有传入流量 |
|  public  | 拒绝除和传出流量相关的，以及ssh,dhcpv6-client预定义服务之外的其它所有传入流 量，新加的网卡默认属于public zone |
| external | 拒绝除和传出流量相关的，以及ssh预定义服务之外的其它所有传入流量，属于 external zone的传出ipv4流量的源地址将被伪装为传出网卡的地址。 |
|   dmz    | 拒绝除和传出流量相关的，以及ssh预定义服务之外的其它所有传入流量 |
|  block   | 拒绝除和传出流量相关的所有传入流量                           |
|   drop   | 拒绝除和传出流量相关的所有传入流量（甚至不以ICMP错误进行回应） |

**预定义服务**

| 服务名称      | 配置                                                         |
| ------------- | ------------------------------------------------------------ |
| ssh           | Local SSH server. Traffic to 22/tcp                          |
| dhcpv6-client | Local DHCPv6 client. Traffic to 546/udp on the fe80::/64 IPv6 network |
| ipp-client    | Local IPP printing. Traffic to 631/udp.                      |
| samba-client  | Local IPP printing. Traffic to 631/udp.                      |
| mdns          | Multicast DNS (mDNS) local-link name resolution. Traffic to 5353/udp to the 224.0.0.251 (IPv4) or ff02::fb (IPv6) multicast addresses. |

firewalld预定义服务配置 

* firewall-cmd --get-services 查看预定义服务列表 
* /usr/lib/firewalld/services/*.xml预定义服务的配置 

firewalld 三种配置方法 

* firewall-config 图形工具: 需安装 firewall-config包 
* firewall-cmd 命令行工具: firewalld包,默认安装 
* /etc/firewalld/ 配置文件，一般不建议,如:/etc/firewalld/zones/public.xml 

#### 16.4.2 firewall-cmd 命令 

firewall-cmd 格式

```
Usage: firewall-cmd [OPTIONS...]
```

常见选项：

```bash
--get-zones 列出所有可用区域
--get-default-zone 查询默认区域
--set-default-zone=<ZONE> 设置默认区域
--get-active-zones 列出当前正使用的区域
--add-source=<CIDR>[--zone=<ZONE>] 添加源地址的流量到指定区域，如果无--zone= 选项，使用默认区域
--remove-source=<CIDR> [--zone=<ZONE>] 从指定区域删除源地址的流量，如无--zone= 选项，使用默认区域
--add-interface=<INTERFACE>[--zone=<ZONE>] 添加来自于指定接口的流量到特定区域，如果无--zone= 选项，使用默认区域
--change-interface=<INTERFACE>[--zone=<ZONE>] 改变指定接口至新的区域，如果无--zone=选项，使用默认区域
--add-service=<SERVICE> [--zone=<ZONE>] 允许服务的流量通过，如果无--zone= 选项，使用默认区域
--add-port=<PORT/PROTOCOL>[--zone=<ZONE>] 允许指定端口和协议的流量，如果无--zone= 选项，使用默认区域
--remove-service=<SERVICE> [--zone=<ZONE>] 从区域中删除指定服务，禁止该服务流量，如果无--zone= 选项，使用默认区域
--remove-port=<PORT/PROTOCOL>[--zone=<ZONE>] 从区域中删除指定端口和协议，禁止该端口的流量，如果无--zone= 选项，使用默认区域
--reload 删除当前运行时配置，应用加载永久配置
--list-services 查看开放的服务
--list-ports   查看开放的端口
--list-all [--zone=<ZONE>] 列出指定区域的所有配置信息，包括接口，源地址，端口，服务等，如果无--zone= 选项，使用默认区域
```

#### 16.4.3 其它规则 

当基本firewalld语法规则不能满足要求时，可以使用以下更复杂的规则 

* rich-rules 富规则，功能强,表达性语言 
* Direct configuration rules 直接规则，灵活性差, 帮助：man 5 firewalld.direct 4

##### 16.4.3.1 管理rich规则 

rich规则比基本的firewalld语法实现更强的功能，不仅实现允许/拒绝，还可以实现日志syslog和 auditd，也可以实现端口转发，伪装和限制速率 

规则实施顺序： 

* 该区域的端口转发，伪装规则 
* 该区域的日志规则 
* 该区域的允许规则 
* 该区域的拒绝规则

每个匹配的规则生效，所有规则都不匹配，该区域默认规则生效 

rich语法：

```
rule
	[source]
	[destination]
	service|port|protocol|icmp-block|masquerade|forward-port
	[log]
	[audit]
	[accept|reject|drop]
```

man 5 firewalld.richlanguage

**rich规则选项**

| 选项                  | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| --add-rich-rule=''    | Add to the specified zone, or the default zone if no zone is specified. |
| --remove-rich-rule='' | Remove to the specified zone, or the default zone if no zone is specified. |
| --query-rich-rule=''  | Query if has been added to the specified zone, or the default zone if no zone is specified. Returns 0 if the rule is present, otherwise 1. |
| --list-rich-rules     | Outputs all rich rules for the specified zone, or the default zone if no zone is specified. |

##### 16.4.3.2 rich规则实现 

拒绝从192.168.0.100的所有流量，当address 选项使用source 或 destination时，必须用family= ipv4  |ipv6

```bash
firewall-cmd --permanent --zone=public --add-rich-rule='rule   family=ipv4 source address=192.168.0.100/32 reject'
```

限制每分钟只有两个连接到ftp服务

```bash
firewall-cmd --add-rich-rule=‘rule service name=ftp limit value=2/m accept’
```

抛弃esp（ IPsec 体系中的一种主要协议）协议的所有数据包

```bash
firewall-cmd --permanent --add-rich-rule='rule protocol value=esp drop'
```

接受所有192.168.1.0/24子网端口5900-5905范围的TCP流量

```bash
firewall-cmd --permanent --zone=vnc --add-rich-rule='rule family=ipv4 source 
address=192.168.1.0/24 port port=5900-5905 protocol=tcp accept'
```

**rich日志规则**

```bash
log [prefix="<PREFIX TEXT>" [level=<LOGLEVEL>] [limit value="<RATE/DURATION>"]

<LOGLEVEL> 可以是emerg,alert, crit, error, warning, notice, info, debug.
<DURATION> s：秒, m：分钟, h：小时, d：天
audit [limit value="<RATE/DURATION>"]
```

##### 16.4.3.3 伪装和端口转发 

NAT网络地址转换，firewalld支持伪装和端口转发两种NAT方式 

**伪装NAT**

```bash
firewall-cmd --permanent --zone=<ZONE> --add-masquerade
firewall-cmd --query-masquerade  #检查是否允许伪装
firewall-cmd --add-masquerade   #允许防火墙伪装IP
firewall-cmd --remove-masquerade #禁止防火墙伪装IP
```

**端口转发** 

端口转发：将发往本机的特定端口的流量转发到本机或不同机器的另一个端口。通常要配合地址伪装才 能实现

```bash
firewall-cmd --permanent --zone=<ZONE> --add-forward-port=port=<PORTNUMBER>:proto=<PROTOCOL>[:toport=<PORTNUMBER>][:toaddr=]
```

说明：toport= 和toaddr= 至少要指定一个

rich规则的port转发语法：

```bash
forward-port port=<PORTNUM> protocol=tcp|udp [to-port=<PORTNUM>] [to-addr=<ADDRESS>]
```

### 16.5 nft 

#### 16.5.1 nft 介绍 

nftables 是一个 netfilter 项目，旨在替换现有的 {ip,ip6,arp,eb}tables 框架，为 {ip,ip6}tables 提供一 个新的包过滤框架、一个新的用户空间实用程序（nft）和一个兼容层。它使用现有的钩子、链接跟踪系 统、用户空间排队组件和 netfilter 日志子系统。 

nftables 主要由三个组件组成：内核实现、libnl netlink 通信和 nftables 用户空间。 其中内核提供了一 个 netlink 配置接口以及运行时规则集，libnl 包含了与内核通信的基本函数，用户空间可以通过 nft 和 用户进行交互。 

在 Linux 内核版本高于 3.13 时可用。提供一个新的命令行工具 nft 语法与 iptables 不同。 

官方Wiki：https://wiki.nftables.org

官方文档：https://www.netfilter.org/projects/nftables/manpage.html 

参考文档：https://www.mankier.com/8/nft

#### 16.5.2 nft 相关概念 

nftables 和 iptables 一样，由表（table）、链（chain）和规则（rule）组成，其中表包含链，链包含 规则，规则是真正的 action，规则由地址，接口，端口或包含当前处理数据包中的其他数据等表达式以 及诸如drop, queue, continue等声明组成。 

与 iptables 相比，nftables 主要有以下几个变化： 

* iptables 规则的布局是基于连续的大块内存的，即数组式布局；而 nftables 的规则采用链式布局， 即数组和链表的区别 
* iptables 大部分工作在内核态完成，如果要添加新功能，只能重新编译内核；而 nftables 的大部分 工作是在用户态完成的，添加新功能更加容易，不需要改内核 
* nftables不包含任何内置表和链 
* 拥有使用额外脚本的能力, 拥有一些高级的类似编程语言的能力，例如定义变量和包含外部文件 
* iptables 有内置的链，即使只需要一条链，其他的链也会跟着注册；而 nftables 不存在内置的链， 可以按需注册。由于 iptables 内置了一个数据包计数器，所以即使这些内置的链是空的，也会带来 性能损耗 
* 简化了 IPv4/IPv6 双栈管理 
* 原生支持集合、字典和映射 

nftables 的每个表只有一个地址簇，并且只适用于该簇的数据包。表可以指定五个簇中的一个：

| nftables簇                | iptables命令行工具  |
| ------------------------- | ------------------- |
| ip IPv4 地址              | iptables            |
| ip6 IPv6 地址             | iptables            |
| inet IPv4 和 IPv6 地址    | iptables和ip6tables |
| arp 地址解析协议(ARP)地址 | arptables           |
| bridge 处理桥接数据包     | ebtables            |

inet 同时适用于 IPv4 和 IPv6 的数据包，即统一了 ip 和 ip6 簇，可以更容易地定义规则，注：当没 有指定地址簇时，默认为ip 

IPv4/IPv6/Inet address family hooks

链是用来保存规则的，和表一样，链也需要被显示创建，因为 nftables 没有内置的链。链有以下两种类 型： 

* **基本链** : 数据包的入口点，需要指定钩子类型和优先级，相当于内置链 
* **常规链** : 不需要指定钩子类型和优先级，可以用来做跳转，从逻辑上对规则进行分类，类似于自定 义链

#### 16.5.3 nft 常见用法 

##### 16.5.3.1 nft 命令格式

```bash
[root@centos8 ~]#nft --help
Usage: nft [ options ] [ cmds... ]
选项说明
-h, --help 显示帮书
-v, --version 显示版本信息
-c, --check 检查命令的有效性，而不实际应用更改。
-f, --file <filename> 包含文件内容<filename>
-i, --interactive 从命令行读取输入
-j, --json 以JSON格式化输出
-n, --numeric 指定一次后，以数字方式显示网络地址（默认行为）。指定两次以数字方式显示Internet服务（端口号）。指定三次以数字方式显示协议，用户ID和组ID。
-s, --stateless 省略规则集的有状态信息
-N 将IP地址转换为名称。
-a, --handle 显示规则句柄handle
-e, --echo Echo what has been added, inserted or replaced
-I, --includepath <directory> 添加<directory>目录到包含文件的搜索路径中。默认为: /etc
--debug <level [,level...]> 添加调试,在level处(scanner, parser, eval, netlink, mnl, proto-ctx, segtree, all)
```

**nft 命令基本格式** 

nft 操作符 操作目标 操作内容

```
1 操作符: 增,删,改,查,清除,插入,创建
表操作：add,delete,list,flush
链操作：add,delete,rename,list,flush,create
规则：add,delete,insert

2 操作目标: 簇,表,链,规则
链类型：filter,route,nat
链钩子：hook

3 操作内容：...
```

**规则选项：**

| 1    | accept   | 接受  | 接受 包                     | 停止处理                                        |
| ---- | -------- | ----- | --------------------------- | ----------------------------------------------- |
| 2    | drop     | 丢弃  | 丢弃包                      | 停止处理                                        |
| 3    | reject   | 拒绝  | 驳回包                      | 停止处理                                        |
| 4    | queue    | 队列  | 发送包到用户空间程 序       | 停止处理                                        |
| 5    | continue | 继续  | 继续处理包                  |                                                 |
| 6    | return   | 返回  | 发送到调用的规则链 进行处理 |                                                 |
| 7    | jump     | 跳跃  | 发送到指定的规则链 进行处理 | 当完成时或执行了返回的声明，返回到 调用的规则链 |
| 8    | goto     | 转到  | 发送到指定的规则链 进行处理 | 不返回到调用的规则链                            |
| 9    | limit    | limit | 达到接收包的匹配限 制，     | 则根据规则处理包                                |
| 10   | log      | log   | 日志记录 包                 | 继续处理                                        |

##### 16.5.3.2 查看

```bash
nft list ruleset # 列出所有规则
nft list tables # 列出所有表
nft list table filter # 列出ip簇的filter表
nft list table inet filter # 列出inet簇的filter表
nft list chain filter INPUT # 列出filter表input链
以上命令后面也可以加 -nn 用于不解析ip地址和端口
加 -a 用于显示 handles
```

##### 16.5.3.3 增加 

增加表：nft add table fillter  

增加链：nft add chain filter input { type filter hook input priority 0 \; } # 要和hook（钩子）相关连 

增加规则：nft add rule filter input tcp dport 22 accept

##### 16.5.3.4 删 

只需要把上面的 add 改为 delete 即可 

##### 16.5.3.5 改 

更改链名用rename 

更改规则用replace 

#### 16.5.4 nft 实战案例 

##### 16.5.4.1 创建表和删除表

```bash
[root@centos8 ~]#nft add table inet test_table
[root@centos8 ~]#nft list tables
table inet filter
table inet test_table
[root@centos8 ~]#nft delete table inet test_table
[root@centos8 ~]#nft list tables
table inet filter
[root@centos8 ~]#nft add table inet test_table
```

列出所有的规则：

```bash
[root@centos8 ~]#nft list ruleset
table inet filter {
 chain input {
 type filter hook input priority 0; policy accept;
 }
 chain forward {
 type filter hook forward priority 0; policy accept;
 }
 chain output {
 type filter hook output priority 0; policy accept;
 }
}
table inet test_table {
}
```

##### 16.5.4.2 创建链 

现在表中还没有任何规则，需要创建一个链来保存规则 

**创建基本链：**

```bash
[root@centos8 ~]#nft add chain inet test_table test_filter_input_chain { type filter hook input priority 0 \; }
```

* 反斜线（ \ ）用来转义，这样 shell 就不会将分号解释为命令的结尾。 
* priority 采用整数值，可以是负数，值较小的链优先处理

**创建常规链：**

```bash
[root@centos8 ~]#nft add chain inet test_table test_chain
```

**列出链：**

```bash
[root@centos8 ~]#nft list table inet test_table
table inet test_table {
 chain test_chain {
 }
 chain test_filter_input_chain {
 type filter hook input priority 0; policy accept;
 }
}
[root@centos8 ~]#nft list chain inet test_table test_filter_input_chain
table inet test_table {
 chain test_filter_input_chain {
 type filter hook input priority 0; policy accept;
 }
}
[root@centos8 ~]#nft list chain inet test_table test_chain
table inet test_table {
 chain test_chain {
 }
}
[root@centos8 ~]#nft list ruleset
table inet filter {
 chain input {
 type filter hook input priority 0; policy accept;
 }
 chain forward {
 type filter hook forward priority 0; policy accept;
 }
 chain output {
 type filter hook output priority 0; policy accept;
 }
}
table inet test_table {
 chain test_chain {
 }
 chain test_filter_input_chain {
 type filter hook input priority 0; policy accept;
 }
}
```

##### 16.5.4.3 创建规则 

###### 16.5.4.3.1 创建常规链规则 

有了表和链之后，就可以创建规则了，规则由语句或表达式构成，包含在链中

```bash
#创建常规链的规则
[root@centos8 ~]#nft add rule inet test_table test_chain tcp dport http reject
[root@centos8 ~]#nft list chain inet test_table test_chain
table inet test_table {
 chain test_chain {
 tcp dport http reject
 }
}
#常规链规则默认不生效
[root@centos7 ~]#curl 10.0.0.8
rft Http Server
```

add 表示将规则添加到链的末尾，如果想将规则添加到链的开头，可以使用 insert 。

```bash
[root@centos8 ~]# nft insert rule inet test_table test_chain tcp dport mysql reject
```

列出规则：

```bash
[root@centos8 ~]#nft list chain inet test_table test_chain
table inet test_table {
 chain test_chain {
 tcp dport mysql reject
 tcp dport http reject
 }
}
[root@centos8 ~]#nft list table inet test_table
table inet test_table {
 chain test_chain {
 tcp dport mysql reject
 tcp dport http reject
 }
 chain test_filter_input_chain {
 type filter hook input priority 0; policy accept;
 }
}
```

###### 16.5.4.3.2 创建基本链规则

```bash
[root@centos8 ~]#nft add rule inet test_table test_filter_input_chain tcp dport 
http reject
[root@centos8 ~]#nft add rule inet test_table test_filter_input_chain ip   saddr 
10.0.0.6 reject
[root@centos8 ~]#nft list chain inet test_table test_filter_input_chain
table inet test_table {
 chain test_filter_input_chain {
 type filter hook input priority 0; policy accept;
 tcp dport http reject
 ip saddr 10.0.0.6 reject
 }
}
[root@centos7 ~]#curl 10.0.0.8
curl: (7) Failed connect to 10.0.0.8:80; Connection refused
```

##### 16.5.4.4 插入链的指定位置 

将规则插入到链的指定位置，有两种方法： 

###### 16.5.4.4.1 使用 index 来指定规则的索引 

index 类似于 iptables 的 -I 选项， index 的值是从 0 开始的 

index 必须指向一个存在的规则，比如 nft insert rule … index 0 就是非法的。 

add 表示新规则添加在索引位置的规则后面 

insert 表示新规则添加在索引位置的规则前面

###### 16.5.4.4.2 使用 handle 来指定规则的句柄 

在 nftables 中，句柄值是固定不变的，除非规则被删除，这就为规则提供了稳定的索引。而 index 的 值是可变的，只要有新规则插入，就有可能发生变化。一般建议使用 handle 来插入新规则。 

add 表示新规则添加在索引位置的规则后面， insert 表示新规则添加在索引位置的规则前面。 handle 的值可以通过参数 --handle 或者 -a 获取

可以在创建规则时就获取到规则的句柄值，在创建规则时同时加上参数 --echo或者-e 和 --handle

##### 16.5.4.5 删除规则 

###### 16.5.4.5.1 删除单个规则 

单个规则只能通过其句柄删除，首先需要找到想删除的规则句柄

然后使用句柄值来删除该规则：

###### 16.5.4.5.2 删除所有规则

```bash
[root@centos8 ~]#nft flush ruleset 
[root@centos8 ~]#nft list ruleset 
```

##### 16.5.4.6 列出规则 

可以列出所有规则，也可以列出规则的一部分 

###### 16.5.4.6.1 列出所有规则

```bash
[root@centos8 ~]#nft list ruleset
table inet filter {
 chain input {
 type filter hook input priority 0; policy accept;
 }
...
```

###### 16.5.4.6.2 列出指定表中的所有规则

```bash
[root@centos8 ~]#nft list table inet test_table
table inet test_table {
 chain test_chain {
 tcp dport mysql reject
 tcp dport http reject
 }
...
```

###### 16.5.4.6.3 列出指定链中的所有规则

```bash
[root@centos8 ~]#nft list chain inet test_table test_filter_input_chain
table inet test_table {
 chain test_filter_input_chain {
 type filter hook input priority 0; policy accept;
 tcp dport mysql reject
 tcp dport ftp reject
 udp dport http-alt reject
 tcp dport http reject
 tcp dport 6379 reject
 }
}
```

##### 16.5.4.7 备份还原 

规则都是临时的，要想永久生效，可以将规则备份，重启后自动加载恢复 

查看service文件

```bash
[root@centos8 ~]#cat /lib/systemd/system/nftables.service 
[Unit]
Description=Netfilter Tables
Documentation=man:nft(8)
Wants=network-pre.target
Before=network-pre.target
...
```

**备份配置并还原**

```bash
#备份至文件中
[root@centos8 ~]#nft list ruleset 
table inet filter {
 chain input {
 type filter hook input priority 0; policy accept;
 }
 chain forward {
...
#删除所有规则
[root@centos8 ~]#nft flush ruleset 
[root@centos8 ~]#nft list ruleset
#重新启动后全部还原
[root@centos8 ~]#systemctl restart nftables.service 
[root@centos8 ~]#nft list ruleset 
table inet filter {
 chain input {
 type filter h
 ...
```

**启用指定的配置文件**

```bash
[root@centos8 ~]#cat nftables2.conf
table inet test2_table {
 chain test2_filter_input_chain {
 type filter hook input priority 0; policy accept;
 ip saddr { 10.0.0.1, 10.0.0.10 } accept
 tcp dport { http, nfs,ssh } reject
 }
}
#-f 指定规则配置文件，如果已经有规则，是追加至现有规则后
[root@centos8 ~]#nft -f nftables2.conf
[root@centos8 ~]#nft list ruleset
table inet test2_table {
 chain test2_filter_input_chain {
 type filter hook input priority 0; policy accept;
 ip saddr { 10.0.0.1, 10.0.0.10 } accept
 tcp dport { ssh, http, nfs } reject
 }
}
```

##### 16.5.4.8 迁移iptables规则到nft

1. To save the existing rules to a file, run below command:

```bash
# iptables-save > rules.iptables
```

2. Move the step1 file to CentOS/RHEL 8 Server via scp or ftp. You can use vi editor as well to  copy the content from CentOS/RHEL 6 or 7 machine. 
3. Run the below command to generate the nft rules file on CentOS/RHEL 8 with iptables rules  file.

```bash
# iptables-restore-translate -f rules.iptables > rules.nft
```

4. Load the rules in CentOS/RHEL 8 machine, make sure nftables service is running on the  system.

```bash
# nft -f rules.nft     # load the rule via nft to nftables.
```

5. To Display rule in CentOS/RHEL 8 Server .

```bash
# nft list ruleset
```

You can see the rules have been migrated from CentOS/RHEL 6 or 7 to CentOS/RHEL 8 server now and can test them as well

### 16.6 练习 

说明：以下练习INPUT和OUTPUT默认策略均为DROP 

1. 限制本地主机的web服务器在周一不允许访问；新请求的速率不能超过100个每秒；web服务器包含 了admin字符串的页面不允许访问；web服务器仅允许响应报文离开本机

2. 在工作时间，即周一到周五的8:30-18:00，开放本机的ftp服务给172.16.0.0网络的主机访问；数据下 载请求的次数每分钟不得超过5个 

3. 开放本机的ssh服务给172.16.x.1-172.16.x.100中的主机，x为你的学号，新请求建立的速率一分钟 不得超过2个；仅允许响应报文通过其服务端口离开本机 

4. 拒绝TCP标志位全部为1及全部为0的报文访问本机 

5. 允许本机ping别的主机；但不开放别的主机ping本机 

6. 判断下述规则的意义

   ```bash
   iptables -N clean_in
   iptables -A clean_in -d 255.255.255.255 -p icmp -j DROP
   iptables -A clean_in -d 172.16.255.255 -p icmp -j DROP
   iptables -A clean_in -p tcp ! --syn -m state --state NEW -j DROP
   iptables -A clean_in -p tcp --tcp-flags ALL ALL -j DROP
   iptables -A clean_in -p tcp --tcp-flags ALL NONE -j DROP
   iptables -A clean_in -d 172.16.100.7 -j RETURN 
   iptables -A INPUT -d 172.16.100.7 -j clean_in
   iptables -A INPUT  -i lo -j ACCEPT
   iptables -A OUTPUT -o lo -j ACCEPT
   iptables -A INPUT  -i eth0 -m multiport -p tcp --dports 53,873,135,137,139,445 -
   j DROP
   iptables -A INPUT  -i eth0 -m multiport -p udp --dports 53,873,135,137,139,445 -
   j DROP
   iptables -A INPUT  -i eth0 -m multiport -p tcp --dports 1433,3389 -j DROP
   iptables -A INPUT  -p icmp -m limit --limit 10/second -j ACCEPT
   ```

7. 实现主机防火墙 放行telnet, ftp, web服务 放行samba服务 放行dns服务(查询和区域传送) 

8. 实现网络防火墙 放行telnet, ftp, web服务 放行samba服务 放行dns服务(查询和区域传送)
