Metasploit
==========

General Information
-------------------
Metasploit is a free tool that has built in exploits which aids in gaining remote
access to a system by exploiting  a vulnerability in that server.

`mfsconsole`                Launch program  
`version`                   Display current version  
`msfupdate`                 Pull the weekly update  
`makerc <FILE.rc>`          Saves recent command to file  
`msfconsole -r <FILE.rc>`   Load a resource file

Executing an exploit
--------------------
`use <MODULE>`              Set the exploit to use  
`set payload <PAYLOAD>`     Set the payload  
`show options`              Show all options  
`set <OPTION> <SETTING>`    Set a setting  
`exploit` or `run`          Execute the exploit

Session Handling
----------------
`session -l`                List all sessions  
`session -i <ID>`           Interact/attach to session  
`background` or `Ẑ`         Detach from session  

Using the DB
============
The DB saves data found during exploitation. Auxiliary scan results, hashdumps,
and credentials show up in the DB.

`db_status`                 Should say connected  
`hosts`                     Show hosts in DB  
`services`                  Show ports in DB  
`vulns`                     Show all vulnerabilities found  

First time setup
----------------
Run from the linux command line.

`service postgres start`    Start DB  
`msfdb init`                Init DB

Finding an Exploit to use
=========================
Do information gathering with db_nmap and auxiliary modules. Aux mods have numerous
scanners, gatherers, fuzzers, and tools that allow you to scan a CIDR block or
single IP and will save the result in the DB.

`db_nmap -sS -A 192.168.1.100`  Do port scan and OS fingerprint then add results to the DB   
`show auxiliary`            Show all auxiliary modules (scanners, fuzzers, proxies, etc.)  
`use auxiliary/scanner/smb/smb_version` Detect SMB version in use  
`use auxiliary/scanner/ft/anonymous`  Scan for anonymous FTP servers  
`use auxiliary/scanner/snmp/snmp_login` Scan for public SNMP strings

Once information is gathered on the host, look at what services or OS the host is
running and do a search for that term. Example: if NMAP found that the host is
running `smb` service, run `search smb` to find exploits for that service.

`search <TERM>`             Searches all exploits, payloads, and auxiliary modules
`show exploits`             Show all exploits
`show payloads`             Show all payloads

Linux commands
--------------
Many linux commands work from within `msf` like `ifconfig`, `nmap`, etc.

Workspaces
----------
`workspace -h`              Help  
`workspace`                 List  
`workspace -a`              Add  
`workspace -d`              Delete  
`workspace -r`              Rename  

Meterpreter commands
====================

`sysinfo`                   Show system info  
`ps`                        Show running processes  
`kill <PID>`                Terminate a process  
`getuid`                    Show your users ID  
`upload`/`download`         Upload/download a file  
`pwd`/`lpwd`                Print current working directory  
`cd`/`lcd`                  Change directory  
`cat`                       Show contents of a file  
`edit <FILE>`               Edit a file (vim)  
`shell`                     Drop into a shell  
`migrate <PID>`             Switch to another process  
`hashdump`                  Show all pw hashes (Win)  
`idletime`                  Display the idle time of user  
`screenshot`                Take a screenshot  
`clearev`                   Clear the logs

Escalate privileges
-------------------

`use priv`                  Load the script  
`getsystem`                 Elevate your privileges  
`getprivs`                  Elevate your privileges

Token stealing (Win)
--------------------

`use incognito`             Load the script  
`list_tokens -u`            Show all tokens  
`impersonate_token DOMAIN\\USER`  Use token  
`drop_token`                Stop using token

Misc
---------------
Enable port forwarding. This opens port 3388 on <LHOST> which forwards all traffic
to 3389 on <RHOST>.  
`meterpreter> portfw [ADD|DELETE] -L <LHOST> -l 3388 -r <RHOST> -p 3389`

Pivot trough a session by adding a route withing msf it allows you you to exploit
or scan adjacent hosts:  
`msf> route add <SUBNET> <MASK> <SESSIONID>`

*Based on v1.0 of TunnelsUp.com Metasploit cheatsheet.*
