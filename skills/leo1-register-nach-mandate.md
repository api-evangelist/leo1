---
name: Register a NACH e-mandate for recurring LEO1 fee debits
description: Set up an NPCI NACH/eNACH mandate so a student's recurring fee instalments can be auto-debited, then track registration status and presentations.
api: openapi/leo1-leofees-openapi-original.json
operations:
  - get_bank_codes_api_v1_nach_get_bank_codes__get
  - initiate_nach_registration_api_v1_nach_registration_initiate__post
  - get_registration_redirect_link_api_v1_nach_registration_link__student_uuid__get
  - get_registration_details_api_v1_nach_registration_details__student_uuid__get
  - get_debit_frequencies_api_v1_enach_debit_frequencies_get
  - preview_debit_schedule_api_v1_enach_preview_debit_schedule_post
  - create_enach_schedule_api_v1_enach_create_schedule_post
  - get_registration_details_api_v1_nach_presentation__post
---

# Register a NACH e-mandate for recurring LEO1 fee debits

Use this when an institute wants fee instalments auto-debited from a student or parent bank account instead of collected per payment.

## Before you start

Send the institute Secret Key in the `secret-key` header. Confirm the institute is enabled for NACH with `get_institute_nach_applicable_api_v1_nach_institute_nach_applicable_get` (`GET /api/v1/nach/institute-nach-applicable`).

## Steps

1. **Resolve the destination bank.** `get_bank_codes_api_v1_nach_get_bank_codes__get` (`GET /api/v1/nach/get-bank-codes/`) lists supported bank codes.

2. **Initiate registration.** `initiate_nach_registration_api_v1_nach_registration_initiate__post` (`POST /api/v1/nach/registration/initiate/`). For the fee-subscription variant use `fee_subscription_initiate_nach_registration_api_v1_nach_fee_subscription_registration_initiate__post` (`POST /api/v1/nach/fee-subscription/registration/initiate/`).

3. **Send the payer to the mandate journey.** `get_registration_redirect_link_api_v1_nach_registration_link__student_uuid__get` (`GET /api/v1/nach/registration/link/{student_uuid}`) returns the redirect link where the payer authenticates and authorises the mandate at their bank.

4. **Track the outcome.** LEO1 receives the bank callback on its own webhook endpoints (`registration_webhook_api_v1_nach_registration_webhook__post`, `mandate_webhook_api_v1_nach_mandate_webhook__post`) — those are inbound to LEO1, not to you. Read status with `get_registration_details_api_v1_nach_registration_details__student_uuid__get` (`GET /api/v1/nach/registration/details/{student_uuid}`) or `request_nach_registration_api_v1_nach_registration_status__get` (`GET /api/v1/nach/registration/status/`).

5. **Build the debit schedule.** List frequencies with `get_debit_frequencies_api_v1_enach_debit_frequencies_get` (`GET /api/v1/enach/debit_frequencies`), dry-run with `preview_debit_schedule_api_v1_enach_preview_debit_schedule_post` (`POST /api/v1/enach/preview_debit_schedule`), then commit with `create_enach_schedule_api_v1_enach_create_schedule_post` (`POST /api/v1/enach/create_schedule`).

6. **Present a debit.** `get_registration_details_api_v1_nach_presentation__post` (`POST /api/v1/nach/presentation/`) presents against an existing mandate via `registration_id`; check the result with `request_nach_registration_api_v1_nach_presentation_status__get` (`GET /api/v1/nach/presentation/status/`).

## Rules

- **Always call `preview_debit_schedule` before `create_schedule`.** Mandate creation and presentation move real money and there is no idempotency key — a blind retry can double-present.
- A mandate is only usable once registration reaches an authorised state at the bank. Do not schedule debits off the initiate response alone; confirm via the status read in step 4.
- Only `200` and `422` are declared. Treat any non-200 as unresolved and re-read status rather than re-initiating.
- Card-consent operations (`get_b2c_card_consent_api_v1_nach_get_b2c_card_consent__get`, `update_b2c_card_consent_api_v1_nach_update_b2c_card_consent__post`) govern payer consent and must not be set on the payer's behalf without a recorded consent event.
