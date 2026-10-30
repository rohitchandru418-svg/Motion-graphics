# Minsky launch video: SaaS motion graphic scripts

Three versions: **LinkedIn 90s (16:9)**, **Reel/Shorts 30s (9:16)**, **WhatsApp 15s (9:16)**. No person on camera. Everything is animated type, UI mockups, icons and numbers, with a voiceover. Written so an AI can build it as code (rendered frames or HTML/Canvas) and stitch it with ffmpeg.

## Part 1. What you need to provide to the AI

Put everything in one folder called `assets/` and use these exact file names so the AI can reference them.

**Logos (most important)**

- `startuptn_logo.svg` (or transparent PNG, at least 1000 px wide). Ask for a version that works on a dark background. If they only have a dark-on-white logo, also send `startuptn_logo_dark_version.png`, or the AI will place it on a white rounded card.
- `famein_logo.svg`: the FaMeTN logo, same rules. (Named `fametn_logo.svg` in this doc; use that name.)
- `minsky_logo_white.svg` and `minsky_logo_green.svg` (white and green versions, transparent background).
- Optional: `minsky_icon.svg` (just the symbol) for corners and the loading animation.

**Brand**

- Exact hex colours from the brochure (dark background, green accent, any secondary). If you can't get them, tell the AI to sample them from the brochure PDF. I have used placeholders below.
- Font files (`.ttf` or `.otf`) used in the brochure/deck. If none, the AI uses Inter or Space Grotesk (free).

**Audio**

- `voiceover.mp3` or `.wav`: generated with ElevenLabs, either now or later (see Part 1B). One set of files per version, because pacing differs. A human recording also works if you prefer it.
- `music.mp3`: low, minimal, royalty-free, 100 to 120 BPM, at least 95 seconds long.
- Optional SFX pack (soft whoosh, click, tick, pop). If none, the AI can generate simple synthesised ones.

**Content and permissions**

- Written confirmation that StartupTN and FaMeTN are happy to be named and have logos shown.
- Confirmation of the numbers (10,000+ applications, 80% less manual screening, 30 days faster, 10,000+ matches, 3,500+ meetings), and how "30 days faster" was measured.
- Contact details for the end card: minsky.in, info@minsky.in, +91 89251 96011.
- Sample (fake) data for the UI mockups. The AI can invent it (fake startup names, fake scores). Never use real applicant data.
- Optional: 2 to 3 screenshots of the real product, blurred or with sample data. If not provided, the AI builds stylised UI mockups.

**Not needed:** photos of people, founder footage, event photos. (The old script used event photos; this one replaces them with animated UI so there are no permission issues.)

## Part 1B. Voiceover with ElevenLabs: now or later

The video can be built in either order, because every scene's length is read from a timing file, not hard-coded. Pick one path.

**Path 1: generate the voiceover first (recommended).** Frames sync to the real audio, so numbers land on the exact spoken word.

1. Choose a voice in ElevenLabs. For this audience, pick a calm, warm Indian-English voice (male or female), conversational, not announcer-style. Listen to the "80 percent" and "thirty days" lines before you commit.
2. Give the AI the **voice ID** and set your API key as an environment variable (`ELEVENLABS_API_KEY`). Never paste the key into the script or the prompt text.
3. The AI generates **one audio file per scene**, not one long file, so you can re-record a single line without re-rendering the whole video. Names: `vo/A_s01.mp3` ... `vo/A_s09.mp3` (Version A), `vo/B_s01.mp3` ... (Version B), `vo/C_s01.mp3` ... (Version C).
4. Ask the AI to use ElevenLabs' text-to-speech endpoint that returns **character-level timestamps**, and save them as `vo/A_s01.json` etc. The caption engine and the number counters read these, so a caption word and a counter hit exactly when spoken.
5. Suggested starting settings (adjust by ear): stability about 45 percent, similarity about 75 percent, style exaggeration low (0 to 15 percent), speaker boost on, speed 0.95 to 1.0. Use the highest-quality model on your plan. Output MP3 at 44.1 kHz, 128 kbps or higher (or WAV if you want zero re-encoding).
6. Ask the AI to measure each file with `ffprobe` and write `timing.json` (scene id, start, duration, caption word times).

**Path 2: build first, voiceover later.** The AI renders a silent draft using estimated durations (about 2.5 words per second, plus 0.4 s between sentences) and writes the same `timing.json`. When the ElevenLabs files arrive, drop them into `vo/`, run the re-time step, and re-render. Expect small changes in scene length; that is why scenes must hold on the last word, not on fixed times.

**Text to send to ElevenLabs (differs slightly from on-screen text).** Write words the way they should be spoken. Keep on-screen text unchanged.

