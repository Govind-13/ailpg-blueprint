# AILPG — Analytics UI

**Document Path:** `docs/04_UI_UX_BLUEPRINT/12_Analytics_UI.md`
**Project:** MP4 → Interactive Learning Platform Generator (AILPG)
**Document Type:** UI/UX Blueprint — Analytics Interface
**Version:** 1.0
**Status:** Draft / Implementation Ready
**Parent Document:** `04_UI_UX_BLUEPRINT`
**Related Documents:** `07_Video_Player.md`, `08_Interactive_Question_UI.md`, `09_Course_Builder.md`, `10_Video_Upload_UI.md`, `11_AI_Review_UI.md`

---

# 1. Purpose

The **Analytics UI** provides administrators, instructors, content managers, and authorized users with a complete view of how AILPG content is being processed, delivered, watched, and learned from.

The analytics system should cover the complete platform lifecycle:

```text
Video Upload
     ↓
AI Processing
     ↓
AI Review
     ↓
Course Publication
     ↓
Video Playback
     ↓
Interactive Questions
     ↓
Student Progress
     ↓
Learning Outcomes
```

The Analytics UI should transform raw platform events into useful, understandable information.

---

# 2. Analytics Objectives

The interface should answer:

1. How many videos were uploaded?
2. How many videos successfully processed?
3. How long does AI processing take?
4. How much AI-generated content requires correction?
5. How many lessons are published?
6. How many students start lessons?
7. How much of each video is watched?
8. Where do students stop watching?
9. Which questions are difficult?
10. Which questions are frequently answered incorrectly?
11. Which concepts cause difficulty?
12. How many students complete lessons?
13. How does completion vary across courses?
14. Which languages are being used?
15. Which video quality levels are being delivered?
16. How does subscription/access status affect available quality?
17. What content needs improvement?

---

# 3. Analytics User Roles

| Role              | Analytics Access            |
| ----------------- | --------------------------- |
| Super Admin       | Platform-wide               |
| Admin             | Platform-wide / assigned    |
| Content Manager   | Content and AI analytics    |
| Instructor        | Own courses                 |
| Reviewer          | Review performance          |
| Student           | Personal learning analytics |
| Institution Admin | Institution-level           |
| Guest             | No analytics                |

Access must be enforced by the backend.

---

# 4. Analytics Dashboard Structure

The main dashboard should use:

```text
┌──────────────────────────────────────────────────────────────┐
│ AILPG Analytics                              Date: [30 Days] │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│ Students     Lessons      Watch Time      Completion         │
│ 12,480       842          9,820 hrs       68.4%              │
│                                                              │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│ Learning Activity                  Course Performance        │
│ ┌────────────────────────────┐     ┌───────────────────────┐ │
│ │                            │     │ Course A      82%     │ │
│ │       Activity Chart       │     │ Course B      74%     │ │
│ │                            │     │ Course C      69%     │ │
│ └────────────────────────────┘     └───────────────────────┘ │
│                                                              │
├──────────────────────────────────────────────────────────────┤
│ Question Performance              Video Engagement           │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

# 5. Global Analytics Header

The header should include:

* Page title
* Date range
* Compare period
* Course filter
* Module filter
* Lesson filter
* Language filter
* User type filter
* Device filter
* Export button

Example:

```text
Analytics

[Last 30 Days ▼]
[Compare: Previous Period ▼]
[All Courses ▼]
[All Languages ▼]

