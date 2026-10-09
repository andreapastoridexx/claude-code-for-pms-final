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
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

The user is the new PM for **Rook Dispatch**. They joined with no overlap
with the previous PM (Priya, who left; her handover is dated 21 Aug 2026).

**Where this comes from.** At the time of writing, `00-rook/company/` held
only one file: `notes/handoff-from-priya.docx`. Everything below comes from
that file. It's one person's account, and parts of it are her opinion. Where
something is her judgement and not a fact, it says so. Other sources haven't
been read yet: `00-rook/code/`, `00-rook/feedback/`, the rook-wiki and the
rook-database.

### The product

- **Dispatch** is Rook's flagship product, and it's the reason responders
  stay. The core loop: an **incident** comes in → the system **ranks**
  available responders → it **offers the callout (pings)** the top-ranked
  one → they **accept or decline**.
- Surfaces: a **console** (stable) and **mobile** (stable since 4.1).
  **Routing**, the ranking logic, is where both the interesting work and
  the risk are.
- **Headline metric: acceptance rate**, meaning the share of pings that get
  taken. Everyone watches it, so be ready to explain what moves it.
- **There is no written spec for how routing ranks responders.** It lives in
  the staff engineer's head. Priya asked for one to be written; it's an open
  task for this PM.

### People (the handover gives roles, not names; fill names in as we learn them)

| Role | What to go to them for |
|---|---|
| **Director of Product** | The user's manager. Gives people room. Owns the call on which Q3 commitments still stand. |
| **Engineering manager (Dispatch)** | Runs Dispatch engineering. Direct; will say when an idea is bad. First stop when unsure. Also the current way to get **data pulls**. |
| **Staff engineer** | Built the routing/ranking logic. Understanding it means talking to her, because nothing is written down. |
| **Support lead** | Hears handler complaints first. Priya suggests a standing 15-minute slot. |
| **Priya** | Previous Dispatch PM, the only PM on it for 14 months. Gone, no overlap. |

### Vocabulary

- **Responder**: the person who gets pinged and takes or turns down a callout.
- **Handler**: a different group from responders. Handlers write in to
  support. *Inferred, not stated:* they probably work in the console,
  managing incidents.
- **Incident**: the event that starts a dispatch.
- **Callout / ping**: the offer of an incident to a responder.
- **Ping timeout**: how long a responder has to answer before the offer moves on.
- **Acceptance rate**: the share of pings accepted. The north-star number.
- **Acceptance history**: a responder's past acceptance record. It's one of
  the routing inputs.
- **Proximity**: how close a responder is to the incident. Another routing input.

### Where things stand (as of the handover, 21 Aug 2026)

**Release 4.2 shipped on 12 Aug 2026 and is "the thing on fire."** It
changed two things at once:
1. **Routing reweight:** proximity now counts for more relative to recent
   acceptance history. Responders covering wide areas had asked for this
   for three quarters: nearby people were sitting unoffered while the system
   pinged someone 40 minutes away with a better record.
2. **Shorter ping timeout.**

(It also included a **console filter persistence** change.)

**Since 4.2:** fewer pings are being accepted, and more handlers are
complaining.

**Priya's read (her opinion, not verified):** it's mostly seasonal, because
August is soft every year, and she expected it to recover in September. She
strongly advised against framing this as "revert 4.2," since that would swap
one unhappy group of responders for another. She suggested ruling out
seasonality before digging into the routing change.

**Things to keep in mind when analysing this:**
- At least three causes are tangled together: seasonality, the routing
  reweight and the timeout cut. Don't credit the change to any one of them
  without separating them.
- September is over now, so her seasonal prediction can be tested. Compare
  against previous Augusts and Septembers.
- Priya admits she "made calls faster than I checked them." Treat her
  conclusions as hypotheses.

**Open items she left:**
- **Features cut from 4.2:** some features were dropped when the timeline
  shrank. Which ones are still Q3 commitments hasn't been agreed with the
  Director of Product. That conversation is overdue, because Q3 has ended.
- **Console filter persistence tickets:** Priya calls them cosmetic noise
  and says not to let them take over the first month.
