# OTW-Bandit








for bandit 14
bandit 14 
the level says you would not be given  the next password and giving

use scp to copy the ssh file from bandit 



scp sshkey.private Eth-steve@your_local_ip:/home/Eth-steve/Desktop/
copy the sshkey.private from 13 , change to 600 and use to login then search etc/bandit_pass/bandit14 for the password for 14 

Copy the key to your local machine:
on the local machine : while not logged in as bandit13
scp -P 2220 bandit13@bandit.labs.overthewire.org:/home/bandit13/sshkey.private /home/Eth-steve/Desktop

Set the correct permissions on the key (critical for SSH):
chmod 600 /home/Eth-steve/Desktop/sshkey.private

Use the key to log in as bandit14:
ssh -i /home/Eth-steve/Desktop/sshkey.private -p 2220 bandit14@bandit.labs.ove

for 15 in 14 use nc local host 30000 < password14-path 

for 16 in 15 use openssl s_client -connect 127.0.0.1:30001 -quiet < /etc/bandit_pass/bandit15

for 17 in 16 use nmap -sV -sT -p31000-32000 localhost 
this should reveal 5  open ports 4 of which is echo while one is  not

then openssl s_client -connect 127.0.0.1:31790 -quiet < /etc/bandit_pass/bandit16
which give me another sshprivate key,
then 
exit
 copy the sshprivate key and save it in separate file,  change the permissions to 600 ,
using ssh -i /path/to/sshkey.private -p 2220 bandit17@bandit.labs.overthewire.org

then find the password for bandit17 in bandit_pass
 
- for 18 while in 17 enter diff password.new password.old
- then the first is the answer

- for 19 while in 18 ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"   then 18s  password

- for 20 in 19 they syntax is clue ie 
  ./bandit20-do whoami
  and the  path/location of the file ie 
 ./bandit20-do cat /etc/bandit_pass/bandit20 


for 21 in 20  echo "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 4445 & (the & makes it run in the background)
then ./suconnect 4445
