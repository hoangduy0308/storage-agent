# Worker Prompt Template

Use this template when spawning workers:

```
You are agent {AGENT_NAME} working on Track {N} of epic {EPIC_ID}.

## Setup
1. Read {PROJECT_PATH}/AGENTS.md for tool preferences
2. Load the worker skill

## Your Assignment
- Track: {TRACK_NUMBER}
- Beads (in order): {BEAD_LIST}
- File scope: {FILE_SCOPE}
- Epic thread: {EPIC_ID}
- Track thread: track:{AGENT_NAME}:{EPIC_ID}

## Tool Preferences (from AGENTS.md)
- Codebase exploration: mcp__gkg__* tools
- File editing: mcp__morph_mcp__edit_file
- Web search: mcp__exa__* tools
- UI components: mcp__shadcn__* tools

## Protocol
For EACH bead in your track:

1. START BEAD
   - register_agent(name="{AGENT_NAME}")
   - summarize_thread(thread_id="track:{AGENT_NAME}:{EPIC_ID}")
   - file_reservation_paths(paths=["{FILE_SCOPE}"], reason="{BEAD_ID}")
   - bd update {BEAD_ID} --status in_progress

2. WORK
   - Implement the bead requirements
   - Use preferred tools from AGENTS.md
   - Check inbox periodically

3. COMPLETE BEAD
   - bd close {BEAD_ID} --reason "..."
   - send_message to orchestrator: "[{BEAD_ID}] COMPLETE"
   - send_message to self (track thread): context for next bead
   - release_file_reservations()

4. NEXT BEAD
   - Read track thread for context
   - Continue with next bead

## When Track Complete
- send_message to orchestrator: "[Track {N}] COMPLETE"
- Return summary of all work

## Important
- ALWAYS read track thread before starting each bead for context
- ALWAYS write context to track thread after completing each bead
- Report blockers immediately to orchestrator
```
