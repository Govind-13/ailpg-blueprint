# AILPG — System Overview

**Document Path:** `docs/05_SYSTEM_DESIGN/01_System_Overview.md`
**Project:** MP4 → Interactive Learning Platform Generator (AILPG)
**Document Type:** System Design
**Version:** 1.0
**Status:** Draft / Implementation Ready

---

# 1. Purpose

This document defines the high-level system design of the **MP4 → Interactive Learning Platform Generator (AILPG)**.

AILPG is a platform that transforms a solved educational video into an interactive digital learning experience.

The primary system flow is:

```text
MP4 Video
   ↓
Upload
   ↓
Validation
   ↓
Video Processing
   ↓
Audio / Frame Extraction
   ↓
AI Analysis
   ↓
Transcript + OCR + Math Recognition
   ↓
Concept Detection
   ↓
Question Generation
   ↓
Translation
   ↓
Interactive Lesson Generation
   ↓
Automated Validation
   ↓
Human Review
   ↓
Course Builder
   ↓
Publish
   ↓
Student Learning
   ↓
Analytics
```

---

# 2. System Vision

AILPG should allow a content creator to upload an existing solved-math video and automatically produce a structured interactive lesson without manually recreating the entire lesson.

The platform combines:

* Video processing
* Speech recognition
* OCR
* Mathematical recognition
* AI content generation
* Translation
* Interactive question generation
* Human review
* Course management
* Video delivery
* Student interaction
* Analytics

---

# 3. Core Problem

Traditional educational videos are largely passive.

A student may:

```text
Watch
 ↓
Listen
 ↓
Watch
 ↓
Leave
```

AILPG converts the experience into:

```text
Watch
 ↓
Understand
 ↓
Question appears
 ↓
Answer
 ↓
Receive feedback
 ↓
Continue
 ↓
Complete lesson
 ↓
Track learning
```

---

# 4. System Objectives

The system must:

1. Accept educational MP4 videos.
2. Validate uploaded videos.
3. Store source media securely.
4. Process video asynchronously.
5. Extract audio and frames.
6. Generate transcripts.
7. Detect text using OCR.
8. Detect mathematical expressions.
9. Segment educational concepts.
10. Generate interactive questions.
11. Generate explanations and hints.
12. Translate generated content.
13. Generate structured interactive lessons.
14. Validate AI-generated content.
15. Provide human review.
16. Allow course construction.
17. Publish lessons.
18. Deliver videos to students.
19. Pause video for interactive questions.
20. Record student interactions.
21. Provide analytics.
22. Enforce user permissions and subscriptions.

---

# 5. System Actors

## 5.1 Student

Uses AILPG to:

* Browse courses
* Watch lessons
* Answer questions
* View explanations
* Use translations
* Change quality
* View transcripts
* Track progress

---

## 5.2 Instructor

Uses AILPG to:

* Upload videos
* Configure AI generation
* Review lessons
* Edit generated content
* Create courses
* Publish lessons
* Analyze student engagement

---

## 5.3 Reviewer

Reviews AI-generated content.

Responsibilities include:

* Transcript verification
* OCR verification
* Equation verification
* Question verification
* Translation verification
* Lesson structure verification

---

## 5.4 Administrator

Manages:

* Users
* Courses
* Videos
* AI jobs
* Reviews
* Subscriptions
* System configuration
* Analytics
* Audit logs

---

## 5.5 AI Processing System

Performs:

* Transcription
* OCR
* Math recognition
* Concept detection
* Question generation
* Translation
* Lesson generation

---

# 6. High-Level Architecture

```text
                         ┌─────────────────────┐
                         │       Users         │
                         │                     │
                         │ Student / Creator   │
                         │ Reviewer / Admin    │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   Web Application   │
                         │                     │
                         │ React / Web UI      │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │      API Layer      │
                         │                     │
                         │ Authentication      │
                         │ Authorization       │
                         │ Business Logic      │
                         └───────┬───────┬─────┘
                                 │       │
                   ┌─────────────┘       └─────────────┐
                   ▼                                   ▼
        ┌─────────────────────┐              ┌─────────────────────┐
        │ Application Services│              │   Object Storage    │
        │                     │              │                     │
        │ Video               │              │ MP4                 │
        │ Course              │              │ Transcoded Video    │
        │ Lesson              │              │ Images              │
        │ Question            │              │ Generated Assets    │
        │ Analytics           │              └─────────────────────┘
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │     Job Queue       │
        │                     │
        │ Video Processing    │
        │ AI Processing       │
        │ Translation         │
        │ Encoding            │
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │    AI Pipeline      │
        │                     │
        │ STT                 │
        │ OCR                 │
        │ Math AI             │
        │ LLM                 │
        │ Translation         │
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │      Database       │
        │                     │
        │ Users               │
        │ Courses             │
        │ Lessons             │
        │ Questions           │
        │ Reviews             │
        │ Analytics           │
        └─────────────────────┘
```

