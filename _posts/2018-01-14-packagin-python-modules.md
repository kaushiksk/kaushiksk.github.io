---
layout: post
title: Packaging a python module for the first time
---

After a really insightful Number Theory and Cryptography course last semester I decided to implement RSA from scratch in python. This was a really good exercise as it allowed me to code all the building blocks and then bring them together.

I also dived a bit deeper into the concept of private variables in Python and how they've been intended to be used. In truest sense, python has no private variables, they are all still accessible, but it's assumed that letting the developers know that certain variables are not to be meddled with is enough and that there is no need to provide exclusive security. This [stackoverflow thread](https://stackoverflow.com/questions/1641219/does-python-have-private-variables-in-classes#1641236) provides some valuable insights.

Python also has it's own version of setter and getter functions in the form of `@property` function. I came across this [blog post](https://codefisher.org/catch/blog/2015/05/17/python-property-function-why-no-private-methods/) that explains this is considerable detail and finesse. I ended up incorporating all of these into my RSA implementation. 

Now that I had a decent module ready, I thought it was a good time to learn package it for easy installation for anybody who wants to use it. It turned out to be easier that I thought. This [website](https://python-packaging.readthedocs.io/en/latest/minimal.html) covers everything you would want to know with a lot of clear examples. It is really just as simply as editing a `setup.py` template. My major challenge was to get the code running on both python 2 & 3 uniformly without bugs, and I was able to fix this after a few hickups.

All in all, this was a really good learning experience. You can install the [rsasim](https://github.com/kaushiksk/rsasim) module from my github page. Feel free to play around with the source code and let me know if you come across any bugs. I've spent a lot of time writing inline documentation, README as well as examples, so using it shouldn't pose any problems. 

I've also created another repository [rsa-from-scratch](https://github.com/kaushiksk/rsa-from-scratch/) where you can implement rsa from scratch in your favourite language and send a PR.

Here's to all the future python packages to come!

### References
 - [Packaging Python Modules](https://python-packaging.readthedocs.io/en/latest/minimal.html)
 - [@property](https://codefisher.org/catch/blog/2015/05/17/python-property-function-why-no-private-methods/)
 - [Private variables in python](https://stackoverflow.com/questions/1641219/does-python-have-private-variables-in-classes#1641236)
 - [rsasim](https://github.com/kaushiksk/rsasim/)
