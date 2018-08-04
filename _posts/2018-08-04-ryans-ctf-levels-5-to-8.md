---
layout: post
title: "Ryan's CTF - Levels 5-8"
categories:
author: "stepMode"
meta: ""
tags: ctf
---

The *Intermediate* section focuses a lot more on the Linux terminal than the previous part. Therefore we are given a handy webshell in 3/4 challenges. Let's head right into it!

# Level 5 - Database Password

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level5_task.png"
            title="level5_task"
            caption="Level 5 Task" %}

<br>

The task provides us with an URL and login credentials. After accessing the page we get prompted to login with the given credentials.

<br>

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level5_webshell.png"
            title="level5_webshell"
            caption="Webshell" %}

<br>

Remembering the challenge task we can check if this machine has any MySQL services running, just to make sure what we are *attacking*.


<br>

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level5_mysql_process.png"
            title="level5_mysql_process"
            caption="MySQL Process" %}

<br>

Now that we know what we got, we can plan our strategy. I mean we already got access to the host running the database server, there has to be something we can do. We could just try to login to the server, maybe it is that easy.

```shell
~$ mysql -u competitor -p
Enter password:  
```

Oh well, we don't know the password of the user competitor. You might think it's `challenge1` but the credentials we used to access the webshell are not connected to the actual user inside the webshell. If we hit `Enter` without providing a password we get the following error.

```shell
ERROR 1045 (28000): Access denied for user 'competitor'@'localhost' (using password: NO)
```

<br>

Since this is a Linux system there should exist at least one other user that might be useful to us. The `root` user is usually the user with the UID (user identifier) 0, which on Linux (or Unix-like) systems makes it the superuser. Thus it comes with close to unlimited permissions. Systems like MySQL often reuse the term `root` to name their default/superuser. Let's see what happens when we try to login as `root`.

Note: We are trying to login as the root user of the MySQL database **not** the root user of the host machine.
```shell
~$ mysql -u root -p
Enter password:
```

Again a password prompt. **But** default installations of MySQL often don't set a password for root. So we just hit `Enter` again without providing a password.

<br>

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level5_mysql_shell.png"
            title="level5_mysql_shell"
            caption="MySQL shell" %}


<br>

There we go. We have a foot in. Now we need to get the password of the user `flag`.
Even though I think the commands are relatively selfexplainatory, I will go over them shortly.

```shell
mysql> SHOW DATABASES;
```

Simply prints out all available databases.

<br>

```shell
mysql> USE mysql;
```

To use the specified database.

<br>

```shell
mysql> SHOW TABLES;
```

Lists all tables in the current database.

<br>

```shell
mysql> SELECT * FROM user where user="flag";
```

This command returns all fields and their values for all users named `flag`.
And here is the output of the command.


{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level5_mysql_select_cmd.png"
            title="level5_mysql_select_cmd"
            caption="MySQL command to display the desired information" %}

<br>

```shell
+-----------+------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+---------
---+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------
------------+------------+--------------+------------------------+----------+------------+-------------+--------------+---------------+-------------+-----------------+----------------------+----------------------
-+-------------------------------------------+------------------+-----------------------+-------------------+----------------+                                                                                      
| Host      | User | Select_priv | Insert_priv | Update_priv | Delete_priv | Create_priv | Drop_priv | Reload_priv | Shutdown_priv | Process_priv | File_priv | Grant_priv | References_priv | Index_priv | Alter_pr
iv | Show_db_priv | Super_priv | Create_tmp_table_priv | Lock_tables_priv | Execute_priv | Repl_slave_priv | Repl_client_priv | Create_view_priv | Show_view_priv | Create_routine_priv | Alter_routine_priv | Creat
e_user_priv | Event_priv | Trigger_priv | Create_tablespace_priv | ssl_type | ssl_cipher | x509_issuer | x509_subject | max_questions | max_updates | max_connections | max_user_connections | plugin               
 | authentication_string                     | password_expired | password_last_changed | password_lifetime | account_locked |                                                                                      
+-----------+------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+---------
---+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------
------------+------------+--------------+------------------------+----------+------------+-------------+--------------+---------------+-------------+-----------------+----------------------+----------------------
-+-------------------------------------------+------------------+-----------------------+-------------------+----------------+                                                                                      
| localhost | flag | N           | N           | N           | N           | N           | N         | N           | N             | N            | N         | N          | N               | N          | N       
   | N            | N          | N                     | N                | N            | N               | N                | N                | N              | N                   | N                  | N    
            | N          | N            | N                      |          |            |             |              |             0 |           0 |               0 |                    0 | mysql_native_password
 | *25A3DA0F4740480EE31E435E03CE4B58A4F1263B | N                | 2018-08-01 19:06:23   |              NULL | N              |                                                                                      
+-----------+------+-------------+-------------+-------------+-------------+-------------+-----------+-------------+---------------+--------------+-----------+------------+-----------------+------------+---------
---+--------------+------------+-----------------------+------------------+--------------+-----------------+------------------+------------------+----------------+---------------------+--------------------+------
------------+------------+--------------+------------------------+----------+------------+-------------+--------------+---------------+-------------+-----------------+----------------------+----------------------
-+-------------------------------------------+------------------+-----------------------+-------------------+----------------+
```

<br>

