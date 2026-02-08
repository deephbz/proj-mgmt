# Beads Standards: Repo Tracking

This repo uses Beads (`bd`) to track work across many repositories under `~/repos`.

Goals:
- Each tracked repo has exactly 1 epic (the "repo epic").
- Each non-default branch worth keeping has an issue under that repo epic.
- Each repo with a dirty working tree has at least 1 issue capturing the local state (so nothing gets lost).

Non-goals:
- Encode everything in labels. Prefer labels for filtering and grouping, and put details in description/notes.

## Canonical Entity Model

1. Repo epic
- `type=epic`
- Parent for all issues for a given repo.

2. Branch issue
- `type=task`
- One per branch that represents a coherent unit of work.

3. Unstaged changes issue (local changes)
- `type=task`
- One per distinct "chunk" of local changes if they are separable, otherwise one issue for the whole dirty state.

## Labels (Declarative Taxonomy)

Labels are lowercase ASCII. Use `:` for namespaces. Avoid free-form labels.

### Required labels (all issues created by this workflow)

- `track:repo` (everything in repo-tracking scope)
- `repo:<slug>`
  - `<slug>` is a stable repo identifier (suggested: basename of repo directory).
  - Examples: `repo:logseq`, `repo:attractor`

### Kind labels (exactly one)

- `kind:repo-epic`
- `kind:branch`
- `kind:unstaged-changes`

### Branch labels (branch issues only)

- `branch:<name>`
  - Use the literal git branch name.
  - For detached HEAD, use `branch:detached` and store the commit SHA in the description.

### unstaged-changes state labels (worktree issues only)

Add all that apply (these are intended to be machine-derived from `git status`):
- `wt:dirty` (tracked file modifications)
- `wt:untracked`
- `wt:staged`
- `sync:ahead`
- `sync:behind`
- `wt:conflict` (merge conflicts)

### Lifecycle labels (optional, human judgement)

- `life:active` (you expect to work on it in the next 30 days)
- `life:parked` (valuable, but not soon)
- `life:archive-candidate` (probably safe to archive/delete after verifying)

## Description Templates (Required Sections)

### Repo epic (`type=epic`, `kind:repo-epic`)

Description should include:
- `Repo`
  - `Path: /Users/brz/repos/<name>`
  - `Remote: <origin url or "none">`
  - `Default branch: <branch>`
- `Intent`
  - What this repo is for (one paragraph).
- `Current state`
  - What exists today, what is broken, what is missing.
- `Next actions`
  - Bullet list, ordered.

### Branch issue (`kind:branch`)

Description should include:
- `Repo path: ...`
- `Branch: ...`
- `Base: <main/master/etc or commit>`
- `Goal`
- `Definition of done`
- `Notes`
  - Link to PR/issue if any (`external-ref` if stable).

### unstaged-changes issue (`kind:worktree`)

Description should include:
- `Repo path: ...`
- `Branch/HEAD: ...`
- `Status snapshot`
  - Paste a short summary (counts + key paths). Avoid pasting megabytes.
  - Include whether changes are staged/unstaged/untracked.
- `Intent`
  - What you were trying to do (even if vague).
- `Recovery plan`
  - Concrete next steps (commit, stash, patch-file export, delete, etc.).
