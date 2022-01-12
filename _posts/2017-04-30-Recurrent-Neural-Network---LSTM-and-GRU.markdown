---
layout: post
author: Eran Amar
title:  Recurrent Neural Network - LSTM and GRU
date:   2017-04-30
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
The <a class="URL" href="https://eranamar.github.io/site/2017/04/20/Recurrent-Neural-Network-Introduction.html">first post</a> in the series discussed the basic structure of recurrent cells and their limitations. We defined two families of functions, the first is <span class="MathJax_Preview"><script type="math/tex">
\mathcal{F}_{\sigma}
</script>
</span> which contains all the affine transformations <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}^{n}\to\mathbb{R}^{m}
</script>
</span> for any <span class="MathJax_Preview"><script type="math/tex">
n
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
m,
</script>
</span> followed by an element-wise activation function <span class="MathJax_Preview"><script type="math/tex">
\sigma.
</script>
</span> And another family <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}_{\sigma}
</script>
</span> which is some kind of extension of <span class="MathJax_Preview"><script type="math/tex">
\mathcal{F}_{\sigma}
</script>
</span> in the sense that the input space is <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}^{n}\times\mathbb{R}^{n}
</script>
</span> rather than <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}^{n}.
</script>
</span> Formally, the definition of <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}_{\sigma}
</script>
</span> is<span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathcal{S}_{\sigma}=\left\{ \mathbf{x},\mathbf{h}\mapsto\sigma\left(W\mathbf{x}+U\mathbf{h}+\mathbf{b}\right)\mid\forall W,U,\mathbf{b}\right\} 

</script>
</span>
In this post, which is the second in the series,  we will focus on two activation functions: <span class="MathJax_Preview"><script type="math/tex">
tanh\left(x\right)=\frac{e^{x}-e^{-x}}{e^{x}+e^{-x}}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
sig\left(x\right)=\left(1+e^{-x}\right)^{-1}
</script>
</span>. Next section describes what are <i>gate</i> functions, then we move to review two common improvements to the basic recurrent cell: the LSTM and GRU cells. 
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Gate Functions
</h1>
<div class="Unindented">
Recall that recurrent layer has two steps. The first is updating its inner state based on both the current input and the previous state vector, then producing an output by applying some other function to the new state. So the <b>input</b> to the layer at each time step <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> is actually the tuple <span class="MathJax_Preview"><script type="math/tex">
\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)
</script>
</span>.
</div>
<div class="Indented">
A <i>gate</i> is a function that takes such a tuple and produces a vector of values between zero and one. Note that any function in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}_{sig}
</script>
</span> and in <span class="MathJax_Preview"><script type="math/tex">
\mathcal{S}_{tanh}
</script>
</span> is actually a gate because of the properties of the <span class="MathJax_Preview"><script type="math/tex">
tanh
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
sig
</script>
</span> functions. We add gates into a recurrent neuron in order to control how much information will flow over the “recurrent connection”. That is, suppose that <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t\right)}
</script>
</span> is the layer’s current state that should be used at the next time step, a gated layer will use the vector <span class="MathJax_Preview"><script type="math/tex">
\hat{\mathbf{h}}^{\left(t\right)}=\mathbf{h}^{\left(t\right)}\circ\mathbf{s}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right),
</script>
</span> instead of using <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t\right)}
</script>
</span> (when the symbol <span class="MathJax_Preview"><script type="math/tex">
\circ
</script>
</span> denotes element-wise multiplication). Note that any coordinate in <span class="MathJax_Preview"><script type="math/tex">
\hat{\mathbf{h}}^{\left(t\right)}
</script>
</span> is a “moderated” version of the corresponding coordinate in <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t\right)}
</script>
</span> because each entry in the gate’s output is in <span class="MathJax_Preview"><script type="math/tex">
\left(0,1\right).
</script>
</span> We will see a concrete example in the next section. 
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> LSTM Cell
</h1>
<div class="Unindented">
Long Short Term Memory Cell (LSTM cell) is an improved version of the recurrent neuron that was proposed on 1997. It went through minor modifications until the version presented below (which is from 2013). LSTM cells solve both limitations of the basic recurrent neuron; it prevents the exploding and vanishing gradients problem, and it can <i>remember</i> as well as it can <i>forget</i>. 
</div>
<div class="Indented">
The main addition to the recurrent layer structure is the use of gates and a memory vector for each time step, denoted by <span class="MathJax_Preview"><script type="math/tex">
\mathbf{c}^{\left(t\right)}
</script>
</span>. The LSTM layer gets as inputs the tuple <span class="MathJax_Preview"><script type="math/tex">
\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)
</script>
</span> and the previous memory vector <span class="MathJax_Preview"><script type="math/tex">
\mathbf{c}^{\left(t-1\right)},
</script>
</span> then outputs <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t\right)}
</script>
</span> and an updated memory vector <span class="MathJax_Preview"><script type="math/tex">
\mathbf{c}^{\left(t\right)}.
</script>
</span>
</div>
<div class="Indented">
Here is how an LSTM layer computes its outputs: it have the following gates <span class="MathJax_Preview"><script type="math/tex">
\mathbf{f},\mathbf{i},\mathbf{o}\in\mathcal{S}_{sig}
</script>
</span> named the <i>forget</i>, <i>input</i> and <i>output</i> gate respectively, and a state-transition function <span class="MathJax_Preview"><script type="math/tex">
\mathbf{s}\in\mathcal{S}_{tanh}
</script>
</span>. It first updates the memory vector <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{c}^{\left(t\right)}=\mathbf{f}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\circ\mathbf{c}^{\left(t-1\right)}+\mathbf{i}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\circ\mathbf{s}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)

