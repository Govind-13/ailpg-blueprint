---
document_id: SD-006
title: Video Processing Design
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Solution Architecture Team
parent_document: SD-005
last_updated: 2026-08-11
---

# Video Processing Design

## 1. Purpose

This document defines the complete video-processing architecture of AILPG.

The system accepts an uploaded MP4 and converts it into:

```text
Original Video
      ↓
Validated Video
      ↓
Metadata
      ↓
Audio
      ↓
Frames
      ↓
Thumbnails
      ↓
Streaming Versions
      ↓
HLS
      ↓
CDN
      ↓
Interactive Student Player
```

The same processing pipeline also produces media assets required by AI services.

---

# 2. Video Processing Goals

The video engine must support:

```text
MP4 upload
Large file handling
Video validation
Metadata extraction
Audio extraction
Frame extraction
Scene detection
Thumbnail generation
Transcoding
Multiple resolutions
HLS streaming
CDN delivery
Signed access
Processing retry
Processing monitoring
```

---

# 3. High-Level Architecture

```text
                 TEACHER
                    │
                    ▼
              Teacher Portal
                    │
                    ▼
               Upload API
                    │
                    ▼
             Signed Upload URL
                    │
                    ▼
             Object Storage
                    │
                    ▼
             Video Job Queue
                    │
                    ▼
             Video Processing
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
   Metadata       Audio        Frames
       │            │            │
       │            ▼            ├──► OCR
       │           STT            │
       │                         └──► Formula
       │
       └──────────────┬─────────────
                      ▼
                 Transcoding
                      │
                      ▼
                    HLS
                      │
                      ▼
                    CDN
                      │
                      ▼
              Student Video Player
```

---

# 4. Supported Input

Initial supported format:

```text
MP4
```

Expected codecs may include common modern codecs such as:

```text
H.264
H.265
AAC
```

The system must validate actual container and codec compatibility rather than trusting the file extension.

---

# 5. Upload Architecture

The browser should upload large files directly to object storage.

```text
Browser
   │
   │ 1. Request upload session
   ▼
Backend
   │
   │ 2. Generate signed URL
   ▼
Browser
   │
   │ 3. Upload MP4
   ▼
Object Storage
```

This prevents the application server from becoming a large-file transfer bottleneck.

---

# 6. Upload Session

When a teacher starts an upload:

```text
POST /api/v1/videos/upload-session
```

Backend creates:

```text
video_id
upload_id
storage_key
upload URL
expiration
```

---

# 7. Upload Completion

After the file upload:

```text
POST /api/v1/videos/upload-complete
```

Backend verifies:

```text
Video exists
Expected storage location
File size
Upload state
Owner
Organization
```

Then creates:

```text
VIDEO_VALIDATION_JOB
```

---

# 8. Object Storage Structure

Recommended structure:

```text
ailpg-media/
│
├── originals/
│   └── {organization_id}/
│       └── {course_id}/
│           └── {lesson_id}/
│               └── {video_id}/
│                   └── original.mp4
│
├── audio/
│   └── {video_id}/
│       └── audio.wav
│
├── frames/
│   └── {video_id}/
│       ├── frame_000001.jpg
│       ├── frame_000002.jpg
│       └── ...
│
├── thumbnails/
│   └── {video_id}/
│       ├── poster.jpg
│       └── thumb.jpg
│
├── streaming/
│   └── {video_id}/
│       └── hls/
│           ├── master.m3u8
│           ├── 1080p/
│           ├── 720p/
│           ├── 480p/
│           └── 360p/
│
└── temporary/
    └── {job_id}/
```

---

# 9. Video Validation Pipeline

```text
Uploaded File
      ↓
Exists?
      ↓
Size Valid?
      ↓
Container Valid?
      ↓
Video Stream Exists?
      ↓
Codec Supported?
      ↓
Duration Valid?
      ↓
Resolution Valid?
      ↓
Audio Valid?
      ↓
Security Validation
      ↓
ACCEPTED
```

---

# 10. Validation Failure

If validation fails:

```text
VALIDATING
    ↓
FAILED
```

Store:

```text
error_code
error_message
failure_stage
timestamp
```

Example:

```text
VIDEO_CODEC_UNSUPPORTED
VIDEO_CORRUPTED
VIDEO_TOO_LARGE
VIDEO_DURATION_INVALID
AUDIO_STREAM_MISSING
```

