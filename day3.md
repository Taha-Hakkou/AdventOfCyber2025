# Day 3 (Splunk Basics)

* Use Search Processing Language (SPL) to filter and refine search results.

Search & Reporting

All time

```sh
> index=main # shows all ingested logs
```

* datasets that have been ingested into Splunk can be accessed on the 'sourcetype' field.

```sh
> index=main sourcetype=DATASET
```

* Visualizing the Logs Timeline:

```sh
> index=main sourcetype=web_traffic | timechart span=1d count
# total event count over time, grouped by day
```

```sh
index=main sourcetype=web_traffic | timechart span=1d count | sort by count | reverse
```

* Anomaly Detection:
    - checking 'user_agent'
    - checking 'client_ip'
    - checking 'path'

```sh
> sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox* | stats count by client_ip | sort -count | head 5
# sort -count: sorts in reverse order
```

* Tracing the Attack Chain (Example):
    - Reconnaissance (Footprinting)
    ```sh
    > sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("/.env", "/*phpinfo*", "/.git*") | table _time, path, user_agent, status
    ```
    - Enumeration (Vulnerability Testing)
    ```sh
    > sourcetype=web_traffic client_ip="<REDACTED>" AND path="*..\/..\/*" OR path="*redirect*" | stats count by path
    ```
    - SQL Injection Attack
    ```sh
    > sourcetype=web_traffic client_ip="<REDACTED>" AND user_agent IN ("*sqlmap*", "*Havij*") | table _time, path, status
    ```
    - Exfiltration Attempts -> Search for attempts to download large, sensitive files(backups, logs)
    ```sh
    > sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("*backup.zip*", "*logs.tar.gz*") | table _time path, user_agent
    ```
    - Ransomware Staging & RCE (web shell & Remote code Execution (RCE))
    ```sh
    sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("*bunnylock.bin*", "*shell.php?cmd=*") | table _time, path, user_agent, status
    ```
    - Correlate Outbound C2 Communication
    ```sh
    sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="<REDACTED>" AND action="ALLOWED" | table _time, action, protocol, src_ip, dest_ip, dest_port, reason # ACTION=ALLOWED and REASON=C2_CONTACT fields confirm  the server established an outbound connection and the malware communication channel was active.
    ```
    - Volume of Data Exfiltrated
    ```sh
    sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="<REDACTED>" AND action="ALLOWED" | stats sum(bytes_transferred) by src_ip
    ```