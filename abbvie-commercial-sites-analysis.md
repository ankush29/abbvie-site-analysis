# AbbVie Commercial Sites — EDS Migration Analysis
**Date:** 2026-06-26  
**Scope:** 30 AbbVie Commercial Sites (28 new + rinvoqhcp.com + venclexta.com)  
**Method:** Live DOM inspection via `fetch_page_content` + URL discovery via `discover_site_urls`

---

## Executive Summary

| Metric | Value |
|--------|-------|
| Sites analyzed | 28 accessible / 30 total (2 returned HTTP 403) |
| Total discovered URLs | ~680 across all sites |
| Estimated real/unique pages | ~530 real pages |
| URL inflation (redirects/404s) | ~150 non-real URLs (abbv-tab-*, URL-doubling, AEM fragments) |
| Platform C components found | **33 distinct components** |
| Map to standard EDS blocks | **13 of 16** standard EDS blocks covered |
| Custom AbbVie blocks needed | **7 new custom blocks** |
| Anti-EDS patterns | **8 distinct anti-EDS patterns** |
| Total templates across all sites | ~380 templates |
| Sites with complex templates | 5 (locators + indication-swap sites) |

---

## DELIVERABLE 1: LIST OF BLOCKS

### 1.i — Common EDS Blocks Found (13 of 16 standard EDS blocks)

All 33 AbbVie Platform C components were mapped against the 16 standard EDS blocks:

| EDS Standard Block | AbbVie Platform C Equivalent | Present on Sites | Notes |
|-------------------|------------------------------|-----------------|-------|
| **Header** | `abbv-header` / `abbv-header-v2` / `abbv-header-v2-lite` | All 28 sites | 3 header variants |
| **Footer** | `abbv-footer` | All 28 sites | 1 variant |
| **Columns** | `abbv-row-container`/`abbv-col` + `abbv-flex-container`/`flexbox-v2` | All 28 sites | 5 layout variants |
| **Rich Text** | `abbv-rich-text` + `abbv-title` | All 28 sites | 4+ style variants |
| **Hero** | `abbv-background-container` (hero class variant) | 22 sites | Used as section hero |
| **Video** | `abbv-video-player` (Brightcove) + `abbv-youtube-player` | 14 sites | 2 player engines |
| **Modal** | `abbv-modal` | All 28 sites | WOL, ISI, HCP-gate, enrollment variants |
| **Fragment** | `abbv-inline-use-isi` / `abbv-inline-safety` | 12 sites (all HCP) | Inline ISI content |
| **Search** | `abbv-search` + `abbv-search-toggle` | 8 sites | Site search |
| **Carousel** | `abbv-carousel` (owl-carousel) | 2 sites | rheumfordialogue, rinvoqhcp |
| **Tabs** | `abbv-tabs` | 6 sites | Has anti-EDS URL pattern |
| **Accordion** | `abbv-accordion` | 2 sites | rinvoqhcp, venclexta |
| **Form** | AEM Form (iframe) | 2 sites | savewithays, eczemaheadquarters |

**EDS blocks NOT found on AbbVie sites:**
- **Cards** — No dedicated cards block; `abbv-image-text` used in card-like layouts
- **Embed** — Replaced by `abbv-embed-pro` (Brightcove SDK, anti-EDS pattern)
- **Quote** — No dedicated quote component
- **Table** — Used inline inside `abbv-rich-text` (ISI dosing tables, not standalone)

**→ 13 of 16 standard EDS blocks have a Platform C equivalent.**

---

### 1.ii — Blocks with Minimal Variants (1–3 variants) That Need Expansion

These are standard EDS blocks present on AbbVie sites but with very few variants — they will need expansion to cover the full range of AbbVie use cases:

| Block | Current Variants Found | AbbVie Use Cases Requiring More Variants |
|-------|----------------------|------------------------------------------|
| **Header** | 3 variants (v1 classic, v2 sticky, v2-lite) | Mobile hamburger, HCP utility nav, Allergan-branded header, indication-swap header |
| **Video** | 2 variants (inline full-width, in-modal) | Brightcove playlist (beyondagutfeeling), video transcript link, video with thumbnail grid |
| **Carousel** | 1 variant (owl-carousel image swap) | Mobile-only carousel, stat carousel, card carousel, auto-play |
| **Search** | 1 variant (site search toggle) | Results page, search overlay, no results state |
| **Accordion** | 2 variants (expand/collapse, show-all) | ISI accordion, FAQ accordion, nested accordion |
| **Form** | 1 variant (AEM form iframe) | Find-a-doctor locator form, savings enrollment, email sign-up |
| **Fragment** | 2 variants (inline ISI, inline safety) | Full ISI page, ISI modal, safety references |