---

# 7. Architectural Style

AILPG should initially use a **modular service-oriented backend**.

The recommended approach is:

```text
Modular Monolith
       +
Asynchronous Workers
       +
External AI Services
       +
Object Storage
```

This avoids unnecessary microservice complexity during early development while allowing individual processing components to scale independently.

---

# 8. Why Modular Architecture

The system contains several distinct domains.

```text
Identity
Videos
Courses
Lessons
Questions
AI Processing
Reviews
Subscriptions
Analytics
```

These should be separated logically even if they initially run inside one backend application.

---

# 9. Core System Modules

```text
backend/
│
├── auth
├── users
├── organizations
├── videos
├── media
├── processing
├── ai
├── transcripts
├── ocr
├── mathematics
├── questions
├── lessons
├── courses
├── translations
├── reviews
├── publishing
├── subscriptions
├── analytics
├── notifications
├── audit
└── administration
```

---

# 10. Request Types

AILPG has two major categories of workloads.

## 10.1 Synchronous

Short operations:

```text
Login
Get course
Get lesson
Submit answer
Update profile
Save question
```

These should return quickly.

---

## 10.2 Asynchronous

Long-running operations:

```text
Upload processing
Video transcoding
Transcription
OCR
Math recognition
Question generation
Translation
Lesson generation
Bulk processing
Analytics aggregation
```

These should use background jobs.

---

# 11. Synchronous Request Architecture

```text
Browser
   ↓
API
   ↓
Authentication
   ↓
Authorization
   ↓
Service
   ↓
Database
   ↓
Response
```

Example:

```http
GET /api/courses/123
```

---

# 12. Asynchronous Processing Architecture

```text
Browser
   ↓
API
   ↓
Create Job
   ↓
Queue
   ↓
Worker
   ↓
Processing
   ↓
Database / Storage
   ↓
Job Status
```

The browser should not remain connected to the server for the entire AI-processing duration.

---

# 13. Video Processing Pipeline

```text
                    MP4 Upload
                        │
                        ▼
                  File Validation
                        │
                        ▼
                  Object Storage
                        │
                        ▼
                 Processing Queue
                        │
                        ▼
               Video Processing
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
       Audio          Frames       Metadata
          │             │             │
          ▼             ▼             ▼
       Speech          OCR        Video Info
     Recognition
          │
          └─────────────┬─────────────┘
                        ▼
                  AI Analysis
                        │
                        ▼
                Lesson Generation
```

---

# 14. AI Pipeline

```text
Video
 │
 ├── Audio
 │     ↓
 │   Transcript
 │
 ├── Frames
 │     ↓
 │   OCR
 │     ↓
 │   Math Recognition
 │
 └── Metadata
       ↓
   Context Builder
       ↓
   Concept Detection
       ↓
   Question Generation
       ↓
   Explanation Generation
       ↓
   Translation
       ↓
   Lesson JSON
```

---

# 15. AI Pipeline Principle

Each AI stage should produce a structured output rather than directly modifying the final lesson.

Example:

```text
Raw Video
   ↓
Transcript JSON
   ↓
OCR JSON
   ↓
Math JSON
   ↓
Concept JSON
   ↓
Question JSON
   ↓
Translation JSON
   ↓
Lesson JSON
```

This makes individual stages:

* Testable
* Retryable
* Versionable
* Reviewable
* Replaceable

---

# 16. Intermediate Representation

AILPG should use an internal structured representation.

Example:

```json id="c5o9n8"
{
  "videoId": "vid_123",
  "segments": [
    {
      "start": 0,
      "end": 12,
      "type": "introduction"
    },
    {
      "start": 12,
      "end": 38,
      "type": "solution_step",
      "concept": "linear_equation"
    }
  ]
}
```

This intermediate representation becomes the foundation for lesson generation.

---

# 17. Lesson Generation

The lesson generator combines:

```text
Transcript
+
OCR
+
Math
+
Concepts
+
Questions
+
Explanations
+
Translations
+
Video timestamps
```

