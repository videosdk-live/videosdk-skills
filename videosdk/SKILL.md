---
name: videosdk
description: >-
  Use when the user wants to build anything with VideoSDK — "integrate
  VideoSDK", "add video calls to my app", "build a telehealth / KYC /
  classroom / live shopping app", "host a webinar", "audio rooms", "stream
  to an audience" — or when an @videosdk.live package is already in the
  project. Covers audio-video calling, interactive live streaming (ILS),
  and HLS broadcast on React, JavaScript, React Native, Flutter, Android,
  and iOS, plus VideoSDK auth: wiring up an API key, generating an auth
  token / JWT, and building the backend token endpoint.
license: MIT
metadata:
  author: videosdk
  version: "1.0.0"
---

# VideoSDK

Four answers decide what gets built, and all four arrive before any code:

- **Q1 — Frontend**: which client SDK.
- **Q2 — Backend**: which server signs the auth tokens, or none yet.
- **Q3 — Product**: audio-video calling, interactive live streaming (ILS),
  or HLS broadcast.
- **Q4 — Use case**: KYC, telehealth, education, live commerce, events &
  webinars. This decides which VideoSDK features the example shows: a
  telehealth call needs a waiting lobby, a KYC call needs image capture, a
  live shopping stream needs polls and a buyer brought up on stage.

The reference files call these Q1 to Q4 throughout, and "step 1" to
"step 5" always means a numbered section of this file. The two never mean
the same thing.

Fill them in — from the project when you can, from step 2's two short
rounds when you can't — then work from the official docs. Never write
VideoSDK code from memory: each platform has its own API surface and each
version its own, and recalled code blends them into something that looks
right and fails at runtime. This file and its references carry no code on
purpose; every snippet comes from the docs at build time.

One rule holds regardless of every answer: **the API secret stays on a
server.** Not in client code, not in a bundled env var, not committed, not
pasted through chat. Write a `.env` placeholder and let the user fill it.

## 1. Read the project first

Answer as many of the questions as possible without asking.

**Frontend** — the root manifest decides: `package.json` (React, React
Native, or plain JS), `pubspec.yaml` (Flutter), otherwise `build.gradle`
(Android) or `Podfile` / `Package.swift` (iOS). React Native and Flutter
carry their own `build.gradle` and `Podfile` under `android/` and
`ios/` — nested manifests confirm the root, never outvote it. A plain-JS
site may have no manifest at all, so check the HTML too. Two platform
manifests at the root with nothing above them means the root doesn't
decide: ask Q1, and where the repo holds a native Android and a native
iOS app side by side, ask which comes first.

**Installed SDK** — each stack declares it differently: an
`@videosdk.live/*` package in `package.json`, a `<script>` tag loading the
VideoSDK JS SDK from a CDN, `videosdk` in `pubspec.yaml`,
`live.videosdk:rtc-android-sdk` in `build.gradle`, `VideoSDKRTC` in a
`Podfile` / `Package.swift`. If found, use that SDK — and detect its
**resolved version**, not the range the manifest requested (`^0.11.0` is a
request, not a version): the lockfile, the installed copy under
`node_modules`, the version pinned in a CDN URL or `build.gradle`,
`pubspec.lock`, `Podfile.lock`, or `Package.resolved`. Major lines differ
in behavior and method names on every stack, so carry that version into
every docs lookup in step 4.

With nothing installed yet there is no version to carry — when step 5
scaffolds, **add the dependency by running the package manager's own add
command** — `npm install`, `flutter pub add` and their equivalents —
exactly as the platform's quick start gives it, unpinned. Let that
command write the version into the manifest, then read the resolved
version back from the lockfile and use it for lookups. Where a platform
has no such command and the manifest is edited by hand, copy the
coordinate from the quick start rather than recalling it.

**Never type a VideoSDK version from memory.** It fails quietly: a
plausible range like `^0.1.x` resolves to a line majors behind, the
install succeeds, and you build against docs for an API the installed
package doesn't have. Writing the manifest before installing? Leave the
version as `latest` and let the add command fill it in.

