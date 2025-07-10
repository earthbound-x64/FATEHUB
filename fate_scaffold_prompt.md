# Objective
Produce a Markdown-only scaffolding system for a Fate RPG campaign that is:

1. **Fractal-ready** – any object can gain aspects, mechanics, and children.  
2. **Indented / Hierarchical** – each parent directory holds its own `_meta.md` plus sub-folders.  
3. **Dial-configurable** – core game parameters live in `/game/settings.md`.  
4. **Automation-friendly** – global index, naming rules, validation stub, timeline files, scene subfolders, dial presets, AI prompt templates, and commit-driven changelog updates.

> **No setting-specific names – use only `{{placeholders}}`.  
> Deliver folder tree, README, settings.md, index.md, naming_rules.md, validation script stub, timeline templates, and all `_meta.md` skeletons in *one* code block.  
> No prose beyond comments.**

# Folder Tree (with comments)

```text
📁 fate_docs/
├── README.md                     # Overview & auto-generated changelog
├── index.md                      # Master list of every id → relative path
├── naming_rules.md               # Slug guidelines (lowercase, snake_case, no spaces)
├── game/
│   ├── settings.md               # Core Fate “dials” (placeholders)
│   ├── dial_presets/             # Alternate tone/genre presets
│   │   └── pulp_adventure.md
│   ├── current_issues.md
│   ├── impending_issues.md
│   └── faces_and_places.md
├── prompts/                      # Example AI prompts for each template
│   └── generate_character.txt
├── scripts/
│   ├── validate.py               # Stub: scans for missing keys, bad parents, dup IDs
│   └── update_log.py             # Stub: appends git commit summaries to README log
└── worlds/
    └── {{world_id}}/
        ├── _meta.md
        ├── _timeline.md          # Major historical events
        ├── scenes/               # World-level encounter templates
        │   └── {{scene_id}}/_scene_meta.md
        ├── _characters/
        │   └── {{character_id}}/_character_meta.md
        ├── _items/
        │   └── {{item_id}}/_meta.md
        ├── _gear/
        │   └── {{gear_id}}/_meta.md
        ├── regions/
        │   └── {{region_id}}/
        │       ├── _meta.md
        │       ├── _timeline.md
        │       ├── scenes/
        │       │   └── {{scene_id}}/_scene_meta.md
        │       ├── _characters/ …
        │       ├── _items/ …
        │       ├── _gear/ …
        │       ├── locations/
        │       │   └── {{location_id}}/
        │       │       ├── _meta.md
        │       │       ├── scenes/
        │       │       │   └── {{scene_id}}/_scene_meta.md
        │       │       ├── _characters/ …
        │       │       ├── _items/ …
        │       │       ├── _gear/ …
        │       │       └── zones/
        │       │           └── {{zone_id}}/_meta.md
        │       └── factions/
        │           └── {{faction_id}}/…   # same child structure
        └── factions/                       # World-wide organizations
            └── {{faction_id}}/…            # same child structure
```

# README.md template (changelog block auto-filled by `scripts/update_log.py`)
```markdown
# Fate RPG World Scaffolding

Generic, fractal-ready Markdown structure for Fate campaigns.  
All files use `{{placeholders}}` for AI injection.

## Design Principles
- Indented hierarchy encodes lineage (world → region → location → zone).
- One `_meta.md` per folder for portability.
- Game dials live in `/game/settings.md`.
- Validation and changelog scripts automate consistency.

## Update Log
<!-- `scripts/update_log.py` appends commit summaries here -->
```

# `/game/settings.md` (dial placeholders)
```markdown
# Fate Game Dials
- Character Aspects: {{5}}
- Phases: {{3}}
- Skill Cap: {{Great (+4)}}
- Skill Structure: {{Pyramid | Columns}}
- Initial Refresh: {{3}}
- Free Stunts: {{3}}
- Stress Tracks: {{Physical, Mental}}
- Default Stress Boxes: {{2}}
- Default Consequences: {{2/4/6}}
```

# Global `index.md` (example header)
```markdown
# Master Index
| id | type | path |
|----|------|------|
| {{world_id}} | world | worlds/{{world_id}}/_meta.md |
| … | … | … |
```

# `naming_rules.md`
```markdown
## Slug Conventions
- Lowercase ASCII only
- Use snake_case
- No spaces or special characters
- Unique across entire repository
```

