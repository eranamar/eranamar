---
layout: post
author: Aviv Netanyahu
title:  Visualizing high-dimensional data with t-SNE
date:   2017-06-11
comments: true
tags: guest_author visualization dimensionality_reduction
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
<b>Guest author:</b> thanks to Aviv Netanyahu for writing this post!
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-1">1</a> What is visualization and why do we need it?
</h1>
<div class="Unindented">
Before working on a dataset we would like to understand it. After working on it, we want to communicate our conclusions with others easily. When dealing with high-dimensional data, it’s hard to get a ’feel’ for the data by just looking at numbers, and we will advance pretty slowly. For example, common visualization methods are histograms and scatter plots, which capture only one or two parameters at a time, meaning we would have to look at many histograms (one for each feature). However, by visualizing our data we may instantly be able to recognize structure. In order to do this we would like to find a representation of our data in <span class="MathJax_Preview"><script type="math/tex">
2
</script>
</span>D or <span class="MathJax_Preview"><script type="math/tex">
3
</script>
</span>D.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Our goal
</h1>
<div class="Unindented">
Find an embedding of high-dimensional data to 2D or 3D (for simplicity, will assume <span class="MathJax_Preview"><script type="math/tex">
2
</script>
</span>D from now on). <br>
Formally, we want to learn <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\{\mathbf{x}_{1},...,\mathbf{x}_{N}\}\to\{\mathbf{y}_{1},...,\mathbf{y}_{N}\}

</script>
</span>
 (<span class="MathJax_Preview"><script type="math/tex">
\{\mathbf{x}_{1},...,\mathbf{x}_{N}\}\subseteq\mathbb{R}^{n}
</script>
</span> is our data, <span class="MathJax_Preview"><script type="math/tex">
\{\mathbf{y}_{1},...,\mathbf{y}_{N}\}\subseteq\mathbb{R}^{2}
</script>
</span> is the embedding) such that the structure of the original data is preserved after the embedding.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> So why not PCA?
</h1>
<div class="Unindented">
PCA projects high-dimensional data onto a lower dimension determined by the directions where the data varies most in. The result is a linear projection which focuses on preserving distance between <b>dissimilar</b> data points. This is problematic:
</div>
<ol>
<li>
Linear sometimes isn’t good enough.
</li>
<li>
We claim structure is mostly determined by <b>similar</b> data points.
</li>
</ol>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> SNE (Hinton&amp;Roweis)
</h1>
<div class="Unindented">
This algorithm for high-dimensional data visualization focuses on preserving high-dimensional ’neighbors’ in low-dimension.
</div>
<div class="Indented">
Assumption - ’neighbors’ in high dimension have small Euclidean distance. SNE converts the Euclidean distances to similarities using Gaussian distribution:<span class="MathJax_Preview">
<script type="math/tex;mode=display">

p_{j|i}=\frac{\exp\left(-||\mathbf{x}_{i}-\mathbf{x}_{j}||^{2}/2\sigma_{i}^{2}\right)}{\sum\limits _{k\neq i}\exp\left(-||\mathbf{x}_{i}-\mathbf{x}_{k}||^{2}/2\sigma_{i}^{2}\right)}

</script>
</span>
<span class="MathJax_Preview"><script type="math/tex">
p_{j|i}
</script>
</span> is the probability that <span class="MathJax_Preview"><script type="math/tex">
j
</script>
</span> is the neighbor of <span class="MathJax_Preview"><script type="math/tex">
i
</script>
</span> in high-dimension.<span class="MathJax_Preview">
<script type="math/tex;mode=display">

q_{j|i}=\frac{\exp\left(-||\mathbf{y}_{i}-\mathbf{y}_{j}||^{2}\right)}{\sum\limits _{k\neq i}\exp\left(-||\mathbf{y}_{i}-\mathbf{y}_{k}||^{2}\right)}

