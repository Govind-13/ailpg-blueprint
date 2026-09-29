# AILPG — Design System

**Project:** AI Learning Platform Generator (AILPG)
**Layer:** UI/UX Blueprint
**Document:** 02 — Design System
**Status:** Draft / Implementation Ready
**Version:** 1.0

---

## 1. Purpose

The AILPG Design System defines the reusable visual language, UI components, interaction patterns, design tokens, accessibility rules, responsive behavior, and component standards used throughout the platform.

The design system must provide a consistent experience across:

* Student application
* Teacher dashboard
* Admin dashboard
* Course builder
* Video upload workflow
* AI processing workflow
* AI review interface
* Interactive lesson player
* Question editor
* Analytics
* Authentication
* Subscription and account management
* Mobile, tablet, and desktop interfaces

The objective is to ensure that every new screen or feature can be built using a predictable set of reusable components rather than designing each screen independently.

---

# 2. Design System Goals

The design system should achieve the following:

1. Consistent visual identity
2. Fast UI development
3. Reusable components
4. Responsive layouts
5. Accessibility
6. Clear educational interactions
7. Strong video-learning experience
8. Easy teacher workflows
9. Clear AI processing states
10. Consistent error handling
11. Consistent loading states
12. Theme support
13. Dark mode support
14. Mathematical content readability
15. Easy Figma-to-development handoff

---

# 3. Design Principles

## 3.1 Clarity First

Every interface should answer:

* Where am I?
* What am I doing?
* What should I do next?
* What happened?
* What requires my attention?

Avoid unnecessary visual complexity.

---

## 3.2 Learning Over Decoration

AILPG is an education platform.

Visual elements must support learning rather than distract from it.

Priority:

```text
Learning Content
      ↓
Interactive Activity
      ↓
Navigation
      ↓
Supporting Information
      ↓
Decoration
```

---

## 3.3 Progressive Disclosure

Do not expose every advanced control immediately.

Example:

```text
Basic View
    ↓
Advanced Options
    ↓
Detailed Configuration
```

For example, a teacher uploading a video should initially see:

* Upload video
* Select course
* Select language
* Start processing

Advanced options can include:

* AI question generation settings
* Difficulty
* Question frequency
* Translation languages
* Quality settings
* Checkpoint behavior

---

## 3.4 Consistency

The same action should look and behave the same everywhere.

For example:

```text
Primary action → Primary Button
Delete → Destructive Button
Save → Save Button
Cancel → Secondary Button
Information → Info Alert
Warning → Warning Alert
```

---

## 3.5 Feedback

Every important user action should produce visible feedback.

Examples:

```text
Upload Started
      ↓
Uploading
      ↓
Upload Complete
      ↓
AI Processing
      ↓
AI Processing Complete
      ↓
Ready for Review
```

---

# 4. Design Token Architecture

AILPG should use design tokens rather than hard-coded styling values throughout the application.

Recommended token hierarchy:

```text
Primitive Tokens
       ↓
Semantic Tokens
       ↓
Component Tokens
       ↓
Screen UI
```

Example:

```text
Primitive:
color.blue.500

        ↓

Semantic:
color.action.primary

        ↓

Component:
button.primary.background

        ↓

UI:
Upload Video Button
```

---

# 5. Color System

The final brand palette should be established during visual design and validated for WCAG contrast.

The following is a proposed token structure rather than a mandatory final color palette.

## 5.1 Brand Colors

```text
brand.primary
brand.primary.hover
brand.primary.active
brand.primary.light
brand.primary.dark
```

Example semantic usage:

| Token                  | Usage                |
| ---------------------- | -------------------- |
| `brand.primary`        | Main actions         |
| `brand.primary.hover`  | Hover                |
| `brand.primary.active` | Pressed state        |
| `brand.primary.light`  | Background highlight |
| `brand.primary.dark`   | Strong emphasis      |

---

# 6. Semantic Colors

## Success

Used for:

* Completed processing
* Correct answers
* Published lessons
* Successful uploads
* Completed tasks

```text
color.success
color.success.background
color.success.border
color.success.text
```

---

## Warning

Used for:

* Processing delays
* Missing configuration
* Subscription limitations
* Unsaved changes

