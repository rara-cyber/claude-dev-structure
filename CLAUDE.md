# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## AI Guidance

* Ignore GEMINI.md and GEMINI-*.md files
* **Memory-Bank-Synchronizer Integration**: Use the memory-bank-synchronizer agent proactively at key workflow points to ensure memory bank accuracy and system knowledge consistency
* To save main context space, for code searches, inspections, troubleshooting or analysis, use code-searcher subagent where appropriate - giving the subagent full context background for the task(s) you assign it.
* After receiving tool results, carefully reflect on their quality and determine optimal next steps before proceeding. Use your thinking to plan and iterate based on this new information, and then take the best next action.
* For maximum efficiency, whenever you need to perform multiple independent operations, invoke all relevant tools simultaneously rather than sequentially.
* Before you finish, please verify your solution
* Do what has been asked; nothing more, nothing less.
* NEVER create files unless they're absolutely necessary for achieving your goal.
* ALWAYS prefer editing an existing file to creating a new one.
* NEVER proactively create documentation files (*.md) or README files. Only create documentation files if explicitly requested by the User.
* When you update or modify core context files, also update markdown documentation and memory bank using memory-bank-synchronizer
* When asked to commit changes, exclude CLAUDE.md and CLAUDE-*.md referenced memory bank system files from any commits. Never delete these files.

## Memory Bank System

This project uses a structured memory bank system with specialized context files. Always check these files for relevant information before starting work:

### Core Context Files

* **CLAUDE-activeContext.md** - Current session state, goals, and progress (if exists)
* **CLAUDE-patterns.md** - Established code patterns and conventions (if exists)
* **CLAUDE-decisions.md** - Architecture decisions and rationale (if exists)
* **CLAUDE-troubleshooting.md** - Common issues and proven solutions (if exists)
* **CLAUDE-config-variables.md** - Configuration variables reference (if exists)
* **CLAUDE-temp.md** - Temporary scratch pad (only read when referenced)

**Important:** Always reference the active context file first to understand what's currently being worked on and maintain session continuity.

### Memory Bank Synchronization

The memory bank system is **actively synchronized** using the memory-bank-synchronizer agent:
- **Automatic Updates**: Hooks trigger synchronization at key workflow points (session start, agent completion, knowledge organization)
- **Pattern Validation**: Ensures documented patterns match actual implementation
- **Decision Tracking**: Updates architectural decisions with current outcomes
- **Context Continuity**: Maintains accurate session state across interactions
- **Proactive Usage**: Use memory-bank-synchronizer explicitly at major workflow transitions

### Memory Bank System Backups

When asked to backup Memory Bank System files, you will copy the core context files above and @.claude settings directory to directory @/path/to/backup-directory. If files already exist in the backup directory, you will overwrite them.

## Project Overview

This repository contains a structured AI-assisted product development workflow system with five core phases:

1. **PRD Creation** (`create-prd.md`) - Generates Product Requirements Documents
2. **Task Generation** (`generate-tasks.md`) - Converts PRDs into actionable task lists
3. **Agent Creation** (`create-agent.md`) - Creates specialized sub-agents for each task domain
4. **Task Processing** (`process-task-list.md`) - Manages task execution and completion
5. **Knowledge Capture** (hooks + `/knowledge/`) - Automatic documentation via `/compact` summaries

## Workflow Architecture

### Phase 1: PRD Generation
- **Input**: User prompt for a new feature
- **Process**: Validate context with memory-bank-synchronizer → Ask clarifying questions → Generate structured PRD → Update memory bank
- **Output**: `prd-[feature-name].md` in `/tasks/` directory
- **Key Rule**: MUST ask clarifying questions before writing PRD
- **Memory Integration**: Use memory-bank-synchronizer for context validation and pattern updates

### Phase 2: Task List Generation  
- **Input**: Reference to existing PRD file
- **Process**: Sync memory bank → Analyze PRD → Generate parent tasks → wait for "Go" → Generate sub-tasks → Update memory bank
- **Output**: `tasks-[prd-file-name].md` in `/tasks/` directory
- **Critical Flow**: Generate parent tasks → wait for "Go" → generate sub-tasks
- **Memory Integration**: Pre-generation sync and post-generation pattern updates

### Phase 3: Agent Creation
- **Input**: Reference to existing task list file
- **Process**: Sync memory bank → Analyze tasks → Generate specialized sub-agents → Update memory bank
- **Output**: Multiple `.claude/agents/[feature-name]-[task-type]-agent.md` files
- **Key Rule**: Each agent follows Anthropic 2025 sub-agent best practices
- **Memory Integration**: Pattern validation and agent coordination documentation

### Phase 4: Task Execution
- **Process**: Sync memory bank → Execute tasks with specialized agents → Update patterns and decisions
- **Status Tracking**: `[ ]` → `[o]` → `[x]` progression
- **Completion Protocol**: Test → Stage → Commit → Mark complete
- **Memory Integration**: Pre-execution validation and post-execution pattern updates

