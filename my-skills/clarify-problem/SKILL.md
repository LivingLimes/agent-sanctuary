---
name: clarify-problem
description: Extracts what the user actually wants instead of what they think they should want, surfaces baked-in assumptions after confirmation, identifies the binding decision criterion, proposes the best next action for user approval, and saves a markdown decision artifact. Achieves this through one-question-at-a-time interview until ~95% confidence about the underlying intent and decision pressure. Use when an ask is underspecified ("build me X" without "for whom" or "why now"), when the user explicitly invokes ("interview me", "grill me", "are we sure?", "stress-test my thinking"), or when you catch yourself silently filling in ambiguous requirements before any plan, spec, or code exists.
---

# Clarify Problem

## Overview

What people ask for and what they actually want are different things. They ask for "a dashboard" because that's what someone asks for, not because a dashboard solves their problem. They say "make it faster" without a metric to hit.

The cheapest moment to find this gap is before any plan, spec, or code exists. Once you've started building, switching costs are real, and the user will rationalize the wrong thing into a "good enough" thing. The misfit gets locked in.

This skill closes the gap before it costs anything. The other Define-phase skills assume you already know roughly what you want: `idea-refine` generates variations from an idea, `spec-driven-development` writes the requirements down, `doubt-driven-development` stress-tests a plan after you've drafted one. Clarify-problem is the part before all of those, where you ask one question at a time, with your best guess attached, until you can predict what the user is going to say before they say it.

The final output is not just shared understanding. The skill must help the user choose the best next action: build, validate, research, write a spec, reduce scope, defer, kill, or do nothing. The agent recommends; the user decides.

## When to Use

Apply this skill when:

- The ask is missing at least one of: **who** the user is, **why** they want it, what **success** looks like, what the binding **constraint** is
- The request is conventional rather than specific ("build me X", "make it faster") and you can't unpack the convention without guessing
- You're tempted to start with assumptions you haven't surfaced
- The user hasn't said which value they're optimizing for when two reasonable ones are in tension (simplicity vs. flexibility, cost vs. speed, learning vs. shipping, validation vs. momentum)
- The user explicitly invokes: "clarify problem", "grill me", "before we start, are we sure?", "stress-test my thinking"

**When NOT to use:**

- The ask is unambiguous and self-contained ("rename this variable", "fix this typo")
- The user has explicitly asked for speed over verification
- Pure information requests ("how does X work?", "what does this code do?")
- Mechanical operations (renames, formats, file moves)
- You already have ≥95% confidence; re-read the stop condition below before assuming you don't

## Loading Constraints

This skill needs a live, responsive user. **Do not invoke in non-interactive contexts** like CI pipelines, scheduled runs, `/loop`, or autonomous-loop. If you're in one of those and the ask is underspecified, flag that as a blocker for the user instead of guessing.

## The Process

### Step 1: Hypothesize, with a confidence number

Before asking anything, write down your current best read of what the user wants in **one sentence**, plus an honest confidence number (0–100%):

```
HYPOTHESIS: You want a way to answer "how are we doing?" in standup, and "dashboard" was the convention that came to mind.
CONFIDENCE: ~30% — missing: who it's for, what "metrics" means in context, and what success looks like
```

The number forces honesty. If you wrote down a high number but can't actually predict the user's reactions to the next three questions you'd ask, the number is wrong. Start at the confidence level you can defend.

When confidence is below ~70%, append a brief reason on the same line — what's still unresolved or missing. This tells the user exactly what the interview needs to surface, and prevents the number from being a vague signal.

### Step 2: Ask one question at a time, each with a guess attached

Format:

```
Q: <one focused question>
GUESS: <your hypothesis for the answer, with the reasoning that produced it>
```

Wait for the user to react before asking the next question.

**Why one at a time, not a batch:**

- The user can't react to your hypotheses if you bury them in a list
- Batches encourage skim-reading and surface answers
- The third question often depends on the answer to the first; asking them all at once locks in the wrong framing
- The user's energy for thinking carefully is finite; spend it one question at a time

**Why attach a guess:**

- The user reacts faster to a wrong guess than they generate an answer from scratch
- It commits you to a hypothesis you can be visibly wrong about, which keeps you honest
- It surfaces *your* assumptions, which is what the interview is meant to expose

The risk here is a polite user agreeing with your guess to be agreeable. Mitigate by being visibly willing to be wrong, and occasionally guess in a direction you expect the user to push back on.

