---
document_id: SD-001
title: System Design Overview
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Solution Architecture Team
parent_layer: SRS
last_updated: 2026-08-11
---

# System Design Overview

## 1. Purpose

This document defines the high-level system design of the AILPG platform.

AILPG converts an uploaded educational MP4 video into an interactive, multilingual, AI-assisted learning lesson.

The system must support:

- MP4 upload
- Video validation
- Video processing
- Speech-to-text
- OCR
- Mathematical formula extraction
- Topic detection
- AI-generated questions
- Translation
- Interactive lesson generation
- Teacher review
- Lesson publishing
- Student learning
- Progress tracking
- Analytics
- Subscription management
- Platform administration

---

# 2. System Goal

The primary system goal is:

```text
MP4
 ↓
AI Analysis
 ↓
Structured Learning Content
 ↓
Interactive HTML Lesson
 ↓
Teacher Review
 ↓
Publish
 ↓
Student Learning
```

---

# 3. High-Level System

```text
                         AILPG PLATFORM
                              │
       ┌──────────────────────┼──────────────────────┐
       │                      │                      │
       ▼                      ▼                      ▼
  Teacher Portal        Student Portal         Admin Portal
       │                      │                      │
       └──────────────────────┼──────────────────────┘
                              │
                              ▼
                         API Gateway
                              │
              ┌───────────────┼───────────────┐
              │               │               │
              ▼               ▼               ▼
        Auth Service     Content Service   Learning Service
              │               │               │
              └───────────────┼───────────────┘
                              │
                              ▼
                         Job System
                              │
              ┌───────────────┼────────────────┐
              │               │                │
              ▼               ▼                ▼
        Video Workers     AI Workers      Translation
              │               │                │
              └───────────────┼────────────────┘
                              │
                              ▼
                    Content Generation
                              │
                 ┌────────────┼────────────┐
                 ▼            ▼            ▼
             PostgreSQL   Object Storage   Redis
                 │
                 ▼
              Analytics
```

---

# 4. Architectural Style

The initial platform should use a:

```text
Modular Backend + Asynchronous Worker Architecture
```

rather than immediately starting with many independent microservices.

Recommended approach:

```text
Frontend
   ↓
API / Modular Backend
   ↓
Queue
   ↓
Specialized Workers
```

This keeps MVP development simpler while allowing future service separation.

---

# 5. Major System Components

```text
1. Web Frontend
2. API Layer
3. Authentication
4. User Management
5. Organization Management
6. Course Management
7. Video Management
8. AI Job Orchestrator
9. Video Processing Workers
10. AI Workers
11. Translation Worker
12. Lesson Generator
13. Review System
14. Learning Engine
15. Analytics
16. Subscription
17. Notification
18. Admin System
19. Database
20. Object Storage
21. Cache
22. Queue
23. CDN
24. Monitoring
```

---

# 6. Frontend Architecture

The frontend contains three major portals.

```text
Frontend
│
├── Student Portal
│
├── Teacher Portal
│
└── Admin Portal
```

The UI should reuse a shared design system.

```text
Shared UI
├── Buttons
├── Forms
├── Dialogs
├── Tables
├── Video Player
├── Charts
├── Notifications
└── Layouts
```

---

# 7. Teacher Workflow

```text
Teacher Login
      ↓
Teacher Dashboard
      ↓
Create Course
      ↓
Create Lesson
      ↓
Upload MP4
      ↓
Upload Complete
      ↓
AI Job Created
      ↓
Processing Dashboard
      ↓
AI Analysis
      ↓
Review Workspace
      ↓
Edit / Approve
      ↓
Preview
      ↓
Publish
```

---

# 8. Student Workflow

```text
Student Login
      ↓
Dashboard
      ↓
Course
      ↓
Lesson
      ↓
Interactive Video
      ↓
Checkpoint
      ↓
Question
      ↓
Answer
      ↓
Feedback
      ↓
Continue Video
      ↓
Lesson Complete
      ↓
Progress Update
```

---

# 9. Admin Workflow

```text
Admin Login
      ↓
Admin Dashboard
      ↓
System Overview
      ↓
Users / Organizations
      ↓
AI Jobs
      ↓
Failed Jobs
      ↓
Storage
      ↓
Subscriptions
      ↓
Audit Logs
      ↓
System Health
```

---

# 10. MP4 Processing Architecture

The core pipeline:

```text
                  MP4
                   │
                   ▼
            File Validation
                   │
                   ▼
             Video Metadata
                   │
                   ▼
           Audio Extraction
                   │
                   ▼
        ┌──────────┴──────────┐
        │                     │
        ▼                     ▼
       STT                    Frames
        │                     │
        ▼                     ▼
   Transcript                OCR
                              │
                              ▼
                         Formula AI
        │                     │
        └──────────┬──────────┘
                   ▼
              Segmentation
                   │
                   ▼
             Topic Detection
                   │
                   ▼
            Quiz Generation
                   │
                   ▼
              Translation
                   │
                   ▼
         Interactive Lesson
                   │
                   ▼
             Teacher Review
                   │
                   ▼
                Publish
```

---

# 11. Asynchronous Processing

The AI pipeline must not run inside the upload HTTP request.

Incorrect:

```text
POST /upload
     ↓
Process entire video
     ↓
Return response
```

Correct:

```text
POST /upload
     ↓
Create upload
     ↓
Return job ID
     ↓
Background Queue
     ↓
Workers
```

---

# 12. Job Orchestration

The system should maintain a central job state.

Example:

```text
JOB-001

UPLOAD
  ✓

VALIDATION
  ✓

AUDIO
  ✓

STT
  ✓

OCR
  ✓

FORMULA
  processing

QUIZ
  pending

TRANSLATION
  pending

LESSON
  pending
```

---

# 13. Worker Architecture

Recommended workers:

```text
Worker System
│
├── Video Worker
│
├── Audio Worker
│
├── STT Worker
│
├── OCR Worker
│
├── Formula Worker
│
├── Quiz Worker
│
├── Translation Worker
│
└── Lesson Generator Worker
```

Workers can initially be implemented as modules/processes and later separated into independently scalable services.

---

# 14. Queue Architecture

```text
API
 │
 ▼
Job Queue
 │
 ├── video-processing
 ├── transcription
 ├── ocr
 ├── formula
 ├── quiz
 ├── translation
 └── lesson-generation
```

Each queue can have different:

```text
Priority
Retry policy
Concurrency
Timeout
Worker type
```

---

# 15. Data Architecture

Primary transactional data:

```text
PostgreSQL
```

Example domains:

```text
Identity
Courses
Lessons
Videos
AI Jobs
Questions
Translations
Progress
Subscriptions
Audit
```

---

# 16. Object Storage Architecture

Object storage contains:

```text
videos/
├── original/
├── processed/
├── hls/
├── thumbnails/
└── frames/

lessons/
├── manifests/
├── assets/
└── exports/

ai/
├── audio/
├── ocr/
└── temporary/
```

Actual storage key structure may be refined during implementation.

---

# 17. Media Delivery

Production architecture:

```text
Object Storage
      ↓
     CDN
      ↓
Student Browser
```

The application server should not stream every video byte itself.

---

# 18. Secure Media Access

```text
Student
   ↓
Request Lesson
   ↓
Backend
   ↓
Authentication
   ↓
Authorization
   ↓
Subscription Check
   ↓
Generate Signed URL
   ↓
CDN
   ↓
Video
```

---

# 19. Database + Cache

```text
             Application
                  │
          ┌───────┴───────┐
          ▼               ▼
      PostgreSQL        Redis
          │               │
       Source           Cache
       of Truth
```

Redis failure must not cause permanent loss of critical business data.

---

# 20. Authentication Architecture

```text
User
 ↓
Login
 ↓
Authentication Service
 ↓
Access Token / Session
 ↓
Frontend
 ↓
API
 ↓
Authorization
```

Roles:

```text
STUDENT
TEACHER
REVIEWER
ORG_ADMIN
PLATFORM_ADMIN
SUPER_ADMIN
```

---

# 21. Multi-Tenant Architecture

AILPG should support multiple organizations.

```text
                 Platform
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
     Org A        Org B        Org C
       │            │            │
    Courses       Courses      Courses
    Users         Users        Users
    Videos        Videos       Videos
```

Every tenant-owned record should contain an organization reference where appropriate.

---

# 22. AI Abstraction Layer

The application should not directly couple business logic to a single AI provider.

```text
                AI Gateway
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
       STT         LLM         Vision
        │           │           │
   Provider A   Provider A   Provider B
   Provider B   Provider B   Provider C
```

This supports provider replacement.

---

# 23. AI Result Pipeline

```text
Raw Video
    ↓
Raw AI Output
    ↓
Normalized AI Output
    ↓
Validation
    ↓
Confidence Scoring
    ↓
Human Review
    ↓
Approved Content
```

