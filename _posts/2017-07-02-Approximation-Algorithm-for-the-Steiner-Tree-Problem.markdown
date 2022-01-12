---
layout: post
author: Eran Amar
title:  Approximation Algorithm for the Steiner Tree Problem
date:   2017-07-02
comments: true
tags: algorithms approximation graphs
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
<a class="toc" name="toc-Section-1">1</a> The Steiner Tree Problem
</h1>
<div class="Unindented">
An instance of the <i>Steiner Tree</i> problem is a connected graph <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> on a set of <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> vertices <span class="MathJax_Preview"><script type="math/tex">
V\left(G\right)
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
m
</script>
</span> edges <span class="MathJax_Preview"><script type="math/tex">
E
</script>
</span><span class="MathJax_Preview"><script type="math/tex">
\left(G\right)
</script>
</span> equipped with a positive weight function <span class="MathJax_Preview"><script type="math/tex">
w:E\left(G\right)\to\mathbb{R}_{>0}
</script>
</span>. Given a set of terminals <span class="MathJax_Preview"><script type="math/tex">
T\subseteq V\left(G\right)
</script>
</span>, a valid solution is s set <span class="MathJax_Preview"><script type="math/tex">
S\subseteq E\left(G\right)
</script>
</span> that induces a sub-graph in which any pair of terminals <span class="MathJax_Preview"><script type="math/tex">
t_{1},t_{2}\in T
</script>
</span> is connected. The weight of a solution <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> is defined by <span class="MathJax_Preview"><script type="math/tex">
w\left(S\right)=\sum_{e\in S}w\left(e\right)
</script>
</span>, and we want to find a valid solution with minimum weight, usually denoted <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span>. Note that since the initial graph is connected there is always a valid solution. 
</div>
<div class="Indented">
Finding an exact optimal solution is NP-Complete. However, there is polynomial time approximation algorithm that yields a factor 2 approximation. We say that a solution <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> is 2-approximation for the problem if <span class="MathJax_Preview"><script type="math/tex">
w\left(S\right)\le2\cdot w\left(S^{*}\right)
</script>
</span> for some optimal solution <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span>.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Adding Assumptions “for Free”
</h1>
<div class="Unindented">
Given a graph <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> and weight-function <span class="MathJax_Preview"><script type="math/tex">
w
</script>
</span>, consider the following construction: Let <span class="MathJax_Preview"><script type="math/tex">
\tilde{G}
</script>
</span> be a clique on the same set of vertices <span class="MathJax_Preview"><script type="math/tex">
V\left(\tilde{G}\right)=V\left(G\right)
</script>
</span>, that is, <span class="MathJax_Preview"><script type="math/tex">
E\left(\tilde{G}\right)=\left\{ \left(u,v\right)\mid u\ne v\in V\left(G\right)\right\} 
</script>
</span>. Define the weight function <span class="MathJax_Preview"><script type="math/tex">
\tilde{w}
</script>
</span> as follows, <span class="MathJax_Preview"><script type="math/tex">
\forall u,v\in V\left(\tilde{G}\right)
</script>
</span> let <span class="MathJax_Preview"><script type="math/tex">
\tilde{w}\left(u,v\right)
</script>
</span> be the weight of the shortest path from <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> in the original <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span>. When “shortest path” is defined to be the path with minimum<i> sum of weights</i> among all the simple paths that from <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span>. Furthermore, it worth noting that by its definition, <span class="MathJax_Preview"><script type="math/tex">
\tilde{w}
</script>
</span> obeys the triangle inequality, meaning that for any triple <span class="MathJax_Preview"><script type="math/tex">
u,v,w\in V\left(\tilde{G}\right)
</script>
</span>, if <span class="MathJax_Preview"><script type="math/tex">
e_{1}=\left(u,v\right)
</script>
</span>, <span class="MathJax_Preview"><script type="math/tex">
e_{2}=\left(v,w\right)
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
e_{3}=\left(u,w\right)
</script>
</span> then <span class="MathJax_Preview"><script type="math/tex">
\tilde{w}\left(e_{3}\right)\le\tilde{w}\left(e_{1}\right)+\tilde{w}\left(e_{2}\right)
</script>
</span>. 
</div>
<div class="Indented">
Fix some <span class="MathJax_Preview"><script type="math/tex">
T\subseteq V\left(G\right)
</script>
</span>, we will show that the weight of any optimal solution for the original instance <span class="MathJax_Preview"><script type="math/tex">
\left(G,w\right)
</script>
</span> is <b>exactly</b> the same weight as of any optimal solution for <span class="MathJax_Preview"><script type="math/tex">
\left(\tilde{G},\tilde{w}\right)
</script>
</span>. We will do that by showing two inequalities: let <span class="MathJax_Preview"><script type="math/tex">
S^{*},\tilde{S}^{*}
</script>
</span> be optimal solutions for the original and the modified instances respectively. Then <span class="MathJax_Preview"><script type="math/tex">
w\left(S^{*}\right)\le w\left(\tilde{S}^{*}\right)
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
w\left(\tilde{S}^{*}\right)\le w\left(S^{*}\right)
</script>
</span>.
</div>
<div class="Indented">
Obviously, the inequality <span class="MathJax_Preview"><script type="math/tex">
w\left(\tilde{S}^{*}\right)\le w\left(S^{*}\right)
</script>
</span> holds, because <span class="MathJax_Preview"><script type="math/tex">
\tilde{G}
</script>
</span> contains all the edges of <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> and for any edge <span class="MathJax_Preview"><script type="math/tex">
e
</script>
</span> in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> its weight over the modified instance <span class="MathJax_Preview"><script type="math/tex">
\tilde{G}
</script>
</span> have <span class="MathJax_Preview"><script type="math/tex">
\tilde{w}\left(e\right)\le w\left(e\right)
</script>
</span>, by the definition of <span class="MathJax_Preview"><script type="math/tex">
\tilde{w}
</script>
</span>. Therefore, the optimal weight over <span class="MathJax_Preview"><script type="math/tex">
\tilde{G}
</script>
</span> can only be equal to or smaller than the one of <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span>.
</div>
<div class="Indented">
The other direction is less trivial. Take any <span class="MathJax_Preview"><script type="math/tex">
\tilde{S}^{*}
</script>
</span> and convert it to be a valid solution over the original <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> as follows: for any <span class="MathJax_Preview"><script type="math/tex">
e\in\tilde{S}^{*}
</script>
</span>, if <span class="MathJax_Preview"><script type="math/tex">
e\in E\left(G\right)
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
w\left(e\right)\le\tilde{w}\left(e\right)
</script>
</span> then add <span class="MathJax_Preview"><script type="math/tex">
e\in S^{*}
</script>
</span>. Otherwise, take all the edges along the shortest path connecting the endpoints of <span class="MathJax_Preview"><script type="math/tex">
e
</script>
</span> other the original graph <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> and add them to <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span>. Note that the entire weight of that path cannot exceeds <span class="MathJax_Preview"><script type="math/tex">
\tilde{w}\left(e\right)
</script>
</span> by the definition of <span class="MathJax_Preview"><script type="math/tex">
\tilde{w}
</script>
</span>. Therefore we end up with a valid solution <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span>, that also have <span class="MathJax_Preview"><script type="math/tex">
w\left(S^{*}\right)\le w\left(\tilde{S}^{*}\right)
</script>
</span>. In conclusion, <span class="MathJax_Preview"><script type="math/tex">
w\left(S^{*}\right)=w\left(\tilde{S}^{*}\right)
</script>
</span>.
</div>
<div class="Indented">
The proof above gave us a concrete polynomial-time procedure to convert any valid solution <span class="MathJax_Preview"><script type="math/tex">
\tilde{S}
</script>
</span> over <span class="MathJax_Preview"><script type="math/tex">
\tilde{G}
</script>
</span> to a valid solution <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> over <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> such that <span class="MathJax_Preview"><script type="math/tex">
w\left(S\right)\le w\left(\tilde{S}\right)
</script>
</span>. From that we can conclude that it is suffice to find a 2-approximation for the “easier” instance <span class="MathJax_Preview"><script type="math/tex">
\left(\tilde{G},\tilde{w}\right)
</script>
</span> and then converts it to a 2-approximation solution for the original instance <span class="MathJax_Preview"><script type="math/tex">
\left(G,w\right)
</script>
</span>, note also that the construction of <span class="MathJax_Preview"><script type="math/tex">
\tilde{G},\tilde{w}
</script>
</span> from <span class="MathJax_Preview"><script type="math/tex">
G,w
</script>
</span> can be done in polynomial time (for example use <a class="URL" href="https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm">Dijkstra's algorithm</a> for computing the shortest paths).
</div>
<div class="Indented">
In other words, we can assume for free (i.e. without losing anything) that <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is a clique and <span class="MathJax_Preview"><script type="math/tex">
w
</script>
</span> have the triangle inequality, and design an approximation algorithm that deals only with such instances.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> Approximation Algorithm for the Easier Problem
</h1>
<div class="Unindented">
Let <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> be a clique, <span class="MathJax_Preview"><script type="math/tex">
w
</script>
</span> be a weight function that obeys the triangle inequality, and <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> is a set of terminals. Use any known polynomial time algorithm to get a <i>Minimum Spanning Tree (MST)</i> on <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> (for instance, use <a class="URL" href="https://en.wikipedia.org/wiki/Kruskal%27s_algorithm">Kruskal's algorithm</a>), let the edges of that tree be <span class="MathJax_Preview"><script type="math/tex">
S\subseteq E\left(G\right)
</script>
</span>. Output <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> as the approximated solution for the <i>Steiner Tree</i> problem.
</div>
<div class="Indented">
Putting it differently, we claim that <span class="MathJax_Preview"><script type="math/tex">
w\left(S\right)\le2w\left(S^{*}\right)
</script>
</span>, when <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span> is some optimal solution for the <i>Steiner Tree</i> problem for that “easier” instance. 
</div>
<div class="Indented">
And now we will prove that. 
</div>
<div class="Indented">
Let <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span> be some optimal solution for the <i>Steiner Tree</i> problem. It must be a tree because otherwise we have a redundant edge such that removing it will decrease the weight of the solution and keep it valid (the terminals will stay connected); in contradiction to the optimality of <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span>. Run a <i>Depth First Search (DFS)</i> at the root of <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span>, traverse all the nodes in the tree and return back to the root. Memorize all the nodes traversed that way (with repetitions) and denote that sequence <span class="MathJax_Preview"><script type="math/tex">
P
</script>
</span>. By the way we build <span class="MathJax_Preview"><script type="math/tex">
P
</script>
</span>, it is a cycle that touches all the vertices of <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> (and maybe also others) and uses twice any edge of <span class="MathJax_Preview"><script type="math/tex">
S^{*}
</script>
</span>, once for the forward pass and once for the backward pass, so it has a length <span class="MathJax_Preview"><script type="math/tex">
k:=2\left|S^{*}\right|
</script>
</span>. Suppose that <span class="MathJax_Preview"><script type="math/tex">
P=\left(u_{1},...,u_{k}\right)
</script>
</span>, then we conclude that <span class="MathJax_Preview">
<script type="math/tex;mode=display">

w\left(P\right)=\sum_{i=1}^{k-1}w\left(\left(u_{i},u_{i+1}\right)\right)=2\cdot w\left(S^{*}\right)

</script>
</span>
</div>
<div class="Indented">
Define the following order on <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span>: for any two terminals <span class="MathJax_Preview"><script type="math/tex">
t_{1},t_{2}\in T
</script>
</span>, we say that <span class="MathJax_Preview"><script type="math/tex">
t_{1}
</script>
</span> comes before <span class="MathJax_Preview"><script type="math/tex">
t_{2}
</script>
</span> if the first appearance of <span class="MathJax_Preview"><script type="math/tex">
t_{1}
</script>
</span> in <span class="MathJax_Preview"><script type="math/tex">
P
</script>
</span> comes before the first appearance of <span class="MathJax_Preview"><script type="math/tex">
t_{2}
</script>
</span> in <span class="MathJax_Preview"><script type="math/tex">
P
</script>
</span>. Let <span class="MathJax_Preview"><script type="math/tex">
\left(t_{1},...,t_{\left|T\right|}\right)
</script>
</span> be the sequence of ordered terminals, note that it is also defines <i>a path</i> because <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is a clique, that is, <span class="MathJax_Preview"><script type="math/tex">
\left(t_{i},t_{i+1}\right)\in E\left(G\right)
</script>
</span> for any <span class="MathJax_Preview"><script type="math/tex">
i
</script>
</span>. Denote that path <span class="MathJax_Preview"><script type="math/tex">
\tilde{P}
</script>
</span>.
</div>
<div class="Indented">
By the triangle inequality, for any pair <span class="MathJax_Preview"><script type="math/tex">
t_{i},t_{i+1}\in T
</script>
</span> the weighed subpath in <span class="MathJax_Preview"><script type="math/tex">
\tilde{P}
</script>
</span>, which is the weight of a single edge, is less or equal to the weighted length in <span class="MathJax_Preview"><script type="math/tex">
P
</script>
</span> (which may use more than one edge), thus <span class="MathJax_Preview"><script type="math/tex">
w\left(\tilde{P}\right)\le w\left(P\right)
</script>
</span>. 
</div>
<div class="Indented">
Since <span class="MathJax_Preview"><script type="math/tex">
\tilde{P}
</script>
</span> touches all the terminals in <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> exactly once, it has no cycles, hence it is also a spanning tree over the vertices <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> and edges of the clique. Recall that the proposed solution <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> is a minimum spanning tree on <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span>, therefore <span class="MathJax_Preview"><script type="math/tex">
w\left(S\right)\le w\left(\tilde{P}\right)
</script>
</span>. We conclude that <span class="MathJax_Preview">
<script type="math/tex;mode=display">

w\left(S\right)\le w\left(\tilde{P}\right)\le w\left(P\right)=2\cdot w\left(S^{*}\right)

</script>
</span>
as required.
</div>
