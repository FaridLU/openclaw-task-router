# Changelog

## [1.0.0] - 2026-05-24

### Added
- Production-grade Task Router prompt (`AGENTS.md`)
- Full OpenClaw agent configuration (`openclaw.json`) with 6 specialists
- README with badges, installation guide, agent directory, customization tips
- MIT license
- Async delegation protocol with 2-minute heartbeat status updates
- Automatic model fallback chains for all 6 agents
- Anti-patterns section with NEVER/ALWAYS constraints
- Loop detection and recovery protocol
- Standing orders for persistent operating authority

### Models Used
- GLM-5.1 (plan — architecture & design)
- Kimi K2.6 (build, review — complex features & code review)
- DeepSeek V4 Pro (debug — production debugging)
- DeepSeek V4 Flash (fast — quick edits & search)
- MiMo V2 Omni (visual — UI analysis & screenshots)
- Qwen 3.5 Plus / 3.6 Plus (fallback tiers)

All models served via OpenCode (`opencode-go/`) provider.

### Authors
- Farid ([@FaridLU](https://github.com/FaridLU))
- Claw 🔀 — the OpenClaw Task Router agent