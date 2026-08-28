# Telehealth

A doctor consults a patient, 1:1 — sometimes three-way with a specialist or
family member.

**Fits**: audio-video calling, 1:1 or small group. **Doesn't fit**: ILS and
HLS.

**Core:**
- **Precall** (setup-call) — patients are the least technical users a call
  will ever have; check devices before the visit, not during.
- **Waiting Lobby** (setup-call) — the patient waits, the doctor admits.
  This is the ask-to-join token permission plus entry events;
  `../auth.md`'s ask-to-join section carries the both-sides requirement.
  Nothing about it is telehealth-specific: the same setup-call feature
  serves a KYC queue, an interview, office hours, or any flow where
  someone admits arrivals one at a time.
- **Chat using PubSub** (collaboration-in-meeting) — links, instructions,
  dosage spellings.
- **Recording** — see the recording rule in `../use-cases.md` for the
  consent notice.

**Optional:**
- **Virtual Background** and **Noise Suppression** (plugins) — doctors
  join from open clinics.
- **Post Transcription & Summary** (transcription-and-summary) — visit
  notes written from the recording rather than from memory. Where the
  consultation exists to produce a report — an insurance medical exam,
  say — the transcript and summary are closer to the deliverable than
  the call is.
- **Change Input Device** (handling-media) — a device picker inside the
  call, not only in precall. Patients plug in headphones halfway
  through, and the precall check above is no help once the visit has
  started.
