---
layout: post
title: 如何在页面中使用Latex
categories: [math]
tags: [latex]
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script>
MathJax.Hub.Config({
  config: ["MMLorHTML.js"],
  extensions: ["tex2jax.js"],
  jax: ["input/TeX"],
  tex2jax: {
    inlineMath: [ ['$','$'], ["\\(","\\)"] ],
    displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
    processEscapes: false
  },
  TeX: {
    extensions: ["AMSmath.js", "AMSsymbols.js"],
    TagSide: "right",
    TagIndent: ".8em",
    MultLineWidth: "85%",
    equationNumbers: {
      autoNumber: "AMS",
    },
    unicode: {
      fonts: "STIXGeneral,'Arial Unicode MS'"
    }
  },
  showProcessingMessages: false
});
</script>

# 使用MathJax学习Latex中的数学表达方式

## 希腊字母

* $eta -> \eta$
* $lambda -> \lambda, Lambda -> \Lambda$
* $epsilon -> \epsilon, varepsilon -> \varepsilon$
* $upsilon -> \upsilon, Upsilon -> \Upsilon$
* $iota -> \iota$
* $beta -> \beta$
* $alpha -> \alpha$
* $delta -> \delta, Delta -> \Delta$
* $nabla -> \nabla$
* $pi -> \pi, varpi -> \varpi, Pi -> \Pi$
* $rho -> \rho, varrho -> \varrho$
* $tau -> \tau$
* $mu -> \mu$
* $psi -> \psi, Psi -> \Psi$
* $chi -> \chi$
* $xi -> \xi, Xi -> \Xi$
* $kappa -> \kappa$
* $sigma -> \sigma, varsigma -> \varsigma, Sigma -> \Sigma$
* $theta -> \theta, vartheta -> \vartheta, Theta -> \Theta$
* $phi -> \phi, varphi -> \varphi, Phi -> \Phi$
* $gamma -> \gamma, Gamma -> \Gamma$
* $omega -> \omega, Omega -> \Omega$

## 数学重音符号
* $hat -> \hat{a}$
* $dot -> \dot{a}$
* $tilde -> \tilde{a}$
* $acute -> \acute{a}$
* $grave -> \grave{a}$
* $bar -> \bar{a}$
* $ddot -> \ddot{a}$
* $vec -> \vec{a}$
* $check -> \check{a}$
* $mathring -> \mathring{a}$
* $widetilde -> \widetilde{abc}$
* $widehat -> \widehat{abcdef}$

## 二元关系符

* $ leq\quad or \quad le -> \quad \leq \le, \not \leq, ge/geq -> \ge \geq, ne -> \ne$
* $ equiv -> \equiv$
* $ doteq -> \doteq$
* $ ll -> \ll$
* $ gg -> \gg$
* $ sim -> \sim$
* $ simeq -> \simeq$
* $ approx -> \approx$
* $ in -> \in, notin -> \notin$
* $ subset -> \subset$
* $ subseteq -> \subseteq$
* $ supset -> \supset$
* $ supseteq -> \supseteq$
* $ propto -> \propto$
* $ parallel -> \parallel$

## 二元运算符

* $pm -> \pm$
* $mp -> \mp$
* $cdot -> \cdot$
* $times -> \times$
* $cap -> \cap$
* $cup -> \cup$

## 角标

下标和上标 $\sum\_{i = 1}^n a_i = 1 $

导数运算 $x' = x^\prime$

## 分式

除法运算 $\frac {x + y} { {x}^2 + 2} $, $x / y$

## 根式

$\sqrt{x + y}, \quad \sqrt[4]{x + y}$

不带横号的根号 $\surd{(x + y)}$

## 求和与积分

行内模式 $\sum_{i=1}^n x^i, \int_a^b f(x)dx$

单独一行的情况下， $$\sum_{i=1}^n x^i, \int_a^b f(x)dx$$

当然在行内模式下，可以手动地指定下标的位置，如$\sum\limits_{i=1}^n {x}^i$

带圆圈的积分符号: $${\oint_a}^b f(x)dx = x^2 + 1$$ 

## 上下符号

上下划线
\overline, \underline, 其中\overline比\bar要长一些

$$\overline{\overline a^2 + \underline{ab} + \bar b^2}$$

上下箭头 \overrightarrow, \underrightarrow
$$\overrightarrow{AB}, \underrightarrow{ABC}, \overleftarrow{XY}$$

上下括号 \overbrace, \underbrace
$$\underbrace{a + \overbrace{b + \cdots + d}^m + c}_n$$

## 堆积符号

\stackrel符号的格式, \stackrel{上位符号}{基位符号}
$$\vec{x} \stackrel{def}{=} (x_1, \ldots, x_n)$$

\atop, \choose的格式为 {上位符号 \atop 下位符号}
$$ \sum\_{1 < x < n \atop x \neq y} a_{ij}, {n + m \choose k}$$

## 定界符

\big(*1.5), \Big(*2), \bigg(*2.5), \Bigg(*3)

$\big(, \Big(, \bigg(, \Bigg($

自适应地放大
$$\left( \sum_{x=1}^m x^2 \right)$$

\left和\right必须要成对地出现，为了只用一个，则必须要将另外一个设置.
$$\left. \frac {\partial f} {\partial x} \right |_{x = 0}$$
