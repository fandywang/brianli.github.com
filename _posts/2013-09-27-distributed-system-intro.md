---
layout: post
title: 分布式系统简介
categories: [distributed system]
tags: [middleware, distributed system]
---

任何一个分布式系统，都需要从如下方面进行考虑:

* Architectures
* Processes
* Communication
* Naming
* Synchronization
* Consistency and Replication
* Fault Tolerance
* Security


# Definition of a distributed system

*A distributed system is a collection of independent computers that appears to its users as a single coherent system*

从上面的定义可以看出，如下的重要的点:

* consists of components (i.e. computers) that are autonomous (各个组件是自治的)
* 但是从使用者的角度来看，以为他们是操作了a single system.
* 系统层面屏蔽掉分布式系统下各个Process/Machine的通信， 用户是感知不到的
* 整个分布式系统是consistent的
* 整个系统是容易扩展expand and scale

分布式系统也经常被称作为**MiddleWare**. 为啥是middleware了呢？ 因为其位于整个系统的中间，

- 上层为higher-level layer consisting of users and applications
- 下层为a layer underneath consisting of operating systems and basic communication facilities

# 分布式系统的4大目标 4 Goals

## Making Resources Accessible

Resources可以是任何东西，如printers/computers/storage facilities, data files, web pages and networks.

随着共享地进行，也会出现如下的问题，即安全性(security)问题

## Distributed Transparency (分布式透明)

Reasonably hide the fact that resources are distributed across a network. 就是用户感觉不到当前的resource是分布式的还是集中式的, 该处理的processes和resources是分布在各个机器上。

下面是各个transparency的定义:

Transparency | Description
-------------|---------------
Access       | Hide differences in data representation and how a resource is accessed
Location     | Hide where a resource is located
Migration    | Hide that a resource may move to another location
Relocation   | Hide that a resource may be moved to another location while in use
Replication  | Hide that a resource is replicated
Concurrency  | Hide that a resource may be shared by several competitive users
Failure      | Hide the failure and recovery of a resource

## Openness

分布式系统必须是开放的，开放的含义是offers services according to standard rules that describe the syntax and semantics of those services.

## Scalability （可扩展性）

一个系统的可扩展性，至少有如下的3个维度

1. Size, 这样可以很方便地增加resources
2. Geographically scalable system
3. Administratively scalable

# 设计分布式系统的pitfalls

1. The network is reliable
2. The network is secure
3. The network is homogeneous
4. The topology does not change
5. Latency is zero
6. Bandwidth is infinite
7. Transport cost is zero
8. There is one administrator

# 分布式系统的类型

1. distributed computing system (cluster computing system, grid computing system. 即cluster基本上是同质的，相同的操作系统、网络等，而grid computing systems就没有这些限制)
2. distributed information system (Transaction Processing Systems, 主要的是transaction， 而transaction主要的是ACID特性；另外一个就是Enterprise Application Integration, 主要是RPC/RMI等)
3. distributed embedded system （Distributed Pervasive System）

