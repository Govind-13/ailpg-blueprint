---
document_id: SRS-002
title: System Context
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Solution Architecture Team
parent_document: SRS-001
last_updated: 2026-08-11
---

# System Context

## 1. Purpose

This document defines the high-level system context of the AILPG platform.

AILPG transforms an educational MP4 video into an AI-assisted, interactive, multilingual HTML learning experience.

The system consists of:

- Student Application
- Teacher Application
- Institution Administration
- Platform Administration
- Backend APIs
- AI Processing Pipeline
- Database
- Object Storage
- Video Processing
- Analytics
- Notification Services
- Optional Billing Services

---

# 2. System Context Overview

```text
                         ┌─────────────────────┐
                         │      STUDENT        │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │ Student Web/Mobile  │
                         │     Application     │
                         └──────────┬──────────┘
                                    │
                                    │
┌─────────────────────┐             ▼
│      TEACHER        │──────►┌───────────────┐
└─────────────────────┘       │               │
                              │  AILPG API    │
┌─────────────────────┐       │   Gateway     │
│ INSTITUTION ADMIN   │──────►│               │
└─────────────────────┘       └───────┬───────┘
                                      │
┌─────────────────────┐               │
│ PLATFORM ADMIN      │───────────────┘
└─────────────────────┘               │
                                      ▼
                              ┌───────────────┐
                              │ Application   │
                              │    Services   │
                              └───────┬───────┘
                                      │
              ┌───────────────────────┼──────────────────────┐
              │                       │                      │
              ▼                       ▼                      ▼
      ┌──────────────┐        ┌──────────────┐       ┌──────────────┐
      │ PostgreSQL   │        │    Redis     │       │ Object       │
      │  Database    │        │ Cache/Queue  │       │  Storage     │
      └──────────────┘        └──────────────┘       └──────┬───────┘
                                                            │
                                                            ▼
                                                   ┌────────────────┐
                                                   │ Video / AI     │
                                                   │ Processing     │
                                                   └───────┬────────┘
                                                           │
                                                           ▼
                                                   ┌────────────────┐
                                                   │ AI Services    │
                                                   │ STT / OCR /    │
                                                   │ Translation /  │
                                                   │ LLM / Formula  │
                                                   └────────────────┘
```

---

# 3. Primary Actors

## 3.1 Student

The Student consumes published learning content.

Primary activities:

- Login
- Browse courses
- Start lesson
- Watch video
- Answer interactive questions
- Change language
- Change video quality
- Change playback speed
- Resume learning
- View progress
- View certificates

---

## 3.2 Teacher

The Teacher creates and manages educational content.

Primary activities:

- Create course
- Upload MP4
- Monitor AI processing
- Review transcript
- Review OCR
- Review formulas
- Review translations
- Review generated quizzes
- Preview interactive HTML
- Edit content
- Publish lesson
- View analytics

---

## 3.3 Content Reviewer

The Reviewer validates AI-generated educational content.

Primary activities:

- Review transcript
- Validate formulas
- Validate translations
- Validate quizzes
- Approve content
- Request corrections

---

## 3.4 Institution Administrator

The Institution Administrator manages users and content within an organization.

Primary activities:

- Manage students
- Manage teachers
- Assign courses
- View organization analytics
- Manage licenses
- View subscription status

---

## 3.5 Platform Administrator

The Platform Administrator manages platform operations.

Primary activities:

- Monitor AI jobs
- Monitor system health
- Review errors
- Manage users
- Review audit logs
- Monitor storage
- Monitor API performance

---

## 3.6 Super Administrator

The Super Administrator controls global platform configuration.

Primary activities:

- Manage organizations
- Configure platform settings
- Configure AI providers
- Manage feature flags
- Manage subscription plans
- Review security events

---

# 4. Client Applications

## 4.1 Student Application

The Student Application provides:

- Interactive video player
- Quiz interface
- Course navigation
- Progress tracking
- Language selector
- Subtitle controls
- Video quality selector
- Playback speed selector
- Notes
- Bookmarks

---

## 4.2 Teacher Application

The Teacher Application provides:

- Course management
- MP4 upload
- AI processing status
- Transcript editor
- OCR editor
- Formula editor
- Translation editor
- Quiz editor
- Interactive lesson preview
- Publishing workflow
- Analytics

---

