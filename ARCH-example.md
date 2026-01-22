# TacoTracker 3000 - High-Level Architecture Document

| Document Information |                                         |
|---------------------|-----------------------------------------|
| Version             | 1.0                                     |
| Status              | Approved                                |
| Author              | Principal Solutions Architect           |
| Last Updated        | 2024-01-15                              |
| Approved By         | CTO, VP Engineering - 2024-01-20        |

## Document History

| Version | Date       | Author                  | Changes                              |
|---------|------------|-------------------------|--------------------------------------|
| 0.1     | 2024-01-02 | Solutions Architect     | Initial draft                        |
| 0.5     | 2024-01-08 | Solutions Architect     | Incorporated stakeholder feedback    |
| 0.9     | 2024-01-12 | Solutions Architect     | Security review updates              |
| 1.0     | 2024-01-15 | Principal Architect     | Final approval version               |

---

## 1. Executive Summary

### System Overview

TacoTracker 3000 is an enterprise-grade taco truck tracking and ordering platform designed to serve Globex Corporation's 50,000+ employees across 12 North American campus locations. The system enables employees to locate nearby taco trucks in real-time, place mobile orders for pickup, and provides vendors with comprehensive fleet management tools.

### Key Business Capabilities

- **Real-time Location Tracking**: GPS-enabled tracking of all taco truck locations with sub-minute update frequency
- **Mobile Ordering**: Native iOS/Android apps enabling employees to browse menus, place orders, and pay seamlessly
- **Vendor Management**: Web portal for taco truck operators to manage menus, view orders, and track revenue
- **Enterprise Administration**: Centralized dashboard for Globex food services to manage vendors, analyze trends, and ensure compliance
- **Payment Processing**: Secure, PCI-compliant payment processing with support for corporate meal subsidies

### High-Level Technology Choices

| Layer | Technology | Rationale |
|-------|------------|-----------|
| Mobile Apps | React Native | Cross-platform efficiency, code reuse |
| Web Frontend | React + TypeScript | Component reusability, type safety |
| Backend API | Node.js + Express | High concurrency, JavaScript ecosystem |
| Database | PostgreSQL + Redis | ACID compliance, caching performance |
| Real-time | Socket.io + Redis Pub/Sub | Low-latency location updates |
| Cloud | AWS | Enterprise agreement, mature services |
| Auth | Okta SSO | Corporate identity integration |
| Payments | Stripe | PCI compliance, developer experience |

### Critical Success Factors

1. Sub-second location updates on mobile devices
2. 99.9% platform availability during business hours (6 AM - 8 PM local)
3. Order placement to confirmation in under 3 seconds
4. Zero payment data breaches
5. Support for 10,000 concurrent users during lunch peak

### Major Risks and Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| GPS accuracy in urban canyons | Medium | Hybrid location (GPS + WiFi + Cell triangulation) |
| Payment system downtime | High | Multi-PSP failover, offline order queue |
| Peak load during lunch hours | High | Auto-scaling, CDN caching, request queuing |
| Vendor app adoption | Medium | Simplified onboarding, training program |

### Estimated Infrastructure Costs

| Environment | Monthly Cost (USD) |
|-------------|-------------------|
| Development | $2,500 - $3,500 |
| Staging | $4,000 - $5,000 |
| Production | $15,000 - $25,000 |
| **Total** | **$21,500 - $33,500** |

*Costs scale with usage; estimates based on 50,000 active users and 100 active vendors.*

---

## 2. Architecture Overview and Principles

### 2.1 Architecture Vision

TacoTracker 3000 will be a cloud-native, event-driven platform that delivers exceptional user experiences through real-time capabilities while maintaining enterprise-grade security, reliability, and scalability. The architecture prioritizes mobile-first design, operational simplicity, and the ability to evolve rapidly as business needs change.

### 2.2 Guiding Principles

#### Principle 1: API-First Design

**Statement**: All functionality is exposed through well-defined APIs before any UI development begins.

**Rationale**: TacoTracker serves multiple clients (iOS, Android, Web Portal, Admin Dashboard) that all require consistent access to business logic. API-first ensures all clients receive equal capabilities and enables future integrations.

**Implications**:
- OpenAPI specifications created before implementation
- API versioning strategy required from day one
- Backend teams cannot depend on specific frontend behaviors

#### Principle 2: Security by Default

**Statement**: Security controls are built into every layer; insecure options require explicit override.

**Rationale**: TacoTracker handles payment information, personal data, and corporate authentication. A breach would damage Globex's reputation and violate compliance requirements.

**Implications**:
- All data encrypted in transit (TLS 1.3) and at rest (AES-256)
- No direct database access; all queries through parameterized APIs
- Security scanning in CI/CD pipeline
- Principle of least privilege for all service accounts

#### Principle 3: Event-Driven Communication

**Statement**: Services communicate asynchronously through events where synchronous communication is not required.

**Rationale**: Real-time location tracking, order status updates, and notifications are inherently event-driven. Asynchronous patterns improve scalability and fault tolerance.

**Implications**:
- Message broker (AWS SNS/SQS) for inter-service communication
- Event sourcing for order lifecycle
- Idempotent event handlers required
- Eventual consistency accepted for non-critical paths

#### Principle 4: Observable Systems

**Statement**: Every service must emit structured logs, metrics, and traces; observability is not optional.

**Rationale**: Distributed systems are complex to debug. Comprehensive observability enables rapid incident response and performance optimization.

**Implications**:
- Standardized logging format (JSON structured logs)
- Distributed tracing with correlation IDs
- Custom business metrics alongside technical metrics
- Dashboards and alerts for all services

#### Principle 5: Graceful Degradation

**Statement**: System components must handle failures gracefully, providing reduced functionality rather than complete outage.

**Rationale**: External dependencies (GPS, payment processor, push notifications) will occasionally fail. Users should still be able to use core functionality.

**Implications**:
- Circuit breakers on all external calls
- Fallback behaviors defined for each integration
- Cached data served when real-time unavailable
- Clear user messaging when features are degraded

### 2.3 Architecture Characteristics

| Characteristic | Priority | Target | Rationale |
|---------------|----------|--------|-----------|
| Availability | Critical | 99.9% during business hours | Revenue impact of downtime |
| Scalability | High | 10x current load | Support campus expansion |
| Performance | High | p95 < 200ms API response | User experience expectations |
| Security | Critical | Zero breaches | PCI compliance, corporate data |
| Maintainability | Medium | <4 hour mean time to deploy | Rapid iteration needs |
| Cost Efficiency | Medium | <$0.50 per active user/month | Budget constraints |

---

## 3. System Context (C4 Level 1)

### 3.1 Context Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                                    EXTERNAL ACTORS                                       │
└─────────────────────────────────────────────────────────────────────────────────────────┘

    ┌─────────────┐      ┌─────────────┐      ┌─────────────┐      ┌─────────────┐
    │   Globex    │      │    Taco     │      │   Globex    │      │   Globex    │
    │  Employee   │      │   Truck     │      │    Food     │      │     IT      │
    │   (User)    │      │   Vendor    │      │  Services   │      │   Admin     │
    └──────┬──────┘      └──────┬──────┘      └──────┬──────┘      └──────┬──────┘
           │                    │                    │                    │
           │ Browse, Order      │ Manage Fleet       │ Oversight          │ System Config
           │ Track, Pay         │ View Orders        │ Analytics          │ User Management
           │                    │ Update Menu        │ Compliance         │
           ▼                    ▼                    ▼                    ▼
    ╔═════════════════════════════════════════════════════════════════════════════════╗
    ║                                                                                  ║
    ║                           TACOTRACKER 3000 PLATFORM                              ║
    ║                                                                                  ║
    ║   ┌─────────────────────────────────────────────────────────────────────────┐   ║
    ║   │  • Real-time taco truck tracking                                        │   ║
    ║   │  • Mobile ordering and payments                                         │   ║
    ║   │  • Vendor fleet management                                              │   ║
    ║   │  • Enterprise analytics and administration                              │   ║
    ║   └─────────────────────────────────────────────────────────────────────────┘   ║
    ║                                                                                  ║
    ╚═══════════════════════════════════╤══════════════════════════════════════════════╝
                                        │
           ┌────────────────────────────┼────────────────────────────┐
           │                            │                            │
           ▼                            ▼                            ▼
    ┌─────────────┐              ┌─────────────┐              ┌─────────────┐
    │    Okta     │              │   Stripe    │              │  Firebase   │
    │    SSO      │              │  Payments   │              │    FCM      │
    │             │              │             │              │             │
    │ [Identity]  │              │ [Payments]  │              │   [Push     │
    │             │              │             │              │ Notifications]│
    └─────────────┘              └─────────────┘              └─────────────┘

           ┌────────────────────────────┬────────────────────────────┐
           │                            │                            │
           ▼                            ▼                            ▼
    ┌─────────────┐              ┌─────────────┐              ┌─────────────┐
    │   Google    │              │    AWS      │              │  Globex     │
    │    Maps     │              │    SES      │              │    HR       │
    │   Platform  │              │             │              │   System    │
    │             │              │  [Email     │              │             │
    │ [Mapping &  │              │  Service]   │              │ [Employee   │
    │  Geocoding] │              │             │              │    Data]    │
    └─────────────┘              └─────────────┘              └─────────────┘
