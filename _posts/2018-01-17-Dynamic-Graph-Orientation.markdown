---
layout: post
author: Eran Amar
title:  Dynamic-Graph Orientation
date:   2018-01-17
comments: true
tags: graphs streaming_algorithm
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
<a class="toc" name="toc-Section-1">1</a> Orienting Acyclic Graphs
</h1>
<div class="Unindented">
Throughout this post, <span class="MathJax_Preview"><script type="math/tex">
G=\left(V,E\right)
</script>
</span> is an undirected acyclic graph on <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> vertices and <span class="MathJax_Preview"><script type="math/tex">
m\le n-1
</script>
</span> edges. The problem we want to solve is called <i>graph orientation</i>:
</div>
<div class="Indented">
The input <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is given in the <i>insertion-only</i> streaming model. That is, we know <span class="MathJax_Preview"><script type="math/tex">
V
</script>
</span> in advance, and the set <span class="MathJax_Preview"><script type="math/tex">
E
</script>
</span> is being revealed one edge at a time. For each edge <span class="MathJax_Preview"><script type="math/tex">
\left(u,v\right)\in E
</script>
</span> that we get, we need to assign a direction (i.e. an orientation), meaning to decide that it either points to <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> or to <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span>. When <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> points to <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> we denote it by <span class="MathJax_Preview"><script type="math/tex">
u\rightarrow v
</script>
</span>. Note that when we assign a direction to a new edge, we may as well update the orientation of previously seen edges. When the stream ends, we left with a directed version of <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span>, denote it <span class="MathJax_Preview"><script type="math/tex">
\hat{G}=\left(V,\hat{E}\right)
</script>
</span>. 
</div>
<div class="Indented">
We are interested in two metrics: the first is the<i> outgoing degree bound</i>, which is defined by the term <span class="MathJax_Preview">
<script type="math/tex;mode=display">

deg\left(\hat{G}\right):=\max_{v\in V}deg_{out}\left(v;\hat{G}\right)

</script>
</span>
 where <span class="MathJax_Preview"><script type="math/tex">
