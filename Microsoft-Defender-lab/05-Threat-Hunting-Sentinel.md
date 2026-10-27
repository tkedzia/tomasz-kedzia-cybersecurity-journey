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

# Learning Path 8 — Lab 1, Exercise 3  
## Explore Entity Behavior Analytics

## Purpose

Explore **User and Entity Behavior Analytics (UEBA)** and anomaly-detection rules in Microsoft Sentinel.

UEBA helps security analysts identify unusual behavior by comparing activity against expected patterns for users, hosts, IP addresses, and other entities. Anomaly rules highlight behavior that may require investigation, even when it does not match a known attack signature.

> **Lab note:** This is a standalone environment. Leaving before completion may require you to repeat the configuration.

***

# Key Concepts

| Term | Meaning |
|---|---|
| UEBA | User and Entity Behavior Analytics; identifies unusual behavior involving users and entities. |
| Entity | An item involved in security activity, such as a user account, host, IP address, file, or process. |
| Anomaly | Activity that differs significantly from established or expected behavior. |
| Anomaly rule | A Sentinel analytics rule that detects unusual behavior using analytics models. |
| Production mode | Standard, active anomaly rule mode used for operational detections. |
| Flighting mode | Testing mode used to evaluate and tune a copy of an anomaly rule. |
| Anomaly score threshold | Sensitivity level that determines how unusual behavior must be before it is reported. |
| Connected data source | A log source supplying the data required for UEBA analysis. |

***

# Task 1 — Explore UEBA

## Goal

Verify that UEBA is enabled, review its connected data sources, and confirm that anomaly detection is enabled for the Sentinel workspace.

## Access Microsoft Sentinel Settings

1. Sign in to `WIN1` as `Admin`.
2. Open Microsoft Edge.
3. Navigate to:

```text
https://security.microsoft.com
```

4. Sign in using the lab tenant credentials or Temporary Access Pass (TAP).

5. In the Microsoft Defender navigation menu, expand:

```text
System
```

6. Select:

```text
Settings
> Microsoft Sentinel
```

## Review UEBA Status

1. In the Microsoft Sentinel settings area, select:

```text
UEBA
```

2. On the **UEBA** tab, confirm that UEBA is enabled.

3. Review the connected data sources that provide telemetry for behavior analytics.

> UEBA depends on ingested data. More relevant data sources generally provide richer entity context and stronger behavior analysis.

## Confirm Anomaly Detection

1. At the top of the Microsoft Sentinel settings menu, select:

```text
SIEM workspaces
> sentinelworkspace-01
```

2. Scroll down and expand:

```text
Anomalies
```

3. Read the anomaly-information section.

4. Confirm that:

```text
Detect Anomalies: On
```

5. Select:

```text
Configure anomalies in analytics
```

This opens the **Analytics > Anomalies** tab.

***

# Task 2 — Review Anomaly Rules

## Goal

Confirm anomaly rules are enabled and compare **Production** and **Flighting** rule behavior.

## Confirm Rule Status

1. On the **Analytics** page, open:

```text
Anomalies
```

2. Review the **Status** column.

3. Confirm that anomaly rules show:

```text
Enabled
```

## Select an Eligible Production Rule

1. Find an enabled rule whose name indicates:

```text
Production
```

2. Use the search bar to confirm there is no corresponding rule with the same base name beginning with:

```text
FLGT
```

3. Select the eligible Production rule.

4. Select:

```text
Edit
```

> Only one Production rule and one Flighting copy of the same rule can exist at a time. In a shared lab environment, every Production rule may already have an existing `FLGT` copy. If so, complete this task as a review-only exercise.

## Review Production Mode

1. On the **General** tab, verify:

```text
Mode: Production
```

2. Select:

```text
Next: Configuration
```

3. Review the configuration.

4. Confirm that the **Anomaly score threshold** cannot be changed in Production mode.

5. Select:

```text
Cancel
```

to exit the rule wizard without modifying the Production rule.

***

