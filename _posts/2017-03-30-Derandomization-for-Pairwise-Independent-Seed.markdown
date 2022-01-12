---
layout: post
author: Eran Amar
title:  Derandomization for Pairwise Independent Seed
date:   2017-03-30
comments: true
tags: probability_theory algorithms
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
<a class="toc" name="toc-Section-1">1</a> Randomized Algorithm Settings
</h1>
<div class="Unindented">
Randomized algorithm is an algorithm that in addition to its inputs also uses internally <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> coin-tosses (i.e. uniformly random bits) in order to decide on its output, so different invocations of the algorithm with same inputs may yield different outputs that depend on the results of these coins. One can think about the internal coin-tosses as a binary string <span class="MathJax_Preview"><script type="math/tex">
r
</script>
</span> of length <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> that is being fixed before the algorithm starts and being consumed sequentially during the algorithm’s execution. Such string is often called <i>seed</i>, and the randomness within the algorithm now comes from choosing a different string at the beginning of each execution. 
</div>
<div class="Indented">
Note that tossing <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> fair coins, is exactly like drawing uniformly at random a string <span class="MathJax_Preview"><script type="math/tex">
r
</script>
</span> from the set <span class="MathJax_Preview"><script type="math/tex">
\left\{ 0,1\right\} ^{t}
</script>
</span> (i.e. each string has a probability of <span class="MathJax_Preview"><script type="math/tex">
2^{-t}
</script>
</span>).
</div>
<div class="Indented">
Suppose we have a randomized algorithm <span class="MathJax_Preview"><script type="math/tex">
A
</script>
</span> that solves a problem of interest. We have a guarantee that for any input <span class="MathJax_Preview"><script type="math/tex">
x
</script>
</span> of length <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span>, algorithm <span class="MathJax_Preview"><script type="math/tex">
A
</script>
</span> runs in <span class="MathJax_Preview"><script type="math/tex">
poly\left(n\right)
</script>
</span>, and outputs either a valid solution <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> if it succeeds or output <span class="MathJax_Preview"><script type="math/tex">
failed
</script>
</span>. The success probability of the algorithm is <span class="MathJax_Preview"><script type="math/tex">
p
</script>
</span> (which may be even infinitesimally small). 
</div>
<div class="Indented">
The first question to ask is, <i>can we convert the randomized algorithm into a deterministic one?</i>
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Brute Force Derandomization
</h1>
<div class="Unindented">
The answer is<i> yes</i>, and there is a simple way to do that: Upon receiving an input <span class="MathJax_Preview"><script type="math/tex">
x
</script>
</span>, iterate over all possible strings <span class="MathJax_Preview"><script type="math/tex">
r\in\left\{ 0,1\right\} ^{t}
</script>
</span> and run <span class="MathJax_Preview"><script type="math/tex">
A_{r}\left(x\right)
</script>
</span>. That is, simulate <span class="MathJax_Preview"><script type="math/tex">
A
</script>
</span> on <span class="MathJax_Preview"><script type="math/tex">
x
</script>
</span> with <span class="MathJax_Preview"><script type="math/tex">
r
</script>
</span> as the random seed. Return the first solution that <span class="MathJax_Preview"><script type="math/tex">
A
</script>
</span> outputs (i.e. anything that is not <span class="MathJax_Preview"><script type="math/tex">
failed
</script>
</span>).
</div>
<div class="Indented">
The idea behind this method is that if there is a non-zero probability for <span class="MathJax_Preview"><script type="math/tex">
A
</script>
</span> to output a valid solution for the input <span class="MathJax_Preview"><script type="math/tex">
x
</script>
</span>, then there must be a random seed that leads to that output when executing algorithm <span class="MathJax_Preview"><script type="math/tex">
A
</script>
</span> on <span class="MathJax_Preview"><script type="math/tex">
x
</script>
</span>. So all we need to do is iterating over all possible seeds of length <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>, and that leads to a deterministic algorithm. 
</div>
<div class="Indented">
You probably already see the problem here. If <span class="MathJax_Preview"><script type="math/tex">
t=\mathcal{O}\left(n\right)
</script>
</span> then the running time of the deterministic procedure is <span class="MathJax_Preview"><script type="math/tex">
2^{\mathcal{O}\left(n\right)}\cdot poly\left(n\right)
</script>
</span>.
</div>
<div class="Indented">
That is, of course, terrible running time.
</div>
<div class="Indented">
On the other hand, if <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> is a constant or even up to <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(\log n\right)
</script>
</span>, then the resulting running time will stay polynomial, and everyone is happy. In the next section we will see that with an additional assumption, we can “decrease” the seed length from <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
\log n
</script>
</span> bits.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> Pairwise Independent Seed
</h1>
<div class="Unindented">
We start off with a definition.
</div>
<div class="Definition">
<i>k</i>-wise Independent Family. Let <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}
</script>
</span> be a set of random variables. We say that <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}
</script>
</span> is a <i>k</i>-wise independent family if for any subset of <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span> variables <span class="MathJax_Preview"><script type="math/tex">
X_{1},..,X_{k}\in\mathcal{X}
</script>
</span>, and values <span class="MathJax_Preview"><script type="math/tex">
y_{1},...,y_{k}
</script>
</span> <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{P}\left[\bigwedge_{i\in I}X_{i}=y_{i}\right]=\prod_{i\in I}\mathbf{P}\left[X_{i}=y_{i}\right]\qquad\forall I\subseteq\left[k\right]

