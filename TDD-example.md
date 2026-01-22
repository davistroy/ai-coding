# TacoTracker 3000 - Technical Design Document

## Enterprise Taco Truck Tracking and Ordering System for Globex Corporation

| Document Information |                                         |
|---------------------|-----------------------------------------|
| Version             | 1.0                                     |
| Status              | Approved                                |
| Author              | Senior Software Architect               |
| Last Updated        | 2026-01-22                              |
| Approved By         | CTO, Principal Architect - 2026-01-20   |

## Document History

| Version | Date       | Author                  | Changes                              |
|---------|------------|-------------------------|--------------------------------------|
| 0.1     | 2026-01-05 | Software Architect      | Initial draft                        |
| 0.5     | 2026-01-10 | Software Architect      | API specifications complete          |
| 0.8     | 2026-01-15 | Senior Architect        | Database schema finalized            |
| 0.9     | 2026-01-18 | Senior Architect        | Security review incorporated         |
| 1.0     | 2026-01-22 | Senior Architect        | Final approval version               |

## Related Documents

| Document | Link | Relevance |
|----------|------|-----------|
| Business Requirements Document | [BRD-example.md](./BRD-example.md) | Business context and objectives |
| Product Requirements Document | [PRD-example.md](./PRD-example.md) | Feature specifications and user stories |
| UI/UX Design Document | [UI-UX-example.md](./UI-UX-example.md) | Design system and interface specifications |
| Architecture Document | [ARCH-example.md](./ARCH-example.md) | High-level architecture and system context |

---

## 1. Technical Overview

### 1.1 Purpose

This Technical Design Document (TDD) provides implementation-ready specifications for the TacoTracker 3000 platform. It translates the high-level architecture decisions from the ARCH document and feature requirements from the PRD into detailed technical specifications including:

- Complete API contracts with request/response schemas
- Database schema design with indexing strategies
- Service layer implementation patterns
- Third-party integration specifications
- Error handling and logging standards
- Testing and deployment strategies

### 1.2 Scope

**In Scope:**
- Backend API services (Order, User, Vendor, Location, Payment, Notification)
- Database schema for PostgreSQL and DynamoDB
- Mobile app data layer and state management
- Vendor web portal backend integration
- Real-time WebSocket communication
- Third-party integration implementations (Stripe, Okta, Firebase)

**Out of Scope:**
- Frontend UI implementation details (covered in UI/UX document)
- Infrastructure provisioning (covered in ARCH document)
- Business logic rationale (covered in PRD document)
- Operational runbooks (separate operations documentation)

### 1.3 Technical Approach Summary

| Aspect | Approach | Rationale |
|--------|----------|-----------|
| API Design | RESTful with OpenAPI 3.0 | Industry standard, tooling support |
| Data Access | Repository pattern with Prisma ORM | Type safety, migration management |
| Real-time | Socket.io with Redis adapter | Horizontal scaling, fallback transport |
| Authentication | JWT with Okta OIDC | Corporate SSO integration |
| Error Handling | Centralized middleware with typed errors | Consistency, observability |
| Testing | Jest + Supertest for API, Detox for E2E | Coverage, integration testing |

---

## 2. System Dependencies and Prerequisites

### 2.1 Infrastructure Dependencies

| Dependency | Version | Purpose | Required/Optional |
|------------|---------|---------|-------------------|
| Node.js | 20.x LTS | Backend runtime | Required |
| PostgreSQL | 15.x | Primary data store | Required |
| Redis | 7.x | Caching, pub/sub, sessions | Required |
| DynamoDB | - | Location time-series data | Required |
| Docker | 24.x | Container runtime | Required |
| AWS ECS Fargate | - | Container orchestration | Required |

### 2.2 External Service Dependencies

| Service | Purpose | SLA | Fallback Strategy |
|---------|---------|-----|-------------------|
| Okta | SSO authentication | 99.99% | Cache JWT public keys, extend session validity |
| Stripe | Payment processing | 99.99% | Queue orders, process when available |
| Firebase FCM | Push notifications | 99.95% | Queue notifications, retry with backoff |
| Google Maps Platform | Geocoding, directions | 99.9% | Cache common routes, offline maps |
| AWS SES | Email notifications | 99.9% | Queue emails, retry |

### 2.3 Internal Service Dependencies

| Service | APIs Used | Data Exchanged |
|---------|-----------|----------------|
| API Gateway | All downstream services | Request routing, auth tokens |
| Location Service | User, Vendor services | Truck positions, geofence events |
| Order Service | Payment, Notification, Vendor services | Order lifecycle events |
| Payment Service | Order service | Payment status, refunds |
| Notification Service | All services | Push, email, in-app alerts |

---

## 3. API Specifications

### 3.1 API Overview

```
Base URL: https://api.tacotracker.globex.com/v1
Authentication: Bearer token (JWT from Okta)
Content-Type: application/json
API Version: v1
Rate Limits:
  - Authenticated users: 100 requests/minute
  - Vendors: 200 requests/minute
  - Admin: 500 requests/minute
```

### 3.2 Authentication Endpoints

#### POST /auth/login

**Description:** Initiate Okta OIDC login flow

**Authentication:** None

**Request:**
```json
{
  "redirect_uri": "tacotracker://auth/callback"
}
```

**Response (200 OK):**
```json
{
  "authorization_url": "https://globex.okta.com/oauth2/v1/authorize?...",
  "state": "abc123",
  "code_verifier": "xyz789"
}
```

---

#### POST /auth/callback

**Description:** Exchange authorization code for tokens

**Authentication:** None

**Request:**
```json
{
  "code": "authorization_code_from_okta",
  "state": "abc123",
  "code_verifier": "xyz789"
}
```

**Response (200 OK):**
```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "v1.MjAyNi0wMS0yMlQxMjowMDowMFo...",
  "expires_in": 3600,
  "token_type": "Bearer",
  "user": {
    "id": "usr_abc123",
    "email": "john.doe@globex.com",
    "firstName": "John",
    "lastName": "Doe",
    "role": "employee",
    "campusId": "campus_sf_hq"
  }
}
```

---

#### POST /auth/refresh

**Description:** Refresh access token

**Authentication:** Refresh token in body

**Request:**
```json
{
  "refresh_token": "v1.MjAyNi0wMS0yMlQxMjowMDowMFo..."
}
```

**Response (200 OK):**
```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600
}
```

---

### 3.3 User Endpoints

#### GET /users/me

**Description:** Get current user profile

**Authentication:** Required (Bearer token)

**Authorization:** Any authenticated user

**Response (200 OK):**
```json
{
  "id": "usr_abc123",
  "email": "john.doe@globex.com",
  "firstName": "John",
  "lastName": "Doe",
  "department": "Engineering",
  "campusId": "campus_sf_hq",
  "campusName": "San Francisco HQ",
  "role": "employee",
  "mealSubsidy": {
    "balance": 150.00,
    "monthlyAllowance": 200.00,
    "resetDate": "2026-02-01"
  },
  "preferences": {
    "dietary": ["vegetarian"],
    "allergens": ["nuts", "shellfish"],
    "notifications": {
      "arrivalAlerts": true,
      "orderUpdates": true,
      "dailyDigest": false,
      "promotional": false
    }
  },
  "favoriteVendors": ["vnd_taco_tornado", "vnd_la_taqueria"],
  "createdAt": "2024-03-15T10:00:00Z",
  "updatedAt": "2026-01-20T14:30:00Z"
}
```

---

#### PATCH /users/me

**Description:** Update current user profile

**Authentication:** Required

**Request:**
```json
{
  "preferences": {
    "dietary": ["vegetarian", "gluten-free"],
    "allergens": ["nuts"],
    "notifications": {
      "arrivalAlerts": true,
      "orderUpdates": true,
      "dailyDigest": true,
      "promotional": false
    }
  }
}
```

**Response (200 OK):**
```json
{
  "id": "usr_abc123",
  "preferences": {
    "dietary": ["vegetarian", "gluten-free"],
    "allergens": ["nuts"],
    "notifications": {
      "arrivalAlerts": true,
      "orderUpdates": true,
      "dailyDigest": true,
      "promotional": false
    }
  },
  "updatedAt": "2026-01-22T10:15:00Z"
}
```

---

#### GET /users/me/favorites

**Description:** Get user's favorite vendors

**Authentication:** Required

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": "vnd_taco_tornado",
      "name": "Taco Tornado",
      "cuisine": "Mexican",
      "rating": 4.7,
      "logoUrl": "https://cdn.tacotracker.globex.com/vendors/taco_tornado/logo.png",
      "addedAt": "2025-06-15T12:00:00Z"
    }
  ],
  "meta": {
    "total": 1
  }
}
```

---

#### POST /users/me/favorites/{vendorId}

**Description:** Add vendor to favorites

**Authentication:** Required

**Path Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| vendorId | string | Yes | Vendor identifier |

**Response (201 Created):**
```json
{
  "vendorId": "vnd_la_taqueria",
  "addedAt": "2026-01-22T10:20:00Z"
}
```

---

#### DELETE /users/me/favorites/{vendorId}

**Description:** Remove vendor from favorites

**Authentication:** Required

**Response (204 No Content)**

---

### 3.4 Vendor Endpoints

#### GET /vendors

**Description:** List vendors with filtering

**Authentication:** Required

**Query Parameters:**

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| campusId | string | No | User's campus | Filter by campus |
| cuisine | string | No | - | Filter by cuisine type |
| dietary | string[] | No | - | Filter by dietary options |
| status | string | No | "active" | Filter by status (active, arriving, closed) |
| page | integer | No | 1 | Page number |
| limit | integer | No | 20 | Results per page (max 50) |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": "vnd_taco_tornado",
      "name": "Taco Tornado",
      "description": "Authentic Mexican street tacos with a modern twist",
      "cuisine": "Mexican",
      "logoUrl": "https://cdn.tacotracker.globex.com/vendors/taco_tornado/logo.png",
      "bannerUrl": "https://cdn.tacotracker.globex.com/vendors/taco_tornado/banner.jpg",
      "rating": 4.7,
      "reviewCount": 342,
      "priceRange": "$$",
      "dietaryOptions": ["vegetarian", "gluten-free"],
      "status": "active",
      "currentLocation": {
        "campusId": "campus_sf_hq",
        "campusName": "San Francisco HQ",
        "lotId": "lot_b",
        "lotName": "Lot B - North",
        "latitude": 37.7749,
        "longitude": -122.4194,
        "updatedAt": "2026-01-22T11:45:00Z"
      },
      "waitTime": {
        "estimate": 12,
        "unit": "minutes",
        "queueLength": 8
      },
      "operatingHours": {
        "today": {
          "open": "11:00",
          "close": "14:00",
          "isOpen": true
        }
      }
    }
  ],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 8,
    "totalPages": 1
  }
}
```

---

#### GET /vendors/{vendorId}

**Description:** Get vendor details

**Authentication:** Required

