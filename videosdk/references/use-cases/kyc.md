# KYC / identity verification

An agent verifies a customer's identity on a short 1:1 call.

**Fits**: audio-video calling, 1:1. **Doesn't fit**: ILS and HLS — an
identity check is a private two-party conversation; reconcile per
SKILL.md step 3.

**Core:**
- **Precall** (setup-call) — device check before the customer joins; a
  camera problem found mid-verification means starting the flow again.
- **ID capture** — **Image Capturer** (handling-media) plus **Upload &
  Fetch Temporary File** (collaboration-in-meeting), which are one flow
  rather than two features. Capture runs only on the device that owns
  the camera, so the agent can't grab a frame of the customer directly:
  the agent requests a shot over PubSub, the customer's app captures and
  uploads it, then publishes the file id back for the agent to fetch.
  The docs name Video KYC as exactly the capturer's purpose. Customers
  are usually on a phone, and the flow needs the rear camera for the
  document and the front one for their face — wire the camera switch
  (change-input-device, handling-media) into the capture step rather
  than leaving them to find it.
- **Recording** — an audit trail is the point of video KYC; fetch the
  record-meeting guide, and see the recording rule in `../use-cases.md`
  for the on-screen notice.
- **Waiting Lobby** (setup-call) — queue customers until an agent is free.

**Optional:**
- **Geo Tag Meeting Recordings** (recording) — embeds the customer's
  coordinates in the recording, which the guide frames as a compliance
  requirement for BFSIs doing vKYC. It works through a custom recording
  template rather than a flag on the recording call, so read the guide
  before promising it.
- **Identity Verification APIs** — OCR, Face Match, Face Spoof Detection
  and Number of Face Detection: server-side REST APIs under the docs'
  Identity Verification section, for automating the check itself. All
  four are documented as Enterprise plan only, so confirm the user's
  plan before building the flow around them.
- **Chat using PubSub** (collaboration-in-meeting) — the agent sends
  instructions ("hold the card closer") without talking over the customer.
- **Post Transcription & Summary** (transcription-and-summary) —
  transcribes the recording and generates a summary from it. What the
  customer was asked and what they answered is half of what an audit
  reviews, and a transcript is what makes an archive searchable rather
  than a shelf of video files.
