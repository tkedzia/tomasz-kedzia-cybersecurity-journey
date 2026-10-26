# Learning Path 7 — Lab 1, Exercise 1  
## Connect Data to Microsoft Sentinel Using Data Connectors
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/9a19d6f2-e485-431d-a5cb-26d19903dd1a" />

## Purpose

Explore how Microsoft Sentinel collects security logs from Microsoft services through **data connectors**.

The lab focused on:

- Accessing the predeployed Sentinel workspace in Defender XDR.
- Reviewing the Microsoft Defender for Cloud connector.
- Reviewing the Azure Activity connector.
- Checking connector status, log tables, retention settings, and UEBA configuration.

> **Lab note:** This is a standalone lab environment. Leaving before finishing may require you to repeat the configuration.

***

# Task 1 — Access Sentinel Workspace

## Environment

- **Device:** `WIN1`
- **User:** `Admin`
- **Portal:** `https://security.microsoft.com`
- **Sentinel workspace:** `sentinelworkspace-01`

Microsoft Sentinel and the required Content hub solutions were already deployed in the lab subscription.

## Steps

1. Sign in to `WIN1` as `Admin`.
2. Open Microsoft Edge.
3. Go to:

```text
https://security.microsoft.com
```

4. Sign in using the provided tenant email and password.
5. If requested, use the provided **Temporary Access Pass (TAP)**.

> Screenshot placeholder: *Signed in to Microsoft Defender XDR / Sentinel environment.*
> <img width="1902" height="948" alt="image" src="https://github.com/user-attachments/assets/bef5b9a4-5dfb-4141-bff4-9c63bac9bd15" />

***

# Task 2 — Microsoft Defender for Cloud Connector

## Purpose

Review the connector that brings Microsoft Defender for Cloud alerts into Microsoft Sentinel / Defender XDR.

## Navigation

```text
Microsoft Sentinel
> Content management
> Content hub
> Microsoft Defender for Cloud
> Manage
```

## Connector reviewed

```text
Tenant-based Microsoft Defender for Cloud (Preview)
```

## Steps

1. Search for:

```text
Microsoft Defender for Cloud
```

2. Open the solution and select:

```text
Manage
```

3. Select the **Tenant-based Microsoft Defender for Cloud** connector.

4. Select:

```text
Open connector page
```

5. Confirm the connector status is:

```text
Connected
```

6. Review the **Instructions** and **Configuration** sections.

## Key Findings

- The solution installs:
  - Subscription-based Microsoft Defender for Cloud (**Legacy**) connector.
  - Tenant-based Microsoft Defender for Cloud (**Preview**) connector.
  - An analytics rule.
- The tenant-based connector is useful when an organization has **multiple Azure subscriptions**.
- Defender for Cloud alerts stream through **Microsoft Defender XDR**.
- The connector cannot be disconnected while Defender XDR remains connected.
- Alert data is stored in the:

```text
SecurityAlert
```

table.

> Screenshot placeholder: *Tenant-based Microsoft Defender for Cloud connector showing Connected status.*
> <img width="1902" height="1052" alt="image" src="https://github.com/user-attachments/assets/703c4ca0-d22d-4515-8b3c-89f5e5e34da8" />

***

# Task 3 — Azure Activity Connector

## Purpose

Review the connector that collects Azure control-plane activity logs for Sentinel monitoring, detection, and hunting.

## Navigation

```text
Microsoft Sentinel
> Content management
> Content hub
> Azure Activity
> Manage
```

## Steps

1. Search for:

```text
Azure Activity
```

2. Open the solution and select:

```text
Manage
```

3. Select:

```text
Azure Activity Data connector
```

4. Select:

```text
Open connector page
```

5. Confirm the connector status is:

```text
Connected
```

6. In the **Setup** tab, under **Table management**, select:

```text
AzureActivity
```

7. Open:

```text
Data retention settings
```

8. Review **Manage AzureActivity settings** in the Analytics tier.

9. Close the retention-settings panel.

10. Open:

```text
Advanced options
```

