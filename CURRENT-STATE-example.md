# Current State Business Process Assessment

## Process Area: Food Truck Operations and Employee Lunch Acquisition
### Globex Corporation Campus Food Services

**Document Version:** 1.0
**Date:** January 22, 2026
**Author:** Business Process Analysis Team
**Status:** Approved
**Assessment Period:** October 2025 - December 2025

---

## 1. Executive Summary

### Assessment Overview

This Current State Assessment documents the existing business processes surrounding food truck operations across Globex Corporation's three-campus facility. The assessment was triggered by the Employee Experience Committee's Q3 2025 "Lunch Satisfaction Initiative" after the infamous "Taco Tuesday Stampede" incident resulted in three sprained ankles and a formal HR investigation.

### Key Findings

| Finding | Severity | Business Impact |
|---------|----------|-----------------|
| No centralized food truck visibility | Critical | 847 hours/month lost productivity |
| Informal communication channels overwhelmed | High | #where-is-the-taco-truck is 3rd most active Slack channel |
| Cash-only payment friction | Medium | 420 unnecessary ATM trips/month |
| Safety concerns from crowd surges | High | 3 injuries in past 6 months |
| Inequitable access across buildings | Medium | Building C employees miss 67% of truck visits |

### Current Performance Summary

| Metric | Current State | Industry Benchmark | Gap |
|--------|---------------|-------------------|-----|
| Average time to locate truck | 23 minutes | 5 minutes | -18 min |
| Successful truck visit rate | 66% | 95% | -29% |
| Average queue wait time | 17 minutes | 5 minutes | -12 min |
| Employee lunch satisfaction | 2.8/5.0 | 4.2/5.0 | -1.4 |

### Strategic Recommendations

1. **Immediate**: Implement real-time truck tracking and notification system
2. **Short-term**: Develop mobile ordering and payment capabilities
3. **Medium-term**: Deploy queue management and demand forecasting
4. **Long-term**: Create integrated food services ecosystem

---

## 2. Assessment Overview

### 2.1 Purpose and Objectives

**Assessment Objectives:**
- Document all processes related to food truck operations and employee food acquisition
- Quantify current performance baselines for future improvement measurement
- Identify pain points, inefficiencies, and safety concerns
- Establish foundation for TacoTracker 3000 requirements development

**Business Drivers:**
- Employee Experience Committee mandate following satisfaction survey decline
- Safety incident investigation recommendations
- CFO inquiry into "why employees spend so much time looking for tacos"
- Food truck vendor complaints about unpredictable demand

**Intended Use:**
- Input to Business Requirements Document (BRD) for TacoTracker 3000
- Baseline for measuring post-implementation improvements
- Evidence for budget justification ($127,000 requested)

### 2.2 Scope Definition

| Dimension | In Scope | Out of Scope |
|-----------|----------|--------------|
| **Processes** | Food truck discovery, queue management, ordering, payment, vendor coordination | Cafeteria operations, vending machines, catering |
| **Business Units** | All employees (2,500), Facilities, Food truck vendors (12) | External visitors, contractors |
| **Locations** | Buildings A, B, C; Parking Lots A, B, C | Off-campus locations, remote workers |
| **Systems** | Slack, Email, Personal spreadsheets, Cash/Card payments | HR systems, Payroll, Building access |
| **Time Period** | Current state as of December 2025 | Historical state prior to 2023 |

### 2.3 Methodology

| Phase | Activities | Outputs |
|-------|------------|---------|
| Discovery | 47 employee interviews, 12 vendor interviews, 3 weeks of observation | Interview transcripts, observation logs |
| Documentation | Process mapping workshops, system inventory | Process flows, SIPOC diagrams |
| Validation | Stakeholder review sessions, metric verification | Validated documentation |
| Analysis | Pain point prioritization, gap analysis | Findings and recommendations |

### 2.4 Stakeholders Engaged

| Stakeholder | Role | Contribution |
|-------------|------|--------------|
| Patricia Vaughn | VP Employee Experience | Executive sponsorship, strategic context |
| Marcus Chen | Employee Experience Manager | Process ownership, employee insights |
| "Tornado" Tony Rodriguez | Taco Tornado Owner | Vendor perspective, operational constraints |
| Sarah Kim | Facilities Manager | Safety concerns, logistics coordination |
| 47 Employees | End Users | Day-to-day pain points, workarounds |
| Jake Morrison | #where-is-the-taco-truck Admin | Slack channel analytics, tribal knowledge |

### 2.5 Assessment Constraints and Limitations

- **Data Availability**: No formal tracking of food truck visits; relied on Slack message analysis and employee recall
- **Vendor Cooperation**: 2 of 12 vendors declined detailed interviews (competitive concerns)
- **Seasonal Variation**: Assessment conducted in fall/winter; summer patterns may differ
- **Self-Reported Data**: Wait times and search times based on employee estimates (±30% accuracy)

---

## 3. Process Landscape Overview

### 3.1 Process Hierarchy

```
Level 0: Employee Services
├── Level 1: Food Services
│   ├── Level 2: Food Truck Operations
│   │   ├── Level 3: Truck Discovery
│   │   │   ├── Level 4: Slack Monitoring
│   │   │   ├── Level 4: Physical Search
│   │   │   └── Level 4: Word of Mouth
│   │   ├── Level 3: Queue Management
│   │   │   ├── Level 4: Line Waiting
│   │   │   └── Level 4: Order Placement
│   │   ├── Level 3: Payment Processing
│   │   │   ├── Level 4: Cash Payment
│   │   │   └── Level 4: Card Payment
│   │   └── Level 3: Vendor Coordination
│   │       ├── Level 4: Schedule Communication
│   │       └── Level 4: Location Assignment
│   ├── Level 2: Cafeteria Operations (Out of Scope)
│   └── Level 2: Vending Services (Out of Scope)
```

