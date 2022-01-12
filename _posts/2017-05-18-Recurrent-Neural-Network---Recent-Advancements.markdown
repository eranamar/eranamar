---
layout: post
author: Eran Amar
title:  Recurrent Neural Network - Recent Advancements
date:   2017-05-18
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
<a class="toc" name="toc-Section-1">1</a> Last Post of the Series
</h1>
<div class="Unindented">
This post is the last in the series that discussed Recurrent Neural Networks (RNNs). The first post introduced the <a class="URL" href="https://eranamar.github.io/site/2017/04/20/Recurrent-Neural-Network-Introduction.html">basic recurrent cells and their limitations</a>, the second post presented the formulation of <a class="URL" href="https://eranamar.github.io/site/2017/04/30/Recurrent-Neural-Network-LSTM-and-GRU.html">gated cells as LSTMs and GRUs</a>, and the third post overviewed <a class="URL" href="https://eranamar.github.io/site/2017/05/12/Recurrent-Neural-Network-Dropout-for-LSTMs.html">dropout variants for LSTMs</a>. This post will present some of the recent advancements in the filed of RNNs (mostly related to LSTM units). 
</div>
<div class="Indented">
We start by reviewing some of the notations that we used during the series. 
</div>
<div class="Indented">
We defined <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}_{\sigma}
</script>
</span> to be the family of all affine transformations <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}^{n}\times\mathbb{R}^{n}\to\mathbb{R}^{m}
</script>
</span> for any dimensions <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
m
</script>
</span>, followed by an element-wise activation function <span class="MathJax_Preview"><script type="math/tex">
\sigma
</script>
</span>, that is,
</div>
<div class="Indented">
<span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathcal{S}_{\sigma}=\left\{ \mathbf{x},\mathbf{h}\mapsto\sigma\left(W\mathbf{x}+U\mathbf{h}+\mathbf{b}\right)\mid\forall W,U,\mathbf{b}\right\} 

</script>
</span>
and noted that with the <span class="MathJax_Preview"><script type="math/tex">
sigmoid\left(x\right)=\left(1+e^{-x}\right)^{-1}
</script>
</span> as the activation function, all the functions in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}_{sig}
</script>
</span> are <i>gate</i> functions, that is, their output belongs to <span class="MathJax_Preview"><script type="math/tex">
\left[0,1\right]^{m}
</script>
</span>. 
</div>
<div class="Indented">
Usually we do not specify <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
m
</script>
</span>, we assume that whenever there is a matrix multiplication of element-wise operation the dimensions always match, as they are not important for the discussion. 
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Variations of the Gate Functions for LSTMs
</h1>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-2.1">2.1</a> Recap: LSTM Formulation - The Use of Gates
</h2>
<div class="Unindented">
LSTM layer is a recurrent layer that keeps a memory vector <span class="MathJax_Preview"><script type="math/tex">
\mathbf{c}^{\left(t\right)}
</script>
</span> for any time step <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>. Upon receiving the current time-step input <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}^{\left(t\right)}
</script>
</span> and the output from previous time-step <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t-1\right)}
</script>
</span>, the layer computes gate activations <span class="MathJax_Preview"><script type="math/tex">
f^{\left(t\right)},i^{\left(t\right)},o^{\left(t\right)}
</script>
</span> by applying a the corresponding gate function <span class="MathJax_Preview"><script type="math/tex">
\mathbf{f},\mathbf{i},\mathbf{o}\in\mathcal{S}_{sig}
</script>
</span> on the tuple <span class="MathJax_Preview"><script type="math/tex">
\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)
</script>
</span>, note that these activations are vectors in <span class="MathJax_Preview"><script type="math/tex">
\left[0,1\right]^{m}
</script>
</span>. 
</div>
<div class="Indented">
A new memory-vector is then being computed by <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{c}^{\left(t\right)}=f^{\left(t\right)}\circ\mathbf{c}^{\left(t-1\right)}+i^{\left(t\right)}\circ\mathbf{s}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)

</script>
</span>
 where <span class="MathJax_Preview"><script type="math/tex">
