# UI: theme, icons, and the call screen

Read this at the top of SKILL.md's build section, before writing any
screen. It exists because a call screen built without it comes out as
whatever palette and icons the model reaches for first, and two builds in
the same project then look like two products.

Everything below is expressed as values rather than CSS, because it has to
land the same way on React, JavaScript, React Native, Flutter, Android and
iOS.

**Build this with no new dependencies.** No icon package, no CSS
framework, no component library, no styling runtime — a call screen is a
grid, a row of buttons and some overlays, and every platform can express
that with what it already has. Use whatever the project already depends
on: if it has Tailwind, use Tailwind; if it has styled-components, use
those. If it has none of that, plain CSS or the platform's own styling is
the right answer, not an opportunity to introduce one. Where you think
something genuinely can't be built without a package, say so and let the
user decide rather than adding it quietly.

## What applies where

This file has two halves with different scopes, and conflating them is
how a call screen ends up looking like a foreign object bolted onto
someone's product.

**The anatomy always applies** — the control bar and its order, muted
showing as danger rather than greyed out, the speaking ring, the tile
overlays and their states, touch targets, safe areas. That is how a call
screen works, not how VideoSDK looks, and it is right in any app.

**The palette is for projects that don't have one yet.**

- *New project*: use the palette below as it stands.
- *Existing project you can read*: take colors, radius, spacing and type
  from what the app already uses — its theme file, CSS variables,
  `ColorScheme`, asset catalog. Map the roles below onto theirs
  (background, surface, border, text, danger, and one accent) and keep
  the anatomy. Their brand wins; only fall back to the palette below for
  a role the app genuinely doesn't define, and say which ones you filled
  in. An app with no theme layer at all is the same as a new project.
- *Existing project you can't read*: don't guess a palette and don't
  hardcode this one into their screens. Put the roles in one small theme
  file, point them at the app's own values where you can infer them, and
  say in the integration note which role maps to which of their tokens
  so they can correct it in one place.

Two things survive a rebrand because they carry meaning rather than
style: mute and leave stay in the danger color, and the active speaker
ring uses whatever the app's accent is. Don't let a host app's palette
turn a muted mic grey.

## The palette (new projects, or roles the app doesn't define)

**Dark is the default.** This is product chrome for a call, not a
marketing page. Light exists and is listed, but build dark first unless
the user asks otherwise.

| Role | Dark | Light |
|---|---|---|
| Brand accent | `#CDB6FF` | `#CDB6FF` |
| Ink on accent | `#070918` | `#070918` |
| Accent hover | `#EADDFF` | `#B5A1E2` |
| Page background | `#000000` | `#FFFFFF` |
| Background subtle | `#161719` | `#FAFAFA` |
| Background muted | `#1B1B1E` | `#F4F4F5` |
| Surface (card, panel) | `#1B1B1E` | `#FFFFFF` |
| Surface raised | `#27272A` | `#FFFFFF` |
| Surface inset | `#09090B` | `#F4F4F5` |
| Border subtle | `#262626` | `#E4E4E7` |
| Border default | `#303033` | `#D4D4D8` |
| Border strong | `#3F3F46` | `#A1A1AA` |
| Text primary | `#FFFFFF` | `#18181B` |
| Text secondary | `#FFFFFF` at 85% | `#343A3E` |
| Text tertiary | `#A1A1AA` | `#71717A` |
| Text disabled | `#52525B` | `#A1A1AA` |
| Focus ring | `#B5A1E2` | `#9A87C6` (2px, 2px offset) |

One accent, and it is lavender. Status colors are for status only — never
decoration:

| Status | Dark text / border | Light text / bg |
|---|---|---|
| Success | `#BBF7D0` / `#166534` | `#166534` / `#DCFCE7` |
| Danger | `#FECACA` / `#991B1B` | `#991B1B` / `#FEE2E2` |
| Warning | `#FDE68A` / `#854D0E` | `#854D0E` / `#FEF3C7` |
| Info | `#BAE6FD` / `#075985` | `#075985` / `#E0F2FE` |

In dark, the second value is the border at full strength and the fill at
25% alpha over the card surface. Where a platform has no reliable alpha
compositing, use these pre-flattened over `#1B1B1E` for the fill:
success `#1A3327`, danger `#341F1F`, warning `#302A17`, info `#16262F`.

## Shape, space and type

