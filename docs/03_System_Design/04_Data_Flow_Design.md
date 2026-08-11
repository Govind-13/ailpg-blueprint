---
document_id: SD-004
title: Data Flow Design
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Solution Architecture Team
parent_document: SD-003
last_updated: 2026-08-11
---

# Data Flow Design

## 1. Purpose

This document defines how data moves through the AILPG platform.

The primary flow is:

```text
MP4
 ↓
Upload
 ↓
Validation
 ↓
Storage
 ↓
Video Processing
 ↓
Audio / Frames
 ↓
AI Analysis
 ↓
Transcript / OCR / Formula
 ↓
Topic Analysis
 ↓
Question Generation
 ↓
Translation
 ↓
Lesson Manifest
 ↓
Teacher Review
 ↓
Publish
 ↓
Student Learning
 ↓
Progress
 ↓
Analytics
```

---

# 2. Data Flow Principles

The platform follows these principles:

```text
1. Large files use object storage.
2. Business data uses PostgreSQL.
3. Long-running work is asynchronous.
4. AI outputs are versioned.
5. Processing stages are independently retryable.
6. Published lessons are immutable versions.
7. Student activity is captured as events.
8. Tenant boundaries are enforced.
9. Sensitive operations are audited.
10. AI output is reviewable before publishing.
```

---

# 3. Complete End-to-End Flow

```text
                    TEACHER
                       │
                       ▼
                Teacher Portal
                       │
                       ▼
                 Upload API
                       │
              ┌────────┴────────┐
              ▼                 ▼
        PostgreSQL         Object Storage
              │                 │
              └────────┬────────┘
                       ▼
                    Job Queue
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
      Video           STT            OCR
      Worker         Worker         Worker
        │              │              │
        ▼              ▼              ▼
      Video         Transcript       OCR
      Assets                         Data
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                Formula Analysis
                       │
                       ▼
                 Topic Analysis
                       │
                       ▼
                Quiz Generation
                       │
                       ▼
                  Translation
                       │
                       ▼
              Lesson Generation
                       │
                       ▼
                 Review Workspace
                       │
                       ▼
                    Publish
                       │
                       ▼
               Student Player
                       │
                       ▼
                 Learning Events
                       │
                       ▼
                  Analytics
```

---

# 4. Data Classification

Data is classified into:

## 4.1 User Data

```text
User profile
Email
Role
Preferences
Language
Organization membership
```

## 4.2 Course Data

```text
Course
Chapter
Lesson
Metadata
Publishing status
```

## 4.3 Media Data

```text
Original MP4
Processed video
Audio
Frames
Thumbnails
HLS segments
```

## 4.4 AI Data

```text
Transcript
OCR
Formula
Topics
Questions
Translations
Confidence
AI metadata
```

## 4.5 Learning Data

```text
Progress
Attempts
Answers
Completion
Bookmarks
Events
```

## 4.6 Billing Data

```text
Plan
Subscription
Entitlement
Usage
Payment status
```

---

# 5. MP4 Upload Flow

## Step 1 — Create Upload

Teacher requests an upload session.

```text
Teacher
   ↓
POST /api/v1/videos/upload-session
```

Backend validates:

```text
Authentication
Authorization
Organization
Course
Lesson
File metadata
```

---

# 6. Upload Session Response

Backend returns:

```text
Video ID
Upload ID
Signed Upload URL
Expiration
```

Example:

```json
{
  "video_id": "video_001",
  "upload_id": "upload_001",
  "upload_url": "...",
  "expires_at": "..."
}
```

---

# 7. Direct File Upload

The browser uploads directly to object storage.

```text
Teacher Browser
      │
      │ MP4
      ▼
Object Storage
```

The backend does not need to proxy the entire video.

---

# 8. Upload Completion

After upload:

```text
Client
  ↓
POST /api/v1/videos/upload-complete
  ↓
Backend
  ↓
Validate Object
  ↓
Create Processing Job
```

---