| Scene | Text sent to ElevenLabs | Why |
| --- | --- | --- |
| S1 | Ten thousand startups applied for Tamil Nadu government funding. Every application was read by hand. | Numbers spelled out so they are read naturally. |
| S2 | Passed around on email. Scored on spreadsheets. | Short, punchy; slight pause between the two lines. |
| S3 | That's what we walked into at Startup T N. So we didn't bolt a chatbot on top. We rebuilt how the work moves. | "StartupTN" spelled "Startup T N" so the letters are said one by one. |
| S4 | Now AI reads the pitch deck, checks the G S T and tax records, and ranks every startup, with evidence behind each score. And here's the part that matters most: the committee still decides. AI ranks. People decide. Every step is logged. | "GST" spelled "G S T". Insert a half-second pause after "People decide." |
| S5 | The result? Eighty percent less manual screening. Applications moving thirty days faster. | Short sentences, a beat between the two results. |
| S6 | Same engine, different problem. For Fa Me T N, over ten thousand buyer and seller matches, and more than three and a half thousand meetings, planned automatically. | "FaMeTN" spelled "Fa Me T N". Check it by ear. |
| S7 | Government is the hardest place to ship AI. Huge volumes. Public scrutiny. Audits. If it holds up there, it'll hold up in your business. | A short pause between each fragment. |
| S8 | If your team still spends its days collecting, checking and forwarding things by hand, let's talk. Thirty minutes. We'll look at where your workflow gets stuck, and tell you honestly whether AI even makes sense. If it does, a two-week, fixed-price sprint maps the whole thing. | Slow slightly on "Thirty minutes." |
| S9 | Minsky. Details below. | Say it "Min-skee" if the default sounds wrong; test a few spellings. |

**Pauses.** Depending on the ElevenLabs model, a pause is either a `<break time="0.5s" />` tag in the text or a short audio tag or a plain full stop. Ask the AI to test the chosen model and use whichever works. If none does, the AI inserts 0.5 s of silence between separately generated sentences with ffmpeg (`adelay` or `apad`).

**Quality check before rendering.** Listen to every file once. Reject any file with a wrong number, a mispronounced brand name or a flat read. Regenerate only that scene.

**Things to give me or the AI when you are ready:** the chosen voice ID, which ElevenLabs plan/model you have, and whether the key will be set as an environment variable.

## Part 2. Master build prompt (paste this first)

> You are building a SaaS-style motion graphic video entirely in code. No stock footage, no people on screen. Use the scene tables below as the single source of truth.
>
> **Pipeline.** Render each scene as a sequence of PNG frames (Python with Pillow/Skia/Cairo, or HTML/CSS/Canvas via Playwright/Puppeteer, whichever you can make deterministic). Frame rate 30 fps. Compute every animation from a function `f(t)` where `t` is seconds into the scene, so it is frame-accurate. Encode each scene to an intermediate MP4 with ffmpeg (`-c:v libx264 -pix_fmt yuv420p -crf 16`), then concatenate scenes with short crossfades or hard cuts as specified, and mix audio (voiceover at 0 dB, music ducked to -22 dB under voice, SFX at -14 dB). Final export: H.264 High, AAC 192 kbps, `-movflags +faststart`, yuv420p, constant 30 fps.
>
> **Timing.** Scene durations below are targets. Measure the real length of the voiceover file for each scene (use `ffprobe`) and stretch or trim scene hold times so every number and word lands on screen at the moment it is spoken. Never let a scene end before its voiceover line finishes; add 0.25 s of breathing room after each line.
>
> **Design system.**
>
> - Background: `#0B0F0D` (near black, slightly green). Surface cards: `#141A17` with 1 px border `#26302B` and 16 px radius. Accent green: `#22C55E` (replace with the brochure value). Text primary `#F4F7F5`, secondary `#9AA8A0`. Warning/chaos colour used only in the "before" state: `#F59E0B` (amber). No other colours.
> - Font: Inter (or the brand font). Headlines 700 weight, body 500. Numbers use tabular figures. Min body size on 1080p frame: 40 px; headlines 96 to 160 px. For 9:16, headlines 88 to 140 px.
> - Subtle background: 4% opacity dot grid (48 px spacing) that drifts very slowly (8 px per second) plus a soft radial green glow (`#22C55E` at 8% opacity, 900 px radius) behind the focal element.
> - Motion language: everything eases. Entrances use cubic-bezier(0.22, 1, 0.36, 1) (ease-out-expo feel) over 0.5 to 0.7 s, with 24 px upward slide and fade. Exits are 0.25 s fade plus 12 px slide. Numbers count up with ease-out over 1.2 s. Stagger list items by 0.12 s. Add 3% overshoot only on pop-in icons. Never linger on static frames for more than 1.5 s: add a micro-motion (slow 2% scale push, glow pulse).
> - Captions: burned in, bottom third (16:9: centred at y=900 px; 9:16: centred at y=1500 px, inside safe zone), white 700 weight, 52 px (16:9) or 60 px (9:16), max 2 lines, max 32 characters per line, a 10% black pill behind. Show one sentence at a time, synced to voiceover. Highlight numbers in accent green.
> - Transitions: default is a 0.3 s crossfade with 2% scale push. Use a hard cut on beat only where marked.
> - Safe zones for 9:16: keep all text inside the central 900 x 1500 px area (60 px side margins, 220 px top, 420 px bottom) so UI chrome on Reels/Shorts does not cover it.
>
> **Deliver.** `minsky_90s_16x9.mp4`, `minsky_30s_9x16.mp4`, `minsky_15s_9x16.mp4`, plus the project folder with the render script so scenes can be tweaked and re-rendered individually. After rendering, extract one frame per scene and check for text overflow, text outside safe zones, and audio/caption sync before finishing.

