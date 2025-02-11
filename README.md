# Building a SOC + Honeynet in Azure (Live Traffic)
![Hondeynet_SOC](https://github.com/user-attachments/assets/0cb7852b-25f5-4b8b-8e67-68d69caff01b)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Open_NSG](https://github.com/user-attachments/assets/7d6f9f16-1da8-45ae-90aa-b7e1f458f789)


## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

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
<img width="1180" alt="(before)-linux-ssh-auth-fail" src="https://github.com/user-attachments/assets/007b8882-163c-46d4-9161-e911b544dc57" /><br>
<img width="1192" alt="(before)-nsg-malicious-allowed-in" src="https://github.com/user-attachments/assets/bd286516-3b45-4b5b-8072-839405cae20e" /><br>
<img width="1230" alt="(before)-windows-rdp-auth-fail" src="https://github.com/user-attachments/assets/d410b452-8bf1-4cc6-8b14-31b2058ac796" /><br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-12-11 18:51
Stop Time 2024-12-12 19:04

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15847
| Syslog                   | 19713
| SecurityAlert            | 0
| SecurityIncident         | 169
| AzureNetworkAnalytics_CL | 4713

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-12-13 17:48
Stop Time	2024-12-14 18:17

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 1287
| Syslog                   | 205
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
