Title: Metrics in an AI assisted development process
Date: 2026-08-25 12:15
Tags: ai, leadership, measurement
Category: Leadership
Slug: build-sdlc-ai-metrics
Status: published
Summary: Time-to-implementation cut in half. Bug triage down more than half. The organization tripled in size during the same window, a separate initiative improved our specifications, and we fixed our intake process along the way. How much of that metric is due to AI?
Series: Leadership Thoughts
Featured: true

[TOC]

## What we rolled out

In my organization, we rolled out major AI initiatives over about eighteen months. First was AI assistance for day-to-day development for my software engineering teams. Then, AI for my technical product management team. Lastly, directly building AI into the product itself in a way that goes beyond "another help chatbot". Those are three different adoption problems, with three different outcomes, and most of the disappointment I hear from other engineering leaders comes from treating "AI" as one initiative with one number attached.

We started for unglamorous reasons, but probably the most common. The year's business goals were more aggressive than our throughput supported, [technical debt was getting more expensive every quarter][4], and we had committed to keep improving customer experience while both of those were true. Surprise! Welcome to engineering. This is the part I love though. It's a challenge and keeps things exciting, as long as it's managed correctly. Modernizing how the systems actually worked was the only path that closed the gap.

We [grew the organization roughly three times over][3] in during this same 18 month window. This is important to know when reading any number below. Nobody was asked to cover two roles because a model could autocomplete. If your AI rollout is quietly a headcount play, the measurement problem I'm about to describe is the smallest one you have, because your engineers already know and they will optimize for looking busy.

## What I chose to measure (and what I didn't)

