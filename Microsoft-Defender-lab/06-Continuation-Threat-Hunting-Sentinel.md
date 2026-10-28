# Microsoft Sentinel Threat Hunting  
## Learning Path 09 — Lab 1, Exercise 1
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/d184666c-bb6b-4fcb-b18b-b9d5201dc60a" />

> **Scenario:** As a Security Operations Analyst, investigate suspected Command and Control (C2) activity and build repeatable hunting capability in Microsoft Sentinel through the Microsoft Defender portal. The lab uses a standalone shared environment; leaving it can require repeating the setup.

## Lab Objectives

- Hunt for suspicious PowerShell activity connected to a simulated DNS-based C2 technique.
- Convert a notable hunting result into a high-severity incident.
- Explore relationships between users and sensitive resources with the Hunting graph.
- Create a one-time Data Lake KQL job to aggregate potential C2 activity.
- Use the MITRE ATT&CK map to create a multi-query hunt around a tactic or technique.

> **Key idea:** Threat hunting begins with a hypothesis, queries relevant telemetry, evaluates results, and either validates or invalidates the hypothesis. The lab’s generated PowerShell activity gives a known pattern to investigate.

***

# Environment and Prerequisites

The following components are preconfigured, but understanding them helps explain why hunting data is available.

| Component | Purpose |
|---|---|
| `WINServer` | Azure Arc-connected non-Azure Windows server used to produce simulated activity. |
| Azure Arc Connected Machine agent | Connects `WINServer` to Azure management services. |
| Azure Monitor Agent (AMA) | Collects Windows security events from the server. |
| `sentinelworkspace-01` | Predeployed Microsoft Sentinel workspace. |
| Windows Security Events via AMA | Connector that collects Windows Security event data. |
| `WIN1` | Management virtual machine used to access the Defender portal. |
| `c2.ps1` | PowerShell script that generates repeated DNS TXT queries with randomized subdomains. |

> The C2 simulation can produce expected DNS resolution failures. Keep the PowerShell process running so telemetry continues to accumulate for hunting.

<img width="1905" height="917" alt="image" src="https://github.com/user-attachments/assets/e720138c-5b81-480b-a780-37ebfcffd402" />

***

# Task 1 — Create a Hunting Query and Incident

## Objective

Use Advanced Hunting to find PowerShell processes, identify the DNS C2 simulation, and create an incident from the query result.

## Open Advanced Hunting

1. Sign in to `WIN1` using the supplied lab credentials.
2. Open:

```text
https://security.microsoft.com
```

3. Navigate to:

```text
Investigation & Response
> Hunting
> Advanced hunting
```

4. Open the **New query** tab.

> Paste KQL into Notepad first, then copy it into the query editor to avoid formatting errors.

<img width="1905" height="893" alt="image" src="https://github.com/user-attachments/assets/69a98a81-f6f6-4dee-b26b-3ab458543a6e" />

## Run the PowerShell Hunt

```kql
let lookback = 2d; 
SecurityEvent
| where TimeGenerated >= ago(lookback) 
| where EventID == 4688 and Process =~ "powershell.exe"
| extend PwshParam = trim(@"[^/\\]*powershell(.exe)+" , CommandLine) 
| project TimeGenerated, Computer, SubjectUserName, PwshParam
```
FIXED QUERY:
let lookback = 2d;
SecurityEvent
| where TimeGenerated >= ago(lookback)
| where EventID == 4688
| where Process =~ "powershell.exe"
| extend PwshParam = extract(
    @"(?i)^\s*""?(?:[^""]*[\\/])?powershell\.exe""?(?:\s+|$)(.*)$",
    1,
    CommandLine
)
| project TimeGenerated, Computer, SubjectUserName, PwshParam

### Query Breakdown

| Query element | Purpose |
|---|---|
| `let lookback = 2d` | Defines a two-day investigation window. |
| `SecurityEvent` | Searches Windows Security event data. |
| `TimeGenerated >= ago(lookback)` | Limits results to the defined time window. |
| `EventID == 4688` | Finds process-creation events. |
| `Process =~ "powershell.exe"` | Selects PowerShell processes; `=~` performs case-insensitive matching. |
| `trim(...)` | Removes the executable path/name from the command line, leaving useful arguments. |
| `project` | Displays selected investigation fields only. |