AI provider-specific formats should be converted into internal platform schemas.

---

# 24. Lesson Generation Architecture

The lesson generator consumes:

```text
Video Metadata
Transcript
OCR
Formulas
Topics
Questions
Translations
```

and generates:

```text
Lesson Manifest
+
Interactive Checkpoints
+
Content Assets
```

---

# 25. Lesson Manifest

Conceptual structure:

```json
{
  "lesson": {
    "id": "lesson_001",
    "version": 1,
    "title": "Quadratic Equations"
  },
  "video": {},
  "transcript": [],
  "topics": [],
  "formulas": [],
  "checkpoints": [],
  "questions": [],
  "translations": []
}
```

The final schema will be defined separately.

---

# 26. Interactive Player Architecture

```text
Lesson Manifest
      │
      ▼
Interactive Player
      │
      ├── Video
      ├── Transcript
      ├── Checkpoints
      ├── Questions
      ├── Feedback
      ├── Translation
      └── Progress
```

---

# 27. Checkpoint Engine

```text
Video Current Time
       ↓
Checkpoint Engine
       ↓
Is checkpoint reached?
       │
    ┌──┴──┐
    │     │
   No    Yes
    │     │
 Continue Pause
          │
          ▼
       Question
          │
          ▼
       Answer
          │
          ▼
       Feedback
          │
          ▼
       Resume
```

---

# 28. Learning Progress Architecture

```text
Video Player
     ↓
Learning Events
     ↓
Progress Service
     ↓
PostgreSQL
     ↓
Analytics Pipeline
```

Events may include:

```text
PLAY
PAUSE
SEEK
CHECKPOINT_REACHED
QUESTION_ANSWERED
LESSON_COMPLETED
```

---

# 29. Analytics Architecture

For MVP:

```text
Application
     ↓
Learning Events
     ↓
PostgreSQL
     ↓
Analytics Queries
     ↓
Dashboard
```

At larger scale:

```text
Application
     ↓
Event Queue
     ↓
Analytics Pipeline
     ↓
Analytics Database / Warehouse
     ↓
Dashboards
```

---

# 30. Subscription Architecture

```text
User
 ↓
Subscription
 ↓
Plan
 ↓
Entitlements
 ↓
Access Check
 ↓
Feature / Content
```

Example:

```text
1080p
Translation
Download
Advanced Analytics
```

---

# 31. Notification Architecture

```text
System Event
     ↓
Notification Service
     ↓
┌────┼─────┐
▼    ▼     ▼
Email In-App Push
```

Examples:

```text
AI processing completed
Lesson requires review
Lesson published
Subscription expiring
Payment failed
```

---

# 32. Admin Monitoring Architecture

Admin dashboard receives aggregated information from:

```text
API Metrics
AI Jobs
Queue Metrics
Database Metrics
Storage Metrics
Application Logs
Audit Logs
```

---

# 33. Observability

The platform should implement:

```text
Logs
Metrics
Traces
Alerts
```

Flow:

```text
Request
 ↓
Request ID
 ↓
API
 ↓
Service
 ↓
Worker
 ↓
External Provider
```

The same correlation ID should be propagated where possible.

---

# 34. Security Architecture Overview

```text
Internet
   ↓
HTTPS
   ↓
CDN / WAF
   ↓
API Gateway
   ↓
Authentication
   ↓
Authorization
   ↓
Application
   ↓
Private Database
```

Storage access should be controlled through signed URLs and appropriate policies.

---

# 35. Network Architecture

Production should separate public and private resources.

```text
                 Internet
                    │
                    ▼
              Public Layer
                    │
             ┌──────┴──────┐
             ▼             ▼
            CDN          API
                           │
                    Private Network
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
             DB          Redis        Workers
```

---

# 36. Environment Architecture

```text
Development
     │
     ▼
Staging
     │
     ▼
Production
```

Each environment should have separate:

```text
Database
Storage
Secrets
AI credentials
Queues
Monitoring
```

---

# 37. Deployment Architecture

Initial production:

```text
                     Internet
                        │
                        ▼
                       CDN
                        │
                ┌───────┴───────┐
                ▼               ▼
             Frontend           API
                                │
                    ┌───────────┼───────────┐
                    ▼           ▼           ▼
                 Database     Redis       Queue
                                            │
                              ┌─────────────┼─────────────┐
                              ▼             ▼             ▼
                           Video        AI Workers    Translation
                           Workers
```

