# HTB Notes

## Starting Point

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