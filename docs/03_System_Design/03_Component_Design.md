---
document_id: SD-003
title: Component Design
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Solution Architecture Team
parent_document: SD-002
last_updated: 2026-08-11
---

# Component Design

## 1. Purpose

This document defines the internal components of the AILPG platform.

Each component is defined by:

- Purpose
- Responsibilities
- Inputs
- Outputs
- Dependencies
- APIs
- Database interactions
- Events
- Failure handling
- Security requirements

---

# 2. Component Classification

AILPG components are grouped into:

```text
Presentation
│
├── Student Portal
├── Teacher Portal
└── Admin Portal

Application
│
├── Authentication
├── User Management
├── Organization Management
├── Course Management
├── Lesson Management
├── Video Management
├── AI Orchestration
├── Question Management
├── Translation
├── Learning
├── Analytics
├── Subscription
├── Notification
└── Administration

Infrastructure
│
├── PostgreSQL
├── Redis
├── Queue
├── Object Storage
├── CDN
└── Observability

Workers
│
├── Video Worker
├── STT Worker
├── OCR Worker
├── Formula Worker
├── Quiz Worker
├── Translation Worker
└── Lesson Worker
```

---

# 3. Component Dependency Overview

```text
                    ┌──────────────┐
                    │   Frontend   │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │ API Gateway  │
                    └──────┬───────┘
                           │
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
      Auth              Content            Learning
        │                  │                  │
        └──────────────────┼──────────────────┘
                           ▼
                    ┌──────────────┐
                    │ PostgreSQL   │
                    └──────────────┘

Content
   │
   ▼
AI Orchestrator
   │
   ▼
Queue
   │
   ├── Video Worker
   ├── STT Worker
   ├── OCR Worker
   ├── Formula Worker
   ├── Quiz Worker
   ├── Translation Worker
   └── Lesson Worker
```

---

# 4. Student Portal

## Purpose

Provides the learning experience for students.

## Responsibilities

```text
Login
Course browsing
Lesson access
Video playback
Interactive questions
Answer submission
Progress display
Language selection
Profile management
```

## Inputs

```text
User actions
Lesson manifest
Video stream
Question data
Progress data
```

## Outputs

```text
API requests
Learning events
Quiz answers
Progress events
```

## Dependencies

```text
Authentication API
Course API
Lesson API
Learning API
Video CDN
Translation API
```

---

# 5. Teacher Portal

## Purpose

Provides content creation and review capabilities.

## Responsibilities

```text
Course creation
Lesson creation
MP4 upload
Processing monitoring
AI result review
Transcript editing
Formula editing
Question editing
Translation review
Preview
Publishing
Analytics
```

## Dependencies

```text
Auth
Course
Lesson
Video
AI
Translation
Analytics
```

---

# 6. Admin Portal

## Purpose

Provides platform-level management.

## Responsibilities

```text
User management
Organization management
AI job monitoring
Failed job retry
Subscription management
Audit logs
System monitoring
Usage monitoring
```

## Dependencies

```text
Admin API
Auth
Users
Organizations
AI Jobs
Subscriptions
Observability
```

---

# 7. API Gateway

## Purpose

Single API entry point for frontend applications.

## Responsibilities

```text
Routing
Authentication
Authorization
Validation
Rate limiting
CORS
Request logging
API versioning
Error normalization
```

## Input

```text
HTTP/HTTPS request
```

## Output

```text
JSON response
```

## Dependencies

```text
Auth
Application modules
Database
Redis
Queue
```

---

# 8. Authentication Component

## Responsibilities

```text
Registration
Login
Logout
Password reset
Session management
Token validation
Role verification
```

## Inputs

```text
Email
Password
Session token
Refresh token
```

## Outputs

```text
Authenticated identity
Access token
Session
Authentication error
```

## Security

```text
Password hashing
Token expiration
Session invalidation
Brute-force protection
Rate limiting
```

---

# 9. User Management Component

## Responsibilities

```text
Create user
Update profile
Deactivate user
Manage preferences
Manage language
Manage roles
```

## Dependencies

```text
Authentication
Organization
Database
Audit
```

---

# 10. Organization Component

## Responsibilities

```text
Create organization
Update organization
Manage members
Manage roles
Manage settings
Track usage
```

## Data

```text
Organization
Membership
OrganizationSettings
Usage
```

---

# 11. Course Component

## Responsibilities

