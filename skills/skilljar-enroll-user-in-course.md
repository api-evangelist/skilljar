---
name: Enroll a user in a published course
description: Create (or look up) a learner in a Skilljar training domain and enroll them in a published course.
api: openapi/skilljar-openapi-original.yml
operations:
  - domains_users_create
  - domains_users_retrieve
  - domains_published_courses_list
  - domains_published_courses_enrollments_create
  - domains_published_courses_enrollments_list
---

# Enroll a user in a published course

Use the Skilljar v1 REST API (`https://api.skilljar.com/v1/`). Authenticate with
HTTP Basic: send your Organization API key as the username and leave the password
empty. All course/enrollment operations are scoped to a training `domain_name`.

## Steps

1. **Find the published course.** `GET /v1/domains/{domain_name}/publishedcourses`
   (`domains_published_courses_list`). Results are page-number paginated
   (`page`, `page_size`; envelope `count`/`next`/`previous`/`results`). Capture the
   published course id.
2. **Create or resolve the learner.** `POST /v1/domains/{domain_name}/users`
   (`domains_users_create`) with the learner's email. If the user may already
   exist, read them back with `domains_users_retrieve`.
3. **Enroll the user.** `POST /v1/domains/{domain_name}/publishedcourses/{published_course_id}/enrollments`
   (`domains_published_courses_enrollments_create`). This operation is documented
   as **naturally idempotent** for the same user and enrollment time — a safe
   retry does not create a duplicate enrollment.
4. **Confirm.** `GET .../enrollments` (`domains_published_courses_enrollments_list`)
   and verify the learner appears.

## Rules
- Errors return a JSON:API-style `{ "errors": [{ status, code, title, detail }] }`
  envelope; handle standard 400/401/403/404 codes.
- No rate-limit headers are documented; back off on any 429/503.
