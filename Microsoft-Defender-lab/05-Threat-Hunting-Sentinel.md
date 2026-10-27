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

<img width="1911" height="969" alt="image" src="https://github.com/user-attachments/assets/ee8cf3e2-ad90-4d61-a0b7-3c3f5658f821" />

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
<img width="1905" height="827" alt="image" src="https://github.com/user-attachments/assets/ee93b871-9fbd-4c25-9298-2814c85875da" />

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

<img width="1899" height="836" alt="image" src="https://github.com/user-attachments/assets/1b32c5da-253c-4d71-8ff1-b975959cdb34" />
        <img width="1894" height="873" alt="image" src="https://github.com/user-attachments/assets/7b95e8de-cc22-4c8c-8640-9e9f37523f3c" />


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

<img width="1889" height="871" alt="image" src="https://github.com/user-attachments/assets/b4082dd3-e0b3-4eb7-b2cb-75a7741c3c7b" />

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

<img width="1894" height="863" alt="image" src="https://github.com/user-attachments/assets/df19c9a6-9264-4615-820e-9d4a44e7c819" />

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

# Learning Path 8 — Lab 1, Exercise 2  
## Create a Scheduled Query Rule from a Template

## Purpose

Create and test a Microsoft Sentinel **scheduled analytics rule** based on the **New CloudShell User** template.

The rule searches Azure Activity logs for actions associated with a new Azure Cloud Shell setup. When matching events are found, Microsoft Sentinel creates an alert and incident for SOC investigation.

> **Lab note:** This is a standalone environment. Leaving before completion may require repeating the configuration.

***

# Key Concepts

| Term | Meaning |
|---|---|
| Analytics rule | A detection rule that searches log data for suspicious activity or defined conditions. |
| Scheduled query rule | An analytics rule that runs a KQL query on a recurring schedule. |
| Rule template | A prebuilt detection rule that can be customized and deployed. |
| Azure Activity | Log source recording Azure control-plane actions, such as resource creation and configuration changes. |
| Alert | A notification produced when a rule identifies matching data. |
| Incident | A case created for SOC analysts to investigate, often containing one or more alerts. |
| Automation rule | A rule that automatically performs actions on an incident, such as assigning an owner or running a playbook. |
| Cloud Shell | Browser-based Azure command-line environment using PowerShell or Bash. |
| KQL | Kusto Query Language, used to search and analyze Sentinel log data. |

***

# Task 1 — Create the Scheduled Query Rule

## Goal

Create a scheduled analytics rule from the **New CloudShell User** template and configure it to run every five minutes.

## Access Microsoft Sentinel

1. Sign in to `WIN1` as `Admin`.
2. Open Microsoft Edge.
3. Navigate to:

```text
https://security.microsoft.com
```

4. Sign in with the provided tenant credentials or Temporary Access Pass (TAP).

## Locate the Rule Template

1. Navigate to:

```text
Microsoft Sentinel
> Configuration
> Analytics
> Rule templates
```

2. Search for:

```text
New CloudShell User
```

3. Select the rule template to open its details pane.

4. Verify the **Azure Activity** data source displays a green icon.

> A green status icon confirms that the rule has access to the required Azure Activity log source. Without connected Azure Activity logs, the rule cannot generate an incident from Cloud Shell activity.

## Create and Configure the Rule

1. Select:

```text
Create rule
```

2. On the **General** tab, set:

| Setting | Value |
|---|---|
| Severity | `Medium` |

3. Select:

```text
Next: Set rule logic
```

4. Under the query section, select:

```text
View query results
```

5. Confirm that the query produces no errors. It is expected to return no results before Cloud Shell activity is generated.

6. Close the Logs window using the upper-right `X`.

7. Select:

```text
OK
```

to discard changes and return to the rule wizard.

## Configure Query Scheduling

Set the following values under **Query scheduling**:

| Setting | Value |
|---|---|
| Run query every | `5 minutes` |
| Lookup data from the last | `1 day` |

Keep the remaining default configuration:

| Section | Required setting |
|---|---|
| Alert threshold | Leave unchanged so every matching event can create an alert. |
| Event grouping | Keep `Group all events into a single alert`. |

> The five-minute schedule and one-day lookback are intentionally configured to generate repeated incidents from matching activity during the lab.

## Save the Rule

1. Select:

```text
Next: Incident settings
```

2. Review the default incident settings.

3. Select:

```text
Next: Automated response
```

4. Select:

```text
Next: Review and create
```

5. Review the rule configuration.

6. Select:

```text
Save
```

<img width="1896" height="872" alt="image" src="https://github.com/user-attachments/assets/8aae5da3-004e-4fd9-b2c5-ef51bd32dc8e" />

***

# Task 2 — Add an Automation Rule

## Goal

Edit the new detection rule and create an automation rule that assigns matching incidents to the student account.

## Open the Active Rule

1. Navigate to:

```text
Microsoft Sentinel
> Configuration
> Analytics
> Active rules
```

2. Find:

```text
New CloudShell User
```

3. Right-click the rule.

4. Select:

```text
Edit
```

## Navigate to Automated Response

1. Select:

```text
Next: Set rule logic
```

2. Select:

```text
Next: Incident settings
```

3. Select:

```text
Next: Automated response
```

## Create the Assignment Automation Rule

1. Under **Automation rules**, select:

```text
+ Add new
```

2. Set the automation-rule name:

```text
Tier 2-[Student ID]
```

Example:

```text
Tier 2-66677890
```

3. Under **Actions**, select:

```text
Assign owner
```

4. Choose the student account or assigned username.

5. Under **Order**, enter:

```text
1
```

6. Select:

```text
Apply
```

7. Select:

```text
Next: Review and create
> Save
```

<img width="1908" height="857" alt="image" src="https://github.com/user-attachments/assets/96718cb2-a344-4e90-91e5-0a17d4020772" />

***

# Task 3 — Test the Detection Rule

## Goal

Create Azure Cloud Shell activity that matches the analytics rule and, if log ingestion is configured for the subscription, generates a Sentinel incident.

## Open Azure Cloud Shell

1. Navigate to:

```text
https://portal.azure.com
```

2. Sign in with the lab tenant credentials or TAP.

3. In the Azure portal toolbar, select the **Cloud Shell** icon:

```text
>_
```

> If the icon is hidden due to screen resolution, select the ellipsis menu `...`.

4. In the welcome window, select:

```text
PowerShell
```

## Configure Cloud Shell Storage

1. On the getting-started page, select:

```text
Mount storage account
```

2. For the subscription, select:

```text
PROD-CSS-MOCTAP-lod53414808
```

3. Select:

```text
Apply
```

4. On the Mount storage account page, select:

```text
I want to create a storage account
> Next
```

5. Configure the storage account:

| Setting | Value |
|---|---|
| Resource group | `cloud-shell` |
| Region | `(US) East US` |
| Storage account name | A unique value, for example `cloudshellstor66677890` |
| File share | A unique value, for example `cloudshellshare` |

6. Wait for Cloud Shell provisioning to finish.

7. Close the Cloud Shell window.
<img width="1903" height="915" alt="image" src="https://github.com/user-attachments/assets/fcbb2aeb-a7e0-4c5d-aa0e-2652bb05865e" />        

## Validate the Azure Activity Events

1. In the Azure portal search bar, search for:

```text
Activity
```

2. Select:

```text
Activity Log
```

3. Select **Refresh** if necessary.

4. Look for these operation names:

```text
List Storage Account Keys
Update Storage Account Create
```

These operations are expected because Cloud Shell provisioning creates or accesses storage resources. The scheduled query searches for this Azure Activity behavior.

***

# Verify the Sentinel Incident

## Open Incidents

1. Return to:

```text
https://security.microsoft.com
```

2. Navigate to:

```text
Investigation & response
> Incidents & alerts
> Incidents
```

3. Set:

```text
Last update time: 1 Day
```

4. Search for:

```text
New CloudShell User
```
<img width="1916" height="862" alt="image" src="https://github.com/user-attachments/assets/3db763c2-462b-4714-bda1-34bed10490cd" />

5. If the incident appears, select it and review the right-side details pane.

Expected information includes:

- Incident title.
- Severity: `Medium`.
- Owner: Student account, assigned by the automation rule.
- Status.
- Related alerts and evidence.

> Incident generation can take more than five minutes because the activity must first reach the Sentinel workspace and then be processed by the scheduled query. If the Cloud Shell subscription is not connected to the workspace through the Azure Activity data connector, no incident will be created.

***

# Detection Flow

```text
Azure Cloud Shell is enabled
        |
        v
Azure creates or accesses Cloud Shell storage resources
        |
        v
Azure Activity Log records matching operations
        |
        v
Azure Activity connector sends events to Sentinel workspace
        |
        v
New CloudShell User scheduled rule runs every 5 minutes
        |
        v
Matching events create an alert and incident
        |
        v
Automation rule assigns the incident to the Tier 2 student account
```

***

# Key Takeaways

| Topic | Key Point |
|---|---|
| Analytics rule | Detects suspicious or defined activity by querying ingested log data. |
| Scheduled query | Runs repeatedly according to its configured schedule and lookback period. |
| Azure Activity dependency | The rule requires Azure Activity events from the Cloud Shell subscription to reach the Sentinel workspace. |
| Template | Provides a prebuilt KQL detection and baseline configuration. |
| Severity | This lab configures the rule as `Medium`. |
| Automation rule | Automatically assigns the created incident to the selected owner. |
| Cloud Shell test | Creating Cloud Shell storage generates Azure Activity operations targeted by the detection. |
| Investigation | Incidents provide the title, severity, owner, status, alerts, and evidence needed for SOC triage. |
