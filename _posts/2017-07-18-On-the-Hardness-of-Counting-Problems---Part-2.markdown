---
layout: post
author: Eran Amar
title:  On the Hardness of Counting Problems - Part 2
date:   2017-07-18
comments: true
tags: complexity approximation
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
<a class="toc" name="toc-Section-1">1</a> Recap
</h1>
<div class="Unindented">
The <a class="URL" href="https://eranamar.github.io/site/2017/07/06/On-the-Hardness-of-Counting-Problems-Part-1.html">previous post</a> discussed the <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P},\mathcal{NP}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\#\mathcal{P}
</script>
</span> families. We said that the hierarchy between decision problems from <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P},\mathcal{NP}
</script>
</span> do not preserved when moving to their Counting versions. We defined <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
DNF
</script>
</span> formulas (see section 2 in previous post), and the problems <i>CNF-SAT </i>that is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span>-complete and <i>DNF-SAT</i> that is in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}
</script>
</span>. We used the relation between their Counting versions to show that <span class="MathJax_Preview"><script type="math/tex">
\#DNF
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\#CNF
</script>
</span> are equally hard to be solved exactly. The relation is as follows: if <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> is a <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> variables <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> formula, then <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\#CNF\left(\phi\right)+\#DNF\left(\neg\phi\right)=2^{n}

