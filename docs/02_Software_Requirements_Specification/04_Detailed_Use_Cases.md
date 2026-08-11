---
document_id: SRS-004
title: Detailed Use Cases
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Product & Solution Architecture Team
parent_document: SRS-003
last_updated: 2026-08-11
---

# Detailed Use Cases

## 1. Purpose

This document defines detailed functional behavior for the major AILPG use cases.

Each use case includes:

- Actor
- Preconditions
- Inputs
- Main flow
- Alternative flow
- Error flow
- Outputs
- Permissions
- Acceptance criteria

---

# 2. Use Case Index

| ID | Use Case | Primary Actor |
|---|---|---|
| UC-TEA-001 | Create Course | Teacher |
| UC-TEA-002 | Upload MP4 | Teacher |
| UC-AI-001 | Process MP4 | AI Engine |
| UC-AI-002 | Generate Transcript | AI Engine |
| UC-AI-003 | Extract OCR | AI Engine |
| UC-AI-004 | Recognize Formula | AI Engine |
| UC-AI-005 | Generate Quiz | AI Engine |
| UC-AI-006 | Translate Lesson | AI Engine |
| UC-AI-007 | Generate Interactive Lesson | AI Engine |
| UC-REV-001 | Review AI Content | Teacher / Reviewer |
| UC-TEA-003 | Publish Lesson | Teacher / Reviewer |
| UC-STU-001 | Start Lesson | Student |
| UC-STU-002 | Answer Interactive Question | Student |
| UC-STU-003 | Resume Lesson | Student |
| UC-STU-004 | Change Video Quality | Student |
| UC-STU-005 | Change Language | Student |
| UC-STU-006 | Track Learning Progress | System |
| UC-ADM-001 | Monitor AI Jobs | Platform Admin |

---

# 3. UC-TEA-001 — Create Course

## Primary Actor

Teacher

## Goal

Create a container for lessons and educational videos.

## Preconditions

- Teacher is authenticated.
- Teacher has course creation permission.
- Organization is active.

## Inputs

- Course title
- Description
- Subject
- Grade
- Language
- Thumbnail

## Main Flow

1. Teacher opens Course Management.
2. Teacher selects Create Course.
3. System displays course form.
4. Teacher enters course information.
5. Teacher submits the form.
6. System validates input.
7. System creates course.
8. System assigns unique course ID.
9. Course appears in Teacher Dashboard.

## Alternative Flow

Teacher may save the course as a draft.

## Error Flow

If validation fails:

- Show field-level errors.
- Do not create invalid course.

## Output

```text
course_id
status = DRAFT
```

## Acceptance Criteria

- Course is created successfully with valid data.
- Invalid required fields are rejected.

---

# 4. UC-TEA-002 — Upload MP4

## Primary Actor

Teacher

## Goal

Upload a mathematical educational video for AI processing.

## Preconditions

- Teacher is authenticated.
- Course exists.
- Teacher has upload permission.
- Subscription permits the upload.

## Inputs

```text
MP4 File
Course ID
Lesson Title
Source Language
```

## Main Flow

1. Teacher selects lesson.
2. Teacher selects Upload Video.
3. System displays upload interface.
4. Teacher selects MP4.
5. Client validates basic file properties.
6. System requests secure upload URL.
7. File uploads to object storage.
8. Upload completion is reported to backend.
9. Backend validates uploaded file.
10. Video metadata is extracted.
11. System creates video record.
12. System creates AI job.
13. Processing status becomes QUEUED.

## Alternative Flow

Large files may use multipart/resumable upload.

## Error Flow

Possible errors:

- Unsupported format
- File too large
- Upload interrupted
- Storage failure
- Subscription quota exceeded

## Output

```text
video_id
upload_id
ai_job_id
status = QUEUED
```

## Acceptance Criteria

- Valid MP4 is accepted.
- Invalid files are rejected.
- Original video is preserved.
- AI job is created automatically.

---

# 5. UC-AI-001 — Process MP4

## Primary Actor

AI Processing Engine

## Goal

Convert MP4 into structured learning assets.

## Preconditions

- Valid video exists.
- AI job exists.
- Required processing services are available.

## Main Flow

```text
MP4
 ↓
Validation
 ↓
Audio Extraction
 ↓
Frame Extraction
 ↓
Speech-to-Text
 ↓
OCR
 ↓
Formula Recognition
 ↓
Topic Segmentation
 ↓
Quiz Generation
 ↓
Translation
 ↓
Interactive Lesson Generation
```

## Processing State

```text
QUEUED
PROCESSING
REVIEW_REQUIRED
COMPLETED
FAILED
```

## Error Flow

If one stage fails:

1. Record failure.
2. Store error details.
3. Retry according to retry policy.
4. Resume from the failed stage where possible.
5. Notify teacher if manual action is required.

---

