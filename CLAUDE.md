# Claude Instructions for aalittle.github.io

## What This Site Is

This is Andrew Little's personal website hosted on GitHub Pages. It serves two purposes:

1. **Personal/Professional Site** - A Jekyll-based site showcasing Andrew's role as VP of Engineering at Salesforce, his writing on engineering leadership, and side projects.

2. **App Legal Pages** - Hosts EULA and Privacy Policy pages for published iOS apps (Wealth+, WealthPlus Net Worth).

## Site Structure

```
├── _config.yml          # Jekyll configuration
├── _includes/           # Header, footer partials
├── _layouts/            # Page templates (default, post)
├── _posts/              # Blog posts (markdown)
├── assets/
│   ├── css/style.css    # Main stylesheet
│   └── headshot.jpg     # Profile photo
├── index.html           # Homepage
├── feed.xml             # RSS feed
├── wealthplus2_eula.html          # Wealth+ 2 EULA (DO NOT MODIFY)
└── wealthplus2_privacypolicy.html # Wealth+ 2 Privacy Policy (DO NOT MODIFY)
```

## Design Principles

- Clean, minimal design with focus on readability
- Dark/light mode support via CSS
- Mobile responsive
- Typography: Fraunces (headings) + Source Sans 3 (body)
- Sections: About, Writing, Projects, Contact

## Content Guidelines

### About Section
- Andrew leads Field Service Mobile at Salesforce
- Global team of 70+ engineers
- Industries served: manufacturing, energy, telecom, healthcare
- Focus areas: reliability, AI-guided workflows, developer velocity

### Stats (keep updated as needed)
- Years at Salesforce
- YoY User Growth percentage
- Global Team size

### Projects
Current order (most prominent first):
1. Wealth+ 2 (App Store link)
2. WealthPlus Net Worth (App Store link)
3. Leave Out Violence App (no link, description only)
4. FineGrind (no link, description only)

## Protected Files - DO NOT MODIFY

The following files are legal documents for published apps and must not be altered without explicit instruction:

- `wealthplus2_eula.html`
- `wealthplus2_privacypolicy.html`

These URLs are referenced in live App Store apps.

## Adding New Blog Posts

Create a new file in `_posts/` with the format:
```
YYYY-MM-DD-title-slug.md
```

Required front matter:
```yaml
---
layout: post
title: "Post Title"
description: "Brief description for previews"
date: YYYY-MM-DD
---
```

## Deployment

- Site auto-deploys via GitHub Pages when pushing to `master`
- Changes typically visible within 1-2 minutes
- No local Jekyll setup required (GitHub builds it)

## Contact Links

- LinkedIn: https://www.linkedin.com/in/aalittle/
- GitHub: https://github.com/aalittle
- No public email displayed on site