## Part 3. Version A: LinkedIn, 90 seconds, 1920x1080 (16:9)

**Voiceover tone:** calm, confident, human, not salesy. About 150 words per minute, short sentences, a half-second pause after "People decide." Contractions are fine.

**Music:** low minimal electronic pulse, starts at 0:00 at -22 dB, swells slightly at the results scene (0:41), dips under CTA.

### Scene 1. Hook (0:00 to 0:08)

- **Voiceover:** "Ten thousand startups applied for Tamil Nadu government funding. Every application was read by hand."
- **Visual:** Dark background. In the centre, a large counter counting from 0 to **10,000+** (160 px, accent green, tabular numerals) over 2.5 s. Behind it, small white application cards (260 x 340 px, each with a title bar, three grey text lines and a tiny "PDF" tag) pour in from the top in a staggered cascade, slowly piling up behind the number at 25% opacity. By 0:05 the pile fills the frame.
- **On-screen text:** below the number, fades in at 0:03: "applications. All read by hand." (56 px, secondary text colour).
- **SFX:** soft tick on each count milestone (every 1,000); a low thud at 10,000+.
- **Transition out:** quick 0.3 s blur-out.

### Scene 2. The mess (0:08 to 0:15)

- **Voiceover:** "Passed around on email. Scored on spreadsheets."
- **Visual:** Pile of cards falls away. Left half: simple email envelope icons fly between three small avatar circles (grey placeholders, no faces), looping lines in amber. Right half: a spreadsheet grid (8 columns x 6 rows) with cells filling in randomly with numbers; some cells highlight amber with a small red-free "?" badge. Everything slightly jittery (1 px random offset every 4 frames) to feel chaotic. Labels in 40 px: "Email" (left) and "Spreadsheets" (right).
- **On-screen text:** centred at the bottom: "Slow. Scattered. Hard to audit." (amber, fades in at 0:12).
- **SFX:** quick layered email "swish" sounds; soft keyboard ticks.

### Scene 3. The pivot (0:15 to 0:23)

- **Voiceover:** "That's what we walked into at StartupTN. So we didn't bolt a chatbot on top. We rebuilt how the work moves."
- **Visual:** The chaos wipes away with a green horizontal line sweep (0.4 s). The StartupTN logo (`startuptn_logo.svg`) fades in centre at 360 px wide inside a rounded card, with a soft green glow. After 2 s it shrinks to the top-left corner (as a small badge, 180 px wide) and the frame splits into two cards side by side: left card "Bolted-on chatbot" showing a chat bubble stuck on top of a messy block, then a green strike-through animation draws across it and it fades to 40% opacity; right card "Rebuilt workflow" showing four small blocks connected by clean green lines, drawing in one by one, with a green tick.
- **On-screen text:** headline at the top: "Not a chatbot. A rebuilt workflow." (96 px). The words "A rebuilt workflow." are accent green.
- **SFX:** swoosh for the sweep; soft chime when the green tick appears.

### Scene 4. How it works: five steps (0:23 to 0:41)

- **Voiceover:** "Now AI reads the pitch deck, checks the GST and tax records, and ranks every startup, with evidence behind each score. And here's the part that matters most: the committee still decides. AI ranks. People decide. Every step is logged."
- **Visual:** A horizontal pipeline of five rounded nodes across the screen, each 280 x 200 px with an icon, a label and a one-line description, connected by thin lines with a travelling green dot:
  1. **Understand** (document icon): "Reads the pitch deck"
  2. **Reason** (magnifier icon): "Checks GST and tax records"
  3. **Decide** (bar-chart icon): "Ranks every startup"
  4. **Act** (arrow icon): "Sends the shortlist"
  5. **Learn** (loop icon): "Improves with feedback" Each node lights up (green border, 20% green fill) exactly as the matching words are spoken (nodes 1 to 3 during the first sentence, 4 and 5 during "Every step is logged"). Below the pipeline, from 0:27 to 0:33, a mock "ranked list" panel slides up: 5 rows of fake startup names (e.g. "Greenleaf Agritech", "MedLoop", "Vaayu Labs", "KrishiOne", "Tamil Robotics") with a score bar each (92, 88, 85, 81, 79) and a small "Evidence" chip. A "Sample data" label sits bottom-right in 28 px secondary text. At "the committee still decides" (about 0:33), the ranked list gets a green outlined box around it and a human-silhouette icon (simple circle head and shoulders, no face) in a "Committee" badge appears with a green tick; the badge pulses once. At "AI ranks. People decide." a large headline types in centre: **"AI ranks. People decide."** (120 px). Hold with a half-second pause. From 0:38 to 0:41, a thin audit-log panel scrolls on the right with fake lines: "10:42 Deck parsed", "10:42 GST verified", "10:43 Score generated", "10:51 Committee approved" in monospace 28 px, each line fading in.