```text
color.warning
color.warning.background
color.warning.border
color.warning.text
```

---

## Error

Used for:

* Failed upload
* Processing failure
* Invalid form data
* Authentication errors

```text
color.error
color.error.background
color.error.border
color.error.text
```

---

## Information

Used for:

* AI explanations
* Helpful tips
* System information
* Feature descriptions

```text
color.info
color.info.background
color.info.border
color.info.text
```

---

# 7. Neutral Colors

Neutral tokens should support:

* Backgrounds
* Borders
* Text
* Disabled states
* Dividers
* Cards

Suggested structure:

```text
neutral.0
neutral.50
neutral.100
neutral.200
neutral.300
neutral.400
neutral.500
neutral.600
neutral.700
neutral.800
neutral.900
neutral.950
```

Do not rely only on color to communicate meaning.

---

# 8. Typography System

Typography must prioritize readability, particularly for:

* Long lesson content
* Mathematical equations
* Video explanations
* Question text
* Teacher analytics
* Mobile devices

---

## 8.1 Font Families

Use a modern UI font for application interfaces.

Example:

```text
Primary UI Font:
Inter / equivalent system UI font

Mathematical Font:
Math-capable rendering system

Code/Technical Font:
Monospace font
```

The exact production fonts can be finalized during visual implementation.

---

# 9. Typography Scale

Recommended hierarchy:

| Token      | Approx. Size | Usage                 |
| ---------- | -----------: | --------------------- |
| Display    |      40–48px | Major page heading    |
| H1         |         32px | Page heading          |
| H2         |         28px | Section heading       |
| H3         |         24px | Subsection            |
| H4         |         20px | Component heading     |
| Body Large |         18px | Important content     |
| Body       |         16px | Default text          |
| Body Small |         14px | Secondary information |
| Caption    |         12px | Metadata              |
| Micro      |         11px | Rare utility text     |

Line height should be selected according to the content type rather than using a single global value.

---

# 10. Mathematical Typography

Mathematical content is a core AILPG requirement.

The UI must support:

* Fractions
* Exponents
* Subscripts
* Square roots
* Integrals
* Summations
* Matrices
* Equations
* Variables
* Greek symbols
* Multi-line equations

Example:

```text
x² + 5x + 6 = 0
```

Rendering should preferably use a proper mathematical rendering system rather than plain text.

Potential implementation:

```text
LaTeX / MathML
       ↓
Math Renderer
       ↓
HTML Lesson
```

Mathematical expressions must remain readable on mobile screens.

---

# 11. Spacing System

Use a consistent spacing scale.

Recommended base:

```text
4px
8px
12px
16px
20px
24px
32px
40px
48px
64px
80px
96px
```

Token example:

```text
space.1 = 4px
space.2 = 8px
space.3 = 12px
space.4 = 16px
space.5 = 20px
space.6 = 24px
space.8 = 32px
space.10 = 40px
space.12 = 48px
space.16 = 64px
```

---

# 12. Border Radius

Suggested radius tokens:

```text
radius.none
radius.sm
radius.md
radius.lg
radius.xl
radius.full
```

Example:

| Token         | Usage               |
| ------------- | ------------------- |
| `radius.sm`   | Inputs              |
| `radius.md`   | Buttons/cards       |
| `radius.lg`   | Large panels        |
| `radius.xl`   | Modal/hero surfaces |
| `radius.full` | Pills/avatars       |

---

# 13. Elevation

Elevation should be subtle.

Recommended:

```text
shadow.none
shadow.sm
shadow.md
shadow.lg
shadow.xl
```

Use elevation to communicate hierarchy, not decoration.

Example:

```text
Page
 └── Card
      └── Modal
           └── Confirmation
```

Each layer can have progressively stronger elevation.

---

# 14. Buttons

Buttons are among the most important reusable components.

## 14.1 Button Variants

```text
Primary
Secondary
Tertiary
Ghost
Destructive
Success
Icon Button
Link Button
```

---

## 14.2 Button Sizes

```text
Small
Medium
Large
```

Recommended default:

```text
Desktop → Medium
Mobile → Medium/Large
Primary CTA → Large
Compact controls → Small
```

---

## 14.3 Button States

