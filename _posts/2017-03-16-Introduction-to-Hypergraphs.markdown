---
layout: post
author: Eran Amar
title:  Introduction to Hypergraphs
date:   2017-03-16
comments: true
tags: hypergraphs cuts_sparsifier
---


<script type="math/tex">
\newcommand{\lyxlock}{}
</script>
<noscript>
<div class="warning">
Warning: <a href="http://www.mathjax.org/">MathJax</a> requires JavaScript to correctly process the mathematics on this page. Please enable JavaScript on your browser.
</div><hr>
</hr></noscript>



<div class="Unindented">
In this post, I will discuss basic definitions and theory related to hypergraphs and cut sparsifiers. I will start from the basic definitions of Graphs, then generalize these definitions to Hypergraphs. Later, I will discuss some related theory about Cut-Sparsifiers.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-1">1</a> Basic Definitions in Graphs
</h1>
<div class="Unindented">
Let’s start from the very beginning - graphs. Graphs are fundamental objects in computer science, and they are used to model relations between objects. Formally, a <i>graph</i> <span class="MathJax_Preview"><script type="math/tex">
G=\left(V,E,w\right)
</script>
</span> is a tuple of two sets, <span class="MathJax_Preview"><script type="math/tex">
V
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
E
</script>
</span> and a function <span class="MathJax_Preview"><script type="math/tex">
w
</script>
</span>. <span class="MathJax_Preview"><script type="math/tex">
V
</script>
</span> is the set of <i>vertices</i> (i.e. the elements), usually denoted as <span class="MathJax_Preview"><script type="math/tex">
\left[n\right]:=\left\{ 1,2,..,n\right\} 
</script>
</span>, and <span class="MathJax_Preview"><script type="math/tex">
E
</script>
</span> is a set of <i>edges</i> between those vertices (i.e. indicating which elements belongs to the relation the graph represents). An edge is just a set of two vertices, that is <span class="MathJax_Preview"><script type="math/tex">
e=\left\{ u,v\right\} \in E\subseteq\left\{ A\mid A\subseteq V,\:\:\left|A\right|=2\right\} 
</script>
</span>. The function <span class="MathJax_Preview"><script type="math/tex">
w:\:E\rightarrow\mathbb{R}
</script>
</span> assigns a weight to every edge in the graph (we will focus on non-negative weights). For instance, we can model friendships in Facebook as a graph, the vertices will be all the users in Facebook, and there will be an edge between any pair of users if they are “friends” of each other.
</div>
<div class="Indented">
A <i>cut</i> in a graph <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is a partition of <span class="MathJax_Preview"><script type="math/tex">
V
</script>
</span> into two sets, <span class="MathJax_Preview"><script type="math/tex">
\left(S,V\backslash S\right)
</script>
</span>. We say that an edge <span class="MathJax_Preview"><script type="math/tex">
e=\left\{ u,v\right\} 
</script>
</span> <i>cross</i> the cut defined by <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> if and only if <span class="MathJax_Preview"><script type="math/tex">
S\cap e\ne\emptyset
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\left(V\backslash S\right)\cap e\ne\emptyset
</script>
</span>, that is, the edge “touches” both parts of the graph. Now we can talk about the <i>weight of the cut</i>, that is, if we have a cut that is defined by <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> and a weight function <span class="MathJax_Preview"><script type="math/tex">
w
</script>
</span> then, <span class="MathJax_Preview">
<script type="math/tex;mode=display">

w_{G}\left(S\right)=\sum_{\begin{array}{c}
e\in E\\
e\cap S\notin\{\emptyset,e\}
\end{array}}w(e)

