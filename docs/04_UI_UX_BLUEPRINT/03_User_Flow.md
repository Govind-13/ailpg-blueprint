# AILPG — User Flow

**Project:** AI Learning Platform Generator (AILPG)
**Layer:** UI/UX Blueprint
**Document:** 03 — User Flow
**Status:** Draft / Implementation Ready
**Version:** 1.0

---

# 1. Purpose

This document defines the end-to-end user flows for the AILPG platform.

The purpose is to ensure that every major user action has:

* A clear starting point
* A predictable sequence
* Defined UI states
* Success conditions
* Failure conditions
* Recovery paths
* Permission rules
* Navigation behavior

The primary AILPG workflow is:

```text
MP4 Upload
    ↓
Video Processing
    ↓
Transcript Extraction
    ↓
AI Analysis
    ↓
Question Generation
    ↓
Translation
    ↓
Interactive Lesson Generation
    ↓
Teacher Review
    ↓
Publish
    ↓
Student Learning
    ↓
Student Answers
    ↓
Analytics
```

---

# 2. User Types

AILPG contains three primary user roles.

```text
                    AILPG
                      │
          ┌───────────┼───────────┐
          │           │           │
       Student      Teacher      Admin
          │           │           │
       Learn        Create       Manage
```

---

# 3. Student Flow

Primary student journey:

```text
Login
  ↓
Dashboard
  ↓
Browse Courses
  ↓
Select Course
  ↓
Select Lesson
  ↓
Start Lesson
  ↓
Watch Video
  ↓
Reach Checkpoint
  ↓
Answer Question
  ↓
Receive Feedback
  ↓
Continue Video
  ↓
Complete Lesson
  ↓
View Results
  ↓
Continue Course
```

---

# 4. Teacher Flow

Primary teacher journey:

```text
Login
  ↓
Teacher Dashboard
  ↓
Create Course
  ↓
Create Module
  ↓
Upload MP4
  ↓
Configure AI
  ↓
Start Processing
  ↓
Monitor Processing
  ↓
AI Analysis Complete
  ↓
Review Generated Content
  ↓
Review Transcript
  ↓
Review Questions
  ↓
Review Translation
  ↓
Preview Lesson
  ↓
Edit
  ↓
Approve
  ↓
Publish
```

---

# 5. Admin Flow

Primary administrator journey:

```text
Login
  ↓
Admin Dashboard
  ↓
System Overview
  ↓
Users / Organizations
  ↓
Courses / Lessons
  ↓
AI Jobs
  ↓
Processing Queue
  ↓
System Health
  ↓
Analytics
  ↓
Audit Logs
  ↓
Configuration
```

---

# 6. Authentication Flow

## 6.1 Login

```text
Open AILPG
    ↓
Login Screen
    ↓
Enter Email / Username
    ↓
Enter Password
    ↓
Validate
    ↓
Authentication Successful?
   ├── No → Show Error
   │          ↓
   │       Retry / Forgot Password
   │
   └── Yes
        ↓
     Determine Role
        ↓
   Student / Teacher / Admin
        ↓
   Role Dashboard
```

---

# 7. Login UI

```text
┌─────────────────────────────────────┐
│              AILPG                   │
│                                     │
│        Welcome Back                 │
│                                     │
│ Email                               │
│ ┌─────────────────────────────────┐ │
│ │                                 │ │
│ └─────────────────────────────────┘ │
│                                     │
│ Password                            │
│ ┌─────────────────────────────────┐ │
│ │ •••••••••                       │ │
│ └─────────────────────────────────┘ │
│                                     │
│ [ Forgot Password? ]                │
│                                     │
│ [           Login             ]     │
│                                     │
│ Don't have an account? Sign Up      │
└─────────────────────────────────────┘
```

---

# 8. Authentication Error Flow

```text
Submit Login
    ↓
Authentication
    ↓
Failed
    ↓
Determine Error
```

Possible errors:

```text
Invalid credentials
Account not found
Account disabled
Email not verified
Server unavailable
Too many attempts
```

UI should provide a useful recovery action.

---

# 9. Password Reset Flow

```text
Login
  ↓
Forgot Password
  ↓
Enter Email
  ↓
Submit
  ↓
Reset Link / OTP
  ↓
Verify
  ↓
Create New Password
  ↓
Password Updated
  ↓
Login
```

