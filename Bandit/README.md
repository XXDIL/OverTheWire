# OvertTheWire Bandit Wargame Hints:
Walkthrough of the bandit wargame at overthewire.org

Link to wargame: [Bandit](http://overthewire.org/wargames/bandit)

### Level 0 ⟶ 1
```console
foo@bar:~$ ssh bandit0@bandit.labs.overthewire.org -p 2220
           password bandit0
bandit0@bandit:~$ ls  #list the contents of the current directory
readme
bandit0@bandit:~$ cat readme # dispays the contents of the readme file
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```
### Level 1 ⟶ 2
```console
foo@bar:~$ ssh bandit1@bandit.labs.overthewire.org -p 2220
	         password boJ9jbbUNNfktd78OOpsqOltutMc3MY1
bandit1@bandit:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```
Here the password is in a file named "-". But this is a special character in bash,
meaning you cant just write "cat -" therefore we need to add the "./" before the name
this gives the location of the file.

#### Note :

##### By now you mush have grown acustom to the following ssh command

```console
foo@bar:~$ ssh banditX@bandit.labs.overthewire.org -p 2220
           password ******
```
* where X is the bandit we are going to ssh to 
* password will be retrieved in the previous bandit

### Level 2 ⟶ 3
```console
bandit2@bandit:~$ cat spaces\ in\ this\ filename
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```
The file containing the password has spaces, thus "\\" is used to escape the special <space> characters.you can even press tab after typing in the first few characters of the file name. 
But ts important to know that **"\\ "** is the space character

### Level 3 ⟶ 4
```console
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere
bandit3@bandit:~inhere$ ls
```
This will not show anything so why not use ls with some more porer **flags** use the "-la" flag:
```console
bandit3@bandit:~inhere$ ls -la
total 12
drwxr-xr-x 2 root    root    4096 May  7 20:14 .
drwxr-xr-x 3 root    root    4096 May  7 20:14 ..
-rw-r----- 1 bandit4 bandit3   33 May  7 20:14 .hidden
bandit3@bandit:~inhere$ cat .hidden
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
```

### Level 4 ⟶ 5

As mentioned in the site **human-readable file**

```console
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere
bandit4@bandit:~/inhere$ ls
-file00  -file02  -file04  -file06  -file08
-file01  -file03  -file05  -file07  -file09

bandit4@bandit:~/inhere$ file * # doing so will result in (No such file or directory).
```
This is because the files are starting with the "-" special character therefore the command should be
```console
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
bandit4@bandit:~/inhere$ cat ./-file07 # being the only ASCII text file
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
```
### Level 5 ⟶ 6
<pre>
ssh -p 2220 bandit5@bandit.labs.overthewire.org
	koReBOKuIDDepwhWk7jZC0RTdopnAYKh]

cd inhere
find . -type f -size 1033c #Here we use find to search for a file of size 1033 bytes (https://www.ostechnix.com/find-files-bigger-smaller-x-size-linux/)
cat ./maybehere07/.file2
</pre>
### Level 6 ⟶ 7
<pre>
ssh -p 2220 bandit6@bandit.labs.overthewire.org
	DXjZPULLxYr17uwoI01bNLQbtFemEgo7

find . -type f -size 33c -user bandit7 -group bandit6

# Here we just append more conditions (group and user) to the find command.

cat ./var/lib/dpkg/info/bandit7.password
</pre>
### Level 7 ⟶ 8
<pre>
ssh -p 2220 bandit7@bandit.labs.overthewire.org
	HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs

grep "millionth" data.txt
# grep is used to search for strings within text files and display the line containing the matched string {https://www.linode.com/docs/tools-reference/tools/how-to-grep-for-text-in-files/}
</pre>
### Level 8 ⟶ 9
<pre>
ssh -p 2220 bandit8@bandit.labs.overthewire.org
	cvX2JJa4CFALtqS87jk27qwqGhBM9plV
