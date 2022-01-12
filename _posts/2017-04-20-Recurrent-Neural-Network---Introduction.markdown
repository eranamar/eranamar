---
layout: post
author: Eran Amar
title:  Recurrent Neural Network - Introduction
date:   2017-04-20
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
<a class="toc" name="toc-Section-1">1</a> The First Post in the Series
</h1>
<div class="Unindented">
Recurrent Neural Network (RNN) is a neural network architecture that leads to state of the art results in many tasks, such as machine translation, speech recognition, image captioning and many more. Because it is a rapidly evolving field, it is difficult to find all the information (both basic and advanced material) in one place, so I decided to write a series of posts that will discuss Recurrent Neural Networks from their basic formulation to their most recent variations.
</div>
<div class="Indented">
This post, as the first in the series, will present the concept of a recurrent neuron and its limitations. Next posts will discuss gated neurons like LSTM and GRU, variants of dropout regularization for RNNs and some advanced stuff that I found interesting (bi-directional RNN, Multiplicative LSTM and etc). 
</div>
<div class="Indented">
The next section is dedicated to review the notations and basic terminology that will be used during the entire series. I will assume that the reader is familiar with the basics of neural network (fully connected architecture, back-propagation algorithm, etc). If you are not familiar with one of those concepts, I strongly recommend to review them online before going further. <a class="URL" href="https://www.academia.edu/25708860/A_Concise_Introduction_to_Machine_Learning_with_Artificial_Neural_Networks">Those notes </a> provides concise introduction to the field. There is also an excellent <a class="URL" href="https://www.youtube.com/watch?v=3BkFq55IEN8&amp;feature=youtu.be">video-lecture</a> (in Hebrew) by Prof. Shai Shalev-Shwartz from 2016. If you really want to dive deeper, you can find broad introduction in Chapter 6 in <i>Deep Learning</i> book (legally free electronic version <a class="URL" href="http://www.deeplearningbook.org/">here</a>).
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-2">2</a> Notations and Basic Terminology
</h1>
<div class="Unindented">
Artificial Neural Network (ANN) is a mathematical hierarchal model inspired from neuroscience. The basic computation unit is a <i>neuron,</i> which is a function from <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}^{d}
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}
</script>
</span>, <span class="MathJax_Preview">
<script type="math/tex;mode=display">

g:\mathbf{x}\mapsto\sigma\left(\left\langle \mathbf{x},\mathbf{w}\right\rangle +b\right)

</script>
</span>
 where <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x},\mathbf{w}\in\mathbb{R}^{d}
</script>
</span> are the <i>input</i> and <i>weight</i> vectors respectively, and <span class="MathJax_Preview"><script type="math/tex">
b\in\mathbb{R}
</script>
</span> is the <i>bias</i>. The function <span class="MathJax_Preview"><script type="math/tex">
\sigma
</script>
</span> is a non-linear <i>activation</i> function <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}\to\mathbb{R}
</script>
</span>, for example <span class="MathJax_Preview"><script type="math/tex">
\sigma\left(z\right)=ReLU\left(z\right):=\max\left(z,0\right)
</script>
</span> or <span class="MathJax_Preview"><script type="math/tex">
\sigma\left(z\right)=tanh\left(z\right):=\frac{e^{z}-e^{-z}}{e^{z}+e^{-z}}
</script>
</span>. Note that a neuron can ignore some of its input coordinates by setting their corresponding entries in its weight vector to zero.
</div>
<div class="Indented">
Neurons are organized within <i>layers</i>. In the very basic ANN architecture, called a<i> fully connected (FC) neural network</i>, a layer is just a collection of neurons that will be computed in parallel (meaning that they are not connected to each other). For any layer <span class="MathJax_Preview"><script type="math/tex">
i
</script>
</span>, each of its neurons accepts as inputs all neurons from layer <span class="MathJax_Preview"><script type="math/tex">
i-1
</script>
</span>. 
</div>
<div class="Indented">
A layer in FC architecture can be formulate mathematically as a function from <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}^{d}
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}^{n}
</script>
</span> such that <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{f}:\mathbf{x}\mapsto\left(\begin{array}{c}
g_{1}\left(\mathbf{x}\right)\\
\vdots\\
g_{n}\left(\mathbf{x}\right)
\end{array}\right)