**Response (200 OK):**
```json
{
  "id": "vnd_taco_tornado",
  "name": "Taco Tornado",
  "description": "Authentic Mexican street tacos with a modern twist. Family-owned since 2015.",
  "cuisine": "Mexican",
  "logoUrl": "https://cdn.tacotracker.globex.com/vendors/taco_tornado/logo.png",
  "bannerUrl": "https://cdn.tacotracker.globex.com/vendors/taco_tornado/banner.jpg",
  "rating": 4.7,
  "reviewCount": 342,
  "priceRange": "$$",
  "dietaryOptions": ["vegetarian", "gluten-free"],
  "allergenInfo": "Kitchen handles nuts, dairy, and gluten. Cross-contamination possible.",
  "phone": "+1-555-TACO-123",
  "email": "hello@tacotornado.com",
  "socialMedia": {
    "instagram": "@tacotornado",
    "twitter": "@tacotornado"
  },
  "paymentMethods": ["credit", "debit", "apple_pay", "google_pay"],
  "schedule": [
    {
      "dayOfWeek": "tuesday",
      "campusId": "campus_sf_hq",
      "lotId": "lot_b",
      "arrivalTime": "11:00",
      "departureTime": "14:00"
    },
    {
      "dayOfWeek": "thursday",
      "campusId": "campus_sf_hq",
      "lotId": "lot_b",
      "arrivalTime": "11:00",
      "departureTime": "14:00"
    }
  ],
  "currentLocation": {
    "campusId": "campus_sf_hq",
    "lotId": "lot_b",
    "latitude": 37.7749,
    "longitude": -122.4194,
    "status": "active",
    "updatedAt": "2026-01-22T11:45:00Z"
  },
  "trucks": [
    {
      "id": "trk_tornado_1",
      "name": "Tornado One",
      "licensePlate": "TACO 1",
      "capacity": 4,
      "status": "active"
    }
  ],
  "stats": {
    "totalOrders": 15420,
    "avgPrepTime": 8,
    "avgRating": 4.7
  }
}
```

---

#### GET /vendors/{vendorId}/menu

**Description:** Get vendor's menu

**Authentication:** Required

**Query Parameters:**

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| category | string | No | - | Filter by category |
| dietary | string[] | No | User's preferences | Filter by dietary options |

**Response (200 OK):**
```json
{
  "vendorId": "vnd_taco_tornado",
  "vendorName": "Taco Tornado",
  "lastUpdated": "2026-01-20T09:00:00Z",
  "categories": [
    {
      "id": "cat_tacos",
      "name": "Tacos",
      "description": "Our signature street tacos",
      "displayOrder": 1,
      "items": [
        {
          "id": "item_carnitas_taco",
          "name": "Carnitas Taco",
          "description": "Slow-roasted pulled pork with cilantro, onions, and salsa verde",
          "price": 4.50,
          "imageUrl": "https://cdn.tacotracker.globex.com/menu/carnitas_taco.jpg",
          "calories": 320,
          "prepTime": 5,
          "available": true,
          "popular": true,
          "dietary": [],
          "allergens": ["gluten"],
          "customizations": [
            {
              "id": "cust_protein",
              "name": "Extra Protein",
              "type": "single",
              "required": false,
              "options": [
                {"id": "opt_extra_meat", "name": "Extra Meat", "price": 2.00}
              ]
            },
            {
              "id": "cust_salsa",
              "name": "Salsa Choice",
              "type": "single",
              "required": false,
              "options": [
                {"id": "opt_verde", "name": "Salsa Verde", "price": 0.00},
                {"id": "opt_roja", "name": "Salsa Roja", "price": 0.00},
                {"id": "opt_habanero", "name": "Habanero (Hot!)", "price": 0.50}
              ]
            },
            {
              "id": "cust_extras",
              "name": "Add Extras",
              "type": "multiple",
              "required": false,
              "maxSelections": 5,
              "options": [
                {"id": "opt_guac", "name": "Guacamole", "price": 1.50},
                {"id": "opt_sour_cream", "name": "Sour Cream", "price": 0.75},
                {"id": "opt_cheese", "name": "Extra Cheese", "price": 1.00}
              ]
            }
          ],
          "userDietaryMatch": {
            "safe": false,
            "warnings": ["Contains gluten"]
          }
        },
        {
          "id": "item_veggie_taco",
          "name": "Veggie Fiesta Taco",
          "description": "Grilled peppers, mushrooms, black beans, and queso fresco",
          "price": 4.00,
          "imageUrl": "https://cdn.tacotracker.globex.com/menu/veggie_taco.jpg",
          "calories": 280,
          "prepTime": 5,
          "available": true,
          "popular": false,
          "dietary": ["vegetarian", "gluten-free"],
          "allergens": ["dairy"],
          "customizations": [],
          "userDietaryMatch": {
            "safe": true,
            "warnings": []
          }
        }
      ]
    },
    {
      "id": "cat_drinks",
      "name": "Beverages",
      "description": "Refreshing drinks",
      "displayOrder": 3,
      "items": [
        {
          "id": "item_horchata",
          "name": "Horchata",
          "description": "Traditional Mexican rice drink with cinnamon",
          "price": 3.00,
          "imageUrl": "https://cdn.tacotracker.globex.com/menu/horchata.jpg",
          "calories": 180,
          "prepTime": 1,
          "available": true,
          "dietary": ["vegetarian"],
          "allergens": ["dairy"],
          "customizations": [
            {
              "id": "cust_size",
              "name": "Size",
              "type": "single",
              "required": true,
              "options": [
                {"id": "opt_regular", "name": "Regular (16oz)", "price": 0.00},
                {"id": "opt_large", "name": "Large (24oz)", "price": 1.50}
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

---

### 3.5 Order Endpoints

#### POST /orders

**Description:** Create a new order

**Authentication:** Required

**Rate Limiting:** 5 orders per minute per user

**Request:**
```json
{
  "vendorId": "vnd_taco_tornado",
  "truckId": "trk_tornado_1",
  "items": [
    {
      "menuItemId": "item_carnitas_taco",
      "quantity": 2,
      "customizations": [
        {"customizationId": "cust_salsa", "optionId": "opt_verde"},
        {"customizationId": "cust_extras", "optionIds": ["opt_guac"]}
      ],
      "specialInstructions": "Extra cilantro please"
    },
    {
      "menuItemId": "item_horchata",
      "quantity": 1,
      "customizations": [
        {"customizationId": "cust_size", "optionId": "opt_large"}
      ]
    }
  ],
  "tipAmount": 2.00,
  "useMealSubsidy": true,
  "paymentMethodId": "pm_stripe_abc123"
}
```

**Response (201 Created):**
```json
{
  "id": "ord_xyz789",
  "orderNumber": "TT-2026-0847",
  "status": "pending",
  "vendor": {
    "id": "vnd_taco_tornado",
    "name": "Taco Tornado",
    "logoUrl": "https://cdn.tacotracker.globex.com/vendors/taco_tornado/logo.png"
  },
  "truck": {
    "id": "trk_tornado_1",
    "location": {
      "campusName": "San Francisco HQ",
      "lotName": "Lot B - North",
      "latitude": 37.7749,
      "longitude": -122.4194
    }
  },
  "items": [
    {
      "id": "oli_001",
      "menuItemId": "item_carnitas_taco",
      "name": "Carnitas Taco",
      "quantity": 2,
      "unitPrice": 4.50,
      "customizations": [
        {"name": "Salsa Verde", "price": 0.00},
        {"name": "Guacamole", "price": 1.50}
      ],
      "specialInstructions": "Extra cilantro please",
      "subtotal": 12.00
    },
    {
      "id": "oli_002",
      "menuItemId": "item_horchata",
      "name": "Horchata",
      "quantity": 1,
      "unitPrice": 3.00,
      "customizations": [
        {"name": "Large (24oz)", "price": 1.50}
      ],
      "subtotal": 4.50
    }
  ],
  "pricing": {
    "subtotal": 16.50,
    "tax": 1.40,
    "taxRate": 0.085,
    "tip": 2.00,
    "mealSubsidyApplied": 10.00,
    "total": 9.90
  },
  "payment": {
    "status": "requires_confirmation",
    "clientSecret": "pi_abc123_secret_xyz789"
  },
  "timing": {
    "estimatedPrepTime": 10,
    "estimatedReadyAt": "2026-01-22T12:25:00Z",
    "pickupWindow": {
      "start": "2026-01-22T12:25:00Z",
      "end": "2026-01-22T12:55:00Z"
    }
  },
  "qrCode": null,
  "createdAt": "2026-01-22T12:15:00Z"
}
```

---

#### GET /orders

**Description:** List user's orders

**Authentication:** Required

**Query Parameters:**

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| status | string | No | - | Filter by status |
| vendorId | string | No | - | Filter by vendor |
| from | date | No | 30 days ago | Start date |
| to | date | No | today | End date |
| page | integer | No | 1 | Page number |
| limit | integer | No | 20 | Results per page |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": "ord_xyz789",
      "orderNumber": "TT-2026-0847",
      "status": "completed",
      "vendor": {
        "id": "vnd_taco_tornado",
        "name": "Taco Tornado",
        "logoUrl": "https://cdn.tacotracker.globex.com/vendors/taco_tornado/logo.png"
      },
      "itemCount": 3,
      "total": 9.90,
      "createdAt": "2026-01-22T12:15:00Z",
      "completedAt": "2026-01-22T12:28:00Z"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 47,
    "totalPages": 3
  }
}
```

---

#### GET /orders/{orderId}

**Description:** Get order details

**Authentication:** Required

**Authorization:** Order owner or vendor

**Response (200 OK):**
```json
{
  "id": "ord_xyz789",
  "orderNumber": "TT-2026-0847",
  "status": "ready",
  "statusHistory": [
    {"status": "pending", "timestamp": "2026-01-22T12:15:00Z"},
    {"status": "confirmed", "timestamp": "2026-01-22T12:15:30Z"},
    {"status": "preparing", "timestamp": "2026-01-22T12:16:00Z"},
    {"status": "ready", "timestamp": "2026-01-22T12:23:00Z"}
  ],
  "vendor": {
    "id": "vnd_taco_tornado",
    "name": "Taco Tornado",
    "logoUrl": "https://cdn.tacotracker.globex.com/vendors/taco_tornado/logo.png",
    "phone": "+1-555-TACO-123"
  },
  "truck": {
    "id": "trk_tornado_1",
    "name": "Tornado One",
    "location": {
      "campusName": "San Francisco HQ",
      "lotName": "Lot B - North",
      "latitude": 37.7749,
      "longitude": -122.4194
    }
  },
  "items": [
    {
      "id": "oli_001",
      "name": "Carnitas Taco",
      "quantity": 2,
      "unitPrice": 4.50,
      "customizations": [
        {"name": "Salsa Verde", "price": 0.00},
        {"name": "Guacamole", "price": 1.50}
      ],
      "specialInstructions": "Extra cilantro please",
      "subtotal": 12.00
    },
    {
      "id": "oli_002",
      "name": "Horchata",
      "quantity": 1,
      "unitPrice": 3.00,
      "customizations": [
        {"name": "Large (24oz)", "price": 1.50}
      ],
      "subtotal": 4.50
    }
  ],
  "pricing": {
    "subtotal": 16.50,
    "tax": 1.40,
    "taxRate": 0.085,
    "tip": 2.00,
    "mealSubsidyApplied": 10.00,
    "total": 9.90
  },
  "payment": {
    "status": "succeeded",
    "method": "Visa ****4242",
    "transactionId": "txn_stripe_xyz789"
  },
  "timing": {
    "estimatedPrepTime": 10,
    "actualPrepTime": 8,
    "estimatedReadyAt": "2026-01-22T12:25:00Z",
    "actualReadyAt": "2026-01-22T12:23:00Z",
    "pickupWindow": {
      "start": "2026-01-22T12:23:00Z",
      "end": "2026-01-22T12:53:00Z"
    }
  },
  "qrCode": {
    "data": "TT-2026-0847:ord_xyz789:1705926780",
    "imageUrl": "https://api.tacotracker.globex.com/v1/orders/ord_xyz789/qr"
  },
  "canCancel": false,
  "canModify": false,
  "createdAt": "2026-01-22T12:15:00Z"
}
```