### 3.2 Process Inventory

| Process ID | Process Name | Category | Owner | Frequency | Criticality | Automation Level |
|------------|--------------|----------|-------|-----------|-------------|------------------|
| FT-001 | Food Truck Discovery | Core | None (ad hoc) | 2,100/month | High | 0% Manual |
| FT-002 | Queue Waiting | Core | None | 2,100/month | High | 0% Manual |
| FT-003 | Order Placement | Core | None | 1,850/month | High | 0% Manual |
| FT-004 | Payment Processing | Core | Vendors | 1,850/month | High | 40% (card readers) |
| FT-005 | Vendor Schedule Communication | Support | Facilities | Weekly | Medium | 10% (email) |
| FT-006 | Truck Location Assignment | Support | Facilities | Daily | Medium | 0% Manual |
| FT-007 | Crowd Management | Support | Security | As needed | High | 0% Manual |

### 3.3 Process Relationships and Dependencies

```
                    ┌─────────────────────────┐
                    │  FT-005: Vendor         │
                    │  Schedule Communication │
                    └───────────┬─────────────┘
                                │ informs (unreliably)
                                ▼
┌─────────────────┐    ┌─────────────────────┐    ┌─────────────────┐
│  FT-006: Truck  │───▶│  FT-001: Food Truck │───▶│  FT-002: Queue  │
│  Location       │    │  Discovery          │    │  Waiting        │
│  Assignment     │    └─────────────────────┘    └────────┬────────┘
└─────────────────┘              │                         │
                                 │ triggers (if found)     │
                                 ▼                         ▼
                    ┌─────────────────────────┐    ┌─────────────────┐
                    │  FT-007: Crowd          │    │  FT-003: Order  │
                    │  Management             │    │  Placement      │
                    └─────────────────────────┘    └────────┬────────┘
                                                           │
                                                           ▼
                                                  ┌─────────────────┐
                                                  │  FT-004: Payment│
                                                  │  Processing     │
                                                  └─────────────────┘
```

| Source Process | Target Process | Relationship Type | Dependency Strength |
|----------------|----------------|-------------------|---------------------|
| FT-005 | FT-001 | Informational | Weak (often unreliable) |
| FT-006 | FT-001 | Sequential | Medium |
| FT-001 | FT-002 | Sequential | Hard |
| FT-001 | FT-007 | Parallel | Soft |
| FT-002 | FT-003 | Sequential | Hard |
| FT-003 | FT-004 | Sequential | Hard |

### 3.4 Value Stream Mapping

| Value Stream | Processes Involved | Customer Value | Lead Time | Value-Add Time | Efficiency Ratio |
|--------------|-------------------|----------------|-----------|----------------|------------------|
| Taco Acquisition | FT-001 → FT-004 | Hot, delicious taco | 38 minutes | 4 minutes | 10.5% |
| Vendor Revenue | FT-005, FT-006 | Sales opportunity | N/A | N/A | N/A |

**Value Stream Analysis - Taco Acquisition:**

| Step | Activity | Time | Value-Add? | Notes |
|------|----------|------|------------|-------|
| 1 | Check Slack for truck sightings | 3 min | No | Often outdated info |
| 2 | Walk to first parking lot | 5 min | No | Building C: 8 min |
| 3 | Search for truck (if not found) | 10 min | No | Visit additional lots |
| 4 | Wait in queue | 15 min | No | No visibility into wait |
| 5 | Review menu (verbal) | 1 min | Yes | Often items sold out |
| 6 | Place order | 1 min | Yes | |
| 7 | Wait for preparation | 3 min | Partial | Could be doing other things |
| 8 | Pay | 1 min | Yes | Cash adds 8 min for ATM |
| **Total** | | **38 min** | **4 min** | **10.5% efficiency** |

---

## 4. Detailed Process Documentation

### 4.1 Process: Food Truck Discovery (FT-001)

#### 4.1.1 Process Overview

| Attribute | Description |
|-----------|-------------|
| **Process ID** | FT-001 |
| **Process Name** | Food Truck Discovery |
| **Process Purpose** | Enable employees to locate food trucks on campus |
| **Business Value** | Access to diverse lunch options; employee satisfaction |
| **Process Owner** | None (completely ad hoc) |
| **Process Type** | Core |
| **Trigger(s)** | Employee hunger; lunch time; Slack notification |
| **Frequency** | ~2,100 attempts per month |
| **Average Volume** | 105 per business day |
| **Criticality** | High |

#### 4.1.2 SIPOC Analysis

| Element | Description |
|---------|-------------|
| **Suppliers** | Slack users, Word of mouth, Personal observation |
| **Inputs** | Slack messages, Rumors, Previous patterns, Hope |
| **Process** | 1. Check Slack → 2. Assess reliability → 3. Choose lot → 4. Walk → 5. Verify presence |
| **Outputs** | Truck location (or disappointment), Time spent searching |
| **Customers** | Hungry employees |

