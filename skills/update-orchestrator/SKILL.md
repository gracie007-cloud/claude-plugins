---
name: update-orchestrator
description: "Orchestrate dependency updates across multiple projects. Use when asked to update multiple sites, batch update projects, manage updates across a portfolio of Drupal sites or Node.js projects, or run updates on several projects at once. Supports both Drupal (Composer) and Node.js (npm) projects."
allowed-tools: Bash(ddev:*), Bash(npm:*), Bash(git:*), Bash(gh:*), Bash(composer:*), Read, Task
---

# Update Orchestrator

Orchestrate dependency updates across multiple Drupal and Node.js projects using subagents for parallel execution.

## Prerequisites

- `drupal-updates` skill available (for Drupal projects)
- `node-modules-update` skill available (for Node.js projects)

## Workflow

### Step 1: Discover Projects

Run the discovery script to find all projects in the current directory. Use the base directory provided at the top of this skill prompt:

```bash
{base_directory}/scripts/discover_projects.sh
```

Replace `{base_directory}` with the actual "Base directory for this skill" path shown above.

Output format:
```json
[
  {"path": "/path/to/site-a", "name": "site-a", "type": "drupal", "version": "10.4.6"},
  {"path": "/path/to/app-b", "name": "app-b", "type": "node", "version": "1.2.3"}
]
```

### Step 2: Ask Project Type

Ask the user what type of projects to update:

```
What type of projects do you want to update?
1. Drupal projects only
2. Node.js projects only
3. Both Drupal and Node.js
```

### Step 3: Interactive Project Selection

Use `AskUserQuestion` with `multiSelect: true` to let users select projects with checkboxes.

**Pagination for large lists:**
AskUserQuestion supports max 4 options per question. For larger lists, paginate:

1. Sort projects alphabetically
2. Split into batches of 4
3. Ask each batch with page indicator (e.g., "1/3")
4. Combine selections from all batches

**Example for 10 Node.js projects:**
```
Page 1/3: "Select Node.js projects to update (1/3):"
  [ ] app-a [1.2.3]
  [ ] app-b [2.0.0]
  [ ] app-c [1.0.0]
  [ ] app-d [3.1.0]

Page 2/3: "Select Node.js projects to update (2/3):"
  [ ] app-e [1.5.0]
  [ ] app-f [2.1.0]
  [ ] app-g [1.0.0]
  [ ] app-h [4.0.0]

Page 3/3: "Select Node.js projects to update (3/3):"
  [ ] app-i [1.2.0]
  [ ] app-j [2.0.0]
```

**AskUserQuestion format:**
```json
{
  "question": "Select Node.js projects to update (1/3):",
  "header": "Node.js",
  "multiSelect": true,
  "options": [
    {"label": "app-a [1.2.3]", "description": "/path/to/app-a"},
    {"label": "app-b [2.0.0]", "description": "/path/to/app-b"},
    {"label": "app-c [1.0.0]", "description": "/path/to/app-c"},
    {"label": "app-d [3.1.0]", "description": "/path/to/app-d"}
  ]
}
```

Collect all selected projects across pages before proceeding to Step 4.

### Step 4: Launch Subagent Updates

For each selected project, launch a subagent using the Task tool:

```
Use the Task tool with subagent_type="general-purpose" for each project:

Prompt template:
"Update the project at {path}.
Project type: {type}
Use the {skill-name} skill workflow.
After creating the PR, return the PR URL.
If the update fails, return the error message."
```

**Subagent configuration:**
- `subagent_type`: "general-purpose"
- `model`: "opus"
- `run_in_background`: true (to run updates in parallel)
- Wait for all subagents to complete using TaskOutput

**Important:** Each subagent must return:
- Status: success or failure
- PR URL (if successful)
- Error message (if failed)

### Step 5: Collect Results

Use TaskOutput to collect results from all subagents. Parse each result for:
- Success/failure status
- PR URL (extract from subagent output)
- Error details if failed

### Step 6: Summary

After all updates complete, display a summary with PR links:

```
=== Update Summary ===

Drupal Projects (2):
✓ site-a
  PR: https://bitbucket.org/workspace/site-a/pull-requests/123
✓ site-b
  PR: https://bitbucket.org/workspace/site-b/pull-requests/456

Node.js Projects (1):
✓ app-a
  PR: https://github.com/org/app-a/pull/789

Failed (1):
✗ app-b (Node)
  Error: Build failed - TypeScript compilation errors

Total: 3 successful, 1 failed
```

## Scripts

### discover_projects.sh
Finds Drupal and Node.js projects in the current directory (1 level deep). Outputs JSON array with path, name, type, and version for each project.
