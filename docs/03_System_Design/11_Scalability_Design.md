# AILPG — Scalability Design

**Document ID:** SD-011
**Version:** 1.0.0
**Status:** Draft
**Project:** AILPG — AI Learning Platform Generator
**Parent Document:** SD-010 Security Design
**Last Updated:** 2026-08-11

---

# 1. Purpose

This document defines the scalability architecture for AILPG.

The platform must scale independently across:

```text
Student Traffic
Video Streaming
MP4 Processing
AI Processing
Database
Storage
Analytics
API Requests
Background Jobs
```

Core principle:

```text
More Users
    ↓
More API Traffic
    ↓
Horizontal Scaling

More Videos
    ↓
More Processing Jobs
    ↓
Worker Scaling

More Streaming
    ↓
CDN Scaling
```

---

# 2. Scalability Goals

AILPG should support growth from:

```text
MVP
 ↓
Small Institution
 ↓
Multiple Institutions
 ↓
Large Education Platform
```

without requiring a complete architecture rewrite.

---

# 3. Scaling Dimensions

The system must scale in multiple dimensions:

```text
1. Compute
2. API
3. Video
4. AI
5. Database
6. Storage
7. Queue
8. Analytics
9. CDN
10. Background workers
```

---

# 4. High-Level Scalable Architecture

```text
                         INTERNET
                            │
                            ▼
                      CDN / WAF
                            │
                            ▼
                     Load Balancer
                            │
              ┌─────────────┼─────────────┐
              ▼             ▼             ▼
           API-1         API-2         API-N
              │             │             │
              └─────────────┼─────────────┘
                            │
                ┌───────────┼───────────┐
                ▼           ▼           ▼
             Cache       Database     Queue
                            │           │
                            │     ┌─────┼─────┐
                            │     ▼     ▼     ▼
                            │   Video  AI   Analytics
                            │  Worker Worker Worker
                            │
                            ▼
                       Object Storage
```

---

# 5. Horizontal Scaling

Primary application scaling strategy:

```text
API Server
   ↓
Multiple Stateless Instances
```

Example:

```text
API-1
API-2
API-3
API-4
```

Traffic is distributed by a load balancer.

---

# 6. Stateless API

Application servers should avoid storing critical session state locally.

Bad:

```text
User Session
     ↓
API Server 1 only
```

Better:

```text
User
 ↓
Load Balancer
 ↓
Any API Server
 ↓
Shared Session / Token / Database
```

---

# 7. Load Balancing

Traffic:

```text
                  Load Balancer
                 /      |      \
                /       |       \
              API-1    API-2    API-3
```

Load balancing can use:

```text
Round Robin
Least Connections
Weighted Routing
Health-Based Routing
```

The exact strategy depends on infrastructure.

---

# 8. Health Checks

Each API instance should expose a health endpoint.

Example:

```text
GET /health
```

Possible responses:

```json
{
  "status": "healthy"
}
```

Health checks should distinguish basic process health from deeper dependency health where appropriate.

---

# 9. Autoscaling

Application instances can scale based on:

```text
CPU
Memory
Request Rate
Latency
Queue Depth
```

Example:

```text
Traffic increases
       ↓
CPU > threshold
       ↓
Add API instances
```

---

# 10. Scale Down

When traffic decreases:

```text
Traffic ↓
   ↓
Unused instances
   ↓
Scale down
```

Avoid aggressive scale-down that causes instability.

---

# 11. API Capacity

API servers primarily handle:

```text
Authentication
Lesson metadata
Progress
Questions
Dashboard requests
Admin requests
```

They should not perform heavy video/AI processing synchronously.

---

# 12. Heavy Processing Separation

Bad architecture:

```text
API Request
   ↓
FFmpeg
   ↓
Transcription
   ↓
Translation
   ↓
AI Generation
```

This causes:

```text
Long requests
Timeouts
High memory
Poor scalability
```

---

# 13. Correct Architecture

```text
API
 ↓
Create Job
 ↓
Queue
 ↓
Worker
 ↓
Processing
 ↓
Storage
 ↓
Job Status
```

---

# 14. Queue-Based Scaling

Example:

```text
100 MP4 uploads
      ↓
100 Jobs
      ↓
Queue
      ↓
Workers
```