# Duplicate the Rule for Flighting

## Goal

Create a Flighting copy of an eligible Production rule so its anomaly threshold can be tested.

1. Find the Production rule reviewed previously.

2. Scroll right, select the ellipsis menu:

```text
...
```

3. Select:

```text
Duplicate
```

4. Scroll left and locate the newly created rule.

5. Confirm that its name begins with:

```text
FLGT
```
<img width="1887" height="860" alt="image" src="https://github.com/user-attachments/assets/90d1e1d9-3a8e-4dc5-9e02-abc599f17bc0" />

***

# Configure Flighting Mode

## Goal

Review the editable anomaly-score threshold in a Flighting rule and set it to `1`.

1. Select the new:

```text
FLGT...
```

rule.

2. Select:

```text
Edit
```

3. On the **General** tab, confirm:

```text
Mode: Flighting
```

4. Select:

```text
Next: Configuration
```

5. Confirm that the **Anomaly score threshold** can be edited.

6. Set the threshold to:

```text
1
```

7. Select:

```text
Next: Submit Feedback
> Next: Review and Create
> Save
```

A lower threshold increases sensitivity, meaning more unusual activities may be reported. This can improve visibility during testing but may also create more alerts for analysts to review.

***

# Clean Up the Test Rule

## Goal

Delete the temporary Flighting rule created for testing.

1. Locate the `FLGT` rule created in this exercise.

2. Select its ellipsis menu:

```text
...
```

3. Select:

```text
Delete
```

4. Confirm the deletion.

> Skip this step if no eligible Production rule was available and the task was completed as a review-only walkthrough.

***

# Production vs Flighting

| Feature | Production Rule | Flighting Rule |
|---|---|---|
| Purpose | Operational anomaly detection | Testing and tuning anomaly detection |
| Mode | `Production` | `Flighting` |
| Anomaly threshold | Locked / not editable | Editable |
| Recommended use | Regular SOC monitoring | Testing detection sensitivity before operational use |
| Relationship | Primary operational version | Test copy associated with a Production rule |

```text
Production anomaly rule
        |
        | Duplicate
        v
FLGT anomaly rule
        |
        | Adjust anomaly score threshold
        v
Evaluate detection sensitivity
        |
        +-- Keep and promote if appropriate
        |
        +-- Delete after testing
```

***

# Key Takeaways

| Topic | Key Point |
|---|---|
| UEBA | Identifies unusual behavior involving users and entities. |
| Data sources | UEBA requires connected log sources to build useful behavior context. |
| Detect Anomalies | Must be enabled to use anomaly analytics. |
| Production rules | Used for active, operational anomaly detection; thresholds are protected from direct editing. |
| Flighting rules | Test copies of Production rules that allow threshold tuning. |
| Threshold of 1 | Increases anomaly-detection sensitivity and can generate more findings. |
| Shared environment | Another learner may have already created a Flighting copy, requiring review-only completion. |

# Learning Path 8 — Lab 1, Exercise 4  
## Prepare to Perform Simulated Attacks

## Purpose

Prepare a non-Azure Windows server for simulated attack activity and connect its Windows Security event logs to Microsoft Sentinel.

This exercise also explains the attacks that will be run later and introduces the detection-development cycle:

```text
Generate activity
> Collect logs
> Write KQL query
> Create analytics rule
> Generate alert and incident
> Investigate and tune detection
```

> **Lab note:** This is a shared, standalone environment. Coordinate with other learners before changing shared resources. Leaving the lab may require repeating the configuration.

***

# Key Concepts

