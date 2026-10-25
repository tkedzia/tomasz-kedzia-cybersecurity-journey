# Learning Path 5 — Lab 1, Exercise 1  
## Microsoft Sentinel KQL Queries

## Purpose

Use **KQL** in Microsoft Defender XDR Advanced Hunting to search, filter, aggregate, visualize, correlate, and parse security logs.
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/0425876e-d490-4d46-899c-038fe282abd7" />

```text
https://security.microsoft.com
Investigation & response > Hunting > Advanced hunting
```

**Lab tables:**

```kql
SecurityEvent_CL
SigninLogs_CL
```

> Use `|` to pass results from one operation to the next. Filter early—especially by time—to improve query performance.

***

## Basic Queries

### Display a table

```kql
SecurityEvent_CL
```
<img width="1367" height="771" alt="image" src="https://github.com/user-attachments/assets/4e5a83a9-3a0f-4bfe-8ad7-eb5a0f01ea0a" />

### Broad text search

```kql
search "Computer"
```
<img width="1398" height="892" alt="image" src="https://github.com/user-attachments/assets/a4c716ea-6b54-4564-92f7-981089c32212" />

```kql
search in (SecurityEvent_CL, App*) "new"
```

> `search` is useful for exploration but less efficient than filtering specific tables and columns.

<img width="1918" height="985" alt="image" src="https://github.com/user-attachments/assets/57741580-55c2-431e-b0b3-f0975c01a7c7" />

### Filter with `where`

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
```
<img width="1907" height="887" alt="image" src="https://github.com/user-attachments/assets/d2eef85b-a96d-412f-ad31-a260a3097d01" />
<img width="1892" height="770" alt="image" src="https://github.com/user-attachments/assets/93b026c1-1c33-4e40-bcc9-f63a979e50e5" />

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s == 4624
```
<img width="1893" height="772" alt="image" src="https://github.com/user-attachments/assets/b7f5c271-1443-48eb-aadb-37a25cba313f" />

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s == 4624
| where AccountType_s =~ "user"
```
<img width="1890" height="761" alt="image" src="https://github.com/user-attachments/assets/23fe15d9-b7a2-4b0f-834d-be2d8fdcbedd" />

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s in (4624, 4625)
```
<img width="1888" height="765" alt="image" src="https://github.com/user-attachments/assets/2f3e8303-6de5-4d2e-9594-a07a058dfe6d" />

| Event ID | Meaning |
|---|---|
| `4624` | Successful logon |
| `4625` | Failed logon |
| `4634` | Logoff |
| `4672` | Privileged logon |
| `4688` | Process creation |

***

## Variables with `let`

### Define values

```kql
let timeOffset = 1d;
let discardEventID = 4688;
SecurityEvent_CL
| where TimeGenerated > ago(timeOffset * 60)
| where TimeGenerated < ago(timeOffset)
| where EventID_s != discardEventID
```
## In Plain English

> Show all security events that occurred **between 60 days ago and 1 day ago**, excluding **process-creation events**.

```text
60 days ago  ----------------  1 day ago  ---  Now
      Include events here          Exclude recent events
```

> **Note:** `timeOffset` may appear more flexible than it really is. Changing its value changes both boundaries:
>
> - Older boundary: `timeOffset * 60`
> - Newer boundary: `timeOffset`
<img width="1892" height="783" alt="image" src="https://github.com/user-attachments/assets/433bf1fb-55bd-45d4-962f-9df05815be14" />

### Define suspicious accounts

```kql
let suspiciousAccounts = datatable(account: string) [
    @"NA\timadmin",
    @"NT AUTHORITY\SYSTEM"
];
SecurityEvent_CL
| where TimeGenerated > ago(7d)
| where Account_s in (suspiciousAccounts)
```
### In Plain English

> Create a short watchlist of two accounts, then show security events from the last seven days involving either account.

```kql
let suspiciousAccounts = datatable(account: string) [
    @"NA\timadmin",
    @"NT AUTHORITY\SYSTEM"
];
```

This creates a temporary table named `suspiciousAccounts`.

It contains two account names:

```text
NA\timadmin
NT AUTHORITY\SYSTEM
```

The `@` before the first account means KQL treats the backslash as a normal character, not as a special escape character.

```kql
SecurityEvent_CL
```

