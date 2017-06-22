---
layout: post
title: Linkup - Bookmarking web links from the command line
---



**The entire code for the tool discussed in this post can be found [here](https://github.com/kaushiksk/linkup).**


Say you're reading about The Beatles today. You read from about 3-4 resources online and listen to a few songs on Youtube. Or maybe you were just reading up on your favorite topic.

-  Wouldn't it be nice if a month later you could just use a few keywords to load all those links at once ?
-  Wouldn't it better if you could somehow group them into topics and subtopics? Say, "Beatles Bio" would load their wiki and some other relevant pages, "Beatles songs" would load the Youtube links you had visited.
- And wouldn't it be even better if when someone asked you "Who are The Beatles?", you could easily share all these links with them immediately?


So I thought I'll just build a tool that does this. All we need is Python's `json` and `webbrowser` modules.

```python
import json
import webbrowser
```

- **json** : Python module to serialize data in JSON format. Good thing about JSON files is they are stored as plain text, meaning you can read them with any text editor. Also when loaded, they behave exactly like Python dictionaries! You can read more on [JSON and other serialization techniques in Python](http://www.diveintopython3.net/serializing.html).

- **webbrowser** : A simple python module that lets you open web pages and do some basic browser control.

So the structure is going to be like this.  

Each web link belongs to a certain topic and subtopic. We create topic-wise json files, and in them store links subtopic-wise.

So the `beatles.json` file should look like this:
```
{
  "bio": [
    "http://sgtpepper.thebeatles.com/",
    "https://en.wikipedia.org/wiki/The_Beatles"
  ],
  "songs": [
    "https://www.youtube.com/user/TheBeatlesVEVO"
  ]
}
```

Now loading all these links in your browser is easy. Let's write a function for that. We'll assume the JSON file already exists for now. Notice that all links in a subtopic are saved as a list.

```python
topic = "beatles"
base_file = topic + ".json"
subtopic = "bio"

def openlinks(base_file,subtopic):
    with open(base_file, mode='r', encoding='utf-8') as f:
        entries = json.load(f) # entries is now a dictionary
    for link in entries[subtopic]: # iterate through every link in subtopic
        webbrowser.open(link)
```
If we want to open *all* the links in a topic:

```python
def openall(base_file):
    with open(base_file, mode='r', encoding='utf-8') as f:
        entries = json.load(f)
    links = [] # Empty list to hold all links
    for subtopic in entries.keys(): # Go through every subtopic
        links = links +  [link for link in entries[subtopic]] #Append all links to a single link
    for link in links: # Open all links
    	 webbrowser.open(link) 
```
That wasn't so hard! Now all we need to is to be able to generate these JSON files and modify them as well. We'll write a function for that.

```python

def addlinks(base_file, links):
    # links is a list of links
    
    if not os.path.isfile(base_file):
        # topic doesn't exist , create a topic file with empty dictionary
        with open(base_file, mode='w', encoding='utf-8') as f:
            obj = {}
            json.dump(obj,f)

    with open(base_file, mode='r', encoding='utf-8') as f: 
        entries = json.load(f) # load existing dictionary
    
    # add links to the dictionary
    if subtopic not in entries:
        entries[subtopic] = links
    else:
        entries[subtopic] += links
        
    # save the modified dictionary
    with open(base_file, mode='w', encoding='utf-8') as f:
        json.dump(entries,f,indent=2)
```

And that's it! All that's left is to add code that reads arguments from the command line and fires up the required functions accordingly.

I name the file `linkup` and add the shebang `#!/usr/bin/python3` on top. This way I can simply call the script as `$ ./linkup` from the command line instead of saving it as a `.py` file and running it as `$ python linkup.py`

Next I copy the file to my `~/bin/` directory (you can put it any directory that's part of your system's `$PATH` variable), add the code for handling command line arguments, some more helper functions, and made sure that the json files are all stored in a hidden folder to reduce clutter and we're good to go.

To generate the above JSON file, all I need to do is type:
`$ linkup -a beatles bio http://sgtpepper.thebeatles.com/ https://en.wikipedia.org/wiki/The_Beatles`

and then 
`$ linkup -a beatles songs https://www.youtube.com/user/TheBeatlesVEVO`

Now to load them all at once in a webbrowser,
`$ linkup -la beatles`
  (The code detects the argument `-la` and fires the `openall()` function we defined above).
  
And the plus? You can just send anyone the json files, it's in human readable format and neatly categorised!

The entire code can be found [here](https://github.com/kaushiksk/linkup).

If you think this tool can be useful do give it a try, if you can think of more features do send a pull request!

On a parting note, this is a really groovy [Beatles song](https://www.youtube.com/watch?v=NCtzkaL2t_Y).