| Term | Meaning |
|---|---|
| Azure Arc | Azure service that manages servers outside Azure, including on-premises servers and servers in other clouds. |
| Azure Arc Connected Machine agent | Agent installed on a non-Azure system to connect it to Azure Arc. |
| `azcmagent` | Command-line tool used to connect, disconnect, and verify Azure Arc status. |
| Data Collection Rule (DCR) | Configuration that defines which machines send logs and which logs are collected. |
| Azure Monitor Agent (AMA) | Agent that collects monitoring and security data from a device. |
| Persistence | Attacker technique that keeps malicious code running after reboot or user logon. |
| Privilege escalation | Attacker technique used to gain higher permissions, such as local administrator rights. |
| Command and Control (C2) | Communication channel used by an attacker to control compromised systems. |
| DNS tunneling / DNS C2 | Misuse of DNS queries to communicate with attacker-controlled infrastructure or transfer data. |
| KQL | Query language used to hunt through and analyze Microsoft Sentinel log data. |
| Analytics rule | Detection logic that turns matching log activity into alerts and incidents. |

***

# Task 1 — Connect WINServer to Azure Arc

## Goal

Confirm that `WINServer` is connected to Azure Arc. If it is not, remove any stale Azure resource and reconnect the server.

## Check Connection Status

1. Sign in to:

```text
WINServer
```

2. Use the provided `Administrator` credentials.

3. Open an elevated Command Prompt:

```text
Search
> cmd
> Right-click Command Prompt
> Run as administrator
```

4. Run:

```cmd
azcmagent show
```

5. Check the output.

Expected result:

```text
Agent status: Connected
```

> If the status is **Connected**, do not make changes. Continue to Task 2.

<img width="1908" height="1002" alt="image" src="https://github.com/user-attachments/assets/3513b406-3e12-4230-a10e-554564e440e1" />

## Reconnect WINServer If Needed

Perform these steps only if the Azure Arc agent is **not connected**.

1. Open the Azure portal:

```text
https://portal.azure.com
```

2. Open the resource group:

```text
SentinelStatic
```

3. Check whether `WINServer` is listed as a resource.

4. If it is listed, select it and delete it.

5. Return to the elevated Command Prompt on `WINServer`.

6. Force the local agent to disconnect:

```cmd
azcmagent disconnect --force-local-only
```

7. Connect the machine to Azure Arc:

```cmd
azcmagent connect -g "cloud-shell" -l "CentralUS" -s "35136189-e83d-4247-89ae-e04b3cc124b0"
```

> Replace the subscription ID with the one supplied in the lab **Resources** tab. Keep the quotation marks.

8. If prompted to choose a browser, select:

```text
Microsoft Edge
```

9. Sign in using the lab tenant account, password, or TAP.

10. Wait for the authentication-complete message, close the browser tab, and return to Command Prompt.

11. Verify the new connection:

```cmd
azcmagent show
```

12. Confirm:

```text
Agent status: Connected
```
***

# Task 2 — Connect WINServer to Microsoft Sentinel

## Goal

Create a Data Collection Rule that collects all Windows Security events from the Azure Arc-connected `WINServer` machine.

## Open the Connector

1. Sign in to `WIN1` as `Admin`.

2. Open Microsoft Defender XDR:

```text
https://security.microsoft.com
```

3. Sign in with the provided tenant credentials or TAP.

4. Navigate to:

```text
Microsoft Sentinel
> Configuration
> Data connectors
```

5. Search for:

```text
Windows Security Events via AMA
```

6. Select the connector and choose:

```text
Open connector page
```

The Windows Security Events solution includes:

| Content | Quantity |
|---|---:|
| Windows Security Events via AMA connector | 1 |
| Security Events via Legacy Agent connector | 1 |
| Workbooks | 2 |
| Analytics rules | 20 |
| Hunting queries | 43 |

## Create the Data Collection Rule

1. Under **Configuration**, select:

```text
Create data collection rule
```

2. Enter a unique rule name:

```text
WIN[Student-ID]DCR
```

Example:

```text
WIN66677890DCR
```

3. Select:

```text
Next: Resources
```

4. Expand the subscription under **Scope**.

5. Expand:

```text
cloud-shell
```

6. Select:

```text
WINServer
```

7. Select:

```text
Next: Collect
```

8. Keep the default setting:

```text
All Security Events
```

9. Select:

```text
Next: Review + create
```

10. Wait for validation to pass.

