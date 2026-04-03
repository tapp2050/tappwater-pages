# Mobile Audit — pdp-v2.html @ 375px viewport

Target: iPhone SE / iPhone 14 (375px logical width)
Existing breakpoints: `@media (max-width: 900px)`, `@media (max-width: 768px)` (WITB only), `@media (max-width: 540px)`
Date: 2026-04-03

---

## 1. Announcement Bar + Breadcrumb

### Section Name: Announcement Bar (.topnav) + Breadcrumb (.breadcrumb)

**Current issues:**
- `.topnav` has `padding: 10px 20px` and `font-size: 12.5px` — acceptable, but the text string "Free express shipping on Annual Packs · Rated 4.9 stars by 400+ Australians · 60-day money-back guarantee" is very long. At 375px this wraps to 3+ lines, creating a tall announcement bar that pushes content down.
- `.breadcrumb` has `padding: 14px 48px` at desktop. At 900px breakpoint, it changes to `12px 20px` — fine.
- No issues with breadcrumb at 375px. The padding and font sizes are correct after the 900px fix.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .topnav {
    font-size: 11px;
    padding: 8px 16px;
    line-height: 1.45;
  }
}
```

---

## 2. ATF — Hero Image Gallery + Thumbnails

### Section Name: Image Panel (.image-panel, .image-main, .thumbs)

**Current issues:**
- `.atf-outer` has `overflow: hidden` at 900px — good.
- `.image-main` has `aspect-ratio: 1 / 1` — works well on mobile, fills the viewport width nicely.
- `.thumbs` — at 900px breakpoint: `max-width: calc(100vw - 40px)`. The `.thumb` is sized as `flex: 0 0 calc((100% - 32px) / 5)` which works with horizontal scroll. However at 375px the thumbnails are very small (~63px each). With 9 Compact images, the user has to scroll a lot. The thumbnails feel cramped.
- `.thumb` is only ~63px square at 375px — tap target is borderline (Apple recommends 44px minimum, so size is okay, but the visual is tiny).
- No scroll indicator or hint that thumbnails are scrollable horizontally.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  /* Make thumbnails slightly larger for easier tapping */
  .thumb {
    flex: 0 0 calc((100% - 24px) / 4);
  }
  .thumbs {
    gap: 6px;
    padding-bottom: 4px;
  }
  .image-main {
    border-radius: 14px;
  }
}
```

---

## 3. ATF — Gallery Review Card

### Section Name: Gallery Review (.gallery-review)

**Current issues:**
- At 900px: padding reduced to `14px 16px`, font reduced to `12.5px` — good.
- `.gr-meta` items (`gr-location`, `gr-verified`) are 11.5px — acceptable.
- The `.gr-top` row with author name + stars could wrap awkwardly if name is long, but existing names are short.
- No significant issues. The card is well-handled.

**Recommended fixes:**
```css
/* Minor tightening for 375px — no critical issues */
@media (max-width: 540px) {
  .gallery-review {
    padding: 12px 14px;
    gap: 10px;
    margin-top: 14px;
  }
  .gr-quote {
    font-size: 12px;
    line-height: 1.45;
  }
  .gr-meta {
    font-size: 11px;
  }
}
```

---

## 4. ATF — Buy Box

### Section Name: Buy Box (.buy-box — product name, tagline, claim strip, model selector, pack selector, ATC, payment icons, trust row)

**Current issues:**

**Product Name:**
- At 900px: `font-size: 28px; white-space: normal;` — good fix.
- "EcoPro Compact" fits at 28px but "EcoPro SMR(tm)" with the TM character could be tight.

**Claim Strip:**
- At 900px: `flex-wrap: wrap; gap: 6px` — good.
- At 375px, claims wrap to 2 rows. Each claim still has `white-space: normal` and `font-size: 10.5px` at 900px. Four claims in a row won't fit at 375px — they wrap to 2x2, which is fine.
- The claim pills are a bit tight with `padding: 4px 8px`.

**Model Selector:**
- `.model-selector` is a 2-column grid with `gap: 10px`. At 375px, each card is ~162px wide. The `.mc-name` at 13px (900px fix) and `.mc-detail` at 10.5px are fine.
- The `.mc-check` radio indicator at `top: 12px; right: 12px` overlaps with the model name text at narrow widths when the name is long. "EcoPro Compact" is 14 chars at 13px, leaving limited space.
- `.mc-badge` text like "+ Swedish Mineral Rock(tm)" may overflow on the SMR card at 162px wide.

**Pack Selector:**
- `.pack-opt` with `padding: 13px 16px` — the row is `display: flex` with radio (22px), info (flex:1), and pricing (flex-shrink:0). At 375px the pack detail text wraps heavily.
- `.pack-name` at 15px with the `Save $80` badge inline — this row can overflow at 375px. The badge is an inline `<span>` with font-size 11px which helps.
- `.pack-price` at 20px and `.pack-was` at 13px — fine.
- `.pack-float-badge` ("Best value") positioned at `top: -12px; right: 16px` — may clip against the edge.
- `.pack-daily` text "55c a day for clean water" at 11.5px — fine.