### Step 3: Listen for "want vs. should want"

The most dangerous answers are the ones where the user says what a thoughtful answer *sounds like* rather than what they actually want. Watch for:

- Answers that pattern-match best-practice talk ("I want it to be scalable", "clean architecture") without specifics
- Answers that defer to convention ("the way most apps do it", "the standard approach")
- Phrases like "I should probably…", "I think I'm supposed to…", "good engineering practice says…"
- Buzzwords as goals — when "modern", "scalable", "robust" are the answer instead of a specific outcome

When you hear these, the question to ask is:

> *"If you didn't have to justify this to anyone, what would you actually want?"*

That single question often does more work than the previous five.

### Step 4: Identify the binding decision criterion

Before restating the intent, make sure you understand what should decide the next action. This is not another intake form. It is the tie-breaker the agent will use when choosing between plausible actions.

Examples:

- Move fast
- Avoid wasted work
- Learn by building
- Validate demand before building
- Reduce scope
- Reduce risk
- Preserve optionality
- Make something shippable
- Produce a reviewable artifact

If the user has already made the criterion obvious, state it as part of the restate. If it is missing or there are competing criteria, ask one focused question with a guess attached:

```
Q: If we have to choose, what matters more right now: learning by building, or avoiding wasted work by validating first?
GUESS: avoiding wasted work, because you've pushed several times on whether this is the right thing to build at all.
```

Do not wait until the final artifact to discover this. The binding decision criterion is part of the 95% confidence target.

### Step 5: Restate intent in the user's own words

When your confidence is high, write back what you now think the user wants. Keep it tight (5–8 lines), use their language where possible, and structure it so the user can confirm or correct line by line:

```
Here's what I now think you want:

- Outcome:      <one line>
- User:         <one line — who benefits>
- Why now:      <one line — what changed>
- Success:      <one line — how we know it worked>
- Constraint:   <one line — the binding limit>
- Decision:     <one line — the criterion that should decide the next action>
- Out of scope: <one line — what we're explicitly not doing>

Yes / no / refine?
```

Including "Out of scope" is non-negotiable. Half of misalignment is silent disagreement about what is *not* being built.

### Step 6: Confirm — explicit yes, not "whatever you think"

The gate is an explicit "yes." The following are **not** yes:

- "Whatever you think is best." → The user is delegating, which means they don't have 95% confidence either. Re-ask with two concrete options framed as a choice.
- "Sounds good." → Ambiguous. Ask: "Anything you'd refine?" Silence isn't confirmation.
- "Sure, let's go." → Often a polite exit, not an endorsement. Same follow-up.
- Silence followed by "okay let's start." → The user has given up on the interview, not converged. Stop and ask whether you've missed something.

If they correct you, fold the correction in and restate. Loop until you get an explicit yes.

### Step 7: Surface baked-in assumptions after confirmation

After the user explicitly accepts the restated intent, list the assumptions that are now baked into the desired outcome. This is not another requirements survey. It is a challenge pass: make the hidden premises visible so the user can decide whether any of them deserve pushback before the intent flows downstream into a spec, plan, or implementation.

Format:

```
ASSUMPTIONS BAKED INTO THIS INTENT:
1. <assumption the plan now depends on>
2. <assumption the user may not have noticed they accepted>
3. <assumption that could change the shape of the solution>

MOST FRAGILE ASSUMPTION: <the one you would challenge hardest, with one sentence why>

Want to challenge any of these before we hand this off?
```

Keep the list short enough to be usable: usually 5-8 assumptions. Prefer assumptions that would materially change the direction if false:

- User or audience assumptions ("the buyer and user are the same person")
- Success assumptions ("speed matters more than completeness")
- Constraint assumptions ("a few weekends is enough")
- Scope assumptions ("a simplified version still teaches the real concept")
- Adoption assumptions ("people will use this in the moment where it matters")
- Trade-off assumptions ("public polish is less important than learning value")

**Assumption vs. fact guardrail:** Do not relabel confirmed user statements as assumptions. If the user says "this is for learning," that is now a fact of the clarified intent. The assumption is a dependency that could still be false, such as "building is the best way for this person to learn." A good assumption list contains hidden premises, not a recap of everything the user already confirmed.