**SIPOC Diagram:**
```
┌──────────────────┬──────────────────┬──────────────────┬──────────────────┬──────────────────┐
│    SUPPLIERS     │      INPUTS      │     PROCESS      │     OUTPUTS      │    CUSTOMERS     │
├──────────────────┼──────────────────┼──────────────────┼──────────────────┼──────────────────┤
│ Slack Users      │ Slack messages   │ 1. Check Slack   │ Truck found      │ Hungry employee  │
│ Coworkers        │ Verbal reports   │ 2. Assess info   │    - OR -        │                  │
│ Window-watchers  │ Visual sighting  │ 3. Pick a lot    │ Disappointment   │                  │
│ "Truck Spotters" │ Historical data  │ 4. Walk there    │ Time wasted      │                  │
│                  │ Gut feeling      │ 5. Verify/Search │ Frustration      │                  │
└──────────────────┴──────────────────┴──────────────────┴──────────────────┴──────────────────┘
```

#### 4.1.3 Process Flow

**Swimlane Diagram:**
```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                     FOOD TRUCK DISCOVERY - CURRENT STATE FLOW                                │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                              │
│ Employee    ○──►[Feel hungry]──►[Check Slack]──►◇ Info found? ──No──►[Ask coworkers]──┐    │
│                                       │              │                                 │    │
│                                       │             Yes                                │    │
│                                       │              │                                 │    │
│                                       │              ▼                                 │    │
│                                       │        ◇ Info reliable?──No──────────────────►├──┐ │
│                                       │              │                                 │  │ │
│                                       │             Yes                                │  │ │
│                                       │              │                                 │  │ │
│                                       │              ▼                                 ▼  │ │
│                                       │        [Walk to reported lot]◄────[Guess a lot]  │ │
│                                       │              │                                    │ │
│                                       │              ▼                                    │ │
│                                       │        ◇ Truck there? ──No──►[Try next lot]──────┤ │
│                                       │              │                      │            │ │
│                                       │             Yes                     │            │ │
│                                       │              │                      ▼            │ │
│                                       │              │              ◇ All lots checked?  │ │
│                                       │              │                   │      │        │ │
│                                       │              │                  Yes     No───────┘ │
│                                       │              │                   │                 │
│                                       │              ▼                   ▼                 │
│                                       │        [Join queue]──────►○  [Return to desk     │
│                                       │                              disappointed]──►○    │
│                                                                                              │
│ Slack        ┌─────────────────┐                                                            │
│ Channel      │ #where-is-the-  │  "Taco Tornado spotted Lot B!" (posted 23 min ago)        │
│              │ taco-truck      │  "Actually they left" (posted 2 min ago)                  │
│              └─────────────────┘                                                            │
│                                                                                              │
└─────────────────────────────────────────────────────────────────────────────────────────────┘

Legend: ○ = Start/End  ◇ = Decision  [box] = Activity
```

**Detailed Process Steps:**

| Step # | Activity | Role | System/Tool | Inputs | Outputs | Avg. Duration | Notes |
|--------|----------|------|-------------|--------|---------|---------------|-------|
| 1 | Check Slack channel | Employee | Slack | Hunger | Truck intel | 3 min | 847 messages/week in channel |
| 2 | Assess information reliability | Employee | Brain | Slack posts | Confidence level | 1 min | Based on poster reputation |
| 3 | Choose parking lot | Employee | Intuition | Intel, history | Target location | 30 sec | Often wrong |
| 4 | Walk to parking lot | Employee | Legs | Target location | Physical presence | 5-12 min | Varies by building |
| 5 | Visual search for truck | Employee | Eyes | Parking lot | Truck found/not | 2-10 min | May repeat for other lots |

#### 4.1.4 Decision Points and Business Rules

| Decision Point | Business Rule | Criteria | Path A | Path B | Rule Source |
|----------------|---------------|----------|--------|--------|-------------|
| Info found in Slack? | Check channel for posts < 30 min old | Post timestamp | Walk to reported lot | Ask coworkers or guess | Informal |
| Info reliable? | Trust posts from known "spotters" | Poster reputation | Act on info | Seek confirmation | Tribal knowledge |
| Truck at location? | Visual confirmation required | Physical presence | Join queue | Try next lot | Common sense |
| All lots checked? | 3 lots on campus | Lots visited count | Give up, return | Continue search | Persistence level |

#### 4.1.5 Roles and Responsibilities (RACI)

| Activity | Employee | Slack Users | Facilities | Security |
|----------|----------|-------------|------------|----------|
| Monitor Slack | R | R | I | - |
| Post sightings | - | R | - | - |
| Walk to lot | R | - | - | - |
| Verify truck presence | R | - | - | - |
| Report outdated info | R | A | - | - |

#### 4.1.6 Systems and Tools

| System/Tool | Purpose in Process | Integration Points | Data Elements | Access Level |
|-------------|-------------------|-------------------|---------------|--------------|
| Slack | Information sharing | None | Messages, timestamps | All employees |
| Personal spreadsheets | Pattern tracking (12% of employees) | None | Historical arrivals | Individual |
| Legs | Physical transportation | None | N/A | All employees |
| Windows | Visual spotting | None | Visual data | Building-dependent |

#### 4.1.7 Current Performance Metrics

| Metric | Definition | Current Value | Target | Gap | Trend |
|--------|------------|---------------|--------|-----|-------|
| Discovery Success Rate | % of attempts finding truck | 66% | 95% | -29% | → Stable |
| Average Search Time | Time from decision to queue | 18 min | 5 min | -13 min | ↑ Worsening |
| Slack Info Accuracy | % of posts still accurate | 34% | 90% | -56% | ↓ Declining |
| Building C Success Rate | Success rate for furthest building | 33% | 95% | -62% | → Stable |

