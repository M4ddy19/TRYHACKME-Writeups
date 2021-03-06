# SIMPLE CTF {TRY HACK ME}

![Theme Image](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/Theme.png)

Let's start by scanning the machine using nmap.

> sudo nmap -sSCV -vv {MACHINE IP}

![Scan Output](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/1.png)

As shown in the output we can login ftp by using ftp as username.

> ftp 10.10.162.108

After loging into ftp we found a directory named pub and in that directory we found a .txt file (ForMitch.txt). Let's get that file on our attacker machine.

![FTP Login](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/2.png)

It's a note for user Mitch

![Note For Mitch](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/3.png)

As we know port 80 is open and running http service, so let's open the webpage. It's a default apache webpage.

![Default Apache Webpage](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/4.png)

Let's run gobuster to find any hidden directory.

> gobuster dir -u http://{MACHINEIP} -w /PATH TO WORDLIST

![gobuster scan](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/5.png)

We found a hidden directory. Let's go to that directory and it's a CMS page. After scrolling down to the page we found the version of the CMS.

![Hidden Directory](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/6.png)

Now we can check for a CVE for CMS Made Simple Version 2.2.8 and we found a CVE on Exploit Database.

![CVE](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/7.png)

Q) What's the CVE you're using against the application?

A) CVE-2***-***3

Q) To what kind of vulnerability is the application vulnerable?

A) s***

Now we have a CVE so we can use it to get password. Let's download the CVE and crack the password.

> python 46635.py -u http://{MACHINE IP}/simple --crack -w /PATH TO WORDLIST

![Attack using CVE](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/8.png)

We can also use hydra to get password by using the userame Mitch we found in the ftp.

> hydra -l mitch -P /PATH TO WORDLIST ssh://MACHINE IP:2222

![Attack using Hydra](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/9.png)

Q) What's the password?

A) s****t

Q) Where can you login with the details obtained?

A) s**

Now we got the password let's login into ssh using the credentials.

> ssh mitch@MACHINE IP -p 2222

Let's first spawn a stable shell using python

> python -c 'import pty;pty.spawn("/bin/bash")'

![Spawn stable shell](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/10.png)

After listing all the files in user directory we found the user flag.

![User Flag](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/11.png)

Q) What's the user flag?

A) G*** ***, **** **!

We also found an another user in the home directory. 

![Another User](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/12.png)

Q) Is there any other user in the home directory? What's its name?

A) S*****h

Let's check for sudo privileges of the user Mitch.

> sudo -l

Mitch have sudo privileges for vim. 

We can use vim for privilege escalation

> sudo vim -c ':!/bin/bash'

![Privelage Escalation](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/13.png)

Q) What can you leverage to spawn a privileged shell?

A) v**

We are root now

Let's cd to root and get the root flag

![Root Flag](https://github.com/M4ddy19/TRYHACKME/blob/main/Simple-CTF/Images/14.png)

Q) What's the root flag?

A) W*** ****. *** **** **!
