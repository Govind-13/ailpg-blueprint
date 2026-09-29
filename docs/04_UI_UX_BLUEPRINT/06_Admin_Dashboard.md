# AILPG — Admin Dashboard UI/UX Blueprint

**Project:** MP4 → Interactive Learning Platform Generator
**Short Name:** AILPG
**Document:** Admin Dashboard UI/UX Blueprint
**Path:** `04_UI_UX_Blueprint/06_Admin_Dashboard.md`
**Version:** 1.0.0
**Status:** Draft / GitHub Ready

---

# 1. Overview

The AILPG Admin Dashboard is the central control interface for platform administrators.

It provides administrators with visibility and control over:

* Users
* Teachers
* Students
* Uploaded videos
* AI processing jobs
* Interactive lessons
* Questions
* Translations
* Subscriptions
* Payments
* System configuration
* AI providers
* Storage
* Analytics
* Security
* Audit logs
* Platform health

The dashboard should provide an operational view of the entire AILPG platform.

---

# 2. Admin Dashboard Objective

The primary objective is:

> Provide administrators with a secure, centralized interface for monitoring, managing, configuring and troubleshooting the AILPG platform.

The dashboard should allow an administrator to understand the state of the system without accessing databases or server terminals directly.

---

# 3. Design Principles

The Admin Dashboard should follow these principles:

1. **Clarity**
2. **Consistency**
3. **Security**
4. **Fast navigation**
5. **Action visibility**
6. **Error visibility**
7. **Data transparency**
8. **Responsive design**
9. **Accessibility**
10. **Auditability**

Administrative actions should be clearly distinguishable from informational data.

---

# 4. Dashboard Layout

Recommended desktop layout:

```text
┌─────────────────────────────────────────────────────────────────────┐
│ AILPG ADMIN        Search...             Notifications   Admin ▼   │
├───────────────┬─────────────────────────────────────────────────────┤
│               │                                                     │
│ Dashboard     │                  PAGE CONTENT                      │
│               │                                                     │
│ Users         │                                                     │
│ Teachers      │                                                     │
│ Students      │                                                     │
│ Videos        │                                                     │
│ AI Jobs       │                                                     │
│ Lessons       │                                                     │
│ Questions     │                                                     │
│ Translations  │                                                     │
│ Subscriptions │                                                     │
│ Analytics     │                                                     │
│               │                                                     │
│ System        │                                                     │
│ Settings      │                                                     │
│ Audit Logs    │                                                     │
│               │                                                     │
│ ───────────   │                                                     │
│ Help          │                                                     │
│ Logout        │                                                     │
└───────────────┴─────────────────────────────────────────────────────┘
```

---

# 5. Main Navigation

The sidebar should contain the following sections.

```text
Dashboard

Content
├── Videos
├── Lessons
├── Questions
└── Translations

Users
├── All Users
├── Students
├── Teachers
└── Administrators

AI & Processing
├── Processing Jobs
├── AI Providers
├── AI Usage
└── Failed Jobs

Commerce
├── Plans
├── Subscriptions
├── Payments
└── Coupons

Analytics
├── Platform Analytics
├── Learning Analytics
├── Content Analytics
└── AI Analytics

System
├── System Health
├── Storage
├── Notifications
├── Settings
└── Audit Logs
```

---

# 6. Admin Dashboard Home

The dashboard homepage should provide an operational summary.

## 6.1 KPI Cards

Recommended cards:

```text
┌────────────────┐ ┌────────────────┐ ┌────────────────┐
│ Total Users    │ │ Total Lessons  │ │ Total Videos   │
│ 12,840         │ │ 1,248          │ │ 1,530          │
│ +8.4%          │ │ +12.2%         │ │ +10.1%         │
└────────────────┘ └────────────────┘ └────────────────┘

┌────────────────┐ ┌────────────────┐ ┌────────────────┐
│ Active Users   │ │ AI Jobs        │ │ Failed Jobs    │
│ 4,823          │ │ 238            │ │ 7              │
└────────────────┘ └────────────────┘ └────────────────┘
```

Actual values must come from backend APIs.

---

# 7. Processing Overview

The dashboard should show current AI processing activity.

Example:

```text
AI PROCESSING

Queued        24
Processing    18
Completed     192
Failed         7

Overall Processing Health
██████████████████░░  91%
```

Clicking the section should navigate to:

`/admin/ai/jobs`

---

# 8. Recent Activity

Show recent administrative and platform events.

Example:

```text
Recent Activity

● Teacher uploaded "Quadratic Equations.mp4"
  2 minutes ago

● Lesson generated successfully
  5 minutes ago

● Translation completed: Tamil
  8 minutes ago

● New teacher registered
  12 minutes ago

● AI processing failed
  15 minutes ago
```

Each activity should contain:

* Event type
* User/system actor
* Object
* Timestamp
* Status
* Link to details

---

# 9. System Health Widget

The dashboard should show the health of major services.

```text
SYSTEM HEALTH

API Server             ● Operational
Database               ● Operational
Object Storage         ● Operational
AI Processing Queue    ● Operational
Video Processing       ● Operational
Translation Service    ● Operational
Email Service          ● Operational
Payment Service        ● Operational
```

Status states:

```text
Operational
Degraded
Warning
Unavailable
Unknown
```

---

# 10. User Management

## Route

```text
/admin/users
```

The user-management page should support:

* Search
* Filtering
* Sorting
* Pagination
* User status
* Role filtering
* Date filtering
* Bulk actions

Example:

```text
Users

[ Search users... ] [Role ▼] [Status ▼] [Date ▼]

┌──────────┬──────────────┬──────────┬──────────┬────────────┐
│ Name     │ Email        │ Role     │ Status   │ Created    │
├──────────┼──────────────┼──────────┼──────────┼────────────┤
│ User A   │ user@...     │ Student  │ Active   │ 2026-09-01 │
│ User B   │ teacher@...  │ Teacher  │ Active   │ 2026-08-20 │
└──────────┴──────────────┴──────────┴──────────┴────────────┘
```

---

# 11. User Detail Page

## Route

```text
/admin/users/:userId
```

Sections:

```text
Profile
Activity
Lessons
Subscriptions
Payments
Sessions
Security
Audit History
```

The administrator should be able to view account information according to their permission level.

---

# 12. User Actions

Possible actions:

```text
View
Edit
Suspend
Activate
Reset access
Change role
View activity
View subscriptions
View audit history
```

Sensitive actions should require confirmation.

Example:

```text
Suspend User?

This user will no longer be able to access
the student platform.

[Cancel] [Confirm Suspension]
```

---

# 13. Teacher Management

## Route

```text
/admin/teachers
```

Teacher-specific information:

* Name
* Email
* Account status
* Lessons created
* Videos uploaded
* Students
* Subscription
* Content status
* Last activity

Example:

```text
Teacher Performance

Teacher             Lessons   Videos   Students
------------------------------------------------
Teacher A             24        31       540
Teacher B             18        22       310
```

The dashboard should display factual activity metrics without assigning subjective performance scores unless such scoring is explicitly defined as a product feature.

---

# 14. Student Management

## Route

```text
/admin/students
```

Possible information:

* Account status
* Lessons started
* Lessons completed
* Questions attempted
* Subscription
* Last activity
* Usage

---

# 15. Video Management

## Route

```text
/admin/videos
```

The video-management interface should provide:

```text
Videos

[Search...] [Status ▼] [Teacher ▼] [Date ▼]

┌──────────────────┬───────────┬──────────┬────────────┐
│ Video            │ Teacher   │ Status   │ Uploaded   │
├──────────────────┼───────────┼──────────┼────────────┤
│ Algebra.mp4      │ Teacher A │ Ready    │ Sep 20     │
│ Fractions.mp4    │ Teacher B │ Process  │ Sep 21     │
│ Geometry.mp4     │ Teacher A │ Failed   │ Sep 22     │
└──────────────────┴───────────┴──────────┴────────────┘
```

---

# 16. Video Detail

## Route

```text
/admin/videos/:videoId
```

Display:

* Video metadata
* Thumbnail
* Duration
* File size
* Resolution
* Codec
* Owner
* Upload date
* Processing status
* Generated lesson
* Transcript
* Processing history

---

# 17. Video Processing Status

Use a clear state machine.

```text
UPLOADED
   ↓
VALIDATING
   ↓
QUEUED
   ↓
PROCESSING
   ↓
ANALYZING
   ↓
TRANSCRIBING
   ↓
MATH_EXTRACTION
   ↓
QUESTION_GENERATION
   ↓
TRANSLATION
   ↓
LESSON_GENERATION
   ↓
QUALITY_CHECK
   ↓
READY
```

Failure path:

```text
Any Stage
   ↓
FAILED
   ↓
Retry
```

---

# 18. AI Jobs Dashboard

## Route

```text
/admin/ai/jobs
```