**ATC Button:**
- `padding: 18px` with `font-size: 16px` — good touch target. No issues.

**Payment Icons:**
- At 900px: `flex-wrap: wrap` and `svg { width: 32px; height: 20px; }` — 7 icons at 32px + 6px gap = 266px. Fits in one row at 375px (with some margin). Actually: 7*32 + 6*6 = 260px. With center alignment, this fits.

**Trust Row:**
- At 900px: `flex-wrap: wrap; flex: 0 0 50%; border-right: none; margin-bottom: 10px` — wraps to 2x2 grid. 4 items at 50% each = 2 rows. Good.
- Trust items at `font-size: 10.5px` with the SVG icon — acceptable but text might be cramped.
- No bottom border separator visible in 2x2 layout.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  /* Model selector — prevent mc-badge overflow */
  .model-card {
    padding: 10px 12px;
  }
  .model-card .mc-check {
    top: 8px;
    right: 8px;
    width: 18px;
    height: 18px;
  }
  .model-card .mc-badge {
    font-size: 9px;
    padding: 2px 7px;
    max-width: 100%;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    display: block;
  }

  /* Pack selector — tighten padding */
  .pack-opt {
    padding: 11px 12px;
    gap: 10px;
  }
  .pack-name {
    font-size: 14px;
  }
  .pack-detail {
    font-size: 12px;
  }
  .pack-price {
    font-size: 18px;
  }
  .pack-was {
    font-size: 12px;
  }
  .pack-float-badge {
    right: 12px;
    font-size: 10px;
    padding: 3px 10px;
  }

  /* ATC button — increase touch target padding slightly */
  .btn-atc {
    font-size: 15px;
    padding: 16px;
  }

  /* Trust row — smaller icons, tighter text */
  .trust-item {
    font-size: 10px;
    padding: 0 4px;
  }
  .ti-svg {
    width: 18px;
    height: 18px;
    margin-bottom: 4px;
  }

  /* Product name */
  .product-name {
    font-size: 26px;
    letter-spacing: -0.8px;
  }
}
```

---

## 5. As Seen In Media Bar

### Section Name: As Seen In (.as-seen-in, .asi-inner)

**Current issues:**
- At 900px: `flex-direction: column; gap: 10px;` and logos `gap: 18px; height: 14px` — good.
- At 375px: 7 logos at 14px height with 18px gap wrapping. That is 7 logos at ~100px max-width. Some logos are wide (e.g., "Better Homes & Gardens"). The flex-wrap on `.asi-logos` means they stack to 2+ rows. This could look messy.
- The label "AS SEEN IN" at 10px is centered above the logos. With column direction it's fine.
- Horizontal padding is 20px at 900px — leaves 335px for logos. 7 logos won't fit in one row.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .asi-logos {
    gap: 14px 20px;
  }
  .asi-logo {
    height: 12px;
    max-width: 80px;
  }
  .as-seen-in {
    padding: 16px 0;
  }
}
```

---

## 6. Why It Matters (Benefit Cards)

### Section Name: Benefits Grid (.btf-benefits, .benefits-grid)

**Current issues:**
- At 900px: `grid-template-columns: 1fr 1fr` and padding `60px 20px`.
- At 540px: `grid-template-columns: 1fr` — single column. Good.
- `.bc-featured` at 900px: `grid-template-columns: 1fr; gap: 16px;` and stat font `48px` — good.
- At 375px with 1-column: each benefit card has `padding: 28px 24px` — that is 48px horizontal padding out of 375px - 40px (outer padding) = 335px content width. Card content width = 335px - 48px = 287px. This is fine.
- 9 benefit cards in single column is a LOT of scrolling. Consider whether this is acceptable UX.
- `.bc-headline` at 16px and `.bc-body` at 13.5px — good sizes for mobile.
- `.section-heading` at 900px is `26px` — acceptable.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .benefit-card {
    padding: 22px 20px;
  }
  .bc-badge {
    width: 40px;
    height: 40px;
    border-radius: 12px;
    font-size: 20px;
    margin-bottom: 12px;
  }
  .bc-headline {
    font-size: 15px;
    margin-bottom: 6px;
  }
  .bc-body {
    font-size: 13px;
  }
  .bc-featured-stat {
    font-size: 40px;
    letter-spacing: -2px;
  }
  .benefit-card.bc-featured {
    padding: 24px 20px;
  }
}
```

---

## 7. Independently Tested (Lab Stats)

### Section Name: Lab Section (.btf-lab, .lab-stats)

**Current issues:**
- At 900px: `.lab-stats { grid-template-columns: 1fr 1fr; }` — 2 columns. At 375px with 20px side padding, that gives ~157px per card. `padding: 24px 20px` on `.lab-stat` leaves 117px for content.
- `.ls-num` at 36px font — "98/100" text at 36px is ~180px wide, which overflows the 117px content area. This is a real overflow issue.
- `.ls-contaminant` at 14px — "Microplastics removed" at 14px wraps fine.
- `.ls-source` at 11px — fine.
- No `@media (max-width: 540px)` rule for lab stats to go single column.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .lab-stats {
    grid-template-columns: 1fr;
  }
  .lab-stat {
    padding: 20px 18px;
    display: flex;
    align-items: baseline;
    gap: 16px;
    flex-wrap: wrap;
  }
  .ls-num {
    font-size: 28px;
  }
  .btf-lab {
    padding: 60px 20px;
  }
}
```