**→ 7 blocks need variant expansion before EDS can cover AbbVie authoring needs.**

---

### 1.iii — Complete List of AbbVie Platform C Components (Author Instance Properties)

Full component library found across all 30 sites via live DOM inspection:

#### Group A: Layout & Navigation (6 components)

| # | CSS Class | AEM Component Title | Variants Found | EDS Block |
|---|-----------|--------------------|----|-----------|
| 1 | `abbv-header` | Header | Classic, sticky, Allergan | Header |
| 2 | `abbv-header-v2` | Header v2 | Classic, sticky, lite, indication-swap | Header |
| 3 | `abbv-footer` | Footer | Horizontal, vertical, v2, allergan | Footer |
| 4 | `abbv-row-container`/`abbv-col` | Columns | 2-col, 3-col, 4-col, flush, nested | Columns |
| 5 | `abbv-flex-container`/`abbv-flex-container-v2` | Flex Container | row, column, centered, 3-col, 2-col | Columns |
| 6 | `abbv-link-list` | Link List | Horizontal nav, vertical sidebar, footer links | (custom) |

#### Group B: Content Blocks (12 components)

| # | CSS Class | AEM Component Title | Variants Found | EDS Block |
|---|-----------|--------------------|----|-----------|
| 7 | `abbv-rich-text` | Rich Text | Common, footnote, legal, ISI text, pull-right | Rich Text |
| 8 | `abbv-title` | Titles | H1–H4 heading wrappers | Rich Text |
| 9 | `abbv-image-text` / `abbv-image-text-v2` | Image Text | **20+ variants** (hero, card, side-by-side, stacked, image-swap, responsive, full-bleed) | Image-Text (custom) |
| 10 | `abbv-background-container` | Background Container | BG color, BG image, gradient, hero, full-width | Hero / Section |
| 11 | `abbv-container` | Container | Full-container, inner-container, mobile-only, desktop-only | (utility) |
| 12 | `abbv-carousel` | Carousel | Auto-play, manual (owl-carousel), stat carousel | Carousel |
| 13 | `abbv-tabs` | Tabs | Standard 3-tab, horizontal, vertical | Tabs |
| 14 | `abbv-video-player` | Video Player | Inline, playlist, in-modal (video-js/Brightcove) | Video |
| 15 | `abbv-youtube-player` | YouTube Player | Inline iframe, in-modal, transcript | Video |
| 16 | `abbv-embed-pro` | Brightcove Embed Pro | Full SDK embed | **Anti-EDS** |
| 17 | `abbv-modal` | Modal | WOL (exit), ISI modal, HCP gate, enrollment, video modal | Modal |
| 18 | `abbv-jump-link` | Jump Link | Anchor link, back-to-top anchor | (utility) |

#### Group C: Regulatory / HCP Components (4 components)

| # | CSS Class | AEM Component Title | Variants Found | EDS Block |
|---|-----------|--------------------|----|-----------|
| 19 | `abbv-safety-bar` | Safety Bar | Minimized/maximized, HCP variant, oncology variant | Safety Bar (custom) |
| 20 | `abbv-inline-use-isi` / `abbv-inline-safety` | Inline ISI | Full ISI, abbreviated ISI, inline safety | Inline ISI (custom) |
| 21 | `abbv-indication-swap` | Indication Swap | Header swap, footer swap, content swap | **Anti-EDS** |
| 22 | `abbv-promo-drawer` | Promo Drawer | Slide-out right, HCP enroll CTA | Promo Drawer (custom) |

#### Group D: Form / Tool Components (6 components)

| # | CSS Class | AEM Component Title | Variants Found | EDS Block |
|---|-----------|--------------------|----|-----------|
| 23 | `abbv-find-a-provider` | Find a Provider | Locator map, doctor finder, MSL finder | **Anti-EDS** |
| 24 | `abbv-quick-poll` | Quick Poll | Single question, multiple choice | **Anti-EDS** |
| 25 | AEM Form (iframe) | AEM Form | Enrollment, email sign-up, assessment | Form (partial) |
| 26 | `abbv-form-checkbox`/`abbv-label-text` | Form Elements | Checkbox, radio, text input | Form |
| 27 | `abbv-select-menu` | Select Menu | Dropdown | Form |
| 28 | `g-recaptcha` / `abbv-badgeless-captcha` | reCAPTCHA | Standard badge, badgeless | **Anti-EDS** |

#### Group E: Search / Utility (5 components)