## 4.3 Admin Dashboard

The Admin Dashboard provides:

- User management
- Organization management
- Subscription management
- AI job monitoring
- System health
- Storage monitoring
- Reports
- Audit logs

---

# 5. API Gateway

The API Gateway is the primary entry point for application requests.

Responsibilities:

- Authentication
- Authorization
- Request validation
- Rate limiting
- API routing
- Logging
- API versioning

Example:

```text
Client
   │
   ▼
API Gateway
   │
   ├── Auth Service
   ├── User Service
   ├── Course Service
   ├── Video Service
   ├── AI Job Service
   ├── Quiz Service
   ├── Learning Service
   ├── Analytics Service
   ├── Subscription Service
   └── Notification Service
```

---

# 6. Core Application Services

## Authentication Service

Responsible for:

- Login
- Token management
- Session management
- MFA
- Password reset

---

## User Service

Responsible for:

- User profiles
- Roles
- Permissions
- Organization membership

---

## Course Service

Responsible for:

- Courses
- Lessons
- Chapters
- Publishing
- Versioning

---

## Video Service

Responsible for:

- Upload management
- Video metadata
- Video validation
- Video processing
- Streaming metadata

---

## AI Job Service

Responsible for:

- Creating AI jobs
- Tracking stages
- Queue management
- Retry handling
- Processing status

---

## Quiz Service

Responsible for:

- Quiz creation
- Questions
- Answers
- Scoring
- Quiz attempts

---

## Learning Service

Responsible for:

- Student progress
- Playback position
- Completion
- Bookmarks
- Notes

---

## Analytics Service

Responsible for:

- Event collection
- Learning analytics
- Video analytics
- Quiz analytics
- Dashboard metrics

---

## Subscription Service

Responsible for:

- Plans
- Entitlements
- Usage limits
- Subscription status
- License management

---

## Notification Service

Responsible for:

- In-app notifications
- Email notifications
- Processing notifications
- Course notifications

---

# 7. AI Processing Context

The AI pipeline is the core of AILPG.

```text
                 MP4
                  │
                  ▼
          ┌───────────────┐
          │ Video Service │
          └───────┬───────┘
                  │
                  ▼
             AI Job Queue
                  │
                  ▼
        ┌───────────────────┐
        │ Audio Extraction  │
        └─────────┬─────────┘
                  ▼
        ┌───────────────────┐
        │ Speech-to-Text     │
        └─────────┬─────────┘
                  ▼
        ┌───────────────────┐
        │ OCR                │
        └─────────┬─────────┘
                  ▼
        ┌───────────────────┐
        │ Formula Recognition│
        └─────────┬─────────┘
                  ▼
        ┌───────────────────┐
        │ Topic Segmentation │
        └─────────┬─────────┘
                  ▼
        ┌───────────────────┐
        │ Quiz Generation    │
        └─────────┬─────────┘
                  ▼
        ┌───────────────────┐
        │ Translation        │
        └─────────┬─────────┘
                  ▼
        ┌───────────────────┐
        │ HTML Generation    │
        └─────────┬─────────┘
                  ▼
             Review
                  │
                  ▼
             Publish
```

---

# 8. External AI Services

The architecture shall support pluggable AI providers.

Possible capabilities:

| Capability | Provider Type |
|------------|---------------|
| Speech-to-Text | STT Provider |
| OCR | OCR Provider |
| Translation | Translation Provider |
| Content Generation | LLM Provider |
| Formula Recognition | Math/OCR Model |
| Text-to-Speech | TTS Provider - Future |

The system should avoid hard-coding the application to one AI provider.

---

# 9. Database Context

PostgreSQL is the recommended primary relational database.

Core domains include:

```text
Users
Organizations
Roles
Permissions
Courses
Lessons
Videos
AI Jobs
Transcripts
OCR Results
Formulas
Translations
Quizzes
Quiz Attempts
Learning Progress
Subscriptions
Analytics Events
Notifications
Audit Logs
```

---

# 10. Object Storage Context

Object storage is used for large files.

Examples:

```text
/raw-videos/
/
  original.mp4

/processed-videos/
/
  master/
  1080p/
  720p/
  480p/

/thumbnails/

/transcripts/

/ocr/

/formulas/

/translations/

/interactive-lessons/

/reports/
```

The database stores metadata and references.