```text
Create course
Update course
Delete course
Create chapters
Create lessons
Reorder content
Publish course
Archive course
```

## Dependencies

```text
Organization
Lesson
User
Database
```

---

# 12. Lesson Component

## Responsibilities

```text
Create lesson
Update lesson
Version lesson
Attach video
Attach transcript
Attach questions
Attach translations
Create manifest
Publish lesson
Archive lesson
```

---

# 13. Video Component

## Responsibilities

```text
Upload initialization
Upload completion
File validation
Metadata extraction
Processing state
Transcoding state
Streaming URL
Thumbnail management
```

## Inputs

```text
MP4
File metadata
Course ID
Lesson ID
```

## Outputs

```text
Video ID
Upload URL
Processing Job ID
Video metadata
Streaming URLs
```

---

# 14. Object Storage Component

## Purpose

Stores large files and generated assets.

## Stores

```text
Original videos
Processed videos
HLS segments
Thumbnails
Extracted frames
Audio
AI artifacts
Lesson assets
Exports
```

## Rules

```text
No business logic
No direct public write access
Private by default
Signed access for protected content
```

---

# 15. Video Processing Component

## Responsibilities

```text
Metadata extraction
Audio extraction
Frame extraction
Transcoding
Quality generation
Thumbnail generation
HLS generation
```

## Input

```text
Original MP4
```

## Output

```text
Audio
Frames
Thumbnails
HLS
Video metadata
```

---

# 16. AI Orchestrator

## Purpose

Controls the complete AI processing pipeline.

## Responsibilities

```text
Create AI jobs
Track stages
Schedule workers
Manage dependencies
Handle retries
Store results
Calculate confidence
Trigger next stage
```

## Example

```text
Video Uploaded
      ↓
Create Pipeline
      ↓
STT
      ↓
OCR
      ↓
Formula
      ↓
Topic
      ↓
Quiz
      ↓
Translation
      ↓
Lesson
```

---

# 17. AI Provider Manager

## Purpose

Abstracts external AI providers.

```text
Application
     ↓
AI Provider Interface
     ↓
Provider Manager
     ├── STT Provider
     ├── LLM Provider
     ├── Vision Provider
     └── Translation Provider
```

## Benefits

```text
Provider replacement
Fallback provider
Cost optimization
Model upgrades
Testing
```

---

# 18. STT Component

## Purpose

Converts spoken audio into timestamped text.

## Input

```text
Audio
```

## Output

```text
Transcript
Word timestamps
Segment timestamps
Language
Confidence
```

Example:

```json
{
  "start": 10.2,
  "end": 14.8,
  "text": "Let us solve this equation.",
  "confidence": 0.96
}
```

---

# 19. OCR Component

## Purpose

Extracts visible text from video frames.

## Input

```text
Video frames
```

## Output

```text
Detected text
Bounding boxes
Timestamp
Confidence
```

---

# 20. Formula Recognition Component

## Purpose

Detects mathematical expressions.

## Input

```text
Video frames
OCR output
```

## Output

```text
Formula
LaTeX
Timestamp
Confidence
```

Example:

```text
x² + 5x + 6 = 0

↓

x^2 + 5x + 6 = 0
```

---

# 21. Topic Analysis Component

## Purpose

Identifies concepts and lesson structure.

## Input

```text
Transcript
OCR
Formula
```

## Output

```text
Topics
Subtopics
Keywords
Learning objectives
```

---

# 22. Quiz Generation Component

## Purpose

Creates interactive questions from learning content.

## Inputs

```text
Transcript
Topics
Formulas
Lesson objectives
```

## Output

```text
MCQ
True/False
Multiple selection
Short answer
Formula-based question
```

---

# 23. Question Component

## Responsibilities

```text
Create question
Edit question
Validate question
Store answers
Store explanation
Manage difficulty
Manage checkpoint
```

## Question structure

```text
Question
├── Prompt
├── Options
├── Correct Answer
├── Explanation
├── Difficulty
├── Timestamp
└── Learning Objective
```

---

# 24. Translation Component

## Responsibilities

```text
Translate transcript
Translate questions
Translate explanations
Translate UI content
Maintain source language
Maintain target language
```

---

# 25. Lesson Generator Component

## Purpose

Combines all processed information into a lesson.

## Inputs

```text
Video
Transcript
OCR
Formula
Topics
Questions
Translations
```

## Output

