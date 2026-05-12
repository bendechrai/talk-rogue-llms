# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A conference talk repository for "Rogue LLMs: Securing Prompts and Ensuring Persona Fidelity" by Ben Dechrai. The talk is 45-60 minutes, aimed at engineering/dev audiences, and draws on experience building BraidFlow (a goal-oriented AI group chat platform). The framing draws on experience building BraidFlow and working on AI dev tools at Zencoder.

The repo contains a Reveal.js slide deck (`index.html`) and three progressive demos in `demos/` illustrating LLM prompt injection and defense techniques.

## Presentation Details

### Serving the slides

The slides must be served via a local web server (not file://) because Google Fonts are blocked by CORS on file:// origins.

```bash
python3 -m http.server 8080
# or
npx serve
# Then open http://localhost:8080
```

### Reveal.js controls

- S: Speaker notes
- B: Blackout screen
- F: Fullscreen
- O: Overview/grid view
- Arrow keys / Space: Navigate

### Slide structure (29 slides, 7 sections)

1. Opening (4 slides): Title with glitch effect, About Me/BraidFlow, The Promise, 10-second leak demo
2. Prompt Leakage (6 slides): Section title, risks, classic attacks, sneakier techniques, why leaks happen, LIVE DEMO 1
3. Persona Failure (4 slides): Section title, reversion example (Kai fitness coach), triggers, spectrum
4. Detection & Evaluation (5 slides): Section title, nondeterminism, promptfoo config, what to test, LIVE DEMO 2, CI/CD
5. Defense Patterns (6 slides): Section title, layered instructions, canary tokens, instruction hierarchy, what doesn't work, LIVE DEMO 3
6. Threat Modelling (3 slides): Section title, template table, integration
7. Closing (4 slides): Monday morning checklist, resources, thank you with contact links

### CSS design system

The presentation uses a cybersecurity-themed aesthetic with these effects:
- Glitch text effect (clip-path animations, skewX, pseudo-elements with data-text attr) - use class `glitch` for headings, `glitch-subtle` for inline text
- Neon glow (text-shadow layers) - use class `neon-blue`, `neon-pink`
- Animated gradient text - use class `gradient-text`
- Glassmorphism cards - use class `glass`, `glass-danger` for red-tinted
- CRT scanlines overlay on code blocks - use class `crt-code`
- Animated gradient border on demo slides - use class `demo-slide`
- Pulsing dots, leak pulse animation, recording dot blink

### Fonts (loaded via Google Fonts CDN)

- `var(--font-display)`: Orbitron - used for title slide, section headings
- `var(--font-heading)`: Outfit - used for slide headings (h2, h3)
- `var(--font-body)`: Space Grotesk - used for body text, paragraphs
- `var(--font-mono)`: JetBrains Mono - used for code blocks, prompt examples

### Color palette (CSS custom properties)

- `--cyber-blue: #00d4ff` - primary accent, links, highlights
- `--cyber-pink: #ff006e` - secondary accent, warnings
- `--cyber-purple: #8b5cf6` - section headers
- `--cyber-green: #00ff88` - success, pass indicators, assistant prompts
- `--cyber-amber: #ffd93d` - attacker prompts, caution
- `--cyber-red: #ff4757` - failures, leaked content, danger

### Reveal.js config

- Resolution: 1280x720 with 0.08 margin
- Transition: fade
- Plugins: highlight.js, notes
- CDN: Reveal.js 5.1.0 via cdnjs.cloudflare.com

### Formatting rules

- No emdashes - use " - " instead
- No curly quotes - use straight quotes only
- No &ensp; or other non-keyboard HTML entities - use regular spaces
- Emoji is fine where it makes sense (e.g. in demo content showing bad LLM behaviour)
- Base font size: 32px (must be readable from the back of a conference room)
- Prompt blocks: 0.72em minimum
- Glass cards: 0.78-0.82em

## Demo Setup

All demos use Ollama (local LLM runtime) with llama3.2:3b as the base model. Install Ollama and promptfoo before running anything.

### Building the models (run once before the talk)

```bash
cd demos/demo-1-vulnerable && ollama create kai-vulnerable -f Modelfile
cd ../demo-3-defended && ollama create kai-defended -f Modelfile
```

### Running demos

```bash
# Demo 1: Interactive prompt extraction against vulnerable model
ollama run kai-vulnerable

# Demo 2: Automated eval against vulnerable model (expect failures)
cd demos/demo-2-eval && promptfoo eval
promptfoo view  # Open HTML results viewer

# Demo 3: Same eval against defended model (expect passes)
cd demos/demo-3-defended && promptfoo eval
```

### Optional: LLM-as-judge assertions

Some commented-out `llm-rubric` assertions in the promptfoo configs can use an external LLM for more nuanced persona checking. Set `OPENAI_API_KEY` or `ANTHROPIC_API_KEY` and uncomment them if desired. All active assertions are deterministic (string contains, regex, JavaScript) and need no API key.

## Architecture

**Demo 1 (`demo-1-vulnerable/`)** - An Ollama Modelfile that embeds a system prompt for "Kai" (a fitness coach persona) containing fake credentials (`acme-cs-v3-7f2a`). Demonstrates how prompts and secrets can be extracted via direct asking, role-play, translation, and JSON extraction attacks.

**Demo 2 (`demo-2-eval/`)** - A `promptfooconfig.yaml` that automates the attacks from Demo 1 plus persona tests. Uses the `kai-vulnerable` Ollama model. 12 test cases: 6 leakage, 5 persona, 1 role-switch. No model build step needed - runs against the model created in Demo 1.

**Demo 3 (`demo-3-defended/`)** - A hardened Modelfile with defense patterns: confidentiality boundary markers (`[SYSTEM INSTRUCTIONS BEGIN/END]`), instruction hierarchy rules, canary tokens, explicit extraction-refusal for all formats, identity lock, and admin override resistance. 14 test cases including admin override and emotional manipulation attacks.

**Presentation (`index.html`)** - Self-contained Reveal.js slide deck (~1390 lines). All CSS is inline. Loaded via CDN. Serve with a local HTTP server.

## Key Files

- `index.html` - The complete slide deck
- `demos/README.md` - Full pre-talk setup checklist and demo run instructions
- `demos/demo-1-vulnerable/Modelfile` - Vulnerable Ollama model definition
- `demos/demo-1-vulnerable/system-prompt.txt` - The vulnerable system prompt (plain text version)
- `demos/demo-2-eval/promptfooconfig.yaml` - Test suite for vulnerable model
- `demos/demo-3-defended/Modelfile` - Defended Ollama model definition
- `demos/demo-3-defended/system-prompt-defended.txt` - The hardened system prompt (plain text version)
- `demos/demo-3-defended/promptfooconfig.yaml` - Test suite for defended model
- `profile.jpg` / `profile.png` - Speaker profile images

## Speaker Info

- Name: Ben Dechrai
- Web: https://bendechr.ai
- Twitter: https://twitter.com/bendechrai
- Email: ben@dechrai.com
- Employer: Zencoder (AI dev tools - IDE plugin + ZenFlow multi-agent orchestration)
- Side project referenced in talk: BraidFlow (goal-oriented AI group chat platform)
