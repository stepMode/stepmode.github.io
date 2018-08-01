---
layout: post
title: "Ryan's CTF - Levels 5-8"
categories:
author: "stepMode"
meta: ""
tags: ctf
---

The *Intermediate* section focuses a lot more on the Linux terminal than the previous part. For that we are given a handy webshell in 3/4 challenges. Let's head right into it!

# Level 5 - Database password

{% include image.html
            img="/assets/2018-08-01-ryans-ctf-levels-5-to-8/level5_task.png"
            title="level5_task"
            caption="Level 5 Task" %}

<br>

The task provides us with an URL and login credentials. After accessing the page we get prompted to login with the given credentials.

<br>

{% include image.html
            img="/assets/2018-08-01-ryans-ctf-levels-5-to-8/level5_webshell.png"
            title="level5_webshell"
            caption="Webshell" %}

<br>

Remembering the challenge task we can check if this machine has any MySQL services running, just to make sure what we are *attacking*.


<br>

{% include image.html
            img="/assets/2018-08-01-ryans-ctf-levels-5-to-8/level5_mysql_process.png"
            title="level5_mysql_process"
            caption="MySQL Process" %}

<br>

Now that we know what we got, we can plan our strategy. I mean we already got access to the host running the database server, there has to be something we can do. We could try to just login to the server, maybe it is that easy.

```shell
~$ mysql -u competitor -p
Enter password:  
```

Oh well, we don't know the password of the user competitor. You might think it's `challenge1` but the credentials we used to access the webshell are not connected to the actual user inside the webshell.

Since this is a Linux system there should exist at least one other user that might be useful to us. The `root` user is usually the user with the UID (user identifier) 0, which on Linux (or Unix-like) systems makes it the superuser. Thus it comes with close to unlimited permissions. Let's see what happens when we try to login as `root`.

```shell
~$ mysql -u root -p
Enter password:
```
