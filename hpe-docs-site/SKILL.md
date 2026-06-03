---
name: hpe-docs-site
description: "Build modern GitHub Pages documentation sites with official HPE branding and VitePress. Use when creating documentation sites, updating page styles, fixing GitHub Pages deployments, or applying HPE brand identity to web documentation. Triggers: docs site, GitHub Pages, VitePress, HPE brand, documentation styling, deploy pages."
---

# HPE Documentation Site

Build professional, modern documentation sites using VitePress with official HPE brand identity.

## Quick Start

```bash
# Build and preview locally
make build && make preview

# Deploy to GitHub Pages (gh-pages branch)
make publish
```

## Design System

### HPE Brand Colors

| Token | Hex | Usage |
|-------|-----|-------|
| Primary Green | `#01A982` | CTAs, links, active states |
| Dark Green | `#008567` | Hover states |
| Deep Green | `#006C54` | Active/pressed states |
| HPE Blue | `#00739D` | Accents, gradients |
| Light Green (dark mode) | `#17EBA0` | Primary in dark mode |
| Text Primary | `#333333` | Body text |
| Text Secondary | `#555555` | Muted text |
| Background | `#FFFFFF` | Page background |
| Surface | `#F7F7F7` | Cards, code blocks |

### Typography

```css
--vp-font-family-base: 'MetricHPE', 'Inter', -apple-system, BlinkMacSystemFont,
  'Segoe UI', Roboto, sans-serif;
--vp-font-family-mono: 'JetBrains Mono', 'Fira Code', 'SF Mono', Menlo, monospace;
```

### Dark Mode

- Use desaturated/lighter tonal variants, not inverted colors
- Primary becomes `#17EBA0`, background becomes `#1A1A2E`
- Test contrast independently from light mode

## Architecture

See [REFERENCE.md](REFERENCE.md) for VitePress config, deployment, and file structure details.

## UI/UX Rules

See [UX-GUIDELINES.md](UX-GUIDELINES.md) for the pre-delivery checklist and anti-patterns.