5. Select **Run query**.
6. Review the results for PowerShell execution.
7. Select the result whose `PwshParam` field contains:

```text
-file c2.ps1
```
## Create an Incident

1. Select the matching result.
2. In the results command bar, select:

```text
…
> Link to incident
```

3. Keep **Create new incident** selected.
4. Create a unique identifier, for example:

```text
TKS1234
```

5. Complete the incident fields:

| Setting | Value |
|---|---|
| Alert title | `PowerShell C2 Hunt - TKS1234` |
| Severity | High |
| Category | Command and Control |
| MITRE technique | `T1094: Custom Command and Control Protocol` |
| Description | `PowerShell C2 Hunt results - TKS1234` |
| Recommended actions | `Perform incident remediation` |

6. Select **Next**.
7. In **Entity mapping**, select:

```text
Impacted Assets
> Add assets
```

8. Configure the mapping:

| Field | Value |
|---|---|
| Entity | Device |
| Identifier | Hostname |
| Column | `Computer` |

9. Select **Next**.
10. Review the summary, select **Submit**, then select **Done**.

## Verify the Incident

1. Navigate to:

```text
Investigation & Response
> Incidents & Alerts
> Incidents
```

2. Set **Last update time** to:

```text
1 Day
```

3. Search for:

```text
PowerShell C2 Hunt - TK1234
```

4. Confirm that the incident is listed.

<img width="1902" height="836" alt="image" src="https://github.com/user-attachments/assets/62141fc1-8061-4f09-a4c9-943b621fdf1a" />

***

# Task 2 — Hunt with the Microsoft Sentinel Graph

## Objective

Use the Hunting graph to visualize relationships between identities and a sensitive storage account.

## Open the Scenario

1. Return to:

```text
Investigation & Response
> Hunting
> Advanced hunting
```

2. Select the **+** next to **New query**.
3. Choose:

```text
Hunting graph
> Search with Predefined scenarios
```

4. Select the scenario:

```text
Users with access to Sensitive data
```

5. For **Target storage account**, enter:

```text
sensitivestorageaccount
```

6. Leave other filters at their default values.
7. Select **Run**.

## Explore the Graph

The graph displays entities and relationships relevant to access to the named storage account.

1. Select the **Defender for Cloud** icon above `sensitivestorageaccount`.
2. In the details pane, verify that **Discovery source** is:

```text
Defender for Cloud
```

> If the Defender for Cloud icon is absent, enable its discovery source in **Layers**.

3. Review the available tabs, especially **All data**, for details about the storage account.
4. Find the user account connected to the storage account with a **gold crown** icon.
5. Select the crown icon to open the critical user’s details.
6. Select the **+** icon on nodes to expand the graph and explore additional relationships.

> The **Attack paths** tab is populated only when multiple attacks are associated with the entity.

***

# Task 3 — Create a Data Lake KQL Job

## Objective

Create a KQL job that searches for PowerShell C2 activity, aggregates results, and writes the output to a custom Analytics-tier table.

> **Important:** KQL jobs may incur consumption billing. The job may take up to 10 minutes to finish.

## Create the Job

1. In Microsoft Sentinel, navigate to:

```text
Data lake exploration
> Jobs
```

2. Select:

```text
Create a new KQL job
```

3. Enter a descriptive job name, for example:

```text
C2 Attack Hunt - TK
```

4. Under **Destination table in Analytics tier**, select:

```text
SentinelWorkspace-01
```

5. Keep **Create a new table** selected.
6. Enter a unique table name:

```text
C2ATTACKHUNT_TK
```

> The system appends `_KQL_CL` to the custom table name.

7. Select **Next**.

## Add the Query

```kql
let lookback = 2d; 
SecurityEvent 
| where TimeGenerated >= ago(lookback) 
| where EventID == 4688 and Process =~ "powershell.exe"
| extend PwshParam = trim(@"[^/\\]*powershell(.exe)+" , CommandLine) 
| project TimeGenerated, Computer, SubjectUserName, PwshParam 
| summarize min(TimeGenerated), count() by Computer, SubjectUserName, PwshParam
```

### Difference From the First Hunt

The final `summarize` operation groups results by host, user, and PowerShell argument, then returns:

- The earliest observed execution time.
- A count of matching process-creation events.