\circ
</script>
</span> is element-wise multiplication and <span class="MathJax_Preview"><script type="math/tex">
\mathbf{s}\in\mathcal{S}_{tanh}
</script>
</span> is the state-transition function. The layer then outputs <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t\right)}=o^{\left(t\right)}\circ tanh\left(\mathbf{c}^{\left(t\right)}\right)
</script>
</span>. 
</div>
<div class="Indented">
The gate functions are of great importance for LSTM layer, they are what allow the layer to learn short and long term dependencies, and they also help avoiding the <i>exploding and vanishing gradients problem</i>.
</div>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-2.2">2.2</a> Simple Variations
</h2>
<div class="Unindented">
The more expressive power a gate function have, the better it can learn short \ long term dependencies. That is, if a gate function is very rich, it can account for subtle changes in the input <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}^{\left(t\right)}
</script>
</span> as well to subtle changes in the “past”, i.e. in <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t-1\right)}
</script>
</span>. One way to enrich a gate function is by making it depends on the previous memory vector <span class="MathJax_Preview"><script type="math/tex">
\mathbf{c}^{\left(t-1\right)}
</script>
</span> in addition to the regular tuple <span class="MathJax_Preview"><script type="math/tex">
\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)
</script>
</span>. That will redefine our gate functions family to be <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathcal{S}_{\sigma}=\left\{ \mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)},\mathbf{c}^{\left(t-1\right)}\mapsto\sigma\left(W\mathbf{x}^{\left(t\right)}+U\mathbf{h}^{\left(t-1\right)}+V\mathbf{c}^{\left(t-1\right)}+\mathbf{b}\right)\right\} 

</script>
</span>
for any <span class="MathJax_Preview"><script type="math/tex">
W,U
</script>
</span> and diagonal <span class="MathJax_Preview"><script type="math/tex">
V
</script>
</span>. Such construction can be found in <a class="URL" href="https://arxiv.org/abs/1308.0850">here</a>, note that we can think of <span class="MathJax_Preview"><script type="math/tex">
V\mathbf{c}^{\left(t-1\right)}
</script>
</span> as a bias vector that is memory-dependent, thus there are formulation in which the vector <span class="MathJax_Preview"><script type="math/tex">
\mathbf{b}
</script>
</span> is omitted. 
</div>
<h2 class="Subsection">
<a class="toc" name="toc-Subsection-2.3">2.3</a> Multiplicative LSTMs
</h2>
<div class="Unindented">
In <a class="URL" href="https://arxiv.org/abs/1609.07959">this paper</a> from October 2016, the authors took that idea one step further. They wanted to make a unique gate function for each possible input. Recall that any function in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}_{\sigma}
</script>
</span> is of the form <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\mapsto W\mathbf{x}^{\left(t\right)}+U\mathbf{h}^{\left(t-1\right)}+\mathbf{b}
</script>
</span>. For simplicity, consider the case where <span class="MathJax_Preview"><script type="math/tex">
\mathbf{b}=\mathbf{0}
</script>
</span>. 
</div>
<div class="Indented">
We end up with a sum of two components: one that depends on the current input and one that depends on the past (i.e. <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t-1\right)}
</script>
</span> which encodes information about previous time steps), and the component with larger magnitude will dominate the transition. If <span class="MathJax_Preview"><script type="math/tex">
W\mathbf{x}^{\left(t\right)}
</script>
</span> is larger, the layer will not be sensitive enough to the past, and if <span class="MathJax_Preview"><script type="math/tex">
U\mathbf{h}^{\left(t-1\right)}
</script>
</span> is larger, then the layer will not be sensitive to subtle changes in the input.
</div>
<div class="Indented">
The author noted that since in most cases the input <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}^{\left(t\right)}
</script>
</span> is an 1-hot vector, then multiply by <span class="MathJax_Preview"><script type="math/tex">
W
</script>
</span> is just selecting a specific column. So we may think of any gate function <span class="MathJax_Preview"><script type="math/tex">
\mathbf{s}\in\mathcal{S}_{\sigma}
</script>
</span> as a <b>fixed</b> base affine transformation <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}\mapsto U\mathbf{h}
</script>
</span> combined with input-dependent bias vector <span class="MathJax_Preview"><script type="math/tex">
\mathbf{b}_{\mathbf{x}^{\left(t\right)}}=W\mathbf{x}^{\left(t\right)}
</script>
</span>. That is <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{s}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)=U\mathbf{h}^{\left(t-1\right)}+\mathbf{b}_{\mathbf{x}^{\left(t\right)}}

