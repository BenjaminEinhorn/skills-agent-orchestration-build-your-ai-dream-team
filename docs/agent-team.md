# Agent team

The Project Pulse dashboard uses a custom team of specialist agents:

- Orchestrator — Claude Opus 4.7: coordinates work, breaks plans into file-scoped tasks, and verifies integration. Definition: .github/agents/orchestrator.agent.md
- Planner — Claude Opus 4.7: researches the repository, produces ordered implementation steps, file assignments, and validation criteria. Definition: .github/agents/planner.agent.md
- Coder — GPT-5.5 (copilot): implements code, fixes bugs, and creates runnable support (e.g., .vscode/launch.json; ensures index.html is open for preview). Definition: .github/agents/coder.agent.md
- Designer — Gemini 3.1 Pro (copilot): handles UI/UX, accessibility, and visual styling (use hooks like .dashboard and .project-card). Definition: .github/agents/designer.agent.md

All work is coordinated via the GitHub Copilot CLI running inside a Codespace to orchestrate tasks and specialist agents.
