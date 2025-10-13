# Zettelkasten Flow Plugin

Transform voice-captured notes into structured Zettelkasten notes with automatic linking and intelligent organization.

## Overview

This Claude Code plugin automates your Zettelkasten workflow by processing unstructured voice transcriptions (captured via SuperWhisper) into atomic, well-linked permanent notes. Inspired by "How to Take Smart Notes," it reduces friction in the capture-to-publish pipeline.

## Features

- **Voice Note Processing** - Expand and clarify voice transcriptions into structured thoughts
- **Intelligent Linking** - Semantic search across your vault to suggest connections
- **Atomic Note Creation** - Ensures each note contains one clear, well-developed idea
- **Context Detection** - Resolves implicit references ("that article I read") automatically
- **Interactive Workflow** - One-by-one review with full control over output

## Commands

### `/playback` - Process Voice Notes

Process voice-captured notes from the Diane folder into structured Zettelkasten notes.

**What it does:**
1. Lists unprocessed voice notes in `00 Diane/`
2. Expands conversational transcriptions into clear prose
3. Finds semantic connections to existing notes
4. Suggests wikilinks and destination folders
5. Creates properly formatted notes with atomic structure

**Usage:**
```bash
/playback
```

### `/find-links` - Discover Connections

Find semantically related notes and suggest wikilinks for any note in your vault.

**What it does:**
1. Analyzes a target note's concepts and themes
2. Searches vault for related notes (semantic, not just keyword matching)
3. Ranks connections by confidence level
4. Provides wikilinks ready to paste
5. Suggests bidirectional links and identifies gaps

**Usage:**
```bash
/find-links
```

## Installation

### Prerequisites