and creates:

```text
Interactive Lesson
```

---

# 18. Lesson Structure

```text
Course
│
├── Module
│   │
│   ├── Lesson
│   │   ├── Video
│   │   ├── Transcript
│   │   ├── Concepts
│   │   ├── Questions
│   │   ├── Explanations
│   │   └── Translation
│   │
│   └── Lesson
│
└── Module
```

---

# 19. Interactive Question Architecture

Questions are linked to video timestamps.

Example:

```text
Video
│
├── 00:00 Introduction
│
├── 01:20 Step 1
│
├── 02:35 Question 1
│
├── 03:10 Step 2
│
├── 05:20 Question 2
│
└── 07:00 Final Solution
```

At the question timestamp:

```text
Video
  ↓
Pause
  ↓
Question
  ↓
Answer
  ↓
Score
  ↓
Explanation
  ↓
Resume
```

---

# 20. Backend-Controlled Scoring

Correct answers must not be trusted from the client.

Architecture:

```text
Student
   ↓
Answer
   ↓
API
   ↓
Question Service
   ↓
Server-side Validation
   ↓
Score
   ↓
Feedback
```

The client should receive the result, not the protected answer key before submission.

---

# 21. Student Playback Architecture

```text
Student Browser
       │
       ▼
Lesson API
       │
       ├── Lesson Metadata
       ├── Questions
       ├── Transcript
       └── Entitlements
       │
       ▼
Video CDN / Media Server
       │
       ▼
Video Player
       │
       ▼
Question Engine
       │
       ▼
Answer API
       │
       ▼
Analytics
```

---

# 22. Subscription-Aware Quality

Video quality should be controlled by the backend.

```text
Student
   ↓
Authentication
   ↓
Entitlement Check
   ↓
Allowed Quality Levels
   ↓
Signed Media Access
```

Example:

```json id="lx9q7k"
{
  "availableQualities": [
    "360p",
    "480p",
    "720p"
  ]
}
```

A higher-quality stream should only be delivered when the user's entitlement permits it.

---

# 23. Object Storage

Large files should not be stored directly in the application database.

Object storage should contain:

```text
/videos/original/
/videos/transcoded/
/videos/thumbnails/
/audio/
/frames/
/transcripts/
/ocr/
/math/
/lessons/
/exports/
```

The database stores metadata and references.

---

# 24. Database Responsibility

The relational database stores structured application information.

Examples:

```text
users
courses
modules
lessons
videos
questions
answers
reviews
subscriptions
analytics
jobs
translations
audit_logs
```

Large binary assets remain in object storage.

---

# 25. Cache Layer

A cache may be introduced for frequently accessed data.

Candidates:

```text
Course metadata
Lesson metadata
User session data
Entitlements
Question configuration
Analytics summaries
```

Cache must never become the authoritative source for critical transactional data.

---

# 26. Queue System

The queue system handles long-running tasks.

Possible queues:

```text
video-processing
transcoding
transcription
ocr
math-recognition
question-generation
translation
lesson-generation
analytics
notifications
```

Jobs should contain identifiers rather than unnecessarily duplicating large data.

---

# 27. Job Lifecycle

```text
QUEUED
  ↓
PROCESSING
  ↓
COMPLETED
```

Failure:

```text
PROCESSING
    ↓
FAILED
    ↓
RETRY
    ↓
PROCESSING
```

Permanent failure:

```text
FAILED
  ↓
REQUIRES_MANUAL_ACTION
```

---

# 28. Job Idempotency

Processing jobs should be safely retryable.

Example:

```text
Generate Transcript
        ↓
Job ID
        ↓
Retry
        ↓
Existing successful result?
      /       \
    Yes        No
     ↓          ↓
Reuse       Generate
```

This reduces duplicate processing and AI costs.

---

# 29. AI Cost Control

AI processing can be expensive.

The system should track:

```text
AI Provider
Model
Tokens
Duration
Frames processed
Audio duration
Estimated cost
Actual cost
Job status
```

This allows administrators to monitor processing expenditure.

---

# 30. AI Model Abstraction

Do not tightly couple business logic to one AI provider.

Recommended:

```text
AILPG AI Interface
       │
 ┌─────┼─────┐
 ▼     ▼     ▼
Provider A
Provider B
Provider C
```

The application should be able to replace individual providers without redesigning the complete platform.

---

# 31. AI Service Interface

