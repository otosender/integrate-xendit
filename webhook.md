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
