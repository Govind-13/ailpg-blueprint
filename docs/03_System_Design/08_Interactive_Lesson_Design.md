---
document_id: SD-008
title: Interactive Lesson Design
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Product Architecture Team
parent_document: SD-007
last_updated: 2026-08-11
---

# Interactive Lesson Design

## 1. Purpose

This document defines the architecture and behavior of the AILPG interactive student lesson.

The lesson converts a normal educational video into an interactive learning experience.

Core experience:

```text
Introduction
    ↓
Learning Objectives
    ↓
Video
    ↓
Transcript / Language
    ↓
Checkpoint
    ↓
Question
    ↓
Student Answer
    ↓
Feedback
    ↓
Explanation
    ↓
Resume Video
    ↓
Progress
    ↓
Completion
```

---

# 2. Design Goals

The student experience must support:

```text
Video playback
Interactive checkpoints
Questions
Instant feedback
Explanations
Transcript
Search
Language switching
Subtitles
Zoom
Playback speed
Quality selection
Progress tracking
Resume playback
Lesson completion
Accessibility
Responsive design
```

---

# 3. Student Lesson Architecture

```text
                    LESSON
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
    INTRODUCTION     VIDEO         CONTENT
                       │
            ┌──────────┼───────────┐
            ▼          ▼           ▼
         Player    Transcript   Controls
            │
            ▼
       Timeline Engine
            │
            ▼
      Checkpoint Engine
            │
            ▼
       Question Engine
            │
            ▼
      Answer Validation
            │
            ▼
         Feedback
            │
            ▼
       Progress Engine
```

---

# 4. Lesson Page Structure

Recommended page:

```text
┌──────────────────────────────────────────────┐
│ Header                                       │
├──────────────────────────────────────────────┤
│ Course / Lesson Breadcrumb                   │
├──────────────────────────────────────────────┤
│                                              │
│              VIDEO PLAYER                    │
│                                              │
├──────────────────────────────────────────────┤
│ Video Controls                               │
├──────────────────────────────────────────────┤
│ Interactive Timeline                         │
├──────────────────────────────────────────────┤
│ Transcript / Notes / Questions               │
├──────────────────────────────────────────────┤
│ Lesson Information                           │
└──────────────────────────────────────────────┘
```

---

# 5. Lesson Introduction

Before video playback:

```text
Lesson Title
Short Description
Teacher
Duration
Difficulty
Learning Objectives
```

Example:

```text
Quadratic Equations — Factorisation

Duration: 12 minutes
Difficulty: Beginner

You will learn:
• What a quadratic equation is
• How to factorise it
• How to find its roots
```

---

# 6. Start Lesson

Primary action:

```text
▶ Start Lesson
```

Optional:

```text
Resume from 04:32
```

If the student previously watched the lesson:

```text
Continue Learning
```

should be displayed.

---

# 7. Video Player

Player must support:

```text
Play
Pause
Seek
Volume
Mute
Fullscreen
Playback speed
Quality
Subtitles
Language
Picture-in-picture where supported
```

---

# 8. Educational Player Controls

Additional controls:

```text
Transcript
Notes
Questions
Zoom
Previous checkpoint
Next checkpoint
```

---

# 9. Playback Speed

Supported speeds may include:

```text
0.5x
0.75x
1x
1.25x
1.5x
1.75x
2x
```

The exact options should be configurable.

---

# 10. Video Quality

Example:

```text
Auto
1080p
720p
480p
360p
```

Available options depend on:

```text
Source video
Subscription entitlement
Network
Device
```

---

# 11. Zoom

Educational videos may require visual zoom.

Example:

```text
100%
125%
150%
200%
```

Zoom must be implemented as player presentation behavior rather than modifying the original media.

---

# 12. Zoom Interaction

```text
Normal
  ↓
Zoom
  ↓
Pan
  ↓
Reset
```

For board/screen-recorded mathematical content, zoom should keep important formula regions visible.

---

# 13. Transcript Panel

Transcript should synchronize with playback.

```text
Video Time
    ↓
Transcript Segment
    ↓
Highlight Current Sentence
```

Example:

```text
00:35
"First, we need to factor the expression."

00:42
"x² + 5x + 6"
```

---

# 14. Transcript Actions

Student can:

```text
Click transcript segment
        ↓
Jump video to timestamp
```