Conceptually:

```text
transcribe(audio)
extractText(frames)
recognizeMath(content)
detectConcepts(content)
generateQuestions(content)
generateExplanation(content)
translate(content)
generateLesson(content)
```

Each function should have structured input/output contracts.

---

# 32. AI Confidence

AI outputs should optionally include confidence metadata.

Example:

```json id="f5qv2w"
{
  "text": "2x + 4 = 10",
  "confidence": 0.94
}
```

Confidence should be treated as a signal for review prioritization, not as proof of correctness.

---

# 33. Human Review Architecture

```text
AI Generated
     ↓
Automated Validation
     ↓
Review Queue
     ↓
Reviewer
     ↓
Edit
     ↓
Approve
     ↓
Publish
```

---

# 34. Automated Validation

Before human review:

```text
Transcript validation
OCR validation
Math syntax validation
Question schema validation
Answer validation
Translation completeness
Timestamp validation
Lesson schema validation
```

---

# 35. Human Approval

Content should not automatically become publicly published solely because AI generation completed.

Possible states:

```text
AI_GENERATED
VALIDATED
IN_REVIEW
CHANGES_REQUESTED
APPROVED
PUBLISHED
REJECTED
```

---

# 36. Course Builder Architecture

Course Builder combines generated lessons into structured educational content.

```text
Course
 ↓
Module
 ↓
Lesson
 ↓
Video
 ↓
Interactive Content
```

The generated lesson should be importable rather than requiring manual reconstruction.

---

# 37. Publishing Architecture

```text
Draft
 ↓
Validation
 ↓
Review
 ↓
Approval
 ↓
Publish
 ↓
Create Published Version
 ↓
CDN / Student Delivery
```

Published content should reference an immutable version.

---

# 38. Versioning

Versionable entities include:

```text
Video
Transcript
Math
Question
Translation
Lesson
Course
```

Example:

```text
Lesson v1
Lesson v2
Lesson v3
```

The published version should remain stable while a new draft is being edited.

---

# 39. Analytics Architecture

AILPG should capture learning events.

```text
Student Action
      ↓
Event Collector
      ↓
Event Queue
      ↓
Analytics Storage
      ↓
Aggregation
      ↓
Dashboard
```

---

# 40. Core Analytics Events

```text
VIDEO_STARTED
VIDEO_PAUSED
VIDEO_COMPLETED

QUESTION_SHOWN
QUESTION_ANSWERED
QUESTION_SKIPPED
QUESTION_HINT_USED
QUESTION_RETRY

LESSON_STARTED
LESSON_COMPLETED

TRANSCRIPT_OPENED
TRANSLATION_CHANGED
QUALITY_CHANGED
```

---

# 41. Analytics Data Separation

Separate:

```text
Operational Database
```

from:

```text
Analytics / Reporting Data
```

where scale requires it.

The initial implementation may begin with shared infrastructure while maintaining logical separation.

---

# 42. Notification Architecture

Notifications can be generated by events.

Example:

```text
AI Job Completed
       ↓
Notification Service
       ↓
Instructor Notification
```

Channels may include:

```text
In-App
Email
Push
```

depending on product requirements.

---

# 43. Authentication Architecture

```text
User
 ↓
Login
 ↓
Authentication Service
 ↓
Session / Token
 ↓
API
```

Authentication should support appropriate methods such as:

* Email/password
* OTP where required
* OAuth/social login if introduced
* Institution login if introduced

---

# 44. Authorization

Authorization should be enforced server-side.

Example:

```text
Request
 ↓
Authenticated?
 ↓
Role?
 ↓
Permission?
 ↓
Resource ownership?
 ↓
Allow / Deny
```

---

# 45. Multi-Tenant Architecture

If AILPG supports institutions, organizations, or businesses:

```text
Platform
│
├── Organization A
│   ├── Users
│   ├── Courses
│   └── Lessons
│
├── Organization B
│   ├── Users
│   ├── Courses
│   └── Lessons
│
└── Platform Admin
```

Tenant boundaries must be enforced at the service and database layers.

---

# 46. Security Architecture

Core security layers:

```text
HTTPS
 ↓
Authentication
 ↓
Authorization
 ↓
Input Validation
 ↓
Application Security
 ↓
Database Security
 ↓
Object Storage Security
 ↓
Audit Logging
```

---

# 47. Video Security

Original videos should not be publicly exposed.

Use:

```text
Private Object Storage
       ↓
Authorization
       ↓
Signed URL / Protected Delivery
       ↓
Video Player
```

---

# 48. Generated Lesson Security

Generated HTML should be sanitized before publishing.

Protect against:

* XSS
* Malicious HTML
* Unsafe links
* Script injection
* Unauthorized embedded resources

AI-generated content must be treated as untrusted input.

---

# 49. API Gateway / Entry Layer

Conceptual structure:

```text
Internet
   ↓
CDN / Load Balancer
   ↓
API
   ↓
Authentication
   ↓
Authorization
   ↓
Application Modules
```

---

# 50. Observability

The system should provide:

```text
Logs
Metrics
Traces
Alerts
```

Monitor:

* API latency
* Error rates
* Queue depth
* Worker failures
* AI processing duration
* AI cost
* Video processing failures
* Database performance
* Storage usage
* CDN performance

---

# 51. Logging

Application logs should include:

```text
timestamp
request_id
user_id where appropriate
service
operation
status
error_code
duration
```

Sensitive data should not be written to logs unnecessarily.

---

# 52. Distributed Request Identification

Use a request/correlation ID.

Example:

```text
Request ID:
req_8f31a2
```

The same identifier or trace context can help connect:

```text
API request
 ↓
Job creation
 ↓
Worker
 ↓
AI provider call
 ↓
Database update
```

---

# 53. Error Handling

Errors should use stable codes.

Example:

```text
VIDEO_INVALID
VIDEO_TOO_LARGE
VIDEO_PROCESSING_FAILED
TRANSCRIPTION_FAILED
OCR_FAILED
AI_GENERATION_FAILED
TRANSLATION_FAILED
LESSON_GENERATION_FAILED
QUESTION_VALIDATION_FAILED
PERMISSION_DENIED
RESOURCE_NOT_FOUND
```

---

# 54. Retry Strategy

Not every failure should be retried.

### Retryable

```text
Temporary network failure
AI provider timeout
Worker interruption
Temporary storage error
```

### Non-retryable

```text
Invalid video
Invalid configuration
Unauthorized request
Malformed content
```

---

# 55. Dead-Letter Queue

Repeatedly failing jobs should move to a dead-letter queue.

```text
Job
 ↓
Retry 1
 ↓
Retry 2
 ↓
Retry 3
 ↓
Dead Letter Queue
 ↓
Admin Review
```

---

# 56. Scalability Model

AILPG should scale independently across major workloads.

```text
API Servers
    ↑
    │
Load Balancer
    │
    ├── API Instance 1
    ├── API Instance 2
    └── API Instance N

Workers
    ├── Video Workers
    ├── AI Workers
    ├── Translation Workers
    └── Analytics Workers
```

---

# 57. Horizontal Scaling

Stateless API servers should be preferred.

```text
                    Load Balancer
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
            API-1      API-2      API-3
```

Shared state should live in external infrastructure such as databases, caches, and object storage.

---

# 58. Video Worker Scaling

Video processing can require substantial CPU/GPU resources.

Workers should scale based on:

```text
Queue depth
Video duration
Processing time
CPU usage
GPU usage
```

---

# 59. AI Worker Scaling

AI workers may scale according to:

```text
Pending jobs
Provider rate limits
Token usage
Frame-processing workload
```

---

# 60. Storage Architecture

```text
                Object Storage
                     │
      ┌──────────────┼──────────────┐
      ▼              ▼              ▼
Original Videos   Processed      Generated
                  Media          Lessons
      │              │              │
      ▼              ▼              ▼
   Private         Protected      Published
```

---

# 61. CDN Architecture

Student-facing media should use CDN delivery where appropriate.

```text
Student
   ↓
CDN
   ↓
Protected Video
   ↓
Origin Storage
```

This reduces load on application servers.

---

# 62. Caching Strategy

Possible cache layers:

```text
Browser Cache
     ↓
CDN Cache
     ↓
Application Cache
     ↓
Database
```

Caching rules must account for published lesson versions and permissions.

---

# 63. Deployment Architecture

Conceptual production environment:

```text
                         Internet
                            │
                            ▼
                         CDN/WAF
                            │
                    ┌───────┴───────┐
                    ▼               ▼
                 Frontend          API
                                    │
                         ┌──────────┼──────────┐
                         ▼          ▼          ▼
                       DB        Cache       Queue
                                              │
                         ┌────────────────────┼───────────────┐
                         ▼                    ▼               ▼
                    Video Worker          AI Worker      Analytics
                         │                    │
                         └──────────┬─────────┘
                                    ▼
                              Object Storage
```

