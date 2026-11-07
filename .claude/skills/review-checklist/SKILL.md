---
name: review-checklist
description: Reviews a product brief (one-pager, PRD, proposal, spec or decision doc) against four checks — it names who owns it, says how we'll know it worked, keeps the same scope from start to finish, and explains the problem before the fix — and returns the same pass/fail checklist every time. Use it whenever the user points at a brief or asks to review, check, sanity-check or critique one before sharing it, even if they don't say "checklist".
---

# Review checklist

The user wants a quick, repeatable read on whether a brief is ready to share.
Run exactly these four checks, in this order, and nothing else. A consistent
checklist is the point: the same brief should get the same result every time,
and briefs can be compared with each other. Don't add a general critique,
rewrite the brief, or comment on the idea itself unless the user asks.

## 1. Read the brief

- Accept a file path (.md, .txt, .docx, .pdf), pasted text or a wiki page. For
  .docx extract the text first (for example `pandoc -t plain`); for .pdf use
  the pdf skill.
- Read the whole brief before judging. Scope drift and a buried problem
  statement only show up when you compare the start with the end.

## 2. The four checks

Each check gets one result: **Pass**, **Partial** or **Fail**. Give evidence
for every result: a short quote (under 15 words) or the section it's in, so
the user can check your call. If something is missing, say where you'd expect
to find it.

**1. Names who owns it.** Pass if one named person or role is clearly
accountable for the work (an "Owner:" line, "I'll own this", a DRI).
Partial if there's only an author or sender ("From: …"), several people with
no lead, or a team with no person. Fail if no one is named. An author isn't
automatically the owner: the question is who answers for it after it ships.

**2. Says how we'll know it worked.** Pass if there's at least one measurable
signal of success: a metric with a target or direction and a timeframe
("acceptance back above 75% within 4 weeks"). Partial if success is described
but not measurable ("responders feel better"), a metric has no target, or the
only evidence is a test or simulation run before launch. Fail if nothing says
what success looks like. Evidence that the idea should work is not the same as
a way to tell, after launch, that it did.

**3. Keeps the same scope start to finish.** Compare what the opening says
the brief covers with what the solution, results and asks actually cover.
Pass if they match. Partial if something appears in one place but not the
other: a fix with no matching problem, a problem the solution doesn't address,
an ask that's outside the brief, or results that test only part of what's
proposed and don't say so. Fail if the brief ends up proposing something
materially different from what it set out to do. List each mismatch.

**4. Explains the problem before the fix.** Pass if the problem (who it hurts,
what's going wrong, and ideally evidence) comes before any proposed solution.
Partial if a solution appears first (in the title, summary or first lines)
and the problem is explained only afterwards, or the problem is stated with
no evidence. Fail if there's no problem statement, only a solution.

## 3. Output

Always use this format:

```markdown
# Review checklist: <brief title> 

Source: <file name or "pasted brief">

| # | Check | Result | Evidence |
|---|---|---|---|
| 1 | Names who owns it | Pass / Partial / Fail | <quote or location; what's missing> |
| 2 | Says how we'll know it worked | … | … |
| 3 | Keeps the same scope start to finish | … | … |
| 4 | Explains the problem before the fix | … | … |

**Result:** <n> of 4 pass

## To fix
- <for each Partial or Fail: one concrete line saying what to add or change, and where>
```

- If all four pass, write "Nothing to fix." under **To fix**.
- Keep fixes to what would make the check pass. Don't draft the missing
  section unless the user asks for it.
- Show the result in the chat. Save it to a file only if the user asks; then
  name it `<brief file name>-review.md`, next to the brief.