---

# 10. Student Registration Flow

```text
Landing Page
    ↓
Sign Up
    ↓
Enter Name
    ↓
Enter Email
    ↓
Create Password
    ↓
Accept Terms
    ↓
Create Account
    ↓
Email Verification
    ↓
Account Activated
    ↓
Student Dashboard
```

---

# 11. Teacher Onboarding Flow

```text
Teacher Registration
        ↓
Profile Setup
        ↓
Organization / Institution
        ↓
Teaching Subject
        ↓
Preferred Language
        ↓
Dashboard
        ↓
Create First Course
```

Optional onboarding checklist:

```text
□ Complete Profile
□ Create Course
□ Upload First Video
□ Review AI Lesson
□ Publish First Lesson
```

---

# 12. Student Dashboard Flow

Dashboard:

```text
Login
  ↓
Student Dashboard
```

Primary sections:

```text
Continue Learning
My Courses
Recent Lessons
Progress
Recommended Content
Achievements / Completion
```

Example:

```text
┌────────────────────────────────────────┐
│ Good evening                           │
│                                        │
│ Continue Learning                      │
│ ┌────────────────────────────────────┐ │
│ │ Quadratic Equations                │ │
│ │ Lesson 4                           │ │
│ │ █████████████░░░ 72%               │ │
│ │ [ Continue ]                       │ │
│ └────────────────────────────────────┘ │
│                                        │
│ My Courses                             │
│                                        │
│ [Algebra] [Geometry] [Calculus]        │
└────────────────────────────────────────┘
```

---

# 13. Student Course Discovery

```text
Student Dashboard
      ↓
My Courses
      ↓
Course List
      ↓
Search / Filter
      ↓
Select Course
```

Filters may include:

* Subject
* Language
* Difficulty
* Progress
* Course status

---

# 14. Course Overview Flow

```text
Course
  ↓
Course Overview
```

Display:

```text
Course Title
Description
Teacher
Language
Progress
Modules
Lessons
Completion
```

Example:

```text
Algebra Fundamentals

Progress: 64%

Module 1
 ✓ Lesson 1
 ✓ Lesson 2
 ✓ Lesson 3

Module 2
 ✓ Lesson 4
 ● Lesson 5
 ○ Lesson 6
```

---

# 15. Lesson Start Flow

```text
Course
  ↓
Lesson
  ↓
Lesson Overview
  ↓
Start Lesson
```

Before starting, display:

* Lesson title
* Duration
* Number of checkpoints
* Difficulty
* Language
* Previous progress

---

# 16. Interactive Lesson Flow

Core student learning flow:

```text
Start Lesson
    ↓
Video Begins
    ↓
Student Watches
    ↓
Checkpoint Reached
    ↓
Video Pauses
    ↓
Question Appears
    ↓
Student Answers
    ↓
Answer Submitted
    ↓
Feedback
    ↓
Continue
    ↓
Video Resumes
```

---

# 17. Lesson Checkpoint

When the configured timestamp is reached:

```text
Video
  ↓
Checkpoint Trigger
  ↓
Pause Playback
  ↓
Show Question
```

Example:

```text
Video paused at 04:32

┌─────────────────────────────────┐
│ Checkpoint                      │
│                                 │
│ What is the next step?          │
│                                 │
│ ○ Add 2                         │
│ ○ Subtract 2                    │
│ ○ Multiply by 2                 │
│ ○ Divide by 2                   │
│                                 │
│ [ Submit Answer ]               │
└─────────────────────────────────┘
```

---

# 18. Question Answer Flow

```text
Question Displayed
      ↓
Student Selects Answer
      ↓
Submit
      ↓
Validate
      ↓
Correct?
 ┌────┴─────┐
Yes         No
 │           │
 ↓           ↓
Correct     Incorrect
Feedback    Feedback
 │           │
 └────┬──────┘
      ↓
Continue
```

---

# 19. Multiple Choice Flow

```text
Display Question
      ↓
Select Option
      ↓
Highlight Selection
      ↓
Submit
      ↓
Lock Answer
      ↓
Evaluate
      ↓
Display Result
```

---

# 20. Multiple Answer Flow

