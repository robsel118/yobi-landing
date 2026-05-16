# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Yobi Landing Page** — A marketing/product landing page for Yobi, a personal finance app built around the Kakeibo (Japanese household budgeting) method. The entire page is a single `index.html` file with embedded CSS and JavaScript.

The page showcases key features:
- Kakeibo-based budget tracking (dividing spending into Survival, Optional, Culture, and Extra)
- Interactive runway calculation (how many days of expenses you can cover)
- Multi-currency support
- What-If scenario planning
- Ko, an animated fox character that reacts to user interactions

## File Structure

- `index.html` — Single-file landing page with embedded styles and JavaScript
- `skills-lock.json` — Tracks the "impeccable" skill dependency
- `.agents/skills/impeccable/` — Imported skill for design/content guidance

## Viewing & Development

No build process is needed. To view the page:

```bash
# Open in default browser
open index.html

# Or use a local server (recommended for testing interactive features)
python3 -m http.server 8000
# Then visit http://localhost:8000/index.html
```

## Code Structure

### HTML Sections

The page is organized in semantic order with section IDs for easy navigation:

1. **Navigation** (`<nav>`) — Sticky header with logo and CTA
2. **Hero** (`#hero`) — Main pitch with Kakeibo circle visualization
3. **Problem** (`#problem`) — Before/After cards showing the problem Yobi solves
4. **Features** (`#features`) — 3 feature sections with phone mockups (Runway, Pillars, Reflection)
5. **Kakeibo Method** (`#kakeibo-method`) — 4-step explanation of the Kakeibo budgeting approach
6. **Pillars** (`#pillars`) — 4-column breakdown of budget categories + animated bar charts
7. **What-If** (`#whatif`) — Interactive expense toggle calculator
8. **Currency** (`#currency`) — Multi-currency wallet display
9. **Philosophy** (`#philosophy`) — Quote + testimonials
10. **Footer CTA** (`#footer-cta`) — App Store download button

Ko the fox appears in multiple sections with animated tail and speech bubbles.

### CSS Design System

Custom properties (root variables) for consistency:

```css
:root {
  --persimmon: #D4704A;    /* Primary accent, Budget Survival category */
  --teal: #3D9B9B;         /* Budget Optional category */
  --purple: #9B6FAF;       /* Budget Culture category */
  --gold: #E8A43C;         /* Budget Extra category + runway numbers */
  --offwhite: #FAF7F2;     /* Background */
  --ink: #1A1510;          /* Text */
  --ash: #6B6560;          /* Secondary text */
  --warm-mid: #F3EEE6;
  --warm-deep: #EDE5D8;
  --section-pad: 80px;     /* (dynamic, modified by tweaks) */
  --serif: 'DM Serif Display';
  --sans: 'DM Sans';
}
```

The page includes a subtle Washi paper texture overlay using SVG fractal noise.

### JavaScript Patterns

#### Intersection Observer (Scroll Reveals)

- `.reveal` elements fade in + slide up when scrolled into view
- Separate observers for: scroll reveals, bar chart fills, count-up animations

#### Interactive Features

1. **Scroll Nav Effect** — Nav gets glassmorphic background when page scrolls
2. **Tweaks Panel** — Fixed toggle button for customizations:
   - Font style (serif vs sans)
   - Ko visibility
   - Accent color swatch
   - Section spacing (compact/default/airy)
3. **What-If Calculator** — Toggles for each expense, dynamically updates runway number:
   - Formula: `31 + (skipped_savings_amount * 0.28) = new_runway_days`
   - Runway turns red if < 35 days
   - Ko reacts with contextual messages
4. **Count-Up Animation** — Used for runway numbers and pillar chart percentages

#### Ko the Fox

- Animated SVG character with wagging tail (hover effect)
- Appears in different poses: sit, celebrate, hug
- Speech bubbles with contextual messages
- Hidden by default, toggled via tweaks panel

### Phone Mockups

The page shows the app UI inside simulated phone frames (`.phone`):

- Dark bezel frame with notch
- Screen content with status bar, app header, and card-based UI
- Different sizes (`.phone.sm` for smaller variant)
- Used to showcase: wallet view (hero), runway display, transaction list, reflection journal

### Responsive Design

Breakpoint at `max-width: 920px`:
- Grid layouts collapse to single column
- Pillars change from 4-col to 2-col
- Phone frame size reduces
- Tweaks panel hidden on mobile

## Key Editing Patterns

### Updating Content

- Text is inline in the HTML (no external content files)
- Colors reference CSS custom properties (`var(--persimmon)`, etc.) — edit in `:root`
- Accent color can be changed via tweaks panel or by updating `--persimmon`

### Modifying Interactive Behavior

- **What-If formula**: Line 988, `Math.round(31 + removed * 0.28)`
- **Ko messages**: Lines 994–998 in the `updateWhatIf()` function
- **Scroll reveal threshold**: Line 945, `threshold: 0.12`
- **Count-up duration**: Lines 974, 990 use `1200` and `500` milliseconds

### Adding Animations

- Use CSS transitions/keyframes for simple effects
- Use `requestAnimationFrame` for smooth counting animations
- IntersectionObserver for scroll-triggered effects (no scroll event listeners)

## Dependencies

- **Google Fonts**: DM Serif Display, DM Sans (linked in `<head>`)
- **Impeccable Skill**: Referenced in `skills-lock.json` for design/content guidance

No npm packages or build tools required.

## Testing

The page is fully functional as-is. When making changes:

1. **Visual check** — Open in browser, scroll through all sections
2. **Interaction test** — Toggle tweaks panel, test What-If calculator, hover Ko
3. **Responsive test** — Check `max-width: 920px` media query breakpoint
4. **Animation smoothness** — Check scroll reveals, count-ups load with IntersectionObserver
5. **Link integrity** — Verify anchor links work (nav uses `#` links)

The page uses a Washi texture overlay and subtle shadows — ensure these render correctly across browsers.
