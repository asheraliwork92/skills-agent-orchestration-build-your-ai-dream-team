# Agent team

For Mona's Project Pulse dashboard, the team uses four custom GitHub Copilot agents coordinated through GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 4.7 (copilot). Responsibility: research the codebase and dependencies, identify edge cases, and produce a practical implementation plan the team can execute. Definition: `.github/agents/planner.agent.md`.
- Designer — Model: Gemini 3.1 Pro (copilot). Responsibility: shape the dashboard experience with UI/UX, accessibility, information hierarchy, visual polish, and responsive design. Definition: `.github/agents/designer.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsibility: implement the assigned features, fix bugs, and validate behavior within the scoped files for the dashboard. Definition: `.github/agents/coder.agent.md`.
- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsibility: break the work into phases, delegate to the specialist agents, manage overlap and sequencing, and verify the final integration. Definition: `.github/agents/orchestrator.agent.md`.

This custom agent team is managed through GitHub Copilot CLI in a Codespace so the planner, designer, coder, and orchestrator can work together on the Mona's Project Pulse dashboard in a coordinated way.
