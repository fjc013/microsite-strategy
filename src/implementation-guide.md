---
layout: layout.njk
title: Implementation Guide
---

# Implementation Guide: Building a Local Microsite with Eleventy

This guide documents the step-by-step process of creating a professional, serverless-ready microsite to share AI-generated content.

## 1. Conceptual Approach
The goal was to transform AI-generated Markdown content into a format that is easy for humans to consume without requiring them to understand Markdown syntax. 

**The Pipeline:**
`AI Content (Markdown)` $\rightarrow$ `Static Site Generator (Eleventy)` $\rightarrow$ `Formatted Webpage (HTML/CSS)`

## 2. Local Environment Setup
The site is built using **Node.js** and **npm**. 

### Dependencies
- **Eleventy (11ty):** A lightweight Static Site Generator (SSG) that transforms Markdown into HTML.
- **Nunjucks:** The templating engine used for the site layout.

### Initial Installation
```bash
npm init -y
npm install @11ty/eleventy --save-dev
```

## 3. Project Architecture
A clean separation between source files and the generated site was established:

- `/src`: Contains all source content and templates.
- `/_site`: The output folder where the final HTML is generated.
- `src/_includes/layout.njk`: The master template that defines the visual look (CSS and HTML structure).

## 4. Content Implementation
The source content was placed in `src/index.md`. To connect the content to the visual design, **Frontmatter** was added to the top of the Markdown file:

```markdown
---
layout: layout.njk
title: Microsites Strategy
---
```

## 5. Key Technical Fixes & Optimizations

### The "Escaped HTML" Issue
Initially, the site rendered HTML tags as literal text (e.g., `<h1>` was visible on the page). This happened because the template engine was escaping the content for security.
- **Solution:** Added the `| safe` filter to the content variable in `layout.njk`.
- **Change:** `{% raw %}{{ content }}{% endraw %}` -> `{% raw %}{{ content | safe }}{% endraw %}`

### Visual Refinements
AI models often use LaTeX for arrows (`$\rightarrow$`), which don't render in standard HTML.
- **Solution:** Replaced all LaTeX arrow notation with the standard `->` for better readability and "flow" in a technical context.

## 6. Running the Site
Two primary scripts were added to `package.json` for ease of use:
- `npm start`: Launches a local development server with live-reload.
- `npm run build`: Generates the final static files in the `_site` directory.

## 7. Future Roadmap (Production)
The current local setup is designed to be mirrored exactly in an AWS serverless environment:
- **Storage:** AWS S3
- **Distribution:** AWS CloudFront (CDN)
- **Automation:** GitHub Actions for CI/CD
