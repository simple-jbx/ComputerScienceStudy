# 操作系统

## 操作系统概述

### 分时与实时操作系统

<div align='center'>
    <img src='./imgs/differTimeOS.png' width='800px'>
    <br/><br/>分时与实时系统
</div>


分时操作系统：主流PC机器，服务器

实时操作系统：单片机，电梯控制系统、飞机、导弹、卫星

Linux分时操作系统，可以改成实时，例如UCOS

## 进程

<div align='center'>
    <img src='./imgs/OS/ProcessState.png' width='800px'>
    <br/><br/>进程状态转移图
</div>



## 进程通信

### 管道

### 系统IPC（消息队列、信号量、信号、共享内存）

### Socket

## 调度算法

### 先来先服务（FCFS）

### 短作业优先（SJF）

### 优先级调度算法

###  高响应比优先调度算法

​	响应比 R_p = (等待时间  + 要求服务时间)  /  要求服务时间

### 时间片轮转

### 多级反馈队列

​	时间片片轮转 + 优先级调度

## 进程同步

### 信号量

P（wait(s)）、V(signal(s))操作

### 管程

### 经典同步问题

- 生产者-消费者问题

<div align='center'>
    <img src='./imgs/20210825175315.png' width='1000px'>
</div>


- 读者-写者问题

<div align='center'>
    <img src='./imgs/20210826195211.png' width='1000px'>
    <img src='./imgs/20210826195315.png' width='1000px'>    
	</br></br>读进程优先
</div>




<div align='center'>
    <img src='./imgs/20210826195514.png' width='1000px'>
    <img src='./imgs/20210826195546.png' width='1000px'>    
    </br></br>读写公平法
</div>


- 哲学家进餐问题

<div align='center'>
    <img src='./imgs/20210826201858.png' width='1000px'>
    <img src='./imgs/20210826201940.png' width='1000px'>    
    </br></br>哲学家进餐
</div>




- 吸烟者问题

<div align='center'>
    <img src='./imgs/20210826202342.png' width='1000px'>
    <img src='./imgs/20210826202715.png' width='1000px'>    
    </br></br>吸烟者问题
</div>


## 死锁

### 死锁的处理策略

<div align='center'>
    <img src='./imgs/20210826202907.png' width='1000px'>
    </br></br>死锁的处理策略
</div>


<div align='center'>
    <img src='./imgs/20210826203319.png' width='1000px'>
    </br></br>死锁的预防
</div>




<div align='center'>
    <img src='./imgs/20210826203442.png' width='1000px'>
    <img src='./imgs/20210826203627.png' width='1000px'>
    <img src='./imgs/20210826203743.png' width='1000px'>
    </br></br>死锁避免
</div>




<div align='center'>
    <img src='./imgs/20210827104730.png' width='1000px'>
    <img src='./imgs/20210827104846.png' width='1000px'>
    <img src='./imgs/20210827104905.png' width='1000px'>
    <img src='./imgs/20210827104929.png' width='1000px'>
    <img src='./imgs/20210827105022.png' width='1000px'>
    </br></br>死锁避免
</div>




<div align='center'>
    <img src='./imgs/20210826203859.png' width='1000px'>
    <img src='./imgs/20210826203937.png' width='1000px'>
    </br></br>死锁检测和解除
</div>


## 页面置换算法

### 最佳（OPT）置换算法

### 先进先出（FIFO）页面置换算法

### 最近最久未使用（LRU）置换算法

<div align='center'>
    <img src='./imgs/LRU.png' width='1000px'>
    </br></br>LRU
</div>

### 时钟（Clock）置换算法（NRU ）

<div align='center'>
    <img src='./imgs/20210903154002.png' width='800px'>
    <img src='./imgs/20210903154019.png' width='800px'>
    </br></br>NRU
</div>

### 操作系统IO模型