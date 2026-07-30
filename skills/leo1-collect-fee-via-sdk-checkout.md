---
name: Collect a student fee through the LEO1 Fees SDK checkout
description: Start a fee transaction server-to-server, hand the encrypted blob to the browser SDK to render checkout, and reconcile the result from the payment-gateway webhook.
api: openapi/leo1-leofees-openapi-original.json
operations:
  - generate_token_api_v1_sdk_generate_access_post
  - start_transaction_api_v1_sdk_start_transaction_post
  - get_transaction_status_api_v1_transactions_get_transaction_status__get
---

# Collect a student fee through the LEO1 Fees SDK checkout

Use this when an institute ERP needs to take a fee payment from a student or parent.

## Before you start

- You need an **Access Key** and a **Secret Key**, issued by LEO1 during institute onboarding. There is no self-service signup.
- Base URL is `https://api.leo1.in` for production and `https://uatapi.leo1.in` for the TEST environment.
- Authenticated operations take the Secret Key in the **`secret-key` request header** (`APIKeyHeader`).

## Steps

1. **Sign the request.** Build the pipe-delimited hash string in exactly this order, appending the Secret Key at the end, then take the lowercase hex SHA-512 digest:

   ```
   key|total_fees|fees_paid|student_name|roll_number|branch_name|course_name|parent_name|institute_name|phone_number|erp_transaction_id
   ```

   Trim leading and trailing whitespace from every value first. A field you do not supply (for example `branch_name` or `course_name`) contributes an empty string but still contributes its `|` separator. Put the digest in the `hash` field.

2. **Optionally mint an access token** with `generate_token_api_v1_sdk_generate_access_post` (`POST /api/v1/sdk/generate_access`).

3. **Start the transaction** with `start_transaction_api_v1_sdk_start_transaction_post` (`POST /api/v1/sdk/start_transaction`). Required body fields: `key`, `roll_number`, `phone_number`, `institute_name`, `branch_name`, `course_name`, `parent_name`, `fees_paid`, `total_fees`, `erp_transaction_id`, `student_name`, `hash`. Optional: `redirect_success`, `redirect_fail`, `extra_data`, `email_id`. Set `erp_transaction_id` to your own ledger identifier — it is the join key for reconciliation and for both webhooks.

   The 200 response is `EncryptedTransaction`: a single `data` string.

4. **Render checkout in the browser.** Pass `data` and your Access Key to the SDK loaded from the CDN script/stylesheet pair:

   ```js
   initiateLeoFeesPayment({ data, accessKey, env: "test", onResolve, onError });  // payment gateway
   initiateLeoFees({ data, accessKey, env: "test", onResolve, onError });         // fee-finance loan journey
   ```

   Set `env` to `test` or `prod` to match the base URL you called.

5. **Reconcile from the webhook, not the browser callback.** LEO1 POSTs the payment-gateway payload to a URL you register with them (not self-service). Match on `erp_transaction_id` and read `status`, `amount`, `txnid`. Treat the `onResolve` callback as a UI hint only.

6. **Poll only if needed.** `get_transaction_status_api_v1_transactions_get_transaction_status__get` (`GET /api/v1/transactions/get_transaction_status/`) reads back current status.

## Rules

- **There is no idempotency key.** Retrying `start_transaction` with the same `erp_transaction_id` is not documented as safe — check status before retrying rather than resending blindly.
- The contract declares only `200` and `422`. A `422` returns the FastAPI envelope `{"detail":[{"loc":[...],"msg":"...","type":"..."}]}` — read `loc` to find the offending field. Auth and server failures are not described in the spec; handle non-200 defensively.
- No rate limits are published. Back off on failure.
