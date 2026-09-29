# AILPG — Interactive Question UI/UX Blueprint

**Document:** `docs/04_UI_UX_BLUEPRINT/08_Interactive_Question_UI.md`
**Project:** MP4 → Interactive Learning Platform Generator (AILPG)
**Document Type:** UI/UX Blueprint
**Version:** 1.0
**Status:** Draft / Implementation Reference

---

# 1. Purpose

The Interactive Question UI is the core learning-interaction layer of AILPG.

It converts passive video watching into an active learning experience by automatically inserting questions at meaningful points in the generated lesson.

The system must support:

* AI-generated questions
* Instructor-created questions
* Multiple-choice questions
* Multiple-answer questions
* True/false questions
* Short-answer questions
* Numerical questions
* Equation-based questions
* Fill-in-the-blank questions
* Matching questions
* Ordering questions
* Hint systems
* Feedback
* Retry
* Scoring
* Progress tracking
* Accessibility
* Multilingual questions

---

# 2. Learning Concept

Traditional video:

```text
Watch
 ↓
Watch
 ↓
Watch
 ↓
Finish
```

AILPG:

```text
Watch
 ↓
Understand
 ↓
Question
 ↓
Answer
 ↓
Feedback
 ↓
Continue
 ↓
Question
 ↓
Answer
 ↓
Complete
```

The objective is to encourage active recall and continuous engagement with the lesson.

---

# 3. Question Lifecycle

```text
AI / Instructor Creates Question
            ↓
Question Stored
            ↓
Question Reviewed
            ↓
Question Approved
            ↓
Question Assigned Timestamp
            ↓
Video Reaches Timestamp
            ↓
Question Triggered
            ↓
Student Answers
            ↓
Answer Evaluated
            ↓
Feedback
            ↓
Progress Updated
            ↓
Analytics Recorded
```

---

# 4. Question UI States

Every question should support explicit states:

```text
Scheduled
Triggered
Displayed
Answering
Submitting
Correct
Incorrect
Retry
Completed
Skipped
Expired
Error
```

---

# 5. Standard Question Layout

Desktop:

```text
┌────────────────────────────────────────────────┐
│                QUICK CHECK                     │
│                                                │
│  What is the value of x?                       │
│                                                │
│  ○ 2                                           │
│  ○ 3                                           │
│  ○ 4                                           │
│  ○ 5                                           │
│                                                │
│  Question 2 of 8                               │
│                                                │
│                 [Submit Answer]                │
└────────────────────────────────────────────────┘
```

---

# 6. Question Header

The header should contain:

```text
Quick Check
Question 2 of 8
Topic: Linear Equations
```

Optional:

```text
Difficulty: Easy
Points: 1
```

Avoid showing unnecessary AI metadata to students.

---

# 7. Question Body

The question body should support:

```text
Text
Images
Diagrams
Mathematical Expressions
LaTeX
Tables
Audio
Video Snippets
```

Example:

```text
Solve:

2x + 4 = 10
```

Mathematical content must remain visually readable on desktop and mobile.

---

# 8. Multiple Choice Question

Example:

```text
What is x?

○ 2
○ 3
○ 4
○ 5
```

Only one option can be selected.

Interaction:

```text
Student selects option
       ↓
Option becomes selected
       ↓
Submit button enabled
```

---

# 9. Multiple Answer Question

Example:

```text
Which statements are correct?

☐ x = 2
☐ x > 0
☐ x = 5
☐ x + 2 = 4
```

The UI must clearly indicate:

```text
Select all that apply.
```

---

# 10. True / False

Example:

```text
Is the following statement true?

2 + 2 = 4

○ True
○ False
```

Use clear controls with sufficient touch area.

---

# 11. Short Answer

Example:

```text
What is the value of x?

[________________]

[Submit Answer]
```

The backend should normalize acceptable responses where appropriate.

For example:

```text
3
3.0
03
```

may represent equivalent numerical answers depending on question configuration.

---

# 12. Numerical Answer

Example:

```text
Calculate:

25 × 4 = ?

[____________]

Answer format:
Enter a number.
```

Question metadata can define:

```text
Expected Value
Tolerance
Unit
Decimal Precision
```

Example:

```text
Expected:
10

Tolerance:
±0.01
```

---

# 13. Equation Input

For mathematical lessons, equation input may be supported.

Example:

```text
Solve:

x + 4 = 10

Your answer:

[ x = __________ ]
```

The interface should provide an optional math keyboard.

---

# 14. Math Keyboard

Possible controls:

```text
+   −   ×   ÷
=   ≠   <   >
√   ²   ³
(   )
x   y   z
π   %
```

Advanced symbols may include:

```text
Fraction
Exponent
Subscript
Integral
Summation
Matrix
Greek Letters
```

---

# 15. Fill in the Blank

Example:

```text
The solution to:

2x = 10

is x = _____.
```

Input:

```text
[ 5 ]
```

The blank can be embedded inside mathematical content.

---

# 16. Matching Question

Example:

```text
Match the equation with the answer.

2 + 2       →   4
3 + 5       →   8
10 − 3      →   7
```

Mobile implementation should use a simpler interaction pattern such as:

```text
Question:
2 + 2

Answer:
[ Select ]
```

---

# 17. Ordering Question

Example:

```text
Arrange the steps in the correct order.

☰ Move 4 to the other side
☰ Divide by 2
☰ Write the original equation
```

Students can drag and reorder items.

Keyboard-accessible alternatives must also be available.

---

# 18. Image-Based Question

The question may include diagrams.

Example:

```text
        /\
       /  \
      /    \
     /______\

What is the area of the triangle?
```

The image should support:

```text
Zoom
Alt Text
Fullscreen
```

---

# 19. Question Overlay Behavior

When triggered during video:

```text
Video Playing
     ↓
Trigger Timestamp
     ↓
Video Pauses
     ↓
Overlay Opens
     ↓
Question Active
```

The player should preserve the timestamp at which the question appeared.

---

# 20. Question Animation

Question appearance should be subtle.

Recommended:

```text
Fade
Short slide
Scale-in
```

Animation duration should remain short.

For reduced-motion users:

```text
No animation
```

---

# 21. Question Progress

Display progress:

```text
Question 3 of 8
```

and optionally:

```text
██████████░░░░░░ 60%
```

Students should understand how much of the interactive portion remains.

---

# 22. Required Questions

Some questions are mandatory.

Example:

```text
Required activity
```

When required:

```text
Submit Answer
      ↓
Feedback
      ↓
Continue
```

The student cannot simply close the question without completing the required action, subject to lesson configuration.

---

# 23. Optional Questions

Optional questions may provide:

```text
[Answer]
[Skip]
```

Skipping should be recorded separately from answering.

Example:

```text
Question Status:
Skipped
```

---

# 24. Submit Button

Default state:

```text
[Submit Answer]
```

Disabled when:

```text
No answer selected
```

Loading:

```text
[Submitting...]
```

Success:

```text
[Submitted]
```

---

# 25. Correct Answer State

Example:

```text
┌───────────────────────────────────┐
│ ✓ Correct                          │
│                                   │
│ x = 3                             │
│                                   │
│ You correctly solved the equation.│
│                                   │
│              [Continue]            │
└───────────────────────────────────┘
```

The interface should provide positive but concise feedback.

---

# 26. Incorrect Answer State

Example:

```text
┌───────────────────────────────────┐
│ Not quite                         │
│                                   │
│ The correct answer is 3.          │
│                                   │
│ Explanation:                      │
│ First subtract 4 from both sides. │
│ Then divide by 2.                 │
│                                   │
│ [Try Again]   [Continue]         │
└───────────────────────────────────┘
```

Whether the correct answer is immediately revealed depends on question configuration.

---

# 27. Hint System

Questions may provide hints.

Example:

```text
Need help?

[Show Hint]
```

After activation:

```text
Hint 1:
Try isolating x.

[Show Another Hint]
```

Question metadata:

```text
Maximum Hints
Hint Penalty
Hint Content
```

---

# 28. Explanation System

An explanation may appear after submission.

Example:

```text
Solution

2x + 4 = 10

2x = 10 − 4

2x = 6

x = 3
```

Mathematical expressions should use consistent math rendering.

---

# 29. Retry Logic

Possible configurations:

```text
Unlimited
2 Attempts
3 Attempts
Single Attempt
```

Display:

```text
Attempts remaining: 2
```

Avoid exposing unnecessary scoring mechanics before the student answers.

---

# 30. Partial Credit

For multiple-answer questions, the system may support partial credit.

Example:

```text
2 of 3 correct
```

Question configuration:

```text
partial_credit = true
```

Scoring rules must be defined by the backend.

---

# 31. Question Timer

Optional timer:

```text
Time remaining: 00:30
```

Possible modes:

```text
No Timer
Recommended Time
Hard Time Limit
```

If the timer expires:

```text
Time's up.

[View Result]
```

Timed questions should be used only when appropriate for the lesson.