```

### 3.2 External Actors

| Actor | Type | Description | Interaction |
|-------|------|-------------|-------------|
| Globex Employee | Human User | 50,000+ employees across 12 campuses who order from taco trucks | Mobile app for browsing, ordering, tracking, and payment |
| Taco Truck Vendor | Human User | ~100 licensed taco truck operators | Web portal for fleet management, order fulfillment, menu updates |
| Globex Food Services | Human User | Corporate food services team (10 users) | Admin dashboard for vendor management, analytics, compliance |
| Globex IT Admin | Human User | IT administrators (5 users) | System configuration, user provisioning, security management |

### 3.3 External System Integrations

| System | Purpose | Protocol | Data Exchanged | SLA |
|--------|---------|----------|----------------|-----|
| Okta SSO | Employee authentication and authorization | OIDC/SAML 2.0 | Identity tokens, user attributes, group memberships | 99.99% |
| Stripe | Payment processing | REST API | Payment intents, charges, refunds, webhooks | 99.99% |
| Firebase FCM | Push notifications | REST API + SDK | Notification payloads, device tokens | 99.95% |
| Google Maps Platform | Mapping, geocoding, directions | REST API + SDK | Coordinates, addresses, routes, ETAs | 99.9% |
| AWS SES | Transactional email | SMTP/API | Order confirmations, receipts, alerts | 99.9% |
| Globex HR System | Employee data synchronization | REST API (internal) | Employee records, department info, meal subsidies | 99.5% |

---

## 4. Container Architecture (C4 Level 2)

### 4.1 Container Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                                      CLIENT TIER                                             │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                              │
│  ┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐ │
│  │   iOS Mobile     │   │  Android Mobile  │   │   Vendor Web     │   │   Admin Web      │ │
│  │      App         │   │      App         │   │     Portal       │   │    Dashboard     │ │
│  │                  │   │                  │   │                  │   │                  │ │
│  │  React Native    │   │  React Native    │   │  React + TS      │   │  React + TS      │ │
│  │  + Native GPS    │   │  + Native GPS    │   │  + Material UI   │   │  + Ant Design    │ │
│  └────────┬─────────┘   └────────┬─────────┘   └────────┬─────────┘   └────────┬─────────┘ │
│           │                      │                      │                      │            │
└───────────┼──────────────────────┼──────────────────────┼──────────────────────┼────────────┘
            │                      │                      │                      │
            └──────────────────────┴──────────┬───────────┴──────────────────────┘
                                              │
                                              ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    EDGE / DELIVERY TIER                                      │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                              │
│  ┌──────────────────────────────┐              ┌──────────────────────────────────────────┐ │
│  │      CloudFront CDN          │              │         AWS WAF + Shield                 │ │
│  │                              │              │                                          │ │
│  │  • Static asset caching      │              │  • DDoS protection                       │ │
│  │  • Global edge locations     │◄────────────►│  • SQL injection prevention              │ │
│  │  • SSL termination           │              │  • Rate limiting                         │ │
│  └──────────────┬───────────────┘              └──────────────────────────────────────────┘ │
│                 │                                                                            │
└─────────────────┼────────────────────────────────────────────────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    APPLICATION TIER                                          │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────────────────┐   │
│  │                           Application Load Balancer                                  │   │
│  │                         (Path-based routing, health checks)                          │   │
│  └───────────┬─────────────────────┬─────────────────────┬─────────────────────────────┘   │
│              │                     │                     │                                   │
│              ▼                     ▼                     ▼                                   │
│  ┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐   │
│  │    API Gateway    │ │  Location Service │ │   Order Service   │ │   User Service    │   │
│  │                   │ │                   │ │                   │ │                   │   │
│  │  Node.js/Express  │ │  Node.js/Express  │ │  Node.js/Express  │ │  Node.js/Express  │   │
│  │  • Auth middleware│ │  • GPS processing │ │  • Order workflow │ │  • Profile mgmt   │   │
│  │  • Rate limiting  │ │  • Geofencing     │ │  • State machine  │ │  • Preferences    │   │
│  │  • Request routing│ │  • WebSocket hub  │ │  • Event emitter  │ │  • Favorites      │   │
│  │                   │ │                   │ │                   │ │                   │   │
│  │  ECS Fargate      │ │  ECS Fargate      │ │  ECS Fargate      │ │  ECS Fargate      │   │
│  │  2-10 tasks       │ │  3-15 tasks       │ │  2-10 tasks       │ │  2-6 tasks        │   │
│  └─────────┬─────────┘ └─────────┬─────────┘ └─────────┬─────────┘ └─────────┬─────────┘   │
│            │                     │                     │                     │              │
│  ┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐   │
│  │  Vendor Service   │ │ Payment Service   │ │Notification Svc   │ │  Analytics Svc    │   │
│  │                   │ │                   │ │                   │ │                   │   │
│  │  Node.js/Express  │ │  Node.js/Express  │ │  Node.js/Express  │ │  Node.js/Express  │   │
│  │  • Menu mgmt      │ │  • Stripe integ   │ │  • FCM push       │ │  • Event ingest   │   │
│  │  • Fleet tracking │ │  • Refunds        │ │  • Email (SES)    │ │  • Aggregation    │   │
│  │  • Revenue report │ │  • Meal subsidies │ │  • In-app alerts  │ │  • Reporting      │   │
│  │                   │ │                   │ │                   │ │                   │   │
│  │  ECS Fargate      │ │  ECS Fargate      │ │  ECS Fargate      │ │  ECS Fargate      │   │
│  │  2-6 tasks        │ │  2-8 tasks        │ │  2-6 tasks        │ │  2-4 tasks        │   │
│  └─────────┬─────────┘ └─────────┬─────────┘ └─────────┬─────────┘ └─────────┬─────────┘   │
│            │                     │                     │                     │              │
└────────────┼─────────────────────┼─────────────────────┼─────────────────────┼──────────────┘
             │                     │                     │                     │
             └─────────────────────┴──────────┬──────────┴─────────────────────┘
                                              │
                                              ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    MESSAGING TIER                                            │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                              │
│  ┌──────────────────────────────┐              ┌──────────────────────────────────────────┐ │
│  │        Amazon SNS            │              │           Amazon SQS                     │ │
│  │                              │              │                                          │ │
│  │  Topics:                     │              │  Queues:                                 │ │
│  │  • order-events              │─────────────►│  • order-processing-queue                │ │
│  │  • location-updates          │              │  • notification-queue                    │ │
│  │  • payment-events            │              │  • analytics-queue                       │ │
│  │  • vendor-events             │              │  • payment-webhook-queue                 │ │
│  └──────────────────────────────┘              │  • location-batch-queue (FIFO)           │ │
│                                                └──────────────────────────────────────────┘ │
│                                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────────────────────┐   │
│  │                              Amazon ElastiCache (Redis)                               │   │
│  │                                                                                       │   │
│  │  • Real-time location pub/sub          • Session storage                             │   │
│  │  • API response caching                 • Rate limiting counters                      │   │
│  │  • WebSocket connection state           • Distributed locks                           │   │
│  │                                                                                       │   │
│  │  Redis Cluster: 3 shards, 2 replicas per shard (r6g.large)                           │   │
│  └──────────────────────────────────────────────────────────────────────────────────────┘   │
│                                                                                              │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
                                              │
                                              ▼
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                                      DATA TIER                                               │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────────────────────┐   │
│  │                              Amazon RDS PostgreSQL                                   │   │
│  │                                                                                       │   │
│  │  Primary: db.r6g.xlarge (Multi-AZ)      Read Replicas: 2x db.r6g.large              │   │
│  │                                                                                       │   │
│  │  Databases:                                                                          │   │
│  │  • tacotracker_main    - Core business data (users, orders, vendors)                │   │
│  │  • tacotracker_geo     - Location data with PostGIS extension                       │   │
│  │  • tacotracker_analytics - Aggregated analytics (partitioned by date)               │   │
│  └─────────────────────────────────────────────────────────────────────────────────────┘   │
│                                                                                              │
│  ┌──────────────────────────────┐              ┌──────────────────────────────────────────┐ │
│  │       Amazon S3              │              │        Amazon DynamoDB                   │ │
│  │                              │              │                                          │ │
│  │  Buckets:                    │              │  Tables:                                 │ │
│  │  • tacotracker-assets        │              │  • location-history (time-series)        │ │
│  │  • tacotracker-uploads       │              │  • user-sessions                         │ │
│  │  • tacotracker-backups       │              │  • device-tokens                         │ │
│  │  • tacotracker-logs          │              │  • feature-flags                         │ │
│  └──────────────────────────────┘              └──────────────────────────────────────────┘ │
│                                                                                              │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Container Descriptions

| Container | Technology | Purpose | Scaling Strategy |
|-----------|------------|---------|------------------|
| iOS/Android Mobile App | React Native 0.72 | Employee-facing app for browsing, ordering, tracking | N/A (client-side) |
| Vendor Web Portal | React 18, TypeScript, Material UI | Vendor dashboard for fleet and order management | Static hosting via CloudFront |
| Admin Dashboard | React 18, TypeScript, Ant Design | Globex admin interface for oversight and analytics | Static hosting via CloudFront |
| API Gateway | Node.js 20, Express 4.18 | Central entry point, auth, rate limiting, routing | ECS auto-scaling (2-10 tasks) |
| Location Service | Node.js 20, Express, Socket.io | GPS processing, real-time location streaming | ECS auto-scaling (3-15 tasks) |
| Order Service | Node.js 20, Express | Order lifecycle, state machine, event emission | ECS auto-scaling (2-10 tasks) |
| User Service | Node.js 20, Express | User profiles, preferences, favorites | ECS auto-scaling (2-6 tasks) |
| Vendor Service | Node.js 20, Express | Menu management, fleet tracking, vendor profiles | ECS auto-scaling (2-6 tasks) |
| Payment Service | Node.js 20, Express | Stripe integration, refunds, meal subsidies | ECS auto-scaling (2-8 tasks) |
| Notification Service | Node.js 20, Express | Push notifications, email, in-app alerts | ECS auto-scaling (2-6 tasks) |
| Analytics Service | Node.js 20, Express | Event ingestion, aggregation, reporting | ECS auto-scaling (2-4 tasks) |
| PostgreSQL | Amazon RDS PostgreSQL 15 | Primary relational data store | Multi-AZ + Read Replicas |
| Redis | Amazon ElastiCache 7.0 | Caching, pub/sub, session storage | Redis Cluster (3 shards) |
| DynamoDB | AWS DynamoDB | Time-series location data, sessions | On-demand capacity |

### 4.3 Container Interactions

| From | To | Protocol | Purpose | Sync/Async |
|------|-----|----------|---------|------------|
| Mobile Apps | API Gateway | HTTPS/REST | All API requests | Sync |
| Mobile Apps | Location Service | WSS | Real-time location updates | Async |
| Web Portals | API Gateway | HTTPS/REST | All API requests | Sync |
| API Gateway | All Services | HTTP/REST | Request routing | Sync |
| Location Service | Redis | Redis Protocol | Location pub/sub | Async |
| Order Service | SNS | HTTPS | Order event publishing | Async |
| Order Service | Payment Service | HTTP/REST | Payment initiation | Sync |
| Payment Service | Stripe | HTTPS/REST | Payment processing | Sync |
| Notification Service | FCM | HTTPS | Push notification delivery | Async |
| Notification Service | SES | HTTPS | Email delivery | Async |
| All Services | PostgreSQL | TCP/5432 | Data persistence | Sync |
| All Services | Redis | TCP/6379 | Caching, sessions | Sync |
| Analytics Service | SQS | HTTPS | Event consumption | Async |

---

## 5. Component Architecture (C4 Level 3)

### 5.1 Order Service Components

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                  ORDER SERVICE                                       │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                      │
│  ┌────────────────────┐    ┌────────────────────┐    ┌────────────────────┐        │
│  │   Order Controller │    │   Cart Controller  │    │  History Controller│        │
│  │                    │    │                    │    │                    │        │
│  │  POST /orders      │    │  GET /cart         │    │  GET /orders       │        │
│  │  GET /orders/:id   │    │  POST /cart/items  │    │  GET /orders/:id   │        │
│  │  PATCH /orders/:id │    │  DELETE /cart/items│    │    /receipt        │        │
│  │  POST /orders/:id/ │    │  POST /cart/       │    │                    │        │
│  │       cancel       │    │       checkout     │    │                    │        │
│  └─────────┬──────────┘    └─────────┬──────────┘    └─────────┬──────────┘        │
│            │                         │                         │                    │
│            └─────────────────────────┴─────────────────────────┘                    │
│                                      │                                              │
│                                      ▼                                              │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │                            Order Domain Layer                                 │  │
│  ├──────────────────────────────────────────────────────────────────────────────┤  │
│  │                                                                               │  │
│  │  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐           │  │
│  │  │   Order Entity   │  │    Cart Entity   │  │  OrderItem Entity│           │  │
│  │  │                  │  │                  │  │                  │           │  │
│  │  │  - id            │  │  - id            │  │  - id            │           │  │
│  │  │  - userId        │  │  - userId        │  │  - orderId       │           │  │
│  │  │  - vendorId      │  │  - items[]       │  │  - menuItemId    │           │  │
│  │  │  - status        │  │  - vendorId      │  │  - quantity      │           │  │
│  │  │  - items[]       │  │  - createdAt     │  │  - price         │           │  │
│  │  │  - total         │  │  - expiresAt     │  │  - customizations│           │  │
│  │  │  - paymentId     │  │                  │  │                  │           │  │
│  │  │  - timestamps    │  │                  │  │                  │           │  │
│  │  └──────────────────┘  └──────────────────┘  └──────────────────┘           │  │
│  │                                                                               │  │
│  │  ┌───────────────────────────────────────────────────────────────────────┐   │  │
│  │  │                      Order State Machine                               │   │  │
│  │  │                                                                        │   │  │
│  │  │   ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐           │   │  │
│  │  │   │ PENDING │───►│CONFIRMED│───►│PREPARING│───►│  READY  │           │   │  │
│  │  │   └────┬────┘    └────┬────┘    └────┬────┘    └────┬────┘           │   │  │
│  │  │        │              │              │              │                 │   │  │
│  │  │        ▼              ▼              ▼              ▼                 │   │  │
│  │  │   ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐           │   │  │
│  │  │   │CANCELLED│    │CANCELLED│    │CANCELLED│    │ PICKED  │           │   │  │
│  │  │   └─────────┘    └─────────┘    └─────────┘    │   UP    │           │   │  │
│  │  │                                                └────┬────┘           │   │  │
│  │  │                                                     │                 │   │  │
│  │  │                                                     ▼                 │   │  │
│  │  │                                                ┌─────────┐           │   │  │
│  │  │                                                │COMPLETED│           │   │  │
│  │  │                                                └─────────┘           │   │  │
│  │  └───────────────────────────────────────────────────────────────────────┘   │  │
│  └──────────────────────────────────────────────────────────────────────────────┘  │
│                                      │                                              │
│                                      ▼                                              │
│  ┌──────────────────────────────────────────────────────────────────────────────┐  │
│  │                          Infrastructure Layer                                 │  │
│  ├──────────────────────────────────────────────────────────────────────────────┤  │
│  │                                                                               │  │
│  │  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐           │  │
│  │  │ Order Repository │  │  Event Publisher │  │  Payment Client  │           │  │
│  │  │                  │  │                  │  │                  │           │  │
│  │  │  PostgreSQL      │  │  SNS Topics:     │  │  HTTP Client to  │           │  │
│  │  │  - orders table  │  │  - order-events  │  │  Payment Service │           │  │
│  │  │  - order_items   │  │                  │  │                  │           │  │
│  │  │  - carts table   │  │  Events:         │  │  Methods:        │           │  │
│  │  │                  │  │  - OrderCreated  │  │  - initiatePayment│          │  │
│  │  │                  │  │  - OrderUpdated  │  │  - refundPayment │           │  │
│  │  │                  │  │  - OrderCancelled│  │  - getPaymentStatus│         │  │
│  │  └──────────────────┘  └──────────────────┘  └──────────────────┘           │  │
│  │                                                                               │  │
│  │  ┌──────────────────┐  ┌──────────────────┐                                  │  │
│  │  │   Vendor Client  │  │  Cache Manager   │                                  │  │
│  │  │                  │  │                  │                                  │  │
│  │  │  HTTP Client to  │  │  Redis           │                                  │  │
│  │  │  Vendor Service  │  │  - menu cache    │                                  │  │
│  │  │                  │  │  - vendor hours  │                                  │  │
│  │  │  Methods:        │  │  - price cache   │                                  │  │
│  │  │  - getVendor     │  │                  │                                  │  │
│  │  │  - getMenu       │  │                  │                                  │  │
│  │  │  - validateItem  │  │                  │                                  │  │
│  │  └──────────────────┘  └──────────────────┘                                  │  │
│  └──────────────────────────────────────────────────────────────────────────────┘  │
│                                                                                      │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

### 5.2 Location Service Components

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                LOCATION SERVICE                                      │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                      │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                           WebSocket Gateway                                  │   │
│  │                                                                              │   │
│  │  • Connection management (authenticate, subscribe, unsubscribe)             │   │
│  │  • Room-based broadcasting (campus rooms, vendor-specific rooms)            │   │
│  │  • Heartbeat/ping-pong for connection health                                │   │
│  │  • Automatic reconnection handling                                          │   │
│  └─────────────────────────────────────────────────────────────────────────────┘   │
│                                      │                                              │
│         ┌────────────────────────────┼────────────────────────────────┐            │
│         │                            │                                │            │
│         ▼                            ▼                                ▼            │
│  ┌─────────────────┐   ┌──────────────────────┐   ┌──────────────────────┐        │
│  │ Location Ingest │   │  Location Broadcast  │   │  Geofence Manager    │        │
│  │                 │   │                      │   │                      │        │
│  │ • Receive GPS   │   │ • Redis Pub/Sub      │   │ • Campus boundaries  │        │
│  │ • Validate data │   │ • Room management    │   │ • Entry/exit events  │        │
│  │ • Rate limiting │   │ • Selective push     │   │ • Proximity alerts   │        │
│  │ • Batch writes  │   │ • Compression        │   │ • Arrival detection  │        │
│  └────────┬────────┘   └──────────┬───────────┘   └──────────┬───────────┘        │
│           │                       │                          │                     │
│           └───────────────────────┴──────────────────────────┘                     │
│                                   │                                                 │
│                                   ▼                                                 │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                          Location Store                                      │   │
│  │                                                                              │   │
│  │  ┌─────────────────────┐    ┌─────────────────────────────────────────┐    │   │
│  │  │    Current State    │    │           Historical Data               │    │   │
│  │  │      (Redis)        │    │          (DynamoDB + S3)                │    │   │
│  │  │                     │    │                                          │    │   │
│  │  │  • Latest position  │    │  • Time-series location data            │    │   │
│  │  │  • Connection state │    │  • Route reconstruction                 │    │   │
│  │  │  • Active vendors   │    │  • Analytics aggregation                │    │   │
│  │  │  • Subscriber list  │    │  • Compliance audit trail               │    │   │
│  │  └─────────────────────┘    └─────────────────────────────────────────┘    │   │
│  └─────────────────────────────────────────────────────────────────────────────┘   │
│                                                                                      │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

### 5.3 Component Catalog

| Component | Responsibility | Dependencies | Interfaces |
|-----------|---------------|--------------|------------|
| Order Controller | HTTP request handling for orders | Order Domain, Auth Middleware | REST API |
| Cart Controller | Shopping cart operations | Cart Entity, Vendor Client | REST API |
| Order State Machine | Manage order lifecycle transitions | Event Publisher | Internal |
| Order Repository | Data persistence for orders | PostgreSQL | Internal |
| Event Publisher | Publish domain events | SNS | Internal |
| WebSocket Gateway | Real-time connection management | Socket.io, Redis | WebSocket |
| Location Ingest | Process incoming GPS data | Redis, DynamoDB | Internal |
| Location Broadcast | Fan-out location updates | Redis Pub/Sub | WebSocket |
| Geofence Manager | Spatial event detection | PostGIS, Redis | Internal |

---

## 6. Data Architecture

### 6.1 Data Model Overview

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                                   CORE DOMAIN MODEL                                          │
└─────────────────────────────────────────────────────────────────────────────────────────────┘

┌──────────────────┐         ┌──────────────────┐         ┌──────────────────┐
│      User        │         │      Vendor      │         │      Truck       │
├──────────────────┤         ├──────────────────┤         ├──────────────────┤
│ id (PK)          │         │ id (PK)          │         │ id (PK)          │
│ okta_id (UK)     │         │ name             │◄────────┤ vendor_id (FK)   │
│ email (UK)       │         │ description      │    1:N  │ license_plate    │
│ first_name       │         │ logo_url         │         │ vehicle_type     │
│ last_name        │         │ phone            │         │ capacity         │
│ department       │         │ tax_id           │         │ status           │
│ campus_id (FK)   │         │ status           │         │ current_driver   │
│ meal_subsidy     │         │ rating           │         │ created_at       │
│ preferences      │         │ created_at       │         └────────┬─────────┘
│ created_at       │         └────────┬─────────┘                  │
└────────┬─────────┘                  │                            │
         │                            │                            │
         │                   ┌────────┴─────────┐                  │
         │                   │                  │                  │
         │              ┌────┴───────┐    ┌─────┴──────┐          │
         │              │    Menu    │    │  Schedule  │          │
         │              ├────────────┤    ├────────────┤          │
         │              │ id (PK)    │    │ id (PK)    │          │
         │              │ vendor_id  │    │ vendor_id  │          │
         │              │ name       │    │ truck_id   │          │
         │              │ active     │    │ campus_id  │          │
         │              │ created_at │    │ day_of_week│          │
         │              └────┬───────┘    │ start_time │          │
         │                   │            │ end_time   │          │
         │              ┌────┴───────┐    └────────────┘          │
         │              │  MenuItem  │                            │
         │              ├────────────┤                            │
         │              │ id (PK)    │                            │
         │              │ menu_id    │                            │
         │              │ name       │         ┌──────────────────┴──────────────────┐
         │              │ description│         │              Location               │
         │              │ price      │         ├─────────────────────────────────────┤
         │              │ category   │         │ truck_id (PK, FK)                   │
         │              │ image_url  │         │ timestamp (PK)                      │
         │              │ available  │         │ latitude                            │
         │              │ prep_time  │         │ longitude                           │
         │              └────────────┘         │ speed                               │
         │                                     │ heading                             │
         │                                     │ accuracy                            │
         │                                     │ source (GPS/WiFi/Cell)              │
         │                                     └─────────────────────────────────────┘
         │
         │         ┌──────────────────┐         ┌──────────────────┐
         │         │      Order       │         │    OrderItem     │
         │    1:N  ├──────────────────┤    1:N  ├──────────────────┤
         └────────►│ id (PK)          │────────►│ id (PK)          │
                   │ user_id (FK)     │         │ order_id (FK)    │
                   │ vendor_id (FK)   │         │ menu_item_id (FK)│
                   │ truck_id (FK)    │         │ quantity         │
                   │ status           │         │ unit_price       │
                   │ subtotal         │         │ customizations   │
                   │ tax              │         │ special_requests │
                   │ tip              │         └──────────────────┘
                   │ total            │
                   │ payment_id       │         ┌──────────────────┐
                   │ pickup_time      │         │     Payment      │
                   │ created_at       │    1:1  ├──────────────────┤
                   │ updated_at       │────────►│ id (PK)          │
                   └──────────────────┘         │ order_id (FK)    │
                                                │ stripe_payment_id│
                                                │ amount           │
                                                │ status           │
                                                │ meal_subsidy_used│
                                                │ created_at       │
                                                └──────────────────┘
```