---

#### POST /orders/{orderId}/cancel

**Description:** Cancel an order

**Authentication:** Required

**Authorization:** Order owner, order status must be pending or confirmed

**Request:**
```json
{
  "reason": "Changed my mind"
}
```

**Response (200 OK):**
```json
{
  "id": "ord_xyz789",
  "status": "cancelled",
  "refund": {
    "amount": 9.90,
    "status": "pending",
    "estimatedArrival": "2026-01-25T12:00:00Z"
  },
  "cancelledAt": "2026-01-22T12:17:00Z"
}
```

**Error Responses:**

| Status | Code | Description |
|--------|------|-------------|
| 400 | ORDER_CANNOT_BE_CANCELLED | Order is already being prepared |
| 404 | ORDER_NOT_FOUND | Order does not exist |
| 403 | NOT_ORDER_OWNER | User does not own this order |

---

#### GET /orders/{orderId}/qr

**Description:** Get order QR code image

**Authentication:** Required

**Authorization:** Order owner

**Response (200 OK):**
- Content-Type: image/png
- Binary QR code image

---

### 3.6 Location Endpoints

#### GET /locations/nearby

**Description:** Get trucks near a location

**Authentication:** Required

**Query Parameters:**

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| latitude | number | No | User's location | Latitude coordinate |
| longitude | number | No | User's location | Longitude coordinate |
| radius | number | No | 1000 | Radius in meters (max 5000) |
| campusId | string | No | - | Filter to specific campus |

**Response (200 OK):**
```json
{
  "data": [
    {
      "truckId": "trk_tornado_1",
      "vendorId": "vnd_taco_tornado",
      "vendorName": "Taco Tornado",
      "cuisine": "Mexican",
      "logoUrl": "https://cdn.tacotracker.globex.com/vendors/taco_tornado/logo.png",
      "location": {
        "latitude": 37.7749,
        "longitude": -122.4194,
        "accuracy": 5,
        "updatedAt": "2026-01-22T12:10:00Z"
      },
      "campus": {
        "id": "campus_sf_hq",
        "name": "San Francisco HQ",
        "lotId": "lot_b",
        "lotName": "Lot B - North"
      },
      "distance": {
        "meters": 150,
        "walkTimeMinutes": 2
      },
      "status": "active",
      "waitTime": {
        "estimate": 12,
        "queueLength": 8
      },
      "operatingUntil": "14:00"
    }
  ],
  "meta": {
    "center": {
      "latitude": 37.7750,
      "longitude": -122.4195
    },
    "radius": 1000,
    "total": 2
  }
}
```

---

#### GET /locations/campus/{campusId}

**Description:** Get all trucks at a campus

**Authentication:** Required

**Response (200 OK):**
```json
{
  "campus": {
    "id": "campus_sf_hq",
    "name": "San Francisco HQ",
    "buildings": [
      {"id": "bldg_a", "name": "Building A", "latitude": 37.7752, "longitude": -122.4196},
      {"id": "bldg_b", "name": "Building B", "latitude": 37.7748, "longitude": -122.4192},
      {"id": "bldg_c", "name": "Building C", "latitude": 37.7745, "longitude": -122.4188}
    ],
    "lots": [
      {"id": "lot_a", "name": "Lot A - South", "latitude": 37.7754, "longitude": -122.4198},
      {"id": "lot_b", "name": "Lot B - North", "latitude": 37.7749, "longitude": -122.4194},
      {"id": "lot_c", "name": "Lot C - East", "latitude": 37.7746, "longitude": -122.4186}
    ]
  },
  "trucks": [
    {
      "truckId": "trk_tornado_1",
      "vendorId": "vnd_taco_tornado",
      "vendorName": "Taco Tornado",
      "lotId": "lot_b",
      "lotName": "Lot B - North",
      "location": {
        "latitude": 37.7749,
        "longitude": -122.4194,
        "updatedAt": "2026-01-22T12:10:00Z"
      },
      "status": "active",
      "waitTime": 12
    }
  ],
  "schedule": {
    "today": [
      {
        "vendorId": "vnd_taco_tornado",
        "vendorName": "Taco Tornado",
        "lotId": "lot_b",
        "arrivalTime": "11:00",
        "departureTime": "14:00",
        "status": "on_site"
      },
      {
        "vendorId": "vnd_la_taqueria",
        "vendorName": "La Taqueria",
        "lotId": "lot_a",
        "arrivalTime": "11:30",
        "departureTime": "14:30",
        "status": "arriving"
      }
    ]
  }
}
```

---

### 3.7 WebSocket API

#### Connection

```
URL: wss://api.tacotracker.globex.com/ws
Authentication: ?token=<jwt_access_token>
```

#### Events (Server to Client)

**location_update:**
```json
{
  "type": "location_update",
  "payload": {
    "truckId": "trk_tornado_1",
    "vendorId": "vnd_taco_tornado",
    "location": {
      "latitude": 37.7749,
      "longitude": -122.4194,
      "accuracy": 5
    },
    "status": "active",
    "waitTime": 12
  },
  "timestamp": "2026-01-22T12:10:30Z"
}
```

**truck_arrived:**
```json
{
  "type": "truck_arrived",
  "payload": {
    "truckId": "trk_tornado_1",
    "vendorId": "vnd_taco_tornado",
    "vendorName": "Taco Tornado",
    "campusId": "campus_sf_hq",
    "lotId": "lot_b",
    "lotName": "Lot B - North"
  },
  "timestamp": "2026-01-22T11:00:00Z"
}
```

**truck_departed:**
```json
{
  "type": "truck_departed",
  "payload": {
    "truckId": "trk_tornado_1",
    "vendorId": "vnd_taco_tornado",
    "vendorName": "Taco Tornado",
    "campusId": "campus_sf_hq"
  },
  "timestamp": "2026-01-22T14:00:00Z"
}
```

**order_status_changed:**
```json
{
  "type": "order_status_changed",
  "payload": {
    "orderId": "ord_xyz789",
    "orderNumber": "TT-2026-0847",
    "previousStatus": "preparing",
    "newStatus": "ready",
    "vendor": {
      "id": "vnd_taco_tornado",
      "name": "Taco Tornado"
    }
  },
  "timestamp": "2026-01-22T12:23:00Z"
}
```

#### Events (Client to Server)

**subscribe_campus:**
```json
{
  "type": "subscribe_campus",
  "payload": {
    "campusId": "campus_sf_hq"
  }
}
```

**subscribe_order:**
```json
{
  "type": "subscribe_order",
  "payload": {
    "orderId": "ord_xyz789"
  }
}
```

**ping:**
```json
{
  "type": "ping"
}
```

---

### 3.8 API Error Codes

| Error Code | HTTP Status | Description | Resolution |
|------------|-------------|-------------|------------|
| VALIDATION_ERROR | 400 | Request body validation failed | Check field requirements |
| INVALID_TOKEN | 401 | JWT token invalid or expired | Refresh token or re-authenticate |
| FORBIDDEN | 403 | User lacks permission | Check user role and permissions |
| NOT_FOUND | 404 | Resource not found | Verify resource ID |
| ORDER_NOT_FOUND | 404 | Order does not exist | Verify order ID |
| VENDOR_NOT_FOUND | 404 | Vendor does not exist | Verify vendor ID |
| MENU_ITEM_UNAVAILABLE | 400 | Menu item is currently unavailable | Remove item from order |
| ORDER_CANNOT_BE_CANCELLED | 400 | Order is past cancellation window | Contact vendor directly |
| PAYMENT_FAILED | 402 | Payment processing failed | Try different payment method |
| INSUFFICIENT_MEAL_SUBSIDY | 400 | Meal subsidy balance too low | Pay remaining with card |
| RATE_LIMIT_EXCEEDED | 429 | Too many requests | Wait and retry |
| INTERNAL_ERROR | 500 | Unexpected server error | Retry or contact support |

---

## 4. Database Schema Design

### 4.1 Entity Relationship Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    CORE DOMAIN MODEL                                         │
└─────────────────────────────────────────────────────────────────────────────────────────────┘

┌──────────────────┐         ┌──────────────────┐         ┌──────────────────┐
│      users       │         │     vendors      │         │      trucks      │
├──────────────────┤         ├──────────────────┤         ├──────────────────┤
│ id (PK)          │         │ id (PK)          │         │ id (PK)          │
│ okta_id (UK)     │         │ name             │◄────────┤ vendor_id (FK)   │
│ email (UK)       │         │ description      │    1:N  │ name             │
│ first_name       │         │ cuisine          │         │ license_plate    │
│ last_name        │         │ logo_url         │         │ status           │
│ department       │         │ rating           │         │ gps_device_id    │
│ campus_id (FK)   │         │ status           │         │ created_at       │
│ role             │         │ created_at       │         └────────┬─────────┘
│ preferences      │         └────────┬─────────┘                  │
│ created_at       │                  │                            │
└────────┬─────────┘                  │                            │
         │                   ┌────────┴─────────┐                  │
         │                   │                  │                  │
         │              ┌────┴───────┐    ┌─────┴──────┐          │
         │              │   menus    │    │ schedules  │          │
         │              ├────────────┤    ├────────────┤          │
         │              │ id (PK)    │    │ id (PK)    │          │
         │              │ vendor_id  │    │ vendor_id  │          │
         │              │ name       │    │ truck_id   │          │
         │              │ is_active  │    │ campus_id  │          │
         │              └────┬───────┘    │ day_of_week│          │
         │                   │            │ start_time │          │
         │              ┌────┴───────┐    │ end_time   │          │
         │              │ menu_items │    └────────────┘          │
         │              ├────────────┤                            │
         │              │ id (PK)    │                            │
         │              │ menu_id    │         ┌──────────────────┴──────────────────┐
         │              │ name       │         │          truck_locations            │
         │              │ description│         ├─────────────────────────────────────┤
         │              │ price      │         │ truck_id (PK, FK)                   │
         │              │ calories   │         │ timestamp (PK)                      │
         │              │ dietary    │         │ latitude                            │
         │              │ allergens  │         │ longitude                           │
         │              │ available  │         │ accuracy                            │
         │              │ image_url  │         │ source (GPS/WiFi)                   │
         │              └────────────┘         └─────────────────────────────────────┘
         │
         │         ┌──────────────────┐         ┌──────────────────┐
         │         │      orders      │         │   order_items    │
         │    1:N  ├──────────────────┤    1:N  ├──────────────────┤
         └────────►│ id (PK)          │────────►│ id (PK)          │
                   │ user_id (FK)     │         │ order_id (FK)    │
                   │ vendor_id (FK)   │         │ menu_item_id (FK)│
                   │ truck_id (FK)    │         │ quantity         │
                   │ order_number     │         │ unit_price       │
                   │ status           │         │ customizations   │
                   │ subtotal         │         │ special_requests │
                   │ tax              │         └──────────────────┘
                   │ tip              │
                   │ total            │         ┌──────────────────┐
                   │ payment_id       │         │     payments     │
                   │ pickup_time      │    1:1  ├──────────────────┤
                   │ created_at       │────────►│ id (PK)          │
                   └──────────────────┘         │ order_id (FK)    │
                                                │ stripe_payment_id│
                                                │ amount           │
                                                │ status           │
                                                │ meal_subsidy_used│
                                                │ created_at       │
                                                └──────────────────┘

