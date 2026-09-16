---
name: grill-me
description: Interrogate a problem statement until it is concrete enough to draft Intent from. Keeps asking until problem, stakeholders, process, baseline, value, scope, constraints, assumptions and open questions are specific. Use when a request is vague, when the blueprint package has gaps, or before drafting Intent.md.
priority: high
stage: intent
used_by: [intent-synthesis]
---

# Grill Me

Keep asking until the problem is concrete. Stop when you could draft `Intent.md` without
guessing.

## When to invoke

- A request arrives as a technology wish: "use AI for X."
- The blueprint package has `Absent` findings.
- Before drafting `Intent.md`.
- Any time you notice yourself about to fill a gap with something plausible.

## The rule

**One question at a time. Wait for the answer.**

A numbered list of fifteen questions gets one paragraph of response covering three of
them, and the other twelve are never revisited. Sequential questioning is slower and it
is the only thing that actually works, because each answer changes what you need to ask
next.

Do not accept an abstraction as an answer. "Improve efficiency" is not an answer to "what
decision gets made better?" Ask again, differently.

## The eight areas

Cover all eight. Adapt the wording; do not skip an area because it feels covered.

### 1. Problem

- What goes wrong today? Describe the last time it happened.
- Who noticed? How did they find out?
- What did it cost — time, money, risk, a bad decision?
- How often?
- What happens if nothing changes?

*Reject:* "We don't have AI in this process." That is a missing technology, not a problem.

### 2. Stakeholders

- Who experiences the problem? Name a role.
- Who decides this gets solved?
- Who would use the result, in what tool, at what point in their day?
- Who could block adoption?
- Who is accountable for the outcome improving?

*If nobody's workflow changes, question whether the use case exists.*

### 3. Process

- Walk me through the current steps.
- At each step: what information, from where, in what condition?
- Where does it break down?
- What workaround exists today? *(Someone is always doing this manually. Find them.)*
- How long does it take now?

### 4. Baseline

- What is the current number?
- How do you know? Where does the measurement come from?
- If it is not measured today, can it be? By when?
- Who owns the measurement?

*This is the most commonly skipped area and the most commonly fatal.* A value claim with
no current-state number cannot be evaluated, cannot be proven, and cannot be defended
when someone asks whether the system worked. Do not move on. If the answer is "we don't
measure that," the finding is that establishing the baseline is prerequisite work — say so.

### 5. Value

- What decision gets made better, faster, or at all?
- What is the target, as a number?
- Who benefits, and would they agree?
- How would you know within 90 days whether it worked?
- **Why AI?** What specifically requires judgment rather than a report or a rule?

*That last question deserves a real answer.* If a scheduled query and a threshold would
deliver most of the value, discovering it now costs a conversation. Discovering it after
the prototype costs a quarter.

### 6. Scope

- What is explicitly **not** included?
- What will someone ask for in the first demo that you intend to refuse?
- What is the smallest version that is still useful?
- What would make you stop?

*Push hard on out-of-scope.* An empty out-of-scope section is what makes demo feedback
unanswerable six weeks later.

### 7. Constraints

- What data exists, who owns it, what condition is it in?
- What is off limits — data, systems, actions?
- Which decisions must stay human?
- Regulatory, policy or contractual limits?
- What must this integrate with?

### 8. Assumptions and unknowns

- What are we believing without evidence?
- What would have to be true?
- What is the riskiest assumption?
- What do you not know that you would need to know?

## Stopping condition

Stop when you can answer all of these without guessing:

- [ ] The problem, as an event that happened to a named role
- [ ] The current process and where it breaks
- [ ] A baseline number, or a stated plan to establish one
- [ ] A target number with a measurement owner
- [ ] The decision that gets made differently
- [ ] Why AI rather than a simpler mechanism
- [ ] What is out of scope
- [ ] Data, ownership and condition
- [ ] The riskiest assumption

Anything still unanswerable becomes an `Intent.md` §10 open question, marked blocking or
non-blocking, addressed to whoever can answer it.

## What not to do

**Do not fill gaps.** The purpose of this skill is to *find* the gaps. An unanswered
question recorded honestly is the deliverable. A plausible invention that closes the gap
is the failure — and it is invisible, which is what makes it dangerous.

**Do not stop early because the answers are getting uncomfortable.** The discomfort is
usually where the real problem is. A stakeholder who cannot state the baseline has told
you something important about whether this project can prove its value.