11. Review **Configure UEBA settings** and confirm that UEBA is enabled for Azure Activity.

## Key Findings

The Azure Activity solution installs:

| Content type | Quantity |
|---|---:|
| Data connector | 1 |
| Analytics rules | 13 |
| Hunting queries | 14 |
| Workbook | 1 |

Azure Activity data is stored in:

```text
AzureActivity
```

The Azure Activity connector is fully integrated into Defender XDR.

> Screenshot placeholder: *Azure Activity connector showing Connected status.*
> <img width="1910" height="867" alt="image" src="https://github.com/user-attachments/assets/1c20ca36-1e40-40f2-bc50-8b6f79a9c9ca" />
> Screenshot placeholder: *AzureActivity table retention settings.*
> <img width="1891" height="845" alt="image" src="https://github.com/user-attachments/assets/181549dc-8cd4-4e80-82a5-6d7af89f8f79" />
> Screenshot placeholder: *UEBA enabled for Azure Activity.*
> <img width="1892" height="851" alt="image" src="https://github.com/user-attachments/assets/e2ddcbf7-14ee-4c99-a28a-ab63be3ca27d" />

***

# Concepts to Remember

| Concept | Meaning |
|---|---|
| Data connector | Connects a security-data source to Microsoft Sentinel. |
| Content hub | Location for installing and managing Sentinel solutions, connectors, analytics rules, workbooks, and hunting queries. |
| Microsoft Defender for Cloud connector | Sends Defender for Cloud security alerts into Sentinel / Defender XDR. |
| Tenant-based connector | Supports centralized connection across multiple subscriptions in one tenant. |
| `SecurityAlert` | Table used for security-alert data from Microsoft Defender for Cloud. |
| Azure Activity connector | Collects Azure control-plane activity, such as subscription, resource, and administrative operations. |
| `AzureActivity` | Table containing Azure Activity log data. |
| Analytics rule | Detects suspicious behavior and can create alerts or incidents. |
| Hunting query | KQL query used by analysts to proactively search for threats. |
| Workbook | Dashboard or visualization for analyzing security data. |
| UEBA | User and Entity Behavior Analytics; identifies unusual activity by comparing behavior with normal patterns. |
| Analytics tier | High-performance data tier used for active investigation, detection, and hunting. |

***

# Plain-English Takeaway

You confirmed that Microsoft Sentinel can receive security data from Microsoft Defender for Cloud and Azure Activity logs. You checked that both connectors were **connected**, identified which log tables hold their data, reviewed retention settings, and verified that UEBA was enabled for Azure Activity.

# Learning Path 7 — Lab 1, Exercise 2  
## Connect Windows Devices to Microsoft Sentinel

## Purpose

Connect Windows devices to Microsoft Sentinel so their Windows security events can be collected, stored, searched, and used for detections and investigations.

This lab covered:

- Creating an Azure-hosted Windows 11 virtual machine.
- Connecting a non-Azure Windows server through **Azure Arc**.
- Using the **Windows Security Events via AMA** connector.
- Creating a **Data Collection Rule (DCR)** to collect Windows security events.

> **Lab note:** This is a shared, standalone environment. Coordinate with other learners before creating or changing resources. Leaving the lab may require repeating the work.

***

# Key Concepts

| Term | Plain-English meaning |
|---|---|
| Azure Arc | Connects non-Azure servers to Azure management services. |
| Azure Monitor Agent (AMA) | Agent that collects monitoring and security data from a machine. |
| Data Collection Rule (DCR) | Configuration that defines **which machines** send data and **which events** they send. |
| Windows Security Events via AMA | Sentinel connector that collects Windows Security event logs through AMA. |
| `SecurityEvent` table | Sentinel table that stores Windows Security event data collected through this connector. |
| Azure VM | A virtual machine running inside Azure. |
| Non-Azure machine | A server located on-premises or in another cloud; it needs Azure Arc before AMA-based Sentinel collection can be configured. |