- **SFX:** soft click on each node lighting; low confirm tone on the Committee tick.

### Scene 5. The result (0:41 to 0:53)

- **Voiceover:** "The result? Eighty percent less manual screening. Applications moving thirty days faster."
- **Visual:** Split screen. Left (label "Before", amber): spreadsheet and email chaos mini-loop. Right (label "After", green): a clean ranked list. A vertical divider slides from left to right. Then two giant stat cards appear side by side, centred:
  - **80%** (180 px green) with "less manual screening" below; count-up 0 to 80 over 1.2 s, starting exactly when "eighty percent" is spoken.
  - **30 days** (180 px green) with "faster" below; the "30" counts up, with a small calendar icon whose pages flip. A small footnote at the bottom, 24 px, secondary colour: "StartupTN screening process." (Edit once the measurement basis is confirmed.)
- **Music:** gentle swell here.
- **SFX:** tick-tick count; thud on landing.

### Scene 6. Same engine, different problem (0:53 to 1:05)

- **Voiceover:** "Same engine, different problem. For FaMeTN, over ten thousand buyer and seller matches, and more than three and a half thousand meetings, planned automatically."
- **Visual:** The FaMeTN logo (`fametn_logo.svg`) appears in a card (300 px) at top-left. Centre: a network animation. Two columns of small circular nodes, left labelled "Buyers" and right "Sellers" (12 nodes each). Thin green lines draw between matching pairs one by one, accelerating until dozens connect. Then the lines resolve into a simple meeting schedule grid (time slots as rows, tables as columns) with coloured blocks filling automatically. Counters appear on the right: **10,000+ matches** (count up), then **3,500+ meetings** (count up), each 120 px.
- **SFX:** soft pops as lines connect; a gentle ping on each counter landing.

### Scene 7. Why it holds up (1:05 to 1:15)

- **Voiceover:** "Government is the hardest place to ship AI. Huge volumes. Public scrutiny. Audits. If it holds up there, it'll hold up in your business."
- **Visual:** Dark centred frame. Three horizontal chips drop in one by one, each with an icon: "Huge volumes" (stack icon), "Public scrutiny" (eye icon), "Audits" (clipboard with tick). Each chip locks with a green outline. Then the three chips slide together and merge into a single shield icon with a green tick. The headline types in below it: **"If it holds up there, it holds up for you."** (88 px; "holds up for you" in green).
- **SFX:** three soft thuds, one chime on the shield.

### Scene 8. Call to action (1:15 to 1:25)

- **Voiceover:** "If your team still spends its days collecting, checking and forwarding things by hand, let's talk. Thirty minutes. We'll look at where your workflow gets stuck, and tell you honestly whether AI even makes sense. If it does, a two-week, fixed-price sprint maps the whole thing."
- **Visual:** A clean card (1200 x 600 px) in the centre with three lines appearing one by one, each with a green tick icon on the left:
  1. "30-minute call"
  2. "Honest fit check"
  3. "No pitch" After the third line, a second smaller row slides in below: a timeline graphic with two dots labelled "2-week sprint" and "Fixed price" connected by a green line. The call-to-action line is highlighted at the exact moment "fixed-price sprint" is spoken.
- **SFX:** soft tick on each line.

### Scene 9. End card (1:25 to 1:30)

- **Voiceover:** "Minsky. Details below."
- **Visual:** Background settles to the darkest tone with the green glow centred. The Minsky logo (white) fades in at 420 px wide. Beneath it, three lines of contact text fade in one at a time (48 px): "minsky.in", "info@minsky.in", "+91 89251 96011". Music fades out over the last 2 seconds.

## Part 3B. Version A storyboard (90s, 1920x1080)

This is the frame-by-frame plan for the AI. Each scene lists keyframes (K1, K2...) with a time inside the scene, what is on screen, where it sits (pixel coordinates on a 1920x1080 canvas, origin top-left) and the motion. Times are targets; the real times come from `timing.json` (Part 1B).