**Paralysis guardrail:** Challenge at most the 1-2 most fragile assumptions unless the user asks for a deeper teardown. The goal is to expose load-bearing uncertainty before handoff, not trap the user in endless reconsideration. If the user accepts the assumptions or asks to move on, proceed.

If the user challenges an assumption, fold the correction back into the restate and ask for explicit confirmation again.

### Step 8: Propose the best next action and get user approval

After the user accepts the assumptions or asks to move on, synthesize the confirmed intent, binding decision criterion, and assumption inventory into a proposed next action. Do not re-ask questions already answered during clarification. Ask only if the decision cannot be made from the confirmed intent.

The recommendation should propose one best next action from this action set:

- Build
- Validate first
- Research
- Write a spec
- Reduce scope
- Defer
- Kill
- Do nothing

Format:

```
RECOMMENDED NEXT ACTION: <one action>
WHY THIS: <1-2 sentences tied to the binding decision criterion>
WHY NOT THE ALTERNATIVES:
- <alternative> — <why not now>
- <alternative> — <why not now>

CHEAPEST TEST: <smallest action that tests the fragile assumption>
DECISION THRESHOLD: <evidence that would change the recommendation>
TIMEBOX: <how long to try before revisiting>

Approve this next action, or would you change it?
```

The user owns the final choice. If they approve the recommendation, save it as the chosen action. If they refine or override it, update the action and rationale to match what they want, then save the artifact. Do not save the artifact until the next action has explicit user approval.

### Step 9: Save the approved decision artifact

Save a markdown file in the repository:

```
.sdas/<problem-slug>/decision.md
```

Use the repository root if it can be detected; otherwise use the current working directory. Derive `<problem-slug>` from the confirmed problem or outcome, not from the original underspecified ask. Use lowercase kebab-case, keep it short, and avoid generic names like `app`, `dashboard`, or `project`.

The artifact must use this structure:

```markdown
# Clarified Problem: <short title>

## Confirmed Intent
- Outcome:
- User:
- Why now:
- Success:
- Constraint:
- Binding decision criterion:
- Out of scope:

## Assumptions Baked Into This Intent
1.
2.
3.

## Most Fragile Assumption

## Chosen Next Action

## Why This Action

## Alternatives Rejected
- <alternative> — <why not now>

## Cheapest Test

## Decision Threshold

## Timebox

## Downstream Handoff
<spec-driven-development / idea-refine / planning-and-task-breakdown / doubt-driven-development / none, with one sentence why>
```

If filesystem writes are unavailable, present the full markdown artifact in the response and name the intended path.

### The 95% Confidence Stop

You're done when you can answer yes to this:

> *Can I predict the user's reaction to the next three questions I would ask?*

If yes, you have shared understanding. Stop interviewing, produce the restate including the decision criterion, get explicit confirmation, surface the baked-in assumptions, propose the best next action, get explicit approval or correction, and save the markdown artifact. If no, you're not done; ask the next question.

This is a checkable test, not a vibe. It also has a floor: if you've gone several rounds and still can't predict, that's information about the ask, not a reason to keep grinding. Stop and tell the user: "I've asked X questions and I still can't predict your reactions. Something foundational is missing. Want to step back?"

## Output

The output of this skill is a **saved decision artifact**: the restate from Step 5, with an explicit yes from Step 6, followed by the baked-in assumptions from Step 7, the user-approved next action from Step 8, and the saved artifact from Step 9.

After the next action is approved, always save the artifact to `.sdas/<problem-slug>/decision.md` when repository filesystem access is available. Specs, plans, and task lists are downstream; they consume the decision artifact this skill produces.

## Example

A short before-and-after.

**Without `clarify-problem`:**

```
User:  build me a dashboard for our metrics
Agent: <starts proposing chart libraries and layouts>
```

The agent has assumed: who it's for, what metrics, what "for our team" means, what success looks like. None of these are stated. Every assumption is a place the user can later say "that's not what I meant."

**With `clarify-problem`:**