---

# 64. Environment Strategy

Maintain separate environments:

```text
Development
     ↓
Testing
     ↓
Staging
     ↓
Production
```

Production data should not be casually copied into development environments.

---

# 65. Configuration Management

Configuration should be externalized.

Examples:

```text
DATABASE_URL
STORAGE_BUCKET
QUEUE_URL
AI_PROVIDER
AI_MODEL
CDN_URL
JWT_SECRET
```

Secrets must be stored in a secure secret-management system rather than committed to Git.

---

# 66. Database Backup

Backups should cover:

```text
Database
Critical object metadata
Configuration
Audit records
```

Backup strategy should define:

* Frequency
* Retention
* Encryption
* Restore procedure
* Recovery objectives

---

# 67. Disaster Recovery

Recovery architecture:

```text
Primary Infrastructure
       ↓
Failure
       ↓
Backup / Replica
       ↓
Restore
       ↓
Service Recovery
```

Recovery objectives should be defined before production launch.

---

# 68. Data Lifecycle

Example:

```text
Upload
 ↓
Processing
 ↓
Published
 ↓
Active
 ↓
Archived
 ↓
Retention / Deletion
```

Different content types may have different retention requirements.

---

# 69. Deletion Architecture

Deleting a video should account for dependent resources.

```text
Video
 ├── Original
 ├── Transcoded files
 ├── Frames
 ├── Audio
 ├── Transcript
 ├── OCR
 ├── Math data
 ├── Questions
 └── Lesson references
```

Deletion must prevent orphaned data while respecting versioning and audit requirements.

---

# 70. API Boundary

Frontend should communicate through stable APIs.

```text
Frontend
    │
    ▼
API Contract
    │
    ▼
Backend Services
```

Frontend should not directly access internal database structures.

---

# 71. API Domain Groups

```text
/api/auth
/api/users
/api/courses
/api/modules
/api/lessons
/api/videos
/api/questions
/api/reviews
/api/translations
/api/analytics
/api/subscriptions
/api/admin
```

---

# 72. Service Communication

Internal modules should communicate through clear interfaces.

Example:

```text
Video Service
      ↓
Processing Service
      ↓
AI Service
      ↓
Lesson Service
```

Avoid tightly coupled cross-module database manipulation.

---

# 73. Event-Driven Architecture

Important system events can trigger downstream processes.

Example:

```text
VIDEO_UPLOAD_COMPLETED
          ↓
PROCESS_VIDEO

TRANSCRIPT_COMPLETED
          ↓
RUN_MATH_ANALYSIS

AI_GENERATION_COMPLETED
          ↓
CREATE_REVIEW_TASK

REVIEW_APPROVED
          ↓
ENABLE_PUBLISH

LESSON_PUBLISHED
          ↓
UPDATE_CDN
```

---

# 74. Event Reliability

Events should support:

* Unique IDs
* Timestamp
* Event type
* Source
* Payload reference
* Retry
* Idempotency

Example:

```json id="drp9mj"
{
  "eventId": "evt_123",
  "type": "LESSON_PUBLISHED",
  "resourceId": "lesson_456"
}
```

---

# 75. System State Model

At a high level:

```text
Video
 ↓
UPLOADED
 ↓
VALIDATED
 ↓
PROCESSING
 ↓
AI_GENERATED
 ↓
REVIEW
 ↓
APPROVED
 ↓
PUBLISHED
```

A failure can occur at any processing stage.

---

# 76. End-to-End Example

A creator uploads:

```text
linear-equation.mp4
```

The system performs:

```text
1. Validate MP4
2. Store original
3. Extract audio
4. Extract frames
5. Generate transcript
6. Detect written equations
7. Recognize mathematical expressions
8. Identify solution steps
9. Generate questions
10. Generate explanations
11. Translate content
12. Build lesson JSON
13. Validate generated content
14. Create review task
15. Reviewer approves
16. Course Builder attaches lesson
17. Instructor publishes
18. Student watches
19. Questions appear
20. Answers are recorded
21. Analytics are generated
```

---

# 77. Example Lesson Processing State

```json id="plj3o9"
{
  "videoId": "vid_001",
  "status": "IN_REVIEW",
  "processing": {
    "upload": "COMPLETED",
    "transcription": "COMPLETED",
    "ocr": "COMPLETED",
    "mathRecognition": "COMPLETED",
    "questionGeneration": "COMPLETED",
    "translation": "COMPLETED",
    "lessonGeneration": "COMPLETED"
  }
}
```

