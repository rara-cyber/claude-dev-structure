# Rule: Creating Specialized Sub-Agents from Task Lists

## Goal

To guide an AI assistant in creating specialized sub-agents for each parent task from an existing task list. Each sub-agent follows Anthropic's 2025 sub-agent documentation best practices and is optimized for its specific domain (UI development, API implementation, testing, etc.).

## Output

- **Format:** YAML frontmatter + Markdown system prompt (`.md`)
- **Location:** `/.claude/agents/`  
- **Filename:** `[feature-name]-[task-type]-agent.md` (e.g., `user-profile-ui-agent.md`, `payment-api-agent.md`)

## Process

1. **Receive Task List Reference:** The user points the AI to a specific `tasks-[prd-file-name].md` file
2. **Synchronize Memory Bank:** Use the memory-bank-synchronizer agent to ensure current architectural patterns and coding standards in memory bank reflect actual implementation.
3. **Analyze Task Structure:** Read and analyze the parent tasks (1.0, 2.0, 3.0, etc.) and their sub-tasks to understand the implementation domains
4. **Assess Codebase Context:** Review existing architectural patterns, frameworks, and conventions that agents should follow
5. **Generate Agent Specifications:** For each parent task, create a specialized sub-agent with:
   - Appropriate tool permissions
   - Domain-specific system prompts
   - PRD context integration
   - Coding standards and conventions
6. **Create Directory Structure:** Ensure `/.claude/agents/` directory exists
7. **Save Sub-Agents:** Generate and save each agent file with proper naming conventions
8. **Update Memory Bank:** Use the memory-bank-synchronizer agent to update CLAUDE-patterns.md with agent creation patterns and document any new architectural decisions for agent coordination.

## Agent Creation Rules

### Color Assignment System
Each agent receives a distinct color for visual identification and coordination:

| Agent Type | Color | Hex Code | Purpose |
|------------|--------|----------|---------|
| **UI/Frontend** | 🔵 Blue | `#3498DB` | User interface and components |
| **API/Backend** | 🟢 Green | `#2ECC71` | Server logic and endpoints |
| **Database** | 🟣 Purple | `#9B59B6` | Data storage and models |
| **Testing** | 🟠 Orange | `#E67E22` | Validation and test coverage |
| **DevOps/Deployment** | 🔴 Red | `#E74C3C` | Build and infrastructure |
| **General/Multi-purpose** | ⚪ Gray | `#95A5A6` | Cross-cutting concerns |

### Naming Convention
- Format: `[feature-name]-task-[parent-number]-agent.md` (updated for parallel execution)
- Use lowercase with hyphens
- Parent number corresponds to task list parent task (1, 2, 3, etc.)
- Examples: `user-auth-task-1-agent.md`, `payment-task-2-agent.md`, `profile-task-3-agent.md`

### YAML Frontmatter Structure
```yaml
---
name: [feature-name]-task-[parent-number]-agent
description: Specialized agent for [parent task description] in the [feature-name] feature
color: "#3498DB"  # Assigned color from color scheme above
tools: Read, Write, Edit, MultiEdit, Bash, Grep, Glob  # Adjust based on task needs
---
```

### Color Assignment Process
1. **Analyze Parent Task Domain**: Determine the primary domain of the parent task
2. **Assign Base Color**: Use color table above to assign appropriate color
3. **Handle Multi-Domain Tasks**: For tasks spanning multiple domains, use the dominant domain's color
4. **Unique Identification**: Each agent gets a distinct color for easy visual identification

### Tool Permission Guidelines
- **Implementation Agents** (UI, API, Database): `Read, Write, Edit, MultiEdit, Bash, Grep, Glob`
- **Review Agents**: `Read, Grep, Glob`  
- **Testing Agents**: `Read, Write, Edit, Bash, Grep, Glob`
- **Documentation Agents**: `Read, Write, Edit, Grep, Glob`

### System Prompt Requirements

Each system prompt must include:

1. **Role Definition:**
   ```
   You are a specialized [domain] development agent for the [feature-name] feature.
   Your primary responsibility is implementing [parent task description].
   ```

2. **PRD Context Integration:**
   ```
   ## Feature Context
   [Include relevant sections from the original PRD: goals, user stories, functional requirements]
   
   ## Technical Requirements  
   [Extract technical considerations and constraints from the PRD]
   ```

3. **Task Scope:**
   ```
   ## Your Responsibilities
   You are specifically focused on:
   - [Sub-task 1.1 description]
   - [Sub-task 1.2 description]
   - [Sub-task 1.3 description]
   
   Do not work on tasks outside your scope - defer to other specialized agents.
   ```

4. **Coding Standards:**
   ```
   ## Development Guidelines
   - Follow existing project conventions and patterns
   - Write clear, maintainable code with appropriate comments
   - Include error handling and input validation
   - Write unit tests for new functionality
   - Update relevant documentation
   ```

5. **Implementation Approach:**
   ```
   ## Implementation Strategy
   [Specific guidance based on task type - e.g., component structure for UI agents, 
   API design patterns for backend agents, testing frameworks for testing agents]
   ```

6. **Automatic Documentation:**
   ```
   ## Automatic Documentation
   Your work is automatically documented through Claude Code hooks:
   - When you complete tasks, `/compact` runs automatically  
   - Conversation summaries are saved to the knowledge base
   - Focus on implementation quality; documentation is handled automatically
   - No need to create manual documentation files unless specifically requested
   ```