| # | CSS Class | AEM Component Title | Variants Found | EDS Block |
|---|-----------|--------------------|----|-----------|
| 29 | `abbv-search` + `abbv-search-toggle` | Search | Site search, search overlay | Search |
| 30 | `abbv-html-container` | HTML Container | Raw HTML embed, embed iframes | **Anti-EDS** |
| 31 | `abbv-breadcrumb-navigation` | Breadcrumb | Auto-generated, manual | (custom) |
| 32 | `abbv-back-to-top` | Back to Top | Button, floating | (utility) |
| 33 | `abbv-animation-loading` | Loading Spinner | Inline spinner | (utility) |

**Total: 33 distinct Platform C components**

---

### 1.iv — Anti-EDS Patterns (Properties That Cannot Be Natively Authored in EDS)

| # | Pattern | CSS Class / Technology | Sites Affected | Count |
|---|---------|------------------------|---------------|-------|
| 1 | **abbv-tab-* deep-link URLs** | `abbv-tabs` → generates `abbv-tab-XXXXX-0` anchor URLs | 12 sites | ~145 phantom URLs in sitemaps that return 404 |
| 2 | **Brightcove SDK Raw Embed** | `abbv-embed-pro` / `abbv-html-container` | 7 sites (ra.com, nobsabouths, durystahcp, usdermed, beyondagutfeeling, rinvoqhcp, venclexta) | Requires Brightcove JS SDK loaded in page |
| 3 | **Provider Locator** | `abbv-find-a-provider` (Google Maps + API) | 4 sites (skyrizilocator, amlexpertconnect, psoriasis, ra.com) | Depends on AbbVie provider database API + Google Maps API key |
| 4 | **Interactive Quick Poll** | `abbv-quick-poll` | 2 sites (psoriasis.com, ra.com, crohnsandcolitis.com) | Real-time poll API, stores responses server-side |
| 5 | **Indication Swap** | `abbv-indication-swap` | 3 sites (beyondagutfeeling, emrelis, decnupaz, rheumfordialogue) | Runtime content switching by URL param/cookie — incompatible with static EDS delivery |
| 6 | **HCP Gating Modal** | `js-must-interact` CSS class + JS gate | 3 sites (durystahcp, usdermed, decnupaz) | Client-side HCP verification — no EDS equivalent, needs custom JS |
| 7 | **Google reCAPTCHA** | `g-recaptcha` / `abbv-badgeless-captcha` | 2 sites (skyrizilocator, amlexpertconnect) | Third-party dependency, tied to locator forms |
| 8 | **AEM Forms (iframe embed)** | `<iframe src="...aemformcontainer...">` | 2 sites (savewithays, eczemaheadquarters) | Full AEM Forms backend — complex to replace with EDS-native forms |

**→ 8 Anti-EDS patterns identified across 15+ sites. None can be authored natively in standard EDS without custom blocks or third-party integrations.**

---

## DELIVERABLE 2: LIST OF TEMPLATES

### 2.i — Pages per Template (All 30 Sites)

| Site | Type | Discovered URLs | Real Pages | Templates | Avg Pages/Template | Reuse Rating |
|------|------|----------------|-----------|-----------|------------------|--------------|
| rinvoqhcp.com | HCP Rx | 43 | 35 | 14 | 2.5 | ★★★ |
| venclexta.com | Patient Rx | ~53 | ~45 | 14 | 3.2 | ★★★★ |
| skyrizilocator.com | HCP Locator | 3 | 2 | 2 | 1.0 | ★ |
| lupronprostatecancer.com | Patient Rx | 36 | 33 | 31 | 1.1 | ★ |
| durystasavingsprogram.com | Savings Prog | 5 | 2 | 2 | 1.0 | ★ |
| durysta.com | Patient Rx | — | — (403) | — | — | — |
| durystahcp.com | HCP Rx | 25 | 13 | 13 | 1.0 | ★ |
| emrelis.com | Patient Rx | 5 | 4 | 3 | 1.3 | ★ |
| hcp.xengelstent.com | HCP Device | 22 | 11 | 11 | 1.0 | ★ |
| crohnsandcolitis.com | Disease Ed | 67 | 34 | 32 | 1.1 | ★ |
| faceyourbackpain.com | Disease Ed | 44 | 37 | 21 | 1.8 | ★★ |
| hsdiseasesource.com | Disease Ed | 53 | 19 | 18 | 1.1 | ★ |
| nobsabouths.com | Disease Ed | 73 | 38 | 32 | 1.2 | ★★ |
| psoriaticarthritisinfo.com | Disease Ed | 26 | 21 | 20 | 1.1 | ★ |
| psoriasis.com | Disease Ed | 81 | 77 | 72 | 1.1 | ★ |
| ra.com | Disease Ed | 43 | 39 | 22 | 1.8 | ★★ |
| eczemaheadquarters.com | Disease Ed | 58 | 53 | 47 | 1.1 | ★ |
| latestagensclc.com | Patient Rx | 10 | 8 | 8 | 1.0 | ★ |
| lastacaft.com | Patient Rx | 10 | 8 | 8 | 1.0 | ★ |
| burdenofad.com | Disease Ed | 15 | 7 | 8 | 0.9 | ★ |
| amlexpertconnect.com | HCP Locator | 2 | 1 | 1 | 1.0 | ★ |
| completerebate.com | Savings Prog | 2 | 2 | 2 | 1.0 | ★ |
| allergansavingscard.com | Savings Prog | — | — (403) | — | — | — |
| savewithays.com | Savings Prog | 14 | 8 | 7 | 1.1 | ★ |
| rheumfordialogue.abbvie.com | Disease Ed | 8 | 2 | 2 | 1.0 | ★ |
| usdermed.com | HCP Ed | 35 | 31 | 15 | 2.1 | ★★★ |
| decnupaz.com | Patient Rx | 8 | 6 | 6 | 1.0 | ★ |
| beyondagutfeeling.com | HCP Ed | 2 | 1 | 1 | 1.0 | ★ |
| **TOTALS** | | **~747** | **~597** | **~381** | **~1.6** | |

