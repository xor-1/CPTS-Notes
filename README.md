- [ ]  Root a Retired Easy Box  
    
- [ ]  Root a Retired Medium Box  
    
- [ ]  Root an Active Box  
    
- [ ]  Complete an Easy Challenge  
    
- [ ]  Share a Walkthrough of a Retired Box  
    
- [ ]  Complete Offensive Academy Modules  
    
- [ ]  Root Live Medium/Hard Boxes  
    
- [ ]  Complete A Track  
    
- [ ]  Win a `Hack The Box Battlegrounds` Battle  
    
- [ ]  Complete A Pro Lab
## Penetration Testing process:

![](attachments/Pasted%20image%2020261101163806.png)


| Port            | Protocol              |
| --------------- | --------------------- |
| `161` (TCP/UDP) | `SNMP`                |
| `389` (TCP/UDP) | `LDAP`                |
| `443` (TCP)     | `SSL`/`TLS` (`HTTPS`) |
| `445` (TCP)     | `SMB`                 |
| `3389` (TCP)    | `RDP`                 |
| `88`            | Kerberos              |


### Types of shells:

- Reverse Shells
- Bind Shells
- Web Shells (Scripts on the webroot that execute the parameter commands we supply and bring back the output to the browser)



### Upgrading shells TTY

once you get the shell on the server, whether its reverse or bind, the first thing is to stablize it by executing the bash shell:

```python
python -c 'import pty; pty.spawn("/bin/bash")'
```

this command executes a single line python script in which a module `pty` is imported and one of its function `spawn()` is used to execute a command `"/bin/bash"` to execute fully interactive bash shell from which we can use special keys like `backspace` and `ctrl+c` etc. and show previous commands using up arrow

after that background this process using `ctrl+z` and type this in your terminal:

```bash
stty raw -echo
```

this command will pass the raw input into your shell terminal without processing the commands, and `-echo` means that the commands you will enter will not be reflected back to you with the response

and then foreground the shell process in your terminal using `bg`

Now I have a stable shell but i also want to match the size of my local TTY with the remote TTY, for this I need the **TWO** variables, i.e `$TERM` and `size` of `TTY`

Open a new terminal on the local machine:

```bash
echo $TERM
```

```bash
stty size
```

now I got the `$TERM` and `size` dimensions in `rows` and `columns`.

now go to the shell terminal and input commands to set the terminal as follows:

```bash
export TERM=<YOUR_TERM_VALUE>
```

this will set your terminal type TTY to the TTY same as of your local machine

```bash
stty rows <rows> columns <columns>
```

using this command you will have the dimensions of shell as same as of the local terminal.


Now you have a fully stablized shell and
 
 
 #### YOU ARE READY TO GO!

- [ ]  Root a Retired Easy Box  
    
- [ ]  Root a Retired Medium Box  
    
- [ ]  Root an Active Box  
    
- [ ]  Complete an Easy Challenge  
    
- [ ]  Share a Walkthrough of a Retired Box  
    
- [ ]  Complete Offensive Academy Modules  
    
- [ ]  Root Live Medium/Hard Boxes  
    
- [ ]  Complete A Track  
    
- [ ]  Win a `Hack The Box Battlegrounds` Battle  
    
- [ ]  Complete A Pro Lab



# REAL GRIND STARTS FROM HERE!


## Enumeration:- `Enumeration is the key`.


`Manual enumeration` is a `critical` component. Many scanning tools simplify and accelerate the process. However, these cannot always bypass the security measures of the services.


Network Mapper (`Nmap`) is an open-source network analysis and security auditing tool written in C, C++, Python, and Lua. It is designed to scan networks and identify which hosts are available on the network using raw packets, and services and applications, including the name and version, where possible. It can also identify the operating systems and versions of these hosts.

### Nmap Architecture:

- Host discovery
- Port scanning
- Service enumeration and detection
- OS detection
- Scriptable interaction with the target service (Nmap Scripting Engine)


The basic syntax of using nmap is generally as follows:

```bash
typhoon26@htb[/htb]$ nmap <scan types> <options> <target>
```


### Scan Techniques:

```bash
typhoon26@htb[/htb]$ sudo nmap -sS localhost

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-11 22:50 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000010s latency).
Not shown: 996 closed ports
PORT          STATE           SERVICE
22/tcp          open           ssh
80/tcp          open           http
5432/tcp     open           postgresql
5901/tcp     open           vnc-1

Nmap done: 1 IP address (1 host up) scanned in 0.18 seconds
```


### Host Discovery:

```bash
sudo nmap -sn 127.0.0.0/24 -oA typhoon | grep for | cut -d" " -f5
```


We can also use a list of IP already to given to us using the List file name with flag `-iL` or an IP range or scan a single or multiple IPs.

- `-PE`: Performs the ping scan using `ICMP echo requests` against the target.
- `--packet-trace`: Shows all packets sent and received
- `--reason`: Displays the reason for the specific result

### Host and Port scanning:

- Open ports and its services
- Service versions
- Information that the services provided
- Operating system


|**State**|**Description**|
|---|---|
|`open`|This indicates that the connection to the scanned port has been established. These connections can be **TCP connections**, **UDP datagrams** as well as **SCTP associations**.|
|`closed`|When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an `RST` flag. This scanning method can also be used to determine if our target is alive or not.|
|`filtered`|Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target.|
|`unfiltered`|This state of a port only occurs during the **TCP-ACK** scan and means that the port is accessible, but it cannot be determined whether it is open or closed.|
|`open\|filtered`|If we do not get a response for a specific port, `Nmap` will set it to that state. This indicates that a firewall or packet filter may protect the port.|
|`closed\|filtered`|This state only occurs in the **IP ID idle** scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall.|
















```bash
sudo nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT
```

 - `--packet-trace`: it logs all the packets that are being sent or RCVD from the target.
 - `-Pn`: It disables the host discovery process and starts the scan directly.
 - `--disable-arp-ping`: It disables the host discovery through ARP lookup and sends packets using other flags like `ICMP ECHO` or `SYN`.
 - -`PE`: It strongly makes the system to send the `ICMP echo` requests for host discovery.

 