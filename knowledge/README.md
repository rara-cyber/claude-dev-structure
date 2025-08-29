# Knowledge Base - Agent Summaries

This directory contains automatically generated summaries from Claude Code agents completing their tasks.

## Structure

```
/knowledge/
├── README.md                    # This file
├── [timestamp]-agent-summary.md # Raw compact outputs from hooks
└── organized/                   # Organized summaries by feature/agent
    ├── [feature-name]/
    │   ├── [agent-type]-summary-001.md
    │   ├── [agent-type]-summary-002.md
    │   └── ...
    └── index.md                 # Cross-reference index
```

## How It Works

1. **Automatic Generation**: When a Claude Code subagent completes a task, hooks automatically:
   - Execute `/compact` command 
   - Save output to timestamped file in this directory
   - Run organization script to sort by feature/agent type

2. **Organization**: The `organize-agent-summary.sh` script processes raw summaries:
   - Extracts feature name and agent type from content
   - Moves to appropriate subdirectory
   - Updates cross-reference index
   - Maintains chronological numbering

## File Naming Conventions

### Raw Files (auto-generated)
- Format: `YYYYMMDD-HHMMSS-agent-summary.md`
- Example: `20250129-143022-agent-summary.md`

### Organized Files  
- Format: `[agent-type]-summary-[number].md`
- Example: `ui-agent-summary-001.md`

## Usage

- **Read summaries** to understand what agents accomplished
- **Reference decisions** made during implementation  
- **Track patterns** across similar agent tasks
- **Maintain context** for future development

## Integration with Workflow

This knowledge base integrates with the 4-phase development workflow:
1. PRD Creation → Task Generation → Agent Creation → **Task Execution + Knowledge Capture**

Each agent completion automatically contributes to the project's institutional knowledge.