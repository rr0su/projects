## 1️⃣ Normal Traffic Baseline

This network supports typical enterprise activity. Traffic patterns are predictable when viewed from a business perspective rather than a protocol perspective.

### North–South Traffic (Crossing Trust Boundaries)

The main external traffic consists of:

- Users accessing SaaS platforms and public internet services.
    
- Remote users connecting through secure access gateways.
    
- Partner API connections entering controlled ingress points.
    

This traffic crosses trust boundaries. Because of that, it is monitored and policy-controlled more heavily than internal traffic. Failures here are visible immediately — users notice quickly if external access breaks.

The purpose of this traffic is direct business enablement: communication, collaboration, customer interaction, and external service consumption.

---

### East–West Traffic (Internal System Movement)

Internal traffic supports application functionality and system coordination. This includes:

- Application servers communicating with database servers.
    
- Identity services validating users and issuing tokens.
    
- Internal API calls between services.
    
- Backup and replication traffic.
    

This traffic stays inside trusted zones but carries higher lateral movement risk if segmentation is weak. Failures in this domain are often less visible at first but can cause cascading problems over time.

Most major breaches and deep outages expand through east–west movement, not north–south entry.

---

### Control-Plane Traffic

Certain traffic keeps the entire environment operational:

- Routing exchanges with upstream providers.
    
- DNS resolution.
    
- Authentication and directory lookups.
    
- Time synchronization.
    
- Certificate validation.
    

These flows do not generate revenue directly, but without them, revenue-generating services fail. Control traffic is treated as foundational and must remain reachable during partial failures.

---

### Monitoring and Observability

Logging, telemetry, and metrics export traffic ensure that issues can be detected and investigated. While temporary monitoring degradation may not immediately stop business functions, prolonged loss of visibility significantly increases operational risk.

Monitoring traffic is protected to avoid blind spots during outages or abuse conditions.

---

## 2️⃣ Routing Intent

Routing decisions at the enterprise edge are not driven only by shortest distance. They are influenced by business, risk, and stability considerations.

External connectivity uses dynamic routing to coordinate with multiple upstream providers. This allows the enterprise to:

- Express path preferences.
    
- Handle provider failures.
    
- Manage inbound and outbound movement more flexibly.
    

Routing policy is influenced by:

- ISP cost and capacity agreements.
    
- Peering relationships with important SaaS and cloud providers.
    
- Risk exposure to unstable or attack-prone upstreams.
    
- Legal or compliance considerations affecting data movement.
    

Because of this, the chosen path is not always the lowest-latency option.

**Path optimality is secondary to policy stability.**

Route aggregation is used at defined boundaries to prevent localized instability from affecting the entire routing domain. Summarization helps contain failures and simplifies operational recovery.

Inbound traffic paths are not fully controllable, so asymmetric routing is expected and accounted for in inspection design.

The routing philosophy prioritizes survivability and containment over minimal latency.

---

## 3️⃣ Behavior Under Failure or Abuse

The design assumes that failures and abuse events will occur. Routing and traffic policies are structured to preserve core functions first.

### Upstream Provider Failure

If a primary external provider fails:

- Traffic shifts to available providers.
    
- Some performance degradation is acceptable.
    
- External connectivity must remain available.
    

Connectivity continuity is prioritized over optimal performance.

---

### Internal Service Failure

If an internal application tier fails:

- The issue remains contained within its trust zone.
    
- Aggregation boundaries prevent unnecessary external routing churn.
    
- Control-plane services remain reachable to support recovery.
    

---

### Control-Plane Protection

Control services such as DNS, identity, and routing adjacencies must remain reachable even during partial outages or mitigation events. These flows must not be rate-limited or redirected through untrusted zones.

Loss of control-plane stability leads to broad systemic impact and is therefore treated as a top survivability priority.

---

### Abuse or Attack Conditions

During volumetric abuse:

- External filtering and provider coordination absorb excess traffic.
    
- Internal segmentation limits lateral movement.
    
- Monitoring flows remain active to preserve visibility.
    

Sensitive internal zones are not allowed to reroute through untrusted external paths under any condition.

Containment is prioritized over optimization.

---

## Closing Perspective

This architecture models traffic as business-driven movement shaped by policy and bounded by trust zones.

Routing decisions reflect contractual, operational, and risk considerations rather than pure path distance. Failures are expected, asymmetry is normal, and aggregation is used to contain instability.

Control-plane survivability is prioritized first, revenue traffic second, and performance optimization only within those constraints.