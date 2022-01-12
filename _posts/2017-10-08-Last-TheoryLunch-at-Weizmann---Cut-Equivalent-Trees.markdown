---
layout: post
author: Eran Amar
title:  Last TheoryLunch at Weizmann - Cut Equivalent Trees
date:   2017-10-08
comments: true
tags: graphs last_theory_lunch_weizmann
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
<a class="toc" name="toc-Section-1">1</a> Last TheoryLunch Meeting at Weizmann Institute of Science
</h1>
<div class="Unindented">
For those of you who don’t know, this blog originated from my lunch meetings with my friends at <a class="URL" href="https://www.weizmann.ac.il/feinberg/academics/msc-program-outline">Weizmann Institute of Science</a> during my Master studies. As all good things comes to an end, also did my time at Weizmann, and last week our lunch group gathered for the last time. This post presents the topic discussed on that meeting.
</div>
<div class="Indented">
I want to thank my good friends, Yosef Pogrow and Ron Shiff, for the great time we had and for all the enriching lunch meetings (also those which didn’t include theory). Without you guys, this blog would not have been created, I wish you both best of luck ahead!
</div>
<div class="Indented">
This post, by the way, contains some results from the research of Yosef Pogrow (who also presented most of the meetings). 
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Min-Cuts in a Graph
</h1>
<div class="Unindented">
This post deals with undirected, weighted graphs and cuts on them. We denote by <span class="MathJax_Preview"><script type="math/tex">
G=\left(V,E_{G},w_{G}\right)
</script>
</span> the graph equipped with a weight function on its edges. A <i>cut</i> in this graph is a partition of its vertices to <span class="MathJax_Preview"><script type="math/tex">
\left(S,V\backslash S\right)
</script>
</span> where <span class="MathJax_Preview"><script type="math/tex">
S\subseteq V
</script>
</span>. For simplicity, we will only mention one of the parts in the partition, usually <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span>. Note that it doesn’t matter whether we refer to the first part in the partition <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span>, or to the second part <span class="MathJax_Preview"><script type="math/tex">
V\backslash S
</script>
</span>, because both parts define the same cut.
</div>
<div class="Indented">
The <i>value</i> of the cut is defined as<span class="MathJax_Preview">
<script type="math/tex;mode=display">

w_{G}\left(S\right):=\sum_{uv\in E_{G},u\in S,v\notin S}w_{G}\left(uv\right)

</script>
</span>
</div>
<div class="Indented">
A <i>min <span class="MathJax_Preview"><script type="math/tex">
st
</script>
</span>-cut</i> for a pair <span class="MathJax_Preview"><script type="math/tex">
s\ne t\in V
</script>
</span> is a cut defined by <span class="MathJax_Preview"><script type="math/tex">
S\subseteq V
</script>
</span> such that <span class="MathJax_Preview">
<script type="math/tex;mode=display">

S=\arg\min_{S'\subseteq V}w_{G}\left(S'\right)

</script>
</span>
note that it is possible that several cuts will satisfy the definition above, but for our purposes they are equivalent (since they all have the same value).
</div>
<div class="Indented">
Before diving into the theorem about cut equivalent trees, we setup the following notation. For a given tree <span class="MathJax_Preview"><script type="math/tex">
T=\left(V,E\right)
</script>
</span> and any edge <span class="MathJax_Preview"><script type="math/tex">
uv\in E
</script>
</span>, removing that edge from the tree breaks it down into two connected components, one that contains <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span>, and another one that contains <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span>. Denote the component containing <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> as <span class="MathJax_Preview"><script type="math/tex">
S_{T|u}\subseteq V
</script>
</span>, and then the other component is exactly <span class="MathJax_Preview"><script type="math/tex">
S_{T|v}:=V\backslash S_{T|u}
</script>
</span>. Since the only path connecting <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> was the removed edge, then <span class="MathJax_Preview"><script type="math/tex">
S_{T|u}
</script>
</span> is actually the min <span class="MathJax_Preview"><script type="math/tex">
uv
</script>
</span>-cut in <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> (the same is true for <span class="MathJax_Preview"><script type="math/tex">
S_{T|v}
</script>
</span> since it defines the same cut). 
</div>
<div class="Indented">
A simple fact about trees is, that for any tree <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> and vertices <span class="MathJax_Preview"><script type="math/tex">
s\ne t
</script>
</span> inside it, the min <span class="MathJax_Preview"><script type="math/tex">
st
</script>
</span>-cut is given by <span class="MathJax_Preview"><script type="math/tex">
S_{T|u}
</script>
</span> where <span class="MathJax_Preview"><script type="math/tex">
uv\in E_{T}
</script>
</span> is the edge with <i>minimum weight</i> along the path that connects <span class="MathJax_Preview"><script type="math/tex">
s
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>. Since any <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> vertices tree has exactly <span class="MathJax_Preview"><script type="math/tex">
n-1
</script>
</span> edges, there are at most <span class="MathJax_Preview"><script type="math/tex">
n-1
</script>
</span> unique <i>values</i> of min-cuts that the tree can yield.
</div>
<div class="Theorem">
Cut-Equivalent Trees [Gomory and Hu, 1961]. For any weighted undirected graph <span class="MathJax_Preview"><script type="math/tex">
G=\left(V,E_{G},w_{G}\right)
</script>
</span> there is a weighted tree on the same set of vertices <span class="MathJax_Preview"><script type="math/tex">
T=\left(V,E_{T},w_{T}\right)
</script>
</span> such that:<br>
(a) for any <span class="MathJax_Preview"><script type="math/tex">
s\ne t\in V
</script>
</span>, the value of the min <span class="MathJax_Preview"><script type="math/tex">
st
</script>
</span>-cut in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is equal the value of the min <span class="MathJax_Preview"><script type="math/tex">
st
</script>
</span>-cut in <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> (with respect to their respective weight functions), and<br>
(b) for any edge <span class="MathJax_Preview"><script type="math/tex">
uv\in E_{T}
</script>
</span>, <span class="MathJax_Preview">
<script type="math/tex;mode=display">

