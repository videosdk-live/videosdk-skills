# Use case → features

Read this at SKILL.md step 3, once step 2's questions are answered — or
earlier, if a "something else" answer in round 2 needs mapping to a shape.

This file is the router. It carries the rules that hold whatever you're
building; the feature set for one use case lives in its own file, so read
this page plus the one row you need.

| Use case | Read | Fits |
|---|---|---|
| KYC / identity verification | `use-cases/kyc.md` | call, 1:1 |
| Telehealth | `use-cases/telehealth.md` | call, 1:1 or small group |
| Education | `use-cases/education.md` | call for a class; ILS for a lecture |
| Live commerce | `use-cases/live-commerce.md` | ILS; HLS for a large watch-mostly audience |
| Events & webinars | `use-cases/events-webinars.md` | ILS or HLS |
| Something else | this page, last section | map to the nearest shape |

Building ILS or HLS? Also read **`use-cases/modes.md`** — which mode each
participant joins in is the decision that fails silently if you get it
wrong.

Each use-case file names the product shapes that fit it and the features
that make the example feel like that product: a telehealth call reads as
telehealth once the patient waits and the doctor admits them. Build the
**core** set; offer the **optional** set in one line after the core works,
and add the ones the user picks.

Every feature names a docs guide. Fetch it through SKILL.md step 4 and
take from the page the implementation it shows for your platform and this
use case — a guide often carries several variants, and copying all of
them lands code in the project that nothing needs. The names in these
files are signposts for finding the right guide, and the same holds for
the behavioral notes: confirm each against the page you fetch. Keep each
page's URL as you go; the build in step 5 closes by naming the sources.

A name in these files is something to search for, not proof it exists on
your platform. Where the guide isn't there, say which feature you
couldn't find, then build the rest of the core set and offer the nearest
documented capability — never fill the gap with an invented method or
event name.

A few conventions for reading them:

- The name in parentheses after a feature is the docs section its guide
  lives under; where a feature has none, search on the feature name.
- Two ILS sections have near-identical names and both are real:
  `interactive-live-streaming` (polls, virtual gifts, live captioning) and
  `interaction-in-livestream` (chat, reactions, notify attendees).
  Neither is a typo of the other.
- Some features are filed differently on the calling and ILS sides —
  screen share is under `handling-media` for calls and
  `device-management` under ILS. Search the section for the product
  you're building.
- Guide names were checked against the React and JavaScript docs; other
  platforms carry equivalents under the same or similar names, so search
  the platform's docs rather than assuming a path.

## Rules that hold across every use case

Read these before the feature list rather than after.

- **Existing project**: wire features into the screens and roles the app
  already has. If the app already has chat, don't add a second chat.
- **The role rule — roles are decided on the server, and the token gates
  less than you think.** Name the permissions on every token you sign —
  an unnamed set defaults to moderator, which `auth.md` covers.
  Beyond mic and camera control, no token permission gates screen share,
  recording, the whiteboard or publishing, and the join mode is client
  config rather than a token claim, so a determined client can cross
  both.
  Build a teacher-only or host-only control in three layers:
  1. The endpoint that signs the token also returns the role it decided
     — never a role the client asked for.
  2. The UI renders the control for that role only.
  3. The app declines to render other participants' streams where the
     product says one person presents; an unrendered share reaches
     nobody.
  Layer 3 needs a host identity the client can't forge, and this is
  where it usually goes wrong: a host id written into shared state by
  the host's own client is writable by anyone. Take it from the same
  server response that decided the role, or have the server mint
  participant ids with a role prefix and scope each token to its
  participant id — a student then cannot join under a teacher's id.
  Where the layers aren't enough, removal is enforced by the moderator
  permission rather than by your UI. Say plainly which layers are UX and
  which are enforced, rather than letting a hidden button read as a lock.
  `auth.md` carries the token paths and the permission each role needs.

  On the dashboard-token path there is no endpoint, so layer 1 is gone
  and layer 3 has nothing to anchor to: the generator cannot scope a
  token to a participant id, and the client supplies its own. Two tokens
  still separate the moderator permission, which is real. Everything
  else is UX, and the summary should lead with that rather than bury it
  under the setup steps.