11. Select:

```text
Create
```

The DCR deploys the Azure Monitor Agent configuration needed to collect Windows Security events from the Azure Arc-enabled server.

<img width="1882" height="824" alt="image" src="https://github.com/user-attachments/assets/08ea4c3c-4e3c-41b3-8c52-52017ece3e30" />
<img width="1883" height="853" alt="image" src="https://github.com/user-attachments/assets/3895aadf-a8f5-4883-935d-4e354d45f1dd" />

***

# Task 3 — Understand the Simulated Attacks

## Important

Do **not** run these commands in this exercise. They are reviewed now and will be executed in the next exercise.

The simulated techniques are based on the open-source **Atomic Red Team** project, which provides small, focused tests mapped to known adversary behaviors.

***

## Attack 1 — Registry Run Key Persistence

### Objective

Create a Registry Run key that causes a program to launch every time the current user signs in.

### Command

```cmd
REG ADD "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /V "SOC Test" /t REG_SZ /F /D "C:\temp\startup.bat"
```

### What It Does

| Component | Meaning |
|---|---|
| `REG ADD` | Adds or modifies a Windows Registry value. |
| `HKCU` | Current user’s Registry hive. |
| `...\CurrentVersion\Run` | Registry location used to start programs at user logon. |
| `SOC Test` | Name of the created Registry value. |
| `REG_SZ` | String data type. |
| `C:\temp\startup.bat` | Program or script configured to run at logon. |

This simulates a persistence technique: an attacker adds a program to a Run key so it launches after the user logs in.

***

## Attack 2 — Create and Elevate a User Account

### Objective

Create a new local user account, set a password, and add it to the local Administrators group.

### Commands

```cmd
net user theusernametoadd /add
```

```cmd
net user theusernametoadd ThePassword1!
```

```cmd
net localgroup administrators theusernametoadd /add
```

### What It Does

| Command | Effect |
|---|---|
| `net user ... /add` | Creates a local user account. |
| `net user ... password` | Sets the user’s password. |
| `net localgroup administrators ... /add` | Adds the user to the local Administrators group. |

This simulates account creation and privilege escalation, allowing an attacker to retain privileged access using a newly created account.

***

## Attack 3 — DNS-Based Command and Control Simulation

### Objective

Generate a large number of DNS TXT queries to simulate possible C2 communication or DNS tunneling behavior.

### Script Behavior

The PowerShell script:

- Uses a target domain, by default:

```text
microsoft.com
```

- Generates random subdomains.
- Sends DNS queries of type:

```text
TXT
```

- Uses configurable timing and jitter.
- Runs for a defined duration.
- Creates a repeated DNS-query pattern designed to trigger threshold-based detections.

### Key Parameters

| Parameter | Default value | Meaning |
|---|---:|---|
| `Domain` | `microsoft.com` | Target domain for generated queries. |
| `QueryType` | `TXT` | DNS record type requested. |
| `C2Interval` | `8` seconds | Base delay between queries. |
| `C2Jitter` | `20` | Random variation added to the timing. |
| `RunTime` | `240` minutes | Maximum execution time. |

> The script is a controlled simulation for detection testing. Do not run it outside the authorized lab environment.

***

# Task 4 — Understand Detection Modeling

## Goal

Understand the iterative process used to turn simulated activity into effective Sentinel detections.

```text
1. Generate known activity on a host
        |
        v
2. Confirm that logs are collected
        |
        v
3. Explore events with KQL
        |
        v
4. Build an analytics rule
        |
        v
5. Trigger alerts and incidents
        |
        v
6. Investigate the incident
        |
        v
7. Tune rule logic, thresholds, entities, and investigation fields
```

## Detection-Modeling Process

