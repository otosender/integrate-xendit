# Webhook

## Notes
1. The `X-CALLBACK-TOKEN` comes with every webhook, use the token to validate that a webhook came from our servers
2. All timestamps are in UTC

## Headers
Each webhook request includes the following headers:

| Header Name          | Example Value                                  | Description |
|----------------------|-----------------------------------------------|-------------|
| `webhook-id`         | `e8dcb949-8e49-4f1d-89f0-67c08913db82`        | Unique identifier for the webhook event |
| `X-CALLBACK-TOKEN`   | `****************************************4AAChscH` | Authentication token   |
| `Content-Type`       | `application/json`                            | Content type of the payload |
| `Content-Length`     | `381`                                         | Content length |

---

## Event Types & Payloads Example

### 1. Credit Card Authentication
**Event:** `credit_card.authentication`

```json
{
    "event": "credit_card.authentication",
    "business_id": "5781d19b2e2385880219791c",
    "created": "2021-01-11T00:00:00.000Z",
    "data": {
        "id": "5824128aa6f9f2e648be9d76",
        "external_id": "order-12345",
        "amount": 10000,
        "card_data": {
            "masked_card_number": "400000XXXXXX0002",
            "exp_month": "10",
            "exp_year": "2025"
        },
        "status": "VERIFIED",
        "eci": "05",
        "three_ds_result": "AUTHENTICATED",
        "three_ds_version": "2.1.0"
    }
}
```

### 2. Credit Card Tokenization
**Event:** `credit_card.tokenization`

```json
{
    "event": "credit_card.tokenization",
    "business_id": "5781d19b2e2385880219791c",
    "created": "2022-12-02T04:14:38.475Z",
    "api_version": null,
    "data": {
        "id": "63897bac0d7847001af64acf",
        "status": "VERIFIED",
        "external_id": "tokenization-callback-test",
        "masked_card_number": "400000XXXXXX1091",
        "card_expiration_month": "10",
        "card_expiration_year": "2025",
        "card_info": {
            "bank": "PT BANK RAKYAT INDONESIA TBK",
            "country": "ID",
            "type": "CREDIT",
            "brand": "VISA",
            "fingerprint": "6368de0e28d184001ad4c245"
        }
    }
}
```

### 3. E-Wallet Payment Status
**Event:** `ewallet.capture`

```json
{
    "event": "ewallet.capture",
    "business_id": "59e0daf7049b567510c63f67",
    "created": "2020-10-21T13:59:14.536400713Z",
    "data": {
        "id": "ewc_95d47d3a-db03-4b4b-9b6c-71077157cbc8",
        "status": "SUCCEEDED",
        "reference_id": "test-payload",
        "charge_amount": 20000,
        "capture_amount": 20000,
        "currency": "IDR",
        "channel_code": "ID_SHOPEEPAY",
        "created": "2020-10-21T13:57:43.355897Z",
        "updated": "2020-10-21T13:57:43.730483Z",
        "checkout_method": "ONE_TIME_PAYMENT",
        "is_redirect_required": true,
        "actions": {
            "mobile_deeplink_checkout_url": "https://wsa.uat.wallet.airpay.co.id/universal-link/wallet/pay?..."
        },
        "channel_properties": {
            "success_redirect_url": "https://google.com"
        },
        "metadata": {
            "branch_code": "senayan_372"
        }
    }
}
```

### 4. E-Wallet Reconciliation Update
**Event:** `ewallet.capture`  

