---
layout: post
author: Eran Amar
title:  Recurrent Neural Network - Dropout for LSTMs
date:   2017-05-12
comments: true
tags: neural_networks
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
<a class="toc" name="toc-Section-1">1</a> Recap
</h1>
<div class="Unindented">
This is the third post in the series about Recurrent Neural Networks. The <a class="URL" href="https://eranamar.github.io/site/2017/04/20/Recurrent-Neural-Network-Introduction.html">first post </a>defined the basic notations and the formulation of a Recurrent Cell, the <a class="URL" href="https://eranamar.github.io/site/2017/04/30/Recurrent-Neural-Network-LSTM-and-GRU.html">second post</a> discussed its extensions such as LSTM and GRU. Recall that LSTM layer receives vector <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}^{\left(t\right)}
</script>
</span> - the input for the current time step <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>, and <span class="MathJax_Preview"><script type="math/tex">
\mathbf{c}^{\left(t-1\right)},\mathbf{h}^{\left(t-1\right)}
</script>
</span> the memory-vector and the output-vector from previous time-step respectively, then the layer computes a new memory vector <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{c}^{\left(t\right)}=\mathbf{f}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\circ\mathbf{c}^{\left(t-1\right)}+\mathbf{i}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\circ\mathbf{s}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)

</script>
</span>
where <span class="MathJax_Preview"><script type="math/tex">
\circ
</script>
</span> denotes element wise multiplication. Then the layer outputs <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{h}^{\left(t\right)}=\mathbf{o}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\circ tanh\left(\mathbf{c}^{\left(t\right)}\right)

