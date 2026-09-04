# llm_workflow_skills

## Installation
Depending on your harness, will determine where the agent skills should live. For pi, link (or copy) the skill into `~/.pi/agent/skills/plan-to-queue.md`.
Then restart pi with either `/reload` or reboot pi.

## Intended workflow
Have an agent create a written plan for a specific feature or workflow, typically one that can handle a higher context. Next I use [Oliver Kriska Elixir Skills](https://github.com/oliver-kriska/claude-elixir-phoenix) specifically the /skill:phx-plan skill, to create a plan for the agent.
Then we can use the 'plan-to-queue' skill, this will generate a txt batch file. Which then we can load into
[Matheus Message Queue](https://github.com/MatheusBBarni/pi-extensions) with the `/queue import [batch.txt]`



```
$ pi
| [using large model] /skill:phx-research You are a security expert adding OTP MFA to this application. Consider rate-limiting, security considerations, backup codes, and a nice user experience, with end to end testing. Write your findings to a research document
| [using large model] /skill:phx-plan Create a plan for [researchdocument.md]
| [using large model] /skill:plan-to-queue [plan.md] -tv
| [using large model] /queue import [batch.txt]


Queue is auto loaded and default LLM is used
```

Verification is optional: append `-t` to also queue `/test_coverage`, `-v` to also queue
`/verify_complete`, or `-tv`/`-vt` for both (each runs in its own fresh session via an
injected `! /new`). Omit flags for a batch with the plan tasks only.
