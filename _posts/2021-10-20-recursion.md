---
title : "Recursion"
categories : 
    - Data Structure
tag :
    - [Linear List, Linked List]  # [C, python]
author_profile: false
sidebar:
    nav: "docs"
use_math: true
toc: true # 목차
toc_sticky: true
toc_label: "On This Page"
---

##dfd

$\overbrace{abcxyz}^{alphabets}$

	$left{ right}$

$f(n)$ 

<span style = "font-size:2em;  color: black;">{</span>n * f(n+-1) ... n <= 1


$left\{ begin{array}{cc} 1 \\ 2 \\ 3 end{array} right$


                $f(n - 1)$
$f(n) = \Bigg\{ \f(n - 1)\\ \\1$


$\documentclass{article}

\usepackage{amsmath,etoolbox}

\makeatletter
\renewenvironment{cases}[1][\lbrace]{%
  \def\@ldelim{#1}
  \matrix@check\cases\env@cases
}{%
  \endarray\right.%
}
\patchcmd{\env@cases}{\lbrace}{\@ldelim}{}{}
\makeatother$$


$

$$
  f(x) = \begin{cases}  
    n f(n-1) & \text{$n >= 1$} \\  
    1 & \text{$n = 0$}  
  \end{cases}
$$