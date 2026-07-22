---
document_id: BRD-006
title: User Personas
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Product Team
parent_document: 05_Future_Process.md
last_updated: 2026-07-22
---

# User Personas

> This document defines the primary user roles, goals, responsibilities, permissions, and success criteria for the AILPG platform.

---

# Table of Contents

1. Purpose
2. Persona Overview
3. Student
4. Teacher
5. Content Reviewer
6. Institution Administrator
7. Platform Administrator
8. Super Administrator
9. Persona Comparison Matrix
10. Related Documents
11. Revision History

---

# 1. Purpose

User Personas represent the different categories of users who interact with the platform.

They help ensure that product design, development, and security align with real user needs.

---

# 2. Persona Overview

| Persona | Primary Goal |
|----------|--------------|
| Student | Learn through interactive lessons |
| Teacher | Create and publish AI-assisted courses |
| Content Reviewer | Validate AI-generated educational content |
| Institution Administrator | Manage users, courses, and subscriptions |
| Platform Administrator | Operate and monitor the platform |
| Super Administrator | Configure platform-wide settings |

---

# 3. Student

## Goals

- Learn effectively
- Complete lessons
- Improve quiz scores
- Track progress
- Access lessons from multiple devices

## Responsibilities

- Watch lessons
- Answer quizzes
- Review feedback
- Complete assigned courses

## Features

- Interactive video
- Auto-pause quizzes
- Playback speed
- Language switching
- Subtitles
- Zoom (supported lesson content)
- Notes
- Bookmarks
- Progress tracking
- Resume learning
- Certificate download (if enabled)

## Dashboard

- My Courses
- Continue Learning
- Completed Courses
- Quiz Scores
- Certificates
- Notifications

## Success Criteria

- Complete assigned lessons
- Improve assessment results
- Maintain learning progress

---

# 4. Teacher

## Goals

- Publish courses quickly
- Reduce manual work
- Review AI-generated content
- Improve student outcomes

## Responsibilities

- Upload MP4 videos
- Review AI output
- Edit generated content
- Publish lessons
- Monitor learner performance

## Features

- Course management
- Video upload
- Transcript editor
- OCR editor
- Formula editor
- Quiz editor
- HTML preview
- Publish workflow
- Version history

## Dashboard

- My Courses
- AI Jobs
- Draft Lessons
- Student Analytics
- Notifications

## Success Criteria

- Publish high-quality courses
- Maintain accurate educational content
- Increase learner engagement

---

# 5. Content Reviewer

## Goals

- Ensure educational quality
- Validate AI-generated content

## Responsibilities

- Review transcripts
- Verify formulas
- Check translations
- Validate quizzes
- Approve or reject content

## Dashboard

- Review Queue
- Pending Approvals
- Feedback History

## Success Criteria

- High content accuracy
- Fast review turnaround

---

# 6. Institution Administrator

## Goals

- Manage the institution efficiently

## Responsibilities

- Manage teachers
- Manage students
- Assign courses
- Monitor subscriptions
- Access reports

## Dashboard

- Institution Overview
- User Management
- Course Management
- Subscription Status
- Reports

## Success Criteria

- High course adoption
- Active learners
- Efficient administration

---

# 7. Platform Administrator

## Goals

- Keep the platform secure and operational

## Responsibilities

- Monitor infrastructure
- Manage AI processing queues
- Monitor storage
- Handle incidents
- View system metrics

## Dashboard

- System Health
- AI Queue
- API Status
- Storage Usage
- Audit Logs
- Alerts

## Success Criteria

- High platform availability
- Stable AI processing
- Rapid incident response

---

# 8. Super Administrator

## Goals

- Configure and govern the entire platform

## Responsibilities

- Global configuration
- Role management
- Feature flags
- AI provider configuration
- Subscription plans
- Security policies

## Dashboard

- Global Settings
- Organization Management
- Licensing
- Feature Management
- Audit Reports

## Success Criteria

- Secure platform governance
- Consistent configuration
- Controlled feature rollout

---

# 9. Persona Comparison Matrix

| Capability | Student | Teacher | Reviewer | Institution Admin | Platform Admin | Super Admin |
|------------|:-------:|:-------:|:--------:|:-----------------:|:--------------:|:-----------:|
| View Courses | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Upload MP4 | ✗ | ✓ | ✗ | Optional | ✗ | ✓ |
| Review AI Content | ✗ | ✓ | ✓ | ✗ | ✗ | ✓ |
| Publish Lessons | ✗ | ✓ | Optional | ✗ | ✗ | ✓ |
| Manage Users | ✗ | ✗ | ✗ | ✓ | ✓ | ✓ |
| Manage Subscriptions | ✗ | ✗ | ✗ | ✓ | ✓ | ✓ |
| System Monitoring | ✗ | ✗ | ✗ | ✗ | ✓ | ✓ |
| Global Settings | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ |

---

# 10. Related Documents

- 05_Future_Process.md
- 07_User_Journeys.md
- Functional_Requirements.md
- RBAC_Design.md (future)
- UI_UX_Blueprint.md (future)

---

# 11. Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | 2026-07-22 | Initial User Personas document |
