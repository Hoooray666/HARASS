# HARASS: Hooray Auto ReseArch SyStem

Autonomous research pipeline for literature review, idea generation, and validation using a three-agent architecture.

## Overview

HARASS orchestrates Worker, Monitor, and Boss agents to automate the research workflow from paper collection to validated research ideas with pilot experiments.

## Architecture

- **Boss** (Opus 4.6): Strategic decisions, idea generation, user communication
- **Monitor** (Sonnet 4.6): Critical quality control, paper analysis, gap detection
- **Worker** (Sonnet 4.6): Execution, paper search, experiments

## Features

- 📚 Automated literature collection (15+ papers)
- 🔍 Critical paper analysis with strict quality ratings
- 💡 Novel idea generation addressing research gaps
- ✅ Novelty checking via web search
- 🧪 Pilot experiment validation
- 📊 Comparative SOTA analysis

## Installation

1. Copy skills to your Claude skills directory:
```bash
cp -r skills/* ~/.claude/skills/
```

2. Copy agent definitions:
```bash
cp -r agents/* /path/to/your/research-workflow/agents/
```

3. Install dependencies:
```bash
uv add networkx numpy pandas matplotlib scipy
```

## Usage

### Full Pipeline
```
/harass "your research topic"
```

### Phase-by-Phase

**Phase 1: Literature Review**
```
/harass-lit "your research topic"
```

**Phase 2: Idea Generation**
```
/harass-ideas
```

**Phase 3: Experiment Validation**
```
/harass-experiment
```

## Output Structure

```
research_YYYYMMDD_topic/
├── papers/
│   ├── index.json           # Paper metadata
│   ├── classification.json  # Same-target vs useful-other
│   └── *.pdf               # Downloaded papers
├── analysis/
│   ├── {paper_id}.md       # Critical reviews
│   ├── evolution.md        # Technical progression + comparative ratings
│   └── missing_papers.md   # SOTA gaps
├── ideas/
│   ├── candidates.md       # 5 research ideas
│   ├── novelty_check.md    # Novelty verification
│   └── final_plan.md       # Selected idea + roadmap
├── experiments/
│   └── pilot_results.json  # Validation results
└── state.json              # Workflow state
```

## Key Features

### Strict Critical Review
The Monitor agent uses harsh but fair critique:
- Most papers rated 2-3/5 (not 4-5/5)
- Challenges weak baselines and insufficient evaluation
- Comparative analysis identifying SOTA vs incremental work
- Flags methodological issues explicitly

### Project Management
- Creates timestamped project folders
- Uses `uv` for dependency management
- Isolated environments per research topic

## Requirements

- Claude Code CLI or compatible environment
- Python 3.8+
- uv package manager
- Internet access for paper search

## License

MIT

## Contributing

Contributions welcome! Please open issues or PRs.
