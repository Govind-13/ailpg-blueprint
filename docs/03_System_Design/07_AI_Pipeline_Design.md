---
document_id: SD-007
title: AI Pipeline Design
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: AI Architecture Team
parent_document: SD-006
last_updated: 2026-08-11
---

# AI Pipeline Design

## 1. Purpose

This document defines the complete AI processing pipeline used by AILPG to transform an educational MP4 video into structured, searchable, translatable, and interactive learning content.

The core transformation is:

```text
MP4
 ↓
Audio + Frames
 ↓
Speech-to-Text
 ↓
Transcript
 ↓
OCR
 ↓
Mathematical Formula Recognition
 ↓
Content Understanding
 ↓
Topic Detection
 ↓
Learning Objectives
 ↓
Question Generation
 ↓
Question Validation
 ↓
Translation
 ↓
Interactive Checkpoints
 ↓
Lesson Manifest
 ↓
Teacher Review
 ↓
Publish
```

---

# 2. AI Pipeline Goals

The AI system must:

```text
Understand educational video content
Extract speech
Extract visible text
Recognize mathematical expressions
Identify concepts
Generate structured learning content
Generate questions
Generate explanations
Translate content
Create interactive checkpoints
Provide confidence scores
Support human review
Track AI versions
Control AI cost
```

---

# 3. AI Architecture

```text
                       VIDEO
                         │
             ┌───────────┴───────────┐
             ▼                       ▼
          AUDIO                    FRAMES
             │                       │
             ▼                       ├────► OCR
            STT                      │
             │                       └────► Formula
             ▼
        TRANSCRIPT
             │
             └───────────┬───────────┘
                         ▼
                  CONTENT MERGER
                         │
                         ▼
                  AI UNDERSTANDING
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
       Topics       Objectives       Concepts
          │              │              │
          └──────────────┼──────────────┘
                         ▼
                 QUESTION GENERATOR
                         │
                         ▼
                  QUESTION VALIDATOR
                         │
                         ▼
                    TRANSLATOR
                         │
                         ▼
              CHECKPOINT GENERATOR
                         │
                         ▼
                  LESSON MANIFEST
                         │
                         ▼
                  HUMAN REVIEW
```

---

# 4. AI Pipeline Stages

The pipeline consists of:

```text
AI-01 Audio Processing
AI-02 Speech-to-Text
AI-03 Transcript Normalization
AI-04 OCR
AI-05 Formula Recognition
AI-06 Content Fusion
AI-07 Topic Detection
AI-08 Learning Objective Generation
AI-09 Question Generation
AI-10 Question Validation
AI-11 Explanation Generation
AI-12 Translation
AI-13 Checkpoint Generation
AI-14 Lesson Manifest Generation
AI-15 Quality Evaluation
```

---

# 5. AI Job Architecture

Every AI stage should execute as an independent job.

Example:

```text
AI_JOB
├── job_id
├── video_id
├── lesson_id
├── job_type
├── input_reference
├── output_reference
├── provider
├── model
├── prompt_version
├── status
├── confidence
├── retry_count
├── token_usage
├── cost
├── started_at
└── completed_at
```

---

# 6. AI Job States

```text
QUEUED
 ↓
PROCESSING
 ↓
VALIDATING
 ↓
COMPLETED
```

Failure:

```text
PROCESSING
 ↓
FAILED
 ↓
RETRYING
```

Repeated failure:

```text
DEAD_LETTER
```

---

# 7. Provider Abstraction

AI providers must not be hard-coded into business logic.

Recommended abstraction:

```text
AILPG AI Service
      │
      ├── Speech Provider
      ├── Vision Provider
      ├── LLM Provider
      └── Translation Provider
```

The platform should be able to switch providers without rewriting the complete pipeline.

---

# 8. Model Routing

Different tasks may use different models.

```text
Speech
  → Speech model

OCR
  → Vision/OCR model

Formula
  → Vision + Math model

Topic
  → LLM

Question generation
  → LLM

Translation
  → Translation model/service
```

---

# 9. AI Model Registry

Maintain a model registry.

Example:

```text
model_id
provider
model_name
task
version
enabled
cost_per_input
cost_per_output
quality_score
```

Example:

```json
{
  "task": "question_generation",
  "provider": "provider_a",
  "model": "model_x",
  "enabled": true
}
```

---

# 10. Speech-to-Text Pipeline

```text
Audio
 ↓
Audio Validation
 ↓
Speech-to-Text
 ↓
Raw Transcript
 ↓
Timestamp Normalization
 ↓
Speaker / Segment Processing
 ↓
Transcript Quality Check
 ↓
Structured Transcript
```

