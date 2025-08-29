# Rule: Creating Specialized Sub-Agents from Task Lists

## Goal

To guide an AI assistant in creating specialized sub-agents for each parent task from an existing task list. Each sub-agent follows Anthropic's 2025 sub-agent documentation best practices and is optimized for its specific domain (UI development, API implementation, testing, etc.).

## Output

- **Format:** YAML frontmatter + Markdown system prompt (`.md`)
- **Location:** `/.claude/agents/`  
- **Filename:** `[feature-name]-[task-type]-agent.md` (e.g., `user-profile-ui-agent.md`, `payment-api-agent.md`)

## Process

1. **Receive Task List Reference:** The user points the AI to a specific `tasks-[prd-file-name].md` file
2. **Analyze Task Structure:** Read and analyze the parent tasks (1.0, 2.0, 3.0, etc.) and their sub-tasks to understand the implementation domains
3. **Assess Codebase Context:** Review existing architectural patterns, frameworks, and conventions that agents should follow
4. **Generate Agent Specifications:** For each parent task, create a specialized sub-agent with:
   - Appropriate tool permissions
   - Domain-specific system prompts
   - PRD context integration
   - Coding standards and conventions
5. **Create Directory Structure:** Ensure `/.claude/agents/` directory exists
6. **Save Sub-Agents:** Generate and save each agent file with proper naming conventions

## Agent Creation Rules

### Naming Convention
- Format: `[feature-name]-[task-type]-agent.md`
- Use lowercase with hyphens
- Task type should reflect the parent task domain (ui, api, database, testing, deployment, etc.)
- Examples: `user-auth-ui-agent.md`, `payment-api-agent.md`, `user-profile-testing-agent.md`

### YAML Frontmatter Structure
```yaml
---
name: [feature-name]-[task-type]-agent
description: Specialized agent for [specific task domain] in the [feature-name] feature
tools: Read, Write, Edit, MultiEdit, Bash, Grep, Glob  # Adjust based on task needs
---
```

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

### UI/Frontend Agents
- **Focus:** Component development, styling, user interactions
- **Tools:** `Read, Write, Edit, MultiEdit, Bash, Grep, Glob`
- **Specialization:** React/Vue/Angular patterns, responsive design, accessibility

### API/Backend Agents  
- **Focus:** Server endpoints, business logic, data processing
- **Tools:** `Read, Write, Edit, MultiEdit, Bash, Grep, Glob`
- **Specialization:** RESTful design, error handling, security practices

### Database Agents
- **Focus:** Schema design, migrations, queries, data models
- **Tools:** `Read, Write, Edit, MultiEdit, Bash, Grep, Glob` 
- **Specialization:** SQL optimization, indexing, data integrity

### Testing Agents
- **Focus:** Unit tests, integration tests, test data setup
- **Tools:** `Read, Write, Edit, MultiEdit, Bash, Grep, Glob`
- **Specialization:** Testing frameworks, mocking, coverage

### DevOps/Deployment Agents
- **Focus:** Build processes, deployment configs, CI/CD
- **Tools:** `Read, Write, Edit, MultiEdit, Bash, Grep, Glob`
- **Specialization:** Docker, build tools, environment configuration

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

After creating all agents, provide a summary:
```markdown
## Generated Sub-Agents for [Feature Name]

- `[feature-name]-ui-agent.md` - Handles frontend components and user interface
- `[feature-name]-api-agent.md` - Manages backend endpoints and business logic  
- `[feature-name]-testing-agent.md` - Creates comprehensive test coverage
- `[feature-name]-deployment-agent.md` - Handles build and deployment processes

Each agent is specialized for its domain and inherits the PRD context and requirements.
```

## Target Audience

The generated sub-agents should be optimized for **efficient, focused implementation** by providing:
- Clear scope boundaries
- Relevant technical context
- Domain-specific best practices  
- Appropriate tool permissions
- Integration guidance with existing codebase patterns