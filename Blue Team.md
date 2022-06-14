# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

The following machines were identified on the network:

- Name of VM 1 **`Hyper V Host Manager`**  
  - **Operating System:** `Windows 10`  
  - **Purpose:** `Contains the vulnerable machines and the attacking machine`  
  - **IP Address:** `192.168.1.1`  
- Name of VM 2 **`Kali`**  
  - **Operating System:** `Linux 5.4.0`  
  - **Purpose:** `Used as attacking machine`  
  - **IP Address:** `192.168.1.90`  
- Name of VM 3 **`Capstone`**  
  - **Operating System:** `Linux (Ubuntu 18.04.1 LTS)`  
  - **Purpose:** `Used as a testing system for alerts`  
  - **IP Address:** `192.168.1.100`  
- Name of VM 4 **`ELK`**  
  - **Operating System:** `Linux (Ubuntu 18.04.1 LTS)`  
  - **Purpose:** `Used for gathering information from the victim machine using Metricbeat, Filebeats, and Packetbeats`  
  - **IP Address:** `192.168.1.100`  
- Name of VM 5 **`Target 1`**  
  - **Operating System:** `Linux 3.2 - 4.9`  
  - **Purpose:** `The VM with WordPress as a vulnerable server`  
  - **IP Address:** `192.168.1.110`  
- Name of VM 6 **`Target 2`**  
  - **Operating System:** `Linux 3.2 - 4.9`  
  - **Purpose:** `The VM with WordPress as a vulnerable server`  
  - **IP Address:** `192.168.1.115` 

### Description of Targets

The target of this attack was: `Target 1` (192.168.1.110).

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors

Alert 1 is implemented as follows:
  - **Metric**: `When count() Grouped Over top 5 http.response.status_code`
  - **Threshold**: `IS ABOVE 400 FOR THE LAST 5 minutes`
  - **Vulnerability Mitigated**: `This could help detect brute force attacks.`
  - **Reliability**: `High: Provides critical data against Brute Force Attacks`

#### HTTP Request Size Monitor

Alert 2 is implemented as follows:
  - **Metric**: `WHEN sum() of http.request.bytes OVER all documents`
  - **Threshold**: `IS ABOVE 3500 FOR THE LAST 1 minute`
  - **Vulnerability Mitigated**: `DDOS attacks`
  - **Reliability**: `Meduim: Based on volume of network traffic, this should not trigger many flase positives.`

#### CPU Usage Monitor

Alert 3 is implemented as follows:
  - **Metric**: `WHEN max() OF system.process.cpu.total.pct OVER all documents`
  - **Threshold**: `IS ABOVE 0.5 FOR THE LAST 5 minutes`
  - **Vulnerability Mitigated**: `It will trigger a memory alert only if the CPU remains at or above 50% consistently for 5 minutes. Virus or Malware`
  - **Reliability**: `Low: This alert can generate a lot of false positives due to CPU spikes occurring when other cpu intensive programs run.`



