---
description: Process voice-captured notes from Diane and fleeting notes into structured Zettelkasten notes
---

# Diane Voice Note Processor

You are processing unstructured notes from two sources: (1) voice-transcribed notes captured via SuperWhisper in the Diane folder, and (2) underdeveloped fleeting notes. Your goal is to transform these into atomic Zettelkasten notes.

## Configuration

**Vault Path:** `/Users/marcusestes/Library/Mobile Documents/iCloud~md~obsidian/Documents/Slip Box`

**Folders:**
- Diane (voice captures): `00 Diane/`
- Fleeting notes: `10 Fleeting notes/`
- Literature notes: `20 Literature notes/`
- Permanent notes: `30 Permanent notes/`
- Project notes: `40 Project notes/`

**Naming Conventions:**
- Files: kebab-case (e.g., `ritual-interface-bridge.md`)
- Wikilinks: Display name format (e.g., `[[Ritual Interface Bridge]]`)

**Processing Mode:** Interactive (one note at a time)

## Workflow

### Step 1: List Unprocessed Notes

Scan **both** the `00 Diane/` folder and the `10 Fleeting notes/` folder for notes to process.

- **Diane notes:** Voice captures, typically named with timestamps like `2025-10-13-0930.md`
- **Fleeting notes:** Underdeveloped thoughts that need expansion into permanent/project notes

Present a numbered list of available notes with a preview of their content (first 100 characters) and indicate the source folder.

Example:
```
Found 5 notes to process:

From 00 Diane/:
[1] 2025-10-13-0930.md
    "Had that thought about how the ritual thing ties into..."

[2] 2025-10-13-1415.md
    "Read interesting article about emergence and how systems..."

From 10 Fleeting notes/:
[3] quick-thought-on-themes.md
    "Themes might work like Tarot cards, need to explore..."

[4] fm-ratio-insight.md
    "Noticed that 7:1 ratios create interesting tension..."

[5] vibe-coding-pattern.md
    "Pattern: viral apps need immediate payoff plus shareability..."

Select a note to process [1-5], or type 'all' to see all at once:
```

### Step 2: Read and Analyze Selected Note

Once the user selects a note, read its full content and:

1. **Expand and Clarify:** Transform the conversational, informal voice transcription into clear, structured prose. Fix transcription errors, complete incomplete thoughts, and make implicit ideas explicit.

2. **Identify Atomic Concepts:** Determine if this note contains:
   - One clear idea (create one note)
   - Multiple distinct ideas (suggest splitting into multiple notes)

3. **Detect Context:** Look for implicit references like "that thing I was thinking about" or "the article" by:
   - Checking recent literature notes (20 Literature notes/)
   - Checking active project notes (40 Project notes/)
   - Checking recent permanent notes (30 Permanent notes/)
   - Using semantic understanding to infer what the user is referencing

### Step 3: Find Connections

Search the entire vault for semantically related notes. Focus on:
- Conceptual relationships (not just keyword matches)
- Related permanent notes
- Relevant literature notes
- Active projects that might connect

Generate suggested wikilinks using display name format: `[[Ritual Interface Bridge]]`

### Step 4: Recommend Destination

Determine the appropriate folder based on the note's maturity:

- **Fleeting (10 Fleeting notes/):** Quick captures, underdeveloped thoughts, need more processing
- **Literature (20 Literature notes/):** References to books, articles, sources, quotes
- **Permanent (30 Permanent notes/):** Well-developed atomic ideas, clear insights, original thinking
- **Project (40 Project notes/):** Goal-oriented work, specific outputs, project planning

### Step 5: Generate Preview

Show the user a structured preview using the atomic Zettelkasten format:

```markdown
Original voice note:
"[original transcription]"

---

Expanded & clarified:

# [Suggested Title in Title Case]

## Idea

[1-3 sentence clear articulation of the core concept]

## Why it matters

[Why this idea is significant, what it connects to, why it's worth keeping]

## Evidence / References

- [Any sources mentioned or implicit references]
- [Related reading or prior thoughts]

## Links

[[Related Note 1]] [[Related Note 2]] [[Related Note 3]]

---

Suggested destination: [folder name]
Suggested filename: [kebab-case-name].md

[A]pprove  [E]dit  [S]kip  [C]ancel
```

