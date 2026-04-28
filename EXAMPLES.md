# Dream Skill Examples (C/F + Priority Format)

## Example1: Simple Task

**Scenario**: Fixed CSS button alignment.

**Before `/dream`:**
```
User: Fix button alignment
Assistant: [edit CSS] [lsp_diagnostics] Fixed.
```

**After `/dream` (row added to table):**
```markdown
| Task | G: Goal | C/F: Change/Fail | Fix: Action | Note: Anti-Loop | P |
| :--- | :--- | :--- | :--- | :--- | :--- |
| CSS Fix | Fix button align | Edit Button.tsx & wrong | goto_definition first | STUPID: Wrong file! | C |
```

---

## Example2: Complex Task (Migration)

**Scenario**: JWT auth across 5 files - lib upgrade.

**Table row:**
```markdown
| JWT Auth | Add auth & refresh | jose v9->v5 & XSS risk | jose v5 & HttpOnly | IF old lib then crash! | B |
```

---

## Example3: Repeated Failure + Breaking Change

**Scenario**: Jackson 2->3 migration, exceptions unchecked now.

**Table row:**
```markdown
| Jackson Migr | Upgr 2->3 | Exc checked->uncheck | Check GlobalExc | No GlobalExc? STOP! | A |
```

**Why Priority A?** Breaking change - sessions will crash without GlobalExceptionHandler.

---

## Example4: Session Continuation

**Scenario**: Task spans sessions.

**Session1 row:**
```markdown
| Context Pruner (ses_abc123) | Build pruner & system | Regex slow & AST crash | tree-sitter & fallback | STUPID: Slow regex. USE ast! | B |
```

**Session2 (update same row):**
```markdown
| Context Pruner (ses_abc123 & ses_def456) | Build pruner & system | Regex slow & AST crash & no Windows | ast-grep WASM & gh search | STUPID: Slow regex. USE ast! | B |
```

*Note: Append to Task column for session refs. Append to C/F column for new fails.*

---

## Example5: Caveman Extreme

**Scenario**: Quick typo fix.

**Table row:**
```markdown
| Typo Fix | Fix recieve -> receive | grep wrong dir & miss | ast_grep_replace pattern | STUPID: Wrong dir! | C |
```

---

## Full Table Example (AGENTS.md)

```markdown
## Technical Backlog

| Task | G: Goal | C/F: Change/Fail | Fix: Action | Note: Anti-Loop/Ref | P |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Jackson Migr | Upgr 2->3 | Exc checked->uncheck | Check GlobalExc | No GlobalExc? STOP! | A |
| JWT Auth | Add auth & refresh | jose v9->v5 & XSS risk | jose v5 & HttpOnly | IF old lib then crash! | B |
| CSS Fix | Fix button align | Edit Button.tsx & wrong | goto_definition first | STUPID: Wrong file! | C |
| Dream JSON | Replace old dream | <<EOF & backtick & wrong | client.prompt & await | STUPID: Bad format! | B |
| Context Pruner (ses_abc123) | Build pruner & system | Regex slow & AST crash | tree-sitter & fallback | STUPID: Slow regex! | B |
| Typo Fix | Fix recieve -> receive | grep wrong dir & miss | ast_grep_replace pattern | STUPID: Wrong dir! | C |
```

**Benefits:**
- 6 memories = 7 table rows (header + 6 data) = ~400 chars
- Priority column = instant filtering (A=must read, C=skip if context tight)
- C/F column = track both changes & fails in 1 cell
- Scannable in 1 second

---

## Token Comparison

| Format | Messages | Tokens | Ratio |
|--------|-----------|--------|------|
| Old bullets | 45 | ~200 | 4:1 |
| Table (no priority) | 45 | ~50 | 45:1 |
| **Table + Priority** | 45 | ~60 | **45:1** |

**Caveman Rule**: Priority A = read first. Priority C = skip if in hurry.

---

## Priority Decision Tree

```
Is this a breaking change?
├─ YES → Priority A
└─ NO → Is this important (migration, security, API change)?
    ├─ YES → Priority B
    └─ NO → Priority C
```

**Examples:**
- Jackson 2->3 (breaking) → A
- JWT upgrade (security) → B
- Typo fix (minor) → C
- Missing GlobalExcHandler (crash risk) → A
- CSS button fix (non-critical) → C
