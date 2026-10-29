---
name: hyperframes-video
description: Make motion-graphics videos (reels, captions, title cards, trend recaps, promos) by writing HTML and rendering it to MP4 locally with HeyGen's open-source HyperFrames CLI. Use when asked to create, animate, or render a video from code, or to turn a script, website, or data into a short video.
metadata:
  origin: ECC
---

# HyperFrames Video (HTML to MP4)

HyperFrames is HeyGen's open-source (Apache-2.0) framework that renders an HTML page to video frame by frame. Claude writes the HTML and GSAP animations; the CLI renders a deterministic MP4 or transparent WebM. Local rendering is free and needs no account.

## When to Use

- Short-form video for TikTok, Instagram Reels, or YouTube Shorts (1080x1920).
- Animated captions, lower thirds, title cards, logo stings, and overlays.
- Turning a script, a trend list, a chart, or a website into an explainer or promo.
- Any time a video should be generated from code instead of edited by hand.

Do not use it for editing long raw footage (use a video editor such as Descript), or when the person asked for a different framework.

## How It Works

Requirements: Node.js 22 or newer and FFmpeg on the machine.

1. Install the official HyperFrames agent skills once, so Claude follows the render contract:

   ```bash
   npx hyperframes skills
   ```

2. Scaffold a project and edit `index.html` (the root composition) plus scenes in `compositions/`:

   ```bash
   npx hyperframes init my-video
   cd my-video
   ```

3. Preview live in the browser studio:

   ```bash
   npx hyperframes preview
   ```

4. Validate, then render:

   ```bash
   npx hyperframes check --json
   npx hyperframes render -o out.mp4
   ```

Key rules from the HyperFrames contract:

- Every timed element has `class="clip"`, `data-start`, and `data-duration` (seconds).
- Each composition builds one paused GSAP timeline and registers it on `window.__timelines`.
- No `Date.now()`, unseeded `Math.random()`, network fetches, or timers; everything visible is a function of timeline time.
- Asset paths are relative (`assets/...`); custom fonts need `@font-face` to a file in `assets/fonts/`.

## Cost Guardrails

- Free: `init`, `preview`, `check`, `render` (local), `tts` (local Kokoro model), `transcribe`, `remove-background`.
- Paid or account-bound: `cloud` (HeyGen cloud rendering), `auth`, `lambda`, `cloudrun`, and HeyGen avatars. Only use these when the person explicitly asks.

## Examples

Vertical 15-second reel with animated captions:

```bash
npx hyperframes init trend-reel
# set root data-duration="15", 1080x1920, add caption clips timed to the voiceover
npx hyperframes tts "Three trends your business should post about this week" -o assets/voice.wav
npx hyperframes render -o trend-reel.mp4
```

Turn a website into a promo video:

```bash
npx hyperframes init promo && cd promo
npx hyperframes capture https://example.com --json
# mount the captured components as scenes in index.html, then:
npx hyperframes render -o promo.mp4
```

## References

- Repository: <https://github.com/heygen-com/hyperframes>
- CLI help: `npx hyperframes --help` and `npx hyperframes docs`
