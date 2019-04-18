---
layout: post
title: "Ryan's CTF - Levels 1-4"
categories:
author: "ruly"
meta: ""
tags: ctf
---

For my first blog post, or rather series of blog posts, I chose [Ryan's CTF](http://ctf.ryanic.com/). In fact, this is also my first write-up for a CTF in general so bear with me. The challenges range from *Beginner* to *Extrem* and at the end there is even a *NINJA!* challenge waiting for us. To be honest, all of the challenge were very well doable even for a relative beginner like me, but the difficulty is perfect for people just starting out with CTFs.

While I am at it, thanks to both [ryanic](https://www.ryanic.com/) for hosting and creating the challenges as well as [John Hammond](https://www.youtube.com/user/RootOfTheNull/) for bringing them to my attention. He also released a series of videos on his YouTube channel solving Ryan's CTF. Go check him out, he produces great content. (And no, I did not use his videos to solve the challenges, that would ruin the fun for me. Enjoyed them afterwards though)

I will start with the first four challenges that represent the beginner section. Posts, containing all the other levels will be release in the near future. So let's start.

# Level 1 - Hidden Web Flag


<!-- ![Challenge 1 - The Task]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level1_task.png" | absolute_url }}) -->

{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level1_task.png"
            title="level1_task"
            caption="Level 1 Task" %}

<br>

We get the URL  [http://ctf.ryanic.com:8080](http://ctf.ryanic.com:8080) and are told to find the flag on the secret.html web page. Let's see what we get when visiting that page.

<br>


<!-- ![secret.html]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level1_webpage.png" | absolute_url }}) -->

{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level1_webpage.png"
            title="level1_webpage"
            caption="Webpage" %}

<br>

> I love water!


Okay, that's nice but not really helpful. First thing I did at this point was checking the source of the page. Using Firefox or Chrome you can just add `view-source:` as prefix to the [URL](http://ctf.ryanic.com:8080/secret.html) or simply hit `CTRL+U`

<br>

<!-- ![Flag (value redacted)]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level1_flag.png" | absolute_url }}) -->

{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level1_flag.png"
            title="level1_flag"
            caption="Flag Level 1 (value redacted)" %}

<br>

There we go, Level 1 done. <br>
Hiding a flag in the page source is a really common beginner challenge and can be seen as taking the first step out of your comfort zone of just browsing the web.

<br>
# Level 2 - FTP File Transfer


<!-- ![Challenge 2 - The Task]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level2_task.png" | absolute_url }}) -->

{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level2_task.png"
            title="level2_task"
            caption="Level 2 Task" %}

<br>


For the second challenge we are presented with a `.pcap` file (so a file containing a packet capture) and are told to find the flag in the captured network traffic of a FTP connection. Let's fire up Wireshark and have a look at it.

<br>

<!-- ![Challenge 2 - pcap]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level2_pcap.png" | absolute_url }}) -->

{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level2_pcap.png"
            title="level2_pcap"
            caption="Level 2 pcap" %}

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

<!-- ![Challenge 2 - flag (value redacted)]({{ "/assets/2018-07-30-ryans-ctf-levels-1-to-4/level2_flag.png" | absolute_url }}) -->


{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level2_flag.png"
            title="level2_flag"
            caption="Flag Level 2 (value redacted)" %}



<br>

Great, second challenge solved.

<br>
# Level 3 - Crack That Hash


{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level3_task.png"
            title="level3_task"
            caption="Level 3 Task" %}

<br>

So we get a hash and we need to crack it. Looks like we need to power our cracking rack and run hashcat against it, or do we? The challenge does not specify what kind of hash it is and to actually tackle it with hashcat we need to provide a hash function. To me it looks like an md5 hash, but that's just something that builds over time. To make sure we can ~~google~~ `<insert favourite search-engine>` the hash and see if we get some useful results.

<br>

{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level3_flag.png"
            title="level3_flag"
            caption="Flag Level 3 (value redacted)" %}

<br>

Well, that was easy. In CTF like these it's usually not required to crack difficult hashes. It's more likely to be hash that is well known and can be found via `<insert favourite search-engine>` or one of the online cracker tools [e.g. https://hashkiller.co.uk/ or https://crackstation.net/] will give it to you.

If you're interested in *actual* hash cracking though, I recommend you take a look at `hashcat`, it's an awesome program and they have a nice community over at their [forums](https://hashcat.net/forum/), with lots of smart people. But remember to stay legal!

<br>
# Level 4 - Encoded Credentials


{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level4_task.png"
            title="level4_task"
            caption="Level 4 Task" %}

<br>

Last challenge in the *Beginner* area.

So the task gives us a `HTTP GET request` and we can see the basic access authentication header field - `Authorization: Basic YWRtaW46dW5pY29ybmlj` - holds a value. If we look at the relevant [RFC 7617](https://tools.ietf.org/html/rfc7617) we can see the value is only base64 encoded and **not** encrypted (encoding != encryption).

<br>

{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level4_rfc7617.png"
            title="level4_rfc7617"
            caption="RFC 7617: The 'Basic' HTTP Authentication Scheme" %}

<br>

Since it's just encoding and a really common and well known too, we can easily decode it to retrieve a more useful representation of the data. There are plenty of online tools that do exactly this, you could also write a little script, for example in python, that decodes that data for you or just use your favourite command line / terminal.

<br>

{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level4_dec_base64_ps.png"
            title="level4_dec_base64_ps"
            caption="Example commands to decode base64 in PowerShell (flag value redacted)" %}

<br>

{% include image.html
            img="/assets/2018-07-30-ryans-ctf-levels-1-to-4/level4_dec_base64_bash.png"
            title="level4_dec_base64_bash"
            caption="Example command to decode base64 in a Linux terminal (flag value redacted)" %}

<br>

That's it, we solved all challenges in the first category. In the next post we will start with the *Intermediate* section.
