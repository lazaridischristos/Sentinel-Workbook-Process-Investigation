<p align="center">
  <img src="https://img.shields.io/badge/Platform-Microsoft%20Sentinel-0078D4?style=for-the-badge&logo=microsoft&logoColor=white" alt="Microsoft Sentinel"/>
  <img src="https://img.shields.io/badge/Language-KQL-742774?style=for-the-badge" alt="KQL"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License"/>
</p>
Process Investigation workbooks is a collection of Microsoft Sentinel workbooks that act as a **log-based process explorer**. Unlike traditional tools such as Sysinternals Process Explorer that require live access to a running system, those workbooks reverse-engineer process trees, parent-child relationships, and execution chains entirely from log telemetry already collected in your Sentinel workspace.

## How they Work
These workbook query Microsoft Sentinel to perform the below actions:

| Step | What it does |
|------|-------------|
| **IOC Hunting** | Filter for processes and machines associated to a specific string searched. |
| **Process Enumeration** | Lists all observed processes across selected machines within a time window. |
| **Process & Machine filtering** | Select a specific machine to see all the live sessions associated with it. <br> Additionally, filter for a specific process file name to identify the machines where it was executed. This filtering affects also the live sessions found. |
| **Parent-Child Relationships** | Reconstructs which process spawned which, building a process tree from log entries. |
| **Per-process activity** | Selecting a process in the process tree, reveals other activities occuring on host due to this process e.g. Network Connections.|
| **Logon Activity** | Selecting a session, it will display the logon activity that it is responsible for creating this session.|
| **Other activity**  | It will display information associated with the **session activity** that is not correlated with a specific process and **device activity** that it is not correlated with any session. |

## 🗂️ Repository Structure

```
Sentinel-Workbook-Process-Investigation/
|── 📄 README.md
|── 📄 LICENCE
|── 📁 Sysmon - Process Investigation/
    |── 📋 Process Investigation - Sysmon.json
    |── 📋 README.md
    |── 📋 Sysmon-Parser.txt
    └── 📋 sysmonconfig-excludes-only-custom-includes.xml
└── 📁 Defender for Endpoint - Process Investigation/
    |── 📋 Process Investigation - Defender for Endpoint.json
    └── 📋 README.md
```
