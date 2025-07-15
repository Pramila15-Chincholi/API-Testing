
# ✅ RESTful Booker API – Test Cases

## 📋 General Information
- **Base URL**: `https://restful-booker.herokuapp.com`
- **Content-Type**: `application/json`
- **Auth Token Required**: For PUT, PATCH, DELETE

---

## 🔁 1. Health Check – `/ping`

| TC ID | Test Case Description | Method | Expected Result |
|-------|------------------------|--------|------------------|
| TC_001 | Verify API health check endpoint | GET `/ping` | 201 Created or 200 OK |

---

## 🔐 2. Auth – `/auth`

| TC ID | Test Case Description | Method | Payload | Expected Result |
|-------|------------------------|--------|---------|------------------|
| TC_002 | Generate token with valid credentials | POST `/auth` | `{ "username": "admin", "password": "password123" }` | Status 200, returns token |
| TC_003 | Fail token generation with invalid password | POST `/auth` | `{ "username": "admin", "password": "wrongpass" }` | 200 OK, empty token or error message |
| TC_004 | Missing required fields | POST `/auth` | `{}` | 400 Bad Request |

---

## 📥 3. Create Booking – `/booking`

| TC ID | Test Case Description | Method | Payload | Expected Result |
|-------|------------------------|--------|---------|------------------|
| TC_005 | Create booking with valid data | POST `/booking` | Valid JSON payload | 200 OK, booking ID returned |
| TC_006 | Create booking with missing required fields | POST `/booking` | Missing `firstname` or `lastname` | 400 Bad Request |
| TC_007 | Invalid data type in price field | POST `/booking` | `"totalprice": "abc"` | 400 Bad Request |
| TC_008 | Create booking with optional field `additionalneeds` empty | POST `/booking` | Empty `"additionalneeds"` | 200 OK |

---

## 📤 4. Get Booking(s) – `/booking` and `/booking/:id`

| TC ID | Test Case Description | Method | Endpoint | Expected Result |
|-------|------------------------|--------|----------|------------------|
| TC_009 | Get all booking IDs | GET | `/booking` | 200 OK, list of booking IDs |
| TC_010 | Get booking by valid ID | GET | `/booking/1` | 200 OK, booking details |
| TC_011 | Get booking by invalid ID | GET | `/booking/99999` | 404 Not Found |
| TC_012 | Get booking filtered by name | GET | `/booking?firstname=Jim&lastname=Brown` | 200 OK, filtered list |

---

## 🛠️ 5. Update Booking – `/booking/:id` (PUT)

| TC ID | Test Case Description | Method | Auth | Payload | Expected Result |
|-------|------------------------|--------|------|---------|------------------|
| TC_013 | Update booking with valid token | PUT `/booking/:id` | ✅ Yes | Full booking object | 200 OK |
| TC_014 | Update booking without token | PUT `/booking/:id` | ❌ No | Valid payload | 403 Forbidden |
| TC_015 | Update with invalid token | PUT `/booking/:id` | ❌ Invalid | Valid payload | 403 Forbidden |
| TC_016 | Update non-existent booking ID | PUT `/booking/99999` | ✅ Yes | Valid payload | 404 Not Found or 405 |

---

## 🧩 6. Partial Update – `/booking/:id` (PATCH)

| TC ID | Test Case Description | Method | Auth | Payload | Expected Result |
|-------|------------------------|--------|------|---------|------------------|
| TC_017 | Patch booking firstname | PATCH `/booking/:id` | ✅ Yes | `{ "firstname": "John" }` | 200 OK |
| TC_018 | Patch with invalid data type | PATCH `/booking/:id` | ✅ Yes | `{ "totalprice": "free" }` | 400 Bad Request |
| TC_019 | Patch without token | PATCH `/booking/:id` | ❌ No | Any | 403 Forbidden |

---

## 🗑️ 7. Delete Booking – `/booking/:id`

| TC ID | Test Case Description | Method | Auth | Expected Result |
|-------|------------------------|--------|------|------------------|
| TC_020 | Delete booking with valid token | DELETE `/booking/:id` | ✅ Yes | 201 Created |
| TC_021 | Delete booking without token | DELETE `/booking/:id` | ❌ No | 403 Forbidden |
| TC_022 | Delete non-existent booking | DELETE `/booking/99999` | ✅ Yes | 405 or 404 |

---

## 🔐 8. Security Test Cases

| TC ID | Test Case Description | Method | Endpoint | Expected Result |
|-------|------------------------|--------|----------|------------------|
| TC_023 | Access protected endpoint without token | PUT `/booking/:id` | 403 Forbidden |
| TC_024 | Access with malformed token | DELETE `/booking/:id` | 403 Forbidden |
| TC_025 | XSS injection in firstname | POST `/booking` | Should escape `<script>` tags |
| TC_026 | SQL injection attempt | POST `/booking` with `' OR '1'='1` in fields | No impact; validation error |

---

## 📉 9. Performance (Optional)

| TC ID | Test Case Description | Tool | Expected Result |
|-------|------------------------|------|------------------|
| TC_027 | Run 100 concurrent GET `/booking` | JMeter | All respond < 2s |
| TC_028 | POST 50 bookings in 1 min | Script | All succeed with 200 OK |
