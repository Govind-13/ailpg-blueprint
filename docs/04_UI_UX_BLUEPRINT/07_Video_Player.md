# AILPG — Video Player UI/UX Blueprint

**Document:** `docs/04_UI_UX_BLUEPRINT/07_Video_Player.md`
**Project:** MP4 → Interactive Learning Platform Generator (AILPG)
**Document Type:** UI/UX Blueprint
**Version:** 1.0
**Status:** Draft / Implementation Reference

---

# 1. Purpose

The AILPG Video Player is the primary learning interface where students watch AI-processed educational videos and interact with dynamically inserted learning activities.

Unlike a conventional video player, the AILPG player combines:

```text
Video Playback
      +
Interactive Questions
      +
Learning Progress
      +
Transcript
      +
Translation
      +
Quality Selection
      +
Accessibility
      +
Analytics
```

The player must maintain the original educational flow while introducing questions at appropriate points in the lesson.

---

# 2. Core Learning Flow

```text
Student Opens Lesson
        ↓
Video Loads
        ↓
Video Starts
        ↓
Student Watches
        ↓
AI-Generated Question Trigger
        ↓
Video Pauses
        ↓
Question Appears
        ↓
Student Answers
        ↓
Answer Evaluated
        ↓
Feedback Displayed
        ↓
Student Continues
        ↓
Next Video Segment
        ↓
Lesson Completion
```

---

# 3. Player Layout

Desktop layout:

```text
┌─────────────────────────────────────────────────────────────┐
│ Lesson Title                                      Language  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│                                                             │
│                       VIDEO AREA                            │
│                                                             │
│                                                             │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│ ▶  ━━━━━━━━━━━━━━━●━━━━━━━━━━━━━━━━━━  04:32 / 08:42       │
│                                                             │
│ 🔊   ⚙ Quality   CC   Transcript   🌐 Language   ⛶ Fullscreen│
└─────────────────────────────────────────────────────────────┘
```

---

# 4. Player Components

The player consists of:

```text
Video Container
Playback Controls
Progress Bar
Play/Pause
Volume
Seek
Quality Selector
Speed Selector
Subtitle/Caption
Language Selector
Transcript
Fullscreen
Picture-in-Picture
Zoom
Question Overlay
Learning Progress
Error State
Loading State
```

---

# 5. Video Container

The video container is the primary visual area.

Requirements:

* Maintain correct aspect ratio.
* Prevent unintended stretching.
* Support fullscreen.
* Support responsive resizing.
* Display loading state.
* Display playback errors.
* Support multiple resolutions.
* Support captions.

Recommended default aspect ratio:

```text
16:9
```

The player must also support source videos with different aspect ratios without distorting the content.

---

# 6. Play / Pause

Primary control:

```text
▶
```

When playing:

```text
Ⅱ
```

Supported interactions:

* Click/tap control.
* Spacebar on desktop.
* Keyboard-accessible activation.
* Tap video area where appropriate.

The player should preserve the current playback position.

---

# 7. Progress Bar

Example:

```text
00:00 ━━━━━━━━━●━━━━━━━━━━━━━━ 08:42
```

The progress bar represents:

```text
Current Position
Buffered Position
Total Duration
Question Markers
Chapter Markers
```

Example:

```text
━━━━━━━━●━━━━◆━━━━━━━━◆━━━━━━
         Q1           Q2
```

Where:

```text
◆ = Interactive Question
```

Question markers allow students to understand where interactive activities occur without revealing answers.

---

# 8. Seek Behavior

Students can move backward and forward when the lesson configuration allows it.

The system should support:

```text
Click to seek
Drag timeline
Keyboard seeking
10-second backward
10-second forward
```

Example:

```text
← 10 sec
10 sec →
```

---

# 9. Learning Restrictions

Some lessons may restrict forward seeking.

Possible configuration:

```text
Free Seeking
Restricted Seeking
No Forward Seeking
```

If forward seeking is restricted:

```text
Student cannot skip unfinished required sections.
```

Backward navigation may remain available for revision depending on course configuration.

These rules must be controlled by lesson settings and enforced by the backend where required.

---

# 10. Volume Control

Controls:

```text
🔊
```

Features:

* Volume slider
* Mute/unmute
* Keyboard control
* Mobile-friendly control

The player should remember the user's volume preference where appropriate.

---

# 11. Playback Speed

Supported values may include:

```text
0.5×
0.75×
1×
1.25×
1.5×
1.75×
2×
```

Default:

```text
1×
```

