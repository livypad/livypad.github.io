- [tex](#tex)
	- [模板](#模板)
	- [符号](#符号)
- [数据结构](#数据结构)
	- [hash](#hash)

# tex

## 模板

```latex
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

其中数据1为100000个字母和数字组成的随机串哈希冲突个数。数据2为100000个有意义的英文句子哈希冲突个数。数据3为数据1的哈希值与 1000003(大素数)求模后存储到线性表中冲突的个数。数据4为数据1的哈希值与10000019(更大素数)求模后存储到线性表中冲突的个数。

经过比较，得出以上平均得分。平均数为平方平均数。可以发现，BKDRHash无论是在实际效果还是编码实现中，效果都是最突出的。APHash也是较为优秀的算法。DJBHash,JSHash,RSHash与SDBMHash各有千秋。PJWHash与ELFHash效果最差，但得分相似，其算法本质是相似的。

所有的字符串哈希算法都是基于对字符编码的迭代运算，只是运算规则不同而已。

1）BKDRHash算法

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

关于BKDRHash算法的较为详细的解析可以参考BKDRHash详解
2）APHash算法

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

3）DJBHash算法
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

4）JSHash算法
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

5）RSHash算法
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


6）SDBMHash算法
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
7）PJWHash算法
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
8）ELFHash算法
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
