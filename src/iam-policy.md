---
layout: layout.njk
title: IAM and Policy Considerations
---

# IAM and Policy Considerations

As a financial institution, the principle of **Least Privilege** is non-negotiable. The architecture implements several layers of policy enforcement.

## 1. S3 Bucket Policy (Origin Security)
The S3 buckets are completely private. We use **Origin Access Control (OAC)** to ensure that only the specific CloudFront distribution can read the objects.

**Example Policy Logic:**
- `Allow` $\rightarrow$ `s3:GetObject`
- `Condition` $\rightarrow$ `StringEquals` $\rightarrow$ `AWS:SourceArn` $\rightarrow$ `arn:aws:cloudfront::ACCOUNT:distribution/DIST_ID`

## 2. CI/CD IAM Role
The GitHub Action does not use long-term credentials. Instead, it uses **OIDC (OpenID Connect)** to assume a short-lived IAM role in AWS.

**Permissions granted to the Deployment Role:**
- `s3:PutObject` (To upload the built site)
- `s3:ListBucket` (To sync files)
- `cloudfront:CreateInvalidation` (To clear the cache)

## 3. Cognito Identity Policies
Cognito provides fine-grained control over who can access which site.
- **User Pool Groups:** We define groups (e.g., `Internal-Employees`, `External-Clients`).
- **Attribute-Based Access Control (ABAC):** The Lambda@Edge guard checks the user's identity claims in the JWT. If a user lacks the required `group` attribute for a specific microsite, access is denied with a `403 Forbidden`.

## 4. Organization-Level Guardrails
To ensure compliance across the account:
- **Service Control Policies (SCPs):** Prevent the accidental creation of public S3 buckets across the entire organization.
- **Encryption:** All S3 buckets are encrypted at rest using AWS KMS (Customer Managed Keys) to meet financial data protection standards.