</script>
</span>
in words, the weight of a cut is the sum of weights of all edges that cross it.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Generalizing to Hyper-Graphs
</h1>
<div class="Unindented">
The model of a graph, as defined above, can’t be used to describe non binary relationships. For example, say we want to model Facebook groups via a graph; the only way to do that is by including in the set of vertices also any group in Facebook, and then connecting each user to any of its groups with an edge. The main issue with such construction is that the vertices represents both <i>users</i> and <i>facebook-groups</i>. In order to avoid such ambiguity we can use a richer model, we can use Hypergraphs.
</div>
<div class="Indented">
Hypergraphs are generalization of graphs in the sense that edges may be of arbitrary size. Meaning that now <span class="MathJax_Preview"><script type="math/tex">
E\subseteq2^{V}\backslash\emptyset=\left\{ A\mid A\subseteq V,\:\:\left|A\right|>0\right\} 
</script>
</span>. Going back to our example, we can model Facebook groups by the graph <span class="MathJax_Preview"><script type="math/tex">
\left(V,E\right)
</script>
</span> when <span class="MathJax_Preview"><script type="math/tex">
V
</script>
</span> is the set of all users, and any group in Facebook will be an edge <span class="MathJax_Preview"><script type="math/tex">
e\in2^{V}
</script>
</span> (that notation stands for the <span class="blue"><a class="URL" href="https://en.wikipedia.org/wiki/Power_set">Power Set</a></span> of <span class="MathJax_Preview"><script type="math/tex">
V
</script>
</span>) such that <span class="MathJax_Preview"><script type="math/tex">
e
</script>
</span> contains the users belongs to that group. 
</div>
<div class="Indented">
Here is another example, in purely mathematical form, let <span class="MathJax_Preview"><script type="math/tex">
G=\left(\left[5\right],\left\{ \left\{ 4\right\} ,\left\{ 1,2,5\right\} \right\} \right)
</script>
</span>, which is a valid hypergraph over five vertices <span class="MathJax_Preview"><script type="math/tex">
\left\{ 1,2,..,5\right\} 
</script>
</span> and with two edges: <span class="MathJax_Preview"><script type="math/tex">
\left\{ 4\right\} 
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\left\{ 1,2,5\right\} 
</script>
</span>. 
</div>
<div class="Indented">
We also require the domain of the<i> weight function </i>to be <span class="MathJax_Preview"><script type="math/tex">
2^{V}\backslash\emptyset
</script>
</span> (rather than <span class="MathJax_Preview"><script type="math/tex">
E
</script>
</span> in the case of regular graphs). That requirement allows us to apply the same definitions of <i>cuts</i> and <i>cut weight</i> as they were phrased for regular graphs. 
</div>
<div class="Indented">
Usually we are interested in family of hypergraphs with limited size, that is, hypergraphs where each edge is of size at most <span class="MathJax_Preview"><script type="math/tex">
r
</script>
</span>. Such hypergraphs are said to be <i>r-uniform</i>.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> Cuts Sparsification 
</h1>
<div class="Unindented">
We move to define Cut-Sparsifiers: Let <span class="MathJax_Preview"><script type="math/tex">
\epsilon\in\left(0,1\right)
</script>
</span>. Given a hypergraph <span class="MathJax_Preview"><script type="math/tex">
H=\left(V,E\right)
</script>
</span> and a weight function <span class="MathJax_Preview"><script type="math/tex">
w
</script>
</span>, we say that a hypergraph <span class="MathJax_Preview"><script type="math/tex">
K=\left(V,E_{\epsilon},w\right)
</script>
</span> is <i><span class="MathJax_Preview"><script type="math/tex">
\epsilon
</script>
</span>-cut-sparsifier</i> of <span class="MathJax_Preview"><script type="math/tex">
H
</script>
</span> if <i><span class="MathJax_Preview">
<script type="math/tex;mode=display">

\forall S\subset V\qquad(1-\epsilon)\cdot w_{H}(S)\le w_{K}(S)\le(1+\epsilon)\cdot w_{H}(S)