An installed SDK settles the frontend and nothing more. On most stacks the
same package serves calling, ILS and HLS, so finding it never answers Q3.

**Backend** — look for a server manifest at the root or in sibling
directories: `package.json` with a server framework, `requirements.txt` /
`pyproject.toml`, `pom.xml` / `build.gradle` (server, not the mobile
shell), `composer.json`, `go.mod`, `Gemfile`, `*.csproj`, `Cargo.toml`. A
full-stack framework (Next.js, Nuxt, SvelteKit) is its own backend — token
signing goes in an API route.

**Product and use case** — the user's own words may have answered them:
"add a 1:1 video call" settles the product; "for doctor consultations"
settles the use case.

**Your own tools** — check your tool list now, in this step, for a
VideoSDK documentation MCP server. A harness exposing no MCP tools at all
is a clear "not connected" — don't go hunting for a way to check.

**If it isn't connected, read `references/docs-lookup.md` now**, before
you ask anything. Round 1 carries an extra question about adding the
server, and the user's answer lands immediately after — so both the
question's wording (in `references/questions.md`) and what to do with a
yes or a no have to be in hand before round 1 goes out, not at step 4.
A "yes" that you answer with another offer instead of actual setup steps
is the failure this ordering exists to prevent.

## 2. Ask the questions — two short rounds

Ask only what step 1 left open, in plain language. Round 1's questions
travel in one message but stay separate, each answerable on its own —
never fuse two into a single "what's your stack and what are you
building?"

**The questions themselves are in `references/questions.md`. Read it now,
before asking, and ask its blocks as written.** The rules below decide
*which* block and *when*; that file holds the wording, and a question
composed fresh tends to fuse two of them or offer a use case the product
can't carry.

**Round 1 — frontend, backend, product**, in one message. Where step 1
found no docs MCP server connected, a fourth question rides along in that
same message rather than costing a turn of its own — asking whether to
add one. `references/questions.md` carries it beside the other three, so
ask it from there.
**Round 2 — the use case**, offering only what fits the product.

Round 2 waits even where the interface renders every question at once,
as a form or wizard: a batch is composed before any of its answers
exist, so a question inside it cannot narrow its options in response to
a sibling. Keep the rounds separate even where the product is already
settled — the two-round shape is the rule, and the skips below are its
only exceptions.

**Skip a round that has nothing left to ask.** Where the repo and the
user's words already settle frontend, backend and product — "add an
interactive live stream to my React app, token server's on Node" — round
1 is empty, and the use-case question is all that's left — carrying the
docs MCP question with it where step 1 found no server, since that
question has no other message to ride in.

**Where answers can't arrive, don't wait.** In a single-turn or automated
run, or with a user who has stopped answering, take the most likely
reading, say which assumptions you made, and build — a named assumption
is correctable, a question nobody will answer is not.

**A use case named by the user carries the product with it.** "Build a
telehealth app" or "live shopping demo" answers Q3 and round 2 together:
read the product off this table and confirm it in the plan sentence
rather than asking. Frontend and backend are still round 1's to ask if
the project hasn't answered them.

| Use case named by the user | Product |
|---|---|
| KYC / identity verification | call |
| Telehealth | call |
| Education | class → call; large one-to-many lecture → ILS |
| Live commerce | ILS; a very large, watch-mostly audience → HLS |
| Events & webinars | audience interacts or comes on stage → ILS; mostly watching → HLS |

Where a row offers two shapes, take it from their own description. Only
when the description settles nothing, ask once, in that use case's own
terms:

> Do the students / viewers / attendees talk and come on camera, or
> mostly watch?

## 3. Reconcile and confirm

Read `references/use-cases.md` now. It routes: it carries the rules that
hold whatever you build, and a table pointing at the one file for this
use case — `references/use-cases/<use-case>.md` — which names the product
shapes that fit and the features to build. Read both, plus
`references/use-cases/modes.md` when the product is ILS or HLS.

