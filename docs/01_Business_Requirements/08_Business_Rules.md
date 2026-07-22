---
document_id: BRD-008
title: Business Rules
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Product Team
parent_document: 07_User_Journeys.md
last_updated: 2026-07-22
---

# Business Rules

> This document defines the business rules that govern user behavior, AI processing, content publishing, subscriptions, analytics, and platform operations.

---

# Table of Contents

1. Purpose
2. Authentication Rules
3. Course Management Rules
4. Video Upload Rules
5. AI Processing Rules
6. Content Review Rules
7. Publishing Rules
8. Student Learning Rules
9. Quiz Rules
10. Subscription Rules
11. Analytics Rules
12. Notification Rules
13. Versioning Rules
14. Audit Rules
15. Related Documents
16. Revision History

---

# 1. Purpose

Business Rules define the policies that the platform must enforce consistently.

Every functional requirement, API, and validation should comply with these rules.

---

# 2. Authentication Rules

## BR-AUTH-001

Every user must authenticate before accessing protected resources.

## BR-AUTH-002

Permissions are determined by the assigned role.

Supported roles:

- Student
- Teacher
- Reviewer
- Institution Administrator
- Platform Administrator
- Super Administrator

## BR-AUTH-003

Inactive or suspended accounts cannot log in.

---

# 3. Course Management Rules

## BR-COURSE-001

Only Teachers and Super Administrators can create courses.

## BR-COURSE-002

Each course must have:

- Title
- Description
- Primary language
- Owner
- Status

## BR-COURSE-003

Course Status:

- Draft
- AI Processing
- Review
- Published
- Archived

---

# 4. Video Upload Rules

## BR-VIDEO-001

Version 1 accepts MP4 uploads.

## BR-VIDEO-002

Video validation must check:

- File type
- File size
- Duration
- Integrity

## BR-VIDEO-003

Unsupported or corrupted uploads are rejected with a clear error message.

## BR-VIDEO-004

Each upload receives a unique identifier and is stored in object storage.

---

# 5. AI Processing Rules

## BR-AI-001

Every uploaded MP4 creates a background AI processing job.

## BR-AI-002

AI processing stages:

1. Metadata extraction
2. Audio extraction
3. Speech-to-text
4. OCR
5. Formula recognition
6. Topic segmentation
7. Quiz generation
8. Translation
9. Interactive HTML generation

## BR-AI-003

Each stage records its status:

- Pending
- Running
- Completed
- Failed

## BR-AI-004

A failure in one stage should not erase successful outputs from previous stages.

---

# 6. Content Review Rules

## BR-REVIEW-001

AI-generated content cannot be published without teacher approval.

## BR-REVIEW-002

Teachers may:

- Edit
- Approve
- Reject
- Regenerate selected AI outputs

## BR-REVIEW-003

All edits must be versioned.

---

# 7. Publishing Rules

## BR-PUBLISH-001

Only approved lessons can be published.

## BR-PUBLISH-002

Publishing creates a new immutable published version.

## BR-PUBLISH-003

Previously published versions remain available for rollback (if enabled).

---

# 8. Student Learning Rules

## BR-STUDENT-001

Students can only access courses assigned to them or available through their subscription.

## BR-STUDENT-002

Learning progress is automatically saved.

## BR-STUDENT-003

Students can resume from the last completed position.

## BR-STUDENT-004

Interactive quizzes pause the lesson until answered or skipped (based on course configuration).

---

# 9. Quiz Rules

## BR-QUIZ-001

Quiz questions are linked to lesson timestamps.

## BR-QUIZ-002

Teachers can modify or replace AI-generated questions.

## BR-QUIZ-003

Quiz results are stored for analytics.

---

# 10. Subscription Rules

## BR-SUB-001

Subscription plans determine feature access.

Example:

### Free

- Standard video quality
- Limited AI-generated features
- Limited storage
- Basic analytics

### Premium

- Adaptive streaming with higher quality (where available)
- Full interactive lesson features
- Advanced analytics
- Priority AI processing
- Increased storage

## BR-SUB-002

Subscription changes apply according to the organization's billing policy.

---

# 11. Analytics Rules

The platform records:

- Lesson completion
- Quiz attempts
- Quiz scores
- Watch time
- Replay events
- Drop-off points
- Language selected
- Device category

Analytics should respect applicable privacy requirements.

---

# 12. Notification Rules

Notifications are generated for events such as:

- AI job started
- AI job completed
- Review required
- Lesson published
- Course assigned
- Course completed
- Certificate available

Delivery channels may include:

- In-app
- Email
- Push notifications (future)

---

# 13. Versioning Rules

Every significant change creates a new version.

Versioned assets include:

- Transcript
- OCR output
- Quiz
- HTML lesson
- Translation
- Course metadata

Administrators can view version history based on permissions.

---

# 14. Audit Rules

The platform records:

- Login events
- Upload events
- Review actions
- Publishing actions
- Administrative changes

Audit logs should be retained according to organizational policy.

---

# 15. Related Documents

- 07_User_Journeys.md
- 09_Functional_Requirements.md
- RBAC_Design.md (future)
- API_Design.md (future)

---

# 16. Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | 2026-07-22 | Initial Business Rules document |
