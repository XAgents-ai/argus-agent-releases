# First Run with ArgusAgent (`argus audit`) 🛡️👁️

Welcome to Agent Argus. This guide takes you from an unconfigured terminal to your first audit, explains how to read the output, and documents the environment variables for optional LLM passes.

---

## 1. Quick Installation

### Python Wheel & PyPI-style Install
```bash
# Install directly from the release wheel asset or repository tag
pip install "https://github.com/XAgents-ai/argus-agent-releases/releases/download/v0.1.0-beta/argus_agent-0.1.0-py3-none-any.whl"
```

### Standalone Executable (Zero-Python Install)
Download the zip package for your OS from [Releases](https://github.com/XAgents-ai/argus-agent-releases/releases/latest), extract it, and run `argus` / `argus.exe`.

---

## 2. Your First Audit

Run `argus audit .` from the root of any repository:

```bash
argus audit .
```

### Understanding the Verdict & Exit Codes

| Verdict | Exit Code | Meaning |
|---|---|---|
| `RELEASE_READY` | `0` | Clean code. No blocking defects, and sufficient coverage depth achieved. |
| `NOT_READY_FOR_RELEASE` | `1` | Blocking defect detected (e.g. secret leak, vacuous assertion, critical orphan code). |
| `INSUFFICIENT_COVERAGE` | `3` | Audit depth below required threshold — cannot award a pass. |

---

## 3. Setting Up an LLM Provider (Optional)

> **Important**: The default run (`argus audit .`) is **100% offline, pure-deterministic, requiring zero LLM tokens, zero API keys, and zero network access**.

LLM dispatch is **strictly opt-in** and is engaged only when you pass `--deep-audit`. Credentials are read **strictly from environment variables** (never from CLI flags or configuration files).

### 🔑 Environment Variables Reference

| Environment Variable | Description | Default if Unset |
|---|---|---|
| `OPENAI_BASE_URL` / `OLLAMA_HOST` / `OLLAMA_URL` | Provider endpoint URL (**Required switch for LLM dispatch**). *Omit trailing `/v1`*. | None — LLM pass degrades gracefully |
| `OPENAI_API_KEY` | Bearer token sent in `Authorization` header. | `"mock-key"` |
| `ARGUS_LLM_MODEL` / `OLLAMA_MODEL` | Model identifier (e.g., `gpt-4o-mini`, `llama3.1`, `claude-3-5-sonnet`, `deepseek-coder`). | `gpt-4o-mini` |

### 💡 Examples

#### OpenAI or OpenAI-Compatible APIs (DeepSeek, Groq, OpenRouter)
```bash
export OPENAI_BASE_URL=https://api.openai.com
export OPENAI_API_KEY=sk-...
export ARGUS_LLM_MODEL=gpt-4o-mini
argus audit . --deep-audit
```

#### Local Ollama (100% Private, Zero-Key)
```bash
export OLLAMA_HOST=http://localhost:11434
export ARGUS_LLM_MODEL=llama3.1
argus audit . --deep-audit
```

> **Endpoint Path Note**: Always set `OPENAI_BASE_URL` without a trailing `/v1`. The adapter automatically appends `/v1/chat/completions`.
