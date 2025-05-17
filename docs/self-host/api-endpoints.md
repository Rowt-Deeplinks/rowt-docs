# API Endpoints
# Authentication Endpoints

## POST `/auth/login`
- **Description**: Authenticates a user and returns JWT tokens
- **Auth**: Public
- **Rate Limit**: 5 requests per minute

**Request Body:**
```json
{
  "email": "admin@example.com",     // Required: User email
  "password": "your-password"       // Required: User password
}
```

**Success Response (200):**
```json
{
  "tokens": {
    "access_token": "jwt-token",      // JWT for API access
    "refresh_token": "refresh-token"  // JWT for token refresh
  },
  "user": {
    "id": 1,                          // User ID
    "email": "admin@example.com",     // User email
    "role": "admin",                  // User role (admin, user)
    "emailVerified": true,            // Email verification status
    "customerId": "stripe-id"         // Optional: Stripe customer ID (multi-tenant mode)
  }
}
```

**Error Response (400):**
```json
{
  "statusCode": 400,
  "message": "Invalid email or password"
}
```

## POST `/auth/refresh`
- **Description**: Refreshes an expired access token
- **Auth**: Public
- **Rate Limit**: 30 requests per minute

**Request Body:**
```json
{
  "refresh_token": "your-refresh-token"  // Required: Valid refresh token
}
```

**Success Response (200):**
```json
{
  "access_token": "new-jwt-token",      // New JWT for API access
  "refresh_token": "new-refresh-token"  // New refresh token
}
```

**Error Response (401):**
```json
{
  "statusCode": 401,
  "message": "Invalid refresh token"
}
```

## POST `/auth/logout`
- **Description**: Invalidates tokens
- **Auth**: Public

**Request Body:**
```json
{
  "refresh_token": "your-refresh-token",  // Required: Refresh token to invalidate
  "access_token": "your-access-token"     // Required: Access token to blacklist
}
```

**Success Response (200):**
```json
{
  "message": "Logout successful"
}
```

**Error Response (400):**
```json
{
  "statusCode": 400,
  "message": "Refresh token and access token are required"
}
```

## POST `/auth/validate`
- **Description**: Validates user credentials without creating tokens
- **Auth**: Public

**Request Body:**
```json
{
  "email": "admin@example.com",     // Required: User email
  "password": "your-password"       // Required: User password
}
```

**Success Response (200):**
```json
{
  "message": "Valid User",
  "isValid": true
}
```

**Error Response (200 with failure):**
```json
{
  "message": "Password did not match",
  "isValid": false
}
```

## POST `/auth/updatepassword`
- **Description**: Updates a user's password
- **Auth**: JWT

**Request Body:**
```json
{
  "email": "user@example.com",      // Required: User email
  "password": "new-secure-password" // Required: New password
}
```

**Success Response (200):**
Returns the updated user object:
```json
{
  "id": 1,
  "email": "user@example.com",
  "role": "user",
  "emailVerified": true
}
```

**Error Response (400):**
```json
{
  "statusCode": 400,
  "message": "Unable to update password: [error details]"
}
```

## GET `/auth/profile`
- **Description**: Returns the current user's profile
- **Auth**: JWT

**Success Response (200):**
```json
{
  "email": "admin@example.com",
  "userId": 1,
  "role": "admin"
}
```

**Error Response (401):**
```json
{
  "statusCode": 401,
  "message": "Unauthorized"
}
```

# Project Endpoints

## POST `/projects/create`
- **Description**: Creates a new project
- **Auth**: JWT

**Request Body:**
```json
{
  "userId": "1",                        // Required: User ID
  "name": "My App",                     // Required: Project name
  "baseUrl": "https://myapp.com",       // Required: Base URL for the project
  "fallbackUrl": "https://myapp.com/download",  // Required: Fallback URL when app not installed
  "appstoreId": "123456789",            // Optional: App Store ID for iOS 
  "playstoreId": "com.example.myapp",   // Optional: Play Store ID for Android
  "iosScheme": "myapp://",              // Optional: iOS URL scheme
  "androidScheme": "myapp://"           // Optional: Android URL scheme
}
```

