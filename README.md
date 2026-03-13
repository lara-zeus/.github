# Organization-level shared GitHub configuration for [lara-zeus](https://github.com/lara-zeus).

## Running the Sync Workflow Locally

### Prerequisites

1. **GitHub CLI (`gh`)**

   ```bash
   # macOS
   brew install gh

   # Ubuntu/Debian
   sudo apt install gh
   ```

   Then authenticate:

   ```bash
   gh auth login
   ```

2. **Act** — run GitHub Actions locally

   ```bash
   # macOS
   brew install act

   # Other platforms: https://nektosact.com/installation
   ```

### Run the sync workflow

```bash
act workflow_dispatch -W .github/workflows/sync.yml -s GH_TOKEN=$(gh auth token)
```

This will sync all files under `.github/` (respecting `.syncignore`) to the repos listed in `sync-repos.json`.

## Configuration Files

### `sync-repos.json`

Defines which repos and branches to sync to. The key is the repo name (under the `lara-zeus` org), and the value is an array of branches to sync.

```json
{
  "bolt": ["4.x"],
  "core": ["main", "3.x"],
  "thunder": ["main", "2.x", "3.x"]
}
```

If the branches array is empty (`[]`), the workflow falls back to the repo's default branch.

### `sync-labels.json`

Defines labels and their colors to be created/updated on target repos. The key is the label name, and the value is the hex color (without `#`).

```json
{
  "bug": "d73a4a",
  "enhancement": "a2eeef"
}
```

Labels referenced in issue templates (via `labels: [...]`) are automatically synced. This file controls their colors. Labels not found in this file default to grey (`ededed`).

### `.syncignore`

Controls which paths under `.github/` are excluded from syncing and cleanup. Works like `.gitignore` — one path prefix per line, comments with `#`.

```
.github/workflows/
```

Files matching these patterns will not be synced to target repos and will not be deleted from them during cleanup.