</script>
</span>
 where each <span class="MathJax_Preview"><script type="math/tex">
g_{i}
</script>
</span> is a function of a neuron as defined above, with its own weight vector and a bias scalar. The formulation can be simplified using matrix notations: <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{f}:\mathbf{x}\mapsto\sigma\left(W\mathbf{x}+\mathbf{b}\right)

</script>
</span>
 where <span class="MathJax_Preview"><script type="math/tex">
W\in\mathbb{R}^{n\times d}
</script>
</span> contains in the <i>i</i>-th row the weight vector of <span class="MathJax_Preview"><script type="math/tex">
g_{i}
</script>
</span>, and <span class="MathJax_Preview"><script type="math/tex">
\mathbf{b}_{i}
</script>
</span> is the bias of <span class="MathJax_Preview"><script type="math/tex">
g_{i}
</script>
</span>. The function <span class="MathJax_Preview"><script type="math/tex">
\sigma
</script>
</span> is the same as before but since its input is now a vector, it apply element-wise on each coordinate of the input, producing an output vector of the same length.
</div>
<div class="Indented">
We denote the family of all possible FC functions based on some fixed activation function as <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathcal{F}_{\sigma}=\left\{ \mathbf{x}\mapsto\sigma\left(W\mathbf{x}+\mathbf{b}\right)\mid\forall W,\mathbf{b}\right\} 

</script>
</span>
that family contains all possible layers for any input\output dimension. For abbreviation, I will not mention the dimensions of layers or the specific activation function <span class="MathJax_Preview"><script type="math/tex">
\sigma
</script>
</span> when they are not important for the discussion. 
</div>
<div class="Indented">
Applying several layers sequentially yield a feedforward FC neural network. That is, given <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}\in\mathbb{R}^{d}
</script>
</span>, a FC network with <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> layers is <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathcal{N}\left(\mathbf{x}\right)=\mathbf{f}_{t}\left(...\mathbf{f}_{2}\left(\mathbf{f}_{1}\left(\mathbf{x}\right)\right)\right)

</script>
</span>
 where <span class="MathJax_Preview"><script type="math/tex">
