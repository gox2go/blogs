---
title: Learning Node
date: 2020-05-26 16:21:03
tags: [Node]
---

# Learning Node 01

## 一、What is Node? Node是什么？

Node是一个`platform`或者`running enviroment`.可以是`javascript`在`server`端跑起来。

<!-- more -->

### 1.凭什么？

### 2.是个什么？

 Node 采用`event-driven,asynchronous I/O model`使得他很轻量高效----凭什么这么说?

#### 2.1 看一眼Apache（Tomcat）和Nginx.

Nginx 采用 asynchronous and evented approach

Apach 使用 阻塞式多线程方式

### 3.用在哪里

因为NodeJS的设计方式，十分实用一些`DIRTy applications -- Data-intensive real-time applications `。

#### 3.1 asynchronous

#### 3.2 run server

#### 3.3 stream data

 关于steam data. 可以想象成`Array`。相比Array是空间上分布；stream data则是时间上分布(distributed over time). --- 意义在于 （不用等全部数据传输完毕，）我们可以按接收到的一块一块的数据来处理。

