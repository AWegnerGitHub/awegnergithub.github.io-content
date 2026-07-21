Title: Red flags in engineering organizations that only show up after week two
Date: 2026-07-06 17:15
Tags: job, leadership
Category: Leadership
Slug: red-flags-engineering-organizations-week-two
Status: published
Summary: Week one is onboarding theater. Everyone's on their best behavior and nothing's under stress, so the flags that matter stay invisible until something finally goes under load. Here's a field guide to what an established engineering organization hides during onboarding, and the week-two moments when it shows.
Series: Leadership Thoughts
Featured: true

[TOC]

Every engineering organization has two versions of itself. There's the one it describes in interviews and first-week onboarding. The version that has the architecture diagrams, the process docs, the confident "here's how we work." Then there's the one that actually runs, which you don't meet until something puts it under load.

Week one won't show you the gap. Week one is theater, and not in a cynical way. Everyone's on their best behavior, you're reading docs and meeting people, nothing's under stress. The company describes itself to you, and you have no reason to doubt the description.

Week two is when the description meets reality. The first release goes out. The first metric gets requested. The first incident hits. The first diagram that's just wrong. In the gap between what you were told and what you see, the real organization shows itself.

[Michael Watkins][1] already wrote the 90-day plan, and it's good. Read it for how to *act* in a new role. What I'm after is different. I'm building a field guide to what an established organization hides during onboarding, and the exact moments in week two when each thing surfaces. Everything here I learned the hard way, by walking into it.

One confession before the examples. I didn't see the pattern connecting these until I'd tripped over every one of them. They looked like five unrelated problems. They aren't, but that part waits for the end. It lands better once you've met them.

![Two lines share one path through week one, then fork at week two: the told line stays flat while the true line falls away as flags surface along it][fig1]

*Figure 1. The organization's account of itself and the way it actually runs share one line through week one, then split. Each flag you hit in week two drops the truth further from the story, and the gap you couldn't see widens with every one.*

## The weekly release that wasn't weekly

The team lead told me they released weekly. Like clockwork. So I went looking for the releases.

No changelog. No trail in the ticket tracker marking what shipped when. When I reconstructed the real history, releases were going out every 2 to 3 weeks, each one a sprawling bundle of changes that had piled up waiting to go out together.

That gap predicted a lot, and all of it held. A team that ships in big, infrequent bundles is usually a team where releasing is expensive. Every release drags a pile of manual steps and coordination behind it, so you batch changes up to pay that cost less often. It's rational, and it's a trap. The bigger the batch, the more that can go wrong in it, and the harder it is to tell which change broke things when one does. The research is blunt about this. [Accelerate][9] found small, frequent releases track with the highest-performing teams, and big bundles with everyone else. It also can't gracefully handle a change that fails QA late, because there's no small, frequent train to catch the next departure. The sprint cadence was fuzzy underneath. Two weeks of development, then maybe it made the release, if QA got to it in time. And "if QA got to it in time" was the tell for a [manual QA process][2] that couldn't keep pace. The bundle that piles up between releases is where [technical debt][12] compounds quietly, since every cycle you don't ship stacks more change into the next one.

Why did the story survive? Nobody ever checked it, including the team telling it. The releases were never broadcast. On release day the team pushed the changes and that was the end of it. No announcement, no notes, nothing said even when the UI visibly changed. No artifact existed anywhere that could contradict "we release weekly," so the claim just stood. The silence that hid the flag is the same silence that kept the story alive.

![The weekly-release flag as a four-box loop, the week-two tell tile highlighted, arrows running story to tell to prediction to hidden and back to the story][fig2]

*Figure 2. The weekly-release flag drawn as a loop: the story you're told, the week-two tell that breaks it, what it predicts, and why it stayed hidden, which feeds straight back to keep the story standing.*

**Go looking for the receipts.** That's the move that matters most in week two, and it travels far past releases.  For every confident claim a company makes about how it works, ask what artifact would prove it, the changelog or the ticket history, then go find it. When it doesn't exist, you've usually found a place where the story and the reality drifted apart, because nothing was ever forcing them to agree.