\forall i:\:\mathbf{f}_{i}\in\mathcal{F}_{\sigma}
</script>
</span> for some fixed <span class="MathJax_Preview"><script type="math/tex">
\sigma
</script>
</span>.
</div>
<div class="Indented">
The number of layers in the network is its <i>depth</i>, and the size of the layers is its <i>width</i> (assuming all the layers are of the same dimension). <i>Training</i> the network is optimizing all the weight matrices and bias vectors such that the network will approximate some target (usually unknown) function. We can think of the optimization process as a process of iteratively updating the choice of layer functions <span class="MathJax_Preview"><script type="math/tex">
\mathbf{f}_{1},\mathbf{f}_{2},..
</script>
</span> from the family <span class="MathJax_Preview"><script type="math/tex">
\mathcal{F}_{\sigma}
</script>
</span>.
</div>
<div class="Indented">
The FC architecture is simple, yet yields a very powerful model. When wide enough, even a single hidden layer network can approximate to any arbitrary precision almost any continuous function (under some mild assumptions). However, for some tasks it might not be optimal, and different architectures are being used, instead of the FC one. 
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-3">3</a> The Need for Context
</h1>
<div class="Unindented">
There are tasks that require some notion of <i>context</i> in order to make a decent prediction. For example, tasks that involve time-steps can consider the history until the current time step as a context (a.k.a time-series tasks). For concrete example, consider predicting the next word in a given sequence. Each word has as a context all words preceding it. Note that there are more ways to define the notion of a context, and it mostly depends on the task in hand. For didactic purposes we will focus on the example of next-word prediction. 
</div>
<div class="Indented">
How can we build a model for next-word prediction using FC neural network? 
</div>
<div class="Indented">
The obvious answer is to fix some window size <span class="MathJax_Preview"><script type="math/tex">
d
</script>
</span>, and build a network that gets the last <span class="MathJax_Preview"><script type="math/tex">
d
</script>
</span> words in the sentence as <i>features</i>, in order to predict the next word. One immediate limitation of this approach is that the window size is the same for <b>all </b>the examples in the training set, and it is not simple to find a suitable window size. On the one hand, choosing small window may cause the model to miss some important long term relations between words that are more than <span class="MathJax_Preview"><script type="math/tex">
d
</script>
</span> steps apart (for sentences longer than <span class="MathJax_Preview"><script type="math/tex">
d
</script>
</span>). On the other hand, choosing very large <span class="MathJax_Preview"><script type="math/tex">
d
</script>
</span> increases the number of parameters in the model, which make the optimization harder, and increase the computation time per example, which affect both training-time and prediction-time.
</div>
<div class="Indented">
Another limitation is that FC layers regards their inputs as ordered information. That is, a FC layer assigns a specific weight for each coordinate in its input vector, causing the order of the features to be meaningful. But in some tasks the order of the features may vary, for example, the subject of a sentence doesn’t limit to specific location inside an English sentence. If we want FC layer to account for that, we have to get a lot of examples (and they need to cover all different cases). 
</div>
<div class="Indented">
What we would like to have here is called <i>parameter sharing</i>. We will come back to that term later in this post.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-4">4</a> Recurrent Cell - Learning to Remember
</h1>
<div class="Unindented">
We want our network to be able to <b>remember</b> information about previous invocations (i.e. the past inputs). In order to do that, our network have to maintain a state. So we need to extend our neuron formulation to include another variable, <span class="MathJax_Preview"><script type="math/tex">
h\in\mathbb{R}
</script>
</span>, that will be its state and be kept during consecutive invocations.That state variable is usually initialized to be zero so that it doesn’t affect the first computation.
</div>
<div class="Indented">
We will use an upper-script to denote the time step, so <span class="MathJax_Preview"><script type="math/tex">
h^{\left(t\right)}
</script>
</span> is the state of a neuron at time <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>. Since the neurons need to pass information to themselves (i.e. their state from previous invocation) they become <i>recurrent neurons,</i> a.k.a <i>recurrent cell</i>s. 
</div>
<div class="Indented">
Recurrent neuron uses internally two functions. The first function updates the inner state based on <b>current input</b> and <b>previous state</b>, which is the transition from time <span class="MathJax_Preview"><script type="math/tex">
t-1
</script>
</span> to time <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>, <span class="MathJax_Preview">
<script type="math/tex;mode=display">

s:\mathbf{x},h^{\left(t-1\right)}\mapsto tanh\left(\left\langle \mathbf{x},\mathbf{w}\right\rangle +b+h^{\left(t-1\right)}\cdot u\right)

</script>
</span>
<span class="MathJax_Preview"><script type="math/tex">
u\in\mathbb{R}
</script>
</span> is a new parameter denoting the weight for the previous state.
</div>
<div class="Indented">
And the second function is for computing the output of the neuron at current time <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>, based on its <b>new state</b><span class="MathJax_Preview">
<script type="math/tex;mode=display">

g^{\left(t\right)}:h^{\left(t\right)}\mapsto\sigma\left(h^{\left(t\right)}\cdot v+c\right)

</script>
</span>
with <span class="MathJax_Preview"><script type="math/tex">
v,c\in\mathbb{R}
</script>
</span> as the weight and bias of the computation respectively. 
</div>
<div class="Indented">
We can move again to matrix notations to denote several parallel recurrent cells that forms a <i>recurrent layer</i>. The first function updates the vector of states for the entire layer <i><span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{s}:\mathbf{x},\mathbf{h}^{\left(t-1\right)}\mapsto tanh\left(W\mathbf{x}+\mathbf{b}+U\mathbf{h}^{\left(t-1\right)}\right)

</script>
</span>
</i> and the second function for computing the output of the layer based on the current state<span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{f}:\mathbf{h}^{\left(t\right)}\mapsto\sigma\left(V\mathbf{h}^{\left(t\right)}+\mathbf{c}\right)