---

## 8. Cost Calculator (Slider + Result Cards)

### Section Name: Savings Calculator (.btf-calc)

**Current issues:**
- `.btf-calc` has `padding: 80px 48px`. At 900px breakpoint it gets `padding-left: 20px; padding-right: 20px` — but wait, this section is NOT in the 900px rules. Let me check... The 900px rule only sets `.btf-compare, .btf-stages, .btf-reviews, .btf-faq, .btf-guarantee { padding-left: 20px; padding-right: 20px; }` and `.btf-lab`. **`.btf-calc` is missing from the 900px horizontal padding reduction!** So at 375px, the calc section still has `padding: 80px 48px` — content width is only 375 - 96 = 279px. This is a significant issue.
- `.calc-heading` at 36px — "You're paying more than you think." at 36px will overflow in 279px width.
- `.calc-result-grid` at 900px: `grid-template-columns: 1fr 1fr` — 2x2. At 279px content width: each card is ~131px. `.calc-card-num` at 36px (or 40px for highlight) will overflow.
- `.calc-slider` at `width: 100%` — fine but the thumb at 22px needs enough space.
- `.calc-cta` button is a block CTA, should be fine width-wise.
- `.calc-footnote` at 12px — fine.
- `calc-slider-value` at 22px font — fine.

**Recommended fixes:**
```css
@media (max-width: 900px) {
  .btf-calc {
    padding: 60px 20px;
  }
}

@media (max-width: 540px) {
  .calc-heading {
    font-size: 26px;
    letter-spacing: -0.8px;
  }
  .calc-result-grid {
    grid-template-columns: 1fr 1fr;
    gap: 10px;
  }
  .calc-card {
    padding: 20px 14px;
  }
  .calc-card-num {
    font-size: 28px;
  }
  .calc-card.highlight .calc-card-num {
    font-size: 30px;
  }
  .calc-card.impact .calc-card-num {
    font-size: 28px;
  }
  .calc-card-label {
    font-size: 10px;
  }
  .calc-cta {
    font-size: 14px;
    padding: 14px 24px;
    width: 100%;
  }
  .calc-sub {
    font-size: 14px;
  }
  .calc-slider-value {
    font-size: 18px;
  }
}
```

---

## 9. EcoPro vs Jug Filters (Comparison Grid)

### Section Name: Vs Jug Section (.btf-vs, .vs-grid)

**Current issues:**
- At 900px: `.vs-grid { grid-template-columns: 1fr 1fr; }` and `.btf-vs { padding: 60px 20px; }` — good.
- At 375px: 2-column grid with 20px side padding = ~157px per card. `.vs-card` has `padding: 24px 22px` = 44px horizontal, leaving 113px for content.
- `.vs-card-us` at 24px (or 32px on hero row) — ">99%" at 32px is ~100px. Fits, but tight.
- `.vs-card-label` at 13px — "Fluoride reduction" at 13px wraps within 113px. Fine.
- `.vs-card-them` at 12.5px — fine.
- `.vs-bottom-line` at 18px with `padding: 28px 36px` — at 375px - 40px = 335px, with 72px padding = 263px for text. "Same money. Wildly different water." at 18px fits fine.
- No 540px breakpoint to go single column — 2 columns at 375px is cramped.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .vs-grid {
    grid-template-columns: 1fr;
  }
  .vs-card {
    padding: 20px 18px;
  }
  .vs-card-us {
    font-size: 22px;
  }
  .vs-grid.vs-hero-row .vs-card-us {
    font-size: 28px;
  }
  .vs-bottom-line {
    font-size: 16px;
    padding: 22px 20px;
  }
}
```

---

## 10. Your First 2 Months (Timeline)

### Section Name: Journey Timeline (.btf-journey, .journey-cards)

**Current issues:**
- At 900px: `.journey-cards { grid-template-columns: 1fr 1fr; gap: 28px; }` and padding `60px 20px`. `journey-track::before { display: none; }`.
- At 540px: `.journey-cards { grid-template-columns: 1fr; gap: 24px; }` — single column. Good.
- `.jcard-dot` at 40px with box-shadow ring (4px + 6px) = 52px effective. In single column this is fine.
- `.jcard-headline` at 15px and `.jcard-body` at 13px — good.
- `.journey-header` at 900px: `flex-direction: column; align-items: flex-start;` — good.
- `.journey-model-badge` has `white-space: nowrap` — "EcoPro Compact · 2-month cartridge" could overflow at 375px - 40px = 335px width. At 12px font this is approximately 290px. Fine.
- 5 cards single column is a lot of scrolling but appropriate for a timeline.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .jcard-dot {
    width: 34px;
    height: 34px;
    font-size: 12px;
  }
  .jcard-headline {
    font-size: 14px;
  }
  .jcard-body {
    font-size: 12.5px;
  }
  .journey-model-badge {
    font-size: 11px;
    padding: 4px 12px;
  }
  .section-heading br {
    display: none;
  }
}
```