sort data.txt | uniq -u

# uniq -u compares each line to an adjacent lines and outputs the line if it is locally unique (compared to adjacent lines)

# To ensure that the line is globally unique, we run the sort command and pipe the output into uniq
</pre>
### Level 9 ⟶ 10
<pre>
ssh -p 2220 bandit9@bandit.labs.overthewire.org
	UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

strings data.txt | grep '^=\+'

# Finding all ASCII strings in data.txt, we pipe that into a grep search utilising regular expression.
# '^=\+' indicatest that the line should start wtih one or more "=" characters 
# {https://www.gnu.org/software/findutils/manual/html_node/find_html/grep-regular-expression-syntax.html}
</pre>
### Level 10 ⟶ 11
<pre>
ssh -p 2220 bandit10@bandit.labs.overthewire.org
	truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk

base64 -d data.txt 
# Decodes the base64 file and prints the output

# Some notes on base64: it is an encoding scheme that encodes binary to ACSII wehre each each base 64 digit represents 6 bits of data (since 2^6 == 64). When used on text, the chars are first converted into octets and bits before being base64 encoded {https://en.wikipedia.org/wiki/Base64#Examples}
</pre>
### Level 11
<pre>
ssh -p 2220 bandit11@bandit.labs.overthewire.org
	IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR

alias rot13="tr 'A-Za-z' 'N-ZA-Mn-za-m'"
# Define a function rot13 to perform the decryption
# A to Z is mapped to N-Z followed by A-M, the rot13 formula

cat data.txt  | rot13
# Pipe the input into the function and read the decrypted message
</pre>
### Level 12
<pre>
ssh -p 2220 bandit12@bandit.labs.overthewire.org
	5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu

# Set up work envt
mkdir /tmp/brandontang
cp data.txt /tmp/brandontang
cd /tmp/brandontang

# Revert Hex Dump
cat data.txt
xxd -r data.txt reverted

# Recursively Unzip the files; start by finding out what type of file it is.
file reverted

# Choose unzip method
gunzip filename.gz
bzip2 -d filename.bz2
tar -xvf filename.bin

# Repeat above until ASCII file is obtained
# Read off password
cat data8
</pre>
### Level 13
<pre>
ssh -p 2220 bandit13@bandit.labs.overthewire.org
	8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL

ssh -i sshkey.private bandit14@localhost
# Simply use the private key to login to the bandit 14 account

cat /etc/bandit_pass/bandit14
</pre>
### Level 14
<pre>
ssh -p 2220 bandit14@bandit.labs.overthewire.org
	4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e

telnet localhost 30000
# use telnet to connect to the host <localhost> on port <30000>
	4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e # nter password
</pre>
### Level 15
<pre>
ssh -p 2220 bandit15@bandit.labs.overthewire.org
	BfMYroe26WYalil77FoDi9qh59eK5xNr

# Connect to localhost at port 30001 with SSL
# open an SSL connection to localhost port 30001 and print the ssl certificate used by the service
openssl s_client -connect localhost:30001
	BfMYroe26WYalil77FoDi9qh59eK5xNr #enter password
</pre>
### Level 16
<pre>
ssh -p 2220 bandit16@bandit.labs.overthewire.org
	cluFn7wTiGryunymYOu4RcffSxQluehd

nmap -sV -p 31000-32000 localhost # Use Nmap to perform a service scan on ports 31000-32000

openssl s_client -connect localhost:31790 # Connect to port 31790
	cluFn7wTiGryunymYOu4RcffSxQluehd


# Output Credentials
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----



# Create a new temp directory and create a credentials file. Use it to ssh into the next level
mkdir /tmp/somename
cd /tmp/somename

echo "-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----" > bandit17key.private

ssh -i bandit17key.private bandit17@localhost
cat /etc/bandit_pass/bandit17 # Find out the actual password for bandit17
</pre>
### Level 17
<pre>
ssh -p 2220 bandit17@bandit.labs.overthewire.org
	xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn
diff passwords.new passwords.old
</pre>
### Level 18
<pre>
# In this level, the .bashrc logs one out immediately once an interactive session is started.
# Thus we read the password without opening an interactive bash shell
ssh -p 2220 bandit18@bandit.labs.overthewire.org "cat readme"
	kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
</pre>
### Level 19
<pre>
ssh -p 2220 bandit19@bandit.labs.overthewire.org
	IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x

./bandit20-do cat /etc/bandit_pass/bandit20 # ./bandit20-do runs a command as bandit20; from there we just use it to access the bandit20 password
</pre>
### Level 20
<pre>
ssh -p 2220 bandit20@bandit.labs.overthewire.org
	GbKksEFF4yrVs6il55v6gwY5aVje5f0j

tmux # create a new tmux session
printf "GbKksEFF4yrVs6il55v6gwY5aVje5f0j" | netcat -l -p 3001 # Start a port listener (daemon) on port 3001 which returns the current password to any connections that are made
[ctrl b + d] # Dettach from the tmux session

./suconnect 3001 # Connect to the port listener

tmux attach # Attach back to the tmux session
# The new password should be printed
</pre>
### Level 21
<pre>
ssh -p 2220 bandit21@bandit.labs.overthewire.org
	gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr

cd /etc/cron.d
ls
cat cronjob_bandit22
'''
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
'''

# We can deduce that the cron job is running /usr/bin/cronjob_bandit22.sh and redirecting output to NULL.


# Now re proceed to find out what /usr/bin/cronjob_bandit22.sh does
cat /usr/bin/cronjob_bandit22.sh
'''
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
'''

# The password is being written to /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv, thus we read the password from there
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
</pre>
### Level 22
<pre>
ssh -p 2220 bandit22@bandit.labs.overthewire.org
	Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI

cd /etc/cron.d
ls
cat cronjob_bandit23
cat /usr/bin/cronjob_bandit23.sh

'''
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget" #We need to find out the value of $mytarget when myname=bandit23

cat /etc/bandit_pass/$myname > /tmp/$mytarget
'''

# Simulating running the cronjob
myname="bandit23"
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
echo $mytarget
'''8ca319486bfbbc3663ea0fbe81326349'''


# Read off the file
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
</pre>
### Level 23
<pre>
ssh -p 2220 bandit23@bandit.labs.overthewire.org
	jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n

cd /etc/cron.d
ls
cat cronjob_bandit24
cat /usr/bin/cronjob_bandit24.sh 
cd /var/spool/bandit24

# We see that the script runs all scripts within /var/spool/bandit24 as bandit24
# thus we need to write a script to extract the password from /etc/bandit_pass/bandit24

# Use nano to write the following script to /var/spool/bandit24/script.sh
# Note that there is no point printing debugging statements because all std output is redirected to Null by the cron job
"""
#!/bin/bash
cat /etc/bandit_pass/bandit24 > bandit24pass 
"""

chmod +x script.sh
# Wait a while for the cronjob to run (~ 1 min or so), keep checking if bandit24pass is generated yet
cat bandit24pass
</pre>
### Level 24
<pre>
ssh -p 2220 bandit24@bandit.labs.overthewire.org
	UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ


# Create a tmp directory and cd there to do all the work
# Create a bash script to bruteforce the daemon (saved as bandit.sh)
"""
#!/bin/bash
for i in {1000..10000}
do
	echo $i
	printf "UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ $i\n" | netcat localhost 30002
done
"""

chmod +x bandit.sh # Make bandit.sh executable

# Pipe the file into the daemon and write output to the "output" file
# Note that letting the daemon write to stdout resulted in a time out error (for me)
./bandit.sh | netcat localhost 30002 > output 

cat output
# the password is on the last line
</pre>
