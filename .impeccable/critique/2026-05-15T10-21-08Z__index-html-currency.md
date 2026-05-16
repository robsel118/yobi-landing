---
target: currency
total_score: 22
p0_count: 0
p1_count: 1
timestamp: 2026-05-15T10-21-08Z
slug: index-html-currency
---
## Design Health Score

| # | Heuristic | Score | Key Issue |
|---|-----------|-------|-----------|
| 1 | Visibility of System Status | 2 | "Updated 4 min ago" implies live data; the section is entirely static — copy creates false expectations |
| 2 | Match System / Real World | 2 | "Combined runway · all currencies" labels a dollar amount ($8,906.42) as "runway," which is a days-of-coverage concept everywhere else on the page |
| 3 | User Control and Freedom | 1 | No interaction at all — expected for marketing, but there is no way to explore or toggle the base currency |
| 4 | Consistency and Standards | 2 | FX badge shows three semantically different things: "Base currency" (status label), "1 EUR = 1.08 USD" (actual rate), "Live FX rate" (category description) |
| 5 | Error Prevention | 3 | Static section — no user input, no error paths. The "Combined runway" label may create an incorrect mental model but causes no functional error |
| 6 | Recognition Rather Than Recall | 3 | Flag emoji + currency code pairing is immediately recognizable. Nothing is hidden. |
| 7 | Flexibility and Efficiency | 1 | Purely static. No base-currency toggle, no wallet selector, no shortcut of any kind. |
| 8 | Aesthetic and Minimalist Design | 3 | Clean. White cards feel appropriate for wallet UI. The FX badge inconsistency introduces minor visual noise. |
| 9 | Help Users Recognize/Recover from Errors | 3 | No user actions, no errors. N/A for this section. |
| 10 | Help and Documentation | 2 | "Runway" is used without definition. THB's badge says "Live FX rate" without showing the rate (EUR does show it). No tooltip, no inline explanation. |
| **Total** | | **22/40** | **Acceptable — significant improvements possible** |

---

## Anti-Patterns Verdict

**LLM assessment — does this look AI-generated?**

Partially. The three-card layout is the most generic marketing-page move possible for a multi-currency feature: one card per currency, same structure, same size, same shadow. The emoji flag + currency code + serif amount + pill badge is literally the first-training-data reflex for a finance app currency demo. It's not ugly — the typography is strong and the teal accent is coherent — but the composition is predictable. A visitor who has seen any fintech landing page in the past three years has already seen this layout.

The section-level label "Multi-Currency Support" in an uppercase pill chip is a named AI editorial scaffold pattern (the automated detector calls it out by name). The detector found 4 such kickers page-wide, and this section is one of them.

The `#ffffff` card backgrounds violate the shared design law ("never use #fff") and create a stark floating effect against the `--offwhite` background rather than an integrated warm surface.

**Deterministic scan — what the automated detector found:**

The full-file scan caught these items relevant to the currency section:

- **`low-contrast` (×1, currency-specific):** `#3D9B9B` (teal) on `#ffffff` = 3.3:1. Fails WCAG AA for body text (need 4.5:1). This hits the `.fx-badge` text color and the "Multi-Currency Support" section kicker. The `.fx-badge` text is rendered at 9px — not remotely "large text" — making the failure more severe.
- **`repeated-section-kickers` (×1, currency-specific):** "Multi-Currency Support" is called out by name as one of four repeated kickers on the page.
- **`low-contrast` (false positives in this section):** Several `#faf7f2 on #ffffff` (1.1:1) hits appear in the scan. These are the offwhite page background being tested against white card surfaces — this is a real structural issue (pure white cards on the offwhite ground) but manifests as a style-rendering artifact in the detector. Real fix: replace `background: white` with a warm tinted surface.

The detector also flagged `nested-cards` (4 instances) and `hero-eyebrow-chip` (1 instance), but these originate in the phone mockup sections and the hero, not the currency section.

---

## Overall Impression