---

## 11. What We Remove (Contaminant Cards + Accordion)

### Section Name: Removes Section (.btf-removes, .removes-hero, .removes-cats)

**Current issues:**
- At 900px: `.removes-hero { grid-template-columns: 1fr 1fr; }` and `.removes-cats { grid-template-columns: 1fr; }`. Padding `20px`.
- `.removes-hero` in 2 columns at 375px: each card ~157px wide. `.rh-stat` has `padding: 24px`. Content width = 157 - 48 = 109px. `.rhs-num` at 32px — ">99%" is ~95px wide. Fits, barely.
- No 540px breakpoint for `.removes-hero` — should go single column.
- `.fluoride-note` has `padding: 12px 16px` at 13px — fine.
- `.rcat` accordion in single column — good. `.rcat-header` has `padding: 18px 20px` — fine.
- `.rcat-row` has `padding: 10px 18px` — at 335px content, the contaminant name + removal value fit, but long names like "Microbial Cysts (Giardia, Cryptosporidium)" at 13px will be very tight. The `.rv` value has `flex-shrink: 0; margin-left: 12px` which helps.
- `.removes-footer` — the `<span>` at 12px with the long lab citation text will wrap. `flex-wrap: wrap` is set. Fine.
- `.btf-removes` padding not explicitly reduced at 900px — but it IS in the rule: `.btf-removes { padding-left: 20px; padding-right: 20px; }` at 900px. Good.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .removes-hero {
    grid-template-columns: 1fr;
    gap: 12px;
  }
  .rh-stat {
    padding: 18px 20px;
    display: flex;
    flex-wrap: wrap;
    gap: 8px 16px;
    align-items: baseline;
  }
  .rhs-bar-wrap {
    width: 100%;
    margin-bottom: 10px;
  }
  .rhs-num {
    font-size: 26px;
  }
  .rcat-row {
    padding: 9px 14px;
    font-size: 12.5px;
  }
  .rcat-header {
    padding: 14px 16px;
  }
  .fluoride-note {
    font-size: 12px;
    padding: 10px 14px;
  }
}
```

---

## 12. How It Works — Six Stages (Stage Cards Grid)

### Section Name: Stages Grid (.btf-stages, .stages-grid)

**Current issues:**
- At 900px: `.stages-grid { grid-template-columns: 1fr 1fr; }` — 2 columns. At 375px - 40px padding = 335px. Each card ~159px.
- `.stage-card` has `padding: 24px 20px` — content = 119px.
- `.stage-name` at 15px and `.stage-desc` at 13px — text wraps fine within 119px.
- `.stage-num-tag` at 10.5px with `padding: 3px 10px` — fine.
- `btf-stages` padding reduced to 20px at 900px — confirmed in the grouped rule.
- 2 columns for 6 cards at 375px is acceptable but a bit tight. Single column would be cleaner.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .stages-grid {
    grid-template-columns: 1fr;
    gap: 12px;
  }
  .stage-card {
    padding: 20px 18px;
  }
  .stage-name {
    font-size: 14px;
  }
  .stage-desc {
    font-size: 12.5px;
  }
}
```

---

## 13. Customer Reviews

### Section Name: Reviews Section (.btf-reviews, .review-meta, .review-grid)

**Current issues:**
- At 900px: `.review-grid { grid-template-columns: 1fr; }` — single column. Good.
- `.review-meta` at 900px: `flex-direction: column; gap: 24px; align-items: flex-start;` — good.
- `.rm-num` at 64px — "4.9" is only 2 chars wide, fits fine.
- `.rm-bars` has `max-width: 280px` — at 335px content, this fits.
- `.rm-highlights` at 900px: `grid-template-columns: 1fr 1fr` — 2 highlights per row at ~157px each. `padding: 14px 16px` = 32px. Content = 125px. Text at 13px wraps. Fine.
- `.review-card` in single column — good. `padding: 22px 20px` is comfortable.
- `.rc-top` has `display: flex; justify-content: space-between` — stars + product tag. `.rc-product-tag` has `white-space: nowrap` — "Compact Annual" at 10px is ~90px. Stars at 13px are ~65px. Total ~155px + gap. Fits in 295px (335 - 40 padding). Fine.
- 6 review cards in single column is a long scroll.
- `btf-reviews` padding reduced to 20px at 900px — confirmed.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .rm-highlights {
    grid-template-columns: 1fr;
  }
  .rm-num {
    font-size: 48px;
  }
  .rm-stars {
    font-size: 16px;
  }
  .review-card {
    padding: 18px 16px;
  }
  .rc-text {
    font-size: 13px;
  }
  .btf-reviews {
    padding-top: 60px;
    padding-bottom: 60px;
  }
}
```

---

## 14. Our Promise / Guarantee

### Section Name: Guarantee (.btf-guarantee, .guarantee-panels, .guarantee-trust)

**Current issues:**
- At 900px: `.guarantee-panels { grid-template-columns: 1fr; }` and `.guarantee-trust { grid-template-columns: 1fr 1fr; }`. Padding `60px 20px`.
- `.gp-card` has `padding: 44px 40px` — at 335px content (375 - 40px), that is 80px horizontal padding, leaving only 255px for content. **Too much padding.**
- `.gp-numeral` at 60px — "60" is ~60px wide. Fine.
- `.gp-title` at 22px — "Try it. Actually try it." is ~260px at 22px. Fits in 255px barely, might wrap.
- `.gp-body` at 14px with line-height 1.85 — long paragraphs in 255px width. Fine for wrapping but the padding is excessive.
- `.guarantee-trust` in 2 columns at 375px: each `.gt-item` is ~157px. With `padding: 16px 18px` (36px) and `gap: 12px` for icon+text, the `.gt-text` at 13px in ~90px width will wrap heavily. "Free express shipping" wraps to 3 lines.
- `.gp-pill` at 12px with `padding: 8px 16px` — fine.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .gp-card {
    padding: 28px 22px;
    border-radius: 18px;
  }
  .gp-numeral {
    font-size: 48px;
  }
  .gp-title {
    font-size: 20px;
    margin-bottom: 12px;
  }
  .gp-body {
    font-size: 13.5px;
    line-height: 1.7;
  }
  .gp-shield {
    width: 48px;
    height: 48px;
    font-size: 24px;
    border-radius: 14px;
  }
  .guarantee-trust {
    grid-template-columns: 1fr;
    gap: 8px;
  }
  .gt-item {
    padding: 14px 16px;
  }
}
```