For questions with multiple correct answers:

```text
Question
  ↓
Select Multiple Options
  ↓
Submit
  ↓
Evaluate Combination
  ↓
Feedback
```

The UI must clearly indicate:

```text
Select all that apply.
```

---

# 21. Short Answer Flow

```text
Question
   ↓
Text Input
   ↓
Student Answer
   ↓
Submit
   ↓
Evaluate
```

Possible evaluation:

```text
Exact Match
Numeric Match
Normalized Text Match
AI-Assisted Evaluation
Teacher-defined Answer
```

---

# 22. Question Retry Flow

If retry is enabled:

```text
Incorrect
   ↓
Feedback
   ↓
Try Again?
 ├── Yes
 │    ↓
 │  Question Reset
 │    ↓
 │  Submit Again
 │
 └── No
      ↓
   Continue
```

---

# 23. Question Skip Flow

If the lesson allows skipping:

```text
Question
  ↓
Skip
  ↓
Confirmation / Immediate Skip
  ↓
Record Skipped
  ↓
Continue Video
```

Teacher configuration determines whether skip is allowed.

---

# 24. Lesson Completion

```text
Last Checkpoint
     ↓
Question Answered
     ↓
Video Complete
     ↓
Lesson Completed
     ↓
Calculate Result
     ↓
Display Completion Screen
```

---

# 25. Lesson Completion Screen

```text
┌──────────────────────────────────────┐
│                                      │
│          Lesson Complete!            │
│                                      │
│             85%                      │
│                                      │
│ Questions:  8 / 10 correct           │
│ Time:       14 min                   │
│                                      │
│ [ Continue Course ]                  │
│ [ Review Lesson ]                    │
└──────────────────────────────────────┘
```

---

# 26. Student Progress Flow

Student progress should update after meaningful events.

```text
Lesson Started
      ↓
Watch Progress
      ↓
Checkpoint Answers
      ↓
Lesson Completion
      ↓
Course Progress
```

Example:

```text
Lesson Progress
██████████████░░ 82%

Course Progress
████████░░░░░░░░ 51%
```

---

# 27. Teacher Dashboard Flow

```text
Login
  ↓
Teacher Dashboard
```

Primary dashboard:

```text
Courses
Lessons
Videos
AI Jobs
Questions
Students
Analytics
```

---

# 28. Create Course Flow

```text
Teacher Dashboard
      ↓
Create Course
      ↓
Course Details
      ↓
Save Draft
      ↓
Course Created
      ↓
Course Builder
```

Course information:

```text
Title
Description
Subject
Category
Language
Thumbnail
Visibility
```

---

# 29. Course Builder Flow

```text
Course
  ↓
Course Builder
  ↓
Create Module
  ↓
Create Lesson
  ↓
Upload Video
```

Structure:

```text
Course
 ├── Module 1
 │    ├── Lesson 1
 │    ├── Lesson 2
 │    └── Lesson 3
 │
 └── Module 2
      ├── Lesson 4
      └── Lesson 5
```

---

# 30. Create Lesson Flow

```text
Create Lesson
      ↓
Lesson Title
      ↓
Description
      ↓
Select Module
      ↓
Upload Video
      ↓
Save
```

---

# 31. MP4 Upload Flow

This is one of the most important AILPG flows.

```text
Teacher
   ↓
Create / Edit Lesson
   ↓
Upload Video
   ↓
Select MP4
   ↓
Validate File
   ↓
Upload
   ↓
Upload Complete
   ↓
Create Processing Job
   ↓
AI Pipeline
```

---

# 32. Upload UI

```text
┌────────────────────────────────────────┐
│ Upload Lesson Video                    │
│                                        │
│        ┌───────────────────┐           │
│        │       ↑           │           │
│        │ Drop MP4 here     │           │
│        │ or Browse         │           │
│        └───────────────────┘           │
│                                        │
│ Supported: MP4                         │
│ Maximum size: configurable             │
└────────────────────────────────────────┘
```

---

# 33. File Validation Flow

```text
Select File
    ↓
Validate Extension
    ↓
Validate MIME Type
    ↓
Validate File Size
    ↓
Validate File Integrity
```

If invalid:

```text
Invalid File
    ↓
Show Reason
    ↓
Select Another File
```

