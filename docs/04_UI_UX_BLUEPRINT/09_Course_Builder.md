# AILPG — Course Builder UI/UX Blueprint

**Document:** `docs/04_UI_UX_BLUEPRINT/09_Course_Builder.md`
**Project:** MP4 → Interactive Learning Platform Generator (AILPG)
**Document Type:** UI/UX Blueprint
**Version:** 1.0
**Status:** Draft / Implementation Reference

---

# 1. Purpose

The AILPG Course Builder is the content-authoring workspace used to organize AI-generated lessons, videos, questions, translations, and learning activities into publishable courses.

It connects the complete content workflow:

```text
Video Upload
     ↓
AI Processing
     ↓
Generated Lesson
     ↓
Interactive Questions
     ↓
Translation
     ↓
Course Structure
     ↓
Review
     ↓
Publish
```

The Course Builder must support both:

* **AI-first content creation**
* **Human-controlled course authoring**

---

# 2. Course Builder Goals

The Course Builder should allow authorized users to:

* Create courses.
* Edit course information.
* Create modules.
* Create lessons.
* Add videos.
* Import AI-generated lessons.
* Add interactive questions.
* Reorder content.
* Configure lesson settings.
* Configure languages.
* Configure access rules.
* Preview the student experience.
* Validate course content.
* Submit for review.
* Publish courses.
* Archive courses.
* Manage course versions.

---

# 3. Course Hierarchy

AILPG uses the following content hierarchy:

```text
Course
│
├── Module
│   │
│   ├── Lesson
│   │   ├── Video
│   │   ├── Transcript
│   │   ├── Questions
│   │   └── Activities
│   │
│   └── Lesson
│
├── Module
│
└── Module
```

Example:

```text
Algebra Fundamentals
│
├── Module 1 — Basic Equations
│   ├── Lesson 1 — Introduction
│   ├── Lesson 2 — One-Step Equations
│   └── Lesson 3 — Two-Step Equations
│
├── Module 2 — Linear Equations
│   ├── Lesson 4 — Variables
│   └── Lesson 5 — Applications
│
└── Module 3 — Practice
    ├── Lesson 6 — Mixed Problems
    └── Lesson 7 — Final Assessment
```

---

# 4. Course Builder Layout

Desktop:

```text
┌──────────────────────────────────────────────────────────────┐
│ AILPG | Course Builder                 Preview | Save | Publish│
├────────────────┬───────────────────────────┬─────────────────┤
│                │                           │                 │
│ COURSE TREE    │       EDITOR              │   PROPERTIES    │
│                │                           │                 │
│ Course         │                           │ Title           │
│ ├ Module 1     │    Selected Content       │ Description     │
│ │ ├ Lesson 1   │                           │ Visibility      │
│ │ └ Lesson 2   │                           │ Language        │
│ └ Module 2     │                           │ Access          │
│                │                           │                 │
└────────────────┴───────────────────────────┴─────────────────┘
```

---

# 5. Three-Panel Architecture

The desktop builder should use three primary areas.

## Left Panel

Content tree:

```text
Course
Modules
Lessons
Activities
```

## Center Panel

Editing workspace:

```text
Course Editor
Lesson Editor
Video Editor
Question Editor
```

## Right Panel

Properties:

```text
Settings
Metadata
Visibility
Access
Language
Publishing
```

---

# 6. Top Toolbar

Example:

```text
← Courses

Algebra Fundamentals

[Undo] [Redo]

[Preview]

[Save Draft]

[Validate]

[Submit Review]

[Publish]
```

Buttons should change according to permissions and course state.

---

# 7. Course Creation

Click:

```text
[Create Course]
```

opens:

```text
Create New Course

Course Title
[____________________________]

Description
[____________________________]

Category
[Select]

Primary Language
[Select]

Instructor
[Select]

Visibility
○ Private
○ Unlisted
○ Public

[Create Course]
```

---

# 8. Course Metadata

