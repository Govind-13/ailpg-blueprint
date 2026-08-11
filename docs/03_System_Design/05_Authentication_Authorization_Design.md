---
document_id: SD-005
title: Authentication and Authorization Design
project: AILPG (AI Learning Platform Generator)
version: 1.0.0
status: Draft
owner: Solution Architecture Team
parent_document: SD-004
last_updated: 2026-08-11
---

# Authentication and Authorization Design

## 1. Purpose

This document defines how AILPG authenticates users, manages sessions, authorizes actions, isolates organizations, and enforces subscription-based access.

The security model covers:

```text
Authentication
Authorization
RBAC
Multi-Tenancy
Session Management
Token Management
API Security
Resource Authorization
Subscription Entitlements
Audit Logging
```

---

# 2. Security Architecture

```text
                    USER
                     │
                     ▼
              ┌──────────────┐
              │ Login / OAuth │
              └──────┬───────┘
                     │
                     ▼
              ┌──────────────┐
              │ Auth Service │
              └──────┬───────┘
                     │
                     ▼
              Identity / Session
                     │
                     ▼
              API Authentication
                     │
                     ▼
              Authorization
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
       RBAC       Tenant       Entitlement
        │          Check          Check
        └────────────┼────────────┘
                     ▼
                Application
```

---

# 3. Supported User Types

Initial roles:

```text
STUDENT
TEACHER
ORG_ADMIN
PLATFORM_ADMIN
```

Future roles:

```text
CONTENT_REVIEWER
SUPPORT_AGENT
ANALYST
INSTRUCTOR
```

---

# 4. Role Definitions

## STUDENT

Can:

```text
Browse published courses
Open authorized lessons
Watch videos
Answer questions
View progress
Manage own profile
```

Cannot:

```text
Create courses
Upload videos
Publish lessons
Manage users
Access admin APIs
```

---

## TEACHER

Can:

```text
Create courses
Create lessons
Upload videos
Start processing
Review AI results
Edit questions
Review translations
Publish permitted content
View course analytics
```

---

## ORG_ADMIN

Can:

```text
Manage organization
Manage organization users
Assign roles
View organization analytics
Manage organization courses
Manage organization settings
```

---

## PLATFORM_ADMIN

Can:

```text
Manage platform users
Manage organizations
Monitor jobs
Retry failed jobs
Manage platform settings
View audit logs
Manage subscriptions
View system health
```

---

# 5. Authentication Methods

Initial authentication:

```text
Email + Password
```

Recommended future options:

```text
Google OAuth
Microsoft OAuth
Apple Sign-In
Phone OTP
Passkeys
```

Authentication providers must be abstracted so additional providers can be added without changing business logic.

---

# 6. Registration Flow

```text
User
 ↓
Registration Form
 ↓
API
 ↓
Validate Input
 ↓
Check Existing Account
 ↓
Hash Password
 ↓
Create User
 ↓
Create Verification Token
 ↓
Send Verification
 ↓
Account Created
```

---

# 7. Email Verification

```text
Registration
     ↓
Verification Token
     ↓
Email
     ↓
User Clicks Link
     ↓
API
     ↓
Validate Token
     ↓
Mark Email Verified
```

Invalid or expired token:

```text
Verification Failed
```

---

# 8. Login Flow

```text
User
 ↓
Email + Password
 ↓
API
 ↓
Find User
 ↓
Verify Password
 ↓
Check Account Status
 ↓
Create Session
 ↓
Issue Access Credentials
 ↓
Authenticated
```

---

# 9. Failed Login

Repeated failures should trigger protection.

```text
Failed Login
 ↓
Record Attempt
 ↓
Rate Limit
 ↓
Temporary Lock / Challenge
```

The exact threshold must be configurable.

---

# 10. Session Architecture

A session represents an authenticated user context.

Session should contain:

```text
Session ID
User ID
Created At
Last Active
Expiration
Device Metadata
Revoked At
```

Sessions must be revocable.

---

# 11. Access Token

The access credential is short-lived.

Conceptual claims:

```json
{
  "sub": "user_001",
  "session_id": "session_001",
  "role": "TEACHER",
  "organization_id": "org_001",
  "iat": 0,
  "exp": 0
}
```

Do not place sensitive personal information in the token.

---

# 12. Refresh Mechanism

When access expires:

```text
Client
 ↓
Refresh Request
 ↓
Session Validation
 ↓
Refresh Token Validation
 ↓
New Access Credential
```

If the session has been revoked:

```text
Refresh Denied
```

---

# 13. Logout

```text
User
 ↓
Logout
 ↓
Session Revocation
 ↓
Credential Invalidated
```

All sensitive session state must be invalidated server-side where applicable.

---

# 14. Password Reset