# `scripts/validate.py` (stub)
```python
"""
Validate scaffold:
1. Ensure every _meta.md has required front-matter keys.
2. Check that parent IDs exist.
3. Detect duplicate ids.
"""
# Implementation to be completed.
```

# `scripts/update_log.py` (stub)
```python
"""
Append latest git commit message + date to README Update Log.
"""
# Implementation to be completed.
```

# Universal `_meta.md`
```markdown
---
id: {{object_id}}
type: {{type}}              # world | region | location | zone | faction | character | item | gear
name: {{object_name}}
parent: {{parent_id}}
tags: [{{tag1}}, {{tag2}}]
fractal: true
---

# {{object_name}}

## Description
{{brief_overview}}

## Aspects
- {{aspect_1}}
- {{aspect_2}}
- {{aspect_3}}

## Traits / Properties
{{custom_fields_for_this_type}}

## Mechanics (optional)
- Skills: {{skill_list}}
- Stunts: {{stunt_list}}
- Stress / Consequences: {{track_or_slots}}

## Fractal Children
- ./{{subfolder}}/{{child_id}}/_meta.md

## Notes
{{design_notes}}
```

# Character `_character_meta.md`
```markdown
---
id: {{character_id}}          # unique slug (e.g., lara_croft)
type: character
name: {{character_name}}
parent: {{parent_id}}         # region, faction, or location ID
role: {{pc_or_npc}}           # PC | NPC
refresh: {{3}}
phase:
  one_adventure:     {{phase_one_title}}
  two_crossing_paths: {{phase_two_title}}
  three_crossing_again: {{phase_three_title}}
tags: [{{tag1}}, {{tag2}}]
fractal: true
---

# {{character_name}}

## High Concept & Trouble
- **High Concept:** {{high_concept}}
- **Trouble:** {{trouble_aspect}}

## Additional Aspects
- {{aspect_3}}
- {{aspect_4}}
- {{aspect_5}}

## Skills Pyramid
- +4 **Great:**   {{skill_Great}}
- +3 **Good:**    {{skill_Good_1}}, {{skill_Good_2}}
- +2 **Fair:**    {{skill_Fair_1}}, {{skill_Fair_2}}, {{skill_Fair_3}}
- +1 **Average:** {{skill_Avg_1}}, {{skill_Avg_2}}, {{skill_Avg_3}}, {{skill_Avg_4}}

## Stunts
1. {{stunt_1}}
2. {{stunt_2}}
3. {{stunt_3}}
4. {{stunt_4}}  <!-- costs 1 refresh beyond free limit -->

## Stress & Consequences
| Track    | Boxes | Notes |
|----------|-------|-------|
| Physical | ❏❏    | + extra if Physique ≥ Fair |
| Mental   | ❏❏    | + extra if Will ≥ Fair     |
- Mild (2): ___ • Moderate (4): ___ • Severe (6): ___

## Gear / Extras
- {{gear_item_1}}
- {{gear_item_2}}

## Relationships & Hooks
- {{relationship_1}} – {{relationship_blurb}}
- {{plot_hook_1}}

## Fractal Children
- ./_items/{{item_id}}/_meta.md
- ./_gear/{{gear_id}}/_meta.md

## Notes
{{gm_notes}}
```

# Scene `_scene_meta.md`
```markdown
---
id: {{scene_id}}
type: scene
name: {{scene_title}}
parent: {{parent_id}}
tags: [{{tag1}}]
fractal: true
---

# {{scene_title}}

## Setup
{{environment_aspects}}

## Stakes
{{why_it_matters}}

## NPCs / Opponents
- {{npc_id}} – role / goal

## Expected Outcomes
{{possible_resolutions}}

## Notes
{{gm_notes}}
```

# Timeline `_timeline.md`
```markdown
# Timeline – {{object_name}}

| Year | Event |
|------|-------|
| {{year}} | {{event_description}} |
```

# Prompts example (`prompts/generate_character.txt`)
```
Create a Fate character file using the `_character_meta.md` template.
High-fantasy genre. Return Markdown front-matter and sections only.
```

# Validation
Ensure every template reflects the Fate Fractal, uses {{placeholder}} tokens, and no setting-specific names appear.