Course information includes:

```text
Title
Subtitle
Description
Thumbnail
Category
Subject
Level
Language
Instructor
Estimated Duration
Learning Objectives
Tags
Prerequisites
```

---

# 9. Course Thumbnail

The course should support:

```text
Upload Image
Replace Image
Remove Image
Preview
```

Recommended validation:

```text
Supported:
JPG
PNG
WEBP

Maximum File Size:
Configurable
```

---

# 10. Course Tree

The left navigation displays the course structure.

Example:

```text
📘 Algebra Fundamentals

▼ Module 1
   ▼ Lesson 1
      🎥 Video
      ❓ Question 1
      ❓ Question 2

   ▶ Lesson 2

▶ Module 2
```

Each item should provide contextual actions.

---

# 11. Context Menu

For a module:

```text
Add Lesson
Rename
Duplicate
Move
Delete
```

For a lesson:

```text
Edit
Preview
Duplicate
Move
Delete
```

For a question:

```text
Edit
Preview
Duplicate
Delete
```

---

# 12. Drag and Drop

Content should be reorderable.

Example:

```text
Module 1
│
├── Lesson 1
├── Lesson 2
├── Lesson 3
└── Lesson 4
```

Dragging:

```text
Lesson 4
   ↓
Lesson 2 position
```

produces:

```text
Lesson 1
Lesson 4
Lesson 2
Lesson 3
```

The new order must be persisted.

---

# 13. Keyboard Reordering

Drag-and-drop must not be the only mechanism.

Accessible controls:

```text
Move Up
Move Down
Move Into
Move Out
```

This ensures keyboard accessibility.

---

# 14. Module Editor

Module fields:

```text
Module Title
Description
Learning Objectives
Estimated Duration
Visibility
```

Example:

```text
MODULE 1

Basic Equations

Learn how to solve simple equations
using algebraic transformations.

Duration:
45 minutes
```

---

# 15. Lesson Editor

Lesson fields:

```text
Lesson Title
Description
Learning Objectives
Video
Questions
Resources
Language
Completion Rules
```

Example:

```text
LESSON 2

Solving Two-Step Equations

Video:
two_step_equation.mp4

Questions:
5

Duration:
08:42
```

---

# 16. AI-Generated Lesson Import

A major AILPG feature is importing generated lessons directly into a course.

Flow:

```text
Course Builder
      ↓
Add Lesson
      ↓
Import AI Lesson
      ↓
Select Generated Lesson
      ↓
Preview
      ↓
Import
```

Example:

```text
Available AI Lessons

✓ Linear Equation — 08:42
✓ Quadratic Equation — 11:20
✓ Fractions — 06:18
```

---

# 17. AI Lesson Import Preview

Before importing:

```text
AI GENERATED LESSON

Title:
Linear Equations

Video:
08:42

Questions:
5

Transcript:
Available

Translations:
English
Tamil

AI Review:
Approved

[Cancel] [Import]
```

---

# 18. Video Attachment

A lesson may contain one or more video assets depending on configuration.

Options:

```text
Upload Video
Select Existing Video
Import AI Lesson
Replace Video
Remove Video
```

---

# 19. Lesson Video Configuration

Properties:

```text
Video
Duration
Default Quality
Available Qualities
Captions
Transcript
Playback Speed
Seeking Rules
```

Example:

```text
Video Quality

360p ✓
480p ✓
720p ✓
1080p ✓
```

Access restrictions can be associated with subscription rules.

---

# 20. Interactive Question Management

Within a lesson:

```text
Interactive Questions

Q1 — 01:25
Q2 — 03:42
Q3 — 05:10
Q4 — 07:21
```

Actions:

```text
Add Question
Edit
Duplicate
Move Timestamp
Preview
Delete
```

---

# 21. Add Question

Button:

```text
[+ Add Question]
```

Options:

```text
Multiple Choice
Multiple Answer
True / False
Short Answer
Numerical
Equation
Fill in the Blank
Matching
Ordering
```

---

# 22. Question Placement

Question placement can be configured through:

```text
Timestamp:
[04:32]

Trigger:
At timestamp

Required:
Yes

Pause Video:
Yes
```

Advanced:

```text
Trigger Type:
Timestamp
Chapter
Concept
AI Marker
```

---

# 23. Lesson Completion Rules

Course creators can define completion conditions.

Example:

```text
Lesson Completion

○ Watch video
○ Watch 80% of video
○ Complete all required questions
○ Achieve minimum score
○ Complete video + required questions
```

Multiple conditions may be combined.

---

# 24. Course Completion Rules

Example:

```text
Course Completion

Required Modules:
All

Required Lessons:
All

Minimum Score:
70%

Final Assessment:
Required
```

---

# 25. Course Access

Possible access modes:

```text
Free
Subscription
Purchase
Institution
Private
Invitation Only
```

Access rules should be controlled centrally.

---

# 26. Course Visibility

States:

```text
Draft
Private
Unlisted
Published
Archived
```

Publishing should not happen accidentally.

---

# 27. Save System

The builder should support:

```text
Save Draft
Autosave
Manual Save
Version Save
```

Example:

```text
Saved 10 seconds ago
```

If there are unsaved changes:

```text
Unsaved changes
```

---

# 28. Autosave

Autosave should:

* Save non-destructive edits automatically.
* Avoid excessive API requests.
* Show save status.
* Recover drafts after accidental navigation.

Example:

```text
Saving...
↓
Saved
```

---

# 29. Conflict Handling

If the same content is edited simultaneously:

```text
This course was changed by another user.

Your version:
Last saved 10:41 PM

Server version:
Last saved 10:43 PM

[Review Changes]
[Keep Mine]
[Use Server Version]
```

Critical conflicts should not be silently overwritten.

---

# 30. Undo / Redo

The builder should support:

```text
Undo
Redo
```

for current editing sessions.

Undo should not necessarily reverse already-published content without creating a new version.

---

# 31. Preview Mode

Button:

```text
[Preview]
```

opens a student-style preview.

Preview should show:

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
Feedback
```

Preview should not create production learning analytics unless explicitly configured as a test session.

---

# 32. Course Preview

Example:

```text
ALGEBRA FUNDAMENTALS

Module 1
Basic Equations

Lesson 1
Introduction

[▶ Start Lesson]
```

---

# 33. Lesson Preview

```text
Lesson 2

Solving Two-Step Equations

▶ Video

At 03:42:
Interactive Question

"What is x?"

○ 2
○ 3
○ 4

[Submit]
```

---

# 34. Validation System

Before publishing, the builder should run validation.

Example:

```text
Course Validation

✓ Course title
✓ Description
✓ Thumbnail
✓ Modules
✓ Lessons
✓ Videos
✓ Questions
✓ Translations

⚠ Lesson 4 has no learning objective

✕ Lesson 6 contains an invalid question timestamp
```

---

# 35. Validation Categories

```text
Content
Media
Questions
Translations
Access
Metadata
Accessibility
Publishing
Security
```

---

# 36. Validation Severity

Use:

```text
Error
Warning
Info
```

Example:

```text
ERROR
Lesson has no video.

WARNING
Lesson has no translated version.

INFO
Course description contains fewer than
the recommended number of characters.
```

---

# 37. Publishing Workflow

```text
Draft
 ↓
Validate
 ↓
Submit Review
 ↓
Content Review
 ↓
AI Review
 ↓
Approved
 ↓
Publish
```

If the platform configuration allows direct publishing for authorized users:

```text
Draft
 ↓
Validate
 ↓
Publish
```

---

# 38. Publish Confirmation

Example:

```text
Publish Course?

Algebra Fundamentals

7 Lessons
3 Modules
34 Questions

Once published, students with access
will be able to view this course.