</script>
</span>
and then computes<span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{h}^{\left(t\right)}=\mathbf{o}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)\circ tanh\left(\mathbf{c}^{\left(t\right)}\right)

</script>
</span>
 Those equations can be explained as follows: the first equation is an element-wise summation of two terms. The first term is the previous memory vector <span class="MathJax_Preview"><script type="math/tex">
\mathbf{c}^{\left(t-1\right)}
</script>
</span> moderated by the <i>forget gate</i>. That is, the layer uses the current input <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}^{\left(t\right)}
</script>
</span> and previous output <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t-1\right)}
</script>
</span> to determine how much to shrink each coordinate of the previous memory vector. The second term is the candidate for the new state, i.e. <span class="MathJax_Preview"><script type="math/tex">
\mathbf{s}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right),
</script>
</span> moderated by the <i>input gate</i>. Note that all the gates operate on the same input tuple <span class="MathJax_Preview"><script type="math/tex">
\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right).
</script>
</span>
</div>
<div class="Indented">
The <i>input</i> and <i>forget</i> gates control the long-short term dependencies (i.e. the recurrent connection), and allow the LSTM layer to adaptively control the balance between new information that come from the state-transition function and the history information that comes from the memory vector, hence the names for the gates: <i>input</i> and <i>forget</i>.
</div>
<div class="Indented">
Another difference is that LSTM layer controls how much of its inner-memory to expose by using the <i>output</i> gate. That is being formulated in the second equation. 
</div>
<div class="Indented">
The addition of gates is what preventing the exploding and vanishing gradients problem. It makes the LSTM layer able to learn long and short term dependencies at the cost of increasing the number of parameters that are needed to be trained, and that makes the network harder to optimize.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> GRU 
</h1>
<div class="Unindented">
Another gated cell proposed on 2014 is the Gated Recurrent Unit (GRU). It has similar advantages as the LSTM cell, but fewer parameters to train because the memory vector was removed and one of the gates. 
</div>
<div class="Indented">
GRU has two gate functions <span class="MathJax_Preview"><script type="math/tex">
\mathbf{z},\mathbf{r}\in\mathcal{S}_{sig}
</script>
</span> named the <i>update</i> and <i>reset</i> gate respectively, and a state transition <span class="MathJax_Preview"><script type="math/tex">
\mathbf{s}\in\mathcal{S}_{tanh}.
</script>
</span> The input to GRU layer is only the tuple <span class="MathJax_Preview"><script type="math/tex">
\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)
</script>
</span> and the output is <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t\right)}
</script>
</span> computed as follow: first the layer computes its gates for the current time step, denoted by <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{z}^{\left(t\right)}:=\mathbf{z}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)

</script>
</span>
 for the <i>update gate</i> and by <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{r}^{\left(t\right)}:=\mathbf{r}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\right)

</script>
</span>
for the <i>reset gate</i>. Then the output of the layer is <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{h}^{\left(t\right)}=\left(1-\mathbf{z}^{\left(t\right)}\right)\circ\mathbf{h}^{\left(t-1\right)}+\mathbf{z}^{\left(t\right)}\circ\mathbf{s}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\circ\mathbf{r}^{\left(t\right)}\right)

</script>
</span>
with all the arithmetic operations being done element wise. 
</div>
<div class="Indented">
The term <span class="MathJax_Preview"><script type="math/tex">
\mathbf{s}\left(\mathbf{x}^{\left(t\right)},\mathbf{h}^{\left(t-1\right)}\circ\mathbf{r}^{\left(t\right)}\right)
</script>
</span> is a candidate for the next state. Note that the state-transition function accepts the previous state <i>moderated</i> by the reset gate, allowing it to forget past states (therefore the name of the gate: reset). Then the output of the layer is a linear interpolation between the previous state and the candidate state, controlled by the update gate.
</div>
<div class="Indented">
As oppose to the LSTM cell, the GRU doesn’t have an output gate to control how much of its inner state to expose therefore the entire state is being exposed at each time step. 
</div>
<div class="Indented">
Both LSTM and GRU are very common in many recurrent network architectures and achieve great results in many tasks. LSTMs and GRUs can learn dependencies of various lengths which make the network very expressive. However, a too expressive network can leads sometimes to overfitting, to prevent that it is common to use some type of regularization, such as Dropout. 
</div>
<div class="Indented">
Next post I will discuss the dropout variations that are specific for RNNs.
</div>
