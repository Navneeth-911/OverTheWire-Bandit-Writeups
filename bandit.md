# OverTheWire Bandit — Levels 0 to 22



## Level 0

### Objective
Connect to the Bandit server using SSH.

### Command Used

ssh bandit0@bandit.labs.overthewire.org -p 2220

### Explanation

The ssh command is used to securely connect to a remote server.

- bandit0 is the username.
- bandit.labs.overthewire.org is the Bandit server.
- -p 2220 specifies the port number used by the server.

After entering the Level 0 password, I successfully logged into the Bandit server as bandit0.





## Level 1

### Objective
Find the password for the next level.

### Commands Used

ls
cat readme

Explanation

The ls command lists the files in the current directory. It showed a file named readme.

The cat readme command displays the contents of the readme file. The contents contained the password required to log into Level 1.

After obtaining the password, I exited the Level 0 session and connected to the Level 1 account using SSH.






## Level 2

### Objective
Find the password for Level 3 in a file whose name contains spaces.

### Commands Used

ls -la
cat "./--spaces in this filename--"

Explanation

The ls -la command lists all files, including hidden files, with detailed information.

The password was stored in a file named --spaces in this filename--.

The cat command displays the contents of the file. The filename was enclosed in quotes because it contains spaces, and ./ specifies that the file is in the current directory.

The contents of the file provided the password for Level 3.






## Level 3

### Objective
Find the password for Level 4. The password is stored in a hidden file inside the `inhere` directory.

### Commands Used
ls
cd inhere
ls -la
find . -maxdepth 1 -type f -name '.*' -exec cat {} \;

Explanation

The ls command lists the files and directories in the current directory.

The cd inhere command changes the current directory to inhere.

The ls -la command lists all files, including hidden files. The -a option displays hidden files.

The find command searches the current directory for hidden files. The options used restrict the search to the current directory and identify files whose names begin with .. The -exec cat {} \; part displays the contents of the matching file.

The contents of the hidden file provided the password for Level 4.




## Level 4

### Objective
Find the password for Level 5. The password is stored in the only human-readable file in the `inhere` directory.

### Commands Used

