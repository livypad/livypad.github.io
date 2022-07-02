[cs主界面](cs.md)
- [tex](#tex)
  - [模板](#模板)
  - [符号](#符号)

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