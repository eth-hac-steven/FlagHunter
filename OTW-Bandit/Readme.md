# OTW-Bandit

This file contains a cleaned and formatted set of hints/commands for the OverTheWire Bandit wargame (levels 0–21). Passwords are intentionally not included here(Even if i did they change afte x period of time, alst change was 28/06/2026), replace placeholders with the actual passwords you obtain while playing.

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

- Common compressed/archive formats: `.bz2`, `.gz`, `.tar`.
- Example workflow when you have a blob that looks like hex/ascii dump and you need to revert it and iteratively unpack:

```sh
# Work in /tmp or a working directory
mkdir -p /tmp/bandit && cd /tmp/bandit
# Suppose data.txt contains a hex dump: revert it
xxd -r data.txt > data2
file data2
# If file is gz data, rename then decompress
mv data2 data2.gz
gzip -d data2.gz
file data2
# If it's bzip2:
mv data2 data2.bz2
bzip2 -d data2.bz2
file data2
# If it's a tar archive:
mv data2 data2.tar
tar -xf data2.tar
ls
```

- Repeat the rename/decompress/extract steps as indicated by `file` output until you reach a plaintext file that contains the password.

Notes:
- `xxd -r` reverses a hex dump produced by `xxd`.
- Use `file <filename>` to determine the current format.
- Many Bandit levels require repeating decompression steps in sequence.

## Level 13 → Level 14

- Bandit 13 gives you a private SSH key. Use `scp` to copy it to your local machine and `ssh -i` to log in.

Example copy (from your local machine):

```sh
# Copy the key from the remote bandit server to your local machine (adjust port and paths)
scp -P 2220 bandit13@bandit.labs.overthewire.org:/home/bandit13/sshkey.private ~/Desktop/sshkey.private
# Set secure permissions
chmod 600 ~/Desktop/sshkey.private
# Use it to log in as bandit14
ssh -i ~/Desktop/sshkey.private -p 2220 bandit14@bandit.labs.overthewire.org
```

- If you copy the key while already on the remote host, use `scp` with your local machine's username and IP. Replace `your_local_ip` and username appropriately.

## Level 14 → Level 15

- The service may require sending the password to a TCP port, e.g. using `nc`:

```sh
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

- The service can output an SSH private key — save it to a file on the remote host, `chmod 600` it, then use it to ssh into the next level.

## Level 17 → Level 18

- When provided two files (`password.new` and `password.old`), `diff` can show differences; the first differing line is often the answer:

```sh
diff password.new password.old
```

## Level 18 → Level 19

- From level 18, you may be able to run a one-liner SSH command to read a file on the remote host:

```sh
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

## Level 19 → Level 20

- Some levels provide a binary that only accepts specific arguments. Example:

```sh
./bandit20-do whoami
./bandit20-do cat /etc/bandit_pass/bandit20
```

Run the provided program with the correct parameters and/or input method.

## Level 20 → Level 21

- Example using netcat and background listener (on your local machine or remote depending on level):

```sh
# On the listener side (background)
echo "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 4445 &
# On the other side, run the suconnect program to connect to port 4445
./suconnect 4445
```

---

