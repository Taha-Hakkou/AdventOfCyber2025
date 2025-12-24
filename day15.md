# day 15 (Web Attack forensics)

* use Splunk to pivot between web (Apache) logs and host-level (Sysmon) telemetry.

* Splunk: (Apache logs)
    - time range
    - View: table/list/raw

1. Suspicious web commands:
    - signs of command execution attempts, such as cmd.exe, PowerShell, or Invoke-Expression.
    - An attacker may try to execute system commands through a vulnerable CGI script.
    - example:
    ```splunk
    index=windows_apache_access (cmd.exe OR powershell OR "powershell.exe" OR "Invoke-Expression") | table _time host clientip uri_path uri_query status
    ```

2. Server-Side Errors or Command Execution:
    - If a request triggers too many “Internal Server Error,” it often means the attacker’s input was processed by the server but failed during execution, a key sign of exploitation attempts.
    - example:
    ```splunk
    index=windows_apache_error ("cmd.exe" OR "powershell" OR "Internal Server Error")
    ```

3. Suspicious Process Creation From Apache:
    - Typically, Apache should only spawn worker threads, not system processes like cmd.exe or powershell.exe. If results show child processes such as:
        * ParentImage = C:\Apache24\bin\httpd.exe
        * Image        = C:\Windows\System32\cmd.exe
    - It indicates a successful command injection where Apache executed a system command. which is one of the strongest indicators that the web attack penetrated the operating system.
    - example: (focuses on process relationships from Sysmon logs...)
    ```splunk
    index=windows_sysmon ParentImage="*httpd.exe"
    ```

4. Attacker Enumeration Activity:
    - filter:
    ```splunk
    index=windows_sysmon *cmd.exe* *whoami*
    ```
    - post-exploitation reconnaissance: Attackers often use the whoami command immediately after gaining code execution to determine which user account their malicious process is running as.

5. Base64-Encoded PowerShell Payloads:
    - filter:
    ```splunk
    index=windows_sysmon Image="*powershell.exe" (CommandLine="*enc*" OR CommandLine="*-EncodedCommand*" OR CommandLine="*Base64*")
    ```