#### 4.1.8 Exceptions and Variations

| Variation | Trigger | Frequency | Handling | Impact |
|-----------|---------|-----------|----------|--------|
| "Truck Convoy" | Multiple trucks same day | 2x/month | Mass confusion, long Slack threads | Higher search time |
| Surprise Arrival | Unscheduled truck visit | 3x/month | Word of mouth only | 80% miss it |
| Early Departure | Truck sells out | 8x/month | Disappointed employees | Wasted trips |
| Weather Cancellation | Rain/extreme weather | 4x/month | No notification | Wasted trips |

**Workarounds Currently in Use:**

| Workaround | Problem Addressed | Who Uses | Frequency | Risk |
|------------|------------------|----------|-----------|------|
| Personal spreadsheet tracking | No historical data | 12% of employees | Daily | Data silos, outdated |
| "Scout" system | Unreliable Slack info | Power users | Daily | Burden on scouts |
| Window watching | No real-time visibility | Building A only | Constant | Limited to one building |
| Calendar invites | No schedule visibility | 3 managers | Weekly | Often wrong |

#### 4.1.9 Pain Points and Issues

| Issue ID | Description | Root Cause | Impact | Affected Parties | Priority |
|----------|-------------|------------|--------|------------------|----------|
| FT001-PP1 | Slack info outdated within minutes | No real-time tracking | 340 failed trips/month | All employees | Critical |
| FT001-PP2 | Building C employees disadvantaged | Distance from lots | 67% lower success rate | 800 employees | High |
| FT001-PP3 | No way to know truck schedule | Informal vendor communication | Cannot plan lunch | All employees | High |
| FT001-PP4 | "Truck Spotters" burned out | Voluntary role, no recognition | Key people disengaging | Community | Medium |

---

### 4.2 Process: Queue Waiting (FT-002)

#### 4.2.1 Process Overview

| Attribute | Description |
|-----------|-------------|
| **Process ID** | FT-002 |
| **Process Name** | Queue Waiting |
| **Process Purpose** | Wait in line to place food order |
| **Business Value** | Access to food truck offerings |
| **Process Owner** | None |
| **Process Type** | Core |
| **Trigger(s)** | Arrival at food truck location |
| **Frequency** | ~1,850 successful queues per month |
| **Average Volume** | 93 per business day |
| **Criticality** | High |

#### 4.2.2 SIPOC Analysis

```
┌──────────────────┬──────────────────┬──────────────────┬──────────────────┬──────────────────┐
│    SUPPLIERS     │      INPUTS      │     PROCESS      │     OUTPUTS      │    CUSTOMERS     │
├──────────────────┼──────────────────┼──────────────────┼──────────────────┼──────────────────┤
│ Other employees  │ Physical queue   │ 1. Find end of   │ Position at      │ Employee ready   │
│ Food truck       │ Time available   │    line          │ order window     │ to order         │
│                  │ Patience         │ 2. Wait          │ Time spent       │                  │
│                  │ Phone battery    │ 3. Inch forward  │ Frustration or   │                  │
│                  │                  │ 4. Reach front   │ anticipation     │                  │
└──────────────────┴──────────────────┴──────────────────┴──────────────────┴──────────────────┘
```

#### 4.2.3 Process Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        QUEUE WAITING - CURRENT STATE                         │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ○──►[Locate end of line]──►[Join queue]──►[Wait...]──►[Shuffle forward]──┐ │
│                                                              │              │ │
│                                                              ▼              │ │
│                                                    ◇ At front of line?      │ │
│                                                         │        │          │ │
│                                                        Yes       No─────────┘ │
│                                                         │                     │
│                                                         ▼                     │
│                                                  [Ready to order]──►○        │
│                                                                              │
│  ACTIVITIES WHILE WAITING:                                                   │
│  ├── Check phone (78%)                                                       │
│  ├── Chat with coworkers (45%)                                              │
│  ├── Try to read menu from distance (89%)                                   │
│  ├── Regret life choices (34%)                                              │
│  └── Calculate if there's time before 1pm meeting (67%)                     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### 4.2.4 Current Performance Metrics

| Metric | Definition | Current Value | Target | Gap | Trend |
|--------|------------|---------------|--------|-----|-------|
| Average Wait Time | Time from joining queue to ordering | 17 min | 5 min | -12 min | ↑ Worsening |
| Peak Wait Time | Maximum observed wait | 42 min | 10 min | -32 min | ↑ Worsening |
| Abandonment Rate | % who leave queue before ordering | 8% | 2% | -6% | ↑ Worsening |
| Wait Time Visibility | Ability to estimate wait | 0% | 100% | -100% | → None |

#### 4.2.5 Pain Points and Issues

| Issue ID | Description | Root Cause | Impact | Priority |
|----------|-------------|------------|--------|----------|
| FT002-PP1 | No wait time visibility | No queue management | Cannot make informed decisions | Critical |
| FT002-PP2 | Peak time congestion | Everyone arrives at noon | 25+ minute waits | High |
| FT002-PP3 | Wasted time standing | No pre-order capability | 17 min unproductive/visit | High |
| FT002-PP4 | Weather exposure | Outdoor queuing | Discomfort, health concerns | Medium |

---

### 4.3 Process: Payment Processing (FT-004)

#### 4.3.1 Process Overview

