---
name: hut-bugreport
description: "Creates a bug report ticket on sourcehut using the hut CLI. Use when asked to file a bug, create a bugreport, or report an issue for a sourcehut-hosted project."
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

### Guidelines

- Gather as much context as possible from the user and the codebase before writing the ticket.
- Write clear, actionable titles (e.g. "API key not loaded from .env (LLM-generated)").
- Structure the description with sections like **Description**, **Steps to Reproduce**, **Expected Behavior**, **Actual Behavior** where applicable.
- If the user provides incomplete information, fill in what you can from the code and note any unknowns in the ticket.
