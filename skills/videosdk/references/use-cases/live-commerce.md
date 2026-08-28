# Live commerce

Also read `modes.md`: this use case is a stream, so which mode each
participant joins in matters.

A seller demos products live; the audience reacts, asks, and buys.

**Fits**: ILS — buying decisions ride on instant interaction. HLS suits a
very large, watch-mostly audience where a few seconds of delay is fine.
**Doesn't fit**: plain calling.

**Core:**
- **ILS or HLS quick start** — per the split above; each has its own.
  ILS is the default here; take the HLS one only when the answer was a
  broadcast.
- **Chat** (interaction-in-livestream) — questions,
  reactions and order intent all arrive on the same channel. It carries
  on an HLS build too, with viewers in the room as `SIGNALLING_ONLY`.
- **Audience Polls** (interactive-live-streaming) — "which color should we
  open next?" Shoppers arrive mid-stream constantly, so the tally has to
  live in the store, per the poll split in `../use-cases.md`.
- *ILS:* **Invite Guest on Stage** (handling-participants; the
  change-mode guide under audience-management covers the same promotion)
  — bring a buyer or co-host up to talk. *HLS:* **Setup HLS Player**
  (integrate-hls) for the watch side instead; bringing a viewer on stage
  isn't a shape a broadcast has, so the interaction is chat and polls.

**Optional:**
- **Sending Virtual Gifts** (interactive-live-streaming) — monetization;
  the guide routes gift transactions through backend validation.
- **Reactions** (interaction-in-livestream) — check the platform's docs
  for a Reactions guide first, since each platform documents it under its
  own path; where a line documents the effect as a PubSub pattern
  instead, build it that way.
- **RTMP Livestream** — simulcast to YouTube / Facebook while selling
  in-app.
- **Customized Live Stream** (interactive-live-streaming) — when the
  broadcast needs branded overlays, a price or product card on screen,
  or a composed layout rather than the default one.