| Attribute | Description |
|-----------|-------------|
| **Process ID** | FT-004 |
| **Process Name** | Payment Processing |
| **Process Purpose** | Complete financial transaction for food order |
| **Business Value** | Revenue for vendor, food for employee |
| **Process Owner** | Individual vendors |
| **Process Type** | Core |
| **Trigger(s)** | Order completion |
| **Frequency** | ~1,850 transactions per month |
| **Criticality** | High |

#### 4.3.2 Current Payment Methods by Vendor

| Vendor | Cash | Credit Card | Mobile Pay | Notes |
|--------|------|-------------|------------|-------|
| Taco Tornado | Yes | Yes | No | Square reader, often slow WiFi |
| Burger Barn | Yes | No | No | **Cash only** - major friction |
| Sushi Express | Yes | Yes | Yes | Most modern setup |
| Pizza Pete | Yes | Yes | No | Card minimum $10 |
| Waffle Wagon | Yes | No | No | **Cash only** |
| Seoul Food | Yes | Yes | No | |
| Curry Up Now | Yes | Yes | Apple Pay | |
| Greek Gods | Yes | No | No | **Cash only** |
| BBQ Boss | Yes | Yes | No | |
| Veggie Victory | Yes | Yes | Google Pay | |
| Coffee Cruiser | Yes | Yes | Yes | |
| Dessert Dreams | Yes | Yes | No | |

**Payment Analysis:**
- 3 of 12 trucks (25%) are cash-only
- Cash-only trucks account for 31% of total visits
- Average ATM trip adds 8 minutes to process
- 420 unnecessary ATM trips per month

#### 4.3.3 Pain Points and Issues

| Issue ID | Description | Root Cause | Impact | Priority |
|----------|-------------|------------|--------|----------|
| FT004-PP1 | Cash-only vendors | Vendor preference/cost | 420 ATM trips/month, 8 min each | High |
| FT004-PP2 | Slow card processing | Poor WiFi in parking lots | 2-3 min added per transaction | Medium |
| FT004-PP3 | No expense tracking | Manual receipts only | Cannot track food spend | Low |
| FT004-PP4 | Card minimums | Vendor policy | Forces larger purchases | Low |

---

## 5. Technology and Systems Landscape

### 5.1 Systems Inventory

| System ID | System Name | Type | Vendor | Purpose | Criticality | Status |
|-----------|-------------|------|--------|---------|-------------|--------|
| SYS-001 | Slack | Communication | Salesforce | Truck sighting sharing | High | Active |
| SYS-002 | Personal Spreadsheets | Tracking | Various | Pattern tracking | Low | Fragmented |
| SYS-003 | Square Readers | Payment | Square | Card processing | Medium | 8 trucks |
| SYS-004 | Email | Communication | Microsoft | Vendor scheduling | Low | Unreliable |
| SYS-005 | Globex WiFi | Infrastructure | IT | Connectivity | Medium | Poor in lots |

### 5.2 System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     CURRENT SYSTEM LANDSCAPE (Minimal)                       │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   EMPLOYEES                           VENDORS                                │
│   ┌─────────────┐                    ┌─────────────┐                        │
│   │   Slack     │                    │   Square    │                        │
│   │   App       │                    │   Reader    │                        │
│   └──────┬──────┘                    └──────┬──────┘                        │
│          │                                  │                                │
│          │ (manual posts)                   │ (payment only)                 │
│          ▼                                  ▼                                │
│   ┌─────────────┐                    ┌─────────────┐                        │
│   │   Slack     │                    │   Square    │                        │
│   │   Cloud     │                    │   Cloud     │                        │
│   └─────────────┘                    └─────────────┘                        │
│                                                                              │
│   NO INTEGRATION BETWEEN SYSTEMS                                            │
│   NO CENTRALIZED DATA                                                       │
│   NO AUTOMATION                                                             │
│                                                                              │
│   ┌─────────────────────────────────────────────────────────┐              │
│   │  FACILITIES                                              │              │
│   │  ┌─────────┐    ┌─────────┐    ┌─────────┐             │              │
│   │  │ Outlook │    │  Phone  │    │ Notepad │             │              │
│   │  │ (email) │    │ (calls) │    │ (notes) │             │              │
│   │  └─────────┘    └─────────┘    └─────────┘             │              │
│   │       └──────────────┼──────────────┘                   │              │
│   │                      │                                   │              │
│   │              [Manual Coordination]                       │              │
│   └─────────────────────────────────────────────────────────┘              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 5.3 Technology Pain Points

| Issue | Affected Systems | Impact | Root Cause | Workaround |
|-------|------------------|--------|------------|------------|
| No real-time tracking | All | Core problem | No tracking system exists | Slack posts |
| Poor parking lot WiFi | SYS-005 | Slow payments | Infrastructure gaps | Cash fallback |
| No centralized data | All | No analytics possible | Siloed systems | Manual aggregation |
| No mobile capability | N/A | Cannot order ahead | No app exists | Physical queuing |

### 5.4 Technical Debt

| Item | Description | Systems Affected | Risk | Remediation Effort |
|------|-------------|------------------|------|-------------------|
| Slack channel scaling | 847 msgs/week unsustainable | SYS-001 | Medium | Build dedicated solution |
| Spreadsheet proliferation | 300+ personal tracking sheets | SYS-002 | Low | Centralize data |
| No backup coordination | Single point of failure (Sarah) | SYS-004 | High | Document process, add backup |

---

## 6. Organizational Structure

### 6.1 Organization Chart