- **Events are not state.** PubSub carries a message to whoever is
  subscribed at the time, and its guide documents an option that also
  replays the backlog to someone who subscribes later. Neither hands you
  a *current value* — a late joiner either misses everything or replays
  every message and recomputes. So where everyone must read the same current
  value — a poll's running tally, which students are muted — keep that
  value in **Realtime Store** (collaboration-in-meeting), whose own
  guide names polls and moderation data as its purpose, and let PubSub
  carry the events around it. It is sized for small shared values — a
  tally, a list of ids — rather than documents or transcripts.

  **The poll split** is where this needs saying out loud, because the
  docs' polls guide is written with PubSub alone: the question and every
  vote travel as events on `POLL` and `POLL_RESPONSE`, and each client
  tallies whatever it happened to hear. Keep that shape for the votes —
  a vote *is* an event — but have the host aggregate and write the
  running tally to the store, and have every client render the tally
  from there rather than from its own count. Address each vote to the
  host alone rather than broadcasting it to the whole audience — the
  PubSub guide documents how to target a single participant.
- **The recording rule.** Wherever Recording appears in a feature set,
  put a visible on-screen notice up before it starts, on every
  participant's screen — a classroom of minors deserves the same
  treatment as a medical visit. No token permission gates starting a
  recording, and a recording bills the account, so make the record
  button host-only by the role rule above and have the host's app watch
  the recording-state event so an unexpected recording can be stopped.
- **Precall.** It sits in the core set of most of these use cases and is
  often the first screen built. Build it from the platform's
  **setup-call** guide, and build what that guide shows: where it
  documents a network-quality test, include it; where it doesn't, the
  green room is permissions, device enumeration and a camera preview,
  and that is finished rather than missing. Don't reach for a method the
  platform's own docs don't show you.
- **Say why it failed.** Subscribe to **both** the error event and the
  meeting's connection-state event, and put what they report on screen.
  Show reconnecting as its own state rather than leaving the last frame
  up.
- **Words on the record.** Where a request mentions captions,
  transcripts, notes, minutes, summaries or action items, read the
  Transcription & Summary guides before building anything custom —
  there is a realtime path for live captions and a post-meeting path
  that transcribes a recording and generates a summary from it.
- **Regulated or restricted.** Where the requirements mention privacy,
  regulated data, encrypted media, enterprise isolation or geographic
  limits, read Security (E2EE) and Geo Fencing & Proxy Controls
  alongside the feature list, and treat the token permissions in
  `auth.md` as part of the same answer. These cut across every use case
  and a list organized by use case won't surface them on its own.
- **At scale.** Where the audience runs to hundreds or more — a webinar,
  a lecture, a live event — read the Scalability and Call Quality guides
  before settling on a layout. Which participants you render and
  subscribe to is a design decision at that size, not a detail.

## Something else

Map their description onto the nearest use case in the table above and
borrow its list — "fitness coaching" is events-with-a-small-audience,
"video banking support" is telehealth-with-an-agent. Read that use case's
file and treat its core set as the starting point.

**A plain call is a finished answer, not a gap.** "Just add video
calling", a 1:1 call, a team standup — there is no vertical to route to
and no file to read. Build the calling quick start plus the pieces every
call wants: a precall device check, mute and camera controls, screen
share, chat, and leave. Say that is what you built and offer the
use-case features as a next step rather than asking again.

Then pick features by what their description names, browsing the docs
sidebar categories: Setup Call, Handling Media, Rendering Media,
Collaboration in Meeting, Control Remote Participant, Recording, Live
Streaming, Transcription & Summary, Security. For a use case that isn't
in the table: if two shapes genuinely fit, pick the nearer one and name
the choice in the plan sentence rather than asking. A use case that *is*
in the table follows SKILL.md's tie-break instead — that one asks once
rather than picking.
