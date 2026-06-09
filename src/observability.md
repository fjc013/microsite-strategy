---
layout: layout.njk
title: Observability Strategy
---

# Observability Strategy

Maintaining a "painless" operation requires proactive monitoring. Because the stack is serverless, we rely on integrated AWS logs rather than agent-based monitoring.

## 1. Traffic and Performance Monitoring
**Amazon CloudFront** provides the primary lens into user experience:
- **Standard Metrics:** Monitor 4xx and 5xx error rates to detect broken links or deployment failures.
- **Real-time Logs:** CloudFront access logs are delivered to an S3 bucket for deep analysis of traffic patterns and origin latency.

## 2. Security and Audit Trails
Given the regulated nature of the business, auditing *who* accessed *what* is critical.
- **AWS CloudTrail:** Logs every API call made to S3 and Cognito. This provides a definitive audit trail of when content was updated and who modified the infrastructure.
- **Cognito User Logs:** Tracks authentication attempts, password resets, and federation events from F5/PingOne.

## 3. Deployment Health
We integrate observability into the CI/CD pipeline:
- **GitHub Actions Logs:** Track build success/failure and S3 sync durations.
- **CloudWatch Alarms:** Set up alerts for spikes in 403 (Forbidden) errors, which may indicate an issue with the Cognito authentication guard or expired certificates.

## 4. Observability Stack Summary
| Metric | Tool | Purpose |
| :--- | :--- | :--- |
| **Request Volume** | CloudFront Metrics | Capacity planning and usage trends |
| **Auth Failures** | Cognito Logs | Troubleshooting SSO/SAML issues |
| **Data Integrity** | S3 Versioning | Recovering from accidental deletions |
| **Compliance** | AWS CloudTrail | Regulatory audit requirements |