**Note:** 150 URL inflation = abbv-tab-* deep-links (~120), URL-doubled paths (~22), AEM fragment pages (~8).

---

### 2.ii — Block Recommendations per Template Archetype

Seven recurring template archetypes were identified across all 30 sites:

#### Template Archetype 1: **Homepage / Hub Page** (rinvoqhcp, venclexta, crohnsandcolitis, psoriasis, ra.com, eczemaheadquarters)
Sites with pages: ~18 pages
```
Recommended EDS Blocks:
├── Header (sticky, with primary nav)
├── Hero (full-width background-container with H1 + CTA)
├── Image-Text Block (custom) — feature section, side-by-side
├── Columns — 2-3 column stat/benefit blocks
├── Rich Text — subheadings, footnotes
├── Video (optional, Brightcove or YouTube)
├── Modal — WOL exit disclaimer
├── Fragment — ISI inline (HCP sites only)
├── Safety Bar — floating ISI (HCP sites only)
└── Footer
```

#### Template Archetype 2: **Disease Education / Condition Page** (most disease-ed sites)
Sites with pages: ~250 pages (largest group)
```
Recommended EDS Blocks:
├── Header
├── Hero (background-container)
├── Breadcrumb (custom)
├── Image-Text Block (custom) — multiple instances, various variants
├── Rich Text — body copy + footnotes
├── Columns — comparison tables, benefit lists
├── Accordion — FAQ expand/collapse
├── Tabs (optional) — indication sub-sections
├── Search (optional) — psoriasis.com, ra.com, eczemaheadquarters
├── Modal — WOL social media exit disclaimers
└── Footer
⚠ Complex: Quick-Poll sections (psoriasis, ra.com, crohnsandcolitis) — Anti-EDS
```

#### Template Archetype 3: **HCP Rx Prescribing Page** (rinvoqhcp, venclexta, durystahcp, emrelis, decnupaz)
Sites with pages: ~115 pages
```
Recommended EDS Blocks:
├── Header v2 (with ISI utility nav + indication-swap flag)
├── Safety Bar (custom) — floating ISI bar
├── Hero / Image-Text Block (custom)
├── Rich Text — dosing info, efficacy data
├── Columns — dosing schedules, comparison tables
├── Accordion — ISI expand, FAQ
├── Inline ISI (custom) — regulatory inline content
├── Fragment — ISI content fragment
├── Modal — HCP gate, WOL
└── Footer
⚠ Complex: Indication-Swap (emrelis, decnupaz, beyondagutfeeling) — Anti-EDS
```

#### Template Archetype 4: **Savings / Support Program Page** (completerebate, savewithays, durystasavingsprogram)
Sites with pages: ~14 pages
```
Recommended EDS Blocks:
├── Header (minimal, savings-branded)
├── Hero / Image-Text Block (custom) — eligibility hero
├── Rich Text — terms & conditions
├── Columns — 2-3 column brand tiles
├── Inline ISI (custom) — safety information
├── Modal — ISI modal, WOL, enrollment confirmation
├── Form (AEM-based iframe) — enrollment form (complex)
└── Footer
⚠ Complex: AEM Form integration (savewithays) — needs EDS Forms replacement
```