### 6.2 Data Stores

| Store | Type | Purpose | Data Types | Retention |
|-------|------|---------|------------|-----------|
| RDS PostgreSQL (main) | Relational | Core business data | Users, Orders, Vendors, Menus | Indefinite |
| RDS PostgreSQL (geo) | Relational + PostGIS | Geospatial queries | Campus boundaries, Geofences | Indefinite |
| DynamoDB (locations) | NoSQL Time-series | Real-time location tracking | Truck positions | 90 days hot, archive to S3 |
| DynamoDB (sessions) | NoSQL Key-Value | User session storage | Session tokens, refresh tokens | 30 days |
| ElastiCache Redis | In-memory Cache | Performance caching | Menu cache, location state | TTL-based (5min - 1hr) |
| S3 (assets) | Object Storage | Static assets | Images, logos, receipts | Indefinite |
| S3 (analytics) | Object Storage | Analytics data lake | Aggregated events, reports | 7 years (compliance) |

### 6.3 Data Flow Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                              ORDER PLACEMENT DATA FLOW                                       │
└─────────────────────────────────────────────────────────────────────────────────────────────┘

┌──────────┐      ┌──────────┐      ┌──────────┐      ┌──────────┐      ┌──────────┐
│  Mobile  │      │   API    │      │  Order   │      │ Payment  │      │  Stripe  │
│   App    │      │ Gateway  │      │ Service  │      │ Service  │      │   API    │
└────┬─────┘      └────┬─────┘      └────┬─────┘      └────┬─────┘      └────┬─────┘
     │                 │                 │                 │                 │
     │ 1. POST /orders │                 │                 │                 │
     │ {cart_id,       │                 │                 │                 │
     │  payment_method}│                 │                 │                 │
     │────────────────►│                 │                 │                 │
     │                 │                 │                 │                 │
     │                 │ 2. Validate JWT │                 │                 │
     │                 │    Extract user │                 │                 │
     │                 │                 │                 │                 │
     │                 │ 3. POST /orders │                 │                 │
     │                 │ (internal)      │                 │                 │
     │                 │────────────────►│                 │                 │
     │                 │                 │                 │                 │
     │                 │                 │ 4. Validate cart│                 │
     │                 │                 │    Check inventory                │
     │                 │                 │    Calculate total                │
     │                 │                 │                 │                 │
     │                 │                 │ 5. Create order │                 │
     │                 │                 │    (PENDING)    │                 │
     │                 │                 │──┐              │                 │
     │                 │                 │  │ PostgreSQL   │                 │
     │                 │                 │◄─┘              │                 │
     │                 │                 │                 │                 │
     │                 │                 │ 6. POST /payments                 │
     │                 │                 │────────────────►│                 │
     │                 │                 │                 │                 │
     │                 │                 │                 │ 7. Create       │
     │                 │                 │                 │    PaymentIntent│
     │                 │                 │                 │────────────────►│
     │                 │                 │                 │                 │
     │                 │                 │                 │◄────────────────│
     │                 │                 │                 │ 8. client_secret│
     │                 │                 │                 │                 │
     │                 │                 │◄────────────────│                 │
     │                 │                 │ 9. payment_id   │                 │
     │                 │                 │    client_secret│                 │
     │                 │                 │                 │                 │
     │                 │◄────────────────│                 │                 │
     │                 │ 10. Order +     │                 │                 │
     │                 │     client_secret                 │                 │
     │                 │                 │                 │                 │
     │◄────────────────│                 │                 │                 │
     │ 11. Order response                │                 │                 │
     │     + client_secret               │                 │                 │
     │                 │                 │                 │                 │
     │ 12. Confirm payment               │                 │                 │
     │     (Stripe SDK)│                 │                 │                 │
     │─────────────────────────────────────────────────────────────────────►│
     │                 │                 │                 │                 │
     │                 │                 │                 │◄────────────────│
     │                 │                 │                 │ 13. Webhook:    │
     │                 │                 │                 │ payment_intent. │
     │                 │                 │                 │ succeeded       │
     │                 │                 │                 │                 │
     │                 │                 │◄────────────────│                 │
     │                 │                 │ 14. Payment     │                 │
     │                 │                 │     confirmed   │                 │
     │                 │                 │                 │                 │
     │                 │                 │ 15. Update order│                 │
     │                 │                 │     (CONFIRMED) │                 │
     │                 │                 │──┐              │                 │
     │                 │                 │  │ PostgreSQL   │                 │
     │                 │                 │◄─┘              │                 │
     │                 │                 │                 │                 │
     │                 │                 │ 16. Publish     │                 │
     │                 │                 │ OrderConfirmed  │                 │
     │                 │                 │──┐              │                 │
     │                 │                 │  │ SNS          │                 │
     │                 │                 │◄─┘              │                 │
     │                 │                 │                 │                 │
     │◄══════════════════════════════════│                 │                 │
     │ 17. Push notification:            │                 │                 │
     │     "Order confirmed!"            │                 │                 │
     │                 │                 │                 │                 │
     ▼                 ▼                 ▼                 ▼                 ▼
