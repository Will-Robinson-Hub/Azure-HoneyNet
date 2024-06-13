# Building a SOC + Honeynet in Azure (Live Traffic)
![2024-06-01 01_49_03-honeynet](https://github.com/Will-Robinson-Hub/Azure-HoneyNet/assets/172574754/368068d7-4c05-447e-8ff2-f8a80cc79a1c)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Public Int1 Open](https://github.com/Will-Robinson-Hub/Azure-HoneyNet/assets/172574754/818317b5-ac47-49c0-b873-56459c9b8951)

## Architecture After Hardening / Security Controls
![network Int1 close](https://github.com/Will-Robinson-Hub/Azure-HoneyNet/assets/172574754/0c0de43e-d438-40cd-b871-e45a1a317505)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
 ![NSG Allowed Malicious Inbound Flows 2024-06-01 01_29_44-Window](https://github.com/Will-Robinson-Hub/Azure-HoneyNet/assets/172574754/3c979822-9665-4bb1-989e-3e0c278db63c)<br>
 ![LinuxSSHFail2024-06-01 01_31_46-Window](https://github.com/Will-Robinson-Hub/Azure-HoneyNet/assets/172574754/a4d48366-39c2-4091-b31c-44beea822f20)<br>
 ![MS SQL Server Authentication Failures 2024-06-01 01_25_28-Window](https://github.com/Will-Robinson-Hub/Azure-HoneyNet/assets/172574754/956ef5ea-9e4c-4f43-9ab0-ec1f5a5d999f)<br>
 ![Windows RDP_SMB Authentication Failures1 2024-06-01 01_34_07-Window](https://github.com/Will-Robinson-Hub/Azure-HoneyNet/assets/172574754/93a0b993-ca83-4198-a90b-9112ecf76406)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-05-31 11:06
Stop Time 2024-06-01 11:06

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15668
| Syslog                   | 1217
| SecurityAlert            | 5
| SecurityIncident         | 296
| AzureNetworkAnalytics_CL | 2084

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-06-04 16:29
Stop Time	2024-06-05 16:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9212
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