</script>
</span>
where <span class="MathJax_Preview"><script type="math/tex">
\mathbf{f},\mathbf{i},\mathbf{o}\in\mathcal{S}_{sigmoid}
</script>
</span> are the <i>forget</i>, <i>input</i> and <i>output</i> gates respectively, and <span class="MathJax_Preview"><script type="math/tex">
\mathbf{s}\in\mathcal{S}_{tanh}
</script>
</span> is the state transition function. <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}_{\sigma}
</script>
</span> denotes a family of all affine transformations <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}^{n}\times\mathbb{R}^{n}\to\mathbb{R}^{m}
</script>
</span> following by an element-wise activation function <span class="MathJax_Preview"><script type="math/tex">
\sigma
</script>
</span>, for any input-dimension <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> and output-dimension <span class="MathJax_Preview"><script type="math/tex">
m
</script>
</span>.
</div>
<div class="Indented">
In this post, we will focus on the different variants of dropout regularization that are being used in LSTM networks. 
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Dropout Regularization
</h1>
<div class="Unindented">
Dropout is a very popular regularization mechanism that is being attached to layers of a neural network in order to reduce overfitting and improve generalization. Applying dropout to a layer starts by fixing some <span class="MathJax_Preview"><script type="math/tex">
p\in\left(0,1\right)
</script>
</span>. Then, any time that layer produces an output during training, each one of its neurons is being zeroed-out independently with probability <span class="MathJax_Preview"><script type="math/tex">
p
</script>
</span>, and with probability <span class="MathJax_Preview"><script type="math/tex">
1-p
</script>
</span> it is being scaled by <span class="MathJax_Preview"><script type="math/tex">
\frac{1}{1-p}
</script>
</span>. Scaling the output is important because it keeps the expected output-value of the neuron unchanged. During test time, the dropout mechanism is being turned-off, that is, the output of each neuron is being passed as is. 
</div>
<div class="Indented">
For fully-connected layers, we can formulate dropout as follows: Suppose that <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}
</script>
</span> should be the output of some layer that has dropout mechanism. Generate a mask <span class="MathJax_Preview"><script type="math/tex">
\mathbf{z}\in\left\{ 0,1\right\} ^{n}
</script>
</span>, by picking each coordinate i.i.d w.p. <span class="MathJax_Preview"><script type="math/tex">
p
</script>
</span> from <span class="MathJax_Preview"><script type="math/tex">
\left\{ 0,1\right\} 
</script>
</span>, and output <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}\circ\mathbf{z}
</script>
</span> instead of <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}
</script>
</span>. Note that we hide here the output-scaling for simplicity, and that the mask <span class="MathJax_Preview"><script type="math/tex">
\mathbf{z}
</script>
</span> is being generated any time the layer required to produce an output. 
</div>
<div class="Indented">
At first glance it seems trivial to apply dropout to LSTM layers. However, when we come to implement the mechanism we encounter some technical issues that should be addressed. For instance, we need to decide whether to apply the given mask only to the input <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}^{\left(t\right)}
</script>
</span> or also to the recurrent connection (i.e. to <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t-1\right)}
</script>
</span>), and if we choose to apply on both, should it be the same mask or different masks? Should a mask be shared <i>across time</i> or be generated for each time-step?
</div>
<div class="Indented">
Several works tried to apply dropout to LSTM layers in several naive ways but without success. It seems that just dropping randomly some coordinates from the recurrent connections <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t-1\right)}
</script>
</span> impair the ability of the LSTM layer to learn long\short term dependencies and does not improve generalization. In the next section we will review some works that applied dropout to LSTMs in a way that successfully yields better generalization. 
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> Variants
</h1>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-3.1">3.1</a> Mask Only Inputs; Regenerate Masks
</h2>
<div class="Unindented">
Wojciech Zaremba, Ilya Sutskever and Oriol Vinyals published in 2014 a <a class="URL" href="https://arxiv.org/abs/1409.2329v5">paper</a> that describe a successful dropout variation for LSTM layers. Their idea was that dropout should be applied only to the inputs of the layer and not to the recurrent connections. Moreover, a new mask should be generated for each time step. 
</div>
<div class="Indented">
Formally, for each time-step <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>, generate a mask <span class="MathJax_Preview"><script type="math/tex">
\mathbf{z}^{\left(t\right)}
</script>
</span> and compute <span class="MathJax_Preview"><script type="math/tex">
\mathbf{\hat{x}}^{\left(t\right)}:=\mathbf{x}^{\left(t\right)}\circ\mathbf{z}^{\left(t\right)}
</script>
</span>. Then continue to compute the LSTM layer as usual but use <span class="MathJax_Preview"><script type="math/tex">
\mathbf{\hat{x}}^{\left(t\right)}
</script>
</span> as the input to the layer rather than <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}^{\left(t\right)}
</script>
</span>. 
</div>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-3.2">3.2</a> rnnDrop: Mask Only the Memory; Fixed Mask
</h2>
<div class="Unindented">
On 2015, Taesup Moon, Heeyoul Choi, Hoshik Lee and Inchul Song <a class="URL" href="https://www.stat.berkeley.edu/~tsmoon/files/Conference/asru2015.pdf">published</a> a different dropout variation: <i>rnnDrop</i>. 
</div>
<div class="Indented">
They suggested to generate a mask for each training sequence and fix it for all the time-steps in that sequence, that is, the mask is being <i>shared across time</i>. The mask is then being applied to the <i>memory vector</i> of the layer rather than the input. In their formulation, only the second formula of the LSTM changed: <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{h}^{\left(t\right)}=\mathbf{o}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\circ tanh\left(\mathbf{c}^{\left(t\right)}\circ\mathbf{z}\right)

</script>
</span>
where <span class="MathJax_Preview"><script type="math/tex">
\mathbf{z}
</script>
</span> if the fixed mask to the entire current training sequence.
</div>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-3.3">3.3</a> Mask Input and Hidden; Fixed Mask
</h2>
<div class="Unindented">
A relatively <a class="URL" href="https://arxiv.org/abs/1512.05287">recent work</a> of Yarin Gal and Zoubin Ghahramani from 2016 also use a make that is shared across time, however it is being applied to the inputs as well on the <i>recurrent connections</i>. This is one of the first successful dropout variants that actually apply the mask to the recurrent connection. 
</div>
<div class="Indented">
Formally, for each training sequence generate two masks <span class="MathJax_Preview"><script type="math/tex">
\mathbf{z}_{1},\mathbf{z}_{2}
</script>
</span> and compute <span class="MathJax_Preview"><script type="math/tex">
\mathbf{\hat{x}}^{\left(t\right)}:=\mathbf{x}^{\left(t\right)}\circ\mathbf{z}_{1}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\mathbf{\hat{h}}^{\left(t-1\right)}:=\mathbf{h}^{\left(t-1\right)}\circ\mathbf{z}_{2}
</script>
</span>. Then use <span class="MathJax_Preview"><script type="math/tex">
\mathbf{\hat{x}}^{\left(t\right)},\mathbf{\hat{h}}^{\left(t-1\right)}
</script>
</span> as the input to the “regular” LSTM layer.
</div>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-3.4">3.4</a> Mask Gates; Fixed Mask
</h2>
<div class="Unindented">
Another <a class="URL" href="https://arxiv.org/abs/1603.05118">paper</a> from 2016, of Stanislau Semeniuta, Aliaksei Severyn and Erhardt Barth demonstrate the mask being applied to some of the <i>gates</i> rather than the input\hidden vectors. For any time-step, generate <span class="MathJax_Preview"><script type="math/tex">
\mathbf{z}^{\left(t\right)}
</script>
</span> and then use it to mask the <i>input gate</i><span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{c}^{\left(t\right)}=\mathbf{f}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\circ\mathbf{c}^{\left(t-1\right)}+\mathbf{z}^{\left(t\right)}\circ\mathbf{i}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\circ\mathbf{s}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)