---

# 34. Upload Progress Flow

```text
Upload Started
      ↓
Uploading
      ↓
Progress Updates
      ↓
Upload Complete
```

Example:

```text
Uploading:

████████████████░░░░ 82%

1.64 GB / 2.00 GB
```

---

# 35. Upload Failure Flow

```text
Upload
  ↓
Failure
  ↓
Identify Cause
```

Possible causes:

```text
Network interruption
File too large
Storage failure
Authentication expired
Server error
```

Recovery:

```text
[Retry]
[Replace File]
[Cancel]
```

---

# 36. AI Configuration Flow

After upload:

```text
Video Uploaded
      ↓
AI Configuration
```

Possible settings:

```text
Source Language
Target Languages
Question Generation
Question Frequency
Difficulty
Checkpoint Density
Explanation Generation
Caption Generation
```

Example:

```text
Question Frequency

○ Low
○ Medium
● High
```

---

# 37. Start AI Processing

```text
Configuration Complete
      ↓
Review Settings
      ↓
Start Processing
      ↓
Confirmation
      ↓
AI Job Created
```

Confirmation:

```text
Start AI Processing?

The system will analyze the video,
generate checkpoints, and create
an interactive lesson.

[Cancel] [Start Processing]
```

---

# 38. AI Processing Flow

The complete pipeline:

```text
                    MP4
                     │
                     ↓
              Video Validation
                     │
                     ↓
               Audio Extraction
                     │
                     ↓
              Speech Transcription
                     │
                     ↓
              Content Segmentation
                     │
                     ↓
              Mathematical Analysis
                     │
                     ↓
              Concept Detection
                     │
                     ↓
            Question Generation
                     │
                     ↓
             Checkpoint Placement
                     │
                     ↓
                Translation
                     │
                     ↓
          Interactive HTML Generation
                     │
                     ↓
                 Validation
                     │
                     ↓
               Teacher Review
```

---

# 39. AI Processing Status Flow

```text
Queued
  ↓
Processing
  ↓
Transcript
  ↓
Analysis
  ↓
Question Generation
  ↓
Translation
  ↓
Lesson Generation
  ↓
Validation
  ↓
Review Required
```

---

# 40. AI Processing UI

```text
┌──────────────────────────────────────────┐
│ AI Lesson Generation                    │
│                                          │
│ ✓ Video uploaded                         │
│ ✓ Audio extracted                        │
│ ✓ Transcript generated                   │
│ ● Analyzing mathematical content         │
│ ○ Generating questions                   │
│ ○ Translating                            │
│ ○ Building interactive lesson            │
│                                          │
│ Current step: Content Analysis           │
└──────────────────────────────────────────┘
```

---

# 41. AI Failure Flow

```text
AI Processing
      ↓
Failure
      ↓
Determine Stage
      ↓
Display Error
      ↓
Retry
```

Example:

```text
Question Generation Failed

The transcript was generated successfully,
but questions could not be generated.

[Retry Question Generation]
[Edit Manually]
```

Previously completed stages should not unnecessarily restart.

---

# 42. AI Review Flow

After processing:

```text
AI Processing Complete
      ↓
Teacher Review
```

Review sections:

```text
Video
Transcript
Concepts
Questions
Translations
Checkpoints
Lesson Preview
```

---

# 43. Transcript Review Flow

```text
Open Transcript
      ↓
Review Segments
      ↓
Edit Errors
      ↓
Save
      ↓
Mark Reviewed
```

Example:

```text
00:00 – 00:12
AI generated transcript...

[Edit]
```

---

# 44. Question Review Flow

```text
AI Generated Questions
      ↓
Question List
      ↓
Open Question
      ↓
Review
      ↓
Approve / Edit / Regenerate / Delete
```

Actions:

```text
[Approve]
[Edit]
[Regenerate]
[Delete]
```

---

# 45. Question Editing Flow

```text
Open Question
    ↓
Edit Question
    ↓
Edit Options
    ↓
Select Correct Answer
    ↓
Edit Explanation
    ↓
Set Difficulty
    ↓
Set Timestamp
    ↓
Save
```

---

# 46. Question Regeneration Flow

```text
Question
  ↓
Regenerate
  ↓
Choose Reason
```

