---
title: "笔记：Distributed Systems An Algorithmic Approach 2nd"
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

# Ghosh, Sukumar (2014) - Distributed Systems An Algorithmic Approach

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
> 对于 $WF=\{w_1,w_2,...\}$ ，上面有个全序关系$\gg$
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

可以应对 two-faced clock（2 个非错时钟向 two-faced 时钟读数结果不一致）问题。$c_k[i]$ 表示时钟 i 读取时钟 k 得到的值。

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
