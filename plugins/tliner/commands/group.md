---
description: "Consolidate scattered steps into coherent tasks. Usage: /group [IDs] [since] [until] <topic>"
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

**Behavior**:
- Copies step to target (preserves original timestamp)
- Adds `prev_tasks: [source_task_id]` to moved step metadata
- Deletes original step from source
- Deletes source task file if all steps moved

### Phase 5. Report Results

```markdown
## Grouping Complete

**Moved**: X steps
**Skipped**: Y steps (groupable: false)
**Deleted tasks**: Z empty task files

### Audit Trail
Moved steps now have `prev_tasks` metadata pointing to original task.
```

---

## Example Usage

```
/group last week                    # Group all steps from last week
/group auth                         # Group steps matching "auth" topic
/group 20251226T120000.123456Z      # Group steps from specific task
/group since Monday MkDocs          # Group MkDocs-related steps since Monday
```

## Edge Cases

- **No matches**: Print "No steps found for grouping" and exit
- **Single task**: Nothing to group - all steps already together
- **All steps protected**: Report "All steps have groupable: false"
- **Same target**: Skip steps already in target task (no-op)

## Writing Style

- Concise summaries when presenting plan
- Technical precision in step/task IDs
- Ask for confirmation before destructive operations
