---
layout:     post
title:      GBN协议&SR协议
subtitle:   Go-back-N(GBN)协议&选择性重复(SR)协议的区别
date:       2021-1-14
author:     Zhao Zihao
header-img: img/5.jpeg
catalog: false
tags:
    - 计算机网络
---

#### 主要区别

GBN协议使用累积确认，其中发送方有缓冲区而接收方没有；发送方同时对封装的包进行计时。

SR协议的接收方对每个包都发送一个单独的ACK，发送方和接收方都有一个缓冲区，发送方为每个未封装的包维护一个计时器。

##### 1 接收缓冲区
对于GBN协议，由于它丢弃了接收端窗口内所有无序的数据包，所以不需要有一个缓冲区来存储接收端窗口内的无序数据包。接收端窗口大小为1。
对于SR协议，由于避免了重传许多不必要的数据包，所以接收端需要对无序的数据包进行缓冲，其中窗口大小为N。

##### 2 实现的复杂性
GBN协议采用累积确认的方法。因为接收方是按顺序接收数据包的，所以任何错误到达的数据包都将被接收方丢弃。
SR协议避免了GBN协议以同时设置发送方窗口和接收方窗口大小相同为代价，重传到达接收方的正确数据包。所以基本上，SR协议比GBN更复杂，因为SR协议的接收者需要一个缓冲区来确认是否正确地接收了包，不管它是否有序。

##### 3 网络效率

SR协议比GBN协议更有效。GBN协议,当窗口大小和带宽的乘积延迟很大,一个数据包在错误可能导致GBN重新发送大量的数据包,而在SR协议,许多正确的数据包的顺序在这个过程中不需要重传。

用表格的形式来列举两种协议的区别：

| 协议 | sender buffer | 接受端buffer | 数据分组计时 | 是否接收正确的乱序package | 采用累积确认 |
| ---- | ------------- | ------------ | ------------ | ------------------------- | ------------ |
| GBN  | 是            | 否           | 同步计时     | 否                        | 是           |
| SR   | 是            | 是           | 独立计时     | 是                        | 否           |