Lesson creators may optionally restrict playback speed for specific educational content.

---

# 12. Video Quality

The player should provide a quality selector.

Example:

```text
Quality

Auto
360p
480p
720p
1080p
```

The actual options depend on:

```text
Source Video
Available Encodings
Device
Network
Subscription
Course Permissions
```

Example subscription logic:

```text
Free Plan
→ Available permitted resolutions

Premium Plan
→ Additional high-resolution options
```

The UI must clearly indicate unavailable options rather than silently failing.

---

# 13. Adaptive Quality

When `Auto` is selected, the player may dynamically choose the appropriate stream quality based on:

```text
Network bandwidth
Buffer health
Device capability
Current resolution
Server availability
```

Example:

```text
Auto

Current:
720p

Reason:
Network conditions
```

The player should avoid unnecessary quality switching that causes visual instability.

---

# 14. Captions

Caption controls:

```text
CC
```

Options:

```text
Off
English
Tamil
Hindi
Malayalam
Telugu
...
```

Captions may originate from:

```text
Speech-to-Text
Uploaded Transcript
Human Edited Transcript
Translated Transcript
```

---

# 15. Language Selector

The player should support language switching.

Example:

```text
Language

English ✓
Tamil
Hindi
Malayalam
Telugu
Kannada
```

Changing language should update supported learning content consistently.

Depending on implementation, language changes may affect:

```text
Captions
Transcript
Question Text
Answer Options
Feedback
UI Labels
```

---

# 16. Translation Behavior

When a translated lesson is available:

```text
Original Content
      ↓
Selected Language
      ↓
Translated Learning Experience
```

If a translation is unavailable:

```text
Tamil

Translation unavailable.

Continue in English
```

The system should not display incomplete or misleading translated content as complete.

---

# 17. Transcript Panel

The transcript panel can appear beside or below the player.

Example:

```text
TRANSCRIPT

00:00
Today we will solve a linear equation.

00:24
First, we move the constant to the
other side of the equation.

01:02
Now divide both sides by two.
```

Transcript lines should be synchronized with video timestamps.

---

# 18. Clickable Transcript

Students should be able to click a transcript line.

Example:

```text
00:42  Divide both sides by 2.
```

Clicking it moves playback to:

```text
00:42
```

This provides a navigation mechanism for revision.

---

# 19. Transcript Search

For longer videos, provide:

```text
Search transcript...
```

Example:

```text
Search:
"quadratic"

Results:

02:13  Quadratic equation...
04:31  Solving quadratic equations...
06:12  Quadratic formula...
```

---

# 20. Interactive Question Trigger

Questions are associated with timestamps.

Example:

```text
Question Q001
Trigger:
00:02:35
```

When the playback position reaches the trigger:

```text
Video
  ↓
Pause
  ↓
Question Overlay
```

The trigger system must prevent duplicate question activation.

---

# 21. Question Overlay

Example:

```text
┌──────────────────────────────────────┐
│              QUICK CHECK              │
│                                      │
│ What is the value of x?              │
│                                      │
│ ○ 2                                  │
│ ○ 3                                  │
│ ○ 4                                  │
│ ○ 5                                  │
│                                      │
│             [Submit Answer]          │
└──────────────────────────────────────┘
```

The video remains paused until the required interaction is completed or the lesson rules allow dismissal.

---

# 22. Question Positioning

Questions may appear as:

```text
Center Overlay
Bottom Sheet
Side Panel
Full Learning Card
```

Desktop:

```text
Video
┌───────────────────────┐
│                       │
│       QUESTION        │
│                       │
└───────────────────────┘
```

Mobile:

```text
┌───────────────┐
│    VIDEO      │
├───────────────┤
│ QUESTION      │
│               │
│ ○ Answer A    │
│ ○ Answer B    │
│               │
│ [Submit]      │
└───────────────┘
```

---

# 23. Question Timing

Questions can be triggered using:

```text
Timestamp
Percentage of Video
Chapter Boundary
AI-Detected Concept
Instructor Defined Point
```

Example:

```text
00:45 → Question 1
02:15 → Question 2
04:32 → Question 3
07:20 → Final Question
```

---

# 24. Preventing Accidental Skipping

When a required question is active:

```text
Video Playback:
PAUSED

Question:
ACTIVE

Forward Seek:
RESTRICTED
```

The student must complete the required interaction before continuing.

---

# 25. Answer Feedback

After submission:

```text
✓ Correct

Great work!

You correctly identified x = 3.

[Continue]
```

