---
name: Report a learner's course progress
description: Look up a Skilljar user and read their published-course enrollments and per-lesson progress.
api: openapi/skilljar-openapi-original.yml
operations:
  - users_list
  - users_retrieve
  - users_published_courses_list
  - users_published_courses_lessons_list
  - lesson_progress_retrieve
---

# Report a learner's course progress

Read-only reporting flow over the Skilljar v1 API (HTTP Basic API-key auth).

## Steps

1. **Find the user.** `GET /v1/users` (`users_list`, page-number paginated) or
   `GET /v1/users/{id}` (`users_retrieve`) by id.
2. **List their course enrollments.** `GET /v1/users/{user_id}/publishedcourses`
   (`users_published_courses_list`) — returns each published course the learner is
   enrolled in with completion state.
3. **Drill into lessons.** `GET /v1/users/{user_id}/publishedcourses/{published_course_id}/lessons`
   (`users_published_courses_lessons_list`) for per-lesson status.
4. **Per-lesson detail.** `GET` `lesson_progress_retrieve` for a specific lesson's
   granular progress record.

## Rules
- All list endpoints paginate with `page`/`page_size`; follow `next` until null.
- This flow is read-only; require only read scopes if using v2 OAuth
  (`students:read`, `enrollments:read`, `published-courses:read`).
