---
name: hut-bugreport
description: "Creates a bug report ticket on sourcehut using the hut CLI. Use when asked to file a bug, create a bugreport, report an issue, or read a bug report for a sourcehut-hosted project."
---

# Hut Bug Report

Creates a bug report ticket on a sourcehut project's bug tracker using the `hut` CLI.

## Workflow

### Step 1: Identify the Repository

Run `hut git show` to get info about the current repository. Note:
- The repository name (used for the bug tracker)
- The visibility (in round braces, e.g. `private`, `public`) — this is needed if a tracker must be created

### Step 2: Ensure the Bug Tracker Exists

Run `hut todo list` and check if a tracker matching the repository name appears in the output.

- If a tracker for the repo **exists**, proceed to Step 3.
- If a tracker **does not exist**, create one:
  ```
  hut todo create <repo-name> -v <visibility>
  ```
  where `<visibility>` matches the repo visibility from Step 1 (e.g. `private` or `public`).

### Step 3: Create the Ticket

Construct the ticket content with a title and description. The title **must** end with `(LLM-generated)`.

Pipe the content into `hut todo ticket create` via stdin:

```
echo "<title> (LLM-generated)

<description>" | hut todo ticket create --stdin
```

- The title is the first line.
- Two newlines separate the title from the description body.
- The description should contain all relevant bug details: what is broken, steps to reproduce, expected vs actual behavior, and any relevant context gathered from the codebase or the user's report.

### Step 4: List and Read Existing Bug Reports

Use `todo ticket` subcommands (not `hut tickets`) to inspect reports.

- List trackers:
  ```
  hut todo list
  ```
- List tickets in a tracker:
  ```
  hut todo ticket list -t <tracker-name>
  ```
- List more tickets:
  ```
  hut todo ticket list -t <tracker-name> --count 20
  ```
- Show a specific ticket by ID:
  ```
  hut todo ticket show -t <tracker-name> <ticket-id>
  ```

Tested example (`yala`):
```
hut todo ticket list -t yala --count 20
hut todo ticket show -t yala 7
```

### Guidelines

- Gather as much context as possible from the user and the codebase before writing the ticket.
- Write clear, actionable titles (e.g. "API key not loaded from .env (LLM-generated)").
- Structure the description with sections like **Description**, **Steps to Reproduce**, **Expected Behavior**, **Actual Behavior** where applicable.
- If the user provides incomplete information, fill in what you can from the code and note any unknowns in the ticket.
