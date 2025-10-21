# CLAUDE.md

Diese Datei beinhaltet Regeln für  (claude.ai/code) bei der Verarbeitung von code in diesem Repository.

## Repository Zweck

Dieses Repository enthält einen **Claude Code-Plugin-Marktplatz** für das **Zettelkasten Flow**-Plugin, das gesprochene Notizen in strukturierte Zettelkasten-Notizen in Obsidian umwandelt. Das Plugin automatisiert den Workflow, um transkribierte Sprachaufnahmen in atomare, gut verknüpfte permanente Notizen zu transformieren.

## Plugin Architectur

### Systemstruktur des Plugin

```
diane/                              # Root des Marktplatzes
├── .claude-plugin/
│   └── marketplace.json            # Marktplatz-Manifest
└── diane/                          # Plugin-Verzeichnis
    ├── .claude-plugin/
    │   └── plugin.json             # Plugin-Konfiguration
    ├── commands/                   # Slash-Befehlsdefinitionen
    │   ├── playback.md             # /playback-Befehl (Sprachnotizen-Verarbeiter)
    │   ├── consult.md              # /consult-Befehl (Vault-Analyst)
    │   └── find-links.md           # /find-links-Befehl (Semantische Verknüpfung)
    └── templates/                  # Obsidian-Templater-Vorlagen
        ├── permanent-note.md
        ├── fleeting-note.md
        ├── literature-note.md
        └── project-note.md
```

### Wichtige Konzepte

- **Marktplatz**: Ein lokales Plugin-Registry, definiert durch `.claude-plugin/marketplace.json` im Repository-Stamm
- **Plugin**: Eine Sammlung von Slash-Befehlen, Vorlagen und Konfiguration, gespeichert in einem Unterverzeichnis
- **Slash-Befehle**: Markdown-Dateien in `commands/`, die Prompts definieren, die Claude Code ausführt, wenn sie aufgerufen werden
- **Plugin-Konfiguration**: JSON-Datei, die Vault-Pfade, Ordnerstruktur und Namenskonventionen definiert

## Konfigurationshinweise

**Wichtig: Dies ist ein Open-Source-Plugin, das für mehrere Benutzer gedacht ist.**

### Pfad Konfiguration

Alle Verzeichnispfade werden **während der Einrichtung konfiguriert** über den Befehl `/diane:setup` und in `diane/.claude-plugin/plugin.json` gespeichert. Befehle dürfen **niemals Pfade harcoden** – sie sollten immer aus der Plugin-Konfiguration lesen.

**Konfigurationsvariablen (in `plugin.json`):**
- `vault_path` - Absoluter Pfad zum Benutzer-Vault (standardmäßig leer, wird während der Einrichtung gesetzt)
- `diane_folder` - Name des Diane-Ordners für Sprachaufnahmen
- `folders` - Objekt mit `fleeting`, `ideas`, `literature`, `permanent`, `project`, `output` Ordner-Namen
- `naming` - Enthält `style` (kebab-case) und `wikilink_format` (Display-Name)

### Obsidian-Vault-Struktur

Das Plugin arbeitet mit jedem Obsidian-Vault, der vom Benutzer während der Einrichtung konfiguriert wird.

**Standard-Ordnerhierarchie:**
- `00 Diane/` - Sprachaufnahmen von SuperWhisper (zeitstempelbasierte Dateien wie `2025-10-13-0930.md`)
- `10 Fleeting notes/` - Schnelle Captures, unentwickelte Gedanken
- `20 Ideas/` - Ausbreitende Proto-Projekte mit vielen Verbindungen, noch nicht strukturiert
- `30 Literature notes/` - Einsichten aus Büchern, Artikeln, Quellen
- `40 Permanent notes/` - Atome, gut entwickelte Ideen (höchster Wert)
- `50 Project notes/` - Zielgerichtete Arbeit, aktive Projekte
- `99 Output/` - Veröffentlichte Arbeit
- `_templates/` - Templater-Vorlagen für Notizen

**Hinweis:** Benutzer können diese Ordner-Namen während der Einrichtung anpassen. Befehle sollten die konfigurierten Pfade referenzieren, nicht die Standardeinstellungen annehmen.

**Naming conventions:**
- Dateien: kebab-case (z.B., `ritual-interface-bridge.md`)
- Wikilinks: Display name format (z.B., `[[Ritual Interface Bridge]]`)

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
type: permanent|fleeting|literature|idea|project
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

**Note:** Ideas notes are less structured than the above format. They typically contain brief thoughts followed by extensive wikilinks, serving as a holding place for proto-projects that aren't ready for formal project structure yet.

## Ideas Folder Philosophy

The **Ideas** folder (20 Ideas/) serves as a creative sandbox between permanent notes and projects:

- **Purpose**: Hold sprawling, multi-concept thoughts that aren't ready to become structured projects
- **Characteristics**:
  - Less structured than permanent notes (no "Why it matters" or "Evidence" sections required)
  - Often contain brief sentences followed by many wikilinks
  - May connect disparate permanent notes into proto-project patterns
  - More exploratory and associative than goal-oriented
- **When to use**:
  - Voice/fleeting note contains project-like thinking but lacks clear structure
  - Multiple permanent notes are connecting in interesting ways but no concrete project goals yet
  - Exploring relationships between concepts without committing to formal project
- **Progression**: When an Idea note gains clarity and specific goals, it graduates to Project notes (50 Project notes/)

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
- **Progressive elaboration**: Fleeting → Literature → Permanent → Ideas → Project → Output
