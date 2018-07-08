---
layout: post
title:  "TL;DR of the paper 'High-Performance Procedural Noise using a Histogram-Preserving Blending Operator'"
date:   2018-07-07
tags: ['tldr', 'graphics', 'noise', 'texture']
author: "Hugo Viala"
---

Inspired by the work of [Eric Arnebäck](https://twitter.com/erkaman2), I wanted to try my own TLDR of graphics acamedic papers. In here, I want to describe the work of Eric Heitz and Fabrice Neyet in the paper [High-Performance Procedural Noise using a Histogram-Preserving Blending Operator](https://hal.inria.fr/hal-01824773/document). Please note that the images are directly taken from the paper and are not mine.

# The problem

Let's say an artist made a texture for the ground in your game, but he only did a small patch of the ground and you want to have the pattern repeat on a ground that is way larger than what the artist drew. You could have the texture repeat in a very simple fashion but this would be dull and ugly. What you really want is a fast way to generate a huge ground texture based on the feature of the smaller texture that the artist made.

![Example. On the left, small input textures. On the right, generated bigger textures.](/images/histogram_article/hist_example.png)
<center>Example. On the left, small input textures. On the right, generated bigger textures.</center>

So this is our problem, you are given a small noisy texture with high-frequency features and you want to generate quickly a much bigger texture from it.

# Blending textures

One basic method for this kind of problem is to do the following : let's say you want to know the final color of one pixel of the final texture, you will pick three points from the initial texture and say that the final color will be a mix, a blending of the three initial colors. If we repeat this operation for each pixel in the final texture, we will have an output that looks a lot like the initial smaller texture. This computation could happen in a fragment shader so that we can generate everything at runtime.

![Blending example](/images/histogram_article/hist_blending.png)
<center>Showing how the blending works. We take three input patches and we blend between them with our operator to have the ouput.</center>

The question is now : how do we blend the three initial pixels together ?

If we affect weights $w_1$, $w_2$ and $w_3$ to the pixels $X_1$, $X_2$ and $X_3$, one classic solution would be to do a linear blend between those to compute the final pixel $X$

$$ X' = w_1 X_1 + w_2 X_2 + w_3 X_3 $$

But this solution does not work because this generates colors and contrasts in the final texture that are not present in the initial texture.

Therefore, the paper states that what needs to be preserved by the blending operation is the histogram of the initial texture.

# Preserving the histogram

The histogram of a texture is the curve that gives us the number of pixels that have a specific intensity on the red, green or blue channel. If the initial texture's histogram and the final texture's histogram are almost the same, then we will have a satisfying result[^1]

![Input Images with their respective histograms](/images/histogram_article/hist_hist.png)
<center>Input textures with their respective histograms</center>

So now our problem is : what is the blending operator that preserves the texture's histogram ?

The authors note that there is a special case in which this problem is very easy : if the histogram corresponds to a Gaussian distribution of the colors, then a variance-blending operator exactly preserves the histogram. This is well-known and quite easy to compute[^2]. The problem is that, in the general case, the input color does not have an histogram that has a Gaussian distribution.

But, if we could transform our input image so that it has a Gaussian distribution, we could use our histogram-preserving operator to generate a bigger texture. This bigger texture would also have a Gaussian distribution, and then we could use the inverse transformation to go back to the initial histogram, but with the output texture. And we would win.

More formally, if we have an initial pixel of color $X$, that follows of initial distribution histogram $H$, we need to find an operator $T$ that transforms $X$ to have a pixel $G$ of Gaussian distribution.

$$ G = T(X)$$

If we have our blending operator $f$ that preserve the Gaussiam histogram, then 

$$ f(G) = f(T(X))$$

also have a Gaussian distribution. To get our final pixel color $X'$ we just do the inverse operation of $T$, which is $T^{-1}$.

$$ X' = T^{-1}[f(G)] = T^{-1}[f(T(X))]$$

This will effectively turn the Gaussian distribution into our initial histogram, but on a bigger texture. And we are done. Our histogram-preserving blending operator $f$ is well-known. So our question is now : what exactly are $T$ and $T^{-1}$?

$T$ must be an operator that transforms our input image so that it has a Gaussian distribution. This is a well-known complex problem and one possible solution is given by an Optimal Transport method[^3]. Explaining the Optimal Transport is out of the scope of this post, but all we have to know is that, with this method, we can transform an image of any histogram into another image with an histogram of our choice, the problem is that this computation is very slow. Yet it could do the job if we use it smartly.

# The final algorithm

Now every elements are in our hand, $f$, $T$, and $T^{-1}$. But, as I said previously, $T$ is very costly to compute. So here is the method that the authors propose :

* In an offline pass, precompute $G = T(X)$ with the Optimal Transport method. So have the input transformed with a Gaussian distribution already at hand.

![Step 1 of the algorithm](/images/histogram_article/hist_step1.png)
<center>Step 1 of the algorithm</center>

* Since we will need to compute $T^{-1}$ at runtime and since it is also costly to compute, precompute some value of $T^{-1}$ with the Optimal Transport method and store it in a look-up table.
* At runtime, we have $G$ that has a Gaussian distribution, we use $f$ to blend the three initial pixels while preserving the Gaussian distribution to get $f(G)$, and we use the precomputed look-up table to apply it to $T^{-1}$. This step can be done in the fragment shader.

![Step 2 of the algorithm](/images/histogram_article/hist_step2.png)
<center>Step 2 of the algorithm</center>


After the third step, we have effectively computed: 

$$T^{-1}[f(G)] = T^{-1}[f(T(X))] = X'$$

![Step 3 of the algorithm](/images/histogram_article/hist_step3.png)
<center>Step 3 of the algorithm</center>

And we have our final color that follows the same histogram as the input color. Tadam.

To conclude, we have in our hand a fast method to generate big textures that have the same histogram and therefore the same features as the smaller input textures. The authors have provided an explanative [video](https://drive.google.com/file/d/1YS8RHNcYff7ReroiBbeXHZ79L0_KrSK6/view) to show some impressive results in real-time.

---

[^1]: A better explanation of color histograms can be found [here](https://thecoffeelicious.com/a-photographers-guide-to-color-histogram-e31a5d92efb2).
[^2]: The exact formula for this operator is: $$X' = \frac{\sum w_i X_i - \mathbb{E}[X]}{W} + \mathbb{E}[X]$$ where $$W = \sqrt{\sum w_i^2}$$
[^3]: Optimal Transport is a huge topic. There is a [book](http://cedricvillani.org/wp-content/uploads/2012/08/preprint-1.pdf) written by Cedric Villani on the subject, and you can find a reference implementation by Bonneel [there](https://perso.liris.cnrs.fr/nicolas.bonneel/FastTransport/).