Every button should support:

```text
Default
Hover
Focus
Pressed
Disabled
Loading
Success
Error
```

Example:

```text
[ Generate Lesson ]

      ↓ click

[ ◌ Generating... ]

      ↓

[ ✓ Lesson Generated ]
```

---

# 15. Form Components

Required components:

* Text input
* Textarea
* Number input
* Password input
* Search
* Select
* Multi-select
* Checkbox
* Radio
* Toggle
* Slider
* Date picker
* Time picker
* File upload
* Language selector

---

# 16. Input States

Every input must support:

```text
Default
Focus
Filled
Hover
Disabled
Read-only
Error
Success
```

Example:

```text
Question Title
┌────────────────────────────┐
│ Enter question title       │
└────────────────────────────┘
```

Error:

```text
┌────────────────────────────┐
│                            │
└────────────────────────────┘
Question title is required.
```

---

# 17. Cards

Cards are used extensively throughout AILPG.

Examples:

* Course card
* Lesson card
* Video card
* AI processing card
* Student progress card
* Analytics card
* Subscription card

Basic structure:

```text
┌─────────────────────────────┐
│ Icon / Thumbnail             │
│                              │
│ Title                        │
│ Description                  │
│ Metadata                     │
│                              │
│ [Action]                     │
└─────────────────────────────┘
```

---

# 18. Navigation

## Desktop

Recommended structure:

```text
┌─────────────────────────────────────────────┐
│ Logo                       Search   Profile │
├──────────────┬──────────────────────────────┤
│              │                              │
│ Dashboard    │                              │
│ Courses      │         Main Content         │
│ Lessons      │                              │
│ Videos       │                              │
│ Analytics    │                              │
│ Settings     │                              │
│              │                              │
└──────────────┴──────────────────────────────┘
```

---

## Student Navigation

Recommended:

```text
Home
My Courses
Continue Learning
Progress
Profile
```

---

## Teacher Navigation

Recommended:

```text
Dashboard
Courses
Lessons
Videos
AI Processing
Question Bank
Analytics
Settings
```

---

## Admin Navigation

Recommended:

```text
Dashboard
Users
Organizations
Courses
Content
AI Jobs
Subscriptions
Analytics
System Health
Audit Logs
Settings
```

---

# 19. Mobile Navigation

Use bottom navigation for primary student actions.

Example:

```text
┌──────────────────────────────┐
│                              │
│         Main Content         │
│                              │
├──────────────────────────────┤
│ Home Courses Progress Profile│
└──────────────────────────────┘
```

Secondary actions should be placed inside menus or sheets.

---

# 20. Top Bar

Desktop top bar may contain:

```text
Logo
Breadcrumb
Search
Notifications
Help
Profile
```

Example:

```text
AILPG | Courses / Algebra / Quadratic Equations

                         🔔   ?   Profile
```

---

# 21. Breadcrumbs

Breadcrumbs help users understand their location.

Example:

```text
Courses
  /
Algebra
  /
Quadratic Equations
  /
Lesson 04
```

Mobile can collapse breadcrumbs when space is limited.

---

# 22. Tabs

Tabs should be used for related views.

Example:

```text
Overview | Content | Questions | Analytics
```

Active tab must be visually distinguishable and accessible without relying only on color.

---

# 23. Chips and Badges

Use chips for:

* Status
* Category
* Difficulty
* Language
* Content type
* Subscription tier

Examples:

```text
[Published]
[Draft]
[Processing]
[AI Generated]
[Easy]
[Medium]
[Hard]
[English]
[Premium]
```

---

# 24. Status System

AILPG processing status should use consistent labels.

```text
Draft
Uploading
Uploaded
Queued
Processing
AI Analyzing
Generating
Review Required
Ready
Published
Failed
Archived
```

Example:

```text
● Processing
```

The visual indicator must also include text.

Do not communicate status using color alone.

---

# 25. Dialogs

Dialogs are appropriate for:

* Delete confirmation
* Publish confirmation
* Important configuration
* Unsaved changes
* Subscription confirmation
* AI regeneration confirmation

Example:

```text
┌─────────────────────────────────────┐
│ Publish Lesson?                     │
│                                     │
│ This lesson will become available  │
│ to enrolled students.               │
│                                     │
│ [Cancel]             [Publish]      │
└─────────────────────────────────────┘
```

