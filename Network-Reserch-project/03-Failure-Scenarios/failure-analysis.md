**Date:** march-1-2026

## Section 1 — Infrastructure Failures

### 1️⃣ Scenario: Core Device Failure

**Description:**  
A central routing device stopped forwarding traffic entirely. The failure was instantaneous, impacting both the control and data planes, with redundancy present but centralized routing topology amplifying the effect.

**First Signal Observed:**

- Monitoring system alerts device unreachable (T0+10s)
    
- Multiple session resets observed (T0+20s)
    
- User complaints escalate within affected zones (T0+30s)
    

**Blast Radius Analysis:**

- Entire organization-wide zone temporarily affected due to centralized topology
    
- Segmented zones experienced limited impact due to alternate core paths
    
- Control plane failure propagated immediately, causing routing adjacency flaps
    
- Secondary instability minimal, contained by redundant uplinks
    

**Detection Path:**

- Monitoring server raised critical alert first
    
- Network engineers received automated notifications via NOC dashboard
    
- User complaints corroborated monitoring data
    

**Time to Clarity:**

- Approximately 2–3 minutes due to clear monitoring signals and consistent symptom pattern
    
- Clarity delay increased slightly in zones where monitoring relied on the same core
    

**Recovery Logic:**

- Automatic failover to redundant core path initiated immediately
    
- Routing reconvergence completed in T0+90s
    
- Manual verification of traffic flows and adjacency stabilization
    

**Architectural Decision That Contained Damage:**

- Redundant core paths with pre-configured failover
    
- Segmentation of zones limited the blast radius
    
- Independent monitoring plane ensured early detection
    

---

### 2️⃣ Scenario: Fiber Cut

**Description:**  
A primary fiber link between two regional sites was severed. Devices remained operational, but the physical path failure triggered data-plane rerouting and partial control-plane adjustments.

**First Signal Observed:**

- Interface down alert from affected ports (T0+15s)
    
- Routing reconvergence notices (T0+30s)
    
- Latency spike detected on backup path (T0+45s)
    
- Users in one site report degraded service
    

**Blast Radius Analysis:**

- Directly impacted zones: the two connected sites
    
- Secondary congestion occurred on backup links due to rerouted traffic
    
- Control plane minimally affected; data plane degraded temporarily
    
- Blast radius constrained by segmentation and alternate paths
    

**Detection Path:**

- Network monitoring interface alert
    
- NOC engineers correlate interface down with traffic rerouting logs
    
- End-user complaints validate degraded service
    

**Time to Clarity:**

- Approximately 15–30 minutes
    
- Delay due to symptom overlap with potential congestion or DDoS scenarios
    
- Clear once interface alerts and traffic patterns correlated
    

**Recovery Logic:**

- Automatic reroute through secondary fiber links
    
- Manual validation of traffic balancing
    
- Long-term remediation involved fiber repair
    

**Architectural Decision That Contained Damage:**

- Segmented topology restricted impact to affected sites
    
- Redundant paths enabled rapid recovery
    
- Monitoring separation prevented blind spots during rerouting
    

---

### 3️⃣ Scenario: Route Leak

**Description:**  
An internal router propagated incorrect prefixes to upstream peers, resulting in traffic misrouting across multiple regions. No physical device failed.

**First Signal Observed:**

- Partial service outages in geographically dispersed zones (T0+10min)
    
- Latency anomalies detected in multiple paths
    
- Users report intermittent connectivity issues
    
- Initial monitoring shows devices healthy, no alarms triggered
    

**Blast Radius Analysis:**

- Affected multiple zones logically, independent of physical topology
    
- Silent failure propagated quickly through control plane
    
- Secondary instability occurred as traffic followed incorrect paths
    
- High potential to impact external peers if leak unchecked
    

**Detection Path:**

- End-user reports highlighted abnormal connectivity
    
- Traffic path analysis indicated deviation from expected prefixes
    
- Correlation with routing logs identified origin of leak
    

**Time to Clarity:**

- 45–60 minutes
    
- High clarity delay due to silent propagation and symptom ambiguity
    
- Difficulty exacerbated by overlapping potential causes (congestion, DDoS, policy misconfiguration)
    

**Recovery Logic:**

- Withdrawn incorrect prefixes immediately upon identification
    
- Re-injection of correct routes
    
- Verification of route propagation stability and convergence
    
- Postmortem analysis updated dependency documentation
    

**Architectural Decision That Contained Damage:**

- Route aggregation and prefix filtering reduced propagation scope
    
- Segmentation prevented complete system-wide outage
    
- Independent monitoring of route consistency enabled eventual detection
    

---

## Section 2 — Defense Drill Answers

**Why didn’t this collapse everything?**

- Redundancy, segmentation, route aggregation, and independent monitoring limited systemic propagation.
    

**What assumption makes this worse?**

- Assuming monitoring is infallible or that humans will react perfectly; relying solely on a single core path or unsegmented links.
    

**What dependency did we forget?**

- Monitoring dependent on the same network segment as failed components; delayed human verification; implicit routing dependencies.
    

**Which monitoring blind spot increases clarity delay?**

- Route leaks propagating silently across segments with no prefix consistency alerts; monitoring systems dependent on affected paths.
    

**Which failure scares you most — and why?**

- Route leaks; because they spread silently, mimic attacks, propagate logically beyond physical containment, and clarity delay is maximal.
    

---

## Section 3 — Systemic Insight & Risk Reasoning

- **Core device failures** are dramatic, loud, and easily contained but test redundancy and failover effectiveness.
    
- **Fiber cuts** are geographically constrained but amplify secondary congestion and stress alternative paths.
    
- **Route leaks** are silent, logic-based, and can create global-scale propagation before detection, stressing visibility and operational discipline.
    

**Clarity Delay Principle:**

- Every failure has a unique clarity delay. Managing it is the primary design control architects exercise.
    

**Containment Principle:**

- Damage containment relies on: segmentation, aggregation, policy isolation, redundant paths, and monitoring independence.
    

**Human Factor Principle:**

- Any system must tolerate operator errors. Panic multiplies blast radius.
    

**Final Takeaway:**

- Network stability is **not just hardware resilience**, but **systemic design awareness**, **observability**, and **human factor management**