This reduces individual events into a compact view of recurring execution patterns.

8. Select **Next**.
9. Choose **One time** as the job frequency.
10. Select **Next**.
11. Review the settings and select **Submit**.
12. Select **Done**.

## Monitor and Review Results

1. On the Jobs page, confirm the job initially shows:

```text
Last run status: In progress
```

2. Use the refresh icon periodically.
3. When the status becomes:

```text
Succeeded
```

select the job to view its details and history.

4. Select the destination table link:

```text
C2ATTACKHUNT_TK_KQL_CL
```

5. Advanced Hunting opens with the destination table inserted into a new query.
6. Select **Run query** to review the aggregated results.

> A red wavy underline under the table name means Advanced Hunting has not yet recognized the new table. Wait several minutes and try again.

<img width="1903" height="947" alt="image" src="https://github.com/user-attachments/assets/5ed63cb2-60ae-4930-9854-e8ec2e8b8b99" />
<img width="1887" height="856" alt="image" src="https://github.com/user-attachments/assets/cc7651e0-45c3-454f-a083-2af2fec3fdab" />
<img width="1907" height="823" alt="image" src="https://github.com/user-attachments/assets/151aab5f-5141-42fc-8d44-f60a7e831f69" />

***

# Task 4 — Create a MITRE ATT&CK Hunt

## Objective

Use the MITRE ATT&CK map to locate existing hunting queries for **Account Manipulation**, clone them into a coordinated hunt, run them, and decide whether evidence supports the hypothesis.

## Find Technique Coverage

1. In Microsoft Sentinel, navigate to:

```text
Threat management
> MITRE ATT&CK
```

2. Clear selections in the **Active rules** filter.
3. In **Simulated rules**, select:

```text
Hunting queries
```

4. Select the **Account Manipulation** technique card.
5. In the details pane, under **Simulated coverage**, select **View** beside **Hunting queries**.

This opens a filtered Hunting Queries view for the selected technique.

## Create and Run the Hunt

1. Select all listed queries using the checkbox at the top of the query list.
2. Open:

```text
Hunt actions
> Create hunt
```

3. Enter a hunt name.
4. Add an optional description that states the investigation hypothesis.
5. Set the working status using the **Hypothesis** dropdown.
6. Select **Create**.

Example hypothesis:

```text
An attacker may be creating, modifying, or elevating accounts to maintain access or increase privileges.
```

7. Open:

```text
Hunts (Preview)
```

8. Select the newly created hunt.
9. Review its name, description, content, creation time, and last update time.
10. Select all queries, then choose:

```text
Run selected queries
```

Alternatively, run an individual query from its details pane using **View results**.

11. Review which queries return results.
12. Decide whether the evidence is sufficient to validate the hypothesis.

If evidence is insufficient:

```text
Close the hunt
> Mark the hypothesis as invalidated
```

## Alternative Filtering Path

The same hunting focus can be reached by filtering directly:

```text
Microsoft Sentinel
> Threat management
> Hunting
> Add filter
```

Apply:

| Filter | Value |
|---|---|
| Tactics | Persistence |
| Techniques | `T1098` |

***

# Key Concepts and Takeaways

| Concept | Notes |
|---|---|
| Threat hunting | Proactive search for suspicious behavior that may not have triggered an alert. |
| Hunting hypothesis | A testable statement about potential attacker activity, such as suspected PowerShell-based C2. |
| Event ID 4688 | Windows Security event associated with process creation. |
| PowerShell C2 indicator | A PowerShell process executing `c2.ps1` is the known simulated indicator in this lab. |
| Entity mapping | Connects query fields, such as `Computer`, to investigation entities such as devices. |
| Hunting graph | Visualizes relationships among users, devices, cloud resources, and security findings. |
| Data Lake KQL job | Runs a KQL query against data lake data and writes its output to an Analytics-tier custom table. |
| MITRE ATT&CK map | Helps identify technique coverage gaps and find relevant built-in hunting queries. |
| Validated hypothesis | Evidence supports the suspected behavior and may justify escalation or remediation. |
| Invalidated hypothesis | Evidence does not support the suspected behavior; document and close the hunt. |

## Knowledge Check

How does changing a raw PowerShell event query into an aggregated KQL job make a SOC analyst’s investigation more efficient?
