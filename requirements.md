# Backend Feature Specifications

## 1. User Authentication

### Overview

Allow users to register, log in, and authenticate securely using hashed passwords and token-based authentication.

### API Endpoints

#### POST `/api/register`

**Input:**

```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "password": "StrongPassword123",
  "role": "guest"
} 

#### Validation Rules:

Email must be valid and unique

Password must be at least 8 characters

Role must be one of: guest, host, admin

#### Output:
{
  "message": "User registered successfully",
  "user_id": "uuid"
}

#### POST /api/login
**Input:**

```json

{
  "email": "john@example.com",
  "password": "StrongPassword123"
}
Output:

{
  "token": "jwt_token",
  "user_id": "uuid",
  "role": "guest"
}

### Performance Criteria
Authentication should complete within 300ms

Token expiration: 24 hours

## 2. Property Management
Overview
Enable hosts to create, update, and delete property listings. Admins can review or moderate listings.

### API Endpoints
POST /api/properties
**Input:**

```json

{
  "host_id": "uuid",
  "name": "Cozy Apartment",
  "description": "2 bedroom apartment near downtown",
  "location": "Nairobi, Kenya",
  "pricepernight": 75.50
}

#### Validation Rules:

Name and location required

Description must not exceed 500 words

Price must be a positive decimal

#### Output:

{
  "message": "Property created successfully",
  "property_id": "uuid"
}
#### GET /api/properties/{id}
####Output:

{
  "property_id": "uuid",
  "name": "Cozy Apartment",
  "description": "2 bedroom apartment near downtown",
  "location": "Nairobi, Kenya",
  "pricepernight": 75.50,
  "host_id": "uuid"
}

###Performance Criteria
Listing creation and update operations should respond within 500ms

Listings should be cached for faster public browsing

## 3. Booking System
Overview
Allow guests to book available properties and view their bookings. Hosts can view bookings for their properties.

### API Endpoints
#### POST /api/bookings
**Input:**

```json

{
  "user_id": "uuid",
  "property_id": "uuid",
  "start_date": "2025-07-10",
  "end_date": "2025-07-15"
}
#### Validation Rules:

Start date must be today or in the future

End date must be after start date

No overlapping bookings for same property

#### Output:

{
  "message": "Booking confirmed",
  "booking_id": "uuid",
  "total_price": 377.50
}
#### GET /api/bookings/{user_id}
#### Output:

[
  {
    "booking_id": "uuid",
    "property_id": "uuid",
    "start_date": "2025-07-10",
    "end_date": "2025-07-15",
    "status": "confirmed",
    "total_price": 377.50
  }
]
### Performance Criteria
Booking confirmation should complete within 700ms

System should handle at least 100 concurrent booking requests