---

# 78. Failure Recovery Example

Suppose translation fails.

The system should preserve:

```text
Video ✓
Transcript ✓
OCR ✓
Math ✓
Questions ✓
Lesson ✓
Translation ✕
```

Then:

```text
Retry Translation
```

rather than restarting the entire pipeline.

---

# 79. System Reliability Principles

AILPG should prioritize:

* Idempotency
* Retryability
* Versioning
* Observability
* Fault isolation
* Graceful degradation
* Data integrity

---

# 80. Graceful Degradation

If optional services fail:

```text
Translation unavailable
        ↓
Original language remains available
```

If transcript fails:

```text
Transcript unavailable
        ↓
Video can remain available
```

Critical functionality should be separated from optional enhancements where practical.

---

# 81. System Boundaries

### Inside AILPG

```text
Authentication
Users
Courses
Lessons
Questions
Reviews
Analytics
Job Management
Permissions
```

### External dependencies

```text
AI Providers
Object Storage Provider
CDN
Email Provider
Payment Provider
Optional Authentication Providers
```

External integrations should be abstracted behind internal interfaces.

---

# 82. External AI Provider Boundary

```text
AILPG
 │
 ▼
AI Adapter
 │
 ├── Provider A
 ├── Provider B
 └── Provider C
```

Provider-specific request formats should not leak into the main business domain.

---

# 83. Testing Architecture

Testing layers:

```text
Unit Tests
    ↓
Integration Tests
    ↓
API Tests
    ↓
AI Pipeline Tests
    ↓
End-to-End Tests
    ↓
Accessibility Tests
    ↓
Performance Tests
    ↓
Security Tests
```

---

# 84. AI Testing

AI output should be tested for:

```text
Schema validity
Math correctness
Timestamp validity
Question quality
Answer consistency
Translation completeness
Content safety
Accessibility metadata
```

---

# 85. Video Testing

Test:

```text
Small MP4
Large MP4
Long video
Short video
Different resolutions
Different codecs
Corrupted file
Missing audio
Poor audio
High frame rate
```

---

# 86. Student Experience Testing

Test:

```text
Start lesson
Pause video
Question appears
Answer question
Receive feedback
Resume video
Skip question where allowed
Change language
Change quality
Open transcript
Complete lesson
```

---

# 87. Performance Testing

Measure:

```text
Upload throughput
Video processing time
AI processing time
API latency
Question response time
Lesson load time
Video start time
Analytics query time
```

---

# 88. Security Testing

Test:

```text
Unauthorized lesson access
Unauthorized video access
Subscription bypass
Question answer-key exposure
Admin endpoint access
Tenant isolation
Malicious uploads
XSS
SQL injection
API abuse
```

---

# 89. Observability Dashboard

Administrators should be able to monitor:

```text
API Health
Queue Health
AI Jobs
Video Processing
Storage
Database
Errors
Latency
Costs
```

---

# 90. System Health Model

```text
                    SYSTEM HEALTH
                         │
       ┌─────────────────┼─────────────────┐
       ▼                 ▼                 ▼
      API              Workers          Database
       │                 │                 │
       ▼                 ▼                 ▼
    Latency          Queue Depth        Queries
    Errors           Failures           Connections
```

---

# 91. Recommended Repository Structure

```text
ailpg-blueprint/
│
├── docs/
│   ├── 01_PROJECT_CHARTER/
│   ├── 02_SRS/
│   ├── 03_INFORMATION_ARCHITECTURE/
│   ├── 04_UI_UX_BLUEPRINT/
│   ├── 05_SYSTEM_DESIGN/
│   ├── 06_TECHNICAL_ARCHITECTURE/
│   ├── 07_DATABASE_DESIGN/
│   ├── 08_API_DESIGN/
│   ├── 09_AI_WORKFLOW/
│   ├── 10_DEPLOYMENT/
│   └── 11_TESTING/
│
├── frontend/
├── backend/
├── ai/
├── workers/
├── infrastructure/
├── scripts/
└── README.md
```

---

# 92. System Design Dependencies

This document connects the following areas:

```text
SRS
 ↓
System Design
 ↓
Technical Architecture
 ↓
Database
 ↓
API
 ↓
AI Workflow
 ↓
Deployment
```

