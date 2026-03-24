# Case Studies: AI Applied to Real Operational Problems

These case studies are drawn from operational challenges I encountered firsthand in AI advisory and operations. Some document problems I solved directly. Others explore how I would apply AI to problems I've seen up close. All of them reflect how I think about operational improvement.

---

## Case Study 1: Automating Release Communication at a SaaS Company

**Context**
At a software company running weekly releases, the same engineering information was being manually rewritten by four different people — Product, Sales, CS, and the engineering lead — each producing a different version of what shipped. This created inconsistency, missed migration warnings, and the occasional accidental disclosure of internal work to customers.

**The Problem**
Release communication was a recurring coordination tax. Every release required 3–4 hours of writing across teams, with no single source of truth driving the outputs.

**AI-Powered Solution**
Built a release intelligence pipeline that ingests completed Linear tickets when a project is marked done, classifies each ticket once (change type, customer impact, visibility, risk flags), and generates five audience-specific artifacts in parallel:
- Customer-facing release notes
- Internal changelog with PR links and rollback notes
- Sales enablement brief with talk tracks
- CS/Support FAQ with escalation paths
- Slack announcement under 300 words

**Key Design Decisions**
- Feature-flagged work is automatically excluded from customer-facing outputs
- Breaking changes surface inline with migration guidance
- All five outputs read from a single structured release object — nothing is generated independently

**Impact**
Eliminated ~3 hours of manual writing per release cycle. Reduced inconsistency across outputs to near zero. Gave CS and Sales accurate, audience-specific information without requiring engineering involvement post-ship.

**Tech Stack:** Mastra, Linear API, OpenAI GPT-4o, Inngest, PostgreSQL, TypeScript

---

## Case Study 2: Fixing EHR Documentation Workflows That Were Slowing Down Patient Care

**Context**
At Oracle Cerner, providers at healthcare client sites were spending a significant and disproportionate amount of time on documentation inside the EHR. This wasn't just an efficiency problem — it was slowing patient throughput and contributing to clinician burnout. It was easy to accept this as the cost of complex healthcare software. I didn't.

**The Problem**
Rather than taking the surface complaint at face value, I went and observed the workflows directly — sitting in on how providers actually used the system in practice. What I found was that the issue wasn't complexity. It was redundancy and poor workflow design:
- The same information was being entered multiple times across different sections
- Providers had to navigate across multiple screens to complete a single task
- Free-text notes were being manually re-entered into structured fields that could have been populated automatically

**What I Did**
I broke the problem down systematically:
- Mapped the full documentation workflow to identify exactly where time was being lost
- Flagged specific friction points: duplicate inputs, unnecessary fields, inefficient navigation paths
- Worked directly with clinicians to validate what was truly required vs. what was legacy or redundant

From there, I partnered with the technical and implementation teams to make targeted changes:
- Consolidated duplicate fields and eliminated repeated data entry
- Improved templates and structured inputs to reduce free-text burden
- Ensured data entered once would populate downstream fields automatically

I acted as the bridge between clinicians and technical teams, translating workflow pain into actionable system improvements neither side could have fully articulated on their own.

**How AI Applies Today**
The same problem exists across healthcare organizations at scale. With AI, this workflow analysis can be done faster and more comprehensively: ambient documentation tools can capture provider-patient interactions and auto-populate structured EHR fields in real time, eliminating the re-entry problem entirely. The diagnostic work I did manually — mapping workflows, identifying redundancy, validating with clinicians — can be augmented with process mining tools that analyze system logs to surface friction points automatically.

**What This Demonstrates**
The instinct to go observe the workflow directly rather than accept the stated problem is the same instinct that makes AI implementations actually work. Most failed deployments happen because no one mapped the real workflow before automating it. I did that work by hand. Now I'd do it faster with better tools, but the judgment about what to look for doesn't change.

---

## Case Study 3: Cutting AI Consultation Timelines from Four Weeks to a Few Days

**Context**
At CGI, initial AI consultations with clients were taking nearly four weeks. Clients didn't know which AI use cases were worth pursuing, and the team was spending weeks manually evaluating feasibility, cost, and ROI for each one. No one had been tasked with fixing it — but the pattern was clear and the inefficiency was significant.

**The Problem**
Every consultation started from scratch. Evaluating whether an AI use case was feasible and worthwhile required deep back-and-forth between consultants, the technical team, and the client — most of which was repetitive and could have been structured. The result was slow client decisions and a team stretched thin on work that shouldn't have required weeks.

**What I Did**
I took ownership of the problem without being asked. I worked directly with the technical team to understand feasibility constraints and AI deployment tradeoffs, then built an AI use-case filtering tool independently. The tool allowed clients to score their own opportunities across three dimensions — cost, complexity, and ROI — and surface the use cases most worth pursuing before any deep consultant involvement.

I then presented the tool to stakeholders internally. They were immediately on board, and it was rolled out into the consultation process.

**Impact**
- Reduced consultation timelines from **four weeks to a few days**
- Clients could make faster, more confident decisions about which AI investments to pursue
- Freed up significant consultant time to focus on higher-value implementation work
- The tool is still in use today

**What This Demonstrates**
This wasn't a top-down initiative. It was a pattern I noticed, a problem I decided to own, and a solution I built without waiting for permission. That combination — seeing operational inefficiency, understanding the technical constraints, and shipping something that actually stuck, is the through-line in how I approach ops problems.

---

## How to Use These Case Studies

Each case study follows the same structure: context → problem → solution → design decisions → impact. Use them as templates for documenting your own AI applications, or as reference points for how to frame AI solutions to operational problems in interviews and stakeholder conversations.