</script>
</span>
that formula emphasis the <i>additive</i> effect of the current input-vector on the transition of <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t-1\right)}
</script>
</span>. 
</div>
<div class="Indented">
The goal in multiplicative LSTM (mLSTM), is to have an <b>unique</b> affine transformation (in our case, unique matrix <span class="MathJax_Preview"><script type="math/tex">
U
</script>
</span>) for each possible input. Obviously, if the number of possible inputs is very large, the number of the parameters will explode and it won’t be feasible to train the network. To overcome that, the authors of the paper suggested to learn <i>shared</i> intermediate matrices that will be used to construct an unique <span class="MathJax_Preview"><script type="math/tex">
U
</script>
</span> for each input, and because the factorization is shared, there are less parameters to learn. 
</div>
<div class="Indented">
The factorization is defined as follow: the unique matrix <span class="MathJax_Preview"><script type="math/tex">
U_{\mathbf{x}^{\left(t\right)}}
</script>
</span> is constructed by <span class="MathJax_Preview"><script type="math/tex">
V_{1}diag\left(T\mathbf{x}^{\left(t\right)}\right)V_{2}
</script>
</span> where <span class="MathJax_Preview"><script type="math/tex">
V_{1},V_{2}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> are intermediate matrices which are shared across all the possible inputs, and <span class="MathJax_Preview"><script type="math/tex">
diag\left(\mathbf{v}\right)
</script>
</span> is an operation that maps any vector <span class="MathJax_Preview"><script type="math/tex">
\mathbf{v}
</script>
</span> to a square diagonal matrix with the elements of <span class="MathJax_Preview"><script type="math/tex">
\mathbf{v}
</script>
</span> on its diagonal.
</div>
<div class="Indented">
Note that the target dimension of <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> may be arbitrarily large (in the paper they chose it to be the same as <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t\right)}
</script>
</span>).
</div>
<div class="Indented">
The difference between LSTM and mLSTM is only in the definition of the gate functions family, all the equations for updating the memory cell and the output are the same. In mLSTM the family <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}_{\sigma}
</script>
</span> is being replaced with <span class="MathJax_Preview">
<script type="math/tex;mode=display">
\begin{aligned}
\mathcal{S}'_{\sigma} & =\left\{ \mathbf{x},\mathbf{h}\mapsto\sigma\left(W\mathbf{x}+U_{\mathbf{x}}\mathbf{h}\right)\right\} \\
 & =\left\{ \mathbf{x},\mathbf{h}\mapsto\sigma\left(\mathbf{b}_{\mathbf{x}}+V_{1}diag\left(T\mathbf{x}\right)V_{2}\mathbf{h}\right)\mid\forall W,V_{1},V_{2},T\right\} 
\end{aligned}
</script>
</span>
Note that we can reduce the number of parameters even further by forcing all the gate functions to use the same <span class="MathJax_Preview"><script type="math/tex">
V_{2}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
T
</script>
</span> matrices. That is, each gate will be parametrized only by <span class="MathJax_Preview"><script type="math/tex">
W
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
V_{1}
</script>
</span>. 
</div>
<div class="Indented">
Formally, define the following transformation <span class="MathJax_Preview"><script type="math/tex">
\tau:\mathbb{R}^{n}\times\mathbb{R}^{n}\to\mathbb{R}^{m}
</script>
</span> such that <span class="MathJax_Preview"><script type="math/tex">
\tau\left(\mathbf{x},\mathbf{h}\right)=T\mathbf{x}\circ V_{2}\mathbf{h}
</script>
</span> and then we can define <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}'_{\sigma}
</script>
</span> for some fixed learned transformation <span class="MathJax_Preview"><script type="math/tex">
\tau
</script>
</span> as <span class="MathJax_Preview">
<script type="math/tex;mode=display">
\begin{aligned}
\mathcal{S}'_{\sigma,\tau} & =\left\{ \mathbf{x},\mathbf{h}\mapsto\sigma\left(W\mathbf{x}+V_{1}\tau\left(\mathbf{x},\mathbf{h}\right)\right)\mid\forall W,V_{1}\right\} \\
 & =\left\{ \mathbf{x},\mathbf{h}\mapsto\mathbf{s}\left(\mathbf{x},\tau\left(\mathbf{x},\mathbf{h}\right)\right)\mid\mathbf{s}\in\mathcal{S}_{\sigma}\right\} 
\end{aligned}
</script>
</span>
 in other words, mLSTM is an LSTM that apply its gates to the tuple <span class="MathJax_Preview"><script type="math/tex">
