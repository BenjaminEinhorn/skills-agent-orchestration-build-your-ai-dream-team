# Project Pulse — Implementation Plan

This document is the Planner's implementation plan for building Mona's Project Pulse dashboard in this repository. Save verbatim to docs/project-pulse-plan.md.

## Summary

Build a lightweight, responsive static dashboard (app/index.html + app/styles.css) that reads a JSON dataset (app/project-data.json) and displays project cards, status badges, priority, progress, and key metadata. Provide a deterministic VS Code launch configuration (.vscode/launch.json) to run a simple static server from ${workspaceFolder}/app. Designer creates the visual styling; Coder implements HTML, data file, and launch configuration. Orchestrator coordinates and validates integration. Planner created this plan.

Key design hooks (shared contract):
- Root dashboard element: `.dashboard`
- Project card: `.project-card`
- Status badge: `.status-badge`
- Priority badge: `.priority`
- Progress bar: `.progress-bar`
- Data file: `app/project-data.json`

Target runtime: static site preview via Python 3 simple HTTP server (or equivalent). No build step required.

## Ordered implementation steps (with ownership and exact files)

1) Planner: docs/project-pulse-plan.md (create)

2) Orchestrator: create tasks; schedule work

3) Designer — Owner: Designer
   - Files: app/styles.css (create), docs/project-pulse-design-notes.md (optional)
   - Deliver: responsive CSS implementing hooks (.dashboard, .project-card, .status-badge, .priority, .progress-bar), CSS variables, accessibility (contrast, focus states).

4) Coder — Owner: Coder
   - Files: app/index.html (create), app/project-data.json (create), app/scripts.js (optional), .vscode/launch.json (create)
   - Deliver: index.html loads styles and fetches project-data.json; sample data (>=6 entries); launch.json with cwd ${workspaceFolder}/app (python http.server).

5) Integration: Designer + Coder (coordinated by Orchestrator)
   - Files: app/index.html, app/styles.css, app/project-data.json, app/scripts.js
   - Deliver: visual polish, interactivity, agreed class names.

6) Validation & QA — Owner: Orchestrator
   - Files: docs/project-pulse-validation.md (create)
   - Deliver: runbook and results; assign fixes.

7) Optional agent docs update: .github/agents/*.agent.md (owned by respective agents)

## File assignments (explicit)
- app/index.html — Coder
- app/styles.css — Designer
- app/project-data.json — Coder
- .vscode/launch.json — Coder
- app/scripts.js — Coder (optional)
- docs/project-pulse-design-notes.md — Designer (optional)
- docs/project-pulse-validation.md — Orchestrator

## Dependencies & environment
- Codespace or local env with Python 3
- Run server: cd app && python -m http.server 5173 (or use .vscode/launch.json)
- Browser for preview; optional accessibility tools (axe)
- No build step required (vanilla HTML/CSS/JS)

## Designer & Coder responsibilities
- Designer: create polished, accessible CSS; document variables and class hooks; provide responsive breakpoints and focus states.
- Coder: implement index.html and simple JS to fetch/render app/project-data.json; create deterministic sample data; provide .vscode/launch.json with cwd ${workspaceFolder}/app; handle missing/invalid data gracefully.

## Parallel work decisions
- Can run in parallel: Designer (styles) and Coder (HTML, data, launch config) if class hooks contract is followed.
- Must be sequential: final integration and accessibility polishing after both complete.

## Validation expectations
Manual checks:
- Launch server via .vscode/launch.json or python -m http.server 5173 and open http://localhost:5173/
- Verify index.html loads and shows sample project cards (>=6)
- Each card shows title, status badge, priority, owner, and progress
- Responsive layout: desktop grid and single-column mobile
- Keyboard accessibility: focusable cards and controls
- Data error handling: UI shows placeholder/error for missing or malformed data
- .vscode/launch.json must set cwd to ${workspaceFolder}/app

## Example templates (short)
app/project-data.json sample:
[
  {"id":"proj-1","name":"Website Redesign","status":"In Progress","priority":"High","progress":62,"owner":"Aisha"},
  {"id":"proj-2","name":"Mobile App MVP","status":"At Risk","priority":"Medium","progress":34,"owner":"Leo"}
]

.vscode/launch.json sample (cwd requirement):
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Serve Project Pulse (python http.server)",
      "type": "python",
      "request": "launch",
      "module": "http.server",
      "args": ["5173"],
      "cwd": "${workspaceFolder}/app",
      "console": "integratedTerminal"
    }
  ]
}

## Edge cases & risks
- Malformed or missing JSON -> show friendly error
- Long text -> truncate with tooltip
- Large dataset -> ensure performance
- CDN blocked -> degrade gracefully (no required external deps)

End of plan.