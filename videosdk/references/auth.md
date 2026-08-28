# Auth

Fetch the platform's **authentication** guide through SKILL.md step 4
before writing token code, and take the payload, the claim names, and the
dashboard steps from it. Which path to build below is Q2's backend answer:
a named backend → the token endpoint; "nothing yet" → the dashboard token.

## What each participant's token carries

Same on both paths:

- A plain participant gets the join permission only, and it has to be
  said out loud. **A token signed with no permissions is not a
  restricted token** — the server SDK documents the default as the join
  *and* moderator permissions together, so the participant you meant to
  limit arrives holding moderator rights and nothing looks wrong. Name
  the permissions on every token you sign, including the ordinary ones.
  Whether an *empty* list behaves like an omitted one, and whether the
  same default applies to a hand-signed JWT as to the server SDK's
  builder, are not documented either way — naming them explicitly is
  what makes both questions moot, which is why it's the rule rather than
  a preference.
- A moderating host — the teacher, the doctor admitting patients, the
  seller running the stream — gets **both** the join and the moderator
  permission; the moderator permission alone will not admit them to the
  room. It covers toggling other participants' mic and camera, and the
  server SDK's grant table also gives it removing a participant. It does
  not gate screen share, recording, the whiteboard, or publishing —
  `use-cases.md`'s role rule carries how to build a host-only control on
  top of it. The per-platform remove-participant guides name no
  permission either way, so take removal from the grant table rather
  than from the platform guide.
- A guest who must be admitted (a waiting lobby) gets the ask-to-join
  permission **alone**. This isn't a matter of taste: the SDK raises an
  invalid-permissions error whose message says not to combine the join
  or moderator permission with ask-to-join, so a token carrying both is
  rejected rather than merely ambiguous. The host who admits them needs
  the ordinary **join** permission, not the moderator one — admitting is
  gated by join.

Take the literal claim names — and confirm each behavior above — from the
auth guide.

## Ask-to-join, either path

Where guests are admitted by a host — telehealth's waiting lobby, KYC's
queue — fetch the **waiting-lobby** guide and build **both** sides from
it. The host app has to receive the entry request and answer it; build
that handler before saying the flow works, or the guest waits and nothing
is raised. Take what the guest holds after admission from that same page —
whether the SDK carries them in on the token they already have, or the app
mints and rejoins with a second one — rather than assuming either.

## No backend yet: the dashboard token

Send the user to the API Keys page on the VideoSDK dashboard — take the
exact path from the auth guide. You can't reach it yourself, so relay the
steps below and keep building while they fetch the tokens: the app should
end up finished except for the token values, so the run delivers the app
*and* the request, not the request alone.

Tell them to:

- Do the whole config — permissions and expiry — in the token generator's
  JSON editor, so the token is defined in one place.
- Leave the token's **roles claim** unset (take the literal claim name
  from the auth guide). A roles claim restricts a token to *either*
  joining rooms *or* calling server APIs, and one temporary token has to
  do both. A server signing its own tokens can set the claim per token
  instead.
- Regenerate after every change, then copy each token before closing the
  dialog.
- Generate a separate token for each participant kind — host and guest —
  since permissions are fixed at signing.

Never invent a token value or leave a placeholder that looks like one. If
they paste an API key and secret instead of a generated token, don't use
them and don't write them to any file: say the secret belongs on a server
and ask for the token the generator produced.

A dashboard token is a live credential for their account until it
expires, so keep what they paste out of committed source: a gitignored
env file where there's a build step, or a separate gitignored config file
the page loads where there isn't — never inline in the HTML. Say when the
tokens expire, and say in one line that this path is for trying things
out; a shipped app wants the endpoint below.

Create the room from the client on this path, as the quick starts do, and
surface the created room id so a second participant can join it. Where the
demo needs both a host and a guest, pick which token to use from a query
parameter or a build-time constant.

## A backend was named: build the token endpoint

**Detect what the repo already answers before asking.** Where the backend
is present, its models or schema usually name the thing a call belongs
to — read them, and ask only to confirm. When the backend isn't in this
workspace — a separate repo, a service you can't read — say so and ask for
its path, or for the token route and the guard from one protected route
pasted in. Where neither is available, write the endpoint as a standalone
file they can drop in, and name the assumptions you made about their
session and models so they can correct them.

Who moderates is never a question — Q4's use case already says it: the
doctor, the teacher, the seller, the agent holds the moderator
permission. Where no role stands out (a plain group call, "something
else" with no host in the description), sign everyone as a plain
participant.

Then ask what neither the repo nor the user has already answered, in one
message — but only while the exchange is still open:

> - What does a call belong to in your app — a class, a booking, a
>   listing, a ticket?
> - How does your app know who's signed in — a login session, a token the
>   app sends, something like Firebase, Supabase, Auth0 or Clerk?

Where the answers can't be inferred and aren't going to arrive — a
single-turn run, or a user who has stopped answering — don't block:
build the endpoint the standalone way described above, with the
assumptions named for correction.

The *what does a call belong to* answer is the endpoint's request shape.
The *how do you know who's signed in* answer is the guard the endpoint
opens with: reuse the check already protecting another route, and read
the user id from it. Where the provider is configured but no protected
route exists to copy, use its documented server-side session helper.
Where nobody logs in yet, say the endpoint can't be finished until they
do, and leave the check throwing.

VideoSDK publishes official token-generation and room-creation examples
for the major backend languages in its `videosdk-rtc-api-server-examples`
GitHub repo — use the one matching Q2's answer for the language
mechanics: which JWT library to reach for, how that language calls the
REST API. Take the token payload from the auth guide and apply the rules
below on top. If
the language has an official VideoSDK server SDK, prefer it and take the
package name and calls from its docs. Otherwise sign the documented
payload with that language's standard JWT library and create rooms over
the REST API, fetching the create-room reference for the call.

This path needs an API key and secret too. Where the user hasn't got
them, send them to the dashboard's API Keys page as the first thing and
keep building around a `.env` placeholder rather than stopping.

What the guide doesn't cover is the shape of your own endpoint, so hold
to these four:

- **The API secret never leaves the server.** Read it from the server's
  environment at signing time. Never return it in a response, never put
  it in a client-readable env var — `NEXT_PUBLIC_*`, `VITE_*` and
  `EXPO_PUBLIC_*` are all bundled into the client — and never commit it.
  Write a `.env` placeholder and let the user fill it in.
- **The server chooses the room and the permissions**, from its own
  session data — after checking this user is entitled to this class,
  booking or listing. If the client can name the room or ask for the
  moderator permission, anyone can `curl` for either.
- **An unfinished auth stub throws.** One that returns a token while the
  check is still a TODO leaves the endpoint open to anyone who calls it.
- **Treat a rejected anonymous request as the gate working.** Pass the
  app's session; never loosen the endpoint to make a test pass.

## Setting the token and ids in the client

Same on both paths. Place the token separately from the ids: in React,
pass the token as a prop on the provider and put the room and participant
ids *inside* its config — that's where the SDK reads them from, and it's
what lets a token scoped to that room and participant authorize the join.
In plain JavaScript, set the token globally before initializing the
meeting and pass the ids to the init call.

Take the exact names and the required fields from the platform's quick
start; each platform places these differently, and the config objects
carry fields beyond these.