This is one of the most important operational pages.

Columns:

```text
Job ID
Video
Job Type
Provider
Status
Started
Duration
Tokens
Cost
Attempts
Created
```

Example:

```text
JOB-10293
Algebra.mp4
Transcription
Whisper
Completed
02:31
$0.08
1 attempt
```

---

# 19. AI Job Detail

## Route

```text
/admin/ai/jobs/:jobId
```

Display:

```text
Job Information

Job ID:
JOB-10293

Status:
Completed

Provider:
AI Provider

Started:
10:21:03

Completed:
10:23:34

Duration:
2m 31s
```

Processing logs:

```text
10:21:03 Job created
10:21:04 Input validated
10:21:12 Audio extracted
10:21:30 Transcription started
10:23:10 Transcription completed
10:23:34 Job completed
```

---

# 20. Failed Jobs

## Route

```text
/admin/ai/failed
```

Display:

* Job ID
* Error type
* Error message
* Failed stage
* Retry count
* Last attempt
* Retry action

Example:

```text
┌───────────┬──────────────┬────────────┬─────────┐
│ Job       │ Stage        │ Error      │ Action  │
├───────────┼──────────────┼────────────┼─────────┤
│ JOB-123   │ Translation  │ Timeout    │ Retry   │
│ JOB-124   │ OCR          │ Invalid    │ Retry   │
└───────────┴──────────────┴────────────┴─────────┘
```

---

# 21. Lesson Management

## Route

```text
/admin/lessons
```

Lesson states:

```text
Draft
Processing
Review
Published
Archived
Failed
```

Filters:

* Teacher
* Subject
* Language
* Status
* Created date
* Updated date

---

# 22. Lesson Detail

## Route

```text
/admin/lessons/:lessonId
```

Sections:

```text
Overview
Video
Transcript
Lesson Structure
Questions
Translations
Versions
Students
Analytics
Audit Log
```

---

# 23. Question Management

## Route

```text
/admin/questions
```

Administrators should be able to inspect generated questions.

Information:

* Question
* Question type
* Lesson
* Timestamp
* Difficulty
* Correct answer
* Explanation
* Generation status
* Validation status

Example:

```text
Question

At 02:34

What is the value of x?

Type:
Multiple Choice

Validation:
✓ Passed
```

---

# 24. Translation Management

## Route

```text
/admin/translations
```

Display:

```text
Lesson
Source Language
Target Language
Status
Provider
Created
Updated
```

Statuses:

```text
Pending
Processing
Completed
Failed
Needs Review
```

---

# 25. Subscription Management

## Route

```text
/admin/subscriptions
```

Display:

* Plan
* User
* Status
* Start date
* Renewal date
* Expiry date
* Billing provider
* Payment status

---

# 26. Plan Management

## Route

```text
/admin/plans
```

Administrators should be able to configure product entitlements.

Example:

```text
Plan: Premium

Video Quality
✓ 1080p
✓ 720p
✓ 480p

Features
✓ Interactive lessons
✓ Advanced analytics
✓ Multiple languages
```

Plan configuration should be stored in the backend.

Frontend applications should not be the authoritative source for entitlement decisions.

---

# 27. Payment Management

## Route

```text
/admin/payments
```

Information:

* Transaction ID
* User
* Plan
* Amount
* Currency
* Payment status
* Provider
* Date
* Refund status

Sensitive financial information should be appropriately protected and access-controlled.

---

# 28. Platform Analytics

## Route

```text
/admin/analytics
```

Dashboard sections:

```text
User Growth
Lesson Growth
Video Processing
Learning Engagement
Question Accuracy
AI Usage
Storage Usage
Subscription Activity
```

Example:

```text
Monthly Platform Activity

Users
│
│          ●
│       ●
│    ●
│ ●
└──────────────────
 Jan Feb Mar Apr
```

Charts should support:

* Date range
* Daily
* Weekly
* Monthly
* Export

---

# 29. Learning Analytics

Important metrics:

```text
Lessons Started
Lessons Completed
Average Completion
Questions Attempted
Questions Correct
Question Retry Rate
Average Watch Percentage
Average Learning Session
```

These should be presented as measurements rather than unexplained quality scores.

---

# 30. AI Analytics

Display:

```text
AI Usage

Total Jobs
Successful Jobs
Failed Jobs
Average Processing Time
Token Usage
Estimated Cost
Provider Usage
```

Example:

```text
AI Provider Usage

Provider A       62%
Provider B       24%
Provider C       14%
```