```

### 6.4 Data Governance

#### Data Classification

| Classification | Description | Examples | Controls |
|---------------|-------------|----------|----------|
| Public | Non-sensitive business data | Menu items, truck schedules | Standard access controls |
| Internal | Internal business data | Order volumes, analytics | Role-based access |
| Confidential | Sensitive business data | Vendor financials, revenue | Encrypted, audit logged |
| Restricted | PII and payment data | User emails, payment tokens | Encrypted, strict access, compliance |

#### Data Ownership

| Data Domain | Owner | Steward |
|-------------|-------|---------|
| User Data | HR Systems Team | User Service Team |
| Order Data | Food Services | Order Service Team |
| Vendor Data | Vendor Operations | Vendor Service Team |
| Payment Data | Finance | Payment Service Team |
| Location Data | Operations | Location Service Team |

---

## 7. Integration Architecture

### 7.1 Integration Patterns

| Pattern | Use Case | Implementation |
|---------|----------|----------------|
| API Gateway | Single entry point for all clients | AWS ALB + Express middleware |
| Event-Driven | Order lifecycle, notifications | SNS/SQS pub/sub |
| Request-Reply | Synchronous service calls | HTTP/REST with circuit breakers |
| Webhook | Payment confirmations | Stripe webhooks to SQS |
| Data Sync | HR employee data | Scheduled batch import |

### 7.2 API Design

#### API Standards

- **Style**: RESTful with JSON payloads
- **Versioning**: URL path versioning (e.g., `/api/v1/orders`)
- **Authentication**: Bearer token (JWT from Okta)
- **Rate Limiting**: 100 requests/minute per user, 1000/minute per vendor

#### API Endpoint Examples

```
# Orders API
POST   /api/v1/orders                    Create new order
GET    /api/v1/orders                    List user's orders (paginated)
GET    /api/v1/orders/:id                Get order details
PATCH  /api/v1/orders/:id                Update order (limited fields)
POST   /api/v1/orders/:id/cancel         Cancel order
GET    /api/v1/orders/:id/receipt        Get order receipt

# Vendors API
GET    /api/v1/vendors                   List vendors (with filters)
GET    /api/v1/vendors/:id               Get vendor details
GET    /api/v1/vendors/:id/menu          Get vendor menu
GET    /api/v1/vendors/:id/trucks        Get vendor's trucks

# Location API
GET    /api/v1/locations/nearby          Get trucks near coordinates
GET    /api/v1/locations/campus/:id      Get trucks on campus
WS     /ws/locations                     Real-time location stream

# User API
GET    /api/v1/users/me                  Get current user profile
PATCH  /api/v1/users/me                  Update user preferences
GET    /api/v1/users/me/favorites        Get favorite vendors
POST   /api/v1/users/me/favorites        Add favorite vendor
```

#### Sample Request/Response

```json
// POST /api/v1/orders
// Request
{
  "vendor_id": "vnd_abc123",
  "truck_id": "trk_xyz789",
  "items": [
    {
      "menu_item_id": "itm_taco001",
      "quantity": 2,
      "customizations": {
        "protein": "carnitas",
        "salsa": "verde",
        "extras": ["guacamole", "sour_cream"]
      }
    },
    {
      "menu_item_id": "itm_drink001",
      "quantity": 1,
      "customizations": {}
    }
  ],
  "pickup_time": "2024-01-15T12:30:00Z",
  "tip_percentage": 20,
  "use_meal_subsidy": true,
  "payment_method_id": "pm_stripe123"
}

