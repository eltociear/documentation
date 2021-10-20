---
title: htop - 进程管理
author: tianci li
contributors: Steven Spencer
date: 2021-10-16
---

# 安装 `htop`
每个系统管理员都喜欢用一些比较常用的命令，今天推荐的是htop，作为 `top` 命令的替代。要正常使用 `htop`命令，需要首先安装它。
``` bash
# 安装epel源（也叫存储库）
dnf -y install epel-release 
# 生成缓存
dnf makecache
# 安装htop
dnf -y install htop
```
# 使用 `htop`
您只需要在终端中键入 htop 即可，交互界面如下：
```
0[ |||                                            3%]       Tasks: 24,  14thr;  1  running
1[ |                                              1%]       Load average:  0.00  0.00  0.05
Mem[ |||||||                                 197M/8G]       Uptime:  00:31:39
Swap[                                        0K/500M]

PID   USER   PRI   NI   VIRT   RES   SHR   S    CPU%   MEM%   TIME+   Commad(merged)
...
```
<kbd>F1</kbd>Help      <kbd>F2</kbd>Setup       <kbd>F3</kbd>Search       <kbd>F4</kbd>Filter        <kbd>F5</kbd>Tree      <kbd>F6</kbd>SortBy        <kbd>F7</kbd>Nice        <kbd>F8</kbd>Nice+      <kbd>F9</kbd> Kill       <kbd>F10</kbd> Quit

## 顶部说明
* 最上面的0和1，表示你的CPU核心数，百分比表示单个内核的占用率（当然也可以显示CPU总的占有率）
    * 进度条的不同颜色表示不同的进程类型占有百分比

        | 颜色 | 说明 |
        | ---------| ------------|
        | 蓝色 | 低优先级进程使用的CPU百分比                  |
        | 绿色 | 普通用户拥有的进程CPU百分比                  |
        | 红色 | 系统进程使用的CPU百分比                  |
        | 橙色 | IRQ时间使用的CPU百分比                  |
        | 洋红色（Magenta） | 软IRQ时间使用的CPU百分比                  |
        | 灰色 | IO等待时间占用的CPU百分比                 |
        | 青色 | Steal time消耗的CPU百分比                  |

* Tasks: 24, 14thr;  1 running，进程信息。在我的示例中，表示我的当前机器有24个任务，它们被分成14个线程，其中只有1个进程处于正在运行的状态。
* Mem内存与swap信息。同样的，也用不同颜色区分

    | 颜色|说明|
    |----|----|
    |蓝色|缓冲区消耗的内存百分比   |
    |绿色|内存区消耗的内存百分比   |
    |橙色|缓存区消耗的内存百分比   |

* Load average，三个值分别表示了系统在最后1分钟、最后5分钟、最后15分钟的平均负载
* Uptime，表示开机以后的运行时间

## 进程信息说明
* **PID - 进程的ID号**
* USER - 进程的所有者
* PRI - 显示Linux 内核所能看到的进程优先级
* NI -  显示了普通用户或root超级用户reset的进程优先级
* VIRI - 一个进程正在消耗的虚拟内存
* **RES - 一个进程正在消耗的物理内存**
* SHR - 一个进程正在消耗的共享内存
* S - 进程的当前状态，有一种特殊的状态要注意！也就是 Z （僵尸进程）。当机器中有大量的僵尸进程时，会影响机器性能的下降。
* **CPU%  - 每个进程消耗的CPU百分比**
* MEM%  - 每个进程消耗的内存百分比
* TIME+ - 显示了自进程开启以来的运行时间
* Commad - 进程所对应的命令

## 快捷键说明
在交互界面，按<kbd>F1</kbd>按键，即可看到对应的快捷键说明。

* 上下左右的方向按键，可以在交互界面滚动，<kbd>space</kbd> 则可以对对应的进程进行标记，以黄色进行标识。
* <kbd>N</kbd>按键 、<kbd>P</kbd>按键 、<kbd>M</kbd>按键 <kbd>T</kbd>按键，分别以PID、CPU%、MEM%、TIME+进行排序，当然，你也可以用鼠标点击的方式，以某个字段升序或降序排列。

## 其他常用
对进程进行管理，使用<kbd>F9</kbd>按键，可以对进程发送不同的信号，信号列表可以在`kill -l`中找到，比较常用的有：

| 信号 | 说明|
|---|---|
|1  | 让进程立刻关闭，然后重新读取配置文件后重启  |
|9  | 用来立即结束程序的运行，用来强制终止进程，类似windows任务栏当中的强制结束 |
|15  |  kill 命令的默认信号。有时如果进程已经发生问题，这个信号时无法正常终止进程的，我们会尝试信号9 |

## 结尾
`htop`比系统自带的`top`好用太多，更加直观，对日常的使用提升非常巨大，这也是为什么通常在安装操作系统后，第一件事就是将其安装上。