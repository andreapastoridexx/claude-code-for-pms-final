# Bringing quiet responders back

**Rook Dispatch · one-pager for Helen Achebe · draft, 9 Oct 2026**

## TLDR

Give responders back enough time to answer, stop treating a missed ping as if they'd said no, and restore the four reliable responders this has already pushed out of their own areas.

## The why

### The problem

1. **4.2 cut the time to answer from 90 seconds to 60.** Responders who can't reach their phone that fast now miss pings.
2. **Routing scores a miss exactly like a "no".** Each one costs 0.12 points, and only a yes (+0.08) earns points back. Nothing fades and nothing resets.
3. **So a few misses can sink a reliable responder for good.** Farlight, The Undertow, Vesper and Meteor Mite each missed 4–5 pings in the release week and dropped to a score of zero. In their own areas they used to be asked first 75–85% of the time. Now it's 4–18%. They get 1–3 pings a fortnight, too few to earn their way back.

To the responder, the phone just stops ringing, and nothing tells them why.

### How we know

- **Ping records (the 26 days after 12 Aug against the 44 days before):** the whole drop in acceptance (76.6% → 64.0%) is missed pings, which rose from 2.3% to 18.0%. Declines actually *fell*, from 21.1% to 18.0%, so people aren't saying no more often.
- **Routing code:** a miss and a "no" go through the same function on purpose, and the only thing that adds points back is a yes. Wen flagged this in 2019 and nobody came back to it:
  > *"should this ease back toward NEUTRAL_SCORE on its own after a while? For: somebody who had a bad month shouldn't still be carrying it in the spring. Against: if somebody has stopped taking work, we probably want that to stick…"* (`history.py`)

  Both sides are right, because a "no" is a choice and a miss usually isn't.
- **Interviews:** Vesper's phone is upstairs. 90 seconds was enough to reach it; 60 isn't.
- **Why it's easy to overlook:** acceptance has recovered to 72.7%, partly because the quiet four barely get pinged and have dropped out of the average. Their handlers have never filed a ticket.
- **What we ruled out:** distance and the reweight. The reweight actually softened the fall.

## Outcome (for the responder)

- **"I have time to answer."** A responder who's a minute from their phone can reach it before the callout moves on.
- **"Missing one isn't the same as saying no."** A slow week doesn't cost them their area or their place in the queue.
- **"My phone rings again."** Reliable responders are asked first in their own areas again, as they were before 4.2.

## How we'll do it

### Must do

| Change | Why |
|---|---|
| **1. Restore the 90-second timeout.** | 60 seconds clearly isn't enough, and the misses started when it was cut. |
| **2. Score a miss less harshly than a "no"** (a smaller penalty, or none). | Someone we didn't reach in time shouldn't be punished as if they refused. Wen and Marcus set the exact rule. |
| **3. One-time restore of the quiet four's scores.** | Changes 1 and 2 only affect future pings. The four already at zero get too few pings to recover by themselves. Wen has to do it, because the code has no reset. |

### Nice to have

| Change | Why |
|---|---|
| **4. Let miss penalties fade over time.** | One bad week shouldn't set someone's ranking for months. |
| **5. Phone app: "You missed a callout" message, plus a "My phone didn't ring" button.** | Responders know what happened. The button also tells us whether notifications are failing. |
| **6. Console: a "gone quiet" flag for handlers.** | A handler like Kip spots it without writing in. |
| **7. Track pings per responder alongside acceptance.** | Stops an average from hiding this again. |

### Not doing

- **Reverting 4.2 or the reweight.** That would hand the problem back to wide-area responders.
- **Changing the ranking weights.**
- **Availability Confidence.** It needs its own roadmap decision.
- **Separate issues:** overload on busy responders, console filter persistence, and push delivery (not confirmed as broken).