</script>
</span>
<span class="MathJax_Preview"><script type="math/tex">
q_{j|i}
</script>
</span> is the probability that <span class="MathJax_Preview"><script type="math/tex">
j
</script>
</span> is the neighbor of <span class="MathJax_Preview"><script type="math/tex">
i
</script>
</span> in low-dimension. <br>
Note - choosing <span class="MathJax_Preview"><script type="math/tex">
\sigma_{i}
</script>
</span> for <span class="MathJax_Preview"><script type="math/tex">
p_{j|i}
</script>
</span> is done by determining the perplexity, i.e. the effective number of neighbors for each data point. Then <span class="MathJax_Preview"><script type="math/tex">
\forall i
</script>
</span>, <span class="MathJax_Preview"><script type="math/tex">
\sigma_{i}
</script>
</span> is chosen such that <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}_{i}
</script>
</span> has according number of neighbors in the multivariate Gaussian with mean <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}_{i}
</script>
</span> and covariance matrix <span class="MathJax_Preview"><script type="math/tex">
\sigma_{i}\cdot I
</script>
</span>. <br>
<br>
By mapping each <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}_{i}\in\mathbb{R}^{n}
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
\mathbf{y}_{i}\in\mathbb{R}^{2}
</script>
</span> such that <span class="MathJax_Preview"><script type="math/tex">
\forall i,j,\,p_{j|i}
</script>
</span> is close as possible to <span class="MathJax_Preview"><script type="math/tex">
q_{j|i}
</script>
</span>, we can preserve similarities. The algorithm finds such a mapping by <b>gradient descent</b> w.r.t. the following loss (cost) function:<span class="MathJax_Preview">
<script type="math/tex;mode=display">

C=\sum_{i}KL(P_{i}||Q_{i})=\sum_{i}\sum_{j}p_{j|i}\log\frac{p_{j|i}}{q_{j|i}}