---

# 11. Metadata Extraction

The video worker extracts:

```text
File size
Duration
Width
Height
Aspect ratio
Frame rate
Video codec
Audio codec
Audio channels
Audio sample rate
Bitrate
```

Example:

```json
{
  "duration": 600.2,
  "width": 1920,
  "height": 1080,
  "fps": 30,
  "video_codec": "h264",
  "audio_codec": "aac"
}
```

---

# 12. Metadata Storage

Metadata is stored in PostgreSQL.

Example conceptual record:

```text
video_id
duration
width
height
fps
video_codec
audio_codec
file_size
bitrate
created_at
```

---

# 13. Audio Extraction

The system extracts audio from the original video.

```text
Original MP4
     ↓
Audio Extraction
     ↓
Normalized Audio
     ↓
STT Queue
```

Recommended internal format:

```text
PCM WAV
```

or another lossless/AI-compatible format appropriate to the selected speech-processing service.

---

# 14. Audio Normalization

Audio processing may include:

```text
Sample-rate normalization
Channel normalization
Volume normalization
Silence handling
Noise reduction where appropriate
```

The original audio should remain untouched.

---

# 15. Audio Processing Flow

```text
MP4
 ↓
Extract Audio
 ↓
Normalize
 ↓
Quality Check
 ↓
Store
 ↓
STT Job
```

---

# 16. Frame Extraction

Frames are extracted for visual analysis.

```text
Video
 ↓
Frame Sampling
 ↓
Frames
 ↓
OCR
 ↓
Formula Recognition
```

Frame extraction should not blindly extract every frame.

---

# 17. Frame Sampling Strategy

Sampling can be based on:

```text
Fixed interval
Scene change
Slide change
Text change
Formula change
Speech segment
```

For educational videos, adaptive sampling is preferred.

---

# 18. Scene Detection

Scene changes can identify:

```text
New slide
New problem
New formula
Board change
Visual transition
```

Conceptual flow:

```text
Frames
 ↓
Visual Difference
 ↓
Scene Boundary
 ↓
Scene Metadata
```

---

# 19. OCR Frame Selection

Not every frame needs OCR.

Optimization:

```text
Video
 ↓
Scene Detection
 ↓
Representative Frames
 ↓
OCR
```

This reduces AI processing cost.

---

# 20. Formula Frame Selection

Mathematical videos require special handling.

```text
Representative Frames
       +
OCR Results
       +
Visual Detection
       ↓
Formula Candidate
       ↓
Formula Recognition
```

---

# 21. Thumbnail Generation

Generate at least:

```text
Poster image
Course thumbnail
Lesson thumbnail
Video preview thumbnail
```

Example:

```text
thumbnail_16x9.jpg
thumbnail_small.jpg
poster.jpg
```

---

# 22. Thumbnail Selection

Preferred frame:

```text
Clear visual
Readable content
No transition blur
Relevant lesson content
```

The teacher should be able to manually replace the generated thumbnail.

---

# 23. Transcoding Architecture

The original video should be preserved.

```text
Original
   │
   ├──► 1080p
   ├──► 720p
   ├──► 480p
   └──► 360p
```

Actual renditions should be generated only when appropriate for the source resolution and product requirements.

---

# 24. Quality Strategy

Example quality tiers:

| Tier | Resolution | Purpose |
|---|---:|---|
| Source | Original | Archive |
| High | 1080p | Premium |
| Medium | 720p | Standard |
| Low | 480p | Low bandwidth |
| Mobile | 360p | Very low bandwidth |

The final ladder should be adjusted based on source resolution, encoding cost, and CDN strategy.

---

# 25. Student Quality Access

The backend determines which quality levels the user may access.

Example:

```text
Free User
 ├── 360p
 └── 480p

Premium User
 ├── 360p
 ├── 480p
 ├── 720p
 └── 1080p
```

This must be enforced server-side.

---

# 26. HLS Generation

HLS structure:

```text
master.m3u8
    │
    ├── 1080p/index.m3u8
    ├── 720p/index.m3u8
    ├── 480p/index.m3u8
    └── 360p/index.m3u8
```

Each playlist references media segments.

---

# 27. HLS Advantages

HLS provides:

```text
Adaptive streaming
Quality switching
Better bandwidth utilization
CDN compatibility
Mobile compatibility
Resume support
```

---

# 28. Segment Strategy

Video is divided into segments.

Conceptually:

```text
Video
│
├── segment_001
├── segment_002
├── segment_003
├── segment_004
└── ...
```

Segment duration should be chosen based on:

```text
Startup latency
Seeking requirements
CDN behavior
Interactive checkpoint requirements
```

---

# 29. Interactive Checkpoint Compatibility

The video player must be able to:

```text
Read current timestamp
Detect checkpoint
Pause playback
Show question
Resume playback
```

HLS must therefore expose reliable timing information to the player.

---

# 30. CDN Architecture

```text
Student
   ↓
Application
   ↓
Authorization
   ↓
Signed URL
   ↓
CDN
   ↓
HLS
   ↓
Video Segments
```

---

# 31. CDN Caching

Public static assets can have longer cache durations.

Protected video assets should use controlled caching and authorization.

Examples:

```text
Thumbnails → Long cache
Lesson assets → Long cache
Video segments → Controlled cache
Private originals → No public access
```

---

# 32. Signed Video Access

Original video:

```text
PRIVATE
```

Processed HLS:

```text
PROTECTED
```

Student receives:

```text
Short-lived signed playback access
```

---

# 33. Video Processing Jobs

Recommended job types:

```text
VIDEO_VALIDATE
VIDEO_METADATA
VIDEO_AUDIO
VIDEO_FRAMES
VIDEO_SCENE_DETECT
VIDEO_THUMBNAIL
VIDEO_TRANSCODE
VIDEO_HLS
VIDEO_FINALIZE
```

---

# 34. Processing Pipeline

```text
VIDEO_VALIDATE
       ↓
VIDEO_METADATA
       ↓
      ┌┴───────────────┐
      ▼                ▼
VIDEO_AUDIO       VIDEO_FRAMES
      │                │
      ▼                ├──► SCENE_DETECT
     STT               │
                       ├──► OCR
                       │
                       └──► FORMULA
       │
       └────────┬───────┘
                ▼
         VIDEO_TRANSCODE
                ↓
             HLS
                ↓
          VIDEO_FINALIZE
```

---

# 35. Job State

Each job supports:

```text
QUEUED
PROCESSING
COMPLETED
FAILED
RETRYING
CANCELLED
```

---

# 36. Progress Tracking

Teacher dashboard should display:

```text
Validation       ✓
Metadata         ✓
Audio            ✓
Frames           ✓
Transcoding      65%
HLS              Pending
AI Analysis      Pending
```

---

# 37. Processing Progress

Progress should be represented as:

```text
stage
status
percentage
started_at
completed_at
error
```

Example:

```json
{
  "stage": "TRANSCODING",
  "status": "PROCESSING",
  "percentage": 65
}
```

---

# 38. Worker Architecture

```text
Queue
 │
 ├── Video Validation Worker
 ├── Metadata Worker
 ├── Audio Worker
 ├── Frame Worker
 ├── Scene Worker
 ├── Thumbnail Worker
 ├── Transcoding Worker
 └── HLS Worker
```

Workers may be combined initially if operationally simpler.

---

# 39. FFmpeg Processing

FFmpeg is the recommended media-processing engine for:

```text
Metadata
Audio extraction
Frame extraction
Transcoding
Thumbnail generation
HLS generation
```

The exact commands should be encapsulated inside the Video Processing service.

---

# 40. FFmpeg Security

Never execute user-provided strings directly as shell commands.

Unsafe:

```text
shell("ffmpeg " + user_input)
```

Use:

```text
Validated parameters
+
Argument arrays
+
Restricted filesystem paths
+
Process timeouts
```

---

# 41. Temporary Workspace

Each processing job receives an isolated workspace:

```text
/tmp/ailpg/{job_id}/
```

Example:

```text
job_123/
├── input.mp4
├── audio.wav
├── frames/
├── thumbnails/
└── output/
```

---

# 42. Temporary File Cleanup

After successful processing:

```text
Temporary files
      ↓
Cleanup
```

After failure:

```text
Temporary files
      ↓
Retain required debug artifacts
      ↓
Cleanup after retention period
```

---

# 43. Large File Handling