This enables fast navigation.

---

# 15. Transcript Search

Example:

```text
Search: factorisation
```

Results:

```text
02:15 Factorisation
05:42 Factorisation method
08:20 Factorisation example
```

Clicking a result seeks to the relevant timestamp.

---

# 16. Language Switcher

Example:

```text
Language

English
தமிழ்
हिन्दी
తెలుగు
മലയാളം
ಕನ್ನಡ
```

Language availability depends on generated translations.

---

# 17. Subtitle Architecture

```text
Video
 ↓
Subtitle Track
 ↓
Language Selection
 ↓
Player
```

Subtitles should remain synchronized with video timestamps.

---

# 18. Interactive Timeline

Timeline:

```text
0:00──────────────────────────────12:00
       ●              ●        ●
       │              │        │
     Intro        Question   Summary
```

Markers represent interactive events.

---

# 19. Timeline Marker Types

```text
INTRO
CONCEPT
FORMULA
QUESTION
SUMMARY
IMPORTANT
```

Visual styling should distinguish marker types without relying only on color.

---

# 20. Checkpoint Engine

The checkpoint engine monitors:

```text
currentTime
lesson events
checkpoint timestamps
checkpoint state
```

Conceptual logic:

```text
currentTime >= checkpoint.timestamp
        ↓
Checkpoint not completed?
        ↓
Pause video
        ↓
Open interaction
```

---

# 21. Checkpoint Behavior

Default:

```text
Video Playing
      ↓
Checkpoint Reached
      ↓
Pause
      ↓
Question Appears
```

Student cannot accidentally miss a required checkpoint.

---

# 22. Optional Checkpoint

Some checkpoints may be optional.

Example:

```text
┌─────────────────────────────┐
│ Quick Check                 │
│                             │
│ Try this question           │
│                             │
│ [Answer]                    │
│                             │
│ Skip for now                │
└─────────────────────────────┘
```

---

# 23. Required Checkpoint

Required checkpoints:

```text
Video
 ↓
Question
 ↓
Answer required
 ↓
Feedback
 ↓
Continue
```

The lesson designer controls whether a checkpoint is required.

---

# 24. Question UI

Example:

```text
┌──────────────────────────────────────┐
│ Quick Check                          │
│                                      │
│ What are the roots of:               │
│                                      │
│ x² + 5x + 6 = 0                      │
│                                      │
│ ○ -2 and -3                          │
│ ○  2 and 3                           │
│ ○ -1 and -6                          │
│ ○  1 and 6                            │
│                                      │
│              [Submit Answer]         │
└──────────────────────────────────────┘
```

---

# 25. Answer Submission

Flow:

```text
Select Answer
      ↓
Submit
      ↓
Validate
      ↓
Correct / Incorrect
```

---

# 26. Correct Feedback

Example:

```text
✓ Correct!

The equation can be factored as:

(x + 2)(x + 3) = 0

Therefore:

x = -2 or x = -3
```

Actions:

```text
Continue Video
Review Explanation
```

---

# 27. Incorrect Feedback

Example:

```text
Not quite.

Try factoring:

x² + 5x + 6

Look for two numbers whose:

Product = 6
Sum = 5
```

The product behavior should be configurable:

```text
Immediate explanation
Retry
Show hint
Continue
```

---

# 28. Retry Configuration

Question can allow:

```text
1 attempt
2 attempts
3 attempts
Unlimited
```

or:

```text
Single submission
```

---

# 29. Hint System

Optional:

```text
Need a hint?
```

Then:

```text
Hint 1
 ↓
Hint 2
 ↓
Explanation
```

Hints may be teacher-created or AI-generated and teacher-approved.

---

# 30. Question Completion

After successful interaction:

```text
Question Completed
       ↓
Record Result
       ↓
Update Progress
       ↓
Resume Video
```

---

# 31. Resume Behavior

After checkpoint completion:

```text
Resume from checkpoint timestamp
```

Optionally seek a small configurable offset forward to prevent immediate retriggering.

---

# 32. Preventing Duplicate Checkpoints

Checkpoint state must be tracked.

Example:

```text
checkpoint_id
status
attempts
completed_at
```

If already completed:

```text
Do not show again
```

unless replay mode is enabled.

---

# 33. Replay Mode

Students may revisit previous checkpoints.

