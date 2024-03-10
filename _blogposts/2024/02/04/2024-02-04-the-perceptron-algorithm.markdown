---
layout: post
title: "Test Post (Perceptron Algorithm)"
date: 2024-02-04 12:45:00 +0000
categories: ['machine learning', 'perceptron', 'algorithm']
permalink: /blog/2024/02/07/the-perceptron-algorithm/
---

<pre><code></code></pre>

My problems with this algorithm began when people online were suggesting that a step (piecewise constant) function is differentiable (including at points of discontinuity). "Just throw the gradient[^1] descent optimisation algorithm at it!" is not gonna fly with me. I went in search of a more rigorous treatment of this algorithm.

\begin{equation} \dfrac{\partial}{\partial \theta_{i}} f(x, \Theta) \tag{1} \label{eqone} \end{equation}

This is a reference to the tagged equation \eqref{eqone}.

<!-- \tag{symbol} \label{equation-name} -->
<!-- \eqref{equation-name} -->

[^1]: Some footnote.