---
description: "Consolidate scattered steps into coherent tasks.  Usage: [task_id | topic] [since] [until]"
argument-hint: "[task_id | topic] [since] [until]"
allowed_tools: ["Read", "mcp__timeliner__get_steps", "mcp__timeliner__group_steps", "mcp__timeliner__task_list"]
---

## Task

Consolidate scattered Timeline steps into coherent task files. LLM decides groupings semantically, MCP executes moves.

$ARGUMENTS

## Flow: Load → Analyze → Group → Execute

### Phase 1. Execute `/load` (`load.md`)

- Read `load.md` file and execute it with $ARGUMENTS
- Retrieve all matching steps

### Phase 2. Analyze Steps Semantically

**Goal**: Identify which steps belong together.

**Rules**:
- Group by topic/feature, not by time alone
- "Time-first becomes king" - earliest task by timestamp = target
- Skip steps with `metadata.groupable: false`
- Consider: same feature? same bug? same investigation thread?

**Output**: Mental mapping of `{step_id: target_task_id}` pairs

### Phase 3. Present Grouping Plan

Before executing, show the user:

```markdown
## Proposed Groupings

### Target: [earliest_task_title] (`task_id`)

Moving from [source_task_title]:
- [step_title] (`step_id`)
- [step_title] (`step_id`)

Moving from [another_source]:
- [step_title] (`step_id`)

### Skipped (groupable: false)
- [step_title] (`step_id`) - protected

### Result
- X steps will move
- Y source tasks will be deleted (emptied)
```

**Ask user to confirm** before proceeding.

### Phase 4. Execute Grouping

Call `mcp__timeliner__group_steps` with instructions:

```python
mcp__timeliner__group_steps({
    "step_id_1": "target_task_id",
    "step_id_2": "target_task_id",
    ...
})
```


### Phase 5. Report Results

```markdown
Grouping Complete:

**Moved**: X steps
**Skipped**: Y steps (groupable: false)
**Deleted tasks**: Z empty task files

```

## Writing Style

- Concise summaries when presenting plan
- Technical precision in step/task IDs
- Ask for confirmation before destructive operations
