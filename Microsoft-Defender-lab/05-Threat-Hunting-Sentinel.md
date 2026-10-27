# Learning Path 8 — Lab 1, Exercise 1  
## Create a Playbook in Microsoft Sentinel

## Purpose

Create a Microsoft Sentinel **playbook** based on an existing ransomware-response template, configure its connection, and create an **automation rule** that runs the playbook for selected incident tactics.

A Sentinel playbook is an Azure Logic Apps workflow used to automate and orchestrate incident-response actions. An automation rule determines **when** Sentinel should run an action, such as triggering a playbook. [learn.microsoft](https://learn.microsoft.com/en-us/azure/sentinel/automation/run-playbooks)

> **Lab note:** This is a standalone environment. Leaving before completion may require repeating the configuration.

***

# Key Concepts

| Term | Meaning |
|---|---|
| Playbook | A workflow that performs automated response, enrichment, notification, or remediation actions. |
| Azure Logic Apps | The workflow service on which Sentinel playbooks are built. |
| SOAR | Security Orchestration, Automation, and Response. |
| Automation rule | Rule that runs specified actions when an incident or alert meets defined conditions. |
| Incident trigger | Starts a playbook when a Microsoft Sentinel incident is created or updated. |
| Content hub | Portal location for installing and managing Sentinel solutions, templates, connectors, and other content. |
| Connection | Authentication and authorization configuration allowing a Logic App to access Sentinel or another service. |
| Tactics | High-level attacker goals from the MITRE ATT&CK framework, such as Execution or Exfiltration. |

Playbooks can run manually or automatically in response to alerts or incidents, and Azure Logic Apps provides their workflow logic and integrations. [learn.microsoft](https://learn.microsoft.com/en-us/azure/sentinel/automation/create-playbooks)

***

# Task 1 — Create a Sentinel Playbook

## Goal

Deploy the ransomware-response playbook template from the **Sentinel SOAR Essentials** solution.

## Access Microsoft Sentinel

1. Sign in to `WIN1` as `Admin`.
2. Open Microsoft Edge.
3. Navigate to:

```text
https://security.microsoft.com
```

4. Sign in with the tenant credentials or Temporary Access Pass (TAP).

<span style="color:red">[Screenshot placeholder: Microsoft Defender XDR portal opened and signed in.]</span>

## Find the Playbook Template

1. Navigate to:

```text
Microsoft Sentinel
> Content management
> Content hub
```

2. Search for:

```text
Sentinel SOAR Essentials
```

3. Select the solution, then select:

```text
Manage
```

4. Find and select:

```text
Defender_XDR_Ransomware_Playbook_for_SecOps-Tasks
```

5. Select the template:

```text
Incident tasks - Microsoft Defender XDR Ransomware Playbook for SecOps
```

6. Select:

```text
Create playbook
```

<span style="color:red">[Screenshot placeholder: Sentinel SOAR Essentials solution and ransomware playbook template selected.]</span>

## Configure and Deploy

1. For **Resource Group**, select:

```text
SentinelStatic
```

2. Update the playbook name to:

```text
Defender_XDR_Ransomware_Playbook_SecOps_Tasks
```

> The template’s default name exceeds the 64-character limit; removing `for` and extra underscores makes it valid.

3. Select:

```text
Next: Connections
```

4. Review the default connection options.

5. Select:

```text
Next: Review and create
> Create playbook
```

6. Wait for deployment to complete.

7. Select:

```text
Close and go to playbook
```

This opens the playbook in the Azure Logic App designer.

<span style="color:red">[Screenshot placeholder: Playbook configuration with SentinelStatic resource group and corrected playbook name.]</span>

<span style="color:red">[Screenshot placeholder: Successful playbook deployment.]</span>

<span style="color:red">[Screenshot placeholder: Defender_XDR_Ransomware_Playbook_SecOps_Tasks opened in the Logic App designer.]</span>

***

# Task 2 — Configure the Playbook Connection

## Goal

Connect the playbook’s Microsoft Sentinel incident trigger to the student Azure account.

## Find the Active Playbook

If the Logic App designer is not already open:

1. In the Azure portal search bar, search for:

```text
Sentinel
```

2. Select:

```text
Microsoft Sentinel
```

3. Select the Sentinel workspace.

4. Navigate to:

```text
Configuration
> Automation
> Active Playbooks
```

5. Select **Refresh** if the playbook is not displayed.

6. Open:

```text
Defender_XDR_Ransomware_Playbook_SecOps_Tasks
```

## Update the Connection

1. In the Logic App designer, select:

```text
Edit
```

2. Select the first workflow block:

```text
Microsoft Sentinel incident
```

3. Select:

```text
Change connection
```

4. Scroll through the available connections and select:

```text
Add new
> Sign in
```

5. Sign in with the Azure subscription administrator account.

6. Confirm that the trigger block displays:

```text
Connected to your-Student-username
```

7. Select:

```text
Publish
```

> Changes normally auto-save, but **Publish** ensures the configured connection is applied.

8. Close the designer window.

<span style="color:red">[Screenshot placeholder: Microsoft Sentinel incident trigger selected in Logic App designer.]</span>

<span style="color:red">[Screenshot placeholder: New Sentinel connection authenticated with student account.]</span>

<span style="color:red">[Screenshot placeholder: Trigger block showing “Connected to [student username]”.]</span>

***

# Task 3 — Create an Automation Rule

## Goal

Create an automation rule that runs the ransomware playbook when a new incident has selected MITRE ATT&CK tactics.

## Create the Rule

1. In Microsoft Sentinel, navigate to:

```text
Configuration
> Automation
```

2. Select:

```text
+ Create
> Automation rule
```

3. Enter the name:

```text
Rule-66677890
```

4. Keep the trigger:

```text
When an incident is created
```

## Add Tactic Conditions

1. Select:

```text
+ Add
> Condition (And)
```

2. Configure:

| Field | Value |
|---|---|
| Property | `Tactics` |
| Operator | `Contains` |
| Values | `Reconnaissance`, `Execution`, `Persistence`, `Command and Control`, `Exfiltration`, `PreAttack` |

The rule will apply when a new incident contains the listed attack tactics.

## Add the Playbook Action

1. Under **Actions**, select:

```text
Run playbook
```

2. Select the ransomware playbook from the list:

```text
Defender_XDR_Ransomware_Playbook-66677890
```

> If the actual deployed playbook name differs slightly, choose the playbook created in Task 1.

3. Select:

```text
Apply
```

The rule-creation window should close automatically.

<span style="color:red">[Screenshot placeholder: Automation rule configured with “When an incident is created” trigger.]</span>

<span style="color:red">[Screenshot placeholder: Tactics condition showing the selected tactic values.]</span>

<span style="color:red">[Screenshot placeholder: Run playbook action with ransomware playbook selected.]</span>

<span style="color:red">[Screenshot placeholder: Completed automation rule displayed in Sentinel Automation.]</span>

***

# Response Flow

```text
New Sentinel incident created
        |
        v
Automation rule checks incident tactics
        |
        +-- Tactic matches configured values?
                |
                +-- No: No playbook action
                |
                +-- Yes
                        |
                        v
              Ransomware-response playbook runs
                        |
                        v
         Logic App performs configured response tasks
```

Automation rules can run when an incident is created or updated, or when an alert is created; available actions include running a playbook. [learn.microsoft](https://learn.microsoft.com/en-us/azure/sentinel/create-manage-use-automation-rules)

***

# Key Takeaways

| Topic | Key point |
|---|---|
| Playbook | A Logic Apps-based workflow for automated security response. |
| Automation rule | Decides when Sentinel applies actions or triggers a playbook. |
| Trigger | This lab uses `When an incident is created`. |
| Condition | Limits automation to incidents containing specified tactics. |
| Action | Runs the selected ransomware-response playbook. |
| Connection | Grants the playbook access to work with Sentinel incident data. |
| Content hub | Provides ready-made Sentinel SOAR templates and solutions. |
| Publish | Ensures Logic App connection changes are deployed. |
| Permissions | Sentinel needs appropriate permissions to run a playbook.  [learn.microsoft](https://learn.microsoft.com/en-us/azure/sentinel/create-manage-use-automation-rules) |
