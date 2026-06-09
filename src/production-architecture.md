---
layout: layout.njk
title: Production Architecture
---

# Production Architecture: Enterprise Microsite Platform

## Executive Summary
This document outlines the architectural transition from local development to a production-ready, enterprise-grade hosting platform. The objective is to provide a scalable, secure, and cost-effective environment capable of hosting hundreds of independent microsites while adhering to the strict regulatory requirements of a financial institution.

### The Core Philosophy
To maintain the tenets of **painless, secure, and cost-effective**, we decouple the content delivery layer from the rigid, RAM-shared VPC workload pattern. By leveraging a serverless "Edge-First" architecture, we remove the overhead of managing virtual networks for simple static content while maintaining rigorous identity-based access control.

### High-Level Architecture
The solution utilizes a "Hub-and-Spoke" identity model where **Amazon Cognito** acts as the central authentication orchestrator, bridging the gap between internal corporate identity and external client identity.

```mermaid
graph TD
    UserEx[External User] --> CF[Amazon CloudFront]
    UserInt[Internal User] --> CF
    CF --> AuthEdge{Lambda@Edge / CloudFront Function}
    AuthEdge -- No Token --> Cog[Amazon Cognito]
    Cog -- SAML --> F5[F5 APM / Active Directory]
    Cog -- OIDC --> Ping[PingOne]
    AuthEdge -- Valid Token --> S3[Amazon S3 Private Bucket]
    S3 --> CF
    CF --> UserEx
    CF --> UserInt
```

### Key Design Decisions
| Feature | Decision | Rationale |
| :--- | :--- | :--- |
| **Hosting** | S3 + CloudFront (OAC) | Maximum cost-efficiency and zero-maintenance serverless footprint. |
| **Auth Hub** | Amazon Cognito | Provides a unified interface for multiple IdPs (F5, PingOne) without custom auth code. |
| **Network** | Edge-Based Access | Bypasses VPC complexity; internal traffic flows naturally via Direct Connect/TGW to the public AWS edge. |
| **Security** | Identity-Gated Content | Prevents "link sharing" by requiring a valid session token for every request. |

---

### Navigation
- [Authentication Strategy](/auth-strategy/)
- [Services and Deployment](/deployment-model/)
- [IAM and Policy Considerations](/iam-policy/)
- [Observability Strategy](/observability/)