[Export CSV] [Export PDF]
```

---

# 6. Date Range

Supported options:

```text
Today
Yesterday
Last 7 Days
Last 30 Days
Last 90 Days
This Month
Previous Month
This Year
Custom Range
```

The selected period must always be clearly displayed.

---

# 7. KPI Cards

Recommended platform-level KPIs:

### Student Metrics

* Total learners
* Active learners
* New learners
* Returning learners

### Content Metrics

* Courses
* Modules
* Lessons
* Published videos

### Engagement Metrics

* Video starts
* Video completions
* Total watch time
* Average watch percentage

### Learning Metrics

* Questions answered
* Correct answer rate
* Lesson completion
* Course completion

---

# 8. KPI Card Design

```text
┌──────────────────────────┐
│ Active Learners          │
│                          │
│ 12,480                   │
│ ↑ 8.4%                   │
│ vs previous period       │
└──────────────────────────┘
```

The UI should show whether a comparison is:

* Absolute
* Percentage
* Previous period
* Previous year

The system must not imply that a change is inherently positive or negative unless a configured business rule explicitly defines the meaning.

---

# 9. Learning Activity Analytics

The platform should track learning activity over time.

Example:

```text
Daily Active Learners

Mon ███████████
Tue ███████████████
Wed █████████
Thu █████████████
Fri █████████████████
Sat ███████
Sun █████
```

Possible metrics:

* Daily active learners
* Weekly active learners
* Monthly active learners
* Lessons started
* Lessons completed
* Questions answered

---

# 10. Video Analytics

The Video Analytics section should provide:

* Video starts
* Unique viewers
* Total plays
* Average watch duration
* Average watch percentage
* Completion rate
* Rewatch rate
* Pause frequency
* Seek frequency
* Playback speed usage
* Quality selection
* Drop-off points

---

# 11. Video Engagement Timeline

Example:

```text
Viewer Retention

100% ┤████████████████████
 80% ┤██████████████████
 60% ┤██████████████
 40% ┤██████████
 20% ┤████
  0% ┼──────────────────────
      0    2    4    6    8   10 min
```

This identifies where viewers leave the lesson.

---

# 12. Drop-Off Analysis

The UI should identify:

```text
Highest Drop-Off Segments

04:32 — 18% drop
07:14 — 14% drop
09:51 — 11% drop
```

Selecting a segment should open the corresponding video timestamp.

---

# 13. Question Analytics

The question analytics system should track:

* Questions displayed
* Questions answered
* Correct answers
* Incorrect answers
* Skipped questions
* Hints used
* Retries
* Time to answer
* Completion after question
* Question abandonment

---

# 14. Question Performance Table

```text
Question             Attempts   Correct   Accuracy
---------------------------------------------------
Question 01            1,204      1,098     91.2%
Question 02            1,184        834     70.4%
Question 03            1,172        542     46.2%
Question 04            1,103        987     89.5%
```

Selecting a question opens detailed analytics.

---

# 15. Question Detail Analytics

```text
Question #03

Attempts:          1,172
Correct:             542
Incorrect:           630
Accuracy:           46.2%
Avg Answer Time:     18.4 sec
Hints Used:          31%
Retries:             14%
```

Additional analysis:

* Option distribution
* Answer time distribution
* Hint usage
* Retry behavior
* Drop-off after question

---

# 16. Answer Distribution

For multiple choice:

```text
A — 12%
B — 46%
C — 31%
D — 11%
```

The UI should distinguish:

* Correct option
* Incorrect options
* Unanswered
* Invalid answers

---

# 17. Difficulty Analytics

Question difficulty can be represented using observed learner performance.

Potential signals:

* Accuracy
* Average attempts
* Answer time
* Hint usage
* Retry rate

Example:

```text
Observed Performance

Easy       88%
Medium     67%
Difficult  43%
```

The interface should distinguish **AI-assigned difficulty** from **observed learner performance**.

---

# 18. Concept Analytics

AILPG should track performance by detected concept.

Example:

```text
Concept Performance

Linear Equations       82%
Fractions              71%
Factorization          63%
Quadratic Equations    54%
```

Selecting a concept should show:

* Related lessons
* Related questions
* Attempts
* Accuracy
* Watch time
* Drop-off
* Review corrections

---

# 19. Learning Progress

Student progress should be represented at multiple levels.

```text
Course
  ↓
