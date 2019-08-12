1. ps 进程查看命令
``` -A 显示所有进程
-a 显示除控制进程外所有进程
-d 显示除控制进程外的所有进程
-e 显示所有进程
-f 显示完整格式的输出
-l 显示长列表
-H 层级格式显示进程
-L 显示进程中的线程
```

2. top命令，实时监测进程

3. kill -9 pid  用于杀死进程

4. 查看cpu信息： cat /proc/cpuinfo

5. 查看内存信息： cat /proc/meminfo

6. 查看系统信息
  ``` 查看磁盘信息： fdisk -l 
   查看内核信息： uname -a 
   查看硬件信息： dmidecode
   查看开机信息： dmesg
   查看内存使用情况： free
```

7. 查看系统文件打开数： lsof 

8. 文件查找：
``` 
    find . -name "*.xml" 当前目录下递归查找所有的xml文件
    find . -name "*.xml" | xargs grep "hello world" 递归查找文件中含有 hello world的xml文件
    find ./ -size 0 | xargs rm -f & 删除所有文件大小为 0 的文件
    ls -l | grep '.jar' 查找当前目录下所有的jar文件
    grep 'test' d* 显示所有以d开头文件中包含test的行
```

9. 查看进程是否在运行： ps -ef | grep java

10. 查看文件，包含隐藏文件： ls -al

11. 查看当前目录: pwd

12. 文档查看 
    ``` 
    查看文件前10行： head -n 10 example.txt
    查看文件尾10行： tail -n 10 example.txt
    ```

13. netstat -untlp | grep 8080 查看8080端口hi使用情况
    lsof -i :8080 查看端口属于那个程序

14. ps anx | grep java 查看java继承
```
  grep -A lineNum key fileName  查看匹配行和其后的lineNum行[-B 前lineNum行，-C 前后lineNum行]
  
```

15. 网络检测：ping ip/hostName<br/>
    远程登陆：ssh userName@ip
