# videosdk skill

Milestone 1 — a short batch of questions, then build from the docs.

```
videosdk/
├── SKILL.md                    the flow: read project, ask, reconcile, docs source, build
└── references/
    ├── questions.md            the four questions as written, and the round-2 blocks
    ├── docs-lookup.md          the docs MCP server, and the fallback when it's absent
    ├── use-cases.md            router: cross-cutting rules + which file to read
    ├── use-cases/
    │   ├── kyc.md              agent verifies an ID on a 1:1 call
    │   ├── telehealth.md       doctor and patient, waiting lobby
    │   ├── education.md        class (call) and lecture (ILS) variants
    │   ├── live-commerce.md    seller streams, buyers chat and vote
    │   ├── events-webinars.md  town halls, workshops, audio rooms
    │   └── modes.md            which mode each participant joins in (ILS/HLS)
    ├── auth.md                 both token paths: dashboard token, backend token endpoint
    └── ui.md                   call-screen anatomy, icons, palette
```

`SKILL.md` is about 250 lines and loads on every trigger. Everything
else is read at the step that needs it: `questions.md` before asking,
`docs-lookup.md` before the first lookup, the router and **one** of the
five use-case files after the answers are in, `modes.md` only for a
stream, `ui.md` before the first screen.

## What it does

An agent asked to "integrate VideoSDK" fills four answers before writing
any code — from the project when it can, from questions when it can't,
in two short rounds:

**Round 1** (one message, each question separate — never merged)

1. **Frontend** — which client SDK.
2. **Backend** — who signs the auth tokens, or "nothing yet".
3. **Product** — audio-video calling, interactive live streaming (ILS),
   or HLS broadcast.

Plus a fourth where no VideoSDK docs MCP server is connected: whether to
add one. Step 1 checks the tool list precisely so round 1 knows whether
to carry it, rather than costing a turn of its own later.

**Round 2**

4. **Use case** — offered *filtered by the product*: a call gets KYC,
   telehealth and education; ILS gets education (lecture), live commerce
   and events & webinars; HLS gets live commerce and events & webinars.
   Plus "something else" everywhere.

The use-case question waits for round 1 because its menu depends on the
product answer, and a batch is composed before any of its answers exist —
many interfaces render every question at once, as a wizard — so a
question inside a batch can't narrow itself in response to a sibling.
Asked together, the menu offers an identity check next to a broadcast.

Rounds collapse whenever the answers are already known: a repo and a
sentence that settle frontend, backend and product leave only the
use-case question, and a request that names the use case ("build a
telehealth app") carries the product with it, so neither gets asked.

The use case is the milestone-1 point: it decides which VideoSDK features
the example implements (image capture for KYC, waiting lobby for
telehealth, whiteboard for education, polls and gifts for live commerce…),
in a new project or wired into an existing one.

## Layout convention

`SKILL.md` loads into context every time the skill triggers, so it holds
only what's needed to *decide*: which question belongs in which round,
when to skip one, how to resolve a use case that fits two products, what
order to build in. What's needed only *after* deciding — including the
question wording itself — lives in `references/`, read on demand.

The split between `SKILL.md` and `references/questions.md` is
rules-versus-text: SKILL.md decides *which* block to ask and *when*,
`questions.md` holds the words. That boundary matters because the round-2
blocks are written per product, so an ILS build never offers an identity
check.

`references/use-cases.md` is a **router**, not a feature list. It carries
what applies to every build — how roles are enforced, when to reach for
Realtime Store instead of PubSub, the recording notice, security and
scale triggers — plus a table pointing at the one file for the use case
in hand. Those rules stay in the router rather than being copied into six
files, because duplicated guidance drifts apart as the docs change.

Each use-case file refers to those rules **by name** ("the role rule",
"the poll split", "the recording rule"). If you add one, give it a name
and put it in the router; if you edit a use-case file, don't restate a
rule there — point at it.

The skill contains no code by design. Every API name and snippet comes
from the official docs at build time — the VideoSDK docs MCP server when
connected, `docs.videosdk.live` via web search when not — matched to the
project's installed SDK version.

## Install

```bash
ln -s "$PWD/videosdk" ~/.claude/skills/videosdk
```

## Maintenance

The files carrying claims from the docs are the ones under
`references/use-cases/` — guide names, which docs section each lives
under, and which features the docs tie to which use cases (verified
2026-08-28). Re-check these after a docs restructure.

`modes.md` is the most consequential of them: a wrong mode fails silently
as a black tile, and the docs' own `manage-roles` page names `RECV_ONLY`
zero times on React, JavaScript, React Native and Flutter — so for the
four platforms most likely to build ILS on the web, that table is the
only place the audience mode is spelled out.

A few rules exist specifically because reading the docs alone misleads.
These are the ones to revisit if the docs change:

- A token signed with **no** permissions defaults to join **plus
  moderator** — documented only on the server SDK page, not on any
  platform's auth guide.
- The moderator permission covers **removing** a participant — in the
  server SDK grant table; the per-platform remove-participant guides name
  no permission at all.
- **Display Attendees Count** filters to `SIGNALLING_ONLY` and excludes
  the presenter, so it counts an HLS-in-room audience and reads zero on a
  broadcast whose viewers only play the URL.

**What deliberately isn't here: per-platform API facts.** No method name,
signature, availability claim or numeric limit lives in this skill, and
that is a rule rather than an oversight. Twice during development a
build failed, the fix looked like "write down which platforms have this
method", and both times the written-down fact was wrong or non-uniform —
the precall network test and the HLS playback URL differ by platform,
including in name, and one of them doesn't exist on Android at all. The
platform's own guide always knew. So when a build goes wrong here, the
repair is to point at the docs more firmly, not to record what they say:
a skill that names an API takes on a maintenance burden it cannot meet,
and a wrong fact is worse than a missing one, because an agent will build
on it instead of looking.

`SKILL.md` and `auth.md` name only package coordinates, manifest files
and endpoint-shape rules, so they age more slowly. Where `auth.md` used
to restate documented facts — token expiry, participant-id uniqueness,
the roles claim — it now points at the auth guide instead, on the
principle that the docs own the facts and this skill owns the decisions.

## Installing into an IDE that reads a copy

Some agent IDEs load skills from their own directory rather than from
this folder — Antigravity, for example, reads
`~/.gemini/config/skills/`. A copy there does not track edits made here,
so re-sync after every change:

```bash
rsync -a --delete --exclude '.DS_Store' videosdk/ ~/.gemini/config/skills/videosdk/
```

Keep only one directory named `videosdk` inside whatever root the IDE
scans — a second copy of the same skill name (a snapshot, a backup) can
stop it from loading.
