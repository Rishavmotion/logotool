name: logo-taste-test description: >
  Builds or updates the Logo Taste Test tool — a Tinder-style swipe interface that shows clients logos from their selected industry and captures love/hate reactions in real time. Use when the user mentions "taste test", "logo swipe", "brand preference", "swipe logos", "design taste", or wants to onboard clients by capturing visual preferences fast. The tool shows 26 logos per session, records swipe direction and decision time per logo, and outputs: top 3 loved logos, top 3 hated logos, a taste profile summary (e.g. "minimal & geometric", "bold & expressive"), average decision time, and full swipe data. Can run as a standalone onboarding tool or be embedded in a client flow. When building or updating this tool, follow the Logo Taste Test Skill instructions below.

Logo Taste Test Skill

This skill builds and iterates on a Tinder-style logo preference tool used during client onboarding to capture design taste automatically.

How It Works

The client selects an industry, then sees 26 logos one at a time. They swipe right (love) or left (hate) — or use keyboard arrows. The session ends with a results screen.

Outputs
- Top 3 loved logos (with names)
- Top 3 hated logos (with names)
- Taste profile: a 2–4 word descriptor derived from the patterns in what they loved (e.g. "Clean & Geometric", "Bold & Expressive", "Warm & Approachable")
- Average decision time in seconds
- Full swipe log (logo name, decision, time taken)

Build Approach

Single .html file with embedded CSS and JS. No backend needed — all data lives in memory for the session.

Logo data structure — each logo entry needs:
  - name: brand name
  - industry: string matching the selector
  - svg or img src: the visual
  - tags: 2–3 descriptors used to compute the taste profile (e.g. ["minimal", "geometric", "dark"])

Taste profile logic — tally the tags from all loved logos, find the top 2–3 most frequent, map them to a human-readable profile string.

Swipe mechanic — support both drag/swipe (touch + mouse) and keyboard left/right arrows. Show a subtle LOVE / HATE overlay on swipe direction.

Decision time — record Date.now() on card appearance and on swipe, compute delta in seconds. Average across all cards.

Results screen — show loved/hated grids, taste profile as a headline, avg decision time, and a "Copy Results" or "Download PDF" button if the user wants to share with clients.

Stages

Stage 1 — Build the core swipe tool with placeholder SVG logos
Stage 2 — Replace placeholders with real logo assets the user provides
Stage 3 — Polish results screen and taste profile logic
Stage 4 — Add export (copy to clipboard as summary, or print/PDF view)

Output Defaults
- Single .html file
- Embedded CSS with :root variables for easy restyling
- No external dependencies except Google Fonts and optional Lucide icons
- Works offline once loaded

---

name: figma-to-code description: > Converts Figma designs into pixel-perfect, production-quality HTML/CSS code by using both a reference screenshot and Figma's exported CSS. Use this skill whenever the user shares a Figma frame image alongside CSS code (from Figma's "Copy as CSS" feature), or when they mention "Figma to code", "build from Figma", "match this design", "replicate this UI", or are trying to extract a design system from Figma. Also trigger when the user uploads a design.md or asks to generate one from a built page. This skill covers the full workflow: building a page 1:1 from a Figma frame, iterating until pixel-perfect, and generating a reusable design.md system file that can be applied to new app flows.

Figma-to-Code Skill

This skill replicates Figma designs into production-grade HTML/CSS with pixel-level fidelity, then optionally extracts a reusable design.md design system that can be applied to any future app or flow.

How the Workflow Works

The user will typically arrive with one or more of:

A screenshot of a Figma frame (exported image)

Figma CSS pasted from: Right-click frame → Copy/Paste as → Copy code → CSS (all)

A design.md file (generated in a previous session)

An IA flow (sitemap, screen list, or written description of pages/flows)

Match your task to the stage they're at:

What they haveWhat to doScreenshot + CSSBuild the page (Stage 1)Built page, wants refinementIterate to 1:1 (Stage 2)Happy with pageGenerate design.md (Stage 3)design.md + IA flowBuild a full new app (Stage 4)

Stage 1 — Build from Figma Frame

Inputs

Figma frame screenshot (reference image)

Figma CSS (from "Copy code → CSS (all)")

How to use the CSS

Figma's exported CSS contains:

Named layers as CSS class selectors or variable names

Exact values for: font-family, font-size, font-weight, line-height, letter-spacing, color, border-radius, gap, padding, width, height, box-shadow, background

CSS custom properties (variables) if the design uses them

Extract CSS variables first. Before building any markup, scan the CSS for repeated values and promote them to :rootcustom properties:

:root {   --color-primary: #1A1A2E;   --color-surface: #F5F5F5;   --radius-card: 12px;   --font-heading: 'Inter', sans-serif;   /* etc. */ } 

This mirrors what the user did in Figma and is the foundation for their design system.

Build approach

Analyse the screenshot — identify the layout regions (nav, sidebar, main, cards, footer), component hierarchy, spacing rhythm, and visual style.

Map Figma layers to HTML structure — use Figma's layer names as class names where they map cleanly.

Apply extracted CSS values directly — don't approximate. Use the exact pixel, color, and font values from Figma CSS.

Single-file output — deliver everything in one .html file with <style> block and <script> if needed.

Use web-safe or CDN-hosted versions of fonts — if Figma uses a Google Font, import it via @import from Google Fonts CDN.

What to prioritize

Spacing and layout fidelity (gap, padding, margin) — these define the "feel"

Typography: size, weight, line-height, letter-spacing — these define the "voice"

Color accuracy — use exact hex values, not approximations

Component states (hover, active) if visible in the reference

What to skip in Stage 1

Don't invent interactions not visible in the design

Don't add pages or sections not in the frame

Don't use generic/placeholder styling — every value should come from the Figma CSS or be directly visible in the screenshot

Stage 2 — Iterate to 1:1

The user will give feedback like "the card spacing is off" or "that font looks too heavy." Treat this as a pixel-pushing session.

Approach:

Ask the user to point at the specific area if it's unclear

Make the change using the exact CSS value from the original Figma CSS if possible

Re-present the full file after changes (not a diff) so they can visually compare

If they add more Figma frames/CSS for other pages, extract new variables and merge them into the :root block — never duplicate, always reuse

Common corrections to watch for:

Icon sizes (Figma often exports SVG at 1x but displays at 2x)

Font weight 500 vs 600 — check the Figma CSS carefully

Border radius on nested elements — Figma applies radius per-layer, not inherited

Shadow values — copy them verbatim, don't rewrite

Stage 3 — Generate design.md

Once the user says they're happy with the built page, generate a design.md file. This is a portable, human-readable design system document that can be fed into future Claude sessions to recreate the design language.

design.md structure

# [Project/Brand] Design System  ## Brand Identity - Visual tone: [minimal / bold / editorial / soft / etc.] - Personality: [3–5 adjectives] - Reference product: [e.g., "Linear", "Notion", "Stripe Dashboard"]  ## Color Palette | Token | Value | Usage | |---|---|---| | --color-primary | #1A1A2E | CTAs, active states | | --color-surface | #F5F5F5 | Page background | | --color-text | #111111 | Body copy | | ... | | |  ## Typography | Role | Font | Size | Weight | Line Height | |---|---|---|---|---| | Heading 1 | Inter | 32px | 700 | 1.2 | | Body | Inter | 14px | 400 | 1.5 | | Label | Inter | 12px | 500 | 1.4 | | ... | | | | |  ## Spacing Scale [List the spacing values observed in the design — gap, padding, margin rhythm]  ## Radius - Cards: 12px - Buttons: 8px - Inputs: 6px - Pills/tags: 999px  ## Shadow [Copy exact box-shadow values from Figma CSS]  ## Component Patterns [Describe structural patterns: card layout, left nav, top bar, data table, etc. with key measurements]  ## CSS Variables (copy-paste ready) \`\`\`css :root {   /* paste the full :root block here */ } \`\`\`  ## Voice & Tone Notes [Any observations about density, white space philosophy, icon style, illustration use, etc.] 

Key principle: design.md should contain enough information that a future Claude session — with no access to the original Figma file — can build a new screen that looks like it belongs to the same product.

Stage 4 — Build a New App from design.md + IA Flow

The user provides:

Their design.md (generated in Stage 3)

An IA flow — this can be a list of screens, a sitemap, a user journey, or a short written description

2 lines of context: what the product is and who it's for

How to execute

Parse design.md — load all tokens, typography, spacing, and component patterns into context.

Map the IA — identify all screens/pages and their relationships.

Build each screen using only the design language from design.md. Do not introduce new colors, fonts, or spacing outside what's defined.

Use a navigation shell — build a persistent shell (sidebar, top nav, or bottom nav) that links all screens, so the output is a navigable prototype.

Single file or multi-page — for 3 or fewer screens, deliver a single-file tabbed prototype. For 4+ screens, consider a multi-page folder structure or ask the user which they prefer.

What makes this powerful

The user gets entirely new flows and pages that have never existed in Figma, but the design feels like it came from the same designer. The design.md is the "taste transfer" layer.

Output Defaults

Format: Single .html file unless user requests otherwise

CSS: Embedded <style> block with :root variables

Fonts: Google Fonts CDN via @import

Icons: Use inline SVG or a CDN icon set (Lucide, Heroicons, Phosphor) — never emoji as icon substitutes

Responsiveness: Match the Figma frame breakpoint by default; add mobile breakpoint only if user asks

Interactivity: Add hover states, active nav states, and basic tab switching; avoid complex JS unless requested

Common Mistakes to Avoid

Don't guess colors — always use the exact hex from Figma CSS

Don't use Inter as a default — only use it if the Figma CSS specifies it

Don't invent spacing — derive all padding/gap from the Figma CSS values

Don't strip the CSS variables — the :root block is the design system backbone, preserve it

Don't add features — build what's in the frame, nothing more

Don't forget the screenshot — always keep the reference image visible while building; it's the ground truth

Tips for Multi-Frame Sessions

If the user adds more Figma frames over the course of a session:

Each new frame may introduce new CSS variables — merge them into the existing :root block

Flag any conflicts (e.g., two different values for what looks like the same token) and ask the user to confirm which to use

Accumulate patterns into a growing component vocabulary that makes later screens faster to build