---

## 15. Tap Compatibility (95% Stat, Tap Grid, Fits/Won't Fit)

### Section Name: Tap Compatibility (.btf-compat, .tap-grid, .compat-how)

**Current issues:**
- At 900px: `.tap-grid { grid-template-columns: repeat(3, 1fr); }` — 3 columns. At 375px - 40px = 335px: each card ~105px wide. `.tap-img { height: 200px; }` — 200px tall images in 105px-wide cards means extreme letterboxing/cropping. `.tap-name` at 13px and `.tap-desc` at 11px are fine but 105px width is too narrow.
- **This is a major layout problem.** 3 columns of tap cards at 375px is way too cramped.
- `.compat-how` at 900px: `grid-template-columns: 1fr;` — single column with arrows rotated. Good.
- `.chs-num` at 56px — "95%" is fine in single column.
- `.wont-fit-strip` at 900px: `flex-direction: column; align-items: flex-start; gap: 14px;` — good.
- `.compat-cta` at 900px: `flex-direction: column; align-items: flex-start;` — good.
- The inline `style="grid-template-columns:repeat(3,1fr)"` on the tap grids overrides any media query changes to `.tap-grid`. **This is the most critical bug** — inline styles cannot be overridden by media queries without `!important`.
- `.tap-img` at 200px fixed height in a 105px-wide column creates terrible aspect ratios.
- `.tap-desc` text blocks at 11px in 105px - 24px padding = ~81px content width — extremely narrow, text wraps to many lines.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  /* Must use !important to override inline grid-template-columns */
  .tap-grid {
    grid-template-columns: 1fr 1fr !important;
    gap: 10px;
  }
  .tap-img {
    height: 140px;
  }
  .tap-name {
    font-size: 12px;
    padding: 0 10px 3px;
  }
  .tap-desc {
    font-size: 10.5px;
    padding: 3px 10px 12px;
    line-height: 1.5;
  }
  .tap-status {
    font-size: 9px;
    padding: 6px 10px;
  }
  .chs-num {
    font-size: 44px;
  }

  /* Compat how — step content */
  .ch-step {
    padding: 22px 18px;
  }
  .ch-step-title {
    font-size: 14px;
  }
  .ch-step-body {
    font-size: 12.5px;
  }
  .ch-thread-split {
    gap: 6px;
  }
  .ch-thread-opt {
    padding: 10px 8px;
  }
  .ch-thread-opt-label {
    font-size: 10px;
  }
  .ch-icon svg {
    width: 40px;
    height: 40px;
  }

  /* Compat CTA */
  .compat-cta {
    padding: 20px 18px;
    border-radius: 12px;
  }
  .compat-cta-text {
    font-size: 13px;
  }
  .compat-cta-btn {
    font-size: 13px;
    padding: 12px 20px;
    width: 100%;
    text-align: center;
    justify-content: center;
  }
}
```

---

## 16. Size & Specs (Product Detail Table)

### Section Name: Size Comparison (.btf-size, .size-compare)

**Current issues:**
- At 900px: `.size-compare { grid-template-columns: 1fr; }` — single column. Good.
- `.size-card` in single column — full width. `padding: 0` with visual and text sections.
- `.size-visual` at `height: 240px` — fine.
- `.size-text` has `padding: 20px 28px 24px` — 56px horizontal out of 335px = 279px content. Fine.
- `.size-detail-row` with label + value at 13px — "Your existing tap aerator — nothing else touches the counter" at 13px wraps in 279px. Fine.
- `.btf-size` has `padding: 80px 48px` at desktop. **Not in the 900px padding reduction rule!** At 375px, padding is still `80px 48px`. Content width = 375 - 96 = 279px. The install grid images will be very small.
- `.install-grid` at 900px: `grid-template-columns: repeat(3, 1fr)` — 3 columns at 279px = ~85px each. Images at 1:1 aspect ratio = 85px squares. Very small.

**Recommended fixes:**
```css
@media (max-width: 900px) {
  .btf-size {
    padding: 60px 20px;
  }
}

