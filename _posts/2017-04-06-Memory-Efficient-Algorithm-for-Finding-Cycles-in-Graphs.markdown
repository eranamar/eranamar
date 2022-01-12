---
layout: post
author: Eran Amar
title:  Memory Efficient Algorithm for Finding Cycles in Graphs
date:   2017-04-06
comments: true
tags: complexity algorithms
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
<a class="toc" name="toc-Section-1">1</a> Cycle Detection in Graphs 
</h1>
<div class="Unindented">
Somewhere in 2015, I encountered a nice algorithmic question during an interview for an algotreading company. The question was <i>how one can decide if there is a cycle in an undirected connected graph</i>? There are two immediate answers here, the first is running <i>Breadth First Search (BFS)</i> on arbitrary node and memorizing the nodes that were already visited. The search ends either when the algorithm complete scanning the whole graph or when it reaches some node twice, outputting “No” or “Yes” respectively. Another possible solution is by running <i>Depth First Search (DFS)</i> and keeping a variable to count the length of the longest path explored. The algorithm declare a cycle if at any point in time the length of the path reaches the number of vertices in the graph. For the rest of this post, denote that number by <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span>.
</div>
<div class="Indented">
Those two solutions are simple, and more important for this discussion, have the same <i>space complexity</i>. That is, both consume around the same amount of working memory (in bits) ignoring the memory needed for the input and output of the algorithm. Even though DFS have implementation that requires less memory than BFS, ultimately both requires <span class="MathJax_Preview"><script type="math/tex">
\Omega\left(n\log n\right)
</script>
</span> bits. One can (informally) justify the aforementioned space complexity as follow: both algorithms have to keep track of their search (a queue in BFS and a stack is DFS) which potentially can be as large as the entire graph thus <span class="MathJax_Preview"><script type="math/tex">
\Omega\left(n\right)
</script>
</span>, and each node need <span class="MathJax_Preview"><script type="math/tex">
\Omega\left(\log n\right)
</script>
</span> bits for encoding so the total space complexity is <span class="MathJax_Preview"><script type="math/tex">
\Omega\left(n\log n\right)
</script>
</span> bits.
</div>
<div class="Indented">
During the rest of this post we will focus on <i>space complexity</i> rather than <i>time complexity</i>. We will present an algorithm that can determine if there is a cycle in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(\log^{2}n\right)
</script>
</span> bits [base on <a class="URL" href="http://www.sciencedirect.com/science/article/pii/S002200007080006X">this paper</a>] and then discuss the importance of related problem: the <i>s-t connectivity</i>.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> The <i>s-t Connectivity</i> Problem
</h1>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-2.1">2.1</a> Solving <i>s-t connectivity</i> as a Sub-step
</h2>
<div class="Unindented">
We will start from solving a slightly different problem then uses its solution as a procedure in our cycle-detection algorithm. 
</div>
<div class="Indented">
Introducing the <i>undirected s-t connectivity</i> problem: The input is an undirected graph <span class="MathJax_Preview"><script type="math/tex">
G=\left(V,E\right)
</script>
</span> of size <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> and two vertices <span class="MathJax_Preview"><script type="math/tex">
s,t\in V
</script>
</span>. The output should be <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span> if there is path in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> that connects <span class="MathJax_Preview"><script type="math/tex">
s
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>, and <span class="MathJax_Preview"><script type="math/tex">
0
</script>
</span> otherwise. The <i>directed s-t connectivity</i> is about the same but the input graph is directed (i.e its edges has directions) and the output should be <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span> whether there is a <i>directed</i> path from <span class="MathJax_Preview"><script type="math/tex">
s
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>.
</div>
<div class="Indented">
Before we present the algorithm, we first define the following predicate: for any <span class="MathJax_Preview"><script type="math/tex">
u,v\in V
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
k\in\mathbb{N}
</script>
</span> define <span class="MathJax_Preview"><script type="math/tex">
conn\left(u,v;k\right)=1
</script>
</span> if exists a path from <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> of at most <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span> steps (i.e. edges), otherwise <span class="MathJax_Preview"><script type="math/tex">
conn\left(u,v;k\right)=0
</script>
</span>. 
</div>
<div class="Indented">
The next observations are key points needed to solve the <i>s-t connectivity </i>efficiently (both for directed and undirected case).
</div>
<ol>
<li>
For any <span class="MathJax_Preview"><script type="math/tex">
u,v\in V
</script>
</span> and any <span class="MathJax_Preview"><script type="math/tex">
k\in\mathbb{N}
</script>
</span>, we have that <span class="MathJax_Preview"><script type="math/tex">
conn\left(u,v;2^{k}\right)=1
</script>
</span> if and only if <span class="MathJax_Preview"><script type="math/tex">
\exists w\in V
</script>
</span> such that <span class="MathJax_Preview"><script type="math/tex">
conn\left(u,w;2^{k-1}\right)=1
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
conn\left(w,v;2^{k-1}\right)=1
</script>
</span>
</li>
<li>
Let <span class="MathJax_Preview"><script type="math/tex">
u,v\in V
</script>
</span>, they are connected if and only if <span class="MathJax_Preview"><script type="math/tex">
conn\left(u,v;2^{\lceil\log n\rceil}\right)=1
</script>
</span>
</li>
</ol>
<div class="Unindented">
From here the algorithm is straightforward: denote the algorithm by <span class="MathJax_Preview"><script type="math/tex">
Alg\left(u,v;2^{k}\right)
</script>
</span> that upon receiving <span class="MathJax_Preview"><script type="math/tex">
u,v
</script>
</span> from the graph <span class="MathJax_Preview"><script type="math/tex">
G=\left(V,E\right)
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
k\in\mathbb{N}
</script>
</span>:
</div>
<ol>
<li>
If <span class="MathJax_Preview"><script type="math/tex">
k=0
</script>
</span>, it outputs <span class="MathJax_Preview"><script type="math/tex">
\mathbf{1}_{\left[u=v\right]}
</script>
</span>, and if <span class="MathJax_Preview"><script type="math/tex">
k=1
</script>
</span> it outputs <span class="MathJax_Preview"><script type="math/tex">
\mathbf{1}_{\left[u=v\vee\left(u,v\right)\in E\right]}
</script>
</span>
</li>
<li>
For each <span class="MathJax_Preview"><script type="math/tex">
w\in V
</script>
</span>: if <span class="MathJax_Preview"><script type="math/tex">
Alg\left(u,w;2^{k-1}\right)=1
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
Alg\left(w,v;2^{k-1}\right)=1
</script>
</span> then output <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span>
</li>
<li>
Output <span class="MathJax_Preview"><script type="math/tex">
0
</script>
</span>
</li>
</ol>
<div class="Unindented">
The solution for <i>s-t connectivity</i> can be achieved by outputting “Yes” if and only if <span class="MathJax_Preview"><script type="math/tex">
Alg\left(s,t,2^{\lceil\log n\rceil}\right)=1
</script>
</span>. 
</div>
<div class="Indented">
Obviously, the depth of the recursion is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(\log n\right)
</script>
</span> and in each level of the recursion the algorithm only needs to keep track of <span class="MathJax_Preview"><script type="math/tex">
w
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
k
</script>
</span>, which is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(\log n\right)
</script>
</span> bits, therefore the total memory complexity of the algorithm is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(\log^{2}n\right)
</script>
</span> bits.
</div>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-2.2">2.2</a> Extending the algorithm to detect cycles
</h2>
<div class="Unindented">
How solving the <i>s-t connectivity</i> problem helps us with cycles detection? The next observation answer that: let <span class="MathJax_Preview"><script type="math/tex">
G=\left(V,E\right)
</script>
</span> be the graph in question and <span class="MathJax_Preview"><script type="math/tex">
u,v\in V
</script>
</span> two endpoints of some edge, they must stay connected after removing that edge from <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> if and only if both participating in a cycle contained in <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span>.
</div>
<div class="Indented">
For the directed case the algorithm is as follow: iterate over any edge <span class="MathJax_Preview"><script type="math/tex">
\left(u,v\right)\in E
</script>
</span>, delete it temporarily from <span class="MathJax_Preview"><script type="math/tex">
G
</script>
</span> and check if <span class="MathJax_Preview"><script type="math/tex">
Alg\left(v,u;2^{\lceil\log n\rceil}\right)=1
</script>
</span> on the resulting graph, that is, if <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> is still connected to <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> even after you delete the edge from <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span>. If <span class="MathJax_Preview"><script type="math/tex">
Alg\left(v,u;2^{\lceil\log n\rceil}\right)=1
</script>
</span> then declare a cycle, else, restore back the edge and continue to the next one. Note that this solution also works for undirected graphs. The memory complexity is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(\log^{2}n\right)+\log n=\mathcal{O}\left(\log^{2}n\right)
</script>
</span> and we done.
</div>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-2.3">2.3</a> Is it the best we can do?
</h2>
<div class="Unindented">
Actually, for undirected graphs there is an algorithm with <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(\log n\right)
</script>
</span> bits, improving the square on the log in the construction from previous section. The curious reader can read more about it <a class="URL" href="http://www.cs.cornell.edu/courses/cs682/2008sp/Handouts/Reingold05.pdf">here</a>. For the directed case it is an open question whether we can improve the <span class="MathJax_Preview"><script type="math/tex">
\log^{2}n
</script>
</span> or not.
</div>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-2.4">2.4</a> Beyond Cycles in Graphs
</h2>
<div class="Unindented">
The <i>directed s-t connectivity </i>problem is an important theoretical problem because it captures the “hardness” of any problem that requires logarithmic amount of memory space. The reduction to show that is actually very easy, suppose we have an algorithm that solves some problem in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(s\right)
</script>
</span> space where <span class="MathJax_Preview"><script type="math/tex">
s
</script>
</span> is logarithmic in <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> (and <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> is the size of the input), generate the following directed graph: create a vertex <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> for any state (memory configuration) of the algorithm. Since its memory is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(s\right)
</script>
</span>, there are at most <span class="MathJax_Preview"><script type="math/tex">
2^{\mathcal{O}\left(s\right)}=\mathcal{O}\left(poly\left(n\right)\right)
</script>
</span> states (thus vertices). Create an edge from <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> iff <span class="MathJax_Preview"><script type="math/tex">
v
</script>
</span> is a valid possible successor state of <span class="MathJax_Preview"><script type="math/tex">
u
</script>
</span>. Add a vertex <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> and connects to it all the states that leads to outputting “Yes”. Denote by <span class="MathJax_Preview"><script type="math/tex">
s
</script>
</span> the starting-state of the algorithm and now solving the original problem is equivalent to finding a directed path from <span class="MathJax_Preview"><script type="math/tex">
s
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> in the states-graph.
</div>
<div class="Indented">
This reduction was presented by Walter Savitch in 1970, and it also works for Nondeterministic algorithms. The conclusion is that any nondeterministic problem that can be solved in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(s\right)
</script>
</span> space (where <span class="MathJax_Preview"><script type="math/tex">
s
</script>
</span> is logarithmic in the size of the input) can be solved deterministically in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(s^{2}\right)
</script>
</span>. Combining that conclusion with the algorithm for <i>directed s-t connectivity</i> yields that any problem with nondeterministic <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(\log n\right)
</script>
</span> space complexity can be solved deterministically (via reduction to <i>directed s-t connectivity</i>) with <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}\left(\log^{2}n\right)
</script>
</span> space complexity.
</div>