```text
Review Checkpoint
 ↓
Question
 ↓
Feedback
```

Replay should not create duplicate completion records.

---

# 34. Student Progress

Progress includes:

```text
Video watched
Checkpoint completed
Questions answered
Correct answers
Incorrect answers
Attempts
Time spent
Lesson completion
```

---

# 35. Video Watch Progress

Example:

```json
{
  "lesson_id": "lesson_001",
  "position": 272.4,
  "duration": 720,
  "percentage": 37.8
}
```

---

# 36. Resume Learning

When student returns:

```text
Lesson
 ↓
Previous progress found
 ↓
Resume prompt
```

Example:

```text
Continue from 04:32?

[Continue] [Start from Beginning]
```

---

# 37. Progress Save Strategy

Progress should be saved:

```text
On playback interval
On pause
On checkpoint
On answer
On page exit
On lesson completion
```

Do not send a server request for every video frame/time update.

---

# 38. Progress Synchronization

```text
Player
 ↓
Local State
 ↓
Periodic Sync
 ↓
Progress API
 ↓
Database
```

Local state can provide resilience during temporary network issues.

---

# 39. Offline / Network Recovery

If network temporarily fails:

```text
Player continues
       ↓
Local progress stored
       ↓
Network restored
       ↓
Sync progress
```

Exact offline-video availability is a separate product/security feature and is not assumed by default.

---

# 40. Lesson Completion

A lesson may be considered completed when configured completion rules are satisfied.

Possible rules:

```text
Watch percentage
+
Required checkpoints
+
Required questions
```

Example:

```text
Watch ≥ 80%
AND
Required checkpoints completed
```

---

# 41. Completion Event

When completion occurs:

```text
LESSON_COMPLETED
```

Record:

```text
student_id
lesson_id
course_id
completed_at
score
watch_percentage
```

---

# 42. Score Calculation

Example:

```text
Correct Answers / Attempted Questions × 100
```

For more complex lessons:

```text
Question Weight
Checkpoint Weight
Difficulty Weight
```

should be configurable.

---

# 43. Course Progress

```text
Course
│
├── Lesson 1 ✓
├── Lesson 2 ✓
├── Lesson 3 65%
└── Lesson 4 ○
```

Course progress can be calculated from lesson completion states.

---

# 44. Student Dashboard

Student dashboard should show:

```text
Continue Learning
Recent Lessons
Course Progress
Completed Lessons
Recommended Lessons
Performance
```

---

# 45. Lesson State Machine

```text
NOT_STARTED
    ↓
STARTED
    ↓
WATCHING
    ↓
CHECKPOINT
    ↓
ANSWERING
    ↓
FEEDBACK
    ↓
WATCHING
    ↓
COMPLETED
```

Possible alternate states:

```text
PAUSED
ABANDONED
REVIEWING
```

---

# 46. Player State

Player maintains:

```text
playing
paused
currentTime
duration
volume
muted
speed
quality
fullscreen
zoom
language
subtitle
```

---

# 47. Lesson State

Lesson maintains:

```text
lessonId
videoId
started
completed
progress
checkpointState
questionState
score
```

Player state and lesson state must remain separate.

---

# 48. Interactive Event Engine

The player should consume an event manifest.

Example:

```json
{
  "events": [
    {
      "id": "event_001",
      "type": "QUESTION",
      "timestamp": 165.5,
      "question_id": "q_001"
    }
  ]
}
```

---

# 49. Lesson Manifest

The lesson manifest is the primary frontend configuration.

Example:

```json
{
  "lesson": {
    "id": "lesson_001",
    "title": "Quadratic Equations"
  },
  "video": {
    "hls_url": "...",
    "duration": 720
  },
  "events": [],
  "transcript": [],
  "languages": [],
  "questions": []
}
```

---

# 50. Manifest Responsibilities

Manifest provides:

```text
Video reference
Duration
Timeline events
Questions
Checkpoints
Transcript
Translations
Subtitles
Lesson metadata
```

It should not contain secrets.

---

# 51. Manifest Versioning

Example:

```text
manifest_v1
manifest_v2
manifest_v3
```

When teacher publishes changes:

```text
New Manifest Version
```

Students should receive a consistent published version.

---

# 52. Draft vs Published Lesson