Workers consume jobs according to available capacity.

---

# 15. Worker Autoscaling

Worker count should depend on:

```text
Queue Depth
Job Duration
CPU
Memory
GPU Usage
```

Example:

```text
Queue = 10
    ↓
2 Workers

Queue = 1000
    ↓
20 Workers
```

Actual scaling limits should be configured based on infrastructure cost and capacity.

---

# 16. Worker Types

Separate worker pools:

```text
Video Workers
AI Workers
Translation Workers
Transcription Workers
Analytics Workers
Notification Workers
```

This prevents one workload from starving another.

---

# 17. Video Processing Scaling

Video pipeline:

```text
Upload
 ↓
Queue
 ↓
Video Worker
 ↓
FFmpeg
 ↓
Transcoding
 ↓
HLS
 ↓
Object Storage
```

More videos:

```text
More Queue Jobs
       ↓
More Video Workers
```

---

# 18. AI Worker Scaling

AI jobs may include:

```text
Transcript analysis
Question generation
Summary
Translation
Metadata generation
Concept extraction
```

Architecture:

```text
AI Job Queue
      ↓
AI Worker Pool
      ↓
AI Provider
```

---

# 19. AI Rate Limits

External AI providers may impose:

```text
Requests per minute
Tokens per minute
Concurrency
Daily quota
```

AILPG must therefore use:

```text
Queue
Retry
Backoff
Rate Limiter
Provider Routing
```

---

# 20. AI Backpressure

If AI provider capacity is reached:

```text
AI Requests
    ↓
Queue
    ↓
Controlled Processing
```

Do not continuously retry immediately.

Use:

```text
Exponential Backoff
+
Jitter
```

---

# 21. Retry Strategy

Temporary failure:

```text
Attempt 1
 ↓
Wait
 ↓
Attempt 2
 ↓
Wait longer
 ↓
Attempt 3
```

After maximum attempts:

```text
FAILED
```

The job should be inspectable and retryable.

---

# 22. Dead Letter Queue

Failed jobs:

```text
Main Queue
   ↓
Retries
   ↓
Still Failed
   ↓
Dead Letter Queue
```

Admin can inspect and replay eligible jobs.

---

# 23. Idempotency

Jobs must be safe against duplicate execution where practical.

Example:

```text
Job ID = video_001_transcode
```

If worker receives it twice:

```text
First → Process
Second → Detect existing state / safely reuse result
```

---

# 24. Object Storage Scaling

Videos should use object storage rather than application-server disks.

Example:

```text
Object Storage
├── Original
├── Proxy
├── HLS
├── Subtitles
├── Thumbnails
└── Generated Assets
```

Object storage scales independently from API servers.

---

# 25. CDN

Student video delivery should use a CDN.

```text
Student
   ↓
CDN
   ↓
Object Storage
```

Benefits:

```text
Lower latency
Reduced origin load
Better global delivery
Better scalability
```

---

# 26. Video Streaming Scaling

Do not stream large video files directly through the API server.

Bad:

```text
Student → API → Video file
```

Better:

```text
Student → CDN → Video segments
```

---

# 27. HLS Scaling

Recommended streaming structure:

```text
Master Playlist
       ↓
1080p
720p
480p
360p
       ↓
Video Segments
```

CDN caches segments close to users.

---

# 28. Adaptive Bitrate

Player can select quality based on:

```text
Network
Device
Bandwidth
Buffer health
Entitlement
```

---

# 29. Database Scaling

Start simple:

```text
Application
    ↓
Primary Database
```

Scale later:

```text
Primary
  ├── Read Replica 1
  ├── Read Replica 2
  └── Read Replica N
```

---

# 30. Database Read Scaling

Read-heavy operations:

```text
Lesson browsing
Course catalog
Analytics dashboards
Transcript access
```

can potentially use read replicas or dedicated analytics storage.

---

# 31. Database Write Scaling

Important writes:

```text
Progress
Question attempts
Lesson completion
Jobs
User updates
```

should initially use a primary database.

More advanced sharding can be introduced only when justified by scale.

---

# 32. Database Connection Pooling

Each API instance should use connection pooling.

Without pooling:

```text
100 API instances
 ×
Many DB connections
 =
Database overload
```

Connection limits must be managed centrally.

---

# 33. Database Indexing

Important indexes:

```text
user_id
tenant_id
course_id
lesson_id
question_id
created_at
status
```

Indexes should be driven by real query patterns.

---

# 34. Caching

Use caching for frequently accessed data.

Candidates:

```text
Course metadata
Lesson metadata
Published manifest
Feature configuration
Authorization-related data where safe
```

---

# 35. Cache Architecture

```text
Request
  ↓
Cache
 ├── HIT → Return
 └── MISS
       ↓
    Database
       ↓
    Cache
       ↓
    Return
```

---

# 36. Cache Invalidation

When content changes:

```text
Lesson Updated
     ↓
Invalidate Cache
     ↓
New Manifest
```

Published content should be versioned to make cache behavior predictable.

---

# 37. Redis / Distributed Cache

A distributed cache may support:

```text
Session-related state
Rate limiting
Short-lived locks
Caching
Job coordination
```

It should not become the permanent source of truth for important learning records.

---

# 38. Distributed Locking

Some jobs may require:

```text
Only one worker
```

Example:

```text
Generate Lesson Manifest
```

Use a distributed lock or database uniqueness constraint where appropriate.

---

# 39. Analytics Scaling

Analytics traffic can become very large.

Do not put every analytics event directly into the main transactional database.

Preferred:

```text
Student
 ↓
Event Collector
 ↓
Queue / Stream
 ↓
Analytics Storage
```

---

# 40. Event Processing Scaling

Processors can scale horizontally:

```text
Processor 1
Processor 2
Processor 3
...
Processor N
```

Partitioning can preserve useful event ordering where required.

---

# 41. Dashboard Scaling

Teacher dashboards may create expensive queries.

Use:

```text
Pre-aggregated metrics
Materialized views
Caching
Analytics database
Pagination
```

---

# 42. Search Scaling

As content grows:

```text
Courses
Lessons
Transcripts
Questions
```

may require a dedicated search system.

Potential capabilities:

```text
Full-text search
Transcript search
Course search
Semantic search
```

Search infrastructure should be introduced when database search is no longer sufficient.

---

# 43. Notification Scaling

Notifications should be asynchronous.

```text
Event
 ↓
Notification Queue
 ↓
Notification Worker
 ↓
Email / Push / In-App
```

---

# 44. Multi-Region Strategy

Initial deployment:

```text
Single Primary Region
+
CDN
```

Later:

```text
Region A
Region B
Region C
```

The architecture should avoid assuming multi-region from day one.

---

# 45. Global Video Delivery

Even with one backend region:

```text
Backend
   ↓
Object Storage
   ↓
Global CDN
   ↓
Students
```

can provide broad geographic video delivery.

---

# 46. High Availability

Critical services should avoid single points of failure.

Example:

```text
Load Balancer
      ↓
API-1  API-2  API-3
```

If API-2 fails:

```text
Traffic
 ↓
API-1 / API-3
```

---

# 47. Database Availability

Use managed database high-availability features where available.

Target architecture:

```text
Primary
   ↓
Standby
```

with automated failover where appropriate.

---

# 48. Queue Availability

Queue infrastructure should provide:

```text
Durability
Retry
Visibility timeout / lease behavior
Dead-letter handling
Monitoring
```

---

# 49. Storage Availability

Critical assets should use durable object storage.

Important:

```text
Original MP4
Processed video
HLS segments
Subtitles
Lesson assets
```

---

# 50. Disaster Recovery

Disaster recovery must cover:

```text
Database
Object storage
Configuration
Secrets
Queue state where necessary
```

---

# 51. Recovery Objectives

Define:

```text
RPO — Recovery Point Objective
RTO — Recovery Time Objective
```

Example targets for an initial production deployment:

```text
RPO: ≤ 1 hour
RTO: ≤ 4 hours
```

These are planning targets and must be validated against infrastructure and business requirements.

---

# 52. Backup Strategy

```text
Database
 ↓
Automated Backup
 ↓
Retention
 ↓
Recovery Testing
```

Backups should be stored separately from the primary failure domain where practical.

---

# 53. Storage Lifecycle

Large media files may use lifecycle policies:

```text
Hot Storage
     ↓
Less Frequently Accessed
     ↓
Archive
```

Do not archive assets that must support low-latency playback unless the playback architecture supports restoration.

---

# 54. Cost-Aware Scaling

Scaling should optimize:

```text
Performance
+
Reliability
+
Cost
```

Avoid:

```text
Always-on maximum infrastructure
```

when workloads are variable.

---

# 55. Processing Priority

Jobs can have priority.

Example:

```text
HIGH
 └── Teacher Preview

NORMAL
 └── Published Lesson

LOW
 └── Bulk Regeneration
```

---

# 56. Priority Queue

```text
                Job Queue
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
      HIGH       NORMAL        LOW
        │           │           │
        └───────────┼───────────┘
                    ▼
                  Worker
```

---

# 57. Resource Isolation

Separate resources for:

```text
API
Video Processing
AI Processing
Analytics
```

so one workload cannot consume all platform capacity.

---

# 58. CPU vs GPU

Most workloads:

```text
API → CPU
FFmpeg → CPU
Database → CPU / memory
Analytics → CPU
```

Some AI workloads may benefit from:

```text
GPU
```

GPU workers should be used only when the selected models/workloads require them.

---

# 59. Scaling MP4 Processing

Example:

```text
100 uploaded videos

Queue:
100 jobs

Workers:
10

Each worker:
10 jobs

Result:
Controlled parallel processing
```

Actual concurrency should be benchmarked.

---

# 60. Processing Progress

Each job should expose:

```text
queued
processing
stage
progress
completed
failed
```

Example:

```text
Video Processing

Upload       ✓
Extract      ✓
Transcribe   65%
Translate    Pending
Questions    Pending
Publish      Pending
```

---

# 61. Processing Pipeline Scaling

```text
MP4
 ↓
Extract Audio
 ↓
Transcription
 ↓
Segmentation
 ↓
Translation
 ↓
Concept Extraction
 ↓
Question Generation
 ↓
Validation
 ↓
Manifest
```

Each stage can be represented as a job or workflow step.

---

# 62. Pipeline Parallelism

Some tasks can run in parallel.

Example:

```text
Transcript
    │
    ├── Translation
    ├── Summary
    ├── Concept Extraction
    └── Question Generation
```

Only tasks with true dependencies should wait.

---

# 63. Workflow Orchestration

For complex pipelines use a workflow/orchestration mechanism.

Example conceptual flow:

```text
UPLOAD_COMPLETE
      ↓
MEDIA_ANALYSIS
      ↓
TRANSCRIPTION
      ↓
CONTENT_ANALYSIS
      ↓
┌─────┼─────┐
▼     ▼     ▼
TRANSLATE SUMMARY QUESTIONS
└─────┼─────┘
      ▼
VALIDATION
      ↓
TEACHER_REVIEW
      ↓
PUBLISH
```

---

# 64. Backpressure

If downstream AI processing is overloaded:

```text
Upload
 ↓
Queue
 ↓
Wait
```

rather than:

```text
Upload
 ↓
Create unlimited workers
 ↓
System crash
```

---

# 65. Concurrency Limits

Set limits for:

```text
Video jobs
AI requests
Database connections
External API calls
File processing
```

---

# 66. API Traffic Protection

If traffic suddenly spikes:

```text
Traffic Spike
     ↓
Rate Limit
     ↓
Queue
     ↓
Autoscaling
```

Important learning operations should remain available.

---

# 67. CDN Cache Strategy

Cache aggressively for immutable assets:

```text
Video segments
Thumbnails
Published static assets
```

Use versioned paths to avoid stale content.

---

# 68. Versioned Assets

Example:

```text
lesson_001/v1/manifest.json
lesson_001/v2/manifest.json
```

This simplifies:

```text
Cache invalidation
Rollback
Publishing
```

---

# 69. Rollback

If a published lesson version is broken:

```text
v3
 ↓
Problem
 ↓
Rollback
 ↓
v2
```

Previous published versions should remain available according to retention policy.

---

# 70. Blue-Green Deployment

For critical releases:

```text
Blue → Current
Green → New
```

Test Green:

```text
Green ✓
```

Then switch traffic.

---