The platform should support large MP4 files without routing the entire file through the API server.

Use:

```text
Direct object-storage upload
Multipart upload where supported
Resumable upload
Upload checksum
```

---

# 44. Upload Resume

If connection fails:

```text
Upload
  ↓
Network Failure
  ↓
Resume
  ↓
Continue Upload
```

The user should not have to restart the entire upload whenever resumable upload is supported.

---

# 45. Checksum Validation

After upload:

```text
Client Upload
 ↓
Object Storage
 ↓
Checksum Verification
 ↓
Accept
```

This reduces corruption risk.

---

# 46. Duplicate Detection

Optional future optimization:

```text
Video
 ↓
Hash
 ↓
Compare
 ↓
Existing?
```

If identical media already exists and reuse is allowed:

```text
Reuse Existing Asset
```

---

# 47. Video Versioning

If a teacher replaces a video:

```text
Video v1
   ↓
New Upload
   ↓
Video v2
```

Do not overwrite the currently published asset blindly.

---

# 48. Published Video Protection

Published video assets should be immutable.

```text
Published Video
       ↓
Immutable Asset
```

Replacement creates a new version.

---

# 49. Video Deletion

Delete workflow:

```text
Delete Request
 ↓
Authorization
 ↓
Check References
 ↓
Soft Delete
 ↓
Audit
 ↓
Background Cleanup
 ↓
Object Storage Cleanup
```

Hard deletion should be restricted.

---

# 50. Storage Lifecycle

Example:

```text
Original
   ↓
Active
   ↓
Archived
   ↓
Retention Period
   ↓
Deleted
```

Temporary assets:

```text
Temporary
   ↓
Processing Complete
   ↓
Automatic Cleanup
```

---

# 51. Video Processing Failure

Example:

```text
Transcoding
   ↓
Failure
   ↓
Record Error
   ↓
Retry
```

If repeated failure:

```text
Dead Letter Queue
       ↓
Admin
```

---

# 52. Retry Policy

Retryable failures:

```text
Temporary storage error
Network error
Temporary provider failure
Worker crash
```

Non-retryable failures:

```text
Corrupted video
Unsupported format
Invalid metadata
Security validation failure
```

---

# 53. Video Processing Cancellation

Teacher/Admin can request cancellation.

```text
PROCESSING
    ↓
CANCEL REQUEST
    ↓
Worker checks cancellation
    ↓
Stop process
    ↓
CLEANUP
    ↓
CANCELLED
```

---

# 54. Monitoring Metrics

Video metrics:

```text
Upload duration
Validation duration
Transcoding duration
HLS generation duration
Average processing time
Failure rate
Retry rate
Storage usage
```

---

# 55. Quality Monitoring

Track:

```text
Transcoding success
Playback errors
Buffering
Startup time
CDN errors
Manifest errors
Segment errors
```

---

# 56. Student Playback Architecture

```text
Student
  ↓
Lesson API
  ↓
Entitlement
  ↓
Playback Session
  ↓
Signed HLS URL
  ↓
Player
  ↓
CDN
  ↓
HLS
```

---

# 57. Playback Session

A playback session may contain:

```text
session_id
user_id
lesson_id
video_id
quality entitlement
created_at
expires_at
```

This can be used for controlled access and analytics.

---

# 58. Player Requirements

Student player must support:

```text
Play
Pause
Seek
Volume
Fullscreen
Playback speed
Quality selection
Subtitles
Transcript
Language
Interactive questions
Progress
```

---

# 59. Zoom Requirement

For educational videos, zoom may be supported at the player UI level.

Possible behavior:

```text
Normal
   ↓
Zoom 1.25x
   ↓
Zoom 1.5x
   ↓
Zoom 2x
```

Zoom should not alter the source video.

---

# 60. Video + Transcript Synchronization

```text
Video Timestamp
      │
      ▼
Transcript Segment
      │
      ▼
Highlight Current Text
```

Example:

```text
00:42 ──► "Now factor the equation..."
```

---

# 61. Video + Question Synchronization

```text
Video Timestamp
      ↓
Checkpoint
      ↓
Pause
      ↓
Question
      ↓
Answer
      ↓
Feedback
      ↓
Resume
```

---

# 62. Processing Completion

A video is considered fully ready when:

```text
Original Valid
✓
Metadata Available
✓
Audio Available
✓
Frames Available
✓
Required AI Assets Available
✓
Streaming Available
✓
Thumbnail Available
✓
Manifest Reference Available
✓
```

---

# 63. Video Status Model

```text
UPLOADING
    ↓
UPLOADED
    ↓
VALIDATING
    ↓
PROCESSING
    ↓
READY_FOR_REVIEW
    ↓
APPROVED
    ↓
PUBLISHED
```

Failure can occur from processing states:

```text
FAILED
```

---

# 64. Video Database Entities

Core entities:

```text
videos
video_metadata
video_assets
video_processing_jobs
video_renditions
video_thumbnails
video_frames
playback_sessions
```

Detailed schema belongs to:

```text
Layer 6 — Database Design
```

---

# 65. API Endpoints

Conceptual endpoints:

```text
POST   /api/v1/videos/upload-session
POST   /api/v1/videos/upload-complete
GET    /api/v1/videos/{video_id}
GET    /api/v1/videos/{video_id}/status
POST   /api/v1/videos/{video_id}/process
POST   /api/v1/videos/{video_id}/cancel
DELETE /api/v1/videos/{video_id}
GET    /api/v1/videos/{video_id}/playback
GET    /api/v1/videos/{video_id}/assets
```

Exact API contracts will be defined in:

```text
Layer 7 — API Design
```

---

# 66. Video Processing Security

Required:

```text
Private original storage
Signed upload
Signed playback
File validation
Path isolation
Process isolation
Resource limits
Timeouts
No arbitrary shell execution
Audit logging
```

---

# 67. Resource Limits

Processing must have limits for:

```text
Maximum file size
Maximum duration
Maximum resolution
Maximum processing time
Maximum frame count
Maximum temporary storage
Maximum concurrent jobs
```

Limits should be configurable by deployment/plan.

---

# 68. Scalability

Video workers should scale horizontally.

```text
                 Queue
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
   Worker 1    Worker 2    Worker 3
       │           │           │
       └───────────┼───────────┘
                   ▼
              Object Storage
```

---

# 69. Priority Queues

Possible priorities:

```text
HIGH
 └── Paid customer processing

NORMAL
 └── Standard processing

LOW
 └── Batch/reprocessing
```

Priority policy must be designed to avoid starvation.

---

# 70. Cost Optimization

Optimization strategies:

```text
Adaptive frame sampling
Avoid unnecessary transcoding
Reuse generated assets
Queue batching where useful
Auto-clean temporary files
Use appropriate AI resolution
Use lower-cost processing for previews
```

---

# 71. Disaster Recovery

Original videos and critical processed assets must follow backup and durability policies appropriate to the chosen storage provider.

Database backups must include:

```text
Video metadata
Processing state
Lesson references
Asset references
```

---

# 72. Video Processing Definition of Done

```text
MP4 Upload                  ✓
Resumable Upload            ✓
Validation                  ✓
Metadata Extraction         ✓
Audio Extraction            ✓
Frame Extraction            ✓
Scene Detection             ✓
Thumbnail Generation        ✓
Transcoding                 ✓
HLS Generation              ✓
CDN Delivery                ✓
Signed Access               ✓
Processing Jobs             ✓
Retry Handling              ✓
Cancellation                ✓
Cleanup                     ✓
Monitoring                  ✓
Security                    ✓
Scaling                     ✓
```

---

# 73. Next Document

```text
07_AI_Pipeline_Design.md
```

This will define the complete AI engine:

```text
MP4
 ↓
Audio
 ↓
Speech-to-Text
 ↓
Transcript
 ↓
Frame Analysis
 ↓
OCR
 ↓
Math Formula Recognition
 ↓
Topic Detection
 ↓
Learning Objective
 ↓
Question Generation
 ↓
Question Validation
 ↓
Translation
 ↓
Interactive Checkpoint Generation
 ↓
Lesson Manifest
 ↓
Teacher Review
```

It will also define:

```text
AI providers
Prompt architecture
AI jobs
Model routing
Confidence scoring
AI validation
Fallbacks
Cost control
Human review
AI versioning
```

---

# 74. Revision History

| Version | Date | Description |
|---|---|---|
| 1.0.0 | 2026-08-11 | Initial video processing design |
