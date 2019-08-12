# JVM命令行工具

## jps - 虚拟机集成状况工具
```
-l 输出Java应用程序的main class的完整包
-q 仅显示pid，不显示其他任何相关信息
-m 输出传递给JVM的参数，在诊断JVM相关问题的时候，该参数可以查看JVM相关参数设置。
```

## jstat - 虚拟机统计信息监视工具 （可以用来查看堆信息，vmid是应用的id）
```
-class 监视类装载、卸载数量，总空间及类装载所消耗的时间
-gc 监视Java堆状况，包括Eden区，2个survivor区，老年代、永久代的容量，已用空间，GC时间合计等信息
-gccapacity 监视内容与-gc基本相同，但输出主要关注Java堆哥哥区域使用到的最大和最小空间
-gcutil 监视内容与-gc基本相同，但输出主要关注已使用空间占总空间的百分比
-gccause 与-gcutil功能一样，但是会额外输出导致上一次GC产生的原因
-gcnew 监视新生代GC的状况
-gcnewcapacity 与-gcnew功能基本相同，输出主要关注使用到的最大和最小空间
-gcpermcapacity 输出永久代使用到的最大和最小空间
-compile 输出JIT编译器编译过的方法、耗时等信息
-printcompilation 输出已经被JIT编译的方法

S0C：年轻代中第一个survivor（幸存区）的容量（字节）
S1C：年轻代中第二个survivor的容量
S0U：年轻代中第一个survivor目前已使用空间（字节）
S1U：年轻代中第二个survivor目前已使用空间（字节）
EC：年轻代中Eden（伊甸园）的容量（字节）
EU：年轻代中Eden目前已使用空间
OC：Old代的容量
OU：Old代目前一是哦有那个空间
PC：Perm（持久代）的容量
PU：Perm（持久代）目前已使用空间
YGC：从应用程序启动到采样时年轻代中gc次数
YGCT：从应用程序启动到采样时年轻代中gc所用时间（s)
FGC：从应用程序启动到采样时Old代（全GC）gc次数
FGCT：从应用程序启动到采样时Old代（全gc）gc所用的时间（s)
GCT：从应用程序启动到采样时gc用的总时间（s)
NGCMN：年轻代（young）中初始化（最小）的大小（字节）
NGCMX：年轻代（young)的最大容量
NGC：年轻代（young）中当前的容量
OGCMN：Old代中初始化（最小）的大小（字节）
OGCMX：Old代的最大容量
OGC：Old代当前新生成的容量
PGCMN：Perm代中初始化的大小
PGCMX：Perm代的最大容量
PGC：Perm代当前新生成的容量
S0： 年轻代中第一个survivor已使用的占当前容量百分比
S1：年轻代第二个survivor已使用的占当前容量百分比
E：年轻代中Eden已使用的占当前容量百分比
O：Old代已使用的占当前容量百分比
P：Perm代已使用的占当前容量百分比
S0CMX：年轻代中第一个survivor的最大容量
S1CMX：年轻代中第二个survivor的最大容量
ECMX：年轻代中Eder的最大容量
DSS：当前需要survivor的容量（Eden区已满）
TT：持有次数限制
MTT：最大持有次数限制
```

## JINFO - Java配置信息工具（观察运行中的Java程序的运行环境参数，包含Java System属性和JVM命令行参数，也可以设置参数的值，使之立即生效）
```
jinfo -flag <name> 打印指定的JVM的参数值
jinfo - flag [+/-]<name> 设置指定JVM参数的布尔值
jinfo -flag <name>=<value> 设置指定JVM参数的值
```

## JMAP - Java内存映像工具（可以生成Java应用程序的对快照和对象统计信息）
```
jmap -heap pid 查看堆内存使用情况，包括使用的GC算法、堆配置参数和各代中对内存使用情况
jmap -histo[:live] pid 查看对内存中的对象数目、大小统计直方图，如带上live则只统计活对象
```

## JSTACK - Java Stack Trace（可以用来查看Java应用程序的堆栈信息）
```
jstack -l pid 可以用来查看进程中线程状态，检测死锁等
```


## 问题排查
```
ps -ef | grep <appName> | grep -v grep -> 获取pid
top -Hp pid -> 获取占用cpu最高的进程pid
printf "%x" pid -> 获取占用cpu最高的进程的pid的十六机制数
jstack pid | grep <3中16进制数> -> 查看那个对象的何种方法在耗用
```