[Cancel] [Publish Course]
```

---

# 39. Unpublish

Authorized users may be able to unpublish a course.

Confirmation:

```text
Unpublish Course?

Students may lose access to this course
depending on your access configuration.

[Cancel] [Unpublish]
```

The system should preserve course history.

---

# 40. Version Management

Course versions:

```text
Version 1
Initial Draft

Version 2
Added Module 2

Version 3
Updated Questions

Version 4
Published
```

Actions:

```text
View
Compare
Restore
Create New Version
```

---

# 41. Version Comparison

Example:

```text
COURSE VERSION COMPARISON

v3                         v4

Lesson 2                   Lesson 2
5 Questions                6 Questions

Question 4                 Question 4
Original Text              Updated Text
```

Differences should be visually clear and accessible.

---

# 42. Translation Management

Course Builder should show language availability.

```text
Course Languages

English ✓
Tamil ✓
Hindi ✓
Malayalam ○
Telugu ○
```

Status:

```text
Complete
Partial
Pending
Needs Review
Unavailable
```

---

# 43. Course Localization

Localization may include:

```text
Course Title
Description
Learning Objectives
Module Names
Lesson Names
Questions
Answer Options
Feedback
Captions
Transcript
```

---

# 44. Learning Objectives

Each course and lesson should support learning objectives.

Example:

```text
Learning Objectives

After completing this lesson,
students should be able to:

✓ Identify variables
✓ Solve one-step equations
✓ Verify solutions
```

Objectives can be displayed to students.

---

# 45. Prerequisites

Course creators can specify prerequisites.

Example:

```text
Prerequisites

☑ Basic Arithmetic
☑ Fractions
☐ Algebra Introduction
```

The actual enforcement policy should be configurable.

---

# 46. Course Tags

Example:

```text
Algebra
Mathematics
Beginner
Equations
Grade 8
```

Tags support search and discovery.

---

# 47. Instructor Assignment

Authorized users can assign instructors.

```text
Instructor

[John Doe ▼]
```

Multiple instructors may be supported.

Permissions determine what assigned instructors can modify.

---

# 48. Course Settings Panel

Right-side settings:

```text
COURSE SETTINGS

General
Visibility
Access
Language
Completion
Certificate
Discussion
Analytics
Notifications
```

---

# 49. Certificate Configuration

If certificates are supported:

```text
Certificate

Enable Certificate
[✓]

Minimum Completion:
100%

Minimum Score:
70%

Certificate Title:
Algebra Fundamentals
```

---

# 50. Student Progress Configuration

Possible settings:

```text
Track Video Progress
Track Question Attempts
Track Lesson Completion
Track Course Completion
Allow Resume
```

---

# 51. Analytics Configuration

Course-level analytics may track:

```text
Enrollment
Lesson Starts
Lesson Completion
Video Watch Time
Question Accuracy
Drop-off Points
Language Usage
```

---

# 52. Course Search

Large content libraries need:

```text
Search courses...
```

Filters:

```text
Category
Instructor
Language
Status
Access Type
Created Date
Updated Date
```

---

# 53. Course Builder Empty State

When a new course has no modules:

```text
Your course is empty.

Start by creating your first module.

[+ Add Module]

or

[Import AI Lesson]
```

---

# 54. Course Builder Error State

Example:

```text
Unable to save course.

Your latest changes have not been
saved to the server.

[Retry Save]
```

Unsaved local state should be preserved when technically possible.

---

# 55. Responsive Design

Desktop:

```text
Three-panel builder
```

Tablet:

```text
Two-panel builder
```

Mobile:

```text
Tree
 ↓
Editor
 ↓
