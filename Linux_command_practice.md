## Useful:
```Shell
# python http
python -m SimpleHTTPServer 8000

# bashrc config
PS1='[\u@\h \W]$ '

# Virtualbox resize disk
VBoxManage modifyhd ./centos7_repo.vdi --resize 20480
fdisk /dev/sda
partprobe #通知系统分区表的变化
mkfs -t ext4 /dev/sda3
mkdir /repo
mount /dev/sda3 /repo

blkid /dev/sda3 # Check UUID
vi /etc/fstab
UUID=e19c9fb0-67b5-4843-8741-c3227a859bae /repo ext4 defaults 0 2

# ssh port change
vim /etc/ssh/ssh_config
systemctl restart sshd
```

### 1. Help
```Shell
    1. man
        man ls
    2. help
        help cd
```
### 2. 文件目录类
```Shell
    3. pwd
        pwd
        scp file root@hostname:`pwd`
    4. ls
        ls -ltr
    5. cd
        cd /path
        cd -
    6. mkdir
        mkdir -p /path/to/create
    7. rmdir
        rmdir /path/to/rm
    8. touch
        touch file
    9. cp
        cp -p source target
    10. rm
        rm -rf /path/files
    11. mv
        mv source target
    12. cat
        cat /var/log/boot.log | grep [^\[]
        cat >>file < EOF
        contents
        EOF

    13. more
    14. less
    15. less
    16. echo
        echo 'export JAVA_HOME=/usr/java/jdk' >> /etc/profile
        echo 'export PATH=$JAVA_HOME/bin:$PATH' >> /etc/profile
    17. head
        head -n 5 file
    18. tail
        tail -n 100 file
        tail -F file
    19. > & >>
    20. ln
        ln -s target source
        ln -s '/usr/lib/systemd/system/multi-user.target' '/etc/systemd/system/default.target'
    21. history
        history -n 100
```

### 3. 时间日期类
```Shell
    22. date
        date "+%Y-%m-%d"
        date "+%H:%M:%S"
        date "+%Y-%m-%d %H:%M:%S"
        date "+%Y_%m_%d %H:%M:%S"
    23. cal
        cal
        cal 2018
```

### 4. 用户管理类
```Shell
    24. useradd
        useradd -u 1000 -g 1000 -G hadoop hadoop

    25. passwd
        passwd username
        passwd -d username
    26. id
        id user
    27. su
        su -
        su - hdfs
    28. sudo
        visudo
            hadoop ALL=(ALL) NOPASSWD: ALL
        sudo -u hdfs hdfs ls /user/hdfs
        sudo -l
        # 记录sudo日志
        touch /var/log/sudo;echo "local2.debug /var/log/sudo" >> /etc/syslog.conf; systemctl restart syslogd

    29. userdel
        userdel hadoop
    30. who
        who/w
    31. usermod
        usermod -A -g hadoop -G hive hadoop
    32. groupadd
        groupadd -g 344 linuxde
    33. groupdel
        groupdel hadoop
        userdel -r linuxde #删除用户linuxde，其家目录及文件一并删除
    34. groupmod
        groupmod -g 400 hadoop
```

### 5. 文件权限类
```Shell
    35. chmod
        chmod +x files
        chmod -r 775 /PATH
        chmod 600 file
    36. chown
        chown root files
        chown root.root files
        chown -r root.root /path
    37. chgrp
        chgrp hadoop /path/file
```

### 6. 搜索查找类
```Shell
    38. find
        find ./ -name hadoop
        find ./ -name *.sh | xargs rename 's/\.sh/\.md/'
    39. grep
        ls -l |grep hadoop
    40. which
        which bash
    41. whereis
        whereis hadoop
    42. readlink
        readlink /usr/bin/hadoop
```

### 7. 压缩解压
```Shell
    43. gzip/gunzip
        gzip -d examples.gz examples
        gunzip examples.gz
    44. zip/unzip
        zip -r examples.zip examples # (examples为目录)
        zip examples.zip
    45. tar
        tar -cvf examples.tar examples
        tar -xvf examples.tar
        tar -xvf examples.tar -C /path
```

### 8. 磁盘分区
```Shell
    46. df
        df -h
    47. fdisk
        fdisk /dev/sdb
            p、打印分区表
            n、新建一个新分区
            d、删除一个分区
            q、退出不保存
            w、把分区写进分区表，保存并退出
    48. parted
        parted -s /dev/sdc -- mklabel gpt mkpart primary xfs 0% 8192GB
    49. mount/umount
        umount -v /dev/sda1
        umount -v /mnt/mymount/
        lsof | grep mymount #查找mymount分区里打开的文件
        mount -t auto /dev/cdrom /mnt/cdrom
```

