# OTW-Bandit

This file contains hints/commands for the OverTheWire Bandit wargame (levels 0–23). Passwords are intentionally not included here(Even if i did they change after x period of time, last change was 28/06/2026).

---

## Level 0 → Level 1

- List files and read the readme:

```sh
ls
cat readme
```

## Level 1 → Level 2

- Look for hidden or oddly named files:

```sh
ls
cat ./-
```

## Level 2 → Level 3

```sh
ls
cat ./--file--
```

## Level 3 → Level 4

```sh
ls
cd inhere
ls -lsa
cat ...file
```

## Level 4 → Level 5

```sh
ls
cd inhere
ls
# read files until you find the password
```
or 
```
cat ./*
```
That will let you read all the files at once

## Level 5 → Level 6

```sh
ls
cd inhere
ls
ls -lsaR
# search for the clue: 1033
cd foldername
cat .file
```

## Level 6 → Level 7

- If you see nothing obvious, check the website instructions: the key phrase is "somewhere on the server".
- Use find to search the entire filesystem for a 33-byte file owned by bandit7:bandit6:

```sh
find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null
```

- Then cat the file path returned by find, for example:

```sh
cat /path/to/the/file/bandit7.password
```

Explanation of find flags:
- `/` — start at root
- `-type f` — regular file
- `-size 33c` — exactly 33 bytes (c = bytes)
- `-user bandit7` — owned by user bandit7
- `-group bandit6` — group bandit6
- `2>/dev/null` — hide permission denied messages

## Level 7 → Level 8

- Look at `data.txt` and search for the word "millionth":

```sh
ls
cat data.txt
# find the line containing 'millionth'
cat data.txt | grep millionth
```

Note: the pipe character `|` sends the output of the left command into the right command.

## Level 8 → Level 9

- The password occurs only once. Find the unique line:

```sh
ls
cat data.txt
cat data.txt | sort | uniq -u
```

- `sort` sorts the lines; `uniq -u` prints lines that are unique (appear only once).

## Level 9 → Level 10

- `data.txt` looks like scrambled text; there may be multiple encodings/transforms. Inspect carefully for patterns that look like a password (Bandit passwords are alphanumeric strings).

## Level 10 → Level 11

- If the file is base64 encoded:

```sh
ls
base64 -d data.txt
```

## Level 11 → Level 12

- If the instructions say letters were rotated by 13 (ROT13), decode with `tr`:

```sh
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

## Level 12 → Level 13

.bz2,.gz,.tar are all compression command extensions
```
ls

cat data.txt
```
reveals a wall of number of wall and text
```
file data.txt
```
reveals this to be ASCII which is somewhat human-readable
```
cd ../../tmp

mkdir dir-name ; cd dir-name
```
**mkdir dir-name ; cd dir-name**
this is command chaining 
we are still the it dir-name folder
```
mv /home/bandit12/data.txt /tmp/dir-name

ls 

```
looking at commands needed for this level i MAN the one i did not know which is  **xxd**
```
man xxd
```
revealing **xxd** can actually revert Hexademical converted files. so
```
xxd -r data.txt > data2
```
-r for revert 
**> data2**: enter the output into data2
```
file data2
```
this reveal that data2 is now a gzip file
the next step is 
```
gzip -d data2
```
right 😂 wrong if you do this i will give you an error
rename data2 as data2.gz using 
```
mv data2 data2.gz
```
then 
```
gzip -d data2.gz
```
-d : for decompressing 
```
file data2 
```
data2 is now a bzip2 file rename it again
```
mv data2 data2.bz2
```
then 
```
bzip2 -d data2.bz2
```
then data2 is now a gzip file again &#x20;rename it again 
```
mv data2 data2.gz
```
&#x20;
```
file data2
```
&#x20;this reveal data2 is a tar file now, rename it again (do you see the pattern 😉 )
```
mv data2 data2.tar
```
then
```
tar -xf data2.tar
```

- **-x** : extract
  
- **f** : file
```
ls 
```
you should see a  data5.bin
```
file data5.bin 
```
reveals that this is a tar file, Rename it again
```
mv  data5.bin data5.bin.tar
```
then
```
tar -xf data5.bin.tar
ls 
```
you should see a  data6.bin file
```
file data6.bin
```
reveals that this is a bzip file again 😢 rename it again
```
mv data6.bin data6.bin.bz2
```
then 
```
bzip2 -d data6.bin.bz2
```
then 
```
file data6.bin
```
reveals that this is a tar file again 😢, rename it again
```
mv data6.bin data6.bin.tar
```
then
```
tar -xf data6.bin.tar

ls 
```
you should see data8.bin, then 
```
file data8.bin
```
reveals that this is a gzip file again 😢, rename it again 
```
mv data8.bin data8.bin.gz
```
then 
```
ls
```
next
```
gzip -d data8.bin.gz

