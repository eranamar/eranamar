---
layout: post
author: Eran Amar
title:  On the Hardness of Counting Problems - Part 1
date:   2017-07-06
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
<a class="toc" name="toc-Section-1">1</a> What is a Counting Problem?
</h1>
<div class="Unindented">
Every undergraduate student in Computer Science knows the <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span> families and the legendary question “is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}
</script>
</span> equals <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span>?”. However, there is another interesting family that is not always covered in undergraduate courses; that is the <span class="MathJax_Preview"><script type="math/tex">
\#\mathcal{P}
</script>
</span> (“sharp P”) family. In this post we will define and discuss this family and two concrete similar-but-different problems from it that demonstrate interesting properties about its hardness. 
</div>
<div class="Indented">
Formally, <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}
</script>
</span> is the family of decision problems that can be solved in polynomial time. A known example is the <i>Perfect Matching</i> problem: given a graph, determine whether it has a perfect matching or not. A perfect matching is a set of edges that touches all the vertices in the graph and any pair of edges have disjoint endpoints.
</div>
<div class="Indented">
The family <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span> contains all the decision problems that has polynomial time algorithm over the non-deterministic computational model (we won’t get into that). We know that <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}\subseteq\mathcal{NP}
</script>
</span>, but we don’t know if the other direction also holds. A famous <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span>-complete example is the <i>Hamiltonian Cycle</i> problem: given a graph, determine whether it contains a cycle that visits every vertex exactly once. We say “complete” to note that the problem is harder at least as any other problem inside that family, that is, if we could solve it efficiently (i.e. in polynomial time) then we could solve any other problem in the family efficiently. When a problem <span class="MathJax_Preview"><script type="math/tex">
P_{1}
</script>
</span> is at least harder as other problem <span class="MathJax_Preview"><script type="math/tex">
P_{2}
</script>
</span> we denote it <span class="MathJax_Preview"><script type="math/tex">
P_{1}\ge P_{2}
</script>
</span>. 
</div>
<div class="Indented">
Any problem in either <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}
</script>
</span> or <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span> can be rephrased slightly different. We can ask “<i>how many</i> perfect matching this graph contains?” rather than “is there a perfect matching in this graph?”. When asking<i> how much</i> instead of <i>is there</i> the problem become a Counting problem. Following that, the formal definition of <span class="MathJax_Preview"><script type="math/tex">
\#\mathcal{P}
</script>
</span> is: the family the Counting version of any problem in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span>.
</div>
<div class="Indented">
Notice that for any problem <span class="MathJax_Preview"><script type="math/tex">
P\in\mathcal{P}\cup\mathcal{NP}
</script>
</span> we have <span class="MathJax_Preview"><script type="math/tex">
P\le\#P
</script>
</span> because having a count of the desired property can, in particular, distinguish between the cases that this count is <span class="MathJax_Preview"><script type="math/tex">
0
</script>
</span> or higher, that is, decide whether that property exists or not. Hence, we can safely state the general hierarchy, that <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}\le\mathcal{NP}\le\#\mathcal{P}
</script>
</span>.
</div>
<div class="Indented">
Let <span class="MathJax_Preview"><script type="math/tex">
P_{1},P_{2}
</script>
</span> be the <i>Perfect Matching</i> and <i>Hamiltonian Cycle</i> problems respectively, and let <span class="MathJax_Preview"><script type="math/tex">
\#P_{1},\#P_{2}
</script>
</span> be their Counting versions. It is interesting to see whether the hierarchy between problems from <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span> preserved for their Counting versions as well. For instance, for the problems above, we know that <span class="MathJax_Preview"><script type="math/tex">
P_{1}\le P_{2}
</script>
</span> since <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}\subseteq\mathcal{NP}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
P_{2}
</script>
</span> is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span>-complete, is that implies also <span class="MathJax_Preview"><script type="math/tex">
\#P_{1}\le\#P_{2}
</script>
</span>?
</div>
<div class="Indented">
The short answer is no. In the next section we will show that in a detailed discussion.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Counting Makes Problems Much Harder
</h1>
<div class="Unindented">
We start with two basic definitions from the field of boolean logic.
</div>
<div class="Definition">
A <i>Conjunctive Normal Form</i> (<span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span>) formula <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> other a set of <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> boolean variables is defined as <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\phi=\bigwedge_{i=1}^{m}C_{i}

