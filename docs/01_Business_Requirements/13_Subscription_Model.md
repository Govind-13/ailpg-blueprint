---
document_id: BRD-013
title: Subscription Model
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Product & Business Team
parent_document: 12_Analytics_Requirements.md
last_updated: 2026-07-22
---

# Subscription Model

> This document defines subscription plans, licensing, usage limits, feature entitlements, and billing policies for the AILPG platform.

---

# Table of Contents

1. Purpose
2. Subscription Strategy
3. Plans
4. Feature Matrix
5. AI Usage Limits
6. Storage Limits
7. Video Streaming
8. Billing Rules
9. Organization Licensing
10. Trial Plan
11. Add-ons
12. Upgrade / Downgrade
13. Cancellation
14. Related Documents
15. Revision History

---

# 1. Purpose

The subscription model defines how users and organizations access platform features while supporting sustainable platform operations.

---

# 2. Subscription Strategy

Supported customer types:

- Individual Teacher
- School
- College
- Coaching Center
- Corporate Training
- Enterprise

Plans may be offered monthly or annually.

---

# 3. Plans

## Free

Suitable for evaluation.

Features:

- Limited course creation
- Limited AI processing
- Standard video streaming
- Basic analytics
- Community support

---

## Basic

Suitable for individual educators.

Features:

- Increased AI processing
- Interactive lessons
- Multi-language support
- Standard reports
- Email support

---

## Professional

Suitable for institutions.

Features:

- Priority AI queue
- Advanced analytics
- Team collaboration
- Institution dashboard
- API access (optional)

---

## Enterprise

Suitable for large organizations.

Features:

- Unlimited organizations (contract-dependent)
- Dedicated onboarding
- SSO integration
- Advanced security options
- SLA support
- Custom branding
- Priority support

---

# 4. Feature Matrix

| Feature | Free | Basic | Professional | Enterprise |
|---------|:----:|:-----:|:------------:|:----------:|
| MP4 Upload | ✓ | ✓ | ✓ | ✓ |
| AI Lesson Generation | Limited | ✓ | ✓ | ✓ |
| Interactive HTML | Limited | ✓ | ✓ | ✓ |
| Translation | Limited | ✓ | ✓ | ✓ |
| Quiz Generation | ✓ | ✓ | ✓ | ✓ |
| Analytics | Basic | Standard | Advanced | Advanced |
| Team Management | ✗ | ✗ | ✓ | ✓ |
| API Access | ✗ | Optional | ✓ | ✓ |
| Custom Branding | ✗ | ✗ | Optional | ✓ |
| SSO | ✗ | ✗ | Optional | ✓ |

---

# 5. AI Usage Limits

Plans may define configurable limits for:

- AI processing jobs
- Concurrent AI jobs
- Translation requests
- Regeneration requests
- OCR processing
- Formula recognition

Limits should be configurable through administration settings.

---

# 6. Storage Limits

Storage quotas may include:

- Video storage
- Generated HTML
- Images
- AI artifacts
- Reports

Organizations may purchase additional storage if supported.

---

# 7. Video Streaming

Streaming capabilities:

- Adaptive streaming
- Resume playback
- Playback speed control
- Subtitle support

Higher video quality and bandwidth optimizations may be available based on subscription level and network conditions.

---

# 8. Billing Rules

Support:

- Monthly billing
- Annual billing
- Automatic renewal (optional)
- Manual renewal
- Invoice generation
- Tax calculation (deployment-specific)

Payment gateway integrations are defined separately.

---

# 9. Organization Licensing

Institution plans may include:

- Teacher licenses
- Student licenses
- Reviewer accounts
- Administrator accounts

License allocation is managed by Institution Administrators.

---

# 10. Trial Plan

Optional trial capabilities:

- Limited duration
- Limited AI processing
- Limited storage
- Sample courses
- Upgrade reminders

---

# 11. Add-ons

Optional add-ons may include:

- Additional AI credits
- Additional storage
- Premium support
- Custom domain
- Dedicated AI capacity
- Advanced reporting

---

# 12. Upgrade / Downgrade

The platform shall support:

- Plan upgrades
- Plan downgrades
- Renewal changes
- License expansion

Changes should preserve user data wherever possible.

---

# 13. Cancellation

Cancellation policy should define:

- End-of-term access
- Data retention period
- Export options
- Reactivation process

Policy details may vary by deployment or contract.

---

# 14. Related Documents

- 09_Functional_Requirements.md
- 11_Reporting_Requirements.md
- Billing_Architecture.md (future)
- API_Design.md (future)

---

# 15. Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | 2026-07-22 | Initial Subscription Model |