Search the `SecurityEvent_CL` log table.

```kql
| where TimeGenerated > ago(7d)
```

Keep only events from the last **seven days**.

```kql
| where Account_s in (suspiciousAccounts)
```

Keep only events where the `Account_s` field matches an account in the temporary watchlist.

```text
All security events
        |
        +-- Keep events from the last 7 days
                |
                +-- Keep events involving timadmin or SYSTEM
```

<img width="1916" height="757" alt="image" src="https://github.com/user-attachments/assets/01700fd0-274c-4ffe-b576-e2fa38d188fd" />

### Define a reusable result set

```kql
let LowActivityAccounts =
    SecurityEvent_CL
    | summarize cnt = count() by Account_s
    | where cnt < 1000;
LowActivityAccounts
| where Account_s contains "sql"
```
### In Plain English

> First, create a temporary list of accounts with fewer than 1,000 recorded events. Then, from that list, show only accounts whose name contains `sql`.

```kql
let LowActivityAccounts =
```

Creates a reusable temporary result set named:

```text
LowActivityAccounts
```

```kql
SecurityEvent_CL
| summarize cnt = count() by Account_s
```

Looks at all security events and counts how many events belong to each account.

Example intermediate result:

| Account_s | cnt |
|---|---:|
| `DOMAIN\sqlservice` | 25 |
| `DOMAIN\administrator` | 1,500 |
| `DOMAIN\sqlbackup` | 87 |

```kql
| where cnt < 1000;
```

Keeps only accounts with fewer than 1,000 events.

```kql
LowActivityAccounts
| where Account_s contains "sql"
```

Queries the temporary list and returns only account names containing `sql`.

```text
All security events
        |
        +-- Count events for each account
                |
                +-- Keep accounts with fewer than 1,000 events
                        |
                        +-- Show accounts whose name contains "sql"
```

> **Important:** “Low activity” here means fewer than 1,000 events in the entire table. Because the query has no `TimeGenerated` filter, it does not mean low activity over a specific period such as the last 7 days.

<img width="1910" height="805" alt="image" src="https://github.com/user-attachments/assets/43a8c4b7-650b-4016-8fd0-bced3cd2b3d5" />

***

## Aggregation with `summarize`

### Count events by device

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s == 4688
| summarize count() by Computer
```
### In Plain English

> Show how many **process-creation events** occurred on each computer during the last **five days**.

```kql
SecurityEvent_CL
```

Start with the `SecurityEvent_CL` security-log table.

```kql
| where TimeGenerated > ago(5d)
```

Keep only events generated within the last **five days**.

```kql
| where EventID_s == 4688
```

Keep only events with ID `4688`, which commonly represents a **new process being created** on Windows.

```kql
| summarize count() by Computer
```

Group the remaining events by computer name and count them.

Example result:

| Computer | count_ |
|---|---:|
| `WIN1` | 450 |
| `VictimPC2` | 1,250 |
| `Server01` | 87 |

```text
All security events
        |
        +-- Keep events from the last 5 days
                |
                +-- Keep only process-creation events (4688)
                        |
                        +-- Group by computer
                                |
                                +-- Count events per computer
```

> **Why this is useful:** A computer with an unusually high number of process-creation events may deserve investigation, especially if the activity is unexpected for that device.

<img width="1912" height="813" alt="image" src="https://github.com/user-attachments/assets/1f3f81ef-92ec-4f60-bf93-5e80edc60425" />

### Count logons by account type and device

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s == 4624
| summarize cnt = count() by AccountType_s, Computer
```
### In Plain English

> Show how many **successful logons** occurred during the last **five days**, grouped by both **account type** and **computer**.

```kql
SecurityEvent_CL
```

Start with the `SecurityEvent_CL` security-log table.

```kql
| where TimeGenerated > ago(5d)
```

Keep only events generated within the last **five days**.

```kql
| where EventID_s == 4624
```

Keep only events with ID `4624`, which commonly represents a **successful Windows logon**.

```kql
| summarize cnt = count() by AccountType_s, Computer
```

Group the remaining events by:

- `AccountType_s` — for example, `User`, `Machine`, or `Service`
- `Computer` — the device where the logon happened

Then count the events in each combination. The count is named `cnt`.

Example result:

