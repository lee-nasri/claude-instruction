# claude.md - Conversation History Management

This file provides instructions to Claude Code for maintaining conversation history.

## History Directory Rules

**Determine your history directory first:**
- If you have been given a team name (e.g., `teamA`, `teamB`) → `~/.claude/history/{team}/`
- If no team was given → default to `teamA` → `~/.claude/history/teamA/`

All paths below use `{history_dir}` to refer to your determined directory.

## Conversation History Management

**IMPORTANT**: Every time you interact, you must maintain conversation history following these rules:

### 1. Starting a New Conversation

When starting a new conversation:
1. Create a new file in `{history_dir}`
2. Name format: `YYYY-MM-DD_conversation_{number}.md`
   - Example: `2025-01-09_conversation_01.md`
   - If multiple conversations on same day, increment number: `_02.md`, `_03.md`
3. Initialize the file with this template:

```markdown
# Conversation History: [Date]

**Started**: [Timestamp]
**Last Updated**: [Timestamp]
**Status**: Active/Completed

## Summary
Brief description of what this conversation is about.

## Conversation Log

### [Timestamp] - User Request
[User's message]

### [Timestamp] - Claude Response
[Your response summary]
- Actions taken: [list]
- Files modified: [list]
- Key decisions: [list]

### [Timestamp] - User Request
...
```

### 2. Updating Conversation History

**Every time you interact** (receive user message or take action):
1. Read the current active conversation file
2. Append new entry with:
   - Timestamp
   - User request or your action
   - Summary of what you did
   - Files created/modified
   - Key decisions or findings
3. Update the "Last Updated" timestamp
4. Write back to the file

### 3. Conversation Entry Format

**User Request Entry**:
```markdown
### [HH:MM:SS] - User Request
> [User's message verbatim or summary]

**Context**: [Any important context]
```

**Claude Action Entry**:
```markdown
### [HH:MM:SS] - Claude Actions
**Summary**: [What you did in 1-2 sentences]

**Actions Taken**:
- Created file: `path/to/file.md`
- Modified file: `path/to/other.go`
- Analyzed: [what you analyzed]
- Found: [key findings]

**Key Decisions**:
- [Decision 1 and rationale]
- [Decision 2 and rationale]

**Files Affected**:
- `/path/to/file1.md` - Created comprehensive project overview
- `/path/to/file2.go` - Added new function for X

**Next Steps**: [If applicable]
- [ ] Task 1
- [ ] Task 2
```

### 4. When to Mark Conversation as Completed

Mark conversation status as "Completed" when:
- User explicitly ends the conversation
- Task is fully completed and verified
- User starts a completely new topic (create new conversation file)

### 5. History Directory Structure

```
.claude/
├── history/
│   ├── 2025-01-09_conversation_01.md  # First conversation of the day
│   ├── 2025-01-09_conversation_02.md  # Second conversation (different topic)
│   ├── 2025-01-10_conversation_01.md
│   └── README.md                       # Index of all conversations
├── overview/
│   └── project_overview.md             # Technical documentation
└── ...
```

### 6. History README.md

Maintain `.claude/history/README.md` as an index:

```markdown
# Conversation History Index

## 2025-01-09
- **conversation_01.md** - Created project overview documentation
  - Status: Completed
  - Duration: ~15 minutes
  - Key outcome: Comprehensive technical documentation created

## 2025-01-10
- **conversation_01.md** - [Topic]
  - Status: Active/Completed
  - Duration: [time]
  - Key outcome: [outcome]
```

## Example Workflow

**Scenario**: User asks you to create project documentation

1. **Before taking any action**, create history file:
   - `.claude/history/2025-01-09_conversation_01.md`
   - Initialize with template

2. **After user sends first message**, update history:
   ```markdown
   ### 14:30:00 - User Request
   > I want you to read how migrate-broker-graph work and then write report...
   ```

3. **As you work**, update continuously:
   ```markdown
   ### 14:32:00 - Claude Actions
   **Summary**: Read source files and analyzed architecture

   **Actions Taken**:
   - Read main.go, cmd/, config/, domain/, script/, repository/
   - Analyzed 5-step migration process
   - Identified key components and data flow

   **Files Analyzed**: [list]
   ```

4. **After completing task**, final update:
   ```markdown
   ### 14:45:00 - Claude Actions
   **Summary**: Created comprehensive project overview

   **Actions Taken**:
   - Created `.claude/overview/project_overview.md`
   - Documented architecture, data flow, components
   - Added usage examples and troubleshooting guide

   **Files Created**:
   - `.claude/overview/project_overview.md` - 15,000+ words technical doc

   **Status**: Task completed
   ```

5. **User confirms or conversation ends**, mark as completed:
   ```markdown
   **Status**: Completed
   ```

## Benefits of This Approach

1. **Continuity**: Future Claude instances can understand conversation context
2. **Traceability**: Track what was done, when, and why
3. **Decision Log**: Record key decisions and rationales
4. **Audit Trail**: Know what files were created/modified
5. **Learning**: Understand patterns and common requests

## Important Notes

- **Always update history BEFORE and AFTER taking actions**
- **Be concise but complete** - capture essence without verbosity
- **Include timestamps** - helps with temporal understanding
- **Link related conversations** - if continuing previous work, reference it
- **Update status** - keep status field current (Active/Completed)

**Remember**: Maintaining conversation history is not optional. It's a required practice for every interaction with this project. Future you (and future users) will thank current you for keeping detailed records.