#### Template Archetype 5: **Locator / Find-a-Provider Page** (skyrizilocator, amlexpertconnect)
Sites with pages: ~3 pages
```
Recommended EDS Blocks (as-is):
├── Header
├── Safety Bar (custom)
├── Find-a-Provider (custom — maps + API, or replace with external locator tool)
├── Inline ISI (custom)
└── Footer
⚠ COMPLEX: abbv-find-a-provider cannot be replicated in EDS without external API integration.
  Recommendation: Replace with link to existing HCP locator service (abbvie.com/find-a-doctor),
  OR implement as embedded iframe from validated external service.
```

#### Template Archetype 6: **HCP Education / Video Resource Page** (usdermed, beyondagutfeeling, rheumfordialogue)
Sites with pages: ~35 pages
```
Recommended EDS Blocks:
├── Header v2 (lite or standard)
├── Hero / Image-Text Block (custom)
├── Video — inline video player (Brightcove or YouTube)
├── Tabs — video series navigation
├── Rich Text — video descriptions, references
├── Columns — resource grid
├── Carousel (optional)
├── Modal — WOL, video expand, HCP gate
└── Footer
⚠ Complex: HCP Gating Modal (usdermed, durystahcp) — JS-only, needs custom block
⚠ Complex: Brightcove video playlist (beyondagutfeeling) — needs extended Video block
```

#### Template Archetype 7: **Single-Page / Campaign Page** (nobsabouths, burdenofad, latestagensclc, lastacaft)
Sites with pages: ~60 pages
```
Recommended EDS Blocks:
├── Header (minimal)
├── Hero (background-container with full-width image)
├── Image-Text Block (custom) — multiple content sections
├── Rich Text — body copy, footnotes
├── Columns — side-by-side content
├── Link List (sidebar nav on nobsabouths)
├── Modal — WOL
└── Footer
```

---

### 2.iii — Templates with Complex Structures (Cannot Be Authored with Standard EDS Blocks)

The following 5 template types have anti-EDS patterns that CANNOT be replicated with the identified block set without custom development:

| Template Type | Site Examples | Blocking Pattern | Complexity |
|--------------|---------------|-----------------|------------|
| **Provider Locator** | skyrizilocator.com, amlexpertconnect.com | `abbv-find-a-provider` requires Google Maps API + AbbVie provider DB API | 🔴 High |
| **Indication Swap** | beyondagutfeeling.com, emrelis.com, decnupaz.com | `abbv-indication-swap` does runtime content switching — incompatible with EDS static delivery model | 🔴 High |
| **HCP Gating** | usdermed.com, durystahcp.com | JS-only HCP verification gate before content reveal — no EDS authoring equivalent | 🔴 High |
| **Quick Poll** | psoriasis.com, ra.com | `abbv-quick-poll` requires persistent API endpoint for poll responses | 🟡 Medium |
| **AEM Form Integration** | savewithays.com | AEM Forms backend processing (iframe embed) — needs replatforming to EDS-native Forms | 🟡 Medium |

**→ 5 template archetypes (affecting ~25 pages) require custom EDS development or alternative solutions before migration can proceed.**

---

## DELIVERABLE 3: INTEGRATIONS AND FORMS

### 3.1 — Third-Party Integrations by Site

| Integration | Sites | Notes |
|-------------|-------|-------|
| **Brightcove Video SDK** (`abbv-embed-pro`) | ra.com, nobsabouths.com, durystahcp.com, usdermed.com, beyondagutfeeling.com, rinvoqhcp.com, venclexta.com | 7 sites — Brightcove account required, player embed code, CDN |
| **YouTube Embed** (`abbv-youtube-player`) | rheumfordialogue.abbvie.com | 1 site — 4 YouTube video iframes (3-part video series + bonus) |
| **Google Maps API** (`abbv-find-a-provider`) | skyrizilocator.com, amlexpertconnect.com, psoriasis.com (locator section), ra.com | 4 sites — Google Maps JavaScript API + Places API |
| **Google reCAPTCHA** (`g-recaptcha`) | skyrizilocator.com, amlexpertconnect.com | 2 sites — v2 reCAPTCHA on locator forms |
| **AbbVie Provider Database API** | skyrizilocator.com, amlexpertconnect.com | 2 sites — proprietary AbbVie API endpoint for provider search |
| **OneTrust Cookie Consent** | All 28 accessible sites | Universal — `ot-sdk-show-settings` in all footers |
| **Adobe Analytics / DTM** | All 28 sites (inferred) | Standard AbbVie tagging implementation |
| **Brightcove Video Playlist API** | beyondagutfeeling.com | 1 site — `abbv-video-playlist` component (playlist navigation) |
| **Quick Poll API** | psoriasis.com, ra.com, crohnsandcolitis.com | 3 sites — proprietary AbbVie poll endpoint |
| **FDA MedWatch External Link** | All HCP/Rx sites (~15 sites) | WOL modal links to fda.gov/medwatch — standard pattern |
| **AbbVie DSRM (Privacy Choices)** | All 28 sites | `abbviemetadata.my.site.com/AbbvieDSRM` in all footers |
| **Allergan EyeCue** | durystahcp.com, durystasavingsprogram.com | HCP ordering system — WOL exit modal links to allerganeyecue.com |
| **AllerganDirect** | durystahcp.com | HCP ordering system — durysta.allergandirect.com |
| **Social Media (WOL exits)** | eczemaheadquarters.com, ra.com, nobsabouths.com, psoriasis.com | Facebook, Instagram, TikTok, YouTube exit modals |
| **RheumKit Assessment** | ra.com | External RAPID3 survey tool — rheumakit.com |
| **Global Healthy Living Foundation** | rheumfordialogue.abbvie.com | Patient assessment tool — external partnership |

