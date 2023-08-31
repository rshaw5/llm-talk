---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: 'LLMs: Why and How'
---

# LLMs: Whence and Whither?

<!--
Hey everybody. You know, to be honest with you, I'm actually really excited to be speaking at this inaugural "LLM".
-->

---
layout: two-cols
---

## LLMs

<v-click> 

- **L**easecake

</v-click>

<v-click>

- **L**ocal

</v-click>

<v-click>

- **M**unch

</v-click>

<v-click>

- 'n

</v-click>

<v-click>

- **L**earn

</v-click>

::right::

<v-click>

![](/cat-eating.png)

</v-click>

<!--
(Local Leasecake Munch-n-Learn).
-->

---
layout: two-cols
---

# Coming Up
## LLMs
- (2 min) Background
- (10 min) LLM Structure
- (3 min) In Practice
- (15 min) Q&A and Demos

::right::

<v-click>

![](/hollywoo.png)

</v-click>

<!--
LLMs. What are they? How do they work? Why do they work? We're already exploring the possibilities with CakeBot and I'd like to shed some light on the mechanics as I understand them now. Hopefully it'll give you some perspective on how things are working under the hood so that we can be even more effective with it. 

Now I do have to caveat, this is my current understanding. I'm still learning, so will not have everything exactly spot on, but I'm going to give you the best I've got so far. 

We'll start with a quick background to set the stage, the meat of the thing will be talking about neural networks and LLMs, and then we'll cover some 

So I'd like to start with a little bit of background on computing in general and how it built up into neural networks. Then we'll go into the LLMs specifically, some applications of some applied ways to consider with LLMs some applications that are important.the Lease Cake Local Munch and Learn.
-->

---
layout: two-cols
---

# Historical Context
*Then to Now in 1 Minute (maybe 2)*

- Turing Machines (imperative) and The Lambda Calculus (declarative)
- State Machines
- Neural Networks
- Deep Neural Networks, Convolutional NNs, Feed-Forward NNs, and more
- The game changer: Transformers

::right::

![](/network.png)

<!--
Okay. So to set the scene, I want to give you just a little bit of historical context in about a minute if I can:

Before computers existed, we had mathematical logic. There was Boolean Logic and The Lambda Calculus. These were both methods of taking inputs, processing them in a predictable way, and producing outputs.

Then along came Alan Turing and we got the concept of "Turing Machines". Now, as Turing Machines, the computers we're familiar with function on what's called an imperative system: You write instructions and it does exactly what you tell it, in the order you tell it, every time.

As we moved on, we started to try to simulate intelligent behavior. One of these early attempts are "State Machines". You can still see these in video games today, although I'm sure they're used elsewhere too. 

A state machine declares a finite number of "states" for the AI to exist in, and triggers that cause the AI to enter or exit those states. So, for example, maybe an AI character is being attacked by the player, so it enters a "defense state". Then the player runs away, so it enters a "hunt state" and so on. But the thing is you have to expressly define these states, what it does, how it gets into and out of them. That becomes really unwieldy very fast. 

So we started to move on to this concept of neural networks: a model of the human mind and its dendron connections. Through this model, the AI gains the ability to "learn", or at least to do something that looks like learning.

From there we moved on to a lot of different ways of trying to enhance these things: deep neural networks, convolutional, feedforward, and more. But the real game changer for us specifically, is transformers, which we will get int shortly.
-->

---
layout: image-right
image: /function-approx.gif
---

# The Mechanics of LLMs
## Neural Networks

- Neural networks are *universal function approximators*

<Transform :scale="0.8">

source: https://www.youtube.com/watch?v=0QczhVg5HaI

</Transform>

<!--
So to get into neural networks, let's start with what they are trying to accomplish.

Neural networks, at their core, are "universal function "approximators", meaning they are attempting to "see" some data and come up with some sort of function that accurately represents that data. 

If successful, the network will then be able to accurately predict the correct output for any given input, that matches the original data. 

So you can see here in this graphic each neuron is building on the previous neuron to try and model this curve. When it does it accurately enough the blue line will match closely enough the orange line to be useful. That is function approximation.
-->

---
layout: iframe
url: https://playground.tensorflow.org
---

## Large Language Models

<!--
So, now that we understand what neural networks are trying to do, let's take a look at this example website. I'll provide the link at the end here. 

So you can see here that there is a classification problem going on. We're trying to create a function that correctly matches this distribution of orange and blue dots. Given some input for this neural network, we want to correctly label it orange or blue.

What happens is each neuron gets some sort of activation pattern. So in this instance, if this neuron sees a new data point on the left, it will want to classify it as "orange". This other neuron wants to classify data points at the bottom as "orange".

Each neuron will "vote" for the activation or deactivation of each neuron in the next layer. These votes are added together along with the "bias" of the neuron and then that layer of neurons votes for the activation of the next layer, and so on.