</script>
</span>
where each of its <span class="MathJax_Preview"><script type="math/tex">
m
</script>
</span> clauses is of the form <span class="MathJax_Preview"><script type="math/tex">
C_{i}=\bigvee_{j=1}^{\left|C_{i}\right|}\ell_{i,j}
</script>
</span>, that is, the clause is a disjunction of some number of literals. A literal <span class="MathJax_Preview"><script type="math/tex">
\ell_{i,j}
</script>
</span> is a variable or its negation. Note that since there are <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> variables then <span class="MathJax_Preview"><script type="math/tex">
\left|C_{i}\right|\le2n
</script>
</span>.
</div>
<div class="Unindented">

</div>
<div class="Definition">
A <i>Disjunctive Normal Form</i> (<span class="MathJax_Preview"><script type="math/tex">
DNF
</script>
</span>) formula <span class="MathJax_Preview"><script type="math/tex">
\psi
</script>
</span> other a set of <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> boolean variables is defined analogously to the <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> version, as <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\psi=\bigvee_{i=1}^{m}C_{i}

</script>
</span>
where each of its <span class="MathJax_Preview"><script type="math/tex">
m
</script>
</span> clauses is of the form <span class="MathJax_Preview"><script type="math/tex">
C_{i}=\bigwedge_{j=1}^{\left|C_{i}\right|}\ell_{i,j}
</script>
</span>.
</div>
<div class="Unindented">
The <i>CNF-SAT</i> problem, gets as an input a <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> boolean formula, and decides whether that formula has a <i>satisfying assignment (s.a.)</i> or not. That is, an assignment of <span class="MathJax_Preview"><script type="math/tex">
True/False
</script>
</span> for each of its variables such that the formula evaluates to <span class="MathJax_Preview"><script type="math/tex">
True
</script>
</span>. The <i>DNF-SAT</i> is defined the same but for <span class="MathJax_Preview"><script type="math/tex">
DNF
</script>
</span> formulas rather than <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> ones.
</div>
<div class="Indented">
We will need the following facts in the coming discussion:
</div>
<ol>
<li>
The <span class="MathJax_Preview"><script type="math/tex">
\neg
</script>
</span> operator (negation) transforms any <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> formula <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> to a <span class="MathJax_Preview"><script type="math/tex">
DNF
</script>
</span> formula and vice versa. 
</li>
<li>
For <i>any</i> boolean formula <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> over <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> variables (regardless its form), there are exactly <span class="MathJax_Preview"><script type="math/tex">
2^{n}
</script>
</span> possible assignments each of which satisfy exactly one from <span class="MathJax_Preview"><script type="math/tex">
\left\{ \phi,\neg\phi\right\} 
</script>
</span>. 
</li>
</ol>
<div class="Unindented">

