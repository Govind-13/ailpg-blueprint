---
document_id: SD-002
title: System Architecture
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Solution Architecture Team
parent_document: SD-001
last_updated: 2026-08-11
---

# System Architecture

## 1. Purpose

This document defines the logical and physical architecture of the AILPG platform.

The architecture covers:

- Student application
- Teacher application
- Admin dashboard
- Backend API
- Authentication
- Database
- Object storage
- CDN
- Queue
- Background workers
- AI services
- Video processing
- Translation
- Analytics
- Monitoring
- Security

---

# 2. Architecture Principles

The architecture follows these principles:

```text
1. API-first
2. Modular backend
3. Asynchronous processing
4. Secure by default
5. Multi-tenant ready
6. Cloud ready
7. Provider independent AI
8. Observable
9. Horizontally scalable
10. Human review for AI content
```

---

# 3. Complete Architecture

```text
                                INTERNET
                                    │
                                    ▼
                           ┌─────────────────┐
                           │   CDN / WAF     │
                           └────────┬────────┘
                                    │
                 ┌──────────────────┼──────────────────┐
                 │                  │                  │
                 ▼                  ▼                  ▼
        ┌────────────────┐ ┌────────────────┐ ┌────────────────┐
        │ Student Portal │ │ Teacher Portal │ │ Admin Dashboard│
        └───────┬────────┘ └───────┬────────┘ └───────┬────────┘
                │                  │                  │
                └──────────────────┼──────────────────┘
                                   ▼
                         ┌──────────────────┐
                         │   API Gateway    │
                         └────────┬─────────┘
                                  │
                    ┌─────────────┼─────────────┐
                    │             │             │
                    ▼             ▼             ▼
              ┌──────────┐ ┌───────────┐ ┌────────────┐
              │   Auth   │ │  Content  │ │ Learning   │
              │  Module  │ │  Module   │ │  Module    │
              └────┬─────┘ └─────┬─────┘ └─────┬──────┘
                   │             │             │
                   └─────────────┼─────────────┘
                                 ▼
                       ┌───────────────────┐
                       │   PostgreSQL      │
                       └───────────────────┘

                                 │
                                 ▼
                       ┌───────────────────┐
                       │   Redis Cache     │
                       └───────────────────┘

                                 │
                                 ▼
                       ┌───────────────────┐
                       │   Job Queue       │
                       └─────────┬─────────┘
                                 │
             ┌───────────────────┼───────────────────┐
             │                   │                   │
             ▼                   ▼                   ▼
      ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
      │Video Worker │     │ AI Workers  │     │Translation  │
      └──────┬──────┘     └──────┬──────┘     │   Worker    │
             │                   │             └──────┬──────┘
             │                   │                    │
             └───────────────────┼────────────────────┘
                                 ▼
                         ┌─────────────────┐
                         │ Object Storage  │
                         └────────┬────────┘
                                  │
                                  ▼
                                CDN
                                  │
                                  ▼
                               Student
```

---

# 4. Application Layers

AILPG is divided into:

```text
Presentation Layer
        ↓
API Layer
        ↓
Application Layer
        ↓
Domain Layer
        ↓
Infrastructure Layer
        ↓
Data / External Services
```

---

# 5. Presentation Layer

Contains:

```text
Student Web App
Teacher Web App
Admin Dashboard
```

Responsibilities:

- Display UI
- Form validation
- User interaction
- API calls
- Video playback
- Progress display
- Real-time job status

The frontend must not directly access the database.

---

# 6. API Layer

The API layer handles:

```text
HTTP requests
Authentication
Authorization
Validation
Rate limiting
Request logging
Response formatting
API versioning
```

Example:

```text
/api/v1/auth/*
/api/v1/users/*
/api/v1/courses/*
/api/v1/lessons/*
/api/v1/videos/*
/api/v1/jobs/*
/api/v1/questions/*
/api/v1/progress/*
/api/v1/analytics/*
/api/v1/admin/*
```

---

# 7. Application Layer

The application layer contains business use cases.

Examples:

```text
CreateCourse
UploadVideo
StartVideoProcessing
ReviewTranscript
GenerateQuiz
PublishLesson
SubmitAnswer
UpdateProgress
CreateSubscription
```

---

# 8. Domain Layer

Contains core business rules.

Examples:

```text
Course
Lesson
Video
AIJob
Question
Attempt
Subscription
Organization
User
```

Domain rules should not depend directly on:

```text
HTTP
Database
AI Provider
Cloud Storage
```

---

# 9. Infrastructure Layer

Handles external implementations:

```text
PostgreSQL
Redis
Object Storage
Queue
AI Providers
Email Provider
Payment Provider
CDN
```

---

# 10. Frontend Architecture

Recommended structure:

```text
frontend/
│
├── shared/
│   ├── components/
│   ├── theme/
│   ├── localization/
│   ├── services/
│   └── utils/
│
├── student/
│   ├── dashboard/
│   ├── courses/
│   ├── player/
│   ├── progress/
│   └── profile/
│
├── teacher/
│   ├── dashboard/
│   ├── courses/
│   ├── upload/
│   ├── processing/
│   ├── review/
│   └── publishing/
│
└── admin/
    ├── dashboard/
    ├── users/
    ├── organizations/
    ├── jobs/
    ├── subscriptions/
    ├── audit/
    └── system-health/
```

---

# 11. Backend Architecture

Recommended modular structure:

```text
backend/
│
├── auth/
├── users/
├── organizations/
├── courses/
├── lessons/
├── videos/
├── ai/
├── questions/
├── translations/
├── learning/
├── analytics/
├── subscriptions/
├── notifications/
├── admin/
├── audit/
└── infrastructure/
```

---

# 12. Authentication Module

Responsibilities:

```text
Registration
Login
Logout
Password reset
Session management
Token management
Role management
```

---

# 13. User Module

Responsibilities:

```text
User profile
User preferences
Language preference
Account status
Organization membership
Role assignment
```

---

# 14. Organization Module

Responsibilities:

```text
Organization creation
Members
Roles
Courses
Organization settings
Usage limits
```

---

# 15. Course Module

Responsibilities:

```text
Create course
Edit course
Course structure
Chapters
Lessons
Course publishing
Course versioning
```

---

# 16. Lesson Module

Responsibilities:

```text
Lesson metadata
Lesson manifest
Interactive checkpoints
Questions
Translations
Versions
Publishing
```

---

# 17. Video Module

Responsibilities:

```text
Upload initialization
Upload completion
Validation
Metadata
Processing
Transcoding
Streaming
Thumbnails
```

---

# 18. AI Module

The AI module is the central orchestration layer.

```text
AI Module
│
├── Job Manager
├── Provider Manager
├── Prompt Manager
├── Result Normalizer
├── Confidence Engine
├── Validation
└── AI Usage Tracking
```

---

# 19. AI Provider Architecture

```text
                 AI Provider Interface
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
       Provider A     Provider B     Provider C
          │              │              │
          ▼              ▼              ▼
         STT            LLM           Vision
```

Business logic communicates with:

```text
AIProviderInterface
```

rather than a specific vendor SDK.

---

# 20. Video Worker Architecture

```text
Video Job
   ↓
Worker
   ├── Metadata extraction
   ├── Audio extraction
   ├── Frame extraction
   ├── Transcoding
   ├── Thumbnail creation
   └── HLS generation
```

---

# 21. AI Worker Architecture

```text
AI Queue
   │
   ├── STT Worker
   ├── OCR Worker
   ├── Formula Worker
   ├── Topic Worker
   ├── Quiz Worker
   └── Lesson Worker
```

---

# 22. Translation Worker

```text
Translation Job
      ↓
Source Content
      ↓
Language Selection
      ↓
Translation Provider
      ↓
Quality Validation
      ↓
Translated Content
```

Supported initial languages:

```text
English
Tamil
Hindi
Malayalam
Telugu
Kannada
```

---

# 23. Job Orchestrator

The Job Orchestrator controls the pipeline.

Example:

```text
VIDEO_UPLOADED
      ↓
VALIDATE
      ↓
EXTRACT_AUDIO
      ↓
TRANSCRIBE
      ↓
EXTRACT_FRAMES
      ↓
OCR
      ↓
FORMULA_ANALYSIS
      ↓
TOPIC_ANALYSIS
      ↓
QUESTION_GENERATION
      ↓
TRANSLATION
      ↓
LESSON_GENERATION
      ↓
READY_FOR_REVIEW
```

---

# 24. Job State Machine

```text
QUEUED
  │
  ▼
PROCESSING
  │
  ├──────────────► FAILED
  │                   │
  │                   ▼
  │                RETRYING
  │                   │
  │                   └──────► PROCESSING
  │
  ▼
COMPLETED
```

---

# 25. Idempotency

Every important background job should be safely retryable.

Example:

```text
Generate Thumbnail
```

If executed twice:

```text
Same logical asset
```

should not create uncontrolled duplicates.

---

# 26. Job Dependencies

Example:

```text
TRANSCRIPTION
      │
      ▼
TOPIC DETECTION
      │
      ├──────────────┐
      ▼              ▼
QUESTION        TRANSLATION
      │              │
      └──────┬───────┘
             ▼
      LESSON GENERATION
```

The orchestrator must understand these dependencies.

---

# 27. Database Architecture

PostgreSQL stores:

```text
users
organizations
memberships
courses
chapters
lessons
videos
ai_jobs
transcripts
ocr_results
formulas
questions
translations
lesson_versions
progress
attempts
subscriptions
audit_logs
```

Detailed schema will be defined in:

```text
LAYER 5 — Database Design
```

---

# 28. Object Storage Architecture

Object storage is divided conceptually into:

```text
raw/
processed/
streaming/
thumbnails/
frames/
transcripts/
exports/
lesson-assets/
temporary/
```

---

# 29. Redis Architecture

Redis is used for:

```text
Caching
Rate limiting
Short-lived state
Job locks
Temporary processing state
Session-related data
```

Redis should not store the only copy of important business data.

---

# 30. Queue Architecture

Queue categories:

```text
video
transcription
ocr
formula
quiz
translation
lesson
notifications
analytics
```

Each queue can have:

```text
Priority
Concurrency
Retry count
Timeout
Dead-letter queue
```

---

# 31. API-to-Queue Flow

Example:

```text
Teacher
   ↓
POST /videos
   ↓
API
   ↓
Create Video Record
   ↓
Create AI Job
   ↓
Queue
   ↓
Worker
```

The API immediately returns a job reference.

---

# 32. Real-Time Job Updates

Teacher should be able to see:

```text
Upload: 100%
Validation: Complete
Audio: Complete
Transcript: 82%
OCR: Pending
Formula: Pending
Quiz: Pending
Translation: Pending
```

Possible implementation:

```text
WebSocket
```

or:

```text
Server-Sent Events
```

Fallback:

```text
Polling
```

---

# 33. Teacher Review Architecture

```text
AI Results
     ↓
Review API
     ↓
Teacher Review Workspace
     │
     ├── Transcript
     ├── OCR
     ├── Formula
     ├── Questions
     ├── Translation
     └── Timeline
     │
     ▼
Save Changes
     ↓
Approve
     ↓
Publish
```

---

# 34. Student Player Architecture

```text
Lesson API
    ↓
Lesson Manifest
    ↓
Video URL
    ↓
Player
    │
    ├── Timeline
    ├── Transcript
    ├── Checkpoints
    ├── Questions
    ├── Feedback
    └── Translation
```

---

# 35. Progress Architecture

Student events:

```text
VIDEO_STARTED
VIDEO_PAUSED
VIDEO_RESUMED
VIDEO_SEEKED
CHECKPOINT_REACHED
QUESTION_STARTED
QUESTION_ANSWERED
LESSON_COMPLETED
```

These events update:

```text
Progress
Attempts
Analytics
```

---

# 36. Subscription Architecture

```text
Subscription
     ↓
Plan
     ↓
Entitlements
     ↓
Access Policy
     ↓
Feature Access
```

The backend must enforce access.

---

# 37. Admin Dashboard Architecture

Admin dashboard modules:

```text
Overview
│
├── Users
├── Organizations
├── Courses
├── AI Jobs
├── Failed Jobs
├── Storage
├── Subscriptions
├── Payments
├── Audit Logs
├── System Health
└── Analytics
```

---

# 38. Admin Job Management

Admin should be able to:

```text
View job
View job stages
View errors
Retry failed job
Cancel job
Inspect processing time
Inspect AI usage
```

Dangerous operations should require elevated permission.

---

# 39. Audit Architecture

Important actions generate audit events.

Examples:

```text
USER_CREATED
ROLE_CHANGED
VIDEO_UPLOADED
LESSON_APPROVED
LESSON_PUBLISHED
LESSON_UNPUBLISHED
SUBSCRIPTION_CHANGED
ADMIN_ACTION
```

---

# 40. Security Boundaries

```text
PUBLIC
  │
  ▼
CDN / WAF
  │
  ▼
API
  │
  ▼
AUTHORIZATION
  │
  ▼
APPLICATION
  │
  ├── Database
  ├── Queue
  ├── Redis
  └── Storage
```

Database should not be publicly exposed.

---

# 41. Tenant Security

Every tenant-aware request must validate:

```text
Authenticated User
       ↓
Organization Membership
       ↓
Resource Organization
       ↓
Permission
       ↓
Action
```

---

# 42. API Security

API must implement:

```text
HTTPS
Authentication
Authorization
Input validation
Rate limiting
Request size limits
File validation
CORS policy
Security headers
Audit logging
```

---

# 43. File Upload Security

Upload pipeline:

```text
Client
  ↓
Signed Upload URL
  ↓
Object Storage
  ↓
Upload Event
  ↓
Backend Validation
  ↓
File Scan
  ↓
Media Metadata Validation
  ↓
Processing
```

---

# 44. CDN Security

Video should use controlled access.

```text
Student Request
      ↓
Backend Authorization
      ↓
Signed CDN URL
      ↓
Video Access
```

---

# 45. Monitoring Architecture

```text
Application
   │
   ├── Logs
   ├── Metrics
   └── Traces
        │
        ▼
 Observability Platform
        │
        ├── Dashboard
        ├── Alerts
        └── Incident Detection
```

---

# 46. Important Metrics

## Application

```text
Request latency
Request rate
Error rate
Active users
```

## AI

```text
Jobs processed
Job duration
Failure rate
Token usage
Provider errors
```

## Video

```text
Upload size
Processing time
Transcoding time
Streaming errors
```

## Learning

```text
Lessons started
Lessons completed
Quiz attempts
Quiz accuracy
```

---

# 47. Failure Domains

Failures should be isolated.

Example:

```text
Translation Provider DOWN
        ↓
Translation jobs delayed
        ↓
Core student playback continues
```

Similarly:

```text
Analytics DOWN
        ↓
Learning should continue
```

Analytics should not block core learning.

---

# 48. Availability Priorities

Critical:

```text
Authentication
Student Playback
Published Lessons
Core API
Database
```

High:

```text
Teacher Review
Video Processing
AI Jobs
```

Medium:

```text
Analytics
Notifications
Reports
```

---

# 49. Architecture Decision

For MVP:

```text
Modular Monolith
+
Background Workers
+
Queue
+
PostgreSQL
+
Redis
+
Object Storage
```

This is preferred over premature microservices.

---

# 50. Future Migration Path

```text
Modular Monolith
       ↓
Identify bottleneck
       ↓
Extract module
       ↓
Independent service
       ↓
Scale independently
```

Possible future services:

```text
Auth Service
Video Service
AI Service
Learning Service
Analytics Service
Billing Service
Notification Service
```

---

# 51. Recommended Repository Architecture

```text
ailpg/
│
├── apps/
│   ├── web/
│   ├── teacher/
│   └── admin/
│
├── backend/
│   ├── src/
│   │   ├── auth/
│   │   ├── users/
│   │   ├── organizations/
│   │   ├── courses/
│   │   ├── lessons/
│   │   ├── videos/
│   │   ├── ai/
│   │   ├── learning/
│   │   ├── analytics/
│   │   ├── subscriptions/
│   │   └── admin/
│   │
│   └── tests/
│
├── workers/
│   ├── video/
│   ├── stt/
│   ├── ocr/
│   ├── formula/
│   ├── quiz/
│   ├── translation/
│   └── lesson/
│
├── packages/
│   ├── shared-types/
│   ├── ui/
│   └── config/
│
├── infrastructure/
│   ├── docker/
│   ├── terraform/
│   └── deployment/
│
├── docs/
│   ├── 01_Project_Charter/
│   ├── 02_Software_Requirement_Specification/
│   ├── 03_System_Design/
│   ├── 04_Technical_Architecture/
│   ├── 05_UI_UX_Blueprint/
│   ├── 06_Database_Design/
│   ├── 07_API_Design/
│   ├── 08_AI_Workflow/
│   └── 09_Deployment_Plan/
│
└── README.md
```

---

# 52. Architecture Completion Criteria

This document is complete when:

- All major components are identified.
- Frontend architecture is defined.
- Backend modules are defined.
- Worker architecture is defined.
- Queue architecture is defined.
- Database responsibilities are defined.
- Storage responsibilities are defined.
- AI abstraction is defined.
- Admin architecture is defined.
- Security boundaries are defined.
- Scaling strategy is defined.
- MVP architecture is defined.
- Future migration path is defined.

---

# 53. Next Document

```text
03_Component_Design.md
```

This document will break the architecture into individual components and define for each:

```text
Component
   ↓
Purpose
   ↓
Responsibilities
   ↓
Inputs
   ↓
Outputs
   ↓
Dependencies
   ↓
APIs
   ↓
Database Tables
   ↓
Events
   ↓
Failure Handling
```

---

# 54. Revision History

| Version | Date | Description |
|---|---|---|
| 1.0.0 | 2026-08-11 | Initial System Architecture |