### 9. 进程线程
```Shell
    50. ps
        ps -ef |grep hadoop
        # 列出目前所有的正在内存当中的程序
        ps -aux |grep hadoop

    51. kill
        kill -9 pid
    52. pstree
        pstree
    53. top
        top –p PID
        M # memory
        P # CPU
    54. netstat
        -a (all)显示所有选项，netstat默认不显示LISTEN相关
        -t (tcp)仅显示tcp相关选项
        -u (udp)仅显示udp相关选项
        -n 拒绝显示别名，能显示数字的全部转化成数字。(重要)
        -l 仅列出有在 Listen (监听) 的服務状态
        -p 显示建立相关链接的程序名(macOS中表示协议 -p protocol)
        -r 显示路由信息，路由表
        -e 显示扩展信息，例如uid等
        -s 按各个协议进行统计 (重要)
        -c 每隔一个固定时间，执行该netstat命令

        # 1. 列出所有端口 (包括监听和未监听的)
        列出所有端口:     netstat -a
        列出所有tcp端口:  netstat -at
        列出所有udp端口:  netstat -au
        # 2. 列出所有处于监听状态的 Sockets
        只显示监听端口:          netstat -l
        只列出所有监听tcp端口:   netstat -lt
        只列出所有监听udp端口:   netstat -lu
        只列出所有监听UNIX端口:  netstat -lx
        # 3. 显示每个协议的统计信息
        显示所有端口的统计信息 netstat -s
        显示 TCP 或 UDP 端口的统计信息 netstat -st 或 -su
        # 4. 显示 PID 和进程名称
        netstat -pt
        # 5. 不显示主机，端口和用户名 (host, port or user)
        netstat -an

        # 如果只是不想让这三个名称中的一个被显示，使用以下命令
        netsat -a --numeric-ports
        netsat -a --numeric-hosts
        netsat -a --numeric-users

        # 6. 持续输出 netstat 信息
        netstat -t -c 2

        # 7. 显示系统不支持的地址族 (Address Families)
        netstat --verbose

        # 8. 显示核心路由信息
        netstat -rn

        # 9. 找出程序运行的端口
        netstat -apn | grep ssh
        netstat -an | grep ':22'

        # 10. 显示网络接口列表
        netstat -i
```

### 10. 定时任务
```Shell
    55. crond
        /etc/init.d/crond restart
    56. crontab
        crontab -e
        crontab -l
        # Example of job definition:
        # .---------------- minute (0 - 59)
        # |  .------------- hour (0 - 23)
        # |  |  .---------- day of month (1 - 31)
        # |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
        # |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
        # |  |  |  |  |
        # *  *  *  *  * user-name command to be executed
```

### 11. 软件包管理
```Shell
    57. rpm
        rpm -qa |grep vim
        rpm -Uvh xxx.rpm
    58. yum
        yum install -y epel-release
        yum remove -y xxx
        yum erase -y xxx
        yum install --installroot=/home/linou hadoop
        apt-get install mailutils
        yum clean all
        yum makecache
        yum repolist
```

### 12. 网络配置
```Shell
    59. ifconfig
        yum install net-tools -y
        ifconfig -a
        ifconfig eth0
        ifconfig eth0 up/down

    60. ping
        ping -c 5 -i 0.6 google.com
    61. hostname
        hostname -f
    62. getenforce/setenforce
        getenforce
        setenforce 0
```