Optional reasons:

```text
Too easy
Too difficult
Incorrect
Unclear
Duplicate
Poor wording
Wrong concept
```

Then:

```text
AI Regenerates
     ↓
New Version
     ↓
Teacher Review
```

---

# 47. Translation Review Flow

```text
Source Content
      ↓
AI Translation
      ↓
Teacher Review
      ↓
Edit Translation
      ↓
Approve
```

Example:

```text
English
The value of x is 5.

Tamil
x-ன் மதிப்பு 5.

[Edit] [Approve]
```

---

# 48. Lesson Timeline Flow

The teacher should see generated checkpoints along the video timeline.

```text
00:00 ───●───────●──────────●────── 15:40
         Q1      Q2         Q3
```

Selecting a checkpoint:

```text
Timeline Marker
      ↓
Question Editor
      ↓
Edit Timestamp / Question
      ↓
Save
```

---

# 49. Preview Flow

Before publishing:

```text
Teacher Review
      ↓
Preview Lesson
      ↓
Student Simulation
      ↓
Watch Video
      ↓
Answer Questions
      ↓
Verify Feedback
      ↓
Exit Preview
```

The preview should behave as closely as possible to the student experience.

---

# 50. Publish Flow

```text
Lesson Review
      ↓
Preview
      ↓
Publish
      ↓
Validation
      ↓
All Required Content Ready?
 ┌────┴─────┐
No         Yes
 │           │
 ↓           ↓
Show        Publish
Issues       │
             ↓
        Student Access
```

---

# 51. Publish Validation

Before publishing, validate:

```text
□ Video available
□ Transcript available where required
□ At least one valid lesson section
□ Questions valid
□ Correct answers configured
□ No broken checkpoints
□ Required translations available
□ Lesson metadata complete
□ Thumbnail available where required
```

---

# 52. Publish Confirmation

```text
┌──────────────────────────────────────┐
│ Publish Lesson?                      │
│                                      │
│ Students will be able to access     │
│ this lesson after publishing.       │
│                                      │
│ [Cancel]            [Publish]        │
└──────────────────────────────────────┘
```

---

# 53. Published Flow

```text
Published
   ↓
Lesson Available
   ↓
Student Enrollment / Access
   ↓
Student Learning
```

---

# 54. Unpublish Flow

```text
Published Lesson
      ↓
Unpublish
      ↓
Confirmation
      ↓
Lesson Hidden From Students
      ↓
Existing Analytics Preserved
```

Unpublishing should not delete historical student records.

---

# 55. Course Publishing Flow

```text
Lesson Published
      ↓
All Required Lessons Ready?
      ↓
Course Validation
      ↓
Publish Course
      ↓
Students Can Access Course
```

---

# 56. Analytics Flow

Teacher:

```text
Dashboard
   ↓
Analytics
   ↓
Select Course
   ↓
Select Lesson
   ↓
View Metrics
```

Metrics:

```text
Students
Views
Completion
Average Score
Question Accuracy
Drop-off Points
Average Watch Time
```

---

# 57. Student Analytics Flow

Student:

```text
Profile
  ↓
Progress
  ↓
Course Progress
  ↓
Lesson Performance
  ↓
Question Performance
```

---

# 58. Subscription / Quality Flow

If AILPG uses subscription-based video quality:

```text
Student Opens Lesson
      ↓
Check Subscription
      ↓
Determine Available Quality
      ↓
Video Player
```

Example:

```text
Free:
Standard quality

Premium:
Higher quality options
```

The UI should clearly show restricted options.

```text
Quality

✓ 480p
🔒 720p Premium
🔒 1080p Premium
```

---

# 59. Language Selection Flow

```text
Open Lesson
     ↓
Language
     ↓
Select Language
     ↓
Check Translation Availability
```

If available:

```text
Load Translation
```

If unavailable:

```text
Translation unavailable
Use original language
```

---

# 60. Translation Processing Flow

If translation is still processing:

```text
Select Language
      ↓
Translation unavailable
      ↓
Show Processing
      ↓
Continue in Source Language
```

The student should not be blocked unnecessarily.

---

# 61. Video Quality Switching

```text
Video Playing
     ↓
Open Quality
     ↓
Select Quality
     ↓
Check Permission
     ↓
Switch Stream
```

