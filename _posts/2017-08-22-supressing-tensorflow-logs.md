---
layout: post
title: Suppressing Tensorflow Logs
---

If you've used tensorflow to build your Machine Learning models or used it as a back-end with Keras, you've definitely encountered a whole lot of debugging info that's output to the terminal before your program begins execution. These are sometimes error messages, most of the time warnings and in some cases additional info as well and can be extremely useful. But in most cases all you really care about is observing the output from your program than wade through a whole lot of tensorflow jargon about how you can improve computation speed (which in fact is pretty important). 

Before we know how to suppress the debugging info that tensorflow outputs, let's know a bit about them. 
If you observe every message that tensorflow throws at you, you'll notice that they are prefixed with an I(Info), W(Warning) or an E(Error) that signifies what the message really is about.  Now what debugging info tensorflow is supposed to tell you is controlled by the `TF_CPP_MIN_LOG_LEVEL` environment variable. 

- The default value of `TF_CPP_MIN_LOG_LEVEL` is set to 0, which means output all logs. 
- Setting it to 1 will remove all the I(Info) logs from the debug messages.
- Setting it to 2 will supress W(Warning) messages in addition to effects of 1.
- And setting it to 3 suppresses all debugging info. This might not be a good thing to do at all times, because sometimes reading these logs will actually help you speed up computation or solve some bugs. But if all you temporarily want is for the program to give clean output without any of tensorflow's technical jargon then setting it to 3 will do the job.

How do we do this in python? Simple. Just add these two lines on top of your code.

```python
import os
os.environ["TF_CPP_MIN_LOG_LEVEL"] = 3
```
You can read this [stack overflow post](https://stackoverflow.com/questions/35911252/disable-tensorflow-debugging-information) for more.
