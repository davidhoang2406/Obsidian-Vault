# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a personal Obsidian knowledge management vault focused on software engineering, system design, and data engineering. It is synced via git to `https://github.com/davidhoang2406/base_vault.git`.

## Git Workflow

The vault uses the **Obsidian Git** plugin for automated backups with commit message format: `vault backup: {{date}}`. When committing manually, follow the same convention. Pull before push is enabled with merge-based sync.

## Vault Structure

- `Master.md` — top-level hub linking to all 6 learning paths
- `Software Engineer/` — Go language notes and projects (most populated area)
- `System Design/Primer/` — System design interview prep (20+ notes)
- `Books/` — Book notes (Fundamentals of Data Engineering)
- `Cloud/` — AWS, Azure (AZ-900), Google Cloud (mostly empty)
- `Data Engineer/` — Data engineering resources (mostly empty)
- `Algorithm/` — Algorithm learning (mostly empty)

## Note Conventions

- Each major domain has a hub file (e.g., `Software Engineer.md`, `Go/Golang.md`) that links to subtopic notes
- Internal links use Obsidian wiki-link syntax: `[[Note Name]]`
- `autoupdateLinks: true` is configured, so renaming files will auto-update links

## Installed Plugins (`.obsidian/plugins/`)

Key community plugins that affect note structure/queries:
- **Dataview** — enables `dataview` code blocks for querying notes as a database
- **Obsidian Git** — automated backups; do not remove or alter its configuration
- **Copilot** — AI integration; conversations saved to `copilot-conversations/` folder
- **Excalidraw** — embedded diagrams stored as `.excalidraw.md` files
- **Kanban** — markdown-backed boards; syntax is specific to the plugin format

## Content in Progress

The Go section (`Software Engineer/Go/`) has the most depth, covering:
- Language fundamentals (flow control, data structures, pointers, interfaces, defer/panic)
- Project: "Building Blockchain in Go" (prototype → PoW → transactions → persistence → CLI → addresses → network)

System Design (`System Design/Primer/`) covers core components (cache, DB, API gateway, message queues, microservices) and a master interview template.
