# The questions, as written

Read this at SKILL.md step 2, before asking anything, and ask these
blocks as written rather than composing your own.

## Round 1 — frontend, backend, product

One message, three separate questions — and a fourth at the end where
step 1 found no docs MCP server connected. Ask them in the order below;
the MCP one is written to come last.

**Q1 — Frontend.**

> What's your frontend — React, plain JavaScript, React Native, Flutter,
> Android, or iOS? Another web framework (Vue, Angular, Svelte) is fine
> too. Not sure? Tell me where people will use this — in a web browser, an
> iPhone app, an Android app — and I'll work it out from your project.

| Frontend | SDK |
|---|---|
| React | `@videosdk.live/react-sdk` |
| Plain JavaScript, or any web framework without its own SDK (Vue, Angular, Svelte) | `@videosdk.live/js-sdk` via npm, or the CDN `<script>` tag from the JS quick start — wire it into that framework's own lifecycle |
| React Native | `@videosdk.live/react-native-sdk` |
| Flutter | `videosdk` (pub.dev) |
| Android | `live.videosdk:rtc-android-sdk` |
| iOS | `VideoSDKRTC` |

**Q2 — Backend, for the auth token.**

> Where should the auth token come from — do you have a backend (Node.js,
> Python, Java, PHP, Go, Ruby, .NET, Rust), or nothing yet?

"Nothing yet" is a real answer, not a blocker: build on a temporary token
from the VideoSDK dashboard and say in one line that it expires and is for
testing only — `auth.md` covers both paths.

**Q3 — Product.**

> What kind of experience is this?
>
> 1. **A call or meeting** — everyone can talk and share media, 1:1 or in
>    a group (audio-video calling).
> 2. **An interactive live stream** — one or a few hosts present to an
>    audience in real time, with features such as live chat, Q&A,
>    reactions, polls, or coming up on stage (ILS).
> 3. **A broadcast** — a one-way stream to a large audience. Viewers
>    primarily watch through an HLS playback URL (`.m3u8`), and a few
>    seconds of delay is acceptable (HLS).

**The docs MCP server**, only when it isn't already connected:

> One more thing before I start. VideoSDK publishes a documentation MCP
> server at **https://docs.videosdk.live/mcp**. Connecting it means every
> API lookup comes back matched to your SDK version, rather than me
> searching the docs and version-checking by hand. Shall I give you the
> steps to add it? I can build either way.

`docs-lookup.md` carries what to do with either answer. Where round 1 is
empty because the project already settled everything, this question goes
with round 2's message instead — it still shouldn't cost a turn of its
own.

## Round 2 — the use case

**Q4 — Use case.** Round 1's product picks which of the three blocks
below you ask. Ask that block as written — each is already the finished
question for its product, so don't assemble a menu from a master list.
This is why round 2 waits: a block chosen before the product is known
offers use cases the product can't carry.

**Product is a call** — ask:

> What's it for?
>
> 1. **KYC / identity verification** — video banking, document checks
> 2. **Telehealth** — doctor–patient consultations
> 3. **Education** — online classes, tutoring, a class where students talk
> 4. Something else — tell me about it

**Product is an interactive live stream (ILS)** — ask:

> What's it for?
>
> 1. **Live commerce** — live shopping, product demos
> 2. **Events & webinars** — town halls, workshops, audio rooms, creator
>    streams
> 3. **Education** — a large lecture, with students invited up to speak
> 4. Something else — tell me about it

**Product is a broadcast (HLS)** — ask:

> What's it for?
>
> 1. **Events & webinars** — town halls, all-hands, conference broadcasts
> 2. **Live commerce** — live shopping for a large, watch-mostly audience
> 3. Something else — tell me about it

**"Something else"** stays on every list: map their description to the
nearest use case using the table in `use-cases.md` and borrow that use
case's feature set, rather than asking again.