</script>
</span>
Furthermore, we showed that both problems are <span class="MathJax_Preview"><script type="math/tex">
\#\mathcal{P}
</script>
</span>-complete, demonstrating that even an “easy” decision problem like <i>DNF-SAT</i> can be very hard when considering its Counting counterpart. 
</div>
<div class="Indented">
This post we will show that there is some hierarchy inside <span class="MathJax_Preview"><script type="math/tex">
\#\mathcal{P}
</script>
</span> when considering <i>approximated solutions</i> instead of exact solutions.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Approximation Algorithm for <i>#DNF-SAT</i>
</h1>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-2.1">2.1</a> Deterministic Approximation
</h2>
<div class="Unindented">
Let <span class="MathJax_Preview"><script type="math/tex">
\psi=\bigvee_{i=1}^{m}C_{i}
</script>
</span> be a <span class="MathJax_Preview"><script type="math/tex">
DNF
</script>
</span> formula, and <span class="MathJax_Preview"><script type="math/tex">
k^{*}=\#DNF\left(\psi\right)
</script>
</span>. We want to find a polynomial time algorithm that yields some <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span> which is an approximation for <span class="MathJax_Preview"><script type="math/tex">
k^{*}
</script>
</span>. Formally, for an approximation factor of <span class="MathJax_Preview"><script type="math/tex">
p>1
</script>
</span>, <span class="MathJax_Preview"><script type="math/tex">
k^{*}\le k\le p\cdot k^{*}
</script>
</span>. We will talk more about the approximation parameter later.
</div>
<div class="Indented">
First note that if a clause contains a variable and its negation then immediately we can conclude that there are no satisfying assignments (s.a.) for that clause as it contains a contradiction, so we will omit it from the formula (can be done in linear time). So from now on, assume that <span class="MathJax_Preview"><script type="math/tex">
\psi
</script>
</span> do not contain any contradiction. For the rest of this discussion, we say that a boolean variable <i>appear</i> in a clause <span class="MathJax_Preview"><script type="math/tex">
C
</script>
</span> if either it or its negation contained in <span class="MathJax_Preview"><script type="math/tex">
C
</script>
</span>.
</div>
<div class="Indented">
Let’s focus on an arbitrary clause <span class="MathJax_Preview"><script type="math/tex">
C_{i}\in\psi
</script>
</span> . We can easily count the number of s.a. it has; it is exactly <span class="MathJax_Preview"><script type="math/tex">
\#DNF\left(C_{i}\right)=2^{t}
</script>
</span> when <span class="MathJax_Preview"><script type="math/tex">
t\in\left\{ 0,1,..,n-1\right\} 
</script>
</span> is the number of variables that do not appear in <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span>. That is true because the value of any variable in <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span> is determined by its appearance (if it has negation or not) so only variables which are not in <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span> can be either <span class="MathJax_Preview"><script type="math/tex">
True
</script>
</span> or <span class="MathJax_Preview"><script type="math/tex">
False
</script>
</span> and having two options.
</div>
<div class="Indented">
Moreover, we can <b>efficiently </b>sample at uniform any s.a. for that clause. Simply iterate over the <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> variables, if a variable appears in <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span> then it have only one allowed value so assign it that value, and if it is not appear in <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span> then toss a coin and assign <span class="MathJax_Preview"><script type="math/tex">
True
</script>
</span> or <span class="MathJax_Preview"><script type="math/tex">
False
</script>
</span> accordingly. 
</div>
<div class="Indented">
Trivial approximation algorithm will calculate the exact <span class="MathJax_Preview"><script type="math/tex">
\#DNF\left(C_{i}\right)
</script>
</span> for each <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span> in <span class="MathJax_Preview"><script type="math/tex">
\psi
</script>
</span> and returns their sum <span class="MathJax_Preview"><script type="math/tex">
k=\sum_{i=1}^{m}\#DNF\left(C_{i}\right)
</script>
</span>. However, we may have s.a. that were counted more than once (if they are satisfying more than a single clause). Thus the approximation factor of that algorithm is <span class="MathJax_Preview"><script type="math/tex">
m
</script>
</span>, because in the worst case all the clauses have the same set of satisfying assignments so we counted each s.a. <span class="MathJax_Preview"><script type="math/tex">
m
</script>
</span> times, yielding <span class="MathJax_Preview"><script type="math/tex">
k\le m\cdot k^{*}
</script>
</span>.
</div>
<div class="Indented">
We can improve that algorithm by using the inclusion-exclusion principle: instead of accumulating the number of s.a. for each clause, accumulate the number of s.a. that <i>weren’t count so far</i>.
</div>
<div class="Indented">
Unfortunately, finding which assignment was already counted forces us to enumerate all possible assignments, which obviously takes exponential time. Hence we will give a probabilistic estimation for that, instead. When working with estimations, we need to state the success probability for our estimation and it is also preferable that we will have a way to increase that probability (mostly at the cost of more computations and drawing more random samples). 
</div>
<div class="Indented">
Next section we will describe how to get an estimation of <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span> such that <span class="MathJax_Preview"><script type="math/tex">
\left(1-p\right)k^{*}\le k\le\left(1+p\right)k^{*}
</script>
</span> for <b>any</b> <span class="MathJax_Preview"><script type="math/tex">
p
</script>
</span>, even a polynomially small one (i.e. <span class="MathJax_Preview"><script type="math/tex">
p=\frac{1}{poly\left(n\right)}
</script>
</span>) with a very high probability. 
</div>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-2.2">2.2</a> Probabilistic Approximation
</h2>
<div class="Unindented">
The overall procedure is still simple: iterate over each clause <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span> and estimate the <i>fraction</i> of s.a. for <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span> that were not counted before (as a number between zero and one), denote that estimation by <span class="MathJax_Preview"><script type="math/tex">
\tilde{k}_{i}
</script>
</span>, let <span class="MathJax_Preview"><script type="math/tex">
S_{i}
</script>
</span> be the total number of s.a. for <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span> calculated as mentioned in the deterministic approximation section. Then output <span class="MathJax_Preview"><script type="math/tex">
k=\sum_{i=1}^{m}\tilde{k}_{i}\cdot S_{i}
</script>
</span>.
</div>
<div class="Indented">
It left to show how to compute <span class="MathJax_Preview"><script type="math/tex">
\tilde{k}_{i}
</script>
</span>, so we do the following. Let <span class="MathJax_Preview"><script type="math/tex">
t=c\left(\frac{m}{p}\right)^{2}\ln m
</script>
</span>, when <span class="MathJax_Preview"><script type="math/tex">
c
</script>
</span> will be determined later. Sample <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> independent times, an uniformly random s.a. for <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span>. For each assignment define a corresponding binary variable <span class="MathJax_Preview"><script type="math/tex">
X_{j}
</script>
</span> that is <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span> if it is not satisfying any previous clause, and <span class="MathJax_Preview"><script type="math/tex">
0
</script>
</span> otherwise. Note that evaluating all <span class="MathJax_Preview"><script type="math/tex">
\left\{ X_{j}\right\} _{j=1}^{t}
</script>
</span> takes <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(poly\left(n,m\right)\cdot t\right)
</script>
</span> time. Then return <span class="MathJax_Preview"><script type="math/tex">
\tilde{k}_{i}=\frac{1}{t}\sum_{j=1}^{t}X_{j}
</script>
</span>. That completes our probabilistic approximation algorithm for <span class="MathJax_Preview"><script type="math/tex">
\#DNF
</script>
</span>.
</div>
<h3 class="Subsubsection-">
<a class="toc" name="toc-Subsubsection--1"></a>Analysis
</h3>
<div class="Unindented">
We need to analyze the quality of approximation of each <span class="MathJax_Preview"><script type="math/tex">
\tilde{k_{i}}
</script>
</span>, then show that with high probability <span class="MathJax_Preview"><script type="math/tex">
\sum_{i=1}^{m}\tilde{k}_{i}\cdot S_{i}
</script>
</span> is within the claimed approximation bounds. For the first part, we focus on a single estimation <span class="MathJax_Preview"><script type="math/tex">
\tilde{k}_{i}
</script>
</span> for the clause <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span>. 
</div>
<div class="Indented">
Let <span class="MathJax_Preview"><script type="math/tex">
S_{i}
</script>
</span> be the number of all s.a. for <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span> and let <span class="MathJax_Preview"><script type="math/tex">
s_{i}
</script>
</span> be the number of s.a. for <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span> that <i>aren’t satisfying any clause before <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span></i>. Out goal is to show that <span class="MathJax_Preview"><script type="math/tex">
\tilde{k}_{i}
</script>
</span> is, with high probability, close to <span class="MathJax_Preview"><script type="math/tex">
\frac{s_{i}}{S_{i}}
</script>
</span>. Observe that all the variables <span class="MathJax_Preview"><script type="math/tex">
\left\{ X_{j}\right\} _{j=1}^{t}
</script>
</span> for estimating <span class="MathJax_Preview"><script type="math/tex">
\tilde{k}_{i}
</script>
</span> were actually sampled i.i.d from a Bernoulli distribution with parameter <span class="MathJax_Preview"><script type="math/tex">
q_{i}=\frac{s_{i}}{S_{i}}
</script>
</span> (which is also its expectation). As a result <span class="MathJax_Preview"><script type="math/tex">
\mathbb{E}\left[\tilde{k}_{i}\right]=q_{i}
</script>
</span>, so apply <a class="URL" href="https://eranamar.github.io/site/2017/03/24/Comparing-Chernoff-Hoeffding-bounds.html">Hoeffding inequality</a> to get <span class="MathJax_Preview">
<script type="math/tex;mode=display">
\begin{aligned}
\mathbf{P}\left[\left|\tilde{k}_{i}-q_{i}\right|>\frac{p}{m}\right] & \le2\exp\left(\frac{-2\left(\frac{p}{m}\right)^{2}t^{2}}{t}\right)\\
 & =2\exp\left(-2c\ln m\right)=2m^{-2c}