**Global layout grid.** 120 px outer margin. Content area 1680 x 840 px, centre at (960, 540). Caption zone: y 880 to 1000, centred. Top badge zone: y 60 to 160. Corner logo zone: top-left (120, 70), max 200 x 70 px.

**Recurring elements (build once, reuse):**

- `Card(x, y, w, h)`: fill `#141A17`, 1 px border `#26302B`, radius 16, soft shadow (0 24 60 rgba(0,0,0,0.45)).
- `Chip(label, icon)`: 72 px tall pill, 28 px horizontal padding, 40 px label, icon 36 px.
- `Counter(value, suffix, size)`: tabular numerals, count-up with ease-out, suffix in secondary colour at 40% of the number size.
- `GlowPulse`: radial green glow behind the focal element, 8% to 14% opacity, 3 s breathing loop.
- `CaptionBar`: bottom zone, one sentence at a time.

### S1. Hook (0:00 to 0:08)

**Goal:** make the size of the problem felt in one second.

- **K1 (0.0 s):** Empty dark frame, dot grid visible, no glow. One thin green line (2 px, 160 px long) draws at (960, 520).
- **K2 (0.4 s):** The counter appears at centre (960, 480), starting at 0, 180 px, green. It counts to 10,000 over 2.4 s with ease-out, and a `+` fades in at the end. Beneath it at y=620, the label fades in at 3.0 s: "applications. All read by hand." (56 px, secondary colour).
- **K3 (0.4 s to 5.0 s):** Application cards (260 x 340) fall from above at random x positions from 120 to 1800 and settle into a loose pile behind the number, 25% opacity, staggered every 0.08 s, 40 cards total. Each card has a title bar (12 px, white 30%), three text lines and a tiny "PDF" tag in the corner.
- **K4 (5.0 s):** The pile fills the frame to the bottom. A very slow 2% zoom-in on the whole composition begins.
- **K5 (7.6 s):** 0.3 s blur-out of everything for the cut to S2.
- **Audio cues:** tick at every 1,000; low thud at 10,000+.

### S2. The mess (0:08 to 0:15)

**Goal:** show manual work as slow, scattered and risky.

- **K1 (0.0 s):** The pile of cards slides down and out in 0.5 s. The left half and right half of the frame become two cards: left Card (120, 200, 780, 560) labelled "Email", right Card (1020, 200, 780, 560) labelled "Spreadsheets". Labels at the top of each card, 40 px, secondary colour.
- **K2 (0.5 s to 4.5 s):** Left: three grey avatar circles (96 px, no faces, initials like "A", "R", "K") placed in a triangle at (330, 430), (690, 430), (510, 640). Envelope icons (56 px, amber outlines) fly between them along curved paths, 6 at a time, looping. Right: an 8 x 6 spreadsheet grid, cells 90 x 70 px, light grey lines; cells fill with random numbers one by one every 0.1 s, and about 6 random cells flash amber with a small "?" badge.
- **K3 (1.0 s onwards):** Everything on the cards has a 1 px random jitter every 4 frames to feel messy.
- **K4 (5.0 s):** Caption-style text at bottom centre (960, 880): "Slow. Scattered. Hard to audit." amber, 56 px, fades in with a 24 px rise.
- **K5 (6.7 s):** Both cards dim to 30% opacity and slide 40 px outward, ready for the wipe.
- **Audio cues:** soft email swishes, keyboard ticks.

### S3. The pivot (0:15 to 0:23)

**Goal:** introduce StartupTN and the key idea: rebuilt workflow, not a chatbot.

- **K1 (0.0 s):** A green vertical-to-horizontal line sweep (4 px, full width) wipes the mess away in 0.4 s from left to right, leaving the clean dark background.
- **K2 (0.5 s):** StartupTN logo (`startuptn_logo.svg`) appears in a Card at centre (960, 480), 460 x 260 px with the logo at 360 px wide, with GlowPulse behind it. Rise-and-fade in 0.6 s.
- **K3 (2.3 s):** The card shrinks and moves to the top-left corner (120, 70) as a 200 px wide badge over 0.6 s. The headline fades in at the top centre (960, 190): "Not a chatbot. A rebuilt workflow." 96 px; the second sentence in green.
- **K4 (3.4 s):** Two Cards appear side by side below the headline: left (180, 330, 720, 430) titled "Bolted-on chatbot"; right (1020, 330, 720, 430) titled "Rebuilt workflow".
- **K5 (3.8 s to 5.5 s):** Left card shows a messy block (rounded rectangle in grey) with a small chat bubble icon stuck on top; a green strike-through line draws across the whole card in 0.4 s, and the card fades to 40%. Right card shows four small blocks (160 x 100) in a row connected by green lines drawing in left to right, 0.3 s per block, then a green tick scales in at the end with 3% overshoot.
- **K6 (7.6 s):** Everything fades out in 0.3 s.
- **Audio cues:** swoosh on the wipe; soft chime on the tick.

