# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/steveabner/Cloud-SOC/assets/164390231/779f6388-6115-4713-8068-8d711c8195c6)

## Introduction

In this project, I set up a mini honeynet in Azure and collected log data from various sources into a Log Analytics workspace. Microsoft Sentinel utilizes this workspace to create attack maps, trigger alerts, and generate incidents. I recorded security metrics in the unsecured environment for 24 hours, implemented security controls to enhance the environment, measured metrics for another 24 hours, and then presented the results below. The metrics include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into the honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/steveabner/Cloud-SOC/assets/164390231/2ad2e3bb-bda6-4e37-93ea-87a90520befb)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/steveabner/Cloud-SOC/assets/164390231/b2b82fae-ad05-458a-9ebc-930f17e4a22b)

The mini honeynet architecture in Azure comprises the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were initially deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls fully open, and all other resources were accessible through public endpoints, with no use of Private Endpoints.

For the "AFTER" metrics, the Network Security Groups were tightened by blocking all traffic except from my admin workstation, and all other resources were secured with their built-in firewalls and Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/steveabner/Cloud-SOC/assets/164390231/65a62138-0290-4d11-81ed-bfbfdf370d85)<br>
![Linux Syslog Auth Failures](https://github.com/steveabner/Cloud-SOC/assets/164390231/e61e0ad4-4933-4f9f-bc5e-1b25aa1c9de3)<br>
![Windows RDP/SMB Auth Failures](https://github.com/steveabner/Cloud-SOC/assets/164390231/4b10bbfd-b4fa-4187-a262-b677d17c8cb6)<br>

## Metrics Before Hardening / Security Controls

The table below presents the metrics measured in my insecure environment over a 24-hour period:

- Start Time 2024-06-09 15:38:45
- Stop Time 2024-06-10 15:38:45

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19610
| Syslog                   | 7154
| SecurityAlert            | 1
| SecurityIncident         | 145
| AzureNetworkAnalytics_CL | 103

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The table below displays the metrics measured in my environment over a 24-hour period following the implementation of security controls:

- Start Time 2024-06-13 20:07:50
- Stop Time 2024-06-14 20:07:50

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8767
| Syslog                   | 6
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was set up in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents based on the ingested logs. Metrics were recorded in the insecure environment before applying security controls and then again after implementing them. The significant reduction in security events and incidents after applying the security controls highlighted their effectiveness.

It is important to mention that if the resources within the network had been heavily utilized by regular users, more security events and alerts might have been generated within the 24-hour period following the implementation of the security controls.