Properties
```

Panels may become drawers or full-screen views.

---

# 56. Mobile Course Builder

Example:

```text
┌───────────────────────────┐
│ ← Course       Save       │
├───────────────────────────┤
│                           │
│ Module 1                  │
│                           │
│ ▼ Lesson 1               │
│   🎥 Video                │
│   ❓ Question             │
│                           │
│ [+ Add Content]           │
│                           │
└───────────────────────────┘
```

---

# 57. Accessibility

The Course Builder must support:

* Keyboard navigation.
* Screen readers.
* Accessible tree navigation.
* Keyboard alternatives to drag/drop.
* Visible focus.
* Accessible dialogs.
* Accessible form validation.
* Accessible error messages.
* Reduced motion.
* Text resizing.

---

# 58. Drag-and-Drop Accessibility

Every drag/drop operation must have equivalent controls.

Example:

```text
Lesson 2

[Move Up]
[Move Down]
[Move Into Module]
[Move Out]
```

---

# 59. Permissions

Course Builder permissions:

| Action          | Super Admin | Admin | Content Manager | Instructor |
| --------------- | ----------: | ----: | --------------: | ---------: |
| Create Course   |           ✓ |     ✓ |               ✓ |   Optional |
| Edit Course     |           ✓ |     ✓ |               ✓ |   Assigned |
| Add Module      |           ✓ |     ✓ |               ✓ |   Assigned |
| Add Lesson      |           ✓ |     ✓ |               ✓ |   Assigned |
| Add Question    |           ✓ |     ✓ |               ✓ |   Assigned |
| Edit AI Lesson  |           ✓ |     ✓ |               ✓ |   Assigned |
| Publish         |           ✓ |     ✓ |        Optional |   Optional |
| Delete Course   |           ✓ |     ✓ |         Limited |          - |
| Manage Access   |           ✓ |     ✓ |         Limited |          - |
| Version Control |           ✓ |     ✓ |               ✓ |   Assigned |

Backend authorization must enforce these permissions.

---

# 60. API Integration

Potential APIs:

```text
GET    /api/courses
POST   /api/courses
GET    /api/courses/:courseId
PATCH  /api/courses/:courseId
DELETE /api/courses/:courseId

POST   /api/courses/:courseId/modules
PATCH  /api/modules/:moduleId
DELETE /api/modules/:moduleId

POST   /api/modules/:moduleId/lessons
PATCH  /api/lessons/:lessonId
DELETE /api/lessons/:lessonId

POST   /api/lessons/:lessonId/questions
PATCH  /api/questions/:questionId

POST   /api/courses/:courseId/validate
POST   /api/courses/:courseId/submit-review
POST   /api/courses/:courseId/publish
POST   /api/courses/:courseId/unpublish
```

Exact API contracts are defined separately.

---

# 61. Course Data Model

Conceptually:

```text
Course
│
├── id
├── title
├── description
├── thumbnail
├── category
├── language
├── status
├── access_type
├── instructor_id
├── version
│
└── Modules
      │
      └── Lessons
            │
            ├── Video
            ├── Transcript
            ├── Questions
            ├── Activities
            └── Translations
```

---

# 62. Course Builder State

Frontend state should distinguish:

```text
Course State
Module State
Lesson State
Question State
UI State
Save State
Validation State
Permission State
Preview State
```

Example:

```text
saveState:
idle
saving
saved
error
```

---

# 63. Autosave Architecture

```text
User Changes Content
        ↓
Debounce
        ↓
Local State Updated
        ↓
Autosave Request
        ↓
Backend
        ↓
Saved Version
        ↓
UI: Saved
```

Autosave should not create excessive versions.

---

# 64. Draft Recovery

If the browser closes unexpectedly:

```text
Unsaved draft found.

Last local update:
10:42 PM

[Recover Draft]
[Discard]
```

Recovery behavior should be predictable.

---

# 65. AI Integration

The Course Builder receives AI-generated content from:

```text
Video Analysis
OCR
Speech Recognition
Topic Detection
Question Generation
Translation
Lesson Generation
```

Example:

```text
MP4
 ↓
AI Pipeline
 ↓
Generated Lesson
 ↓
