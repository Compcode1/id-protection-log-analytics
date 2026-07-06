# Project Summary: Global Identity Isolation and Telemetry Streaming



### What We Attempted to Accomplish (The Blueprint)  

The core objective of this project was to build a complete, closed-loop identity security pipeline that could automatically detect a risky login, force a user to secure their account, and stream the entire operational footprint into a centralized corporate database for permanent storage. 

To achieve this, the architecture required connecting two entirely different cloud environments:

1. **The Identity Environment (Microsoft Entra admin center):** This is where we managed our test user accounts and attempted to configure a Microsoft Entra ID Conditional Access policy. The goal of this policy was to act as a security gatekeeper—monitoring incoming logins, identifying environmental anomalies (like an unrecognized private browsing session), and challenging the user to perform a security remediation action like an explicit password reset or Multi-Factor Authentication challenge.
2. **The Infrastructure Environment (The Azure portal):** Because the identity platform does not store long-term activity logs natively, we had to leave the Microsoft Entra admin center entirely and log into the primary Azure portal. In this environment, we utilized an active Azure subscription to purchase and deploy an Azure Log Analytics Workspace. This workspace was intended to serve as our centralized security database.

The final piece of the project was to build a bridge between these two worlds by configuring a Microsoft Entra ID Diagnostic Setting. This setting was designed to automatically capture every single login event from the identity side and stream it directly across the portal boundary into the Azure Log Analytics Workspace database for long-term auditing and verification.

---

### What Didn't Work and Why It Broke Down

While the structural concept of the pipeline was sound, the project suffered a catastrophic breakdown during the live testing and validation phase due to severe friction within the cloud platform management layers:

* **Portal Dependency Locks:** When we attempted to fine-tune our security rules inside the Microsoft Entra admin center, the cloud platform's built-in validation rules locked the interface. The policy engine enforces strict, hidden dependencies—such as forcing specific risk levels to remain active if a password change control is checked—which triggered generic interface errors and greyed out our configuration options.
* **Browser Cache Contamination:** During live login testing, the local web browser persistently cached previous session credentials. This prevented clean, isolated testing of our new test accounts, forcing creative workarounds like generating entirely new identities just to get a clean authentication attempt.
* **User Interface Layout Desync:** The ultimate failure occurred after a test user successfully triggered and completed a security remediation flow. When we jumped over to the Azure portal to verify that the log data had successfully streamed into our Azure Log Analytics Workspace, the database console layout failed to display a workable text terminal. The portal interface loaded into a confusing, empty state that lacked responsive input lines, leaving us completely unable to run verification queries despite the backend infrastructure being actively deployed.

---

### Future Project Realignment

This project demonstrated that managing complex identity configurations and infrastructure data pipelines solely through web-based graphical user interfaces introduces severe operational risk, layout confusion, and unpredictable interface locks. 

To ensure the success of our next project and eliminate multi-hour portal troubleshooting bottlenecks, we will completely abandon manual portal click-throughs. Future security architectures will be built using highly predictable, straight-line engineering workflows. We will deploy our resources utilizing local automation scripts, programmatic direct Application Programming Interface deployments, or local command-line interfaces, keeping our engineering focus entirely on building clean, verifiable security solutions.