### 13. 系统管理
```Shell
    63. shutdown/halt/reboot
        shutdown -h now
    64. ssh/scp
        ssh -p 20122 root@ip
        scp -P20122 source root@ip:target
    65. wget
        wget -c --header 'Cookie: oraclelicense=accept-securebackup-cookie' http://xxx.tar.gz
    66. lsof
        lsof -i:22
    67. systemctl
        systemctl set-default multi-user.target
        systemctl start mysql
        systemctl status mysql
        systemctl stop mysql
    68. umask
        umask
        umask 022
    69. telnet
        telnet ip port
    70. iotop
        apt-get install iotop
        yum install iotop
        iotop
            -o：只显示有io操作的进程
            -b：批量显示，无交互，主要用作记录到文件。
            -n NUM：显示NUM次，主要用于非交互式模式。
            -d SEC：间隔SEC秒显示一次。
            -p PID：监控的进程pid。
            -u USER：监控的进程用户。
    71. traceroute
        traceroute -r www.baidu.com
        traceroute -n -m 5 -q 4 -w 3 www.baidu.com
            -n 显示IP地址，不查主机名
            -m 设置跳数
            -q 4每个网关发送4个数据包
            -w 把对外发探测包的等待响应时间设置为3秒
    72. set
        set |grep

    73. vmstat --Virtual Meomory Statistics--虚拟内存统计
        vmstat 5

    74. sar
        # 1. CPU利用率
        sar -p
        sar -u 1 10

        # 2. 内存利用率
        sar -r
        sar -r 1 10

        # 3. 磁盘I/O
        sar -d
        sar -d 1 2

        # 4. 网络流量
        sar -n DEV
        sar -n DEV 1 2

    75. du
        du -h
        du -h --max-depth=1,2
    76. source
        source /etc/profile
    77. time
        time hdfs dfs -ls /
        time -v ps -aux
    78. free
        free -h
    79. uptime
        uptime
    80. export
        export JAVA_HOME=/usr/java/jdk
    81. env
    82. alias
        alias ll='ls -ltr'
        alias du1='du -h --max-depth=1'
        alias du2='du -h --max-depth=2'
        alias grep='grep --color=auto'
        unalias ll
    83. df
        df -h
    84. ssh-keygen
        ssh-keygen -t dsa
        ssh-keygen -t rsa
        ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.10
        cat ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
        chmod 600 ~/.ssh/authorized_keys
        ssh-keygen -t dsa -P '' -f  /etc/ssh/ssh_host_dsa_key
        ssh-keygen -t rsa -P '' -f  /etc/ssh/ssh_host_rsa_key
    85. read
        read -e -p "Enter Your Path:" -i "${HOME}/hadoop/" PATH
    86. mail
        echo "This will go into the body of the mail." | mail -s "Hello world" linou518@hotmail.com
        mail -s "hello" linou518@hotmail.com < file
    87. partprobe
        partprobe /dev/sdb
    88. blkid
        blkid /dev/sdb2
    89. iptables（4个表，5个链组成）
        iptables -L #查看规则
        iptables -F #清除预设表filter中的所有规则链的规则
        iptables -X #清除预设表filter中使用者自定链中的规则

        iptables -t row -L -n #查看row表里面的记录
        ###############配置filter表防火墙###############
        #开启22端口 （如果改了ssh的端口22，使用对应得端口）
        iptables -A INPUT -p tcp --dport 22 -j ACCEPT

        #如果OUTPUT设置成DROP需要添加
        iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT

        #关闭22端口
        iptables -D INPUT -p tcp --dport 22 -j ACCEPT

        #开启常用端口
        iptables -A INPUT -p tcp --dport 80 -j ACCEPT
        iptables -A INPUT -p tcp --dport 3306 -j ACCEPT
        iptables -A OUTPUT -p tcp --sport 80 -j ACCEPT
        iptables -A OUTPUT -p tcp --sport 3306 -j ACCEPT
        iptables -A INPUT -p tcp --dport 20 -j ACCEPT
        iptables -A INPUT -p tcp --dport 21 -j ACCEPT
        iptables -A INPUT -p tcp --dport 10000 -j ACCEPT
        iptables -A INPUT -p tcp --dport 25 -j ACCEPT
        iptables -A INPUT -p tcp --dport 110 -j ACCEPT
        iptables -A INPUT -p udp --dport 53 -j ACCEPT

        #允许ping
        iptables -A INPUT -p icmp -j ACCEPT

        #如果OUTPUT设置成DROP需要添加
        iptables -A OUTPUT -p icmp -j ACCEPT

        #允许loopback
        iptables -A INPUT -i lo -p all -j ACCEPT

        #如果OUTPUT设置成DROP需要添加
        iptables -A OUTPUT -o lo -p all -j ACCEPT

        #屏蔽指定ip
        iptables -A INPUT -p tcp -s 192.168.10.1 -j DROP

        #减少不安全的端口连接
        iptables -A OUTPUT -p tcp --sport 31337 -j DROP
        iptables -A OUTPUT -p tcp --dport 31337 -j DROP

        #允许某个IP远程连接
        iptables -A INPUT -s 192.168.10.1 -p tcp --dport 22 -j ACCEPT

        #允许某个网段的IP远程连接
        iptables -A INPUT -s 192.168.10.0/24 -p tcp --dport 22 -j ACCEPT

        #允许指定网段通过、指定网口通过SSH连接本机
        iptables -A INPUT -i eth0 -p tcp -s 192.168.10.0/24 --dport 22 -m state --state NEW,ESTABLESHED -j ACCEPT
        iptables -A OUTPUT -o eth0 -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT
        iptables -A INPUT -i eth0 -p tcp -s 192.168.10.0/24 --dport 22 -m state --state ESTABLESHED -j ACCEPT
        iptables -A OUTPUT -o eth0 -p tcp --sport 22 -m state --state NEW,ESTABLISHED -j ACCEPT

        #开启转发功能
        iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
        iptables -A FORWARD -i eth1 -o eh0 -j ACCEPT

        #丢弃坏的TCP包
        iptables -A FORWARD -p TCP ! --syn -m state --state NEW -j DROP

        #处理IP碎片数量,防止攻击,允许每秒100个
        iptables -A FORWARD -f -m limit --limit 100/s --limit-burst 100 -j ACCEPT

        #设置ICMP包过滤,允许每秒1个包,限制触发条件是10个包
        iptables -A FORWARD -p icmp -m limit --limit 1/s --limit-burst 10 -j ACCEPT

        #丢弃非法连接
        iptables -A INPUT -m state --state INVALID -j DROP
        iptables -A OUTPUT -m state --state INVALID -j DROP
        iptables -A FORWARD -m state --state INVALID -j DROP

        #允许所有已经建立的和相关的连接
        iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
        iptables -A OUTPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

        #设定默认规则，数据包的默认处规则
        iptables -P INPUT DROP
        iptables -P OUTPUT ACCEPT
        iptables -P FORWARD DROP

        ###############保存iptables文件，重启服务###############
        #保存iptables规则
        service iptables save

        #重启iptables服务
        service iptables restart

        ###############配置NAT表防火墙###############
        #查看NAT表规则
        iptables -t nat -L

        #清除NAT规则
        iptables -F -t nat
        iptables -X -t nat
        iptables -Z -t nat

        #防止外网用内网IP欺骗
        iptables -t nat -A PREROUTING -i eth0 -s 10.0.0.0/8 -j DROP
        iptables -t nat -A PREROUTING -i eth0 -s 172.16.0.0/12 -j DROP
        iptables -t nat -A PREROUTING -i eth0 -s 192.168.0.0/16 -j DROP

        #禁止与某个IP的所有连接
        iptables -t nat -A PREROUTING -d 192.168.10.1 -j DROP

        #禁用80端口
        iptables -t nat -A PREROUTING -p tcp --dport 80 -j DROP

    90. ldap
        ldapsearch -W passwd -D user@hostname
        ldapsearch -W -D systemjp-hadoop@lixilt.lan -b "OU=Users,dc=lixilt,dc=lan"

    91. last
        last -10

    92. watch
        watch uptime
        watch -d -n 1 netstat -ntlp
        watch -d 'ls -l |grep test'
        watch -n 1 'df -h'

    93. mkfs
        mkfs -t ext4 /dev/sda6 #将sda6分区格式化为ext4格式
        mkfs -t xfs /dev/sda6 #将sda6分区格式化为xfs格式
```

