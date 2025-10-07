# uber-clone


## API Endpoints

### Auth Service (http://localhost/api/v1/auth)

#### POST /register
Register a new user

**Request:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+998901234567",
  "password": "SecurePass123",
  "role": "passenger"
}
```

**Response:**
```json
{
  "success": true,
  "user": {
    "id": "uuid",
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "+998901234567",
    "role": "passenger",
    "created_at": "2024-01-15T10:30:00Z"
  },
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "refresh_token_here"
}
```

#### POST /login
User login

**Request:**
```json
{
  "email": "john@example.com",
  "password": "SecurePass123"
}
```

**Response:**
```json
{
  "success": true,
  "user": {
    "id": "uuid",
    "name": "John Doe",
    "email": "john@example.com",
    "role": "passenger"
  },
  "token": "jwt_token_here",
  "refresh_token": "refresh_token_here"
}
```

#### POST /refresh
Refresh access token

**Request:**
```json
{
  "refresh_token": "refresh_token_here"
}
```

**Response:**
```json
{
  "success": true,
  "token": "new_jwt_token_here"
}
```

#### DELETE /logout
Logout user

**Headers:**
```
Authorization: Bearer jwt_token_here
```

**Response:**
```json
{
  "success": true,
  "message": "Logged out successfully"
}
```

#### POST /forgot-password
Request password reset

**Request:**
```json
{
  "email": "john@example.com"
}
```

#### POST /reset-password
Reset password with token

**Request:**
```json
{
  "token": "reset_token_here",
  "password": "NewPassword123"
}
```

---

### User Service (http://localhost/api/v1/users)

#### GET /profile/:userId
Get user profile

**Headers:**
```
Authorization: Bearer jwt_token_here
```

**Response:**
```json
{
  "id": "uuid",
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+998901234567",
  "avatar_url": "https://example.com/avatar.jpg",
  "rating": 4.8,
  "total_rides": 25,
  "created_at": "2024-01-15T10:30:00Z"
}
```

#### PUT /profile/:userId
Update user profile

**Request:**
```json
{
  "name": "John Updated",
  "avatar_url": "https://example.com/new-avatar.jpg"
}
```

#### GET /profile/:userId/addresses
Get user saved addresses

**Response:**
```json
{
  "addresses": [
    {
      "id": "uuid",
      "label": "home",
      "address": "123 Main St, Tashkent",
      "latitude": 41.3111,
      "longitude": 69.2797,
      "is_default": true
    },
    {
      "id": "uuid",
      "label": "work",
      "address": "456 Business Ave, Tashkent",
      "latitude": 41.3275,
      "longitude": 69.2819,
      "is_default": false
    }
  ]
}
```

#### POST /profile/:userId/addresses
Add new address

**Request:**
```json
{
  "label": "home",
  "address": "123 Main St, Tashkent",
  "latitude": 41.3111,
  "longitude": 69.2797,
  "is_default": true
}
```

#### DELETE /profile/:userId/addresses/:addressId
Delete address

---

### Driver Service (http://localhost/api/v1/drivers)

#### POST /register
Register as driver

**Request:**
```json
{
  "user_id": "uuid",
  "license_number": "AA1234567",
  "license_expiry": "2026-12-31",
  "vehicle": {
    "make": "Toyota",
    "model": "Camry",
    "year": 2020,
    "color": "White",
    "license_plate": "01A123BC",
    "vehicle_type": "economy"
  },
  "documents": [
    {
      "type": "license",
      "url": "https://example.com/license.jpg"
    },
    {
      "type": "insurance",
      "url": "https://example.com/insurance.jpg"
    }
  ]
}
```

**Response:**
```json
{
  "success": true,
  "driver_id": "uuid",
  "status": "pending_verification",
  "message": "Driver application submitted successfully"
}
```

#### GET /:driverId
Get driver details

**Response:**
```json
{
  "id": "uuid",
  "user_id": "uuid",
  "license_number": "AA1234567",
  "is_verified": true,
  "is_online": false,
  "rating": 4.9,
  "total_rides": 156,
  "vehicle": {
    "make": "Toyota",
    "model": "Camry",
    "year": 2020,
    "color": "White",
    "license_plate": "01A123BC",
    "vehicle_type": "economy"
  },
  "earnings_today": 250000,
  "created_at": "2024-01-10T08:00:00Z"
}
```

#### PUT /:driverId/status
Update driver online status

**Request:**
```json
{
  "is_online": true
}
```

#### GET /:driverId/earnings
Get driver earnings

**Query Params:**
```
?from=2024-01-01&to=2024-01-31
```

**Response:**
```json
{
  "total_earnings": 5000000,
  "total_rides": 250,
  "average_per_ride": 20000,
  "breakdown": [
    {
      "date": "2024-01-15",
      "earnings": 180000,
      "rides": 9
    }
  ]
}
```

#### GET /:driverId/ratings
Get driver ratings and reviews

**Response:**
```json
{
  "average_rating": 4.9,
  "total_reviews": 156,
  "rating_breakdown": {
    "5": 140,
    "4": 12,
    "3": 3,
    "2": 1,
    "1": 0
  },
  "recent_reviews": [
    {
      "rating": 5,
      "comment": "Excellent driver, very professional",
      "passenger_name": "John D.",
      "date": "2024-01-15"
    }
  ]
}
```

---

### Ride Service (http://localhost/api/v1/rides)

#### POST /request
Request a ride

**Request:**
```json
{
  "passenger_id": "uuid",
  "pickup_latitude": 41.3111,
  "pickup_longitude": 69.2797,
  "dropoff_latitude": 41.3275,
  "dropoff_longitude": 69.2819,
  "pickup_address": "Amir Timur Square, Tashkent",
  "dropoff_address": "Tashkent City Mall",
  "vehicle_type": "economy",
  "passenger_notes": "Please call when you arrive"
}
```

**Response:**
```json
{
  "success": true,
  "request_id": "uuid",
  "status": "requested",
  "estimated_fare": 25000,
  "distance_km": 5.5,
  "duration_minutes": 12,
  "nearby_drivers": 5,
  "message": "Finding nearby drivers..."
}
```

#### GET /:rideId
Get ride details

**Response:**
```json
{
  "id": "uuid",
  "passenger_id": "uuid",
  "driver_id": "uuid",
  "pickup_location": {
    "latitude": 41.3111,
    "longitude": 69.2797,
    "address": "Amir Timur Square, Tashkent"
  },
  "dropoff_location": {
    "latitude": 41.3275,
    "longitude": 69.2819,
    "address": "Tashkent City Mall"
  },
  "status": "started",
  "vehicle_type": "economy",
  "estimated_fare": 25000,
  "final_fare": null,
  "distance_km": 5.5,
  "duration_minutes": 12,
  "driver": {
    "id": "uuid",
    "name": "Driver Name",
    "phone": "+998901234568",
    "rating": 4.9,
    "vehicle": {
      "make": "Toyota",
      "model": "Camry",
      "color": "White",
      "license_plate": "01A123BC"
    }
  },
  "requested_at": "2024-01-15T10:30:00Z",
  "accepted_at": "2024-01-15T10:31:00Z",
  "started_at": "2024-01-15T10:45:00Z",
  "completed_at": null
}
```

#### PUT /:rideId/accept
Driver accepts ride

**Request:**
```json
{
  "driver_id": "uuid"
}
```

**Response:**
```json
{
  "success": true,
  "ride_id": "uuid",
  "status": "accepted",
  "pickup_eta": 5,
  "message": "Ride accepted successfully"
}
```

#### PUT /:rideId/start
Start the ride

**Response:**
```json
{
  "success": true,
  "ride_id": "uuid",
  "status": "started",
  "started_at": "2024-01-15T10:45:00Z"
}
```

#### PUT /:rideId/complete
Complete the ride

**Request:**
```json
{
  "final_fare": 28000,
  "distance_km": 6.2,
  "duration_minutes": 15
}
```

**Response:**
```json
{
  "success": true,
  "ride_id": "uuid",
  "status": "completed",
  "final_fare": 28000,
  "payment_status": "pending"
}
```

#### PUT /:rideId/cancel
Cancel the ride

**Request:**
```json
{
  "cancelled_by": "uuid",
  "cancellation_reason": "Driver not available"
}
```

#### POST /:rideId/rate
Rate the ride

**Request:**
```json
{
  "rater_id": "uuid",
  "rated_id": "uuid",
  "rating": 5,
  "comment": "Excellent service!"
}
```

#### GET /history/:userId
Get ride history

**Query Params:**
```
?limit=20&offset=0&status=completed
```

**Response:**
```json
{
  "rides": [
    {
      "id": "uuid",
      "pickup_address": "Amir Timur Square",
      "dropoff_address": "Tashkent City Mall",
      "status": "completed",
      "fare": 28000,
      "distance_km": 6.2,
      "date": "2024-01-15T10:45:00Z",
      "driver_name": "Driver Name",
      "rating": 5
    }
  ],
  "total": 25,
  "page": 1
}
```

#### GET /active/:userId
Get active ride for user

**Response:**
```json
{
  "has_active_ride": true,
  "ride": {
    "id": "uuid",
    "status": "started",
    "driver": {...},
    "current_location": {...}
  }
}
```

---

### Location Service (http://localhost/api/v1/location)

#### POST /update
Update user location

**Request:**
```json
{
  "user_id": "uuid",
  "ride_id": "uuid",
  "latitude": 41.3111,
  "longitude": 69.2797,
  "heading": 45.5,
  "speed": 35.2,
  "accuracy": 10.0
}
```

**Response:**
```json
{
  "success": true,
  "message": "Location updated successfully"
}
```

#### GET /nearby-drivers
Find nearby drivers

**Query Params:**
```
?latitude=41.3111&longitude=69.2797&radius=5&vehicle_type=economy
```

**Response:**
```json
{
  "drivers": [
    {
      "driver_id": "uuid",
      "name": "Driver Name",
      "latitude": 41.3115,
      "longitude": 69.2800,
      "distance_km": 0.5,
      "eta_minutes": 3,
      "rating": 4.9,
      "vehicle_type": "economy",
      "last_update": "2024-01-15T10:30:00Z"
    }
  ],
  "total": 5
}
```

#### POST /calculate-route
Calculate route between two points

**Request:**
```json
{
  "pickup_latitude": 41.3111,
  "pickup_longitude": 69.2797,
  "dropoff_latitude": 41.3275,
  "dropoff_longitude": 69.2819
}
```

**Response:**
```json
{
  "distance_km": 5.5,
  "duration_minutes": 12,
  "estimated_fare": 25000,
  "route_polyline": "encoded_polyline_string",
  "waypoints": [
    {
      "latitude": 41.3111,
      "longitude": 69.2797
    },
    {
      "latitude": 41.3150,
      "longitude": 69.2810
    },
    {
      "latitude": 41.3275,
      "longitude": 69.2819
    }
  ]
}
```

#### GET /eta
Get estimated time of arrival

**Query Params:**
```
?from_lat=41.3111&from_lng=69.2797&to_lat=41.3275&to_lng=69.2819
```

**Response:**
```json
{
  "eta_minutes": 12,
  "distance_km": 5.5,
  "current_traffic": "moderate"
}
```

#### GET /driver/:driverId/location
Get driver's current location

**Response:**
```json
{
  "driver_id": "uuid",
  "latitude": 41.3115,
  "longitude": 69.2800,
  "heading": 45.5,
  "speed": 35.2,
  "timestamp": "2024-01-15T10:30:00Z"
}
```

#### GET /ride/:rideId/route
Get ride route history

**Response:**
```json
{
  "ride_id": "uuid",
  "route": [
    {
      "latitude": 41.3111,
      "longitude": 69.2797,
      "timestamp": "2024-01-15T10:30:00Z"
    },
    {
      "latitude": 41.3115,
      "longitude": 69.2800,
      "timestamp": "2024-01-15T10:31:00Z"
    }
  ],
  "total_distance_km": 5.5
}
```

---

### Payment Service (http://localhost/api/v1/payments)

#### POST /methods
Add payment method

**Request:**
```json
{
  "user_id": "uuid",
  "type": "card",
  "card_token": "stripe_token_here",
  "card_last_four": "4242",
  "card_brand": "Visa",
  "is_default": true
}
```

**Response:**
```json
{
  "success": true,
  "payment_method_id": "uuid",
  "message": "Payment method added successfully"
}
```

#### GET /methods/:userId
Get user payment methods

**Response:**
```json
{
  "payment_methods": [
    {
      "id": "uuid",
      "type": "card",
      "card_last_four": "4242",
      "card_brand": "Visa",
      "is_default": true
    },
    {
      "id": "uuid",
      "type": "wallet",
      "wallet_balance": 50000,
      "is_default": false
    }
  ]
}
```

#### DELETE /methods/:methodId
Delete payment method

#### POST /process
Process payment

**Request:**
```json
{
  "ride_id": "uuid",
  "payer_id": "uuid",
  "payment_method_id": "uuid",
  "amount": 28000,
  "currency": "UZS"
}
```

**Response:**
```json
{
  "success": true,
  "transaction_id": "uuid",
  "status": "completed",
  "amount": 28000,
  "gateway_transaction_id": "stripe_transaction_id"
}
```

#### GET /transactions/:userId
Get transaction history

**Response:**
```json
{
  "transactions": [
    {
      "id": "uuid",
      "ride_id": "uuid",
      "amount": 28000,
      "status": "completed",
      "payment_method": "Visa ****4242",
      "date": "2024-01-15T11:00:00Z"
    }
  ],
  "total": 25
}
```

#### GET /wallet/:userId
Get wallet balance

**Response:**
```json
{
  "user_id": "uuid",
  "balance": 50000,
  "currency": "UZS",
  "last_transaction": "2024-01-15T11:00:00Z"
}
```

#### POST /wallet/:userId/topup
Top up wallet

**Request:**
```json
{
  "amount": 100000,
  "payment_method_id": "uuid"
}
```

---

### Notification Service (http://localhost/api/v1/notifications)

#### POST /send
Send notification

**Request:**
```json
{
  "user_id": "uuid",
  "type": "push",
  "title": "Ride Accepted",
  "message": "Your ride has been accepted by the driver",
  "data": {
    "ride_id": "uuid",
    "action": "open_ride"
  }
}
```

#### POST /send-sms
Send SMS notification

**Request:**
```json
{
  "phone": "+998901234567",
  "message": "Your ride is arriving in 2 minutes"
}
```

#### POST /send-email
Send email notification

**Request:**
```json
{
  "email": "user@example.com",
  "subject": "Ride Receipt",
  "template": "ride_receipt",
  "data": {
    "ride_id": "uuid",
    "amount": 28000
  }
}
```

#### GET /preferences/:userId
Get notification preferences

**Response:**
```json
{
  "push_enabled": true,
  "sms_enabled": true,
  "email_enabled": true,
  "ride_updates": true,
  "promotions": false
}
```

#### PUT /preferences/:userId
Update notification preferences

---

### Analytics Service (http://localhost/api/v1/analytics)

#### GET /dashboard
Get dashboard statistics

**Query Params:**
```
?from=2024-01-01&to=2024-01-31
```

**Response:**
```json
{
  "total_rides": 1500,
  "total_revenue": 45000000,
  "active_users": 250,
  "active_drivers": 80,
  "average_rating": 4.8,
  "completed_rides": 1420,
  "cancelled_rides": 80,
  "cancellation_rate": 5.3,
  "peak_hours": [
    { "hour": 8, "rides": 120 },
    { "hour": 18, "rides": 150 }
  ],
  "popular_routes": [
    {
      "from": "Amir Timur Square",
      "to": "Tashkent City",
      "count": 85
    }
  ]
}
```

#### GET /driver/:driverId/stats
Get driver statistics

**Response:**
```json
{
  "driver_id": "uuid",
  "total_rides": 156,
  "total_earnings": 3500000,
  "average_rating": 4.9,
  "acceptance_rate": 95.5,
  "completion_rate": 98.7,
  "average_response_time": 30,
  "online_hours_today": 8.5
}
```

#### GET /user/:userId/stats
Get user statistics

**Response:**
```json
{
  "user_id": "uuid",
  "total_rides": 25,
  "total_spent": 650000,
  "average_fare": 26000,
  "favorite_destination": "Tashkent City Mall",
  "most_used_vehicle_type": "economy"
}
```

#### GET /revenue
Get revenue analytics

**Response:**
```json
{
  "total_revenue": 45000000,
  "platform_commission": 9000000,
  "driver_earnings": 36000000,
  "daily_breakdown": [
    {
      "date": "2024-01-15",
      "revenue": 1500000,
      "rides": 65
    }
  ]
}
```

---

## WebSocket Endpoints

### Real-time Service (ws://localhost:3000)

#### Passenger - Ride Updates
```
ws://localhost:3000/ws/passenger/:userId/ride
```

**Incoming Messages:**
```json
{
  "type": "connected",
  "data": "Connected to ride updates",
  "timestamp": 1705315800
}