---

# 26. Drawers

Use drawers for secondary configuration.

Examples:

```text
Question Settings
Video Settings
AI Settings
Filter Panel
Lesson Properties
```

Desktop:

```text
Main Content                Drawer
──────────────────────────┌──────────────┐
                          │ Settings     │
                          │              │
                          │ Options      │
                          │              │
                          │ [Save]       │
                          └──────────────┘
```

---

# 27. Toast Notifications

Toasts should provide short-lived feedback.

Examples:

```text
✓ Lesson saved
✓ Question added
✓ Video uploaded
⚠ Translation still processing
✕ Upload failed
```

Toasts should not contain critical information that disappears before the user can act on it.

---

# 28. Alerts

Use persistent alerts for important information.

Types:

```text
Info
Success
Warning
Error
```

Example:

```text
┌─────────────────────────────────────┐
│ ⚠ Translation is still processing. │
│ You can continue editing English.  │
└─────────────────────────────────────┘
```

---

# 29. Progress Indicators

AILPG requires several progress patterns.

## Upload Progress

```text
Uploading video

██████████████░░░░  78%

1.4 GB / 1.8 GB
```

---

## AI Processing Progress

Avoid showing false precision if the backend cannot accurately estimate completion.

Prefer:

```text
Analyzing video
●●●○○

Current step:
Extracting transcript
```

rather than claiming:

```text
73% complete
```

unless the system has reliable progress data.

---

# 30. Skeleton Loading

Use skeleton screens when content structure is known.

Example:

```text
┌──────────────────────────┐
│ ███████████              │
│ █████████████████        │
│ ███████                  │
└──────────────────────────┘
```

Skeletons should approximately match the final layout.

---

# 31. Empty States

Every major screen should have an intentional empty state.

Example:

```text
No Courses Yet

Create your first course to start building
interactive learning experiences.

[ Create Course ]
```

---

# 32. Error States

Errors must explain:

1. What happened
2. Why it happened when known
3. What the user can do

Example:

```text
Video Processing Failed

We couldn't process this video.

Possible reasons:
• Unsupported format
• Corrupted file
• Processing service unavailable

[ Retry Processing ]
[ Contact Support ]
```

---

# 33. Video Player Design

The video player is a core AILPG component.

Basic structure:

```text
┌─────────────────────────────────────────┐
│                                         │
│               VIDEO                     │
│                                         │
│                                         │
├─────────────────────────────────────────┤
│ ▶  ━━━━━━━━━━━●━━━━━━━━━━  08:32/15:40 │
│ 🔊  ⚙ Quality  CC  Language  ⛶         │
└─────────────────────────────────────────┘
```

Required controls:

* Play/pause
* Seek
* Volume
* Fullscreen
* Playback speed
* Quality
* Captions
* Language
* Picture-in-picture where supported
* Progress

---

# 34. Video Quality Selector

Example:

```text
Quality

○ Auto
○ 360p
○ 480p
○ 720p
○ 1080p
```

Subscription restrictions should be clearly communicated.

Example:

```text
1080p
Premium
```

Do not silently downgrade quality.

---

# 35. Language Selector

Example:

```text
Language

English
தமிழ்
हिन्दी
العربية
```

The UI should display native language names where practical.

---

# 36. Interactive Question Component

Questions are inserted at configured lesson checkpoints.

Example:

```text
┌──────────────────────────────────────┐
│ Checkpoint Question                  │
│                                      │
│ What is the value of x?              │
│                                      │
│ ○ 2                                  │
│ ○ 3                                  │
│ ○ 4                                  │
│ ○ 5                                  │
│                                      │
│              [ Submit Answer ]        │
└──────────────────────────────────────┘
```

---

# 37. Question States

```text
Not Started
Active
Answered
Correct
Incorrect
Skipped
Locked
Review
```

---

# 38. Correct Answer Feedback

Example:

```text
✓ Correct!

x = 3 because:

2x + 4 = 10
2x = 6
x = 3
```

Feedback should support learning rather than simply saying:

```text
Correct.
```

---

# 39. Incorrect Answer Feedback

