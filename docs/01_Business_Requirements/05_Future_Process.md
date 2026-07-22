---
document_id: BRD-005
title: Future Business Process (To-Be)
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Business Analysis Team
parent_document: 04_Current_Process.md
last_updated: 2026-07-22
---

# Future Business Process (To-Be)

> This document describes the future AI-powered workflow for converting an educational MP4 video into an interactive HTML learning experience.

---

# Table of Contents

1. Purpose
2. Future Vision
3. Actors
4. End-to-End Workflow
5. AI Processing Pipeline
6. Teacher Review Workflow
7. Student Learning Workflow
8. Analytics Workflow
9. Exception Handling
10. Future Process Diagram
11. Expected Benefits
12. Related Documents
13. Revision History

---

# 1. Purpose

The purpose of this document is to define the future ("To-Be") business process that replaces today's manual workflow with an AI-assisted, review-driven publishing pipeline.

The objective is to reduce repetitive work while preserving teacher control over published educational content.

---

# 2. Future Vision

A teacher uploads a single MP4 video.

The platform automatically:

- extracts audio,
- transcribes speech,
- recognizes mathematical formulas,
- extracts on-screen text,
- identifies lesson topics,
- generates quizzes,
- creates summaries,
- translates content,
- builds an interactive HTML lesson,

and then presents everything for teacher review before publication.

---

# 3. Actors

Primary actors:

- Teacher
- AI Processing Engine
- Reviewer (optional)
- Student
- Institution Administrator
- Platform Administrator

Supporting services:

- Authentication Service
- Video Processing Service
- AI Service
- Translation Service
- Notification Service
- Analytics Service

---

# 4. End-to-End Workflow

## Phase 1 – Upload

Teacher logs in.

↓

Creates a course.

↓

Uploads MP4.

↓

Video stored in Object Storage.

↓

Metadata extracted.

↓

AI processing job created.

---

## Phase 2 – AI Analysis

Video Processing Service:

- Extract audio
- Generate thumbnails
- Detect duration
- Detect resolution

↓

Speech Recognition

↓

Transcript

↓

Timestamp Mapping

↓

OCR

↓

Formula Recognition

↓

Topic Segmentation

↓

Content Classification

---

## Phase 3 – AI Content Generation

The AI engine automatically creates:

- subtitles,
- lesson notes,
- chapter titles,
- learning objectives,
- summaries,
- glossary,
- hints,
- quizzes,
- explanations,
- translations,
- interactive HTML lesson.

Each generated artifact is versioned and linked to the original video.

---

## Phase 4 – Teacher Review

Teacher reviews:

- transcript,
- OCR output,
- formulas,
- quizzes,
- translations,
- HTML lesson.

Teacher may:

- accept,
- edit,
- regenerate selected sections,
- reject content,
- save drafts.

Publishing is allowed only after review.

---

## Phase 5 – Publishing

Approved lesson becomes available.

Generated outputs include:

- HTML lesson
- subtitles
- quizzes
- notes
- downloadable resources
- learning analytics configuration

---

## Phase 6 – Student Learning

Student logs in.

↓

Selects course.

↓

Interactive lesson opens.

Student can:

- watch video,
- answer quizzes,
- pause,
- resume,
- change playback speed,
- zoom supported lesson content,
- switch language,
- view subtitles,
- bookmark,
- take notes,
- download approved resources,
- continue from last progress.

---

## Phase 7 – Analytics

The platform records:

- lesson completion,
- quiz scores,
- replay events,
- pause frequency,
- chapter completion,
- average watch time,
- learning progress.

Teachers access dashboards for class-level and lesson-level insights.

---

# 5. AI Processing Pipeline

```
MP4 Upload
      │
      ▼
Video Validation
      │
      ▼
Metadata Extraction
      │
      ▼
Audio Extraction
      │
      ▼
Speech-to-Text
      │
      ▼
Timestamp Alignment
      │
      ▼
OCR
      │
      ▼
Formula Recognition
      │
      ▼
Topic Segmentation
      │
      ▼
Knowledge Extraction
      │
      ▼
Quiz Generation
      │
      ▼
Translation
      │
      ▼
Interactive HTML Generation
      │
      ▼
Teacher Review
      │
      ▼
Publish
      │
      ▼
Student Learning
      │
      ▼
Analytics
```

---

# 6. Teacher Review Workflow

Teachers can:

- Edit transcript
- Correct OCR
- Replace formulas
- Modify quizzes
- Regenerate AI content
- Edit HTML lesson
- Preview lesson
- Publish
- Archive
- Create a new version

Every change is stored with version history.

---

# 7. Student Learning Workflow

Interactive features include:

- Video player
- Auto pause at quiz checkpoints
- Multiple-choice questions
- Fill-in-the-blank questions
- Mathematical answer input (future enhancement)
- Progress tracking
- Resume learning
- Notes
- Bookmarks
- Dark mode
- Language switching
- Adaptive streaming based on subscription and network conditions

Premium subscriptions may unlock higher video quality where available.

---

# 8. Analytics Workflow

Data collected includes:

- Student progress
- Quiz accuracy
- Learning time
- Drop-off points
- Frequently replayed sections
- Completion rate
- Device type
- Language selection

Dashboards support teachers, institutions, and platform administrators.

---

# 9. Exception Handling

Examples:

- Unsupported video format
- Corrupted upload
- AI processing failure
- OCR confidence below threshold
- Translation unavailable
- Quiz generation failure

The platform should notify users and allow retry or manual correction where appropriate.

---

# 10. Future Process Diagram

```text
Teacher
    │
    ▼
Upload MP4
    │
    ▼
AI Processing Pipeline
    │
    ├── Speech-to-Text
    ├── OCR
    ├── Formula Recognition
    ├── Topic Detection
    ├── Translation
    ├── Quiz Generation
    ├── HTML Generation
    │
    ▼
Teacher Review
    │
    ▼
Publish
    │
    ▼
Student Portal
    │
    ▼
Learning Analytics
```

---

# 11. Expected Benefits

## Teachers

- Reduced manual effort
- Faster publishing
- AI-assisted content creation
- Centralized workflow

## Students

- Interactive learning
- Immediate feedback
- Multilingual access
- Better engagement

## Institutions

- Scalable course production
- Standardized quality
- Actionable analytics
- Improved operational efficiency

---

# 12. Related Documents

- 04_Current_Process.md
- 06_User_Personas.md
- Functional_Requirements.md
- AI_Workflow.md (future)
- System_Architecture.md (future)

---

# 13. Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | 2026-07-22 | Initial Future Business Process |