**Success Response (200):**
Returns the created project object:
```json
{
  "id": "project-uuid",
  "apiKey": "auto-generated-api-key",
  "name": "My App",
  "baseUrl": "https://myapp.com",
  "fallbackUrl": "https://myapp.com/download",
  "appstoreId": "123456789",
  "playstoreId": "com.example.myapp",
  "iosScheme": "myapp://",
  "androidScheme": "myapp://",
  "userId": "1"
}
```

**Error Response (500):**
```json
{
  "message": "An error occurred while creating the project."
}
```

## POST `/projects/update`
- **Description**: Updates an existing project
- **Auth**: JWT

**Request Body:**
```json
{
  "id": "project-uuid",                 // Required: Project UUID
  "apiKey": "api-key",                  // Required: Project API key
  "userId": "1",                        // Required: User ID
  "name": "Updated App Name",           // Required: Project name
  "baseUrl": "https://myapp.com",       // Required: Base URL
  "fallbackUrl": "https://myapp.com/download",  // Required: Fallback URL
  "appstoreId": "123456789",            // Optional: App Store ID
  "playstoreId": "com.example.myapp",   // Optional: Play Store ID
  "iosScheme": "myapp://",              // Optional: iOS URL scheme
  "androidScheme": "myapp://"           // Optional: Android URL scheme
}
```

**Success Response (200):**
Returns the updated project object:
```json
{
  "id": "project-uuid",
  "apiKey": "api-key",
  "name": "Updated App Name",
  "baseUrl": "https://myapp.com",
  "fallbackUrl": "https://myapp.com/download",
  "appstoreId": "123456789",
  "playstoreId": "com.example.myapp",
  "iosScheme": "myapp://",
  "androidScheme": "myapp://",
  "userId": "1"
}
```

**Error Response (404):**
```json
{
  "message": "Project not found"
}
```

**Error Response (500):**
```json
{
  "message": "An error occurred while updating the project."
}
```

## POST `/projects/getUserProjects`
- **Description**: Returns all projects for the current user
- **Auth**: JWT

**Success Response (200):**
Returns an array of project objects:
```json
[
  {
    "id": "project-uuid-1",
    "apiKey": "api-key-1",
    "name": "App 1",
    "baseUrl": "https://app1.com",
    "fallbackUrl": "https://app1.com/download",
    "appstoreId": "123456789",
    "playstoreId": "com.example.app1",
    "iosScheme": "app1://",
    "androidScheme": "app1://",
    "userId": "1"
  },
  {
    "id": "project-uuid-2",
    "apiKey": "api-key-2",
    "name": "App 2",
    "baseUrl": "https://app2.com",
    "fallbackUrl": "https://app2.com/download",
    "appstoreId": "987654321",
    "playstoreId": "com.example.app2",
    "iosScheme": "app2://",
    "androidScheme": "app2://",
    "userId": "1"
  }
]
```

**Error Response (400):**
```json
{
  "message": "Missing userId"
}
```

**Error Response (500):**
```json
{
  "message": "An error occurred while fetching user projects."
}
```

## POST `/projects/getById`
- **Description**: Returns a specific project
- **Auth**: JWT

**Request Body:**
```json
{
  "id": "project-uuid",       // Required: Project UUID
  "options": {                // Optional: Query options
    "includeLinks": true,     // Optional: Include links in response (default: false)
    "includeInteractions": true,  // Optional: Include interactions in response (default: false)
    "startDate": "2023-01-01",    // Optional: Start date for interactions filter
    "endDate": "2023-12-31",      // Optional: End date for interactions filter
    "getPreviousPeriod": true     // Optional: Include previous period stats (default: false)
  }
}
```

**Success Response (200):**
Returns the project object with optional links and interactions:
```json
{
  "id": "project-uuid",
  "apiKey": "api-key",
  "name": "My App",
  "baseUrl": "https://myapp.com",
  "fallbackUrl": "https://myapp.com/download",
  "appstoreId": "123456789",
  "playstoreId": "com.example.myapp",
  "iosScheme": "myapp://",
  "androidScheme": "myapp://",
  "userId": "1",
  "links": [
    {
      "id": "abcd1234",
      "url": "myapp://page/123",
      "title": "Check out this page",
      "description": "Great content inside",
      "imageUrl": "https://example.com/image.jpg",
      "fallbackUrlOverride": "https://example.com/fallback",
      "additionalMetadata": { "og:type": "article" },
      "properties": { "source": "email" },
      "lifetimeClicks": 42,
      "createdAt": "2023-06-01T12:00:00.000Z",
      "interactions": [
        {
          "id": "interaction-uuid",
          "referer": "https://twitter.com",
          "country": "United States",
          "device": "iphone",
          "os": "ios",
          "browser": "safari",
          "timestamp": "2023-06-02T15:30:00.000Z"
        }
      ]
    }
  ],
  "previousPeriodInteractionCount": 28
}
```

