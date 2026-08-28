# VideoSDK Skills

The fastest way to build real-time video and live streaming into your app with
an AI coding assistant.

This is VideoSDK's official agent skill — a knowledge pack that teaches Claude
Code, Antigravity, Cursor and other skill-capable assistants how to build on
VideoSDK properly. Ask for a feature in your own words, and your assistant works
from the official documentation rather than from recall.

## Quick start

```bash
npx skills add github:videosdk-live/videosdk-skills
```

Restart your assistant, then ask:

> add video calling to my app

It asks a few short questions — your frontend, where auth tokens come from,
which product, and what you're building — then scaffolds a working example with
the features that use case actually needs.

## What this skill helps you build

A telehealth consultation with a waiting lobby. A KYC verification with document
capture. A classroom with a whiteboard and teacher-only controls. A live shopping
stream with polls and buyers brought on stage. A town hall broadcast to
thousands.

Not a generic video demo — the example is shaped by what you said you're
building.

## What's covered

| Area | What you get | Platforms |
|---|---|---|
| **Audio-video calling** | 1:1 and group calls, precall device checks, waiting lobby, screen share, chat, recording | React, JavaScript, React Native, Flutter, Android, iOS |
| **Interactive live streaming** | Host and audience roles, invite on stage, polls, reactions, virtual gifts, live captions | React, JavaScript, React Native, Flutter, Android, iOS |
| **HLS broadcast** | One-way streaming to large audiences, player setup, playback URL delivery | React, JavaScript, React Native, Flutter, Android, iOS |
| **Auth & tokens** | API key setup, a token endpoint on your own backend, or a dashboard token to start with | Node.js, Python, Java, PHP, Go, Ruby, .NET, Rust |
| **Use-case feature sets** | KYC, telehealth, education, live commerce, events & webinars | all |

## Connect the docs MCP server

VideoSDK publishes a documentation MCP server that returns answers matched to
your SDK version, instead of pages your assistant has to version-check by hand:

```
https://docs.videosdk.live/mcp
```

Add it as a **streamable-http** server in your assistant's MCP configuration and
restart. The skill offers to walk you through this the first time it runs, and
works without it too — just more slowly.

## Installation

**Skills CLI** — recommended

```bash
npx skills add github:videosdk-live/videosdk-skills
```

**Manually**, for Claude Code:

```bash
git clone https://github.com/videosdk-live/videosdk-skills.git
ln -s "$PWD/videosdk-skills/skills/videosdk" ~/.claude/skills/videosdk
```

For any other assistant, copy `skills/videosdk/` into its skills directory — it's
plain markdown, nothing to build.

## Requirements

A VideoSDK account for your API key and secret, from the
[dashboard](https://app.videosdk.live). The skill tells you when it needs them
and keeps building around a placeholder while you fetch them, so you get the app
and the request together rather than a blocked session.

## Links

- [Documentation](https://docs.videosdk.live)
- [Dashboard](https://app.videosdk.live)

## License

MIT
