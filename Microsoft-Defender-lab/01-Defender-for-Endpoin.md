# Learning Path 4 — Lab 1, Exercise 2  
## Mitigate Attacks with Microsoft Defender for Endpoint

## Purpose

Validate Microsoft Defender for Endpoint onboarding, generate a safe test detection, investigate the resulting alert and incident, then classify it as authorized security testing.

## Environment

- **Portal:** `https://security.microsoft.com`
- **Endpoint:** `WIN1`
- **Account:** Tenant Administrator
- **Simulated activity:** Suspicious PowerShell command line

## Task 1 — Verify onboarding

1. Open:

   ```text
   Assets > Devices
   ```
<img width="1377" height="777" alt="image" src="https://github.com/user-attachments/assets/ce244c23-99ce-4e37-96d5-418e9778ee19" />

2. Confirm `WIN1` appears in the device inventory.

3. Open:

   ```text
   System > Settings > Endpoints > Device management > Onboarding
   ```

4. Select:

   ```text
   Windows 10 and 11
   ```

5. Confirm onboarding status:

   ```text
   First device onboarded: Completed
   ```

6. Under **Run a detection test**, copy the supplied test script.

7. On `WIN1`, open an elevated Command Prompt:

   ```text
   Windows Search > CMD > Run as administrator
   ```

8. Approve UAC, paste the script, and press Enter.

### Expected result

- Command Prompt closes automatically after successful execution.
- Defender XDR generates alerts after a few minutes.
- This confirms endpoint telemetry, onboarding, and detection capability are working.

### Troubleshooting

If `WIN1` does not appear after about one hour, investigate:

- Onboarding-policy deployment.
- Network, proxy, or firewall connectivity.
- Endpoint connectivity to Microsoft Defender services.
- Defender service or local endpoint issues.

***

## Task 2 — Investigate alert

1. Open:

   ```text
   Investigation & response > Incidents & alerts > Alerts
   ```

2. Select:

   ```text
   [TestAlert] Suspicious PowerShell commandline
   ```

3. Review:
   - **Alert story timeline**
   - **Details**
   - **Recommendations**

4. In **Incident details**, open:

   ```text
   Execution incident on one endpoint
   ```

### Why PowerShell matters

PowerShell is legitimate administration tooling but is also frequently abused for execution, download activity, discovery, persistence, and credential theft. Alert context—not the alert name alone—determines whether activity is malicious.

***

## Task 3 — Manage incident

1. Open:

   ```text
   Incidents & alerts > Incidents
   ```

2. Clear the **Alert severity** filter if necessary.

3. Select:

   ```text
   Execution incident on one endpoint
   ```

4. Select **Manage incident**.

5. Add the tag:

   ```text
   Simulation
   ```

6. Assign the incident:

   ```text
   Assign to > Me
   ```

7. Classify it as:

   ```text
   Informational, expected activity > Security testing
   ```

8. Select **Save**.

***

## Tabs to review

| Tab | Purpose |
|---|---|
| **Attack story** | Shows the sequence and relationships of relevant activity; real incidents may show an incident graph. |
| **Alerts** | Displays correlated detections. |
| **Assets** | Shows affected devices and users. |
| **Investigations** | Contains investigation activity and automated-investigation results, where available. |
| **Evidence and response** | Shows artifacts, findings, and potential response actions. |
| **Summary** | Consolidates incident status, classification, owner, and key details. |

> If a tab is hidden, open the ellipsis menu: `...`

***

## Key SOC takeaways

- **Alert:** A single detection of suspicious activity.
- **Incident:** A correlated collection of related alerts, evidence, assets, and investigation context.
- **Onboarding:** Connects an endpoint to Defender for Endpoint so it can send telemetry and be monitored.
- **Incident owner:** The analyst accountable for investigation and closure.
- **Incident tag:** A label used for filtering and reporting, such as `Simulation`.
- **Expected activity:** Use only after confirming the activity is authorized and intentional.
- In a real investigation, validate the command line, time, user, device, source, authorization, related alerts, and evidence before classifying an incident as benign.

## Workflow to remember

```text
Onboard device
→ Verify device inventory
→ Run controlled detection test
→ Review alert
→ Pivot to incident
→ Analyze attack story, assets, alerts, and evidence
→ Assign owner and tag
→ Classify and save
```
