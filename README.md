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



---
---
---


# REAL GRIND STARTS FROM HERE!


## Enumeration:- `Enumeration is the key`.

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



---



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


| **States**         | **Description**                                                                                                                                                                                         |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `open`             | This indicates that the connection to the scanned port has been established. These connections can be **TCP connections**, **UDP datagrams** as well as **SCTP associations**.                          |
| `closed`           | When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an `RST` flag. This scanning method can also be used to determine if our target is alive or not. |
| `filtered`         | Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target.                  |
| `unfiltered`       | This state of a port only occurs during the **TCP-ACK** scan and means that the port is accessible, but it cannot be determined whether it is open or closed.                                           |
| `open\|filtered`   | If we do not get a response for a specific port, `Nmap` will set it to that state. This indicates that a firewall or packet filter may protect the port.                                                |
| `closed\|filtered` | This state only occurs in the **IP ID idle** scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall.                                           |



---



### Important Flags:

- **Scan Top 10 Ports:** --top-ports=10
- **Top 100 Ports:** -F
- **trace all packets**: `--packet-trace`
- **Disable all ARP pings**: `--disable-ping-scan`
- **Disable the host discovery process**: `-Pn`
- **Disable the host name resolving**: `-n` 
- **shows reason for a port state**: `--reason`
- **Show scan status every 5 seconds:** `--status-every=5s`
- **Verbosity level:** `-v` or `-vv`
- **Speed:** `-T1`, `-T2`, `-T3`, `-T4`, `-T5`
- `--min-parallelism <number>`
- `--max-rtt-timeout <time>`
- `--initial-rtt-timeout <time>`
- `--max-retries`
- `--min-rate 300`
- **Number of Open Ports:** `cat tnet.default | grep "/tcp" | wc -l`



---



### Scan Flags:

- **Connect Scan:** `-sT`
- **Half TCP Scan:** `-sS`
- **UDP Scan:** `-sU`
- **FIN Scan**: `-sF`
- **NULL Scan:** `-sN`
- **Xmass Scan:** `-sX`
- **IDLE / Zombie scan:** `-sI`
- **Service Enumeration and Detection:** `-sV`
- **Aggressive Scan:** `-A`



---



### **OS guessing by TTL:**

- **Windows:** `128`
- **Linux**:`64`
- **Cisco Routers and Network Equipment:** `255` 



---



### Reporting:

- **`-oN`:** Normal report
- **`-oG`:** Grep able report
- **`-oX`:** XML report
- **`-oA`:** All reporting formats

**Turn the XML to HTML using the command:**

```bash
xsltproc report.xml -o report.html
```


> **We can generally say that the service headers come up with `PSH` flag in the `TCP` headers.**



---



### Nmap Scripting Engine (NSE):

- Used to execute specific scripts written in Lua.
- 14 categories of Scripts can be found at webstie.


#### Default Scripts

```bash
typhoon26@htb[/htb]$ sudo nmap <target> -sC
```


#### Specific Scripts Category

```bash
typhoon26@htb[/htb]$ sudo nmap <target> --script <category>
```


#### Defined Scripts

```bash
typhoon26@htb[/htb]$ sudo nmap <target> --script <script-name>,<script-name>,...
```



---



### Firewall and IDS / IPS Evasion:

- **Detect Firewall:** Use Flag manipulation and observe the responses by nmap.
- **Detect IDS:** Interact heavily with the target and it will send the alert to admin and admin will block you.
- **Detect IPS:** Interact heavily with the target and it will automatically block you.

- Use Decoys:

```bash
nmap -sS <target> -Pn -n --packet-trace --disable-arp-ping --reason -D RND:5
```

- **`IP ID` Manipulation:**  We can use IP ID to check the behavior or ports and responses, we can get the number of expected hosts in there and also guess the OS with IP ID, Modern systems set sequential `IP ID`s while old system and linux set random and large values and sometimes `0`.

- **Scan using a Different Source IP (Spoofing**):

```bash
nmap -sS <target> -Pn -n -S <SPOOFED_IP> -e <INTERFACE>
```


Sometimes IDS, IPS and firewalls trust DNS Servers as in DMZs we can have to interact with the web servers so company's own DNS Servers are more trusted than externals mostly on port 53.

```bash
nmap -sS <target> -n -Pn --source-port 53
```

We can then connect to that port with ncat to check the info such as banner.

```bash
ncat -nv --source-port 53 10.129.2.28 50000
```


**Lessons learned for nmap enumeration:**

- Never overlook any anything, go into the details.
- It takes time, be patient and have sbrrr.
- Good things take time.



![](attachments/Pasted%20image%2020261111111542.png)



---



NEXT