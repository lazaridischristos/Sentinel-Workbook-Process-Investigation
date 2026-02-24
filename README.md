# Sentinel-Workbookx-Process-Investigation

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Microsoft%20Sentinel-0078D4?style=for-the-badge&logo=microsoft&logoColor=white" alt="Microsoft Sentinel"/>
  <img src="https://img.shields.io/badge/Language-KQL-742774?style=for-the-badge" alt="KQL"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License"/>
</p>
Process Investigation workbooks is a collection of Microsoft Sentinel workbooks that act as a **log-based process explorer**. Unlike traditional tools such as Sysinternals Process Explorer that require live access to a running system, those workbooks reverse-engineer process trees, parent-child relationships, and execution chains entirely from log telemetry already collected in your Sentinel workspace.

## Data Sources
This workbook supports the following data sources:

| Data Sources | Type of Events (Rendered Description) | Key Fields  |
|:-------------:|:-----------:|:-------------:|
| Sysmon (Event)   | Process Create <br> Network connection detected, Dns query <br> File created, File Executable Detected, File creation time changed, File Delete logged, File Delete archived <br> Image loaded <br> Registry value set, Registry object added or deleted <br> Process accessed <br> Other Activity | CommandLine, LogonId, ProcessGuid, ParentProcessGuid, Computer <br> SourceIp, SourcePort, DestinationIp, DestinationPort, QueryName, QueryResults <br>  |
| Security Events | Logon Events (4624 & 4672) <br> Other Events | test |


## Prerequisites
- A Microsoft Sentinel workspace
- Microsoft Defender for Endpoint P2 licence for each endpoint monitored
- Microsoft Defender for Endpoint Server licence for each server monitored
- Active machines sending telemetry (Defender for Endpoint onboarded)
- Microsoft Defender XDR Connector enabled on Sentinel with device telemetry ingested to Log Analytics workspace

## How It Works
This workbook queries Microsoft Defender for Endpoint telemetry to:

| Step | What it does |
|------|-------------|
| **IOC Hunting** | Filter for processes and machines associated to the IOC searched. |
| **Process Enumeration** | Lists all observed processes across selected machines within a time window. |
| **Process & Machine filtering** | Select a specific machine to see all the live sessions associated with it. <br> Additionally, filter for a specific process file name to identify the machines where it was executed. This filtering affects also the live sessions found. |
| **Parent-Child Relationships** | Reconstructs which process spawned which, building a process tree from log entries. |
| **Per-process activity** | Selecting a process in the process tree, reveals other activities occuring on host due to this process e.g. Network Connections.|
| **Logon Activity** | Selecting a session, it will display the logon activity that it is responsible for creating this session.|
| **Other activity**  | It will display information associated with the **session activity** that is not correlated with a specific process and **device activity** that it is not correlated with any session. |

