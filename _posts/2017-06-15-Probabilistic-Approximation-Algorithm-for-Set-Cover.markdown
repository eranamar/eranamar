---
layout: post
author: Eran Amar
title:  Probabilistic Approximation Algorithm for Set Cover
date:   2017-06-15
comments: true
tags: algorithms approximation probability_theory
---


<script type="math/tex">
\newcommand{\lyxlock}{}
</script>
<noscript>
<div class="warning">
Warning: <a href="http://www.mathjax.org/">MathJax</a> requires JavaScript to correctly process the mathematics on this page. Please enable JavaScript on your browser.
</div><hr>
</hr></noscript>



<h1 class="Section">
<a class="toc" name="toc-Section-1">1</a> The Weighted Set Cover Problem
</h1>
<div class="Unindented">
In the <i>Weighted Set Cover</i> (WSC) problem, the input is a triple <span class="MathJax_Preview"><script type="math/tex">
\left(U,\mathcal{F},w\right)
</script>
</span> where <span class="MathJax_Preview"><script type="math/tex">
U
</script>
</span> is a universe of <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> elements, <span class="MathJax_Preview"><script type="math/tex">
\mathcal{F}=\left\{ A_{1},A_{2},..,A_{m}\right\} 
</script>
</span> is a family of <span class="MathJax_Preview"><script type="math/tex">
m
</script>
</span> sets such that each <span class="MathJax_Preview"><script type="math/tex">
A_{i}\subseteq U
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\bigcup_{A\in\mathcal{F}}A=U
</script>
</span>, and <span class="MathJax_Preview"><script type="math/tex">
w:\mathcal{F}\to\mathbb{R}_{>0}
</script>
</span> is a weight function that assigns positive weight to each set in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{F}
</script>
</span>. A solution <span class="MathJax_Preview"><script type="math/tex">
S\subseteq\mathcal{F}
</script>
</span> for the <i>WSC</i> problem is <i>valid</i> if and only if <span class="MathJax_Preview"><script type="math/tex">
\bigcup_{A\in S}A=U
</script>
</span>, the weight of the solution is <span class="MathJax_Preview"><script type="math/tex">
w\left(S\right)=\sum_{A\in S}w\left(A\right)
</script>
</span>. Note that there is always a valid solution, and the goal in the WSC problem is to find such one with minimum weight. 
</div>
<div class="Indented">
Weighted Set Cover is “hard” in many aspects. First, it is hard to efficiently find the <i>exact solution</i>, that is, the problem is NP-Complete so no time-efficient algorithm is known for it. Moreover, it is also hard to approximate the solution better than a <span class="MathJax_Preview"><script type="math/tex">
\log n
</script>
</span> factor. Formally, we said that a solution <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> is a <span class="MathJax_Preview"><script type="math/tex">
\log n
</script>
</span> approximation for WSC if <span class="MathJax_Preview"><script type="math/tex">
w\left(S\right)\le w\left(S^{*}\right)\cdot\log n
</script>
</span>, when <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span> is an optimal solution with minimum weight. The WSC is <i>hard to approximate</i> in the sense that any <span class="MathJax_Preview"><script type="math/tex">
\left(1-\epsilon\right)\cdot\log n
</script>
</span> approximation for that problem is NP-Complete by itself, and that is for any <span class="MathJax_Preview"><script type="math/tex">
\epsilon>0
</script>
</span>.
</div>
<div class="Indented">
In this post we will describe a probabilistic <span class="MathJax_Preview"><script type="math/tex">
c\cdot\log n
</script>
</span> approximation algorithm for WSC which runs in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(poly\left(n,m\right)\right)
</script>
</span> time, and <span class="MathJax_Preview"><script type="math/tex">
c
</script>
</span> is some positive constant that controls the success probability of the algorithm.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Equivalent Formulation
</h1>
<div class="Unindented">
We can reformulate the Weighted Set Cover problem as a system of linear equations as follows: For any set <span class="MathJax_Preview"><script type="math/tex">
A_{i}
</script>
</span> define an indicator variable <span class="MathJax_Preview"><script type="math/tex">
X_{i}\in\left\{ 0,1\right\} 
</script>
</span> and let <span class="MathJax_Preview"><script type="math/tex">
w\left(X_{i}\right):=w\left(A_{i}\right)\cdot X_{i}
</script>
</span>, thus if <span class="MathJax_Preview"><script type="math/tex">
X_{i}=1
</script>
</span> then <span class="MathJax_Preview"><script type="math/tex">
w\left(X_{i}\right)=w\left(A_{i}\right)
</script>
</span>, and <span class="MathJax_Preview"><script type="math/tex">
w\left(X_{i}\right)=0
</script>
</span> otherwise. The objective is then to minimize <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\sum_{i=1}^{m}w\left(X_{i}\right)