</script>
</span>
When we come to compare that to the FC formulation, we see that actually the function <span class="MathJax_Preview"><script type="math/tex">
\mathbf{f}
</script>
</span> itself hasn’t changed, but now it accepts as input the state of the layer <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}
</script>
</span>, rather than the input to the layer <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}
</script>
</span>. Another change is that before the layer computes its output, it first update its hidden state.
</div>
<div class="Indented">
Note that we can extend the definition of the family <span class="MathJax_Preview"><script type="math/tex">
\mathcal{F}_{tanh}
</script>
</span> to contains functions like the state-transforming function <span class="MathJax_Preview"><script type="math/tex">
\mathbf{s}
</script>
</span>, by making each <span class="MathJax_Preview"><script type="math/tex">
\mathbf{f}\in\mathcal{F}_{tanh}
</script>
</span> accepts two vectors as input instead of one (and we also need another weight matrix for the second input). So denote the family of all hidden-state transition functions as <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathcal{S}_{\sigma}=\left\{ \mathbf{x},\mathbf{h}^{\left(t-1\right)}\mapsto\sigma\left(W\mathbf{x}+\mathbf{b}+U\mathbf{h}^{\left(t-1\right)}\right)\mid\forall W,U,\mathbf{b}\right\} 

</script>
</span>
usually the activation function is <span class="MathJax_Preview"><script type="math/tex">
\sigma=tanh
</script>
</span>. We will use that family later on, when discuss gated units as LSTM and GRU.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-5">5</a> Comparing the Architectures - an Example
</h1>
<div class="Unindented">
Consider again the next-word prediction problem. Suppose we have an ordered dictionary that contains the entire vocabulary we will ever need. We will represent any word by an integer indicating its index in the dictionary. In this section we will construct two single-neuron networks, once using the FC architecture and once using the Recurrent architecture.
</div>
<div class="Indented">
FC network for next-word prediction: Let the window size be <span class="MathJax_Preview"><script type="math/tex">
d=3
</script>
</span>. The input to the network can be represent as a vector <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}\in\mathbb{N}^{3}
</script>
</span> and the output as <span class="MathJax_Preview"><script type="math/tex">
y\in\mathbb{N}
</script>
</span>. The FC network amounts to <span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathcal{N}_{FC}\left(\mathbf{x}\right)=f\left(x\right)

</script>
</span>
 for some <span class="MathJax_Preview"><script type="math/tex">
