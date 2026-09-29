# AILPG — Accessibility Blueprint

**Document Path:** `docs/04_UI_UX_BLUEPRINT/14_Accessibility.md`
**Project:** MP4 → Interactive Learning Platform Generator (AILPG)
**Document Type:** UI/UX Accessibility Specification
**Version:** 1.0
**Status:** Draft / Implementation Ready

---

## 1. Document Purpose

This document defines the accessibility requirements for the AILPG platform.

AILPG converts solved-math-problem videos into interactive learning experiences. Accessibility must therefore apply not only to ordinary navigation and forms, but also to:

* Video playback
* Captions and transcripts
* Mathematical equations
* Interactive questions
* AI-generated learning content
* OCR content
* Course navigation
* Student progress
* Instructor tools
* AI review interfaces
* Analytics dashboards
* Administrative interfaces

The objective is to ensure that users with different visual, auditory, motor, cognitive, language, and device-access needs can successfully create, review, access, and complete lessons.

---

# 2. Accessibility Goals

AILPG should be designed so that accessibility is a fundamental product requirement rather than a later enhancement.

### Primary goals

1. All important functionality must be usable without a mouse.
2. Content must work with screen readers.
3. Interactive questions must be accessible.
4. Mathematical content must have accessible representations.
5. Video content must provide appropriate captions/transcripts.
6. Color must never be the only method of communicating information.
7. Text must remain usable when enlarged.
8. Interfaces must support keyboard focus and logical navigation.
9. Motion and animation must respect user preferences.
10. Accessibility must be included in development, QA, and release processes.

---

# 3. Accessibility Principles

AILPG follows four core principles.

## 3.1 Perceivable

Users must be able to perceive important information.

Examples:

* Text alternatives for meaningful images
* Captions
* Transcripts
* Accessible mathematical representations
* Sufficient contrast
* Non-color indicators
* Scalable text
* Visual focus indicators

---

## 3.2 Operable

Users must be able to operate the interface.

Examples:

* Keyboard navigation
* Accessible buttons
* Logical focus order
* Large touch targets
* Accessible dialogs
* No keyboard traps
* Accessible video controls
* Sufficient interaction time

---

## 3.3 Understandable

The interface should behave predictably.

Examples:

* Clear labels
* Consistent navigation
* Meaningful error messages
* Predictable controls
* Plain-language instructions
* Consistent terminology
* Clear question feedback

---

## 3.4 Robust

Content should work across different technologies.

Examples:

* Semantic HTML
* Correct ARIA usage
* Screen-reader compatibility
* Browser compatibility
* Assistive-technology testing
* Accessible API/state communication

---

# 4. Accessibility Target

AILPG should target **WCAG 2.2 AA** as the primary accessibility baseline for web interfaces.

The implementation should treat accessibility as a combination of:

```text
Design
   ↓
Semantic HTML
   ↓
Keyboard Support
   ↓
Screen Reader Support
   ↓
Visual Accessibility
   ↓
Content Accessibility
   ↓
Assistive Technology Testing
   ↓
Continuous Accessibility QA
```

Accessibility requirements should be incorporated into the Definition of Done for every UI component.

---

# 5. Accessibility Scope

Accessibility applies to the following AILPG surfaces.

| Area              | Accessibility Requirement                    |
| ----------------- | -------------------------------------------- |
| Authentication    | Full keyboard and screen-reader support      |
| Student Dashboard | Accessible navigation and cards              |
| Course Builder    | Keyboard-operable editing                    |
| Video Player      | Accessible controls and captions             |
| Questions         | Accessible input and feedback                |
| Mathematics       | Accessible equation representation           |
| Transcript        | Searchable and screen-reader friendly        |
| OCR               | Text alternatives                            |
| Upload            | Keyboard-accessible upload workflow          |
| AI Review         | Accessible review controls                   |
| Analytics         | Accessible charts and tables                 |
| Admin Dashboard   | Accessible navigation and forms              |
| Settings          | Accessible controls                          |
| Translation       | Language identification and readable content |
| Notifications     | Accessible status announcements              |
| Dialogs           | Correct focus handling                       |

---

# 6. Semantic HTML Requirements

AILPG should prefer native HTML elements before using custom ARIA widgets.

### Preferred

```html
<button type="button">
  Submit Answer
</button>
```

### Avoid

