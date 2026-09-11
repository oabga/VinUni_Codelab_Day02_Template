# Phase 1 — Problem Scan

| # | Subsidiary | Lens | Problem |
| --- | --- | --- | --- |
| 1 | Xanh SM | Time-consuming | Dispatcher manually assesses whether an EV has sufficient battery for the next trip |
| 2 | Xanh SM | Stakeholder Pain | Manual analysis of trip cancellation reasons |
| 3 | VinFast | Repetitive | Manual routing of customer vehicle fault descriptions |
| 4 | Vinhomes | AI-upgrade | Manual classification of resident complaints |
| 5 | Vinpearl | Stakeholder Pain | Manual detection of urgent negative guest reviews |
QUICK PROBLEM CARD #1

Problem:
Xanh SM dispatchers need to assess whether an EV has enough
battery to safely complete an upcoming trip.

Actor:
Xanh SM Dispatcher

Current Workflow:

1. Receive trip request
→ 2. Find available vehicles
→ 3. Check battery
→ 4. Check pickup/trip distance
→ 5. Estimate battery risk
→ 6. Assign vehicle

Bottleneck:
Manual battery-risk assessment.

Estimated baseline:
~2–4 min / decision
(prototype assumption; requires operational validation)

AI Step:
Summarize the risk and generate a recommendation.

Success Metric:
Reduce decision-support time to <30 seconds.

Architecture:
Rule + LLM Feature + Human-in-the-loop

Quick Card 2 — VinFast Fault Routing
Problem:
Customer descriptions of vehicle problems must be manually
interpreted and routed to the appropriate technical team.

Actor:
Customer service / service advisor

Workflow:

Customer complaint
→ read description
→ interpret issue
→ choose category
→ route to technician

Bottleneck:
Natural-language interpretation.

AI:
Classify complaint and explain classification.

Metric:
>=90% correct routing on labelled test dataset.

Architecture:
LLM Feature + Human Review
Quick Card 3 — Vinhomes Complaint Routing
Problem:
Resident complaints need to be manually classified and routed
to the appropriate building-management team.

Actor:
Customer service staff

Workflow:

Complaint received
→ staff reads complaint
→ identify category
→ identify department
→ forward request

AI:
Complaint classification + suggested routing.

Metric:
>=90% routing accuracy.

Architecture:
LLM Feature

## Selected Problem

The selected problem is **BatteryGuard AI — Pre-Dispatch Battery Risk
Assessment for Xanh SM**.

This problem was selected because:

- it has a clear operational actor;
- the inputs can be represented as structured data;
- safety boundaries can be clearly defined;
- success can be measured quantitatively;
- deterministic rules and LLM capabilities can be meaningfully separated.