Module
  ↓
Lesson
  ↓
Video
  ↓
Question
  ↓
Concept
```

Example:

```text
Course Progress
████████████████░░░░ 78%

Module 1
██████████████████ 90%

Module 2
██████████████░░░░ 72%

Module 3
██████████░░░░░░░░ 51%
```

---

# 20. Student Analytics

Authorized users should be able to view:

* Lessons started
* Lessons completed
* Watch time
* Question accuracy
* Attempts
* Hint usage
* Course progress
* Recent activity
* Learning streak where implemented
* Language
* Device category

Avoid exposing unnecessary personal information.

---

# 21. Student Personal Dashboard

Students should see only their own learning data.

```text
My Learning

Course Progress       72%

Watch Time            18h 42m

Lessons Completed     24 / 36

Questions Answered    418

Question Accuracy     81%

Recent Activity
✓ Algebra — Lesson 04
✓ Fractions — Lesson 07
○ Geometry — Lesson 03
```

---

# 22. Course Analytics

For each course:

```text
Course Analytics

Students              2,840
Lessons                42
Total Watch Time      1,420 hrs
Avg Completion          74%
Question Accuracy       79%
```

Additional sections:

* Enrollment
* Engagement
* Completion
* Question performance
* Concept performance
* Language usage
* Video performance

---

# 23. Lesson Analytics

Each lesson should provide:

* Views
* Unique learners
* Starts
* Completions
* Watch time
* Completion percentage
* Question attempts
* Accuracy
* Drop-off timestamps

---

# 24. AI Processing Analytics

The platform should measure AI pipeline performance.

Metrics:

* Videos submitted
* Processing jobs
* Successful jobs
* Failed jobs
* Average processing time
* Queue time
* AI processing cost
* Retry count
* Component generation time

---

# 25. AI Pipeline Dashboard

```text
AI Processing

Videos Processed        4,821
Successful              4,692
Failed                    129

Avg Processing Time     7m 42s

Transcript Success       98.7%
OCR Success              96.2%
Equation Detection       94.8%
Question Generation      97.1%
Translation Success      99.1%
```

---

# 26. AI Confidence Analytics

Track confidence by component:

```text
Component             Avg Confidence
-------------------------------------
Transcript                 94%
OCR                        82%
Equation                   88%
Concept                    91%
Question                   93%
Translation                89%
```

Low-confidence output should be connected to the AI Review queue.

---

# 27. AI Review Analytics

Metrics:

* Pending reviews
* Completed reviews
* Average review duration
* Corrections per lesson
* Regeneration rate
* Rejection rate
* Approval rate
* Low-confidence frequency

Example:

```text
Review Performance

Avg Review Time       14m 32s
AI Corrections        17.4%
Regeneration           8.2%
Changes Requested     12.1%
```

---

# 28. Correction Analytics

Corrections should be categorized.

```text
Reviewer Corrections

Transcript      24%
OCR             18%
Equation        11%
Question        21%
Explanation      9%
Translation     13%
Metadata          4%
```

This helps identify weaknesses in the AI pipeline.

---

# 29. Content Quality Dashboard

Possible metrics:

* AI confidence
* Human correction rate
* Review completion
* Question validation
* Equation validation
* Translation validation
* Publication failures

Example:

```text
Content Quality

High Confidence       74%
Needs Review           21%
Critical Review         5%
```

These are descriptive categories, not quality rankings of individual people.

---

# 30. Translation Analytics

Track:

* Languages generated
* Translation requests
* Translation completion
* Translation corrections
* Translation regeneration
* Language usage

Example:

```text
Language Usage

English       42%
Tamil         31%
Hindi         14%
Malayalam      7%
Other          6%
```

---

# 31. Language Analytics

Selecting a language should show:

```text
Tamil