The Azure Arc Connected Machine agent connects a non-Azure server to Azure, while AMA is a separate agent used to collect monitoring data. [learn.microsoft](https://learn.microsoft.com/en-us/azure/azure-arc/servers/agent-overview)

***

# Task 1 — Create an Azure Windows VM

## Purpose

Create a Windows 11 virtual machine in Azure that will later be connected to Microsoft Sentinel.

## Access Azure

1. Sign in to `WIN1` as `Admin`.
2. Open Microsoft Edge.
3. Navigate to:

```text
https://portal.azure.com
```

4. Sign in with the provided tenant account, password, or Temporary Access Pass (TAP).

## Create the VM

1. Select:

```text
+ Create a resource
```

2. Search for:

```text
Windows 11
```

3. Select the Windows 11 offer.

4. Configure:

| Setting | Lab value |
|---|---|
| Subscription | `PROD-CSS-MOCTAP-lod53406000` |
| Plan | `Windows 11 Enterprise, version 24H2` |
| Preset configuration | `Dev/Test` |
| Resource group | `ResourceGroup1` |
| Virtual machine name | `AZWIN-66624652` |
| Region | `(US) East US` |
| Image | `Windows 11 Enterprise, version 24H2` |
| VM size | `Standard_D2s_v3` |
| Username | A unique non-reserved name |
| Password | A strong password |
| Licensing | Confirm eligible license |

> Use a unique VM name in shared environments, for example `AZWIN-[initials][number]`.

## VM Size / Deployment Notes

If `Standard_D2s_v3` is unavailable, try:

```text
Standard_D2s_v5
Standard_D2as_v5
Standard_D2s_v6
```

If Automanage causes an image-support error:

```text
Management tab
> Disable Automanage
```

## Create

1. Select:

```text
Review + create
```

2. Wait for validation to pass.
3. Select:

```text
Create
```

4. Wait for deployment to finish.

> Screenshot placeholder: *Windows 11 VM configuration before creation.*  
> Screenshot placeholder: *Successful Azure VM deployment.*  
> Screenshot placeholder: *AZWIN VM visible in ResourceGroup1.*

***

# Task 2 — Connect Non-Azure Server with Azure Arc

## Purpose

Connect the on-premises-style `WINServer` virtual machine to the Azure subscription using Azure Arc.

This makes the non-Azure server manageable in Azure and allows it to be selected for AMA-based Microsoft Sentinel data collection. Azure Arc is required before using the AMA Windows Security Events connector for machines outside Azure. [learn.microsoft](https://learn.microsoft.com/en-us/azure/azure-arc/servers/scenario-onboard-azure-sentinel)

## Access the Server

1. Switch to the `WINServer` virtual machine.
2. Sign in as:

```text
Username: Administrator
Password: Passw0rd!
```

3. Open an elevated Command Prompt:

```text
Search > cmd
> Right-click Command Prompt
> Run as administrator
```

## Connect Azure Arc

Run:

```cmd
azcmagent connect -g "ResourceGroup1" -l "CentralUS" -s "02ab4807-5d28-4850-af2c-d9bfbaccd949"
```

If prompted to choose a browser, select:

```text
Microsoft Edge
```

Sign in using the lab tenant credentials, wait for authentication to complete, then close the browser tab and return to Command Prompt.

## Verify Connection

Run:

```cmd
azcmagent show
```

Confirm the output contains:

```text
Agent status: Connected
```

`azcmagent show` displays the Connected Machine agent’s current connection state and Azure resource details. [learn.microsoft](https://learn.microsoft.com/en-us/azure/azure-arc/servers/azcmagent-show)

> Screenshot placeholder: *Azure Arc sign-in / authentication completed.*  
> Screenshot placeholder: *`azcmagent show` output showing Agent status: Connected.*  
> Screenshot placeholder: *WINServer visible as an Azure Arc-enabled server.*

***

# Task 3 — Connect Azure Windows VM to Sentinel

## Purpose

Configure Sentinel to collect Windows Security events from the Azure VM using a DCR.

## Open the Connector

1. On `WIN1`, open:

```text
https://security.microsoft.com
```

2. Navigate to:

```text
Microsoft Sentinel
> Content management
> Content hub
```

3. Search for:

```text
Windows Security Events
```

4. Open the solution and select:

```text
Manage
```

## Solution Content

The solution includes:

| Content | Quantity |
|---|---:|
| Windows Security Events via AMA connector | 1 |
| Security Events via Legacy Agent connector | 1 |
| Workbooks | 2 |
| Analytics rules | 20 |
| Hunting queries | 43 |

## Create a Data Collection Rule

1. Select:

```text
Windows Security Events via AMA
> Open connector page
```

2. Under **Configuration**, select:

```text
+ Create data collection rule
```

3. Enter a unique rule name, for example:

```text
AZWIN[student-number]DCR
```

4. Select:

```text
Next: Resources
```

5. Expand:

```text
MOC Subscription
> ResourceGroup1
```

6. Select your Azure VM:

```text
AZWIN-66624652
```

7. Select:

```text
Next: Collect
```

8. Keep the default collection setting:

```text
All Security Events
```

9. Select:

```text
Next: Review + create
> Create
```

10. Wait briefly and select **Refresh** to confirm the DCR appears.

DCRs define the data to collect from agents and the machines to which the collection configuration applies. Selecting **All Security Events** collects the full Windows Security event set through the connector. [docs.azure](https://docs.azure.cn/en-us/sentinel/connect-services-windows-based)

> Screenshot placeholder: *Windows Security Events via AMA connector page.*  
> Screenshot placeholder: *DCR resources page with AZWIN VM selected.*  
> Screenshot placeholder: *DCR set to All Security Events.*  
> Screenshot placeholder: *Created DCR listed in connector configuration.*

***

# Task 4 — Add the Non-Azure Windows Server

## Purpose

Add the Azure Arc-connected `WINServer` to the existing DCR so Sentinel collects its Windows Security events too.

## Update the DCR

1. Stay in:

```text
Windows Security Events via AMA
> Configuration
```

2. Find the DCR created in Task 3.

3. Select the **pencil icon** to edit it.

4. Select:

```text
Next: Resources
```

5. Expand:

```text
MOC Subscription
> ResourceGroup1
```

6. Select:

```text
WINServer
```

7. Select:

```text
Next: Collect
> Next: Review + create
```

8. After **Validation passed**, select:

```text
Create
```

> Screenshot placeholder: *WINServer selected as a DCR resource.*  
> Screenshot placeholder: *Updated DCR successfully created / validated.*

***

# Data Flow

```text
Azure VM (AZWIN)
        |
        | Windows Security events
        v
Azure Monitor Agent
        |
        v
Data Collection Rule
        |
        v
Windows Security Events via AMA connector
        |
        v
Microsoft Sentinel SecurityEvent table
```

```text
Non-Azure WINServer
        |
        | Azure Arc connection
        v
Azure Arc-enabled server
        |
        | Azure Monitor Agent + DCR
        v
Windows Security Events via AMA connector
        |
        v
Microsoft Sentinel SecurityEvent table
```

***

# Key Takeaways

- Azure VMs can be selected directly in a Microsoft Sentinel DCR.
- Non-Azure Windows servers need an active **Azure Arc** connection before they can use the AMA-based Windows Security Events connector. [learn.microsoft](https://learn.microsoft.com/en-us/azure/azure-arc/servers/scenario-onboard-azure-sentinel)
- `azcmagent connect` registers a server with Azure Arc.
- `azcmagent show` confirms whether the Azure Arc agent is connected. [learn.microsoft](https://learn.microsoft.com/en-us/azure/azure-arc/servers/azcmagent)
- The DCR determines which computers send data and what type of Windows Security events are collected.
- Choosing **All Security Events** provides the broadest visibility but can increase data ingestion and storage costs.
- Collected Windows security logs are written to the Sentinel `SecurityEvent` table. [docs.azure](https://docs.azure.cn/en-us/azure-monitor/vm/data-collection-windows-events)
