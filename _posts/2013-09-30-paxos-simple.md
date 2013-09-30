---
layout: post
title: Paxos Made Simple学习
categories: [distributed system, paper reading]
tags: [paxos, consensus]
---

![](http://www.lamport.org/leslie.jpg)

由资深大拿[Leslie Lamport](http://www.lamport.org/)所写的一篇分布式系统中很经典的论文，该论文中主要讲述分布式系统中常用到的Paxos算法或者协议。大拿就是大拿，在论文的Abstract中只是写了这么一句话:

> *The Paxos algorithm, when presented in plain English, is very simple.*

好多人看这篇论文都表示看不懂啊，真替自己的智商捉急啊！

在该论文中，主要是分成三个部分，第一部分是Introduction， 第二部分是论文的重点，讲述consensus algorithm（共识算法？），第三部分讲具体的状态机的实现

# Introduction

Paxos算法是实现容错分布式系统(*fault-tolerant distributed system*)中很重要的一个算法，该算法的核心即为Consensus算法。

# The Consensus算法

## 具体问题

Consensus含义：

1. 假设Processes集合能够propose values
2. Consensus算法就是确保从各个processes所proposed values中选择一个值
3. 若proposed values是空的，则不选择任何值
4. 若一个value被选择了，那么所有的processes都需要learn这个所选择的value

满足上面的Consensus所需要的条件:

* Only a value that has been proposed may be choosen
* Only a single value is chosen
* A process never learns that a value has been chosen unless it actually has been

在整个consensus算法中一共有3种agent，*proposers*, *acceptors*, *learners*. Agent和processes之间的映射关系不是Paxos算法关注的重点。Agent之间是通过发送消息(*sending messages*)来进行通信的，使用常见的异步、非拜占庭(*asynchronous, non-Byzantine model*)的分布式模型来介绍consensus算法，因此，Agent和通信会出现如下的问题:

* Agent可能以任意速度进行运行，有可能运行失败(fail by stopping), 也有可能会重启(may restart). 假设一个value被选择上了，但是此时所有的agent都失败了，在这种情况下，除非失败并且重启的agent记录下来一些信息，否则没法达到consensus。
* 通信过程中消息有可能会任意长的时间到达，也有可能会出现复制(duplicated),丢失(lost),但是这些消息不会出现被毁坏(corrupted)。

## 选择一个Value

最简单的办法来选择一个Value就是只有一个Acceptor agent， 这样Proposer都往Acceptor上发送Proposal, Acceptor就选择其第一个接收到的Proposal即可。虽然这种方式是最简单的，但是不能满足需求，假设Acceptor挂了的话，那么后续的操作就没法进行了，相当于有SPOF(Single point of failure)。

下面就换一种方式来选择一个Value，此时不只是有一个acceptor，而是有多个acceptor。这样proposer会往acceptor集合(a set of acceptors)发送proposal value, 而Acceptor有可能接收这个proposal，若有足够多的acceptors接收了该Proposal，那么就选择该value。(**The value is chosen when a large enough set of acceptors have accepted it**).那么多少个Acceptor算足够多了呢？ 这里面选择是大多数即可（A large enough set consist of any majority of the agents）。

在没有失败或者消息丢失的情况下，在只有一个value被一个proposer提出的情况下都能被选上，需要满足如下的条件:

1. P1. An acceptor must accept the first proposal that it receives
    
   只是满足P1这个条件的情况下会出现如下的问题
     - 在同时有很多的Proposer提出好几个不同的Proposal value，若每个acceptor只能接收一个proposal value的话，那么会出现没有一个proposal value被大多数的acceptors进行accept.
     - 若只有两个proposal value的情况下，每个proposal value都被差不多半数acceptors所接收，此时一个acceptor的失败都会使得不太能learn所选择的的那个value?? 原文: if each is accepted by about half the acceptors, failure of a single acceptor could make it impossible to learn which of the values was chosen.
 
   P1和其条件要求acceptor必须要接收到至少一个proposal，为了区分不同的proposal，需要给有可能接收的不同的proposal进行编号(a natural number)，因此，任何一个proposal都存在有编号和value两个属性. 我们允许选择多个proposal，但是必须要保证多个被选择上的proposal是有着相同的value。 通过引入proposal number之后，那么就又有P2条件必须要满足:

2. P2. If a proposal with value *v* is chosen, then every higher-numbered proposal that is chosen has value *v*

   由于编号是单调递增的，条件P2保证只是一个value被选择上。为了选上一个value，至少要被一个acceptor来接收这个proposal，因此，为了满足P2条件，又分解如下的子条件:

     - **P2(a) If a proposal with *v* is chosen, then every higher-numbered proposal accepted by any acceptor has value *v* **.  引入P2(a)的原因是，异步通信会使得有些acceptor会没有接收到本要被选择上的proposal，若此时有其他的proposer "醒来"并且发送一个更高的并且但是不是之前value的proposal，那么根据P1条件，这个acceptor就必须要accept这个proposal了，那么此时是同P2(a)冲突了。

     - **P2(b) If a proposal with *v* is chosen, then every higher-numbered proposal issued by any proposer has value *v* **. 所有的Proposal都是由Proposer发出来的，然后再由acceptor接收，因此，P2(b)满足，P2(a)也就能满足。

     - **P2(c ) For any *v* and *n*, if a proposal with value *v* and number *n* is issued, there is a set S consisting of a majority of acceptors such that either (a) no acceptor in S has accepted any proposal numbered less than *n*, or (b) *v* is the value of the highest-numbered proposal among all proposals numbered less than *n* accepted by the acceptors in S**. 此条件就要求Proposer在提proposal的时候，必须要了解其proposal的number.

这样就需要使用如下的算法来发起一个proposal. (1）首先Proposer选择一个新的proposal编号n发送给acceptors，并且要求这些acceptors做出如下的应答,(a)不再接收编号小于n的proposal了，（b）给出已经接收的编号小于n的最大的proposal。这request被称作为Prepare request。（2）Proposer接收到来自大多数acceptor的request应答之后，其就创建一个编号为n，value为v的proposal，其中v的选择是所有的应答中higher-numbered的proposal的值，若没有则随机地由proposer来选择一个即可。再将新的proposal发送给acceptor，这种叫做accept request。
          
上面介绍了Proposer的算法。那么acceptor的呢？Acceptor可以从Proposer那儿得到两种不同的Request，分别为prepare request和accept request。在Acceptor端又引入了P1(a)条件

P1(a) An acceptor can accept a proposal numbered n iff it has not responded to a prepare request having a number greater than n

到目前为止，选择一个value的算法已经完全地给出，在给出最终的算法之前，再考虑下如下的简单的优化:

> 假设一个acceptor已经应答了编号大于n的prepare request，若此时再来一个编号为n的prepare request，则直接忽略; 同样忽略掉已经接收的proposal的prepare request。

有了上面的优化之后，acceptor只需要记住这两个信息即可，(1) 已经接收的最高编号的proposal；（2）已经应答的最大编号的prepare request的编号。而proposer不需要记住这些信息。

将上面的Proposer和Acceptor的交互整理如下，则选择一个Value需要如下的两个阶段:

- Phrase 1. 
   * A proposer selects a proposal number n and sends a prepare request with number n to a majority of acceptors
   * If an acceptor receives a prepare request with number n greater than that of any prepare request to which it has already responded, then it responds to the request with a promise not to accept any more proposals numbered less than n and with the highest-numbered proposal (if any) that it has accepted.

- Phrase 2.
   * If the proposer receives a response to its prepare requests (numbered n) from a majority of acceptors, then it sends an accept request to each of those acceptors for a proposal numbered n with a value v, where v is the value of the highest-numbered proposal among the responses, or is any value if the responses reported no proposals
   * If an acceptor receives an accept request for a proposal numbered n, it accepts the proposal unless it has already responded to a prepare request having a number greater than n 

## 学习(Learning)所选择的Value

要学习所选择的Value，那Learner首先需要知道被大多数的acceptor接收的Proposal是啥。最直观的办法是，若一个Acceptor接收到了一个Proposal，那么将这个接收的Proposal告诉Learner即可。这样Learner可以快速地知道接收了啥Proposal，但是弊端是存在有Acceptor个数*Learner个数的应答消息，消息数比较多。

为了解决上面的那个问题，在非拜占庭失败的情况下，一个Learner可以很容易地从其他的Learner得到所接收到得Value，因此，在这样的前提条件下，只需要确定一个Distinguished Learner，这样Acceptor只需要告诉Distinguished Learner，然后由Distinguished Learner和其他的Learner进行同步来着，需要传递的消息数变成Acceptor个数+Learner个数。当然，由于Distinguished Learner有可能会失败，这种方案没有上面那种方案可靠。

有了上面的两个极端的情况，在实际中，一般会取一个Distinguished Learner集合来着，这个集合越大，则越可靠。

## Progress

由于整个Choose Value过程有两步，因此会存在两个不同的Process不断地提高自己的proposal的编号来进行Propose，这样很容易造成死锁。

为了解决上述问题，对Proposer中也选出一个distinguished proposer来，只由distinguished proposer来发起proposal。