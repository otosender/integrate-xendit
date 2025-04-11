Here’s the **reorganized documentation** with payment methods categorized into **topics and subtopics** for clarity:

---

# **Xendit Payment Integration Guide**  
*Last updated: [Date]*  

## **1. Preparation**  
### **Account & Keys**  
- **Secret API Key**: `xnd_development_Q8RrwXTaCJOJlA7ck87f5y2E9CNeXuYfpDi5RlPXClmytVGQjIro2Bn6MaB6Qrr` *(Server-side only!)*  
- **Public Key**: `xnd_public_development_OfYSJLBmLVc4oWjkOgzd0QJ5HSJwCXLtg0FXm13tU_Zg9vFP0coayC2qN_ztlKer`  

### **Dependency**  
```gradle
implementation 'com.xendit:xendit-android:{latest_version}'
```

---

## **2. Payment Methods**  
### **Topic 1: Virtual Account (VA)**  
#### **Subtopic: BNI/Mandiri/Other Banks**  
- **Initialize VA**:  
  ```java
  xendit.createVA("bni", 100000, "order-123", new XenditCallback<VirtualAccount>() { ... });
  ```  
- **Webhook Handling**:  
  ```json
  { "status": "PAID", "account_number": "9889901234567890" }
  ```  

#### **Subtopic: Callback Statuses**  
| Status | Action |  
|--------|--------|  
| `PAID` | Fulfill order |  
| `EXPIRED` | Regenerate VA |  

---

### **Topic 2: QRIS (QR Code Payments)**  
#### **Subtopic: Static QR**  
- **Generate QR**:  
  ```java
  xendit.createQRCode(50000, "IDR", "order-456", new XenditCallback<QRCode>() { ... });
  ```  
- **Webhook**:  
  ```json
  { "status": "SUCCEEDED", "qr_string": "000201010212..." }
  ```  

#### **Subtopic: Dynamic QR**  
- **For variable amounts**: Use `is_dynamic: true` in API request.  

---

### **Topic 3: E-Wallets (OVO/DANA/LinkAja)**  
#### **Subtopic: OVO Payments**  
- **Charge**:  
  ```java
  xendit.chargeEwallet("OVO", "08123456789", 10000, new XenditCallback<EwalletCharge>() { ... });
  ```  
- **Deep Link**: Redirect to `ovo://payment?trxId=123`.  

#### **Subtopic: DANA Payments**  
- **Webhook**:  
  ```json
  { "status": "COMPLETED", "ewallet_type": "DANA" }
  ```  

---

### **Topic 4: Credit/Debit Cards**  
#### **Subtopic: Tokenization**  
- **Get Token**:  
  ```java
  xendit.createToken(card, new XenditCallback<Token>() { ... });
  ```  
- **Charge**: Server-side only.  

#### **Subtopic: 3DS Authentication**  
- Handle `AUTHENTICATION_REQUIRED` status.  

---

### **Topic 5: Retail Outlets (Alfamart/Indomaret)**  
#### **Subtopic: Payment Code**  
- **Generate**:  
  ```java
  xendit.createRetailOutlet("ALFAMART", 120000, "order-789", new XenditCallback<RetailOutlet>() { ... });
  ```  

---

## **3. Common Utilities**  
### **Subtopic: Check Activated Payment Channels**  
```java
// Fetch all enabled methods
xendit.getPaymentChannels(new XenditCallback<List<PaymentChannel>>() {
    @Override
    public void onSuccess(List<PaymentChannel> channels) {
        // Display: channels.get(0).getName() → "QRIS", "OVO", etc.
    }
});
```

### **Subtopic: Webhook Security**  
```java
if (!Xendit.verifyWebhookSignature(requestBody, "xnd_webhook_123")) {
    throw new SecurityException("Invalid signature");
}
```

---

## **4. Best Practices**  
- **Dynamic Channels**: Always fetch payment methods from API (don’t hardcode).  
- **Error Handling**:  
  ```java
  xendit.setErrorHandler(error -> {
      if (error.getErrorCode().equals("INSUFFICIENT_BALANCE")) {
          showTopUpPrompt();
      }
  });
  ```  

---

## **5. Appendix**  
### **Payment Channel Reference Table**  
| Channel | API Key | Webhook Event |  
|---------|---------|---------------|  
| QRIS | `qr_` prefix | `qr.paid` |  
| OVO | `ovo_` prefix | `ewallet.credit` |  
| BNI VA | `va_` prefix | `va.paid` |  

---

**Need visual workflows?** See [Xendit’s Integration Diagrams](https://docs.xendit.co/diagrams).  

Let me know if you'd like to expand any subtopic!