</script>
</span>
 subject to the <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> following linear constrains <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\forall u\in U:\quad\sum_{i\text{ s.t. }u\in A_{i}}X_{i}\ge1

</script>
</span>
</div>
<div class="Indented">
Given a solution <span class="MathJax_Preview"><script type="math/tex">
l=\left(x_{1},..,x_{m}\right)\in\left\{ 0,1\right\} ^{m}
</script>
</span> to that system of equations, we can define the corresponding solution for WSC as <span class="MathJax_Preview"><script type="math/tex">
S=\left\{ A_{i}\in\mathcal{F}\mid x_{i}=1\right\} 
</script>
</span>. Note that by this construction <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> must be <i>valid</i>, moreover, the weights are the same: <span class="MathJax_Preview"><script type="math/tex">
w\left(l\right)=w\left(S\right)
</script>
</span>, so if <span class="MathJax_Preview"><script type="math/tex">
w\left(l\right)
</script>
</span> is minimal then also <span class="MathJax_Preview"><script type="math/tex">
w\left(S\right)
</script>
</span>. The proof of the other direction (from WSC to the system of equations) is similar.
</div>
<div class="Indented">
Because the linear-time mapping from any optimal solution of one problem to an optimal solution for the other, we can conclude that solving this system of equations is NP-Hard. However, if we relax the “integer requirement”, i.e. that <span class="MathJax_Preview"><script type="math/tex">
X_{i}\in\left\{ 0,1\right\} 
</script>
</span>, then the problem becomes much easier. That is, when <span class="MathJax_Preview"><script type="math/tex">
X_{i}\in\left[0,1\right]
</script>
</span> for any <span class="MathJax_Preview"><script type="math/tex">
i
</script>
</span>, that system becomes a convex optimization problem, which we can solved with a polynomial-time algorithm. From now on, we will refer only to the convex version, and denote its (exact) solution as <i>fractional solution</i>.
</div>
<div class="Indented">
So suppose we have a fractional optimal solution for the linear system <span class="MathJax_Preview"><script type="math/tex">
l^{*}=\left(x_{1},..,x_{m}\right)\in\left[0,1\right]^{m}
</script>
</span>. Note that <span class="MathJax_Preview"><script type="math/tex">
w\left(l^{*}\right)\le w\left(S^{*}\right)
</script>
</span> for any optimal solution <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span> for WSC, because the convex formulation allows for more possible valid solutions so the minimum weight can only be smaller than of <span class="MathJax_Preview"><script type="math/tex">
w\left(S^{*}\right)
</script>
</span>. 
</div>
<div class="Indented">
How do we get from <span class="MathJax_Preview"><script type="math/tex">
l^{*}
</script>
</span> a solution for the WSC? The nice trick here is <b>probabilistic rounding</b>. That is, we will treat each <span class="MathJax_Preview"><script type="math/tex">
x_{i}
</script>
</span> as the probability for selecting <span class="MathJax_Preview"><script type="math/tex">
A_{i}
</script>
</span>. 
</div>
<div class="Indented">
We define the following procedure <span class="MathJax_Preview"><script type="math/tex">
Alg\left(U,\mathcal{F},w\right)
</script>
</span> as an helper function:
</div>
<ol>
<li>
Repeat for <span class="MathJax_Preview"><script type="math/tex">
t=1,...,\left(2\log n\right)
</script>
</span> times:<ol>
<li>
Initialize <span class="MathJax_Preview"><script type="math/tex">
S_{t}=\emptyset
</script>
</span>.
</li>
<li>
Solve the convex optimization version of the WSC to get an optimal fractional solution <span class="MathJax_Preview"><script type="math/tex">
l_{t}^{*}
</script>
</span>.
</li>
<li>
For each <span class="MathJax_Preview"><script type="math/tex">
A_{i}\in\mathcal{F}
</script>
</span>, add it to <span class="MathJax_Preview"><script type="math/tex">
S_{t}
</script>
</span> with probability <span class="MathJax_Preview"><script type="math/tex">
x_{i}\in l_{t}^{*}
</script>
</span>.
</li>
<li>
Let the “solution” for the current iteration be <span class="MathJax_Preview"><script type="math/tex">
S_{t}
</script>
</span>.
</li>
</ol>
</li>
<li>
Output <span class="MathJax_Preview"><script type="math/tex">
\bigcup_{t=1}^{2\log n}S_{t}
</script>
</span>
</li>
</ol>
<div class="Unindented">
The final algorithm is just applying the <span class="MathJax_Preview"><script type="math/tex">
Alg\left(U,\mathcal{F},w\right)
</script>
</span> procedure <span class="MathJax_Preview"><script type="math/tex">
c
</script>
</span> independent times, and keeping the candidate solutions <span class="MathJax_Preview"><script type="math/tex">
S^{\left(1\right)},...,S^{\left(c\right)}
</script>
</span>, then returning the <span class="MathJax_Preview"><script type="math/tex">
S_{final}
</script>
</span> solution with minimum weight among those candidates. 
</div>
<div class="Indented">
Note that all the procedures above have polynomial time (in <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
m
</script>
</span>), therefore the total running time of the algorithm is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(poly\left(n,m\right)\right)
</script>
</span>.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> The Solution is <i>Approximately</i> Optimal
</h1>
<div class="Unindented">
We want to show that with high probability <span class="MathJax_Preview"><script type="math/tex">
w\left(S_{final}\right)\le c\cdot\log n\cdot w\left(S^{*}\right)
</script>
</span>. It is easy to see that for each iteration of the sub-procedure <span class="MathJax_Preview"><script type="math/tex">
Alg
</script>
</span>, the intermediate set <span class="MathJax_Preview"><script type="math/tex">
S_{t}
</script>
</span> has expected weight equals to the weight of the optimal fractional solution. Formally, <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbb{E}\left[w\left(S_{t}\right)\right]=\sum_{i=1}^{m}w\left(A_{i}\right)\mathbb{E}\left[X_{i}\right]=\sum_{i=1}^{m}w\left(A_{i}\right)x_{i}=w\left(l_{t}^{*}\right)