---

# 31. Storage Dashboard

## Route

```text
/admin/system/storage
```

Display:

```text
Storage

Original Videos      2.4 TB
Processed Videos     1.1 TB
Audio Files          310 GB
Thumbnails            24 GB
Generated Lessons     18 GB
Total                3.85 TB
```

Include:

* Storage utilization
* Largest files
* File counts
* Retention status
* Cleanup candidates

Deletion operations should require appropriate authorization.

---

# 32. System Health

## Route

```text
/admin/system/health
```

Monitor:

```text
API
Database
Cache
Queue
Object Storage
AI Providers
Translation
Email
Payments
CDN
```

Each service should expose a health state.

---

# 33. Notifications

## Route

```text
/admin/notifications
```

Administrators can monitor system-generated notifications.

Examples:

* Processing completed
* Processing failed
* Payment failed
* Subscription expiring
* System warning
* Security event

---

# 34. System Settings

## Route

```text
/admin/settings
```

Categories:

```text
General
Authentication
Video
AI
Translation
Storage
Email
Payments
Subscriptions
Security
Notifications
```

Dangerous settings should require additional confirmation.

---

# 35. AI Provider Configuration

## Route

```text
/admin/settings/ai
```

Possible configuration:

```text
AI Provider
API Status
Model
Timeout
Retry Limit
Token Limit
Usage Limit
Fallback Provider
```

API keys should never be displayed as plaintext.

Use:

```text
••••••••••••••••
```

with controlled rotation functionality.

---

# 36. Audit Logs

## Route

```text
/admin/audit-logs
```

Every important administrative action should be recorded.

Example:

```text
Timestamp
Actor
Action
Resource
Resource ID
IP / Session Reference
Result
```

Example:

```text
2026-09-29 10:32
Admin A
Updated Plan
Premium
PLAN-002
Success
```

Audit logs should be append-oriented and protected from unauthorized modification.

---

# 37. Role-Based Access Control

The admin system should support granular permissions.

Example roles:

```text
Super Admin
Admin
Content Admin
Support Admin
Finance Admin
Analytics Admin
```

Example permission model:

```text
users.read
users.update
users.suspend

videos.read
videos.delete

lessons.read
lessons.publish
lessons.archive

ai.jobs.read
ai.jobs.retry

subscriptions.read
subscriptions.manage

analytics.read

settings.read
settings.update

audit.read
```

---

# 38. Permission Matrix

| Capability         | Super Admin | Admin | Content Admin | Support | Finance |
| ------------------ | ----------: | ----: | ------------: | ------: | ------: |
| View users         |           ✓ |     ✓ |             ✓ |       ✓ | Limited |
| Edit users         |           ✓ |     ✓ |             — | Limited |       — |
| Suspend users      |           ✓ |     ✓ |             — | Limited |       — |
| Manage videos      |           ✓ |     ✓ |             ✓ |       — |       — |
| Manage lessons     |           ✓ |     ✓ |             ✓ |       — |       — |
| Publish lessons    |           ✓ |     ✓ |             ✓ |       — |       — |
| Retry AI jobs      |           ✓ |     ✓ |             ✓ |       — |       — |
| View analytics     |           ✓ |     ✓ |             ✓ | Limited |       ✓ |
| Manage plans       |           ✓ |     ✓ |             — |       — |       ✓ |
| Payment management |           ✓ |     ✓ |             — |       — |       ✓ |
| System settings    |           ✓ |     ✓ |             — |       — |       — |
| Audit logs         |           ✓ |     ✓ |       Limited | Limited | Limited |

The final permission model should be configurable rather than permanently hard-coded.

---

# 39. Global Search

The dashboard should provide global search.

Searchable objects:

```text
Users
Teachers
Students
Videos
Lessons
Questions
AI Jobs
Transactions
Subscriptions
```

Example:

```text
Search: "Algebra"

Results

Lessons
  Algebra Basics

Videos
  Algebra.mp4

AI Jobs
  JOB-10321
```

---

# 40. Filters

Tables should support reusable filters.

Common filters:

```text
Status
Role
Language
Teacher
Date
Processing State
Subscription
Plan
Provider
```

Filters should be combinable.

Example:

```text
Status = Failed
+
Job Type = Translation
+
Language = Tamil
+
Date = Last 7 Days
```

---

# 41. Bulk Operations

Where safe and appropriate:

```text
Select multiple
       ↓
Bulk Action
       ↓
Confirmation
       ↓
Execute
       ↓
Result Summary
```

