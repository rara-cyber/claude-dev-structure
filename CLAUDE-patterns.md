# CLAUDE-patterns.md

## Established Code Patterns and Conventions

*This file is automatically maintained by the memory-bank-synchronizer agent to ensure patterns documented here match actual implementation reality.*

### Development Workflow Patterns

#### 5-Phase AI-Assisted Development Process
**Status:** Implemented and Active
**Last Verified:** Initial Setup

1. **PRD Creation** (`create-prd.md`)
   - Always ask clarifying questions before writing PRD
   - Target junior developer comprehension
   - Save as `prd-[feature-name].md` in `/tasks/`

2. **Task Generation** (`generate-tasks.md`)
   - Two-phase approach: Parent tasks → User confirmation → Sub-tasks
   - Analyze existing codebase for relevant patterns
   - Include "Relevant Files" section
   - Use numbered task format (1.0, 1.1, 1.2, etc.)

3. **Agent Creation** (`create-agent.md`)
   - One specialized agent per parent task domain
   - Color-coded agent system for visual identification
   - YAML frontmatter with focused system prompts
   - Tool permissions based on domain requirements

4. **Task Execution** (`process-task-list.md`)
   - One sub-task at a time with user permission
   - Status progression: `[ ]` → `[o]` → `[x]`
   - Dependency analysis for parallel execution
   - Synchronized testing and commits per execution group

5. **Knowledge Capture** (Automatic via hooks)
   - SubagentStop hooks trigger `/compact` command
   - Timestamped summaries in `/knowledge/`
   - Organization scripts sort by feature and agent type

#### Agent Coordination Patterns

**Parallel Execution Groups:**
- Independent tasks grouped for simultaneous processing
- Color-coded agents for visual coordination
- Synchronized testing and committing protocols
- File conflict detection and resolution

**Sequential Dependencies:**
- Database schema → API endpoints → Frontend integration
- Core utilities → Components that use them
- Authentication → User management → Profile features

### File Organization Patterns

#### Task Management Structure
```
/tasks/
├── prd-[feature-name].md          # Product Requirements Documents
└── tasks-prd-[feature-name].md    # Generated task lists
```

#### Agent Structure
```
/.claude/agents/
├── [feature-name]-task-[number]-agent.md  # Specialized sub-agents
└── memory-bank-synchronizer.md            # Central knowledge coordinator
```

#### Knowledge Base Structure
```
/knowledge/
├── README.md                              # Documentation
├── [timestamp]-agent-summary.md           # Raw compact outputs
└── organized/                             # Sorted summaries
    ├── [feature-name]/
    │   └── [agent-type]-summary-[num].md
    └── index.md                           # Cross-reference index
```

### Code Style and Standards

#### General Principles
- Write clear, beginner-friendly code with helpful comments
- Follow existing project conventions and patterns
- Include error handling and input validation
- Write unit tests for new functionality
- Update relevant documentation

#### Commit Message Format
```bash
git commit -m "feat: add payment validation logic" \
           -m "- Validates card type and expiry" \
           -m "- Adds unit tests for edge cases" \
           -m "Related to T123 in PRD"
```

### Testing Patterns

**Test Strategy:**
- Unit tests alongside source files
- Full test suite run per execution group (not per task)
- Tests must pass before any commits
- Use project-appropriate test commands (`pytest`, `npm test`, etc.)

### Memory Bank System Patterns

**Active Synchronization:**
- Memory-bank-synchronizer agent validates pattern accuracy
- Proactive updates when code evolves beyond documentation
- Cross-reference validation between memory files
- Pattern detection for undocumented conventions

**Integration Points:**
- Pre-task execution: Validate current state
- Post-completion: Update patterns and decisions
- Cross-session: Maintain consistency

---

*This file is part of the structured memory bank system. It's automatically synchronized with actual code patterns by the memory-bank-synchronizer agent to ensure accuracy and relevance.*