┌──────────────────┐         ┌──────────────────┐
│ user_favorites   │         │    campuses      │
├──────────────────┤         ├──────────────────┤
│ user_id (PK, FK) │         │ id (PK)          │
│ vendor_id (PK,FK)│         │ name             │
│ created_at       │         │ address          │
└──────────────────┘         │ timezone         │
                             │ boundary (GIS)   │
                             └────────┬─────────┘
                                      │
                             ┌────────┴─────────┐
                             │ parking_lots     │
                             ├──────────────────┤
                             │ id (PK)          │
                             │ campus_id (FK)   │
                             │ name             │
                             │ location (GIS)   │
                             └──────────────────┘
```

### 4.2 Table Definitions

#### Users Table
```sql
-- Table: users
-- Purpose: Store employee user accounts and preferences
-- Estimated rows: 50,000

CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    okta_id VARCHAR(128) NOT NULL UNIQUE,
    email VARCHAR(255) NOT NULL UNIQUE,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    department VARCHAR(100),
    campus_id UUID NOT NULL REFERENCES campuses(id),
    role VARCHAR(50) NOT NULL DEFAULT 'employee' CHECK (role IN ('employee', 'vendor_admin', 'vendor_staff', 'food_services_admin', 'system_admin')),
    meal_subsidy_balance DECIMAL(10, 2) DEFAULT 0.00,
    meal_subsidy_monthly DECIMAL(10, 2) DEFAULT 200.00,
    preferences JSONB DEFAULT '{"dietary": [], "allergens": [], "notifications": {"arrivalAlerts": true, "orderUpdates": true, "dailyDigest": false, "promotional": false}}',
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_users_okta_id ON users(okta_id);
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_campus_id ON users(campus_id);

-- Trigger for updated_at
CREATE TRIGGER update_users_updated_at
    BEFORE UPDATE ON users
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
```

#### Vendors Table
```sql
-- Table: vendors
-- Purpose: Store food truck vendor information
-- Estimated rows: 500

CREATE TABLE vendors (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(200) NOT NULL,
    description TEXT,
    cuisine VARCHAR(100) NOT NULL,
    logo_url VARCHAR(500),
    banner_url VARCHAR(500),
    phone VARCHAR(20),
    email VARCHAR(255),
    tax_id VARCHAR(50),
    rating DECIMAL(2, 1) DEFAULT 0.0,
    review_count INTEGER DEFAULT 0,
    price_range VARCHAR(10) CHECK (price_range IN ('$', '$$', '$$$')),
    dietary_options TEXT[] DEFAULT '{}',
    allergen_info TEXT,
    social_media JSONB DEFAULT '{}',
    payment_methods TEXT[] DEFAULT '{credit, debit}',
    status VARCHAR(50) NOT NULL DEFAULT 'pending' CHECK (status IN ('pending', 'active', 'suspended', 'inactive')),
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_vendors_status ON vendors(status);
CREATE INDEX idx_vendors_cuisine ON vendors(cuisine);
CREATE INDEX idx_vendors_rating ON vendors(rating DESC);
```

#### Orders Table
```sql
-- Table: orders
-- Purpose: Store order transactions
-- Estimated rows: 1,000,000+ (partitioned by month)

CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_number VARCHAR(20) NOT NULL UNIQUE,
    user_id UUID NOT NULL REFERENCES users(id),
    vendor_id UUID NOT NULL REFERENCES vendors(id),
    truck_id UUID NOT NULL REFERENCES trucks(id),
    status VARCHAR(50) NOT NULL DEFAULT 'pending' CHECK (status IN ('pending', 'confirmed', 'preparing', 'ready', 'picked_up', 'completed', 'cancelled', 'abandoned')),
    subtotal DECIMAL(10, 2) NOT NULL,
    tax DECIMAL(10, 2) NOT NULL,
    tax_rate DECIMAL(5, 4) NOT NULL DEFAULT 0.0850,
    tip DECIMAL(10, 2) DEFAULT 0.00,
    meal_subsidy_applied DECIMAL(10, 2) DEFAULT 0.00,
    total DECIMAL(10, 2) NOT NULL,
    special_instructions TEXT,
    estimated_ready_at TIMESTAMP WITH TIME ZONE,
    actual_ready_at TIMESTAMP WITH TIME ZONE,
    picked_up_at TIMESTAMP WITH TIME ZONE,
    cancelled_at TIMESTAMP WITH TIME ZONE,
    cancellation_reason TEXT,
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW()
) PARTITION BY RANGE (created_at);

-- Create partitions for each month
CREATE TABLE orders_2026_01 PARTITION OF orders
    FOR VALUES FROM ('2026-01-01') TO ('2026-02-01');
CREATE TABLE orders_2026_02 PARTITION OF orders
    FOR VALUES FROM ('2026-02-01') TO ('2026-03-01');
-- Continue for each month...

-- Indexes
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_orders_vendor_id ON orders(vendor_id);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_created_at ON orders(created_at DESC);
CREATE INDEX idx_orders_order_number ON orders(order_number);
```

#### Order Items Table
```sql
-- Table: order_items
-- Purpose: Store individual items within orders
-- Estimated rows: 3,000,000+

CREATE TABLE order_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_id UUID NOT NULL REFERENCES orders(id) ON DELETE CASCADE,
    menu_item_id UUID NOT NULL REFERENCES menu_items(id),
    quantity INTEGER NOT NULL CHECK (quantity > 0 AND quantity <= 10),
    unit_price DECIMAL(10, 2) NOT NULL,
    customizations JSONB DEFAULT '[]',
    special_instructions TEXT,
    subtotal DECIMAL(10, 2) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_order_items_order_id ON order_items(order_id);
CREATE INDEX idx_order_items_menu_item_id ON order_items(menu_item_id);
```

#### Menu Items Table
```sql
-- Table: menu_items
-- Purpose: Store menu items for vendors
-- Estimated rows: 5,000

CREATE TABLE menu_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    menu_id UUID NOT NULL REFERENCES menus(id),
    name VARCHAR(200) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL CHECK (price >= 0),
    calories INTEGER,
    prep_time_minutes INTEGER DEFAULT 5,
    image_url VARCHAR(500),
    category VARCHAR(100) NOT NULL,
    dietary TEXT[] DEFAULT '{}',
    allergens TEXT[] DEFAULT '{}',
    is_available BOOLEAN DEFAULT true,
    is_popular BOOLEAN DEFAULT false,
    display_order INTEGER DEFAULT 0,
    customizations JSONB DEFAULT '[]',
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_menu_items_menu_id ON menu_items(menu_id);
CREATE INDEX idx_menu_items_category ON menu_items(category);
CREATE INDEX idx_menu_items_available ON menu_items(is_available) WHERE is_available = true;
CREATE INDEX idx_menu_items_dietary ON menu_items USING GIN(dietary);
```

#### Payments Table
```sql
-- Table: payments
-- Purpose: Store payment transactions
-- Estimated rows: 1,000,000+

CREATE TABLE payments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_id UUID NOT NULL UNIQUE REFERENCES orders(id),
    stripe_payment_intent_id VARCHAR(100) NOT NULL UNIQUE,
    stripe_charge_id VARCHAR(100),
    amount DECIMAL(10, 2) NOT NULL,
    currency VARCHAR(3) DEFAULT 'USD',
    status VARCHAR(50) NOT NULL CHECK (status IN ('pending', 'requires_confirmation', 'processing', 'succeeded', 'failed', 'cancelled', 'refunded', 'partially_refunded')),
    payment_method_type VARCHAR(50),
    payment_method_last4 VARCHAR(4),
    meal_subsidy_amount DECIMAL(10, 2) DEFAULT 0.00,
    refund_amount DECIMAL(10, 2) DEFAULT 0.00,
    refund_reason TEXT,
    failure_code VARCHAR(100),
    failure_message TEXT,
    metadata JSONB DEFAULT '{}',
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_payments_order_id ON payments(order_id);
CREATE INDEX idx_payments_stripe_payment_intent_id ON payments(stripe_payment_intent_id);
CREATE INDEX idx_payments_status ON payments(status);
```

### 4.3 Database Indexing Strategy

| Table | Index Name | Columns | Type | Rationale |
|-------|------------|---------|------|-----------|
| users | idx_users_okta_id | okta_id | B-tree | SSO lookup |
| users | idx_users_campus_id | campus_id | B-tree | Campus filtering |
| orders | idx_orders_user_status | user_id, status | B-tree | User order history |
| orders | idx_orders_vendor_date | vendor_id, created_at | B-tree | Vendor analytics |
| menu_items | idx_menu_items_dietary | dietary | GIN | Dietary filtering |
| trucks | idx_trucks_location | location | GiST | Geospatial queries |

### 4.4 DynamoDB Schema (Location Time-Series)

**Table: truck_locations**

| Attribute | Type | Key Type | Description |
|-----------|------|----------|-------------|
| truck_id | String | Partition Key | Truck identifier |
| timestamp | Number | Sort Key | Unix epoch milliseconds |
| latitude | Number | - | Latitude coordinate |
| longitude | Number | - | Longitude coordinate |
| accuracy | Number | - | GPS accuracy in meters |
| speed | Number | - | Speed in km/h |
| heading | Number | - | Heading in degrees |
| source | String | - | GPS, WiFi, or Cell |
| ttl | Number | - | Time-to-live (90 days) |

**GSI: campus_timestamp**
- Partition Key: campus_id
- Sort Key: timestamp
- Purpose: Query all trucks at a campus by time range

---

## 5. Data Models and Types

### 5.1 TypeScript Interfaces

```typescript
// ============================================
// Domain Entities
// ============================================

interface User {
  id: string;
  oktaId: string;
  email: string;
  firstName: string;
  lastName: string;
  department: string | null;
  campusId: string;
  role: UserRole;
  mealSubsidy: MealSubsidy;
  preferences: UserPreferences;
  createdAt: Date;
  updatedAt: Date;
}

enum UserRole {
  EMPLOYEE = 'employee',
  VENDOR_ADMIN = 'vendor_admin',
  VENDOR_STAFF = 'vendor_staff',
  FOOD_SERVICES_ADMIN = 'food_services_admin',
  SYSTEM_ADMIN = 'system_admin'
}

interface MealSubsidy {
  balance: number;
  monthlyAllowance: number;
  resetDate: Date;
}

interface UserPreferences {
  dietary: DietaryOption[];
  allergens: AllergenType[];
  notifications: NotificationPreferences;
}

enum DietaryOption {
  VEGETARIAN = 'vegetarian',
  VEGAN = 'vegan',
  GLUTEN_FREE = 'gluten-free',
  HALAL = 'halal',
  KOSHER = 'kosher'
}

enum AllergenType {
  NUTS = 'nuts',
  DAIRY = 'dairy',
  GLUTEN = 'gluten',
  SHELLFISH = 'shellfish',
  SOY = 'soy',
  EGGS = 'eggs'
}

interface NotificationPreferences {
  arrivalAlerts: boolean;
  orderUpdates: boolean;
  dailyDigest: boolean;
  promotional: boolean;
}

// ============================================
// Order Domain
// ============================================

interface Order {
  id: string;
  orderNumber: string;
  userId: string;
  vendorId: string;
  truckId: string;
  status: OrderStatus;
  items: OrderItem[];
  pricing: OrderPricing;
  timing: OrderTiming;
  specialInstructions: string | null;
  createdAt: Date;
  updatedAt: Date;
}