Lessons Available       318
Learners                 4,820
Watch Time             1,842 hrs
Question Accuracy         79%
Completion                71%
```

---

# 32. Video Quality Analytics

Because AILPG supports subscription-aware quality delivery, analytics should track:

* Available qualities
* Requested quality
* Delivered quality
* Quality switching
* Buffering
* Playback failures

Example:

```text
Quality Selection

1080p      38%
720p       42%
480p       15%
360p        5%
```

Access entitlement must be determined by backend authorization.

---

# 33. Playback Performance

Track:

* Startup time
* Buffering events
* Rebuffer duration
* Playback errors
* Quality changes
* CDN errors
* Device category

Example:

```text
Playback Health

Startup Time       1.8 sec
Rebuffer Rate      2.1%
Playback Errors    0.4%
```

---

# 34. Device Analytics

Device categories:

```text
Desktop
Tablet
Mobile
Smart TV / Other
```

Additional dimensions:

* Operating system
* Browser family
* Screen category
* Network category

Avoid collecting unnecessary device-identifying information.

---

# 35. Engagement Funnel

The system should visualize:

```text
Video Uploaded
      ↓
Lesson Generated
      ↓
Published
      ↓
Lesson Started
      ↓
Question Answered
      ↓
Lesson Completed
      ↓
Course Completed
```

Example:

```text
10,000 Visitors
      ↓
 7,800 Started
      ↓
 6,200 Answered Questions
      ↓
 4,900 Completed Lesson
      ↓
 2,100 Completed Course
```

---

# 36. Course Completion Funnel

For a selected course:

```text
Enrolled
  100%
   ↓
Started
   86%
   ↓
Reached 50%
   71%
   ↓
Completed Lessons
   64%
   ↓
Completed Course
   48%
```

---

# 37. Cohort Analytics

Cohorts may be grouped by:

* Enrollment month
* Course
* Language
* Access type
* Institution
* Device category

Example:

```text
Cohort             Week 1   Week 2   Week 3   Week 4

September          82%      69%      58%      51%
October             85%      71%      61%      54%
```

The system should clearly identify cohort definitions and population sizes.

---

# 38. Retention Analytics

Track whether learners return after:

* 1 day
* 7 days
* 14 days
* 30 days

Example:

```text
Day 1       78%
Day 7       52%
Day 14      41%
Day 30      32%
```

---

# 39. Learning Session Analytics

A session may contain:

```text
Session Start
     ↓
Video Start
     ↓
Pause
     ↓
Question
     ↓
Answer
     ↓
Video Resume
     ↓
Lesson Completion
```

The system should record timestamps for supported events.

---

# 40. Event Tracking Model

Recommended event format:

```json
{
  "event": "QUESTION_ANSWERED",
  "userId": "user_123",
  "lessonId": "lesson_456",
  "questionId": "question_789",
  "timestamp": "2026-09-29T16:20:00Z",
  "metadata": {
    "correct": true,
    "attempt": 1,
    "answerTimeMs": 8200
  }
}
```

---

# 41. Core Analytics Events

### Video Events

```text
VIDEO_LOADED
VIDEO_STARTED
VIDEO_PAUSED
VIDEO_RESUMED
VIDEO_SEEKED
VIDEO_COMPLETED
VIDEO_ERROR
VIDEO_QUALITY_CHANGED
```

### Question Events

```text
QUESTION_SHOWN
QUESTION_STARTED
QUESTION_ANSWERED
QUESTION_CORRECT
QUESTION_INCORRECT
QUESTION_SKIPPED
QUESTION_HINT_USED
QUESTION_RETRY
```

### Lesson Events

```text
LESSON_STARTED
LESSON_PROGRESS
LESSON_COMPLETED
```

### Course Events

```text
COURSE_STARTED
COURSE_PROGRESS
COURSE_COMPLETED
```

---

# 42. Analytics Event Architecture

```text
Student / Admin Action
        ↓
Frontend Event
        ↓
Analytics SDK
        ↓
