# Linux

# 常用命令

## 查看磁盘/内存使用情况

```shell
# 查看磁盘剩余空间
# df -B[K M G T P E Z Y]
(base) [snnuxjb@mu01 ~]$ df -BT
文件系统       1T-块  已用  可用 已用% 挂载点
devtmpfs          1T    0T    1T    0% /dev
tmpfs             1T    0T    1T    0% /dev/shm
tmpfs             1T    1T    1T    1% /run
tmpfs             1T    0T    1T    0% /sys/fs/cgroup
/dev/sda3         1T    1T    1T   46% /
/dev/sda1         1T    1T    1T    3% /boot/efi
/dev/sdb         22T    8T   13T   38% /home
tmpfs             1T    0T    1T    0% /run/user/1004
tmpfs             1T    0T    1T    0% /run/user/1002
```

```shell
# 查看内存使用情况
# free -[b k m g]
(base) [snnuxjb@mu01 ~]$ free -g
              total        used        free      shared  buff/cache   available
Mem:             62           2          14           0          45          59
Swap:            31           0          31

```

## 查看端口占用情况

## 过滤器

### grep 打印匹配行

grep pattern [file...] 简单模式：用来找到文件中的匹配文本

head/tail 打印文件开头/结尾部分

```shell
head -n 5 output.txt # 打印前5行
tail -n 5 output.txt # 打印后5行
tail -f output.txt # 循环打印（查看日志，打印文件末尾最新内容）
```



### 权限

chmod 更改文件模式

chown 更改文件所有者和用户组

### 进程

#### ps 报告当前进程快照

常用 ps aux

#### top 显示任务

#### jobs 列出活跃任务

#### kill 给一个进程发送信号

| 编号 | 名字 | 作用 |
| ---- | ---- | ---- |
| 1    | hup  | 挂起 |
| 2    | int  | 中断 |
| 9    | kill | 杀死 |
| 15   | term | 终止 |
| 18   | cont | 继续 |
| 19   | stop | 停止 |



#### killall 杀死指定名称的进程

#### shutdown 关机或重启系统

### 网络

#### ping

#### ssh

#### ftp

#### 查看端口号占用

nestat lsof

```shell
# netstat -tunlp|grep 10000

[snnuxjb@mu01 ~]$ netstat -tunlp|grep 10000
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp        0      0 0.0.0.0:10000           0.0.0.0:*               LISTEN      4463/python         
tcp6       0      0 :::10000                :::*                    LISTEN      4463/python 


# lsof -i:10000
(base) [snnuxjb@mu01 ~]$ lsof -i:10000
COMMAND    PID    USER   FD   TYPE    DEVICE SIZE/OFF NODE NAME
jupyter-n 4463 snnuxjb    5u  IPv4  54493055      0t0  TCP *:ndmp (LISTEN)
jupyter-n 4463 snnuxjb    6u  IPv6  54493056      0t0  TCP *:ndmp (LISTEN)
jupyter-n 4463 snnuxjb   10u  IPv4 112158367      0t0  TCP mu01:ndmp->10.150.195.154:vchat (ESTABLISHED)
jupyter-n 4463 snnuxjb   13u  IPv4 112158372      0t0  TCP mu01:ndmp->10.150.195.154:ms-sql-m (ESTABLISHED)

```

### 查找文件

locate 通过名字查找文件

find 在目录层次结构中搜索文件

### 压缩文件

tar

zip

rar

# 计算机硬件软件体系

## 冯诺依曼体系结构

- 计算机处理的数据和指令一律用二进制表示
- 顺序执行程序
- 计算机硬件由运算器、控制器、存储器、输入和输出设备五大部分组成

## 计算机硬件组成

- 输入设备
- 输出设备
- 存储器
	- 存放数据和程序
	- RAM 速度快，容量小 掉电易失 逻辑IO
	- ROM 容量大，速度慢  长久保存 物理IO
- CPU（中央处理器）
	- 控制器
	- 运算器

## 硬盘分类

### 存储介质不同分类

- 机械硬盘（Hard Disk Driver，HDD）
- 固态硬盘（Solid State Disk，SSD）

## 顺序读写与随机读写

每个扇区能存的数据是等大的

扇区编号是不连续的（1 10 2 9 3 8 4 7 5 6）

## 网络

- IP地址
- 子网掩码
- 默认网关
- 域名服务器（DNS）

## 网络连接模式

### 桥接模式

虚拟机和宿主机之间的关系就像连接在同一个Hub上的两台电脑

有可能会产生IP冲突

### NAT（网络地址转换）

## 软件分类

应用软件

系统软件