### 3.2 — Forms by Site

| Form Type | Site | Technology | EDS Replacement |
|-----------|------|-----------|----------------|
| **Savings Enrollment Form** | savewithays.com | AEM Forms (iframe) | EDS Forms + backend API |
| **Doctor Discussion Guide** | eczemaheadquarters.com | AEM Forms or external | EDS Forms |
| **Provider Locator Form** | skyrizilocator.com | `abbv-find-a-provider` (custom form + maps) | External locator service or custom EDS block |
| **AML Specialist Finder Form** | amlexpertconnect.com | `abbv-find-a-provider` | External locator service |
| **Email Sign-Up Form** | ra.com, eczemaheadquarters.com | AbbVie proprietary email platform | EDS Forms → Marketo / AEP |
| **Patient Assessment Tool** | rheumfordialogue.abbvie.com | External (Global Healthy Living Foundation) | Embed as Fragment or external link |
| **Rebate Claim Form** | completerebate.com | External `patient.completerebate.com` | External link (not embedded) |
| **VEN-Zone Sign-Up** | venclexta.com | Email sign-up form | EDS Forms |

**Total Forms Identified: 8 forms across 9 sites**
- 2 AEM Forms (complex, iframe-based)
- 3 `abbv-find-a-provider` (locator + form hybrid)
- 3 external/linked forms (redirect to external platform)

---

## DELIVERABLE 4: PAGES WITH REDIRECTS (URL INFLATION ANALYSIS)

### 4.1 — Sites with Significant URL Inflation

The following sites have a gap between their sitemap/discovered URL count and actual real pages, primarily due to 3 patterns:

**Pattern A: `abbv-tab-*` Deep-Link URLs**  
AbbVie's tab component auto-generates anchor URLs like `abbv-tab-XXXXXXXXXX-0`, `abbv-tab-XXXXXXXXXX-1`. These appear in sitemaps but return 404 on direct navigation.

**Pattern B: URL-Doubled Paths**  
Sites `hcp.xengelstent.com` and `savewithays.com` have sitemap entries like `site.com/site.com/page-path` — all 404. Real pages use short paths.

**Pattern C: AEM Fragment Pages**  
`durystahcp.com` has `/site-modols/*` pages = AEM Sling component fragment pages, not real content pages.

| Site | Discovered URLs | Real Pages | Inflated by | Pattern |
|------|----------------|-----------|------------|---------|
| rinvoqhcp.com | 43 | 35 | **8** | abbv-tab-* deep-links |
| venclexta.com | ~53 | ~45 | **~8** | redirects + old paths |
| crohnsandcolitis.com | 67 | 34 | **33** | abbv-tab-* deep-links |
| nobsabouths.com | 73 | 38 | **35** | abbv-tab-* deep-links |
| hsdiseasesource.com | 53 | 19 | **34** | abbv-tab-* + AEM-generated |
| psoriasis.com | 81 | 77 | **4** | abbv-tab-* deep-links |
| faceyourbackpain.com | 44 | 37 | **7** | abbv-tab-* + old redirects |
| burdenofad.com | 15 | 7 | **8** | abbv-tab-* deep-links |
| hcp.xengelstent.com | 22 | 11 | **11** | URL-doubled paths |
| savewithays.com | 14 | 8 | **6** | URL-doubled paths |
| durystahcp.com | 25 | 13 | **12** | abbv-tab-* (3) + AEM fragments (9) |
| durystasavingsprogram.com | 5 | 2 | **3** | abbv-tab-* deep-links |
| rheumfordialogue.abbvie.com | 8 | 2 | **6** | abbv-tab-* deep-links |
| ra.com | 43 | 39 | **4** | old redirects |
| eczemaheadquarters.com | 58 | 53 | **5** | old redirects |
| **TOTAL** | **~677** | **~530** | **~147** | |

