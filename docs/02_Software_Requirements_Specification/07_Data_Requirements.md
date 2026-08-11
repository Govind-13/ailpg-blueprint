---
document_id: SRS-007
title: Data Requirements
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Data & Solution Architecture Team
parent_document: SRS-006
last_updated: 2026-08-11
---

# Data Requirements

## 1. Purpose

This document defines the data requirements for the AILPG platform.

The data architecture must support:

- Users
- Organizations
- Courses
- Lessons
- MP4 videos
- Video processing
- AI jobs
- Transcripts
- OCR
- Mathematical formulas
- Topics
- Questions
- Answers
- Translations
- Interactive checkpoints
- Student progress
- Analytics
- Subscriptions
- Entitlements
- Notifications
- Audit logs
- Content versions

---

# 2. Data Architecture

```text
                         AILPG
                           │
             ┌─────────────┴─────────────┐
             │                           │
          Identity                    Content
             │                           │
      ┌──────┼──────┐          ┌─────────┼─────────┐
      │      │      │          │         │         │
    Users  Roles   Orgs      Courses   Lessons   Videos
                                      │
                                      ▼
                                AI Processing
                                      │
          ┌──────────┬──────────┬─────┼──────┬──────────┐
          ▼          ▼          ▼     ▼      ▼          ▼
      Transcript    OCR      Formula  Quiz Translation Topics
                                      │
                                      ▼
                              Interactive Lesson
                                      │
                                      ▼
                                  Students
                                      │
                       ┌──────────────┼──────────────┐
                       ▼              ▼              ▼
                   Progress         Quiz          Analytics
```

---

# 3. Data Storage Strategy

The platform should use different storage systems for different data types.

## Primary Database

Recommended:

```text
PostgreSQL
```

Used for:

- Users
- Organizations
- Courses
- Lessons
- AI jobs
- Questions
- Progress
- Subscriptions
- Permissions
- Audit metadata

## Object Storage

Used for:

- Original MP4
- Processed video
- Audio
- Thumbnails
- Extracted frames
- Formula images
- Generated lesson assets

## Cache

Recommended:

```text
Redis
```

Used for:

- Sessions where applicable
- Frequently accessed data
- Job state
- Rate limiting
- Temporary processing state

## Search

Future option:

```text
OpenSearch / Elasticsearch
```

Used for:

- Transcript search
- Course search
- Lesson search
- Full-text search

---

# 4. Data Classification

| Classification | Examples |
|---|---|
| Public | Published course metadata |
| Internal | AI processing metadata |
| Private | Student progress |
| Sensitive | Authentication/security information |
| Restricted | Billing/subscription information |

Sensitive information must be protected using appropriate security controls.

---

# 5. User Data

## Entity

```text
users
```

## Required Fields

```text
id
organization_id
email
phone
display_name
avatar_url
status
created_at
updated_at
last_login_at
```

## Status

```text
ACTIVE
INACTIVE
SUSPENDED
DELETED
```

---

# 6. Role Data

## Entity

```text
roles
```

Example roles:

```text
STUDENT
TEACHER
REVIEWER
ORG_ADMIN
PLATFORM_ADMIN
SUPER_ADMIN
```

Users may have one or more roles depending on the authorization design.

---

# 7. Permission Data

## Entity

```text
permissions
```

Examples:

```text
course.create
course.edit
course.publish
video.upload
lesson.review
lesson.publish
user.manage
organization.manage
ai_job.retry
analytics.view
subscription.manage
```

---

# 8. Organization Data

## Entity

```text
organizations
```

Fields:

```text
id
name
slug
logo_url
status
default_language
created_at
updated_at
```

Every tenant-owned resource should be traceable to its organization.

---

# 9. Course Data

## Entity

```text
courses
```

Fields:

```text
id
organization_id
title
description
subject
grade
thumbnail_url
default_language
status
created_by
created_at
updated_at
published_at
```

---

# 10. Chapter Data

## Entity

```text
chapters
```

Fields:

```text
id
course_id
title
description
position
status
created_at
updated_at
```

A course may contain multiple chapters.

---

# 11. Lesson Data

## Entity

```text
lessons
```

Fields:

```text
id
course_id
chapter_id
title
description
position
status
current_version_id
created_by
created_at
updated_at
published_at
```

---

# 12. Video Data

## Entity

```text
videos
```

Fields:

```text
id
lesson_id
organization_id
original_filename
storage_key
mime_type
file_size
duration_seconds
width
height
codec
fps
source_language
status
checksum
uploaded_by
created_at
updated_at
```

