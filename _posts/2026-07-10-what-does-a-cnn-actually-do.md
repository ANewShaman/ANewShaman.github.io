---
layout: post
title: "What Does a CNN Actually Learn?"
date: 2026-07-10
categories: machine-learning
---

After writing about accuracy, I found myself asking a related question. If a model keeps confusing certain digits, what did it actually learn that caused those mistakes?

When I first trained a CNN, I imagined there was a neuron somewhere that had learned "what a 7 looks like." That's not what happens.

A CNN doesn't learn concepts. It learns representations. The two are not the same thing.

**Pixels First**

To us, a 7 is obviously a seven, regardless of font, slant, or handwriting. We've built an abstract concept of what a seven is over a lifetime of seeing them.

A CNN starts with none of that. To the network, an image is just a grid of numbers. 28 by 28. 784 values. No labels attached.

Before it can classify anything, it has to answer a more basic question: how should this image even be represented?

**Representations, Not Features**

We usually say CNNs "learn features." That's true, but a better way to put it is that they learn a sequence of representations.

Raw pixels are one representation. The output of the first layer is another. Each layer rewrites the previous one. Not because it was wrong. Because the new version makes classification a little easier.

The first layers don't detect digits. They detect edges, corners, small strokes, local contrast. None of that resembles a number on its own. But it's the raw material the next layer needs.

As the network gets deeper, those simple patterns combine. Edges into curves. Curves into loops. Small structures into larger ones. By the later layers, the representation is abstract enough that a simple classifier can finish the job with little effort.

The classifier was never the hard part. Getting the representation right was.

**Reorganizing Space**

This is the part that changed how I think about the whole process.

Plot every MNIST image in a space defined only by pixel values, and images of the same digit don't naturally sit together. Handwriting scatters them everywhere. Each layer moves the data into a new space, and it does this on purpose. Similar digits get pulled closer together. Different ones get pushed apart.

By the last hidden layer, that space has a name: an embedding. It's just a vector, a fixed list of numbers. But it's a coordinate system the network built for itself, one layer at a time, specifically so that classification becomes trivial by the end.

A CNN isn't recognizing digits so much as reorganizing space until recognizing digits becomes the easy part left over.

**What Feature Maps Actually Are**

Filters, feature maps, activation maps, Grad-CAM. It's tempting to treat these as what the network is "seeing." They're not.

Filters are learned weights. Feature maps are what those weights produce on a given input. Grad-CAM highlights which pixels mattered most to a prediction.

All of this is useful. But useful for us. The network doesn't consult any of it. It's just moving tensors from one representation to the next.

**Why This Matters for Failures**

A misclassified digit isn't just a wrong answer. It's a sign that the embedding wasn't good enough to separate that example from a neighboring class.

Looking at failures isn't only about evaluating predictions. It's a way of looking at the representation itself, and where exactly it broke down.

**How Much of This Does MNIST Actually Need?**

Fair question, worth answering honestly.

A plain linear classifier on raw pixels gets to about 92%. A shallow network with no convolutions at all gets to around 97-98%. A CNN pushes past 99%. Most of the accuracy on this dataset doesn't require any of the machinery described above. MNIST is centered, cropped, and clean enough that a straight line through pixel space gets you most of the way there.

What depth actually buys you isn't the accuracy number. It's robustness. Shift the digit a few pixels, rotate it slightly, and a linear model degrades much faster than a CNN does. A CNN's early layers don't care exactly where in the image a stroke sits. A linear model does.

**Is This Too Much for MNIST?**

Yes.

Yes, it is.

A dataset this simple doesn't need a theory of representation learning to be solved. But maybe that's exactly why it's a good place to notice the idea in the first place. Nothing about MNIST is complicated enough to hide it.

---

### References

* LeCun, Y., Bottou, L., Bengio, Y., & Haffner, P. *Gradient-Based Learning Applied to Document Recognition* (1998).
* Zeiler, M. D., & Fergus, R. *Visualizing and Understanding Convolutional Networks* (ECCV 2014).
* Olah, C., Mordvintsev, A., & Schubert, L. *Feature Visualization* (Distill, 2017).
* Bengio, Y., Courville, A., & Vincent, P. *Representation Learning: A Review and New Perspectives* (IEEE TPAMI, 2013).