</script>
</span>
that is, any subset of <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span> random variables from <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}
</script>
</span> is fully independent. The family <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}
</script>
</span> is called pairwise independent family when <span class="MathJax_Preview"><script type="math/tex">
k=2
</script>
</span>.
</div>
<div class="Unindented">

</div>
<div class="Indented">
What this definition have with our settings? Observe that up until now we implicitly assumed that the algorithm needs its coins to be fully independent. That is, if we think of <span class="MathJax_Preview"><script type="math/tex">
s
</script>
</span> as a series of Bernoulli RVs then we actually require <span class="MathJax_Preview"><script type="math/tex">
s
</script>
</span> to be <i>t</i>-wise independent family, which is a much stronger property compare to the pairwise independence property. If we relax that requirement and assume that the algorithm only needs its coins to be pairwise independent, then there is a way (described in next section) to construct <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> <i>pairwise independent</i> bits using only <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(\log n\right)
</script>
</span> <i>fully independent</i> bits. That construction can be “embedded” into our algorithm such that we ultimately end up with a procedure that only requires <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(\log n\right)
</script>
</span> fully independent seed. Applying the derandomization method from previous section will result with a deterministic algorithm that will still run in <span class="MathJax_Preview"><script type="math/tex">
poly\left(n\right)
</script>
</span> time.
</div>
<div class="Indented">
Of course, the aforementioned derandomization method is valid only if the assumption of pairwise independent seed doesn’t break the correctness analysis of the randomized algorithm.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> Stretching the Seed
</h1>
<div class="Unindented">
Suppose we have <span class="MathJax_Preview"><script type="math/tex">
\approx\log n
</script>
</span> fair fully independent coins, we want to construct a method to generate from them <span class="MathJax_Preview"><script type="math/tex">
\approx n
</script>
</span> fair pairwise independent bits. The following claim shows such construction.
</div>
<div class="Claim">
Suppose we have <span class="MathJax_Preview"><script type="math/tex">
t:=\log\left(n\right)+1
</script>
</span> fully independent Bernoulli random variables, each with probability <span class="MathJax_Preview"><script type="math/tex">
\frac{1}{2}
</script>
</span>. Fix them in some arbitrary order to get a (random) vector <span class="MathJax_Preview"><script type="math/tex">
\mathbf{s}
</script>
</span> of length <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>. Let <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}=\left\{ \left\langle \mathbf{s},\mathbf{x}\right\rangle _{mod2}\mid\mathbf{x}\in\left\{ 0,1\right\} ^{t},\:\:x\ne\mathbf{0}\right\} 
</script>
</span> be the set of all possible sums modulo 2 of those RVs and note that <span class="MathJax_Preview"><script type="math/tex">
\left|\mathcal{S}\right|=2^{t}-1=\Theta\left(n\right)
</script>
</span>, then <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}
</script>
</span> is a family of pairwise independent bits.
</div>
<div class="Unindented">
To prove that we start with a very basic lemma.
</div>
<div class="Lemma">
Let <span class="MathJax_Preview"><script type="math/tex">
X,Y\sim Bernoulli\left(0.5\right)
</script>
</span> be two independent random variables, let <span class="MathJax_Preview"><script type="math/tex">
Z=X+Y
</script>
</span> then <span class="MathJax_Preview"><script type="math/tex">
Z\sim Bernoulli\left(0.5\right)
</script>
</span>. Moreover, <span class="MathJax_Preview"><script type="math/tex">
Z
</script>
</span> is independent from <span class="MathJax_Preview"><script type="math/tex">
X
</script>
</span> and from <span class="MathJax_Preview"><script type="math/tex">
Y
</script>
</span>.
</div>
<div class="Proof">
For any <span class="MathJax_Preview"><script type="math/tex">
b\in\left\{ 0,1\right\} 
</script>
</span><span class="MathJax_Preview">
<script type="math/tex;mode=display">
\begin{aligned}
\mathbf{P}\left[X+Y=b\right] & =\mathbf{P}\left[X=1,Y=1-b\right]+\mathbf{P}\left[X=0,Y=b\right]\\
 & =\mathbf{P}\left[X=1\right]\mathbf{P}\left[Y=1-b\right]+\mathbf{P}\left[X=0\right]\mathbf{P}\left[Y=b\right]\\
 & =\frac{1}{2}\left(\mathbf{P}\left[Y=1-b\right]+\mathbf{P}\left[Y=1-b\right]\right)\\
 & =\frac{1}{2}
