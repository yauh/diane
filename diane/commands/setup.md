---
description: Configure Diane plugin with your Obsidian vault path and folder structure
---

# Diane Setup Wizard

You are configuring the Diane plugin for a new user. This command will guide them through setting up their vault path and folder structure.

## Setup Process

Follow these steps to configure Diane:

### Step 1: Welcome & Explanation

Greet the user and explain what we'll configure:

```
Welcome to Diane plugin setup!

I'll help you configure Diane to work with your Obsidian vault. I need to know:

1. The path to your Obsidian vault
2. Your folder structure for notes (or we can use sensible defaults)
3. Your naming conventions

This should take about 2 minutes. Ready to begin? [Y/n]
```

### Step 2: Vault Path Discovery

Ask for the vault path:

```
What's the path to your Obsidian vault?

Common locations:
- macOS: /Users/[username]/Documents/[VaultName]
- macOS (iCloud): /Users/[username]/Library/Mobile Documents/iCloud~md~obsidian/Documents/[VaultName]
- Windows: C:\Users\[username]\Documents\[VaultName]
- Linux: /home/[username]/Documents/[VaultName]

Enter your vault path:
```

Once they provide a path, verify it exists using the Read tool to check for `.obsidian` folder:

```bash
# Try to read the .obsidian folder to verify this is a valid vault
```

If the path doesn't exist or isn't a valid Obsidian vault, ask them to try again.

### Step 3: Folder Structure

Ask about their folder organization:

```
Diane works with a Zettelkasten-style folder structure.

Do you already have folders for:
- Voice captures/inbox notes
- Fleeting notes (quick captures)
- Literature notes (reading notes)
- Permanent notes (developed atomic ideas)
- Project notes (active projects)

[Y] Yes, I'll specify my folders
[N] No, use default structure (recommended for new users)
```

**If Yes**: Ask for each folder name:
```
What folder do you use for:
1. Voice captures/inbox? (default: 00 Diane)
2. Fleeting notes? (default: 10 Fleeting notes)
3. Literature notes? (default: 20 Literature notes)
4. Permanent notes? (default: 30 Permanent notes)
5. Project notes? (default: 40 Project notes)
```

**If No**: Use defaults:
- Voice captures: `00 Diane`
- Fleeting notes: `10 Fleeting notes`
- Literature notes: `20 Literature notes`
- Permanent notes: `30 Permanent notes`
- Project notes: `40 Project notes`

### Step 4: Naming Conventions

Ask about naming style:

```
What naming convention do you use for files?

[1] kebab-case (my-note-name.md) - recommended
[2] snake_case (my_note_name.md)
[3] Camel Case (MyNoteName.md)
[4] Spaces (My Note Name.md)

Select [1-4]:
```

### Step 5: Archive Existing Files (Optional)

Before creating the folder structure, check if there are existing markdown files in the vault root:

```bash
# List .md files in vault root (not in subdirectories)
ls *.md 2>/dev/null
```

If there are existing files, ask the user:

```
I found existing markdown files in your vault root.

Would you like to move them to an Archive folder before setting up the new structure?
This will help keep your vault organized.

[Y] Yes, move existing files to Archive/
[N] No, leave them where they are
```

If yes, create Archive folder and move files:

```bash
mkdir -p "[vault_path]/Archive"
mv "[vault_path]"/*.md "[vault_path]/Archive/" 2>/dev/null
```

### Step 6: Create Folders

Check which folders are missing and offer to create them:

```
Checking your vault structure...

Missing folders:
- 00 Diane/
- 10 Fleeting notes/
- 30 Permanent notes/
- _templates/

Should I create these folders?

[Y] Yes, create missing folders
[N] No, I'll create them manually
```

If yes, create the complete folder structure:

```bash
mkdir -p "[vault_path]/00 Diane/processed"
mkdir -p "[vault_path]/10 Fleeting notes"
mkdir -p "[vault_path]/20 Literature notes"
mkdir -p "[vault_path]/30 Permanent notes"
mkdir -p "[vault_path]/40 Project notes"
mkdir -p "[vault_path]/99 Output"
mkdir -p "[vault_path]/_templates"
```

Report success for each folder created.

### Step 7: Update Configuration File

Now update the plugin configuration:

**Read the current configuration** from `diane/.claude-plugin/plugin.json` and update it with the new settings using the Edit tool.

Update these fields in `plugin.json`:
- `configuration.vault_path` - Set to the validated vault path
- `configuration.diane_folder` - Set to the voice captures folder name (e.g., "00 Diane")
- `configuration.folders.fleeting` - Set to fleeting notes folder name
- `configuration.folders.literature` - Set to literature notes folder name
- `configuration.folders.permanent` - Set to permanent notes folder name
- `configuration.folders.project` - Set to project notes folder name
- `configuration.naming.style` - Set to the chosen naming convention

**IMPORTANT**: Do NOT modify any command files (playback.md, consult.md, find-links.md). They are designed to read configuration from plugin.json dynamically. This ensures the plugin works for all users without modification.

### Step 8: Confirmation

Show a summary and confirm setup is complete:

```
✓ Setup complete!

Configuration:
- Vault path: [path]
- Voice captures: [folder]
- Fleeting notes: [folder]
- Permanent notes: [folder]
- Literature notes: [folder]
- Project notes: [folder]
- Output: 99 Output/
- Templates: _templates/
- Naming style: [style]

Vault structure:
[Vault Name]/
├── 00 Diane/              # Voice note captures
│   └── processed/         # Archived processed notes
├── 10 Fleeting notes/     # Quick captures
├── 20 Literature notes/   # Source insights
├── 30 Permanent notes/    # Atomic ideas
├── 40 Project notes/      # Active work
├── 99 Output/             # Published work
└── _templates/            # Templater templates

You can now use Diane commands:
- /diane:playback - Process voice notes into structured Zettelkasten notes
- /diane:consult - Get vault insights and pattern analysis
- /diane:find-links - Discover semantic connections for any note

Ready to try /diane:playback? [Y/n]
```

If they say yes, run the playback command automatically.

## Error Handling

- **Invalid path**: Ask user to verify and try again
- **Permission errors**: Explain they may need to grant Claude Code access to the vault directory
- **Missing .obsidian folder**: Warn that this doesn't appear to be an Obsidian vault, ask if they want to continue anyway

## Important Notes

- ALWAYS verify the vault path exists before proceeding
- Check for existing files in vault root and offer to archive them before creating structure
- Create the complete folder structure including subfolders (e.g., `00 Diane/processed/`)
- Report success/failure for each folder created
- Use absolute paths, never relative paths
- After updating configuration, confirm by reading back the updated files
- Be friendly and encouraging throughout the setup process
- The folder structure ensures a clean Zettelkasten workflow from voice capture to published output

---

Begin by welcoming the user and explaining the setup process.
