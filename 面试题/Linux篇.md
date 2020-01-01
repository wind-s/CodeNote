
linux篇：

    1.查看 CPU 的命令和磁盘 IO 的命令
    top
    iostat
    2.生产环境系统变慢诊断,思路？
    1.整机 top
        注意load average 1.61，0.90， 0.41（系统的负载均衡）分别代表系统的 1分钟 5分钟 15分钟的负载值
                         a     b    c
                    若 （a+b+c）/3*100% > 60% 则系统过载
     uptime 也可查看load averages
    2.cpu：vmstat
        1.vmstat -n 2 3
        每2s采样一次，共计采样3次。
        wushaofeng@ubuntu:~$ vmstat -n 2 3
        procs -----------memory---------- ---swap-- -----io---- -system-- ----cpu----
         r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa
         3  0 2080268 386068 100140 1869800    0    0     2    11    0    0 11  1 88  0
         0  0 2080268 387588 100156 1869824    0    0     0    68 32266 34914  7  1 92  0
         2  0 2080268 384928 100156 1869872    0    0     0   144 34758 39464 10  2 88  0

        主要看procs cpu 两列
         procs:
            r: 运行和等待cpu时间片的进程数，原则上1核的cpu的运行队列不要超过2，整个系统的运行队列不能超过
                总核数的2倍。否则代表系统压力过大。
            b: 等待资源的进程数，比如正在等待磁盘i/o，网络i/o
         cpu:
            us: 用户进程消耗cpu时间百分比，us值高，用户进程消耗cpu时间多，若长期大于50%，优化程序。
            sy: 内核进程消耗cpu时间百分比。
            us + sy > 80%, 说明可能存在cpu不足

            id: 处于空闲的cpu百分比。如果小于百分之60，说明开始有点压力了。
            wa: 系统等待io的cpu时间百分比
            st: 来自于一个虚拟机偷取的cpu时间百分比。

        2.查看所有cpu核信息：
            mpstat -P ALL 2
            2s采样一次
            Linux 3.2.0-136-custom (ubuntu)         12/07/2019      _x86_64_        (24 CPU)

            03:44:20 PM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest   %idle
            03:44:22 PM  all   14.21    0.02    0.99    0.96    0.00    0.29    0.00    0.00   83.53 （看这行即可）
            03:44:22 PM    0   11.68    0.00    2.03   23.86    0.00    0.51    0.00    0.00   61.93
            03:44:22 PM    1   10.00    0.00    2.50    0.00    0.00    0.50    0.00    0.00   87.00
            03:44:22 PM    2   13.71    0.00    1.52    0.00    0.00    0.51    0.00    0.00   84.26
            03:44:22 PM    3   12.50    0.00    3.00    0.00    0.00    0.50    0.00    0.00   84.00
            03:44:22 PM    4   27.72    0.50    1.49    0.00    0.00    0.99    0.00    0.00   69.31
            03:44:22 PM    5   11.17    0.00    1.02    0.00    0.00    1.02    0.00    0.00   86.80


        3.每个进程使用cpu的用量分解信息
            pidstat -u 1 -p 进程编号
            Linux 3.2.0-136-custom (ubuntu)         12/07/2019      _x86_64_        (24 CPU)
            03:49:07 PM       PID    %usr %system  %guest    %CPU   CPU  Command
            03:49:08 PM     19157    0.00    0.00    0.00    0.00    18  jsvc
            03:49:09 PM     19157    0.00    0.00    0.00    0.00    18  jsvc
            03:49:10 PM     19157    0.00    0.00    0.00    0.00    18  jsvc
            03:49:11 PM     19157    0.00    0.00    0.00    0.00    18  jsvc
            03:49:12 PM     19157    0.00    0.00    0.00    0.00    18  jsvc

    3.内存：free
        1.应用程序可用内存数
            free
            free -g
            free -m (常用)
            经验值：
                应用程序可用内存/系统物理内存 > 70%内存充足
                应用程序可用内存/系统物理内存 < 20%内存不足，需要增加内存
                20% < 应用程序可用内存/系统物理内存 < 70%内存基本够用
        2.查看额外
            pidstat -p 进程号 -r 采样间隔秒数
            Linux 3.2.0-136-custom (ubuntu)         12/07/2019      _x86_64_        (24 CPU)

            08:46:20 PM       PID  minflt/s  majflt/s     VSZ    RSS   %MEM  Command
            08:46:22 PM     17439      1.00      0.00 15416648 1582056   9.82  jsvc
            08:46:24 PM     17439      0.00      0.00 15416648 1582056   9.82  jsvc
            08:46:26 PM     17439     83.00      0.00 15416648 1582844   9.83  jsvc

    4.硬盘：df -h
        查看磁盘剩余空间
        df -h
        Filesystem      Size  Used Avail Use% Mounted on
        /dev/sda1        28G  7.4G   19G  29% /
        udev            7.7G  552K  7.7G   1% /dev
        tmpfs           3.1G  7.3M  3.1G   1% /run
        none            5.0M     0  5.0M   0% /run/lock
        none            7.7G     0  7.7G   0% /run/shm
        /dev/sda3       244G  117G  115G  51% /data

    5.磁盘io: iostat
        iostat -xdk 2 3

        Linux 3.2.0-136-custom (ubuntu)         12/07/2019      _x86_64_        (24 CPU)
        Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
        sda               0.09    11.00    2.02   10.71    44.39   262.96    48.28     0.02    1.24    1.61    1.16   3.36   4.28

        Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
        sda               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00

        Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
        sda               0.00    15.00    0.00    8.50     0.00    94.00    22.12     0.05    5.65    0.00    5.65   5.65   4.80

        磁盘块设备分布
        rkB/s 每秒读取数据量kB
        wkB/s 每秒写入数据量kB
        svtm I/O 请求的平均服务时间，单位毫秒；
        await I/O 请求的平均等待时间，单位毫秒；值越小，性能越好；
      (重点) util 一秒中有百分几的时间用于I/O操作。接近100%时，表示磁盘带宽跑满了，需要优化程序或者增加磁盘；
            rkB/s, wkB/s 根据系统应用不同会有不同的值，但有规律遵循：长期、超大数据读写，肯定不正常，需要优化程序读取。
            svctm的值与await的值很接近， 表示几乎没有I/O 等待，磁盘性能好。
            如果await的值远高于远高于svctm的值，则表示I/O队列等待太长，需要优化程序，或者更换更快磁盘。
    

    6.网络： ifstat
        默认本地没有，需要下载
            wget http://gael.roualland.free.fr/ifstat/ifstat-1.1.tar.gz
            tar xzvf ifstat-1.1.tar.gz
            cd ifstat-1.1
            ./configure
            make
            make install

            wushaofeng@ubuntu:~/ifstat-1.1$ ifstat 1
                   eth0                eth1       
             KB/s in  KB/s out   KB/s in  KB/s out
               75.53    115.32      0.12      0.00
               71.89     93.57      0.34      1.08
               60.53     89.90      0.93      0.81
               71.61    108.00      0.54      7.01

    3. 生产环境cpu过高，怎么定位解决。
    1.先用top命令找出cpu占比最高的
    2.ps -ef 或者jps进一步定位，得知是一个什么样子的进程
        jps -l
        ps -ef|grep 应用名|grep -v grep
    3.定位到具体线程或者代码
        ps -mp 进程 -o THREAD,tid,time    tid线程id, time是线程占用cpu的时间
        参数说明：
            -m 显示所有的线程
            -p pid进程使用cpu的时间
            -o 该参数后是用户自定义格式

        wushaofeng@ubuntu:~/ifstat-1.1$ ps -mp 60863 -o THREAD,tid,time     
        USER     %CPU PRI SCNT WCHAN  USER SYSTEM   TID     TIME
        www-data  0.3   -    - -         -      -     - 07:47:42
        www-data  0.0  19    - hrtime    -      - 60863 00:00:02
        www-data  35.0  19    - futex_    -     - 60897 00:00:43  (有问题线程id 60897)
        www-data  0.0  19    - futex_    -      - 60898 00:00:42
        www-data  0.0  19    - futex_    -      - 60899 00:00:43
        www-data  0.0  19    - futex_    -      - 60900 00:00:43
        www-data  0.0  19    - futex_    -      - 60901 00:00:42

    4.将需要的线程id转换为16进制格式（英文小写格式）
        printf "%x\n" 有问题的线程id
        printf "%x\n" 60863
        edbf
    5.jstack 进程ID | greo tid(16进制线程ID小写英文) -A60
        jstack 60863 |grep edbf -A60 
        
        一般会报错：
            17439: Unable to open socket file: target process not responding or HotSpot VM not loaded
            The -F option can be used when the target process is not responding
        处理方法：
            sudo -s
            jstack -F 60863 |grep edbf -A60 
    6.sudo jstat -gcutil 17439 1000 5
        root@ubuntu:/home/wushaofeng# sudo jstat -gcutil 17439 1000 5
          S0     S1     E      O      M     CCS    YGC     YGCT    FGC    FGCT     GCT   
          0.00   7.48  65.36  96.52  92.14  90.88  70707  419.644 1037871 59070.128 59489.773
          0.00   7.48  66.91  96.52  92.14  90.88  70707  419.644 1037871 59070.128 59489.773
          0.00   7.48  67.13  96.52  92.14  90.88  70707  419.644 1037871 59070.128 59489.773
          0.00   7.48  68.80  96.52  92.14  90.88  70707  419.644 1037871 59070.128 59489.773
          0.00   7.48  70.38  96.52  92.14  90.88  70707  419.644 1037871 59070.128 59489.773

         图中参数含义如下： 
            S0 — Heap上的 Survivor space 
            0 区已使用空间的百分比    
            S1 — Heap上的 Survivor space 1 区已使用空间的百分比     E   — Heap上的 Eden space 区已使用空间的百分比     O   — Heap上的 Old space 区已使用空间的百分比     P   — Perm space 区已使用空间的百分比 
            YGC — 从应用程序启动到采样时发生 Young GC 的次数 
            YGCT– 从应用程序启动到采样时 Young GC 所用的时间(单位秒)     FGC — 从应用程序启动到采样时发生 Full GC 的次数 
            FGCT– 从应用程序启动到采样时 Full GC 所用的时间(单位秒)     GCT — 从应用程序启动到采样时用于垃圾回收的总时间(单位秒) 
    故障排查：    
    https://www.javatang.com/archives/2017/10/20/12131956.html