### S4. How it works: five steps (0:23 to 0:41)

**Goal:** make it easy to understand what happens, and show the human stays in control.

- **K1 (0.0 s):** Five node Cards (280 x 200) appear in a row, spaced 40 px apart, centred at y=360 (x = 160, 480, 800, 1120, 1440 as left edges, with the whole row centred). Each has an icon (64 px), a title (44 px) and a one-line description (28 px): Understand / document / "Reads the pitch deck"; Reason / magnifier / "Checks GST and tax records"; Decide / bar chart / "Ranks every startup"; Act / arrow / "Sends the shortlist"; Learn / loop / "Improves with feedback". Thin connector lines (2 px, grey) join the nodes and a small green dot waits at node 1.
- **K2 (about 1.0 s):** Node 1 lights up when "reads the pitch deck" is spoken (border becomes green, 20% green fill, icon turns green); the dot travels to node 2 (0.4 s) and node 2 lights at "checks the G S T"; node 3 lights at "ranks every startup".
- **K3 (about 4.0 s):** A "ranked list" panel slides up from the bottom: Card (360, 560, 1200, 380) with 5 rows. Each row: rank number, fake startup name (40 px), score bar (width proportional to score), score value, and an "Evidence" chip. Names and scores: Greenleaf Agritech 92, MedLoop 88, Vaayu Labs 85, KrishiOne 81, Tamil Robotics 79. Bars fill in sequence, 0.12 s stagger. Bottom-right of the card: "Sample data" (24 px, secondary colour).
- **K4 (about 9.0 s):** At "the committee still decides": a green outline (3 px) draws around the ranked list. A "Committee" badge (Chip with a simple head-and-shoulders icon, no face) slides in at the top-right of the panel (1560, 520) with a green tick; the badge pulses once (scale 1.06).
- **K5 (about 11.5 s):** The five-node row and the panel dim to 30%. The headline "AI ranks. People decide." types in at centre (960, 540), 120 px. Hold for 0.5 s of silence after "People decide."
- **K6 (about 14.0 s to 18.0 s):** The headline moves up to y=200 at 64 px. An audit-log Card (480, 380, 960, 420) slides in, with 4 monospace lines (28 px) appearing one by one: "10:42  Deck parsed", "10:42  G S T verified" (written "GST verified" on screen), "10:43  Score generated", "10:51  Committee approved" (this line has a green tick). Title of the card: "Every step is logged".
- **Audio cues:** soft click on each node lighting; confirm tone on the Committee tick; quiet tick for each log line.

### S5. The result (0:41 to 0:53)

**Goal:** land the two numbers with weight.

- **K1 (0.0 s):** Transition: cards clear. A split Card layout appears: left half labelled "Before" (amber, 40 px) and right half labelled "After" (green). Left shows a tiny looping version of the S2 chaos (grid with amber cells). Right shows a clean ranked list of 5 grey bars.
- **K2 (1.5 s):** A vertical divider (3 px, green) slides from the left edge to the centre and the "Before" side dims to 35%.
- **K3 (3.0 s):** Both halves slide out. Two large stat Cards (700 x 520) enter side by side at y=290: left at x=180, right at x=1040.
- **K4 (at the spoken "eighty percent"):** Left card: Counter 0 to 80, suffix "%", 200 px green, over 1.2 s. Label below: "less manual screening" (48 px).
- **K5 (at the spoken "thirty days"):** Right card: Counter 0 to 30 with the word "days" (200 px green); a calendar icon (96 px) above, whose page flips 3 times. Label: "faster" (48 px).
- **K6 (8.0 s):** Footnote at the bottom (960, 1000): "StartupTN screening process." 24 px, secondary colour. (Edit once you confirm how the 30 days was measured.)
- **Audio cues:** music swell begins at K3; tick-tick during each counter; thud on landing.

### S6. Same engine, different problem (0:53 to 1:05)

**Goal:** show the engine works beyond one case.

- **K1 (0.0 s):** Stat Cards slide out. FaMeTN logo (`fametn_logo.svg`) appears in a Card at top-left (120, 70), 300 px wide, rise-and-fade.
- **K2 (0.6 s):** A network diagram appears in the left two-thirds, within Card (120, 200, 1100, 700). Two columns of nodes: left column "Buyers" (12 circles, 44 px, at x=300, y from 300 to 800 evenly spaced), right column "Sellers" (12 circles at x=1040). Column labels at y=250.
- **K3 (1.2 s to 6.0 s):** Green lines (2 px) draw between matching pairs, one every 0.4 s at first and accelerating to one every 0.1 s, until 20+ are connected; each connection pops the two nodes (scale 1.15 and back).
- **K4 (6.0 s):** The lines fade and the circles collapse into a schedule grid: columns "Table 1" to "Table 4", rows "10:00" to "13:00", coloured blocks filling in cell by cell (green 30% fill, 1 px green border), 0.1 s stagger.
- **K5 (at the spoken "ten thousand ... matches"):** Right column: Counter "10,000+" (120 px) at (1500, 400) with label "matches" beneath it.
- **K6 (at the spoken "three and a half thousand"):** Counter "3,500+" (120 px) at (1500, 700) with label "meetings" beneath it.
- **Audio cues:** soft pops as lines connect; ping on each counter.