</div>
<div class="Indented">
It is very easy to solve <i>DNF-SAT</i> efficiently, meaning, in polynomial time. Given a <span class="MathJax_Preview"><script type="math/tex">
DNF
</script>
</span> formula <span class="MathJax_Preview"><script type="math/tex">
\psi
</script>
</span>, iterate over its clauses until you find a satisfiable clause then declare that <span class="MathJax_Preview"><script type="math/tex">
\psi
</script>
</span> is satisfiable, however, if you failed to find such a clause then declare that <span class="MathJax_Preview"><script type="math/tex">
\psi
</script>
</span> is not satisfiable. Observe that any clause of the form <span class="MathJax_Preview"><script type="math/tex">
C_{i}=\bigwedge_{j=1}^{\left|C_{i}\right|}\ell_{i,j}
</script>
</span> is satisfiable if and only if it does not contains a variable and its negation; which we can check with a single pass over the literals of <span class="MathJax_Preview"><script type="math/tex">
C_{i}
</script>
</span>. As a result, <span class="MathJax_Preview"><script type="math/tex">
DNFSAT\in\mathcal{P}
</script>
</span>.
</div>
<div class="Indented">
In contrast to <i>DNF-SAT</i>, the <i>CNF-SAT</i> is know to be <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span>-complete, so we <i>believe</i> that it cannot be solved in polynomial time. Similar to the claim from the previous section, we conclude that <i>CNF-SAT</i> is “at least harder” as <i>DNF-SAT</i>, that is, <span class="MathJax_Preview"><script type="math/tex">
DNFSAT\le CNFSAT
</script>
</span>. However, as regard to the Counting versions of those problems, we can show the opposite inequality; that <span class="MathJax_Preview"><script type="math/tex">
\#CNFSAT\le\#DNFSAT
</script>
</span>. 
</div>
<div class="Indented">
To prove that, we need to show that if we could solve <span class="MathJax_Preview"><script type="math/tex">
\#DNFSAT
</script>
</span> efficiently, then we could solve <span class="MathJax_Preview"><script type="math/tex">
\#CNFSAT
</script>
</span> efficiently. So suppose we have a magical algorithm that can solve <span class="MathJax_Preview"><script type="math/tex">
\#DNFSAT
</script>
</span> in polynomial time. Given a <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> formula <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> over <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> boolean variables, construct <span class="MathJax_Preview"><script type="math/tex">
\psi=\neg\phi
</script>
</span> and feed it to the magical algorithm. Let its output be the number <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>, then return <span class="MathJax_Preview"><script type="math/tex">
2^{n}-t
</script>
</span>. 
</div>
<div class="Indented">
The correctness of that reduction follows from the two facts we stated earlier. Note that also all the procedures can be executed in polynomial time, so we solved <span class="MathJax_Preview"><script type="math/tex">
\#CNFSAT
</script>
</span> in polynomial time using an efficient solver for <span class="MathJax_Preview"><script type="math/tex">
\#DNFSAT
</script>
</span>, therefore, <span class="MathJax_Preview"><script type="math/tex">
\#CNFSAT\le\#DNFSAT
</script>
</span>.
</div>
<div class="Indented">
Actually, if we will look closely on that reduction, we will see that it should work perfectly for the other direction as well, that is, solving <span class="MathJax_Preview"><script type="math/tex">
\#DNFSAT
</script>
</span> by a an efficient algorithm for the <span class="MathJax_Preview"><script type="math/tex">
\#CNFSAT
</script>
</span>, so we may conclude that both problems are <b>equally hard</b>. This is an evidence that the hierarchy between decision problems do not preserved for their Counting versions. Putting it differently, we can say that Counting makes problems much harder, such that even an “easy” problems from <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}
</script>
</span> become harder as the entire <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span> when you consider its Counting version.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> <span class="MathJax_Preview"><script type="math/tex">
\#DNF
</script>
</span> is Also <span class="MathJax_Preview"><script type="math/tex">
\#\mathcal{P}
</script>
</span>-Complete
</h1>
<div class="Unindented">
Since <span class="MathJax_Preview"><script type="math/tex">
\#DNF
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\#CNF
</script>
</span> are equally hard, it is enough to show that <span class="MathJax_Preview"><script type="math/tex">
\#CNF
</script>
</span> is <span class="MathJax_Preview"><script type="math/tex">
\#\mathcal{P}
</script>
</span>-complete; for that we need a reduction from any problem in <span class="MathJax_Preview"><script type="math/tex">
\#\mathcal{P}
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
\#CNF
</script>
</span>. Recall the Cook-Levin reduction which proves that <i>CNF-SAT</i> is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{NP}
</script>
</span>-complete. That reduction can be applied to any <span class="MathJax_Preview"><script type="math/tex">
P\in\mathcal{NP}
</script>
</span> and produce a polynomial procedure that converts any input <span class="MathJax_Preview"><script type="math/tex">
x
</script>
</span> for the original problem <span class="MathJax_Preview"><script type="math/tex">
P
</script>
</span>, to a <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> formula <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> for the problem <i>CNF-SAT</i>, such that <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> is satisfiable if and only if the decision problem <span class="MathJax_Preview"><script type="math/tex">
P
</script>
</span> says <span class="MathJax_Preview"><script type="math/tex">
Yes
</script>
</span> for the original input <span class="MathJax_Preview"><script type="math/tex">
x
</script>
</span>. That way, any efficient solver for <i>CNF-SAT</i> solves <span class="MathJax_Preview"><script type="math/tex">
P
</script>
</span> efficiently, so <span class="MathJax_Preview"><script type="math/tex">
P\le CNFSAT
</script>
</span>. 
</div>
<div class="Indented">
An important property of the Cook-Levin reduction is that it keeps the number of s.a. for <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> <u>exactly</u> as the number of witnesses for the original input <span class="MathJax_Preview"><script type="math/tex">
x
</script>
</span> of <span class="MathJax_Preview"><script type="math/tex">
P
</script>
</span>. That means, that we can use the same reduction to convert any Counting problem <span class="MathJax_Preview"><script type="math/tex">
\#P\in\#\mathcal{P}
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
\#CNF
</script>
</span>. That is, the reduction will yields a procedure to convert any input <span class="MathJax_Preview"><script type="math/tex">
x
</script>
</span> for <span class="MathJax_Preview"><script type="math/tex">
\#P
</script>
</span> to a formula <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> for <span class="MathJax_Preview"><script type="math/tex">
\#CNF
</script>
</span> such that the number of s.a. of <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> is the solution for <span class="MathJax_Preview"><script type="math/tex">
\#P\left(x\right)
</script>
</span>. As a result, <span class="MathJax_Preview"><script type="math/tex">
\#CNF
</script>
</span> is <span class="MathJax_Preview"><script type="math/tex">
\#\mathcal{P}
</script>
</span>-complete, and then also <span class="MathJax_Preview"><script type="math/tex">
\#DNF
</script>
</span>.
</div>
<div class="Indented">
Following the conclusion from previous section, we can say further that even a simple problem from <span class="MathJax_Preview"><script type="math/tex">
\mathcal{P}
</script>
</span>, can have a Counting version that is not just “harder” but harder as <i>any other problem</i> in <span class="MathJax_Preview"><script type="math/tex">
\#\mathcal{P}
</script>
</span>.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> Is There a Structure inside <span class="MathJax_Preview"><script type="math/tex">
\#\mathcal{P}
</script>
</span>?
</h1>
<div class="Unindented">
Going back to the <span class="MathJax_Preview"><script type="math/tex">
\#CNF
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\#DNF
</script>
</span> problems, we saw that they are related via the relation <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\#CNF\left(\phi\right)+\#DNF\left(\neg\phi\right)=2^{n}

</script>
</span>
 for any <span class="MathJax_Preview"><script type="math/tex">
CNF
</script>
</span> formula <span class="MathJax_Preview"><script type="math/tex">
\phi
</script>
</span> over <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> boolean variables. We exploited that relation to show that the problems are equally “hard” when talking about <b>exact-solutions</b>. However, an hierarchy emerged once we moved to consider<b> approximated solutions</b> for those problems. Therefore, there is some kind of structure for the family <span class="MathJax_Preview"><script type="math/tex">
\#\mathcal{P}
</script>
</span> under the appropriate notion of “solving” the problems.
</div>
<div class="Indented">
Next post we will discuss an approximation algorithm for <span class="MathJax_Preview"><script type="math/tex">
\#DNF
</script>
</span> and the hardness of approximating <span class="MathJax_Preview"><script type="math/tex">
\#CNF
</script>
</span>, showing that the additive relation from above it not enough for constructing an approximation algorithm for <span class="MathJax_Preview"><script type="math/tex">
\#CNF
</script>
</span> based on approximation for <span class="MathJax_Preview"><script type="math/tex">
\#DNF
</script>
</span>.
</div>
