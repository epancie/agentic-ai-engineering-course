# Copilot / AI Agent Instructions — agentic-ai-engineering-course

Quick, actionable guide to get an AI coding agent productive in this repository.

## Project snapshot
- Purpose: a 6‑week educational course with runnable labs and agent demos (OpenAI Agents, CrewAI, LangGraph, AutoGen, MCP).
- Top-level structure: `1_foundations/`..`6_mcp/` (week labs), `guides/` (how-to notebooks), `setup/` (install docs).

## Quickstart (developer setup)
1. Read `README.md` and `setup/SETUP-*.md` for environment steps (mac/windows/linux). ✅
2. Install dependencies with UV (recommended in course):
   - `uv sync` (installs Python & packages per repo `pyproject.toml`)
   - If needed: `uv --native-tls sync` or `uv --allow-insecure-host github.com sync`.
3. Create `.env` in the repo root and add keys used by notebooks/apps, e.g.:
   - `OPENAI_API_KEY=...`, `ANTHROPIC_API_KEY=...`, `GOOGLE_API_KEY=...`, `POLYGON_API_KEY=...`
4. If notebooks use browser automation: run `playwright install` (see `4_langgraph/3_lab3.ipynb`).

## How to run the main demos
- Gradio UIs: `cd 6_mcp && python app.py` (or `python 6_mcp/app.py` from repo root) — opens in browser.
- MCP servers (trader demo): use `uv` as orchestration or run directly inside `6_mcp`:
  - `uv run accounts_server.py`
  - `uv run push_server.py`
  - `uv run market_server.py` (or let `mcp_params.py` choose a polygon-backed server if configured)
- Many lab examples are runnable as Jupyter notebooks under each week (open with Jupyter/VS Code/ Cursor).

## Repo conventions & important patterns
- Notebooks are the primary instructional artifacts; code living next to them (e.g., `app.py`, `*.py`) are runnable reference implementations.
- Use `.env` for secrets (do **not** hardcode credentials). Many READMEs mention this (e.g., `3_crew/*/README.md`).
- Package management: project expects Python >= 3.12 (see `pyproject.toml`) and uses `uv` as package manager; many subfolders have independent `uv.lock` files.
- Servers & orchestration:
  - `6_mcp/mcp_params.py` shows how MCP services are launched (via `uv`, `uvx`, or `npx`).
  - Tracing/logging is implemented in `6_mcp/tracers.py` and persisted to `6_mcp/accounts.db` via `6_mcp/database.py`.
- Tests: some subprojects contain tests (`3_crew/**/test_*.py` or `unittest`-based files). Run tests per-subfolder with `pytest` or `python -m unittest`.

## Debugging tips / quick checks
- If a Gradio UI behaves oddly, check `6_mcp/accounts.db` (SQLite) via `6_mcp/database.py` helpers; logs are stored in `logs` table.
- If dependent services fail, inspect `6_mcp/mcp_params.py` to see the specific `uv`/`npx` commands expected.
- To reproduce Playwright failures, ensure `playwright install` was run and the right browser runtime is present.

## Suggested tasks for an AI assistant (how to contribute safely)
- When changing code, prefer small tests and runnable notebook cells that demonstrate the change.
- Keep changes self-contained to one lab or component when possible (easier review and isolation).
- Update or add a short example in the corresponding notebook referencing your change (this repo uses notebooks as canonical docs).
- If you change dependencies, run `uv sync` locally and include updated lock files (`uv.lock`) in the same subfolder.

## Files to inspect first (examples)
- `6_mcp/app.py` — Gradio trader dashboard (how UI hooks call into `accounts` and `database`).
- `6_mcp/mcp_params.py` — orchestration pattern for MCP servers (`uv`, `uvx`, `npx`).
- `6_mcp/tracers.py` and `6_mcp/database.py` — how tracing/logging is recorded and surfaced to UIs.
- `4_langgraph/sidekick.py` and `4_langgraph/3_lab3.ipynb` — examples for Playwright and LangGraph integrations.

---
If anything here is unclear or you want me to expand a section (e.g., add exact test commands for a subfolder or walkthrough for starting MCP servers), say which part and I'll iterate.🔧