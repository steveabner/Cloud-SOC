# Building a SOC + Honeynet in Azure (Live Traffic)
![Untitled drawing (3)](https://github.com/user-attachments/assets/cb551476-6f70-4eb5-bf22-0be793cb1038)


## Introduction

In this project, I set up a mini honeynet in Azure and collected log data from various sources into a Log Analytics workspace. Microsoft Sentinel utilizes this workspace to create attack maps, trigger alerts, and generate incidents. I recorded security metrics in the unsecured environment for 24 hours, implemented security controls to enhance the environment, measured metrics for another 24 hours, and then presented the results below. The metrics include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into the honeynet)

## Architecture Before Hardening / Security Controls
![Completed Before Hardening](https://github.com/user-attachments/assets/6b38b28c-e97b-4ebb-99c8-f14892272a04)

The architecture of the mini honeynet in Azure includes several key components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (1 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

Initially, all resources were deployed with public access, meaning the Virtual Machines had their NSGs and built-in firewalls fully open, exposing them to the internet. Other resources also had public endpoints visible, which eliminated the need for Private Endpoints.

After implementing security improvements, the NSGs were significantly hardened by restricting all traffic except from the designated admin workstation. Additionally, all resources are now secured by their built-in firewalls and utilize Private Endpoints to enhance protection against external threats. This ultimate hardening process greatly reduces the attack surface and enhances overall security.

## Attack Maps Before Hardening / Security Controls

NSG Allowed Inbound Malicious Flows
![(before)nsg-allowed-in](https://github.com/user-attachments/assets/521f4940-9de4-468a-b4c6-d7c2a754c28b)

Linux Syslog Auth Failures
![(before)linux-ssh-auth-fail](https://github.com/user-attachments/assets/c3efbb75-95e0-4ffe-9282-067be74f97f1)

Windows RDP/SMB Auth Failures
![(before)windows-rdp-auth-fail](https://github.com/user-attachments/assets/6fb158c9-17ed-4ecd-8bb9-b05b76df1734)

MSSQL Auth Failures
![(before)mssql-auth-fail](https://github.com/user-attachments/assets/26210ced-3fe1-4671-a6cd-8bca3046b6be)

<!-- ![NSG Allowed Inbound Malicious Flows](https://github.com/steveabner/Cloud-SOC/assets/164390231/65a62138-0290-4d11-81ed-bfbfdf370d85)<br> -->
<!-- ![Linux Syslog Auth Failures](https://github.com/steveabner/Cloud-SOC/assets/164390231/e61e0ad4-4933-4f9f-bc5e-1b25aa1c9de3)<br> -->
<!-- ![Windows RDP/SMB Auth Failures](https://github.com/steveabner/Cloud-SOC/assets/164390231/4b10bbfd-b4fa-4187-a262-b677d17c8cb6)<br> -->

## Metrics Before Hardening / Security Controls

The table below presents the metrics measured in my insecure environment over a 24-hour period:

- Start Time 9/23/2024, 1:35PM
- Stop Time 9/24/2024, 1:35PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 82091
| Syslog                   | 22019
| SecurityAlert            | 10
| SecurityIncident         | 282
| AzureNetworkAnalytics_CL | 3553

## Architecture After Hardening / Security Controls
![Honeynet After Hardening](https://github.com/user-attachments/assets/78ff760f-7f42-42a7-b18b-55ab8a3b03f7)

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The table below displays the metrics measured in my environment over a 24-hour period following the implementation of security controls:

- Start Time 9/25/2024, 9:57AM
- Stop Time 9/26/2024, 9:57AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 2798
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Overall Results

| Metric                                       | Change After Hardening
| -------------------------------------------- | -----
| SecurityEvent (Windows VMs)                  | 96.59%
| Syslog (Linux VMs)                           | 100%
| SecurityAlert (Microsoft Defender for Cloud) | 100%
| SecurityIncident (Sentinel Incidents)        | 100%
| AzureNetworkAnalytics_CL                     | 100%

## Conclusion

In this project, a mini honeynet was set up in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents based on the ingested logs. Metrics were recorded in the insecure environment before applying security controls and then again after implementing them. The significant reduction in security events and incidents after applying the security controls highlighted their effectiveness.

It is important to mention that if the resources within the network had been heavily utilized by regular users, more security events and alerts might have been generated within the 24-hour period following the implementation of the security controls.
