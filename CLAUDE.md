# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Answer only the question the student asks, from the sources they
  name. Don't investigate beyond it or offer a theory about what
  Release 4.2 changed unless they ask for one.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I'm the new PM for Rook Dispatch, taking over from Priya Raghunathan,
who left on 21 Aug 2026 without overlapping with me. Sources: the
rook-wiki, the rook-database, `00-rook/code/dispatch-routing/` and
`00-rook/company/notes/handoff-from-priya.docx`.

### Company and products
- Rook sells software to masked responders and their handlers, billed
  per active responder. 241 staff, mostly remote. Monthly 4.x releases.
- **Confidentiality is a hard rule:** Rook never holds a responder's
  legal identity. Never design or analyse anything that assumes it does
  (Security Policy 4.1).
- **Rook Dispatch** (mine): ranks available responders and pings them
  about callouts. Handlers use the desktop console; responders use the
  phone app. Routing is where the risk is. There's no written description
  of it, and Priya asked me to write one.
- **Rook Supply**: requisitions, maintenance and failure reports, used by
  handlers and quartermasters. It reads Dispatch's Responder Availability
  Record to schedule maintenance.

### Vocabulary
- **Responder** takes callouts (not an employee). **Handler** looks after
  one or a few responders and is usually who uses the product.
  **Quartermaster** approves gear (Supply).
- **Callout** is the unit of work. **Ping** is one callout offered to one
  responder, with three outcomes: **taken**, **turned down** or **missed**
  (no answer within the **ping wait**, which is the same for everyone).
  Both turned down and missed pass the ping on.
- **Acceptance rate** (headline metric) is the share of pings taken.
  **Time-to-accept** is the median seconds to take a ping. **Coverage
  gap** means no available responder had the required tags.
- **Routing priority** ranks responders on proximity (travel time),
  recent acceptance and capability match. **Capability tags** include
  flight, structural-entry, hazmat-tolerant and cold-weather.
  **Mutual aid** (cross-area cover) isn't supported yet.

### People (wiki comments use roles)
| Role | Person | Notes |
| --- | --- | --- |
| Engineering manager | Marcus Oyelaran | Runs Dispatch eng. Start here |
| Staff engineer | Wen Li (she) | Built routing |
| Support lead | Nadia Hoffmann | Tickets. Worth a standing 15 min |
| Product designer | Sofia Marino | Console and phone app. Ran the Sep interviews |
| Director of Product | Helen Achebe | My director. Owns the roadmap |
| Data analyst | Ravi Menon | Owns the weekly acceptance numbers |

Customers who come up: Halloran (Sgt. Bulwark; mostly Supply), Aunt Dot
(Vesper), Mr. Ambrose (Captain Vantage), Kip (Meteor Mite, The Gale).

### Releases and where 4.2 stands
- **4.0** (7 Apr): console navigation, profile redesign, override audit
  log. **4.1** (16 Jun): travel-time proximity, bulk callout, push
  reliability.
- **4.2** (12 Aug): proximity weight up (0.45→0.60) and recent acceptance
  down (0.40→0.25), ping wait 90→60s, filters persist.
- Since 4.2, tickets have run at about 3x normal. They split roughly 2/3
  "phone never goes off" and 1/3 "gone before I could answer."
- Marcus asked whether the routing change was meant to cover responders
  who've been turning jobs down. Nobody has answered.
- Priya's view is that it's mostly a seasonal August dip, and that it
  shouldn't become a revert debate. That's her opinion, not a finding.

### Roadmap (Q3, last reviewed 30 Jun, every owner still "Priya")
- 4.2, committed: change to who gets pinged (shipped), ping timeout
  tuning (shipped as 90→60s), and **Availability Confidence (not
  shipped)**.
- 4.3, committed: requisition approval chains (Supply).
- Q4, exploring: handler phone app and shared cover.
- To do: agree with Helen what's still committed, fix the owners, and
  write the routing description.

### Contradictions and gaps (wiki vs DB vs code, checked 24 Oct 2026)
- **Missed = turned down in the code:** the glossary says they're
  recorded separately, and the DB does keep them apart. But in
  `offer.py` and `history.py` both cost −0.12, a yes earns only +0.08,
  and the score never recovers on its own (Wen's 2019 TODO was never
  decided). The config doesn't treat people who turn jobs down
  differently, which answers Marcus.
- **Capability tags rank but don't filter** (15% of the score). With no
  tag data anywhere, the coverage-gap metric can't be computed.