```
                           ┌─────────────────────┐
                           │ Patricia Vaughn     │
                           │ VP Employee Exp     │
                           └──────────┬──────────┘
                                      │
                    ┌─────────────────┼─────────────────┐
                    │                 │                 │
          ┌─────────▼─────────┐ ┌────▼────────┐ ┌─────▼─────────┐
          │ Marcus Chen       │ │ Sarah Kim   │ │ Security      │
          │ EE Manager        │ │ Facilities  │ │ Team          │
          └─────────┬─────────┘ └─────────────┘ └───────────────┘
                    │
          ┌─────────▼─────────┐
          │ Jake Morrison     │
          │ Slack Admin       │
          │ (Volunteer)       │
          └───────────────────┘
                    │
          ┌─────────▼─────────┐
          │ "Truck Spotters"  │
          │ ~15 volunteers    │
          └───────────────────┘
```

### 6.2 Roles and FTE Allocation

| Role | Department | FTE Count | Processes Supported | Notes |
|------|------------|-----------|---------------------|-------|
| Facilities Coordinator | Facilities | 0.1 FTE | FT-005, FT-006 | Part of larger role |
| Slack Admin | Volunteer | 0.0 FTE | FT-001 | Unpaid, unsanctioned |
| Truck Spotters | Volunteer | 0.0 FTE | FT-001 | ~15 informal volunteers |
| Security Guard | Security | 0.05 FTE | FT-007 | Reactive only |

**Total dedicated FTE: 0.15**

### 6.3 Skills Matrix

| Role | Slack | Vendor Relations | Data Analysis | Crowd Management | GPS/Tech |
|------|-------|------------------|---------------|------------------|----------|
| Sarah Kim | ◐ | ● | ○ | ◐ | ○ |
| Jake Morrison | ● | ○ | ◐ | ○ | ○ |
| Truck Spotters | ● | ○ | ○ | ○ | ○ |

**Legend:** ● Expert  ◐ Proficient  ○ Basic/Training Needed

### 6.4 Knowledge Management

| Process/Area | Documentation Status | Training Available | Knowledge Risk |
|--------------|---------------------|-------------------|----------------|
| Vendor coordination | None | None | High (Sarah only) |
| Slack channel management | Informal | None | Medium (Jake only) |
| Truck patterns | Tribal knowledge | None | High (scattered) |
| Payment processing | Vendor-specific | None | Low |

---

## 7. Performance Analysis

### 7.1 Process Performance Dashboard

| Process | Cycle Time | Volume | Quality | Cost Impact | Overall Health |
|---------|------------|--------|---------|-------------|----------------|
| FT-001 Discovery | 18 min ❌ | 2,100/mo ✅ | 66% ❌ | $87K/yr ❌ | ❌ Critical |
| FT-002 Queue | 17 min ❌ | 1,850/mo ✅ | 92% ⚠️ | $252K/yr ❌ | ❌ Critical |
| FT-003 Order | 2 min ✅ | 1,850/mo ✅ | 85% ⚠️ | N/A | ⚠️ Moderate |
| FT-004 Payment | 2 min ⚠️ | 1,850/mo ✅ | 95% ✅ | $34K/yr ⚠️ | ⚠️ Moderate |
| FT-005 Scheduling | N/A | Weekly | 40% ❌ | Indirect | ❌ Critical |

### 7.2 Trend Analysis (6-Month View)

| Metric | Jul 2025 | Oct 2025 | Dec 2025 | Trend | Root Cause |
|--------|----------|----------|----------|-------|------------|
| Avg Search Time | 15 min | 17 min | 18 min | ↑ Worsening | More trucks, more confusion |
| Avg Wait Time | 12 min | 15 min | 17 min | ↑ Worsening | Popularity growth |
| Slack Messages/Day | 120 | 145 | 170 | ↑ Growing | Channel becoming unusable |
| Discovery Success | 72% | 68% | 66% | ↓ Declining | Info quality degrading |
| Employee Satisfaction | 3.1 | 2.9 | 2.8 | ↓ Declining | Cumulative frustration |

### 7.3 Cost Analysis

| Cost Category | Annual Cost | Per Transaction | % of Total | Notes |
|---------------|-------------|-----------------|------------|-------|
| Lost Productivity - Search | $87,000 | $3.91 | 22.8% | 340 failed trips × 15 min × $50/hr × 12 mo |
| Lost Productivity - Wait | $252,000 | $11.35 | 66.0% | 2,100 visits × 12 min excess × $50/hr × 12 mo |
| Lost Productivity - ATM | $33,600 | $6.67 | 8.8% | 420 trips × 8 min × $50/hr × 12 mo |
| Lost Productivity - Missed | $9,500 | N/A | 2.5% | Disappointment impact estimate |
| **Total Annual Impact** | **$382,100** | **$17.21** | **100%** | |

### 7.4 Capacity Analysis

| Resource | Current Load | Capacity | Utilization | Constraint |
|----------|--------------|----------|-------------|------------|
| Slack Channel | 847 msgs/week | ~500 useful | 170% | Information overload |
| Parking Lot A | 3 trucks max | 3 trucks | 100% | Space limited |
| Parking Lot B | 2 trucks max | 2 trucks | 100% | Space limited |
| Parking Lot C | 2 trucks max | 2 trucks | 50% | Low traffic |
| Peak Lunch (12-1pm) | 800 employees | 400 served | 200% | Major bottleneck |

---

## 8. Pain Points and Issues Summary