## The metrics that measured nothing

Ask a healthy organization how it's doing and you get numbers. Ask an unhealthy one and you get stories.

I asked for metrics on the delivery process. Not "are we busy," but "where are we slow, where do we break, where could we get better." What came back were anecdotes. A good sprint someone remembered. A bad incident someone else remembered. When I pushed for real numbers, it turned out things were being tracked: lines of code, tickets opened versus closed. Activity. Nothing that measured an outcome anyone cared about.

That's the tell, and it's easy to miss, because [vanity metrics][10] look like measurement. A dashboard of closed tickets feels like rigor. It's just motion. The team had never been asked whether it was getting better, so it never built the instruments that would answer.

![A matrix comparing activity metrics against outcome metrics across throughput, stability, and human experience, with the activity column highlighted][fig3]

*Figure 3. The same three axes, measured two ways. Read across each row: the activity a team counts by default next to the outcome that actually moves, for throughput, stability, and human experience.*

A mature organization tracks across three axes. **Throughput**, how fast work moves: deployment frequency, lead time, [flow efficiency][3] (the ratio of time work is actively worked versus sitting idle, which is quietly brutal in the release scenario above). **Stability**, whether that speed is costing you quality: change-failure rate, escaped-defect rate, time to restore. **Human experience**, the axis most teams never instrument at all: where engineers lose time, how heavy the toil is, whether the people doing the work believe it's improving. The [SPACE framework][5] is one way to put numbers on that last one.

[DORA][4] lives in the first two axes, and it's a fine place to start. But "just adopt DORA" is cliche advice at this point, and it skips the actual flag. The flag is that they measure **activity instead of outcomes**. Until someone asks for an improvement number, nobody notices the difference.

## The founder who couldn't leave the room

Every so often you inherit a team with a founding engineer still on it. Someone who was there at the beginning, knows every system and every product decision, and is visibly indispensable. That last word is the problem, and it takes a stressor to see why.

At one company, this person sat in the middle of everything. The twist that separates this from the tired "hero engineer" trope, though, is that they were trying to get out. They could see the dependency and hated it. The team wouldn't move without their input. Every decision routed back through them, because it always had, and habit is hard to unwind.

The obvious prediction was [bus factor][6]. The team couldn't function if this person took a vacation or left. The deeper one was about the team itself because the group had never built the muscle of deciding on its own; someone had always been there to decide for it. That muscle has to grow as the team grows, and it's easy to skip while you're busy adding headcount. More on that in [scaling a team without losing what works][13].

The fix was counterintuitive and a little painful. Delegation had already failed. They'd tried it, and the team just waited them out. So we went the other way and made them deliberately unavailable for specific kinds of decisions, pushing the call to the engineers who should've been making it by taking away the option to escalate to the person everyone leaned on. Uncomfortable for a while. The team got through it, and started making calls it had been quietly outsourcing handing off.

Why does this hide in week one? It reads as strength. An engaged, deeply involved veteran looks like an asset on day one. It only shows up as a dependency the first time you need the team to move and the veteran isn't in the room.

## The decision that wasn't yours until it went wrong

You walk in owning something. The reporting lines say so, the offer letter said so, and in week one everyone agrees the call is yours. Then you try to make one.

This flag is quiet and common. Decisions in your own area keep getting made around you. One gets settled in a side conversation you weren't in. Another gets defaulted into by inaction, nobody decided, so the existing behavior won by attrition. A third turns out to have been held for years by convention, by someone who was "always consulted" on this kind of thing, and nobody thought to mention it, because to them it was never a decision. It was just how things work.

You find out you owned that decision the moment it goes wrong and lands on your desk. Usually followed by a baffled "why didn't you talk to me?" from someone who had every reason to expect a heads-up.