</script>
</span>
note that the optimal value of all the fractional solutions is the same so we will remove the subscript <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>. Next, we continue to calculate the expected weight of the output solution of the sub-procedure, that is, the weight of the candidate <span class="MathJax_Preview"><script type="math/tex">
S^{\left(i\right)}
</script>
</span>
</div>
<div class="Indented">
<span class="MathJax_Preview">
<script type="math/tex;mode=display">
\begin{aligned}
\mathbb{E}\left[w\left(S^{\left(i\right)}\right)\right] & =\mathbb{E}\left[w\left(\bigcup_{t=1}^{2\log n}S_{t}\right)\right]\\
 & \le\sum_{t=1}^{2\log n}\mathbb{E}\left[w\left(S_{t}\right)\right]\\
 & =2\log n\cdot w\left(l^{*}\right)\\
 & \le2\log n\cdot w\left(S^{*}\right)
\end{aligned}
</script>
</span>
It left to show that with high probability all the candidates <span class="MathJax_Preview"><script type="math/tex">
\left\{ S^{\left(i\right)}\right\} _{i}
</script>
</span> are within the weight bound of <span class="MathJax_Preview"><script type="math/tex">
c\cdot\log n\cdot w\left(S^{*}\right)
</script>
</span>, and because <span class="MathJax_Preview"><script type="math/tex">
S_{final}
</script>
</span> is being selected from those candidates, we conclude that it also has <span class="MathJax_Preview"><script type="math/tex">
\mathbb{E}\left[w\left(S_{final}\right)\right]\le c\cdot\log n\cdot w\left(S^{*}\right)
</script>
</span>. In order to get high probability for a single candidate, we can use Markov’s inequality as follows. Define a “failure” of a candidate <span class="MathJax_Preview"><script type="math/tex">
S^{\left(k\right)}
</script>
</span> as <span class="MathJax_Preview"><script type="math/tex">
w\left(S^{\left(k\right)}\right)\ge c\cdot\log n\cdot w\left(S^{*}\right)
</script>
</span>, then the probability of a single candidate to fail is bounded by <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbb{P}\left[w\left(S^{\left(k\right)}\right)\ge c\cdot\log n\cdot w\left(S^{*}\right)\right]\le\frac{\mathbb{E}\left[w\left(S^{\left(k\right)}\right)\right]}{c\cdot\log n\cdot w\left(S^{*}\right)}\le\frac{2}{c}