```html
<div role="button">
  Submit Answer
</div>
```

Native elements provide built-in keyboard and accessibility behavior.

---

## 6.1 Required semantic elements

Use:

```text
<header>
<nav>
<main>
<section>
<article>
<aside>
<footer>
<button>
<a>
<form>
<label>
<input>
<select>
<textarea>
<table>
<fieldset>
<legend>
```

where appropriate.

---

# 7. Landmark Architecture

The application should expose meaningful page landmarks.

Example:

```text
Application
│
├── Header
│
├── Navigation
│
├── Main
│   ├── Page heading
│   ├── Content
│   └── Secondary content
│
└── Footer
```

Screen-reader users should be able to navigate directly to:

* Navigation
* Main content
* Search
* Complementary information
* Footer

---

# 8. Heading Hierarchy

Every page should maintain a logical heading structure.

Example:

```text
H1 — Algebra: Linear Equations

H2 — Video Lesson

H2 — Interactive Questions

H3 — Question 1

H3 — Question 2

H2 — Transcript
```

Do not use headings purely for visual styling.

---

# 9. Keyboard Navigation

All interactive functionality must be accessible using a keyboard.

Minimum supported interactions:

```text
Tab
Shift + Tab
Enter
Space
Arrow Keys
Escape
Home
End
```

Where appropriate.

---

## 9.1 Keyboard Focus Order

Focus order should follow the visual and logical reading order.

Example:

```text
Skip Navigation
      ↓
Header
      ↓
Main Navigation
      ↓
Page Heading
      ↓
Primary Content
      ↓
Secondary Content
      ↓
Footer
```

Focus should never unexpectedly jump to unrelated content.

---

# 10. Visible Focus Indicator

Keyboard focus must always be visually distinguishable.

Example:

```css
:focus-visible {
  outline: 3px solid currentColor;
  outline-offset: 3px;
}
```

The exact visual design may vary, but focus must remain clearly visible against the surrounding interface.

---

# 11. Skip Navigation

Student and administrative pages should provide a skip link.

Example:

```text
[Skip to main content]
```

Expected behavior:

```text
Keyboard user
     ↓
Tab
     ↓
Skip to main content
     ↓
Enter
     ↓
Main content receives focus
```

---

# 12. Focus Management

AILPG contains many dynamic interfaces.

Focus must be managed when:

* Opening dialogs
* Closing dialogs
* Opening drawers
* Submitting questions
* Loading new lesson sections
* Showing errors
* Showing AI review results
* Changing video chapters
* Opening settings

---

## 12.1 Dialog Focus

When a modal opens:

```text
Current focus
     ↓
Modal opens
     ↓
Focus moves to modal heading or first meaningful control
     ↓
User interacts
     ↓
Modal closes
     ↓
Focus returns to triggering element
```

---

## 12.2 Avoid Focus Loss

Do not allow:

```text
Button clicked
     ↓
Component rerenders
     ↓
Focus disappears
     ↓
User must search for current position
```

---

# 13. Screen Reader Requirements

Important UI state changes must be exposed to assistive technologies.

Examples:

```text
"Video paused"

"Question 3 of 8"

"Answer submitted"

"Correct answer"

"Incorrect answer. Try again."

"Upload completed"

"AI processing started"

"AI processing completed"

"Translation ready"
```

Use live regions carefully for dynamic announcements.

---

# 14. ARIA Rules

ARIA should supplement semantic HTML rather than replace it.

Use ARIA when necessary for:

* Tabs
* Dialogs
* Menus
* Comboboxes
* Live regions
* Expandable sections
* Application-specific widgets

Avoid unnecessary ARIA.

### Rule

```text
Native HTML
     ↓
Use native behavior
     ↓
Add ARIA only when needed
```

---

# 15. Button Accessibility

Every button must have an accessible name.

Bad:

```text
[ icon ]
```

Good:

```text
[ icon ] Play video
```

For icon-only controls:

```html
<button aria-label="Play video">
```

Examples:

```text
Play video
Pause video
Mute video
Change quality
Open settings
Translate lesson
Enter fullscreen
Close dialog
Submit answer
Show hint
```

---

# 16. Link Accessibility

Links must communicate their destination.

Avoid:

```text
Click here
Read more
Learn more
```

Prefer:

```text
View Algebra Lesson
Open Course Analytics
Review AI Questions
View Upload Details
```