| Phase | Analyst activity | Purpose |
|---|---|---|
| Generate | Perform a controlled simulated attack. | Produce known activity to detect. |
| Collect | Confirm the endpoint logs are reaching Sentinel. | Ensure reliable data is available. |
| Hunt | Build and test a KQL query. | Identify useful evidence and detection logic. |
| Detect | Convert the KQL query into an analytics rule. | Generate alerts when activity reoccurs. |
| Investigate | Review alerts, incidents, entities, and evidence. | Decide whether the activity is malicious or benign. |
| Tune | Adjust query logic, thresholds, scheduling, grouping, and entity mapping. | Improve detection quality and reduce false positives. |

> Lab alerts may trigger faster than normal operational detections so you can observe the full attack-to-investigation cycle in a short lab period.

***

# Key Takeaways

| Topic | Key Point |
|---|---|
| Azure Arc | Makes the on-premises-style `WINServer` manageable through Azure. |
| `azcmagent show` | Verifies whether the Azure Arc agent is connected. |
| DCR | Defines the log source and the event types collected from `WINServer`. |
| AMA | Collects Windows Security event data for Microsoft Sentinel. |
| Registry Run key | Common persistence location used to launch programs at user logon. |
| New local admin account | Represents account creation and privilege escalation behavior. |
| DNS C2 simulation | Generates unusual DNS activity that can be detected using volume, timing, or domain patterns. |
| Detection modeling | Iterative process of generating data, writing KQL, creating detections, investigating, and tuning. |

# Learning Path 8 — Lab 1, Exercise 5  
## Conduct Simulated Attacks

## Purpose

Run controlled attack simulations on `WINServer` so Microsoft Sentinel can later detect, alert on, and support investigation of the generated activity.

The exercises simulate three common attacker behaviors:

1. **Persistence** through a Windows Registry Run key.
2. **Privilege escalation** through a new local administrator account.
3. **Command and control (C2)** behavior through repeated DNS TXT queries.

> **Lab note:** Perform these actions only in the authorized lab environment. The DNS simulation must remain running because later exercises use its generated logs.

***

# Environment

| Item | Value |
|---|---|
| Target system | `WINServer` |
| Required state | Connected to Azure Arc and configured with Azure Monitor Agent / Windows event collection |
| Account | `Admin` / Administrator, using lab-provided credentials |
| Purpose of generated events | Create logs for Sentinel detections, alerts, investigations, and later threat hunting |

***

# Task 1 — Persistence Attack Using Registry Run Key

## Goal

Create a Registry Run key that causes a batch-file path to launch whenever the current user signs in.

Registry Run keys are commonly used by attackers to maintain persistence after a reboot or user logon.

## Restart WINServer

1. On `WINServer`, select:

```text
Start
> Power
> Restart
```

2. Wait for the computer to restart.
3. Sign in again using the lab credentials.

> The lab save functionality can disconnect `WINServer` from Azure Arc. Restarting the system restores the required connection state.

## Open an Elevated Command Prompt

1. In the taskbar search box, type:

```text
Command
```

2. Right-click **Command Prompt**.

3. Select:

```text
Run as administrator
```

4. Select **Yes** in the User Account Control prompt.

## Create the Working Folder

Run the following commands, pressing Enter after each line:

```cmd
cd \
mkdir temp
cd temp
```

This creates and opens:

```text
C:\temp
```

## Add the Registry Run Key

Run:

```cmd
REG ADD "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /V "SOC Test" /t REG_SZ /F /D "C:\temp\startup.bat"
```

## What the Command Does

| Element | Meaning |
|---|---|
| `REG ADD` | Adds or modifies a Windows Registry value. |
| `HKCU` | Current user Registry hive. |
| `...\CurrentVersion\Run` | Location that starts configured programs at user logon. |
| `SOC Test` | Name assigned to the Registry value. |
| `REG_SZ` | String value type. |
| `C:\temp\startup.bat` | Path configured to run when the user signs in. |
| `/F` | Forces overwrite without an additional confirmation prompt. |

***

# Task 2 — Privilege Elevation Through User Creation

## Goal

Create a new local account, configure its password, and add it to the local Administrators group.

This simulates an attacker establishing an alternative privileged account.

## Run the Commands

In the same elevated Command Prompt, run:

```cmd
net user theusernametoadd /add
```

```cmd
net user theusernametoadd ThePassword1!
```

```cmd
net localgroup administrators theusernametoadd /add
```

> Replace `theusernametoadd` only if the lab instructions or shared-environment guidance requires a unique account name.

## What the Commands Do

| Command | Effect |
|---|---|
| `net user ... /add` | Creates a new local user account. |
| `net user ... ThePassword1!` | Sets the password for the new account. |
| `net localgroup administrators ... /add` | Adds the account to the local Administrators group. |

This activity should generate Windows security evidence associated with local account management and group-membership changes.

<span style="color:red">[Screenshot placeholder: Local user account created successfully.]</span>

<span style="color:red">[Screenshot placeholder: New account added to the local Administrators group.]</span>

***

# Task 3 — Command and Control Simulation with DNS

## Goal

Create and run a PowerShell script that generates repeated DNS TXT requests with randomized subdomains.

This simulates a possible DNS-based command-and-control pattern. The script is expected to generate some DNS resolution errors; those errors are normal in this lab.

## Create the PowerShell Script

In Command Prompt, run:

```cmd
notepad c2.ps1
```

1. When prompted, select **Yes** to create a new file.
2. Paste the lab-provided PowerShell DNS simulation script into `c2.ps1`.
3. Verify the pasted script is complete.
4. In Notepad, select:

```text
File
> Save
```

5. Keep the file as:

```text
C:\temp\c2.ps1
```

<img width="1908" height="969" alt="image" src="https://github.com/user-attachments/assets/44a374b6-61ca-4a91-ac7c-ca0d7235beda" />

## Start the DNS Simulation

Return to the elevated Command Prompt and run:

```cmd
Start PowerShell.exe -file c2.ps1
```

The command starts a separate PowerShell process in the background.

Expected behavior:

- The script sends repeated DNS TXT queries.
- Randomized subdomains make queries appear distinct.
- Query timing includes jitter so the requests do not occur at exactly fixed intervals.
- DNS resolution errors may appear and are expected.
- The script runs for the configured duration, allowing logs to accumulate.

> **Do not close the Command Prompt or PowerShell windows.** Leave the script running while you continue to later exercises.

***

# DNS Simulation Behavior

| Setting | Default value | Purpose |
|---|---:|---|
| Domain | `microsoft.com` | Base domain used to construct DNS queries. |
| Query type | `TXT` | DNS record type requested. |
| Base interval | `8` seconds | Base delay between requests. |
| Jitter | `20` | Random timing variation around the base interval. |
| Runtime | `240` minutes | Maximum time for the simulation to run. |
| Random subdomains | Random numeric values | Creates many different query names for detection testing. |

```text
WINServer
    |
    | Registry Run key added
    v
Windows Security events
    |
    | Local user created and added to Administrators
    v
Windows Security events
    |
    | Repeated randomized DNS TXT queries
    v
DNS / network telemetry
    |
    v
Microsoft Sentinel detections and investigations
```

***

# Attack-to-Detection Mapping

| Simulated behavior | Intended security concept | Likely investigation focus |
|---|---|---|
| Registry Run key creation | Persistence | Registry modification, autorun location, initiating account and process |
| New local administrator account | Account creation and privilege escalation | New account name, account-management events, group-membership change |
| Repeated DNS TXT queries | Command and control / DNS anomaly | Query volume, unusual subdomains, query type, timing, source host |

***

# Key Takeaways

| Topic | Key Point |
|---|---|
| Registry persistence | Run keys can cause a program or script to launch at user sign-in. |
| Local administrator creation | Attackers may create and elevate accounts to retain privileged access. |
| DNS C2 simulation | Repeated DNS TXT queries can resemble command-and-control communication patterns. |
| Expected errors | DNS resolution errors from the simulation are expected. |
| Ongoing execution | Keep the DNS simulation running to generate logs for later labs. |
| Sentinel value | Generated security events provide known data for creating and validating detections. |
