# Agent Council

A multi-agent advisory pattern for hard architectural decisions. **Experimental** — use as a tool to think through tough calls, not as a load-bearing automation.

## What it does, in plain English

You face a hard decision with lots of competing trade-offs. You don't want to just go with your gut and you don't want to ask a single AI for an answer either, because a single AI gives you one perspective.

You run `/agent-council`. Seven AI agents wake up:

- **Five specialists** — each obsessed with one thing only: **Quality**, **Cost**, **Speed**, **Stability**, **Security**. They each write their take on the decision, then read each other's takes and argue back, defending their corner. They have to keep at least one disagreement — they're not allowed to merge into a hive mind.
- **An architect** — reads all five arguments and writes one clear plan. Resolves the contradictions, calls out the open questions for you.
- **A reviewer** — tears the architect's plan apart. Looks for hand-waving, made-up numbers, things the architect skipped. If anything serious comes back, the architect rewrites and the reviewer reviews again. Hard cap of two review cycles so it doesn't spiral.

You get a Plan of Record back. **You read it. You decide.** The council never decides — it advises.

That's the whole thing.

## How long it takes / what it costs

About 25 minutes happy path, up to 40-55 minutes worst case. Roughly $1.74-$2.10 per run (placeholder until enough real runs accumulate to publish proper figures). Hard cap of $5.22 per run with an automatic abort if a run is forecast to exceed that.

## When to use it

- A decision that's hard to reverse
- A decision that touches lots of downstream systems
- A decision with three or more competing trade-offs (cost vs speed vs reliability, etc.)
- You have time to wait — this is async by design

## When NOT to use it

- Quick lookups or tactical edits
- Decisions where you already know the answer
- When you're waiting in a chat conversation — 25 minutes is too long; ask a single AI instead
- On itself, more than once — the pattern has a self-blindness limit

## How to invoke

The skill is invoked manually with `/agent-council` (or however your harness wires up Claude Skills). There is no automatic firing — the user knows when a decision is worth the wait. If you want it to fire automatically on certain conditions, write your own wrapper.

## Status

This is **experimental**. It has been field-tested twice (once on a real architecture decision, once on this skill's own spec) and the second run caught the first run's failure mode in subtler form. The pattern has been hardened against the failure modes that actually surfaced, but it has not been validated at scale.

Use it as a tool to think with, not as a process gate.

## What's in this repo

- [`SKILL.md`](SKILL.md) — the full specification: rounds, briefs, stop rules, resilience, adversarial hardening, telemetry, history.
- `README.md` — this file.
- `LICENSE` — MIT.

## A note on the failure mode this skill is designed to catch

Single-AI plans tend to paper over hard trade-offs. The architect agent will say "we'll figure that out later" or "this is a placeholder we'll calibrate" instead of resolving the actual question. The two-reviewer-pass machinery is specifically there to catch that pattern — and it does, even when the architect is reviewing its own work. The non-skippable human-decides rule is the final backstop: any council output is a recommendation to the human, not an instruction to the system.

## Forks

If you fork this skill, please rename and re-version. The skill has no built-in integrity check — a fork that quietly modifies the architect's prompt could exfiltrate context. Treat the upstream `SKILL.md`'s SHA-256 as the canonical fingerprint.

## License

MIT. See `LICENSE`.
