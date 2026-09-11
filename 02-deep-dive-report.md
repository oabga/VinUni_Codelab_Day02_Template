Incoming trip
      │
      ▼
Automatically retrieve:

- Battery
- GPS
- Pickup distance
- Trip distance
      │
      ▼
 ┌────────────────┐
 │ Battery < 5% ? │
 └───────┬────────┘
      YES│          NO
         │
         ▼
 DO_NOT_ASSIGN      Battery-risk
         │          calculation
         │              │
         ▼              ▼
dispatch_mobile_    Risk category
charger                  │
         │               │
         └───────┬───────┘
                 ▼
              🔵 LLM
        Explain recommendation
                 │
                 ▼
           [DRAFT_ONLY]
                 │
                 ▼
         🟢 Human Dispatcher
              /      \
          approve    reject
             │         │
             ▼         ▼
           assign    manual
                     review
