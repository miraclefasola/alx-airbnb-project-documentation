# 🏠 Airbnb Clone Backend Requirements

## 📘 Overview
This document outlines the **technical and functional requirements** for the core backend features of the Airbnb Clone project.  
Each feature includes details about **API endpoints**, **input/output specifications**, **validation rules**, and **performance expectations**.

---

## 1. 🔐 User Authentication & Authorization

### Functional Description
Handles secure user registration, login, and role-based access control (guest, host, admin).

### API Endpoints

| Method | Endpoint | Description |
|---------|-----------|-------------|
| `POST` | `/api/v1/auth/register` | Register a new user |
| `POST` | `/api/v1/auth/login` | Authenticate and generate JWT token |
| `GET` | `/api/v1/auth/profile` | Retrieve user profile |
| `PUT` | `/api/v1/auth/update-password` | Change user password |

### Input / Output

#### Request (Register)

{
  "first_name": "Mimi",
  "last_name": "Fasola",
  "email": "mimi@example.com",
  "password": "strongpassword123",
  "role": "host"
}
Response (Success)


{
  "message": "User registered successfully",
  "user_id": "uuid-generated",
  "token": "jwt.token.string"
}
## Validation Rules

email must be unique and in valid format.

password must contain at least 8 characters.

role defaults to guest if not specified.

Performance Criteria
Authentication requests must complete within 500ms.

Passwords are securely hashed using bcrypt or Argon2.

JWT tokens expire after 24 hours.

## 2. 🏡 Property Management
Functional Description
Allows hosts to create, update, and delete property listings. Guests can view and filter available properties.

API Endpoints
Method	Endpoint	Description
POST	/api/v1/properties	Create a new property (Host only)
GET	/api/v1/properties	Get all available properties
GET	/api/v1/properties/:id	Get property details
PUT	/api/v1/properties/:id	Update property details (Host only)
DELETE	/api/v1/properties/:id	Remove a property listing

Input / Output
Request (Create)
json

{
  "name": "Luxury Apartment",
  "description": "A 2-bedroom apartment with ocean view.",
  "location": "Lagos, Nigeria",
  "price_per_night": 200.00
}
Response (Success)
json

{
  "property_id": "uuid-generated",
  "message": "Property created successfully"
}
Validation Rules
name, description, and location cannot be empty.

price_per_night must be numeric and greater than zero.

Only authenticated users with the role host can create or modify properties.

Performance Criteria
Must handle up to 500 concurrent read requests efficiently.

Paginated results for property listings (max 20 per page).

3. 📅 Booking System
Functional Description
Manages bookings between guests and hosts, including date validation, payment linkage, and cancellation policies.

## API Endpoints
Method	Endpoint	Description
POST	/api/v1/bookings	Create a new booking
GET	/api/v1/bookings/:id	Retrieve a booking
PUT	/api/v1/bookings/:id/cancel	Cancel a booking
GET	/api/v1/bookings/user/:id	Get all bookings by user

Input / Output
Request (Create)
json

{
  "property_id": "uuid-of-property",
  "user_id": "uuid-of-user",
  "start_date": "2025-11-05",
  "end_date": "2025-11-08"
}
Response (Success)
json
Copy code
{
  "booking_id": "uuid-generated",
  "status": "confirmed",
  "total_price": 600.00,
  "message": "Booking confirmed successfully"
}
Validation Rules
start_date must be before end_date.

Prevent overlapping bookings for the same property.

Total price = price_per_night * number_of_days.

Performance Criteria
Ensure booking creation completes within 1 second.

Use database indexing on property_id and user_id for faster lookups.