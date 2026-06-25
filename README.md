# Rogue LLMs: Securing Prompts and Ensuring Persona Fidelity

A conference talk by [Ben Dechrai](https://bendechr.ai) on what happens when an
LLM stops following its system prompt - leaking its instructions, leaking
embedded secrets, or quietly dropping out of character - and the practical
patterns that keep it on the rails.

The talk is 45-60 minutes, aimed at an engineering audience, and draws on
building [BraidFlow](https://braidflow.io) (a multi-persona AI group chat
platform) and working on AI dev tools at Zencoder.

## What's in here

| Path | What it is |
|------|------------|
| `index.html` | The complete slide deck (self-contained Reveal.js, all CSS inline) |
| `demos/` | Three progressive live demos. Setup and run instructions live in [`demos/README.md`](demos/README.md) |
| `SPEAKER-CHEATSHEET.md` | Pre-talk checklist, per-demo scripts, timing guide, and fallbacks |
| `profile.jpg` / `profile.png` | Speaker photo used on the bio slide |

## Viewing the slides

The deck must be served over HTTP, not opened as a `file://` URL - the Google
Fonts it loads are blocked by CORS on `file://` origins.

```bash
python3 -m http.server 8080
# or
npx serve
```

Then open the printed URL (e.g. `http://localhost:8080`) and press `F` for
fullscreen.

### Controls

| Key | Action |
|-----|--------|
| `Arrows` / `Space` | Navigate |
| `S` | Speaker notes (opens a second window) |
| `B` | Blackout (pause for demos or questions) |
| `F` | Fullscreen |
| `O` | Overview grid |
| `T` | Toggle light / dark theme |

### Light / dark theme

The deck defaults to a dark cyber aesthetic but ships a light theme for bright
rooms and projectors that wash out dark backgrounds. It follows your OS
`prefers-color-scheme` on first load; pressing `T` flips it and remembers your
choice (overriding the OS) via `localStorage`. The light theme keeps the glitch
effects but drops the neon glow, which needs a dark backdrop to read.

## Talk structure

33 slides across 7 sections:

1. **Opening** - the system prompt as a contract, and a 10-second prompt leak
2. **Prompt Leakage** - classic and sneaky extraction attacks, why leaks happen
3. **Persona Failure** - reversion and drift, and the spectrum between them
4. **Detection & Evaluation** - nondeterminism, promptfoo, evals in CI/CD
5. **Defense Patterns** - layered instructions, canary tokens, instruction hierarchy
6. **Threat Modelling** - making prompt security systematic
7. **Closing** - a Monday-morning checklist and resources

The live demos slot into sections 2, 4, and 5. See
[`demos/README.md`](demos/README.md) to set them up and run them.

## A note on the video

The "in the wild" slide references a third-party video that is not distributed
with this repo (it is gitignored for copyright). The slide and its speaker notes
still stand on their own without it. The original clip is by
[@SteveMould](https://www.youtube.com/@SteveMould):
[youtube.com/shorts/GJVSDjRXVoo](https://www.youtube.com/shorts/GJVSDjRXVoo).

## Built with

[Reveal.js](https://revealjs.com) 5.1.0 (loaded from CDN), with the highlight
and notes plugins. No build step - it is a single HTML file.

## License

The slide content and demos are free to learn from and adapt. The speaker photo
and the third-party video clip are not.
