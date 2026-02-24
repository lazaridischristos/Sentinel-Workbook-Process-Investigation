<p align="center">
  <img src="https://img.shields.io/badge/Platform-Microsoft%20Sentinel-0078D4?style=for-the-badge&logo=microsoft&logoColor=white" alt="Microsoft Sentinel"/>
  <img src="https://img.shields.io/badge/Language-KQL-742774?style=for-the-badge" alt="KQL"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License"/>
</p>
Process Investigation workbooks is a collection of Microsoft Sentinel workbooks that act as a **log-based process explorer**. Unlike traditional tools such as Sysinternals Process Explorer that require live access to a running system, those workbooks reverse-engineer process trees, parent-child relationships, and execution chains entirely from log telemetry already collected in your Sentinel workspace.

## Data Sources
This workbook works using the following data sources. The Key Fields are important for the correlation of the different types of events or to display some important information. All the fields per event can be viewed through links.

| Data Source | Type of Events (Rendered Description) | Key Fields  |
|:-------------:|:-----------:|:-------------:|
| Sysmon (Event)   | Process Create  | CommandLine, LogonId, ProcessGuid, ParentProcessGuid, Computer  |
| Sysmon (Event) | Network connection detected, Dns query | SourceIp, SourcePort, DestinationIp, DestinationPort, QueryName, QueryResults, ProcessGuid, Computer |
| Sysmon (Event) | File created, File Executable Detected, File creation time changed, File Delete logged, File Delete archived | ProcessGuid, Computer, TargetFilename, SHA256 |
| Sysmon (Event) | Image loaded | ImageLoaded, Signed, SignatureStatus, OriginalFileName, ProcessGuid, Computer|
| Sysmon (Event) | Registry value set, Registry object added or deleted | TargetObject, Details, ProcessGuid, Computer |
| Sysmon (Event) | Process accessed | SourceProcessGuid, Computer, TargetImage, TargetUser, TargetProcessId |
| Security Events | Logon Events (4624 & 4672) | TargetLogonId (4624), SubjectLogonId (4672), Computer |
| Security Events | Other Events | TargetLogonId, SubjectLogonId, LogonID, Computer |


## Prerequisites
- A Microsoft Sentinel workspace
- Sysmon installed on monitored endpoints. (The configuration that was used, was created by using Olaf Hartong's [sysmon-modular](https://github.com/olafhartong/sysmon-modular), and can be found [here](./sysmonconfig-excludes-only-custom-includes.xml).) <br> **DISCLAIMER:** The Sysmon configuration file is very important because limited telemetry will reduce the number of events and 'brake' process trees.
- Availability of endpoints in Azure. (Azure Virtual Machines or Arc-enabled machines)
- Monitoring extention installed on endpoints.
- Data collection rule that sends Sysmon events to Sentinel. (XPath query: Microsoft-Windows-Sysmon/Operational!*)
- Data collection rule that sends Windows Security events to Sentinel. (Data Connector: Windows Security Events via AMA)
- Sysmon parser (KQL Function) saved on Sentinel, which can be found [here](./Sysmon-Parser.txt). <br>
**IMPORTANT:** The Sysmon parser is using 3 variables for better query efficiency. Save the parser with the name and variables as it appears in the below screenshot.

  <img width="573" height="545" alt="Sysmon Parser Name and variables" src="https://github.com/user-attachments/assets/a0ab7a1a-27ce-4cf0-8a7c-768f0530e10f" />


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