@media (max-width: 540px) {
  .size-text {
    padding: 16px 20px 20px;
  }
  .size-visual {
    height: 200px;
  }
  .size-name {
    font-size: 16px;
  }
  .size-detail-row {
    font-size: 12.5px;
    padding: 8px 0;
  }
  .sdr-val {
    font-size: 12px;
  }
  .install-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 8px;
  }
  .install-grid-label {
    font-size: 13px;
  }
}
```

---

## 17. Install Grid (Customer Photos)

### Section Name: Install Grid (.install-grid)

**Current issues:**
- Covered in Section 16 above. The install grid is inside `.btf-size`.
- At 900px: 3 columns. At 375px with 48px side padding (bug — should be 20px): only 279px / 3 = ~85px per image.
- 8 images in a 3-col grid = 3 rows. At 85px each, the images are thumbnails. Usable but very small.
- With the padding fix from Section 16 (20px sides), width = 335px / 3 = ~105px each. Better, but still small.

**Recommended fixes:**
```css
/* Already covered in Section 16 — install-grid goes to 2 columns at 540px */
@media (max-width: 540px) {
  .install-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 8px;
  }
  .install-grid img {
    border-radius: 10px;
  }
}
```

---

## 18. Split Video Sections (Install, Replace, Diverter, Flow Rate)

### Section Name: Split Sections (.split-wrapper, .split-section)

**Current issues:**
- At 900px: `.split-section { grid-template-columns: 1fr; }` and `.split-video { min-height: 280px; }` and `.split-content { padding: 48px 24px; }` and `.split-wrapper { padding: 40px 20px; gap: 16px; }`. Good base mobile treatment.
- At 375px: video at 280px height in full width — fine.
- `.split-heading` at 24px with `<br>` tags — "Up and running<br>in 60 seconds." wraps differently on mobile. The `<br>` is redundant. At 375px - 48px padding = 327px at 24px font — "Up and running" fits, then "in 60 seconds." on next line. The `<br>` tag causes an empty line visually in some cases.
- `.split-sub` at 13px with `max-width: 380px` — at 327px content width, max-width has no effect. Fine.
- `.split-steps` with step numbers, titles, and descriptions — `.ss-num` at 28px circle, `.ss-title` at 15px, `.ss-desc` at 13px. All fine at 327px width.
- `.split-note` at 12px — fine.
- 4 split sections stacked vertically is a very long scroll. Each is ~600px+ (280 video + 300+ content). Total ~2400px of split sections.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .split-video {
    min-height: 240px;
  }
  .split-content {
    padding: 32px 20px;
  }
  .split-heading {
    font-size: 22px;
  }
  .split-sub {
    font-size: 12.5px;
    max-width: none;
  }
  .ss-title {
    font-size: 14px;
  }
  .ss-desc {
    font-size: 12.5px;
  }
  .split-note {
    font-size: 11.5px;
    padding: 8px 12px;
  }
}
```

---

## 19. Product Comparison (Compact vs SMR Cards + Comparison Table)

### Section Name: Compare Section (.btf-compare, .compare-cards, .ctable, .decision-helper)

**Current issues:**

**Compare Cards:**
- At 900px: `.compare-cards { grid-template-columns: 1fr; }` — single column. Good.
- `.compare-card` has `padding: 28px 24px` — at 335px content: 287px for text. Fine.
- `.cc-img-wrap` has negative margins `margin: -28px -24px 20px` — pulls image to card edges. Good.
- `.cc-bottom` flex row with name + price — "EcoPro Compact" at 18px + "$109.99" at 18px. Total ~290px. Fits in 287px barely. May need wrapping.

**Comparison Table:**
- `.ctable` at `width: 100%` and `font-size: 14px`. 3 columns (feature, Compact, SMR).
- `.ctable thead th` and `td` have `padding: 16px 20px` / `padding: 15px 20px`.
- At 335px total: 3 columns with 20px padding each side = 120px of padding. Content splits are roughly 1/3 each = ~71px per cell content area. This is VERY tight.
- Cell text like "✓ Swedish Mineral Rock(tm)" at 14px + 700 weight will overflow its cell.
- `td:first-child` text like "Minerals added (Ca, Mg, K)" at 13px in ~71px wraps to 3+ lines.
- The table will likely cause horizontal overflow at 375px.
- **`.btf-compare` padding**: at 900px it IS reduced to 20px — confirmed.