```text
Forgot Password
 ↓
Email
 ↓
Reset Token
 ↓
New Password
 ↓
Password Hash Updated
 ↓
Existing Sessions Revoked
```

Reset tokens must:

```text
Expire
Be single-use
Be unpredictable
Not expose password information
```

---

# 15. Password Security

Passwords must never be stored in plaintext.

Use a modern password hashing algorithm such as:

```text
Argon2id
```

or another appropriately configured password-hashing mechanism supported by the chosen authentication stack.

---

# 16. Authorization Architecture

Authentication answers:

```text
Who are you?
```

Authorization answers:

```text
What are you allowed to do?
```

AILPG requires both.

---

# 17. Authorization Layers

Authorization is evaluated in this order:

```text
1. Authentication
       ↓
2. Account Status
       ↓
3. Organization Membership
       ↓
4. Role
       ↓
5. Resource Ownership
       ↓
6. Permission
       ↓
7. Subscription Entitlement
```

---

# 18. RBAC

Role-Based Access Control:

```text
User
 ↓
Role
 ↓
Permissions
```

Example:

```text
TEACHER
 ├── course:create
 ├── course:update
 ├── lesson:create
 ├── video:upload
 ├── ai:process
 ├── lesson:review
 └── lesson:publish
```

---

# 19. Permission Naming Convention

Use:

```text
resource:action
```

Examples:

```text
course:create
course:read
course:update
course:delete

lesson:create
lesson:read
lesson:update
lesson:publish

video:upload
video:read
video:delete

user:create
user:update
user:delete
```

---

# 20. Role-Permission Matrix

| Permission | Student | Teacher | Org Admin | Platform Admin |
|---|---:|---:|---:|---:|
| course:read | ✓ | ✓ | ✓ | ✓ |
| course:create | - | ✓ | ✓ | ✓ |
| course:update | - | ✓ | ✓ | ✓ |
| course:delete | - | ✓ | ✓ | ✓ |
| lesson:create | - | ✓ | ✓ | ✓ |
| lesson:update | - | ✓ | ✓ | ✓ |
| lesson:publish | - | ✓ | ✓ | ✓ |
| video:upload | - | ✓ | ✓ | ✓ |
| ai:process | - | ✓ | ✓ | ✓ |
| user:manage | - | - | ✓ | ✓ |
| organization:manage | - | - | ✓ | ✓ |
| platform:manage | - | - | - | ✓ |
| audit:read | - | - | limited | ✓ |

---

# 21. Multi-Tenant Architecture

AILPG should support multiple organizations.

Example:

```text
Platform
│
├── Organization A
│   ├── Teachers
│   ├── Students
│   └── Courses
│
├── Organization B
│   ├── Teachers
│   ├── Students
│   └── Courses
│
└── Organization C
```

---

# 22. Tenant Isolation

Every organization-owned resource should contain:

```text
organization_id
```

Example:

```text
courses.organization_id
lessons.organization_id
videos.organization_id
```

---

# 23. Tenant Authorization

Request:

```text
User → Course
```

Authorization:

```text
Authenticated?
      ↓
Organization member?
      ↓
Same organization?
      ↓
Role allowed?
      ↓
Permission allowed?
      ↓
Access
```

---

# 24. Cross-Tenant Access

Example:

```text
User belongs to Org A

Requests:

Org B Course
```

Result:

```text
ACCESS DENIED
```

The backend must enforce this even if the client modifies IDs.

---

# 25. Resource Ownership

Resources may be owned by:

```text
User
Organization
Course
Lesson
```

Example:

```text
Teacher A
   ↓
Course A
   ↓
Lesson A
```

Teacher B should not automatically access Teacher A's private draft unless organization permissions allow it.

---

# 26. Teacher Course Access

Teacher access may be:

```text
Organization-wide
```

or:

```text
Assigned-course only
```

Recommended permission model:

```text
Teacher
 ↓
Organization
 ↓
Course Assignment
 ↓
Lesson
```

---

# 27. Draft vs Published Authorization

Draft:

```text
Teacher
Org Admin
Platform Admin
```

Published:

```text
Authorized Students
Teachers
Admins
```

Students must not access unpublished lessons.

---

# 28. Subscription Authorization

Authentication alone is not enough for premium content.

```text
Authenticated User
       ↓
Lesson Access
       ↓
Subscription Check
       ↓
Entitlement
       ↓
Allow / Deny
```

---

# 29. Entitlement Model

Example:

```text
PLAN_FREE
 ├── basic_courses
 └── standard_video

PLAN_PREMIUM
 ├── premium_courses
 ├── high_quality_video
 ├── translations
 └── advanced_progress

PLAN_ORGANIZATION
 ├── organization_courses
 ├── teacher_tools
 ├── analytics
 └── admin_tools
```

---

# 30. Feature Access

Do not hard-code plan names throughout application code.

