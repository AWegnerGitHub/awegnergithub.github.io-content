Title: Management Debt: When Leadership Patterns Rot
Date: 2026-04-30 13:15
Tags: job, leadership
Category: Leadership
Slug: management-debt-leadership-patterns-rot
Status: published
Summary: Technical debt has a sibling that gets less airtime and, in my experience, does at least as much damage. Decision rights that drift, skip-levels that quietly fall off the calendar, review meetings that turn into status updates. Let's talk about how I clean them up once I notice they've gone bad.
Series: Leadership Thoughts
Featured: true

[TOC]

## Introduction

Engineering leaders talk about [technical debt][1] frequently. We track it, we visualize it, we plan around it, we explain it to executives. There's a related kind of debt that gets a fraction of that attention and, in my experience, does at least as much damage to engineering organizations. I've come to think of it as management debt - the engineering-leadership flavor of what [recent research calls organizational debt][2]: the accumulation of outdated structures, policies, and processes that quietly hinder how a company operates.

By management debt I mean the gap between how leadership is supposed to operate and how it actually operates. Decision rights that have quietly migrated to whoever speaks up first. Skip-levels that became calendar holds nobody runs. Quarterly planning sessions that turned into backwards-looking status reviews. Performance feedback that gets saved up for the yearly review cycle because nobody had the harder conversation when a problem occurred.

None of that looks like failure in the moment. It looks like a function nobody changed for three years. It still works, technically. Every change to it just costs more than it should.

## How management debt accumulates

Most of the management debt I've seen didn't enter the organization through a bad decision. It entered through a deferred one. It entered through a "best at the time" decision that was never revisited.

A skip-level got cancelled because a customer escalation came up. The next month it got cancelled again for similar reasons. After that it quietly fell off the calendar, "until things calm down." But, things never calm down. Six months later the engineering manager is making decisions that the director should be involved in, and the director doesn't know those decisions are being made.

The same pattern shows up in design reviews. A team starts skipping the architecture review for "small" changes. Over time, the definition of "small" expands. A year later, the team is shipping huge changes without review and finding integration issues in production that the review process was specifically designed to catch.

Each of these is defensible at the time. They look like sensible prioritization and efficiency, not debt. The problem is the same as with technical debt. The small concessions accumulate into a system that's worse than the one you designed, and at no single point did anyone make the call to make it worse.

## The four patterns I see most

I've come to recognize four common patterns. Each has its own diagnostic and its own fix.

### Decision rights drift

The clearest sign is that you can ask three people who's responsible for a particular call (production deploys, architectural decisions, prioritization of a bug over a feature) and get three different answers. The RACI chart says one thing. Practice has drifted somewhere else, usually because the formal owner delegated informally years ago and the delegation never got written down.

The fix is uncomfortable. List the decisions that matter. Write down who actually owns each one today, not who's supposed to. Then write down who should own it. Where those two lists disagree, you've got a decision to make and to communicate.

I've done this exercise on every leadership team. Every time, it surfaces things the leadership team didn't realize had drifted. [Fast-scaling teams][3] are particularly prone to this. Decision rights set when the team was eight people rarely survive contact with thirty.

### Ritual decay

Skip-levels, design reviews, retrospectives, planning sessions. These are leadership rituals that exist for a reason, and they decay along a predictable path. First the rhythm slips ("let's skip this week"). Then the agenda hollows out ("we don't really have anything to discuss, let's just do round-robin status"). Then the meeting becomes a calendar hold that nobody prepares for and nobody gets value from.

The fix is to either kill the ritual or restore it to its original purpose. If you kill it, write down what it was supposed to do and confirm you're getting that signal another way. Keeping the calendar hold but letting it stay hollow is the worst of both worlds. People look at it, decide leadership doesn't take its own rituals seriously, and stop preparing for the ones that still matter.

### Feedback latency