ls
cd inhere
ls -la
file ./*
cat ./-file07

Explanation

ls lists the files and directories in the current directory.

cd inhere moves into the inhere directory.

ls -la displays all files, including hidden files, along with detailed information.

file ./* checks the type of each file in the directory. It helps identify which file contains human-readable text.

The file identified as ASCII text contains the password.

cat ./-fileXX displays the contents of that file. The displayed contents are the password for Level 5.



## Level 5

### Objective
Find the password for Level 6. The password is stored in a human-readable file that is exactly 1033 bytes in size and is not executable.

### Commands Used

cd inhere
find . -type f -size 1033c ! -executable
cat ./path/to/the/file

Explanation

cd inhere moves into the inhere directory.

The find command searches for files matching the required conditions.

. — searches from the current directory.
-type f — searches only for regular files.
-size 1033c — finds files that are exactly 1033 bytes (c means bytes).
! -executable — excludes executable files.

The resulting file contained the password, which was displayed using the cat command.



## Level 6

### Objective
Find the password for Level 7. The file is located somewhere on the server, is owned by user `bandit7`, belongs to group `bandit6`, and is exactly 33 bytes in size.

### Commands Used

find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password

Explanation

The find command searches the filesystem for a file matching specific conditions.

/ — searches the entire filesystem.
-user bandit7 — finds files owned by bandit7.
-group bandit6 — finds files belonging to group bandit6.
-size 33c — finds files exactly 33 bytes in size.
2>/dev/null — hides permission-denied error messages.



## Level 7

### Objective
Find the password for Level 8. The password is stored in `data.txt` next to the word `millionth`.

### Command Used

grep millionth data.txt

Explanation

The grep command searches for a specific word or pattern inside a file.

Here, grep millionth data.txt searches data.txt for the word millionth.

The matching line contains the password for Level 8.

Using grep avoids manually searching through the entire file.



## Level 9

### Objective
Find the password for Level 10. The password is stored in `data.txt` among human-readable strings and is preceded by several `=` characters.

### Command Used

strings data.txt | grep "==="

Explanation

strings data.txt extracts readable text from the binary data in the file.

The | (pipe) sends the output of strings to grep.

grep "===" searches the readable text for lines containing several = characters.

The matching output contained the password for Level 10.

## Level 10

### Objective
Find the password for Level 11. The password is stored in `data.txt` and is encoded using Base64.

### Command Used

base64 -d data.txt

Explanation
The base64 command is used to encode or decode Base64 data.

The -d option tells the command to decode the contents of data.txt.

## Level 11

### Objective
Find the password for Level 12. The password is stored in `data.txt`, where all uppercase and lowercase letters have been shifted by 13 positions using ROT13.

### Command Used

cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'

Explanation

The cat data.txt command displays the contents of the file.

The | (pipe) sends the output of cat to the tr command.

The tr command translates characters from one set to another.

A-Za-z represents all uppercase and lowercase letters.
N-ZA-Mn-za-m shifts each letter by 13 positions.



## Level 12

### Objective
Find the password for Level 13. The password is stored in a file that has been repeatedly compressed and archived.

### Commands Used

mkdir /tmp/bandit12
cp data.txt /tmp/bandit12/
cd /tmp/bandit12
xxd -r data.txt > data
file data

mv data data.gz
gunzip data.gz
file data

mv data data.bz2
bunzip2 data.bz2
file data

mv data data.gz
gunzip data.gz
file data

tar -xf data
file *

tar -xf data5.bin
file *

bunzip2 data6.bin
file data6.bin.out

tar -xf data6.bin.out
file *

gunzip -S .bin data8.bin
file data8

cat data8


Explanation

The xxd -r command converts the hexadecimal representation in data.txt back into binary data.

The file command identifies the type of the current file so that the appropriate decompression or extraction command can be selected.

gunzip decompresses gzip files.

bunzip2 decompresses bzip2 files.

tar -xf extracts files from a tar archive.

Because some files did not have standard extensions, the output filenames created by the decompression commands were used for the next step.

The process was repeated until file identified data8 as ASCII text.

Finally, cat data8 displayed the password for Level 13.

## Level 13

### Objective
Use the SSH private key provided in Level 13 to log in to Level 14.

### File Found

sshkey.private

### Command Used
ls -l sshkey.private
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private ~/sshkey.private
chmod 600 ~/sshkey.private
ssh -i ~/sshkey.private bandit14@bandit.labs.overthewire.org -p 2220

Explanation

sshkey.private is an SSH private key supplied by the Bandit Level 13 challenge.

scp was used to securely copy the private key from the Bandit server to my local Linux computer.

The -P 2220 option specifies the SSH port used by Bandit.

chmod 600 restricts access to the private key so that only the owner can read and modify it.

The ssh -i option tells SSH to use the specified private key for authentication.

Using this key, I successfully logged in as bandit14, completing Level 13 → Level 14.


## Level 14

### Objective
Find the password for Level 15 by submitting the Level 14 password to a service running on localhost port 30000.

### Command Used

nc localhost 30000

Explanation

nc (Netcat) is a networking utility that can establish connections to network services.



## Level 15

### Objective
Find the password for Level 16 by sending the Level 15 password to a service that requires an SSL/TLS connection.

### Command Used

openssl s_client -connect localhost:30001

### Explanation

`openssl s_client` creates an SSL/TLS connection to a server.

- `openssl` is a toolkit for SSL/TLS.
- `s_client` acts as an SSL/TLS client.
- `-connect localhost:30001` connects to the service running on port 30001.

After establishing the secure connection, I entered the Level 15 password.

The service verified the password and returned the password for Level 16.

### Result

The returned password was used to log in to Level 16.


## Level 16

### Objective
Find the SSH private key required to log in to Level 17.

### Commands Used

nmap -p 31000-32000 localhost

echo 'LEVEL16_PASSWORD' | openssl s_client -connect localhost:31790 -quiet 2>/dev/null

### Explanation
The `nmap` command scans ports 31000 through 32000 on the local machine and identifies the open ports.

The `openssl s_client` command connects to the SSL/TLS service running on port 31790.

The Level 16 password is sent to the service using `echo` and the pipe (`|`).

The correct service responds with:

Correct!
-----BEGIN OPENSSH PRIVATE KEY-----
...
-----END OPENSSH PRIVATE KEY-----

The SSH private key returned by the service is used to authenticate and log in to Level 17.

## Level 17

### Objective
Find the password for Level 18. The password is stored in `passwords.new` and is the only line that has changed between `passwords.old` and `passwords.new`.

### Command Used

diff passwords.old passwords.new

### Explanation
The `diff` command compares two files and displays the differences between them.

`passwords.old` contains the old passwords, while `passwords.new` contains the updated passwords.

The output shows the line that was changed. That changed line is the password for Level 18.



## Level 18

### Objective
Log in to Level 18 and find the password for Level 19. The SSH connection closes immediately after login because the `.bashrc` file is configured to log the user out.

### Command Used

ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"

### Explanation
Normally, SSH opens an interactive shell after logging in.

In Level 18, the connection is immediately closed by the `.bashrc` configuration.

Instead, we execute the `cat readme` command directly through SSH.

The `cat readme` command displays the contents of the `readme` file.

The output contains the password for Level 19.

## Level 19

### Objective
Find the password for Level 20. The `bandit20-do` program allows you to run a command as another user.

### Command Used

./bandit20-do whoami

./bandit20-do cat /etc/bandit_pass/bandit20

### Explanation
The `bandit20-do` program allows you to execute commands with the permissions of another user.

First, `./bandit20-do whoami` checks which user the command is being run as.

Then, `./bandit20-do cat /etc/bandit_pass/bandit20` reads the password file for Level 20.

The output of the second command is the password for Level 20.

## Level 20

### Objective
Find the password for Level 21. The `suconnect` program connects to a port on localhost, reads the Level 20 password, and if it matches, sends the password for Level 21. :contentReference[oaicite:0]{index=0}

### Commands Used

Terminal 1:

nc -vl localhost 50004

Terminal 2:

./suconnect 50004

### Explanation
First, `nc -vl localhost 50004` starts a Netcat listener on port 50004.

Then, `./suconnect 50004` connects to that listener.

After the connection is received, the Level 20 password is entered into the Netcat terminal.

If the password is correct, `suconnect` confirms the match and sends the password for Level 21. :contentReference[oaicite:1]{index=1}

The password received from `suconnect` is used to log in to Level 21.


## Level 21

### Objective
Find the password for Level 22. A cron job runs a script as `bandit22` every minute. The script copies the Level 22 password into a temporary file.

### Command Used

cat /etc/cron.d/cronjob_bandit22

### Explanation
The cron configuration shows that `/usr/bin/cronjob_bandit22.sh` is executed every minute as the `bandit22` user.

The script contains:

chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

This means the password for Level 22 is copied to:

/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

### Command to Get the Password

cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

If the file does not exist yet, wait for the cron job to run and try the command again.

The output is the password for Level 22.


## Level 22

### Objective
Find the password for Level 23. A cron job runs a script as `bandit23`. The script creates a temporary filename using an MD5 hash and copies the Level 23 password into that file.

### Command Used

cat /usr/bin/cronjob_bandit23.sh

### Explanation
The script contains:

myname=$(whoami)

mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

The cron job runs as `bandit23`, so `myname` becomes:

bandit23

Therefore, we calculate the MD5 hash of:

I am user bandit23

### Command Used

echo "I am user bandit23" | md5sum

### Result

8ca319486bfbbc3663ea0fbe81326349

The script therefore stores the Level 23 password in:

/tmp/8ca319486bfbbc3663ea0fbe81326349

### Command to Get the Password

cat /tmp/8ca319486bfbbc3663ea0fbe81326349

The output is the password for Level 23.


## Level 23

### Objective
Find the password for Level 24. A cron job runs a script as `bandit24` every minute. The script executes files in `/var/spool/bandit24/foo` if they are owned by `bandit23`.

### Command Used

cat /etc/cron.d/cronjob_bandit24

### Explanation
The cron configuration shows that `/usr/bin/cronjob_bandit24.sh` is executed every minute as `bandit24`.

We then examined the script:

cat /usr/bin/cronjob_bandit24.sh

The script works inside:

/var/spool/bandit24/foo

It checks whether a file is owned by `bandit23`. If it is, the script executes it as `bandit24` and then deletes it.

### Commands Used

cd /var/spool/bandit24/foo

echo 'cat /etc/bandit_pass/bandit24 > /tmp/bandit24_password' > /var/spool/bandit24/foo/getpass.sh

chmod 777 /var/spool/bandit24/foo/getpass.sh

### Explanation
We created a script named `getpass.sh` in the cron directory.

The script contains:

cat /etc/bandit_pass/bandit24 > /tmp/bandit24_password

Because the cron job executes our script as `bandit24`, it can read the Level 24 password and save it to `/tmp/bandit24_password`.

After waiting for the cron job to execute, we used:

cat /tmp/bandit24_password

The output is the password for Level 24.