---

# 13. Video Asset Data

Processed assets should be represented separately.

## Entity

```text
video_assets
```

Examples:

```text
ORIGINAL
AUDIO
THUMBNAIL
FRAME
HLS_MANIFEST
DASH_MANIFEST
LOW_QUALITY
MEDIUM_QUALITY
HIGH_QUALITY
```

Fields:

```text
id
video_id
asset_type
storage_key
quality
format
file_size
duration_seconds
created_at
```

---

# 14. AI Job Data

## Entity

```text
ai_jobs
```

Fields:

```text
id
video_id
lesson_id
organization_id
job_type
status
current_stage
progress_percent
retry_count
priority
started_at
completed_at
error_code
error_message
created_at
updated_at
```

## Job Types

```text
FULL_PIPELINE
TRANSCRIPTION
OCR
FORMULA
QUIZ
TRANSLATION
LESSON_GENERATION
```

---

# 15. AI Processing Stage Data

## Entity

```text
ai_job_stages
```

Fields:

```text
id
job_id
stage
status
progress_percent
started_at
completed_at
retry_count
error_message
metadata
```

## Stages

```text
VALIDATION
AUDIO_EXTRACTION
STT
OCR
FORMULA
SEGMENTATION
QUIZ
TRANSLATION
LESSON_GENERATION
```

---

# 16. Transcript Data

## Entity

```text
transcripts
```

Fields:

```text
id
video_id
language
version
status
created_by
created_at
updated_at
```

---

# 17. Transcript Segment Data

## Entity

```text
transcript_segments
```

Fields:

```text
id
transcript_id
start_time
end_time
text
confidence
speaker
position
```

Example:

```json
{
  "start_time": 120.5,
  "end_time": 127.8,
  "text": "Now we solve the equation.",
  "confidence": 0.96
}
```

---

# 18. OCR Data

## Entity

```text
ocr_results
```

Fields:

```text
id
video_id
timestamp
text
confidence
frame_storage_key
bounding_box
created_at
```

---

# 19. Formula Data

## Entity

```text
formulas
```

Fields:

```text
id
video_id
lesson_id
timestamp
plain_text
latex
mathml
confidence
image_storage_key
status
created_at
updated_at
```

---

# 20. Topic Data

## Entity

```text
topics
```

Fields:

```text
id
lesson_id
name
description
start_time
end_time
position
confidence
```

Example:

```text
Quadratic Equation
Factorization
Roots
Final Answer
```

---

# 21. Question Data

## Entity

```text
questions
```

Fields:

```text
id
lesson_id
checkpoint_time
question_type
question_text
explanation
difficulty
source
status
created_at
updated_at
```

## Question Source

```text
AI_GENERATED
TEACHER_CREATED
IMPORTED
```

---

# 22. Question Option Data

## Entity

```text
question_options
```

Fields:

```text
id
question_id
option_text
position
is_correct
```

The correct-answer information must never be exposed to unauthorized student clients before answer submission.

---

# 23. Quiz Attempt Data

## Entity

```text
quiz_attempts
```

Fields:

```text
id
question_id
student_id
lesson_id
selected_option_id
answer_value
is_correct
attempt_number
time_spent_seconds
created_at
```

---

# 24. Translation Data

## Entity

```text
translations
```

Fields:

```text
id
resource_type
resource_id
source_language
target_language
translated_text
version
confidence
status
created_at
updated_at
```

## Resource Types

```text
TRANSCRIPT
QUESTION
EXPLANATION
LESSON
UI
```

---

# 25. Interactive Checkpoint Data

## Entity

```text
interactive_checkpoints
```

Fields:

```text
id
lesson_id
timestamp
checkpoint_type
question_id
is_required
allow_skip
max_attempts
position
status
```

---

# 26. Lesson Version Data

## Entity

```text
lesson_versions
```

Fields:

```text
id
lesson_id
version_number
manifest_storage_key
status
created_by
created_at
published_at
```

## Version Status

```text
DRAFT
REVIEW
APPROVED
PUBLISHED
ARCHIVED
```

Published versions should be immutable.

---

# 27. Student Enrollment Data

## Entity

```text
enrollments
```

Fields:

```text
id
student_id
course_id
organization_id
status
enrolled_at
completed_at
```

---

# 28. Student Lesson Progress

## Entity

```text
lesson_progress
```

Fields:

```text
id
student_id
lesson_id
last_position_seconds
completion_percent
watch_time_seconds
last_accessed_at
completed_at
```

---

# 29. Student Course Progress

## Entity