# 6. UC-AI-002 — Generate Transcript

## Primary Actor

STT Service

## Goal

Convert spoken audio into timestamped text.

## Preconditions

- Audio extraction completed.

## Main Flow

1. Extract audio.
2. Send audio to STT engine.
3. Receive transcript segments.
4. Normalize text.
5. Store timestamps.
6. Store confidence values.
7. Save transcript version.

## Example

```json
{
  "start": 125.4,
  "end": 131.8,
  "text": "Now we move the 2 to the other side.",
  "confidence": 0.94
}
```

## Output

Timestamped transcript.

---

# 7. UC-AI-003 — Extract OCR

## Primary Actor

OCR Service

## Goal

Detect text visible inside video frames.

## Main Flow

1. Extract representative frames.
2. Detect text regions.
3. Run OCR.
4. Calculate confidence.
5. Associate result with timestamp.
6. Store OCR result.

## Output

```text
timestamp
text
bounding_box
confidence
```

## Error Flow

Low-confidence results are flagged for manual review.

---

# 8. UC-AI-004 — Recognize Formula

## Primary Actor

Formula Recognition Engine

## Goal

Detect mathematical expressions.

## Main Flow

1. Identify mathematical regions.
2. Extract formula image.
3. Process through formula recognition model.
4. Generate structured representation.
5. Generate LaTeX where supported.
6. Store confidence.
7. Link formula to timestamp.

## Example

Input:

```text
x² + 5x + 6 = 0
```

Output:

```latex
x^2 + 5x + 6 = 0
```

---

# 9. UC-AI-005 — Generate Quiz

## Primary Actor

LLM / Quiz Generation Engine

## Goal

Generate questions at meaningful learning checkpoints.

## Inputs

- Transcript
- Formula
- Topic
- Learning objective
- Difficulty
- Grade level

## Main Flow

1. Analyze lesson segment.
2. Identify learning concept.
3. Generate candidate questions.
4. Generate answers.
5. Generate distractors.
6. Assign difficulty.
7. Assign timestamp.
8. Validate question structure.
9. Store quiz draft.

## Example

```text
Video Timestamp: 03:42

Question:
What is the value of x?

Options:
A. 2
B. 3
C. 4
D. 5

Correct Answer:
B
```

## Important Rule

AI-generated questions must remain editable before publishing.

---

# 10. UC-AI-006 — Translate Lesson

## Primary Actor

Translation Service

## Goal

Generate multilingual learning content.

## Inputs

- Transcript
- Quiz
- UI labels
- Formula explanations where applicable
- Target language

## Main Flow

1. Identify source language.
2. Select target language.
3. Translate transcript.
4. Translate quiz content.
5. Preserve mathematical expressions.
6. Preserve timestamps.
7. Store translation version.
8. Mark for review when required.

## Supported Language Direction

The architecture should support configurable languages.

Initial deployment may prioritize Indian languages such as:

```text
English
Tamil
Hindi
Malayalam
Telugu
Kannada
Bengali
Marathi
```

The final language list shall be configurable.

---

# 11. UC-AI-007 — Generate Interactive Lesson

## Primary Actor

Lesson Generation Service

## Goal

Create the final interactive learning experience.

## Inputs

- Video metadata
- Transcript
- OCR
- Formulas
- Quiz checkpoints
- Translations
- Lesson configuration

## Main Flow

1. Load processed assets.
2. Create lesson manifest.
3. Create video timeline.
4. Insert quiz checkpoints.
5. Attach subtitles.
6. Attach translations.
7. Configure player controls.
8. Configure responsive layout.
9. Generate lesson package.
10. Create preview.
11. Save lesson version.

## Interactive Player Features

```text
Play
Pause
Seek
Volume
Fullscreen
Playback Speed
Quality
Language
Subtitles
Zoom
Quiz Checkpoints
Resume
```

---

# 12. UC-REV-001 — Review AI Content

## Primary Actor

Teacher / Reviewer

## Goal

Validate AI-generated content before publishing.

## Review Sections

```text
Video
Transcript
OCR
Formula
Translation
Quiz
Interactive Timeline
Preview
```

## Main Flow

1. Reviewer opens Review Workspace.
2. System displays synchronized video and AI results.
3. Reviewer selects content section.
4. Reviewer edits incorrect content.
5. Reviewer saves changes.
6. System creates new content version.
7. Reviewer approves content.

## Important Rule

AI-generated content must not automatically become published educational content without the configured approval workflow.

---

# 13. UC-TEA-003 — Publish Lesson

## Primary Actor

Teacher / Reviewer

## Preconditions

- Required content has been reviewed.
- Lesson passes validation.
- User has publishing permission.

## Main Flow

1. User selects Publish.
2. System runs publishing validation.
3. System checks required assets.
4. System creates immutable published version.
5. CDN/cache invalidation occurs where required.
6. Lesson status becomes PUBLISHED.
7. Student access becomes available.