The currency section communicates its core idea clearly in about two seconds: you have wallets in multiple currencies, and Yobi rolls them up into one total. The headline is sharp. But the execution is the first thing you'd reach for when building a multi-currency feature demo — and that's the problem. A section promising that Yobi "is built for people whose financial life doesn't fit one country" deserves a composition that feels as expansive and fluid as that claim. Three white boxes on an off-white page does not.

The biggest single opportunity: fix the "Combined runway" label. It's the one thing in this section that will make an interested visitor stop and distrust the page.

---

## What's Working

1. **The headline.** "Wherever you earn, wherever you spend." is earned, specific, and identity-resonant. It doesn't just describe a feature — it names the user's life. Keep it.
2. **DM Serif for amounts.** Financial figures in a high-contrast serif feel authoritative. The 52px combined total reads as a confident, satisfying endpoint. The weight contrast between the 10px currency code and the 26px serif amount works well within each card.
3. **Flag + code pairing.** Users process flag emoji faster than text. Pairing 🇺🇸 with `USD` gives two recognition paths for the same information — one emotional, one analytical. Effective shorthand that doesn't talk down to the user.

---

## Priority Issues

**[P1] "Combined runway · all currencies" is the wrong label for a dollar amount**

- **What:** The combined total strip uses "runway" as a label for `$8,906.42`. Throughout the rest of the page, "runway" is defined as a number of *days* of expenses you can cover. Here, the same word labels a dollar balance. A visitor who has absorbed the runway concept will pause, confused — is $8,906.42 a number of days? A dollar figure? Why does runway mean something different here?
- **Why it matters:** Label contradictions erode trust. In a finance app, trust is the product. A visitor who catches this inconsistency will question the accuracy of the app itself.
- **Fix:** Replace with "Combined balance · all currencies" or "Total across all wallets." If you want to keep the runway framing, show the computed runway in days: "142 days of runway · across all currencies." That would also make this section's number consistent with the runway concept shown in the features section.
- **Suggested command:** `/impeccable clarify currency`

**[P2] FX badge shows three semantically different things**

- **What:** The `.fx-badge` element serves a different informational purpose in each of the three cards. USD: "Base currency" (a role label). EUR: "1 EUR = 1.08 USD" (a specific live rate). THB: "Live FX rate" (a category description with no actual rate). Three cards, three different types of badge content — visually identical, semantically inconsistent.
- **Why it matters:** A user scanning for the THB exchange rate will see "Live FX rate" and wonder what the rate actually is. The EUR card answered that question; THB didn't. The inconsistency teaches the user they can't rely on the badge for consistent information.
- **Fix:** Show the actual rate for all foreign currencies: "1 EUR = 1.08 USD", "1 THB = 0.029 USD". Remove the badge entirely from the USD card (it's understood as base, and "Base currency" adds nothing a savvy user doesn't know). Consistent slot, consistent information type.
- **Suggested command:** `/impeccable clarify currency`

**[P2] Teal on white fails WCAG AA**

- **What:** The `.fx-badge` uses `color: var(--teal)` (`#3D9B9B`) against a white background — 3.3:1 contrast ratio. The badge text is rendered at 9px, making the WCAG AA threshold 4.5:1 (small text), which this fails by a meaningful margin. The section-kicker "Multi-Currency Support" also renders teal on the off-white background.
- **Why it matters:** 9px teal text on white is hard to read for anyone, inaccessible for users with low vision. The automated detector flagged this.
- **Fix:** Either darken the teal to pass contrast — approximately `oklch(48% 0.085 185)` passes 4.5:1 on white — or invert the badge: teal background, off-white or ink text. Inverting also gives the badge more visual presence, which the 9px text currently lacks entirely.
- **Suggested command:** `/impeccable audit currency`

**[P2] Pure white card backgrounds (`#ffffff`) on an off-white ground**