What it predicts is a gap between the authority the organization granted you and the authority it actually routes to you. Different things. The organizational chart shows the first. The second lives in habits and history that no document captures. Decision rights that drift like this are one of the clearest forms of [management debt][14], the leadership cousin of the technical kind. The fix starts with dragging those habits onto paper by listing the decisions in your area and name who really makes each call in practice. [Bain's RAPID][11] is one way to run that exercise.

It hides through week one because you're not deciding yet, you're learning. Week two is the first time you reach for a lever you were told you own, pull it, and feel it wired to something on the other end you can't see. The tell is quiet. It's a decision that resolves without you, in an area that's supposedly yours.

## The silence during the outage

This is the one that taught me the most, and it announced itself with silence.

A production outage, real customer impact. The engineering team did what looked, from inside, like the right thing. They huddled in a private channel, just the team and their manager, and got to work. Heads down, solving the problem.

Meanwhile the rest of the company was finding out on its own. The help desk started fielding reports. Sales started fielding reports. Eventually a Sales director went hunting for an engineering counterpart to ask what was going on, and reached an Engineering director who didn't know there was an outage at all. The news had never climbed. The people fixing the problem and the people who needed to know it was being fixed sat in different rooms, with no door between them.

The silence from engineering during a live, customer-impacting outage was deafening. Inside the channel it felt like focus. Outside, it looked like nobody was handling it.

What it predicted was a company where bad news didn't travel, up or out. A team could be heroically on top of a problem while the company around it had no signal anything was being handled. That's dangerous, because everything can feel fine from inside engineering while the truth is already loose in the building, reaching customers through every channel except the one that could reassure them.

The fix cost the team some comfort. Two changes. Engineering leadership hears about a live incident within minutes, no exceptions. And the troubleshooting moves into a channel other people can see, so the work is visible while it's happening.

It felt like exposure at first, like working with the door open. But the next outage resolved faster, because more eyes on an open channel meant more information surfacing to narrow the problem down. And the news climbed on its own this time, because the room it was being fixed in was one anybody could look into.

The piece we'd been missing was someone owning the outward story. During that first outage nobody was telling anyone outside the room what was happening, and an open channel only helps if someone points people to it. Google's SRE book calls that job a [communications lead][7]. The exposure that felt like a cost turned into a tool. In a remote environment, [communication is _vital_.][15]

## Finding these when you can't hold the wrench yourself

Everything above assumes you can get your hands on the thing. That works when you're close to one team, and it quietly stops working the day you're running several teams through directors who run them.

So the detection has to change shape. Checking the receipts yourself doesn't scale, so you push the habit down. "What artifact would prove this claim?" becomes a question your directors ask as a reflex, in every skip-level and every incident review, the way you learned to ask it.

At that height the tell changes too. The missing changelog gives way to something subtler: lag and polish. You watch which teams' bad news reaches you late, and which reaches you already sanded smooth. A problem that arrives pre-explained, the rough edges gone, came through a filter on the way up. The filter is the flag. Same silence from the outage, one layer quieter.

The fix scales the same way. You require the information to move before you need it, so no hero has to carry it. Every incident leaves a written timeline leadership can read. Every "we do X" in a review shows up with the artifact that proves it, or it gets logged as a thing to check. Do that and you stop needing to go looking, because the receipts come to you.

## What they all have in common

Here's the pattern I promised, the one I couldn't see until I'd tripped over all five.

Look at what each flag actually was. The real release cadence never reached me. The metrics that would show whether the team was improving didn't exist. The knowledge locked in one person's head wouldn't spread to the team. The authority I supposedly held wouldn't route to me. News of a live outage wouldn't climb.

So it's really one problem in five rooms: **information failing to reach the person who needs it, when they need it.**

I can't take credit for the insight. [Ron Westrum][8] described it decades ago when he sorted organizational cultures into pathological, bureaucratic, and generative, mostly by how they handle information. Pathological cultures bury it. Generative ones let it flow. I want to add that burying is rarely deliberate, and it's nearly invisible from the inside. Nobody in these companies was hiding anything. The pipes just weren't connected, and no one noticed, because nothing had ever required the information to move.

That's why these flags wait for week two. Week one is low-stakes. You're onboarding, nothing's urgent, nobody needs information to travel anywhere fast. Week two is the first time you need something to move, a decision, a metric, a release, an escalation, and you find out whether the plumbing exists.

One uncomfortable footnote, because I've sat on the wrong side of it. Westrum's claim runs deeper than "the pipes weren't connected." How information moves is downstream of how leaders treat the person carrying it. The team watches what happens to the first person who brings you a bad number or a broken release, and they price their own honesty on what they see.

Before you go hunting these flags in the place you inherited, check the one you bring into the room with you. The first time someone hands you bad news, your reaction sets the how the team responds on every piece of news after it. 

One last thing, because "your company is broken, run" would be the wrong takeaway. Most organizations have a few of these. Finding them just shows you where to spend your first real capital as a leader. If you want a place to start, start with how bad news travels during an incident. It's the flag with customers already in the blast radius while you figure things out, so a broken pipe there costs you the fastest. Fix the escalation path first. The releases and the metrics will still be waiting next week, and you'll have bought some credibility to spend on them. Every symptom here runs downstream of the same source, so if you fix how information moves before you start chasing symptoms one by one, a surprising number of them turn out to have never been separate problems.

They were just the silence, in different rooms, waiting for you to notice.

## Common Questions

### Why do engineering organization problems only show up after week two?

Week one is onboarding theater. Everyone's on their best behavior and nothing is under stress, so the gap between how a company says it works and how it actually runs stays invisible. Week two is when the description meets reality: the first release, the first metric request, the first incident. The flags surface the first time you actually need information, a decision, or an escalation to move.

### What are the biggest red flags to watch for when joining an engineering organization?

A stated release cadence the receipts don't support; metrics that track activity (lines of code, tickets closed) instead of outcomes; a founding engineer everyone routes decisions through (a bus-factor risk and a team that never learned to decide for itself); ownership of a decision that only becomes yours when it goes wrong; and silence during a customer-impacting outage because bad news doesn't travel up or out.

### How can a new engineering leader detect these problems early?

Go looking for the receipts. For every confident claim about how the company works, ask what artifact would prove it: the changelog, the ticket history, the incident timeline. Then go find it. When it doesn't exist, you've usually found where the story and reality drifted apart. At scale you push that habit down so directors ask "what artifact would prove this?" as a reflex, and you watch for bad news that arrives late or suspiciously pre-polished.

### Which problem should an engineering leader fix first?

Start with how bad news travels during an incident, the escalation path. It's the flag with customers already in the blast radius, so a broken pipe there costs you fastest, and fixing it buys credibility to spend on releases and metrics later. Many of the other symptoms turn out to run downstream of the same source: information not moving.

### How does a leader's own behavior contribute to these red flags?

How information moves is downstream of how leaders treat the person carrying it. The team watches what happens to the first person who brings bad news (a broken release, an ugly number) and prices their own honesty accordingly. Before hunting these flags in an inherited org, check your own reaction to the first piece of bad news, because it sets the tone for every piece after it.

[1]: https://hbr.org/books/watkins
[2]: https://martinfowler.com/bliki/TestPyramid.html
[3]: https://kanban.university/flow-efficiency-a-great-metric-you-probably-arent-using/
[4]: https://dora.dev/guides/dora-metrics-four-keys/
[5]: https://queue.acm.org/detail.cfm?id=3454124
[6]: https://en.wikipedia.org/wiki/Bus_factor
[7]: https://sre.google/sre-book/managing-incidents/
[8]: https://dora.dev/capabilities/generative-organizational-culture/
[9]: https://itrevolution.com/product/accelerate/
[10]: https://tim.blog/2009/05/19/vanity-metrics-vs-actionable-metrics/
[11]: https://www.bain.com/insights/rapid-decision-making/
[12]: {filename}2025_04_08_tech_debt_strategic_approach.md
[13]: {filename}2025_04_01_scaling_teams_without_losing_culture.md
[14]: {filename}2026_04_30_management_debt_leadership_patterns_rot.md
[15]: {filename}2025_08_22_remote_leadership_remote_work.md
[fig1]: {attach}images/red-flags-week-two-figure-1-divergence.png
[fig2]: {attach}images/red-flags-week-two-figure-2-loop.png
[fig3]: {attach}images/red-flags-week-two-figure-3-metrics.png