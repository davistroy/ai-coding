# Business Requirements Document (BRD)

## Project: TacoTracker 3000
### Enterprise Taco Truck Tracking and Ordering System

**Document Version:** 1.0
**Date:** January 22, 2026
**Author:** Business Analysis Team
**Status:** Approved for Development

---

## 1. Executive Summary

### 1.1 Business Justification

Globex Corporation's 2,500 employees across three campus buildings face a critical challenge: the unpredictable nature of visiting food trucks, particularly the beloved "Taco Tornado" truck, results in an estimated 847 hours of lost productivity per month. Employees wander parking lots searching for trucks, stand in excessively long lines due to poor timing, and frequently miss trucks entirely—leading to what HR has formally classified as "Taco-Related Disappointment Syndrome" (TRDS).

TacoTracker 3000 will revolutionize how Globex employees interact with food trucks by providing real-time tracking, predictive arrival notifications, mobile pre-ordering capabilities, and intelligent queue management. This enterprise-grade solution transforms chaotic taco acquisition into a streamlined, data-driven experience.

### 1.2 Expected Benefits

| Benefit Category | Projected Impact |
|-----------------|------------------|
| Productivity Recovery | 640 hours/month ($48,000/month in recovered wages) |
| Employee Satisfaction | 34% improvement in "Lunch Experience" survey scores |
| Food Truck Revenue | 28% increase (driving better truck attendance) |
| Reduced "Hangry Incidents" | 67% decrease in afternoon meeting conflicts |

### 1.3 Strategic Alignment

TacoTracker 3000 directly supports Globex's strategic initiatives:
- **Employee Experience Excellence**: Part of the "Happy Employees, Happy Profits" program
- **Digital Transformation**: Demonstrates innovative technology adoption
- **Sustainability Goals**: Reduces unnecessary walking (carbon footprint from heavy breathing)
- **Competitive Advantage**: Positions Globex as an employer of choice for taco enthusiasts

### 1.4 Investment Summary

- **Estimated Development Cost:** $127,000
- **Annual Operating Cost:** $18,000
- **Projected Annual Savings:** $576,000
- **ROI:** 354% (Year 1)
- **Payback Period:** 2.6 months

---

## 2. Project Overview

### 2.1 Project Background

