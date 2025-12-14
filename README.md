# Claude Plugins Marketplace

A collection of Claude Code plugins for Drupal development and skill creation.

## Installation

Add this marketplace to Claude Code:

```bash
/plugin marketplace add yoggu/claude-plugins
```

Then install individual plugins:

```bash
/plugin install drupal-updates@yoggu
/plugin install drupal-update-orchestrator@yoggu
/plugin install skill-creator@yoggu
```

Or use the interactive plugin menu:

```bash
/plugin
```

## Available Plugins

### drupal-updates

Perform Drupal core and contributed module updates using Composer.

**Use when:** Updating Drupal core, contrib modules, checking for updates, running database updates, or handling patch failures.

**Features:**
- Step-by-step update workflow
- Database sync from production
- Configuration export handling
- Common issue troubleshooting

### drupal-update-orchestrator

Orchestrate Drupal updates across multiple projects.

**Use when:** Updating multiple Drupal sites, batch updating projects, or managing updates across a portfolio of sites.

**Features:**
- Project discovery across configured directories
- Interactive project selection
- Launches individual Terminal sessions per project
- Works with the drupal-updates plugin

### skill-creator

Guide for creating effective Claude Code skills.

**Use when:** Creating new skills or updating existing skills that extend Claude's capabilities.

**Features:**
- Skill structure guidance
- Best practices for skill authoring
- Validation and packaging scripts
- Reference documentation for patterns

## License

MIT