---

# 32. Question Difficulty

Internal metadata may include:

```text
Easy
Medium
Hard
```

The student-facing UI may optionally display difficulty.

Example:

```text
Difficulty: Medium
```

---

# 33. Question Categories

Questions may be classified by learning purpose:

```text
Recall
Understanding
Application
Calculation
Problem Solving
Concept Check
Practice
Review
```

This metadata can be used by analytics and lesson design.

---

# 34. Question Timestamp Metadata

Example:

```json
{
  "questionId": "Q1023",
  "videoId": "VID2048",
  "triggerAt": 152,
  "required": true
}
```

The trigger must be deterministic.

---

# 35. Question Trigger Rules

Questions can be triggered based on:

```text
Exact Timestamp
Video Chapter
Concept Boundary
Instructor Marker
AI-Detected Learning Point
```

Example:

```text
AI detects:
"First equation transformation completed"

Question:
What should we do next?
```

---

# 36. Question Density

The platform should avoid excessive questions.

Recommended configurable settings:

```text
Questions per Video
Minimum Time Between Questions
Maximum Questions
Question Difficulty Distribution
```

Example:

```text
Video Duration: 10 minutes

Questions:
5

Minimum Gap:
90 seconds
```

These are configurable defaults rather than fixed platform rules.

---

# 37. Question Randomization

For suitable question types, options may be randomized.

Example:

Original:

```text
2
3
4
5
```

Session:

```text
4
2
5
3
```

The correct answer must remain logically mapped to its option.

---

# 38. Question Bank

A lesson may contain a question bank.

```text
Question Bank
─────────────

Q001
Q002
Q003
Q004
Q005
```

The lesson may select:

```text
Fixed Questions
Random Questions
Adaptive Questions
```

---

# 39. Adaptive Question Selection

Future versions may select questions based on student performance.

Example:

```text
Student answers correctly
        ↓
Next question:
Medium difficulty

Student struggles
        ↓
Next question:
Foundational question
```

Adaptive logic must be configurable and explainable.

---

# 40. Language Support

Questions should support multiple languages.

Example:

```text
English:
What is the value of x?

Tamil:
x இன் மதிப்பு என்ன?

Hindi:
x का मान क्या है?
```

Answer choices should also be translated where appropriate.

---

# 41. Language Switching During Question

If supported:

```text
Question Language

English ✓
Tamil
Hindi
```

Switching languages should preserve:

```text
Selected Answer
Question State
Current Attempt
```

where technically appropriate.

---

# 42. Accessibility

Question interfaces must support:

* Keyboard navigation
* Screen readers
* Focus management
* Labels
* Error messages
* Accessible radio buttons
* Accessible checkboxes
* Accessible drag/drop alternatives
* High contrast
* Reduced motion
* Text resizing

---

# 43. Focus Management

When a question opens:

```text
Video Paused
     ↓
Question Opens
     ↓
Keyboard Focus → Question Heading / First Input
```

After completion:

```text
Continue
     ↓
Focus returns to appropriate player control
```

Focus must never become lost behind the overlay.

---

# 44. Mobile Question UI

Example:

```text
┌─────────────────────────────┐
│ Question 2 of 8             │
├─────────────────────────────┤
│                             │
│ What is x?                  │
│                             │
│ ○ 2                         │
│                             │
│ ○ 3                         │
│                             │
│ ○ 4                         │
│                             │
│ ○ 5                         │
│                             │
│ [ Submit Answer ]           │
└─────────────────────────────┘
```

Buttons should be easy to tap.

---

# 45. Tablet Question UI

On tablets:

```text
┌────────────────────────────────┐
│              VIDEO              │
│                                │
├────────────────────────────────┤
│           QUESTION             │
│                                │
│ What is the answer?            │
│                                │
│ ○ A     ○ B     ○ C     ○ D   │
│                                │
│         [Submit]               │
└────────────────────────────────┘
```

---

# 46. Desktop Side-Panel Mode

For large screens, an alternative layout may be:

```text
┌──────────────────────────┬──────────────────────┐
│                          │                      │
│                          │      QUESTION        │
│          VIDEO           │                      │
│                          │      ○ Answer A      │
│                          │      ○ Answer B      │
│                          │      ○ Answer C      │
│                          │                      │
└──────────────────────────┴──────────────────────┘
```

This allows the video context to remain visible.

---

# 47. Question Card Variants

Supported variants:

```text
Standard Card
Full-screen Card
Side Panel
Bottom Sheet
Inline Activity
```

