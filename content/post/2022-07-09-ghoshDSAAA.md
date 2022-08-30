---
title: "笔记：Distributed Systems An Algorithmic Approach 2nd（更新到第9章）"
date: 2022-07-09T01:31:57+08:00
draft: false
tags: ["教程", "读书笔记"]
isCJKLanguage: true
---

- [Ghosh, Sukumar (2014) - Distributed Systems An Algorithmic Approach](#ghosh-sukumar-2014---distributed-systems-an-algorithmic-approach)
    - [Ch 1 introduction](#ch-1-introduction)
    - [Ch 2 interprocess communication](#ch-2-interprocess-communication)
        - [naming](#naming)
        - [rpc remote procedure call](#rpc-remote-procedure-call)
        - [cloud computing](#cloud-computing)
            - [MapReduce](#mapreduce)
    - [ch 3 Models for Communication](#ch-3-models-for-communication)
        - [shared variable 共享变量](#shared-variable-共享变量)
            - [linda](#linda)
            - [mobile agent](#mobile-agent)
        - [模型强弱](#模型强弱)
            - [resequencing protocol, 非 FIFO 模拟 FIFO](#resequencing-protocol-非-fifo-模拟-fifo)
            - [共享变量模拟信息传递](#共享变量模拟信息传递)
            - [信息传递模拟共享变量](#信息传递模拟共享变量)
            - [信道判空的不可能性](#信道判空的不可能性)
        - [系统分类](#系统分类)
        - [复杂度计数](#复杂度计数)
    - [ch 4 Representing Distributed Algorithms](#ch-4-representing-distributed-algorithms)
        - [公正性](#公正性)
        - [scheduler](#scheduler)
    - [ch 5 Program Correctness](#ch-5-program-correctness)
        - [断言推理 assertional reasoning](#断言推理-assertional-reasoning)
        - [良基集 well-founeded set](#良基集-well-founeded-set)
        - [时间逻辑 temporal logic](#时间逻辑-temporal-logic)
    - [ch 6 Time in a Distributed System](#ch-6-time-in-a-distributed-system)
        - [逻辑时钟](#逻辑时钟)
            - [弱时钟条件 偏序关系 $\prec$](#弱时钟条件-偏序关系-prec)
            - [弱时钟实现的逻辑时钟 logic clock](#弱时钟实现的逻辑时钟-logic-clock)
            - [强时钟关系 全序关系 $\ll$](#强时钟关系-全序关系-ll)
        - [向量时钟 vector clock](#向量时钟-vector-clock)
        - [物理时钟同步](#物理时钟同步)
            - [Berkeley 算法 内部同步](#berkeley-算法-内部同步)
            - [Lamport and Melliar–Smith 算法 内部同步](#lamport-and-melliarsmith-算法-内部同步)
            - [Cristian 算法 外部同步](#cristian-算法-外部同步)
            - [NTP network time protocol 实现](#ntp-network-time-protocol-实现)
    - [ch 7 Mutual Exclusion](#ch-7-mutual-exclusion)
        - [消息传递模型的方案](#消息传递模型的方案)
            - [Lamport 方案](#lamport-方案)
            - [Ricart–Agrawala 方案](#ricartagrawala-方案)
            - [Maekawa 方案](#maekawa-方案)
        - [token-passing 的方案](#token-passing-的方案)
            - [Suzuki–Kasami 方案](#suzukikasami-方案)
            - [Raymond 方案](#raymond-方案)
        - [共享存储的方案](#共享存储的方案)
            - [Peterson 方案 不使用原子指令](#peterson-方案-不使用原子指令)
            - [test-and-set 原子指令](#test-and-set-原子指令)
            - [load-linked 和 store-conditional 原子指令](#load-linked-和-store-conditional-原子指令)
        - [组同步互斥](#组同步互斥)
            - [中心化方案](#中心化方案)
            - [去中心化方案](#去中心化方案)
    - [ch 8 Distributed Snapshot](#ch-8-distributed-snapshot)
        - [Chandy-Lamport 算法](#chandy-lamport-算法)
        - [Lai-Yang 算法](#lai-yang-算法)
        - [分布式 debug](#分布式-debug)
    - [ch 9 Global State Collection](#ch-9-global-state-collection)
        - [全局广播](#全局广播)
        - [程序终止检测](#程序终止检测)
            - [Dijstra-Scholten 算法](#dijstra-scholten-算法)
            - [单向环的 token passing](#单向环的-token-passing)
            - [信用点分配算法 credit-recovery algorithm](#信用点分配算法-credit-recovery-algorithm)
        - [浪潮 wave 算法](#浪潮-wave-算法)
    - [ch 11 Coordination Algorithms](#ch-11-coordination-algorithms)
    - [ch 12 Fault-Tolerant Systems](#ch-12-fault-tolerant-systems)
    - [ch 13 Distributed Consensus](#ch-13-distributed-consensus)

# Ghosh, Sukumar (2014) - Distributed Systems An Algorithmic Approach

一本高屋建瓴讨论分布式里面重要问题的书。这篇笔记主要就是结合我的理解复述一下书里的我感觉比较重要内容的流水账。

## Ch 1 introduction

特征

1. 多进程
2. 进程间通讯
3. 分离地址空间
4. 单任务

使用分布式的原因

- 地理分离
- 加速
- 远端资源共享
- 容错

一般的问题

1. leader 选举
2. 互斥
3. 时钟同步
4. 全局状态
5. 组播
6. 副本管理

## Ch 2 interprocess communication

### naming

位置无关，方便定位实体。一般是文本形式，树状结构

### rpc remote procedure call

- 机器差异（大小端，指针和地址）
- 阻塞/非阻塞
- 丢包：at-least-once/at-most-once,exactly once

| Client Stub                                                    | Server Stub                                                                       |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| Pack parameters into a message，Send message to remote machine | Do no message → skip od ，Unpack the call parameters                              |
| Do no result → skip od\*，Receive result and unpack it         | Call the server procedure Receive result and unpack it Pack result into a message |
| Return to the client program                                   | Send it to the client                                                             |

### cloud computing

- software-as-a-service, SaaS
  - Google doc，云盘等
- platform-as-a-service, PaaS
  - 小程序 API（？
- infrastructure-as-a-service, IaaS
  - elastic computing cloud， EC2 云服务器

#### MapReduce

Map: 〈_key, value_〉→ list of 〈_key, value_〉

{This is the intermediate 〈key, value〉 pair}

Reduce: 〈_key, list of values_〉 → list of 〈_key, value_〉

## ch 3 Models for Communication

> 本书的可靠信道公理 channel
>
> 1. 所有发出消息都被接收方收到，所有收到消息都有发送者
>    - 对数据链路/传输层不适用，需要恢复
> 2. 每条信息都有一条任意，有限长，非零延迟
>    - 为了放宽对不同延迟的支持
> 3. 信道呈现 FIFO 特性
>    - 电报 datagram 网络乱序

### shared variable 共享变量

> DSM
>
> distribted shared memory，方便使用共享变量有关的编程工具

1. state reading/locally shared variable 读取自身和邻居状态，只能修改自己状态
2. link register 链路自带单读者单写者寄存器

#### linda

共享的 tuple space

| 原语    | 含义                                         |
| ------- | -------------------------------------------- |
| IN，INP | 取出 tuple，分阻塞、非阻塞版本               |
| OUT     | 放入 tuple                                   |
| RD，RDP | 读 tuple，类比 in 不删除。分阻塞、非阻塞版本 |
| EVAL    | 创建新进程                                   |

```c
/** main program**/
real_main(argc,argv)
int argc;
char *argv[];
{
    int nslave, j, hello();
    nslave = atoi (argv[1]);
    for (j = 0; j < nslave; j++)
    	EVAL ("slave", hello(j));
    for(j = 0; j < nslave; j++)
    	IN("done");
    printf("Task completed.\n");
}
/** subroutine hello **/
    int hello (i)
    int i;
{
    printf("Task from number %d.\n",i);
    OUT("done");
    return(0);
}
```

#### mobile agent

移动执行的代码

(I, P, B)

- I 标识符
- P 对应代码
- B 代码的变量

### 模型强弱

强模型限制多，操作多；强模型模拟弱模型容易，反之可能困难

#### resequencing protocol, 非 FIFO 模拟 FIFO

缓存再发送。可以优化为环形缓存编号

```plain
{Sender process P}
var i : integer {initially 0}
repeat
    send m[i],i to Q;
    i := i+1;
forever

{Receiver process Q}
var k : integer {initially 0}
buffer : buffer [0..∞] of message
{initially for all k:buffer[k] = null}
repeat{store}
	receive m[i],i from P;
    store m[i] into buffer[i];
    {deliver}
    while buffer[k] ≠ null do
    begin
        deliver the content of buffer [k];
        buffer [k] := null; k := k+1;
    end
forever
```

#### 共享变量模拟信息传递

```plain
shared var p, q: integer {initially p = q}
buffer: array [0..max-1] of message
{Sender process P}
var s: array of messages sent by P, i : integer {initially 0}
repeat
	if p ≠ q − 1 mod max then
    begin
    	buffer[p] := s[i]; i := i + 1; p := p + 1 mod max
    end
forever

{Receiver process Q}
var r: array of messages received by Q, j : integer {initially 0}
repeat
    if q ≠ p mod max then
    begin
    	r[j] := buffer[q]; j := j + 1; q := q + 1 mod max
    end
forever
```

#### 信息传递模拟共享变量

下列实现错误：

1. 组播不 atmoic，atmoic 组播有代价
2. 组播不一定遵循更新变量顺序

```plain
{Implementing shared memory by message passing: first attempt}
{read X by process i}
read x[i] x[i] := v


{write X := v by process i}
x[i] := v
Multicast v to every other process j (j ≠ i) in the system;
Process j (j ≠ i), after receiving the multicast, sets x[j] to v.
```

#### 信道判空的不可能性

如果进程`i,j`之间信道需要判空

- 已知`i,j`之间最大时延 T，等待 T
- 未知最大时延，初始信道可以包含任意信息，无法判空（即使使用一个清零帧）

### 系统分类

- reactive：反应性，对请求响应
- transformational：转移性，存在初态和末态
- named：和处理器标识有关
- anonymous：和处理器标识无关，即使标识改变可以继续运行，需要如随机数等机制打破对称性

### 复杂度计数

- 空间复杂度：按 node 数目的增长要求的空间
- 时间复杂度
  - 总步数
  - 总轮次 round：最远端的执行一次是一轮

## ch 4 Representing Distributed Algorithms

> guard
>
> 断言，条件执行，如果多个断言判正确，由 scheduler 决定执行任意一个代码段，下面假定每个代码段原子执行

程序终止条件

1. 每个进程终止，guard 全判`false`
2. 无消息还在传递

类似如下的伪代码和对应的 Rust 实现

```plain
program uncertain;
define x : integer;
initially x = 0
do
  x < 4 → x := x + 1
  [] x = 3 → x := 0
od
```

```rust
use rand::Rng;

use std::sync::atomic::{AtomicUsize, Ordering};
static CNT: AtomicUsize = AtomicUsize::new(0);
fn main() {
    for _ in 0..10 {
        CNT.store(0, Ordering::Relaxed);
        the(0);
    }
}

fn the(i: i32) -> () {
    CNT.fetch_add(1, Ordering::Relaxed);

    match (i == 3, i < 4) {
        (true, _) => {
            let guess = rand::thread_rng().gen_range(0, 100);
            match guess {
                0 => the(i + 1),
                _ => the(0),
            };
        }
        (false, true) => the(i + 1),
        (false, false) => println!("terminate in {:?}", CNT),
    };
}
```

### 公正性

由于 guarded action 的选择依赖 scheduler （调度器） 决定，scheduler 需要考虑公平性

- 无条件公平：每个代码段最终会进入调度队列，无论 guard 值
- 弱公平：只要代码段的 guard 变为并保持 `true`就最终会进入调度队列
- 强公平：只要代码段的 guard 能无限次经常变为`true`就最终会进入调度队列，弱公平能跑出的顺序，强公平一定能跑出，反之不一定

```plain
program test
define
x: integer {initial value undefined}
do
  true → x := 0
  [] x = 0 → x := 1
  [] x = 1 → x := 2
od
```

- 不公平时可能只执行第 1 条
- 弱公平时只保证执行第 1、2 条（一直执行第 1 条会保证 x=0 从而执行 2，但 x=1 时可以只执行第 1 条而使得第 3 条永不满足）

### scheduler

> central scheduler
>
> 又名 serial scheduler：原子的调度执行 guard 对应的代码段，简单，可以用 token 传递实现，拓展性差，并行度差，是分布式 scheduler 的退化情形
>
> 有关的 Theorem：
>
> 1. 算法在 central scheduler 下正确
> 2. 已为`true` 的 guard 不会被其他代码段修改为`false`
>
> =>在 distributed scheduler 下正确

## ch 5 Program Correctness

> 安全 safety 条件
>
> "bad things never happen."

如：

- 同步互斥：至多一个进程在临界区
- 有限容量信道：$nC\le nP\le nC+B$
- 读写锁：$(nW\le 1 \wedge nR=0) \vee (nW=0\wedge nR\ge 0)$
- 无死锁：$Q\wedge GG$
- 部分正确性：如果不死锁，必得正确结果 $\neg GG\implies Q$

> 活跃 liveness 条件
>
> "good things eventually happen."

如：

- 进行：对于同步互斥问题，不死锁，不活锁（声明了临界区，最终总能获得机会）
- 公平性：有限时间后 schduler 总会调度
- 可达性：是否可以从初始状态 $S_0$ 到达状态 $S_k$
- 终止：部分正确+终止状态可达

### 断言推理 assertional reasoning

用于证明安全性。构造不变式 $I$ 代表安全性条件（类似数学归纳法）

1. 初态不变式 $I$满足
2. 假定前状态不变式 $I$满足，任何执行后不变式 $I$ 仍然满足

### 良基集 well-founeded set

用于证明活跃性。一个映射（测量函数/变化函数 measured/variant function）从系统全局状态到一个 well-founed set $f:S\to WF$

> well-founded set
>
> 对于 $WF=\\{w_1,w_2,...\\}$ ，上面有个全序关系$\gg$
>
> 1. 不存在无穷比较链 $w_1\gg w_2 \gg ...$
> 2. 系统状态从 $s_1$ 到 $s_2$ ，有 $w_1=f(s_1),w_2=f(s_2) \implies w_1 \gg w_2$

一般 well-founed set 会选择非负整数集，0 就是终止状态，全序关系就是大于；也有选择 set 是一组元素，全序关系是字典顺序。

### 时间逻辑 temporal logic

- $\Box P$ 意思是 $P$ 永真
- $\lozenge P$ 意思是 $P$ 最终会变为真

有

1. $\lozenge P=\neg \Box (\neg P)$：P 假不是一直不真的=P 假最终会不真
2. $\Box P\wedge \Box Q=\Box (P\wedge Q)$
3. $\Box P\vee \Box Q=\Box (P\vee Q)$
4. $\lozenge\Box P\implies \Box \lozenge P$：P 最终会一直真推出 P 会一直经常真（强公平蕴含弱公平）
5. $\lozenge P\wedge \lozenge Q=\lozenge (P\wedge Q)$
6. $\lozenge P\vee \lozenge Q=\lozenge (P\vee Q)$
7. $\lozenge P\vee Q\neq \lozenge (P\vee Q)$: **注意：** 如果 $Q=\neg P$，右边永真，左边可以为假

## ch 6 Time in a Distributed System

### 逻辑时钟

#### 弱时钟条件 偏序关系 $\prec$

1. 同一进程里，a 在 b 后发生，$a\prec b$
2. 两个进程通讯，i 进程的 a 发送，j 进程 b 接受，$a\prec b$
3. $(a\prec b)\wedge(b\prec c)\implies (a\prec c)$

#### 弱时钟实现的逻辑时钟 logic clock

1. 每个本地事件发生，$LC$ 递增 1
2. 发送消息时附上当前 $LC$ 作为时间戳
3. 接受到消息时 $LC=1+\max(local\; LC, message\; LC)$

此时可以保证，但是反之不成立。逻辑时钟不能完全和因果关系互推，需要[向量时钟](#向量时钟-vector-clock)。

$a\prec b\implies LC(a)<LC(b)$

#### 强时钟关系 全序关系 $\ll$

$a\ll b$ 成立条件

- 要么$LC(a)<LC(b)$
- 要么不同进程，事件 a、b 对应进程 i、j，有 $LC(a)=LC(b),i<j$

### 向量时钟 vector clock

从事件集到进程数大小(假定为 n)的非负整数向量的函数 $VC:V\to A$。事件 a、b 的向量时钟之间大小关系$VC(a)<VC(b)$的充要条件为

1. $\forall i:i\in [0,n-1]:VC_i(a)\le VC_i(b)$
2. $\exists j\in[0,n-1]:VC_j(a)<VC_j(b)$

如果两个事件之间向量时钟既不大于也不小于，认为事件同时，记作$a\parallel b$。向量时钟保证了因果检测，即 $a\prec b \iff VC(a)<VC(b)$ 。实现要求：

1. 进程 i 本地事件发生时候 $VC_i[i]+=1$
2. 发送信息时附带整个向量时钟
3. 进程 j 收到消息附带时间戳 $T$ 时，先更新本地 $VC_j[j]+=1$ ，再更新全局 $\forall k \in [0,n-1]:VC_k[j]=\max(T_k,VC_k[j])$

### 物理时钟同步

1. 外部时钟同步：和 UTC/原子钟/GPS 同步时间。可以借助 NTP（network time protocol）协议
2. 内部时钟同步：在即使内部少数错误时钟下仍然同步时间，只需要内部相互同步即可，注意一般通过调快/调慢实现以防止突变
3. 相同步
4. 有界时钟：只能通过加位数保证可预见未来不溢出

- 偏移率 drift rate：内部时钟和理想时钟走时速度不完全相等：$(1-\rho)\le \frac{\rm{d}C}{\rm{d}t}\le(1+\rho)$
- 时钟偏差 clock skew：时钟之间最大允许读数偏差 $\delta$
- 重同步间隔 resynchronization interval ：$R$

#### Berkeley 算法 内部同步

选择一个 leader 收集计算各个内部时钟的平均值，并按此调整

#### Lamport and Melliar–Smith 算法 内部同步

去中心化的算法。可以应对 two-faced clock 问题。$c_k[i]$ 表示时钟 i 读取时钟 k 得到的值。

> two-faced clock
>
> 2 个非错时钟向 two-faced 时钟读数结果不一致

1. 读系统中每个时钟的值
2. 将离群值丢弃,用本地值代替： $|c_i[i]-c_j[i]|>\delta \implies c_j[i]\gets c_i[i]$
3. 使用处理后的平均值覆盖本地时钟值

对于 n 个时钟，只要 two-faced clock 数目 t 有 $n>3t$ 就能保证同步。

考虑 3 个时钟，i、j 好，k 为 two-faced clock。有如下配置

- $c_i[i]=c$
- $c_j[j]=c-\delta$
- $c_k[i]=c+\delta$
- $c_k[j]=c-2\delta$

这时候 i、j 平均值差为 $\frac{3t\delta}{n}$。如果 $n>3t$ 就有$\frac{3t\delta}{n}<\delta$。同步间隔 $R\le(\delta -\frac{3t\delta}{n})/\rho$

#### Cristian 算法 外部同步

每个 client 向有精确时钟的 time server 定时通信来同步时钟： $R<\frac{\delta}{2\rho}$。

> round-trip time
>
> 每次同步通过发起 RPC，用来度量信号延迟：$RTT=T_2-T_1$

每次时钟修正为 $T_s+\frac{RTT}{2}=T_s+\frac{T_2-T_1}{2}$。但实际网络波动，返回时间是一个区间，记最短用时 $min$，对应同步精度 $\pm(\frac{T_2-T_1}{2}-min)$

#### NTP network time protocol 实现

按层级 stratum 组织各计算机，stratum 0 对应直连高精度时钟，stratum $i$ 作为 $i+1$ 的 time server，精度随着层级增加递减。

- 组播：使用 UDP 协议定期发送
- RPC：使用 [Cristian 算法](#cristian-算法-外部同步)
- P2P 通信：同层级的 time server 互相同步保持精度。设 Q 在 P 之前$\delta$，互相发报：$T_2=T_1+T_{PQ}+\delta,T_4=t_3+T_{QP}-\delta$，则有 $\delta=\frac{T_2-T_4-T_1+T_3}{2}-\frac{T_{PQ}-T_{QP}}{2},RTT=T_{PQ}+T_{QP}=T_2+T_4-T_1-T_3$， 这时候两个时钟误差$\delta$就可以控制在 $\frac{T_2-T_4-T_1+T_3}{2}\pm\frac{RTT}{2}$ 里面（注意到 $T_{PQ},T_{QP}>0$，相减的值可以由 RTT 控制）

## ch 7 Mutual Exclusion

3 个基本要求

1. 同步互斥 Mutual exclusion：至多一个进程在临界区，这是安全性性质
2. 不死锁 Freedom from deadlock：至少一个进程可以运行和进入临界区，也是安全性性质
3. 进展 Progress：每个尝试进入临界区的进程最终总能进入，这是活跃性性质

> livelock/starvation
>
> 违反性质 3。进程一直被阻止进入临界区

> FIFO fairness，FIFO 公平
>
> 进入临界区的顺序按照申请的顺序，类似 FIFO 队列。注意是申请的时间而不是申请到达 central coordinator（如果采用中心化的算法）的时间，所以一般世俗机构的办事先到先得不是 FIFO fairness

### 消息传递模型的方案

下面算法一般要求发送消息时间戳。注意到如果只考虑同步互斥问题，时间戳最大差 $(n-1)$，因此可以选择 $\mathrm{mod}(2n-1)$ 的时间戳，有效规避无界时钟问题（详见 [物理时钟章节](#物理时钟同步) ）。

#### Lamport 方案

- 全连接网络
- 信道 FIFO，不丢信息
- 每个进程维护一个队列 Q
- 3 特性+FIFO 公平，一轮需要 $3(n-1)$ 次消息传递

1. 期望进入临界区的进程广播带时间戳`request`
2. 接受到`request`的进程
   1. 不在临界区：回复 `ack`
   2. 在临界区：直到退出临界区再回复`ack`
3. 进入临界区条件：
   1. 检查本地队列 Q 自己的请求最早
   2. 其他进程都回复了`ack`
4. 退出临界区时：
   1. 删除本地队列 Q 中自己的请求
   2. 广播带时间戳`release`
5. 收到`release`后进程删除对应的请求

#### Ricart–Agrawala 方案

- 不需要维护本地队列
- 只是更多的缓存请求.对时间戳晚于自己的请求，当时不在临界区时：
  - Lamport 方案回复
  - Ricart–Agrawala 方案缓存
- 3 特性+FIFO 公平，一轮需要 $2(n-1)$ 次消息传递

1. 期望进入临界区的进程广播带时间戳`request`
2. 接受到`request`的进程回复 `ack` 条件，反之缓存请求
   - 该进程不准备进入临界区
   - 该进程期望进入的时间戳晚于对应的`request`
3. 进入临界区条件其他进程都回复了`ack`
4. 退出临界区执行其他操作前，对等待的请求回复`ack`

#### Maekawa 方案

每个进程 i 属于单独的通信组 $S_i$。组内互相监督满足临界区，只要组的覆盖足够好，就可以减少通信支出。大概为 $3\sqrt{n}=O(\sqrt{n})$

1. $\forall i,j\in [0,n-1],S_i\cap S_j\neq \varnothing$：保证全局覆盖
2. $i\in S_i$：自身也被监督
3. 最好的，每个进程属于通信组的次数相同（有对称性）

> global FIFO
>
> 每个进程严格按照发送时间戳接受消息：极难实现

- global FIFO 成立时
  1.  期望进入临界区的进程对 $S_i$ 广播带时间戳`request`
  2.  对时间戳最早的请求回复`ack`，锁住，其他请求排在队列中；如果进程在临界区里，退出时再进行
  3.  进入临界区条件：收到 $S_i$ 中每个进程的 `ack`
  4.  退出临界区时对 $S_i$ 广播`release`
  5.  接收到`release`后从队列剔除对应请求，解锁，重复`步骤2`
- 没有 global FIFO，可能会因为循环等 `ack` 导致死锁，需要添加放弃机制
  1. 期望进入临界区的进程对 $S_i$ 广播带时间戳`request`
  2. 不在临界区时接受到请求：
  3. 未锁：对时间戳最早的请求回复`ack`，锁定
  4. 已锁，新请求的时间戳更晚：回复`failed`
  5. 已锁，新请求的时间戳更早：排队请求，对之前锁定请求的发送方发`inquire`，可能会重排顺序
  6. 进入临界区条件：收到 $S_i$ 中每个进程的 `ack`
     - 如果收到 `inquire` 还接受到了`failed`，对 $S_i$ 广播 `relinquish` 放弃排期自己的请求
     - 如果只收到 `inquire` 可以忽略
  7. 退出临界区时对 $S_i$ 广播`release`
  8. 接收到`release`后从队列剔除对应请求，解锁，重复`步骤2`
  9. 已锁，接受到`relinquish`，重排队列，对时间戳最早的发`ack`

### token-passing 的方案

#### Suzuki–Kasami 方案

全连接网络。初始有个进程拥有 token。期望进入临界区的进程 $i$ 广播带序列号的消息 $(i,num)$。拿到 token 即允许进入临界区。每个进程有本地队列 Q 和本地向量

- $req[0,...,n-1]$ 记录对应进程最近请求序列号
- $last[0,...,n-1]$ 记录对应进程进入临界区次数

进程 $i$ 拿到 token 后

1. $last[i]\gets num$
2. 将满足 $1+last[k]=req[k]$ 的每个进程 k 加入本地队列 Q
3. 执行临界区
4. 取出 Q 第一项传递 token

对应消息总数 $(n-1)+1$（发出 $n-1$，接收 1 条 token）

#### Raymond 方案

关系组织成树。每个进程有一个本地队列。一般的，树之间节点距离就是通信开销 $O(\log{n})$

1. 节点拥有 token 时候为树的根，并可以进去临界区，反之将自己的请求加入自己本地队列
2. 节点没有 token，本地队列非空时给父节点发送请求，除非已经发送并在等待
3. 根节点结束临界区，收到请求时，给本地队列第一项的邻居传递 token，并改为指向该邻居，该邻居变成根节点
4. 接受到 token 时候，向本地队列第一项的邻居继续传递，并删除对应的请求，改为指向该邻居，如果队列中还有请求，向新的父节点发送请求

### 共享存储的方案

一般依靠原子指令：

- compare-and-swap (CAS)：比较预期值和内存变量，相等时候改为新传入的值，反之不修改，返回执行之后的内存变量值
- fetch-and-add(FA)：原子加
- semaphore 信号量：非负整数支持原子操作，可以对应资源个数
  - $P(s)\triangleq\\{waituntil\; s>0\implies s-=1\\}$：申请资源，取得后可用资源-1
  - $V(s)\triangleq\\{s+=1\\}$：释放资源，可用资源+1

#### Peterson 方案 不使用原子指令

2 个进程版本

```
program peterson;
define flag[0], flag[1] shared Boolean;
turn: shared integer
initially flag[0] = false, flag[1] = false, turn = 0 or 1
{program for process 0}
do true→
    flag[0] := true;
    turn := 0;

    do (flag[1] ∧ turn = 0) → skip od//不需要原子语句，turn要么0要么1，不会死锁；如果是flag引起进入临界区，process 1已经执行完临界区了；如果是turn引起，process 1 会等待：保证互斥

    critical section;
    flag[0] := false;
    non-critical section codes
od
{program for process 1}
do  true →
    flag[1] := true;
    turn := 1;

    do (flag[0] ∧ turn = 1) → skip od;//不需要原子语句

    critical section;
    flag[1] := false;
    non-critical section codes
od
```

多进程拓展版本。跑 $n-1$ 轮，每轮留下一个（最后一个修改 $turn[j]$ 的），最后选出 1 个执行临界区。最高位执行完后，$flag$ 会置 0，剩下 flag 最高的会结束等待，然后按照 轮数递减执行临界区。

```
program Peterson n-process;
define flag, turn: array [0.. n − 1] of shared integer;
initially ∀k:flag[k] = 0, and turn = 0
{program for process i}
do true →
    j:=1;
    do j ≠ n − 1
        flag[i] := j;
        turn[j] := i;

        do ((∃k ≠ i: flag[k] ≥ j) ∧ turn[j] = i) → skip od;// （1：选出的执行完后递减执行）∧（每轮修改turn的留下，flag不动）

        j := j + 1;
    od;

    critical section;

    flag[i] := 0;

    non-critical section codes
od
```

#### test-and-set 原子指令

特殊的原子指令，取得某`bool`变量值，然后将其置 `True/1`

```
program Test-and-set (for any process);
define
    x: shared integer;
    r: integer (private);
initially
    x = 0, r = 1;
do true →

    do r ≠ 0 → TS(r, x) od;

    critical section;
    x := 0
od
```

#### load-linked 和 store-conditional 原子指令

- load-linked $LL(r,x)$：类似普通 load 功能 $r\gets x$，还会对 x 插装
- store-conditional $SC(r,x)$：类似 store $x\gets r$，如果 SC 是在其他进程的 LL 后执行后没修改，r 返回成功，反之 x 的值不改变，r 返回失败。LL 和 SC 配合类似 test-and-set

```
program mutex (for process i);
define x: shared integer; r: integer (private);
initially x = 0;
do true →
try:
    do r ≠ 0 → LL(r, x) od; //critical section is busy
    r = 1; SC(r, x);

    if r = 0 → goto try fi;// SC did not succeed

    critical section;
    x := 0;
    non-critical section codes;
od
```

### 组同步互斥

进程可以属于不同的独立的 forum，按 forum 为单位占有资源 in session。这是单独同步互斥、读写锁等经典问题的推广化。

1. 同步互斥：同一时间最多 1 个 forum 在 in session
2. 无死锁：任何时间最少一个进程可以有效行动
3. 有界等待：有成员的 forum 在有界时间内能 in session
4. 同步进入：只要 forum 在 in session，其他有意愿的进程都能加入

#### 中心化方案

- 每个进程拥有一个 $flag\in\\{F_i,\perp\\}$，中心协调器按顺序读取 flag 信息，安排进入 forum 和 in session
- 为了防止一个 forum 一直有进入，指定一个 leader（一般是第一个进入的进程），当 leader 退出时 forum 结束 in session

#### 去中心化方案

每个进程拥有一个 $flag=(state,op),state\in \\{request, in\\_cs, in\\_forum, passive\\},op\in\\{F,F',\perp\\}$。类似于 [peterson 的 2 进程方案](#peterson-方案)。为了保证想要进入 forum 的都可以，而不是偶尔检查条件被 skip，可以选择第一个进入的为 leader，leader 保证申请的随后进入 forum

```
First attempt with two forums F and F′
define  flag: array[1..n − 1] of (state, op), turn ∈ {F, F′}
        state ∈ {request, in_cs, in_forum, passive}
        op ∈ {F, F′, ⊥}
{Program for process i trying to attend forum F}
do ∃j ≠ i: flag[j] = (in_cs, F′) →

    flag[i] := (request, F); //发送请求

    do turn ≠ F ∧ ¬(∀j ≠ i: flag[j].op ≠ F′) → skip od; // (1 F'之前执行完)∧(2 没有要求进入F'的)

    flag[i] := (in_cs, F);//准备进入 forum 的临界区
od;

attend forum F;

turn := F′;
flag[i] := (passive, ⊥)
```

## ch 8 Distributed Snapshot

记录分布式系统的单个分布组分的状态信息，收集分散的状态信息在下一章 [global state collection](#ch-9-global-state-collection) 介绍。非常有用：死锁检测、程序终止检测、系统回滚等。

> cut 切分
>
> 一组事件，而且每个进程至少有一个事件

> consistent cut
>
> cut，而且对于里面的事件，其因的事件也在 cut 当中： $(a\in C)\wedge(b\prec a)\implies b\in C$

> consistent
>
> 对于一次运行（computation，run，behavior），$\forall a,b, a\prec b$，a 发生在 b 前，就称为 consistent 的，保证 consistent 下可以有多种可能的实际事件顺序，如，交换并行的两个事件执行顺序不会影响运行的 consistent 特性

### Chandy-Lamport 算法

强连通图，信道 FIFO，有一个启动进程 initiator，发生一个 \* 标志消息启动记录，每个进程有两种状态，`white`和`red`，初始为`white`

1. 启动进程原子执行
   1. 变`red`
   2. 记录本地状态
   3. 向所有对外信道广播\*标志
2. 所有进程在**第一次**接受到\*标志后，先做以下原子操作再执行其他任务
   1. 变`red`
   2. 记录本地状态：发送事件和接收事件分别由发送进程和接收进程记录
   3. 向所有对外信道广播\*标志

算法记录最后一次白色+第一次红色事件。由于**白色事件不可能引发红色消息**，实际记录下来的事件顺序必然保持因果关系。

- 算法记录下来的 snapshot state 都是由初始状态可达的，但不保证每次运行都能跑出这个状态
- 每个对于初始状态可达的最终状态，对算法记录下的 snapshot state 也是可达的：这保证了回滚的正确性

### Lai-Yang 算法

对 Chandy-Lamport 的改进，信道不需要 FIFO，消息也附加两种状态，`white`和`red`。是一种懒记录方法，主要期待借用已有的各种消息传递。对于程序终止检测（终止后不会再收发任务消息）等需要额外发控制消息。

1. 启动进程记录本地状态，任何外发消息为$(msg,red)$
2. 任何进程第一次接受到$(msg,red)$时，先记录本地状态，再处理接收信息

### 分布式 debug

> 本地状态 $s(i),s(j)$ 对应 consistent 的全局状态
>
> 如果本地状态 $s(i),s(j)$ 是由事件 $e_i,e_j$ 引发，那么逻辑时钟关系 $\forall k,VC_k(e_i)\sim VC_k(e_j)$

对由初始状态可达的 consistent 的全局状态应用判断 $\phi$。这样的判断时间复杂度巨大，需要注意可拓展性：n 个进程每个 m 个可能行动 $O(m^n)$

- Possibly $\phi$：至少一个为真
- Definetly $\phi$：永真 $definetly\;\phi \implies possibly\; \phi$
- Never $\phi$：永假

## ch 9 Global State Collection

本章假定底层任务能表现出预期的性质（如检测终止算法中，底层任务确实能终止）

### 全局广播

假设本地需要被收集的为 $s(i)$,最后每个进程都能收集到 $\forall i:V(i)=\{s(k):0\le k\le n-1\}$。那 naive 的方法就是每次向邻居通知自己新知道的其他进程的信息，直到大家知道全了。消息复杂度：向每个邻居发，每次多一个：$O(n^2)$，全局 $O(n^3)$

```
program broadcast (for process i}
define Vi, Wi: set of values;
initially Vi = {s(i)}, Wi = Ø {and every channel is empty}
do Vi ≠ Wi      →   send Vi\Wi to every outgoing channel;
                    Wi:= Vi
[] ¬empty (k,i) →   receive X from channel (k,i);
                    Vi:= Vi ∪ X
od
```

- $empty(i,j)\implies W_i\subseteq V_j$：归纳法易证，注意到 $W_i^{r+1}=V_i^{r+1},V^{r+1}/W^r_i\subseteq V_j$
- 停止时候能保证 $\forall i:V(i)=\{s(k):0\le k\le n-1\}$: 由上一条+停止条件 有 $\forall i,j:V_i\subseteq V_j$，显然
- 有界步终止：必然递增

### 程序终止检测

（不一定需要是全局终止，相同步时候也要检测某相结束以开启下一相）

#### Dijstra-Scholten 算法

> diffusing computation
>
> 由一个 initiator 开启，通知邻居逐步开始的计算

- 沿方向的消息为 `signal` ，反向消息为`ack`
- 环境 environment 节点：只有向外边
- 内部 internal 节点：从环境节点可达
- 环境节点起始发`signal` 开始算法
- 任何节点第一次收到`signal` 的发送方为父节点，然后自己开始向邻居广播`signal`
- 之后收到`signal` 立刻回复`ack`，自己邻居都回复了`ack`后向父节点回复`ack`，起始节点收完`ack`即算法结束
- 对于某条有向边，沿向`signal`和反向`ack`数值差为 deficit
- 对于某个节点：
  - $C$：入边的 deficit 和
  - $D$：出边的 deficit 和

在这样设定下，有如下 2 不变式：

1. $(C\ge 0)\wedge(D\ge 0)$:deficit 定义可知
2. $(C>0)\vee(D=0)$:（1 等待邻居子图完成）或者是（2 邻居都完成了，可以回复父节点了）

注意上述不变式有 $(C>1)\vee(C=1\wedge D=0)$，整个进程之间关系是一棵树。要求信道 FIFO（保证工作信息和检测信息之间正确顺序，防止虚假终止）。在大家确实停止后，消息复杂度 $O(|E|)$（每个信道来去各一次）

```
program detect {for an internal node i}
define  C, D : integer
        m: (signal, ack) {represents the type of message received}
        state: (active, passive)
initially C = 0, D = 0, parent(i) = i
do (m = signal) ∧ (C = 0)   → C := 1; state := active;
                                parent := sender
                                //开始准备向邻居广播
    [] m = ack                  → D := D − 1
    [] (C = 1 ∧ D = 0) ∧ (state = passive) → send ack to parent;
                                            C:= 0; parent(i) = i
                                 //节点可以返回初始状态了
    [](m = signal) ∧ (C = 1)    → send ack to the sender
                                // 对多余的signal直接回复ack
od
```

#### 单向环的 token passing

1. 每个节点 `black,white`两个状态，初始`white`
2. 向 token 传递反向高的节点发消息后，变`black`
3. `white`节点传递 token 时颜色不变，`black`节点传递时染黑 token
4. 节点传递完 token 后变回`white`
5. initiator 能收发白色 token 即终止

算法要求消息通信瞬时（新的消息要追上 token 速度）

```
program term {for process i > 0，假定进程0为启动进程}
define  color, token: (black, white) {colors of process and token}
        state : (active, passive)
do  (token = white) ∧ (state ≠ passive)   → skip
    [](token = white) ∧ (state = passive) →
            if color(i) = black → color(i) := white; send a black token
            [] color(i) = white → send a white token
            fi
    [](token = black)   → send a black token
    []i sends a message to a higher numbered process → color(i) :=black
od
{for process 0}
send a white token;
do
(token ≠ white) → send a white token
od
//收回白token 结束
```

#### 信用点分配算法 credit-recovery algorithm

- $\sum credit(i)=1$
- 对于活跃进程：$credit(i)>0$
- 对于休眠进程：$credit(i)=0$

1. 活跃进程发消息时，将自身$credit/2$ 随消息发出
2. 休眠进程接到消息，转活跃，并对于自身$+=msg_{credit}$
3. 活跃进程收到消息，可以：发回给启动进程，或者为了减少消息数目 保留加到自己的 $credit$

### 浪潮 wave 算法

> wave
>
> 一个启动进程某活动，引发邻居活动，进而邻居的邻居活动，此谓浪潮
>
> 1. 每个计算有界
> 2. 每个计算至少包括一个确定性事件 decision event
> 3. 一个确定性事件 decision event 在每个进程中一些事件的因果前

> PIF
>
> Propagation of Information with Feedback，类似于 [Dijstra Scholten 算法](#dijstra-scholten-算法)里的广播，但是此时返回是副本不是`ack`

```
program PIF {for the initiator node i}
define  count : integer
        N(i): set of neighbors of process i

send M to each neighbor; count := |N(i)|
do
    count ≠ 0 ∧ M received → count: = count − 1
od
{program for a non-initiator node j≠i}
if
    message M received → parent := sender
                        send M to each neighbor except parent;
                        count := |N(j)|;
    []count > 0 ∧ M received → count: = count − 1
    []count = 0              → send M to parent
fi
```

## ch 11 Coordination Algorithms

## ch 12 Fault-Tolerant Systems

## ch 13 Distributed Consensus