Example:

```text
Not quite.

Remember to subtract 4 before dividing by 2.

[Try Again]
[Continue]
```

Teacher configuration can determine whether retry is allowed.

---

# 40. Question Difficulty

Use consistent difficulty levels:

```text
Easy
Medium
Hard
```

Optional internal metadata:

```text
difficulty_score
cognitive_level
estimated_time
```

---

# 41. Course Components

Reusable course components:

```text
CourseCard
CourseHeader
CourseProgress
CourseModule
LessonCard
LessonList
LessonProgress
LessonStatus
EnrollmentCard
```

---

# 42. Lesson Components

Reusable lesson components:

```text
LessonHeader
LessonPlayer
LessonTimeline
LessonCheckpoint
LessonQuestion
LessonTranscript
LessonResources
LessonNavigation
LessonProgress
```

---

# 43. Teacher Components

```text
VideoUploader
ProcessingStatus
AIReviewPanel
QuestionEditor
QuestionTimeline
TranscriptEditor
TranslationEditor
LessonPreview
PublishPanel
```

---

# 44. Admin Components

```text
UserTable
OrganizationTable
SystemHealthCard
AIJobTable
ProcessingQueue
AuditLogTable
SubscriptionTable
PlatformMetrics
```

---

# 45. Analytics Components

Analytics components should include:

```text
MetricCard
LineChart
BarChart
ProgressChart
CompletionChart
QuestionPerformance
EngagementChart
FilterBar
DateRangePicker
DataTable
```

---

# 46. Analytics Metric Cards

Example:

```text
┌──────────────────────┐
│ Lesson Completion    │
│                      │
│ 78.4%                │
│ ↑ 6.2%               │
└──────────────────────┘
```

Do not use color alone to communicate positive/negative changes.

Include symbols and text where appropriate.

---

# 47. Data Tables

Tables are primarily for teacher/admin interfaces.

Required features:

* Sorting
* Filtering
* Pagination
* Search
* Column selection where needed
* Row actions
* Responsive fallback
* Loading state
* Empty state
* Error state

Example:

```text
Course        Students    Completion    Status
------------------------------------------------
Algebra       120         82%           Published
Geometry       84         71%           Published
Calculus       32         43%           Draft
```

---

# 48. Filters

Common filter components:

```text
Search
Status
Course
Language
Difficulty
Date
Teacher
Subscription
```

Filters should be removable individually.

Example:

```text
Filters:
[Published ×] [English ×] [Medium ×]
```

---

# 49. Icons

Use a single consistent icon library.

Icons should:

* Have consistent stroke/fill style
* Have consistent sizing
* Support accessibility
* Not replace text for critical actions

Example:

```text
[ + Add Question ]
```

is preferable to:

```text
[ + ]
```

for ambiguous actions.

---

# 50. Icon Sizes

Recommended:

```text
12px → Micro
16px → Standard
20px → Button/navigation
24px → Main UI
32px → Feature/icon card
48px+ → Empty states
```

---

# 51. Avatar System

Support:

```text
User avatar
Initial avatar
Organization logo
Teacher avatar
Admin avatar
```

Sizes:

```text
24
32
40
48
64
```

---

# 52. Responsive Breakpoints

The exact breakpoints may be adjusted during implementation.

Recommended baseline:

```text
Mobile:
< 640px

Tablet:
640px – 1023px

Desktop:
1024px – 1439px

Large Desktop:
≥ 1440px
```

---

# 53. Responsive Layout Rules

## Mobile

Prioritize:

```text
Content
↓
Primary action
↓
Secondary actions
```

Hide or collapse:

* Large navigation
* Secondary filters
* Nonessential metadata

---

## Tablet

Use:

* Collapsible sidebar
* Two-column layouts where appropriate
* Larger touch targets

---

## Desktop

Use:

* Persistent navigation
* Multi-column dashboards
* Side panels
* Timeline editors
* Advanced analytics

---

# 54. Touch Targets

Interactive elements should have sufficiently large touch targets.

Target:

```text
~44px minimum interactive area
```

This is particularly important for:

* Video controls
* Question options
* Mobile navigation
* Buttons
* Sliders
* Checkboxes

---

# 55. Accessibility

