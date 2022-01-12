---
layout: post
author: Eran Amar
title:  Comparing Chernoff - Hoeffding bounds
date:   2017-03-24
comments: true
tags: probability_theory
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
<a class="toc" name="toc-Section-1">1</a> The Definitions and Motivation
</h1>
<div class="Unindented">
Hoeffding and Chernoff bounds (a.k.a “inequalities”) are very common <i>concentration measures</i> that are being used in many fields in computer science. A concentration measure is a way to bound the probability for the event in which the sum of random variables is “far” from the sum of their means. 
</div>
<div class="Indented">
For example: suppose we have <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> (fair) coins to toss and we want to know what is the probability that the total number of heads (i.e sum of indicators) will be “far” from half of the tosses (which is the sum of their means), say that either <span class="MathJax_Preview"><script type="math/tex">
\#heads>\frac{3}{4}n
</script>
</span> or <span class="MathJax_Preview"><script type="math/tex">
\#heads<\frac{1}{4}n
</script>
</span>. Chernoff’s and Hoeffding’s bounds can be used to <i>bound</i> the probability for that to happen. 
</div>
<div class="Indented">
Both bounds are similar in their settings and their results, and sometimes it is not clear which bound is better for a given setting; maybe even both bounds are equivalent. In this post we will try to get an intuition which bound is stronger (and when). 
</div>
<div class="Indented">
Note that each bounds have several common variations and we will discuss only the ones that are stated below. Let’s start with the first bound, a multiplicative version of Chernoff’s bound.
</div>
<div class="Definition">
Chernoff bound. Let <span class="MathJax_Preview"><script type="math/tex">
\left\{ X_{i}\right\} _{i=1}^{n}
</script>
</span> be a collection of independent random variables ranging in <span class="MathJax_Preview"><script type="math/tex">
\left[0,1\right]
</script>
</span>, denote <span class="MathJax_Preview"><script type="math/tex">
X:=\sum_{i=1}^{n}X_{i}
</script>
</span> and let <span class="MathJax_Preview"><script type="math/tex">
\mu:=\mathbb{E}\left[X\right]=\sum_{i=1}^{n}\mathbb{E}\left[X_{i}\right]
</script>
</span>, then for any <span class="MathJax_Preview"><script type="math/tex">
\epsilon\in\left(0,1\right)
</script>
</span> <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{P}\left[\left|X-\mu\right|>\epsilon\mu\right]\le2\exp\left(-\epsilon^{2}\frac{\mu}{3}\right)

</script>
</span>
</div>
<div class="Unindented">

</div>
<div class="Definition">
Hoeffding bound. Let <span class="MathJax_Preview"><script type="math/tex">
\left\{ X_{i}\right\} _{i=1}^{n}
</script>
</span> be independent random variables ranging in <span class="MathJax_Preview"><script type="math/tex">
\left[a,b\right]
</script>
</span> where <span class="MathJax_Preview"><script type="math/tex">
a<b
</script>
</span>, denote <span class="MathJax_Preview"><script type="math/tex">
X:=\sum_{i=1}^{n}X_{i}
</script>
</span> and let <span class="MathJax_Preview"><script type="math/tex">
\mu:=\mathbb{E}\left[X\right]=\sum_{i=1}^{n}\mathbb{E}\left[X_{i}\right]
</script>
</span>, then for any <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>: <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{P}\left[\left|X-\mu\right|>t\right]\le2\exp\left(\frac{-t^{2}}{n\left(b-a\right)^{2}}\right)

</script>
</span>
</div>
<div class="Unindented">

</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Writing Hoeffding’s bound as a Chernoff’s bound 
</h1>
<div class="Unindented">
In first glance it seems that we cannot compare between the definitions, mostly because this small technical issue: Hoeffding’s bound allows the random variables to be in any close interval <span class="MathJax_Preview"><script type="math/tex">
\left[a,b\right]
</script>
</span> rather than <span class="MathJax_Preview"><script type="math/tex">
\left[0,1\right]
</script>
</span>. The solution for that is to scale and shift the variables to make them takes values in <span class="MathJax_Preview"><script type="math/tex">
\left[0,1\right]
</script>
</span>. We will start with exactly that, and then transform Hoeffding’s inequality into “Chernoff’s”.
</div>
<div class="Indented">
So for any <span class="MathJax_Preview"><script type="math/tex">
i\in\left[n\right]
</script>
</span>, denote <span class="MathJax_Preview"><script type="math/tex">
Y_{i}=\frac{X_{i}}{b-a}-a
</script>
</span> and let <span class="MathJax_Preview"><script type="math/tex">
Y=\sum_{i=1}^{n}Y_{i}=\frac{X}{b-a}-na
</script>
</span>. Observe that <span class="MathJax_Preview"><script type="math/tex">
\mathbb{E}\left[Y\right]=\frac{\mu}{b-a}-na
</script>
</span>. Clearly, now the range of the variables <span class="MathJax_Preview"><script type="math/tex">
\left\{ Y_{i}\right\} _{i=1}^{n}
</script>
</span> is <span class="MathJax_Preview"><script type="math/tex">
\left[0,1\right]
</script>
</span> so we can write Hoeffding’s bound in terms of <span class="MathJax_Preview"><script type="math/tex">
Y_{i}
</script>
</span>’s
</div>
<div class="Indented">
<span class="MathJax_Preview">
<script type="math/tex;mode=display">
\begin{aligned}
\mathbf{P}\left[\left|X-\mu\right|>t\right] & =\mathbf{P}\left[\left|\frac{X}{b-a}-na-\frac{\mu}{b-a}+na\right|>\frac{t}{b-a}\right]\\
 & =\mathbf{P}\left[\left|Y-\mathbb{E}\left[Y\right]\right|>\frac{t}{b-a}\cdot\frac{\mathbb{E}\left[Y\right]}{\mathbb{E}\left[Y\right]}\right]\\
 & \le2\exp\left(\frac{-t^{2}}{\left(b-a\right)^{2}\mathbb{E}\left[Y\right]^{2}}\cdot\frac{\mathbb{E}\left[Y\right]}{3}\right)\\
 & =2\exp\left(\frac{-t^{2}}{3\left(b-a\right)^{2}}\left(\frac{\mu}{b-a}-na\right)^{-1}\right)\\
 & =2\exp\left(\frac{-t^{2}}{3\mu\left(b-a\right)-3a\cdot n\left(b-a\right)^{2}}\right)
