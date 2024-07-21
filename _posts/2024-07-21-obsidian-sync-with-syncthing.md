---
layout: post
title: Obsidian Sync With Syncthing
---

I recently started using [Obsidian](https://obsidian.md) on my android phone for my note-taking and journaling needs, after having found Notion to have too many features for my liking. I needed a simple markdown file editor which would house all my notes, and allow me to backlink related notes. The only caveat, which is also the neatest feature is that by default, obsidian works with local files only. Cloud sync is a paid subscription, so I couldn't access my notes from my laptop.

This is when I found [Syncthing](https://syncthing.net/). It is an open source, private, encrypted file synchronization program. Everything is stored on your devices and synced via a common WiFi network that your registered devices are connected to. This is perfect, because I rarely want to access my obsidian notes outside of my primary devices. Syncthing is surprisingly simple as easy to setup - you install it in the devices you want to sync, register both devices with each other over a common wifi network, choose the device you want to sync and you're done. In my case, I enabled Syncthing to sync the obsidian folder on my phone, and the web UI on my laptop automatically notified me if I wanted to sync the folder to my machine, created a folder in the location of my choosing, and synced it. I opened the folder in Obsidian and I'm good to go.

You can turn off syncthing once you're done, and turn it back on when you want to sync between devices again. This is an extremely well build program, which gave me zero friction to onboard and helped me achieve my intended task in seconds. A huge shout out to all the contributors!


### References
 - [Obsidian](https://obsidian.md)
 - [Syncthing](https://syncthing.net/)