Healthy organizations give feedback close to whatever prompted it. [Gallup's research][4] is blunt about this: employees who get weekly feedback are far more likely to be engaged than those who only hear from their manager at year-end, and only 14% of employees say their performance review actually inspired them to improve. Organizations carrying management debt batch feedback into year-end performance reviews, where it lands as a list of surprises about events from eight months ago that nobody can do anything about.

This one is the hardest to fix because the people involved have to do something they've been avoiding for a year. The structural piece is making sure feedback has a low-friction venue. Consistent 1:1s, written notes, performance check-ins quarterly rather than annually. The behavioral piece is the one that actually matters: the leader has to commit to delivering feedback soon after whatever prompted it, and their own manager has to check that it's happening.

In my experience, this is where management debt does the most damage to retention. An engineer who hears "you should have been doing X differently for the past year" in a performance review either loses trust in their manager or starts interviewing. Often both. I've seen the same avoidance dynamic show up in [unlimited-PTO programs][5] - a perk turns into a liability when the management work behind it never happens.

### Meeting metabolism

The slowest variant to spot is the gradual expansion of recurring meetings until calendar load makes deep work impossible for the leadership team itself. I like to call this "Calendar Tetris", and symptoms include frequent, overlapping meetings, often of a recurring variety. Each meeting was added for a defensible reason. The aggregate is that an engineering manager spends 32 hours a week in meetings and wonders why nothing strategic is getting done. On [globally distributed teams][6] the math gets worse, since synchronous slots compress into a few overlapping hours and "let's just add a recurring sync" becomes the default response to any coordination problem.

The fix is blunt. Cancel everything recurring for two weeks. Re-add only the meetings that someone actively missed. The recurring meetings nobody noticed missing weren't serving the purpose they were created for. They were calendar inertia, and they were eating the leadership team's capacity. [Shopify did exactly this at scale in early 2023][7], deleting roughly 12,000 recurring meetings in a single calendar purge with a two-week cooling-off period before anything could be re-added. Their reported outcome was a 33% drop in time spent in meetings and a 25% increase in completed projects.

I've done this, and it's always refreshing when recurring meetings don't come back. Those weren't as needed as once believed.

## How to find your own management debt

The diagnostic question I run on myself, and on every leadership team I've inherited, is this: What rituals are we running today that we wouldn't design from scratch if we were starting tomorrow? If the answer is "none," you're either not looking hard enough or you've genuinely just done a refactor. 

Another version: where in our leadership operating model are we relying on heroics or institutional memory instead of process? The answers are the line items in your management debt portfolio.

A more concrete version I use with my staff: which decisions in the last quarter took longer than they should have, and why? The "why" almost always points to a piece of management debt. A missing decision right, a stale ritual, a communication channel that nobody runs anymore or that needs the help of management to clean up.

## Why this matters more than technical debt

Technical debt slows code velocity. Management debt slows everything.

These are decisions that aren't being made on time, feedback that isn't landing, and rituals that aren't producing signal. They show up as engineers who feel like they're working harder for less output, leaders who feel like they're spending the whole day in meetings without making progress, and a strategic plan that everyone agrees with and nobody actually executes.

Engineering leaders are usually fluent in technical debt management. We track it, we explain its cost, we plan its repayment. We need the same discipline for the management practices themselves. Make it visible, talk about it explicitly, refactor it deliberately instead of waiting until the organization is genuinely broken.

## Conclusion

Across the leadership roles I've held, the leaders I've seen succeed long-term aren't the ones with the most polished initial operating model. They're the ones who are willing to look at how they're actually leading, six or twelve months in, and fix what's drifted.

Management debt is real. It accumulates the same way technical debt does. It costs at least as much, and it doesn't show up in any dashboard you already track. The next time you catch yourself thinking "we should really restart that ritual" or "we should really write down who owns that decision," that's not a nice-to-have on a backlog. That's overdue maintenance, and the longer you defer it the more expensive it gets.

## Common Questions

### What is management debt?

It's the gap between how leadership is supposed to operate and how it actually operates, the engineering-leadership sibling of technical debt. It shows up as decision rights that quietly migrate to whoever speaks up first, skip-levels that became calendar holds nobody runs, planning sessions that turned into backward-looking status reviews, and feedback saved up for the annual review. None of it looks like failure in the moment; it just costs more than it should every time you touch it.

### How does management debt accumulate?

Rarely through a bad decision. Usually through a deferred one. A skip-level gets cancelled for a customer escalation, then again, then quietly falls off the calendar "until things calm down," and things never calm down. A team starts skipping architecture review for "small" changes and the definition of small keeps expanding. Each concession is defensible at the time, and at no single point did anyone decide to make the system worse.

### What are the most common patterns of management debt?

Decision-rights drift (ask three people who owns a call and get three answers); ritual decay (skip-levels and reviews slip, hollow out, then become empty calendar holds); feedback latency (feedback batched into year-end reviews instead of delivered close to the event); and meeting metabolism, or "Calendar Tetris" (recurring meetings expanding until deep work is impossible). Each has its own diagnostic and its own fix.

### How do you fix drifting decision rights and decaying leadership rituals?

For decision rights, list the decisions that matter, write down who actually owns each one today versus who should, and where the lists disagree, make and communicate the call. That exercise surfaces drift on nearly every leadership team, especially fast-scaling ones. For rituals, either kill them (and confirm you're getting their signal another way) or restore them to their original purpose; keeping a hollow calendar hold is the worst option, because it teaches people that leadership doesn't take its own rituals seriously.

### How can a leader clear out unnecessary recurring meetings?

Cancel everything recurring for two weeks, then re-add only the meetings someone actively missed. The ones nobody noticed missing were calendar inertia, not signal. Shopify did this at scale in early 2023, deleting roughly 12,000 recurring meetings with a two-week cooling-off period, and reported a 33% drop in meeting time and a 25% increase in completed projects.

### Why can management debt be more damaging than technical debt?

Because technical debt slows code velocity, but management debt slows everything: decisions that aren't made on time, feedback that doesn't land, and rituals that produce no signal. It shows up as engineers working harder for less output and strategic plans everyone agrees with but nobody executes, and unlike technical debt it appears on no dashboard. Engineering leaders are fluent in managing technical debt; they need the same discipline for their own leadership practices: make it visible, talk about it, and refactor it deliberately.

[1]: {filename}2025_04_08_tech_debt_strategic_approach.md
[2]: https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0308183
[3]: {filename}2025_04_01_scaling_teams_without_losing_culture.md
[4]: https://www.gallup.com/workplace/357764/fast-feedback-fuels-performance.aspx
[5]: {filename}2023_08_14_unlimited_pto_management_failure.md
[6]: {filename}2025_04_15_leading_distributed_teams.md
[7]: https://www.worklife.news/culture/how-to-create-a-25-productivity-hike-lessons-from-shopifys-meetings-purge/