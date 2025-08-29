# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains a structured AI-assisted product development workflow system with four core phases:

1. **PRD Creation** (`create-prd.md`) - Generates Product Requirements Documents
2. **Task Generation** (`generate-tasks.md`) - Converts PRDs into actionable task lists
3. **Agent Creation** (`create-agent.md`) - Creates specialized sub-agents for each task domain
4. **Task Processing** (`process-task-list.md`) - Manages task execution and completion

## Workflow Architecture

### Phase 1: PRD Generation
- **Input**: User prompt for a new feature
- **Process**: Ask clarifying questions, then generate structured PRD
- **Output**: `prd-[feature-name].md` in `/tasks/` directory
- **Key Rule**: MUST ask clarifying questions before writing PRD

### Phase 2: Task List Generation  
- **Input**: Reference to existing PRD file
- **Process**: Two-phase approach with user confirmation between phases
- **Output**: `tasks-[prd-file-name].md` in `/tasks/` directory
- **Critical Flow**: Generate parent tasks → wait for "Go" → generate sub-tasks

### Phase 3: Agent Creation
- **Input**: Reference to existing task list file
- **Process**: Generate specialized sub-agents for each parent task domain
- **Output**: Multiple `.claude/agents/[feature-name]-[task-type]-agent.md` files
- **Key Rule**: Each agent follows Anthropic 2025 sub-agent best practices

### Phase 4: Task Execution
- **Process**: One sub-task at a time with user permission between tasks, using specialized agents
- **Status Tracking**: `[ ]` → `[o]` → `[x]` progression
- **Completion Protocol**: Test → Stage → Commit → Mark complete

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
└── /.claude/agents/        # Generated specialized sub-agents
    ├── [feature]-[type]-agent.md
    └── [feature]-[type]-agent.md
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