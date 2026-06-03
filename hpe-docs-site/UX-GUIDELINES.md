# UX Guidelines for Documentation Sites

## Pre-Delivery Checklist

### Visual Quality
- [ ] No emojis used as icons — use SVG icons (Heroicons, Lucide) or omit
- [ ] Consistent icon family and style throughout
- [ ] Official HPE brand assets with correct proportions
- [ ] Semantic color tokens used consistently (no hardcoded hex per page)
- [ ] Light and dark mode tested independently

### Typography & Readability
- [ ] Body text minimum 16px (avoids iOS auto-zoom)
- [ ] Line height 1.5-1.75 for body text
- [ ] Line length 60-75 characters max per line
- [ ] Font scale is consistent (e.g., 12/14/16/18/24/32)
- [ ] Heading hierarchy is sequential (h1 > h2 > h3, no skipping)
- [ ] Code blocks use monospace font with adequate contrast

### Color & Contrast
- [ ] Text contrast ratio >= 4.5:1 (WCAG AA)
- [ ] Large text contrast >= 3:1
- [ ] Color is not the only indicator of meaning
- [ ] Dark mode uses desaturated lighter tonal variants
- [ ] Links distinguishable from surrounding text without relying on color alone

### Layout & Responsive
- [ ] Mobile-first approach, scales up to desktop
- [ ] Breakpoints: 375px / 768px / 1024px / 1440px
- [ ] No horizontal scroll on any viewport
- [ ] Content width capped (max-w-6xl or similar)
- [ ] Spacing follows 4px/8px incremental system
- [ ] Fixed nav does not obscure content

### Navigation
- [ ] Current page clearly indicated in nav/sidebar
- [ ] Navigation consistent across all pages
- [ ] Skip-to-content link available for keyboard users
- [ ] Breadcrumbs for 3+ level deep hierarchies
- [ ] Search accessible from every page (if available)

### Performance
- [ ] Images use WebP/AVIF with responsive srcset
- [ ] Fonts loaded with font-display: swap
- [ ] Critical CSS inlined or early-loaded
- [ ] Lazy load below-fold content
- [ ] No layout shift on load (CLS < 0.1)

### Accessibility
- [ ] All meaningful images have alt text
- [ ] Focus states visible on all interactive elements (2-4px ring)
- [ ] Tab order matches visual order
- [ ] prefers-reduced-motion respected
- [ ] Skip navigation link present
- [ ] ARIA labels on icon-only buttons

### Interaction
- [ ] cursor-pointer on all clickable elements
- [ ] Hover states with smooth transitions (150-300ms)
- [ ] Buttons have distinct hover/active/disabled states
- [ ] No reliance on hover for critical information
- [ ] Touch targets minimum 44x44px on mobile

## Anti-Patterns to Avoid

| Anti-Pattern | Why | Alternative |
|--------------|-----|-------------|
| Emojis as structural icons | Inconsistent across platforms, unprofessional, not themeable | SVG icons from a consistent set |
| Gray-on-gray text | Fails contrast, unreadable | Use semantic text colors with 4.5:1 ratio |
| Placeholder-only labels | Disappear on focus, accessibility failure | Persistent visible labels |
| Mixing flat and skeuomorphic | Visual inconsistency | Pick one style, apply consistently |
| Animating width/height | Causes layout reflow, janky | Use transform/opacity only |
| Raw hex in components | Unmaintainable, breaks theming | CSS custom properties / design tokens |
| Linear easing for UI | Feels mechanical | ease-out for enter, ease-in for exit |
| Decorative-only animation | Distracting, harms reduced-motion users | Every animation conveys cause-effect |
| Icon-only navigation | Poor discoverability | Icon + text label |
| Fixed pixel containers | Breaks on different viewports | Responsive max-width with relative units |

## Design Principles for Documentation

1. **Content-first** — typography and whitespace serve readability
2. **Consistent rhythm** — 8px spacing grid, type scale, color tokens
3. **Professional restraint** — no gratuitous decoration, gradients, or effects
4. **Accessible by default** — contrast, focus states, keyboard nav, screen readers
5. **Fast** — minimal dependencies, lazy loading, optimized assets
6. **Brand-aligned** — HPE Green as primary accent, Inter/Metric as type, clean surfaces
