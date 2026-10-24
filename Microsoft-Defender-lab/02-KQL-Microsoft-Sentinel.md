# Learning Path 5 — Lab 1, Exercise 1  
## Microsoft Sentinel KQL Queries

## Purpose

Use **KQL** in Microsoft Defender XDR Advanced Hunting to search, filter, aggregate, visualize, correlate, and parse security logs.

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

### Broad text search

```kql
search "Computer"
```

```kql
search in (SecurityEvent_CL, App*) "new"
```

> `search` is useful for exploration but less efficient than filtering specific tables and columns.

### Filter with `where`

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
```

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s == 4624
```

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s == 4624
| where AccountType_s =~ "user"
```

```kql
SecurityEvent_CL
| where TimeGenerated > ago(5d)
| where EventID_s in (4624, 4625)
```

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