---

# 11. Transcript Structure

Example:

```json
{
  "segments": [
    {
      "start": 0.0,
      "end": 5.2,
      "text": "Today we are solving a quadratic equation.",
      "confidence": 0.96
    }
  ]
}
```

---

# 12. Transcript Normalization

Normalization may include:

```text
Remove unnecessary repetition
Correct obvious recognition errors
Normalize punctuation
Normalize numbers
Preserve mathematical terms
Preserve timestamps
```

The original transcript must be retained.

---

# 13. Transcript Versioning

Store:

```text
raw transcript
normalized transcript
teacher-edited transcript
published transcript
```

Example:

```text
Transcript v1 — AI
Transcript v2 — Teacher edited
Transcript v3 — Published
```

---

# 14. OCR Pipeline

```text
Frames
 ↓
Frame Filtering
 ↓
Text Detection
 ↓
OCR
 ↓
Text Normalization
 ↓
Timestamp Mapping
 ↓
Confidence
```

---

# 15. OCR Output

Example:

```json
{
  "frame_id": "frame_001",
  "timestamp": 42.5,
  "text": "x² + 5x + 6 = 0",
  "confidence": 0.91
}
```

---

# 16. Mathematical Formula Recognition

Math content requires separate processing.

```text
Frame
 ↓
Math Region Detection
 ↓
Formula Recognition
 ↓
LaTeX
 ↓
Validation
 ↓
Normalized Formula
```

Example:

```text
Image:

x² + 5x + 6 = 0

Output:

x^2 + 5x + 6 = 0
```

---

# 17. Formula Validation

Formula validation should check:

```text
Syntax
Variable consistency
Operators
Parentheses
Numerical values
Equation structure
```

Where possible, mathematical expressions should also undergo symbolic or computational validation.

---

# 18. Formula Confidence

Example:

```json
{
  "formula": "x^2 + 5x + 6 = 0",
  "confidence": 0.94
}
```

Low-confidence formulas:

```text
confidence < threshold
```

should be flagged for review rather than silently accepted.

---

# 19. Content Fusion

The AI system combines:

```text
Transcript
+
OCR
+
Formula
+
Timestamp
```

into a unified content timeline.

Example:

```text
00:00 Speech
00:05 Speech
00:10 Formula appears
00:12 Explanation
00:25 Formula changes
00:30 Example
```

---

# 20. Unified Content Timeline

Example:

```json
{
  "timestamp": 42.5,
  "speech": "Factor the equation.",
  "visual_text": "x² + 5x + 6 = 0",
  "formula": "x^2 + 5x + 6 = 0"
}
```

This timeline becomes the main AI input for educational understanding.

---

# 21. Content Segmentation

The AI should divide the video into meaningful educational segments.

Possible segment types:

```text
INTRODUCTION
CONCEPT
EXPLANATION
EXAMPLE
FORMULA
STEP
QUESTION
SUMMARY
CONCLUSION
```

---

# 22. Concept Detection

Example:

```text
Video
 ↓
Content Understanding
 ↓
Concepts
```

Output:

```text
Quadratic equation
Factorization
Roots
Zero-product property
```

---

# 23. Topic Detection

Example:

```json
{
  "topic": "Quadratic Equations",
  "subtopics": [
    "Factorization",
    "Finding Roots"
  ]
}
```

---

# 24. Learning Objective Generation

AI generates measurable objectives.

Example:

```text
By the end of this lesson, the student should be able to:

1. Identify a quadratic equation.
2. Factor a quadratic expression.
3. Find roots using factorization.
4. Verify the solution.
```

Objectives should be linked to lesson content.

---

# 25. Difficulty Classification

Each concept and question may receive:

```text
EASY
MEDIUM
HARD
```

Future:

```text
BEGINNER
INTERMEDIATE
ADVANCED
```

---

# 26. Question Generation

Question generation uses:

```text
Transcript
OCR
Formula
Concept
Learning Objective
Difficulty
Timestamp
```

Flow:

```text
Content
 ↓
Question Generator
 ↓
Draft Question
 ↓
Answer Generator
 ↓
Explanation Generator
 ↓
Validator
```

---

# 27. Question Types

MVP:

```text
Multiple Choice
True / False
Short Answer
```

Future:

```text
Fill in the Blank
Numeric Answer
Formula Input
Ordering
Matching
Step Ordering
```

---

# 28. Multiple Choice Example

Input:

```text
x² + 5x + 6 = 0
```

Generated:

```text
What are the roots?

A. 1, 6
B. -2, -3
C. 2, 3
D. -1, -6
```

Correct:

```text
B
```

---

# 29. Question Metadata

Each question should contain:

```text
question_id
lesson_id
concept_id
type
difficulty
question
options
correct_answer
explanation
source_timestamp
confidence
ai_model
prompt_version
```

---

# 30. Question Validation

Every generated question must pass:

```text
Completeness
Correct answer
Distractor quality
Question clarity
Content relevance
Difficulty validity
Formula validity
Language quality
```

---

# 31. Answer Validation

For mathematical questions:

```text
Exact comparison
Numeric normalization
Symbolic comparison
Equivalent expression detection
```

Example:

```text
2/4
```

and:

```text
1/2
```

may be mathematically equivalent.

---

# 32. Question Confidence

Example:

```json
{
  "confidence": 0.93
}
```

Suggested classification:

```text
HIGH
MEDIUM
LOW
```

Thresholds must be configurable and calibrated using evaluation data.

---

# 33. Low-Confidence AI Output

```text
AI Output
 ↓
Confidence Check
 │
 ├── HIGH → Continue
 │
 ├── MEDIUM → Review Flag
 │
 └── LOW → Regenerate / Human Review
```

---

# 34. Question Regeneration

If validation fails:

```text
Question
 ↓
Validator
 ↓
FAIL
 ↓
Regenerate
 ↓
Validate
```

Maximum attempts must be configurable.

---

# 35. Explanation Generation

Each question may include:

```text
Correct answer
Explanation
Step-by-step reasoning
Relevant formula
Related concept
```

Example:

```text
x² + 5x + 6
= (x + 2)(x + 3)

Therefore:

x = -2
x = -3
```

---

# 36. AI Explanation Safety

AI-generated explanations should not automatically be treated as mathematically correct.

They should pass:

```text
Formula validation
Answer validation
Teacher review
```

for high-stakes published educational content.

---

# 37. Translation Pipeline

```text
Approved Source
      ↓
Language Detection
      ↓
Translation
      ↓
Formula Protection
      ↓
Translation Validation
      ↓
Localized Content
```

---

# 38. Translation Requirements

Translation must preserve:

```text
Numbers
Variables
Formulas
Timestamps
Question structure
Answer choices
Meaning
Formatting
```

---

# 39. Supported Languages

Initial architecture should support configurable language codes.

Example:

```text
en-IN
ta-IN
hi-IN
te-IN
ml-IN
kn-IN
```

The actual launch languages should be controlled by product requirements and available translation quality.

---

# 40. Formula Protection

Before translation:

```text
x² + 5x + 6 = 0
```

should be protected as a mathematical token.

Conceptually:

```text
Translate:
"Find the roots of [FORMULA_001]"

Restore:
"Find the roots of x² + 5x + 6 = 0"
```

This prevents accidental formula corruption.

---

# 41. Interactive Checkpoint Generation

The AI identifies natural interruption points.

Example:

```text
Video
──────────────────────────────────────
00:00     01:20      02:45      04:10
                     ▲
                     │
                 Checkpoint
```

---

# 42. Checkpoint Selection Rules

Prefer checkpoints after:

```text
Concept explanation
Important formula
Solved step
Example
Key definition
Common mistake
```

Avoid excessive interruption.

---

# 43. Checkpoint Metadata

```json
{
  "checkpoint_id": "cp_001",
  "timestamp": 165.5,
  "question_id": "q_001",
  "pause_video": true,
  "required": false
}
```

---

# 44. Checkpoint Quality

A checkpoint should:

```text
Test recently explained content
Have a clear answer
Not require unseen information
Be appropriately difficult
Not interrupt a critical explanation
```

---

# 45. AI Lesson Structure

AI should produce:

```text
Lesson
├── Introduction
├── Learning Objectives
├── Video
├── Transcript
├── Concepts
├── Formulas
├── Checkpoints
├── Questions
├── Explanations
├── Summary
└── Translation
```

---

# 46. Lesson Manifest Generation

The AI pipeline produces structured lesson data.

```text
AI Outputs
    ↓
Normalizer
    ↓
Schema Validator
    ↓
Lesson Manifest
```

---

# 47. AI Output Schema

Every AI stage should return structured output.

Avoid relying on unstructured prose.

Example:

```json
{
  "status": "success",
  "data": {},
  "confidence": 0.94,
  "model": "model_x",
  "prompt_version": "v3"
}
```

---

# 48. Prompt Architecture

Prompts should be version controlled.

