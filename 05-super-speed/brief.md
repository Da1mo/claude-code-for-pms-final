# Bringing quiet responders back — one-pager

**To:** Helen Achebe · **From:** Rook Dispatch PM · **Date:** 7 Nov 2026 · **Status:** Draft for discussion
**Owner:** Rook Dispatch PM (accountable from decision through the 4-week check after launch) · **Engineering:** Wen Li, to confirm with Marcus

## The problem, in one paragraph
Since 4.2 (12 Aug), four responders — Farlight, Meteor Mite, The Undertow and Vesper — have almost stopped being pinged, while The Gale and Nightwell got busier. The cause is in how routing scores people: **a missed ping costs the same as a "no" (−0.12), a "yes" earns only +0.08, and the score never recovers on its own** (Wen's 2019 TODO). The shorter 60s ping wait raised misses from 2.3% to 18% of pings, so people who were busy or out of signal sank to the bottom without anyone — them or their handler — being told. Farlight went from 1.0 to 0 in three weeks; her last ping was 28 Aug.

## What we'd build instead
| # | Change | Why | What people notice |
|---|---|---|---|
| 1 | **A miss costs less than a no** (−0.04 vs −0.12) | Not answering in 60s isn't refusing. We want to reward saying yes, and not punish being busy as hard as saying no | Responders stop vanishing from the list after a bad week |
| 2 | **Busy for 30 min after taking a callout**, with "I'm available again" | We pinged people mid-job 56 times; the wiki says they're "marked engaged" but the code doesn't | Responder: no pings while suiting up. One tap to take a second job |
| 3 | **Responders set their own status** (Available / Busy until… / On leave) | Requested in tickets #3035 and #3143; better that they tell us than we guess | A clear control on the phone app |
| 4 | **Only count a miss if the ping reached the phone** | A dead phone or no signal shouldn't cost ranking | Fewer "gone before I could answer" moments |
| 5 | **Scores ease back toward neutral each week** (10%) | Nobody should carry a bad month forever; answers Wen's TODO | A quiet spell isn't permanent |
| 6 | **3 misses in 7 days → paused, plus "Still there?" check-in**; those 3 misses are forgiven | Treats absence as absence, not refusal, and tells the person | Responder: *"You've missed 3 pings. Still available? [I'm here]"*. Kip, in the console: *"Meteor Mite paused — waiting for check-in"* |
| 7 | **One-off reset** of the four to their 11 Aug scores | No rule change brings them back on its own; at ~1 ping/week Farlight would need ~10 weeks | Their handlers get a note saying what happened and that it's fixed |

The 60s ping wait stays as it is.

## Does it work? Replay of all 1,696 recorded pings (29 Jun–7 Sep)
| Score on 7 Sep | Today's rules | With changes 1, 2, 5, 6 |
|---|---|---|
| Farlight / Meteor Mite / The Undertow / Vesper | 0.00 / 0.00 / 0.00 / 0.00 | 0.64 / 0.65 / 0.60 / 0.62 |
| Other 12 responders — average (lowest) | 0.90 (0.72) | 0.94 (0.84) |

Nobody ends lower than today. The 30-min busy window blocks 21 pings (6 misses, 15 turn-downs) and keeps all 37 second jobs, assuming people tap "available again". Changes 3 and 4 can't be tested: there's no status or delivery data.

## How we'll know it worked
Checked weekly with Ravi; judged **4 weeks after launch** against the weeks before 4.2 (29 Jun–3 Aug):
| Signal | Before 4.2 | Since 4.2 | Target by week 4 |
|---|---|---|---|
| Missed pings, share of all pings | 2.3% | 18.0% | under 5% |
| Acceptance rate | 75–78%/week | 54–73% | 75% or above |
| The four sunk responders, pings/week | Farlight ~12 | ~3 or none | each back to at least half their pre-4.2 level |
| Unfilled callouts/week | 4–12 | 14–17 | 12 or fewer |
| "Responder gone quiet" tickets | 0 | 30 | none new after week 2 |

## Limits and risks
- The replay keeps history in the order it happened; it can't re-rank (no location data), so it shows score effects, not who would have been asked instead.
- Data ends 7 Sep and has no answer times. Live location for travel time is undocumented and may touch Security Policy 4.1.
- Only change 1 alters a scoring value. It's a deliberate choice about which behaviour we reward.

## What I need
- **Helen:** agree the direction, and that this replaces the "quick number change".
- **Wen:** rerun the replay against the real routing code; confirm where scores are stored.
- **Marcus:** can the app report ping delivery and a "busy" status?
- **Ravi:** answer times per ping and per-responder weekly pings, including after 7 Sep.
- **Next:** test the check-in and status screens with Kip, Linda Pruitt (Farlight) and Desmond Okafor (The Undertow).