That looks like a mess, so here is the important part

```mysql
| mysql_native_password | *25A3DA0F4740480EE31E435E03CE4B58A4F1263B |
```

It is a hash again. Remembering Level 3 we already have some experience with hashes. Let us try one of these [online crackers](https://crackstation.net/).

<br>

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level5_flag.png"
            title="level5_flag"
            caption="Flag Level 5 (value redacted)" %}


<br>

And we are already done with level 1  of the *Intermediate* category.


# Level 6 - Escape the Jail

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level6_task.png"
            title="level6_task"
            caption="Level 6 Task" %}

<br>

This time we are told to just read the file `flag.txt` located in `/root`.
So if we login and just try to print the file's content, we get an error, permission denied.

```shell
~$ cat /root/flag.txt
cat: /root/flag.txt: Permission denied
```

We need to escalate our privileges to access the file. There are a lot of popular linux enumeration scripts out there that run a bunch of commands to detect common misconfigurations, that allow a user to escalate privileges. From personal experience I can recommend [g0tmi1k's](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/) basic linux privilege escalation cheat sheet. One on the commands listed there is `sudo -l`.

It just shows a list of commands that our current user can run with sudo.

```shell
~$ sudo -l
Matching Defaults entries for competitor on 8e7491778326:
	env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
User competitor may run the following commands on 8e7491778326:
	(root) NOPASSWD: /usr/bin/vim                                                                                                                                                                                   
```

Looks like we can run vim (yay!) as superuser using sudo  and we won't be prompted for a password. Great!
At that point we could just `sudo vim /root/flag.txt` and access the flag file like this, but there's a *better* way. Vim is a powerful text based editor and allows us to execute commands from within in. If we open a new vim instance by running `sudo vim` we end up in the [vim command mode](http://vimdoc.sourceforge.net/htmldoc/cmdline.html#Command-line-mode). Now we just execute `:shell` and are dropped into a shell as root. Now we simply `cat` the flag file and are done with this challenge.

<br>

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level6_flag.png"
            title="level6_flag"
            caption="Flag Level 6 (value redacted)" %}

<br>


# Level 7 - Steganography

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level7_task.png"
            title="level7_task"
            caption="Level 7 Task" %}

<br>

No webshell this time, just an [image](/assets/2018-08-04-ryans-ctf-levels-5-to-8/hacker.jpg) and the challenge title already tells us what we are looking for.

[Steganography:](https://en.wikipedia.org/wiki/Steganography)  
> [...] is the practice of concealing a file, message, image, or video within another file, message, image, or video.

So we  have to look for a possible file or message hidden within the image, to obtain the flag. For the purpose of CTFs these pictures often hold archives or one is supposed to play with different image manipulation features like contrast or RGB. In the case of `hacker.jpg` the message was not hidden all that well. Running `strings` against it already revealed something suspicious. A base64 encoded string that when decoded gives us the flag.

<br>

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level7_flag.png"
            title="level7_flag"
            caption="Flag Level 7" %}

<br>

So let me try explain what happened. I will not dig too deep, just want to give a small overview of what the individual commands did.

```shell
strings hacker.jpg
```
Simply outputs strings of printable characters found in a file. And that's the output (at least part of it):

<br>

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level7_strings.png"
            title="level7_strings"
            caption="strings command" %}

<br>

It's on the last line, how convenient. We can use `tail` to get rid of all the *useless* output.

```shell
strings hacker.jpg | tail -n 1
```
`|` is called a pipe. It is used to redirect the output (`stdout`) of one program to another one as input (`stdin`). With `tail` we can output parts of the provided data (starting from the end) so if we provide the flag `-n 1` it prints the last line of the input. Looks like this:

<br>

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level7_tail.png"
            title="level7_tail"
            caption="tail command" %}

<br>

Now the only part that's left is decoding the string. We already did this in an earlier challenge.

```shell
strings hacker.jpg | tail -n 1 | base64 -d
```

I guess I don't need to explain that `base64` is used to `-d`ecode the given string. You already know what the output of the final command looks like so we're done with this challenge.

# Level 8 - What's the password

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level8_task.png"
            title="level8_task"
            caption="Level 8 Task" %}

<br>

We get another webshell and are told to *reverse-engineer* a program.

[Reverse Engineering](https://en.wikipedia.org/wiki/Reverse_engineering)  
> [...] is the process by which a man-made object is deconstructed to reveal its designs, architecture, or to extract knowledge from the object;

In this case the *object* seems to be a program.

After logging in we should just try to run the program and see what happens.

<br>

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level8_program.png"
            title="level8_program"
            caption="Running the program" %}

<br>

We are expected to provide the correct password so the program returns the flag. Obviously we don't have that password. `strings` worked quite well in the last challenge, we should try it again.

<br>

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level8_flag.png"
            title="level8_flag"
            caption="Flag Level 8" %}

<br>

Well...that was fast. Maybe we can build another chain of commands that only spits out the flag.

<br>

{% include image.html
            img="/assets/2018-08-04-ryans-ctf-levels-5-to-8/level8_flag_command.png"
            title="level8level8_flag_command"
            caption="grep-ing the flag" %}

<br>

This time it is up to you to figure out what the `grep` command does.

That's it already for the second blog post. Hope you had a good read and maybe even learned something.
