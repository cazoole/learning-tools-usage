# Object description
## clone
- 通过该方式去复制一个对象，含有深拷贝和浅拷贝
## getClass
- 万物接对象，该方法会返回当前类型的class值
## hashcode & equals
- 提升效率，使用hashcode去定位数据，然后，equals去比较，hashcode可以算是伪地址
## registerNatives
```
 用于在启动时将native方法加载到jvm中。
 需要执行两个步骤：
   第一，通过System.loadLibrary()将包含本地方法实现的动态文件加载进内存；
   第二，当Java程序需要调用本地方法时，虚拟机在加载的动态文件中定位并链接该本地方法，从而得以执行本地方法。
1. 通过registerNatives方法在类被加载的时候就主动将本地方法链接到调用方，
   比当方法被使用时再由虚拟机来定位和链接更方便有效；
2. 如果本地方法在程序运行中更新了，可以通过调用registerNative方法进行更新；
3. Java程序需要调用一个本地应用提供的方法时，因为虚拟机只会检索本地动态库，
  因而虚拟机是无法定位到本地方法实现的，这个时候就只能使用registerNatives()方法进行主动链接。
```