// Response (201 Created)
{
  "id": "ord_def456",
  "status": "PENDING",
  "vendor": {
    "id": "vnd_abc123",
    "name": "Taco Loco Express"
  },
  "truck": {
    "id": "trk_xyz789",
    "location": {
      "latitude": 37.7749,
      "longitude": -122.4194,
      "campus": "SF HQ"
    }
  },
  "items": [
    {
      "id": "oli_001",
      "name": "Street Taco",
      "quantity": 2,
      "unit_price": 4.50,
      "total": 9.00,
      "customizations": {
        "protein": "carnitas",
        "salsa": "verde",
        "extras": ["guacamole", "sour_cream"]
      }
    },
    {
      "id": "oli_002",
      "name": "Horchata",
      "quantity": 1,
      "unit_price": 3.00,
      "total": 3.00
    }
  ],
  "subtotal": 12.00,
  "tax": 1.02,
  "tip": 2.40,
  "meal_subsidy_applied": 5.00,
  "total": 10.42,
  "pickup_time": "2024-01-15T12:30:00Z",
  "estimated_ready": "2024-01-15T12:25:00Z",
  "payment": {
    "status": "REQUIRES_CONFIRMATION",
    "client_secret": "pi_xxx_secret_yyy"
  },
  "created_at": "2024-01-15T12:15:00Z"
}
```

### 7.3 Message Flows

#### Order Confirmation Notification Flow

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Order     │     │     SNS     │     │     SQS     │     │Notification │     │   Firebase  │
│  Service    │     │   Topic     │     │   Queue     │     │  Service    │     │     FCM     │
└──────┬──────┘     └──────┬──────┘     └──────┬──────┘     └──────┬──────┘     └──────┬──────┘
       │                   │                   │                   │                   │
       │ OrderConfirmed    │                   │                   │                   │
       │ {order_id,        │                   │                   │                   │
       │  user_id,         │                   │                   │                   │
       │  vendor_name,     │                   │                   │                   │
       │  total,           │                   │                   │                   │
       │  pickup_time}     │                   │                   │                   │
       │──────────────────►│                   │                   │                   │
       │                   │                   │                   │                   │
       │                   │ Fan-out to        │                   │                   │
       │                   │ subscribers       │                   │                   │
       │                   │──────────────────►│                   │                   │
       │                   │                   │                   │                   │
       │                   │                   │ Poll messages     │                   │
       │                   │                   │◄──────────────────│                   │
       │                   │                   │                   │                   │
       │                   │                   │ Message batch     │                   │
       │                   │                   │──────────────────►│                   │
       │                   │                   │                   │                   │
       │                   │                   │                   │ Lookup device     │
       │                   │                   │                   │ token (DynamoDB)  │
       │                   │                   │                   │──┐                │
       │                   │                   │                   │◄─┘                │
       │                   │                   │                   │                   │
       │                   │                   │                   │ Send push         │
       │                   │                   │                   │ notification      │
       │                   │                   │                   │──────────────────►│
       │                   │                   │                   │                   │
       │                   │                   │                   │◄──────────────────│
       │                   │                   │                   │ Delivery receipt  │
       │                   │                   │                   │                   │
       │                   │                   │◄──────────────────│                   │
       │                   │                   │ Delete message    │                   │
       │                   │                   │ (ack)             │                   │
       │                   │                   │                   │                   │
       ▼                   ▼                   ▼                   ▼                   ▼
```

### 7.4 External API Contracts

#### Stripe Integration

| Operation | Endpoint | Purpose |
|-----------|----------|---------|
| Create PaymentIntent | `POST /v1/payment_intents` | Initialize payment |
| Confirm PaymentIntent | `POST /v1/payment_intents/:id/confirm` | Complete payment |
| Create Refund | `POST /v1/refunds` | Process refunds |
| Webhook Handler | `POST /webhooks/stripe` | Receive payment events |

#### Okta Integration

| Operation | Endpoint | Purpose |
|-----------|----------|---------|
| Authorization | `/oauth2/v1/authorize` | Initiate SSO flow |
| Token Exchange | `/oauth2/v1/token` | Get access token |
| UserInfo | `/oauth2/v1/userinfo` | Get user profile |
| Logout | `/oauth2/v1/logout` | End session |

---

## 8. Infrastructure Architecture

### 8.1 Cloud Architecture

**Primary Cloud Provider**: Amazon Web Services (AWS)

**Rationale**:
- Existing Globex enterprise agreement with favorable pricing
- Team familiarity with AWS services
- Mature managed services (RDS, ElastiCache, ECS)
- Strong compliance certifications (SOC2, PCI-DSS)

**Multi-Region Strategy**: Single region (us-west-2) with Multi-AZ deployment

**Rationale for single region**:
- All campuses within North America (latency acceptable)
- Cost optimization vs. multi-region complexity
- Simpler disaster recovery procedures
- Future: Expand to us-east-1 when East Coast campuses onboard

### 8.2 Deployment Topology

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    AWS REGION: us-west-2                                     │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                              │
│  ┌────────────────────────────────────────────────────────────────────────────────────────┐ │
│  │                                    INTERNET EDGE                                        │ │
│  │                                                                                         │ │
│  │  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐                    │ │
│  │  │   Route 53      │    │   CloudFront    │    │   AWS Shield    │                    │ │
│  │  │   (DNS)         │───►│   (CDN)         │───►│   + WAF         │                    │ │
│  │  │                 │    │                 │    │                 │                    │ │
│  │  │ tacotracker.    │    │ Static assets   │    │ DDoS protection │                    │ │
│  │  │ globex.com      │    │ API caching     │    │ Rate limiting   │                    │ │
│  │  └─────────────────┘    └─────────────────┘    └────────┬────────┘                    │ │
│  │                                                          │                             │ │
│  └──────────────────────────────────────────────────────────┼─────────────────────────────┘ │
│                                                              │                               │
│  ┌──────────────────────────────────────────────────────────┼─────────────────────────────┐ │
│  │                              VPC: 10.0.0.0/16            │                              │ │
│  │                                                          │                              │ │
│  │  ┌─────────────────────────────────────────────────────────────────────────────────┐  │ │
│  │  │                        PUBLIC SUBNETS (10.0.0.0/20)                              │  │ │
│  │  │                                                                                  │  │ │
│  │  │  ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐          │  │ │
│  │  │  │   AZ: us-west-2a │    │   AZ: us-west-2b │    │   AZ: us-west-2c │          │  │ │
│  │  │  │                  │    │                  │    │                  │          │  │ │
│  │  │  │  ┌────────────┐  │    │  ┌────────────┐  │    │  ┌────────────┐  │          │  │ │
│  │  │  │  │    ALB     │  │    │  │    ALB     │  │    │  │    ALB     │  │          │  │ │
│  │  │  │  │  (Target)  │  │    │  │  (Target)  │  │    │  │  (Target)  │  │          │  │ │
│  │  │  │  └────────────┘  │    │  └────────────┘  │    │  └────────────┘  │          │  │ │
│  │  │  │                  │    │                  │    │                  │          │  │ │
│  │  │  │  ┌────────────┐  │    │  ┌────────────┐  │    │  ┌────────────┐  │          │  │ │
│  │  │  │  │  NAT GW    │  │    │  │  NAT GW    │  │    │  │  NAT GW    │  │          │  │ │
│  │  │  │  └────────────┘  │    │  └────────────┘  │    │  └────────────┘  │          │  │ │
│  │  │  └──────────────────┘    └──────────────────┘    └──────────────────┘          │  │ │
│  │  └─────────────────────────────────────────────────────────────────────────────────┘  │ │
│  │                                         │                                              │ │
│  │  ┌─────────────────────────────────────────────────────────────────────────────────┐  │ │
│  │  │                       PRIVATE SUBNETS - APP (10.0.16.0/20)                       │  │ │
│  │  │                                                                                  │  │ │
│  │  │  ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐          │  │ │
│  │  │  │   AZ: us-west-2a │    │   AZ: us-west-2b │    │   AZ: us-west-2c │          │  │ │
│  │  │  │                  │    │                  │    │                  │          │  │ │
│  │  │  │  ┌────────────┐  │    │  ┌────────────┐  │    │  ┌────────────┐  │          │  │ │
│  │  │  │  │ ECS Fargate│  │    │  │ ECS Fargate│  │    │  │ ECS Fargate│  │          │  │ │
│  │  │  │  │ Cluster    │  │    │  │ Cluster    │  │    │  │ Cluster    │  │          │  │ │
│  │  │  │  │            │  │    │  │            │  │    │  │            │  │          │  │ │
│  │  │  │  │ • API GW   │  │    │  │ • API GW   │  │    │  │ • API GW   │  │          │  │ │
│  │  │  │  │ • Location │  │    │  │ • Location │  │    │  │ • Location │  │          │  │ │
│  │  │  │  │ • Order    │  │    │  │ • Order    │  │    │  │ • Order    │  │          │  │ │
│  │  │  │  │ • User     │  │    │  │ • User     │  │    │  │ • User     │  │          │  │ │
│  │  │  │  │ • Vendor   │  │    │  │ • Vendor   │  │    │  │ • Vendor   │  │          │  │ │
│  │  │  │  │ • Payment  │  │    │  │ • Payment  │  │    │  │ • Payment  │  │          │  │ │
│  │  │  │  │ • Notif    │  │    │  │ • Notif    │  │    │  │ • Notif    │  │          │  │ │
│  │  │  │  └────────────┘  │    │  └────────────┘  │    │  └────────────┘  │          │  │ │
│  │  │  └──────────────────┘    └──────────────────┘    └──────────────────┘          │  │ │
│  │  └─────────────────────────────────────────────────────────────────────────────────┘  │ │
│  │                                         │                                              │ │
│  │  ┌─────────────────────────────────────────────────────────────────────────────────┐  │ │
│  │  │                       PRIVATE SUBNETS - DATA (10.0.32.0/20)                      │  │ │
│  │  │                                                                                  │  │ │
│  │  │  ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐          │  │ │
│  │  │  │   AZ: us-west-2a │    │   AZ: us-west-2b │    │   AZ: us-west-2c │          │  │ │
│  │  │  │                  │    │                  │    │                  │          │  │ │
│  │  │  │  ┌────────────┐  │    │  ┌────────────┐  │    │  ┌────────────┐  │          │  │ │
│  │  │  │  │    RDS     │  │    │  │    RDS     │  │    │  │    RDS     │  │          │  │ │
│  │  │  │  │  Primary   │  │    │  │  Standby   │  │    │  │ Read Repl  │  │          │  │ │
│  │  │  │  └────────────┘  │    │  └────────────┘  │    │  └────────────┘  │          │  │ │
│  │  │  │                  │    │                  │    │                  │          │  │ │
│  │  │  │  ┌────────────┐  │    │  ┌────────────┐  │    │  ┌────────────┐  │          │  │ │
│  │  │  │  │   Redis    │  │    │  │   Redis    │  │    │  │   Redis    │  │          │  │ │
│  │  │  │  │  Shard 1   │  │    │  │  Shard 2   │  │    │  │  Shard 3   │  │          │  │ │
│  │  │  │  └────────────┘  │    │  └────────────┘  │    │  └────────────┘  │          │  │ │
│  │  │  └──────────────────┘    └──────────────────┘    └──────────────────┘          │  │ │
│  │  └─────────────────────────────────────────────────────────────────────────────────┘  │ │
│  │                                                                                        │ │
│  └────────────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                              │
│  ┌────────────────────────────────────────────────────────────────────────────────────────┐ │
│  │                                   SHARED SERVICES                                       │ │
│  │                                                                                         │ │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  │ │
│  │  │     SNS     │  │     SQS     │  │  DynamoDB   │  │     S3      │  │   Secrets   │  │ │
│  │  │   Topics    │  │   Queues    │  │   Tables    │  │   Buckets   │  │   Manager   │  │ │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘  │ │
│  │                                                                                         │ │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  │ │
│  │  │ CloudWatch  │  │   X-Ray     │  │     ECR     │  │     KMS     │  │   Lambda    │  │ │
│  │  │   Logs      │  │   Traces    │  │   Registry  │  │    Keys     │  │  Functions  │  │ │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘  │ │
│  │                                                                                         │ │
│  └────────────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                              │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