| AccountType_s | Computer | cnt |
|---|---|---:|
| `User` | `WIN1` | 42 |
| `Machine` | `WIN1` | 15 |
| `User` | `Server01` | 109 |

```text
All security events
        |
        +-- Keep events from the last 5 days
                |
                +-- Keep only successful logons (4624)
                        |
                        +-- Group by account type and computer
                                |
                                +-- Count successful logons in each group
```

> **Why this is useful:** It helps identify which account types are logging into which devices and highlights unusually high logon activity.

<img width="1898" height="758" alt="image" src="https://github.com/user-attachments/assets/28edc586-1441-4086-8cc1-f93133f2339c" />

### Count distinct IP addresses

```kql
SigninLogs_CL
| where TimeGenerated > ago(5d)
| summarize dcount(IPAddress)
```
### In Plain English

> Estimate how many **different IP addresses** attempted to sign in during the last **five days**.

```kql
SigninLogs_CL
```

Start with the `SigninLogs_CL` sign-in log table.

```kql
| where TimeGenerated > ago(5d)
```

Keep only sign-in events from the last **five days**.

```kql
| summarize dcount(IPAddress)
```

Count the number of **distinct** IP addresses in those results.

`dcount()` means **distinct count**. It provides an approximate count, which is usually faster and suitable when working with large log volumes.

Example result:

| dcount_IPAddress |
|---:|
| 143 |

```text
All sign-in events
        |
        +-- Keep events from the last 5 days
                |
                +-- Count unique IP addresses
```

> **Why this is useful:** A sudden increase in unique source IP addresses can indicate unusual access activity, password spraying, distributed attacks, or a large number of users connecting through different networks.

<img width="1910" height="810" alt="image" src="https://github.com/user-attachments/assets/89144ae6-1eab-48a7-99b8-ffd1d9b9a611" />

### Find disabled-account attempts

```kql
let timeframe = 30d;
let threshold = 1;
SigninLogs_CL
| where TimeGenerated >= ago(timeframe)
| where ResultDescription has "User account is disabled"
| summarize applicationCount = dcount(AppDisplayName_s) by UserPrincipalName_s, IPAddress
| where applicationCount >= threshold
```
### In Plain English

> Find disabled user accounts that attempted to sign in during the last **30 days**, then show which applications they tried to access and from which IP addresses.

```kql
let timeframe = 30d;
```

Create a variable called `timeframe` and set it to **30 days**.

```kql
let threshold = 1;
```

Create a variable called `threshold` and set it to `1`.

This means an account is included if it attempted to access at least **one distinct application**.

```kql
SigninLogs_CL
```

Start with the `SigninLogs_CL` sign-in log table.

```kql
| where TimeGenerated >= ago(timeframe)
```

Keep only sign-in attempts from the last **30 days**.

```kql
| where ResultDescription has "User account is disabled"
```

Keep only events where the sign-in failed because the account is disabled.

```kql
| summarize applicationCount = dcount(AppDisplayName_s) by UserPrincipalName_s, IPAddress
```

Group the remaining events by:

- `UserPrincipalName_s` — the disabled account that attempted sign-in
- `IPAddress` — the source IP address used

For each account-and-IP combination, count the approximate number of **different applications** targeted. Store this number in `applicationCount`.

```kql
| where applicationCount >= threshold
```

Show only results with at least one targeted application.

Because the threshold is `1`, this final filter does not remove much; every matching event should already involve at least one application.

Example result:

| UserPrincipalName_s | IPAddress | applicationCount |
|---|---|---:|
| `former.employee@company.com` | `203.0.113.10` | 3 |
| `disabled.admin@company.com` | `198.51.100.25` | 1 |

```text
All sign-in events
        |
        +-- Keep events from the last 30 days
                |
                +-- Keep failures caused by disabled accounts
                        |
                        +-- Group by account and source IP
                                |
                                +-- Count distinct applications targeted
                                        |
                                        +-- Keep results meeting the threshold
```

> **Why this is useful:** A disabled account attempting to sign in may be expected—for example, from an old device with cached credentials—or suspicious, such as an attacker continuing to use stolen credentials after the account was disabled.

<img width="1907" height="796" alt="image" src="https://github.com/user-attachments/assets/f7f26965-31dd-40e5-a928-396d7bcf6eb3" />

***

## Latest and Earliest Events

