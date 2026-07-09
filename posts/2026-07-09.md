---
layout: post
title: "99% Accuracy Isn't the Interesting Part"
date: 2026-07-09
categories: machine-learning
---

Like many people getting into deep learning, one of the first models I trained was a Convolutional Neural Network (CNN) on MNIST. The objective seemed simple. Train the model, tune it a little, and get the highest accuracy possible. That's also how most tutorials are structured. The architecture changes. The optimizer changes. The hyperparameters change. But the conversation almost always ends with one number.

**Accuracy.**

My model reached around 99%. Just like countless implementations before it. Objectively, that's a good result. But after looking at that number for a while, I realised it wasn't answering the question I had started asking myself.

**What does the remaining 1% actually represent?**

At first, that felt like an odd question. After all, MNIST is a benchmark of handwritten digits. Every incorrect prediction carries the same penalty. Calling a handwritten **3** a **5** is one mistake. Calling a **7** a **1** is another. Every error contributes equally to the final score, which is exactly why accuracy works so well as a benchmark metric.

Accuracy is probably the most intuitive evaluation metric in machine learning. It tells us how many predictions the model got right out of the total number it made. It's easy to compute, easy to compare, and easy to understand.

The problem isn't that accuracy is a bad metric.

The problem is that it's sometimes the **only** metric we look at.

That sounds like a subtle distinction, but it completely changes how you look at a model. Accuracy tells me how often my CNN is right. It doesn't tell me which digits confuse it. It doesn't tell me whether it repeatedly makes the same mistakes. It doesn't tell me how confident it was when it made those mistakes. And it certainly doesn't tell me whether those mistakes resemble the kinds of mistakes a human would make.

That was the point where I realised I wasn't really interested in improving the number anymore.

I was interested in understanding the behaviour behind it.

The limitation of accuracy becomes much clearer once you leave benchmark datasets behind.

Imagine a medical diagnosis model where 99 out of 100 patients are healthy. A model that predicts **"healthy"** for every patient would achieve 99% accuracy. On paper, that sounds excellent. In reality, it has failed at the very task it was built for: identifying patients who are actually sick.

The mathematics isn't wrong. Accuracy is doing exactly what it was designed to do. It's measuring the overall proportion of correct predictions. What it isn't measuring is the importance of different kinds of mistakes.

That made me wonder whether I was asking the wrong question while looking at my own MNIST model.

Instead of asking,

> **"How accurate is it?"**

I started asking,

> **"How does it fail?"**

Looking into that question led me to *Beyond Accuracy* by Robert Geirhos and colleagues, along with revisiting Yann LeCun's original MNIST paper. They didn't argue that accuracy was unimportant. Quite the opposite. Accuracy remains one of the most useful evaluation metrics we have. Their argument was that it shouldn't be treated as the complete story.

That idea resonated with me.

Accuracy tells us **how often** a model is correct. It says much less about **which** examples it gets wrong, **why** it gets them wrong, **how confident** it is when it is wrong, or whether those mistakes resemble the kinds of mistakes humans make.

One concept that particularly stood out to me was **error consistency**.

Imagine two CNNs that both achieve 99% accuracy on MNIST.

If we only compare their final score, they appear identical.

But are they?

One model might consistently struggle with messy handwritten **4s** and **9s**. Another might confidently misclassify a completely different subset of digits. They have the same accuracy, yet they have learned different decision boundaries and exhibit different behaviour.

Accuracy alone cannot tell us that.

The same applies to confidence.

Suppose two models misclassify the exact same digit.

One predicts the wrong class with 51% confidence.

The other predicts it with 99.99% confidence.

Accuracy records both as one incorrect prediction.

As a human reading those outputs, however, they tell two very different stories. One model was uncertain. The other was almost completely convinced it was right.

That naturally led me to another question.

Do models fail in the same places humans do?

Research suggests that the answer is more nuanced than I expected. Some handwritten digits are genuinely ambiguous. Even humans may disagree on what the correct label should be. In those cases, a model making the same mistake doesn't necessarily indicate poor performance. At the same time, researchers have also shown that models often make mistakes that humans would not, despite achieving similar overall accuracy.

Again, a single percentage cannot capture that distinction.

Even something as simple as a confusion matrix begins to reveal information that accuracy hides. Instead of telling us that a model is "99% accurate," it tells us *how* it arrived at that number. Which digits are repeatedly confused? Are there systematic patterns? Is one class consistently more difficult than the others?

Those questions reveal behaviour.

The more I read, the more I realised that evaluation isn't about replacing accuracy with another metric.

It's about recognising that no single metric can fully describe how a model behaves.

Accuracy tells us **how often** a model succeeds.

Confidence tells us **how certain** it was.

A confusion matrix tells us **where** it struggles.

Error consistency tells us **whether those struggles resemble human behaviour**.

Each metric answers a different question.

MNIST is one of the simplest datasets in machine learning. The consequences of getting a digit wrong are practically nonexistent. Yet it introduces an idea that carries into much larger problems.

When models begin assisting doctors, driving cars, approving loans, or screening résumés, understanding *how* they fail becomes just as important as understanding *how often* they succeed.

That, more than the 99% itself, was my biggest takeaway from implementing a CNN on MNIST.

Before this, I thought evaluation ended once the accuracy was reported.

Now I think that's where it begins.

---

### References

* LeCun, Y., Bottou, L., Bengio, Y., & Haffner, P. *Gradient-Based Learning Applied to Document Recognition*.
* Geirhos, R., et al. *Beyond Accuracy: Quantifying Trial-by-Trial Behaviour of CNNs and Humans by Measuring Error Consistency*.

**Is this too much for MNIST?**

Yes.

Yes, it is.

But perhaps that's exactly why a dataset of 28×28 handwritten digits is still teaching us new ways to think about machine learning nearly three decades later.