# Self-Questioning — CCAR-F Exam Prep

Personal study repo for preparing for the **Claude AI Engineer (CCAR-F)** certification, built around a self-questioning method: instead of re-reading notes, each domain is turned into a list of probing questions (and practice scenarios) to actively test recall and understanding.

## Structure

```
.
├── Docs/                 # Domain reference notes (source material), .md + .pdf
├── Exam Guide/           # Self-questioning lists per domain — conceptual recall questions
├── Practice Exam/        # Scenario-based self-questioning — applied/practice judgment calls
├── Self-Questioning_Strategy.pptx   # Slide deck explaining the study method
└── code/                 # Hands-on course exercises and sample projects
```

### Docs/
Reference notes for each exam domain, written as narrative explanations with examples.

### Exam Guide/
One self-questioning list per domain — short, pointed questions ("What distinguishes X from Y?", "Why does Z matter architecturally?") used to test whether the concept has actually been internalized, not just read.

### Practice Exam/
Scenario-driven self-questioning: realistic situations paired with the key distinctions needed to reason through them (e.g. choosing between architectural patterns, spotting the flawed justification among several plausible-sounding ones).

## Domains covered

1. Agentic Architecture & Orchestration
2. Tool Design & MCP Integration
3. Claude Code Configuration & Workflows
4. Prompt Engineering & Structured Output
5. Context Management & Reliability

## code/

Course exercises and sample projects used for hands-on practice alongside the conceptual prep:

- `Claude-code-classroom` — lesson-by-lesson exercises (model selection, extended thinking, agentic architecture, Claude Code config, Agent SDK, custom tools, structured outputs, Claude Skills, MCP integration, multi-agent orchestration, TDD with AI, evaluating agents, computer use)
- `Claude-AI-Engineer-Bounded-Autonomy-Guardrails` — hub-and-spoke multi-agent system, deterministic hooks for agent compliance
- `Claude-AI-Engineer-Evaluation-and-Observability` — document/policy extraction pipelines, multi-source synthesis
- `Claude-AI-Engineer-Harness-Engineering` — long-conversation context strategy, `stop_reason`-driven agent loops, multi-shift monitoring
- `Claude-AI-MCP-in-Action` — inventory agent with MCP tools, scoped MCP governance

## How to use this repo

1. Read the domain notes in `Docs/`.
2. Without looking back at the notes, work through the matching list in `Exam Guide/` — answer each question from memory.
3. Work through the matching scenarios in `Practice Exam/` to pressure-test judgment on applied cases, not just definitions.
4. Use `code/` to validate understanding by building/running the actual patterns discussed (agent loops, tool design, hooks, evaluation harnesses, etc.).