The lesson configuration can determine the preferred presentation.

---

# 48. Error State

If answer submission fails:

```text
Unable to submit your answer.

Your response has not been recorded.

[Try Again]
```

The selected answer should remain visible where possible.

---

# 49. Duplicate Submission Protection

The frontend should prevent accidental repeated submissions.

```text
Student clicks Submit
       ↓
Button disabled
       ↓
Request sent
       ↓
Backend processes
       ↓
Result returned
```

The backend should use idempotency or equivalent safeguards where necessary.

---

# 50. Offline / Connection Loss

If the connection is lost:

```text
Connection lost.

Your answer will be submitted
when the connection is restored.
```

Where offline submission is not supported:

```text
Connection required to submit this answer.
```

The UI must clearly distinguish between saved and unsaved responses.

---

# 51. Analytics Events

The question UI should record:

```text
question_viewed
question_started
answer_selected
answer_submitted
answer_correct
answer_incorrect
hint_opened
explanation_opened
retry_started
question_skipped
question_completed
question_timeout
```

Example event:

```json
{
  "event": "question_answered",
  "question_id": "Q1023",
  "lesson_id": "LES1002",
  "attempt": 1,
  "time_spent_seconds": 18,
  "is_correct": true
}
```

---

# 52. Privacy

Only required learning data should be collected.

Avoid collecting unnecessary information such as:

```text
Unnecessary device identifiers
Unnecessary location information
Unnecessary personal information
```

Analytics collection should follow the platform's privacy requirements.

---

# 53. Question Data Model

Example:

```json
{
  "id": "Q1023",
  "lessonId": "LES1002",
  "type": "mcq",
  "question": "What is the value of x?",
  "options": [
    {
      "id": "A",
      "text": "2"
    },
    {
      "id": "B",
      "text": "3"
    },
    {
      "id": "C",
      "text": "4"
    }
  ],
  "triggerAt": 152,
  "required": true,
  "maxAttempts": 2,
  "hints": [],
  "explanation": "Subtract 4 and divide by 2."
}
```

Correct-answer data should not be exposed unnecessarily to the client.

---

# 54. Backend APIs

Potential endpoints:

```text
GET  /api/lessons/:lessonId/questions
GET  /api/questions/:questionId

POST /api/questions/:questionId/answer
POST /api/questions/:questionId/hint
POST /api/questions/:questionId/skip

GET  /api/questions/:questionId/result
```

Exact contracts belong in the API Design documentation.

---

# 55. Scoring Architecture

```text
Student Answer
      ↓
Frontend Validation
      ↓
Backend Validation
      ↓
Answer Evaluation
      ↓
Score Calculation
      ↓
Feedback Selection
      ↓
Progress Update
      ↓
Analytics Event
```

The backend should be authoritative for scoring.

---

# 56. Question Review UI Integration

AI-generated questions should pass through:

```text
AI Generated
      ↓
AI Review
      ↓
Human Review
      ↓
Approved
      ↓
Published
```

The student UI only displays approved lesson content.

---

# 57. Question Editing

Authorized content managers should be able to edit:

```text
Question Text
Options
Correct Answer
Explanation
Hint
Difficulty
Timestamp
Question Type
Required Status
Attempt Limit
Score
Language
```

Changes should create a new content version where versioning is enabled.

---

# 58. Question Validation

Before publication, the system should validate:

```text
Question text exists
Options are complete
Correct answer exists
Timestamp is valid
Question type is supported
Translation is available where required
Explanation is valid
Scoring configuration is valid
```

Invalid questions must not silently reach production.

---

# 59. Question Preview

Admin preview:

```text
QUESTION PREVIEW

Video Timestamp:
04:32

Question:
What is x?

○ 2
○ 3
○ 4
○ 5

[Preview Student Experience]
```

Preview should behave like the real student interaction without changing production data.

---

# 60. Versioning

Question versions:

```text
Q1023 v1
AI Generated

Q1023 v2
Reviewer Edited

Q1023 v3
Translation Updated

Q1023 v4
Published
```

Administrators should be able to inspect changes.

---

# 61. Security

The question UI must not expose:

```text
Correct answer before submission
Internal AI prompts
Private reviewer notes
Administrative metadata
Sensitive user information
```

Authorization must be enforced server-side.

---

# 62. Performance

The question system should:

* Load questions efficiently.
* Preload upcoming question metadata where appropriate.
* Avoid blocking video playback unnecessarily.
* Cache static question assets.
* Submit analytics asynchronously.
* Prevent duplicate requests.
* Preserve answer state during transient network failures.