The actual large files should not normally be stored directly in PostgreSQL.

---

# 11. Video Delivery Context

The production video delivery architecture should support:

```text
Student
   │
   ▼
CDN
   │
   ▼
Video Streaming Origin
   │
   ▼
Object Storage
```

The system may generate multiple video representations.

Example:

```text
1080p
720p
480p
360p
```

The available quality levels depend on the content, subscription entitlement, device, and implementation.

---

# 12. Subscription Context

The subscription system controls feature access.

```text
Student
   │
   ▼
Subscription Service
   │
   ▼
Entitlements
   │
   ├── Video Quality
   ├── AI Usage
   ├── Storage
   ├── Translation
   ├── Analytics
   └── Downloads
```

Authorization must be enforced on the backend.

The frontend must not be the only enforcement layer.

---

# 13. Analytics Context

```text
Student Interaction
        │
        ▼
Event Collector
        │
        ▼
Event Queue
        │
        ▼
Analytics Processor
        │
        ├───────────────┐
        ▼               ▼
Analytics DB       Aggregation
                        │
                        ▼
                   Dashboards
```

Example events:

```text
video_started
video_paused
video_resumed
video_seeked
video_completed

quiz_displayed
quiz_answered
quiz_completed

language_changed
quality_changed
playback_speed_changed

lesson_completed
course_completed
```

---

# 14. Notification Context

```text
Application Event
       │
       ▼
Notification Service
       │
       ├── In-App
       ├── Email
       └── Push (Future)
```

Examples:

```text
AI processing completed
Review required
Course published
Course assigned
Course completed
Certificate generated
Subscription expiring
```

---

# 15. Security Boundary

The following components are protected:

```text
                Internet
                   │
                   ▼
             API Gateway
                   │
             Authentication
                   │
             Authorization
                   │
        ┌──────────┴──────────┐
        │                     │
        ▼                     ▼
 Application Services     Admin APIs
        │                     │
        └──────────┬──────────┘
                   ▼
             Private Data
               Services
```

Sensitive infrastructure should not be directly exposed to the public internet.

---

# 16. Multi-Tenant Context

AILPG should support organization-based tenancy.

Example:

```text
AILPG Platform
│
├── Organization A
│   ├── Admin
│   ├── Teachers
│   └── Students
│
├── Organization B
│   ├── Admin
│   ├── Teachers
│   └── Students
│
└── Organization C
    ├── Admin
    ├── Teachers
    └── Students
```

Data access must be isolated according to organization permissions.

---

# 17. High-Level Data Flow

```text
Teacher
   │
   │ Upload MP4
   ▼
Frontend
   │
   ▼
API
   │
   ▼
Upload Service
   │
   ▼
Object Storage
   │
   ▼
AI Job
   │
   ▼
Queue
   │
   ▼
AI Workers
   │
   ├── STT
   ├── OCR
   ├── Formula
   ├── Translation
   └── Quiz
   │
   ▼
Lesson Generator
   │
   ▼
Interactive HTML
   │
   ▼
Teacher Review
   │
   ▼
Publish
   │
   ▼
CDN / Application
   │
   ▼
Student
   │
   ▼
Analytics
```

---

# 18. System Context Boundaries

## Inside AILPG

- Authentication
- User Management
- Course Management
- Video Management
- AI Orchestration
- Quiz Engine
- Learning Engine
- Analytics
- Notifications
- Subscription Management
- Administration

## External Dependencies

- AI Providers
- Object Storage
- CDN
- Email Provider
- Payment Provider
- Identity Provider
- Monitoring Provider

---

# 19. Architectural Principles

The platform shall follow these principles:

1. API-first architecture
2. Secure-by-default design
3. Asynchronous AI processing
4. Object storage for large files
5. Database for metadata and transactional data
6. Provider-independent AI orchestration
7. Role-based authorization
8. Organization-level data isolation
9. Observable background jobs
10. Versioned educational content
11. Scalable video delivery
12. Mobile-responsive learning experience

---

# 20. Related Documents

- SRS-001: SRS Overview
- SRS-003: Use Case Diagrams
- System Architecture
- Database Design
- API Design
- AI Workflow
- Deployment Plan
- Security Architecture

---

# 21. Revision History

| Version | Date | Description |
|---------|------|-------------|
| 1.0.0 | 2026-08-11 | Initial System Context |