Instead:

```text
User
 ↓
Plan
 ↓
Entitlements
 ↓
Feature Access
```

Example:

```text
can_access_hd_video
can_access_translation
can_create_course
can_view_analytics
```

---

# 31. API Authorization Middleware

Conceptual flow:

```text
HTTP Request
    ↓
Request ID
    ↓
Authenticate
    ↓
Load User
    ↓
Load Organization
    ↓
Check Permission
    ↓
Check Resource
    ↓
Check Entitlement
    ↓
Controller
```

---

# 32. API Error Responses

Unauthorized:

```http
401 Unauthorized
```

Authenticated but not permitted:

```http
403 Forbidden
```

Resource not accessible:

```http
404 Not Found
```

The application should avoid leaking whether protected resources exist when doing so would reveal sensitive information.

---

# 33. API Rate Limiting

Rate limits should apply to:

```text
Login
Registration
Password reset
AI generation
File upload initialization
Question submission
Public APIs
```

Different roles and endpoints may have different limits.

---

# 34. File Access Authorization

Original videos should not be publicly accessible.

Flow:

```text
Student
 ↓
Lesson Access API
 ↓
Entitlement Check
 ↓
Signed URL
 ↓
CDN
 ↓
Video
```

---

# 35. Signed URL

Signed URLs should have:

```text
Short expiration
Resource restriction
Optional IP/device constraints
```

Exact restrictions depend on CDN capabilities and product requirements.

---

# 36. Admin Authorization

Platform administration requires stronger controls.

Recommended:

```text
Separate admin permissions
Shorter sessions
Strong authentication
Audit logging
Optional MFA
```

---

# 37. High-Risk Actions

Require elevated authorization for:

```text
Delete organization
Delete course
Delete video
Publish lesson
Change role
Change subscription
Retry expensive AI job
Change platform configuration
```

---

# 38. Audit Logging

Audit event:

```json
{
  "actor_id": "user_001",
  "organization_id": "org_001",
  "action": "LESSON_PUBLISHED",
  "resource_type": "lesson",
  "resource_id": "lesson_001",
  "timestamp": "..."
}
```

---

# 39. Security Event Types

```text
LOGIN_SUCCESS
LOGIN_FAILED
LOGOUT
PASSWORD_CHANGED
PASSWORD_RESET
ROLE_CHANGED
PERMISSION_DENIED
LESSON_PUBLISHED
VIDEO_DELETED
AI_JOB_RETRIED
SUBSCRIPTION_CHANGED
```

---

# 40. Session Revocation

Sessions must be revocable when:

```text
Password reset
Password changed
Admin account disabled
User deactivated
Suspicious activity
Manual logout-all
```

---

# 41. Account States

```text
PENDING_VERIFICATION
ACTIVE
SUSPENDED
DISABLED
DELETED
```

Only:

```text
ACTIVE
```

accounts should normally authenticate.

---

# 42. Organization Membership States

```text
INVITED
ACTIVE
SUSPENDED
REMOVED
```

Only active memberships should provide organization access.

---

# 43. Organization Invitation Flow

```text
Org Admin
 ↓
Invite User
 ↓
Invitation Token
 ↓
Email
 ↓
User Accepts
 ↓
Account Created / Linked
 ↓
Membership ACTIVE
```

---

# 44. Role Change Flow

```text
Org Admin
 ↓
Select User
 ↓
Select New Role
 ↓
Authorization Check
 ↓
Update Role
 ↓
Audit Log
 ↓
Invalidate Relevant Sessions / Permissions
```

---

# 45. OAuth Flow

Future OAuth:

```text
User
 ↓
Provider Login
 ↓
Provider Callback
 ↓
Validate Identity
 ↓
Find / Create User
 ↓
Create Session
 ↓
AILPG Login
```

OAuth provider identity must be mapped to an internal user record.

---

# 46. MFA

Recommended for:

```text
Platform Admin
Organization Admin
```

Future support:

```text
Authenticator App
Passkey
Security Key
Email OTP
```

MFA should be optional for students initially and configurable for organizations.

---

# 47. Security Headers

Production API/web applications should implement appropriate headers such as:

```text
Content-Security-Policy
Strict-Transport-Security
X-Content-Type-Options
Referrer-Policy
Frame protection
```

Exact policy depends on deployment architecture.

---

# 48. CORS

Allowed origins must be explicitly configured.

Development:

```text
localhost origins
```

Production:

```text
Official application domains only
```

Avoid:

```text
Access-Control-Allow-Origin: *
```

for authenticated sensitive APIs unless there is a deliberate, reviewed reason.

---

# 49. CSRF Protection

For cookie-based authentication, implement CSRF protection.

For bearer-token APIs, ensure token handling and browser storage architecture are designed to prevent token theft and cross-site abuse.

