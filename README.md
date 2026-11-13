# Witness

Witness is an early, local-first project-memory prototype. This version records Git commit activity and provides a basic local dashboard and handoff document. It does **not** analyze code for security or correctness, read AI-agent session logs, or claim that a project is safe.

## Requirements

- Python 3.10+
- Git

## Install

From a clone of this repository:

```bash
python -m pip install .
```

For development, use an editable install instead:

```bash
python -m pip install -e .
```

## Start recording a project

Run `init` once for each Git repository:

```bash
witness init /path/to/your/repo
```

Witness stores the latest 100 commits in a SQLite database inside that repository's Git metadata directory, seeds a brief overview from the README and top-level item names, then installs a Git `post-commit` hook to record later commits. It captures commit messages, dates, authors, and changed file names. It does not copy source contents or send data anywhere.

The local database also keeps append-only memory snapshots and a structured place for future review results. Each review record is tied to a known commit and carries its classification, explanation, review engine, and evidence references; an alert must include a concrete next step. This storage foundation does not itself run reviews or generate alerts. Memory snapshots record what changed and where the supporting information came from; they do not turn commit history into verified project decisions.

If a post-commit hook already exists, Witness leaves it untouched and reports how to connect Witness manually:

```sh
witness hook /path/to/your/repo
```

Add that command to the existing hook if you want both hooks to run.

Check setup and recorded history:

```bash
witness status /path/to/your/repo
```

## View the local dashboard

```bash
witness dashboard /path/to/your/repo
```

Open the printed URL in a browser. By default, the dashboard binds to `127.0.0.1` and is reachable only from the same machine. Stop it with Ctrl+C. To use a different local port:

```bash
witness dashboard /path/to/your/repo --port 8766
```

## Export project memory

```bash
witness memory /path/to/your/repo --output memory.md
```

The memory is an additive activity record built from commits Witness has captured, plus the initial README-based project overview. The initial overview is a snapshot and is not silently rewritten if the README later changes. Commits outside the initial 100-commit seed range are not backfilled. The file is local and is not uploaded or shared.

## Other command

Print a summary directly from Git history without setting up recording:

```bash
witness summarize /path/to/your/repo
```

## Current limitations

This is the local foundation, not the complete product described in the product requirements. It does not yet provide an AI-based second-pass review, silent-pass/informational/alert classification, local agent-session adapters, notifications, accounts, remote sharing, or a full decision-reasoning memory. The dashboard displays observed Git activity only; its output must not be treated as a security review or a guarantee.
# Witness
