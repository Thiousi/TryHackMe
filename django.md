# Introduction to Django

## Room
This a Try Hack Me room called `Introduction to Django` 

The Web address for this room is

https://tryhackme.com/room/django

## Synopsis.
How it works and why should I learn it?


---

## Getting started

You should absolutely read and perform Tasks 1 through 4, this is a great crash course on django... and you'll need the knowledge for the last task!

Make sure you are connected to the Try Hack Me VPN before starting!

The first thing I do when starting a room is to note the victim and attacker's IP as given by Try Hack Me.

```
export MYIP=10.9.32.24
export IP=10.10.159.95
```

This format allows me to copy and paste in any terminal and use `$IP` anytime I need the victim's IP or `$MYIP` if I need my own

---

## Unit 5 - CTF 

Now it's time for a small CTF!

```
It might take around ~3 minutes for the machine to boot properly.
```

Fix the error and retrieve all the flags! (Use knowledge from previous units)

```
Username: django-admin
Password: roottoor1212
```

---

### Enumeration

Before jumping in with the credentials given above, it's good practice to run at least some enumeration.

```
nmap -sC -sV -oN nmap/initial $IP
```
*If you're unfamiliar with these commands, I highly recommend the great [RP: NMAP room](https://tryhackme.com/room/rpnmap).*

```
22/tcp   open  ssh      OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 35:30:91:45:b9:d1:ed:5a:13:42:3e:20:95:6d:c7:b7 (RSA)
|   256 f5:69:6a:7b:c8:ac:89:b5:38:93:50:2f:05:24:22:70 (ECDSA)
|_  256 8f:4d:37:ba:40:12:05:fa:f0:e6:d6:82:fb:65:52:e8 (ED25519)
8000/tcp open  http-alt WSGIServer/0.2 CPython/3.6.9
```
We see SSH running on port 22, no surprise there, and http-alt running on 8000.

And by the way, don't waste any time trying to look for Trinity, she's lost in the matrix
```
<meta name="robots" content="NONE,NOARCHIVE">
|     <title>DisallowedHost
|     /nice ports,/Trinity.txt.bak</title>
```

If we go to `http://<Machine IP>:8000` we see a Django page with lots of messages about DISALLOWED HOSTS. Rings a bell from the tasks above? We should add the machine IP to the hosts before we can access the app. Let's do that.

### #1  Admin panel flag?

We are given the credentials to SSH to the machine:

```
Username: django-admin
Password: roottoor1212
```

This gives us the following command:
```ssh django-admin@$IP```

We are in! 

From the great explanations on how to use Django in the earlier tasks of this room, we know quite a lot.

The first thing is that we should change our ALLOWED_HOSTS in the app's settings.py

we can use nano to edit the file
```nano settings.py```

Change this line to include the IP of the machine:
```
ALLOWED_HOSTS = ['0.0.0.0', '10.10.147.62']
```

`Ctrl+X` to exit and Y to save the changes.

You should now be able to access the website at `<machine IP>:8000`

go to the admin page at `<machine IP>:8000/admin`

Now how do we get in? If you read the first part of this room, you should have seen that we can create a super user. That should do!

Navigate to `/home/django-admin/messagebox/`

Execute the command to create the superuser

```
python3 manage.py createsuperuser
```
Follow the prompts to create a username and password and you now have a super user account.

Back on the admin page, you can use those credentials to login.

Once in, navigate through the not so many options and you should see a Users page.

Here's your first flag disguised as the First Name of the user Flag.


### #2  User flag?

There's the long route... and the short route version.

Once you are logged in as django-admin and run `ls -la /home` you can see that you have access to another user's home folder: StrangeFox. Let's get his flag!

```
ls -la /home
cd /home/StrangeFox
ls
cat user.txt
THM{xxx}
```


Now if you didn't think of looking there, the admin panel flag from above gave you a great hint: 
```
StrangeFox         Password hash: https://pastebin.com/nmKt4BSf
```

Opening the url, we find a long hash. Copy it into an online hash cracking tool (easy one: https://crackstation.net/) and you will find StrangeFox's password. 

You can now either :
1. Open a new ssh as StrangeFox `ssh Strangefox@$IP`
2. change your current user `su StrangeFox`

Cnce StrangeFox, you can see the content of its home folder and `cat user.txt` to get the flag.


### #3 	Hidden flag?

There are many ways to get this flag but showcasing grep is what I chose:

```
cd /home
grep -r 'THM'
django-admin/messagebox/messagebox/home.html:   <!-- Flag 3: THM{xxxxx_xxxxxxx} -->
Binary file django-admin/messagebox/db.sqlite3 matches
Binary file django-admin/.cache/mozilla/firefox/u9lfuwen.default/cache2/entries/B3EBE75A12F34D5C99BEE109D78C33D01D396538 matches
```

And we are done! What a great room to learn!
