# 2026 (rolling) — Evaluation infrastructure for coding agents

## Why this matters

The
[agentic engineering umbrella entry](./2026-agentic-engineering.md)
listed seven sub-disciplines and flagged **evaluation and
feedback** as the gap with no dedicated entry yet. This entry
fills that gap.

The argument for why evaluation deserves its own entry: by
2026 it's clear that **the bottleneck on agentic-engineering
practice isn't model quality — it's the ability to tell
whether the agent is doing the right thing without re-reading
every output.** Models change every few months;
[harness improvements outperform model swaps](./2026-harness-engineering.md)
on the same model; what holds the whole thing together is
*knowing* which configuration of model + harness + skill +
hook actually performs better.

Without evaluation discipline, you optimise on vibes. With it,
you can ship and iterate. The 2026 evaluation toolchain isn't
mature, but the patterns are converging.

## The dominant benchmark — SWE-bench Verified

**SWE-bench Verified** is the canonical 2026 benchmark for
coding agents. It refines the original SWE-bench
(a harvest of real GitHub bug-fix tasks) through human
validation — each task is verified to have a clear, testable
fix, with a working solution that passes a defined test suite.

Why Verified specifically:

- The original SWE-bench had ambiguous tasks (bug reports
  without enough information to determine the right fix) and
  noisy ground truth (tests that passed for the wrong reasons).
  Verified filters these out.
- Vendor numbers are reported on Verified in 2026, not the
  original. When you see "X% on SWE-bench," it's almost always
  Verified.
- The 2026 distribution: top models score in the **70–80% range**
  on Verified. Claude Opus 4.7 lands around 81%; that's
  approximately where the SOTA cluster sits.

