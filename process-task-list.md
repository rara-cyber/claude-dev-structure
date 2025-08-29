# Task List Management

Guidelines for managing task lists in markdown files to track progress on completing a PRD

## Agent Assignment Protocol

- **One Agent Per Parent Task**: Each parent task (1.0, 2.0, 3.0, etc.) is assigned to one specialized agent
- **Agent Naming Convention**: `[feature-name]-task-[parent-number]-agent.md`
  - Example: `user-auth-task-1-agent.md`, `user-auth-task-2-agent.md`
- **Agent Scope**: Agent handles ALL sub-tasks under its assigned parent task
- **Agent Discovery**: Check `/.claude/agents/` directory for available agents
- **Fallback**: Use general-purpose agent if specialized agent unavailable

## Dependency Analysis for Parallel Execution

### Task Dependency Types
- **Independent Tasks**: Can run in parallel without conflicts
  - UI components + Documentation
  - Testing + Configuration files
  - Separate feature modules
- **Dependent Tasks**: Must run sequentially
  - Database schema → API endpoints → Frontend integration
  - Authentication system → User management → Profile features
  - Core utilities → Components that use them

### Parallel Safety Assessment
Before launching agents in parallel, verify:
1. **File Conflicts**: No shared files being modified simultaneously
2. **Dependency Chain**: No parent task depends on another's completion
3. **Resource Competition**: No competing database migrations or API changes
4. **Test Dependencies**: Tests can run independently without setup conflicts

### Dependency Analysis Protocol
1. **Review Task List**: Examine all parent tasks for interdependencies
2. **Map File Usage**: Identify which files each parent task will modify
3. **Create Execution Groups**: Group independent tasks for parallel processing
4. **Define Sequence**: Order dependent groups sequentially

## Task Implementation
- **Agent-Based Processing**: Each parent task is handled by one specialized agent
- **One sub-task at a time:** Do **NOT** start the next sub‑task until you ask the user for permission and they say "yes" or "y"
- **Agent Context**: Pass PRD context, task scope, and relevant files to the assigned agent
- **Task status indicators:**
  - `[ ]` - Not started
  - `[o]` - Currently in progress (ongoing)
  - `[x]` - Completed
- **Completion protocol:**  
  1. **Sub-task Level** (per agent):
     - When you **start** a sub‑task, immediately mark it as ongoing by changing `[ ]` to `[o]`
     - When you finish a **sub‑task**, immediately mark it as completed by changing `[o]` to `[x]`
     - Ask user permission before proceeding to next sub-task within same parent task
  2. **Parent Task Level** (per execution group):
     - **Parallel Completion**: Wait for ALL agents in execution group to finish their parent tasks
     - **Synchronized Testing**: Run full test suite once for entire execution group
     - **Group-Level Protocol**:
       - **Test All**: Run the full test suite (`pytest`, `npm test`, `bin/rails test`, etc.) covering all parallel changes
       - **Only if all tests pass**: Stage all changes (`git add .`)
       - **Clean up**: Remove any temporary files and temporary code before committing
       - **Consolidated Commit**: Single commit message covering all parallel parent tasks:
         - Uses conventional commit format (`feat:`, `fix:`, `refactor:`, etc.)
         - Summarizes what was accomplished across all parent tasks in group
         - Lists key changes and additions from all agents
         - References all task numbers and PRD context
         - **Formats the message as a single-line command using `-m` flags**, e.g.:

           ```
           git commit -m "feat: implement user auth system" -m "- Add login/logout UI components (T1.0)" -m "- Create user session management (T3.0)" -m "- Add authentication tests (T4.0)" -m "Related to user-auth PRD"
           ```
       - Mark ALL **parent tasks** in execution group as completed `[x]`
- Stop after each sub‑task and wait for the user's go‑ahead.

## Task List Maintenance

1. **Update the task list as you work:**
   - Mark tasks as ongoing (`[o]`) when starting them.
   - Mark tasks and subtasks as completed (`[x]`) per the protocol above.
   - Add new tasks as they emerge.

2. **Maintain the "Relevant Files" section:**
   - List every file created or modified.
   - Give each file a one‑line description of its purpose.

## Automatic Documentation

