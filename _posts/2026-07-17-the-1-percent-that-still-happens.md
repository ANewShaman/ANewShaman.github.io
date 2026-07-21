---
layout: post
title: "The 1% That Still Happens"
date: 2026-07-21
categories: machine-learning
---

I thought I was done with this idea.

Two posts back, I asked what the remaining 1% represented for a model.

One post back, I asked what it represented for a person.

I assumed that was the end of it. Understand the statistic. Understand the experience behind the statistic. Move on.

Then it happened again.

Not to me, exactly. But to a system I'd built, on a problem I actually cared about, months after I thought I'd already learned this lesson.

---

Here's roughly what happened.

A model I was working on had been performing well for weeks. Calibrated, monitored, evaluated against more than just a single accuracy figure — everything I'd argued for in the last two posts. And then it made a mistake it had no business making. Not a borderline case. Not an ambiguous one. A mistake a first-year student would have caught.

My first instinct was to treat it as a bug. Something to patch.

But nothing was broken. The model was doing exactly what it had always done. It had simply landed, this one time, on the wrong side of a probability it had been carrying around all along.

That was the uncomfortable part. I already knew this could happen. I had written about it happening. And I still reacted like it was a surprise.

---

I think there's a reason for that.

Understanding uncertainty in the abstract is not the same as sitting with it in practice.

It's easy to accept that a well-calibrated model will be wrong roughly one time in a hundred. It's a different thing entirely to watch that one time arrive, on a Tuesday, in your own project, with your name on the commit.

The math hadn't changed. My relationship to the math had.

---

This is, I think, where a lot of people quietly stop trusting probabilistic systems, even after intellectually accepting them.

Not because the systems failed to perform as promised.

Because performing as promised includes failing sometimes, and living through that failure feels different from reading about it.

Gigerenzer writes about this gap between how we calculate risk and how we feel it. Kahneman writes about how we generally reason far more with our reactions to outcomes than with the probabilities that produced them. Neither of them are talking about machine learning specifically. But the pattern transfers cleanly. A number can be perfectly correct and still land badly.

---

So what do you actually do with that?

Not rhetorically. Practically. If the 1% is guaranteed to keep happening no matter how good the system gets, what changes?

I don't think the answer is to try harder to prevent it. Some of that 1% is genuinely irreducible — the ambiguous handwriting, the atypical presentation, the case that would confuse a competent human too. Chasing it to zero usually means overfitting to the dataset in front of you rather than building something more honest.

I think the answer is closer to building for the failure, not just around it.

That means the confusion matrix isn't a one-time diagnostic you run before deployment — it's something you keep watching. It means confidence scores aren't just reported, they're acted on: a low-confidence prediction should trigger something different downstream than a high-confidence one, whether that's a second opinion, a human reviewer, or simply a flag. It means the system is designed with the assumption that the 1% will show up, rather than the hope that it won't.

Model monitoring exists for exactly this reason. Not because the model is expected to degrade, but because the guarantee was never "always right." It was "right about this often, under these conditions" — and someone has to keep checking that the conditions still hold.

---

There's also a harder question underneath this, one I don't have a tidy answer for.

When the 1% happens to someone, does it matter that the system was honest about the odds beforehand?

I want to say yes. A well-calibrated model that told you upfront it was 99% confident, and was wrong anyway, is a fundamentally different kind of failure than an overconfident model that insisted it couldn't be wrong. One is a system being honest about uncertainty. The other is a system lying about it.

But I'm not sure that distinction is much comfort to the person on the receiving end of the outcome. The forecast being honest about the 1% chance of rain doesn't make you any less wet.

Maybe that's fine. Maybe honesty about uncertainty was never supposed to make the outcome feel better. It was only ever supposed to make the decision-making around it more defensible — for the designer, the doctor, the reviewer, whoever has to explain afterward why the system was trusted in the first place.

That's a colder kind of comfort than I'd like. But I think it's the honest one.

---

So here's where I've landed, three posts into a question I thought would take one.

Accuracy hides behaviour. Confidence hides certainty. And even once you've accounted for both, the 1% doesn't go away — it just becomes something you've agreed to build around instead of something you're surprised by.

I used to think the goal of evaluation was to shrink that number.

Now I think the goal is to make sure that when it happens — and it will happen — the system, and the people relying on it, aren't caught off guard.

The 1% was never the part you eliminate.

It's the part you design for.

---

### References

* LeCun, Y., Bottou, L., Bengio, Y., & Haffner, P. *Gradient-Based Learning Applied to Document Recognition.*
* Geirhos, R., et al. *Beyond Accuracy: Quantifying Trial-by-Trial Behaviour of CNNs and Humans by Measuring Error Consistency.*
* Guo, C., Pleiss, G., Sun, Y., & Weinberger, K. Q. *On Calibration of Modern Neural Networks.* Proceedings of the 34th International Conference on Machine Learning (ICML), 2017.
* Gigerenzer, G. *Calculated Risks: How to Know When Numbers Deceive You.*
* Kahneman, D. *Thinking, Fast and Slow.*
* Amodei, D., et al. *Concrete Problems in AI Safety.*