Course Builder
```

---

# 66. AI Content Identification

The builder should clearly identify generated content.

Example:

```text
AI Generated
AI Reviewed
Human Edited
Approved
```

This metadata helps content managers understand the origin and review state.

---

# 67. Human Editing

Administrators should be able to edit AI-generated:

```text
Title
Description
Transcript
Questions
Answers
Explanations
Translations
Timestamps
Learning Objectives
```

Human edits should be versioned.

---

# 68. Course Content Validation Flow

```text
Course Builder
      ↓
Save
      ↓
Validate
      ↓
┌───────────────┐
│ Errors?       │
└───────┬───────┘
        │
   Yes  │  No
        ↓
 Fix    Continue
        ↓
 Submit Review
```

---

# 69. Publish Readiness Checklist

Before publishing:

```text
CONTENT

[ ] Course title
[ ] Description
[ ] Thumbnail
[ ] At least one module
[ ] At least one lesson

MEDIA

[ ] Videos available
[ ] Video processing complete
[ ] Captions checked

QUESTIONS

[ ] Questions valid
[ ] Timestamps valid
[ ] Correct answers configured

TRANSLATION

[ ] Required languages complete

ACCESS

[ ] Access rules configured

QUALITY

[ ] Required video qualities available

REVIEW

[ ] Content reviewed
[ ] AI-generated content reviewed
```

---

# 70. Course Builder Navigation Flow

```text
Courses
   ↓
Create / Select Course
   ↓
Course Builder
   ├── Course Settings
   ├── Modules
   │    └── Lessons
   │         ├── Video
   │         ├── Questions
   │         └── Activities
   │
   ├── Translations
   ├── Access
   └── Analytics
        ↓
      Validate
        ↓
      Review
        ↓
      Publish
```

---

# 71. Complete AILPG Content Workflow

```text
                  MP4 UPLOAD
                      ↓
               VIDEO PROCESSING
                      ↓
                 AI ANALYSIS
                      ↓
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
     Transcript      OCR       Topic Detection
        │             │             │
        └─────────────┼─────────────┘
                      ↓
             QUESTION GENERATION
                      ↓
              LESSON GENERATION
                      ↓
                TRANSLATION
                      ↓
                  AI REVIEW
                      ↓
                HUMAN REVIEW
                      ↓
               COURSE BUILDER
                      ↓
                  VALIDATION
                      ↓
                  PUBLISHING
                      ↓
               STUDENT PLAYER