**Decision Helper:**
- At 900px: `.decision-helper { grid-template-columns: 1fr; }` — good.
- `.dh-card` has `padding: 36px 32px 32px` — 64px horizontal. At 335px content = 271px. Fine.
- `.dh-list li` at 14px with `padding-left: 28px` — fine.
- `.dh-btn` at 15px full-width — fine.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  /* Comparison table — critical overflow fix */
  .ctable {
    font-size: 12px;
  }
  .ctable thead th {
    padding: 12px 8px;
    font-size: 11px;
  }
  .ctable tbody td {
    padding: 10px 8px;
    font-size: 12px;
  }
  .ctable tbody td:first-child {
    font-size: 11px;
    min-width: 100px;
  }

  /* Compare cards bottom row */
  .cc-bottom {
    flex-direction: column;
    gap: 6px;
  }
  .cc-bottom-right {
    text-align: left;
  }
  .cc-name {
    font-size: 16px;
  }
  .cc-price {
    font-size: 16px;
  }

  /* Decision helper — tighten padding */
  .dh-card {
    padding: 28px 22px 24px;
  }
  .dh-title {
    font-size: 17px;
  }
  .dh-sub {
    font-size: 13px;
  }
  .dh-list li {
    font-size: 13px;
  }
  .dh-btn {
    font-size: 14px;
    padding: 14px 24px;
  }
}
```

---

## 20. RO Upsell Section

### Section Name: RO Card (.ro-card)

**Current issues:**
- At 900px: `.ro-card { grid-template-columns: 1fr; }`, `.ro-img-side { height: 300px; order: -1; }`, `.ro-content-side { padding: 32px 24px 36px; }`, `.ro-stats-row { grid-template-columns: repeat(2, 1fr); }`, etc. Good base.
- At 375px with 20px side padding: `.ro-content-side` is 335px - 48px padding = 287px content.
- `.ro-headline` at 24px — "For those who want absolutely everything removed." wraps fine at 287px.
- `.ro-stats-row` in 2 columns at 287px: each stat ~135px. With `padding: 16px 8px` and text at 20px/10.5px — fine.
- `.ro-features` at 900px: `grid-template-columns: 1fr` — single column. Good.
- `.ro-bottom` at 900px: `flex-direction: column; align-items: flex-start; gap: 16px;` and `.ro-cta { width: 100%; justify-content: center; }`. Good.
- `.ro-img-side` at 300px height is large for mobile — eats 300px of viewport.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .ro-img-side {
    height: 220px;
  }
  .ro-content-side {
    padding: 24px 20px 28px;
  }
  .ro-headline {
    font-size: 20px;
  }
  .ro-desc {
    font-size: 13.5px;
  }
  .ro-stats-row {
    grid-template-columns: repeat(3, 1fr);
  }
  .ro-stat {
    padding: 12px 6px;
  }
  .ro-stat-num {
    font-size: 18px;
  }
  .ro-stat-label {
    font-size: 10px;
  }
}
```

---

## 21. FAQ Accordion

### Section Name: FAQ (.btf-faq)

**Current issues:**
- `.btf-faq` has `padding: 80px 48px; max-width: 760px; margin: 0 auto;` — at 375px, the `max-width` has no effect (375 < 760), but `padding: 80px 48px` does apply. Content width = 375 - 96 = 279px. **This is another section missing from the 900px padding reduction.**
- Looking again at the 900px rule: `.btf-compare, .btf-stages, .btf-reviews, .btf-faq, .btf-guarantee { padding-left: 20px; padding-right: 20px; }` — **FAQ IS included!** So padding goes to 20px. Content = 335px. Good.
- `.faq-q` at 15px with 335px width minus 24px icon = 311px for question text. Fine.
- `.faq-a` at 14px with line-height 1.75 — fine.
- `.faq-icon` at 24px circle — good touch target.
- The FAQ section has its top padding at 80px still — could be tightened.
- The hot water question has an inline style `style="color:var(--amber);"` — fine.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .btf-faq {
    padding-top: 60px;
    padding-bottom: 60px;
  }
  .faq-q {
    font-size: 14px;
    padding: 16px 0;
    gap: 12px;
  }
  .faq-a {
    font-size: 13px;
    line-height: 1.65;
  }
}
```

---

## 22. Get in Touch

### Section Name: Contact (.btf-contact)

**Current issues:**
- At 540px: `.btf-contact { padding: 48px 20px; }`, `.contact-actions { flex-direction: column; }`, `.contact-btn { justify-content: center; }`. Good.
- `.contact-heading` at 24px — "Still not sure?" fits easily.
- `.contact-sub` at 15px — fine.
- `.contact-btn` at 14px with `padding: 12px 24px` — good touch target at 48px height. In column layout, each is full-width.
- `.contact-note` at 12px — fine.
- No issues found.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .contact-heading {
    font-size: 22px;
  }
  .contact-sub {
    font-size: 14px;
  }
  .contact-btn {
    padding: 14px 20px;
    font-size: 14px;
  }
}
```

---

## 23. Sticky ATC Nav Bar

### Section Name: Sticky ATC (.sticky-atc)