### Latest event: `arg_max()`

```kql
SecurityEvent_CL
| where Computer == "VictimPC2"
| summarize arg_max(TimeGenerated, *) by Computer
```
### In Plain English

> Find the **most recent security event** recorded for the computer named `VictimPC2`.

```kql
SecurityEvent_CL
```

Start with the `SecurityEvent_CL` security-log table.

```kql
| where Computer == "VictimPC2"
```

Keep only events from the computer:

```text
VictimPC2
```

```kql
| summarize arg_max(TimeGenerated, *) by Computer
```

Find the row with the **latest** `TimeGenerated` value for that computer.

- `arg_max()` means: return the row where a chosen value is the largest.
- `TimeGenerated` is the value used to decide which row is latest.
- `*` means: return **all columns** from that latest event.
- `by Computer` groups results by computer. In this case, only `VictimPC2` is included.

Example result:

| Computer | TimeGenerated | EventID_s | Account_s |
|---|---|---:|---|
| `VictimPC2` | `2026-10-25 04:30:15` | `4624` | `DOMAIN\alex` |

```text
All security events
        |
        +-- Keep only events from VictimPC2
                |
                +-- Find the newest event by time
                        |
                        +-- Return all details from that event
```

> **Why this is useful:** It quickly shows the most recent known activity on a device, which can help determine whether it is still active or what happened most recently during an investigation.

<img width="1897" height="806" alt="image" src="https://github.com/user-attachments/assets/c6be39f3-59d7-458c-97a2-1f4dc9cc5082" />

### Earliest event: `arg_min()`

```kql
SecurityEvent_CL
| where Computer == "VictimPC2"
| summarize arg_min(TimeGenerated, *) by Computer
```
### In Plain English

> Find the **oldest security event** recorded for the computer named `VictimPC2`.

```kql
SecurityEvent_CL
```

Start with the `SecurityEvent_CL` security-log table.

```kql
| where Computer == "VictimPC2"
```

Keep only events from this computer:

```text
VictimPC2
```

```kql
| summarize arg_min(TimeGenerated, *) by Computer
```

Find the row with the **earliest** `TimeGenerated` value for that computer.

- `arg_min()` means: return the row where a selected value is smallest.
- `TimeGenerated` is used to determine which event happened first.
- `*` means: return **all columns** from that event.
- `by Computer` groups the output by computer.

Example result:

| Computer | TimeGenerated | EventID_s | Account_s |
|---|---|---:|---|
| `VictimPC2` | `2026-10-20 08:12:34` | `4624` | `DOMAIN\alex` |

```text
All security events
        |
        +-- Keep only events from VictimPC2
                |
                +-- Find the oldest event by time
                        |
                        +-- Return all details from that event
```

> **Why this is useful:** It helps establish the earliest activity available for a device within the retained log data, which can help build an investigation timeline.

<img width="1906" height="786" alt="image" src="https://github.com/user-attachments/assets/7a4999fe-9434-4576-9c39-92cb8775d3f8" />

### Pipe-order lesson

**Accounts whose latest overall event was a logon:**

```kql
SecurityEvent_CL
| summarize arg_max(TimeGenerated, *) by Account_s
| where EventID_s == 4624
```
### In Plain English

> Find each account’s **most recent security event**, then keep only accounts where that most recent event was a **successful logon**.

```kql
SecurityEvent_CL
```

Start with all records in the `SecurityEvent_CL` security-log table.

```kql
| summarize arg_max(TimeGenerated, *) by Account_s
```

For every account:

- Find the event with the latest `TimeGenerated` timestamp.
- Return all details (`*`) from that event.
- Produce one row per account.

```kql
| where EventID_s == 4624
```

From those latest events, keep only rows where the event ID is `4624`, meaning a **successful Windows logon**.

Example:

| Account | Latest event | Included? |
|---|---:|---|
| `DOMAIN\alice` | `4624` — successful logon | Yes |
| `DOMAIN\bob` | `4634` — logoff | No |
| `DOMAIN\admin` | `4688` — process creation | No |

```text
All security events
        |
        +-- Find the newest event for every account
                |
                +-- Keep accounts whose newest event is a successful logon
```

> **Important:** This does **not** show the latest successful logon for every account. It shows only accounts whose latest activity of any kind was a successful logon.