enum OrderStatus {
  PENDING = 'pending',
  CONFIRMED = 'confirmed',
  PREPARING = 'preparing',
  READY = 'ready',
  PICKED_UP = 'picked_up',
  COMPLETED = 'completed',
  CANCELLED = 'cancelled',
  ABANDONED = 'abandoned'
}

interface OrderItem {
  id: string;
  menuItemId: string;
  name: string;
  quantity: number;
  unitPrice: number;
  customizations: OrderItemCustomization[];
  specialInstructions: string | null;
  subtotal: number;
}

interface OrderItemCustomization {
  customizationId: string;
  name: string;
  optionId: string;
  optionName: string;
  price: number;
}

interface OrderPricing {
  subtotal: number;
  tax: number;
  taxRate: number;
  tip: number;
  mealSubsidyApplied: number;
  total: number;
}

interface OrderTiming {
  estimatedPrepTime: number;
  actualPrepTime: number | null;
  estimatedReadyAt: Date;
  actualReadyAt: Date | null;
  pickupWindow: {
    start: Date;
    end: Date;
  };
}

// ============================================
// DTOs (Data Transfer Objects)
// ============================================

interface CreateOrderRequest {
  vendorId: string;
  truckId: string;
  items: CreateOrderItemRequest[];
  tipAmount?: number;
  useMealSubsidy?: boolean;
  paymentMethodId: string;
}

interface CreateOrderItemRequest {
  menuItemId: string;
  quantity: number;
  customizations?: {
    customizationId: string;
    optionId?: string;
    optionIds?: string[];
  }[];
  specialInstructions?: string;
}

interface OrderResponse {
  id: string;
  orderNumber: string;
  status: OrderStatus;
  vendor: VendorSummary;
  truck: TruckLocation;
  items: OrderItemResponse[];
  pricing: OrderPricing;
  payment: PaymentResponse;
  timing: OrderTiming;
  qrCode: QRCodeData | null;
  createdAt: string;
}

interface PaginatedResponse<T> {
  data: T[];
  meta: {
    page: number;
    limit: number;
    total: number;
    totalPages: number;
  };
}
```

### 5.2 Validation Rules

| Field | Type | Validation Rules | Error Message |
|-------|------|------------------|---------------|
| order.items | array | min: 1, max: 10 | "Order must have 1-10 items" |
| order.items[].quantity | number | min: 1, max: 10 | "Quantity must be between 1 and 10" |
| order.tipAmount | number | min: 0, max: 100 | "Tip must be between $0 and $100" |
| user.email | string | valid email, @globex.com | "Must use Globex email address" |
| menu_item.price | number | min: 0, max: 999.99 | "Price must be between $0 and $999.99" |

---

## 6. Service Layer Design

### 6.1 Service Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                  SERVICE LAYER                                       │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                      │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐│
│  │   OrderService  │  │   UserService   │  │  VendorService  │  │ LocationService ││
│  │                 │  │                 │  │                 │  │                 ││
│  │ • createOrder   │  │ • getProfile    │  │ • listVendors   │  │ • getNearby     ││
│  │ • getOrder      │  │ • updatePrefs   │  │ • getVendor     │  │ • subscribe     ││
│  │ • cancelOrder   │  │ • getFavorites  │  │ • getMenu       │  │ • broadcast     ││
│  │ • updateStatus  │  │ • addFavorite   │  │ • updateMenu    │  │ • processGPS    ││
│  └────────┬────────┘  └────────┬────────┘  └────────┬────────┘  └────────┬────────┘│
│           │                    │                    │                    │          │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐│
│  │ PaymentService  │  │ NotificationSvc │  │ AnalyticsService│  │   AuthService   ││
│  │                 │  │                 │  │                 │  │                 ││
│  │ • createPayment │  │ • sendPush      │  │ • trackEvent    │  │ • validateToken ││
│  │ • confirmPayment│  │ • sendEmail     │  │ • getMetrics    │  │ • refreshToken  ││
│  │ • refundPayment │  │ • sendInApp     │  │ • generateReport│  │ • getUserFromJWT││
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘│
│                                                                                      │
└──────────────────────────────────────────────┬──────────────────────────────────────┘
                                               │
                                               ▼
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                REPOSITORY LAYER                                      │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                      │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐│
│  │ OrderRepository │  │  UserRepository │  │ VendorRepository│  │ LocationRepo    ││
│  │   (Prisma)      │  │   (Prisma)      │  │   (Prisma)      │  │  (DynamoDB)     ││
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘│
│                                                                                      │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

### 6.2 OrderService Implementation

```typescript
/**
 * OrderService
 *
 * Responsibility: Manages order lifecycle from creation to completion
 * Dependencies: OrderRepository, PaymentService, NotificationService, VendorService, EventPublisher
 */
class OrderService {
  constructor(
    private readonly orderRepository: OrderRepository,
    private readonly paymentService: PaymentService,
    private readonly notificationService: NotificationService,
    private readonly vendorService: VendorService,
    private readonly eventPublisher: EventPublisher,
    private readonly cacheService: CacheService
  ) {}

  /**
   * createOrder
   *
   * Creates a new order, validates items, calculates pricing, and initiates payment.
   *
   * @param userId - The authenticated user's ID
   * @param request - Order creation request
   * @returns Created order with payment client secret
   * @throws ValidationError - If order items are invalid
   * @throws PaymentError - If payment initiation fails
   */
  async createOrder(userId: string, request: CreateOrderRequest): Promise<OrderResponse> {
    // 1. Validate vendor and truck are available
    const vendor = await this.vendorService.getVendor(request.vendorId);
    if (vendor.status !== 'active') {
      throw new ValidationError('VENDOR_NOT_AVAILABLE', 'Vendor is not currently accepting orders');
    }

    // 2. Validate and price menu items
    const pricedItems = await this.validateAndPriceItems(request.vendorId, request.items);

    // 3. Calculate order totals
    const pricing = this.calculatePricing(pricedItems, request.tipAmount, request.useMealSubsidy);

    // 4. Generate order number
    const orderNumber = this.generateOrderNumber();

    // 5. Create order record
    const order = await this.orderRepository.create({
      orderNumber,
      userId,
      vendorId: request.vendorId,
      truckId: request.truckId,
      status: OrderStatus.PENDING,
      items: pricedItems,
      pricing,
      estimatedReadyAt: this.calculateEstimatedReadyTime(pricedItems)
    });

    // 6. Initiate payment
    const payment = await this.paymentService.createPaymentIntent({
      orderId: order.id,
      amount: pricing.total,
      userId,
      paymentMethodId: request.paymentMethodId,
      mealSubsidyAmount: pricing.mealSubsidyApplied
    });

    // 7. Publish order created event
    await this.eventPublisher.publish('order-events', {
      type: 'ORDER_CREATED',
      payload: { orderId: order.id, userId, vendorId: request.vendorId }
    });

    return this.mapToResponse(order, payment);
  }

  /**
   * cancelOrder
   *
   * Cancels an order if within cancellation window.
   *
   * @param orderId - Order to cancel
   * @param userId - User requesting cancellation
   * @param reason - Cancellation reason
   * @returns Updated order with refund information
   * @throws OrderCannotBeCancelledError - If order is past cancellation window
   */
  async cancelOrder(orderId: string, userId: string, reason: string): Promise<Order> {
    const order = await this.orderRepository.findById(orderId);

    if (order.userId !== userId) {
      throw new ForbiddenError('NOT_ORDER_OWNER', 'You can only cancel your own orders');
    }

    if (!this.canBeCancelled(order)) {
      throw new ValidationError('ORDER_CANNOT_BE_CANCELLED', 'Order is already being prepared');
    }

    // Process refund
    const refund = await this.paymentService.refundPayment(order.paymentId, order.pricing.total);

    // Update order status
    const updatedOrder = await this.orderRepository.update(orderId, {
      status: OrderStatus.CANCELLED,
      cancelledAt: new Date(),
      cancellationReason: reason
    });

    // Notify vendor
    await this.notificationService.sendVendorNotification(order.vendorId, {
      type: 'ORDER_CANCELLED',
      orderId: order.id,
      orderNumber: order.orderNumber
    });

    // Publish event
    await this.eventPublisher.publish('order-events', {
      type: 'ORDER_CANCELLED',
      payload: { orderId, userId, reason }
    });

    return updatedOrder;
  }

  private canBeCancelled(order: Order): boolean {
    const cancellableStatuses = [OrderStatus.PENDING, OrderStatus.CONFIRMED];
    return cancellableStatuses.includes(order.status);
  }

  private generateOrderNumber(): string {
    const date = new Date();
    const year = date.getFullYear();
    const sequence = Math.floor(Math.random() * 9999).toString().padStart(4, '0');
    return `TT-${year}-${sequence}`;
  }
}
```

### 6.3 Sequence Diagram: Order Creation

```
┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
│  Mobile  │   │   API    │   │  Order   │   │  Vendor  │   │ Payment  │   │   SNS    │
│   App    │   │ Gateway  │   │ Service  │   │ Service  │   │ Service  │   │  Topic   │
└────┬─────┘   └────┬─────┘   └────┬─────┘   └────┬─────┘   └────┬─────┘   └────┬─────┘
     │              │              │              │              │              │
     │ POST /orders │              │              │              │              │
     │─────────────►│              │              │              │              │
     │              │              │              │              │              │
     │              │ createOrder()│              │              │              │
     │              │─────────────►│              │              │              │
     │              │              │              │              │              │
     │              │              │ getVendor()  │              │              │
     │              │              │─────────────►│              │              │
     │              │              │              │              │              │
     │              │              │◄─────────────│              │              │
     │              │              │ vendor       │              │              │
     │              │              │              │              │              │
     │              │              │ validateItems()             │              │
     │              │              │──────────────►              │              │
     │              │              │              │              │              │
     │              │              │◄──────────────              │              │
     │              │              │ pricedItems  │              │              │
     │              │              │              │              │              │
     │              │              │ createPaymentIntent()       │              │
     │              │              │──────────────────────────────►             │
     │              │              │              │              │              │
     │              │              │◄──────────────────────────────             │
     │              │              │ clientSecret │              │              │
     │              │              │              │              │              │
     │              │              │ publish(ORDER_CREATED)      │              │
     │              │              │────────────────────────────────────────────►
     │              │              │              │              │              │
     │              │◄─────────────│              │              │              │
     │              │ order + secret              │              │              │
     │              │              │              │              │              │
     │◄─────────────│              │              │              │              │
     │ 201 Created  │              │              │              │              │
     │              │              │              │              │              │
     ▼              ▼              ▼              ▼              ▼              ▼
```

---

## 7. Authentication and Authorization

### 7.1 Authentication Flow

```
┌──────────────────────────────────────────────────────────────────────────────────────┐
│                           OKTA OIDC AUTHENTICATION FLOW                               │
└──────────────────────────────────────────────────────────────────────────────────────┘

┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  Mobile  │     │   API    │     │   Okta   │     │   User   │     │  Backend │
│   App    │     │ Gateway  │     │   IdP    │     │          │     │ Services │
└────┬─────┘     └────┬─────┘     └────┬─────┘     └────┬─────┘     └────┬─────┘
     │                │                │                │                │
     │ 1. Tap Login   │                │                │                │
     │────────────────►                │                │                │
     │                │                │                │                │
     │ 2. POST /auth/login             │                │                │
     │ {redirect_uri} │                │                │                │
     │───────────────►│                │                │                │
     │                │                │                │                │
     │ 3. Generate PKCE code_verifier  │                │                │
     │                │──┐             │                │                │
     │                │◄─┘             │                │                │
     │                │                │                │                │
     │◄───────────────│                │                │                │
     │ auth_url,      │                │                │                │
     │ state,         │                │                │                │
     │ code_verifier  │                │                │                │
     │                │                │                │                │
     │ 4. Open browser│                │                │                │
     │ (in-app)       │                │                │                │
     │─────────────────────────────────►                │                │
     │                │                │                │                │
     │                │                │ 5. Login page  │                │
     │                │                │───────────────►│                │
     │                │                │                │                │
     │                │                │◄───────────────│                │
     │                │                │ 6. Credentials │                │
     │                │                │    + MFA       │                │
     │                │                │                │                │
     │◄─────────────────────────────────                │                │
     │ 7. Redirect:   │                │                │                │
     │ tacotracker:// │                │                │                │
     │ ?code=xyz      │                │                │                │
     │                │                │                │                │
     │ 8. POST /auth/callback          │                │                │
     │ {code, state,  │                │                │                │
     │  code_verifier}│                │                │                │
     │───────────────►│                │                │                │
     │                │                │                │                │
     │                │ 9. POST /token │                │                │
     │                │ {code,         │                │                │
     │                │  code_verifier}│                │                │
     │                │───────────────►│                │                │
     │                │                │                │                │
     │                │◄───────────────│                │                │
     │                │ 10. access_token,              │                │
     │                │     refresh_token,             │                │
     │                │     id_token   │                │                │
     │                │                │                │                │
     │                │ 11. Validate + │                │                │
     │                │ Create session │                │                │
     │                │──┐             │                │                │
     │                │◄─┘             │                │                │
     │                │                │                │                │
     │◄───────────────│                │                │                │
     │ 12. tokens +   │                │                │                │
     │ user profile   │                │                │                │
     │                │                │                │                │
     ▼                ▼                ▼                ▼                ▼
```

### 7.2 JWT Token Structure

```json
{
  "header": {
    "alg": "RS256",
    "typ": "JWT",
    "kid": "okta-signing-key-id"
  },
  "payload": {
    "sub": "00u1a2b3c4d5e6f7g8h9",
    "email": "john.doe@globex.com",
    "email_verified": true,
    "given_name": "John",
    "family_name": "Doe",
    "groups": ["Globex-Employees", "SF-Campus"],
    "tacotracker_role": "employee",
    "tacotracker_campus_id": "campus_sf_hq",
    "iat": 1705924800,
    "exp": 1705928400,
    "iss": "https://globex.okta.com/oauth2/default",
    "aud": "0oa1b2c3d4e5f6g7h8i9"
  }
}
```

### 7.3 Authorization Rules

| Resource | Action | Employee | Vendor Admin | Vendor Staff | Food Services | System Admin |
|----------|--------|----------|--------------|--------------|---------------|--------------|
| Orders | Create own | ✓ | ✗ | ✗ | ✗ | ✓ |
| Orders | View own | ✓ | ✗ | ✗ | ✓ | ✓ |
| Orders | View vendor's | ✗ | ✓ | ✓ | ✓ | ✓ |
| Orders | Cancel own | ✓ | ✗ | ✗ | ✗ | ✓ |
| Vendors | View all | ✓ | ✓ | ✓ | ✓ | ✓ |
| Vendors | Update own | ✗ | ✓ | ✗ | ✗ | ✓ |
| Menus | View | ✓ | ✓ | ✓ | ✓ | ✓ |
| Menus | Update own | ✗ | ✓ | ✗ | ✗ | ✓ |
| Users | View self | ✓ | ✓ | ✓ | ✓ | ✓ |
| Users | View all | ✗ | ✗ | ✗ | ✓ | ✓ |
| Analytics | View own | ✓ | ✓ | ✗ | ✓ | ✓ |
| Analytics | View all | ✗ | ✗ | ✗ | ✓ | ✓ |

### 7.4 Authorization Middleware

```typescript
// Authorization middleware implementation
import { Request, Response, NextFunction } from 'express';
import { ForbiddenError } from '../errors';

type Permission = 'orders:create' | 'orders:read' | 'orders:cancel' | 'vendors:update' | 'menus:update' | 'analytics:view';

const rolePermissions: Record<string, Permission[]> = {
  employee: ['orders:create', 'orders:read', 'orders:cancel'],
  vendor_admin: ['orders:read', 'vendors:update', 'menus:update', 'analytics:view'],
  vendor_staff: ['orders:read'],
  food_services_admin: ['orders:read', 'analytics:view'],
  system_admin: ['orders:create', 'orders:read', 'orders:cancel', 'vendors:update', 'menus:update', 'analytics:view']
};

export function requirePermission(permission: Permission) {
  return (req: Request, res: Response, next: NextFunction) => {
    const userRole = req.user?.role;

    if (!userRole) {
      throw new ForbiddenError('UNAUTHORIZED', 'Authentication required');
    }

    const permissions = rolePermissions[userRole] || [];

    if (!permissions.includes(permission)) {
      throw new ForbiddenError('FORBIDDEN', `Permission '${permission}' required`);
    }

    next();
  };
}

// Resource ownership check
export function requireOwnership(resourceGetter: (req: Request) => Promise<{ userId: string }>) {
  return async (req: Request, res: Response, next: NextFunction) => {
    const resource = await resourceGetter(req);

    if (resource.userId !== req.user?.id && req.user?.role !== 'system_admin') {
      throw new ForbiddenError('NOT_OWNER', 'You do not own this resource');
    }

    next();
  };
}
```

---

## 8. Third-Party Integrations

### 8.1 Stripe Integration

**Purpose:** Payment processing for orders

**Authentication:**
```
Header: Authorization: Bearer sk_live_xxx (server-side)
Client: Publishable key pk_live_xxx (mobile app)
```

**Endpoints Used:**

| Endpoint | Purpose | Rate Limit |
|----------|---------|------------|
| POST /v1/payment_intents | Create payment | 100/sec |
| POST /v1/payment_intents/:id/confirm | Confirm payment | 100/sec |
| POST /v1/refunds | Process refund | 100/sec |
| GET /v1/payment_methods/:id | Get saved card | 100/sec |

**Webhook Events:**

| Event | Handler |
|-------|---------|
| payment_intent.succeeded | Update order to confirmed |
| payment_intent.payment_failed | Update order to failed |
| charge.refunded | Update payment status |

**Implementation:**

```typescript
// Stripe service implementation
import Stripe from 'stripe';

class StripePaymentService implements PaymentService {
  private stripe: Stripe;

  constructor() {
    this.stripe = new Stripe(process.env.STRIPE_SECRET_KEY!, {
      apiVersion: '2023-10-16',
      typescript: true
    });
  }

  async createPaymentIntent(params: CreatePaymentParams): Promise<PaymentIntent> {
    const paymentIntent = await this.stripe.paymentIntents.create({
      amount: Math.round(params.amount * 100), // Convert to cents
      currency: 'usd',
      customer: params.stripeCustomerId,
      payment_method: params.paymentMethodId,
      metadata: {
        order_id: params.orderId,
        user_id: params.userId
      },
      automatic_payment_methods: {
        enabled: true,
        allow_redirects: 'never'
      }
    });

    return {
      id: paymentIntent.id,
      clientSecret: paymentIntent.client_secret!,
      status: this.mapStatus(paymentIntent.status)
    };
  }

  async handleWebhook(payload: Buffer, signature: string): Promise<void> {
    const event = this.stripe.webhooks.constructEvent(
      payload,
      signature,
      process.env.STRIPE_WEBHOOK_SECRET!
    );

    switch (event.type) {
      case 'payment_intent.succeeded':
        await this.handlePaymentSuccess(event.data.object as Stripe.PaymentIntent);
        break;
      case 'payment_intent.payment_failed':
        await this.handlePaymentFailure(event.data.object as Stripe.PaymentIntent);
        break;
    }
  }
}
```

### 8.2 Firebase Cloud Messaging Integration

**Purpose:** Push notifications to mobile devices

**Authentication:**
```
Service Account: firebase-adminsdk-xxx.json
```

**Message Types:**

| Type | Priority | TTL | Collapsible |
|------|----------|-----|-------------|
| Order status | High | 1 hour | No |
| Truck arrival | Normal | 4 hours | Yes |
| Daily digest | Low | 24 hours | Yes |

**Implementation:**

```typescript
import * as admin from 'firebase-admin';

class FCMNotificationService implements PushNotificationService {
  private messaging: admin.messaging.Messaging;

  constructor() {
    admin.initializeApp({
      credential: admin.credential.cert(serviceAccount)
    });
    this.messaging = admin.messaging();
  }

  async sendOrderStatusNotification(
    deviceToken: string,
    order: Order,
    newStatus: OrderStatus
  ): Promise<void> {
    const message: admin.messaging.Message = {
      token: deviceToken,
      notification: {
        title: this.getStatusTitle(newStatus),
        body: this.getStatusBody(order, newStatus)
      },
      data: {
        type: 'order_status',
        orderId: order.id,
        orderNumber: order.orderNumber,
        status: newStatus,
        deepLink: `tacotracker://orders/${order.id}`
      },
      android: {
        priority: 'high',
        notification: {
          channelId: 'order_updates',
          sound: 'default'
        }
      },
      apns: {
        payload: {
          aps: {
            sound: 'default',
            badge: 1
          }
        }
      }
    };

    await this.messaging.send(message);
  }

  private getStatusTitle(status: OrderStatus): string {
    const titles: Record<OrderStatus, string> = {
      [OrderStatus.CONFIRMED]: 'Order Confirmed!',
      [OrderStatus.PREPARING]: 'Your Order is Being Prepared',
      [OrderStatus.READY]: 'Your Order is Ready!',
      [OrderStatus.COMPLETED]: 'Thanks for Your Order!',
      [OrderStatus.CANCELLED]: 'Order Cancelled'
    };
    return titles[status] || 'Order Update';
  }
}
```

---

## 9. Error Handling Strategy

### 9.1 Error Hierarchy

```typescript
// Base application error
class AppError extends Error {
  constructor(
    public code: string,
    public message: string,
    public statusCode: number,
    public details?: Record<string, any>
  ) {
    super(message);
    this.name = this.constructor.name;
    Error.captureStackTrace(this, this.constructor);
  }

  toJSON() {
    return {
      error: {
        code: this.code,
        message: this.message,
        details: this.details
      }
    };
  }
}

// Specific error types
class ValidationError extends AppError {
  constructor(code: string, message: string, details?: Record<string, any>) {
    super(code, message, 400, details);
  }
}

class AuthenticationError extends AppError {
  constructor(code: string, message: string) {
    super(code, message, 401);
  }
}

class ForbiddenError extends AppError {
  constructor(code: string, message: string) {
    super(code, message, 403);
  }
}

class NotFoundError extends AppError {
  constructor(code: string, message: string) {
    super(code, message, 404);
  }
}

class PaymentError extends AppError {
  constructor(code: string, message: string, details?: Record<string, any>) {
    super(code, message, 402, details);
  }
}

class RateLimitError extends AppError {
  constructor(retryAfter: number) {
    super('RATE_LIMIT_EXCEEDED', 'Too many requests', 429, { retryAfter });
  }
}
```

### 9.2 Error Response Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Request validation failed",
    "details": {
      "fields": {
        "items[0].quantity": "Quantity must be between 1 and 10"
      }
    },
    "requestId": "req_abc123xyz",
    "timestamp": "2026-01-22T12:15:00.000Z"
  }
}
```