Example:

```text
Selected: 8 failed jobs

[Retry Selected]

8 jobs selected.
Retry processing?

[Cancel] [Retry]
```

Bulk destructive operations should have stronger confirmation requirements.

---

# 42. Confirmation Dialogs

Administrative actions should clearly explain consequences.

Example:

```text
Archive Lesson?

Lesson:
Quadratic Equations

Archived lessons will no longer be
available for normal student discovery.

[Cancel] [Archive Lesson]
```

---

# 43. Error Handling

Errors should be understandable.

Bad:

```text
Error 500
```

Better:

```text
Unable to retry this processing job.

The AI processing service did not respond.

Job ID: JOB-12345

[Try Again] [View Logs]
```

Do not expose internal secrets, stack traces or sensitive infrastructure information to administrators unless their permission level explicitly allows diagnostic details.

---

# 44. Empty States

Every data table should have a useful empty state.

Example:

```text
No failed jobs

All recent AI processing jobs completed
successfully.

[View All Jobs]
```

---

# 45. Loading States

Use skeleton loading for large dashboard components.

Example:

```text
┌─────────────────────┐
│ ███████████         │
│ ███████             │
│ ███████████████     │
└─────────────────────┘
```

Avoid blank screens while data is loading.

---

# 46. Responsive Design

Desktop is the primary administrative environment.

The dashboard should also support:

* Tablet
* Mobile

On mobile:

```text
┌─────────────────────┐
│ ☰  AILPG ADMIN   🔔 │
├─────────────────────┤
│                     │
│ Total Users         │
│ 12,840              │
│                     │
│ AI Jobs             │
│ 238                 │
│                     │
│ Failed              │
│ 7                   │
│                     │
└─────────────────────┘
```

Complex tables should become horizontally scrollable or convert into card layouts.

---

# 47. Accessibility

The dashboard should follow accessibility best practices.

Requirements:

* Keyboard navigation
* Visible focus state
* Screen-reader labels
* Sufficient contrast
* Semantic HTML
* Accessible form labels
* Accessible dialogs
* Error announcements
* Reduced-motion support

---

# 48. Admin Session Security

Administrative sessions should have stronger security controls.

Recommended:

```text
Login
 ↓
Authentication
 ↓
MFA
 ↓
Admin Session
 ↓
Permission Check
 ↓
Action
 ↓
Audit Log
```

Session controls may include:

* Session timeout
* Device/session management
* MFA
* Login monitoring
* Suspicious-session detection
* Re-authentication for sensitive actions

---

# 49. Navigation Routes

Recommended route structure:

```text
/admin
/admin/users
/admin/users/:userId

/admin/teachers
/admin/students

/admin/videos
/admin/videos/:videoId

/admin/lessons
/admin/lessons/:lessonId

/admin/questions
/admin/translations

/admin/ai/jobs
/admin/ai/jobs/:jobId
/admin/ai/failed

/admin/plans
/admin/subscriptions
/admin/payments

/admin/analytics
/admin/analytics/learning
/admin/analytics/ai

/admin/system/health
/admin/system/storage
/admin/system/notifications

/admin/settings
/admin/settings/ai
/admin/settings/security

/admin/audit-logs
```

---

# 50. Component Architecture

Recommended reusable components:

```text
AdminLayout
├── Sidebar
├── Topbar
├── Breadcrumbs
└── NotificationCenter

Dashboard
├── KPI Card
├── Chart Card
├── Activity Feed
├── Health Status
└── Processing Summary

Data Management
├── DataTable
├── SearchBar
├── FilterPanel
├── Pagination
├── StatusBadge
├── ActionMenu
└── ConfirmationDialog

Forms
├── FormField
├── Select
├── Toggle
├── DatePicker
└── FileInput
```

---

# 51. Design System

The dashboard should use a consistent design system.

Define:

```text
Typography
Spacing
Grid
Buttons
Inputs
Tables
Cards
Badges
Dialogs
Alerts
Charts
Icons
Navigation
```

Status badges should consistently represent:

```text
Success
Warning
Error
Processing
Pending
Inactive
```

---

# 52. Admin UX Flow

Typical operational flow:

```text
Admin Login
     ↓
Dashboard
     ↓
Notice Failed Job
     ↓
Open AI Jobs
     ↓
Open Job Details
     ↓
Review Error
     ↓
Retry
     ↓
Monitor Processing
     ↓
Verify Completion
     ↓
Audit Event Recorded
```

