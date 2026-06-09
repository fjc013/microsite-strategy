# Enterprise AI Microsite Platform

A lightweight, serverless-ready framework for transforming AI-generated agentic outputs (Markdown) into professional, secure, and highly consumable microsites.

## 🚀 Overview

This project provides a streamlined pipeline for sharing "atomic" AI content—such as technical guides, strategic analysis, and architectural blueprints—without the overhead of a full-scale CMS. By leveraging a Static Site Generator (SSG) and an "Edge-First" AWS architecture, it ensures that high-value AI insights are delivered in a polished, accessible format that meets the rigorous security standards of a regulated financial institution.

## ✨ Key Features

- **Markdown-to-Web Pipeline:** Uses Eleventy (11ty) to translate raw Markdown into clean, responsive HTML.
- **Identity-Gated Access:** Designed for integration with Amazon Cognito, bridging corporate SSO (Active Directory/F5 APM) and external identity providers (PingOne).
- **Visual Documentation:** Built-in support for Mermaid.js to render complex architectural diagrams directly from text.
- **Serverless Production Path:** Optimized for deployment via AWS S3 and Amazon CloudFront, ensuring zero-maintenance and minimal cost.
- **CI/CD Ready:** Automated build and deployment workflow via GitHub Actions.

## 🛠️ Local Development

### Prerequisites
- [Node.js](https://nodejs.org/) (v20+ recommended)
- npm (comes with Node.js)

### Installation
1. Clone the repository:
   ```bash
   git clone <your-repo-url>
   cd microsites
   ```
2. Install dependencies:
   ```bash
   npm install
   ```

### Running the Site
To start the local development server with live-reload:
```bash
npm start
```
The site will be available at `http://localhost:8080`.

To generate the final static build:
```bash
npm run build
```
The output will be located in the `/_site` directory.

## 🏗️ Project Structure

```text
.
├── src/               # Source files
│   ├── _includes/     # Templates and shared components
│   │   └── layout.njk # Master HTML layout
│   ├── index.md       # Homepage / Strategy
│   └── ...            # Additional microsite pages
├── _site/             # Generated static assets (Production output)
├── .eleventy.js       # Eleventy configuration
├── package.json       # Project dependencies and scripts
└── README.md          # Project documentation
```

## 🌐 Production Architecture (AWS)

The platform is designed for an enterprise-grade serverless deployment:

- **Origin:** Amazon S3 (Private bucket with Origin Access Control).
- **Distribution:** Amazon CloudFront (CDN) with AWS WAF for perimeter security.
- **Authentication:** Lambda@Edge verifying JWTs from Amazon Cognito.
- **Identity:** Federated access via SAML 2.0 and OIDC.
- **Automation:** GitHub Actions for automated build $\rightarrow$ sync $\rightarrow$ invalidate flow.

---
*This project was designed and implemented as a proof-of-concept for delivering agentic AI content within a regulated corporate environment.*
