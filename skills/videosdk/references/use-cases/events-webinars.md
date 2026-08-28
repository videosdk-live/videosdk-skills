# Events & webinars

Also read `modes.md`: this use case is a stream, so which mode each
participant joins in matters.

Speakers present to an audience: town halls, workshops, product launches,
audio rooms, creator streams.

**Fits**: ILS when the audience interacts in real time or comes on stage;
HLS when they mostly watch and a few seconds of delay is fine. An audio
room is either of those or a plain call, so let the interaction model
decide rather than the word "room": where everyone present may speak,
it's a call with cameras off; where a few speak and the rest listen
until invited up, it's ILS with cameras off.

**Core:**
- **ILS or HLS quick start** — per the split above; each has its own.
- **Precall** (setup-call) — a green room for whoever presents: device
  check before they go live, per the precall rule in `../use-cases.md`.
  A broadcast has no second take, and an audience of thousands has no
  way to tell the speaker their mic is wrong.
- **Chat** (interaction-in-livestream).
- *ILS:* **Raise Hand** + **Invite Guest on Stage** — Q&A that ends on
  stage. *HLS:* **Setup HLS Player** (integrate-hls) for the watch side.
- **Recording** — the replay outlives the event.

**Optional:**
- **Audience Polls** (interactive-live-streaming) — split votes and tally
  per the poll split in `../use-cases.md`.
- **RTMP Livestream** — simulcast to social platforms.
- **Live Captioning** (interactive-live-streaming) — accessibility.
- **Customized Live Stream** (interactive-live-streaming) — branding,
  lower thirds, or a composed layout for a produced event rather than
  the default one.
- **Post Transcription & Summary** (transcription-and-summary) — the
  transcript and the action items that outlive the event.
- **Notify Attendees** (interaction-in-livestream) — "going live" pings.
- **Active Speaker Indication** — panel discussions with many speakers.
- **Reactions** (interaction-in-livestream) — a silent audience gives a
  speaker nothing to work with; reactions are the cheapest signal back.