### 8.3 Infrastructure as Code

**IaC Tool**: Terraform v1.6+

**Repository Structure**:
```
infrastructure/
├── modules/
│   ├── vpc/
│   ├── ecs-cluster/
│   ├── rds/
│   ├── elasticache/
│   ├── alb/
│   └── monitoring/
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── terraform.tfvars
│   ├── staging/
│   └── production/
└── shared/
    ├── ecr/
    ├── route53/
    └── iam/
```

**Environment Strategy**:

| Environment | Purpose | Scale | Data |
|-------------|---------|-------|------|
| Development | Feature development | Minimal (1 task/service) | Synthetic data |
| Staging | Integration testing | Production-like (2 tasks) | Anonymized prod snapshot |
| Production | Live system | Full scale (auto-scaling) | Production data |

### 8.4 Networking

#### VPC Design

| Component | CIDR | Purpose |
|-----------|------|---------|
| VPC | 10.0.0.0/16 | Main VPC (65,536 IPs) |
| Public Subnets | 10.0.0.0/20 | ALB, NAT Gateways |
| Private App Subnets | 10.0.16.0/20 | ECS Fargate tasks |
| Private Data Subnets | 10.0.32.0/20 | RDS, ElastiCache |

#### Load Balancing

- **Application Load Balancer** for HTTP/HTTPS traffic
- **Target Groups** per service with health checks
- **Path-based routing**:
  - `/api/v1/orders/*` -> Order Service
  - `/api/v1/vendors/*` -> Vendor Service
  - `/api/v1/locations/*` -> Location Service
  - `/ws/*` -> Location Service (WebSocket)

#### DNS Configuration

| Record | Type | Target |
|--------|------|--------|
| tacotracker.globex.com | A (Alias) | CloudFront Distribution |
| api.tacotracker.globex.com | A (Alias) | ALB |
| vendor.tacotracker.globex.com | A (Alias) | CloudFront Distribution |
| admin.tacotracker.globex.com | A (Alias) | CloudFront Distribution |

---

## 9. Security Architecture

### 9.1 Security Principles

1. **Defense in Depth**: Multiple security layers - network, application, data
2. **Least Privilege**: Minimal permissions for all services and users
3. **Zero Trust**: Verify every request, assume breach mentality
4. **Secure by Default**: Security controls enabled out of the box
5. **Audit Everything**: Comprehensive logging of security-relevant events

### 9.2 Authentication and Authorization

#### Authentication Flow (Okta OIDC)

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  Mobile  │     │   Okta   │     │   API    │     │ Backend  │
│   App    │     │   IdP    │     │ Gateway  │     │ Services │
└────┬─────┘     └────┬─────┘     └────┬─────┘     └────┬─────┘
     │                │                │                │
     │ 1. Login tap   │                │                │
     │───────────────►│                │                │
     │                │                │                │
     │ 2. Okta login  │                │                │
     │   (in browser) │                │                │
     │◄───────────────│                │                │
     │                │                │                │
     │ 3. User creds  │                │                │
     │───────────────►│                │                │
     │                │                │                │
     │ 4. MFA (if     │                │                │
     │    required)   │                │                │
     │◄──────────────►│                │                │
     │                │                │                │
     │ 5. Auth code   │                │                │
     │◄───────────────│                │                │
     │                │                │                │
     │ 6. Exchange    │                │                │
     │    code        │                │                │
     │───────────────►│                │                │
     │                │                │                │
     │ 7. Tokens      │                │                │
     │    (access,    │                │                │
     │     refresh,   │                │                │
     │     id_token)  │                │                │
     │◄───────────────│                │                │
     │                │                │                │
     │ 8. API request │                │                │
     │    + Bearer    │                │                │
     │    token       │                │                │
     │─────────────────────────────────►                │
     │                │                │                │
     │                │                │ 9. Validate    │
     │                │                │    JWT         │
     │                │                │    (cached     │
     │                │                │     JWKS)      │
     │                │                │                │
     │                │                │ 10. Forward    │
     │                │                │     request    │
     │                │                │───────────────►│
     │                │                │                │
     ▼                ▼                ▼                ▼
```

#### Authorization Model (RBAC)

| Role | Permissions | Users |
|------|-------------|-------|
| Employee | Browse vendors, place orders, view own history | All Globex employees |
| Vendor Admin | Manage own menus, view own orders, fleet management | Taco truck operators |
| Vendor Staff | View and fulfill orders for their vendor | Truck staff |
| Food Services Admin | View all vendors, analytics, approve vendors | Globex food services team |
| System Admin | Full system access, user management | Globex IT |

#### JWT Claims Structure

```json
{
  "sub": "okta_user_id",
  "email": "user@globex.com",
  "given_name": "John",
  "family_name": "Doe",
  "groups": ["employees", "sf-campus"],
  "tacotracker_role": "employee",
  "tacotracker_vendor_id": null,
  "tacotracker_meal_subsidy": 10.00,
  "iat": 1705312800,
  "exp": 1705316400,
  "iss": "https://globex.okta.com",
  "aud": "tacotracker-api"
}
```

### 9.3 Data Security

#### Encryption at Rest

| Data Store | Encryption | Key Management |
|------------|------------|----------------|
| RDS PostgreSQL | AES-256 | AWS KMS (CMK) |
| ElastiCache Redis | AES-256 | AWS KMS |
| DynamoDB | AES-256 | AWS managed |
| S3 | AES-256 | AWS KMS (CMK) |
| EBS Volumes | AES-256 | AWS KMS |

#### Encryption in Transit

- All external traffic: TLS 1.3
- Internal service-to-service: TLS 1.2+
- Database connections: SSL required
- Redis connections: TLS enabled

#### Secrets Management

- **AWS Secrets Manager** for all credentials
- **Automatic rotation** for database passwords (90-day cycle)
- **No secrets in code** - all injected via environment variables
- **Audit logging** for all secret access

### 9.4 Network Security

#### Security Groups

| Group | Inbound | Outbound |
|-------|---------|----------|
| ALB-SG | 443 from 0.0.0.0/0 | App-SG:8080 |
| App-SG | 8080 from ALB-SG | Data-SG:5432, Data-SG:6379, 443 (internet via NAT) |
| Data-SG | 5432, 6379 from App-SG | None |

#### WAF Rules

| Rule | Action | Purpose |
|------|--------|---------|
| AWS Managed - Common | Block | OWASP Top 10 protection |
| AWS Managed - SQL Injection | Block | SQL injection prevention |
| Rate Limit - IP | Block (>1000/5min) | DDoS mitigation |
| Rate Limit - User | Block (>100/min) | Abuse prevention |
| Geo Block | Block (non-US) | Reduce attack surface |

### 9.5 Compliance

| Requirement | Implementation | Evidence |
|-------------|---------------|----------|
| PCI-DSS | Stripe handles card data; no PAN storage | Stripe attestation, architecture review |
| SOC 2 Type II | AWS controls, access logging, encryption | AWS compliance reports, audit logs |
| CCPA | Data deletion API, privacy controls | Data retention policies, deletion logs |
| Globex Security Policy | SSO, MFA, encryption, access controls | Security assessment, penetration test |

---

## 10. Scalability and Performance

### 10.1 Scalability Strategy

#### Horizontal Scaling (Primary Approach)

| Component | Scaling Trigger | Min | Max | Scale Increment |
|-----------|----------------|-----|-----|-----------------|
| API Gateway | CPU > 70% or Requests > 500/sec | 2 | 10 | +2 tasks |
| Location Service | CPU > 60% or WebSocket connections > 5000 | 3 | 15 | +2 tasks |
| Order Service | CPU > 70% or Queue depth > 100 | 2 | 10 | +2 tasks |
| User Service | CPU > 70% | 2 | 6 | +1 task |
| Vendor Service | CPU > 70% | 2 | 6 | +1 task |
| Payment Service | CPU > 70% or Latency p95 > 500ms | 2 | 8 | +2 tasks |
| Notification Service | Queue depth > 500 | 2 | 6 | +2 tasks |

#### Database Scaling

- **Read scaling**: Read replicas for analytics and read-heavy queries
- **Write scaling**: Vertical scaling of primary instance (future: sharding)
- **Connection pooling**: PgBouncer for efficient connection management

### 10.2 Performance Targets

| Metric | Target | Critical Threshold | Measurement |
|--------|--------|-------------------|-------------|
| API Response Time (p50) | < 100ms | > 200ms | CloudWatch, X-Ray |
| API Response Time (p95) | < 200ms | > 500ms | CloudWatch, X-Ray |
| API Response Time (p99) | < 500ms | > 1000ms | CloudWatch, X-Ray |
| Location Update Latency | < 500ms | > 2000ms | Custom metrics |
| Order Placement E2E | < 3s | > 5s | Synthetic monitoring |
| Mobile App Launch | < 2s | > 4s | Firebase Performance |
| Database Query Time (p95) | < 50ms | > 100ms | RDS Performance Insights |

### 10.3 Caching Strategy

| Cache Layer | Technology | TTL | Invalidation Strategy |
|-------------|------------|-----|----------------------|
| CDN (Static) | CloudFront | 24 hours | Deploy-time invalidation |
| CDN (API) | CloudFront | 60 seconds | Cache-Control headers |
| Application | Redis | 5-60 minutes | Event-driven invalidation |
| Database | PostgreSQL | Query plan cache | Automatic |

#### Cache Keys and Policies

```
# Menu cache (rarely changes)
menu:{vendor_id}                    TTL: 30 minutes
menu:{vendor_id}:items              TTL: 30 minutes