</script>
</span>
So with probability of at least <span class="MathJax_Preview"><script type="math/tex">
1-\frac{2}{c}
</script>
</span> the candidate has <span class="MathJax_Preview"><script type="math/tex">
w\left(S^{\left(k\right)}\right)\le c\cdot\log n\cdot w\left(S^{*}\right)
</script>
</span> as required.
</div>
<div class="Indented">
When considering the final solution <span class="MathJax_Preview"><script type="math/tex">
S_{final}
</script>
</span>, it fails only when all the candidates fail and the probability for that is <span class="MathJax_Preview"><script type="math/tex">
\le\left(\frac{2}{c}\right)^{c}
</script>
</span>. So we have high constant probability to get a solution that is <span class="MathJax_Preview"><script type="math/tex">
c\cdot\log n
</script>
</span> approximation for the optimal solution.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> The Solution is <i>Probably</i> Valid
</h1>
<div class="Unindented">
What is the probability that the returned solution <span class="MathJax_Preview"><script type="math/tex">
S_{final}
</script>
</span> is not valid? that is, there is at least one element <span class="MathJax_Preview"><script type="math/tex">
u\in U
</script>
</span> that is not covered by <span class="MathJax_Preview"><script type="math/tex">
S_{final}
</script>
</span>. We analyze that by first considering an arbitrary element <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> and an intermediate solution <span class="MathJax_Preview"><script type="math/tex">
S_{t}
</script>
</span> of the sub procedure <span class="MathJax_Preview"><script type="math/tex">
Alg
</script>
</span>. Suppose that <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> belongs to <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span> sets, and for simplicity suppose they are <span class="MathJax_Preview"><script type="math/tex">
A_{1},...,A_{k}
</script>
</span>. We know that their corresponding variables have <span class="MathJax_Preview"><script type="math/tex">
\sum_{i=1}^{k}x_{i}\ge1
</script>
</span> by the requirements of the linear system. The probability to leave all these sets outside <span class="MathJax_Preview"><script type="math/tex">
S_{t}
</script>
</span> is <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\prod_{i=1}^{k}\left(1-x_{i}\right)\le\prod_{i=1}^{k}e^{-x_{i}}\le e^{-1}

</script>
</span>
that is, the probability that an intermediate solution <span class="MathJax_Preview"><script type="math/tex">
S_{t}
</script>
</span> will not cover <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> is up to <span class="MathJax_Preview"><script type="math/tex">
e^{-1}
</script>
</span>.
</div>
<div class="Indented">
Next, we calculate the probability that <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> is not covered by a <span class="MathJax_Preview"><script type="math/tex">
S_{final}
</script>
</span> where <span class="MathJax_Preview"><script type="math/tex">
S_{final}=S^{\left(i\right)}
</script>
</span> for some selected candidate <span class="MathJax_Preview"><script type="math/tex">
S^{\left(i\right)}
</script>
</span>. Then it must be that <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> is not covered by any intermediate solution <span class="MathJax_Preview"><script type="math/tex">
S_{1},..,S_{2\log n}
</script>
</span> of the procedure <span class="MathJax_Preview"><script type="math/tex">
Alg
</script>
</span>, and that will happen with probability <span class="MathJax_Preview"><script type="math/tex">
\le\prod_{t=1}^{2\log n}e^{-1}=\frac{1}{n^{2}}
</script>
</span>. Applying the union-bound on all the <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> elements, we conclude that with probability of at most <span class="MathJax_Preview"><script type="math/tex">
\frac{1}{n}
</script>
</span>, the selected candidate <span class="MathJax_Preview"><script type="math/tex">
S^{\left(i\right)}
</script>
</span> will not cover the entire universe. In other words, with at least <span class="MathJax_Preview"><script type="math/tex">
1-\frac{1}{n}
</script>
</span> probability, <span class="MathJax_Preview"><script type="math/tex">
S_{final}
</script>
</span> is a valid solution.
</div>
