---
name: Reconcile LEO1 institute transactions and settlements
description: Pull an institute's fee transactions for a date range and the matching settlement records to reconcile collected fees against funds received.
api: openapi/leo1-leofees-openapi-original.json
operations:
  - get_institute_transactions_api_v1_transactions_institute_transactions__post
  - retrieve_institute_transactions_date_api_v1_institute_transactions_date_post
  - retrieve_institute_settlements_api_v1_institute_settlement_post
  - get_institute_transactions_csv_api_v1_transactions_institute_transactions_csv__post
  - get_institute_aggregate_transcations_api_v1_transactions_aggregate_transcations__inst_name__get
---

# Reconcile LEO1 institute transactions and settlements

Use this to close the loop between fees a student paid and money that landed in the institute account.

## Before you start

Send the institute Secret Key in the `secret-key` header. Base URL `https://api.leo1.in`.

## Steps

1. **Fetch transactions for the period.** `retrieve_institute_transactions_date_api_v1_institute_transactions_date_post` (`POST /api/v1/institute/transactions/date`) takes a date range. For a single lookup keyed on your own ledger id, use `get_institute_transactions_api_v1_transactions_institute_transactions__post` (`POST /api/v1/transactions/institute_transactions/`), which resolves by `erp_transaction_id`.

2. **Fetch settlements.** `retrieve_institute_settlements_api_v1_institute_settlement_post` (`POST /api/v1/institute/settlement`) returns settlement records for the same window.

3. **Match.** Join transactions to your ledger on `erp_transaction_id`; `InstituteTransactionResponse` also carries `transaction_id`, `leofees_id` and the gateway-side `easebuzz_id` for tracing a payment back to the processor.

4. **Cross-check totals.** `get_institute_aggregate_transcations_api_v1_transactions_aggregate_transcations__inst_name__get` (`GET /api/v1/transactions/aggregate_transcations/{inst_name}`) returns aggregate figures to sanity-check the sum of the detail rows.

5. **Export.** `get_institute_transactions_csv_api_v1_transactions_institute_transactions_csv__post` (`POST /api/v1/transactions/institute_transactions_csv/`) produces a CSV extract for finance.

## Rules

- **There is no pagination.** Collection endpoints return the full filtered result set — narrow by date range rather than paging, and prefer the CSV export for large windows.
- Amounts arrive as strings on the webhook payloads (`amount`, `fees_paid`, `total_fees`) — parse deliberately and do not use floating point for money.
- Only `200` and `422` are declared; a `422` names the bad field in `detail[].loc`.
- Reversals are a separate operation (`reverse_transaction_api_v1_transactions_reverse_transaction__post`); a reversed transaction still appears in the transaction list, so reconcile on status rather than presence.