```json
{
    "event": "ewallet.capture",
    "business_id": "59e0daf7049b567510c63f67",
    "created": "2020-10-21T13:59:14.536400713Z",
    "data": {
        "id": "ewc_95d47d3a-db03-4b4b-9b6c-71077157cbc8",
        "status": "SUCCEEDED",
        "reference_id": "test-payload",
        "charge_amount": 20000,
        "capture_amount": 20000,
        "currency": "IDR",
        "channel_code": "ID_SHOPEEPAY",
        "created": "2020-10-21T13:57:43.355897Z",
        "updated": "2020-10-21T13:57:43.730483Z",
        "checkout_method": "ONE_TIME_PAYMENT",
        "is_redirect_required": true,
        "actions": {
            "mobile_deeplink_checkout_url": "https://wsa.uat.wallet.airpay.co.id/universal-link/wallet/pay?..."
        },
        "channel_properties": {
            "success_redirect_url": "https://google.com"
        },
        "metadata": {
            "branch_code": "senayan_372"
        }
    }
}
```
---

Webhook Listed. (Need to Add Used Webhook in Webhook URL space)

---

| Product Category               | Event Type                          | Webhook URL           | Notes |
|--------------------------------|-------------------------------------|-----------------------|-------|
| **FIXED VIRTUAL ACCOUNTS**      | FVA paid                            | `http://example.com`  | Triggered when a customer completes payment to a virtual account. Verify amount and sender details. |
|                                | FVA created                         | `http://example.com`  | Sent when a new virtual account number is generated. Log for reconciliation. |
| **PAYOUT LINK**                | Payout Links                        | `http://example.com`  | Fires when a payout transaction is initiated. Check status via API. |
| **RETAIL OUTLETS (OTC)**       | Retail outlets (OTC) paid           | `http://example.com`  | Confirms cash payment at retail partners. Validate reference ID. |
| **CARDS**                      | Cards authentication                | `http://example.com`  | 3DSecure or biometric auth result. Update UI for success/failure. |
|                                | Cards tokenization                  | `http://example.com`  | New card token generated. Store securely for recurring payments. |
| **DIRECT DEBIT**               | Account linked                      | `http://example.com`  | Customer’s bank account successfully connected. Start debit mandates. |
|                                | Payment completed                   | `http://example.com`  | Direct debit successful. Reconcile with invoice system. |
|                                | Expiring/Expired payment method     | `http://example.com`  | Notify customer to update payment method if recurring. |
|                                | Refund finalized                    | `http://example.com`  | Process completed. Update accounting records. |
| **REPORT**                     | Balance and Transactions report     | `http://example.com`  | Daily summary. Automate ledger updates. |
| **UNIFIED REFUNDS (BETA)**     | Refund request succeeded            | `http://example.com`  | Refund processed. Notify customer via email/SMS. |
|                                | Refund request failed               | `http://example.com`  | Investigate reason (e.g., insufficient balance). Retry if needed. |
| **PAYMENT REQUESTS V3**        | Payment Status                      | `http://example.com`  | Real-time payment update. Sync with order management. |
|                                | Payment Request Status              | `http://example.com`  | High-level status (e.g., expired, paid). |
| **PAYMENT TOKENS V3**          | Payment Token Status                | `http://example.com`  | Token lifecycle event (e.g., revoked). Block fraud attempts. |
| **RECURRING**                  | Recurring                           | `http://example.com`  | Subscription payment charged. Generate invoice. |
| **E-WALLETS**                  | eWallet Payment Status              | `http://example.com`  | eWallet transaction result (e.g., GrabPay, OVO). |
|                                | eWallet Reconciliation Update       | `http://example.com`  | Batch settlement notification. Verify totals. |
| **PAYLATER**                   | Paylater Payment Status             | `http://example.com`  | BNPL payment confirmation. Ship goods if approved. |
| **QR CODES**                   | QR code paid & refunded             | `http://example.com`  | Scan-based payment. Close loop for offline POS. |
|                                | QR Reconciliation                   | `http://example.com`  | Discrepancy alerts. Investigate mismatches. |
| **PAYMENT SESSION**            | Payment Session Completed           | `http://example.com`  | Customer completed checkout. Fulfill order. |
|                                | Payment Session Expired             | `http://example.com`  | Abandoned cart. Trigger retargeting campaigns. |

---
