# Frameworks: Applying AI to Operational Problem Patterns

Operations work is full of recurring problem patterns. The problems change on the surface — different companies, different tools, different teams — but the underlying structure repeats. Once you recognize the pattern, you can apply AI systematically rather than reactively.

This is a collection of those patterns, how to diagnose them, and how AI can be applied to each one.

---

## Pattern 1: The Same Information, Multiple Audiences

**What it looks like**
The same underlying information needs to be communicated differently to different groups — customers, internal teams, executives, support staff. Someone (or multiple people) rewrites it manually each time. The outputs are inconsistent. Things get missed. Internal details leak into external communications.

**How to diagnose it**
- Are multiple people writing different versions of the same update?
- Does the communication process happen after every release, sprint, or milestone?
- Are there recurring complaints about CS not knowing what shipped, or customers getting confusing updates?
- Is there a "source of truth" for the information, or does everyone pull from different places?

**Why AI fits here**
This pattern is ideal for AI because the transformation logic is consistent — the same information, reframed for a different audience — and the rules for what to include or exclude per audience can be made explicit. AI doesn't get tired of rewriting the same release notes five different ways.

**What a well-designed solution looks like**
- A single structured input (one source of truth)
- Explicit audience rules defined upfront (what each audience needs to know, what they don't)
- Parallel generation — all outputs produced from the same input, not sequentially
- Human review at the output stage, not the writing stage

**What failure looks like**
Asking AI to write all five versions in one prompt, with no structured input and no explicit rules. The outputs will be plausible but inconsistent, and errors in one version won't be traceable.

---

## Pattern 2: Unstructured Input, Structured Decision Needed

**What it looks like**
Raw, unstructured information — customer feedback, support tickets, meeting notes, sales call summaries — needs to be categorized, prioritized, and acted on. Without structure, it sits in a backlog. With manual processing, it takes hours. Decisions get made based on whoever spoke loudest, not the actual signal in the data.

**How to diagnose it**
- Is there a backlog of feedback or requests that "someone needs to go through"?
- Are prioritization decisions made in meetings based on anecdote rather than data?
- Does the team have a rough sense of what customers want but no structured view of frequency or severity?
- Is the same type of information being manually sorted and categorized repeatedly?

**Why AI fits here**
Classification and clustering are tasks AI handles well at scale. A human reading 200 support tickets to find themes is slow and inconsistent. AI can process the same input in seconds, apply consistent classification logic, and surface structure that would take hours to find manually.

**What a well-designed solution looks like**
- Clear classification schema defined before the AI touches the data (categories, severity levels, source types)
- Separation of classification from prioritization — classify first, prioritize second
- Human review of the classification output before decisions are made
- Output in a format that feeds directly into the decision-making process (not a separate document to re-read)

**What failure looks like**
Asking AI to "analyze this feedback and tell me what to build next." The model will produce a plausible-sounding answer with no traceable logic. Decisions made from it are fragile.

---

## Pattern 3: Recurring Coordination Tax

**What it looks like**
A process requires the same human effort every cycle — weekly, monthly, or per release. Status updates, onboarding checklists, reporting, meeting prep, handoff documents. The work is low-judgment but time-consuming. It crowds out higher-value work. Everyone knows it could be more efficient but no one has fixed it.

**How to diagnose it**
- What does your team do every week that feels like it shouldn't require human time?
- Are there tasks where the output is always structurally similar but the inputs change?
- What would you automate first if you had an engineer available for a day?
- What recurring task creates the most drag on the team?

**Why AI fits here**
Recurring coordination work has predictable structure and variable inputs — exactly the condition where AI provides the most leverage. The output format is known, the rules are consistent, and the only thing that changes is the data.

**What a well-designed solution looks like**
- The trigger is defined (what starts the process — a calendar event, a completed task, a data update)
- The inputs are structured or can be made structured
- The output template is explicit
- A human reviews and sends — AI drafts, humans decide

**What failure looks like**
Automating the wrong thing. If the process itself is broken, automating it makes it faster to produce bad output. Diagnose the process first, then automate.

---

## Pattern 4: Reactive Firefighting Instead of Proactive Signals

**What it looks like**
Problems surface when someone complains — a client calls, a metric drops, an escalation lands in your inbox. By the time it's visible, it's already impacting people. The ops team spends more time responding than preventing. There's data available that could have flagged the issue earlier, but no one is watching it systematically.

**How to diagnose it**
- What percentage of your team's time is spent on problems that "came out of nowhere"?
- Is there data being generated that no one is regularly reviewing?
- Do post-mortems consistently find that early signals existed but weren't noticed?
- Are escalations concentrated around the same types of issues?

**Why AI fits here**
Humans are bad at monitoring many signals simultaneously over long periods. AI can watch for deviations from normal patterns continuously, at a granularity no human team can sustain. The goal isn't prediction — it's early detection.

**What a well-designed solution looks like**
- A clear definition of "normal" for each signal being monitored
- Tiered alerts: informational → warning → critical (not everything is a fire)
- Alerts routed to the right person with enough context to act — not just a raw number
- Regular review of false positive rates to tune thresholds

**What failure looks like**
Alert fatigue. If everything triggers an alert, nothing gets attention. The design of the threshold logic matters as much as the detection logic.

---

## Pattern 5: Knowledge Trapped in People

**What it looks like**
Critical operational knowledge lives in someone's head, in scattered Slack threads, or in documents no one can find. Onboarding takes months because the knowledge isn't written down. Key person risk is high. When someone leaves, institutional knowledge leaves with them. The same questions get asked and answered repeatedly.

**How to diagnose it**
- Who are the people that "just know how things work"?
- How long does it take a new hire to become independently effective?
- Are the same questions being asked in Slack that were asked six months ago?
- What would break if a specific person were suddenly unavailable?

**Why AI fits here**
AI can help structure, surface, and query institutional knowledge at scale — but only if the knowledge is first captured. The value isn't in AI generating knowledge it doesn't have; it's in making captured knowledge accessible and useful.

**What a well-designed solution looks like**
- A systematic process for capturing knowledge as it's created, not retrospectively
- A queryable format — not a document graveyard, but structured enough to search
- An AI layer that can synthesize across sources and answer questions in context
- Regular audits for staleness — knowledge that isn't maintained becomes misinformation

**What failure looks like**
Building a knowledge base no one updates. AI querying stale information confidently produces wrong answers. The maintenance process matters as much as the initial build.

---

## Using These Frameworks

These patterns aren't mutually exclusive — most operational problems involve more than one. A broken release communication process (Pattern 1) is usually also a recurring coordination tax (Pattern 3). A feedback backlog (Pattern 2) often exists because the knowledge of what customers want is trapped in a few people's heads (Pattern 5).

The diagnostic questions are the starting point. Before applying any AI solution, be clear on which pattern you're actually solving for — and whether AI is the right tool, or whether the process needs to be fixed first
