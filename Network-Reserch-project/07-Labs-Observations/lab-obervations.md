**Project:** Elite Network Architecture – ISP + Enterprise Hybrid (Nepal Context)  
**Purpose:** Validate network architecture, traffic, failure, and attack assumptions without using tools. Observations are the key artifact.

---

## 1️⃣ Traffic Baseline Capture

**Objective:** Define “normal” network behavior for comparison.

**Procedure (Thinking Only):**

- Identify key traffic flows: DNS, HTTP/HTTPS, email, internal DB queries
- Note volume, frequency, and source/destination zones

**Observations:**

- Most traffic is north–south (user ↔ Internet) in DMZ zones
- East–west flows are minimal in segmented enterprise VLANs
- Traffic patterns show predictable peak times (office hours)
- Deviations from this baseline would be first signal of anomaly

**Validation:**

- Confirms that trust boundaries and segmentation limit east–west exposure
- Provides reference for detecting later attack-like behavior

---

## 2️⃣ Link / Path Failure Simulation

**Objective:** Understand impact of infrastructure failure on traffic flow.

**Simulation (Conceptual):**

- Consider core router failure or fiber cut
- Mentally trace rerouted traffic and affected zones

**Observations:**

- North–south traffic re-routes within aggregation layer with minimal delay
- Certain services experience transient unreachability due to failover delay
- Segmentation prevents cascade into unrelated zones

**Validation:**

- Confirms blast-radius assumptions from architecture diagram
- Confirms routing logic allows predictable recovery under failure

---

## 3️⃣ Attack-Like Behavior Simulation

**Objective:** Validate network-visible signals of compromise.

**Simulation (Conceptual):**

- Repeated authentication failures (credential abuse)
- Abnormal DNS or SMB queries (lateral movement)

**Observations:**

- Auth anomalies appear first at boundary authentication servers
- East–west traffic spikes in affected segments are detectable against baseline
- Segmentation slows lateral movement

**Validation:**

- Confirms discrimination logic between failure vs attack signals
- Supports attack-path reasoning artifact (Week 5)

---

## 4️⃣ Recovery Verification

**Objective:** Ensure architecture contains failures and recovers predictably.

**Simulation (Conceptual):**

- Restore failed link or service
- Trace traffic restoration and functional recovery

**Observations:**

- Traffic returns to baseline routes without impacting unrelated zones
- Some services temporarily limited due to session resets, but blast radius contained
- Monitoring plane placement allows early detection of recovery anomalies

**Validation:**

- Confirms design intent for resilience and containment
- Supports architecture tradeoff and limitations artifact (Week 5)

---

## ✅ Summary of Lab Observations

1. **Segmentation and trust boundaries work as intended** — east–west traffic contained.
2. **Traffic baseline is a reliable reference** for detecting anomalies.
3. **Failures are predictable**; architecture supports recovery without cascading impact.
4. **Attack-like behavior produces network-visible deviations**; detectable without tools.
5. **Lab artifacts validate, not create, architecture decisions** — nothing added here that wasn’t thought through previously.

**Conclusion:**  
These labs do not measure skill at packet capture or tool usage. They validate **thinking, architecture assumptions, and signal detection logic**, creating a **defensible, interview-ready artifact**.