**Error Response (400):**
```json
{
  "message": "Missing projectId"
}
```

**Error Response (500):**
```json
{
  "message": "An error occurred while fetching the project."
}
```

## POST `/projects/generateApiKey`
- **Description**: Generates a new API key for a project
- **Auth**: JWT

**Request Body:**
```json
{
  "projectId": "project-uuid"  // Required: Project UUID
}
```

**Success Response (200):**
```json
{
  "apiKey": "new-api-key"
}
```

**Error Response (401):**
```json
{
  "message": "Unauthorized"
}
```

**Error Response (404):**
```json
{
  "message": "Project not found or access denied"
}
```

# Link Endpoints

## POST `/link`
- **Description**: Creates a new deeplink
- **Auth**: Public (with API key)

**Request Body:**
```json
{
  "projectId": "project-uuid",         // Required: Project UUID
  "apiKey": "project-api-key",         // Required: Project API key
  "url": "myapp://page/123",           // Required: Deep link URL
  "title": "Check out this page",      // Optional: Link title for metadata
  "description": "Great content inside", // Optional: Link description for metadata
  "imageUrl": "https://example.com/image.jpg", // Optional: Image URL for metadata
  "fallbackUrlOverride": "https://example.com/fallback", // Optional: Override project fallback
  "additionalMetadata": {             // Optional: Additional metadata for link previews
    "og:type": "article",
    "twitter:card": "summary_large_image",
    "og:site_name": "My App"
  },
  "properties": {                     // Optional: Custom properties for analytics
    "source": "email", 
    "campaign": "summer2023",
    "utm_medium": "social"
  },
  "expiration": "2024-12-31T23:59:59.999Z"  // Optional: Link expiration date
}
```

**Success Response (200):**
Returns the short URL path (e.g., `abcd1234`) which can be appended to your Rowt server base URL:
```
abcd1234
```

**Error Response (400):**
```json
{
  "statusCode": 400,
  "message": "Invalid request - missing body"
}
```

**Error Response (401):**
```
Unauthorized
```

**Error Response (403):**
```
You have reached the maximum of X links allowed for your plan. Please upgrade to create more links.
```

**Error Response (500):**
```
Failed to create link
```

## POST `/link/byProjectId`
- **Description**: Returns all links for a specific project
- **Auth**: JWT

**Request Body:**
```json
{
  "projectId": "project-uuid",         // Required: Project UUID
  "includeInteractions": true          // Optional: Include interaction data (default: false)
}
```

**Success Response (200):**
Returns an array of link objects:
```json
[
  {
    "id": "abcd1234",
    "url": "myapp://page/123",
    "title": "Check out this page",
    "description": "Great content inside",
    "imageUrl": "https://example.com/image.jpg",
    "fallbackUrlOverride": "https://example.com/fallback",
    "additionalMetadata": { 
      "og:type": "article",
      "twitter:card": "summary_large_image"
    },
    "properties": { 
      "source": "email",
      "campaign": "summer2023"
    },
    "lifetimeClicks": 42,
    "createdAt": "2023-06-01T12:00:00.000Z",
    "interactions": [
      {
        "id": "interaction-uuid",
        "referer": "https://twitter.com",
        "country": "United States",
        "device": "iphone",
        "os": "ios",
        "browser": "safari",
        "timestamp": "2023-06-02T15:30:00.000Z"
      }
    ]
  },
  // More links...
]
```

**Error Response (400):**
```
Missing projectId
```

**Error Response (404):**
```
No links found
```

## GET `/:shortCode`
- **Description**: Handles the redirection and analytics tracking for a short link
- **Auth**: Public
- **Rate Limit**: 10 requests per minute per shortCode

