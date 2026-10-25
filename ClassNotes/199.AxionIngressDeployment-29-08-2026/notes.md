# Class Notes: Axion Systems: End-to-End Ingress Verification & Testing 🧪

**Date:** 29-08-2026  
**Topic:** Verifying Complete Request Traversal from Browser to Microservices and Database

---

## 📌 1. The Complete End-to-End Request Journey

```
 1. User opens https://axion.vistara.website in browser.
 2. Public DNS resolves hostname to Application Gateway Public IP.
 3. Application Gateway terminates TLS (SSL certificate verified).
 4. AGIC Ingress rules forward traffic to the axion-ui Service.
 5. axion-ui React bundle loads in user's browser.
 6. React UI sends AJAX call to https://api.vistara.website/telemetry.
 7. Ingress routes request to axion-query Service.
 8. axion-query queries PostgreSQL for live metrics.
 9. PostgreSQL returns real-time sensor metrics; UI graphs temperature/RPM!
```

---

## 💡 Key Takeaways
1. Verify each link in the communication chain sequentially when debugging complex microservices.
2. Use browser developer tools (Network tab) to confirm API response payloads and CORS headers.