```text
Lesson Manifest
```

---

# 26. Lesson Manifest Component

The manifest is the machine-readable representation of the interactive lesson.

Conceptual structure:

```json
{
  "lesson_id": "lesson_001",
  "version": 1,
  "video": {},
  "transcript": [],
  "topics": [],
  "formulas": [],
  "checkpoints": [],
  "questions": [],
  "translations": []
}
```

---

# 27. Review Component

## Purpose

Allows teachers to validate AI-generated content.

## Review Areas

```text
Transcript
OCR
Formula
Topics
Questions
Translations
Timeline
```

## Actions

```text
Accept
Edit
Reject
Regenerate
Approve
```

---

# 28. Publishing Component

## Responsibilities

```text
Validate lesson
Validate required assets
Create publishable version
Generate manifest
Update lesson state
Invalidate cache
Notify students
```

Publishing states:

```text
DRAFT
REVIEW
APPROVED
PUBLISHED
ARCHIVED
```

---

# 29. Interactive Player Component

## Responsibilities

```text
Video playback
Timeline
Checkpoint detection
Question display
Answer handling
Feedback
Transcript synchronization
Language switching
Progress tracking
```

---

# 30. Checkpoint Engine

## Input

```text
Current video timestamp
Lesson checkpoints
```

## Output

```text
Checkpoint triggered
```

Example:

```text
Video time = 120 sec

Checkpoint:
start = 118
end = 120

↓

Trigger question
```

---

# 31. Learning Component

## Responsibilities

```text
Record progress
Record attempts
Calculate completion
Track lesson status
Track course progress
```

---

# 32. Analytics Component

## Responsibilities

```text
Collect events
Aggregate metrics
Calculate KPIs
Generate reports
Provide dashboards
```

## Events

```text
VIDEO_STARTED
VIDEO_COMPLETED
CHECKPOINT_REACHED
QUESTION_ANSWERED
LESSON_COMPLETED
```

---

# 33. Subscription Component

## Responsibilities

```text
Plans
Subscriptions
Entitlements
Usage
Billing state
Access validation
```

---

# 34. Notification Component

## Channels

```text
Email
In-app
Push
```

## Events

```text
AI_COMPLETED
REVIEW_REQUIRED
LESSON_PUBLISHED
PAYMENT_FAILED
SUBSCRIPTION_EXPIRING
```

---

# 35. Admin Component

## Responsibilities

```text
Platform configuration
User management
Job monitoring
System monitoring
Audit management
Subscription management
```

---

# 36. Audit Component

Every sensitive action should create an audit record.

Example:

```json
{
  "actor_id": "user_123",
  "action": "LESSON_PUBLISHED",
  "resource_type": "lesson",
  "resource_id": "lesson_001",
  "timestamp": "..."
}
```

---

# 37. Redis Component

Used for:

```text
Cache
Rate limiting
Locks
Temporary state
Session-related state
```

Not used as the permanent source of truth.

---

# 38. Queue Component

The queue connects the API/application layer with background processing.

```text
Application
    ↓
Queue
    ↓
Worker
    ↓
Result
    ↓
Database
```

---

# 39. Worker Components

## Video Worker

```text
Video
→ metadata
→ audio
→ frames
→ transcoding
→ HLS
```

## STT Worker

```text
Audio
→ transcript
```

## OCR Worker

```text
Frames
→ text
```

## Formula Worker

```text
Frames + OCR
→ formulas
```

## Quiz Worker

```text
Learning content
→ questions
```

## Translation Worker

```text
Content
→ translations
```

## Lesson Worker

```text
All AI results
→ lesson manifest
```

---

# 40. Worker Failure Handling

```text
Worker
  ↓
Failure
  ↓
Retry
  ↓
Failure
  ↓
Retry
  ↓
Failure
  ↓
Dead Letter Queue
  ↓
Admin Review
```

---

# 41. Component Communication

Synchronous:

```text
Frontend
   ↓
API
   ↓
Application Module
```

Asynchronous:

```text
Application
   ↓
Queue
   ↓
Worker
```

Event-based:

```text
Worker
   ↓
Job Completed Event
   ↓
Next Worker
```

---

# 42. Component Ownership