**Current issues:**
- At 900px: padding `8px 16px; gap: 10px`, font sizes reduced to 13px/11px, price 16px, button `8px 14px` at 12px. Good start.
- At 375px: the sticky bar is `display: flex; justify-content: space-between`. Left side has product info (name + pack) + stars row in a flex column within a flex row. Right side has price + button.
- The left side inner layout: `display:flex; align-items:center; gap:20px;` (inline style on the wrapper div). Product info + stars side by side. At 375px - 32px padding = 343px. Stars at ~130px, product info at ~130px, gap 20px = 280px for left. Right side: price 16px "199.99" ~70px + button "Add to Cart" 12px at 72px padding = ~142px. But gap between left and right is 10px. Total: 280 + 10 + 142 = 432px. **This overflows 343px!**
- The sticky bar will overflow horizontally or the product info will wrap awkwardly.
- Stars row should be hidden on mobile to save space.
- Button text "Add to Cart" could be shortened to "Add" or the stars hidden.

**Recommended fixes:**
```css
@media (max-width: 540px) {
  .sticky-atc-inner {
    padding: 8px 12px;
    gap: 8px;
  }
  /* Hide stars in sticky bar on narrow mobile */
  .sticky-atc-inner .sticky-stars {
    display: none;
  }
  /* Remove the gap between product info and stars (inline style override) */
  .sticky-atc-inner > div:first-child {
    gap: 0 !important;
  }
  .sticky-product-name {
    font-size: 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: 140px;
  }
  .sticky-product-pack {
    font-size: 10px;
  }
  .sticky-price {
    font-size: 15px;
  }
  .sticky-btn {
    padding: 8px 12px;
    font-size: 12px;
    border-radius: 8px;
  }
  .sticky-right {
    gap: 6px;
    flex-shrink: 0;
  }
}
```

---

## Global / Cross-Section Issues

### Section Name: Global Mobile Issues

**Current issues:**
- **Missing padding reduction for `.btf-calc` and `.btf-size` at 900px** — these sections still use `padding: 80px 48px` on mobile. This is the most impactful layout bug across the page.
- **Inline `grid-template-columns` on tap grids** — cannot be overridden by media queries without `!important`. Three instances in the HTML: the "fits", "contact", and "won't fit" grids all use `style="grid-template-columns:repeat(3,1fr)"`.
- **Section heading `<br>` tags** — several `section-heading` and `split-heading` elements use `<br>` for desktop line breaks. On mobile these create awkward breaks mid-sentence, e.g., "Filtered water changes<br>more than the taste." reads fine, but "You're paying more than<br>you think." creates a one-word second line.
- **Total page length on mobile** — the page has 23+ sections with many in single column. The scroll depth at 375px is likely 25,000-30,000px. This is very long. Consider lazy-loading below-fold sections or collapsing some into tabs.
- **No horizontal overflow protection on body** — the 900px rule sets `html, body { overflow-x: hidden; }` which is a band-aid. The real overflow sources should be fixed.
- The `white-space: nowrap` on `.product-name` is removed at 900px (set to `normal`) — good.
- **`calc-heading` and `section-heading`** use `<br>` tags that should be hidden on mobile via `display: none` or replaced with CSS.

**Recommended fixes:**
```css
/* Critical: Add missing sections to 900px padding rule */
@media (max-width: 900px) {
  .btf-calc {
    padding: 60px 20px;
  }
  .btf-size {
    padding: 60px 20px;
  }
}

/* Handle <br> tags in headings on mobile */
@media (max-width: 540px) {
  .section-heading br,
  .calc-heading br,
  .split-heading br {
    display: none;
  }

  /* Tighten vertical section spacing globally */
  .btf-benefits,
  .btf-lab,
  .btf-stages,
  .btf-reviews,
  .btf-removes,
  .btf-guarantee,
  .btf-vs,
  .btf-journey,
  .btf-compat,
  .btf-size {
    padding-top: 48px;
    padding-bottom: 48px;
  }

  /* Section headings */
  .section-heading {
    font-size: 24px;
    letter-spacing: -0.6px;
  }
  .section-sub {
    font-size: 14px;
    margin-bottom: 32px;
  }
  .section-eyebrow {
    font-size: 11px;
  }
}
```

---

## Priority Summary

### Critical (causes overflow or broken layout):
1. **`.btf-calc` missing from 900px padding rule** — 48px side padding at 375px
2. **`.btf-size` missing from 900px padding rule** — 48px side padding at 375px
3. **Tap grid inline `grid-template-columns`** — 3-col at 375px is broken, needs `!important` override
4. **Sticky ATC bar horizontal overflow** — stars + product info + price + button don't fit at 375px
5. **Comparison table (`.ctable`) cell overflow** — 3 columns too wide at 375px

### High (poor usability or visual problems):
6. Lab stats `.ls-num` "98/100" overflows in 2-column layout at 375px
7. Guarantee card `.gp-card` padding too generous (44px 40px) for mobile
8. `.removes-hero` 2-column at 375px is too cramped — should go 1-column at 540px
9. Vs Jug grid 2-column at 375px is cramped — should go 1-column at 540px
10. Stages grid 2-column at 375px is cramped — should go 1-column at 540px

### Medium (polish and spacing):
11. Announcement bar text wrapping to 3+ lines
12. Model card badge text overflow on narrow cards
13. Install grid images too small in 3-column at narrow widths
14. Section `<br>` tags creating bad line breaks on mobile
15. Excessive vertical padding (80px) on many sections at mobile
16. Review highlights 2-column at 375px — single column cleaner