\left(\mathbf{x}^{\left(t\right)},\tau\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\right)
</script>
</span> rather than <span class="MathJax_Preview"><script type="math/tex">
\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)
</script>
</span>, and <span class="MathJax_Preview"><script type="math/tex">
\tau
</script>
</span> is another learned transformation. If you look again at the exact formula of <span class="MathJax_Preview"><script type="math/tex">
\tau
</script>
</span> you will see the <i>multiplicative</i> effect of the input vector on the transformation of <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}
</script>
</span>. That way, the authors said, <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}'_{\sigma,\tau}
</script>
</span> can yield much richer family of input-dependent transformations for <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t\right)}
</script>
</span>, which can be sensitive to the past as well as to subtle changes in the current input.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> Recurrent Batch Normalization
</h1>
<div class="Unindented">
Batch normalization is an operator applied to a layer before going through the non-linearity function in order to “normalize” the values before activation. That operator has two hyper-parameters for tuning and two statistics that it accumulates internally. Formally, given a vector of pre-activation values <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}
</script>
</span>, batch-normalization is <span class="MathJax_Preview">
<script type="math/tex;mode=display">

BN\left(\mathbf{x};\gamma,\beta\right)=\beta+\gamma\circ\frac{\mathbf{x}-\mathbb{\hat{E}\left[\mathbf{x}\right]}}{\sqrt{\hat{\mathbb{V}}\left[\mathbf{x}\right]+\epsilon}}

</script>
</span>
where <span class="MathJax_Preview"><script type="math/tex">
\hat{\mathbb{E}}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\hat{\mathbb{V}}
</script>
</span> are the empirical mean and variance of the current batch respectively. <span class="MathJax_Preview"><script type="math/tex">
\gamma,\beta
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\epsilon
</script>
</span> are vectors, and the division in the operator is being computed element-wise. Note that at inference time, the population statistics are estimated by averaging the empirical statistics across all the batches.
</div>
<div class="Indented">
<a class="URL" href="https://arxiv.org/abs/1603.09025">That paper</a> from February 2017, applied batch normalization also to the <i>recurrent connections</i> in LSTM layers, and they showed empirically that it helped the network to converge faster. The usage is simple, each gate function <span class="MathJax_Preview"><script type="math/tex">
\mathbf{i},\mathbf{f},\mathbf{o}\in\mathcal{S}_{sig}
</script>
</span> and the transition function <span class="MathJax_Preview"><script type="math/tex">
\mathbf{s}\in\mathcal{S}_{tanh}
</script>
</span> computes <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\mapsto\sigma\left(BN\left(W\mathbf{h}^{\left(t-1\right)};\gamma_{h},\beta_{h}\right)+BN\left(W\mathbf{x}^{\left(t\right)};\gamma_{x},\beta_{x}\right)+\mathbf{b}\right)

</script>
</span>
 when the hyper-parameters <span class="MathJax_Preview"><script type="math/tex">
\gamma
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\beta
</script>
</span> are shared across different gates. Then the output of the layer is <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t\right)}=o^{\left(t\right)}\circ tanh\left(BN\left(\mathbf{c}^{\left(t\right)};\gamma_{c},\beta_{c}\right)\right)
</script>
</span>. The authors suggested to set <span class="MathJax_Preview"><script type="math/tex">
\beta_{x}=\beta_{h}=\mathbf{0}
</script>
</span> because there is already a bias parameter <span class="MathJax_Preview"><script type="math/tex">
\mathbf{b}
</script>
</span> and to prevent redundancy. In addition, they said that sharing the internal <span class="MathJax_Preview"><script type="math/tex">
BN
</script>
</span> statistics across time degrades performance severely. Therefore, one should use “fresh” <span class="MathJax_Preview"><script type="math/tex">
BN
</script>
</span> operators for each time-step with their own internal statistics (but share the <span class="MathJax_Preview"><script type="math/tex">
\beta
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\gamma
</script>
</span> parameters).
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> The Ever-growing Field
</h1>
<div class="Unindented">
RNNs is a rapidly growing field and this series only covers a small part of it. There are much more advanced models, some of them are only from few months ago. If you are interested in this field, you may find very interesting stuff <a class="URL" href="https://smerity.com/articles/2016/iclr_2017_submissions.html">here</a>. 
</div>