The food truck phenomenon at Globex began in 2019 when "Taco Tornado" started visiting the main campus. What began as a weekly Tuesday occurrence has evolved into a complex ecosystem of 12 rotating food trucks across three campuses. The informal "Food Truck Slack Channel" (#where-is-the-taco-truck) has become the third most active channel in the company, surpassing #engineering-announcements and #actual-work-stuff.

In Q3 2025, the Employee Experience Committee commissioned a study revealing:
- Average employee spends 23 minutes per food truck visit (vs. 8-minute optimal)
- 34% of attempted food truck visits result in "truck not found" disappointment
- The infamous "Taco Tuesday Stampede" of September 2025 resulted in three sprained ankles
- 12% of employees have created personal spreadsheets to track truck patterns

### 2.2 Business Problem Statement

**Primary Problem:** Employees lack real-time visibility into food truck locations, arrival times, and wait times, resulting in:

1. **Productivity Loss**: Employees spend excessive time searching for and waiting at food trucks
2. **Missed Opportunities**: Trucks frequently depart before interested employees are aware of their presence
3. **Inequitable Access**: Employees in Building C (furthest from main lot) consistently miss peak truck times
4. **Vendor Frustration**: Food truck operators cannot predict demand, leading to food waste and stockouts
5. **Safety Concerns**: Large crowds forming suddenly create pedestrian hazards in parking areas

**Secondary Problems:**
- No centralized menu information (employees rely on outdated photos in Slack)
- Cash-only trucks create friction (ATM is in Building A basement)
- Dietary restriction filtering is impossible without visiting each truck
- New employees have no way to discover the food truck culture

### 2.3 Project Objectives

| ID | Objective | Success Metric | Target |
|----|-----------|----------------|--------|
| OBJ-1 | Reduce time spent locating food trucks | Average search time | < 2 minutes |
| OBJ-2 | Increase successful food truck visits | Visit success rate | > 95% |
| OBJ-3 | Decrease average wait time in line | Queue time | < 5 minutes |
| OBJ-4 | Improve employee satisfaction with lunch options | Survey score | > 4.2/5.0 |
| OBJ-5 | Increase food truck vendor revenue | Monthly revenue per truck | +25% |
| OBJ-6 | Reduce food waste from demand unpredictability | Waste percentage | < 8% |

### 2.4 Success Criteria

The project will be considered successful when:

1. **Adoption**: 70% of employees have downloaded and used the app within 60 days of launch
2. **Engagement**: Average of 500+ daily active users during lunch hours (11 AM - 2 PM)
3. **Efficiency**: Documented 50% reduction in food truck-related Slack messages
4. **Satisfaction**: Post-implementation survey shows 80%+ satisfaction rating
5. **Vendor Participation**: 100% of regular food trucks onboarded within 90 days
6. **Zero Safety Incidents**: No crowd-related injuries during pilot period

### 2.5 Project Scope and Boundaries

#### In Scope
- Mobile application (iOS and Android) for employees
- Web-based vendor portal for food truck operators
- Real-time GPS tracking integration
- Push notification system for truck arrivals
- Mobile ordering and payment processing
- Queue management and wait time estimation
- Menu management with dietary filtering
- Analytics dashboard for facilities management
- Integration with Globex employee directory (SSO)
- Support for all three campus locations

#### Out of Scope
- Delivery services (employees must walk to trucks)
- Integration with external food delivery apps (DoorDash, etc.)
- Catering or bulk ordering capabilities
- Non-food-truck vendors (cafeteria, vending machines)
- Personal vehicle food runs coordination
- Drone-based taco delivery (Phase 2 consideration)

#### Future Scope (Phase 2+)
- Loyalty and rewards program
- Social features (group ordering, reviews)
- AI-powered taste preference learning
- Integration with calendar for meeting-aware suggestions
- Expansion to partner company campuses

### 2.6 Assumptions and Constraints

#### Assumptions
1. Food truck operators will agree to use GPS tracking devices or the vendor app
2. Employees have access to personal smartphones during work hours
3. Campus WiFi coverage is sufficient in parking areas
4. Food truck operators can provide digital menus
5. Existing payment processors (Stripe) can be integrated
6. Legal has approved employee location data collection (for crowding estimates only)
7. Food trucks will continue visiting campus for the foreseeable future

#### Constraints
1. **Budget**: Total development budget capped at $150,000
2. **Timeline**: Must launch before "Summer Food Truck Festival" (June 1, 2026)
3. **Technology**: Must integrate with existing Okta SSO infrastructure
4. **Privacy**: Cannot track individual employee locations, only aggregate foot traffic
5. **Vendor**: Cannot require vendors to purchase hardware costing more than $50
6. **Compliance**: Must meet WCAG 2.1 AA accessibility standards
7. **Support**: IT can dedicate only 0.5 FTE to ongoing maintenance

---

## 3. Stakeholder Analysis

### 3.1 Primary Stakeholders

#### Executive Sponsor
**Name:** Patricia Vaughn, VP of Employee Experience
**Role:** Project champion and budget authority
**Interest:** Demonstrating innovative employee programs; Winning "Best Workplace" award
**Influence:** High - controls budget and organizational priority
**Concerns:** ROI justification, timeline alignment with Summer Festival
**Communication:** Bi-weekly executive summary, monthly steering committee

#### Product Owner
**Name:** Marcus Chen, Employee Experience Manager
**Role:** Day-to-day decision maker, requirements prioritization
**Interest:** Successful delivery, employee adoption
**Influence:** High - controls feature prioritization
**Concerns:** Scope creep, vendor cooperation, user adoption
**Communication:** Daily standups, weekly backlog grooming

#### Primary Users: Globex Employees
**Population:** 2,500 employees across three buildings
**Segments:**
- Power Users (15%): Visit food trucks 3+ times per week
- Regular Users (45%): Visit food trucks 1-2 times per week
- Occasional Users (30%): Visit food trucks 1-3 times per month
- Non-Users (10%): Bring lunch or use cafeteria exclusively

### 3.2 Secondary Stakeholders

| Stakeholder | Interest | Influence | Engagement Strategy |
|-------------|----------|-----------|---------------------|
| Food Truck Operators | Increased revenue, demand visibility | Medium | Vendor advisory council, revenue sharing model |
| Facilities Management | Safety, parking lot logistics | Medium | Weekly coordination meetings |
| IT Security | Data privacy, system security | Medium | Security review gates |
| Finance | Payment processing, expense tracking | Low | Monthly reconciliation reports |
| Legal/Compliance | Terms of service, liability | Low | Contract review, privacy policy approval |
| HR | Employee policy, benefits integration | Low | Policy alignment review |
| Building Security | Access control, crowd management | Low | Incident protocol coordination |

### 3.3 Stakeholder Roles and Responsibilities (RACI Matrix)

| Activity | Patricia (Sponsor) | Marcus (PO) | IT | Facilities | Vendors |
|----------|-------------------|-------------|-----|------------|---------|
| Requirements Approval | A | R | C | C | I |
| Budget Allocation | A | R | I | I | I |
| Vendor Onboarding | I | A | C | C | R |
| Technical Architecture | I | C | R/A | I | I |
| User Acceptance Testing | I | A | C | R | R |
| Go-Live Decision | A | R | C | C | I |
| Ongoing Operations | I | A | R | C | R |

*R = Responsible, A = Accountable, C = Consulted, I = Informed*

### 3.4 Communication Plan

| Audience | Content | Frequency | Channel | Owner |
|----------|---------|-----------|---------|-------|
| Executive Team | Status dashboard, risks, decisions needed | Bi-weekly | Email + Meeting | Marcus |
| Project Team | Sprint progress, blockers | Daily | Slack + Standup | Scrum Master |
| All Employees | Feature previews, launch updates | Monthly | All-hands, Newsletter | Comms Team |
| Food Truck Vendors | Onboarding info, feature updates | Weekly | Email + Portal | Marcus |
| IT Security | Security assessments, compliance | As needed | Jira tickets | Tech Lead |

### 3.5 Change Management Considerations

**Anticipated Resistance:**
1. **Vendors**: "Another app to manage" → Mitigation: Demonstrate revenue increase, minimize required effort
2. **Privacy-conscious employees**: "Tracking my taco habits" → Mitigation: Clear privacy policy, optional features
3. **Cash-preferred users**: "I don't want to link payment" → Mitigation: Cash remains an option, app is for info only
4. **Technology-averse employees**: "I just want a taco" → Mitigation: QR code alternatives, simple UI

**Adoption Strategy:**
- Soft launch with "Taco Pioneers" beta group (50 enthusiastic volunteers)
- Gamification: "First 500 users get free taco coupon"
- Food truck vendor incentives for promoting app adoption
- IT Help Desk training for support questions
- Physical signage at truck locations with QR codes

---

## 4. Business Context and Environment

### 4.1 Current State Analysis

#### Current Process: Finding and Ordering from Food Trucks

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        CURRENT STATE: TACO ACQUISITION                       │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  Employee                                                                    │
│     │                                                                        │
│     ▼                                                                        │
│  ┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐    │
│  │ Check Slack for  │────▶│ Walk to Parking  │────▶│ Truck Present?   │    │
│  │ truck sightings  │     │ Lot A            │     │                  │    │
│  └──────────────────┘     └──────────────────┘     └────────┬─────────┘    │
│         │                                                    │              │
│         │ No Slack posts                              No     │    Yes       │
│         ▼                                                    ▼              │
│  ┌──────────────────┐                              ┌──────────────────┐    │
│  │ Walk to all 3    │                              │ Check other lots │    │
│  │ parking lots     │                              │ (B, then C)      │    │
│  └──────────────────┘                              └──────────────────┘    │
│         │                                                    │              │
│         ▼                                                    ▼              │
│  ┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐    │
│  │ Stand in line    │────▶│ Wait 10-25 min   │────▶│ Order verbally   │    │
│  │ (if truck found) │     │ (no estimate)    │     │ (hope they have  │    │
│  └──────────────────┘     └──────────────────┘     │  what you want)  │    │
│                                                    └──────────────────┘    │
│                                                             │               │
│                                                             ▼               │
│                                                    ┌──────────────────┐    │
│                                                    │ Pay (cash only   │    │
│                                                    │ for some trucks) │    │
│                                                    └──────────────────┘    │
│                                                                             │
│  PAIN POINTS: ⚠️  No visibility  ⚠️  Wasted trips  ⚠️  Long waits         │
│               ⚠️  No menus      ⚠️  Cash friction  ⚠️  Stockouts          │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Quantified Pain Points

| Pain Point | Frequency | Impact | Annual Cost |
|------------|-----------|--------|-------------|
| Failed truck searches | 340/month | 15 min wasted each | $51,000 |
| Excessive wait times | 2,100 visits/month | 12 min over optimal | $252,000 |
| Missed trucks (arrived too late) | 180/month | Disappointment + 20 min | $36,000 |
| Stockouts (wanted item unavailable) | 95/month | Partial satisfaction | $9,500 |
| Cash-only friction | 420/month | 8 min ATM trip | $33,600 |
| **Total Annual Impact** | | | **$382,100** |

*Calculations based on average fully-loaded employee cost of $50/hour*

### 4.2 Future State Vision

#### Future Process: TacoTracker 3000 Experience

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     FUTURE STATE: TACOTRACKER 3000                          │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  Employee                                                                    │
│     │                                                                        │
│     ▼                                                                        │
│  ┌──────────────────┐                                                       │
│  │ 📱 Receive push  │  "Taco Tornado arriving at Lot B in 15 min!"         │
│  │ notification     │  "Current pre-orders: 12 | Est. wait: 4 min"         │
│  └────────┬─────────┘                                                       │
│           │                                                                  │
│           ▼                                                                  │
│  ┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐    │
│  │ Open app, view   │────▶│ Browse menu with │────▶│ Place order &    │    │
│  │ real-time map    │     │ dietary filters  │     │ pay in-app       │    │
│  └──────────────────┘     └──────────────────┘     └────────┬─────────┘    │
│                                                              │               │
│                                                              ▼               │
│  ┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐    │
│  │ 📱 "Order ready  │◀────│ Walk directly to │◀────│ Receive order    │    │
│  │ for pickup!"     │     │ correct lot      │     │ confirmation     │    │
│  └──────────────────┘     └──────────────────┘     └──────────────────┘    │
│           │                                                                  │
│           ▼                                                                  │
│  ┌──────────────────┐                                                       │
│  │ Skip line, grab  │  Total time: 8 minutes (vs. 23 minutes today)        │
│  │ pre-paid order   │  Satisfaction: Maximum 🌮                             │
│  └──────────────────┘                                                       │
│                                                                              │
│  BENEFITS: ✅ Real-time visibility  ✅ Pre-ordering  ✅ Mobile payment      │
│            ✅ Wait time estimates   ✅ Menu browsing  ✅ Notifications       │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Future State Success Indicators

| Indicator | Current | Target | Measurement Method |
|-----------|---------|--------|-------------------|
| Avg. time to acquire food | 23 min | 8 min | App analytics |
| Visit success rate | 66% | 98% | App analytics |
| Employee lunch satisfaction | 3.1/5.0 | 4.3/5.0 | Quarterly survey |
| Food truck revenue per visit | $11.50 | $14.75 | Vendor reporting |
| Food waste | 18% | 7% | Vendor reporting |
| Slack #food-truck messages | 450/day | 50/day | Slack analytics |

### 4.3 Gap Analysis

| Gap Area | Current State | Future State | Complexity | Priority |
|----------|---------------|--------------|------------|----------|
| Location Awareness | None (word of mouth) | Real-time GPS tracking | Medium | Critical |
| Arrival Prediction | None | ML-based ETA with notifications | High | Critical |
| Menu Information | Photos in Slack (outdated) | Digital menus with allergen info | Low | High |
| Ordering | Verbal, in-person only | Mobile pre-ordering | Medium | High |
| Payment | Cash or card at truck | In-app payment | Medium | High |
| Wait Time | Unknown until arrival | Real-time queue estimates | Medium | High |
| Demand Forecasting | None (vendors guess) | Historical analytics + pre-orders | High | Medium |
| Dietary Filtering | Manual label reading | Searchable dietary tags | Low | Medium |

### 4.4 Business Process Flows

#### Pre-Order Process Flow

1. **Notification Trigger**: System detects truck GPS entering 5-mile radius
2. **User Alert**: Push notification sent to users who favorited this truck
3. **Menu Load**: Current day's menu loaded from vendor portal
4. **Order Placement**: User selects items, customizations, confirms order
5. **Payment Processing**: Stripe processes payment, order confirmed
6. **Queue Assignment**: Order assigned pickup time slot (5-min windows)
7. **Vendor Receipt**: Order appears on vendor tablet with prep countdown
8. **Ready Alert**: Vendor marks order complete, user notified
9. **Pickup**: User shows QR code, receives order
10. **Completion**: Both parties confirm handoff, transaction closed

### 4.5 Competitive Analysis

While no direct competitor exists for corporate food truck management, comparable solutions include:

| Solution | Strengths | Weaknesses | TacoTracker Advantage |
|----------|-----------|------------|----------------------|
| StreetFoodFinder (Public App) | Large truck database | No corporate features, no ordering | Enterprise integration, pre-ordering |
| Slack Channel (Current) | Free, familiar | Unreliable, no structure | Real-time accuracy, rich features |
| Corporate Cafeteria | Consistent, nearby | Limited variety, "boring" | Variety, excitement, culture |
| DoorDash/UberEats | Convenient delivery | Expensive fees, no truck support | Direct vendor relationship, no fees |

---

## 5. Functional Requirements

### 5.1 Core Business Functions

#### Priority 1: Critical (MVP)

| ID | Function | Description | Acceptance Criteria |
|----|----------|-------------|---------------------|
| FR-1.1 | Real-time Truck Tracking | Display current location of all active food trucks on campus map | Location updates every 30 seconds with 10m accuracy |
| FR-1.2 | Arrival Notifications | Push notifications when favorited trucks arrive or are en route | Notification delivered within 60 seconds of trigger event |
| FR-1.3 | Digital Menu Display | Show current menu with prices for each truck | Menu loads in < 2 seconds, reflects same-day updates |
| FR-1.4 | Wait Time Estimation | Display estimated current wait time at each truck | Estimate within 3 minutes of actual 80% of the time |
| FR-1.5 | Employee Authentication | SSO login via existing Okta infrastructure | Seamless login, no separate credentials required |

#### Priority 2: Important (Launch)

| ID | Function | Description | Acceptance Criteria |
|----|----------|-------------|---------------------|
| FR-2.1 | Mobile Pre-Ordering | Place orders in advance for pickup | Order confirmed within 10 seconds, synced to vendor |
| FR-2.2 | In-App Payment | Process payments via saved cards | PCI-compliant, < 3 second transaction time |
| FR-2.3 | Dietary Filtering | Filter menus by dietary restrictions | Support for: vegetarian, vegan, gluten-free, nut-free, halal, kosher |
| FR-2.4 | Favorite Trucks | Save preferred trucks for quick access and notifications | Unlimited favorites, instant notification opt-in |
| FR-2.5 | Order History | View past orders for easy reordering | Display last 50 orders with one-tap reorder |

#### Priority 3: Enhancement (Post-Launch)

| ID | Function | Description | Acceptance Criteria |
|----|----------|-------------|---------------------|
| FR-3.1 | Group Ordering | Coordinate orders among colleagues | Support groups up to 10, single payment option |
| FR-3.2 | Ratings and Reviews | Rate trucks and menu items | 5-star rating, optional text review |
| FR-3.3 | Schedule View | See weekly truck schedule | 7-day forward visibility, calendar sync option |
| FR-3.4 | Crowd Density | Show current crowding at each location | Heat map visualization, "best time to go" suggestions |

### 5.2 User Stories and Use Cases

#### Epic: Food Truck Discovery

**US-1.1: View Active Trucks**
> As a hungry employee, I want to see which food trucks are currently on campus so that I can decide where to eat.

*Acceptance Criteria:*
- Map view shows all three campus lots with truck pins
- List view shows truck name, cuisine type, distance from user
- Trucks shown only during their operating hours
- "No trucks available" message when campus is empty

**US-1.2: Truck Arrival Alerts**
> As a Taco Tornado fan, I want to be notified when my favorite truck is arriving so that I don't miss it.

*Acceptance Criteria:*
- Notification sent 15 minutes before predicted arrival
- Notification sent upon actual arrival (GPS confirmation)
- User can customize notification timing preferences
- User can mute notifications for specific days

#### Epic: Ordering Experience

**US-2.1: Browse Menu**
> As someone with dietary restrictions, I want to filter the menu by allergens so that I can safely choose what to eat.

*Acceptance Criteria:*
- Each menu item displays allergen badges (contains: gluten, nuts, dairy, etc.)
- Filter toggle for each major dietary restriction
- "Safe for you" indicator based on saved preferences
- Ingredient list available for each item on detail tap

**US-2.2: Place Pre-Order**
> As a busy professional, I want to order and pay in advance so that I can minimize my time away from work.

*Acceptance Criteria:*
- Select items, customize (extra salsa, no onions, etc.)
- See order total including tax before confirming
- Receive estimated pickup time after ordering
- Receive push notification when order is ready
- Show QR code for order retrieval

**US-2.3: Quick Reorder**
> As a creature of habit, I want to reorder my usual with one tap so that I don't waste time.

*Acceptance Criteria:*
- "Recent Orders" prominently displayed on home screen
- "Reorder" button on each past order
- Verify item availability before reorder confirmation
- Alert if prices have changed since last order

### 5.3 Business Rules

| Rule ID | Rule Description | Enforcement |
|---------|------------------|-------------|
| BR-001 | Orders cannot be placed more than 60 minutes before truck arrival | System prevents early orders |
| BR-002 | Orders cannot be modified after vendor confirms receipt (typically 2 min) | Modification locked, cancel only |
| BR-003 | Refunds for canceled orders issued within 24 hours | Automatic via payment processor |
| BR-004 | Users limited to one active pre-order per truck at a time | System prevents duplicate orders |
| BR-005 | Wait time estimates recalculate every 60 seconds | Automated background process |
| BR-006 | Vendors must update menu by 9 AM for same-day accuracy | Portal reminder, stale menu warning |
| BR-007 | Trucks not updating GPS for 10+ minutes shown as "Location Unknown" | Automatic status change |
| BR-008 | Pre-orders close when vendor has 10+ orders in queue | Automatic cutoff, walk-up only |

### 5.4 Data Requirements

#### Data to Capture
- User profiles (linked from Okta): name, email, building, department
- User preferences: favorite trucks, dietary restrictions, notification settings
- Order history: items, timestamps, amounts, vendor, location
- Vendor data: truck name, cuisine type, menu items, pricing, schedule
- Location data: truck GPS coordinates, timestamp
- Queue data: current wait time, orders in queue

#### Data to Process
- Arrival predictions based on GPS trajectory and historical patterns
- Wait time calculations based on queue depth and historical fulfillment rates
- Popular items analysis for vendor insights
- Peak hour identification for facilities planning

#### Data to Report
- Daily/weekly order volumes by truck
- Average wait times by time of day
- User adoption and engagement metrics
- Revenue per truck, revenue per user
- Most popular menu items

### 5.5 Integration Requirements

| System | Integration Type | Data Flow | Frequency |
|--------|------------------|-----------|-----------|
| Okta SSO | Authentication | User identity → App | Each login |
| Stripe | Payment Processing | Payment info ↔ App | Each transaction |
| GPS Provider (vendor devices) | Location Data | Coordinates → App | Every 30 seconds |
| Globex Email | Notifications | App → User | Event-triggered |
| Slack (optional) | Status Updates | App → Channel | Configurable |

### 5.6 Reporting and Analytics

#### Employee-Facing Reports
- Personal order history with spending summary
- "Your taco stats" (fun annual summary: "You ate 47 tacos this year!")

#### Vendor-Facing Reports
- Daily sales summary
- Popular items ranking
- Peak ordering times
- Pre-order vs. walk-up ratio

#### Management Dashboard
- Overall adoption metrics (DAU, MAU, retention)
- Aggregate spending by department (for team lunch budgets)
- Wait time trends
- Vendor performance scorecards
- Food waste reduction tracking

### 5.7 Workflow and Approval Processes

#### Vendor Onboarding Workflow
1. Vendor submits application via web portal
2. Facilities reviews and approves vendor eligibility
3. Vendor completes profile (menu, photos, schedule)
4. System administrator activates vendor account
5. Vendor receives GPS device and training materials
6. Vendor appears in app on next scheduled visit

#### Refund Request Workflow
1. User submits refund request via app (with reason)
2. If under $15 and first request this month: auto-approve
3. If over $15 or repeat: route to Marcus (Product Owner) for review
4. Approved refunds processed within 24 hours
5. User notified of outcome via push notification

---

## 6. Non-Functional Requirements

### 6.1 Performance Requirements

| Metric | Requirement | Measurement |
|--------|-------------|-------------|
| App Launch Time | < 3 seconds to interactive | 95th percentile |
| Map Load Time | < 2 seconds with all truck pins | 95th percentile |
| Order Submission | < 5 seconds end-to-end | 99th percentile |
| Push Notification Delivery | < 60 seconds from trigger | 95th percentile |
| Concurrent Users | Support 800 simultaneous users | Load test validated |
| GPS Update Processing | Handle 50 updates/second | Peak lunch hour |

### 6.2 Security Requirements

| Requirement | Description | Implementation |
|-------------|-------------|----------------|
| Authentication | Enterprise SSO via Okta | SAML 2.0 / OIDC |
| Authorization | Role-based access (user, vendor, admin) | App-level RBAC |
| Data Encryption | All data encrypted in transit and at rest | TLS 1.3, AES-256 |
| Payment Security | PCI-DSS compliant payment handling | Stripe tokenization |
| API Security | Rate limiting, API key authentication | 100 req/min per user |
| Audit Logging | Log all transactions and admin actions | 90-day retention |

### 6.3 Usability Requirements

| Requirement | Description |
|-------------|-------------|
| Accessibility | WCAG 2.1 AA compliance; screen reader support |
| Platform Support | iOS 14+, Android 10+, responsive web for vendors |
| Offline Mode | Show cached menu and last known locations when offline |
| Onboarding | < 30 seconds from download to first useful action |
| Error Handling | Friendly error messages with recovery suggestions |
| Language | English (primary), Spanish (Phase 2) |

### 6.4 Reliability and Availability

| Metric | Target | Notes |
|--------|--------|-------|
| Uptime | 99.5% during business hours | 8 AM - 6 PM local time |
| Planned Maintenance | Outside business hours only | Weekend nights preferred |
| Recovery Time Objective (RTO) | < 1 hour | For critical failures |
| Recovery Point Objective (RPO) | < 5 minutes | Transaction data |
| Backup Frequency | Every 6 hours | Full database backup |

### 6.5 Compliance and Regulatory

| Requirement | Standard | Applicability |
|-------------|----------|---------------|
| Payment Processing | PCI-DSS Level 4 | All card transactions |
| Accessibility | ADA / WCAG 2.1 AA | All user interfaces |
| Data Privacy | Internal Globex Privacy Policy | Employee data handling |
| Food Safety | Display vendor health ratings | Informational only |

### 6.6 Maintainability

| Requirement | Description |
|-------------|-------------|
| Code Documentation | All APIs documented via OpenAPI spec |
| Monitoring | Application performance monitoring (DataDog or equivalent) |
| Logging | Centralized logging with 90-day retention |
| Deployment | CI/CD pipeline with staging environment |
| Update Frequency | Mobile app updates no more than bi-weekly |

---

## 7. User Requirements

### 7.1 User Personas

#### Persona 1: "Taco Titan" Terrence
**Demographics:** Software Engineer, Building A, 28 years old
**Tech Savviness:** Expert
**Lunch Behavior:** Food truck 4x/week, always trying new items

**Goals:**
- Never miss Taco Tornado Tuesday
- Minimize time away from coding
- Discover new food trucks

**Pain Points:**
- Hates walking to lot and finding no truck
- Frustrated by long lines during peak hours
- Wishes he knew menu before arriving

**Quote:** "If I could pre-order and skip the line, I'd literally be 20% more productive."

---

#### Persona 2: "Cautious" Carol
**Demographics:** Finance Manager, Building C, 45 years old
**Tech Savviness:** Moderate
**Lunch Behavior:** Food truck 1x/week, has severe nut allergy

**Goals:**
- Safely identify allergen-free options
- Not waste time on trucks without safe options
- Simple, straightforward experience

**Pain Points:**
- Afraid of allergic reaction from cross-contamination
- Building C is far; often misses trucks
- Doesn't want to download "another app"

**Quote:** "I just need to know if it's safe for me to eat there. That's it."

---

#### Persona 3: "New Kid" Nancy
**Demographics:** Marketing Associate, Building B, 24 years old (started 2 months ago)
**Tech Savviness:** High
**Lunch Behavior:** Wants to participate but doesn't know the culture

**Goals:**
- Learn about food truck culture at Globex
- Meet colleagues over lunch
- Find vegetarian options

**Pain Points:**
- Doesn't know which trucks visit or when
- Feels out of the loop on Slack channel
- Hasn't found her "go-to" truck yet

**Quote:** "Everyone talks about Taco Tuesday but I've never actually found the truck."

---

#### Persona 4: "Vendor" Victor
**Demographics:** Owner of "Taco Tornado," 52 years old
**Tech Savviness:** Low-Moderate
**Business Model:** 3 corporate stops per day, Globex is largest

**Goals:**
- Increase revenue per stop
- Reduce food waste from overprep
- Minimize time spent on admin

**Pain Points:**
- Unpredictable demand leads to waste
- Long lines frustrate customers and slow service
- Hard to communicate specials or sellouts

**Quote:** "If I knew how many people were coming, I could prep the right amount and serve everyone faster."

### 7.2 User Journeys

#### Journey: Terrence's Perfect Taco Tuesday

| Stage | Action | Feeling | Touchpoint |
|-------|--------|---------|------------|
| **Trigger** | Receives "Taco Tornado 15 min away!" notification at 11:45 AM | Excited | Push notification |
| **Discovery** | Opens app, sees truck en route to Lot B | Informed | App map view |
| **Decision** | Checks menu, sees carnitas special is available | Satisfied | Menu screen |
| **Order** | Places pre-order: 2 carnitas tacos, extra lime | Efficient | Order flow |
| **Payment** | Apple Pay confirms $9.50 charge | Seamless | Payment confirmation |
| **Wait** | Continues coding, receives "Order ready!" at 12:08 | Productive | Push notification |
| **Pickup** | Shows QR code, grabs order, back to desk by 12:15 | Delighted | Physical pickup |
| **Post** | Rates 5 stars, shares "Carnitas was amazing" in Slack | Advocate | Rating prompt |

**Total Time:** 8 minutes away from desk (vs. 25+ minutes previously)

### 7.3 User Interface Requirements

| Requirement | Description |
|-------------|-------------|
| Primary Navigation | Bottom tab bar: Map, Order, Activity, Profile |
| Map Interaction | Pinch-zoom, tap truck for details, pull-up sheet for truck info |
| Order Flow | Maximum 4 taps from home to order confirmation |
| Visual Design | Align with Globex brand guidelines; vibrant food photography |
| Dark Mode | Support system-level dark mode preference |
| Haptics | Subtle feedback on order confirmation and notifications |

### 7.4 Training and Support Requirements

| User Type | Training Approach |
|-----------|-------------------|
| Employees | In-app onboarding tutorial (< 60 seconds), FAQ in help section |
| Vendors | 30-minute virtual training session, printed quick-start guide |
| IT Help Desk | 1-hour training session, internal knowledge base articles |

### 7.5 User Adoption Strategy

| Phase | Tactic | Target | Timing |
|-------|--------|--------|--------|
| Pre-Launch | Teaser campaign: "Something delicious is coming" | Awareness | 4 weeks before |
| Launch Week | "First 500 Users" free taco coupon | 500 downloads | Week 1 |
| Week 2-4 | Vendor promotions: "10% off for app orders" | Trial conversion | Weeks 2-4 |
| Ongoing | Monthly "Taco Champion" recognition | Engagement | Monthly |

---

## 8. Technical Context (Business Perspective)

### 8.1 System Architecture Overview

From a business perspective, TacoTracker 3000 consists of:

1. **Mobile Application**: Primary employee touchpoint (iOS + Android)
2. **Vendor Portal**: Web-based interface for food truck operators
3. **Admin Dashboard**: Internal management and analytics interface
4. **Backend Services**: Cloud-hosted APIs handling business logic
5. **GPS Integration**: Real-time location tracking infrastructure

**Business Rationale for Cloud Hosting:**
- Rapid deployment without infrastructure procurement delays
- Scalability to handle lunch rush traffic spikes
- Managed services reduce IT maintenance burden
- Disaster recovery built into platform

### 8.2 Technology Standards

| Component | Standard | Business Rationale |
|-----------|----------|-------------------|
| Mobile Development | React Native | Single codebase for iOS/Android reduces cost |
| Authentication | Okta SSO | Existing enterprise standard, no new credentials |
| Payment Processing | Stripe | Industry leader, fast onboarding, low fraud |
| Cloud Platform | AWS | Existing Globex relationship, volume discounts |
| Push Notifications | Firebase Cloud Messaging | Reliable, cross-platform, cost-effective |

### 8.3 Infrastructure Requirements

| Requirement | Business Need |
|-------------|---------------|
| Auto-scaling | Handle 5x normal load during lunch peak without slowdown |
| Multi-AZ deployment | No single point of failure; business continuity |
| CDN for static assets | Fast app experience regardless of network conditions |
| API rate limiting | Prevent abuse, ensure fair access for all users |

### 8.4 Third-Party Dependencies

| Vendor | Service | Business Risk | Mitigation |
|--------|---------|---------------|------------|
| Stripe | Payments | Payment disruption | Maintain backup processor option |
| Google Maps | Map display | App unusable without maps | Cache offline map tiles |
| Okta | Authentication | Users can't log in | Graceful degradation to cached session |
| AWS | Infrastructure | Complete outage | Multi-region design, disaster recovery |

### 8.5 Data Migration Strategy

**Migration Scope:** Minimal—this is a new system.

**Data to Seed:**
- Employee directory sync from Okta (automatic via SSO)
- Initial vendor profiles (manual entry during onboarding)
- Historical truck schedules (manual entry from Facilities records)

**No Legacy System Migration Required.**

---

## 9. Data Requirements

### 9.1 Data Sources and Ownership

| Data Type | Source | Owner | Steward |
|-----------|--------|-------|---------|
| Employee Profiles | Okta Directory | HR | IT Security |
| Order Transactions | TacoTracker App | Finance | Marcus (PO) |
| Menu Content | Vendor Portal | Vendors | Individual truck owners |
| GPS Coordinates | Vendor Devices | Facilities | Facilities Manager |
| User Preferences | TacoTracker App | Employees | Individual users |

### 9.2 Data Models

#### Key Business Entities

```
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│     USER        │       │     VENDOR      │       │    MENU_ITEM    │
├─────────────────┤       ├─────────────────┤       ├─────────────────┤
│ user_id         │       │ vendor_id       │       │ item_id         │
│ email           │       │ name            │       │ vendor_id (FK)  │
│ name            │       │ cuisine_type    │       │ name            │
│ building        │       │ description     │       │ description     │
│ dietary_prefs   │       │ logo_url        │       │ price           │
│ favorite_trucks │───┐   │ schedule        │   ┌───│ allergens       │
└─────────────────┘   │   └────────┬────────┘   │   │ available       │
                      │            │            │   └─────────────────┘
                      │            │            │
                      └────────────┼────────────┘
                                   │
┌─────────────────┐       ┌────────┴────────┐       ┌─────────────────┐
│     ORDER       │       │    LOCATION     │       │   ORDER_ITEM    │
├─────────────────┤       ├─────────────────┤       ├─────────────────┤
│ order_id        │       │ location_id     │       │ order_item_id   │
│ user_id (FK)    │       │ vendor_id (FK)  │       │ order_id (FK)   │
│ vendor_id (FK)  │       │ latitude        │       │ item_id (FK)    │
│ status          │       │ longitude       │       │ quantity        │
│ total_amount    │       │ timestamp       │       │ customizations  │
│ pickup_time     │       │ campus_lot      │       │ price           │
└─────────────────┘       └─────────────────┘       └─────────────────┘
```

### 9.3 Data Quality Requirements

| Data Element | Accuracy | Completeness | Timeliness |
|--------------|----------|--------------|------------|
| GPS Location | 10m radius | All active trucks | < 30 sec lag |
| Menu Items | 100% price accuracy | All available items | Same-day updates |
| Wait Time Estimate | ±3 min of actual | Always displayed | Real-time |
| Allergen Info | 100% (safety critical) | All items tagged | Verified quarterly |

### 9.4 Data Governance

| Policy | Description |
|--------|-------------|
| Retention | Order history: 2 years; Location data: 90 days; User accounts: duration of employment |
| Access | Users see own data only; Vendors see own truck data; Admins see anonymized aggregates |
| Deletion | Account deletion removes personal data within 30 days (legal hold excepted) |
| Anonymization | All analytics use anonymized, aggregated data |

### 9.5 Data Migration and Integration

**Ongoing Integrations:**
- **Okta → TacoTracker**: New employee provisioning, termination deprovisioning (daily sync)
- **TacoTracker → Finance Systems**: Transaction reports for expense reconciliation (weekly export)

### 9.6 Master Data Management

| Master Data | Source of Truth | Sync Mechanism |
|-------------|-----------------|----------------|
| Employee List | Okta | SCIM provisioning |
| Vendor Registry | TacoTracker Admin Portal | Manual entry |
| Menu Items | Vendor Portal | Vendor self-service |

---

## 10. Risk Analysis and Mitigation

### 10.1 Business Risks

| Risk ID | Risk Description | Likelihood | Impact | Score |
|---------|------------------|------------|--------|-------|
| BR-001 | Low employee adoption | Medium | High | High |
| BR-002 | Food trucks decline to participate | Medium | High | High |
| BR-003 | App perceived as "surveillance" | Low | Medium | Medium |
| BR-004 | Competing priorities delay project | Medium | Medium | Medium |

### 10.2 Technical Risks

| Risk ID | Risk Description | Likelihood | Impact | Score |
|---------|------------------|------------|--------|-------|
| TR-001 | GPS accuracy insufficient in urban canyon | Low | High | Medium |
| TR-002 | Payment processing integration delays | Medium | Medium | Medium |
| TR-003 | Performance issues during peak lunch | Medium | High | High |
| TR-004 | Third-party API changes break integration | Low | Medium | Low |

### 10.3 Resource Risks

| Risk ID | Risk Description | Likelihood | Impact | Score |
|---------|------------------|------------|--------|-------|
| RR-001 | Key developer leaves mid-project | Low | High | Medium |
| RR-002 | Budget overrun due to scope expansion | Medium | Medium | Medium |
| RR-003 | IT support bandwidth insufficient | Medium | Low | Low |

### 10.4 External Risks

| Risk ID | Risk Description | Likelihood | Impact | Score |
|---------|------------------|------------|--------|-------|
| ER-001 | Food truck industry regulation changes | Low | Medium | Low |
| ER-002 | Economic downturn reduces food truck visits | Low | Medium | Low |
| ER-003 | Competing app launched by food truck network | Low | Low | Low |

### 10.5 Risk Mitigation Strategies

| Risk ID | Mitigation Strategy |
|---------|---------------------|
| BR-001 | Gamification, incentives, executive promotion, beta user champions |
| BR-002 | Revenue sharing model, demonstrate demand data value, minimal burden |
| BR-003 | Clear privacy policy, no individual tracking, transparent data use |
| TR-001 | Manual check-in option, WiFi-based positioning backup |
| TR-003 | Load testing, auto-scaling, performance budget in development |
| RR-002 | Strict MVP scope, change control board, buffer in estimates |

### 10.6 Contingency Plans

| Scenario | Contingency Action |
|----------|-------------------|
| < 30% adoption at 60 days | Extend incentive program, mandatory all-hands demo, peer ambassador program |
| Key vendor (Taco Tornado) declines | Negotiate exclusivity benefits, escalate to executive sponsor |
| Launch deadline at risk | Reduce to MVP-only scope, defer P2/P3 features, add contractor resources |

### 10.7 Risk Monitoring

**Monthly Risk Review:**
- Product Owner reviews risk register monthly
- Escalate any "High" risks to Steering Committee
- Update likelihood/impact based on current project status

---

## 11. Requirements Traceability Matrix

| Req ID | Requirement | Business Objective | Stakeholder | Source | Priority | Dependencies |
|--------|-------------|-------------------|-------------|--------|----------|--------------|
| FR-1.1 | Real-time truck tracking | OBJ-1, OBJ-2 | All employees | Discovery interview | P1-Critical | GPS integration |
| FR-1.2 | Arrival notifications | OBJ-2 | Power users | User research | P1-Critical | FR-1.1, Push infrastructure |
| FR-1.3 | Digital menu display | OBJ-4 | All employees | User research | P1-Critical | Vendor onboarding |
| FR-1.4 | Wait time estimation | OBJ-3 | All employees | Discovery interview | P1-Critical | FR-1.1, Queue algorithm |
| FR-1.5 | Employee SSO | (Security) | IT Security | Constraint | P1-Critical | Okta integration |
| FR-2.1 | Mobile pre-ordering | OBJ-3, OBJ-5 | Power users, Vendors | Discovery interview | P2-Important | FR-1.3, FR-2.2 |
| FR-2.2 | In-app payment | OBJ-3 | Power users | User research | P2-Important | Stripe integration |
| FR-2.3 | Dietary filtering | OBJ-4 | Carol persona | User research | P2-Important | FR-1.3 |
| FR-2.4 | Favorite trucks | OBJ-2 | Power users | User research | P2-Important | FR-1.2 |
| FR-2.5 | Order history | OBJ-4 | Regular users | User research | P2-Important | FR-2.1 |

---

## 12. Implementation Considerations

### 12.1 Project Phases

| Phase | Scope | Key Milestones |
|-------|-------|----------------|
| **Phase 1: Foundation** | Backend infrastructure, Okta integration, basic GPS tracking | Architecture approved, Dev environment ready |
| **Phase 2: MVP** | Employee app (tracking, menus, notifications), Vendor portal (basic) | Internal alpha testing |
| **Phase 3: Ordering** | Pre-ordering, payment processing, queue management | Beta launch (50 users) |
| **Phase 4: Launch** | Full rollout, vendor onboarding, analytics dashboard | General availability |
| **Phase 5: Enhancement** | Group ordering, reviews, advanced analytics | Quarterly releases |

### 12.2 Change Management

| Change Area | Audience | Approach |
|-------------|----------|----------|
| New app adoption | All employees | Lunch-and-learn sessions, email campaign, Slack announcements |
| Vendor workflow change | Food truck operators | 1:1 onboarding, printed guides, dedicated support line |
| IT support processes | Help Desk | Training session, runbook documentation, escalation path |

### 12.3 Training and Communication

**Pre-Launch (T-4 weeks):**
- Teaser campaign: "The future of lunch is coming"
- Executive sponsor video announcement
- Vendor recruitment and onboarding begins

**Launch Week:**
- All-hands announcement with live demo
- "Download Day" with on-site support tables
- Free taco vouchers for first users

**Ongoing:**
- Monthly feature highlight emails
- Quarterly "Taco Champion" recognition
- In-app tips and new feature callouts

### 12.4 Testing Strategy

| Test Type | Scope | Responsibility | Timing |
|-----------|-------|----------------|--------|
| Unit Testing | All code modules | Development team | Continuous |
| Integration Testing | API, third-party services | Development team | Each sprint |
| User Acceptance Testing | End-to-end scenarios | Product Owner + Beta users | Phase 3-4 |
| Load Testing | Concurrent users, peak simulation | DevOps | Pre-launch |
| Security Testing | Penetration testing, vulnerability scan | IT Security | Pre-launch |

### 12.5 Go-Live Considerations

**Go-Live Checklist:**
- [ ] All P1 and P2 requirements validated
- [ ] Load test passed (800 concurrent users)
- [ ] Security review approved
- [ ] Vendor onboarding complete (minimum 5 trucks)
- [ ] Support documentation published
- [ ] Rollback plan tested
- [ ] Executive sponsor sign-off

**Rollout Strategy:**
- **Week 1**: Building A only (closest to Lot A, highest food truck usage)
- **Week 2**: Add Building B
- **Week 3**: Add Building C, full campus availability
- **Week 4**: Vendor incentive promotions begin

---

## 13. Appendices

### Appendix A: Glossary

| Term | Definition |
|------|------------|
| **Campus** | Globex corporate campus, consisting of Buildings A, B, and C |
| **Food Truck** | Mobile food vendor operating from a vehicle |
| **Pre-Order** | Order placed via app before arriving at truck for expedited pickup |
| **Walk-Up Order** | Traditional in-person ordering at the truck |
| **Lot** | Parking lot where food trucks are permitted to operate (Lots A, B, C) |
| **Power User** | Employee who visits food trucks 3+ times per week |
| **TRDS** | Taco-Related Disappointment Syndrome (informal term for missed truck frustration) |
| **Queue Depth** | Number of orders awaiting fulfillment at a given truck |
| **Taco Tornado** | Most popular food truck vendor, visits Tuesdays and Thursdays |

### Appendix B: Reference Materials

- Globex Employee Experience Survey 2025 (Lunch Satisfaction Section)
- Globex Brand Guidelines v3.2
- Globex Information Security Policy
- Food Truck Vendor Agreements (Facilities Management)
- Okta SSO Integration Guide
- Stripe Payment Integration Documentation

### Appendix C: Stakeholder Interview Summaries

**Interview: Patricia Vaughn (Executive Sponsor)**
- Primary motivation: "Winning Best Workplace award requires innovative perks"
- Success metric: "If employees are raving about this in exit interviews, we've won"
- Concern: "Don't let this become a six-month project that misses summer"

**Interview: Marcus Chen (Product Owner)**
- Primary motivation: "Solve a real pain point that affects me personally every week"
- Success metric: "Slack #food-truck channel becomes a ghost town"
- Concern: "Vendors need to actually use it or the whole thing falls apart"

**Interview: Victor (Taco Tornado Owner)**
- Primary motivation: "I throw away $200 of food every week from wrong guesses"
- Success metric: "Knowing how many people are coming so I prep the right amount"
- Concern: "I don't have time to fiddle with complicated technology"

**Interview: Focus Group (10 Employees)**
- Top request: "Just tell me where the trucks are right now"
- Second request: "Let me see the menu before I walk over"
- Third request: "Pre-order so I can skip the line"
- Surprise insight: "I'd pay a small fee to skip the line" (not implementing, but noted)

### Appendix D: Sample Data

**Sample Menu Item:**
```json
{
  "item_id": "tt-001",
  "name": "Carnitas Street Taco",
  "description": "Slow-roasted pork with cilantro, onion, and house salsa verde",
  "price": 4.50,
  "category": "Tacos",
  "allergens": ["gluten"],
  "dietary_tags": ["dairy-free"],
  "customizations": [
    {"name": "Extra lime", "price": 0},
    {"name": "Extra salsa", "price": 0.50},
    {"name": "No cilantro", "price": 0}
  ],
  "image_url": "https://cdn.tacotracker.globex.com/items/tt-001.jpg",
  "available": true
}
```

### Appendix E: Requirements Change Log

| Date | Change | Requested By | Disposition |
|------|--------|--------------|-------------|
| 2026-01-10 | Add drone delivery feature | Engineering (joke) | Rejected (out of scope) |
| 2026-01-15 | Add calorie information to menu | HR Wellness | Approved for Phase 2 |
| 2026-01-18 | Integrate with expense system | Finance | Deferred to Phase 3 |

---

## Document Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Executive Sponsor | Patricia Vaughn | _________________ | ________ |
| Product Owner | Marcus Chen | _________________ | ________ |
| IT Representative | [TBD] | _________________ | ________ |
| Facilities Representative | [TBD] | _________________ | ________ |

---

*This document serves as the definitive business requirements for the TacoTracker 3000 project. Any changes to requirements must follow the change control process outlined in Appendix E.*

**Document Control:**
- **Location:** SharePoint > Projects > TacoTracker 3000 > Documentation
- **Review Cycle:** Monthly during development, quarterly post-launch
- **Distribution:** Project team, Steering Committee, Vendor Advisory Council

---

*"Because no one should have to wonder where the tacos are."*
— TacoTracker 3000 Project Team