### 9.3 Global Error Handler

```typescript
import { Request, Response, NextFunction } from 'express';
import { logger } from '../utils/logger';

export function globalErrorHandler(
  error: Error,
  req: Request,
  res: Response,
  next: NextFunction
) {
  const requestId = req.headers['x-request-id'] as string;

  if (error instanceof AppError) {
    logger.warn('Application error', {
      requestId,
      code: error.code,
      message: error.message,
      statusCode: error.statusCode,
      path: req.path
    });

    return res.status(error.statusCode).json({
      error: {
        code: error.code,
        message: error.message,
        details: error.details,
        requestId,
        timestamp: new Date().toISOString()
      }
    });
  }

  // Unexpected error
  logger.error('Unexpected error', {
    requestId,
    error: error.message,
    stack: error.stack,
    path: req.path
  });

  return res.status(500).json({
    error: {
      code: 'INTERNAL_ERROR',
      message: 'An unexpected error occurred',
      requestId,
      timestamp: new Date().toISOString()
    }
  });
}
```

---

## 10. Logging and Monitoring

### 10.1 Logging Standards

**Log Format:**
```json
{
  "timestamp": "2026-01-22T12:15:00.000Z",
  "level": "info",
  "message": "Order created successfully",
  "service": "order-service",
  "version": "1.2.3",
  "requestId": "req_abc123xyz",
  "traceId": "trace_xyz789",
  "userId": "usr_abc123",
  "data": {
    "orderId": "ord_xyz789",
    "vendorId": "vnd_taco_tornado",
    "total": 14.07
  }
}
```

**Log Levels:**

| Level | Usage | Example |
|-------|-------|---------|
| ERROR | System failures, unhandled exceptions | Database connection failed |
| WARN | Recoverable issues, deprecations | Rate limit approaching |
| INFO | Business events, state changes | Order created, payment succeeded |
| DEBUG | Development diagnostics | Query parameters, cache hits |

### 10.2 Metrics Specifications

| Metric Name | Type | Labels | Description |
|-------------|------|--------|-------------|
| http_requests_total | Counter | method, path, status | Total HTTP requests |
| http_request_duration_seconds | Histogram | method, path | Request latency |
| orders_created_total | Counter | vendor_id, campus_id | Orders created |
| orders_status_changes_total | Counter | from_status, to_status | Order state transitions |
| payment_amount_total | Counter | status | Payment amounts processed |
| active_websocket_connections | Gauge | campus_id | WebSocket connections |
| location_update_latency_seconds | Histogram | - | GPS to client latency |

### 10.3 CloudWatch Dashboards

**Operations Dashboard:**
- Request rate by endpoint
- Error rate (4xx, 5xx)
- Latency percentiles (p50, p95, p99)
- Active orders by status
- WebSocket connections

**Business Dashboard:**
- Orders per hour
- Revenue by vendor
- Average order value
- Popular menu items
- User retention

### 10.4 Alerting Rules

| Alert | Condition | Severity | Response |
|-------|-----------|----------|----------|
| High Error Rate | 5xx > 1% for 5 min | Critical | Page on-call |
| High Latency | p95 > 1s for 5 min | High | Page on-call |
| Payment Failures | > 5% for 5 min | Critical | Page on-call |
| Database CPU | > 80% for 10 min | High | Notify team |
| Low Disk Space | < 20% free | Medium | Notify team |
| WebSocket Spike | > 5000 connections | Medium | Monitor |

---

## 11. Caching Strategy

### 11.1 Cache Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                  CACHING LAYERS                                      │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                      │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                          CDN CACHE (CloudFront)                              │   │
│  │                                                                              │   │
│  │  • Static assets (images, fonts)           TTL: 24 hours                    │   │
│  │  • Menu images                             TTL: 1 hour                      │   │
│  │  • Vendor logos                            TTL: 24 hours                    │   │
│  └─────────────────────────────────────────────────────────────────────────────┘   │
│                                       │                                              │
│                                       ▼                                              │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                       APPLICATION CACHE (Redis)                              │   │
│  │                                                                              │   │
│  │  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐             │   │
│  │  │   Menu Cache    │  │  Location Cache │  │  Session Cache  │             │   │
│  │  │                 │  │                 │  │                 │             │   │
│  │  │  TTL: 30 min    │  │  TTL: 30 sec    │  │  TTL: 1 hour    │             │   │
│  │  │  Invalidate on  │  │  Pub/sub        │  │  JWT sessions   │             │   │
│  │  │  menu update    │  │  updates        │  │                 │             │   │
│  │  └─────────────────┘  └─────────────────┘  └─────────────────┘             │   │
│  │                                                                              │   │
│  │  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐             │   │
│  │  │  Vendor Cache   │  │   User Cache    │  │  Rate Limits    │             │   │
│  │  │                 │  │                 │  │                 │             │   │
│  │  │  TTL: 15 min    │  │  TTL: 60 min    │  │  Sliding window │             │   │
│  │  │                 │  │                 │  │  counters       │             │   │
│  │  └─────────────────┘  └─────────────────┘  └─────────────────┘             │   │
│  └─────────────────────────────────────────────────────────────────────────────┘   │
│                                       │                                              │
│                                       ▼                                              │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                          DATABASE (PostgreSQL)                               │   │
│  │                                                                              │   │
│  │  • Query plan cache (automatic)                                             │   │
│  │  • Connection pooling (PgBouncer)                                           │   │
│  └─────────────────────────────────────────────────────────────────────────────┘   │
│                                                                                      │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

### 11.2 Cache Key Patterns

| Pattern | Example | TTL | Invalidation |
|---------|---------|-----|--------------|
| menu:{vendorId} | menu:vnd_taco_tornado | 30 min | Menu update webhook |
| vendor:{vendorId} | vendor:vnd_taco_tornado | 15 min | Vendor update |
| vendor:nearby:{campusId} | vendor:nearby:campus_sf_hq | 1 min | Location update |
| location:{truckId}:current | location:trk_tornado_1:current | 30 sec | GPS update |
| user:{userId}:profile | user:usr_abc123:profile | 60 min | Profile update |
| user:{userId}:favorites | user:usr_abc123:favorites | 30 min | Favorite add/remove |
| ratelimit:{userId}:{endpoint} | ratelimit:usr_abc123:orders | 1 min | Sliding window |

### 11.3 Cache Implementation

```typescript
import Redis from 'ioredis';

class CacheService {
  private redis: Redis;

  constructor() {
    this.redis = new Redis({
      host: process.env.REDIS_HOST,
      port: parseInt(process.env.REDIS_PORT || '6379'),
      password: process.env.REDIS_PASSWORD,
      tls: process.env.NODE_ENV === 'production' ? {} : undefined
    });
  }

  async get<T>(key: string): Promise<T | null> {
    const data = await this.redis.get(key);
    if (!data) return null;
    return JSON.parse(data) as T;
  }

  async set<T>(key: string, value: T, ttlSeconds: number): Promise<void> {
    await this.redis.setex(key, ttlSeconds, JSON.stringify(value));
  }

  async delete(key: string): Promise<void> {
    await this.redis.del(key);
  }

  async deletePattern(pattern: string): Promise<void> {
    const keys = await this.redis.keys(pattern);
    if (keys.length > 0) {
      await this.redis.del(...keys);
    }
  }

  // Cache-aside pattern helper
  async getOrSet<T>(
    key: string,
    fetcher: () => Promise<T>,
    ttlSeconds: number
  ): Promise<T> {
    const cached = await this.get<T>(key);
    if (cached) {
      return cached;
    }

    const data = await fetcher();
    await this.set(key, data, ttlSeconds);
    return data;
  }
}
```

---

## 12. Testing Strategy

### 12.1 Test Coverage Requirements

| Type | Coverage Target | Focus Areas |
|------|-----------------|-------------|
| Unit | 80% | Business logic, utilities, validators |
| Integration | 60% | API endpoints, database operations |
| E2E | Critical paths | Order flow, payment, authentication |

### 12.2 Unit Test Examples

```typescript
// Order service unit tests
describe('OrderService', () => {
  let orderService: OrderService;
  let mockOrderRepository: jest.Mocked<OrderRepository>;
  let mockPaymentService: jest.Mocked<PaymentService>;
  let mockVendorService: jest.Mocked<VendorService>;

  beforeEach(() => {
    mockOrderRepository = createMock<OrderRepository>();
    mockPaymentService = createMock<PaymentService>();
    mockVendorService = createMock<VendorService>();

    orderService = new OrderService(
      mockOrderRepository,
      mockPaymentService,
      mockVendorService,
      createMock<NotificationService>(),
      createMock<EventPublisher>(),
      createMock<CacheService>()
    );
  });

  describe('createOrder', () => {
    it('should create order successfully with valid items', async () => {
      // Arrange
      const userId = 'usr_abc123';
      const request: CreateOrderRequest = {
        vendorId: 'vnd_taco_tornado',
        truckId: 'trk_tornado_1',
        items: [
          { menuItemId: 'item_carnitas_taco', quantity: 2, customizations: [] }
        ],
        tipAmount: 2.00,
        useMealSubsidy: true,
        paymentMethodId: 'pm_stripe_abc123'
      };

      mockVendorService.getVendor.mockResolvedValue({
        id: 'vnd_taco_tornado',
        status: 'active',
        name: 'Taco Tornado'
      } as Vendor);

      mockVendorService.getMenuItem.mockResolvedValue({
        id: 'item_carnitas_taco',
        price: 4.50,
        available: true
      } as MenuItem);

      mockOrderRepository.create.mockResolvedValue({
        id: 'ord_xyz789',
        orderNumber: 'TT-2026-0001',
        status: OrderStatus.PENDING
      } as Order);

      mockPaymentService.createPaymentIntent.mockResolvedValue({
        id: 'pi_abc123',
        clientSecret: 'pi_abc123_secret_xyz',
        status: 'requires_confirmation'
      });

      // Act
      const result = await orderService.createOrder(userId, request);

      // Assert
      expect(result.id).toBe('ord_xyz789');
      expect(result.status).toBe(OrderStatus.PENDING);
      expect(mockVendorService.getVendor).toHaveBeenCalledWith('vnd_taco_tornado');
      expect(mockPaymentService.createPaymentIntent).toHaveBeenCalled();
    });

    it('should throw error when vendor is not active', async () => {
      // Arrange
      mockVendorService.getVendor.mockResolvedValue({
        id: 'vnd_taco_tornado',
        status: 'inactive'
      } as Vendor);

      // Act & Assert
      await expect(orderService.createOrder('usr_abc123', {
        vendorId: 'vnd_taco_tornado',
        truckId: 'trk_tornado_1',
        items: [],
        paymentMethodId: 'pm_123'
      })).rejects.toThrow(ValidationError);
    });
  });

  describe('cancelOrder', () => {
    it('should cancel order when within cancellation window', async () => {
      // Arrange
      const orderId = 'ord_xyz789';
      const userId = 'usr_abc123';

      mockOrderRepository.findById.mockResolvedValue({
        id: orderId,
        userId,
        status: OrderStatus.PENDING,
        pricing: { total: 10.00 }
      } as Order);

      mockPaymentService.refundPayment.mockResolvedValue({
        id: 'ref_123',
        amount: 10.00,
        status: 'pending'
      });

      // Act
      const result = await orderService.cancelOrder(orderId, userId, 'Changed mind');

      // Assert
      expect(result.status).toBe(OrderStatus.CANCELLED);
      expect(mockPaymentService.refundPayment).toHaveBeenCalled();
    });

    it('should throw error when order is already preparing', async () => {
      // Arrange
      mockOrderRepository.findById.mockResolvedValue({
        id: 'ord_xyz789',
        userId: 'usr_abc123',
        status: OrderStatus.PREPARING
      } as Order);

      // Act & Assert
      await expect(
        orderService.cancelOrder('ord_xyz789', 'usr_abc123', 'reason')
      ).rejects.toThrow(ValidationError);
    });
  });
});
```

