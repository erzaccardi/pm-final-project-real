# Spotlight Curated Rail, Simplified PRD (StreamLine)

**Author:** Me · **Status:** Draft · **Target:** High-Fidelity Prototype · **Persona:** Young Professional

## 1. The Big Picture
- **Vision:** To kill decision fatigue and stale perception, the spotlight curated rail provides a fresh showcase for renewed value perception.
- **Press release:** Today, StreamLine launched Spotlight, a hand-picked rail at the top of the homepage that cuts through the endless scroll. After a long workday, young professionals told us the hardest part of watching wasn't the watching. It was the choosing: rows of algorithm picks that all blur together, and twenty minutes of browsing that ends with nothing. Spotlight replaces that with a short set of 8–12 titles chosen by StreamLine's editors and refreshed every week, so there's always a clear place to start.

"I used to open the app, scroll, and close it," said one early tester. "Now I check Spotlight first, and most nights that's where I stop looking." Spotlight is live today on [platform], and it's the first step in making StreamLine feel personal, fresh, and worth coming back to.
- **Success metric:** 30-min+ session rate
- **Guardrail:** Browse engagement ≥ 3 min

## 2. The Details
### User stories
- As a young professional winding down after work, I want a short, hand-picked set of titles at the top of my homepage so that I can pick something good without scrolling through endless rows.
- As a young professional, I want to know the Spotlight picks were chosen by people, not the algorithm, so that I trust them as a fresh starting point.
- As a young professional with limited evening time, I want to see a title's runtime and why it's notable before I press play so that I can commit to it with confidence.
- As a returning user, I want Spotlight to skip titles I've already finished so that it always feels fresh.
### Screens to build
- 1. Home: Spotlight rail pinned at the top above existing algorithm rails, labeled "Spotlight · Picked by our editors," showing 8–12 title tiles.
- 2. Core: Title detail opened from a Spotlight tile, showing poster, title, genre, runtime, Hidden Gem badge (if applicable), a one-line synopsis, and a "Play" button.
- 3. Confirmation: Now Playing screen confirming playback started from Spotlight, with a simple session progress bar toward 30 minutes.
### Functional requirements
- The system must render the Spotlight rail as the first rail on the homepage, visible without scrolling.
- The system must display between 8 and 12 titles in the Spotlight rail, in the fixed editorial order from the data file.
- The system must label the rail "Spotlight · Picked by our editors."
- The system must exclude any title marked unavailable for the user's region from the rail.
- The system must hide the Spotlight rail entirely if fewer than 3 eligible titles remain.
- The system must open the title detail screen within one tap or click of selecting a Spotlight tile.
- The system must tag every playback started from Spotlight with source = "spotlight" and log it in a visible on-screen event log.
### Smart behaviors (Situation → Outcome)
- If user has finished a title → that title is removed from the Spotlight rail and the next eligible title fills the slot.
- If fewer than 3 eligible Spotlight titles remain → the rail hides and the homepage starts with the existing algorithm rails.
- If a Spotlight title is high-rated with low view count → a "Hidden Gem" badge appears on its tile and detail screen.
- If user presses Play from a Spotlight title → the Now Playing screen opens and a "spotlight_play_start" event is logged.
- If a Spotlight session passes 30 minutes → a "session_30min" event is logged and the progress bar shows complete.
- If user browses for 3+ minutes without pressing Play → a "browse_3min" event is logged for the guardrail.
### Technical constraints
- No external API calls; all titles come from a hard-coded mock JSON array (12–15 titles with title, genre, runtime, rating, viewCount, available, watched).
- React useState only; no database, backend, or localStorage.
- No recommendation algorithm or AI ranking; rail order is fixed by the array order.
- No real video playback; Now Playing is a static screen with a simulated timer.
- No login or user accounts; "watched" and "region" are toggles on the mock data.
- No admin or CMS screen; editors' picks are set in code.
- Placeholder poster art only (solid colors or initials); no copyrighted images.
- Single-file component; no routing library, screen switching via state.

## 3. The Logistics
### Features out
- No AI ranking or personalized ordering (A5); no 'Why You'll Love This' labels (A2); no mood filter or user filters (A4/A9); no curator profiles or follows (A7); no sponsored or Sales-requested placements; no email or push promotion (A6); no editorial CMS or admin screen; no login, settings, or real watch history; no second Spotlight rail or multi-platform build.
### Edge cases & safety guard
- Empty or missing data file → rail hides, homepage starts with existing rails, no error shown to user.
- Fewer than 3 eligible titles → rail hides rather than showing a thin, half-empty row.
- All titles watched → rail hides; never repeats finished titles to fill space.
- Missing poster or runtime → show placeholder art and "Runtime unavailable"; tile stays playable.
- Long title name → truncate with ellipsis on tile, show in full on detail screen.
- Duplicate title IDs in data → show the title once.
- Play tapped twice quickly → log one play_start event, not two.
- User exits playback before 30 min → no session_30min event logged.
- Safety: never show a title marked unavailable; never invent titles, ratings, or runtimes not in the data; never reorder the rail algorithmically; never insert a sponsored or unlisted title; never render a blank or broken rail.
### Decision log
- Removed personalized ordering (a Should-Have) to keep the algorithm out entirely, so any lift in 30-min sessions can be attributed to curation alone.
- Deferred 'Why You'll Love This' labels (A2) to NEXT, so the rail's effect is measured on its own before adding a second variable.
### Evals
- Rail accuracy: 100% of test runs show only eligible titles (available, unwatched) in the exact editorial order, with 8–12 tiles or the rail hidden.
- Time-on-task: ≥80% of testers go from opening Home to pressing Play on a Spotlight title in ≤30 seconds and ≤3 taps.
- Safety: 0 instances across test runs of an unavailable, watched, invented, or sponsored title appearing, and 0 blank or broken rails.

## MoSCoW scope
- **Must:** A distinct container to house the content so that it stands out from core library; rotated selection every three days
- **Should:** genre tags; Media type tags (movie, series, podcast); Capacity for high volume should the feature expand in the future; hidden gem badge or picked for you badge (as it is slated for same sprint)
- **Could:** Add to my list; Don't show me things like this button; Different curation per market; Runtime shown on tiles, to help her commit to 30+ minutes
- **Won't (now):** Any new recommendation model or AI ranking. That's A5, in NEXT.; 'Why You'll Love This' labels. That's A2, in NEXT. Keep it separate so each feature's effect can be measured on its own.; Mood filtering (A4) or user filters of any kind. These add choices, and this rail exists to reduce them.; Named curators, follow buttons or curator profiles. That's A7, which you cut.; Sponsored, paid or "Sales-requested" placements. Write this into the PRD explicitly. It's the fastest route to the "company pushing random things" failure you flagged.; Email or push promotion of the rail. That's A6, in NEXT.; More than one Spotlight rail, or launching on every platform at once.

---
**Builder hook:** Build a working prototype based on this PRD. Use the User Story as the core flow, Functional Requirements as build constraints, and prioritize speed and clarity over visual complexity.

https://lovable.dev/preview/Uw4X6WTxnmHeBnF09wtPQK7cVztXWmOG
