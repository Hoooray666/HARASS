---
name: harass-lit
description: HARASS Phase 1 - Literature review and analysis only. Collects papers, analyzes them, identifies gaps.
trigger: harass-lit, literature review, 文献综述
---

# HARASS Phase 1: Literature Review

Collects and analyzes papers on a research topic.

## Usage
```
/harass-lit "your research topic"
```

## What It Does
1. Collects 15+ papers (recent + classical)
2. Analyzes each paper with quality rubric
3. Identifies technical evolution and gaps

## Output
- `papers/` - PDFs + index.json
- `analysis/` - Paper reviews + evolution.md + missing_papers.md

## Implementation

### Step 1: Check/Create Workspace
```bash
if [ ! -f state.json ]; then
  PROJECT_NAME="research_$(date +%Y%m%d)_$(echo '{topic}' | tr ' ' '_' | tr -cd '[:alnum:]_-' | cut -c1-50)"
  mkdir -p "$PROJECT_NAME"/{papers,analysis,ideas,experiments}
  cd "$PROJECT_NAME"
  uv init --no-readme
  uv add networkx numpy pandas matplotlib scipy
  echo '{"phase": "lit", "step": 1, "topic": "{topic}"}' > state.json
else
  echo "Resuming in existing project"
fi
```

### Step 2: Spawn Worker for Papers
Use Agent tool with worker agent:
- Prompt: "Search '{topic}' using research-lit. Download at least 15 papers covering recent (2024-2026) and classical (2015-2023) work. Download to papers/. Create papers/index.json with metadata."
- Agent file: D:\Claude\research-workflow\agents\worker.md

### Step 3: Spawn Monitor for Analysis
Use Agent tool with monitor agent:
- Prompt: "Review all papers in papers/. Write analysis/{paper_id}.md using quality rubric with strict ratings. Create papers/classification.json. Create analysis/evolution.md tracking technical progression AND rate papers comparatively: identify which papers set new SOTA, flag papers with weak baselines, note which papers have strongest/weakest methodology. List missing SOTA papers in analysis/missing_papers.md."
- Agent file: D:\Claude\research-workflow\agents\monitor.md

### Step 4: Spawn Worker for Gap Filling
Use Agent tool with worker agent:
- Prompt: "Read analysis/missing_papers.md. Search and download missing papers. Update papers/index.json."
- Agent file: D:\Claude\research-workflow\agents\worker.md

### Step 5: Report
Tell user: "Literature review complete. Files in D:\Claude\{project_folder}. **Please review analysis/evolution.md and analysis/missing_papers.md. You can edit any files before proceeding.** Run /harass-ideas when ready."
