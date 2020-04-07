---
layout: post
title: "java之深拷贝与浅拷贝"
date: 2019-04-17 14:48
comments: true
tags: 
	- java
	
---


`浅拷贝`：①对于数据类型是基本数据类型的成员变量，浅拷贝会直接进行值传递，也就是将该属性值复制一份给新的对象。因为是两份不同的数据，所以对其中一个对象的该成员变量值进行修改，不会影响另一个对象拷贝得到的数据。②对于数据类型是引用数据类型的成员变量，比如说成员变量是某个数组、某个类的对象等，那么浅拷贝会进行引用传递，也就是只是将该成员变量的引用值（内存地址）复制一份给新的对象。因为实际上两个对象的该成员变量都指向同一个实例。在这种情况下，在一个对象中修改该成员变量会影响到另一个对象的该成员变量值。
<!--more-->
`深拷贝`：深拷贝对引用数据类型的成员变量的对象图中所有的对象都开辟了内存空间；而浅拷贝只是传递地址指向，新的对象并没有对引用数据类型创建内存空间。
测试代码如下：[啰里吧嗦，直接查看总结](#jump)

```实体类```

	package cn.edu.nuaa.qjj.domain;

	public class Location implements Cloneable {
	private int localId;// 区域的编号（0、1....N）
	private int[] load = new int[3];// 三个时间段的平均负载情况
	private double ownCapacity;// 实际分配的计算能力，两种情况；有协作和无协作

	public int getLocalId() {
		return localId;
	}

	// 实现对象的浅复制
	@Override
	public Location clone() {
		// TODO Auto-generated method stub
		Location loc = null;
		try {
			loc = (Location) super.clone();
		} catch (Exception e) {
			e.printStackTrace();
		}

		return loc;
	}

	public void setLocalId(int localId) {
		this.localId = localId;
	}

	public int[] getLoad() {
		return load;
	}

	public void setLoad(int[] load) {
		this.load = load;
	}

	public double getOwnCapacity() {
		return ownCapacity;
	}

	public void setOwnCapacity(double ownCapacity) {
		this.ownCapacity = ownCapacity;
	}

	// 不包含设置计算能力的构造方法
	public Location(int localId, int[] load) {
		super();
		this.localId = localId;
		this.load = load;
	}

	public Location() {
		// TODO Auto-generated constructor stub
	}
	}


```main测试类```
	
	package cn.edu.nuaa.qjj.util;
	import java.util.ArrayList;
	import java.util.Arrays;

	import cn.edu.nuaa.qjj.domain.Location;
	import cn.edu.nuaa.qjj.genera.GraphGen;
	import cn.edu.nuaa.qjj.genera.LocationGen;
	import cn.edu.nuaa.qjj.main.MainSchedule;

	public class SortDemo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		new GraphGen().graphGen();// 生成无向图
		new LocationGen().locationGen();// 生成区域点的信息
		ArrayList<Location> ll = new ArrayList<Location>();
		// 实现对象的浅复制，但是对于引用对象数组必须单独copy，因为浅复制没有拷贝引用对象
		for (Location l : MainSchedule.loaclList) {
			int[] list = l.getLoad().clone();
			Location lo = l.clone();
			lo.setLoad(list);
			ll.add(lo);
		}
	//		ArrayList<Location> ll = MainSchedule.loaclList;
		for (Location l : ll) {
			Arrays.sort(l.getLoad()); // 把这个区域的3个负载升序排序
		}
		for (Location l : ll) {
			System.out.println(l.getLoad()[0] + "," + l.getLoad()[1] + "," + l.getLoad()[2]);
		}
		System.out.println("没有排序以前的负载情况如下：");
		for (Location lc : MainSchedule.loaclList) {
			System.out.println(lc.getLoad()[0] + "," + lc.getLoad()[1] + "," + lc.getLoad()[2]);
		}
	}

	}

```结果如下：```
	
	100,500,500
	200,400,600
	300,400,800
	300,400,600
	100,400,500
	没有排序以前的负载情况如下：
	500,100,500
	200,600,400
	300,800,400
	400,600,300
	100,500,400

<span id='jump'>总结：Location对象中包含不是基本数据类型的数组引用类型，在传统的浅clone的情况下是不能完成整个对象的复制的，有两种方法可以解决：一种是直接实现深clone，另一种是直接像我上面的一样在浅clone之后，再单独手动clone引用类型，并手动赋值进去。</span>
