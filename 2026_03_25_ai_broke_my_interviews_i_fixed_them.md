Title: I Proved AI Could Beat Our Technical Interviews. Then I Had to Fix Them.
Date: 2026-03-25 15:00
Tags: job, leadership, ai-interviews
Category: Leadership
Slug: ai-broke-our-interview-process-i-had-to-fix-it
Summary: After proving AI could beat most technical assessments, I had to rebuild how I hire engineers. Here's the process I've developed; focused on what actually predicts success on the job.
Series: Leadership Thoughts

[TOC]

## The interview process I used to run
 
For years, I used timed online assessments as part of my hiring process. At PacketFabric, when we needed to scale the engineering team rapidly during our Series B, I helped evaluate several assessment platforms to find one that balanced candidate experience with the quality of signal we received as hiring managers. We landed on [Woven][woven], and it worked. The assessments helped us move quickly through a high volume of candidates and gave us comparable data points across applicants.
 
I had doubts, though. I've been opposed to LeetCode style interviews forever. Woven wasn't LeetCode as it provided amazing feedback to the candidate on what did and did not work. I liked the company so much that [I joined Woven][1] for a while. But even with the best assessment platform I'd found, I saw cracks. The candidates who passed the assessments and joined the team didn't always succeed in the ways I expected. Some produced clean code under time pressure but struggled to collaborate with the team. Others passed technical screens convincingly but couldn't translate that skill into shipping features in a real codebase with real constraints. The assessment told me they could solve a problem in isolation. It told me very little about whether they could do the actual job.
 
Then ChatGPT arrived, and my doubts became certainties.
 
In late 2022 and early 2023, I ran ChatGPT through technical assessments on [TestGorilla][2], [CodeSignal][3], [Codility][4], [HackerRank][5], and [CoderByte][6]. It passed most of them. Not marginally; it produced solutions that would have advanced through our pipeline. That wasn't a theoretical concern anymore. If a freely available tool could pass our technical assessment, we weren't evaluating engineering skill. We were evaluating test-taking ability, and we now had a machine that was better at test-taking than many humans. 
 
That was 2022 and 2023. We are now several AI model generations beyond that early ChatGPT. Tools like Claude Code, Codex, Cursor, and GitHub Copilot are used by millions of developers daily. These aren't novelty chatbots any longer. They're integrated development environments that write, debug, and refactor production code. The gap between what AI could do when I ran those tests and what it can do today is enormous. Any assessment that was vulnerable to early ChatGPT is trivially solvable now.
 
I had to rebuild how I hire.
 
## What timed assessments and take-homes actually measure
 
Timed assessments reward speed and pattern recognition under artificial pressure. That correlates poorly with the actual work. Nobody ships production code in 45 minutes with a countdown timer and no access to documentation. The skills that make someone fast on a Codility problem like memorized algorithms, familiarity with specific puzzle patterns, comfort performing under observation, are not the same skills that make someone effective on a team building real products.
 
Take-home projects have the opposite problem. Without time constraints or observation, there's no way to verify who actually did the work, how long it took, or what tools they used. Even before AI, take-homes had issues: they disproportionately disadvantage candidates with families or other time commitments, and they're difficult to evaluate consistently across candidates.
 
Both formats share a deeper flaw though. They optimize for a narrow technical signal while telling you almost nothing about how a person thinks, communicates, or works with others. I've seen candidates pass assessments convincingly and then struggle on the job because they couldn't explain their decisions to a colleague, couldn't adapt when requirements changed, or couldn't take ownership of a system beyond the specific code they wrote. The assessment didn't predict any of that. It just told me they could solve a contrived problem under contrived conditions.
 
I want to be fair to these tools, though. They feel rigorous and objective on the surface. That's why they're popular. The scores are comparable, the process is standardized, and it feels like you're making data-driven decisions. But the data is measuring the wrong thing.
 
## What I look for instead
 
My core philosophy has stayed the same across companies even though the specific steps change based on the team, the roles, and the company's needs. The philosophy is this: I want to understand how someone thinks, how they communicate, and whether they take genuine ownership of their work. The specific technology matters, but it matters less than those three things.
 
How I evaluate depends on the level of the role.
 
### Mid-level and senior engineers
 
For experienced candidates, I focus on past project deep-dives. I ask the candidate to describe something they built - not a rehearsed elevator pitch, but a real system they worked on. Then I poke at it.
 
The questions I ask depend on what the candidate brings up, and I rotate through several lines of inquiry based on what I'm hearing. If someone describes a system they architected, I'll ask about the tradeoffs they made and why. If they describe a project that clearly had rough patches, I'll ask what went wrong and dig into how they handled it. This isn't because "tell me about a failure" is a novel question, but because the follow-up conversation reveals depth. The most telling question, though, is usually some form of "what would you do differently if you built this again?" Candidates who were deeply involved in a project have learned something from it. They have opinions about what they'd change. Candidates who are exaggerating their involvement tend to stumble here because they haven't actually wrestled with the consequences of the original decisions.
 
