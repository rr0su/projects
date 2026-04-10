# Failure vs Attack Discrimination Matrix

Purpose:
Quick reasoning reference for distinguishing normal infrastructure failures from potential attacks using observable network signals.

---

Symptom: Packet Loss
Failure Cause: Network congestion
Attack Cause: Volumetric DDoS
How I Differentiate:
Congestion usually shows large flows from a few known systems.
DDoS traffic usually comes from many distributed sources with abnormal packet rates.

---

Symptom: Authentication Failures
Failure Cause: Misconfiguration or password policy change
Attack Cause: Credential brute force
How I Differentiate:
Normal failures involve a few users with slow retry attempts.
Brute force attempts show high-frequency login attempts or multiple source addresses.

---

Symptom: DNS Slow Response
Failure Cause: Cache miss or overloaded DNS resolver
Attack Cause: DNS tunneling or data exfiltration
How I Differentiate:
Normal DNS traffic shows predictable query patterns.
Tunneling often produces high-entropy queries or unusual domain patterns.

---

Symptom: East–West Traffic Spike
Failure Cause: Backup job or system scanning
Attack Cause: Lateral movement after host compromise
How I Differentiate:
Operational jobs usually originate from known infrastructure servers.
Lateral movement often starts from a user workstation accessing multiple internal systems.

---

Symptom: Route Change
Failure Cause: Configuration update or routing convergence
Attack Cause: Route leak or BGP hijack
How I Differentiate:
Legitimate changes match expected routing policy updates.
Unexpected announcements or path changes from unknown peers indicate possible hijack.

---

Symptom: Traffic Saturation
Failure Cause: Large file transfers or scheduled backups
Attack Cause: Network flooding attack
How I Differentiate:
Legitimate transfers usually involve known endpoints and predictable flow sizes.
Flooding attacks show sudden high packet rates from many sources.

---

Symptom: Abnormal Authentication Session Activity
Failure Cause: User error or repeated login attempts
Attack Cause: Credential abuse or Kerberos ticket abuse
How I Differentiate:
Normal activity is limited to one user session.
Credential abuse often creates multiple simultaneous sessions or rapid ticket requests.