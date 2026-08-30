# Auth Testing Playbook

1. MongoDB verification:
   - `mongosh` → `use test_database`
   - `db.users.find({role: "super_admin"})` should show admin@sabzimandi.com with bcrypt password_hash starting `$2b$`
   - Indexes: users.mobile (unique sparse), users.email (unique sparse), products.sku (unique), orders.id (unique), otps.expires_at (TTL)

2. API testing:
```
API=http://localhost:8001
# OTP customer flow
curl -X POST $API/api/auth/send-otp -H "Content-Type: application/json" -d '{"identifier":"9876543210"}'
# response includes test_otp (123456 in test mode)
curl -X POST $API/api/auth/verify-otp -H "Content-Type: application/json" -d '{"identifier":"9876543210","otp":"123456","name":"Test User"}'
# → {token, user}
TOKEN=...
curl $API/api/auth/me -H "Authorization: Bearer $TOKEN"

# Admin flow
curl -X POST $API/api/auth/admin-login -H "Content-Type: application/json" -d '{"email":"admin@sabzimandi.com","password":"Admin@123"}'
```

3. Brute force: 5 wrong admin passwords from same IP+email → 429 lockout for 15 min.
4. OTP rules: 2-minute expiry, 5 attempt limit, max 10 OTP sends/hour/identifier.
