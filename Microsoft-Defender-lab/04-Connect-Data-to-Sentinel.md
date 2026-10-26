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
> Screenshot placeholder: *Connector details showing the SecurityAlert table.*

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
> Screenshot placeholder: *AzureActivity table retention settings.*  
> Screenshot placeholder: *UEBA enabled for Azure Activity.*

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
