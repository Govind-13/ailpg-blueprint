---
document_id: SD-009
title: Analytics Design
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Data Architecture Team
parent_document: SD-008
last_updated: 2026-08-11
---

# Analytics Design

## 1. Purpose

This document defines the analytics architecture for AILPG.

The analytics system captures learning activity and converts raw events into useful insights for:

```text
Students
Teachers
Course Owners
Administrators
Platform Operators
AI Quality Teams
```

Core flow:

```text
Student Activity
       ↓
Event Collector
       ↓
Message Queue
       ↓
Event Processor
       ↓
Analytics Storage
       ↓
Aggregations
       ↓
Dashboards
       ↓
Insights
```

---

# 2. Analytics Goals

The platform should measure:

```text
Lesson engagement
Video watch time
Video completion
Video drop-off
Checkpoint performance
Question accuracy
Question attempts
Concept mastery
Lesson completion
Course completion
Student activity
Teacher content performance
AI-generated content quality
Platform health
```

---

# 3. Analytics Architecture

```text
                    STUDENT
                       │
                       ▼
                  Web / Mobile
                       │
                       ▼
                 Event Collector
                       │
                       ▼
                  Event Queue
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
      Processor     Validator    Enricher
          │
          ▼
                 Analytics Storage
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
     Aggregations   Reports      Insights
          │            │            │
          └────────────┼────────────┘
                       ▼
                Dashboard APIs
                       │
              ┌────────┴────────┐
              ▼                 ▼
         Teacher Dashboard   Admin Dashboard
```

---

# 4. Analytics Layers

```text
Layer 1 — Event Collection
Layer 2 — Event Validation
Layer 3 — Event Processing
Layer 4 — Raw Event Storage
Layer 5 — Aggregation
Layer 6 — Analytics API
Layer 7 — Dashboard
```

---

# 5. Event Types

Core events:

```text
LESSON_STARTED
LESSON_RESUMED
VIDEO_PLAYED
VIDEO_PAUSED
VIDEO_SEEKED
VIDEO_COMPLETED
CHECKPOINT_REACHED
CHECKPOINT_COMPLETED
QUESTION_OPENED
QUESTION_SUBMITTED
QUESTION_CORRECT
QUESTION_INCORRECT
HINT_OPENED
TRANSCRIPT_OPENED
TRANSCRIPT_SEARCHED
LANGUAGE_CHANGED
QUALITY_CHANGED
LESSON_COMPLETED
COURSE_COMPLETED
```

---

# 6. Event Envelope

Every event should use a common structure.

```json
{
  "event_id": "evt_001",
  "event_type": "QUESTION_SUBMITTED",
  "user_id": "user_001",
  "lesson_id": "lesson_001",
  "course_id": "course_001",
  "session_id": "session_001",
  "timestamp": "2026-08-11T10:30:00Z",
  "properties": {}
}
```

---

# 7. Event ID

Every event must have a unique:

```text
event_id
```

This enables:

```text
Deduplication
Retry safety
Debugging
Audit
```

---

# 8. Session ID

A student learning session should have:

```text
session_id
```

Example:

```text
Student opens lesson
       ↓
session_001
       ↓
Watch
       ↓
Pause
       ↓
Question
       ↓
Complete
```

A later visit creates another session.

---

# 9. Event Timestamp

Store:

```text
client timestamp
server received timestamp
```

Server timestamp should be used for authoritative ordering where appropriate.

---

# 10. Video Analytics

Track:

```text
Start
Pause
Resume
Seek
Buffer
Quality change
Playback error
Completion
```

---

# 11. Watch Time

Watch time should represent actual playback activity rather than simply page-open duration.

Example:

```text
Lesson opened       10:00
Video played        10:02
Video paused        10:07

Watch time = 5 seconds
```

---

# 12. Watch Percentage

Formula:

```text
watch_percentage =
watched_duration / video_duration × 100
```

Example:

```text
Watched = 480 sec
Duration = 600 sec

Watch percentage = 80%
```

The implementation should define how repeated seeks/replays are handled.

---

# 13. Drop-Off Analysis

Identify timestamps where students stop watching.

Example:

```text
00:00 ───────────────────────── 12:00
          ▲
          │
       Drop-off
        03:45
```

This helps teachers identify difficult or uninteresting sections.

---

# 14. Drop-Off Aggregation

For each lesson:

```text
timestamp
students_reaching_point
students_leaving_point
drop_off_rate
```

---

# 15. Video Engagement Curve

Example conceptual data:

```text
Time      Remaining Students

00:00     100%
02:00      95%
04:00      88%
06:00      72%
08:00      68%
10:00      55%
12:00      48%
```

This can be visualized as an engagement curve.

---

# 16. Rewatch Analytics

Track repeated viewing.

```text
First watch
     ↓
Replay section
     ↓
Replay formula
     ↓
Replay checkpoint
```