Most rollouts measure how developers feel about the tools. I asked too, because you should know whether your engineers hate something you've made them use. I never counted it as an outcome. Self-reported productivity and actual productivity rarely match. People who love a tool overestimate its effect, and people who resent being told to use it will underreport a real gain. I've written about [why that survey number isn't the thing you think it is][1].

The other easy number is volume. Commits, lines, pull requests opened. We deliberately didn't optimize against any of them because they measure activity, not outcome. All of those go up when you hand people a fast text generator, and none of them say whether the business got anything. They are also the most easily gamed numbers, making them interesting but useless engineering metrics to evaluate the team against.

[What I wanted was flow][8]. How long does an idea take to become something a customer can use, and how much of that time are engineers doing work only engineers can do? So, I tracked time-to-implementation from specifications handed off to engineering, through to ready for production. This was the defination I set, and it's the definition I presented to everyone. Notice that I stopped before "in production" because the last leg includes things the team doesn't control, like UAT waiting on business sign-off. The clock started after specifications because my focus at the time was the development teams. I've since extended it backward, to measure how long specifications take to reach a state engineering can accept.

The other number I track is total engineering hours going into bug triage. Bug triage is a nearly pure cost center, it's well-instrumented most places because it runs through ticketing, and it has a short causal chain to the intervention. If a model helps someone reproduce and localize a defect faster, that time savings shows up quickly. 

Year over year: time-to-implementation cut in half. Bug-triage engineering hours down by more than half.

Those are good numbers. 

![Building a metric for time to implementation requires setting the boundries of what's measured.][fig1]

## What the number doesn't show

The halving is a year-over-year comparison in an organization that tripled in size inside the comparison window. The team that produced the second number is not the team that produced the first, and [the people we added were hired against a bar we'd been raising for a year][5]. Some real fraction of that improvement is just better hiring.

The inputs changed too. Another change in the same window improved how specifications get written before engineering ever sees them. Better specified work moves faster whether or not an AI model touches it. That change was worth doing on its own merits, and it landed at the same time as the many other improvements being made. Year-over-year comparisons in a growing org compare two different team, at two different levels of maturity.

You cannot roll out developer tooling across fifty-plus people without also touching how work gets described, reviewed, and handed off. Ticket quality was tightened because the models produced garbage from vague tickets. But, we also tightened ticket quality because we had new team members that didn't have the context to understand a couple sentences thrown into Jira. It's the same problem applied to both new human team members and new AI assistants. We standardized parts of the review process because inconsistent review was where AI-generated code caused the most friction. [Either change would have improved delivery on their own][11], even without AI assistants involved. My guess is that a meaningful share of the improvements seen in the first year was process discipline and it applied to both human and AI at the same time.

[Getting the opportunity to fix your intake and review process is a real outcome][7]. The interesting piece will be seeing how that number improves next, because the team hasn't continued to grow, but AI processes do continue to be implemented. I think we've reached the point where the foundation work set up good processes and allowed the team to set higher quality expectations. I think that the next year is what seperates the foundation from true AI assistance being measurable. 

## What I learned

The team tried to get AI to handle technical debt bugs on its own. Dependency updates work fine: a model can read a changelog, bump a version, run the tests, and open a pull request, and that pattern held up. [We assumed the rest of the technical debt backlog was a harder version of the same job][4]. Months of watching a model try and fail says otherwise.

The failures were productive though. We learned about where guardrails have to sit and what a model does when a task is underspecified. They did not produce working automation. We shelved the effort, wrote down what we learned, and we'll come back to it when we understand the problem better.

Review time goes up on code where the model did all the work and no person was involved. [Faros.ai has a nice study][10] showing that my observations aren't unique; review time goes up for senior engineers. The diffs AI produce are large. Something a person would have changed in twelve lines arrives as several hundred. The model asserts that it works, confidently, until the rest suite fails. We don't encourage one shotting a change, and this is why. Confidence, without evidence, continues to be AI's weakness. The important thing is to build a test suite that you trust and to run that suite. [Don't let the model build tests against code it's already written][9], because it'll write tests that pass. A second, adversarial model, needs to build the tests or a human with an understanding of the requirements needs to build the tests. You _have_ to trust your test suite and if you don't, it's difficult to trust what the AI is building.

Investigation and planning work well. Point a model at a user-reported problem, let it do the first pass of localizing it, and hand an experienced engineer the result. It doesn't need to be a solution to be worth the time it saves, and it's useful when the person on call didn't write the feature they're being paged about. 

## The junior engineer question

Everyone on the team got the same tools and the same training regardless of level. Everyone adopted the same tools and saw the benefit. What I'm watching now is slower than adoption. [Deep context about how our systems actually work isn't accumulating at the rate I'd expect][2]. They still go to the model, or to a senior, for things that should be in their heads by this point.

That sits uncomfortably next to the on call note above. First-pass investigation by a model is most valuable exactly when the engineer looking at the problem has no context on the code in front of them. That's a real win in the middle of the night  when it happens. It also relieves the pressure that used to put context into someone's head.

Unfortunately, we're trading middle of the night convinence for longer term...something. I don't know if it's a problem, but it is a concern. The organization tripled in size, so there is more system to hold in your head than there was and less senior attention per person to help you hold it. Both of those slow knowledge accumulation with no models involved at all.

What I'm watching for is whether this is a lag or a ceiling. If the context arrives six months later than it used to, that's a scheduling problem and I'll change how I scope the work. If it doesn't arrive at all, that's a different conversation, and I don't yet know what I'd do about it.

![Knowledge loss is a problem for junior engineers and AI generated code is a tax on the senior engineers. These are a loop not two different problems][fig2]

The cost to review changes increases on senior engineers. Junior engineers learn about the system at a slower rate. I think these two problems are a loop that is feeding itself, not two different problems to solve.

## What I'd do differently

Instrument first. We had telemetry from before the rollout by accident and buried within complex JIRA queries, not by design. Starting now, I'd spend the first month defining and baselining two or three flow metrics, because the version of this post I'd rather be writing is the one where I can separate the tooling effect from the growth effect. I can't.

I'd separate "a model can do this task" from "a model can do this task without a person in the loop" before committing to any target. Those look like the same claim on paper and they are not the same claim, which is most of what the shelved tech debt work cost us to learn.

I'd budget enablement as a line item from day one. Buying the tools is a small share of the work, and the part that isn't buying tools has no purchase order attached to it.

If you're quoting a number off your own rollout, [go find out what else changed in the same window][6]. You'll end up with a smaller number. You'll also find that the foundational changes compound over time.

[1]: {filename}2026_08_10_ai_productivity_study_update.md
[2]: {filename}2025_08_18_junior_dev_gen_ai_crisis.md
[3]: {filename}2025_04_01_scaling_teams_without_losing_culture.md
[4]: {filename}2025_04_08_tech_debt_strategic_approach.md
[5]: {filename}2026_03_25_ai_broke_my_interviews_i_fixed_them.md
[6]: {filename}2026_07_06_red_flags_engineering_organizations_week_two.md
[7]: {filename}2026_04_30_management_debt_leadership_patterns_rot.md
[8]: https://dora.dev/guides/dora-metrics/
[9]: https://www.anthropic.com/engineering/claude-code-best-practices
[10]: https://www.faros.ai/blog/ai-acceleration-whiplash-takeaways
[11]: https://blog.google/innovation-and-ai/technology/developers-tools/dora-report-2025/
[fig1]: {attach}images/ai-sdlc-metric-development.png
[fig2]: {attach}images/ai-sdlc-junior-senior-loop.png