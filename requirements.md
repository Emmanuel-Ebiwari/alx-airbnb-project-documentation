# Airbnb Project - Backend Requirements

This document outlines the technical and functional requirements for three core backend features of the Airbnb project: User Authentication, Property Management, and Booking System.

---

## 1. User Authentication

### Functional Requirements
- Allow users to register as Guest or Host.
- Allow registered users to log in securely.
- Provide JWT-based authentication for API access.

### API Endpoints

#### POST `/api/auth/register`

**Input:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securePass123",
  "role": "guest"
}
```

**Validation:**
- Email must be unique and valid.
- Password must be at least 8 characters.
- Role must be either `guest` or `host`.

**Output (Success):**
```json
{
  "message": "User registered successfully",
  "userId": "abc123"
}
```

---

#### POST `/api/auth/login`

**Input:**
```json
{
  "email": "john@example.com",
  "password": "securePass123"
}
```

**Output (Success):**
```json
{
  "token": "jwt_token_here",
  "user": {
    "id": "abc123",
    "name": "John Doe",
    "role": "guest"
  }
}
```

### Performance Criteria
- Response time: ≤ 300ms
- Auth token expiration: 24 hours

---

## 2. Property Management

### Functional Requirements
- Hosts can create, update, and delete listings.
- Listings include title, description, images, price, and availability.

### API Endpoints

#### POST `/api/properties`

**Input:**
```json
{
  "title": "Cozy Apartment in NYC",
  "description": "A clean and safe apartment.",
  "price_per_night": 80,
  "location": "New York",
  "images": ["img1.jpg", "img2.jpg"]
}
```

**Authorization:** Host role required.

---

#### PUT `/api/properties/:id`
- Update listing details.

#### DELETE `/api/properties/:id`
- Delete property listing.

**Output (Success):**
```json
{
  "message": "Property created/updated/deleted successfully"
}
```

### Validation Rules
- Price must be a positive number.
- Title and description must be present.

### Performance Criteria
- Listings retrieval must support pagination and filtering.
- Response time: ≤ 500ms for listing operations.

---

## 3. Booking System

### Functional Requirements
- Guests can book available properties.
- System checks date availability.
- Payments must be successful before confirming booking.

### API Endpoints

#### POST `/api/bookings`

**Input:**
```json
{
  "property_id": "prop123",
  "start_date": "2025-06-10",
  "end_date": "2025-06-15",
  "guests": 2,
  "payment_token": "tok_sample"
}
```

**Output (Success):**
```json
{
  "message": "Booking confirmed",
  "booking_id": "book789"
}
```

### Validation Rules
- Dates must be in the future and not overlap existing bookings.
- Guests count must be within the property limit.

### Performance Criteria
- Prevent double bookings (atomic transaction or lock).
- Booking confirmation response: ≤ 700ms

---

## Notes

- All endpoints should be RESTful and return appropriate status codes (`200`, `201`, `400`, `401`, `404`, `500`).
- Use HTTPS for all data exchanges.
- Implement rate limiting on authentication and booking endpoints.