About half of the candidates I interview can genuinely defend their decisions in this format. The other half can describe what they built but can't articulate why they made the choices they did, or what they learned from the experience. That's the signal. I'm not looking for perfect decisions. I'm looking for evidence that they understand systems deeply enough to reason about them, not just implement them.
 
For more senior and architecturally-focused roles, I lean harder on design tradeoff discussions. For mid-level engineers, I focus more on the "what went wrong" and "what would you change" angles, which are more accessible and still reveal a lot.
 
### Junior engineers
 
Past project deep-dives work less well for junior candidates. They don't have enough experience to defend decisions they haven't had the opportunity to make yet. Asking a recent graduate to explain the architectural tradeoffs of their senior capstone project isn't a fair evaluation.
 
Instead, I use a small live coding exercise. Something that can be written in 5-10 lines of code, with multiple valid approaches. For example, a problem that can be solved iteratively and recursively. I ask them to write it one way and we talk through it for a minute or two. Then I ask them to write it the other way. I don't tell them in advance that I'll ask for the alternative approach. I want to see what they do naturally when the problem shifts.
 
The code itself is almost secondary. What I'm evaluating is the conversation. Can they reason about the difference between the two approaches? Can they articulate why one might be preferable in a given context? Do they get flustered when asked to think differently, or do they engage with the challenge? A junior engineer who writes imperfect code but can reason clearly about alternatives and ask good questions is far more promising than one who produces a clean solution but can't explain their thinking.
 
### Leads and directors
 
For leadership roles, I shift to scenario and system design conversations. Less about code, more about organizational and architectural judgment. How would you structure a team to build a particular kind of system? How would you evaluate and prioritize a backlog of technical debt? How do you handle a situation where product and engineering disagree on timeline or scope? The signal here is similar to the engineering deep-dives. I'm listening for whether a candidate can reason through ambiguity, make a decision with incomplete information, and explain their thinking clearly. A strong candidate will ask clarifying questions, acknowledge tradeoffs, and arrive at a defensible position. A weak candidate will give a textbook answer that doesn't account for the messy reality of the scenario.
 
### Technical product managers
 
I also interview Technical Product Managers (TPM), and the approach shares the same philosophy as the engineering deep-dives. I'll present a feature or problem that has already been solved so that I have detailed knowledge of what has worked. I ask them to walk through how they'd approach it. I'm watching how they think through the problem, what questions they ask, what tradeoffs they identify, and how they communicate their reasoning. Then during the conversation, I poke at their decisions the same way I would if I were working through the problem with a TPM on my team. Can they explain why they prioritized one approach over another? Can they articulate the user impact? Can they adjust their thinking when I introduce a constraint they hadn't considered? This fits directly into the communication signal I look for across every role which is the ability to reason through a problem, defend your position, and adapt when new information arrives.
 
## The soft skills that predict long-term success
 
Technical skill gets someone through the interview. Soft skills determine whether they succeed on the team. I assess these through a combination of specific questions, observing behavior throughout the interview, and paying close attention to how candidates describe the systems and processes they've worked in.
 
Three signals have been the strongest predictors across my career:
 
**Ownership without ego.** I want someone who takes responsibility for the system, not just the lines of code they wrote. But ownership can become toxic when it turns into "this is my baby and it's always right and the user is wrong." The best engineers own the outcome for the user, not just the implementation. When something breaks, they don't deflect. But they also don't get defensive when someone suggests a different approach or when a user reports that the system isn't working the way they intended. You can often hear the difference in how candidates describe past work: do they talk about what "I built" in isolation, or do they talk about how the system served its users and how the team improved it over time?
 
**Communication.** Can they explain a technical decision to someone who isn't an engineer? Can they disagree with a colleague constructively? The interview itself is a communication exercise and I'm paying attention to how clearly they explain their past projects, how they handle follow-up questions, and whether they can adjust their level of detail based on the conversation. An engineer who can only communicate with other engineers who share their context will hit a ceiling quickly, regardless of their technical ability.
 
**Curiosity and willingness to learn.** The technology stack will change. The industry is already changing and AI has introduced so much disruption in such a short time that it is vital candidates are willing to learn and adapt. I've worked across many industries in my career, and the engineers who thrived through transitions were the ones who were genuinely energized by learning something new rather than threatened by it. In interviews, I listen for whether candidates describe their learning in terms of genuine interest or obligation. "I had to learn Kubernetes for the role" sounds very different from "I got interested in how our deployment pipeline could be more reliable, which led me to Kubernetes."
 