{
  "type": "ride_accepted",
  "request_id": "uuid",
  "driver_id": "uuid",
  "data": "Ride accepted by driver",
  "timestamp": 1705315800
}

{
  "type": "ride_started",
  "request_id": "uuid",
  "data": "Ride started",
  "timestamp": 1705315900
}

{
  "type": "ride_completed",
  "request_id": "uuid",
  "data": "Ride completed",
  "timestamp": 1705316800
}
```

#### Passenger - Driver Location
```
ws://localhost:3000/ws/passenger/:userId/location?request_id=uuid
```

**Incoming Messages:**
```json
{
  "type": "driver_location",
  "request_id": "uuid",
  "driver_id": "uuid",
  "data": {
    "lat": 41.3111,
    "lng": 69.2797,
    "heading": 45.5,
    "speed": 35.2
  },
  "timestamp": 1705315800
}
```

#### Driver - Ride Requests
```
ws://localhost:3000/ws/driver/:driverId/requests
```

**Incoming Messages:**
```json
{
  "type": "ride_requested",
  "request_id": "uuid",
  "user_id": "uuid",
  "data": {
    "id": "uuid",
    "passenger_id": "uuid",
    "pickup_lat": 41.3111,
    "pickup_lng": 69.2797,
    "dropoff_lat": 41.3275,
    "dropoff_lng": 69.2819,
    "pickup_address": "Amir Timur Square",
    "dropoff_address": "Tashkent City",
    "estimated_fare": 25000,
    "distance_km": 5.5
  },
  "timestamp": 1705315800
}
```

**Outgoing Messages:**
```json
{
  "type": "accept_ride",
  "request_id": "uuid"
}

