**Project:** Elite Network Architecture – ISP + Enterprise Hybrid (Nepal Context)  
**Purpose:** Document tradeoffs, limitations, and engineering honesty.

---

## 1️⃣ Tradeoffs Made

|Decision|Tradeoff Reason|Impact|
|---|---|---|
|Limited east–west monitoring|Cost & complexity|Some lateral movement may go unnoticed initially|
|Minimal redundancy in low-risk zones|Budget|Small outages possible, but blast radius contained|
|Legacy system inclusion|Operational necessity|Increased attack surface, mitigated with segmentation|

---

## 2️⃣ Limitations

- **Detection without SIEM:** Pure network-observable anomalies are limited in granularity.
- **Trust boundaries are human-dependent:** Misconfigurations may override segmentation.
- **Capacity vs cost:** High resilience everywhere is cost-prohibitive; prioritized zones only.

---

## 3️⃣ Future Evolution

- Add SIEM / correlation for fine-grained attack vs failure discrimination.
- Extend monitoring into previously low-risk zones if budget allows.
- Phase out legacy systems gradually, reducing overall exposure.

---

## 4️⃣ Interview Positioning

- Can explain **why each risk exists** and **what architectural design mitigates it**.
- Demonstrates **senior-level engineering honesty**, critical for architecture roles.
- Defensible, no tool dependence — purely architecture and reasoning.