f\in\mathcal{F}_{\sigma}
</script>
</span> that maps from <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}^{3}
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}
</script>
</span>. 
</div>
<div class="Indented">
Recurrent neural network (RNN), however, leads to different representation: the input and the output both integers <span class="MathJax_Preview"><script type="math/tex">
x,y\in\mathbb{N},
</script>
</span> so the state transition function <span class="MathJax_Preview"><script type="math/tex">
s\in\mathcal{S}
</script>
</span> and the output function <span class="MathJax_Preview"><script type="math/tex">
f\in\mathcal{F}_{\sigma}
</script>
</span> are both from <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}
</script>
</span> to <span class="MathJax_Preview"><script type="math/tex">
\mathbb{R}
</script>
</span>. The idea of the recurrent architecture is to apply the <b>same recurrent layer</b> several times on <b>different inputs </b>(each input from different time-step) in order to get a prediction. 
</div>
<div class="Indented">
Here is a detailed description of how to do that: we take a series of three consecutive words from the beginning of a sentence: <span class="MathJax_Preview"><script type="math/tex">
x^{\left(1\right)},x^{\left(2\right)},x^{\left(3\right)}
</script>
</span> and a target next word: <span class="MathJax_Preview"><script type="math/tex">
y
</script>
</span>. First, feed the network with <span class="MathJax_Preview"><script type="math/tex">
x^{\left(1\right)}
</script>
</span>, it updates the inner state from <span class="MathJax_Preview"><script type="math/tex">
h^{\left(0\right)}=0
</script>
</span> to some hidden state <span class="MathJax_Preview"><script type="math/tex">
h^{\left(1\right)}
</script>
</span> and outputs <span class="MathJax_Preview"><script type="math/tex">
\hat{y}^{\left(1\right)}
</script>
</span>. We ignore that output and feed the next word <span class="MathJax_Preview"><script type="math/tex">
x^{\left(2\right)}
</script>
</span>, the network updates its state to <b><span class="MathJax_Preview"><script type="math/tex">
h^{\left(2\right)}
</script>
</span> </b>and produces <span class="MathJax_Preview"><script type="math/tex">
\hat{y}^{\left(2\right)}
</script>
</span> that is also ignored. Lastly, we feed <span class="MathJax_Preview"><script type="math/tex">
x^{\left(3\right)}
</script>
</span> to the network, the inner state is being updated to <span class="MathJax_Preview"><script type="math/tex">
h^{\left(3\right)}
</script>
</span> and another output is produced <span class="MathJax_Preview"><script type="math/tex">
\hat{y}^{\left(3\right)}
</script>
</span> which is now considered as the prediction of the network. Note that in each invocation of the network, the output depends on both the current input and the previous state, therefore <span class="MathJax_Preview"><script type="math/tex">
\hat{y}^{\left(3\right)}
</script>
</span> is the result of the entire history <span class="MathJax_Preview"><script type="math/tex">
x^{\left(1\right)},x^{\left(2\right)},x^{\left(3\right)}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
h^{\left(0\right)}
</script>
</span> (which was initialized to zero).
</div>
<div class="Indented">
At first glance it looks like there is no meaningful differences between the architectures. We have an implicit sliding window over the words in the sentence for the recurrent neuron rather than explicit sliding window as with the FC neuron. Moreover, the output <span class="MathJax_Preview"><script type="math/tex">
y
</script>
</span> of FC neuron also depends on all previous time steps because the input <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}
</script>
</span> is just the values of <span class="MathJax_Preview"><script type="math/tex">
\left(x^{\left(1\right)},x^{\left(2\right)},x^{\left(3\right)}\right)
</script>
</span>. 
</div>
<div class="Indented">
However, note that in the FC architecture, each word in the input vector has its own weight and bias whereas in recurrent architecture we use the same weights over and over again (by applying the same function). In other words, in recurrent layers we <i>share the parameters across time</i>. The formula below demonstrate how we applied the same function <span class="MathJax_Preview"><script type="math/tex">
s\in\mathcal{S}
</script>
</span> to updates the hidden-state across different time steps <span class="MathJax_Preview">
<script type="math/tex;mode=display">

h^{\left(t\right)}=s\left(x^{\left(t\right)},h^{\left(t-1\right)}\right)\Longrightarrow h^{\left(3\right)}=s\left(x^{\left(3\right)},s\left(x^{\left(2\right)},s\left(x^{\left(1\right)},0\right)\right)\right)

</script>
</span>
then making a prediction using some <span class="MathJax_Preview"><script type="math/tex">
f\in\mathcal{F}_{\sigma}
</script>
</span> on the current state<span class="MathJax_Preview">
<script type="math/tex;mode=display">

\hat{y}^{\left(3\right)}=f\left(h^{\left(3\right)}\right)

</script>
</span>
Parameter sharing has a very powerful effect on the learnability of the network both because it reduces the number of parameters that needed to be optimized and it make the network robust to small changes in the context (recall the example about the order of features in the input). In addition, the recurrent layer practically has a<i> dynamic</i> window size. Going back to our next-word prediction network, we can continue to apply the network on consecutive inputs, teaching it dependencies between words that are further than 3 time steps. 
</div>
<div class="Indented">
Note that in our example, we ignored the “intermediate” predictions of the network: <span class="MathJax_Preview"><script type="math/tex">
\hat{y}^{\left(1\right)}
</script>
</span> and <span class="MathJax_Preview"><script type="math/tex">
\hat{y}^{\left(2\right)}
</script>
</span>, but we could keep them and feed them as inputs to another recurrent layer making our RNN network deeper. We will discuss it in a later post.
</div>
<h1 class="Section">
<a class="toc" name="toc-Section-6">6</a> Limitations of Recurrent Cells
</h1>
<div class="Unindented">
Unfortunately, RNNs are not perfect and maintaining memory is not enough, sometimes the networks needs to be able to <i>forget</i>, for instance, when predicting next-word in a sentence the network needs to reset its state once it reach the end of the sentence. 
</div>
<div class="Indented">
Furthermore, parameter sharing has a serious drawback. Recall that applying the network for multiple time steps leads to a composition of the same <span class="MathJax_Preview"><script type="math/tex">
\mathbf{s}\in\mathcal{S}
</script>
</span> upon itself <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> times, something like this<span class="MathJax_Preview">
<script type="math/tex;mode=display">