{
  "type": "start_ride",
  "request_id": "uuid"
}

{
  "type": "complete_ride",
  "request_id": "uuid"
}
```

#### Driver - Location Updates
```
ws://localhost:3000/ws/driver/:driverId/location
```

**Outgoing Messages:**
```json
{
  "request_id": "uuid",
  "location": {
    "lat": 41.3111,
    "lng": 69.2797,
    "heading": 45.5,
    "speed": 35.2
  }
}
```

**Incoming Messages:**
```json
{
  "type": "location_received",
  "data": "Location updated successfully",
  "timestamp": 1705315800
}
```

#### Admin - Dashboard
```
ws://localhost:3000/ws/admin/:adminId/dashboard
```

**Incoming Messages:**
```json
{
  "type": "dashboard_stats",
  "data": {
    "active_passengers": 150,
    "active_drivers": 80,
    "timestamp": 1705315800
  },
  "timestamp": 1705315800
}

{
  "type": "ride_requested",
  "user_id": "uuid",
  "data": {...},
  "timestamp": 1705315800
}

{
  "type": "driver_location",
  "driver_id": "uuid",
  "data": {...},
  "timestamp": 1705315800
}
```

---

## Database Schema

### auth.users
```sql
CREATE TABLE auth.users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role VARCHAR(20) DEFAULT 'passenger',
    is_verified BOOLEAN DEFAULT FALSE,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### users.profiles