# 9. Video Validation Flow

```text
Uploaded MP4
      ↓
File Exists?
      ↓
Size Valid?
      ↓
Extension Valid?
      ↓
Container Valid?
      ↓
Video Stream Exists?
      ↓
Audio Stream?
      ↓
Duration Valid?
      ↓
Accepted
```

Failure:

```text
Invalid
  ↓
Upload Rejected
  ↓
Error Recorded
```

---

# 10. Video Metadata Flow

The Video Worker extracts:

```text
Duration
Width
Height
Frame rate
Codec
Audio codec
Bitrate
File size
```

Stored in:

```text
videos
video_metadata
```

---

# 11. Video Processing Flow

```text
Original MP4
     │
     ├──────────────► Metadata
     │
     ├──────────────► Audio
     │
     ├──────────────► Frames
     │
     ├──────────────► Thumbnail
     │
     └──────────────► Streaming Format
```

---

# 12. Audio Extraction

```text
MP4
 ↓
Audio Extraction
 ↓
Normalized Audio
 ↓
STT Queue
```

The extracted audio should be stored temporarily or retained according to processing requirements.

---

# 13. Frame Extraction

```text
MP4
 ↓
Frame Sampling
 ↓
Frames
 ↓
OCR Queue
 ↓
Formula Queue
```

Frame sampling strategy can depend on:

```text
Video duration
Scene changes
Visual changes
Speech timestamps
Content type
```

---

# 14. STT Data Flow

```text
Audio
  ↓
STT Worker
  ↓
Speech Recognition
  ↓
Transcript
  ↓
Timestamp Normalization
  ↓
Confidence Validation
  ↓
PostgreSQL
```

Example:

```json
{
  "segment_id": "seg_001",
  "start": 12.4,
  "end": 17.8,
  "text": "Now we solve the equation.",
  "confidence": 0.95
}
```

---

# 15. OCR Data Flow

```text
Frames
  ↓
OCR Worker
  ↓
Text Detection
  ↓
Bounding Box Detection
  ↓
Timestamp Mapping
  ↓
Confidence
  ↓
OCR Result
```

---

# 16. Formula Data Flow

```text
Frames
   +
OCR
   ↓
Formula Worker
   ↓
Mathematical Region Detection
   ↓
Formula Recognition
   ↓
LaTeX Normalization
   ↓
Confidence
   ↓
Formula Result
```

Example:

```text
Visual:

x² + 5x + 6 = 0

Stored:

x^2 + 5x + 6 = 0
```

---

# 17. Topic Analysis Flow

Inputs:

```text
Transcript
OCR
Formula
```

Processing:

```text
Content Normalization
       ↓
Semantic Analysis
       ↓
Topic Extraction
       ↓
Subtopic Extraction
       ↓
Learning Objective
```

Output:

```json
{
  "topic": "Quadratic Equations",
  "subtopics": [
    "Factorization",
    "Roots"
  ]
}
```

---

# 18. Question Generation Flow

```text
Transcript
   +
Topics
   +
Formulas
   +
Learning Objectives
        ↓
Question Generator
        ↓
Question Validation
        ↓
Difficulty Classification
        ↓
Question Database
```

---

# 19. Question Validation

Generated questions must pass:

```text
Question completeness
Answer existence
Correct answer validity
Option uniqueness
Formula correctness
Language validation
Difficulty validation
```

Invalid questions:

```text
Reject
  ↓
Regenerate
```

---

# 20. Checkpoint Generation

Questions are mapped to video timestamps.

```text
Transcript Segment
       ↓
Concept Boundary
       ↓
Question
       ↓
Timestamp
       ↓
Interactive Checkpoint
```

Example:

```text
Video:
00:00 ─────────────── 02:30 ─────────────── 05:00

                       ▲
                       │
                  Question
                  Checkpoint
```

---

# 21. Translation Flow

```text
Approved Source Content
       ↓
Translation Queue
       ↓
Target Language
       ↓
Translation Provider
       ↓
Validation
       ↓
Translated Content
```