**Parameters:**
- `shortCode`: The short code of the link (e.g., `abcd1234`)

**Success Response (200):**
Returns an HTML page with metadata and JavaScript that:
1. Sets appropriate metadata for link previews
2. Attempts to open the app with the deeplink
3. Falls back to the appropriate store or fallback URL if the app is not installed
4. Logs the interaction asynchronously

**Error Response (307):**
Redirects to error page when link not found or invalid

# User Endpoints (Multi-tenant Mode)

## POST `/users/create`
- **Description**: Creates a new user (multi-tenant mode only)
- **Auth**: Public

**Request Body:**
```json
{
  "email": "user@example.com",      // Required: User email
  "password": "secure-password",    // Required: User password
  "role": "user"                    // Required: User role (user, admin)
}
```

**Success Response (201):**
Returns the created user object:
```json
{
  "id": "user-uuid",
  "email": "user@example.com",
  "role": "user",
  "emailVerified": false,
  "customerId": "stripe-customer-id"  // Only if Stripe integration is enabled
}
```

**Error Response (400):**
```json
{
  "statusCode": 400,
  "message": "Invalid email format"
}
```

**Error Response (409):**
```json
{
  "statusCode": 409,
  "message": "A user with this email already exists"
}
```

## POST `/users/byEmail`
- **Description**: Gets a user by email
- **Auth**: JWT

**Request Body:**
```json
{
  "email": "user@example.com"  // Required: User email
}
```

**Success Response (200):**
Returns the user object:
```json
{
  "id": "user-uuid",
  "email": "user@example.com",
  "role": "user",
  "emailVerified": true,
  "customerId": "stripe-customer-id"  // Only if Stripe integration is enabled
}
```

**Error Response:**
```
User not found
```

## POST `/users/byId`
- **Description**: Gets a user by ID (UUID)
- **Auth**: JWT

**Request Body:**
```json
{
  "id": 00000-00000-00000-00000  // Required: User ID
}
```

**Success Response (200):**
Returns the user object:
```json
{
  "id": 00000-00000-00000-00000,
  "email": "user@example.com",
  "role": "user",
  "emailVerified": true,
  "customerId": "stripe-customer-id"  // Only if Stripe integration is enabled
}
```

**Error Response:**
```
User not found
```

## GET `/users/currentUser`
- **Description**: Gets the current authenticated user
- **Auth**: JWT

**Success Response (200):**
Returns the user object:
```json
{
  "id": 1,
  "email": "user@example.com",
  "role": "user",
  "emailVerified": true,
  "customerId": "stripe-customer-id"  // Only if Stripe integration is enabled
}
```

**Error Response (404):**
```
User not found
```

## POST `/users/delete`
- **Description**: Deletes a user
- **Auth**: JWT

**Request Body:**
```json
{
  "email": "user@example.com"  // Required: Email of user to delete
}
```

**Success Response (200):**
```
User with email user@example.com deleted
```

**Error Response (400):**
```
User not authorized to delete this user
```

## POST `/users/usage`
- **Description**: Gets usage statistics for a user
- **Auth**: JWT

**Request Body:**
```json
{
  "userId": 1  // Required: User ID
}
```

**Success Response (200):**
```json
{
  "links": 42,                 // Number of links created in the period
  "interactions": 1024,        // Number of interactions in the period
  "period": {
    "start": "2023-05-17T00:00:00.000Z",  // Start of the period
    "end": "2023-06-17T00:00:00.000Z"     // End of the period
  }
}
```

**Error Response (400):**
```json
{
  "statusCode": 400,
  "message": "Unable to get user usage: User not found"
}
```

## POST `/users/tier`
- **Description**: Gets the subscription tier for a user (multi-tenant only)
- **Auth**: JWT

**Request Body:**
```json
{
  "userId": 1  // Required: User ID
}
```

**Success Response (200):**
```json
{
  "tier": 2,                   // User's subscription tier
  "allowances": {
    "links": 5000,             // Maximum links allowed for tier
    "interactions": 175000     // Maximum interactions for tier
  }
}
```

**Error Response:**
Returns the default tier (0) if user is not found or has no subscription:
```json
{
  "tier": 0,
  "allowances": {
    "links": 50,
    "interactions": 500
  }
}
```