### Step 6: Handle User Response

- **Approve (A):** Create the note in the suggested location with the expanded content. Archive the original note (see Step 7).
- **Edit (E):** Allow the user to modify title, content, destination, or links before creating.
- **Skip (S):** Leave the note in its current folder, move to next note.
- **Cancel (C):** Stop processing, return to main list.

### Step 7: Create Note

When creating the note:

1. Create file in destination folder with kebab-case filename
2. Use the appropriate Zettelkasten structure:
   - Frontmatter with created date, type, tags
   - # Title
   - ## Idea
   - ## Why it matters
   - ## Evidence / References
   - ## Links
3. Ensure wikilinks use display name format
4. After successful creation, archive the original note:
   - **Diane notes (`00 Diane/`):** Move to `00 Diane/processed/` (create folder if needed)
   - **Fleeting notes (`10 Fleeting notes/`):** Move to `10 Fleeting notes/processed/` (create folder if needed)

## Special Handling

### Voice Transcription Cleanup
- Fix common transcription errors (their/there, its/it's)
- Add proper punctuation
- Break into logical paragraphs
- Clarify unclear proper nouns

### Multi-Concept Notes
If a single voice note contains multiple distinct ideas:
```
This voice note contains 3 distinct concepts:

1. Ritual as interface bridge
2. Digital sacred practices
3. Project idea for cyber-religion essay

Process them as:
[A] Separate permanent notes
[B] One fleeting note to split later
[C] Let me decide one-by-one
```

### Literature References
If the voice note mentions reading something:
```
Detected literature reference: "article about emergence"

Found possible matches:
1. Johnson_Emergence.md (read 3 days ago) - HIGH CONFIDENCE
2. Alexander_Pattern_Language.md (mentions emergence) - MEDIUM

Link to: [1/2/Skip]
```

### Project Ideas
If the voice note is about a project:
```
This appears to be a project-oriented note.

Create as:
[P] New project note in 40 Project notes/
[F] Fleeting note to develop later
[A] Add to existing project: [list active projects]
```

## Best Practices

1. **Preserve Voice:** Maintain the user's writing style and perspective in expanded notes
2. **Stay Atomic:** One clear idea per permanent note
3. **Link Generously:** Suggest connections even if not obvious
4. **Context Matters:** Use temporal and semantic context to resolve ambiguous references
5. **Quality Over Speed:** Better to create one excellent note than three mediocre ones

## Example Transformation

**Voice note (2025-10-13-0930.md):**
> "Had that thought about how the ritual thing ties into what I was reading about interfaces, you know the cyber religion stuff, maybe this is the bridge between those two things I've been working on"

**Processed output:**

```markdown
---
created: 2025-10-13 14:30
modified: 2025-10-13 14:30
type: permanent
tags: [ritual, interface, cyber-religion]
---

# Ritual as Interface Bridge

## Idea

Rituals function as interfaces between human intention and digital systems. They provide a structured protocol for interaction, similar to how religious rituals mediate between human and divine realms.

## Why it matters

This concept bridges two separate research threads: interface theory and cyber-religion. It suggests that digital rituals aren't merely metaphorical but are actual interface designs that structure human-computer interaction.

## Evidence / References

- Connected to interface theory work from Johnson's "Emergence"
- Relates to ongoing cyber-religion essay project
- Builds on prior thinking about ritual as structured practice

## Links

[[Ritual as Interface]] [[Cyber Religion Essay]] [[Interface Theory Johnson]] [[Digital Sacred Practices]]
```

**Destination:** `30 Permanent notes/ritual-as-interface-bridge.md`

---

Begin by scanning both the `00 Diane/` and `10 Fleeting notes/` folders and listing all available notes to process.