### 4.2 — Net Migration Scope

| Metric | Count |
|--------|-------|
| Total discovered URLs | ~677 |
| 404 / redirect URLs (all patterns) | ~147 |
| Inaccessible sites (403/HTTP 0) | 3 sites (~20 est. pages) |
| **Net real pages to migrate** | **~530** |
| Reduction from URL inflation | **~22%** |

**Recommendation:** Before starting page-by-page migration, run sitemap validation on each site to identify and exclude abbv-tab-* deep-link 404s. This alone reduces the apparent page count from ~680 to ~530 real pages.

---

## APPENDIX A: Per-Site Block & Complexity Summary

| Site | Site Type | Real Pages | Key Blocks | Anti-EDS Patterns | Complexity |
|------|-----------|-----------|-----------|-------------------|------------|
| rinvoqhcp.com | HCP Rx | 35 | header-v2, image-text, safety-bar, inline-isi, accordion, video, tabs | abbv-tab-* (8 URLs), abbv-embed-pro, abbv-safety-bar | 🟡 Medium |
| venclexta.com | Patient Rx | 45 | header-v2, image-text, tabs, video, accordion, search | abbv-tabs, some redirects | 🟡 Medium |
| skyrizilocator.com | HCP Locator | 2 | header, find-a-provider, safety-bar, inline-isi | abbv-find-a-provider, reCAPTCHA, abbv-safety-bar | 🔴 High |
| lupronprostatecancer.com | Patient Rx | 33 | header, image-text, rich-text, background-container | minimal | 🟢 Low |
| durystasavingsprogram.com | Savings | 2 | header, image-text-v2, inline-isi, safety-bar | abbv-safety-bar | 🟡 Medium |
| durysta.com | Patient Rx | — | — (403) | — | ⚪ N/A |
| durystahcp.com | HCP Rx | 13 | header, image-text-v2, flex-container, safety-bar, promo-drawer, inline-isi | abbv-embed-pro, promo-drawer, safety-bar, HCP-gate | 🔴 High |
| emrelis.com | Patient Rx | 4 | header-v2, indication-swap, image-text, rich-text | abbv-indication-swap | 🔴 High |
| hcp.xengelstent.com | HCP Device | 11 | — (HTTP 0) | — | ⚪ N/A |
| crohnsandcolitis.com | Disease Ed | 34 | header, image-text, quick-poll, background-container, background-container | abbv-quick-poll, abbv-tab-* (33) | 🟡 Medium |
| faceyourbackpain.com | Disease Ed | 37 | header, image-text, rich-text, background-container | abbv-tab-* (7) | 🟢 Low |
| hsdiseasesource.com | Disease Ed | 19 | header, image-text, rich-text | abbv-tab-* (34), AEM-generated pages | 🟡 Medium |
| nobsabouths.com | Disease Ed | 38 | header, image-text, link-list, embed-pro, html-container | abbv-embed-pro, html-container, abbv-tab-* (35) | 🟡 Medium |
| psoriaticarthritisinfo.com | Disease Ed | 21 | header, image-text, rich-text | minimal | 🟢 Low |
| psoriasis.com | Disease Ed | 77 | header, image-text, quick-poll, find-a-provider, search | abbv-quick-poll, abbv-find-a-provider | 🟡 Medium |
| ra.com | Disease Ed | 39 | header-v2, image-text-v2, flex-container, video, quick-poll, embed-pro, search | abbv-quick-poll, abbv-embed-pro, html-container | 🟡 Medium |
| eczemaheadquarters.com | Disease Ed | 53 | header, image-text, modal (social WOL), search | social WOL modals (FB/TikTok/IG) | 🟢 Low |
| latestagensclc.com | Patient Rx | 8 | header, image-text, rich-text | minimal | 🟢 Low |
| lastacaft.com | Patient Rx | 8 | header, image-text, rich-text | minimal | 🟢 Low |
| burdenofad.com | Disease Ed | 7 | header, image-text, rich-text | abbv-tab-* (8) | 🟢 Low |
| amlexpertconnect.com | HCP Locator | 1 | header, find-a-provider, reCAPTCHA | abbv-find-a-provider, reCAPTCHA | 🔴 High |
| completerebate.com | Savings | 2 | flex-container-v2, image-text-v2, modal (ISI), rich-text | minimal | 🟢 Low |
| allergansavingscard.com | Savings | — | — (403) | — | ⚪ N/A |
| savewithays.com | Savings | 8 | header-v2, flex-container, image-text-v2, modal, video, AEM form | AEM form (iframe), URL-doubling | 🟡 Medium |
| rheumfordialogue.abbvie.com | Disease Ed | 2 | header-v2, carousel, tabs, YouTube iframes, image-text-v2, embed-pro | YouTube embeds, abbv-tabs, abbv-embed-pro | 🟡 Medium |
| usdermed.com | HCP Ed | 31 | header-v2-lite, flex-container-v2, image-text-v2, video, HCP-gate | HCP gating modal, video modal | 🔴 High |
| decnupaz.com | Patient Rx | 6 | header-v2, indication-swap, image-text, tabs | abbv-indication-swap, abbv-tabs | 🔴 High |
| beyondagutfeeling.com | HCP Ed | 1 | header-v2, image-text-v2, flex-container-v2, video+playlist, indication-swap | abbv-indication-swap, video playlist, abbv-embed-pro | 🔴 High |