### 8.1 Pain Point Inventory

| ID | Category | Description | Process | Business Impact | Frequency | Priority |
|----|----------|-------------|---------|-----------------|-----------|----------|
| PP-001 | Process | No real-time truck visibility | FT-001 | $87K/year lost productivity | Constant | Critical |
| PP-002 | Process | No wait time estimates | FT-002 | Cannot make informed decisions | Constant | Critical |
| PP-003 | Process | No pre-ordering capability | FT-002/003 | $252K/year in queue time | Constant | Critical |
| PP-004 | Technology | Cash-only vendors | FT-004 | $34K/year in ATM trips | 31% of visits | High |
| PP-005 | People | Building C inequity | FT-001 | 800 employees disadvantaged | Constant | High |
| PP-006 | Process | Unreliable schedule info | FT-005 | Cannot plan lunch | Weekly | High |
| PP-007 | Safety | Crowd surge incidents | FT-007 | 3 injuries in 6 months | Monthly | High |
| PP-008 | Data | No menu visibility | FT-003 | Stockout disappointment | Daily | Medium |
| PP-009 | People | Volunteer burnout | FT-001 | Key spotters disengaging | Ongoing | Medium |
| PP-010 | Integration | No dietary filtering | FT-003 | Allergy/preference friction | Daily | Medium |

### 8.2 Impact Quantification

| Pain Point | Time Lost/Year | Cost Impact | Quality Impact | Customer Impact |
|------------|----------------|-------------|----------------|-----------------|
| PP-001 | 4,080 hours | $204,000 | 34% failed trips | High frustration |
| PP-002 | 5,040 hours | $252,000 | Cannot plan | Moderate frustration |
| PP-003 | (included above) | (included above) | Wasted standing | High frustration |
| PP-004 | 672 hours | $33,600 | Payment friction | Moderate frustration |
| PP-005 | 1,600 hours | $80,000 | 67% miss rate | High frustration (Bldg C) |
| PP-006 | N/A | Indirect | Poor planning | Moderate frustration |
| PP-007 | N/A | Liability risk | Safety incidents | High concern |

### 8.3 Stakeholder Feedback Summary

| Stakeholder Group | Top Pain Points | Verbatim Quotes | Satisfaction |
|-------------------|-----------------|-----------------|--------------|
| Power Users | Wait times, stockouts | "I've memorized Tony's schedule but he still surprises me" | 3.2/5 |
| Regular Users | Finding trucks, cash | "I gave up and just bring lunch now" | 2.6/5 |
| Building C | Distance, missing trucks | "By the time I get there, they're gone or sold out" | 2.1/5 |
| Vendors | Unpredictable demand | "I either run out or throw away food" | 3.0/5 |
| Facilities | Coordination burden | "I spend 2 hours a week on truck logistics" | 2.5/5 |

**Selected Employee Quotes:**
- "The Slack channel is useless - by the time someone posts, the line is 30 people deep"
- "I created a spreadsheet to track Taco Tornado but Tony keeps changing his schedule"
- "The Taco Tuesday Stampede was inevitable - we're lucky it wasn't worse"
- "I work in Building C. I've basically given up on food trucks."
- "Why is it 2025 and I can't just order a taco from my phone?"

---

## 9. Compliance and Risk Assessment

### 9.1 Safety Incidents

| Date | Incident | Location | Severity | Root Cause | Resolution |
|------|----------|----------|----------|------------|------------|
| Sep 15, 2025 | "Taco Tuesday Stampede" | Lot A | High | Crowd surge when truck arrived | 3 sprained ankles, HR investigation |
| Nov 3, 2025 | Slip and fall | Lot B | Medium | Running to truck in rain | 1 minor injury |
| Dec 12, 2025 | Near-miss vehicle | Lot A | High | Pedestrians cutting through traffic | No injury, policy review |

### 9.2 Risk Register

| Risk ID | Risk Description | Category | Likelihood | Impact | Score | Current Controls | Effectiveness |
|---------|------------------|----------|------------|--------|-------|-----------------|---------------|
| RSK-001 | Crowd surge injury | Safety | High | High | 16 | Security awareness | Ineffective |
| RSK-002 | Food safety incident | Compliance | Low | High | 8 | County health inspection | Effective |
| RSK-003 | Vendor no-show | Operational | Medium | Low | 6 | None | N/A |
| RSK-004 | Payment fraud | Financial | Low | Medium | 4 | Vendor responsibility | Unknown |
| RSK-005 | Key person dependency | Operational | High | Medium | 12 | None | N/A |

### 9.3 Risk Heat Map

```
              │   Low (1)   │  Medium (2) │   High (3)  │ Critical (4)│
──────────────┼─────────────┼─────────────┼─────────────┼─────────────┤
High (4)      │             │  RSK-005    │  RSK-001    │             │
──────────────┼─────────────┼─────────────┼─────────────┼─────────────┤
Medium (3)    │  RSK-003    │             │             │             │
──────────────┼─────────────┼─────────────┼─────────────┼─────────────┤
Low (2)       │             │  RSK-004    │  RSK-002    │             │
──────────────┼─────────────┼─────────────┼─────────────┼─────────────┤
Rare (1)      │             │             │             │             │
──────────────┴─────────────┴─────────────┴─────────────┴─────────────┘
```

---

## 10. Improvement Opportunities

### 10.1 Quick Wins (< 30 days, minimal investment)