### Phase 5: Knowledge Capture
- **Trigger**: SubagentStop hooks automatically execute after agent completion
- **Process**: `/compact` → organize summaries → memory-bank-synchronizer updates patterns
- **Storage**: Timestamped files in `/knowledge/` directory
- **Organization**: Helper scripts sort by feature and agent type, then sync memory bank
- **Output**: Structured knowledge base with cross-reference index and updated memory bank
- **Memory Integration**: Automatic pattern extraction and memory bank synchronization

## File Structure and Conventions

### Directory Structure
```
/
├── create-prd.md           # PRD generation rules
├── generate-tasks.md       # Task generation rules
├── create-agent.md         # Agent creation rules
├── process-task-list.md    # Task execution rules
├── /tasks/                 # Generated PRDs and task lists
│   ├── prd-[feature].md
│   └── tasks-prd-[feature].md
├── /.claude/               # Claude Code configuration
│   ├── settings.json       # Hooks configuration for auto-documentation
│   └── agents/             # Generated specialized sub-agents
│       ├── [feature]-[type]-agent.md
│       └── [feature]-[type]-agent.md
├── /knowledge/             # Automatic documentation knowledge base
│   ├── README.md           # Knowledge base documentation
│   ├── [timestamp]-agent-summary.md  # Raw compact summaries (temporary)
│   └── organized/          # Organized summaries by feature/agent
│       ├── [feature]/
│       │   ├── [agent-type]-summary-001.md
│       │   └── [agent-type]-summary-002.md
│       └── index.md        # Cross-reference index
└── /scripts/               # Helper automation scripts
    └── organize-agent-summary.sh  # Organizes raw summaries
```

### Task Status Indicators
- `[ ]` - Not started
- `[o]` - Currently in progress (ongoing)  
- `[x]` - Completed

## Critical Implementation Rules

### PRD Generation (`create-prd.md`)
1. **Always ask clarifying questions first** - provide lettered/numbered options for easy selection
2. Target audience: Junior developers
3. Include all required sections: Goals, User Stories, Functional Requirements, Non-Goals, etc.
4. Save as `prd-[feature-name].md` in `/tasks/`

### Task Generation (`generate-tasks.md`) 
1. **Two-phase process**: Parent tasks first, then wait for "Go" confirmation
2. Analyze existing codebase for relevant patterns and components
3. Include "Relevant Files" section with file paths and descriptions
4. Use numbered task format (1.0, 1.1, 1.2, etc.)

### Agent Creation (`create-agent.md`)
1. **Input**: Reference to existing `tasks-[prd-file-name].md` file
2. **Generate specialized agents**: One per parent task domain (UI, API, testing, etc.)
3. **Follow Anthropic best practices**: YAML frontmatter, focused system prompts, appropriate tool permissions
4. **Save format**: `/.claude/agents/[feature-name]-[task-type]-agent.md`

### Task Execution (`process-task-list.md`)
1. **One sub-task at a time** - MUST ask user permission between sub-tasks
2. **Status updates**: Mark `[o]` when starting, `[x]` when complete
3. **Parent task completion protocol**:
   - Run full test suite
   - If tests pass: stage changes (`git add .`)
   - Clean up temporary files
   - Commit with conventional format and `-m` flags
   - Mark parent task complete
4. Update "Relevant Files" section as files are created/modified

## Testing and Commits

### Testing
- Use appropriate test command for the project stack
- All tests MUST pass before committing
- Test files should be alongside source files

### Commit Format
Use conventional commits with multiple `-m` flags:
```bash
git commit -m "feat: add payment validation logic" -m "- Validates card type and expiry" -m "- Adds unit tests for edge cases" -m "Related to T123 in PRD"
```

## Interaction Patterns

### PRD Phase
1. Receive user prompt
2. Ask clarifying questions (with lettered options)
3. Generate and save PRD based on answers

### Task Phase  
1. Receive PRD reference
2. Generate parent tasks only
3. Wait for "Go" confirmation
4. Generate detailed sub-tasks and relevant files

### Agent Phase
1. Receive task list reference
2. Analyze parent tasks and their domains
3. Generate specialized sub-agents for each task type
4. Save agents with proper YAML frontmatter and system prompts

### Execution Phase
1. Check next sub-task in sequence
2. Mark as `[o]` and implement using appropriate specialized agent
3. Mark as `[x]` when complete  
4. Ask user permission before next sub-task
5. Follow completion protocol for parent tasks

## Key Constraints

- PRDs target junior developer comprehension
- Task execution requires explicit user approval between sub-tasks
- All parent task completions require passing tests and proper commits  
- Files are organized in `/tasks/` directory and `/.claude/agents/` with specific naming conventions
- Two-phase task generation prevents overwhelming detail upfront
- Agent creation follows Anthropic 2025 sub-agent best practices with focused, single-responsibility agents