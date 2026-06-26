# Site Analysis Summary: www.venclexta.com

**Analysed:** 2026-06-26 | **Tool:** EDS Site Analyser + Chrome DevTools MCP

---

## Overview

VENCLEXTA is a patient-facing brand site covering two cancer indications: CLL/SLL (Previously Untreated and Previously Treated) and AML. The site uses AbbVie's Platform C (AEM 6.x) with the `abbv-*` CSS class fingerprint. Content is deeply mirrored across the three indication sub-sites, creating strong batch migration opportunities.

| Metric | Value |
|---|---|
| Total pages analysed | 49 |
| Semantic template groups | 14 |
| Unique UI components | 43 |
| Consolidated block types | 35 |
| Anti-EDS patterns found | 2 |

---

## Template Groups

| ID | Template | Pages | Key Blocks |
|---|---|---|---|
| T01 | Homepage | 1 | Hero, Accordion, Image Text, Safety Bar |
| T02 | Indication Hub | 3 | Hero, Background Container, Columns, Section Nav |
| T03 | How It May Help | 3 | Hero, Accordion, Image Text V2, Safety Bar |
| T04 | How It Works (MoA) | 2 | Hero, Video, Columns, Rich Text |
| T05 | Efficacy / Combination Regimens | 5 | Hero, Accordion, Columns, Image Text V2 |
| T06 | Patient Stories | 2 | Hero, Patient Story Card, Video, Rich Text |
| T07 | About the Disease | 8 | Hero, Accordion, Columns, Image Text V2 |
| T08 | Side Effects | 3 | Hero, Accordion, Rich Text, Safety Bar |
| T09 | Dosing Schedule | 3 | Hero, Columns (Ramp-Up), Accordion, Section Nav |
| T10 | Dosing Instructions | 3 | Hero, Columns, Accordion, Section Nav |
| T11 | Resources & Support | 6 | Hero, Columns, CTA, Actions Block |
| T12 | Conversation Guide | 3 | Hero, Accordion, Columns, CTA |
| T13 | VEN Zone Sign-Up | 2 | Hero, Contact Form, Rich Text |
| T14 | Find HCP / Utility | 5 | HCP Finder Tool, Rich Text, Safety Bar |

**Migration efficiency:** Template T07 (About the Disease) covers 8 pages across both CLL and AML indications — the highest-value batch. T11 (Resources & Support) covers 6 pages. Together these 14 pages can be migrated with 2 template builds.

---

## Block Inventory

### Complexity breakdown

| Complexity | Count | Blocks |
|---|---|---|
| Simple (S) | 11 | Rich Text, Titles, CTA, Actions Block, Footer, Section Nav, References, Exit Modal, HCP Gate Modal, Background Container |
| Medium (M) | 18 | Header, Hero (14 variants), Accordion, Columns (5 variants), Image Text V2, Video |
| Complex (C) | 6 | Safety Bar (CLL + AML variants), Inline ISI, Patient Story Card, Contact Form, HCP Finder Tool |

### Custom / Anti-EDS blocks

| Block | Issue | Priority |
|---|---|---|
| Safety Bar | Two indication variants (CLL vs AML) with different ISI copy | P1 — must build |
| Inline ISI | Indication-aware inline statement | P1 — must build |
| Patient Story Card - Video | Inline video + patient profile card hybrid | P2 — custom variant |
| HCP Finder Tool | Google Maps provider locator (abbv-find-a-provider) | P2 — requires Maps API decision |
| Columns - Ramp-Up Schedule | Custom step-based visual calendar for dosing ramp-up | P2 — custom Columns variant |
| Tabs (abbv-tab-* URLs) | Resources pages use abbv-tab-* deep-link pattern | P2 — URL strategy required |

---

## Key Migration Findings

1. **Strong mirroring across indications:** Most content templates are shared verbatim across Previously Untreated CLL, Previously Treated CLL, and AML — 3× page count from a single template build. T07, T08, T09, T10, T11 all follow this pattern.
2. **Safety Bar complexity:** Unlike RINVOQ HCP, Venclexta has two distinct ISI variants — one for CLL/SLL and one for AML. The Safety Bar block needs to support indication-scoped ISI content.
3. **Ramp-Up Schedule (T09):** The CLL dosing ramp-up uses a custom step-by-step visual calendar that does not map to any standard EDS block. Requires a custom Columns variant with step indicators.
4. **HCP Finder (T14):** Uses Google Maps embed (abbv-find-a-provider). This is an Anti-EDS pattern requiring either a third-party map integration or a static "find a doctor" redirect strategy.
5. **Patient Story Card (T06):** Combines a video player with a patient testimonial card layout — requires a custom Video variant or a new block.
6. **No Brand Explorer:** Unlike RINVOQ HCP, Venclexta has no indication-swap runtime block. The indication selector is a standard nav pattern — much simpler to implement.

---

## Recommended Migration Order

1. **Phase 1 (Global blocks):** Header, Footer, Safety Bar (both ISI variants), Inline ISI, Rich Text, Titles — unblocks all template builds
2. **Phase 2 (High-volume templates):** T07 About Disease (8 pages), T11 Resources (6 pages), T05 Efficacy (5 pages), T08 Side Effects (3 pages), T09 Dosing Schedule (3 pages), T10 Dosing Instructions (3 pages) — 28 pages, no Anti-EDS blockers
3. **Phase 3 (Indication Hubs + Homepage):** T02 Indication Hub (3 pages), T03 How It May Help (3 pages), T04 MoA (2 pages), T01 Homepage
4. **Phase 4 (Custom blocks):** Patient Story Card → T06; Ramp-Up Columns → T09 refinement; Conversation Guide accordion → T12
5. **Phase 5 (Dependent on API decisions):** T14 HCP Finder (Maps strategy), T13 VEN Zone Sign-Up (Form replacement)

---

## Next Steps

- Open `site-analysis.html` over HTTP (`python3 -m http.server 4173`) to browse blocks and templates with screenshots
- Review `eds-blocks-analysis.csv` for full component-level detail
- Review `eds-blocks-consolidated.csv` for block reuse opportunities across indications
- Align on HCP Finder strategy (Google Maps vs. static locator) before scheduling T14
- Confirm Safety Bar ISI copy strategy for dual-indication (CLL vs AML) support
- Coordinate with content team on Ramp-Up Schedule visual — determine if a custom block or rich Columns variant is preferred