<img width="1898" height="750" alt="image" src="https://github.com/user-attachments/assets/6425e3aa-9fa6-49db-8270-6ed35fd89ade" />

**Latest successful logon for every account that logged on:**

```kql
SecurityEvent_CL
| where EventID_s == 4624
| summarize arg_max(TimeGenerated, *) by Account_s
```

> Filter before `summarize` when looking for the latest event of a specific type.

### In Plain English

> For every account that has successfully logged on, show its **most recent successful logon**.

```kql
SecurityEvent_CL
```

Start with all records in the `SecurityEvent_CL` security-log table.

```kql
| where EventID_s == 4624
```

Keep only event ID `4624`, which represents a **successful Windows logon**.

```kql
| summarize arg_max(TimeGenerated, *) by Account_s
```

For each account:

- Find the row with the latest `TimeGenerated` value.
- Return all columns (`*`) from that latest successful-logon event.
- Return one result row per account.

Example result:

| Account_s | TimeGenerated | Computer | EventID_s |
|---|---|---|---:|
| `DOMAIN\alice` | `2026-10-25 05:30:12` | `WIN1` | `4624` |
| `DOMAIN\bob` | `2026-10-24 18:42:07` | `Server01` | `4624` |

```text
All security events
        |
        +-- Keep only successful logons (4624)
                |
                +-- Group by account
                        |
                        +-- Find the newest successful logon per account
```

> **Key difference:** The query filters for logons **before** finding the newest event. Therefore, an account is included even if its most recent overall activity was later a logoff, process creation, or another event.

<img width="1897" height="801" alt="image" src="https://github.com/user-attachments/assets/b21c0e1c-5ddf-4842-90f5-6c4fd42152cd" />

***

## Lists and Sets

### Include duplicates: `make_list()`

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s == 4624
| summarize make_list(Account_s) by Computer
```
### In Plain English

> For each computer, create a list of all accounts that successfully logged on during the last five days. The same account can appear more than once.

```kql
SecurityEvent_CL
```

Start with the `SecurityEvent_CL` security-log table.

```kql
| where TimeGenerated > ago(5d)
```

Keep only events from the last **five days**.

```kql
| where EventID_s == 4624
```

Keep only event ID `4624`, which represents a **successful Windows logon**.

```kql
| summarize make_list(Account_s) by Computer
```

Group the remaining events by `Computer`.

For each computer, create a list containing the `Account_s` value from every successful logon event.

Example result:

| Computer | list_Account_s |
|---|---|
| `WIN1` | `["DOMAIN\alice", "DOMAIN\bob", "DOMAIN\alice"]` |
| `Server01` | `["DOMAIN\admin", "DOMAIN\service_sql"]` |

`DOMAIN\alice` appears twice for `WIN1` because that account logged on successfully twice.

```text
All security events
        |
        +-- Keep events from the last 5 days
                |
                +-- Keep successful logons (4624)
                        |
                        +-- Group by computer
                                |
                                +-- Create a list of all logged-on accounts
```

> **Why this is useful:** It shows which accounts were used on each computer and how frequently they appear. Duplicate entries can help reveal repeated logons.

<img width="1902" height="810" alt="image" src="https://github.com/user-attachments/assets/6ae8dc5e-0544-4b14-a072-480eb144adfe" />

### Unique values: `make_set()`

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s == 4624
| summarize make_set(Account_s) by Computer
```
### In Plain English

> For each computer, create a list of the **unique accounts** that successfully logged on during the last five days.

```kql
SecurityEvent_CL
```

Start with the `SecurityEvent_CL` security-log table.

```kql
| where TimeGenerated > ago(5d)
```

Keep only events from the last **five days**.

```kql
| where EventID_s == 4624
```

Keep only event ID `4624`, which represents a **successful Windows logon**.

```kql
| summarize make_set(Account_s) by Computer
```

Group the remaining events by `Computer`.

For each computer, create a set of unique `Account_s` values. If the same user logged on multiple times, that user appears only once.

Example result:

| Computer | set_Account_s |
|---|---|
| `WIN1` | `["DOMAIN\alice", "DOMAIN\bob"]` |
| `Server01` | `["DOMAIN\admin", "DOMAIN\service_sql"]` |

