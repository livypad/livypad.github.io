- [tex](#tex)
	- [模板](#模板)
	- [符号](#符号)
- [数据结构](#数据结构)
	- [hash](#hash)
	- [c 系列 IO](#c-系列-io)
		- [cin/cout](#cincout)
		- [fread](#fread)
		- [mmap](#mmap)
	- [POSIX信号](#posix信号)
		- [6 SIGABRT](#6-sigabrt)
		- [8 SIGFPE](#8-sigfpe)
		- [11 SIGSEGV](#11-sigsegv)

# tex

## 模板

```tex
\documentclass[UTF8]{ctexart}%文档宏观定义
\usepackage{lmodern}%导入宏包，LaTex导入宏包多次不会报错，所以可以随便复制粘贴宏包，即使不需要
\usepackage{amssymb}
\usepackage{amsmath}
\usepackage{graphicx}
\usepackage{float}
\usepackage{adjustbox}
\usepackage{geometry}
\usepackage{fullpage}
\usepackage{longtable}
\usepackage{booktabs}
\usepackage{tikz}
\usepackage{listings}
\usepackage{xcolor}
\usepackage{subfigure}
\lstset{
      %背景框
      framexleftmargin=10mm,
      frame=none,
     %背景色
     %backgroundcolor=\color[rgb]{1,1,0.76},
     backgroundcolor=\color[RGB]{245,245,244},
    %样式
    keywordstyle=\bf\color{blue},
     identifierstyle=\bf,
     numberstyle=\color[RGB]{0,192,192},
     commentstyle=\it\color[RGB]{0,96,96},
     stringstyle=\rmfamily\slshape\color[RGB]{128,0,0},  %显示空格
     showstringspaces=false
 }
\newcommand*{\de}{^\circ\hspace{-0.09em}}
%温度
\newcommand*{\dif}{\mathop{}\!\mathrm{d}}
%导数的d
\newcommand*{\e}[1]{\times 10^{#1}}
%\e{}用来表示科学计数法
\newcommand*{\celsius}{\ensuremath{^\circ\hspace{-0.09em}\mathrm{C}}}
%摄氏度

\ctexset{
section = {
format = \raggedright\large\bfseries,
}
}

\title{}%标题
\author{}%作者名
\date{\today}%自动生成今天日期

\geometry{hcentering}
\textwidth 16cm%文章页面宽度
\linespread{1}
\setCJKmainfont{Microsoft YaHei}
%设置字体
\begin{document}%文章开头
\maketitle

\begin{figure}[H]%插入图片
    \centering%居中
    \includegraphics[height=12cm,width=0.6\linewidth]{5pic1.jpg}

    \caption{}%描述名字
\end{figure}

\begin{figure}[H]%多图片
    \centering
    \subfigure[title]%多图片的图片名
    {
        \begin{minipage}[t]{0.5\linewidth}
            \centering
            \includegraphics[width=\linewidth]{5pic3.jpg}
        \end{minipage}%
    }
    \subfigure[]%多图片的图片名
    {
        \begin{minipage}[t]{0.5\linewidth}
            \centering
           \includegraphics[width=\linewidth]{5pic4.jpg}
        \end{minipage}%
    }
    \centering
\end{figure}

\begin{lstlisting}[language=c]
    xx[j] = xx[j - 1] + d * vv[j - 1];
\end{lstlisting}

\end{document}

```

## 符号

\Alpha \alpha

A \AlphaA α \alphaα

\Beta \beta

B \BetaB β \betaβ

\Gamma \gamma

Γ \GammaΓ γ \gammaγ

\Delta \delta

Δ \DeltaΔ δ \deltaδ

\Epsilon \epsilon \varepsilon

E \EpsilonE ϵ \epsilonϵ ε \varepsilonε

\Zeta \zeta

Z \ZetaZ ζ \zetaζ

\Nu \nu

N \NuN ν \nuν

\Xi \xi

Ξ \XiΞ ξ \xiξ

\Omicron \omicron

O \OmicronO ο \omicronο

\Pi \pi

Π \PiΠ π \piπ

\Rho \rho

R \RhoR ρ \rhoρ

\Sigma \sigma \varsigma

Σ \SigmaΣ σ \sigmaσ ς \varsigmaς

\Eta \eta

H \EtaH η \etaη

\Theta \theta \vartheta

Θ \ThetaΘ θ \thetaθ ϑ \varthetaϑ

\Iota \iota

I \IotaI ι \iotaι

\Kappa \kappa \varkappa

K \KappaK κ \kappaκ ϰ \varkappaϰ

\Lambda \lambda

Λ \LambdaΛ λ \lambdaλ

\Mu \mu

M \MuM μ \muμ

\Tau \tau

T \TauT τ \tauτ

\Upsilon \upsilon

Υ \UpsilonΥ υ \upsilonυ

\Phi \phi \varphi

Φ \PhiΦ ϕ \phiϕ φ \varphiφ

\Chi \chi

X \ChiX χ \chiχ

\Psi \psi

Ψ \PsiΨ ψ \psiψ

\Omega \omega

Ω \OmegaΩ ω \omegaω

# 数据结构

## hash

本文将介绍什么是字符串哈希函数，字符串哈希函数常见用法，以及字符串哈希函数的实现原理和常用算法。

其中数据 1 为 100000 个字母和数字组成的随机串哈希冲突个数。数据 2 为 100000 个有意义的英文句子哈希冲突个数。数据 3 为数据 1 的哈希值与 1000003(大素数)求模后存储到线性表中冲突的个数。数据 4 为数据 1 的哈希值与 10000019(更大素数)求模后存储到线性表中冲突的个数。

经过比较，得出以上平均得分。平均数为平方平均数。可以发现，BKDRHash 无论是在实际效果还是编码实现中，效果都是最突出的。APHash 也是较为优秀的算法。DJBHash,JSHash,RSHash 与 SDBMHash 各有千秋。PJWHash 与 ELFHash 效果最差，但得分相似，其算法本质是相似的。

所有的字符串哈希算法都是基于对字符编码的迭代运算，只是运算规则不同而已。

1）BKDRHash 算法

```cpp
// BKDR Hash Function
unsigned int BKDRHash(char *str)
{
   unsigned int seed = 131; // 31 131 1313 13131 131313 etc..
   unsigned int hash = 0;

   while (*str)
   {
       hash = hash * seed + (*str++);
    }

    return (hash & 0x7FFFFFFF);
 }
```

关于 BKDRHash 算法的较为详细的解析可以参考 BKDRHash 详解
                            
2）APHash 算法

```cpp
// AP Hash Function
unsigned int APHash(char *str)
{
	unsigned int hash = 0;
	int i;

	for (i=0; *str; i++)
	{
		if ((i & 1) == 0)
		{
			hash ^= ((hash << 7) ^ (*str++) ^ (hash >> 3));
		}
		else
		{
			hash ^= (~((hash << 11) ^ (*str++) ^ (hash >> 5)));
		}
	}

	return (hash & 0x7FFFFFFF);
	}
```

3）DJBHash 算法

```cpp
// DJB Hash Function
unsigned int DJBHash(char *str)
{
	unsigned int hash = 5381;

	while (*str)
	{
	     hash += (hash << 5) + (*str++);
	}
	return (hash & 0x7FFFFFFF);
}
```

4）JSHash 算法

```cpp
// JS Hash Function
unsigned int JSHash(char *str)
{
	unsigned int hash = 1315423911;

	while (*str)
	{
		hash ^= ((hash << 5) + (*str++) + (hash >> 2));
	}
return (hash & 0x7FFFFFFF);
}
```

5）RSHash 算法

```cpp
unsigned int RSHash(char *str)
{
   unsigned int b = 378551;
   unsigned int a = 63689;
   unsigned int hash = 0;

   while (*str)
   {
        hash = hash * a + (*str++);
        a *= b;
    }

    return (hash & 0x7FFFFFFF);
}
```

// RS Hash Function

6）SDBMHash 算法

```cpp
unsigned int SDBMHash(char *str)
{
   unsigned int hash = 0;

   while (*str)
   {
  // equivalent to: hash = 65599*hash + (*str++);
  hash = (*str++) + (hash << 6) + (hash << 16) - hash;
   }

   return (hash & 0x7FFFFFFF);
}
```

7）PJWHash 算法

```cpp
// P. J. Weinberger Hash Function
unsigned int PJWHash(char *str)
{
	unsigned int BitsInUnignedInt = (unsigned int)(sizeof(unsigned int) * 8);
	unsigned int ThreeQuarters    = (unsigned int)((BitsInUnignedInt  * 3) / 4);
	unsigned int OneEighth        = (unsigned int)(BitsInUnignedInt / 8);
	unsigned int HighBits         = (unsigned int)(0xFFFFFFFF) << (BitsInUnignedInt - OneEighth);
	unsigned int hash             = 0;
	unsigned int test             = 0;

	while (*str)
	{
		hash = (hash << OneEighth) + (*str++);
		if ((test = hash & HighBits) != 0)
		{
			hash = ((hash ^ (test >> ThreeQuarters)) & (~HighBits));
		}
	}

	return (hash & 0x7FFFFFFF);
	}
```

8）ELFHash 算法

```cpp
// ELF Hash Function
unsigned int ELFHash(char *str)
{
	unsigned int hash = 0;
	unsigned int x    = 0;

	while (*str)
	{
		hash = (hash << 4) + (*str++);
		if ((x = hash & 0xF0000000L) != 0)
		{
			hash ^= (x >> 24);
			hash &= ~x;
		}
	}

	return (hash & 0x7FFFFFFF);
	}
```

## c 系列 IO

### cin/cout

默认 cin 和 cout 之间会同步刷新缓存。即调用一个时，另一个会先清空缓冲区，这些检查消耗时间。见cpp primer。

```cpp
 const int MAXN = 10000000;
 
 int numbers[MAXN];
 
 void cin_read_nosync()
 {
         freopen("data.txt","r",stdin);
         std::ios::sync_with_stdio(false);
         for (int i=0;i<MAXN;i++)
                 std::cin >> numbers[i];
 }
```

### fread

把整个文件读入一个字符串最常用的方法是用 fread，代码如下：

```cpp
const int MAXS = 60*1024*1024;
 char buf[MAXS];
 
 void analyse(char *buf,int len = MAXS)
 {
         int i;
         numbers[i=0]=0;
         for (char *p=buf;*p && p-buf<len;p++)
                 if (*p == ' ')
                         numbers[++i]=0;
                 else
                         numbers[i] = numbers[i] * 10 +*p - '0';
 }


 const int MAXN = 10000000;
 const int MAXS = 60*1024*1024;
 
 int numbers[MAXN];
 char buf[MAXS];
 
 void fread_analyse()
 {
         freopen("data.txt","rb",stdin);
         int len = fread(buf,1,MAXS,stdin);
         buf[len] = '\0';
         analyse(buf,len);
 }
```

### mmap

内存映射

```cpp
const int MAXN = 10000000;
const int MAXS = 60*1024*1024;
 
int numbers[MAXN];
char buf[MAXS];
void mmap_analyse()
{
	int fd = open("data.txt",O_RDONLY);
    int len = lseek(fd,0,SEEK_END);
    char *mbuf = (char *) mmap(NULL,len,PROT_READ，MAP_PRIVATE,fd,0);
    analyse(mbuf,len);
}
```

## POSIX信号

[参见链接](https://dsa.cs.tsinghua.edu.cn/oj/static/unix_signal.html)

| Signal  | Value    | Action | Comment                                                                 |
| ------- | -------- | ------ | ----------------------------------------------------------------------- |
| SIGHUP  | 1        | Term   | Hangup detected on controlling terminal or death of controlling process |
| SIGINT  | 2        | Term   | Interrupt from keyboard                                                 |
| SIGQUIT | 3        | Core   | Quit from keyboard                                                      |
| SIGILL  | 4        | Core   | Illegal Instruction                                                     |
| SIGABRT | 6        | Core   | Abort signal from abort(3)                                              |
| SIGFPE  | 8        | Core   | Floating point exception                                                |
| SIGKILL | 9        | Term   | Kill signal                                                             |
| SIGSEGV | 11       | Core   | Invalid memory reference                                                |
| SIGPIPE | 13       | Term   | Broken pipe: write to pipe with no readers                              |
| SIGALRM | 14       | Term   | Timer signal from alarm(2)                                              |
| SIGTERM | 15       | Term   | Termination signal                                                      |
| SIGUSR1 | 30,10,16 | Term   | User-defined signal 1                                                   |
| SIGUSR2 | 31,12,17 | Term   | User-defined signal 2                                                   |
| SIGCHLD | 20,17,18 | Ign    | Child stopped or terminated                                             |
| SIGCONT | 19,18,25 | Cont   | Continue if stopped                                                     |
| SIGSTOP | 17,19,23 | Stop   | Stop process                                                            |
| SIGTSTP | 18,20,24 | Stop   | Stop typed at terminal                                                  |
| SIGTTIN | 21,21,26 | Stop   | Terminal input for background process                                   |
| SIGTTOU | 22,22,27 | Stop   | Terminal output for background process                                  |

### 6 SIGABRT

若非你主动调用 abort()，则一般是断言失败，或者程序未捕获异常。常见的有：

1. 调用 C / C++ 标准库时（例如 delete 等），标准库有的会检查参数和当前状态，如果检查失败就会以 SIGABRT 中止。
2. 使用 new（或 malloc）的内存时，如果越界写入，就有可能在 delete（或 free）时出现 SIGABRT。
3. 未捕获的异常。
4. 调用 new 分配内存，内存不足时抛出 std::bad_alloc（例如 ```new int[1024 * 1024 * 1024]```）。
   
### 8 SIGFPE

浮点数计算错误。一般是整数除以 0。

### 11 SIGSEGV

段错误。常见的有：

1. 越界访问。包括越界读、越界写。
2. 访问（包括读和写）空指针、野指针、已经释放过的内存地址。
3. delete（或 free）野指针、已经释放过的内存地址。
4. 在函数体内部定义很大的变量。函数体内部定义的变量使用栈空间，栈的默认大小是 8 MB。
5. 递归层数过多，超出栈大小限制。
6. 写成 ```int n; scanf("%d", n);```，误把 n 的值当作地址传给 scanf 函数，导致 scanf 函数写野指针。
7. 调用 qsort() 排序时，必须保证传入的比较函数是有意义的。如果数组与比较函数不构成全序关系，例如有元素 a < b, b < a，就有可能出现段错误。
8. 我们对程序可以使用的内存大小设置了 soft limit，比题目限制的稍大一点。如果程序超出了虚拟内存限制，而小于 soft limit，那么结果是 Memory Limit Exceeded；如果程序直接超出了 soft limit，就会出现段错误。