deg_{out}(v;\hat{G}):=\left|\left\{ u\in V\mid v\rightarrow u\right\} \right|
</script>
</span> is the number of <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span>’s neighbors which <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> points to, in the oriented graph <span class="MathJax_Preview"><script type="math/tex">
\hat{G}
</script>
</span>.
</div>
<div class="Indented">
The second metric is a bound on the <i>orientation cost</i> for a single step of the stream. That is, a bound on the number of edge updates for every time a new edge is being revealed. 
</div>
<div class="Indented">
Due to the fact that <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is acyclic, we know it is actually a collection of one or more trees, then there exists an orientation <span class="MathJax_Preview"><script type="math/tex">
\hat{G}
</script>
</span> in which <span class="MathJax_Preview"><script type="math/tex">
deg\left(\hat{G}\right)\le1
</script>
</span> (think that each node points to its parent in its corresponding tree).
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(1\right)
</script>
</span> Orientation Cost
</h1>
<div class="Unindented">
Lets start with a very simple orientation rule: For every step of the stream, let <span class="MathJax_Preview"><script type="math/tex">
e_{i}=\left(u,v\right)
</script>
</span> be the edge given in the current step, and <span class="MathJax_Preview"><script type="math/tex">
E_{i}=\left\{ e_{1},..,e_{i}\right\} 
</script>
</span>. Consider the endpoints of <span class="MathJax_Preview"><script type="math/tex">
e_{i}
</script>
</span>. If <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> belongs to a larger connected component relative to the connected component of <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span>, under the graph induced by <span class="MathJax_Preview"><script type="math/tex">
E_{i}
</script>
</span>, then orient <span class="MathJax_Preview"><script type="math/tex">
e_{i}
</script>
</span> to be <span class="MathJax_Preview"><script type="math/tex">
v\rightarrow u
</script>
</span>, otherwise orient it <span class="MathJax_Preview"><script type="math/tex">
u\rightarrow v
</script>
</span>. Note that <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> cannot be in the same connected component in <span class="MathJax_Preview"><script type="math/tex">
G_{i}=\left(V,E_{i}\right)
</script>
</span>.
</div>
<div class="Indented">
Obviously, that update rule has constant orientation cost (in particular it never change the orientations of previous edges). However, the outgoing degree bound is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(\log n\right)
</script>
</span>. To prove that, consider some arbitrary vertex <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span>. We increase its outgoing degree by <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span> when we make it points to another <i>larger</i> connected component. So every time the outgoing degree of <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> increases by <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span>, the size of its connected component is at least doubled. Since we have <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> vertices, we cannot double that size more than <span class="MathJax_Preview"><script type="math/tex">
\log_{2}n
</script>
</span> times.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(1\right)
</script>
</span> Outgoing Degree Bound
</h1>
<div class="Unindented">
Now we will see an update rule that yields a constant degree bound, say <span class="MathJax_Preview"><script type="math/tex">
4
</script>
</span>: 
</div>
<div class="Indented">
Upon receiving <span class="MathJax_Preview"><script type="math/tex">
e_{i}=\left(u,v\right)
</script>
</span>, orient it <span class="MathJax_Preview"><script type="math/tex">
v\rightarrow u
</script>
</span>. Then, if the outgoing degree of <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> exceeds <span class="MathJax_Preview"><script type="math/tex">
4
</script>
</span>, we apply <span class="MathJax_Preview"><script type="math/tex">
makeSink\left(v\right)
</script>
</span>. This procedure will flip the orientation of any outgoing edge of <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span>. Next, check any neighbor <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> that was affected by this change, and apply <span class="MathJax_Preview"><script type="math/tex">
makeSink\left(u\right)
</script>
</span> if it has more than <span class="MathJax_Preview"><script type="math/tex">
4
</script>
</span> outgoing edges. This process continues recursively until no orientation is changed.
</div>
<div class="Indented">
Interesting question that immediately pops to mind is, <i>why this process of recursive updates eventually terminates? </i>
</div>
<div class="Indented">
To see that infinite oscillation is impossible, consider some optimal orientation of <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span>, denote it <span class="MathJax_Preview"><script type="math/tex">
\hat{G}^{*}
</script>
</span>, in which <span class="MathJax_Preview"><script type="math/tex">
deg\left(\hat{G}^{*}\right)\le1
</script>
</span>. Suppose that <span class="MathJax_Preview"><script type="math/tex">
e_{i}=\left(v,u_{1}\right)
</script>
</span> was revealed to the stream, and let <span class="MathJax_Preview"><script type="math/tex">
u_{1},u_{2},...
</script>
</span> be the chain of vertices that we applied <span class="MathJax_Preview"><script type="math/tex">
makeSink\left(\cdot\right)
</script>
</span> to, after orienting it. Denote by <span class="MathJax_Preview"><script type="math/tex">
\phi_{k}
</script>
</span> the number of edges in <span class="MathJax_Preview"><script type="math/tex">
G_{i}=\left(V,E_{i}\right)
</script>
</span> that don’t agree with <span class="MathJax_Preview"><script type="math/tex">
\hat{G}^{*}
</script>
</span>, after applying <span class="MathJax_Preview"><script type="math/tex">
makeSink\left(\cdot\right)
</script>
</span> on <span class="MathJax_Preview"><script type="math/tex">
u_{1},..,u_{k}
</script>
</span>. So for example, <span class="MathJax_Preview"><script type="math/tex">
\phi_{0}
</script>
</span> is the number of conflicting edges right after we inserted <span class="MathJax_Preview"><script type="math/tex">
e_{i}
</script>
</span> but before we start the recursive fixing process, and <span class="MathJax_Preview"><script type="math/tex">
\phi_{1}
</script>
</span> is the number of conflicting edges after we flip all the outgoing edges of <span class="MathJax_Preview"><script type="math/tex">
u_{1}
</script>
</span>, and so on.
</div>
<div class="Indented">
We will show that <span class="MathJax_Preview"><script type="math/tex">
\phi_{0},\phi_{1},..
</script>
</span> is a strictly decreasing series, and since <span class="MathJax_Preview"><script type="math/tex">
\phi_{k}
</script>
</span> cannot be negative, that series must terminates. Fix some <span class="MathJax_Preview"><script type="math/tex">
\phi_{k}
</script>
</span>, which is the number of edges conflicting with <span class="MathJax_Preview"><script type="math/tex">
\hat{G}^{*}
</script>
</span> before applying <span class="MathJax_Preview"><script type="math/tex">
makeSink\left(u_{k+1}\right)
</script>
</span>. In that moment, <span class="MathJax_Preview"><script type="math/tex">
deg_{out}\left(u_{k+1}\right)>4
</script>
</span>, but in the optimal orientation it should be at most <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span>, which means that at least <span class="MathJax_Preview"><script type="math/tex">
4
</script>
</span> of <span class="MathJax_Preview"><script type="math/tex">
u_{k+1}
</script>
</span>’s outgoing edges have wrong orientations. So applying <span class="MathJax_Preview"><script type="math/tex">
makeSink\left(u_{k+1}\right)
</script>
</span> adding up to one conflicting edge, but on the other hand, fixing the orientation of at least <span class="MathJax_Preview"><script type="math/tex">
4
</script>
</span> other edges, thus the total number of conflicting edges with <span class="MathJax_Preview"><script type="math/tex">
\hat{G}^{*}
</script>
</span> decreased by at least <span class="MathJax_Preview"><script type="math/tex">
3
</script>
</span>, i.e. <span class="MathJax_Preview"><script type="math/tex">
\phi_{k}\ge3+\phi_{k+1}
</script>
</span>. 
</div>
<div class="Indented">
It is immediate that this algorithm yields an orientation with outgoing degree bound of <span class="MathJax_Preview"><script type="math/tex">
4
</script>
</span>. We now move to show that the <i>amortize</i> orientation cost is also constant. To show that we will use the <a class="URL" href="https://en.wikipedia.org/wiki/Accounting_method_(computer_science)">accounting method</a>, in which we assign positive and negative values to different operations that we perform during processing the stream. As long as those values are constants, if in every step of the stream the aggregated value is non-negative, then we may conclude that the amortize cost per operation is also constant. So suppose that the action of inserting a new edge has a value of <span class="MathJax_Preview"><script type="math/tex">
+2
</script>
</span>, and flipping orientation of an existing edge has a value of <span class="MathJax_Preview"><script type="math/tex">
-1
</script>
</span>. We first prove the following claim.
</div>
<div class="Claim">
Suppose we got a new edge <span class="MathJax_Preview"><script type="math/tex">
e_{i}
</script>
</span>, and let <span class="MathJax_Preview"><script type="math/tex">
u_{1},u_{2},...,u_{k}
</script>
</span> be the series of vertices that we applied on <span class="MathJax_Preview"><script type="math/tex">
makeSink
</script>
</span> during the recursive fixing process, after inserting <span class="MathJax_Preview"><script type="math/tex">
e_{i}
</script>
</span> to the graph. Then, all <span class="MathJax_Preview"><script type="math/tex">
u_{1},u_{2},...,u_{k}
</script>
</span> are distinct, and the value of <span class="MathJax_Preview"><script type="math/tex">
makeSink\left(u_{t}\right)
</script>
</span> is exactly <span class="MathJax_Preview"><script type="math/tex">
-5
</script>
</span> for any <span class="MathJax_Preview"><script type="math/tex">
t\in\left\{ 1,...,k\right\} 
</script>
</span>.
</div>
<div class="Proof">
For the first part, if there is a vertex in <span class="MathJax_Preview"><script type="math/tex">
u_{1},u_{2},...,u_{k}
</script>
</span> appearing twice, it means that we found a cycle in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> which is impossible. For the second part, assume toward contradiction that we have <span class="MathJax_Preview"><script type="math/tex">
t\in\left\{ 1,..,k\right\} 
</script>
</span> such that the outgoing degree of <span class="MathJax_Preview"><script type="math/tex">
u_{t}
</script>
</span> was at least 6. It must be due to <span class="MathJax_Preview"><script type="math/tex">
makeSink\left(\cdot\right)
</script>
</span> application on some vertices from <span class="MathJax_Preview"><script type="math/tex">
\left\{ u_{1},...,u_{t-1}\right\} 
</script>
</span>, which means that <span class="MathJax_Preview"><script type="math/tex">
u_{t}
</script>
</span> has two neighbors in <span class="MathJax_Preview"><script type="math/tex">
\left\{ u_{1},...,u_{t-1}\right\} 
</script>
</span>, and we know that those neighbors are connected, so we found a cycle in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> and that is a contradiction.
</div>
<div class="Unindented">