Example:

```text
English
   ↓
Tamil
Hindi
Malayalam
Telugu
Kannada
```

---

# 22. Translation Rules

The translation pipeline must preserve:

```text
Meaning
Formula
Variables
Numbers
Timestamps
Question structure
Answer structure
Formatting
```

Mathematical expressions should not be translated as normal text.

Example:

```text
x² + 5x + 6 = 0
```

must remain mathematically identical.

---

# 23. Lesson Generation Flow

The Lesson Generator consumes:

```text
Video Metadata
Transcript
OCR
Formula
Topics
Questions
Translations
```

and creates:

```text
Lesson Version
Lesson Manifest
Interactive Checkpoints
```

---

# 24. Lesson Manifest Flow

```text
Processed Content
      ↓
Normalize
      ↓
Validate
      ↓
Build Manifest
      ↓
Schema Validation
      ↓
Store Manifest
```

---

# 25. Lesson Manifest Example

```json
{
  "lesson_id": "lesson_001",
  "version": 1,
  "title": "Quadratic Equations",
  "video": {
    "duration": 600
  },
  "transcript": [],
  "topics": [],
  "formulas": [],
  "checkpoints": [],
  "questions": [],
  "translations": []
}
```

---

# 26. Teacher Review Flow

```text
AI Generated
     ↓
READY_FOR_REVIEW
     ↓
Teacher Opens Review
     ↓
Transcript Review
     ↓
Formula Review
     ↓
Question Review
     ↓
Translation Review
     ↓
Timeline Review
     ↓
Approve
```

Teacher can:

```text
Edit
Delete
Regenerate
Move checkpoint
Change answer
Change explanation
Change translation
```

---

# 27. Publish Flow

```text
Teacher Approves
       ↓
Final Validation
       ↓
Create Immutable Version
       ↓
Generate Production Manifest
       ↓
Publish
       ↓
CDN Cache
       ↓
Student Access
```

---

# 28. Published Lesson Data

Published content should reference immutable versions.

```text
Lesson
 │
 ├── Version 1
 ├── Version 2
 └── Version 3
```

Only one version should be active for normal student access at a given time.

---

# 29. Student Access Flow

```text
Student
  ↓
Open Course
  ↓
Open Lesson
  ↓
Authentication
  ↓
Authorization
  ↓
Entitlement Check
  ↓
Lesson Manifest
  ↓
Signed Video URL
  ↓
Player
```

---

# 30. Video Playback Flow

```text
Student
  ↓
Lesson Player
  ↓
Request Video
  ↓
Authorization
  ↓
Signed CDN URL
  ↓
CDN
  ↓
Video Segments
```

The application server should not become the video streaming bottleneck.

---

# 31. Interactive Checkpoint Flow

```text
Video Player
     ↓
Current Timestamp
     ↓
Checkpoint Engine
     ↓
Checkpoint Found
     ↓
Pause Video
     ↓
Display Question
     ↓
Student Answer
     ↓
Validate Answer
     ↓
Display Feedback
     ↓
Record Event
     ↓
Resume Video
```

---

# 32. Answer Data Flow

```text
Student Answer
      ↓
API
      ↓
Authentication
      ↓
Authorization
      ↓
Question Validation
      ↓
Answer Evaluation
      ↓
Attempt Stored
      ↓
Feedback
```

---

# 33. Progress Data Flow

```text
Player
  ↓
Learning Events
  ↓
Progress Service
  ↓
Calculate Progress
  ↓
PostgreSQL
```

Example:

```text
Lesson Progress = 75%
Course Progress = 42%
```

---

# 34. Learning Event Flow

Events include:

```text
LESSON_OPENED
VIDEO_STARTED
VIDEO_PAUSED
VIDEO_SEEKED
CHECKPOINT_REACHED
QUESTION_STARTED
QUESTION_ANSWERED
VIDEO_COMPLETED
LESSON_COMPLETED
```

