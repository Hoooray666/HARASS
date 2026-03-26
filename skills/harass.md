---
name: harass
description: HARASS (Hooray Auto ReseArch SyStem) - Three-agent autonomous research pipeline from literature review to validated ideas
trigger: harass, HARASS, auto research, autonomous research, 自动科研
---

# HARASS: Hooray Auto ReseArch SyStem

Orchestrates a three-agent workflow for autonomous research: literature → analysis → ideas → validation.

## Architecture

- **Boss** (Opus 4.6): Strategic decisions, idea generation, user communication
- **Monitor** (Sonnet 4.6): Quality control, paper analysis, gap detection
- **Worker** (Sonnet 4.6): Execution, paper search, experiments

## Usage

```
/harass "your research topic"
```

## What It Does

1. **Literature Collection**: Worker searches and downloads papers
2. **Deep Analysis**: Monitor reviews each paper with structured rubric
3. **Gap Detection**: Monitor finds missing SOTA papers
4. **Idea Generation**: Boss synthesizes novel research directions
5. **Pilot Validation**: Worker runs GPU experiments
6. **Final Plan**: Boss selects best idea with experiment roadmap

## Output Structure

```
papers/
  index.json           # Paper metadata
  classification.json  # Same-target vs useful-other
  *.pdf               # Downloaded papers
analysis/
  {paper_id}.md       # Monitor's reviews
  missing_papers.md   # SOTA gaps
ideas/
  candidates.md       # Boss's 5 ideas
  final_plan.md       # Selected idea + roadmap
experiments/
  pilot_results.json  # GPU test results
state.json            # Workflow state
```

## Implementation

You are the **Boss agent** - the only one who talks to the user.

### Workflow

1. Create workspace structure
2. Spawn Worker → collect papers
3. Spawn Monitor → analyze papers + find gaps
4. Spawn Worker → fetch missing papers
5. Generate 5 ideas from analysis
6. Spawn Worker → run pilot experiments
7. Select best idea + create final plan

### Step 1: Initialize Workspace

Create project-specific folder with sanitized topic name:

```bash
# Create project folder: research_YYYYMMDD_sanitized_topic
PROJECT_NAME="research_$(date +%Y%m%d)_$(echo '{topic}' | tr ' ' '_' | tr -cd '[:alnum:]_-' | cut -c1-50)"
mkdir -p "$PROJECT_NAME"/{papers,analysis,ideas,experiments}
cd "$PROJECT_NAME"

# Initialize uv project
uv init --no-readme
uv add networkx numpy pandas matplotlib scipy

echo '{"phase": "init", "step": 0, "topic": "{topic}", "retries": {}}' > state.json
```

### Step 2: Spawn Worker for Paper Collection

Use Agent tool with worker agent:
- Prompt: "Search '{topic}' using research-lit. Download at least 15 papers covering:
  - Recent work (2024-2026): 8+ papers
  - Classical foundational work (2015-2023): 7+ papers
  Download to papers/. Create papers/index.json with title, venue, year, topic. Parse arXiv IDs for dates (YYMM.NNNNN)."
- Agent file: D:\Claude\research-workflow\agents\worker.md

### Step 3: Spawn Monitor for Analysis

Use Agent tool with monitor agent:
- Prompt: "Review all papers in papers/. Write analysis/{paper_id}.md using quality rubric with strict ratings. Create papers/classification.json. Create analysis/evolution.md tracking technical progression AND rate papers comparatively: identify which papers set new SOTA, flag papers with weak baselines, note which papers have strongest/weakest methodology. List missing SOTA papers in analysis/missing_papers.md."
- Agent file: D:\Claude\research-workflow\agents\monitor.md
### Step 4: Spawn Worker for Gap Filling

Use Agent tool with worker agent:
- Prompt: "Read analysis/missing_papers.md. Search and download missing papers. Update papers/index.json."
- Agent file: D:\Claude\research-workflow\agents\worker.md

### Step 5: Generate Ideas (You Do This)

1. Read all files in analysis/
2. Identify SOTA methods and limitations
3. **Identify newest/best baseline** - Find most recent comprehensive tool with largest dataset
4. Generate 5 concrete ideas addressing gaps
5. Check novelty with novelty-check skill
6. Write ideas/candidates.md
7. **ASK USER**: Present all 5 ideas and ask which ones to explore further (allow editing candidates.md in D:\Claude folder)

### Step 6: Spawn Worker for Pilots

Use Agent tool with worker agent:
- Prompt: "Read ideas/candidates.md. Deploy 30min-2hr GPU pilot tests for each idea. Write experiments/pilot_results.json."
- Agent file: D:\Claude\research-workflow\agents\worker.md

### Step 6.5: Experiment Bridge (You Do This)

**Phase 1.5: Implementation → Sanity Check → Deployment**

1. Read ideas/candidates.md (must contain 5+ ideas)
2. **Assess all ideas** for feasibility and impact:
   - Feasibility: implementation complexity, data availability, compute requirements
   - Impact: novelty, potential contribution, baseline comparison strength
3. **ASK USER**: Present assessment and ask which idea to implement (allow editing in D:\Claude folder)
4. Use experiment-plan skill to create detailed roadmap
5. **ASK USER**: Review experiment plan - which baseline to use? which dataset? (allow editing EXPERIMENT_PLAN.md)
6. Implement experiment code (Python/PyTorch):
   - Training scripts with argparse
   - Evaluation scripts with metrics
   - Data loaders
   - Fixed random seeds
7. Sanity check: Run smallest version locally to catch bugs
8. Deploy to GPU servers using run-experiment skill
9. Monitor with monitor-experiment skill
10. Write experiments/pilot_results.json with selection rationale

### Step 7: Final Plan (You Do This)

1. Read experiments/pilot_results.json
2. Select best idea based on results
3. **Compare against newest baseline** - Ensure proposed approach exceeds current SOTA metrics
4. Use experiment-plan skill for detailed roadmap
5. Write ideas/final_plan.md with baseline comparison
6. Report to user

## Error Handling

- Track retries in state.json
- If agent fails 3 times, you handle it directly
- Provide detailed instructions or alternative approach