## Agent Types by Task Domain

### 🔵 UI/Frontend Agents (Blue #3498DB)
- **Focus:** Component development, styling, user interactions
- **Tools:** `Read, Write, Edit, MultiEdit, Bash, Grep, Glob`
- **Specialization:** React/Vue/Angular patterns, responsive design, accessibility
- **Color Usage:** Visual identification in parallel execution, UI task coordination

### 🟢 API/Backend Agents (Green #2ECC71)
- **Focus:** Server endpoints, business logic, data processing
- **Tools:** `Read, Write, Edit, MultiEdit, Bash, Grep, Glob`
- **Specialization:** RESTful design, error handling, security practices
- **Color Usage:** Backend service identification, API coordination

### 🟣 Database Agents (Purple #9B59B6)
- **Focus:** Schema design, migrations, queries, data models
- **Tools:** `Read, Write, Edit, MultiEdit, Bash, Grep, Glob` 
- **Specialization:** SQL optimization, indexing, data integrity
- **Color Usage:** Data layer identification, database dependency tracking

### 🟠 Testing Agents (Orange #E67E22)
- **Focus:** Unit tests, integration tests, test data setup
- **Tools:** `Read, Write, Edit, MultiEdit, Bash, Grep, Glob`
- **Specialization:** Testing frameworks, mocking, coverage
- **Color Usage:** Quality assurance identification, test coordination

### 🔴 DevOps/Deployment Agents (Red #E74C3C)
- **Focus:** Build processes, deployment configs, CI/CD
- **Tools:** `Read, Write, Edit, MultiEdit, Bash, Grep, Glob`
- **Specialization:** Docker, build tools, environment configuration
- **Color Usage:** Infrastructure identification, deployment coordination

### ⚪ General/Multi-purpose Agents (Gray #95A5A6)
- **Focus:** Cross-cutting concerns, documentation, configuration
- **Tools:** `Read, Write, Edit, MultiEdit, Bash, Grep, Glob`
- **Specialization:** Project setup, general utilities, coordination tasks
- **Color Usage:** Neutral identification for multi-domain tasks

## Directory Structure Creation

Ensure the following structure exists:
```
/.claude/
└── agents/
    ├── [feature-name]-[task-type]-agent.md
    ├── [feature-name]-[task-type]-agent.md
    └── [feature-name]-[task-type]-agent.md
```

## Automatic Documentation Integration

Each generated agent automatically participates in knowledge capture through Claude Code hooks:

### Hook-Based Workflow
- **SubagentStop Hook**: When an agent completes, automatically executes `/compact` command
- **Knowledge Storage**: Compact summaries saved to `/knowledge/` directory  
- **Organization**: Helper scripts categorize summaries by feature and agent type
- **Zero Manual Work**: Agents focus on implementation; documentation happens automatically

## Integration with Existing Workflow

This creates a **5-phase workflow**:
1. **PRD Creation** → `prd-[feature-name].md`
2. **Task Generation** → `tasks-prd-[feature-name].md`  
3. **Agent Creation** → `/.claude/agents/[feature-name]-*-agent.md`
4. **Task Execution** → Using specialized agents for implementation
5. **Knowledge Capture** → Automatic `/compact` and organization via hooks

## Output Format

After creating all agents, provide a color-coded summary:
```markdown
## Generated Sub-Agents for [Feature Name]

- 🔵 `[feature-name]-task-1-agent.md` (Blue #3498DB) - UI/Frontend components and user interface
- 🟢 `[feature-name]-task-2-agent.md` (Green #2ECC71) - API/Backend endpoints and business logic  
- 🟣 `[feature-name]-task-3-agent.md` (Purple #9B59B6) - Database schema and data models
- 🟠 `[feature-name]-task-4-agent.md` (Orange #E67E22) - Testing and quality assurance
- 🔴 `[feature-name]-task-5-agent.md` (Red #E74C3C) - DevOps and deployment processes

### Color Coordination Benefits:
- **Visual Identification**: Quickly distinguish agent roles in parallel execution
- **Dependency Tracking**: Color-code task dependencies and execution groups
- **Status Monitoring**: Use colors to track parallel agent progress
- **Conflict Resolution**: Identify potential resource conflicts by color overlap

Each agent is specialized for its parent task domain and inherits the PRD context and requirements.
```

### Integration with Parallel Execution
The color system enhances the parallel execution workflow:
- **Execution Groups**: Color-code independent vs dependent task groups
- **Resource Management**: Identify potential file conflicts by color coordination
- **Visual Dashboard**: Create color-coded status views for multiple parallel agents
- **Agent Coordination**: Use colors for clear communication between parallel agents

## Memory Bank Integration

The agent creation process actively uses the memory-bank-synchronizer agent to:
- **Pre-Creation:** Validate current architectural patterns and coding standards accuracy
- **During Creation:** Apply consistent patterns across all generated agents
- **Post-Creation:** Update memory bank with agent coordination patterns and architectural decisions

This ensures agents are created with current, accurate context and contribute to consistent architectural evolution.

## Target Audience

The generated sub-agents should be optimized for **efficient, focused implementation** by providing:
- Clear scope boundaries
- Relevant technical context
- Domain-specific best practices  
- Appropriate tool permissions
- Integration guidance with existing codebase patterns