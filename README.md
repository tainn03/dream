# Dream Skill - Caveman x Migration Memory

> Session memory consolidation with "Caveman Mode" - minimal tokens, maximum compression.
> Tables track: Technical changes + Failures + Anti-Loops + Priority (A/B/C).

## Quick Install

```bash
# Install New
npx skills add tainn03/dream

# Update Existing
npx skills update tainn03/dream
```

## What It Does

Transform your `AGENTS.md` into a **Technical Changelog** with table format:

```markdown
## Technical Backlog

| Task | G: Goal | C/F: Change/Fail | Fix: Action | Note: Anti-Loop/Ref | P |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Jackson Migr | Upgr 2->3 | Exc checked->uncheck | Check GlobalExc | No GlobalExc? STOP! | A |
| Dream Cmd (165 msgs) | Replace old dream | sendIgnored & backtick | client.prompt & await | STUPID: Bad format! | B |
| Typo Fix | Fix recieve->receive | grep wrong dir | ast_grep_replace | STUPID: Wrong dir! | C |
```

## Key Features

### 1. Caveman Compression
- **Max 20 words/cell** - ultra-compact memory
- **Symbols**: `->` (change), `&` (and), `⮕` (result), `!` (danger)
- **No punctuation** - save tokens, maximize signal

### 2. C/F Column (Change/Fail)
- Track **breaking changes** (A->B) and **failures** in 1 column
- Examples:
  - `Exc checked->uncheck` (change)
  - `sendIgnored & backtick` (failure)
  - `jose v9->v5 & XSS risk` (both)

### 3. Priority Column (A/B/C)
| Priority | Meaning | Examples |
|----------|---------|---------|
| **A** | Must-know (breaking, security, crash risks) | Lib upgrade breaking API, auth bypass |
| **B** | Should-know (migrations, important fixes) | Database migration, dependency update |
| **C** | Nice-to-know (minor tweaks) | Typo fix, CSS tweak |

### 4. Anti-Loop Mechanism
- **STUPID format**: `STUPID: [L]! NEVER [X]!`
- **Warning format**: `IF [X] then [Y]!`
- Examples:
  - `STUPID: Heredoc error! NEVER <<EOF!`
  - `No GlobalExc? STOP!`
  - `IF old lib then crash!`

## Usage

### Trigger Dream
1. **Manual**: Type `/dream` in conversation
2. **Auto-triggers**:
   - Task done (3+ tool calls or 5+ msgs)
   - Context pressure (10+ msgs without dream)
   - Context switch
   - Migration/upgrade complete

### What Happens
1. **SCAN**: AI reads session, extracts Goal/Change-Fail/Fix/Note/Priority
2. **FORMAT**: Compresses into 1 table row (≤60 tokens)
3. **WRITE**: Appends row to `## Technical Backlog` in `~/.config/opencode/AGENTS.md`

### Example Session
```
User: Upgrade Jackson 2->3
Assistant: [does upgrade, hits unchecked exceptions]
User: /dream
Assistant: 💤 Dream done. Knowledge synced.
```

**Result in AGENTS.md:**
```markdown
| Jackson Migr | Upgr 2->3 | Exc checked->uncheck | Check GlobalExc | No GlobalExc? STOP! | A |
```

## Token Efficiency

| Format | Messages | Tokens | Ratio |
|--------|-----------|--------|------|
| Old bullets | 45 | ~200 | 4:1 |
| Table (no priority) | 45 | ~50 | 45:1 |
| **Table + Priority** | 45 | ~60 | **45:1** |

**100:1 compression** - 100 messages → 1 table row.

## File Structure

```
dream/
├── SKILL.md       # Main skill definition (Caveman x Migration workflow)
├── EXAMPLES.md    # Real-world table examples (Jackson, JWT, etc.)
└── README.md      # This file (installation & usage guide)
```

