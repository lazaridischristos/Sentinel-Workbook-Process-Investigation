<p align="center">
  <img src="https://img.shields.io/badge/Platform-Microsoft%20Sentinel-0078D4?style=for-the-badge&logo=microsoft&logoColor=white" alt="Microsoft Sentinel"/>
  <img src="https://img.shields.io/badge/Language-KQL-742774?style=for-the-badge" alt="KQL"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License"/>
</p>
Process Investigation workbooks is a collection of Microsoft Sentinel workbooks that act as a **log-based process explorer**. Unlike traditional tools such as Sysinternals Process Explorer that require live access to a running system, those workbooks reverse-engineer process trees, parent-child relationships, and execution chains entirely from log telemetry already collected in your Sentinel workspace.

## Data Source
This workbook supports the following data source:

| Data Source | Log Tables | Key Fields  |
|:-------------:|:-----------:|:---------------------------------------------:|
| Defender for Endpoint | DeviceProcessEvents | ProcessCommandLine, LogonId, DeviceName, ProcessUniqueId, InitiatingProcessUniqueId |
| Defender for Endpoint | DeviceNetworkEvents | InitiatingProcessUniqueId, DeviceName, LocalIP, LocalPort, RemoteIP, RemotePort, RemoteUrl |
| Defender for Endpoint | DeviceFileEvents | InitiatingProcessUniqueId, DeviceName, ActionType ,FolderPath, FileName, FileSize, SHA1, FileOriginUrl | 
| Defender for Endpoint | DeviceLogonEvents | DeviceName, LogonId, ActionType, AdditionalFields, IsLocalAdmin, LogonType, Protocol, RemoteIP, InitiatingProcessCommandLine |
| Defender for Endpoint | DeviceImageLoadEvents | InitiatingProcessUniqueId, DeviceName, FolderPath, FileSize, SHA1 |
| Defender for Endpoint | DeviceEvents | InitiatingProcessUniqueId, DeviceName, ActionType |
| Defender for Endpoint | DeviceRegistryEvents | InitiatingProcessUniqueId, DeviceName, ActionType, RegistryValueType, RegistryKey, RegistryValueName, RegistryValueData |

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
| **IOC Hunting** | Filter for processes and machines associated to a specific string searched. |
| **Process Enumeration** | Lists all observed processes across selected machines within a time window. |
| **Process & Machine filtering** | Select a specific machine to see all the live sessions associated with it. <br> Additionally, filter for a specific process file name to identify the machines where it was executed. This filtering affects also the live sessions found. |
| **Parent-Child Relationships** | Reconstructs which process spawned which, building a process tree from log entries. |
| **Per-process activity** | Selecting a process in the process tree, reveals other activities occuring on host due to this process e.g. Network Connections.|
| **Logon Activity** | Selecting a session, it will display the logon activity that it is responsible for creating this session.|
| **Other activity**  | It will display information associated with the **session activity** that is not correlated with a specific process and **device activity** that it is not correlated with any session. |

## Click below to install it!!

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Flazaridischristos%2FSentinel-Workbook-Process-Investigation%2Frefs%2Fheads%2Fmain%2FDefender%2520for%2520Endpoint%2520-%2520Process%2520Investigation%2FProcess%2520Investigation%2520-%2520Defender%2520for%2520Endpoint.json)