## Output

```text
lesson_version
status = PUBLISHED
published_at
published_by
```

---

# 14. UC-STU-001 — Start Lesson

## Primary Actor

Student

## Preconditions

- Student is authenticated.
- Lesson is published.
- Student has access.

## Main Flow

1. Student opens lesson.
2. System validates access.
3. System loads lesson configuration.
4. Player loads appropriate video stream.
5. Student begins playback.
6. Learning session is created.
7. Progress events are tracked.

---

# 15. UC-STU-002 — Answer Interactive Question

## Primary Actor

Student

## Preconditions

- Student is watching an interactive lesson.
- A quiz checkpoint is reached.

## Main Flow

```text
Video Playing
     ↓
Checkpoint Reached
     ↓
Video Paused
     ↓
Question Displayed
     ↓
Student Answers
     ↓
Answer Evaluated
     ↓
Feedback Displayed
     ↓
Progress Saved
     ↓
Video Continues
```

## Possible Outcomes

### Correct

```text
Correct
↓
Positive Feedback
↓
Continue
```

### Incorrect

```text
Incorrect
↓
Feedback
↓
Optional Hint
↓
Retry / Continue
```

---

# 16. UC-STU-003 — Resume Lesson

## Primary Actor

Student

## Main Flow

1. Student opens lesson.
2. System retrieves latest progress.
3. System retrieves playback position.
4. Player offers Resume.
5. Student selects Resume.
6. Playback starts from saved position.

## Stored Information

```text
lesson_id
student_id
position_seconds
completion_percentage
last_accessed_at
```

---

# 17. UC-STU-004 — Change Video Quality

## Primary Actor

Student

## Goal

Change video playback quality according to available entitlement and network conditions.

## Main Flow

1. Student opens quality selector.
2. System determines available qualities.
3. Student selects quality.
4. Backend validates entitlement where required.
5. Player switches representation.

## Example

```text
Auto
1080p
720p
480p
360p
```

## Subscription Rule

The frontend may hide unavailable options, but backend authorization and signed media access must enforce subscription restrictions.

---

# 18. UC-STU-005 — Change Language

## Primary Actor

Student

## Main Flow

1. Student opens language selector.
2. System displays available languages.
3. Student selects language.
4. System loads corresponding transcript/subtitle/content version.
5. Player continues without losing learning progress.

---

# 19. UC-STU-006 — Track Learning Progress

## Primary Actor

System

## Events

```text
lesson_started
video_played
video_paused
video_resumed
video_seeked
video_completed
quiz_displayed
quiz_answered
quiz_completed
language_changed
quality_changed
lesson_completed
```

## Output

Analytics and learning-progress records.

---

# 20. UC-ADM-001 — Monitor AI Jobs

## Primary Actor

Platform Administrator

## Dashboard

Display:

- Total jobs
- Queued jobs
- Running jobs
- Completed jobs
- Failed jobs
- Retry count
- Processing time
- Worker utilization

## Actions

Admin may:

- View job
- Inspect logs
- Retry job
- Cancel job
- Reprocess failed stage

---

# 21. Cross-Cutting Requirements

All use cases shall respect:

## Authentication

Only authenticated users may access protected functions.

## Authorization

Access shall be controlled using RBAC and organization-level permissions.

## Audit Logging

Security-sensitive actions should be recorded.

## Versioning

Educational content changes should be versioned.

## Observability

Important system operations should expose logs, metrics, and traces.

## Data Isolation

Organization data must remain isolated according to tenant rules.

---

# 22. End-to-End Business Flow

```text
Teacher
   │
   ▼
Create Course
   │
   ▼
Upload MP4
   │
   ▼
AI Processing
   │
   ├── STT
   ├── OCR
   ├── Formula
   ├── Segmentation
   ├── Quiz
   └── Translation
   │
   ▼
Interactive Lesson
   │
   ▼
Teacher Review
   │
   ▼
Publish
   │
   ▼
Student
   │
   ▼
Interactive Learning
   │
   ▼
Analytics
```

---

# 23. Use Case Completion Definition

A use case is considered implemented when:

- Main flow works.
- Required alternative flows work.
- Required error handling exists.
- Authorization is implemented.
- Data is persisted correctly.
- Relevant analytics events are recorded.
- Acceptance criteria pass.
- QA test cases pass.

---

# 24. Related Documents

- SRS-003 — Use Case Diagrams
- SRS-005 — System Features
- SRS-006 — Interface Requirements
- SRS-007 — Data Requirements
- BRD-009 — Functional Requirements
- BRD-014 — Acceptance Criteria
- AI Workflow
- API Design
- Database Design

---

# 25. Revision History

| Version | Date | Description |
|---|---|---|
| 1.0.0 | 2026-08-11 | Initial Detailed Use Cases |
