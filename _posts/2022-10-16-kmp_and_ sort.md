---
title: 「一本通 1.2 练习 2」扩散（经典题型精析）
date: 2022-10-16 9:48 +0800
categories: [随笔]
tags: [学习]
pin: true
author: Duyin

toc: true
comments: true

math: false
mermaid: true

typora-root-url: ./..
---

# kmp实现最短路

## **题目描述**

一个点每过一个单位时间就会向 4 个方向扩散一个距离，如图所示：两个点 *a* 、*b* 连通，记作 *e*(*a*,*b*)，当且仅当 *a* 、*b* 的扩散区域有公共部分。连通块的定义是块内的任意两个点 *u*、*v* 都必定存在路径*e*(*u*, \sqrt{a} ),*e*(*a*0,*a*1),…*e*(*a**k*,*v*)。

给定平面上的 n*n* 个点，问最早什么时候它们形成一个连通块。

```c++
include<iostream>
using namespace std;

int main(){
  printf("Hello world~");
  return 0;
}
```

这是我博客的第一篇文章，也是一个开始，感谢 @汤姆还在写代码 提供的博客框架，以及博客使用方法