![Diane](./diane/assets/diane.jpg)

# Diane

**Your AI vault analyst and creative consultant for Obsidian**

Diane is a Claude Code plugin that transforms voice-captured notes into structured Zettelkasten notes with automatic linking and intelligent organization. Built for the Obsidian + Claude Code workflow, Diane reduces friction in your knowledge capture pipeline—from quick dictaphone-style voice memos "from the field" to polished, well-connected permanent notes.

## Features

- **Voice Note Processing** (`/playback`) - Transform SuperWhisper transcriptions into atomic, well-linked permanent notes
- **Semantic Link Discovery** (`/find-links`) - Find conceptual connections across your entire vault
- **Vault Analysis** (`/consult`) - Get insights and discover patterns in your knowledge graph
- **Intelligent Context Detection** - Resolves vague references by analyzing recent notes
- **Interactive Workflow** - One-by-one review with full editorial control

## Philosophy

Based on Sönke Ahrens' *How to Take Smart Notes*:

- **Reduce capture friction** - Voice → structured note in minimal steps
- **Build knowledge graph** - Every note connects to existing ideas
- **Atomic notes** - One idea, one note, many connections
- **Semantic over keyword** - Find conceptual relationships, not just text matches
- **Progressive elaboration** - Fleeting → Literature → Permanent → Project → Output

## Installation

### Prerequisites

