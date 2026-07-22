---
document_id: BRD-004
title: Current Business Process (As-Is)
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Business Analysis Team
parent_document: 03_Business_Objectives.md
last_updated: 2026-07-22
---

# Current Business Process (As-Is)

> This document describes how educational videos are currently converted into online learning content without the AILPG platform.

---

# Table of Contents

1. Purpose
2. Overview
3. Actors
4. Current Workflow
5. Manual Activities
6. Pain Points
7. Current Technology Landscape
8. Process Metrics
9. Current Workflow Diagram
10. Related Documents
11. Revision History

---

# 1. Purpose

This document captures the existing ("As-Is") process used by educators to create online learning materials from recorded educational videos.

Understanding the current workflow helps identify inefficiencies and opportunities for automation.

---

# 2. Overview

Today, educators typically perform many manual tasks after recording a lesson.

A single MP4 video often requires additional work before it becomes a complete online course.

These activities may involve multiple software tools and significant manual effort.

---

# 3. Actors

The current process involves:

- Teacher
- Video Editor
- Content Reviewer
- Translator
- LMS Administrator
- Student

In smaller organizations, one person may perform multiple roles.

---

# 4. Current Workflow

Typical workflow:

1. Record lesson.
2. Export as MP4.
3. Upload video to storage.
4. Watch the video again.
5. Create transcript manually or using separate tools.
6. Correct transcript errors.
7. Create subtitles.
8. Translate subtitles (if required).
9. Write lesson notes.
10. Create quizzes manually.
11. Build HTML/LMS pages.
12. Upload supporting documents.
13. Review all content.
14. Publish course.
15. Students access the lesson.

---

# 5. Manual Activities

The following activities are largely manual:

| Activity | Manual Effort |
|----------|---------------|
| Transcript correction | High |
| Subtitle preparation | High |
| Quiz creation | High |
| Topic segmentation | High |
| Lesson notes | High |
| Formula extraction | High |
| HTML page creation | High |
| Translation review | Medium to High |

These tasks increase overall production time.

---

# 6. Pain Points

## PP-001 Repeated Work

Every new lesson repeats the same preparation workflow.

---

## PP-002 Multiple Tools

Teachers may switch between:

- Video editor
- Subtitle editor
- Translation tool
- Word processor
- LMS
- Image editor

This reduces efficiency.

---

## PP-003 Long Publishing Cycle

Publishing a lesson can take significantly longer than recording it.

---

## PP-004 Human Errors

Manual workflows may introduce:

- Typing mistakes
- Subtitle timing issues
- Missing quiz questions
- Inconsistent formatting

---

## PP-005 Limited Learning Analytics

Many platforms provide limited visibility into:

- Student engagement
- Quiz performance
- Learning difficulties
- Content effectiveness

---

# 7. Current Technology Landscape

Common tools used today include:

- Video recording software
- Video editing software
- Subtitle editing tools
- Translation tools
- Office applications
- LMS platforms
- Cloud storage

These tools often operate independently and require manual coordination.

---

# 8. Process Metrics

Typical characteristics:

| Metric | Current State |
|---------|---------------|
| Manual work | High |
| Automation | Low |
| AI usage | Minimal |
| Content consistency | Varies |
| Publishing speed | Slow |
| Scalability | Limited |

Exact values depend on the organization and workflow.

---

# 9. Current Workflow Diagram

```text
Teacher
    │
    ▼
Record Video
    │
    ▼
Export MP4
    │
    ▼
Upload Video
    │
    ▼
Watch Again
    │
    ▼
Write Transcript
    │
    ▼
Correct Transcript
    │
    ▼
Create Subtitles
    │
    ▼
Translate
    │
    ▼
Create Notes
    │
    ▼
Create Quiz
    │
    ▼
Build HTML/LMS
    │
    ▼
Review
    │
    ▼
Publish
    │
    ▼
Student Learning
```

---

# Key Observations

- The workflow depends heavily on manual effort.
- Multiple disconnected tools are required.
- Publishing is time-consuming.
- Interactive learning elements are often added manually.
- Opportunities exist for AI-assisted automation while preserving teacher review.

---

# 10. Related Documents

- 02_Business_Problem.md
- 03_Business_Objectives.md
- 05_Future_Process.md

---

# 11. Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | 2026-07-22 | Initial Current Business Process document |
