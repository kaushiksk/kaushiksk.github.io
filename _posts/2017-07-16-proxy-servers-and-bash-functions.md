---
layout: post
title: Proxy Servers and Bash Functions
---

Some of the perks from my [Student Developer Pack Pack](https://education.github.com/) from Github Education were sitting untouched so I finally decided to put it to good use. In case you didn't know the Student Developer Pack is a set of free tools and services provided by Github for the benefit of students. All you have to do is provide your Institute email address and your application will be processed and evaluated in no time and you get access to a ton of useful tools and services. If you haven't availed it yet, you should do it now!

Anyway, as part of the pack you get 50$ credit on [Digital Ocean](https://www.digitalocean.com/), and I set up a basic Ubuntu Server at 5$ per month. All I had to do was run some simple setup commands provided there, set up SSH and the server was up and running. After that it was just a matter of following instructions provided in this really nice [blog post](https://ma.ttias.be/socks-proxy-linux-ssh-bypass-content-filters/) to set up a SOCKS5 proxy using my newly created server. The post explains that all you need is a server to SSH into, then you can tunnel all your http/https requests to that server to bypass content filters imposed by your network. Basically, instead of your computer making internet requests, the proxy server does it for you, and sends you back the contents it fetched. The post explains all of this very neatly, I suggest you go through it and set up a socks5 proxy yourself, it's really simple and extremely useful too!

All that's fine, but the thing is, every time I have to set up an SSH tunnel to my server, I have to type this into my terminal :

`$ ssh -D 1337 -q -C -N user@server.ip.address`

That would create a SSH tunnel on port 1337. Now even for people who are into memorizing options and parameters in shell commands, that's quite a bit to remember, and you're guaranteed to forget it sooner or later. You could say, I'll just save it in a file and copy paste it whenever I need to run the server. But why do redundant work when you don't have to? Also, you can open multiple ssh tunnel at different ports, so there must be a novel way to get things done here?

Indeed, what we need are *bash functions!*
These are like functions in any other language, you give it a name, define what it has to do (which in this case, are a series of bash commands) and put it in a place that makes it accessible in the terminal.

Here's how I define it. I call my function **myproxy**, add the command to start an SSH tunnel at any port, but use 1337 as default and then simply print out a confirmation message.
```
myproxy () {
  ssh -D ${1-1337} -q -C -N user@server.ip.address;
  echo "Tunneling on port ${1-1337}"
}
```

First, note the whitespace between *myproxy* and the *(*, it's mandatory and part of the syntax. Secondly, `${1-1337}` is just the bash way of saying, if the user supplies an additional argument to the function, which in this case is assumed to be the port number, use that value here, else, use the default value 1337.

So, `$ myproxy 5550` would open up an SSH tunnel in port 5550 and output "Tunneling at port 5550". Whereas, `$myproxy` would open up an SSH tunnel in port 1337 (the default value) and output "Tunneling at port 1337".

Now I just copy paste this code into the end of the `~/.bashrc` file, which just contains a set of bash commands which are executed each time you open a terminal, making available the variables and functions defined in the file in that terminal.

And, with minimum additional work, I think we've achieved quite a bit of efficiency and done away with a lot of redundancy.

Happy ssh-ing until next time!
