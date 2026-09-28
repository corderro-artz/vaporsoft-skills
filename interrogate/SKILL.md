---
name: interrogate
description: Surfaces unstated requirements before planning or writing any code for a feature, bug fix, or design decision — ambiguous scope, unspecified edge cases, unclear success criteria, or multiple valid interpretations of the request. Use this whenever a request involves a design decision, not just before starting a big feature: a wrong guess costs a full rewrite, while a batch of upfront questions costs a few turns. Skip only for a single unambiguous action with no design decisions in it (rename this variable, fix this typo, bump this version). Trigger on phrases like "build a", "add a feature for", "implement", "I want a system that", or any request whose shape depends on unstated preferences.
---

# Interrogate

Questions cost a few turns now; a wrong implementation costs a full rewrite later. This skill exists to stop guessing at what the user actually wants and instead find out, cheaply, before committing to a plan.

## When to use

Invoked with a feature, bug fix, or design request — before creating a plan, a TodoWrite list, or editing any file.

Skip only if the request is a single unambiguous action with no design decisions in it (e.g. "rename this variable," "fix this typo," "bump the version to 2.1"). If in doubt, err toward using it: the cost of one skipped round of questions is much higher than the cost of an unnecessary one.

## Procedure

1. **Read first.** Check existing relevant files, `CLAUDE.md` (or `AGENTS.md` if that's what the project uses), and any prior planning notes or design docs for this project before asking anything. Don't ask what's already answered on disk or earlier in the conversation.

2. **Find the gaps.** Look for unstated decisions across these angles, and keep only the ones that actually apply to this request:
   - **Scope** — what's explicitly in vs. out of this request?
   - **Inputs/outputs** — exact shape, format, types?
   - **Edge cases** — empty input, error states, concurrent use, existing data?
   - **Constraints** — performance, compatibility, must reuse an existing pattern?
   - **Success criteria** — how will the user know this is done and correct?
   - **Non-goals** — what looks related but should *not* be touched?

3. **Ask in one batch.** Post all questions together in a single message — never dribble them out one at a time, which burns turns and makes the user re-explain context repeatedly. If the `AskUserQuestion` tool is available, prefer it: it renders as selectable options plus free text, which is faster for the user than typing full sentences and gives you structured answers back. Otherwise, post a single numbered list. If genuinely nothing is ambiguous, say so explicitly and skip straight to step 5.

4. **Wait for answers.** Do not plan or write code until the user responds. If the answers open new gaps, ask one tighter follow-up round — same format as step 3. There's no fixed round limit — use judgment: most requests resolve in one or two rounds. If you're still finding new open questions after several rounds, that's usually a sign the request itself is underspecified at a level the user needs to resolve by narrowing scope, not something more questions will fix. When you reach that point, state your remaining assumptions explicitly and move to step 5 rather than continuing to ask.

5. **Confirm shared understanding.** Summarize the agreed spec back in a short list: what will be built, what won't, and the key decisions from the answers (including any assumptions you're carrying forward because further questions stopped paying off). Get explicit go-ahead — "yes," "go," "correct" — before implementing.

6. **Then proceed.** Normal workflow (TodoWrite plan, any project-specific task log, implementation) starts only after step 5 is confirmed.

## Rules

- Never guess a requirement and silently proceed past it — ask instead.
- Don't ask questions already answerable from the codebase, `CLAUDE.md`/`AGENTS.md`, or earlier in the conversation.
- Don't pad the batch with questions whose answer wouldn't change the implementation.
- Don't stretch this into an interview for its own sake — once the shape of the implementation is clear, stop asking and confirm.
