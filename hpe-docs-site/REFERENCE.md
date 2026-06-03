# Reference: VitePress + HPE Docs Site

## Project Structure

```
docs/
├── .vitepress/
│   ├── config.js          # VitePress configuration
│   └── theme/
│       ├── index.js       # Theme entry (imports custom.css)
│       └── custom.css     # HPE brand overrides
├── index.md               # Home page (layout: home)
├── *.md                   # Content pages
Makefile                   # Build/publish commands
package.json               # Dependencies (vitepress)
```

## VitePress Configuration

```js
// docs/.vitepress/config.js
export default {
  title: 'Project Name',
  description: 'Project description',
  // CRITICAL: GHE Pages base path includes org name
  // Format: /<org-name>/<repo-name>/
  base: '/hpe-networking/dnsguard/',
  head: [
    ['meta', { name: 'theme-color', content: '#01A982' }],
  ],
  themeConfig: {
    siteTitle: 'Project Name',
    nav: [...],
    sidebar: [...],
    socialLinks: [
      { icon: 'github', link: 'https://github.hpe.com/hpe-networking/<repo>' }
    ],
    footer: {
      message: 'Built by <a href="https://github.hpe.com/hpe-networking">HPE Networking</a>',
      copyright: 'Copyright © 2026 Hewlett Packard Enterprise Development LP'
    }
  },
  buildEnd(siteConfig) {
    const fs = require('node:fs')
    const path = require('node:path')
    fs.writeFileSync(path.join(siteConfig.outDir, '.nojekyll'), '')
  }
}
```

## Theme Entry

```js
// docs/.vitepress/theme/index.js
import DefaultTheme from 'vitepress/theme'
import './custom.css'

export default DefaultTheme
```

## Custom CSS Template

```css
/* docs/.vitepress/theme/custom.css */
:root {
  --vp-c-brand-1: #01A982;
  --vp-c-brand-2: #00C799;
  --vp-c-brand-3: #01A982;
  --vp-c-brand-soft: rgba(1, 169, 130, 0.14);

  --vp-c-text-1: #333333;
  --vp-c-text-2: #555555;
  --vp-c-bg: #ffffff;
  --vp-c-bg-soft: #f7f7f7;
  --vp-c-bg-mute: #eeeeee;

  --vp-button-brand-bg: #01A982;
  --vp-button-brand-hover-bg: #008567;
  --vp-button-brand-active-bg: #006C54;
  --vp-button-brand-text: #ffffff;

  --vp-home-hero-name-color: transparent;
  --vp-home-hero-name-background: linear-gradient(135deg, #01A982 0%, #00739D 100%);

  --vp-font-family-base: 'MetricHPE', 'Inter', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  --vp-font-family-mono: 'JetBrains Mono', 'Fira Code', 'SF Mono', Menlo, monospace;
}

.dark {
  --vp-c-brand-1: #17EBA0;
  --vp-c-brand-2: #01A982;
  --vp-c-brand-3: #17EBA0;
  --vp-c-brand-soft: rgba(23, 235, 160, 0.14);
  --vp-c-bg: #1A1A2E;
  --vp-c-bg-soft: #222240;
  --vp-c-bg-mute: #2A2A4A;
  --vp-c-text-1: #ffffffde;
  --vp-c-text-2: #ffffffab;
  --vp-button-brand-bg: #17EBA0;
  --vp-button-brand-hover-bg: #01A982;
  --vp-button-brand-text: #1A1A2E;
}

.VPNav { border-bottom: 1px solid var(--vp-c-divider); backdrop-filter: blur(12px); }
.VPFooter { border-top: 3px solid var(--vp-c-brand-1); }

.VPHero .name { font-size: 3rem; font-weight: 800; letter-spacing: -0.03em; }
.VPHero .tagline { font-size: 1.25rem; line-height: 1.6; max-width: 560px; }
.VPHero .actions .VPButton { border-radius: 8px; font-weight: 600; padding: 12px 24px; transition: all 0.2s ease; }
```

## Deployment (GHE Pages)

### GitHub Pages Settings

- Source: **Deploy from a branch**
- Branch: `gh-pages`
- Folder: `/docs`

### Makefile Publish Target

```makefile
publish: build
	rm -rf _gh-pages-deploy
	mkdir -p _gh-pages-deploy/docs
	cp -r docs/.vitepress/dist/* _gh-pages-deploy/docs/
	touch _gh-pages-deploy/docs/.nojekyll
	touch _gh-pages-deploy/.nojekyll
	cd _gh-pages-deploy && \
	git init -b gh-pages && \
	git add -A && \
	git commit -m "Deploy docs [$(shell date -u +%Y-%m-%dT%H:%M:%SZ)]" && \
	git push -f $(shell git remote get-url origin) gh-pages
	rm -rf _gh-pages-deploy
```

### CI Workflow Deploy Step

```yaml
- name: Deploy to gh-pages branch
  if: github.event_name != 'pull_request'
  run: |
    git config --global user.name "github-actions[bot]"
    git config --global user.email "github-actions[bot]@users.noreply.github.com"
    mkdir -p deploy/docs
    cp -r docs/.vitepress/dist/* deploy/docs/
    touch deploy/.nojekyll
    touch deploy/docs/.nojekyll
    cd deploy
    git init -b gh-pages
    git add -A
    git commit -m "Deploy VitePress site from ${{ github.sha }}"
    git push --force "https://x-access-token:${{ github.token }}@github.hpe.com/${{ github.repository }}.git" gh-pages:gh-pages
```

## Common Pitfalls

| Problem | Cause | Fix |
|---------|-------|-----|
| CSS not loading (404) | Wrong `base` path | GHE Pages: `/<org>/<repo>/` not just `/<repo>/` |
| Theme not bundled | `.vitepress/` in `.gitignore` | Only ignore `dist/` and `cache/` subdirs |
| Config symlink breaks theme | Symlink resolves relative paths wrong | Use a real file in `docs/.vitepress/config.js` |
| ESM import error in config | Using `import from 'vitepress'` without `"type": "module"` | Use plain `export default {}` with `require()` for Node APIs |
| No `/docs` folder on gh-pages | Build output pushed to branch root | Deploy step must create `docs/` subdir matching Pages config |
| Jekyll interfering | Missing `.nojekyll` | Add `.nojekyll` at root AND inside `/docs` on gh-pages |

## Home Page Template

```md
---
layout: home

hero:
  name: Project Name
  text: Subtitle Here
  tagline: A clear description of what this project does.
  actions:
    - theme: brand
      text: Get Started
      link: /getting-started
    - theme: alt
      text: View on GitHub
      link: https://github.hpe.com/hpe-networking/<repo>

features:
  - title: Feature One
    details: Description of the feature with clear, concise language.
  - title: Feature Two
    details: Description of the feature with clear, concise language.
---
```

Note: Use `title` only in features — no `icon` field with emojis.