Round 2's per-product blocks keep most mismatches from arising, though one
can still arrive through "something else". Where the use case and the
product disagree, go with the shape the use case fits and surface the
substitution in the plan sentence rather than opening another round of
questions.

Then restate the whole plan in one sentence before building — "React +
Node token endpoint, 1:1 call, telehealth: precall, waiting lobby, chat,
recording" — so a wrong guess costs one sentence instead of a rebuild.

## 4. REQUIRED: every name comes from the docs

Every method, event, claim and config name in this build comes from the
official docs rather than from memory. That is the requirement, and it
holds however you meet it.

The VideoSDK documentation MCP server at **https://docs.videosdk.live/mcp**
is the best way to meet it: it serves the docs from an index and takes the
SDK version as an argument, so it hands back version-matched answers
instead of pages you have to version-check yourself. Where it isn't
connected, the fallback still meets the requirement, more slowly.

You checked your tool list for it back in step 1, so you already know
which of these you're on.

**Read `references/docs-lookup.md` before the first lookup**, if step 1
hasn't already sent you there. It carries how to query the server, the
question to ask when it's absent and how to honour the answer, the
web-search fallback and its hand version-line matching, when to refuse to
build, and which of the docs or this skill wins when they disagree.

## 5. Build

Read `references/ui.md` first, before anything below — scaffolding and
the use-case features both write screens, so a UI reference read after
them is a reference read too late. Its call-screen anatomy and icon
guidance apply everywhere; its palette applies to a new project, while an
app that already has a theme keeps its own. Add no dependency for it.

Then work through these in order, whether the project is new or existing.
They're named rather than numbered because "step N" throughout this skill
always means one of the five top-level sections, and a second set of
numbers here would make every cross-reference ambiguous.

- **Scaffold or wire in.**
  - *New project*: the smallest standard app for the stack, then the
    platform's quick start for the chosen product — calling, ILS and HLS
    each have their own. The two streaming quick starts are documented
    under closely related names, so confirm which one you have by the
    mode its audience joins in; `references/use-cases/modes.md` maps
    each mode to its product.
  - *Existing project you can read*: wire the quick start into the
    screens the app already has, rather than scaffolding a parallel app
    beside it.
  - *Existing project you can't read* — the user names their app but
    its files aren't in the workspace, which is common: don't scaffold
    a parallel app and don't guess at their component tree. Write
    self-contained files under one new folder, and put an integration
    note beside them naming where to mount or render each piece and
    which assumptions to correct.
- **Auth.** Read `references/auth.md` and build the path Q2 chose: a
   token endpoint on the named backend, or the dashboard token with its
   caveat stated. Never skip to a hardcoded token without saying what it
   is. The endpoint path carries two questions of its own — what a call
   belongs to in their app, and how the app knows who's signed in — so
   expect one more short exchange there.
- **Use-case features.** Implement the core set from the use-case file
  for the answer to Q4 — each feature names the docs guide to fetch
  through the docs source in step 4. This is what the use-case question
  was for: the example should feel like the user's product rather than a
  general-purpose demo.
- **Verify, and say who verified what.** Run what you can — the build,
  the app loading — and be precise about what you couldn't. What counts
  as working differs by product: a call means two participants seeing and
  hearing each other, ILS means a viewer who sees the stream and can't
  send, HLS means the playback URL plays. Anything needing real media is
  theirs to run — hand them that list rather than reporting it as done,
  and note that a second device needs HTTPS or localhost or the camera
  never opens.
- **Name the sources — by paste, never from memory.** Close with the
  docs pages you built from, one line each. Every URL must be one you
  actually opened, copied from your own tool history, never
  reconstructed from what the path probably looks like. Where you no
  longer have the exact URL, give the page's title and section and say
  you don't have the link rather than inventing one.
