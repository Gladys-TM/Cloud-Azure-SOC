# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/Gladys-TM/Cloud-Azure-SOC/blob/main/Honeynet%20proj.jpg)

## Introduction
In this project, I constructed a miniature honeynet within the Azure environment. I aggregated log data from diverse sources into a Log Analytics workspace. Microsoft Sentinel then utilized this workspace to craft attack maps, trigger alerts, and generate incidents. The process involved measuring specific security metrics within an initially insecure environment over a 24-hour period. Subsequently, security controls were implemented to fortify the environment, and a further 24-hour metric measurement was conducted. The outcomes are presented below, encompassing the following metrics:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/Gladys-TM/Cloud-Azure-SOC/blob/main/Unsecured%20NSGs.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/Gladys-TM/Cloud-Azure-SOC/blob/main/Secure%20NSGs.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In the initial "BEFORE" metrics phase, all resources were initially deployed with direct exposure to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls configured with unrestricted access. Additionally, all other resources were deployed with public endpoints visible to the internet, without leveraging Private Endpoints.

During the subsequent "AFTER" metrics phase, significant security enhancements were implemented. Network Security Groups were hardened by blocking ALL traffic, with the sole exception being traffic originating from my designated admin workstation. Furthermore, all remaining resources were safeguarded by their built-in firewalls and additionally benefited from the enhanced security provided by Private Endpoints.




## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/Gladys-TM/Cloud-Azure-SOC/blob/main/nsg-malicious-allowed-in-before.jpg)<br>
![Linux Syslog Auth Failures](https://github.com/Gladys-TM/Cloud-Azure-SOC/blob/main/linux-ssh-auth-fail-before.jpg)<br>
![Windows RDP/SMB Auth Failures](https://github.com/Gladys-TM/Cloud-Azure-SOC/blob/main/windows-rdp-auth-fail-before.jpg)<br>
![MSSQL Server Auth Failures](https://github.com/Gladys-TM/Cloud-Azure-SOC/blob/main/mssql-auth-fail-before.jpg)<br>

## Metrics Before Hardening / Security Controls

The table below outlines the metrics evaluated within our insecure environment over a 24-hour period:

Start Time 2024-02-09 01:59

Stop Time 2024-02-10  01:59

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 18706
| Syslog                   | 8551
| SecurityAlert            | 1
| SecurityIncident         | 166
| AzureNetworkAnalytics_CL | 2687

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The presented table illustrates the metrics assessed within our environment over an additional 24-hour duration. These measurements were taken subsequent to the implementation of enhanced security controls:

Start Time 2024-02-14 05:21

Stop Time	2024-02-15  05:21

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8673
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a miniature honeynet was established in Microsoft Azure, and log sources were seamlessly integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and generating incidents based on the ingested logs. Furthermore, metrics were gauged in the insecure environment before the implementation of security controls, and then subsequently, after the implementation of security measures. It is noteworthy that the number of security events and incidents observed a substantial reduction after the application of these security controls, underscoring their effectiveness.

Importantly, it should be acknowledged that if the network's resources were extensively utilized by regular users, there might have been a potential for a higher volume of security events and alerts during the 24-hour period following the enforcement of the security controls.
