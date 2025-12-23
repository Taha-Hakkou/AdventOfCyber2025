## day 13

* YARA = Yet Another Recursive Acronym
* YARA is an open-source tool built to identify and classify malware by searching for unique patterns, the digital fingerprints left behind by attackers.
* it scans code, files, and memory for subtle traces that reveal a threat’s identity.
* it allows you to define your own rules, providing your own view of what constitutes "malicious" behavior.
* it brings several key advantages: speed, flexibility, control, shareability, visibility...
* YARA empowers defenders to move from passive monitoring to active hunting.
* A YARA rule is built from several key elements:
    - Metadata: information about the rule itself: who created it, when, and for what purpose.
    - Strings: the clues YARA searches for: text, byte sequences, or regular expressions that mark suspicious content.
    - Conditions: the logic that decides when the rule triggers, combining multiple strings or parameters into a single decision.
* Example:
```sh
rule rule_name
{
    meta:
        author = "author"
        description = "description"
        date = "2025-10-10"
    strings:
        $s1 = "rundll32.exe" fullword ascii
        $s2 = "msvcrt.dll" fullword wide
        $url1 = /http:\/\/.*malhare.*/ nocase
    condition:
        any of them
}
```
* metadata is optional but recommanded (especially when collection of YARA rules grow)
* Strings (3 main types):
    - Text strings
        1. YARA treats it by default as ascii and case-sensitive
        2. special modifiers: encoding, case tricks, or even encryption (nocase, wide, ascii, xor, base64...)
        3. wide and ascii modifiers can be used both together
    - Hexadecimal strings
        1. to detect malware fragments like file headers, shellcode, or binary signatures
        2. example:
        ```sh
        $mz = { 4D 5A 90 00 }   // MZ header of a Windows executable
        $hex_string = { E3 41 ?? C8 G? VB }
        ```
    - Regular expression strings
        1. useful for spotting URLs, encoded commands, or filenames that share a structure but differ slightly each time.
        2. example:
        ```sh
        $url = /http:\/\/.*malhare.*/ nocase
        $cmd = /powershell.*-enc\s+[A-Za-z0-9+/=]+/ nocase
        ```
* Conditions: (matching)
    - single string:         $str
    - any string:            any of them
    - all strings:           all of them
    - Combine logic using:   and, or, not
    - Use comparisons like:  filesize, entrypoint, or hash (example: any of them and (filesize < 700KB))

* YARA usage: (man yara)
    - -r: Allows YARA to scan directories recursively and follow symlinks
    - -s: Prints the strings found within files that match the rule
    - example:
    ```sh
    yara -r icedid_starter.yar C:\
    ```