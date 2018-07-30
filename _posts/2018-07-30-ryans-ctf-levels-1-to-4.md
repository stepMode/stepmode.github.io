---
layout: post
title: "Ryan's CTF - Levels 1-4"
categories:
author: "stepMode"
meta: ""
tags: ctf
---

For my first blog post, or rather series of blog posts, I chose [Ryan's CTF](http://ctf.ryanic.com/). In fact, this is also my first write-up for a CTF in general so bear with me. The challenges range from *Beginner* to *Extrem* and at the end there is even a *NINJA!* challenge waiting for us. I will start with the first four challenges that represent the beginner section. Posts, containing all the other levels will be release in the near future. So let's start.

# Level 1 - Hidden Web Flag


![Challenge 1 - The Task]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level1_task.png" | absolute_url }})

<br>

We get the URL  [http://ctf.ryanic.com:8080](http://ctf.ryanic.com:8080) and are told to find the flag on the secret.html web page. Let's see what we get when visiting that page.

<br>


![secret.html]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level1_webpage.png" | absolute_url }})

<br>

> I love water!


Okay, that's nice but not really helpful. First thing I did at this point was checking the source of the page. Using Firefox or Chrome you can just add `view-source:` as prefix to the [URL](http://ctf.ryanic.com:8080/secret.html) or simply hit `CTRL+U`

<br>

![Flag (value redacted)]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level1_flag.png" | absolute_url }})

<br>

There we go, Level 1 done. <br>
Hiding a flag in the page source is a really common beginner challenge and can be seen as taking the first step out of your comfort zone of just browsing the web.

<br>
# Level 2 - FTP File Transfer


![Challenge 2 - The Task]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level2_task.png" | absolute_url }})

<br>


For the second challenge we are presented with a `.pcap` file (so a file containing a packet capture) and are told to find the flag in the captured network traffic of a FTP connection. Let's fire up Wireshark and have a look at it.

<br>

![Challenge 2 - pcap]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level2_pcap.png" | absolute_url }})

<br>

I'm not planning to explain Wireshark to you, it's free and a pretty great tool, you should play around with it yourself if you don't already know it. But just to get some basic information out there:


1. All captured packets (usually) in correct order
2. Contains the human readable representation of data of the selected packets
3. Displays the actual data in form of a hexdump

<br>

I guess we start by looking for interesting packets by just scrolling through the packet list. This shouldn't be too hard since FTP usually sends data in plaintext.
Scrolling through the packets we see a `USER nicholsr` and a `password ftppass`, but that is not what we're looking for. We want the flag for the points.

We can use Wireshark's search function (`CTRL+F`) and set the dropdown menu to the left side of the search bar to `Strings`. After that we just enter our search term `flag` and Wireshark highlights the next packet that contains the string `flag`. It's the request to the server to send over the data of the specified file (indicated by the `RETR Flag.txt` command)

A few packets later we see a packet that says `FTP Data: 28 bytes`. A closer look at the packet reveals those 28 bytes, and yes that's our flag.

<br>

![Challenge 2 - flag (value redacted)]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level2_flag.png" | absolute_url }})


{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level2_flag.png"
            title="level2_flag"
            caption="Flag Level 2" %}



<br>

Great, second challenge solved.

<br>
# Level 3 - Crack That Hash


{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level3_task.png"
            title="level3_task"
            caption="Level 3 Task" %}

<br>