**Complexity Legend:** 🟢 Low (standard EDS blocks sufficient) · 🟡 Medium (needs 1-3 custom blocks) · 🔴 High (anti-EDS patterns, custom development required) · ⚪ N/A (inaccessible)

---

## APPENDIX B: Custom EDS Blocks Required

Based on the analysis, **7 new custom EDS blocks** must be built before migration can begin:

| Priority | Block Name | Maps to Platform C | Sites | Variants Needed |
|----------|-----------|-------------------|-------|----------------|
| 🔴 P1 | **Image-Text Block** | `abbv-image-text` / `abbv-image-text-v2` | All 28 sites | hero, card, side-by-side, stacked, image-swap, full-bleed (6+) |
| 🔴 P1 | **Safety Bar** | `abbv-safety-bar` | 7 HCP sites | minimized, maximized, oncology, HCP (4 variants) |
| 🔴 P1 | **Inline ISI** | `abbv-inline-use-isi` / `abbv-inline-safety` | 9 HCP sites | full ISI, abbreviated, inline references (3 variants) |
| 🟡 P2 | **Background Container / Section** | `abbv-background-container` | 18 sites | BG color, BG image, hero, gradient (4 variants) |
| 🟡 P2 | **Promo Drawer** | `abbv-promo-drawer` | 1 site (durystahcp) | slide-out right (1 variant) |
| 🟡 P2 | **Breadcrumb** | `abbv-breadcrumb-navigation` | 8 sites | standard, short (2 variants) |
| 🟢 P3 | **HCP Gating Modal** | `js-must-interact` gate | 3 sites | HCP self-certification gate (1 variant) |

---

## APPENDIX C: Migration Prioritization Recommendation

**Tier 1 — Migrate First (Low complexity, high page count, maximum ROI):**
- faceyourbackpain.com (37 pages, mostly rich-text + image-text)
- psoriaticarthritisinfo.com (21 pages, standard disease-ed)
- latestagensclc.com (8 pages, simple Rx)
- lastacaft.com (8 pages, simple Rx)
- lupronprostatecancer.com (33 pages, standard patient-ed)
- eczemaheadquarters.com (53 pages, modulo social WOL modals)

**Tier 2 — Migrate Second (Medium complexity, needs custom blocks P1/P2):**
- crohnsandcolitis.com (34 pages — needs quick-poll decision)
- nobsabouths.com (38 pages — needs Brightcove embed decision)
- psoriasis.com (77 pages — needs quick-poll decision)
- ra.com (39 pages — needs Brightcove + quick-poll decision)
- venclexta.com (45 pages — already analyzed)
- rinvoqhcp.com (35 pages — already analyzed)
- usdermed.com (31 pages — needs HCP gating decision)

**Tier 3 — Requires Custom Development (Anti-EDS patterns must be resolved first):**
- skyrizilocator.com (2 pages — needs locator replacement strategy)
- amlexpertconnect.com (1 page — needs locator replacement strategy)
- beyondagutfeeling.com (1 page — needs indication-swap strategy)
- emrelis.com (4 pages — needs indication-swap strategy)
- decnupaz.com (6 pages — needs indication-swap strategy)
- savewithays.com (8 pages — needs AEM Forms replacement)

**Tier 4 — Inaccessible / Blocked:**
- durysta.com (HTTP 403)
- allergansavingscard.com (HTTP 403)
- hcp.xengelstent.com (HTTP 0)
