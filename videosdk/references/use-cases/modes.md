# Choosing each participant's mode (ILS and HLS)

Read this whenever the product is ILS or HLS, alongside the use-case file
and the rules in `../use-cases.md`.

Everyone who joins a room joins in a mode, and the mode decides which
media that person receives. A mismatch shows up as a black tile rather
than as an error, so settle it up front. Decide from one observable
fact: **how does this person's media flow?**

| How their media flows | Mode |
|---|---|
| They publish — host, co-host, invited on stage | `SEND_AND_RECV` |
| Real-time from the room, rendered by your app (ILS audience) | `RECV_ONLY` |
| From an HLS player, and they're in the room for chat or a head count | `SIGNALLING_ONLY` |
| From an HLS player only, never joining the room | no mode, no token |

Each wrong answer costs something real:

- `SIGNALLING_ONLY` on an ILS audience leaves every viewer on a black
  tile — that mode carries no media, though chat and PubSub still work.
- `RECV_ONLY` on an HLS audience subscribes them to real-time streams the
  page never renders, spending bandwidth on media the player already
  delivers.

Bringing a viewer on stage is a **mode change**, not a rejoin — promote to
`SEND_AND_RECV`, demote back after. `CONFERENCE` and `VIEWER` are the
earlier names for `SEND_AND_RECV` and `SIGNALLING_ONLY`, so use whichever
names appear in the docs for the installed line. `RECV_ONLY` arrived
with live streaming and has no earlier name — on a line old enough to
still say `CONFERENCE`, don't try to back-map it.

The same fact identifies the **quick starts**. ILS and HLS are documented
under closely related names, so confirm which product a page covers from
the mode its audience joins in rather than from its title: an audience in
`RECV_ONLY` is real-time ILS; an audience in `SIGNALLING_ONLY` playing a
playback URL is HLS. `SIGNALLING_ONLY` is there for viewers who also join
the room — for chat, or a head count — and a viewer who only plays the URL
joins nothing and needs no token at all.

A viewer who **never joins the room** can't read the URL from it. Serve
it from your backend, captured by the host's app once the stream is
playable. With no backend yet — a real Q2 answer — bring them in as
`SIGNALLING_ONLY` instead, or show the URL in the host's page to copy,
and say that a real audience wants the endpoint.