Incorrect:

```text
✕ Not quite

The correct answer is 3.

Explanation:
Subtract 4 from both sides, then divide
by 2.

[Continue]
```

Feedback content should come from the lesson's configured question data.

---

# 26. Retry Behavior

Question configuration may define:

```text
Unlimited Attempts
Limited Attempts
Single Attempt
```

Example:

```text
Attempts:
2 / 3
```

If retry is available:

```text
[Try Again]
```

If no retry remains:

```text
Correct Answer
Explanation
[Continue]
```

---

# 27. Question Scoring

The player should send answer events to the backend.

Example:

```text
question_id
lesson_id
student_id
attempt_number
selected_answer
is_correct
timestamp
time_spent
```

The backend calculates authoritative scoring.

---

# 28. Learning Progress

Display lesson progress separately from video playback when useful.

Example:

```text
Lesson Progress
████████████░░░░ 72%

7 / 10 Activities Completed
```

This prevents students from confusing:

```text
Video Playback = 72%
```

with:

```text
Learning Completion = 72%
```

---

# 29. Chapter Navigation

Long lessons should support chapters.

Example:

```text
Chapters

00:00 Introduction
01:12 Problem Setup
02:30 First Step
04:05 Simplification
06:12 Final Answer
07:20 Practice
```

Selecting a chapter moves playback to the appropriate timestamp.

---

# 30. Fullscreen

Fullscreen mode should expand the player.

Controls remain available:

```text
Play
Pause
Progress
Volume
Quality
Captions
Language
Settings
Exit Fullscreen
```

Interactive questions must remain functional in fullscreen mode.

---

# 31. Picture-in-Picture

Where browser/device support exists:

```text
Picture-in-Picture
```

The platform should define whether interactive questions are allowed while the video is in PiP.

If a required question is reached:

```text
Video pauses
↓
Student returns to lesson
↓
Question appears
```

---

# 32. Zoom

AILPG supports content zoom where appropriate.

Controls:

```text
−
100%
+
```

Zoom should primarily support:

* Mathematical notation
* Diagrams
* Text overlays
* Accessibility

The system must avoid making the video itself unusably cropped.

---

# 33. Mathematical Content

Because AILPG focuses on solved mathematics, the player should support clear rendering of:

```text
Fractions
Exponents
Square roots
Equations
Matrices
Graphs
Geometry diagrams
Symbols
LaTeX/MathML content
```

Interactive overlays should preserve mathematical formatting.

---

# 34. Mobile Player

Mobile layout:

```text
┌───────────────────────┐
│ Lesson Title          │
├───────────────────────┤
│                       │
│       VIDEO           │
│                       │
├───────────────────────┤
│ ▶  ━━━━━●━━━━ 08:42  │
├───────────────────────┤
│ 🔊  CC  ⚙  ⛶         │
├───────────────────────┤
│ Transcript            │
│ Questions             │
└───────────────────────┘
```

Touch targets should be sufficiently large for reliable interaction.

---

# 35. Tablet Player

Tablet layout may use:

```text
┌───────────────────────────────┐
│            VIDEO              │
├───────────────────────────────┤
│ Controls                      │
├───────────────────────────────┤
│ Transcript / Questions        │
└───────────────────────────────┘
```

Landscape mode should prioritize the video area.

---

# 36. Keyboard Shortcuts

Desktop shortcuts may include:

| Key   | Action             |
| ----- | ------------------ |
| Space | Play/Pause         |
| ←     | Backward           |
| →     | Forward            |
| ↑     | Increase volume    |
| ↓     | Decrease volume    |
| M     | Mute               |
| F     | Fullscreen         |
| C     | Captions           |
| P     | Picture-in-Picture |

Shortcuts must not interfere with focused form fields.

---

# 37. Accessibility

The player must support:

* Keyboard navigation
* Screen readers
* Captions
* Transcript
* Visible focus states
* Accessible control names
* Adjustable text size
* Sufficient contrast
* Reduced motion preferences
* Accessible question overlays

Controls must not rely solely on icons.

Example:

```text
[▶]
aria-label="Play video"
```

---

# 38. Audio Accessibility

The platform should support captions and transcripts for users who cannot rely on audio.

Where available, future versions may support:

```text
Audio Description
Multiple Audio Tracks
```

---

# 39. Reduced Motion

If the user prefers reduced motion:

```text
Disable:
Animated question transitions
Excessive overlay motion
Non-essential visual effects
```

Question appearance should remain clear without animation.

---

# 40. Loading State