- **Write the routing/ranking spec.**
- Use the first month's fresh eyes on the parts of the product nobody has
  looked at closely. Priya thinks her unchecked calls are most likely to be
  hiding there.

### Learned in session 1 (6 Oct 2026), from the rook-wiki and `00-rook/code/`

- **Names:** Helen Achebe is the Director of Product. Marcus Oyelaran is the
  engineering manager. Wen Li is the staff engineer who owns routing; she was
  away 14–24 Aug. Nadia Hoffmann is the support lead. **Ravi Menon** is the
  data analyst who owns the weekly acceptance numbers, so go to him for data
  rather than Marcus. **Sofia Marino** is the designer for the console and
  phone app, and she ran the customer interviews in September.
- **The console** is the handlers' web app. Handlers enter incidents, watch
  coverage, override individual routing decisions, and set responders'
  availability windows and capability tags. Handlers can't change routing
  settings; those ship with each monthly release. Responders use the phone app.
- **What was cut from 4.2:** Availability Confidence, a confidence score shown
  next to a responder's stated availability. It's still marked Committed on
  the Q3 roadmap, which hasn't been reviewed since 30 Jun. Under the roadmap
  rules, changing a committed item has to go through Product (Helen).
- **Support tickets since 4.2** run at about 3x normal. Roughly 2/3 say "my
  phone never goes off" and 1/3 say "it was gone before I could answer". The
  60-second timeout explains the second group but **not the first**.
- **Open question:** on 14 Aug, Marcus asked whether the routing change also
  boosts responders who keep declining jobs, since the config doesn't seem to
  tell them apart. Nobody has answered. This is a lead for the "phone never
  goes off" complaints, but it isn't verified yet.
- The team planned to regroup on 4.2 once the new PM was settled. Nadia has a
  ticket breakdown ready.
- **Not yet read:** the customer interviews database, the product briefs, the
  4.0 and 4.1 release pages, the Glossary, the handler records, the
  dispatch-routing code (`00-rook/code/dispatch-routing/`, which has the
  ranking weights in `config.py`), and the rook-database.

### Learned in session 2 (7 Oct 2026), from interviews, tickets, pings and the routing code

- **Sources:** `00-rook/feedback/` is empty. The 4 handler interviews (2–5 Sep) are in the rook-wiki "Customer interviews" database. rook-database has `support_tickets` (147, 29 Jun–7 Sep), `callouts`, `pings`, `responders` and `handlers` (from 29 Jun). There are no travel times, scores or prior years, so ask Ravi for past Augusts and Septembers.
- **The whole acceptance drop is missed pings.** Before vs after 12 Aug: acceptance 76.6% → 64.0%, missed 2.3% → 18.0%, declines *fell* 21.1% → 18.0%, callouts covered 94.5% → 88.9%. The timeout went from 90s to 60s. Falling declines count against Marcus's "boosts decliners" idea and against seasonality as the explanation for acceptance.
- **Stuck responders:** Farlight, The Undertow, Vesper and Meteor Mite are down about 85% in pings; Ashgrove and Halfmoon about 55%. They were reliable, missed 4–5 pings in the 4.2 week, and a miss costs the same as a decline (−0.12, vs +0.08 for a yes). Scores never ease back toward neutral (`history.py` TODO from 2019). Now neighbours outrank them *in their own areas*. It's not distance; that guess was wrong.
- **Tickets vs interviews:** "Phone quiet" (30 tickets) affects 4 responders; "gone before I could answer" (15) is spread across 11. Overload (Kip's The Gale) only shows up in interviews. Filter persistence has 12 complaints since launch, including silent resets, so it's not purely cosmetic. Only Captain Vantage appears in both sources.
- **Proposal:** restore the timeout, reset the scores of responders who crashed after 12 Aug, and decide whether a miss should count as a decline. Keep the reweight. Take it to Marcus and Wen. **Open:** why the timeout was cut. Next interviews should include Linda Pruitt (Farlight) and Desmond Okafor (The Undertow).

### Learned in session 3 (9 Oct 2026), from pings, support_tickets, callouts, the interviews and the routing code

- **Acceptance is recovering, but that hides the quiet responders.** Weekly acceptance: 46.8% (12–16 Aug) → 65.8 → 66.7 → 72.7% (31 Aug), against about 77% before. Missed: 29.4 → 17.7 → 14.8 → 12.7%. Most extra misses came out of turn-downs (21% → 14.5%), not out of yeses. The other 12 responders are nearly back to normal. The quiet ones barely get pinged, so they've almost vanished from the metric. Track pings per responder alongside acceptance.
- **Six responders went quiet, measured per day** (before = 44 days, after = 26; earlier raw-count percentages compared unequal windows and overstated the drop). Farlight −75%, The Undertow −68%, Vesper −67%, Meteor Mite −66%: they miss more than half their pings now and lost their own area (asked first 75–85% → 4–18%). Halfmoon and Ashgrove are only down about 20% and still get asked first in their own areas.
- **Tickets show who writes in, not who's hurt.** Aunt Dot (Vesper), Kip (Meteor Mite, The Gale) and Halloran have filed zero tickets, ever. Mr. Ambrose has filed one. These are exactly the four handlers Sofia interviewed. Both Dot and Kip described the quiet problem in interviews but treated it as normal. Ticket times match individual pings exactly.
- **Vesper's misses (from Dot's interview):** his phone is upstairs, and he can't get downstairs within 60 seconds, though 90 was enough. This backs the "slow answerer" explanation. It's one account; check with Linda and Desmond.
- **The reweight didn't cause the quiet:** it cut the weight on acceptance (0.40 → 0.25), which softened the fall. The quiet responders were pinged normally in the release week. Their pings dropped after misses, scored as declines, pushed their scores down. Callout volume is about 10–12% lower after 12 Aug, which may be seasonal. Seasonality doesn't explain the rates. **Open:** prior-year Aug/Sep from Ravi, real score history from Wen, response times from app logs.

