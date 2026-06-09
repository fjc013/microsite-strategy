---
layout: layout.njk
title: Services and Deployment
---

# Services and Deployment

## Infrastructure Components
The platform utilizes a purely serverless stack to minimize operational overhead and cost.

### 1. Amazon S3 (The Origin)
- **Configuration:** Private buckets with "Static Website Hosting" **disabled**.
- **Access:** Content is stored as static assets. S3 is configured to only allow access via **Origin Access Control (OAC)**, ensuring users cannot bypass CloudFront to access the raw bucket.
- **Scaling:** S3 natively handles 10s to 100s of buckets (one per microsite or a shared bucket with folder-level partitioning).

### 2. Amazon CloudFront (The Edge)
- **Distribution:** A single global distribution (or one per high-profile site) to cache content close to the user.
- **Security:** Integrated with AWS WAF to block malicious traffic and restrict access by geography or IP.
- **Headers:** Configured to forward authorization headers to the Lambda@Edge guard.

### 3. Deployment Pipeline (CI/CD)
To keep deployment "painless," we use GitHub Actions to automate the path from AI output to production.

```mermaid
graph LR
    Claude[Claude Code] --> Git[GitHub Repo]
    Git --> Action[GitHub Action]
    Action --> Build[Eleventy Build]
    Build --> S3Sync[S3 Sync]
    S3Sync --> Inv[CloudFront Invalidation]
    Inv --> Live[Production Site]
```

**Deployment Steps:**
1.  **Push:** Developer pushes Markdown changes to GitHub.
2.  **Build:** GitHub Action triggers an Eleventy build, generating the `_site` folder.
3.  **Sync:** The resulting HTML/CSS is synced to the target S3 bucket.
4.  **Purge:** A CloudFront invalidation is triggered to clear the cache for the updated files.

## Cost Optimization
- **Zero Idle Cost:** You only pay for the storage used in S3 and the data transferred via CloudFront.
- **No Server Management:** No EC2 instances, no patching, no OS scaling.
- **Shared Infrastructure:** Multiple microsites can share a single CloudFront distribution using path-based routing (e.g., `cdn.company.com/site-a/`).
