# Education

Building the lecture variant? Also read `modes.md`.

A teacher runs a class. Size decides the shape.

**Fits**: audio-video calling for a class where students talk (up to a few
dozen); ILS for a large one-to-many lecture where students are brought up
to speak. **Doesn't fit**: HLS alone — students need a path to speak or
ask, and HLS is the one-way shape. The product answer decides which of
the two you build: a call means the class shape below, ILS means the
lecture variant.

**Core (class → calling):**
- **Precall** (setup-call) — device check before the lesson starts. A
  teacher fixing audio in the first minute has already lost the room's
  attention, and students join from whatever hardware is to hand.
- **Whiteboard** (collaboration-in-meeting) — the single most
  classroom-defining feature.
- **Screen Share** (handling-media) — slides and worked examples. A
  shared video is silent unless the system audio goes with it, so where
  the teacher plays clips, follow what the screen-share guide says about
  sharing audio for the platform in hand.
- **Chat using PubSub** (collaboration-in-meeting).
- **Mute All / Remove Participant** (control-remote-participant) —
  both ride on the moderator permission, which the teacher holds and
  students don't. That makes them the exception among the controls in
  this file — genuinely enforced rather than merely hidden. Still render
  them for the teacher's role only, per the role rule in
  `../use-cases.md`, and make sure the student token names its
  permissions: a token signed without any defaults to moderator, which
  would hand every student the mute-all and removal this bullet just
  gave the teacher. Keep the moderation list itself in Realtime Store so
  it reads the same for everyone and survives a reconnect.
- **Recording** — lesson capture for absent students.

**Optional (class):**
- **Polls** — a comprehension check the teacher can read at a glance.
  The written guide is the ILS one (audience polls); on the calling side
  build the same thing from the PubSub and Realtime Store guides,
  splitting votes and tally as the poll split in `../use-cases.md`
  describes.
- **Raise Hand** — built on PubSub; the ILS section carries the guide,
  and the same PubSub pattern works in a call.
- **Active Speaker Indication** (handling-media) — who's answering, at a
  glance.
- **Large-room scalability guide** — fetch it once the class runs to
  dozens; it covers the layout and subscription patterns that keep a
  large grid smooth beyond quick-start scale.
- **Post Transcription & Summary** (transcription-and-summary) — a
  searchable transcript of the lesson, and a summary for the students
  who missed it.
- **Virtual Background** and **Noise Suppression** (plugins) — students
  join from noisy homes into a room with a few dozen live mics, and a
  child's bedroom is a background worth hiding.

**Lecture variant (→ ILS)** — the teacher publishes, students join as a
receive-only audience. This is its own core set, not the class list minus
a few things:

- **ILS quick start** — teacher in `SEND_AND_RECV`, students in
  `RECV_ONLY`. See `modes.md`.
- **Precall** (setup-call) — the teacher's green room. At lecture scale
  nobody in the audience can tell them their mic is wrong, so check
  devices and network before going live.
- **Screen Share** (device-management under ILS) — slides carry the
  lecture, so wire it in from the start.
- **Chat** (interaction-in-livestream) — how students ask
  questions at lecture scale.
- **Audience Polls** (interactive-live-streaming) — the one form of
  participation that scales to hundreds; split votes and tally as the
  poll split in `../use-cases.md` describes.
- **Raise Hand** plus **Invite Guest on Stage** (handling-participants;
  the change-mode guide under audience-management covers the same
  promotion) — turns a student into a speaker and back.
- **Recording** — the same lesson-capture reason as the class, and more
  so at lecture scale.

Whiteboard stays with the class build: it's documented under the calling
SDK's collaboration guides, so reach for it there rather than in the
lecture variant.
