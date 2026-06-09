---
layout: layout.njk
title: Microsites Strategy
---

# Microsites Strategy

[View Implementation Guide](/implementation-guide/) | [View Production Architecture](/production-architecture/)

## User Request
> Teach me about microsites.
> - I have basically zero background on this topic
> - My goal is to determine if microsites are a valid platform to serve and share content created with Claude Code or other Agentic assistants
> - For the audience I need to connect with, Markdown is difficult to consume
> - My deployment goals are local for development, and AWS for production
>   - The AWS environment should be painless to maintain, secure, and cost effective. Serverless or service-oriented would be preferred

## Response

Since you have zero background in this topic, let's start with the concept and move toward the technical implementation for your specific goals.

### What is a Microsite?
A **microsite** is a small, standalone website (typically 1–5 pages) designed for a single, specific purpose. Unlike a main corporate website, which tries to be everything to everyone, a microsite focuses on one a specific topic, product launch, or campaign.

**Why they are useful for your goal:**
AI assistants generate a lot of "atomic" content—single-topic guides, specific tool tutorials, or focused analysis. Creating a full-scale website for every AI-generated piece of content is overkill. A microsite provides a dedicated "home" for a specific AI-generated output without the complexity of a large site architecture.

---

### Addressing the "Markdown Problem"
You mentioned your audience finds Markdown difficult to consume. This is a common friction point: **AI likes to write in Markdown, but humans like to read formatted HTML.**

The solution is a **Static Site Generator (SSG)**. 
An SSG acts as a translator:
1. **Input:** You feed it the Markdown files created by Claude Code.
2. **Process:** It applies a visual template (CSS/HTML) to that text.
3. **Output:** It generates a set of standard web pages (HTML) that look like a professional website to the end-user.

This allows you to keep your source files in a format Claude Code understands perfectly, while your audience sees a polished, accessible webpage.

---

### Is this a valid platform for AI content?
**Yes, absolutely.** In fact, it is one of the most efficient ways to share agentic output.

*   **Speed of Iteration:** Claude Code can write a new page in seconds; an SSG can "build" that page into a site instantly.
*   **Version Control:** Since the source is just text (Markdown), you can use Git to track exactly how the AI improved the content over time.
*   **Portability:** Because the output is "static" (just HTML/CSS), the site can be hosted anywhere for almost zero cost.

---

### Recommended Technical Architecture (AWS Serverless)
To meet your goals of being **painless, secure, and cost-effective**, I recommend a "Static Web Hosting" stack. This is the gold standard for serverless content delivery.

#### 1. Local Development
*   **Tool:** Use a lightweight SSG like **Eleventy (11ty)** or **Hugo**.
*   **Workflow:** Claude Code writes `.md` files -> You run a local build command -> You view the result in your browser at `localhost:8080`.

#### 2. Production Deployment (AWS)
Instead of renting a server (which is expensive and requires maintenance), use these three services:

*   **AWS S3 (Simple Storage Service):** Your site is essentially a folder of files. S3 can "host" these files as a website. It costs pennies per month.
*   **AWS CloudFront (CDN):** This sits in front of S3. It provides:
    *   **Security:** It handles HTTPS (SSL) so your site is secure.
    *   **Performance:** It caches your site globally so it loads instantly for users.
    *   **Cost:** Extremely cheap for low-to-medium traffic.
*   **GitHub Actions (CI/CD):** This is the "glue." When you push the AI-generated content to GitHub, this automation automatically builds the site and uploads it to S3. You never have to manually "upload" files.

### Summary for your Strategy
| Goal | Solution |
| :--- | :--- |
| **Consumption** | **Markdown -> SSG -> HTML** |
| **Content Source** | **Claude Code / Agentic Assistants** |
| **AWS Infrastructure** | **S3 -> CloudFront (Serverless)** |
| **Maintenance** | **Automated via GitHub Actions (Zero-Ops)** |
