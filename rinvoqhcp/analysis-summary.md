# Site Analysis Summary: www.rinvoqhcp.com

**Analysed:** 2026-06-26 | **Tool:** EDS Site Analyser + Chrome DevTools MCP

---

## Overview

RINVOQ HCP is a multi-indication HCP-facing brand site covering 8 approved indications across Rheumatology, Dermatology, and Gastroenterology. The site uses AbbVie's Platform C (AEM 6.x) with the `abbv-*` CSS class fingerprint throughout.

| Metric | Value |
|---|---|
| Total pages analysed | 45 |
| Semantic template groups | 14 |
| Unique UI components | 44 |
| Consolidated block types | 42 |
| Anti-EDS patterns found | 3 |

---

## Template Groups

| ID | Template | Pages | Key Blocks |
|---|---|---|---|
| T01 | Homepage | 1 | Brand Explorer, Hero, Safety Bar |
| T02 | Specialty Hub Landing | 3 | Brand Explorer, Hero, Tabs, Promo Drawer |
| T03 | Condition Landing | 8 | Hero, Image Text, Accordion, Safety Bar |
| T04 | Efficacy (Clinical Data) | 8 | Hero, Tabs, Accordion, Image Text, Section Nav |
| T05 | Head-to-Head Comparison | 1 | Hero, Tabs, Image Compare Slider, Video |
| T06 | Safety Information | 3 | Hero, Accordion, Inline ISI, Section Nav |
| T07 | Dosing & Lab Monitoring | 3 | Hero, Tabs, Table, Section Nav |
| T08 | Access & Reimbursement | 3 | Hero, Formulary Container, Accordion, Section Nav |
| T09 | Mechanism of Action | 2 | Hero, Video, Interactive Image, Rich Text |
| T10 | Real Patients | 1 | Hero, Video, Rich Text |
| T11 | Expert Insights | 1 | Hero, Video, Accordion |
| T12 | Resources & Support | 2 | Hero, Resource Container, CTA List |
| T13 | Contact a Rep | 1 | Hero, Contact Form |
| T14 | Utility / Redirect | 5 | Varies (redirects, PI, site-map) |

**Migration efficiency:** Templates T03 (8 pages) and T04 (8 pages) each unlock batch migration of 8 pages with a single template build.

---

## Block Inventory

### Complexity breakdown

| Complexity | Count | Blocks |
|---|---|---|
| Simple (S) | 14 | Rich Text, Titles, CTA, Footer, Section Nav, References, Exit Modal, Footnotes Modal, Gold CTA, Section BG Brushstroke |
| Medium (M) | 20 | Header, Hero (11 variants), Accordion, Flexbox Content, Image Text, Image Text V2, Video, Featured Section, Affordability Container, Promo Drawer |
| Complex (C) | 8 | Brand Explorer, Safety Bar, Inline ISI, Tabs, Interactive Image, Image Compare Slider, Formulary Container, Contact Form |

### Custom / Anti-EDS blocks

| Block | Issue | Priority |
|---|---|---|
| Brand Explorer | Unique to RINVOQ; indication-swap runtime logic | P1 — must build |
| Formulary Container | Live AbbVie coverage API (abbv-formulary-dynamic) | P1 — requires API decision |
| Tabs Component | Generates abbv-tab-* phantom deep-link URLs (Anti-EDS) | P2 — URL strategy required |
| Image Compare Slider | Custom draggable slider (Level-Up H2H page only) | P2 — custom variant build |
| Interactive Image | Hotspot overlay image (MoA pages) | P2 — custom build |

---

## Key Migration Findings

1. **Indication depth:** 8 indications × 5 content types (Condition Landing, Efficacy, Safety, Dosing, Access) = ~40 pages following a repeatable template pattern — high batch efficiency.
2. **Tab URL inflation:** Every Tabs block generates `abbv-tab-*` deep-link URLs. ~120 phantom URLs exist in crawl data. These are Anti-EDS and must be resolved before migration.
3. **Brand Explorer:** Appears on Homepage, Specialty Hubs, and Condition Landing pages. This is a P1 custom block — migration of T01/T02/T03 is blocked until Brand Explorer is built.
4. **Formulary Tool (T08):** Access pages use a live formulary coverage lookup (abbv-formulary-dynamic). This requires either a real-time API integration or a redirect strategy.
5. **Level-Up H2H (T05):** Unique template — custom image compare slider, brushstroke backgrounds, and Brightcove video. Complex build, 1 page only.
6. **Safety Bar:** Floating ISI with inline indication statement — required on all pages, P1 custom block.

---

## Recommended Migration Order

1. **Phase 1 (Global blocks):** Header, Footer, Safety Bar, Inline ISI, Rich Text, Titles — unblocks all template builds
2. **Phase 2 (High-volume templates):** T03 Condition Landing (8 pages), T04 Efficacy (8 pages), T06 Safety (3 pages), T07 Dosing (3 pages) — 22 pages with no Anti-EDS blockers
3. **Phase 3 (Custom blocks):** Brand Explorer → unblocks T01, T02; Promo Drawer → T02
4. **Phase 4 (Complex templates):** T08 Access (Formulary decision), T05 H2H (Image Compare Slider), T09 MoA (Interactive Image)
5. **Phase 5 (Utility):** T14 redirects, T13 Contact, T10 Real Patients, T11 Expert Insights

---

## Next Steps

- Open `site-analysis.html` over HTTP (`python3 -m http.server 4173`) to browse blocks and templates with screenshots
- Review `eds-blocks-analysis.csv` for full component-level detail
- Review `eds-blocks-consolidated.csv` for block reuse opportunities across indications
- Align with stakeholders on Formulary Tool strategy before scheduling T08 migration
- Confirm Brand Explorer spec before scheduling T01/T02/T03 migration