```text
DRAFT
 ↓
AI GENERATED
 ↓
TEACHER REVIEW
 ↓
APPROVED
 ↓
PUBLISHED
```

Students only receive:

```text
PUBLISHED
```

content.

---

# 53. Teacher Preview

Teacher can preview:

```text
Video
Checkpoints
Questions
Transcript
Translations
```

Preview should behave as closely as possible to the student player.

---

# 54. Teacher Editing

Teacher can edit:

```text
Lesson title
Description
Transcript
Question
Answer
Explanation
Checkpoint timestamp
Language
Learning objectives
Thumbnail
```

---

# 55. Checkpoint Editing

Teacher can:

```text
Move timestamp
Change question
Change required status
Delete checkpoint
Add checkpoint
Change question type
```

---

# 56. AI Regeneration

Teacher can request:

```text
Regenerate Question
Regenerate Explanation
Regenerate Translation
Regenerate Summary
```

Only the selected content should be regenerated where possible.

---

# 57. Accessibility

The player should support:

```text
Keyboard navigation
Captions
Transcript
Readable typography
Screen-reader labels
Focus indicators
Reduced-motion behavior
Accessible question controls
```

Do not rely on color alone to communicate correctness.

---

# 58. Responsive Design

Supported layouts:

```text
Desktop
Tablet
Mobile
```

Desktop:

```text
Video + Side Panel
```

Mobile:

```text
Video
 ↓
Controls
 ↓
Interactive Content
```

---

# 59. Mobile Interaction

On mobile:

```text
Tap → Play/Pause
Double tap → Seek
Pinch → Zoom
Swipe → Timeline navigation
```

Exact gesture behavior should remain consistent with platform conventions.

---

# 60. Error States

The player must handle:

```text
Video unavailable
Network failure
Manifest failure
Playback error
Question loading failure
Progress sync failure
Translation unavailable
```

Example:

```text
We couldn't load this lesson.

[Retry]
```

---

# 61. Graceful Degradation

If optional AI content fails:

```text
Video
✓

Transcript
✓

Question
FAILED
```

The student should still be able to continue if the checkpoint is not required.

Required checkpoint failures should be surfaced clearly and handled according to lesson policy.

---

# 62. Security

Student player must never trust client-side completion alone.

Server validates:

```text
Student entitlement
Lesson access
Playback session
Completion events
Question submissions
```

---

# 63. Anti-Cheating Considerations

For basic lessons:

```text
Client submission + server validation
```

For higher-stakes assessments, consider:

```text
Server-side answer validation
Attempt limits
Timing rules
Question randomization
Question pools
```

No anti-cheating mechanism should compromise accessibility unnecessarily.

---

# 64. Event Tracking

Student interaction events:

```text
LESSON_STARTED
VIDEO_PLAYED
VIDEO_PAUSED
VIDEO_SEEKED
CHECKPOINT_REACHED
QUESTION_OPENED
QUESTION_SUBMITTED
QUESTION_CORRECT
QUESTION_INCORRECT
HINT_OPENED
LESSON_COMPLETED
```

---

# 65. Analytics Separation

Player events should be sent to an analytics pipeline rather than making the lesson database the primary analytics store.

```text
Player
 ↓
Event Collector
 ↓
Analytics Queue
 ↓
Analytics Storage
```

---

# 66. Performance Requirements

Target:

```text
Fast lesson loading
Fast video startup
Minimal UI blocking
Smooth seeking
Low checkpoint latency
```

Heavy AI processing must never happen directly inside the student player.

---

# 67. Frontend Architecture

Suggested modules:

```text
lesson/
├── LessonPage
├── VideoPlayer
├── PlayerControls
├── Timeline
├── TranscriptPanel
├── CheckpointEngine
├── QuestionPanel
├── FeedbackPanel
├── ProgressTracker
├── LanguageSelector
└── Accessibility
```

---

# 68. State Management

Frontend state should separate:

```text
Player State
Lesson State
Question State
Progress State
UI State
```

Example:

```text
PlayerState
LessonState
CheckpointState
QuestionState
ProgressState
UIState
```

---

# 69. API Communication

Student player APIs:

```text
GET  /api/v1/lessons/{lesson_id}
GET  /api/v1/lessons/{lesson_id}/manifest
POST /api/v1/lessons/{lesson_id}/start
POST /api/v1/lessons/{lesson_id}/progress
POST /api/v1/questions/{question_id}/submit
POST /api/v1/checkpoints/{checkpoint_id}/complete
POST /api/v1/lessons/{lesson_id}/complete
```

Exact contracts belong to API Design.

---

# 70. Caching Strategy

Cache:

```text
Lesson metadata
Published manifest
Transcript
Static thumbnails
```

Do not cache sensitive student-specific data as public content.

---

# 71. Lesson Loading Strategy

Recommended:

```text
Load lesson metadata
       ↓
Load manifest
       ↓
Render player
       ↓
Load video
       ↓
Load optional panels
```

The video should not wait for every optional AI asset before rendering the core player.

---

# 72. Player Error Recovery

Example:

```text
Playback Error
 ↓
Retry
 ↓
Refresh Manifest
 ↓
Retry Playback
```

If the problem persists:

```text
Display Error
+
Support reference ID
```

---

# 73. Lesson Data Model

Conceptual entities:

```text
lessons
lesson_versions
lesson_manifests
lesson_events
checkpoints
questions
question_attempts
student_progress
lesson_completions
```

Detailed schema belongs to:

```text
Layer 6 — Database Design
```

---

# 74. Interactive Lesson API Model

The frontend should consume a stable contract:

```text
Lesson
 ├── Metadata
 ├── Video
 ├── Transcript
 ├── Languages
 ├── Events
 ├── Questions
 └── Completion Rules
```

---

# 75. Example Complete Lesson Manifest

```json
{
  "version": "1.0",
  "lesson": {
    "id": "lesson_001",
    "title": "Quadratic Equations",
    "duration": 720
  },
  "video": {
    "type": "hls",
    "playback_url": "SIGNED_URL"
  },
  "transcript": {
    "language": "en-IN",
    "segments": []
  },
  "events": [
    {
      "id": "checkpoint_001",
      "type": "QUESTION",
      "timestamp": 165.5,
      "question_id": "question_001",
      "required": true
    }
  ],
  "completion": {
    "minimum_watch_percentage": 80,
    "required_checkpoints": true
  }
}
```

---

# 76. End-to-End Student Flow

```text
Student Login
      ↓
Open Course
      ↓
Open Lesson
      ↓
Load Lesson Manifest
      ↓
Check Entitlement
      ↓
Start / Resume Video
      ↓
Watch
      ↓
Checkpoint
      ↓
Question
      ↓
Submit Answer
      ↓
Server Validation
      ↓
Feedback
      ↓
Resume Video
      ↓
More Checkpoints
      ↓
Complete Lesson
      ↓
Update Course Progress
```

---

# 77. Lesson Completion Example

```text
Lesson Duration: 12:00

Student watched: 10:25
Watch percentage: 87%

Required checkpoints:
CP1 ✓
CP2 ✓
CP3 ✓

Result:

LESSON_COMPLETED
```

---

# 78. Definition of Done

```text
Lesson Introduction             ✓
Video Player                    ✓
HLS Playback                    ✓
Quality Selection               ✓
Playback Speed                  ✓
Zoom                            ✓
Transcript                      ✓
Transcript Search               ✓
Language Switch                 ✓
Interactive Timeline             ✓
Checkpoint Engine               ✓
Question Engine                  ✓
Answer Validation               ✓
Feedback                         ✓
Hints                            ✓
Progress Tracking                ✓
Resume Playback                  ✓
Lesson Completion                ✓
Accessibility                    ✓
Responsive UI                    ✓
Error Recovery                   ✓
Teacher Preview                  ✓
Lesson Versioning                ✓
```

---

# 79. Next Document

```text
09_Analytics_Design.md
```

This document will define the complete analytics system:

```text
Student Events
      ↓
Event Collector
      ↓
Message Queue
      ↓
Analytics Processor
      ↓
Data Warehouse
      ↓
Teacher Dashboard
      ↓
Admin Dashboard
```

It will cover:

```text
Watch time
Completion rate
Drop-off points
Question accuracy
Student performance
Concept mastery
Video engagement
Checkpoint analytics
Course analytics
Teacher analytics
AI analytics
```

---

# 80. Revision History

| Version | Date | Description |
|---|---|---|
| 1.0.0 | 2026-08-11 | Initial interactive lesson design |
