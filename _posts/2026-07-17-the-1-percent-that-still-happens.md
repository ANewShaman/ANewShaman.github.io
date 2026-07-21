---
layout: post
title: "The 1% That Still Happens"
date: 2026-07-21
categories: machine-learning
---

I thought I was done with this idea.

Two posts back, I asked what the remaining 1% represented for a model.

One post back, I asked what it represented for a person.

I assumed that was the end of it.

Understand the statistic. Understand the experience behind the statistic. Move on.

Then it happened again.

Not to me, exactly. But to a system I'd built, on a problem I actually cared about, months after I thought I'd already learned this lesson.

---

A model I was working on had been performing well for weeks. Calibrated. Monitored. Evaluated against more than just a single accuracy figure — everything I'd argued for across the last two posts.

And then it made a mistake it had no business making.

Not a borderline case. Not something ambiguous.

A mistake a first-year student would have caught.

My first instinct was to treat it as a bug. Something to patch. I went looking for the cause the way you look for the cause of any failure — assuming there had to be one, some specific thing that went wrong this time that hadn't gone wrong the other 999 times.

There wasn't.

The model was doing exactly what it had always done. It had simply landed, this one time, on the wrong side of a probability it had been carrying around all along.

I sat with that for longer than I expected to.

---

That's worth making a little more precise.

Because **"carrying around a probability"** isn't just a turn of phrase.

If a model is right 99% of the time, each prediction is basically a coin flip weighted 99-to-1. Over a batch of 1,000 predictions, you don't get exactly 10 errors. You get somewhere around 10, with some natural spread above and below.

Seven errors one week. Thirteen the next.

That's not the model degrading.

That's the variance the number always had. It just wasn't visible when I only ever looked at the aggregate, at the single 99% printed at the end of a training run.

The mistake I made wasn't statistical.

It was mistaking a normal draw from a known distribution for a violation of it. I had read the number correctly and still reacted to it incorrectly, which is a strange kind of error to catch yourself making.

---

The same goes for confidence.

A model saying "99% sure" is making a checkable claim, not just a reassuring one. Gather every prediction where it said 99%, and roughly 99% of those should actually be correct.

When that stops holding, there's a name for it.

**Calibration.**

It's a separate failure from accuracy. A model can score well on one and badly on the other. Guo et al. found that a lot of modern deep networks are exactly this — accurate, but quietly overconfident, especially compared to the shallower networks that came before them. More capacity, it turns out, doesn't automatically buy you more honesty about uncertainty. Sometimes it buys you the opposite.

Here's why that distinction matters.

An honest 1% and a hidden 1% look identical in an accuracy report. Both say 99%. Both round to the same headline. They only stop looking identical once you check whether the confidence scores attached to each prediction were telling the truth, and by then most people have already stopped looking.

---

There's a similar story with the confusion matrix. It tells you *where* a model's errors concentrate, not just how many there are.

Take the classic example.

A screening model where 99 out of 100 patients are healthy. Predicting "healthy" every single time gets you 99% accuracy, and zero ability to catch the one patient who's actually sick.

Accuracy treats every mistake as equally costly.

Almost nothing important actually is.

That's why fields like medicine report sensitivity and specificity separately instead of one blended number. Missing a real case and raising a false alarm are not the same mistake, even when accuracy scores them as if they were. A false alarm costs you a follow-up test. A missed case costs something you can't undo.

I think this is where the first post and this one finally meet. Error consistency asked whether two 99%-accurate models fail on the same examples or different ones. Cost asks whether all those failures are worth the same amount, once they happen. Neither question shows up if you only ever report the single number at the top.

---

So what do you do once you've accepted that the 1% is not only real, but statistically expected to keep showing up, in every model you'll ever ship?

I don't think the answer is to chase it toward zero.

Some of it is genuinely irreducible — ambiguous input, edge cases even a careful human would get wrong. Squeezing a benchmark number tighter usually just means overfitting to the dataset in front of you, learning the noise in that particular sample rather than anything that generalizes.

I think the better answer is simpler.

Design around the fact that it *will* happen, instead of being surprised each time it does.

That means routing low-confidence predictions somewhere other than straight into a decision, whether that's a second model, a human reviewer, or an honest "unsure" flag that says the system itself isn't sure enough to be trusted here. It means watching the error rate over time instead of checking it once at deployment and calling it done, since a shift in that rate is a signal worth investigating, not noise to be smoothed over. It means reporting accuracy with some sense of how much data it's based on, because "99% accurate on 100 examples" and "99% accurate on 100,000" are not the same claim, even though they're the same number on the slide.

None of that closes the actual gap, though.

---

Even a model that's well-calibrated, properly monitored, and sensibly routed around its own uncertainty still eventually produces a case that lands on the wrong side of the threshold.

And that case is a person. A diagnosis. A decision that didn't go the way the odds suggested.

The math can tell you how often that will happen.

It can tell you how confident you should have been each time.

It can't tell you what you owe the one it happened to.

I don't think that's a flaw in the math.

I think it's just where the math stops being the relevant tool, and something else has to take over — a process, a person, a policy for what happens next, none of which live inside the model itself.

What surprised me most, going back through this a third time, wasn't that the 1% happened again.

It's that knowing the exact shape of the distribution didn't make the individual outcome feel any smaller. I could tell you precisely how likely it was. I still had to sit with it exactly as much as if I hadn't known at all.

Maybe that's the actual lesson, buried under two posts' worth of metrics.

Understanding uncertainty doesn't shrink it.

It just tells you, in advance, exactly how much of it you're going to have to carry.

The 1% was never the part you eliminate.

It's the part the model can quantify, and the part it can't carry for you.

---

### References

* LeCun, Y., Bottou, L., Bengio, Y., & Haffner, P. *Gradient-Based Learning Applied to Document Recognition.*
* Geirhos, R., et al. *Beyond Accuracy: Quantifying Trial-by-Trial Behaviour of CNNs and Humans by Measuring Error Consistency.*
* Guo, C., Pleiss, G., Sun, Y., & Weinberger, K. Q. *On Calibration of Modern Neural Networks.* Proceedings of the 34th International Conference on Machine Learning (ICML), 2017.
* Gigerenzer, G. *Calculated Risks: How to Know When Numbers Deceive You.*
* Kahneman, D. *Thinking, Fast and Slow.*