```sql
CREATE TABLE users.profiles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    name VARCHAR(100) NOT NULL,
    avatar_url VARCHAR(500),
    date_of_birth DATE,
    rating DECIMAL(3,2) DEFAULT 5.0,
    total_rides INTEGER DEFAULT 0,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### drivers.profiles
```sql
CREATE TABLE drivers.profiles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    license_number VARCHAR(50) UNIQUE NOT NULL,
    license_expiry DATE NOT NULL,
    is_verified BOOLEAN DEFAULT FALSE,
    is_online BOOLEAN DEFAULT FALSE,
    rating DECIMAL(3,2) DEFAULT 5.0,
    total_rides INTEGER DEFAULT 0,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### rides.requests
```sql
CREATE TABLE rides.requests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    passenger_id UUID NOT NULL REFERENCES auth.users(id),
    driver_id UUID REFERENCES drivers.profiles(id),
    pickup_latitude DECIMAL(10,8) NOT NULL,
    pickup_longitude DECIMAL(11,8) NOT NULL,
    dropoff_latitude DECIMAL(10,8) NOT NULL,
    dropoff_longitude DECIMAL(11,8) NOT NULL,
    pickup_address TEXT NOT NULL,
    dropoff_address TEXT NOT NULL,
    vehicle_type VARCHAR(20) DEFAULT 'economy',
    status VARCHAR(20) DEFAULT 'requested',
    estimated_fare DECIMAL(10,2),
    final_fare DECIMAL(10,2),
    distance_km DECIMAL(8,2),
    duration_minutes INTEGER,
    requested_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    accepted_at TIMESTAMP WITH TIME ZONE,
    started_at TIMESTAMP WITH TIME ZONE,
    completed_at TIMESTAMP WITH TIME ZONE,
    cancelled_at TIMESTAMP WITH TIME ZONE,
    cancelled_by UUID REFERENCES auth.users(id),
    cancellation_reason TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### locations.real_time
```sql
CREATE TABLE locations.real_time (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES auth.users(id),
    ride_id UUID REFERENCES rides.requests(id),
    latitude DECIMAL(10,8) NOT NULL,
    longitude DECIMAL(11,8) NOT NULL,
    heading DECIMAL(5,2),
    speed DECIMAL(5,2),
    accuracy DECIMAL(6,2),
    timestamp TIMESTAMP WITH TIME ZONE NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### payments.transactions
```sql
CREATE TABLE payments.transactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    ride_id UUID NOT NULL REFERENCES rides.requests(id),
    payer_id UUID NOT NULL REFERENCES auth.users(id),
    payment_method_id UUID REFERENCES payments.methods(id),
    amount DECIMAL(10,2) NOT NULL,
    currency VARCHAR(3) DEFAULT 'UZS',
    status VARCHAR(20) DEFAULT 'pending',
    gateway VARCHAR(20),
    gateway_transaction_id VARCHAR(255),
    initiated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    processed_at TIMESTAMP WITH TIME ZONE,
    completed_at TIMESTAMP WITH TIME ZONE
);
```

---

## Message Queue Topics

### Redis Pub/Sub Topics

#### ride:requested
```json
{
  "type": "ride_requested",
  "request_id": "uuid",
  "passenger_id": "uuid",
  "pickup_location": {
    "lat": 41.3111,
    "lng": 69.2797,
    "address": "Amir Timur Square"
  },
  "dropoff_location": {
    "lat": 41.3275,
    "lng": 69.2819,
    "address": "Tashkent City"
  },
  "vehicle_type": "economy",
  "estimated_fare": 25000,
  "timestamp": 1705315800
}
```

#### ride:accepted
```json
{
  "type": "ride_accepted",
  "request_id": "uuid",
  "driver_id": "uuid",
  "passenger_id": "uuid",
  "accepted_at": "2024-01-15T10:31:00Z"
}
```

#### ride:started
```json
{
  "type": "ride_started",
  "request_id": "uuid",
  "driver_id": "uuid",
  "started_at": "2024-01-15T10:45:00Z"
}
```

#### ride:completed
```json
{
  "type": "ride_completed",
  "request_id": "uuid",
  "final_fare": 28000,
  "distance_km": 6.2,
  "duration_minutes": 15,
  "completed_at": "2024-01-15T11:00:00Z"
}
```

#### ride:cancelled
```json
{
  "type": "ride_cancelled",
  "request_id": "uuid",
  "cancelled_by": "uuid",
  "cancellation_reason": "Driver not available",
  "cancelled_at": "2024-01-15T10:35:00Z"
}
```

#### location:driver
```json
{
  "type": "driver_location",
  "driver_id": "uuid",
  "request_id": "uuid",
  "location": {
    "lat": 41.3111,
    "lng": 69.2797,
    "heading": 45.5,
    "speed": 35.2
  },
  "timestamp": 1705315800
}
```

#### location:passenger
```json
{
  "type": "passenger_location",
  "passenger_id": "uuid",
  "request_id": "uuid",
  "location": {
    "lat": 41.3111,
    "lng": 69.2797
  },
  "timestamp": 1705315800
}
```

#### driver:online
```json
{
  "type": "driver_online",
  "driver_id": "uuid",
  "location": {
    "lat": 41.3111,
    "lng": 69.2797
  },
  "vehicle_type": "economy",
  "timestamp": 1705315800
}
```

#### driver:offline
```json
{
  "type": "driver_offline",
  "driver_id": "uuid",
  "timestamp": 1705315800
}
```

#### payment:processed
```json
{
  "type": "payment_processed",
  "transaction_id": "uuid",
  "ride_id": "uuid",
  "amount": 28000,
  "status": "completed"
}
```

#### notification:send
```json
{
  "type": "notification_send",
  "user_id": "uuid",
  "notification_type": "push",
  "title": "Ride Accepted",
  "message": "Your ride has been accepted",
  "data": {
    "ride_id": "uuid",
    "action": "open_ride"
  }
}
```

---

## Authentication Flow

### JWT Token Structure

**Access Token (expires in 24 hours):**
```json
{
  "user_id": "uuid",
  "email": "user@example.com",
  "role": "passenger",
  "iat": 1705315800,
  "exp": 1705402200
}
```

**Refresh Token (expires in 30 days):**
```json
{
  "user_id": "uuid",
  "token_type": "refresh",
  "iat": 1705315800,
  "exp": 1707907800
}
```

### Protected Endpoints

All endpoints except `/auth/register` and `/auth/login` require authentication.

**Headers:**
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Role-Based Access Control

**Passenger Role:**
- Can request rides
- Can view own profile and ride history
- Can rate drivers
- Can manage payment methods

**Driver Role:**
- Can accept/decline rides
- Can update location
- Can view earnings
- Can manage vehicle information

**Admin Role:**
- Can view all data
- Can manage users and drivers
- Can view analytics
- Can manage system settings

---

## Error Codes

### HTTP Status Codes

| Code | Description | Example |
|------|-------------|---------|
| 200 | Success | Request successful |
| 201 | Created | Resource created successfully |
| 400 | Bad Request | Invalid input data |
| 401 | Unauthorized | Missing or invalid token |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource not found |
| 409 | Conflict | Resource already exists |
| 422 | Unprocessable Entity | Validation error |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server error |
| 503 | Service Unavailable | Service temporarily down |

### Error Response Format

```json
{
  "error": true,
  "code": "VALIDATION_ERROR",
  "message": "Invalid input data",
  "details": {
    "field": "email",
    "error": "Invalid email format"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Custom Error Codes

| Code | Description |
|------|-------------|
| AUTH_001 | Invalid credentials |
| AUTH_002 | Token expired |
| AUTH_003 | Invalid token |
| USER_001 | User not found |
| USER_002 | User already exists |
| DRIVER_001 | Driver not verified |
| DRIVER_002 | Driver not available |
| RIDE_001 | No drivers available |
| RIDE_002 | Ride already accepted |
| RIDE_003 | Invalid ride status |
| PAYMENT_001 | Payment failed |
| PAYMENT_002 | Insufficient balance |
| LOCATION_001 | Invalid coordinates |

---

## Rate Limiting

### Default Limits

| Endpoint Category | Requests per Minute |
|------------------|---------------------|
| Auth endpoints | 10 |
| General API | 60 |
| Location updates | 120 |
| WebSocket connections | 5 new connections |

### Rate Limit Headers

```
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1705316400
```

---

## Pagination

### Query Parameters

```
?page=1&limit=20&sort=created_at&order=desc
```

### Response Format

```json
{
  "data": [...],
  "pagination": {
    "current_page": 1,
    "total_pages": 5,
    "total_items": 100,
    "items_per_page": 20,
    "has_next": true,
    "has_prev": false
  }
}
```

---

## Webhooks

### Webhook Events

#### ride.completed
```json
{
  "event": "ride.completed",
  "data": {
    "ride_id": "uuid",
    "passenger_id": "uuid",
    "driver_id": "uuid",
    "final_fare": 28000,
    "completed_at": "2024-01-15T11:00:00Z"
  },
  "timestamp": 1705316400
}
```

#### payment.processed
```json
{
  "event": "payment.processed",
  "data": {
    "transaction_id": "uuid",
    "ride_id": "uuid",
    "amount": 28000,
    "status": "completed"
  },
  "timestamp": 1705316400
}
```

### Webhook Configuration

**POST /api/v1/webhooks/configure**

```json
{
  "url": "https://your-server.com/webhook",
  "events": ["ride.completed", "payment.processed"],
  "secret": "webhook_secret_key"
}
```

---

## Testing Endpoints

### Health Checks

```bash
# Auth Service
curl http://localhost:8001/health

# User Service
curl http://localhost:8002/health

# Driver Service
curl http://localhost:8003/health

# Ride Service
curl http://localhost:8004/health

# Location Service
curl http://localhost:8005/health

# Payment Service
curl http://localhost:8006/health

# Notification Service
curl http://localhost:8007/health

# Real-time Service
curl http://localhost:3000/health
```

### Example API Calls

#### Register User
```bash
curl -X POST http://localhost/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "+998901234567",
    "password": "SecurePass123",
    "role": "passenger"
  }'
