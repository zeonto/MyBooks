Linux 系统下调试与追踪进程
=======================

## ps

ps -ef | grep mysql




## pstack

pstack 进程ID


## strace

strace -cp <PID>

-p : 指定追踪的进程ID
-f : 跟踪目标进程，以及目标进程创建的所有子进程
-T : 在每行语句后面显示每次系统调用所花费的时间。如：<0.000020>
-c : 汇总显示系统调用 syscall 的耗时和调用情况
-o : 把 strace 的输出单独写到指定的文件
-tt : 在每行输出的前面，显示当前执行的毫秒级别的时间。如：[pid 27344] 11:37:56.489705

如果直接用 strace 跟踪某个进程的话，那么往往是满屏翻滚的字符，通过「c」选项用来汇总各个操作的总耗时，得出结果：

```
> strace -cp 28712
strace: Process 28712 attached
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 71.41    0.497539           2    233910           getpid
  6.45    0.044948          14      3136           socket
  5.28    0.036760           3      9545      6210 openat
  3.65    0.025424           2     12420           fcntl
  2.92    0.020316           3      6471           close
  2.39    0.016647           5      3136      3136 connect
  1.29    0.009013           2      3105           read
  1.24    0.008666           5      1610      1610 chown
  1.14    0.007910           2      3335           fstat
  1.05    0.007289           5      1380           stat
  0.86    0.005967          17       345           sendto
  0.80    0.005569           2      2185           poll
  0.75    0.005231           2      2185           recvfrom
  0.42    0.002948          12       230           write
  0.15    0.001065           9       114           nanosleep
  0.14    0.000981           2       460           lseek
  0.07    0.000462           2       230           geteuid
  0.00    0.000006           6         1           restart_syscall
------ ----------- ----------- --------- --------- ----------------
100.00    0.696741                283798     10956 total
```

如果想跟踪 chown 操作的信息，通过「e」选项可以跟踪某个操作，通过「T」选项可以获取操作实际消耗的时间：

```
> strace -T -e chown -p 26774
strace: Process 26774 attached
chown("/home/html/runtime/log/201901", 1002, -1) = -1 EPERM (Operation not permitted) <0.000020>
chown("/home/html/runtime/log/201901", 1002, -1) = -1 EPERM (Operation not permitted) <0.000012>
chown("/home/html/runtime/log/201901", -1, 1002) = -1 EPERM (Operation not permitted) <0.000008>
chown("/home/html/runtime/log/201901", 1002, -1) = -1 EPERM (Operation not permitted) <0.000026>
chown("/home/html/runtime/log/201901", -1, 1002) = -1 EPERM (Operation not permitted) <0.000011>
```


## gdb

进入 gdb

```
> gdb
GNU gdb (GDB) Fedora 8.1-11.fc28
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-redhat-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word".
(gdb) 
```

附加调试进程 attach <进程PID>

```
(gdb) attach 27943
Attaching to process 27943
Reading symbols from /usr/local/php72/bin/php...done.
Reading symbols from /lib64/libcrypt.so.1...Reading symbols from /lib64/libcrypt.so.1...(no debugging symbols found)...done.
(no debugging symbols found)...done.
Reading symbols from /lib64/libz.so.1...Reading symbols from /lib64/libz.so.1...warning: Loadable section ".note.gnu.property" outside of ELF segments
(no debugging symbols found)...done.
(no debugging symbols found)...done.
Reading symbols from /lib64/libresolv.so.2...(no debugging symbols found)...done.
Reading symbols from /lib64/librt.so.1...(no debugging symbols found)...done.
Reading symbols from /lib64/libpng15.so.15...Reading symbols from /lib64/libpng15.so.15...(no debugging symbols found)...done.
......
(gdb) 
```

查看堆栈信息

```
(gdb) bt
#0  0x00007fdf068788d4 in nanosleep () from /lib64/libc.so.6
#1  0x00007fdf068a4194 in usleep () from /lib64/libc.so.6
#2  0x000000000074aeca in zif_usleep (execute_data=<optimized out>, return_value=0x7fff4d55ad30)
    at /home/vagrant/php-7.2.5/ext/standard/basic_functions.c:4555
#3  0x00007fdef57fa31d in xdebug_execute_internal (current_execute_data=0x7fdf02221d50, 
    return_value=0x7fff4d55ad30) at /home/abc/tmp/xdebug/xdebug.c:1977
......
---Type <return> to continue, or q <return> to quit---
```