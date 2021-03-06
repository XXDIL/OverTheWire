# OvertTheWire Bandit Wargame Hints:
Walkthrough of the bandit wargame at overthewire.org

![Bandit](https://user-images.githubusercontent.com/66634743/84546755-0c442d80-ad13-11ea-8afd-fdb7f17d2945.png)

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
```console
bandit5@bandit:~$ cd inhere
bandit5@bandit:~/inhere$ find . -type f -size 1033c # we can use find to search for a file of size 1033 bytes
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
```
### Level 6 ⟶ 7
```console
bandit6@bandit:~$ find . -type f -size 33c -user bandit7 -group bandit6
bandit6@bandit:~$ cat ./var/lib/dpkg/info/bandit7.password
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
```
Here we just append more conditions (group and user) to the find command.
* The '.' in the find command means, "in the current directory"

### Level 7 ⟶ 8
```console
bandit7@bandit:~$ grep "millionth" data.txt
cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```
'**grep**' is one of the most useful command to search for strings within text files and display the line containing the matched string

An alternat way is as follows (Easier):
```console
bandit7@bandit:~$ cat data.txt | grep "millionth"
cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```
* '**|**' symbol is called the '**pipe**' it is used to carry the output of the left command to the input of the right command

### Level 8 ⟶ 9
```console
bandit8@bandit:~$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```
* uniq -u compares each line to an adjacent lines and outputs the line if it is locally unique.
* To ensure that the line is globally unique, we run the sort command and pipe the output into uniq

You can even do the following:
```console
bandit8@bandit:~$ cat data.txt | sort | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```

### Level 9 ⟶ 10
```console
bandit9@bandit:~$ strings data.txt | grep =
========== the*2i"4
=:G e
========== password
<I=zsGi
Z)========== is
A=|t&E
Zdb=
c^ LAh=3G
*SF=s
&========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
S=A.H&^
```
Finding all ASCII strings in data.txt, we pipe that into a grep, that searches for '**=**'
* we know how our key looks like **truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk**

### Level 10 ⟶ 11
```console
bandit10@bandit:~$ base64 -d data.txt
IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
```
Decodes the base64 file and prints the output. (We could also use an online decoder.)

### Level 11 ⟶ 12
```console
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
```
There is a popular technique called 'rot13' cipher, used in Crypto.
We can optain a similar result using the **tr** function to 'rotate' the letters (like in rot13).
* A - N, B - O, C - P, ..., M - Z, N - A, O - B, ..., Z - M.

You can try it out yourself by replacing \<letter\> with any letter. 
```console
foo@bar:~$ echo <letter> | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

### Level 12 ⟶ 13

First you will need to create a tmp folder.(I'm calling it XXDIL, feel free to your tab :p)
```console
bandit12@bandit:~$ mkdir /tmp/XXDIL
bandit12@bandit:~$ cp data.txt /tmp/XXDIL #copy data.txt to the newly created folder (XXDIL)
bandit12@bandit:~$ cd /tmp/XXDIL
bandit12@bandit:/tmp/XXDIL$
```
Now we are ready to do the real work...
```console
bandit12@bandit:/tmp/XXDIL$ xxd -r data.txt data1
```
we will be using one of the following unzip methods:

* gunzip filename.gz
* bzip2 -d filename.bz2
* tar -xvf filename.bin

use the **file** command to find the type of file and use the right unzip method
```console
bandit12@bandit:/tmp/XXDIL$ file data1
data1: gzip compressed data, was "data2.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
bandit12@bandit:/tmp/XXDIL$ mv data1.gz  #changing the extention to .gz
```
notice the **gzip** word therefore we will use the gunzip (remember to add the right extensions).

Repeat above until ASCII file is obtained. (arount 8 files)
```console
bandit12@bandit:/tmp/XXDIL$ cat data8
8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
```

### Level 13 ⟶ 14
For this section you will need to understand how ssh keys work.
[Start here](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)
```console
bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
```

### Level 14 ⟶ 15
The command is like follows <br />
telnet \<host\> \<port\>
```console
bandit14@bandit:~$ telnet localhost 30000
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e # enter password
Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr

Connection closed by foreign host.
```

### Level 15 ⟶ 16
You will need a little bit of recon for this task. If you know anything about openssl, you will know s_client & s_server.
then look up s_client this will lead you to how its used.
```console
bandit15@bandit:~$ openssl s_client -connect localhost:30001
BfMYroe26WYalil77FoDi9qh59eK5xNr #enter password
Correct!
cluFn7wTiGryunymYOu4RcffSxQluehd

closed
```

### Level 16 ⟶ 17
```console
bandit16@bandit:~$ nmap -sV -p 31000-32000 localhost
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo           	# This is the one with ssl
31691/tcp open  echo
31790/tcp open  ssl/unknown		# even this one but whats 'unknown' (looks interesting)
31960/tcp open  echo
bandit16@bandit:~$ openssl s_client -connect localhost:31790
cluFn7wTiGryunymYOu4RcffSxQluehd
```

#### Output
-----BEGIN RSA PRIVATE KEY-----<br />
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ<br />
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ<br />
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu<br />
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW<br />
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX<br />
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD<br />
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl<br />
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd<br />
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC<br />
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A<br />
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama<br />
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT<br />
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx<br />
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd<br />
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt<br />
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A<br />
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi<br />
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg<br />
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu<br />
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni<br />
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU<br />
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM<br />
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b<br />
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3<br />
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=<br />
-----END RSA PRIVATE KEY-----<br />

Now we can use this key to get the ssh login.
```console
bandit16@bandit:~$ mkdir /tmp/lol
bandit16@bandit:~$ cd /tmp/lol
bandit16@bandit:/tmp/lol$ vim key
```
Copy past the key into the file then save it by 
* Press Esc and then type :wq!

```console
bandit16@bandit:/tmp/lol$ ssh -i key bandit17@localhost
bandit17@bandit:~$ cat /etc/bandit_pass/bandit17
xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn
```
### Level 17 ⟶ 18
```console
bandit17@bandit:~$ diff passwords.new passwords.old
```
### Level 18 ⟶ 19
<pre>
# In this level, the .bashrc logs one out immediately once an interactive session is started.
# Thus we read the password without opening an interactive bash shell
ssh -p 2220 bandit18@bandit.labs.overthewire.org "cat readme"
	kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
</pre>
### Level 19 ⟶ 20
<pre>
ssh -p 2220 bandit19@bandit.labs.overthewire.org
	IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x

./bandit20-do cat /etc/bandit_pass/bandit20 # ./bandit20-do runs a command as bandit20; from there we just use it to access the bandit20 password
</pre>
### Level 20 ⟶ 21
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
