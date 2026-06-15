# Git History (soksak-plugin-git-history)

A commit history panel. Shows history (⎇) in the right sidebar and lets you browse commits.

- **List**: 50 most recent commits in reverse chronological order — short hash (monospace) + subject, author and date (dimmed).
- **Pagination**: "Load more" button at the bottom appends the next 50 commits (accumulated skip). "⟳" refresh reloads the list from the beginning.
- **Commit detail**: Clicking a row replaces the list with the detail view — metadata (subject/hash/author/date) + changed file list (status letter + path) + full patch (`<pre>`, scrollable, max-height capped). "← List" to go back.
- **Honest failures**: Non-git directories and git errors are shown as dimmed error text in the empty state (no silent failures).
- **Safe rendering**: External strings such as commit subject, paths, and patch content are all inserted via `textContent` — no `innerHTML`.

## Permission Rationale

| Permission | Rationale |
| --- | --- |
| `ui` | Register and display the history view (`history`) in the sidebar |
| `git:read` | Read calls to `git.log` (commit list) and `git.show` (commit detail) |

No other permissions declared — no write, filesystem, command, or network access.

## Installation

```bash
# GitHub shorthand
sok plugin.install '{"source":"<user>/soksak-plugin-git-history"}'

# Local path (this example directory)
sok plugin.install '{"source":"/path/to/repo/examples/plugins/soksak-plugin-git-history"}'

# Activate (consent must be given by a human in the app UI)
sok plugin.enable '{"id":"soksak-plugin-git-history"}'
```

## Usage

1. Open ⎇ (git history) in the right sidebar icon rail.
2. Scroll through the commit list; press "Load more" to fetch older history.
3. Click a commit to see the changed file list and patch. Press "← List" to go back.
4. After making a new commit, press "⟳" to refresh the list.

## DOM Exposure (Structural Addresses)

The host accesses the DOM via structural path addresses instead of arbitrary CSS selectors. Elements exposed by this plugin to the outside (address clicks/measurements, E2E) are declared in the manifest (`contributes.nodes`) and have a `data-node` attribute on the actual element. Undeclared or unattributed elements are not accessible (`NOT_EXPOSED`).

Global address: `win/<label>/<region>/view/soksak-plugin-git-history.history/node/<data-node>`

| Node | data-node | Description |
| --- | --- | --- |
| `commit` | `commit/<commit-hash>` | Commit row (click to view detail). Stable key is the full commit hash (lowercase hex) — not a counter index |
| `more` | `more` | "Load more" button (load next page) |
| `refresh` | `refresh` | "⟳" refresh button (reload list from the beginning) |