w_{T}\left(S_{T|u}\right)=w_{G}\left(S_{T|u}\right)

</script>
</span>
Such trees are known as Cut-Equivalent trees (also known as GH-trees).
</div>
<div class="Unindented">

</div>
<div class="Indented">
The proof is omitted from this post, and we move to discuss a nice implication of that theorem: it puts a limit on the number of different min-cut <i>values</i> that a graph can have. Since there are <span class="MathJax_Preview"><script type="math/tex">
{n \choose 2}
</script>
</span> different <span class="MathJax_Preview"><script type="math/tex">
st
</script>
</span> pairs in a graph there can be potentially <span class="MathJax_Preview"><script type="math/tex">
{n \choose 2}
</script>
</span> unique min-cut values, however, the theorem says that any value of a min-cut in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is being represented by a (possibly different) min-cut in <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span>, and there are at most <span class="MathJax_Preview"><script type="math/tex">
n-1
</script>
</span> min-cut values in <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span>, then <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> has at most <span class="MathJax_Preview"><script type="math/tex">
n-1
</script>
</span> unique min-cut values.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> General Cuts in a Graph
</h1>
<div class="Unindented">
Until now, we talked about minimum <span class="MathJax_Preview"><script type="math/tex">
st
</script>
</span> cuts. The theorem above state that for any graph <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span>, there is a cut-equivalent tree <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> that can tell us much about all the minimum cuts in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span>. It can tell us all the possible min-cut values that <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> can have, moreover, for any min-cut value <span class="MathJax_Preview"><script type="math/tex">
x
</script>
</span>, there is a very easy way to find a cut in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> that have this exact value. All we need to do is to find an edge in the tree that have weight <span class="MathJax_Preview"><script type="math/tex">
x
</script>
</span>, suppose that edge is <span class="MathJax_Preview"><script type="math/tex">
uv\in E_{T}
</script>
</span>, and the desired cut in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> is given by <span class="MathJax_Preview"><script type="math/tex">
S_{T|u}
</script>
</span>.
</div>
<div class="Indented">
One interesting question to ask is, <i>what cut-equivalent trees can tell us about “general” cuts?</i> The answer is given in the following theorem.
</div>
<div class="Theorem">
[Krauthgamer and Pogrow, 2017]. Let <span class="MathJax_Preview"><script type="math/tex">
G=\left(V,E_{G},w_{G}\right)
</script>
</span> be a weighted undirected graph with <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> vertices, and let <span class="MathJax_Preview"><script type="math/tex">
T=\left(V,E_{T},w_{T}\right)
</script>
</span> be a cut-equivalent tree of <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span>. Then for <b>any</b> <span class="MathJax_Preview"><script type="math/tex">
S\subseteq V
</script>
</span><span class="MathJax_Preview">
<script type="math/tex;mode=display">

w_{G}\left(S\right)\le w_{T}\left(S\right)\le\left(n-1\right)\cdot w_{G}\left(S\right)

</script>
</span>
</div>
<div class="Proof">
We start with the left-hand side. Recall that the value of a cut <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> is the sum of weights of all the “crossing edges”, meaning, edges which have endpoints in both parts of the partition. Then, combined with the second property of the tree, and the fact that <span class="MathJax_Preview"><script type="math/tex">
w_{T}\left(st\right)=w_{T}\left(S_{T|s}\right)
</script>
</span> for any edge <span class="MathJax_Preview"><script type="math/tex">
st\in E_{T}
</script>
</span>, we get<span class="MathJax_Preview">
<script type="math/tex;mode=display">
\begin{aligned}
w_{T}\left(S\right) & =\sum_{st\in E_{T},s\in S,t\notin S}w_{T}\left(st\right)\\
 & =\sum_{st\in E_{T},s\in S,t\notin S}w_{G}\left(S_{T|s}\right)