---

# 53. Content Review Flow

```text
AI Generated Lesson
        ↓
Admin/Teacher Review
        ↓
Inspect Transcript
        ↓
Inspect Math Steps
        ↓
Inspect Questions
        ↓
Inspect Translation
        ↓
Preview Lesson
        ↓
Approve
        ↓
Publish
```

---

# 54. Admin Dashboard API Dependencies

The frontend should consume backend APIs rather than directly accessing the database.

Example:

```text
GET    /api/admin/dashboard
GET    /api/admin/users
GET    /api/admin/users/:id
GET    /api/admin/videos
GET    /api/admin/lessons
GET    /api/admin/ai/jobs
POST   /api/admin/ai/jobs/:id/retry
GET    /api/admin/analytics
GET    /api/admin/system/health
GET    /api/admin/audit-logs
```

All endpoints require authentication and authorization.

---

# 55. Dashboard Data Refresh

Real-time or near-real-time updates should be considered for:

* AI processing
* System health
* Notifications
* Job status

Possible technologies:

```text
WebSocket
Server-Sent Events
Polling
```

For the MVP, polling may be sufficient.

---

# 56. Security Requirements

The Admin Dashboard must never rely solely on frontend controls.

For every request:

```text
Request
  ↓
Authentication
  ↓
Authorization
  ↓
Permission Check
  ↓
Validation
  ↓
Business Logic
  ↓
Audit
  ↓
Response
```

The backend must independently enforce permissions.

---

# 57. Performance Requirements

Dashboard goals:

* Fast initial rendering
* Paginated tables
* Lazy-loaded analytics
* Efficient API queries
* Cached dashboard summaries
* Virtualized large datasets where required

Large datasets must not be loaded into the browser unnecessarily.

---

# 58. Auditability Requirements

The following actions should generate audit events:

* User suspension
* User role change
* Lesson publication
* Lesson archival
* Video deletion
* AI job retry
* Plan modification
* Subscription modification
* Payment-related administrative action
* System configuration change
* Security configuration change

---

# 59. MVP Admin Dashboard

The first release should include:

```text
✓ Admin Login
✓ Dashboard
✓ User Management
✓ Teacher Management
✓ Student Management
✓ Video Management
✓ Lesson Management
✓ AI Job Monitoring
✓ Failed Job Retry
✓ Question Management
✓ Translation Status
✓ Basic Analytics
✓ System Health
✓ Audit Logs
✓ Role-Based Permissions
```

---

# 60. Post-MVP Features

Later releases may add:

```text
Advanced AI cost optimization
Advanced analytics
Custom admin roles
Automated alerts
Advanced system monitoring
Multi-tenant administration
Advanced billing management
Data export
Scheduled reports
AI provider failover
Advanced content moderation
```

---

# 61. Definition of Done

The Admin Dashboard UI/UX specification is complete when:

* All primary admin workflows have defined screens.
* Navigation routes are documented.
* Roles and permissions are defined.
* Loading states are defined.
* Empty states are defined.
* Error states are defined.
* Confirmation flows are defined.
* Responsive behavior is defined.
* Accessibility requirements are documented.
* API dependencies are identified.
* Security requirements are defined.
* Audit requirements are defined.

---

# 62. Final Admin Architecture

```text
                         AILPG ADMIN
                              │
              ┌───────────────┴───────────────┐
              │                               │
          Dashboard                       Navigation
              │                               │
     ┌────────┼────────┐              ┌───────┴────────┐
     │        │        │              │                │
   Users    Content    AI          Commerce          System
     │        │        │              │                │
     │        │        │              │                │
 Students  Videos    Jobs          Plans            Health
 Teachers  Lessons   Providers     Payments          Settings
 Admins    Questions Usage         Subs              Audit
           Translation
              │
              └──────────────┬────────────────
                             │
                       Analytics
                             │
                    ┌────────┴────────┐
                    │                 │
                Learning             AI
                Analytics          Analytics
```

---

# 63. Summary

The AILPG Admin Dashboard is the operational command center of the platform.

It connects:

```text
Users
  +
Content
  +
AI Processing
  +
Learning
  +
Commerce
  +
Analytics
  +
System Operations
  +
Security
```

The dashboard should provide administrators with enough visibility to understand what is happening across the platform and enough controlled functionality to manage the system safely.

The core design principle is:

> **Observe → Understand → Act → Verify → Audit**

This principle should be maintained throughout the AILPG administrative experience.
