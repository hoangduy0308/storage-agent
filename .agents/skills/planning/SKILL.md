---
name: planning
description: Generates feature plans by exploring the codebase, assessing risk with spikes, and decomposing work into beads. Use when a feature roadmap is needed.
---

# Feature Planning Pipeline

Systematic discovery → synthesis → verification → decomposition for feature planning.

## Pipeline Overview

```
USER REQUEST → Discovery → Synthesis → Verification → Decomposition → Refinement → Validation → Track Planning → Ready Plan
```

| Phase             | Tool                                     | Output                              |
| ----------------- | ---------------------------------------- | ----------------------------------- |
| 1. Discovery      | Parallel sub-agents, gkg, Librarian, exa | Discovery Report                    |
| 2. Synthesis      | Oracle                                   | Approach + Risk Map                 |
| 3. Verification   | Spikes via orchestrating-beads skill     | Validated Approach + Learnings      |
| 4. Decomposition  | filing-beads skill                       | .beads/\*.md files                  |
| 5. Refinement     | reviewing-beads skill                    | Polished beads ready for workers    |
| 6. Validation     | bv + Oracle                              | Validated dependency graph          |
| 7. Track Planning | bv --robot-plan                          | Execution plan with parallel tracks |

## Phase 1: Discovery (Parallel Exploration)

Launch parallel sub-agents to gather codebase intelligence:

```
Task() → Agent A: Architecture snapshot (gkg repo_map)
Task() → Agent B: Pattern search (find similar existing code)
Task() → Agent C: Constraints (package.json, tsconfig, deps)
Librarian → External patterns ("how do similar projects do this?")
exa → Library docs (if external integration needed)
```

### Discovery Report

Save to `history/<feature>/discovery.md`. See `reference/discovery-template.md` for full template.

## Phase 2: Synthesis (Oracle)

Feed Discovery Report to Oracle for gap analysis:

```
oracle(
  task: "Analyze gap between current codebase and feature requirements",
  context: "Discovery report attached. User wants: <feature>",
  files: ["history/<feature>/discovery.md"]
)
```

Oracle produces:

1. **Gap Analysis** - What exists vs what's needed
2. **Approach Options** - 1-3 strategies with tradeoffs
3. **Risk Assessment** - LOW / MEDIUM / HIGH per component

### Risk Classification

| Level  | Criteria                      | Verification                 |
| ------ | ----------------------------- | ---------------------------- |
| LOW    | Pattern exists in codebase    | Proceed                      |
| MEDIUM | Variation of existing pattern | Interface sketch, type-check |
| HIGH   | Novel or external integration | Spike required               |

### Risk Indicators

```
Pattern exists in codebase? ─── YES → LOW base
                            └── NO  → MEDIUM+ base

External dependency? ─── YES → HIGH
                     └── NO  → Check blast radius

Blast radius >5 files? ─── YES → HIGH
                       └── NO  → MEDIUM
```

Save to `history/<feature>/approach.md`. See `reference/approach-template.md` for full template.

## Phase 3: Verification (Risk-Based)

### For HIGH Risk Items → Create Spike Beads

Spikes are mini-plans executed via the orchestrating-beads skill:

```bash
bd create "Spike: <question to answer>" -t epic -p 0
bd create "Spike: Test X" -t task --deps <spike-epic>
bd create "Spike: Verify Y" -t task --deps <spike-epic>
```

### Spike Bead Template

See `reference/spike-template.md` for the full spike bead template.

### Execute Spikes

Use the orchestrating-beads skill:

1. `bv --robot-plan` to parallelize spikes
2. `Task()` per spike with time-box
3. Workers write to `.spikes/<feature>/<spike-id>/`
4. Close with learnings: `bd close <id> --reason "<result>"`

### Aggregate Spike Results

```
oracle(
  task: "Synthesize spike results and update approach",
  context: "Spikes completed. Results: ...",
  files: ["history/<feature>/approach.md"]
)
```