### 12.3 Integration Test Examples

```typescript
// API integration tests
import request from 'supertest';
import { app } from '../src/app';
import { setupTestDatabase, teardownTestDatabase, seedTestData } from './helpers';

describe('Orders API Integration', () => {
  let testToken: string;
  let testUserId: string;

  beforeAll(async () => {
    await setupTestDatabase();
    const auth = await seedTestData();
    testToken = auth.token;
    testUserId = auth.userId;
  });

  afterAll(async () => {
    await teardownTestDatabase();
  });

  describe('POST /api/v1/orders', () => {
    it('should create order with valid request', async () => {
      // Arrange
      const orderRequest = {
        vendorId: 'vnd_test_vendor',
        truckId: 'trk_test_truck',
        items: [
          {
            menuItemId: 'item_test_taco',
            quantity: 2,
            customizations: []
          }
        ],
        tipAmount: 2.00,
        useMealSubsidy: false,
        paymentMethodId: 'pm_test_card'
      };

      // Act
      const response = await request(app)
        .post('/api/v1/orders')
        .set('Authorization', `Bearer ${testToken}`)
        .send(orderRequest);

      // Assert
      expect(response.status).toBe(201);
      expect(response.body.id).toBeDefined();
      expect(response.body.orderNumber).toMatch(/^TT-\d{4}-\d{4}$/);
      expect(response.body.status).toBe('pending');
      expect(response.body.payment.clientSecret).toBeDefined();
    });

    it('should return 401 without authentication', async () => {
      const response = await request(app)
        .post('/api/v1/orders')
        .send({});

      expect(response.status).toBe(401);
      expect(response.body.error.code).toBe('INVALID_TOKEN');
    });

    it('should return 400 with invalid item quantity', async () => {
      const response = await request(app)
        .post('/api/v1/orders')
        .set('Authorization', `Bearer ${testToken}`)
        .send({
          vendorId: 'vnd_test',
          truckId: 'trk_test',
          items: [{ menuItemId: 'item_test', quantity: 100 }],
          paymentMethodId: 'pm_test'
        });

      expect(response.status).toBe(400);
      expect(response.body.error.code).toBe('VALIDATION_ERROR');
    });
  });

  describe('GET /api/v1/orders/:id', () => {
    it('should return order details for owner', async () => {
      // First create an order
      const createResponse = await request(app)
        .post('/api/v1/orders')
        .set('Authorization', `Bearer ${testToken}`)
        .send({
          vendorId: 'vnd_test_vendor',
          truckId: 'trk_test_truck',
          items: [{ menuItemId: 'item_test_taco', quantity: 1 }],
          paymentMethodId: 'pm_test_card'
        });

      const orderId = createResponse.body.id;

      // Then fetch it
      const response = await request(app)
        .get(`/api/v1/orders/${orderId}`)
        .set('Authorization', `Bearer ${testToken}`);

      expect(response.status).toBe(200);
      expect(response.body.id).toBe(orderId);
    });
  });
});
```

### 12.4 E2E Test Scenarios

| Scenario | Steps | Expected Result |
|----------|-------|-----------------|
| Complete order flow | Login -> Browse -> Add to cart -> Checkout -> Pay -> Track | Order completed, notifications received |
| Order cancellation | Create order -> Cancel within window | Order cancelled, refund initiated |
| Dietary filtering | Set preferences -> View menu | Only safe items shown |
| Real-time tracking | Subscribe to campus -> Truck arrives | Location update received via WebSocket |
| Payment failure | Create order -> Payment fails | Error shown, order not created |

---

## 13. Deployment Strategy

### 13.1 CI/CD Pipeline

```yaml
# .github/workflows/deploy.yml
name: Deploy TacoTracker

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  AWS_REGION: us-west-2
  ECR_REPOSITORY: tacotracker
  ECS_CLUSTER: tacotracker-cluster

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: test
        ports:
          - 5432:5432
      redis:
        image: redis:7
        ports:
          - 6379:6379

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run type check
        run: npm run typecheck

      - name: Run unit tests
        run: npm run test:unit -- --coverage

      - name: Run integration tests
        run: npm run test:integration
        env:
          DATABASE_URL: postgresql://postgres:test@localhost:5432/test
          REDIS_URL: redis://localhost:6379

      - name: Upload coverage
        uses: codecov/codecov-action@v3

  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build and push Docker image
        env:
          ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          IMAGE_TAG: ${{ github.sha }}
        run: |
          docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
          docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG

  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    environment: staging

    steps:
      - name: Deploy to staging
        run: |
          aws ecs update-service \
            --cluster $ECS_CLUSTER-staging \
            --service tacotracker-api \
            --force-new-deployment

      - name: Wait for deployment
        run: |
          aws ecs wait services-stable \
            --cluster $ECS_CLUSTER-staging \
            --services tacotracker-api

      - name: Run smoke tests
        run: npm run test:smoke -- --env staging

  deploy-production:
    needs: deploy-staging
    runs-on: ubuntu-latest
    environment: production

    steps:
      - name: Deploy to production
        run: |
          aws ecs update-service \
            --cluster $ECS_CLUSTER-production \
            --service tacotracker-api \
            --force-new-deployment

      - name: Wait for deployment
        run: |
          aws ecs wait services-stable \
            --cluster $ECS_CLUSTER-production \
            --services tacotracker-api
```

### 13.2 Environment Configuration

| Variable | Development | Staging | Production | Secret |
|----------|-------------|---------|------------|--------|
| NODE_ENV | development | staging | production | No |
| DATABASE_URL | local postgres | RDS staging | RDS production | Yes |
| REDIS_URL | local redis | ElastiCache staging | ElastiCache prod | Yes |
| STRIPE_SECRET_KEY | sk_test_xxx | sk_test_xxx | sk_live_xxx | Yes |
| OKTA_ISSUER | dev.okta.com | globex-staging.okta.com | globex.okta.com | No |
| LOG_LEVEL | debug | info | info | No |

### 13.3 Deployment Checklist

**Pre-deployment:**
- [ ] All tests passing in CI
- [ ] Database migrations reviewed and tested
- [ ] Environment variables configured in Secrets Manager
- [ ] Feature flags set for gradual rollout
- [ ] Rollback plan documented

**Deployment:**
- [ ] Deploy to staging first
- [ ] Run smoke tests on staging
- [ ] Verify monitoring dashboards
- [ ] Deploy to production with canary (10%)
- [ ] Monitor error rates and latency
- [ ] Gradually increase to 100%

**Post-deployment:**
- [ ] Verify all health checks passing
- [ ] Check CloudWatch logs for errors
- [ ] Confirm alerting is working
- [ ] Update deployment documentation
- [ ] Notify stakeholders

---

## 14. Feature Flags

### 14.1 Feature Flag Definitions

| Flag Name | Type | Default | Description |
|-----------|------|---------|-------------|
| enable_preorders | Boolean | true | Enable pre-ordering functionality |
| enable_meal_subsidy | Boolean | true | Enable meal subsidy feature |
| enable_group_orders | Boolean | false | Enable group ordering (Phase 2) |
| new_checkout_flow | Percentage | 0 | New checkout UI rollout |
| enable_ratings | Boolean | false | Enable vendor ratings (Phase 2) |

### 14.2 Rollout Plan

| Phase | Audience | Duration | Success Criteria | Rollback Trigger |
|-------|----------|----------|------------------|------------------|
| 1 | Internal (Eng team) | 1 day | No critical errors | Any critical error |
| 2 | 10% employees | 3 days | Error rate < 0.5% | Error rate > 1% |
| 3 | 50% employees | 1 week | KPIs stable | KPIs degraded > 10% |
| 4 | 100% employees | - | Full rollout | - |

---

## 15. Security Implementation

### 15.1 Security Controls

| Control | Implementation | Validation |
|---------|----------------|------------|
| Input validation | Zod schemas on all endpoints | Unit tests, fuzz testing |
| SQL injection | Prisma ORM (parameterized queries) | Security scan, code review |
| XSS | React escaping, CSP headers | Security scan |
| CSRF | SameSite cookies, origin validation | Integration tests |
| Rate limiting | Redis sliding window | Load testing |

### 15.2 Sensitive Data Handling

| Data Type | Storage | Transmission | Access Control |
|-----------|---------|--------------|----------------|
| Passwords | N/A (Okta handles) | N/A | N/A |
| Payment cards | Stripe tokens only | TLS 1.3 | Payment service only |
| User PII | PostgreSQL (encrypted at rest) | TLS 1.3 | User service + owner |
| JWT tokens | Client-side only | TLS 1.3 | Per-user |
| API keys | AWS Secrets Manager | Environment injection | Service accounts |

### 15.3 Security Audit Checklist

- [x] OWASP Top 10 addressed
- [x] Dependency vulnerabilities scanned (npm audit)
- [x] Secrets not in code (verified via git-secrets)
- [x] Access logging enabled (CloudWatch)
- [x] Rate limiting implemented
- [x] Input validation on all endpoints
- [x] SQL injection prevention (ORM)
- [x] XSS prevention (React + CSP)
- [ ] Penetration test scheduled (Q1 2026)
- [ ] Security review by Globex IT (scheduled)

---

## Appendices

### Appendix A: Glossary

| Term | Definition |
|------|------------|
| BRD | Business Requirements Document |
| PRD | Product Requirements Document |
| TDD | Technical Design Document |
| ARCH | Architecture Document |
| OIDC | OpenID Connect - authentication protocol |
| JWT | JSON Web Token - compact token format |
| PKCE | Proof Key for Code Exchange - OAuth security extension |
| ORM | Object-Relational Mapping |
| DTO | Data Transfer Object |
| TTL | Time to Live - cache expiration |

### Appendix B: References

**Internal Documents:**
- [BRD-example.md](./BRD-example.md) - Business Requirements
- [PRD-example.md](./PRD-example.md) - Product Requirements
- [UI-UX-example.md](./UI-UX-example.md) - Design System
- [ARCH-example.md](./ARCH-example.md) - Architecture

**External Documentation:**
- [Stripe API](https://stripe.com/docs/api)
- [Okta Developer](https://developer.okta.com/)
- [Firebase FCM](https://firebase.google.com/docs/cloud-messaging)
- [Prisma ORM](https://www.prisma.io/docs)
- [Socket.io](https://socket.io/docs/v4/)

### Appendix C: Change Log

| Date | Change | Author |
|------|--------|--------|
| 2026-01-05 | Initial draft | Software Architect |
| 2026-01-10 | Added API specifications | Software Architect |
| 2026-01-15 | Database schema finalized | Senior Architect |
| 2026-01-18 | Security review updates | Security Team |
| 2026-01-22 | Final approval version | Senior Architect |

---

*Document End - TacoTracker 3000 Technical Design Document v1.0*

**Document Classification:** Internal Use Only
**Last Review:** January 22, 2026
**Next Review:** April 2026
