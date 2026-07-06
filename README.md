### Project 7 Deployment Documentation: Identity Risk Mitigation & Diagnostic Telemetry Routing

**Project Objective:**
To implement real-time identity threat mitigation by deploying Microsoft Entra ID Protection risk policies and establishing an automated diagnostic stream that routes user risk telemetry directly to an Azure Log Analytics workspace for deep analytics auditing.

**Initial State:**
User sign-ins and behavioral anomalies are evaluated by the default security heuristics, but no automated block or remediation enforcement actions exist for high-risk events. Furthermore, identity risk logs are limited to short-term portal retention rather than long-term diagnostic workspace aggregation.

**Configuration Procedure:**
1. Sign into the Microsoft Entra admin center using an identity with the global rights of a Global Administrator (GA).
2. Navigate to **Protection > Identity Protection > User risk policy**.
3. Under **Assignments**, scope the policy to **All users**, and under **Conditions**, set the **User risk** level to **High**.
4. Under **Controls > Access**, select **Require password change** (which implicitly triggers a secure Multi-Factor Authentication (MFA) check via Self-Service Password Reset (SSPR) protocols). Set the policy enforcement toggle to **On** and select **Save**.
5. Navigate to **Identity > Monitoring & health > Diagnostic settings**.
6. Click **Add diagnostic setting** to establish a new telemetry data path.
7. Enter the configuration name "Identity-Protection-Risk-Stream".
8. Under the **Logs > Categories** section, select the checkboxes for **RiskyUsers** and **UserRiskEvents** to isolate the risk log streams.
9. Under **Destination details**, check the box for **Send to Log Analytics workspace**, select your active subscription, and target your primary Azure Log Analytics workspace. Click **Save**.

**Verification Matrix:**

| Security Target | Policy Type Configuration | Expected Destination Architecture | Actual Testing Outcome |
|---|---|---|---|
| Microsoft Entra ID Tenant | Diagnostic telemetry routing rules. | Log events stream into the Azure Log Analytics workspace under specialized threat log tables. | Pending |

**Validation Steps:**
1. Within the main Global Administrator (GA) session, navigate to **Identity > Monitoring & health > Log Analytics**.
2. Open the query workspace tool connected to your Azure Log Analytics workspace destination.
3. Execute a basic diagnostic query using Kusto Query Language (KQL) by entering `SigninLogs` or `UserRiskEvents` into the query editor pane.
4. Click the execution command button and verify that the platform successfully runs the database query without throwing access exceptions, demonstrating that the workspace table schema is active and listening for tenant log streams.