---

# 17. Form Accessibility

All form fields must have labels.

Example:

```text
Video Title
[________________________]

Primary Language
[ English ▼ ]

Grade Level
[ Grade 8 ▼ ]
```

The label must be programmatically associated with the field.

---

# 18. Error Handling

Errors must be:

* Visible
* Understandable
* Associated with the relevant field
* Announced when appropriate
* Actionable

Example:

```text
Video Title

[________________]

Error:
Video title is required.
```

Avoid:

```text
Invalid input.
```

---

# 19. Form Validation

Validation should occur without unnecessarily disrupting the user's work.

Example:

```text
Submit
  ↓
Validation
  ↓
Error summary
  ↓
Focus moves to first relevant error
```

For long forms, provide an error summary.

---

# 20. Color Accessibility

Color must not be the only information indicator.

Bad:

```text
Green = Correct
Red = Incorrect
```

Better:

```text
✓ Correct
✕ Incorrect
```

Color can reinforce meaning, but text/icons should communicate the actual state.

---

# 21. Contrast

Important content must have sufficient contrast against its background.

Pay particular attention to:

* Body text
* Buttons
* Placeholder text
* Disabled-looking controls
* Form borders
* Focus indicators
* Video overlays
* Chart labels
* Error messages
* Success messages

---

# 22. Typography

The design system should support:

* Text enlargement
* Increased line spacing
* Increased letter spacing
* Responsive wrapping
* Longer translated text
* Large mathematical expressions

Avoid fixed-height text containers that clip content.

---

# 23. Zoom and Text Scaling

AILPG must remain usable when users enlarge content.

The responsive system from:

`13_Responsive_Design.md`

must work together with accessibility requirements.

Expected behavior:

```text
Normal
   ↓
200% zoom
   ↓
Content remains readable
   ↓
No critical content disappears
   ↓
No horizontal scrolling for ordinary text
```

Exceptions may exist for naturally wide content such as complex data tables or mathematical expressions, but an accessible alternative should be provided where practical.

---

# 24. Reflow

At narrow widths:

```text
Desktop
┌───────────────┬──────────────┐
│ Navigation    │ Content      │
└───────────────┴──────────────┘
```

may become:

```text
Mobile
┌─────────────────────────┐
│ Header                  │
├─────────────────────────┤
│ Content                 │
├─────────────────────────┤
│ Navigation / Drawer     │
└─────────────────────────┘
```

Accessibility must not depend on a particular screen orientation.

---

# 25. Touch Accessibility

Interactive controls should have sufficiently large touch targets.

Important controls include:

* Play
* Pause
* Seek
* Volume
* Quality
* Translation
* Settings
* Question options
* Submit
* Next
* Previous
* Navigation
* Close

Avoid tightly packed controls that are difficult to activate accurately.

---

# 26. Video Player Accessibility

The AILPG video player is one of the most important accessibility surfaces.

Required controls:

```text
Play / Pause
Progress
Current Time
Duration
Volume
Mute
Captions
Transcript
Quality
Playback Speed
Fullscreen
Settings
```

Every control requires:

* Accessible name
* Keyboard support
* Visible focus
* Predictable behavior
* Screen-reader state

---

# 27. Video Captions

Where speech exists, captions should be provided.

Captions should identify meaningful audio information where necessary.

Example:

```text
Teacher:
"First, subtract 4 from both sides."

[writing sound]
```

AI-generated captions should pass through the AI review workflow when accuracy is important.

---

# 28. Transcript Accessibility

The transcript should be:

* Keyboard accessible
* Searchable
* Selectable
* Readable by screen readers
* Synchronized with video
* Available independently from the video

Example:

```text
00:00
Today we will solve a linear equation.

00:08
First, move the constant to the other side.

00:15
Now divide both sides by 2.
```

Selecting a transcript segment may move the video to that timestamp.

---

# 29. Audio Description

Where visual information is essential and is not adequately conveyed through narration, the platform should support an appropriate description mechanism.

For mathematical lessons, examples include:

```text
"The equation changes from 2x + 4 = 10
to 2x = 6."
```

This can be especially important when the teacher's spoken explanation does not describe changes visible only on screen.

---

# 30. Interactive Question Accessibility

Every question must be usable independently of pointer input.

Example:

```text
Question 3 of 8

What is x?

○ 2
○ 3
○ 4
○ 5

[Submit Answer]
```

For multiple-choice questions, use appropriate semantic grouping such as:

```html
<fieldset>
  <legend>What is x?</legend>
  ...
</fieldset>
```

---

# 31. Radio Questions

Radio buttons should support:

```text
Tab
Arrow keys
Space
```

The question should clearly expose:

* Question text
* Number
* Available answers
* Selected answer
* Required state

---

# 32. Checkbox Questions

For multiple-answer questions:

```text
Select all correct answers.

☐ 2
☐ 4
☐ 6
☐ 8
```

The UI must clearly communicate that multiple answers may be selected.

---

# 33. Short Answer Questions

Example:

```text
Enter the value of x.

[___________]

[Submit Answer]
```

The input must have:

* Label
* Instructions
* Validation
* Error message
* Correct/incorrect feedback

---

# 34. Numerical Answer Accessibility

Numerical input should not unnecessarily require complex interaction.

Where appropriate:

```text
Answer
[ 12.5 ]
```

instead of forcing users to use an inaccessible custom keypad.

If a custom math keypad is provided, every key must have an accessible name.

---

# 35. Equation Input Accessibility

Math input is a special accessibility requirement.

The platform should support accessible representations such as:

```text
Visual equation
      +
Accessible mathematical representation
      +
Optional spoken description
```

Example:

```text
Visual:
2x + 4 = 10

Accessible text:
Two x plus four equals ten.
```

---

# 36. Mathematical Content

AILPG must avoid relying exclusively on mathematical images.

Bad:

```text
[image of equation]
```

Preferred:

```text
Visual Equation
      ↓
Semantic Math Representation
      ↓
Screen Reader Interpretation
```

Where technically feasible, MathML or another semantically meaningful math representation should be used.

LaTeX may be retained as a source representation, but should not be the only accessibility layer.

---

# 37. Complex Equations

Long equations should not be clipped.

Example:

```text
┌───────────────────────────────────────────────┐
│ x = (−b ± √(b² − 4ac)) / 2a                 │
└───────────────────────────────────────────────┘
```

For complex mathematics, provide:

* Visual rendering
* Semantic representation
* Text/speech description where useful
* Accessible navigation through complex expressions

---

# 38. AI-Generated Mathematics

AI-generated mathematical content must be reviewed for both:

1. Mathematical correctness
2. Accessibility correctness

Reviewers should be able to inspect:

```text
Original video
      ↓
OCR
      ↓
Detected equation
      ↓
Normalized equation
      ↓
Accessible representation
```

---

# 39. OCR Accessibility

OCR output should not be exposed only as an image.

Example:

```text
Video frame
    ↓
OCR
    ↓
Recognized text
    ↓
Accessible HTML text
```

OCR confidence should not be communicated through color alone.

Example:

```text
Low confidence

⚠ Review required
```

---

# 40. AI Review UI Accessibility

The AI Review interface must support:

* Keyboard navigation
* Screen readers
* Focus management
* Accessible tabs
* Accessible dialogs
* Accessible diff views
* Accessible status indicators
* Accessible comments
* Accessible review controls

Reviewers must be able to complete the core workflow without a mouse.

---

# 41. Review Timeline Accessibility

Visual timeline markers should have accessible equivalents.

Visual:

```text
0:00 ───●──────●────────●──── 10:00
        Q1     Q2       Q3
```

Accessible representation:

```text
Question 1 — 02:15
Question 2 — 05:32
Question 3 — 08:04
```

Selecting the accessible item should move the player to the associated timestamp.

---

# 42. Course Builder Accessibility

The Course Builder contains potentially complex drag-and-drop interactions.

Drag-and-drop must never be the only method.

Instead of:

```text
Drag Lesson 3 above Lesson 2
```

also provide:

```text
Move lesson up
Move lesson down
Move to module
Change position
```

Keyboard users must be able to reorder content.

---

# 43. Accessible Tree Navigation

The course hierarchy should communicate:

```text
Course
 ├── Module 1
 │    ├── Lesson 1
 │    ├── Lesson 2
 │
 └── Module 2
      ├── Lesson 3
      └── Lesson 4
```

Assistive technologies should receive:

* Node name
* Node type
* Expanded/collapsed state
* Position
* Parent relationship

---

# 44. Video Upload Accessibility