Event API
        ↓
Event Queue
        ↓
Analytics Processor
        ↓
Aggregations
        ↓
Analytics Database
        ↓
Analytics API
        ↓
Analytics UI
```

---

# 43. Analytics API

## Dashboard

```http
GET /api/analytics/dashboard
```

Parameters:

```text
from
to
courseId
language
device
```

---

## Video Analytics

```http
GET /api/analytics/videos/:videoId
```

---

## Lesson Analytics

```http
GET /api/analytics/lessons/:lessonId
```

---

## Course Analytics

```http
GET /api/analytics/courses/:courseId
```

---

## Question Analytics

```http
GET /api/analytics/questions/:questionId
```

---

## Student Analytics

```http
GET /api/analytics/students/:studentId
```

---

## AI Analytics

```http
GET /api/analytics/ai
```

---

## Review Analytics

```http
GET /api/analytics/reviews
```

---

## Export

```http
POST /api/analytics/export
```

Example:

```json
{
  "report": "course_performance",
  "format": "csv",
  "from": "2026-09-01",
  "to": "2026-09-29"
}
```

---

# 44. Analytics Database Model

Recommended event table:

```text
analytics_events
-----------------------------
id
event_type
user_id
course_id
module_id
lesson_id
video_id
question_id
session_id
timestamp
device_type
language
metadata
created_at
```

---

# 45. Aggregated Analytics Tables

For performance, raw events should not be queried for every dashboard request.

Possible aggregation tables:

```text
daily_platform_metrics
daily_course_metrics
daily_lesson_metrics
daily_video_metrics
daily_question_metrics
daily_learning_metrics
ai_processing_metrics
review_metrics
```

---

# 46. Analytics Data Flow

```text
Raw Events
    ↓
Validation
    ↓
Deduplication
    ↓
Normalization
    ↓
Aggregation
    ↓
Analytics Storage
    ↓
API
    ↓
Dashboard
```

---

# 47. Data Accuracy

Analytics must account for:

* Duplicate events
* Offline events
* Network retries
* Multiple browser tabs
* Video seeking
* Refreshes
* Abandoned sessions
* Delayed event delivery

Events should have unique IDs where appropriate.

---

# 48. Privacy

Analytics should follow data minimization principles.

Do not collect unnecessary:

* Passwords
* Authentication tokens
* Private message content
* Full uploaded personal documents
* Unnecessary location data

Student-level analytics should only be visible to authorized users.

---

# 49. Data Retention

Analytics retention should be configurable.

Example:

```text
Raw Events:
90 days

Aggregated Metrics:
24 months

Audit Logs:
Configured according to governance requirements
```

The actual retention policy should be defined by deployment and legal requirements.

---

# 50. Dashboard Filters

Global filters:

```text
Date
Course
Module
Lesson
Language
User Type
Access Type
Device
Video Quality
```

Filters should persist while navigating within the analytics section where practical.

---

# 51. Export System

Supported formats:

```text
CSV
XLSX
PDF
JSON
```

Reports:

* Platform analytics
* Course analytics
* Lesson analytics
* Question analytics
* AI processing
* Review analytics

Large exports should run asynchronously.

---

# 52. Export Status

```text
Preparing Report...

██████████████░░░░ 78%

[Cancel]
```

Completed:

```text
Report Ready

course_analytics_sep_2026.csv

[Download]
```

---

# 53. Scheduled Reports

Administrators may configure:

```text
Report:
Weekly Course Analytics

Frequency:
Every Monday

Recipients:
Authorized users

Format:
PDF + CSV
```

Access to reports must respect the recipient's authorization scope.

---

# 54. Alerts

Optional analytics alerts:

```text
Processing Failure Rate
Question Accuracy Drop
Playback Error Increase
Review Queue Increase
Storage Threshold
```

Example:

```text
AI Processing Alert

Processing failures exceeded configured threshold.

