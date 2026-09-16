# Agent team for Mona's Project Pulse

I am using GitHub Copilot CLI in a Codespace to orchestrate the work for Mona's Project Pulse dashboard. The custom agent team is defined under the repository's agent folder at `.github/agents/` and includes the following specialists:

- Orchestrator — Target model: Claude Opus 4.7 (copilot). Responsibility: coordinates the overall build by breaking the work into phases, delegating tasks to specialist agents, and verifying the integrated result. Definition: `.github/agents/orchestrator.agent.md`.
- Planner — Target model: Claude Opus 4.7 (copilot). Responsibility: researches the repo, checks dependencies and edge cases, and produces a practical implementation plan with file ownership, sequencing, validation expectations, and open questions. Definition: `.github/agents/planner.agent.md`.
- Coder — Target model: GPT-5.5 (copilot). Responsibility: implements the assigned code changes within the orchestrated file scope, keeps the project structure predictable, and validates behavior before reporting completion. Definition: `.github/agents/coder.agent.md`.
- Designer — Target model: Gemini 3.1 Pro (copilot). Responsibility: focuses on UI/UX, accessibility, hierarchy, interaction flow, and polished dashboard styling for Project Pulse, including responsive visual design decisions. Definition: `.github/agents/designer.agent.md`.

This setup gives the project a clear division of labor: Planner scopes the work, Designer shapes the experience, Coder builds the implementation, and Orchestrator keeps the workflow coordinated and efficient.
