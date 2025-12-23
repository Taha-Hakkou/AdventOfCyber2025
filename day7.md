## Day 7

* Network and port discovery tools: netcat, nmap, naabu

* nmap (Usage):

```sh
nmap MACHINE_IP
# scans top 1000 ports
nmap -p- --script=banner MACHINE_IP
# -p- : scans whole range
# --script=banner : what's likely behind the port !
```

* ftp (Usage):

```sh
ftp MACHINE_IP PORT
# try to login with: 'anonymous' mode
ftp> passive # switching passive mode off
ftp> ls # list files
ftp> get tbfc_qa_key1 - # downloads the file and prints it
ftp> ! # exits ftp shell
```

* port scan modes:
- when the service running on a port isn't well-known, use 'netcat' (universal tool to interact with network services)

```sh
nc -v MACHINE_IP PORT
# -v: verbose
```

* UDP scan (with nmap)

```sh
nmap -sU MACHINE_IP
# another 65535 ports to scan
# upd scan requires root privileges
```

* DNS

```sh
dig @MACHINE_IP TXT key3.tbfc.local +short
# dig: performs advanced dns queries
# basic usage: dig [@server_ip:optional] [domain:required] [type:optional]
# !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! (how should i know ?!)
```

* list listening ports:
```sh
ss -tunlp # OR netstat for older systems
# lists also services listening on 127.0.0.1 (available only from the host)
# can also view process column (with root permissions)
```

* mysql
- usually databases require password for remote clients, but allow unauthorized logins from localhost

```sh
mysql -D tbfcqa01 -e "show tables;"
# -D: database
# -e: execute
mysql -D tbfcqa01 -e "select * from flags;"
# OR:
mysql
mysql> show databases;
mysql> use tbfcqa01;
mysql> show tables;
mysql> select * from flags;
```