Useful metrics:

```text
Replay count
Replay duration
Most replayed timestamp
```

---

# 17. Question Analytics

Track:

```text
Question viewed
Question answered
Correct
Incorrect
Attempts
Hints
Time to answer
```

---

# 18. Question Accuracy

Formula:

```text
accuracy =
correct_attempts / total_attempts × 100
```

Example:

```text
Correct = 80
Attempts = 100

Accuracy = 80%
```

---

# 19. First-Attempt Accuracy

Separate:

```text
First attempt correct
```

from:

```text
Eventually correct
```

Example:

```text
First attempt: 60%
Final success: 90%
```

This gives a better view of actual understanding.

---

# 20. Question Difficulty Analytics

For each question:

```text
Attempts
Correct
Incorrect
Average attempts
Average answer time
Hint usage
```

---

# 21. Question Difficulty Signal

Example:

```text
Question Q15

Accuracy: 32%
Average attempts: 2.8
Hint usage: 67%
```

This may indicate:

```text
Hard question
Poor explanation
Ambiguous wording
Prerequisite gap
```

It should be treated as a signal, not an automatic diagnosis.

---

# 22. Concept Analytics

Questions should be mapped to concepts.

Example:

```text
Question
   ↓
Concept
   ↓
Quadratic Factorization
```

Aggregate:

```text
Student performance by concept
```

---

# 23. Concept Mastery

Concept mastery can combine:

```text
Question accuracy
First-attempt accuracy
Recent performance
Difficulty
Repeated practice
```

Example:

```text
Factorization       92%
Quadratic Roots     76%
Discriminant        54%
```

The exact mastery algorithm should be versioned.

---

# 24. Mastery Levels

Example:

```text
0–39%   Needs Support
40–59%  Developing
60–79%  Proficient
80–100% Strong
```

Thresholds should be configurable.

---

# 25. Student Learning Profile

The platform may maintain:

```text
Lessons completed
Average accuracy
Concept mastery
Watch behavior
Question behavior
Recent activity
```

Avoid creating sensitive personal profiles that are unnecessary for the educational purpose.

---

# 26. Student Dashboard Metrics

Student can see:

```text
Courses in progress
Lessons completed
Learning time
Quiz accuracy
Concept strengths
Concepts needing practice
Recent activity
```

---

# 27. Teacher Dashboard

Teacher dashboard should show:

```text
Total students
Active students
Lesson completion
Average watch percentage
Question accuracy
Weak concepts
Drop-off timestamps
Most replayed sections
```

---

# 28. Lesson Analytics Dashboard

Example:

```text
Lesson: Quadratic Equations

Students: 420

Completion: 78%
Average Watch: 71%
Average Score: 76%

Most Difficult Concept:
Factorisation

Highest Drop-off:
06:42

Most Replayed:
03:15
```

---

# 29. Course Analytics

Course-level metrics:

```text
Enrollment
Active students
Completion rate
Average score
Average learning time
Lesson drop-off
Concept mastery
```

---

# 30. Course Funnel

Example:

```text
Enrolled
  1000
    ↓
Started
   820
    ↓
Completed 1st Lesson
   740
    ↓
Completed 50%
   590
    ↓
Completed Course
   430
```

---

# 31. Teacher Content Quality

Teachers should see which lessons need improvement.

Signals:

```text
High drop-off
Low question accuracy
High replay
Low completion
High error rate
```

---

# 32. AI Content Analytics

Track AI-generated content performance.

For questions:

```text
AI question
 ↓
Student attempts
 ↓
Accuracy
 ↓
Teacher edits
 ↓
Regeneration
```

---

# 33. AI Quality Metrics

Track:

```text
AI acceptance rate
Teacher edit rate
Teacher rejection rate
Regeneration rate
Question error rate
Formula correction rate
Translation correction rate
```

---

# 34. AI Model Comparison

Example:

```text
Model A
Acceptance: 92%

Model B
Acceptance: 85%
```

This allows evaluation of model performance.

---

# 35. Prompt Version Analytics

Example:

```text
Prompt v1
Acceptance = 78%

Prompt v2
Acceptance = 91%
```

Prompt changes can therefore be evaluated using production data.

---

# 36. Translation Analytics

Track:

```text
Translation generated
Translation viewed
Teacher edited
Student selected language
Translation errors reported
```

---

# 37. Language Analytics

Example:

```text
English     55%
Tamil       28%
Hindi        9%
Telugu       5%
Other        3%
```

This helps prioritize localization work.

---

# 38. Playback Quality Analytics

Track:

```text
Startup time
Buffering time
Playback failures
CDN errors
Quality switches
Video errors
```

---

# 39. Buffering Ratio

Conceptually:

```text
buffering_ratio =
buffering_time / playback_time
```

Lower is better.

