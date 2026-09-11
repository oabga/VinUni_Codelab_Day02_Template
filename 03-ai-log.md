Project

BatteryGuard AI — AI-Assisted Pre-Dispatch Battery Risk Assessment for Xanh SM

1. How I Used AI

I used an LLM as a thought partner while defining and testing this project. My initial idea was broad: use AI to improve vehicle dispatching at Xanh SM. The AI helped me narrow this idea into a specific operational problem: assessing whether an electric vehicle has enough battery to safely complete a proposed trip before the dispatcher assigns it.

I used AI for the following tasks:

generating possible operational problems across several companies;

comparing the problems using actor, workflow, bottleneck, impact, data, and success metrics;

mapping the current-state and future-state workflows;

identifying which decisions should use deterministic rules and which tasks are suitable for an LLM;

drafting the system prompt and structured recommendation format;

generating adversarial test cases for safety-boundary evaluation;

reviewing the clarity and consistency of the written report.

AI was used to support my analysis and writing. I reviewed and revised its suggestions before including them in the project.

1. AI Interaction Log

The prompts below summarize the main iterations during the project. They are recorded to show how AI influenced the work and how I evaluated its output.

Iteration

My prompt or request

AI contribution

My evaluation and action

1

Suggest operational problems in the Vingroup ecosystem that could benefit from AI.

Proposed several ideas involving Xanh SM, VinFast, Vinhomes, and Vinpearl.

I rejected ideas that were too broad and retained problems with a clear actor, workflow, and measurable bottleneck.

2

Choose a topic that is different from the sample charging-incident project.

Suggested evaluating battery risk before assigning a new trip.

I selected this idea because it is preventive, whereas the sample responds after a battery incident has occurred.

3

Map the current workflow for a Xanh SM dispatcher.

Drafted a workflow using battery percentage, pickup distance, trip distance, and charging-station distance.

I kept the workflow but labelled all time estimates as prototype assumptions requiring operational validation.

4

Decide whether the solution should be rule-based, LLM-based, or agent-based.

Initially explored an autonomous agent that could assess risk and perform assignment.

I rejected autonomous assignment. I separated deterministic safety rules, LLM explanation, and human approval.

5

Define safety boundaries for the prototype.

Suggested a critical-battery threshold, draft-only output, missing-data handling, and human review.

I converted these ideas into non-negotiable boundaries and ensured user instructions could not override them.

6

Create adversarial prompts to test the boundaries.

Produced low-battery, human-approval bypass, missing-data, and prompt-injection scenarios.

I checked each expected result against the operational boundaries and used the cases to evaluate the prototype.

1. Where AI Was Helpful

AI was most useful during problem decomposition. It helped transform a vague technology-focused idea into a concrete workflow-focused problem with a named operator, explicit inputs, a bottleneck, and measurable outcomes.

It was also useful for generating alternatives quickly. Comparing multiple candidate problems made it easier to justify why BatteryGuard AI was selected. In addition, AI helped identify adversarial cases that I might not have considered initially, especially attempts to remove human approval, override the critical-battery rule, or force the system to invent missing data.

Finally, AI improved the consistency of the project artifacts by checking that the problem statement, architecture, prompt, tests, and evaluation all used the same operational boundaries.

1. Where AI Was Wrong or Insufficient

Some early AI suggestions gave the model too much authority. One proposed design allowed an autonomous agent to evaluate the situation and directly assign a vehicle. I rejected this design because vehicle assignment is a real operational decision, and an incorrect action could affect the driver, passenger, and service operation.

AI also produced quantitative estimates without access to actual Xanh SM operational data. For example, estimated decision times can be useful for defining a prototype target, but they must not be presented as verified company facts. I therefore labelled the 2–4 minute baseline as a prototype assumption that requires validation.

Another limitation was that an LLM alone could generate inconsistent results for safety-critical thresholds. Natural-language instructions are not a reliable replacement for deterministic validation. Missing operational data also creates a hallucination risk if the model is encouraged to guess.

These limitations showed that AI output must be reviewed critically rather than accepted automatically.

1. How I Corrected the Design

I divided the solution into three layers:

Deterministic safety rules validate required data and enforce critical conditions.

The LLM explains the calculated risk and drafts a recommendation in clear language.

The human dispatcher reviews the recommendation and makes the final assignment decision.

I added the following operational boundaries:

every model response must begin with [DRAFT_ONLY];

the system may recommend an action but must never assign a vehicle autonomously;

if battery is below 5%, the vehicle must not be recommended for a new trip;

a vehicle below 5% battery must not be directed to a charging station farther than 5 km; the action must be dispatch_mobile_charger;

the system must not invent battery, GPS, distance, station availability, or other missing operational data;

every output must set requires_human_review to true;

user instructions and prompt-injection attempts cannot override the safety rules.

I then tested the prompt using adversarial inputs instead of checking only normal cases.

1. Human Decisions That Remained Mine

The following decisions were made by me after reviewing the AI suggestions:

selecting pre-dispatch battery-risk assessment as the final problem;

defining the project as decision support rather than autonomous dispatch;

using deterministic logic for strict safety constraints;

using the LLM only for explanation and recommendation drafting;

retaining human approval for every vehicle-assignment decision;

treating all operational performance figures as assumptions until real data is available;

choosing GO for a limited internal prototype, not GO for production deployment.

1. What I Learned

I learned that AI product design should begin with an operational problem rather than with a preferred technology. A clear actor, workflow, bottleneck, and success metric are necessary before deciding whether AI is appropriate.

I also learned that not every part of an AI-enabled system should be handled by an LLM. Deterministic rules are more appropriate for strict, testable safety constraints, while an LLM is useful for interpreting context and producing human-readable explanations.

Human-in-the-loop design is essential when recommendations affect real-world operations. The final system should help the dispatcher make a faster and more consistent decision without removing the dispatcher's authority or accountability.

1. Final Reflection

AI accelerated ideation, comparison, drafting, and adversarial testing, but it did not replace my judgment. The most important improvement to the original concept came from limiting the AI's authority and separating safety logic from language generation.

Based on this exercise, I would proceed with BatteryGuard AI only as a limited prototype. Before any production use, the assumptions, thresholds, performance targets, and workflow fit must be validated using real operational data and feedback from Xanh SM dispatchers.