</script>
</span>
and the second LSTM equation left unchanged. 
</div>
<div class="Indented">
Small note, the authors also addressed in their paper some issues related to scaling the not-dropped coordinates, which won’t be covered here.
</div>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-3.5">3.5</a> Zoneout
</h2>
<div class="Unindented">
The very last dropout variation (to the day I wrote this post) is by <a class="URL" href="https://arxiv.org/abs/1606.01305">David Krueger et al</a>, from 2017. They suggested to treat the memory vector <span class="MathJax_Preview"><script type="math/tex">
\mathbf{c}^{\left(t\right)}
</script>
</span> and the output vector (a.k.a <i>hidden</i> vector) <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t\right)}
</script>
</span> as follows: each coordinate in each of the vectors either being updated as usual or <i>preserved its value</i> from previous time-step. As opposed to regular dropout, where “dropping a coordinate” meaning to make it zero, in zoneout it just keeps its previous value - acting as a random identity map that allows better gradients propagation through more time steps. Note that preserving a coordinate in the memory vector should not affect the computation of the hidden vector. Therefore we have to rewrite the formulas for the LSTM layer: given <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\mathbf{c}^{\left(t-1\right)}
</script>
</span>. Generate two masks <span class="MathJax_Preview"><script type="math/tex">
\mathbf{z}_{1}^{\left(t\right)},\mathbf{z}_{2}^{\left(t\right)}
</script>
</span> for the current time-step <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>. Start by computing a candidate memory-vector with the regular formula, <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{\hat{c}}^{\left(t\right)}=\mathbf{f}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\circ\mathbf{c}^{\left(t-1\right)}+\mathbf{i}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\circ\mathbf{s}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)

</script>
</span>
and then use only <i>some</i> of its coordinates to update the memory-vector, that is, <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{c}^{\left(t\right)}=\mathbf{z}_{1}^{\left(t\right)}\circ\mathbf{c}^{\left(t-1\right)}+\left(1-\mathbf{z}_{1}^{\left(t\right)}\right)\circ\mathbf{\hat{c}}^{\left(t\right)}

</script>
</span>
Similarly, for the hidden-vector, compute a candidate <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{\hat{h}}^{\left(t\right)}=\mathbf{o}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\circ tanh\left(\mathbf{\hat{c}}^{\left(t\right)}\right)

</script>
</span>
and selectively update <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{h}^{\left(t\right)}=\mathbf{z}_{2}^{\left(t\right)}\circ\mathbf{h}^{\left(t-1\right)}+\left(1-\mathbf{z}_{2}^{\left(t\right)}\right)\circ\mathbf{\hat{h}}^{\left(t\right)}

</script>
</span>
Observe again that the computation of the candidate hidden-vector is based on <span class="MathJax_Preview"><script type="math/tex">
\mathbf{\hat{c}}^{\left(t\right)}
</script>
</span> therefore unaffected by the mask <span class="MathJax_Preview"><script type="math/tex">
\mathbf{z}_{1}^{\left(t\right)}
</script>
</span>.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> More and More and More
</h1>
<div class="Unindented">
In the last year, dropout for LSTM networks gained more attention, and the list of different variants is growing quickly. This post tried to cover only <i>some</i> fraction of all the variants available out there. It is left for the curious reader to search the literature for more about this topic. 
</div>
<div class="Indented">
Note that some of the dropout variations discussed above can be applied to basic RNN and GRU cells without much modifications - please refer to the papers themselves for more details. 
</div>
<div class="Indented">
Next post, I will discuss recent advancement in the field of RNNs, such as Multiplicative LSTMs, Recurrent Batch Normalization and more.
</div>
