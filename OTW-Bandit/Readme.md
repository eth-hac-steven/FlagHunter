# OTW-Bandit

for lvl 1 in lvl 0
ls 
cat readme

for lvl2 in lvl1
ls
cat ./-

for lvl3 in lvl2
ls 
cat ./--file--

for lvl4 in lvl3
ls
cd inhere
ls -lsa
cat ...file

for lvl5 in lvl4
ls 
cd inhere
ls 
cat all the file still you get the password

for lvl6 in lvl5 
ls
cd inhere
ls
ls -lsaR
search  for the clue: 1033
cd foldername
cat .file

for lvl7 in lvl6
ls 
nothing 
read the instructions on the website the key word is "somewhere on the server"
find  / -type f -size  33c -user bandit7 -group bandit6 2>/dev/null
/ : search the root dir
-type f : type of file being search for , in this case is a normal file hence f
-size 33c : the size of the file c represents bytes
-user bandit7 : search for the specified user
-group bandit6 : search for the specified group
-2>/dev/null: this gets rid of the any permission denied errors

cat path/to/the/file/bandit7.password


for lvl8 in lvl7 
ls
cat data.txt
wall of text 
read the instructions on the website The password in next to the word "millionth"
cat data.txt | grep millionth

| this sign is called pipe it takes the output of one commands sends to the next command, this is an example of command chaining

for lvl9 in lvl8
ls 
cat data.txt
wall of text
read the instructions on the website The password only occurs once
cat data.txt | sort | uniq -u

sort : sorts the wall of text alphanumerically
uniq -u : find the unique one amongst them


for lvl 10 in lvl 9 
ls
cat data.txt
you will see rubbish literally scrambled text , do not disregard it the password is there
CLEARFULLY go through it you should see the password 
at this point you should already know a password in this game should look like so find it

is there a better way to find it yes....
find that too :joy


for lvl11 in lvl 10
ls
data.txt
base64 -d data.txt

for lvl12 iv lvl11
ls 
cat data.txt
read the instructions on the website The password is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'

for lvl13 in lvl12
.bz2
.gz
.tar are all extensions

ls
cat data.txt
file data.txt
cd ../..
cd tmp
mkdir dir-name
cd dir-name
mv data.txt data
ls 
looking at commands needed for this level i MAN the one i did not know aka xxd
revealing xxd can actually revert ascii converted files. so
xxd -r data > data2
-r for revert 
> data2 : enter the output into data2

file data2
this reveal that data2 is now a gzip file
the next step is 
gzip -d data2 right :joy wrong

if you do this i will give you an error
rename data2 as data2.gz using 
mv data2 data2.gz
then 
gzip -d data2.gz

-d : for decompressing 

file data2 
this is now a bzip2 file 
rename it again
mv data2 data2.bz2

then 
bzip2 -d data2.bz2
then data2 is now a gzip file again
 rename it again 
mv data2 data2.gz
 
file data2
 this reveal data2 is a tar file now

rename it again (do you see the pattern:wink )
mv data2 data2.tar
then
tar -xf data2.tar

-x : extract 
f : file

ls 
you should see a  data5.bin
file data5.bin 
reveals that this is a tar file 
Rename it again
mv  data5.bin data5.bin.tar
then
tar -xf data5.bin.tar
ls 
you should see a  data6.bin
file data6.bin
reveals that this is a bzip file again :crying
rename it again
mv data6.bin data6.bin.bz2

then 
bzip2 -d data6.bin.bz2
then 
file data6.bin
reveals that this is a tar file again :crying
rename it again
mv data6.bin data6.bin.tar
then
tar -xf data6.bin.tar
ls 
you should see data8.bin
then 
file data8.bin
reveals that this is a gzip file again :crying
rename it again 
mv data8.bin data8.bin.gz
then 
ls
next
gzip -d data8.bin.gz
ls
you should see data8.bin
cat data8.bin
The password is *********(finally :happy)



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