```

---

# 72. Component Architecture

```text
CourseBuilder
│
├── BuilderHeader
│   ├── SaveButton
│   ├── PreviewButton
│   ├── ValidateButton
│   └── PublishButton
│
├── CourseTree
│   ├── CourseNode
│   ├── ModuleNode
│   ├── LessonNode
│   └── QuestionNode
│
├── EditorPanel
│   ├── CourseEditor
│   ├── ModuleEditor
│   ├── LessonEditor
│   ├── VideoEditor
│   └── QuestionEditor
│
├── PropertiesPanel
│   ├── GeneralSettings
│   ├── AccessSettings
│   ├── LanguageSettings
│   └── CompletionSettings
│
├── ValidationPanel
└── PreviewPanel
```

---

# 73. Performance Requirements

The Course Builder should:

* Lazy-load large course trees.
* Save changes efficiently.
* Avoid re-rendering unrelated content.
* Use pagination for large resource lists.
* Cache reusable metadata.
* Optimize video previews.
* Avoid loading all course media simultaneously.
* Preserve local draft state.

---

# 74. Security Requirements

The builder must enforce:

```text
Authentication
Authorization
RBAC
Course Ownership
Instructor Permissions
Content Permissions
Publishing Permissions
Version Permissions
```

The frontend must not be treated as the security boundary.

---

# 75. Audit Requirements

Record significant actions:

```text
Course Created
Course Edited
Module Added
Lesson Added
Question Added
AI Lesson Imported
Question Modified
Course Submitted
Course Approved
Course Published
Course Unpublished
Course Deleted
```

---

# 76. Testing Requirements

## Functional

* [ ] Create course.
* [ ] Edit course.
* [ ] Add module.
* [ ] Add lesson.
* [ ] Add video.
* [ ] Import AI lesson.
* [ ] Add question.
* [ ] Reorder content.
* [ ] Save draft.
* [ ] Recover draft.
* [ ] Validate course.
* [ ] Preview course.
* [ ] Submit review.
* [ ] Publish course.
* [ ] Unpublish course.
* [ ] Version course.

## Accessibility

* [ ] Keyboard navigation.
* [ ] Screen reader.
* [ ] Focus management.
* [ ] Drag/drop alternative.
* [ ] Accessible forms.
* [ ] Accessible errors.

## Security

* [ ] Unauthorized users cannot edit.
* [ ] Unauthorized users cannot publish.
* [ ] Course ownership is enforced.
* [ ] API permissions are enforced.

---

# 77. Acceptance Criteria

The Course Builder is complete when:

* [ ] Courses can be created.
* [ ] Modules can be created.
* [ ] Lessons can be created.
* [ ] Videos can be attached.
* [ ] AI-generated lessons can be imported.
* [ ] Questions can be added and edited.
* [ ] Content can be reordered.
* [ ] Course metadata can be edited.
* [ ] Learning objectives can be configured.
* [ ] Completion rules can be configured.
* [ ] Access rules can be configured.
* [ ] Languages can be configured.
* [ ] Course validation works.
* [ ] Preview works.
* [ ] Draft saving works.
* [ ] Autosave works.
* [ ] Versioning works.
* [ ] Review workflow works.
* [ ] Publishing works.
* [ ] RBAC works.
* [ ] Audit logging works.
* [ ] Responsive design works.
* [ ] Accessibility requirements are satisfied.

---

# 78. Definition of Done

```text
Course Creation             ✓
Course Structure            ✓
Module Management           ✓
Lesson Management           ✓
Video Integration           ✓
AI Lesson Import            ✓
Question Management         ✓
Translation Management      ✓
Access Configuration        ✓
Completion Rules            ✓
Preview                     ✓
Validation                  ✓
Autosave                    ✓
Versioning                  ✓
Review Workflow             ✓
Publishing                  ✓
RBAC                        ✓
Audit Logging               ✓
Responsive UI               ✓
Accessibility               ✓
Testing                     ✓
```

---

# 79. Relationship With Other UI Documents

```text
04_UI_UX_BLUEPRINT/
│
├── 06_Admin_Dashboard.md
│
├── 07_Video_Player.md
│
├── 08_Interactive_Question_UI.md
│
├── 09_Course_Builder.md       ← THIS DOCUMENT
│
├── 10_Video_Upload_UI.md
│
├── 11_AI_Review_UI.md
│
├── 12_Analytics_UI.md
│
├── 13_Responsive_Design.md
│
├── 14_Accessibility.md
│
└── 15_UI_UX_Appendix.md
```

---

# 80. Final Architecture

```text
                    COURSE BUILDER
                           │
        ┌──────────────────┼──────────────────┐
        ↓                  ↓                  ↓
      COURSE             MODULE             SETTINGS
        │                  │
        │                  ↓
        │                LESSON
        │                  │
        │        ┌─────────┼──────────┐
        │        ↓         ↓          ↓
        │      VIDEO    QUESTIONS  TRANSCRIPT
        │        │         │          │
        │        └─────────┼──────────┘
        │                  ↓
        │             TRANSLATIONS
        │                  ↓
        └─────────────── REVIEW
                           ↓
                       VALIDATION
                           ↓
                        PUBLISH
                           ↓
                     STUDENT PLAYER
```

The Course Builder is the **content orchestration layer** of AILPG, connecting AI-generated educational assets with the final student learning experience.

**Document Status:** Ready for implementation planning.