\end{aligned}
</script>
</span>
thus <span class="MathJax_Preview"><script type="math/tex">
Z\sim Bernoulli\left(0.5\right)
</script>
</span>. Next w.l.o.g we prove that <span class="MathJax_Preview"><script type="math/tex">
Z
</script>
</span> independent from <span class="MathJax_Preview"><script type="math/tex">
X
</script>
</span>. Take any <span class="MathJax_Preview"><script type="math/tex">
b_{1},b_{2}\in\left\{ 0,1\right\} 
</script>
</span>. In case <span class="MathJax_Preview"><script type="math/tex">
b_{1}=b_{2}
</script>
</span> then <span class="MathJax_Preview">
<script type="math/tex;mode=display">
\begin{aligned}
\mathbf{P}\left[X=b_{1},Z=b_{2}\right] & =\mathbf{P}\left[X=b_{1},X+Y=b_{1}\right]\\
 & =\mathbf{P}\left[X=b_{1},Y=0\right]\\
 & =\mathbf{P}\left[X=b_{1}\right]\mathbf{P}\left[Y=0\right]\\
 & =\mathbf{P}\left[X=b_{1}\right]\cdot\frac{1}{2}\\
 & =\mathbf{P}\left[X=b_{1}\right]\mathbf{P}\left[Z=b_{1}\right]
\end{aligned}
</script>
</span>
so <span class="MathJax_Preview"><script type="math/tex">
X
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
Z
</script>
</span> are independent. And in case that <span class="MathJax_Preview"><script type="math/tex">
b_{2}=1-b_{1}
</script>
</span> then similarly we get<span class="MathJax_Preview">
<script type="math/tex;mode=display">
\begin{aligned}
\mathbf{P}\left[X=b_{1},Z=b_{2}\right] & =\mathbf{P}\left[X=b_{1},X+Y=1-b_{1}\right]\\
 & =\mathbf{P}\left[X=b_{1},Y=1\right]\\
 & =\mathbf{P}\left[X=b_{1}\right]\mathbf{P}\left[Z=b_{1}\right]