Update approach.md with validated learnings.

## Phase 4: Decomposition (filing-beads skill)

Load the filing-beads skill and create beads with embedded learnings:

```bash
skill("filing-beads")
```

### Bead Requirements

Each bead MUST include:

- **Spike learnings** embedded in description (if applicable)
- **Reference to .spikes/ code** for HIGH risk items
- **Clear acceptance criteria**
- **File scope** for track assignment

### Example Bead with Learnings

```markdown
# Implement Stripe webhook handler

## Context

Spike bd-12 validated: Stripe SDK works with our Node version.
See `.spikes/billing-spike/webhook-test/` for working example.

## Learnings from Spike

- Must use `stripe.webhooks.constructEvent()` for signature verification
- Webhook secret stored in `STRIPE_WEBHOOK_SECRET` env var
- Raw body required (not parsed JSON)

## Acceptance Criteria

- [ ] Webhook endpoint at `/api/webhooks/stripe`
- [ ] Signature verification implemented
- [ ] Events: `checkout.session.completed`, `invoice.paid`
```
## Phase 5: Refinement (reviewing-beads skill)

Run `skill("reviewing-beads")` to polish beads. See that skill for the full quality checklist (clarity, completeness, dependencies, scope, priority).

## Phase 6: Validation

Run bv analysis and fix issues. If issues found, re-run `reviewing-beads` skill.

```bash
bv --robot-suggest   # Find missing dependencies
bv --robot-insights  # Detect cycles, bottlenecks
bv --robot-priority  # Validate priorities
bd dep add <from> <to>      # Add missing deps
bd dep remove <from> <to>   # Break cycles
```

Oracle final review:

```
oracle(
  task: "Review plan completeness and clarity",
  files: [".beads/"]
)
```

## Phase 7: Track Planning

Creates an **execution-ready plan** for the orchestrating-beads skill.

1. Get tracks: `bv --robot-plan 2>/dev/null | jq '.plan.tracks'`
2. Assign non-overlapping file scopes per track
3. Generate agent names (adjective+noun: BlueLake, GreenCastle, etc.)
4. Save to `history/<feature>/execution-plan.md`. See `reference/execution-plan-template.md`.

Final check:

```bash
bv --robot-insights 2>/dev/null | jq '.Cycles'  # Must be empty
bv --robot-plan 2>/dev/null | jq '.plan.unassigned'  # Must be empty
```

## Output Artifacts

| Artifact          | Location                              | Purpose                            |
| ----------------- | ------------------------------------- | ---------------------------------- |
| Discovery Report  | `history/<feature>/discovery.md`      | Codebase snapshot                  |
| Approach Document | `history/<feature>/approach.md`       | Strategy + risks                   |
| Spike Code        | `.spikes/<feature>/`                  | Reference implementations          |
| Spike Learnings   | Embedded in beads                     | Context for workers                |
| Beads             | `.beads/*.md`                         | Executable work items              |
| Execution Plan    | `history/<feature>/execution-plan.md` | Track assignments for orchestrator |

## Quick Reference

### Tool Selection

| Need               | Tool                                    |
| ------------------ | --------------------------------------- |
| Codebase structure | `mcp__gkg__repo_map`                    |
| Find definitions   | `mcp__gkg__search_codebase_definitions` |
| Find usages        | `mcp__gkg__get_references`              |
| External patterns  | `librarian`                             |
| Library docs       | `mcp__exa__get_code_context_exa`        |
| Gap analysis       | `oracle`                                |
| Create beads       | `skill("filing-beads")` + `bd create`   |
| Validate graph     | `bv --robot-*`                          |

### Common Mistakes

- **Skipping discovery** → Plan misses existing patterns
- **No risk assessment** → Surprises during execution
- **No spikes for HIGH risk** → Blocked workers
- **Missing learnings in beads** → Workers re-discover same issues
- **No bv validation** → Broken dependency graph
