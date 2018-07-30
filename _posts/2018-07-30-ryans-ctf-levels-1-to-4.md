---
layout: post
title: "Ryan's CTF - Levels 1-4"
categories:
author: "stepMode"
meta: ""
tags: ctf
---
# Ryan's CTF - Levels 1-4

For my first blog post I chose [Ryan's CTF](http://ctf.ryanic.com/), in fact this is also my first write-up for a CTF in general so bear with me. The challenges range from *Beginner* to *Extrem* and at the end there is even a *NINJA!* challenge waiting for us. I will start with the first four challenges that represent the beginner section. Posts, containing all the other levels will be release in the near future. So let's start.

## Level 1 - Hidden Web Flag

![Challenge 1 - The Task]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level1_task.png" | absolute_url }})

We get the URL  [http://ctf.ryanic.com:8080](http://ctf.ryanic.com:8080) and are told to find the flag on the secret.html web page. Let's see what we get when visiting that page.

![secret.html]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level1_webpage.png" | absolute_url }})

> I love water!

Okay, that's nice but not really helpful. First thing I did at this point was checking the source of the page. Using Firefox or Chrome you can just add `view-source:` as prefix to the [URL](http://ctf.ryanic.com:8080/secret.html) or simply hit `CTRL+U`

![Flag (value redacted)]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level1_flag.png" | absolute_url }})

There we go, Level 1 done.

Hiding a flag in the page source is a really common beginner challenge and can be seen as taking the first step out of your comfort zone of just browsing the web.

## Level 2 - FTP File Transfer