```

#### Login
```bash
curl -X POST http://localhost/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "SecurePass123"
  }'
```

#### Request Ride
```bash
curl -X POST http://localhost/api/v1/rides/request \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "passenger_id": "uuid",
    "pickup_latitude": 41.3111,
    "pickup_longitude": 69.2797,
    "dropoff_latitude": 41.3275,
    "dropoff_longitude": 69.2819,
    "pickup_address": "Amir Timur Square",
    "dropoff_address": "Tashkent City",
    "vehicle_type": "economy"
  }'
```

---

## Environment Variables

### Auth Service (.env)
```bash
DATABASE_URL=postgres://admin:password@localhost:5432/uber_clone
REDIS_URL=redis://localhost:6379
JWT_SECRET=your-super-secret-key
JWT_EXPIRES_IN=24h
REFRESH_TOKEN_EXPIRES_IN=720h
PORT=8001
```

### Payment Service (.env)
```bash
DATABASE_URL=postgres://admin:password@localhost:5432/uber_clone
STRIPE_SECRET_KEY=sk_test_your_stripe_key
STRIPE_WEBHOOK_SECRET=whsec_your_webhook_secret
PORT=8006
```

### Notification Service (.env)
```bash
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_PRIVATE_KEY=your-private-key
TWILIO_ACCOUNT_SID=your-twilio-sid
TWILIO_AUTH_TOKEN=your-twilio-token
TWILIO_PHONE_NUMBER=+1234567890
SENDGRID_API_KEY=your-sendgrid-key
PORT=8007
```

### Real-time Service (.env)
```bash
REDIS_URL=redis://localhost:6379
PORT=3000
```

---

## API Versioning

Current Version: **v1**

Base URL: `http://localhost/api/v1`

