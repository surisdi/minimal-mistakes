---
title: "Maximizing mutual information for representation learning"
comments: true
---
<blockquote>
  <p>Self-supervised learning is. Important because this an that... Maximization of Mutual Information between the signal and its representation is a mathematically-sound way of obtaining good representations. </p>
</blockquote>


 

This is an overview of recent work on the topic of . The purpose of this post is to provide an accessible review of the 
work. 

## Self-supervised learning

As a side note, some people complain about the term self-supervised learning being just a rebranding of unsupervised
learning. Link to tweets:

<div align="center">
<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Check out my iOS game, High5! <a href="https://t.co/QZEKLg3G2i">https://t.co/QZEKLg3G2i</a></p>&mdash; Keita Ito (@keitaitok) <a href="https://twitter.com/keitaitok/status/504110217940836353">August 26, 2014</a></blockquote>
<script async="" src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
</div>



https://twitter.com/RandomlyWalking/status/1150773129624473602
https://twitter.com/ylecun/status/1151527997326757888
https://twitter.com/cvondrick/status/1151090628505276417


Why? Why now?
- Text
- Images

Different techniques

## Mutual Information

Mutual information (MI in short) is an information theoretic measure of the mutual dependence between two random 
variables. From a beginner point of view, it is a generalized correlation that takes into account non-linear 
relationships. From an information theory point of view, it measures the amount of *information* we obtain about a 
variable when we know the value of another one. Mathematically it is defined as:

$$
I(X;Y)=\mathbb{E}_{p(x,y)}\Bigg[ \log \frac{p(x,y)}{p(x)p(y)} \Bigg]
$$

for two random variables $$X$$ and $$Y$$. It can be represented


## Mutual information estimation and maximization

We refer to the paper for formal derivations of these bounds. Several bounds:

| Name  | Lower bound |
|-------|:-----:|
| Barber & Agakov   | $$I(X;Y)\geq \mathbb{E}_{p(x,y)}[\log q(x\|y)] + h(X)$$|
| NWJ; f-GAN KL; MINE-f   | $$I(X;Y)\geq \mathbb{E}_{p(x,y)}[f(x,y)]-\frac{1}{e}\mathbb{E}_{p(x)p(y)}\Big[e^{f(x,y)}\Big]$$|
| Noise Contrastive Estimation (NCE)   | $$I(X;Y)\geq \mathbb{E}_{\prod_i p(x_i,y_i)}\Bigg[\frac{1}{K}\displaystyle\sum_{i=1}^{K}\log\frac{e^{f(x_i, y_i)}}{\frac{1}{K}\sum_{j=1}^{K}e^{f(x_i, y_i)}}\Bigg]$$|
| Tractable NCE   | $$I(X;Y)\leq \mathbb{E}_{\prod_i p(x_i,y_i)}\Bigg[\frac{1}{K}\displaystyle\sum_{i=1}^{K}\log\frac{p(y_i\|x_i)}{\frac{1}{K-1}\sum_{j\neq i}p(y_i\|x_j)}\Bigg]$$|


There is also an upper bound. It relies on the knowledge of the posterior $$p(y|x)$$, which can still be useful for 
neural networks if we force a known posterior, like a Gaussian. This can be done by making the network output a mean
and a variance vector and sampling from an independent Gaussian (mean-shift noseque). In the paper they use it to get
some disentanglement and they obtain good results. 

<p class="notice--danger"><strong>Disclaimer:</strong> Small letter: I was not able to make this bound work in practice, 
as it was too unstable for my system. I will wait some months and try again, I guess..</p>

(𝜔 ̂,𝜓 ̂ )=" "   argmax┬(𝜔, 𝜓)⁡〖𝐼 ̂_𝜔 〗 (𝑋;𝐸_𝜓 (𝑥))


## Representations by maximizing Mutual Information

The papers explained in this section use (and sometimes derive) some of the different bounds in the previous section.
We will not worry about the specific bounds used by these papers, as we only need to know that they use a lower bound
to the mutual information. The goal of this section is, thus, explaining how the different papers use the bound to 
obtain good representations. **The main difference between them is how they define the views among which the MI has to be
maximized**. A view is a general concept representing any signal describing a scene or context, and we want views 
representing the same scene or context to have high MI.

Schematic here
CONTEXT  --> Picture (view 1) --> Augmentation of the picture (view 2)
                              --> Features of the picture after encoder (view 3)
                              --> Features of the picture in the middle of the encoder (view 8)
         --> Audio recording (view 4)  --> Features of the audio after encoder (view 5)
                                       --> Audio recording at second 5 (view 6)
                                       --> Audio recording at second 10 (view 7)     
         
All these views are representing the same concept. Depending on what the task is, we may want to stress some 
consistencies or some others in this context, and this decision will lead us to choose the set of views we want to use.

       

Not just representations, but *very good* representations

Other recent papers more application-focused:


## Disclaimer

It is in its early stages. Some theoretically limiting results. Not studied if only learning low level features.
But very promising.