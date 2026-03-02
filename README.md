# Merge Mate — CLI

Command-line tool for syncing branches with AI-powered conflict resolution.

## Installation

**Linux / macOS:**

```bash
curl -fsSL https://raw.githubusercontent.com/gitkraken/merge-mate-cli/main/install/install.sh | bash
```

**Windows (PowerShell):**

```powershell
irm https://raw.githubusercontent.com/gitkraken/merge-mate-cli/main/install/install.ps1 | iex
```

**Options:**

```bash
# Install specific version
curl -fsSL .../install.sh | bash -s -- --version 0.1.0

# Custom installation directory
curl -fsSL .../install.sh | bash -s -- --dir /usr/local/bin
```

Or download binaries manually from [GitHub Releases](https://github.com/gitkraken/merge-mate-cli/releases).

## Usage

```bash
merge-mate <command> [branches...] [options]
```

## Commands

### `rebase` / `merge` — Sync branches

Rebase branches onto (or merge into them) the target branch, resolving conflicts with AI.

```bash
merge-mate rebase feature-1 feature-2 --base main
merge-mate merge feature-1 --base develop --apply-policy dry-run
```

If no branches are specified, an interactive picker is shown.

| Option | Default | Description |
|--------|---------|-------------|
| `--base` | `main` | Target branch to sync with |
| `--provider` | `gitkraken` | AI provider: `gitkraken` |
| `--model` | `anthropic/claude-sonnet-4-5` | AI model identifier (provider-specific) |
| `--api-key` | `$MERGE_MATE_API_KEY` | API key for the AI provider |
| `--cache` | `true` | Use git rerere cache for previously resolved conflicts |
| `--apply-policy` | `auto` | `auto` — apply if above threshold. `hidden-only` — save to refs only. `dry-run` — preview, no push |
| `--confidence-threshold` | `100` | Minimum AI confidence (0–100) to auto-apply. `100` = only when fully confident |
| `--telemetry` | `true` | Enable telemetry and error tracking |

### `status` — Show sync results

```bash
merge-mate status
```

Displays a table with branch, base, mode, sync status, confidence, resolved files count, and application status.

### `review` — Inspect AI resolutions

```bash
merge-mate review feature-1
merge-mate review --resolved-only
```

Shows detailed resolution report: strategy, confidence, AI reasoning, and review hints per file. Optionally opens a diff viewer comparing before/after states.

| Option | Default | Description |
|--------|---------|-------------|
| `--resolved-only` | `false` | Only show files with conflicts (hide clean files) |

### `apply` — Apply resolved changes

```bash
merge-mate apply feature-1 feature-2
merge-mate apply --all --confidence-threshold 80
```

Applies sync results to branch refs. If the branch is currently checked out, updates the working tree.

| Option | Default | Description |
|--------|---------|-------------|
| `--all` | `false` | Apply all pending results |
| `--confidence-threshold` | `100` | Minimum AI confidence (0–100) to apply. `100` = only when fully confident |

### `rollback` — Undo applied changes

```bash
merge-mate rollback feature-1
merge-mate rollback --all
```

Restores branches from backup refs created during sync.

| Option | Default | Description |
|--------|---------|-------------|
| `--all` | `false` | Rollback all applied branches |

### `clean` — Remove sync state

```bash
merge-mate clean feature-1
merge-mate clean --all
```

Deletes backup refs, resolved refs, and sync report files.

| Option | Default | Description |
|--------|---------|-------------|
| `--all` | `false` | Clean all branches |

## Environment Variables

| Variable | Description |
|----------|-------------|
| `MERGE_MATE_API_KEY` | API key for the AI provider |
| `MERGE_MATE_API_BASE` | Custom API base URL |
| `LOG_LEVEL` | Logging verbosity: `error` \| `warn` \| `info` \| `debug`. Disables interactive TTY mode when set |

## Typical Workflow

```bash
# 1. Sync branches with AI resolution
merge-mate rebase feature-1 feature-2 --base main

# 2. Check results
merge-mate status

# 3. Review AI decisions
merge-mate review feature-1

# 4. Apply if satisfied
merge-mate apply feature-1

# 5. Or rollback if not
merge-mate rollback feature-1

# 6. Clean up when done
merge-mate clean --all
```
