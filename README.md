# Startup factory

A team of agents that takes a niche or an idea and tells you whether it is worth your next 30 days. It runs inside Claude Code and inside Codex (OpenAI).

## Use it in Codex

Open this folder in Codex and trust the project, so Codex reads `.codex/config.toml`. Then type:

```
$factory invoicing for freelance designers in Brazil
```

Other commands:

```
$factory status
$factory resume 20261116_invoicing_designers
```

What is different from Claude Code:

* Questions to you are asked in the chat as numbered options. Reply with a number.
* The specialists ship inside the skill, in `skills/factory/specialists/`. The skill copies them to `.codex/agents/` on its first run. They are generated from `.claude/agents/`, so after you edit an agent, run `python3 scripts/sync_codex.py`.
* The research needs live web pages. `.codex/config.toml` sets `web_search = "live"`.
* Codex does not limit where an agent writes. The agent files tell each one to write only inside its run folder, but nothing enforces it. Read the diff of a run before you commit it.
* Models are inherited from your session. The judge and the critic keep high reasoning effort. To give them a stronger model than the rest, fill `CODEX_MODELS` in `scripts/sync_codex.py` with ids from your Codex model list, then run the script.

## Use it in Claude Code

Open this folder in Claude Code and type:

```
/factory invoicing for freelance designers in Brazil
```

Other commands:

```
/factory status
/factory resume 20261116_invoicing_designers
```

The first time you open the folder, Claude Code asks you to trust it. The file `.claude/settings.json` lets the agents search the web, fetch pages and write inside `runs/` without asking each time. Remove a line there if you prefer to approve those yourself.

## What happens

1. Intake. The factory writes a brief and asks you what is missing.
2. Research. Three agents work at the same time. The scout maps the market and runs search trend analysis, the miner reads competitor reviews and forums for real complaints, and the analyst sizes the market and estimates competitor revenue from their traffic.
3. Verdict. The judge checks the evidence, scores the candidate problems and says GO, PIVOT or NO GO.
4. Gate 1. You decide: proceed, pick another candidate, dig deeper or stop.
5. Plan. The strategist writes a 30 day validation and campaign plan with thresholds, a calendar and copy you can paste.
6. Review. The critic stress tests the plan and sends it back for up to two revisions.
7. Gate 2. You approve the plan, ask for changes or stop.
8. Build plan. The architect picks existing tools for the validation and for the first version of the product.

The factory also stops to ask you whenever an agent is unsure about direction. It never contacts anyone, publishes anything or spends money.

## Where things are

```
founder.md                      who you are, your hours and budget
runs/<date>_<slug>/             one folder per idea
  brief.md
  research/market.md
  research/pains.md
  research/viability.md
  verdict.md
  plan.md
  review.md
  build.md
  summary.md
  decisions.md                  every answer you gave
  state.json                    where the run stopped
AGENTS.md                       shared rules, read by Codex and by Claude Code
CLAUDE.md                       imports AGENTS.md
.claude/skills/factory/SKILL.md the orchestrator in Claude Code
.claude/agents/                 the seven specialists in Claude Code (source of truth)
skills/factory/SKILL.md          the orchestrator in Codex (also linked at .agents/skills/factory)
skills/factory/specialists/           the seven specialists in Codex (generated)
.codex/config.toml              Codex settings for this project
scripts/sync_codex.py           makes the Codex specialists from .claude/agents
```

## Models

The agents do not all run on the biggest model. The researchers, the strategist and the architect run on Sonnet. The judge and the critic run on Opus, because a wrong verdict or a weak review costs you a month. The table in `CLAUDE.md` explains each choice, and the `model` line at the top of each file in `.claude/agents/` is where you change it.

The orchestrator asks for Sonnet when you type `/factory`. That setting covers the turn that starts the run, so start the session on Sonnet (`/model sonnet`) if you want the whole run to stay there.

## Several ideas at once

Each run has its own folder, so you can open one Claude Code session per idea and let them run side by side.

## Before the first run

Fill in `founder.md`. The hours per week and the budget shape the whole plan. If you leave them blank the factory asks you once and saves the answer.
