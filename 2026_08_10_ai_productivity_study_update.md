Title: A Developer Survey Is Not a Measurement of Developer Productivity
Date: 2026-08-10 01:30
Tags: job, leadership
Category: Leadership
Slug: metr-ai-productivity-study-update
Status: published
Summary: A year ago I cited METR's finding that AI made experienced developers 19% slower. They ran it again with an error bar so wide it no longer means anything. Here's what it means for the junior engineer problem I keep coming back to.
Series: Leadership Thoughts

[TOC]

## Where the 19% came from

Almost exactly a year ago, I wrote about a [randomized controlled trial by METR][2] that measured open source developers 19% slower with AI than without. As part of [my observation that AI assistants are decimating the "Junior Engineer" role][1] at companies, this piece of information didn't help my view.

I've seen this in my own hiring, and touched on [improvements my teams have made utilizing AI assistants][4] in a post a few months ago. The story is the same though, while I've continued to hire for roles on my teams, none of those have been junior engineering roles. 

I have a planned follow up to the Junior engineering crisis we are currently in, but let's get back to that METR study. METR started running it again in August 2025, and in February 2026 [published what came back][3]. Let's talk about this...

## What the first study measured

Those 16 developers worked 246 tasks. The repositories averaged more than 22,000 stars and a million lines of code, and the developers had been contributing to them for years. Tooling was Cursor Pro with Claude 3.5 and 3.7 Sonnet.

Every one of those variables impacts the result. 16 developers is a small sample. A repository you've worked in for years is similar to a senior engineer at a company working on the same code base for years. They already hold the context an assistant would otherwise supply. Inline code suggestions in early 2025 are a different way of working than an agent running on its own for 20 minutes today.

## What happened when they ran it again

The [follow-up][3] was a bigger study with 57 developers, 143 repositories, more than 800 tasks.

For the 10 developers who had also been in the original study, the estimated effect was an 18% slowdown, with a confidence interval running from 38% slower to 9% faster. For the 47 newly recruited developers, 4% slower, with an interval from 15% slower to 9% faster.

18% against the original 19%. The first result came back almost exactly the same. But the confidence interval around it changed. The original ran from 2% slower to 39% slower and excluded zero. Both of these new results cross it, which means the study can no longer separate the effect from nothing at all.

So this is not a retraction, and it isn't a failure to land on the same number. It's a measurement that came back the same size and stopped meaning anything. METR spent that post explaining why they no longer trust their own instrument.

## Why METR stopped trusting the measurement

So what went wrong? METR lists six problems, names two of them as the important ones, and most of them follow from AI getting better over the past year to year and a half.

Three have the shape of a selection effect, and they all point the same direction. Developers started declining to participate once there was a chance they would be assigned to work without AI, so the study systematically missed the people most optimistic about it. Among those who did participate, 30% to 50% told METR they were holding back specific tasks because they didn't want to do those tasks without AI, so it systematically missed the work with the most to gain. And developers were less likely to finish a task at all once it was assigned to the no-AI condition. One of them completed none of theirs. Pay dropped from $150 an hour to $50 an hour over the same stretch, which METR believes contributed, though they put developers' expectations about AI ahead of money as the cause.

A fourth problem isn't selection at all, it's the clock. Developers told METR that timing a task got hard once agents were involved, because a developer waiting on an agent tends to go work on something unrelated while it runs.

Look at the direction of the first three. Refusing to work without AI, withholding the tasks where AI helps most, and abandoning the ones that landed in the wrong condition all push the measured result toward slowdown. The bias in the experiment becomes fairly obvious.

METR's own conclusion says:

> Based on conversations with study participants, we believe it is likely that developers are more sped up from AI tools now - in early 2026 - compared to our estimates from early 2025. However, because of the selection effects in our experiment, our data is only very weak evidence for the size of this increase.

They are now redesigning around developer-level randomization, shorter experiments, and observational data.

## What held up?

One result from the original study has held up better than the 19%.

Before starting, those developers predicted AI would make them 24% faster. After finishing the tasks, having done the work, they still believed they had been sped up by about 20%. The clock said 19% slower.

Watch out for that difference though. It's calculated from the same bias I mentioned, and the selection effects that pushed the measured result toward slowdown would widen it. METR didn't formally re-measure perception in the follow-up.

Under each condition METR has tested, developers' estimates of their own speedup have sat above what measurement showed, and METR now warns about the difficulty of interpreting self-reported productivity estimates "and thus their potential biases." This also isn't surprising to me. I've sat in enough developer sprint cycles, planning sessions, retros, quarterly plannings and other agile ceremonies to know that estimating effort is an inexact science. It takes time, experience as a developer and more importantly, experience with the system to be able to accurately estimate how much time and effort a change will take.

A developer survey is not a measurement of developer productivity. Ask your team how much AI is helping and you'll get a real answer about morale and an unreliable one about throughput. I have never once seen a self-reported productivity number survive contact with an actual measurement. I see no reason leaders are exempt from the same gap. We sit further from the work than the people we're asking.

The METR developers were an unusual population. Deep existing context, mature code, high standards, work they had been doing for years. My read has been that an assistant supplying context to someone who already has all of it isn't worth much. In short, they were "Senior Developers".

Daniotti and colleagues, [publishing in Science in January][5], ran a classifier over more than 30 million commits from 160,000 developers in six countries. It put AI-written Python functions in the US at 29% by the end of 2024, up from 5% in 2022, with a 3.6% rise in quarterly commit rates alongside it. That's positive, and much smaller than what's being sold. It also finds that the gain lands somewhere specific:

> GenAI increases output and helps programmers expand into new domains - but only for senior-level developers. Early-career developers, despite being the most enthusiastic adopters, see no measurable gains.

That is the reverse of the ordering I just described, so my interpretation is that that the two studies aren't measuring the same thing. METR timed a few hundred self-chosen tasks inside million-line repositories its participants had maintained for years. Daniotti counted commits across an entire population, where "senior" means a few years of history rather than maintainer of a 22,000-star project. On mature code, with people who already hold the context, measured in wall-clock time, the gain is not obviously there.

Inside my own organization I write down the _conditions_ before I write down the number. Which teams, which kind of work, measured how, over what period, against what baseline. I learned this [flagging spam a decade ago][7], where I published human accuracy at 85.7% before I let a classifier act on a live queue, because a machine that beats nothing in particular isn't an improvement.

An engineering leader who can state their AI gain but not the conditions that produced it is telling you a story. Ask how the rollout is going and you should get numbers back, [the same way you should from any healthy organization][6].

The first study ran on Cursor with Claude 3.5 and 3.7 Sonnet. That was 18 months ago. If you're still citing the number that came out of it, read what METR has published since.


[1]: {filename}2025_08_18_junior_dev_gen_ai_crisis.md
[2]: https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/
[3]: https://metr.org/blog/2026-02-24-uplift-update/
[4]: {filename}2026_04_06_review_claude_code_for_python_developers.md
[5]: https://www.science.org/doi/10.1126/science.adz9311
[6]: {filename}2026_07_06_red_flags_engineering_organizations_week_two.md
[7]: {filename}2017_02_19_can-a-machine-be-taught-to-flag-spam-automatically.md
