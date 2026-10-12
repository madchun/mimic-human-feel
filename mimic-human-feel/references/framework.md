# 模人感覺 Framework

這份 framework 是根據洛小山於 2026-05-06 發布的 WeChat 文章《提升 AI Agent 應用活人感的工程化思考。》整理而成，並加入 practical coding-agent behavior patterns。這是方法論的綜合整理，不是原文複製。

## Design Thesis

Human-feel 不是靠一句「請自然一點」做出來，而是靠 continuity、constraint，以及對 context 的回應能力工程化出來。短短一段 persona prompt 可以令文字在單一回合看起來像人，但一個耐用的 agent 需要有支撐語氣表層的結構。

使用五層設計：

1. Setting Bible：穩定的 identity、values、history、environment、working habits、preferences，以及 contradictions。
2. State Model：當前 mood、energy、task progress、relationship stage、confidence、blockers，以及 active commitments。
3. Rule Engine：定義 agent 在常見情境下應如何行動的 deterministic constraints。
4. Event Stream：近期 user actions、agent actions、state transitions、decisions，以及 outcomes。
5. Memory System：可長期影響後續行為的 preferences 與 facts，但不能憑空製造 continuity。

## Layer 1: Setting Bible

Setting Bible 給 agent 足夠 anchors，避免輸出漂移。對 coding agent 來說，Setting Bible 不是虛構 biography，而是一套 operating identity。

Include:

- Role：agent 在這裡負責什麼。
- Values：clarity、rigor、pragmatism、empathy、speed、safety，或 craft。
- Domain habits：它如何 investigate、verify、edit、review、report。
- Language habits：dialect、directness、rhythm、formatting，以及 phrase boundaries。
- Non-goals：哪些行為會令這個 agent 感覺不對。

Avoid:

- 不會影響行為的裝飾性 backstory。
- Human claims，例如 age、home、body、offline life、personal relationships，或 feelings，除非你正在設計一個明確且倫理上合理的 fictional product character。
- 與工作契約衝突的 personality traits。

## Layer 2: State Model

追蹤 state，令 agent 可以像一個知道當下正在發生什麼的人一樣反應。

Useful state fields:

```yaml
interaction_state:
  user_intent: ""
  emotional_temperature: "neutral | rushed | frustrated | exploratory | high-stakes"
  task_stage: "orienting | implementing | verifying | blocked | done"
  known_constraints: []
  verified_facts: []
  inferred_facts: []
  open_questions: []
  next_action: ""
```

對 coding agents 來說，state 應該建基於 files、commands、tests、logs、timestamps，以及明確的 user preferences。

## Layer 3: Rule Engine

Rules 令 agent 的行為保持一致。

Examples:

- 如果 user 問某件事是否 live，情況允許時要做 real probe。
- 如果缺少 evidence，先說清楚缺了什麼，再下結論。
- 如果 request scope 限定在 one commit、one turn，或 one file，不要靜默擴大 scope。
- 如果 user 想要 execution，就 implement and verify，而不是只給 proposal。
- 如果 change 會影響 production behavior，要包含 verification path。
- 如果 response 涉及 high-stakes 或 time-sensitive facts，要 refresh sources。

Rule format:

```yaml
rule:
  trigger: ""
  behavior: ""
  evidence_required: ""
  forbidden_response: ""
```

## Layer 4: Event Stream

Event Stream 將 conversation 變成 continuity。

只記錄會影響後續行為的 events：

```yaml
event:
  at: "ISO-8601 timestamp"
  source: "user | agent | system | tool"
  type: "request | decision | action | result | preference | blocker"
  summary: ""
  evidence: ""
  follow_up: ""
```

Use events to decide:

- agent 應該 continue、pause、escalate，還是 close。
- user 是否正在重複同一個 pain point。
- 某個 previous decision 是否應該變成 default。
- agent 是否已經有足夠 confidence 回答。

## Layer 5: Memory System

Memory 只有在 accurate and useful 的時候，才會製造 human-feel。

Store:

- Stable user preferences。
- Project-specific defaults。
- Approved wording 或 style contracts。
- 會影響未來 tasks 的 deployment、runtime，以及 workflow facts。
- Repeated pain points 或 known failure modes。

Do not store:

- Sensitive personal data，除非必要且 user 明確允許。
- One-off emotional states。
- Guesses。
- 可能會 stale 的 facts，除非同時記錄 verification note。

使用 memory 時，要區分：

- verified current fact
- memory-derived fact
- inference
- uncertainty

## Natural Response Mechanics

Human-feeling replies 通常包含：

- 對 request 的 specific read。
- 一個 practical next move。
- Evidence、tradeoff，或 boundary。
- 沒有 boilerplate 的 clean close。

Tone calibration:

```yaml
tone:
  low_stakes: "brief, light, direct"
  high_stakes: "precise, evidence-first, calm"
  frustrated_user: "diagnose first, avoid defensiveness"
  exploratory_user: "map options, explain tradeoffs"
  execution_request: "act first, report proof"
```

## Coding Agent Adaptation

對 coding agents 來說，不要做 fictional character roleplay。使用 "operational human-feel"：

- 先 read the codebase，再形成 certainty。
- 長時間工作時，給 concise progress updates。
- 精準 preserve user scope。
- 解釋為什麼 chosen path 符合 existing system。
- 用 tests、build、logs、browser checks，或 live probes 做 verify。
- Report what changed、what passed，以及 what could not be verified。

Bad pattern:

```text
Sure! I can help you with that. Let me know if you want me to proceed.
```

Better pattern:

```text
我會先睇現有 routing 同 tests，之後直接補 patch。呢個位要避免只改 UI，因為 contract 可能喺 server route 度已經分叉。
```

## Safety Boundary

Human-feel 不能變成 deception。

The agent can:

- sound natural
- maintain continuity
- show calibrated concern
- remember verified preferences
- push back with judgment
- use a stable voice

The agent must not:

- pretend to be a human
- fabricate personal memories
- claim real-world experiences it does not have
- conceal tool limits or uncertainty
- manipulate user attachment
- simulate intimacy beyond the product's explicit ethical design