1. [Claude Code](https://claude.com/code) installed
2. [Obsidian](https://obsidian.md/) with your vault configured
3. [Templater plugin](https://github.com/SilentVoid13/Templater) for Obsidian

### Quick Start

```bash
# Add the marketplace
/plugin marketplace add https://github.com/yourusername/diane

# Install the plugin
/plugin install diane@diane

# Configure your vault (IMPORTANT - Run this first!)
/diane:setup

# Then use the commands
/diane:playback
/diane:consult
/diane:find-links
```

**⚠️ Important**: Run `/diane:setup` immediately after installation to configure your Obsidian vault path and folder structure. The plugin won't work until you complete setup.

For detailed installation instructions, see [diane/README.md](./diane/README.md).

## SuperWhisper Setup for iOS

A big part of Diane's workflow is capturing quick voice notes "from the field"—thoughts while walking, reading, or working away from your desk. SuperWhisper on iOS makes this seamless, especially when configured with the iPhone's Action Button for instant recording.

### Installing SuperWhisper

1. Download [SuperWhisper](https://superwhisper.com/) from the App Store
2. Open SuperWhisper and grant microphone permissions
3. Configure Obsidian integration:
   - Open SuperWhisper settings
   - Go to **Integrations** → **Obsidian**
   - Set vault path to your iCloud Obsidian vault
   - Set target folder to `00 Diane/`
   - Enable **Auto-save to Obsidian**

### Configuring the Action Button (iPhone 15 Pro/Pro Max or later)

The Action Button provides instant voice capture—no unlocking, no app switching, just press and speak:

1. Open **Settings** → **Action Button**
2. Scroll to **Shortcut** and select it
3. Tap **Choose Shortcut**
4. Select **SuperWhisper** (or create a shortcut if needed):
   - Open **Shortcuts** app
   - Create new shortcut
   - Add action: **Open App** → **SuperWhisper**
   - Configure to start recording immediately (see SuperWhisper settings)
   - Name it "Voice Capture" or "Diane"
5. Test by pressing and holding the Action Button

**Pro tip**: In SuperWhisper settings, enable "Auto-start recording" so pressing the Action Button immediately begins recording without additional taps.

### File Naming Convention

Configure SuperWhisper to save files with timestamps:
- Format: `YYYY-MM-DD-HHmm.md` (e.g., `2025-10-13-0930.md`)
- This helps track when thoughts were captured and maintains chronological order
- Diane automatically recognizes this format during processing

### Workflow: Field to Vault

1. **In the field** - Press Action Button → speak → release
2. **Auto-save** - SuperWhisper transcribes and saves to `00 Diane/` in your Obsidian vault
3. **iCloud sync** - Note automatically syncs to your Mac
4. **Evening processing** - Run `/diane:playback` to transform voice notes into structured Zettelkasten notes

This creates a nearly frictionless path from fleeting thought to permanent knowledge.

## Commands

### `/diane:setup` - Initial Configuration

**Run this first!** Configure Diane with your Obsidian vault path and folder structure.

```bash
/diane:setup
```

The setup wizard will guide you through:
1. **Vault verification** - Locating and validating your Obsidian vault
2. **Folder configuration** - Using your existing structure or sensible defaults
3. **Naming conventions** - Choose your preferred file naming style (kebab-case, snake_case, etc.)
4. **Archive existing files** (optional) - Move any loose files in vault root to an Archive folder for clean organization
5. **Create folder structure** (optional) - Automatically create the complete Zettelkasten folder hierarchy:
   ```
   [Vault Name]/
   ├── 00 Diane/              # Voice note captures
   │   └── processed/         # Archived processed notes
   ├── 10 Fleeting notes/     # Quick captures
   ├── 20 Literature notes/   # Source insights
   ├── 30 Permanent notes/    # Atomic ideas
   ├── 40 Project notes/      # Active work
   ├── 99 Output/             # Published work
   └── _templates/            # Templater templates
   ```

The wizard ensures your vault is ready for a clean Zettelkasten workflow from voice capture to published output.

### `/diane:playback` - Process Voice Notes

Process voice-transcribed notes from your Diane folder into structured Zettelkasten notes.

```bash
/diane:playback
```

**What it does:**
1. Lists unprocessed voice notes with preview
2. Expands conversational transcriptions into clear prose
3. Finds semantic connections to existing notes
4. Suggests wikilinks and destination folders
5. Creates properly formatted atomic notes

### `/find-links` - Discover Connections

Find semantically related notes and suggest wikilinks.

```bash
/diane:find-links
```

**What it does:**
1. Analyzes note concepts and themes
2. Searches vault for related notes (semantic, not keyword matching)
3. Ranks connections by confidence level
4. Provides ready-to-paste wikilinks
5. Identifies conceptual clusters and gaps

### `/consult` - Vault Analysis

Consult with Diane to analyze patterns and discover creative connections.

```bash
/diane:consult
```

**What it does:**
1. Reads your Zettelkasten notes
2. Identifies patterns and themes
3. Suggests unexpected connections
4. Provides fresh perspective on your thinking

## Vault Structure

```
Slip Box/
├── 00 Diane/              # Voice note captures
│   └── processed/         # Archived notes
├── 10 Fleeting notes/     # Quick captures
├── 20 Literature notes/   # Source insights
├── 30 Permanent notes/    # Atomic ideas
├── 40 Project notes/      # Active work
├── 99 Output/             # Published work
└── _templates/            # Templater templates
```

## Daily Workflow

The Diane workflow embraces capturing thoughts exactly when and where they occur—"from the field":

1. **Capture** - Throughout the day, press Action Button to record voice memos
   - Walking between meetings
   - Reading in a coffee shop
   - Lying in bed with a late-night insight
   - → SuperWhisper auto-saves to `00 Diane/`, syncs via iCloud

2. **Process** - Evening review with `/diane:playback`
   - Transform raw transcriptions into clear, atomic notes
   - Diane expands conversational speech into structured prose
   - Automatically finds connections to existing notes
   - Archive processed voice notes

3. **Connect** - Weekly review with `/diane:find-links` on recent notes
   - Discover semantic relationships across your vault
   - Strengthen your knowledge graph
   - Identify emerging patterns and themes

4. **Consult** - Ask `/diane:consult` for insights when exploring themes
   - "What patterns do you see in my permanent notes?"
   - "What should I work on next?"
   - "How has my thinking evolved on [topic]?"

5. **Create** - Build projects from connected permanent notes
   - Let your knowledge graph guide your creative work
   - `40 Project notes/` pulls from `30 Permanent notes/`
   - Publish refined work to `99 Output/`

This workflow reduces capture friction to nearly zero while maintaining rigorous knowledge management standards.

## Configuration

Configuration is handled through the `/diane:setup` wizard, which updates `diane/.claude-plugin/plugin.json` and all command files with your settings:
- Vault path
- Folder names
- Naming conventions
- Template locations

You can re-run `/diane:setup` anytime to reconfigure.

## Note Format

All notes use atomic Zettelkasten structure with frontmatter, main idea, significance, evidence, and wikilinks. See [templates](./diane/templates/) for examples.

## Contributing

This is a personal workflow plugin built for my specific Obsidian setup, but suggestions and improvements are welcome! Open an issue or submit a PR.

## License

MIT

## Acknowledgments

- Inspired by Sönke Ahrens' *How to Take Smart Notes*
- Built for Obsidian + Claude Code workflow
- Icon design inspired by Twin Peaks

---

**Version:** 1.0.0
**Author:** Marcus Estes
**Documentation:** [Full documentation](./diane/README.md)