| Opportunity | Effort | Impact | Payback | Owner |
|-------------|--------|--------|---------|-------|
| Formalize Slack channel rules | Low | Medium | Immediate | Jake |
| Create vendor contact list | Low | Low | 1 week | Sarah |
| Post truck schedule on intranet | Low | Medium | 1 week | Sarah |
| Designate overflow lot | Low | Medium | Immediate | Facilities |

### 10.2 Strategic Improvements

| Opportunity | Current State | Desired Outcome | Gap | Benefit | Complexity |
|-------------|---------------|-----------------|-----|---------|------------|
| Real-time tracking | Slack posts | GPS-based live tracking | Complete rebuild | $87K/year | High |
| Mobile pre-ordering | Walk-up only | Order from desk | New capability | $252K/year | High |
| Queue management | Physical lines | Virtual queue | New capability | Improved UX | Medium |
| Unified payments | Vendor-specific | Single app payment | Integration | $34K/year | Medium |

### 10.3 Automation Candidates

| Process/Activity | Current State | Automation Potential | Technology | ROI Estimate |
|------------------|---------------|---------------------|------------|--------------|
| Truck location tracking | Manual Slack posts | Full | GPS + Mobile app | High |
| Wait time estimation | None | Full | Queue sensors/ML | High |
| Arrival notifications | Manual posts | Full | Push notifications | High |
| Menu updates | Word of mouth | Full | Vendor portal | Medium |
| Payment processing | Per-vendor | Partial | Unified payment | Medium |

### 10.4 Recommended Solution: TacoTracker 3000

Based on this current state assessment, we recommend development of an integrated food truck management platform with the following capabilities:

| Capability | Pain Points Addressed | Expected Impact |
|------------|----------------------|-----------------|
| Real-time GPS tracking | PP-001, PP-005 | 95% discovery success rate |
| Push notifications | PP-001, PP-006 | Eliminate wasted trips |
| Mobile pre-ordering | PP-002, PP-003 | Reduce wait time to <5 min |
| Unified payments | PP-004 | Eliminate ATM trips |
| Queue management | PP-002, PP-007 | Predictable waits, safety |
| Menu with dietary filters | PP-008, PP-010 | Informed decisions |

**Projected ROI:** $382,100 annual savings vs. $127,000 development cost = 300% Year 1 ROI

---

## 11. Appendices

### Appendix A: Glossary

| Term | Definition |
|------|------------|
| Taco Tornado | Most popular food truck; owned by "Tornado" Tony Rodriguez |
| TRDS | Taco-Related Disappointment Syndrome (HR classification) |
| Truck Spotter | Volunteer employee who posts truck sightings to Slack |
| The Stampede | September 15, 2025 incident with 3 injuries |
| Building C | Furthest building from main parking lot; 800 employees |
| Power User | Employee visiting food trucks 3+ times per week |

### Appendix B: Interview Log

| Date | Interviewee | Role | Key Insights |
|------|-------------|------|--------------|
| Oct 15 | Patricia Vaughn | VP EE | Executive support confirmed; budget available |
| Oct 16 | Marcus Chen | EE Manager | Detailed pain point catalog |
| Oct 17-20 | 47 Employees | Various | Quantified frustrations; workaround inventory |
| Oct 21 | Tony Rodriguez | Taco Tornado | Vendor perspective; demand unpredictability |
| Oct 22 | Sarah Kim | Facilities | Coordination burden; safety concerns |
| Oct 23 | Jake Morrison | Slack Admin | Channel analytics; burnout warning |
| Oct 24-Nov 5 | 11 Vendors | Truck Operators | Payment preferences; scheduling challenges |

### Appendix C: Slack Channel Analysis

**#where-is-the-taco-truck Statistics (December 2025):**
- Members: 1,847 (74% of company)
- Messages per week: 847
- Peak activity: Tuesday 11:30 AM - 12:30 PM
- Average message lifespan (accuracy): 8 minutes
- Top posters: 15 "Truck Spotters" account for 67% of sightings
- Most common message: "Is Taco Tornado here today?" (142 times/month)

### Appendix D: Vendor Schedule (As Communicated)

| Day | Scheduled Vendor(s) | Actual Reliability |
|-----|--------------------|--------------------|
| Monday | Burger Barn, Sushi Express | 70% |
| Tuesday | Taco Tornado (confirmed), Pizza Pete | 85% |
| Wednesday | Seoul Food, Curry Up Now | 60% |
| Thursday | BBQ Boss, Veggie Victory | 75% |
| Friday | Rotating (varies) | 40% |

*Note: Schedule often changes without notice*

### Appendix E: Building Distance Analysis

| Origin | To Lot A | To Lot B | To Lot C | Avg. Walk Time |
|--------|----------|----------|----------|----------------|
| Building A | 2 min | 5 min | 8 min | 5 min |
| Building B | 4 min | 3 min | 6 min | 4.3 min |
| Building C | 8 min | 6 min | 3 min | 5.7 min |

*Building C employees face longest average walk AND most trucks park in Lots A/B*

---

**Document Approval:**

| Role | Name | Date | Signature |
|------|------|------|-----------|
| Process Owner | Marcus Chen | Jan 20, 2026 | ✓ Approved |
| Executive Sponsor | Patricia Vaughn | Jan 21, 2026 | ✓ Approved |
| Facilities | Sarah Kim | Jan 20, 2026 | ✓ Approved |

---

*This Current State Assessment serves as the foundation for the TacoTracker 3000 Business Requirements Document (BRD) and will be used as the baseline for measuring post-implementation improvements.*
