
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

