## day 22

* __Real Intelligence Threat Analytics (RITA)__ is an open-source framework created by Active Countermeasures.
* Its core functionality is to detect command and control (C2) communication by analyzing network traffic captures and logs.
* Its primary features are:
    - C2 beacon detection
    - DNS tunneling detection
    - Long connection detection
    - Data exfiltration detection
    - Checking threat intel feeds
    - Score connections by severity
    - Show the number of hosts communicating with a specific external IP
    - Shows the datetime when the external host was first seen on the network

* The magic behind RITA is its analytics. It correlates several captured fields, including IP addresses, ports, timestamps, and connection durations, among others. Based on the normalized and correlated dataset, RITA runs several analysis modules collecting information like:
    - Periodic connection intervals
    - Excessive number of DNS queries
    - Long FQDN
    - Random subdomains
    - Volume of data over time over HTTPS, DNS, or non-standard ports
    - Self-signed or short-lived certificates
    - Known malicious IPs by cross-referencing with public threat intel feeds or blocklists

* RITA only accepts network traffic input as __Zeek__ logs.
* Zeek is an open-source network security monitoring (NSM) tool. Zeek is not a firewall or IPS/IDS; it does not use signatures or specific rules to take an action. It simply observes network traffic via configured SPAN ports (used to copy traffic from one port to another for monitoring), physical network taps, or imported packet captures in the PCAP format. Zeek then analyzes and converts this input into a structured, enriched output. This output can be used in incident detection and response, as well as threat hunting.
* Out of the box, Zeek covers two of the four types of NSM data:
    - transaction data (summarized records of application-layer transactions).
    - extracted content data (files or artifacts extracted, such as executables).

### practical

1. Convert pcap to zeek logs:
    * Zeek can convert packet captures (PCAPs) into structured logs
    * [Bradly Duncan's blog](https://malware-traffic-analysis.net/) contains a wonderful collection of malware-related PCAPs that cover real-world threats.
    * Command:
    ```sh
    zeek readpcap <pcapfile> <outputdirectory>
    ```
    * For using RITA, we don't really need to know what is in these logs (although the names are quite self-descriptive); however, you can find more info at ["zeek logs"](https://docs.zeek.org/en/master/logs/index.html)

2. RITA analysis:
    * Command:
    ```sh
    rita import --logs <logspath> --database <dbname>
    rita view <database-name>
    ```
    * RITA will parse and analyze the imported logs (lot of output is produced).
    * Larger datasets will provide more insights than smaller ones. Smaller datasets are also more prone to false positive entries.
    * The terminal window shows three elements:
        - Search bar:
            + forward slash (/) -> search
            + Esc -> exit search functionality
            + ? -> help (searhc fields)
            + ? again -> exit help page
        - Results pane:
            + _Severity_: A score calculated based on the results of threat modifiers (discussed below)
            + _Source and destination_: IP/FQDN (Fully Qualified Domain Name)
            + _Beacon_ likelihood
            + _Duration of the connection_: Long connections can be indicators of compromise. Most application layer protocols are stateless and close the connection quickly after exchanging data (exceptions are SSH, RDP, and VNC).
            + _Subdomains_: Connections to subdomains with the same domain name. If there are many subdomains, it could indicate the use of a C2 beacon or other techniques for data exfiltration.
            + _Threat intel_: lists any matches on threat intel feeds
        - Details pane:
            + Threat Modifiers (criteria to determine the severity and likelihood of a potential threat):
                * __MIME type/URI mismatch__: Flags connections where the MIME type reported in the HTTP header doesn't match the URI. This can indicate an attacker is trying to trick the browser or a security tool.
                * __Rare signature__: Points to unusual patterns that attackers might overlook, such as a unique user agent string that is not seen in any other connections on the network.
                * __Prevalence__: Analyzes the number of internal hosts communicating with a specific external host. A low percentage of internal hosts communicating with an external one can be suspicious.
                * __First Seen__: Checks the date an external host was first observed on the network. A new host on the network is more likely to be a potential threat.
                * __Missing host header__: Identifies HTTP connections that are missing the host header, which is often an oversight by attackers or a sign of a misconfigured system.
                * __Large amount of outgoing data__: Flags connections that send a very large amount of data out from the network.
                * __No direct connections__: Flags connections that don't have any direct connections, which can be a sign of a more complex or hidden command and control communication.
            + Connection Info (connections' metadata and basic connection info)
                * __Connection count__: Shows the number of connections initiated between the source and destination. A very high number can be an indicator of C2 beacon activity.
                * __Total bytes sent__: Displays the total amount of bytes sent from source to destination. If this is a very high number, it could be an indication of data exfiltration.
                * __Port number - Protocol - Service__: If the port number is non-standard, it warrants further investigation. The lack of SSL in the Service info could also be an indicator that warrants further investigation.
    
    * the results that are displayed warrant some attention. Even if the entry does not have a high severity score, it can still be an indicator of compromise.
    * Indicators of compromise:
        - low 'first seen'
        - long FQDN (search in VirusTotal for malicious URLs)
        - rare signature: Malware or C2 connections often create unique TLS handshake patterns that differ from those of browsers and legitimate clients.
    * Be cautious, as some of the PCAPs may contain malicious files, domains, and IPs that are still in use!
