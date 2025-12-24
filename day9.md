# day 9

- Different file formats use different algorithms and key derivation methods. For example, PDF encryption and ZIP encryption differ in details (how the key is derived, salt use, number of hash iterations).

- Attackers don't usually try to "break" the encryption itself because that would take far too long with modern cryptography. Instead, they focus on guessing the password that protects the file. The two most common ways of doing this are dictionary attacks and brute-force (or mask) attacks.

- In a **dictionary attack**, the attacker uses a predefined list of potential passwords, known as a wordlist. It often contain leaked passwords from previous breaches, common substitutions like password123, predictable combinations of names and dates...

- Brute-force and mask attacks go one step further. A **brute-force attack** systematically tries every possible combination. While this guarantees success eventually, the time it takes grows exponentially.

- **Mask attacks** aim to reduce that time by limiting guesses to a specific format.

* Tips:
- Use GPU-accelerated cracking when possible; it dramatically speeds up attacks for some algorithms.
- Keep an eye on resource use: cracking is CPU/GPU intensive. That behaviour can be detected on a monitored endpoint.

* Practical:

1. file type confirmation
```sh
file <file>
# or open file with a  hex viewer
```

2. tool picking:
- PDF: pdfcrack, john (via pdf2john)
- ZIP: fcrackzip, john (via zip2john)
- General: john (very flexible) and hashcat (GPU acceleration, more advanced)

> Common lists: rockyou.txt, common-passwords.txt

3. dictionary attack:

- pdfcrack:
```sh
pdfcrack -f flag.pdf -w /usr/share/wordlists/rockyou.txt 
```

- john
```sh
# Create a hash that John can understand
zip2john flag.zip > ziphash.txt
# John will report the recovered password if it finds one.
john --wordlist=/usr/share/wordlists/rockyou.txt ziphash.txt
```

## detection of indicators and telemetry

* Offline cracking does not hit login services, so lockouts and failed logon dashboards stay quiet. We can detect the work where it runs, on endpoints and jump boxes. The important signals to monitor include:

* process creation: well-known binaries and command patterns...
1. Binaries and aliases: **john**, **hashcat**, **fcrackzip**, **pdfcrack**, **zip2john**, **pdf2john.pl**, **7z**, **qpdf**, **unzip**, **7za**, **perl** invoking pdf2john.pl.
2. Command‑line traits: **--wordlist**, **-w**, **--rules**, **--mask**, **-a 3**, **-m** in Hashcat, references to **rockyou.txt**, **SecLists**, **zip2john**, **pdf2john**.
3. Potfiles and state: **~/.john/john.pot**, **.hashcat/hashcat.potfile**, **john.rec**

- On Windows systems, Sysmon Event ID 1 captures process creation with full command line properties.
- while on Linux, auditd, execve, or EDR sensors capture binaries and arguments.

* GPU and Resource Artefacts
    - **nvidia-smi** shows long‑running processes named hashcat or john.
    - High, steady GPU utilisation and power draw while the fan curve spikes.
    - Libraries loaded: **nvcuda.dll**, **OpenCL.dll**, **libcuda.so**, **amdocl64.dll**.

* Network Hints: Offline cracking does not need the network once wordlists are present. Yet most operators fetch lists and tools first.
- Large text files (rockyou.txt...), or Git clones of popular wordlist repos.
- Package installs (apt install john hashcat...), detected by EDR package telemetry.

* Unusual File Reads: Repeated reads of files such as wordlists or encrypted files would need analysis.

* Detection rules:
    - Sysmon (ProcessName="..." OR CommandLine="...")
    - Linux audit rules (auditctl ...)
    - Sigma style rules (yaml-based)

* Response Playbook: the immediate actions to follow when such incidents occur.

* Practical:
    - output content of encrypted pdf:
```sh
pdftotext flag.pdf -upw <password> -
```
    - extracting encrypted zip content:
```sh
7z e -p<password> flag.zip
# using 7z instead of unzip because unzip doesn't support AES enryption (compression method 99)
```