\end{aligned}
</script>
</span>
which conclude the proof.
</div>
<div class="Unindented">
Now we prove the claim from above.
</div>
<div class="Proof">
Take two different variables <span class="MathJax_Preview"><script type="math/tex">
X_{1}=\left\langle \mathbf{s},\mathbf{x}_{1}\right\rangle \in\mathcal{S}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
X_{2}=\left\langle \mathbf{s},\mathbf{x}_{2}\right\rangle \in\mathcal{S}
</script>
</span>, and let <span class="MathJax_Preview"><script type="math/tex">
b_{1},b_{2}\in\left\{ 0,1\right\} 
</script>
</span>, we want to show that <span class="MathJax_Preview"><script type="math/tex">
X_{1}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
X_{2}
</script>
</span> are independent. Obviously, we only care about the set of indices which are non-zero in the vectors <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}_{1}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}_{2}
</script>
</span>. Let these sets be <span class="MathJax_Preview"><script type="math/tex">
I_{1},I_{2}
</script>
</span> respectively. Denote <span class="MathJax_Preview"><script type="math/tex">
M_{0}=\sum_{i\in I_{1}\cap I_{2}}\mathbf{s}_{i}
</script>
</span>, <span class="MathJax_Preview"><script type="math/tex">
M_{1}=\sum_{i\in I_{1}\backslash I_{2}}\mathbf{s}_{i}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
M_{2}=\sum_{i\in I_{2}\backslash I_{1}}\mathbf{s}_{i}
</script>
</span> where summation over empty set is defined as the constant <span class="MathJax_Preview"><script type="math/tex">
0
</script>
</span> (all the operations in the proof are modulo 2). Note that any pair of variables from <span class="MathJax_Preview"><script type="math/tex">
\left\{ M_{0},M_{1},M_{2}\right\} 
</script>
</span> are independent because they defined on mutually disjoint set of indices. Moreover, by the lemma from above, each of them if it is not the constant zero then is distributed as <span class="MathJax_Preview"><script type="math/tex">
Bernoulli\left(0.5\right)
</script>
</span>. When plugging-in the new notations we get<span class="MathJax_Preview">
<script type="math/tex;mode=display">
\begin{aligned}
\mathbf{P}\left[X_{1}=b_{1},\:X_{2}=b_{2}\right] & =\mathbf{P}\left[M_{0}+M_{1}=b_{1},\:M_{0}+M_{2}=b_{2}\right]
\end{aligned}
</script>
</span>
Because <span class="MathJax_Preview"><script type="math/tex">
I_{1}\ne I_{2}
</script>
</span> there must be at most one variable from <span class="MathJax_Preview"><script type="math/tex">
\left\{ M_{0},M_{1},M_{2}\right\} 
</script>
</span> which may be constant. Therefore, no matter which <span class="MathJax_Preview"><script type="math/tex">
M_{i}
</script>
</span> it is (if any), either <span class="MathJax_Preview"><script type="math/tex">
X_{1}
</script>
</span> or <span class="MathJax_Preview"><script type="math/tex">
X_{2}
</script>
</span> is a sum of two non-constant variables. W.l.o.g let it be <span class="MathJax_Preview"><script type="math/tex">
X_{1}
</script>
</span>, then it has the <span class="MathJax_Preview"><script type="math/tex">
M_{1}
</script>
</span> component which does not appear in <span class="MathJax_Preview"><script type="math/tex">
X_{2}
</script>
</span> and can flip the result of the sum with probability <span class="MathJax_Preview"><script type="math/tex">
0.5
</script>
</span> <b>regardless</b> of what happen anywhere else, therefore <span class="MathJax_Preview"><script type="math/tex">
X_{1}
</script>
</span> is independent of <span class="MathJax_Preview"><script type="math/tex">
X_{2}
</script>
</span>, and that completes the proof.
</div>
<div class="Unindented">

</div>
<div class="Indented">
Last one remark. In our settings the randomized algorithm either return a solution or says “failed”, if we have a polynomial procedure to validate a solution we can generalize the settings to the case when the algorithm always output a solution, and then we verify it to determine if it is “succeeds” case of “failed” case.
</div>
