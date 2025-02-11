# Building a SOC + Honeynet in Azure (Live Traffic)
![Hondeynet_SOC](https://github.com/user-attachments/assets/0cb7852b-25f5-4b8b-8e67-68d69caff01b)

## Introduction

In this project, I created a mini honeynet in Azure and collected log data from various sources into a Log Analytics workspace. Microsoft Sentinel uses this data to generate attack maps, trigger alerts, and create incidents. I tracked security metrics in an insecure environment for 24 hours, then applied security controls to strengthen it and measured the metrics again for another 24 hours. The metrics are shown below.
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Open_NSG](https://github.com/user-attachments/assets/7d6f9f16-1da8-45ae-90aa-b7e1f458f789)


## Architecture After Hardening / Security Controls
![Post_Harden](https://github.com/user-attachments/assets/a9c4dd69-2f90-4bae-bd2c-7aecd68dbaa9)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In the "BEFORE" setup, all resources were exposed to the internet with Virtual Machines having open Network Security Groups and firewalls, and other resources had public endpoints accessible(no need for private endpoints). 

In the "AFTER" setup, Network Security Groups were secured by blocking all traffic except from the admin workstation, and additional protection was added through firewalls(FW) and Private Endpoints(PE) for all resources.
## Attack Maps Before Hardening / Security Controls
<img width="1180" alt="(before)-linux-ssh-auth-fail" src="https://github.com/user-attachments/assets/007b8882-163c-46d4-9161-e911b544dc57" /><br>
<img width="1192" alt="(before)-nsg-malicious-allowed-in" src="https://github.com/user-attachments/assets/bd286516-3b45-4b5b-8072-839405cae20e" /><br>
<img width="1230" alt="(before)-windows-rdp-auth-fail" src="https://github.com/user-attachments/assets/d410b452-8bf1-4cc6-8b14-31b2058ac796" /><br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics measured in the insecure environment for 24 hours:

Start Time 12/11/2024 18:51
Stop Time 12/12/2024 19:04

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15847
| Syslog                   | 19713
| SecurityAlert            | 0
| SecurityIncident         | 169
| AzureNetworkAnalytics_CL | 4713

## Attack Maps After Hardening / Security Controls

```All map queries returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics  measured in the environment for another 24 hours, but after having applied security controls:

Start Time 12/13/2024 17:48
Stop Time	12/14/2024 18:17

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 1287
| Syslog                   | 205
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was set up in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was used to generate alerts and incidents from the logs. Metrics were collected before and after applying security controls in the insecure environment. The results showed a significant reduction in security events and incidents after implementing the controls, highlighting their effectiveness.

Additionally, if the network resources had been more heavily used by regular users, it’s likely that more security events and alerts would have occurred in the 24 hours following the implementation.
