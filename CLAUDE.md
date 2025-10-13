# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repository contains a **Claude Code plugin marketplace** for the **Zettelkasten Flow** plugin, which transforms voice-transcribed notes into structured Zettelkasten notes within Obsidian. The plugin automates the workflow of processing voice captures from SuperWhisper into atomic, well-linked permanent notes.

## Plugin Architecture

### Plugin System Structure

```
diane/                              # Marketplace root
├── .claude-plugin/
│   └── marketplace.json            # Marketplace manifest
└── diane/                          # Plugin directory
    ├── .claude-plugin/
    │   └── plugin.json             # Plugin configuration
    ├── commands/                   # Slash command definitions
    │   ├── playback.md             # /playback command (voice note processor)
    │   ├── consult.md              # /consult command (vault analyst)
    │   └── find-links.md           # /find-links command (semantic linking)
    └── templates/                  # Obsidian Templater templates
        ├── permanent-note.md
        ├── fleeting-note.md
        ├── literature-note.md
        └── project-note.md
```

### Key Concepts

- **Marketplace**: A local plugin registry defined by `.claude-plugin/marketplace.json` at the repository root
- **Plugin**: A collection of slash commands, templates, and configuration stored in a subdirectory
- **Slash Commands**: Markdown files in `commands/` that define prompts Claude Code executes when invoked
- **Plugin Configuration**: JSON file defining vault paths, folder structure, and naming conventions

## Obsidian Vault Structure

The plugin operates on an Obsidian vault at:
`/Users/marcusestes/Library/Mobile Documents/iCloud~md~obsidian/Documents/Slip Box`

**Folder hierarchy:**
- `00 Diane/` - Voice note captures from SuperWhisper (timestamped files like `2025-10-13-0930.md`)
- `10 Fleeting notes/` - Quick captures, underdeveloped thoughts
- `20 Literature notes/` - Insights from books, articles, sources
- `30 Permanent notes/` - Atomic, well-developed ideas (highest value)
- `40 Project notes/` - Goal-oriented work, active projects
- `99 Output/` - Published work
- `_templates/` - Templater templates for note creation

**Naming conventions:**
- Files: kebab-case (e.g., `ritual-interface-bridge.md`)
- Wikilinks: Display name format (e.g., `[[Ritual Interface Bridge]]`)

## Commands

### `/playback` - Voice Note Processor

**Purpose:** Process voice-transcribed notes from `00 Diane/` into structured Zettelkasten notes

**Workflow:**
1. List unprocessed voice notes with preview
2. User selects a note to process
3. Read and analyze content:
   - Expand conversational transcription into clear prose
   - Fix transcription errors and complete thoughts
   - Identify atomic concepts (one idea per note)
   - Detect implicit references by checking recent notes
4. Find semantic connections across entire vault
5. Generate preview with suggested title, destination folder, and wikilinks
6. User approves, edits, skips, or cancels
7. Create note in destination folder, archive original to `00 Diane/processed/`

**Key behaviors:**
- Interactive (one note at a time)
- Preserves user's writing voice
- Uses semantic search (not just keyword matching) for connections
- Suggests splitting multi-concept notes
- Resolves vague references ("that article") by temporal/semantic context

### `/find-links` - Semantic Link Discovery

**Purpose:** Find semantically related notes and suggest wikilinks for any note

**Workflow:**
1. User specifies target note (current file, path, or paste content)
2. Read and understand note's main concepts
3. Search entire vault for conceptual relationships:
   - Prioritize permanent notes (highest value)
   - Include literature notes for evidence
   - Connect to active projects
   - Flag orphaned fleeting notes
4. Rank suggestions by confidence level (High/Medium/Potential)
5. Provide wikilink text ready to paste
6. Optionally suggest bidirectional links and identify missing bridge notes

**Key behaviors:**
- Focus on conceptual relationships, not keyword matches
- Explain why each connection is relevant
- Identify conceptual clusters and gaps in knowledge graph
- Suggest bridge notes to strengthen vault structure

## Note Format

All notes follow atomic Zettelkasten structure:

```markdown
---
created: YYYY-MM-DD HH:mm
modified: YYYY-MM-DD HH:mm
type: permanent|fleeting|literature|project
tags: [tag1, tag2]
---

# Note Title

## Idea

One clear, well-articulated concept in 1-3 sentences.

## Why it matters

Why this idea is significant, what it connects to, implications.

## Evidence / References

- Source material
- Prior thoughts
- Related reading

## Links

[[Related Note 1]] [[Related Note 2]]
```

## Development Workflow

### Testing Plugin Changes

When modifying slash commands or templates:

1. **Edit command files** in `diane/commands/`
2. **No rebuild needed** - commands are read dynamically
3. **Restart Claude Code** if adding new commands or changing `plugin.json`
4. **Test in Obsidian vault** to verify note creation and linking

### Adding New Commands

1. Create markdown file in `diane/commands/[command-name].md`
2. Include frontmatter with description:
   ```markdown
   ---
   description: Brief description shown in /help
   ---
   ```
3. Write detailed prompt for Claude Code behavior
4. Update README.md with command documentation

### Modifying Plugin Configuration

Edit `diane/.claude-plugin/plugin.json` to change:
- Vault path
- Folder names
- Naming conventions
- Template locations

### Installing Plugin Locally

```bash
# From this directory
/plugin marketplace add /Users/marcusestes/Websites/diane
/plugin install diane@diane
```

## Important Constraints

- **iCloud sync delays**: Vault is on iCloud Drive, so file operations may have sync latency
- **Wikilink format**: Must use display name format `[[Display Name]]` mapping to `display-name.md`
- **Atomic notes**: Each permanent note should contain exactly one clear idea
- **Template dependency**: Requires Obsidian Templater plugin for note creation
- **Voice note archival**: Always move processed notes to `00 Diane/processed/` to avoid reprocessing

## Philosophy

Based on Sönke Ahrens' "How to Take Smart Notes":

- **Reduce capture friction**: Voice → structured note in minimal steps
- **Build knowledge graph**: Every note should connect to existing ideas
- **Atomic notes**: One idea, one note, many connections
- **Semantic over keyword**: Find conceptual relationships, not just text matches
- **Progressive elaboration**: Fleeting → Literature → Permanent → Project → Output
