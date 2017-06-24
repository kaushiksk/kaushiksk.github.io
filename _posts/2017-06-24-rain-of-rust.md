---
layout: post
title: Rain of Rust @ RedHat, Bangalore
---


Today, I attended a [#RainofRust](https://blog.mozillaindia.org/1932) event at the RedHat office in Bangalore. It was an open meet-up where Mozilla volunteers introduced us to the applications of [Rust](https://www.rust-lang.org/), a systems programming language sponsored by Mozilla.

Rust has recently gained a lot of popularity, thanks to it's in-built support for memory management and concurrency which have led major companies like Dropbox and Coursera to move their backends to Rust.

We were taught about the basic syntax of Rust, compiling and running Rust code, cargo package manager, various datatypes and constructs and it's applications. There was also a live demo to set up a server that handles mongodb requests. Developers from startups like [Ather](https://www.atherenergy.com/) talked about how they have begun to use Rust to handle the server side code for collecting data from their various devices in real time.

What interested me the most is how you could build python modules with Rust! Instead of say, writing a module to compute the nth fibonacci number in Python, you write that code in Rust and compile it to be python compatible module to import and use in your Python scripts and this drastically improves the speed and performance thanks to Rust's robustness!

One of the speakers talked about how he has been using Python modules written in Rust to speed up the various preprocessing tasks for his AI projects and this was really interesting! I'm really looking forward to working with Rust, especially in this particular direction and hope to do some meaningful contributions in the future. 

You can get started with Rust by going through the demo codes in the [RustIndia Github repo](https://github.com/MozillaIndia/RustIndia).