ls
```
you should see data8.bin

cat data8.bin

The password is \*\*\*\*\*\*\*\*\*(finally 😃)

- Common compressed/archive formats: `.bz2`, `.gz`, `.tar`.
- Repeat the rename/decompress/extract steps as indicated by `file` output until you reach a plaintext file that contains the password.

Notes:
- `xxd -r` reverses a hex dump produced by `xxd`.
- Use `file <filename>` to determine the current format.
- Many Bandit levels require repeating decompression steps in sequence.

## Level 13 → Level 14

- Bandit 13 gives you a private SSH key. Use `scp` to copy it to your local machine and `ssh -i` to log in.
```
ls
```
this reveals to file 
```
HINT sshkey.private
```
right, **cat**-ing the sshkey.private, you should see an ssh private key 

An **SSH private key** is a secret cryptographic file that acts as the unique "key" to unlock access for a user or process, allowing them to prove their identity to a server via challenge-response authentication without ever sharing the file itself

so highlight everything start from  "-- to the last --", then copy it
exit from bandit13 
on your local Machine 
```
nano sshkey.private
```
paste the key you copied in it then save the file, 
# Set secure permissions
```
chmod 600 ~/Desktop/sshkey.private
```
# Use it to log in as bandit14
```
ssh -i ~/Desktop/sshkey.private -p 2220 bandit14@bandit.labs.overthewire.org
```
pay attention to the location of the sshkey.private file 

## Level 14 → Level 15

- The service may require sending the password to a TCP port, e.g. using `nc`:

```
# from the remote host, where password14-path is the file containing bandit14's password
nc localhost 30000 < /etc/bandit_pass/bandit14
```

## Level 15 → Level 16

- Use OpenSSL s_client to communicate over TLS to a local port and feed the previous password in:

```sh
openssl s_client -connect 127.0.0.1:30001 -quiet < /etc/bandit_pass/bandit15
```

## Level 16 → Level 17

- Use nmap to discover open high ports and find the interesting service:

```sh
nmap -sV -sT -p31000-32000 localhost
```

- Suppose one port is not an echo service; connect with openssl s_client and send the bandit16 password:

```sh
openssl s_client -connect 127.0.0.1:31790 -quiet < /etc/bandit_pass/bandit16
```

- This command output an SSH private key, save it to a file, `chmod 600` the file, then use it to ssh into the next level.
- with this command
  ```
  ssh -i ~/Desktop/sshkey.private -p 2220 bandit17@bandit.labs.overthewire.org
  ```

## Level 17 → Level 18

- When provided two files (`password.new` and `password.old`), `diff` can show differences; the first differing line is often the answer:

```sh
diff password.new password.old
```

## Level 18 → Level 19

- From level 18, you may be able to run a one-liner SSH command to read a file on the remote host:

```
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```
Then enter the password for 18 to get 19

## Level 19 → Level 20

- Some levels provide a binary that only accepts specific arguments. Example:

```sh
./bandit20-do whoami
./bandit20-do cat /etc/bandit_pass/bandit20
```

Run the provided program with the correct parameters and/or input method.

## Level 20 → Level 21

- Example using netcat and background listener (on your local machine or remote depending on level):

```
# In one terminal run 
echo "passowrd-fo-20" | nc -l -p 4445 &
# In another terminal, run the suconnect program to connect to port 4445
./suconnect 4445
```
then check the previous terminal for the password for 21

---

## Level 21 → Level 22
```
cd  /etc/cron.d

ls

cat cronjob_bandit22
```
reading the content of the program it **reboot** bandit22 with program cronjod_bandit22.sh which is scheduled  to run everytime 
so
```
cd /usr/bin
cat cronjod_bandit22.sh
```
this reveals that the program changes the perms of the file to 644 then **cat** the password for bandit22 into the /tmp/filename, so 
```
cat tmp/filename
```
This should reveal the password for 22


## Level 22 → 23

```
cd /etc/cron.d

ls

cat cronjob_bandit22
```
Reading the content of the program it **reboot** bandit22 with program cronjod_bandit22.sh which is scheduled  to run everytime so 
```
cd /usr/bin
cat cronjod_bandit22.sh
```
which reveals
```
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```
the program runs the command **whoami** which gives "bandit22" store it in a variable called "myname"
the next line runs the command "echo I am user $myname | md5sum | cut -d ' ' -f 1"
- **md5sum**: Pipes that string into the md5sum tool, which calculates the MD5 hash of the input.
Note: Because echo adds a newline, it hashes the string fish\n, not just fish.
- **cut -d ' ' -f 1**: Pipes the hash output (which includes the hash and the filename marker -) into cut. It uses a space ( ) as the delimiter (-d) and extracts only the first field (-f 1), effectively isolating just the hash string

And saves it in a variable called "mytarget"
the next command is just echo
the next command cats  "bandit22" into /tmp/"mytarget"
so 
```
cat tmp/$mytarget
```
This should reveal the password for 23

## Level 22 → 23
```
ls 

cd into /etc/cron.d

ls

cat cronjob_bandit24
```

reading the content of the cronjob  it **reboot** bandit24 with program cronjob_bandit24.sh which is scheduled  to run every time so
```
cd /usr/bin

cat cronjob_bandit24.sh
```
run cat cronjob_bandit24.sh reveals the content of the script
```
#!/bin/bash

shopt -s nullglob

myname=$(whoami)

cd /var/spool/"$myname"/foo || exit
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" "./$i")"
        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi
        rm -rf "./$i"
    fi
done
```
the script above 
ask who the user is with **whoami**
then checks /var/spool/myname/foo 
then echo 
then deletes all the file owned by myname in that folder

which means this particular folder  can execute any and every script in that folder 

so lets create a very simple script
to do that  we will 
```
mdkir /tmp/tunde ; cd tunde
```
then the  script 
```
nano  get-bandit24-passwd.sh 
```
paste this in the file 
```
#!/bin/bash 
cat /etc/bandit_pass/bandit14 > /tmp/tunde/the_password.txt
```
this script will run as bandit24 thanks to the folder will put it in, so it will be able to read the  bandit24 passwd file
after saving the script  
then 
```
chmod +x  get-bandit24-passwd.sh 
```
this make the script an executable
then 
```
cp get-bandit24-passwd.sh /var/spool/bandit24/foo
```
wait for a minute to go by before typing 
```
ls
``` 
then you should see the_password.txt file in  /tmp/tunde dir
the **cat** it 
you should get your answer for level 24.

## Level 23 → 24
After reading the instruction from the site i pick the scent that this level would require a script that will first run  all the possible combination from 0000-10000 
then send the pin+bandit24_passwd to the listener on port 30002