\end{aligned}
</script>
</span>
which means that with high probability the estimator is “successful”, that is,<span class="MathJax_Preview">
<script type="math/tex;mode=display">

q_{i}-\frac{p}{m}\le\tilde{k}_{i}\le q_{i}+\frac{p}{m}

</script>
</span>
and equivalently, <span class="MathJax_Preview">
<script type="math/tex;mode=display">

s_{i}-S_{i}\frac{p}{m}\le\tilde{k}_{i}\cdot S_{i}\le s_{i}+S_{i}\frac{p}{m}

</script>
</span>
If all the estimators <span class="MathJax_Preview"><script type="math/tex">
\tilde{k}_{1},\tilde{k}_{2},..
</script>
</span> are successful, then <span class="MathJax_Preview">
<script type="math/tex;mode=display">

k\le\sum_{i=1}^{m}\left(s_{i}+S_{i}\frac{p}{m}\right)=k^{*}+\sum_{i=1}^{m}S_{i}\frac{p}{m}\le k^{*}+\frac{p}{m}\left(mk^{*}\right)=\left(1+p\right)k^{*}

</script>
</span>
and also similarly <span class="MathJax_Preview"><script type="math/tex">
k\ge\left(1-p\right)k^{*}
</script>
</span>, which are exactly the approximation bounds that we want to achieve. To conclude the analysis, note that by applying the union bound, all the estimators will be successful with probability <span class="MathJax_Preview"><script type="math/tex">
\ge1-\sum_{i=1}^{m}2m^{-2c}=1-\frac{2}{m^{2c-1}}
</script>
</span>.
</div>
<h3 class="Subsubsection-">
<a class="toc" name="toc-Subsubsection--2"></a>Trade off between the parameters
</h3>
<div class="Unindented">
It is interesting to examine the relation between the different parameters, and how each affect the running time, success probability, and the quality of approximation.
</div>
<div class="Indented">
Consider the parameter <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>, which is the number of samples the algorithm has to draw to estimate each <span class="MathJax_Preview"><script type="math/tex">
\tilde{k}_{i}
</script>
</span>. That <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> depends linearly on the parameter <span class="MathJax_Preview"><script type="math/tex">
c
</script>
</span> which controls the “success probability” of all the estimators (simultaneously). On the one hand, large <span class="MathJax_Preview"><script type="math/tex">
c
</script>
</span> gives a success probability that is extremely close to <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span>. But on the other hand, it increase the number of samples <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> per clause, and the total running time of the algorithm, which is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(poly\left(n,m\right)\cdot c\cdot p^{-2}\right)
</script>
</span>.
</div>
<div class="Indented">
Similarly, the better approximation we want for <span class="MathJax_Preview"><script type="math/tex">
k^{*}
</script>
</span>, the smaller <span class="MathJax_Preview"><script type="math/tex">
p
</script>
</span> that we should set (even smaller than <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span>), then the number of samples and the running time increase inversely proportional to <span class="MathJax_Preview"><script type="math/tex">
p^{2}
</script>
</span>.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> What About Approximating <i>#CNF-SAT</i>?
</h1>
<div class="Unindented">
Previous section dealt with approximating, either in the deterministic or probabilistic manner, the <span class="MathJax_Preview"><script type="math/tex">
\#DNF
</script>
</span> problem. Unfortunately, as oppose to that we did with exact solutions, approximating <span class="MathJax_Preview"><script type="math/tex">
\#DNF
</script>
</span> do not help us approximating <span class="MathJax_Preview"><script type="math/tex">
\#CNF
</script>
</span>. We will prove that by proving a stronger claim.
</div>
<div class="Claim">
Let <span class="MathJax_Preview"><script type="math/tex">
P
</script>
</span> be any <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span>-complete problem, and denote <span class="MathJax_Preview"><script type="math/tex">
\#P
</script>
</span> its Counting versions. There is no polynomial-time approximation algorithm of <span class="MathJax_Preview"><script type="math/tex">
\#P
</script>
</span> to <b>any factor</b>, unless <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}=\mathcal{NP}
</script>
</span>.
</div>
<div class="Proof">
Assume that <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}\ne\mathcal{NP}
</script>
</span>. Suppose toward contradiction that we have some polynomial-time approximation algorithm for <span class="MathJax_Preview"><script type="math/tex">
\#P
</script>
</span>, and denote it by <span class="MathJax_Preview"><script type="math/tex">
Alg
</script>
</span>. Given an instance <span class="MathJax_Preview"><script type="math/tex">
x
</script>
</span> for the <b>decision</b> problem <span class="MathJax_Preview"><script type="math/tex">
P
</script>
</span>, simulate <span class="MathJax_Preview"><script type="math/tex">
Alg\left(x\right)
</script>
</span> and get an approximation for <span class="MathJax_Preview"><script type="math/tex">
\#P\left(x\right)
</script>
</span>. Then declare <span class="MathJax_Preview"><script type="math/tex">
No
</script>
</span> as the output of the decision problem if that approximation is zero, and <span class="MathJax_Preview"><script type="math/tex">
Yes
</script>
</span> otherwise. Note that this reduction runs in polynomial time. Moreover, the Counting versions have zero solutions if and only if <span class="MathJax_Preview"><script type="math/tex">
P\left(x\right)=No
</script>
</span>. Since any multiplicative approximation of zero is still zero, we can use the approximation algorithm for the counting problem to efficiently solve the <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span>-complete decision problem, contradicting that <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}\ne\mathcal{NP}
</script>
</span>.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> <span class="MathJax_Preview"><script type="math/tex">
DNF
</script>
</span> Formulas are Not <i>Always</i> Easy!
</h1>
<div class="Unindented">
During this and the previous post, we mentioned <span class="MathJax_Preview"><script type="math/tex">
DNF
</script>
</span> formulas as something which is easy to solve. We showed that <span class="MathJax_Preview"><script type="math/tex">
DNFSAT\in\mathcal{P}
</script>
</span>, and always compared it to its “difficult” variation - the <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> version. However, a <span class="MathJax_Preview"><script type="math/tex">
DNF
</script>
</span> formula can also be “difficult”. It just depends what we are asking. For example, deciding if a <span class="MathJax_Preview"><script type="math/tex">
DNF
</script>
</span> formula <span class="MathJax_Preview"><script type="math/tex">
\psi
</script>
</span> is a tautology (i.e. evaluated to <span class="MathJax_Preview"><script type="math/tex">
True
</script>
</span> for any assignment) is an <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span>-complete problem. One can show that by noting that <span class="MathJax_Preview"><script type="math/tex">
\phi:=\neg\psi
</script>
</span> is a <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> formula, and <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> is a <span class="MathJax_Preview"><script type="math/tex">
No
</script>
</span> instance for <i>CNF-SAT</i> if and only if <span class="MathJax_Preview"><script type="math/tex">
\psi
</script>
</span> is a <span class="MathJax_Preview"><script type="math/tex">
Yes
</script>
</span> instance for the <i>DNF-TAUTOLOGY</i> problem. Thus, the problem are equally hard.
</div>
<div class="Indented">
Similarly, we can define an “easy” problem for <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> formulas: decide whether a <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> formula <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> is a tautology can be done in polynomial time, by checking if its negation <span class="MathJax_Preview"><script type="math/tex">
\psi:=\neg\phi
</script>
</span> has a s.a. which is polynomial since <span class="MathJax_Preview"><script type="math/tex">
DNFSAT\in\mathcal{P}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\psi
</script>
</span> is a <span class="MathJax_Preview"><script type="math/tex">
DNF
</script>
</span> formula.
</div>
<div class="Indented">
As a conclusion, the hardness of those decision problems depends not only on the form of the formula but also on what are we asking to decide.
</div>
