---
name: harass-experiment
description: HARASS Phase 3 - Run pilot experiments and create final plan
trigger: harass-experiment, run pilots, 跑实验
---

# HARASS Phase 3: Experiment Validation

Assesses ideas, runs pilot experiments, creates final plan.

## Usage
```
/harass-experiment
```

## Prerequisites
Must have ideas/candidates.md from /harass-ideas

## What It Does
1. Assesses all ideas for feasibility and impact
2. Selects best idea
3. Implements and runs pilot experiment
4. Creates detailed experiment plan
5. Writes final_plan.md

## Output
- `experiments/` - Pilot code + results
- `ideas/EXPERIMENT_PLAN.md` - Detailed roadmap
- `ideas/final_plan.md` - Selected idea + rationale

## Implementation

### Step 1: Assess Ideas (Boss Does This)
1. Read ideas/candidates.md (must contain 5+ ideas)
2. **Assess all ideas** for feasibility and impact:
   - Feasibility: implementation complexity, data availability, compute requirements
   - Impact: novelty, potential contribution, baseline comparison strength
3. **Select best idea** balancing feasibility and meaningfulness

### Step 2: Implement Pilot
4. Implement minimal experiment code (Python)
5. Sanity check: Run smallest version locally to catch bugs
6. Write experiments/pilot_results.json with selection rationale

### Step 3: Create Final Plan
7. Use experiment-plan skill for detailed roadmap
8. Write ideas/final_plan.md with:
   - Selected idea rationale
   - **Compare against newest baseline** - Ensure proposed approach exceeds current SOTA
   - Experiment plan reference
9. Report to user
