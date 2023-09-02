# Azure-SOC-Honeynet
# Building an SOC + Honeynet in Azure with Live Traffic
![Cloud Honeynet / SOC](https://camo.githubusercontent.com/34f1d947cc00bc943970a1206dec7bfda42540fe98df09c6bfaba03e150edf78/68747470733a2f2f692e696d6775722e636f6d2f4937727a67376c2e6a7067)

## Introduction

In this project, I build a honeynet in Azure and ingest logs from numerous resources into a Log Analytics workspace. I then use the Microsoft Sentinel SIEM to build attack maps, trigger alerts, create incidents, and handle incident response with severities ranging from low to high. I measure security metrics in the insecure environment for 24 hours, apply security controls including NIST 800-61 SC-7 to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics shown are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

## Failed Authentication / Brute Force Attempts
![bruteforce-attempts](https://github.com/quadicyber/Azure-SOC-Honeynet/assets/104177919/35e3ece2-ee4d-4d6f-8558-77f2b85741f1)

This example shows a live threat actor / entity attempting a brute-force attack.

## High Severity Incident Investigation/Remediation
![investigation-remediation](https://github.com/quadicyber/Azure-SOC-Honeynet/assets/104177919/44f1e890-137b-443a-b942-e65630eb649e)

Here is where I investigated a high-severity alert for Malware Detection.

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

When all the resources were originally deployed, they were exposed to the public Internet. The Network Security Group rules, as well as the VM's built-in firewalls, were manually set to allow any and all traffic. The other resources were deployed with public endpoints visible to the Internet (no private endpoints were used). Any security incidents, such as unauthenticated (failed) logins, were configured to be ingested by Azure's Log Analytics Workspace (LAW).


## Attack Maps Before Hardening / Security Controls
![nsg-before](https://github.com/quadicyber/Azure-SOC-Honeynet/assets/104177919/89155d59-9257-4dd6-93bc-5b0165b5264d)<br>
![syslog-before](https://github.com/quadicyber/Azure-SOC-Honeynet/assets/104177919/83c5417f-c643-4e28-a8ac-f1d7013581a3)<br>
![rdp-before](https://github.com/quadicyber/Azure-SOC-Honeynet/assets/104177919/23c65d29-509f-47d1-929f-ee8b6fe1dbbe)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in the insecure environment for 24 hours:

Start Time 2023-08-25 13:04:29
Stop Time 2023-08-26 13:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17117
| Syslog                   | 5012
| SecurityAlert            | 3
| SecurityIncident         | 402
| AzureNetworkAnalytics_CL | 2511


## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as private endpoints being enabled.

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in the environment for another 24 hours, but after having applied security controls:

Start Time 2023-08-29 15:37
Stop Time	2023-08-29 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10998
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Results Of Hardening / Security Controls
|                   | % Change After Hardening
| ------------------------ | -----
| SecurityEvent (Windows VMs)           | -35.81%
| Syslog (Linus VM)                  | -99.98%
| SecurityAlert (MDC)           | -100.00%
| SecurityIncident (Sentinel Incidents)        | -100.00%
| NSG Inbound Malicious Flows Allowed | -100.00%

## Conclusion

In this project, a honeynet was constructed in Microsoft Azure and logs from VMs were ingested into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

## Cost Analysis / Security Posture
![cost-analysis](https://github.com/quadicyber/Azure-SOC-Honeynet/assets/104177919/fa9e3145-0da2-436c-a489-401ef133caae)

Here is a breakdown of the cost analysis of this lab. For a little less than $30 USD, I was able to gain invaluable experience configuring a cloud infrastructure, observe live cyber attacks against it, and implement security controls to improve the security posture of the environment.

![post-hardening-graphs](https://github.com/quadicyber/Azure-SOC-Honeynet/assets/104177919/d354f1e9-74e4-43c1-b825-c9508ad42f09)

Here is the final security posture of the environment. A final posture of 77% is a large improvement over the 52% it was in the insecure environment. Regulatory compliance was also drastically increased.