</script>
</span>
</i>and the set <span class="MathJax_Preview"><script type="math/tex">
E_{\epsilon}
</script>
</span> may be <b>any set</b> in <span class="MathJax_Preview"><script type="math/tex">
2^{V}\backslash\emptyset
</script>
</span>. The subscript “<span class="MathJax_Preview"><script type="math/tex">
H
</script>
</span>” in the function <span class="MathJax_Preview"><script type="math/tex">
w_{H}\left(\cdot\right)
</script>
</span> indicates that the weights are over the hypergraph <span class="MathJax_Preview"><script type="math/tex">
H
</script>
</span> (and similarly with <span class="MathJax_Preview"><script type="math/tex">
w_{K}\left(\cdot\right)
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
K
</script>
</span>). 
</div>
<div class="Indented">
It is not part of the definition, but the goal is to find cut-sparsifier that shrink the number of edges, that is, with the smallest <span class="MathJax_Preview"><script type="math/tex">
\left|E_{\epsilon}\right|
</script>
</span> possible. Whereas in regular graphs it is understood that we want to minimize the <b>number</b> of edges, in hypergraphs we should also consider the <b>size</b> of the edges in <span class="MathJax_Preview"><script type="math/tex">
E_{\epsilon}
</script>
</span>. That is, the quality of the sparsifier will be measured also by the size of the edges in the resulting graph. For that we define the <i>total edge size</i> of a hypergraph to be <span class="MathJax_Preview"><script type="math/tex">
\sum_{e\in E_{\epsilon}}\left|e\right|
</script>
</span>.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> Upper &amp; Lower Bounds for Sparsification
</h1>
<div class="Unindented">
For regular graphs, we<a class="URL" href="http://dx.doi.org/10.1016/j.jpdc.2009.04.011"> already know</a> how to reduce the number of edges to be <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(n/\epsilon^{2}\right)
</script>
</span> for any <span class="MathJax_Preview"><script type="math/tex">
\epsilon\in\left(0,1\right)
</script>
</span>. That is surprising, because no matter how many edges there are in the original graph, which can be up to <span class="MathJax_Preview"><script type="math/tex">
n^{2}
</script>
</span>, there is an algorithms that can reduce the number of edges to <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(n/\epsilon^{2}\right)
</script>
</span>, while maintaining approximately the same weights for all possible cuts in the graph. Moreover, that algorithm find the cut-sparsifier in polynomial time. 
</div>
<div class="Indented">
Later results show that this upper bound is also a lower bound, that is, there are graphs that cannot be reduced into less that <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(n/\epsilon^{2}\right)
</script>
</span> edges.
</div>
<div class="Indented">
However, when working with hypergraphs it is not quite clear if one can reduce the total edges size of the graph to be even order of <span class="MathJax_Preview"><script type="math/tex">
n^{2}
</script>
</span>. Think about it, potentially the number of edges in a hypergraph can be <span class="MathJax_Preview"><script type="math/tex">
2^{n}
</script>
</span>, so <span class="MathJax_Preview"><script type="math/tex">
n^{2}
</script>
</span> can be considered quite small for such hypergraphs. 
</div>
<div class="Indented">
An interesting<a class="URL" href="https://arxiv.org/abs/1409.2391"> result from 2015</a>, showed lower bound of <span class="MathJax_Preview"><script type="math/tex">
O(\epsilon^{-2}n\cdot r)
</script>
</span> edges for <i>r-</i>uniform hypergraphs (assuming that <span class="MathJax_Preview"><script type="math/tex">
r>\log\left(n\right)
</script>
</span>), which, when converting to total edges size is actually <span class="MathJax_Preview"><script type="math/tex">
O(\epsilon^{-2}n\cdot r^{2})
</script>
</span>. 
</div>
<div class="Indented">
The question that one can ask now is, can we do better? Is there an algorithm that can construct a cut-sparsifier for hypergraphs with <i>smaller</i> total edges size?
</div>
<div class="Indented">
That is, actually, an open question which is studied nowdays.
</div>