Then, the network enters a "learning" phase in which it adjusts the neurons weights and biases through a process called backpropagation. If I really slow down the learning rate here you can see the hidden layers changing over time as their weights and biases are adjusted and "learning" occurs.

Importantly, a difference between neural networks and actual minds is that after training, there is no more learning. Back propagation is the process of each neuron looking at the result and then adjusting its weights and biases and then telling the next neuron back what it did and so on until you get to the end. And they make adjustments. But that process only happens during training. It's very expensive computationally. So that is one of the main differences and one of the weaknesses of neural networks.
-->

---

### So, what about language?
- If I want to complete the sentence "Black cat ate", how would we do it?
- Statistical prediction from previous words (prohibitively expensive)
  - "ate" => "cheese"
  - "cat ate" => "mouse"
- Recurrent neural networks (better, but still only works for about one sentence)
  - "Black cat" => [23, 1]
  - [23, 1] "ate" => "the"
  - "[23, 1] "ate" => [45, 11, 55]
  - [45, 11, 55] "the" => "mouse"
- Attention (using transformers)

<!--
So that's all well and good, but what about language? What if we want to do something like translate "Black cat ate mouse" from English to French? (I took out some words to make the example that's coming up easier.) If we want to translate that to French, how would we do that? 

Well, the first idea would be to predict the next word simply based on the previous word. So, maybe our model has learned that the most frequent word to appear after "ate" is "mouse". But, more likely, it's probably learned the most likely word after "ate" is "cheese", or something like that. Obviously one word isn't enough, so what if we look at the statistical probability of the word that follows "cat ate"? Well, maybe it is "mouse", but the issue is that we now have to learn statistics for every word that comes after "ate", "cat", "cat ate", and "ate cat". If we add another word, we have to memorize even more statistics. It quickly becomes untenable.

So, what if we take the whole sentence, do some math to it, and turn it into a "vector"? So now we give the AI the vector and the most recent word, and ask it for a prediction. Then we change the vector and repeat. The issue here is that we are actively modifying our vector as we are making word predictions. The further we go, the more we start to lose the context of the original sentence. This method only works for about a sentence length or so.

What if we want more context than that? That's where attention comes in.

-->

---
layout: two-cols
---

## How does "attention" work?

- At each timestep, the AI actively decides which part of the input data is relevant.
- The input data is unchanged as it moves through the prediction mechanism.

### Example: Translate "Black cat ate mouse" to French

<v-clicks depth="2">

- Tokenization: ['Black', 'cat', 'ate', '#']
- Encoding:
  - ['Black'] => [2]
  - ['Black', 'cat'] => [3, 99]
  - ['Black', 'cat', 'ate'] => [10, 55, 6]
  - ['Black', 'cat', 'ate', 'mouse'] => [33, 8, 100, 1]
  - ['Black', 'cat', 'ate', 'mouse', '#'] => [94, 5, 33, 1, 6]

</v-clicks>

::right::

<v-clicks depth="2">

- Decoder Initialization:
  - Receives [94, 5, 33, 1, 6]
  - Attention: [.1, .9, 0, 0, 0] (Attention on "cat")
  - Prediction: "Chat"

</v-clicks>

<v-clicks depth="2">

- Encoding: "Black cat ate mouse # Chat" => [44, 1, 32, 56, 22, 103]
- Decoder Initialization:
  - Receives [44, 1, 32, 56, 22, 103]
  - Attention: [.7, .2, .1, 0, 0, 0] (Attention on "Black")
  - Prediction: "noir"
- Etc.

</v-clicks>


<!--
We start with naive statisical prediction and see that that is too expensive,example
-->

---

<transform scale="0.75">

![](/attn.png)

</transform>

---

## Strengths and Weaknesses
--
### Strengths
- Logic appears to spontaneously emerge
- Attention allows for long term memory
- Generalization and adaptability

--

### Weaknesses
- Prioritizes fluency over correctness
- Convincing hallucionations
- Bias
- No active learning
- Overfitting
- Prompt Injection

<!--
Give prompt injection link
Definition of react framework
-->

---

## Q&A and Demos

### Further Reading & Sources
- [Neural Network in 5 Minutes](https://www.youtube.com/watch?v=bfmFfD2RIcg)
- [Why Neural Networks can learn (almost) anything](https://www.youtube.com/watch?v=bfmFfD2RIcg)
- [Neural Net Playground](https://playground.tensorflow.org/)
- [Neural Net Playground Explained](https://www.druva.com/blog/understanding-neural-networks-through-visualization)
- [ConvNetJS](https://cs.stanford.edu/people/karpathy/convnetjs/)
- [Neural Net Graph Generator](http://alexlenail.me/NN-SVG/index.html)
- [Attention Mechanism Overview](https://www.youtube.com/watch?v=fjJOgb-E41w)
