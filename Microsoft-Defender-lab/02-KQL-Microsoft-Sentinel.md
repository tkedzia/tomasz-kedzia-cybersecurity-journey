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

***

## Aggregation with `summarize`

### Count events by device

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s == 4688
| summarize count() by Computer
```

### Count logons by account type and device

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s == 4624
| summarize cnt = count() by AccountType_s, Computer
```

### Count distinct IP addresses

```kql
SigninLogs_CL
| where TimeGenerated > ago(5d)
| summarize dcount(IPAddress)
```

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

***

## Latest and Earliest Events

### Latest event: `arg_max()`

```kql
SecurityEvent_CL
| where Computer == "VictimPC2"
| summarize arg_max(TimeGenerated, *) by Computer
```

### Earliest event: `arg_min()`

```kql
SecurityEvent_CL
| where Computer == "VictimPC2"
| summarize arg_min(TimeGenerated, *) by Computer
```

### Pipe-order lesson

**Accounts whose latest overall event was a logon:**

```kql
SecurityEvent_CL
| summarize arg_max(TimeGenerated, *) by Account_s
| where EventID_s == 4624
```

**Latest successful logon for every account that logged on:**

```kql
SecurityEvent_CL
| where EventID_s == 4624
| summarize arg_max(TimeGenerated, *) by Account_s
```

> Filter before `summarize` when looking for the latest event of a specific type.

***

## Lists and Sets

### Include duplicates: `make_list()`

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s == 4624
| summarize make_list(Account_s) by Computer
```

### Unique values: `make_set()`

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s == 4624
| summarize make_set(Account_s) by Computer
```

***

## Visualizations

### Column chart

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| summarize count() by Account_s
| render columnchart
```

### Time-series chart

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| summarize count() by bin(TimeGenerated, 1m)
| render timechart
```

> `bin()` groups timestamps into fixed intervals.

***

## Multi-Table Queries

### `union`

Append rows from tables:

```kql
SecurityEvent_CL
| union SigninLogs_CL
```

Count all rows:

```kql
SecurityEvent_CL
| union SigninLogs_CL
| summarize count()
```

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
