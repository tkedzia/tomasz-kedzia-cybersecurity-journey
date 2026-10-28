# SC-200: Tools and Relationships

Workflow: Collect → Analyze → Detect → Investigate → Respond → Improve

| Stage | Tool / Capability | Core Purpose | Connects To |
|---|---|---|---|
| Protect endpoints | Defender for Endpoint | Detects suspicious device activity | Sends endpoint signals to Defender XDR |
| Correlate threats | Defender XDR | Combines related alerts and evidence into incidents | Integrates Microsoft security signals and works with Sentinel |
| Manage hybrid servers | Azure Arc | Connects non-Azure servers to Azure management | Enables the hybrid management path used for AMA |
| Collect logs | AMA + DCR + Data Connectors | AMA collects; DCR configures collection/routing; connectors integrate sources | Sends data into Sentinel tables |
| Centralize data | Microsoft Sentinel | Provides SIEM analytics, hunting, and response orchestration | Connects logs, detections, investigations, and automation |
| Query data | KQL / Advanced Hunting | Searches, filters, matches, and summarizes accessible data | Supports hunting, investigations, and detection logic |
| Add context | Watchlists + Threat Intelligence + UEBA | Adds asset importance, threat indicators, and behavioral context | Enriches analysis and prioritization |
| Detect threats | Analytics Rules | Automatically evaluates data for suspicious activity | Creates alerts and, when configured, incidents |
| Investigate | Incidents + Entity Views + Hunting Graph | Organizes evidence and relationships | Helps determine scope and response |
| Trigger actions | Automation Rules | Decides when to assign, tag, update, or run workflows | Invokes built-in actions or playbooks |
| Execute response | Playbooks / Azure Logic Apps | Performs configured response and enrichment workflows | Runs manually or through automation rules |
| Improve coverage | Hunts + MITRE ATT&CK + Data Lake KQL Jobs | Searches for missed threats and analyzes historical activity | Informs investigations and better detections |

## Key Distinction

**Defender XDR** correlates signals → Sentinel centralizes and analyzes →
KQL searches → Analytics rules detect → Automation rules trigger →
Playbooks execute.

The Defender portal is the shared interface; table availability still depends
on connected sources, workspace scope, and permissions.
