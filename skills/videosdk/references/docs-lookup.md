# Getting the docs: the MCP server, and building without it

Read this at SKILL.md step 4, before the first documentation lookup —
and earlier, before round 1, when no server is connected, since the
question about adding one travels in that message.
SKILL.md owns the requirement — every method, event, claim and config
name comes from the official docs rather than from memory. This file owns
how you meet it.

## Check for MCP availability

Check whether a VideoSDK documentation MCP server is connected, and take
the tool names and their arguments from what that server advertises
rather than assuming a fixed set. Where it is present, use it for every
documentation lookup in this build:

- Search the docs before implementing any feature.
- Verify method and event names and their parameters.
- Look up config options and the values they accept.
- Find the worked example for the platform and product at hand.

In practice its search takes a library and a query, and library names
are `videosdk-`prefixed (`videosdk-react`, `videosdk-javascript`,
`videosdk-flutter`; bare `react` won't match) — confirm both against the
schemas the server actually advertises. It generally offers a way to list
the libraries it carries with the versions indexed for each — call that
first when you are unsure of a name. Keep the query short and
topic-shaped ("quick start", "image capturer") rather than the user's
whole sentence. Where the project has the SDK installed, pass its
resolved version if the search accepts one, and use the server's
version-resolution tool where it offers one to find the indexed line
closest to a release the user named. Read the version reported on the
results either way. Prefer the server's indexed documentation search over
any general URL-fetching tool it exposes.

## If it isn't connected: ask

**First, is asking worth their turn?** Where the harness has no way to
add an MCP server, or the user has already said no, don't ask at all —
an offer into a foregone answer wastes a turn. Go straight to the
fallback and say once that lookups won't be version-matched.

Otherwise ask — a question they answer, not a tip they skim. The question
itself sits in `questions.md` beside round 1's, because that is the
message it travels in and it shouldn't cost a turn of its own.

Then honour the answer:

- **Yes** → actually give the steps, in that same reply. Don't answer
  "yes" with another offer. VideoSDK publishes per-client setup
  instructions — search its docs for the MCP server page, which covers
  Cursor, Claude Desktop, VS Code and Antigravity, by GUI and by JSON
  config. **Take the per-client steps from it, not the URL**: that page
  documents a different VideoSDK MCP server, and the one you want is the
  URL in the question you just asked. **That page doesn't cover every
  client** — Claude Code, for one, takes a single `claude mcp add`
  command. Where the page doesn't cover theirs, or you can't reach it,
  hand them what any MCP client needs regardless: a server name,
  **streamable-http** as the connection type — not SSE and not stdio,
  and don't offer those as alternatives — and that URL. Say where their
  client keeps MCP settings if you know it. Adding it needs the agent
  restarted before the tools appear, so say so, and build this session on
  the fallback rather than waiting.
- **No, or no answer** → take the fallback and don't raise it again.
  Someone who declined does not want it re-offered at the next lookup;
  asking twice is how a reasonable prompt becomes nagging.

It is added to the coding agent's MCP configuration as a streamable HTTP
server. One thing to keep separate: VideoSDK also documents MCP in
another sense — how a VideoSDK AI voice agent calls external MCP tool
servers at runtime — which is a different feature from the docs server
here.

## Building without it

Say once that lookups won't be version-matched, then carry on. Web-search
each topic scoped to the official docs — `videosdk react waiting lobby
site:docs.videosdk.live` — and read the matching page; where search isn't
available but the site is, navigate it by URL rather than giving up.
**Match the version line by hand**, which is the job the server would
have done: the unversioned URL is always the latest docs, and older lines
sit under a version segment — but those segments name *docs* lines
(`0.x.x`, `1.x.x`), not SDK releases, so map the installed major onto its
line rather than hunting for its exact number. The installed package's
own typings are not a substitute for a guide — a signature carries no
semantics.

**Where neither the server nor the docs site is reachable, don't build.**
Describe the approach in prose, name the guides the user should open, and
skip the build rather than scaffolding around calls you can't source — a
partial app built on guessed calls costs more than no app.

A feature released after the installed line won't appear in that line's
docs. Say the feature postdates their version, leave it out, and let
upgrading be the user's call — code from a newer line is what this
section keeps out of an older install.

## Record the URL as you go

Copy each page's URL the moment you open it, into your working notes.
The build closes by listing them, and by then the exact paths are no
longer in front of you — a link written at that point is a guess wearing
a link's clothes. This costs one line per lookup and is the difference
between a citation the user can click and one that 404s.

## What comes from where

**When a page and this skill disagree, follow whichever owns the
question.** The docs own the facts: method and event names, payload and
claim names, config fields, per-platform paths, version differences.
This skill owns the decisions: the four answers, the use case's feature
set, where the secret lives, who chooses permissions, which participant
joins in which mode, and who verifies what. So a quick start that hands
every participant the same token is showing the shortest path to a demo,
and the token rules in `auth.md` still apply on top of it.