The upload interface must support:

* Keyboard file selection
* Drag-and-drop as an optional method
* File picker
* Upload progress
* Processing status
* Validation errors
* Retry
* Cancellation

Example:

```text
Upload video

[Choose MP4 file]

or

Drop video here

Status:
Uploading — 64%

64% complete
```

Progress must not depend exclusively on animation or color.

---

# 45. Upload Status Announcements

Important states should be communicated accessibly.

```text
Upload started.

Upload 50 percent complete.

Upload completed.

Video validation failed.

Processing started.

Processing completed.

Interactive lesson generated.
```

---

# 46. Analytics Accessibility

Analytics must not depend solely on visual charts.

Example:

```text
Visual Chart
     +
Accessible Summary
     +
Data Table
```

---

# 47. Chart Alternative

Instead of exposing only:

```text
[visual line chart]
```

provide:

```text
Video completion rate

Week 1: 64%
Week 2: 68%
Week 3: 72%
Week 4: 75%
```

Complex charts should have meaningful textual summaries.

---

# 48. Accessible Data Tables

Tables should provide:

* Header associations
* Caption where appropriate
* Sort state
* Filter state
* Row/column relationships

Example:

```text
Question | Attempts | Correct | Accuracy
-----------------------------------------
Q1       | 120      | 94      | 78%
Q2       | 118      | 61      | 52%
```

---

# 49. Accessible Filters

Analytics and administration filters should expose:

```text
Filter
Date range
Course
Module
Lesson
Student group
Language
Question type
```

Each filter requires a proper accessible label.

---

# 50. Notifications

Notifications should be categorized and clearly announced.

Examples:

```text
Success:
Lesson published successfully.

Warning:
Three AI-generated questions require review.

Error:
Video processing failed.

Information:
Translation is still processing.
```

Do not rely on color alone.

---

# 51. Toast Notifications

Transient notifications should not disappear before users can reasonably perceive them.

Important errors should remain available until dismissed or otherwise accessible from the page.

---

# 52. Dialog Accessibility

Dialogs must include:

* Accessible name
* Description where needed
* Focus trapping where appropriate
* Escape behavior where appropriate
* Correct focus restoration
* Keyboard operation

Example:

```text
┌──────────────────────────────┐
│ Delete video                 │
│                              │
│ Are you sure?                │
│                              │
│ [Cancel] [Delete]            │
└──────────────────────────────┘
```

---

# 53. Drawer Accessibility

Side drawers must behave like meaningful interactive regions.

Example:

```text
Student
   ↓
Settings
   ↓
Drawer opens
   ↓
Focus enters drawer
   ↓
Close
   ↓
Focus returns to Settings
```

---

# 54. Tooltip Accessibility

Tooltips should not contain essential information that is unavailable elsewhere.

For icon-only buttons:

```text
Settings
```

should be available through an accessible name, not only a hover tooltip.

---

# 55. Reduced Motion

AILPG should respect the user's reduced-motion preference.

Where supported:

```css
@media (prefers-reduced-motion: reduce) {
  /* Reduce non-essential animation */
}
```

Avoid unnecessary:

* Parallax
* Rapid transitions
* Animated backgrounds
* Excessive bouncing
* Continuous movement

---

# 56. Video Motion

Users should be able to control video playback normally.

The platform should not automatically introduce additional motion effects around the player that could interfere with learning or accessibility.

---

# 57. Audio Preferences

Provide appropriate controls for:

* Volume
* Mute
* Playback speed
* Captions
* Transcript
* Audio track where supported

---

# 58. Cognitive Accessibility

AILPG should minimize unnecessary cognitive load.

Use:

* Clear instructions
* Consistent controls
* Predictable layouts
* Short labels
* Simple error messages
* Clear progress indicators
* Step-by-step workflows
* Consistent terminology

Avoid unnecessary technical terminology in student-facing interfaces.

---

# 59. Question Cognitive Load

Interactive questions should clearly communicate:

```text
What is being asked?
What input is expected?
Is an answer required?
How do I submit?
What happened after submission?
What can I do next?
```

---

# 60. Timing Requirements

Timed questions must provide an accessible experience.

Where timing is pedagogically required, consider:

* Clear timer announcement
* Remaining-time visibility
* Appropriate extension/accommodation mechanisms
* Warning before expiration
* Non-color timer indication