\end{aligned}
</script>
</span>
so it is suffice to show that any crossing edge in the sum of the original graph <span class="MathJax_Preview"><script type="math/tex">
w_{G}\left(S\right)
</script>
</span>, contributes also to a sum of <span class="MathJax_Preview"><script type="math/tex">
w_{G}\left(S_{T|s}\right)
</script>
</span> for <i>some</i> tree-edge <span class="MathJax_Preview"><script type="math/tex">
st\in E_{T}
</script>
</span> such that <span class="MathJax_Preview"><script type="math/tex">
s\in S,t\notin S
</script>
</span>. So fix some crossing edge <span class="MathJax_Preview"><script type="math/tex">
uv\in E_{G}
</script>
</span> (<span class="MathJax_Preview"><script type="math/tex">
u\in S,v\in V\backslash S
</script>
</span>) from the original graph, and consider the unique path connecting <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> over the tree. Since we starting the path with a vertex from <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> and ending with a vertex in <span class="MathJax_Preview"><script type="math/tex">
V\backslash S
</script>
</span>, there must be an edge <span class="MathJax_Preview"><script type="math/tex">
st\in E_{T}
</script>
</span> along the path such that <span class="MathJax_Preview"><script type="math/tex">
s\in S
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
t\in V\backslash S
</script>
</span>. That edge <span class="MathJax_Preview"><script type="math/tex">
st
</script>
</span> separates <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> from <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> in the tree, so <span class="MathJax_Preview"><script type="math/tex">
u\in S_{T|s}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
v\notin S_{T|s}
</script>
</span>. Since <span class="MathJax_Preview"><script type="math/tex">
uv\in E_{G}
</script>
</span> then it contributes to <span class="MathJax_Preview"><script type="math/tex">
w_{G}\left(S_{T|s}\right)
</script>
</span>, which is what we wanted to prove.<br>
We proceed to show the second inequality. For each <span class="MathJax_Preview"><script type="math/tex">
st\in E_{T}
</script>
</span> in the sum of <span class="MathJax_Preview"><script type="math/tex">
w_{T}\left(S\right)
</script>
</span>, we know that <span class="MathJax_Preview"><script type="math/tex">
w_{T}\left(st\right)\le w_{G}\left(S\right)
</script>
</span> since the weight of the tree-edge <span class="MathJax_Preview"><script type="math/tex">
w_{T}\left(st\right)
</script>
</span> is equal to the <i>min <span class="MathJax_Preview"><script type="math/tex">
st
</script>
</span>-cut</i> in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span>, thus in particular smaller or equal to the value of the cut <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> (which also separates <span class="MathJax_Preview"><script type="math/tex">
s
</script>
</span> from <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> but might not be of minimum weight). The inequality follows by summing over all the crossing edges in <span class="MathJax_Preview"><script type="math/tex">
S
</script>
</span> over <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span>, which is at most <span class="MathJax_Preview"><script type="math/tex">
n-1
</script>
</span> edges.
</div>
<div class="Unindented">

</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> The Inequalities are Tight
</h1>
<div class="Unindented">
We conclude this post with examples for cases in which one of the inequalities became an equality. 
</div>
<div class="Indented">
For the first inequality, take any graph <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> and a corresponding cut equivalent tree <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span>. Fix an arbitrary tree-edge <span class="MathJax_Preview"><script type="math/tex">
st\in E_{T}
</script>
</span>. The cut <span class="MathJax_Preview"><script type="math/tex">
S=S_{T|s}
</script>
</span> is the min <span class="MathJax_Preview"><script type="math/tex">
st
</script>
</span>-cut in <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> and by the first property of the tree <span class="MathJax_Preview"><script type="math/tex">
w_{G}\left(S\right)=w_{T}\left(S\right)
</script>
</span>.
</div>
<div class="Indented">
For the second inequality, consider the complete graph on <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> vertices with all its edges weighted <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span>. Its cut-equivalent tree must be a star in which every edge weighed <span class="MathJax_Preview"><script type="math/tex">
n-1
</script>
</span>. See example of an eight vertices star in the figure below (taken from Wikipedia).
</div>
<div class="Indented">
<div class="Frameless" style="width: 100%;">
<div class="center">
<img alt="figure star_graph.png" class="embedded" src="/site/assets/imgs/2017-10-08-cut-equivalent-trees/star_graph.png" style="width: 5cm; max-width: 1200px; height: auto; max-height: 1200px;">
</div>
</div>
</div>
<div class="Indented">
Suppose the vertex in the center of the star is <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span>. The cut <span class="MathJax_Preview"><script type="math/tex">
S=\left\{ v\right\} 
</script>
</span> has weight of <span class="MathJax_Preview"><script type="math/tex">
n-1
</script>
</span> over <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span>, however, it has weight <span class="MathJax_Preview"><script type="math/tex">
\left(n-1\right)^{2}
</script>
</span> in the star <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> which is exactly <span class="MathJax_Preview"><script type="math/tex">
\left(n-1\right)\cdot w_{G}\left(S\right)
</script>
</span>.
</div>
