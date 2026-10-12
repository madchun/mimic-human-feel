# mimic-human-feel | 活人感 Skill

Make AI agent replies natural, specific, context-aware, and grounded in evidence. Help agents maintain a stable voice, use verified memory, explain uncertainty, and avoid generic filler or invented personal experience.

令 AI Agent 回應更自然、更貼近當下情境，保持一致嘅語氣同清楚嘅邊界。適合調整 coding agent、chatbot、assistant persona 同 system prompt。

## Install

Requires Node.js and npm for the following commands. The [skills CLI](https://github.com/vercel-labs/skills) supports Codex, Claude Code, Cursor, OpenCode, and other compatible agents.

Choose an agent interactively:

```bash
npx skills add madchun/mimic-human-feel --skill mimic-human-feel
```

Install globally for Codex:

```bash
npx skills add madchun/mimic-human-feel --skill mimic-human-feel --agent codex --global
```

Install globally for Claude Code:

```bash
npx skills add madchun/mimic-human-feel --skill mimic-human-feel --agent claude-code --global
```

Preview available skills without installing:

```bash
npx skills add madchun/mimic-human-feel --list
```

Restart your agent or start a new session if the skill does not appear.

## Use

In Codex:

```text
Use $mimic-human-feel to make this agent reply with natural, grounded, non-deceptive human-feel.
```

Example request:

```text
用 mimic-human-feel 幫我調整呢個 Agent 嘅 Instructions：用廣東話，直接、自然，記住已確認嘅偏好；唔好假裝有個人經歷，完成工作要有證據。
```

## Other AI Platforms

Platforms without an Agent Skills loader cannot install a GitHub repo directly. Use the body of [SKILL.md](mimic-human-feel/SKILL.md) as the agent's Instructions or system prompt, omitting the YAML frontmatter. For deeper persona and state design, include [framework.md](mimic-human-feel/references/framework.md) as supporting context.

Direct instruction file:

https://raw.githubusercontent.com/madchun/mimic-human-feel/main/mimic-human-feel/SKILL.md

This skill provides behavioral guidance. Persistent memory, tools, and live state tracking depend on the host platform; installing it does not add those capabilities.

## Files

```text
mimic-human-feel/
  SKILL.md
  agents/openai.yaml
  references/framework.md
```

## Attribution

The framework credits a WeChat article by 洛小山, published on 2026-05-06, titled《提升 AI Agent 應用活人感的工程化思考。》. It combines that methodology with practical coding-agent behavior patterns. The original article URL was not included in the local source material. This repository publishes the existing local skill; it is not the article author's original repository.
