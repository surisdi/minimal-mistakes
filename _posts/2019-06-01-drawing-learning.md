---
title: "Learning Words by Drawing Images"
comments: true
---

In this post I will introduce and motivate our recent CVPR'19 paper 
[Learning Words by Drawing Images](/publications/#drawing-learning-ref).

The idea of the project is to train an audio-visual model that learns the correspondence between spoken words and visual
concepts. You can imagine it as babies learning language, where they have to learn new words, and their mapping to the 
"real" (visual) world. The only available supervision is just pairs of matching images and spoken descriptions of these
 images, as babies learn language by associating the world they perceive with the language they hear describing that
 world.
 
 A basic approach with very good results is the one presented in our previous 
 [ECCV'18 paper](/publications/#eccv_harwath), where a triplet loss was used to generate embeddings both for the speech 
and for the images such that corresponding concepts &mdash;both in the visual and spoken domain&mdash; are close in the 
embedding space. While this method proved to be very effective, it has a main drawback: it does not learn complex 
concepts such as attributes, relationships, adverbs or even verbs. It mainly relies on learning nouns. 

*Why is that?* The reason is that it is a discriminative model, and as such its goal is to learn a border between correct 
and incorrect associations of images and text. It does not need to learn *all* possible borders, just one. And thus it 
learns the easy one, which is created by nouns (instead of more complex parts of speech). In contrast, generative models, like the generators learned by a generative 
 adversarial network (GAN) model all the distribution, all the details.

*And what can we do to improve the model so that it learns more interesting concepts?* Just take a look at the previous
paragraph, and you will see that the main problem was that a regular discriminative model is not able to (because it does 
not *need* to) discriminate complex concepts.
 
Someone could say "well, let's force it to learn them". -- How can we *directly* train the discriminative model in a 
more demanding and complex setting?

Someone else would
answer "let's just change the framework and work with generative &mdash;instead of discriminative&mdash; models". And 
we say: "let's do both". In this project we rely on the powerful representations learned by a generative model, a GAN in
this case, to learn a better discriminative model. A trained generator of a GAN already has a very good representation 
of the world: it has learned to model attributes and objects, as well the relationship between them. However, it does 
not know anything about language, those concepts are probably mixed together in a non-interpretable way, and this is why 
we need to train the discriminative model on top, which implies learning the cross-modal relation between vision and 
language, given a good vision model, but nothing else.

The idea of having discriminative and generative models helping each other is not new: among other works, GANs consist 
precisely on that. However, we propose a new approach in which the main focus is the discriminative 
&mdash;not the generative&mdash; model, and more important, the generative model helps learning something outside of its
domain, we use it to learn models for other modalities.

More specifically, what we do is generate very hard negatives for the discriminative model to learn from. The harder the
negatives (with changes wrt the positive being more specific), the more complex details the discriminative model will learn. 
We use the generative model to generate these harder negatives. Some examples can be seen in the following figure, where
the top row shows positive examples, and the bottom row shows the generated negative examples. All of this is without any 
supervision, just image-speech pairs.

![](/assets/images/examples_edits.pdf) 

Then, a more practical way of viewing this project is the following: we are using a contrastive loss that uses negatives 
that the model has to learn to move far away from the positive (anchor). To get a finer border for the discriminative 
model, we need hard negatives (this is a very known idea in machine learning): **let's CREATE these hard negatives instead of just 
retrieving them from the training set**. And how do we create them? With a GAN, of course. 

I hope you got the main ideas of the project. For the technical details please go to the paper. Feel free to message me 
for more information or suggestions. Let me finish with a couple of pictures of Adrià and me presenting this work at 
CVPR'19.

| ![](/assets/images/IMG_8310.png)  	| ![](/assets/images/IMG_8311.JPG) 	| 
|----------	|:-------------:	|
|    |     |


