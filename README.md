<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/syspilot-logo-dark.svg">
    <img src="assets/syspilot-logo.svg" alt="syspilot" width="200">
  </picture>
</p>

<p align="center"><strong>Requirements Engineering that scales with AI.</strong></p>

> **⚠️ Early Research Project** — syspilot is under active development. Feel free to use it, fork it, or just study the approach. Updates may introduce breaking changes without migration paths.

> Your project has 5000 requirements. A change affects 5 of them.
> syspilot follows [sphinx-needs](https://sphinx-needs.readthedocs.io/) traceability links to find exactly those 5 — so your AI agent gets focused context, not the entire codebase.

Cover 100% of your specs with 6x less tokens. Links are deterministic — search is probabilistic. syspilot gives your agents the map, not the flashlight. Built on [sphinx-needs](https://sphinx-needs.readthedocs.io/) by [useblocks](https://useblocks.com). Powered by [GitHub Copilot](https://github.com/features/copilot).

## Installation

Install [Git](https://git-scm.com/) and [uv](https://docs.astral.sh/uv/), then
run the command for your harness from the root of the target Git repository.

Production harnesses: GitHub Copilot in VS Code and OpenCode.

### GitHub Copilot in VS Code

```bash
uv run --no-project "https://raw.githubusercontent.com/hubertusgbecker/syspilot/main/syspilot/installer.py" install --harness vscode
```

### OpenCode

```bash
uv run --no-project "https://raw.githubusercontent.com/hubertusgbecker/syspilot/main/syspilot/installer.py" install --harness opencode
```

Run the same command again to update. Run one command for each harness used in
the repository. The defaults are the current directory, repository
`hubertusgbecker/syspilot`, and branch `main`. Synchronous orchestration is
installed deterministically.
For a fork or branch, change the URL and add `--repository <owner>/<repo>
--branch <branch>`.

The command installs only the selected harness's native files, the stable
`.syspilot/installer.py` runtime, shared resources in `.syspilot/skills/` and
`.syspilot/templates/`, and missing documentation bootstrap files.
Installed Setup uses that runtime directly for later updates. Installation
rejects target-path links and rolls back only syspilot-owned mutable paths, so
unrelated project files and concurrent unrelated changes remain untouched.

### Experimental / next target

Claude Code and Qoder adapters are experimental. Their deterministic fixture
generation remains available for development, but neither is a public CLI
target until clean install, update, rollback, and live native invocation UAT
are complete.

GitHub Copilot CLI has been evaluated for artifact compatibility but is not
production-certified. Production support for GitHub Copilot currently means
GitHub Copilot in VS Code.

## What You Get

Four **managers** that orchestrate the work, and seven **engineers** that execute it:

| Managers | What they do |
|----------|-------------|
| `@syspilot.pm` | Plans features, manages backlog, delegates change requests |
| `@syspilot.cm` | Orchestrates engineers through the change workflow |
| `@syspilot.qm` | Runs independent quality checks |
| `@syspilot.setup` | Installs/updates syspilot in your project |

| Engineers | What they do |
|-----------|-------------|
| `@syspilot.design` | Analyzes a change request, creates a Change Document |
| `@syspilot.implement` | Executes approved changes with traceability |
| `@syspilot.uat` | Generates user acceptance test artifacts |
| `@syspilot.verify` | Validates implementation against the Change Document |
| `@syspilot.docu` | Keeps project documentation current |
| `@syspilot.mece` | Finds gaps and redundancies in your specs |
| `@syspilot.trace` | Traces one item through all levels |
| `@syspilot.release` | Manages versioning and release process |

Agents are stable processes (WHAT to do). **Skills** are exchangeable tool bindings (HOW to do it) — customize syspilot by swapping skills, not agents.

## How It Works

```
User Story (WHY)  ──links──▶  Requirements (WHAT)  ──links──▶  Design Specs (HOW)
```

When you request a change, syspilot follows these links to find only the affected elements. **O(affected), not O(total).**

## Documentation

📖 **Full docs:** [hubertusgbecker.github.io/syspilot](https://hubertusgbecker.github.io/syspilot/index.html)

Includes methodology, naming conventions, and traceability matrices.

## Requirements

- **Git**
- **[uv](https://docs.astral.sh/uv/)**
- **GitHub Copilot in VS Code** or **OpenCode**

`uv` resolves the exact Python and package dependencies declared by the remote
PEP 723 script. No project `.venv`, preinstalled Python packages, or `pip`
setup is required.

## License

[Apache License 2.0](LICENSE)

