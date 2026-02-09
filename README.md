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
merge-mate <command> [options]
```

### Commands

| Command | Description |
|---------|-------------|
| `rebase` | Rebase current branch onto target branch |
| `merge` | Merge target branch into current branch |

### Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `--target` | string | repository default branch | Target branch to sync with |
| `--source` | string | current branch | Source branch to sync |
| `--apply` | string | `interactive` | Apply strategy: `interactive`, `auto`, `dry-run` |

### Examples

```bash
merge-mate rebase --target main
merge-mate merge --target main --apply dry-run
```