### Learned in session 4 (9 Oct 2026), from `00-rook/code/dispatch-routing/`, the 4.1/4.2 wiki release pages and pings

- **How routing works:** whoever is free in the area gets scored on travel time (60%; zero at 45+ min, linear below), yes-history (25%) and skill match (15%), and is buzzed one at a time, best first, for 60s. Nobody is removed, only reordered. Code that finds who's free, estimates travel time and actually sends the buzz is only stubbed in this folder, so delivery problems can't be ruled out here. Rook Supply reads the availability records, so tell Supply before changing them.
- **The history score:** starts at 0.5. A yes adds 0.08; a no or a miss takes 0.12 (deliberately treated the same, `offer.py`/`history.py`). **The only thing that adds points back is a yes:** no fading, no reset, no refunds. The break-even is a 60% yes rate. A reset of the quiet four can't be done with what's in the code; Wen would have to do it.
- **Replayed scores from pings (my reconstruction, not stored values):** on 12 Aug all 16 responders were 0.80–1.00. Lowest before 4.2 were Halfmoon (0.89 avg) and Meteor Mite (0.90), the two highest decliners (~28%), a gap worth about 1½ min of travel. Now Farlight, The Undertow, Vesper and Meteor Mite are at 0.00 and got 1–3 pings each with zero yeses in 24 Aug–6 Sep. Ironvale, Falkirk, The Drift and Stormwrack slipped to 0.72–0.84. Recovering alone would take months, so the proposal needs both the 90s timeout and a manual reset.
- **Marcus's 14 Aug question is answered:** the weights apply to everyone the same, and nobody had a poor record at release. The reweight shrinks the weight of everyone's record: a zero-score responder now needs ~19 min closer to beat a perfect one, against ~40 before. That softened the quiet four's fall. A draft reply was written for Marcus; it hasn't been sent.
- **4.2 also had 3 bug fixes** missing from the code changelog, including "duplicate push notification when a ping is sent again". That fix is a possible second cause of "phone never goes off", **unverified**. 4.1 (16 Jun: travel time, bulk callout, push reliability) predates our data.
- **Ask Wen:** the full 4.2 change list and what the duplicate-push fix changed; how travel time is estimated (start location, missing or stale location); where scores are stored and the real score history; whether 4.2 reset any scores.
