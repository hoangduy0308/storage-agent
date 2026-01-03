---
name: reviewing-beads
description: Reviews, proofreads, and refines Beads epics and issues for clarity, completeness, and correct dependencies. Use when polishing issues before implementation or auditing issue quality.
---

# Reviewing Beads Issues

Used in Planning Phase 5: Refinement (and during Validation if issue quality problems are found).

## Step 1: Load Current Issues

```bash
bd list --json
bd ready --json
```

## Step 2: Systematic Review Checklist

For EACH issue, verify:

### Clarity

- [ ] Title is action-oriented and specific
- [ ] Description is clear and unambiguous
- [ ] A developer unfamiliar with the codebase could understand the task
- [ ] No jargon without explanation

### Completeness

- [ ] Acceptance criteria are defined and testable
- [ ] Technical implementation hints are provided where helpful
- [ ] Relevant file paths or modules are mentioned
- [ ] Edge cases and error handling are considered

### Dependencies

- [ ] All blocking dependencies are linked
- [ ] No circular dependencies exist
- [ ] Dependencies are minimal (not over-constrained)
- [ ] Ready issues exist for parallel work

### Scope

- [ ] Issue is appropriately sized (not too large)
- [ ] Large issues are broken into subtasks
- [ ] No duplicate or overlapping issues

### Priority

- [ ] Priority reflects actual importance
- [ ] Critical path items are prioritized correctly
- [ ] Dependencies and priorities align

## Step 3: Common Issues to Fix

1. **Vague titles**: "Fix bug" → "Fix null pointer in UserService.getProfile when user not found"
2. **Missing context**: Add relevant file paths, function names, or module references
3. **Implicit knowledge**: Make assumptions explicit
4. **Missing acceptance criteria**: Add "Done when..." statements
5. **Over-coupling**: Break dependencies that aren't strictly necessary
6. **Under-specified**: Add technical notes for complex tasks
7. **Duplicate work**: Merge or link related issues
8. **Missing dependencies**: Link issues that should be sequenced
9. **Wrong priorities**: Adjust based on critical path analysis
10. **Typos and grammar**: Fix for professionalism

## Step 4: Update Issues

```bash
bd update <id> --title "Improved title" --json
bd update <id> --priority <new-priority> --json
bd update <id> --description "New description" --json
bd update <id> --acceptance "Acceptance criteria" --json
```

Manage dependencies:

```bash
bd dep add <issue-id> <dependency-id> --json
bd dep remove <issue-id> <dependency-id> --json
bd dep tree <issue-id> --json
bd dep cycles --json
```

For major rewrites, close and recreate:

```bash
bd close <id> --reason "Replaced by <new-id>" --json
bd create "Better title" -t <type> -p <priority> --deps <dep-id> --json
```

## Step 5: Dependency Graph Validation

```bash
bd list --json
bd list --status open --json
bd ready --json
bd dep cycles --json
bd dep tree <epic-id> --json
```

Check:

- No orphaned issues (except entry points)
- No circular dependencies
- Critical path is clear
- Parallelization opportunities are preserved

## Step 6: Final Quality Gate

1. **Readability**: Any developer can pick up any ready issue
2. **Traceability**: Issues link to epics, epics link to the plan
3. **Testability**: Each issue has clear "done" criteria
4. **Parallelism**: Multiple issues can be worked simultaneously
5. **Completeness**: No gaps in the plan coverage

## Output Format

### Summary

- Total issues reviewed: X
- Issues updated: Y
- Issues created: Z
- Issues closed/merged: W

### Changes Made

- List significant updates with rationale

### Remaining Concerns

- Any issues that need user input
- Ambiguities that couldn't be resolved

### Ready for Implementation

- List of ready issues workers can start with
- Suggested execution order for optimal flow

## Iteration Tracking

You may iterate up to 5 times. After 5 iterations: "The issues are thoroughly reviewed, well-documented, and ready for workers to implement."

**Next step**: Run `bv --robot-triage` or `bv --robot-plan` to confirm graph health, then use `orchestrating-beads` skill to execute.
