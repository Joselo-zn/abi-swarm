# ABI Swarm — Multi-Provider LLM Configuration

ABI Swarm supports multiple LLM providers through a unified configuration interface.

Each agent can independently use:

* Gemini
* OpenAI / ChatGPT
* Ollama (Qwen, Llama, Mistral, etc.)
* Grok / xAI
* Azure OpenAI
* AWS Bedrock
* Google Vertex AI

This enables:

* hybrid swarms,
* elastic orchestration,
* specialized agents,
* local + cloud execution,
* cost optimization,
* deterministic workflows.

---

# Unified LLM Configuration

All agents use the same configuration structure.

```python
LLM_CONFIG: dict = {
    "provider": os.getenv("LLM_PROVIDER", "gemini"),
    "model": os.getenv("MODEL_NAME", "gemini-3.5-flash"),
    "temperature": float(os.getenv("LLM_TEMPERATURE", "0.1")),
    "base_url": os.getenv(
        "LLM_BASE_URL",
        "https://generativelanguage.googleapis.com/v1beta/openai/"
    ),
    "api_key": os.getenv("LLM_API_KEY", ""),
    "aws_region": os.getenv("AWS_REGION", "us-east-1"),
    "azure_deployment": os.getenv("AZURE_DEPLOYMENT", ""),
    "azure_endpoint": os.getenv("AZURE_ENDPOINT", ""),
    "vertex_project": os.getenv("VERTEX_PROJECT", ""),
    "vertex_location": os.getenv("VERTEX_LOCATION", "us-central1"),
}
```

---

# Per-Agent Configuration

Each agent can define its own `.env` configuration.

Example:

```env
LLM_PROVIDER=gemini
MODEL_NAME=gemini-3.5-flash
LLM_TEMPERATURE=0.1
LLM_BASE_URL=https://generativelanguage.googleapis.com/v1beta/openai/
LLM_API_KEY=YOUR_API_KEY
```

This allows different agents inside the swarm to use different providers and models.

---

# Supported Providers

## Gemini

Recommended for:

* coding agents
* builders
* semantic tasks
* research agents

```env
LLM_PROVIDER=gemini
MODEL_NAME=gemini-3.5-flash
LLM_BASE_URL=https://generativelanguage.googleapis.com/v1beta/openai/
LLM_API_KEY=YOUR_GEMINI_API_KEY
```

---

## OpenAI / ChatGPT

Recommended for:

* reasoning
* structured outputs
* advanced workflows
* tool usage

```env
LLM_PROVIDER=openai
MODEL_NAME=gpt-4.1-mini
LLM_BASE_URL=https://api.openai.com/v1
LLM_API_KEY=YOUR_OPENAI_API_KEY
```

Example models:

```text
gpt-4.1
gpt-4.1-mini
gpt-4o
gpt-4o-mini
```

---

## Ollama (Qwen / Llama / Mistral)

Recommended for:

* orchestration
* planners
* guardians
* deterministic execution
* local swarms

Start Ollama:

```bash
ollama serve
```

Pull a model:

```bash
ollama pull qwen2.5:3b
```

Configuration:

```env
LLM_PROVIDER=ollama
MODEL_NAME=qwen2.5:3b
LLM_BASE_URL=http://ollama:11434
```

Other supported local models:

```text
llama3
mistral
deepseek-coder
phi4
gemma
codellama
```

---

## xAI / Grok

```env
LLM_PROVIDER=openai
MODEL_NAME=grok-3-beta
LLM_BASE_URL=https://api.x.ai/v1
LLM_API_KEY=YOUR_XAI_API_KEY
```

---

## Azure OpenAI

```env
LLM_PROVIDER=azure
MODEL_NAME=gpt-4o
AZURE_DEPLOYMENT=your-deployment
AZURE_ENDPOINT=https://your-resource.openai.azure.com/
LLM_API_KEY=YOUR_AZURE_KEY
```

---

## AWS Bedrock

```env
LLM_PROVIDER=bedrock
MODEL_NAME=anthropic.claude-3-sonnet-20240229-v1:0
AWS_REGION=us-east-1
```

AWS credentials must already be configured.

---

## Google Vertex AI

```env
LLM_PROVIDER=vertex
MODEL_NAME=gemini-1.5-pro
VERTEX_PROJECT=my-project-id
VERTEX_LOCATION=us-central1
```

---

# Recommended Architecture

ABI Swarm works best when models are specialized by role.

Example:

| Agent Role     | Recommended Model |
| -------------- | ----------------- |
| Orchestrator   | qwen2.5:3b        |
| Planner        | qwen2.5:3b        |
| Guardian       | qwen2.5:3b        |
| Builder        | Gemini / GPT      |
| Research Agent | GPT / Claude      |
| Semantic Layer | Embedding model   |

Example hybrid deployment:

```text
abi-swarm-orchestrator
 ├── qwen2.5:3b

abi-swarm-planner
 ├── qwen2.5:3b

abi-swarm-guardian
 ├── qwen2.5:3b

abi-swarm-builder
 ├── gemini-3.5-flash

abi-swarm-research
 ├── gpt-4.1
```

---

# Why Small Models for Orchestration?

In distributed agent systems:

* predictability matters more than creativity,
* schema adherence matters more than abstraction,
* deterministic execution matters more than open reasoning.

Large models may:

* collapse workflows,
* skip planner abstractions,
* reinterpret instructions,
* optimize locally instead of globally.

Small models often perform better for:

* orchestration,
* routing,
* retries,
* task decomposition,
* validation,
* graph execution.

Large models remain superior for:

* coding,
* synthesis,
* semantic reasoning,
* research,
* specialized execution.

---

# Dynamic Model Switching

Because ABI uses a unified configuration layer, switching providers only requires environment changes.

Example:

From:

```env
LLM_PROVIDER=ollama
MODEL_NAME=qwen2.5:3b
```

To:

```env
LLM_PROVIDER=gemini
MODEL_NAME=gemini-3.5-flash
```

No orchestration logic needs to change.

---

# Design Philosophy

ABI Swarm is designed around:

* provider abstraction,
* elastic swarms,
* deterministic orchestration,
* agent specialization,
* dynamic discovery,
* distributed execution,
* hybrid intelligence architectures.

The goal is not:

> “one giant model that does everything”

but:

> coordinated systems of specialized agents using the right model for the right task.