Current: 8.2%
Threshold: 5%

[View Processing Analytics]
```

---

# 55. Analytics Drill-Down

Users should be able to navigate:

```text
Platform
   ↓
Course
   ↓
Module
   ↓
Lesson
   ↓
Video
   ↓
Question
```

Example:

```text
Course Analytics
      ↓
Lesson Performance
      ↓
Question #12
      ↓
Answer Distribution
      ↓
Video Timestamp
```

---

# 56. Analytics Empty States

### No Data

```text
No analytics data available
for the selected period.
```

### New Course

```text
This course has not received
enough activity to display analytics.
```

### Restricted Data

```text
You do not have permission
to view this analytics data.
```

---

# 57. Analytics Loading States

Use skeleton loaders for:

* KPI cards
* Charts
* Tables
* Heatmaps
* Funnel diagrams

Avoid displaying misleading zeros while data is still loading.

---

# 58. Error States

```text
Unable to load analytics.

The analytics service may be temporarily unavailable.

[Retry]
```

The UI should distinguish:

* No data
* Loading
* Permission denied
* Server error
* Invalid filter

---

# 59. Accessibility

Charts must not rely solely on visual representation.

Provide:

* Text summaries
* Accessible chart labels
* Keyboard navigation
* Table alternatives
* Screen-reader descriptions
* Accessible filters
* Accessible date selectors

Example:

```text
Video completion rate:
68.4 percent.

The chart shows completion declining
from 91 percent at the beginning to
68.4 percent at the end.
```

---

# 60. Mobile Analytics

Mobile dashboard:

```text
Analytics

[Date Filter]

Active Learners
12,480

Completion
68.4%

Watch Time
9,820 hrs

[Engagement]
[Questions]
[Courses]
[AI]
```

Charts should support horizontal scrolling or simplified views where necessary.

---

# 61. Performance Requirements

Analytics pages should:

* Cache common queries
* Use server-side aggregation
* Paginate large tables
* Lazy-load secondary charts
* Avoid excessive raw-event queries
* Use indexed database fields
* Support asynchronous exports

Suggested target:

```text
Dashboard initial load:
< 3 seconds under normal load
```

Actual performance targets should be validated against production infrastructure.

---

# 62. Frontend Component Architecture

```text
AnalyticsPage
│
├── AnalyticsHeader
│   ├── DateRangePicker
│   ├── Filters
│   └── ExportButton
│
├── KPIGrid
│   ├── LearnerCard
│   ├── CourseCard
│   ├── WatchTimeCard
│   └── CompletionCard
│
├── EngagementSection
│   ├── ActivityChart
│   └── RetentionChart
│
├── LearningSection
│   ├── QuestionAnalytics
│   ├── ConceptAnalytics
│   └── CompletionFunnel
│
├── ContentSection
│   ├── CourseAnalytics
│   ├── LessonAnalytics
│   └── VideoAnalytics
│
├── AISection
│   ├── ProcessingAnalytics
│   ├── ConfidenceAnalytics
│   └── ReviewAnalytics
│
└── ExportCenter
```

---

# 63. Analytics State

Recommended frontend state:

```text
dateRange
comparisonPeriod
filters
kpis
activity
engagement
completion
questions
concepts
aiMetrics
reviewMetrics
loading
error
exportStatus
```

---

# 64. Authorization Model

Analytics API responses must be scoped according to the authenticated user.

Example:

```text
Super Admin
    → Platform

Admin
    → Platform / assigned organization

Instructor
    → Own courses

Reviewer
    → Review analytics

Student
    → Own learning data
