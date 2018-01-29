---
layout: post
title: Running a block of python code within vim
---

I've been configuring vim to work as a python IDE and have added quite a few enhancements. There's a handy plugin called [python-mode](https://github.com/klen/python-mode) that really powers up your vim for python development. Some notable features include static code analysis, code completion, and executing the script with `<Leader> m` which prints the output in a new split.

But a lot of times what I really want to do is execute a few simple commands, or a simple for loop and checkout the results, and to do this I have to move out of vim to a separate window with a python interpreter. But that's not required at all!

I found this really cool [stack overflow answer](https://stackoverflow.com/questions/501585/how-can-you-use-python-in-vim) that explains a very handy trick.

So lets say we want to execute the following two lines of python code and check it's result.

```python
print "Hello World"
print 3//2
```

Just follow these simple steps after typing the above two lines in your vim window:

  1. Enter visual mode by pressing `Esc` and then `v`
  2. Use `j` and `k` or the arrow keys to select the lines you want to execute.
  3. Type `:!python`
  4. Since you are in visual mode, you will see this displayed as `:'<, '>!python`
  5. Press `Enter`

And that's it! The result will be displayed in-place and will replace the selected block of code! 

```python
print "Hello World"
print 3//2
```

Will now be replaced by 

```
Hello World
1
```

You can simply press `u` to undo this change and get back the lines of code you typed.

And that's not it. You can run bash commands too. 

  1. Type `ls -l`
  2. Enter visual mode and select the text
  3. Type `:!bash`
  4. Since you are in visual mode, you will see this displayed as `:'<, '>!bash`
  5. Press `Enter`
  6. Watch as the result is displayed in-place
  7. Press `u` to undo this change
  
I think that's a very handy feature! Hope that helps :)