The application should not silently submit or discard work.

---

# 61. Language Accessibility

Every generated lesson should identify its language programmatically.

Example:

```html
<html lang="en">
```

For Tamil:

```html
<html lang="ta">
```

For mixed-language content, language changes should be marked appropriately where practical.

---

# 62. Translation Accessibility

Translated content should preserve:

* Meaning
* Mathematical notation
* Reading order
* Accessible labels
* Question instructions
* Feedback
* Captions
* Transcript structure

Translation must not remove accessibility metadata.

---

# 63. RTL Support

If right-to-left languages are supported, the interface should support appropriate text direction.

Example:

```text
LTR:
English
Tamil

RTL:
Arabic
Hebrew
```

Mathematical notation requires special testing because equations may contain directional symbols even inside RTL content.

---

# 64. Student Player Accessibility Architecture

```text
Student Player
│
├── Accessible Video Controls
│
├── Captions
│
├── Transcript
│
├── Accessible Math
│
├── Question Overlay
│
├── Keyboard Navigation
│
├── Screen Reader Announcements
│
├── Translation
│
├── Quality
│
└── Accessibility Preferences
```

---

# 65. Accessibility Preferences

The application may provide a dedicated preferences section.

Example:

```text
Accessibility

☐ Reduce motion

☐ Prefer larger text

☐ High contrast mode

☐ Always show captions

☐ Open transcript automatically

Playback speed
[ 1.0x ]

Text size
[ Normal ▼ ]
```

Browser and operating-system preferences should be respected where possible.

---

# 66. Accessibility Data Model

Accessibility preferences should be stored separately from learning content.

Example:

```text
UserAccessibilityPreferences

id
user_id
reduced_motion
captions_enabled
transcript_enabled
text_scale
contrast_preference
preferred_language
created_at
updated_at
```

Not every preference needs to be persisted if it can safely follow browser/device settings.

---

# 67. Accessibility API Considerations

Example:

```http
GET /api/users/me/accessibility-preferences
```

```http
PATCH /api/users/me/accessibility-preferences
```

Example payload:

```json
{
  "reducedMotion": true,
  "captionsEnabled": true,
  "transcriptEnabled": true,
  "textScale": "large"
}
```

Server-side validation is required.

---

# 68. Accessibility Metadata for Lessons

Generated lesson content should be capable of storing:

```text
caption availability
transcript availability
audio description availability
language
math accessibility representation
image alternative text
content warnings where appropriate
```

Example:

```json
{
  "language": "en",
  "captionsAvailable": true,
  "transcriptAvailable": true,
  "mathAccessible": true
}
```

---

# 69. Accessibility and Security

Accessibility must not weaken security.

Examples:

* Accessible error messages must not reveal sensitive information.
* Screen-reader-visible content must respect permissions.
* Hidden correct answers must remain protected.
* Accessible transcript content must follow lesson access permissions.
* Admin-only information must not be exposed through accessibility trees to unauthorized users.

---

# 70. Accessibility and Privacy

Analytics should avoid exposing unnecessary personal information.

For example:

```text
Accessible analytics:
"42 students completed the lesson."

Avoid unnecessary:
"Student John Smith completed the lesson at 10:32 PM from IP ..."
```

Accessibility does not require exposing more personal information than the user is authorized to access.

---

# 71. Accessibility and Performance

Accessibility features should not create unnecessary performance problems.

Optimize:

* Screen-reader DOM complexity
* Large transcript rendering
* Math rendering
* Analytics tables
* AI review pages
* Large course trees

Virtualized content must still provide an accessible experience.

---

# 72. Accessible Loading States

Loading states should communicate meaningful progress.

Bad:

```text
[spinner]
```

Better:

```text
Generating interactive questions…

Step 3 of 5
Question generation
```

Where exact progress is unavailable:

```text
Generating your lesson…
```

---

# 73. Accessible Empty States

Example:

```text
No lessons found.

Create a course or upload a solved-math video
to generate your first interactive lesson.
```

Empty states should explain what happened and what action can be taken.

---

# 74. Accessible Error States

Example:

```text
Video processing failed.

The uploaded file could not be processed.

[Retry processing]

Error reference: VID-2048
```

Avoid technical-only messages such as:

```text
FFmpeg error 127
```

unless shown in an appropriate technical/admin context.

---