```

The frontend must never be relied upon as the only authorization layer.

---

# 65. Security

Required:

* API authentication
* RBAC
* Object-level authorization
* Rate limiting
* Input validation
* Export authorization
* Audit logging
* Secure report URLs
* Expiring download links
* Protection against unauthorized student-data access

---

# 66. Analytics Audit Trail

Track:

```text
ANALYTICS_VIEWED
REPORT_CREATED
REPORT_EXPORTED
REPORT_DOWNLOADED
FILTER_APPLIED
STUDENT_ANALYTICS_VIEWED
```

Sensitive analytics access should be auditable.

---

# 67. Recommended Charts

AILPG should use charts according to the question being answered.

| Use Case              | Visualization    |
| --------------------- | ---------------- |
| Activity over time    | Line chart       |
| Category distribution | Bar chart        |
| Question accuracy     | Bar chart        |
| Video retention       | Line chart       |
| Course completion     | Funnel           |
| Concept performance   | Bar chart        |
| Quality selection     | Bar/donut        |
| AI confidence         | Distribution/bar |
| Review status         | Stacked bar      |
| Device usage          | Bar/donut        |
| Cohorts               | Heatmap          |

Avoid using complex charts when a simple table is clearer.

---

# 68. Analytics Naming Standards

Events should use consistent naming.

Recommended:

```text
VIDEO_STARTED
VIDEO_PAUSED
VIDEO_COMPLETED

QUESTION_SHOWN
QUESTION_ANSWERED
QUESTION_SKIPPED

LESSON_STARTED
LESSON_COMPLETED

COURSE_STARTED
COURSE_COMPLETED
```

Use uppercase `SNAKE_CASE` for event names.

---

# 69. Analytics Architecture

```text
                  ┌──────────────────┐
                  │ Student Platform │
                  └────────┬─────────┘
                           │
                           ▼
                    Analytics Events
                           │
                           ▼
                  ┌──────────────────┐
                  │ Event Collection │
                  └────────┬─────────┘
                           │
                           ▼
                    Message Queue
                           │
                           ▼
                  ┌──────────────────┐
                  │ Event Processor  │
                  └────────┬─────────┘
                           │
               ┌───────────┴───────────┐
               ▼                       ▼
        Raw Event Store         Aggregated Store
               │                       │
               └───────────┬───────────┘
                           ▼
                     Analytics API
                           │
                           ▼
                     Analytics UI
```

---

# 70. End-to-End Analytics Flow

```text
Student watches video
        ↓
Video event generated
        ↓
Event collected
        ↓
Validated
        ↓
Stored
        ↓
Aggregated
        ↓
Course metrics updated
        ↓
Analytics API
        ↓
Dashboard
```

---

# 71. Analytics Quality Controls

The platform should periodically verify:

* Event completeness
* Duplicate rate
* Missing timestamps
* Invalid IDs
* Aggregation accuracy
* Time-zone consistency
* Data freshness
* Export correctness

Example:

```text
Analytics Health

