---
layout: post
title: Python Tricks
image: https://user-images.githubusercontent.com/18272237/56463610-b4779400-63a5-11e9-9492-2f15f7a35b76.jpg
---
List of some useful tips and tricks for python3.

* `dir`
* `emoji`
* `pprint`
* `sh`


### dir
I use this method a lot for looking at an objects attributes. It is extremely useful when I'm using python interactively, it helps me discover the new modules.

```python
dir()
dir("Hello anonymous!")
dir(list)
```

### emoji
Super nice, I recently discovered this module.
The entire set of Emoji codes as defined by the (unicode consortium)[http://www.unicode.org/emoji/charts/full-emoji-list.html] is supported in addition to a bunch of aliases. By default only the official list is enabled but doing emoji.emojize(use_aliases=True) enables both the full list and (aliases)[https://www.webfx.com/tools/emoji-cheat-sheet/].
```python
# required pip installs: emoji
from emoji import emojize
print(emojize(':thumbs_up:'))
print(emojize(':thumbsup:', use_aliases=True))
```

### pprint
This module prints out objects to a human readable form.

Example to prettify a json list of unicode emoji codes for the emoji module mentioned above. 👆
```python
import emoji, pprint
pprint.pprint(emoji.unicode_codes.EMOJI_UNICODE)
```

### sh
Recently stumbled upon this one from the reference mentioned below.
Awesome alternative to os and subprocess libraries, the methods are much like the command line interface.
```python
import sh
sh.pwd()
sh.mkdir('anonymous_folder')
sh.touch('anonymous.dat')
sh.whoami()
sh.echo('Looks anonymous!')
```

***
[Reference](https://medium.freecodecamp.org/an-a-z-of-useful-python-tricks-b467524ee747)
