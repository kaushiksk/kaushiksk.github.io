---
layout: post
title: Always secure your MySQL servers.
---

I had to quickly rig up a MySQL server instance for a small DBMS project, so that me and my teammmate could collaborate seamlessly with the same data. Now because this was a temperory thing that was meant just for testing our app for a while, I just created an instance on Digital Ocean and set up a MySQL server with default credentials and allowed remote access from all hosts. Well, big mistake.

We didn't have any sensitive information and our tables weren't of much importance - thankfully. Because the next time I logged in to my server, all my databases were lost. There was a new database called `WARNING`, which had a table named `README` which had instructions to how to send bitcoins to a certain account to get back access to the backed up databases that were deleted. Well, that was a first.

But clearly, this was a mistake on my part, something that I shouldn't have overlooked. There are thousands of bots scanning the internet for MySQL server instances and cracking weak passwords isn't a big deal for them. By allowing access to all hosts I was asking for trouble. There are [tutorials](https://null-byte.wonderhowto.com/how-to/hack-databases-hunting-for-microsofts-sql-server-0148993/) on how you can use tools to search, exploit and take control of remote MySQL server instances.

Sensitive information or not, step 1 is always follow basic security measures. Lesson learnt. There is always someone waiting to exploit your resources. There was absolutely no reason for me to not spend 2 extra minutes to grant acces only to my teammates and my machine's IP address to access the db.

I completely uninstalled MySQL server, removed all associated files, installed it again, and made sure this time I provided access to specific remote hosts only.

For future reference, or for anyone reading this, once you've set up your MySQL server, ALWAYS grant access to specific remote IPs only. And ALWAYS have a strong password.

[This page](http://devdocs.magento.com/guides/v2.0/install-gde/prereq/mysql_remote.html) has clear instructions to setup a server instance with the proper access rights.

Security is not an option, it's the default course of action.

Don't repeat my mistake.
