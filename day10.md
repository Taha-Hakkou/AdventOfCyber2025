# Day 10 (Alert Triage)

* Triaging helps security teams identify which alerts deserve immediate attention, which can be deprioritised, and which can be safely ignored for a moment.

* fundamental factors of alert triaging:
    - Severity Level
    - Timestamp and Frequency (Helps identify ongoing attacks or patterns of repeated behaviour)
    - Attack Stage: Determine which stage of the attack lifecycle this alert indicates (reconnaissance, persistence, or data exfiltration)
    - Affected Asset: Identify the system, user, or resource involved and assess its importance to operations.

* Diving Deeper into an Alert
    - Investigate the alert in detail.
    - Check the related logs.
    - Correlate multiple alerts.
    - Build context and a timeline.
    - Decide on the following action.
    - Document findings and lessons learned.

* Microsoft Sentinel, a cloud-native SIEM and SOAR platform. Sentinel collects data from various Azure services, applications, and connected sources to detect, investigate, and respond to threats in real time.


## Practical (MS Azure):

Lab environement logs rendering:  
Search 'Microsoft Sentinel' > click on sentinel instance > Logs > Tables > Custom Logs (Run: Syslog_CL)
time range: include '10/12/2025' as its the date of the challenge


sentinel instance > Threat Management (Incidents)
When multiple alerts are linked to a single entity, such as the same machine, user, or IP address, it typically indicates that these detections are not isolated incidents, but somewhat different stages of the same intrusion.


* In-Depth Log Analysis (Sentinel):
The next step involves diving into the underlying log data within Microsoft Sentinel to validate the alerts and uncover the exact attacker actions that triggered them
Evidence > Events (or in MS Defender: Attack story > Devices)
manipulate KQL query: KQL Mode (or in MS Defender: view query) > customize query > Run
Example:
```KQL
set query_now = datetime(2025-12-12T00:08:52.0545899Z);
Syslog_CL | where host_s == 'websrv-01' | project TimeGenerated, host_s, Message
```