# ZGC
```
 是一个可伸缩、低延迟的垃圾收集器：
    1. 停顿时间不会超过10ms
    2. 停顿时间不会随着堆的增大而增大
    3. 支持几百M甚至T级别，最大支持4T
 相比与G1只能回收部分Region，避免了全堆扫描，改善了大堆下的停顿时间，
但是在普通堆大小里表现一般
```
## Concurrent
- ZGC 只有短暂的STW，大部分过程都是和应用线程并发执行，比如最耗时的并发标记和并发移动过程
## Region-based
- 只有一块块的内存区域page，以page为单位进行分配和回首
## Compacting
- 每次进行GC时，都会对page进行压缩操作，完全避免了CMS算法中的碎片化问题
## NUMA-aware
- 默认支持NUMA架构，创建对象时，会根据线程由那个cpu执行，优先在对应的Cpu内存进行分配，可显著提升性能
## Using colored pointers
```
  G1 和 CMS 会在对象头部进行标记，ZGC则是标记对象的指针。
64位的对象，其中低42为是对象的地址，42-45位用来做指标标记
```
## Using load barries
```
因为在标记和移动过程中，GC线程和应用线程是并发执行的，所以存在这种情况：对象A内部的引用所指的对象B
在标记或者移动状态，为了保证应用线程拿到的B对象是对的，那么在读取B的指针时会经过一个load barries
读屏障，这个屏障可以保证在执行GC时，数据读取的正确性。
```
# 说明
- JDK11是最初版本，且不支持calss unloading  XX:+ClassUnloading 无效果
- JDK12 进一步减少停顿时间，支持卸载功能
- 只能在Linux/x64平台上使用
- 编译开启： --with-jvm-features=zgc,11.0.3 或12以后默认支持
- 使用：-XX:+UnlockExperimentaVMOptions -XX:+UseZGC -Xmx10g -Xlog:gc 
- 并发线程数： -XX:ConcGCThread=numbers 设置并发线程数不建议太大
- STW时并行线程数：-XX:ParallelCGThreads=numbers，用于GC Roots移动时STW的并行线程数，不超过核数的0.6

