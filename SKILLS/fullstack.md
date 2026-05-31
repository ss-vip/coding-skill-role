---
name: fullstack
description: Full-stack patterns (creative frontend + secure backend).
---
## FULLSTACK

### Stack Awareness
Detect framework from `package.json`/imports before coding. Use existing React/Vue/Bootstrap/Tailwind APIs fully — never hand-roll vanilla when a framework is present. Fall back to vanilla only when none detected.

### Design Thinking (Frontend)
Before writing any code, establish a clear creative direction:

- **Purpose**: What problem does this interface solve? Who uses it?
- **Tone**: Pick an extreme — brutally minimal, maximalist chaos, retro-futuristic, organic/natural, luxury/refined, playful/toy-like, editorial/magazine, brutalist/raw, art deco, soft/pastel, industrial. Intentionality matters more than intensity.
- **Differentiation**: What makes it UNFORGETTABLE? One thing someone will remember.
- **Constraints**: Framework, performance budget, accessibility requirements.

Vary themes across generations — never repeat the same look twice.

### Frontend Aesthetics

- **Color & Theme**: One dominant color + one sharp accent. Avoid timid evenly-distributed palettes. Define all tokens in `:root`. NEVER default to purple-on-white or generic blue.
- **Typography**: Choose distinctive, characterful fonts. Pair an unexpected display font with a refined body font. NEVER use Inter, Roboto, Arial, or system fonts as primary choices. Import via `@font-face` or `@import`.
- **Layout**: CSS Grid/Flexbox, `rem`/`clamp()` for fluid scaling, modular spacing `--space-{xs..3xl}`. Embrace asymmetry, diagonal flow, overlap, grid-breaking. Generous negative space OR controlled density.
- **Atmosphere & Details**: Gradient meshes, layered shadows, noise textures, grain overlays, geometric patterns, `rgba` borders, decorative flourishes. Add depth beyond flat solid colors.
- **Motion**: CSS-only preferred. Staggered page-load reveals via `animation-delay`. One big orchestrated moment > scattered micro-animations. Scroll-triggered reveals and surprising hover states.
- **Standards**: Semantic HTML, WCAG AA contrast, keyboard navigation, `prefers-reduced-motion`.

**Anti-patterns to avoid**: Overused font families (Inter, Roboto, Arial, Space Grotesk), clichéd color schemes (purple gradient on white), predictable card-grid layouts, generic cookie-cutter components. Match implementation complexity to aesthetic vision — maximalist needs elaborate code, minimalist needs precision in spacing and restraint.

### Backend

- **API Design**: RESTful resource naming (`/users/:id`), proper HTTP methods (GET/POST/PUT/PATCH/DELETE), consistent status codes (200/201/204/400/401/403/404/422/500). Support API versioning via URL prefix (`/v1/`).
- **Request Validation**: Use schema-based validation (Zod, Joi, Yup, or Pydantic) on all inputs. Never trust raw request bodies. Validate type, format, bounds, and required fields server-side.
- **CORS**: Explicitly configure allowed origins, methods, and headers. Never use `Access-Control-Allow-Origin: *` in production with credentials.
- **Data Access**: Parameterized queries only. Never string-concatenate SQL/NoSQL queries. Use ORM or query builder where available.
- **Idempotency**: Use idempotency keys for mutations (POST/PUT) to prevent duplicate processing on retry.
- **Caching**: Set `ETag`, `Cache-Control`, and `Last-Modified` headers on GET responses. Use conditional requests (`If-None-Match`) to reduce server load.
- **Error Handling**: Structured response `{ error: string, code: string }`. Log full error details server-side; never expose stack traces, schema details, or internal paths to the client.
- **Secrets & Config**: Environment variables only. Never hardcode API keys, tokens, or passwords. Keep `.env.example` in repo with placeholder values. Add `.env` to `.gitignore`.
- **Transport Security**: Enforce HTTPS. Set security headers: `Content-Security-Policy`, `X-Content-Type-Options: nosniff`, `Strict-Transport-Security`, `X-Frame-Options: DENY`. Rate-limit authenticated endpoints.
- **Observability**: Structured logging with correlation IDs for each request. Log request IDs, outcomes, and timing. Never log passwords, tokens, PII, or request/response bodies containing sensitive data.