```text
prompts/
├── transcript/
├── topic/
├── objective/
├── question/
├── explanation/
├── translation/
└── checkpoint/
```

---

# 49. Prompt Versioning

Example:

```text
question_generation_v1
question_generation_v2
question_generation_v3
```

Every generated result must record the prompt version.

---

# 50. Prompt Variables

Prompts should use structured variables.

Example:

```text
{{topic}}
{{transcript}}
{{formula}}
{{difficulty}}
{{language}}
{{learning_objective}}
```

Do not construct prompts through uncontrolled string concatenation.

---

# 51. Context Window Management

Long videos may produce large transcripts.

Use:

```text
Chunking
Summarization
Hierarchical analysis
Relevant-context retrieval
```

Pipeline:

```text
Long Transcript
 ↓
Chunks
 ↓
Chunk Analysis
 ↓
Intermediate Summaries
 ↓
Global Analysis
```

---

# 52. Chunking Strategy

Example:

```text
Video
 ↓
5-minute segments
 ↓
Transcript chunks
 ↓
Local AI analysis
 ↓
Global lesson analysis
```

Chunk size must be configurable according to the selected model's context limits and cost.

---

# 53. Hierarchical AI Analysis

```text
Raw Video Content
       ↓
Segment Analysis
       ↓
Section Summaries
       ↓
Lesson Summary
       ↓
Learning Objectives
       ↓
Question Generation
```

This reduces context size and improves consistency for long videos.

---

# 54. AI Memory

The pipeline should not assume the model remembers previous requests.

Context must be explicitly provided through:

```text
Database
Object Storage
Vector Retrieval where useful
Prompt Context
```

---

# 55. Retrieval-Augmented AI

Future enhancement:

```text
Lesson Content
 ↓
Embeddings
 ↓
Vector Database
 ↓
Semantic Search
 ↓
Relevant Context
 ↓
AI
```

Useful for:

```text
Question generation
Student explanations
Lesson search
Teacher assistant
```

---

# 56. AI Cost Tracking

Each job records:

```text
Input tokens
Output tokens
Audio duration
Image count
Model
Provider
Cost
```

Example:

```json
{
  "job": "question_generation",
  "input_tokens": 12000,
  "output_tokens": 2500,
  "estimated_cost": 0.04
}
```

---

# 57. Cost Limits

Organization plans may have:

```text
Monthly AI credits
Maximum video minutes
Maximum AI jobs
Translation quota
```

Before expensive processing:

```text
Check entitlement
 ↓
Check quota
 ↓
Process
```

---

# 58. AI Provider Failure

```text
Primary Provider
      ↓
Failure
      ↓
Retry
      ↓
Fallback Provider
      ↓
Continue
```

Fallback must only occur when the alternative provider supports the same required task and output contract.

---

# 59. AI Timeout

Every AI job must have:

```text
Request timeout
Job timeout
Retry policy
Cancellation support
```

---

# 60. AI Hallucination Control

The AI should be grounded in extracted source content.

Use:

```text
Transcript
OCR
Formula
Video timestamps
Retrieved lesson context
```

AI should not invent facts that are unrelated to the source lesson.

---

# 61. Grounding Rules

Question generation:

```text
Question must be answerable from source content.
```

Explanation:

```text
Explanation must be consistent with verified answer.
```

Translation:

```text
Translation must preserve source meaning.
```

---

# 62. AI Evaluation

Before publishing, evaluate:

```text
Transcript quality
OCR quality
Formula accuracy
Topic accuracy
Question accuracy
Translation quality
Checkpoint quality
```

---

# 63. Automated Evaluation

Possible metrics:

```text
STT confidence
OCR confidence
Formula validation success
Question validator score
Translation quality score
Schema validity
Duplicate question rate
```

---

# 64. Human-in-the-Loop

AI-generated content should enter:

```text
READY_FOR_REVIEW
```

before publication where required.

Teacher can:

```text
Approve
Edit
Reject
Regenerate
```

---

# 65. Review Priority

Flag for review:

```text
Low confidence
Formula uncertainty
Incorrect answer risk
Translation uncertainty
Missing transcript
Question validation failure
```

---

# 66. AI Audit Trail

Every AI-generated object should retain:

```text
provider
model
prompt_version
input_reference
output_reference
timestamp
confidence
token_usage
cost
```

This enables reproducibility and debugging.

---

# 67. AI Reprocessing

Teacher/Admin can reprocess individual stages.

Example:

```text
OCR
 ↓
Regenerate
```

without reprocessing the complete video.

---

# 68. Partial Pipeline Execution

