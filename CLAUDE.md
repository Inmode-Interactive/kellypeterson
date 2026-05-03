# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hugo-based political campaign website for Kelly Peterson's Texas State Representative District 126 campaign. The site is deployed on Netlify and uses Bootstrap 5, custom partials, and a content-driven architecture.

## Development Commands

### Local Development
```bash
hugo serve
```
Runs local development server at `localhost:1313`

### Build Commands
```bash
# Production build (as used by Netlify)
hugo --gc --minify && cp robots.production.txt public/robots.txt

# Preview/staging build
hugo --gc --buildFuture && cp robots.staging.txt public/robots.txt
```

### Requirements
- Hugo >= v0.108.0 (Extended version required)
- Configured in netlify.toml to use Hugo v0.141.0

## Architecture

### Content Organization
The site uses Hugo's taxonomy system with a custom `section_categories` taxonomy to organize page sections:

- **Content structure**: Content is organized in `/content/` with subdirectories for different page types (news, policies, sections, contact, etc.)
- **Sections system**: Home page sections are markdown files in `/content/sections/` that include frontmatter parameters like `weight`, `id`, `position`, `size`, and `section_categories`
- **Section rendering**: The `layouts/partials/page-sections.html` partial uses Hugo taxonomy queries to filter and render sections based on the page's `sections` parameter matching the `section_categories` taxonomy

### Layout System
- **Main layout**: `layouts/index.html` serves as the homepage template
- **Partials**: Reusable components in `layouts/partials/` including:
  - `head.html`: Meta tags, CSS, scripts
  - `header.html`: Site navigation
  - `footer.html`: Footer with social links and "paid for" text
  - `page-sections.html`: Dynamic section rendering using taxonomy filtering
  - `signup.html`: Email signup form
  - `form.html`: Contact/volunteer forms
- **Section layouts**: Empty placeholder files in `layouts/sections/` (content-driven architecture)

### Key Configuration
- **config.yaml**: Contains site parameters including social media links, form endpoints (usebasin.com), donation links (WinRed), and menu structure
- **Forms**: Two Basin form endpoints configured - one for general contact, one for volunteers
- **Donation**: External WinRed link configured in site params
- **Social**: Twitter (X) and Facebook links in site params

### Frontend Assets
- **CSS**: Bootstrap 5.1.3 (customized/minified) in `/assets/css/`
- **JavaScript**: Magnific Popup, AOS (Animate on Scroll), Owl Carousel
- **Images**: Static images in `/static/img/`
- **Lazy loading**: Images use `lazyload` class with `data-src` attributes

### Deployment
- **Platform**: Netlify
- **Build**: Configured in `netlify.toml` with different environments (production, deploy-preview, branch-deploy)
- **Robots.txt**: Different files for production vs staging (copied during build)
- **Plugin**: netlify-plugin-image-optim for image optimization

## Content Editing

### Adding Sections to Homepage
Create a new markdown file in `/content/sections/` with frontmatter:
```yaml
---
title: Section Title
weight: 1  # Controls display order
id: section-id  # HTML anchor
position: justify-content-end align-content-center  # Flexbox positioning
size: col-12 col-md-7  # Bootstrap column classes
image: /img/background.jpg  # Optional background image
section_image: /img/inline.jpg  # Optional inline image
section_categories:
    - Home  # Must match the "sections" param in content/_index.md
---
```

### Homepage Banner
Configured in `/content/_index.md` frontmatter:
- `banner_title`: Main heading text
- `banner_tagline`: Subheading (supports HTML)
- `banner_image`: Background image path
- `banner_video`: YouTube video ID (commented out by default)
- `candidate_image`: Candidate photo path
- `sections`: Value that matches section_categories taxonomy for filtering sections

## Important Notes

- The site uses a taxonomy-based section system where sections are filtered by matching the page's `sections` parameter with content's `section_categories` taxonomy
- NetlifyCMS support is included but commented out in the codebase
- Google Tag Manager placeholder exists in config but is commented out
- Instagram link is available in config but commented out
