# Claude Global Instructions

## Session Startup Routine (MUST DO FIRST)

At the start of **every** new session, before doing anything else, run these steps in order:

### Step 1 — Read full instructions
Read these files:
- `~/.claude/instructions/claude-instruction.md`
- `~/.claude/instructions/claude-history.md`
- `~/.claude/instructions/rules/critical-rules.md`

### Step 2 — Read teams registry

Read `~/.claude/history/teams-registry.md`

This shows what every team is currently focused on. Update the relevant row whenever your team's focus changes.

### Step 3 — Read conversation history

Determine your history directory based on your team:
- If you have been given a team name (e.g., `teamA`, `teamB`) → use `~/.claude/history/{team}/`
- If no team was given → default to `teamA` → use `~/.claude/history/teamA/`

Then:
1. Compute today's date and yesterday's date (format: `YYYY-MM-DD`)
2. Look for and read the following files if they exist:
   - `{history_dir}/{YESTERDAY}_conversation_01.md`
   - `{history_dir}/{TODAY}_conversation_01.md`
3. If today's file does not exist yet, create it using the template in `~/.claude/instructions/claude-history.md`

This restores context so you understand what was worked on recently without the user needing to re-explain.

### Step 4 — Load trading system knowledge

Read `~/.claude/agents/trading-domain-expert.md`

This gives you full knowledge of your trading system — service overviews, order lifecycle, domain enums, queue patterns, Redis keys, fee rules, and the index of per-service knowledge files to read when deep detail is needed.


---

## Full Instructions Reference

- `~/.claude/instructions/rules/critical-rules.md` — all critical rules (loaded in Step 1)
- `~/.claude/instructions/claude-instruction.md` — complete rules with examples
- `~/.claude/instructions/claude-history.md` — conversation history management protocol
- `~/.claude/instructions/00-quick-reference.md` — quick reference card