Radius `4 · 5 · 6 · 8 · 12 · 16`, then pill and round. Spacing is a 2px
grid — the steps you'll actually use are `4, 8, 12, 16, 20, 24, 32`.

Type is Inter, and the body size is **14px / 20px line height**, not 16 —
this is dense technical UI. Steps: 10/14, 12/16, **14/20**, 16/24, 18/26,
20/28, 24/32. Weights 400 / 500 / 600 / 700; use 500 for anything that
labels a control.

## Icons — from what the platform already ships

**Add no icon dependency.** A call screen needs about ten glyphs, and no
platform needs a package to draw them. But only two platforms hand you
the glyphs — on the rest you author them, and the difference matters
because referencing an icon you never created fails at compile time, not
where you wrote it.

| Platform | Where the glyphs come from | You must create |
|---|---|---|
| Flutter | `Icons.*` ships with the framework | nothing — reference them directly |
| iOS | SF Symbols via `Image(systemName:)`, built into the OS | nothing — reference them directly |
| React / JS | inline SVG | one small component per glyph, written by you |
| Android | vector drawable XML | one `.xml` file per glyph in `res/drawable/`, written by you |
| React Native | see below — the awkward case | depends; check the manifest first |

**On Android and web, write the files before you reference them.**
`res/drawable/` is the app's own folder and is empty in a new project;
there is no bundled Material set to point `R.drawable.*` at. Create each
vector XML first, then reference it. The same holds for every other
resource a screen reaches for — `@color/`, `@string/`, a font, an image:
if you did not create it and cannot see it in the project, it does not
exist, and naming it anyway produces a build that fails on a missing
resource rather than on anything you can see in the code you wrote.

The visual style is Lucide's, whatever renders it: **2px stroke, round
caps and joins, no fill, `currentColor` so it inherits, a 24×24 viewBox,
drawn at 16–20px.** Pick one family per screen and stay in it — Material
and SF Symbols already match this weight closely enough that a Flutter
app using `Icons.mic_off` and a web app using an inline Lucide path read
as the same product.

The ten controls. Flutter and iOS names are references; the Android
column is the **file you create** in `res/drawable/`, and the web column
is the glyph you draw as a component.

| Control | Web (Lucide name) | Flutter | iOS (SF Symbol) | Android — file to create |
|---|---|---|---|---|
| Mic on / off | `mic` / `mic-off` | `Icons.mic` / `Icons.mic_off` | `mic.fill` / `mic.slash.fill` | `ic_mic.xml` / `ic_mic_off.xml` |
| Camera on / off | `video` / `video-off` | `Icons.videocam` / `Icons.videocam_off` | `video.fill` / `video.slash.fill` | `ic_video.xml` / `ic_video_off.xml` |
| Screen share | `monitor-up` | `Icons.screen_share` | `rectangle.on.rectangle` | `ic_screen_share.xml` |
| Leave | `phone-off` | `Icons.call_end` | `phone.down.fill` | `ic_call_end.xml` |
| Chat | `message-square` | `Icons.chat_bubble_outline` | `message.fill` | `ic_chat.xml` |
| Participants | `users` | `Icons.people` | `person.2.fill` | `ic_people.xml` |
| Raise hand | `hand` | `Icons.pan_tool` | `hand.raised.fill` | `ic_raise_hand.xml` |
| Record | `circle-dot` | `Icons.fiber_manual_record` | `record.circle` | `ic_record.xml` |
| Pin | `pin` | `Icons.push_pin` | `pin.fill` | `ic_pin.xml` |
| More | `ellipsis` | `Icons.more_vert` | `ellipsis` | `ic_more.xml` |

An Android vector drawable is a `<vector>` root with a `<path>` whose
`android:pathData` carries the same path the web column draws, sized to a
24dp viewport and tinted with `android:tint` so one file serves both
states. Twelve small files for the ten controls — mic and camera each
need an on and an off — and no dependency, but they have to exist on disk
before any layout or Compose call names them.

**React Native is the exception**: it ships no vector icon set, and the
VideoSDK React Native SDK doesn't pull an SVG renderer either. So check
the project's manifest first. If `react-native-svg` is already there, use
it and inline the paths as on web. If it isn't, don't add it — say so,
and either use icon images the project already has or ask which the user
prefers. Silently adding a package to draw a microphone is the thing to
avoid.