---

# 50. Secret Management

Secrets must never be committed to Git.

Examples:

```text
Database password
JWT signing secret
OAuth client secret
AI provider key
Storage credentials
Payment credentials
```

Use:

```text
Environment secrets
Secret Manager
CI/CD secret store
```

---

# 51. Authentication Database Entities

Core entities:

```text
users
sessions
password_credentials
email_verifications
password_reset_tokens
roles
permissions
role_permissions
organization_memberships
invitations
```

Detailed schema will be defined in the Database Design layer.

---

# 52. Authorization Database Entities

```text
roles
permissions
role_permissions
organization_memberships
course_assignments
entitlements
subscriptions
```

---

# 53. Authorization Decision Example

Request:

```text
POST /api/v1/lessons/lesson_001/publish
```

Decision:

```text
Authenticated?
       │
       ├── No → 401
       │
       ▼
Account active?
       │
       ├── No → 403
       │
       ▼
Organization member?
       │
       ├── No → 403
       │
       ▼
Has lesson:publish?
       │
       ├── No → 403
       │
       ▼
Can access lesson?
       │
       ├── No → 403
       │
       ▼
Publish
```

---

# 54. Student Lesson Decision

```text
Student
  ↓
Authenticated?
  ↓
Active?
  ↓
Course published?
  ↓
Lesson published?
  ↓
Organization access?
  ↓
Subscription entitlement?
  ↓
ALLOW
```

---

# 55. Teacher Video Upload Decision

```text
Teacher
  ↓
Authenticated?
  ↓
Active?
  ↓
Organization member?
  ↓
video:upload?
  ↓
Course access?
  ↓
Storage quota?
  ↓
ALLOW
```

---

# 56. Admin Job Retry Decision

```text
Platform Admin
  ↓
Authenticated?
  ↓
MFA / elevated security
  ↓
job:retry permission
  ↓
Job exists?
  ↓
Retry allowed?
  ↓
Audit
  ↓
Queue Job
```

---

# 57. Authorization Caching

Permissions may be cached using Redis.

However:

```text
Database = Source of Truth
Redis = Cache
```

Role changes must invalidate relevant cache entries.

---

# 58. Security Logging

Do not log:

```text
Passwords
Access tokens
Refresh tokens
Secrets
Sensitive credentials
```

Logs may contain:

```text
User ID
Request ID
Action
Status
Timestamp
IP metadata where justified
```

---

# 59. Authentication Testing

Required tests:

```text
Valid login
Invalid password
Unknown user
Expired session
Revoked session
Password reset
Email verification
Role validation
Tenant isolation
Permission denial
Subscription denial
```

---

# 60. Authorization Testing

Test:

```text
Student → Student resource
Student → Teacher resource
Teacher → Own course
Teacher → Other teacher course
Org Admin → Same organization
Org Admin → Other organization
Platform Admin → Platform resources
```

Expected result must be explicitly defined for each.

---

# 61. Security Acceptance Criteria

```text
Passwords are hashed
✓

Sessions are revocable
✓

Protected APIs require authentication
✓

Authorization is server-side
✓

Tenant isolation is enforced
✓

Premium access checks entitlement
✓

Protected video uses controlled access
✓

Sensitive actions are audited
✓

Admin permissions are restricted
✓

Secrets are not stored in Git
✓
```

---

# 62. Recommended Authentication Architecture

For MVP:

```text
Email/Password
      +
Session Management
      +
RBAC
      +
Organization Membership
      +
Subscription Entitlements
      +
Audit Logging
```

Future:

```text
OAuth
MFA
Passkeys
SSO
```

---

# 63. Authentication Definition of Done

```text
Registration               ✓
Email Verification         ✓
Login                      ✓
Logout                     ✓
Password Reset             ✓
Session Management         ✓
RBAC                       ✓
Tenant Isolation           ✓
Resource Authorization     ✓
Subscription Authorization ✓
Admin Security             ✓
Audit Logging              ✓
Rate Limiting              ✓
Security Testing           ✓
```

---

# 64. Next Document

```text
06_Video_Processing_Design.md
```

This will define the most important media pipeline:

```text
MP4 Upload
    ↓
Validation
    ↓
Metadata Extraction
    ↓
Audio Extraction
    ↓
Frame Extraction
    ↓
Scene Detection
    ↓
Transcoding
    ↓
HLS Generation
    ↓
Thumbnail Generation
    ↓
CDN
    ↓
Student Player
```

It will also define:

```text
Video quality levels
Processing workers
FFmpeg pipeline
Storage structure
HLS
Thumbnails
Frames
Retry strategy
Large-file handling
```

---

# 65. Revision History

| Version | Date | Description |
|---|---|---|
| 1.0.0 | 2026-08-11 | Initial authentication and authorization design |