If not permitted:

```text
Premium quality

[Upgrade]
```

---

# 62. Network Recovery Flow

During video playback:

```text
Network Interrupted
       ↓
Detect Failure
       ↓
Buffer / Retry
       ↓
Connection Restored?
   ┌────┴─────┐
  Yes         No
   │           │
   ↓           ↓
Resume       Retry UI
```

---

# 63. Session Expiration Flow

```text
User Action
   ↓
Session Expired
   ↓
Save Safe State
   ↓
Login Required
   ↓
Re-authenticate
   ↓
Return to Previous Screen
```

Where possible, preserve the user's editing or learning position.

---

# 64. Teacher Draft Flow

Teacher changes should follow:

```text
Edit
 ↓
Auto Save
 ↓
Draft
 ↓
Review
 ↓
Publish
```

A draft should not automatically become visible to students.

---

# 65. Versioning Flow

Lesson versions:

```text
Version 1
   ↓
Edit
   ↓
Version 2
   ↓
Edit
   ↓
Version 3
```

Recommended states:

```text
Draft
Published
Archived
```

Published versions should remain recoverable according to the platform's retention policy.

---

# 66. Delete Flow

For important content:

```text
Delete
  ↓
Confirmation
  ↓
Permission Check
  ↓
Soft Delete / Archive
  ↓
Audit Log
```

Example:

```text
Delete Course?

This course will be archived and removed
from normal student access.

[Cancel] [Archive]
```

---

# 67. Admin User Management Flow

```text
Admin Dashboard
      ↓
Users
      ↓
Search User
      ↓
Open User
      ↓
View Account
      ↓
Actions
```

Possible actions:

```text
View
Suspend
Reactivate
Change Role
Reset Access
View Activity
```

All sensitive administrative actions should be audited.

---

# 68. Admin AI Job Flow

```text
Admin
  ↓
AI Jobs
  ↓
Search / Filter
  ↓
Open Job
  ↓
View Pipeline
  ↓
View Logs / Status
  ↓
Retry / Cancel
```

---

# 69. Processing Queue Flow

```text
Video Upload
     ↓
Job Created
     ↓
Queue
     ↓
Worker
     ↓
Processing
     ↓
Complete / Failed
```

Admin view:

```text
Queued:      18
Processing:   6
Completed:  421
Failed:       4
```

---

# 70. Error Recovery Matrix

| Flow        | Failure             | Recovery          |
| ----------- | ------------------- | ----------------- |
| Login       | Invalid credentials | Retry             |
| Upload      | Network error       | Resume/retry      |
| Upload      | Invalid file        | Replace           |
| Processing  | Worker failure      | Retry job         |
| AI          | Generation failure  | Retry stage       |
| Translation | Provider failure    | Retry translation |
| Preview     | Asset unavailable   | Regenerate        |
| Publish     | Validation failure  | Fix issues        |
| Playback    | Network error       | Retry/buffer      |
| Question    | Submission failure  | Retry submission  |

---

# 71. Global Navigation Rule

Users should be able to understand:

```text
Where am I?
What can I do?
How do I go back?
What is the next action?
```

Every major page should provide:

```text
Page Title
Breadcrumb / Context
Primary Action
Secondary Actions
Navigation
```

---

# 72. Unsaved State Flow

```text
User Editing
     ↓
Changes Detected
     ↓
Auto Save
     ↓
Saved
```

If auto-save fails:

```text
Save Failed
     ↓
Retry
     ↓
Keep Local Editing State
```

Before navigation:

```text
Unsaved Changes?
   ├── No → Navigate
   └── Yes
        ↓
   Confirm Action
```

---

# 73. Notification Flow

System events may generate notifications.

Examples:

```text
Video Processing Complete
AI Processing Failed
Translation Complete
Lesson Published
Course Updated
Student Activity
```

Notification flow:

```text
Event
 ↓
Notification Service
 ↓
User Notification
 ↓
Open Notification
 ↓
Relevant Screen
```

---

# 74. Global Search Flow

```text
Search
  ↓
Enter Query
  ↓
Search
  ↓
Results
```

Results may include:

```text
Courses
Lessons
Videos
Questions
Students
```