### S7. Why it holds up (1:05 to 1:15)

**Goal:** turn the government credibility into trust for private buyers.

- **K1 (0.0 s):** Left cards clear. Three Chips drop in one by one, centred horizontally, at y=330, 450 and 570 (each 640 px wide): "Huge volumes" (stack icon), "Public scrutiny" (eye icon), "Audits" (clipboard-with-tick icon). Each drops from 60 px above with a 3% overshoot and locks with a green outline when its words are spoken.
- **K2 (4.0 s):** The three chips slide toward the centre and merge into a single shield icon (220 px, green outline, tick inside) at (960, 440), over 0.7 s.
- **K3 (5.0 s):** The headline types in below at (960, 720): "If it holds up there, it holds up for you." 88 px; "holds up for you" in green. If the line is too wide, break it onto two lines.
- **K4 (9.2 s):** Everything fades out in 0.3 s.
- **Audio cues:** three soft thuds, one chime on the shield.

### S8. Call to action (1:15 to 1:25)

**Goal:** make the next step feel easy and low-risk.

- **K1 (0.0 s):** A Card (360, 220, 1200, 420) appears at centre with the title "Let's talk" (56 px).
- **K2 (1.0 s to 3.5 s):** Three rows appear one by one inside it, each with a green tick on the left and text 64 px: "30-minute call", "Honest fit check", "No pitch". Stagger 0.8 s.
- **K3 (4.5 s):** A second Card (360, 700, 1200, 220) slides up beneath it with a mini timeline: two dots connected by a green line, labelled "2-week sprint" (left dot) and "Fixed price" (right dot), 40 px.
- **K4 (at the spoken "fixed-price sprint"):** The timeline card gets a green border and a brief glow.
- **Audio cues:** soft tick on each row; chime on the timeline.

### S9. End card (1:25 to 1:30)

**Goal:** leave the brand and the contact details on screen.

- **K1 (0.0 s):** All cards fade out. The background settles to `#070A09`, with a green glow centred behind (960, 400).
- **K2 (0.4 s):** Minsky logo (`minsky_logo_white.svg`) fades in at (960, 400), 420 px wide.
- **K3 (1.2 s to 2.6 s):** Three lines appear one by one centred at y=620, 720 and 820, 48 px: "minsky.in" (green), "info@minsky.in", "+91 89251 96011".
- **K4 (3.0 s to 5.0 s):** Hold. Music fades out over the last 2 s. Final frame must be fully static.

### Storyboard checks for the AI

- No text may overlap another element or leave the 1920x1080 frame. Check one extracted frame per scene.
- Every counter must reach its final value on the spoken word (use the word timestamps from ElevenLabs).
- Captions must not collide with cards; if they do, move the cards up by 60 px in that scene.
- Keep "Sample data" labels in S4 and S5 at all times.
- Total length must be 90 s plus or minus 2 s. If the voiceover runs long, shorten holds, never speed up the voice.

## Part 4. Version B: Reels / Shorts / LinkedIn mobile, 30 seconds, 1080x1920 (9:16)

**Voiceover (about 70 words, brisk):** "Ten thousand startups applied for Tamil Nadu government funding. Every application, read by hand. We rebuilt that process with AI. It reads, checks and ranks, and the committee still makes every decision. Eighty percent less manual screening. Thirty days faster. If your team still moves work by hand, let's map your workflow. Thirty minutes, an honest fit check, no pitch. minsky.in."

**Music:** same track, starts at 0:00 at -20 dB, hard cut on the final frame. A new visual every 2 to 3 seconds. Captions are large (60 px) and burned in.

### Scene 1 (0:00 to 0:05). Hook

- **Voiceover:** "Ten thousand startups applied for Tamil Nadu government funding. Every application, read by hand."
- **Visual:** Giant **10,000+** (200 px, green) centred at y=700, counting up in 1.5 s, with application cards cascading behind it from the top. Small line under it: "applications. All read by hand." The StartupTN logo sits small at the top (at y=300, 240 px wide) from 0:02.

### Scene 2 (0:05 to 0:12). The rebuild