\mathbf{h}^{\left(t\right)}=\mathbf{s}\left(\mathbf{x}^{\left(t\right)},\mathbf{s}\left(\mathbf{x}^{\left(t-1\right)},\mathbf{s}\left(\mathbf{x}^{\left(t-2\right)},...\right)\right)\right)

</script>
</span>
 which may cause the gradient during back-propagation to either explode or vanish. This problem is known as <i>the</i> <i>vanishing and exploding gradients</i>. In order to get an intuition about that, we make the following simplifications: assume that the activation function inside <span class="MathJax_Preview"><script type="math/tex">
\mathbf{s}
</script>
</span> is the identity instead of <span class="MathJax_Preview"><script type="math/tex">
tanh
</script>
</span>, and <span class="MathJax_Preview"><script type="math/tex">
\mathbf{x}^{\left(t\right)}=\mathbf{0}
</script>
</span> for all <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span>. Suppose that <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(0\right)}
</script>
</span> is given to us and it is not all zeros (that is, it encodes some information about the past). 
</div>
<div class="Indented">
We have that <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t\right)}=U\mathbf{h}^{\left(t-1\right)}
</script>
</span> so applying the recurrent neuron for <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> time steps amounts to <span class="MathJax_Preview"><script type="math/tex">
\mathbf{h}^{\left(t\right)}=U^{t}\mathbf{h}^{\left(0\right)}
</script>
</span>. What happen to <span class="MathJax_Preview"><script type="math/tex">
U^{t}
</script>
</span> as <span class="MathJax_Preview"><script type="math/tex">
t
</script>
</span> increases? it easier to see the answer when considering the scalar case. If <span class="MathJax_Preview"><script type="math/tex">
U
</script>
</span> was a real positive number, then either <span class="MathJax_Preview"><script type="math/tex">
U^{t}\to0
</script>
</span> when <span class="MathJax_Preview"><script type="math/tex">
U<1
</script>
</span>, or <span class="MathJax_Preview"><script type="math/tex">
U^{t}\to\infty
</script>
</span> when <span class="MathJax_Preview"><script type="math/tex">
U>1
</script>
</span>. The same goes with matrices, suppose <span class="MathJax_Preview"><script type="math/tex">
U
</script>
</span> has an eigenvalue decomposition such that <span class="MathJax_Preview"><script type="math/tex">
U=V^{\top}\Sigma V
</script>
</span> for orthogonal <span class="MathJax_Preview"><script type="math/tex">
V
</script>
</span>, so <span class="MathJax_Preview"><script type="math/tex">
U^{t}=V^{\top}\Sigma^{t}V
</script>
</span> causing the small entries on the diagonal of <span class="MathJax_Preview"><script type="math/tex">
\Sigma
</script>
</span> to vanish, and the large entries to explode. Since the gradients in our network are being scaled according to <span class="MathJax_Preview"><script type="math/tex">
\Sigma^{t}
</script>
</span>, they are either explode or vanished. The problem with very large gradients is that they cause the network to diverge (large updates to the parameters), on the other hand, very small gradients cause infinitesimally small updates to the parameters which prevent the network from learning. That is describe much more formally and rigorously <a class="URL" href="https://arxiv.org/abs/1211.5063">here</a>.
</div>
<div class="Indented">
Next post I will discuss two popular improvements to the recurrent neurons that helps avoid the limitations above: the LSTM and GRU cells.
</div>
<div class="Indented">
<b>Acknowledgments:</b> I want to thank Ron Shiff for his useful comments and corrections to this post.
</div>