| Component | Owner |
|---|---|
| Student Portal | Frontend |
| Teacher Portal | Frontend |
| Admin Portal | Frontend |
| API Gateway | Backend |
| Auth | Backend |
| Courses | Backend |
| Lessons | Backend |
| Videos | Backend + Media |
| AI Orchestrator | AI Platform |
| STT | AI Worker |
| OCR | AI Worker |
| Formula | AI Worker |
| Quiz | AI Worker |
| Translation | AI Worker |
| Learning | Backend |
| Analytics | Backend/Data |
| Subscription | Backend |
| Database | Infrastructure |
| Storage | Infrastructure |
| Queue | Infrastructure |

---

# 43. Component Security

Every backend component must enforce:

```text
Authentication
Authorization
Tenant validation
Input validation
Audit where required
```

---

# 44. Component Observability

Each component should produce:

```text
Logs
Metrics
Errors
Trace ID
Request ID
Job ID
```

Workers additionally require:

```text
Job duration
Retry count
Provider latency
Provider errors
```

---

# 45. Component Testing

Each component requires:

```text
Unit tests
Integration tests
Failure tests
Security tests
```

AI components additionally require:

```text
Quality evaluation
Regression tests
Prompt tests
Provider fallback tests
```

---

# 46. MVP Component Set

The first implementation must prioritize:

```text
Student Portal
Teacher Portal
Admin Portal

Auth
Users
Courses
Lessons
Videos
AI Orchestrator
STT
OCR
Formula
Quiz
Translation
Lesson Generator
Review
Learning
PostgreSQL
Object Storage
Queue
Redis
```

---

# 47. Future Components

Future releases may add:

```text
AI Tutor
Recommendation Engine
Advanced Analytics
Live Classes
Mobile Apps
Offline Learning
Advanced Search
Content Marketplace
AI Voice Tutor
Automatic Curriculum Generator
```

These are outside the initial MVP.

---

# 48. Component Dependency Matrix

| Component | DB | Queue | Storage | AI | Redis |
|---|---:|---:|---:|---:|---:|
| Auth | ✓ | - | - | - | ✓ |
| Users | ✓ | - | - | - | ✓ |
| Courses | ✓ | - | - | - | ✓ |
| Lessons | ✓ | ✓ | ✓ | ✓ | ✓ |
| Videos | ✓ | ✓ | ✓ | - | ✓ |
| AI Orchestrator | ✓ | ✓ | ✓ | ✓ | ✓ |
| STT Worker | ✓ | ✓ | ✓ | ✓ | - |
| OCR Worker | ✓ | ✓ | ✓ | ✓ | - |
| Formula Worker | ✓ | ✓ | ✓ | ✓ | - |
| Quiz Worker | ✓ | ✓ | - | ✓ | - |
| Translation | ✓ | ✓ | - | ✓ | - |
| Learning | ✓ | - | - | - | ✓ |
| Analytics | ✓ | ✓ | - | - | - |
| Subscription | ✓ | - | - | - | ✓ |
| Admin | ✓ | ✓ | - | - | ✓ |

---

# 49. Component Design Rules

### Rule 1

Components should have one clear responsibility.

### Rule 2

AI providers must remain replaceable.

### Rule 3

Long-running processing must be asynchronous.

### Rule 4

Database access should happen through defined repositories/services.

### Rule 5

Frontend must not contain business-critical authorization logic.

### Rule 6

All published lessons must be versioned.

### Rule 7

AI-generated content must support human review.

### Rule 8

Workers must support retries.

### Rule 9

Critical data must not depend on Redis.

### Rule 10

Tenant boundaries must be enforced server-side.

---

# 50. Component Definition of Done

A component is considered designed when:

```text
Purpose
✓
Responsibilities
✓
Inputs
✓
Outputs
✓
Dependencies
✓
Data
✓
APIs
✓
Events
✓
Security
✓
Observability
✓
Failure handling
✓
Testing
✓
```

---

# 51. Next Document

The next file is:

```text
04_Data_Flow_Design.md
```

It will define the exact movement of data through the platform:

```text
MP4
 ↓
Upload
 ↓
Storage
 ↓
Video Job
 ↓
Audio / Frames
 ↓
STT / OCR / Formula
 ↓
AI Results
 ↓
Quiz
 ↓
Translation
 ↓
Lesson Manifest
 ↓
Teacher Review
 ↓
Publish
 ↓
Student
 ↓
Progress
 ↓
Analytics
```

---

# 52. Revision History

| Version | Date | Description |
|---|---|---|
| 1.0.0 | 2026-08-11 | Initial component design |
