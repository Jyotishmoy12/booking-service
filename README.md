

### MySQL Database for booking:

<img width="1529" height="473" alt="image" src="https://github.com/user-attachments/assets/901d1593-f160-4205-98b5-923787cac684" />

### MySQL Database for seats:

<img width="1239" height="514" alt="image" src="https://github.com/user-attachments/assets/e2ff766b-b422-4e89-8c88-001cef3ff719" />


````markdown
# Booking & Info API Documentation

Base URL: `/api/v1`

---

## 1. Service Info

**Endpoint:**  
`GET /api/v1/info`

**Purpose:**  
Check if the service is running and fetch basic info about the API.

**Response (200 - OK):**  
```json
{
  "success": true,
  "message": "API is up and running",
  "data": {
    "service": "Flight Booking API",
    "version": "1.0.0"
  },
  "error": {}
}
````

---

## 2. Create Booking

**Endpoint:**
`POST /api/v1/bookings`

**Purpose:**
Create a booking for a given flight and user.

**Request Payload:**

```json
{
  "flightId": 1,
  "userId": 101,
  "noofSeats": 2
}
```

**Response (200 - OK):**

```json
{
  "success": true,
  "message": "Successfully completed the request",
  "data": {
    "id": 5001,
    "flightId": 1,
    "userId": 101,
    "noofSeats": 2,
    "status": "BOOKED",
    "totalCost": 200,
    "createdAt": "2025-08-16T12:45:00.000Z"
  },
  "error": {}
}
```

**Response (500 - Server Error):**

```json
{
  "success": false,
  "message": "Something went wrong",
  "data": {},
  "error": {
    "statusCode": 500,
    "explanation": "Could not process booking"
  }
}
```

---

## 3. Make Payment

**Endpoint:**
`POST /api/v1/bookings/payments`

**Headers Required:**

* `x-idempotency-key: <unique-key>` (Prevents duplicate payment requests)

**Purpose:**
Process a payment for a booking.

**Request Payload:**

```json
{
  "totalCost": 200,
  "userId": 101,
  "bookingId": 5001
}
```

**Response (200 - OK):**

```json
{
  "success": true,
  "message": "Payment successful",
  "data": {
    "paymentId": 9001,
    "bookingId": 5001,
    "amount": 200,
    "status": "PAID",
    "createdAt": "2025-08-16T12:50:00.000Z"
  },
  "error": {}
}
```

**Response (400 - Missing Idempotency Key):**

```json
{
  "message": "Idempotency key is required"
}
```

**Response (400 - Duplicate Request):**

```json
{
  "message": "Cant process the same request twice"
}
```

---

## Notes

* **Idempotency Handling:**

  * Payments require `x-idempotency-key` in headers.
  * If the same key is reused, the request will be rejected to prevent double-charging.

* **Booking Lifecycle:**

  * `BOOKED` → Created booking (before payment)
  * `PAID` → Payment successful

* Response structure remains consistent:

  ```json
  {
    "success": boolean,
    "message": string,
    "data": object | {},
    "error": object | {}
  }
  ```