- **What:** `.cur-card { background: white }` — the design laws explicitly forbid `#fff`. On the `--offwhite` (#FAF7F2) page background, the white cards float with a sharp pop that reads as clinical rather than warm. The automated detector caught multiple `#faf7f2 on #ffffff` near-zero-contrast hits as a consequence.
- **Why it matters:** The page's entire visual language is warm (offwhite ground, persimmon/teal/gold palette, DM Serif, washi texture). White cards introduce a cold, stark element that breaks that warmth precisely in the section that should feel the most welcoming — multi-currency means the app adapts to you, wherever you are.
- **Fix:** Replace `background: white` with a warm tinted surface. `--warm-mid` (`#F3EEE6`) or a card-specific variable like `oklch(98% 0.008 75)` gives enough contrast against the offwhite ground while staying within the warm register.
- **Suggested command:** `/impeccable colorize currency`

**[P3] Three identical cards undersell the feature**

- **What:** All three `.cur-card` elements share identical structure, padding, border-radius, shadow, and size. There is no visual differentiation by currency type or balance size. For a marketing demo, identical structure implies identical importance — but the currencies have different roles (base vs. foreign), different balance sizes, and represent a globally diverse financial life.
- **Why it matters:** The section claims Yobi is "built for people whose financial life doesn't fit one country." Three identical white boxes don't embody that. The composition contradicts the copy.
- **Fix:** Consider differentiating cards by balance weight (wider or taller card for larger balance), adding a subtle flag-derived tint to each card (🇺🇸 → warm blue wash, 🇪🇺 → warm gold wash, 🇹🇭 → warm red wash), or varying the layout (feature the base currency card at a larger scale). Any of these makes the section feel discovered rather than templated.
- **Suggested command:** `/impeccable bolder currency`

---

## Persona Red Flags

**Jordan (First-Timer)** — landing page first impression, trust-building

Walking through the currency section as Jordan, who has never used Yobi or seen a runway-based finance app:

- **"Combined runway · all currencies"** — Jordan reads "runway" for the first time here (or has just learned it from the features section above) and now sees it attached to a dollar amount. She expected a number of days. She second-guesses whether she understood the concept correctly, and checks back up the page. Friction introduced at exactly the wrong moment — when Yobi should be closing the sale.
- **"Live FX rate" badge on THB** — Jordan is curious what the live rate is. The EUR card told her. THB doesn't. She notices the inconsistency and wonders if the data is reliable or if this is placeholder content.
- **9px badge text** — Jordan tries to read the currency code label and the badge. At 9px, the badge is a blur unless she's close to her screen. She probably can't read it at all on a laptop at arm's length.

**Casey (Distracted Mobile User)** — one-handed, interrupted, thumb-zoning

- **Card layout on small screens:** Three cards with `min-width: 175px` will wrap on a 375px viewport. They likely stack into a single column. The combined total appears below all three, requiring Casey to scroll past 3 stacked cards (each ~130px tall) to reach the section endpoint. If she arrives scrolling fast, she may not register the total at all.
- **9px badge text** — on a retina mobile screen, 9px is physically tiny. At 1x DPR, it's invisible. This is the only piece of text in the section that attempts to explain the live rate — Casey will skip it.
- **Tap target audit** — the section has no tappable elements, which means no tap target failures, but also means Casey gets zero signal about Yobi's interaction quality from this section. A low bar.

---

## Minor Observations

- The `.cur-code` (currency code label) at 10px, 700 weight, 0.12em tracking is so small and muted that it provides almost no scannability gain over the flag alone. If the code serves a purpose, it should be larger; if the flag is sufficient, remove the code.
- "Updated 4 min ago" in `opacity: 0.7` at 13px already low-contrast (`--ash` on `--offwhite`) — the opacity reduction pushes this below any meaningful contrast threshold. If this line is important (it establishes credibility), make it readable. If it's not, remove it.
- The combined total uses a fractional amount styled as `$8,906<span style="font-size:28px;opacity:0.5">.42</span>`. The inline span is fine, but `opacity: 0.5` on the cents buries them. Standard financial convention is to render cents at reduced scale but maintained contrast — not ghosted.

---

## Questions to Consider

- What if the combined total showed runway in *days* rather than dollars? It would make this section pay off the runway concept established earlier, rather than introducing a different mental model.
- Does the static nature of this section serve a purpose? A subtle animated ticker (numbers counting up on scroll) would make "live rates" feel real without requiring actual API calls.
- What would happen if the three cards had visibly different weights — sizes or tints — that reflected the different balances? Would the section start to feel like a real wallet view instead of a feature spec?