**Knowledge Capture**: When specialized agents complete their tasks, Claude Code hooks automatically:
- Execute `/compact` command to summarize the conversation
- Save summaries to `/knowledge/` directory with timestamps
- Organize summaries by feature and agent type using helper scripts
- Update knowledge base index for easy reference

**No Manual Documentation Required**: Agents focus on implementation; hooks handle documentation automatically.

## Agent Workflow

### Agent Selection Process
1. **Synchronize Memory Bank**: Use memory-bank-synchronizer agent to ensure current system state is accurately reflected in memory bank before task execution.
2. **Dependency Analysis**: Run dependency analysis protocol first
3. **Identify Execution Groups**: Group independent parent tasks for parallel processing
4. **Locate Agents**: Check for `[feature-name]-task-[parent-number]-agent.md` in `/.claude/agents/`
5. **Launch Strategy**: Choose sequential or parallel execution based on dependencies

### Parallel Agent Execution Flow
```
Sequential Groups:
Group 1: Parent Task 1.0 → Agent 1 ┐
         Parent Task 3.0 → Agent 3 ├─ Run in Parallel → Test All → Commit All
         Parent Task 4.0 → Agent 4 ┘

Group 2: Parent Task 2.0 → Agent 2 → Test → Commit (depends on Group 1)
```

### Agent Launch Strategies

#### Parallel Launch (Independent Tasks)
```javascript
// Launch multiple agents simultaneously
Task(agent-1, context) + Task(agent-3, context) + Task(agent-4, context)
```

#### Sequential Launch (Dependent Tasks)
```javascript
// Wait for completion before next group
Task(agent-1, context) → Complete → Task(agent-2, context)
```

### Cross-Agent Coordination
- **Parallel Sessions**: Multiple agents run simultaneously on independent parent tasks
- **Shared Context**: All agents receive same PRD context and current state
- **Status Synchronization**: Real-time task status updates across parallel agents
- **Conflict Detection**: Monitor for file conflicts during parallel execution
- **Synchronized Completion**: Wait for all parallel agents before testing phase

## Conflict Resolution and Performance Optimization

### File Conflict Management
- **Pre-Execution Check**: Verify no file overlap between parallel agents
- **Real-Time Monitoring**: Track file modifications during parallel execution
- **Conflict Detection**: Immediately flag when agents attempt to modify same files
- **Resolution Strategy**: 
  - Pause conflicting agent
  - Complete first agent's work
  - Resume second agent with updated context

### Performance Optimization Best Practices
- **Optimal Grouping**: Group 2-4 independent parent tasks for parallel execution
- **Resource Management**: Monitor system resources to prevent overload
- **Context Efficiency**: Share common context between parallel agents to reduce overhead
- **Staggered Launch**: Start agents with slight delays to prevent resource competition

### Error Handling for Parallel Execution
- **Agent Failure Recovery**: If one parallel agent fails, others continue
- **Partial Success Protocol**: Commit successful agents, retry failed ones
- **Rollback Strategy**: Ability to revert parallel changes if critical failure occurs
- **Status Reconciliation**: Ensure task list accuracy despite partial failures

## AI Instructions

When working with task lists, the AI must:

1. **Memory Bank Synchronization**: Use memory-bank-synchronizer agent to validate system state before and after major workflow phases
2. **Dependency Analysis**: Always run dependency analysis before starting any parent tasks
3. **Parallel Agent Management**: 
   - Launch independent parent tasks in parallel using multiple Task tool calls
   - Monitor for file conflicts and resource competition
   - Wait for all parallel agents to complete before testing
4. **Task Status Management**: 
   - Mark each **started** sub‑task `[o]`
   - Mark each finished **sub‑task** `[x]`
   - Mark ALL **parent tasks** in execution group `[x]` only after group testing and committing
5. **Synchronized Testing Protocol**: Run full test suite once per execution group, not per parent task
6. **File Management**: Keep "Relevant Files" accurate and up to date across all parallel agents
7. **Permission-Based Flow**: Ask user approval between sub-tasks within parent tasks
8. **Context Passing**: Provide complete context to all agents including PRD, scope, files, and current state
9. **Conflict Resolution**: Handle file conflicts and agent coordination issues proactively
10. **Memory Bank Updates**: Use memory-bank-synchronizer agent to update patterns and decisions discovered during execution
11. **Trust the hooks**: Focus on implementation quality; documentation happens automatically via SubagentStop hooks