AILPG should target WCAG 2.2 AA as the accessibility direction.

Requirements include:

* Keyboard navigation
* Visible focus indicators
* Screen reader labels
* Sufficient contrast
* Semantic HTML
* Accessible form errors
* Accessible dialogs
* Accessible media controls
* Captions
* Transcript support
* Reduced-motion support
* Logical heading hierarchy

---

# 56. Color Accessibility

Never communicate information using color alone.

Bad:

```text
● Green = Correct
● Red = Incorrect
```

Better:

```text
✓ Correct
✕ Incorrect
```

Color can reinforce the meaning but should not be the only signal.

---

# 57. Focus States

Every interactive element must have a visible focus state.

Example:

```text
┌─────────────────────────┐
│   Create Lesson         │
└─────────────────────────┘
       ↑
   Focus indicator
```

---

# 58. Dark Mode

AILPG should support dark mode through semantic tokens.

Example:

```text
Background
Surface
Surface Elevated
Text Primary
Text Secondary
Border
Primary
Success
Warning
Error
```

Avoid hard-coding:

```css
background: #ffffff;
```

throughout the application.

Instead use:

```text
color.background.primary
```

---

# 59. Theme Architecture

Recommended:

```text
Theme
 ├── Light
 ├── Dark
 └── System
```

User preference:

```text
System
Light
Dark
```

---

# 60. Motion System

Motion should communicate state and hierarchy.

Use motion for:

* Page transitions
* Modal opening
* Drawer opening
* Progress
* Question feedback
* Upload state
* AI processing state

Avoid unnecessary animation.

---

# 61. Motion Tokens

Example:

```text
motion.fast
motion.normal
motion.slow
```

Recommended conceptual ranges:

```text
Fast:
~100–150ms

Normal:
~200–300ms

Slow:
~400–500ms
```

Exact values should be validated during implementation.

---

# 62. Reduced Motion

When the operating system requests reduced motion:

```text
Disable decorative animations
Reduce transitions
Avoid large movement
Keep functional feedback
```

---

# 63. AI Processing UI

AI is a major part of AILPG, so processing states need special treatment.

Example:

```text
Video Uploaded
      ↓
Extracting Audio
      ↓
Generating Transcript
      ↓
Analyzing Solution
      ↓
Detecting Concepts
      ↓
Generating Questions
      ↓
Generating Translation
      ↓
Building Interactive Lesson
      ↓
Ready for Review
```

The UI should show the current stage.

---

# 64. AI Confidence Indicators

If the AI pipeline produces confidence information, the teacher review UI may display:

```text
High Confidence
Medium Confidence
Needs Review
```

These indicators should be explained clearly.

Example:

```text
AI confidence: Medium

Review recommended before publishing.
```

---

# 65. AI Review Components

Recommended UI:

```text
┌─────────────────────────────────────────────┐
│ Video                                      │
│                                             │
│ Transcript                                  │
│ ─────────────────────────────────────────── │
│                                             │
│ AI Generated Question                      │
│                                             │
│ [ Edit ] [ Regenerate ] [ Approve ]        │
└─────────────────────────────────────────────┘
```

---

# 66. Question Timeline

Teacher interface:

```text
00:00 ───────●────────●──────────●────── 15:40
             Q1       Q2         Q3
```

Selecting a checkpoint opens its editor.

---

# 67. Course Builder Design

Course builder should support:

```text
Course
 ├── Module
 │    ├── Lesson
 │    ├── Lesson
 │    └── Lesson
 │
 ├── Module
 │    ├── Lesson
 │    └── Lesson
```

Drag-and-drop may be supported on desktop.

Mobile should use explicit move/reorder controls where drag-and-drop is difficult.

---

# 68. Auto-Save

Teacher editing interfaces should support auto-save where practical.

States:

```text
Saving...
Saved
Unsaved Changes
Save Failed
```

Example:

```text
✓ Saved 10 seconds ago
```

---

# 69. Unsaved Changes

When leaving an unsaved editing screen:

```text
Unsaved Changes

You have changes that haven't been saved.

[Stay]
[Discard]
[Save & Exit]
```

---

# 70. Component Naming Convention

Use predictable component names.

Example:

```text
Button
PrimaryButton
VideoPlayer
QuestionCard
QuestionOption
LessonCard
CourseCard
ProcessingStatus
ProgressBar
Modal
Drawer
Toast
```

Avoid ambiguous names such as:

```text
Box1
CardNew
BlueButton
Thing
Container2
```

---

# 71. Component Folder Structure

Recommended:

```text
src/
└── components/
    ├── common/
    │   ├── Button/
    │   ├── Input/
    │   ├── Modal/
    │   └── Toast/
    │
    ├── video/
    │   ├── VideoPlayer/
    │   ├── VideoControls/
    │   └── QualitySelector/
    │
    ├── learning/
    │   ├── LessonPlayer/
    │   ├── QuestionCard/
    │   ├── ProgressBar/
    │   └── Checkpoint/
    │
    ├── teacher/
    │   ├── VideoUploader/
    │   ├── AIReview/
    │   └── QuestionEditor/
    │
    └── analytics/
        ├── MetricCard/
        ├── Chart/
        └── DataTable/
```

---

# 72. Figma Organization

The Figma project should mirror the design system.

Recommended structure:

```text
AILPG Design System

01 Foundations
02 Colors
03 Typography
04 Spacing
05 Icons
06 Components
07 Patterns
08 Student
09 Teacher
10 Admin
11 Responsive
12 Prototypes
```

---

# 73. Component Documentation

Every major component should document:

```text
Component Name
Purpose
Variants
States
Properties
Responsive Behavior
Accessibility
Usage
Do / Don't
```

Example:

```text
Component:
QuestionCard

Variants:
MCQ
TrueFalse
ShortAnswer

States:
Default
Selected
Correct
Incorrect
Disabled

Accessibility:
Keyboard selectable
Screen reader labels
Visible focus state
```

---

# 74. Design-to-Code Mapping

Each Figma component should map to a production component.

Example:

```text
Figma:
AILPG / Button / Primary

        ↓

Frontend:
components/common/Button/PrimaryButton
```

This reduces design-development mismatch.

---

# 75. Design Token Export

Design tokens should eventually be exportable into application configuration.

Conceptual structure:

```json
{
  "color": {
    "action": {
      "primary": "...",
      "secondary": "..."
    },
    "status": {
      "success": "...",
      "warning": "...",
      "error": "..."
    }
  },
  "spacing": {
    "sm": "...",
    "md": "...",
    "lg": "..."
  }
}
```

The actual implementation format can be JSON, TypeScript, CSS variables, Flutter theme configuration, or another platform-specific representation.

---

# 76. Web Implementation

For web:

```text
Design Tokens
      ↓
CSS Variables / Theme
      ↓
Component Library
      ↓
Pages
```

Example:

```css
:root {
  --color-bg-primary: ...;
  --color-surface: ...;
  --color-text-primary: ...;
  --space-md: ...;
  --radius-md: ...;
}
```

---

# 77. Flutter Implementation

If Flutter is used for the mobile application:

```text
Design Tokens
      ↓
ThemeData
      ↓
Reusable Widgets
      ↓
Screens
```

Recommended reusable layers:

```text
AppTheme
AppColors
AppTypography
AppSpacing
AppRadius
AppButton
AppInput
AppCard
AppDialog
```

---

# 78. Design System Versioning

The design system should be versioned.

Example:

```text
Design System v1.0
Design System v1.1
Design System v2.0
```

Breaking component changes should trigger a major version.

---

# 79. Backward Compatibility

When changing components:

```text
Old Component
      ↓
Deprecation
      ↓
Migration Guide
      ↓
New Component
      ↓
Old Component Removed
```

Do not silently change behavior that could break existing screens.

---

# 80. Design QA Checklist

Before a component is considered complete:

### Visual

* [ ] Correct typography
* [ ] Correct spacing
* [ ] Correct alignment
* [ ] Correct borders
* [ ] Correct radius
* [ ] Correct elevation
* [ ] Correct icons

### Interaction

* [ ] Hover
* [ ] Focus
* [ ] Pressed
* [ ] Disabled
* [ ] Loading
* [ ] Error
* [ ] Success

### Responsive

* [ ] Mobile
* [ ] Tablet
* [ ] Desktop
* [ ] Large desktop

