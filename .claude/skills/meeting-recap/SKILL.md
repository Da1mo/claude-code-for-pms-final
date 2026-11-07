---
name: meeting-recap
description: Reads meeting notes and pulls out the two things that matter afterwards, the decisions that were made and who owns each follow-up, in the same fixed format every time. Use it whenever the user points at meeting notes, minutes, a call transcript, a standup or retro write-up, a handoff doc or a wiki meeting page and wants a recap, the decisions, the action items, the next steps, "who's doing what" or "what did we agree", even if they don't say "recap".
---

# Meeting recap

The reader of this recap already knows the meeting happened. They want to know
two things without rereading the notes: **what was decided** and **who owes
what**. Everything else (the discussion, the background, who said what) is
noise for this purpose, so leave it out. The value of the skill is that the
read is the same every time, so follow the format below exactly.

## 1. Get the notes

- The user may give a file path (.md, .txt, .docx, .pdf), pasted text, or a
  wiki page or link. Read the whole thing before writing anything: decisions
  are often made near the end, after a long discussion that seems to go the
  other way.
- For a .docx, extract the text first (for example `pandoc -t plain`, or the
  docx skill if it's available). For a .pdf, use the pdf skill.
- If several meetings are in one file, make one recap per meeting, in the
  order they appear.
- Find the meeting's name and date if the notes give them. Don't guess.

## 2. Sort what you read

**A decision** is something the group settled: agreed, approved, chose,
ruled out, deferred on purpose, or decided not to do. Signals: "we agreed",
"decided", "going with", "approved", "won't", "parked until", "final call".

Not decisions, and so left out of the Decisions section:
- a proposal or recommendation nobody confirmed ("I think we should…")
- an opinion, however senior the person ("Priya feels it's seasonal")
- a question raised but not answered

If something sounds settled but the notes don't show agreement, put it under
**Open or unclear** instead. Calling an unconfirmed idea a decision is the
worst mistake this recap can make, because people will act on it.

**A follow-up** is a piece of work someone will do after the meeting. Its
**owner** is the person the notes assign it to or who volunteered ("I'll…",
"Wen to…", "@Marcus", "Owner: Ravi").

- Use the owner's name exactly as the notes write it. If the notes only give
  a role ("the support lead"), write the role. In a note written by one
  person to another (a handoff, an email), "I" and "you" are people too:
  write who they are ("Priya", "Incoming PM").
- Advice like "worth doing" or "I'd look at X first" is not an assignment.
  Put it under **Open or unclear** as a suggestion, not as a follow-up.
- If no one is named, the owner is **Unassigned**. Don't infer an owner from
  who raised the topic or whose area it sounds like. A wrong owner is worse
  than a blank one, because the real owner assumes it's handled.
- If two people share it, list both. If it's assigned to a team or to someone
  outside the company (a customer), write that and flag it under
  **Open or unclear**.
- **Due**: copy the deadline as written. If the notes give a relative date
  ("next Friday") and the meeting date is known, convert it to a date
  (e.g. "Fri 14 Nov 2026"); otherwise write it as given. No deadline → "—".

Merge items that are the same action said twice. Keep each item to one line,
in plain words close to the notes' own.

## 3. Write the recap

Always use this template, in this order, with these headings, even when a
section is empty (write "None recorded." so the reader knows you checked):

```markdown
# Meeting recap: <meeting name or "Untitled meeting"> · <date or "date not given">

Source: <file name, page title or "pasted notes">

## Decisions
| # | Decision | Agreed by |
|---|---|---|
| D1 | <what was decided, one line> | <names/roles, "the group", or "not stated"> |

## Follow-ups
| # | Action | Owner | Due |
|---|---|---|---|
| F1 | <verb-first action, one line> | <name, or **Unassigned**> | <date or —> |

## Open or unclear
- <proposals not agreed, questions left open, actions with no owner or an owner outside the team, contradictions in the notes>

**Counts:** <n> decisions · <n> follow-ups (<n> unassigned) · <n> open items
```

- Number decisions D1, D2… and follow-ups F1, F2… in the order they come up
  in the notes, so the user can refer to them ("F3 has no owner").
- If a follow-up comes from a decision, add "(from D2)" at the end of the
  action.
- Quote the notes only when the wording matters, in under 15 words.
- No summary of the discussion, no recommendations, no commentary. If the
  user wants analysis, they'll ask for it separately.

## 4. Where to put it

Show the recap in the chat. Only save it to a file if the user asks; then
save it next to the notes as `<notes file name>-recap.md` unless they name
another place.
