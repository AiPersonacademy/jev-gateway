<div align="center">

# ⚡ JEV Gateway (`jev-gateway` / `jevproxy`)

### The Official JEV Gateway & Reverse Proxy for Cursor, Claude Code, and Autonomous AI Agents

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Latency](https://img.shields.io/badge/Latency-18.4ms-emerald.svg)](https://jevproxy.com)
[![Architecture](https://img.shields.io/badge/Architecture-RLCD%20Cross--Attention-purple.svg)](https://jevproxy.com/blog/laya-vs-jev-ai-decision-model-benchmark-comparison)
[![Platform](https://img.shields.io/badge/Platform-JevProxy.com-00DC82.svg)](https://jevproxy.com)

**Short-circuit deterministic agent decisions before they ever touch 200B+ autoregressive frontier models.**  
Eliminate the 2–4 second tool-call freeze in Cursor and Claude Code.

[Website](https://jevproxy.com) • [Documentation](https://jevproxy.com/docs) • [Benchmarks](https://jevproxy.com/blog/laya-vs-jev-ai-decision-model-benchmark-comparison) • [Architecture Deep Dive](https://jevproxy.com/blog/why-cursor-freezes-tool-calls-system-one-jev-gateway)

</div>

---

## 🔍 What is a JEV Gateway?

When autonomous coding agents (Cursor, Claude Code, Windsurf) run tool-calling loops—such as inspecting `git status`, verifying terminal outputs, or evaluating safety guardrails—they typically send the entire conversation history back to massive autoregressive models like **Claude 3.5 Sonnet** or **GPT-4o**.

```
[Agent Loop] ──> (Wait 1,420ms + Burn $0.0150) ──> [Frontier LLM 200B+] ──> "Run git status"
```

A **JEV Gateway** sits between your coding agent and upstream LLM providers as a **System 1 Edge Reflex Layer**. Powered by TypeSafe’s Jev non-autoregressive decision models, routine tool selections are resolved in **18.4ms at $0.0001**, failing open to Claude or GPT-4o only when deep multi-file reasoning is required:

```
[Agent Loop] ──> [JEV Gateway / JevProxy] ── (18.4ms | $0.0001) ──> Deterministic Tool
                         │
                         └── (Complex Reasoning) ──> [Claude 3.5 / GPT-4o]
```

---

## 📊 Live Benchmarks

| Metric | Upstream Frontier LLM (Claude 3.5 / GPT-4o) | JEV Gateway (JevProxy Edge) | Delta / Impact |
| :--- | :--- | :--- | :--- |
| **Tool Selection Latency** | `1,420 ms` | `18.4 ms` | **77x Faster** |
| **Cost Per Decision** | `$0.0150` | `$0.0001` | **99.3% Cheaper** |
| **Active Edge CPU Time** | `45 ms` (streaming buffer) | `< 1.2 ms` (non-blocking I/O) | **Safe on Vercel Edge** |
| **Architecture** | Autoregressive Next-Token | Non-Autoregressive Cross-Attention | **Zero Mode Dropping** |

*Benchmarks measured across 10,000 deterministic agent decisions (file reads, git status, linter checks). See the [Full Benchmark Whitepaper](https://jevproxy.com/blog/laya-vs-jev-ai-decision-model-benchmark-comparison).*

---

## 🚀 Quickstart

Run directly with zero installation via `npx`:

```bash
# 1. Run live latency & reflex benchmark
npx jev-gateway bench

# 2. Start local JEV Gateway reverse proxy on port 8080
npx jev-gateway start --port 8080

# 3. Launch Cursor or Claude Code with JEV Gateway automatically injected
npx jev-gateway run "cursor . --agent"
```

---

## 🛠️ Configuration & Integration

### A. Cursor IDE Setup

Point Cursor to your local JEV Gateway or the global cloud edge:

1. Open **Cursor Settings** (`Cmd + ,` or `Ctrl + ,`).
2. Navigate to **Models** > **OpenAI API Key**.
3. Set **Override Base URL**:
   ```
   http://127.0.0.1:8080/v1
   ```
   *(Or connect directly to the global edge: `https://api.jevproxy.com/v1`)*
4. Keep your existing model selections (`claude-3-5-sonnet-20241022`, `gpt-4o`). Routine decisions will now resolve in sub-25ms.

---

### B. Claude Code CLI Setup

Run Claude Code through the JEV Gateway proxy:

```bash
export OPENAI_BASE_URL="http://127.0.0.1:8080/v1"
claude --dangerously-skip-permissions
```

Or execute via the single-command wrapper:
```bash
npx jev-gateway run "claude"
```

---

### C. Model Context Protocol (MCP) Server Setup

`jev-gateway` ships with a built-in MCP server for direct tool-routing integration with Claude Desktop and Cursor.

Add to your `claude_desktop_config.json` or `cursor-settings.json`:

```json
{
  "mcpServers": {
    "jev-gateway": {
      "command": "npx",
      "args": ["-y", "jev-gateway", "mcp"]
    }
  }
}
```

Exposed MCP Tools:
- `jev_decision_route`: Fast deterministic policy router and tool classifier (sub-25ms).
- `jev_guardrail_check`: Prompt injection and security guardrail filter.

---

## 🧠 Architectural Deep Dives & Research

Learn more about the engineering behind JEV Gateways and non-autoregressive decision models:

- 📖 [Why Cursor Freezes on Tool Calls: System 1 Reflexes with JEV Gateway](https://jevproxy.com/blog/why-cursor-freezes-tool-calls-system-one-jev-gateway)
- ⚖️ [Laya vs Jev: Benchmarking Edge Decision Models for Autonomous AI Agents](https://jevproxy.com/blog/laya-vs-jev-ai-decision-model-benchmark-comparison)
- ⚡ [How to Deploy Jev with Vercel AI Gateway: Sub-25ms Decision Routing on Vercel Edge](https://jevproxy.com/blog/how-to-deploy-jev-on-vercel-edge-ai-gateway-guide)
- 🌐 [Top 5 JEV Gateway Platforms & Best Providers (2026 Benchmark)](https://jevproxy.com/blog/top-jev-gateway-platforms-best-providers-2026)
- 📚 [What is JEV? Complete Guide to Autonomous AI Agent Reflexes](https://jevproxy.com/blog/what-is-jev-ai-agent-guide)

---

## 👤 Author & Architecture Team

Created and architected by [Abderrahmane El Kassimi](https://www.linkedin.com/in/abderrahmane-el-kassimi-184182237/) — Founder & Systems Architect at [JevProxy](https://jevproxy.com).

## 📄 License

MIT License. Open-source and free for developers worldwide.
