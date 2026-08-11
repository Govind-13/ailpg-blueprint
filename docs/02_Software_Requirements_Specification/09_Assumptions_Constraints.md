---
document_id: SRS-009
title: Assumptions, Constraints & Dependencies
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Product & Solution Architecture Team
parent_document: SRS-008
last_updated: 2026-08-11
---

# Assumptions, Constraints & Dependencies

## 1. Purpose

This document defines the assumptions, constraints, dependencies, risks, and boundaries that affect the AILPG platform.

The purpose is to prevent unclear requirements during development.

---

# 2. Project Scope Boundary

AILPG is designed around this core workflow:

```text
MP4 Upload
    ↓
Video Validation
    ↓
AI Analysis
    ↓
Speech-to-Text
    ↓
OCR
    ↓
Math Formula Detection
    ↓
Topic / Concept Detection
    ↓
Quiz Generation
    ↓
Translation
    ↓
Interactive Lesson Generation
    ↓
Teacher Review
    ↓
Publish
    ↓
Student Learning
    ↓
Analytics
```

---

# 3. Core Assumptions

## A-001 — MP4 Input

The primary source input is assumed to be an MP4 educational video.

The system may support additional formats in future versions.

---

## A-002 — Educational Content

The primary target content is educational video.

Initial focus:

```text
Mathematics
```

Future subjects may include:

```text
Science
Physics
Chemistry
Biology
Computer Science
Social Science
Languages
```

---

## A-003 — Human Review

AI-generated content is not assumed to be 100% accurate.

Therefore:

```text
AI Generation
     ↓
Teacher Review
     ↓
Publish
```

is part of the core architecture.

---

## A-004 — AI Is Probabilistic

AI output may vary.

The system must support:

- Confidence scores
- Review flags
- Manual editing
- Versioning
- Retry
- Human approval

---

# 4. AI Accuracy Assumption

The platform must not assume perfect:

```text
Speech recognition
OCR
Formula recognition
Translation
Question generation
```

Accuracy thresholds should be configurable.

Example:

```text
Confidence >= threshold
      ↓
Auto Approve Candidate

Confidence < threshold
      ↓
Review Required
```

The actual thresholds must be validated during testing.

---

# 5. Video Quality Assumptions

Input videos may vary in:

```text
Resolution
Bitrate
Frame rate
Audio quality
Background noise
Lighting
Text clarity
Speaker speed
```

The processing pipeline must be tolerant of reasonable variation.

---

# 6. Audio Assumptions

Videos may contain:

- Teacher narration
- Background noise
- Multiple speakers
- Silence
- Music
- Classroom discussion

Speech-to-text quality will depend on source audio quality.

---

# 7. OCR Assumptions

Mathematical OCR depends on:

```text
Font
Handwriting
Camera angle
Resolution
Contrast
Motion blur
Lighting
```

Handwritten mathematical content may require human verification.

---

# 8. Formula Recognition Constraint

Formula recognition cannot be treated as guaranteed.

Example:

```text
Video Frame
     ↓
AI detects formula
     ↓
Confidence score
     ↓
Teacher validation
```

The platform must allow manual correction.

---

# 9. Translation Assumption

Translation quality depends on:

```text
Source language
Technical vocabulary
Subject terminology
Sentence structure
AI provider
```

The original source content must always remain available.

---

# 10. Indian Language Requirement

The platform should be designed for Indian educational use.

Initial localization target:

```text
English
Tamil
Hindi
Malayalam
Telugu
Kannada
```

Additional Indian languages may be added later.

The architecture must support Unicode throughout the system.

---

# 11. Localization Constraint

No UI component should assume English-only text.

Avoid:

```text
Fixed-width text containers
Hard-coded English strings
English-only validation messages
```

The UI must support text expansion and different scripts.

---

# 12. Student Device Assumption

Students may access the platform through:

```text
Desktop
Laptop
Tablet
Mobile
```

The web application must therefore be responsive.

---

# 13. Network Assumption

Internet connectivity may vary.

Students may experience:

```text
High latency
Low bandwidth
Temporary disconnection
Mobile network switching
```

The platform should handle temporary network failures gracefully.

---

# 14. Video Streaming Constraint

Large video files should not be delivered directly from the application server.

Recommended:

```text
Object Storage
      ↓
CDN
      ↓
Student
```

Adaptive streaming should be considered for production.

---

# 15. Storage Constraint

Video files can consume significant storage.

Storage architecture must support:

```text
Original Video
Processed Video
Multiple Qualities
Audio
Thumbnails
Frames
Generated Assets
```

Lifecycle policies should be implemented.

---

# 16. Processing Time Constraint

AI processing may take longer than a normal HTTP request.

Therefore:

```text
Upload API
   ↓
Create Job
   ↓
Background Queue
   ↓
AI Workers
```

The upload API must not wait for the complete AI pipeline.

---

# 17. Asynchronous Processing Constraint

The following operations must be asynchronous:

```text
Video transcoding
Speech-to-text
OCR
Formula extraction
Translation
Quiz generation
Lesson generation
Large file processing
```

---

# 18. Third-Party AI Dependency

AILPG may depend on external AI services for:

```text
Speech-to-text
LLM processing
Translation
OCR
Vision
Formula recognition
```

Therefore external provider failures must be expected.

---

# 19. AI Provider Abstraction

The application should avoid tightly coupling business logic to one AI provider.

Recommended:

```text
AI Service Interface
        │
   ┌────┼────┐
   ▼    ▼    ▼
Provider A
Provider B
Provider C
```

This allows provider replacement or fallback.

---

# 20. Cloud Dependency

The production system will require cloud infrastructure or equivalent server infrastructure for:

```text
Application
Database
Object Storage
Queue
Workers
CDN
Monitoring
```

The exact cloud provider is a deployment decision.

---

# 21. Database Constraint

The primary transactional database should support:

```text
ACID transactions
Foreign keys
Indexes
Migrations
Backups
Point-in-time recovery
```

PostgreSQL is the recommended initial choice.

---

# 22. Cache Dependency

Redis may be used for:

```text
Caching
Rate limiting
Temporary state
Job coordination
Session-related data
```

Redis should not be treated as the permanent source of truth for critical business data.

---

# 23. Queue Dependency

Background processing requires a reliable queue.

Example:

```text
Video Upload
    ↓
Job Queue
    ↓
AI Worker
```

The queue should support:

- Retry
- Visibility timeout
- Dead-letter handling
- Job status
- Monitoring

---

# 24. Security Constraints

The system must enforce:

```text
Authentication
Authorization
Tenant isolation
Secure media access
Encryption
Audit logging
Secret management
```

---

# 25. Tenant Isolation

If multiple organizations use the platform:

```text
Organization A
      │
      ├── Users
      ├── Courses
      └── Videos

Organization B
      │
      ├── Users
      ├── Courses
      └── Videos
```

Organization A must never access Organization B's private resources.

---

# 26. Subscription Constraint

Premium features may depend on subscription entitlements.

Example:

```text
Free
 ├── Basic quality
 └── Limited content

Premium
 ├── Higher quality
 ├── Advanced translation
 └── Additional features
```

Exact plans are a business configuration.

---

# 27. Authorization Constraint

Frontend visibility is not sufficient for security.

Example:

```text
Frontend hides 1080p
```

is not security.

The backend must verify:

```text
User
+
Subscription
+
Lesson
+
Entitlement
```

before granting access.

---

# 28. Content Publishing Constraint

AI-generated content should not automatically become public by default.

Recommended lifecycle:

```text
DRAFT
  ↓
AI_GENERATED
  ↓
REVIEW
  ↓
APPROVED
  ↓
PUBLISHED
```

---

# 29. Versioning Constraint

Published lesson versions should remain immutable.

If a teacher edits a published lesson:

```text
Published v1
     ↓
Edit
     ↓
Draft v2
     ↓
Review
     ↓
Publish v2
```

Existing analytics must retain version context.

---

# 30. Data Privacy Constraint

The system should collect only data required for:

```text
Authentication
Learning
Analytics
Security
Billing
Platform operation
```

Unnecessary personal information should not be collected.

---

# 31. Analytics Constraint

Analytics should not compromise student privacy.

Data should be collected according to:

```text
Purpose
Retention
Access Control
Privacy Requirements
```

---

# 32. File Security Constraint

Uploaded files must be treated as untrusted input.

Validation should include:

```text
Extension
MIME type
File signature
Size
Codec
Decode capability
Malware/security scanning where required
```

---

# 33. Upload Security

Do not trust:

```text
Filename
Extension
Client MIME type
Client metadata
```

The backend must independently validate the file.

---

# 34. Performance Constraints

The system must remain responsive even while large AI jobs are running.

Heavy workloads should execute in background workers.

---

# 35. Scalability Constraint

The architecture should allow independent scaling of:

```text
API Servers
AI Workers
Video Workers
Queue Consumers
Database Read Capacity
Storage
CDN
```

---

# 36. Cost Constraint

AI and video processing may become the largest infrastructure costs.

The system should track:

```text
Video processing cost
AI token usage
Translation usage
Storage usage
Bandwidth
Worker time
```

This data will support:

```text
Cost monitoring
Subscription pricing
Usage limits
Optimization
```

---

# 37. Browser Constraint

The system depends on browser capabilities for:

```text
Video playback
Fullscreen
Audio
WebSocket/SSE
File upload
Responsive UI
```

Unsupported browser behavior must have graceful fallbacks.

---

# 38. Accessibility Constraint

Accessibility should be considered from the beginning rather than added after development.

Required areas:

```text
Keyboard navigation
Captions
Contrast
Focus states
Screen readers
Text scaling
Accessible forms
```

---

# 39. Content Quality Constraint

AI-generated lessons must have quality checks.

At minimum:

```text
Transcript validation
Formula validation
Question validation
Translation review
Lesson structure validation
```