1. **Claude Code** installed on your machine
2. **Obsidian** with your vault set up
3. **Templater plugin** for Obsidian ([install from Community Plugins](https://github.com/SilentVoid13/Templater))

### Step 1: Install Templater in Obsidian

1. Open Obsidian
2. Go to Settings → Community Plugins
3. Browse and search for "Templater"
4. Install and Enable Templater
5. In Templater settings, set:
   - Template folder location: `_templates`
   - Enable "Trigger Templater on new file creation" (optional)

### Step 2: Copy Templates to Your Vault

Copy the template files from this plugin to your vault's `_templates/` folder:

```bash
# Create templates folder in your vault
mkdir -p "/Users/marcusestes/Library/Mobile Documents/iCloud~md~obsidian/Documents/Slip Box/_templates"

# Copy templates
cp templates/* "/Users/marcusestes/Library/Mobile Documents/iCloud~md~obsidian/Documents/Slip Box/_templates/"
```

Or manually copy the four template files:
- `permanent-note.md`
- `fleeting-note.md`
- `literature-note.md`
- `project-note.md`

### Step 3: Set Up Local Marketplace

Create a local marketplace to install this plugin in Claude Code:

```bash
# From the parent directory of the diane plugin
cd /Users/marcusestes/Websites/diane

# Create marketplace structure
mkdir -p .claude-plugin

# Create marketplace manifest
cat > .claude-plugin/marketplace.json << 'EOF'
{
  "name": "diane",
  "owner": {
    "name": "Marcus Estes"
  },
  "plugins": [
    {
      "name": "diane",
      "source": "./diane",
      "description": "Process voice notes into structured Zettelkasten notes"
    }
  ]
}
EOF
```

### Step 4: Install Plugin in Claude Code

```bash
# Start Claude Code
claude

# Add the local marketplace
/plugin marketplace add /Users/marcusestes/Websites/diane

# Install the plugin
/plugin install diane@diane
```

Select "Install now" when prompted, then restart Claude Code.

### Step 5: Verify Installation

After restarting Claude Code:

```bash
# Check that commands are available
/help

# You should see:
# /playback - Process voice-captured notes
# /find-links - Find semantic connections
```

## Folder Structure

Your vault should have this structure:

```
Slip Box/
├── 00 Diane/              # Voice note captures (timestamped files)
│   ├── 2025-10-13-0930.md
│   ├── 2025-10-13-1415.md
│   └── processed/         # Archived voice notes (auto-created)
├── 10 Fleeting notes/     # Quick captures, temporary thoughts
├── 20 Literature notes/   # Insights from sources
├── 30 Permanent notes/    # Atomic, well-developed ideas
├── 40 Project notes/      # Goal-oriented work
├── 99 Output/             # Published work
└── _templates/            # Templater templates
    ├── permanent-note.md
    ├── fleeting-note.md
    ├── literature-note.md
    └── project-note.md
```

## Usage Workflow

### Daily Capture

1. **Throughout the day:** Use iPhone Action Button → SuperWhisper → Notes saved to `00 Diane/`
2. **Evening review:** Run `/playback` in Claude Code
3. **Process each note:**
   - Review expanded version
   - Approve/edit suggested links
   - Choose destination folder
   - Create note
4. **Original voice notes** automatically archived to `00 Diane/processed/`

### Weekly Review

1. **Review fleeting notes:** Check `10 Fleeting notes/` for ideas to promote
2. **Find connections:** Use `/find-links` on recent permanent notes
3. **Strengthen graph:** Add suggested links to build structure
4. **Identify clusters:** Notice emerging themes across notes

### Project Development

1. **Notice patterns:** Related permanent notes suggest project ideas
2. **Create project note:** Use project template in `40 Project notes/`
3. **Gather notes:** Use `/find-links` to collect relevant permanent notes
4. **Build outline:** Structure project around linked notes
5. **Generate draft:** Synthesize linked notes into coherent output

## Note Format

All notes use atomic Zettelkasten structure:

```markdown
---
created: 2025-10-13 14:30
modified: 2025-10-13 14:30
type: permanent
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

[[Related Note 1]] [[Related Note 2]] [[Related Note 3]]
```

### File Naming

- **Format:** kebab-case (e.g., `ritual-interface-bridge.md`)
- **Wikilinks:** Display name format (e.g., `[[Ritual Interface Bridge]]`)
- **Pattern:** descriptive, searchable, lowercase with hyphens

## Configuration

Plugin configuration is stored in `.claude-plugin/plugin.json`:

```json
{
  "vault_path": "/Users/marcusestes/Library/Mobile Documents/iCloud~md~obsidian/Documents/Slip Box",
  "diane_folder": "00 Diane",
  "folders": {
    "fleeting": "10 Fleeting notes",
    "literature": "20 Literature notes",
    "permanent": "30 Permanent notes",
    "project": "40 Project notes",
    "output": "99 Output"
  }
}
```

To customize paths, edit `plugin.json` directly.

## Design Principles

**Simple first, expand based on experience.**

This MVP focuses on the core workflow:
1. Voice capture → structured notes
2. Finding connections
3. Building knowledge graph

Additional features will be added iteratively based on real-world usage:
- Batch processing
- Project synthesis
- Output generation
- Automated review reminders

## Troubleshooting

### "Command not found"

- Verify plugin is installed: `/plugin` → "Manage Plugins"
- Restart Claude Code after installation
- Check marketplace was added correctly

### "Cannot find vault"

- Verify vault path in `plugin.json`
- Check iCloud sync is complete
- Ensure folder names match exactly

### "Templates not working"

- Verify Templater plugin is installed and enabled in Obsidian
- Check template folder path in Templater settings: `_templates`
- Ensure templates were copied to vault's `_templates/` folder

### "Links not creating properly"

- Check wikilink format in note (should be `[[Display Name]]`)
- Verify target notes exist in vault
- Ensure filenames are kebab-case: `display-name.md`

## Future Enhancements

Based on usage patterns, potential additions:

- **Batch processing** - Process multiple voice notes at once
- **Timeline view** - See patterns across recent captures
- **Project synthesis** - Generate drafts from linked notes
- **Automated hooks** - Process notes automatically on capture
- **Literature extraction** - Parse PDFs/articles into literature notes
- **Graph visualization** - See conceptual clusters and gaps

## Contributing

This is a personal workflow plugin, but suggestions and improvements are welcome!

## License

MIT

## Acknowledgments

- Inspired by Sönke Ahrens' "How to Take Smart Notes"
- Built for Obsidian + Claude Code workflow
- Uses Templater plugin for consistent note structure

---

**Version:** 1.0.0
**Author:** Marcus Estes
**Last Updated:** 2025-10-13
