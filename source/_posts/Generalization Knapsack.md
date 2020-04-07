---
layout: post
title: "Algorithm-Generalization Knapsack"
date: 2020-04-01 14:48
comments: true
tags: 
	- 算法
	
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# 泛化背包问题的描述

> 有M个区域，每个区域要投入计算能力不同的云计算服务器，每个区域的收益在投入的某个计算能力之前呈正比例增长，比如其中的一个区域为效益为

$$
\chi(j) =
\begin{cases}
3 x, & \text{x $<$= 200;} \\
2x+200, & \text{200$<$ x $<$400;}\\
x+600, & \text{400$<$ x $<$= 600;}\\
1200, & \text{x$>$600.}
\end{cases}
$$
<!--more-->
 
>假设我一共有的计算能力为S的服务器，求我如何分配M个区域的计算能力使得总的收益最大？（给出具体的方案就行）