Flow:

```text
Student Action
      ↓
Event
      ↓
Learning API
      ↓
Event Validation
      ↓
Event Storage
      ↓
Progress Update
      ↓
Analytics
```

---

# 35. Analytics Data Flow

MVP:

```text
Learning Events
      ↓
PostgreSQL
      ↓
Analytics Queries
      ↓
Dashboard
```

Future:

```text
Learning Events
      ↓
Event Queue
      ↓
Analytics Pipeline
      ↓
Warehouse
      ↓
BI / Dashboard
```

---

# 36. AI Job Data Flow

Each AI processing stage produces a job record.

```text
AI Job
├── ID
├── Type
├── Status
├── Input
├── Output
├── Error
├── Retry Count
├── Started At
├── Completed At
└── Provider Metadata
```

---

# 37. Job Dependency Flow

```text
VIDEO
  │
  ├──► AUDIO ──► STT
  │
  └──► FRAMES
          │
          ├──► OCR
          │
          └──► FORMULA

STT + OCR + FORMULA
          ↓
        TOPIC
          ↓
       QUESTION
          ↓
     TRANSLATION
          ↓
        LESSON
```

---

# 38. Retry Data Flow

```text
Worker
  ↓
Failure
  ↓
Record Error
  ↓
Increment Retry Count
  ↓
Queue Retry
  ↓
Worker
```

Maximum retry count must be configurable.

---

# 39. Dead Letter Flow

```text
Job
 ↓
Failure
 ↓
Retry 1
 ↓
Retry 2
 ↓
Retry 3
 ↓
Dead Letter Queue
 ↓
Admin Dashboard
 ↓
Manual Retry / Cancel
```

---

# 40. Error Data Flow

```text
Error
 ↓
Structured Error
 ↓
Job Record
 ↓
Application Log
 ↓
Monitoring
 ↓
Admin Alert
```

Error should contain:

```text
Error Code
Message
Job ID
Request ID
Component
Timestamp
Retryable
```

---

# 41. Storage Data Flow

## Original Video

```text
Browser
 ↓
Object Storage
 ↓
Original Video
```

## Processed Assets

```text
Video Worker
 ↓
Object Storage
 ↓
Processed Assets
```

## AI Artifacts

```text
AI Worker
 ↓
Object Storage / PostgreSQL
```

---

# 42. Database Data Flow

Business transactions:

```text
API
 ↓
Service
 ↓
Repository
 ↓
PostgreSQL
```

Workers:

```text
Worker
 ↓
Repository
 ↓
PostgreSQL
```

---

# 43. Cache Data Flow

```text
Request
 ↓
Redis
 │
 ├── HIT → Return Cached Data
 │
 └── MISS
       ↓
    PostgreSQL
       ↓
    Redis
       ↓
    Return
```

Only suitable non-critical data should be cached.

---

# 44. Subscription Data Flow

```text
Student
 ↓
Open Premium Lesson
 ↓
API
 ↓
Authentication
 ↓
Subscription Lookup
 ↓
Entitlement Check
 │
 ├── Allowed → Continue
 │
 └── Denied → Access Response
```

---

# 45. Multi-Tenant Data Flow

```text
Request
 ↓
Authenticated User
 ↓
Organization Membership
 ↓
Resource Organization
 ↓
Tenant Validation
 ↓
Business Operation
```

Cross-tenant access must be rejected.

---

# 46. Audit Data Flow

Sensitive action:

```text
Admin
 ↓
API
 ↓
Authorization
 ↓
Business Action
 ↓
Audit Event
 ↓
Audit Log
```

Examples:

```text
ROLE_CHANGED
LESSON_PUBLISHED
LESSON_DELETED
SUBSCRIPTION_CHANGED
AI_JOB_RETRIED
```

---

# 47. Notification Data Flow

```text
System Event
      ↓
Notification Queue
      ↓
Notification Worker
      ↓
Channel
 ┌────┼────┐
 ▼    ▼    ▼
Email In-App Push
```

