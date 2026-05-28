---
name: fullstack
description: Full-stack patterns (creative frontend + secure backend).
---
## FULLSTACK

### Stack Awareness
Detect framework from `package.json`/imports before coding. Use existing React/Vue/Bootstrap/Tailwind APIs fully — never hand-roll vanilla when a framework is present. Fall back to vanilla only when none detected.

### Frontend
- **Direction**: Commit to a bold tone (minimal, maximalist, retro, editorial, playful, luxury, etc.). Pick one thing that makes it UNFORGETTABLE. Never generic AI slop (Inter/Roboto/Space Grotesk, purple-on-white, predictable cards). Vary themes across generations — don't converge on the same look.
- **Color**: Dominant color + sharp accent. Avoid timid evenly-distributed palettes. Design Tokens in `:root`.
- **Typography**: Import distinctive premium fonts. Pair display + body. Never system default.
- **Layout**: CSS Grid/Flexbox, `rem`/`clamp()`, modular spacing `--space-{xs..3xl}`. Asymmetry, diagonal flow, overlap, grid-breaking.
- **Atmosphere**: Gradient meshes, layered shadows, noise textures, grain overlays, `rgba` borders, decorative flourishes.
- **Motion**: CSS-only preferred. Staggered reveals via `animation-delay`. One big moment > scattered micro-animations. Scroll-triggering and surprise hover states.
- **Standards**: Semantic HTML, WCAG AA, Design Tokens in `:root`.

### Backend
- **API**: RESTful naming, proper HTTP/status. Validate input server-side.
- **Data**: Parameterized queries only. Never string-concatenate SQL.
- **Errors**: Structured `{ error, code }`. Log details server-side; never expose stack/schema to client.
- **Secrets**: Environment variables only. Never hardcode. Keep `.env.example` in repo.
- **Transport**: HTTPS, CSP/X-Content-Type-Options/Strict-Transport-Security, rate limit auth.
- **Logging**: Log IDs and outcomes. Never log passwords, tokens, PII, or full bodies.
