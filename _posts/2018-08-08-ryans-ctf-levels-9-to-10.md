---
layout: post
title: "Ryan's CTF - Levels 9-10"
categories:
author: "stepMode"
meta: ""
tags: ctf
---

Since I only want to publish finished posts and free time is sparse at the moment, I reduced the post size from 4 challenges to 2.  
Therefore this post will be about Level 9 and 10 of the *Advanced* section. Two very similar challenges that let us explore the command line a bit more.


# Level 9 - Find the Flag(.txt)

{% include image.html
            img="/assets/2018-08-08-ryans-ctf-levels-9-to-10/level9_task.png"
            title="level9_task"
            caption="Level 9 Task" %}

<br>

After logging in we notice really fast that executing commands seems to be not possible.

<br>

{% include image.html
            img="/assets/2018-08-08-ryans-ctf-levels-9-to-10/level9_no_cmd_exec.png"
            title="level9_no_cmd_exec"
            caption="No command execution" %}

<br>

Looking for `alias` it does show some but those seem to be not working aswell. Even the environmental variable `$PATH` seems to be broken.

<br>

{% include image.html
            img="/assets/2018-08-08-ryans-ctf-levels-9-to-10/level9_alias_path.png"
            title="level9_alias_path"
            caption="alias and $PATH" %}

<br>

Maybe we are able to execute commands if we specify their absolute path.

<br>

{% include image.html
            img="/assets/2018-08-08-ryans-ctf-levels-9-to-10/level9_absolute_path.png"
            title="level9_absolute_path"
            caption="Using absolute paths to execute commands" %}

<br>

There we go, now  we can just look for `Flag.txt` using `/usr/bin/find` and print the content with `/bin/cat`.

<br>

{% include image.html
            img="/assets/2018-08-08-ryans-ctf-levels-9-to-10/level9_flag.png"
            title="level9_flag"
            caption="Flag Level 9 (value redacted)" %}

<br>

Notice how I use the output of the `find` command as input for `cat`. This is called [command substitution](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html) and it is rather handy from time to time.

# Level 10 - Find the Flag(.txt) Again

{% include image.html
            img="/assets/2018-08-08-ryans-ctf-levels-9-to-10/level10_task.png"
            title="level10_task"
            caption="Level 10 Task" %}

<br>

The name already tells us it is going to be similar to Level 9. Let us see how similar.

<br>

{% include image.html
            img="/assets/2018-08-08-ryans-ctf-levels-9-to-10/level10_flag.png"
            title="level10_flag"
            caption="Flag Level 10 (value redacted)" %}

<br>

Well, I would say they are pretty close. I am not sure if these are the intended ways to solve the challenges but it worked.
