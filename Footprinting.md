
### `Enumeration is the key`

- What can we see?
- What reasons can we have for seeing it?
- What image does what we see create for us?
- What do we gain from it?
- How can we use it?
- What can we not see?
- What reasons can there be that we do not see?
- What image results for us from what we do not see?


**Principles of Enumeration:**
1. There is more than meets the eye. Consider all points of view.
2. Distinguish between what we see and what we do not see.
3. There are always ways to gain more information. Understand the target.



---



**ENUMERATION IS AN EXPERIENCE BASED APPROACH!**


![700](attachments/Pasted%20image%2020261112092634.png)


> **Methodology is a dynamic and growing aspect that is constantly changing and can therefore differ.**



---



### Domain Information


1. SSL certificates and transparency logs   --->  https://crt.sh/

```bash
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq .
```

2. Pickup the unique subdomains: https://crt.sh/

```bash
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u
```

3. Check for company hosted servers by checking the IP addresses and ranges.

```bash
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done
```

4. extract IP Addresses ( we cannot test 3rd party infra without permission):

``` bash
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f4 >> ip-addresses.txt;done
```

5. Check shodan info of these IP Addresses:

```bash
for i in $(cat ip-addresses.txt);do shodan host $i;done
```

6. Check DNS Records

```bash
dig any example.com
```

6. Investigate any 3rd party services you find there and investigate and analyze how the company will be using it for that.



----



### Cloud Resources

- S3 Buckets
- Blobs
- Cloud Storage


- Google Dorking filters like `inurl:` and `intext:`
	- for aws: `*.amazonaws.com`
	- for azure: `*.blob.core.windows.net`
- Check the target website for source code
- [Domain.Glass](https://Domain.Glass)
- [GrayHatWareFare](https://buckets.GrayHatWareFare.com)
- Check for leaked SSH keys

and donot stop here!



---



### Staff


Searching for and identifying employees on social media platforms can also reveal a lot about the teams' infrastructure and makeup. This, in turn, can lead to us identifying which technologies, programming languages, and even software applications are being used.

- [LinkedIn](https://www.linkedin.com/) or [Xing](https://www.xing.de/)



---



### File Transfer Protocol (FTP):

- TCP Port 21 = Control Plane
- TCP Port 20 = Data Plane

**Connection Type:**
- Active = Data plane connection initiated by the remote host
- Passive = Data plane connection initiated by our host


**TFTP (Trivial File Transfer Protocol):**
- Uses UDP Port 69
- no directory listing fucntionality


**Default configuration:**

```bash
nano /etc/vsftpd.conf
```

**FTP Users:**

this file contains the isers that are not allowed to login to the ftp server

```bash
cat /etc/ftpusers
```



**Anonymous Login:**

- Users can login with a anonymous user
- Can be configured in `/etc/vsftpd.conf`


**Download All Available Files:**

```bash
wget -m --no-passive ftp://anonymous@anonymous@IP
```

**Upload a file:**

```bash
put filename.txt
```


**Common Scripts for FTP enumeration:**

```
ftp-anon
ftp-syst
```


**Connect over a secure connection using `openssl`:**

```bash
openssl s_client -connect IP:PORT -starttls ftp
```