- **Voiceover:** "We rebuilt that process with AI. It reads, checks and ranks, and the committee still makes every decision."
- **Visual:** Vertical three-step stack: "Reads" (document icon), "Checks" (magnifier), "Ranks" (bar chart), each a 900 x 180 px card, lighting up green in sequence with a travelling dot between them. Then a "Committee decides" badge slides in at the bottom with a green tick. Headline at the top: **"AI ranks. People decide."** (100 px).

### Scene 3 (0:12 to 0:19). The result

- **Voiceover:** "Eighty percent less manual screening. Thirty days faster."
- **Visual:** Two stacked stat cards, 900 x 520 px each. First **80%** (220 px) with "less manual screening"; then, 1.5 s later, **30 days** with "faster" and the flipping calendar icon. Counts land exactly on the spoken words. Footnote in 24 px: "StartupTN screening process."

### Scene 4 (0:19 to 0:25). Hook to your business

- **Voiceover:** "If your team still moves work by hand, let's map your workflow."
- **Visual:** The ranked-list mockup morphs into a generic workflow diagram labelled "Your workflow" with a pulsing question mark on a stuck step (amber) that turns green when "map" is spoken.

### Scene 5 (0:25 to 0:30). CTA and end card

- **Voiceover:** "Thirty minutes, an honest fit check, no pitch. minsky.in."
- **Visual:** Three tick lines appear quickly ("30 minutes", "Honest fit check", "No pitch"), then cut to the end card: Minsky logo (white, 500 px wide), "minsky.in" in 72 px green beneath it, and a pulsing "Link in bio" pill (LinkedIn: "Link in comments").

## Part 5. Version C: WhatsApp status and broadcast, 15 seconds, 1080x1920 (9:16)

**Voiceover (about 35 words):** "Ten thousand applications, all read by hand. We took eighty percent of that work away, and thirty days off the process. Want to see how? minsky.in."

**Music:** punchy start, -18 dB, ends on the last frame. A new visual every 2 s. Because it autoplays muted in many chats, make the captions and big numbers carry the whole story. Keep the file under 16 MB (use `-crf 23` and `-maxrate 4M -bufsize 8M` if needed, since WhatsApp compresses heavily).

### Scene 1 (0:00 to 0:04). Hook

- **Voiceover:** "Ten thousand applications, all read by hand."
- **Visual:** **10,000+** at 220 px green, counting up fast (1.2 s), with the card pile cascading behind it. Subtext: "applications, all read by hand" (56 px). Small StartupTN logo at the top.

### Scene 2 (0:04 to 0:09). The result

- **Voiceover:** "We took eighty percent of that work away, and thirty days off the process."
- **Visual:** The pile collapses into a clean, ranked list (3 rows of fake data, labelled "Sample data"). Two stat chips drop in: **80%** "less manual work" then **30 days** "faster". Each is 200 px, one in each half of the frame.

### Scene 3 (0:09 to 0:12). Teaser

- **Voiceover:** "Want to see how?"
- **Visual:** A large pulsing play-style button (green circle, white triangle) in the centre with the line "See how Minsky did it" below it. Soft glow pulse.

### Scene 4 (0:12 to 0:15). End card

- **Voiceover:** "minsky.in."
- **Visual:** Minsky logo (white, 500 px) with "minsky.in" in green (80 px) below, and the phone number "+91 89251 96011" in 48 px. Hold for the full last 2 seconds.

## Part 6. Notes and checks before you publish

- **Voiceover quality matters most.** If using TTS, pick a natural, warm voice and ask for a slightly slower pace on the numbers. Reject any read that sounds like a robot reading a list.
- **Plain words first.** The script deliberately avoids "agentic" and "AI-native" and says what happens: it reads, checks, ranks, and people decide.
- **Numbers.** Confirm all five numbers with client data, and fix the footnote wording to say exactly how "30 days faster" was measured.
- **Names and logos.** Only publish with StartupTN and FaMeTN permission. If either declines, replace the logo with the text "a state government startup programme" and "a state government matchmaking event".
- **Sample data.** All startup names, scores and log lines above are invented. Keep the small "Sample data" labels.
- **Political work.** Still left out on purpose, as in the original script.
- **Changes from the original.** The founder-on-camera shots are removed; the honest founder lines ("the part I care about most", "Honestly") are rewritten as neutral brand voice. The "I'm \[NAME\]" sign-off is now just "Minsky. Details below." The event photos are replaced by animated UI.

## Part 7. Quick checklist for the person handing this to the AI

1. Create `assets/` with the logos, fonts, voiceover files and music (Part 1).
2. Paste the Master build prompt (Part 2).
3. Paste one version at a time (Part 3, 4 or 5) and ask the AI to render it.
4. Ask for one preview frame per scene first, approve the look, then render the full video.
5. Final export check: correct resolution, 30 fps, captions in sync, file size under platform limits.