Even if `DOMAIN\alice` logged on ten times to `WIN1`, `make_set()` shows it once.

```text
All security events
        |
        +-- Keep events from the last 5 days
                |
                +-- Keep successful logons (4624)
                        |
                        +-- Group by computer
                                |
                                +-- Create a unique list of logged-on accounts
```

> **Key difference:** `make_list()` preserves duplicates; `make_set()` removes duplicates.

<img width="1900" height="818" alt="image" src="https://github.com/user-attachments/assets/e4f1cbfb-67a7-4d15-a07a-a9e84d2817dd" />

***

## Visualizations

### Column chart

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| summarize count() by Account_s
| render columnchart
```
### In Plain English

> Create a bar chart showing how many security events each account generated during the last five days.

```kql
SecurityEvent_CL
```

Start with the `SecurityEvent_CL` security-log table.

```kql
| where TimeGenerated > ago(5d)
```

Keep only events from the last **five days**.

```kql
| summarize count() by Account_s
```

Group the events by account and count the number of events for each account.

```kql
| render columnchart
```

Display the results as a **column chart**.

Example data before visualization:

| Account_s | count_ |
|---|---:|
| `DOMAIN\alice` | 120 |
| `DOMAIN\bob` | 45 |
| `DOMAIN\admin` | 310 |

```text
All security events
        |
        +-- Keep events from the last 5 days
                |
                +-- Group events by account
                        |
                        +-- Count events per account
                                |
                                +-- Display as a column chart
```

> **Why this is useful:** It makes unusually active accounts easier to spot visually. High event counts are not automatically malicious, but they can indicate accounts worth investigating.

<img width="1902" height="806" alt="image" src="https://github.com/user-attachments/assets/278da484-4970-4676-9c90-8f3a6799ba49" />

### Time-series chart

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| summarize count() by bin(TimeGenerated, 1m)
| render timechart
```

> `bin()` groups timestamps into fixed intervals.

### In Plain English

> Create a line chart showing how many security events occurred each minute during the last five days.

```kql
SecurityEvent_CL
```

Start with the `SecurityEvent_CL` security-log table.

```kql
| where TimeGenerated > ago(5d)
```

Keep only events from the last **five days**.

```kql
| summarize count() by bin(TimeGenerated, 1m)
```

Group event timestamps into **one-minute intervals**, then count how many events occurred in each minute.

```kql
| render timechart
```

Display the results as a **time-series line chart**.

Example data before visualization:

| TimeGenerated | count_ |
|---|---:|
| `2026-10-25 05:40:00` | 12 |
| `2026-10-25 05:41:00` | 8 |
| `2026-10-25 05:42:00` | 47 |

```text
All security events
        |
        +-- Keep events from the last 5 days
                |
                +-- Group timestamps into 1-minute intervals
                        |
                        +-- Count events per minute
                                |
                                +-- Display as a line chart
```

> `bin(TimeGenerated, 1m)` rounds timestamps into one-minute buckets. For example, events at `05:40:12` and `05:40:55` are both counted in the `05:40:00` interval.

> **Why this is useful:** It helps identify spikes in activity, such as a sudden burst of logons, process creation, or other security events.

<img width="1908" height="807" alt="image" src="https://github.com/user-attachments/assets/d62f8efa-058d-43ba-aa52-a49cd04c8585" />

***

## Multi-Table Queries

### `union`

Append rows from tables:

```kql
SecurityEvent_CL
| union SigninLogs_CL
```
### In Plain English

> Combine all rows from the `SecurityEvent_CL` table with all rows from the `SigninLogs_CL` table into one result set.

```kql
SecurityEvent_CL
```

Start with all records from the `SecurityEvent_CL` table.

```kql
| union SigninLogs_CL
```

Add all records from the `SigninLogs_CL` table below them.

```text
SecurityEvent_CL rows
        +
SigninLogs_CL rows
        =
One combined result set
```

Example conceptually:

| Source table | Example data |
|---|---|
| `SecurityEvent_CL` | Windows logon, logoff, process-creation events |
| `SigninLogs_CL` | Cloud sign-in events, application access, source IP data |

> **Important:** `union` does **not** match related records together. It simply appends rows from one or more tables.

> **Difference from `join`:** Use `union` to combine similar log data into one list; use `join` when you want to match records between tables based on a shared value, such as an account name or IP address.