\end{aligned}
</script>
</span>
</div>
<div class="Indented">
Where the inequality in the middle of the process is where we apply Chernoff’s bound with <span class="MathJax_Preview"><script type="math/tex">
\epsilon=\frac{t}{\left(b-a\right)\mathbb{E}\left[Y\right]}
</script>
</span>, note that there is an implicit assumption here that <span class="MathJax_Preview"><script type="math/tex">
\epsilon<1
</script>
</span>. 
</div>
<div class="Indented">
So we managed to formulate Hoeffding’s bound in Chernoff’s settings, ending with a very messy formula. In order to explore the behavior of the bounds further, we will simplify the analysis by adding another assumption: that <span class="MathJax_Preview"><script type="math/tex">
a=0
</script>
</span>.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> Which bound to used?
</h1>
<div class="Unindented">
Now we assuming that <span class="MathJax_Preview"><script type="math/tex">
a=0
</script>
</span>. The restriction from previous section that <span class="MathJax_Preview"><script type="math/tex">
\epsilon<1
</script>
</span> is now holds whenever <span class="MathJax_Preview"><script type="math/tex">
t<\mu
</script>
</span>, which make sense because Chernoff’s bound, as we defined it, only concerned with deviations that are up to <span class="MathJax_Preview"><script type="math/tex">
\mu
</script>
</span>. 
</div>
<div class="Indented">
Next, the term we got for Hoeffding is simplified further into <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{P}\left[\left|X-\mu\right|>t\right]\le2\exp\left(\frac{-t^{2}}{3\mu b}\right)

</script>
</span>
Now easy to compare between the bounds. With Hoeffding we can achieve bound of <span class="MathJax_Preview"><script type="math/tex">
2\exp\left(\frac{-t^{2}}{nb^{2}}\right)
</script>
</span> and with Chernoff <span class="MathJax_Preview"><script type="math/tex">
2\exp\left(\frac{-t^{2}}{3\mu b}\right)
</script>
</span>, it just left to compare between <span class="MathJax_Preview"><script type="math/tex">
nb
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
3\mu
</script>
</span>. If <span class="MathJax_Preview"><script type="math/tex">
nb<3\mu
</script>
</span> then Chernoff is stronger and vise versa. We conclude that none of the bounds is always preferred upon the other (which is not so surprising), and the answer to the question “which one to use” depends on the parameters of the distribution in hand.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> Chernoff bound is tight!
</h1>
<div class="Unindented">
One surprising fact about Chernoff’s bound is that in some cases it is tight (i.e. gives a lower bound). The theorem below is taken from these <a class="URL" href="https://ece.uwaterloo.ca/~nmousavi/Papers/Chernoff-Tightness.pdf">notes</a> (see the link for the full proof).
</div>
<div class="Theorem">
Let <span class="MathJax_Preview"><script type="math/tex">
\left\{ X_{i}\right\} _{i=1}^{n}
</script>
</span> be i.i.d random variables ranging in <span class="MathJax_Preview"><script type="math/tex">
\left\{ 0,1\right\} 
</script>
</span> with <span class="MathJax_Preview"><script type="math/tex">
\mathbf{P}\left[X_{i}=1\right]=p
</script>
</span>, denote <span class="MathJax_Preview"><script type="math/tex">
X:=\sum_{i=1}^{n}X_{i}
</script>
</span> and let <span class="MathJax_Preview"><script type="math/tex">
\mu:=\mathbb{E}\left[X\right]=np
</script>
</span>. <br>
If <span class="MathJax_Preview"><script type="math/tex">
p\le\frac{1}{4}
</script>
</span>, then for any <span class="MathJax_Preview"><script type="math/tex">
t>0
</script>
</span> <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{P}\left[X-\mu>t\right]\ge\frac{1}{4}\exp\left(-t^{2}\frac{2}{\mu}\right)

</script>
</span>
If <span class="MathJax_Preview"><script type="math/tex">
p<\frac{1}{2}
</script>
</span>, then for any <span class="MathJax_Preview"><script type="math/tex">
t\in\left[0,n\left(1-2p\right)\right]
</script>
</span><span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{P}\left[X-\mu>t\right]\ge\frac{1}{4}\exp\left(-t^{2}\frac{2}{\mu}\right)

</script>
</span>
</div>
<div class="Unindented">

</div>
<div class="Indented">
Note the differences between that regime and the one from the definition at the beginning of the post: in this theorem the variables are i.i.d (rather than only independent) and they are discrete.
</div>