</script>
</span>
where <span class="MathJax_Preview"><script type="math/tex">
P_{i}=(p_{1|i},...,p_{N|i}),\,Q_{i}=(q_{1|i},...,q_{N|i})
</script>
</span> and the initialization of low-dimensional data is random around the origin. 
</div>
<div class="Indented">
Notice the loss function, Kullback–Leibler (KL) divergence, is a natural way to measure similarity between two distributions.<br>
Also notice that this loss puts more emphasis on preserving the local structure of the data. KL-div is asymmetric, so different kinds of errors have a different affect:
</div>
<ul>
<li>
Type 1 error: <span class="MathJax_Preview"><script type="math/tex">
p_{j|i}
</script>
</span> is <b>large</b> (i.e. two points were originally close) and <span class="MathJax_Preview"><script type="math/tex">
q_{j|i}
</script>
</span> <b>small</b> (i.e. these points were mapped too far), we pay a <b>large</b> penalty.
</li>
<li>
Type 2 error: <span class="MathJax_Preview"><script type="math/tex">
p_{j|i}
</script>
</span> is <b>small</b> (i.e. two points were originally far away) and <span class="MathJax_Preview"><script type="math/tex">
q_{j|i}
</script>
</span> <b>large</b> (i.e. these points were mapped too close), we pay a <b>small</b> penalty.
</li>
</ul>
<div class="Unindented">
This is good, as structure is preserved better by focusing on keeping close points together. This is easy to see in the following example: think of “unrolling” this swiss roll (like unrolling a carpet). Close points will stay within the same distance, far points won’t. This is true for many manifolds (spaces that locally resemble Euclidean space near each point) besides the swiss roll.
</div>
<div class="Indented">
<div class="Frameless" style="width: 100%;">
<div class="center">
<img alt="figure swiss_roll.png" class="embedded" src="/site/assets/imgs/2017-06-11-t-SNE/swiss_roll.png" style="width: 336px; max-width: 672px; height: 244px; max-height: 489px;">
</div>
</div>
</div>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-4.1">4.1</a> The crowding problem
</h2>
<div class="Unindented">
The main problem with SNE - mapped points tend to “crowd” together, like in this example: images of 5 MNIST digits mapped with SNE to 2D and then marked by labels
</div>
<div class="Indented">
<div class="Frameless" style="width: 100%;">
<div class="center">
<img alt="figure crowding.jpg" class="embedded" src="/site/assets/imgs/2017-06-11-t-SNE/crowding.jpg" style="width: 237px; max-width: 475px; height: 230px; max-height: 461px;">
</div>
</div>
</div>
<div class="Indented">
This happens because when reducing the dimensionality we need more “space” in which to represent our data-points. You can think of a sphere flattened to a circle where all distances are preserved (as much as possible) - the radius of the circle will have to be larger than the radius of the sphere!
</div>
<div class="Indented">
On one hand this causes type <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span> error (close points mapped too far), which forces far mapped points to come closer together. On the other, causes type <span class="MathJax_Preview"><script type="math/tex">
2
</script>
</span> error (far points mapped close) which forces them to move away from each other. Type <span class="MathJax_Preview"><script type="math/tex">
2
</script>
</span> error makes us pay a small price, however if many points cause error <span class="MathJax_Preview"><script type="math/tex">
2
</script>
</span>, the price will be higher, forcing the points to indeed move. What we just described causes the mapped points to correct themselves to the center such that they end up very close to each other. Consider the MNIST example above. If we did not know the labels we couldn’t make much sense of the mapping.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-5">5</a> T-SNE (van der Maaten&amp;Hinton)
</h1>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-5.1">5.1</a> Solving the crowding problem
</h2>
<div class="Unindented">
Instead of mapped points correcting themselves to minimize the loss, we can do this by increasing <span class="MathJax_Preview"><script type="math/tex">
q_{j|i}
</script>
</span> in advance. Recall, the loss is: <span class="MathJax_Preview"><script type="math/tex">
\sum\limits _{i}\sum\limits _{j}p_{j|i}\log\frac{p_{j|i}}{q_{j|i}}
</script>
</span>, thus larger <span class="MathJax_Preview"><script type="math/tex">
q_{j|i}
</script>
</span> minimizes it. We do this by defining: <span class="MathJax_Preview">
<script type="math/tex;mode=display">

q_{ij}=\frac{(1+||\mathbf{y}_{i}-\mathbf{y}_{j}||)^{-1}}{\sum\limits _{k\neq l}(1+||\mathbf{y}_{k}-\mathbf{y}_{l}||)^{-1}}

</script>
</span>
using Cauchy distribution instead of Gaussian distribution. This increases <span class="MathJax_Preview"><script type="math/tex">
q_{j|i}
</script>
</span> since Cauchy distribution has a heavier tail than Gaussian distribution:
</div>
<div class="Indented">
<div class="Frameless" style="width: 100%;">
<div class="center">
<img alt="figure cauchy.jpg" class="embedded" src="/site/assets/imgs/2017-06-11-t-SNE/cauchy.jpg" style="width: 398px; max-width: 797px; height: 274px; max-height: 548px;">
</div>
</div>
(Hence the name of the algorithm - Cauchy distribution is a T-student distribution with <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span> degree of freedom).
</div>
<div class="Indented">
Note also that we changed the probability from conditional to joint probability (this was done for technical optimization reasons). For the same reason, <span class="MathJax_Preview"><script type="math/tex">
p_{j|i}
</script>
</span> was converted to <span class="MathJax_Preview"><script type="math/tex">
p_{ij}=\frac{p_{j|i}+p_{i|j}}{2N}
</script>
</span>. We can still use Gaussian distribution for the original data since we are trying to compensate for error in the <b>mapped</b> space.
</div>
<div class="Indented">
Now the performance on MNIST is much better:
</div>
<div class="Indented">
<div class="Frameless" style="width: 100%;">
<div class="center">
<img alt="figure mnist.png" class="embedded" src="/site/assets/imgs/2017-06-11-t-SNE/mnist.png" style="width: 378px; max-width: 757px; height: 214px; max-height: 429px;">
</div>
</div>
</div>
<div class="Indented">
It’s obvious that global structure is preserved and crowding is resolved. Notice also that when zooming in on the mapping of <span class="MathJax_Preview"><script type="math/tex">
1
</script>
</span> digits, the local structure is preserved as well - digits with same orientation were mapped close to each other.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-6">6</a> Barnes-Hut approximation (van der Maaten)
</h1>
<div class="Unindented">
Computing the gradient for each mapped point each iteration is very costly, especially when the dataset is very large. At each iteration, <span class="MathJax_Preview"><script type="math/tex">
\forall i
</script>
</span>, we compute:<span class="MathJax_Preview">
<script type="math/tex;mode=display">

