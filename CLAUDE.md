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

## Configuration Pattern

**IMPORTANT: This is an open-source plugin designed for use by multiple users.**

### Path Configuration

All directory paths are **configured during setup** via the `/diane:setup` command and stored in `diane/.claude-plugin/plugin.json`. Commands must **never hardcode paths** - they should always read from the plugin configuration.

**Configuration variables (in `plugin.json`):**
- `vault_path` - Absolute path to the user's Obsidian vault (empty by default, set during setup)
- `diane_folder` - Name of the Diane folder for voice captures
- `folders` - Object containing `fleeting`, `literature`, `permanent`, `project`, `output` folder names
- `naming` - Contains `style` (kebab-case) and `wikilink_format` (display-name)

### Obsidian Vault Structure

The plugin operates on any Obsidian vault configured by the user during setup.

**Default folder hierarchy:**
- `00 Diane/` - Voice note captures from SuperWhisper (timestamped files like `2025-10-13-0930.md`)
- `10 Fleeting notes/` - Quick captures, underdeveloped thoughts
- `20 Literature notes/` - Insights from books, articles, sources
- `30 Permanent notes/` - Atomic, well-developed ideas (highest value)
- `40 Project notes/` - Goal-oriented work, active projects
- `99 Output/` - Published work
- `_templates/` - Templater templates for note creation

**Note:** Users may customize these folder names during setup. Commands should reference the configured folder paths, not assume these defaults.

**Naming conventions:**
- Files: kebab-case (e.g., `ritual-interface-bridge.md`)
- Wikilinks: Display name format (e.g., `[[Ritual Interface Bridge]]`)

## Commands

### `/playback` - Voice Note Processor

**Purpose:** Process voice-transcribed notes from the Diane folder into structured Zettelkasten notes

**Workflow:**
1. Read plugin configuration to get vault path and folder structure
2. List unprocessed voice notes with preview from configured Diane and Fleeting folders
3. User selects a note to process
4. Read and analyze content:
   - Expand conversational transcription into clear prose
   - Fix transcription errors and complete thoughts
   - Identify atomic concepts (one idea per note)
   - Detect implicit references by checking recent notes
5. Find semantic connections across entire vault
6. Generate preview with suggested title, destination folder, and wikilinks
7. User approves, edits, skips, or cancels
8. Create note in destination folder, archive original to configured processed subfolder

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
# From the plugin repository directory
/plugin marketplace add <path-to-diane-repo>
/plugin install diane@diane
```

### Accessing Configuration in Commands

When writing command prompts, always:

1. **Read configuration first** - Load `diane/.claude-plugin/plugin.json` to get vault path and folder structure
2. **Use configured paths** - Construct file paths using `vault_path` + folder name from config
3. **Never hardcode** - Don't assume vault locations or folder names
4. **Validate paths exist** - Check that `vault_path` is not empty and directories are accessible before operations

Example pattern:
```markdown
1. Read configuration from diane/.claude-plugin/plugin.json
2. Extract vault_path and diane_folder (or folders.fleeting, etc.)
3. Construct full path: ${vault_path}/${diane_folder}
4. Validate that vault_path is not empty (if empty, prompt user to run /diane:setup)
5. Scan for notes in that directory
```

## Important Constraints

- **No hardcoded paths**: This is an open-source plugin - always read paths from plugin configuration, never hardcode directory locations
- **iCloud sync delays**: Many users store vaults on iCloud Drive, so file operations may have sync latency
- **Wikilink format**: Must use display name format `[[Display Name]]` mapping to `display-name.md`
- **Atomic notes**: Each permanent note should contain exactly one clear idea
- **Template dependency**: Requires Obsidian Templater plugin for note creation
- **Voice note archival**: Always move processed notes to the configured processed folder to avoid reprocessing

## Philosophy

Based on Sönke Ahrens' "How to Take Smart Notes":

- **Reduce capture friction**: Voice → structured note in minimal steps
- **Build knowledge graph**: Every note should connect to existing ideas
- **Atomic notes**: One idea, one note, many connections
- **Semantic over keyword**: Find conceptual relationships, not just text matches
- **Progressive elaboration**: Fleeting → Literature → Permanent → Project → Output