The UI/UX layer is implemented against the interfaces defined by these engineering documents.

---

# 93. Key Design Decisions

### Decision 1 — Asynchronous AI Processing

Long-running AI operations use background jobs.

### Decision 2 — Object Storage

Large media files are stored outside the relational database.

### Decision 3 — Human Review

AI-generated educational content can pass through a human review stage before publication.

### Decision 4 — Versioned Lessons

Published lessons reference stable versions.

### Decision 5 — Backend Authorization

Permissions and subscription entitlements are enforced server-side.

### Decision 6 — Modular Backend

Business domains are separated logically to support future scaling.

---

# 94. Future Scalability

The initial architecture should allow future extraction of independent services.

Potential future services:

```text
Video Processing Service
AI Processing Service
Question Service
Translation Service
Analytics Service
Notification Service
```

These should only be separated when actual scale or operational requirements justify the additional complexity.

---

# 95. Final System Architecture

```text
                         ┌──────────────────┐
                         │      USERS       │
                         └────────┬─────────┘
                                  │
                                  ▼
                         ┌──────────────────┐
                         │   WEB / MOBILE   │
                         └────────┬─────────┘
                                  │
                                  ▼
                         ┌──────────────────┐
                         │   API / AUTH     │
                         └────────┬─────────┘
                                  │
              ┌───────────────────┼───────────────────┐
              │                   │                   │
              ▼                   ▼                   ▼
        ┌───────────┐       ┌───────────┐       ┌───────────┐
        │  Courses  │       │  Videos   │       │ Analytics │
        └─────┬─────┘       └─────┬─────┘       └───────────┘
              │                   │
              │                   ▼
              │             ┌───────────┐
              │             │   Queue   │
              │             └─────┬─────┘
              │                   │
              │          ┌────────┼────────┐
              │          ▼        ▼        ▼
              │       Video      AI     Translation
              │      Worker    Worker     Worker
              │          │        │
              │          └────┬───┘
              │               ▼
              │        ┌──────────────┐
              │        │ Lesson Engine│
              │        └──────┬───────┘
              │               ▼
              │        ┌──────────────┐
              └───────►│ Human Review │
                       └──────┬───────┘
                              ▼
                       ┌──────────────┐
                       │   Publish    │
                       └──────┬───────┘
                              ▼
                       ┌──────────────┐
                       │Student Player│
                       └──────┬───────┘
                              ▼
                       ┌──────────────┐
                       │  Questions   │
                       └──────┬───────┘
                              ▼
                       ┌──────────────┐
                       │   Analytics  │
                       └──────────────┘

        Shared Infrastructure
        ┌─────────────────────────────────────┐
        │ Database │ Cache │ Object Storage   │
        │ CDN      │ Queue │ Observability    │
        └─────────────────────────────────────┘
```

---

# 96. Definition of Done

The System Overview is considered complete when:

* [ ] All major system actors are defined.
* [ ] Core platform modules are identified.
* [ ] Synchronous and asynchronous workloads are separated.
* [ ] Video-processing pipeline is defined.
* [ ] AI pipeline is defined.
* [ ] Human review flow is defined.
* [ ] Course and lesson architecture is defined.
* [ ] Student playback architecture is defined.
* [ ] Subscription-aware media access is defined.
* [ ] Database responsibility is defined.
* [ ] Object-storage responsibility is defined.
* [ ] Queue architecture is defined.
* [ ] Event architecture is defined.
* [ ] Security boundaries are defined.
* [ ] Authorization flow is defined.
* [ ] Analytics architecture is defined.
* [ ] Observability requirements are defined.
* [ ] Deployment architecture is defined.
* [ ] Scalability approach is defined.
* [ ] Testing architecture is identified.
* [ ] Failure and retry behavior is defined.
* [ ] Major external dependencies are identified.

---

# 97. Next System Design Documents

The System Design sequence should continue with:

```text
01_System_Overview.md
02_System_Architecture.md
03_Module_Architecture.md
04_Data_Flow.md
05_Video_Processing_Architecture.md
06_AI_Processing_Architecture.md
07_Lesson_Generation_Architecture.md
08_Question_Engine_Architecture.md
09_User_And_Role_Architecture.md
10_Subscription_Architecture.md
11_Analytics_Architecture.md
12_Security_Architecture.md
13_Integration_Architecture.md
14_Event_And_Queue_Architecture.md
15_System_Design_Appendix.md
```

---

**End of `01_System_Overview.md`**