# 75. Accessibility Component Standards

Every reusable component should define:

```text
Component
├── Semantic element
├── Accessible name
├── Keyboard behavior
├── Focus behavior
├── Screen-reader behavior
├── Error behavior
├── Loading behavior
├── Disabled behavior
├── Responsive behavior
└── Reduced-motion behavior
```

---

# 76. Example Component Specification

### Accessible Question Card

```text
QuestionCard

Semantic:
<section>

Heading:
Question 3 of 8

Inputs:
fieldset + legend

Actions:
button

Feedback:
aria-live region

Focus:
Move to feedback after submission

Keyboard:
Full keyboard support
```

---

# 77. Accessibility Testing Strategy

Accessibility testing should use multiple methods.

```text
Automated Testing
       +
Keyboard Testing
       +
Screen Reader Testing
       +
Zoom Testing
       +
Contrast Testing
       +
Mobile Testing
       +
Manual UX Testing
       +
User Testing
```

No single testing method is sufficient.

---

# 78. Automated Testing

Integrate automated accessibility checks into CI/CD.

Potential checks include:

* Missing labels
* Invalid ARIA
* Heading hierarchy problems
* Contrast issues
* Missing accessible names
* Form association issues
* Landmark problems

Automated testing should be treated as an early detection mechanism, not complete accessibility validation.

---

# 79. Keyboard Test Checklist

For each major page:

```text
[ ] Can page be reached with keyboard?
[ ] Can every button be focused?
[ ] Can every link be focused?
[ ] Can every form field be reached?
[ ] Is focus visible?
[ ] Is focus order logical?
[ ] Can dialogs be operated?
[ ] Can dialogs be closed?
[ ] Are there keyboard traps?
[ ] Can video controls be operated?
[ ] Can questions be answered?
[ ] Can course items be reordered?
[ ] Can upload workflow be completed?
```

---

# 80. Screen Reader Test Checklist

```text
[ ] Page title is meaningful
[ ] H1 exists
[ ] Landmarks are meaningful
[ ] Navigation is understandable
[ ] Buttons have accessible names
[ ] Form fields have labels
[ ] Errors are announced
[ ] Dynamic state changes are announced
[ ] Video controls are understandable
[ ] Captions can be enabled
[ ] Transcript is readable
[ ] Questions are understandable
[ ] Feedback is announced
[ ] Mathematical content has accessible representation
```

---

# 81. Visual Accessibility Checklist

```text
[ ] Text has adequate contrast
[ ] Focus is visible
[ ] Color is not the only indicator
[ ] Text can enlarge
[ ] Content reflows
[ ] Important controls are visible
[ ] Error states are clear
[ ] Charts have alternatives
[ ] Icons have labels where required
```

---

# 82. Mobile Accessibility Testing

Test on:

```text
Small phone
Large phone
Tablet
Desktop
Touch device
Keyboard-connected device
```

Verify:

* Touch targets
* Screen reader gestures
* Orientation
* Zoom
* Virtual keyboard
* Video controls
* Question interaction
* Dialog behavior

---

# 83. Assistive Technology Testing

The QA strategy should include representative assistive technologies, such as:

```text
Screen readers
Keyboard-only navigation
Browser accessibility tools
OS accessibility settings
Mobile screen readers
Magnification / zoom
High-contrast configurations
Reduced-motion settings
```

Exact supported combinations should be maintained in the project's QA matrix.

---

# 84. Accessibility CI/CD

Recommended pipeline:

```text
Developer
   ↓
Component Accessibility Test
   ↓
Automated Accessibility Scan
   ↓
Unit Tests
   ↓
Integration Tests
   ↓
Keyboard QA
   ↓
Screen Reader QA
   ↓
Visual Regression
   ↓
Release
```

---

# 85. Accessibility Regression Testing

Every major UI change should be evaluated for regressions.

High-risk areas:

* Video player
* Question overlay
* Course Builder
* AI Review
* Dialog system
* Navigation
* Authentication
* Analytics
* Math rendering

---

# 86. Accessibility Acceptance Criteria

A feature is accessibility-complete when:

```text
[ ] Keyboard accessible
[ ] Screen-reader accessible
[ ] Proper semantic structure
[ ] Visible focus
[ ] Accessible names
[ ] Accessible errors
[ ] Accessible dynamic states
[ ] Responsive at
```
