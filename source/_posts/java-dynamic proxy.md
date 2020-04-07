---
layout: post
title: "动态代理"
date: 2020-04-01 14:48
comments: true
tags: 
	- java
	
---


   代理模式是为了提供额外或不同的操作，而插入的用来替代”实际”对象的对象，这些操作涉及到与”实际”对象的通信，因此代理通常充当中间人角色。Java的动态代理比代理的思想更前进了一步，它可以动态地创建并代理并动态地处理对所代理方法的调用。在动态代理上所做的所有调用都会被重定向到单一的调用处理器上，它的工作是揭示调用的类型并确定相应的策略。以下是一个动态代理示例：
   
   接口和实现类：
   	
	public interface Interface {
    void doSomething();
    void somethingElse(String arg);
	}
	public class RealObject implements Interface {
    public void doSomething() {
        System.out.println("doSomething.");
    }
    public void somethingElse(String arg) {
        System.out.println("somethingElse " + arg);
    }
	}
<!--more-->
   动态代理对象处理器：
   
    	public class DynamicProxyHandler implements InvocationHandler {
    private Object proxyed;
    
    public DynamicProxyHandler(Object proxyed) {
        this.proxyed = proxyed;
    }
    
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws IllegalAccessException, IllegalArgumentException, InvocationTargetException {
        System.out.println("代理工作了.");
        return method.invoke(proxyed, args);
    }
	}

测试类：

	public class Main {
    public static void main(String[] args) {
        RealObject real = new RealObject();
        Interface proxy = (Interface) Proxy.newProxyInstance(
                Interface.class.getClassLoader(), new Class[] {Interface.class},
                new DynamicProxyHandler(real));
        
        proxy.doSomething();
        proxy.somethingElse("luoxn28");
    }
 	}
 	
 通过调用Proxy静态方法Proxy.newProxyInstance()可以创建动态代理，这个方法需要得到一个类加载器，一个你希望该代理实现的接口列表(不是类或抽象类)，以及InvocationHandler的一个实现类。动态代理可以将所有调用重定向到调用处理器，因此通常会调用处理器的构造器传递一个”实际”对象的引用，从而将调用处理器在执行中介任务时，将请求转发。