Event ingestion       ✓ Healthy
Aggregation            ✓ Healthy
Data freshness         ✓ 2 min
Duplicate rate         ✓ 0.4%
Failed events          ✓ 0.2%
```

---

# 72. Time Zone Handling

All backend event timestamps should be stored consistently, preferably in UTC.

The UI should display dates/times according to the user's configured timezone.

Analytics reports should state the timezone used for aggregation.

Example:

```text
Daily metrics
Timezone: Asia/Kolkata
```

---

# 73. Data Freshness

Dashboard should indicate whether data is:

```text
Live
Updated 2 minutes ago
Updated 1 hour ago
Historical
```

Real-time and batch analytics should not be presented as equivalent if their update frequencies differ.

---

# 74. Analytics Versioning

Analytics definitions can change over time.

For example:

```text
Completion Rate v1
Completion Rate v2
```

Metric definitions should be documented so historical reports remain interpretable.

---

# 75. Definition of Key Metrics

### Video Completion Rate

```text
Completed Video Plays
────────────────────── × 100
Started Video Plays
```

### Question Accuracy

```text
Correct Attempts
──────────────── × 100
Scored Attempts
```

### Lesson Completion

```text
Completed Lessons
───────────────── × 100
Started Lessons
```

Metric definitions should be finalized centrally and reused across API, UI, and reports.

---

# 76. Acceptance Criteria

The Analytics UI is complete when:

* [ ] Dashboard loads successfully.
* [ ] Date filtering works.
* [ ] Course filtering works.
* [ ] KPI cards display correct data.
* [ ] Video analytics work.
* [ ] Question analytics work.
* [ ] Course analytics work.
* [ ] Lesson analytics work.
* [ ] Student analytics respect authorization.
* [ ] AI processing analytics work.
* [ ] AI confidence analytics work.
* [ ] Review analytics work.
* [ ] Translation analytics work.
* [ ] Video quality analytics work.
* [ ] Playback performance metrics work.
* [ ] Drill-down navigation works.
* [ ] Export works.
* [ ] Permissions are enforced server-side.
* [ ] Audit logging works.
* [ ] Empty states work.
* [ ] Error states work.
* [ ] Mobile layout works.
* [ ] Accessibility requirements are satisfied.
* [ ] Analytics definitions are documented.

---

# 77. Definition of Done

The Analytics UI is production-ready when:

1. Platform activity can be measured.
2. Student engagement can be analyzed.
3. Video performance can be analyzed.
4. Interactive-question performance can be analyzed.
5. Course and lesson progress can be analyzed.
6. AI processing can be monitored.
7. AI review quality can be measured.
8. Translation usage can be measured.
9. Playback quality can be monitored.
10. Authorized users can export reports.
11. Student data is properly protected.
12. Analytics data can be traced back to source events.
13. Metric definitions are consistent throughout the platform.

---

# 78. Relationship With Complete AILPG UI/UX System

```text
01 Foundation
     │
     ▼
02 Authentication
     │
     ▼
03 Dashboard
     │
     ▼
04 UI/UX Blueprint
     │
     ├── 06 Admin Dashboard
     ├── 07 Video Player
     ├── 08 Interactive Question UI
     ├── 09 Course Builder
     ├── 10 Video Upload UI
     ├── 11 AI Review UI
     └── 12 Analytics UI
             │
             ▼
       Platform Intelligence
```

---

# 79. Final Analytics Architecture

```text
                        AILPG
                          │
          ┌───────────────┼────────────────┐
          │               │                │
          ▼               ▼                ▼
       Students         Admins         AI Pipeline
          │               │                │
          └───────┬───────┴────────┬───────┘
                  │                │
                  ▼                ▼
             Event System      AI Metrics
                  │                │
                  └───────┬────────┘
                          ▼
                    Analytics Engine
                          │
             ┌────────────┼────────────┐
             ▼            ▼            ▼
          Learning      Content        AI
          Analytics     Analytics    Analytics
             │            │            │
             └────────────┼────────────┘
                          ▼
                    Analytics API
                          │
                          ▼
                    Analytics UI
                          │
             ┌────────────┼────────────┐
             ▼            ▼            ▼
          Dashboard     Reports      Alerts
```

---

# 80. Final Product Principle

AILPG analytics should not merely report how many videos were watched.

It should connect the entire product lifecycle:

```text
CONTENT CREATION
       ↓
AI PROCESSING
       ↓
AI REVIEW
       ↓
PUBLICATION
       ↓
STUDENT ENGAGEMENT
       ↓
INTERACTIVE ANSWERS
       ↓
LEARNING PROGRESS
       ↓
CONTENT INSIGHTS
       ↓
AI / CONTENT IMPROVEMENT
```

This creates a continuous measurement loop for the platform:

```text
Generate
   ↓
Review
   ↓
Publish
   ↓
Measure
   ↓
Learn
   ↓
Improve
   ↓
Generate Better Content
```

The Analytics UI therefore becomes the measurement layer connecting **AILPG's AI pipeline, content management system, interactive video player, student learning experience, and administrative operations**.