### Accessibility

* [ ] Keyboard navigation
* [ ] Focus visible
* [ ] Screen reader support
* [ ] Contrast checked
* [ ] Touch target checked
* [ ] Color not sole indicator

### Content

* [ ] Long text
* [ ] Short text
* [ ] Empty state
* [ ] Error state
* [ ] Loading state

---

# 81. Design Review Checklist

Before releasing a screen:

```text
□ Does the screen have one clear primary action?
□ Is the user's current location clear?
□ Are important states visible?
□ Are errors understandable?
□ Is loading handled?
□ Is the empty state designed?
□ Is mobile behavior defined?
□ Is accessibility considered?
□ Is mathematical content readable?
□ Does the screen use existing components?
□ Are new components documented?
```

---

# 82. Definition of Done

The AILPG Design System is considered implementation-ready when:

* [ ] Design tokens are defined
* [ ] Color system is defined
* [ ] Typography is defined
* [ ] Spacing system is defined
* [ ] Component naming is defined
* [ ] Button system is defined
* [ ] Form system is defined
* [ ] Navigation system is defined
* [ ] Status system is defined
* [ ] Video components are defined
* [ ] Interactive question components are defined
* [ ] Course/lesson components are defined
* [ ] Analytics components are defined
* [ ] Responsive rules are defined
* [ ] Accessibility rules are defined
* [ ] Dark mode architecture is defined
* [ ] Motion rules are defined
* [ ] Figma organization is defined
* [ ] Development mapping is defined
* [ ] QA checklist is defined

---

# 83. Recommended Component Priority

Implementation should follow this order.

## Phase 1 — Foundations

```text
Colors
Typography
Spacing
Icons
Theme
```

## Phase 2 — Core Components

```text
Button
Input
Select
Card
Modal
Toast
Alert
Tabs
Badge
```

## Phase 3 — Learning Components

```text
Video Player
Question Card
Checkpoint
Lesson Progress
Transcript
```

## Phase 4 — Teacher Components

```text
Video Upload
AI Processing
AI Review
Question Editor
Timeline
Lesson Preview
```

## Phase 5 — Analytics

```text
Metric Card
Chart
Table
Filters
Date Range
```

## Phase 6 — Advanced

```text
Course Builder
Subscription UI
Admin Components
Advanced Analytics
```

---

# 84. Design System Architecture

Final conceptual architecture:

```text
                    AILPG DESIGN SYSTEM
                           │
             ┌─────────────┴─────────────┐
             │                           │
        FOUNDATIONS                  THEMING
             │                           │
     ┌───────┼────────┐          ┌───────┴───────┐
     │       │        │          │               │
   Color  Type     Spacing      Light           Dark
     │       │        │
     └───────┼────────┘
             │
       CORE COMPONENTS
             │
   ┌─────────┼──────────┐
   │         │          │
 Common   Learning   Dashboard
   │         │          │
   └─────────┼──────────┘
             │
       PRODUCT PATTERNS
             │
     ┌───────┼────────┐
     │       │        │
 Student  Teacher   Admin
     │       │        │
     └───────┼────────┘
             │
           SCREENS
```

---

# 85. Final Design Principle

The AILPG design system should make the platform feel like **one product**, even though it contains multiple applications and workflows.

The user should not feel that:

```text
Student App
Teacher Dashboard
Admin Dashboard
AI Review
Video Player
Analytics
```

are separate products.

They should feel like:

```text
                 AILPG
                   │
        ┌──────────┼──────────┐
        │          │          │
      Learn      Create     Manage
        │          │          │
     Student     Teacher     Admin
```

The design system is the visual and interaction layer connecting all of them.

---

## Next Document

The next UI/UX blueprint file is:

```text
03_User_Flow.md
```

It will define the complete user journeys for:

* Student
* Teacher
* Admin
* MP4 upload
* AI processing
* AI review
* Interactive lesson generation
* Question answering
* Course publishing
* Student learning
* Analytics
* Subscription/quality flow
* Error and recovery paths

---

## Git Commit

```bash
git add docs/04_UI_UX_Blueprint/02_Design_System.md
git commit -m "docs(ui-ux): add design system"
```
