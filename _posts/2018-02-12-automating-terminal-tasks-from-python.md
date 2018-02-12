---
layout: post
title: Automating terminal tasks using python with os and subprocess
---

This [vimcast](http://vimcasts.org/episodes/synchronizing-plugins-with-git-submodules-and-pathogen/) explains a really nice way to synchronize your vim plugins using git and Pathogen - the package manager for vim. 

You organize your `.vim` folder as a git repository and add new plugins as submodules via git. Then all you need to do is clone the repo on a new machine and pull all the submodules and since we have pathogen installed all the plugins are now available! You can read the transcript of the vimcast for more details.

But my problem was that I already had about 20-25 plugins installed as git repositories which meant that i had to individually add them as submodules by first noting their remote repo addresses. For this, I need to go into every plugin's directory - which itself is a git repo - and run `git remote -v`, grab the remote url and then navigate back to `~/.vim` and run 

`$ git submodule add remote-repo-url local/dest/of/plugin`. 

Perfect task for python to automate!

I use `os` and `subprocess` modules in Python2 which are available by default. So what we need to do is simple. Enter every plugin directory, grab the remote url by running `git remote -v`, navigate back to the `/.vim` directory and add that repo as a submodule.

We'll use the following three functions :
 - **os.chdir(path)**                     : This is equivalent to `cd path`.
 - **os.listdir(path)**                   : returns a list of directory in `path`
 - **subprocess.call(command)**           : runs `command` as a subprocess
 - **subprocess.check_output(command)**   : runs `command` as subprocess and returns output. Note that it is mandatory to pass the command as a list made up of every word in the overall command. I'll make it clear when we use it below.

So we will first import the necessary modules and set up some default values.
```python
import os
import subprocess

HOME = "/home/kaushiksk/" # Change this to your home folder
os.chdir(HOME + ".vim/")
bundle_folder = "./bundle/"
```

I set the home folder to my home, changed to my `.vim` directory and set the path to the `bundle` directory where all the plugins reside.

Next, we'll get the list of directories inside `bundle`.

```python
modules = os.listdir(bundle_folder)
```

Now for every directory(plugin) in the list, we need to first change to that directory.

```python
os.chdir(bundle_folder + module)
```

We now get the remote url for this plugin - which is a git repo:

```python
out = subprocess.check_output(["git", "remote","-v"])
```

Notice how I passed the command as a list. We now have the output from that command.
On your terminal running `$ git remote -v` should give an output like this
```
origin  https://github.com/user/repo.git (fetch)
origin  https://github.com/user/repo.git (push)
```
because the terminal renders tabs and newlines.

Our ouput though will look something like this:

`'origin\thttps://github.com/user/repo (fetch)\norigin\thttps://github.com/user/repo (push)\n'`

That's because what we're getting is as a python string.

We just need the repo url. That's just `out.split()[1]` - split at whitespace (space, tab, newline) and return a list and choose the second element in the list.

We'll now change back to the `~/.vim` directory because that's the root of our
git repo. 
```python
os.chdir(HOME + ".vim/")
```

And finally we will use the url we just extracted and add it as a submodule.
```python
subprocess.call(["git", "submodule", "add", out.split()[1], bundle_folder +
module ])
```

The whole script will look like this
```python
import os
import subprocess

HOME = "/home/kaushiksk/" # Change this to your home folder
os.chdir(HOME + ".vim/")
bundle_folder = "./bundle/"
modules = os.listdir(bundle_folder)

for module in modules:
    os.chdir(bundle_folder + module)
    
	out = subprocess.check_output(["git", "remote","-v"])
	os.chdir(HOME + ".vim/")
	subprocess.call(["git", "submodule", "add", out.split()[1], bundle_folder + module ])
	print "Done adding ", module
```

Of course, this assumes that every plugin directory under the `bundle` is a git repo. We'll add a simple try catch just in case.
So the final code with error handling:

```python
import os
import subprocess

HOME = "/home/kaushiksk/" # Change this to your home folder
os.chdir(HOME + ".vim/")
bundle_folder = "./bundle/"
modules = os.listdir(bundle_folder)

for module in modules:
    os.chdir(bundle_folder + module)
    try:
        out = subprocess.check_output(["git", "remote","-v"])
        os.chdir(HOME + ".vim/")
        subprocess.call(["git", "submodule", "add", out.split()[1], bundle_folder + module ])
        print "Done adding ", module

    except:
        print "Leaving ", module, ". Not a git repository"
		os.chdir(HOME + ".vim/")
```

And that's it!

### References
 - [Synchronizing plugins with git submodules and pathogen](http://vimcasts.org/episodes/synchronizing-plugins-with-git-submodules-and-pathogen/)
 - [Gist to the script](https://gist.github.com/kaushiksk/48381494302de2b158ea7f083fd90a32)
