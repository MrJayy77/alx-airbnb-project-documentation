Backend Requirements Specifications – ALX Airbnb Project
1. User Authentication
Overview

The User Authentication module allows users (guests, hosts, and admins) to securely register, log in, and manage their accounts.

Functional Requirements

Users must be able to:

Register with email, password, and role (guest, host).

Log in using valid credentials.

Reset forgotten passwords.

Update profile information.

API Endpoints

POST /api/auth/register

Input: { "name": "John Doe", "email": "john@example.com", "password": "********", "role": "host" }

Output: { "message": "User registered successfully", "userId": "12345" }

POST /api/auth/login

Input: { "email": "john@example.com", "password": "********" }

Output: { "token": "JWT_TOKEN", "userId": "12345", "role": "host" }

POST /api/auth/reset-password

Input: { "email": "john@example.com" }

Output: { "message": "Password reset link sent to email" }

Validation Rules

Password must be at least 8 characters, with one uppercase, one number, and one special character.

Email must be unique.

Role must be either guest or host.

Performance Criteria

Login should respond within 2 seconds.

Authentication requests must handle 500 concurrent users.

2. Property Management
Overview

Hosts can list, update, and manage their properties. Guests can view property details.

Functional Requirements

Hosts can:

Add new properties with images, descriptions, pricing, and availability.

Edit property details.

Delete properties.

Guests can:

Browse and search available properties.

View property details.

API Endpoints

POST /api/properties

Input: { "title": "Beach House", "description": "2-bed beachfront", "price": 120, "capacity": 4, "availability": ["2025-09-01", "2025-09-10"], "hostId": "12345" }

Output: { "message": "Property created successfully", "propertyId": "56789" }

GET /api/properties

Output: [ { "id": "56789", "title": "Beach House", "price": 120, "capacity": 4 } ]

PUT /api/properties/{id}

Input: { "price": 150, "availability": ["2025-09-15", "2025-09-30"] }

Output: { "message": "Property updated successfully" }

DELETE /api/properties/{id}

Output: { "message": "Property deleted successfully" }

Validation Rules

Price must be a positive number.

Capacity must be greater than 0.

Availability dates must follow YYYY-MM-DD.

Performance Criteria

Must support image uploads up to 5MB per file.

API must respond within 3 seconds for property queries.

3. Booking System
Overview

The booking system enables guests to book properties and hosts to manage bookings.

Functional Requirements

Guests can:

Create bookings by selecting available dates.

View booking history.

Cancel bookings.

Hosts can:

Accept or decline booking requests.

View all bookings for their properties.

API Endpoints

POST /api/bookings

Input: { "guestId": "78901", "propertyId": "56789", "startDate": "2025-09-05", "endDate": "2025-09-10" }

Output: { "message": "Booking created successfully", "bookingId": "98765" }

GET /api/bookings/guest/{guestId}

Output: [ { "bookingId": "98765", "property": "Beach House", "status": "confirmed" } ]

GET /api/bookings/host/{hostId}

Output: [ { "bookingId": "98765", "guest": "John Doe", "status": "pending" } ]

PUT /api/bookings/{id}

Input: { "status": "cancelled" }

Output: { "message": "Booking updated successfully" }

Validation Rules

Start and end dates must not overlap with existing bookings.

Guests cannot book their own properties.

Cancellation is only allowed up to 24 hours before check-in.

Performance Criteria

Booking creation should complete in under 2 seconds.

The system must handle 1,000 bookings per minute during peak usage.