---

# 63. Component Architecture

```text
InteractiveQuestion
│
├── QuestionHeader
│   ├── QuestionNumber
│   ├── Progress
│   └── Difficulty
│
├── QuestionContent
│   ├── Text
│   ├── Image
│   ├── MathRenderer
│   └── Media
│
├── AnswerArea
│   ├── MCQ
│   ├── MultiSelect
│   ├── TrueFalse
│   ├── ShortAnswer
│   ├── NumericInput
│   ├── EquationInput
│   ├── Matching
│   └── Ordering
│
├── HintPanel
├── Timer
├── SubmitButton
├── FeedbackPanel
├── ExplanationPanel
└── ContinueButton
```

---

# 64. State Architecture

```text
Question State
│
├── questionData
├── selectedAnswer
├── attemptNumber
├── submissionStatus
├── result
├── feedback
├── hintsUsed
├── timeSpent
└── completionStatus
```

---

# 65. Complete Interaction Flow

```text
VIDEO PLAYING
      ↓
QUESTION TIMESTAMP
      ↓
VIDEO PAUSED
      ↓
QUESTION APPEARS
      ↓
STUDENT READS
      ↓
STUDENT SELECTS ANSWER
      ↓
SUBMIT
      ↓
BACKEND VALIDATES
      ↓
RESULT
   ┌──┴───┐
   ↓      ↓
CORRECT INCORRECT
   ↓      ↓
FEEDBACK RETRY/FEEDBACK
   └──┬───┘
      ↓
QUESTION COMPLETE
      ↓
PROGRESS UPDATE
      ↓
VIDEO RESUMES
```

---

# 66. Acceptance Criteria

The Interactive Question UI is complete when:

* [ ] Questions can be displayed at video timestamps.
* [ ] Video pauses when required questions appear.
* [ ] MCQ works.
* [ ] Multiple-answer works.
* [ ] True/false works.
* [ ] Short-answer works.
* [ ] Numerical answers work.
* [ ] Mathematical input works where enabled.
* [ ] Hints work.
* [ ] Feedback works.
* [ ] Explanations work.
* [ ] Retry rules work.
* [ ] Timers work where configured.
* [ ] Progress updates correctly.
* [ ] Analytics events are recorded.
* [ ] Questions support translation.
* [ ] Mobile layout works.
* [ ] Keyboard navigation works.
* [ ] Screen-reader support works.
* [ ] Error recovery works.
* [ ] Duplicate submissions are prevented.
* [ ] Backend authorization is enforced.

---

# 67. Definition of Done

```text
Question Rendering          ✓
Question Triggering         ✓
Answer Input                ✓
Validation                  ✓
Scoring                     ✓
Feedback                    ✓
Hints                       ✓
Retry                       ✓
Math Input                  ✓
Translation                 ✓
Progress Tracking           ✓
Analytics                   ✓
Accessibility               ✓
Responsive UI               ✓
Security                    ✓
Error Recovery              ✓
Testing                     ✓
```

---

# 68. Relationship With Other UI Documents

```text
04_UI_UX_BLUEPRINT/
│
├── 06_Admin_Dashboard.md
│
├── 07_Video_Player.md
│        │
│        └── Interactive Question Trigger
│
├── 08_Interactive_Question_UI.md
│        │
│        ├── Question UI
│        ├── Answer System
│        ├── Feedback
│        └── Progress
│
├── 09_Course_Builder.md
│        │
│        └── Question Configuration
│
├── 10_Video_Upload_UI.md
│
├── 11_AI_Review_UI.md
│        │
│        └── Question Review
│
└── 12_Analytics_UI.md
         │
         └── Question Analytics
```

---

# 69. Final Architecture

```text
                  MP4 VIDEO
                     ↓
               AI ANALYSIS
                     ↓
            QUESTION GENERATION
                     ↓
              QUESTION REVIEW
                     ↓
                 APPROVAL
                     ↓
              LESSON BUILDER
                     ↓
               VIDEO PLAYER
                     ↓
          ┌─────────────────────┐
          │ INTERACTIVE QUESTION│
          └──────────┬──────────┘
                     ↓
                STUDENT ANSWER
                     ↓
              BACKEND EVALUATION
                     ↓
              FEEDBACK / SCORE
                     ↓
             LEARNING PROGRESS
                     ↓
                 ANALYTICS
```

The Interactive Question UI is therefore the bridge between **AI-generated educational content** and **active student learning**.

**Document Status:** Ready for implementation planning.