Example:

```text
Video
✓ STT
✓ OCR
✓ Formula
✓ Topic

Question Generation
FAILED
```

Only the failed stage should need to restart.

---

# 69. AI Pipeline Dependency Graph

```text
VIDEO
 │
 ├──► AUDIO ──► STT ─────────┐
 │                            │
 └──► FRAMES                  │
       │                      │
       ├──► OCR ──────────────┤
       │                      │
       └──► FORMULA ──────────┤
                              ▼
                        CONTENT FUSION
                              │
                     ┌────────┼────────┐
                     ▼        ▼        ▼
                   TOPIC   OBJECTIVE CONCEPT
                     │        │        │
                     └────────┼────────┘
                              ▼
                      QUESTION GENERATION
                              │
                              ▼
                       QUESTION VALIDATION
                              │
                              ▼
                         TRANSLATION
                              │
                              ▼
                       CHECKPOINTS
                              │
                              ▼
                      LESSON MANIFEST
```

---

# 70. AI Pipeline Status

Lesson-level status:

```text
UPLOADED
VALIDATING
EXTRACTING
ANALYZING
GENERATING
TRANSLATING
READY_FOR_REVIEW
APPROVED
PUBLISHED
FAILED
```

---

# 71. AI Security

AI system must protect:

```text
API keys
Provider credentials
Student data
Teacher content
Private videos
Organization data
Prompt templates
```

Do not send unnecessary personal information to external AI providers.

---

# 72. Data Privacy

AI requests should use the minimum data necessary.

For example:

```text
Question Generation
```

may require:

```text
Transcript
Formula
Topic
```

but does not require:

```text
Student email
Password
Billing information
```

---

# 73. Prompt Injection Protection

Video/transcript content must be treated as untrusted input.

Example:

```text
Transcript:
"Ignore previous instructions..."
```

must not override the system's AI instructions.

Use:

```text
System instructions
+
Structured content fields
+
Explicit content boundaries
+
Output schema validation
```

---

# 74. AI Output Validation

AI output must be parsed and validated against schemas.

```text
AI Response
 ↓
JSON Parser
 ↓
Schema Validation
 ↓
Business Validation
 ↓
Store
```

Invalid output:

```text
Reject
 ↓
Retry / Regenerate
```

---

# 75. Duplicate Question Detection

Generated questions should be compared for semantic duplication.

```text
Question A
Question B
Question C
       ↓
Similarity Detection
       ↓
Duplicate?
```

Duplicate questions can be removed or regenerated.

---

# 76. Question Diversity

A lesson should avoid generating:

```text
Same concept
Same wording
Same answer
Same distractors
```

repeatedly.

Question generation should consider:

```text
Concept coverage
Difficulty distribution
Question type distribution
```

---

# 77. Adaptive Question Generation

Future feature:

```text
Student Performance
       ↓
Weak Concepts
       ↓
AI
       ↓
Personalized Questions
```

This is separate from initial lesson generation.

---

# 78. AI Pipeline Observability

Monitor:

```text
Job latency
Success rate
Failure rate
Retry count
Token usage
Cost
Confidence
Provider availability
```

---

# 79. AI Definition of Done

```text
STT                         ✓
Transcript Normalization    ✓
OCR                         ✓
Formula Recognition         ✓
Content Fusion              ✓
Topic Detection             ✓
Learning Objectives         ✓
Question Generation         ✓
Question Validation         ✓
Explanation Generation      ✓
Translation                 ✓
Checkpoint Generation       ✓
Manifest Generation         ✓
Confidence Scoring          ✓
Human Review                ✓
AI Versioning               ✓
Cost Tracking               ✓
Provider Fallback           ✓
Audit Trail                 ✓
```

---

# 80. Next Document

```text
08_Interactive_Lesson_Design.md
```

This document will define the actual **student learning experience**:

```text
Lesson Introduction
        ↓
Video Player
        ↓
Transcript
        ↓
Zoom
        ↓
Language Switch
        ↓
Interactive Checkpoint
        ↓
Question
        ↓
Student Answer
        ↓
Instant Feedback
        ↓
Explanation
        ↓
Resume Video
        ↓
Progress Tracking
        ↓
Lesson Completion
```

It will also define:

```text
Lesson Manifest
Interactive timeline
Question engine
Checkpoint engine
Player state
Student progress
Offline/resume behavior
Accessibility
Responsive UI
```

---

# 81. Revision History

| Version | Date | Description |
|---|---|---|
| 1.0.0 | 2026-08-11 | Initial AI pipeline design |