\frac{\partial C}{\partial\mathbf{y}_{i}}=4\sum_{j\neq i}\frac{(p_{ij}-q_{ij})}{1+||\mathbf{y}_{i}-\mathbf{y}_{j}||^{2}}\cdot(\mathbf{y}_{i}-\mathbf{y}_{j})

</script>
</span>
We can think of <span class="MathJax_Preview"><script type="math/tex">
\mathbf{y}_{i}-\mathbf{y}_{j}
</script>
</span> as a spring between these two points (it might be too tight/loose depending on if the points were mapped to far/close accordingly). The fraction can be thought of as exertion/compression of the spring, in other words the correction we do in this iteration. We calculate this for <span class="MathJax_Preview"><script type="math/tex">
N
</script>
</span> mapped points, and derive <span class="MathJax_Preview"><script type="math/tex">
C
</script>
</span> by all data-points, so for each iteration we have <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}(N^{2})
</script>
</span> computations. This limits us to datasets with only few thousands of points! However, we can reduce the computation time to <span class="MathJax_Preview"><script type="math/tex">
\mathcal{O}(N\log N)
</script>
</span> by reducing the number of computations per iteration. The main idea - forces exerted by a group of points on a point that is relatively far away are all very similar. Therefore we can calculate <span class="MathJax_Preview"><script type="math/tex">
\mathbf{y}_{i}-\mathbf{y}_{j^{*}}
</script>
</span>where <span class="MathJax_Preview"><script type="math/tex">
\mathbf{y}_{j^{*}}
</script>
</span> is the average of the group of far away points, instead of <span class="MathJax_Preview"><script type="math/tex">
\mathbf{y}_{i}-\mathbf{y}_{j}
</script>
</span> for each <span class="MathJax_Preview"><script type="math/tex">
\mathbf{y}_{j}
</script>
</span> in the far away group.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-7">7</a> Some applications
</h1>
<ul>
<li>
Visualize word embeddings - <a class="URL" href="http://nlp.yvespeirsman.be/blog/visualizing-word-embeddings-with-tsne/">http://nlp.yvespeirsman.be/blog/visualizing-word-embeddings-with-tsne/</a>
</li>
<li>
Multiple maps word embeddings - <a class="URL" href="http://homepage.tudelft.nl/19j49/multiplemaps/Multiple_maps_t-SNE/Multiple_maps_t-SNE.html">http://homepage.tudelft.nl/19j49/multiplemaps/Multiple_maps_t-SNE/Multiple_maps_t-SNE.html</a>
</li>
<li>
Visualization of genetic disease phenotype similarities by multiple maps t-SNE - <a class="URL" href="https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4243097/">https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4243097/</a>
</li>
</ul>
<h1 class="Section">
<a class="toc" name="toc-Section-8">8</a> When can t-SNE fail?
</h1>
<div class="Unindented">
When the data is noisy or on a highly varying manifold, the local linearity assumption we make (by converting Euclidean distances to similarities) does not hold. What can we do? - smooth data with PCA (in case of noise) or use Autoencoders (in case of highly varying manifold).
</div>
