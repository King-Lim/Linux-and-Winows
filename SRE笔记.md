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

```
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

REGEXP： Regular Expressions，由一类特殊字符及文本字符所编写的模式，其中有些字符（元字符） 不表示字符字面意义，而表示控制或通配的功能，类似于增强版的通配符功能，但与通配符不同，通配 符功能是用来处理文件名，而正则表达式是处理文本内容中字符

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

```
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
\w #匹配单词构成部分，等价于[_[:alnum:]]
\W #匹配非单词构成部分，等价于[^_[:alnum:]]
\S     #匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。
\s     #匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。注意
Unicode 正则表达式会匹配全角空格符
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

```
^ #行首锚定, 用于模式的最左侧
$ #行尾锚定，用于模式的最右侧
^PATTERN$ #用于模式匹配整行
^$ #空行
^[[:space:]]*$ #空白行
\< 或 \b   #词首锚定，用于单词模式的左侧
\> 或 \b        #词尾锚定，用于单词模式的右侧
\<PATTERN\>     #匹配整个单词
#注意: 单词是由字母,数字,下划线组成
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

```
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
-q 静默模式，不输出任何信息
-A # after, 后#行
-B # before, 前#行
-C # context, 前后各#行
-e 实现多个选项间的逻辑or关系,如：grep –e ‘cat ' -e ‘dog' file
-w 匹配整个单词
-E 使用ERE，相当于egrep
-F 不支持正则表达式，相当于fgrep
-f file 根据模式文件处理
-r   递归目录，但不处理软链接
-R   递归目录，但处理软链接
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
