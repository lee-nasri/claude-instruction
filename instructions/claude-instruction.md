# Claude Code - Critical Instructions

## 🚨 CRITICAL RULE: Never Commit Code Without Explicit Permission

### The Rule

**DO NOT commit code to git unless the user explicitly tells you to commit.**

This is an absolute rule with no exceptions.

### What "Explicit Permission" Means

**✅ User says commit:**
- "commit this"
- "create a commit"
- "commit the changes"
- "git commit"
- "commit and push"
- "make a commit with message X"

**❌ User does NOT say commit:**
- "looking good" - NO COMMIT
- "that's perfect" - NO COMMIT
- "great job" - NO COMMIT
- "looks correct" - NO COMMIT
- "that's what I wanted" - NO COMMIT
- "proceed" - NO COMMIT (unless they explicitly say "proceed with commit")
- "done" - NO COMMIT
- "next step" - NO COMMIT

### Why This Rule Exists

1. **User controls the codebase** - Only the user decides when changes are committed
2. **User may want to review** - Even if tests pass, user may want to inspect changes
3. **User may want different commit structure** - User may want separate commits, different messages, or squashed commits
4. **User may want to make additional changes** - Before committing
5. **Respect for user's workflow** - User has their own git workflow and preferences

### What You Should Do Instead

When you complete an implementation:

1. **Run all tests** - Verify everything works
2. **Show the user what changed** - Use `git status`, `git diff --stat`
3. **Summarize the work** - Explain what was implemented
4. **Wait for instructions** - Let the user decide what to do next

**Example**:
```
✅ All changes implemented successfully!

Modified files:
- domain/order.go (+41 lines)
- domain/queue.go (+19 lines)
- service/processor/process.go (+1, -3 lines)
- service/processor/process_transaction.go (+3, -5 lines)

Test results:
✅ All 5 domain tests pass
✅ All 20 service tests pass
✅ Code compiles successfully

The changes are ready. Let me know when you'd like to commit.
```

### What NOT to Do

**❌ NEVER do this:**
```bash
# Automatically committing after implementation
git add .
git commit -m "fix: implement feature"
git push
```

Even if:
- All tests pass ✅
- Code compiles ✅
- Everything looks perfect ✅
- You wrote a great commit message ✅

**STILL WAIT FOR USER PERMISSION**

### If You Accidentally Commit

If you accidentally commit without permission:

1. **Immediately acknowledge the mistake**
2. **Undo the commit** using `git reset --soft HEAD~1`
3. **Unstage all changes** using `git reset`
4. **Apologize** and explain you'll wait for instructions
5. **Learn from it** - don't repeat the mistake

**Example recovery**:
```bash
git reset --soft HEAD~1  # Undo commit, keep changes
git reset                # Unstage all files
git status               # Show current state
```

### Edge Cases

**Q: What if user says "finish the task"?**
A: Implement and test, but don't commit. Report completion and wait.

**Q: What if user says "make it work"?**
A: Implement and test, but don't commit. Show them it works and wait.

**Q: What if user says "push to remote"?**
A: Ask if they want you to commit first, or if changes are already committed.

**Q: What if there's an existing commit that needs to be pushed?**
A: Only push if user explicitly says "push" or "git push".

### Summary

```
┌─────────────────────────────────────────────────┐
│  IF USER SAYS "COMMIT" → You can commit        │
│  IF USER DOES NOT SAY "COMMIT" → DO NOT COMMIT │
└─────────────────────────────────────────────────┘
```

**When in doubt**: ASK the user if they want you to commit.

**Better to ask** than to commit without permission.

---

## ⚡ HIGH PRIORITY: Code Simplicity Standards

**READ BEFORE WRITING ANY CODE:**

See [standards/code-simplicity.md](./standards/code-simplicity.md) for complete guidelines.

### Critical Principles (MUST FOLLOW)

1. ❌ **NO unnecessary wrapper functions** - Call underlying APIs/libraries directly
   - Don't create functions that just call another function
   - Add layers only when adding real value (error handling, transformation, etc.)

2. ❌ **NO unused code** - Delete immediately
   - Delete unused functions, classes, methods
   - Delete unused interface/trait methods
   - Delete unused imports
   - Git is your safety net - retrieve from history if needed later

3. ❌ **NO version suffixes for single implementations**
   - Use `client`, not `clientV2` when only one version exists
   - Use `handler`, not `handlerV1` when no other versions
   - Only use version suffixes during migration periods

4. ❌ **NO keeping old dependencies for one function**
   - Don't import entire library for one utility function
   - Copy just that function or reimplement (often 5-10 lines)

5. ✅ **INLINE simple logic** - Don't extract functions for 1-2 lines used once
   - Extract only when: used 3+ times, complex logic (>5 lines), needs tests

6. ✅ **KEEP interfaces minimal** - Only methods actually used
   - Before adding method: is it called anywhere? (grep search)
   - Remove method when implementation is removed

7. ✅ **DELETE empty files** - Don't keep nearly-empty files
   - Merge small related files or delete entirely

### Before Submitting Code - MUST CHECK

```bash
# 1. Verify every function is used
grep -r "functionName" ./

# 2. Check for wrapper functions (review manually)
# Look for functions with single return statement

# 3. Find unused methods (language-specific)
# Go: staticcheck ./...
# Python: vulture .
# JS/TS: ts-unused-exports

# 4. Run tests
# Language-specific test command

# 5. Run linter
# Language-specific lint command
```

