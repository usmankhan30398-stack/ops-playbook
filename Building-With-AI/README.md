# Building With AI Without Losing Control of the Output

Most people treat AI like a vending machine. You put in a big request, you get out a result, and if it's wrong you try again. That works for simple tasks. For anything complex — a multi-step workflow, a structured pipeline, a tool with real logic — it breaks down fast.

This is a practical guide to building with AI in a way that keeps you in control: constrained inputs, verifiable outputs, and no runaway hallucinations.

---

## The Core Problem: AI Doesn't Know What It Doesn't Know

When you give an AI model a large, open-ended prompt, it will always produce *something*. The model is trained to generate plausible output — not to flag when it's operating outside its reliable range. This means:

- It will fill gaps in your specification with assumptions you didn't make
- It will generate structurally correct output that is semantically wrong
- It will confidently produce details that don't exist

The solution isn't a better prompt. It's a better architecture.

---

## Principle 1: Break the Build Into Stages

Instead of asking AI to do everything in one pass, break the task into discrete stages where each stage has:

- A **narrow, well-defined input**
- A **specific, verifiable output**
- A **clear handoff condition** to the next stage

**Single-shot (fragile):**
```
"Build me a release communication pipeline that takes Linear tickets 
and generates customer notes, a changelog, a sales brief, a support FAQ, 
and a Slack announcement."
```

**Staged (controlled):**
```
Stage 1: "Given these tickets, classify each one. Output only: 
ticket ID, change type, customer visibility, risk flags. 
Nothing else."

Stage 2: "Given this classified ticket list, group tickets into themes. 
Output only: theme name, ticket IDs in that theme. Nothing else."

Stage 3: "Given this theme structure, generate customer release notes only. 
Exclude any ticket marked internal or feature-flagged."

Stage 4: "Given the same theme structure, generate the internal changelog only..."
```

Each stage is small enough to verify. If Stage 1 output looks wrong, you catch it before it cascades into five broken artifacts.

---

## Principle 2: Constrain the Output Format

Unstructured output is hard to validate and hard to pipe into the next stage. Force structure at every step.

**Unconstrained (hard to verify):**
```
"Classify this ticket and tell me about it."
```

**Constrained (verifiable):**
```
"Classify this ticket. Respond ONLY with a JSON object in this exact format:
{
  'change_type': 'feature | bug_fix | infrastructure | improvement',
  'customer_impact': 'high | medium | low | none',
  'visibility': 'visible | internal',
  'risk_flags': ['breaking_change', 'feature_flagged', 'requires_migration']
}
Do not include any explanation or additional text."
```

When the output format is fixed, you can programmatically validate it, catch deviations immediately, and pass it cleanly to the next step.

---

## Principle 3: Make Constraints Explicit, Not Implied

AI will follow implicit conventions — but it will also violate them when it finds a plausible reason to. Explicit constraints are harder to override.

**Implied (fragile):**
```
"Generate customer release notes. Don't include internal stuff."
```

**Explicit (controlled):**
```
"Generate customer release notes using only tickets where visibility = 'visible' 
and risk_flags does not contain 'feature_flagged'.

If a ticket has risk_flag 'breaking_change', include a migration note inline 
immediately after that item.

Do not reference ticket IDs, PR numbers, or internal system names. 
Do not mention features that are partially rolled out."
```

Every constraint you leave implicit is a decision you're delegating to the model.

---

## Principle 4: Separate Classification from Generation

One of the most common failure modes in AI-powered workflows is asking the model to classify *and* generate in the same step. When classification is wrong, generation is built on a bad foundation — and the error is invisible in the output.

**Combined (error-prone):**
```
"Read these tickets and write customer release notes."
```

**Separated (traceable):**
```
Step 1 — Classify: "For each ticket, determine: is this customer-visible? 
What is the change type? Are there risk flags?"

Step 2 — Review: [Human or programmatic check of classification output]

Step 3 — Generate: "Using only the pre-classified data, generate release notes."
```

The classification step becomes an artifact you can inspect. If the notes look wrong, you check the classification first.

---

## Principle 5: Ask the Model to Flag Its Own Uncertainty

AI models don't volunteer uncertainty — they resolve it silently. You can counteract this by explicitly requiring uncertainty flags.

```
"Classify the following ticket. If you cannot determine the correct 
classification with high confidence, add 'needs_review': true to the output 
and explain what information is missing."
```

This turns silent failures into visible flags. A ticket marked `needs_review` is infinitely better than a ticket confidently misclassified.

---

## Putting It Together: A Real Example

> **Note:** The following is a concrete example of these principles applied to a real project — a release communication pipeline that ingests Linear tickets and generates five audience-specific artifacts. If you're building something different, the stages and constraints will look different, but the underlying approach is the same.

The release communication pipeline was built using exactly these principles. Here's how the architecture maps:

| Stage | Input | Constraint | Output |
|---|---|---|---|
| Batch classification | Raw Linear tickets | Strict JSON schema, enumerated values only | Classified ticket objects |
| Theme grouping | Classified tickets | Each ticket assigned to exactly one theme | Theme → ticket mapping |
| Structured release object | Themes + metadata | Single source of truth, no independent generation | Release object |
| Parallel generation | Release object only | Audience-specific rules, explicit exclusion logic | 5 artifacts |

Every output reads from the structured release object. Nothing is generated from raw input twice. If a ticket is misclassified, the error is traceable back to Stage 1 — not buried inside five different documents.

---

## Summary

| Risky Pattern | Controlled Alternative |
|---|---|
| One large prompt | Break into stages with narrow inputs |
| Unstructured output | Enforce JSON or fixed schema |
| Implicit constraints | State every rule explicitly |
| Classify + generate together | Separate into distinct steps |
| Assume model confidence | Require uncertainty flags |

The goal isn't to distrust AI — it's to build systems where errors are visible, traceable, and fixable. That's not different from how you'd design any reliable process.
