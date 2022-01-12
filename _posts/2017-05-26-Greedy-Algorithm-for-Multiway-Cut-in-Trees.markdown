---
layout: post
author: Eran Amar
title:  Greedy Algorithm for Multiway-Cut in Trees
date:   2017-05-26
comments: true
tags: algorithms graphs
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
<a class="toc" name="toc-Section-1">1</a> The Multiway Cut Problem
</h1>
<div class="Unindented">
The <i>Multiway Cut</i> (MC) problem is a generalization of the known<i> Min-Cut</i> problem. The input is a triple <span class="MathJax_Preview"><script type="math/tex">
\left(G,T,w\right)
</script>
</span> where <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is an undirected graph with a set <span class="MathJax_Preview"><script type="math/tex">
V\left(G\right)
</script>
</span> of <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> vertices and a set <span class="MathJax_Preview"><script type="math/tex">
E\left(G\right)
</script>
</span> of <span class="MathJax_Preview"><script type="math/tex">
m
</script>
</span> edges, <span class="MathJax_Preview"><script type="math/tex">
T\subset V\left(G\right)
</script>
</span> is the set of <i>terminals</i>, and <span class="MathJax_Preview"><script type="math/tex">
w:E\left(G\right)\to\mathbb{R}_{>0}
</script>
</span> is a function that assign positive weight to each edge in the graph. A valid <i>cut</i> <span class="MathJax_Preview"><script type="math/tex">
C\subseteq E\left(G\right)
</script>
</span> is a set of edges such that any two vertices <span class="MathJax_Preview"><script type="math/tex">
t_{1},t_{2}\in T
</script>
</span> are disconnected over the sub-graph <span class="MathJax_Preview"><script type="math/tex">
H:=\left(V\left(G\right),E\left(G\right)\backslash C\right)
</script>
</span>. The <i>weight</i> of a given cut <span class="MathJax_Preview"><script type="math/tex">
C
</script>
</span> is the sum of wights of its edges, that is, <span class="MathJax_Preview"><script type="math/tex">
w\left(C\right)=\sum_{e\in C}w\left(e\right)
</script>
</span>. A solution for the <i>MC</i> problem is a valid cut of minimum weight.
</div>
<div class="Indented">
Let <span class="MathJax_Preview"><script type="math/tex">
k=\left|T\right|
</script>
</span>. Note that when <span class="MathJax_Preview"><script type="math/tex">
k=2
</script>
</span> the problem is reduced to the regular <i>Min-Cut</i> problem. For general graphs, any <span class="MathJax_Preview"><script type="math/tex">
k>2
</script>
</span> makes the problem NP-Hard. However, there are efficient algorithms for <i>Multiway-Cut</i> in <b>trees</b>. A tree is a connected acyclic graph. It is easy to see that if our graph is a tree, any two vertices in it must be connected by exactly one path. Moreover, <span class="MathJax_Preview"><script type="math/tex">
m=n-1
</script>
</span>. We will use these properties later when we prove that the algorithm gives an optimal solution.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Efficient Algorithm for <i>MC</i> in Trees
</h1>
<div class="Unindented">
Let <span class="MathJax_Preview"><script type="math/tex">
\left(G,T,w\right)
</script>
</span> be the input to the algorithm, and <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is a tree. To simplify the analysis later on, we will work slightly differently; instead of trying to find a small-weight set <span class="MathJax_Preview"><script type="math/tex">
C\subseteq E\left(G\right)
</script>
</span> <i>to discard</i> from the graph, we will try to find a large-weight set <span class="MathJax_Preview"><script type="math/tex">
S\subseteq E\left(G\right)
</script>
</span> <i>to</i> <i>keep</i> in the graph. Note that obviously <span class="MathJax_Preview"><script type="math/tex">
S=V\left(G\right)\backslash C
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
C=V\left(G\right)\backslash S
</script>
</span>. Therefore, finding <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> with maximum weight is equivalent to finding <span class="MathJax_Preview"><script type="math/tex">
C
</script>
</span> with minimum weight.
</div>
<div class="Indented">
The idea is to start with a trivial (and probably not optimal) solution <span class="MathJax_Preview"><script type="math/tex">
S=\emptyset
</script>
</span>, that is, we don’t keep any edge in the graph such that all the terminals are separated, and we will improve <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> iteratively. 
</div>
<div class="Indented">
We start off with some definitions: 
</div>
<div class="Indented">
A <i>connected component</i> (CC) of a vertex <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> over a graph <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is the set of all vertices that can be reached from <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span>. Note that if <span class="MathJax_Preview"><script type="math/tex">
\left|V\left(G\right)\right|=n
</script>
</span> then there are at most <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> CCs in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> (each vertex has its own CC, which is the case when the graph has no edges). 
</div>
<div class="Indented">
A vertex <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> inside some CC is a <i>master node</i> if it is a terminal vertex. We then say that all the other vertices in that CC are <i>dominated</i> by <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>.
</div>
<div class="Indented">
Now it is time to describe the algorithm:
</div>
<ol>
<li>
Start by sorting the <span class="MathJax_Preview"><script type="math/tex">
n-1
</script>
</span> edges of <span class="MathJax_Preview"><script type="math/tex">
E
</script>
</span> in descending order according to their weights. Let <span class="MathJax_Preview"><script type="math/tex">
e_{1},e_{2},..,e_{n-1}
</script>
</span> be the ordered sequence such that <span class="MathJax_Preview"><script type="math/tex">
\forall i,\:w\left(e_{i}\right)>w\left(e_{i+1}\right)
</script>
</span>. 
</li>
<li>
Initialize a trivial solution <span class="MathJax_Preview"><script type="math/tex">
S_{0}=\emptyset
</script>
</span>. Maintain a set <span class="MathJax_Preview"><script type="math/tex">
R_{0}=\left\{ \left\{ v\right\} \mid v\in V\left(G\right)\right\} 
</script>
</span> of all the CCs in the resulting graph (each vertex is its own CC).
</li>
<li>
For each <span class="MathJax_Preview"><script type="math/tex">
i=1,...,n-1
</script>
</span> do the following: <ol>
<li>
Consider the endpoints of the edge <span class="MathJax_Preview"><script type="math/tex">
e_{i}=\left\{ v_{1},v_{2}\right\} 
</script>
</span> and let <span class="MathJax_Preview"><script type="math/tex">
CC_{1},CC_{2}
</script>
</span> be the connected components of <span class="MathJax_Preview"><script type="math/tex">
v_{1},v_{2}
</script>
</span> respectively. If at least one of them do not has a master node, update <span class="MathJax_Preview"><script type="math/tex">
S_{i}=S_{i-1}\cup\left\{ e_{i}\right\} 
</script>
</span> and let <span class="MathJax_Preview"><script type="math/tex">
R_{i}
</script>
</span> be the same as <span class="MathJax_Preview"><script type="math/tex">
R_{i-1}
</script>
</span> but with <span class="MathJax_Preview"><script type="math/tex">
CC_{1},CC_{2}
</script>
</span> merged into a new single connected component. Otherwise <span class="MathJax_Preview"><script type="math/tex">
S_{i}=S_{i-1}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
R_{i}=R_{i-1}
</script>
</span>. 
</li>
</ol>
</li>
<li>
Return the set <span class="MathJax_Preview"><script type="math/tex">
S_{n-1}
</script>
</span>.
</li>
</ol>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> The Solution is Correct and Optimal
</h1>
<div class="Unindented">
It is easy to see that <span class="MathJax_Preview"><script type="math/tex">
S_{n-1}
</script>
</span> is a valid solution; we started with all the terminals separated in their own CC, and in each iteration we never connect two CCs that have different masters (i.e. terminals). Therefore, in the returned set, all the connected components has at most one master, meaning that the terminals are separated.
</div>
<div class="Indented">
The fact that <span class="MathJax_Preview"><script type="math/tex">
S_{n-1}
</script>
</span> has maximum weight follows by the two following claims:
</div>
<ol>
<li>
Any optimal solution <span class="MathJax_Preview"><script type="math/tex">
S^{*}\subseteq E\left(G\right)
</script>
</span> has <span class="MathJax_Preview"><script type="math/tex">
\left|S^{*}\right|=n-k
</script>
</span>. Moreover, <span class="MathJax_Preview"><script type="math/tex">
\left|S_{n-1}\right|=n-k
</script>
</span>.
</li>
<li>
For any iteration <span class="MathJax_Preview"><script type="math/tex">
i
</script>
</span> of the algorithm, there is an (possibly different) optimal solution <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span> such that <span class="MathJax_Preview"><script type="math/tex">
S_{i}\subseteq S^{*}
</script>
</span>.
</li>
</ol>
<div class="Unindented">
So we conclude that there exists some optimal <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span> such that <span class="MathJax_Preview"><script type="math/tex">
S_{n-1}=S^{*}
</script>
</span>, thus <span class="MathJax_Preview"><script type="math/tex">
S_{n-1}
</script>
</span> has maximum weight.
</div>
<div class="Indented">
We move to state and prove those claims formally.
</div>
<div class="Claim">
Let <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}
</script>
</span> be the set of all maximum-wight solutions for <i>Multiway-Cut</i> for the instance <span class="MathJax_Preview"><script type="math/tex">
\left(G,T,w\right)
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is a tree. Then any <span class="MathJax_Preview"><script type="math/tex">
S^{*}\in\mathcal{X}
</script>
</span> have <span class="MathJax_Preview"><script type="math/tex">
\left|S^{*}\right|=n-k
</script>
</span>. Moreover, <span class="MathJax_Preview"><script type="math/tex">
\left|S_{n-1}\right|=n-k
</script>
</span>.
</div>
<div class="Proof">
Since we started from a graph that is a tree, the number of CCs in the final sub-graph is exactly <span class="MathJax_Preview"><script type="math/tex">
n-\left|S\right|
</script>
</span>, for any <span class="MathJax_Preview"><script type="math/tex">
S\subseteq E\left(G\right)
</script>
</span>. Let <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span> be some optimal solution. It left to show that the sub-graph <span class="MathJax_Preview"><script type="math/tex">
H^{*}=\left(V\left(G\right),S^{*}\right)
</script>
</span> has exactly <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span> connected components:<br>
<br>
Assume that there are less than <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span>, then we must have two terminals which live in the same CC - invalidating the solution <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span>. If we assume that there are more than <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span> CCs, then we must have a connected component without a master, so we can safely connect it to some other CC with some edge <span class="MathJax_Preview"><script type="math/tex">
e
</script>
</span> that is not in <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span> (and such edge exists since <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is connected). That way, we get a new valid solution with greater weight than the weight of <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span> which is a contradiction.<br>
<br>
Similarly, we prove that <span class="MathJax_Preview"><script type="math/tex">
S_{n-1}
</script>
</span> has <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span> CCs: If it has less than <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span>, there must be two terminals in the same CCs, and that contradicts the correctness of <span class="MathJax_Preview"><script type="math/tex">
S_{n-1}
</script>
</span>. If it has more than <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span> CCs after <span class="MathJax_Preview"><script type="math/tex">
n-1
</script>
</span> iterations, there must be a CC without a master, so we may connect it with some edge, let it be <span class="MathJax_Preview"><script type="math/tex">
e_{i}
</script>
</span>, to other CC without invalidating the solution. But that <span class="MathJax_Preview"><script type="math/tex">
e_{i}
</script>
</span> wasn’t picked when the algorithm considered it - meaning that it could cause a violation in the sub-graph of the <span class="MathJax_Preview"><script type="math/tex">
i
</script>
</span>th iteration, and since that sub-graph is contained in the sub-graph of the last iteration then we can’t add <span class="MathJax_Preview"><script type="math/tex">
e_{i}
</script>
</span> to our solution - a contradiction.
</div>
<div class="Unindented">
Note that since any optimal solution have exactly <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span> CCs, we have that each CC must contains one terminal. So in the final sub-graph defined by the solution, any <span class="MathJax_Preview"><script type="math/tex">
u\in V\left(G\right)
</script>
</span> is dominated by exactly one master mode <span class="MathJax_Preview"><script type="math/tex">
t\in T
</script>
</span>.
</div>
<div class="Claim">
Let <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}
</script>
</span> be the set of all optimal solutions for the instance <span class="MathJax_Preview"><script type="math/tex">
\left(G,T,w\right)
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> in a tree. For any <span class="MathJax_Preview"><script type="math/tex">
i\in\left\{ 0,1,..,n-1\right\} 
</script>
</span> let <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}_{i}=\left\{ S^{*}\in\mathcal{X}\mid S_{i}\subset S^{*}\right\} 
</script>
</span> be the set of optimal solutions such that <span class="MathJax_Preview"><script type="math/tex">
S_{i}
</script>
</span> contained in them. Then, for any <span class="MathJax_Preview"><script type="math/tex">
i
</script>
</span>, <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}_{i}\ne\emptyset
</script>
</span>.
</div>
<div class="Proof">
For <span class="MathJax_Preview"><script type="math/tex">
i=0
</script>
</span> we have <span class="MathJax_Preview"><script type="math/tex">
S_{0}=\emptyset
</script>
</span> thus <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}_{0}=\mathcal{X}
</script>
</span> which is not empty. Assume toward contradiction that the claim is false, and let <span class="MathJax_Preview"><script type="math/tex">
i
</script>
</span> be the smallest such that <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}_{i}=\emptyset
</script>
</span>, so we know that <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}_{i-1}
</script>
</span> is not empty, and hence <span class="MathJax_Preview"><script type="math/tex">
S_{i-1}\ne S_{i}
</script>
</span>. In other words, during the <span class="MathJax_Preview"><script type="math/tex">
i
</script>
</span>th iteration, the algorithm added the edge <span class="MathJax_Preview"><script type="math/tex">
e_{i}=\left\{ v,u\right\} 
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
S_{i-1}
</script>
</span>, causing <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}_{i}
</script>
</span> to be empty.<br>
<br>
Fix some <span class="MathJax_Preview"><script type="math/tex">
S^{*}\in\mathcal{X}_{i-1}
</script>
</span>. Denote <span class="MathJax_Preview"><script type="math/tex">
H_{i-1}=\left(V\left(G\right),S_{i-1}\right)
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
H^{*}=\left(V\left(G\right),S^{*}\right)
</script>
</span> the subgraphs of <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span>. From the update rule of the algorithm, at least one of <span class="MathJax_Preview"><script type="math/tex">
\left\{ u,v\right\} 
</script>
</span> is not dominated by a master over <span class="MathJax_Preview"><script type="math/tex">
H_{i-1}
</script>
</span>, w.l.o.g assume it is <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span>. Let <span class="MathJax_Preview"><script type="math/tex">
r\in T
</script>
</span> be the master that dominates <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> over <span class="MathJax_Preview"><script type="math/tex">
H^{*}
</script>
</span>, and consider the path connecting <span class="MathJax_Preview"><script type="math/tex">
r\rightsquigarrow u
</script>
</span>, note that any edge along that path belongs to <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span>. Since the path does not exists in <span class="MathJax_Preview"><script type="math/tex">
H_{n-1}
</script>
</span> and in particular in <span class="MathJax_Preview"><script type="math/tex">
H_{i-1}
</script>
</span>, there is at least one edge that is part of the path and not in <span class="MathJax_Preview"><script type="math/tex">
S_{i-1}
</script>
</span>, let <span class="MathJax_Preview"><script type="math/tex">
e_{j}
</script>
</span> be such one with minimum weight.<br>
<br>
Observe that it is impossible that <span class="MathJax_Preview"><script type="math/tex">
w\left(e_{j}\right)>w\left(e_{i}\right)
</script>
</span> because it means that the algorithm already rejected <span class="MathJax_Preview"><script type="math/tex">
e_{j}
</script>
</span> on the <span class="MathJax_Preview"><script type="math/tex">
j
</script>
</span>th iteration and since it was a violation for <span class="MathJax_Preview"><script type="math/tex">
H_{j}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
H_{j}\subset H_{i-1}
</script>
</span> then it is also a violation for <span class="MathJax_Preview"><script type="math/tex">
H_{i-1}
</script>
</span> therefore <span class="MathJax_Preview"><script type="math/tex">
e_{j}
</script>
</span> does not belongs to any optimal solution in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}_{i-1}
</script>
</span>, in contradiction that <span class="MathJax_Preview"><script type="math/tex">
e_{j}\in S^{*}\in\mathcal{X}_{i-1}
</script>
</span>.<br>
<br>
So suppose <span class="MathJax_Preview"><script type="math/tex">
w\left(e_{j}\right)\le w\left(e_{i}\right)
</script>
</span>. Note that in the sub-graph <span class="MathJax_Preview"><script type="math/tex">
H^{*}
</script>
</span> induced by <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span>, <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> must be dominated my some terminal <span class="MathJax_Preview"><script type="math/tex">
t\in T
</script>
</span>. Because <span class="MathJax_Preview"><script type="math/tex">
H^{*}
</script>
</span> is a sub-graph of a tree, if <span class="MathJax_Preview"><script type="math/tex">
t=r
</script>
</span> then we can close a cycle by adding <span class="MathJax_Preview"><script type="math/tex">
e_{i}
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span>, thus <span class="MathJax_Preview"><script type="math/tex">
t\ne r
</script>
</span>. Consider the sub-graph <span class="MathJax_Preview"><script type="math/tex">
H'=\left(V\left(G\right),S^{*}\cup\left\{ e_{i}\right\} \right)
</script>
</span>. The edge <span class="MathJax_Preview"><script type="math/tex">
e_{i}
</script>
</span> connects now the terminals <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
r
</script>
</span>, and that is the only violation in <span class="MathJax_Preview"><script type="math/tex">
H'
</script>
</span>. Since <span class="MathJax_Preview"><script type="math/tex">
H'
</script>
</span> is a sub-graph of a tree there is only one path connecting <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
r
</script>
</span> and it must contains the path <span class="MathJax_Preview"><script type="math/tex">
r\rightsquigarrow u
</script>
</span>, so removing the edge <span class="MathJax_Preview"><script type="math/tex">
e_{j}
</script>
</span> will break the connection between <span class="MathJax_Preview"><script type="math/tex">
t,r
</script>
</span> leading to a valid solution <span class="MathJax_Preview"><script type="math/tex">
S'=\left(S^{*}\cup\left\{ e_{i}\right\} \right)\backslash\left\{ e_{j}\right\} 
</script>
</span>. By the assumption that <span class="MathJax_Preview"><script type="math/tex">
w\left(e_{j}\right)\le w\left(e_{i}\right)
</script>
</span> the weight of <span class="MathJax_Preview"><script type="math/tex">
S'
</script>
</span> can only increase (in fact, the optimality of <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span> forces <span class="MathJax_Preview"><script type="math/tex">
w\left(e_{j}\right)=w\left(e_{i}\right)
</script>
</span>) and we get an optimal solution that contains <span class="MathJax_Preview"><script type="math/tex">
S_{i}
</script>
</span>, therefore <span class="MathJax_Preview"><script type="math/tex">
\mathcal{X}_{i}\ne\emptyset
</script>
</span>, contradiction. 
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> Running Time and Similar Algorithms
</h1>
<div class="Unindented">
Note that <span class="MathJax_Preview"><script type="math/tex">
\left|E\left(G\right)\right|=n-1
</script>
</span>, so step 1 of the algorithm takes <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(n\log n\right)
</script>
</span>. Step 2 is order of <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span>, times the cost of the operations inside the loop. There are data structures such as Union-Find that allow us to track the CCs of the current sub-graph in amortize small running time that is smaller than <span class="MathJax_Preview"><script type="math/tex">
\log n
</script>
</span>. Therefore, the total running time is bounded by <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}
</script>
</span><span class="MathJax_Preview"><script type="math/tex">
\left(n\log n\right)
</script>
</span>. 
</div>
<div class="Indented">
Note that the algorithm presented in section 2, is similar to Kruskal’s algorithm for finding the minimum spanning tree. In both cases, the greedy approach leads to an optimal solution. 
</div>
