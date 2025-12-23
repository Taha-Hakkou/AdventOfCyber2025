### Day 6

* malware analysis branches:
- static: focuses on inspecting a file without executing it
- dynamic: involves executing it

* sandboxes (safe isolated envs): think of this as disposable digital play-pens
- VMs are a popular choice: snapshotting...

* connection to remote machine (sandbox) is gonna be via rdp
- to connect to windows from linux client via rdp:
```sh
sudo apt install remmina
```

* malicious code and apps are referred to as samples

* rules:
- never run dangerous apps on devices you care about

* tools of the trade: pestudio, procmon, regshot...

* informations gathered from static analysis:
- checksums: track and catalogue files/execs (can google it to check if already identified)
- strings: sequences of readable chars (IPs, URLs, commands, passwords...)
- imports: librairies, functions (OS...)
- resources: data (images...) -> malware is known to be hidden in this section

* you cannot truly know how a sample functions until it is executed
- attackers use techniques like "obfuscation" to obscure how a sample appears, to evade anti-viruses/analysts

* PeStudio (Usage): static analysis

- gathering information:
1. launch pestudio
2. load the executable
3. click in "indicators" tab in dropdown
4. look for sha256sum (unique identifier for the executable: keep note of it)
5. reviewing strings ("strings" indicator on left panel)

* RegShot (Usage): dynamic analysis (on Windows)

- creates 2 snapshots of the registry before and after running the sample, and compares them.

- malware aims to establish persistense (seeks to run as soon as the device is switched on)
-> common technique: adds 'Run' key to the registry ('Run' is frequently used to specify startup apps)

- Steps:
1. launch regshot
2. log as 'html doc'
3. path as 'Desktop'
4. 1st shot > Shot
5. execute the sample
6. 2nd shot > Shot
7. compare (use 'txt', 'html' is not working properly (corrupted))

* ProcMon (Usage): dynamic analysis

- from sysinternals suite

- for monitoring and investigating how processes are interacting with windowsOS (allows to see exacly what a process is doing)

- Steps:
1. launch procmon
2. execute the sample
3. stop capturing more events
4. filter > filter (process name + is + "HopHelper.exe")
5. Note: operation of interest: RegOpenKey, CreateFile, TCP Connect, TCP Receive
6. filter > filter (operation + contains + "TCP")
7. check for used network protocol in 'path' panel
8. filter > reset filter (to start over)

- Bonus:
using wireshar (loopback interface), i found that HopHelper.exe is communicating with a internet panel and shares collected system info !!!!


* DFIR tools folder !!!!!!!!!