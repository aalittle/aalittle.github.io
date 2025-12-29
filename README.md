# aalittle.com

Personal website for Andrew Little. Built with Jekyll and hosted on GitHub Pages.

## Quick Start

1. Create a new GitHub repo (e.g., `aalittle-site` or `aalittle.github.io`)
2. Push this folder to the repo
3. Go to **Settings → Pages** and set source to `main` branch
4. Add your custom domain `aalittle.com` in the Pages settings
5. Update your DNS with a CNAME record pointing to `<username>.github.io`

GitHub Pages will automatically build the Jekyll site on every push.

## Writing a New Post

1. Create a new file in `_posts/` with the format: `YYYY-MM-DD-title-slug.md`
2. Add front matter at the top:

```yaml
---
title: "Your Post Title"
date: 2025-01-20
description: "A brief description for the homepage listing"
read_time: 5
---
```

3. Write your post in Markdown below the front matter
4. Commit and push. The site rebuilds automatically.

## File Structure

```
├── _config.yml          # Site configuration
├── _layouts/
│   ├── default.html     # Base template
│   └── post.html        # Blog post template
├── _includes/
│   ├── header.html      # Site header + nav
│   └── footer.html      # Site footer
├── _posts/              # Blog posts (Markdown)
├── assets/
│   ├── css/style.css    # Main stylesheet
│   └── headshot.jpg     # Your photo (add this)
├── index.html           # Homepage
└── feed.xml             # RSS feed (auto-generated)
```

## Customization

### Update your info
Edit `_config.yml` to change:
- Site title and description
- Author email, LinkedIn, GitHub URLs

### Add your headshot
Place your photo at `assets/headshot.jpg`. Recommended: square crop, at least 240x240px.

### Modify styles
Edit `assets/css/style.css`. The site uses CSS variables for colors, so you can easily adjust the palette in the `:root` section.

## Local Development (Optional)

If you want to preview changes locally before pushing:

```bash
# Install Jekyll (requires Ruby)
gem install bundler jekyll

# Run local server
bundle exec jekyll serve

# View at http://localhost:4000
```

## Cross-posting to LinkedIn

After publishing a post:
1. Copy the key points from your post
2. Create a LinkedIn post with a summary and link back to the full article
3. The RSS feed at `/feed.xml` can also be used with automation tools

## Dark Mode

The site automatically adapts to system preferences using `prefers-color-scheme`. No toggle needed—it just works.

## License

Content © Andrew Little. Code structure available for personal use.