## Designing interviews that AI can't shortcut
 
The ChatGPT experiment wasn't just a blog series - it fundamentally changed how I approach hiring. My current process is designed so that AI assistance is either irrelevant or immediately visible.
 
You can't have ChatGPT defend your past project decisions in a live conversation. When I ask a candidate what they'd do differently about a system they built two years ago, there's no prompt that generates an authentic answer. The deep-dive format is inherently AI-resistant because it's evaluating lived experience, not producible knowledge.
 
For the junior coding exercises, the code portion could theoretically be AI-assisted. But the follow-up conversation, "now write it the other way, and explain why you'd choose one approach over the other", reveals immediately whether the candidate understands what they wrote. I've caught candidates who clearly used AI for their solution and then could not explain their own code when I asked follow-up questions. The conversation is the evaluation, not the code.
 
My position on AI use in interviews is nuanced. I don't ban AI tools, but I do expect candidates to disclose if they're using them. If a candidate uses AI and tells me about it, we can have a productive conversation about how they used it and what their own contribution was. If we've asked, and they haven't disclosed, that's a failure of integrity that outweighs technical competence. When you're hiring someone to join a team, trust matters as much as skill.
 
This isn't a hypothetical concern. I've encountered candidates who were clearly using AI during interviews and couldn't explain their own answers. I'm not alone. [CNBC reported in 2025][7] that tools like Interview Coder and Cluely now use invisible screen overlays that are undetectable by standard screen sharing, and hiring managers describe the telltale pattern of a pause, an "Hmm," and then a suspiciously perfect answer. An [analysis of over 19,000 interviews by Fabric][8] found that cheating adoption more than doubled from 15% to 35% between June and December 2025, with technical roles showing a 48% cheating rate.
 
I've also encountered the broader trend of candidates working at multiple companies simultaneously. This is a [real and growing problem in remote engineering][9]. A [ResumeBuilder.com survey][10] found that 37% of remote workers hold two full-time jobs, with AI tools making it increasingly feasible to juggle multiple roles within a standard workweek. Both of these trends make it even more critical that your interview process evaluates the actual human sitting across from you, not just the output they produce.
 
## What I haven't solved yet
 
No hiring process is perfect, and I don't want to pretend mine is. There are areas I'm still actively working to improve.
 
Evaluating candidates from very different technology stacks remains a challenge. When someone's entire career has been in a language or ecosystem that's different from what we use, the past project deep-dive still works for assessing how they think, but it's harder to gauge how quickly they'll become productive in a new environment. I haven't found a clean solution for this that doesn't fall back on the kind of generalized assessments I've moved away from.
 
Scaling the process is another challenge. Deep-dive conversations take time and they're significantly more time-intensive per candidate than a timed assessment that can be administered asynchronously. When you're hiring for many roles simultaneously, the time cost is real. I've mitigated this by involving more of the team in interviews, but that creates its own overhead.
 
And the overall hiring pipeline is still slower than I'd like. The industry-wide process of sourcing, screening, interviewing, and closing candidates has friction at every stage. Improving my interview process doesn't fix the weeks lost to scheduling coordination, offer negotiations, and notice periods.
 
## What actually matters
 
I've been hiring engineers across industries for nearly two decades and for companies ranging from pre-seed startups to Fortune 500 corporations. I've been wrong about candidates in both directions; people I was confident about who didn't work out, and people I had reservations about who became some of the strongest members of the team.
 
What I've learned is that the interview process should evaluate what actually matters on the job: Can this person reason about complex systems? Can they communicate clearly and work constructively with others? Do they take ownership of outcomes, not just code? Are they honest about what they know and what they don't?
 
No timed assessment answers those questions. A conversation does.
 

[woven]: https://www.woventeams.com/
[1]: {filename}2022_06_04_the_other_side_of_the_mirror.md
[2]: {filename}2022_12_26_breaking-the-interview-with-chatgpt.md
[3]: {filename}2023_01_01_chatgpt-breaks-more-interview-questions.md
[4]: {filename}2023_01_05_chatgpt-continues-beating-interview-questions.md
[5]: {filename}2023_01_08_solving-more-interview-questions-with-chatgpt.md
[6]: {filename}2023_02_04_chatgpt-beats-more-interview-assessments.md
[7]: https://www.cnbc.com/2025/03/09/google-ai-interview-coder-cheat.html
[8]: https://www.fabrichq.ai/blogs/state-of-ai-interview-cheating-in-2026-insights-from-19-368-interviews
[9]: https://fortune.com/2025/08/03/workers-holding-multiple-full-time-jobs-secretly-remote-work-40-hour-week-overemployment-high-income/
[10]: https://www.resumebuilder.com/7-in-10-remote-workers-have-multiple-jobs/
