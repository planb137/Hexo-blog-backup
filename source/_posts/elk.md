---
layout: post
title: "ELK初探"
date: 2020-04-05 20:26
comments: true
tags: 
	- ELK
  - k8s
---

## 1、Logspout + Graylog
## 2、ELK（EFK）

#### ELK典型架构
![](https://s1.ax1x.com/2020/04/05/Grpqrd.png)
<!--more-->
#### ELK优化架构
![](https://s1.ax1x.com/2020/04/05/Gr9WQg.png)

#### ELK — Kafka  VS Redis
  `ELK Stack 官网建议使用 Redis 来做消息队列，但是正常建议采用 Kafka。 `

| 数据丢失                                               | 数据堆积                                                     |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| 1.Redis 队列多用于实时性较高的消息推送，并不保证可靠； | 1.Redis 队列容量取决于机器内存大小，如果超过设置的Max memory，数据就会抛弃； |
| 2.Kafka保证可靠但有点延时。                            | 2.Kafka 的堆积能力取决于机器硬盘大小。                       |

#### ELK部署方式

1. 传统分布式部署
2. Docker方式部署
3. 在K8S集群中部署（Helm）

