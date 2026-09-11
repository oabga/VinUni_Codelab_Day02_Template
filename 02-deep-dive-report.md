# Deep-Dive Report

## 1. Actor / Operator

The primary operator is a **Xanh SM dispatcher** responsible for assigning available electric vehicles to incoming trips.

## 2. Current Workflow

When a trip request arrives, the dispatcher identifies available vehicles, checks their battery status, evaluates the distance to the pickup point and the expected trip distance, and manually determines whether a vehicle can safely accept the trip.

## 3. Bottleneck

The main bottleneck is combining multiple operational variables to assess battery risk before vehicle assignment.

The dispatcher must interpret battery percentage, pickup distance, trip distance, and charging availability before making a decision.

## 4. Business Impact

Slow or inconsistent battery-risk assessment may increase dispatcher workload and could result in assigning vehicles with insufficient battery reserves.

Potential consequences include driver downtime, trip reassignment, customer waiting time, and emergency charging incidents.

## 5. Success Metrics

Prototype targets:

1. Reduce battery-risk decision-support time from an estimated 2–4 minutes to below 30 seconds.
2. Achieve 100% compliance with the critical-battery boundary in adversarial test cases.
3. Prevent autonomous assignment: 100% of recommendations require dispatcher approval.

## 6. Operational Boundary

The AI **may**:

- analyse provided vehicle and trip information;
- classify battery risk;
- draft an explanation and recommended action.

The AI **must NOT**:

- directly assign a vehicle to a trip;
- invent missing battery, GPS, distance, or station data;
- override deterministic critical-battery safety rules;
- send operational instructions without human approval.

## 7. Safety Rules & Constraints

- If the battery is below **5%**, the vehicle must not be assigned to a new trip.
- If safe charging cannot be reached within the defined safety boundary, the system must recommend dispatching a mobile charger.
- The magnitude of these effects requires validation using real operational logs.

## 8. Decision Flow Diagram

```text
                  Incoming trip
                       │
                       ▼
        Automatically retrieve:
        ┌─────────────────────────────┐
        │ • Battery                   │
        │ • GPS                       │
        │ • Pickup distance           │
        │ • Trip distance             │
        └──────────────┬──────────────┘
                       │
                       ▼
              ┌─────────────────┐
              │ Battery < 5% ?  │
              └───────┬─────────┘
              YES     │     NO
        ┌─────────────┴─────────────┐
        │                           │
        ▼                           ▼
  DO_NOT_ASSIGN            Battery-risk
        │                  calculation
        │                           │
        ▼                           ▼
  dispatch_mobile_            Risk category
  charger                           │
        │                           │
        └────────────┬──────────────┘
                     │
                     ▼
                 🔵 LLM
        Explain recommendation
                     │
                     ▼
              [DRAFT_ONLY]
                     │
                     ▼
          🟢 Human Dispatcher
               /        \
          approve      reject
              │           │
              ▼           ▼
           assign     manual review
```

## 9. Final Decision — GO for Limited Prototype

BatteryGuard AI should proceed as a limited internal prototype.

The problem has clear inputs and measurable success criteria.
Safety-critical decisions can be separated from language-model
generation using deterministic rules.

The LLM acts only as a recommendation and explanation layer,
while the human dispatcher retains final operational authority.

Production deployment should only be considered after evaluation
using historical and pilot operational data.