The headline shift since the 2024 SWE-bench inflection (when
Devin's 13.86% was state-of-the-art): the benchmark is *not
saturated*. The remaining ~20% of tasks are where the
disagreement-with-humans, the multi-file-coordination, and the
genuine-design-judgement live. A model that scored 100% on
Verified would be a meaningfully different kind of system from
what exists today.

## Vertical-specific benchmarks

SWE-bench Verified is good for general coding-agent comparison.
For vertical surfaces (finance, design, biotech), it's not
informative. The 2026 pattern: each vertical develops its own
benchmark.

| Benchmark | Vertical | Surfaced in |
|---|---|---|
| **SWE-bench Verified** | General software engineering | Most public model comparisons |
| **Vals AI Finance Agent** | Financial analysis | [Claude for Finance entry](./2026-05-claude-for-finance.md) (Opus 4.7 leads at 64.37%) |
| **HumanEval / MBPP / LiveCodeBench** | Function-level code generation | Older / for narrower comparisons |
| **WebArena / VisualWebArena** | Browser-agent navigation | Webwright-shape evaluations |
| **AgentBench / GAIA** | General-purpose agentic reasoning | Cross-vendor agent comparison |
| **TerminalBench** | CLI-agent workflows | Emerging; relevant for CC/Codex/Hermes |

The trend: **vertical evaluation is fragmenting the way
horizontal evaluation has converged.** SWE-bench Verified is the
shared coding-agent reference; everything else is sector-specific
and harder to compare across.

## LLM-as-judge — and its problems

The standard 2026 pattern for *production* evaluation (your
own work, not benchmark leaderboards): **LLM-as-judge**. A
strong model (GPT-5, Opus 4.7, equivalent) is prompted to rate
or rank the outputs of another model based on quality criteria
you define.

The pattern is appealing because it's cheap, scalable, and
doesn't require human labellers. The 2026 reality check on its
limits:

- **Systematic biases.** LLM judges favour outputs that match
  their own style, longer outputs over shorter, confident-
  sounding answers over hedged-but-correct ones.
- **Error rates exceed 50% on complex evaluation tasks** per the
  *JudgeBench* arxiv paper
  ([2410.12784](https://arxiv.org/pdf/2410.12784)) and follow-up
  work — when the judging task is itself hard, the LLM judge is
  unreliable.
- **Approximately 64–68% agreement with domain experts** in
  specialised domains (medicine, law, finance). That's "useful
  signal but not authoritative."

The 2026 best-practice response, per the
[Galileo agent-evaluation framework
writeup](https://galileo.ai/blog/agent-evaluation-framework-metrics-rubrics-benchmarks):
**don't use a single judge for production decisions.** Combine
LLM-judge signal with rubrics, structured metrics, and periodic
human-expert calibration. Aim for 0.80+ Spearman correlation
with expert judgement before treating LLM-judge output as
load-bearing.

## The 2026 evaluation discipline (what *good* looks like)

Converged practice from multiple
[2026 arxiv surveys](https://arxiv.org/html/2503.16416v2):

1. **Define success criteria *before* running anything.** Not
   "did Claude do a good job," but "did the agent's output pass
   these three concrete tests."
2. **Layer the evaluation.** Cheap-and-broad first
   (rule-based / exact-match), expensive-and-deep second
   (LLM-judge), human-expert spot-checks for calibration.
3. **Track regressions across model versions.** Same eval suite,
   same agent, different model — measure the delta. This is
   what catches the "Opus 4.6 → 4.7 broke X" regressions before
   they ship to production.
4. **Don't trust a single eval.** Aggregate across at least
   three orthogonal signals. The signals don't all have to be
   automated; manual code review is still the gold standard for
   high-stakes outputs.

## What this means for Claude Code use

Three practical implications:

1. **Treat hooks as the evaluation surface.** A
   [`PostToolUseFailure` hook](./2026-hooks-and-effort.md)
   logging every failure is the cheapest possible evaluation
   pipeline. You can do statistical work on that log later.
2. **Build a regression suite for your CLAUDE.md.** A small set
   of test prompts you run after every meaningful CLAUDE.md
   edit. Watch for drift in what the agent does on a
   known-good case.
3. **For multi-agent setups: the orchestrator is the judge.**
   See the
   [Managed Agents entry](./2026-04-managed-agents-sdk.md) and
   the [harness engineering entry's three-agent
   pattern](./2026-harness-engineering.md) — the evaluation
   agent is the third agent in the planning + generation +
   evaluation triple, and it runs in its own isolated context
   precisely because it needs to judge independently of how the
   work got done.

## Try this

1. **Find your own SWE-bench Verified result.** The benchmark
   is reproducible. Run a subset of tasks through your own
   Claude Code setup; compare against Anthropic's reported
   numbers. The delta is your harness's effect.
2. **Set up one LLM-judge eval** for a recurring task you
   care about. Compare its judgements to your own on the same
   outputs for a week. Note where it agrees with you and where
   it doesn't.
3. **Read [the *Survey on Evaluation of LLM-based
   Agents*](https://arxiv.org/html/2503.16416v2)** for the
   academic-rigorous version of this entry. It's longer; it's
   more thorough; it's where the field's consensus actually
   lives.

## Sources

- [SWE-bench Verified](https://www.swebench.com/) — canonical
  coding-agent benchmark with human-validated tasks
- *A Survey on Evaluation of LLM-based Agents* — arxiv
  2503.16416: <https://arxiv.org/html/2503.16416v2>
- *JudgeBench: A Benchmark for Evaluating LLM-based Judges* —
  arxiv 2410.12784: <https://arxiv.org/pdf/2410.12784>
- *When AIs Judge AIs: The Rise of Agent-as-a-Judge Evaluation
  for LLMs* — arxiv 2508.02994:
  <https://arxiv.org/pdf/2508.02994>
- *Are We on the Right Way to Assessing LLM-as-a-Judge?* —
  arxiv 2512.16041: <https://arxiv.org/pdf/2512.16041>
- *How to Build an Agent Evaluation Framework With Metrics,
  Rubrics, and Benchmarks* — Galileo:
  <https://galileo.ai/blog/agent-evaluation-framework-metrics-rubrics-benchmarks>
- *Automatically Benchmarking LLM Code Agents through
  Agent-Driven Annotation and Evaluation* — arxiv 2510.24358:
  <https://arxiv.org/pdf/2510.24358>
- *JudgeAgent: Beyond Static Benchmarks for Knowledge-Driven
  and Dynamic LLM Evaluation* — arxiv 2509.02097:
  <https://arxiv.org/pdf/2509.02097>
- **Vals AI** (the Finance Agent benchmark referenced in the
  Finance entry) — official site at <https://vals.ai/>
