## Designing Network-Visible Attack Paths

**Project:** Elite Network Architecture – ISP + Enterprise Hybrid (Nepal Context)  
**Purpose:** Reason about attack progression purely from network-observable signals to inform architecture defense decisions. No exploitation or tool usage is described.

---

## 1️⃣ Introduction

Modern networks are not secure because of perimeter defenses alone. Attackers often enter via social engineering, misconfigurations, or exposed services. This document models **how attacks would appear in network traffic**, how they propagate internally, and how architectural decisions mitigate risk.

The approach focuses on **network visibility**, trust boundaries, segmentation, and blast-radius limitation. This reasoning allows architects to **anticipate failure vs attack signals** and design resilient systems.

---

## 2️⃣ Initial Access

**Vector Examples:**

|Vector|First Network Signal|Trust Boundary Impact|Notes|
|---|---|---|---|
|Phishing → workstation compromise|Unusual outbound connections, unexpected SMB/DNS requests|User LAN → Internal services|Signals may resemble user error or application update; requires correlation|
|Exposed web service|Traffic from unknown IP, unexpected session frequency|Internet → DMZ|Volume low initially; lateral movement may begin once foothold exists|
|Misconfigured VPN / RDP|Login attempts outside normal hours|Internet → VPN termination|Often appears as legitimate session at first; baseline comparison critical|

**Observations:**

- Initial access rarely looks like “malicious” at the packet level.
- Trust boundary crossing is the first architectural red flag.
- Detection relies on **network pattern deviation**, not signature scanning.

---

## 3️⃣ Lateral Movement

Once inside, attackers move **east–west**, targeting higher-value systems.

**Movement Patterns:**

|Method|Observable Network Signal|Segmentation Impact|Blast Radius Limitation|
|---|---|---|---|
|SMB / RDP access|New session initiation in non-user VLANs|VLAN segmentation slows spread|Only hosts in same segment initially exposed|
|Credential reuse / Kerberos|Authentication anomalies, ticket requests outside baseline|Critical domain boundaries|Containment depends on account scope|
|RPC / service hopping|Multiple unusual connections across services|Zone-to-zone segmentation|Blast radius can be contained if controls enforce zone isolation|

**Architectural Takeaway:**

- Segmentation and trust boundaries **reduce lateral movement speed**.
- Traffic deviation is subtle; requires mental mapping of baseline vs observed patterns.

---

## 4️⃣ Privilege Escalation

Elevating privileges enables access to sensitive infrastructure.

**Network Signals:**

|Escalation|Network-Visible Change|Zone Impact|Notes|
|---|---|---|---|
|Admin account compromise|Sudden new management plane sessions (SSH, RDP)|Management network|High-value traffic; rapid detection possible|
|Database access|Internal traffic to DB servers increases|Database VLAN|Often resembles legitimate maintenance traffic; requires correlation|
|Service account abuse|Unexpected cross-service flows|Multi-zone|Architecture must accept some risk; monitoring placement is critical|

**Observations:**

- Escalation often produces **small but critical deviations**.
- Systems with high trust assumptions (identity servers, routing infrastructure) represent **single points of failure**.

---

## 5️⃣ Command & Control (C2) Logic

C2 traffic is visible only indirectly in network flows:

- **Beaconing:** periodic, low-volume traffic from internal host to external C2 endpoint.
- **Data exfiltration:** unusual outbound volume, asymmetric traffic patterns.
- **Protocol anomalies:** DNS tunneling, HTTP request spikes.

**Architectural Reasoning:**

- Monitoring placement at **boundary aggregation points** improves detection.
- Segmented zones can slow or block lateral exfiltration.
- Timing and flow direction analysis is more important than protocol inspection.

---

## 6️⃣ Key Takeaways

1. **Network Visibility Matters:** Even without tools, traffic anomalies reveal compromise.
2. **Segmentation and Trust Boundaries Limit Damage:** East–west movement is slowed; blast radius is contained.
3. **Accepted Risks:** Some services and accounts require broad access; architects must document these exposures.
4. **Detection vs Failure Confusion:** Always model whether observed anomaly could be failure or attack.
5. **Defensible Architecture:** All reasoning focuses on **network impact**, not exploit mechanics.

---

### ✅ Conclusion

This document bridges **security theory and network architecture thinking**. By modeling attack paths purely from traffic patterns, trust boundaries, and segmentation logic, we gain a **defensible design framework** for interviews, architecture reviews, and resilience planning.