Before playback:

```text
┌──────────────────────────────┐
│                              │
│           Loading...         │
│             ◌                │
│                              │
└──────────────────────────────┘
```

The system should display meaningful status when buffering takes longer than expected.

---

# 41. Buffering State

Example:

```text
Buffering...
```

The player should avoid repeatedly flashing loading indicators during normal short buffering events.

---

# 42. Video Error State

Example:

```text
Unable to play this video.

Please check your connection
and try again.

[Retry]
```

Possible error categories:

```text
Network Error
Source Error
Permission Error
Unsupported Format
Expired Stream
Server Error
```

Internal technical details should be logged but not unnecessarily exposed to students.

---

# 43. Subscription Restriction

If a quality level requires a particular subscription:

```text
1080p

Available with Premium access.

[View Plan]
```

The player should continue functioning at the highest resolution available to the student.

The restriction should not block access to the lesson unless the lesson itself requires a subscription.

---

# 44. Offline / Network Recovery

If connectivity is temporarily lost:

```text
Connection interrupted.

Trying to reconnect...
```

The player should preserve:

```text
Current Position
Lesson Progress
Completed Questions
```

Where technically possible, answer submissions should use safe retry/idempotency mechanisms to prevent duplicate records.

---

# 45. Analytics Events

The player should generate learning analytics events such as:

```text
video_started
video_paused
video_resumed
video_seeked
video_completed
quality_changed
language_changed
caption_enabled
transcript_opened
question_triggered
question_answered
question_completed
fullscreen_entered
fullscreen_exited
```

Events should contain only the information necessary for the analytics purpose and comply with platform privacy requirements.

---

# 46. Example Playback Event

```json
{
  "event": "video_started",
  "lesson_id": "LES1029",
  "video_id": "VID2048",
  "position_seconds": 0,
  "timestamp": "2026-09-29T17:10:00Z"
}
```

The backend remains the authoritative source for completion and scoring.

---

# 47. Player State Machine

The player should maintain explicit states.

```text
IDLE
 ↓
LOADING
 ↓
READY
 ↓
PLAYING
 ↓
PAUSED
 ↓
QUESTION_ACTIVE
 ↓
ANSWER_SUBMITTED
 ↓
FEEDBACK
 ↓
PLAYING
 ↓
COMPLETED
```

Error branch:

```text
ANY STATE
   ↓
ERROR
   ↓
RETRY
   ↓
LOADING
```

---

# 48. Question State Machine

```text
SCHEDULED
   ↓
TRIGGERED
   ↓
DISPLAYED
   ↓
ANSWERING
   ↓
SUBMITTED
   ↓
CORRECT / INCORRECT
   ↓
FEEDBACK
   ↓
COMPLETED
```

---

# 49. Data Required by Player

The player may receive:

```json
{
  "lessonId": "LES1029",
  "videoId": "VID2048",
  "duration": 522,
  "source": "...",
  "qualities": [],
  "languages": [],
  "captions": [],
  "chapters": [],
  "questions": []
}
```

Sensitive information should not be exposed to the client unnecessarily.

---

# 50. Question Object

Example:

```json
{
  "id": "Q1001",
  "timestamp": 152,
  "type": "mcq",
  "question": "What is the value of x?",
  "options": [
    "2",
    "3",
    "4",
    "5"
  ],
  "required": true
}
```

Correct answers should not be exposed to the browser before submission if doing so would allow easy manipulation.

---

# 51. Security Requirements

The video player must not assume that frontend restrictions provide security.

Backend authorization must validate:

```text
User Access
Course Access
Lesson Access
Video Access
Subscription
Question Submission
Completion
```

Signed or protected media URLs should be used where required.

---

# 52. Anti-Tampering Considerations

The platform should not trust:

```text
Client-reported score
Client-reported completion
Client-reported watch duration
Client-reported subscription status
```

These values should be validated server-side where necessary.

---

# 53. Player API Integration

Potential APIs:

```text
GET  /api/lessons/:lessonId/player
GET  /api/videos/:videoId/stream
GET  /api/videos/:videoId/captions
GET  /api/lessons/:lessonId/transcript

POST /api/player/events
POST /api/questions/:questionId/answer
POST /api/lessons/:lessonId/progress
POST /api/lessons/:lessonId/complete
```

Exact contracts belong in the API documentation.

---

# 54. Performance Requirements

The player should:

* Load only required resources.
* Use adaptive streaming where available.
* Avoid unnecessary network requests.
* Lazy-load transcripts where appropriate.
* Avoid blocking the video on analytics requests.
* Use asynchronous analytics submission.
* Handle slow networks gracefully.
* Preserve playback state during UI changes.

---

# 55. Browser Support

The implementation should target modern:

```text
Chrome
Edge
Firefox
Safari
Mobile Safari
Chrome Android
```

Exact supported versions should be defined in the technical requirements.

---

# 56. Testing Requirements

## Functional Tests

* [ ] Video loads.
* [ ] Video plays.
* [ ] Pause works.
* [ ] Seek works.
* [ ] Volume works.
* [ ] Quality selection works.
* [ ] Captions work.
* [ ] Language switching works.
* [ ] Transcript synchronization works.
* [ ] Questions trigger correctly.
* [ ] Questions pause playback.
* [ ] Answers submit correctly.
* [ ] Feedback appears.
* [ ] Progress is saved.
* [ ] Lesson completion works.
* [ ] Fullscreen works.

## Edge Cases

* [ ] Network disconnect.
* [ ] Video unavailable.
* [ ] Translation unavailable.
* [ ] Question API failure.
* [ ] Duplicate answer submission.
* [ ] Browser refresh.
* [ ] Student leaves mid-question.
* [ ] Quality stream unavailable.
* [ ] Expired media URL.

---

# 57. UX Acceptance Criteria

The Video Player is complete when:

* [ ] Students can watch videos without confusion.
* [ ] Controls are discoverable.
* [ ] Questions appear at configured timestamps.
* [ ] Required questions prevent unintended progression.
* [ ] Students receive clear feedback.
* [ ] Progress is preserved.
* [ ] Transcript and captions are synchronized.
* [ ] Language selection works.
* [ ] Quality selection follows access rules.
* [ ] Mobile layout is usable.
* [ ] Keyboard navigation works.
* [ ] Screen-reader labels exist.
* [ ] Errors provide recovery actions.
* [ ] Analytics events are generated correctly.

---

# 58. Recommended Component Structure

```text
VideoPlayer
│
├── VideoViewport
│
├── PlayerOverlay
│   ├── LoadingIndicator
│   ├── ErrorMessage
│   └── QuestionOverlay
│
├── PlayerControls
│   ├── PlayButton
│   ├── ProgressBar
│   ├── VolumeControl
│   ├── SpeedControl
│   ├── QualitySelector
│   ├── CaptionSelector
│   ├── LanguageSelector
│   ├── TranscriptButton
│   ├── ZoomControl
│   ├── PictureInPicture
│   └── FullscreenButton
│
├── TranscriptPanel
├── ChapterPanel
├── QuestionPanel
└── LearningProgress
```

---

# 59. Final Player Architecture

```text
                    ┌──────────────────┐
                    │   LESSON DATA    │
                    └────────┬─────────┘
                             ↓
                    ┌──────────────────┐
                    │   VIDEO PLAYER   │
                    └────────┬─────────┘
                             │
          ┌──────────────────┼──────────────────┐
          ↓                  ↓                  ↓
       VIDEO             TRANSCRIPT          CONTROLS
          │                  │                  │
          ↓                  ↓                  ↓
      TIMELINE          TIMESTAMPS        QUALITY
          │                                  LANGUAGE
          ↓                                  CAPTIONS
   QUESTION ENGINE                           SPEED
          │                                  FULLSCREEN
          ↓
   QUESTION OVERLAY
          │
          ↓
     ANSWER SUBMIT
          │
          ↓
      FEEDBACK
          │
          ↓
    LEARNING PROGRESS
          │
          ↓
       ANALYTICS
```

---

# 60. Relationship With AILPG

The Video Player connects the generated AI lesson to the student's actual learning experience.

```text
MP4
 ↓
AI Analysis
 ↓
Transcript
 ↓
Question Generation
 ↓
Interactive Lesson
 ↓
VIDEO PLAYER
 ↓
Student Interaction
 ↓
Analytics
```

The player therefore acts as the runtime layer for the generated interactive HTML lesson.

---

# 61. Definition of Done

```text
Video Rendering              ✓
Playback Controls            ✓
Adaptive Quality             ✓
Captions                     ✓
Translation                  ✓
Transcript                   ✓
Interactive Questions        ✓
Answer Feedback              ✓
Progress Tracking            ✓
Learning Analytics           ✓
Responsive UI                ✓
Accessibility                ✓
Security                     ✓
Error Recovery               ✓
Testing                      ✓
```

**Document Status:** Ready for implementation planning.