- **Bulk callout** shipped in 4.1, but the code and the DB send pings one
  at a time. The Jan brief still says it isn't picked up.
- **"August is always soft" can't be checked:** the DB starts 29 Jun.
  Take rate was 75–78%/week through 3 Aug, then 54% in the release week,
  ~66% for two weeks, and 73% in the week of 31 Aug.
- **Filter tickets aren't all noise:** 13 since 4.2, including resets
  after updates (Ambrose had two).
- **Numbers:** Ravi owns them, but the handoff says ask Marcus. Nobody
  has asked Ravi about 4.2.
- **Requisition approval chains:** the brief adds a second approver, but
  Halloran's interview complaint is speed (11 days waiting). Owner is
  given as Priya, the Supply team, or "Halloran's team"; Halloran is a
  customer.
- **No record of what was cut from 4.2:** Releases omits deferred items.
- **Missing data:** no answer time on pings (so no time-to-accept), no
  per-responder reporting, and the DB ends 6–7 Sep.
- **Per responder after 12 Aug** (pings/week): Farlight ~12→3 (last ping
  28 Aug), with The Undertow, Meteor Mite and Vesper also down sharply,
  and Nightwell up ~15→19. Unfilled callouts went from 4–12/week to
  14–17. Incoming callouts dipped from ~140 to 110 in the release week,
  unexplained (ask Ravi).
- **Course files** for modules 4–6 state later-module conclusions. They
  aren't evidence.

- Module 1 (24 Oct 2026): the DB has five tables (callouts, pings, responders, handlers, support_tickets), covering 29 Jun–7 Sep 2026. It has no capability tags, availability or answer times.
- The ticket themes in the DB match Nadia's split: since 4.2, about 26 "quiet / nothing coming" tickets vs 11 "vanished before he could answer."
- The Sep 2026 customer interviews (wiki > Research, run by Sofia) fit the per-responder data. Kip says Meteor Mite is quiet while The Gale is busy, Aunt Dot describes both quiet weeks and pings vanishing for Vesper, and Ambrose says Captain Vantage lost one mid-suit-up.
- Open questions: whether the routing change was meant to hit responders who decline (ask Wen), why incoming callouts dipped in the release week (ask Ravi), and which 4.2 items are still Q3 commitments (ask Helen).
- Most likely 4.2 story (my read, not confirmed): the 60s ping wait caused a wave of missed pings. Because a miss is scored like a "no" with no recovery, some responders sank in the ranking until they stopped being reached. The proximity change likely shaped who got busier but doesn't explain the release-week drop. The seasonal explanation isn't supported by the data.
- No data from Ravi exists yet: his wiki page is empty, and all the 4.2 numbers so far are my own counts from the raw DB. To ask him for: time-to-answer per ping, per-responder weekly pings and scores, and data after 7 Sep.
- Travel-time proximity can't be checked: `travel_time_minutes()` is a placeholder in the repo, and the DB has no locations.
- Terminology: "tickets" are support complaints, while the system routes "callouts" and offers them as "pings".
- Module 2: all 147 tickets were grouped by hand. Since 4.2 there are 30 "responder gone quiet" and 15 "vanished before answering" (exactly Nadia's 2:1 split), up from zero of each before. The quiet tickets come from only 4 handlers (Farlight, The Undertow, Halfmoon, Corporal Ashgrove), who keep following up. 14 filter tickets since 4.2, about half of them real bugs. Most other tickets come in identical pairs, which looks like a quirk of how the data was put together.
- The interviews and the tickets come from different people: Dot, Kip and Halloran filed 0 tickets, Ambrose filed 1, and none of the 11 main ticket filers were interviewed. Vesper and Meteor Mite went quiet in the data but have no tickets, and overload (The Gale, Nightwell) appears in no ticket at all.
- Meteor Mite and The Gale share a handler and an area (Eastgate) but went opposite ways after 4.2, so proximity doesn't explain the split; acceptance history does. Not everyone with more misses sank, though (Captain Vantage and Sgt. Bulwark got busier), so the timing of misses likely matters.
- Contradiction: Halloran says maintenance scheduling got smarter, but tickets show suits booked for servicing on city-marathon day.
- My priority list (impact × reach × confidence ÷ effort): first fix the 60s ping wait and the miss-as-decline scoring with no recovery, together with a reset for affected responders. Then get real measurement from Ravi, then roadmap and owner cleanup. Next interviews should include Linda Pruitt (Farlight) and Desmond Okafor (The Undertow), and ask directly about callouts.