---

# 40. Mathematical Content Constraint

For mathematics:

```text
AI output
    ↓
Mathematical validation
    ↓
Teacher review
```

Where possible, computational validation should be used for generated mathematical answers.

---

# 41. Dependency Failure Assumption

Any external service may become:

```text
Slow
Unavailable
Rate-limited
Incorrect
Deprecated
Changed
```

Therefore integration layers must isolate third-party dependencies.

---

# 42. API Versioning Constraint

APIs should be versioned.

Example:

```text
/api/v1/
```

Future breaking changes should use:

```text
/api/v2/
```

---

# 43. Backward Compatibility

Published lessons should remain playable after backend updates whenever technically feasible.

Lesson manifests should contain sufficient version information.

---

# 44. Deployment Constraint

Production deployment must support:

```text
Environment separation
Development
Staging
Production
```

Configuration must not be hard-coded.

---

# 45. Environment Separation

```text
Development
    ↓
Staging
    ↓
Production
```

Production credentials must never be used in development.

---

# 46. Configuration Management

Environment-specific values should be externalized.

Examples:

```text
DATABASE_URL
REDIS_URL
STORAGE_BUCKET
AI_PROVIDER_KEY
JWT_SECRET
PAYMENT_PROVIDER_KEY
```

Secrets must be stored securely.

---

# 47. Backup Constraint

Critical data requires automated backup.

At minimum:

```text
Database
Lesson metadata
Published lesson manifests
Important object storage assets
```

Backup restoration must be tested.

---

# 48. Disaster Recovery Constraint

The system should define:

```text
RPO — Recovery Point Objective
RTO — Recovery Time Objective
```

Final values should be selected based on business requirements and infrastructure cost.

---

# 49. Development Team Assumptions

The project requires expertise across:

```text
Frontend
Backend
Database
Cloud
DevOps
AI/ML
Video Processing
UI/UX
QA
Security
```

A small team may combine roles during MVP development.

---

# 50. MVP Constraints

The MVP should focus on:

```text
MP4 Upload
       ↓
AI Analysis
       ↓
Transcript
       ↓
Basic OCR
       ↓
Formula Detection
       ↓
Quiz Generation
       ↓
Translation
       ↓
Interactive HTML Lesson
       ↓
Teacher Review
       ↓
Student Player
```

Advanced features should not block the first usable release.

---

# 51. Out of Scope for Initial MVP

Potentially deferred:

```text
Native mobile apps
Advanced adaptive learning
AI tutor
Offline video
Complex recommendation engine
Advanced competency graph
Multi-region deployment
Enterprise SSO
Advanced proctoring
```

These may be added later.

---

# 52. Key Dependencies

| Dependency | Purpose |
|---|---|
| PostgreSQL | Transactional data |
| Object Storage | Video/assets |
| Redis | Cache/temporary state |
| Queue | Background processing |
| AI Provider | AI processing |
| STT Provider | Speech recognition |
| OCR Provider | Text extraction |
| Translation Provider | Localization |
| CDN | Video delivery |
| Payment Provider | Subscription billing |
| Email/SMS Provider | Notifications |
| Monitoring | Observability |

---

# 53. Key Risks

| Risk | Impact | Mitigation |
|---|---|---|
| AI inaccurate output | High | Human review |
| AI provider outage | High | Retry/fallback |
| Video processing cost | High | Usage controls |
| Large storage growth | High | Lifecycle policies |
| Poor OCR | Medium/High | Confidence + review |
| Translation errors | Medium | Review workflow |
| Network instability | Medium | Resume/retry |
| Security breach | Critical | Defense in depth |
| Database failure | Critical | Backup/DR |
| Vendor lock-in | Medium | Provider abstraction |

---

# 54. Requirement Change Management

New requirements must be evaluated for:

```text
Scope
Cost
Timeline
Architecture
Database
API
UI
Security
Testing
Deployment
```

Major changes require versioned documentation updates.

---

# 55. Assumption Validation

Each major assumption should eventually be validated through:

```text
Prototype
Proof of Concept
Performance Test
AI Accuracy Test
User Test
Security Test
```

---

# 56. Definition of Done

This document is complete when:

- Assumptions are documented.
- Constraints are documented.
- Dependencies are identified.
- Major risks are identified.
- MVP boundaries are defined.
- Future scope is separated.
- AI limitations are acknowledged.
- Indian language requirements are captured.
- Security boundaries are documented.
- Deployment constraints are documented.

---

# 57. Related Documents

- SRS-001 — SRS Overview
- SRS-005 — System Features
- SRS-006 — Interface Requirements
- SRS-007 — Data Requirements
- SRS-008 — Error Handling
- Technical Architecture
- Database Design
- API Design
- AI Workflow
- Deployment Plan
- Security Architecture

---

# 58. Revision History

| Version | Date | Description |
|---|---|---|
| 1.0.0 | 2026-08-11 | Initial Assumptions, Constraints & Dependencies |
