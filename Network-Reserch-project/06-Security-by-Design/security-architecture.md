.

---

# 📄 security-architecture.md

## 1️⃣ Security Philosophy

This architecture assumes that **perimeter defenses may fail** and prioritizes **containment of internal compromise**. Security controls focus on **protecting control-plane infrastructure**, **limiting lateral movement**, and **ensuring failure containment**.

All systems, users, and network segments are treated as **semi-hostile**, and the design accepts that **perfect security is impossible**. Decisions are made explicitly with **risk tradeoffs**: operational efficiency is balanced with containment, visibility, and critical service protection.

> Key mindset: “We assume compromise, trace failure chains, and limit damage rather than chasing impossible perfection.”

---

## 2️⃣ Trust Boundaries

The network is divided into **logical trust zones**:

- **Internet:** fully untrusted external traffic.
- **ISP Edge:** minimal trust; only validated routing and filtered connections allowed.
- **Enterprise Internal Network:** semi-trusted; most services and users exist here, but trust is continuously verified.
- **Management Network:** highly restricted; hosts administrative interfaces and critical infrastructure management.
- **Critical Infrastructure Zone:** contains DNS, routing, identity, and monitoring systems; access is strictly limited, and failure here is mitigated through redundancy and monitoring.

> Boundaries exist to **limit blast radius, prevent uncontrolled lateral movement, and isolate critical systems**, not just to segment traffic physically.

---

## 3️⃣ Critical Infrastructure Protection

The systems most sensitive to compromise include:

- **Routing Infrastructure:** misconfiguration or compromise can redirect traffic, bypass controls, or isolate monitoring.
- **Identity Services:** compromise enables privilege escalation and lateral movement.
- **DNS:** inaccurate resolution can redirect users and services to malicious endpoints.
- **Monitoring Infrastructure:** if disabled or bypassed, internal compromise may remain undetected.

> These systems receive **extra protections** in design, not through specific tools, but via **network isolation, segmentation, and failure containment logic**.

---

## 4️⃣ Segmentation Strategy

Segmentation is **damage containment**, not a prevention mechanism.

- **Limits lateral movement:** attackers cannot freely hop between segments.
- **Protects sensitive systems:** critical infrastructure resides in zones with minimal trust exposure.
- **Reduces blast radius:** failure in one zone does not cascade across the network.

Segmentation decisions balance **operational simplicity, usability, and security**; overly complex segmentation can hurt operations, so risk tradeoffs are explicitly accepted.

> Mental model: Segments = fire compartments; the fire may start, but it cannot destroy the whole structure.

---

## 5️⃣ Accepted Risks (Critical)

- **Low-volume internal lateral movement** may go undetected for short periods.
- **Identity infrastructure** represents a high-impact failure point but must remain accessible for operational efficiency.
- Some internal services require **broader access** due to business or operational necessity.
- **Monitoring gaps** exist for low-priority systems; detection is prioritized on critical zones.


-----------------

                    ┌─────────────┐
                    │   Internet  │
                    │  (Untrusted)│
                    └─────┬───────┘
                          │
                          ▼
                    ┌─────────────┐
                    │  ISP Edge   │
                    │ Minimal Trust│
                    └─────┬───────┘
                          │
            ┌─────────────┼─────────────┐
            │                           │
            ▼                           ▼
    ┌──────────────┐            ┌──────────────┐
    │  Enterprise  │            │ Management   │
    │ Internal NW  │            │ Network      │
    │ Semi-Trusted │            │ Highly       │
    │ Zones        │            │ Restricted   │
    └─────┬────────┘            └─────┬────────┘
          │                             │
          ▼                             ▼
    ┌──────────────┐            ┌──────────────┐
    │Critical      │            │Monitoring &  │
    │Infrastructure│            │Control Plane │
    │DNS, Routing, │            │Integrity     │
    │Identity      │            │Systems       │
    └──────────────┘            └──────────────┘