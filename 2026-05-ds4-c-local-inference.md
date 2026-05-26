# 2026-05 — `ds4.c`: local DeepSeek V4 inference (and what it means for CC)

## Why this matters

[`antirez/ds4`](https://github.com/antirez/ds4) — a single-file C
inference engine that runs **DeepSeek V4 Flash** locally on Apple
Metal, written by Salvatore Sanfilippo (creator of Redis). It's
worth an entry in this notes set for one specific reason:
**ds4.c exposes an OpenAI/Anthropic-compatible HTTP API**, and
Claude Code can point at that local endpoint instead of Anthropic's.

That's the load-bearing piece. You can run Claude Code in 2026
backed by a frontier-grade open-weight model running on your own
hardware. The implications take the rest of this entry.

Adjacent context worth carrying:

- **DeepSeek V4** was released on 2026-04-24
  ([API release note](https://api-docs.deepseek.com/news/news260424),
  [Fortune coverage](https://fortune.com/2026/04/24/deepseek-v4-ai-model-price-performance-china-open-source/)).
  Two variants: **V4-Pro** (1.6T parameters, 1M token context,
  ~81% SWE-bench) and **V4-Flash** (smaller, cheaper, the local-
  runnable variant). [LMSYS day-0 writeup](https://www.lmsys.org/blog/2026-04-25-deepseek-v4/)
  covers the day-0 SGLang integration and verified RL.
- **ds4.c** itself landed shortly after — published in early May
  2026, alpha-stage, Metal-only at the time of writing (CUDA
  planned). [Knightli.com walkthrough](https://knightli.com/en/2026/05/11/deepseek-v4-flash-ds4-metal/)
  documents the 2026-05-11 local-Mac run.

## What ds4.c is

A deliberately narrow C codebase: not a generic GGUF runner, not a
framework, not a wrapper around another runtime. *One model,
implemented well.* The design choice is the headline.

| Property | Value |
|---|---|
| Language | C, single file (in the llama2.c lineage) |
| Target | DeepSeek V4 Flash, Apple Metal first; CUDA planned |
| Quantisation | 2-bit, tuned for V4-Flash; fits 128 GB Mac RAM |
| KV cache | Disk-backed, **persists session state across restarts** |
| Context window | 1M tokens (matches V4-Flash's native window) |
| Server surface | OpenAI- *and* Anthropic-compatible HTTP API |
| Integrations called out in the README | Claude Code, OpenCode, Pi |
| License | Open source (see repo); no GGML dependency |

The "narrow on purpose" framing per the [Flowtivity walkthrough](https://flowtivity.ai/blog/deepseek-v4-flash-ds4-local-inference-128gb-mac/)
and [Hackobar item](https://hackobar.com/item/github-antirez-ds4):
by constraining to one model architecture, ds4.c implements V4-
Flash-specific optimisations (the hybrid CSA + HCA attention; the
Metal graph executor tuned to the actual layer shapes) that general
engines like llama.cpp's GGUF runner can't match. The README
explicitly credits llama.cpp's path-opening role; ds4.c is what
becomes possible *after* GGUF normalised the territory but *without*
inheriting the generic-runner constraint.

## The Claude Code connection

The HTTP server speaks OpenAI / Anthropic message-API shapes. That
means:

```sh
# Point Claude Code at a local ds4 instance
export ANTHROPIC_BASE_URL=http://localhost:8080
claude
```

(Exact env var name + port differ by ds4 release; the principle
is what matters.) Claude Code's CLI doesn't care that the "Claude"
on the other end of the wire is actually DeepSeek V4 Flash running
on your Mac. Tool use, hook events, the harness — all unchanged.
Only the inference backend swaps.

This shape of integration is what the
[Hermes Agent entry](./2026-04-hermes-agent.md) gestures at when
it talks about "model flexibility" — except Hermes does it at the
orchestrator level (Hermes itself decides which model to route a
given task to). ds4 does it one layer further down: the wire
protocol stays Claude-shaped; the bytes on the other end are
something else.

For Claude Code specifically, the practical reasons to consider
the local-ds4 path:

1. **Offline / sovereign workflows.** Air-gapped environments,
   privacy-sensitive code, or regulated industries where the code
   you're touching can't leave the laptop.
2. **Cost.** $0.30/MTok pricing for V4-Pro via DeepSeek's own API
   is already attractive; *zero* per-token cost via local
   inference is a different curve entirely.
3. **Latency reliability.** Local inference has no network
   variance, no rate limits, no regional outages.
4. **Specific-model experiments.** Want to compare CC's behaviour
   on V4-Flash vs. Claude Opus 4.x? Same harness, different model.
   This is the cleanest way to do that comparison.

## Why the integration matters more than the engine

Worth being clear about what's load-bearing here: ds4.c is a
genuinely impressive engineering artefact (antirez's reputation
precedes it; 2-bit quantisation fitting V4-Flash on 128 GB is
non-trivial), but for a Claude Code notes set the engine itself
isn't the interesting bit. **The interesting bit is the
wire-compatibility move.**

The same move underlies a quietly important 2026 architectural
fact: **the OpenAI / Anthropic message-API shapes have become
the de facto cross-vendor standards for serving LLMs.** vLLM
speaks them. SGLang speaks them. Ollama speaks them. ds4.c
speaks them. Local inference of any model becomes drop-in
replaceable behind tools that were built against Anthropic-shaped
APIs.

For the harness-engineering thesis (see the
[harness engineering entry](./2026-harness-engineering.md)):
this means the *backend* layer of the agent stack is increasingly
hot-swappable while the *harness* stays put. The harness is where
the work is; the model behind it can come from anywhere that
speaks the right wire shape.

## Try this

If you have a 128 GB Mac and want to feel the swap firsthand:

1. Download DeepSeek V4 Flash from
   [Hugging Face](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash).
   This is the open-weight artefact ds4.c loads.
2. Clone [`antirez/ds4`](https://github.com/antirez/ds4) and build.
   Follow the README for the current Metal build steps; the
   project is moving fast so don't rely on third-party
   walkthroughs for the *exact* invocation.
3. Start the server. Point `ANTHROPIC_BASE_URL` (or whatever the
   current CC env-var name is for backend override) at the local
   port.
4. Run a CC session. Notice what's the same (every hook fires the
   same; CLAUDE.md loads the same; plan mode dispatches the same)
   and what's different (the model's actual code-writing style is
   noticeably distinct from Anthropic's).

The exercise isn't about adopting ds4 as a daily driver. It's
about *feeling* that the harness and the model are separable
concerns. Once you've seen CC running on a non-Anthropic backend,
the "Claude Code is just the harness; the model is replaceable"
framing stops being abstract.

## Sources

- [`antirez/ds4`](https://github.com/antirez/ds4) — canonical repo,
  README is the source of truth on what's implemented today
- [`bhardwajRahul/ds4-inference-engine`](https://github.com/bhardwajRahul/ds4-inference-engine)
  — community fork covering current build instructions
- *DeepSeek V4 on Day 0: From Fast Inference to Verified RL with
  SGLang* — LMSYS Blog (2026-04-25):
  <https://www.lmsys.org/blog/2026-04-25-deepseek-v4/>
- [DeepSeek-V4-Flash on Hugging Face](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash)
- [DeepSeek-V4-Pro on Hugging Face](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro)
- [DeepSeek V4 Preview release notes](https://api-docs.deepseek.com/news/news260424)
  (2026-04-24)
- *DeepSeek unveils V4 model* — Fortune (2026-04-24):
  <https://fortune.com/2026/04/24/deepseek-v4-ai-model-price-performance-china-open-source/>
- *DeepSeek V4 (2026): 1T Parameters, 81% SWE-bench, $0.30/MTok* —
  NxCode: <https://www.nxcode.io/resources/news/deepseek-v4-release-specs-benchmarks-2026>
- *How to Run DeepSeek V4 Flash Locally: ds4 Engine Runs Frontier
  AI on Your Laptop* — Flowtivity:
  <https://flowtivity.ai/blog/deepseek-v4-flash-ds4-local-inference-128gb-mac/>
- *Running DeepSeek 4 Locally: Antirez's ds4 Experiment on Apple
  Silicon Mac* — Knightli (2026-05-11):
  <https://knightli.com/en/2026/05/11/deepseek-v4-flash-ds4-metal/>
- *DwarfStar4 (DS4) Roadmap by antirez* —
  <https://pasqualepillitteri.it/en/news/2253/ds4-antirez-deepseek-v4-flash-inference-engine>
- *ds4.c: Native inference engine for DeepSeek V4 Flash* —
  Hackobar: <https://hackobar.com/item/github-antirez-ds4>