# 71. Canary Deployment

For higher-risk changes:

```text
95% → Existing
5%  → New
```

Monitor:

```text
Errors
Latency
Completion
```

Then increase traffic gradually.

---

# 72. Observability

Monitor:

```text
CPU
Memory
Latency
Errors
Queue depth
AI latency
Video processing time
Database load
CDN performance
Storage usage
```

---

# 73. Service-Level Metrics

Important metrics:

```text
API p95 latency
API error rate
Video startup time
Processing job duration
Queue wait time
AI failure rate
Database latency
```

---

# 74. Scalability Alerts

Examples:

```text
CPU > threshold
Queue depth increasing
Database connections near limit
AI quota nearly exhausted
Storage capacity growing rapidly
Error rate increasing
```

---

# 75. Performance Targets

Initial planning targets:

```text
API p95 latency:
< 500 ms for normal read operations

Critical API availability:
≥ 99.9%

Video startup:
Target < 3 seconds under normal network conditions

Job queue:
No uncontrolled backlog

```

Targets must be validated through load testing and adjusted to real infrastructure.

---

# 76. Load Testing

Test scenarios:

```text
100 concurrent users
1,000 concurrent users
10,000 concurrent users
```

and:

```text
10 concurrent video jobs
100 concurrent video jobs
1,000 queued video jobs
```

Exact production limits should be established through benchmark results.

---

# 77. Stress Testing

Push beyond expected capacity:

```text
Normal
 ↓
High
 ↓
Very High
 ↓
Failure Point
```

Observe:

```text
What fails first?
Does the system recover?
Are data writes safe?
```

---

# 78. Scalability Bottleneck Analysis

Potential bottlenecks:

```text
Database
AI provider
Video transcoding
Object storage
Queue
Network
Analytics queries
```

Monitor and optimize the actual bottleneck rather than prematurely optimizing every component.

---

# 79. Scaling Strategy by Stage

## Stage 1 — MVP

```text
1 API
1 Worker
Managed DB
Object Storage
CDN
Basic Queue
```

---

## Stage 2 — Growth

```text
Multiple API instances
Multiple workers
Redis/cache
Read replica
Analytics storage
Autoscaling
```

---

## Stage 3 — Large Platform

```text
Multiple worker pools
Advanced queueing
Dedicated analytics infrastructure
Search infrastructure
Multi-region options
Advanced observability
```

---

# 80. Recommended Initial Architecture

For AILPG MVP:

```text
Flutter / Web
       ↓
CDN / WAF
       ↓
API
       ↓
Managed Database
       │
       ├── Object Storage
       │
       ├── Queue
       │
       └── Workers
             ├── Video
             ├── AI
             └── Analytics
```

Keep the first implementation simple enough to operate.

---

# 81. Scaling Principles

```text
Scale horizontally first
Use queues for heavy work
Use CDN for video
Use object storage for media
Keep APIs stateless
Cache read-heavy data
Separate analytics
Isolate workers
Use autoscaling
Monitor before optimizing
```

---

# 82. Definition of Done

```text
Horizontal API Scaling       ✓
Load Balancing               ✓
Autoscaling                  ✓
Queue Architecture           ✓
Worker Scaling               ✓
Video Scaling                ✓
AI Scaling                   ✓
CDN                          ✓
Object Storage               ✓
Database Scaling             ✓
Caching                      ✓
Analytics Scaling            ✓
Backpressure                 ✓
Retry / DLQ                  ✓
High Availability            ✓
Backup / Recovery            ✓
Load Testing                 ✓
Observability                ✓
Cost-Aware Scaling           ✓
```

---

# 83. Next Document

```text
12_System_Design_Appendix.md
```

This will close **Layer 3 — System Design** with:

```text
Architecture Decisions
Technology Decision Matrix
Non-Functional Requirements
Environment Architecture
Deployment Topology
Failure Scenarios
Sequence Diagrams
Data Flow Summary
System Design Checklist
```

---

# 84. Revision History

| Version | Date       | Description                |
| ------- | ---------- | -------------------------- |
| 1.0.0   | 2026-08-11 | Initial scalability design |

````

**Next → `12_System_Design_Appendix.md` — System Design layer-ஐ complete செய்வோம்.**