---

# 40. Quality Switching

Track:

```text
1080p → 720p
720p → 480p
480p → 720p
```

Frequent downward switches may indicate network limitations.

---

# 41. Device Analytics

Capture non-sensitive technical information such as:

```text
Device category
Browser
Operating system
Screen size class
Network type where available
```

Avoid collecting unnecessary personal information.

---

# 42. Error Analytics

Events:

```text
VIDEO_LOAD_ERROR
PLAYBACK_ERROR
MANIFEST_ERROR
QUESTION_LOAD_ERROR
API_ERROR
PROGRESS_SYNC_ERROR
```

---

# 43. Error Monitoring

Each error should contain:

```text
error_code
event_id
user/session reference
lesson reference
timestamp
client version
```

Do not expose internal stack traces to students.

---

# 44. Analytics Event Pipeline

```text
Frontend
   ↓
Event API
   ↓
Validation
   ↓
Queue
   ↓
Stream Processor
   ↓
Raw Events
   ↓
Aggregation
   ↓
Analytics Database
```

---

# 45. Event Batching

High-frequency events should be batched.

Example:

```text
Playback updates
     ↓
Local buffer
     ↓
Batch
     ↓
Send
```

This reduces API traffic.

---

# 46. Event Reliability

If event transmission fails:

```text
Event
 ↓
Local Queue
 ↓
Retry
 ↓
Server
```

Events should have idempotency protection.

---

# 47. Duplicate Event Handling

Use:

```text
event_id
```

to prevent duplicate processing.

Example:

```text
evt_123 received
     ↓
Processed

evt_123 received again
     ↓
Ignore duplicate
```

---

# 48. Raw Event Storage

Keep immutable raw events where required.

```text
Raw Event
   ↓
Immutable
```

Aggregated tables can be rebuilt from raw events when appropriate.

---

# 49. Analytics Storage

Architecture may use:

```text
Operational DB
        +
Analytics DB / Warehouse
```

Operational database:

```text
Current progress
Current completion
```

Analytics system:

```text
Historical events
Aggregations
Reports
```

---

# 50. Data Model

Core analytics entities:

```text
analytics_events
learning_sessions
video_watch_events
question_attempts
lesson_progress
lesson_completions
concept_performance
daily_learning_metrics
```

Detailed schema belongs to:

```text
Layer 6 — Database Design
```

---

# 51. Aggregation Jobs

Examples:

```text
Hourly Aggregation
Daily Aggregation
Weekly Aggregation
Course Aggregation
Lesson Aggregation
Student Aggregation
```

---

# 52. Real-Time Analytics

Some metrics should update quickly:

```text
Active students
Current lesson activity
Live processing status
Current question attempts
```

---

# 53. Batch Analytics

Other metrics can be calculated periodically:

```text
Weekly performance
Concept mastery
Course trends
AI quality
Long-term engagement
```

---

# 54. Teacher Alerts

Optional future capability:

```text
High drop-off detected
Low question accuracy
Lesson completion declining
Question appears too difficult
```

Example:

```text
⚠ Students are dropping off around 06:40.
Consider reviewing this section.
```

Alerts should be actionable and avoid overstating causality.

---

# 55. Student Recommendations

Analytics can feed recommendations:

```text
Weak Concept
    ↓
Recommended Lesson
    ↓
Practice Questions
```

Example:

```text
Your factorisation accuracy is low.

Recommended:
"Factorisation Basics"
```

---

# 56. Recommendation Safety

Recommendations should be based on learning signals and configurable rules.

Avoid presenting inferred mastery as a medical, psychological, or otherwise sensitive diagnosis.

---

# 57. Privacy

Analytics must follow:

```text
Data minimization
Purpose limitation
Access control
Retention policy
Deletion policy
Audit logging
```

---

# 58. Teacher Access Control

Teacher can only access analytics for:

```text
Owned courses
Assigned courses
Authorized organization
```

---

# 59. Admin Analytics

Platform administrators may see:

```text
Platform usage
Storage
AI usage
Processing jobs
System errors
Course activity
```

Tenant boundaries must still be enforced.

---

# 60. Organization Analytics

Organizations can see aggregated:

```text
Student activity
Course performance
Teacher performance
Usage
AI consumption
```

Only authorized organization administrators should access organization-level data.

---

# 61. Analytics Retention

Retention should be configurable.

Example:

```text
Raw events       → 12 months
Aggregates       → 36 months
Operational data → Product policy
```

Actual retention must follow applicable legal and product requirements.

---

# 62. Data Deletion

When user data must be deleted:

```text
User Request
 ↓
Identity Verification
 ↓
Data Discovery
 ↓
Deletion / Anonymization
 ↓
Audit
```

Analytics systems must be included in the deletion workflow where applicable.

---

# 63. Analytics API

Conceptual endpoints:

```text
GET /api/v1/analytics/student
GET /api/v1/analytics/student/courses
GET /api/v1/analytics/lessons/{lesson_id}
GET /api/v1/analytics/courses/{course_id}
GET /api/v1/analytics/courses/{course_id}/students
GET /api/v1/analytics/questions/{question_id}
GET /api/v1/analytics/concepts/{concept_id}
GET /api/v1/analytics/ai
```

Exact contracts belong to:

```text
Layer 7 — API Design
```

---

# 64. Dashboard Filters

Teacher dashboard should support:

```text
Date range
Course
Lesson
Class
Student group
Language
Question
Concept
```

---

# 65. Export

Authorized users may export:

```text
CSV
Excel
PDF report
```

Export should respect permissions and data privacy policies.

---

# 66. Analytics Visualization

Recommended visualizations:

```text
Completion → KPI
Watch time → KPI
Engagement → Line chart
Question accuracy → Bar chart
Concept mastery → Bar chart
Drop-off → Line chart
Course funnel → Funnel
Language usage → Distribution chart
```

---

# 67. Analytics Performance

Dashboard APIs should use:

```text
Pre-aggregated data
Caching
Pagination
Date partitioning
Indexed queries
```

Avoid scanning millions of raw events for every dashboard request.

---

# 68. Analytics Security

Protect:

```text
Student analytics
Teacher analytics
Organization analytics
AI cost data
Platform operational metrics
```

Use:

```text
RBAC
Tenant filtering
Audit logs
API authorization
```

---

# 69. Analytics Observability

Monitor:

```text
Event ingestion rate
Event processing latency
Dropped events
Queue depth
Aggregation failures
Dashboard latency
Storage growth
```

---

# 70. Analytics Failure Handling

If analytics is temporarily unavailable:

```text
Learning experience
        ↓
Continue
```

Analytics should generally be non-blocking.

Example:

```text
Student submits answer
       ↓
Answer validation succeeds
       ↓
Analytics event fails
       ↓
Student still receives result
```

---

# 71. Analytics Architecture Principle

Critical learning operations:

```text
Answer validation
Progress persistence
Lesson completion
```

must not depend synchronously on analytics processing.

---

# 72. End-to-End Example

Student watches:

```text
Lesson: Quadratic Equations
```

Events:

```text
LESSON_STARTED
VIDEO_PLAYED
VIDEO_PAUSED
CHECKPOINT_REACHED
QUESTION_OPENED
QUESTION_SUBMITTED
QUESTION_INCORRECT
QUESTION_SUBMITTED
QUESTION_CORRECT
VIDEO_PLAYED
VIDEO_COMPLETED
LESSON_COMPLETED
```

Analytics processor converts these into:

```text
Watch Time: 10m 25s
Completion: 100%
Questions: 3
Correct: 2
Accuracy: 66.7%
```

---

# 73. Teacher Insight

Analytics may surface:

```text
Lesson completion: 82%

Largest drop-off:
06:40

Question Q3:
Accuracy 41%

Concept:
Quadratic Factorisation
Mastery 58%
```

Teacher can then:

```text
Review video section
Edit explanation
Regenerate question
Add another example
```

---

# 74. Analytics Feedback Loop

```text
Student Activity
       ↓
Analytics
       ↓
Insight
       ↓
Teacher Improvement
       ↓
Better Lesson
       ↓
Better Student Performance
```

This creates the continuous improvement loop of AILPG.

---

# 75. AI + Analytics Feedback Loop

```text
AI Generated Question
       ↓
Student Performance
       ↓
Question Analytics
       ↓
Teacher Review
       ↓
AI Quality Metrics
       ↓
Prompt / Model Improvement
```

---

# 76. Definition of Done

```text
Event Collection              ✓
Event Validation              ✓
Event Queue                   ✓
Raw Event Storage             ✓
Watch Analytics               ✓
Drop-off Analytics            ✓
Question Analytics            ✓
Concept Analytics             ✓
Student Analytics             ✓
Teacher Analytics             ✓
Course Analytics              ✓
AI Analytics                  ✓
Playback Analytics            ✓
Language Analytics            ✓
Error Analytics               ✓
Dashboard API                 ✓
Privacy Controls              ✓
Retention Policy              ✓
Export                         ✓
Monitoring                    ✓
```

---

# 77. Next Document

```text
10_Security_Design.md
```

This document will define the complete security architecture:

```text
Authentication
Authorization
RBAC
Multi-tenancy
API Security
JWT / Session Security
File Security
Video Protection
AI Security
Data Encryption
Secrets
Audit Logs
Rate Limiting
Input Validation
Prompt Injection Protection
Privacy
Backup Security
Incident Response
```

---

# 78. Revision History

| Version | Date | Description |
|---|---|---|
| 1.0.0 | 2026-08-11 | Initial analytics design |
