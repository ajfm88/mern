# Backend API - Project Structure & Overview

A RESTful API built with Express.js for managing places and users with Google Maps integration.

## 📁 Project Structure

```
backend/
├── controllers/
│   ├── places-controllers.js    # CRUD operations for places
│   └── users-controllers.js     # User authentication & management
├── models/
│   └── http-error.js            # Custom error class
├── routes/
│   ├── places-routes.js         # Place endpoints with validation
│   └── users-routes.js          # User endpoints with validation
├── util/
│   └── location.js              # Google Maps Geocoding helper
├── .env                         # Environment variables (not committed)
├── .gitignore                   # Git ignore file
├── app.js                       # Main application entry point
├── package.json                 # Dependencies & scripts
└── package-lock.json            # Locked dependencies
```

## 🚀 Quick Start

```bash
# Install dependencies
npm install

# Create .env file with your API key
echo "GOOGLE_MAPS_API_KEY=your_key_here" > .env

# Start development server
npm start
```

Server runs on: `http://localhost:5000`

## 🔑 Core Files Explained

### **app.js** - Application Entry Point

Sets up Express server, middleware, routes, and error handling.

**Middleware Order (Critical):**

1. `bodyParser.json()` - Parse JSON request bodies
2. Route handlers (`/api/places`, `/api/users`)
3. 404 catch-all handler
4. Global error handler

**Key Pattern:**

```javascript
Request → Body Parser → Routes → Validation → Controller → Response
                                                    ↓
                                              Error Handler (if error)
```

### **Controllers** - Business Logic

**places-controllers.js:**

- `getPlaceById` - GET single place
- `getPlacesByUserId` - GET all places by user
- `createPlace` - POST new place (calls Google Maps API)
- `updatePlace` - PATCH existing place
- `deletePlace` - DELETE place

**users-controllers.js:**

- `getUsers` - GET all users
- `signup` - POST register new user
- `login` - POST authenticate user

**Current Storage:** In-memory arrays (`DUMMY_PLACES`, `DUMMY_USERS`)

### **Routes** - API Endpoints

**Places Routes (`/api/places`):**

- `GET /:pid` - Get place by ID
- `GET /user/:uid` - Get places by user ID
- `POST /` - Create place (validates: title, description, address)
- `PATCH /:pid` - Update place (validates: title, description)
- `DELETE /:pid` - Delete place

**User Routes (`/api/users`):**

- `GET /` - Get all users
- `POST /signup` - Register (validates: name, email, password)
- `POST /login` - Login

### **Models** - Data Structures

**http-error.js:**
Custom error class that extends `Error` with HTTP status codes.

```javascript
throw new HttpError("Error message", 404);
```

### **Utils** - Helper Functions

**location.js:**
Converts addresses to coordinates using Google Maps Geocoding API.

```javascript
const coords = await getCoordsForAddress("20 W 34th St, New York");
// Returns: { lat: 40.7484474, lng: -73.9871516 }
```

## 🛠️ Technologies & Dependencies

- **express** - Web framework
- **body-parser** - Parse JSON request bodies
- **express-validator** - Input validation middleware
- **axios** - HTTP client for API calls
- **uuid** - Generate unique IDs
- **dotenv** - Environment variable management
- **nodemon** (dev) - Auto-restart on file changes

## 📊 Request Flow Example

**Creating a new place:**

```
1. POST /api/places
   ↓
2. bodyParser parses JSON body
   ↓
3. Routes to placesRoutes
   ↓
4. Validation middleware checks input
   ↓
5. createPlace controller executes
   ↓
6. getCoordsForAddress() calls Google Maps API
   ↓
7. New place created with coordinates
   ↓
8. 201 response with place data
```

## 🔒 Security & Best Practices

- ✅ API keys in environment variables (never committed)
- ✅ Input validation on all POST/PATCH routes
- ✅ Centralized error handling
- ✅ Custom error classes with HTTP status codes
- ⚠️ Passwords stored in plain text (use bcrypt in production)
- ⚠️ No authentication tokens (add JWT in production)

## 🎯 Key Design Patterns

1. **MVC Architecture** - Models, Controllers, Routes separation
2. **RESTful API** - Standard HTTP methods and status codes
3. **Middleware Chain** - Validation → Controller → Response
4. **Error Handling** - Custom errors caught by global handler
5. **Separation of Concerns** - Each file has single responsibility

## 📝 Common Status Codes Used

- `200` - Success (GET, PATCH, DELETE)
- `201` - Created (POST)
- `401` - Unauthorized (login failed)
- `404` - Not Found (resource doesn't exist)
- `422` - Unprocessable Entity (validation failed, invalid address)
- `500` - Server Error (catch-all for unexpected errors)

## 🚧 Production Readiness Checklist

- [ ] Replace in-memory storage with database (MongoDB)
- [ ] Hash passwords with bcrypt
- [ ] Implement JWT authentication
- [ ] Add CORS configuration
- [ ] Rate limiting
- [ ] Input sanitization
- [ ] Comprehensive error logging
- [ ] Environment-specific configs
- [ ] API documentation (Swagger/OpenAPI)
- [ ] Unit and integration tests

## 💡 Architecture and Structure

**Architecture:**

- Modular, scalable structure with separation of concerns
- RESTful design with proper HTTP methods
- Middleware pipeline for request processing

**Error Handling:**

- Custom HttpError class for consistency
- Centralized error handler catches all errors
- Appropriate status codes for different scenarios

**External Integration:**

- Google Maps API for geocoding addresses
- Async/await for handling promises
- Error handling for API failures

**Security:**

- Environment variables for sensitive data
- Input validation at route level
- Ready for authentication implementation

---

**Need to refresh on a specific part?** Jump to the relevant file in the structure above!
