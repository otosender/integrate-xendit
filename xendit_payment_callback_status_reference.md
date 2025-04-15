---

# **Xendit Payment Callback Status Reference**  
*Last updated: [Current Date]*  

## **1. Virtual Account (VA) Callback Statuses**  
| Status Code | Description | Action Required |
|------------|-------------|-----------------|
| `ACTIVE`   | VA created successfully. | Display VA details to user. |
| `PAID`     | Payment received. | Fulfill order/service. |
| `EXPIRED`  | VA expired without payment. | Offer retry option. |
| `INACTIVE` | VA deactivated (manual/error). | Notify user to contact support. |

**Webhook Example (JSON):**  
```json
{
  "id": "va_123456789",
  "external_id": "order-789",
  "owner_id": "user_123",
  "bank_code": "BNI",
  "account_number": "9889901234567890",
  "status": "PAID",
  "amount": 100000,
  "created": "2023-10-01T12:00:00Z",
  "updated": "2023-10-01T14:30:00Z"
}
```

---

## **2. Credit/Debit Card Statuses**  
| Status Code | Description | Action Required |
|------------|-------------|-----------------|
| `AUTHORIZED` | Card charged but not settled. | Hold items until `CAPTURED`. |
| `CAPTURED`   | Payment settled. | Complete order. |
| `FAILED`     | Charge failed. | Show error and retry option. |
| `REFUNDED`   | Payment refunded. | Reverse order fulfillment. |

**Webhook Example (JSON):**  
```json
{
  "id": "card_charge_123",
  "status": "CAPTURED",
  "external_id": "order-456",
  "amount": 50000,
  "capture_amount": 50000,
  "card_last_four": "1234",
  "failure_reason": null,
  "created": "2023-10-01T12:05:00Z"
}
```

---

## **3. E-Wallet (OVO/DANA/LinkAja)**  
| Status Code | Description | Action Required |
|------------|-------------|-----------------|
| `SUCCEEDED` | Payment successful. | Deliver order. |
| `FAILED`    | Payment failed. | Prompt retry. |
| `PENDING`   | Payment waiting completion. | Monitor for updates. |

**Webhook Example (JSON):**  
```json
{
  "id": "ewallet_789",
  "external_id": "order-101",
  "status": "SUCCEEDED",
  "amount": 75000,
  "ewallet_type": "OVO",
  "created": "2023-10-01T13:20:00Z"
}
```

---

## **4. Retail Outlets (Alfamart/Indomaret)**  
| Status Code | Description | Action Required |
|------------|-------------|-----------------|
| `COMPLETED` | Payment confirmed. | Fulfill order. |
| `EXPIRED`   | Payment code expired. | Regenerate code. |
| `PENDING`   | Awaiting payment. | Wait for callback. |

**Webhook Example (JSON):**  
```json
{
  "payment_code": "ALFA123456",
  "status": "COMPLETED",
  "amount": 120000,
  "retail_outlet_name": "ALFAMART",
  "created": "2023-10-01T10:15:00Z"
}
```

---

## **5. QRIS Payments**  
| Status Code | Description | Action Required |
|------------|-------------|-----------------|
| `SUCCESS`  | Payment completed. | Complete transaction. |
| `FAILED`   | Payment rejected. | Notify user. |
| `EXPIRED`  | QR code expired. | Generate new QR. |

**Webhook Example (JSON):**  
```json
{
  "qr_id": "qris_987",
  "status": "SUCCESS",
  "amount": 30000,
  "qr_string": "000201010212...",
  "created": "2023-10-01T11:45:00Z"
}
```

---

## **Handling Callbacks in Code**  
### **Java (Spring Boot Backend)**  
```java
@PostMapping("/xendit-webhook")
public ResponseEntity<String> handleWebhook(@RequestBody String payload, @RequestHeader("x-callback-token") String token) {
    // 1. Verify signature
    if (!verifySignature(token, payload)) {
        return ResponseEntity.status(403).body("Invalid signature");
    }

    // 2. Parse JSON
    JsonNode json = new ObjectMapper().readTree(payload);
    String status = json.get("status").asText();

    // 3. Handle status
    switch (status) {
        case "PAID":
        case "CAPTURED":
            fulfillOrder(json.get("external_id").asText());
            break;
        case "FAILED":
            log.error("Payment failed: " + json.get("failure_reason"));
            break;
        default:
            log.warn("Unhandled status: " + status);
    }
    return ResponseEntity.ok("Webhook processed");
}
```

### **Android (Java)**  
```java
// Polling example
public void checkPaymentStatus(String paymentId) {
    XenditService service = RetrofitClient.getService();
    Call<PaymentStatus> call = service.getStatus(paymentId);
    call.enqueue(new Callback<PaymentStatus>() {
        @Override
        public void onResponse(Call<PaymentStatus> call, Response<PaymentStatus> response) {
            if (response.isSuccessful() && "PAID".equals(response.body().getStatus())) {
                runOnUiThread(() -> showSuccess());
            }
        }
        @Override
        public void onFailure(Call<PaymentStatus> call, Throwable t) {
            Log.e("Xendit", "Error: " + t.getMessage());
        }
    });
}
```

---

## **Key Notes**  
1. **Security:** Always verify webhook signatures using `x-callback-token`.  
2. **Idempotency:** Handle duplicate callbacks (use `external_id` for tracking).  
3. **Testing:** Use [Xendit’s Postman Collection](https://docs.xendit.co/api-reference/) for sandbox testing.  