```
User: build me a dashboard for our metrics

Agent:
HYPOTHESIS: You want a way to answer "how are we doing?" in standup, and "dashboard" came to mind because that's the conventional answer.
CONFIDENCE: ~30% — missing: who it's for, what "metrics" are, and what success looks like

Q:     When you say "how are we doing?", who's asking — you alone, the engineering team in standup, or up the chain (e.g., your manager, an exec, a board)?
GUESS: engineering team in standup, because "we" usually scopes that way and standups are where this question gets asked. If it's for execs the metrics and the framing change a lot.

User: It's actually for me. I keep losing track of which experiments are running and what their early signal looks like.

Agent:
Updated read: not a team dashboard at all. It's a personal experiment tracker. Confidence: ~60% — still missing: what "early signal" means and what done looks like.

Q:     Is the gap that you don't know which experiments exist, or that you can't see their results in one place?
GUESS: the second one. You have a list of experiments somewhere, but their results live in five different tools, and reconciling them by hand is what you're tired of.

User: First one actually. I literally don't have a list. They're spread across different docs.

Agent:
Updated read: you need a personal experiment inventory before you need a dashboard. Confidence: ~78% — still missing what "good enough" looks like.

Q:     Would success be "I can see every active experiment in one place and decide what needs attention this week"?
GUESS: yes. The pain sounds like losing track, not performing detailed analysis.

User: Yes. I just want a weekly view of what's active, owner, next check-in, and current signal.

Agent:
Here's what I now think you want:

- Outcome:      A personal weekly experiment tracker, not a metrics dashboard.
- User:         You, as the person trying to keep active experiments visible.
- Why now:      Experiments are scattered across docs and you no longer trust your memory.
- Success:      You can see active experiments, owners, next check-ins, and current signal in one place.
- Constraint:   Keep it lightweight enough to maintain weekly.
- Decision:     Avoid building the wrong thing; choose the smallest action that proves the workflow will stay useful.
- Out of scope: Automated analytics, executive reporting, and complex charting.

Yes / no / refine?

User: Yes.

Agent:
ASSUMPTIONS BAKED INTO THIS INTENT:
1. Manual weekly maintenance is acceptable and will not become the new bottleneck.
2. A lightweight tracker solves more of the pain than automated metrics collection.
3. "Current signal" can be represented in a simple human-written status field.
4. The experiment list is more valuable right now than historical trend analysis.
5. You are the primary user, so collaboration features can wait.

MOST FRAGILE ASSUMPTION: Manual weekly maintenance is acceptable. If the tracker is tedious to update, it will become another stale doc.

Want to challenge any of these before we hand this off?

User: No, move on.

Agent:
RECOMMENDED NEXT ACTION: Validate first.
WHY THIS: The binding criterion is avoiding wasted work, and the fragile assumption is whether manual weekly maintenance will hold up.
WHY NOT THE ALTERNATIVES:
- Build — too easy to create another stale tracker before proving the update habit works.
- Write a spec — premature until the weekly workflow has survived at least one real pass.

CHEAPEST TEST: Run one manual weekly experiment review in a plain markdown table.
DECISION THRESHOLD: If the review takes more than 20 minutes or misses active experiments, reduce scope or change the capture mechanism.
TIMEBOX: One week.

Approve this next action, or would you change it?

User: Approved.

Agent:
Saved: .sdas/personal-experiment-tracker/decision.md
```

Two questions in, the agent has discovered the actual ask isn't "a dashboard." It's "a list." Different artifact, different scope, different work. The dashboard would have been wrong.

## Interaction with Other Skills