Search results must respect permissions.

---

# 75. Mobile Student Flow

Simplified:

```text
Login
 ↓
Home
 ↓
Course
 ↓
Lesson
 ↓
Video
 ↓
Question
 ↓
Answer
 ↓
Feedback
 ↓
Continue
 ↓
Complete
```

Bottom navigation:

```text
Home | Courses | Progress | Profile
```

---

# 76. Mobile Teacher Flow

```text
Dashboard
 ↓
Courses
 ↓
Course
 ↓
Lesson
 ↓
Video
 ↓
AI Processing
 ↓
Review
 ↓
Publish
```

Advanced editing interfaces may recommend desktop/tablet for better usability.

---

# 77. End-to-End AILPG Flow

The complete platform flow:

```text
                         AILPG
                           │
                           ↓
                      Teacher Login
                           │
                           ↓
                     Create Course
                           │
                           ↓
                    Create Lesson
                           │
                           ↓
                       Upload MP4
                           │
                           ↓
                   Configure AI
                           │
                           ↓
                    Start Processing
                           │
                           ↓
                 ┌───────────────────┐
                 │   AI PIPELINE     │
                 │                   │
                 │ Transcript        │
                 │ Analysis          │
                 │ Concepts          │
                 │ Questions         │
                 │ Checkpoints       │
                 │ Translation       │
                 │ HTML Generation   │
                 └─────────┬─────────┘
                           │
                           ↓
                     Teacher Review
                           │
                 ┌─────────┼─────────┐
                 │         │         │
             Transcript Questions Translation
                 │         │         │
                 └─────────┼─────────┘
                           │
                           ↓
                         Preview
                           │
                           ↓
                        Publish
                           │
                           ↓
                     Student Access
                           │
                           ↓
                      Watch Video
                           │
                           ↓
                    Reach Checkpoint
                           │
                           ↓
                     Answer Question
                           │
                           ↓
                       Feedback
                           │
                           ↓
                    Continue Lesson
                           │
                           ↓
                      Completion
                           │
                           ↓
                       Analytics
```

---

# 78. Primary User Journey

The most important AILPG user journey is:

```text
MP4
 ↓
AI
 ↓
Interactive Lesson
 ↓
Teacher Approval
 ↓
Student Learning
```

This should remain the central product flow.

Every additional feature should support this journey rather than obscure it.

---

# 79. User Flow Success Criteria

A user flow is considered successful when:

* [ ] User understands where they are
* [ ] User knows the next action
* [ ] Important system states are visible
* [ ] Loading is handled
* [ ] Errors are recoverable
* [ ] Data is not unexpectedly lost
* [ ] Permissions are respected
* [ ] Mobile behavior is defined
* [ ] Accessibility is supported
* [ ] Analytics events can be captured

---

# 80. User Flow Definition of Done

The AILPG user-flow layer is complete when:

* [ ] Authentication flow defined
* [ ] Student registration defined
* [ ] Teacher onboarding defined
* [ ] Student learning flow defined
* [ ] Teacher course creation defined
* [ ] MP4 upload flow defined
* [ ] AI configuration defined
* [ ] AI processing defined
* [ ] AI failure/retry defined
* [ ] AI review defined
* [ ] Question editing defined
* [ ] Translation flow defined
* [ ] Lesson preview defined
* [ ] Publishing defined
* [ ] Student checkpoint interaction defined
* [ ] Lesson completion defined
* [ ] Analytics flow defined
* [ ] Subscription quality flow defined
* [ ] Language flow defined
* [ ] Admin flow defined
* [ ] Error recovery defined
* [ ] Mobile flows defined
* [ ] Session recovery defined
* [ ] Draft/version flow defined

---

# 81. Next Document

The next UI/UX Blueprint document is:

```text
04_Student_App.md
```

It will define the complete student-facing application, including:

* Student dashboard
* Home screen
* Course discovery
* Course details
* Lesson screen
* Interactive video player
* Question experience
* Progress tracking
* Results
* Profile
* Notifications
* Language selection
* Video quality
* Mobile navigation
* Responsive behavior

---

# 82. Git Commit

```bash
git add docs/04_UI_UX_Blueprint/03_User_Flow.md
git commit -m "docs(ui-ux): add user flows"
```
