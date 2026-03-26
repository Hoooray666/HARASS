---
name: harass-ideas
description: HARASS Phase 2 - Generate and validate research ideas from literature analysis
trigger: harass-ideas, generate ideas, 生成idea
---

# HARASS Phase 2: Idea Generation

Generates 5 research ideas from analyzed papers and validates novelty.

## Usage
```
/harass-ideas
```

## Prerequisites
Must have completed /harass-lit (requires analysis/ folder)

## What It Does
1. Reads analysis files
2. Generates 5 novel ideas addressing gaps
3. Checks novelty via web search
4. Creates candidates.md

## Output
- `ideas/candidates.md` - 5 research ideas
- `ideas/novelty_check.md` - Novelty verification

## Implementation

### Step 1: Generate Ideas (Boss Does This)
1. Read all files in analysis/
2. Identify SOTA methods and limitations
3. **Identify newest/best baseline** - Find most recent comprehensive tool with largest dataset
4. Generate 5 concrete ideas addressing gaps
5. Check novelty with novelty-check skill
6. Write ideas/candidates.md
7. **ASK USER**: Present all 5 ideas. Ask which ones to explore further. Allow editing ideas/candidates.md in D:\Claude folder.

### Step 2: Report
Tell user: "Ideas generated in D:\Claude\{project_folder}/ideas/. **Review and edit candidates.md if needed.** Run /harass-experiment when ready."