</div>
<div class="Indented">
It is left to show that at any point in time, the total value of previous operation is non negative. The following claim prove that.
</div>
<div class="Claim">
For every step, <span class="MathJax_Preview"><script type="math/tex">
i
</script>
</span>, of the stream, let <span class="MathJax_Preview"><script type="math/tex">
e_{i}
</script>
</span> be the edge revealed on that step. Let <span class="MathJax_Preview"><script type="math/tex">
u_{i,1},u_{i,2},...,u_{i,k_{i}}
</script>
</span> be the series of vertices that we applied on <span class="MathJax_Preview"><script type="math/tex">
makeSink
</script>
</span> during the recursive fixing process, with <span class="MathJax_Preview"><script type="math/tex">
\phi_{i,0,}...,\phi_{i,k_{i}}
</script>
</span> as defined before. Denote by <span class="MathJax_Preview"><script type="math/tex">
\psi_{i,t}
</script>
</span> the aggregated value of all the operations done until inserting <span class="MathJax_Preview"><script type="math/tex">
e_{i}
</script>
</span> (including) and after applying <span class="MathJax_Preview"><script type="math/tex">
makeSink
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
\left\{ u_{1},..,u_{t}\right\} 
</script>
</span>. Then, <span class="MathJax_Preview"><script type="math/tex">
\psi_{i,t}\ge2\cdot\phi_{i,t}
</script>
</span> for any <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>.
</div>
<div class="Proof">
By induction on <span class="MathJax_Preview"><script type="math/tex">
i
</script>
</span>. For <span class="MathJax_Preview"><script type="math/tex">
i=1
</script>
</span>, note that <span class="MathJax_Preview"><script type="math/tex">
\phi_{1,0}=0
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\psi_{1,0}=3
</script>
</span>. For <span class="MathJax_Preview"><script type="math/tex">
i>1
</script>
</span>, we have <span class="MathJax_Preview"><script type="math/tex">
\psi_{i,0}=\psi_{i-1,k_{i-1}}+2
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\phi_{i,0}\le\phi_{i-1,k_{i-1}}+1
</script>
</span>, so using the induction hypothesis, <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\psi_{i,0}\ge2+2\cdot\phi_{i-1,k_{i-1}}\ge2\cdot\phi_{i,0}

</script>
</span>
 for <span class="MathJax_Preview"><script type="math/tex">
t>0
</script>
</span>, assume by induction on <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> that the claim holds, then <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\psi_{i,t}=\psi_{i,t-1}-5\ge2\cdot\phi_{i,t-1}-5\ge2\left(\phi_{i,t}+3\right)-5\ge2\phi_{i,t}

</script>
</span>
</div>
<div class="Unindented">

</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> Releasing the Assumptions
</h1>
<div class="Unindented">
This post dealt with a relative simple model, in which the stream only revealing new edges but never deleting a previously seen one. In addition, we assume that our graph is acyclic which is also a very strong assumption. There are works that shows more sophisticated streaming algorithm for fully dynamic graphs (i.e. with edges insertions and <i>deletions</i>), and also works on general graphs that are not necessarily acyclic. Further details can be found in this <a class="URL" href="https://arxiv.org/abs/1312.1382">paper</a>.
</div>
