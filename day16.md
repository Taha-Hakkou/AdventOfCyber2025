# Day 16 (Registry Forensics)

* The registry contains all the information (configs...) that the Windows OS needs for its functioning. 

* this registry is not stored in 1 single place, but rather made up of several separate files, each storing information on different configuration settings. These files are known as **Hives**:

    | Hive Name    | Contains (Not all)                                                                 | Location                                                       |
    | ------------ | ---------------------------------------------------------------------------------- | -------------------------------------------------------------- |
    | SYSTEM       | Services, Mounted Devices, Boot Configuration, Drivers, Hardware                   | C:\Windows\System32\config\SYSTEM                              |
    | SECURITY     | Local Security Policies, Audit Policy Settings                                     | C:\Windows\System32\config\SECURITY                            |
    | SOFTWARE     | Installed Programs, OS Version and other info, Autostarts, Program Settings        | C:\Windows\System32\config\SOFTWARE                            |
    | SAM 	     | Usernames and their Metadata, Password Hashes, Group Memberships, Account Statuses | C:\Windows\System32\config\SAM                                 |
    | NTUSER.DAT   | Recent Files, User Preferences, User-specific Autostarts                           | C:\Users\username\NTUSER.DAT                                   |
    | USRCLASS.DAT | Shellbags, Jump Lists                                                              | C:\Users\username\AppData\Local\Microsoft\Windows\USRCLASS.DAT |

* These Registry Hives contain binary data that cannot be opened directly from the file, but rather the Windows OS has a built-in tool known as the Registry Editor.

* Windows organizes all the Registry Hives into structured Root Keys (HKEY_LOCAL_MACHINE, HKEY_CURRENT_USER, HKEY_USERS).

* Which registry key contains which registry hive's data?

    | Hive on Disk |	Where You See It in Registry Editor    |
    | ------------ | ----------------------------------------- |
    | SYSTEM       |	HKEY_LOCAL_MACHINE\SYSTEM              |
    | SECURITY     |	HKEY_LOCAL_MACHINE\SECURITY            |
    | SOFTWARE     |	HKEY_LOCAL_MACHINE\SOFTWARE            |
    | SAM 	       |    HKEY_LOCAL_MACHINE\SAM                 |
    | NTUSER.DAT   |	HKEY_USERS\<SID> and HKEY_CURRENT_USER |
    | USRCLASS.DAT |	HKEY_USERS\<SID>\Software\Classes      |

* The other two keys (HKEY_CLASSES_ROOT (HKCR) and HKEY_CURRENT_CONFIG (HKCC)) are not part of any separate hive files. They are dynamically populated when Windows is running.

* extracting information from the registry:
    - Example 1: View Connected USB Devices
        + path: "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USBSTOR" shows USB devices' information (make, model, and device ID).
        + A main subkey that is the identification of the type and manufacturer of the USB device.
        + A subkey under the above (for example) that represents the unique devices under this model.
    - Example 2: View Programs Run by the User
        + path: "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU".
        + list of commands typed by the user in the Run dialog (Win + R) to run applications.

* Registry forensics is the process of extracting and analyzing evidence from the registry.

* Some registry keys that are particularly useful during forensic investigations:

| Registry Key                                                           |	Importance (What it stores)                                                                                |
| ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist     |	Information on recently accessed applications launched via the GUI.                                        |
| HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths     |	All the paths and locations typed by the user inside the Explorer address bar.                             |
| HKLM\Software\Microsoft\Windows\CurrentVersion\App Paths               |	The path of the applications.                                                                              |
| HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery |	All the search terms typed by the user in the Explorer search bar.                                         |
| HKLM\Software\Microsoft\Windows\CurrentVersion\Run                     |	Information on the programs that are set to automatically start (startup programs) when the users logs in. |
| HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs     |	Information on the files that the user has recently accessed.                                              |
| HKLM\SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName        |	The computer's name (hostname).                                                                            |
| HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall               |	Information on the installed programs.                                                                     |

* The investigation of registry keys during forensics cannot be done via the built-in Registry Editor tool on the system under investigation (chance of modification).

* We collect the Registry Hives and open them offline into our forensic workstation. However, the Registry Editor does not allow opening offline hives. Also it displays some of the key values in binary which are not readable.

* Some tools built for registry forensics:
    - [Registry Explorer](https://ericzimmerman.github.io/)

* Registry Hives can sometimes be "dirty" when collected from live systems, meaning they may have incomplete transactions.

* Practical:
    1. Launch 'Registry Explorer'
    2. Load Hive (contains binary data)
    3. "Shift + Open": loads associated transaction log files (ensures you get a clean, consistent hive state for analysis).
    4. Repeat the same process for all the other hives you want to load.
    5. Navigate to specific registry keys using:
        - Path
        - Search bar
        - available bookmarks

