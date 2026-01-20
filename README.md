# Radiotherapy-capacity-control
# Variability-Aware Radiotherapy Scheduling & Capacity Control

## Problem Context
Radiotherapy departments operate under persistently high demand and
tightly coupled workflows involving simulation, planning, and treatment
delivery. In real-world clinical settings, daily schedules routinely
destabilize due to an unpredictable mix of new patient starts,
simulations, and ongoing treatment fractions.

Although total treatment demand is well understood, departments suffer
from cascading delays, prolonged patient waiting times, underutilized
linear accelerators, and high operational stress. The failure is not poor
planning or inaccurate time estimates, but the absence of a system-level
mechanism to **control how workflow variability propagates across the
day**.

---

## Core Insight
Most scheduling systems implicitly assume that variability should be
eliminated or predicted away. This project treats variability as an
**irreducible operational property** and instead focuses on **containing
its spread**.

The central idea is that not all radiotherapy sessions behave similarly:
some are intrinsically more time-variable than others. When variability
is allowed to propagate unchecked through a tightly coupled schedule,
small overruns escalate into system-wide instability.

This project introduces **variability-aware control** as a missing
operational layer in radiotherapy scheduling.

---

## System Architecture (Conceptual)
The system operates as a supervisory control layer composed of three
coordinated components:

1. **Variability Classification Layer**  
   Radiotherapy sessions are classified based on observed workflow
   variability patterns using existing HIS/OIS timestamps, rather than
   assumed fixed durations.

2. **Elastic Scheduling & Buffering Layer**  
   Time blocks and strategic buffers are dynamically assigned to absorb
   disruption, protecting downstream treatments from cascading delay.

3. **Capacity Intelligence Layer**  
   The system measures effective capacity loss and recovery caused by
   variability propagation, making hidden operational inefficiencies
   visible to schedulers.

These layers work together to stabilize the treatment day without
requiring precise duration prediction.

---

## Clinical Integration
The framework is designed as clinical decision support and integrates
with existing enterprise platforms such as Varian ARIA and Elekta
MOSAIQ.

- Preserves existing clinical workflows  
- Allows full human override at all times  
- Adapts gradually based on observed operational behavior  
- Does not automate clinical decision-making  

The goal is **stability**, not automation.

---

## Novel Contribution
Existing enterprise platforms primarily manage appointments and
documentation, assuming fixed treatment times. Research schedulers often
attempt optimization or duration prediction, but remain fragile under
high variability.

This project introduces a **first-class operational treatment of
variability**, classifying sessions by variability behavior rather than
nominal duration and actively preventing disruption from propagating
through the schedule.

It adds a missing control layer that stabilizes throughput without
relying on prediction accuracy.

---

## Hackathon Output
Under time-constrained build conditions, this project resolves into:

- A variability-classification scheme based on observed timestamps  
- A supervisory scheduling flow defining where buffers are placed  
- A capacity-visibility model showing lost vs recovered treatment time  

The focus is reasoning clarity and operational stability, not
performance metrics.

---

## Explicit Exclusions
- No clinical outcome prediction  
- No treatment prioritization or triage  
- No automation of scheduling decisions  
- No accuracy or optimization claims  

This repository documents a **systems-level operational contribution**,
not a deployable clinical product.
## Supervisory Variability Control Flow

```mermaid
flowchart TD
    A[Daily Schedule Initiated] --> B[Session Variability Classification]

    B --> C{High Variability?}
    C -->|Yes| D[Assign Elastic Block / Buffer]
    C -->|No| E[Standard Scheduling]

    D --> F[Protect Downstream Sessions]
    E --> F

    F --> G[Measure Capacity Loss / Recovery]
    G --> H[Scheduler Visibility & Override]