Pair icon size to control size: a 24px control takes a 14px icon, 32px
takes 16px, 40px takes 18px, 44px takes 20px. An icon-only button needs
an accessible label — `aria-label` on web, `contentDescription`,
`semanticsLabel`, or `accessibilityLabel` on the native platforms.

## The call screen

Regions: a thin top bar, the video grid, an optional right-hand side
panel, and the control bar at the bottom.

**Top bar** — 56px tall, 16px horizontal padding, a 1px bottom border in
the subtle border colour. Carries what identifies the call on the left —
its name, the room id if the user needs to share it — and status on the
right: recording and live badges, in the danger and accent colours rather
than as plain text.

**Control bar** — 76px tall, buttons 44×44 with 8px radius and a 20px
icon, 10px between them, a 1×32px divider to group destructive controls
away from the rest. Order left to right: mic, camera, screen share, then
the secondary group (chat, participants, raise hand, record, more), then
leave. State reads off the icon and the fill: inactive is a transparent
or surface fill with a subtle border; **muted or camera-off is the danger
color, not a greyed-out icon**; leave is danger-filled at rest.

**Participant tile** — 12px radius, minimum 150px tall, 12px grid gap,
16px padding around the grid. Show video when it's on; otherwise a
centred circular avatar (56px, initials at 40% of that) on the muted
surface. Overlays: a name chip bottom-left — 24px tall, 6px radius, 8px
inset, `rgba(0,0,0,0.55)` background, 12px/500 text, with a 13px mic icon
that turns danger when muted — and a 26px pin badge top-left when pinned.

Tile states, and the SDK event each one follows:

| State | Look | Comes from |
|---|---|---|
| Speaking | 2.5px inset ring in accent `#CDB6FF` | active-speaker event |
| Idle | 1px inset `rgba(255,255,255,0.06)` | default |
| Muted | mic icon in the name chip turns danger | mic-state event |
| Camera off | avatar replaces video | webcam-state event |
| Screen share | share stream takes the main area, camera tiles shrink to a strip | screen-share event |
| Pinned | pin badge, tile promoted in the grid | your own pin state |

**Side panel** — 320px wide with a 1px left border, header at 10/12px
padding. Chat and participants share the panel rather than stacking two.

**Grid** — square-ish tiles, not letterboxed 16:9. One participant fills
the area; two split; beyond that use a square-ish grid and cap it, with
the rest as a count. Past a few dozen tiles stop rendering everyone and
read the scalability rule in `use-cases.md`.

**Render the local participant yourself.** The SDK's participants
collection holds everyone *except* the local one, so a grid built by
iterating it alone shows the host an empty screen with no error. Add the
local participant to the list you render — and don't be talked out of it
by the reference page, which calls that collection "all the connected
participants" while the SDK's own typings say "except local
participant".

## On phones

The desktop numbers hold except for touch: the 44px control button is
already the iOS minimum, but Android wants **48dp**, so scale the bar to
48 there. Put the control bar inside the safe area, not flush to the
screen edge. Two tiles stack vertically in portrait rather than sitting
side by side, and the side panel becomes a sheet over the call rather
than a column beside it.

## Expressing the tokens per platform

Define them once as constants and reference them everywhere — the failure
this section prevents is a hex literal typed inline in a component.

| Platform | Where tokens live | Notes |
|---|---|---|
| React / JS | CSS custom properties on `:root` | Theme via a `data-theme="dark"` attribute or `.dark` class — **not** `prefers-color-scheme`; the app owns the flag |
| React Native | one exported constants object | No CSS vars. Shadows: `elevation` on Android, `shadowOffset`/`shadowRadius`/`shadowOpacity` on iOS. No backdrop blur — use a solid `rgba` |
| Flutter | `ThemeData` + `ColorScheme`, `Color(0xFF…)` | Pill = `StadiumBorder`, round = `CircleBorder`; shadows are a `List<BoxShadow>` |
| Android | Compose `ColorScheme`, `Color(0xFF…)` | `RoundedCornerShape`, `CircleShape`; single `elevation` value |
| iOS | asset-catalog colors or a `Color` extension | `Capsule()`, `.clipShape(Circle())`, `.shadow()` per layer |

Two shapes don't survive a literal port: the pill radius is a sentinel
`999px` that means "fully rounded" — use the platform's capsule shape —
and the "inset hairline" used on filled controls is a 1px interior stroke
wearing a shadow's clothes, so express it as a border.