---

# 38. Scaling Strategy

## Scale API

```text
API Instance 1
API Instance 2
API Instance 3
```

behind a load balancer.

## Scale Workers

```text
AI Worker × N
Video Worker × N
```

according to queue depth.

---

# 39. Failure Recovery

```text
Worker Failure
     ↓
Job remains recoverable
     ↓
Queue Retry
     ↓
New Worker
     ↓
Resume Stage
```

Previously completed stages should not be unnecessarily repeated.

---

# 40. System Data Flow

```text
Teacher
  │
  ▼
Web UI
  │
  ▼
API
  │
  ├──────────────► PostgreSQL
  │
  └──────────────► Object Storage
                         │
                         ▼
                       Queue
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
             STT        OCR       Video
              │          │          │
              └──────────┼──────────┘
                         ▼
                      AI Data
                         │
                         ▼
                  Lesson Generator
                         │
                         ▼
                    Review UI
                         │
                         ▼
                      Publish
                         │
                         ▼
                     Student
```

---

# 41. Core Design Principle

The platform should separate:

```text
Content Creation
        from
Content Consumption
```

Teacher side:

```text
Upload
Analyze
Review
Edit
Publish
```

Student side:

```text
Discover
Learn
Interact
Answer
Track Progress
```

---

# 42. Core Processing Principle

The AI pipeline must be:

```text
Asynchronous
Resumable
Observable
Retryable
Versioned
Reviewable
```

---

# 43. Core Security Principle

Every protected request should conceptually pass through:

```text
Authentication
      ↓
Authorization
      ↓
Tenant Check
      ↓
Resource Check
      ↓
Entitlement Check
      ↓
Operation
```

---

# 44. Core Data Principle

Use:

```text
PostgreSQL
    ↓
Business Source of Truth

Object Storage
    ↓
Large Media / Assets

Redis
    ↓
Temporary / Cached Data

Queue
    ↓
Asynchronous Work
```

---

# 45. MVP Architecture

The first implementation should avoid unnecessary complexity.

```text
                    MVP
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
      Web UI     Modular API    Admin UI
                     │
             ┌───────┼───────┐
             ▼       ▼       ▼
           DB      Redis    Queue
                             │
                         AI Workers
                             │
                       Object Storage
```

---

# 46. Future Architecture

As scale increases:

```text
                    API Gateway
                         │
       ┌─────────────────┼─────────────────┐
       ▼                 ▼                 ▼
 Auth Service       Content Service   Learning Service
       │                 │                 │
       └─────────────────┼─────────────────┘
                         │
                     Event Bus
                         │
       ┌─────────────────┼─────────────────┐
       ▼                 ▼                 ▼
 Video Service       AI Platform      Analytics
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
             STT        OCR        LLM
```

Microservice separation should happen only when justified by scale or team boundaries.

---

# 47. Design Decision Summary

| Decision | Choice |
|---|---|
| Frontend | Web-first |
| Backend | Modular architecture |
| Database | PostgreSQL |
| Cache | Redis |
| Queue | Asynchronous job queue |
| Media | Object Storage |
| Delivery | CDN |
| AI | Provider abstraction |
| Processing | Background workers |
| Architecture | Modular + async |
| Multi-tenancy | Organization-based |
| Lesson | Versioned manifest |
| Video | Secure streaming |
| Analytics | Event-based |
| Deployment | Cloud-ready |

---

# 48. System Design Definition of Done

System Design is complete when:

- Major components are identified.
- System boundaries are defined.
- Data flow is defined.
- AI pipeline is defined.
- Worker architecture is defined.
- Storage architecture is defined.
- Security boundaries are defined.
- Multi-tenancy is defined.
- Deployment model is defined.
- Scaling approach is defined.
- Failure recovery is defined.
- MVP architecture is separated from future architecture.

---

# 49. Next System Design Documents

```text
docs/03_System_Design/

01_System_Design_Overview.md
02_System_Architecture.md
03_Component_Design.md
04_Data_Flow_Design.md
05_Authentication_Authorization_Design.md
06_Video_Processing_Design.md
07_AI_Pipeline_Design.md
08_Interactive_Lesson_Design.md
09_Analytics_Design.md
10_Security_Design.md
11_Scalability_Design.md
12_System_Design_Appendix.md
```

---

# 50. Revision History

| Version | Date | Description |
|---|---|---|
| 1.0.0 | 2026-08-11 | Initial System Design Overview |