- **`idea-refine`**: downstream. If the confirmed intent is "I want X but I don't know how to scope it," hand off to `idea-refine` to generate variations against the now-explicit intent.
- **`spec-driven-development`**: downstream. If the confirmed intent is concrete ("I want X for Y users with Z success criteria"), hand off to `spec-driven-development` to write it down.
- **`planning-and-task-breakdown`**: two hops downstream of this skill (after the spec).
- **`doubt-driven-development`**: opposite end of the timeline. Clarify-problem is pre-decision intent extraction; doubt-driven is post-decision artifact review. The Step 7 assumption inventory is a lightweight challenge pass; if the user wants an adversarial review of the confirmed intent or a downstream plan, hand off to `doubt-driven-development`.
- **`source-driven-development`**: orthogonal. Clarify-problem clarifies what the user wants; SDD verifies framework facts. They don't compete.
- **No downstream skill**: valid when the best next action is defer, kill, do nothing, or run a small offline validation test.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The ask is clear enough" | If you can't write the user's desired outcome in one sentence right now, the ask isn't clear. Run Step 1 before deciding. |
| "Asking too many questions wastes their time" | Time wasted by 4–6 targeted questions is small. Time wasted by building the wrong thing is enormous, and the user is the one bearing that cost. |
| "I'll figure it out as I build" | Switching costs after code exists are 10x what they are now. Discovery during implementation is rework. |
| "They said 'whatever you think,' so I should just decide" | "Whatever you think" is delegation, not decision. Re-ask with two concrete options as a choice. |
| "I should give them several options to pick from" | Options work when the user knows what they want and is choosing between trade-offs. They don't know what they want yet. Listing options widens the search; asking narrows it. |
| "If I attach my guess, I'm leading them" | Leading is the point. Reacting is faster than generating from scratch. The risk is sycophancy, not leading; mitigate by being visibly willing to be wrong. |
| "We've talked enough, I get it" | Test it: can you predict their reaction to the next three questions? If not, you don't get it yet. |
| "The user said yes, we're done" | If the yes followed a vague restate or an open-ended "sounds good," the yes is hollow. Restate concretely and re-confirm. After a real yes, still surface baked-in assumptions before handoff. |
| "The next action is obvious" | Obvious compared to what? Name the rejected alternatives and tie the recommendation to the user's binding decision criterion. |
| "I'll ask for decision criteria at the end" | Too late. The criterion is part of clarification; use it to shape the restate and the recommendation. |
| "The agent recommended it, so that's the decision" | No. The recommendation is a proposal. The final decision is whatever the user explicitly approves or corrects. |

## Red Flags

- Three or more questions in a single message: that's batching, not interviewing
- A question without your hypothesis attached: that's surveying, not committing
- Accepting "whatever you think is best" as a terminal answer
- Producing a spec, plan, or task list before the user has explicitly confirmed your restate
- Questions framed as "what would be best practice?" instead of "what do you actually want?"
- The user gives a sophistication-signaling answer ("scalable", "clean", "modern") and you accept it without probing whether it's what they actually want
- Three or more rounds without your confidence visibly rising: you're asking the wrong questions, step back and reframe
- A confidence number below ~70% with no reason attached: the user can't help close the gap if they don't know what's missing
- Saving the intent doc before the user has confirmed (the doc itself implies a yes the user didn't give)
- Skipping the "Out of scope" line in the restate (silent disagreement about non-goals is half of misalignment)
- Skipping the assumption inventory after the user confirms the restate
- Treating the assumptions list as a reason to block progress, instead of a final chance for the user to challenge hidden premises
- Recommending a next action without naming the binding decision criterion
- Saving the artifact before the user approves, refines, or overrides the recommended next action
- Re-asking "who is this for?" or "what is success?" in Step 8 after those were already confirmed
- Failing to save `.sdas/<problem-slug>/decision.md` when filesystem access is available

## Verification

After applying clarify-problem:

- [ ] An explicit hypothesis with a confidence number was stated in the first turn
- [ ] Every confidence number below ~70% was accompanied by a one-line reason (what's still unresolved or missing)
- [ ] Questions were asked one at a time, each with the agent's guess attached
- [ ] At least one "what would you actually want if you didn't have to justify it?" probe ran when the user gave a sophistication-signaling or convention-signaling answer
- [ ] The binding decision criterion was discovered during the interview, not bolted on at the end
- [ ] A concrete restate (Outcome / User / Why now / Success / Constraint / Decision / Out of scope) was written back to the user
- [ ] The user confirmed the restate with an explicit yes (not "whatever you think," not "sounds good," not silence)
- [ ] After confirmation, baked-in assumptions were listed and the most fragile assumption was identified
- [ ] The assumption inventory separated confirmed facts from hidden premises that could still be false
- [ ] The challenge pass focused on the 1-2 most fragile assumptions unless the user asked for a deeper teardown
- [ ] If the user challenged an assumption, the correction was folded back into the restate and re-confirmed
- [ ] A best next action was recommended from the action set and justified against the binding decision criterion
- [ ] The user explicitly approved, refined, or overrode the recommended next action before the artifact was saved
- [ ] Rejected alternatives, cheapest test, decision threshold, and timebox were included
- [ ] The final artifact was saved to `.sdas/<problem-slug>/decision.md`, or the intended path was named if filesystem writes were unavailable
- [ ] At the stop point, the agent could predict reactions to the next three questions it would ask
- [ ] Any handoff to a downstream skill (`idea-refine`, `spec-driven-development`, `planning-and-task-breakdown`, `doubt-driven-development`) was framed in terms of the saved decision artifact, not the original underspecified ask