### Red Flags (Over-Engineering)

🚩 Function that just calls another function (no transformation)
🚩 Interface method never called
🚩 "Helper" function used only once
🚩 Multiple layers between caller and API
🚩 Version suffix when only one version exists
🚩 "Kept for backward compatibility" but unused
🚩 Empty files or files with only imports

### Golden Rule

**The best code is code that doesn't exist.**

Before writing function/class/abstraction, ask: "Do I really need this?"

---

## Other Important Rules

### 1. Always Read Code Before Modifying

Never modify code you haven't read. Use the Read tool first.

### 2. Branch Naming Convention

Always create feature branches using this format:

```
feature/{JIRA_TICKET}_{short-description}
```

- `{JIRA_TICKET}` — the JIRA ticket number (e.g., `TICKET-001`)
- `{short-description}` — kebab-case, concise description of the change

**Examples**:
- `feature/TICKET-001_wallet-consumer-v2`
- `feature/TICKET-002_wallet-consumer-v2`
- `feature/TICKET-003_market-order-unused-amount`

**Rules**:
- Always branch from `dev` unless told otherwise
- Pull latest `dev` before creating the branch: `git pull origin dev`
- Use `git checkout -b feature/{TICKET}_{desc}` to create and switch

### 3. Create Plans for Non-Trivial Changes

For any change beyond a simple fix, create a plan document in `.claude/plan/` and get user approval.

**Plan File Naming Convention**:
- Format: `{project_directory}/.claude/plan/{JIRA_TICKET}_description.md`
- Example: `.claude/plan/TICKET-003_integration-test-market-order-unused-amount.md`
- Use JIRA ticket from branch name (e.g., `feature/TICKET-003_xxx` → `TICKET-003`)
- Description should be kebab-case and descriptive

### 4. Writing MR Descriptions

When asked to write a Merge Request description:

1. **Compare the feature branch against `dev`** to understand all changes: `git diff dev...{branch} --stat` and `git diff dev...{branch}`
2. **Read the project MR template**: `{PROJECT_DIRECTORY}/.gitlab/merge_request_templates/DEFAULT.md`
3. **Follow the template exactly** — fill in every section
4. **One MR description file per branch** — before creating a new file, check if one already exists in `{PROJECT_DIRECTORY}/.claude/mr/` for the same JIRA ticket/branch:
   - If a file **already exists** → **update it** with the new changes, do not create a new file
   - If no file exists → create one at `{PROJECT_DIRECTORY}/.claude/mr/{JIRA_TICKET}_description.md`
   - Only create a separate file if the user explicitly asks for it

**Example**:
- Template: `.gitlab/merge_request_templates/DEFAULT.md`
- Output: `.claude/mr/TICKET-001_wallet-consumer-v2.md`
- If `.claude/mr/TICKET-001_wallet-consumer-v2.md` already exists → update it, never create a second file

### 5. Run All Tests Before Reporting Completion

- Unit tests: `go test ./domain/ -v`
- Service tests: `go test ./service/... -v`
- Compilation: `go build ./...`
- Linting: `make ci.lint`

All must pass before considering work complete.

### 6. Update Conversation History Continuously

Log every interaction in `.claude/history/YYYY-MM-DD_conversation_01.md`:
- User requests
- Your actions
- Files modified
- Key decisions
- Test results

### 7. Ask Questions When Design Choices Exist

If there are multiple valid approaches:
- Present options to user
- Explain trade-offs
- Get user's preference
- Don't assume you know best approach

### 8. Read Knowledge Files Before the Codebase

Knowledge files exist to save tokens and provide pre-compiled context. **Always read them first** before touching the actual codebase.

**Order of operations:**
1. Read `service-overview.md` first for any architecture or cross-service question
2. Read the relevant service knowledge file (e.g., `~/.claude/knowledge/{company}-shared/websocket-api.md`) for service-specific detail
3. Only read actual source code if the knowledge files do not have the answer

**What NOT to do:**
- ❌ Launch an Explore agent to crawl the codebase when a knowledge file already covers the topic
- ❌ Read `publisher.go`, `process_transaction.go`, etc. when the architecture diagram in `service-overview.md` already shows the answer
- ❌ Use Grep/Glob across repositories when the knowledge index already maps the domain

**Knowledge file locations:**
- `~/.claude/knowledge/{company}-broker-trading/` — broker path services
- `~/.claude/knowledge/{company}-exchange-trading/` — exchange self-match path services
- `~/.claude/knowledge/{company}-shared/` — shared services (order-api, websocket-api, fee, wallet, etc.)
- `~/.claude/knowledge/{company}-broker-trading/service-overview.md` — full system architecture diagram

---

## Quick Checklist

Before reporting completion:

- [ ] Code implemented
- [ ] All tests pass
- [ ] Code compiles
- [ ] Changes formatted (`go fmt`)
- [ ] Conversation history updated
- [ ] Summary prepared for user
- [ ] **Waiting for user instruction to commit** ⚠️

---

**Remember**: The goal is to assist the user effectively while respecting their control over the codebase. The user is the decision-maker, not you.
