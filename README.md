<p align="center">
  <img src="https://img.shields.io/badge/OpenClaw-Task_Router-6E40C9?style=for-the-badge&labelColor=1a1a2e" alt="OpenClaw Task Router">
  <img src="https://img.shields.io/badge/Models-6_Specialists-00C9A7?style=for-the-badge&labelColor=1a1a2e" alt="6 Specialist Models">
  <img src="https://img.shields.io/badge/Fallback-Auto_Switch-FF6B6B?style=for-the-badge&labelColor=1a1a2e" alt="Auto Fallback">
</p>

<h1 align="center">🔀 OpenClaw Task Router</h1>

<p align="center">
  <strong>Production-grade multi-agent orchestration for OpenClaw</strong><br>
  Route tasks to the best AI model. Get real-time status. Never wait at a blank terminal.
</p>

<p align="center">
  <a href="#quick-start">Quick Start</a> · <a href="#agent-directory">Agents</a> · <a href="#how-it-works">How It Works</a> · <a href="#customization">Customize</a> · <a href="https://gist.github.com/FaridLU/58b99eb559c17c75e0ef8991338b0a15">Gist Mirror</a>
</p>

---

## What Is This?

A **single prompt + config** that turns your [OpenClaw](https://github.com/openclaw/openclaw) instance into an AI command center. Instead of one model doing everything poorly, 6 specialist agents handle what they're best at:

| You Say | Router Picks | Model | Why |
|---------|-------------|-------|-----|
| "Plan the architecture" | `plan` | GLM-5.1 | High reasoning for design decisions |
| "Build the payment module" | `build` | Kimi K2.6 | Strong multi-file code generation |
| "Debug the 500 error" | `debug` | DeepSeek V4 Pro | Deep code understanding, terminal work |
| "Review PR #42" | `review` | Kimi K2.6 | Thorough analysis with high thinking |
| "Convert this screenshot to code" | `visual` | MiMo V2 Omni | Vision-native model |
| "Find where sessions are handled" | `fast` | DeepSeek V4 Flash | Cheap, fast, good enough |

**All models served via [OpenCode](https://opencode.ai/go?ref=QCXVTDZ17F)** — one API, one subscription, no per-provider keys needed.

---

## ✨ Features

- **🧠 Smart Task Classification** — automatically routes to the right specialist
- **⚡ Async Delegation** — spawns agents and returns instantly; no waiting
- **📊 2-Minute Heartbeat** — live status trees showing what's running, blocked, or done
- **🔄 Automatic Fallback** — when a model hits its rate limit, switches to the next best
- **🛡️ Hallucination Prevention** — hard NEVER/ALWAYS constraints from production experience
- **🔁 Loop Detection** — catches stuck agents and intervenes
- **💰 Cost Optimization** — cheap models for simple tasks, powerful reasoning for complex ones

---

## Quick Start

### 1. Install OpenClaw

```bash
npm install -g openclaw
openclaw onboard
```

See the [OpenClaw docs](https://docs.openclaw.ai) for full setup.

### 2. Place AGENTS.md

```bash
# Clone this repo or download directly
mkdir -p ~/.openclaw/workspace
curl -o ~/.openclaw/workspace/AGENTS.md https://raw.githubusercontent.com/FaridLU/openclaw-task-router/main/AGENTS.md
```

> **Where does OpenClaw look for AGENTS.md?**
> OpenClaw loads `AGENTS.md` from the workspace directory at session start. Default: `~/.openclaw/workspace/`. Configure it in `openclaw.json`:
> ```json
> { "agents": { "defaults": { "workspace": "/path/to/your/workspace" } } }
> ```

### 3. Configure the Agents

Add the [openclaw.json](openclaw.json) from this repo to your OpenClaw config:

```bash
cp openclaw.json ~/.openclaw/openclaw.json
# Then edit to add your OpenCode API key and any personal preferences
```

Or manually add the 6 specialist agents to your existing config — see [openclaw.json](openclaw.json) for the full template.

### 4. Start Routing

Just talk to OpenClaw normally:

```
Plan a microservices architecture for our e-commerce app
```

```
Debug the 500 error in production
```

```
Review PR #42
```

The router announces the chosen agent and model, then returns instantly. Every 2 minutes you'll see:

```
Router
├── plan (Microservices architecture) → GLM-5.1 (thinking: high) [RUNNING]
└── fast (Search session handling) → DeepSeek V4 Flash (thinking: off) [FINISHED: Found in auth/views.py]
```

---

## Agent Directory

| Agent | Best For | Primary Model | Thinking | Fallback Chain |
|-------|-----------|--------------|----------|----------------|
| `plan` | Architecture, specs, design docs | GLM-5.1 | High | Kimi K2.6 |
| `build` | Features, refactors, multi-file | Kimi K2.6 | High | DeepSeek V4 Pro → Qwen 3.6 Plus |
| `debug` | Production errors, terminal work | DeepSeek V4 Pro | Medium | Kimi K2.6 → DeepSeek V4 Flash |
| `review` | Code review, security audits | Kimi K2.6 | High | DeepSeek V4 Pro |
| `visual` | Screenshots, mockups, UI analysis | MiMo V2 Omni | Off | Qwen 3.6 Plus |
| `fast` | Quick edits, search, tests, docs | DeepSeek V4 Flash | Off | Qwen 3.5 Plus |

All models via **OpenCode** (`opencode-go/`) provider — [opencode.ai](https://opencode.ai/go?ref=QCXVTDZ17F)

---

## How It Works

### Request Flow

```
User → Task Router → Classify → Spawn Specialist Agent → Return to User
                          ↓
              ┌─────────────────────────────────────┐
              │  plan → GLM-5.1 (high thinking)      │
              │  build → Kimi K2.6 (high thinking)    │
              │  debug → DeepSeek V4 Pro (medium)     │
              │  review → Kimi K2.6 (high thinking)   │
              │  visual → MiMo V2 Omni (no thinking)  │
              │  fast → DeepSeek V4 Flash (no thinking)│
              └─────────────────────────────────────┘
                          ↓
              Agent finishes → Result appears in session
```

### Fallback Chain

If the primary model is unavailable (rate limit, outage, etc.):

```
plan:   GLM-5.1 → Kimi K2.6
build:  Kimi K2.6 → DeepSeek V4 Pro → Qwen 3.6 Plus
debug:  DeepSeek V4 Pro → Kimi K2.6 → DeepSeek V4 Flash
review: Kimi K2.6 → DeepSeek V4 Pro
visual: MiMo V2 Omni → Qwen 3.6 Plus
fast:   DeepSeek V4 Flash → Qwen 3.5 Plus
```

You never see the switch. The router handles it and reports fallbacks in the next heartbeat.

---

## Customization

### Swap Models

Replace any model in `openclaw.json` with your preferred provider:

```json
{
  "id": "plan",
  "model": {
    "primary": "anthropic/claude-sonnet-4-20250514",
    "fallbacks": ["opencode-go/kimi-k2.6"]
  }
}
```

Works with any OpenRouter, OpenAI, Anthropic, or local model provider OpenClaw supports.

### Add New Agents

Define new specialists in `openclaw.json` and add routing rules to `AGENTS.md`:

```json
{
  "id": "security",
  "description": "Security audits and vulnerability scanning",
  "model": { "primary": "opencode-go/deepseek-v4-pro" }
}
```

### Adjust Thinking Levels

Change `high` → `medium` → `off` per agent to trade reasoning depth for speed/cost:

```json
{ "id": "fast", "thinking": "off" }   // cheapest, fastest
{ "id": "debug", "thinking": "medium" } // balanced
{ "id": "plan", "thinking": "high" }    // deepest reasoning
```

---

## Why It Works

| Problem | Solution |
|---------|----------|
| One model does everything poorly | 6 specialists, each best at their job |
| Expensive reasoning on simple tasks | Fast agent uses Flash (cheap), complex tasks use Pro |
| Waiting at a blank screen | Async delegation — instant confirmation |
| No visibility into agent status | 2-minute heartbeat trees |
| Model outages break everything | Automatic fallback chains |
| AI hallucinates nonexistent code | Hard NEVER constraints + mandatory search-before-write |

---

## Repository Structure

```
openclaw-task-router/
├── AGENTS.md              ← The router prompt (copy to ~/.openclaw/workspace/)
├── openclaw.json           ← Full agent configuration (copy to ~/.openclaw/)
├── README.md               ← This file
├── LICENSE                 ← MIT
├── CHANGELOG.md            ← Version history
└── examples/
    └── demo-session.md     ← Example router conversation
```

---

## Contributing

1. Fork this repo
2. Create your feature branch (`git checkout -b feature/my-improvement`)
3. Commit your changes
4. Push to the branch (`git push origin feature/my-improvement`)
5. Open a Pull Request

This is a living project. Every time the router makes a mistake, update the **Anti-Patterns** section in `AGENTS.md` to encode the lesson.

---

## Authors

- **Farid** ([@FaridLU](https://github.com/FaridLU)) — OpenClaw power user who wanted the ultimate AI orchestration experience
- **Claw** 🔀 — The OpenClaw Task Router agent that eats its own dogfood

Powered by [OpenCode](https://opencode.ai/go?ref=QCXVTDZ17F) models — one API, six specialists, zero per-provider keys.

---

## License

[MIT](LICENSE) — use it, fork it, share it.