### 14. 三剑客
```Shell
    sed
        sed -i 's/192.168.56.101/192.168.56.10/g' /etc/sysconfig/network-scripts/ifcfg-enp0s3
        sed -i s/^SELINUX=.*$/SELINUX=disabled/ /etc/selinux/config
    grep
        grep "IPADDR" /etc/sysconfig/network-scripts/ifcfg-enp0s8
    awk
        awk '{print $3 "\t" $4}' `ls -l /`
```

### 15. rpm package
```Shell
    which rpmbuild
    mkdir -p ~/rpmbuild/BUILD
    mkdir -p ~/rpmbuild/BUILDROOT
    mkdir -p ~/rpmbuild/RPMS
    mkdir -p ~/rpmbuild/SOURCES
    mkdir -p ~/rpmbuild/SPECS
    mkdir -p ~/rpmbuild/SRPMS

    mv hadoop-3.0.0-alpha4.tar.gz ~/rpmbuild/SOURCES
    cat SPECS/hadoop3_0_0.spec
    %define pkg hadoop-3.0.0-alpha4
    Name:           hadoop
    Version:        3.0.0
    Release:        4
    Summary:        Apache Hadoop 3.0.0
    Group:          Apache/Hadoop
    License:        Apache License 2.0
    URL:            http://ftp.kddilabs.jp/infosystems/apache/hadoop/common/hadoop-3.0.0-alpha4/hadoop-3.0.0-alpha4.tar.gz
    Source0:        %{pkg}.tar.gz
    AutoReqProv:    no
    %undefine __arch_install_post
    %description
    Hadoop 3.0.0 rpm package created using tarball.
    %prep
    %setup -n %{pkg}
    %install
    rm -rf %{buildroot}
    mkdir -p %{buildroot}/usr/hdp/3.0.0/%{pkg}
    cp -r * %{buildroot}/usr/hdp/3.0.0/%{pkg}
    %clean
    rm -rf %{buildroot}
    %files
    %defattr(-,root,root)
    /usr/hdp/3.0.0/%{pkg}

    rpmbuild -bb SPECS/hadoop3_0_0.spec
```