<img width="1907" height="820" alt="image" src="https://github.com/user-attachments/assets/80ad3dfe-e93d-4d1e-837c-0ee3380cf705" />

Count all rows:

```kql
SecurityEvent_CL
| union SigninLogs_CL
| summarize count()
```
### In Plain English

> Count the total number of records in both the `SecurityEvent_CL` and `SigninLogs_CL` tables combined.

```kql
SecurityEvent_CL
```

Start with all security-event records.

```kql
| union SigninLogs_CL
```

Add all sign-in-log records to the same result set.

```kql
| summarize count()
```

Count every row in the combined result set.

Example:

| Table | Number of rows |
|---|---:|
| `SecurityEvent_CL` | 8,000 |
| `SigninLogs_CL` | 2,500 |
| **Combined count** | **10,500** |

```text
SecurityEvent_CL rows
        +
SigninLogs_CL rows
        |
        +-- Count all combined rows
```

> **Important:** This counts **events**, not unique users, devices, or IP addresses. If the same user has 50 events, all 50 rows are included in the total.

<img width="1910" height="817" alt="image" src="https://github.com/user-attachments/assets/b53c92ce-02a1-4780-a173-cb2dab2ce1d6" />

Use a wildcard:

```kql
union Sec*
| summarize count() by Type
```


### `join`

Match logon and logoff activity by account:

```kql
SecurityEvent_CL
| where EventID_s == 4624
| summarize LogOnCount = count() by Account_s
| project LogOnCount, Account_s
| join kind = inner (
    SecurityEvent_CL
    | where EventID_s == 4634
    | summarize LogOffCount = count() by Account_s
    | project LogOffCount, Account_s
) on Account_s
```

| Operator | Purpose |
|---|---|
| `union` | Appends rows from multiple tables |
| `join` | Matches rows using a shared field |
| `inner` | Returns matching rows only |
| `leftouter` | Returns all left-table rows and matching right-table rows |
| `leftanti` | Returns left-table rows without a match |

***

## Strings and JSON

### Extract with regex

```kql
print extract("x=([0-9.]+)", 1, "hello x=45.6|wo")
```

### Extract account name

```kql
SecurityEvent_CL
| where EventID_s == 4672
| where AccountType_s == "User"
| extend Account_Name = extract(@"^(.*\\)?([^@]*)(@.*)?$", 2, tolower(Account_s))
| summarize LoginCount = count() by Account_Name
| where Account_Name != ""
| where LoginCount < 10
```

### Parse structured text

```kql
Traces
| parse EventText with * "resourceName=" resourceName ", totalSlices=" totalSlices:long *
| project resourceName, totalSlices
```

### Parse JSON

```kql
SigninLogs_CL
| extend AuthDetails = parse_json(AuthenticationDetails_s)
| extend AuthMethod = AuthDetails[0].authenticationMethod
| extend AuthResult = AuthDetails[0]["authenticationStepResultDetail"]
| project AuthMethod, AuthResult
```

### Expand JSON arrays

```kql
SigninLogs_CL
| mv-expand AuthDetails = parse_json(AuthenticationDetails_s)
| project AuthDetails
```

### Filter each JSON-array element

```kql
SigninLogs_CL
| mv-apply AuthDetails = parse_json(AuthenticationDetails_s) on
(
    where AuthDetails.authenticationMethod == "Password"
)
```

***

## Saved Functions

A function is a saved KQL query that can be reused.

1. Run the query.
2. Select **Save** > **Save as function**.
3. Name it, for example:

```text
PrivLogins
```

4. Save under **My functions** or **Shared functions**.

Run it with:

```kql
PrivLogins
```

***

## Key Takeaways

| KQL element | Use |
|---|---|
| `where` | Filter records |
| `let` | Define variables, lists, or temporary tables |
| `summarize` | Group and aggregate results |
| `dcount()` | Approximate distinct count |
| `arg_max()` | Most recent record |
| `arg_min()` | Oldest record |
| `make_list()` | List, including duplicates |
| `make_set()` | Unique list |
| `render` | Chart results |
| `union` | Append tables |
| `join` | Correlate tables |
| `extract()` | Regex extraction |
| `parse_json()` | Convert JSON string to dynamic data |
| `mv-expand` | Convert array items into rows |
