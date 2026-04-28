---
name: dream
description: Session memory consolidation with "Caveman x Migration" - tracks technical changes & failures. Table format in AGENTS.md with Priority (A/B/C). Triggered by /dream or task end. SCAN → FORMAT → WRITE.
---

# Dream - Technical Changelog & Anti-Loop

## Overview

Caveman x Migration: **SCAN → FORMAT → WRITE**

Tables track: Technical changes + Failures + Anti-Loops + Priority.

Every task end → 1 table row. C/F column combines Change & Fail.

## When to Use

- **`/dream` command** - User explicit trigger
- **Complex task done** - 3+ tool calls or 5+ msgs
- **Migration/Upgrade** - Breaking changes, lib upgrades
- **Context pressure** - 10+ msgs without dream
- **Context switch** - Save before moving on

## The Caveman Workflow

### Phase1: SCAN (Read-Only)

Read session. Extract:
- **G**: Goal (what tried)
- **C/F**: Change (A -> B) OR Fail (what broke & why)
- **Fix**: Action (what worked)
- **Note**: Anti-Loop (STUPID: [L]!) OR Warning (IF X then Y!)
- **P**: Priority (A/B/C)

**Caveman Rule**: Read only. No writing.

### Phase2: FORMAT (Compress)

Create 1 Markdown table row with Priority:

```markdown
| [Task] | [G: Goal] | [C/F: Change/Fail] | [Fix: Action] | [Note: Anti-Loop/Warning] | [P] |
```

**Priority Levels:**
- **A** = Must-know (breaking changes, critical anti-loops, security fixes)
- **B** = Should-know (important fixes, migrations, API changes)
- **C** = Nice-to-know (minor tweaks, non-critical updates)

**Caveman Compression Rules:**
- Max **20 words** per cell
- Use `&` not "and"
- Use `->` for changes (A -> B)
- Use `⮕` for results
- Use `!` for danger
- No extra punctuation (no commas, periods unless needed)
- Short words: `msg` not "message", `cfg` not "config"

**Examples:**
```markdown
| Task | G: Goal | C/F: Change/Fail | Fix: Action | Note: Anti-Loop/Ref | P |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Jackson | Upgr 2->3 | Exc checked->uncheck | Check GlobalExc | No GlobalExc? STOP! | A |
| Fix EOF | Update .md | cat <<EOF & error | use printf | STUPID: Heredoc. NEVER <<EOF! | C |
| Path Err | Move binary | cargo build default | --target-dir flag | STUPID: Manual move. NEVER! | B |
```

### Phase3: WRITE (Append Row)

Target: `~/.config/opencode/AGENTS.md` or other global AGENTS file.

**Location**: `## Technical Backlog` section

**Write process:**
1. Read `AGENTS.md`
2. Find `## Technical Backlog`
3. If no table → create header + separator with Priority column:
   ```markdown
   | Task | G: Goal | C/F: Change/Fail | Fix: Action | Note: Anti-Loop/Ref | P |
   | :--- | :--- | :--- | :--- | :--- | :--- |
   ```
4. Append new row at end of table
5. Use `edit` tool to insert row (not `write` entire file)

**Caveman Rule**: Edit only. Preserve other content.

## Note Column Format

**Anti-Loop format:**
```
STUPID: [L]! NEVER [X]!
```

**OR warning format:**
```
IF [X] then [Y]!
```

**Rules:**
- `STUPID:` prefix (not "STUPID MISTAKE:")
- `[L]` = What happened (max 5 words)
- `NEVER [X]!` = What to avoid (max 5 words)
- No period after `!`
- Use `?` for questions: `No GlobalExc? STOP!`

**Examples:**
- `STUPID: Heredoc error! NEVER <<EOF!`
- `No GlobalExc? STOP!`
- `IF old lib then crash!`

**Caveman Rule**: Failed 2+ times = STUPID. Document it.

## Multi-Session (Update Row)

**Same task, new session?**
→ Update existing row (don't add new row)

**Update format:**
- Append `& [new change/fail]` to C/F column
- Update Fix column if solved
- Keep Note column (first STUPID)
- Keep Priority (usually A stays A)

**Session reference:** Add `(see ses_abc123)` at end of Task name

Example:
```markdown
| Jackson Migr (ses_abc123) | Upgr 2->3 & test | Exc checked->uncheck & miss handler | GlobalExc & test | No GlobalExc? STOP! | A |
```

## Priority Guidelines

| Priority | When to Use | Examples |
|----------|------------|---------|
| **A** | Breaking changes, security, critical bugs | Lib upgrade breaking API, auth bypass, data loss risk |
| **B** | Migrations, important fixes, API changes | Database migration, dependency update, refactoring |
| **C** | Minor fixes, typos, non-critical | Typo fix, CSS tweak, log message change |

**Caveman Rule**: When in doubt, use B. A is reserved for "session will crash without this".

## Red Flags (Caveman No-Nos)

| Anti-Pattern | Why Stupid |
|--------------|------------|
| Bullet points in AGENTS.md | Vertical bloat. Use table! |
| Paragraph in cell | >20 words. Compress! |
| Missing Note column | Will repeat. Add STUPID! |
| New row for same task | Update row. Don't duplicate! |
| "I think" in cell | Not caveman. Fact only! |
| Periods in cells | Extra tokens. Remove! |
| Wrong Priority | A for typos? Stupid! |

## Verification

After updating AGENTS.md:
- [ ] Row added to `## Technical Backlog` table
- [ ] Max 20 words per cell
- [ ] Note column has `STUPID:` or `IF X then Y!`
- [ ] Priority column has A/B/C
- [ ] Uses `->`, `&`, `⮕`, `!` symbols
- [ ] No extra punctuation in cells
- [ ] Other content in AGENTS.md preserved
- [ ] Used `edit` not `write`

## Caveman Manifesto

> "Ugh. Task done. Row added. Changes tracked. No stupid twice."
>
> — Caveman Developer (2026)

**Token budget:**
- SCAN: Read only, no limit
- FORMAT: ≤ 60 tokens per row (with Priority)
- WRITE: One edit operation

**Compression ratio**: 100:1 (100 msgs → 1 table row)

---

**Tables = Scannable. Changes = Tracked. Priority = Focused. Caveman = Efficient.**