---

# 48. Complete AI Data Pipeline

```text
                         ORIGINAL MP4
                              │
                              ▼
                       Video Validation
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
        Audio Extraction                 Frame Extraction
              │                               │
              ▼                         ┌─────┴─────┐
         STT Worker                    OCR        Formula
              │                         │            │
              ▼                         └─────┬──────┘
         Transcript                           │
              │                               │
              └───────────────┬───────────────┘
                              ▼
                       Topic Analysis
                              │
                              ▼
                       Quiz Generation
                              │
                              ▼
                         Translation
                              │
                              ▼
                      Lesson Generator
                              │
                              ▼
                       Lesson Manifest
                              │
                              ▼
                        Human Review
                              │
                              ▼
                           Publish
```

---

# 49. Data Ownership

| Data | Source of Truth |
|---|---|
| Users | PostgreSQL |
| Organizations | PostgreSQL |
| Courses | PostgreSQL |
| Lessons | PostgreSQL |
| Video metadata | PostgreSQL |
| Original video | Object Storage |
| Processed video | Object Storage |
| Transcript | PostgreSQL |
| OCR | PostgreSQL / Object Storage |
| Formula | PostgreSQL |
| Questions | PostgreSQL |
| Translation | PostgreSQL |
| Lesson manifest | Object Storage + DB reference |
| Progress | PostgreSQL |
| Events | PostgreSQL / Event Store |
| Cache | Redis |
| Jobs | PostgreSQL + Queue |

---

# 50. Data Retention

Retention policies should be defined for:

```text
Original videos
Temporary audio
Extracted frames
AI intermediate artifacts
Logs
Audit logs
Learning events
```

Temporary processing data should be deleted after successful processing where business requirements allow.

---

# 51. Data Consistency

Critical operations should use transactional database updates.

Examples:

```text
Publish Lesson
Create Subscription
Record Quiz Attempt
Update Progress
```

Long-running AI workflows should use eventual consistency.

---

# 52. Data Versioning

Versioned entities:

```text
Lesson
Transcript
Questions
Translations
AI Results
Lesson Manifest
```

Example:

```text
Lesson v1
Lesson v2
Lesson v3
```

Published versions should remain immutable.

---

# 53. Data Validation

Validation occurs at:

```text
Frontend
 ↓
API
 ↓
Application
 ↓
Database
 ↓
Worker
 ↓
AI Result Normalization
```

Never rely only on frontend validation.

---

# 54. Data Security

Sensitive data must be protected through:

```text
Encryption in transit
Encryption at rest
Access control
Tenant isolation
Signed URLs
Secret management
Audit logging
```

---

# 55. Data Flow Monitoring

Important stages must expose:

```text
Job ID
Stage
Status
Duration
Input reference
Output reference
Error
Retry count
```

Example:

```text
JOB-001
STT
COMPLETED
Duration: 42 sec
Retry: 0
```

---

# 56. Data Flow Definition of Done

The data-flow design is complete when:

```text
Upload Flow              ✓
Validation Flow          ✓
Video Processing         ✓
STT                      ✓
OCR                      ✓
Formula                  ✓
Topic Analysis           ✓
Quiz Generation          ✓
Translation              ✓
Lesson Generation        ✓
Teacher Review           ✓
Publishing               ✓
Student Playback         ✓
Progress                 ✓
Analytics                ✓
Subscription             ✓
Audit                    ✓
Failure / Retry          ✓
```

---

# 57. Next Document

```text
05_Authentication_Authorization_Design.md
```

This document will define:

```text
User Registration
        ↓
Login
        ↓
Session / Token
        ↓
Role-Based Access
        ↓
Organization / Tenant Access
        ↓
Resource Authorization
        ↓
Subscription Entitlement
        ↓
API Security
```

---

# 58. Revision History

| Version | Date | Description |
|---|---|---|
| 1.0.0 | 2026-08-11 | Initial end-to-end data flow design |