# Vendor cache (moderate changes)
vendor:{vendor_id}                  TTL: 15 minutes
vendor:{vendor_id}:schedule         TTL: 5 minutes
vendor:nearby:{campus_id}           TTL: 1 minute

# Location cache (frequent changes)
location:truck:{truck_id}:current   TTL: 30 seconds
location:campus:{campus_id}:trucks  TTL: 10 seconds

# User cache (session data)
user:{user_id}:profile              TTL: 60 minutes
user:{user_id}:favorites            TTL: 30 minutes
```

### 10.4 Performance Testing Approach

#### Load Testing

- **Tool**: k6 for scripted load tests
- **Frequency**: Weekly automated runs, pre-release mandatory
- **Scenarios**:
  - Normal load: 1,000 concurrent users
  - Peak load: 10,000 concurrent users (lunch rush simulation)
  - Stress test: Ramp to 20,000 users to find breaking point
  - Soak test: 5,000 users for 4 hours

#### Capacity Planning

| Time Period | Expected Users | Required Capacity |
|-------------|---------------|-------------------|
| Off-peak (before 10 AM) | 500 | Minimum (baseline) |
| Morning (10 AM - 11 AM) | 2,000 | 2x baseline |
| Lunch peak (11 AM - 1 PM) | 10,000 | 5x baseline |
| Afternoon (1 PM - 5 PM) | 3,000 | 2x baseline |
| Evening (after 5 PM) | 1,000 | Minimum (baseline) |

---

## 11. Reliability and Availability

### 11.1 Availability Targets

| Tier | Services | Target | Monthly Downtime |
|------|----------|--------|-----------------|
| Critical | Order, Payment | 99.9% | 43 minutes |
| High | Location, Notification | 99.5% | 3.6 hours |
| Standard | Analytics, Admin | 99.0% | 7.3 hours |

**Business Hours Focus**: 99.9% availability during 6 AM - 8 PM local time (all campuses)

### 11.2 Fault Tolerance

#### Single Points of Failure Analysis

| Component | SPOF Risk | Mitigation |
|-----------|-----------|------------|
| RDS Primary | Yes | Multi-AZ standby, automatic failover |
| Redis | Partial | Cluster mode with replicas |
| ALB | No | AWS managed, multi-AZ |
| ECS Tasks | No | Multi-AZ deployment, auto-replacement |
| NAT Gateway | Yes (per AZ) | NAT Gateway per AZ |

#### Circuit Breaker Configuration

```javascript
// Circuit breaker settings for external services
const circuitBreakerConfig = {
  stripe: {
    failureThreshold: 5,      // Open after 5 failures
    successThreshold: 3,       // Close after 3 successes
    timeout: 30000,            // 30 second timeout
    resetTimeout: 60000        // Try again after 60 seconds
  },
  okta: {
    failureThreshold: 3,
    successThreshold: 2,
    timeout: 10000,
    resetTimeout: 30000
  },
  fcm: {
    failureThreshold: 10,
    successThreshold: 5,
    timeout: 5000,
    resetTimeout: 120000
  }
};
```

#### Retry Policies

| Operation | Max Retries | Backoff | Timeout |
|-----------|-------------|---------|---------|
| Database queries | 3 | Exponential (100ms base) | 5s |
| External APIs | 3 | Exponential (500ms base) | 30s |
| Message publishing | 5 | Exponential (100ms base) | 10s |
| Cache operations | 2 | Fixed (50ms) | 500ms |

### 11.3 Disaster Recovery

| Scenario | RTO | RPO | Recovery Strategy |
|----------|-----|-----|-------------------|
| Single AZ failure | 5 minutes | 0 | Automatic failover (Multi-AZ) |
| Service failure | 2 minutes | 0 | ECS auto-recovery, health checks |
| Database corruption | 1 hour | 5 minutes | Point-in-time recovery |
| Region failure | 4 hours | 1 hour | Cross-region restore from S3 |
| Complete data loss | 24 hours | 24 hours | Restore from daily backup |

#### Backup Schedule

| Data | Frequency | Retention | Storage |
|------|-----------|-----------|---------|
| RDS (automated) | Continuous | 7 days | RDS snapshots |
| RDS (manual) | Daily | 30 days | RDS snapshots |
| DynamoDB | Continuous | 35 days | PITR |
| S3 assets | Real-time | Indefinite | Cross-region replication |
| Redis | Hourly | 24 hours | S3 snapshots |

### 11.4 Monitoring and Alerting

#### Monitoring Stack

- **Metrics**: Amazon CloudWatch
- **Logs**: CloudWatch Logs + Logs Insights
- **Traces**: AWS X-Ray
- **APM**: Datadog (future consideration)
- **Uptime**: AWS Synthetics (Canaries)

#### Key Dashboards

1. **Executive Dashboard**: SLI/SLO status, order volume, revenue
2. **Operations Dashboard**: Service health, error rates, latency
3. **Infrastructure Dashboard**: CPU, memory, network, database
4. **Security Dashboard**: Auth failures, blocked requests, anomalies

#### Alert Thresholds

| Alert | Condition | Severity | Response |
|-------|-----------|----------|----------|
| High Error Rate | 5xx > 1% (5 min) | Critical | Page on-call |
| High Latency | p95 > 1s (5 min) | High | Page on-call |
| Database CPU | > 80% (10 min) | High | Notify team |
| Low Disk Space | < 20% free | Medium | Notify team |
| Failed Health Checks | > 2 consecutive | Critical | Auto-replace + notify |
| Payment Failures | > 5% (5 min) | Critical | Page on-call |

---

## 12. Technology Stack

### 12.1 Technology Decisions

| Layer | Technology | Version | Rationale |
|-------|------------|---------|-----------|
| Mobile Framework | React Native | 0.72 | Cross-platform, code sharing, team expertise |
| Mobile Navigation | React Navigation | 6.x | Standard for React Native |
| Web Frontend | React | 18.x | Component model, ecosystem, team expertise |
| State Management | Redux Toolkit | 2.x | Predictable state, DevTools, RTK Query |
| UI Components | Material UI / Ant Design | 5.x | Comprehensive, accessible, themeable |
| Backend Runtime | Node.js | 20 LTS | JavaScript ecosystem, async I/O, team expertise |
| API Framework | Express.js | 4.18 | Mature, flexible, large middleware ecosystem |
| WebSocket | Socket.io | 4.x | Reliable real-time, fallback transport |
| Database | PostgreSQL | 15 | ACID, PostGIS, JSON support, reliability |
| Cache | Redis | 7.x | Performance, pub/sub, data structures |
| Search (future) | OpenSearch | 2.x | Full-text search for menus (Phase 2) |
| Container | Docker | 24.x | Standard containerization |
| Orchestration | ECS Fargate | - | Serverless containers, AWS integration |
| IaC | Terraform | 1.6+ | Multi-cloud capability, state management |
| CI/CD | GitHub Actions | - | GitHub integration, marketplace actions |
| Monitoring | CloudWatch + X-Ray | - | AWS native, cost-effective |

### 12.2 Technology Radar

#### Adopt (Use in Production)
- React Native for mobile
- Node.js + Express for backend
- PostgreSQL for relational data
- Redis for caching and pub/sub
- ECS Fargate for containers
- Terraform for IaC

#### Trial (Use with Caution)
- GraphQL for mobile API (evaluate complexity vs. REST)
- Prisma ORM (evaluate vs. raw SQL performance)
- AWS App Runner (simpler alternative to ECS)

#### Assess (Research)
- Bun runtime (potential Node.js replacement)
- Drizzle ORM (TypeScript-first alternative)
- AWS Lambda for specific workloads

#### Hold (Avoid)
- MongoDB (PostgreSQL meets all needs)
- Kubernetes (over-engineered for our scale)
- GraphQL Federation (unnecessary complexity)
- Self-hosted Redis (ElastiCache preferred)

---

## 13. Architecture Decision Records (ADRs)

### ADR-001: Use React Native for Mobile Development

**Status**: Accepted

**Context**: TacoTracker requires native mobile apps for iOS and Android. We need to decide between native development (Swift/Kotlin), cross-platform (React Native/Flutter), or hybrid (Ionic/Capacitor) approaches.

**Decision**: Use React Native for mobile app development.

**Consequences**:

**Positive:**
- Single codebase for iOS and Android (~85% code sharing)
- Leverage existing JavaScript/TypeScript expertise
- Faster development velocity
- Hot reloading for rapid iteration
- Large ecosystem of libraries

**Negative:**
- Some native modules may require platform-specific code
- Performance slightly lower than pure native
- Dependency on Meta's continued investment
- Native GPS integration requires additional work

**Alternatives Considered**:

| Alternative | Pros | Cons | Reason Not Chosen |
|-------------|------|------|-------------------|
| Native (Swift/Kotlin) | Best performance, full API access | 2x development effort, separate teams | Resource constraints |
| Flutter | Good performance, single codebase | Dart learning curve, smaller ecosystem | Team lacks Dart experience |
| Ionic | Web technology reuse | Poor native feel, performance issues | Not suitable for real-time GPS |

---

### ADR-002: Event-Driven Architecture for Order Processing

**Status**: Accepted

**Context**: Order processing involves multiple services (order, payment, notification, analytics) that need to be coordinated. We need to decide between synchronous orchestration or asynchronous choreography.

**Decision**: Use event-driven architecture with SNS/SQS for order processing.

**Consequences**:

**Positive:**
- Services are loosely coupled
- Easy to add new consumers (analytics, reporting)
- Better fault tolerance (failures don't cascade)
- Natural audit trail of events
- Independent scaling of consumers

**Negative:**
- Eventual consistency (order status may lag)
- More complex debugging and tracing
- Need for idempotent event handlers
- Message ordering challenges

**Alternatives Considered**:

| Alternative | Pros | Cons | Reason Not Chosen |
|-------------|------|------|-------------------|
| Synchronous REST | Simple, immediate consistency | Tight coupling, cascading failures | Poor fault tolerance |
| Saga Orchestrator | Centralized control, easier rollback | Single point of failure, complexity | Over-engineering |
| AWS Step Functions | Visual workflow, built-in retry | Cost at scale, vendor lock-in | Cost concerns |

---

### ADR-003: PostgreSQL as Primary Database

**Status**: Accepted

**Context**: TacoTracker needs to store relational data (users, orders, vendors) and geospatial data (truck locations, campus boundaries). We need to select a database that handles both requirements.

**Decision**: Use PostgreSQL with PostGIS extension as the primary database.

**Consequences**:

**Positive:**
- ACID transactions for order integrity
- PostGIS provides enterprise-grade geospatial capabilities
- JSON columns for flexible schema where needed
- Excellent query performance with proper indexing
- Mature, well-understood technology

**Negative:**
- Vertical scaling limits (mitigated by read replicas)
- Requires careful schema design
- PostGIS queries can be complex

**Alternatives Considered**:

| Alternative | Pros | Cons | Reason Not Chosen |
|-------------|------|------|-------------------|
| MongoDB | Flexible schema, easy geo queries | No ACID for orders, inconsistency risk | Data integrity concerns |
| MySQL | Familiar, good performance | Weaker geospatial support | PostGIS superiority |
| DynamoDB | Infinite scale, managed | No joins, complex geo queries | Relational needs |

---

### ADR-004: Okta for Identity Management

**Status**: Accepted

**Context**: TacoTracker needs to authenticate Globex employees and authorize access based on roles. Globex has an existing Okta deployment for enterprise SSO.

**Decision**: Integrate with Globex's existing Okta instance for authentication and authorization.

**Consequences**:

**Positive:**
- Single sign-on for employees (no new passwords)
- Leverage existing MFA enrollment
- Centralized user provisioning/deprovisioning
- Group-based authorization from Okta
- Compliance with Globex security policies

**Negative:**
- Dependency on external service for authentication
- Okta outage = TacoTracker outage (for new logins)
- Limited customization of login experience
- Additional complexity for vendor authentication

**Alternatives Considered**:

| Alternative | Pros | Cons | Reason Not Chosen |
|-------------|------|------|-------------------|
| AWS Cognito | AWS native, lower cost | Separate identity silo, no SSO | Globex policy requires Okta |
| Auth0 | Developer-friendly, flexible | Additional cost, separate from Globex IdP | No SSO with existing Okta |
| Custom JWT | Full control, no dependencies | Security risk, maintenance burden | Security concerns |

---

### ADR-005: DynamoDB for Location Time-Series Data

**Status**: Accepted

**Context**: Location data arrives at high frequency (every 5-30 seconds per truck) and needs to be stored for 90 days for analytics and compliance. Query patterns are primarily time-range based.

**Decision**: Use DynamoDB with TTL for location time-series data.

**Consequences**:

**Positive:**
- Handles high write throughput efficiently
- Automatic TTL-based data expiration
- On-demand scaling for variable load
- Cost-effective for time-series patterns
- Point-in-time recovery for compliance

**Negative:**
- Query flexibility limited to partition/sort keys
- No complex aggregations (must process in application)
- Different operational model than PostgreSQL

**Alternatives Considered**:

| Alternative | Pros | Cons | Reason Not Chosen |
|-------------|------|------|-------------------|
| PostgreSQL | Unified database, complex queries | Partition management complexity, cost at scale | Operational overhead |
| TimescaleDB | Time-series optimized SQL | Additional infrastructure, licensing | Complexity |
| InfluxDB | Purpose-built time-series | Yet another database, learning curve | Team expertise |

---

## 14. Non-Functional Requirements Mapping

| NFR ID | Requirement | Architecture Response | Validation Method |
|--------|-------------|----------------------|-------------------|
| NFR-001 | Support 10,000 concurrent users | ECS auto-scaling, Redis caching, read replicas | Load testing with k6 |
| NFR-002 | 99.9% availability during business hours | Multi-AZ deployment, health checks, auto-recovery | CloudWatch SLI tracking |
| NFR-003 | API response time p95 < 200ms | Caching strategy, CDN, optimized queries | APM monitoring, synthetic tests |
| NFR-004 | Location updates within 1 second | Redis pub/sub, WebSocket, optimized path | E2E latency monitoring |
| NFR-005 | Zero PCI data storage | Stripe tokenization, no PAN in logs | Security audit, log review |
| NFR-006 | Support Okta SSO | OIDC integration, JWT validation | Integration testing |
| NFR-007 | Audit trail for all orders | Event sourcing, CloudWatch Logs | Log retention verification |
| NFR-008 | Data retention: 7 years financial | S3 archival, Glacier lifecycle | Backup restore testing |
| NFR-009 | Mobile app offline support | Local storage, sync queue | Offline mode testing |
| NFR-010 | Support 12 campus locations | Multi-tenant design, campus filtering | Functional testing |

---

## 15. Risks and Mitigations

| Risk ID | Risk Description | Likelihood | Impact | Mitigation | Owner |
|---------|-----------------|------------|--------|------------|-------|
| RISK-001 | Stripe outage prevents payments | Low | High | Payment queue, retry logic, display cached status | Payment Team |
| RISK-002 | GPS inaccuracy in parking garages | Medium | Medium | Hybrid location (WiFi, BLE beacons), manual check-in | Location Team |
| RISK-003 | Peak lunch load exceeds capacity | Medium | High | Aggressive auto-scaling, load shedding, caching | Platform Team |
| RISK-004 | Okta outage blocks all logins | Low | Critical | Token caching, extended session validity | Security Team |
| RISK-005 | Vendor adoption is slow | Medium | Medium | Simplified onboarding, training, incentives | Product Team |
| RISK-006 | Data breach exposes user information | Low | Critical | Encryption, WAF, security monitoring, incident response | Security Team |
| RISK-007 | Cost overrun on AWS | Medium | Medium | Budget alerts, reserved instances, right-sizing | Platform Team |
| RISK-008 | Key team member leaves | Medium | Medium | Documentation, knowledge sharing, cross-training | Engineering Lead |
| RISK-009 | React Native breaking changes | Low | Medium | Version pinning, gradual upgrades, testing | Mobile Team |
| RISK-010 | Competitor launches similar service | Low | Low | Rapid iteration, user feedback, feature differentiation | Product Team |

---

## 16. Future Evolution / Roadmap

### 16.1 Phase 1: MVP (Months 1-4)

**Capabilities**:
- Employee mobile app (iOS and Android)
- Vendor web portal (basic)
- Real-time truck tracking
- Menu browsing and ordering
- Payment processing
- Push notifications

**Architecture Scope**:
- Single region (us-west-2)
- Core services only
- Basic auto-scaling
- CloudWatch monitoring

### 16.2 Phase 2: Enhancement (Months 5-8)

**Capabilities**:
- Order scheduling (pre-orders)
- Favorites and order history
- Vendor analytics dashboard
- Admin dashboard
- Loyalty program foundation
- Search and filtering

**Architecture Changes**:
- OpenSearch for menu search
- Enhanced analytics pipeline
- Improved caching strategy
- Performance optimization

### 16.3 Phase 3: Scale (Months 9-12)

**Capabilities**:
- Multi-campus rollout
- Advanced analytics
- Recommendation engine
- Catering/group orders
- Third-party integrations

**Architecture Changes**:
- Multi-region preparation
- Machine learning pipeline
- Advanced event streaming
- API gateway enhancements

### 16.4 Long-term Vision (Year 2+)

**Future State Architecture**:
- Multi-region active-active deployment
- AI-powered recommendations
- Autonomous vendor onboarding
- White-label platform capabilities
- Third-party marketplace

**Technology Evolution**:
- Evaluate GraphQL federation
- Consider edge computing for location
- Explore ML-driven demand prediction
- Assess serverless migration

**Scalability Runway**:
- Current architecture supports 100,000 users
- Identified path to 1M users with sharding
- Cost optimization through reserved capacity

---

## Appendix A: Glossary

| Term | Definition |
|------|------------|
| ADR | Architecture Decision Record - documented architectural decision |
| ALB | Application Load Balancer - AWS L7 load balancer |
| C4 Model | Context, Container, Component, Code - architecture diagram methodology |
| CDN | Content Delivery Network - edge caching for static content |
| ECS | Elastic Container Service - AWS container orchestration |
| Fargate | AWS serverless container compute engine |
| FCM | Firebase Cloud Messaging - push notification service |
| OIDC | OpenID Connect - authentication protocol |
| PostGIS | PostgreSQL extension for geospatial data |
| RBAC | Role-Based Access Control |
| RDS | Relational Database Service - AWS managed database |
| RTO | Recovery Time Objective - max acceptable downtime |
| RPO | Recovery Point Objective - max acceptable data loss |
| SLI | Service Level Indicator - metric for service quality |
| SLO | Service Level Objective - target for SLI |
| SSO | Single Sign-On - unified authentication |
| TTL | Time To Live - cache/data expiration |
| WAF | Web Application Firewall |

---

## Appendix B: References

### Internal Documents
- Globex IT Security Policy v2.3
- Globex Cloud Architecture Standards
- TacoTracker Product Requirements Document
- TacoTracker Business Requirements Document

### External Standards
- [C4 Model](https://c4model.com/)
- [OWASP Top 10](https://owasp.org/Top10/)
- [12-Factor App](https://12factor.net/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)

### Vendor Documentation
- [Stripe API Documentation](https://stripe.com/docs/api)
- [Okta Developer Documentation](https://developer.okta.com/)
- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging)
- [React Native Documentation](https://reactnative.dev/)

---

## Appendix C: Diagram Legend

### Box Styles
```
┌─────────────┐
│   Solid     │  = System/Container/Component
└─────────────┘

╔═════════════╗
║   Double    ║  = External System / System Boundary
╚═════════════╝

┌ ─ ─ ─ ─ ─ ─┐
    Dashed     = Optional / Future Component
└ ─ ─ ─ ─ ─ ─┘
```

### Arrow Styles
```
────────────►   = Synchronous communication
═══════════►    = Asynchronous communication / Event
- - - - - -►    = Optional / Conditional flow
◄───────────►   = Bidirectional communication
```

### Color Coding (when applicable)
- **Blue**: User-facing components
- **Green**: Core business services
- **Orange**: Data stores
- **Red**: Security components
- **Gray**: Infrastructure/Platform

---

*Document End - TacoTracker 3000 High-Level Architecture Document v1.0*