```text
course_progress
```

Fields:

```text
id
student_id
course_id
completion_percent
completed_lessons
total_lessons
last_accessed_at
completed_at
```

---

# 30. Learning Session Data

## Entity

```text
learning_sessions
```

Fields:

```text
id
student_id
lesson_id
started_at
ended_at
duration_seconds
device_type
browser
ip_hash
```

Only information necessary for legitimate product, security, and analytics purposes should be collected.

---

# 31. Learning Event Data

## Entity

```text
learning_events
```

Examples:

```text
LESSON_STARTED
PLAY
PAUSE
SEEK
VIDEO_COMPLETED
QUESTION_SHOWN
QUESTION_ANSWERED
LANGUAGE_CHANGED
QUALITY_CHANGED
LESSON_COMPLETED
```

Fields:

```text
id
student_id
lesson_id
session_id
event_type
timestamp
position_seconds
metadata
created_at
```

---

# 32. Bookmark Data

## Entity

```text
bookmarks
```

Fields:

```text
id
student_id
lesson_id
timestamp
note
created_at
```

---

# 33. Student Notes

## Entity

```text
student_notes
```

Fields:

```text
id
student_id
lesson_id
timestamp
content
created_at
updated_at
```

---

# 34. Subscription Data

## Entity

```text
subscriptions
```

Fields:

```text
id
organization_id
user_id
plan_id
status
started_at
expires_at
cancelled_at
provider
provider_reference
```

---

# 35. Subscription Plan Data

## Entity

```text
subscription_plans
```

Fields:

```text
id
name
description
price
currency
billing_interval
status
created_at
updated_at
```

---

# 36. Entitlement Data

## Entity

```text
entitlements
```

Examples:

```text
VIDEO_1080P
VIDEO_720P
TRANSLATION
DOWNLOAD
OFFLINE
ADVANCED_ANALYTICS
AI_GENERATION
STORAGE_LIMIT
COURSE_ACCESS
```

---

# 37. Usage Data

## Entity

```text
usage_records
```

Track:

```text
video_storage
video_processing
AI_requests
translation_requests
watch_time
downloads
```

---

# 38. Notification Data

## Entity

```text
notifications
```

Fields:

```text
id
user_id
type
title
message
read_at
created_at
```

---

# 39. Audit Log Data

## Entity

```text
audit_logs
```

Fields:

```text
id
organization_id
user_id
action
resource_type
resource_id
metadata
created_at
```

Examples:

```text
COURSE_CREATED
VIDEO_UPLOADED
LESSON_EDITED
LESSON_PUBLISHED
USER_DISABLED
ROLE_CHANGED
AI_JOB_RETRIED
```

---

# 40. Data Relationships

```text
Organization
    │
    ├── Users
    │
    ├── Courses
    │      │
    │      ├── Chapters
    │      │      │
    │      │      └── Lessons
    │      │             │
    │      │             ├── Videos
    │      │             ├── Questions
    │      │             ├── Topics
    │      │             └── Versions
    │      │
    │      └── Enrollments
    │
    └── Subscriptions

Video
 │
 ├── Video Assets
 ├── AI Jobs
 ├── Transcripts
 │     └── Transcript Segments
 ├── OCR Results
 └── Formulas

Lesson
 │
 ├── Questions
 │     └── Options
 │
 ├── Checkpoints
 ├── Translations
 └── Versions

Student
 │
 ├── Enrollments
 ├── Lesson Progress
 ├── Course Progress
 ├── Quiz Attempts
 ├── Notes
 ├── Bookmarks
 └── Learning Sessions
```

---

# 41. Data Versioning

The following content should support versioning:

```text
Transcript
Translation
Question
Lesson
Interactive Checkpoint
Lesson Manifest
```

Example:

```text
Lesson v1
   ↓
Teacher edits
   ↓
Lesson v2
   ↓
Review
   ↓
Published
```

---

# 42. Data Integrity

The system must enforce:

- Foreign key relationships
- Required fields
- Unique constraints
- Valid status transitions
- Transactional updates where necessary
- Referential integrity

---

# 43. Soft Delete

Important business entities should preferably support soft deletion where historical records are required.

Example:

```text
deleted_at
deleted_by
```

Hard deletion should be restricted.

---

# 44. Data Retention

Retention policies should be configurable for:

- AI intermediate files
- Temporary frames
- Processing logs
- Analytics events
- Audit logs
- User-generated notes

Retention must follow applicable organizational and legal requirements.

---

# 45. Backup Requirements

Critical database data should support:

```text
Automated Backups
Point-in-Time Recovery
Backup Verification
Disaster Recovery
```

Object storage should have appropriate durability and backup/versioning strategy.

---

# 46. Data Security

The platform should implement:

- Encryption in transit
- Encryption at rest
- Access control
- Tenant isolation
- Secret management
- Audit logging
- Secure media URLs
- Restricted database access

---

# 47. Media Access Security

Video files should not be exposed through unrestricted permanent public URLs.

Recommended architecture:

```text
Student
   │
   ▼
Backend Authorization
   │
   ▼
Entitlement Check
   │
   ▼
Signed Media URL
   │
   ▼
CDN / Object Storage
```

This allows quality and subscription access to be enforced server-side.

---

# 48. Data Privacy

The system should minimize collection of personal data.

Student analytics should collect only what is necessary for:

- Learning progress
- Product functionality
- Security
- Reporting

Sensitive information should not be exposed through analytics dashboards unnecessarily.

---

# 49. Data Migration

Database schema changes must use controlled migrations.

Example:

```text
migration_001_initial_schema
migration_002_add_ai_jobs
migration_003_add_translations
migration_004_add_subscriptions
```

Migrations must be version-controlled.

---

# 50. Data Export

Authorized users may eventually export:

```text
Course Data
Lesson Data
Quiz Data
Student Progress
Analytics
```

Export permissions must be role-based.

---

# 51. Data Import

Future support may include:

```text
CSV
JSON
Question Banks
Course Metadata
```

Imported content must pass validation.

---

# 52. Data Performance Requirements

Large datasets must use:

- Pagination
- Indexing
- Query optimization
- Caching
- Batch processing
- Asynchronous analytics processing

Large learning-event tables should not be queried directly for every dashboard request.

---

# 53. Recommended Indexes

Important indexes include:

```text
users.email
users.organization_id
courses.organization_id
lessons.course_id
videos.lesson_id
ai_jobs.status
ai_jobs.video_id
transcript_segments.transcript_id
questions.lesson_id
quiz_attempts.student_id
lesson_progress.student_id
lesson_progress.lesson_id
learning_events.student_id
learning_events.lesson_id
audit_logs.organization_id
```

---

# 54. Data Lifecycle

```text
Upload
  ↓
Raw Data
  ↓
Processing
  ↓
AI Results
  ↓
Human Review
  ↓
Versioned Content
  ↓
Published Content
  ↓
Student Usage
  ↓
Analytics
  ↓
Archive / Retention
```

---

# 55. Source of Truth

| Data | Source of Truth |
|---|---|
| User | PostgreSQL |
| Course | PostgreSQL |
| Lesson | PostgreSQL |
| Video Metadata | PostgreSQL |
| Original Video | Object Storage |
| Processed Video | Object Storage |
| Transcript | PostgreSQL/Object Storage depending on scale |
| OCR | PostgreSQL/Object Storage |
| Formula | PostgreSQL |
| Questions | PostgreSQL |
| Translation | PostgreSQL/Object Storage |
| Progress | PostgreSQL |
| Events | Event Store / PostgreSQL initially |
| Analytics | Analytics Store |
| Cache | Redis |

---

# 56. MVP Data Scope

MVP must include:

```text
Users
Roles
Organizations
Courses
Chapters
Lessons
Videos
Video Assets
AI Jobs
AI Job Stages
Transcripts
Transcript Segments
OCR
Formulas
Topics
Questions
Question Options
Translations
Checkpoints
Lesson Versions
Enrollments
Lesson Progress
Quiz Attempts
Subscriptions
Entitlements
Notifications
Audit Logs
```

---

# 57. Future Data Scope

Future versions may add:

```text
Certificates
AI Tutor Conversations
Adaptive Learning Profiles
Recommendation Models
Offline Sync Data
Advanced Analytics Warehouse
Learning Competencies
Skill Graph
```

---

# 58. Data Definition of Done

Data requirements are complete when:

- Entities are defined.
- Relationships are defined.
- Required fields are defined.
- Security classification is defined.
- Retention requirements are defined.
- Versioning requirements are defined.
- Backup requirements are defined.
- Database design is mapped.
- API contracts reference the required data.

---

# 59. Related Documents

- SRS-005 — System Features
- SRS-006 — Interface Requirements
- Database Design
- API Design
- Technical Architecture
- AI Workflow
- Security Architecture
- Deployment Plan

---

# 60. Revision History

| Version | Date | Description |
|---|---|---|
| 1.0.0 | 2026-08-11 | Initial Data Requirements |
