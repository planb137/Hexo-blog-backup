---
layout: post
title: "K8s架构概述"
date: 2020-04-05 21:03
comments: true
tags: 
	- k8s
---


1、Kubernetes架构

![](https://s1.ax1x.com/2020/04/05/GrkIPI.png)
<!--more-->
2、Kubernetes Master

![](https://s1.ax1x.com/2020/04/05/GrAeiR.png)



3、Kubernetes Node

![](https://s1.ax1x.com/2020/04/05/GrAfyT.png)

4、Deploying a Pod

![](https://s1.ax1x.com/2020/04/05/GrAbf1.png)



5、Service - Exposing Services

![](https://s1.ax1x.com/2020/04/05/GrESTH.png)

6、Kubernetes Labels&Selector

![](https://s1.ax1x.com/2020/04/05/GrE3cV.png)



7、Kubernetes Ingress

![](https://s1.ax1x.com/2020/04/05/GrEUAJ.png)

8、Kubernetes PersistentStorage

![](https://s1.ax1x.com/2020/04/05/GrEdhR.png)

9、Kubernetes Overview

![](https://s1.ax1x.com/2020/04/05/GrEcHe.png)

# Kubernetes Features

## 多租户

1. Namespace
2. ServiceAccount
3. Role
4. RoleBind
5. ClusterRole
6. ClusterRoleBind
7. NetworkPolicy
8. ResourceQuota
9. Secret  

## 调度

1. Node的亲和性和反亲和性(Affinity/Anti-Affinity)
2. Node的污点和容忍(Taints and Tolerations)
3. Pod的的亲和性和反亲和性(Affinity/Anti-Affinity)
4. 自定义调度器（基于框架的调度器）

## APP访问

- 外部访问
  1. Service NodePort
  2. Ingress
  3. Keepalived + Service NodePort
  4. Service - Loadbalance
  5. Loadbalance + 云提供商
- 内部访问
  1. Service Cluster ip
  2. 在Pod中使用Service name

## APP高可用

1. Replication Controller
2. Deployment + Replica Set
3. Service expose 服务去代理Pod
4. Deployment的灰度发布／版本控制／回滚
5. Daemon Set
6. Stateful Set
7. Keepalived
8. Autoscale

## 服务发现

1. Service
2. Ingress
3. Kube-Dns

## 权限

1. ServiceAccount
2. Role
3. RoleBind
4. ClusterRole
5. ClusterRoleBind

## 网络（提供接口由第三方以插件实现）

1. Weave
2. Calico
3. Cni
    a. bridge plugin               d. ipvlan                g. tuning
    b. dhcp plugin                  e. macvlan
    c. host local                       f.  ptp

## 存储

1. StorageClass
2. PersistentVolume
3. PersistentVolumeClaim
4. ConfigMap
    支持：NFS,Scaleio,PX,Ceph,Vsphere,Glusterfs,GCE,AWS,HostPath,EmptyDir

## 监控

1. Heapster +  influxdb+grafana
2. Promethues + Kubelet + NodeExporter
3. Sysdig
4. Kube-state-metrics  +  Promethues

## 集群高可用

1. Etcd集群
2. 多api server提供Load Balance
3. 多ControllerManager（master/standby机制）
4. 多Scheduler（master/standby机制）

![](https://s1.ax1x.com/2020/04/05/Grm6xI.png)











