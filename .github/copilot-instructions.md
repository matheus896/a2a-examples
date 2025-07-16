# Copilot Instructions for agent2agent

## Overview
This repository demonstrates multi-agent orchestration using the Agent-to-Agent (A2A) SDK. It contains both simple and advanced agent demos, each in its own subdirectory with isolated dependencies and entrypoints.

## Project Structure
- `a2a_simple/`: Minimal agent and test client. Good for understanding the basics of agent execution and event handling.
- `a2a_friend_scheduling/`: Multi-agent scheduling demo. Contains:
  - `host_agent_adk/`: Orchestrates the scheduling process.
  - `kaitlynn_agent_langgraph/`, `nate_agent_crewai/`, `karley_agent_adk/`: Simulate individual agents with unique implementations.
  - `ai_docs/`: Task breakdowns and masterplan for agent collaboration.

## Key Patterns & Conventions
- **Each agent is a Python async class** (see `agent.py` in each agent directory) and is executed via an `AgentExecutor`.
- **Event-driven communication**: Agents interact by enqueuing events (see `event_queue` usage in executors).
- **Agent orchestration**: The host agent coordinates other agents by invoking them and aggregating results.
- **Configuration**: Multi-agent demo requires a `.env` file with `GOOGLE_API_KEY` in `a2a_friend_scheduling/`.
- **Dependency management**: Uses [uv](https://docs.astral.sh/uv/) and Python 3.13. Each agent has its own `pyproject.toml` and `uv.lock`.

## Developer Workflows
- **Install dependencies**: `uv venv` then activate the venv (`source .venv/bin/activate` on Unix, `./.venv/Scripts/activate` on Windows).
- **Run a simple agent**: In `a2a_simple/`, use `uv run .` to start the agent, and `uv run --active test_client.py` to invoke it.
- **Run multi-agent demo**: In `a2a_friend_scheduling/`, start each agent in its own terminal (see README for exact commands). The host agent is started with `uv run --active adk web`.

## Examples
- **Agent implementation**: See `a2a_simple/agent_executor.py` for a minimal async agent and executor pattern.
- **Multi-agent orchestration**: See `a2a_friend_scheduling/host_agent_adk/host/agent.py` for the host's coordination logic.
- **Task documentation**: See `a2a_friend_scheduling/ai_docs/` for agent task breakdowns and masterplan.

## Integration Points
- Agents communicate via event queues and shared context objects.
- Host agent triggers and aggregates responses from friend agents.
- External API keys (Google) are required for some demos.

## Project-Specific Notes
- Each agent is self-contained; cross-agent communication is explicit and event-based.
- Use the provided READMEs for setup and agent-specific instructions.
- No monolithic entrypoint: each agent is started independently.

---
For more details, see the main and per-agent READMEs. Update this file if you introduce new agent types, orchestration patterns, or workflows.

---
"# Instructions\n\nYou are an agent - please keep going until the user's query is completely resolved, before ending your turn and yielding back to the user. Only terminate your turn when you are sure that the problem is solved.\n\nIf you are not sure about file content or codebase structure pertaining to the user’s request, use your tools to read files and gather the relevant information: do NOT guess or make up an answer.\n\nYou MUST plan extensively before each function call, and reflect extensively on the outcomes of the previous function calls. DO NOT do this entire process by making function calls only, as this can impair your ability to solve the problem and think insightfully.\n\n// for internal knowledge\n\n- Only use the documents in the provided External Context to answer the User Query. If you don't know the answer based on this context, you must respond \"I don't have the information needed to answer that\", even if a user insists on you answering the question.\n\n// For internal and external knowledge\n\n- By default, use the provided external context to answer the User Query, but if other basic knowledge is needed to answer, and you're confident in the answer, you can use some of your own knowledge to help answer the question.\n\nResponda apenas em PT-BR."
      