---
name: Subscribe to Skilljar platform events with webhooks
description: Register a webhook target, inspect a sample payload, and verify delivery for Skilljar learning events.
api: openapi/skilljar-openapi-original.yml
operations:
  - webhooks_create
  - webhooks_list
  - webhooks_ping_create
  - webhooks_sample_course_completion_retrieve
  - webhooks_destroy
---

# Subscribe to Skilljar platform events with webhooks

Skilljar posts real-time events to a subscriber `target_url`. Manage subscriptions
through the `/v1/webhooks` operations (HTTP Basic API-key auth).

## Steps

1. **(Optional) Preview a payload.** `GET /v1/webhooks/sample-course-completion`
   (`webhooks_sample_course_completion_retrieve`) — retrieve a representative
   payload for the event you plan to consume. Sample endpoints exist for each of
   the 10 event types (course/lesson/path completion & enrollment, quiz
   completion, purchase fulfillment, VILT registration, domain enrollment,
   dashboard task created).
2. **Create the subscription.** `POST /v1/webhooks` (`webhooks_create`) with
   `target_url` and an `event_type` (omit / null to receive **all** events).
3. **Verify delivery.** `POST /v1/webhooks/{webhook_id}/ping`
   (`webhooks_ping_create`) to trigger a test delivery to your endpoint.
4. **Audit.** `GET /v1/webhooks` (`webhooks_list`) to confirm the subscription is
   `active`; a subscription can be auto-deactivated (see `deactivate_reason`).
5. **Tear down** with `webhooks_destroy` when finished.

## Rules
- Respond `2xx` quickly from your `target_url`; repeated failures set
  `deactivate_reason` and disable the hook.
- Event types are enumerated in `asyncapi/skilljar-webhooks.yml`.
