---
name: research-workflow-phase1
description: Three-agent literature review and idea generation workflow
---

# Phase 1: Literature Review and Idea Generation

You are orchestrating a three-agent research workflow.

## Agents

- **Worker** (Sonnet 4.6): Executes tasks, has all skills
- **Monitor** (Sonnet 4.6): Reviews quality, enforces thresholds
- **Boss** (Opus 4.6): Strategic decisions, talks to user

## Workflow

### Step 1-2: Paper Collection (Worker)
Invoke worker agent with:
```
Search and download papers on "{topic}".
Use research-lit skill.
Output: papers/index.json with [title, venue, year, topic]
```

### Step 3: Paper Analysis (Monitor)
Invoke monitor agent with:
```
Read papers from papers/
Answer 7 questions per paper (see agents/monitor.md)
Output: analysis/{paper_id}.md
Classify: papers/classification.json
```

### Step 3b: SOTA Gap Check (Monitor)
```
Find SOTA paper, read related work
List missing papers: analysis/missing_papers.md
```

### Step 4: Re-search (Worker)
```
Read analysis/missing_papers.md
Search and download missing papers
Update papers/index.json
```

### Step 5: Idea Generation (Boss)
Invoke boss agent with:
```
Read analysis/
Identify gaps and limitations
Generate 5 concrete ideas
Check novelty
Output: ideas/candidates.md
```

### Step 6: Pilot Tests (Worker)
```
Read ideas/candidates.md
Deploy 30min-2hr GPU tests in parallel
Output: experiments/pilot_results.json
```

### Step 7: Final Plan (Boss)
```
Review experiments/pilot_results.json
Select best idea
Create claim-driven experiment roadmap
Output: ideas/final_plan.md
```

## Retry Logic

Track in state.json. If task fails:
- Retry 1-2: Monitor provides feedback
- Retry 3: Escalate to boss

## Usage

```
/research-workflow-phase1 "your research topic"
```