Future versions will use:
- `http://localhost/api/v2`
- `http://localhost/api/v3`

### Version Header (Optional)
```
X-API-Version: v1
```

---

## CORS Configuration

### Allowed Origins (Development)
```
http://localhost:3000
http://localhost:8080
http://127.0.0.1:3000
file://
```

### Allowed Methods
```
GET, POST, PUT, DELETE, OPTIONS
```

### Allowed Headers
```
Origin, Content-Type, Accept, Authorization, X-API-Version
```

---

## Performance Optimization

### Caching Strategy

**Redis Cache Keys:**
- `user:{user_id}` - User profile (TTL: 1 hour)
- `driver:{driver_id}` - Driver details (TTL: 30 minutes)
- `ride:{ride_id}` - Ride details (TTL: 5 minutes)
- `location:driver:{driver_id}` - Driver location (TTL: 30 seconds)
- `nearby:drivers:{lat}:{lng}` - Nearby drivers (TTL: 10 seconds)



## Security Best Practices

1. **Always use HTTPS in production**
2. **Implement rate limiting**
3. **Validate all user inputs**
4. **Use parameterized queries (prevent SQL injection)**
5. **Encrypt sensitive data at rest**
6. **Implement CSRF protection**
7. **Use secure password hashing (bcrypt)**
8. **Implement proper CORS policies**
9. **Regular security audits**
10. **Keep dependencies updated**

---


**Last Updated:** January 15, 2024  
**API Version:** v1.0.0  
**Maintained by:** Uber Clone Development Team
